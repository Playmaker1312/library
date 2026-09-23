# 30 - Guard Fundamental - CanActivate & ExecutionContext

## Penjelasan

Di Level 1 kita belajar **Pipe** yang bertugas **memvalidasi dan mentransformasi data yang masuk** — seperti satpam yang mengecek KTP di pintu masuk. Sekarang kita naik ke lantai berikutnya: **Guard**.

Jika Pipe adalah **satpam administrasi** (ngecek format KTP), maka Guard adalah **satpam keamanan utama** yang memutuskan: **"Apakah orang ini boleh masuk ke ruangan ini?"** Guard berjalan **sebelum** request mencapai handler route. Guard bisa menolak request (`throw ForbiddenException` / `UnauthorizedException`) atau mengizinkannya masuk.

## Fungsi

- **Autentikasi**: Memastikan user sudah login
- **Otorisasi**: Memastikan user punya role tertentu
- **Validasi akses**: API key, token, IP whitelist, dll.
- **Protection layer pertama**: Mencegah request tidak sah masuk ke handler

## Cara Implementasi

### 1. Membuat Guard sederhana — ApiKeyGuard

```typescript
// src/common/guards/api-key.guard.ts
import { Injectable, CanActivate, ExecutionContext, UnauthorizedException } from '@nestjs/common';
import { Request } from 'express';

@Injectable()
export class ApiKeyGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const request = context.switchToHttp().getRequest<Request>();
    const apiKey = request.headers['x-api-key'];

    if (!apiKey || apiKey !== 'rahasia123') {
      throw new UnauthorizedException('API Key tidak valid');
    }

    return true; // request dizinkan masuk
  }
}
```

### 2. Menggunakan `@UseGuards`

```typescript
// src/cats/cats.controller.ts
import { Controller, Get, UseGuards } from '@nestjs/common';
import { ApiKeyGuard } from '../common/guards/api-key.guard';

@Controller('cats')
@UseGuards(ApiKeyGuard) // semua endpoint di controller ini butuh API key
export class CatsController {
  @Get()
  findAll() {
    return ['kucing1', 'kucing2'];
  }

  @Get(':id')
  @UseGuards(ApiKeyGuard) // bisa juga per-endpoint
  findOne(@Param('id') id: string) {
    return `kucing ${id}`;
  }
}
```

### 3. Guard Global

```typescript
// src/main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { ApiKeyGuard } from './common/guards/api-key.guard';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalGuards(new ApiKeyGuard());
  await app.listen(3000);
}
```

Atau pakai provider:

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { APP_GUARD } from '@nestjs/core';
import { ApiKeyGuard } from './common/guards/api-key.guard';

@Module({
  providers: [
    {
      provide: APP_GUARD,
      useClass: ApiKeyGuard,
    },
  ],
})
export class AppModule {}
```

### 4. ExecutionContext

Guard menerima `ExecutionContext` — ini adalah **pintu komprehensif** yang memberi akses ke:

- `getHandler()` — method handler yang dipanggil
- `getClass()` — controller class
- `switchToHttp()` — akses ke Request/Response
- `switchToRpc()` — untuk microservices
- `switchToWs()` — untuk WebSocket

```typescript
canActivate(context: ExecutionContext): boolean {
  // 1. Dapatkan request
  const request = context.switchToHttp().getRequest<Request>();

  // 2. Dapatkan handler yang sedang dipanggil
  const handler = context.getHandler();
  console.log(`Handler: ${handler.name}`);

  // 3. Dapatkan controller class
  const controller = context.getClass();
  console.log(`Controller: ${controller.name}`);

  return true;
}
```

## Analogi — Gedung Bertingkat

Bayangkan **Guard** adalah **pintu akses utama setiap lantai** di gedung:

- **Lantai 1 (Pipe)**: Satpam administrasi ngecek format KTP — fotokopi udah bener? Tanda tangan jelas?
- **Lantai 2 (Guard)**: Satpam keamanan yang **memutuskan** — "KTP-nya udah sesuai data? Boleh masuk ruangan ini?"
- **ExecutionContext** adalah **papan informasi lengkap**: siapa yang datang (Request), mau ke ruang apa (Handler), di lantai berapa (Controller)
- **Return true** = **Pintu terbuka**, tamu dipersilakan masuk
- **Return false / throw** = **Pintu terkunci**, tamu ditolak

Pipe adalah **meja resepsionis** — ngecek format formulir. Guard adalah **pintu putar keamanan** — memutuskan siapa yang benar-benar boleh lewat.

## Dipakai untuk Apa

- **Melindungi endpoint agar hanya bisa diakses oleh client yang punya API key**
- **Gate awal autentikasi** sebelum JWT strategy dijalankan
- **Rate limiting based on IP/token**
- **Maintenance mode** — Guard bisa return false semua request saat maintenance
- **CORS/CSRF validation tambahan**

## Kesalahan Umum

1. **Lupa return boolean**: `canActivate` harus return `boolean | Promise<boolean> | Observable<boolean>`. Kalo lupa `return` dia akan return `undefined` (falsy) dan request ditolak.
2. **Melempar error yang salah**: Gunakan `UnauthorizedException` (401) untuk autentikasi, `ForbiddenException` (403) untuk otorisasi.
3. **Guard bergantung pada database async**: Guard bisa async — cukup return `Promise<boolean>` atau gunakan `Observable<boolean>`.
4. **Lupa register di module**: Guard harus di-provide atau di-inject dengan benar, apalagi kaloGuard punya dependency injection.
5. **Menganggap Guard berjalan setelah middleware**: Guard jalan **sebelum** handler, tapi **setelah** middleware. Jangan taruh logic yang butuh session express di middleware kalo Guard-nya duluan.

## Soal Latihan

**Soal**: Buat `ApiKeyGuard` yang membaca API key dari header `x-api-key`. Jika API key cocok dengan `process.env.API_KEY`, return true. Jika tidak, lempar `UnauthorizedException`. Terapkan di `CatsController` secara global di module.

<details>
<summary>Jawaban</summary>

```typescript
// src/common/guards/api-key.guard.ts
import { Injectable, CanActivate, ExecutionContext, UnauthorizedException } from '@nestjs/common';
import { Request } from 'express';

@Injectable()
export class ApiKeyGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const request = context.switchToHttp().getRequest<Request>();
    const apiKey = request.headers['x-api-key'];

    if (!apiKey || apiKey !== process.env.API_KEY) {
      throw new UnauthorizedException('API Key tidak valid');
    }

    return true;
  }
}
```

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { APP_GUARD } from '@nestjs/core';
import { ApiKeyGuard } from './common/guards/api-key.guard';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
  providers: [
    {
      provide: APP_GUARD,
      useClass: ApiKeyGuard,
    },
  ],
})
export class AppModule {}
```

```typescript
// .env
API_KEY=rahasia123
```
</details>
