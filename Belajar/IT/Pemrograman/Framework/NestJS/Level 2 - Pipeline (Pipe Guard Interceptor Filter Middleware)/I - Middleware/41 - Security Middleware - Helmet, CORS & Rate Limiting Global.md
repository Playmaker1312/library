# Security Middleware — Helmet, CORS & Rate Limiting Global

## Penjelasan

Setelah memahami middleware dasar (request ID, logging), sekarang kita akan mengimplementasikan **security middleware** yang melindungi aplikasi dari serangan HTTP umum. NestJS menyediakan integrasi langsung dengan:

- **Helmet** — middleware Express untuk security headers
- **CORS** — Cross-Origin Resource Sharing (bawaan Express/NestJS)
- **@nestjs/throttler** — rate limiting dengan dukungan DI dan Redis

Ketiganya bekerja di **level middleware** — sebelum Guard/Interceptor/Handler — sebagai lapisan keamanan pertama.

## Fungsi

| Middleware | Fungsi |
|------------|--------|
| **Helmet** | Menambahkan ~15 security headers (CSP, X-Frame-Options, X-Content-Type-Options, dll) untuk mencegah serangan XSS, clickjacking, MIME sniffing |
| **CORS** | Mengontrol domain mana yang bisa mengakses API (origin, methods, headers, credentials) |
| **Rate Limiting** | Membatasi jumlah request dari satu IP dalam periode waktu tertentu untuk mencegah brute force / DDoS |

## Implementasi

### Setup Helmet

```typescript
// main.ts
import { NestFactory } from '@nestjs/core';
import helmet from 'helmet';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Helmet — semua security headers aktif
  app.use(helmet());

  // Atau kustomisasi:
  app.use(
    helmet({
      contentSecurityPolicy: {
        directives: {
          defaultSrc: ["'self'"],
          scriptSrc: ["'self'", "'trusted-cdn.com'"],
        },
      },
      frameguard: { action: 'deny' },
      hidePoweredBy: true,
      hsts: { maxAge: 31536000, includeSubDomains: true },
    }),
  );

  await app.listen(3000);
}
```

### Setup CORS

**Cara 1 — Di main.ts (global):**

```typescript
// main.ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  app.enableCors({
    origin: ['https://frontend-saya.com', 'https://admin-saya.com'],
    methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH'],
    allowedHeaders: ['Content-Type', 'Authorization', 'X-Request-Id'],
    exposedHeaders: ['X-Request-Id'],
    credentials: true,
    maxAge: 3600,
  });

  await app.listen(3000);
}
```

**Cara 2 — Lewat NestFactory:**

```typescript
const app = await NestFactory.create(AppModule, {
  cors: {
    origin: ['https://frontend-saya.com'],
    methods: ['GET', 'POST'],
  },
});
```

### Setup Rate Limiting (@nestjs/throttler)

Install:
```bash
npm install @nestjs/throttler
```

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { ThrottlerModule, ThrottlerGuard } from '@nestjs/throttler';
import { APP_GUARD } from '@nestjs/core';

@Module({
  imports: [
    ThrottlerModule.forRoot({
      throttlers: [
        {
          ttl: 60000,           // 60 detik (dalam ms)
          limit: 10,            // maksimal 10 request per ttl
        },
      ],
    }),
  ],
  providers: [
    {
      provide: APP_GUARD,       // global guard
      useClass: ThrottlerGuard,
    },
  ],
})
export class AppModule {}
```

### Rate Limiting dengan Redis Storage

```typescript
// app.module.ts
import { ThrottlerModule, ThrottlerStorageRedis } from '@nestjs/throttler';
import Redis from 'ioredis';

@Module({
  imports: [
    ThrottlerModule.forRoot({
      throttlers: [
        {
          ttl: 60000,
          limit: 100,
        },
      ],
      storage: new ThrottlerStorageRedis(
        new Redis({ host: 'localhost', port: 6379 }),
      ),
    }),
  ],
  providers: [
    { provide: APP_GUARD, useClass: ThrottlerGuard },
  ],
})
export class AppModule {}
```

### Rate Limit Per Controller / Route

```typescript
import { SkipThrottle, Throttle } from '@nestjs/throttler';

@Controller('users')
export class UsersController {
  @SkipThrottle()                    // tidak di-rate-limit
  @Get('public')
  findAll() {}

  @Throttle({ default: { limit: 3, ttl: 60000 } })  // limit lebih ketat
  @Post()
  create() {}
}
```

### Gabungan Semua Security Middleware

```typescript
// main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import helmet from 'helmet';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // 1. Helmet — security headers
  app.use(helmet());

  // 2. CORS — izinkan domain tertentu
  app.enableCors({
    origin: process.env.CORS_ORIGIN?.split(',') || '*',
    methods: ['GET', 'POST', 'PUT', 'DELETE'],
    credentials: true,
  });

  // 3. Trust proxy (jika di belakang reverse proxy)
  app.getHttpAdapter().getInstance().set('trust proxy', 1);

  await app.listen(3000);
}
```

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { ThrottlerModule, ThrottlerGuard } from '@nestjs/throttler';
import { APP_GUARD } from '@nestjs/core';

@Module({
  imports: [
    ThrottlerModule.forRoot({
      throttlers: [
        {
          ttl: 60000,
          limit: 30,
        },
      ],
    }),
  ],
  providers: [
    { provide: APP_GUARD, useClass: ThrottlerGuard },
  ],
})
export class AppModule {}
```

## Analogi — Gedung Bertingkat

Bayangkan gedung perkantoran dengan tiga lapis keamanan di pintu masuk:

- **Helmet** adalah **papan peringatan dan pagar luar**: "Dilarang memotret", "Area ini diawasi CCTV", "Gunakan seragam keselamatan". Ini memberikan sinyal dan aturan yang mencegah penyusup mencoba hal berbahaya sejak awal.
- **CORS** adalah **daftar tamu yang diizinkan masuk**: hanya orang dengan undangan dari perusahaan tertentu (origin) yang bisa masuk. Tamu dari perusahaan lain langsung ditolak di pintu.
- **Rate Limiting** adalah **sistem antrean dan batas pengunjung**: resepsionis hanya melayani 10 orang per jam dari organisasi yang sama. Jika ada yang mencoba masuk berkali-kali dalam waktu singkat (brute force), mereka akan diblokir sementara.

Ketiganya bekerja di **pintu masuk gedung** — sebelum pengunjung bertemu resepsionis (Guard) atau masuk ke ruangan (Handler).

## Dipakai Untuk

- **Helmet**: Semua aplikasi web production — mencegah XSS, clickjacking, MIME sniffing
- **CORS**: API yang dikonsumsi oleh frontend di domain berbeda, mobile apps, atau third-party
- **Rate Limiting**: Endpoint login, register, API publik, endpoint dengan operasi mahal
- **Redis Storage**: Aplikasi multi-instance/load-balanced untuk rate limiting yang akurat

## Kesalahan Umum

1. **CORS terlalu permisif** — `origin: '*'` dengan `credentials: true` tidak valid. Jika perlu credentials, tentukan origin eksplisit.
2. **Lupa `trust proxy`** — Jika di belakang reverse proxy (Nginx, AWS ALB), IP client adalah IP proxy, bukan IP asli. Rate limiting jadi tidak akurat. Set `app.getHttpAdapter().getInstance().set('trust proxy', 1)`.
3. **Rate limit terlalu agresif** — Limit terlalu rendah bikin user frustrasi. Monitor dan adjust berdasarkan traffic aktual.
4. **Hanya mengandalkan rate limiting tanpa caching** — Rate limiting bukan solusi DDoS. Kombinasikan dengan CDN dan WAF.
5. **Mengubah helmet config tanpa testing** — Content-Security-Policy yang terlalu ketat bisa memblokir resource legitimate (font, gambar, script dari CDN).
6. **Helmet dipasang setelah CORS** — Urutan tidak masalah signifikan, tapi best practice: Helmet di paling luar.

## Soal Latihan

### Soal 1: Setup CORS untuk Domain Tertentu

Buat konfigurasi CORS di NestJS yang mengizinkan request dari `https://admin.example.com` dan `https://app.example.com`, mengizinkan method `GET, POST, PUT, DELETE`, dan mengirimkan cookie (credentials: true). Gunakan environment variable untuk domain.

<details>
<summary>Jawaban</summary>

```typescript
// main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  const allowedOrigins = process.env.ALLOWED_ORIGINS
    ? process.env.ALLOWED_ORIGINS.split(',')
    : ['https://admin.example.com', 'https://app.example.com'];

  app.enableCors({
    origin: (origin, callback) => {
      // Allow requests with no origin (mobile apps, curl, etc)
      if (!origin) return callback(null, true);
      if (allowedOrigins.includes(origin)) {
        callback(null, true);
      } else {
        callback(new Error(`Origin ${origin} not allowed by CORS`));
      }
    },
    methods: ['GET', 'POST', 'PUT', 'DELETE'],
    credentials: true,
  });

  await app.listen(3000);
}
```

```env
# .env
ALLOWED_ORIGINS=https://admin.example.com,https://app.example.com
```
</details>

### Soal 2: Custom Throttler Guard

Buat custom ThrottlerGuard yang memberikan respons berbeda ketika rate limit tercapai — kembalikan status `429 Too Many Requests` dengan body JSON `{ "message": "Terlalu banyak permintaan, coba lagi nanti", "retryAfter": <detik> }` daripada default NestJS.

<details>
<summary>Jawaban</summary>

```typescript
// common/guards/custom-throttler.guard.ts
import { Injectable } from '@nestjs/common';
import { ThrottlerGuard } from '@nestjs/throttler';
import { HttpException, HttpStatus } from '@nestjs/common';

@Injectable()
export class CustomThrottlerGuard extends ThrottlerGuard {
  protected throwThrottlingException(): Promise<void> {
    throw new HttpException(
      {
        statusCode: HttpStatus.TOO_MANY_REQUESTS,
        message: 'Terlalu banyak permintaan, coba lagi nanti',
        retryAfter: Math.ceil(this.ttl / 1000),
      },
      HttpStatus.TOO_MANY_REQUESTS,
    );
  }
}
```

```typescript
// app.module.ts
import { APP_GUARD } from '@nestjs/core';

providers: [
  {
    provide: APP_GUARD,
    useClass: CustomThrottlerGuard,
  },
],
```
</details>

### Soal 3: Rate Limit Berdasarkan Role

Implementasikan rate limiting yang berbeda untuk user biasa (10 request/menit) dan admin (100 request/menit). Gunakan decorator `@Throttle()` dan custom guard.

<details>
<summary>Jawaban</summary>

```typescript
// common/guards/role-throttler.guard.ts
import { Injectable, ExecutionContext } from '@nestjs/common';
import { ThrottlerGuard } from '@nestjs/throttler';
import { Reflector } from '@nestjs/core';

@Injectable()
export class RoleThrottlerGuard extends ThrottlerGuard {
  constructor(options: any, storageService: any, reflector: Reflector) {
    super(options, storageService, reflector);
  }

  protected async handleRequest(
    context: ExecutionContext,
    limit: number,
    ttl: number,
  ): Promise<boolean> {
    const request = context.switchToHttp().getRequest();
    const user = request.user;

    // Admin mendapatkan limit lebih besar
    if (user?.role === 'admin') {
      limit = 100;   // 100 request per menit untuk admin
      ttl = 60000;
    } else {
      limit = 10;    // 10 request per menit untuk user biasa
      ttl = 60000;
    }

    return super.handleRequest(context, limit, ttl);
  }
}
```

```typescript
// app.module.ts
providers: [
  {
    provide: APP_GUARD,
    useClass: RoleThrottlerGuard,
  },
],
```
</details>
