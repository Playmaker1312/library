# 54 — npm: Package Manager

## 1. Penjelasan

**npm** (Node Package Manager) adalah:
- **Registry** — toko online berisi ~2 juta package JavaScript
- **CLI tool** — `npm install`, `npm run`, dll
- **package.json** — file konfigurasi project + daftar dependensi

**File penting:**
| File | Fungsi |
|------|--------|
| `package.json` | Nama project, versi, scripts, dependencies |
| `package-lock.json` | Versi persis tiap package (lock) |
| `node_modules/` | Folder tempat package terinstall |

**Jenis dependensi:**
| `dependencies` | `devDependencies` |
|----------------|-------------------|
| Dibutuhkan di production | Hanya untuk development |
| Contoh: `express` | Contoh: `nodemon`, `jest` |
| Install dengan `--save-prod` (default) | Install dengan `--save-dev` |

**Semantic Versioning (SemVer):** `MAJOR.MINOR.PATCH`
- `^2.3.0` → minor/patch boleh naik
- `~2.3.0` → hanya patch
- `2.3.0` → exact

---

## 2. Fungsi

- **Manajemen dependensi** — install, update, hapus package
- **Script runner** — `npm run start`, `npm run dev`
- **Versioning** — kontrol versi package project
- **Distribusi** — publish package sendiri ke registry

---

## 3. Code — Setup Project dengan npm

```bash
# Inisialisasi project
npm init -y

# Install dependensi utama
npm install chalk

# Install devDependencies
npm install --save-dev nodemon
```

```json
// package.json hasil inisialisasi
{
  "name": "project-rumah",
  "version": "1.0.0",
  "description": "Project manajemen pembangunan rumah",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  },
  "dependencies": {
    "chalk": "^5.3.0"
  },
  "devDependencies": {
    "nodemon": "^3.0.0"
  }
}
```

```js
// index.js — menggunakan chalk
import chalk from "chalk";

console.log(chalk.green("=== PROGRESS RUMAH ==="));
console.log(chalk.blue("Pondasi") + " selesai 100%");
console.log(chalk.yellow("Dinding") + " selesai 60%");
console.log(chalk.red("Atap") + " belum dimulai");
```

---

## 4. Analogi Rumah — Toko Material Bangunan

| Konsep npm | Analogi Rumah |
|-----------|---------------|
| `npm init` | Buka proyek rumah baru, siapkan buku catatan |
| `package.json` | Buku catatan proyek — daftar belanja |
| `npm install` | Pergi ke toko material, beli barang |
| `node_modules` | Gudang penyimpanan material |
| `dependencies` | Material utama (semen, bata) |
| `devDependencies` | Alat bantu (bor, palu) — dipakai pas bangun, disimpan setelah |
| `npm run` | Perintah ke tukang: "Kerjakan tahap X" |
| `package-lock.json` | Nota belanja — bukti persis barang yang dibeli |

---

### Cerita

Kamu mau bangun rumah. Pertama kamu **buka buku proyek** (`npm init`). Di buku itu kamu catat: "butuh semen 50 sak, bata 1000 biji" (`dependencies`). Kamu juga catat: "sewa bor listrik, beli palu" (`devDependencies`). Kamu pergi ke **toko material** (`npm install`), beli semua. Barang ditaruh di **gudang** (`node_modules/`). Nota toko kamu simpan (`package-lock.json`) — kalau nanti ada masalah, kamu tahu persis barang yang dibeli.

---

## 5. Use Case

- **Setup project baru**: `npm init -y`, lalu `npm install express`
- **Development**: `npm install --save-dev nodemon eslint`
- **Deploy**: `npm ci` (clean install) di server production
- **Update package**: `npm update` atau `npm outdated` cek versi lama

---

## 6. Kesalahan Umum

| Kesalahan | Seharusnya |
|-----------|------------|
| `node_modules/` di-commit ke git | Tambahkan `.gitignore` |
| Lupa `--save-dev` | Dev package masuk `dependencies` |
| `npm install -g` tanpa sudo/admin | Hindari global, pakai `npx` |
| Delete `node_modules` manual | Pakai `npm ci` untuk install ulang |
| Tidak commit `package-lock.json` | Lock file wajib di-commit |

---

## 7. Benang Merah

Materi 53 (fs) → **Materi 54 (npm)** → Materi 55 (HTTP)

Project kita sudah punya package manager. Sekarang siap membuat web server dengan module HTTP.

---

## 8. Soal & Jawaban

### Soal 1
Apa beda `npm install express` dan `npm install --save-dev express`?

**Jawaban:**
Yang pertama masuk ke `dependencies` (butuh di production). Yang kedua masuk ke `devDependencies` (hanya untuk development).

### Soal 2
Apa fungsi `package-lock.json`?

**Jawaban:**
Mengunci versi persis setiap package dan dependensi turunannya. Memastikan semua developer dan server production punya versi package yang sama persis.

### Soal 3
Kapan pakai `npm ci` bukan `npm install`?

**Jawaban:**
`npm ci` dipakai di CI/CD atau production. Lebih cepat karena skip resolusi versi, hapus `node_modules` dulu, dan install persis dari `package-lock.json`. Gagal kalau `package-lock.json` tidak cocok dengan `package.json`.
