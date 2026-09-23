# 69 - Job Events & Error Handling di Queue

## Penjelasan
Queue berjalan di background, jadi kita tidak bisa lihat langsung apakah job sukses atau gagal. Kita perlu **event listener** untuk memonitor. BullMQ menyediakan events: `active` (sedang diproses), `completed` (selesai), `failed` (gagal), `progress` (progress update), `stalled` (macet). Kalau job gagal total setelah retry habis, kita perlu **dead letter queue** — tempat terakhir sebelum job benar-benar dibuang. Ibarat gedung, events adalah **CCTV dan laporan petugas** — kita bisa lihat apakah paket sudah diantar, gagal, atau hilang.

## Fungsi
- Event `active` — job mulai diproses
- Event `completed` — job selesai
- Event `failed` — job gagal (setelah retry habis)
- Event `progress` — update progress (misal 50%)
- Event `stalled` — worker crash saat proses job
- **Dead Letter Queue** — penyimpanan job gagal untuk investigasi
- Catat error ke database untuk audit

## Cara Pengimplementasian

### 1. Event Listener di Worker

```typescript
// email.processor.ts
import { Processor, WorkerHost, OnWorkerEvent } from '@nestjs/bullmq';
import { Job } from 'bullmq';

@Processor('email')
export class EmailProcessor extends WorkerHost {
  constructor(private prisma: PrismaService) {
    super();
  }

  async process(job: Job): Promise<any> {
    // Update progress
    await job.updateProgress(10);
    console.log(`Proses email ke ${job.data.to}`);

    await job.updateProgress(50);
    // Simulasi kirim email
    await new Promise((r) => setTimeout(r, 1000));

    await job.updateProgress(100);
    return { sent: true };
  }

  @OnWorkerEvent('active')
  onActive(job: Job) {
    console.log(`Job ${job.id} aktif — memproses ${job.data.to}`);
  }

  @OnWorkerEvent('completed')
  onCompleted(job: Job) {
    console.log(`Job ${job.id} selesai — email ke ${job.data.to} terkirim`);
  }

  @OnWorkerEvent('failed')
  async onFailed(job: Job, err: Error) {
    console.error(`Job ${job.id} gagal — ${err.message}`);

    // Simpan ke database
    await this.prisma.failedJob.create({
      data: {
        jobId: job.id?.toString(),
        queue: 'email',
        data: job.data,
        error: err.message,
        attemptsMade: job.attemptsMade,
        failedAt: new Date(),
      },
    });
  }

  @OnWorkerEvent('progress')
  onProgress(job: Job, progress: number) {
    console.log(`Job ${job.id} progress: ${progress}%`);
  }

  @OnWorkerEvent('stalled')
  onStalled(job: Job) {
    console.warn(`Job ${job.id} stalled — worker mungkin crash`);
  }
}
```

### 2. Dead Letter Queue Pattern

```typescript
// dead-letter.module.ts
@Module({
  imports: [
    BullModule.registerQueue(
      { name: 'email' },
      { name: 'email-dlq' }, // Dead Letter Queue
    ),
  ],
  providers: [EmailProcessor, DlqProcessor, DlqService],
})
export class EmailModule {}
```

```typescript
// dlq.processor.ts — proses job dari DLQ
@Processor('email-dlq')
export class DlqProcessor extends WorkerHost {
  constructor(private prisma: PrismaService) {
    super();
  }

  async process(job: Job): Promise<any> {
    // DLQ job: catat detail gagal untuk investigasi
    await this.prisma.deadLetter.create({
      data: {
        originalJobId: job.data.originalJobId,
        queue: job.data.queue,
        data: job.data.originalData,
        error: job.data.error,
        attemptsMade: job.data.attemptsMade,
        receivedAt: new Date(),
      },
    });

    // Kirim notifikasi ke admin
    console.log(`[DLQ] Job ${job.data.originalJobId} masuk dead letter queue`);

    return { logged: true };
  }
}
```

```typescript
// email.service.ts — kirim ke DLQ jika gagal total
@Injectable()
export class EmailService {
  constructor(
    @InjectQueue('email') private emailQueue: Queue,
    @InjectQueue('email-dlq') private dlqQueue: Queue,
  ) {}

  async handleFailedJob(job: Job, error: Error) {
    if (job.attemptsMade >= (job.opts.attempts || 1)) {
      // Kirim ke dead letter queue
      await this.dlqQueue.add('failed-job', {
        originalJobId: job.id,
        queue: 'email',
        originalData: job.data,
        error: error.message,
        attemptsMade: job.attemptsMade,
      });
    }
  }
}
```

### 3. Global Queue Events

```typescript
// queue.event.service.ts
@Injectable()
export class QueueEventService implements OnModuleInit {
  constructor(@InjectQueue('email') private emailQueue: Queue) {}

  onModuleInit() {
    this.emailQueue.on('completed', (job: Job) => {
      console.log(`[Global] Job ${job.id} completed`);
    });

    this.emailQueue.on('failed', (job: Job, err: Error) => {
      console.error(`[Global] Job ${job.id} failed: ${err.message}`);
    });
  }
}
```

### 4. Failed Job DB Model (Prisma)

```prisma
model FailedJob {
  id           Int      @id @default(autoincrement())
  jobId        String?
  queue        String
  data         Json
  error        String
  attemptsMade Int
  failedAt     DateTime @default(now())
}

model DeadLetter {
  id             Int      @id @default(autoincrement())
  originalJobId  String?
  queue          String
  data           Json
  error          String
  attemptsMade   Int
  receivedAt     DateTime @default(now())
}
```

## Analogi
- **Event active** = CCTV menunjukkan petugas **mulai baca** kertas di kotak saran
- **Event completed** = CCTV menunjukkan paket **sukses diantar** ke tujuan
- **Event failed** = CCTV menunjukkan paket **gagal diantar** (alamat salah)
- **Event stalled** = Petugas **jatuh sakit** saat sedang mengantar — job-nya macet
- **Dead Letter Queue** = Kotak khusus untuk **surat-surat yang tidak bisa dikirim** — nanti diperiksa supervisor
- **Simpan failed job ke DB** = **Buku laporan** yang mencatat semua kegagalan pengiriman

## Dipakai untuk Apa
- Monitoring kesehatan queue
- Audit trail job failure
- Alerting ketika job gagal
- Investigasi error secara mendalam
- Retry manual dari DLQ

## Kesalahan Umum
- **Tidak listen event failed** — developer tidak tahu job gagal
- **Event handler blocking** — handler berat bikin worker lambat
- **Lupa simpan stack trace** — error message doang, susah debug
- **DLQ tidak dimonitor** — job numpuk di DLQ, tidak ada yang investigasi
- **Progress tidak diupdate** — user tidak lihat progress job

## Soal Latihan

**Soal:**
Buat implementasi error handling untuk email queue: tangkap event `failed`, simpan detail kegagalan ke database, dan kirim job ke dead letter queue jika sudah retry maksimal.

**Jawaban:**

```typescript
// email.processor.ts
@Processor('email')
export class EmailProcessor extends WorkerHost {
  constructor(
    private prisma: PrismaService,
    private dlqService: DlqService,
  ) { super(); }

  async process(job: Job): Promise<any> {
    const sent = await this.sendEmail(job.data);
    if (!sent) throw new Error('SMTP connection timeout');
    return sent;
  }

  @OnWorkerEvent('failed')
  async onFailed(job: Job, err: Error) {
    // Simpan ke DB
    await this.prisma.failedJob.create({
      data: {
        jobId: job.id?.toString(),
        queue: 'email',
        data: job.data,
        error: err.message,
        attemptsMade: job.attemptsMade,
      },
    });

    // Jika retry habis, kirim ke DLQ
    if (job.attemptsMade >= (job.opts.attempts || 1)) {
      await this.dlqService.sendToDlq(job, err);
    }
  }
}

// dlq.service.ts
@Injectable()
export class DlqService {
  constructor(@InjectQueue('email-dlq') private dlqQueue: Queue) {}

  async sendToDlq(job: Job, err: Error) {
    await this.dlqQueue.add('failed', {
      originalJobId: job.id,
      originalData: job.data,
      error: err.message,
      attemptsMade: job.attemptsMade,
      queue: 'email',
    });
  }
}
```
