# 10. GROUP BY — Mengelompokkan dan Menghitung

> **Level 3 - Modul E** | **Durasi**: 2 hari | **Prasyarat**: Level 2 (WHERE, ORDER BY)

---

## 1. Penjelasan Singkat & Konsep Dasar

`WHERE` filter *baris*. `GROUP BY` kelompokkan baris menjadi *ringkasan*.

Jika bos tanya "rata-rata harga per kategori?" — kamu tidak bisa jawab dengan `WHERE`. Kamu butuh: kelompokkan by `kategori`, lalu `AVG(harga)` per kelompok.

Alur: `FROM → WHERE (filter baris) → GROUP BY (kelompokkan) → HAVING (filter kelompok) → SELECT → ORDER BY → LIMIT`

**Hubungan dengan materi sebelumnya**: Level 2 query baris per baris. Level 3 naik ke *laporan*: dari 1000 baris jadi 5 baris ringkasan.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Fitur | Fungsi |
|-------|--------|
| `GROUP BY col` | Kelompokkan baris by kolom |
| `COUNT(*), SUM, AVG, MIN, MAX` | Agregat per kelompok |
| `HAVING` | Filter *kelompok* (WHERE filter baris, HAVING filter hasil GROUP BY) |
| `WITH ROLLUP` | Tambah baris total/subtotal |

Tanpa GROUP BY, laporan harus tarik semua data ke aplikasi lalu hitung manual — lambat & boros.

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── Agregat tanpa GROUP BY ──────────────────────────────────────
SELECT COUNT(*) AS total_buku, ROUND(AVG(harga),0) AS avg_harga,
       SUM(harga*stok) AS total_nilai FROM buku;

-- ─── GROUP BY per kategori ───────────────────────────────────────
SELECT kategori, COUNT(*) AS jumlah_buku FROM buku GROUP BY kategori;

-- Statistik lengkap per kategori
SELECT kategori, COUNT(*) AS jml, MIN(harga) AS termurah,
       MAX(harga) AS termahal, ROUND(AVG(harga),0) AS avg_harga,
       SUM(stok) AS total_stok
FROM buku GROUP BY kategori ORDER BY jml DESC;

-- Multi-kolom
SELECT kategori, tahun, COUNT(*) AS jml, ROUND(AVG(harga),0) AS avg_harga
FROM buku GROUP BY kategori, tahun ORDER BY kategori, tahun DESC;

-- ─── HAVING vs WHERE ─────────────────────────────────────────────
-- WHERE: filter baris SEBELUM group
-- HAVING: filter kelompok SETELAH group
SELECT kategori, COUNT(*) AS jml FROM buku GROUP BY kategori HAVING jml > 2;
SELECT kategori, ROUND(AVG(harga),0) AS avg_harga FROM buku GROUP BY kategori HAVING avg_harga > 150000;

-- Kombinasi
SELECT kategori, COUNT(*) AS tersedia, ROUND(AVG(harga),0) AS avg_harga
FROM buku WHERE stok>0 GROUP BY kategori HAVING avg_harga > 100000 ORDER BY avg_harga DESC;

-- ─── ROLLUP (subtotal + grand total) ─────────────────────────────
SELECT COALESCE(kategori,'SEMUA') AS kategori, COUNT(*) AS jml, SUM(stok) AS stok
FROM buku GROUP BY kategori WITH ROLLUP;

-- ─── GROUP_CONCAT ────────────────────────────────────────────────
SELECT kategori, GROUP_CONCAT(judul SEPARATOR ', ') AS daftar FROM buku GROUP BY kategori;
```

**Skenario Nyata (E-commerce):**

```sql
-- Laporan penjualan per kategori
SELECT k.nama AS kategori, COUNT(o.id) AS transaksi, SUM(o.total) AS omzet
FROM orders o JOIN produk p ON o.produk_id=p.id JOIN kategori k ON p.kategori_id=k.id
WHERE o.status='paid' GROUP BY k.nama HAVING omzet > 10000000 ORDER BY omzet DESC;
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| `GROUP BY kategori` | **Pisahkan buku per rak kategori** — Fiksi di meja A, Teknologi di meja B |
| `COUNT(*) per kelompok` | **Hitung buku per meja** |
| `WHERE stok>0` | **Buang buku stok 0** *sebelum* dihitung |
| `HAVING COUNT>2` | **Hanya meja dengan >2 buku** *setelah* dihitung |
| `WITH ROLLUP` | **Tambah meja "SEMUA"** — total semua rak |

> 💡 **Key Insight**: WHERE = saring buku sebelum dikelompokkan. HAVING = saring meja setelah dihitung.

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | Query |
|---------|-------|
| Laporan per kategori | `GROUP BY kategori` |
| Filter kelompok (hanya kategori >2 buku) | `HAVING COUNT>2` |
| Statistik inventaris | `SUM(harga*stok) GROUP BY kategori` |
| Dashboard | `GROUP BY DATE(created_at)` untuk tren harian |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ Semua kolom di SELECT harus di GROUP BY atau agregat
SELECT kategori, COUNT(*) FROM buku GROUP BY kategori; -- benar
-- SELECT kategori, judul, COUNT(*) FROM buku GROUP BY kategori; -- ERROR (judul tidak di GROUP BY)

-- ✅ HAVING untuk filter agregat, WHERE untuk filter baris
SELECT kategori, AVG(harga) AS avg_h FROM buku WHERE stok>0 GROUP BY kategori HAVING avg_h>100000;

-- ✅ ROLLUP untuk total
GROUP BY kategori WITH ROLLUP
```

**Best Practices:**
- Selalu `GROUP BY` semua kolom non-agregat di SELECT.
- Gunakan `HAVING` untuk filter setelah agregasi, jangan `WHERE AVG(...)`.
- Untuk performa, pastikan kolom GROUP BY di-index (Level 6).

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **SELECT kolom tidak di GROUP BY** | Error `ONLY_FULL_GROUP_BY` / hasil acak | Masukkan ke GROUP BY atau agregat |
| **Pakai WHERE untuk filter agregat** | `WHERE COUNT>2` error | Pakai `HAVING` |
| **Lupa beda WHERE vs HAVING** | Filter di waktu salah | WHERE sebelum, HAVING sesudah |
| **GROUP BY tanpa index** | Lambat di data besar | Index kolom GROUP BY |

**Troubleshooting:**

```sql
SET sql_mode = 'ONLY_FULL_GROUP_BY'; -- cegah GROUP BY ambigu
EXPLAIN SELECT kategori, COUNT(*) FROM buku GROUP BY kategori; -- cek index
```

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: Laporan Kategori

> Buat laporan `kategori, jumlah_buku, avg_harga, total_stok` untuk buku dengan `stok>0`, hanya kategori dengan `avg_harga>100000`.

**Jawaban:**

```sql
SELECT kategori, COUNT(*) AS jumlah_buku, ROUND(AVG(harga),0) AS avg_harga, SUM(stok) AS total_stok
FROM buku WHERE stok>0 GROUP BY kategori HAVING avg_harga>100000 ORDER BY avg_harga DESC;
```

**Pembahasan**: WHERE filter stok dulu, GROUP BY kelompokkan, HAVING filter avg_harga.

### Tantangan 2: ROLLUP

> Tambah grand total ke laporan jumlah buku per kategori.

**Jawaban:**

```sql
SELECT COALESCE(kategori,'TOTAL') AS kategori, COUNT(*) AS jml FROM buku GROUP BY kategori WITH ROLLUP;
```

**Pembahasan**: ROLLUP tambah baris NULL untuk total, COALESCE ganti NULL jadi 'TOTAL'.

