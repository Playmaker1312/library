# 123 — API Gateway, Rate Limiting & Load Balancing

## 1. Penjelasan

Tiga komponen penting di depan microservices:

| Komponen | Fungsi |
|----------|--------|
| **API Gateway** | Entry point tunggal — routing, auth, logging |
| **Rate Limiting** | Batasi jumlah request per user dalam waktu tertentu |
| **Load Balancing** | Distribusi traffic ke beberapa instance server |

## 2. Fungsi

- **API Gateway:** Single entry → routing ke service yang tepat, agregasi respons, transformasi protokol.
- **Rate Limiting (Token Bucket / Sliding Window):** Cegah abuse, fair usage.
- **Load Balancing (Round-Robin / Least Connections):** Skala horizontal, hindari overload satu server.

## 3. Code

```javascript
// Custom API Gateway dengan Express
const express = require('express')
const { createProxyMiddleware } = require('http-proxy-middleware')
const rateLimit = require('express-rate-limit')

const gateway = express()

// Rate limiting — token bucket style
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 menit
  max: 100, // max 100 request per window
  message: { error: 'Too many requests, slow down!' }
})
gateway.use(limiter)

// Routing ke service
gateway.use('/buku', createProxyMiddleware({
  target: 'http://localhost:3001',
  changeOrigin: true
}))
gateway.use('/user', createProxyMiddleware({
  target: 'http://localhost:3002',
  changeOrigin: true
}))

gateway.listen(8080)
```

```javascript
// Load balancing sederhana — Round Robin
const servers = ['http://localhost:3001', 'http://localhost:3002', 'http://localhost:3003']
let current = 0

function getNextServer() {
  const server = servers[current]
  current = (current + 1) % servers.length
  return server
}

// Gunakan di proxy
const target = getNextServer()
```

## 4. Analogi Rumah

| Komponen | Analogi Rumah |
|----------|---------------|
| API Gateway | Resepsionis gedung — terima tamu, cek ID, arahkan ke lantai |
| Rate Limiting | Batasi jumlah tamu yang masuk per jam |
| Load Balancing | Arahkan tamu ke lift yang paling kosong |
| Auth di Gateway | Cek KTP sebelum masuk |

## 5. Use Case

- **API Gateway di e-commerce:** `/products` → service produk, `/orders` → service order.
- **Rate limiting:** API publik — 100 req/menit untuk free tier, 1000 untuk premium.
- **Load balancing:** Black Friday — traffic spike, distribusi ke 10 instance backend.

## 6. Kesalahan Umum

- **Gateway jadi bottleneck:** Semua traffic lewat satu gateway — skalakan juga.
- **Rate limit tanpa user context:** Rate limit per IP tidak adil untuk pengguna di belakang NAT.
- **Load balancing tanpa health check:** Traffic tetap dikirim ke server yang sudah down.

## 7. Benang Merah

Setelah memahami trade-off distributed di **122 (CAP Theorem)**, kita butuh komponen yang mengatur lalu lintas — **123 (API Gateway)**. Ini adalah enabler untuk **124 (Event-Driven Architecture)**.

## 8. Soal & Jawaban

### Soal 1
Apa keuntungan utama menggunakan API Gateway?

<details>
<summary>Jawaban</summary>
Single entry point — client tidak perlu tahu alamat masing-masing service. Gateway handle cross-cutting concerns: auth, logging, rate limiting, routing.
</details>

### Soal 2
Jelaskan perbedaan Token Bucket dan Sliding Window untuk rate limiting.

<details>
<summary>Jawaban</summary>
Token Bucket: token ditambahkan per detik — request menghabiskan token. Sliding Window: hitung request dalam jendela waktu geser — lebih akurat mencegah spike.
</details>

### Soal 3
Apa yang terjadi jika load balancer tidak punya health check?

<details>
<summary>Jawaban</summary>
Traffic tetap dikirim ke server yang crash → request gagal. Health check memastikan traffic hanya ke server sehat.
</details>
