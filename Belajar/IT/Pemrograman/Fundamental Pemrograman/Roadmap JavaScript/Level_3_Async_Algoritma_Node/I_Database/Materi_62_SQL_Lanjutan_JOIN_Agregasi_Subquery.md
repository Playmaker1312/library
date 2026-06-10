# Materi 62: SQL Lanjutan — JOIN, Agregasi & Subquery

---

## 1. Penjelasan

Setelah bisa membuat tabel dan CRUD dasar (Materi 61), sekarang saatnya **menghubungkan data antar tabel** dan **meringkas data**.

### Relasi Database
| Tipe Relasi | Contoh | Implementasi |
|-------------|--------|-------------|
| **1:1** (One-to-One) | User ↔ Profil | FOREIGN KEY + UNIQUE |
| **1:M** (One-to-Many) | Buku ↔ Peminjaman | FOREIGN KEY di sisi M |
| **M:M** (Many-to-Many) | Buku ↔ Kategori | Tabel penghubung (junction table) |

### JOIN
| Jenis JOIN | Hasil |
|------------|-------|
| `INNER JOIN` | Hanya baris yang cocok di kedua tabel |
| `LEFT JOIN` | Semua baris kiri + yang cocok dari kanan (NULL jika tidak cocok) |
| `RIGHT JOIN` | Semua baris kanan + yang cocok dari kiri |
| `FULL JOIN` | Semua baris kedua tabel |

### Agregasi
| Fungsi | Kegunaan |
|--------|----------|
| `COUNT(*)` | Jumlah baris |
| `SUM(kolom)` | Total nilai numerik |
| `AVG(kolom)` | Rata-rata |
| `MIN(kolom)` | Nilai minimum |
| `MAX(kolom)` | Nilai maksimum |
| `GROUP BY` | Kelompokkan baris berdasarkan kolom |
| `HAVING` | Filter setelah GROUP BY (seperti WHERE untuk grup) |

### Subquery
Query di dalam query — bisa di SELECT, FROM, atau WHERE.

---

## 2. Fungsi

- Menggabungkan data dari 2+ tabel berdasarkan relasi.
- Meringkas data dalam grup (laporan, statistik).
- Menyusun query bertingkat untuk logika kompleks.

---

## 3. Code

Setup awal (gunakan tabel dari Materi 61):

```sql
-- =============================================
-- JOIN: Buku yang sedang dipinjam + data anggota
-- =============================================

-- INNER JOIN: hanya peminjaman aktif dengan data buku & anggota
SELECT
    p.id AS peminjaman_id,
    b.judul AS buku,
    a.nama AS anggota,
    p.tgl_pinjam,
    p.status
FROM peminjaman p
INNER JOIN buku b ON p.buku_id = b.id
INNER JOIN anggota a ON p.anggota_id = a.id
WHERE p.status = 'dipinjam'
ORDER BY p.tgl_pinjam DESC;

-- LEFT JOIN: semua buku, termasuk yang tidak pernah dipinjam
SELECT b.judul, p.tgl_pinjam
FROM buku b
LEFT JOIN peminjaman p ON b.id = p.buku_id;

-- =============================================
-- AGREGASI: Laporan statistik
-- =============================================

-- Berapa kali setiap buku dipinjam?
SELECT
    b.judul,
    COUNT(p.id) AS total_dipinjam
FROM buku b
LEFT JOIN peminjaman p ON b.id = p.buku_id
GROUP BY b.id, b.judul
ORDER BY total_dipinjam DESC;

-- Anggota yang paling banyak meminjam (minimal 2 kali)
SELECT
    a.nama,
    COUNT(p.id) AS jumlah_pinjam
FROM anggota a
INNER JOIN peminjaman p ON a.id = p.anggota_id
GROUP BY a.id, a.nama
HAVING COUNT(p.id) >= 2
ORDER BY jumlah_pinjam DESC;

-- =============================================
-- SUBQUERY
-- =============================================

-- Buku yang stoknya di atas rata-rata
SELECT judul, stok
FROM buku
WHERE stok > (SELECT AVG(stok) FROM buku);

-- Anggota yang pernah meminjam buku 'Laskar Pelangi'
SELECT nama
FROM anggota
WHERE id IN (
    SELECT anggota_id FROM peminjaman
    WHERE buku_id = (SELECT id FROM buku WHERE judul = 'Laskar Pelangi')
);
```

---

## 4. Analogi Rumah

| Konsep SQL | Analogi Rumah |
|-----------|---------------|
| Relasi 1:M | Satu lemari (buku) bisa berisi banyak barang. Satu barang punya satu lemari tujuan. |
| Relasi M:M | Satu buku bisa di banyak kategori, satu kategori bisa berisi banyak buku — pakai buku catatan penghubung. |
| INNER JOIN | Cocokkan dua daftar inventaris dan ambil barang yang muncul di kedua daftar. |
| LEFT JOIN | Ambil semua barang dari gudang A, lalu cari label tambahan dari gudang B. Jika tidak ada, tulis NULL. |
| GROUP BY | Kelompokkan semua paku di satu tumpukan, semua baut di tumpukan lain, lalu hitung jumlahnya. |
| HAVING | Setelah dikelompokkan, ambil hanya kelompok yang jumlahnya > 10. |
| Subquery | "Cari rak yang berisi buku terlaris" — cari dulu buku terlarisnya, baru cari raknya. |

---

## 5. Use Case

### Use Case 1: Laporan buku yang sedang dipinjam
```sql
-- INNER JOIN 3 tabel
SELECT a.nama, b.judul, p.tgl_pinjam
FROM peminjaman p
JOIN anggota a ON p.anggota_id = a.id
JOIN buku b ON p.buku_id = b.id
WHERE p.status = 'dipinjam';
```

### Use Case 2: Cari anggota yang belum pernah meminjam
```sql
SELECT a.nama, a.email
FROM anggota a
LEFT JOIN peminjaman p ON a.id = p.anggota_id
WHERE p.id IS NULL;
```

### Use Case 3: Rata-rata durasi peminjaman per buku
```sql
SELECT b.judul,
       AVG(p.tgl_kembali - p.tgl_pinjam) AS rata_rata_hari
FROM peminjaman p
JOIN buku b ON p.buku_id = b.id
WHERE p.status = 'dikembalikan'
GROUP BY b.id, b.judul;
```

---

## 6. Kesalahan Umum

1. **Lupa JOIN condition (ON)** — Terjadi CROSS JOIN (produk kartesian), baris meledak tak terkendali. Selalu tulis `ON`.
2. **LEFT JOIN tapi pakai WHERE filter kolom kanan** — WHERE mengubah LEFT JOIN jadi INNER JOIN. Filter di ON clause.
3. **GROUP BY tanpa agregasi** — Ambigu, database error (kecuali MySQL dengan sql_mode longgar).
4. **Menggunakan HAVING tanpa GROUP BY** — HAVING untuk grup, WHERE untuk baris.
5. **Subquery tidak efisien** — Kadang JOIN lebih cepat dari subquery. Cek execution plan.

---

## 7. Benang Merah

```
Materi 60 (Teori DB) → Materi 61 (DDL/DML dasar)
                           ↓
              *Materi 62: JOIN, Agregasi, Subquery*  ←── ANDA DI SINI
                  ↓  "SQL sudah cukup kuat, sekarang waktunya implementasi"
        Materi 63: PostgreSQL Setup (database sungguhan)
                  ↓
        Materi 64: Prisma ORM (abstraksi SQL)
                  ↓
        Materi 65: Express + Prisma (API lengkap)
```

---

## 8. Soal & Jawaban

### Soal 1
**Apa perbedaan INNER JOIN dan LEFT JOIN? Berikan contoh kapan menggunakan masing-masing.**

<details>
<summary>Jawaban</summary>
- **INNER JOIN**: Hanya mengembalikan baris yang memiliki kecocokan di kedua tabel. Contoh: "Tampilkan daftar peminjaman beserta judul buku dan nama anggota" — hanya data peminjaman yang valid.

- **LEFT JOIN**: Mengembalikan semua baris dari tabel kiri, dan data dari tabel kanan jika cocok (NULL jika tidak). Contoh: "Tampilkan semua buku, termasuk yang tidak pernah dipinjam" — kita ingin buku dengan 0 peminjaman tetap muncul.
</details>

### Soal 2
**Apa bedanya WHERE dan HAVING? Kapan masing-masing digunakan?**

<details>
<summary>Jawaban</summary>
- **WHERE**: Memfilter baris **sebelum** GROUP BY. Digunakan untuk kondisi individual baris. Contoh: `WHERE stok > 0`
- **HAVING**: Memfilter grup **setelah** GROUP BY. Digunakan untuk kondisi agregat. Contoh: `HAVING COUNT(p.id) >= 2`

Urutan eksekusi: WHERE → GROUP BY → HAVING → ORDER BY → LIMIT.
</details>

### Soal 3
**Tulis query untuk menampilkan 3 buku paling banyak dipinjam beserta jumlahnya.**

<details>
<summary>Jawaban</summary>
```sql
SELECT b.judul, COUNT(p.id) AS jumlah_pinjam
FROM buku b
LEFT JOIN peminjaman p ON b.id = p.buku_id
GROUP BY b.id, b.judul
ORDER BY jumlah_pinjam DESC
LIMIT 3;
```
LEFT JOIN digunakan agar buku yang belum pernah dipinjam tetap muncul dengan jumlah 0.
</details>
