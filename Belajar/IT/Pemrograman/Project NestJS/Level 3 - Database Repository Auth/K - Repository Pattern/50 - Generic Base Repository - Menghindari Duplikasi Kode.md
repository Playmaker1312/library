# 50 - Generic Base Repository - Menghindari Duplikasi Kode

## Penjelasan

Setelah menerapkan Repository Pattern (Pertemuan 49), kita akan melihat banyak duplikasi — setiap repository punya method `findById`, `findAll`, `create`, `update`, `delete` yang hampir sama persis. Generic Base Repository menghilangkan duplikasi itu dengan memanfaatkan TypeScript generics. Cukup tulis sekali, pakai untuk semua model.

Jika Repository Pattern adalah **standar keran SNI**, maka Generic Base Repository adalah **pabrik keran otomatis** — kita tinggal bilang "saya perlu keran untuk kamar mandi" dan pabrik langsung produksi tanpa harus desain ulang.

## Fungsi

- **BaseRepository<T, CreateDto, UpdateDto>**: Class generik dengan method CRUD umum
- **Method umum**: findById, findAll, create, update, delete, count
- **Extend untuk spesifik**: Repository spesifik bisa extend + tambah method sendiri
- **Type Safety**: Generic memastikan tipe data sesuai model

## Cara Pengimplementasian

### 1. BaseRepository Generik

```typescript
// src/common/repositories/base.repository.ts
import { PrismaService } from '../../prisma/prisma.service';

export class BaseRepository<T, CreateDto, UpdateDto> {
  constructor(
    protected prisma: PrismaService,
    protected modelName: string,
  ) {}

  // Gunakan any karena Prisma client tidak secara eksplisit mengekspos tipe delegate
  private get delegate(): any {
    return (this.prisma as any)[this.modelName];
  }

  async findById(id: number | string): Promise<T | null> {
    return this.delegate.findUnique({ where: { id } });
  }

  async findAll(skip = 0, take = 10): Promise<T[]> {
    return this.delegate.findMany({ skip, take });
  }

  async create(data: CreateDto): Promise<T> {
    return this.delegate.create({ data });
  }

  async update(id: number | string, data: UpdateDto): Promise<T> {
    return this.delegate.update({ where: { id }, data });
  }

  async delete(id: number | string): Promise<void> {
    await this.delegate.delete({ where: { id } });
  }

  async count(): Promise<number> {
    return this.delegate.count();
  }

  async findFirst(where: any): Promise<T | null> {
    return this.delegate.findFirst({ where });
  }
}
```

### 2. Type Helper untuk Prisma

```typescript
// src/common/repositories/types.ts
import { Prisma } from '@prisma/client';

// Helper untuk extract Create/Update type dari Prisma
export type PrismaCreate<T> = T extends { create: infer C } ? C : never;
export type PrismaUpdate<T> = T extends { update: infer U } ? U : never;

// Atau lebih sederhana:
export type CreateDto<T> = Prisma.Args<T, 'create'>['data'];
export type UpdateDto<T> = Prisma.Args<T, 'update'>['data'];
```

### 3. Implementasi Spesifik (UserRepository)

```typescript
// src/users/repositories/user.repository.ts
import { Injectable } from '@nestjs/common';
import { User, Prisma } from '@prisma/client';
import { BaseRepository } from '../../common/repositories/base.repository';
import { PrismaService } from '../../prisma/prisma.service';

type UserCreateDto = Prisma.UserCreateInput;
type UserUpdateDto = Prisma.UserUpdateInput;

@Injectable()
export class UserRepository extends BaseRepository<User, UserCreateDto, UserUpdateDto> {
  constructor(prisma: PrismaService) {
    super(prisma, 'user');
  }

  // Method spesifik untuk User
  async findByEmail(email: string): Promise<User | null> {
    return this.prisma.user.findUnique({ where: { email } });
  }

  async findWithPosts(userId: number): Promise<User | null> {
    return this.prisma.user.findUnique({
      where: { id: userId },
      include: { posts: true },
    });
  }
}
```

### 4. Module Setup

```typescript
// src/users/users.module.ts
import { Module } from '@nestjs/common';
import { UsersService } from './users.service';
import { UsersController } from './users.controller';
import { UserRepository } from './repositories/user.repository';

@Module({
  controllers: [UsersController],
  providers: [UsersService, UserRepository],
})
export class UsersModule {}
```

### 5. Service Menggunakan Repository

```typescript
// src/users/users.service.ts
import { Injectable } from '@nestjs/common';
import { User } from '@prisma/client';
import { UserRepository } from './repositories/user.repository';

@Injectable()
export class UsersService {
  constructor(private readonly userRepo: UserRepository) {}

  async getAllUsers(page: number) {
    return this.userRepo.findAll((page - 1) * 10, 10);
  }

  async getUserById(id: number) {
    const user = await this.userRepo.findById(id);
    if (!user) throw new Error('User not found');
    return user;
  }

  async getUserByEmail(email: string) {
    return this.userRepo.findByEmail(email);
  }

  async createUser(data: { email: string; name: string }) {
    return this.userRepo.create(data);
  }

  async deleteUser(id: number) {
    await this.userRepo.delete(id);
  }
}
```

### 6. Implementasi dengan Interface untuk Testability

```typescript
// src/common/repositories/base-repository.interface.ts
export interface IBaseRepository<T, CreateDto, UpdateDto> {
  findById(id: number | string): Promise<T | null>;
  findAll(skip?: number, take?: number): Promise<T[]>;
  create(data: CreateDto): Promise<T>;
  update(id: number | string, data: UpdateDto): Promise<T>;
  delete(id: number | string): Promise<void>;
  count(): Promise<number>;
}

// BaseRepository implements IBaseRepository
```

## Analogi

**Membangun Gedung Bertingkat**

- **BaseRepository Generik** = **mesin cetak keran otomatis** — setting ukuran sekali, cetak ribuan
- **Generic `<T, CreateDto, UpdateDto>`** = **cetakan yang bisa diatur** — "saya perlu keran diameter 1/2 inch, model bulat"
- **UserRepository extends BaseRepository** = **ambil cetakan keran, tinggal tambah fitur khusus** seperti filter air
- **Method findByEmail spesifik** = **tambah fitur sensor suhu** di keran yang sudah jadi
- **Prisma.Args<T, 'create'>['data']** = **spesifikasi teknis** yang sudah distandarisasi pabrik

## Dipakai untuk Apa

- Project dengan banyak model (10+ tabel) — hemat ribuan baris kode
- Memastikan konsistensi method CRUD di semua repository
- Memudahkan maintenance — cukup edit BaseRepository untuk perubahan global
- Onboarding developer baru — pattern sudah jelas dan seragam

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Generic terlalu kompleks sehingga susah dipahami | Simpan generic sederhana dulu, tambah complexity bertahap |
| Typing yang tidak aman (banyak `any`) | Gunakan `Prisma.Args` helper untuk type safety |
| BaseRepository terlalu kaku untuk kasus spesifik | Override method di child class jika perlu |
| Lupa dependency inject PrismaService di BaseRepository | Constructor injection via super() |
| Method spesifik tersebar tidak konsisten | Satukan di repository masing-masing, jangan di service |

## Soal Latihan

1. Buat `BaseRepository<T, CreateDto, UpdateDto>` dengan method: findById, findAll, create, update, delete, count
2. Buat `UserRepository` extends BaseRepository dengan tambahan method `findByEmail`
3. Buat service yang menggunakan UserRepository

### Jawaban

**base.repository.ts:**
```typescript
import { PrismaService } from '../../prisma/prisma.service';

export class BaseRepository<T, CreateDto, UpdateDto> {
  constructor(
    protected prisma: PrismaService,
    private readonly model: string,
  ) {}

  private get delegate() {
    return (this.prisma as any)[this.model];
  }

  async findById(id: number | string): Promise<T | null> {
    return this.delegate.findUnique({ where: { id } });
  }

  async findAll(skip = 0, take = 10): Promise<T[]> {
    return this.delegate.findMany({ skip, take });
  }

  async create(data: CreateDto): Promise<T> {
    return this.delegate.create({ data });
  }

  async update(id: number | string, data: UpdateDto): Promise<T> {
    return this.delegate.update({ where: { id }, data });
  }

  async delete(id: number | string): Promise<void> {
    await this.delegate.delete({ where: { id } });
  }

  async count(where?: any): Promise<number> {
    return this.delegate.count({ where });
  }
}
```

**user.repository.ts:**
```typescript
import { Injectable } from '@nestjs/common';
import { User, Prisma } from '@prisma/client';
import { BaseRepository } from '../../common/repositories/base.repository';
import { PrismaService } from '../../prisma/prisma.service';

@Injectable()
export class UserRepository extends BaseRepository<
  User,
  Prisma.UserCreateInput,
  Prisma.UserUpdateInput
> {
  constructor(prisma: PrismaService) {
    super(prisma, 'user');
  }

  async findByEmail(email: string): Promise<User | null> {
    return this.prisma.user.findUnique({ where: { email } });
  }
}
```

**users.service.ts:**
```typescript
import { Injectable, NotFoundException } from '@nestjs/common';
import { UserRepository } from './repositories/user.repository';

@Injectable()
export class UsersService {
  constructor(private readonly userRepo: UserRepository) {}

  async findAll(page = 1, limit = 10) {
    const data = await this.userRepo.findAll((page - 1) * limit, limit);
    const total = await this.userRepo.count();
    return { data, total, page, limit };
  }

  async findById(id: number) {
    const user = await this.userRepo.findById(id);
    if (!user) throw new NotFoundException('User not found');
    return user;
  }

  async findByEmail(email: string) {
    return this.userRepo.findByEmail(email);
  }

  async create(data: { email: string; name: string; password: string }) {
    return this.userRepo.create(data);
  }

  async update(id: number, data: { name?: string }) {
    await this.findById(id);
    return this.userRepo.update(id, data);
  }

  async delete(id: number) {
    await this.findById(id);
    await this.userRepo.delete(id);
  }
}
```
