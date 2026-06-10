# 121 — Microservices: Patterns & Trade-offs

## 1. Penjelasan

Microservices memecah aplikasi menjadi service kecil yang independen.

| Aspek | Monolith | Microservices |
|-------|----------|---------------|
| Deployment | Satu unit | Setiap service deploy sendiri |
| Skala | Skala seluruh app | Skala per service (yang sibuk saja) |
| Kompleksitas | Sederhana di awal | Kompleks (network, data consistency) |
| Tim | Satu tim besar | Tim kecil per service |

## 2. Fungsi

- **Service Decomposition:** Pisah berdasarkan domain (bounded context).
- **Inter-service Communication:** Sync (gRPC/REST) vs Async (message queue).
- **Saga Pattern:** Distributed transaction tanpa 2PC — pakai choreography/orchestration.
- **API Gateway:** Entry point tunggal — routing, auth, rate limit.

## 3. Code

```javascript
// Service Buku — bertanggung jawab atas data buku
const express = require('express')
const app = express()
app.get('/api/buku', (req, res) => { res.json([{ id: 1, title: 'Laskar Pelangi' }]) })
app.listen(3001)

// Service User — bertanggung jawab atas user
const app2 = express()
app2.get('/api/user/:id', (req, res) => { res.json({ id: 1, name: 'Andi' }) })
app2.listen(3002)

// Service Peminjaman — komunikasi async via message queue
const { Queue } = require('bullmq')
const peminjamanQueue = new Queue('peminjaman')
async function pinjamBuku(userId, bukuId) {
  // Kirim event ke queue — tidak blocking
  await peminjamanQueue.add('pinjam', { userId, bukuId })
  // Service Buku akan consume dan update stok
}
```

```javascript
// Saga Pattern — Orchestration (Choreography)
// Service Peminjaman kirim event "PinjamBuku"
// Service Buku terima → kurangi stok → kirim "StokDikurangi"
// Jika gagal → kirim "StokGagal" → Service Peminjaman rollback
```

## 4. Analogi Rumah

| Architecture | Analogi Rumah |
|--------------|---------------|
| Monolith | Rumah besar — semua fungsi dalam satu bangunan |
| Microservices | Kompleks perumahan — setiap bangunan punya fungsi sendiri |
| API Gateway | Pos satpam — terima tamu, cek ID, arahkan ke blok |
| Saga Pattern | Rantai telepon antar ketua RT — informasi menyebar bertahap |

## 5. Use Case

- **E-commerce:** Service produk, keranjang, pembayaran, pengiriman — masing-masing tim sendiri.
- **Netflix:** Ratusan service — setiap service handle satu concern.
- **Startup:** Mulai dari monolith, pisah ke microservices saat tim dan traffic besar.

## 6. Kesalahan Umum

- **Premature decomposition:** Pisah service sebelum domain jelas — banyak komunikasi network.
- **Shared database:** Service pakai database sama — hilang independensi.
- **Distributed monolith:** Service saling panggil sync membentuk rantai — satu lambat, semua lambat.
- **Testing terlalu kompleks:** Butuh integration test antar service.

## 7. Benang Merah

**120 (gRPC)** sebagai protokol komunikasi → **121 (Microservices)** sebagai arsitektur. Pola di sini (Saga, API Gateway) mengarah ke **122 (Distributed Systems)** dan **123 (API Gateway)**.

## 8. Soal & Jawaban

### Soal 1
Kapan sebaiknya menggunakan monolith daripada microservices?

<details>
<summary>Jawaban</summary>
Saat tim kecil (<10 orang), domain belum matang, traffic rendah. Conway's Law — arsitektur mencerminkan struktur tim. Mulai monolith, pisah saat tim tumbuh.
</details>

### Soal 2
Apa masalah utama dari komunikasi synchronous antar microservices?

<details>
<summary>Jawaban</summary>
Coupling temporal — jika service B down, service A ikut down. Membentuk distributed monolith di mana failure merambat. Solusi: gunakan async (message queue) untuk operasi kritis.
</details>

### Soal 3
Jelaskan Saga Pattern secara singkat.

<details>
<summary>Jawaban</summary>
Saga adalah rangkaian local transaction. Setiap service melakukan transaksi dan mempublikasikan event. Jika ada yang gagal, service sebelumnya menjalankan compensating transaction (rollback).
</details>
