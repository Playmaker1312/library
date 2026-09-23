# 54 - Multiple Environment - Development Staging Production

---

## Penjelasan

Setelah tahu cara setup ConfigModule dengan validasi Joi, sekarang kita perlu mengelola konfigurasi untuk environment yang berbeda. Kode development tidak bisa pakai database production, secret key harus berbeda, dan log level harus diatur. NestJS mendukung multiple `.env` file dengan `envFilePath` array, dan kita bisa memilih file mana yang dimuat berdasarkan `NODE_ENV`.

---

## Fungsi

- Memisahkan konfigurasi development, staging, dan production
- Mengelola secret secara aman (tidak di git)
- Mengubah perilaku aplikasi (log, database, cache) berdasarkan environment
- Integrasi dengan vault / secret management service

---

## Cara Pengimplementasian

### 1. Struktur file env

```
project/
├── .env                        # fallback / shared
├── .env.development            # dev specific
├── .env.staging                # staging specific
├── .env.production             # production specific
├── .env.example                # template (boleh di git)
└── src/
    └── app.module.ts
```

### 2. Isi file `.env.*`

```bash
# .env.development
NODE_ENV=development
PORT=3000
DATABASE_URL=postgresql://dev:dev@localhost:5432/dev_db
JWT_SECRET=dev-secret-key-12345678901234567890
JWT_REFRESH_SECRET=dev-refresh-secret-1234567890
LOG_LEVEL=debug
CORS_ORIGIN=http://localhost:5173
```

```bash
# .env.production
NODE_ENV=production
PORT=8080
DATABASE_URL=postgresql://prod:${DB_PASSWORD}@prod-host:5432/prod_db
JWT_SECRET=${JWT_SECRET}
JWT_REFRESH_SECRET=${JWT_REFRESH_SECRET}
LOG_LEVEL=warn
CORS_ORIGIN=https://app.example.com
```

### 3. Load env file berdasarkan NODE_ENV

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';
import * as Joi from 'joi';

const envFilePath = [
  `.env.${process.env.NODE_ENV || 'development'}`,
  '.env',
];

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      envFilePath, // Array: file pertama yang ditemukan akan dipakai
      validationSchema: Joi.object({
        NODE_ENV: Joi.string()
          .valid('development', 'staging', 'production')
          .default('development'),
        PORT: Joi.number().default(3000),
        DATABASE_URL: Joi.string().required(),
        JWT_SECRET: Joi.string().required().min(32),
        JWT_REFRESH_SECRET: Joi.string().required().min(32),
        LOG_LEVEL: Joi.string()
          .valid('debug', 'info', 'warn', 'error')
          .default('info'),
      }),
    }),
  ],
})
export class AppModule {}
```

### 4. Script package.json

```json
{
  "scripts": {
    "start:dev": "cross-env NODE_ENV=development nest start --watch",
    "start:staging": "cross-env NODE_ENV=staging node dist/main",
    "start:prod": "cross-env NODE_ENV=production node dist/main",
    "build": "nest build"
  }
}
```

### 5. Secret management — vault / 1Password / AWS Secrets Manager

Untuk production, **jangan simpan secret di file .env** yang dibundel. Gunakan vault:

```typescript
// config/vault-loader.ts
import { SecretsManager } from '@aws-sdk/client-secrets-manager';

export async function loadSecretsFromVault(): Promise<Record<string, string>> {
  if (process.env.NODE_ENV !== 'production') {
    return {}; // dev cukup pakai .env
  }

  const client = new SecretsManager({ region: 'us-east-1' });
  const secret = await client.getSecretValue({ SecretId: 'prod/nestjs-app' });
  return JSON.parse(secret.SecretString || '{}');
}
```

```typescript
// app.module.ts — load dinamik
import { ConfigModule } from '@nestjs/config';
import { loadSecretsFromVault } from './config/vault-loader';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      load: [loadSecretsFromVault], // async loader
    }),
  ],
})
export class AppModule {}
```

### 6. File yang tidak boleh di git

```gitignore
# .gitignore
.env
.env.development
.env.staging
.env.production
*.local.env

# hanya template yang boleh di git
!.env.example
```

---

## Analogi

Gedung bertingkat punya lantai yang berbeda fungsi. Lantai 1-2 untuk **development** (kantor desain, banyak coretan), lantai 3-4 untuk **staging** (showroom, mirip asli), lantai 5+ untuk **production** (kantor utama, ketat). Masing-masing lantai punya **kunci akses (secret)** yang berbeda. Tidak mungkin kunci lantai 1 bisa buka brankas lantai 5. File `.env` adalah **papan petunjuk** yang ditempel di masing-masing lantai — tidak boleh dibawa keluar gedung.

---

## Dipakai Untuk Apa

- Menggunakan database terpisah di setiap environment
- Log level: debug di dev, warn/error di prod
- CORS origin berbeda tiap environment
- API key dan secret yang berbeda
- SSL/TLS config (development pakai self-signed, prod pakai valid)
- Rate limiter: longgar di dev, ketat di prod

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| .env production ikut ter-commit | Tambahkan ke `.gitignore` dan gunakan `.env.example` |
| Meng-hardcode `NODE_ENV` di kode | Ambil dari `ConfigService.get('NODE_ENV')` |
| Satu file env untuk semua environment | Pisahkan dengan `envFilePath` array |
| Secret di .env production masih plaintext | Gunakan vault / environment variable OS |
| Lupa validasi Joi untuk tiap environment | Buat validasi yang strict, `.required()` di prod |

---

## Soal Latihan

### Soal 1
Buat konfigurasi yang memuat `.env.development` saat `NODE_ENV=development` dan `.env.production` saat `NODE_ENV=production`. Set `isGlobal: true`.

### Jawaban 1
```typescript
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';
import * as Joi from 'joi';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      envFilePath: [`.env.${process.env.NODE_ENV || 'development'}`, '.env'],
      validationSchema: Joi.object({
        NODE_ENV: Joi.string().valid('development', 'production').default('development'),
        PORT: Joi.number().default(3000),
        DATABASE_URL: Joi.string().required(),
      }),
    }),
  ],
})
export class AppModule {}
```

### Soal 2
Buat script npm `start:dev` dan `start:prod` dengan cross-env.

### Jawaban 2
```json
{
  "scripts": {
    "start:dev": "cross-env NODE_ENV=development nest start --watch",
    "start:prod": "cross-env NODE_ENV=production node dist/main"
  }
}
```

### Soal 3
Sebutkan 3 file yang HARUS masuk `.gitignore` dan 1 file yang BOLEH di git.

### Jawaban 3
- **WAJIB di .gitignore:** `.env`, `.env.development`, `.env.production`
- **BOLEH di git:** `.env.example` (template tanpa nilai sensitif)
