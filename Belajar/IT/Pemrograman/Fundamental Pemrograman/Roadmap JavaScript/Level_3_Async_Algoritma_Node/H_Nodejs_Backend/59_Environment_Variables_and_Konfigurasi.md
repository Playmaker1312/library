# 59 — Environment Variables & Konfigurasi

## 1. Penjelasan

**Jangan hardcode konfigurasi** di kode. Pisahkan konfigurasi ke **environment variable** (env var).

**Cara kerja:**
```
Kode           → process.env.PORT
                 process.env.DB_HOST
                 process.env.NODE_ENV

File .env      → PORT=3000
                 DB_HOST=localhost
                 NODE_ENV=development

OS langsung    → set PORT=5000 (Windows)
                 export PORT=5000 (Linux/Mac)
```

**Package `dotenv`:** membaca file `.env` dan memasukkan nilainya ke `process.env`.

**Environment:**
| NODE_ENV | Tujuan |
|----------|--------|
| `development` | Coding, debug detail, log lengkap |
| `staging` | Uji coba pra-production |
| `production` | Optimal, minim log, error saja |

---

## 2. Fungsi

- **Keamanan** — API key, password tidak di-commit ke git
- **Fleksibilitas** — ganti konfigurasi tanpa ubah kode
- **Portability** — kode sama, konfigurasi beda tiap environment

---

## 3. Code — Konfigurasi dengan dotenv

```bash
npm install dotenv
```

```env
# .env — jangan di-commit ke git!
PORT=4000
NODE_ENV=development
DB_HOST=localhost
DB_PORT=27017
DB_NAME=rumah_impian
SECRET_KEY=rahasia123
LOG_LEVEL=debug
```

```js
// index.js
require("dotenv").config();

const express = require("express");
const app = express();

// Semua konfigurasi dari env vars
const config = {
  port: process.env.PORT || 3000,
  nodeEnv: process.env.NODE_ENV || "development",
  dbHost: process.env.DB_HOST || "localhost",
  dbPort: process.env.DB_PORT || 27017,
  dbName: process.env.DB_NAME || "app",
  secretKey: process.env.SECRET_KEY,
  logLevel: process.env.LOG_LEVEL || "info"
};

console.log("=== KONFIGURASI RUMAH ===");
console.log(`Mode: ${config.nodeEnv}`);
console.log(`Server: http://localhost:${config.port}`);
console.log(`Database: ${config.dbHost}:${config.dbPort}/${config.dbName}`);
console.log(`Log level: ${config.logLevel}`);

// Contoh endpoint yang pakai env
app.get("/api/config", (req, res) => {
  // Jangan expose secret key!
  res.json({
    nodeEnv: config.nodeEnv,
    dbHost: config.dbHost,
    port: config.port
  });
});

app.listen(config.port, () => {
  console.log(`Server jalan di port ${config.port} (${config.nodeEnv})`);
});
```

```gitignore
# .gitignore
.env
node_modules/
```

```env
# .env.example — contoh, di-commit ke git
PORT=3000
NODE_ENV=development
DB_HOST=localhost
DB_PORT=27017
DB_NAME=myapp
LOG_LEVEL=info
```

---

## 4. Analogi Rumah — Panel Kontrol Rumah

| Konsep Env Var | Analogi Rumah |
|----------------|---------------|
| `process.env` | Panel kontrol rumah |
| `.env` file | Buku panduan panel kontrol |
| `PORT` | Saklar utama (berapa volt) |
| `NODE_ENV` | Mode: ada orang / tidak ada orang |
| `DB_HOST` | Alamat pipa air utama |
| `SECRET_KEY` | Kunci gudang belakang |
| `dotenv` config | Orang yang menyalakan panel |
| `.gitignore` | Lemari terkunci — jangan tunjukkan ke tamu |
| `.env.example` | Contoh diagram panel (tanpa kunci) |

---

### Cerita

Setiap rumah punya **panel kontrol** — tempat semua saklar, colokan, dan meteran terpusat. Di panel itu ada:
- **Saklar utama** (`PORT`) — atur tegangan listrik rumah
- **Mode rumah** (`NODE_ENV`) — "liburan" (production) atau "ada pesta" (development)
- **Sambungan pipa** (`DB_HOST`) — ke PDAM mana rumah tersambung
- **Kunci gudang** (`SECRET_KEY`) — hanya kamu yang tahu, tidak pernah ditulis di buku tamu

Kalau kamu mau ganti tegangan listrik, kamu **tidak perlu bongkar kabel** di seluruh rumah. Cukup buka panel, putar saklar (`ganti .env`). Kode di dalam rumah tetap sama.

---

## 5. Use Case

- **API keys**: `process.env.STRIPE_SECRET_KEY`
- **Database URL**: `process.env.DATABASE_URL`
- **Port server**: `process.env.PORT` (Heroku/Railway kasih ini otomatis)
- **Feature toggle**: `process.env.FEATURE_X_ENABLED`
- **Environment-specific**: beda konfigurasi dev vs production

---

## 6. Kesalahan Umum

| Kesalahan | Seharusnya |
|-----------|------------|
| `.env` di-commit ke git | Tambahkan ke `.gitignore` |
| Lupa `require("dotenv").config()` | dotenv tidak otomatis jalan |
| Akses `process.env.SECRET` bisa `undefined` | Selalu kasih default atau guard |
| Simpan .env di public folder | Simpan di root project |
| Bedakan `.env` untuk tiap env | Pakai `.env.development`, `.env.production` |

---

## 7. Benang Merah

Materi 58 (REST API) → **Materi 59 (Environment Variables)** → Materi 60 (Database)

Konfigurasi sudah terpusat dan aman. Sekarang kita siap connect ke database beneran (MongoDB, PostgreSQL, dll).

---

## 8. Soal & Jawaban

### Soal 1
Kenapa `.env` tidak boleh di-commit ke git?

**Jawaban:**
`.env` berisi rahasia seperti API key, password database, secret key. Kalau tercommit, semua orang yang akses repo bisa lihat. Gunakan `.env.example` sebagai template.

### Soal 2
Apa yang terjadi kalau lupa panggil `require("dotenv").config()` di paling atas file?

**Jawaban:**
`process.env.PORT` akan `undefined` — semua env var tidak terbaca. Server bisa jalan di port `undefined` (error) atau pakai default yang ditentukan.

### Soal 3
Bagaimana caranya mensimulasikan production environment di lokal?

**Jawaban:**
Buat file `.env.production` dengan nilai production, lalu jalankan dengan:
```bash
NODE_ENV=production node -r dotenv/config index.js dotenv_config_path=.env.production
```
Atau buat script di `package.json`:
```json
"scripts": {
  "start:prod": "NODE_ENV=production node index.js"
}
```
