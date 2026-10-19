# 3. Membuat Database dan Tabel Pertama

> **Level 1 - Modul A** | **Durasi**: 1-2 hari | **Prasyarat**: 02 Instalasi MySQL

---

## 1. Penjelasan Singkat & Konsep Dasar

Jika 02 adalah *membangun gedung perpustakaan*, maka materi ini adalah **membuat ruangan (DATABASE) dan memasang rak pertama (TABLE)**.

- **DATABASE** = wadah logis yang mengelompokkan tabel. Satu server bisa punya banyak database (`perpustakaan_db`, `toko_db`).
- **TABLE** = struktur baris-kolom untuk satu entitas (`buku`). Harus punya **skema**: nama kolom + tipe data + constraint.
- **PRIMARY KEY + AUTO_INCREMENT** = nomor inventaris unik yang naik otomatis — jaminan tiap buku bisa dibedakan meski judul sama.

**Hubungan dengan materi sebelumnya**: Di 02 kamu sudah bisa konek ke server. Sekarang kamu pakai bahasa SQL (`CREATE DATABASE`, `CREATE TABLE`) untuk *membentuk struktur fisik* tempat data akan disimpan (materi 04 akan mengisinya).

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Perintah | Fungsi & Masalah yang Diselesaikan |
|----------|-----------------------------------|
| `CREATE DATABASE ... CHARACTER SET utf8mb4` | Buat namespace terisolasi + cegah bug emoji/karakter Asia |
| `USE db` | Pilih konteks aktif, cegah salah eksekusi di DB lain |
| `CREATE TABLE` | Definisikan skema rigid — MySQL akan *menolak* data yang tidak sesuai tipe |
| `PRIMARY KEY` | Identitas unik, index otomatis, syarat untuk Foreign Key & Replication |
| `NOT NULL / DEFAULT / UNIQUE` | Validasi di level engine, bukan hanya di aplikasi |
| `TIMESTAMP DEFAULT CURRENT_TIMESTAMP` | Audit trail otomatis kapan data dibuat |

Tanpa skema yang benar: data duplikat, harga jadi string, stok bisa negatif — bug yang mahal di production.

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
-- ─── 1. Lihat & buat database ────────────────────────────────────
SHOW DATABASES;
-- Hanya information_schema, mysql, performance_schema, sys = masih kosong

CREATE DATABASE perpustakaan_db
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;
-- Verifikasi charset
SHOW CREATE DATABASE perpustakaan_db;

USE perpustakaan_db;
SELECT DATABASE(); -- perpustakaan_db

-- ─── 2. Buat tabel buku (rak pertama) ────────────────────────────
CREATE TABLE buku (
    id          INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    judul       VARCHAR(200) NOT NULL,
    pengarang   VARCHAR(100) NOT NULL,
    tahun       YEAR NOT NULL,
    harga       DECIMAL(10, 2) NOT NULL DEFAULT 0.00,
    stok        INT UNSIGNED NOT NULL DEFAULT 0,
    kategori    VARCHAR(50) DEFAULT 'Umum',
    isbn        VARCHAR(13) UNIQUE,
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- ─── 3. Inspeksi struktur ────────────────────────────────────────
DESCRIBE buku;
SHOW CREATE TABLE buku\G
SHOW TABLES;
SHOW INDEX FROM buku;

-- ─── 4. Alter contoh (evolusi skema) ─────────────────────────────
ALTER TABLE buku ADD COLUMN deskripsi TEXT NULL AFTER kategori;
ALTER TABLE buku MODIFY COLUMN judul VARCHAR(250) NOT NULL;
-- Rollback jika salah
ALTER TABLE buku DROP COLUMN deskripsi;
```

**Skenario Nyata (Sistem Inventaris Toko):**

```sql
CREATE DATABASE inventaris_toko CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE inventaris_toko;

CREATE TABLE produk (
    id          INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    sku         VARCHAR(20) UNIQUE NOT NULL COMMENT 'Stock Keeping Unit',
    nama        VARCHAR(150) NOT NULL,
    harga       DECIMAL(12,2) NOT NULL CHECK (harga >= 0),
    stok        INT UNSIGNED NOT NULL DEFAULT 0,
    kategori_id INT UNSIGNED,
    is_active   TINYINT(1) DEFAULT 1,
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB;

-- Constraint CHECK (MySQL 8.0.16+) cegah harga negatif di level DB
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| `CREATE DATABASE perpustakaan_db` | **Bangun Lantai 2 khusus Koleksi Umum** — ruangan terpisah dari Lantai 1 (DB lain) |
| `CREATE TABLE buku` | **Pasang Rak Buku** dengan sekat-sekat berlabel: Judul, Pengarang, Tahun... |
| `INT UNSIGNED AUTO_INCREMENT PRIMARY KEY` | **Mesin stempel nomor inventaris** — tiap buku baru otomatis dapat nomor unik naik 1 |
| `VARCHAR(200) NOT NULL` | **Label sekat "Judul"** — maksimal 200 huruf, wajib diisi, kalau kosong ditolak |
| `DECIMAL(10,2)` | **Label harga dengan 2 desimal** — presisi uang, bukan perkiraan |
| `TIMESTAMP DEFAULT CURRENT_TIMESTAMP` | **Stempel tanggal masuk** — otomatis dicap saat buku ditaruh di rak |

> 💡 **Key Insight**: Rak tanpa sekat (tabel tanpa skema) = tumpukan buku berantakan. Skema = sekat yang memaksa kerapian sejak awal.

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | Aksi |
|---------|------|
| Project baru | `CREATE DATABASE ... CHARACTER SET utf8mb4` — selalu! |
| Butuh entitas baru (anggota, peminjaman) | `CREATE TABLE` dengan PK `id` |
| Butuh identitas unik yang tidak bisa duplikat | `UNIQUE` (isbn, email, sku) |
| Kolom wajib ada | `NOT NULL` + `DEFAULT` |
| Audit kapan dibuat/diubah | `created_at` + `updated_at` TIMESTAMP |
| Ubah struktur tanpa hapus data | `ALTER TABLE ... ADD/MODIFY/DROP` |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ DO
CREATE TABLE buku (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY, -- selalu UNSIGNED, selalu PK
    judul VARCHAR(200) NOT NULL,                -- NOT NULL jika wajib
    harga DECIMAL(10,2) NOT NULL DEFAULT 0.00,  -- DECIMAL untuk uang
    stok INT UNSIGNED NOT NULL DEFAULT 0,       -- cegah stok negatif
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- ❌ DON'T
CREATE TABLE Buku (              -- jangan kapital
    ID int PRIMARY KEY,          -- tanpa AUTO_INCREMENT = manual, rawan duplikat
    judul VARCHAR(255),          -- NULL boleh = data tidak konsisten
    harga FLOAT,                 -- FLOAT untuk uang = error pembulatan!
    stok INT                     -- bisa negatif!
);
```

**Konvensi Penamaan:**
- Database/tabel/kolom: `snake_case`, lowercase, singular/plural konsisten (pilih satu: `buku` ATAU `bukus`, jangan campur)
- PK: `id` (simple) atau `buku_id` untuk FK
- Index: `idx_nama_kolom`, Unique: `uk_nama_kolom`

**Perintah CLI Terkait:**

```cmd
SHOW DATABASES;
SHOW TABLES;
DESCRIBE buku;
SHOW CREATE TABLE buku\G
DROP DATABASE perpustakaan_db; -- hati-hati!
DROP TABLE IF EXISTS buku;
```

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **Lupa `CHARACTER SET utf8mb4`** | Emoji error `Incorrect string value` | Selalu explicit di CREATE DATABASE & TABLE |
| **Tidak pakai PRIMARY KEY** | Replica error, UPDATE/DELETE ambigu, full scan | Tiap tabel wajib PK (INT UNSIGNED AUTO_INCREMENT) |
| **Pakai `FLOAT/DOUBLE` untuk harga** | `0.1+0.2=0.3000004` | Pakai `DECIMAL(10,2)` |
| **`VARCHAR(255)` untuk semua** | Boros memory, index besar, tidak deskriptif | Sesuaikan: `VARCHAR(13)` untuk ISBN, `VARCHAR(100)` nama |
| **Lupa `USE db`** | Tabel terbuat di DB yang salah | Selalu `SELECT DATABASE();` sebelum CREATE |
| **`AUTO_INCREMENT` tanpa `UNSIGNED`** | Range setengah terbuang (-2M s/d 2M) | Pakai `INT UNSIGNED` (0 s/d 4M) |
| **Hapus kolom tanpa backup** | Data hilang permanen | `mysqldump` dulu, atau `ALTER` di staging |

**Troubleshooting:**

```sql
-- Error: Table already exists
DROP TABLE IF EXISTS buku;

-- Error: Invalid default value for 'created_at'
SET time_zone = '+07:00'; -- sesuaikan WIB

-- Cek engine & charset
SELECT TABLE_NAME, ENGINE, TABLE_COLLATION
FROM information_schema.TABLES
WHERE TABLE_SCHEMA = 'perpustakaan_db';
-- ENGINE harus InnoDB (support transaction & FK)
```

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: Buat Database & Tabel Lengkap

> Buat database `perpustakaan_db` (jika belum ada) dan tabel `buku` sesuai skema di materi. Lalu tambahkan kolom `isbn VARCHAR(13) UNIQUE` dan `deskripsi TEXT`.

**Jawaban:**

```sql
CREATE DATABASE IF NOT EXISTS perpustakaan_db
    CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE perpustakaan_db;

CREATE TABLE IF NOT EXISTS buku (
    id         INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    judul      VARCHAR(200) NOT NULL,
    pengarang  VARCHAR(100) NOT NULL,
    tahun      YEAR NOT NULL,
    harga      DECIMAL(10,2) NOT NULL DEFAULT 0.00,
    stok       INT UNSIGNED NOT NULL DEFAULT 0,
    kategori   VARCHAR(50) DEFAULT 'Umum',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE buku ADD COLUMN isbn VARCHAR(13) UNIQUE AFTER kategori;
ALTER TABLE buku ADD COLUMN deskripsi TEXT NULL AFTER isbn;
DESCRIBE buku;
```

**Pembahasan**: `IF NOT EXISTS` cegah error saat rerun. `UNIQUE` di `isbn` otomatis buat index B-Tree + cegah duplikat ISBN.

---

### Tantangan 2: Validasi Constraint

> Coba insert baris tanpa `judul` dan dengan `harga` negatif. Apa yang terjadi? Perbaiki skema agar `stok` tidak bisa negatif via CHECK.

**Jawaban:**

```sql
-- Akan error: Field 'judul' doesn't have a default value
INSERT INTO buku (pengarang, tahun, harga) VALUES ('Anonim', 2024, 50000);

-- MySQL 8.0.16+ : tambah CHECK
ALTER TABLE buku ADD CONSTRAINT chk_harga CHECK (harga >= 0);
ALTER TABLE buku ADD CONSTRAINT chk_stok CHECK (stok >= 0);

-- Test
INSERT INTO buku (judul, pengarang, tahun, harga, stok) VALUES ('Test', 'A', 2024, -1000, 5);
-- ERROR 3819: Check constraint 'chk_harga' is violated.

SELECT * FROM information_schema.CHECK_CONSTRAINTS
WHERE CONSTRAINT_SCHEMA = 'perpustakaan_db';
```

**Pembahasan**: Constraint di level DB = *last line of defense*. Aplikasi bisa bug, tapi DB tetap tolak data tidak valid. `INT UNSIGNED` sudah cegah negatif di level tipe, `CHECK` tambah validasi eksplisit untuk kejelasan.

