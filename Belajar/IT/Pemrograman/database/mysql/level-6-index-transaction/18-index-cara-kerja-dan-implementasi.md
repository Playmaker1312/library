# 18. Index — Cara Kerja dan Implementasi

> **Level 6 - Modul J** | **Durasi**: 3 hari | **Prasyarat**: Level 5, 07 WHERE, 08 ORDER BY

---

## 1. Penjelasan Singkat & Konsep Dasar

Index = **daftar isi** untuk tabel. Tanpa index: *full table scan* baca semua baris. Dengan index (B-Tree): loncat ke halaman yang tepat.

Index percepat `WHERE, JOIN, ORDER BY` tapi perlambat `INSERT/UPDATE` (harus update index juga) + makan storage.

**Hubungan dengan materi sebelumnya**: Level 2-4 query sudah benar tapi lambat di 100k baris. 18 bikin query <100ms.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Jenis Index | Fungsi |
|-------------|--------|
| `PRIMARY KEY` | Clustered index, data terurut by PK |
| `INDEX (col)` | B-Tree untuk filter/sort |
| `UNIQUE INDEX` | Cegah duplikat + percepat |
| `COMPOSITE (col1,col2)` | Untuk filter multi-kolom (urutan penting: leftmost prefix) |
| `FULLTEXT (col)` | Pencarian teks `MATCH ... AGAINST` |

Masalah: `WHERE kategori='Teknologi'` tanpa index → scan 1jt baris. Dengan index → 10 baris via B-Tree.

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── Cek index ───────────────────────────────────────────────────
SHOW INDEX FROM buku;

-- ─── Buat index ──────────────────────────────────────────────────
CREATE INDEX idx_buku_kategori ON buku(kategori);
CREATE INDEX idx_buku_tahun ON buku(tahun);
-- Composite: urutan penting!
CREATE INDEX idx_buku_kategori_tahun ON buku(kategori, tahun);
-- Bisa dipakai: WHERE kategori='Teknologi' ✅
--               WHERE kategori='Teknologi' AND tahun>2000 ✅
--               WHERE tahun>2000 ❌ (tidak prefix kiri)

CREATE UNIQUE INDEX uk_buku_isbn ON buku(isbn);
CREATE FULLTEXT INDEX ft_buku_judul ON buku(judul, pengarang);

-- ─── Pakai ───────────────────────────────────────────────────────
-- Via WHERE/JOIN/ORDER BY otomatis kepakai jika selective
SELECT * FROM buku WHERE kategori='Teknologi'; -- pakai idx_buku_kategori
SELECT * FROM buku WHERE kategori='Teknologi' AND tahun>2000; -- pakai composite

-- Fulltext
SELECT judul, MATCH(judul) AGAINST ('clean code' IN NATURAL LANGUAGE MODE) AS skor
FROM buku WHERE MATCH(judul) AGAINST ('clean code' IN NATURAL LANGUAGE MODE)
ORDER BY skor DESC;

-- ─── Hapus ───────────────────────────────────────────────────────
DROP INDEX idx_buku_tahun ON buku;

-- ─── Covering index ──────────────────────────────────────────────
CREATE INDEX idx_cover ON buku(kategori, harga); -- query SELECT kategori,harga bisa dari index saja
EXPLAIN SELECT kategori, harga FROM buku WHERE kategori='Teknologi'; -- Using index
```

**Skenario Nyata (E-commerce):**

```sql
CREATE INDEX idx_produk_kat_harga ON produk(kategori_id, harga);
CREATE INDEX idx_orders_user_status ON orders(user_id, status);
-- Query katalog: WHERE kategori_id=3 ORDER BY harga → index komposit
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| **Tanpa index** | **Cari buku tanpa katalog** — telusuri 10k buku satu-satu |
| **Index B-Tree** | **Kartu katalog terurut** — binary search, langsung ke rak X |
| **Composite index** | **Katalog kategori → tahun** — urut kategori dulu, lalu tahun |
| **FULLTEXT** | **Index kata kunci** — cari "code" langsung ketemu |

> 💡 **Key Insight**: Index = *jalan pintas*. Bayar biaya tulis ekstra untuk baca super cepat.

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Buat Index Jika | Jangan Jika |
|-----------------|-------------|
| Kolom di `WHERE, JOIN, ORDER BY` sering | Kolom jarang difilter |
| `WHERE kategori='X'` selective (<10% baris) | `WHERE is_active=1` (50% baris) → tidak selective |
| `ORDER BY` sering | Tabel tulis-heavy (log) — index perlambat INSERT |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ Composite urutan: yang paling selective / sering duluan
-- ✅ EXPLAIN cek pakai index
EXPLAIN SELECT * FROM buku WHERE kategori='Teknologi';

-- ✅ Jangan index semua kolom — ukur via slow query log
-- ✅ Untuk low-cardinality (ENUM status), composite dengan kolom lain lebih berguna

-- CLI
SHOW INDEX FROM buku;
ANALYZE TABLE buku; -- update statistik
```

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **Index di kolom low-cardinality saja** | Tidak kepakai | Composite |
| **Fungsi di WHERE bikin index tidak kepakai** | `WHERE YEAR(tahun)=2024` → scan | `WHERE tahun=2024` |
| **LIKE '%code%'** | Tidak pakai B-Tree | FULLTEXT |
| **Terlalu banyak index** | INSERT lambat 5x | Hapus yang tidak dipakai (`performance_schema`) |
| **Lupa leftmost prefix** | `WHERE tahun=2024` tidak pakai `idx(kat,tahun)` | Buat `idx(tahun)` terpisah |

**Troubleshooting:**

```sql
EXPLAIN SELECT * FROM buku WHERE kategori='Teknologi';
-- type: ref / range = pakai index, ALL = scan!
EXPLAIN FORMAT=TREE SELECT ...;
```

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: Buat & Verifikasi Index

> Buat index untuk query `WHERE kategori='Teknologi' AND tahun>2000 ORDER BY harga`.

**Jawaban:**

```sql
CREATE INDEX idx_buku_kat_tahun_harga ON buku(kategori, tahun, harga);
EXPLAIN SELECT * FROM buku WHERE kategori='Teknologi' AND tahun>2000 ORDER BY harga;
-- Harus type: range, Extra: Using index condition
```

### Tantangan 2: FULLTEXT

> Cari judul mengandung "code" dengan relevansi.

**Jawaban:**

```sql
SELECT judul, MATCH(judul) AGAINST('code' IN BOOLEAN MODE) AS skor
FROM buku WHERE MATCH(judul) AGAINST('code' IN BOOLEAN MODE) ORDER BY skor DESC;
```

