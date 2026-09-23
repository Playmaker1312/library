# 7. WHERE — Filter Data yang Dibutuhkan

> **Level 2 - Modul C** | **Durasi**: 2 hari | **Prasyarat**: 06 SELECT

---

## 1. Penjelasan Singkat & Konsep Dasar

`WHERE` = **filter baris**. Jika `SELECT` pilih *kolom*, `WHERE` pilih *baris* yang memenuhi kondisi.

Tanpa `WHERE`, kamu selalu dapat semua baris. Dengan `WHERE`, kamu minta "hanya buku Teknologi yang harga >150rb dan stok ada".

**Hubungan dengan materi sebelumnya**: 06 ambil kolom spesifik tapi masih semua baris. 07 sempurnakan: *kolom spesifik + baris spesifik* = query efisien.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Operator | Fungsi |
|----------|--------|
| `=, !=, <>, >, <, >=, <=` | Perbandingan nilai |
| `AND, OR, NOT` | Kombinasi logika (AND lebih kuat dari OR) |
| `BETWEEN a AND b` | Rentang inklusif |
| `IN (a,b,c)` | Daftar nilai — lebih singkat dari `OR` |
| `LIKE '%pola%'` | Pencarian pola (`%` 0+ char, `_` 1 char) |
| `IS NULL / IS NOT NULL` | Cek NULL (tidak bisa `= NULL`) |
| `CASE WHEN` | Logika kondisional di SELECT |

Masalah yang diselesaikan: tanpa WHERE, aplikasi tarik 1jt baris lalu filter di kode — lambat & boros memory. WHERE filter di DB (pakai index, Level 6).

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── Perbandingan ────────────────────────────────────────────────
SELECT judul, harga FROM buku WHERE kategori = 'Teknologi';
SELECT judul, harga FROM buku WHERE harga > 150000;
SELECT judul, stok FROM buku WHERE stok >= 5;
SELECT judul FROM buku WHERE kategori != 'Fiksi'; -- atau <>

-- ─── Logika AND/OR/NOT (pakai kurung!) ──────────────────────────
SELECT judul, harga, kategori FROM buku
WHERE kategori = 'Teknologi' AND harga > 150000;

SELECT judul, kategori FROM buku
WHERE kategori = 'Fiksi' OR kategori = 'Sains';

-- Kombinasi: kurung WAJIB karena AND dievaluasi dulu
SELECT judul, harga, kategori, stok FROM buku
WHERE (kategori = 'Teknologi' OR kategori = 'Sains')
  AND harga > 150000
  AND stok > 0;

-- Tanpa kurung → salah!
-- WHERE kategori='Teknologi' OR kategori='Sains' AND harga>150000
-- = kategori='Teknologi' OR (kategori='Sains' AND harga>150000) → tidak sesuai niat

-- ─── BETWEEN, IN ─────────────────────────────────────────────────
SELECT judul, harga FROM buku WHERE harga BETWEEN 100000 AND 200000; -- inklusif
SELECT judul, kategori FROM buku WHERE kategori IN ('Teknologi','Sains');
SELECT judul FROM buku WHERE kategori NOT IN ('Fiksi','Umum');

-- ─── LIKE ────────────────────────────────────────────────────────
SELECT judul FROM buku WHERE judul LIKE '%Code%';  -- mengandung Code
SELECT judul FROM buku WHERE judul LIKE 'The%';    -- diawali The
SELECT judul FROM buku WHERE judul LIKE '%Time';   -- diakhiri Time
SELECT pengarang FROM buku WHERE pengarang LIKE '%Martin%';

-- LIKE case-insensitive tergantung collation utf8mb4_unicode_ci

-- ─── IS NULL ─────────────────────────────────────────────────────
-- (setelah tambah kolom deskripsi yang NULL-able)
SELECT judul FROM buku WHERE deskripsi IS NULL;
SELECT judul FROM buku WHERE deskripsi IS NOT NULL;
-- JANGAN: WHERE deskripsi = NULL (selalu FALSE!)

-- ─── CASE WHEN ───────────────────────────────────────────────────
SELECT
    judul, harga,
    CASE
        WHEN harga > 200000 THEN 'Mahal'
        WHEN harga > 100000 THEN 'Sedang'
        ELSE 'Murah'
    END AS kategori_harga,
    CASE
        WHEN stok > 5 THEN 'Banyak'
        WHEN stok > 0 THEN 'Terbatas'
        ELSE 'Habis'
    END AS status_stok
FROM buku;

-- CASE di WHERE juga bisa
SELECT judul, harga FROM buku
WHERE CASE WHEN kategori='Teknologi' THEN harga > 150000 ELSE harga > 100000 END;
```

**Skenario Nyata (E-commerce):**

```sql
-- Filter produk untuk halaman katalog: kategori Elektronik, harga 1-5jt, stok >0, keyword "HP"
SELECT id, nama, harga, stok FROM produk
WHERE kategori_id = 3
  AND harga BETWEEN 1000000 AND 5000000
  AND stok > 0
  AND nama LIKE '%HP%'
  AND is_active = 1;
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| `WHERE kategori='Teknologi'` | **"Hanya rak Teknologi"** — pustakawan ke rak itu saja, tidak keliling semua lantai |
| `AND` | **"Teknologi DAN harga >150rb"** — dua syarat harus lolos |
| `OR` | **"Fiksi ATAU Sains"** — salah satu cukup |
| `BETWEEN` | **"Harga 100-200rb"** — buku di rentang itu |
| `IN ('Teknologi','Sains')` | **"Daftar belanja"** — sebut kategori yang mau, pustakawan ambil semua |
| `LIKE '%Code%'` | **"Judul mengandung Code"** — cari pakai kata kunci, seperti katalog |
| `CASE WHEN` | **"Beri label harga"** — pustakawan tempel stiker Mahal/Sedang/Murah |

> 💡 **Key Insight**: WHERE = *pustakawan filter di gudang*. Jangan bawa semua buku pulang lalu filter di rumah!

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | WHERE |
|---------|-------|
| Filter kategori/harga/stok | `WHERE kategori='Teknologi' AND stok>0` |
| Pencarian keyword | `WHERE judul LIKE '%code%'` (produksi: pakai FULLTEXT, Level 6) |
| Rentang tanggal/harga | `WHERE harga BETWEEN 100000 AND 200000` |
| Cek stok habis | `WHERE stok = 0` |
| Label kondisional | `CASE WHEN stok>5 THEN 'Banyak' ... END` |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ Pakai kurung untuk AND/OR
WHERE (a OR b) AND c

-- ✅ IN untuk banyak nilai (lebih readable & optimizer-friendly)
WHERE kategori IN ('Teknologi','Sains','Sejarah') -- vs OR berulang

-- ✅ BETWEEN inklusif, jelas
WHERE harga BETWEEN 100000 AND 200000

-- ✅ IS NULL, bukan = NULL
WHERE deleted_at IS NULL

-- ❌ Hindari fungsi di kolom WHERE jika ada index (bikin index tidak kepakai)
-- WHERE YEAR(created_at)=2024 -- buruk
WHERE created_at >= '2024-01-01' AND created_at < '2025-01-01' -- baik

-- ✅ LIKE '%keyword%' tidak pakai index — untuk search, pakai FULLTEXT di Level 6
```

**Keamanan:** Selalu pakai prepared statement `WHERE id = ?` — jangan concat string!

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **Lupa kurung `AND/OR`** | Hasil salah | Selalu `(A OR B) AND C` |
| **`WHERE col = NULL`** | Selalu 0 baris | `WHERE col IS NULL` |
| **`LIKE` tanpa `%`** | `LIKE 'Code'` = `= 'Code'` | `LIKE '%Code%'` |
| **Fungsi di kolom WHERE** | Full table scan | `WHERE created_at >= '2024-01-01'` |
| **Tidak pakai `WHERE` di UPDATE/DELETE** | Ubah/hapus semua baris! | Selalu test `SELECT` dulu |
| **SQL Injection `WHERE id = ' + input`** | Bobol DB | Prepared statement |

**Troubleshooting:**

```sql
-- Cek berapa baris terfilter
SELECT COUNT(*) FROM buku WHERE kategori='Teknologi';

-- Test WHERE sebelum UPDATE/DELETE
SELECT * FROM buku WHERE stok=0 AND tahun<2000; -- lihat dulu
-- DELETE FROM buku WHERE stok=0 AND tahun<2000;

EXPLAIN SELECT * FROM buku WHERE kategori='Teknologi'; -- cek pakai index?
```

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: Filter Kompleks

> Tampilkan `judul, kategori, harga, stok` untuk buku **(Teknologi ATAU Sains) DAN harga >150rb DAN stok >0**.

**Jawaban:**

```sql
SELECT judul, kategori, harga, stok FROM buku
WHERE (kategori = 'Teknologi' OR kategori = 'Sains')
  AND harga > 150000
  AND stok > 0;
```

**Pembahasan**: Kurung jamin OR dievaluasi dulu. Tanpa kurung, `AND harga>150000` hanya nempel ke `Sains`. Filter di DB jauh lebih cepat daripada filter di aplikasi.

---

### Tantangan 2: LIKE + CASE

> Cari buku dengan judul mengandung "The" ATAU pengarang mengandung "Martin", lalu beri label `status_harga` (Mahal >200k, Sedang >100k, else Murah).

**Jawaban:**

```sql
SELECT
    judul, pengarang, harga,
    CASE
        WHEN harga > 200000 THEN 'Mahal'
        WHEN harga > 100000 THEN 'Sedang'
        ELSE 'Murah'
    END AS status_harga
FROM buku
WHERE judul LIKE '%The%' OR pengarang LIKE '%Martin%';
```

**Pembahasan**: `LIKE '%The%'` cari substring. `CASE` di SELECT untuk label — berguna untuk badge di UI tanpa logika tambahan di frontend. Untuk search production, ganti dengan `FULLTEXT` + `MATCH ... AGAINST` (Level 6).

