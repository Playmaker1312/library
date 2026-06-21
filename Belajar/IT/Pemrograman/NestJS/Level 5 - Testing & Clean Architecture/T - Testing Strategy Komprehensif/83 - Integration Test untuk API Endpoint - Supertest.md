# 83 - Integration Test untuk API Endpoint - Supertest

## Penjelasan

Setelah unit test mengamankan logic bisnis di level atomik (bab 82), kini kita naik satu tingkat ke **integration test**. Jika unit test menggunakan mock untuk database, integration test menggunakan **database asli** (via Testcontainers dari bab 81). Tujuannya: memastikan bahwa **semua lapisan bekerja bersama** — controller, service, guard, interceptor, pipe, dan Prisma — tanpa mock.

Kita akan menggunakan **Supertest** (`supertest`) untuk mengirim HTTP request ke aplikasi NestJS yang dijalankan dalam mode testing (`TestingModule`), ditambah **Testcontainers** untuk PostgreSQL dan Redis yang nyata.

## Fungsi

- Memverifikasi endpoint API berfungsi end-to-end dengan database asli
- Menguji **auth flow** (register → login → akses protected route)
- Menguji **CRUD produk** dengan data real di database
- Menyediakan **seed + cleanup** agar test dapat dijalankan berulang kali (idempotent)

## Cara Pengimplementasian

### 1. Setup Integration Test dengan Supertest + Testcontainers

```typescript
// test/integration/setup.ts
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication, ValidationPipe } from '@nestjs/common';
import * as request from 'supertest';
import { AppModule } from '../../src/app.module';
import { PrismaService } from '../../src/prisma/prisma.service';
import {
  setupTestcontainers,
  teardownTestcontainers,
  getPrismaClient,
} from '../setup-testcontainers';

let app: INestApplication;
let prisma: PrismaService;
let httpServer: any;

beforeAll(async () => {
  await setupTestcontainers();
  prisma = getPrismaClient() as unknown as PrismaService;

  const moduleFixture: TestingModule = await Test.createTestingModule({
    imports: [AppModule],
  })
    .overrideProvider(PrismaService)
    .useValue(prisma)
    .compile();

  app = moduleFixture.createNestApplication();
  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true,
      forbidNonWhitelisted: true,
      transform: true,
    }),
  );
  await app.init();

  httpServer = app.getHttpServer();
}, 60_000);

afterAll(async () => {
  await app?.close();
  await teardownTestcontainers();
});

export { app, prisma, httpServer, request };
```

### 2. Helper untuk Seed + Cleanup

```typescript
// test/integration/helpers/db.helper.ts
import { makeUser } from '../../factories/user.factory';
import { makeProduct } from '../../factories/product.factory';
import * as bcrypt from 'bcrypt';

export async function seedUser(prisma: any, overrides = {}) {
  const userData = makeUser(overrides);
  const hashedPassword = await bcrypt.hash(userData.password, 10);

  return prisma.user.create({
    data: {
      ...userData,
      password: hashedPassword,
    },
  });
}

export async function seedProduct(prisma: any, overrides = {}) {
  const productData = makeProduct(overrides);
  return prisma.product.create({ data: productData });
}

export async function cleanupDatabase(prisma: any) {
  const tablenames = await prisma.$queryRaw<
    Array<{ tablename: string }>
  >`SELECT tablename FROM pg_tables WHERE schemaname='public'`;

  for (const { tablename } of tablenames) {
    if (tablename !== '_prisma_migrations') {
      await prisma.$executeRawUnsafe(
        `TRUNCATE TABLE "${tablename}" CASCADE;`,
      );
    }
  }
}
```

### 3. Integration Test Auth Flow

```typescript
// test/integration/auth/auth-flow.spec.ts
import { app, prisma, httpServer, request } from '../setup';
import { cleanupDatabase, seedUser } from '../helpers/db.helper';

beforeEach(async () => {
  await cleanupDatabase(prisma);
});

describe('Auth Flow — Integration Test', () => {
  describe('POST /api/auth/register', () => {
    it('should register a new user and return tokens', async () => {
      const res = await request(httpServer)
        .post('/api/auth/register')
        .send({
          email: 'newuser@test.com',
          password: 'StrongPass123!',
          name: 'New User',
        })
        .expect(201);

      expect(res.body).toHaveProperty('accessToken');
      expect(res.body).toHaveProperty('refreshToken');
      expect(res.body.user.email).toBe('newuser@test.com');
    });

    it('should reject duplicate email', async () => {
      await seedUser(prisma, { email: 'duplicate@test.com' });

      await request(httpServer)
        .post('/api/auth/register')
        .send({
          email: 'duplicate@test.com',
          password: 'StrongPass123!',
          name: 'Duplicate',
        })
        .expect(409);
    });

    it('should reject weak password', async () => {
      await request(httpServer)
        .post('/api/auth/register')
        .send({
          email: 'weak@test.com',
          password: '123',
          name: 'Weak',
        })
        .expect(400);
    });
  });

  describe('POST /api/auth/login', () => {
    it('should login with valid credentials', async () => {
      await seedUser(prisma, {
        email: 'login@test.com',
        password: 'StrongPass123!',
      });

      const res = await request(httpServer)
        .post('/api/auth/login')
        .send({ email: 'login@test.com', password: 'StrongPass123!' })
        .expect(200);

      expect(res.body).toHaveProperty('accessToken');
    });

    it('should reject wrong password', async () => {
      await seedUser(prisma, {
        email: 'wrongpass@test.com',
        password: 'StrongPass123!',
      });

      await request(httpServer)
        .post('/api/auth/login')
        .send({ email: 'wrongpass@test.com', password: 'WrongPass!' })
        .expect(401);
    });
  });

  describe('GET /api/auth/me (protected)', () => {
    it('should return current user with valid token', async () => {
      const user = await seedUser(prisma, { email: 'me@test.com' });

      const loginRes = await request(httpServer)
        .post('/api/auth/login')
        .send({ email: 'me@test.com', password: user.password })
        .expect(200);

      const res = await request(httpServer)
        .get('/api/auth/me')
        .set('Authorization', `Bearer ${loginRes.body.accessToken}`)
        .expect(200);

      expect(res.body.email).toBe('me@test.com');
    });

    it('should reject missing token', async () => {
      await request(httpServer)
        .get('/api/auth/me')
        .expect(401);
    });

    it('should reject expired token', async () => {
      await request(httpServer)
        .get('/api/auth/me')
        .set('Authorization', 'Bearer expired.token.here')
        .expect(401);
    });
  });
});
```

### 4. Integration Test Product CRUD

```typescript
// test/integration/product/product-crud.spec.ts
import { app, prisma, httpServer, request } from '../setup';
import { cleanupDatabase, seedUser, seedProduct } from '../helpers/db.helper';

let authToken: string;

beforeEach(async () => {
  await cleanupDatabase(prisma);

  const user = await seedUser(prisma, { email: 'admin@test.com', role: 'ADMIN' });
  const loginRes = await request(httpServer)
    .post('/api/auth/login')
    .send({ email: 'admin@test.com', password: user.password })
    .expect(200);
  authToken = loginRes.body.accessToken;
});

describe('Product CRUD — Integration Test', () => {
  let productId: string;

  it('POST /api/products — should create product', async () => {
    const res = await request(httpServer)
      .post('/api/products')
      .set('Authorization', `Bearer ${authToken}`)
      .send({
        name: 'Test Product',
        description: 'A product for testing',
        price: 50000,
        stock: 100,
        categoryId: 'cat-1',
      })
      .expect(201);

    expect(res.body.name).toBe('Test Product');
    expect(res.body.price).toBe(50000);
    productId = res.body.id;
  });

  it('GET /api/products — should list products with pagination', async () => {
    await seedProduct(prisma, { name: 'Product A' });
    await seedProduct(prisma, { name: 'Product B' });

    const res = await request(httpServer)
      .get('/api/products')
      .query({ page: 1, limit: 10 })
      .expect(200);

    expect(res.body.data).toHaveLength(2);
    expect(res.body.meta.total).toBe(2);
  });

  it('GET /api/products/:id — should get product by id', async () => {
    const product = await seedProduct(prisma, { name: 'Find Me' });

    const res = await request(httpServer)
      .get(`/api/products/${product.id}`)
      .expect(200);

    expect(res.body.name).toBe('Find Me');
  });

  it('GET /api/products/:id — should return 404 for non-existent product', async () => {
    await request(httpServer)
      .get('/api/products/non-existent-id')
      .expect(404);
  });

  it('PATCH /api/products/:id — should update product', async () => {
    const product = await seedProduct(prisma, { name: 'Old Name' });

    await request(httpServer)
      .patch(`/api/products/${product.id}`)
      .set('Authorization', `Bearer ${authToken}`)
      .send({ name: 'New Name' })
      .expect(200);

    const updated = await request(httpServer)
      .get(`/api/products/${product.id}`)
      .expect(200);

    expect(updated.body.name).toBe('New Name');
  });

  it('DELETE /api/products/:id — should delete product', async () => {
    const product = await seedProduct(prisma);

    await request(httpServer)
      .delete(`/api/products/${product.id}`)
      .set('Authorization', `Bearer ${authToken}`)
      .expect(204);

    await request(httpServer)
      .get(`/api/products/${product.id}`)
      .expect(404);
  });

  it('should reject unauthorized CRUD operations', async () => {
    await request(httpServer)
      .post('/api/products')
      .send({ name: 'No Auth' })
      .expect(401);

    await request(httpServer)
      .patch('/api/products/some-id')
      .send({ name: 'No Auth' })
      .expect(401);

    await request(httpServer)
      .delete('/api/products/some-id')
      .expect(401);
  });
});
```

## Analogi — Gedung Bertingkat

| Konsep | Analogi Gedung |
|--------|----------------|
| **Integration Test** | Menguji seluruh lantai: apakah listrik dari panel (controller) sampai ke stop kontal (service) lalu ke lampu (database) benar-benar menyala |
| **Supertest** | Tukang tes yang menekan semua saklar dan melihat apakah lampu menyala |
| **Testcontainers** | Membangun gedung kembaran di halaman belakang untuk dites tanpa merusak aslinya |
| **Seed Data** | Menata perabot mini di gedung kembaran agar tes realistis |
| **Cleanup Database** | Merobohkan gedung kembaran setelah tes agar besok bisa dibangun lagi dari awal |

## Dipakai untuk Apa

- **Validasi endpoint** — memastikan request → response sesuai spec (status code, body, headers)
- **Auth flow** — menguji register, login, refresh token, logout, protected routes
- **CRUD operations** — memastikan create, read, update, delete bekerja dengan database
- **Error handling** — memastikan error response konsisten (format, status code)
- **Regression** — ketika ada perubahan di satu service, integration test akan menangkap jika ada endpoint yang rusak

## Kesalahan Umum yang Sering Terjadi

1. **Tidak cleanup database** — test saling bergantung; test B gagal karena data dari test A masih ada
2. **Timeout terlalu pendek** — Testcontainers butuh waktu pull image; set `beforeAll` timeout ke 60+ detik
3. **Menggunakan database production** — test menimpa data asli; selalu gunakan Testcontainers atau database terisolasi
4. **Tidak override provider** — app module tetap menggunakan koneksi production; override `PrismaService` dengan instance Testcontainers
5. **Test tidak idempotent** — seed data menggunakan ID statis; test kedua kali gagal karena ID duplikat; selalu gunakan factory dengan faker

## Soal Latihan

### Soal 1: Integration Test untuk Auth Flow

Tulis integration test untuk skenario berikut:
- Register user baru dengan data valid
- Kirim request ke endpoint yang membutuhkan autentikasi TANPA token → harapannya 401
- Login dengan user yang sudah register
- Gunakan token dari login untuk mengakses `/api/auth/me`

### Jawaban 1:

```typescript
import { app, prisma, httpServer, request } from '../setup';
import { cleanupDatabase } from '../helpers/db.helper';

describe('Auth Flow Integration Test', () => {
  beforeEach(async () => {
    await cleanupDatabase(prisma);
  });

  it('complete auth flow: register → login → access protected route', async () => {
    // 1. Register
    const registerRes = await request(httpServer)
      .post('/api/auth/register')
      .send({
        email: 'flow@test.com',
        password: 'StrongPass123!',
        name: 'Flow User',
      })
      .expect(201);

    expect(registerRes.body).toHaveProperty('accessToken');

    // 2. Akses protected route tanpa token
    await request(httpServer)
      .get('/api/auth/me')
      .expect(401);

    // 3. Login
    const loginRes = await request(httpServer)
      .post('/api/auth/login')
      .send({ email: 'flow@test.com', password: 'StrongPass123!' })
      .expect(200);

    expect(loginRes.body).toHaveProperty('accessToken');

    // 4. Akses protected route dengan token
    const meRes = await request(httpServer)
      .get('/api/auth/me')
      .set('Authorization', `Bearer ${loginRes.body.accessToken}`)
      .expect(200);

    expect(meRes.body.email).toBe('flow@test.com');
    expect(meRes.body.name).toBe('Flow User');
  });
});
```

### Soal 2: Integration Test untuk Product CRUD

Buat integration test untuk skenario:
- Admin login → membuat produk → mendapatkan produk → mengupdate produk → menghapus produk
- Customer (bukan admin) mencoba membuat produk → 403 Forbidden

### Jawaban 2:

```typescript
import { app, prisma, httpServer, request } from '../setup';
import { cleanupDatabase, seedUser } from '../helpers/db.helper';

describe('Product CRUD — Role Based Access', () => {
  beforeEach(async () => {
    await cleanupDatabase(prisma);
  });

  it('admin can perform full CRUD', async () => {
    const admin = await seedUser(prisma, {
      email: 'admin@test.com',
      role: 'ADMIN',
    });

    const loginRes = await request(httpServer)
      .post('/api/auth/login')
      .send({ email: 'admin@test.com', password: admin.password })
      .expect(200);

    const token = loginRes.body.accessToken;

    // Create
    const createRes = await request(httpServer)
      .post('/api/products')
      .set('Authorization', `Bearer ${token}`)
      .send({ name: 'New Product', price: 25000, categoryId: 'cat-1' })
      .expect(201);

    const productId = createRes.body.id;

    // Read
    await request(httpServer)
      .get(`/api/products/${productId}`)
      .expect(200);

    // Update
    await request(httpServer)
      .patch(`/api/products/${productId}`)
      .set('Authorization', `Bearer ${token}`)
      .send({ price: 30000 })
      .expect(200);

    // Delete
    await request(httpServer)
      .delete(`/api/products/${productId}`)
      .set('Authorization', `Bearer ${token}`)
      .expect(204);
  });

  it('customer cannot create product', async () => {
    const customer = await seedUser(prisma, {
      email: 'cust@test.com',
      role: 'CUSTOMER',
    });

    const loginRes = await request(httpServer)
      .post('/api/auth/login')
      .send({ email: 'cust@test.com', password: customer.password })
      .expect(200);

    const token = loginRes.body.accessToken;

    await request(httpServer)
      .post('/api/products')
      .set('Authorization', `Bearer ${token}`)
      .send({ name: 'Unauthorized', price: 1000, categoryId: 'cat-1' })
      .expect(403);
  });
});
```
