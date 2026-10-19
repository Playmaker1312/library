# 1. Apa Itu Database dan Mengapa MySQL

> **Level 1 - Modul A** | **Durasi**: 1-2 hari | **Prasyarat**: Dasar komputer & logika

---

## 1. Penjelasan Singkat & Konsep Dasar

**Database** adalah kumpulan data terstruktur yang disimpan secara sistematis sehingga mudah diakses, dikelola, dan diperbarui. Bayangkan database sebagai **perpustakaan digital** — bukan sekadar tumpukan buku, tapi rak-rapi yang terorganisir dengan katalog yang memungkinkan pencarian cepat.

**MySQL** adalah salah satu **RDBMS (Relational Database Management System)** paling populer di dunia. Sebagai DBMS, MySQL bertugas:
- Menyimpan data ke disk (persistensi)
- Mengelola akses bersamaan (concurrency)
- Menjamin integritas data (ACID)
- Menyediakan bahasa query (SQL) untuk berinteraksi dengan data

**Hubungan dengan materi sebelumnya**: Ini adalah fondasi paling awal. Sebelum menulis query `SELECT` atau mendesain tabel, kamu harus paham *apa* yang dikelola MySQL dan *mengapa* kita pilih MySQL di antara database lain.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Konsep | Peran di Ekosistem Database |
|--------|----------------------------|
| **Database** | Wadah logis yang mengelompokkan tabel-tabel terkait (misal: `perpustakaan_db`) |
| **Tabel** | Representasi entitas nyata (Buku, Anggota, Peminjaman) |
| **Baris (Row/Record)** | Satu instance data (satu buku, satu anggota) |
| **Kolom (Column/Field)** | Atribut dari entitas (judul, pengarang, harga) |
| **SQL** | Bahasa standar untuk "berbicara" dengan database: `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `CREATE`, `ALTER` |

**Masalah yang diselesaikan**:
- **Spreadsheet (Excel) kewalahan** di >100 ribu baris, tidak support multi-user, tidak punya transaksi, relasi manual (VLOOKUP rawan salah)
- **File teks/CSV** tidak punya index, query manual, tidak ACID
- **MySQL** handle jutaan baris, ribuan user bersamaan, transaksi terjamin, relasi *enforced* di level engine

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
-- ─── Koneksi ke MySQL Server ─────────────────────────────────────
-- Di terminal/command prompt:
-- mysql -u root -p
-- Masukkan password → prompt mysql>

-- ─── Lihat database yang sudah ada (system databases) ────────────
SHOW DATABASES;
-- +--------------------+
-- | Database           |
-- +--------------------+
-- | information_schema |  ← metadata sistem, JANGAN diutak-atik
-- | mysql              |  ← user, privilege, system tables
-- | performance_schema | ← monitoring performa
-- | sys                |  ← view helper untuk performance_schema
-- +--------------------+

-- ─── Buat database aplikasi kita ─────────────────────────────────
CREATE DATABASE perpustakaan_db
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;
-- utf8mb4 = support penuh Unicode (termasuk emoji 📚, bahasa Cina, Arab, dll)
-- utf8mb4_unicode_ci = sorting & perbandingan yang benar untuk multibahasa

-- ─── Gunakan database ────────────────────────────────────────────
USE perpustakaan_db;

-- ─── Lihat tabel di database saat ini ────────────────────────────
SHOW TABLES;
-- Empty set (belum ada tabel)
```

**Skenario Nyata (E-commerce):**
```sql
-- Database untuk toko online
CREATE DATABASE toko_online_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE toko_online_db;

-- Tabel produk, user, orders, dll akan dibuat di Level 1.3
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep Database | Analogi Perpustakaan |
|-----------------|---------------------|
| **Database (MySQL)** | **Gedung Perpustakaan** — bangunan fisik yang menampung semuanya |
| **Database (logis: `perpustakaan_db`)** | **Satu Lantai/Koleksi** — misal: Lantai 2 Non-Fiksi |
| **Tabel (`buku`)** | **Rak Buku** — tiap rak untuk satu kategori entitas |
| **Kolom (`judul`, `pengarang`)** | **Label/Kolom di Kartu Katalog** — spesifikasi buku |
| **Baris/Record** | **Satu Buku Fisik** — item nyata di rak |
| **Primary Key (`id`)** | **Nomor Inventaris Unik** — tidak ada dua buku sama nomornya |
| **SQL** | **Bahasa Permintaan ke Pustakawan** — "Cari buku judul X", "Pinjam buku Y" |

> 💡 **Key Insight**: MySQL = *pustakawan super canggih* yang tidak pernah salah tempatin buku, bisa melayani ribu pengunjung sekaligus, dan mencatat siapa pinjam apa kapan.

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | Gunakan MySQL? | Alasan |
|---------|----------------|--------|
| Aplikasi web (Laravel, Node, Django, Go) | ✅ **YA** | Stack standar, driver matang, performa baik |
| Data relasional kompleks (FK, JOIN, transaksi) | ✅ **YA** | ACID compliance, FK enforcement, JOIN optimizer |
| Data tidak terstruktur / dokumen fleksibel | ⚠️ **MUNGKIN** | MySQL 8+ punya JSON support, tapi MongoDB lebih natural |
| Analitik OLAP / Data Warehouse besar | ❌ **TIDAK** | Gunakan ClickHouse, Snowflake, BigQuery |
| Cache / session store | ❌ **TIDAK** | Gunakan Redis (lebih cepat, TTL built-in) |
| Embedded / mobile / offline-first | ❌ **TIDAK** | Gunakan SQLite |

**Industri yang pakai MySQL**: Facebook (awalnya), YouTube, Netflix, Twitter, GitHub, Shopify, WordPress.com, dll.

---

## 6. Poin-poin Penting & Best Practices

### ✅ Aturan Emas Saat Mulai
```sql
-- 1. SELALU gunakan utf8mb4 (bukan utf8 lama!)
CREATE DATABASE nama_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- 2. SELALU beri PRIMARY KEY pada setiap tabel
CREATE TABLE contoh (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,  -- wajib!
    ...
);

-- 3. Nama database & tabel: snake_case, lowercase
-- ✅ perpustakaan_db, buku, peminjaman_buku
-- ❌ PerpustakaanDB, Buku, PeminjamanBuku

-- 4. Nama kolom: snake_case, singular, deskriptif
-- ✅ tanggal_pinjam, batas_kembali, nama_lengkap
-- ❌ tglPinjam, batasKembali, nama, col1, data
```

### 🛠️ Perintah CLI MySQL Penting
| Perintah | Fungsi |
|----------|--------|
| `mysql -u root -p` | Koneksi ke server (interaktif) |
| `mysql -u root -p -e "QUERY"` | Jalankan query sekali lalu keluar |
| `mysqldump -u root -p db > backup.sql` | Backup database ke file |
| `mysql -u root -p db < backup.sql` | Restore dari file backup |
| `mysql --version` | Cek versi MySQL |
| `SHOW PROCESSLIST;` | Lihat query yang sedang jalan |
| `KILL <id>;` | Hentikan query bermasalah |

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **Pakai `utf8` (bukan `utf8mb4`)** | Emoji & karakter khusus jadi `???` atau error | Selalu `CHARACTER SET utf8mb4` |
| **Tidak bikin PRIMARY KEY** | Duplicate data, UPDATE/DELETE ambigu, performa buruk | Setiap tabel wajib punya PK |
| **Nama tabel/kolom pakai spasi atau kapital** | Butuh backtick `` `Nama Tabel` `` selalu, case-sensitive di Linux | Pakai `snake_case` lowercase |
| **Lupa `USE database`** | Query jalan di database salah / error "No database selected" | Selalu `USE nama_db` setelah koneksi |
| **Root tanpa password di production** | Celaka keamanan besar | Set password kuat, buat user terpisah per app |

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: Setup Database Baru
> Buat database bernama `inventaris_toko` dengan charset yang benar, lalu buat tabel `produk` dengan kolom: `id` (PK, auto), `nama_produk` (varchar 150, not null), `harga` (decimal 12,2), `stok` (int unsigned), `kategori` (varchar 50), `created_at` (timestamp default now).

**Jawaban:**
```sql
CREATE DATABASE inventaris_toko
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;

USE inventaris_toko;

CREATE TABLE produk (
    id           INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nama_produk  VARCHAR(150) NOT NULL,
    harga        DECIMAL(12, 2) NOT NULL DEFAULT 0.00,
    stok         INT UNSIGNED NOT NULL DEFAULT 0,
    kategori     VARCHAR(50) DEFAULT 'Umum',
    created_at   TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Verifikasi
DESCRIBE produk;
SHOW CREATE TABLE produk;
```

**Pembahasan**:
- `utf8mb4` + `unicode_ci` = standar modern, support emoji & multibahasa
- `INT UNSIGNED` untuk `id` & `stok` = tidak negatif, range 2x lebih besar
- `DECIMAL(12,2)` untuk `harga` = **presisi eksak** untuk uang (bukan FLOAT!)
- `TIMESTAMP DEFAULT CURRENT_TIMESTAMP` = otomatis terisi saat `INSERT`

---

### Tantangan 2: Verifikasi Instalasi
> Jalankan perintah untuk: (a) cek versi MySQL, (b) lihat database yang ada, (c) masuk ke database `perpustakaan_db`, (d) lihat tabel di dalamnya.

**Jawaban:**
```cmd
-- Di terminal (bukan di dalam mysql> prompt):
mysql --version
mysql -u root -p -e "SHOW DATABASES;"
mysql -u root -p -e "USE perpustakaan_db; SHOW TABLES;"

-- Atau di dalam mysql> prompt:
SELECT VERSION();
SHOW DATABASES;
USE perpustakaan_db;
SHOW TABLES;
```

**Pembahasan**: Selalu verifikasi instalasi & koneksi sebelum mulai coding. Versi 8.0+ direkomendasikan (support CTE, Window Functions, JSON, dll).