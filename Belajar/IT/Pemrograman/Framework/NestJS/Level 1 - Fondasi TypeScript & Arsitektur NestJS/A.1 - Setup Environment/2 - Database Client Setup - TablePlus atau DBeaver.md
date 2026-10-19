# Database Client Setup — TablePlus atau DBeaver

## Penjelasan

Di materi sebelumnya kita sudah menyiapkan **mesin diesel (Node.js)**, **gudang material (pnpm)**, **mandor (NestJS CLI)**, dan **ruang server prefab (Docker)**. Sekarang kita butuh **jendela kaca** untuk melihat isi gedung — yaitu **database client**.

NestJS sering digunakan bersama database relasional seperti PostgreSQL. Untuk berinteraksi dengan database (melihat tabel, menjalankan query, mengelola data), kita perlu **GUI database client**. Dua yang paling populer adalah **TablePlus** (ringan, modern, berbayar) dan **DBeaver** (gratis, fitur lengkap).

## Fungsi

Database client memungkinkan kita:

- Melihat struktur tabel tanpa menulis SQL panjang
- Menjalankan query ad-hoc dengan cepat
- Mengimpor/ekspor data
- Memantau koneksi database
- Membuat database dan user baru lewat GUI

## Cara Pengimplementasian

### 1. Setup PostgreSQL via Docker

Sebelum menggunakan database client, kita harus menjalankan PostgreSQL. Karena kita sudah punya Docker, buat container PostgreSQL:

```bash
docker run --name postgres-nestjs \
  -e POSTGRES_PASSWORD=root \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_DB=nestjs_learn \
  -p 5432:5432 \
  -d postgres:16-alpine
```

Verifikasi container berjalan:

```bash
docker ps
# CONTAINER ID   IMAGE                ...   PORTS                    NAMES
# xxxxxxxx       postgres:16-alpine    ...   0.0.0.0:5432->5432/tcp   postgres-nestjs
```

### 2. Install Database Client

**Opsi A — TablePlus** (direkomendasikan untuk pengguna Windows/macOS)
- Unduh dari [https://tableplus.com](https://tableplus.com)
- Install dan buka
- Klik "Create New Connection" → pilih PostgreSQL

**Opsi B — DBeaver** (gratis, open source)
- Unduh dari [https://dbeaver.io](https://dbeaver.io)
- Install dan buka
- Klik icon "New Database Connection" → pilih PostgreSQL

### 3. Konfigurasi Koneksi

Baik TablePlus maupun DBeaver, isi field berikut:

| Field | Value |
|-------|-------|
| Host | `localhost` atau `127.0.0.1` |
| Port | `5432` |
| User | `postgres` |
| Password | `root` |
| Database | `nestjs_learn` |

### 4. Connection String (untuk konfigurasi aplikasi)

Nantinya di aplikasi NestJS, kita akan menggunakan connection string seperti ini:

```
postgresql://postgres:root@localhost:5432/nestjs_learn
```

Atau dalam bentuk objek konfigurasi:

```typescript
const dbConfig = {
  host: 'localhost',
  port: 5432,
  username: 'postgres',
  password: 'root',
  database: 'nestjs_learn',
};
```

## Analogi

Database client adalah **jendela kaca besar** yang dipasang di gedung bertingkat:

- **Docker PostgreSQL** = ruang server di lantai dasar yang menyimpan semua arsip
- **Database client** = jendela kaca yang memungkinkan kita melihat isi lemari arsip tanpa harus masuk ke ruangan
- **Query SQL** = perintah ke petugas arsip untuk mengambil dokumen tertentu
- **TablePlus / DBeaver** = dua merek jendela — satu bening tipis (TablePlus), satu bening tebal dengan fitur tambahan (DBeaver)

Tanpa database client, kita seperti berjalan di lorong gedung buta — tidak bisa melihat isi database tanpa menulis query dari terminal.

## Dipakai untuk Apa

Database client digunakan di **seluruh siklus development**:

- **Development**: melihat hasil migrasi, mengecek data yang baru di-insert
- **Debugging**: memeriksa isi tabel saat ada bug di query
- **Testing**: memverifikasi data test
- **Production**: (dengan hati-hati) melakukan query darurat

## Kesalahan Umum yang Sering Terjadi

1. **Docker container tidak running** — Database client gagal konek karena lupa `docker start postgres-nestjs`. Cek dengan `docker ps`.
2. **Port bentrok** — Port 5432 sudah dipakai PostgreSQL lokal. Solusi: gunakan port berbeda (misal `-p 5433:5432`).
3. **Password salah** — Typo saat input environment variable `POSTGRES_PASSWORD`.
4. **Tidak mengganti driver** — TablePlus/DBeaver kadang default ke MySQL, bukan PostgreSQL.
5. **Connection timeout** — Firewall Windows memblokir port 5432. Solusi: tambahkan rule inbound.

## Soal Latihan Beserta Jawaban

### Soal 1
Buat database baru bernama `nestjs_learn` menggunakan GUI database client (TablePlus atau DBeaver) melalui koneksi ke PostgreSQL yang berjalan di Docker.

**Jawaban (TablePlus):**
1. Buka TablePlus
2. Klik "Create New Connection" → PostgreSQL
3. Isi Host: `localhost`, Port: `5432`, User: `postgres`, Password: `root`, Database: biarkan default atau isi `postgres`
4. Klik "Connect"
5. Setelah terhubung, klik kanan pada server → "New Database"
6. Isi nama: `nestjs_learn`
7. Klik "OK"
8. Database akan muncul di daftar.

**Jawaban (DBeaver):**
1. Buka DBeaver
2. Klik icon "New Database Connection" → pilih PostgreSQL
3. Isi Host: `localhost`, Port: `5432`, User: `postgres`, Password: `root`
4. Klik "Finish"
5. Klik kanan pada koneksi → "Create New Database"
6. Isi nama: `nestjs_learn`
7. Apply

### Soal 2
Apa yang terjadi jika Docker container PostgreSQL tidak di-start? Bagaimana cara mengatasinya?

**Jawaban:** Database client akan menampilkan error "Connection refused" atau "Connection timeout" karena tidak ada server yang mendengarkan di port 5432. Cara mengatasi: jalankan `docker start postgres-nestjs` atau `docker run` lagi jika container belum pernah dibuat.

### Soal 3
Tulis connection string untuk PostgreSQL di atas dalam format URI.

**Jawaban:**
```
postgresql://postgres:root@localhost:5432/nestjs_learn
```
