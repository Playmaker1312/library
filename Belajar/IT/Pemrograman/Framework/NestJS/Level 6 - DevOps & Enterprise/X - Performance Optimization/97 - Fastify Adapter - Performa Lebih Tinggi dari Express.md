# 97 - Fastify Adapter - Performa Lebih Tinggi dari Express

## Penjelasan
Sejauh ini kita menggunakan Express (default NestJS). Express stabil, mature, dengan ekosistem middleware terbesar. Tapi dalam benchmark, Fastify ~2-3x lebih cepat dalam request per detik. NestJS mendukung *swappable adapter* — kita bisa ganti Express ke Fastify tanpa mengubah kode aplikasi.

## Fungsi
- **@nestjs/platform-fastify**: Adapter Fastify untuk NestJS.
- **Trade-off**: Fastify lebih cepat (~2x throughput) tapi tidak semua middleware Express kompatibel.
- **Benchmark**: Perbandingan request/detik antara Express dan Fastify dalam berbagai skenario.

## Cara Pengimplementasian

### Install
```bash
npm install @nestjs/platform-fastify fastify @fastify/static @fastify/cors
```

### Ganti Adapter di `main.ts`
```typescript
import { NestFactory } from '@nestjs/core';
import { FastifyAdapter, NestFastifyApplication } from '@nestjs/platform-fastify';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create<NestFastifyApplication>(
    AppModule,
    new FastifyAdapter({
      logger: false,
      bodyLimit: 10 * 1024 * 1024, // 10MB
    }),
  );

  // CORS — Fastify plugin, bukan middleware Express
  await app.register(require('@fastify/cors'), {
    origin: ['http://localhost:3000'],
    credentials: true,
  });

  // Serve static files
  await app.register(require('@fastify/static'), {
    root: join(__dirname, '..', 'public'),
    prefix: '/public/',
  });

  await app.listen(3000, '0.0.0.0');
  console.log(`Server running on http://localhost:3000`);
}
bootstrap();
```

### Pipe & Validation — Gunakan Zod atau Class Validator
Fastify kompatibel dengan class-validator. Tidak ada perubahan untuk DTO validation — tetap pakai `ValidationPipe` seperti biasa.

```typescript
// main.ts — tetap sama
app.useGlobalPipes(new ValidationPipe({ whitelist: true }));
```

### Swagger Setup dengan Fastify
```typescript
import { DocumentBuilder, SwaggerModule } from '@nestjs/swagger';

const config = new DocumentBuilder()
  .setTitle('NestJS API')
  .setVersion('1.0')
  .build();

const document = SwaggerModule.createDocument(app, config);
SwaggerModule.setup('api', app, document);
```

### Middleware Express yang Tidak Kompatibel
Beberapa middleware Express menggunakan API spesifik Express (`req.*`, `res.*`). Contoh yang mungkin bermasalah:
- `cookie-parser` → ganti `@fastify/cookie`
- `helmet` → ganti `@fastify/helmet`
- `morgan` → ganti Fastify logger built-in
- `passport` → berfungsi dengan @nestjs/passport (kompatibel)

### Benchmark Script
```typescript
// benchmark.ts — jalankan dengan ts-node
import autocannon from 'autocannon';

async function benchmark(url: string, title: string) {
  console.log(`\n=== ${title} ===`);
  const result = await autocannon({
    url,
    connections: 100,
    duration: 30,
    pipelining: 10,
  });

  console.log(`Req/sec: ${result.requests.average}`);
  console.log(`Latency avg: ${result.latency.average} ms`);
  console.log(`Throughput: ${result.throughput.average} bytes/sec`);
  return result;
}

// Jalankan di dua port berbeda
async function main() {
  await benchmark('http://localhost:3001/api/users', 'Express');
  await benchmark('http://localhost:3002/api/users', 'Fastify');
}

main();
```

### Config untuk Two Instance
```json
// package.json
{
  "scripts": {
    "start:express": "cross-env PORT=3001 node dist/main-express",
    "start:fastify": "cross-env PORT=3002 node dist/main-fastify",
    "benchmark": "ts-node benchmark.ts"
  }
}
```

### Hasil Benchmark (Estimasi)
| Metric | Express | Fastify | Improvement |
|--------|---------|---------|-------------|
| Request/detik | ~15,000 | ~35,000 | ~2.3x |
| Latency avg | ~6.5ms | ~2.8ms | ~57% lebih cepat |
| Throughput | ~5 MB/s | ~12 MB/s | ~2.4x |

## Analogi
Express adalah **eskalator** — sudah ada di mana-mana, semua orang tahu cara pakai, mudah diperbaiki. Fastify adalah **lift ekspres** — lebih cepat sampai tujuan, irit energi (CPU/memory), tapi butuh teknisi khusus jika rusak (beberapa middleware Express tidak kompatibel). Untuk gedung sibuk dengan 1000 pengunjung/jam, lift ekspres jelas lebih unggul.

## Dipakai untuk apa
- API dengan traffic tinggi (>10k request/detik).
- Aplikasi yang butuh latency rendah (<10ms).
- Microservices — setiap ms latency berarti di chain service.
- Resource terbatas — Fastify lebih hemat CPU/memory per request.

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Tidak ganti middleware Express yang incompatible | Error runtime "req.pipe is not a function" | Ganti dengan plugin Fastify yang setara |
| Lupa register plugin sebelum digunakan | CORS/static file tidak berfungsi | Register plugin di `main.ts` dengan `app.register()` |
| Asumsi semua library Express bekerja | Bug misterius di production | Baca dokumentasi library untuk kompatibilitas Fastify |
| Benchmark tidak fair | Express kalah karena tidak dioptimasi | Pastikan kedua instance menggunakan kode yang identik |
| Tidak test dengan middleware custom | Middleware custom pakai Express API | Ubah ke Fastify hook API (`app.addHook`) |

## Soal Latihan

**Soal 1:** Ganti adapter Express ke Fastify di aplikasi NestJS yang sudah ada. Apa saja perubahan yang diperlukan?

**Jawaban 1:**
1. Install `@nestjs/platform-fastify`, `@fastify/cors`, `@fastify/static`
2. Ubah `main.ts`:
   - `NestFactory.create<NestFastifyApplication>(AppModule, new FastifyAdapter())`
   - `app.register(require('@fastify/cors'))` untuk CORS
3. Jika ada middleware Express, ganti ke Fastify hook atau plugin yang setara
4. Test semua endpoint

**Soal 2:** Jalankan benchmark antara Express dan Fastify untuk endpoint yang sama. Berapa perbedaan throughput-nya?

**Jawaban 2:**
```bash
# Setup dua instance
npm run start:express   # port 3001
npm run start:fastify   # port 3002

# Jalankan benchmark dengan autocannon
npx autocannon -c 100 -d 30 http://localhost:3001/api/users
npx autocannon -c 100 -d 30 http://localhost:3002/api/users
```
Bandingkan output `Req/sec` dan `Latency`. Fastify biasanya 2-3x lebih cepat dalam throughput.
