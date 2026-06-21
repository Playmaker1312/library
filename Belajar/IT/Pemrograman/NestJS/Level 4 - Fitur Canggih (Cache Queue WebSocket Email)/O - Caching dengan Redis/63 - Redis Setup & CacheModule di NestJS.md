# 63 - Redis Setup & CacheModule di NestJS

## Penjelasan
Di level sebelumnya kita sudah menyimpan data di database relasional (PostgreSQL/MySQL) dan juga MongoDB. Tapi semakin banyak pengguna yang mengakses data yang sama berulang kali, semakin lambat sistem karena harus query ke disk setiap saat. Redis hadir sebagai penyimpanan **in-memory** yang super cepat. Ibarat di gedung bertingkat, database lantai 1-3 itu seperti **gudang arsip besar** (database) tempat semua dokumen disimpan — sedangkan Redis adalah **meja resepsionis** yang menyimpan dokumen paling sering diminta sehingga tamu tidak perlu antre ke gudang setiap kali.

## Fungsi
- Menyediakan **CacheModule** dari `@nestjs/cache-manager` untuk integrasi Redis
- Mengelola koneksi Redis melalui `cache-manager-ioredis-yet` atau `ioredis`
- Menyediakan `CACHE_MANAGER` token untuk inject cache instance ke service
- Mendukung konfigurasi async via `registerAsync()` agar bisa membaca env variable

## Cara Pengimplementasian

### 1. Setup Redis via Docker

Buat file `docker-compose.yml` di root project:

```yml
services:
  redis:
    image: redis:7-alpine
    container_name: nest-redis
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data

volumes:
  redis-data:
```

Jalankan:

```bash
docker compose up -d
```

### 2. Install Package

```bash
npm install @nestjs/cache-manager cache-manager cache-manager-ioredis-yet ioredis
```

### 3. Import CacheModule

**app.module.ts** — konfigurasi async:

```typescript
import { Module } from '@nestjs/common';
import { CacheModule } from '@nestjs/cache-manager';
import { redisInsStore } from 'cache-manager-ioredis-yet';

@Module({
  imports: [
    CacheModule.registerAsync({
      useFactory: () => ({
        store: redisInsStore,
        host: process.env.REDIS_HOST || 'localhost',
        port: Number(process.env.REDIS_PORT) || 6379,
        ttl: 60, // default TTL dalam detik
        max: 100, // maksimum item di cache
      }),
    }),
  ],
})
export class AppModule {}
```

Atau sync (sederhana):

```typescript
CacheModule.register({
  store: redisInsStore,
  host: 'localhost',
  port: 6379,
  ttl: 60,
})
```

### 4. Inject CACHE_MANAGER

```typescript
import { CACHE_MANAGER } from '@nestjs/cache-manager';
import { Inject } from '@nestjs/common';
import { Cache } from 'cache-manager';

export class ProductService {
  constructor(@Inject(CACHE_MANAGER) private cacheManager: Cache) {}
}
```

## Analogi
Redis adalah **meja resepsionis** di lobi gedung. Data yang sering ditanyakan (jadwal rapat, daftar tamu) ditaruh di meja itu supaya siapapun bisa lihat tanpa harus ke gudang arsip (database). CacheModule adalah **buku panduan** yang memberitahu resepsionis cara mengatur meja — apa yang harus ditaruh, berapa lama, dan di mana tempatnya.

## Dipakai untuk Apa
- Menyimpan hasil query database yang sering diakses
- Menyimpan session user
- Rate limiting
- Distributed locking
- Cache API response

## Kesalahan Umum
- **Lupa setting TTL** — data cache numpuk terus dan memori habis
- **Tidak pakai registerAsync** — koneksi Redis gagal karena env belum terbaca
- **Install driver yang salah** — `cache-manager-ioredis-yet` vs `cache-manager-redis-store`, pastikan versi cocok dengan `@nestjs/cache-manager`
- **Redis belum running** — app jalan tapi cache silent fail, tidak ada error jelas

## Soal Latihan

**Soal:**
Buat setup Redis + CacheModule dengan konfigurasi async. Gunakan environment variable untuk host, port, dan TTL default. Pastikan CacheManager bisa di-inject ke service.

**Jawaban:**

**1. `.env`:**
```
REDIS_HOST=localhost
REDIS_PORT=6379
CACHE_TTL=120
```

**2. `app.module.ts`:**
```typescript
import { Module } from '@nestjs/common';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { CacheModule } from '@nestjs/cache-manager';
import { redisInsStore } from 'cache-manager-ioredis-yet';

@Module({
  imports: [
    ConfigModule.forRoot(),
    CacheModule.registerAsync({
      imports: [ConfigModule],
      inject: [ConfigService],
      useFactory: (config: ConfigService) => ({
        store: redisInsStore,
        host: config.get('REDIS_HOST'),
        port: config.get('REDIS_PORT'),
        ttl: config.get('CACHE_TTL'),
      }),
    }),
  ],
})
export class AppModule {}
```

**3. `product.service.ts`:**
```typescript
import { Injectable, Inject } from '@nestjs/common';
import { CACHE_MANAGER } from '@nestjs/cache-manager';
import { Cache } from 'cache-manager';

@Injectable()
export class ProductService {
  constructor(@Inject(CACHE_MANAGER) private cacheManager: Cache) {}

  async testConnection() {
    await this.cacheManager.set('ping', 'pong', 60);
    const result = await this.cacheManager.get('ping');
    console.log('Redis test:', result); // pong
  }
}
```
