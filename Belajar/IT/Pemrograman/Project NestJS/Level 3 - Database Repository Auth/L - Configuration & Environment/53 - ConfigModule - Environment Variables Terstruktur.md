# 53 - ConfigModule - Environment Variables Terstruktur

---

## Penjelasan

Di level sebelumnya kita sudah membuat service, controller, dan modul-modul bisnis seperti BlogModule dan UserModule. Semua konfigurasi masih hardcode — koneksi database, JWT secret, dan port server ditulis langsung di kode. NestJS menyediakan `@nestjs/config` untuk mengelola environment variables secara terstruktur. Module ini membungkus `dotenv` dan memberikan `ConfigService` yang bisa diinjeksi ke mana saja.

---

## Fungsi

- Membaca file `.env` dan memparse nilainya
- Menyediakan `ConfigService` global untuk akses konfigurasi di seluruh aplikasi
- Validasi environment variables dengan Joi (zod juga bisa)
- Namespace & typed configuration agar type-safe
- Memisahkan konfigurasi dari logika bisnis

---

## Cara Pengimplementasian

### 1. Install package

```bash
npm install @nestjs/config joi
```

### 2. Setup ConfigModule global

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';
import * as Joi from 'joi';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true, // ConfigService tersedia di semua module
      envFilePath: '.env',
      validationSchema: Joi.object({
        NODE_ENV: Joi.string()
          .valid('development', 'production', 'test')
          .default('development'),
        PORT: Joi.number().default(3000),
        DATABASE_URL: Joi.string().required(),
        JWT_SECRET: Joi.string().required().min(32),
        JWT_REFRESH_SECRET: Joi.string().required().min(32),
      }),
      validationOptions: {
        abortEarly: true,
      },
    }),
  ],
})
export class AppModule {}
```

### 3. ConfigService di service

```typescript
// config/database.config.ts
import { registerAs } from '@nestjs/config';

export default registerAs('database', () => ({
  url: process.env.DATABASE_URL,
  host: process.env.DB_HOST || 'localhost',
  port: Number(process.env.DB_PORT) || 5432,
  username: process.env.DB_USERNAME,
  password: process.env.DB_PASSWORD,
}));
```

```typescript
// config/jwt.config.ts
import { registerAs } from '@nestjs/config';

export default registerAs('jwt', () => ({
  secret: process.env.JWT_SECRET,
  refreshSecret: process.env.JWT_REFRESH_SECRET,
  accessExpiresIn: '15m',
  refreshExpiresIn: '7d',
}));
```

### 4. Menggunakan namespace config

```typescript
import { Injectable } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';

@Injectable()
export class DatabaseService {
  constructor(private configService: ConfigService) {
    // Akses tanpa namespace
    const dbUrl = this.configService.get<string>('DATABASE_URL');

    // Akses dengan namespace (typed)
    const dbConfig = this.configService.get('database');
    console.log(dbConfig.host); // typed sebagai any
  }
}
```

### 5. Typed configuration (rekomendasi)

```typescript
// config/configuration.ts
export interface DatabaseConfig {
  url: string;
  host: string;
  port: number;
}

export interface JwtConfig {
  secret: string;
  refreshSecret: string;
  accessExpiresIn: string;
  refreshExpiresIn: string;
}

export interface AppConfig {
  nodeEnv: string;
  port: number;
}

export default () => ({
  app: {
    nodeEnv: process.env.NODE_ENV || 'development',
    port: parseInt(process.env.PORT, 10) || 3000,
  },
  database: {
    url: process.env.DATABASE_URL,
    host: process.env.DB_HOST || 'localhost',
    port: parseInt(process.env.DB_PORT, 10) || 5432,
  },
  jwt: {
    secret: process.env.JWT_SECRET,
    refreshSecret: process.env.JWT_REFRESH_SECRET,
    accessExpiresIn: '15m',
    refreshExpiresIn: '7d',
  },
});
```

---

## Analogi

Membangun gedung bertingkat tanpa cetak biru (blueprint) yang jelas akan kacau. `ConfigModule` adalah **lemari arsip pusat** di lobi gedung. Setiap kontraktor (service) cukup datang ke lemari arsip untuk mengambil informasi seperti "berapa ukuran pintu?" atau "di mana letak panel listrik?" — tanpa harus mengebor tembok sendiri. Joi adalah **safety inspector** yang memastikan semua informasi di lemari arsip valid sebelum gedung dioperasikan.

---

## Dipakai Untuk Apa

- Menyimpan credential database, JWT secret, API key pihak ketiga
- Mengubah perilaku aplikasi berdasarkan `NODE_ENV`
- Konfigurasi port, CORS origin, rate limiter, mailer
- Memisahkan kode dari data sensitive

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Lupa `isGlobal: true` sehingga ConfigService tidak tersedia | Set `isGlobal: true` atau ekspor ConfigModule |
| Tidak pakai validation schema, typo di nama env | Pakai Joi schema dengan `.required()` |
| Menyimpan .env di git (kebocoran secret) | Tambahkan `.env` ke `.gitignore` |
| Parse number manual tanpa fallback | Pakai `parseInt()` dengan default value |
| Baca variable di file statis (`` dotenv`` saja) | Gunakan `ConfigService` agar testable |

---

## Soal Latihan

### Soal 1
Buat `ConfigModule` yang:
- Global
- Membaca file `.env`
- Validasi Joi untuk `PORT` (number, default 3000), `NODE_ENV` (development/production), `APP_NAME` (string required)

### Jawaban 1
```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';
import * as Joi from 'joi';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      envFilePath: '.env',
      validationSchema: Joi.object({
        NODE_ENV: Joi.string()
          .valid('development', 'production')
          .default('development'),
        PORT: Joi.number().default(3000),
        APP_NAME: Joi.string().required(),
      }),
    }),
  ],
})
export class AppModule {}
```

### Soal 2
Buat namespace config `registerAs('mail', ...)` untuk konfigurasi SMTP email (host, port, user, password).

### Jawaban 2
```typescript
// config/mail.config.ts
import { registerAs } from '@nestjs/config';

export default registerAs('mail', () => ({
  host: process.env.MAIL_HOST || 'smtp.gmail.com',
  port: parseInt(process.env.MAIL_PORT, 10) || 587,
  user: process.env.MAIL_USER,
  password: process.env.MAIL_PASSWORD,
  from: process.env.MAIL_FROM || 'noreply@example.com',
}));
```

### Soal 3
Apa perbedaan `ConfigService.get('key')` dengan `@Inject(config.KEY)`?

### Jawaban 3
`ConfigService.get('key')` mengembalikan value dengan tipe `any`, sedangkan `@Inject(config.KEY)` menggunakan injection token dari `registerAs` sehingga bisa dikombinasikan dengan typed interface. Yang kedua lebih aman secara type.
