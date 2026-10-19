# Middleware — Perbedaan dengan Guard & Interceptor & Kapan Digunakan

## Penjelasan

Middleware adalah fungsi yang dijalankan **sebelum** route handler. Dalam NestJS, middleware pada dasarnya adalah Express middleware — ia beroperasi pada level HTTP request/response, **bukan** pada level aplikasi NestJS. Karena itu, middleware **tidak punya akses ke Dependency Injection container** secara langsung (tidak bisa inject service kecuali pakai class middleware yang didaftarkan sebagai provider).

Middleware berjalan sebelum Guard, sebelum Interceptor, dan sebelum Pipe. Urutan eksekusi dari luar ke dalam:

```
Client Request
  → Middleware (Express-level)
    → Guard (auth/roles)
      → Interceptor (sebelum handler)
        → Pipe (validasi/transformasi)
          → Route Handler
        → Pipe (setelah handler)
      → Interceptor (setelah handler)
    → Guard
  → Middleware
  → Client Response
```

## Fungsi

- Memodifikasi request/response object
- Menghentikan request sebelum mencapai handler
- Menambahkan header, logging, redirect
- Express-style: `(req, res, next) => void`

## Kapan Menggunakan Apa?

| Komponen | Level | Akses DI | Untuk |
|----------|-------|----------|-------|
| **Middleware** | HTTP (Express) | Tidak langsung | CORS, helmet, compression, logging, request ID, rate limiting |
| **Guard** | Nest (Controller) | Ya (via DI) | Autentikasi, otorisasi (roles/permissions) |
| **Interceptor** | Nest (Handler) | Ya (via DI) | Transform response, logging bisnis, caching, serialisasi |
| **Pipe** | Nest (Handler) | Ya (via DI) | Validasi input, transformasi tipe data |
| **Filter** | Nest (Exception) | Ya (via DI) | Tangkap & format exception |

**Aturan praktis:**
- Jika berurusan dengan **raw HTTP** (header, status code mentah, redirect) → Middleware
- Jika berurusan dengan **auth/roles** → Guard
- Jika berurusan dengan **transform response/logika bisnis** → Interceptor
- Jika berurusan dengan **validasi data input** → Pipe

## Implementasi

### Functional Middleware

```typescript
// common/middleware/x-request-id.middleware.ts
import { Request, Response, NextFunction } from 'express';
import { v4 as uuidv4 } from 'uuid';

export function xRequestIdMiddleware(
  req: Request,
  res: Response,
  next: NextFunction,
) {
  const requestId = uuidv4();
  req.headers['x-request-id'] = requestId;
  res.setHeader('X-Request-Id', requestId);
  next();
}
```

### Class Middleware

```typescript
// common/middleware/logger.middleware.ts
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
    next();
  }
}
```

### Mendaftarkan Middleware

```typescript
// app.module.ts
import { Module, MiddlewareConsumer, NestModule, RequestMethod } from '@nestjs/common';

@Module({ imports: [], controllers: [], providers: [] })
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(xRequestIdMiddleware)
      .forRoutes('*')                   // semua route

    consumer
      .apply(LoggerMiddleware)
      .exclude(
        { path: 'health', method: RequestMethod.GET },
        { path: 'metrics', method: RequestMethod.GET },
      )
      .forRoutes({ path: '*', method: RequestMethod.ALL });
  }
}
```

## Analogi — Gedung Bertingkat

Bayangkan sebuah **gedung perkantoran**:

- **Middleware** adalah **satpam di pintu masuk**. Ia memeriksa barang bawaan (CORS), memasang kartu identitas (request ID), mencatat pengunjung (logging). Satpam ini tidak tahu apa-apa tentang departemen yang dituju — ia hanya bekerja di gerbang.
- **Guard** adalah **resepsionis lift**. Ia mengecek badge akses — "Kamu punya akses ke lantai 5?" Jika tidak, langsung ditolak.
- **Interceptor** adalah **asisten personal** di dalam kantor. Ia bisa membungkus ulang dokumen (transform response), mencatat waktu kedatangan (logging bisnis), atau memberikan dokumen tambahan (cache).
- **Pipe** adalah **penerjemah dokumen**. Ia memastikan format dokumen sesuai standar sebelum diserahkan ke manajer (handler).
- **Filter** adalah **tim darurat** yang menangani jika terjadi kecelakaan atau error di dalam kantor.

Satpam (middleware) bekerja di perimeter paling luar — tidak punya akses ke dalam gedung, hanya mengatur arus di pintu masuk.

## Dipakai Untuk

- CORS headers
- Compression (gzip)
- Helmet (security headers)
- Rate limiting
- Request ID tracking
- Logging HTTP access
- Body parsing
- Cookie parsing
- Redirect / rewrite URL
- Serve static files

## Kesalahan Umum

1. **Menggunakan middleware untuk logic bisnis** — Middleware bukan tempat untuk auth complex atau validasi data. Itu tugas Guard dan Pipe.
2. **Mengharapkan DI langsung di functional middleware** — Functional middleware tidak bisa inject service. Gunakan class middleware dengan `@Injectable()` jika perlu service.
3. **Lupa `next()`** — Request akan menggantung (hang) jika `next()` tidak dipanggil.
4. **Mengubah response setelah dikirim** — Setelah `res.send()` dipanggil, middleware selanjutnya tidak akan berfungsi.
5. **Mendaftarkan middleware di provider** — Middleware bukan provider. Daftarkan via `configure()` di module.

## Soal Latihan

### Soal 1: X-Request-ID Middleware

Buat sebuah **functional middleware** yang menambahkan header `X-Request-Id` ke setiap response menggunakan UUID. Pastikan ID juga tersedia di `req.headers['x-request-id']` untuk digunakan oleh handler. Daftarkan middleware di AppModule untuk semua route.

<details>
<summary>Jawaban</summary>

```typescript
// common/middleware/x-request-id.middleware.ts
import { Request, Response, NextFunction } from 'express';
import { v4 as uuidv4 } from 'uuid';

export function xRequestIdMiddleware(
  req: Request,
  res: Response,
  next: NextFunction,
) {
  const requestId = req.headers['x-request-id'] as string || uuidv4();
  req.headers['x-request-id'] = requestId;
  res.setHeader('X-Request-Id', requestId);
  next();
}
```

```typescript
// app.module.ts
import { Module, MiddlewareConsumer, NestModule } from '@nestjs/common';

@Module({ imports: [], controllers: [], providers: [] })
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(xRequestIdMiddleware)
      .forRoutes('*');
  }
}
```
</details>

### Soal 2: Functional vs Class Middleware

Jelaskan kapan Anda memilih **functional middleware** dibanding **class middleware**. Berikan contoh situasi untuk masing-masing.

<details>
<summary>Jawaban</summary>

**Functional middleware** dipilih ketika:
- Tidak perlu dependency injection (tidak perlu service)
- Logika sederhana (set header, log, redirect)
- Performa sedikit lebih baik (tanpa instansiasi class)

Contoh: menambahkan header keamanan, request ID, redirect HTTP ke HTTPS.

**Class middleware** dipilih ketika:
- Membutuhkan dependency injection (service, repository)
- Perlu state atau konfigurasi complex
- Perlu testing dengan mock dependencies

Contoh: logger yang menyimpan log ke database, rate limiter yang membaca konfigurasi dari service, middleware yang perlu mengakses cache.
</details>

### Soal 3: Urutan Eksekusi

Seorang developer mendaftarkan middleware di AppModule, Guard di controller, dan Interceptor di controller. Jika client mengirim request, apa urutan eksekusi yang benar?

<details>
<summary>Jawaban</summary>

```
1. Middleware (paling luar — Express level)
2. Guard (sebelum handler)
3. Interceptor (sebelum handler — kode sebelum `next()`)
4. Route Handler
5. Interceptor (setelah handler — kode setelah `next()`)
6. Guard (setelah handler — jika ada logging di after)
7. Middleware (setelah handler — jika ada logging di `res.on('finish')`)
```

Jadi middleware berjalan **pertama kali masuk** dan **terakhir kali keluar** (kecuali async hooks). Guard mengecek akses sebelum handler, Interceptor membungkus eksekusi handler.
</details>
