# Roadmap MySQL: Step-by-Step Menguasai Database dari Nol hingga Production

## Filosofi Roadmap Ini

> **"Database bukan sekadar tempat menyimpan data — database adalah fondasi kebenaran (single source of truth) dari seluruh aplikasi. Memahami MySQL secara mendalam membuat kamu bisa membangun aplikasi yang cepat, aman, dan scalable, bukan aplikasi yang 'jalan tapi lambat'"** — setiap konsep yang dipelajari ada alasannya, bukan sekadar hafal syntax.

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: sistem manajemen perpustakaan dari satu tabel sederhana → database relasional lengkap → optimized production database
- **Query Dulu, Desain Kemudian**: pahami cara _mengambil_ data sebelum mendesain _struktur_ data — ini membuat keputusan desain lebih intuitif
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **MySQL Modern**: fokus pada MySQL 8.x features (CTE, Window Functions, JSON, dll.)
- **Performa dari Hari Pertama**: bukan topik lanjutan, tapi mindset yang dibangun sejak query pertama

### Prasyarat Sebelum Memulai

text

```
Sebelum roadmap ini, pastikan sudah memahami:
├── Cara menggunakan komputer dan terminal/command prompt
├── Konsep dasar file dan folder
├── Logika dasar: AND, OR, NOT, perbandingan (>, <, =)
├── Spreadsheet dasar (baris, kolom, tabel) — ini analogi terbaik untuk database
└── Tidak perlu pengalaman programming sama sekali
```

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: "Database Pertama" — install, buat database, tabel pertama, data pertama
    ↓ (enhance)
Level 2: + SELECT, WHERE, ORDER → Query data buku dengan filter dan sorting
    ↓ (enhance)
Level 3: + GROUP BY, HAVING, Subquery → Statistik dan laporan perpustakaan
    ↓ (enhance)
Level 4: + JOIN, relasi antar tabel → Sistem peminjaman yang terhubung
    ↓ (enhance)
Level 5: + Database Design, Normalisasi → Schema yang benar dan efisien
    ↓ (enhance)
Level 6: + Index, Transaction, Optimization → Database yang cepat dan aman
    ↓ (enhance)
Level 7: + Stored Procedure, Trigger, Backup, Replication → Production-ready
```

---

## 🟢 LEVEL 1: FONDASI MYSQL (Minggu 1-3)

> **Tema**: _"Dari nol ke database pertama yang berisi data nyata"_  
> **Benang Merah**: Apa itu database → Install MySQL → Buat database dan tabel → Insert data → Query pertama  
> **Output**: Database perpustakaan dengan satu tabel buku berisi data nyata

---

### A. Memahami Database dan Setup

> 💡 **Mengapa dimulai di sini?** Sebelum menulis query, pahami dulu _apa itu database_ dan _bagaimana data disimpan_. Analogi spreadsheet akan sangat membantu.

text

```
Benang Merah Bagian A:
Spreadsheet: data dalam baris dan kolom →
Database: kumpulan tabel yang saling terhubung →
MySQL: software yang mengelola database (DBMS) →
SQL: bahasa untuk "berbicara" dengan database →
Install MySQL → buat database → buat tabel → isi data → ambil data
```

#### [[1. Apa Itu Database dan Mengapa MySQL]]

text

```
ANALOGI SPREADSHEET → DATABASE:
─────────────────────────────────────────────────────────────
Spreadsheet                    Database
─────────────────────────────  ─────────────────────────────
File Excel (.xlsx)          →  Database (perpustakaan_db)
Sheet "Buku"                →  Tabel (buku)
Sheet "Anggota"             →  Tabel (anggota)
Baris (row)                 →  Record / Row
Kolom (column)              →  Column / Field
Header kolom (Judul, Harga) →  Column name + Data type
Filter di Excel             →  WHERE clause
Sort di Excel               →  ORDER BY clause
VLOOKUP                     →  JOIN
Pivot Table                 →  GROUP BY

PERBEDAAN UTAMA:
├── Database bisa handle jutaan baris (Excel mulai lambat di 100K+)
├── Database mendukung banyak user bersamaan (Excel: satu user)
├── Database punya relasi antar tabel yang terjamin (VLOOKUP bisa salah)
├── Database punya transaction (all or nothing — Excel tidak punya)
└── Database bisa di-query dengan bahasa terstruktur (SQL)

MENGAPA MySQL?
├── Open source dan gratis
├── Paling populer untuk web development (LAMP stack)
├── Digunakan oleh: Facebook, YouTube, Netflix, Twitter
├── Dokumentasi melimpah, komunitas besar
├── Kompatibel dengan hampir semua bahasa pemrograman
└── Cukup powerful untuk sebagian besar use case
```

#### [[2. Instalasi MySQL dan Setup Environment]]

text

```
PILIHAN INSTALASI (pilih satu):

1. Laragon (Windows) — PALING MUDAH
   ├── Download: laragon.org
   ├── Install → Start All
   ├── MySQL sudah termasuk!
   ├── Akses phpMyAdmin: http://localhost/phpmyadmin
   └── Username: root, Password: (kosong)

2. XAMPP (Windows/macOS/Linux)
   ├── Download: apachefriends.org
   ├── Install → Start MySQL
   ├── Akses phpMyAdmin: http://localhost/phpmyadmin
   └── Username: root, Password: (kosong)

3. MySQL Installer (Windows) — untuk standalone
   ├── Download: dev.mysql.com/downloads/installer
   ├── Install MySQL Server + MySQL Workbench
   ├── Set root password saat install
   └── Buka MySQL Workbench untuk mulai

4. Homebrew (macOS)
   ├── brew install mysql
   ├── brew services start mysql
   └── mysql -u root

5. Docker (semua OS) — untuk yang sudah familiar
   ├── docker run --name mysql-perpustakaan \
   │   -e MYSQL_ROOT_PASSWORD=secret \
   │   -e MYSQL_DATABASE=perpustakaan_db \
   │   -p 3306:3306 -d mysql:8.0
   └── docker exec -it mysql-perpustakaan mysql -u root -p

VERIFIKASI INSTALASI:
Buka terminal/command prompt:
mysql --version
# mysql  Ver 8.0.xx ...

mysql -u root -p
# Masukkan password → muncul prompt mysql>
# Ketik: SELECT VERSION();
# Harus muncul versi MySQL 8.x
```

#### [[3. Membuat Database dan Tabel Pertama]]

SQL

```
-- ─── Koneksi ke MySQL ─────────────────────────────────────────────
-- Di terminal: mysql -u root -p

-- Lihat semua database yang ada
SHOW DATABASES;
-- +--------------------+
-- | Database           |
-- +--------------------+
-- | information_schema |  ← sistem, jangan diutak-atik
-- | mysql              |  ← sistem, jangan diutak-atik
-- | performance_schema |  ← sistem, jangan diutak-atik
-- | sys                |  ← sistem, jangan diutak-atik
-- +--------------------+

-- Buat database baru
CREATE DATABASE perpustakaan_db
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;
-- utf8mb4: mendukung semua karakter termasuk emoji 📚
-- utf8mb4_unicode_ci: sorting yang benar untuk banyak bahasa

-- Gunakan database yang baru dibuat
USE perpustakaan_db;

-- ─── Buat tabel pertama ───────────────────────────────────────────
CREATE TABLE buku (
    id          INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    judul       VARCHAR(200) NOT NULL,
    pengarang   VARCHAR(100) NOT NULL,
    tahun       YEAR NOT NULL,
    harga       DECIMAL(10, 2) NOT NULL DEFAULT 0.00,
    stok        INT UNSIGNED NOT NULL DEFAULT 0,
    kategori    VARCHAR(50) DEFAULT 'Umum',
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Penjelasan setiap bagian:
-- id: INT UNSIGNED = angka bulat positif (0 - 4.294.967.295)
--     AUTO_INCREMENT = otomatis naik setiap insert (1, 2, 3, ...)
--     PRIMARY KEY = identitas unik setiap baris, tidak boleh duplikat
--
-- judul: VARCHAR(200) = teks dengan panjang maksimal 200 karakter
--        NOT NULL = wajib diisi, tidak boleh kosong
--
-- harga: DECIMAL(10,2) = angka desimal, max 10 digit, 2 di belakang koma
--        DEFAULT 0.00 = jika tidak diisi, otomatis 0.00
--
-- stok: INT UNSIGNED = angka bulat positif (tidak bisa negatif)
--
-- created_at: TIMESTAMP = tanggal dan waktu otomatis saat record dibuat

-- Lihat struktur tabel
DESCRIBE buku;
-- +------------+------------------+------+-----+-------------------+
-- | Field      | Type             | Null | Key | Default           |
-- +------------+------------------+------+-----+-------------------+
-- | id         | int unsigned     | NO   | PRI | NULL              |
-- | judul      | varchar(200)     | NO   |     | NULL              |
-- | pengarang  | varchar(100)     | NO   |     | NULL              |
-- | tahun      | year             | NO   |     | NULL              |
-- | harga      | decimal(10,2)    | NO   |     | 0.00              |
-- | stok       | int unsigned     | NO   |     | 0                 |
-- | kategori   | varchar(50)      | YES  |     | Umum              |
-- | created_at | timestamp        | YES  |     | CURRENT_TIMESTAMP |
-- +------------+------------------+------+-----+-------------------+

-- Lihat semua tabel di database saat ini
SHOW TABLES;
```

#### [[4. Insert Data — Mengisi Tabel dengan Data]]

SQL

```
-- ─── Insert satu baris ────────────────────────────────────────────
INSERT INTO buku (judul, pengarang, tahun, harga, stok, kategori)
VALUES ('Clean Code', 'Robert C. Martin', 2008, 150000.00, 5, 'Teknologi');

-- ─── Insert banyak baris sekaligus ────────────────────────────────
INSERT INTO buku (judul, pengarang, tahun, harga, stok, kategori)
VALUES
    ('Laskar Pelangi', 'Andrea Hirata', 2005, 95000.00, 8, 'Fiksi'),
    ('The Pragmatic Programmer', 'Andrew Hunt', 1999, 175000.00, 3, 'Teknologi'),
    ('Cosmos', 'Carl Sagan', 1980, 200000.00, 2, 'Sains'),
    ('Bumi Manusia', 'Pramoedya Ananta Toer', 1980, 85000.00, 6, 'Fiksi'),
    ('Sapiens', 'Yuval Noah Harari', 2011, 135000.00, 4, 'Sejarah'),
    ('The Great Gatsby', 'F. Scott Fitzgerald', 1925, 110000.00, 0, 'Fiksi'),
    ('A Brief History of Time', 'Stephen Hawking', 1988, 180000.00, 1, 'Sains'),
    ('Design Patterns', 'Gang of Four', 1994, 220000.00, 2, 'Teknologi'),
    ('Filosofi Teras', 'Henry Manampiring', 2018, 79000.00, 10, 'Umum');

-- ─── Insert tanpa sebut kolom (tidak direkomendasikan!) ───────────
-- Urutan harus sesuai dengan urutan kolom di tabel
-- BERBAHAYA: jika struktur tabel berubah, query ini bisa salah!
INSERT INTO buku VALUES (NULL, 'Test Book', 'Author', 2024, 50000.00, 1, 'Umum', NOW());

-- ─── Insert dengan ON DUPLICATE KEY UPDATE ────────────────────────
-- Jika ISBN sudah ada, update stok. Jika belum, insert baru.
-- (akan berguna setelah kita tambah kolom ISBN yang UNIQUE)

-- Cek data yang sudah masuk
SELECT * FROM buku;
```

---

### B. Tipe Data MySQL — Memilih Tipe yang Tepat

> 💡 **Mengapa ini penting?** Memilih tipe data yang salah bisa menyebabkan: penyimpanan boros, query lambat, data tidak akurat, atau bug yang sulit dilacak. "Pakai VARCHAR(255) untuk semua" adalah kebiasaan buruk.

#### [[5. Tipe Data MySQL — Panduan Lengkap]]

SQL

```
-- ─── NUMERIC (Angka) ──────────────────────────────────────────────

-- Integer: angka bulat
-- TINYINT:    -128 sampai 127 (UNSIGNED: 0 sampai 255)
-- SMALLINT:   -32.768 sampai 32.767
-- MEDIUMINT:  -8.388.608 sampai 8.388.607
-- INT:        -2.147.483.648 sampai 2.147.483.647
-- BIGINT:     -9.223.372.036.854.775.808 sampai ... (sangat besar)

-- Aturan: pilih tipe TERKECIL yang cukup untuk data kamu
-- Umur manusia? TINYINT UNSIGNED (0-255) — cukup!
-- Jumlah penduduk kota? INT UNSIGNED
-- ID transaksi global? BIGINT UNSIGNED

-- Decimal: angka dengan koma yang AKURAT
-- DECIMAL(10,2) = maksimal 10 digit total, 2 di belakang koma
-- Contoh: 99999999.99
-- WAJIB untuk uang! Jangan pakai FLOAT/DOUBLE untuk uang!
-- FLOAT dan DOUBLE bisa menghasilkan pembulatan yang salah:
-- SELECT 0.1 + 0.2; → 0.30000000000000004 (salah!)
-- SELECT CAST(0.1 AS DECIMAL(5,2)) + CAST(0.2 AS DECIMAL(5,2)); → 0.30 (benar!)

-- ─── STRING (Teks) ────────────────────────────────────────────────

-- CHAR(n): panjang TETAP, selalu n karakter
-- Cocok untuk: kode negara ('ID', 'US'), jenis kelamin ('L', 'P'),
--              ISBN-13 (selalu 13 karakter)
-- CHAR(13) untuk ISBN: '9780132350884' — selalu 13 karakter, padding spasi jika kurang

-- VARCHAR(n): panjang VARIABEL, maksimal n karakter
-- Cocok untuk: nama, judul, email, alamat
-- VARCHAR(200) untuk judul: 'Clean Code' hanya pakai 10 karakter, bukan 200
-- Aturan: jangan selalu pakai 255! Sesuaikan dengan data nyata.
-- Nama orang di Indonesia jarang > 100 karakter → VARCHAR(100) cukup
-- Email maksimal 254 karakter menurut RFC → VARCHAR(255)

-- TEXT: teks panjang tanpa batas praktis (hingga 65.535 karakter)
-- Cocok untuk: deskripsi buku, artikel, komentar
-- TIDAK bisa di-index secara penuh (hanya prefix)
-- MEDIUMTEXT: hingga 16 juta karakter
-- LONGTEXT: hingga 4 miliar karakter

-- ENUM: pilihan yang sudah ditentukan
-- Cocok untuk: status, kategori yang tetap
-- ENUM('aktif', 'nonaktif', 'suspended')
-- Keuntungan: hemat storage, validasi di level database
-- Kerugian: sulit menambah pilihan baru (butuh ALTER TABLE)

-- ─── DATE DAN TIME ────────────────────────────────────────────────

-- DATE: tanggal saja (YYYY-MM-DD)
-- Contoh: '2024-01-15' — tanggal lahir, tanggal pinjam

-- TIME: waktu saja (HH:MM:SS)
-- Contoh: '14:30:00' — jam buka, jam tutup

-- DATETIME: tanggal + waktu (YYYY-MM-DD HH:MM:SS)
-- Contoh: '2024-01-15 14:30:00'
-- Range: 1000-01-01 sampai 9999-12-31
-- Tidak terpengaruh timezone

-- TIMESTAMP: tanggal + waktu yang terpengaruh timezone
-- Contoh: '2024-01-15 14:30:00'
-- Range: 1970-01-01 sampai 2038-01-19 (Year 2038 problem!)
-- Otomatis konversi ke UTC saat simpan, ke timezone server saat baca
-- Cocok untuk: created_at, updated_at, login_terakhir

-- YEAR: tahun saja (YYYY)
-- Contoh: 2024 — tahun terbit buku
-- Range: 1901 sampai 2155

-- ─── BOOLEAN ──────────────────────────────────────────────────────
-- MySQL tidak punya tipe BOOLEAN asli
-- BOOLEAN = TINYINT(1): 0 = false, 1 = true
-- Contoh: tersedia BOOLEAN DEFAULT TRUE

-- ─── JSON (MySQL 5.7+) ──────────────────────────────────────────
-- Menyimpan data JSON yang bisa di-query
-- Cocok untuk: metadata yang fleksibel, konfigurasi, atribut dinamis
-- Contoh: metadata JSON → {"halaman": 350, "bahasa": "Indonesia", "edisi": 2}
```

---

### 🏗️ Checkpoint Level 1

text

```
✅ Checklist sebelum lanjut ke Level 2:

PEMAHAMAN:
├── Bisa jelaskan perbedaan database, tabel, baris, dan kolom
├── Bisa jelaskan perbedaan VARCHAR vs CHAR vs TEXT
├── Bisa jelaskan mengapa DECIMAL untuk uang, bukan FLOAT
├── Bisa jelaskan perbedaan DATETIME vs TIMESTAMP
└── Bisa jelaskan apa itu PRIMARY KEY dan AUTO_INCREMENT

PRAKTIK:
├── MySQL terinstall dan bisa diakses via terminal atau GUI
├── Database perpustakaan_db sudah dibuat dengan utf8mb4
├── Tabel buku sudah dibuat dengan kolom yang tepat
├── Minimal 10 baris data sudah di-insert
└── SELECT * FROM buku menampilkan semua data dengan benar

KEBIASAAN:
├── Selalu gunakan utf8mb4 untuk database baru
├── Selalu tentukan PRIMARY KEY untuk setiap tabel
├── Selalu gunakan DECIMAL untuk uang, bukan FLOAT/DOUBLE
├── Jangan gunakan SELECT * di production (nanti dibahas)
└── Beri nama kolom yang deskriptif (bukan col1, col2)

SQL: CREATE DATABASE, CREATE TABLE, INSERT INTO, SELECT *
```

---

## 🔵 LEVEL 2: QUERY DASAR — MENGAMBIL DAN MEMANIPULASI DATA (Minggu 3-6)

> **Tema**: _"Dari melihat semua data ke mengambil tepat data yang dibutuhkan"_  
> **Benang Merah**: SELECT * (Level 1) → SELECT kolom tertentu → WHERE untuk filter → ORDER BY untuk sort → UPDATE dan DELETE untuk ubah data  
> **Output**: Query yang bisa menjawab pertanyaan bisnis tentang perpustakaan

---

### C. SELECT — Mengambil Data dari Tabel

> 💡 **Mengapa SELECT paling penting?** 90% interaksi aplikasi dengan database adalah membaca data (SELECT). Menulis query SELECT yang efisien adalah skill yang paling sering dipakai sepanjang karir kamu.

text

```
Benang Merah Bagian C:
SELECT * mengambil semua (Level 1) →
Tapi kita jarang butuh semua kolom dan semua baris →
SELECT kolom tertentu: hemat bandwidth dan memori →
WHERE: filter baris yang dibutuhkan →
ORDER BY: urutkan hasil →
LIMIT: batasi jumlah baris →
Kombinasi ketiganya: query yang efisien
```

#### [[6. SELECT — Dari Semua ke Spesifik]]

SQL

```
-- ─── SELECT semua kolom (hindari di production!) ──────────────────
SELECT * FROM buku;
-- Masalah: mengambil kolom yang tidak dibutuhkan = boros memori dan bandwidth
-- Jika tabel punya 50 kolom dan kamu hanya butuh 3, ini sangat tidak efisien

-- ─── SELECT kolom tertentu (SELALU lakukan ini) ──────────────────
SELECT judul, pengarang, harga FROM buku;

-- ─── SELECT dengan alias (AS) — nama kolom di hasil ──────────────
SELECT
    judul AS judul_buku,
    pengarang AS nama_pengarang,
    harga AS harga_rupiah,
    stok AS jumlah_stok
FROM buku;

-- ─── SELECT dengan ekspresi ───────────────────────────────────────
SELECT
    judul,
    harga,
    stok,
    harga * stok AS total_nilai_stok,
    harga * 0.9 AS harga_diskon_10_persen
FROM buku;

-- ─── SELECT DISTINCT — hapus duplikat ─────────────────────────────
-- Berapa kategori unik yang ada?
SELECT DISTINCT kategori FROM buku;
-- Hasil: Teknologi, Fiksi, Sains, Sejarah, Umum

-- ─── SELECT dengan fungsi built-in ────────────────────────────────
SELECT
    COUNT(*) AS total_buku,
    MIN(harga) AS harga_termurah,
    MAX(harga) AS harga_termahal,
    AVG(harga) AS rata_rata_harga,
    SUM(stok) AS total_stok
FROM buku;

-- ─── SELECT dengan fungsi string ──────────────────────────────────
SELECT
    UPPER(judul) AS judul_besar,
    LOWER(pengarang) AS pengarang_kecil,
    LENGTH(judul) AS panjang_judul,
    CONCAT(judul, ' oleh ', pengarang) AS info_lengkap,
    SUBSTRING(judul, 1, 20) AS judul_pendek
FROM buku;

-- ─── SELECT dengan fungsi tanggal ─────────────────────────────────
SELECT
    judul,
    created_at,
    DATE(created_at) AS tanggal_saja,
    YEAR(created_at) AS tahun_dibuat,
    DATEDIFF(NOW(), created_at) AS hari_sejak_dibuat,
    DATE_FORMAT(created_at, '%d/%m/%Y %H:%i') AS format_indonesia
FROM buku;
```

#### [[7. WHERE — Filter Data yang Dibutuhkan]]

SQL

```
-- ─── Operator perbandingan ────────────────────────────────────────

-- Sama dengan
SELECT judul, harga FROM buku WHERE kategori = 'Teknologi';

-- Tidak sama dengan
SELECT judul, harga FROM buku WHERE kategori != 'Fiksi';
-- Atau: WHERE kategori <> 'Fiksi'

-- Lebih besar / lebih kecil
SELECT judul, harga FROM buku WHERE harga > 150000;
SELECT judul, harga FROM buku WHERE harga <= 100000;
SELECT judul, stok FROM buku WHERE stok >= 5;

-- ─── Operator logika ──────────────────────────────────────────────

-- AND: semua kondisi harus benar
SELECT judul, harga, kategori
FROM buku
WHERE kategori = 'Teknologi' AND harga > 150000;

-- OR: salah satu kondisi cukup
SELECT judul, kategori
FROM buku
WHERE kategori = 'Fiksi' OR kategori = 'Sains';

-- NOT: kebalikan
SELECT judul, kategori
FROM buku
WHERE NOT kategori = 'Umum';

-- Kombinasi AND dan OR (gunakan tanda kurung!)
SELECT judul, harga, kategori, stok
FROM buku
WHERE (kategori = 'Teknologi' OR kategori = 'Sains')
  AND harga > 150000
  AND stok > 0;
-- TANPA tanda kurung, hasilnya bisa salah karena AND dievaluasi lebih dulu dari OR!

-- ─── Operator khusus ──────────────────────────────────────────────

-- BETWEEN: rentang nilai (inclusive)
SELECT judul, harga FROM buku WHERE harga BETWEEN 100000 AND 200000;
-- Sama dengan: WHERE harga >= 100000 AND harga <= 200000

-- IN: daftar nilai
SELECT judul, kategori FROM buku
WHERE kategori IN ('Teknologi', 'Sains');
-- Sama dengan: WHERE kategori = 'Teknologi' OR kategori = 'Sains'
-- Tapi lebih singkat dan lebih cepat untuk banyak nilai

-- NOT IN
SELECT judul, kategori FROM buku
WHERE kategori NOT IN ('Fiksi', 'Umum');

-- LIKE: pencarian pola
-- % = wildcard (0 atau lebih karakter)
-- _ = wildcard (tepat 1 karakter)

-- Judul yang mengandung kata "Code"
SELECT judul FROM buku WHERE judul LIKE '%Code%';

-- Judul yang dimulai dengan "The"
SELECT judul FROM buku WHERE judul LIKE 'The%';

-- Judul yang diakhiri dengan "Time"
SELECT judul FROM buku WHERE judul LIKE '%Time';

-- Pengarang yang nama depannya 1 huruf (inisial)
SELECT pengarang FROM buku WHERE pengarang LIKE '_ %';

-- IS NULL / IS NOT NULL
-- (belum relevan karena semua kolom NOT NULL, tapi penting nanti)
SELECT judul FROM buku WHERE deskripsi IS NULL;
SELECT judul FROM buku WHERE deskripsi IS NOT NULL;

-- ─── CASE WHEN: kondisi di dalam SELECT ───────────────────────────
SELECT
    judul,
    harga,
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
```

#### [[8. ORDER BY dan LIMIT — Urutkan dan Batasi Hasil]]

SQL

```
-- ─── ORDER BY: urutkan hasil ──────────────────────────────────────

-- Urutkan berdasarkan harga, termurah dulu (ASC = ascending, default)
SELECT judul, harga FROM buku ORDER BY harga ASC;

-- Urutkan berdasarkan harga, termahal dulu (DESC = descending)
SELECT judul, harga FROM buku ORDER BY harga DESC;

-- Urutkan berdasarkan banyak kolom
SELECT judul, kategori, harga
FROM buku
ORDER BY kategori ASC, harga DESC;
-- Pertama urutkan kategori A-Z, lalu dalam setiap kategori urutkan harga Z-A

-- Urutkan berdasarkan ekspresi
SELECT judul, harga, stok, harga * stok AS total_nilai
FROM buku
ORDER BY total_nilai DESC;

-- ─── LIMIT: batasi jumlah baris ───────────────────────────────────

-- Ambil 5 buku termahal
SELECT judul, harga FROM buku ORDER BY harga DESC LIMIT 5;

-- Ambil 3 buku pertama (berdasarkan ID)
SELECT judul, harga FROM buku ORDER BY id ASC LIMIT 3;

-- ─── LIMIT dengan OFFSET: untuk pagination ────────────────────────
-- Halaman 1: baris 1-10
SELECT judul, harga FROM buku ORDER BY judul LIMIT 10 OFFSET 0;

-- Halaman 2: baris 11-20
SELECT judul, harga FROM buku ORDER BY judul LIMIT 10 OFFSET 10;

-- Halaman 3: baris 21-30
SELECT judul, harga FROM buku ORDER BY judul LIMIT 10 OFFSET 20;

-- Rumus: OFFSET = (halaman - 1) * per_halaman
-- Shortcut: LIMIT 10, 10 sama dengan LIMIT 10 OFFSET 10

-- ─── Kombinasi WHERE + ORDER BY + LIMIT ───────────────────────────
-- "Tampilkan 5 buku Teknologi termurah yang masih tersedia"
SELECT judul, harga, stok
FROM buku
WHERE kategori = 'Teknologi' AND stok > 0
ORDER BY harga ASC
LIMIT 5;
```

---

### D. UPDATE dan DELETE — Mengubah dan Menghapus Data

> 💡 **Peringatan**: UPDATE dan DELETE tanpa WHERE akan mengubah/menghapus SEMUA baris! Selalu double-check WHERE clause sebelum jalankan.

#### [[9. UPDATE dan DELETE — Dengan Keamanan]]

SQL

```
-- ─── UPDATE: ubah data ────────────────────────────────────────────

-- Update satu baris (berdasarkan PRIMARY KEY — paling aman)
UPDATE buku
SET stok = 7
WHERE id = 1;

-- Update beberapa kolom sekaligus
UPDATE buku
SET harga = 160000.00,
    stok = 10,
    kategori = 'Teknologi'
WHERE id = 1;

-- Update dengan ekspresi
UPDATE buku
SET stok = stok - 1    -- kurangi stok 1
WHERE id = 1 AND stok > 0;  -- hanya jika stok masih ada!

-- Update dengan kondisi
UPDATE buku
SET harga = harga * 0.9    -- diskon 10%
WHERE kategori = 'Fiksi' AND tahun < 2010;

-- ⚠️ BAHAYA: UPDATE tanpa WHERE = ubah SEMUA baris!
-- UPDATE buku SET harga = 0;  -- SEMUA buku jadi gratis!
-- SELALU test dengan SELECT dulu:
SELECT * FROM buku WHERE kategori = 'Fiksi' AND tahun < 2010;
-- Pastikan baris yang terpilih adalah yang ingin di-update

-- ─── DELETE: hapus data ───────────────────────────────────────────

-- Hapus satu baris
DELETE FROM buku WHERE id = 11;

-- Hapus berdasarkan kondisi
DELETE FROM buku WHERE stok = 0 AND tahun < 2000;

-- ⚠️ BAHAYA: DELETE tanpa WHERE = hapus SEMUA baris!
-- DELETE FROM buku;  -- SEMUA buku hilang!
-- SELALU test dengan SELECT dulu:
SELECT * FROM buku WHERE stok = 0 AND tahun < 2000;

-- TRUNCATE: hapus semua baris dengan cepat (reset AUTO_INCREMENT)
-- TRUNCATE TABLE buku;  -- HATI-HATI: tidak bisa di-rollback!
-- Gunakan ini hanya saat development untuk reset data

-- ─── SOFT DELETE: alternatif yang lebih aman ──────────────────────
-- Daripada benar-benar hapus, tandai sebagai "dihapus"
-- Tambah kolom deleted_at:
ALTER TABLE buku ADD COLUMN deleted_at TIMESTAMP NULL DEFAULT NULL;

-- "Hapus" dengan soft delete:
UPDATE buku SET deleted_at = NOW() WHERE id = 5;

-- Query hanya data yang "aktif":
SELECT * FROM buku WHERE deleted_at IS NULL;

-- "Restore" data yang di-soft-delete:
UPDATE buku SET deleted_at = NULL WHERE id = 5;
```

---

### 🏗️ Checkpoint Level 2

text

```
✅ Checklist sebelum lanjut ke Level 3:

QUERY YANG HARUS BISA DITULIS:
├── SELECT kolom tertentu (bukan SELECT *)
├── WHERE dengan AND, OR, NOT, BETWEEN, IN, LIKE
├── ORDER BY dengan banyak kolom dan ASC/DESC
├── LIMIT dan OFFSET untuk pagination
├── UPDATE dengan WHERE yang spesifik
├── DELETE dengan WHERE yang spesifik
└── CASE WHEN untuk logika kondisional di SELECT

LATIHAN (jawab dengan query):
├── Berapa buku yang harganya di atas 150.000?
├── Tampilkan 3 buku termurah yang masih tersedia
├── Tampilkan semua buku Fiksi dan Sains, urutkan berdasarkan tahun
├── Update stok buku ID 3 menjadi 5
├── "Hapus" (soft delete) buku yang stoknya 0
└── Tampilkan judul dan status stok (Banyak/Terbatas/Habis)

KEAMANAN:
├── SELALU test UPDATE/DELETE dengan SELECT dulu
├── SELALU gunakan WHERE di UPDATE dan DELETE
├── Gunakan soft delete untuk data yang penting
└── Jangan pernah jalankan DELETE tanpa WHERE di production

SQL: SELECT, WHERE, ORDER BY, LIMIT, UPDATE, DELETE, CASE WHEN
```

---

## 🟡 LEVEL 3: QUERY LANJUTAN — AGREGASI DAN SUBQUERY (Minggu 6-9)

> **Tema**: _"Dari mengambil data mentah ke menghasilkan laporan dan statistik"_  
> **Benang Merah**: Query baris per baris (Level 2) → GROUP BY untuk agregasi → HAVING untuk filter grup → Subquery untuk query di dalam query → Window Functions untuk analisis canggih  
> **Output**: Laporan statistik perpustakaan: buku per kategori, rata-rata harga, tren peminjaman

---

### E. GROUP BY dan HAVING — Agregasi Data

> 💡 **Mengapa GROUP BY?** Bayangkan kamu punya 10.000 buku dan bos bertanya "berapa rata-rata harga per kategori?". Kamu tidak bisa jawab dengan WHERE saja — kamu perlu _mengelompokkan_ data lalu _menghitung_ setiap kelompok.

text

```
Benang Merah Bagian E:
WHERE: filter BARIS sebelum dikelompokkan →
GROUP BY: kelompokkan baris berdasarkan kolom →
Fungsi agregat: COUNT, SUM, AVG, MIN, MAX per kelompok →
HAVING: filter KELOMPOK setelah agregasi →
Urutan eksekusi: FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
```

#### [[10. GROUP BY — Mengelompokkan dan Menghitung]]

SQL

```
-- ─── Fungsi Agregat Dasar ─────────────────────────────────────────

-- Berapa total buku?
SELECT COUNT(*) AS total_buku FROM buku;

-- Berapa total buku yang tersedia?
SELECT COUNT(*) AS buku_tersedia FROM buku WHERE stok > 0;

-- Berapa rata-rata harga semua buku?
SELECT ROUND(AVG(harga), 0) AS rata_rata_harga FROM buku;

-- Berapa total nilai inventaris?
SELECT SUM(harga * stok) AS total_nilai_inventaris FROM buku;

-- ─── GROUP BY: agregasi per kelompok ──────────────────────────────

-- Berapa buku per kategori?
SELECT
    kategori,
    COUNT(*) AS jumlah_buku
FROM buku
GROUP BY kategori;
-- +------------+-------------+
-- | kategori   | jumlah_buku |
-- +------------+-------------+
-- | Fiksi      | 3           |
-- | Sains      | 2           |
-- | Sejarah    | 1           |
-- | Teknologi  | 3           |
-- | Umum       | 1           |
-- +------------+-------------+

-- Statistik lengkap per kategori
SELECT
    kategori,
    COUNT(*) AS jumlah_buku,
    MIN(harga) AS harga_termurah,
    MAX(harga) AS harga_termahal,
    ROUND(AVG(harga), 0) AS rata_rata_harga,
    SUM(stok) AS total_stok
FROM buku
GROUP BY kategori
ORDER BY jumlah_buku DESC;

-- Group by banyak kolom
SELECT
    kategori,
    tahun,
    COUNT(*) AS jumlah_buku,
    ROUND(AVG(harga), 0) AS rata_rata_harga
FROM buku
GROUP BY kategori, tahun
ORDER BY kategori, tahun DESC;

-- ─── HAVING: filter kelompok (bukan baris!) ───────────────────────
-- Perbedaan penting:
-- WHERE: filter BARIS SEBELUM grouping
-- HAVING: filter KELOMPOK SETELAH grouping

-- Kategori yang punya lebih dari 2 buku
SELECT
    kategori,
    COUNT(*) AS jumlah_buku
FROM buku
GROUP BY kategori
HAVING jumlah_buku > 2;

-- Kategori dengan rata-rata harga di atas 150.000
SELECT
    kategori,
    COUNT(*) AS jumlah_buku,
    ROUND(AVG(harga), 0) AS rata_rata_harga
FROM buku
GROUP BY kategori
HAVING rata_rata_harga > 150000
ORDER BY rata_rata_harga DESC;

-- Kombinasi WHERE + GROUP BY + HAVING
-- "Rata-rata harga per kategori untuk buku yang masih tersedia,
--  hanya tampilkan kategori dengan rata-rata > 100.000"
SELECT
    kategori,
    COUNT(*) AS buku_tersedia,
    ROUND(AVG(harga), 0) AS rata_rata_harga
FROM buku
WHERE stok > 0                    -- filter baris DULU (sebelum group)
GROUP BY kategori
HAVING rata_rata_harga > 100000   -- filter kelompok SETELAH group
ORDER BY rata_rata_harga DESC;

-- ─── GROUP BY dengan ROLLUP: subtotal dan grand total ─────────────
SELECT
    COALESCE(kategori, 'SEMUA KATEGORI') AS kategori,
    COUNT(*) AS jumlah_buku,
    SUM(stok) AS total_stok
FROM buku
GROUP BY kategori WITH ROLLUP;
-- Menambahkan baris total di akhir hasil
```

---

### F. Subquery dan CTE — Query di Dalam Query

> 💡 **Mengapa Subquery?** Terkadang kamu butuh hasil dari satu query sebagai input untuk query lain. Contoh: "tampilkan buku yang harganya di atas rata-rata". Kamu perlu hitung rata-rata dulu, baru filter.

#### [[11. Subquery — Query Bersarang]]

SQL

```
-- ─── Subquery di WHERE ────────────────────────────────────────────

-- Buku yang harganya di atas rata-rata
SELECT judul, harga
FROM buku
WHERE harga > (SELECT AVG(harga) FROM buku);

-- Buku yang harganya sama dengan buku termahal
SELECT judul, harga
FROM buku
WHERE harga = (SELECT MAX(harga) FROM buku);

-- Buku yang stoknya di bawah rata-rata stok per kategori
SELECT judul, kategori, stok
FROM buku
WHERE stok < (SELECT AVG(stok) FROM buku);

-- Subquery dengan IN
-- Buku yang kategorinya memiliki lebih dari 2 judul
SELECT judul, kategori
FROM buku
WHERE kategori IN (
    SELECT kategori
    FROM buku
    GROUP BY kategori
    HAVING COUNT(*) > 2
);

-- Subquery dengan NOT IN
-- Kategori yang TIDAK memiliki buku dengan stok 0
SELECT DISTINCT kategori
FROM buku
WHERE kategori NOT IN (
    SELECT DISTINCT kategori
    FROM buku
    WHERE stok = 0
);

-- ─── Subquery di FROM (Derived Table) ─────────────────────────────
-- Anggap hasil subquery sebagai "tabel sementara"

SELECT
    kategori,
    rata_rata_harga,
    CASE
        WHEN rata_rata_harga > 150000 THEN 'Premium'
        ELSE 'Standar'
    END AS segmen
FROM (
    SELECT
        kategori,
        ROUND(AVG(harga), 0) AS rata_rata_harga
    FROM buku
    GROUP BY kategori
) AS statistik_kategori
ORDER BY rata_rata_harga DESC;

-- ─── Subquery di SELECT (Correlated Subquery) ─────────────────────
-- Subquery yang bergantung pada baris dari query luar

SELECT
    judul,
    harga,
    (SELECT COUNT(*) FROM buku AS b2 WHERE b2.kategori = buku.kategori) AS buku_dalam_kategori,
    (SELECT ROUND(AVG(harga), 0) FROM buku AS b3 WHERE b3.kategori = buku.kategori) AS rata_rata_kategori
FROM buku
ORDER BY kategori, harga DESC;
-- HATI-HATI: correlated subquery bisa lambat untuk data besar!
```

#### [[12. CTE (Common Table Expression) — Query yang Lebih Readable]]

SQL

```
-- CTE: beri nama pada subquery agar bisa dipakai berulang
-- Lebih readable dari subquery bersarang
-- Tersedia di MySQL 8.0+

-- ─── CTE Dasar ────────────────────────────────────────────────────

WITH statistik_kategori AS (
    SELECT
        kategori,
        COUNT(*) AS jumlah_buku,
        ROUND(AVG(harga), 0) AS rata_rata_harga,
        SUM(stok) AS total_stok
    FROM buku
    GROUP BY kategori
)
SELECT
    kategori,
    jumlah_buku,
    rata_rata_harga,
    total_stok,
    CASE
        WHEN rata_rata_harga > 150000 THEN 'Premium'
        WHEN rata_rata_harga > 100000 THEN 'Menengah'
        ELSE 'Ekonomis'
    END AS segmen_harga
FROM statistik_kategori
ORDER BY rata_rata_harga DESC;

-- ─── Multiple CTE ─────────────────────────────────────────────────

WITH
buku_tersedia AS (
    SELECT * FROM buku WHERE stok > 0 AND deleted_at IS NULL
),
statistik AS (
    SELECT
        kategori,
        COUNT(*) AS jumlah,
        ROUND(AVG(harga), 0) AS avg_harga
    FROM buku_tersedia
    GROUP BY kategori
),
kategori_populer AS (
    SELECT kategori FROM statistik WHERE jumlah >= 2
)
SELECT s.*
FROM statistik s
INNER JOIN kategori_populer kp ON s.kategori = kp.kategori
ORDER BY s.avg_harga DESC;

-- ─── CTE Rekursif (untuk data hierarki) ───────────────────────────
-- Contoh: kategori induk → sub-kategori → sub-sub-kategori
-- (akan lebih relevan setelah kita buat tabel kategori hierarki)

WITH RECURSIVE kategori_tree AS (
    -- Base case: kategori root (parent_id IS NULL)
    SELECT id, nama, parent_id, 0 AS level, CAST(nama AS CHAR(200)) AS path
    FROM kategori
    WHERE parent_id IS NULL

    UNION ALL

    -- Recursive case: anak dari kategori yang sudah ditemukan
    SELECT k.id, k.nama, k.parent_id, kt.level + 1,
           CONCAT(kt.path, ' → ', k.nama)
    FROM kategori k
    INNER JOIN kategori_tree kt ON k.parent_id = kt.id
)
SELECT * FROM kategori_tree ORDER BY path;
```

---

### G. Window Functions — Analisis Canggih (MySQL 8.0+)

> 💡 **Mengapa Window Functions?** GROUP BY "menghancurkan" baris individual — kamu kehilangan detail. Window Functions memungkinkan kamu menghitung agregat _tanpa_ kehilangan baris individual. Ini sangat powerful untuk ranking, running total, dan perbandingan.

#### [[13. Window Functions — Agregasi Tanpa Kehilangan Detail]]

SQL

```
-- ─── ROW_NUMBER: nomor urut per kelompok ──────────────────────────

-- Ranking buku termahal per kategori
SELECT
    kategori,
    judul,
    harga,
    ROW_NUMBER() OVER (
        PARTITION BY kategori
        ORDER BY harga DESC
    ) AS ranking_harga
FROM buku;
-- PARTITION BY: "reset" nomor urut setiap kategori baru
-- ORDER BY: urutan di dalam setiap partisi

-- ─── RANK dan DENSE_RANK ─────────────────────────────────────────

SELECT
    judul,
    harga,
    RANK() OVER (ORDER BY harga DESC) AS rank_harga,
    DENSE_RANK() OVER (ORDER BY harga DESC) AS dense_rank_harga,
    ROW_NUMBER() OVER (ORDER BY harga DESC) AS row_num
FROM buku;
-- Perbedaan jika ada harga yang sama (misal 150000, 150000, 100000):
-- RANK:       1, 1, 3  (lompat)
-- DENSE_RANK: 1, 1, 2  (tidak lompat)
-- ROW_NUMBER: 1, 2, 3  (selalu unik)

-- ─── Agregat sebagai Window Function ──────────────────────────────

-- Bandingkan harga setiap buku dengan rata-rata kategorinya
SELECT
    judul,
    kategori,
    harga,
    ROUND(AVG(harga) OVER (PARTITION BY kategori), 0) AS avg_kategori,
    harga - ROUND(AVG(harga) OVER (PARTITION BY kategori), 0) AS selisih_dari_rata_rata,
    CASE
        WHEN harga > AVG(harga) OVER (PARTITION BY kategori) THEN 'Di atas rata-rata'
        WHEN harga < AVG(harga) OVER (PARTITION BY kategori) THEN 'Di bawah rata-rata'
        ELSE 'Sama dengan rata-rata'
    END AS posisi_harga
FROM buku
ORDER BY kategori, harga DESC;

-- ─── LAG dan LEAD: akses baris sebelumnya/selanjutnya ─────────────

-- Bandingkan harga buku dengan buku sebelumnya (dalam urutan harga)
SELECT
    judul,
    harga,
    LAG(harga, 1) OVER (ORDER BY harga) AS harga_sebelumnya,
    LEAD(harga, 1) OVER (ORDER BY harga) AS harga_selanjutnya,
    harga - LAG(harga, 1) OVER (ORDER BY harga) AS selisih
FROM buku;

-- ─── Running Total ────────────────────────────────────────────────

-- Akumulasi stok per kategori
SELECT
    kategori,
    judul,
    stok,
    SUM(stok) OVER (
        PARTITION BY kategori
        ORDER BY judul
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_total_stok
FROM buku
ORDER BY kategori, judul;

-- ─── NTILE: bagi data menjadi N kelompok yang sama besar ──────────

-- Bagi buku menjadi 4 kuartil berdasarkan harga
SELECT
    judul,
    harga,
    NTILE(4) OVER (ORDER BY harga) AS kuartil_harga
FROM buku;
-- Kuartil 1 = 25% termurah, Kuartil 4 = 25% termahal

-- ─── Top N per kategori (pola yang sangat berguna!) ──────────────

-- 2 buku termahal per kategori
WITH ranked AS (
    SELECT
        kategori,
        judul,
        harga,
        ROW_NUMBER() OVER (PARTITION BY kategori ORDER BY harga DESC) AS rn
    FROM buku
)
SELECT kategori, judul, harga
FROM ranked
WHERE rn <= 2
ORDER BY kategori, harga DESC;
```

---

### 🏗️ Checkpoint Level 3

text

```
✅ Checklist sebelum lanjut ke Level 4:

QUERY YANG HARUS BISA DITULIS:
├── GROUP BY dengan COUNT, SUM, AVG, MIN, MAX
├── HAVING untuk filter kelompok
├── Subquery di WHERE, FROM, dan SELECT
├── CTE untuk query yang kompleks dan readable
├── Window Functions: ROW_NUMBER, RANK, LAG, LEAD
└── Top N per kategori menggunakan CTE + ROW_NUMBER

LATIHAN (laporan perpustakaan):
├── Laporan jumlah buku dan rata-rata harga per kategori
├── Daftar 3 buku termahal per kategori
├── Perbandingan harga setiap buku dengan rata-rata kategorinya
├── Kategori mana yang total nilai stoknya paling tinggi?
└── Ranking buku berdasarkan harga keseluruhan

PEMAHAMAN:
├── Bisa jelaskan perbedaan WHERE vs HAVING
├── Bisa jelaskan urutan eksekusi SQL (FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT)
├── Bisa jelaskan perbedaan RANK vs DENSE_RANK vs ROW_NUMBER
└── Bisa jelaskan kapan pakai subquery vs CTE vs JOIN

SQL: GROUP BY, HAVING, Subquery, CTE (WITH), Window Functions
```

---

## 🟠 LEVEL 4: JOIN DAN RELASI ANTAR TABEL (Minggu 9-13)

> **Tema**: _"Dari satu tabel ke sistem tabel yang saling terhubung"_  
> **Benang Merah**: Semua data di satu tabel (Level 1-3) → dunia nyata butuh banyak tabel → JOIN menghubungkan tabel → Foreign Key menjaga integritas → Sistem perpustakaan yang terhubung  
> **Output**: Database perpustakaan lengkap dengan tabel buku, anggota, peminjaman, dan query yang menghubungkan semuanya

---

### H. Membuat Tabel Terkait dan JOIN

> 💡 **Mengapa JOIN?** Di dunia nyata, data tidak pernah di satu tabel. Perpustakaan punya buku, anggota, peminjaman, kategori, pengarang — semua saling terhubung. JOIN adalah cara SQL "menjahit" tabel-tabel ini menjadi satu hasil yang bermakna.

text

```
Benang Merah Bagian H:
Satu tabel: semua data buku (Level 1-3) →
Tapi di mana data anggota? Di mana data peminjaman? →
Buat tabel baru: anggota, peminjaman →
Foreign Key: pastikan peminjaman merujuk ke buku dan anggota yang ADA →
JOIN: gabungkan data dari banyak tabel dalam satu query
```

#### [[14. Tabel Baru dengan Foreign Key]]

SQL

```
-- ─── Tabel Anggota ────────────────────────────────────────────────
CREATE TABLE anggota (
    id              INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nomor_anggota   VARCHAR(20) UNIQUE NOT NULL,
    nama            VARCHAR(100) NOT NULL,
    email           VARCHAR(150) UNIQUE NOT NULL,
    telepon         VARCHAR(15),
    tanggal_daftar  DATE NOT NULL,
    status          ENUM('aktif', 'nonaktif', 'suspended') DEFAULT 'aktif',
    created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,

    INDEX idx_nama (nama),
    INDEX idx_status (status)
);

-- ─── Tabel Peminjaman ─────────────────────────────────────────────
CREATE TABLE peminjaman (
    id              INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    buku_id         INT UNSIGNED NOT NULL,
    anggota_id      INT UNSIGNED NOT NULL,
    tanggal_pinjam  DATE NOT NULL,
    batas_kembali   DATE NOT NULL,
    tanggal_kembali DATE NULL,
    denda           DECIMAL(10, 2) DEFAULT 0.00,
    status          ENUM('dipinjam', 'dikembalikan', 'terlambat') DEFAULT 'dipinjam',
    created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    -- FOREIGN KEY: memastikan data yang dirujuk ADA di tabel lain
    FOREIGN KEY (buku_id)
        REFERENCES buku(id)
        ON UPDATE CASCADE      -- jika id buku berubah, ikut berubah
        ON DELETE RESTRICT,    -- tidak bisa hapus buku yang masih dipinjam

    FOREIGN KEY (anggota_id)
        REFERENCES anggota(id)
        ON UPDATE CASCADE
        ON DELETE RESTRICT,

    INDEX idx_anggota_status (anggota_id, status),
    INDEX idx_tanggal_pinjam (tanggal_pinjam),
    INDEX idx_buku (buku_id)
);

-- ─── Insert data anggota ──────────────────────────────────────────
INSERT INTO anggota (nomor_anggota, nama, email, telepon, tanggal_daftar)
VALUES
    ('ANG-2024-001', 'Budi Santoso', 'budi@email.com', '081234567890', '2024-01-15'),
    ('ANG-2024-002', 'Siti Rahayu', 'siti@email.com', '081234567891', '2024-02-20'),
    ('ANG-2024-003', 'Andi Wijaya', 'andi@email.com', '081234567892', '2024-03-10'),
    ('ANG-2024-004', 'Dewi Lestari', 'dewi@email.com', '081234567893', '2024-01-05'),
    ('ANG-2024-005', 'Rudi Hartono', 'rudi@email.com', '081234567894', '2024-04-01');

-- ─── Insert data peminjaman ───────────────────────────────────────
INSERT INTO peminjaman (buku_id, anggota_id, tanggal_pinjam, batas_kembali, tanggal_kembali, status)
VALUES
    (1, 1, '2024-06-01', '2024-06-15', '2024-06-14', 'dikembalikan'),
    (2, 2, '2024-06-05', '2024-06-19', NULL, 'dipinjam'),
    (3, 1, '2024-06-10', '2024-06-24', NULL, 'dipinjam'),
    (4, 3, '2024-05-20', '2024-06-03', '2024-06-10', 'terlambat'),
    (5, 4, '2024-06-12', '2024-06-26', NULL, 'dipinjam'),
    (1, 2, '2024-06-15', '2024-06-29', NULL, 'dipinjam'),
    (6, 5, '2024-05-01', '2024-05-15', '2024-05-14', 'dikembalikan'),
    (8, 3, '2024-06-18', '2024-07-02', NULL, 'dipinjam');
```

#### [[15. JOIN — Menghubungkan Tabel]]

SQL

```
-- ─── INNER JOIN: hanya baris yang punya pasangan di kedua tabel ───

-- Siapa meminjam buku apa?
SELECT
    a.nama AS anggota,
    b.judul AS buku,
    p.tanggal_pinjam,
    p.batas_kembali,
    p.status
FROM peminjaman p
INNER JOIN buku b ON p.buku_id = b.id
INNER JOIN anggota a ON p.anggota_id = a.id
ORDER BY p.tanggal_pinjam DESC;

-- ─── LEFT JOIN: semua baris dari tabel kiri, cocok atau tidak ─────

-- Semua anggota dan peminjaman mereka (termasuk yang belum pernah pinjam)
SELECT
    a.nama AS anggota,
    a.nomor_anggota,
    COUNT(p.id) AS jumlah_peminjaman
FROM anggota a
LEFT JOIN peminjaman p ON a.id = p.anggota_id
GROUP BY a.id, a.nama, a.nomor_anggota
ORDER BY jumlah_peminjaman DESC;

-- Anggota yang BELUM pernah meminjam
SELECT a.nama, a.email
FROM anggota a
LEFT JOIN peminjaman p ON a.id = p.anggota_id
WHERE p.id IS NULL;  -- tidak ada peminjaman yang cocok

-- ─── RIGHT JOIN: kebalikan LEFT JOIN ──────────────────────────────
-- Jarang dipakai — lebih baik ubah urutan tabel dan pakai LEFT JOIN

-- ─── CROSS JOIN: semua kombinasi (hati-hati, bisa sangat besar!) ──
-- Setiap anggota × setiap buku = N × M baris
-- Berguna untuk: generate semua kemungkinan, matriks

-- ─── SELF JOIN: tabel join dengan dirinya sendiri ─────────────────
-- (akan berguna untuk hierarki, misal: kategori parent-child)

-- ─── JOIN dengan agregasi ─────────────────────────────────────────

-- Statistik peminjaman per buku
SELECT
    b.judul,
    b.kategori,
    COUNT(p.id) AS total_dipinjam,
    SUM(CASE WHEN p.status = 'terlambat' THEN 1 ELSE 0 END) AS jumlah_terlambat,
    SUM(p.denda) AS total_denda
FROM buku b
LEFT JOIN peminjaman p ON b.id = p.buku_id
GROUP BY b.id, b.judul, b.kategori
ORDER BY total_dipinjam DESC;

-- Buku yang PALING SERING dipinjam (Top 5)
SELECT
    b.judul,
    b.pengarang,
    COUNT(p.id) AS kali_dipinjam
FROM buku b
INNER JOIN peminjaman p ON b.id = p.buku_id
GROUP BY b.id, b.judul, b.pengarang
ORDER BY kali_dipinjam DESC
LIMIT 5;

-- ─── JOIN dengan subquery/CTE ─────────────────────────────────────

-- Anggota dengan peminjaman terlambat beserta detail bukunya
WITH peminjaman_terlambat AS (
    SELECT *
    FROM peminjaman
    WHERE status = 'terlambat'
       OR (status = 'dipinjam' AND batas_kembali < CURDATE())
)
SELECT
    a.nama AS anggota,
    a.email,
    b.judul AS buku,
    pt.tanggal_pinjam,
    pt.batas_kembali,
    DATEDIFF(CURDATE(), pt.batas_kembali) AS hari_terlambat,
    DATEDIFF(CURDATE(), pt.batas_kembali) * 1000 AS estimasi_denda
FROM peminjaman_terlambat pt
INNER JOIN anggota a ON pt.anggota_id = a.id
INNER JOIN buku b ON pt.buku_id = b.id
ORDER BY hari_terlambat DESC;
```

---

### 🏗️ Checkpoint Level 4

text

```
✅ Checklist sebelum lanjut ke Level 5:

TABEL YANG HARUS ADA:
├── buku (dengan kolom lengkap + soft delete)
├── anggota (dengan UNIQUE email dan nomor_anggota)
├── peminjaman (dengan FOREIGN KEY ke buku dan anggota)
└── Semua foreign key punya ON UPDATE CASCADE dan ON DELETE RESTRICT

QUERY YANG HARUS BISA DITULIS:
├── INNER JOIN: siapa pinjam buku apa
├── LEFT JOIN: semua anggota termasuk yang belum pinjam
├── LEFT JOIN + IS NULL: cari yang tidak punya pasangan
├── JOIN + GROUP BY: statistik per buku/anggota
├── Multi-table JOIN: 3+ tabel dalam satu query
└── CTE + JOIN: laporan peminjaman terlambat

PEMAHAMAN:
├── Bisa jelaskan perbedaan INNER JOIN vs LEFT JOIN
├── Bisa jelaskan mengapa FOREIGN KEY penting
├── Bisa jelaskan ON DELETE RESTRICT vs CASCADE vs SET NULL
└── Bisa jelaskan mengapa LEFT JOIN + IS NULL = "yang tidak punya pasangan"

SQL: JOIN (INNER, LEFT, RIGHT), FOREIGN KEY, ON UPDATE/DELETE
```

---

## 🔴 LEVEL 5: DATABASE DESIGN DAN NORMALISASI (Minggu 13-18)

> **Tema**: _"Dari tabel yang 'jalan' ke tabel yang dirancang dengan benar"_  
> **Benang Merah**: Semua data di beberapa tabel (Level 4) → tapi apakah strukturnya benar? → Normalisasi: hilangkan redundansi → Denormalisasi: kapan boleh melanggar aturan → ERD: visualisasi desain  
> **Output**: Schema database perpustakaan yang ternormalisasi dengan benar dan siap untuk skala besar

---

### I. Normalisasi — Menghilangkan Redundansi

> 💡 **Mengapa Normalisasi?** Data yang duplikat = data yang tidak konsisten. Jika nama pengarang disimpan di 100 baris buku dan pengarang ganti nama, kamu harus update 100 baris. Normalisasi memecah data ke tabel terpisah sehingga setiap fakta disimpan di SATU tempat saja.

text

```
Benang Merah Bagian I:
Tabel buku punya kolom pengarang (Level 4) →
Tapi jika satu pengarang punya 50 buku, namanya diulang 50x →
Jika nama salah ketik di 1 baris → data tidak konsisten →
Normalisasi: pisahkan pengarang ke tabel sendiri →
Setiap fakta disimpan di satu tempat → update sekali, berlaku di mana-mana
```

#### [[16. Normal Form 1-3 — Aturan Desain Database]]

SQL

```
-- ─── 1NF (First Normal Form) ──────────────────────────────────────
-- Aturan: setiap kolom harus bernilai ATOMIK (tidak bisa dipecah lagi)
-- Tidak boleh ada kolom yang berisi banyak nilai (comma-separated)

-- ❌ MELANGGAR 1NF:
-- | id | judul       | kategori              |
-- | 1  | Clean Code  | Teknologi, Programming |  ← dua nilai dalam satu kolom!

-- ✅ MEMENUHI 1NF:
-- | id | judul       | kategori    |
-- | 1  | Clean Code  | Teknologi   |  ← satu nilai per kolom
-- Solusi: buat tabel relasi buku_kategori (M:N)

-- ─── 2NF (Second Normal Form) ─────────────────────────────────────
-- Aturan: sudah 1NF + tidak ada partial dependency
-- Setiap kolom non-key harus bergantung pada SELURUH primary key

-- ❌ MELANGGAR 2NF (tabel peminjaman_detail dengan composite key):
-- | peminjaman_id | buku_id | judul_buku | harga_saat_pinjam |
-- Composite key: (peminjaman_id, buku_id)
-- judul_buku hanya bergantung pada buku_id, bukan peminjaman_id → partial!

-- ✅ MEMENUHI 2NF:
-- Pisahkan: judul_buku ada di tabel buku, bukan di peminjaman
-- | peminjaman_id | buku_id | harga_saat_pinjam |

-- ─── 3NF (Third Normal Form) ──────────────────────────────────────
-- Aturan: sudah 2NF + tidak ada transitive dependency
-- Kolom non-key tidak boleh bergantung pada kolom non-key lain

-- ❌ MELANGGAR 3NF:
-- | id | judul | pengarang | negara_pengarang |
-- negara_pengarang bergantung pada pengarang, bukan pada id buku → transitive!

-- ✅ MEMENUHI 3NF:
-- Tabel buku: | id | judul | pengarang_id |
-- Tabel pengarang: | id | nama | negara |
-- negara bergantung pada pengarang_id → benar!

-- ─── Implementasi: pisahkan pengarang ke tabel sendiri ────────────

CREATE TABLE pengarang (
    id      INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nama    VARCHAR(100) NOT NULL,
    negara  VARCHAR(50),
    bio     TEXT,
    UNIQUE KEY uk_nama (nama)
);

-- Insert pengarang unik
INSERT INTO pengarang (nama, negara)
SELECT DISTINCT pengarang, NULL FROM buku;

-- Tambah kolom pengarang_id ke tabel buku
ALTER TABLE buku
    ADD COLUMN pengarang_id INT UNSIGNED AFTER pengarang,
    ADD FOREIGN KEY (pengarang_id) REFERENCES pengarang(id);

-- Update pengarang_id berdasarkan nama
UPDATE buku b
INNER JOIN pengarang p ON b.pengarang = p.nama
SET b.pengarang_id = p.id;

-- Hapus kolom lama (setelah yakin data sudah benar)
-- ALTER TABLE buku DROP COLUMN pengarang;

-- Sekarang: update nama pengarang cukup 1x di tabel pengarang!
UPDATE pengarang SET nama = 'Robert C. Martin' WHERE nama = 'Robert Martin';
-- Semua buku yang ditulisnya otomatis ter-update
```

#### [[17. ERD dan Relasi — Visualisasi Desain Database]]

text

```
ENTITY RELATIONSHIP DIAGRAM (ERD) Perpustakaan:

┌──────────────┐       ┌──────────────────┐       ┌──────────────┐
│  pengarang   │       │      buku        │       │   kategori   │
├──────────────┤       ├──────────────────┤       ├──────────────┤
│ PK id        │──┐    │ PK id            │   ┌───│ PK id        │
│    nama      │  │    │    judul         │   │   │    nama      │
│    negara    │  └───<│ FK pengarang_id  │   │   │    slug      │
│    bio       │  1:N  │ FK kategori_id   │>──┘   │    parent_id │
└──────────────┘       │    isbn          │  N:1  │              │
                       │    tahun         │       └──────────────┘
                       │    harga         │              │
                       │    stok          │         (self-ref)
                       │    deleted_at    │
                       └────────┬─────────┘
                                │ 1:N
                                │
                       ┌────────┴─────────┐       ┌──────────────┐
                       │   peminjaman     │       │   anggota    │
                       ├──────────────────┤       ├──────────────┤
                       │ PK id            │   ┌───│ PK id        │
                       │ FK buku_id       │   │   │    nomor     │
                       │ FK anggota_id    │>──┘   │    nama      │
                       │    tanggal_pinjam│  N:1  │    email     │
                       │    batas_kembali │       │    telepon   │
                       │    tanggal_kembali│      │    status    │
                       │    denda         │       └──────────────┘
                       │    status        │
                       └──────────────────┘

JENIS RELASI:
├── 1:1 (One-to-One)
│   Contoh: anggota → profil_anggota (satu anggota = satu profil)
│   Implementasi: foreign key dengan UNIQUE constraint
│
├── 1:N (One-to-Many) ← PALING UMUM
│   Contoh: pengarang → buku (satu pengarang = banyak buku)
│   Implementasi: foreign key di tabel "banyak" (buku.pengarang_id)
│
├── M:N (Many-to-Many)
│   Contoh: buku ↔ kategori (satu buku = banyak kategori, satu kategori = banyak buku)
│   Implementasi: tabel pivot/junction (buku_kategori)
│
└── Self-Reference
    Contoh: kategori → parent_kategori (sub-kategori)
    Implementasi: foreign key ke tabel sendiri (parent_id)
```

SQL

```
-- ─── Implementasi relasi M:N: buku ↔ kategori ─────────────────────

CREATE TABLE kategori (
    id        INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nama      VARCHAR(50) NOT NULL,
    slug      VARCHAR(50) UNIQUE NOT NULL,
    parent_id INT UNSIGNED NULL,
    FOREIGN KEY (parent_id) REFERENCES kategori(id) ON DELETE SET NULL
);

-- Tabel pivot untuk M:N
CREATE TABLE buku_kategori (
    buku_id     INT UNSIGNED NOT NULL,
    kategori_id INT UNSIGNED NOT NULL,
    PRIMARY KEY (buku_id, kategori_id),  -- composite PK = tidak bisa duplikat
    FOREIGN KEY (buku_id) REFERENCES buku(id) ON DELETE CASCADE,
    FOREIGN KEY (kategori_id) REFERENCES kategori(id) ON DELETE CASCADE
);

INSERT INTO kategori (nama, slug) VALUES
    ('Fiksi', 'fiksi'),
    ('Non-Fiksi', 'non-fiksi'),
    ('Sains', 'sains'),
    ('Teknologi', 'teknologi'),
    ('Sejarah', 'sejarah'),
    ('Programming', 'programming'),
    ('Novel', 'novel');

-- Satu buku bisa punya banyak kategori
INSERT INTO buku_kategori (buku_id, kategori_id) VALUES
    (1, 4), (1, 6),   -- Clean Code: Teknologi + Programming
    (2, 1), (2, 7),   -- Laskar Pelangi: Fiksi + Novel
    (4, 3);            -- Cosmos: Sains

-- Query: semua buku dengan kategorinya (M:N)
SELECT
    b.judul,
    GROUP_CONCAT(k.nama SEPARATOR ', ') AS kategori_list
FROM buku b
LEFT JOIN buku_kategori bk ON b.id = bk.buku_id
LEFT JOIN kategori k ON bk.kategori_id = k.id
GROUP BY b.id, b.judul;
```

---

### 🏗️ Checkpoint Level 5

text

```
✅ Checklist sebelum lanjut ke Level 6:

DESAIN DATABASE:
├── Semua tabel memenuhi minimal 3NF
├── Tidak ada data yang duplikat (redundansi)
├── Setiap fakta disimpan di satu tempat saja
├── Foreign key terdefinisi dengan benar
├── Relasi 1:N, M:N, dan self-reference terimplementasi
└── ERD bisa digambar dan dijelaskan

TABEL LENGKAP:
├── pengarang (1:N ke buku)
├── kategori (self-reference untuk hierarki)
├── buku (FK ke pengarang)
├── buku_kategori (M:N pivot)
├── anggota
└── peminjaman (FK ke buku dan anggota)

PEMAHAMAN:
├── Bisa jelaskan 1NF, 2NF, 3NF dengan contoh
├── Bisa jelaskan kapan normalisasi vs denormalisasi
├── Bisa jelaskan perbedaan 1:N vs M:N dan cara implementasi
└── Bisa jelaskan ON DELETE CASCADE vs RESTRICT vs SET NULL

SQL: ALTER TABLE, FOREIGN KEY, GROUP_CONCAT, tabel pivot
```

---

## ⚫ LEVEL 6: INDEXING, TRANSACTION, DAN OPTIMASI (Minggu 18-24)

> **Tema**: _"Dari database yang benar ke database yang cepat dan aman"_  
> **Benang Merah**: Schema sudah benar (Level 5) → tapi query lambat saat data besar → Index mempercepat pencarian → Transaction menjaga konsistensi → EXPLAIN untuk analisis performa  
> **Output**: Database yang bisa handle 100K+ records dengan query < 100ms

---

### J. Index — Mempercepat Query

> 💡 **Mengapa Index?** Bayangkan cari satu nama di buku telepon 10.000 halaman. Tanpa index: baca dari halaman 1 (full table scan). Dengan index: buka daftar isi, langsung ke halaman yang tepat. Index adalah "daftar isi" untuk database.

text

```
Benang Merah Bagian J:
Query lambat saat data besar →
Full table scan: MySQL baca SEMUA baris →
Index: struktur data (B-Tree) yang mempercepat pencarian →
Tapi index bukan gratis: memperlambat INSERT/UPDATE, makan storage →
Strategi: index kolom yang sering di-WHERE, JOIN, ORDER BY
```

#### [[18. Index — Cara Kerja dan Implementasi]]

SQL

```
-- ─── Cek index yang sudah ada ─────────────────────────────────────
SHOW INDEX FROM buku;
-- PRIMARY KEY otomatis membuat index

-- ─── Buat index ───────────────────────────────────────────────────

-- Single column index
CREATE INDEX idx_buku_kategori ON buku(kategori);
CREATE INDEX idx_buku_tahun ON buku(tahun);

-- Composite index (banyak kolom)
-- Urutan kolom PENTING! Kolom yang paling sering difilter duluan.
CREATE INDEX idx_buku_kategori_tahun ON buku(kategori, tahun);
-- Query yang bisa pakai index ini:
-- WHERE kategori = 'Teknologi'              ✅ (prefix kiri)
-- WHERE kategori = 'Teknologi' AND tahun > 2000  ✅
-- WHERE tahun > 2000                       ❌ (tidak mulai dari kiri)

-- Unique index (sekaligus constraint)
CREATE UNIQUE INDEX idx_buku_isbn ON buku(isbn);

-- Full-text index (untuk pencarian teks)
CREATE FULLTEXT INDEX idx_buku_judul_pengarang ON buku(judul, pengarang);

-- ─── Gunakan index ────────────────────────────────────────────────

-- Pencarian full-text
SELECT judul, pengarang,
       MATCH(judul, pengarang) AGAINST('clean code' IN NATURAL LANGUAGE MODE) AS relevansi
FROM buku
WHERE MATCH(judul, pengarang) AGAINST('clean code' IN NATURAL LANGUAGE MODE)
ORDER BY relevansi DESC;

-- ─── Hapus index ──────────────────────────────────────────────────
DROP INDEX idx_buku_tahun ON buku;

-- ─── Kapan buat index? ────────────────────────────────────────────
-- ✅ BUAT INDEX untuk kolom yang:
```