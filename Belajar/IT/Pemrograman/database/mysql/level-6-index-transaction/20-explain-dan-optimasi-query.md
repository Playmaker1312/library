# 20. EXPLAIN dan Optimasi Query

> **Level 6 - Modul K** | **Durasi**: 3 hari | **Prasyarat**: 18 Index, 19 Transaction

---

## 1. Penjelasan Singkat & Konsep Dasar

`EXPLAIN` = **X-ray untuk query** — lihat *bagaimana* MySQL eksekusi: pakai index apa, scan berapa baris, ada filesort?

Tanpa EXPLAIN, optimasi tebak-tebakan. Dengan EXPLAIN, ukur dulu, baru index/refactor.

**Hubungan dengan materi sebelumnya**: Level 2-5 query benar, Level 6 index & transaction bikin cepat+aman. 20 adalah *alat ukur* untuk buktikan cepat/lambat.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Fitur | Fungsi |
|-------|--------|
| `EXPLAIN SELECT ...` | Tampilkan plan: type, key, rows, Extra |
| `EXPLAIN ANALYZE SELECT ...` (8.0.18+) | Plan + waktu aktual |
| `Slow Query Log` | Tangkap query > long_query_time |
| `Optimizer Hint` | Paksa index jika perlu |
| `Profiling` | Ukur waktu per stage |

Kolom EXPLAIN penting: `type (ALL=scan buruk, ref/range/index=baik)`, `key (index dipakai)`, `rows (estimasi)`, `Extra (Using filesort, Using index, Using temporary)`.

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── EXPLAIN dasar ───────────────────────────────────────────────
EXPLAIN SELECT * FROM buku WHERE kategori='Teknologi';
-- +----+-------------+-------+------+---------------+------+---------+-------+------+-------------+
-- | id | select_type | table | type | possible_keys | key  | rows | Extra                    |
-- +----+-------------+-------+------+---------------+------+---------+-------+------+-------------+
-- | 1  | SIMPLE      | buku  | ref  | idx_kat       | idx_kat | 3 | Using index condition |
-- type=ref (pakai index), key=idx_kat (kepakai), Extra baik

EXPLAIN SELECT * FROM buku WHERE YEAR(created_at)=2024;
-- type=ALL, key=NULL, rows=15, Extra=Using where → SCAN! Buruk, karena fungsi di kolom

-- Perbaiki: sargable
EXPLAIN SELECT * FROM buku WHERE created_at >= '2024-01-01' AND created_at < '2025-01-01';
-- type=range, key=idx_created → pakai index

-- ─── EXPLAIN ANALYZE (aktual) ────────────────────────────────────
EXPLAIN ANALYZE SELECT kategori, COUNT(*) FROM buku GROUP BY kategori;
-- Tampilkan actual time & rows

-- ─── Optimasi: hindari SELECT *, pakai covering index ───────────
CREATE INDEX idx_kat_harga ON buku(kategori, harga);
EXPLAIN SELECT kategori, harga FROM buku WHERE kategori='Teknologi';
-- Extra: Using index → covering, tidak baca tabel!

-- ─── JOIN optimasi ───────────────────────────────────────────────
EXPLAIN SELECT a.nama, b.judul FROM peminjaman p
JOIN buku b ON p.buku_id=b.id JOIN anggota a ON p.anggota_id=a.id
WHERE p.status='dipinjam';
-- Pastikan p.anggota_id, p.buku_id index (FK sudah)

-- ─── Slow log ────────────────────────────────────────────────────
SET GLOBAL slow_query_log = ON;
SET GLOBAL long_query_time = 0.1; -- 100ms
SHOW VARIABLES LIKE 'slow_query%';
-- Lihat log: mysqldumpslow / performance_schema

-- ─── Optimizer hint (jika perlu paksa) ───────────────────────────
SELECT * FROM buku USE INDEX (idx_kat) WHERE kategori='Teknologi';

-- ─── Profiling ───────────────────────────────────────────────────
SET profiling = 1;
SELECT * FROM buku WHERE kategori='Teknologi' ORDER BY harga DESC LIMIT 5;
SHOW PROFILES;
SHOW PROFILE FOR QUERY 1;
```

**Skenario Nyata (E-commerce — Query Lambat):**

```sql
-- Sebelum: SELECT * FROM produk WHERE YEAR(created_at)=2024 AND kategori_id=3
EXPLAIN SELECT * FROM produk WHERE YEAR(created_at)=2024;
-- ALL, 1M rows

-- Sesudah:
CREATE INDEX idx_kat_created ON produk(kategori_id, created_at);
SELECT id, nama, harga FROM produk
WHERE kategori_id=3 AND created_at BETWEEN '2024-01-01' AND '2024-12-31';
-- range, 10k rows, 10x lebih cepat
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| **EXPLAIN** | **Tanya pustakawan "rencanamu gimana?"** sebelum dia jalan — mau lewat katalog atau keliling semua rak? |
| **type=ALL** | **Keliling semua rak** — lambat |
| **type=ref/range** | **Lewat katalog** — cepat |
| **Using filesort** | **Harus susun ulang di meja** — butuh index untuk hindari |
| **Covering index** | **Jawab dari katalog saja** — tidak perlu ke rak |

> 💡 **Key Insight**: Jangan suruh pustakawan jalan dulu baru nilai — tanya rencananya via EXPLAIN!

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | Aksi |
|---------|------|
| Query >100ms | `EXPLAIN` + `EXPLAIN ANALYZE` |
| Pagination lambat | Cek `Using filesort`, tambah index ORDER BY |
| JOIN lambat | Cek FK index, type=ALL? |
| Production monitoring | `slow_query_log` + `performance_schema` |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ Selalu EXPLAIN sebelum buat index
-- ✅ Sargable: hindari fungsi di kolom WHERE
WHERE created_at >= '2024-01-01' -- baik
WHERE YEAR(created_at)=2024 -- buruk

-- ✅ Covering index untuk SELECT yang sering
-- ✅ LIMIT + ORDER BY butuh index ORDER BY

-- CLI
EXPLAIN FORMAT=TREE SELECT ...;
EXPLAIN ANALYZE SELECT ...;
SHOW WARNINGS; -- lihat rewrite optimizer
```

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **Tidak EXPLAIN** | Tebak index, tidak efektif | EXPLAIN dulu |
| **Fungsi di WHERE** | Index tidak kepakai | Sargable |
| **`SELECT *` padahal butuh 2 kolom** | Tidak covering, I/O besar | Sebut kolom |
| **OFFSET besar tanpa cursor** | `LIMIT 100000,20` scan 100k | Cursor `WHERE id>last_id` |
| **Tidak pakai slow log** | Tidak tahu query lambat | Aktifkan `slow_query_log` |

**Troubleshooting:**

```sql
-- Cek index terpakai
EXPLAIN SELECT * FROM buku WHERE kategori='Teknologi' \G

-- Cek histogram
ANALYZE TABLE buku UPDATE HISTOGRAM ON kategori;

-- Paksa index jika optimizer salah pilih
SELECT * FROM buku FORCE INDEX (idx_kat) WHERE kategori='Teknologi';
```

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: EXPLAIN & Perbaiki

> Query `SELECT * FROM buku WHERE YEAR(created_at)=2024` lambat. EXPLAIN dan perbaiki.

**Jawaban:**

```sql
EXPLAIN SELECT * FROM buku WHERE YEAR(created_at)=2024;
-- type=ALL → scan

-- Perbaiki
CREATE INDEX idx_created ON buku(created_at);
EXPLAIN SELECT id, judul FROM buku WHERE created_at BETWEEN '2024-01-01' AND '2024-12-31';
-- type=range, key=idx_created → cepat
```

### Tantangan 2: Covering Index

> Buat query `SELECT kategori, harga FROM buku WHERE kategori='Teknologi'` jadi covering.

**Jawaban:**

```sql
CREATE INDEX idx_kat_harga ON buku(kategori, harga);
EXPLAIN SELECT kategori, harga FROM buku WHERE kategori='Teknologi';
-- Extra: Using index → covering, tidak akses tabel
-- Pembahasan: index (kategori,harga) simpan kedua kolom, query jawab dari index saja
```

