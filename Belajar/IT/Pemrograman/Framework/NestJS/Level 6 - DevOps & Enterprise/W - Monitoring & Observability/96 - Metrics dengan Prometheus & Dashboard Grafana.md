# 96 - Metrics dengan Prometheus & Dashboard Grafana

## Penjelasan
Distributed tracing (materi 95) memberi kita detail *per request*. Tapi bagaimana dengan gambaran besar — berapa request per detik? Berapa error rate? Berapa memory usage? Metrics menjawab ini. Prometheus mengumpulkan metrics secara periodik, Grafana memvisualisasikannya dalam dashboard. `prom-client` adalah library Prometheus untuk Node.js.

## Fungsi
- **prom-client**: Library resmi Prometheus untuk Node.js, expose `/metrics` endpoint.
- **Default metrics**: CPU, memory, event loop lag, garbage collection.
- **Custom metrics**: Request count, order total, active users, durasi response.
- **Grafana dashboard**: Visualisasi real-time dengan panel grafik.
- **Alerting**: Notifikasi jika metrics melewati threshold (misal: error rate >5%).

## Cara Pengimplementasian

### Install
```bash
npm install prom-client @willsoto/nestjs-prometheus
```

### Prometheus Module
```typescript
import { Module } from '@nestjs/common';
import { PrometheusModule } from '@willsoto/nestjs-prometheus';
import { MetricsController } from './metrics.controller';
import { CustomMetricsService } from './custom-metrics.service';

@Module({
  imports: [
    PrometheusModule.register({
      defaultMetrics: {
        enabled: true,
        config: {
          prefix: 'nestjs_',
        },
      },
      path: '/metrics',          // Endpoint untuk Prometheus scrape
    }),
  ],
  controllers: [MetricsController],
  providers: [CustomMetricsService],
  exports: [CustomMetricsService],
})
export class MonitoringModule {}
```

### Custom Metrics Service
```typescript
import { Injectable } from '@nestjs/common';
import { Counter, Histogram, Gauge } from 'prom-client';

@Injectable()
export class CustomMetricsService {
  private readonly httpRequestCounter: Counter<string>;
  private readonly httpRequestDuration: Histogram<string>;
  private readonly activeUsersGauge: Gauge<string>;
  private readonly orderTotalCounter: Counter<string>;

  constructor() {
    this.httpRequestCounter = new Counter({
      name: 'nestjs_http_requests_total',
      help: 'Total HTTP requests',
      labelNames: ['method', 'path', 'status'],
    });

    this.httpRequestDuration = new Histogram({
      name: 'nestjs_http_request_duration_seconds',
      help: 'HTTP request duration in seconds',
      labelNames: ['method', 'path'],
      buckets: [0.01, 0.05, 0.1, 0.5, 1, 2, 5], // dalam detik
    });

    this.activeUsersGauge = new Gauge({
      name: 'nestjs_active_users',
      help: 'Number of active users',
    });

    this.orderTotalCounter = new Counter({
      name: 'nestjs_orders_total',
      help: 'Total orders placed',
      labelNames: ['status'], // success / failed
    });
  }

  incrementHttpRequest(method: string, path: string, status: number) {
    this.httpRequestCounter.inc({ method, path, status });
  }

  observeHttpRequestDuration(method: string, path: string, durationSeconds: number) {
    this.httpRequestDuration.observe({ method, path }, durationSeconds);
  }

  setActiveUsers(count: number) {
    this.activeUsersGauge.set(count);
  }

  incrementOrder(status: 'success' | 'failed') {
    this.orderTotalCounter.inc({ status });
  }
}
```

### Middleware untuk Metrics
```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';
import { CustomMetricsService } from './custom-metrics.service';

@Injectable()
export class MetricsMiddleware implements NestMiddleware {
  constructor(private metrics: CustomMetricsService) {}

  use(req: Request, res: Response, next: NextFunction) {
    const start = Date.now();
    const { method, path } = req;

    res.on('finish', () => {
      const duration = (Date.now() - start) / 1000;
      this.metrics.incrementHttpRequest(method, path, res.statusCode);
      this.metrics.observeHttpRequestDuration(method, path, duration);
    });

    next();
  }
}
```

### Docker Compose — Prometheus + Grafana
```yaml
version: '3.8'
services:
  app:
    build: .
    ports: ["3000:3000"]

  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml:ro
      - prometheus-data:/prometheus
    ports: ["9090:9090"]

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports: ["3001:3000"]
    volumes:
      - grafana-data:/var/lib/grafana
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin

volumes:
  prometheus-data:
  grafana-data:
```

### `prometheus.yml`
```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'nestjs-app'
    static_configs:
      - targets: ['app:3000']
    metrics_path: '/metrics'
```

### Setup Grafana
1. Buka `http://localhost:3001` (login: admin/admin)
2. Add data source → Prometheus → URL: `http://prometheus:9090`
3. Import dashboard → bisa pakai ID 14513 (NestJS Dashboard) atau buat custom
4. Panel contoh:
   - **Request Rate**: `rate(nestjs_http_requests_total[5m])`
   - **Error Rate**: `rate(nestjs_http_requests_total{status=~"5.."}[5m])`
   - **P95 Latency**: `histogram_quantile(0.95, rate(nestjs_http_request_duration_seconds_bucket[5m]))`
   - **Active Users**: `nestjs_active_users`
   - **CPU**: `process_cpu_seconds_total`

### Alerting Rule (`alerts.yml`)
```yaml
groups:
  - name: nestjs
    rules:
      - alert: HighErrorRate
        expr: rate(nestjs_http_requests_total{status=~"5.."}[5m]) > 0.05
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "Error rate >5% for 5 minutes"

      - alert: HighLatency
        expr: histogram_quantile(0.95, rate(nestjs_http_request_duration_seconds_bucket[5m])) > 2
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "P95 latency >2s"
```

## Analogi
Gedung dipasangi **sensor di setiap lantai**: sensor suhu (CPU), sensor getaran (memory), sensor jumlah orang (active users), sensor waktu lift (request duration). Semua sensor mengirim data ke **ruang kontrol** (Prometheus) setiap 15 detik. Di ruang kontrol, ada **layar besar** (Grafana) yang menampilkan grafik: "Lantai 3 suhu naik — mungkin AC rusak, lift lantai 1-5 rata-rata 30 detik — ada bottleneck." Ada **alarm** (alerting) yang bunyi jika suhu >40°C atau lift >60 detik.

## Dipakai untuk apa
- Monitoring real-time — dashboard Grafana di monitor TV tim.
- Capacity planning — lihat trend penggunaan resource.
- SLA reporting — bukti uptime dan performance ke client.
- Auto-scaling — berdasarkan metrics (misal: request rate >1000 → spin up container baru).

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Tidak register default metrics | Tidak ada data CPU/memory | Set `defaultMetrics: { enabled: true }` |
| Cardinality explosion (label dinamis) | Prometheus kehabisan memory | Batasi label — jangan pakai userID sebagai label |
| Bucket histogram tidak sesuai | Data latency tidak akurat | Sesuaikan bucket dengan SLO (misal: 0.1, 0.5, 1, 2) |
| Prometheus tidak bisa scrape | Grafana kosong | Cek network dan `metrics_path` |
| Lupa pasang middleware metrics | Semua metrics = 0 | Register MetricsMiddleware untuk semua route |

## Soal Latihan

**Soal 1:** Buat custom metric `nestjs_orders_total` dengan label `status` (success/failed) dan increment di service.

**Jawaban 1:**
```typescript
// custom-metrics.service.ts
private readonly orderCounter = new Counter({
  name: 'nestjs_orders_total',
  help: 'Total orders',
  labelNames: ['status'],
});

// Di service
async createOrder(dto: CreateOrderDto) {
  try {
    const order = await this.prisma.order.create({ data: dto });
    this.metrics.incrementOrder('success');
    return order;
  } catch (error) {
    this.metrics.incrementOrder('failed');
    throw error;
  }
}
```

**Soal 2:** Buat query PromQL untuk menampilkan rata-rata response time 5 menit terakhir per endpoint.

**Jawaban 2:**
```promql
# Rata-rata durasi per endpoint
rate(nestjs_http_request_duration_seconds_sum[5m]) / rate(nestjs_http_request_duration_seconds_count[5m])

# Atau pakai histogram_quantile untuk P50
histogram_quantile(0.50, rate(nestjs_http_request_duration_seconds_bucket[5m]))
```
