# 95 - Distributed Tracing - OpenTelemetry

## Penjelasan
Structured logging (materi 94) memberi kita log per request dengan correlation ID. Tapi correlation ID hanya menghubungkan log — tidak memberi tahu *berapa lama* setiap operasi. OpenTelemetry (OTEL) adalah standar open-source untuk *distributed tracing*: melacak perjalanan request melewati service, database, queue, dan cache — lengkap dengan durasi setiap langkah.

## Fungsi
- **Observability 3 pilar**: Metrics (angka), Logs (teks), Traces (waktu & hubungan).
- **OpenTelemetry**: Framework vendor-agnostic untuk instrumentasi telemetry.
- **Auto-instrumentation**: Detect dan trace HTTP request, database query, message queue secara otomatis.
- **Jaeger / Tempo**: Backend untuk menyimpan dan visualisasi trace.

## Cara Pengimplementasian

### Install
```bash
npm install @opentelemetry/sdk-node \
  @opentelemetry/api \
  @opentelemetry/auto-instrumentations-node \
  @opentelemetry/exporter-trace-otlp-http \
  @opentelemetry/instrumentation-http \
  @opentelemetry/instrumentation-express \
  @opentelemetry/instrumentation-nestjs-core
```

### `otel-sdk.ts` — Setup OpenTelemetry
```typescript
import { NodeSDK } from '@opentelemetry/sdk-node';
import { getNodeAutoInstrumentations } from '@opentelemetry/auto-instrumentations-node';
import { OTLPTraceExporter } from '@opentelemetry/exporter-trace-otlp-http';
import { Resource } from '@opentelemetry/resources';
import { ATTR_SERVICE_NAME } from '@opentelemetry/semantic-conventions';

const otelSDK = new NodeSDK({
  resource: new Resource({
    [ATTR_SERVICE_NAME]: 'nestjs-app',
  }),
  traceExporter: new OTLPTraceExporter({
    url: process.env.OTEL_EXPORTER_OTLP_ENDPOINT || 'http://localhost:4318/v1/traces',
  }),
  instrumentations: [
    getNodeAutoInstrumentations({
      '@opentelemetry/instrumentation-fs': { enabled: false },
    }),
  ],
});

export default otelSDK;
```

### `tracing.ts` — Initialize Before App
```typescript
import otelSDK from './otel-sdk';

async function initializeTracing() {
  try {
    await otelSDK.start();
    console.log('[Tracing] OpenTelemetry initialized');
  } catch (error) {
    console.error('[Tracing] Failed to initialize', error);
  }
}

export { initializeTracing };
```

### `main.ts` — Start Tracing First
```typescript
import { initializeTracing } from './common/tracing';

async function bootstrap() {
  // Start tracing BEFORE creating app
  await initializeTracing();

  const { NestFactory } = await import('@nestjs/core');
  const { AppModule } = await import('./app.module');

  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```

### Custom Span di Service
```typescript
import { Injectable } from '@nestjs/common';
import { trace, Span } from '@opentelemetry/api';

@Injectable()
export class OrdersService {
  private readonly tracer = trace.getTracer('orders-service');

  async processOrder(orderId: string) {
    // Create custom span
    return this.tracer.startActiveSpan('processOrder', async (span: Span) => {
      span.setAttribute('order.id', orderId);

      try {
        const result = await this.doWork();
        span.setStatus({ code: 1 }); // OK
        return result;
      } catch (error) {
        span.recordException(error);
        span.setStatus({ code: 2, message: error.message }); // ERROR
        throw error;
      } finally {
        span.end();
      }
    });
  }

  private async doWork() {
    // Simulasi
    await new Promise(r => setTimeout(r, 100));
    return { status: 'ok' };
  }
}
```

### Docker Compose untuk Jaeger
```yaml
# docker-compose.monitoring.yml
version: '3.8'
services:
  jaeger:
    image: jaegertracing/all-in-one:latest
    container_name: jaeger
    ports:
      - "16686:16686"   # UI
      - "4318:4318"     # OTLP HTTP
      - "4317:4317"     # OTLP gRPC
    environment:
      - COLLECTOR_OTLP_ENABLED=true
```

### Visualisasi Trace di Jaeger
```
Buka http://localhost:16686
Cari service "nestjs-app"
Lihat waterfall diagram:
├── HTTP GET /orders/123          (200ms)
│   ├── middleware                (2ms)
│   ├── OrdersController.findAll  (5ms)
│   ├── OrdersService.processOrder(180ms)
│   │   ├── Prisma.query          (50ms)
│   │   └── Redis.get             (30ms)
│   └── Response                  (1ms)
```

## Analogi
Gedung dipasangi **kamera CCTV di setiap sudut** (auto-instrumentation). Setiap kali seseorang bergerak — masuk lift (HTTP request), ambil arsip (database query), buka lemari (Redis get) — kamera merekam: siapa, dari mana, berapa lama, ke mana selanjutnya. Semua rekaman dikirim ke **ruang kontrol** (Jaeger UI). Di ruang kontrol, kita bisa putar ulang (waterfall trace) untuk melihat: "Oh, lift lambat hari ini karena banyak orang di lantai 3 — database query-nya slow."

## Dipakai untuk apa
- Microservices — melacak request melewati 5+ service berbeda.
- Performance troubleshooting — menemukan bottleneck (database slow? Redis timeout?).
- SLA monitoring — berapa persen request yang selesai <500ms.
- Root cause analysis — trace menunjukkan persis di langkah mana error terjadi.

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Initialize OTEL setelah app start | Instrumentasi tidak menangkap request awal | Panggil `otelSDK.start()` SEBELUM `NestFactory.create()` |
| Tidak export trace ke backend | OTEL berjalan tapi data hilang | Konfigurasi OTLP exporter ke Jaeger/Tempo |
| Sampling rate 100% di production | Biaya storage sangat tinggi | Gunung sampling rate 10-20% atau head-based sampling |
| Tidak menambahkan attribute ke span | Trace tidak informatif | Tambahkan `span.setAttribute('key', value)` |
| Lupa record exception di span | Error tidak tercatat di trace | `span.recordException(error)` di catch block |

## Soal Latihan

**Soal 1:** Setup Jaeger dengan Docker Compose dan konfigurasi aplikasi NestJS untuk mengirim trace ke Jaeger.

**Jawaban 1:**
```yaml
# docker-compose.monitoring.yml
version: '3.8'
services:
  jaeger:
    image: jaegertracing/all-in-one:latest
    ports:
      - "16686:16686"
      - "4318:4318"
    environment:
      - COLLECTOR_OTLP_ENABLED=true
```
```bash
docker compose -f docker-compose.monitoring.yml up -d
```
Aplikasi akan mengirim trace ke `http://localhost:4318/v1/traces` secara otomatis.

**Soal 2:** Buat custom span untuk operasi `createUser` yang mencatat `user.email` sebagai attribute.

**Jawaban 2:**
```typescript
import { trace, Span } from '@opentelemetry/api';

@Injectable()
export class UsersService {
  private tracer = trace.getTracer('users-service');

  async createUser(dto: CreateUserDto) {
    return this.tracer.startActiveSpan('createUser', async (span: Span) => {
      span.setAttribute('user.email', dto.email);
      try {
        const user = await this.prisma.user.create({ data: dto });
        span.setStatus({ code: 1 });
        return user;
      } catch (error) {
        span.recordException(error);
        span.setStatus({ code: 2, message: error.message });
        throw error;
      } finally {
        span.end();
      }
    });
  }
}
```
