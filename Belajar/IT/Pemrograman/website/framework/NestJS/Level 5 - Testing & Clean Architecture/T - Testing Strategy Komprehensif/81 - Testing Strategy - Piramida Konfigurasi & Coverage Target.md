# 81 - Testing Strategy - Piramida Konfigurasi & Coverage Target

## Penjelasan

Setelah menyelesaikan module-module sebelumnya (dari dasar NestJS hingga deployment), kini saatnya memastikan semua yang telah dibangun benar-benar **andal**. Testing strategy adalah fondasi yang menentukan seberapa percaya diri kita terhadap kode yang berjalan di production. Di level ini, kita tidak hanya menulis test, tetapi **merancang strategi** — kapan pakai unit test, kapan integration test, dan kapan e2e test.

Piramida testing (Mike Cohn) membagi test menjadi 3 lapis:

- **Unit Test** — paling bawah, paling banyak, cepat, mengisolasi fungsi/class kecil
- **Integration Test** — tengah, menguji interaksi antar modul (database, API)
- **E2E Test** — paling atas, sedikit, lambat, menguji skenario user lengkap

## Fungsi

- Menentukan **coverage target** yang realistis (minimal 80% line coverage)
- Mengkonfigurasi **Jest** dengan threshold agar build gagal jika coverage turun
- Menyediakan **infrastruktur testing** via Testcontainers (PostgreSQL + Redis asli, bukan mock)
- Membuat **factory functions** dengan faker-js untuk data test yang realistis

## Cara Pengimplementasian

### 1. Jest Config dengan Coverage Threshold

```typescript
// jest.config.ts
import type { Config } from 'jest';

const config: Config = {
  moduleFileExtensions: ['js', 'json', 'ts'],
  rootDir: '.',
  testRegex: '.*\\.spec\\.ts$',
  transform: {
    '^.+\\.(t|j)s$': 'ts-jest',
  },
  collectCoverageFrom: [
    'src/**/*.(t|j)s',
    '!src/main.ts',
    '!src/**/*.module.ts',
    '!src/**/*.interface.ts',
    '!src/**/*.dto.ts',
  ],
  coverageDirectory: './coverage',
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
    'src/domain/**/*.ts': {
      branches: 90,
      functions: 90,
      lines: 90,
      statements: 90,
    },
  },
  testEnvironment: 'node',
};

export default config;
```

### 2. Setup Testcontainers untuk PostgreSQL + Redis

```typescript
// test/setup-testcontainers.ts
import {
  PostgreSqlContainer,
  StartedPostgreSqlContainer,
} from '@testcontainers/postgresql';
import { RedisContainer, StartedRedisContainer } from '@testcontainers/redis';
import { Client } from 'pg';
import Redis from 'ioredis';

let pgContainer: StartedPostgreSqlContainer;
let redisContainer: StartedRedisContainer;
let pgClient: Client;
let redisClient: Redis;

export async function startContainers() {
  pgContainer = await new PostgreSqlContainer('postgres:16-alpine')
    .withDatabase('test_db')
    .withUsername('test')
    .withPassword('test')
    .start();

  redisContainer = await new RedisContainer('redis:7-alpine').start();

  pgClient = new Client({
    host: pgContainer.getHost(),
    port: pgContainer.getMappedPort(5432),
    database: 'test_db',
    user: 'test',
    password: 'test',
  });
  await pgClient.connect();

  redisClient = new Redis({
    host: redisContainer.getHost(),
    port: redisContainer.getMappedPort(6379),
  });

  return { pgClient, redisClient, pgContainer, redisContainer };
}

export async function stopContainers() {
  await pgClient?.end();
  await redisClient?.quit();
  await pgContainer?.stop();
  await redisContainer?.stop();
}
```

### 3. Factory Functions dengan faker-js

```typescript
// test/factories/user.factory.ts
import { faker } from '@faker-js/faker';
import { Role } from '@prisma/client';

export interface CreateUserInput {
  email?: string;
  password?: string;
  name?: string;
  role?: Role;
  isVerified?: boolean;
}

export function makeUser(overrides: CreateUserInput = {}) {
  return {
    email: faker.internet.email(),
    password: faker.internet.password({ length: 12 }),
    name: faker.person.fullName(),
    role: Role.CUSTOMER,
    isVerified: true,
    ...overrides,
  };
}

// test/factories/product.factory.ts
import { faker } from '@faker-js/faker';
import { ProductStatus } from '@prisma/client';

export function makeProduct(overrides: Partial<any> = {}) {
  return {
    name: faker.commerce.productName(),
    description: faker.commerce.productDescription(),
    price: parseFloat(faker.commerce.price({ min: 1000, max: 5000000 })),
    stock: faker.number.int({ min: 0, max: 1000 }),
    status: ProductStatus.ACTIVE,
    categoryId: faker.string.uuid(),
    ...overrides,
  };
}

// test/factories/order.factory.ts
import { faker } from '@faker-js/faker';
import { OrderStatus } from '@prisma/client';

export function makeOrder(overrides: Partial<any> = {}) {
  return {
    userId: faker.string.uuid(),
    status: OrderStatus.PENDING,
    totalAmount: parseFloat(faker.commerce.price({ min: 10000, max: 10000000 })),
    shippingAddress: faker.location.streetAddress(),
    ...overrides,
  };
}

// test/factories/order-item.factory.ts
export function makeOrderItem(overrides: Partial<any> = {}) {
  return {
    orderId: faker.string.uuid(),
    productId: faker.string.uuid(),
    quantity: faker.number.int({ min: 1, max: 10 }),
    price: parseFloat(faker.commerce.price({ min: 1000, max: 500000 })),
    ...overrides,
  };
}

// test/factories/payment.factory.ts
export function makePayment(overrides: Partial<any> = {}) {
  return {
    orderId: faker.string.uuid(),
    method: faker.helpers.arrayElement(['CREDIT_CARD', 'BANK_TRANSFER', 'E_WALLET']),
    amount: parseFloat(faker.commerce.price({ min: 10000, max: 10000000 })),
    status: 'SUCCESS',
    ...overrides,
  };
}
```

### 4. Penggunaan Testcontainers di Jest Global Setup

```typescript
// test/jest-global-setup.ts
import { startContainers } from './setup-testcontainers';

export default async function globalSetup() {
  const { pgClient, redisClient } = await startContainers();

  // Jalankan migrasi Prisma
  const { execSync } = await import('child_process');
  execSync('npx prisma migrate deploy', {
    env: {
      ...process.env,
      DATABASE_URL: `postgresql://test:test@${pgClient['host']}:${pgClient['port']}/test_db`,
      REDIS_URL: `redis://${redisClient['options']['host']}:${redisClient['options']['port']}`,
    },
  });

  // Simpan koneksi ke global agar bisa diakses test
  (global as any).__PG_CLIENT__ = pgClient;
  (global as any).__REDIS_CLIENT__ = redisClient;
}
```

## Analogi — Gedung Bertingkat

Membangun gedung tanpa pengujian struktur ibarat membangun 20 lantai tanpa pernah mengecek pondasi.

| Lapisan Piramida | Analogi Gedung |
|-----------------|----------------|
| **Unit Test** | Tes kekuatan setiap bata, setiap besi, setiap baut secara terpisah |
| **Integration Test** | Tes apakah dinding menyatu dengan kolom, apakah kabel listrik terhubung ke stop kontak |
| **E2E Test** | Simulasi orang masuk lift, naik ke lantai 10, buka pintu, nyalakan lampu |
| **Testcontainers** | Membangun gedung mini di halaman belakang yang persis sama dengan aslinya untuk latihan |
| **Factory faker-js** | Pekerja magang yang bisa membuat ribuan batu bata identik dengan cepat untuk tes |
| **Coverage Threshold** | Aturan bahwa 80% baut harus sudah diperiksa sebelum gedung diizinkan dipakai |

## Dipakai untuk Apa

- **CI Pipeline** — setiap push, test dijalankan otomatis; jika coverage turun, build gagal
- **Development** — developer bisa menjalankan test dengan database asli tanpa instalasi manual
- **Code Review** — reviewer bisa lihat coverage report untuk menilai kualitas test
- **Refactoring** — jaring pengaman saat mengubah kode; test akan menangkap regression

## Kesalahan Umum yang Sering Terjadi

1. **Mock berlebihan** — semua service di-mock sehingga test hanya menguji dirinya sendiri, bukan interaksi nyata
2. **Testcontainers tidak dimatikan** — container tetap berjalan dan menghabiskan resource; selalu panggil `stopContainers()` di `afterAll` atau `globalTeardown`
3. **Factory terlalu rigid** — factory function tidak menerima override sehingga test jadi sulit mengatur skenario spesifik
4. **Coverage threshold terlalu rendah** — threshold 50% membuat tim lengah dan banyak kode tak teruji
5. **Lupa exclude file boilerplate** — file seperti `main.ts` dan `*.module.ts` ikut dihitung coverage, menambah beban tanpa nilai

## Soal Latihan

### Soal 1: Setup Testcontainers

Buat file `setup-testcontainers.ts` yang:
- Menjalankan PostgreSQL container dengan image `postgres:16-alpine`
- Menjalankan Redis container dengan image `redis:7-alpine`
- Export fungsi `getPrismaClient()` dan `getRedisClient()` yang mengembalikan koneksi
- Container auto-terminate setelah 30 detik jika tidak dipakai

### Jawaban 1:

```typescript
import {
  PostgreSqlContainer,
  StartedPostgreSqlContainer,
} from '@testcontainers/postgresql';
import { RedisContainer, StartedRedisContainer } from '@testcontainers/redis';
import { PrismaClient } from '@prisma/client';
import Redis from 'ioredis';

let pgContainer: StartedPostgreSqlContainer;
let redisContainer: StartedRedisContainer;
let prisma: PrismaClient;
let redis: Redis;

async function setupTestcontainers() {
  pgContainer = await new PostgreSqlContainer('postgres:16-alpine')
    .withStartupTimeout(30_000)
    .start();

  redisContainer = await new RedisContainer('redis:7-alpine')
    .withStartupTimeout(30_000)
    .start();

  prisma = new PrismaClient({
    datasources: {
      db: {
        url: `postgresql://${pgContainer.getUsername()}:${pgContainer.getPassword()}@${pgContainer.getHost()}:${pgContainer.getMappedPort(5432)}/${pgContainer.getDatabase()}`,
      },
    },
  });

  redis = new Redis({
    host: redisContainer.getHost(),
    port: redisContainer.getMappedPort(6379),
  });

  return { prisma, redis };
}

export function getPrismaClient() {
  return prisma;
}

export function getRedisClient() {
  return redis;
}

export async function teardownTestcontainers() {
  await prisma?.$disconnect();
  await redis?.quit();
  await pgContainer?.stop();
  await redisContainer?.stop();
}

export { setupTestcontainers };
```

### Soal 2: Factory Function

Buat factory function `makeCategory()` yang menghasilkan kategori produk dengan faker, lengkap dengan override parsial.

### Jawaban 2:

```typescript
import { faker } from '@faker-js/faker';

export function makeCategory(overrides: Partial<{
  id: string;
  name: string;
  slug: string;
  description: string;
  isActive: boolean;
}> = {}) {
  const name = faker.commerce.department();
  return {
    id: faker.string.uuid(),
    name,
    slug: faker.helpers.slugify(name).toLowerCase(),
    description: faker.lorem.sentence(),
    isActive: true,
    ...overrides,
  };
}
```
