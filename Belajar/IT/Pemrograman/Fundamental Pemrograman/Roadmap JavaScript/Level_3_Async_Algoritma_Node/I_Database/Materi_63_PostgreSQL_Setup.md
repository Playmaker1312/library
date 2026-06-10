# Materi 63: PostgreSQL & Setup

---

## 1. Penjelasan

**PostgreSQL** (Postgres) adalah database relasional open-source terkemuka — mendukung ACID penuh, tipe data lanjutan (JSON, array, geometric), indexing kompleks, dan ekstensibilitas. Dipilih karena:

- **Standar SQL ketat**: Lebih patuh pada standar SQL dibanding MySQL.
- **Fitur enterprise**: MVCC, point-in-time recovery, replication, partitioning.
- **Ekstensi populer**: PostGIS (geospatial), pgvector (AI/vector search).
- **Performance**: Handling konkurensi tinggi dengan baik.

### Perbandingan dengan MySQL
| Aspek | PostgreSQL | MySQL |
|-------|-----------|-------|
| ACID compliance | ✅ Penuh | ✅ (dengan InnoDB) |
| JSON support | ✅ Native + indexing | ✅ (sejak 5.7) |
| CTE / Window Functions | ✅ Lengkap | ✅ (sejak 8.0) |
| Ekstensi | ✅ PostGIS, pgvector | ❌ Terbatas |
| Replication | Streaming, logical | Master-slave, Group |
| Popularitas backend modern | ✅ (Prisma, Rails) | ✅ (Laravel, WP) |

---

## 2. Fungsi

- Menyimpan dan mengelola data relasional dengan integritas tinggi.
- Menyediakan akses multiuser dengan permission granular.
- Mendukung backup, recovery, replication untuk production.

---

## 3. Code

### Instalasi (Windows)
```powershell
# 1. Download installer dari https://www.postgresql.org/download/windows/
# 2. Jalankan installer, catat password untuk user 'postgres'
# 3. Biarkan port default 5432
# 4. Centang "Stack Builder" jika ingin tools tambahan (pgAdmin)
```

### Setup via psql CLI

```sql
-- Masuk ke psql (command line)
-- psql -U postgres

-- Buat database untuk sistem perpustakaan
CREATE DATABASE perpustakaan_db;

-- Buat user khusus aplikasi (jangan pakai postgres langsung)
CREATE USER librarian WITH PASSWORD 'rahasia123';

-- Beri hak akses ke database
GRANT CONNECT ON DATABASE perpustakaan_db TO librarian;

-- Beri hak akses ke semua tabel (nanti setelah tabel dibuat)
GRANT ALL PRIVILEGES ON DATABASE perpustakaan_db TO librarian;

-- Catatan: Untuk PostgreSQL, GRANT ALL ON DATABASE tidak mencakup schema public
-- Beri akses ke schema public juga:
GRANT ALL ON SCHEMA public TO librarian;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO librarian;

-- Cek daftar database
\l

-- Cek daftar user
\du

-- Connect ke database
\c perpustakaan_db

-- Buat tabel (ulang dari Materi 61, kali ini di PostgreSQL sungguhan)
CREATE TABLE buku (
    id       SERIAL PRIMARY KEY,
    judul    VARCHAR(200) NOT NULL,
    penulis  VARCHAR(100) NOT NULL,
    isbn     VARCHAR(20) UNIQUE,
    tahun    INT CHECK (tahun > 0),
    stok     INT DEFAULT 1 CHECK (stok >= 0),
    dibuat_pada TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Keluar dari psql
\q
```

### Perintah Dasar psql
| Perintah | Fungsi |
|----------|--------|
| `\l` | List semua database |
| `\c nama_db` | Connect ke database |
| `\dt` | List semua tabel |
| `\d nama_tabel` | Deskripsi tabel (kolom, tipe, constraint) |
| `\du` | List semua user/role |
| `\i file.sql` | Jalankan SQL dari file |
| `\q` | Keluar |

---

## 4. Analogi Rumah

| PostgreSQL | Analogi Membangun Rumah |
|-----------|------------------------|
| PostgreSQL Server | Lahan/tanah tempat gudang akan dibangun. |
| `CREATE DATABASE` | Menggali pondasi dan menyiapkan lahan untuk satu gudang. |
| `CREATE USER` | Membuat tukang kunci — setiap tukang punya akses berbeda. |
| `GRANT` | Memberi kunci gudang ke tukang tertentu. |
| `Schema` | Pembagian ruang dalam gudang (lantai 1, lantai 2). |
| Tabel | Rak-rak di dalam gudang. |
| `psql` | Pintu masuk utama ke gudang (CLI). |
| pgAdmin | CCTV dan panel kontrol grafis gudang. |
| Port 5432 | Nomor pintu khusus gudang Postgres. |

---

## 5. Use Case

### Use Case 1: Membuat database dan user untuk aplikasi Node.js
```sql
CREATE DATABASE myapp;
CREATE USER app_user WITH PASSWORD 'secure_pass';
GRANT CONNECT ON DATABASE myapp TO app_user;
GRANT ALL ON SCHEMA public TO app_user;
```

### Use Case 2: Reset database untuk development
```sql
DROP DATABASE IF EXISTS perpustakaan_db;
CREATE DATABASE perpustakaan_db;
```

### Use Case 3: Backup dan restore
```powershell
# Backup (command line, bukan psql)
pg_dump -U postgres perpustakaan_db > backup_perpustakaan.sql

# Restore
psql -U postgres -d perpustakaan_db < backup_perpustakaan.sql
```

---

## 6. Kesalahan Umum

1. **Lupa password postgres** — Reset dengan mengedit `pg_hba.conf` ke `trust`, restart, login, ganti password.
2. **Port 5432 sudah dipakai** — Aplikasi lain (seperti MySQL di port berbeda tidak masalah, tapi bisa saja Redis atau service lain). Cek dengan `netstat -a | findstr 5432`.
3. **User tidak punya akses schema** — `GRANT ALL ON DATABASE` saja tidak cukup. Harus `GRANT ALL ON SCHEMA public` juga.
4. **Tidak mengaktifkan ekstensi** — Contoh: butuh UUID? Jalankan `CREATE EXTENSION IF NOT EXISTS "uuid-ossp";`.
5. **Mematikan service PostgreSQL** — Di Windows: `net stop postgresql-x64-16` atau dari Services.msc.

---

## 7. Benang Merah

```
Materi 62 (SQL JOIN/Agregasi — teori kuat)
    ↓  "Sekarang implementasi di database sungguhan"
*Materi 63: PostgreSQL Setup*  ←── ANDA DI SINI
    ↓  "Database sudah siap, tapi akses langsung SQL itu repot"
Materi 64: Prisma ORM (abstraksi database dengan code)
    ↓
Materi 65: Express + Prisma + PostgreSQL (API production-ready)
```

---

## 8. Soal & Jawaban

### Soal 1
**Apa perintah untuk membuat user baru `app_admin` dengan password `admin123` dan memberikan akses ke database `perpustakaan_db`?**

<details>
<summary>Jawaban</summary>
```sql
CREATE USER app_admin WITH PASSWORD 'admin123';
GRANT CONNECT ON DATABASE perpustakaan_db TO app_admin;
GRANT ALL ON SCHEMA public TO app_admin;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO app_admin;
```
</details>

### Soal 2
**Jelaskan apa itu `pg_hba.conf` dan fungsinya dalam konteks keamanan database.**

<details>
<summary>Jawaban</summary>
`pg_hba.conf` (Host-Based Authentication) adalah file konfigurasi yang mengatur metode autentikasi client ke PostgreSQL. Ia menentukan:
- Dari IP mana saja user bisa login.
- Metode autentikasi (password, trust, md5, scram-sha-256).
- Database dan user mana yang terkena aturan.

Fungsinya seperti **daftar tamu di pintu gudang** — siapa yang boleh masuk, dari pintu mana, dan harusunjukkan KTP apa.
</details>

### Soal 3
**Apa bedanya `pg_dump` dan `psql` dalam kaitannya dengan backup database?**

<details>
<summary>Jawaban</summary>
- `pg_dump`: Tool untuk **backup** — mengekspor database ke file SQL/format lain. Contoh: `pg_dump -U postgres db > backup.sql`
- `psql`: Tool untuk **interaksi** langsung dengan database — bisa menjalankan query atau **restore** dari file backup. Contoh: `psql -U postgres -d db < backup.sql`

Analoginya: pg_dump = fotokopi seluruh isi gudang ke kertas. psql = tukang yang membaca kertas itu dan menaruh kembali barang-barang ke gudang.
</details>
