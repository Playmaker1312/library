# 15. JOIN — Menghubungkan Tabel

> **Level 4 - Modul H** | **Durasi**: 3 hari | **Prasyarat**: 14 Foreign Key

---

## 1. Penjelasan Singkat & Konsep Dasar

FK hubungkan tabel di *storage*. `JOIN` hubungkan di *query* — jahit baris dari banyak tabel jadi satu hasil.

- `INNER JOIN` → hanya yang punya pasangan di kedua tabel.
- `LEFT JOIN` → semua dari kiri, plus pasangan jika ada (NULL jika tidak).
- `LEFT JOIN + WHERE right.id IS NULL` → yang *tidak* punya pasangan (anti-join).

**Hubungan dengan materi sebelumnya**: 14 buat rak terpisah. 15 adalah *cara tanya lintas rak*: "siapa pinjam buku apa?"

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Jenis JOIN | Fungsi |
|------------|--------|
| `INNER JOIN` | Pasangan wajib ada — peminjaman + buku + anggota |
| `LEFT JOIN` | Termasuk yang tidak punya pasangan — anggota yang belum pernah pinjam |
| `RIGHT JOIN` | Kebalikan LEFT (jarang, ganti urutan tabel saja) |
| `CROSS JOIN` | Semua kombinasi (M×N) — untuk matriks |
| `JOIN + GROUP BY` | Statistik per buku/anggota |
| `SELF JOIN` | Tabel join dirinya (hierarki) |

Tanpa JOIN, kamu harus query 3x lalu gabung di aplikasi — lambat & kompleks.

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── INNER JOIN: siapa pinjam apa ─────────────────────────────────
SELECT a.nama AS anggota, b.judul AS buku, p.tanggal_pinjam, p.status
FROM peminjaman p
INNER JOIN buku b ON p.buku_id=b.id
INNER JOIN anggota a ON p.anggota_id=a.id
ORDER BY p.tanggal_pinjam DESC;

-- ─── LEFT JOIN: semua anggota + jumlah pinjam ─────────────────────
SELECT a.nama, a.nomor_anggota, COUNT(p.id) AS jml_pinjam
FROM anggota a LEFT JOIN peminjaman p ON a.id=p.anggota_id
GROUP BY a.id, a.nama, a.nomor_anggota ORDER BY jml_pinjam DESC;

-- Anti-join: anggota yang BELUM pernah pinjam
SELECT a.nama, a.email FROM anggota a
LEFT JOIN peminjaman p ON a.id=p.anggota_id WHERE p.id IS NULL;

-- ─── JOIN + Agregasi ──────────────────────────────────────────────
SELECT b.judul, COUNT(p.id) AS total_dipinjam,
       SUM(CASE WHEN p.status='terlambat' THEN 1 ELSE 0 END) AS terlambat
FROM buku b LEFT JOIN peminjaman p ON b.id=p.buku_id
GROUP BY b.id, b.judul ORDER BY total_dipinjam DESC;

-- Top 5 buku paling sering dipinjam
SELECT b.judul, b.pengarang, COUNT(p.id) AS kali FROM buku b
JOIN peminjaman p ON b.id=p.buku_id GROUP BY b.id ORDER BY kali DESC LIMIT 5;

-- ─── CTE + JOIN: terlambat ───────────────────────────────────────
WITH terlambat AS (
    SELECT * FROM peminjaman WHERE status='terlambat' OR (status='dipinjam' AND batas_kembali < CURDATE())
)
SELECT a.nama, b.judul, t.batas_kembali, DATEDIFF(CURDATE(), t.batas_kembali) AS hari
FROM terlambat t JOIN anggota a ON t.anggota_id=a.id JOIN buku b ON t.buku_id=b.id
ORDER BY hari DESC;

-- ─── SELF JOIN (hierarki kategori) ────────────────────────────────
SELECT c.nama AS kategori, p.nama AS parent FROM kategori c
LEFT JOIN kategori p ON c.parent_id=p.id;
```

**Skenario Nyata (E-commerce):**

```sql
-- Order + user + produk
SELECT o.id, u.nama, p.nama AS produk, o.total, o.status
FROM orders o
JOIN users u ON o.user_id=u.id
JOIN produk p ON o.produk_id=p.id
WHERE o.status='paid';
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| `INNER JOIN` | **Jahit catatan pinjam + rak buku + rak anggota** — hanya yang lengkap |
| `LEFT JOIN` | **Daftar semua anggota**, plus catatan pinjam jika ada — anggota tanpa pinjam tetap muncul, kolom pinjam NULL |
| `LEFT JOIN + IS NULL` | **Cari anggota yang belum pernah pinjam** — yang kolom pinjamnya kosong |
| `JOIN + GROUP BY` | **Hitung per buku**: berapa kali dipinjam |

> 💡 **Key Insight**: JOIN = *jahit rak*. INNER = jahit ketat (harus ada), LEFT = jahit longgar (boleh kosong).

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | JOIN |
|---------|------|
| Laporan pinjam + detail buku | `INNER JOIN buku` |
| Cari yang belum pinjam | `LEFT JOIN ... WHERE p.id IS NULL` |
| Statistik per buku | `LEFT JOIN + GROUP BY` |
| Hierarki | `SELF JOIN` |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ Pakai alias pendek: p, b, a
FROM peminjaman p JOIN buku b ON p.buku_id=b.id

-- ✅ JOIN kolom harus di-index (FK sudah index)
-- ✅ LEFT JOIN untuk "termasuk yang tidak punya"
-- ❌ RIGHT JOIN → ganti urutan, pakai LEFT
-- ✅ Filter di WHERE, bukan di ON (kecuali LEFT JOIN conditional)
```

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **Lupa ON** | `CROSS JOIN` 100×100=10k baris! | Selalu `ON p.buku_id=b.id` |
| **INNER vs LEFT salah** | Kehilangan anggota tanpa pinjam | LEFT untuk "semua kiri" |
| **N+1 query di ORM** | 1 query anggota + N query pinjam = lambat | 1 JOIN |
| **JOIN tanpa index** | Full scan | Index FK |

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: INNER + LEFT

> (a) Siapa pinjam apa (INNER), (b) anggota belum pernah pinjam.

**Jawaban:**

```sql
-- (a)
SELECT a.nama, b.judul FROM peminjaman p
JOIN buku b ON p.buku_id=b.id JOIN anggota a ON p.anggota_id=a.id;
-- (b)
SELECT a.nama FROM anggota a LEFT JOIN peminjaman p ON a.id=p.anggota_id WHERE p.id IS NULL;
```

### Tantangan 2: Statistik

> Buku paling sering dipinjam top 3.

**Jawaban:**

```sql
SELECT b.judul, COUNT(p.id) AS kali FROM buku b
JOIN peminjaman p ON b.id=p.buku_id GROUP BY b.id ORDER BY kali DESC LIMIT 3;
```

