# 78 - File Upload dengan Multer - Single & Multiple Files

---

## Penjelasan (Keterkaitan dengan Materi Sebelumnya)

Sejauh ini kita hanya berurusan dengan **data JSON/string** — request body, query params, JWT payload. Tapi aplikasi nyata butuh **upload file**: avatar, foto profil, dokumen, dll.

NestJS menggunakan **Multer** (middleware Express untuk file upload) yang sudah terbundel di `@nestjs/platform-express`. Kita akan belajar:

- Upload **satu file** (`FileInterceptor`)
- Upload **banyak file** (`FilesInterceptor`)
- **Validasi file** (tipe & ukuran)

**Kaitannya dengan materi sebelumnya:**
- **Module + Controller** → endpoint REST
- **DTO + Validation** → validasi data, termasuk file
- **Service** → logic untuk memproses file setelah upload

---

## Fungsi

| Fungsi | Penjelasan |
|--------|------------|
| **FileInterceptor** | Interceptor untuk menangkap **satu file** dari request |
| **FilesInterceptor** | Interceptor untuk menangkap **banyak file** sekaligus |
| **@UploadedFile()** | Decorator untuk extract file yang sudah di-intercept |
| **@UploadedFiles()** | Decorator untuk extract array file (multiple) |
| **fileFilter** | Validasi tipe file (misal hanya jpg/png) |
| **limits** | Batas ukuran file |

---

## Analogi: Pos Paket di Gedung Bertingkat

Bayangkan gedung bertingkat punya **pos paket** di lobby:

- **Multer** = Satpam pos paket yang **menerima kiriman** dari kurir.
- **FileInterceptor** = Satpam yang menerima **satu paket** dari satu kurir.
- **FilesInterceptor** = Satpam yang menerima **banyak paket** dari satu kurir.
- **@UploadedFile()** = Tanda terima untuk **satu paket** — "Ini paketnya, Pak."
- **@UploadedFiles()** = Tanda terima untuk **semua paket** — "Ini semua paketnya, Pak."
- **fileFilter** = Aturan pos: "Hanya terima amplop, tidak terima kardus" — filter tipe file.
- **limits** = "Maksimal paket 2 kg" — batas ukuran file.

Setelah satpam menerima paket, dia serahkan ke **bagian terkait (Service)** — misal bagian arsip untuk disimpan.

---

## Cara Pengimplementasian (Code)

### 1. Install Dependencies

```bash
npm install @nestjs/platform-express
```

(Platform express sudah include Multer secara default di NestJS)

### 2. Upload Avatar (Single File) — Endpoint

```typescript
// avatar.controller.ts
import {
  Controller,
  Post,
  UseInterceptors,
  UploadedFile,
  BadRequestException,
} from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';
import { diskStorage } from 'multer';
import { extname, join } from 'path';

@Controller('avatar')
export class AvatarController {
  @Post('upload')
  @UseInterceptors(
    FileInterceptor('file', {
      storage: diskStorage({
        destination: join(__dirname, '..', '..', 'uploads', 'avatars'),
        filename: (req, file, callback) => {
          // Format: avatar-{timestamp}-{random}.ext
          const uniqueSuffix = Date.now() + '-' + Math.round(Math.random() * 1e9);
          const ext = extname(file.originalname);
          callback(null, `avatar-${uniqueSuffix}${ext}`);
        },
      }),
      fileFilter: (req, file, callback) => {
        // Hanya izinkan jpg, jpeg, png, webp
        const allowedTypes = /jpeg|jpg|png|webp/;
        const extName = allowedTypes.test(extname(file.originalname).toLowerCase());
        const mimeType = allowedTypes.test(file.mimetype);

        if (extName && mimeType) {
          callback(null, true);
        } else {
          callback(new BadRequestException('Hanya file gambar (jpg/png/webp) yang diizinkan'), false);
        }
      },
      limits: {
        fileSize: 2 * 1024 * 1024, // 2 MB
      },
    }),
  )
  uploadAvatar(@UploadedFile() file: Express.Multer.File) {
    if (!file) {
      throw new BadRequestException('File tidak ditemukan');
    }

    return {
      message: 'Avatar berhasil diupload',
      originalName: file.originalname,
      filename: file.filename,
      path: file.path,
      size: file.size,
      mimetype: file.mimetype,
    };
  }
}
```

### 3. Upload Multiple Files (FilesInterceptor)

```typescript
// photos.controller.ts
import {
  Controller,
  Post,
  UseInterceptors,
  UploadedFiles,
  BadRequestException,
} from '@nestjs/common';
import { FilesInterceptor } from '@nestjs/platform-express';
import { diskStorage } from 'multer';
import { extname, join } from 'path';

@Controller('photos')
export class PhotosController {
  @Post('upload')
  @UseInterceptors(
    FilesInterceptor('photos', 5, { // maksimal 5 file
      storage: diskStorage({
        destination: join(__dirname, '..', '..', 'uploads', 'photos'),
        filename: (req, file, callback) => {
          const uniqueSuffix = Date.now() + '-' + Math.round(Math.random() * 1e9);
          const ext = extname(file.originalname);
          callback(null, `photo-${uniqueSuffix}${ext}`);
        },
      }),
      fileFilter: (req, file, callback) => {
        if (!file.mimetype.match(/^image\/(jpeg|png|webp)$/)) {
          callback(new BadRequestException('Hanya file gambar'), false);
        }
        callback(null, true);
      },
      limits: { fileSize: 5 * 1024 * 1024 }, // 5 MB per file
    }),
  )
  uploadPhotos(@UploadedFiles() files: Array<Express.Multer.File>) {
    if (!files || files.length === 0) {
      throw new BadRequestException('Tidak ada file yang diupload');
    }

    return {
      message: `${files.length} foto berhasil diupload`,
      files: files.map((f) => ({
        filename: f.filename,
        size: f.size,
      })),
    };
  }
}
```

### 4. Module — Daftarkan Controller

```typescript
// upload.module.ts
import { Module } from '@nestjs/common';
import { AvatarController } from './avatar.controller';
import { PhotosController } from './photos.controller';

@Module({
  controllers: [AvatarController],
})
export class UploadModule {}
```

### 5. Serve Static Files (Akses file yang diupload)

```typescript
// main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { join } from 'path';
import * as express from 'express';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Serve folder uploads sebagai static files
  app.use('/uploads', express.static(join(__dirname, '..', 'uploads')));

  await app.listen(3000);
}
```

Setelah ini, file bisa diakses via: `http://localhost:3000/uploads/avatars/avatar-1712345678-123.jpg`

### 6. Test dengan Postman / cURL

```bash
# Single file
curl -X POST http://localhost:3000/avatar/upload \
  -F "file=@/path/to/avatar.jpg"

# Multiple files
curl -X POST http://localhost:3000/photos/upload \
  -F "photos=@/path/to/photo1.jpg" \
  -F "photos=@/path/to/photo2.jpg"
```

---

## Dipakai untuk Apa?

- **Upload avatar/foto profil**
- **Upload dokumen** (KTP, ijazah, dll)
- **Upload gambar produk** di e-commerce
- **Upload lampiran** di form/email
- **Bulk upload foto** di galeri

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| **File terlalu besar** | Set `limits.fileSize` dan handle error di global filter |
| **Tipe file tidak sesuai** | Gunakan `fileFilter` untuk validasi mime type |
| **Lupa membuat folder upload** | Buat folder `uploads/avatars` dulu atau gunakan `fs.mkdirSync` |
| **FileInterceptor tidak bekerja** | Pastikan `@UseInterceptors` ditempatkan dengan benar |
| **Form field name salah** | Nama field di `FileInterceptor('file')` harus cocok dengan key di form-data |
| **File tidak persist setelah restart** | Disk storage hanya untuk development; production pakai S3 |

---

## Soal Latihan

### Soal 1: Endpoint Upload Avatar

Buatlah endpoint `POST /avatar/upload` yang:

1. Menerima file dengan field name `avatar`
2. Hanya menerima **jpg, png, webp** — max **2MB**
3. Simpan dengan nama unik `avatar-{timestamp}-{userId}.ext`
4. Kembalikan URL file yang bisa diakses

```typescript
// ========= JAWABAN =========

// avatar.controller.ts
import {
  Controller,
  Post,
  UseInterceptors,
  UploadedFile,
  BadRequestException,
  Req,
} from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';
import { diskStorage } from 'multer';
import { extname, join } from 'path';
import { Request } from 'express';

@Controller('avatar')
export class AvatarController {
  @Post('upload')
  @UseInterceptors(
    FileInterceptor('avatar', {
      storage: diskStorage({
        destination: join(__dirname, '..', '..', 'uploads', 'avatars'),
        filename: (req: Request, file, cb) => {
          const userId = (req as any).user?.sub || 'anonymous';
          const ext = extname(file.originalname);
          cb(null, `avatar-${Date.now()}-${userId}${ext}`);
        },
      }),
      fileFilter: (req, file, cb) => {
        const allowed = /jpeg|jpg|png|webp/;
        const isValid = allowed.test(extname(file.originalname).toLowerCase())
          && allowed.test(file.mimetype);
        isValid ? cb(null, true) : cb(new BadRequestException('Format tidak didukung'), false);
      },
      limits: { fileSize: 2 * 1024 * 1024 },
    }),
  )
  uploadAvatar(@UploadedFile() file: Express.Multer.File) {
    if (!file) throw new BadRequestException('File diperlukan');
    return {
      url: `/uploads/avatars/${file.filename}`,
      filename: file.filename,
    };
  }
}
```
