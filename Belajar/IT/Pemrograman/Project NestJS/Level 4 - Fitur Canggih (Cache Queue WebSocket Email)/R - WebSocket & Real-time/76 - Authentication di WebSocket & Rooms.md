# 76 - Authentication di WebSocket & Rooms

---

## Penjelasan (Keterkaitan dengan Materi Sebelumnya)

Di Poin 75 kita belajar **Gateway dasar** — siapa pun bisa connect, join room, dan kirim pesan tanpa verifikasi. Di dunia nyata, kita harus tahu **siapa** yang connect. Sama seperti di **JWT Auth (Level 2)**, kita perlu verifikasi token sebelum client bisa berkomunikasi via WebSocket.

Selain itu, kita butuh **room per user** — setiap user punya "kamar pribadi" di gedung supaya notifikasi atau pesan yang bersifat personal hanya sampai ke user yang benar.

---

## Fungsi

| Fungsi | Penjelasan |
|--------|------------|
| **JWT verification di handshake** | Memvalidasi token saat koneksi WebSocket dibuat |
| **Middleware Socket.io** | Filter/middleware yang jalan sebelum event diproses |
| **Room per user** | Setiap user join room unik (misal `user:uuid`) untuk kirim pesan personal |
| **Simpan user data di socket** | `socket.data.userId` untuk akses data user di event handler |

---

## Analogi: Security Check & Kamar Pribadi di Gedung Bertingkat

Bayangkan gedung bertingkat dengan sistem keamanan:

- **Handshake (JWT verification)** = Waktu kamu masuk lobby, satpam minta **KTP (token)** dulu. KTP valid? Baru boleh masuk.
- **socket.data.userId** = Satpam kasih kamu **lanyar ID** setelah check-in. Sepanjang kamu di gedung, lanyar itu nempel — satpam tahu siapa kamu tanpa minta KTP lagi.
- **Room per user (`user:uuid`)** = Setiap penghuni punya kamar pribadi. Surat atau paket untukmu hanya dikirim ke kamarmu, bukan ke lobby umum.
- **join:connect / leave:disconnect** = Waktu kamu masuk lift, kamu pencet tombol lantai tujuan (join room). Waktu keluar gedung, kamu otomatis keluar dari semua lantai.

Tanpa JWT verification, siapa pun bisa masuk gedung — termasuk orang yang tidak punya KTP.

---

## Cara Pengimplementasian (Code)

### 1. Auth Guard untuk WebSocket (mirip seperti Guard HTTP)

```typescript
// ws-auth.guard.ts
import { CanActivate, ExecutionContext, Injectable, UnauthorizedException } from '@nestjs/common';
import { JwtService } from '@nestjs/jwt';
import { Socket } from 'socket.io';

@Injectable()
export class WsAuthGuard implements CanActivate {
  constructor(private jwtService: JwtService) {}

  canActivate(context: ExecutionContext): boolean {
    const client: Socket = context.switchToWs().getClient();

    // Ambil token dari handshake auth atau query string
    const token =
      client.handshake.auth?.token ||
      client.handshake.query?.token;

    if (!token) {
      throw new UnauthorizedException('Token tidak ditemukan');
    }

    try {
      const payload = this.jwtService.verify(token);
      // Simpan data user di socket untuk dipakai handler
      client.data.userId = payload.sub;
      client.data.email = payload.email;
      return true;
    } catch {
      throw new UnauthorizedException('Token tidak valid');
    }
  }
}
```

### 2. Gateway dengan Auth + Room per User

```typescript
// chat-auth.gateway.ts
import {
  WebSocketGateway,
  WebSocketServer,
  SubscribeMessage,
  OnGatewayConnection,
  OnGatewayDisconnect,
} from '@nestjs/websockets';
import { Server, Socket } from 'socket.io';
import { UseGuards } from '@nestjs/common';
import { WsAuthGuard } from './ws-auth.guard';

@WebSocketGateway({
  cors: { origin: '*' },
  namespace: '/chat-auth',
})
export class ChatAuthGateway implements OnGatewayConnection, OnGatewayDisconnect {
  @WebSocketServer()
  server: Server;

  async handleConnection(client: Socket) {
    try {
      // Manual verify token di connection
      const token = client.handshake.auth?.token || client.handshake.query?.token;
      if (!token) {
        client.disconnect();
        return;
      }

      // Verifikasi manual (alternative tanpa guard)
      // const payload = await this.jwtService.verifyAsync(token);
      client.data.userId = 'dummy-user-id'; // ganti dengan hasil verify

      // Setelah verify, join room personal: user:{userId}
      const userRoom = `user:${client.data.userId}`;
      client.join(userRoom);

      console.log(`User ${client.data.userId} connected & joined ${userRoom}`);
    } catch {
      client.disconnect();
    }
  }

  handleDisconnect(client: Socket) {
    console.log(`User ${client.data.userId} disconnected`);
    // Socket.io otomatis leave dari semua room
  }

  @UseGuards(WsAuthGuard)
  @SubscribeMessage('sendMessage')
  handleMessage(client: Socket, payload: { room: string; text: string }) {
    // Hanya bisa kirim pesan jika sudah terautentikasi
    this.server.to(payload.room).emit('message', {
      sender: client.data.userId,
      text: payload.text,
    });
  }
}
```

### 3. Middleware Global Socket.io (di main.ts)

```typescript
// main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { IoAdapter } from '@nestjs/platform-socket.io';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Middleware contoh: validasi token di level adapter
  // (tidak dijalankan, hanya ilustrasi)
  // app.useWebSocketAdapter(new AuthIoAdapter(app));

  await app.listen(3000);
}
```

### 4. Kirim Notifikasi ke Room Spesifik dari Service Mana Pun

```typescript
// notification.service.ts (dari poin 74)
@Injectable()
export class NotificationService {
  constructor(
    @Inject('NOTIFICATION_MODEL')
    private notificationModel: Model<Notification>,
    @Inject('USER_MODEL')
    private userModel: Model<User>,
    private webSocketServer: Server, // inject Server
  ) {}

  async createAndNotify(createDto: CreateNotificationDto) {
    const notification = await this.notificationModel.create(createDto);

    // Kirim real-time ke room user tersebut
    const userRoom = `user:${createDto.userId}`;
    this.webSocketServer.to(userRoom).emit('notification', notification);

    return notification;
  }
}
```

### 5. Client-Side dengan Auth

```html
<script src="https://cdn.socket.io/4.7.2/socket.io.min.js"></script>
<script>
  const token = 'Bearer eyJhbGciOiJIUzI1NiIs...'; // JWT token

  const socket = io('http://localhost:3000/chat-auth', {
    auth: { token },
  });

  // Atau bisa lewat query string
  // io('http://localhost:3000/chat-auth?token=...');

  socket.on('connect', () => {
    console.log('Connected & authenticated!');
  });

  socket.on('notification', (data) => {
    console.log('Notifikasi baru:', data);
  });

  socket.on('connect_error', (err) => {
    console.log('Gagal konek:', err.message);
  });
</script>
```

---

## Dipakai untuk Apa?

- **Chat pribadi** — hanya user tertentu yang bisa kirim pesan ke user lain
- **Notifikasi personal** — notifikasi "like", "comment", "follow" hanya sampai ke pemilik akun
- **Admin dashboard** — admin room (khusus admin)
- **Multi-tenant apps** — tiap tenant punya room terisolasi

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| **Token tidak dikirim waktu connect** | Client harus kirim token via `auth` atau `query` di opsi socket.io |
| **Lupa verifikasi token** | Selalu verify di `handleConnection` atau pakai Guard |
| **Client disconnect error** | Bungkus verify dengan try-catch, disconnect client jika gagal |
| **Room name bentrok** | Prefix room dengan `user:` atau `room:` biar unik |
| **Server crash saat verify async** | Pastikan guard/handler adalah async jika pakai `verifyAsync` |

---

## Soal Latihan

### Soal 1: Implementasikan Auth WebSocket

Buatlah:

1. Gateway `AuthGateway` di namespace `/auth`
2. Di `handleConnection`, ambil token dari `handshake.auth.token`
3. Verify JWT (asumsikan ada `JwtService`)
4. Jika valid, join room `user:{userId}`
5. Jika tidak valid, disconnect client dengan reason "Invalid token"

```typescript
// ========= JAWABAN =========

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
  namespace: '/auth',
})
export class AuthGateway implements OnGatewayConnection, OnGatewayDisconnect {
  constructor(private jwtService: JwtService) {}

  @WebSocketServer()
  server: Server;

  async handleConnection(client: Socket) {
    try {
      const token = client.handshake.auth?.token;

      if (!token) {
        client.disconnect();
        return;
      }

      const payload = await this.jwtService.verifyAsync(token);
      client.data.userId = payload.sub;

      const userRoom = `user:${payload.sub}`;
      client.join(userRoom);

      console.log(`[AUTH] User ${payload.sub} masuk room ${userRoom}`);
    } catch {
      client.disconnect();
    }
  }

  handleDisconnect(client: Socket) {
    console.log(`[AUTH] User ${client.data?.userId} keluar`);
  }
}
```
