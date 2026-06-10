# 75 - WebSocket Gateway - Dasar Real-time di NestJS

---

## Penjelasan (Keterkaitan dengan Materi Sebelumnya)

Setelah kita belajar **Notification Service** di Level 3, notifikasi yang kita buat masih bersifat **pasif** — user harus nge-refresh halaman atau polling API untuk melihat notifikasi baru. Biar notifikasi bisa **real-time**, kita perlu WebSocket.

WebSocket adalah protokol komunikasi dua arah (full-duplex) antara client dan server. NestJS menyediakan module `@nestjs/websockets` yang dibangun di atas **Socket.io** untuk memudahkan implementasi WebSocket.

**Kaitannya dengan materi Level 3:**
- Module → struktur organisasi proyek
- Provider + Service → logic bisnis
- Controller → HTTP request/response
- **Gateway** → logic WebSocket (request/response via event)

---

## Fungsi WebSocket Gateway

| Fungsi | Penjelasan |
|--------|------------|
| **Menerima & mengirim event real-time** | Client kirim event, server process lalu broadcast |
| **Manajemen koneksi** | Handle connect / disconnect client |
| **Group chat / Room** | Client join room tertentu, hanya terima pesan di room itu |
| **Broadcast** | Kirim pesan ke semua client (atau room tertentu) |

---

## Analogi: Sistem Interkom di Gedung Bertingkat

Bayangkan kamu adalah **resepsionis di lobby gedung bertingkat**.

- **Gateway** = Panel interkom utama yang menghubungkan semua ruangan.
- **@WebSocketGateway** = Memasang panel interkom di lobby.
- **server.emit** = Resepsionis berteriak lewat pengeras suara ke **semua** ruangan.
- **socket.emit** = Resepsionis bicara langsung ke satu orang lewat interkom.
- **server.to(room).emit** = Resepsionis mengumumkan sesuatu **hanya ke lantai tertentu**.
- **@SubscribeMessage** = Tombol di interkom untuk menanggapi pesan tertentu (misal "Panggil Pak Budi", maka resepsionis akan respon).
- **Client join room** = Penghuni pindah ke lantai tertentu dan "mendaftarkan" interkomnya ke lantai itu.

Client = penghuni yang punya interkom di kamarnya. Biar bisa dengar pengumuman, interkom harus connect dulu ke panel lobby.

---

## Cara Pengimplementasian (Code)

### 1. Install Dependencies

```bash
npm install @nestjs/websockets @nestjs/platform-socket.io
npm install -D @types/socket.io
```

### 2. Chat Gateway Sederhana

```typescript
// chat.gateway.ts
import {
  WebSocketGateway,
  WebSocketServer,
  SubscribeMessage,
  OnGatewayConnection,
  OnGatewayDisconnect,
} from '@nestjs/websockets';
import { Server, Socket } from 'socket.io';

@WebSocketGateway({
  cors: { origin: '*' },
  namespace: '/chat',  // seperti prefix route di controller
})
export class ChatGateway implements OnGatewayConnection, OnGatewayDisconnect {
  @WebSocketServer()
  server: Server;

  // Track koneksi
  handleConnection(client: Socket) {
    console.log(`Client connected: ${client.id}`);
  }

  handleDisconnect(client: Socket) {
    console.log(`Client disconnected: ${client.id}`);
  }

  // Client kirim: socket.emit('joinRoom', { room: 'lobby' })
  @SubscribeMessage('joinRoom')
  handleJoinRoom(client: Socket, payload: { room: string }) {
    client.join(payload.room);
    client.emit('message', `Kamu join room: ${payload.room}`);

    // Broadcast ke room (termasuk pengirim)
    this.server.to(payload.room).emit('message', {
      sender: 'system',
      text: `${client.id} bergabung ke ${payload.room}`,
    });
  }

  // Client kirim: socket.emit('sendMessage', { room: 'lobby', text: 'Halo' })
  @SubscribeMessage('sendMessage')
  handleMessage(client: Socket, payload: { room: string; text: string }) {
    // Kirim ke semua client di room (kecuali pengirim)
    client.to(payload.room).emit('message', {
      sender: client.id,
      text: payload.text,
    });

    // Atau pakai server.to(room).emit jika ingin termasuk pengirim
    // this.server.to(payload.room).emit('message', { ... });
  }

  // Kirim ke semua client terhubung
  broadcastToAll(message: string) {
    this.server.emit('notification', { text: message });
  }
}
```

### 3. Daftarkan di Module

```typescript
// chat.module.ts
import { Module } from '@nestjs/common';
import { ChatGateway } from './chat.gateway';

@Module({
  providers: [ChatGateway],
})
export class ChatModule {}
```

### 4. Client-Side (Browser contoh)

```html
<script src="https://cdn.socket.io/4.7.2/socket.io.min.js"></script>
<script>
  const socket = io('http://localhost:3000/chat');

  socket.emit('joinRoom', { room: 'lobby' });
  socket.emit('sendMessage', { room: 'lobby', text: 'Halo semua!' });

  socket.on('message', (data) => {
    console.log('Pesan diterima:', data);
  });
</script>
```

---

## Dipakai untuk Apa?

- **Chat aplikasi** — pesan real-time antar user
- **Notifikasi langsung** — ada notifikasi baru, langsung muncul tanpa reload
- **Kolaborasi real-time** — live editing, cursor position sharing
- **Live dashboard** — update data tanpa refresh (misal monitoring server)
- **Game online** — multiplayer state synchronization

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| **Lupa daftarkan gateway di module** | Pastikan `ChatGateway` masuk di `providers` module |
| **CORS error** | Tambahkan `cors: { origin: '*' }` di dekorator gateway |
| **Client tidak dengar event** | Periksa ejaan nama event — harus cocok antara emit dan on |
| **Server crash saat banyak koneksi** | Pastikan Socket.io adapter sudah di-scale pakai Redis |
| **Mengirim pesan tanpa join room** | Client harus panggil `client.join(roomId)` dulu baru bisa dengar event room |

---

## Soal Latihan

### Soal 1: Buat Gateway Chat Lengkap

Buatlah `ChatGateway` dengan fitur:

1. Client bisa **join** room tertentu
2. Client bisa **kirim pesan** ke room
3. **Broadcast** ke semua client di room (termasuk pengirim, gunakan `server.to`)
4. Saat client **disconnect**, otomatis **leave** dari semua room dan kirim notifikasi ke room

```typescript
// ========= JAWABAN =========

import {
  WebSocketGateway,
  WebSocketServer,
  SubscribeMessage,
  OnGatewayConnection,
  OnGatewayDisconnect,
} from '@nestjs/websockets';
import { Server, Socket } from 'socket.io';

@WebSocketGateway({
  cors: { origin: '*' },
  namespace: '/chat',
})
export class ChatGateway implements OnGatewayConnection, OnGatewayDisconnect {
  @WebSocketServer()
  server: Server;

  handleConnection(client: Socket) {
    console.log(`[+] ${client.id} connected`);
  }

  handleDisconnect(client: Socket) {
    console.log(`[-] ${client.id} disconnected`);

    // Notifikasi semua room bahwa user keluar
    // Socket.io otomatis leave dari semua room, kita kirim notifikasi
    client.rooms.forEach((room) => {
      if (room !== client.id) {
        this.server.to(room).emit('message', {
          sender: 'system',
          text: `${client.id} meninggalkan room ${room}`,
        });
      }
    });
  }

  @SubscribeMessage('joinRoom')
  handleJoinRoom(client: Socket, payload: { room: string }) {
    client.join(payload.room);
    this.server.to(payload.room).emit('message', {
      sender: 'system',
      text: `${client.id} bergabung ke ${payload.room}`,
    });
  }

  @SubscribeMessage('sendMessage')
  handleMessage(client: Socket, payload: { room: string; text: string }) {
    this.server.to(payload.room).emit('message', {
      sender: client.id,
      text: payload.text,
      timestamp: new Date().toISOString(),
    });
  }
}
```
