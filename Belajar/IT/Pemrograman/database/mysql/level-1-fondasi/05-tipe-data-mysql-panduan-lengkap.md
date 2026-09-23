# 5. Tipe Data MySQL — Panduan Lengkap

> **Level 1 - Modul B** | **Durasi**: 2 hari | **Prasyarat**: 03-04 Database & Insert

---

## 1. Penjelasan Singkat & Konsep Dasar

Tipe data = **aturan main** untuk tiap kolom: jenis nilai apa yang boleh disimpan, berapa besar, seberapa presisi.

Memilih tipe yang salah = boros storage, query lambat, bug pembulatan uang. Memilih yang tepat = hemat, cepat, akurat.

**Hubungan dengan materi sebelumnya**: Di 03 kamu buat tabel `buku` dengan `VARCHAR`, `DECIMAL`, `YEAR` tanpa paham *mengapa* itu dipilih. Di sini kamu paham *trade-off* tiap tipe sehingga di Level 5 (Desain DB) kamu bisa rancang skema yang efisien dari awal.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Kategori | Tipe | Kegunaan & Masalah yang Diselesaikan |
|----------|------|--------------------------------------|
| **Numeric** | `TINYINT` s/d `BIGINT`, `DECIMAL` | `INT UNSIGNED` untuk ID/stok (hemat 50% vs BIGINT), `DECIMAL(10,2)` untuk uang (presisi eksak, bukan `FLOAT` yang 0.1+0.2=0.3000004) |
| **String** | `CHAR`, `VARCHAR`, `TEXT`, `ENUM` | `CHAR(13)` untuk ISBN fix-length (cepat), `VARCHAR(100)` untuk nama (hemat vs 255), `TEXT` untuk deskripsi panjang (tidak di-index penuh) |
| **Date/Time** | `DATE`, `DATETIME`, `TIMESTAMP`, `YEAR` | `TIMESTAMP` auto-timezone untuk `created_at`, `DATETIME` untuk jadwal tetap, `YEAR` untuk tahun terbit (1 byte!) |
| **Boolean** | `BOOLEAN (=TINYINT(1))` | Flag `is_active` 0/1 |
| **JSON** | `JSON` (MySQL 5.7+) | Metadata fleksibel, queryable (`JSON_EXTRACT`) |

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── NUMERIC: pilih TERKECIL yang cukup ──────────────────────────
CREATE TABLE contoh_numeric (
    umur        TINYINT UNSIGNED,      -- 0-255, cukup untuk umur
    stok        INT UNSIGNED,          -- 0-4M, untuk inventaris
    total_user  BIGINT UNSIGNED,       -- untuk skala besar
    harga       DECIMAL(10,2) NOT NULL, -- WAJIB untuk uang!
    diskon      DECIMAL(5,2),          -- 999.99 max
    rating      FLOAT                  -- untuk sains, bukan uang
);

-- Bukti FLOAT berbahaya untuk uang
SELECT 0.1 + 0.2 AS float_hasil; -- 0.30000000000000004
SELECT CAST(0.1 AS DECIMAL(5,2)) + CAST(0.2 AS DECIMAL(5,2)) AS decimal_hasil; -- 0.30

-- ─── STRING ───────────────────────────────────────────────────────
CREATE TABLE contoh_string (
    kode_negara CHAR(2) NOT NULL,              -- ID, US → selalu 2 char, pakai CHAR
    isbn        CHAR(13) NOT NULL,             -- selalu 13 char
    judul       VARCHAR(200) NOT NULL,         -- variabel, hemat
    deskripsi   TEXT,                          -- panjang, tidak di-index
    status      ENUM('aktif','nonaktif','suspended') DEFAULT 'aktif'
);

-- CHAR vs VARCHAR: CHAR(5) 'AB' disimpan 'AB   ' (padding), VARCHAR 'AB' tetap 'AB'
-- ENUM hemat 1-2 byte vs VARCHAR, tapi tambah opsi butuh ALTER TABLE

-- ─── DATE/TIME ────────────────────────────────────────────────────
CREATE TABLE contoh_waktu (
    tanggal_lahir DATE,                        -- 2024-01-15
    jam_buka      TIME,                        -- 08:00:00
    jadwal_pinjam DATETIME,                    -- 2024-01-15 14:30:00 (tanpa timezone)
    created_at    TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- auto UTC, konversi timezone
    tahun_terbit  YEAR                         -- 2024 (1 byte, range 1901-2155)
);

-- Perbedaan DATETIME vs TIMESTAMP
SELECT NOW() AS datetime_now, CURRENT_TIMESTAMP AS ts_now;
-- TIMESTAMP terpengaruh @@time_zone, DATETIME tidak

-- ─── JSON (MySQL 8.0) ────────────────────────────────────────────
CREATE TABLE buku_meta (
    id       INT UNSIGNED PRIMARY KEY,
    metadata JSON,
    INDEX idx_metadata ((CAST(metadata->'$.bahasa' AS CHAR(20))))
);

INSERT INTO buku_meta VALUES (1, '{"halaman": 350, "bahasa": "Indonesia", "edisi": 2}');
SELECT metadata->>'$.halaman' AS halaman, metadata->>'$.bahasa' AS bahasa FROM buku_meta;
-- Query JSON
SELECT * FROM buku_meta WHERE metadata->>'$.bahasa' = 'Indonesia';

-- ─── BOOLEAN ──────────────────────────────────────────────────────
CREATE TABLE anggota_flag (
    id        INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nama      VARCHAR(100),
    is_active BOOLEAN DEFAULT TRUE -- alias TINYINT(1): 1=true, 0=false
);
INSERT INTO anggota_flag (nama, is_active) VALUES ('Budi', TRUE), ('Siti', FALSE);
SELECT * FROM anggota_flag WHERE is_active = TRUE;

-- ─── Contoh Tabel Buku Ideal ─────────────────────────────────────
CREATE TABLE buku_ideal (
    id         INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    judul      VARCHAR(200) NOT NULL,
    pengarang  VARCHAR(100) NOT NULL,
    isbn       CHAR(13) UNIQUE,               -- fix length → CHAR
    tahun      YEAR NOT NULL,
    harga      DECIMAL(10,2) NOT NULL,
    stok       SMALLINT UNSIGNED DEFAULT 0,   -- stok jarang > 65535 → SMALLINT cukup, hemat 2 byte
    kategori   ENUM('Fiksi','Sains','Teknologi','Sejarah','Umum') DEFAULT 'Umum',
    deskripsi  TEXT,
    metadata   JSON,
    is_deleted BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

**Skenario Nyata (Sistem Keuangan):**

```sql
CREATE TABLE transaksi (
    id          BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY, -- transaksi bisa miliaran
    kode        CHAR(10) NOT NULL, -- TRX-2024-001 (fix pattern → CHAR)
    jumlah      DECIMAL(15,2) NOT NULL CHECK (jumlah > 0), -- presisi uang!
    kurs        DECIMAL(10,4), -- kurs butuh 4 desimal
    metode      ENUM('transfer','cash','qris') NOT NULL,
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
-- Jangan pakai FLOAT untuk jumlah! Selisih 1 rupiah x 1jt transaksi = rugi 1jt
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Tipe Data | Analogi |
|-----------|---------|
| `CHAR(2)` kode negara | **Kotak stempel 2 huruf** — selalu 2 slot, kosong dipadding spasi |
| `VARCHAR(200)` judul | **Label elastis** — panjang sesuai judul, tidak buang kertas |
| `TEXT` deskripsi | **Buku catatan tebal** — untuk ringkasan panjang, tidak muat di label kecil |
| `DECIMAL(10,2)` harga | **Kalkulator kasir presisi** — 2 desimal pas, tidak ngarang |
| `FLOAT` | **Penggaris karet** — kelihatan pas, tapi melar 0.0000001 |
| `YEAR` | **Cap tahun 1 stempel** — 1 byte, bukan tulis lengkap `2024-01-01` |
| `TIMESTAMP` | **Jam dinding otomatis** — ikut timezone perpustakaan |
| `ENUM` | **Stempel pilihan** — hanya boleh cap "aktif/nonaktif", tidak bebas tulis |
| `JSON` | **Kantong fleksibel** — isi bebas (halaman, bahasa), tapi tetap bisa dicari |

> 💡 **Key Insight**: Pilih kotak yang *pas* — jangan pakai kardus besar untuk simpan peniti!

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Tipe | Kapan Pakai | Contoh |
|------|-------------|--------|
| `TINYINT UNSIGNED` | Nilai 0-255 | Umur, rating 1-5, stok kecil |
| `INT UNSIGNED` | ID, stok jutaan | `id`, `stok`, `jumlah_penduduk` |
| `BIGINT` | Data >4M | ID global, transaksi high-volume |
| `DECIMAL` | Uang, akurasi penting | `harga`, `saldo`, `gaji` |
| `CHAR(n)` | Panjang tetap | `isbn CHAR(13)`, `kode CHAR(2)` |
| `VARCHAR(n)` | Teks variabel <255 | `nama VARCHAR(100)`, `email VARCHAR(255)` |
| `TEXT` | Teks panjang | `deskripsi`, `artikel`, `komentar` |
| `ENUM` | Pilihan tetap, jarang berubah | `status`, `kategori` (jika <10 opsi) |
| `DATE/DATETIME` | Tanggal tanpa timezone | `tanggal_lahir`, `jadwal_event` |
| `TIMESTAMP` | Audit, log, timezone-aware | `created_at`, `updated_at`, `last_login` |
| `YEAR` | Tahun saja | `tahun_terbit` |
| `BOOLEAN` | Flag | `is_active`, `is_deleted` |
| `JSON` | Atribut dinamis | `metadata`, `config`, `attributes` |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ Aturan Emas
-- 1. Uang = DECIMAL, bukan FLOAT/DOUBLE
harga DECIMAL(10,2) -- 10 total, 2 desimal

-- 2. Pilih ukuran terkecil yang cukup
umur TINYINT UNSIGNED -- bukan INT

-- 3. VARCHAR jangan 255 semua
nama VARCHAR(100)  -- ukur data nyata, nama Indo <100
email VARCHAR(255) -- max email RFC 254 → 255 ok

-- 4. TIMESTAMP untuk audit, DATETIME untuk jadwal tetap
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
jadwal_ujian DATETIME NOT NULL

-- 5. ENUM hanya jika opsi sangat stabil
status ENUM('aktif','nonaktif') -- jika sering nambah → pakai VARCHAR + FK lookup table

-- 6. JSON index via generated column
ALTER TABLE buku_meta ADD COLUMN bahasa VARCHAR(20)
  GENERATED ALWAYS AS (metadata->>'$.bahasa') STORED,
  ADD INDEX idx_bahasa (bahasa);
```

**CLI Terkait:**

```sql
SHOW CREATE TABLE buku\G
DESCRIBE buku;
SELECT DATA_TYPE FROM information_schema.COLUMNS WHERE TABLE_NAME='buku';
```

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **Pakai `FLOAT` untuk harga** | `SUM(harga)` meleset, laporan keuangan salah | `DECIMAL(10,2)` |
| **`VARCHAR(255)` untuk semua** | Index besar, memory boros, tidak semantik | Sesuaikan: `VARCHAR(13)` ISBN |
| **`TEXT` untuk `nama`** | Tidak bisa di-index penuh, lambat | `VARCHAR(100)` |
| **Lupa `UNSIGNED` untuk stok/id** | Setengah range terbuang, stok bisa -1 | `INT UNSIGNED` |
| **`DATETIME` untuk `created_at` tanpa timezone** | Bingung saat server pindah timezone | `TIMESTAMP` untuk audit |
| **`ENUM` untuk kategori dinamis** | Tambah kategori butuh `ALTER TABLE` (lock!) | Buat tabel `kategori` + FK |
| **Simpan angka sebagai `VARCHAR`** | Sorting salah (`'10' < '2'`), tidak bisa `SUM` | Pakai numeric |
| **Pakai `YEAR(4)` deprecated** | Syntax lama | `YEAR` saja |

**Troubleshooting:**

```sql
-- Cek pembulatan
SELECT CAST(123.456 AS DECIMAL(5,2)); -- 123.46

-- Cek panjang data
SELECT MAX(LENGTH(judul)) FROM buku; -- jika max 80, VARCHAR(200) masih ok, jangan 255

-- Cek charset kolom
SELECT COLUMN_NAME, CHARACTER_SET_NAME FROM information_schema.COLUMNS
WHERE TABLE_NAME='buku' AND DATA_TYPE LIKE '%char%';
```

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: Pilih Tipe yang Tepat

> Rancang tabel `anggota` dengan: `id`, `nomor_anggota CHAR(10) UNIQUE` (format ANG-0001), `nama VARCHAR(100)`, `email VARCHAR(255)`, `umur TINYINT UNSIGNED`, `saldo DECIMAL(12,2)`, `status ENUM('aktif','nonaktif')`, `tanggal_daftar DATE`, `created_at TIMESTAMP`.

**Jawaban:**

```sql
CREATE TABLE anggota (
    id              INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nomor_anggota   CHAR(10) UNIQUE NOT NULL,
    nama            VARCHAR(100) NOT NULL,
    email           VARCHAR(255) UNIQUE NOT NULL,
    umur            TINYINT UNSIGNED CHECK (umur BETWEEN 0 AND 120),
    saldo           DECIMAL(12,2) NOT NULL DEFAULT 0.00,
    status          ENUM('aktif','nonaktif') DEFAULT 'aktif',
    tanggal_daftar  DATE NOT NULL,
    created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

DESCRIBE anggota;
```

**Pembahasan**: `CHAR(10)` pas untuk format tetap, `TINYINT UNSIGNED` hemat vs INT, `DECIMAL` untuk saldo, `ENUM` untuk status yang stabil.

---

### Tantangan 2: Perbaiki Skema Buruk

> Tabel `penjualan` punya `harga VARCHAR(50)` dan `tanggal VARCHAR(20)`. Perbaiki agar bisa `SUM(harga)` dan `ORDER BY tanggal` dengan benar. Migrasi datanya.

**Jawaban:**

```sql
-- Skema buruk
CREATE TABLE penjualan_buruk (id INT PRIMARY KEY, harga VARCHAR(50), tanggal VARCHAR(20));
INSERT INTO penjualan_buruk VALUES (1,'100000','2024-01-10'), (2,'20000','2024-01-02');

-- Sorting salah: '100000' < '20000' secara string!
SELECT * FROM penjualan_buruk ORDER BY harga; -- 100000 dulu? salah!

-- Perbaiki
ALTER TABLE penjualan_buruk
    MODIFY harga DECIMAL(10,2) NOT NULL,
    MODIFY tanggal DATE NOT NULL;

-- Sekarang benar
SELECT SUM(harga) AS total FROM penjualan_buruk; -- 120000.00
SELECT * FROM penjualan_buruk ORDER BY tanggal; -- 2024-01-02 dulu

-- Jika data kotor, bersihkan dulu
-- UPDATE penjualan_buruk SET harga = REPLACE(harga, ',', '') WHERE harga LIKE '%,%';
```

**Pembahasan**: Angka sebagai string = tidak bisa agregasi & sorting. Migrasi tipe harus plus *data cleansing*. Selalu pakai tipe semantik sejak awal untuk hindari migrasi mahal di production.

