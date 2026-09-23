# 84 - E2E Testing - Skenario User Journey Lengkap

## Penjelasan

Setelah integration test memvalidasi setiap endpoint secara terpisah (bab 83), kini tiba saatnya menguji **keseluruhan sistem dari sudut pandang user**. E2E (End-to-End) testing mensimulasikan perjalanan user yang lengkap — dari registrasi, verifikasi email, login, melihat produk, checkout, hingga menerima konfirmasi order.

Di bab ini, kita juga akan menguji **WebSocket** — fitur real-time seperti notifikasi order status — menggunakan `socket.io-client` yang benar-benar terhubung ke server.

E2E test berbeda dengan integration test:
- **Integration test** menguji satu endpoint dalam isolasi
- **E2E test** menguji sekumpulan aksi yang dilakukan user dalam urutan tertentu

## Fungsi

- Mensimulasikan **user journey lengkap**: register → verify → login → browse → add to cart → checkout → pay → receive confirmation
- Menguji **WebSocket flow**: koneksi, event, dan notifikasi real-time
- Memastikan **semua modul bekerja bersama** (auth, product, order, payment, notification)
- Mendeteksi **regression** yang tidak tertangkap oleh unit/integration test (misal: token expired di tengah checkout)

## Cara Pengimplementasian

### 1. Setup E2E Test dengan User Journey

```typescript
// test/e2e/setup.ts
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication, ValidationPipe } from '@nestjs/common';
import * as request from 'supertest';
import { io, Socket } from 'socket.io-client';
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
    new ValidationPipe({ whitelist: true, forbidNonWhitelisted: true, transform: true }),
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

### 2. Helper untuk WebSocket Client

```typescript
// test/e2e/helpers/ws.helper.ts
import { io, Socket } from 'socket.io-client';

export function createSocketClient(
  httpServer: any,
  token?: string,
): Promise<Socket> {
  return new Promise((resolve, reject) => {
    const client = io(`http://localhost:${httpServer.address().port}`, {
      auth: { token },
      transports: ['websocket'],
      forceNew: true,
    });

    client.on('connect', () => resolve(client));
    client.on('connect_error', (err) => reject(err));

    setTimeout(() => reject(new Error('WebSocket connection timeout')), 5000);
  });
}

export function waitForEvent(
  socket: Socket,
  eventName: string,
  timeout = 10000,
): Promise<any> {
  return new Promise((resolve, reject) => {
    const timer = setTimeout(() => {
      reject(new Error(`Timeout waiting for event: ${eventName}`));
    }, timeout);

    socket.once(eventName, (data: any) => {
      clearTimeout(timer);
      resolve(data);
    });
  });
}
```

### 3. E2E Test — User Journey Lengkap

```typescript
// test/e2e/checkout-flow.spec.ts
import { app, prisma, httpServer, request } from './setup';
import { cleanupDatabase, seedUser, seedProduct } from '../integration/helpers/db.helper';
import { createSocketClient, waitForEvent } from './helpers/ws.helper';
import { Socket } from 'socket.io-client';

beforeEach(async () => {
  await cleanupDatabase(prisma);
});

describe('Complete User Journey — Register to Checkout', () => {
  let userToken: string;
  let userId: string;
  let productId: string;
  let orderId: string;
  let socket: Socket;

  it('Step 1: Register user', async () => {
    const res = await request(httpServer)
      .post('/api/auth/register')
      .send({
        email: 'buyer@test.com',
        password: 'BuyerPass123!',
        name: 'John Buyer',
      })
      .expect(201);

    userToken = res.body.accessToken;
    userId = res.body.user.id;
    expect(res.body.user.email).toBe('buyer@test.com');
  });

  it('Step 2: Verify email', async () => {
    // Dalam implementasi nyata, token verifikasi dikirim via email.
    // Di test, kita ambil langsung dari database.
    const user = await prisma.user.findUnique({
      where: { email: 'buyer@test.com' },
    });

    const verificationToken = user.verificationToken;

    const res = await request(httpServer)
      .post('/api/auth/verify-email')
      .send({ token: verificationToken })
      .expect(200);

    expect(res.body.message).toContain('verified');
  });

  it('Step 3: Login after verification', async () => {
    const res = await request(httpServer)
      .post('/api/auth/login')
      .send({ email: 'buyer@test.com', password: 'BuyerPass123!' })
      .expect(200);

    userToken = res.body.accessToken;
  });

  it('Step 4: Browse products', async () => {
    // Seed beberapa produk
    await seedProduct(prisma, {
      name: 'Gaming Keyboard',
      price: 750000,
      stock: 10,
    });
    await seedProduct(prisma, {
      name: 'Gaming Mouse',
      price: 450000,
      stock: 15,
    });

    const res = await request(httpServer)
      .get('/api/products')
      .query({ page: 1, limit: 10 })
      .expect(200);

    expect(res.body.data).toHaveLength(2);
    productId = res.body.data[0].id;
  });

  it('Step 5: Add to cart and checkout', async () => {
    // Add to cart
    await request(httpServer)
      .post('/api/cart/items')
      .set('Authorization', `Bearer ${userToken}`)
      .send({ productId, quantity: 2 })
      .expect(201);

    // Checkout
    const checkoutRes = await request(httpServer)
      .post('/api/orders/checkout')
      .set('Authorization', `Bearer ${userToken}`)
      .send({
        shippingAddress: 'Jl. Merdeka No. 1, Jakarta',
        paymentMethod: 'BANK_TRANSFER',
      })
      .expect(201);

    orderId = checkoutRes.body.orderId;
    expect(checkoutRes.body.status).toBe('PENDING');
    expect(checkoutRes.body.totalAmount).toBe(1500000); // 2 x 750000
  });

  it('Step 6: Pay the order', async () => {
    const res = await request(httpServer)
      .post('/api/payments/pay')
      .set('Authorization', `Bearer ${userToken}`)
      .send({
        orderId,
        amount: 1500000,
        paymentMethod: 'BANK_TRANSFER',
      })
      .expect(201);

    expect(res.body.status).toBe('SUCCESS');
  });

  it('Step 7: Check order status', async () => {
    const res = await request(httpServer)
      .get(`/api/orders/${orderId}`)
      .set('Authorization', `Bearer ${userToken}`)
      .expect(200);

    expect(res.body.status).toBe('PAID');
    expect(res.body.items).toHaveLength(1);
    expect(res.body.items[0].quantity).toBe(2);
  });

  it('Step 8: Receive WebSocket notification', async () => {
    // Connect WebSocket
    socket = await createSocketClient(httpServer, userToken);

    // Trigger event: admin mengupdate status order menjadi SHIPPED
    const adminUser = await seedUser(prisma, {
      email: 'admin@store.com',
      role: 'ADMIN',
    });

    const adminLoginRes = await request(httpServer)
      .post('/api/auth/login')
      .send({ email: 'admin@store.com', password: adminUser.password })
      .expect(200);

    await request(httpServer)
      .patch(`/api/orders/${orderId}/status`)
      .set('Authorization', `Bearer ${adminLoginRes.body.accessToken}`)
      .send({ status: 'SHIPPED' })
      .expect(200);

    // User harus menerima notifikasi real-time
    const notification = await waitForEvent(socket, 'order.status.updated', 5000);
    expect(notification.orderId).toBe(orderId);
    expect(notification.status).toBe('SHIPPED');

    socket.disconnect();
  });
});
```

### 4. E2E Test untuk WebSocket

```typescript
// test/e2e/websocket-chat.spec.ts
import { httpServer } from './setup';
import { createSocketClient, waitForEvent } from './helpers/ws.helper';
import { Socket } from 'socket.io-client';

describe('WebSocket — Real-time Chat', () => {
  let client1: Socket;
  let client2: Socket;
  let token1: string;
  let token2: string;

  beforeAll(async () => {
    // Register & login 2 users
    const res1 = await request(httpServer)
      .post('/api/auth/register')
      .send({ email: 'user1@test.com', password: 'Pass123!', name: 'User 1' });
    token1 = res1.body.accessToken;

    const res2 = await request(httpServer)
      .post('/api/auth/register')
      .send({ email: 'user2@test.com', password: 'Pass123!', name: 'User 2' });
    token2 = res2.body.accessToken;
  });

  beforeEach(async () => {
    client1 = await createSocketClient(httpServer, token1);
    client2 = await createSocketClient(httpServer, token2);
  });

  afterEach(() => {
    client1?.disconnect();
    client2?.disconnect();
  });

  it('should send and receive private message', async () => {
    const messagePromise = waitForEvent(client1, 'private.message');

    client2.emit('private.message', {
      to: 'user1@test.com',
      text: 'Hello from User 2!',
    });

    const message = await messagePromise;
    expect(message.text).toBe('Hello from User 2!');
    expect(message.from).toBe('user2@test.com');
  });

  it('should notify when user is typing', async () => {
    const typingPromise = waitForEvent(client1, 'user.typing');

    client2.emit('typing', { to: 'user1@test.com' });

    const typing = await typingPromise;
    expect(typing.from).toBe('user2@test.com');
  });
});
```

## Analogi — Gedung Bertingkat

| Konsep | Analogi Gedung |
|--------|----------------|
| **User Journey E2E** | Seseorang masuk gedung, naik lift ke lantai 3, buka pintu kantor, duduk di kursi, nyalakan komputer, dan mulai bekerja — semua dalam satu rangkaian |
| **Integration Test** | Hanya menguji lift: naik dari lantai 1 ke 3, lalu turun lagi |
| **Unit Test** | Hanya menguji tombol lift: apakah tombol lantai 3 menyala saat ditekan |
| **WebSocket Test** | Menguji interkom: apakah pesan dari resepsionis terdengar jelas di lantai 10 |
| **Checkout Flow** | Seseorang masuk toko, ambil barang, antre di kasir, bayar, dapat struk, keluar |

## Dipakai untuk Apa

- **Regression testing** sebelum rilis — pastikan semua fitur utama masih berfungsi
- **User acceptance testing (UAT)** — simulasikan scenario user nyata
- **Flow validation** — deteksi bug yang muncul hanya saat beberapa langkah dilakukan berurutan
- **Real-time feature testing** — validasi WebSocket, notification, dan event-driven flows

## Kesalahan Umum yang Sering Terjadi

1. **E2E test terlalu banyak** — E2E test lambat; prioritaskan skenario kritis, jangan coba cover semua baris kode
2. **Data state tidak dikelola** — test gagal karena data dari test sebelumnya masih ada; selalu cleanup di `beforeEach`
3. **Flaky test karena timing** — WebSocket test sering gagal karena race condition; gunakan `waitForEvent` dengan timeout yang cukup
4. **Token statis** — hardcode token yang sudah expired; selalu login di awal test untuk mendapatkan token fresh
5. **Mock di E2E test** — E2E harus menggunakan komponen nyata; jangan mock payment gateway, tapi gunakan sandbox/test mode

## Soal Latihan

### Soal 1: E2E Test Checkout Flow

Buat E2E test untuk skenario berikut:
- Customer mendaftar dan login
- Customer melihat produk
- Customer menambahkan produk ke cart
- Customer checkout
- Customer membayar
- Customer menerima WebSocket notification bahwa order telah dikonfirmasi

### Jawaban 1:

```typescript
import { app, prisma, httpServer, request } from './setup';
import { cleanupDatabase } from '../helpers/db.helper';
import { createSocketClient, waitForEvent } from './helpers/ws.helper';
import { Socket } from 'socket.io-client';
import * as bcrypt from 'bcrypt';
import { makeProduct } from '../../factories/product.factory';

describe('Checkout Flow E2E', () => {
  let token: string;
  let socket: Socket;

  beforeEach(async () => {
    await cleanupDatabase(prisma);

    // Register + login langsung via seed untuk kecepatan
    const hashedPassword = await bcrypt.hash('Pass123!', 10);
    await prisma.user.create({
      data: {
        email: 'customer@test.com',
        password: hashedPassword,
        name: 'Customer',
        role: 'CUSTOMER',
        isVerified: true,
      },
    });

    const loginRes = await request(httpServer)
      .post('/api/auth/login')
      .send({ email: 'customer@test.com', password: 'Pass123!' })
      .expect(200);

    token = loginRes.body.accessToken;
    socket = await createSocketClient(httpServer, token);
  });

  afterEach(() => {
    socket?.disconnect();
  });

  it('should complete checkout flow end-to-end', async () => {
    // Seed product
    const product = await prisma.product.create({
      data: makeProduct({ name: 'Test Item', price: 100000, stock: 5 }),
    });

    // Add to cart
    await request(httpServer)
      .post('/api/cart/items')
      .set('Authorization', `Bearer ${token}`)
      .send({ productId: product.id, quantity: 2 })
      .expect(201);

    // Checkout
    const orderRes = await request(httpServer)
      .post('/api/orders/checkout')
      .set('Authorization', `Bearer ${token}`)
      .send({
        shippingAddress: 'Test Address',
        paymentMethod: 'E_WALLET',
      })
      .expect(201);

    expect(orderRes.body.totalAmount).toBe(200000);
    expect(orderRes.body.status).toBe('PENDING');

    // Pay
    const payRes = await request(httpServer)
      .post('/api/payments/pay')
      .set('Authorization', `Bearer ${token}`)
      .send({
        orderId: orderRes.body.orderId,
        amount: 200000,
      })
      .expect(201);

    expect(payRes.body.status).toBe('SUCCESS');

    // Wait for WebSocket confirmation
    const notification = await waitForEvent(socket, 'payment.success', 5000);
    expect(notification.orderId).toBe(orderRes.body.orderId);
    expect(notification.status).toBe('SUCCESS');
  });
});
```
