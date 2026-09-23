# 89 - API Versioning - Backward Compatibility

## Penjelasan

Setelah aplikasi berjalan dan digunakan oleh banyak klien (mobile app, web, third-party), kita tidak bisa sembarangan mengubah API. Perubahan yang **breaking** akan merusak aplikasi klien yang sudah berjalan.

**API Versioning** adalah strategi untuk memperkenalkan perubahan tanpa merusak klien lama. NestJS menyediakan built-in versioning yang bisa dikonfigurasi secara global atau per controller.

Ada beberapa strategi versioning:
1. **URI Versioning** — `/api/v1/products`, `/api/v2/products`
2. **Header Versioning** — `Accept-Version: v1` atau custom header `X-API-Version: v1`
3. **Media Type Versioning** — `Accept: application/json; version=1`

## Fungsi

- Memungkinkan **backward compatibility** — klien lama tetap bisa pakai endpoint lama
- Memberi waktu transisi bagi klien untuk migrasi ke versi baru
- NestJS built-in versioning (`enableVersioning`) — mudah diatur
- Strategi **deprecation** — memberitahu klien bahwa versi lama akan dihapus

## Cara Pengimplementasian

### 1. Global Setup Versioning

```typescript
// src/main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { VersioningType } from '@nestjs/common';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  app.enableVersioning({
    type: VersioningType.URI,
    prefix: 'v',
    defaultVersion: '1',
  });

  await app.listen(3000);
}
bootstrap();
```

### 2. Controller dengan Multiple Versions

```typescript
// src/presentation/controllers/product.controller.ts
import { Controller, Get } from '@nestjs/common';

@Controller({ path: 'products', version: '1' })
export class ProductControllerV1 {
  @Get()
  findAll() {
    return { version: 'v1', products: [] };
  }
}

@Controller({ path: 'products', version: '2' })
export class ProductControllerV2 {
  @Get()
  findAll() {
    return {
      version: 'v2',
      data: [],
      meta: { total: 0, page: 1, limit: 10 },
    };
  }
}
```

### 3. Versioning per Endpoint

```typescript
// src/presentation/controllers/order.controller.ts
import { Controller, Get, Version } from '@nestjs/common';

@Controller('orders')
export class OrderController {
  @Get()
  @Version('1')
  findAllV1() {
    return { version: 'v1', orders: [] };
  }

  @Get()
  @Version('2')
  findAllV2() {
    return {
      version: 'v2',
      data: [],
      meta: { total: 0, page: 1, limit: 10 },
    };
  }

  @Get('health')
  @Version(Version.NEUTRAL)
  health() {
    return { status: 'ok' };
  }
}
```

### 4. Header Versioning

```typescript
// src/main.ts — header versioning
app.enableVersioning({
  type: VersioningType.HEADER,
  header: 'Accept-Version',
  defaultVersion: '1',
});

// Penggunaan dari klien:
// curl -H "Accept-Version: 2" http://localhost:3000/products
```

### 5. V2 Endpoint dengan Perubahan Breaking

```typescript
// v1: response { name: string, price: number }
// v2: response { name: string, price: { amount: number, currency: string } }

// src/presentation/controllers/product.controller.ts
import { Controller, Get, Param } from '@nestjs/common';

@Controller({ path: 'products', version: '1' })
export class ProductControllerV1 {
  constructor(private readonly useCase: GetProductUseCase) {}

  @Get(':id')
  async findById(@Param('id') id: string) {
    const product = await this.useCase.execute(id);
    return { id: product.id, name: product.name, price: product.price };
  }
}

@Controller({ path: 'products', version: '2' })
export class ProductControllerV2 {
  constructor(private readonly useCase: GetProductUseCase) {}

  @Get(':id')
  async findById(@Param('id') id: string) {
    const product = await this.useCase.execute(id);
    return {
      id: product.id,
      name: product.name,
      price: { amount: product.price, currency: 'IDR' },
    };
  }
}
```

### 6. Module Setup untuk Multiple Versions

```typescript
// src/presentation/modules/product.module.ts
import { Module } from '@nestjs/common';
import { CqrsModule } from '@nestjs/cqrs';
import { ProductControllerV1 } from '../controllers/product-v1.controller';
import { ProductControllerV2 } from '../controllers/product-v2.controller';
import { GetProductHandler } from '../../application/queries/get-product.handler';
import { InfrastructureModule } from '../../infrastructure/infrastructure.module';

@Module({
  imports: [CqrsModule, InfrastructureModule],
  controllers: [ProductControllerV1, ProductControllerV2],
  providers: [GetProductHandler],
})
export class ProductModule {}
```

### 7. Deprecation Strategy

```typescript
// src/presentation/controllers/product.controller.ts
import { Controller, Get, Headers, GoneException } from '@nestjs/common';

@Controller({ path: 'products', version: '1' })
export class ProductControllerV1 {
  @Get()
  findAll(@Headers('X-API-Version') version: string) {
    // Kirim deprecation warning di header
    // Klien bisa melihat header ini dan migrasi
    const response = { version: 'v1', products: [] };

    return response;
  }
}

// V1 dihapus total — return 410 Gone
@Controller({ path: 'products', version: '1' })
export class DeprecatedProductController {
  @Get()
  findAll() {
    throw new GoneException({
      message: 'API v1 is deprecated and removed. Please use v2.',
      migrationGuide: 'https://docs.api.com/migration-v1-to-v2',
      sunsetDate: '2026-06-01',
    });
  }
}
```

### 8. Integrasi Test untuk API Versioning

```typescript
// test/integration/api-versioning.spec.ts
import { app, httpServer, request } from '../setup';

describe('API Versioning', () => {
  it('v1 products should return old format', async () => {
    const res = await request(httpServer)
      .get('/v1/products')
      .expect(200);

    expect(res.body).toHaveProperty('version', 'v1');
    expect(res.body).toHaveProperty('products');
  });

  it('v2 products should return new format', async () => {
    const res = await request(httpServer)
      .get('/v2/products')
      .expect(200);

    expect(res.body).toHaveProperty('version', 'v2');
    expect(res.body).toHaveProperty('data');
    expect(res.body).toHaveProperty('meta');
  });

  it('neutral endpoint should work without version', async () => {
    const res = await request(httpServer)
      .get('/orders/health')
      .expect(200);

    expect(res.body.status).toBe('ok');
  });

  it('should return 404 for non-existent version', async () => {
    await request(httpServer)
      .get('/v99/products')
      .expect(404);
  });
});
```

### 9. Middleware untuk Version Logging

```typescript
// src/infrastructure/middleware/version-logger.middleware.ts
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class VersionLoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    const version = req.url.match(/\/v(\d+)\//)?.[1] || 'neutral';

    res.on('finish', () => {
      console.log(`[${new Date().toISOString()}] ${req.method} ${req.url} -> ${res.statusCode} (v${version})`);
    });

    next();
  }
}
```

## Analogi — Gedung Bertingkat

| Konsep | Analogi Gedung |
|--------|----------------|
| **API v1** | Pintu masuk utama di lantai 1 — sudah dipakai semua orang |
| **API v2** | Pintu masuk baru di lantai 1 — lebih lebar, ada ramp untuk kursi roda |
| **Versioning URI** | Plang di atas pintu: "PINTU UTAMA V1" dan "PINTU UTAMA V2" |
| **Header Versioning** | Pengunjung menunjukkan kartu "saya pengunjung V2" di pintu yang sama, petugas mengarahkan ke dalam sesuai versi |
| **Deprecation** | Papan pengumuman "Pintu V1 akan ditutup 1 Juni 2026, gunakan pintu V2" |
| **Backward Compatibility** | Pintu lama tetap terbuka sampai semua orang pindah ke pintu baru |
| **Version.NEUTRAL** | Pintu darurat yang selalu bisa dipakai siapa saja, versi berapa pun |

## Dipakai untuk Apa

- **Public API** yang digunakan oleh banyak klien eksternal
- **Mobile app** — user tidak bisa diwajibkan update app versi terbaru
- **Third-party integration** — partner integration butuh stabilitas
- **Gradual migration** — memperkenalkan perubahan besar secara bertahap

## Kesalahan Umum yang Sering Terjadi

1. **Versioning di endpoint, tidak di module** — mencampur v1 dan v2 di controller yang sama menyebabkan kode sulit dibaca; pisahkan controller atau bahkan module terpisah
2. **Lupa set default version** — klien tanpa version header mendapatkan 404 karena tidak ada default; selalu set `defaultVersion`
3. **V1 dan V2 berbagi DTO yang sama** — perubahan DTO v2 otomatis mengubah v1; buat DTO terpisah untuk setiap versi
4. **Terlalu cepat menghapus versi lama** — klien butuh waktu migrasi; beri deprecation notice minimal 6 bulan sebelum sunset
5. **Versi mayor terlalu sering** — versi 1.0, lalu 1.1, lalu 2.0 dalam 3 bulan; versioning API adalah komitmen jangka panjang, jangan ganti versi untuk perubahan kecil

## Soal Latihan

### Soal 1: Setup API Versioning

Buat implementasi berikut:
- Enable URI versioning di NestJS dengan prefix `/api/v1/`
- Buat `UserControllerV1` dengan endpoint `GET /api/v1/users` yang mengembalikan `{ users: [], total: 0 }`
- Buat `UserControllerV2` dengan endpoint `GET /api/v2/users` yang mengembalikan `{ data: [], meta: { total: 0, page: 1 } }`
- Buat endpoint neutral `GET /api/health` yang mengembalikan `{ status: 'ok' }`

### Jawaban 1:

```typescript
// main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { VersioningType } from '@nestjs/common';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  app.setGlobalPrefix('api');

  app.enableVersioning({
    type: VersioningType.URI,
    prefix: 'v',
    defaultVersion: '1',
  });

  await app.listen(3000);
}
bootstrap();

// controllers/user-v1.controller.ts
@Controller({ path: 'users', version: '1' })
export class UserControllerV1 {
  @Get()
  findAll() {
    return { users: [], total: 0 };
  }
}

// controllers/user-v2.controller.ts
@Controller({ path: 'users', version: '2' })
export class UserControllerV2 {
  @Get()
  findAll() {
    return { data: [], meta: { total: 0, page: 1, limit: 10 } };
  }
}

// controllers/health.controller.ts
@Controller({ path: 'health', version: Version.NEUTRAL })
export class HealthController {
  @Get()
  check() {
    return { status: 'ok', timestamp: new Date().toISOString() };
  }
}

// user.module.ts
@Module({
  controllers: [UserControllerV1, UserControllerV2, HealthController],
})
export class UserModule {}
```
