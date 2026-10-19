# 68 - Job Options — Priority, Delay, Retry & Repeat

## Penjelasan
Queue dasar sudah jalan. Tapi di dunia nyata, tidak semua job sama pentingnya. Email verifikasi harus segera dikirim (priority tinggi), sementara email newsletter bisa nanti (delay). Kalau gagal, harus dicoba ulang (retry). Ada juga job rutin setiap malam (repeat). Ibarat gedung: ada paket **prioritas** (dokumen penting) yang harus diantar duluan, paket **biasa** bisa diantar nanti, paket **gagal** dikirim ulang 3 kali sebelum dikembalikan, dan ada **jadwal kebersihan** setiap hari jam 10 malam.

## Fungsi
- **Priority** — urutkan job berdasarkan prioritas (1 = tertinggi)
- **Delay** — tunda eksekusi job (ms)
- **Attempts + Backoff** — retry otomatis dengan exponential backoff
- **Repeat** — job berulang dengan cron expression
- **RemoveOnComplete / RemoveOnFail** — hapus job otomatis

## Cara Pengimplementasian

### 1. Priority Job

```typescript
// Prioritas 1 = paling penting
await this.emailQueue.add('verification', data, {
  priority: 1,
});

// Prioritas 10 = kurang penting
await this.emailQueue.add('newsletter', data, {
  priority: 10,
});
```

### 2. Delay Job

```typescript
// Kirim email reminder 1 jam lagi
await this.emailQueue.add('reminder', data, {
  delay: 60 * 60 * 1000, // 1 jam dalam ms
});

// Kirim email ulang tahun besok
await this.emailQueue.add('birthday', data, {
  delay: 24 * 60 * 60 * 1000,
});
```

### 3. Retry dengan Exponential Backoff

```typescript
await this.emailQueue.add('send', data, {
  attempts: 3, // coba 3 kali
  backoff: {
    type: 'exponential', // 1s → 2s → 4s
    delay: 1000, // delay pertama 1 detik
  },
});

// Atau fixed delay:
// backoff: { type: 'fixed', delay: 5000 } // tunggu 5 detik setiap retry
```

### 4. Repeat / Cron Job

```typescript
// app.module.ts
import { ScheduleModule } from '@nestjs/schedule';

@Module({
  imports: [ScheduleModule.forRoot()],
})
// Atau via queue:

// Di service:
await this.reportQueue.add('daily-report', {}, {
  repeat: {
    pattern: '0 0 * * *', // setiap hari jam 00:00 UTC
    // pattern: '*/5 * * * *', // setiap 5 menit
    // pattern: '0 8 * * 1-5', // setiap hari kerja jam 8 pagi
  },
  removeOnComplete: { age: 7 * 24 * 3600 }, // simpan 7 hari
});

// Hentikan job repeat:
const jobs = await this.reportQueue.getRepeatableJobs();
for (const job of jobs) {
  await this.reportQueue.removeRepeatableByKey(job.key);
}
```

### 5. Kombinasi Lengkap

```typescript
@Injectable()
export class OrderService {
  constructor(
    @InjectQueue('order') private orderQueue: Queue,
    @InjectQueue('email') private emailQueue: Queue,
  ) {}

  async processOrder(orderId: number) {
    // Job prioritas tinggi — proses order
    await this.orderQueue.add('process', { orderId }, {
      priority: 2,
      attempts: 3,
      backoff: { type: 'exponential', delay: 2000 },
      removeOnComplete: true,
      removeOnFail: { age: 86400 }, // simpan failed job 1 hari
    });

    // Kirim email konfirmasi — delay 10 detik
    await this.emailQueue.add('order-confirmation', { orderId }, {
      delay: 10_000,
      priority: 5,
      attempts: 5,
      backoff: { type: 'fixed', delay: 10_000 },
    });
  }

  // Cron job — laporan malam
  async scheduleDailyReport() {
    await this.emailQueue.add('daily-report', {}, {
      repeat: { pattern: '0 22 * * *' }, // jam 10 malam
      priority: 20,
    });
  }
}
```

### 6. Worker Handler Retry

```typescript
@Processor('email')
export class EmailProcessor extends WorkerHost {
  async process(job: Job): Promise<any> {
    try {
      // Kirim email
      await this.sendEmail(job.data);
    } catch (error) {
      if (job.attemptsMade < (job.opts.attempts || 1)) {
        throw error; // akan di-retry oleh BullMQ
      }
      // Gagal total — log ke DB
      await this.saveFailedJob(job, error);
    }
  }
}
```

## Analogi
- **Priority** = Paket **YES** (urgent) vs paket **REGULER** di jasa pengiriman
- **Delay** = Surat pengingat yang baru dikirim besok, bukan sekarang
- **Retry + Backoff** = Tukang pos gagal antar paket → coba lagi 1 jam → 2 jam → 4 jam
- **Repeat / Cron** = Kebersihan gedung setiap Senin pagi — rutin tanpa diingatkan
- **RemoveOnComplete** = Setelah paket diantar, hapus dari catatan

## Dipakai untuk Apa
- Email verifikasi (priority tinggi, retry 3x)
- Email promosi (delay, priority rendah)
- Laporan harian/mingguan (repeat cron)
- Proses pembayaran (retry + exponential backoff)
- Reminder janji temu (delay 24 jam)

## Kesalahan Umum
- **Backoff lupa diset** — retry coba lagi dalam 0 detik, langsung gagal lagi
- **Delay terlalu panjang** — user complaint karena email tak kunjung sampai
- **Repeat job tidak di-remove** — setelah tidak diperlukan, job tetap jalan terus
- **Attempts terlalu besar** — retry 100x, server jadi spam
- **Priority diset sama untuk semua** — tidak optimal

## Soal Latihan

**Soal:**
Buat job retry 3x dengan exponential backoff untuk kirim email. Tambahkan cron job setiap malam jam 12 untuk mengirim laporan harian via email.

**Jawaban:**

```typescript
// email.service.ts
@Injectable()
export class EmailService {
  constructor(@InjectQueue('email') private queue: Queue) {}

  async sendWithRetry(to: string, subject: string, body: string) {
    await this.queue.add('send-critical', { to, subject, body }, {
      attempts: 3,
      backoff: { type: 'exponential', delay: 1000 },
      removeOnComplete: true,
      removeOnFail: { age: 7 * 24 * 3600 },
    });
  }

  async scheduleDailyReport() {
    // Hapus repeat job lama
    const jobs = await this.queue.getRepeatableJobs();
    for (const j of jobs) { await this.queue.removeRepeatableByKey(j.key); }

    await this.queue.add('daily-report', {}, {
      repeat: { pattern: '0 0 * * *' },
      priority: 20,
    });
  }
}

// email.processor.ts
@Processor('email')
export class EmailProcessor extends WorkerHost {
  async process(job: Job): Promise<any> {
    console.log(`[${job.name}] Mencoba ke-${job.attemptsMade + 1}: ${job.data.to}`);

    if (job.name === 'send-critical') {
      // Simulasi sukses/gagal
      if (Math.random() < 0.3) throw new Error('Gagal kirim');
      console.log(`Email terkirim ke ${job.data.to}`);
    }

    if (job.name === 'daily-report') {
      console.log('Mengirim laporan harian...');
    }
  }
}
```
