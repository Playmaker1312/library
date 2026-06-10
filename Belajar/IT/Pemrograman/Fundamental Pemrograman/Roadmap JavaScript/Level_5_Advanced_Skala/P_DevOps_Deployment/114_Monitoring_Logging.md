# 114. Monitoring & Logging

**Benang Merah**: Aplikasi sudah di-deploy ke cloud (Materi 113). Sekarang kita perlu **tahu apa yang terjadi** — apakah aplikasi sehat? Ada error? Lambat? Monitoring & logging menjawab itu semua.

---

## Penjelasan

Monitoring & logging adalah **mata dan telinga** aplikasi di production. 
- **Logging**: Mencatat semua kejadian (request, error, warning) secara terstruktur.
- **Monitoring**: Memantau health, performa, dan availability secara real-time.
- **Error Tracking**: Menangkap exception dengan stack trace lengkap.

```javascript
// Winston — structured logging
const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.Console({ format: winston.format.simple() })
  ]
});

logger.info('Server started', { port: 3000, env: 'production' });

// Sentry — error tracking
Sentry.init({ dsn: process.env.SENTRY_DSN });
Sentry.captureException(error);
```

---

## Fungsi

- **Deteksi dini** — tahu ada error sebelum user complain
- **Debugging** — lihat log kronologis untuk trace penyebab masalah
- **Analisa performa** — response time, error rate, throughput
- **Alert** — notifikasi otomatis jika ada anomali

---

## Cara Pengimplementasian

### 1. Setup Winston (Structured Logging)

```bash
npm install winston
```

```javascript
// src/lib/logger.js
const winston = require('winston');

const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.errors({ stack: true }),
    winston.format.json()
  ),
  defaultMeta: { service: 'my-app' },
  transports: [
    // Error log ke file
    new winston.transports.File({
      filename: 'logs/error.log',
      level: 'error',
      maxsize: 5242880, // 5MB
      maxFiles: 5,
    }),
    // Semua log ke file
    new winston.transports.File({
      filename: 'logs/combined.log',
      maxsize: 5242880,
      maxFiles: 10,
    }),
  ],
});

// Di development, log ke console dengan format rapi
if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.combine(
      winston.format.colorize(),
      winston.format.simple()
    ),
  }));
}

module.exports = logger;
```

Middleware Express:
```javascript
// src/middleware/logger.js
const logger = require('../lib/logger');

function requestLogger(req, res, next) {
  const start = Date.now();
  res.on('finish', () => {
    const duration = Date.now() - start;
    logger.info('HTTP Request', {
      method: req.method,
      url: req.originalUrl,
      status: res.statusCode,
      duration: `${duration}ms`,
      ip: req.ip,
      userAgent: req.get('user-agent'),
    });
  });
  next();
}
```

### 2. Setup Sentry (Error Tracking)

```bash
npm install @sentry/node
```

```javascript
// src/app.js — inisialisasi di awal
const Sentry = require('@sentry/node');

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV || 'development',
  tracesSampleRate: 1.0, // sample rate untuk performance tracing
});

// Middleware Sentry error handler (harus PALING AKHIR)
app.use(Sentry.Handlers.errorHandler());
```

Sentry akan:
- Capture unhandled exception otomatis
- Capture promise rejection
- Kirim stack trace lengkap ke dashboard Sentry
- Group error yang sama (supaya tidak spam)

### 3. Uptime Monitor Sederhana

```javascript
// scripts/uptime-monitor.js
// Jalankan dengan cron job (setiap 5 menit)
const https = require('https');
const { Webhook } = require('discord-webhook-node');

const WEBHOOK_URL = process.env.ALERT_WEBHOOK_URL;
const TARGET_URL = process.env.TARGET_URL || 'https://myapp.com/health';

function checkHealth() {
  return new Promise((resolve, reject) => {
    const start = Date.now();
    https.get(TARGET_URL, (res) => {
      const duration = Date.now() - start;
      let data = '';
      res.on('data', chunk => data += chunk);
      res.on('end', () => resolve({ status: res.statusCode, duration, data }));
    }).on('error', reject);
  });
}

async function main() {
  try {
    const result = await checkHealth();
    if (result.status !== 200) {
      const hook = new Webhook(WEBHOOK_URL);
      hook.send(`🔴 **DOWN** — ${TARGET_URL} responded ${result.status} in ${result.duration}ms`);
    } else if (result.duration > 2000) {
      const hook = new Webhook(WEBHOOK_URL);
      hook.send(`⚠️ **SLOW** — ${TARGET_URL} took ${result.duration}ms (threshold: 2000ms)`);
    }
  } catch (err) {
    const hook = new Webhook(WEBHOOK_URL);
    hook.send(`🚨 **CRASH** — ${TARGET_URL} unreachable: ${err.message}`);
  }
}

main();
```

Cron job (crontab):
```bash
# Cek setiap 5 menit
*/5 * * * * node /var/www/my-app/scripts/uptime-monitor.js
```

### 4. Metrics Penting

| Metric | Arti | Threshold |
|---|---|---|
| Response Time | Waktu respon rata-rata | < 200ms (API), < 2s (page) |
| Error Rate | % request yang error | < 1% |
| Throughput | Request per detik | Tergantung kapasitas |
| CPU Usage | Pemakaian CPU | < 80% |
| Memory Usage | Pemakaian RAM | < 80% |
| Disk Usage | Pemakaian disk | < 80% |
| Uptime | Persentase waktu hidup | 99.9%+ |

---

## Analogi: Membangun Rumah (Dashboard Rumah)

| Monitoring & Logging | Dashboard Rumah |
|---|---|
| Winston log | **Buku catatan** — semua kejadian dicatat dengan waktu |
| Structured log (JSON) | Catatan rapi — "14:30 lampu dapur nyala, 14:31 AC mati" |
| Sentry | **Alarm kebakaran** — langsung bunyi + kirim notifikasi jika ada error |
| Error stack trace | **CCTV rekaman** — bisa lihat kejadian frame-by-frame |
| Uptime monitor | **Satpam** — ngecek tiap 5 menit "rumah masih aman?" |
| Response time | **Meteran listrik** — berapa cepat listrik mengalir |
| Error rate | **Persentase kerusakan** — berapa % perangkat rusak per hari |
| Throughput | **Jumlah tamu per jam** — berapa orang masuk-keluar |
| Alert (Discord/Email) | **Bel rumah** — bunyi kalau ada masalah |

Bayangkan dashboard rumah pintar: ada buku catatan digital yang mencatat semua aktivitas (logger). Ada alarm kebakaran yang bunyi otomatis kalau ada asap (Sentry). Ada satpam yang ngecek rumah tiap 5 menit (uptime monitor). Dan ada panel yang menunjukkan meteran listrik, jumlah tamu, dan status semua perangkat (metrics). Kalau ada yang aneh, bel rumah bunyi ke HP kita.

---

## Dipakai Untuk Apa

- **Production debugging** — tracing error dengan stack trace lengkap
- **Performance monitoring** — tahu endpoint mana yang lambat
- **SLA monitoring** — pastikan uptime sesuai janji ke user
- **Security audit** — track IP mencurigakan dari log
- **Capacity planning** — lihat tren usage, upgrade sebelum penuh
- **Business analytics** — hitung jumlah request, user aktif, dll

---

## Kesalahan Umum

| Kesalahan | Akibat |
|---|---|
| Log level terlalu rendah (`debug`) di production | File log membengkak, disk penuh |
| Log level terlalu tinggi (`error` saja) | Tidak tahu warning atau info penting |
| Log tidak terstruktur (string biasa) | Susah di-parse dan dicari |
| Sentry DSN hardcode di kode | Bocor — orang bisa kirim error palsu |
| Tidak ada alert | Ada error tapi tidak tahu sampai user complain |
| Log tanpa timestamp | Tidak tahu urutan kejadian |
| Tidak rotasi log | Log file bisa GB-an |

---

## Hubungan dengan Materi Sebelumnya/Selanjutnya

- **Materi 113 (Cloud Deployment)**: Setelah deploy, perlu monitoring
- **Materi 111 (Docker Compose)**: Tambahkan container untuk monitoring stack (Prometheus, Grafana)
- **Level 6 (Spesialisasi)**: Monitoring adalah fondasi untuk observability di level lanjutan

---

## Soal Latihan

### Soal 1 (Mudah)
Sebutkan 3 level log di Winston dan kapan masing-masing digunakan.

**Jawaban**:
1. `error` — aplikasi error, tidak bisa lanjut (koneksi DB putus, file corrupt)
2. `warn` — sesuatu tidak ideal tapi aplikasi tetap jalan (response > 2s, rate limiting)
3. `info` — informasi normal (server start, request datang, user login)
4. `debug` — detail untuk debugging development, **jangan di production**

### Soal 2 (Sedang)
Buat Winston logger yang:
- Log ke file `app.log` dengan format JSON (timestamp, level, message)
- Log error juga ke file terpisah `error.log`
- Rotasi file setiap 10MB, max 3 file
- Di console, tampilkan format `[LEVEL] message` dengan warna

**Jawaban**:
```javascript
const winston = require('winston');
require('winston-daily-rotate-file');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json()
  ),
  transports: [
    new winston.transports.File({
      filename: 'logs/app.log',
      maxsize: 10485760, // 10MB
      maxFiles: 3,
    }),
    new winston.transports.File({
      filename: 'logs/error.log',
      level: 'error',
      maxsize: 10485760,
      maxFiles: 3,
    }),
  ],
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.combine(
      winston.format.colorize(),
      winston.format.printf(({ timestamp, level, message, ...meta }) => {
        return `${level}: ${message} ${Object.keys(meta).length ? JSON.stringify(meta) : ''}`;
      })
    ),
  }));
}

module.exports = logger;
```

### Soal 3 (Tantangan)
Buat monitoring script lengkap yang:
1. Panggil health endpoint setiap 60 detik
2. Catat response time, status code
3. Jika 3 kali berturut-turut gagal (status != 200), kirim alert ke webhook Discord/Telegram
4. Simpan history 1 jam terakhir di memory, beri summary setiap 15 menit ke log (avg response time, error count, uptime %)

**Jawaban**:
```javascript
const https = require('https');
const logger = require('./logger'); // Winston dari soal 2

const TARGET = process.env.TARGET_URL || 'https://myapp.com/health';
const WEBHOOK_URL = process.env.ALERT_WEBHOOK_URL;
const CHECK_INTERVAL = 60000; // 60 detik
const SUMMARY_INTERVAL = 900000; // 15 menit

class HealthMonitor {
  constructor() {
    this.history = []; // [{status, duration, timestamp}, ...]
    this.consecutiveFails = 0;
    this.totalChecks = 0;
    this.failedChecks = 0;
  }

  check() {
    const start = Date.now();
    return new Promise((resolve) => {
      const req = https.get(TARGET, (res) => {
        let data = '';
        res.on('data', chunk => data += chunk);
        res.on('end', () => {
          const duration = Date.now() - start;
          resolve({ status: res.statusCode, duration });
        });
      });
      req.on('error', (err) => {
        resolve({ status: 0, duration: Date.now() - start, error: err.message });
      });
      req.setTimeout(10000, () => {
        req.destroy();
        resolve({ status: 0, duration: Date.now() - start, error: 'timeout' });
      });
    });
  }

  async run() {
    const result = await this.check();
    const now = Date.now();

    this.history.push({ ...result, timestamp: now });
    this.totalChecks++;
    if (result.status !== 200) this.failedChecks++;

    // Hapus history > 1 jam
    this.history = this.history.filter(h => now - h.timestamp < 3600000);

    // Cek consecutive fails
    if (result.status !== 200) {
      this.consecutiveFails++;
      logger.error('Health check gagal', {
        attempt: this.consecutiveFails,
        status: result.status,
        duration: result.duration,
      });

      if (this.consecutiveFails >= 3) {
        this.sendAlert(`🔴 **DOWN** — ${TARGET} gagal ${this.consecutiveFails}x berturut-turut`);
        this.consecutiveFails = 0; // reset setelah alert
      }
    } else {
      if (this.consecutiveFails > 0) {
        logger.info('Health check pulih', { previousFails: this.consecutiveFails });
        this.sendAlert(`🟢 **RECOVERED** — ${TARGET} kembali normal`);
      }
      this.consecutiveFails = 0;
    }

    // Log jika slow
    if (result.duration > 2000) {
      logger.warn('Health check lambat', { duration: result.duration });
    }
  }

  summary() {
    const window = this.history;
    if (window.length === 0) return;

    const durations = window.map(h => h.duration);
    const avgDuration = durations.reduce((a, b) => a + b, 0) / durations.length;
    const errors = window.filter(h => h.status !== 200).length;
    const uptimePct = ((window.length - errors) / window.length * 100).toFixed(2);

    logger.info('=== Health Summary (15 min) ===', {
      checks: window.length,
      avgResponseTime: `${avgDuration.toFixed(0)}ms`,
      errors,
      uptime: `${uptimePct}%`,
      maxDuration: `${Math.max(...durations)}ms`,
      minDuration: `${Math.min(...durations)}ms`,
    });
  }

  sendAlert(message) {
    if (!WEBHOOK_URL) {
      logger.warn('WEBHOOK_URL tidak diset, alert dikirim ke log saja');
      return;
    }
    // Kirim ke Discord webhook
    const postData = JSON.stringify({ content: message });
    const url = new URL(WEBHOOK_URL);
    const req = https.request({
      hostname: url.hostname,
      path: url.pathname,
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
    });
    req.write(postData);
    req.end();
  }

  start() {
    logger.info('Health Monitor started', { target: TARGET, interval: CHECK_INTERVAL });
    this.run(); // Langsung cek
    setInterval(() => this.run(), CHECK_INTERVAL);
    setInterval(() => this.summary(), SUMMARY_INTERVAL);
  }
}

const monitor = new HealthMonitor();
monitor.start();
```
