# 52 — Node.js Fundamentals: global, process, Buffer, __dirname

## 1. Penjelasan

Node.js adalah runtime JavaScript di **server**. Berbeda dengan browser yang punya `window` dan `document`, Node.js punya object global sendiri:

| Global Browser | Global Node.js |
|----------------|----------------|
| `window` | `global` |
| `document` | — |
| `fetch` | `global.fetch` (baru) |
| — | `process` |
| — | `Buffer` |
| — | `__dirname`, `__filename` |

**`global`** — object global tempat semua built-in berada.  
**`process`** — informasi & kontrol proses Node.js yang sedang berjalan.  
**`Buffer`** — handle data biner (file, network stream).  
**`__dirname`** — path direktori file saat ini.  
**`__filename`** — path lengkap file saat ini.

```js
// 52_global_process.js
console.log("__dirname :", __dirname);
console.log("__filename:", __filename);
console.log("process.argv:", process.argv);

const args = process.argv.slice(2);
const name = args[0] || "Tamu";
console.log(`Halo, ${name}!`);
```

---

## 2. Fungsi

- `process.argv` → baca argumen dari command line
- `process.env` → baca environment variable
- `process.exit()` → keluar dari proses
- `process.cwd()` → current working directory
- `Buffer.from()` → buat buffer dari string
- `__dirname` / `__filename` → referensi path absolut

---

## 3. Code — CLI Tool Sederhana

```js
// cli_greet.js
const args = process.argv.slice(2);
const name = args[0] || "pembeli";
const item = args[1] || "bata";

console.log(`Halo ${name}, silakan ambil ${item} di gudang!`);
console.log(`Path proyek: ${__dirname}`);
console.log(`PID proses: ${process.pid}`);

const data = Buffer.from(`Pesanan: ${item} untuk ${name}`);
console.log("Data dalam buffer:", data);
console.log("String asli:", data.toString());
```

Jalankan:
```bash
node cli_greet.js Budi "pipa paralon"
```

---

## 4. Analogi Rumah — Listrik & Pipa Terpasang

| Konsep Node.js | Analogi Rumah |
|----------------|---------------|
| `global` | Stopkontak utama — semua alat colok di sini |
| `process` | Panel listrik & meteran — monitor pemakaian |
| `__dirname` | Alamat rumah (path) — tahu posisi |
| `__filename` | Nama ruangan spesifik |
| `Buffer` | Pipa air — saluran data biner |

---

### Cerita

Di browser, kamu harus pasang sendiri kabel listrik dari tiang ke rumah (`<script src="...">`, polyfill). Di Node.js, rumah sudah dilengkapi **listrik, pipa, dan kabel internet** dari awal. Kamu tinggal colok (`require`) dan pakai. `process` adalah panel meteran yang kasih tahu berapa watt dipakai. `Buffer` adalah pipa yang ngangkut air (data biner) dari sumur ke keran.

---

## 5. Use Case

- **CLI tools**: `node cli.js --name=Andi`
- **Env config**: `process.env.PORT`
- **Binary data**: Upload file, parsing gambar
- **Path resolution**: `path.resolve(__dirname, 'public')`

---

## 6. Kesalahan Umum

| Kesalahan | Seharusnya |
|-----------|------------|
| `console.log(window)` di Node | Error — pakai `global` |
| Anggap `__dirname` selalu di root | `__dirname` sesuai folder file |
| Lupa `slice(2)` di `process.argv` | Selalu `slice(2)` untuk argumen sendiri |
| Baca `process.env` tanpa default | `process.env.PORT || 3000` |

---

## 7. Benang Merah

Materi 51 (async) → **Materi 52 (Node.js fundamentals)** → Materi 53 (File System)

Kita pindah dari browser ke server. Sekarang kita paham `process`, `__dirname`, dan `Buffer`. Next: baca/tulis file dengan `fs` module.

---

## 8. Soal & Jawaban

### Soal 1
Apa output dari `node script.js` jika script hanya `console.log(process.argv)`?

**Jawaban:**
Array dengan dua elemen: path node executable dan path script.js. Tidak ada argumen tambahan.

### Soal 2
Buat script yang menerima `--name` dan `--age` lalu mencetak `"Nama: X, Umur: Y"`.

**Jawaban:**
```js
const args = process.argv.slice(2);
const nameFlag = args.indexOf("--name");
const ageFlag = args.indexOf("--age");
const name = nameFlag !== -1 ? args[nameFlag + 1] : "Unknown";
const age = ageFlag !== -1 ? args[ageFlag + 1] : "?";
console.log(`Nama: ${name}, Umur: ${age}`);
```

### Soal 3
Apa beda `__dirname` dan `process.cwd()`?

**Jawaban:**
`__dirname` = path folder tempat file JS berada. `process.cwd()` = folder tempat user menjalankan `node`. Bisa berbeda jika script dijalankan dari folder lain.
