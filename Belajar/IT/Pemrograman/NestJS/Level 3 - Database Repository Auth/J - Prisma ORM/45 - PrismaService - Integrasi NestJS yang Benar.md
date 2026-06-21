# 45 - PrismaService - Integrasi NestJS yang Benar

## Penjelasan

Setelah kita memiliki schema Prisma dan migration (Pertemuan 43 & 44), langkah selanjutnya adalah mengintegrasikan Prisma Client ke dalam ekosistem NestJS dengan benar. NestJS memiliki siklus hidup module (onModuleInit, onModuleDestroy) yang harus kita manfaatkan agar koneksi database dikelola dengan baik — dibuka saat aplikasi start dan ditutup saat aplikasi shutdown.

Di pertemuan sebelumnya kita sudah punya **blueprint dan fondasi gedung**. Sekarang kita **memasang panel listrik utama (MCB)** dan **sistem grounding** — memastikan instalasi database terintegrasi dengan aman ke seluruh gedung.

## Fungsi

- **PrismaService**: Service wrapper untuk PrismaClient dengan manajemen koneksi yang tepat
- **OnModuleInit ($connect)**: Memastikan koneksi database terbangun saat module di-load
- **enableShutdownHooks**: Menangani shutdown NestJS agar Prisma Client juga ditutup dengan bersih
- **PrismaModule global**: Agar PrismaService bisa di-inject di module mana pun tanpa import ulang

## Cara Pengimplementasian

### 1. PrismaService

```typescript
// src/prisma/prisma.service.ts
import { Injectable, OnModuleInit, OnModuleDestroy } from '@nestjs/common';
import { PrismaClient } from '@prisma/client';

@Injectable()
export class PrismaService extends PrismaClient implements OnModuleInit, OnModuleDestroy {
  constructor() {
    super({
      log: ['query', 'info', 'warn', 'error'], // opsional: logging query
    });
  }

  async onModuleInit() {
    await this.$connect();
  }

  async onModuleDestroy() {
    await this.$disconnect();
  }
}
```

### 2. enableShutdownHooks (NestJS < v9)

Untuk NestJS versi lama, perlu tambahan shutdown hook:

```typescript
// src/prisma/prisma.service.ts
import { Injectable, OnModuleInit, INestApplication } from '@nestjs/common';
import { PrismaClient } from '@prisma/client';

@Injectable()
export class PrismaService extends PrismaClient implements OnModuleInit {
  constructor() {
    super({
      log: ['query'],
    });
  }

  async onModuleInit() {
    await this.$connect();
  }

  async enableShutdownHooks(app: INestApplication) {
    process.on('beforeExit', async () => {
      await app.close();
    });
  }
}
```

### 3. PrismaModule (Global)

```typescript
// src/prisma/prisma.module.ts
import { Global, Module } from '@nestjs/common';
import { PrismaService } from './prisma.service';

@Global()
@Module({
  providers: [PrismaService],
  exports: [PrismaService],
})
export class PrismaModule {}
```

### 4. Import di AppModule

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { PrismaModule } from './prisma/prisma.module';

@Module({
  imports: [PrismaModule],
})
export class AppModule {}
```

### 5. Penggunaan di Service

```typescript
// src/users/users.service.ts
import { Injectable } from '@nestjs/common';
import { PrismaService } from '../prisma/prisma.service';

@Injectable()
export class UsersService {
  constructor(private prisma: PrismaService) {}

  async findAll() {
    return this.prisma.user.findMany();
  }

  async findById(id: number) {
    return this.prisma.user.findUnique({ where: { id } });
  }
}
```

## Analogi

**Membangun Gedung Bertingkat**

- **PrismaService extends PrismaClient** = **MCB utama** yang menghubungkan seluruh aliran listrik gedung ke PLN
- **OnModuleInit ($connect)** = **menekan tombol ON** panel listrik saat gedung pertama kali dioperasikan
- **OnModuleDestroy ($disconnect)** = **mematikan panel listrik** saat gedung tutup/maintanance
- **enableShutdownHooks** = **sistem grounding** yang amankan gedung saat ada masalah listrik
- **PrismaModule Global** = **instalasi listrik dasar yang ada di setiap lantai** tanpa harus wiring ulang tiap lantai
- **Inject PrismaService di service** = **colokan listrik** yang siap pakai di setiap ruangan

## Dipakai untuk Apa

- Semua service yang perlu akses database
- Memastikan satu instance Prisma Client untuk seluruh aplikasi (singleton)
- Mencegah memory leak dari koneksi database yang tidak ditutup
- Logging query database untuk debugging

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Membuat instance `new PrismaClient()` di setiap service | Gunakan PrismaService sebagai single instance via DI |
| Lupa `await app.close()` atau enableShutdownHooks | Koneksi database menggantung saat restart |
| Tidak pakai `@Global()` sehingga harus import PrismaModule di mana-mana | Decorator `@Global()` pada PrismaModule |
| Log query terlalu banyak di production | Set log level sesuai environment: `log: env === 'production' ? ['error'] : ['query', 'info', 'warn', 'error']` |

## Soal Latihan

1. Buat `PrismaService` yang extends `PrismaClient`
2. Implementasikan `OnModuleInit` untuk $connect
3. Implementasikan `OnModuleDestroy` untuk $disconnect
4. Buat `PrismaModule` dengan decorator `@Global()`
5. Export PrismaService dari module
6. Import PrismaModule di AppModule dan gunakan di UsersService

### Jawaban

**prisma.service.ts:**
```typescript
import { Injectable, OnModuleInit, OnModuleDestroy } from '@nestjs/common';
import { PrismaClient } from '@prisma/client';

@Injectable()
export class PrismaService extends PrismaClient implements OnModuleInit, OnModuleDestroy {
  async onModuleInit() {
    await this.$connect();
    console.log('Database connected');
  }

  async onModuleDestroy() {
    await this.$disconnect();
    console.log('Database disconnected');
  }
}
```

**prisma.module.ts:**
```typescript
import { Global, Module } from '@nestjs/common';
import { PrismaService } from './prisma.service';

@Global()
@Module({
  providers: [PrismaService],
  exports: [PrismaService],
})
export class PrismaModule {}
```

**app.module.ts:**
```typescript
import { Module } from '@nestjs/common';
import { PrismaModule } from './prisma/prisma.module';
import { UsersModule } from './users/users.module';

@Module({
  imports: [PrismaModule, UsersModule],
})
export class AppModule {}
```

**users.service.ts:**
```typescript
import { Injectable } from '@nestjs/common';
import { PrismaService } from '../prisma/prisma.service';

@Injectable()
export class UsersService {
  constructor(private prisma: PrismaService) {}

  async getUsers() {
    return this.prisma.user.findMany();
  }
}
```
