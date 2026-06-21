# 43 - Prisma Setup - Init, Schema, Migrate & Generate

## Penjelasan

Setelah kita memahami dasar-dasar NestJS (module, controller, service) dan konsep dependency injection, langkah selanjutnya adalah menghubungkan aplikasi kita ke database. Prisma adalah ORM modern untuk Node.js/TypeScript yang menyediakan type safety, auto-completion, dan migrasi database yang mudah. Prisma menggantikan cara manual menulis query SQL atau menggunakan ORM tradisional seperti TypeORM.

Jika NestJS adalah **arsitek proyek gedung**, Prisma adalah **tim kontraktor spesialis kelistrikan dan pipa** — dia menangani semua urusan database dengan rapi, terstruktur, dan type-safe.

## Fungsi

- **Prisma Init**: Membuat struktur folder Prisma dan file `.env` untuk konfigurasi database
- **Schema Prisma**: Mendefinisikan model data (tabel) dan relasi antar tabel
- **Migrate Dev**: Membuat dan menjalankan migration berdasarkan perubahan schema
- **Generate**: Membuat Prisma Client berdasarkan schema untuk digunakan di kode
- **Studio**: GUI untuk melihat dan mengedit data di database
- **DB Seed**: Mengisi database dengan data awal

## Cara Pengimplementasian

### 1. Install Prisma

```bash
npm install @prisma/client
npm install -D prisma
```

### 2. Init Prisma

```bash
npx prisma init
```

Perintah ini membuat:
- `prisma/schema.prisma` — file utama untuk mendefinisikan model
- `.env` — file environment untuk konfigurasi database

### 3. Konfigurasi Schema Prisma

```prisma
// prisma/schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String
  posts     Post[]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Post {
  id        Int      @id @default(autoincrement())
  title     String
  content   String?
  published Boolean  @default(false)
  authorId  Int
  author    User     @relation(fields: [authorId], references: [id])
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

### 4. Konfigurasi .env

```
DATABASE_URL="postgresql://user:password@localhost:5432/mydb?schema=public"
```

### 5. Migration

```bash
npx prisma migrate dev --name init
```

Perintah ini:
- Membandingkan schema dengan database
- Membuat file migration SQL di `prisma/migrations/`
- Menjalankan migration ke database
- Men-generate Prisma Client

### 6. Generate Prisma Client

```bash
npx prisma generate
```

Biasanya sudah otomatis saat `migrate dev`, tapi bisa dijalankan sendiri jika hanya ingin update client.

### 7. Studio

```bash
npx prisma studio
```

Membuka GUI di `http://localhost:5555` untuk melihat/mengedit data.

### 8. DB Seed

Setup di `package.json`:

```json
"prisma": {
  "seed": "ts-node prisma/seed.ts"
}
```

Jalankan:

```bash
npx prisma db seed
```

## Analogi

**Membangun Gedung Bertingkat**

- `npx prisma init` = **menyiapkan lahan** dan mendatangkan kontraktor database
- `schema.prisma` = **blueprint gedung** — denah setiap lantai (tabel) dan hubungan antar lantai
- `migrate dev` = **proses pembangunan fondasi** — cetak beton sesuai blueprint
- `generate` = **memberi kunci dan saklar** ke setiap ruangan agar bisa dipakai
- `studio` = **jalan-jalan keliling gedung** melihat hasil bangunan lewat GUI
- `db seed` = **menyiapkan furniture awal** sebelum penghuni pindah

## Dipakai untuk Apa

- Inisialisasi project baru yang membutuhkan database
- Membuat dan mengelola skema database dari kode
- Melihat dan memverifikasi data selama development
- Mengisi data dummy/testing secara terstruktur

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Lupa menjalankan `prisma generate` setelah schema berubah | `postinstall` script: `"postinstall": "prisma generate"` |
| Migrasi gagal karena database tidak bisa diakses | Cek `DATABASE_URL` dan pastikan database server running |
| Konflik migration karena pull terbaru dari tim | `prisma migrate dev` akan mendeteksi dan membuat migration baru |
| Data hilang saat migration karena kolom dihapus | Gunakan `prisma migrate dev --created-only` untuk preview |

## Soal Latihan

1. Inisialisasi Prisma di project NestJS baru
2. Definisikan model `User` dengan field: id (UUID), email (unique), name, password
3. Definisikan model `Post` dengan field: id, title, content, published, authorId (relasi ke User)
4. Jalankan migration dengan nama "init"
5. Jalankan Prisma Studio untuk melihat hasil

### Jawaban

**1-3: Schema**

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        String   @id @default(uuid())
  email     String   @unique
  name      String
  password  String
  posts     Post[]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Post {
  id        String   @id @default(uuid())
  title     String
  content   String?
  published Boolean  @default(false)
  authorId  String
  author    User     @relation(fields: [authorId], references: [id])
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

**4. Migration**

```bash
npx prisma migrate dev --name init
```

**5. Studio**

```bash
npx prisma studio
```
