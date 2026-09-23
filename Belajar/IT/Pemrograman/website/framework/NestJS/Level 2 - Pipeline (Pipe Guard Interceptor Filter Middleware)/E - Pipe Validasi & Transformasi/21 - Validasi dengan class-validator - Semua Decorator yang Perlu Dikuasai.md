# Validasi dengan class-validator — Semua Decorator yang Perlu Dikuasai

## Penjelasan (Nyambung dari materi sebelumnya)

Di materi sebelumnya kita belajar **DTO Pattern** sebagai cetak biru input. Sekarang kita akan **mengisi cetak biru itu dengan aturan** — aturan berupa dekorator validasi dari `class-validator`.

DTO yang tidak divalidasi sama seperti cetak biru tanpa ukuran: tukang bisa pasang pintu selebar 10 meter. `class-validator` memberi "ukuran standar" pada setiap field.

---

## Fungsi

- Menjamin data yang masuk sesuai tipe dan format yang diharapkan
- Mengurangi logic validasi manual di controller/service
- Memberi pesan error yang seragam dan terstruktur
- Bekerja di runtime (tidak seperti interface TypeScript yang compile-time)
- Terintegrasi langsung dengan `ValidationPipe` bawaan NestJS

---

## Instalasi

```bash
npm install class-validator class-transformer
```

---

## Cara Implementasi & Code

### 1. Decorator String

```typescript
import {
  IsString, IsEmail, IsUrl, IsAlphanumeric,
  MinLength, MaxLength, Matches, Contains,
} from 'class-validator';

export class CreateUserDto {
  @IsString()
  @MinLength(3, { message: 'Nama minimal 3 karakter' })
  @MaxLength(50)
  name: string;

  @IsEmail({}, { message: 'Format email tidak valid' })
  email: string;

  @IsUrl()
  website: string;

  @IsAlphanumeric()
  username: string;

  @Matches(/^[A-Za-z0-9]+$/, { message: 'Hanya huruf dan angka' })
  slug: string;

  @Contains('prefix-')
  sku: string;
}
```

### 2. Decorator Number

```typescript
import { IsNumber, IsInt, IsPositive, Min, Max } from 'class-validator';

export class CreateProductDto {
  @IsNumber()
  @IsPositive()
  price: number;

  @IsInt()
  @Min(0)
  @Max(10000)
  stock: number;

  @IsNumber()
  @Min(0)
  @Max(5)
  rating: number;
}
```

### 3. Decorator Boolean & Date

```typescript
import { IsBoolean, IsDate, IsDateString } from 'class-validator';
import { Type } from 'class-transformer';

export class CreateEventDto {
  @IsBoolean()
  isActive: boolean;

  @IsDate()
  @Type(() => Date)
  startDate: Date;

  @IsDateString()
  publishedAt: string; // Format ISO 8601
}
```

### 4. Decorator Umum

```typescript
import {
  IsNotEmpty, IsOptional, IsEnum, IsUUID,
  IsDefined, Equals, NotEquals,
} from 'class-validator';

enum Role {
  ADMIN = 'admin',
  USER = 'user',
}

export class CreateUserDto {
  @IsNotEmpty()
  @IsUUID()
  id: string;

  @IsEnum(Role, { message: 'Role harus admin atau user' })
  role: Role;

  @IsOptional()
  @IsString()
  nickname?: string;

  @IsDefined()
  @NotEquals('banned')
  status: string;

  @Equals('superadmin')
  accessLevel: string;
}
```

### 5. Decorator Array

```typescript
import { IsArray, ArrayMinSize, ArrayMaxSize, ArrayUnique, IsString } from 'class-validator';

export class CreateOrderDto {
  @IsArray()
  @ArrayMinSize(1)
  @ArrayMaxSize(50)
  @ArrayUnique()
  productIds: string[];
}
```

### 6. Nested Object — @ValidateNested + @Type

```typescript
import { ValidateNested, IsArray, IsString, IsNumber } from 'class-validator';
import { Type } from 'class-transformer';

class OrderItemDto {
  @IsString()
  productId: string;

  @IsNumber()
  quantity: number;
}

export class CreateOrderDto {
  @IsArray()
  @ValidateNested({ each: true })
  @Type(() => OrderItemDto)
  items: OrderItemDto[];

  @IsString()
  customerId: string;
}
```

### 7. Conditional — @ValidateIf

```typescript
import { ValidateIf, IsString, IsEmail } from 'class-validator';

export class LoginDto {
  @ValidateIf((o) => !o.email) // Hanya wajib jika email tidak diisi
  @IsString()
  username?: string;

  @ValidateIf((o) => !o.username) // Hanya wajib jika username tidak diisi
  @IsEmail()
  email?: string;
}
```

### 8. Custom Message

Semua decorator menerima opsi `message` — bisa string literal atau function:

```typescript
export class CreateProductDto {
  @IsString({ message: 'Nama produk harus berupa string' })
  @MinLength(3, { message: (args) => `"${args.property}" minimal ${args.constraints[0]} karakter, sekarang ${args.value.length}` })
  name: string;
}
```

### 9. DTO Lengkap — Semua Jenis Validasi

```typescript
// src/product/dto/create-product.dto.ts
import {
  IsString, IsNumber, IsInt, IsPositive, IsOptional,
  IsBoolean, IsUrl, IsUUID, IsEnum, IsArray,
  ArrayMinSize, ArrayMaxSize, MinLength, MaxLength,
  Min, Max, ValidateNested, IsNotEmpty,
} from 'class-validator';
import { Type } from 'class-transformer';

enum ProductStatus {
  ACTIVE = 'active',
  INACTIVE = 'inactive',
  DRAFT = 'draft',
}

class VariantDto {
  @IsString()
  name: string;

  @IsNumber()
  @IsPositive()
  price: number;

  @IsInt()
  @Min(0)
  stock: number;
}

export class CreateProductDto {
  @IsString()
  @MinLength(3)
  @MaxLength(100)
  @IsNotEmpty()
  name: string;

  @IsString()
  @IsOptional()
  @MaxLength(1000)
  description?: string;

  @IsNumber()
  @IsPositive()
  price: number;

  @IsInt()
  @Min(0)
  stock: number;

  @IsUrl()
  @IsOptional()
  imageUrl?: string;

  @IsUUID()
  categoryId: string;

  @IsEnum(ProductStatus)
  status: ProductStatus;

  @IsBoolean()
  @IsOptional()
  isFeatured?: boolean;

  @IsArray()
  @ValidateNested({ each: true })
  @Type(() => VariantDto)
  @IsOptional()
  variants?: VariantDto[];
}
```

---

## Analogi — Membangun Gedung

| Konsep | Analogi Gedung |
|--------|----------------|
| **@IsString** | "Pintu harus dari bahan kayu, bukan besi" |
| **@IsEmail** | "Format alamat harus Jalan Merdeka No.1, bukan 'asdf'" |
| **@MinLength(3)** | "Nama ruangan minimal 3 huruf, 'WC' boleh, 'KM' jangan" |
| **@MaxLength(100)** | "Judul proyek maksimal 100 karakter" |
| **@IsNumber** | "Luas tanah harus angka, bukan teks" |
| **@IsInt** | "Jumlah lantai harus bilangan bulat, bukan 2.5" |
| **@IsPositive** | "Suhu AC diatur positif, tidak boleh -5°C" |
| **@IsEnum** | "Tipe rumah hanya: Minimalis, Klasik, Modern" |
| **@IsUUID** | "ID proyek harus format unik yang sudah ditentukan" |
| **@ValidateNested** | "Setiap kamar di dalam apartemen juga harus punya cetak biru sendiri" |
| **@ValidateIf** | "Kalau pakai atap kaca, wajib pakai pelapis UV. Kalau tidak, tidak perlu" |
| **@IsOptional** | "Taman belakang sifatnya opsional" |
| **@IsArray** | "Daftar material harus berupa daftar, bukan benda tunggal" |
| **@ArrayMinSize(1)** | "Setidaknya ada 1 material dalam daftar" |

---

## Dipakai Untuk Apa

- **Validasi body request** di semua endpoint POST, PUT, PATCH
- **Validasi query params** untuk pagination/filter
- **Validasi nested object** — misalnya item dalam order
- **Validasi conditional** — field A wajib jika field B diisi
- **Validasi enum** — memastikan nilai hanya dari opsi tertentu

---

## Kesalahan Umum

1. **Lupa install `class-transformer`** — `@Type()` dan `@Transform()` tidak jalan, error runtime
2. **Tidak pakai `@Type(() => Date)` di properti Date** — string tidak otomatis jadi Date
3. **@ValidateNested tanpa @Type** — nested object tidak tervalidasi karena class-validator tidak tahu tipe konkritnya
4. **Pesan error tidak informatif** — pakai `{ message: '...' }` untuk UX yang lebih baik
5. **Validasi array tanpa `{ each: true }`** — decorator `@IsString({ each: true })` untuk array of strings
6. **Over-validasi** — terlalu banyak decorator sampai membingungkan, pakai secukupnya

---

## Soal Latihan & Jawaban

### Soal

Buat `CreateOrderDto` dengan validasi lengkap:

| Field | Tipe | Validasi |
|-------|------|----------|
| orderNumber | string | tidak boleh kosong, hanya alfanumerik |
| customerEmail | string | format email |
| items | array of object | minimal 1 item, maksimal 100 |
| setiap item: productId | string | UUID |
| setiap item: quantity | number | integer, minimal 1, maksimal 999 |
| setiap item: notes | string | optional, maksimal 200 karakter |
| totalAmount | number | positive |
| status | enum | pending / confirmed / shipped / delivered / cancelled |
| shippedAt | Date | optional |
| isGift | boolean | optional |

### Jawaban

```typescript
// src/order/dto/create-order.dto.ts
import {
  IsString, IsNumber, IsInt, IsPositive, IsOptional,
  IsEmail, IsUUID, IsEnum, IsBoolean, IsDate,
  IsArray, IsAlphanumeric, IsNotEmpty,
  ArrayMinSize, ArrayMaxSize, Min, Max,
  MaxLength, ValidateNested,
} from 'class-validator';
import { Type } from 'class-transformer';

enum OrderStatus {
  PENDING = 'pending',
  CONFIRMED = 'confirmed',
  SHIPPED = 'shipped',
  DELIVERED = 'delivered',
  CANCELLED = 'cancelled',
}

class OrderItemDto {
  @IsUUID()
  productId: string;

  @IsInt()
  @Min(1)
  @Max(999)
  quantity: number;

  @IsOptional()
  @IsString()
  @MaxLength(200)
  notes?: string;
}

export class CreateOrderDto {
  @IsString()
  @IsNotEmpty()
  @IsAlphanumeric()
  orderNumber: string;

  @IsEmail()
  customerEmail: string;

  @IsArray()
  @ValidateNested({ each: true })
  @Type(() => OrderItemDto)
  @ArrayMinSize(1)
  @ArrayMaxSize(100)
  items: OrderItemDto[];

  @IsNumber()
  @IsPositive()
  totalAmount: number;

  @IsEnum(OrderStatus)
  status: OrderStatus;

  @IsOptional()
  @IsDate()
  @Type(() => Date)
  shippedAt?: Date;

  @IsOptional()
  @IsBoolean()
  isGift?: boolean;
}
```
