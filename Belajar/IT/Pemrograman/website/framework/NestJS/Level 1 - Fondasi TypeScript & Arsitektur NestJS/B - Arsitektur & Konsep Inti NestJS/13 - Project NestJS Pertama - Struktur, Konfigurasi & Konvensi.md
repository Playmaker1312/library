# 13 - Project NestJS Pertama - Struktur, Konfigurasi & Konvensi

## Penjelasan

Setelah memahami semua konsep inti NestJS — Module, DI, Custom Provider, Request Lifecycle — saatnya **praktek bikin project nyata**.

Seperti arsitek yang pertama kali datang ke lokasi proyek, kita akan: mendirikan gedung (nest new), mengatur cetak biru (nest-cli.json, tsconfig.json), menunjuk kontraktor (eslint, prettier), dan membangun lantai pertama dengan cepat (nest g resource).

Tujuan utama: membuat developer bisa **produktif dalam 5 menit pertama**.

## Fungsi

- **Scaffolding** project dengan struktur standar NestJS dalam hitungan detik
- **Konfigurasi** TypeScript, linting, formatting secara otomatis
- **Code generation** dengan Nest CLI untuk module, controller, service, dll
- **Konvensi** penamaan dan struktur folder yang konsisten

## Cara Pengimplementasian / Code

### 1. Install NestJS CLI

```bash
# Install globally
npm install -g @nestjs/cli

# Cek versi
nest --version
```

**Analogi**: `nest` CLI adalah **mandor proyek** yang bisa mendirikan gedung dalam hitungan detik.

### 2. Membuat Project Baru

```bash
nest new my-app
```

Selama proses, CLI akan bertanya:
- **Package manager**: npm / yarn / pnpm — pilih npm
- **Project name**: my-app (default)

Struktur yang dihasilkan:

```
my-app/
├── src/
│   ├── app.controller.ts      # Controller root
│   ├── app.controller.spec.ts # Test controller
│   ├── app.module.ts          # Root module
│   ├── app.service.ts         # Service root
│   └── main.ts                # Entry point
├── test/
│   ├── app.e2e-spec.ts        # E2E test
│   └── jest-e2e.json          # Jest config E2E
├── nest-cli.json              # Konfigurasi Nest CLI
├── tsconfig.json              # Konfigurasi TypeScript
├── tsconfig.build.json        # TSConfig untuk build
├── .eslintrc.js               # ESLint config
├── .prettierrc                # Prettier config
├── jest.config.js             # Jest config
├── package.json
├── tsconfig.json
└── README.md
```

### 3. main.ts — Entry Point

```typescript
// src/main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Konfigurasi tambahan
  app.setGlobalPrefix('api');   // Semua route jadi /api/...
  app.enableCors();              // Enable CORS

  await app.listen(process.env.PORT || 3000);
  console.log(`Application running on: ${await app.getUrl()}`);
}
bootstrap();
```

**Analogi**: `main.ts` adalah **pintu utama gedung** dan **ruang kontrol** — tempat pertama kali kita menyalakan listrik gedung.

### 4. app.module.ts — Root Module

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],               // Module lain
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

### 5. nest-cli.json — Konfigurasi CLI

```json
{
  "$schema": "https://json.schemastore.org/nest-cli",
  "collection": "@nestjs/schematics",
  "sourceRoot": "src",
  "compilerOptions": {
    "deleteOutDir": true
  }
}
```

Keterangan:
- `sourceRoot`: folder sumber kode (default: `src`)
- `deleteOutDir`: hapus folder `dist` sebelum build ulang
- `collection`: package schematics yang dipakai (default dari NestJS)

### 6. tsconfig.json — TypeScript Configuration

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "declaration": true,
    "removeComments": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "allowSyntheticDefaultImports": true,
    "target": "ES2021",
    "sourceMap": true,
    "outDir": "./dist",
    "baseUrl": "./",
    "incremental": true,
    "skipLibCheck": true,
    "strictNullChecks": true,
    "noImplicitAny": true,
    "strictBindCallApply": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true
  }
}
```

**Yang PALING PENTING** untuk NestJS:
- `experimentalDecorators`: true (agar bisa pakai `@Controller`, `@Get`, dll)
- `emitDecoratorMetadata`: true (agar NestJS bisa baca tipe parameter untuk DI)

### 7. Generate Resource dengan Nest CLI

Ini adalah **fitur paling powerfull** Nest CLI — generate seluruh CRUD dalam satu perintah.

```bash
nest g resource product
```

CLI akan bertanya:
```
? What transport layer do you use? (Use arrow keys)
> REST API          ← Pilih ini
  GraphQL (code first)
  GraphQL (schema first)
  Microservice (non-HTTP)
  WebSockets

? Would you like to generate CRUD entry points? (Y/n) Y ← Yes
```

Hasil generate:

```
src/
└── product/
    ├── dto/
    │   ├── create-product.dto.ts
    │   └── update-product.dto.ts
    ├── entities/
    │   └── product.entity.ts
    ├── product.controller.ts
    ├── product.controller.spec.ts
    ├── product.module.ts
    ├── product.service.ts
    └── product.service.spec.ts
```

**Hasil generate — isi file yang dibuat:**

```typescript
// product.module.ts
@Module({
  controllers: [ProductController],
  providers: [ProductService],
})
export class ProductModule {}
```

```typescript
// product.controller.ts
@Controller('product')
export class ProductController {
  constructor(private readonly productService: ProductService) {}

  @Post()
  create(@Body() createProductDto: CreateProductDto) {
    return this.productService.create(createProductDto);
  }

  @Get()
  findAll() {
    return this.productService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.productService.findOne(+id);
  }

  @Patch(':id')
  update(@Param('id') id: string, @Body() updateProductDto: UpdateProductDto) {
    return this.productService.update(+id, updateProductDto);
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return this.productService.remove(+id);
  }
}
```

```typescript
// product.service.ts
@Injectable()
export class ProductService {
  create(createProductDto: CreateProductDto) {
    return 'This action adds a new product';
  }

  findAll() {
    return `This action returns all product`;
  }

  findOne(id: number) {
    return `This action returns a #${id} product`;
  }

  update(id: number, updateProductDto: UpdateProductDto) {
    return `This action updates a #${id} product`;
  }

  remove(id: number) {
    return `This action removes a #${id} product`;
  }
}
```

### 8. Menjalankan Project

```bash
# Development mode (watch mode)
npm run start:dev

# Production build
npm run build

# Production start
npm run start:prod
```

### 9. Struktur Folder Final

```
my-app/
├── src/
│   ├── product/
│   │   ├── dto/
│   │   │   ├── create-product.dto.ts
│   │   │   └── update-product.dto.ts
│   │   ├── entities/
│   │   │   └── product.entity.ts
│   │   ├── product.controller.ts
│   │   ├── product.controller.spec.ts
│   │   ├── product.module.ts
│   │   ├── product.service.ts
│   │   └── product.service.spec.ts
│   ├── app.controller.spec.ts
│   ├── app.controller.ts
│   ├── app.module.ts
│   ├── app.service.ts
│   └── main.ts
├── test/
│   └── app.e2e-spec.ts
├── nest-cli.json
├── tsconfig.json
├── tsconfig.build.json
├── .eslintrc.js
├── .prettierrc
├── jest.config.js
└── package.json
```

## Analogi (Gedung Bertingkat)

Proses setup project NestJS seperti **mendirikan gedung dari nol**:

| Tahap | Command/Action | Analogi |
|-------|---------------|---------|
| `npm install -g @nestjs/cli` | **Menyewa mandor proyek** | Si mandor tahu cara mendirikan gedung standar |
| `nest new my-app` | **Menggali fondasi & cetak biru** | Mandor membuat pondasi, memasang kerangka dasar |
| `nest-cli.json` | **Buku panduan gedung** | Catatan: bagaimana cara membangun, di mana letak ruangan |
| `tsconfig.json` | **Standar material bangunan** | Tentukan jenis bata, ukuran besi (kompilasi TS) |
| `.eslintrc.js` + `.prettierrc` | **Aturan kebersihan & kerapihan** | Semua pekerja harus rapi, seragam |
| `main.ts` | **Panel listrik utama** | Tempat menyalakan semua sistem gedung |
| `app.module.ts` | **Lobi & papan direktori** | Daftar lengkap: lantai apa saja yang ada di gedung |
| `nest g resource product` | **Bangun satu lantai lengkap** | Lantai "Product" — langsung jadi dengan ruangan (controller), staf (service), dan prosedur (CRUD) |
| `npm run start:dev` | **Nyalakan listrik** | Gedung mulai beroperasi |

## Dipakai Untuk Apa

- **Semua project NestJS** dimulai dari sini
- **Rapid prototyping** — dalam 5 menit sudah punya REST API dengan CRUD lengkap
- **Standarisasi tim** — semua proyek NestJS punya struktur yang sama, memudahkan onboarding developer baru
- **Code generation** untuk module, controller, service, guard, pipe, interceptor — semuanya bisa lewat CLI

## Kesalahan Umum

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| Lupa `experimentalDecorators: true` di tsconfig.json | Decorator tidak bekerja, error kompilasi | Pastikan tsconfig.json punya `experimentalDecorators: true` |
| Tidak menjalankan `nest build` sebelum production | Aplikasi tidak jalan | Jalankan `npm run build` |
| Manual create file tanpa Nest CLI | Struktur tidak konsisten, ketinggalan boilerplate | Gunakan `nest g` — lebih cepat dan standar |
| Semua kode di root module | Module jadi raksasa | Pisahkan per fitur dengan `nest g module` |
| Lupa register module di `imports` | Error provider tidak ditemukan | Setiap feature module harus di-import di AppModule |
| Menimpa file hasil generate Nest CLI manual | Konflik saat generate ulang | Gunakan `--no-spec` jika tidak butuh test file |

## Soal Latihan & Jawaban

### Soal 1
Buatlah project NestJS baru dengan nama `blog-app` dan generate resource `article` dengan CRUD lengkap. Tuliskan perintah-perintah yang diperlukan.

**Jawaban:**

```bash
# 1. Install CLI (jika belum ada)
npm install -g @nestjs/cli

# 2. Buat project
nest new blog-app
# (pilih npm sebagai package manager)

# 3. Masuk ke folder
cd blog-app

# 4. Generate resource article dengan CRUD
nest g resource article
# Pilih: REST API → Yes (CRUD entry points)

# 5. Jalankan development server
npm run start:dev

# 6. Test endpoint
# GET http://localhost:3000/article → "This action returns all article"
# POST http://localhost:3000/article → "This action adds a new article"
# GET http://localhost:3000/article/1 → "This action returns a #1 article"
# PATCH http://localhost:3000/article/1 → "This action updates a #1 article"
# DELETE http://localhost:3000/article/1 → "This action removes a #1 article"
```

### Soal 2
Sebutkan isi dan fungsi dari file `nest-cli.json`.

**Jawaban:**
`nest-cli.json` adalah file konfigurasi untuk NestJS CLI. Isinya:
- `$schema`: referensi schema JSON untuk autocomplete
- `collection`: package schematics yang digunakan (`@nestjs/schematics`)
- `sourceRoot`: folder sumber kode (`src`)
- `compilerOptions.deleteOutDir`: menghapus folder `dist` sebelum build ulang

### Soal 3
Apa fungsi dari `emitDecoratorMetadata: true` di tsconfig.json?

**Jawaban:**
`emitDecoratorMetadata` memerintahkan TypeScript untuk menyertakan metadata tipe (type metadata) saat kompilasi. NestJS membutuhkan metadata ini untuk **Dependency Injection** — membaca tipe parameter di constructor agar tahu provider mana yang harus di-inject. Tanpa setting ini, `@Injectable()` dan constructor injection tidak akan bekerja.

### Soal 4
Apa yang dilakukan perintah `nest g resource product`? File apa saja yang dihasilkan?

**Jawaban:**
Perintah `nest g resource product` (dengan jawaban REST API + Yes untuk CRUD) akan menghasilkan 7 file:
1. `product/product.module.ts` — Module
2. `product/product.controller.ts` — Controller dengan 5 method (CRUD)
3. `product/product.controller.spec.ts` — Test controller
4. `product/product.service.ts` — Service dengan 5 method (CRUD)
5. `product/product.service.spec.ts` — Test service
6. `product/dto/create-product.dto.ts` — DTO untuk create
7. `product/dto/update-product.dto.ts` — DTO untuk update
8. `product/entities/product.entity.ts` — Entity

Serta otomatis mendaftarkan `ProductModule` ke `AppModule`.
