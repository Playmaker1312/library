# 37 - Logging Interceptor - Audit Trail Request & Response

## Penjelasan

Di materi sebelumnya (36 — Response Wrapper), kita belajar **mentransformasi response** — mengubah format data. Sekarang kita fokus pada **logging dan audit trail** — mencatat **seluruh aktivitas** request-response untuk keperluan debugging, monitoring, dan audit keamanan.

Logging interceptor yang baik mencatat:
- HTTP method dan URL
- Status code response
- Durasi eksekusi
- User ID (jika autentikasi aktif)
- Request body (hati-hati dengan data sensitif)
- Timestamp

NestJS menyediakan **Logger** bawaan yang bisa digunakan untuk logging terstruktur.

## Fungsi

- **Audit trail** — catatan siapa melakukan apa dan kapan
- **Performance monitoring** — deteksi endpoint lambat via durasi
- **Debugging** — lihat request/response mentah untuk troubleshooting
- **Security audit** — catat akses yang mencurigakan
- **Compliance** — memenuhi standar logging untuk regulasi

## Cara Implementasi

### 1. LoggingInterceptor — Versi Dasar

```typescript
// src/common/interceptors/logging.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
  Logger,
} from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';
import { Request } from 'express';

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  private readonly logger = new Logger('HTTP');

  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const request = context.switchToHttp().getRequest<Request>();
    const { method, url } = request;
    const userAgent = request.headers['user-agent'] || 'unknown';
    const startTime = Date.now();

    return next.handle().pipe(
      tap({
        next: (data) => {
          const response = context.switchToHttp().getResponse();
          const duration = Date.now() - startTime;

          this.logger.log({
            method,
            url,
            statusCode: response.statusCode,
            duration: `${duration}ms`,
            userAgent,
            user: (request as any).user?.id || 'anonymous',
            timestamp: new Date().toISOString(),
          });
        },
        error: (error) => {
          const duration = Date.now() - startTime;

          this.logger.error({
            method,
            url,
            statusCode: error.status || 500,
            duration: `${duration}ms`,
            error: error.message,
            user: (request as any).user?.id || 'anonymous',
            timestamp: new Date().toISOString(),
          });
        },
      }),
    );
  }
}
```

### 2. Logging Body Request (Opsional)

```typescript
import { map } from 'rxjs/operators';

// Untuk log body request (hati-hati dengan password!)
if (method !== 'GET') {
  const body = { ...request.body };
  // Jangan log password
  if (body.password) body.password = '***';
  if (body.token) body.token = '***';
  this.logger.log(`Body: ${JSON.stringify(body)}`);
}
```

### 3. Logging dengan Metadata — Konfigurasi Per-Endpoint

```typescript
// src/common/decorators/log.decorator.ts
import { SetMetadata } from '@nestjs/common';

export const LOG_KEY = 'logConfig';
export interface LogConfig {
  body?: boolean;
  headers?: boolean;
  skip?: boolean;
}

export const Log = (config: LogConfig) => SetMetadata(LOG_KEY, config);
```

```typescript
@Controller('cats')
export class CatsController {
  @Post()
  @Log({ body: true }) // log body untuk endpoint ini
  create() {
    return 'dibuat';
  }

  @Get('health')
  @Log({ skip: true }) // skip logging untuk health check
  health() {
    return { status: 'ok' };
  }
}
```

### 4. Logging ke File atau External Service

```typescript
// Tambahkan transport di Logger
const { createLogger, format, transports } = require('winston');

// Atau gunakan NestJS Logger dengan custom transport
private readonly logger = new Logger('HTTP');

// Di production, bisa kirim ke Elasticsearch, Datadog, dll.
this.logger.log({
  ...logData,
  service: 'nestjs-api',
  environment: process.env.NODE_ENV,
});
```

### 5. Audit Trail Lengkap

```typescript
// src/common/interceptors/audit.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
  Logger,
} from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';
import { Request } from 'express';

@Injectable()
export class AuditInterceptor implements NestInterceptor {
  private readonly audit = new Logger('AUDIT');

  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const request = context.switchToHttp().getRequest<Request>();
    const startTime = Date.now();

    // Siapkan audit entry
    const auditEntry: any = {
      action: `${request.method} ${request.url}`,
      userId: (request as any).user?.id || null,
      ip: request.ip,
      userAgent: request.headers['user-agent'],
      timestamp: new Date().toISOString(),
    };

    return next.handle().pipe(
      tap({
        next: (data) => {
          auditEntry.duration = Date.now() - startTime;
          auditEntry.status = 'SUCCESS';
          auditEntry.statusCode = context.switchToHttp().getResponse().statusCode;
          this.audit.log(auditEntry);
        },
        error: (error) => {
          auditEntry.duration = Date.now() - startTime;
          auditEntry.status = 'ERROR';
          auditEntry.error = error.message;
          auditEntry.statusCode = error.status || 500;
          this.audit.warn(auditEntry);
        },
      }),
    );
  }
}
```

## Analogi — Gedung Bertingkat

Bayangkan **Logging Interceptor** adalah **CCTV & Buku Tamu** di gedung:

- **Method + URL** = Kamera merekam: "Budi masuk ke lantai 3, ruang 301"
- **Status code** = CCTV mencatat: "Budi berhasil masuk" (200) atau "Budi ditolak" (403)
- **Durasi** = Waktu: "Budi di ruang 301 selama 2,5 detik"
- **User ID** = Identitas: "Karyawan ID: 1234 (Budi)"
- **Timestamp** = Stempel waktu: "10:30:15 01-Jan-2025"

Tanpa Logging Interceptor = Gedung tanpa CCTV dan buku tamu — kalau ada masalah, tidak ada jejak.

Dengan Logging Interceptor = **Setiap langkah tercatat** — siapa masuk ruang apa, kapan, berapa lama, berhasil atau gagal. Ini penting untuk **audit keamanan** dan **debugging**.

## Dipakai untuk Apa

- **Debugging production** — lihat request apa yang menyebabkan error
- **Performance monitoring** — deteksi endpoint yang lambat
- **Security audit** — lacak akses mencurigakan
- **Customer support** — lihat riwayat request user
- **Compliance & regulasi** — HIPAA, PCI-DSS, GDPR memerlukan audit log

## Kesalahan Umum

1. **Log data sensitif**: Jangan log `password`, `token`, `secret`, `credit card`. Filter dulu.
2. **Log terlalu verbose**: Log `body` untuk semua endpoint bisa membanjiri storage. Log hanya yang penting.
3. **Tidak handle error logging**: `tap({ next: ..., error: ... })` — error juga perlu dicatat, jangan cuma success.
4. **Async logging blocking request**: Jika kirim log ke database/external service, jangan blocking. Gunakan queue atau fire-and-forget.
5. **Log tidak terstruktur**: Gunakan objek JSON untuk log, bukan string bebas — biar mudah di-parse oleh tools seperti Elasticsearch / Datadog.

## Soal Latihan

**Soal**: Buat LoggingInterceptor yang:
1. Mencatat method, URL, status code, durasi (ms)
2. Mencatat user ID (jika ada) — dari `request.user?.id`
3. Mencatat timestamp
4. Membedakan log success dan error (tap next vs tap error)
5. Gunakan NestJS Logger dengan konteks 'HTTP'

<details>
<summary>Jawaban</summary>

```typescript
// src/common/interceptors/logging.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
  Logger,
} from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';
import { Request } from 'express';

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  private readonly logger = new Logger('HTTP');

  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const request = context.switchToHttp().getRequest<Request>();
    const { method, url } = request;
    const userId = (request as any).user?.id || 'anonymous';
    const startTime = Date.now();

    return next.handle().pipe(
      tap({
        next: () => {
          const response = context.switchToHttp().getResponse();
          this.logger.log({
            method,
            url,
            statusCode: response.statusCode,
            duration: `${Date.now() - startTime}ms`,
            userId,
            timestamp: new Date().toISOString(),
          });
        },
        error: (error) => {
          this.logger.error({
            method,
            url,
            statusCode: error.status || 500,
            duration: `${Date.now() - startTime}ms`,
            error: error.message,
            userId,
            timestamp: new Date().toISOString(),
          });
        },
      }),
    );
  }
}
```
</details>
