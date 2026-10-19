# 8. ORDER BY dan LIMIT — Urutkan dan Batasi Hasil

> **Level 2 - Modul C** | **Durasi**: 1 hari | **Prasyarat**: 07 WHERE

---

## 1. Penjelasan Singkat & Konsep Dasar

`WHERE` filter *baris mana*. `ORDER BY` + `LIMIT` atur *urutan* dan *jumlah* yang dikembalikan.

- `ORDER BY harga ASC/DESC` → urutkan (default ASC). Bisa multi-kolom: `ORDER BY kategori ASC, harga DESC`.
- `LIMIT n OFFSET m` → paginasi. `LIMIT 10 OFFSET 20` = lewati 20, ambil 10.
- Urutan eksekusi: `FROM → WHERE → ORDER BY → LIMIT` — filter dulu, baru urutkan, baru potong.

**Hubungan dengan materi sebelumnya**: 06-07 ambil data spesifik tapi urutan acak (tidak deterministik). 08 jamin urutan konsisten untuk UI (termurah dulu, terbaru dulu) dan paginasi.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Fitur | Fungsi |
|-------|--------|
| `ORDER BY col ASC/DESC` | Sorting — untuk ranking, daftar terurut |
| `ORDER BY col1, col2` | Multi-sort — misal kategori A-Z, lalu harga DESC per kategori |
| `ORDER BY ekspresi` | Urutkan by hitungan (`harga*stok DESC`) |
| `LIMIT n` | Batasi hasil — Top N query |
| `LIMIT n OFFSET m` | Pagination — halaman 2,3,... |
| `ORDER BY RAND()` | Random (hati-hati, lambat di data besar) |

Tanpa ORDER BY, urutan tidak terjamin — bisa berubah antar query!

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── ORDER BY ────────────────────────────────────────────────────
SELECT judul, harga FROM buku ORDER BY harga ASC;  -- termurah dulu
SELECT judul, harga FROM buku ORDER BY harga DESC; -- termahal dulu

-- Multi-kolom
SELECT judul, kategori, harga FROM buku
ORDER BY kategori ASC, harga DESC;
-- Kategori A-Z, dalam tiap kategori harga mahal dulu

-- By ekspresi
SELECT judul, harga, stok, harga*stok AS total FROM buku
ORDER BY total DESC;

-- By alias
SELECT judul, harga*stok AS total FROM buku ORDER BY total DESC;

-- ─── LIMIT ───────────────────────────────────────────────────────
SELECT judul, harga FROM buku ORDER BY harga DESC LIMIT 5; -- 5 termahal
SELECT judul FROM buku ORDER BY id ASC LIMIT 3;

-- ─── LIMIT + OFFSET (Pagination) ─────────────────────────────────
-- Halaman 1: 1-10
SELECT judul, harga FROM buku ORDER BY judul LIMIT 10 OFFSET 0;
-- Halaman 2: 11-20
SELECT judul, harga FROM buku ORDER BY judul LIMIT 10 OFFSET 10;
-- Halaman 3: 21-30
SELECT judul, harga FROM buku ORDER BY judul LIMIT 10 OFFSET 20;
-- Rumus: OFFSET = (halaman-1)*per_halaman
-- Shortcut: LIMIT 10,10 = LIMIT 10 OFFSET 10

-- ─── Kombinasi WHERE + ORDER BY + LIMIT ──────────────────────────
-- 5 buku Teknologi termurah yang tersedia
SELECT judul, harga, stok FROM buku
WHERE kategori='Teknologi' AND stok>0
ORDER BY harga ASC
LIMIT 5;

-- ─── ORDER BY RAND() (random) ────────────────────────────────────
SELECT judul FROM buku ORDER BY RAND() LIMIT 3; -- 3 buku acak (lambat jika 1jt baris)

-- ─── NULLS handling ──────────────────────────────────────────────
SELECT judul, isbn FROM buku ORDER BY isbn ASC; -- NULL di awal (tergantung)
-- Paksa NULL terakhir:
SELECT judul, isbn FROM buku ORDER BY isbn IS NULL, isbn ASC;
```

**Skenario Nyata (E-commerce — Pagination Katalog):**

```sql
-- API: GET /produk?page=2&per_page=20&sort=price_asc
SELECT id, nama, harga FROM produk
WHERE is_active=1
ORDER BY harga ASC
LIMIT 20 OFFSET 20; -- halaman 2

-- Cursor pagination (lebih cepat untuk data besar, Level 6)
SELECT id, nama, harga FROM produk
WHERE is_active=1 AND id > 100
ORDER BY id ASC LIMIT 20;
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| `ORDER BY harga DESC` | **Susun buku dari termahal ke termurah** di meja |
| `ORDER BY kategori, harga` | **Susun per rak kategori A-Z**, lalu dalam tiap rak susun harga DESC |
| `LIMIT 5` | **"Kasih 5 teratas saja"** — tidak bawa semua |
| `OFFSET 10` | **"Lewati 10 pertama, kasih 10 berikutnya"** — halaman 2 |
| Tanpa ORDER BY | **Tumpukan acak** — tiap minta, urutan beda! |

> 💡 **Key Insight**: Pagination = *bagi tumpukan terurut jadi halaman*. Tanpa urut, halaman 2 bisa duplikat halaman 1!

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | Query |
|---------|-------|
| Top N (termahal, terlaris) | `ORDER BY harga DESC LIMIT 5` |
| Pagination API | `ORDER BY id LIMIT 20 OFFSET 40` |
| Laporan terurut | `ORDER BY kategori, judul` |
| Random rekomendasi | `ORDER BY RAND() LIMIT 3` (kecil saja) |
| Leaderboard | `ORDER BY total DESC` |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ Selalu ORDER BY jika pakai LIMIT (deterministik)
SELECT judul FROM buku ORDER BY id LIMIT 10; -- baik
-- SELECT judul FROM buku LIMIT 10; -- buruk: urutan acak!

-- ✅ Index untuk ORDER BY (Level 6)
CREATE INDEX idx_harga ON buku(harga); -- percepat ORDER BY harga

-- ✅ Cursor pagination untuk data besar (OFFSET lambat di 100k+)
-- OFFSET 100000 harus scan 100k baris
WHERE id > last_id ORDER BY id LIMIT 20 -- lebih cepat

-- ❌ ORDER BY RAND() di tabel besar = full scan + filesort
-- Alternatif: ambil id random di app, lalu SELECT WHERE id IN (...)
```

**Best Practices:**
- Selalu `ORDER BY` + `LIMIT` berpasangan.
- Untuk pagination besar, pakai *keyset/cursor pagination* (`WHERE id > ?`), bukan OFFSET.
- `ORDER BY` kolom yang di-index = cepat (hindari `ORDER BY RAND()`).

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **LIMIT tanpa ORDER BY** | Hasil acak, tidak konsisten | Selalu `ORDER BY` |
| **OFFSET besar (100k)** | Lambat (harus scan & buang 100k) | Cursor pagination |
| **`ORDER BY` kolom tidak di-index** | Filesort lambat | Buat index |
| **`ORDER BY RAND()` di 1jt baris** | Sangat lambat | Random di app |
| **Lupa `ASC/DESC`** | Default ASC, mungkin salah | Explicit `DESC` untuk termahal |

**Troubleshooting:**

```sql
EXPLAIN SELECT * FROM buku ORDER BY harga DESC LIMIT 5;
-- Jika Extra: Using filesort → tambah INDEX(harga)

-- Cek pagination duplikat
SELECT id, judul FROM buku ORDER BY judul LIMIT 10 OFFSET 10;
-- Jika judul duplikat, pakai ORDER BY judul, id
```

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: Top N + Pagination

> (a) 5 buku termahal, (b) halaman 2 (10 per halaman) urut judul A-Z.

**Jawaban:**

```sql
-- (a)
SELECT judul, harga FROM buku ORDER BY harga DESC LIMIT 5;

-- (b) halaman 2: OFFSET 10
SELECT judul FROM buku ORDER BY judul ASC LIMIT 10 OFFSET 10;
-- Atau LIMIT 10,10
SELECT judul FROM buku ORDER BY judul ASC LIMIT 10,10;
```

**Pembahasan**: Top N = `ORDER BY ... LIMIT`. Pagination = `OFFSET = (page-1)*per_page`. Selalu `ORDER BY` untuk deterministik.

---

### Tantangan 2: Multi-sort + Ekspresi

> Tampilkan `judul, kategori, harga, stok, total_nilai (harga*stok)` urut `kategori ASC, total_nilai DESC`, ambil 3 teratas.

**Jawaban:**

```sql
SELECT judul, kategori, harga, stok, harga*stok AS total_nilai
FROM buku
ORDER BY kategori ASC, total_nilai DESC
LIMIT 3;
```

**Pembahasan**: Multi-sort: kategori dulu, lalu total_nilai per kategori. `ORDER BY ekspresi` hitung dulu baru urut — berguna untuk ranking nilai inventaris per kategori.

