# 71 - Queue Use Cases — Email, Image Processing & Notifikasi

## Penjelasan
Queue sudah siap: ada dashboard, error handling, retry, dan event. Sekarang kita bahas **use case nyata**. Dalam satu aplikasi, queue tidak hanya untuk satu hal. E-commerce butuh: kirim email setelah register, kompres gambar produk, kirim notifikasi push, generate laporan PDF. Semua bisa di-queue. Ibarat gedung, ada beberapa **jalur conveyor** terpisah: satu untuk paket dokumen (email), satu untuk paket besar (gambar), satu untuk memo internal (notifikasi).

## Fungsi
- **Email Queue** — kirim email welcome, order confirmation, reset password
- **Image Processing Queue** — resize, compress, generate thumbnail
- **Notification Queue** — push notifikasi, in-app notification
- **Flow Jobs (Chain)** — job berurutan: upload gambar → kompres → generate thumbnail → notifikasi

## Cara Pengimplementasian

### 1. Multi Queue Setup

```typescript
// app.module.ts
@Module({
  imports: [
    BullModule.forRoot({ connection: { host: 'localhost', port: 6379 } }),
    BullModule.registerQueue(
      { name: 'email', defaultJobOptions: { attempts: 3, backoff: { type: 'exponential', delay: 1000 } } },
      { name: 'image', defaultJobOptions: { attempts: 2 } },
      { name: 'notification', defaultJobOptions: { attempts: 5 } },
    ),
  ],
})
export class AppModule {}
```

### 2. Email Queue — Dipanggil Setelah Register

```typescript
// auth.service.ts
@Injectable()
export class AuthService {
  constructor(
    private usersService: UsersService,
    @InjectQueue('email') private emailQueue: Queue,
  ) {}

  async register(dto: RegisterDto) {
    const user = await this.usersService.create(dto);

    // Kirim email via queue — tidak blocking response
    await this.emailQueue.add('welcome', {
      userId: user.id,
      to: user.email,
      name: user.name,
      template: 'welcome',
    });

    // Kirim email verifikasi
    await this.emailQueue.add('verify-email', {
      userId: user.id,
      to: user.email,
      token: user.verificationToken,
    });

    return user; // Response cepat tanpa nunggu email
  }
}
```

### 3. Image Processing Queue

```typescript
// image.service.ts
@Injectable()
export class ImageService {
  constructor(
    @InjectQueue('image') private imageQueue: Queue,
  ) {}

  async uploadImage(file: Express.Multer.File, productId: number) {
    // Simpan file original
    const url = await this.saveFile(file);

    // Queue processing: buat 3 ukuran thumbnail
    await this.imageQueue.add('resize', {
      filePath: file.path,
      productId,
      sizes: [
        { width: 150, height: 150, suffix: 'thumb' },
        { width: 400, height: 400, suffix: 'medium' },
        { width: 1200, height: 1200, suffix: 'large' },
      ],
    });

    return { url, message: 'Gambar sedang diproses' };
  }
}

// image.processor.ts
@Processor('image')
export class ImageProcessor extends WorkerHost {
  async process(job: Job): Promise<any> {
    const { filePath, sizes } = job.data;

    for (const size of sizes) {
      await job.updateProgress(Math.round((sizes.indexOf(size) / sizes.length) * 100));

      // Proses resize (contoh pakai sharp)
      // const buffer = await sharp(filePath)
      //   .resize(size.width, size.height)
      //   .toBuffer();
      // await this.saveFile(buffer, `${filePath}_${size.suffix}`);

      console.log(`Resize ${size.suffix}: ${size.width}x${size.height}`);
    }

    return { processed: true };
  }
}
```

### 4. Notification Queue

```typescript
// notification.service.ts
@Injectable()
export class NotificationService {
  constructor(
    @InjectQueue('notification') private notifQueue: Queue,
  ) {}

  async sendOrderNotification(userId: number, orderId: number, status: string) {
    await this.notifQueue.add('order-update', {
      userId,
      orderId,
      status,
      channel: ['in-app', 'push', 'email'], // multi-channel
    });
  }
}
```

### 5. Flow Jobs (Chain)

```typescript
// Flow: upload gambar → resize → generate thumbnail → notifikasi selesai
@Injectable()
export class ProductImageFlowService {
  constructor(
    @InjectQueue('image') private imageQueue: Queue,
    @InjectQueue('notification') private notifQueue: Queue,
  ) {}

  async processProductImages(productId: number, files: Express.Multer.File[]) {
    for (const file of files) {
      // Job 1: Upload
      const uploadJob = await this.imageQueue.add('upload', {
        productId,
        fileName: file.filename,
        path: file.path,
      });

      // Job 2: Resize (setelah upload selesai)
      await this.imageQueue.add('resize', {
        productId,
        filePath: file.path,
        parentJobId: uploadJob.id,
      }, {
        delay: 100, // sedikit delay untuk pastikan parent selesai
      });

      // Job 3: Generate Thumbnail
      await this.imageQueue.add('thumbnail', {
        productId,
        filePath: file.path,
      });

      // Job 4: Notifikasi (via queue berbeda)
      await this.notifQueue.add('image-ready', {
        productId,
        fileName: file.filename,
      });
    }
  }
}
```

Atau gunakan **parent-child job** BullMQ:

```typescript
await this.imageQueue.add('resize', data, {
  parent: { id: uploadJob.id, queue: 'image' },
});
```

### 6. Orchestrator Service

```typescript
// order.orchestrator.ts — koordinasi semua queue untuk satu flow
@Injectable()
export class OrderOrchestrator {
  constructor(
    @InjectQueue('email') private emailQueue: Queue,
    @InjectQueue('image') private imageQueue: Queue,
    @InjectQueue('notification') private notifQueue: Queue,
  ) {}

  async onOrderCreated(order: Order) {
    // Jalankan semua task paralel
    await Promise.all([
      this.emailQueue.add('order-confirmation', { orderId: order.id, userId: order.userId }),
      this.imageQueue.add('process-order-images', { orderId: order.id }),
      this.notifQueue.add('new-order', { orderId: order.id, admin: true }),
    ]);
  }
}
```

## Analogi
Gedung bertingkat punya **beberapa jalur conveyor** berbeda:
- **Conveyor Merah** (Email Queue) = untuk dokumen dan surat
- **Conveyor Biru** (Image Queue) = untuk paket besar (gambar, banner)
- **Conveyor Hijau** (Notification Queue) = untuk memo dan pengumuman
- **Flow Chain** = paket masuk conveyor merah → otomatis pindah ke conveyor biru → lalu ke hijau

Setiap conveyor punya pekerja khusus (worker) yang hanya menangani jenis paket tersebut.

## Dipakai untuk Apa
- Flow register → welcome email
- Flow order → confirmation email + invoice PDF
- Flow upload → kompres gambar + thumbnail + notifikasi admin
- Flow reset password → email link + notifikasi sukses
- Scheduled report → generate data + email attachment

## Kesalahan Umum
- **Satu queue untuk semua** — campur aduk email, gambar, notifikasi, susah scaling
- **Tidak handle error per queue** — error gambar gambar ganggu proses email
- **Job terlalu besar** — data gambar base64 masuk ke job data
- **Flow job tanpa error handling** — satu job gagal, chain putus
- **Lupa configure defaultJobOptions** — tiap queue perlu strategy retry sendiri

## Soal Latihan

**Soal:**
Buat flow: user register → simpan user → queue email welcome + queue notifikasi admin. Implementasikan 3 queue terpisah (email, notification) dengan processor masing-masing.

**Jawaban:**

```typescript
// auth.service.ts
@Injectable()
export class AuthService {
  constructor(
    @InjectQueue('email') private emailQueue: Queue,
    @InjectQueue('notification') private notifQueue: Queue,
    private prisma: PrismaService,
  ) {}

  async register(dto: RegisterDto) {
    const user = await this.prisma.user.create({ data: dto });

    await Promise.all([
      this.emailQueue.add('welcome', { userId: user.id, email: user.email, name: user.name }),
      this.notifQueue.add('admin-alert', { type: 'new-user', userId: user.id }),
    ]);

    return user;
  }
}

// email.processor.ts
@Processor('email')
export class EmailProcessor extends WorkerHost {
  async process(job: Job): Promise<any> {
    if (job.name === 'welcome') {
      console.log(`📧 Kirim email welcome ke ${job.data.email}`);
      // await mailService.send({...})
    }
  }
}

// notification.processor.ts
@Processor('notification')
export class NotificationProcessor extends WorkerHost {
  async process(job: Job): Promise<any> {
    if (job.name === 'admin-alert') {
      console.log(`🔔 Notifikasi admin: user baru id ${job.data.userId}`);
      // await this.notifService.sendToAdmin({...})
    }
  }
}

// app.module.ts
@Module({
  imports: [
    BullModule.forRoot({ connection: { host: 'localhost', port: 6379 } }),
    BullModule.registerQueue(
      { name: 'email' },
      { name: 'notification' },
    ),
  ],
  providers: [AuthService, EmailProcessor, NotificationProcessor],
})
export class AppModule {}
```
