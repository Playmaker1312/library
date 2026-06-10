# class-transformer — Transformasi & Serialisasi Data

## Penjelasan (Nyambung dari materi sebelumnya)

`class-validator` memvalidasi data. Tapi data mentah dari client jarang langsung cocok dengan format yang kita butuhkan:
- Query string selalu berupa `string` — padahal kita mau `number`
- Spasi berlebih di input — perlu di-*trim*
- Password / field sensitif — perlu disembunyikan dari response

Di sinilah `class-transformer` bekerja — ia **mentransformasi** data dari bentuk mentah ke bentuk yang siap pakai.

---

## Fungsi

- Mengubah tipe data secara otomatis (`string` → `number`, `string` → `Date`)
- Membersihkan input (misal: trim whitespace, lowercase email)
- Mengekspos / menyembunyikan properti saat serialisasi (response)
- Memberi nilai default pada properti yang tidak diisi
- Melakukan transformasi kompleks (misal: mengubah format tanggal)

---

## Cara Implementasi & Code

### 1. Instalasi

class-transformer sudah terinstall bareng class-validator:

```bash
npm install class-validator class-transformer
```

### 2. @Type — Konversi Tipe Dasar

**Masalah**: Query string `?page=1` — `page` adalah string `"1"`, bukan number `1`.

```typescript
import { Type } from 'class-transformer';
import { IsNumber, Min } from 'class-validator';

export class PaginationDto {
  @Type(() => Number)
  @IsNumber()
  @Min(1)
  page: number;

  @Type(() => Number)
  @IsNumber()
  @Min(1)
  limit: number;
}
```

Tanpa `@Type(() => Number)`, `@IsNumber()` akan gagal karena value masih string.

### 3. @Transform — Transformasi Kustom

```typescript
import { Transform } from 'class-transformer';

export class CreateProductDto {
  // Auto trim string
  @Transform(({ value }) => value?.trim())
  name: string;

  // Konversi harga: "Rp10.000" → 10000
  @Transform(({ value }) => {
    if (typeof value === 'string') {
      return parseInt(value.replace(/[^0-9]/g, ''));
    }
    return value;
  })
  price: number;

  // Lowercase email
  @Transform(({ value }) => value?.toLowerCase().trim())
  email: string;

  // Default value jika tidak ada
  @Transform(({ value }) => value ?? 'default-slug')
  slug: string;

  // Split string "a,b,c" → ["a","b","c"]
  @Transform(({ value }) => {
    if (typeof value === 'string') return value.split(',');
    return value;
  })
  tags: string[];
}
```

### 4. @Expose & @Exclude — Serialisasi Response

**Masalah**: Field `password` ikut terkirim ke response → bocor!

```typescript
import { Exclude, Expose } from 'class-transformer';

export class UserEntity {
  id: string;

  @Expose() // Eksplisit: field ini boleh ditampilkan
  name: string;

  @Exclude() // Sembunyikan password dari response
  password: string;

  @Exclude()
  refreshToken: string;

  // Virtual property — tidak ada di DB tapi muncul di response
  @Expose()
  get fullName(): string {
    return `${this.firstName} ${this.lastName}`;
  }

  constructor(partial: Partial<UserEntity>) {
    Object.assign(this, partial);
  }
}
```

### 5. excludeExtraneousValues — Buang Field Tak Terdefinisi

```typescript
// main.ts — global setting
import { plainToInstance } from 'class-transformer';

app.useGlobalPipes(
  new ValidationPipe({
    transform: true,
    transformOptions: {
      enableImplicitConversion: false,
      excludeExtraneousValues: true, // Hanya field dengan @Expose() yang dipertahankan
    },
  }),
);
```

Atau per method:

```typescript
import { SerializeOptions } from 'class-transformer';

@SerializeOptions({
  excludeExtraneousValues: true,
})
export class UserEntity {
  @Expose()
  id: string;

  @Expose()
  name: string;

  password: string; // Tidak akan muncul
}
```

### 6. @SerializeOptions — Opsi Serialisasi

```typescript
import { SerializeOptions } from 'class-transformer';

@SerializeOptions({
  strategy: 'excludeAll', // Semua field tersembunyi kecuali @Expose()
  // — atau —
  strategy: 'exposeAll', // Semua field tampil kecuali @Exclude()
})
export class ProductEntity {
  @Expose()
  id: string;

  @Expose()
  name: string;

  internalNotes: string; // Tersembunyi karena excludeAll
}
```

### 7. DTO Lengkap dengan Transformasi

```typescript
// src/product/dto/create-product.dto.ts
import {
  IsString, IsNumber, IsPositive, IsOptional, MinLength,
} from 'class-validator';
import { Transform, Type } from 'class-transformer';

export class CreateProductDto {
  @Transform(({ value }) => value?.trim())
  @IsString()
  @MinLength(3)
  name: string;

  @Transform(({ value }) => value?.trim())
  @IsOptional()
  @IsString()
  description?: string;

  @Transform(({ value }) => {
    if (typeof value === 'string') {
      return parseInt(value.replace(/[^0-9]/g, ''));
    }
    return value;
  })
  @IsNumber()
  @IsPositive()
  price: number;

  @Type(() => Number)
  @IsNumber()
  @IsPositive()
  stock: number;

  @Transform(({ value }) => value?.toLowerCase().trim())
  @IsString()
  email: string;

  @Transform(({ value }) => {
    if (typeof value === 'string') return value.split(',').map(s => s.trim());
    return value;
  })
  @IsOptional()
  tags?: string[];
}
```

### 8. Response Entity — Sembunyikan Password

```typescript
// src/user/entities/user.entity.ts
import { Exclude, Expose } from 'class-transformer';

export class UserEntity {
  id: string;
  name: string;

  @Exclude()
  password: string;

  @Exclude()
  pin: string;

  @Expose()
  get maskedPin(): string {
    return this.pin ? '***' : undefined;
  }

  constructor(partial: Partial<UserEntity>) {
    Object.assign(this, partial);
  }
}

// Controller
@Get(':id')
async findOne(@Param('id') id: string) {
  const user = await this.userService.findOne(id);
  return plainToInstance(UserEntity, user, {
    excludeExtraneousValues: true,
  });
}
```

---

## Analogi — Membangun Gedung

| Konsep | Analogi Gedung |
|--------|----------------|
| **@Type** | Arsitek otomatis mengubah "3 meter" (tulisan) → 3 (angka) di cetak biru |
| **@Transform** | Tukang otomatis membersihkan puing sebelum membangun, atau memotong besi sesuai ukuran |
| **@Exclude** | **Pintu rahasia** — ada ruangan yang tidak boleh tampak di denah publik |
| **@Expose** | **Tanda "+"** — "luas termasuk balkon" (virtual, tidak ada di struktur asli) |
| **excludeExtraneousValues** | Hanya gambar yang ada di cetak biru yang boleh dipakai — coretan lain diabaikan |
| **@SerializeOptions** | Aturan: "Jenis A cetak biru memperlihatkan semuanya, Jenis B hanya ruang tamu saja" |
| **plainToInstance** | Menerjemahkan sketsa mentah → cetak biru formal |

---

## Dipakai Untuk Apa

- **Auto-trim** input string (spasi berlebih di nama, alamat)
- **Lowercase email** agar konsisten di database
- **Konversi query string** `?page=1` dari string ke number
- **Sembunyikan field sensitif** (password, token, PIN) dari response
- **Split CSV string** `"a,b,c"` menjadi array `["a","b","c"]`
- **Default value** untuk field opsional
- **Virtual/computed property** yang dihitung dari field lain

---

## Kesalahan Umum

1. **Lupa `@Type(() => Number)` untuk query params** — `@IsNumber()` gagal karena value string
2. **`@Transform` mengubah tipe tapi tidak berurutan dengan `@Type`** — urutan eksekusi dekorator bisa membingungkan
3. **Terlalu banyak transformasi dalam satu field** — membuat kode sulit dibaca, pisahkan ke fungsi helper
4. **`@Exclude()` tidak bekerja di controller** — pastikan response melalui `plainToInstance` atau `ClassSerializerInterceptor`
5. **Mutasi data asli** — `@Transform` mengubah value di place, hati-hati jika object dipakai ulang
6. **Lupa import** — error `@Transform is not defined`
7. **excludeExtraneousValues = true tanpa @Expose** — semua field hilang dari response

---

## Soal Latihan & Jawaban

### Soal

Buat `RegisterUserDto` dengan transformasi berikut:
1. `email` — auto lowercase + trim
2. `name` — auto trim
3. `phone` — hapus karakter non-digit (`"0812-3456-7890"` → `"081234567890"`)
4. `birthDate` — `@Type(() => Date)`
5. `tags` — string `"news,promo"` → array `["news","promo"]`
6. `referralCode` — uppercase

### Jawaban

```typescript
// src/user/dto/register-user.dto.ts
import {
  IsString, IsEmail, IsOptional, IsArray, MinLength, MaxLength, IsDate,
} from 'class-validator';
import { Transform, Type } from 'class-transformer';

export class RegisterUserDto {
  @Transform(({ value }) => value?.toLowerCase().trim())
  @IsEmail()
  email: string;

  @Transform(({ value }) => value?.trim())
  @IsString()
  @MinLength(3)
  @MaxLength(100)
  name: string;

  @Transform(({ value }) => value?.replace(/[^0-9]/g, ''))
  @IsString()
  @MinLength(10)
  @MaxLength(15)
  phone: string;

  @Type(() => Date)
  @IsDate()
  birthDate: Date;

  @Transform(({ value }) => {
    if (typeof value === 'string') {
      return value.split(',').map(s => s.trim());
    }
    return value;
  })
  @IsOptional()
  @IsArray()
  @IsString({ each: true })
  tags?: string[];

  @Transform(({ value }) => value?.toUpperCase().trim())
  @IsOptional()
  @IsString()
  referralCode?: string;
}
```
