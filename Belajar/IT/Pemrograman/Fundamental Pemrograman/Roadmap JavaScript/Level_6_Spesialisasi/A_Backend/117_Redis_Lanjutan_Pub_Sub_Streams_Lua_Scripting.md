# 117 — Redis Lanjutan: Pub/Sub, Streams, Lua Scripting

## 1. Penjelasan

Redis bukan sekadar cache. Redis menyediakan:
- **Pub/Sub:** Komunikasi real-time satu-to-banyak.
- **Streams:** Log event berurutan — mirip Kafka, tapi ringan.
- **Lua Scripting:** Eksekusi multi-perintah atomik di sisi Redis.

## 2. Fungsi

| Fitur | Fungsi |
|-------|--------|
| Pub/Sub | Notifikasi real-time (chat, alert) |
| Streams | Event sourcing, job queue, logging kronologis |
| Lua Script | Transaksi kompleks tanpa race condition |

## 3. Code

```javascript
const Redis = require('ioredis')
const pub = new Redis()
const sub = new Redis()

// PUBLISHER — notifikasi real-time
pub.publish('notifikasi', JSON.stringify({
  user: 'andi',
  pesan: 'Buku favorit Anda tersedia!'
}))

// SUBSCRIBER
sub.subscribe('notifikasi')
sub.on('message', (channel, message) => {
  console.log(`[${channel}]`, JSON.parse(message))
})

// REDIS STREAMS — event log
const stream = new Redis()
async function logEvent() {
  await stream.xadd('events:pinjam', '*', 'userId', '1', 'bukuId', '42')
  const logs = await stream.xrange('events:pinjam', '-', '+')
  console.log(logs)
}
```

```lua
-- Lua scripting — transaksi atomik
local saldo = redis.call('GET', KEYS[1])
if tonumber(saldo) >= tonumber(ARGV[1]) then
  redis.call('DECRBY', KEYS[1], ARGV[1])
  return 1
end
return 0
```

```javascript
// Panggil Lua dari Node.js
const result = await redis.eval(
  script, 1, 'saldo:user:1', '50000'
)
```

## 4. Analogi Rumah

| Redis Feature | Analogi Rumah |
|---------------|---------------|
| Pub/Sub | Pengeras suara di kompleks — satu orang ngomong, semua dengar |
| Streams | Buku tamu — ada catatan kronologis siapa datang kapan |
| Lua Scripting | Satu juru kunci yang melakukan beberapa pekerjaan sekali masuk |

## 5. Use Case

- **Pub/Sub:** Sistem notifikasi — user mendapat alert saat buku favorit tersedia.
- **Streams:** Audit log peminjaman — catat setiap transaksi.
- **Lua:** Top-up saldo dompet digital — baca saldo, kurangi, simpan — semua atomik.

## 6. Kesalahan Umum

- **Pub/Sub tidak persist:** Jika subscriber offline, pesan hilang. Gunakan Streams jika perlu persist.
- **Blocking di Lua:** Jangan panggil `redis.call()` lambat di dalam script — blokir semua klien.
- **XADD tanpa trim:** Streams tumbuh tak terbatas — selalu set `MAXLEN`.

## 7. Benang Merah

Materi **116 (Database Lanjutan)** mengelola data di disk → **117 (Redis)** mengelola data di memory dengan pola messaging. Ini menjadi fondasi untuk **118 (Message Queue)**.

## 8. Soal & Jawaban

### Soal 1
Apa perbedaan Redis Pub/Sub dan Redis Streams dalam hal persistensi?

<details>
<summary>Jawaban</summary>
Pub/Sub tidak menyimpan pesan — jika subscriber offline, pesan hilang. Streams menyimpan pesan di memory/disk sehingga bisa dibaca kapan saja.
</details>

### Soal 2
Kapan sebaiknya menggunakan Lua scripting di Redis?

<details>
<summary>Jawaban</summary>
Saat perlu menjalankan beberapa perintah Redis secara atomik tanpa interupsi dari klien lain, misalnya debit saldo + cek stok dalam satu transaksi.
</details>

### Soal 3
Mengapa Pub/Sub tidak cocok untuk job queue?

<details>
<summary>Jawaban</summary>
Karena Pub/Sub tidak menjamin delivery — jika consumer sibuk atau crash saat pesan dikirim, pesan akan hilang tanpa diproses ulang.
</details>
