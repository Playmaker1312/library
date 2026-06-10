# Materi 65: Integrasi — Express + Prisma + PostgreSQL

---

## 1. Penjelasan

Ini adalah **puncak dari materi database** — menggabungkan tiga teknologi dalam satu API:

```
[Client] → HTTP → [Express Router] → [Prisma Client] → [PostgreSQL]
                      ↓                    ↓
               Validasi request      Query type-safe
               Routing               Auto-generate SQL
               Error handling        Relation handling
```

### Arsitektur: Repository Pattern

```
routes/         →  Define endpoints & HTTP methods
controllers/    →  Handle request/response logic
services/       →  Business logic (opsional)
repositories/   →  Akses database via Prisma (opsional)
prisma/         →  Schema & migrations
```

Untuk kesederhanaan, kita gabung controller + repository langsung di route handler.

---

## 2. Fungsi

- Menyediakan REST API lengkap untuk sistem perpustakaan.
- CRUD buku, anggota, dan peminjaman.
- Pagination, filtering, sorting dari query parameters.
- Error handling untuk constraint database (unique, foreign key).

---

## 3. Code

### 3.1 Setup Project

```bash
mkdir perpustakaan-api && cd perpustakaan-api
npm init -y
npm install express @prisma/client cors
npm install -D prisma typescript @types/express ts-node nodemon
npx prisma init
```

### 3.2 Schema Prisma (gunakan dari Materi 64)

### 3.3 Server Entry (`src/index.ts`)

```typescript
import express from 'express';
import cors from 'cors';
import bukuRoutes from './routes/bukuRoutes';
import anggotaRoutes from './routes/anggotaRoutes';
import peminjamanRoutes from './routes/peminjamanRoutes';

const app = express();
const PORT = process.env.PORT || 3000;

app.use(cors());
app.use(express.json());

app.use('/api/buku', bukuRoutes);
app.use('/api/anggota', anggotaRoutes);
app.use('/api/peminjaman', peminjamanRoutes);

app.listen(PORT, () => {
  console.log(`Server jalan di port ${PORT}`);
});
```

### 3.4 Route Handler (`src/routes/bukuRoutes.ts`)

```typescript
import { Router } from 'express';
import { PrismaClient } from '@prisma/client';

const router = Router();
const prisma = new PrismaClient();

// GET /api/buku?search=&page=1&limit=10&sortBy=judul&order=asc
router.get('/', async (req, res) => {
  const { search, page = '1', limit = '10', sortBy = 'judul', order = 'asc' } = req.query;

  const skip = (Number(page) - 1) * Number(limit);
  const take = Number(limit);

  const where = search
    ? {
        OR: [
          { judul: { contains: search as string, mode: 'insensitive' as const } },
          { penulis: { contains: search as string, mode: 'insensitive' as const } },
        ],
      }
    : {};

  const [data, total] = await Promise.all([
    prisma.buku.findMany({
      where,
      skip,
      take,
      orderBy: { [sortBy as string]: order },
    }),
    prisma.buku.count({ where }),
  ]);

  res.json({ data, total, page: Number(page), totalPages: Math.ceil(total / take) });
});

// GET /api/buku/:id
router.get('/:id', async (req, res) => {
  const buku = await prisma.buku.findUnique({
    where: { id: Number(req.params.id) },
    include: { peminjaman: { include: { anggota: true } } },
  });
  if (!buku) return res.status(404).json({ error: 'Buku tidak ditemukan' });
  res.json(buku);
});

// POST /api/buku
router.post('/', async (req, res) => {
  try {
    const buku = await prisma.buku.create({ data: req.body });
    res.status(201).json(buku);
  } catch (err: any) {
    if (err.code === 'P2002') return res.status(409).json({ error: 'ISBN sudah terdaftar' });
    res.status(400).json({ error: err.message });
  }
});

// PUT /api/buku/:id
router.put('/:id', async (req, res) => {
  try {
    const buku = await prisma.buku.update({
      where: { id: Number(req.params.id) },
      data: req.body,
    });
    res.json(buku);
  } catch (err: any) {
    if (err.code === 'P2025') return res.status(404).json({ error: 'Buku tidak ditemukan' });
    res.status(400).json({ error: err.message });
  }
});

// DELETE /api/buku/:id
router.delete('/:id', async (req, res) => {
  try {
    await prisma.buku.delete({ where: { id: Number(req.params.id) } });
    res.json({ message: 'Buku berhasil dihapus' });
  } catch (err: any) {
    if (err.code === 'P2003') return res.status(409).json({ error: 'Buku sedang direferensi peminjaman' });
    if (err.code === 'P2025') return res.status(404).json({ error: 'Buku tidak ditemukan' });
    res.status(400).json({ error: err.message });
  }
});

export default router;
```

### 3.5 Route Peminjaman (`src/routes/peminjamanRoutes.ts`)

```typescript
import { Router } from 'express';
import { PrismaClient } from '@prisma/client';

const router = Router();
const prisma = new PrismaClient();

// POST /api/peminjaman/pinjam — meminjam buku
router.post('/pinjam', async (req, res) => {
  const { bukuId, anggotaId } = req.body;

  const result = await prisma.$transaction(async (tx) => {
    const buku = await tx.buku.findUnique({ where: { id: bukuId } });
    if (!buku || buku.stok <= 0) throw new Error('Stok buku habis');

    await tx.buku.update({
      where: { id: bukuId },
      data: { stok: { decrement: 1 } },
    });

    return tx.peminjaman.create({
      data: { bukuId, anggotaId, status: 'dipinjam' },
    });
  });

  res.status(201).json(result);
});

// POST /api/peminjaman/kembalikan/:id — mengembalikan buku
router.post('/kembalikan/:id', async (req, res) => {
  const result = await prisma.$transaction(async (tx) => {
    const peminjaman = await tx.peminjaman.findUnique({
      where: { id: Number(req.params.id) },
    });
    if (!peminjaman || peminjaman.status === 'dikembalikan') {
      throw new Error('Peminjaman tidak valid atau sudah dikembalikan');
    }

    await tx.buku.update({
      where: { id: peminjaman.bukuId },
      data: { stok: { increment: 1 } },
    });

    return tx.peminjaman.update({
      where: { id: Number(req.params.id) },
      data: { status: 'dikembalikan', tglKembali: new Date() },
    });
  });

  res.json(result);
});

// GET /api/peminjaman?status=dipinjam
router.get('/', async (req, res) => {
  const where = req.query.status ? { status: req.query.status as string } : {};
  const data = await prisma.peminjaman.findMany({
    where,
    include: { buku: true, anggota: true },
    orderBy: { tglPinjam: 'desc' },
  });
  res.json(data);
});

export default router;
```

### 3.6 Contoh Request

```bash
# GET pagination + search
curl "http://localhost:3000/api/buku?search=laskar&page=1&limit=5"

# POST pinjam buku
curl -X POST http://localhost:3000/api/peminjaman/pinjam \
  -H "Content-Type: application/json" \
  -d '{"bukuId": 1, "anggotaId": 1}'
```

---

## 4. Analogi Rumah

| Lapisan | Analogi Rumah |
|---------|---------------|
| **Express** | **Pintu depan rumah** — semua tamu (request) masuk lewat sini. Tukang pos (router) mengarahkan ke ruangan yang tepat. |
| **Route Handler** | **Tukang pos** dalam rumah — membaca amplop (HTTP method + path) dan membawanya ke ruangan yang benar. |
| **Prisma Client** | **Penerjemah** — Anda bilang "ambilkan buku", dia bilang SQL ke gudang. |
| **Prisma Transaction** | **Satu perintah kerja** — "Buka lemari A, ambil paku, catat di buku, tutup lemari. Jika satu langkah gagal, urungkan semua." |
| **PostgreSQL** | **Gudang belakang** — tempat semua barang disimpan. |
| **Pagination** | **Ambil 10 barang dari rak, mulai dari barang ke-20.** Seperti membuka lemari secara bertahap. |
| **Error handling** | **Papan "Maaf, ruangan ini tutup"** — memberi tahu tamu dengan sopan jika ada masalah. |
| **Prisma Studio** | **Jendela kaca** untuk mengintip gudang tanpa harus ke belakang. |

### Arsitektur Lengkap Rumah

```
        [Tamu/Client]
             ↓ HTTP
        ┌────────────────────┐
        │  PINTU (Express)   │  ← middleware, CORS, parser JSON
        └────────┬───────────┘
                 ↓ Router
        ┌────────────────────┐
        │  RUANG TAMU        │  ← route handler: validasi, logic
        │  (Controller)      │
        └────────┬───────────┘
                 ↓ Prisma Client
        ┌────────────────────┐
        │  PENERJEMAH        │  ← ORM: JS ↔ SQL
        │  (Prisma)          │
        └────────┬───────────┘
                 ↓ SQL over TCP
        ┌────────────────────┐
        │  GUDANG            │  ← PostgreSQL
        │  (Database)        │
        └────────────────────┘
```

---

## 5. Use Case

### Use Case 1: API Buku — Search + Pagination
```bash
GET /api/buku?search=atomic&page=1&limit=10&sortBy=tahun&order=desc
```
Response: `{ data: [...], total: 1, page: 1, totalPages: 1 }`

### Use Case 2: Transaksi Pinjam Buku
```bash
POST /api/peminjaman/pinjam
Body: { "bukuId": 1, "anggotaId": 1 }
```
Aksi: Kurangi stok + buat record peminjaman (dalam 1 transaksi).

### Use Case 3: Filter peminjaman aktif dengan detail relasi
```bash
GET /api/peminjaman?status=dipinjam
```
Response array peminjaman dengan `include: { buku: true, anggota: true }`.

### Use Case 4: Error constraint handling
```
POST /api/buku { isbn: "yang-sudah-ada" } → 409 Conflict "ISBN sudah terdaftar"
DELETE /api/buku/1 (sedang dipinjam)      → 409 Conflict "Buku sedang direferensi"
```

---

## 6. Kesalahan Umum

1. **Tidak pakai `$transaction` untuk operasi multi-tabel** — Jika update stok gagal setelah insert peminjaman, stok buku berkurang tapi record peminjaman hilang. **Data inconsistent.**
2. **Lupa handle error Prisma** — Error code `P2002` (unique), `P2025` (not found), `P2003` (foreign key). Tanpa catch, server crash atau leak error detail ke client.
3. **Tidak sanitasi sorting** — `sortBy` dari query params bisa injection. Validasi hanya terhadap kolom yang diizinkan.
4. **N+1 di route detail** — Saambilmengambil detail buku beserta peminjamannya, pastikan pakai `include` bukan loop manual.
5. **Tidak disconnect Prisma** — App bisa kehabisan koneksi database. Panggil `prisma.$disconnect()` di `process.on('SIGINT', ...)`.

---

## 7. Benang Merah

```
Level 1 - JavaScript Dasar
Level 2 - Node.js & Express
Level 3 - Async, Database, Algoritma
              ↓
  Materi 60: Teori Database (SQL vs NoSQL)
  Materi 61: SQL DDL & DML
  Materi 62: SQL JOIN & Agregasi
  Materi 63: PostgreSQL Setup
  Materi 64: Prisma ORM
  Materi 65: Express + Prisma + PostgreSQL   ←── PUNCAK DATABASE
              ↓
      ┌───────┴───────┐
      ↓                ↓
  Algoritma        Level 3 Proyek
  (66-73)          (Capstone)
```

**Ini adalah puncak dari materi database Level 3.** Anda telah membangun:
- Database relasional (PostgreSQL)
- Abstraksi ORM type-safe (Prisma)
- REST API dengan Express
- Error handling, pagination, transaksi

Dari sini, lanjut ke **Algoritma (Materi 66-73)** atau langsung **Proyek Level 3**.

---

## 8. Soal & Jawaban

### Soal 1
**Apa fungsi `prisma.$transaction` dalam endpoint peminjaman? Mengapa transaction penting?**

<details>
<summary>Jawaban</summary>
`prisma.$transaction` menjalankan beberapa operasi database dalam satu kesatuan atomic — semua berhasil atau semua gagal (rollback). Dalam endpoint pinjam buku:
1. Cek stok buku.
2. Kurangi stok.
3. Buat record peminjaman.

Jika step 2 berhasil tapi step 3 gagal (misal koneksi putus), transaction menjamin bahwa pengurangan stok juga di-rollback. Tanpa transaction, stok buku bisa berkurang tanpa ada peminjaman tercatat — **data inconsistent**.
</details>

### Soal 2
**Jelaskan alur dari client request sampai response di arsitektur Express + Prisma + PostgreSQL.**

<details>
<summary>Jawaban</summary>
1. Client mengirim HTTP request ke Express server.
2. Express middleware parsing JSON, CORS check.
3. Router mengarahkan ke route handler yang sesuai (GET `/api/buku` → `bukuRoutes.ts`).
4. Route handler memanggil Prisma Client method (misal `findMany`).
5. Prisma menerjemahkan JS call ke SQL query dan mengirim ke PostgreSQL via TCP.
6. PostgreSQL menjalankan query, mengembalikan hasil.
7. Prisma memetakan hasil SQL ke objek JavaScript.
8. Route handler mengirim response JSON ke client.
</details>

### Soal 3
**Apa itu Repository Pattern? Apakah kita menerapkannya di kode di atas? Jelaskan.**

<details>
<summary>Jawaban</summary>
Repository pattern adalah pola desain yang memisahkan logika akses database dari logika bisnis. Di kode di atas, kita **tidak** menerapkan pemisahan yang ketat — akses Prisma langsung di route handler (gabung controller + repository).

Pola idealnya:
```
routes/ → controllers/ → services/ → repositories/ → prisma
```

Untuk aplikasi kecil seperti ini, akses langsung di route handler cukup. Untuk aplikasi besar, repository pattern memudahkan testing (mocking Prisma) dan perubahan database (ganti ORM tanpa sentuh controller).
</details>
