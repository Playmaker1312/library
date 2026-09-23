# 49 - Repository Pattern - Mengapa & Bagaimana di NestJS

## Penjelasan

Selama ini kita menggunakan PrismaService langsung di service (Pertemuan 45-48). Ini praktis, tapi menyebabkan **tight coupling** — service kita terikat langsung ke Prisma. Jika suatu saat kita ganti ORM (ke Drizzle, TypeORM, atau bahkan database lain), kita harus ubah semua service. Repository Pattern adalah lapisan abstraksi antara service dan database.

Jika PrismaService adalah **kontraktor pipa spesifik (merk tertentu)**, maka Repository adalah **keran standar internasional** — service tinggal pakai keran, tidak peduli pipa di balik tembok pakai merk apa. Kalau mau ganti kontraktor pipa, cukup ganti implementasi repository, service tidak perlu diotak-atik.

## Fungsi

- **Abstraksi data access**: Service tidak tahu database apa yang dipakai di belakang
- **Testability**: Repository mudah di-mock untuk unit test
- **Flexibility**: Bisa ganti ORM/database tanpa ubah business logic
- **Separation of concerns**: Tanggung jawab database terisolasi di repository
- **Interface contract**: Kontrak jelas antara service dan data layer

## Cara Pengimplementasian

### 1. Interface Repository

```typescript
// src/products/interfaces/product-repository.interface.ts
import { Product } from '@prisma/client';

export interface IProductRepository {
  findById(id: number): Promise<Product | null>;
  findAll(): Promise<Product[]>;
  create(data: { name: string; price: number }): Promise<Product>;
  update(id: number, data: Partial<Product>): Promise<Product>;
  delete(id: number): Promise<void>;
}
```

### 2. Implementasi dengan Prisma

```typescript
// src/products/repositories/prisma-product.repository.ts
import { Injectable } from '@nestjs/common';
import { Product } from '@prisma/client';
import { PrismaService } from '../../prisma/prisma.service';
import { IProductRepository } from '../interfaces/product-repository.interface';

@Injectable()
export class PrismaProductRepository implements IProductRepository {
  constructor(private prisma: PrismaService) {}

  async findById(id: number): Promise<Product | null> {
    return this.prisma.product.findUnique({ where: { id } });
  }

  async findAll(): Promise<Product[]> {
    return this.prisma.product.findMany();
  }

  async create(data: { name: string; price: number }): Promise<Product> {
    return this.prisma.product.create({ data });
  }

  async update(id: number, data: Partial<Product>): Promise<Product> {
    return this.prisma.product.update({ where: { id }, data });
  }

  async delete(id: number): Promise<void> {
    await this.prisma.product.delete({ where: { id } });
  }
}
```

### 3. Custom Provider di Module

```typescript
// src/products/products.module.ts
import { Module } from '@nestjs/common';
import { ProductsService } from './products.service';
import { ProductsController } from './products.controller';
import { PrismaProductRepository } from './repositories/prisma-product.repository';
import { IProductRepository } from './interfaces/product-repository.interface';

@Module({
  controllers: [ProductsController],
  providers: [
    ProductsService,
    {
      provide: 'IProductRepository', // token (bisa string atau class)
      useClass: PrismaProductRepository,
    },
  ],
})
export class ProductsModule {}
```

Bisa juga pakai simbol agar lebih aman:

```typescript
// src/products/repositories/product-repository.token.ts
export const PRODUCT_REPOSITORY = Symbol('PRODUCT_REPOSITORY');

// Di module:
{
  provide: PRODUCT_REPOSITORY,
  useClass: PrismaProductRepository,
}
```

### 4. Service Menggunakan Repository

```typescript
// src/products/products.service.ts
import { Inject, Injectable } from '@nestjs/common';
import { IProductRepository } from './interfaces/product-repository.interface';
import { PRODUCT_REPOSITORY } from './repositories/product-repository.token';

@Injectable()
export class ProductsService {
  constructor(
    @Inject(PRODUCT_REPOSITORY) private readonly productRepo: IProductRepository,
  ) {}

  async getAll() {
    return this.productRepo.findAll();
  }

  async getById(id: number) {
    const product = await this.productRepo.findById(id);
    if (!product) {
      throw new Error('Product not found');
    }
    return product;
  }

  async create(data: { name: string; price: number }) {
    return this.productRepo.create(data);
  }
}
```

### 5. Testing dengan Mock Repository

```typescript
// src/products/products.service.spec.ts
describe('ProductsService', () => {
  let service: ProductsService;
  let mockRepo: jest.Mocked<IProductRepository>;

  beforeEach(async () => {
    mockRepo = {
      findById: jest.fn(),
      findAll: jest.fn(),
      create: jest.fn(),
      update: jest.fn(),
      delete: jest.fn(),
    };

    const module = await Test.createTestingModule({
      providers: [
        ProductsService,
        { provide: PRODUCT_REPOSITORY, useValue: mockRepo },
      ],
    }).compile();

    service = module.get(ProductsService);
  });

  it('should return all products', async () => {
    mockRepo.findAll.mockResolvedValue([{ id: 1, name: 'Test', price: 100 }]);
    const result = await service.getAll();
    expect(result).toHaveLength(1);
  });
});
```

## Analogi

**Membangun Gedung Bertingkat**

- **Tanpa Repository** = **setiap ruangan punya pipa langsung ke PDAM** — ganti merk pipa? bongkar semua tembok
- **Interface IProductRepository** = **standar ukuran keran (SNI)** — semua merk keran harus comply
- **PrismaProductRepository** = **keran merk Prisma** — comply dengan standar SNI
- **Custom Provider** = **toko bangunan** yang memutuskan mau stok keran merk apa
- **Service hanya pakai keran** = **tukang ledeng tinggal pasang** — tidak peduli kerannya merk apa
- **Mock Repository** = **keran dummy untuk uji coba** — tidak perlu air beneran

## Dipakai untuk Apa

- Aplikasi besar yang mungkin ganti database di masa depan
- Tim yang ingin separation of concern yang jelas
- Unit testing tanpa database nyata
- Microservices dengan multiple database types

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Repository hanya wrapper tipis tanpa nilai tambah | Tambahkan logic bisnis database di repository |
| Interface terlalu spesifik ke Prisma | Jangan pakai tipe Prisma (`Prisma.ProductCreateInput`) di interface |
| Lupa daftarkan di module dengan provide token | Error `Nest can't resolve dependencies` |
| Tidak inject interface, malah langsung class | DI container perlu token — pakai `@Inject('TOKEN')` |
| Over-engineering untuk project kecil | Repository pattern berguna untuk project menengah-besar |

## Soal Latihan

1. Buat interface `IProductRepository` dengan method: findById, findAll, create, update, delete
2. Buat `PrismaProductRepository` yang mengimplementasikan interface tersebut
3. Daftarkan dengan custom provider di module
4. Gunakan di service dengan `@Inject`

### Jawaban

**product-repository.interface.ts:**
```typescript
export interface IProductRepository<T> {
  findById(id: number): Promise<T | null>;
  findAll(): Promise<T[]>;
  create(data: Partial<T>): Promise<T>;
  update(id: number, data: Partial<T>): Promise<T>;
  delete(id: number): Promise<void>;
}
```

**prisma-product.repository.ts:**
```typescript
import { Injectable } from '@nestjs/common';
import { Product } from '@prisma/client';
import { PrismaService } from '../../prisma/prisma.service';
import { IProductRepository } from '../interfaces/product-repository.interface';

@Injectable()
export class PrismaProductRepository implements IProductRepository<Product> {
  constructor(private prisma: PrismaService) {}

  async findById(id: number) {
    return this.prisma.product.findUnique({ where: { id } });
  }

  async findAll() {
    return this.prisma.product.findMany();
  }

  async create(data: Partial<Product>) {
    return this.prisma.product.create({ data: data as any });
  }

  async update(id: number, data: Partial<Product>) {
    return this.prisma.product.update({ where: { id }, data });
  }

  async delete(id: number) {
    await this.prisma.product.delete({ where: { id } });
  }
}
```

**products.module.ts:**
```typescript
import { Module } from '@nestjs/common';
import { ProductsController } from './products.controller';
import { ProductsService } from './products.service';
import { PrismaProductRepository } from './repositories/prisma-product.repository';

export const PRODUCT_REPOSITORY = Symbol('PRODUCT_REPOSITORY');

@Module({
  controllers: [ProductsController],
  providers: [
    ProductsService,
    { provide: PRODUCT_REPOSITORY, useClass: PrismaProductRepository },
  ],
})
export class ProductsModule {}
```

**products.service.ts:**
```typescript
import { Inject, Injectable } from '@nestjs/common';
import { Product } from '@prisma/client';
import { IProductRepository } from './interfaces/product-repository.interface';
import { PRODUCT_REPOSITORY } from './products.module';

@Injectable()
export class ProductsService {
  constructor(
    @Inject(PRODUCT_REPOSITORY) private readonly repo: IProductRepository<Product>,
  ) {}

  async findAll() { return this.repo.findAll(); }
  async findById(id: number) { return this.repo.findById(id); }
  async create(data: Partial<Product>) { return this.repo.create(data); }
  async update(id: number, data: Partial<Product>) { return this.repo.update(id, data); }
  async delete(id: number) { return this.repo.delete(id); }
}
```
