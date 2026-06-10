# 56 — HTTP Server dengan Node.js — Tanpa Framework

## 1. Penjelasan

Module `http` di Node.js bisa bikin web server tanpa framework (Express, Fastify, dll). Kita handle semuanya manual:
- `http.createServer(callback)` — buat server
- `req` (IncomingMessage) — object request
- `res` (ServerResponse) — object response
- Routing manual dengan `req.url` + `req.method`
- Parsing body manual

**Struktur dasar:**
```js
const http = require("http");

const server = http.createServer((req, res) => {
  // req = request dari client
  // res = response ke client
});

server.listen(3000, () => console.log("Server jalan di 3000"));
```

---

## 2. Fungsi

- **Memahami inner working** web server sebelum pakai framework
- **Aplikasi mikro** — cukup 1 file, tanpa dependensi
- **Learning tool** — paham konsep routing, middleware, parsing body

---

## 3. Code — Server Sederhana 3 Route

```js
// server.js
const http = require("http");
const url = require("url");

const server = http.createServer((req, res) => {
  const parsedUrl = url.parse(req.url, true);
  const path = parsedUrl.pathname;
  const method = req.method.toUpperCase();

  // Helper kirim JSON response
  const kirimJSON = (status, data) => {
    res.writeHead(status, { "Content-Type": "application/json" });
    res.end(JSON.stringify(data));
  };

  // --- ROUTING MANUAL ---
  if (path === "/" && method === "GET") {
    kirimJSON(200, {
      message: "Selamat datang di proyek rumah!",
      routes: ["/", "/about", "/api/data"]
    });

  } else if (path === "/about" && method === "GET") {
    kirimJSON(200, {
      nama: "Proyek Rumah Impian",
      versi: "1.0.0",
      status: "Pondasi selesai, dinding 60%"
    });

  } else if (path === "/api/data" && method === "GET") {
    kirimJSON(200, {
      material: [
        { nama: "Bata", jumlah: 1000, satuan: "biji" },
        { nama: "Semen", jumlah: 50, satuan: "sak" },
        { nama: "Pasir", jumlah: 10, satuan: "kubik" }
      ],
      tukang: [
        { nama: "Budi", peran: "Kepala Tukang" },
        { nama: "Andi", peran: "Tukang Batu" }
      ]
    });

  } else {
    // Route tidak ditemukan
    kirimJSON(404, { error: "Route tidak ditemukan", path, method });
  }
});

const PORT = 3000;
server.listen(PORT, () => {
  console.log(`Server rumah jalan di http://localhost:${PORT}`);
});
```

Coba:
```bash
node server.js
# buka browser: http://localhost:3000
# http://localhost:3000/about
# http://localhost:3000/api/data
```

---

## 4. Analogi Rumah — Bangun dari Nol

| Konsep | Analogi Rumah |
|--------|---------------|
| `http.createServer` | Siapkan lahan kosong |
| `req` | Surat/telepon dari pembeli |
| `res` | Balasan surat ke pembeli |
| `req.url` | Alamat tujuan di dalam rumah |
| `req.method` | Jenis permintaan (tanya, minta, dll) |
| Routing manual `if/else` | Kamu sendiri yang jadi resepsionis |
| `res.writeHead` | Tulis kop surat |
| `res.end` | Kirim surat balasan |

---

### Cerita

Kamu bangun rumah **tanpa kontraktor** — kamu sendiri yang jadi tukang, arsitek, dan mandor. Kamu potong kayu sendiri (`createServer`), pasang paku sendiri. Setiap ada tamu datang (`request`), kamu langsung sambut, tanya maunya apa (`req.url`), terus ambilkan (`res`). Tidak ada resepsionis (framework). Semua manual, tapi kamu **paham betul** setiap sudut rumah.

---

## 5. Use Case

- **Mock server** — testing API palsu
- **Embedded device** — IoT dengan resource terbatas
- **Microservice sederhana** — 1-2 endpoint, tidak perlu Express
- **Pendidikan** — belajar fundamental HTTP server

---

## 6. Kesalahan Umum

| Kesalahan | Seharusnya |
|-----------|------------|
| `res.end()` tanpa header | `res.writeHead()` dulu |
| Lupa `return` setelah `res.end()` | Kode lanjut jalan & error |
| Body request tidak di-parse | Kumpulkan `data` event, lalu `end` |
| Asal path `/api/data/` beda dengan `/api/data` | Normalize trailing slash |
| Tidak handle error | `server.on('error', handler)` |

---

## 7. Benang Merah

Materi 55 (HTTP theory) → **Materi 56 (HTTP Server manual)** → Materi 57 (Express.js — sudah ada)

Dari routing manual pakai `if/else`, kita naik ke Express.js yang lebih rapi. Tapi sekarang kita tahu apa yang terjadi "di balik layar" Express.

---

## 8. Soal & Jawaban

### Soal 1
Apa output jika client request `GET /api/tidak-ada` ke server di atas?

**Jawaban:**
Response JSON 404: `{ "error": "Route tidak ditemukan", "path": "/api/tidak-ada", "method": "GET" }`

### Soal 2
Bagaimana cara membaca body dari POST request di server Node.js tanpa framework?

**Jawaban:**
Dengarkan event `data` untuk mengumpulkan chunk, lalu event `end` untuk menggabungkan:
```js
let body = "";
req.on("data", chunk => body += chunk);
req.on("end", () => {
  const data = JSON.parse(body);
  // proses data
});
```

### Soal 3
Apa fungsi `res.writeHead(200, { "Content-Type": "application/json" })`?

**Jawaban:**
Menulis status code (200) dan response header (content type JSON) ke response. Sama seperti amplop surat — menulis "Balasan" dan "Isi: JSON" di amplop sebelum surat dimasukkan.
