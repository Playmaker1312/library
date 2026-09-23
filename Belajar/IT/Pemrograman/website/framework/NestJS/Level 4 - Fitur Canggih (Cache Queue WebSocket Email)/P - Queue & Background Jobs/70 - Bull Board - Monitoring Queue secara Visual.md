# 70 - Bull Board — Monitoring Queue secara Visual

## Penjelasan
Queue sudah berjalan dengan event handler dan error handling. Tapi developer tidak mungkin terus-terusan lihat console log atau query DB untuk cek status job. Kita butuh **dashboard visual** untuk monitor queue. Bull Board adalah UI yang menampilkan semua queue, job status, retry, dan manage job. Ibarat gedung, Bull Board adalah **ruang kontrol CCTV** yang menampilkan semua aktivitas pengiriman paket secara real-time — petugas mana yang sedang bekerja, paket mana yang macet, dan mana yang gagal.

## Fungsi
- Dashboard visual untuk semua queue
- Lihat job: waiting, active, completed, failed, delayed
- Retry job yang gagal (tombol "Retry")
- Hapus job (cleanup)
- Protect dashboard dengan Auth Guard agar hanya admin yang bisa akses
- Lihat detail job (data, options, stack trace error)

## Cara Pengimplementasian

### 1. Install Package

```bash
npm install @bull-board/nestjs @bull-board/api @bull-board/ui
```

### 2. Setup Bull Board

**app.module.ts:**

```typescript
import { BullModule } from '@nestjs/bullmq';
import { BullBoardModule } from '@bull-board/nestjs';
import { ExpressAdapter } from '@bull-board/express';
import { BullMQAdapter } from '@bull-board/api/bullMQAdapter';

@Module({
  imports: [
    BullModule.forRoot({ connection: { host: 'localhost', port: 6379 } }),

    // Daftarkan queue yang mau dimonitor
    BullModule.registerQueue({ name: 'email' }),
    BullModule.registerQueue({ name: 'image-processing' }),
    BullModule.registerQueue({ name: 'report' }),

    // Setup Bull Board
    BullBoardModule.forRoot({
      route: '/admin/queues',
      adapter: ExpressAdapter, // atau FastifyAdapter
    }),

    // Daftarkan setiap queue ke board
    BullBoardModule.forFeature({
      name: 'email',
      adapter: BullMQAdapter,
    }),
    BullBoardModule.forFeature({
      name: 'image-processing',
      adapter: BullMQAdapter,
    }),
    BullBoardModule.forFeature({
      name: 'report',
      adapter: BullMQAdapter,
    }),
  ],
})
export class AppModule {}
```

### 3. Proteksi dengan Auth Guard

Buat controller wrapper yang melindungi Bull Board:

```typescript
// bull-board.controller.ts
import { Controller, Get, Res, Req, UseGuards } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';
import { InjectQueue } from '@nestjs/bullmq';
import { Queue } from 'bullmq';

@Controller('admin/queues')
@UseGuards(AuthGuard('jwt')) // atau guard custom
export class BullBoardController {
  // Controller ini bisa kosong karena Bull Board handle routing sendiri.
  // Tapi kita perlu pastikan guard bekerja.

  // Alternatif: biarkan BullBoardModule handle route,
  // dan pasang guard global di app module untuk /admin/*
}
```

Atau pasang middleware global untuk route `/admin/*`:

```typescript
// app.module.ts — tambahkan di imports setelah BullBoardModule.forRoot({...})
// Tidak ada cara langsung, tapi kita bisa override route handler:

// Lebih mudah: buat guard di controller yang listen di /admin/queues
```

**Cara termudah — pasang guard di module level:**

Buat `AdminGuard`:

```typescript
// admin.guard.ts
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';

@Injectable()
export class AdminGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const request = context.switchToHttp().getRequest();
    const user = request.user;
    return user?.role === 'admin';
  }
}
```

Terus tambahkan ke app module:

```typescript
// app.module.ts
BullBoardModule.forRoot({
  route: '/admin/queues',
  adapter: ExpressAdapter,
  middleware: AdminGuard, // Guard akan dijalankan sebelum akses dashboard
}),
```

> **Catatan:** BullBoard v6+ mendukung `middleware` parameter. Untuk versi lama, pasang guard di controller manual.

### 4. Akses Dashboard

Buka browser:

```
http://localhost:3000/admin/queues
```

Anda akan melihat:

```
[email]        ██ Completed: 150  ██ Failed: 3  ██ Waiting: 12
[image-processing]  ██ Completed: 89   ██ Failed: 1  ██ Waiting: 0
[report]       ██ Completed: 12   ██ Failed: 0  ██ Waiting: 1
```

Klik salah satu queue untuk lihat detail job.

## Analogi
Bull Board adalah **ruang kontrol utama** gedung bertingkat yang menampilkan:
- **CCTV** = status job real-time
- **Panel notifikasi** = jumlah waiting, active, failed
- **Tombol darurat** = retry job yang gagal
- **Logbook digital** = detail setiap job (data, error, durasi)
- **Kartu akses** = hanya admin yang boleh masuk ruang kontrol

## Dipakai untuk Apa
- Monitoring queue secara real-time
- Debug job yang gagal dengan lihat stack trace
- Operasional: retry job tanpa harus restart app
- Laporan ke manajemen: berapa banyak job diproses per hari
- Cleanup job yang numpuk (remove all completed/failed)

## Kesalahan Umum
- **Tidak pasang auth guard** — dashboard bisa diakses publik (security risk)
- **Lupa daftarkan queue ke BullBoardModule.forFeature** — queue tidak muncul
- **Route bentrok** — route `/admin/queues` bentrok dengan controller sendiri
- **Bull Board expose data sensitif** — job data bisa lihat password/API key
- **Tidak mengatur `removeOnComplete`** — job numpuk, UI jadi lambat

## Soal Latihan

**Soal:**
Setup Bull Board dengan:
1. Queue `email`, `image`, `report`
2. Route di `/admin/queues`
3. Proteksi AdminGuard (hanya role `admin` yang boleh akses)
4. Retry job gagal dari dashboard

**Jawaban:**

```typescript
// app.module.ts
import { BullModule } from '@nestjs/bullmq';
import { BullBoardModule } from '@bull-board/nestjs';
import { ExpressAdapter } from '@bull-board/express';
import { BullMQAdapter } from '@bull-board/api/bullMQAdapter';
import { AdminGuard } from './common/guards/admin.guard';

@Module({
  imports: [
    BullModule.forRoot({ connection: { host: 'localhost', port: 6379 } }),

    BullModule.registerQueue(
      { name: 'email' },
      { name: 'image' },
      { name: 'report' },
    ),

    BullBoardModule.forRoot({
      route: '/admin/queues',
      adapter: ExpressAdapter,
      middleware: AdminGuard,
    }),

    BullBoardModule.forFeature({ name: 'email', adapter: BullMQAdapter }),
    BullBoardModule.forFeature({ name: 'image', adapter: BullMQAdapter }),
    BullBoardModule.forFeature({ name: 'report', adapter: BullMQAdapter }),
  ],
})
export class AppModule {}

// admin.guard.ts
@Injectable()
export class AdminGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const req = context.switchToHttp().getRequest();
    return req.user?.role === 'admin';
  }
}
```

**Akses:**
```
http://localhost:3000/admin/queues
```
Login sebagai admin → lihat dashboard → retry job gagal klik tombol **Retry**.
