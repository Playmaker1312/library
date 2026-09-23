# 11 - Custom Provider - useValue, useClass, useFactory & useExisting

## Penjelasan

Setelah memahami DI dasar, kamu mungkin bertanya: "Bagaimana kalau provider tidak bisa dibuat dengan `@Injectable()` biasa? Bagaimana kalau nilainya statis? Atau perlu konfigurasi async sebelum dipakai?"

Jawabannya: **Custom Provider**.

Selain deklarasi provider standar (class → otomatis di-instantiate oleh NestJS), kita bisa mendaftarkan provider secara eksplisit dengan 4 tipe custom provider: `useValue`, `useClass`, `useFactory`, dan `useExisting`.

Ini mirip seperti **gudang alat pusat** yang tidak hanya menyediakan alat jadi (class), tapi juga:
- Barang jadi dari toko (`useValue`)
- Alat yang dibuat pabrik berbeda (`useClass`)
- Alat yang dirakit langsung di gudang (`useFactory`)
- Alias — "kalau minta bor, kasih aja gerinda" (`useExisting`)

## Fungsi

- Mendaftarkan **nilai statis** atau objek sebagai provider
- Memilih **implementasi berbeda** berdasarkan kondisi (development vs production)
- Membuat provider yang membutuhkan **inisialisasi async** (koneksi database, load config)
- Membuat **alias** untuk provider yang sudah ada
- Menggunakan **token non-class** (string atau symbol) untuk inject

## Cara Pengimplementasian / Code

### 1. useValue — Nilai Statis

Cocok untuk objek konfigurasi, nilai constant, mock testing.

```typescript
// config/config.provider.ts
export const DATABASE_CONFIG = 'DATABASE_CONFIG';

export const databaseConfigProvider = {
  provide: DATABASE_CONFIG,
  useValue: {
    host: 'localhost',
    port: 5432,
    database: 'myapp',
    username: 'admin',
    password: 'secret',
  },
};
```

```typescript
// Menggunakan useValue provider
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class DatabaseService {
  constructor(
    @Inject(DATABASE_CONFIG) private readonly config: Record<string, any>,
  ) {
    console.log(`Connecting to ${config.host}:${config.port}`);
  }
}
```

```typescript
// Mendaftarkan di module
@Module({
  providers: [databaseConfigProvider, DatabaseService],
})
export class DatabaseModule {}
```

### 2. useClass — Implementasi Berganti

Cocok untuk strategy pattern — ganti implementasi berdasarkan environment.

```typescript
// interfaces/storage.interface.ts
export interface StorageService {
  upload(file: Buffer, path: string): Promise<string>;
}

// providers/local-storage.service.ts
export class LocalStorageService implements StorageService {
  async upload(file: Buffer, path: string): Promise<string> {
    // Simpan ke disk lokal
    return `/uploads/${path}`;
  }
}

// providers/s3-storage.service.ts
export class S3StorageService implements StorageService {
  async upload(file: Buffer, path: string): Promise<string> {
    // Upload ke AWS S3
    return `https://s3.amazonaws.com/${path}`;
  }
}
```

```typescript
// providers/storage.provider.ts
import { Provider } from '@nestjs/common';

export const StorageProvider: Provider = {
  provide: 'STORAGE_SERVICE',
  useClass: process.env.NODE_ENV === 'production'
    ? S3StorageService  // Production pakai S3
    : LocalStorageService,  // Development pakai local
};
```

### 3. useFactory — Provider Factory (Async/Synchronous)

Cocok untuk provider yang butuh inisialisasi kompleks atau async.

```typescript
// config/config-factory.provider.ts
import { ConfigService } from './config.service';

export const configFactoryProvider = {
  provide: 'APP_CONFIG',
  useFactory: async (configService: ConfigService) => {
    // Load konfigurasi — bisa async
    const env = await configService.loadEnvironmentConfig();
    const secrets = await configService.loadSecrets();

    return {
      ...env,
      ...secrets,
      appName: 'My NestJS App',
      isProduction: process.env.NODE_ENV === 'production',
    };
  },
  inject: [ConfigService],  // Dependencies untuk factory function
};
```

```typescript
// Mendaftarkan di module
@Module({
  providers: [ConfigService, configFactoryProvider],
  exports: ['APP_CONFIG'],
})
export class ConfigModule {}
```

**Contoh useFactory dengan lebih banyak dependensi:**

```typescript
@Module({
  providers: [
    {
      provide: 'DATABASE_CONNECTION',
      useFactory: async (
        config: Record<string, any>,
        logger: LoggerService,
      ) => {
        logger.log('Initializing database connection...');
        const connection = await createConnection(config);
        logger.log('Database connected');
        return connection;
      },
      inject: ['APP_CONFIG', LoggerService],
    },
  ],
})
export class DatabaseModule {}
```

### 4. useExisting — Alias

Provider A adalah alias dari Provider B — keduanya merujuk instance yang sama.

```typescript
@Injectable()
export class LoggerService {
  log(message: string) {
    console.log(message);
  }
}

// Alias — LoggerAliasService dan LoggerService adalah objek yang SAMA
const loggerAliasProvider = {
  provide: 'LOGGER_ALIAS',
  useExisting: LoggerService,
};
```

```typescript
@Module({
  providers: [
    LoggerService,
    loggerAliasProvider,
    {
      provide: 'ANOTHER_ALIAS',
      useExisting: LoggerService,  // Instance yang sama lagi
    },
  ],
})
export class AppModule {}
```

### Full Custom Provider dengan @Inject() Token

```typescript
// Semua tipe custom provider dalam satu module
@Module({
  providers: [
    // Standard class provider
    UsersService,

    // useValue — nilai statis
    { provide: 'MAX_RETRY', useValue: 3 },

    // useClass — implementasi berbeda
    { provide: 'CACHE_SERVICE', useClass: RedisCacheService },

    // useFactory — async initialization
    {
      provide: 'CONFIG',
      useFactory: async () => {
        const config = await loadFromFile();
        return config;
      },
      inject: [],
    },

    // useExisting — alias
    { provide: 'LOGGER', useExisting: LoggerService },
  ],
})
export class AppModule {}
```

## Analogi (Gedung Bertingkat)

Gudang alat pusat (DI Container) punya 4 cara menyediakan alat:

| Jenis | Analogi |
|-------|---------|
| **useValue** | "Saya butuh palu" → Langsung kasih palu yang sudah jadi dari toko. Tidak perlu dirakit, tinggal pakai. |
| **useClass** | "Saya butuh bor" → Tergantung situasi: kalau lagi di lantai 2 kasih bor listrik, kalau di lantai 5 kasih bor baterai (implementasi berbeda, interface sama). |
| **useFactory** | "Saya butuh mesin kopi" → Tukang di gudang merakit mesin kopi dari komponen-komponen yang ada (pemanas, wadah, pompa). Bisa async — "tunggu 5 menit, sedang dirakit." |
| **useExisting** | "Saya butuh obeng" → "Obeng itu ada di laci yang sama dengan alat nomor #123" — alias, dua nama benda yang sama. |

## Dipakai Untuk Apa

- **Konfigurasi**: useValue untuk config object, useFactory untuk config yang perlu async loading
- **Feature flags / A/B testing**: useClass untuk memilih implementasi fitur
- **Third-party library wrapper**: useFactory untuk inisialisasi koneksi Redis/MongoDB
- **Testing**: useValue untuk mock object di unit test
- **Alias/shortcut**: useExisting untuk backward compatibility atau naming convenience

## Kesalahan Umum

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| Lupa `inject` di useFactory | Factory menerima parameter kosong (undefined) | Tambahkan `inject` array sesuai parameter factory |
| useFactory async tapi tidak di-await di provider lain | Mendapatkan Promise, bukan nilai | NestJS handle async factory secara internal — aman |
| Alias circular (A → B, B → A) | Error infinite loop | Hindari circular alias |
| `provide` pakai string, `@Inject()` pakai string beda | Provider tidak ketemu | Pastikan token string konsisten |
| useValue untuk objek yang mutable dan dipakai banyak tempat | Side effect tidak terduga | Gunakan `Object.freeze()` jika perlu immutable |

## Soal Latihan & Jawaban

### Soal 1
Buatlah ConfigProvider yang menggunakan `useFactory` untuk membaca file konfigurasi dari environment variable `CONFIG_PATH` (default: `config.json`). Provider harus:
- Membaca file JSON secara async
- Me-parse JSON
- Menyediakan nilai `database.host` dan `database.port`

**Jawaban:**

```typescript
// config.provider.ts
import * as fs from 'fs/promises';
import * as path from 'path';

export interface AppConfig {
  database: {
    host: string;
    port: number;
  };
}

export const CONFIG_TOKEN = 'APP_CONFIG';

export const configProvider = {
  provide: CONFIG_TOKEN,
  useFactory: async (): Promise<AppConfig> => {
    const configPath = process.env.CONFIG_PATH || 'config.json';
    const fullPath = path.resolve(configPath);

    try {
      const raw = await fs.readFile(fullPath, 'utf-8');
      const config: AppConfig = JSON.parse(raw);

      return {
        database: {
          host: config.database?.host || 'localhost',
          port: config.database?.port || 5432,
        },
      };
    } catch (error) {
      // Fallback default config
      console.warn(`Config file not found at ${fullPath}, using defaults`);
      return {
        database: {
          host: 'localhost',
          port: 5432,
        },
      };
    }
  },
  inject: [],  // Tidak ada dependency
};

// app.module.ts
@Module({
  providers: [configProvider],
  exports: [CONFIG_TOKEN],
})
export class ConfigModule {}

// database.service.ts
@Injectable()
export class DatabaseService {
  constructor(@Inject(CONFIG_TOKEN) private readonly config: AppConfig) {
    console.log(`DB Config: ${config.database.host}:${config.database.port}`);
  }
}
```

### Soal 2
Apa perbedaan `useClass` dan `useExisting`?

**Jawaban:**
- `useClass`: Setiap kali provider di-inject, NestJS membuat **instance baru** dari class yang ditentukan (kecuali singleton). Provider dan class target adalah instance berbeda.
- `useExisting`: Provider adalah **reference alias** ke provider lain. Keduanya merujuk ke **instance yang sama persis**. Jika provider A `useExisting: B`, maka A dan B adalah objek yang identik.

### Soal 3
Kapan kita menggunakan `useFactory` dibanding `useValue`?

**Jawaban:**
`useValue` untuk nilai yang sudah diketahui saat kompilasi (statis, hardcoded, atau synchronous). `useFactory` untuk nilai yang membutuhkan **inisialisasi kompleks**: async operations (read file, network call), conditional logic, atau membutuhkan dependency injection di dalam factory function-nya.
