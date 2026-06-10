# Materi 64: Prisma ORM

---

## 1. Penjelasan

**ORM** (Object-Relational Mapping) adalah lapisan yang menerjemahkan antara kode aplikasi (JavaScript/TypeScript) dan database relasional (SQL). Alih-alih menulis SQL mentah, Anda menulis kode JS — ORM yang mengurus konversinya.

**Prisma** adalah ORM modern untuk Node.js/TypeScript dengan keunggulan:
- **Type-safe**: Setiap query dicek tipe-nya saat compile. Error ketahuan sebelum runtime.
- **Auto-completion**: IDE suggestion penuh berkat Prisma Client yang digenerate.
- **Migrations otomatis**: Ubah schema → Prisma buat file migration.
- **Studio GUI**: Visual database editor built-in (`npx prisma studio`).

### Alur Kerja Prisma
```
Schema (schema.prisma)
    ↓ prisma migrate
Migration SQL files + Database tables
    ↓ prisma generate
Prisma Client (type-safe JS/TS)
    ↓ digunakan di
Aplikasi Express / Next.js / dll
```

---

## 2. Fungsi

- **Schema-first approach**: Definisikan model data di `schema.prisma`, Prisma generate semuanya.
- **CRUD otomatis**: Prisma Client menyediakan method `.create()`, `.findMany()`, `.update()`, `.delete()`.
- **Migrations**: Version control untuk perubahan database — bisa rollback.
- **Relation handling**: Termasuk nested create, eager loading, lazy loading.

---

## 3. Code

### 3.1 Installasi & Setup

```bash
# Inisialisasi project
mkdir myapp && cd myapp
npm init -y
npm install prisma @prisma/client
npx prisma init
```

### 3.2 Schema (`prisma/schema.prisma`)

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model Buku {
  id        Int          @id @default(autoincrement())
  judul     String
  penulis   String
  isbn      String?      @unique
  tahun     Int?
  stok      Int          @default(1)
  peminjaman Peminjaman[]
  dibuatPada DateTime    @default(now())
}

model Anggota {
  id        Int          @id @default(autoincrement())
  nama      String
  email     String       @unique
  telepon   String?
  alamat    String?
  peminjaman Peminjaman[]
}

model Peminjaman {
  id          Int      @id @default(autoincrement())
  buku        Buku     @relation(fields: [bukuId], references: [id])
  bukuId      Int
  anggota     Anggota  @relation(fields: [anggotaId], references: [id])
  anggotaId   Int
  tglPinjam   DateTime @default(now())
  tglKembali  DateTime?
  status      String   @default("dipinjam")
}
```

### 3.3 Environment Variables (`.env`)

```env
DATABASE_URL="postgresql://librarian:rahasia123@localhost:5432/perpustakaan_db"
```

### 3.4 Migrate & Generate

```bash
npx prisma migrate dev --name init
npx prisma generate
```

### 3.5 CRUD dengan Prisma Client

```typescript
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

async function main() {
  // CREATE
  const bukuBaru = await prisma.buku.create({
    data: {
      judul: 'Atomic Habits',
      penulis: 'James Clear',
      isbn: '9780735211292',
      stok: 5,
    },
  });

  // READ (semua buku)
  const semuaBuku = await prisma.buku.findMany({
    orderBy: { judul: 'asc' },
  });

  // READ (filter + include relasi)
  const peminjamanAktif = await prisma.peminjaman.findMany({
    where: { status: 'dipinjam' },
    include: {
      buku: true,
      anggota: true,
    },
  });

  // UPDATE
  await prisma.buku.update({
    where: { id: 1 },
    data: { stok: { decrement: 1 } },
  });

  // DELETE
  await prisma.peminjaman.delete({
    where: { id: 5 },
  });
}

main()
  .catch(console.error)
  .finally(() => prisma.$disconnect());
```

---

## 4. Analogi Rumah

| Prisma | Analogi Rumah |
|--------|---------------|
| `schema.prisma` | Cetak biru (blueprint) rumah — menentukan di mana ruangan, pintu, jendela. |
| Prisma Migrate | Kontraktor yang membaca cetak biru dan **membangun** ruangan sungguhan. |
| Prisma Client | **Penerjemah** — Anda bicara bahasa Indonesia (JS), dia bicara Inggris (SQL) ke database. |
| `prisma.generate()` | Pelatih yang mengajarkan penerjemah kosakata baru sesuai cetak biru. |
| `findMany()` | "Tolong ambilkan semua paku dari gudang." |
| `include` | "Sekalian ambilkan juga label rak-nya." |
| `where` | "Yang warna merah saja." |
| Transaction | "Saya ingin ambil paku **dan** catat pengambilannya — jika satu gagal, batal semua." |
| `npx prisma studio` | Jendela kaca di gudang — bisa lihat isi tanpa harus masuk. |

---

## 5. Use Case

### Use Case 1: Nested create (buat peminjaman + update stok dalam 1 request)
```typescript
const pinjam = await prisma.peminjaman.create({
  data: {
    bukuId: 1,
    anggotaId: 1,
    status: 'dipinjam',
  },
});

// Update stok manual (atau pakai transaction + $transaction)
await prisma.buku.update({
  where: { id: 1 },
  data: { stok: { decrement: 1 } },
});
```

### Use Case 2: Pagination (skip/take)
```typescript
const halaman2 = await prisma.buku.findMany({
  skip: 10,
  take: 10,
  orderBy: { judul: 'asc' },
});
```

### Use Case 3: Filter + search
```typescript
const cari = await prisma.buku.findMany({
  where: {
    OR: [
      { judul: { contains: 'laskar', mode: 'insensitive' } },
      { penulis: { contains: 'hirata', mode: 'insensitive' } },
    ],
  },
});
```

---

## 6. Kesalahan Umum

1. **Lupa menjalankan `prisma generate` setelah ubah schema** — Client tidak sinkron dengan schema terbaru.
2. **`N+1 problem`** — Memanggil `findMany` untuk parent lalu loop `include` child satu per satu. Solusi: pakai `include` atau `select` sekali jalan.
3. **Tidak pakai `$transaction` untuk operasi multi-tabel** — Jika update stok gagal setelah insert peminjaman, data tidak konsisten.
4. **Menyimpan password di `.env` yang ikut ter-commit** — Pastikan `.env` ada di `.gitignore`.
5. **Lupa `await` pada query** — Prisma mengembalikan Promise. Tanpa await, query tidak jalan.

---

## 7. Benang Merah

```
Materi 63 (PostgreSQL — database sudah siap)
    ↓  "Menulis SQL manual itu rentan error, butuh abstraksi"
*Materi 64: Prisma ORM*  ←── ANDA DI SINI
    ↓  "ORM sudah siap, sekarang integrasi dengan Express"
Materi 65: Express + Prisma + PostgreSQL
    ↓
Level 3 Capstone Project
```

---

## 8. Soal & Jawaban

### Soal 1
**Apa fungsi `prisma migrate dev` dan `prisma generate`? Jelaskan perbedaannya.**

<details>
<summary>Jawaban</summary>
- `prisma migrate dev`: Membaca perubahan di `schema.prisma`, membuat file migration SQL, dan menjalankannya ke database. Fungsinya **mengubah struktur database** (DDL).
- `prisma generate`: Membaca `schema.prisma` yang sudah sinkron dengan database dan **mengenerate Prisma Client** (kode JS/TS type-safe). Harus dijalankan setiap kali schema berubah.

Urutan: ubah schema → `migrate dev` → `generate` → gunakan Client.
</details>

### Soal 2
**Apa yang dimaksud N+1 problem di Prisma? Bagaimana cara menghindarinya?**

<details>
<summary>Jawaban</summary>
N+1 problem terjadi saat Anda query parent (1 query) lalu di-loop untuk mengambil child tiap parent (N query). Contoh: ambil semua buku, lalu untuk setiap buku cari peminjamannya.
```typescript
// N+1: 1 query buku + N query peminjaman
const bukuList = await prisma.buku.findMany();
for (const b of bukuList) {
  const p = await prisma.peminjaman.findMany({ where: { bukuId: b.id } });
}

// Solusi: 1 query dengan include
const result = await prisma.buku.findMany({
  include: { peminjaman: true },
});
```
</details>

### Soal 3
**Dalam analogi rumah, apa yang dimaksud dengan `schema.prisma` dan `prisma migrate`?**

<details>
<summary>Jawaban</summary>
- `schema.prisma` = **Cetak biru (blueprint)** rumah: denah ruangan, letak pintu, ukuran jendela. Semua detail struktur ditentukan di sini.
- `prisma migrate dev` = **Kontraktor** yang membaca cetak biru lalu membangun tembok, memasang pintu, dan mengecat sesuai blueprint. Setiap perubahan blueprint akan diikuti perubahan fisik bangunan.
</details>
