# 144: Observability — Prometheus, Grafana, Jaeger

## 1) Penjelasan

Observability ≠ Monitoring. Monitoring hanya "tahu ada masalah". Observability: **tahu kenapa** ada masalah. Tiga pilar observability: **Metrics** (angka), **Logs** (teks), **Traces** (jejak request). Alat: Prometheus (metrics), Grafana (dashboard), Jaeger (tracing), Loki/ELK (logs).

## 2) Fungsi

- **Metrics (Prometheus)**: Kumpulkan data numerik — CPU, memory, request rate, error rate.
- **Logs (Loki/ELK)**: Catatan event — stack trace, error messages, access logs.
- **Traces (Jaeger)**: Tracking satu request melewati berbagai service — latency per service.
- **Grafana**: Visualisasi — dashboard yang menyatukan semua data observability.
- **Alerting**: Kirim notifikasi (Slack, Email, PagerDuty) jika threshold terlampaui.

## 3) Code/Config

```yaml
# prometheus.yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'express-api'
    static_configs:
      - targets: ['localhost:3000']
    metrics_path: '/metrics'

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['localhost:9100']
---
# docker-compose.observability.yml
version: '3.8'
services:
  prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin123
    ports:
      - "3000:3000"
    depends_on:
      - prometheus

  jaeger:
    image: jaegertracing/all-in-one:1.54
    ports:
      - "16686:16686"   # UI
      - "4318:4318"     # OTLP HTTP
```

```javascript
// Instrumentasi Express dengan OpenTelemetry
const { NodeTracerProvider } = require('@opentelemetry/sdk-trace-node');
const { PrometheusExporter } = require('@opentelemetry/exporter-prometheus');

const provider = new NodeTracerProvider();
provider.register();

const meter = new PrometheusExporter({ port: 9464 });
// ... metric counter, histogram untuk request duration
```

## 4) Analogi Rumah (Tabel)

| Konsep Observability | Analogi Rumah |
|----------------------|---------------|
| **Observability** | Dashboard pesawat — tahu status, penyebab, dan riwayat |
| **Monitoring** | Lampu indikator "mesin nyala" saja |
| **Metrics** | Suhu mesin, kecepatan, ketinggian (angka) |
| **Logs** | Black box recorder — log penerbangan detail |
| **Traces** | Tracking jalur pesawat dari A ke B |
| **Grafana** | Layar dashboard di kokpit |
| **Prometheus** | Sensor yang mengumpulkan data dari seluruh pesawat |
| **Alerting** | Alarm "mesin kiri overheat" |

## 5) Use Case

- Debugging slow API — trace menunjukkan bottleneck di service mana
- Capacity planning — lihat tren metrics selama seminggu terakhir
- Post-mortem incident — logs + traces untuk analisa root cause

## 6) Kesalahan Umum

- **Hanya monitoring, tanpa tracing**: Tahu API lambat tapi tidak tahu penyebabnya.
- **Terlalu banyak metrics tanpa labeling**: "request_total" tanpa label method/path — susah analisa.
- **Log level terlalu rendah di production**: Debug log penuh sampai storage penuh dan cost mahal.

## 7) Benang Merah

Dari **Ansible** (143) yang setup server. Ansible juga bisa setup Prometheus/Grafana. Selanjutnya: **Chaos Engineering** (145) yang menggunakan observability untuk validasi eksperimen.

## 8) Soal

**1. Apa beda monitoring dan observability?**
Monitoring tahu "server down". Observability tahu "server down karena OOM, disebabkan memory leak di service auth".

**2. Apa fungsi tracing (Jaeger) yang tidak bisa dilakukan metrics/logs?**
Tracing melacak satu request melewati banyak service — tahu persis waktu yang dihabiskan di tiap service.

**3. Kenapa Prometheus menggunakan model pull (scrape), bukan push?**
Agar lebih mudah detect service down (kalau tidak bisa scrape = mati) dan tidak perlu konfigurasi target di tiap service.
