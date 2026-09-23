# 93 - Health Checks & Graceful Shutdown

## Penjelasan
Sebelumnya kita membuat pipeline CI/CD yang melakukan health check setelah deploy. Kini kita implementasikan health check endpoint secara proper di dalam aplikasi NestJS. Health check penting agar orchestrator (Docker, Kubernetes) tahu apakah aplikasi sehat. Graceful shutdown memastikan koneksi database, Redis, dan request in-flight tertutup rapi saat aplikasi dimatikan.

## Fungsi
- **@nestjs/terminus**: Library resmi untuk health checks.
- **Custom health indicator**: Cek database, Redis, disk usage, dll.
- **app.enableShutdownHooks()**: Mendaftarkan handler SIGTERM/SIGINT.
- **SIGTERM handling**: Docker/k8s kirim SIGTERM → aplikasi tutup koneksi dengan rapi.

## Cara Pengimplementasian

### Install
```bash
npm install @nestjs/terminus @prisma/client
```

### Health Module
```typescript
import { Module } from '@nestjs/common';
import { TerminusModule } from '@nestjs/terminus';
import { HealthController } from './health.controller';
import { PrismaHealthIndicator } from './prisma.health';
import { PrismaModule } from '../prisma/prisma.module';
import { RedisHealthModule } from '../redis/redis-health.module';

@Module({
  imports: [
    TerminusModule.forRoot({
      gracefulShutdownTimeoutMs: 1000, // 1 detik
    }),
    PrismaModule,
    RedisHealthModule,
  ],
  controllers: [HealthController],
  providers: [PrismaHealthIndicator],
})
export class HealthModule {}
```

### Health Controller
```typescript
import { Controller, Get } from '@nestjs/common';
import {
  HealthCheckService,
  HealthCheck,
  DiskHealthIndicator,
  MemoryHealthIndicator,
} from '@nestjs/terminus';
import { PrismaHealthIndicator } from './prisma.health';
import { RedisHealthIndicator } from './redis.health';

@Controller('health')
export class HealthController {
  constructor(
    private health: HealthCheckService,
    private prisma: PrismaHealthIndicator,
    private redis: RedisHealthIndicator,
    private disk: DiskHealthIndicator,
    private memory: MemoryHealthIndicator,
  ) {}

  @Get()
  @HealthCheck()
  check() {
    return this.health.check([
      () => this.prisma.isHealthy('database'),
      () => this.redis.isHealthy('redis'),
      () => this.disk.checkStorage('disk', {
        thresholdPercent: 0.9,  // 90% disk usage = unhealthy
        path: '/',
      }),
      () => this.memory.checkHeap('memory_heap', 300 * 1024 * 1024), // 300MB max
    ]);
  }
}
```

### Prisma Health Indicator
```typescript
import { Injectable } from '@nestjs/common';
import { HealthIndicator, HealthIndicatorResult, HealthCheckError } from '@nestjs/terminus';
import { PrismaService } from '../prisma/prisma.service';

@Injectable()
export class PrismaHealthIndicator extends HealthIndicator {
  constructor(private prisma: PrismaService) {
    super();
  }

  async isHealthy(key: string): Promise<HealthIndicatorResult> {
    try {
      await this.prisma.$queryRaw`SELECT 1`;
      return this.getStatus(key, true);
    } catch (e) {
      throw new HealthCheckError('Prisma check failed', this.getStatus(key, false, { message: e.message }));
    }
  }
}
```

### Graceful Shutdown di `main.ts`
```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Enable shutdown hooks (SIGTERM, SIGINT)
  app.enableShutdownHooks();

  await app.listen(3000);
}
bootstrap();
```

### Listener Shutdown di Service
```typescript
import { Injectable, OnApplicationShutdown } from '@nestjs/common';
import { PrismaClient } from '@prisma/client';

@Injectable()
export class PrismaService extends PrismaClient implements OnApplicationShutdown {
  async onApplicationShutdown(signal?: string) {
    console.log(`[Prisma] Disconnecting on ${signal}`);
    await this.$disconnect();
  }
}
```

### Verifikasi
```bash
# Cek health endpoint
curl http://localhost:3000/health
# Response:
# {
#   "status": "ok",
#   "info": { "database": { "status": "up" }, "redis": { "status": "up" } },
#   "error": {},
#   "details": { ... }
# }

# Test graceful shutdown
docker stop nestjs-app --time=30  # Kirim SIGTERM, tunggu 30 detik
```

## Analogi
Gedung punya **panel status** di lobi (health endpoint). Panel menunjukkan: lift berfungsi (database), listrik menyala (Redis), kapasitas genset 70% (disk/memory). Ketika pemilik ingin menutup gedung (SIGTERM), **satpam** tidak langsung mematikan semua listrik — dia menunggu semua penghuni keluar lift dulu (request in-flight selesai), baru matikan AC, lift, dan listrik (disconnect DB, Redis, close server). Ini graceful shutdown.

## Dipakai untuk apa
- Load balancer / orchestrator tahu instance mana yang sehat.
- Docker health check (lihat materi 91) merujuk ke endpoint ini.
- Monitoring — dashboard menunjukkan status seluruh service.
- Deployment zero-downtime — container baru di-start hanya saat health check lulus.

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Tidak enableShutdownHooks() | SIGTERM diabaikan, container dipaksa kill (SIGKILL) setelah timeout | Panggil `app.enableShutdownHooks()` |
| Tidak disconnect database | Koneksi menggantung sampai timeout DB | Implement `OnApplicationShutdown` |
| Health check timeout terlalu pendek | Container dianggap unhealthy padahal hanya lambat | Set `gracefulShutdownTimeoutMs` dan `timeout` health check wajar |
| Disk health check di path yang tidak ada | Error `ENOENT` | Gunakan path yang ada (`/` atau `/app`) |
| Health check endpoint tanpa auth | Ekspos informasi internal | Jangan ekspos detail stack trace di health response |

## Soal Latihan

**Soal 1:** Implementasikan Redis health indicator menggunakan `@nestjs/terminus`.

**Jawaban 1:**
```typescript
import { Injectable } from '@nestjs/common';
import { HealthIndicator, HealthIndicatorResult, HealthCheckError } from '@nestjs/terminus';
import Redis from 'ioredis';

@Injectable()
export class RedisHealthIndicator extends HealthIndicator {
  constructor(private redis: Redis) { super(); }

  async isHealthy(key: string): Promise<HealthIndicatorResult> {
    try {
      const ping = await this.redis.ping();
      if (ping !== 'PONG') throw new Error('Redis not responding');
      return this.getStatus(key, true);
    } catch (e) {
      throw new HealthCheckError('Redis check failed', this.getStatus(key, false, { message: e.message }));
    }
  }
}
```

**Soal 2:** Apa yang terjadi jika kita tidak mengimplementasikan `OnApplicationShutdown` di PrismaService, lalu container menerima SIGTERM?

**Jawaban 2:** Container akan dipaksa berhenti dalam 10 detik (default Docker stop timeout). Koneksi Prisma ke PostgreSQL akan terputus secara paksa, menyebabkan:
- Koneksi menggantung di PostgreSQL sampai `idle_in_transaction_session_timeout`
- Potensi data corruption jika ada transaksi yang belum selesai
- Log error "Connection pool closed unexpectedly"

Implementasi `onApplicationShutdown` dengan `this.$disconnect()` memastikan pool koneksi ditutup dengan rapi.
