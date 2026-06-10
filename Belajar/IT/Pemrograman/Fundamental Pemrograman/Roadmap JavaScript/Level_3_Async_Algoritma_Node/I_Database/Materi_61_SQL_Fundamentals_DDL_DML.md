# Materi 61: SQL Fundamentals — DDL & DML

---

## 1. Penjelasan

SQL (Structured Query Language) adalah bahasa standar untuk berkomunikasi dengan database relasional. Terbagi menjadi beberapa sub-bahasa:

- **DDL (Data Definition Language)**: Mendefinisikan struktur database — membuat, mengubah, menghapus tabel dan constraint.
  - Perintah: `CREATE`, `ALTER`, `DROP`, `TRUNCATE`
- **DML (Data Manipulation Language)**: Memanipulasi data di dalam tabel — menambah, membaca, mengubah, menghapus baris.
  - Perintah: `INSERT`, `SELECT`, `UPDATE`, `DELETE`

### Tipe Data SQL Umum
| Tipe | Keterangan | Contoh |
|------|-----------|--------|
| `INT` / `SERIAL` | Bilangan bulat | `id INT` |
| `VARCHAR(n)` | String dengan panjang max | `nama VARCHAR(100)` |
| `TEXT` | String panjang tak terbatas | `deskripsi TEXT` |
| `DATE` | Tanggal (YYYY-MM-DD) | `tanggal_lahir DATE` |
| `TIMESTAMP` | Tanggal + waktu | `created_at TIMESTAMP` |
| `BOOLEAN` | true/false | `aktif BOOLEAN` |
| `DECIMAL(p,s)` | Angka desimal presisi | `harga DECIMAL(10,2)` |

### Klausa Penting SELECT
- `WHERE` — filter baris
- `ORDER BY kolom ASC|DESC` — urutkan
- `LIMIT n` — batasi jumlah baris
- `OFFSET n` — lewati n baris (untuk pagination)

---

## 2. Fungsi

- **DDL**: Membangun dan memodifikasi kerangka penyimpanan data (schema).
- **DML**: Mengisi, mengambil, memperbarui, dan menghapus data di dalam kerangka tersebut.

---

## 3. Code

### DDL — Membuat database perpustakaan

```sql
-- Buat database
CREATE DATABASE perpustakaan_db;

-- Gunakan database
USE perpustakaan_db;

-- Tabel buku
CREATE TABLE buku (
    id       SERIAL PRIMARY KEY,
    judul    VARCHAR(200) NOT NULL,
    penulis  VARCHAR(100) NOT NULL,
    isbn     VARCHAR(20) UNIQUE,
    tahun    INT CHECK (tahun > 0),
    stok     INT DEFAULT 1 CHECK (stok >= 0),
    dibuat_pada TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tabel anggota
CREATE TABLE anggota (
    id       SERIAL PRIMARY KEY,
    nama     VARCHAR(100) NOT NULL,
    email    VARCHAR(100) UNIQUE NOT NULL,
    telepon  VARCHAR(20),
    alamat   TEXT
);

-- Tabel peminjaman
CREATE TABLE peminjaman (
    id       SERIAL PRIMARY KEY,
    buku_id  INT NOT NULL REFERENCES buku(id),
    anggota_id INT NOT NULL REFERENCES anggota(id),
    tgl_pinjam   DATE DEFAULT CURRENT_DATE,
    tgl_kembali  DATE,
    status   VARCHAR(20) DEFAULT 'dipinjam'
);
```

### DML — Memanipulasi data

```sql
-- INSERT: menambahkan data
INSERT INTO buku (judul, penulis, isbn, tahun, stok)
VALUES
    ('Laskar Pelangi', 'Andrea Hirata', '9789793062792', 2005, 3),
    ('Bumi Manusia', 'Pramoedya A. Toer', '9789799731223', 1980, 2),
    ('Ronggeng Dukuh Paruk', 'Ahmad Tohari', '9789799731216', 1982, 1);

INSERT INTO anggota (nama, email, telepon)
VALUES
    ('Siti Nuraini', 'siti@email.com', '08123456789'),
    ('Budi Santoso', 'budi@email.com', '08987654321');

INSERT INTO peminjaman (buku_id, anggota_id, tgl_pinjam, status)
VALUES (1, 1, '2026-06-01', 'dipinjam');

-- SELECT: membaca data
SELECT * FROM buku;
SELECT judul, penulis FROM buku WHERE tahun > 2000;
SELECT * FROM buku ORDER BY tahun DESC;
SELECT * FROM buku LIMIT 2 OFFSET 1;

-- UPDATE: mengubah data
UPDATE buku SET stok = stok - 1 WHERE id = 1;

-- DELETE: menghapus data
DELETE FROM anggota WHERE id = 99; -- hati-hati jika ada relasi
```

---

## 4. Analogi Rumah

| SQL | Analogi Rumah |
|-----|---------------|
| DDL `CREATE TABLE` | Membangun rak baru di gudang — menentukan ukuran, label, dan aturan isi. |
| DDL `ALTER TABLE` | Memodifikasi rak — menambah sekat baru, memperlebar laci. |
| DDL `DROP TABLE` | Membongkar rak dan membuangnya. |
| DML `INSERT` | Meletakkan barang ke rak yang sudah jadi. |
| DML `SELECT` | Mengambil barang dari rak — bisa filter, urut, ambil sebagian. |
| DML `UPDATE` | Mengganti barang lama dengan yang baru di posisi yang sama. |
| DML `DELETE` | Membuang barang dari rak. |
| `WHERE` | "Ambil barang yang warnanya merah saja." |
| `ORDER BY` | "Urutkan dari yang paling berat ke paling ringan." |
| `LIMIT` | "Ambil 5 barang teratas saja." |
| `OFFSET` | "Abaikan 10 barang pertama." |

---

## 5. Use Case

### Use Case 1: Menambah buku baru
```sql
INSERT INTO buku (judul, penulis, isbn, tahun, stok)
VALUES ('Atomic Habits', 'James Clear', '9780735211292', 2018, 4);
```
### Use Case 2: Cari buku berdasarkan judul
```sql
SELECT * FROM buku WHERE judul ILIKE '%laskar%';
```
### Use Case 3: Tampilkan 5 buku terbaru
```sql
SELECT judul, tahun FROM buku ORDER BY tahun DESC LIMIT 5;
```
### Use Case 4: Kembalikan buku (update status & stok)
```sql
UPDATE peminjaman SET status = 'dikembalikan', tgl_kembali = CURRENT_DATE
WHERE id = 1 AND status = 'dipinjam';
UPDATE buku SET stok = stok + 1 WHERE id = 1;
```

---

## 6. Kesalahan Umum

1. **Lupa PRIMARY KEY** — Duplikasi data tidak terdeteksi, baris tidak bisa diidentifikasi unik.
2. **Tidak pakai NOT NULL untuk field wajib** — Data masuk tanpa judul buku.
3. **Tidak pakai FOREIGN KEY** — Data peminjaman bisa mengacu ke buku_id yang tidak ada (orphan record).
4. **UPDATE tanpa WHERE** — Semua baris ke-update (bencana!). Selalu preview dengan SELECT dulu.
5. **DELETE tanpa WHERE** — Semua data terhapus.
6. **Salah tipe data** — `VARCHAR` untuk tanggal, atau `INT` untuk nomor telepon (0821... hilang angka depan).

---

## 7. Benang Merah

```
Materi 60 (Teori Database: SQL vs NoSQL)
    ↓  "Sekarang kita pahami SQL konkret"
Materi 61 (DDL & DML — Bangun & Isi Database)  ←── ANDA DI SINI
    ↓  "Selanjutnya bagaimana menggabungkan data dari banyak tabel?"
Materi 62 (JOIN, Agregasi, Subquery)
    ↓
Materi 63-65 (PostgreSQL → Prisma → Express)
```

---

## 8. Soal & Jawaban

### Soal 1
**Apa perbedaan DDL dan DML? Berikan masing-masing 2 contoh perintah.**

<details>
<summary>Jawaban</summary>
DDL (Data Definition Language) mengelola struktur database:
- `CREATE TABLE buku (...)` — membuat tabel
- `ALTER TABLE buku ADD COLUMN penerbit VARCHAR(100)` — menambah kolom

DML (Data Manipulation Language) mengelola isi data:
- `INSERT INTO buku VALUES (...)` — menambah data
- `SELECT * FROM buku` — membaca data
</details>

### Soal 2
**Jelaskan apa yang terjadi jika menjalankan: `DELETE FROM peminjaman;` (tanpa WHERE). Bagaimana cara mencegahnya?**

<details>
<summary>Jawaban</summary>
Semua baris dalam tabel peminjaman akan terhapus permanen. Cara mencegah:
1. Selalu preview dengan `SELECT * FROM peminjaman WHERE ...` sebelum DELETE.
2. Gunakan transaction: `BEGIN; DELETE ...; ROLLBACK;` jika salah.
3. Setel `sql_safe_updates = 1` di beberapa database (MySQL) yang mewajibkan WHERE.
</details>

### Soal 3
**Dalam analogi rumah, apa yang dimaksud dengan `LIMIT` dan `OFFSET` dalam konteks mengambil buku dari rak?**

<details>
<summary>Jawaban</summary>
- `LIMIT 3` = Ambil 3 buku teratas dari rak, sisanya tidak usah diambil.
- `OFFSET 5` = Abaikan 5 buku pertama, ambil sisanya.
Keduanya sering dipakai bersama untuk **pagination**: halaman 1 (LIMIT 10 OFFSET 0), halaman 2 (LIMIT 10 OFFSET 10), dst. Seperti membuka lemari yang isinya 100 buku, Anda hanya ambil 10 buku per sesi — mulai dari buku ke-0, lalu ke-10, lalu ke-20.
</details>
