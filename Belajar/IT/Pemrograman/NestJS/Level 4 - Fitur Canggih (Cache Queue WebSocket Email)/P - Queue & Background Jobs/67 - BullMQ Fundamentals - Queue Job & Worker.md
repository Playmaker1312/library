# 67 - BullMQ Fundamentals — Queue, Job & Worker

## Penjelasan
Setelah sukses caching dengan Redis, kita lanjut ke fitur **background job**. Kadang ada tugas yang tidak perlu dikerjakan langsung saat request — misal kirim email, resize gambar, generate laporan. Kalau dikerjakan di request yang sama, user menunggu lama. Ibarat gedung, queue adalah **kotak saran di lobi** — tamu tulis pesan, masukkan ke kotak, nanti petugas ambil dan proses tanpa tamu harus menunggu.

## Fungsi
- **Queue** — tempat antrian job (Redis List)
- **Job** — unit pekerjaan yang dikirim ke queue
- **Worker** — pemroses job dari queue
- **Scheduler** — penjadwal job recurring (cron)
- `@nestjs/bullmq` — integrasi BullMQ untuk NestJS
- `BullModule.registerQueue()` — daftarkan queue

## Cara Pengimplementasian

### 1. Install Package

```bash
npm install @nestjs/bullmq bullmq
```

### 2. Import BullModule

```typescript
// app.module.ts
import { BullModule } from '@nestjs/bullmq';

@Module({
  imports: [
    BullModule.forRoot({
      connection: {
        host: process.env.REDIS_HOST || 'localhost',
        port: Number(process.env.REDIS_PORT) || 6379,
      },
    }),
    EmailModule,
  ],
})
export class AppModule {}
```

### 3. Buat Queue & Worker

**email.module.ts:**

```typescript
import { Module } from '@nestjs/common';
import { BullModule } from '@nestjs/bullmq';
import { EmailService } from './email.service';
import { EmailProcessor } from './email.processor';

@Module({
  imports: [
    BullModule.registerQueue({
      name: 'email',
    }),
  ],
  providers: [EmailService, EmailProcessor],
  exports: [BullModule],
})
export class EmailModule {}
```

**email.processor.ts:**

```typescript
import { Processor, WorkerHost } from '@nestjs/bullmq';
import { Job } from 'bullmq';

@Processor('email')
export class EmailProcessor extends WorkerHost {
  async process(job: Job): Promise<any> {
    console.log(`Memproses email ke ${job.data.to}`);
    console.log(`Subjek: ${job.data.subject}`);

    // Simulasi kirim email
    await new Promise((resolve) => setTimeout(resolve, 1000));

    return { sent: true, to: job.data.to };
  }
}
```

**email.service.ts:**

```typescript
import { Injectable } from '@nestjs/common';
import { InjectQueue } from '@nestjs/bullmq';
import { Queue } from 'bullmq';

@Injectable()
export class EmailService {
  constructor(@InjectQueue('email') private emailQueue: Queue) {}

  async sendWelcomeEmail(userId: number, email: string) {
    await this.emailQueue.add('welcome', {
      userId,
      to: email,
      subject: 'Selamat Datang!',
      template: 'welcome',
    });
    console.log(`Email welcome ditambahkan ke antrian untuk ${email}`);
  }
}
```

**email.controller.ts:**

```typescript
@Controller('email')
export class EmailController {
  constructor(private readonly emailService: EmailService) {}

  @Post('send-welcome')
  async sendWelcome(@Body() dto: { userId: number; email: string }) {
    await this.emailService.sendWelcomeEmail(dto.userId, dto.email);
    return { message: 'Email sedang diproses' };
  }
}
```

### 4. Flow Keseluruhan

```
Client → Controller → Queue (Redis) → Worker → Kirim Email
```

## Analogi
- **Queue** = **Kotak saran** di lobi gedung — semua tamu bisa masukkan pesan
- **Job** = **Selembar kertas pesan** — berisi instruksi siapa, apa, kapan
- **Worker** = **Petugas keamanan** yang setiap 5 menit buka kotak, ambil kertas, dan kerjakan
- **Scheduler** = **Jadwal rutin** — misal setiap jam 8 pagi petugas cek semua lantai

## Dipakai untuk Apa
- Kirim email (welcome, notifikasi, reset password)
- Proses gambar (resize, compress, generate thumbnail)
- Export laporan (PDF, CSV, Excel)
- Webhook/callback ke pihak ketiga
- Sinkronisasi data ke service lain

## Kesalahan Umum
- **Redis connection string salah** — queue tidak bisa diproses
- **Worker tidak terdaftar di module** — job masuk queue tapi tidak diproses
- **Job terlalu besar** — data puluhan MB masuk ke job, bikin Redis berat
- **Tidak handle error di worker** — job gagal tanpa ada log
- **Lupa InjectQueue** — error `InjectQueue requires a queue name`

## Soal Latihan

**Soal:**
Buat queue `email` dengan processor yang mengirim email welcome. Service harus bisa menambahkan job ke queue. Tampilkan cara memanggil dari controller.

**Jawaban:**

```typescript
// app.module.ts
@Module({
  imports: [
    BullModule.forRoot({ connection: { host: 'localhost', port: 6379 } }),
    EmailModule,
  ],
})
export class AppModule {}

// email.module.ts
@Module({
  imports: [BullModule.registerQueue({ name: 'email' })],
  providers: [EmailProcessor, EmailService],
  exports: [EmailService],
})
export class EmailModule {}

// email.processor.ts
@Processor('email')
export class EmailProcessor extends WorkerHost {
  async process(job: Job<any>): Promise<any> {
    console.log(`Mengirim email ke ${job.data.to}: ${job.data.subject}`);
    // Kirim email...
    return { success: true };
  }
}

// email.service.ts
@Injectable()
export class EmailService {
  constructor(@InjectQueue('email') private queue: Queue) {}
  async sendEmail(to: string, subject: string, body: string) {
    await this.queue.add('send', { to, subject, body });
  }
}

// auth.controller.ts — memanggil setelah register
@Post('register')
async register(@Body() dto: RegisterDto) {
  const user = await this.authService.register(dto);
  await this.emailService.sendEmail(user.email, 'Welcome!', '...');
  return user;
}
```
