# 74 - In-App Notification System

## Penjelasan
Email sudah bisa dikirim. Tapi tidak semua notifikasi perlu email — kadang cukup tampil di dalam aplikasi (in-app notification). Misal: "Pesanan Anda telah dikirim", "Ada diskon baru", "Admin membalas komentar". Notifikasi ini disimpan di database dan bisa ditampilkan sebagai **bell icon** di pojok kanan atas aplikasi. Ibarat gedung, in-app notification adalah **papan pengumuman** di lobi — setiap ada info baru, ditempel di papan, siapapun bisa lihat tanpa harus dikirimi surat.

## Fungsi
- **Model Notification** — schema dengan userId, type, message, isRead, metadata
- **NotificationService** — create, findAll, markAsRead, markAllAsRead
- **Endpoint REST** — GET notifikasi, PATCH mark read
- **Integrasi WebSocket** — kirim notifikasi real-time ke user
- **Unread count** — badge di frontend

## Cara Pengimplementasian

### 1. Prisma Model

```prisma
model Notification {
  id        Int      @id @default(autoincrement())
  userId    Int
  type      String   // 'order_update', 'promo', 'system', 'comment'
  title     String
  message   String
  isRead    Boolean  @default(false)
  metadata  Json?    // data tambahan (orderId, link, dll)
  createdAt DateTime @default(now())
  readAt    DateTime?

  user      User     @relation(fields: [userId], references: [id])

  @@index([userId, isRead])
}
```

### 2. NotificationService

```typescript
// notification.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class NotificationService {
  constructor(private prisma: PrismaService) {}

  async create(dto: {
    userId: number;
    type: string;
    title: string;
    message: string;
    metadata?: any;
  }) {
    const notif = await this.prisma.notification.create({
      data: {
        userId: dto.userId,
        type: dto.type,
        title: dto.title,
        message: dto.message,
        metadata: dto.metadata ?? {},
      },
    });

    return notif;
  }

  async findByUser(userId: number, page = 1, limit = 20) {
    const where = { userId };

    const [data, total, unreadCount] = await Promise.all([
      this.prisma.notification.findMany({
        where,
        orderBy: { createdAt: 'desc' },
        skip: (page - 1) * limit,
        take: limit,
      }),
      this.prisma.notification.count({ where }),
      this.prisma.notification.count({ where: { ...where, isRead: false } }),
    ]);

    return {
      data,
      meta: { total, page, limit, totalPages: Math.ceil(total / limit) },
      unreadCount,
    };
  }

  async markAsRead(id: number, userId: number) {
    return this.prisma.notification.updateMany({
      where: { id, userId },
      data: { isRead: true, readAt: new Date() },
    });
  }

  async markAllAsRead(userId: number) {
    return this.prisma.notification.updateMany({
      where: { userId, isRead: false },
      data: { isRead: true, readAt: new Date() },
    });
  }

  async getUnreadCount(userId: number): Promise<number> {
    return this.prisma.notification.count({
      where: { userId, isRead: false },
    });
  }
}
```

### 3. Notification Controller

```typescript
// notification.controller.ts
import { Controller, Get, Patch, Param, Query, UseGuards, Request } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@Controller('notifications')
@UseGuards(AuthGuard('jwt'))
export class NotificationController {
  constructor(private readonly notifService: NotificationService) {}

  @Get()
  async findAll(
    @Request() req,
    @Query('page') page?: string,
    @Query('limit') limit?: string,
  ) {
    return this.notifService.findByUser(
      req.user.id,
      page ? +page : 1,
      limit ? +limit : 20,
    );
  }

  @Get('unread-count')
  async unreadCount(@Request() req) {
    const count = await this.notifService.getUnreadCount(req.user.id);
    return { unreadCount: count };
  }

  @Patch(':id/read')
  async markRead(@Param('id') id: string, @Request() req) {
    await this.notifService.markAsRead(+id, req.user.id);
    return { message: 'Notifikasi ditandai sudah dibaca' };
  }

  @Patch('read-all')
  async markAllRead(@Request() req) {
    await this.notifService.markAllAsRead(req.user.id);
    return { message: 'Semua notifikasi ditandai sudah dibaca' };
  }
}
```

### 4. Integrasi dengan Queue + Event

```typescript
// notification.processor.ts
@Processor('notification')
export class NotificationProcessor extends WorkerHost {
  constructor(
    private notifService: NotificationService,
    private wsGateway: NotificationGateway, // WebSocket
  ) {
    super();
  }

  async process(job: Job): Promise<any> {
    const { userId, type, title, message, metadata } = job.data;

    // 1. Simpan ke database
    const notif = await this.notifService.create({
      userId, type, title, message, metadata,
    });

    // 2. Kirim real-time via WebSocket
    this.wsGateway.sendToUser(userId, {
      event: 'notification',
      data: notif,
    });

    return notif;
  }
}
```

### 5. WebSocket Gateway (Preview)

```typescript
// notification.gateway.ts
import {
  WebSocketGateway,
  WebSocketServer,
  SubscribeMessage,
  OnGatewayConnection,
} from '@nestjs/websockets';
import { Server, Socket } from 'socket.io';

@WebSocketGateway({ cors: true })
export class NotificationGateway implements OnGatewayConnection {
  @WebSocketServer()
  server: Server;

  private userSockets: Map<number, Set<string>> = new Map();

  handleConnection(client: Socket) {
    const userId = client.data.user?.id;
    if (userId) {
      if (!this.userSockets.has(userId)) {
        this.userSockets.set(userId, new Set());
      }
      this.userSockets.get(userId).add(client.id);
    }
  }

  handleDisconnect(client: Socket) {
    for (const [userId, sockets] of this.userSockets) {
      sockets.delete(client.id);
      if (sockets.size === 0) this.userSockets.delete(userId);
    }
  }

  sendToUser(userId: number, payload: any) {
    const sockets = this.userSockets.get(userId);
    if (sockets) {
      for (const socketId of sockets) {
        this.server.to(socketId).emit(payload.event, payload.data);
      }
    }
  }
}
```

### 6. Trigger Notifikasi dari Service Lain

```typescript
// order.service.ts
@Injectable()
export class OrderService {
  constructor(
    @InjectQueue('notification') private notifQueue: Queue,
  ) {}

  async updateOrderStatus(orderId: number, status: string) {
    const order = await this.prisma.order.update({
      where: { id: orderId },
      data: { status },
    });

    // Kirim notifikasi ke user
    await this.notifQueue.add('send', {
      userId: order.userId,
      type: 'order_update',
      title: 'Pesanan Diupdate',
      message: `Pesanan #${order.id} sekarang: ${status}`,
      metadata: { orderId: order.id, status },
    });
  }
}
```

## Analogi
In-app notification adalah **papan pengumuman digital** di lobi gedung:
- **Model Notification** = **Papan itu sendiri** — ada sekat-sekat (userId), kategori (type), isi pesan (message)
- **NotificationService** = **Petugas papan pengumuman** — tempel notif (create), cek notif (findAll), centang sudah dibaca (markRead)
- **Unread count** = **Stiker merah "BARU!"** yang ditempel di notif yang belum dibaca
- **WebSocket** = **Bel pintu otomatis** yang berbunyi setiap kali ada notif baru — langsung tahu tanpa harus lihat papan terus-terusan

## Dipakai untuk Apa
- Notifikasi order (status berubah)
- Notifikasi komentar/reply
- Notifikasi promo/diskon
- Notifikasi sistem (maintenance, update)
- Notifikasi sosial (follow, like, share)

## Kesalahan Umum
- **Tidak ada pagination** — notifikasi ribuan, API lambat
- **Tidak ada unreadCount** — frontend harus hitung manual
- **Notifikasi dikirim tanpa queue** — proses jadi lambat
- **WebSocket tidak handle reconnect** — user disconnect, tidak terima notif
- **Metadata tidak disimpan** — frontend tidak bisa navigasi ke halaman terkait

## Soal Latihan

**Soal:**
Buat notification system dengan:
1. Model Notification (userId, type, message, isRead, metadata)
2. Service create, findByUser (with pagination + unreadCount), markAsRead, markAllAsRead
3. Endpoint GET /notifications dan PATCH /notifications/:id/read

**Jawaban:**

```prisma
model Notification {
  id        Int      @id @default(autoincrement())
  userId    Int
  type      String
  title     String
  message   String
  isRead    Boolean  @default(false)
  metadata  Json?
  createdAt DateTime @default(now())
  readAt    DateTime?

  @@index([userId, isRead])
}
```

```typescript
// notification.service.ts
@Injectable()
export class NotificationService {
  constructor(private prisma: PrismaService) {}

  async create(dto: { userId: number; type: string; title: string; message: string; metadata?: any }) {
    return this.prisma.notification.create({ data: { ...dto, metadata: dto.metadata ?? {} } });
  }

  async findByUser(userId: number, page = 1, limit = 20) {
    const where = { userId };
    const [data, total, unreadCount] = await Promise.all([
      this.prisma.notification.findMany({ where, orderBy: { createdAt: 'desc' }, skip: (page-1)*limit, take: limit }),
      this.prisma.notification.count({ where }),
      this.prisma.notification.count({ where: { ...where, isRead: false } }),
    ]);
    return { data, meta: { total, page, limit, totalPages: Math.ceil(total/limit) }, unreadCount };
  }

  async markAsRead(id: number, userId: number) {
    return this.prisma.notification.updateMany({ where: { id, userId }, data: { isRead: true, readAt: new Date() } });
  }

  async markAllAsRead(userId: number) {
    return this.prisma.notification.updateMany({ where: { userId, isRead: false }, data: { isRead: true, readAt: new Date() } });
  }
}

// notification.controller.ts
@Controller('notifications')
@UseGuards(AuthGuard('jwt'))
export class NotificationController {
  constructor(private readonly notifService: NotificationService) {}

  @Get()
  async findAll(@Request() req, @Query('page') page?: string) {
    return this.notifService.findByUser(req.user.id, page ? +page : 1);
  }

  @Patch(':id/read')
  async markRead(@Param('id') id: string, @Request() req) {
    await this.notifService.markAsRead(+id, req.user.id);
    return { message: 'OK' };
  }

  @Patch('read-all')
  async markAllRead(@Request() req) {
    await this.notifService.markAllAsRead(req.user.id);
    return { message: 'Semua sudah dibaca' };
  }
}
```
