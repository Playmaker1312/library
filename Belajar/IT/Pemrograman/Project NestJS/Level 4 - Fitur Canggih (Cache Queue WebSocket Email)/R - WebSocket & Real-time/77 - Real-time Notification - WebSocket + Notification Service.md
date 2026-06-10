# 77 - Real-time Notification - WebSocket + Notification Service

---

## Penjelasan (Keterkaitan dengan Materi Sebelumnya)

Di **Poin 74 (Notification Service)** kita sudah membuat service untuk **menyimpan notifikasi** di database. Di **Poin 75-76** kita belajar **Gateway + Auth + Room**. Sekarang kita **gabungkan keduanya**:

> Saat notifikasi dibuat → push real-time via WebSocket → user langsung terima tanpa reload

Plus, di aplikasi production dengan **banyak server** (horizontal scaling), Socket.io perlu **Redis adapter** supaya event dari server A bisa diteruskan ke client yang connect ke server B.

---

## Fungsi

| Fungsi | Penjelasan |
|--------|------------|
| **Push notifikasi real-time** | Ketika notifikasi dibuat, langsung kirim ke user via WebSocket |
| **Redis store socket ID** | Mapping user → socket ID agar tahu user sedang online di mana |
| **Redis adapter Socket.io** | Memungkinkan broadcast antar server (multi-instance) |
| **Online status tracking** | Track user mana yang sedang online |

---

## Analogi: Papan Pengumuman Digital di Gedung Bertingkat

Bayangkan gedung bertingkat dengan sistem **pengumuman digital**:

- **Notification Service (Poin 74)** = Depan gedung yang **menulis pengumuman** di secarik kertas.
- **Gateway + Room (Poin 75-76)** = Papan interkom yang sudah terpasang di setiap kamar.
- **Integrasi** = Sekarang, setiap kali admin (depan gedung) menulis pengumuman, resepsionis **langsung mengumumkan lewat interkom** ke kamar yang dituju — tanpa harus fotokopi dan tempel secara manual.
- **Redis Adapter** = Jika ada **dua gedung kembar** (server instance A dan B), Redis adalah **kabel telepon** yang menghubungkan interkom gedung A ke gedung B — pengumuman dari gedung A bisa didengar di kamar gedung B.
- **Redis store socket ID** = Buku catatan resepsionis: "si Budi (user:123) ada di interkom nomor 5" → kalau mau kirim pengumuman ke Budi, tinggal lihat buku, cari interkom nomor 5.

---

## Cara Pengimplementasian (Code)

### 1. Install Redis Adapter

```bash
npm install @socket.io/redis-adapter redis
```

### 2. Setup Redis Adapter di main.ts

```typescript
// main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { createAdapter } from '@socket.io/redis-adapter';
import { createClient } from 'redis';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Setup Redis pub/sub untuk Socket.io adapter
  const pubClient = createClient({ url: 'redis://localhost:6379' });
  const subClient = pubClient.duplicate();
  await Promise.all([pubClient.connect(), subClient.connect()]);

  // Dapatkan HTTP server dari NestJS
  const httpServer = app.getHttpServer();

  // Setup Socket.io dengan Redis adapter
  const io = require('socket.io')(httpServer, {
    cors: { origin: '*' },
    adapter: createAdapter(pubClient, subClient),
  });

  // (Opsional) Gunakan custom IoAdapter, lihat dokumentasi NestJS untuk detail
  await app.listen(3000);
}
```

> **Catatan**: Untuk NestJS murni, buat custom `RedisIoAdapter` yang extend `IoAdapter`.

### 3. Custom Redis IoAdapter

```typescript
// redis-io.adapter.ts
import { IoAdapter } from '@nestjs/platform-socket.io';
import { ServerOptions } from 'socket.io';
import { createAdapter } from '@socket.io/redis-adapter';
import { createClient } from 'redis';

export class RedisIoAdapter extends IoAdapter {
  private adapterConstructor: ReturnType<typeof createAdapter>;

  async connectToRedis(): Promise<void> {
    const pubClient = createClient({ url: 'redis://localhost:6379' });
    const subClient = pubClient.duplicate();
    await Promise.all([pubClient.connect(), subClient.connect()]);
    this.adapterConstructor = createAdapter(pubClient, subClient);
  }

  createIOServer(port: number, options?: ServerOptions) {
    const server = super.createIOServer(port, options);
    server.adapter(this.adapterConstructor);
    return server;
  }
}
```

```typescript
// main.ts (pakai custom adapter)
import { RedisIoAdapter } from './redis-io.adapter';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  const redisIoAdapter = new RedisIoAdapter(app);
  await redisIoAdapter.connectToRedis();
  app.useWebSocketAdapter(redisIoAdapter);
  await app.listen(3000);
}
```

### 4. Gateway Notifikasi

```typescript
// notification.gateway.ts
import {
  WebSocketGateway,
  WebSocketServer,
  OnGatewayConnection,
  OnGatewayDisconnect,
} from '@nestjs/websockets';
import { Server, Socket } from 'socket.io';
import { Injectable } from '@nestjs/common';
import { JwtService } from '@nestjs/jwt';

@Injectable()
@WebSocketGateway({
  cors: { origin: '*' },
  namespace: '/notifications',
})
export class NotificationGateway implements OnGatewayConnection, OnGatewayDisconnect {
  constructor(private jwtService: JwtService) {}

  @WebSocketServer()
  server: Server;

  // Track online users: userId -> Set<socketId>
  private onlineUsers = new Map<string, Set<string>>();

  async handleConnection(client: Socket) {
    try {
      const token = client.handshake.auth?.token;
      const payload = await this.jwtService.verifyAsync(token);
      const userId = payload.sub;

      client.data.userId = userId;

      // Join room pribadi
      client.join(`user:${userId}`);

      // Track online user
      if (!this.onlineUsers.has(userId)) {
        this.onlineUsers.set(userId, new Set());
      }
      this.onlineUsers.get(userId).add(client.id);

      // Broadcast online status
      this.server.emit('userOnline', { userId, online: true });

      console.log(`[NOTIF] User ${userId} online (socket: ${client.id})`);
    } catch {
      client.disconnect();
    }
  }

  handleDisconnect(client: Socket) {
    const userId = client.data.userId;
    if (userId && this.onlineUsers.has(userId)) {
      this.onlineUsers.get(userId).delete(client.id);

      // Jika tidak ada socket lain untuk user ini
      if (this.onlineUsers.get(userId).size === 0) {
        this.onlineUsers.delete(userId);
        this.server.emit('userOnline', { userId, online: false });
      }
    }
    console.log(`[NOTIF] User ${userId} offline`);
  }

  // Method yang dipanggil oleh service
  sendNotificationToUser(userId: string, notification: any) {
    this.server.to(`user:${userId}`).emit('notification', notification);
  }

  getOnlineUsers(): string[] {
    return Array.from(this.onlineUsers.keys());
  }

  isUserOnline(userId: string): boolean {
    return this.onlineUsers.has(userId);
  }
}
```

### 5. Integrasi Notification Service + Gateway

```typescript
// notification.service.ts
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { Notification, NotificationDocument } from './notification.schema';
import { NotificationGateway } from './notification.gateway';

@Injectable()
export class NotificationService {
  constructor(
    @InjectModel(Notification.name)
    private notifModel: Model<NotificationDocument>,
    private notifGateway: NotificationGateway,
  ) {}

  async create(createDto: { userId: string; title: string; message: string; type?: string }) {
    // 1. Simpan ke database
    const notification = await this.notifModel.create({
      userId: createDto.userId,
      title: createDto.title,
      message: createDto.message,
      type: createDto.type || 'info',
      read: false,
    });

    // 2. Kirim real-time via WebSocket
    //    (akan langsung diterima client yang sedang online)
    this.notifGateway.sendNotificationToUser(createDto.userId, notification);

    return notification;
  }

  async markAsRead(notifId: string, userId: string) {
    const updated = await this.notifModel.findOneAndUpdate(
      { _id: notifId, userId },
      { read: true },
      { new: true },
    );

    // (Opsional) kirim event "notifikasi sudah dibaca" ke user
    if (updated) {
      this.notifGateway.sendNotificationToUser(userId, {
        action: 'markRead',
        notifId,
      });
    }

    return updated;
  }
}
```

### 6. Redis Store Socket Mapping (Lebih Aman)

```typescript
// redis-socket.store.ts
import { Injectable } from '@nestjs/common';
import { createClient } from 'redis';

@Injectable()
export class RedisSocketStore {
  private client = createClient({ url: 'redis://localhost:6379' });

  constructor() {
    this.client.connect();
  }

  async setUserSocket(userId: string, socketId: string): Promise<void> {
    await this.client.sAdd(`user_sockets:${userId}`, socketId);
    await this.client.expire(`user_sockets:${userId}`, 86400); // 24 jam
  }

  async removeUserSocket(userId: string, socketId: string): Promise<void> {
    await this.client.sRem(`user_sockets:${userId}`, socketId);
  }

  async getUserSockets(userId: string): Promise<string[]> {
    return this.client.sMembers(`user_sockets:${userId}`);
  }

  async isUserOnline(userId: string): Promise<boolean> {
    const count = await this.client.sCard(`user_sockets:${userId}`);
    return count > 0;
  }
}
```

---

## Dipakai untuk Apa?

- **Notifikasi real-time** — like, comment, follow, mention langsung muncul
- **Online/offline status** — tampilkan user yang sedang online
- **Live activity feed** — update feed tanpa polling
- **Multi-server deployment** — Redis adapter untuk scale horizontally

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| **Gateway tidak di-inject di service** | Pastikan `NotificationGateway` ada di `providers` module |
| **Redis adapter tidak terhubung** | Periksa apakah Redis server berjalan (`redis-cli ping`) |
| **Sirkular dependency** | Gunakan `@Inject(forwardRef(() => ...))` jika service dan gateway saling memanggil |
| **Event tidak sampai ke server lain** | Pastikan Redis adapter sudah terpasang di semua instance |
| **Memory leak — socket tracking tidak dibersihkan** | Hapus socket ID dari map di `handleDisconnect` |

---

## Soal Latihan

### Soal 1: Integrasi Notifikasi Real-time

Buatlah:

1. `NotificationGateway` di namespace `/notif`
2. Di `handleConnection`, verify JWT token dan join room `user:{userId}`
3. `NotificationService.create()` — simpan ke DB lalu panggil gateway untuk notifikasi real-time
4. Module yang mendaftarkan keduanya

```typescript
// ========= JAWABAN =========

// notif-real.module.ts
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { NotificationService } from './notification.service';
import { NotificationGateway } from './notification.gateway';
import { Notification, NotificationSchema } from './notification.schema';
import { JwtModule } from '@nestjs/jwt';

@Module({
  imports: [
    MongooseModule.forFeature([
      { name: Notification.name, schema: NotificationSchema },
    ]),
    JwtModule.register({ secret: process.env.JWT_SECRET }),
  ],
  providers: [NotificationGateway, NotificationService],
  exports: [NotificationService],
})
export class NotifRealModule {}

// notification.gateway.ts
import {
  WebSocketGateway,
  WebSocketServer,
  OnGatewayConnection,
  OnGatewayDisconnect,
} from '@nestjs/websockets';
import { Server, Socket } from 'socket.io';
import { JwtService } from '@nestjs/jwt';

@WebSocketGateway({
  cors: { origin: '*' },
  namespace: '/notif',
})
export class NotificationGateway implements OnGatewayConnection, OnGatewayDisconnect {
  constructor(private jwtService: JwtService) {}

  @WebSocketServer()
  server: Server;

  async handleConnection(client: Socket) {
    try {
      const token = client.handshake.auth?.token;
      const payload = await this.jwtService.verifyAsync(token);
      client.data.userId = payload.sub;
      client.join(`user:${payload.sub}`);
    } catch {
      client.disconnect();
    }
  }

  handleDisconnect(client: Socket) {}

  sendToUser(userId: string, event: string, data: any) {
    this.server.to(`user:${userId}`).emit(event, data);
  }
}

// notification.service.ts
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { Notification } from './notification.schema';
import { NotificationGateway } from './notification.gateway';

@Injectable()
export class NotificationService {
  constructor(
    @InjectModel(Notification.name) private notifModel: Model<Notification>,
    private notifGateway: NotificationGateway,
  ) {}

  async create(dto: { userId: string; title: string; message: string; type?: string }) {
    const notif = await this.notifModel.create({
      userId: dto.userId,
      title: dto.title,
      message: dto.message,
      type: dto.type || 'info',
      read: false,
    });

    this.notifGateway.sendToUser(dto.userId, 'notification', notif);
    return notif;
  }
}
```
