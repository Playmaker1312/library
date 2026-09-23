# 16 - Custom Decorators: Membuat Shortcut untuk Logika Berulang

## Penjelasan

Di materi sebelumnya kita melihat resepsionis (Controller) perlu membaca berbagai informasi dari request — param, query, body, headers. NestJS sudah menyediakan dekorator bawaan seperti `@Param()`, `@Query()`, dll. Tapi kadang kita butuh **kombinasi logika yang berulang** di banyak endpoint, misalnya membaca pagination (page + limit) atau mengambil data user dari token. Custom decorator memungkinkan kita membuat **resepsionis khusus** yang langsung memberikan informasi jadi, bukan mentah.

---

## Fungsi

- Mengekstrak dan mentransformasi data request
- Membuat reusable parameter decorator
- Menggabungkan beberapa dekorator (composing decorators)
- Mengurangi boilerplate code di controller
- Membaca metadata dari token/session

---

## Cara Pengimplementasian

### 1. `createParamDecorator` — Decorator Sederhana

```typescript
// decorators/user.decorator.ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common';

export const User = createParamDecorator(
  (data: string | undefined, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    const user = request.user; // dari auth middleware/guard

    return data ? user?.[data] : user;
  },
);
```

Penggunaan di controller:

```typescript
@Controller('coffee')
export class CoffeeController {
  @Get('profile')
  getProfile(@User() user: any): string {
    return `User: ${user.name}`;
  }

  @Get('profile-name')
  getProfileName(@User('name') name: string): string {
    return `Nama: ${name}`;
  }
}
```

### 2. Decorator dengan Transformasi Data — `@CurrentUser()`

```typescript
// decorators/current-user.decorator.ts
export const CurrentUser = createParamDecorator(
  (data: keyof UserEntity | undefined, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    const user = request.user as UserEntity;

    if (!user) {
      throw new UnauthorizedException('User tidak ditemukan');
    }

    return data ? user[data] : user;
  },
);

// Type safety
export interface UserEntity {
  id: number;
  email: string;
  name: string;
  role: string;
}
```

### 3. `@Pagination()` Decorator — Contoh Kompleks

Sering kita membaca `?page=1&limit=10` berulang kali. Mari buat decorator yang langsung mengembalikan object pagination + default values.

```typescript
// decorators/pagination.decorator.ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common';

export interface PaginationParams {
  page: number;
  limit: number;
  offset: number;
}

export const Pagination = createParamDecorator(
  (defaultLimit = 10, ctx: ExecutionContext): PaginationParams => {
    const request = ctx.switchToHttp().getRequest();
    const query = request.query;

    const page = Math.max(1, parseInt(query.page, 10) || 1);
    const limit = Math.min(100, Math.max(1, parseInt(query.limit, 10) || defaultLimit));
    const offset = (page - 1) * limit;

    return { page, limit, offset };
  },
);
```

Penggunaan:

```typescript
@Controller('coffee')
export class CoffeeController {
  @Get()
  findAll(@Pagination() pagination: PaginationParams): string {
    return `Page ${pagination.page}, limit ${pagination.limit}, offset ${pagination.offset}`;
  }

  @Get('reviews')
  getReviews(@Pagination(5) pagination: PaginationParams): string {
    // default limit = 5 (bukan 10)
    return `Reviews page ${pagination.page}`;
  }
}
```

Response query `GET /coffee?page=2&limit=50`:

```json
{
  "page": 2,
  "limit": 50,
  "offset": 50
}
```

### 4. Composing Decorators — Gabungan Beberapa Decorator

```typescript
// decorators/auth.decorator.ts
import { applyDecorators, UseGuards, SetMetadata } from '@nestjs/common';
import { AuthGuard } from '../guards/auth.guard';
import { RolesGuard } from '../guards/roles.guard';
import { Roles } from './roles.decorator';

export function Auth(role?: string) {
  return applyDecorators(
    UseGuards(AuthGuard, RolesGuard),
    Roles(role || 'user'),
    SetMetadata('description', 'Endpoint terproteksi'),
  );
}
```

```typescript
// decorators/roles.decorator.ts
import { SetMetadata } from '@nestjs/common';

export const Roles = (role: string) => SetMetadata('role', role);
```

Penggunaan:

```typescript
@Controller('admin')
export class AdminController {
  @Get()
  @Auth('admin') // satu baris = guard + roles + metadata
  getDashboard(): string {
    return 'Admin dashboard';
  }
}
```

### 5. Decorator dengan Pipe Validasi

```typescript
// decorators/parse-uuid.decorator.ts
import { createParamDecorator, ExecutionContext, ParseUUIDPipe } from '@nestjs/common';

export const UUIDParam = createParamDecorator(
  (data: string, ctx: ExecutionContext) => {
    const params = ctx.switchToHttp().getRequest().params;
    return params[data];
  },
);

// Penggunaan di controller
@Get(':id')
findOne(@UUIDParam('id', ParseUUIDPipe) id: string): string {
  return `Coffee ${id}`;
}
```

### 6. Decorator untuk IP Address

```typescript
// decorators/ip.decorator.ts
export const IpAddress = createParamDecorator(
  (data: unknown, ctx: ExecutionContext): string => {
    const request = ctx.switchToHttp().getRequest();
    return request.ip ||
      request.headers['x-forwarded-for']?.split(',')[0] ||
      request.connection?.remoteAddress;
  },
);

// Controller
@Get('visitor')
trackVisitor(@IpAddress() ip: string): string {
  return `Visitor dari IP: ${ip}`;
}
```

---

## Analogi: Gedung Bertingkat

| Custom Decorator | Analogi Gedung |
|---|---|
| `createParamDecorator` | Resepsionis **custom** yang dilatih tugas spesifik |
| `@User()` | Resepsionis yang **sudah hafal** data semua penghuni — langsung kasih nama, tidak perlu KTP |
| `@Pagination()` | Resepsionis yang langsung hitung **"halaman berapa, berapa baris, mulai dari mana"** |
| `applyDecorators` | **Stempel gabungan** — satu stempel berisi 3 informasi sekaligus |
| `@Auth('admin')` | Satu perintah yang langsung: **cek KTP + cek lantai akses + catat log** |

Daripada resepsionis harus **setiap kali** menghitung sendiri page/limit, kita buat resepsionis yang **otomatis** melakukannya.

---

## Dipakai Untuk Apa

- Autentikasi / authorisasi (`@CurrentUser()`)
- Pagination (`@Pagination()`)
- Parsing IP / device info (`@IpAddress()`, `@UserAgent()`)
- Transformasi parameter (`@UUIDParam()`, `@Sanitize()`)
- Logging / audit trail
- A/B testing flags

---

## Kesalahan Umum

1. **Decorator tidak jalan di global pipes** — custom decorator diproses sebelum pipe; gunakan pipe secara eksplisit jika perlu.
2. **Mutasi request object** — hati-hati mengubah `request` langsung, bisa menyebabkan side effect.
3. **Lupa export decorator** — decorator tidak bisa dipakai jika tidak di-export dari module.
4. **Typing tidak sesuai** — selalu definisikan interface return type decorator.
5. **Data parameter tidak dipakai** — parameter `data` di `createParamDecorator((data, ctx) => ...)` membawa nilai argumen decorator (misal `@User('name')` → `data = 'name'`).
6. **ExecutionContext assumptions** — decorator bisa dipakai di konteks non-HTTP (WebSocket, RPC); cek `ctx.getType()` untuk amannya.

---

## Soal Latihan

### Soal 1
Buat decorator `@Pagination()` yang membaca query `?page=1&limit=10` dan mengembalikan `{ page, limit, offset }` dengan:
- Default limit = 10
- Maksimal limit = 100
- Page minimal = 1
- Tidak perlu query parameter → pakai default

Tulis implementasi lengkap dan contoh penggunaannya di controller.

<details>
<summary>Jawaban</summary>

```typescript
// decorators/pagination.decorator.ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common';

export interface PaginationParams {
  page: number;
  limit: number;
  offset: number;
}

export const Pagination = createParamDecorator(
  (defaultLimit = 10, ctx: ExecutionContext): PaginationParams => {
    const request = ctx.switchToHttp().getRequest();
    const query = request.query;

    const page = Math.max(1, parseInt(query.page, 10) || 1);
    const limit = Math.min(100, Math.max(1, parseInt(query.limit, 10) || defaultLimit));
    const offset = (page - 1) * limit;

    return { page, limit, offset };
  },
);

// Controller
import { Controller, Get } from '@nestjs/common';
import { Pagination, PaginationParams } from './decorators/pagination.decorator';

@Controller('items')
export class ItemsController {
  @Get()
  findAll(@Pagination() pagination: PaginationParams): string {
    return JSON.stringify(pagination);
  }

  @Get('recent')
  findRecent(@Pagination(5) pagination: PaginationParams): string {
    return `Recent items — ${JSON.stringify(pagination)}`;
  }
}

// Test: GET /items?page=2&limit=20 → {"page":2,"limit":20,"offset":20}
// Test: GET /items/recent → {"page":1,"limit":5,"offset":0}
// Test: GET /items?page=0&limit=200 → {"page":1,"limit":100,"offset":0}
```
</details>

### Soal 2
Buat decorator `@UserAgent()` yang membaca header `User-Agent` dari request dan mengembalikan string. Jika tidak ada, kembalikan `'Unknown'`.

<details>
<summary>Jawaban</summary>

```typescript
// decorators/user-agent.decorator.ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common';

export const UserAgent = createParamDecorator(
  (data: unknown, ctx: ExecutionContext): string => {
    const request = ctx.switchToHttp().getRequest();
    return request.headers['user-agent'] || 'Unknown';
  },
);

// Controller
@Controller('analytics')
export class AnalyticsController {
  @Get('visit')
  trackVisit(@UserAgent() userAgent: string): string {
    return `Visitor menggunakan: ${userAgent}`;
  }
}
```
</details>

### Soal 3
Buat composing decorator `@ApiEndpoint(summary, status)` yang menggabungkan:
- `@Get()` atau `@Post()` — tergantung parameter
- `@HttpCode(status)`
- `SetMetadata('summary', summary)`

<details>
<summary>Jawaban</summary>

```typescript
// decorators/api-endpoint.decorator.ts
import { applyDecorators, Get, Post, HttpCode, SetMetadata } from '@nestjs/common';

export function ApiEndpoint(method: 'GET' | 'POST', path: string, summary: string, status = 200) {
  const methodDecorator = method === 'GET' ? Get(path) : Post(path);

  return applyDecorators(
    methodDecorator,
    HttpCode(status),
    SetMetadata('summary', summary),
    SetMetadata('description', summary),
  );
}

// Controller
@Controller('coffee')
export class CoffeeController {
  @ApiEndpoint('GET', '/', 'Daftar semua coffee')
  findAll(): string {
    return 'Semua coffee';
  }

  @ApiEndpoint('POST', '/', 'Buat coffee baru', 201)
  create(): string {
    return 'Coffee dibuat';
  }
}
```
</details>
