# 57. Express.js — Framework Web untuk Node.js

**Benang Merah**: Sebelumnya (Materi 56) kita bikin HTTP Server dengan `http` module **manual**. Kita harus routing sendiri, parse body sendiri. Express.js adalah **framework** yang menyederhanakan semua itu.

---

## Penjelasan

Express.js adalah framework web paling populer untuk Node.js. Jika `http` module ibarat **membangun rumah dengan alat manual (palu, gergaji)**, Express adalah **toolkit modern (bor listrik, gergaji mesin)** — tetap melakukan hal yang sama, tapi jauh lebih cepat dan rapi.

```javascript
// HTTP Module — manual (Materi 56)
const http = require('http');
const server = http.createServer((req, res) => {
  if (req.url === '/' && req.method === 'GET') {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Halo Dunia');
  }
});
server.listen(3000);

// Express.js — framework
const express = require('express');
const app = express();
app.get('/', (req, res) => res.send('Halo Dunia'));
app.listen(3000);
```

---

## Fungsi

Menyediakan **abstraksi** untuk HTTP server: routing, middleware, request parsing, error handling — semua yang di http module harus manual, di Express jadi **deklaratif dan terstruktur**.

---

## Cara Pengimplementasian

### 1. Setup Project
```bash
npm init -y
npm install express
```

### 2. Server Dasar
```javascript
const express = require('express');
const app = express();
const PORT = 3000;

// Route
app.get('/', (req, res) => {
  res.send('Halo dari Express!');
});

app.listen(PORT, () => {
  console.log(`Server jalan di http://localhost:${PORT}`);
});
```

### 3. Routing CRUD
```javascript
const express = require('express');
const app = express();
app.use(express.json()); // middleware untuk parse JSON body

let todos = [
  { id: 1, task: "Belajar Express", done: false },
  { id: 2, task: "Buat REST API", done: false }
];

// GET — ambil semua
app.get('/todos', (req, res) => {
  res.json(todos);
});

// POST — tambah baru
app.post('/todos', (req, res) => {
  const newTodo = {
    id: todos.length + 1,
    task: req.body.task,
    done: false
  };
  todos.push(newTodo);
  res.status(201).json(newTodo);
});

// PUT — update
app.put('/todos/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const todo = todos.find(t => t.id === id);
  if (!todo) return res.status(404).json({ error: "Not found" });
  todo.task = req.body.task || todo.task;
  todo.done = req.body.done ?? todo.done;
  res.json(todo);
});

// DELETE — hapus
app.delete('/todos/:id', (req, res) => {
  const id = parseInt(req.params.id);
  todos = todos.filter(t => t.id !== id);
  res.status(204).send();
});

app.listen(3000);
```

### 4. Middleware — Logging Sederhana
```javascript
// Middleware: fungsi yang jalan sebelum route handler
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url} — ${new Date().toISOString()}`);
  next(); // lanjut ke route handler
});

// Middleware spesifik untuk route tertentu
app.use('/api', (req, res, next) => {
  console.log('API route diakses');
  next();
});

// Error handling middleware (harus 4 parameter)
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Internal Server Error' });
});
```

---

## Analogi: Membangun Rumah (Toolkit Modern)

| Express.js | Toolkit Tukang Bangunan |
|---|---|
| `app.get('/')` | Alat khusus untuk tugas tertentu |
| `express.json()` | Mesin otomatis (mixer semen) |
| `req.params` | Cetakan ukuran (template) |
| `res.json()` | Alat finishing (hasil rapi) |
| Middleware | Quality control sebelum produk lanjut |
| Error handler | Tim darurat saat ada masalah |

Kalau `http` module manual seperti **memotong papan dengan gergaji tangan**, Express seperti **menggunakan mesin potong CNC** — hasil sama, tapi lebih cepat, presisi, dan konsisten.

---

## Dipakai Untuk Apa

- **REST API** — backbone backend modern
- **Web application** — server-side rendering
- **Middleware pipeline** — logging, auth, parsing, compression
- **Backend untuk mobile apps** — API endpoint
- **Microservices** — service kecil yang jalan di Express

---

## Kesalahan Umum

| Kesalahan | Contoh | Perbaikan |
|---|---|---|
| Lupa `express.json()` | `req.body` undefined | Tambah `app.use(express.json())` |
| Response 2 kali | `res.send()` lalu `res.json()` | Hanya 1 response per request |
| Tidak handle error | Async error bikin crash | Pakai `try/catch` atau error middleware |
| Route tidak spesifik | `app.get('/user')` dan `app.get('/user/update')` bentrok | Urutkan dari spesifik ke umum |

---

## Hubungan dengan Materi Sebelumnya

Express adalah **puncak dari perjalanan backend** sejauh ini:
- Materi 52 (Node.js fundamentals) → Environment untuk Express
- Materi 55 (HTTP) → Express menangani req/res dengan rapi
- Materi 56 (HTTP module) → Express adalah **evolusi** dari http module
- Materi 58 (REST API) → Kita akan pakai Express untuk bikin REST API

---

## Soal Latihan

### Soal 1 (Mudah)
Buat server Express dengan 2 route: `GET /` mengirim "Selamat Datang" dan `GET /about` mengirim "Tentang Kami".

**Jawaban**:
```javascript
const express = require('express');
const app = express();
const PORT = 3000;

app.get('/', (req, res) => res.send('Selamat Datang'));
app.get('/about', (req, res) => res.send('Tentang Kami'));

app.listen(PORT, () => console.log(`Server di port ${PORT}`));
```

### Soal 2 (Sedang)
Buat API todo dengan 3 route: GET semua todo, POST tambah todo, DELETE hapus todo (by ID dari query param). Data disimpan di array in-memory.

**Jawaban**:
```javascript
const express = require('express');
const app = express();
app.use(express.json());

let todos = [];
let id = 1;

app.get('/todos', (req, res) => res.json(todos));

app.post('/todos', (req, res) => {
  const todo = { id: id++, task: req.body.task, done: false };
  todos.push(todo);
  res.status(201).json(todo);
});

app.delete('/todos/:id', (req, res) => {
  todos = todos.filter(t => t.id !== parseInt(req.params.id));
  res.status(204).send();
});

app.listen(3000);
```

### Soal 3 (Tantangan)
Buat middleware logging custom yang mencatat: method, url, response time (dalam ms), lalu tambahkan ke route GET `/data`.

**Jawaban**:
```javascript
const express = require('express');
const app = express();

// Middleware logging with response time
app.use((req, res, next) => {
  const start = Date.now();
  res.on('finish', () => {
    const duration = Date.now() - start;
    console.log(`${req.method} ${req.url} — ${duration}ms`);
  });
  next();
});

app.get('/data', (req, res) => {
  setTimeout(() => {
    res.json({ message: 'Data berhasil diambil' });
  }, 200); // simulasi delay
});

app.listen(3000);
// Test: GET /data → log: "GET /data — 200ms"
```
