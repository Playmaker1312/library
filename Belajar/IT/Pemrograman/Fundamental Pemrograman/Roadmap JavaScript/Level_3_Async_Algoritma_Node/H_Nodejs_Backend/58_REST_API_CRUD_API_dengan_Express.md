# 58 — REST API: CRUD API dengan Express

## 1. Penjelasan

**REST API** adalah standar desain API yang menggunakan:
- Resource-based URL: `/api/todos`
- HTTP method sebagai operasi CRUD
- Response format JSON yang konsisten

**Struktur folder:**
```
project-rumah/
  ├── index.js         # entry point
  ├── routes/
  │   └── todos.js     # route handler todos
  │── package.json
```

**CRUD untuk Todo List:**
| Method | Endpoint | Aksi | Status Code |
|--------|----------|------|-------------|
| GET | `/api/todos` | Ambil semua todo | 200 |
| GET | `/api/todos/:id` | Ambil satu todo | 200 / 404 |
| POST | `/api/todos` | Buat todo baru | 201 |
| PUT | `/api/todos/:id` | Update todo | 200 / 404 |
| DELETE | `/api/todos/:id` | Hapus todo | 200 / 404 |

---

## 2. Fungsi

- Backend service untuk aplikasi frontend
- CRUD data (create, read, update, delete)
- API public untuk integrasi third-party
- Standar komunikasi antar microservices

---

## 3. Code — REST API Todo List

```js
// index.js
const express = require("express");
const app = express();

app.use(express.json());

// In-memory data — "catatan proyek di papan tulis"
let todos = [
  { id: 1, tugas: "Pasang pondasi", selesai: true },
  { id: 2, tugas: "Bangun dinding", selesai: false },
  { id: 3, tugas: "Pasang atap", selesai: false }
];
let nextId = 4;

// Helper: response konsisten
function respond(res, status, data, message = "") {
  res.status(status).json({ success: status < 400, message, data });
}

// GET /api/todos — ambil semua
app.get("/api/todos", (req, res) => {
  respond(res, 200, todos);
});

// GET /api/todos/:id — ambil satu
app.get("/api/todos/:id", (req, res) => {
  const todo = todos.find(t => t.id === Number(req.params.id));
  if (!todo) return respond(res, 404, null, "Todo tidak ditemukan");
  respond(res, 200, todo);
});

// POST /api/todos — buat baru
app.post("/api/todos", (req, res) => {
  const { tugas } = req.body;
  if (!tugas || typeof tugas !== "string") {
    return respond(res, 400, null, "Field 'tugas' diperlukan (string)");
  }
  const baru = { id: nextId++, tugas, selesai: false };
  todos.push(baru);
  respond(res, 201, baru, "Todo berhasil dibuat");
});

// PUT /api/todos/:id — update
app.put("/api/todos/:id", (req, res) => {
  const todo = todos.find(t => t.id === Number(req.params.id));
  if (!todo) return respond(res, 404, null, "Todo tidak ditemukan");

  const { tugas, selesai } = req.body;
  if (tugas !== undefined) todo.tugas = tugas;
  if (selesai !== undefined) todo.selesai = selesai;

  respond(res, 200, todo, "Todo berhasil diupdate");
});

// DELETE /api/todos/:id — hapus
app.delete("/api/todos/:id", (req, res) => {
  const index = todos.findIndex(t => t.id === Number(req.params.id));
  if (index === -1) return respond(res, 404, null, "Todo tidak ditemukan");

  const dihapus = todos.splice(index, 1)[0];
  respond(res, 200, dihapus, "Todo berhasil dihapus");
});

// 404 handler — route tidak dikenal
app.use((req, res) => {
  respond(res, 404, null, `Route ${req.method} ${req.url} tidak ditemukan`);
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`API Rumah jalan di http://localhost:${PORT}`);
});
```

Testing dengan `curl`:
```bash
curl http://localhost:3000/api/todos
curl -X POST http://localhost:3000/api/todos -H "Content-Type: application/json" -d "{\"tugas\":\"Cat rumah\"}"
```

---

## 4. Analogi Rumah — Standar Pelayanan Toko

| Konsep REST | Analogi Rumah |
|------------|---------------|
| Resource `/api/todos` | Rak "Pesanan Pelanggan" di toko |
| `GET /api/todos` | "Mbak, tolong lihat semua pesanan" |
| `GET /api/todos/2` | "Mbak, lihat pesanan nomor 2" |
| `POST /api/todos` | "Mbak, catat pesanan baru" |
| `PUT /api/todos/2` | "Mbak, ubah pesanan nomor 2" |
| `DELETE /api/todos/2` | "Mbak, hapus pesanan nomor 2" |
| HTTP Status Code | Ekspresi wajah kasir |
| Response JSON konsisten | Format nota standar |
| Validation | Kasir cek: "Namanya siapa?" |

---

### Cerita

Toko material bangunan punya **SOP pelayanan** yang jelas (REST API). Ada **rak pesanan** (`/api/todos`). Setiap pelanggan datang:
- "Mbak, lihat pesanan saya" → **GET** → kasir cek rak
- "Mbak, saya mau pesan bata" → **POST** → kasir tulis nota baru (201 Created)
- "Mbak, ganti bata jadi 1500" → **PUT** → kasir ubah nota
- "Mbak, batalkan pesanan" → **DELETE** → kasir sobek nota

Kalau ada yang minta lihat pesanan dengan nomor yang tidak ada, kasir bilang **"Tidak ada, Mas"** (404). Format nota (`response`) selalu sama biar gampang dibaca.

---

## 5. Use Case

- **Aplikasi Todo / Task Manager**
- **E-commerce**: CRUD produk, keranjang, order
- **Blog**: CRUD artikel, komentar
- **Dashboard**: CRUD data pengguna
- **Mobile app backend**: semua data lewat REST API

---

## 6. Kesalahan Umum

| Kesalahan | Seharusnya |
|-----------|------------|
| Status code selalu 200 | Gunakan kode yang sesuai (201, 404, 400) |
| Response format tidak konsisten | Buat helper `respond()` seperti di atas |
| Tidak validasi input | Cek `req.body` sebelum proses |
| Error tidak ditangani | `try/catch` + error handling middleware |
| ID menggunakan index array | Pakai ID unik incremental |

---

## 7. Benang Merah

Materi 57 (Express.js) → **Materi 58 (REST API CRUD)** → Materi 59 (Environment Variables)

API sudah jadi. Sekarang kita pindahkan konfigurasi ke environment variable supaya aman & fleksibel.

---

## 8. Soal & Jawaban

### Soal 1
Apa response yang tepat untuk POST yang berhasil membuat data baru?

**Jawaban:**
Status `201 Created` dengan body JSON berisi data yang baru dibuat. Contoh: `{ success: true, message: "Todo berhasil dibuat", data: { id: 4, tugas: "...", selesai: false } }`

### Soal 2
Apa beda `req.params` dan `req.body`?

**Jawaban:**
`req.params` = parameter dari URL path (contoh: `:id` di `/api/todos/:id`). `req.body` = data yang dikirim dalam body request (biasanya JSON untuk POST/PUT).

### Soal 3
Apa yang terjadi jika kita DELETE todo yang sudah dihapus sebelumnya?

**Jawaban:**
Server akan mencari ID di array, tidak ketemu, lalu mengembalikan `404 Not Found` dengan pesan "Todo tidak ditemukan". Data sudah tidak ada, jadi tidak bisa dihapus dua kali (idempotent).
