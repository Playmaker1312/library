# 115-A. System Design — Prinsip Merancang Sistem Berskala Besar

**Benang Merah**: Level 1-5 kita membangun **aplikasi tunggal**. System Design adalah **arsitektur keseluruhan** — bagaimana banyak komponen (service, database, cache, queue) bekerja bersama untuk melayani jutaan user.

---

## Penjelasan

System Design adalah seni merancang **arsitektur sistem** yang memenuhi kebutuhan fungsional (fitur) dan non-fungsional (skalabilitas, ketersediaan, performa). Ibarat **master plan kota** — bukan hanya satu gedung, tapi bagaimana jalan, listrik, air, transportasi, dan gedung-gedung terintegrasi.

```text
User → Load Balancer → Web Server → Cache → Database
                         ↓
                   Microservice A → Queue → Worker
                         ↓
                   Microservice B → External API
```

### Konsep Kunci:

1. **Scalability** — vertikal (perkuat server) vs horizontal (tambah server)
2. **Availability** — uptime sistem (99.9% = 8 jam downtime/tahun)
3. **Latency** — waktu respons (target <200ms)
4. **Throughput** — request per detik yang bisa ditangani
5. **Consistency** — semua node lihat data yang sama
6. **CAP Theorem** — Consistency, Availability, Partition Tolerance — pilih 2 dari 3

---

## Fungsi

Memastikan sistem bisa **tumbuh** (dari 100 ke 10 juta user) tanpa perlu di-rewrite dari awal. System Design mencegah **rewrite mahal** karena arsitektur salah di awal.

---

## Cara Pengimplementasian (Pendekatan)

### 1. Load Balancer
```text
User → [Load Balancer] → Server 1
                      → Server 2
                      → Server 3
```
Mendistribusikan request ke banyak server. Jika satu server mati, traffic dialihkan.

### 2. Database Scaling
```javascript
// Read Replicas — tulis di master, baca di replica
const master = new PrismaClient({ url: process.env.DB_MASTER });
const replica = new PrismaClient({ url: process.env.DB_REPLICA });

// Sharding — pisah data berdasarkan key
// UserID 1-1000 → DB 1, UserID 1001-2000 → DB 2
function getShard(userId) {
  const shardId = userId % 4; // 4 database shard
  return dbConnections[shardId];
}
```

### 3. Caching Strategy
```javascript
// Cache-aside pattern
async function getUser(id) {
  // 1. Cek cache dulu
  let user = await redis.get(`user:${id}`);
  if (user) return JSON.parse(user);

  // 2. Cache miss → ambil dari DB
  user = await db.user.findUnique({ where: { id } });

  // 3. Simpan ke cache untuk next request
  await redis.set(`user:${id}`, JSON.stringify(user), 'EX', 3600);
  return user;
}
```

### 4. Message Queue (Opsional)
```javascript
// Async processing — kirim email tidak perlu blocking
// Producer
app.post('/register', async (req, res) => {
  const user = await createUser(req.body);
  await queue.send('email_welcome', { userId: user.id });
  res.json(user); // response cepat, email diproses background
});

// Consumer (worker terpisah)
queue.consume('email_welcome', async (msg) => {
  await sendEmail(msg.userId, 'Selamat Datang!');
});
```

---

## Analogi: Membangun Rumah (Perencanaan Kota)

| System Design | Perencanaan Kota |
|---|---|
| Load Balancer | Bundaran lalu lintas — distribusi kendaraan |
| Database | Pusat penyimpanan air kota |
| Cache | Tangki air di setiap perumahan |
| CDN | Gudang cadangan di berbagai daerah |
| Microservices | Bangunan khusus: RS, sekolah, pasar |
| Message Queue | Pos — surat diantar tanpa perlu nunggu |
| Monitoring | CCTV kota — lihat kondisi real-time |

System Design seperti **master plan kota** untuk 10 tahun ke depan. Anda tidak bisa bangun sembarangan — harus pikirkan: berapa jumlah penduduk 5 tahun lagi? di mana kawasan industri? bagaimana jalur evakuasi bencana? Keputusan awal menentukan apakah kota akan macet total atau tetap lancar saat populasi membengkak.

---

## Dipakai Untuk Apa

- **Technical interview** — posisi senior/staff engineer
- **Arsitektur baru** — merancang sistem dari nol
- **Migrasi** — monolith → microservices
- **Scalability audit** — mengapa sistem lambat saat traffic naik
- **Capacity planning** — berapa server yang dibutuhkan tahun depan

---

## Kesalahan Umum

| Kesalahan | Dampak |
|---|---|
| Over-engineering | Microservices dari awal untuk 100 user |
| Ignoring CAP theorem | Ingin consistency + availability + partition tolerance |
| Tidak ada monitoring | Tidak tahu mana yang lemah sampai crash |
| Single point of failure | Satu server mati, semua mati |

---

## Hubungan dengan Materi Sebelumnya

System Design adalah **puncak arsitektur**:
- Materi 57 (Express) → Web server yang akan diskalakan
- Materi 63 (PostgreSQL) → Database yang di-sharding
- Materi 108 (Caching → Redis) → Cache di depan database
- Materi 110 (Docker) → Container untuk setiap service
- Materi 111 (Docker Compose) → Orkestrasi multi-service
- Materi 114 (Monitoring) → Observability sistem

---

## Soal Latihan

### Soal 1 (Mudah)
Jelaskan perbedaan **vertical scaling** dan **horizontal scaling** dalam 2 kalimat.

**Jawaban**:
Vertical scaling: upgrade RAM/CPU server yang sama (terbatas, mahal). Horizontal scaling: tambah lebih banyak server (lebih murah, scalable, butuh load balancer).

### Soal 2 (Sedang)
Sebutkan 3 komponen yang diperlukan untuk membuat sistem yang **highly available** (99.99% uptime) dan jelaskan fungsinya.

**Jawaban**:
1. **Load Balancer** — distribusi traffic, jika satu server mati, arahkan ke yang lain
2. **Database Replication** — master-slave, jika master mati, promote slave ke master
3. **Health Check + Auto-healing** — monitor server, restart otomatis jika crash

### Soal 3 (Tantangan)
Desain sistem untuk URL Shortener (seperti bit.ly). Fitur:
- User input URL panjang → dapat URL pendek
- Redirect ke URL asli saat dikunjungi
- 100 juta URL baru per bulan
- Baca 100x lebih banyak dari tulis

Apa strategi database, caching, dan scaling Anda?

**Jawaban**:
**Database**: Gunakan key-value store (Redis) atau NoSQL (DynamoDB/Cassandra) karena pola read-heavy. Key: short code (7 karakter base62 → 3.5 triliun kombinasi). Tulis langsung ke DB.

**Caching**: Cache 80% URL terpopuler di Redis. Cache-aside pattern. TTL 24 jam.

**Scaling**:
- Web server: horizontal scaling di belakang load balancer
- Database: read replicas (karena read 100x > write)
- Generate short code: counter terpusat (Redis INCR) + base62 encode, atau distributed ID (Snowflake)

**Estimasi**: 100M URL/bulan ≈ 37 URL/detik write, 3700 URL/detik read. Satu instance Redis cukup untuk cache, DB perlu 2-3 read replicas.
