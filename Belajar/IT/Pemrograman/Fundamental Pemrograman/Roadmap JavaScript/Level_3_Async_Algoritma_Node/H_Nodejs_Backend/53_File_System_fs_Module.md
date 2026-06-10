# 53 — File System (fs) Module

## 1. Penjelasan

Module `fs` (File System) menyediakan API untuk berinteraksi dengan file dan folder di sistem operasi.

**Mode akses:**
- **Async (callback)**: `fs.readFile(path, cb)` — non-blocking
- **Async (promise)**: `fs.promises.readFile(path)` — pakai `async/await`
- **Sync**: `fs.readFileSync(path)` — blocking, hanya untuk startup

**Operasi utama:**
| Operasi | Async | Sync |
|---------|-------|------|
| Baca file | `fs.readFile` | `fs.readFileSync` |
| Tulis file | `fs.writeFile` | `fs.writeFileSync` |
| Tambah file | `fs.appendFile` | `fs.appendFileSync` |
| Hapus file | `fs.unlink` | `fs.unlinkSync` |
| Buat folder | `fs.mkdir` | `fs.mkdirSync` |
| Baca folder | `fs.readdir` | `fs.readdirSync` |
| Cek ada? | `fs.access` | `fs.existsSync` |

---

## 2. Fungsi

- **Logging** — catat aktivitas ke file
- **Config file** — baca file JSON konfigurasi
- **Upload/download** — baca/tulis file dari request
- **Data persistence** — simpan data sebelum punya database

---

## 3. Code — Sistem Log Sederhana

```js
// logger.js
const fs = require("fs");
const path = require("path");

const LOG_DIR = path.join(__dirname, "logs");
const LOG_FILE = path.join(LOG_DIR, "aktivitas.log");

// Buat folder logs kalau belum ada
if (!fs.existsSync(LOG_DIR)) {
  fs.mkdirSync(LOG_DIR, { recursive: true });
  console.log("Folder logs dibuat.");
}

function catatAktivitas(pesan) {
  const timestamp = new Date().toISOString();
  const entry = `[${timestamp}] ${pesan}\n`;
  fs.appendFile(LOG_FILE, entry, (err) => {
    if (err) console.error("Gagal menulis log:", err);
    else console.log("Log tersimpan:", entry.trim());
  });
}

// Contoh penggunaan
catatAktivitas("Pintu rumah dibuka");
catatAktivitas("Batu bata diangkut ke lantai 2");
catatAktivitas("Listrik lantai 1 dinyalakan");

// Baca log
setTimeout(() => {
  fs.readFile(LOG_FILE, "utf-8", (err, data) => {
    if (err) return console.error("Gagal baca:", err);
    console.log("\n=== ISI LOG ===");
    console.log(data);
  });
}, 500);
```

---

## 4. Analogi Rumah — Lemari Arsip

| Konsep fs | Analogi Rumah |
|-----------|---------------|
| `fs.readFile` | Buka lemari, ambil map, baca isinya |
| `fs.writeFile` | Tulis catatan baru, simpan di lemari |
| `fs.appendFile` | Tambah halaman ke map yang sudah ada |
| `fs.unlink` | Buang map ke tempat sampah |
| `fs.mkdir` | Pasang lemari baru di ruangan |
| `fs.readdir` | Lihat daftar map di dalam lemari |
| `fs.existsSync` | Cek apakah lemari sudah ada |

---

### Cerita

Bayangin kamu lagi bangun rumah. Kamu punya **lemari arsip** besar di ruang kerja. Setiap kali tukang selesai kerja, kamu catat di map (`fs.appendFile`). Kalau mau lihat progres, kamu buka map (`fs.readFile`). Kalau mau bikin kategori baru, kamu pasang lemari baru (`fs.mkdir`). Semua catatan proyek rumah tersimpan rapi.

---

## 5. Use Case

- **Aplikasi catatan** — simpan/ambil note dari file
- **Log server** — catat request ke file
- **Config file** — baca `config.json` di startup
- **Static file server** — baca file HTML/CSS/JS dari disk
- **Build tools** — baca file sumber, tulis file hasil build

---

## 6. Kesalahan Umum

| Kesalahan | Seharusnya |
|-----------|------------|
| `readFile` tanpa encoding → return buffer | `fs.readFile(path, 'utf-8', cb)` |
| Lupa handle error callback | Selalu cek `err` di callback |
| Path relatif tergantung `cwd()` | Pakai `path.join(__dirname, ...)` |
| `writeFile` hapus isi lama | Pakai `appendFile` atau `flag: 'a'` |
| Sync di loop → blocking | Lebih baik baca sekali atau pakai stream |

---

## 7. Benang Merah

Materi 52 (Node.js globals) → **Materi 53 (File System)** → Materi 54 (npm)

Kita sudah bisa baca/tulis file. Sekarang kita belajar kelola package dengan npm supaya project makin rapi.

---

## 8. Soal & Jawaban

### Soal 1
Apa beda `fs.readFile` dan `fs.readFileSync`?

**Jawaban:**
`readFile` async — tidak blocking, jalankan callback setelah selesai. `readFileSync` blocking — kode berhenti sampai file selesai dibaca. Async untuk production, sync untuk startup/script sederhana.

### Soal 2
Buat fungsi yang membaca file `data.json` lalu mencetak isinya sebagai object.

**Jawaban:**
```js
const fs = require("fs");
fs.readFile("data.json", "utf-8", (err, data) => {
  if (err) return console.error(err);
  const obj = JSON.parse(data);
  console.log(obj);
});
```

### Soal 3
Kenapa kita pakai `path.join(__dirname, 'logs')` bukan `'./logs'`?

**Jawaban:**
`__dirname` memberikan path absolut. `'./logs'` relatif terhadap `process.cwd()` (tempat node dijalankan). Kalau script dijalankan dari folder berbeda, path relatif bisa salah. `__dirname` lebih aman.
