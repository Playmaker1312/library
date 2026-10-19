# 60 - Permission-Based Authorization dengan CASL

---

## Penjelasan

Setelah user terautentikasi (siapa kamu), kita perlu mengontrol **apa yang boleh dilakukan** user di aplikasi (authorization). Dua pendekatan umum: **RBAC** (Role-Based Access Control — user punya role, role punya permission) dan **ABAC** (Attribute-Based Access Control — keputusan berdasarkan atribut user, resource, dan lingkungan). CASL (pronounced "castle") adalah library yang mendukung keduanya. Dengan CASL, kita mendefinisikan **ability** (subject + action) untuk setiap user.

---

## Fungsi

- Mendefinisikan permission secara deklaratif (can/cannot)
- Memeriksa apakah user bisa melakukan action tertentu pada subject tertentu
- PoliciesGuard untuk melindungi endpoint berbasis permission
- `@CheckPolicies()` custom decorator untuk deklarasi permission di controller
- Mendukung kondisi dinamis (misal: author hanya bisa edit post miliknya)

---

## Cara Pengimplementasian

### 1. Install package

```bash
npm install @casl/ability @casl/mongoose
```

### 2. Define AbilityFactory

```typescript
// auth/casl/ability.factory.ts
import { Injectable } from '@nestjs/common';
import {
  AbilityBuilder,
  AbilityTuple,
  ExtractSubjectType,
  PureAbility,
} from '@casl/ability';
import { User, UserRole } from '../../user/entities/user.entity';
import { Post } from '../../blog/entities/post.entity';

export type Action = 'manage' | 'create' | 'read' | 'update' | 'delete';

@Injectable()
export class AbilityFactory {
  defineAbility(user: User) {
    const { can, cannot, build } = new AbilityBuilder<
      PureAbility<[Action, any]>
    >(PureAbility);

    // Admin bisa melakukan apa saja
    if (user.role === UserRole.ADMIN) {
      can('manage', 'all');
      return build();
    }

    // User biasa: baca semua Post
    can('read', Post);

    // Buat Post baru
    can('create', Post);

    // Update/Delete hanya post milik sendiri
    can('update', Post, { authorId: user.id });
    can('delete', Post, { authorId: user.id });

    // User tidak bisa manage user lain
    cannot('manage', User);

    return build({
      detectSubjectType: (item) =>
        item.constructor as ExtractSubjectType<any>,
    });
  }
}
```

### 3. PoliciesGuard

```typescript
// auth/guards/policies.guard.ts
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Reflector } from '@nestjs/core';
import { AbilityFactory } from '../casl/ability.factory';
import { CHECK_POLICIES_KEY } from '../decorators/check-policies.decorator';

export interface PolicyHandler {
  handle(ability: any): boolean;
}

type PolicyHandlerCallback = (ability: any) => boolean;

@Injectable()
export class PoliciesGuard implements CanActivate {
  constructor(
    private readonly reflector: Reflector,
    private readonly abilityFactory: AbilityFactory,
  ) {}

  async canActivate(context: ExecutionContext): Promise<boolean> {
    const handlers =
      this.reflector.getAllAndOverride<PolicyHandler[] | PolicyHandlerCallback[]>(
        CHECK_POLICIES_KEY,
        [context.getHandler(), context.getClass()],
      );

    if (!handlers) {
      return true; // tidak ada policy restriction
    }

    const { user } = context.switchToHttp().getRequest();
    const ability = this.abilityFactory.defineAbility(user);

    return handlers.every((handler) => {
      if (typeof handler === 'function') {
        return handler(ability);
      }
      return handler.handle(ability);
    });
  }
}
```

### 4. @CheckPolicies() Decorator

```typescript
// auth/decorators/check-policies.decorator.ts
import { SetMetadata } from '@nestjs/common';
import { PolicyHandler } from '../guards/policies.guard';

export const CHECK_POLICIES_KEY = 'check_policies';
export const CheckPolicies = (...handlers: PolicyHandler[]) =>
  SetMetadata(CHECK_POLICIES_KEY, handlers);
```

### 5. Controller — Post dengan CASL

```typescript
// blog/blog.controller.ts
import { Controller, Post, Get, Patch, Delete, Param, Body, UseGuards } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';
import { PoliciesGuard } from '../auth/guards/policies.guard';
import { CheckPolicies } from '../auth/decorators/check-policies.decorator';
import { AbilityFactory, Action } from '../auth/casl/ability.factory';
import { BlogService } from './blog.service';
import { CreatePostDto } from './dto/create-post.dto';
import { UpdatePostDto } from './dto/update-post.dto';
import { Post as PostEntity } from './entities/post.entity';
import { CurrentUser } from '../auth/decorators/current-user.decorator';
import { User } from '../user/entities/user.entity';

@Controller('posts')
export class BlogController {
  constructor(private readonly blogService: BlogService) {}

  @Get()
  @UseGuards(AuthGuard('jwt'), PoliciesGuard)
  @CheckPolicies((ability: any) => ability.can('read', PostEntity))
  findAll() {
    return this.blogService.findAll();
  }

  @Post()
  @UseGuards(AuthGuard('jwt'), PoliciesGuard)
  @CheckPolicies((ability: any) => ability.can('create', PostEntity))
  create(@Body() dto: CreatePostDto, @CurrentUser() user: User) {
    return this.blogService.create(dto, user.id);
  }

  @Patch(':id')
  @UseGuards(AuthGuard('jwt'), PoliciesGuard)
  @CheckPolicies((ability: any) => ability.can('update', PostEntity))
  async update(
    @Param('id') id: string,
    @Body() dto: UpdatePostDto,
    @CurrentUser() user: User,
  ) {
    const post = await this.blogService.findById(id);
    const ability = new AbilityFactory().defineAbility(user);

    // Cek ability dengan resource spesifik
    if (!ability.can('update', post)) {
      throw new ForbiddenException('You can only edit your own posts');
    }

    return this.blogService.update(id, dto);
  }

  @Delete(':id')
  @UseGuards(AuthGuard('jwt'), PoliciesGuard)
  @CheckPolicies((ability: any) => ability.can('delete', PostEntity))
  async remove(@Param('id') id: string, @CurrentUser() user: User) {
    const post = await this.blogService.findById(id);
    const ability = new AbilityFactory().defineAbility(user);

    if (!ability.can('delete', post)) {
      throw new ForbiddenException('You can only delete your own posts');
    }

    return this.blogService.remove(id);
  }
}
```

### 6. Service — Throw ForbiddenException

```typescript
// blog/blog.service.ts
import { ForbiddenException } from '@nestjs/common';

@Injectable()
export class BlogService {
  async update(id: string, dto: UpdatePostDto, user: User) {
    const post = await this.findById(id);

    // Security check — jangan percaya hanya pada guard
    if (post.authorId !== user.id && user.role !== UserRole.ADMIN) {
      throw new ForbiddenException('You can only edit your own posts');
    }

    return this.postRepo.save({ ...post, ...dto });
  }
}
```

### 7. Update User Entity dengan Role

```typescript
// user/entities/user.entity.ts
export enum UserRole {
  USER = 'user',
  AUTHOR = 'author',
  ADMIN = 'admin',
}

export class User {
  @Column({
    type: 'enum',
    enum: UserRole,
    default: UserRole.USER,
  })
  role: UserRole;
}
```

### 8. Daftarkan di Module

```typescript
// auth/auth.module.ts
import { AbilityFactory } from './casl/ability.factory';
import { PoliciesGuard } from './guards/policies.guard';

@Module({
  providers: [
    AuthService,
    JwtStrategy,
    JwtRefreshStrategy,
    GoogleStrategy,
    GithubStrategy,
    AbilityFactory,
    PoliciesGuard,
  ],
  exports: [AbilityFactory],
})
export class AuthModule {}
```

---

## CASL Rule Explanation

```typescript
// Rule definition examples
can('read', Post);                          // Semua user bisa baca semua Post
can('update', Post, { authorId: user.id }); // Update hanya post milik sendiri
cannot('manage', User);                     // Tidak bisa manage user lain
can('manage', 'all');                       // Admin bisa apa saja
```

| Action | Subject | Condition | Artinya |
|--------|---------|-----------|---------|
| read | Post | — | Bisa membaca semua post |
| create | Post | — | Bisa membuat post baru |
| update | Post | `{ authorId: user.id }` | Bisa update jika penulis |
| delete | Post | `{ authorId: user.id }` | Bisa hapus jika penulis |
| manage | User | — | ADMIN: bisa manage semua user |

---

## Analogi

CASL seperti **daftar izin akses gedung bertingkat**. Setiap orang punya kartu akses (ability) yang menentukan lantai (subject) dan tindakan (action) yang diizinkan:
- **Admin** punya kartu **master** — bisa ke semua lantai (manage all)
- **Author** punya kartu yang bisa **membuka pintu ruangannya sendiri** (update/delete post milik sendiri)
- **User biasa** hanya bisa **masuk lobi** (read post)
- Aturan "hanya penulis yang bisa edit" seperti **sensor sidik jari di gagang pintu** — hanya sidik jari yang cocok dengan pemilik ruangan yang bisa membuka

---

## Dipakai Untuk Apa

- Aplikasi multi-role (admin, author, user)
- Blog / CMS: author hanya bisa edit tulisannya sendiri
- Dokumen sharing: owner, editor, viewer
- Sistem yang butuh granular permission
- Pengganti RBAC sederhana jika role mulai kompleks

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Hanya guard di controller, tidak di service | Guard bisa dilewati — selalu cek ulang di service |
| Ability didefinisikan inline di controller | Gunakan AbilityFactory agar reusable |
| Lupa `detectSubjectType` | CASL tidak tahu tipe subject tanpa ini |
| Policy handler tidak mengembalikan boolean | Pastikan return `ability.can(...)` |
| Tidak handle ForbiddenException dengan baik | Gunakan `ForbiddenException` + ExceptionFilter |

---

## Soal Latihan

### Soal 1
Buat AbilityFactory yang:
- Admin bisa manage all
- User biasa bisa read Post dan User
- User hanya bisa update data dirinya sendiri (User dengan id yang cocok)

### Jawaban 1
```typescript
@Injectable()
export class AbilityFactory {
  defineAbility(user: User) {
    const { can, cannot, build } = new AbilityBuilder(PureAbility);

    if (user.role === UserRole.ADMIN) {
      can('manage', 'all');
      return build({ detectSubjectType: (item) => item.constructor });
    }

    can('read', Post);
    can('read', User);
    can('update', User, { id: user.id });
    cannot('delete', User);

    return build({ detectSubjectType: (item) => item.constructor });
  }
}
```

### Soal 2
Buat `PoliciesGuard` yang membaca `@CheckPolicies()` decorator dan mengeksekusi handler dengan ability dari user.

### Jawaban 2
```typescript
@Injectable()
export class PoliciesGuard implements CanActivate {
  constructor(
    private readonly reflector: Reflector,
    private readonly abilityFactory: AbilityFactory,
  ) {}

  async canActivate(context: ExecutionContext): Promise<boolean> {
    const handlers = this.reflector.getAllAndOverride(CHECK_POLICIES_KEY, [
      context.getHandler(),
      context.getClass(),
    ]);

    if (!handlers) return true;

    const { user } = context.switchToHttp().getRequest();
    const ability = this.abilityFactory.defineAbility(user);

    return handlers.every((handler) => handler(ability));
  }
}
```

### Soal 3
Apa perbedaan RBAC dan ABAC? Mana yang lebih fleksibel?

### Jawaban 3
**RBAC**: Permission ditentukan oleh role user (admin = semua, user = terbatas). Sederhana tapi kaku — sulit menangani kasus "author hanya bisa edit post sendiri" tanpa menambahkan role baru.

**ABAC**: Permission ditentukan oleh atribut user (id, role, departemen), resource (authorId, status), dan lingkungan (waktu, lokasi). CASL menggunakan ABAC. Lebih fleksibel karena bisa mendefinisikan aturan granular seperti "edit hanya jika authorId = userId".
