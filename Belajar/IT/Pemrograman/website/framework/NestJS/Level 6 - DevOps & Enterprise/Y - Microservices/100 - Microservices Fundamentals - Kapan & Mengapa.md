# 100 - Microservices Fundamentals - Kapan & Mengapa

## Penjelasan
Selama Level 1-5, kita membangun satu aplikasi monolitik NestJS — satu kode, satu deployment. Ini pendekatan paling sederhana dan paling tepat untuk sebagian besar proyek. Tapi ketika aplikasi dan tim berkembang, monolith mulai terasa berat. Microservices adalah arsitektur alternatif: **memecah satu aplikasi besar menjadi beberapa service kecil yang berjalan independen**.

## Fungsi
- **Microservices vs Monolith**: Memahami trade-off — kapan cocok, kapan tidak.
- **Kapan microservices**: Tim besar (>10 orang), skala berbeda per fitur, kebutuhan teknologi berbeda per service.
- **NestJS transport layer**: TCP (langsung), Redis (pub/sub), RabbitMQ/Kafka (message broker), gRPC (high-performance RPC).
- **Analisis pemisahan service**: Menentukan service mana yang cocok dipisah berdasarkan bounded context.

## Cara Pengimplementasian

### Kapan Pakai Microservices — Checklist
```
✅ TIM BESAR — Setiap service dikelola tim berbeda
✅ SKALA BERBEDA — Auth: 100 req/s, Report: 1 req/s → bisa di scale berbeda
✅ TEKNOLOGI BERBEDA — Service A pakai Postgres, Service B pakai MongoDB
✅ RELEASE INDEPENDEN — Deploy service Auth tanpa deploy service Order
❌ TIM KECIL (<10 orang) → Monolith dulu
❌ MVP / EARLY STAGE → Monolith dulu
❌ DOMAIN SEDERHANA → CRUD sederhana tanpa kompleksitas bisnis
```

### NestJS Transport Layer
```typescript
// 1. TCP — Langsung, sederhana, tanpa broker
const app = await NestFactory.createMicroservice(AppModule, {
  transport: Transport.TCP,
  options: { port: 3001 },
});

// 2. Redis — Pub/sub, ringan, built-in NestJS
const app = await NestFactory.createMicroservice(AppModule, {
  transport: Transport.REDIS,
  options: { host: 'localhost', port: 6379 },
});

// 3. RabbitMQ — Message broker, queue, reliable
const app = await NestFactory.createMicroservice(AppModule, {
  transport: Transport.RMQ,
  options: {
    urls: ['amqp://localhost:5672'],
    queue: 'orders_queue',
    queueOptions: { durable: true },
  },
});

// 4. Kafka — Event streaming, high throughput
const app = await NestFactory.createMicroservice(AppModule, {
  transport: Transport.KAFKA,
  options: {
    client: { brokers: ['localhost:9092'] },
    consumer: { groupId: 'order-consumer' },
  },
});

// 5. gRPC — High performance, typed contract via .proto
const app = await NestFactory.createMicroservice(AppModule, {
  transport: Transport.GRPC,
  options: {
    package: 'hero',
    protoPath: join(__dirname, 'hero/hero.proto'),
  },
});
```

### Analisis Pemisahan — Bounded Context
```
┌─────────────────────────────────────────────────┐
│                 MONOLITH                         │
│  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐           │
│  │ Auth │ │Order │ │Product│ │Email │           │
│  └──────┘ └──────┘ └──────┘ └──────┘           │
└─────────────────────────────────────────────────┘

                        ↓

┌──────┐   ┌──────┐   ┌──────┐   ┌──────┐
│ Auth │   │Order │   │Product│   │Email │
│Service│   │Service│   │Service│   │Service│
└──┬───┘   └──┬───┘   └──┬───┘   └──┬───┘
   │          │          │          │
   └──────────┴──────────┴──────────┘
                        │
                 Message Broker
                 (RabbitMQ/Kafka)
```

### Contoh Analisis — Service Mana yang Cocok Dipisah
```typescript
// 1. Auth Service — Cocok dipisah
// Alasan: Security boundary, reusable di banyak aplikasi
// Database sendiri: users table, tokens
// Teknologi: JWT + Redis

// 2. Email Service — Perfect candidate
// Alasan: I/O heavy (SMTP), skalabilitas berbeda, bisa pakai queue
// Tidak perlu response real-time → Event-driven
// Database: tidak perlu (cukup queue)

// 3. Report Service — Cocok dipisah
// Alasan: Heavy computation, resource intensif, bisa di-trigger schedule
// Database: read replica (tidak perlu write)
// Bisa scale terpisah dari API

// 4. Product Service — Mungkin masih di monolith
// Alasan: Tight coupling dengan Order (stok, price)
// Jika sering berubah barengan → lebih baik satu service
```

## Analogi
Gedung kecil (monolith): satu pintu, satu resepsionis, satu tim kebersihan. Semua fungsi di lantai yang sama. Ketika gedung membesar, kita bangun **gedung-gedung terpisah**:
- **Gedung A (Auth Service)** — resepsionis, cek KTP
- **Gedung B (Order Service)** — ruang transaksi
- **Gedung C (Email Service)** — ruang pos (bisa di pinggir, tidak perlu di pusat)
- **Gedung D (Report Service)** — ruang arsip (bisa di basement)

Setiap gedung punya **satpam sendiri** (database sendiri), **manajer sendiri** (tim sendiri), **jadwal operasi berbeda** (scale berbeda). Antar gedung terhubung dengan **terowongan** (message broker). Tapi untuk gedung kecil dengan 5 karyawan — satu gedung cukup, jangan bikin 5 gedung terpisah.

## Dipakai untuk apa
- Aplikasi enterprise dengan domain kompleks (e-commerce, banking, SaaS).
- Organisasi dengan banyak tim (setiap tim pegang service sendiri).
- Aplikasi dengan kebutuhan skalabilitas berbeda per fitur.
- Sistem yang perlu teknologi berbeda (misal: real-time pakai WebSocket, report pakai Python).

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Microservices dari awal | Overhead besar, development lambat | Mulai monolith, refactor setelah stabil |
| Terlalu kecil (nanoservices) | Banyak service, banyak overhead komunikasi | Gabung service yang erat hubungannya (cohesion) |
| Database masih shared | Tight coupling, perubahan DB satu service pengaruhi yang lain | Setiap service punya database sendiri |
| Testing jadi sangat sulit | Butuh orchestrated testing (contract test) | Gunakan Pact untuk contract testing |
| Debugging jadi nightmare | Request melewati banyak service | Distributed tracing + correlation ID |

## Soal Latihan

**Soal 1:** Sebuah startup e-commerce dengan 5 developer. Apakah perlu microservices? Jelaskan.

**Jawaban 1:** Tidak. Startup dengan 5 developer sebaiknya monolith dulu. Microservices menambah kompleksitas: network latency, distributed tracing, message broker, orchestration. Fokus pada product-market fit. Jika nanti tim >10-15 orang dan traffic >10k req/s, baru refactor ke microservices.

**Soal 2:** Dari modul-modul berikut, mana yang paling cocok dipisah menjadi microservice? Beri alasan.
- Modul Auth
- Modul Product
- Modul Order
- Modul Email
- Modul Review

**Jawaban 2:**
1. **Email Service** — Paling cocok. I/O heavy, bisa async via queue, scale sendiri, tidak butuh response real-time.
2. **Auth Service** — Cocok. Security boundary penting, reusable untuk aplikasi lain (mobile, web, partner API).
3. **Review Service** — Cocok. Traffic bisa tiba-tiba tinggi (viral product), tidak kritis untuk transaksi.
4. **Product + Order** — Sebaiknya tetap bersama atau dipisah hati-hati karena tight coupling (stok, pricing).
