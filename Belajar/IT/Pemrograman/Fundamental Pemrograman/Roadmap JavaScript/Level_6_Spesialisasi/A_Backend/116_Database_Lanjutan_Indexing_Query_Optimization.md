# 116 — Database Lanjutan: Indexing, Query Optimization, Sharding, Replication

## 1. Penjelasan

Database lanjutan membahas teknik skala dan optimasi di luar CRUD dasar. Empat pilar utama:

| Konsep | Fungsi |
|--------|--------|
| **Index** | Mempercepat pencarian data tanpa full table scan |
| **Query Optimization** | Menulis ulang query agar efisien (EXPLAIN, hindari N+1) |
| **Sharding** | Partisi horizontal — membagi data ke beberapa server |
| **Replication** | Duplikasi data ke beberapa node (baca/tulis terpisah) |

## 2. Fungsi

- **Index (B-tree, Compound Index):** Mempercepat WHERE, JOIN, ORDER BY.
- **EXPLAIN ANALYZE:** Melihat eksekusi query — apakah pakai index atau full scan.
- **Sharding:** Mengatasi data terlalu besar untuk satu server.
- **Replication (Master-Slave):** Master untuk write, slave untuk read — meningkatkan throughput.

## 3. Code

```sql
-- Membuat compound index
CREATE INDEX idx_user_email_status ON users (email, status);

-- Sebelum optimasi (full scan)
EXPLAIN ANALYZE SELECT * FROM orders WHERE total > 1000;

-- Setelah optimasi (pakai index)
CREATE INDEX idx_orders_total ON orders (total);
EXPLAIN ANALYZE SELECT * FROM orders WHERE total > 1000;
```

```javascript
// Prisma — setup read replica
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
  directUrl = env("DATABASE_URL_DIRECT") // untuk write
}

// Di koneksi:
const { PrismaClient } = require('@prisma/client')
const prisma = new PrismaClient({
  datasources: {
    db: {
      url: process.env.DATABASE_URL_REPLICA // read replica
    }
  }
})
```

## 4. Analogi Rumah

| Database | Analogi Rumah |
|----------|---------------|
| Tanpa index | Baca buku dari halaman 1 sampai ketemu (full scan) |
| Index B-tree | Daftar isi buku — langsung lompat ke halaman |
| Compound index | Indeks abjad + nomor halaman gabungan |
| Sharding | Pisah buku ke beberapa rak berdasarkan abjad A–M, N–Z |
| Replication | Fotokopi buku untuk perpustakaan cabang |

## 5. Use Case

- **Index:** Aplikasi e-commerce — filter produk by kategori + harga.
- **Sharding:** Database pengguna 100 juta — pisah berdasarkan region.
- **Replication:** Sistem dengan rasio baca:tulis 80:20 — baca dari replica.

## 6. Kesalahan Umum

- **Over-indexing:** Index mempercepat SELECT tapi memperlambat INSERT/UPDATE.
- **Shard key salah:** Pilih kolom dengan distribusi tidak merata (hotspot).
- **Replication lag:** Data dibaca dari replica sebelum selesai sinkron dari master.

## 7. Benang Merah

Database adalah fondasi materi **Level 5 (Database Dasar)** → **115 (System Design)** → **116 (Database Lanjutan)** → **117 (Redis Lanjutan)**. Indexing dan sharding adalah prasyarat untuk memahami caching (Redis) dan distributed systems.

## 8. Soal & Jawaban

### Soal 1
Apa perbedaan sharding dan replication?

<details>
<summary>Jawaban</summary>
Sharding membagi data secara horizontal (setiap server punya subset data berbeda). Replication menduplikasi data yang sama ke beberapa server.
</details>

### Soal 2
Mengapa compound index lebih efisien daripada dua index terpisah untuk query `WHERE email = 'x' AND status = 'y'`?

<details>
<summary>Jawaban</summary>
Compound index menyimpan kedua kolom dalam satu struktur B-tree, sehingga database bisa mencocokkan kedua kondisi sekaligus tanpa perlu menggabungkan hasil dua index berbeda (bitmap scan).
</details>

### Soal 3
Apa risiko utama dari read replication?

<details>
<summary>Jawaban</summary>
Replication lag — data yang baru ditulis ke master belum tersedia di slave, menyebabkan stale read.
</details>
