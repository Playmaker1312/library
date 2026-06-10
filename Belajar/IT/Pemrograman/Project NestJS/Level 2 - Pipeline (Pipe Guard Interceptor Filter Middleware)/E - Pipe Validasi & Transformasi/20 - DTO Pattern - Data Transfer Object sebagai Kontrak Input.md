# DTO Pattern — Data Transfer Object sebagai Kontrak Input

## Penjelasan (Nyambung dari materi sebelumnya)

Sebelumnya kita belajar bahwa **Pipe** bertugas mentransformasi dan memvalidasi data yang masuk. Tapi Pipe tidak bekerja sendiri — ia butuh sesuatu yang mendefinisikan *bentuk* data yang valid. Di sinilah **DTO (Data Transfer Object)** berperan.

Di awal belajar NestJS, kita mungkin langsung menggunakan interface TypeScript untuk mendefinisikan body request. Itu berfungsi saat compile-time, tapi hilang saat runtime. Padahal Pipe (dan dekorator validasi) bekerja di **runtime**. Maka DTO sebagai class adalah solusinya.

---

## Fungsi

- Memisahkan input dari domain model — kode terjemah tidak mencampur aduk apa yang masuk dari client dengan apa yang disimpan di database
- Menjadi **kontrak input** yang jelas untuk setiap operasi (Create, Update, Query)
- Memungkinkan validasi runtime via `class-validator` (karena class, bukan interface)
- Memungkinkan transformasi otomatis via `class-transformer`
- Memudahkan dokumentasi dengan Swagger/OpenAPI

---

## Cara Implementasi & Code

### 1. DTO Dasar — CreateProductDto

```typescript
// src/product/dto/create-product.dto.ts
import { IsString, IsNumber, IsPositive, IsOptional, MinLength, MaxLength } from 'class-validator';

export class CreateProductDto {
  @IsString()
  @MinLength(3)
  @MaxLength(100)
  name: string;

  @IsString()
  @IsOptional()
  description?: string;

  @IsNumber()
  @IsPositive()
  price: number;

  @IsNumber()
  @IsPositive()
  stock: number;

  @IsString()
  categoryId: string;
}
```

### 2. PartialType — UpdateProductDto

`PartialType` membuat semua field menjadi optional — cocok untuk update PATCH.

```typescript
// src/product/dto/update-product.dto.ts
import { PartialType } from '@nestjs/mapped-types';
import { CreateProductDto } from './create-product.dto';

export class UpdateProductDto extends PartialType(CreateProductDto) {}
```

### 3. PickType & OmitType

```typescript
// Hanya ambil name dan price saja
import { PickType } from '@nestjs/mapped-types';
export class UpdatePriceDto extends PickType(CreateProductDto, ['price'] as const) {}

// Semua kecuali stock
import { OmitType } from '@nestjs/mapped-types';
export class CreateWithoutStockDto extends OmitType(CreateProductDto, ['stock'] as const) {}
```

### 4. IntersectionType — Gabungan dua DTO

```typescript
import { IntersectionType } from '@nestjs/mapped-types';

class ProductBasicDto {
  name: string;
  price: number;
}

class ProductInventoryDto {
  stock: number;
  warehouse: string;
}

export class CompleteProductDto extends IntersectionType(
  ProductBasicDto,
  ProductInventoryDto,
) {}
```

### 5. QueryProductDto — untuk filter & pagination

```typescript
// src/product/dto/query-product.dto.ts
import { IsOptional, IsString, IsNumber, Min, Max } from 'class-validator';
import { Type } from 'class-transformer';

export class QueryProductDto {
  @IsOptional()
  @IsString()
  search?: string;

  @IsOptional()
  @IsNumber()
  @Type(() => Number)
  @Min(0)
  skip?: number;

  @IsOptional()
  @IsNumber()
  @Type(() => Number)
  @Min(1)
  @Max(100)
  take?: number;

  @IsOptional()
  @IsString()
  categoryId?: string;

  @IsOptional()
  @IsString()
  sortBy?: string;
}
```

### 6. Controller — menggunakan DTO

```typescript
// src/product/product.controller.ts
@Controller('products')
export class ProductController {
  constructor(private readonly productService: ProductService) {}

  @Post()
  create(@Body() dto: CreateProductDto) {
    return this.productService.create(dto);
  }

  @Patch(':id')
  update(@Param('id') id: string, @Body() dto: UpdateProductDto) {
    return this.productService.update(id, dto);
  }

  @Get()
  findAll(@Query() query: QueryProductDto) {
    return this.productService.findAll(query);
  }
}
```

---

## Analogi — Membangun Gedung

| Komponen | Analogi Gedung |
|----------|----------------|
| **DTO** | **Cetak biru (blueprint)** yang detail: ukuran pintu, jumlah jendela, jenis material |
| **CreateProductDto** | Cetak biru untuk **membangun dari nol** — semua field wajib |
| **UpdateProductDto** | Cetak biru untuk **renovasi** — hanya field yang berubah saja |
| **PartialType** | "Kamu boleh renovasi 1 ruangan saja, tidak perlu gambar ulang seluruh gedung" |
| **PickType** | "Saya cuma perlu ukuran dapur saja" — ambil sebagian kecil dari cetak biru |
| **OmitType** | "Gambarkan seluruh rumah kecuali garasi" |
| **IntersectionType** | Gabungkan cetak biru struktur + cetak biru interior jadi satu |
| **QueryProductDto** | Cetak biru untuk **survei** — "cari kamar dengan luas > 20m² di lantai 3" |

---

## Dipakai Untuk Apa

- **Create**: Menerima input lengkap untuk membuat resource baru
- **Update**: Menerima input parsial (PATCH) — semua field opsional
- **Query**: Menerima parameter filter, sorting, pagination via query string
- **Response**: Bisa juga untuk shape response (meski biasanya pakai `@nestjs/swagger`)
- **Microservice**: Sebagai kontrak antar service

---

## Kesalahan Umum

1. **Menggunakan interface bukan class** — validasi runtime tidak jalan karena interface hilang setelah compile
2. **Lupa import `@nestjs/mapped-types`** — kena error "Cannot find module"
3. **Tidak pakai `@Type(() => Number)` untuk query params** — query string selalu string, jadi `@IsNumber()` akan gagal meskipun input angka
4. **Mengubah DTO secara tidak sengaja karena pass by reference** — class biasa masih mutable
5. **DTO terlalu besar / tidak spesifik** — satu DTO untuk semua operasi → melanggar prinsip Separation of Concerns
6. **Lupa `as const` di PickType/OmitType array** — TypeScript akan complain kalau tidak pakai `as const` di tuple

---

## Soal Latihan & Jawaban

### Soal

1. Buat `CreateUserDto` dengan field: `email` (string), `password` (string, min 8), `name` (string, min 3), `age` (number, positive, optional)
2. Buat `UpdateUserDto` menggunakan `PartialType` dari `CreateUserDto`
3. Buat `QueryUserDto` dengan field: `search` (string, optional), `role` (string, optional), `page` (number, min 1, default 1), `limit` (number, min 1, max 100)

### Jawaban

**1. CreateUserDto**

```typescript
// src/user/dto/create-user.dto.ts
import { IsString, IsEmail, IsNumber, IsPositive, IsOptional, MinLength } from 'class-validator';

export class CreateUserDto {
  @IsEmail()
  email: string;

  @IsString()
  @MinLength(8)
  password: string;

  @IsString()
  @MinLength(3)
  name: string;

  @IsOptional()
  @IsNumber()
  @IsPositive()
  age?: number;
}
```

**2. UpdateUserDto**

```typescript
// src/user/dto/update-user.dto.ts
import { PartialType } from '@nestjs/mapped-types';
import { CreateUserDto } from './create-user.dto';

export class UpdateUserDto extends PartialType(CreateUserDto) {}
```

**3. QueryUserDto**

```typescript
// src/user/dto/query-user.dto.ts
import { IsOptional, IsString, IsNumber, Min, Max } from 'class-validator';
import { Type } from 'class-transformer';

export class QueryUserDto {
  @IsOptional()
  @IsString()
  search?: string;

  @IsOptional()
  @IsString()
  role?: string;

  @IsOptional()
  @IsNumber()
  @Type(() => Number)
  @Min(1)
  page?: number = 1;

  @IsOptional()
  @IsNumber()
  @Type(() => Number)
  @Min(1)
  @Max(100)
  limit?: number = 10;
}
```
