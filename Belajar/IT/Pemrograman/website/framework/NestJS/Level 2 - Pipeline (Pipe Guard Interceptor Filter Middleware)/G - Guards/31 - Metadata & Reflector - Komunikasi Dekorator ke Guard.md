# 31 - Metadata & Reflector - Komunikasi Dekorator ke Guard

## Penjelasan

Di materi sebelumnya (30 — Guard Fundamental), Guard kita **sama untuk semua endpoint** — semua harus punya API key. Tapi bagaimana kalau kita ingin **beberapa endpoint bersifat publik** (tidak perlu API key)? Atau bagaimana caranya Guard tahu **role apa yang dibutuhkan** untuk suatu endpoint?

Di sinilah **metadata** dan **Reflector** berperan. Kita bisa menempelkan metadata ke handler/controller menggunakan `@SetMetadata()` atau custom decorator, lalu Guard membaca metadata itu menggunakan `Reflector`.

Ini seperti memberikan **stiker akses** di pintu setiap ruangan: "Ruangan ini: LEVEL VIP" — lalu satpam (Guard) membaca stiker itu sebelum memutuskan siapa yang boleh masuk.

## Fungsi

- **Menyimpan informasi tambahan** di dekorator handler/controller
- **Membaca metadata** dari handler/controller di dalam Guard
- **Membuat endpoint publik** yang tidak perlu autentikasi
- **Mendefinisikan role requirement** untuk RBAC (Role-Based Access Control)
- **Konfigurasi per-endpoint** untuk Guard/Interceptor

## Cara Implementasi

### 1. `@SetMetadata` — Cara langsung

```typescript
// src/cats/cats.controller.ts
import { Controller, Get, SetMetadata } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Get()
  @SetMetadata('isPublic', true) // endpoint ini publik
  findAll() {
    return ['kucing1', 'kucing2'];
  }

  @Get(':id')
  @SetMetadata('roles', ['admin']) // cuma admin bisa akses
  findOne(@Param('id') id: string) {
    return `kucing ${id}`;
  }
}
```

### 2. Custom Decorator — `@Public()` & `@Roles()`

Cara langsung dengan `@SetMetadata` agak bertele-tele. Lebih baik buat **custom decorator**:

```typescript
// src/common/decorators/public.decorator.ts
import { SetMetadata } from '@nestjs/common';

export const IS_PUBLIC_KEY = 'isPublic';
export const Public = () => SetMetadata(IS_PUBLIC_KEY, true);
```

```typescript
// src/common/decorators/roles.decorator.ts
import { SetMetadata } from '@nestjs/common';

export const ROLES_KEY = 'roles';
export const Roles = (...roles: string[]) => SetMetadata(ROLES_KEY, roles);
```

Penggunaan di controller:

```typescript
@Controller('cats')
export class CatsController {
  @Get()
  @Public() // publik — tanpa API key
  findAll() {
    return ['kucing1'];
  }

  @Post()
  @Roles('admin') // butuh role admin
  create() {
    return 'dibuat';
  }
}
```

### 3. Membaca Metadata dengan Reflector di Guard

Sekarang kita modifikasi `ApiKeyGuard` dari materi sebelumnya agar **melewati endpoint yang diberi `@Public()`**:

```typescript
// src/common/guards/api-key.guard.ts
import { Injectable, CanActivate, ExecutionContext, UnauthorizedException } from '@nestjs/common';
import { Reflector } from '@nestjs/core';
import { IS_PUBLIC_KEY } from '../decorators/public.decorator';
import { Request } from 'express';

@Injectable()
export class ApiKeyGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    // Baca metadata isPublic dari handler, fallback ke controller
    const isPublic = this.reflector.getAllAndOverride<boolean>(IS_PUBLIC_KEY, [
      context.getHandler(),
      context.getClass(),
    ]);

    if (isPublic) {
      return true; // skip validasi API key
    }

    const request = context.switchToHttp().getRequest<Request>();
    const apiKey = request.headers['x-api-key'];

    if (!apiKey || apiKey !== process.env.API_KEY) {
      throw new UnauthorizedException('API Key tidak valid');
    }

    return true;
  }
}
```

### 4. `getAllAndMerge` vs `getAllAndOverride` vs `get`

```typescript
// Dapat dari handler (prioritas) atau controller
const value = this.reflector.get<string>('key', context.getHandler());

// Override: handler override controller
const value = this.reflector.getAllAndOverride<string>('key', [
  context.getHandler(),
  context.getClass(),
]);

// Merge: gabungkan array dari handler dan controller
const roles = this.reflector.getAllAndMerge<string[]>('roles', [
  context.getHandler(),
  context.getClass(),
]);
// Jika controller punya ['admin'] dan handler punya ['editor'], hasil: ['admin', 'editor']
```

## Analogi — Gedung Bertingkat

Bayangkan Guard adalah **satpam di pintu utama setiap lantai**:

- **`@SetMetadata('isPublic', true)`** = Menempel stiker **"RUANG TERBUKA — Semua Boleh Masuk"** di pintu ruangan
- **`@Roles('admin')`** = Menempel stiker **"HANYA KARYAWAN LEVEL ADMIN"** di pintu
- **`Reflector`** = **Alat pembaca stiker** yang dipegang satpam — satpam bisa lihat stiker di pintu
- **`getAllAndOverride`** = Satpam cek stiker di pintu dulu, baru stiker di lantai — stiker pintu lebih penting
- **`getAllAndMerge`** = Satpam baca **semua stiker** dari lantai dan pintu, lalu gabungkan

Tanpa Reflector, satpam (Guard) tidak punya cara untuk tahu **ruangan mana yang boleh dimasuki siapa**. Metadata dan Reflector adalah **sistem komunikasi** antara yang pasang stiker (developer) dan yang baca stiker (Guard).

## Dipakai untuk Apa

- **Public endpoint**: Login, register, forgot-password — endpoint yang tidak perlu token
- **Role-Based Access Control**: Menentukan role apa saja yang boleh akses suatu endpoint
- **Feature flags**: Endpoint tertentu hanya aktif jika fitur diaktifkan
- **Custom permission**: Metadata kustom seperti batas rate, cache TTL, dll.
- **Throttle configuration**: Konfigurasi rate limiting per-endpoint

## Kesalahan Umum

1. **Lupa memberikan constructor `Reflector`**: Guard yang butuh `Reflector` harus punya `constructor(private reflector: Reflector) {}`.
2. **Salah urutan `getAllAndOverride`**: Parameter pertama harus handler, kedua class. Kalo terbalik, metadata handler akan diabaikan.
3. **Typo key constant**: Gunakan constant (`IS_PUBLIC_KEY`) jangan string literal, biar konsisten antara decorator dan guard.
4. **Lupa fallback value**: `reflector.get()` bisa return `undefined` — pastikan handle default value.
5. **Decorator di controller vs handler**: `@Public()` di controller akan mempengaruhi **semua endpoint** di controller itu. `@Public()` di handler hanya untuk satu endpoint. Ini yang membuat `getAllAndOverride` berguna.

## Soal Latihan

**Soal**: Buat `@Public()` decorator dan modifikasi `ApiKeyGuard` agar bisa membaca `@Public()`. Endpoint dengan `@Public()` harus bisa diakses tanpa API key.

<details>
<summary>Jawaban</summary>

```typescript
// src/common/decorators/public.decorator.ts
import { SetMetadata } from '@nestjs/common';

export const IS_PUBLIC_KEY = 'isPublic';
export const Public = () => SetMetadata(IS_PUBLIC_KEY, true);
```

```typescript
// src/common/guards/api-key.guard.ts
import { Injectable, CanActivate, ExecutionContext, UnauthorizedException } from '@nestjs/common';
import { Reflector } from '@nestjs/core';
import { IS_PUBLIC_KEY } from '../decorators/public.decorator';
import { Request } from 'express';

@Injectable()
export class ApiKeyGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const isPublic = this.reflector.getAllAndOverride<boolean>(IS_PUBLIC_KEY, [
      context.getHandler(),
      context.getClass(),
    ]);

    if (isPublic) {
      return true;
    }

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
// src/cats/cats.controller.ts
import { Controller, Get, Post, UseGuards } from '@nestjs/common';
import { ApiKeyGuard } from '../common/guards/api-key.guard';
import { Public } from '../common/decorators/public.decorator';

@Controller('cats')
@UseGuards(ApiKeyGuard)
export class CatsController {
  @Get()
  findAll() {
    return ['kucing1'];
  }

  @Post('login')
  @Public() // endpoint publik — tanpa API key
  login() {
    return 'login berhasil';
  }
}
```
</details>
