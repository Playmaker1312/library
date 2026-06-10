# 55 — HTTP & Web

## 1. Penjelasan

**HTTP** (HyperText Transfer Protocol) adalah protokol komunikasi antara client (browser) dan server.

**Arsitektur Client-Server:**

```
Client (Browser)          Server
     |                       |
     | ---- HTTP Request --> |
     |                       |
     | <-- HTTP Response --- |
     |                       |
```

**HTTP Methods:**
| Method | Fungsi | CRUD |
|--------|--------|------|
| `GET` | Ambil data | Read |
| `POST` | Kirim data baru | Create |
| `PUT` | Ganti data (full) | Update |
| `PATCH` | Ubah sebagian | Update |
| `DELETE` | Hapus data | Delete |

**Status Code:**
| Range | Arti | Contoh |
|-------|------|--------|
| 2xx | Sukses | 200 OK, 201 Created |
| 3xx | Redirect | 301 Moved Permanently |
| 4xx | Client error | 404 Not Found, 400 Bad Request |
| 5xx | Server error | 500 Internal Server Error |

**REST Principles:**
- Resource-based URL: `/api/rumah`, `/api/rumah/1`
- HTTP method sebagai aksi
- Stateless — tiap request berdiri sendiri
- Response JSON

---

## 2. Fungsi

- Memahami cara kerja komunikasi web
- Mendesain API endpoint yang baik
- Debugging request/response
- Integrasi frontend-backend

---

## 3. Code — Siklus Request-Response CRUD

```js
// 55_http_crud.js (komentar siklus)
// Bayangkan ini server bangunan rumah

// CLIENT mengirim REQUEST:
// GET  /api/rumah          → lihat daftar rumah
// POST /api/rumah          → tambah rumah baru
// GET  /api/rumah/1        → lihat detail rumah id=1
// PUT  /api/rumah/1        → update seluruh data rumah id=1
// DELETE /api/rumah/1      → hapus rumah id=1

// SERVER memproses:
const request = {
  method: "POST",           // method HTTP
  url: "/api/rumah",        // endpoint
  headers: {                // header
    "Content-Type": "application/json",
    "Authorization": "Bearer token123"
  },
  body: {                   // body (untuk POST/PUT/PATCH)
    nama: "Rumah Budi",
    luasTanah: 120,
    jumlahLantai: 2
  }
};

// SERVER mengirim RESPONSE:
const response = {
  statusCode: 201,          // Created
  headers: {
    "Content-Type": "application/json"
  },
  body: {
    success: true,
    message: "Rumah berhasil dicatat",
    data: {
      id: 1,
      nama: "Rumah Budi",
      luasTanah: 120,
      jumlahLantai: 2
    }
  }
};

console.log("Siklus HTTP:", request.method, request.url);
console.log("Response:", response.statusCode, response.body.message);
```

---

## 4. Analogi Rumah — Pesan Material ke Toko

| Konsep HTTP | Analogi Rumah |
|-------------|---------------|
| Client | Kamu (pemilik rumah) |
| Server | Toko material bangunan |
| Request | Kamu telepon/surat ke toko |
| Response | Toko balas |
| GET | "Tolong cek stok bata" |
| POST | "Tolong catat pesanan baru" |
| PUT | "Ubah pesanan saya — ganti semua item" |
| PATCH | "Ubah jumlah bata saja" |
| DELETE | "Batalkan pesanan" |
| 200 OK | "Ini barangnya, Kak" |
| 404 Not Found | "Barang tidak tersedia" |
| 500 Server Error | "Toko kebakaran, maaf" |

---

### Cerita

Kamu mau bangun rumah, butuh material. Kamu **kirim surat** (request) ke toko bangunan: "Tolong kirim 1000 bata merah" (POST). Toko **balas surat** (response): "Baik, pesanan diterima. Nomor pesanan: #123" (201 Created). Nanti kamu mau **cek status** (GET /pesanan/123). Toko balas: "Barang sedang diantar" (200 OK). Mau **ubah jumlah** (PATCH /pesanan/123): "Jadi 1200 bata". Toko balas: "OK, diubah" (200 OK). Mau **batal** (DELETE /pesanan/123): "Batalkan". Toko balas: "Pesanan dibatalkan" (200 OK).

---

## 5. Use Case

- **Web browsing** — browser GET halaman
- **REST API** — backend menyediakan data untuk frontend
- **Microservices** — service A HTTP call service B
- **Third-party API** — integrasi dengan layanan lain (payment, shipping)

---

## 6. Kesalahan Umum

| Kesalahan | Seharusnya |
|-----------|------------|
| PUT untuk perubahan kecil | Pakai PATCH |
| Response selalu 200 | Gunakan status code sesuai konteks |
| GET dengan body | GET tidak punya body |
| Password di URL GET | Pakai POST atau header |
| Lupa handle error response | Selalu cek `res.ok` / status code client |

---

## 7. Benang Merah

Materi 54 (npm) → **Materi 55 (HTTP)** → Materi 56 (HTTP Server)

Teori HTTP sudah paham. Sekarang kita implementasi server HTTP dari nol tanpa framework.

---

## 8. Soal & Jawaban

### Soal 1
Sebutkan method HTTP yang tepat untuk: membuat data, mengambil data, mengubah seluruh data, dan menghapus data.

**Jawaban:**
Create → POST, Read → GET, Update (full) → PUT, Delete → DELETE.

### Soal 2
Apa beda 404 dan 500?

**Jawaban:**
404 = client error, resource tidak ditemukan (salah client). 500 = server error, ada masalah di server (salah server).

### Soal 3
Kenapa REST API sebaiknya **stateless**?

**Jawaban:**
Setiap request harus mengandung semua informasi yang diperlukan. Server tidak menyimpan state client antar request. Ini membuat server mudah diskalakan (scaling horizontal) dan lebih sederhana.
