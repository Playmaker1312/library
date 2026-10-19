# 9 - Module System - Unit Organisasi Kode dalam NestJS

## Penjelasan

Setelah memahami arsitektur NestJS secara keseluruhan, saatnya kita bedah komponen pertama dan terpenting: **Module**. Module adalah fondasi organisasi kode di NestJS.

Jika NestJS adalah **gedung bertingkat**, maka Module adalah **setiap lantainya**. Lantai 1 untuk resepsionis, Lantai 2 untuk HRD, Lantai 3 untuk Keuangan — masing-masing lantai punya ruangan (controller), staf (provider), dan akses ke lantai lain (exports/imports).

Module juga berfungsi sebagai **bounded context** — batas tegas yang memisahkan satu domain fitur dengan domain lainnya. Ini mencegah spaghetti code di proyek besar.

## Fungsi

- Mengorganisir kode ke dalam **unit fungsional** yang kohesif
- Mengatur **dependency graph** antar fitur
- Mengontrol **scope visibility** — provider mana yang bisa dipakai module lain
- Memungkinkan **lazy loading** dan **dynamic modules** untuk optimasi

## Cara Pengimplementasian / Code

### Anatomi @Module() Decorator

```typescript
@Module({
  imports: [],       // Module lain yang dibutuhkan
  controllers: [],   // Controller di module ini
  providers: [],     // Service/Provider di module ini
  exports: [],       // Provider yang boleh dipakai module lain
})
export class SomeModule {}
```

### Root Module (AppModule)

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { UsersModule } from './users/users.module';
import { PostsModule } from './posts/posts.module';
import { DatabaseModule } from './database/database.module';

@Module({
  imports: [
    DatabaseModule,  // Module database — menyediakan koneksi
    UsersModule,     // Module user — fitur manajemen user
    PostsModule,     // Module post — fitur blog post
  ],
  controllers: [],   // Root module biasanya tidak punya controller
  providers: [],
})
export class AppModule {}
```

### Feature Module (UsersModule)

```typescript
// users/users.module.ts
import { Module } from '@nestjs/common';
import { UsersController } from './users.controller';
import { UsersService } from './users.service';
import { PrismaModule } from '../database/prisma.module';

@Module({
  imports: [PrismaModule],          // Butuh database
  controllers: [UsersController],    // Handle HTTP /users
  providers: [UsersService],        // Logic bisnis user
  exports: [UsersService],          // Module lain bisa pakai UsersService
})
export class UsersModule {}
```

### Shared Module

```typescript
// common/shared.module.ts
import { Module } from '@nestjs/common';
import { LoggerService } from './logger.service';
import { CacheService } from './cache.service';

@Module({
  providers: [LoggerService, CacheService],
  exports: [LoggerService, CacheService],  // Export agar dipakai module lain
})
export class SharedModule {}
```

Module lain tinggal import:
```typescript
@Module({
  imports: [SharedModule],  // Sekarang bisa inject LoggerService & CacheService
  providers: [OrdersService],
})
export class OrdersModule {}
```

### @Global() Module

Agar tidak perlu import terus-menerus, gunakan `@Global()` — HATI-HATI, ini membuat provider menjadi global scope.

```typescript
// common/global.module.ts
import { Global, Module } from '@nestjs/common';
import { LoggerService } from './logger.service';

@Global()  // LoggerService tersedia di semua module tanpa import
@Module({
  providers: [LoggerService],
  exports: [LoggerService],
})
export class GlobalModule {}
```

```typescript
// app.module.ts
@Module({
  imports: [GlobalModule],  // Cukup import sekali disini
})
export class AppModule {}
```

Sekarang `OrdersService` di `OrdersModule` bisa langsung inject `LoggerService` tanpa mengimport `GlobalModule`.

## Analogi (Gedung Bertingkat)

Module dalam NestJS seperti **lantai gedung perkantoran**:

- **AppModule (Root)** = Lobi utama gedung — pintu masuk yang menghubungkan semua lantai
- **Feature Module** = Lantai khusus — Lantai 2 untuk HRD, Lantai 3 untuk Finance
- **Shared Module** = Fasilitas bersama — Toilet, lift, tangga darurat yang bisa dipakai semua lantai
- **@Global()** = Lift utama — tersedia di lantai manapun tanpa perlu jalan ke lantai lain dulu

**Imports** = "Lantai HRD butuh akses ke ruang server di Lantai 1" → import DatabaseModule
**Exports** = "Lantai HRD menyediakan mesin fotokopi untuk dipakai lantai lain" → export CopierService
**Controllers** = "Resepsionis di lantai HRD yang menerima tamu"
**Providers** = "Staff HRD yang mengerjakan administrasi"

Dengan pembagian lantai yang jelas, pekerja di lantai Finance tidak perlu tahu detail ruangan HRD. Mereka cukup tahu "ada pintu yang menghubungkan" (exports).

## Dipakai Untuk Apa

- **Feature grouping**: UsersModule, PostsModule, PaymentsModule — setiap fitur punya module sendiri
- **Library/utility sharing**: SharedModule untuk kode yang dipakai banyak module
- **Third-party integration**: DatabaseModule, RedisModule, MailModule sebagai wrapper integrasi
- **Microservice boundaries**: Setiap microservice bisa diorganisir sebagai module terpisah
- **Lazy loading & Dynamic modules**: Module bisa dimuat sesuai kebutuhan

## Kesalahan Umum

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| Satu module raksasa berisi semua fitur | Module jadi tidak terawat, sulit di-test | Pisahkan per fitur (satu fitur = satu module) |
| Lupa export provider | Error `Nest can't resolve dependencies` | Cek exports di module |
| Terlalu banyak @Global() | Circular dependency, coupling tinggi | Gunakan @Global() hanya untuk utility sejati |
| Import module tapi tidak pakai provider apapun | Module ikut terdaftar tapi tidak berguna | Hapus import yang tidak diperlukan |
| Circular module (A import B, B import A) | Error runtime | Gunakan `forwardRef()` atau pikirkan ulang arsitektur |

## Soal Latihan & Jawaban

### Soal 1
Buatlah diagram dependency antar module untuk aplikasi blog dengan fitur berikut:
- **AuthModule** (login, register)
- **UsersModule** (manajemen user)
- **PostsModule** (CRUD post)
- **CommentsModule** (komentar pada post)
- **DatabaseModule** (koneksi database)
- **MailModule** (kirim email notifikasi)

Ketentuan dependency:
- AuthModule butuh UsersModule
- PostsModule butuh UsersModule dan DatabaseModule
- CommentsModule butuh PostsModule dan UsersModule
- AuthModule butuh MailModule untuk kirim email verifikasi

**Jawaban:**

```
                    ┌──────────────┐
                    │ DatabaseModule │
                    └──────┬───────┘
                           │
              ┌────────────┼────────────┐
              │            │            │
              ▼            ▼            ▼
        ┌──────────┐ ┌──────────┐ ┌──────────────┐
        │ UsersModule │ │ PostsModule │ │ CommentsModule │
        └─────┬────┘ └─────┬────┘ └──────┬───────┘
              │            │              │
              │            │              │
              ▼            ▼              │
        ┌──────────┐ ┌──────────┐         │
        │ MailModule │ │ UsersModule──────┘
        └─────┬────┘ │ (depends on Users)
              │      │
              │      └── depends on Database
              │
              ▼
        ┌──────────┐
        │ AuthModule │──── depends on UsersModule + MailModule
        └──────────┘
```

### Soal 2
Apa yang dimaksud dengan `exports` dalam `@Module()` dan mengapa penting?

**Jawaban:**
`exports` mendaftarkan provider mana dari module ini yang boleh digunakan oleh module lain. Tanpa exports, provider/provider tersebut bersifat private — hanya bisa dipakai di dalam module sendiri. Ini penting untuk **encapsulation**: module hanya mengekspos apa yang perlu, menyembunyikan detail internal.

### Soal 3
Kapan kita menggunakan `@Global()`? Sebutkan contoh kasus penggunaan yang tepat.

**Jawaban:**
`@Global()` digunakan untuk provider yang **semua module butuh akses ke sana**. Contoh tepat: LoggerService, ConfigService (environment variables), RedisCacheService. Contoh tidak tepat: UsersService — karena tidak semua module perlu akses user.

### Soal 4
Apa akibatnya jika Module A mengimport Module B, tapi Module A lupa menambahkan Module B ke `imports`?

**Jawaban:**
NestJS akan melempar error: `Nest can't resolve dependencies of XService`. Solusinya: pastikan module yang menyediakan provider sudah di-import di `@Module({ imports: [...] })`.
