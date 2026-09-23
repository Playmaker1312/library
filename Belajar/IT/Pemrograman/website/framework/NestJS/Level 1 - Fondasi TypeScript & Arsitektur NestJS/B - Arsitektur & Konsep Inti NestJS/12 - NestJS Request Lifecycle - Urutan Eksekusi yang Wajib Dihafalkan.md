# 12 - NestJS Request Lifecycle - Urutan Eksekusi yang Wajib Dihafalkan

## Penjelasan

Setelah memahami Module, DI, dan Custom Provider, sekarang kita bahas **bagaimana semua komponen bekerja bersama saat sebuah request masuk**. Inilah yang disebut **Request Lifecycle** — urutan eksekusi dari request masuk hingga response keluar.

Ini penting karena: jika kamu tidak paham urutan ini, kamu akan bingung kenapa Guard dijalankan sebelum Pipe, kenapa Interceptor bisa membungkus response, atau kenapa Exception Filter bisa menangkap error dari manapun.

Ini seperti **jalur evakuasi gedung**: dari pintu masuk sampai keluar darurat, ada urutan yang harus dilewati. Setiap pos (Guard, Pipe, Interceptor) punya tugas spesifik di urutan tertentu.

## Fungsi

- Memahami **kapan** setiap komponen dieksekusi
- Menentukan di **lapisan mana** logic tertentu ditempatkan (logging di Interceptor, validasi di Pipe, otorisasi di Guard)
- **Debugging** lebih mudah — tahu persis di urutan mana error terjadi
- **Optimasi** — hindari operasi berat di komponen yang dieksekusi lebih awal tanpa perlu

## Cara Pengimplementasian / Code

### Diagram Visual Request Lifecycle NestJS

```
REQUEST MASUK (HTTP Request)
         │
         ▼
   ╔═════════════════════════╗
   ║     MIDDLEWARE (1)      ║  — body parser, cors, helmet, logger
   ╚═════════════════════════╝
         │
         ▼
   ╔═════════════════════════╗
   ║        GUARD (2)        ║  — autentikasi, otorisasi, role check
   ╚═════════════════════════╝
         │ (jika Guard pass)
         ▼
   ╔═════════════════════════╗
   ║  INTERCEPTOR (before) (3)║  — logging request, transform input, caching
   ╚═════════════════════════╝
         │
         ▼
   ╔═════════════════════════╗
   ║        PIPE (4)         ║  — validasi (class-validator), transform (parseInt)
   ╚═════════════════════════╝
         │
         ▼
   ╔═════════════════════════╗
   ║   CONTROLLER / HANDLER (5)║  — route handler, panggil service
   ╚═════════════════════════╝
         │
         ▼
   ╔═════════════════════════╗
   ║   INTERCEPTOR (after) (6)║  — transform response, logging response time
   ╚═════════════════════════╝
         │
         ▼
   ╔═════════════════════════╗
   ║   EXCEPTION FILTER (7)  ║  — jika ada error di komponen manapun
   ╚═════════════════════════╝
         │
         ▼
RESPONSE KELUAR (HTTP Response)
```

### Code: Middleware

```typescript
// logger.middleware.ts
import { Injectable, NestMiddleware } from '@nestjs/common';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: any, res: any, next: () => void) {
    console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
    next();  // Penting: panggil next() untuk lanjut
  }
}

// app.module.ts — apply middleware
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes('*');  // Semua route
  }
}
```

### Code: Guard

```typescript
// roles.guard.ts
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';

@Injectable()
export class RolesGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const request = context.switchToHttp().getRequest();
    // Guard return false = request ditolak (403 Forbidden)
    return request.headers['x-api-key'] === 'valid-key';
  }
}
```

### Code: Interceptor

```typescript
// logging.interceptor.ts
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable, tap } from 'rxjs';

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const now = Date.now();
    console.log('Interceptor BEFORE — request masuk');

    return next
      .handle()  // Eksekusi handler
      .pipe(
        tap(() => {
          console.log(`Interceptor AFTER — response keluar (${Date.now() - now}ms)`);
        }),
      );
  }
}
```

### Code: Pipe

```typescript
// parse-int.pipe.ts
import { PipeTransform, Injectable, BadRequestException } from '@nestjs/common';

@Injectable()
export class ParseIntPipe implements PipeTransform<string, number> {
  transform(value: string): number {
    const val = parseInt(value, 10);
    if (isNaN(val)) {
      throw new BadRequestException(`Validation failed: "${value}" is not a number`);
    }
    return val;
  }
}
```

### Code: Exception Filter

```typescript
// http-exception.filter.ts
import { ExceptionFilter, Catch, ArgumentsHost, HttpException } from '@nestjs/common';

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();
    const status = exception.getStatus();

    response.status(status).json({
      statusCode: status,
      timestamp: new Date().toISOString(),
      message: exception.message,
    });
  }
}
```

### Semua Komponen Bersama dalam Satu Controller

```typescript
@Controller('users')
@UseGuards(RolesGuard)          // Guard di level controller
@UseInterceptors(LoggingInterceptor)  // Interceptor di level controller
export class UsersController {
  @Get(':id')
  @UsePipes(new ParseIntPipe())  // Pipe di level handler
  findOne(@Param('id') id: number) {
    // id sudah berupa number (Pipe sudah transform)
    return { id, name: 'John Doe' };
  }
}
```

## Analogi (Gedung Bertingkat)

Request lifecycle adalah **proses masuknya tamu ke gedung perkantoran**:

| Langkah | Komponen | Analogi |
|---------|----------|---------|
| **1** | **Middleware** | **Pintu utama gedung** — semua orang lewat sini. Petugas pintu mencatat siapa masuk (logging), memeriksa tas secara umum (cors, helmet). |
| **2** | **Guard** | **Resepsionis + Satpam** — "Ada KTP? Mau ke lantai berapa?" Kalau tidak punya KTP (token invalid), langsung ditolak. |
| **3** | **Interceptor (before)** | **CCTV & Sensor suhu** — merekam tamu yang masuk, mencatat waktu kedatangan. |
| **4** | **Pipe** | **Mesin X-Ray & Validator dokumen** — memeriksa bawaan tamu, memastikan format dokumen sesuai. |
| **5** | **Controller/Handler** | **Staff di lantai tujuan** — "Selamat datang! Ada yang bisa dibantu?" Melayani tamu. |
| **6** | **Interceptor (after)** | **CCTV lagi** — mencatat waktu tamu keluar, menghitung berapa lama tamu di dalam. |
| **7** | **Exception Filter** | **Tim pemadam kebakaran & P3K** — kalau ada masalah (error), mereka yang turun tangan membereskan. |

Urutan ini **WAJIB DIHAPALKAN** karena menentukan di mana logic tertentu harus ditempatkan.

## Dipakai Untuk Apa

- **Memahami di mana menempatkan logic**: logging → Middleware/Interceptor, validasi → Pipe, otorisasi → Guard, error handling → Exception Filter
- **Troubleshooting**: tahu bahwa error di Pipe terjadi SEBELUM handler dijalankan
- **Performa**: Interceptor bisa membungkus handler, mengukur waktu eksekusi
- **Keamanan**: Guard memblokir request sebelum mencapai handler — tidak perlu validasi di handler untuk request yang tidak sah

## Kesalahan Umum

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| Menaruh logic otorisasi di Controller, bukan Guard | Kode redundan, keamanan bocor | Pindahkan ke Guard |
| Melakukan validasi di handler, bukan Pipe | Kode Controller jadi gemuk | Gunakan class-validator + Pipe |
| Lupa return boolean di Guard | Guard selalu return undefined (false) — request selalu ditolak | Pastikan Guard return true/false |
| Tidak panggil `next()` di Middleware | Request menggantung (hang) | Selalu panggil `next()` |
| Lupa `.pipe()` di Interceptor | Response tidak terkirim | Pastikan return `next.handle().pipe(...)` |

## Soal Latihan & Jawaban

### Soal 1
Gambarkan dari memory diagram request lifecycle NestJS secara lengkap, urutkan dari request masuk hingga response keluar.

**Jawaban:**

```
REQUEST
  │
  ▼
[1] MIDDLEWARE
    - body parser, cors, helmet, logger
    - Panggil next() untuk lanjut
  │
  ▼
[2] GUARD
    - canActivate() → true/false
    - false = block request (403)
  │ (if true)
  ▼
[3] INTERCEPTOR (before)
    - Sebelum next.handle()
    - Logging, transform input
  │
  ▼
[4] PIPE
    - transform() → validasi & transform
    - throw error jika invalid
  │
  ▼
[5] CONTROLLER / HANDLER
    - Terima parameter yang sudah divalidasi
    - Panggil Service → business logic
  │
  ▼
[6] INTERCEPTOR (after)
    - Setelah next.handle() return Observable
    - .pipe(tap(...)) — transform response
  │
  ▼
[7] EXCEPTION FILTER
    - Menangkap error dari step 1-6
    - Format error response
  │
  ▼
RESPONSE
```

### Soal 2
Di urutan ke berapa Pipe dieksekusi? Sebutkan komponen sebelum dan sesudahnya.

**Jawaban:**
Pipe dieksekusi di urutan ke-4. Sebelumnya: Middleware (1) → Guard (2) → Interceptor before (3). Sesudahnya: Controller/Handler (5).

### Soal 3
Apa yang terjadi jika Guard me-return false?

**Jawaban:**
NestJS akan menghentikan eksekusi dan melempar `ForbiddenException` (403). Request tidak akan diteruskan ke Interceptor, Pipe, atau Controller. Ini adalah mekanisme keamanan penting — request yang tidak sah dihentikan sedini mungkin.

### Soal 4
Di komponen mana sebaiknya kita menghitung durasi request (response time)?

**Jawaban:**
Di **Interceptor**. Interceptor membungkus handler dengan `next.handle()`, jadi kita bisa hitung waktu sebelum `next.handle()` (start) dan setelahnya (end) menggunakan RxJS `.pipe(tap())`.

```typescript
@Injectable()
export class ResponseTimeInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const start = Date.now();
    return next.handle().pipe(
      tap(() => console.log(`Response time: ${Date.now() - start}ms`)),
    );
  }
}
```
