# 99 - Load Testing - k6 untuk Identifikasi Bottleneck

## Penjelasan
Database sudah dioptimasi (materi 98), adapter sudah Fastify (materi 97). Tapi *seberapa kuat* aplikasi kita? Load testing memberikan jawaban kuantitatif. k6 adalah tool load testing open-source dari Grafana — ringan, bisa scripting (JavaScript), dan terintegrasi dengan Grafana/Prometheus.

## Fungsi
- **k6**: CLI load testing — simulasikan ribuan virtual user (VU) secara bersamaan.
- **Skenario**: Ramp up (naik bertahap), soak test (beban konstan lama), stress test (beban ekstrem).
- **Metrics**: Response time, throughput (req/s), error rate.
- **Identifikasi bottleneck**: Dari CPU, memory, database, atau network.

## Cara Pengimplementasian

### Install k6
```bash
# Windows - download dari https://k6.io/docs/getting-started/installation/
# Atau via winget
winget install k6

# Verifikasi
k6 version
```

### Test Script: Ramp Up 100 VU dalam 5 Menit
```javascript
// load-test.js
import http from 'k6/http';
import { check, sleep } from 'k6';
import { Rate, Trend } from 'k6/metrics';

// Custom metrics
const errorRate = new Rate('errors');
const responseTime = new Trend('response_time');

export const options = {
  stages: [
    { duration: '1m', target: 20 },   // Ramp up ke 20 VU
    { duration: '2m', target: 50 },   // Ramp up ke 50 VU
    { duration: '1m', target: 100 },  // Ramp up ke 100 VU
    { duration: '2m', target: 100 },  // Stay di 100 VU
    { duration: '1m', target: 0 },    // Ramp down
  ],
  thresholds: {
    http_req_duration: ['p(95)<500'],   // 95% request <500ms
    errors: ['rate<0.05'],              // Error rate <5%
    http_req_failed: ['rate<0.01'],     // Failed request <1%
  },
};

const BASE_URL = __ENV.BASE_URL || 'http://localhost:3000';

export default function () {
  // GET /users
  const getUsers = http.get(`${BASE_URL}/api/users`);
  check(getUsers, {
    'getUsers status 200': (r) => r.status === 200,
  });
  errorRate.add(getUsers.status !== 200);
  responseTime.add(getUsers.timings.duration);

  // GET /posts
  const getPosts = http.get(`${BASE_URL}/api/posts`);
  check(getPosts, {
    'getPosts status 200': (r) => r.status === 200,
  });
  errorRate.add(getPosts.status !== 200);

  // POST /orders
  const payload = JSON.stringify({
    userId: 1,
    items: [{ productId: 1, quantity: 2 }],
  });
  const createOrder = http.post(`${BASE_URL}/api/orders`, payload, {
    headers: { 'Content-Type': 'application/json' },
  });
  check(createOrder, {
    'createOrder status 201': (r) => r.status === 201,
  });

  sleep(1); // Tunggu 1 detik antara siklus
}
```

### Stress Test
```javascript
export const options = {
  stages: [
    { duration: '2m', target: 200 },    // Naik ke 200 VU
    { duration: '5m', target: 500 },    // Naik ke 500 VU
    { duration: '2m', target: 1000 },   // Peak 1000 VU
    { duration: '2m', target: 0 },      // Turun
  ],
};
```

### Soak Test (Beban Stabil untuk Waktu Lama)
```javascript
export const options = {
  stages: [
    { duration: '5m', target: 100 },   // Ramp up
    { duration: '60m', target: 100 },  // Soak — 1 jam beban konstan
    { duration: '5m', target: 0 },     // Ramp down
  ],
};
```

### Jalankan Load Test
```bash
# Test lokal
k6 run load-test.js

# Dengan environment variable
k6 run -e BASE_URL=http://staging.example.com load-test.js

# Output hasil
k6 run --summary-export=results.json load-test.js
```

### Analisis Hasil
```
✓ status 200: 100% of requests
✗ p(95) < 500ms: 78% of requests → THRESHOLD FAILED

http_req_duration........: avg=245ms  min=12ms  med=180ms  max=3200ms
  { expected_response:true }........: avg=245ms  min=12ms  med=180ms  max=3200ms

http_reqs...............: 4521  15.07/s
errors..................: 0     0%
vus.....................: 100   min=0  max=100
vus_max.................: 100

response_time...........: avg=245  min=12  med=180  max=3200
```

### Identifikasi Bottleneck dari Hasil
| Skenario | Indikasi Bottleneck | Tindakan |
|----------|--------------------|----------|
| Response time naik linear dengan VU | Resource tersedia, tapi lambat | Optimasi kode, cache |
| Response time spike di VU tertentu | Connection pool habis | Tambah pool size / PgBouncer |
| Error 5xx mulai muncul | Server overload | Scale horizontally |
| CPU 100% di VU rendah | Infinite loop / heavy computation | Profiling kode |
| Error rate tinggi di endpoint tertentu | Bug/query N+1 | Logging + fix query |

### Dashboard Grafana untuk k6
```bash
# k6 bisa output ke Grafana via Prometheus remote write
k6 run --out output-prometheus-remote load-test.js
```

```yaml
# docker-compose.yml tambahan
services:
  prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml

  grafana:
    image: grafana/grafana
    ports: ["3000:3000"]
```

## Analogi
Gedung ingin diuji kapasitas **lift**. Load test seperti mengirim 100, 500, 1000 orang secara bertahap. Kita ukur: berapa lama mereka sampai ke lantai tujuan (response time), berapa orang per menit (throughput), berapa yang jatuh dari lift (error rate). 

- **Ramp up**: 1 orang → 10 → 50 → 100 — seperti buka pintu bertahap.
- **Soak test**: 100 orang naik-turun lift selama 1 jam — cek apakah lift kepanasan.
- **Stress test**: 1000 orang sekaligus — lift mana yang pertama rusak?
- **Threshold**: Aturan "lift tidak boleh lebih dari 500ms" — jika 95% penumpang >500ms, lift perlu diperbaiki.

## Dipakai untuk apa
- Sebelum rilis ke production — pastikan aplikasi tahan traffic.
- Setelah perubahan besar — migrasi database, ganti adapter, deploy baru.
- Capacity planning — berapa VPS/server yang dibutuhkan untuk traffic target.
- SLO verification — bukti aplikasi memenuhi target latency.

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Test dari local ke production | Network latency bikin hasil bias | Test dari environment yang sama atau cloud |
| Threshold terlalu ketat/ longgar | False positive / false negative | Awali dengan exploratory test, baru set threshold |
| Tidak test endpoint yang berbeda | Bottleneck di endpoint tertentu terlewat | Test endpoint kritis: login, checkout, search |
| Hanya test dengan VU rendah | Tidak terlihat bottleneck di scale | Target VU = peak traffic x 2 |
| Tidak monitoring resource saat test | Tidak tahu bottleneck di CPU/DB/Disk | Pantau Prometheus + Grafana saat test |

## Soal Latihan

**Soal 1:** Buat k6 script untuk stress test endpoint `/api/login` dengan 200 VU selama 5 menit, dengan threshold p(95) < 2 detik.

**Jawaban 1:**
```javascript
import http from 'k6/http';
import { check } from 'k6';

export const options = {
  stages: [
    { duration: '2m', target: 200 },
    { duration: '3m', target: 200 },
    { duration: '1m', target: 0 },
  ],
  thresholds: {
    http_req_duration: ['p(95)<2000'],
  },
};

export default function () {
  const payload = JSON.stringify({ email: 'test@test.com', password: 'password123' });
  const res = http.post('http://localhost:3000/api/login', payload, {
    headers: { 'Content-Type': 'application/json' },
  });
  check(res, { 'login status 200': (r) => r.status === 200 });
}
```

**Soal 2:** Setelah load test, response time rata-rata 800ms. Bagaimana cara menentukan apakah bottleneck di database atau di aplikasi?

**Jawaban 2:**
1. Buka **Grafana** → lihat CPU aplikasi vs CPU database:
   - Jika CPU database 100% → bottleneck DB. Tambah index, optimasi query, atau scale DB.
   - Jika CPU aplikasi 100% → bottleneck aplikasi. Profiling kode (event loop lag, heavy computation).
2. Dari **Prisma query logging**: jika ada query >500ms, itu bottleneck DB.
3. Matikan fitur yang dicurigai (cache, auth) — jika response time turun drastis, itu penyebabnya.
4. Tambah instance aplikasi (horizontal scaling): jika throughput naik linear → bottleneck aplikasi. Jika tidak naik → bottleneck DB.
