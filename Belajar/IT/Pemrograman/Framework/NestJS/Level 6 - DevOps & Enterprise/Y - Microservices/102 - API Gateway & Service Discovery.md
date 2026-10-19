# 102 - API Gateway & Service Discovery

## Penjelasan
Setelah memisahkan beberapa service (Auth, Order, Email — materi 101), client (mobile/web) harus tahu alamat setiap service. Bayangkan client harus request ke `auth.service.com`, `order.service.com`, `email.service.com` — rumit dan tidak aman. **API Gateway** adalah satu pintu masuk tunggal yang merutekan request ke service yang tepat. **Service Registry** adalah direktori alamat service.

## Fungsi
- **API Gateway**: Entry point tunggal — client hanya tahu satu URL.
- **Routing request**: Gateway meneruskan request ke service yang tepat berdasarkan path.
- **Authentication di gateway**: Cek token sekali di gateway, service di belakang tidak perlu cek lagi.
- **Service registry**: Daftar alamat service (host:port) — bisa statis (env) atau dinamis (Consul/Eureka).

## Cara Pengimplementasian

### 1. API Gateway Sederhana dengan Routing Manual

**`apps/api-gateway/src/main.ts`**
```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.setGlobalPrefix('api');

  await app.listen(3000);
  console.log('API Gateway running on http://localhost:3000');
}
bootstrap();
```

**`apps/api-gateway/src/app.module.ts`**
```typescript
import { Module } from '@nestjs/common';
import { RouterModule } from '@nestjs/core';
import { AuthModule } from './auth/auth.module';
import { OrdersModule } from './orders/orders.module';
import { ProductsModule } from './products/products.module';

@Module({
  imports: [
    AuthModule,
    OrdersModule,
    ProductsModule,

    // Router — routing request ke module yang tepat
    RouterModule.register([
      {
        path: 'api/auth',
        module: AuthModule,
      },
      {
        path: 'api/orders',
        module: OrdersModule,
      },
      {
        path: 'api/products',
        module: ProductsModule,
      },
    ]),
  ],
})
export class AppModule {}
```

### 2. Authentication Middleware di Gateway

**`apps/api-gateway/src/common/guards/gateway-auth.guard.ts`**
```typescript
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Reflector } from '@nestjs/core';
import { JwtService } from '@nestjs/jwt';
import { Request } from 'express';

@Injectable()
export class GatewayAuthGuard implements CanActivate {
  constructor(
    private jwtService: JwtService,
    private reflector: Reflector,
  ) {}

  canActivate(context: ExecutionContext): boolean {
    const isPublic = this.reflector.getAllAndOverride<boolean>('isPublic', [
      context.getHandler(),
      context.getClass(),
    ]);

    if (isPublic) return true; // Route public (login, register)

    const request = context.switchToHttp().getRequest<Request>();
    const token = this.extractToken(request);

    if (!token) return false;

    try {
      const payload = this.jwtService.verify(token);
      request['user'] = payload; // Service di belakang bisa lihat req.user

      // Forward token ke service downstream via header
      request.headers['x-user-id'] = payload.sub;
      request.headers['x-user-role'] = payload.role;

      return true;
    } catch {
      return false;
    }
  }

  private extractToken(request: Request): string | undefined {
    const [type, token] = request.headers.authorization?.split(' ') ?? [];
    return type === 'Bearer' ? token : undefined;
  }
}
```

### 3. Service Registry — Dynamic Discovery

**`apps/api-gateway/src/service-registry/service-registry.service.ts`**
```typescript
import { Injectable } from '@nestjs/common';

interface ServiceInstance {
  name: string;
  host: string;
  port: number;
  healthy: boolean;
  lastHeartbeat: Date;
}

@Injectable()
export class ServiceRegistryService {
  private services: Map<string, ServiceInstance[]> = new Map();

  register(instance: ServiceInstance) {
    const existing = this.services.get(instance.name) || [];
    // Hapus instance lama dengan host:port sama
    const filtered = existing.filter(s => s.host !== instance.host || s.port !== instance.port);
    filtered.push({ ...instance, healthy: true, lastHeartbeat: new Date() });
    this.services.set(instance.name, filtered);
    console.log(`[Registry] Registered ${instance.name} at ${instance.host}:${instance.port}`);
  }

  unregister(name: string, host: string, port: number) {
    const existing = this.services.get(name) || [];
    this.services.set(name, existing.filter(s => s.host !== host || s.port !== port));
  }

  getService(name: string): ServiceInstance | null {
    const instances = this.services.get(name)?.filter(s => s.healthy) || [];
    if (instances.length === 0) return null;

    // Simple round-robin
    const idx = Math.floor(Math.random() * instances.length);
    return instances[idx];
  }

  heartbeat(name: string, host: string, port: number) {
    const instances = this.services.get(name);
    if (instances) {
      const instance = instances.find(s => s.host === host && s.port === port);
      if (instance) {
        instance.lastHeartbeat = new Date();
        instance.healthy = true;
      }
    }
  }

  // Remove stale services (no heartbeat > 30s)
  cleanup() {
    const now = new Date();
    this.services.forEach((instances, name) => {
      this.services.set(
        name,
        instances.filter(s => (now.getTime() - s.lastHeartbeat.getTime()) < 30000),
      );
    });
  }
}
```

**`apps/api-gateway/src/service-registry/service-registry.controller.ts`**
```typescript
import { Controller, Post, Get, Body, Param } from '@nestjs/common';
import { ServiceRegistryService } from './service-registry.service';

@Controller('internal/registry')
export class ServiceRegistryController {
  constructor(private registry: ServiceRegistryService) {}

  @Post('register')
  register(@Body() body: { name: string; host: string; port: number }) {
    this.registry.register({ ...body, healthy: true, lastHeartbeat: new Date() });
    return { status: 'registered' };
  }

  @Post('heartbeat')
  heartbeat(@Body() body: { name: string; host: string; port: number }) {
    this.registry.heartbeat(body.name, body.host, body.port);
    return { status: 'ok' };
  }

  @Get(':name')
  getService(@Param('name') name: string) {
    return this.registry.getService(name);
  }
}
```

### 4. Proxy ke Internal Service

**`apps/api-gateway/src/orders/orders.controller.ts`**
```typescript
import { Controller, Get, Post, Req } from '@nestjs/common';
import { HttpService } from '@nestjs/axios';
import { ServiceRegistryService } from '../service-registry/service-registry.service';
import { lastValueFrom } from 'rxjs';
import { Request } from 'express';

@Controller()
export class OrdersController {
  constructor(
    private httpService: HttpService,
    private registry: ServiceRegistryService,
  ) {}

  @Get()
  async findAll(@Req() req: Request) {
    const service = this.registry.getService('order-service');
    if (!service) throw new Error('Order service unavailable');

    // Forward request + headers ke internal service
    const { data } = await lastValueFrom(
      this.httpService.get(`http://${service.host}:${service.port}/api/orders`, {
        headers: {
          'x-user-id': req.headers['x-user-id'],
          'x-user-role': req.headers['x-user-role'],
        },
      }),
    );
    return data;
  }
}
```

### 5. Docker Compose — API Gateway + Services
```yaml
version: '3.8'
services:
  api-gateway:
    build: ./apps/api-gateway
    ports: ["80:3000"]
    environment:
      - AUTH_SERVICE_HOST=auth-service
      - ORDER_SERVICE_HOST=order-service

  auth-service:
    build: ./apps/auth-service
    environment:
      - REGISTRY_URL=http://api-gateway:3000/internal/registry

  order-service:
    build: ./apps/order-service
    environment:
      - REGISTRY_URL=http://api-gateway:3000/internal/registry
```

## Analogi
Gedung besar dengan banyak **gedung satelit** (microservices). Client tidak perlu tahu alamat setiap gedung satelit — cukup datang ke **pintu utama** (API Gateway). Resepsionis di pintu utama:
1. Cek KTP (authentication — cek token JWT)
2. Tanya tujuan: "Order?" → arahkan koridor kiri (route ke Order Service)
3. Jika koridor kiri tutup (service down), resepsionis tahu dari **papan direktori** (service registry) dan arahkan alternatif

**Service Registry** adalah papan direktori digital yang mencatat: "Gedung Order ada di Jalan A nomor 10, saat ini buka; Gedung Email ada di Jalan B nomor 5, sedang renovasi (unhealthy)."

## Dipakai untuk apa
- Arsitektur microservices — satu entry point untuk semua client.
- Security — auth dilakukan di satu tempat, service internal tidak perlu expose ke publik.
- Routing — versioning API (`/v1/orders` → service lama, `/v2/orders` → service baru).
- Load balancing — gateway bisa distribusikan request ke multiple instance.

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Gateway jadi monolith kedua | Semua logic bisnis di gateway | Gateway hanya routing + auth umum, bukan business logic |
| Tidak forward headers | Service downstream kehilangan context user | Forward `x-user-id`, `x-user-role` |
| Hardcode alamat service | Pindah server = update semua config | Gunakan service registry atau environment variable |
| Single point of failure | Gateway down = semua aplikasi down | Deploy minimal 2 instance gateway + load balancer |
| Overhead HTTP terlalu besar | Latensi bertambah untuk setiap request | Pertimbangkan gRPC untuk komunikasi internal |

## Soal Latihan

**Soal 1:** Implementasikan API Gateway yang merutekan `/api/users` ke `UserService` dan `/api/orders` ke `OrderService`.

**Jawaban 1:**
```typescript
// app.module.ts
@Module({
  imports: [
    UsersModule,
    OrdersModule,
    RouterModule.register([
      { path: 'api/users', module: UsersModule },
      { path: 'api/orders', module: OrdersModule },
    ]),
  ],
})
export class AppModule {}
```
Atau jika service terpisah (HTTP), buat proxy controller yang forward request.

**Soal 2:** Bagaimana API Gateway bisa mengetahui apakah suatu service sedang sehat atau tidak?

**Jawaban 2:** 
1. **Health check endpoint**: Setiap service punya `/health`. Gateway melakukan ping periodik.
2. **Heartbeat**: Service mengirim POST `/internal/registry/heartbeat` setiap 10 detik. Jika tidak ada heartbeat >30 detik, service dianggap unhealthy.
3. **Circuit breaker**: Gateway menggunakan circuit breaker pattern — jika request ke service gagal 3 kali berturut-turut, gateway berhenti mengirim request ke service tersebut selama beberapa waktu.
