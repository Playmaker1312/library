# 2. Instalasi MySQL dan Setup Environment

> **Level 1 - Modul A** | **Durasi**: 1 hari | **Prasyarat**: 01 Apa Itu Database

---

## 1. Penjelasan Singkat & Konsep Dasar

Setelah paham *apa itu database* (materi 01), langkah logis berikutnya adalah **menginstal MySQL Server** — software yang mengelola database — dan **menyiapkan environment** untuk berinteraksi dengannya.

Pahami 3 komponen utama:

- **MySQL Server** → mesin utama yang menyimpan data, menjalankan query, menjaga ACID. Ibarat *gudang perpustakaan* yang menyimpan semua buku.
- **MySQL Client** → alat untuk "berbicara" dengan server (MySQL CLI, MySQL Workbench, phpMyAdmin, DBeaver). Ibarat *meja resepsionis*.
- **Port 3306** → pintu default tempat client mengetuk server. Jika port tertutup/salah, koneksi gagal meski server nyala.

**Hubungan dengan materi sebelumnya**: Di 01 kamu belajar gedung perpustakaan itu apa. Di sini kamu *membangun gedungnya* dan *membuka pintunya* sehingga bisa mulai simpan buku (tabel & data di materi 03-04).

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

Mengapa setup environment penting?

| Komponen | Fungsi |
|----------|--------|
| **Server `mysqld`** | Proses daemon yang listen di port 3306, handle query, transaction, storage engine (InnoDB) |
| **Client `mysql`** | CLI untuk kirim SQL ke server, lihat hasil |
| **MySQL Workbench / phpMyAdmin / DBeaver** | GUI untuk visualisasi tabel, ERD, import/export |
| **Docker Image `mysql:8.0`** | Isolasi environment, mudah reset, konsisten antar OS |
| **Konfigurasi `my.ini` / `my.cnf`** | Atur charset default, max connection, buffer pool |

Masalah yang diselesaikan: tanpa instalasi yang benar, kamu akan buang waktu debug error `Can't connect to MySQL server` atau `Access denied for user 'root'`.

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

### Opsi A — Laragon (Windows, Paling Mudah untuk Pemula)

```cmd
:: 1. Download dari laragon.org → Install → Klik "Start All"
:: 2. MySQL otomatis jalan di port 3306
:: 3. Buka terminal Laragon (Menu > Terminal)

mysql -u root -p
:: Password kosong → langsung Enter → muncul prompt mysql>

SELECT VERSION();
-- +-----------+
-- | VERSION() |
-- +-----------+
-- | 8.0.36    |
-- +-----------+

SHOW DATABASES;
```

### Opsi B — Docker (Rekomendasi untuk Project Modern)

```cmd
:: Jalankan MySQL 8.0 terisolasi, data persisten di volume
docker run --name mysql-perpustakaan ^
  -e MYSQL_ROOT_PASSWORD=secret123 ^
  -e MYSQL_DATABASE=perpustakaan_db ^
  -p 3306:3306 -d mysql:8.0

:: Cek status
docker ps
docker logs mysql-perpustakaan

:: Masuk ke client di dalam container
docker exec -it mysql-perpustakaan mysql -u root -p
:: Masukkan: secret123

:: Atau koneksi dari host
mysql -h 127.0.0.1 -P 3306 -u root -p
```

### Opsi C — MySQL Installer + Workbench (Windows Standalone)

```cmd
:: 1. Download dev.mysql.com/downloads/installer
:: 2. Pilih "Developer Default" → set root password: MyStr0ng!Pass
:: 3. Buka MySQL Workbench → New Connection → Test Connection

:: Verifikasi via CLI
mysql --version
:: mysql  Ver 8.0.36 for Win64 on x86_64

mysql -u root -p -e "SELECT VERSION(); SHOW DATABASES;"
```

### Opsi D — Homebrew (macOS) / APT (Linux)

```cmd
# macOS
brew install mysql@8.0
brew services start mysql
mysql -u root

# Ubuntu/Debian
sudo apt update && sudo apt install mysql-server -y
sudo systemctl status mysql
sudo mysql -u root -p
```

### Setup Awal Wajib Setelah Install

```sql
-- ─── Cek charset server ──────────────────────────────────────────
SHOW VARIABLES LIKE 'character_set_server';
-- Harus utf8mb4, jika masih latin1/utf8 ubah di my.ini:
-- [mysqld]
-- character-set-server=utf8mb4
-- collation-server=utf8mb4_unicode_ci

-- ─── Buat user khusus aplikasi (JANGAN pakai root untuk app!) ───
CREATE USER 'perpus_app'@'localhost' IDENTIFIED BY 'AppS3cret!2024';
GRANT ALL PRIVILEGES ON perpustakaan_db.* TO 'perpus_app'@'localhost';
FLUSH PRIVILEGES;

-- ─── Test koneksi user baru ──────────────────────────────────────
-- Di terminal baru:
-- mysql -u perpus_app -p
-- SHOW DATABASES; -- hanya lihat perpustakaan_db + information_schema

-- ─── Cek port & proses ───────────────────────────────────────────
SHOW VARIABLES LIKE 'port'; -- 3306
SHOW PROCESSLIST;
```

**Skenario Nyata (E-commerce):**
Setiap microservice (katalog, order, payment) punya user DB terpisah dengan privilege minimal — `katalog_app` hanya bisa SELECT/INSERT ke `produk`, tidak bisa DROP database. Ini mencegah blast radius jika kredensial bocor.

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep Setup | Analogi Perpustakaan |
|--------------|----------------------|
| **Install MySQL Server** | **Membangun gedung perpustakaan** + pasang rak, listrik, AC |
| **Port 3306** | **Pintu masuk utama** — semua pengunjung lewat sini |
| **MySQL Client (CLI/Workbench)** | **Kartu anggota & meja resepsionis** — cara pengunjung minta buku |
| **Docker Container** | **Perpustakaan portable** — bisa dipindah, di-copy, di-reset tanpa bongkar gedung asli |
| **User `root` vs `perpus_app`** | **Kepala perpustakaan vs petugas sirkulasi** — kepala bisa bongkar gedung, petugas hanya bisa pinjam-kembalikan |

> 💡 **Key Insight**: `root` = kunci master gedung. Jangan kasih ke setiap aplikasi — buat kunci duplikat terbatas per petugas.

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | Pilihan Setup |
|---------|---------------|
| Belajar cepat di Windows, tidak mau ribet | **Laragon / XAMPP** |
| Project tim, butuh environment identik | **Docker `mysql:8.0`** |
| Butuh GUI visual untuk ERD & query | **MySQL Workbench / DBeaver** |
| Production server | **MySQL di VM/Cloud (RDS, Cloud SQL) + user terpisah + password kuat + backup** |
| Butuh isolasi banyak versi MySQL | **Docker dengan port berbeda (3306, 3307)** |

---

## 6. Poin-poin Penting & Best Practices

### ✅ Setup Checklist

```cmd
:: 1. Selalu pakai MySQL 8.0+ (support CTE, Window Function, JSON)
mysql --version

:: 2. Verifikasi utf8mb4 sebagai default
:: my.ini / my.cnf
[mysqld]
character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci
[client]
default-character-set=utf8mb4

:: 3. Jangan pakai root untuk aplikasi
CREATE USER 'app'@'%' IDENTIFIED BY 'kuat!';

:: 4. Simpan kredensial di .env, JANGAN di kode
:: .env
DB_HOST=127.0.0.1
DB_PORT=3306
DB_USER=perpus_app
DB_PASS=secret123

:: 5. Backup sebelum eksperimen
mysqldump -u root -p perpustakaan_db > backup_awal.sql
```

### 🛠️ Perintah CLI Esensial

| Perintah | Fungsi |
|----------|--------|
| `mysql -u root -p` | Login interaktif |
| `mysql -u root -p -e "QUERY"` | One-shot query |
| `mysql --version` / `SELECT VERSION();` | Cek versi |
| `SHOW DATABASES;` | List DB |
| `SHOW VARIABLES LIKE 'port';` | Cek port |
| `SHOW PROCESSLIST;` | Query yang sedang jalan |
| `docker ps` / `docker logs mysql-perpustakaan` | Monitoring Docker |
| `mysqldump -u root -p db > file.sql` | Backup |
| `mysql -u root -p db < file.sql` | Restore |

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Gejala | Solusi |
|-----------|--------|--------|
| **Port 3306 bentrok** (XAMPP + Laragon + Docker bareng) | `Can't connect`, `Address already in use` | Matikan salah satu, atau mapping port Docker `-p 3307:3306` |
| **Lupa password root** | `Access denied for user 'root'` | Reset via `--skip-grant-tables` atau reinstall Docker volume |
| **Charset masih `latin1`** | Emoji jadi `???`, `Incorrect string value` | Ubah `my.ini` ke `utf8mb4`, restart server |
| **Pakai root di aplikasi** | Risiko keamanan, bisa DROP semua DB | Buat user khusus `GRANT ON perpustakaan_db.*` saja |
| **Tidak cek `docker ps`** | Kira server mati padahal container exited | `docker ps -a` → `docker start mysql-perpustakaan` |
| **Firewall blok port 3306** | `Can't connect` dari host lain | Buka firewall / pakai `127.0.0.1` untuk lokal |

**Troubleshooting Cepat:**

```cmd
:: Cek siapa pakai 3306 (Windows)
netstat -ano | findstr :3306
tasklist | findstr <PID>

:: Cek error log
docker logs mysql-perpustakaan --tail 50
:: atau di Windows: C:\laragon\data\mysql\*.err

:: Test koneksi tanpa password
mysql -u root
:: Jika bisa, set password:
ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewPass123!';
```

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: Instalasi & Verifikasi

> Install MySQL via salah satu metode, lalu buktikan: (a) versi 8.0+, (b) charset `utf8mb4`, (c) bisa login sebagai `root`.

**Jawaban:**

```cmd
:: Terminal
mysql --version
:: mysql  Ver 8.0.36 ...

mysql -u root -p -e "SELECT VERSION(); SHOW VARIABLES LIKE 'character_set_server';"
-- +-----------+
-- | VERSION() | character_set_server | Value   |
-- +-----------+----------------------+---------+
-- | 8.0.36    | character_set_server | utf8mb4 |
-- +-----------+----------------------+---------+
```

**Pembahasan**: Jika `character_set_server` bukan `utf8mb4`, perbaiki `my.ini` lalu restart. Ini mencegah bug encoding di materi 03-05.

---

### Tantangan 2: Buat User Aplikasi

> Buat database `perpustakaan_db` (utf8mb4) dan user `perpus_app` yang hanya punya akses ke DB tersebut. Test login sebagai `perpus_app` dan buat tabel dummy.

**Jawaban:**

```sql
-- Sebagai root
CREATE DATABASE perpustakaan_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'perpus_app'@'localhost' IDENTIFIED BY 'Perpus2024!';
GRANT ALL PRIVILEGES ON perpustakaan_db.* TO 'perpus_app'@'localhost';
FLUSH PRIVILEGES;

-- Test sebagai perpus_app (terminal baru)
-- mysql -u perpus_app -p
USE perpustakaan_db;
CREATE TABLE test_koneksi (id INT PRIMARY KEY, catatan VARCHAR(50));
SHOW TABLES;
DROP TABLE test_koneksi;

-- Verifikasi privilege terbatas
SHOW GRANTS FOR 'perpus_app'@'localhost';
-- GRANT USAGE ON *.* ...
-- GRANT ALL PRIVILEGES ON `perpustakaan_db`.* ...
```

**Pembahasan**: Prinsip *least privilege* — aplikasi hanya bisa akses DB-nya sendiri. Jika kredensial bocor, attacker tidak bisa DROP `mysql` system DB. Ini fondasi keamanan production.

