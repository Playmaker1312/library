# Built-in Pipes & ValidationPipe Global

## Penjelasan (Nyambung dari materi sebelumnya)

Setelah kita belajar membuat DTO dengan validasi (`class-validator`) dan transformasi (`class-transformer`), sekarang kita perlu **mengaktifkannya** di pipeline NestJS.

NestJS menyediakan **built-in pipes** untuk tugas umum (parsing param, validasi) dan **ValidationPipe** yang mengintegrasikan class-validator + class-transformer secara otomatis.

---

## Fungsi

- **ParseIntPipe** — mengubah parameter string ke number, throw `BadRequestException` jika gagal
- **ParseUUIDPipe** — memvalidasi UUID, throw jika format salah
- **ParseBoolPipe** — mengubah `"true"/"1"` ke boolean
- **DefaultValuePipe** — memberi nilai default jika parameter tidak ada
- **ParseArrayPipe** — mem-parsing string query menjadi array
- **ValidationPipe** — otomatis menjalankan class-validator + class-transformer di DTO

---

## Cara Implementasi & Code

### 1. ParseIntPipe — Parameter Route

```typescript
@Get(':id')
findOne(@Param('id', ParseIntPipe) id: number) {
  return this.productService.findOne(id);
}
```

Response error saat `GET /products/abc`:
```json
{
  "statusCode": 400,
  "message": "Validation failed (numeric string is expected)",
  "error": "Bad Request"
}
```

### 2. ParseUUIDPipe — Validasi UUID

```typescript
@Get(':uuid')
findByUuid(@Param('uuid', ParseUUIDPipe) uuid: string) {
  return this.productService.findByUuid(uuid);
}
```

### 3. ParseBoolPipe — Boolean dari Query

```typescript
@Get()
findAll(@Query('isActive', ParseBoolPipe) isActive: boolean) {
  // GET /products?isActive=true → isActive = true
  return this.productService.findAll({ isActive });
}
```

### 4. DefaultValuePipe — Default Value

```typescript
@Get()
findAll(
  @Query('page', DefaultValuePipe, ParseIntPipe) page: number = 1,
  @Query('limit', DefaultValuePipe, ParseIntPipe) limit: number = 10,
) {
  // GET /products → page = 1, limit = 10
  // GET /products?page=3 → page = 3, limit = 10
  return this.productService.findAll({ page, limit });
}
```

**Catatan**: Jika `DefaultValuePipe` digunakan, parameter harus dideklarasikan sebagai opsional atau dengan nilai default.

### 5. ParseArrayPipe — Array dari Query String

```typescript
@Get()
findAll(
  @Query('ids', ParseArrayPipe) ids: string[],
) {
  // GET /products?ids=a,b,c → ids = ["a","b","c"]
  // GET /products?ids=a&ids=b&ids=c → ids = ["a","b","c"]
}
```

Dengan opsi:

```typescript
@Get()
findAll(
  @Query('ids', new ParseArrayPipe({ items: String, separator: ',' })) ids: string[],
) {
  // items: Number → memparse setiap elemen jadi number
  // separator: ',' → koma sebagai pemisah
}
```

### 6. ValidationPipe — Setup Global

```typescript
// src/main.ts
import { NestFactory } from '@nestjs/core';
import { ValidationPipe } from '@nestjs/common';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  app.useGlobalPipes(
    new ValidationPipe({
      // --- Opsi Penting ---
      whitelist: true,           // Hapus properti yang tidak didefinisikan di DTO
      forbidNonWhitelisted: true, // Lempar error jika ada properti asing
      transform: true,           // Otomatis transform tipe (@Type, @Transform)
      transformOptions: {
        enableImplicitConversion: true, // Konversi implisit (string → number tanpa @Type)
      },
      // --- Opsi Tambahan ---
      disableErrorMessages: false, // Tampilkan detail error (false di production)
      stopAtFirstError: false,     // Berhenti di error pertama atau lanjut ke semua field
      exceptionFactory: (errors) => {
        // Kustom format error
        return new BadRequestException({
          statusCode: 400,
          message: 'Validasi gagal',
          errors: errors.map((e) => ({
            field: e.property,
            constraints: e.constraints,
          })),
        });
      },
    }),
  );

  await app.listen(3000);
}
bootstrap();
```

### 7. ValidationPipe Per-Route

Global pipe bisa di-override per route:

```typescript
@Post()
@UsePipes(new ValidationPipe({ whitelist: false })) // Override global
create(@Body() dto: CreateProductDto) {
  // Route ini tetap menerima properti asing
}
```

Atau tanpa pipe sama sekali:

```typescript
@Post()
create(@Body(new ValidationPipe({ validatior: false })) dto: any) {
  // Tidak ada validasi
}
```

### 8. Demo — Whitelist vs ForbidNonWhitelisted

```typescript
// DTO
export class CreateProductDto {
  name: string;
  price: number;
}
```

**whitelist: true, forbidNonWhitelisted: false**

```json
// Request
{ "name": "Laptop", "price": 10000, "extraField": "hack" }
// DTO yang diterima
{ "name": "Laptop", "price": 10000 } // extraField dihapus
```

**whitelist: true, forbidNonWhitelisted: true**

```json
// Request
{ "name": "Laptop", "price": 10000, "extraField": "hack" }
// Response
{
  "statusCode": 400,
  "message": ["property extraField should not exist"],
  "error": "Bad Request"
}
```

---

## Analogi — Membangun Gedung

| Pipe | Analogi Gedung |
|------|----------------|
| **ParseIntPipe** | Petugas keamanan: "Nomor lantai harus angka, 'tiga' tidak valid" |
| **ParseUUIDPipe** | Petugas: "ID proyek harus format ABCD-1234, bukan sembarang tulisan" |
| **ParseBoolPipe** | Saklar: "Hanya 'nyala' atau 'mati' yang valid, bukan 'mungkin'" |
| **DefaultValuePipe** | "Kalau tidak disebutkan, pakai ukuran standar 3×3 meter" |
| **ParseArrayPipe** | Mengubah "kursi,meja,lemari" menjadi daftar terpisah |
| **ValidationPipe** | **Konsultan pengawas** yang: `whitelist` → buang coretan tidak penting di cetak biru, `forbidNonWhitelisted` → tegur kalau ada gambar tidak sesuai kontrak, `transform` → otomatis ubah sketsa jadi angka presisi |
| **whitelist: true** | "Hanya gambar yang ada di kontrak yang boleh dipakai, coretan lain diabaikan" |
| **forbidNonWhitelisted: true** | "Ada coretan asing! Lempar cetak biru ini!" |
| **transform: true** | "Sketsa 'lantai 2' saya tulis 2, Anda ubah jadi angka 2 bukan teks" |

---

## Dipakai Untuk Apa

- **ParseIntPipe** — Parameter `id` di route `GET /products/:id`
- **ParseUUIDPipe** — Parameter yang menggunakan UUID
- **ParseBoolPipe** — Query `?isActive=true` / `?includeDeleted=false`
- **DefaultValuePipe** — Pagination `?page=1&limit=10` (default value)
- **ParseArrayPipe** — Filter multi-value `?category=a,b,c`
- **ValidationPipe** — Semua endpoint yang menerima body/query — **wajib dipasang global**

---

## Kesalahan Umum

1. **Lupa pasang ValidationPipe global** — DTO tidak divalidasi sama sekali, error baru ketahuan di service
2. **whitelist: false (default)** — Client bisa kirim field ekstra yang tidak didefinisikan → berpotensi celah keamanan
3. **forbidNonWhitelisted: true tanpa whitelist: true** — `forbidNonWhitelisted` hanya berfungsi jika `whitelist: true`
4. **enableImplicitConversion: true + ParseIntPipe** — konflik, satu otomatis satu manual. Pilih salah satu
5. **ParseIntPipe di query param tanpa DefaultValuePipe** — error jika query tidak dikirim
6. **Lupa `new` di pipe dengan opsi** — `new ParseArrayPipe({ items: Number })` bukan `ParseArrayPipe({ items: Number })`
7. **exceptionFactory merusak format error standar** — pastikan tetap kompatibel dengan frontend

---

## Soal Latihan & Jawaban

### Soal

Buat endpoint berikut dengan pipe yang sesuai:

1. `GET /users/:id` — ParseIntPipe
2. `GET /users/:uuid` — ParseUUIDPipe
3. `GET /users` — query `?isAdmin=true` (ParseBoolPipe), `?page=1&limit=10` (DefaultValuePipe + ParseIntPipe)
4. `POST /users` — validasi body dengan `CreateUserDto`, setup ValidationPipe global dengan whitelist & forbidNonWhitelisted

### Jawaban

**1 & 2 — Controller**

```typescript
// src/user/user.controller.ts
import { Controller, Get, Post, Param, Query, Body, ParseIntPipe, ParseUUIDPipe } from '@nestjs/common';
import { UserService } from './user.service';
import { CreateUserDto } from './dto/create-user.dto';

@Controller('users')
export class UserController {
  constructor(private readonly userService: UserService) {}

  @Get(':id')
  findOne(@Param('id', ParseIntPipe) id: number) {
    return this.userService.findOne(id);
  }

  @Get(':uuid')
  findByUuid(@Param('uuid', ParseUUIDPipe) uuid: string) {
    return this.userService.findByUuid(uuid);
  }
}
```

**3 — Query dengan DefaultValuePipe & ParseBoolPipe**

```typescript
@Get()
findAll(
  @Query('isAdmin', new DefaultValuePipe(false), ParseBoolPipe) isAdmin: boolean,
  @Query('page', new DefaultValuePipe(1), ParseIntPipe) page: number,
  @Query('limit', new DefaultValuePipe(10), ParseIntPipe) limit: number,
) {
  return this.userService.findAll({ isAdmin, page, limit });
}
```

**4 — main.ts Global Pipe**

```typescript
// src/main.ts
import { NestFactory } from '@nestjs/core';
import { ValidationPipe } from '@nestjs/common';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true,
      forbidNonWhitelisted: true,
      transform: true,
    }),
  );

  await app.listen(3000);
}
bootstrap();
```
