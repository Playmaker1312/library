# 11. Subquery — Query Bersarang

> **Level 3 - Modul F** | **Durasi**: 2 hari | **Prasyarat**: 10 GROUP BY

---

## 1. Penjelasan Singkat & Konsep Dasar

Subquery = **query di dalam query**. Hasil query dalam jadi input query luar.

Contoh: "buku yang harganya di atas rata-rata" → butuh hitung `AVG` dulu (subquery), baru filter `WHERE harga > (avg)`.

Jenis: di `WHERE` (filter), di `FROM` (derived table), di `SELECT` (correlated — hati-hati lambat).

**Hubungan dengan materi sebelumnya**: GROUP BY hasilkan ringkasan. Subquery pakai ringkasan itu untuk filter/laporan lebih kompleks.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Jenis | Fungsi |
|-------|--------|
| `WHERE col > (SELECT ...)` | Filter by hasil agregat |
| `WHERE col IN (SELECT ...)` | Filter by daftar dari query lain |
| `FROM (SELECT ...) AS t` | Anggap hasil subquery sebagai tabel sementara |
| `SELECT (SELECT ... WHERE outer.col=inner.col)` | Correlated subquery — hitung per baris (lambat) |

Subquery selesaikan: "bandingkan tiap baris dengan statistik global" tanpa JOIN.

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── Subquery di WHERE ───────────────────────────────────────────
SELECT judul, harga FROM buku WHERE harga > (SELECT AVG(harga) FROM buku);
SELECT judul, harga FROM buku WHERE harga = (SELECT MAX(harga) FROM buku);

-- IN: kategori yang punya >2 buku
SELECT judul, kategori FROM buku
WHERE kategori IN (SELECT kategori FROM buku GROUP BY kategori HAVING COUNT(*)>2);

-- NOT IN (hati-hati NULL → pakai NOT EXISTS lebih aman)
SELECT DISTINCT kategori FROM buku
WHERE kategori NOT IN (SELECT DISTINCT kategori FROM buku WHERE stok=0);

-- ─── Subquery di FROM (Derived Table) ────────────────────────────
SELECT kategori, rata_rata_harga,
       CASE WHEN rata_rata_harga>150000 THEN 'Premium' ELSE 'Standar' END AS segmen
FROM (SELECT kategori, ROUND(AVG(harga),0) AS rata_rata_harga FROM buku GROUP BY kategori) AS stat
ORDER BY rata_rata_harga DESC;

-- ─── Correlated Subquery di SELECT ───────────────────────────────
SELECT judul, harga,
       (SELECT COUNT(*) FROM buku b2 WHERE b2.kategori=buku.kategori) AS jml_kategori,
       (SELECT ROUND(AVG(harga),0) FROM buku b3 WHERE b3.kategori=buku.kategori) AS avg_kategori
FROM buku ORDER BY kategori, harga DESC;
-- Lambat untuk 100k baris! Ganti JOIN/GROUP BY atau Window Function (materi 13)

-- ─── EXISTS (lebih efisien dari IN untuk besar) ──────────────────
SELECT judul FROM buku b
WHERE EXISTS (SELECT 1 FROM buku b2 WHERE b2.kategori=b.kategori GROUP BY kategori HAVING COUNT(*)>2);
```

**Skenario Nyata (E-commerce):**

```sql
-- Produk dengan harga di atas rata-rata kategorinya
SELECT nama, harga FROM produk p
WHERE harga > (SELECT AVG(harga) FROM produk WHERE kategori_id=p.kategori_id);
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| `WHERE harga > (SELECT AVG...)` | **Tanya pustakawan "rata-rata harga?" dulu**, baru minta buku di atas rata-rata |
| `FROM (SELECT ...)` | **Buat daftar ringkasan dulu**, lalu tanya dari daftar itu |
| `Correlated` | **Tanya per buku**: "berapa rata-rata rak ini?" — 1000x tanya = lambat |

> 💡 **Key Insight**: Subquery = *tanya bertahap*. CTE (materi 12) bikin tanya bertahap jadi lebih readable.

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | Teknik |
|---------|--------|
| Filter vs agregat global | `WHERE harga > (SELECT AVG...)` |
| Derived table untuk CASE | `FROM (SELECT ... ) AS stat` |
| Hindari jika bisa pakai JOIN | JOIN biasanya lebih cepat (optimizer) |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ IN vs EXISTS: EXISTS lebih cepat jika subquery besar
-- ✅ Hindari correlated subquery di SELECT untuk tabel besar — pakai JOIN
SELECT b.judul, s.avg_harga FROM buku b
JOIN (SELECT kategori, AVG(harga) AS avg_harga FROM buku GROUP BY kategori) s ON b.kategori=s.kategori;

-- ✅ Beri alias untuk derived table (wajib!)
FROM (SELECT ...) AS stat
```

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **Subquery return >1 baris untuk `=`** | `Subquery returns more than 1 row` | Pakai `IN` atau `LIMIT 1` |
| **Correlated lambat** | N+1 query di DB | Ganti JOIN + GROUP BY |
| **`NOT IN` dengan NULL** | Hasil 0 baris | Pakai `NOT EXISTS` |
| **Lupa alias derived table** | `Every derived table must have its own alias` | `AS t` |

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: Filter di Atas Rata-rata

> Tampilkan buku dengan harga di atas rata-rata keseluruhan, plus selisihnya.

**Jawaban:**

```sql
SELECT judul, harga, harga - (SELECT ROUND(AVG(harga),0) FROM buku) AS selisih
FROM buku WHERE harga > (SELECT AVG(harga) FROM buku) ORDER BY harga DESC;
```

### Tantangan 2: Derived Table

> Buat segmen kategori (Premium >150k, Standar) dari rata-rata harga per kategori.

**Jawaban:**

```sql
SELECT kategori, rata_rata_harga,
       CASE WHEN rata_rata_harga>150000 THEN 'Premium' ELSE 'Standar' END AS segmen
FROM (SELECT kategori, ROUND(AVG(harga),0) AS rata_rata_harga FROM buku GROUP BY kategori) AS s
ORDER BY rata_rata_harga DESC;
```

