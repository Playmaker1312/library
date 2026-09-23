# 13. Window Functions — Agregasi Tanpa Kehilangan Detail

> **Level 3 - Modul G** | **Durasi**: 2 hari | **Prasyarat**: 12 CTE | **MySQL 8.0+**

---

## 1. Penjelasan Singkat & Konsep Dasar

`GROUP BY` *hancurkan* baris jadi ringkasan. Window Function **hitung agregat TANPA hancurkan baris** — tiap buku tetap tampil, plus info ranking/rata-rata kelompoknya.

Syntax: `FUNC() OVER (PARTITION BY kategori ORDER BY harga DESC)`

- `PARTITION BY` = reset perhitungan per kelompok (mirip GROUP BY tapi tidak collapse)
- `ORDER BY` = urutan di dalam partisi
- `ROWS BETWEEN` = frame (running total)

**Hubungan dengan materi sebelumnya**: CTE + GROUP BY butuh JOIN untuk bandingkan tiap buku vs avg kategori. Window Function lakukan dalam 1 scan tanpa JOIN.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Fungsi | Kegunaan |
|--------|----------|
| `ROW_NUMBER() OVER (...)` | Nomor urut unik per partisi |
| `RANK(), DENSE_RANK()` | Ranking dengan tie handling |
| `AVG() OVER (PARTITION BY ...)` | Rata-rata per kelompok tanpa GROUP BY |
| `LAG/LEAD` | Akses baris sebelum/sesudah |
| `SUM() OVER (ORDER BY ... ROWS ...)` | Running total |
| `NTILE(n)` | Bagi data jadi n kuartil |

Tanpa window: Top N per kategori butuh correlated subquery lambat. Dengan window: 1 CTE + `ROW_NUMBER`.

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── ROW_NUMBER per kategori ─────────────────────────────────────
SELECT kategori, judul, harga,
       ROW_NUMBER() OVER (PARTITION BY kategori ORDER BY harga DESC) AS ranking
FROM buku;

-- ─── RANK vs DENSE_RANK ──────────────────────────────────────────
SELECT judul, harga,
       RANK() OVER (ORDER BY harga DESC) AS rnk, -- 1,1,3 (lompat)
       DENSE_RANK() OVER (ORDER BY harga DESC) AS dense, -- 1,1,2
       ROW_NUMBER() OVER (ORDER BY harga DESC) AS rn -- 1,2,3
FROM buku;

-- ─── Bandingkan harga vs avg kategori (tanpa GROUP BY) ───────────
SELECT judul, kategori, harga,
       ROUND(AVG(harga) OVER (PARTITION BY kategori),0) AS avg_kat,
       harga - ROUND(AVG(harga) OVER (PARTITION BY kategori),0) AS selisih,
       CASE WHEN harga > AVG(harga) OVER (PARTITION BY kategori) THEN 'Di atas' ELSE 'Di bawah' END AS posisi
FROM buku ORDER BY kategori, harga DESC;

-- ─── LAG/LEAD ────────────────────────────────────────────────────
SELECT judul, harga,
       LAG(harga,1) OVER (ORDER BY harga) AS prev,
       LEAD(harga,1) OVER (ORDER BY harga) AS next,
       harga - LAG(harga,1) OVER (ORDER BY harga) AS diff
FROM buku;

-- ─── Running Total ───────────────────────────────────────────────
SELECT kategori, judul, stok,
       SUM(stok) OVER (PARTITION BY kategori ORDER BY judul ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running
FROM buku ORDER BY kategori, judul;

-- ─── NTILE ───────────────────────────────────────────────────────
SELECT judul, harga, NTILE(4) OVER (ORDER BY harga) AS kuartil FROM buku;

-- ─── Top N per kategori (Pola Paling Berguna!) ───────────────────
WITH ranked AS (
    SELECT kategori, judul, harga, ROW_NUMBER() OVER (PARTITION BY kategori ORDER BY harga DESC) AS rn FROM buku
)
SELECT kategori, judul, harga FROM ranked WHERE rn <=2 ORDER BY kategori, harga DESC;
```

**Skenario Nyata (E-commerce — Ranking Produk):**

```sql
-- 3 produk termurah per kategori
WITH r AS (
    SELECT kategori_id, nama, harga, ROW_NUMBER() OVER (PARTITION BY kategori_id ORDER BY harga ASC) AS rn FROM produk
)
SELECT * FROM r WHERE rn <=3;
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| `GROUP BY` | **Hancurkan rak**, sisa label kategori + hitungan |
| `Window` | **Biarkan buku tetap di rak**, tapi tempel stiker "ranking 1, avg 150k" di tiap buku |
| `PARTITION BY kategori` | **Reset hitungan tiap rak** — ranking mulai 1 lagi di rak baru |
| `LAG` | **Lihat buku sebelumnya** di rak terurut |
| `Running Total` | **Hitung akumulasi stok** dari buku pertama sampai buku ini |

> 💡 **Key Insight**: Window = *kaca pembesar* — lihat detail + konteks kelompok sekaligus.

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | Fungsi |
|---------|--------|
| Top N per kategori | `ROW_NUMBER() + CTE WHERE rn<=N` |
| Ranking leaderboard | `RANK()` |
| Bandingkan vs rata-rata | `AVG() OVER (PARTITION BY)` |
| Running total / tren | `SUM() OVER (ORDER BY ...)` |
| Bagi kuartil | `NTILE(4)` |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ PARTITION BY untuk per kelompok, kosong untuk global
AVG(harga) OVER () -- global
AVG(harga) OVER (PARTITION BY kategori) -- per kategori

-- ✅ Selalu ORDER BY di window jika ranking
ROW_NUMBER() OVER (PARTITION BY kategori ORDER BY harga DESC)

-- ✅ Untuk performa, index kolom PARTITION BY + ORDER BY
CREATE INDEX idx_kat_harga ON buku(kategori, harga DESC);
```

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **Lupa PARTITION BY** | Ranking global, bukan per kategori | Tambah `PARTITION BY kategori` |
| **RANK vs ROW_NUMBER salah** | Tie handling salah | RANK lompat, DENSE tidak, ROW_NUMBER unik |
| **Pakai GROUP BY + Window bersamaan salah** | `Mix of GROUP BY and window` error | Window setelah GROUP BY via CTE |
| **Window di WHERE** | `Window function not allowed in WHERE` | Pakai CTE lalu filter `WHERE rn<=N` |

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: Ranking + Selisih

> Tampilkan `judul, kategori, harga, avg_kategori, selisih, ranking` per kategori.

**Jawaban:**

```sql
SELECT judul, kategori, harga,
       ROUND(AVG(harga) OVER (PARTITION BY kategori),0) AS avg_kat,
       harga - ROUND(AVG(harga) OVER (PARTITION BY kategori),0) AS selisih,
       ROW_NUMBER() OVER (PARTITION BY kategori ORDER BY harga DESC) AS ranking
FROM buku ORDER BY kategori, ranking;
```

### Tantangan 2: Top 2 per Kategori

> Ambil 2 buku termahal per kategori.

**Jawaban:**

```sql
WITH ranked AS (
    SELECT kategori, judul, harga, ROW_NUMBER() OVER (PARTITION BY kategori ORDER BY harga DESC) AS rn FROM buku
)
SELECT kategori, judul, harga FROM ranked WHERE rn<=2 ORDER BY kategori, harga DESC;
```

**Pembahasan**: Pola emas untuk "Top N per grup" — jauh lebih cepat dari correlated subquery.

