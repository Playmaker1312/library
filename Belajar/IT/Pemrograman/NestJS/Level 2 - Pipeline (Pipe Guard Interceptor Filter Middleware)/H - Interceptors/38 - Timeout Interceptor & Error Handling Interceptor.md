# 38 - Timeout Interceptor & Error Handling Interceptor

## Penjelasan

Di materi sebelumnya (37 — Logging Interceptor), kita belajar mencatat **durasi eksekusi**. Sekarang kita naik level: **mengontrol durasi eksekusi** (timeout) dan **menangani error** secara terpusat di interceptor.

### Timeout Interceptor

Terkadang handler route membutuhkan waktu terlalu lama — database lambat, external API timeout, atau infinite loop. Timeout Interceptor memutus request yang melebihi batas waktu, mencegah resource server habis.

### Error Handling Interceptor

Sementara Exception Filter menangani **HTTP exception** yang dilempar, interceptor bisa menangani **RxJS error** yang terjadi di stream. Tapi kapan pakai interceptor vs exception filter?

- **Exception Filter** → untuk error yang sudah diketahui (HTTP exceptions, validation errors)
- **Interceptor (catchError)** → untuk error dari stream RxJS, timeout, transformasi error

## Fungsi

- **Timeout**: Memutus request yang melebihi batas waktu (misal 5 detik)
- **Error transformation**: Mengubah format error sebelum dikirim ke exception filter
- **Fallback response**: Mengembalikan response default jika terjadi error
- **Retry logic**: Mencoba ulang request yang gagal (RxJS `retry`)
- **Circuit breaker** (advanced): Mematikan endpoint yang terus error

## Cara Implementasi

### 1. TimeoutInterceptor — Dasar

```typescript
// src/common/interceptors/timeout.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
  RequestTimeoutException,
} from '@nestjs/common';
import { Observable, TimeoutError } from 'rxjs';
import { timeout, catchError } from 'rxjs/operators';
import { throwError } from 'rxjs';

@Injectable()
export class TimeoutInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next.handle().pipe(
      timeout(5000), // 5 detik maksimal
      catchError((err) => {
        if (err instanceof TimeoutError) {
          return throwError(() => new RequestTimeoutException('Request terlalu lama'));
        }
        return throwError(() => err);
      }),
    );
  }
}
```

### 2. TimeoutInterceptor — Configurable via Metadata

```typescript
// src/common/decorators/timeout.decorator.ts
import { SetMetadata } from '@nestjs/common';

export const TIMEOUT_KEY = 'timeout';
export const Timeout = (ms: number) => SetMetadata(TIMEOUT_KEY, ms);
```

```typescript
// src/common/interceptors/timeout.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
  RequestTimeoutException,
} from '@nestjs/common';
import { Observable, TimeoutError } from 'rxjs';
import { timeout, catchError } from 'rxjs/operators';
import { throwError } from 'rxjs';
import { Reflector } from '@nestjs/core';
import { TIMEOUT_KEY } from '../decorators/timeout.decorator';

@Injectable()
export class TimeoutInterceptor implements NestInterceptor {
  constructor(private reflector: Reflector) {}

  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const defaultTimeout = 5000; // default 5 detik

    const timeoutValue = this.reflector.getAllAndOverride<number>(TIMEOUT_KEY, [
      context.getHandler(),
      context.getClass(),
    ]) ?? defaultTimeout;

    return next.handle().pipe(
      timeout(timeoutValue),
      catchError((err) => {
        if (err instanceof TimeoutError) {
          return throwError(() => new RequestTimeoutException(
            `Request timeout setelah ${timeoutValue}ms`,
          ));
        }
        return throwError(() => err);
      }),
    );
  }
}
```

Penggunaan di controller:

```typescript
@Controller('cats')
@UseInterceptors(TimeoutInterceptor)
export class CatsController {
  @Get()
  findAll() {
    return this.catsService.findAll(); // default 5 detik
  }

  @Get('slow-report')
  @Timeout(30000) // endpoint ini boleh 30 detik
  async generateReport() {
    return this.catsService.generateHeavyReport();
  }
}
```

### 3. Error Handling Interceptor — Transformasi Error

```typescript
// src/common/interceptors/error-handler.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
  InternalServerErrorException,
} from '@nestjs/common';
import { Observable } from 'rxjs';
import { catchError } from 'rxjs/operators';
import { throwError } from 'rxjs';

@Injectable()
export class ErrorHandlerInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next.handle().pipe(
      catchError((error) => {
        // Log error ke file monitoring
        console.error('[ErrorHandlerInterceptor]', error.message);

        // Transformasi error database
        if (error.code === 'ER_DUP_ENTRY') {
          return throwError(() => ({
            statusCode: 409,
            message: 'Data sudah ada',
            error: 'Conflict',
          }));
        }

        if (error.name === 'CastError') {
          return throwError(() => ({
            statusCode: 400,
            message: 'Format ID tidak valid',
            error: 'Bad Request',
          }));
        }

        // Jika error sudah HttpException, biarkan exception filter yang handle
        return throwError(() => error);
      }),
    );
  }
}
```

### 4. Fallback Response Interceptor

```typescript
// src/common/interceptors/fallback.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
} from '@nestjs/common';
import { Observable, of } from 'rxjs';
import { catchError } from 'rxjs/operators';

@Injectable()
export class FallbackInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next.handle().pipe(
      catchError((error) => {
        // Jika error, berikan response default (tidak throw)
        return of({
          statusCode: 503,
          message: 'Service temporer tidak tersedia',
          data: null,
        });
      }),
    );
  }
}
```

### 5. Kombinasi Timeout + Error Handler

```typescript
@Controller('cats')
@UseInterceptors(TimeoutInterceptor, ErrorHandlerInterceptor)
export class CatsController {
  // Kedua interceptor akan berjalan:
  // 1. TimeoutInterceptor — cek batas waktu
  // 2. ErrorHandlerInterceptor — tangkap error
  // Urutan penting! TimeoutInterceptor di luar, ErrorHandler di dalam
}
```

## Kapan Interceptor vs Exception Filter

| Skenario | Interceptor | Exception Filter |
|----------|-------------|------------------|
| Timeout request | ✅ `timeout()` | ❌ |
| Transformasi error sebelum filter | ✅ `catchError()` | ❌ |
| Format error response | ❌ (biarkan filter) | ✅ |
| Log error secara detail | ✅ bisa setara | ✅ bisa juga |
| Filter by error type | ✅ `instanceof` | ✅ `@Catch()` |
| Global vs scoped | ✅ per-route | ✅ global/per-controller |

**Aturan praktis**: Gunakan **Interceptor** untuk **error dari stream** (timeout, retry, fallback). Gunakan **Exception Filter** untuk **formatting error response** ke client.

## Analogi — Gedung Bertingkat

### Timeout Interceptor
Bayangkan **Timeout Interceptor** adalah **Petugas Keamanan dengan Jam Alarm**:

- Setiap tamu masuk ruangan, petugas **memasang timer 5 menit**
- Jika tamu **keluar sebelum 5 menit** → timer dicabut, tidak masalah
- Jika **5 menit belum keluar** → alarm berbunyi, petugas **mengeluarkan paksa** tamu tersebut
- **`@Timeout(30000)`** = Petugas pakai timer **30 menit** untuk ruangan khusus (rapat direksi)

### Error Handling Interceptor
Bayangkan **Error Handler Interceptor** adalah **Petugas Tanggap Darurat**:

- Jika di dalam ruangan terjadi **masalah listrik** → petugas tanggap darurat **tenangkan situasi** dan beri genset (fallback)
- Jika terjadi **kebakaran kecil** → petugas **padamkan sendiri** sebelum memanggil pemadam (transform error)
- Jika terjadi **kebakaran besar** → petugas **panggil pemadam kebakaran** (throw ke exception filter)

## Dipakai untuk Apa

- **Timeout**: Mencegah request menggantung selamanya karena database/API lambat
- **Error handling**: Menangani error spesifik seperti duplicate entry, invalid ID format
- **Fallback**: Memberikan response graceful saat service error
- **Retry**: Mencoba ulang operasi yang gagal (misal external API)
- **Circuit breaker**: Mematikan endpoint yang terus gagal untuk melindungi sistem

## Kesalahan Umum

1. **Timeout tidak handle `TimeoutError`**: Tanpa `catchError`, timeout akan throw `TimeoutError` yang tidak tertangani dan menyebabkan unhandled rejection.
2. **Timeout di set terlalu rendah**: 1000ms (1 detik) mungkin terlalu agresif untuk database query. 5000-10000ms lebih realistis.
3. **Error handler malah throw error baru yang tidak tertangani**: Pastikan `catchError` return `throwError(...)` atau `of(...)` jangan throw biasa.
4. **Salah urutan interceptor**: Timeout harus di luar (didaftarkan pertama), error handler di dalam — karena stream mengalir dari luar ke dalam.
5. **Fallback interceptor menyembunyikan error**: Hati-hati — fallback yang menangkap semua error bisa menyembunyikan bug. Gunakan selektif.

## Soal Latihan

**Soal**:
1. Buat `TimeoutInterceptor` yang bisa dikonfigurasi via `@Timeout()` decorator
2. Default timeout 5000ms
3. Jika timeout, lempar `RequestTimeoutException` dengan pesan "Request timeout setelah Xms"
4. Buat endpoint `GET /cats/slow` dengan `@Timeout(10000)` yang sengaja delay 8 detik

<details>
<summary>Jawaban</summary>

```typescript
// src/common/decorators/timeout.decorator.ts
import { SetMetadata } from '@nestjs/common';

export const TIMEOUT_KEY = 'timeout';
export const Timeout = (ms: number) => SetMetadata(TIMEOUT_KEY, ms);
```

```typescript
// src/common/interceptors/timeout.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
  RequestTimeoutException,
} from '@nestjs/common';
import { Observable, TimeoutError } from 'rxjs';
import { timeout, catchError } from 'rxjs/operators';
import { throwError } from 'rxjs';
import { Reflector } from '@nestjs/core';
import { TIMEOUT_KEY } from '../decorators/timeout.decorator';

@Injectable()
export class TimeoutInterceptor implements NestInterceptor {
  constructor(private reflector: Reflector) {}

  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const timeoutMs = this.reflector.getAllAndOverride<number>(TIMEOUT_KEY, [
      context.getHandler(),
      context.getClass(),
    ]) ?? 5000;

    return next.handle().pipe(
      timeout(timeoutMs),
      catchError((err) => {
        if (err instanceof TimeoutError) {
          return throwError(() => new RequestTimeoutException(
            `Request timeout setelah ${timeoutMs}ms`,
          ));
        }
        return throwError(() => err);
      }),
    );
  }
}
```

```typescript
// src/cats/cats.controller.ts
import { Controller, Get, UseInterceptors } from '@nestjs/common';
import { TimeoutInterceptor } from '../common/interceptors/timeout.interceptor';
import { Timeout } from '../common/decorators/timeout.decorator';

@Controller('cats')
@UseInterceptors(TimeoutInterceptor)
export class CatsController {
  @Get()
  findAll() {
    return ['kucing1', 'kucing2'];
  }

  @Get('slow')
  @Timeout(10000)
  async findSlow() {
    // Simulasi operasi lambat — 8 detik
    await new Promise((resolve) => setTimeout(resolve, 8000));
    return 'akhirnya selesai!';
  }
}
```
</details>
