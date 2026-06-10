# 118 — Message Queue: RabbitMQ & Apache Kafka

## 1. Penjelasan

Message Queue (MQ) adalah middleware untuk komunikasi asynchronous antar service.

| Konsep | Deskripsi |
|--------|-----------|
| **Producer** | Mengirim pesan ke queue |
| **Consumer** | Menerima & memproses pesan dari queue |
| **Broker** | Server yang mengelola queue (RabbitMQ, Kafka) |
| **Decoupling** | Producer tidak perlu tahu siapa consumer |

## 2. Fungsi

- **Buffering:** Menahan lonjakan traffic — pesan antre, diproses perlahan.
- **Decoupling:** Microservices tidak saling panggil langsung.
- **Reliability:** Pesan tidak hilang meski consumer crash (aknowledgment).
- **RabbitMQ:** Cocok untuk routing kompleks (exchange → binding → queue).
- **Kafka:** Cocok untuk high throughput, event log, replay.

## 3. Code

```javascript
// BullMQ — Redis-based queue untuk Node.js
const { Queue, Worker } = require('bullmq')
const connection = { host: 'localhost', port: 6379 }

// Producer — kirim job
const queue = new Queue('peminjaman', { connection })
await queue.add('pinjam-buku', {
  userId: 1, bukuId: 42, durasi: 7
})

// Consumer — proses job
const worker = new Worker('peminjaman', async job => {
  console.log(`Memproses: ${job.data.userId} pinjam buku ${job.data.bukuId}`)
  // logika: cek stok, kurangi, catat
}, { connection })
```

```javascript
// RabbitMQ dengan amqplib
const amqp = require('amqplib')
const conn = await amqp.connect('amqp://localhost')
const ch = await conn.createChannel()

// Producer
await ch.assertQueue('peminjaman')
ch.sendToQueue('peminjaman', Buffer.from(JSON.stringify({ userId: 1 })))

// Consumer
ch.consume('peminjaman', msg => {
  const data = JSON.parse(msg.content.toString())
  console.log('Proses:', data)
  ch.ack(msg)
})
```

## 4. Analogi Rumah

| MQ Concept | Analogi Rumah |
|------------|---------------|
| Producer | Pengirim surat |
| Queue | Kantor pos |
| Consumer | Tukang pos yang antar surat |
| Broker | Manajer kantor pos (atur alur surat) |
| Ack | Tanda terima — surat sudah diterima |

## 5. Use Case

- **RabbitMQ:** Order processing — routing ke queue berbeda berdasarkan tipe order.
- **Kafka:** Event streaming — log aktivitas user, real-time analytics.
- **BullMQ:** Job queue Node.js — background job (email, notifikasi, export PDF).

## 6. Kesalahan Umum

- **Lupa acknowledge:** Pesan tetap dianggap belum diproses → menumpuk di queue.
- **Queue tanpa batas:** Backlog tak terbatas saat consumer lambat → out-of-memory.
- **Memilih broker salah:** Pakai Kafka untuk 10 msg/detik (overkill) — pakai RabbitMQ/BullMQ saja.

## 7. Benang Merah

**117 (Redis Pub/Stream)** adalah pengantar messaging → **118 (Message Queue)** memperkenalkan broker dedicated. Ini menjadi pondasi komunikasi antar service di **121 (Microservices)**.

## 8. Soal & Jawaban

### Soal 1
Apa perbedaan mendasar antara RabbitMQ dan Kafka?

<details>
<summary>Jawaban</summary>
RabbitMQ berbasis push/routing (AMQP) — ideal untuk task distribution. Kafka berbasis log — ideal untuk event streaming dan replay. Kafka memiliki throughput lebih tinggi.
</details>

### Soal 2
Mengapa message queue penting dalam arsitektur microservices?

<details>
<summary>Jawaban</summary>
Karena membuat service decoupled — producer dan consumer tidak perlu online bersamaan. Jika consumer crash, pesan tetap aman di queue sampai diproses.
</details>

### Soal 3
Apa yang terjadi jika consumer lupa mengirim acknowledgment?

<details>
<summary>Jawaban</summary>
Broker akan mengirim ulang pesan yang sama ke consumer (atau consumer lain). Jika terjadi terus, pesan akan diproses berulang (duplicate processing).
</details>
