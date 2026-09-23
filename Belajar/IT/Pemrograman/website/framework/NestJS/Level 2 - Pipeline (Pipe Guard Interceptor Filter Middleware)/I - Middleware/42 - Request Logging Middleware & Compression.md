# Request Logging Middleware & Compression

## Penjelasan

Setelah mengamankan aplikasi dengan helmet, CORS, dan rate limiting, kita perlu dua middleware tambahan untuk **observability** dan **performa**:

1. **Request Logging Middleware** — mencatat setiap HTTP request yang masuk (method, URL, status code, duration, request ID). Berguna untuk debugging, audit, dan monitoring.
2. **Compression Middleware** — mengompres response body dengan gzip/brotli untuk mengurangi ukuran data yang dikirim ke client.

Keduanya adalah middleware Express standard yang bisa dipasang di NestJS via `app.use()` atau `MiddlewareConsumer`. Request logging sebaiknya memanfaatkan **request ID** (dari middleware sebelumnya) untuk tracing.

## Fungsi

| Middleware | Fungsi |
|------------|--------|
| **Request Logging** | Merekam akses HTTP — mirip log Apache/Nginx `access.log`. Mencatat IP, method, URL, status, durasi, user-agent |
| **Compression** | Memampatkan response body (gzip/deflate) — mengurangi bandwidth hingga 70% untuk response JSON besar |
| **Request ID Propagation** | Menyebarkan request ID ke log, header response, dan service downstream |

## Implementasi

### Custom Request Logging Middleware

```typescript
// common/middleware/request-logger.middleware.ts
import { Injectable, NestMiddleware, Logger } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class RequestLoggerMiddleware implements NestMiddleware {
  private readonly logger = new Logger('HTTP');

  use(req: Request, res: Response, next: NextFunction) {
    const { method, originalUrl, ip } = req;
    const userAgent = req.headers['user-agent'] || '-';
    const requestId = req.headers['x-request-id'] || '-';
    const startTime = Date.now();

    res.on('finish', () => {
      const { statusCode } = res;
      const contentLength = res.get('content-length') || 0;
      const duration = Date.now() - startTime;

      this.logger.log(
        `[${requestId}] ${method} ${originalUrl} ${statusCode} ${duration}ms ${contentLength}b - ${userAgent} ${ip}`,
      );
    });

    next();
  }
}
```

### Mendaftarkan Request Logger dengan MiddlewareConsumer

```typescript
// app.module.ts
import { Module, MiddlewareConsumer, NestModule } from '@nestjs/common';
import { RequestLoggerMiddleware } from './common/middleware/request-logger.middleware';

@Module({ imports: [], controllers: [], providers: [] })
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(RequestLoggerMiddleware)
      .forRoutes('*');
  }
}
```

### Atau menggunakan Morgan (alternatif populer)

```bash
npm install morgan
npm install @types/morgan --save-dev
```

```typescript
// main.ts
import { NestFactory } from '@nestjs/core';
import morgan from 'morgan';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Morgan dengan format combined (Apache-style)
  app.use(morgan('combined'));

  // Atau custom format
  app.use(
    morgan(':method :url :status :response-time ms - :res[content-length]', {
      skip: (req) => req.url === '/health',   // skip health check
    }),
  );

  await app.listen(3000);
}
```

### Compression Middleware

```bash
npm install compression
npm install @types/compression --save-dev
```

```typescript
// main.ts
import { NestFactory } from '@nestjs/core';
import compression from 'compression';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Gzip compression — global
  app.use(compression());

  // Atau dengan opsi:
  app.use(
    compression({
      level: 6,               // level kompresi 0-9 (default 6)
      threshold: 1024,        // hanya kompres response > 1KB
      filter: (req, res) => {
        if (req.headers['x-no-compression']) return false;  // skip jika header ada
        return compression.filter(req, res);                 // default filter
      },
    }),
  );

  await app.listen(3000);
}
```

### Request ID Propagation Lengkap

```typescript
// common/middleware/request-context.middleware.ts
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';
import { v4 as uuidv4 } from 'uuid';

@Injectable()
export class RequestContextMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    // Ambil dari header incoming, atau buat baru
    const requestId = req.headers['x-request-id'] as string || uuidv4();

    // Set di request — bisa diakses oleh handler/service
    req.headers['x-request-id'] = requestId;

    // Set di response header
    res.setHeader('X-Request-Id', requestId);

    // Propagasi ke downstream — log duration saat response selesai
    const start = Date.now();
    const { method, originalUrl } = req;

    res.on('finish', () => {
      const duration = Date.now() - start;
      console.log(
        `[${requestId}] ${method} ${originalUrl} ${res.statusCode} ${duration}ms`,
      );
    });

    next();
  }
}
```

### Gabungan Logging + Compression + Request ID

```typescript
// main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import compression from 'compression';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // 1. Trust proxy (jika di belakang reverse proxy)
  app.getHttpAdapter().getInstance().set('trust proxy', 1);

  // 2. Compression
  app.use(compression({ threshold: 1024 }));

  // 3. Request ID & Logging — via middleware consumer di AppModule
  //    (lebih rapi daripada app.use untuk class middleware)

  await app.listen(3000);
}
```

```typescript
// app.module.ts
import { Module, MiddlewareConsumer, NestModule } from '@nestjs/common';
import { RequestContextMiddleware } from './common/middleware/request-context.middleware';

@Module({ imports: [], controllers: [], providers: [] })
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(RequestContextMiddleware)
      .forRoutes('*');
  }
}
```

## Analogi — Gedung Bertingkat

Bayangkan lagi gedung perkantoran kita:

- **Request Logging Middleware** adalah **buku tamu di pintu masuk**. Setiap pengunjung dicatat: jam datang, ke lantai berapa, bertemu siapa, jam pulang. Catatan ini berguna untuk audit keamanan dan investigasi jika ada masalah.
- **Compression Middleware** adalah **petugas pengepak di pintu keluar**. Sebelum dokumen atau barang diberikan ke pengunjung, petugas mengepaknya serapi mungkin (mengompres) agar lebih ringan dibawa. Semakin besar dokumen, semakin terasa manfaatnya.
- **Request ID** adalah **nomor tiket pengunjung**. Dari masuk sampai keluar, semua aktivitas pengunjung dicatat dengan nomor tiket yang sama. Jika ada masalah di lantai 3, petugas keamanan bisa melacak semua gerak-gerik pengunjung berdasarkan nomor tiket itu.

Ketiganya bekerja di **pintu masuk dan keluar gedung** — logging mencatat saat masuk dan keluar, compression bekerja saat keluar, request ID mewarnai seluruh kunjungan.

## Dipakai Untuk

- **Request Logging**: Debugging production, audit trail, monitoring (ELK/Datadog), analisis traffic
- **Compression**: Response JSON besar (list ribuan item), file statis, API yang digunakan oleh mobile dengan bandwidth terbatas
- **Request ID**: Tracing request di microservices, correlation ID antar service, debugging distributed system
- **Morgan**: Quick setup untuk development, format standar Apache/NGINX

## Kesalahan Umum

1. **Logging di production tanpa sampling** — Jika aplikasi menerima 10.000 request/detik, menulis log setiap request bisa membebani disk dan biaya. Gunakan sampling atau log hanya error.
2. **Compression untuk response kecil** — Response < 1KB malah lebih besar setelah dikompres karena overhead header. Selalu set threshold.
3. **Compression di endpoint yang sudah mentransfer binary** — Gambar, video, PDF biasanya sudah terkompres. Jangan kompres ulang. Filter dengan `content-type`.
4. **Lupa skip health checks di logging** — Health check (setiap 5-10 detik) membanjiri log. Skip dengan filter URL.
5. **Request ID tidak dipropagasi ke service downstream** — Di microservices, request ID harus dikirim via header HTTP ke service lain. Jika tidak, tracing terputus.
6. **Log mengandung data sensitif** — Jangan log body request yang berisi password, token, atau PII. Filter atau mask fields sensitive.

## Soal Latihan

### Soal 1: Implementasi Request Logging dengan Duration

Buat sebuah class middleware `HttpAccessLogger` yang mencatat log berikut untuk setiap request:

```
[2024-01-01T12:00:00.000Z] GET /api/users 200 45.2ms 1.2kB
```

Sertakan timestamp ISO, method, URL, status code, duration (ms), dan content-length. Eksclude endpoint `/health` dan `/metrics` dari logging.

<details>
<summary>Jawaban</summary>

```typescript
// common/middleware/http-access-logger.middleware.ts
import { Injectable, NestMiddleware, Logger } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class HttpAccessLoggerMiddleware implements NestMiddleware {
  private readonly logger = new Logger('HTTP');

  use(req: Request, res: Response, next: NextFunction) {
    const { method, originalUrl } = req;
    const start = Date.now();

    res.on('finish', () => {
      const duration = (Date.now() - start).toFixed(1);
      const contentLength = res.get('content-length') || '0';
      const timestamp = new Date().toISOString();

      this.logger.log(
        `[${timestamp}] ${method} ${originalUrl} ${res.statusCode} ${duration}ms ${this.formatBytes(contentLength)}`,
      );
    });

    next();
  }

  private formatBytes(bytes: string): string {
    const numBytes = parseInt(bytes, 10);
    if (!numBytes) return '0B';
    if (numBytes < 1024) return `${numBytes}B`;
    if (numBytes < 1024 * 1024) return `${(numBytes / 1024).toFixed(1)}KB`;
    return `${(numBytes / (1024 * 1024)).toFixed(1)}MB`;
  }
}
```

```typescript
// app.module.ts
import { Module, MiddlewareConsumer, NestModule, RequestMethod } from '@nestjs/common';
import { HttpAccessLoggerMiddleware } from './common/middleware/http-access-logger.middleware';

@Module({ imports: [], controllers: [], providers: [] })
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(HttpAccessLoggerMiddleware)
      .exclude(
        { path: 'health', method: RequestMethod.GET },
        { path: 'metrics', method: RequestMethod.GET },
      )
      .forRoutes('*');
  }
}
```
</details>

### Soal 2: Compression Threshold

Seorang developer mengaktifkan compression tanpa threshold untuk semua response. Ia melihat response `{ "status": "ok" }` (18 bytes) malah menjadi 38 bytes setelah kompresi. Jelaskan mengapa ini terjadi dan bagaimana solusinya.

<details>
<summary>Jawaban</summary>

**Penyebab:** Kompresi (gzip) menambahkan overhead header (gzip header, dictionary, checksum). Untuk data yang sangat kecil ( < ~1KB ), overhead ini lebih besar dari penghematan ukuran. Response `{ "status": "ok" }` hanya 18 bytes — setelah dikompres menjadi 38 bytes karena header gzip (20 bytes tambahan).

**Solusi:** Set threshold minimum agar response kecil tidak dikompres:

```typescript
app.use(compression({ threshold: 1024 }));   // hanya kompres response >= 1KB
```

Atau threshold lebih tinggi (2KB-4KB) tergantung karakteristik response API.
</details>

### Soal 3: Request ID Propagation di Microservices

Anda memiliki arsitektur microservices: `API Gateway → User Service → Order Service`. Implementasikan propagasi request ID dari gateway ke service downstream menggunakan header `x-request-id`.

<details>
<summary>Jawaban</summary>

**Di API Gateway (NestJS middleware):**

```typescript
// api-gateway/common/middleware/request-id.middleware.ts
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';
import { v4 as uuidv4 } from 'uuid';

@Injectable()
export class RequestIdMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    const requestId = req.headers['x-request-id'] as string || uuidv4();
    req.headers['x-request-id'] = requestId;
    res.setHeader('X-Request-Id', requestId);
    next();
  }
}
```

**Di service downstream (misal User Service):**

```typescript
// user-service/src/common/interceptors/request-id.interceptor.ts
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { v4 as uuidv4 } from 'uuid';

@Injectable()
export class RequestIdInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const request = context.switchToHttp().getRequest();

    // Propagasi ID — jika ada dari upstream, gunakan; jika tidak, buat baru
    if (!request.headers['x-request-id']) {
      request.headers['x-request-id'] = uuidv4();
    }

    return next.handle();
  }
}
```

**Saat HTTP call ke service downstream (axios/fetch):**

```typescript
// common/http/http-client.service.ts
import { Injectable } from '@nestjs/common';
import { HttpService } from '@nestjs/axios';
import { Request } from 'express';

@Injectable()
export class HttpClientService {
  constructor(private readonly httpService: HttpService) {}

  callDownstream(url: string, req: Request) {
    return this.httpService.get(url, {
      headers: {
        'x-request-id': req.headers['x-request-id'] || 'unknown',
      },
    });
  }
}
```
</details>
