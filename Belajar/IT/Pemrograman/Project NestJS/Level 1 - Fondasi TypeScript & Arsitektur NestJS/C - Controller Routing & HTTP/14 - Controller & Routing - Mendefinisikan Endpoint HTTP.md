# 14 - Controller & Routing: Mendefinisikan Endpoint HTTP

## Penjelasan

Di materi sebelumnya kita sudah belajar tentang modul sebagai **lemari arsip** yang mengelompokkan dokumen. Sekarang kita akan membangun **papan nama dan penerima tamu** di setiap lantai gedung — itulah Controller. Controller adalah kelas yang bertugas menerima HTTP request dan mengembalikan HTTP response. Ia menentukan **siapa yang masuk lewat pintu mana** (routing) dan **apa yang harus dilakukan** (handler).

---

## Fungsi

- Menerima request HTTP dari client
- Menentukan route/endpoint (URL + method)
- Mengekstrak data dari request (`@Param`, `@Query`, `@Body`, `@Headers`)
- Mendelegasikan logika ke service
- Mengembalikan response ke client

---

## Cara Pengimplementasian

### 1. Setup Controller Dasar

```typescript
// coffee.controller.ts
import { Controller, Get } from '@nestjs/common';

@Controller('coffee')
export class CoffeeController {
  @Get()
  findAll(): string {
    return 'Daftar semua coffee';
  }
}
```

### 2. Route Parameters (@Param)

```typescript
@Controller('coffee')
export class CoffeeController {
  @Get(':id')
  findOne(@Param('id') id: string): string {
    return `Coffee dengan id ${id}`;
  }

  @Get(':id/reviews')
  getReviews(
    @Param('id') id: string,
    @Param() params: Record<string, string>,
  ): string {
    return `Review coffee ${id}, semua params: ${JSON.stringify(params)}`;
  }
}
```

### 3. Query Parameters (@Query)

```typescript
@Controller('coffee')
export class CoffeeController {
  @Get()
  findAll(@Query('page') page: string, @Query('limit') limit: string): string {
    return `Halaman ${page}, limit ${limit}`;
  }
}
```

### 4. Request Body (@Body)

```typescript
import { IsString, IsNumber } from 'class-validator';

export class CreateCoffeeDto {
  @IsString()
  readonly name: string;

  @IsNumber()
  readonly price: number;
}

@Controller('coffee')
export class CoffeeController {
  @Post()
  create(@Body() createCoffeeDto: CreateCoffeeDto): string {
    return `Membuat coffee: ${createCoffeeDto.name}`;
  }

  @Post('bulk')
  createBulk(@Body() coffees: CreateCoffeeDto[]): string {
    return `Membuat ${coffees.length} coffee`;
  }
}
```

### 5. HTTP Method Lainnya

```typescript
@Controller('coffee')
export class CoffeeController {
  @Post()
  create(@Body() dto: CreateCoffeeDto): string {
    return `Create ${dto.name}`;
  }

  @Put(':id')
  update(@Param('id') id: string, @Body() dto: UpdateCoffeeDto): string {
    return `Update ${id} dengan ${JSON.stringify(dto)}`;
  }

  @Patch(':id')
  partialUpdate(@Param('id') id: string, @Body() dto: Partial<CreateCoffeeDto>): string {
    return `Partial update ${id}`;
  }

  @Delete(':id')
  remove(@Param('id') id: string): string {
    return `Hapus coffee ${id}`;
  }
}
```

### 6. Headers

```typescript
@Controller('coffee')
export class CoffeeController {
  @Get()
  findAll(@Headers('authorization') auth: string): string {
    return `Auth header: ${auth}`;
  }

  @Get('info')
  getAllHeaders(@Headers() headers: Record<string, string>): string {
    return `Semua headers: ${JSON.stringify(headers)}`;
  }
}
```

### 7. Wildcard Route & Prefix Global

```typescript
// app.controller.ts — global prefix di main.ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.setGlobalPrefix('api/v1'); // semua route jadi /api/v1/...
  await app.listen(3000);
}

// Controller dengan wildcard
@Get('ab*cd')
wildcardRoute(): string {
  return 'Cocok dengan abcd, abXcd, abXXXcd, dll';
}
```

### 8. Test dengan REST Client

Buat file `test.http`:

```http
### GET all coffee
GET http://localhost:3000/coffee

### GET coffee by id
GET http://localhost:3000/coffee/1

### GET with query
GET http://localhost:3000/coffee?page=1&limit=10

### POST create coffee
POST http://localhost:3000/coffee
Content-Type: application/json

{
  "name": "Kopi Luwak",
  "price": 50000
}

### PUT update coffee
PUT http://localhost:3000/coffee/1
Content-Type: application/json

{
  "name": "Kopi Baru",
  "price": 60000
}

### PATCH partial update
PATCH http://localhost:3000/coffee/1
Content-Type: application/json

{
  "price": 55000
}

### DELETE coffee
DELETE http://localhost:3000/coffee/1

### GET with headers
GET http://localhost:3000/coffee
Authorization: Bearer my-token
```

---

## Analogi: Gedung Bertingkat

| NestJS | Analogi Gedung |
|---|---|
| Controller | **Resepsionis** di setiap lantai |
| `@Controller('coffee')` | Papan nama **"Lantai Coffee"** |
| `@Get(':id')` | Prosedur **"Tamu datang, tunjukkan id"** |
| `@Param('id')` | Resepsionis melihat **KTP tamu** |
| `@Query('page')` | Resepsionis bertanya **"dari halaman berapa?"** |
| `@Body()` | Resepsionis menerima **amplop berisi formulir** |
| `@Headers()` | Resepsionis membaca **identitas di seragam tamu** |
| `@Post()` | Prosedur **"tambah data baru ke arsip"** |
| `@Put(':id')` | Prosedur **"ganti seluruh dokumen id X"** |
| `@Patch(':id')` | Prosedur **"perbaiki satu baris dokumen id X"** |
| `@Delete(':id')` | Prosedur **"hancurkan dokumen id X"** |

Resepsionis tidak menghafal semua data — dia hanya menerima tamu, mencatat, lalu menyuruh **satpam (Service)** yang menangani sisanya.

---

## Dipakai Untuk Apa

- REST API endpoints (CRUD)
- Webhooks (Post)
- File upload/download
- API Gateway routing
- Microservice entry points

---

## Kesalahan Umum

1. **Lupa register controller di module** — error `Nest can't resolve dependencies`. Pastikan `CoffeeController` ada di `providers` atau `controllers` module.
2. **Duplicate route** — dua `@Get(':id')` di controller yang sama akan error.
3. **Param vs Query tertukar** — `@Param` untuk path variable (`/coffee/1`), `@Query` untuk query string (`?page=1`).
4. **Lupa `@Body()`** — parameter tanpa decorator tidak otomatis membaca body.
5. **Async handler tanpa `async`** — NestJS support Promise/async, tapi kalau return Observable harus di-subscribe.
6. **DTO tanpa validasi** — body langsung `any` tanpa DTO + `ValidationPipe`.

---

## Soal Latihan

### Soal 1
Buat controller `ProductController` dengan prefix `products` yang memiliki method:
- `GET /products` — mengembalikan string "Semua produk"
- `GET /products/:id` — mengembalikan "Produk id: {id}"
- `POST /products` — mengembalikan "Membuat produk: {name}" (name dari body)
- `PUT /products/:id` — mengembalikan "Update produk {id}: {name}"
- `PATCH /products/:id` — mengembalikan "Partial update produk {id}"
- `DELETE /products/:id` — mengembalikan "Hapus produk {id}"

<details>
<summary>Jawaban</summary>

```typescript
import { Controller, Get, Post, Put, Patch, Delete, Param, Body } from '@nestjs/common';

@Controller('products')
export class ProductController {
  @Get()
  findAll(): string {
    return 'Semua produk';
  }

  @Get(':id')
  findOne(@Param('id') id: string): string {
    return `Produk id: ${id}`;
  }

  @Post()
  create(@Body('name') name: string): string {
    return `Membuat produk: ${name}`;
  }

  @Put(':id')
  update(@Param('id') id: string, @Body('name') name: string): string {
    return `Update produk ${id}: ${name}`;
  }

  @Patch(':id')
  partialUpdate(@Param('id') id: string): string {
    return `Partial update produk ${id}`;
  }

  @Delete(':id')
  remove(@Param('id') id: string): string {
    return `Hapus produk ${id}`;
  }
}
```
</details>

### Soal 2
Buat controller yang membaca query parameter `search` dan `page`, serta header `x-api-key`. Jika `x-api-key` tidak ada, kembalikan "Unauthorized".

<details>
<summary>Jawaban</summary>

```typescript
import { Controller, Get, Query, Headers, ForbiddenException } from '@nestjs/common';

@Controller('search')
export class SearchController {
  @Get()
  search(
    @Query('search') search: string,
    @Query('page') page: string,
    @Headers('x-api-key') apiKey: string,
  ): string {
    if (!apiKey) {
      throw new ForbiddenException('x-api-key required');
    }
    return `Mencari "${search}" di halaman ${page || '1'}`;
  }
}
```
</details>
