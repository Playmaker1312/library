# 36 - Response Wrapper Interceptor - Format Response yang Konsisten

## Penjelasan

Di materi sebelumnya (35 — Interceptor Fundamental), kita belajar bahwa Interceptor bisa **memanipulasi response** sebelum dikirim ke client menggunakan RxJS `map`. Sekarang kita gunakan kemampuan itu untuk **membungkus response dalam format yang konsisten**.

Biasanya response dari controller hanya mengembalikan data mentah. Client API sering mengharapkan format seragam seperti:

```json
{
  "statusCode": 200,
  "message": "OK",
  "data": [...],
  "timestamp": "2025-01-01T00:00:00.000Z"
}
```

Dengan Response Wrapper Interceptor, kita tidak perlu manual membungkus response di setiap controller — cukup **satu interceptor global** yang melakukannya untuk semua endpoint.

## Fungsi

- **Format response seragam** — semua endpoint mengembalikan struktur yang sama
- **Memudahkan client parsing** — client tahu persis struktur response
- **Menambahkan metadata** — timestamp, statusCode, message
- **Konsisten antara sukses dan error** — wrapper sukses di interceptor, wrapper error di exception filter
- **Skip wrapping** untuk response tertentu (file stream, redirect, dll.)

## Cara Implementasi

### 1. Response Wrapper Interface

```typescript
// src/common/interfaces/response-wrapper.interface.ts
export interface ResponseWrapper<T = any> {
  statusCode: number;
  message: string;
  data: T;
  timestamp: string;
}
```

### 2. ResponseWrapperInterceptor

```typescript
// src/common/interceptors/response-wrapper.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
} from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';
import { Response } from 'express';

@Injectable()
export class ResponseWrapperInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const response = context.switchToHttp().getResponse<Response>();

    return next.handle().pipe(
      map((data) => {
        // Skip wrapping jika response sudah berbentuk wrapper
        if (data && typeof data === 'object' && data.statusCode && data.data !== undefined) {
          return data;
        }

        // Skip wrapping jika response adalah stream/buffer
        if (data instanceof Buffer || data instanceof Uint8Array) {
          return data;
        }

        return {
          statusCode: response.statusCode,
          message: 'OK',
          data: data ?? null,
          timestamp: new Date().toISOString(),
        };
      }),
    );
  }
}
```

### 3. Skip Wrapping dengan Metadata

Kadang kita ingin response **tidak dibungkus** — misalnya endpoint download file atau redirect:

```typescript
// src/common/decorators/skip-wrap.decorator.ts
import { SetMetadata } from '@nestjs/common';

export const SKIP_WRAP_KEY = 'skipWrap';
export const SkipWrap = () => SetMetadata(SKIP_WRAP_KEY, true);
```

```typescript
// Update interceptor
import { Reflector } from '@nestjs/core';
import { SKIP_WRAP_KEY } from '../decorators/skip-wrap.decorator';

@Injectable()
export class ResponseWrapperInterceptor implements NestInterceptor {
  constructor(private reflector: Reflector) {}

  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const skipWrap = this.reflector.getAllAndOverride<boolean>(SKIP_WRAP_KEY, [
      context.getHandler(),
      context.getClass(),
    ]);

    if (skipWrap) {
      return next.handle();
    }

    // ... wrapping logic
  }
}
```

### 4. Registrasi Global

```typescript
// src/main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { ResponseWrapperInterceptor } from './common/interceptors/response-wrapper.interceptor';
import { Reflector } from '@nestjs/core';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  const reflector = app.get(Reflector);
  app.useGlobalInterceptors(new ResponseWrapperInterceptor(reflector));

  await app.listen(3000);
}
```

### 5. Contoh Response Sebelum vs Sesudah

**Sebelum (tanpa interceptor):**
```json
["kucing1", "kucing2"]
```

**Sesudah (dengan ResponseWrapperInterceptor):**
```json
{
  "statusCode": 200,
  "message": "OK",
  "data": ["kucing1", "kucing2"],
  "timestamp": "2025-01-01T10:30:00.000Z"
}
```

### 6. Pagination Wrapper

Untuk response yang memuat data terlist, kita bisa buat wrapper khusus:

```typescript
@Get()
findAll(
  @Query('page') page = 1,
  @Query('limit') limit = 10,
) {
  const items = this.catsService.findAll(page, limit);
  const total = this.catsService.count();

  return {
    data: items,
    meta: {
      page: +page,
      limit: +limit,
      total,
      totalPages: Math.ceil(total / limit),
    },
  };
}
```

Interceptor akan membungkusnya menjadi:

```json
{
  "statusCode": 200,
  "message": "OK",
  "data": {
    "data": [...],
    "meta": { "page": 1, "limit": 10, "total": 50, "totalPages": 5 }
  },
  "timestamp": "..."
}
```

## Analogi — Gedung Bertingkat

Bayangkan **Response Wrapper Interceptor** adalah **Bagian Dokumentasi & Arsip** di gedung:

- Setiap **departemen** (controller) menghasilkan dokumen dalam format masing-masing
- Sebelum dokumen **dikirim ke client**, Bagian Dokumentasi **membungkusnya dengan kop surat resmi perusahaan**:
  - `statusCode` = Kode dokumen
  - `message` = Status pesan
  - `data` = Isi dokumen asli dari departemen
  - `timestamp` = Tanggal & stempel waktu

- **`@SkipWrap()`** = Dokumen **RAHASIA** — tidak perlu dibungkus kop surat, langsung kirim

- Tanpa interceptor = Setiap departemen **membungkus dokumen sendiri-sendiri** — ada yang pakai kop format A, ada yang format B — tidak konsisten.

Dengan ResponseWrapperInterceptor, kita punya **satu standar seragam** — semua dokumen keluar dengan kop surat yang sama.

## Dipakai untuk Apa

- **REST API publik** — konsistensi response memudahkan frontend
- **Mobile API** — client parsing lebih mudah dengan format seragam
- **Microservices API Gateway** — gateway bisa wrapper response dari berbagai service
- **API versioning** — format response bisa diubah via interceptor tanpa ubah controller
- **Error vs Success consistency** — format success (di interceptor) dan error (di exception filter) jadi mirip

## Kesalahan Umum

1. **Double wrapping**: Jika controller sudah return `{ data: [...], statusCode: 200 }`, interceptor akan bungkus lagi — jadinya nested. Selalu cek apakah data sudah berbentuk wrapper.
2. **Tidak handle null/undefined**: Jika controller return `null` atau `undefined`, map akan error atau mengirim response kosong. Beri default `data ?? null`.
3. **Stream/Buffer response rusak**: File download, PDF, streaming — jangan dibungkus dengan JSON wrapper. Gunakan `@SkipWrap()` atau deteksi tipe response.
4. **Error response ikut terbungkus**: Error seharusnya ditangani oleh **Exception Filter**, bukan interceptor. Interceptor hanya untuk **success response**.
5. **Status code tidak akurat**: `response.statusCode` di interceptor bisa saja 201 (Created) atau 204 (No Content). Pastikan wrapper mencerminkan status code yang benar.

## Soal Latihan

**Soal**: Implementasikan ResponseWrapperInterceptor yang membungkus response dengan `{ data, message, statusCode, timestamp }`. Pastikan interceptor:
1. Lewati wrapping jika response sudah berbentuk `{ statusCode, data }`
2. Lewati wrapping untuk `Buffer` / stream
3. Tambahkan timestamp ISO
4. Daftarkan sebagai global interceptor

<details>
<summary>Jawaban</summary>

```typescript
// src/common/interceptors/response-wrapper.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
} from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';
import { Response } from 'express';

@Injectable()
export class ResponseWrapperInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const response = context.switchToHttp().getResponse<Response>();

    return next.handle().pipe(
      map((data) => {
        if (data instanceof Buffer || data instanceof Uint8Array) {
          return data;
        }

        if (data && typeof data === 'object' && 'statusCode' in data && 'data' in data) {
          return data;
        }

        return {
          statusCode: response.statusCode,
          message: 'OK',
          data: data ?? null,
          timestamp: new Date().toISOString(),
        };
      }),
    );
  }
}
```

```typescript
// src/main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { ResponseWrapperInterceptor } from './common/interceptors/response-wrapper.interceptor';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalInterceptors(new ResponseWrapperInterceptor());
  await app.listen(3000);
}
```

**Contoh output:**
```json
{
  "statusCode": 200,
  "message": "OK",
  "data": ["kucing1", "kucing2"],
  "timestamp": "2025-01-01T10:30:00.000Z"
}
```
</details>
