# 15 - Response Handling: Status Code, Headers, Redirect & Format Standar

## Penjelasan

Di materi sebelumnya kita belajar bahwa Controller adalah **resepsionis** yang menerima tamu. Sekarang kita bahas **bagaimana resepsionis menyampaikan jawaban** — apakah tamu diterima (200), dialihkan ke lantai lain (302), atau ditolak (404). NestJS memberi kita dekorator untuk mengontrol format jawaban ini secara deklaratif.

---

## Fungsi

- Mengontrol HTTP status code response
- Menambahkan custom headers ke response
- Redirect client ke URL lain
- Menggunakan library-specific response (`@Res`) untuk kontrol penuh
- Membentuk response format standar (konsisten di seluruh API)

---

## Cara Pengimplementasian

### 1. Default JSON Response

Secara default, NestJS mengubah return value menjadi JSON:

```typescript
@Controller('coffee')
export class CoffeeController {
  @Get()
  findAll(): { data: string[]; count: number } {
    return {
      data: ['Kopi Luwak', 'Kopi Arabika'],
      count: 2,
    };
  }
}
// Response: {"data":["Kopi Luwak","Kopi Arabika"],"count":2}
```

### 2. @HttpCode — Status Code Kustom

```typescript
import { HttpCode } from '@nestjs/common';

@Controller('coffee')
export class CoffeeController {
  @Post()
  @HttpCode(201) // Created
  create(@Body() dto: CreateCoffeeDto): string {
    return `Coffee ${dto.name} dibuat`;
  }

  @Delete(':id')
  @HttpCode(204) // No Content
  remove(@Param('id') id: string): void {
    // tidak return apa-apa
  }
}
```

### 3. @Header — Custom Headers

```typescript
import { Header } from '@nestjs/common';

@Controller('coffee')
export class CoffeeController {
  @Get()
  @Header('X-Powered-By', 'NestJS')
  @Header('Cache-Control', 'no-store')
  findAll(): string {
    return 'Daftar coffee';
  }

  @Get('pdf')
  @Header('Content-Type', 'application/pdf')
  @Header('Content-Disposition', 'attachment; filename="menu.pdf"')
  getPdf(): string {
    return '...binary data...';
  }
}
```

### 4. @Redirect — Redirect ke URL Lain

```typescript
import { Redirect } from '@nestjs/common';

@Controller('coffee')
export class CoffeeController {
  @Get('docs')
  @Redirect('https://docs.nestjs.com', 301)
  getDocs(): void {
    // redirect permanent
  }

  @Get('promo')
  @Redirect()
  getPromo(@Query('version') version: string) {
    if (version === 'v2') {
      return { url: 'https://example.com/promo-v2', statusCode: 302 };
    }
    return { url: 'https://example.com/promo', statusCode: 302 };
  }
}
```

### 5. @Res — Library-Specific Response (Express/Fastify)

```typescript
import { Res } from '@nestjs/common';
import { Response } from 'express';

@Controller('coffee')
export class CoffeeController {
  @Get('download')
  download(@Res() res: Response): void {
    const file = createReadStream('menu.pdf');
    res.setHeader('Content-Type', 'application/pdf');
    file.pipe(res);
  }
}
```

> **Peringatan**: Pakai `@Res()` mengaktifkan **Library-Specific Mode** — NestJS tidak akan mengelola response secara otomatis. Kamu harus手动调用 `res.json()` atau `res.send()`.

### 6. Response Format Standar

Buat interceptor atau helper untuk format konsisten:

```typescript
// common/interfaces/api-response.interface.ts
export interface ApiResponse<T = any> {
  data: T;
  message: string;
  statusCode: number;
  timestamp: string;
}

// common/helpers/response.helper.ts
import { ApiResponse } from '../interfaces/api-response.interface';

export function success<T>(
  data: T,
  message = 'Success',
  statusCode = 200,
): ApiResponse<T> {
  return {
    data,
    message,
    statusCode,
    timestamp: new Date().toISOString(),
  };
}

export function error(
  message: string,
  statusCode: number,
  data: any = null,
): ApiResponse {
  return {
    data,
    message,
    statusCode,
    timestamp: new Date().toISOString(),
  };
}
```

Implementasi di controller:

```typescript
// coffee.controller.ts
import { Controller, Get, Param, HttpCode } from '@nestjs/common';
import { success, ApiResponse } from '../common/helpers/response.helper';

@Controller('coffee')
export class CoffeeController {
  @Get()
  findAll(): ApiResponse<string[]> {
    const coffees = ['Kopi Luwak', 'Kopi Arabika'];
    return success(coffees, 'Berhasil mengambil data coffee');
  }

  @Get(':id')
  findOne(@Param('id') id: string): ApiResponse<{ id: string }> {
    return success({ id }, `Detail coffee ${id}`);
  }

  @Post()
  @HttpCode(201)
  create(@Body() dto: CreateCoffeeDto): ApiResponse<CreateCoffeeDto> {
    return success(dto, 'Coffee berhasil dibuat', 201);
  }
}
```

Response JSON:

```json
{
  "data": ["Kopi Luwak", "Kopi Arabika"],
  "message": "Berhasil mengambil data coffee",
  "statusCode": 200,
  "timestamp": "2026-06-10T07:00:00.000Z"
}
```

### 7. Global Response Interceptor (Opsional)

```typescript
// common/interceptors/response.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
} from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

export interface WrappedResponse<T> {
  data: T;
  message: string;
  statusCode: number;
  timestamp: string;
}

@Injectable()
export class ResponseInterceptor<T>
  implements NestInterceptor<T, WrappedResponse<T>>
{
  intercept(
    context: ExecutionContext,
    next: CallHandler,
  ): Observable<WrappedResponse<T>> {
    const statusCode = context.switchToHttp().getResponse().statusCode;
    return next.handle().pipe(
      map((data) => ({
        data,
        message: 'Success',
        statusCode,
        timestamp: new Date().toISOString(),
      })),
    );
  }
}
```

---

## Analogi: Gedung Bertingkat

| Response Handling | Analogi Gedung |
|---|---|
| Default JSON | Resepsionis menjawab **dengan formulir standar** |
| `@HttpCode(201)` | Resepsionis menjawab **"Berhasil dibuat!"** (bukan hanya "OK") |
| `@Header()` | Resepsionis menambahkan **stempel khusus** di amplop balasan |
| `@Redirect()` | Resepsionis bilang **"Silakan ke lantai 3, pintu 5"** |
| `@Res()` | Resepsionis **menulis manual** tanpa format gedung |
| Response standar `{data, message, statusCode, timestamp}` | Amplop balasan dengan **format seragam**: isi surat, perihal, kode, tanggal |
| Global Interceptor | **Pabrik amplop** yang otomatis membungkus semua jawaban |

Gedung yang baik memiliki **format surat balasan yang seragam** — memudahkan siapa pun yang membaca surat tersebut.

---

## Dipakai Untuk Apa

- REST API yang konsisten dan mudah diintegrasi client
- API versioning via headers
- Download file (PDF, Excel, CSV)
- Redirect (setelah login, URL shortening)
- Custom status code untuk domain logic (201 Created, 202 Accepted, 204 No Content)

---

## Kesalahan Umum

1. **Campur aduk `@Res()` dan dekorator NestJS** — kalau pakai `@Res()`, dekorator `@HttpCode` dan `@Header` diabaikan.
2. **Lupa return di `@HttpCode(204)`** — method harus return `void` atau tidak mengembalikan apa pun.
3. **Redirect infinite loop** — pastikan URL redirect benar dan tidak mengarah ke dirinya sendiri.
4. **Format response tidak konsisten** — kadang `{data}`, kadang `{data, message}` — client bingung. Gunakan helper/interceptor.
5. **Header typo** — header name salah eja (misal `'Cotent-Type'`).
6. **Mengirim response dua kali** — return value + panggil `res.send()` akan error `Cannot set headers after they are sent`.

---

## Soal Latihan

### Soal 1
Buatlah `OrderController` dengan:
- `POST /orders` — status 201, tambahkan header `X-Order-Source: API`, return format standar
- `GET /orders/:id` — return format standar
- `DELETE /orders/:id` — status 204, no content
- `GET /orders/recent` — redirect ke `/orders?sort=desc&limit=5`

Gunakan helper `success()` dari materi di atas.

<details>
<summary>Jawaban</summary>

```typescript
import {
  Controller, Get, Post, Delete,
  Param, Body, HttpCode, Header, Redirect, Query,
} from '@nestjs/common';
import { success, ApiResponse } from '../common/helpers/response.helper';

@Controller('orders')
export class OrderController {
  @Post()
  @HttpCode(201)
  @Header('X-Order-Source', 'API')
  create(@Body() body: any): ApiResponse {
    return success(body, 'Pesanan dibuat', 201);
  }

  @Get(':id')
  findOne(@Param('id') id: string): ApiResponse<{ id: string }> {
    return success({ id }, `Detail pesanan ${id}`);
  }

  @Delete(':id')
  @HttpCode(204)
  remove(@Param('id') id: string): void {
    // pesanan dihapus, tidak return apa-apa
  }

  @Get('recent')
  @Redirect()
  getRecent() {
    return { url: '/orders?sort=desc&limit=5', statusCode: 302 };
  }
}
```
</details>

### Soal 2
Buatlah response interceptor global yang membungkus semua response dengan format `{ data, message, statusCode, timestamp }`. Tulis kode lengkapnya.

<details>
<summary>Jawaban</summary>

```typescript
// common/interceptors/response.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
} from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

export interface WrappedResponse<T> {
  data: T;
  message: string;
  statusCode: number;
  timestamp: string;
}

@Injectable()
export class ResponseInterceptor<T>
  implements NestInterceptor<T, WrappedResponse<T>>
{
  intercept(
    context: ExecutionContext,
    next: CallHandler,
  ): Observable<WrappedResponse<T>> {
    const response = context.switchToHttp().getResponse();
    const statusCode = response.statusCode;

    return next.handle().pipe(
      map((data) => ({
        data,
        message: 'Success',
        statusCode,
        timestamp: new Date().toISOString(),
      })),
    );
  }
}

// main.ts — register global
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { ResponseInterceptor } from './common/interceptors/response.interceptor';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalInterceptors(new ResponseInterceptor());
  await app.listen(3000);
}
bootstrap();
```
</details>
