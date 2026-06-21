# 33 - Role-Based Authorization Guard - RBAC

## Penjelasan

Di materi sebelumnya (32 — JWT Auth Guard), kita sudah bisa **memverifikasi siapa usernya** (autentikasi). Sekarang kita perlu menentukan **apa yang boleh dilakukan user** (otorisasi).

RBAC (Role-Based Access Control) adalah sistem otorisasi di mana setiap user punya **role** (admin, editor, viewer), dan setiap endpoint punya **role requirement**. Guard akan membandingkan: "Apakah role user termasuk dalam daftar role yang diizinkan?"

Ini seperti **Kartu Akses Bertingkat** di gedung: karyawan biasa cuma bisa akses lantai 1-2, supervisor bisa lantai 1-5, direktur bisa semua lantai.

## Fungsi

- **Membatasi akses endpoint** berdasarkan role user
- **Hierarchical roles** — admin bisa melakukan apa yang bisa dilakukan editor
- **Granular permission** — endpoint tertentu hanya untuk role tertentu
- **Integrasi dengan JWT** — role diambil dari token yang sudah diverifikasi

## Cara Implementasi

### 1. Decorator `@Roles()`

```typescript
// src/common/decorators/roles.decorator.ts
import { SetMetadata } from '@nestjs/common';

export const ROLES_KEY = 'roles';
export const Roles = (...roles: string[]) => SetMetadata(ROLES_KEY, roles);
```

### 2. RolesGuard

```typescript
// src/auth/guards/roles.guard.ts
import { Injectable, CanActivate, ExecutionContext, ForbiddenException } from '@nestjs/common';
import { Reflector } from '@nestjs/core';
import { ROLES_KEY } from '../../common/decorators/roles.decorator';
import { Request } from 'express';

@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    // Baca role requirement dari metadata handler/controller
    const requiredRoles = this.reflector.getAllAndOverride<string[]>(ROLES_KEY, [
      context.getHandler(),
      context.getClass(),
    ]);

    // Jika endpoint tidak punya @Roles(), izinkan semua
    if (!requiredRoles || requiredRoles.length === 0) {
      return true;
    }

    const request = context.switchToHttp().getRequest<Request>();
    const user = request.user as { role: string };

    if (!user) {
      throw new ForbiddenException('User tidak terautentikasi');
    }

    // Cek apakah role user termasuk dalam required roles
    const hasRole = requiredRoles.includes(user.role);

    if (!hasRole) {
      throw new ForbiddenException(
        `Akses ditolak. Butuh role: ${requiredRoles.join(', ')}`,
      );
    }

    return true;
  }
}
```

### 3. Urutan Guard — JwtAuthGuard Dulu, Baru RolesGuard

Urutan sangat penting. **JwtAuthGuard harus dijalankan duluan** karena RolesGuard butuh `request.user` yang diisi oleh JwtAuthGuard.

```typescript
// src/cats/cats.controller.ts
import { Controller, Get, Post, Patch, Delete, UseGuards } from '@nestjs/common';
import { JwtAuthGuard } from '../auth/guards/jwt-auth.guard';
import { RolesGuard } from '../auth/guards/roles.guard';
import { Roles } from '../common/decorators/roles.decorator';
import { Public } from '../common/decorators/public.decorator';
import { CurrentUser } from '../common/decorators/current-user.decorator';

@Controller('cats')
@UseGuards(JwtAuthGuard, RolesGuard) // JwtAuthGuard jalan duluan
export class CatsController {
  @Get()
  @Roles('admin', 'editor', 'viewer') // semua role bisa lihat
  findAll() {
    return ['kucing1', 'kucing2'];
  }

  @Post()
  @Roles('admin', 'editor') // hanya admin & editor bisa create
  create() {
    return 'kucing dibuat';
  }

  @Patch(':id')
  @Roles('admin') // hanya admin bisa update
  update() {
    return 'kucing diupdate';
  }

  @Delete(':id')
  @Roles('admin') // hanya admin bisa hapus
  remove() {
    return 'kucing dihapus';
  }

  @Get('public')
  @Public() // override JwtAuthGuard, tidak perlu autentikasi
  publicList() {
    return ['kucing publik'];
  }
}
```

### 4. Hierarchical Roles — (Bonus)

Kadang kita ingin role yang lebih tinggi bisa melakukan apa yang bisa dilakukan role di bawahnya:

```typescript
// src/auth/guards/roles.guard.ts — dengan hierarchy
const ROLE_HIERARCHY: Record<string, number> = {
  viewer: 1,
  editor: 2,
  admin: 3,
  superadmin: 4,
};

// Di canActivate:
const userRoleLevel = ROLE_HIERARCHY[user.role] ?? 0;

const hasRole = requiredRoles.some((role) => {
  return ROLE_HIERARCHY[role] <= userRoleLevel;
});
```

### 5. Global Registration

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { APP_GUARD } from '@nestjs/core';
import { JwtAuthGuard } from './auth/guards/jwt-auth.guard';
import { RolesGuard } from './auth/guards/roles.guard';

@Module({
  providers: [
    { provide: APP_GUARD, useClass: JwtAuthGuard },
    { provide: APP_GUARD, useClass: RolesGuard },
  ],
})
export class AppModule {}
```

## Analogi — Gedung Bertingkat

Bayangkan **RBAC** seperti **Sistem Akses Lantai** di gedung perkantoran:

- **`@Roles('admin')`** = Pintu ruangan bertuliskan **"HANYA DIREKTUR"**
- **`request.user.role`** = **Level kartu akses** karyawan — tertera di kartu: `role: admin`
- **RolesGuard** = **Satpam yang baca stiker pintu** lalu **cek kartu karyawan** — cocok? Silakan masuk. Tidak cocok? Dilarang.
- **Urutan Guard**: **Satpam pintu utama** (JwtAuthGuard) ngecek dulu: "Kartunya asli?" — baru **Satpam lantai** (RolesGuard) ngecek: "Berhak akses ruangan ini?"
- **Hierarchical roles** = Kartu akses **Direktur** bisa buka **semua pintu**. Kartu **Editor** cuma bisa lantai 1-3.

JwtAuthGuard = **Pintu utama gedung** — verifikasi kartu.  
RolesGuard = **Pintu setiap ruangan** — cek otorisasi per ruangan.

## Dipakai untuk Apa

- **Admin panel** — endpoint CRUD hanya untuk admin
- **Multi-tenant apps** — user biasa vs super admin
- **Content management** — editor bisa tulis, viewer cuma baca
- **B2B vs B2C** — fitur berbeda untuk tipe user berbeda
- **Feature gating** — fitur beta hanya untuk tester role tertentu

## Kesalahan Umum

1. **Urutan guard terbalik**: RolesGuard dijalankan SEBELUM JwtAuthGuard — `request.user` masih `undefined`, semua request ditolak.
2. **Lupa `@Roles()` di handler**: Jika lupa, semua role bisa akses (karena guard return true kalo requiredRoles kosong). Kadang ini yang diinginkan, kadang tidak — pastikan sadar akan behavior ini.
3. **Role dari JWT vs database**: Role di token sudah usang? User diubah rolenya tapi token masih lama. Solusi: cek role dari database di JwtStrategy.validate() atau pakai token short-expiry.
4. **Mengirim role yang salah di token**: Pastikan `JwtStrategy.validate()` return object dengan field `role`.
5. **Hardcoded role names**: Buat enum atau constant untuk role names biar tidak typo.

## Soal Latihan

**Soal**: Buat RBAC dengan role `admin`, `editor`, `viewer`:
1. Buat `@Roles()` decorator
2. Buat `RolesGuard` yang membaca metadata dan membandingkan dengan `request.user.role`
3. Implementasikan di `CatsController`:
   - `GET /cats` — `admin`, `editor`, `viewer`
   - `POST /cats` — `admin`, `editor`
   - `DELETE /cats/:id` — hanya `admin`

<details>
<summary>Jawaban</summary>

```typescript
// src/common/decorators/roles.decorator.ts
import { SetMetadata } from '@nestjs/common';

export const ROLES_KEY = 'roles';
export const Roles = (...roles: string[]) => SetMetadata(ROLES_KEY, roles);
```

```typescript
// src/auth/guards/roles.guard.ts
import { Injectable, CanActivate, ExecutionContext, ForbiddenException } from '@nestjs/common';
import { Reflector } from '@nestjs/core';
import { ROLES_KEY } from '../../common/decorators/roles.decorator';
import { Request } from 'express';

@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const requiredRoles = this.reflector.getAllAndOverride<string[]>(ROLES_KEY, [
      context.getHandler(),
      context.getClass(),
    ]);

    if (!requiredRoles || requiredRoles.length === 0) {
      return true;
    }

    const request = context.switchToHttp().getRequest<Request>();
    const user = request.user as { role: string };

    if (!user) {
      throw new ForbiddenException('User tidak terautentikasi');
    }

    if (!requiredRoles.includes(user.role)) {
      throw new ForbiddenException(`Butuh role: ${requiredRoles.join(', ')}`);
    }

    return true;
  }
}
```

```typescript
// src/cats/cats.controller.ts
import { Controller, Get, Post, Delete, UseGuards } from '@nestjs/common';
import { JwtAuthGuard } from '../auth/guards/jwt-auth.guard';
import { RolesGuard } from '../auth/guards/roles.guard';
import { Roles } from '../common/decorators/roles.decorator';

@Controller('cats')
@UseGuards(JwtAuthGuard, RolesGuard)
export class CatsController {
  @Get()
  @Roles('admin', 'editor', 'viewer')
  findAll() {
    return ['kucing1', 'kucing2'];
  }

  @Post()
  @Roles('admin', 'editor')
  create() {
    return 'kucing dibuat';
  }

  @Delete(':id')
  @Roles('admin')
  remove() {
    return 'kucing dihapus';
  }
}
```
</details>
