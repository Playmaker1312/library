# 35 - Interceptor Fundamental - NestInterceptor & RxJS Observable

## Penjelasan

Di Level 2 sejauh ini kita sudah mempelajari **Guard** — yang **memutuskan siapa yang boleh masuk** ke handler. Sekarang kita masuk ke komponen pipeline berikutnya: **Interceptor**.

Jika Guard adalah **satpam di pintu masuk**, maka Interceptor adalah **asisten pribadi** yang bisa:
- **Mencatat** apa yang terjadi sebelum dan sesudah handler dijalankan
- **Mengubah** response sebelum dikirim ke client
- **Menangani** error secara terpusat
- **Memperkaya** request dengan data tambahan

Interceptor memanfaatkan **RxJS Observable** karena handler route return Observable. Ini memberi kita kekuatan penuh RxJS: `map`, `tap`, `catchError`, `timeout`, dll.

## Fungsi

- **Logging** — mencatat request dan response
- **Transformasi response** — membungkus response dalam format seragam
- **Caching** — menyimpan response untuk request yang sama
- **Timeout** — memutus request yang terlalu lama
- **Error handling** — menangkap error dan mengubah formatnya
- **Audit trail** — mencatat siapa melakukan apa

## Cara Implementasi

### 1. Anatomi Interceptor

```typescript
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';
import { Request } from 'express';

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const request = context.switchToHttp().getRequest<Request>();

    console.log(`[Before] ${request.method} ${request.url}`);

    const now = Date.now();

    return next
      .handle() // menjalankan handler route
      .pipe(
        tap(() => {
          const duration = Date.now() - now;
          console.log(`[After] ${request.method} ${request.url} — ${duration}ms`);
        }),
      );
  }
}
```

### 2. CallHandler — `next.handle()`

`next.handle()` adalah **entry point ke handler route**. Hasilnya adalah `Observable` — stream yang akan mengeluarkan response dari controller.

```typescript
// Interceptor bisa memanipulasi stream ini:
return next.handle().pipe(
  map((data) => ({ originalData: data, processed: true })),
);
```

### 3. `@UseInterceptors` — Controller & Handler Level

```typescript
import { Controller, Get, UseInterceptors } from '@nestjs/common';
import { LoggingInterceptor } from './common/interceptors/logging.interceptor';

@Controller('cats')
@UseInterceptors(LoggingInterceptor) // semua endpoint
export class CatsController {
  @Get()
  findAll() {
    return ['kucing1', 'kucing2'];
  }

  @Get(':id')
  @UseInterceptors(LoggingInterceptor) // per-endpoint
  findOne(@Param('id') id: string) {
    return `kucing ${id}`;
  }
}
```

### 4. Interceptor Global

```typescript
// src/main.ts
app.useGlobalInterceptors(new LoggingInterceptor());
```

Atau via provider:

```typescript
import { Module } from '@nestjs/common';
import { APP_INTERCEPTOR } from '@nestjs/core';
import { LoggingInterceptor } from './common/interceptors/logging.interceptor';

@Module({
  providers: [
    {
      provide: APP_INTERCEPTOR,
      useClass: LoggingInterceptor,
    },
  ],
})
export class AppModule {}
```

### 5. RxJS Operators yang Sering Dipakai

```typescript
import { map, tap, catchError, timeout } from 'rxjs/operators';
import { of, throwError, TimeoutError } from 'rxjs';

// tap — efek samping (logging, audit)
return next.handle().pipe(
  tap((response) => console.log('Response:', response)),
);

// map — transformasi response
return next.handle().pipe(
  map((data) => ({ success: true, data })),
);

// catchError — tangkap error
return next.handle().pipe(
  catchError((err) => {
    console.error('Error caught:', err.message);
    return throwError(() => err); // lempar lagi
  }),
);

// timeout — batasi waktu eksekusi
return next.handle().pipe(
  timeout(5000),
  catchError((err) => {
    if (err instanceof TimeoutError) {
      return throwError(() => new Error('Request timeout'));
    }
    return throwError(() => err);
  }),
);
```

## Analogi — Gedung Bertingkat

Bayangkan **Interceptor** seperti **Asisten Eksekutif** di setiap lantai gedung:

- **Sebelum handler** (`intercept` dipanggil) = Asisten **menerima tamu** — catat nama, jam kedatangan, keperluan
- **`next.handle()`** = Asisten **mengantar tamu** masuk ke ruang meeting (handler route)
- **Setelah handler** (RxJS pipe) = Asisten **mengantar tamu keluar** — catat jam selesai, ambil feedback
- **`map`** = Asisten **merapikan hasil meeting** — ubah format catatan biar rapi
- **`tap`** = Asisten **mencatat** di logbook
- **`catchError`** = Asisten **meng-handle** jika meeting error — ambil alih dan selesaikan
- **`timeout`** = Asisten **batasi waktu meeting** — maksimal 5 menit

Berbeda dengan Guard yang **memutuskan boleh/tidak boleh masuk**, Interceptor **membungkus dan memproses** apa yang terjadi di sekeliling handler — seperti asisten yang mengelola seluruh pengalaman tamu.

## Dipakai untuk Apa

- **Logging request/response** — audit trail
- **Transformasi response** — wrapper format seragam
- **Manajemen waktu** — timeout handler yang lambat
- **Caching** — simpan dan serve response dari cache
- **Serialization** — exclude field sensitif dari response
- **Error formatting** — ubah format error sebelum dikirim

## Kesalahan Umum

1. **Lupa return Observable**: Interceptor harus return `next.handle().pipe(...)`. Kalo lupa, response tidak pernah dikirim.
2. **Pipe setelah return**: `return next.handle().pipe(...)` — pipe harus di-chain sebelum return.
3. **Side effect di `map` yang seharusnya di `tap`**: `map` digunakan untuk **mengubah data**, `tap` untuk **side effect** (logging). Jangan mengubah data di `tap`.
4. **Error di interceptor tidak ditangani**: Error dari handler akan membypass `map` dan `tap` — gunakan `catchError` jika ingin handle error di interceptor.
5. **Asumsi handler selalu sukses**: Handler bisa throw exception — pastikan interceptor punya `catchError`.

## Soal Latihan

**Soal**: Buat LoggingInterceptor sederhana yang:
1. Mencetak `[REQUEST] method url` sebelum handler
2. Mencetak `[RESPONSE] method url — duration ms` setelah handler selesai
3. Terapkan di `CatsController`

<details>
<summary>Jawaban</summary>

```typescript
// src/common/interceptors/logging.interceptor.ts
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';
import { Request } from 'express';

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const request = context.switchToHttp().getRequest<Request>();
    const { method, url } = request;

    console.log(`[REQUEST] ${method} ${url}`);

    const startTime = Date.now();

    return next.handle().pipe(
      tap(() => {
        const duration = Date.now() - startTime;
        console.log(`[RESPONSE] ${method} ${url} — ${duration}ms`);
      }),
    );
  }
}
```

```typescript
// src/cats/cats.controller.ts
import { Controller, Get, UseInterceptors } from '@nestjs/common';
import { LoggingInterceptor } from '../common/interceptors/logging.interceptor';

@Controller('cats')
@UseInterceptors(LoggingInterceptor)
export class CatsController {
  @Get()
  findAll() {
    return ['kucing1', 'kucing2'];
  }
}
```
</details>
