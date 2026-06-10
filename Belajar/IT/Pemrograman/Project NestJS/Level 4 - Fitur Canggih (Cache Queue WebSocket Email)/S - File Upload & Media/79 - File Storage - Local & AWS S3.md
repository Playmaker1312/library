# 79 - File Storage - Local & AWS S3

---

## Penjelasan (Keterkaitan dengan Materi Sebelumnya)

Di **Poin 78** kita belajar upload file dengan Multer dan menyimpannya di **disk lokal**. Masalahnya:

1. **Disk lokal tidak scalable** — aplikasi dijalankan di banyak server, file hanya tersimpan di satu server
2. **Backup & durability** — file di disk lokal hilang jika server mati
3. **Production-ready** — aplikasi nyata pakai cloud storage (AWS S3, Google Cloud Storage, MinIO)

Solusi: Gunakan **AWS S3** (atau S3-compatible seperti MinIO). Kita akan buat **S3Service** yang handle upload, download (signed URL), dan delete.

---

## Fungsi

| Fungsi | Penjelasan |
|--------|------------|
| **Disk storage (dev only)** | Simpan file lokal untuk development sederhana |
| **S3 upload** | Upload file ke bucket S3 |
| **S3 getSignedUrl** | Generate URL sementara untuk akses file privat |
| **S3 delete** | Hapus file dari bucket |
| **Simpan URL di database** | Setelah upload, simpan path/URL ke DB |

---

## Analogi: Gudang Arsip di Gedung Bertingkat

Bayangkan gedung bertingkat punya **dua opsi penyimpanan**:

- **Disk Local (Dev)** = Lemari arsip di **ruang resepsionis**. Praktis untuk sementara, tapi kalau ruangannya kebakaran, semua arsip hilang. Juga kalau ada resepsionis lain di lantai lain, mereka tidak bisa akses lemari yang sama.
- **AWS S3 (Production)** = **Gudang arsip pusat** di luar gedung. Semua resepsionis dari gedung mana pun bisa akses. Aman, tahan api, dan bisa di-scale.
- **getSignedUrl** = **Surat izin akses** yang berlaku 1 jam — visitor bisa lihat arsip tertentu, setelah itu suratnya kadaluarsa.
- **Simpan URL di DB** = Buku catatan resepsionis: "Arsip si Budi ada di rak nomor S3://bucket/avatars/budi.jpg".

---

## Cara Pengimplementasian (Code)

### 1. Install Dependencies

```bash
npm install @aws-sdk/client-s3 @aws-sdk/s3-request-presigner
```

### 2. S3 Config Module

```typescript
// s3.config.ts
import { S3Client } from '@aws-sdk/client-s3';

export const S3_CONFIG = 'S3_CONFIG';

export const s3ConfigFactory = () => {
  return new S3Client({
    region: process.env.AWS_REGION || 'ap-southeast-1',
    credentials: {
      accessKeyId: process.env.AWS_ACCESS_KEY_ID,
      secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
    },
    // Untuk MinIO / S3-compatible (opsional):
    // endpoint: 'http://localhost:9000',
    // forcePathStyle: true,
  });
};
```

### 3. S3Service

```typescript
// s3.service.ts
import { Injectable, Inject } from '@nestjs/common';
import {
  S3Client,
  PutObjectCommand,
  DeleteObjectCommand,
  GetObjectCommand,
} from '@aws-sdk/client-s3';
import { getSignedUrl } from '@aws-sdk/s3-request-presigner';
import { v4 as uuidv4 } from 'uuid';
import { extname } from 'path';

@Injectable()
export class S3Service {
  constructor(@Inject('S3_CLIENT') private s3: S3Client) {}

  private bucket = process.env.AWS_S3_BUCKET || 'my-app-uploads';

  async uploadFile(
    file: Express.Multer.File,
    folder: string = 'general',
  ): Promise<{ key: string; url: string }> {
    const ext = extname(file.originalname);
    const key = `${folder}/${uuidv4()}${ext}`;

    const command = new PutObjectCommand({
      Bucket: this.bucket,
      Key: key,
      Body: file.buffer,
      ContentType: file.mimetype,
      // ACL: 'public-read', // jika ingin public
    });

    await this.s3.send(command);

    // URL publik (jika public-read):
    // const url = `https://${this.bucket}.s3.${region}.amazonaws.com/${key}`;

    return {
      key,
      url: key, // nanti dipakai getSignedUrl
    };
  }

  async deleteFile(key: string): Promise<void> {
    const command = new DeleteObjectCommand({
      Bucket: this.bucket,
      Key: key,
    });
    await this.s3.send(command);
  }

  async getSignedUrl(key: string, expiresIn: number = 3600): Promise<string> {
    const command = new GetObjectCommand({
      Bucket: this.bucket,
      Key: key,
    });

    return getSignedUrl(this.s3, command, { expiresIn });
  }
}
```

### 4. Module S3

```typescript
// s3.module.ts
import { Module, Global } from '@nestjs/common';
import { S3Service } from './s3.service';
import { s3ConfigFactory } from './s3.config';

@Global() // supaya bisa dipakai di module manapun tanpa import ulang
@Module({
  providers: [
    {
      provide: 'S3_CLIENT',
      useFactory: s3ConfigFactory,
    },
    S3Service,
  ],
  exports: [S3Service],
})
export class S3Module {}
```

### 5. Upload Controller (Menggunakan S3)

```typescript
// avatar-s3.controller.ts
import {
  Controller,
  Post,
  UseInterceptors,
  UploadedFile,
  BadRequestException,
} from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';
import { S3Service } from './s3.service';

@Controller('avatar')
export class AvatarS3Controller {
  constructor(private s3Service: S3Service) {}

  @Post('upload')
  @UseInterceptors(
    FileInterceptor('avatar', {
      fileFilter: (req, file, cb) => {
        const allowed = /jpeg|jpg|png|webp/;
        const isValid = allowed.test(file.mimetype);
        isValid ? cb(null, true) : cb(new BadRequestException('Format tidak didukung'), false);
      },
      limits: { fileSize: 2 * 1024 * 1024 },
    }),
  )
  async uploadAvatar(@UploadedFile() file: Express.Multer.File) {
    if (!file) throw new BadRequestException('File diperlukan');

    // Upload ke S3
    const result = await this.s3Service.uploadFile(file, 'avatars');

    // Generate signed URL (berlaku 1 jam)
    const signedUrl = await this.s3Service.getSignedUrl(result.key, 3600);

    return {
      message: 'Avatar berhasil diupload',
      key: result.key,
      signedUrl,
    };
  }
}
```

### 6. Service yang Simpan URL ke Database

```typescript
// user-profile.service.ts
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { User } from '../user/user.schema';
import { S3Service } from '../s3/s3.service';

@Injectable()
export class UserProfileService {
  constructor(
    @InjectModel(User.name) private userModel: Model<User>,
    private s3Service: S3Service,
  ) {}

  async updateAvatar(userId: string, file: Express.Multer.File) {
    // Upload ke S3
    const { key } = await this.s3Service.uploadFile(file, 'avatars');

    // Simpan key di database
    await this.userModel.findByIdAndUpdate(userId, { avatarKey: key });

    return { avatarKey: key };
  }

  async getAvatarUrl(userId: string): Promise<string | null> {
    const user = await this.userModel.findById(userId).select('avatarKey');
    if (!user?.avatarKey) return null;

    // Generate signed URL setiap kali diakses
    return this.s3Service.getSignedUrl(user.avatarKey, 3600);
  }
}
```

### 7. .env Contoh

```env
AWS_REGION=ap-southeast-1
AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
AWS_S3_BUCKET=my-app-uploads
```

---

## Dipakai untuk Apa?

- **Upload avatar/foto profil** (production)
- **Upload dokumen** (KTP, ijazah, kontrak)
- **Upload gambar produk** — e-commerce
- **File sharing** — cloud storage aplikasi
- **Backup file** — database dump, log file

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| **Credentials tidak valid** | Set environment variable `AWS_ACCESS_KEY_ID` dan `AWS_SECRET_ACCESS_KEY` dengan benar |
| **Bucket belum dibuat** | Buat bucket dulu via AWS Console atau SDK |
| **File terlalu besar** | Batasi di Multer (`limits.fileSize`) dan di S3 (bucket policy) |
| **Signed URL expired** | Set `expiresIn` yang sesuai (default 3600 detik = 1 jam) |
| **CORS error saat akses langsung** | Konfigurasi CORS di bucket S3 |
| **File tidak bisa diakses publik** | Pakai signed URL atau set ACL public-read |
| **Lupa inject S3Service** | Pastikan S3Module diimport di module yang memakai |

---

## Soal Latihan

### Soal 1: Implementasikan S3Service

Buatlah:

1. `S3Module` global dengan S3Client
2. `S3Service` dengan method `uploadFile`, `deleteFile`, `getSignedUrl`
3. Controller `POST /profile/avatar` yang upload file ke S3 folder `avatars` dan simpan key ke database (asumsikan ada `UserModel`)

```typescript
// ========= JAWABAN =========

// s3.module.ts
import { Module, Global } from '@nestjs/common';
import { S3Service } from './s3.service';
import { S3Client } from '@aws-sdk/client-s3';

@Global()
@Module({
  providers: [
    {
      provide: 'S3_CLIENT',
      useFactory: () => {
        return new S3Client({
          region: process.env.AWS_REGION,
          credentials: {
            accessKeyId: process.env.AWS_ACCESS_KEY_ID,
            secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
          },
        });
      },
    },
    S3Service,
  ],
  exports: [S3Service],
})
export class S3Module {}

// s3.service.ts
import { Injectable, Inject } from '@nestjs/common';
import { S3Client, PutObjectCommand, DeleteObjectCommand, GetObjectCommand } from '@aws-sdk/client-s3';
import { getSignedUrl } from '@aws-sdk/s3-request-presigner';
import { v4 as uuidv4 } from 'uuid';
import { extname } from 'path';

@Injectable()
export class S3Service {
  constructor(@Inject('S3_CLIENT') private s3: S3Client) {}

  private bucket = process.env.AWS_S3_BUCKET;

  async uploadFile(file: Express.Multer.File, folder: string) {
    const key = `${folder}/${uuidv4()}${extname(file.originalname)}`;
    await this.s3.send(new PutObjectCommand({
      Bucket: this.bucket, Key: key, Body: file.buffer, ContentType: file.mimetype,
    }));
    return { key };
  }

  async deleteFile(key: string) {
    await this.s3.send(new DeleteObjectCommand({ Bucket: this.bucket, Key: key }));
  }

  async getSignedUrl(key: string, expiresIn = 3600) {
    return getSignedUrl(this.s3, new GetObjectCommand({ Bucket: this.bucket, Key: key }), { expiresIn });
  }
}

// profile-avatar.controller.ts
import { Controller, Post, UseInterceptors, UploadedFile, UseGuards } from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';
import { S3Service } from './s3.service';
import { AuthGuard } from '@nestjs/passport';
import { User as UserDecorator } from '../user/user.decorator';

@Controller('profile')
export class ProfileAvatarController {
  constructor(private s3Service: S3Service, private userService: UserProfileService) {}

  @Post('avatar')
  @UseGuards(AuthGuard('jwt'))
  @UseInterceptors(FileInterceptor('avatar', {
    fileFilter: (req, file, cb) => {
      cb(null, /jpeg|jpg|png|webp/.test(file.mimetype));
    },
    limits: { fileSize: 2 * 1024 * 1024 },
  }))
  async uploadAvatar(@UploadedFile() file: Express.Multer.File, @UserDecorator() user: any) {
    const { key } = await this.s3Service.uploadFile(file, 'avatars');
    await this.userService.updateAvatarKey(user.sub, key);
    return { key };
  }
}
```
