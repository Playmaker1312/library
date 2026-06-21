# 80 - Image Processing - Sharp untuk Optimasi & Resize

---

## Penjelasan (Keterkaitan dengan Materi Sebelumnya)

Di **Poin 78-79** kita belajar upload file ke lokal dan S3. Masalahnya: user upload foto **10 MB** dengan resolusi **4000x3000** — terlalu besar untuk ditampilkan di web.

Kita perlu **memproses gambar**:
- **Resize** — buat thumbnail (150x150), medium (600x600), large (1200x1200)
- **Konversi format** — ubah ke **WebP** (format modern dengan ukuran 30% lebih kecil dari JPEG)
- **Optimasi** — kompres kualitas gambar

**Sharp** adalah library Node.js untuk image processing yang cepat. Kita kombinasikan dengan **Queue (Bull)** dari Poin 70-72 agar processing tidak blocking request.

---

## Fungsi

| Fungsi | Penjelasan |
|--------|------------|
| **Sharp resize** | Ubah ukuran gambar ke dimensi tertentu |
| **Sharp webp** | Konversi gambar ke format WebP |
| **Image processing queue** | Proses gambar di background, tidak blocking response |
| **Generate multiple sizes** | Hasilkan thumbnail, medium, large dari satu upload |
| **Upload hasil ke S3** | Setelah diproses, upload ke S3 |

---

## Analogi: Studio Foto di Gedung Bertingkat

Bayangkan gedung bertingkat punya **studio foto** di lantai dasar:

- **Upload (Poin 78-79)** = Tamu datang ke lobby dan **menyerahkan foto original** (file besar).
- **Sharp** = **Fotografer & editor** di studio yang:
  - Mencetak foto ukuran **kecil** (thumbnail 150x150) — untuk preview
  - Mencetak foto ukuran **sedang** (medium 600x600) — untuk galeri
  - Mencetak foto ukuran **besar** (large 1200x1200) — untuk cetak
  - Mengubah ke **WebP** — seperti laminasi anti air, kualitas sama tapi lebih ringan
- **Queue (Bull)** = Antrean order di studio. Tamu bisa serahkan foto dan **kembali nanti** — tidak perlu nunggu di lobby. Resepsionis (API) langsung balas "OK, lagi diproses", sementara studio mengerjakan di belakang.
- **Upload ke S3** = Setelah foto selesai diproses, hasilnya dikirim ke **gudang pusat (S3)** dan resepsionis kasih link ke tamu.

---

## Cara Pengimplementasian (Code)

### 1. Install Dependencies

```bash
npm install sharp
npm install @nestjs/bull bull ioredis   # jika pakai queue
```

### 2. Image Processor Service (Sharp langsung)

```typescript
// image-processor.service.ts
import { Injectable } from '@nestjs/common';
import * as sharp from 'sharp';
import { join } from 'path';

export interface ImageSizes {
  thumbnail: Buffer;
  medium: Buffer;
  large: Buffer;
}

@Injectable()
export class ImageProcessorService {
  async processImage(
    buffer: Buffer,
    options?: { quality?: number },
  ): Promise<ImageSizes> {
    const quality = options?.quality || 80;

    const [thumbnail, medium, large] = await Promise.all([
      // Thumbnail: 150x150 crop
      sharp(buffer)
        .resize(150, 150, { fit: 'cover' })
        .webp({ quality })
        .toBuffer(),

      // Medium: 600px width, maintain aspect ratio
      sharp(buffer)
        .resize(600, undefined, { fit: 'inside', withoutEnlargement: true })
        .webp({ quality })
        .toBuffer(),

      // Large: 1200px width, maintain aspect ratio
      sharp(buffer)
        .resize(1200, undefined, { fit: 'inside', withoutEnlargement: true })
        .webp({ quality })
        .toBuffer(),
    ]);

    return { thumbnail, medium, large };
  }

  async getMetadata(buffer: Buffer): Promise<sharp.Metadata> {
    return sharp(buffer).metadata();
  }
}
```

### 3. Image Processing Queue (Bull + Sharp)

```typescript
// image-processor.queue.ts
import { Process, Processor } from '@nestjs/bull';
import { Job } from 'bull';
import { Injectable } from '@nestjs/common';
import { ImageProcessorService } from './image-processor.service';
import { S3Service } from '../s3/s3.service';

@Injectable()
@Processor('image-processing')
export class ImageProcessorQueue {
  constructor(
    private imageProcessor: ImageProcessorService,
    private s3Service: S3Service,
  ) {}

  @Process('resize')
  async handleResize(job: Job<{
    originalBuffer: Buffer;  // dalam production, simpan di temp storage dulu
    originalName: string;
    userId: string;
    folder: string;
  }>) {
    const { originalBuffer, originalName, userId, folder } = job.data;

    console.log(`[QUEUE] Memproses gambar: ${originalName}`);

    // 1. Proses gambar jadi 3 ukuran
    const sizes = await this.imageProcessor.processImage(originalBuffer);

    // 2. Upload semua hasil ke S3
    const baseKey = `${folder}/${userId}/${Date.now()}`;

    const [thumbResult, mediumResult, largeResult] = await Promise.all([
      this.s3Service.uploadBuffer(sizes.thumbnail, `${baseKey}/thumbnail.webp`, 'image/webp'),
      this.s3Service.uploadBuffer(sizes.medium, `${baseKey}/medium.webp`, 'image/webp'),
      this.s3Service.uploadBuffer(sizes.large, `${baseKey}/large.webp`, 'image/webp'),
    ]);

    // 3. Update job progress
    await job.progress(100);

    return {
      thumbnail: thumbResult.key,
      medium: mediumResult.key,
      large: largeResult.key,
    };
  }
}
```

### 4. Producer — Panggil Queue dari Controller

```typescript
// photo.controller.ts
import {
  Controller,
  Post,
  UseInterceptors,
  UploadedFile,
  BadRequestException,
} from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';
import { InjectQueue } from '@nestjs/bull';
import { Queue } from 'bull';
import { S3Service } from '../s3/s3.service';

@Controller('photos')
export class PhotoController {
  constructor(
    @InjectQueue('image-processing') private imageQueue: Queue,
    private s3Service: S3Service,
  ) {}

  @Post('upload')
  @UseInterceptors(
    FileInterceptor('photo', {
      fileFilter: (req, file, cb) => {
        cb(null, file.mimetype.startsWith('image/'));
      },
      limits: { fileSize: 10 * 1024 * 1024 }, // 10 MB
    }),
  )
  async uploadPhoto(@UploadedFile() file: Express.Multer.File) {
    if (!file) throw new BadRequestException('File diperlukan');

    // Dapatkan metadata gambar
    const metadata = await sharp(file.buffer).metadata();

    // Kirim ke queue untuk diproses di background
    const job = await this.imageQueue.add('resize', {
      originalBuffer: file.buffer,
      originalName: file.originalname,
      userId: 'user-123', // dari JWT
      folder: 'photos',
    }, {
      attempts: 3,        // retry 3 kali jika gagal
      backoff: 5000,      // tunggu 5 detik antar retry
    });

    return {
      message: 'Foto sedang diproses',
      jobId: job.id,
      metadata: {
        width: metadata.width,
        height: metadata.height,
        size: file.size,
        format: metadata.format,
      },
    };
  }

  @Get('status/:jobId')
  async getJobStatus(@Param('jobId') jobId: string) {
    const job = await this.imageQueue.getJob(jobId);
    if (!job) throw new NotFoundException('Job tidak ditemukan');

    const state = await job.getState();
    const progress = await job.progress();
    const result = job.returnvalue;

    return { jobId, state, progress, result };
  }
}
```

### 5. Tambahkan uploadBuffer di S3Service

```typescript
// Di s3.service.ts, tambahkan method:
async uploadBuffer(buffer: Buffer, key: string, contentType: string) {
  await this.s3.send(new PutObjectCommand({
    Bucket: this.bucket,
    Key: key,
    Body: buffer,
    ContentType: contentType,
  }));
  return { key };
}
```

### 6. Module Setup

```typescript
// image.module.ts
import { Module } from '@nestjs/common';
import { BullModule } from '@nestjs/bull';
import { ImageProcessorService } from './image-processor.service';
import { ImageProcessorQueue } from './image-processor.queue';
import { PhotoController } from './photo.controller';

@Module({
  imports: [
    BullModule.registerQueue({
      name: 'image-processing',
    }),
  ],
  controllers: [PhotoController],
  providers: [ImageProcessorService, ImageProcessorQueue],
})
export class ImageModule {}
```

### 7. Sharp Helper — Fungsi Tambahan

```typescript
// sharp.helper.ts
import * as sharp from 'sharp';

export class SharpHelper {
  // Konversi ke WebP dengan kualitas tertentu
  static async toWebP(buffer: Buffer, quality = 80): Promise<Buffer> {
    return sharp(buffer).webp({ quality }).toBuffer();
  }

  // Resize dengan max width
  static async resizeToWidth(buffer: Buffer, width: number): Promise<Buffer> {
    return sharp(buffer)
      .resize(width, undefined, { fit: 'inside', withoutEnlargement: true })
      .toBuffer();
  }

  // Generate blurhash / blur placeholder (opsional)
  static async getDominantColor(buffer: Buffer): Promise<string> {
    const { dominant } = await sharp(buffer).stats();
    const { r, g, b } = dominant;
    return `rgb(${r},${g},${b})`;
  }
}
```

---

## Dipakai untuk Apa?

- **Upload avatar** — resize ke 150x150, konversi WebP
- **Galeri foto** — thumbnail grid (150x150), detail (600x600), fullscreen (1200x1200)
- **E-commerce** — foto produk dengan ukuran standar
- **Social media** — optimasi gambar upload user
- **CDN optimization** — kirim gambar ukuran tepat sesuai device

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| **Sharp tidak bisa diinstall** | Install `libvips` atau pakai `npm install --platform=linux sharp` |
| **Memory leak — buffer besar** | Gunakan stream (sharp().pipe()) untuk file besar, atau batasi ukuran upload |
| **Lupa konversi ke WebP** | Selalu konversi ke WebP untuk ukuran file lebih kecil (kecuali butuh transparansi) |
| **Queue tidak jalan** | Pastikan Redis server berjalan dan BullModule dikonfigurasi dengan benar |
| **File terlalu besar di queue** | Jangan simpan buffer besar di job data; simpan di temp file, kirim path-nya |
| **Aspect ratio rusak** | Gunakan `fit: 'inside'` atau `fit: 'cover'` sesuai kebutuhan |

---

## Soal Latihan

### Soal 1: Buat Image Processor Queue Lengkap

Buatlah:

1. `ImageProcessorService` dengan method `processImage` — resize 3 ukuran (150, 600, 1200) + konversi WebP
2. `ImageProcessorQueue` (Bull processor) yang memproses gambar dan upload ke S3
3. Controller `POST /photos/upload` yang menerima file, kirim ke queue, kembalikan jobId

```typescript
// ========= JAWABAN =========

// image-processor.service.ts
import { Injectable } from '@nestjs/common';
import * as sharp from 'sharp';

@Injectable()
export class ImageProcessorService {
  async processImage(buffer: Buffer) {
    const [thumb, medium, large] = await Promise.all([
      sharp(buffer).resize(150, 150, { fit: 'cover' }).webp({ quality: 80 }).toBuffer(),
      sharp(buffer).resize(600, undefined, { fit: 'inside' }).webp({ quality: 80 }).toBuffer(),
      sharp(buffer).resize(1200, undefined, { fit: 'inside' }).webp({ quality: 80 }).toBuffer(),
    ]);
    return { thumbnail: thumb, medium, large };
  }
}

// image-processor.queue.ts
import { Process, Processor } from '@nestjs/bull';
import { Job } from 'bull';
import { ImageProcessorService } from './image-processor.service';
import { S3Service } from '../s3/s3.service';

@Processor('image-processing')
export class ImageProcessorQueue {
  constructor(
    private processor: ImageProcessorService,
    private s3: S3Service,
  ) {}

  @Process('resize')
  async handle(job: Job<{ buffer: Buffer; userId: string; folder: string }>) {
    const sizes = await this.processor.processImage(job.data.buffer);
    const base = `${job.data.folder}/${job.data.userId}/${Date.now()}`;
    const [t, m, l] = await Promise.all([
      this.s3.uploadBuffer(sizes.thumbnail, `${base}/thumb.webp`, 'image/webp'),
      this.s3.uploadBuffer(sizes.medium, `${base}/medium.webp`, 'image/webp'),
      this.s3.uploadBuffer(sizes.large, `${base}/large.webp`, 'image/webp'),
    ]);
    return { thumbnail: t.key, medium: m.key, large: l.key };
  }
}

// photo.controller.ts
import { Controller, Post, UseInterceptors, UploadedFile } from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';
import { InjectQueue } from '@nestjs/bull';
import { Queue } from 'bull';

@Controller('photos')
export class PhotoController {
  constructor(@InjectQueue('image-processing') private queue: Queue) {}

  @Post('upload')
  @UseInterceptors(FileInterceptor('photo', {
    fileFilter: (req, file, cb) => cb(null, file.mimetype.startsWith('image/')),
    limits: { fileSize: 10 * 1024 * 1024 },
  }))
  async upload(@UploadedFile() file: Express.Multer.File) {
    const job = await this.queue.add('resize', {
      buffer: file.buffer,
      userId: 'user-123',
      folder: 'photos',
    });
    return { jobId: job.id, message: 'Sedang diproses' };
  }
}

// image.module.ts
import { Module } from '@nestjs/common';
import { BullModule } from '@nestjs/bull';
import { ImageProcessorService } from './image-processor.service';
import { ImageProcessorQueue } from './image-processor.queue';
import { PhotoController } from './photo.controller';

@Module({
  imports: [BullModule.registerQueue({ name: 'image-processing' })],
  controllers: [PhotoController],
  providers: [ImageProcessorService, ImageProcessorQueue],
})
export class ImageModule {}
```
