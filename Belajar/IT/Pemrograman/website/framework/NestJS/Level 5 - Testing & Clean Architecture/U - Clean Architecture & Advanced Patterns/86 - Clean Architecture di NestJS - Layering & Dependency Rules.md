# 86 - Clean Architecture di NestJS - Layering & Dependency Rules

## Penjelasan

Setelah kita menguasai testing (bab 81-85), saatnya menata struktur kode agar **testable**, **maintainable**, dan **independent dari framework/infrastruktur**. Selama ini kita menulis kode dengan struktur NestJS standar (module → controller → service → Prisma). Masalahnya: service kita **terikat erat** dengan Prisma, Express, dan NestJS itu sendiri.

**Clean Architecture** (Robert C. Martin) memecah aplikasi menjadi 4 lapisan:

1. **Domain Layer** (paling dalam) — entitas bisnis, value object, domain event, interface/repository contract
2. **Application Layer** — use case / interaktor, DTO, port interfaces
3. **Infrastructure Layer** — implementasi teknis (Prisma, email, S3, Redis)
4. **Presentation Layer** (paling luar) — controller, GraphQL resolver, WebSocket gateway

**Aturan utama (Dependency Rule):** kode di lapisan dalam TIDAK BOLEH tahu tentang lapisan luar. Domain tidak boleh import dari Infrastructure.

## Fungsi

- **Memisahkan business logic dari framework** — domain layer murni TypeScript, tanpa NestJS/Express/Prisma
- **Membuat kode testable** — use case bisa di-test tanpa mock Prisma, cukup mock interface
- **Memudahkan pergantian infrastruktur** — ganti Prisma ke Drizzle atau TypeORM cukup di 1 tempat (infrastructure)
- **Menerapkan Dependency Inversion** — layer dalam define interface, layer luar implementasi

## Cara Pengimplementasian

### Struktur Folder Clean Architecture

```
src/
├── domain/
│   ├── entities/
│   │   ├── product.entity.ts
│   │   └── order.entity.ts
│   ├── value-objects/
│   │   ├── price.ts
│   │   └── address.ts
│   ├── events/
│   │   ├── order-placed.event.ts
│   │   └── payment-received.event.ts
│   └── repositories/
│       ├── product.repository.interface.ts
│       └── order.repository.interface.ts
├── application/
│   ├── use-cases/
│   │   ├── product/
│   │   │   ├── create-product.use-case.ts
│   │   │   └── get-product.use-case.ts
│   │   └── order/
│   │       └── checkout.use-case.ts
│   └── dtos/
│       ├── create-product.dto.ts
│       └── checkout.dto.ts
├── infrastructure/
│   ├── persistence/
│   │   ├── prisma/
│   │   │   ├── prisma.service.ts
│   │   │   └── repositories/
│   │   │       ├── product-prisma.repository.ts
│   │   │       └── order-prisma.repository.ts
│   │   └── prisma.module.ts
│   ├── email/
│   │   ├── email.service.ts
│   │   └── email.module.ts
│   └── payment/
│       └── payment-gateway.ts
└── presentation/
    ├── controllers/
    │   ├── product.controller.ts
    │   └── order.controller.ts
    └── modules/
        ├── product.module.ts
        └── order.module.ts
```

### 1. Domain Layer — Entity & Interface

```typescript
// src/domain/entities/product.entity.ts
export class Product {
  constructor(
    public readonly id: string,
    public name: string,
    public description: string | null,
    private _price: number,
    private _stock: number,
    public readonly createdAt: Date,
    public updatedAt: Date,
  ) {}

  get price(): number {
    return this._price;
  }

  get stock(): number {
    return this._stock;
  }

  updatePrice(newPrice: number): void {
    if (newPrice <= 0) {
      throw new Error('Price must be positive');
    }
    this._price = newPrice;
    this.updatedAt = new Date();
  }

  reduceStock(quantity: number): void {
    if (quantity > this._stock) {
      throw new Error('Insufficient stock');
    }
    this._stock -= quantity;
    this.updatedAt = new Date();
  }

  increaseStock(quantity: number): void {
    this._stock += quantity;
    this.updatedAt = new Date();
  }
}
```

```typescript
// src/domain/value-objects/price.ts
export class Price {
  private constructor(private readonly _amount: number) {
    if (_amount < 0) {
      throw new Error('Price cannot be negative');
    }
  }

  static create(amount: number): Price {
    return new Price(amount);
  }

  get amount(): number {
    return this._amount;
  }

  add(other: Price): Price {
    return new Price(this._amount + other._amount);
  }

  multiply(quantity: number): Price {
    return new Price(this._amount * quantity);
  }

  equals(other: Price): boolean {
    return this._amount === other._amount;
  }
}
```

```typescript
// src/domain/repositories/product.repository.interface.ts
import { Product } from '../entities/product.entity';

export interface ProductRepository {
  findById(id: string): Promise<Product | null>;
  findAll(page: number, limit: number): Promise<[Product[], number]>;
  save(product: Product): Promise<Product>;
  update(product: Product): Promise<Product>;
  delete(id: string): Promise<void>;
}
```

### 2. Application Layer — Use Case & DTO

```typescript
// src/application/use-cases/product/create-product.use-case.ts
import { Injectable } from '@nestjs/common';
import { ProductRepository } from '../../../domain/repositories/product.repository.interface';
import { Product } from '../../../domain/entities/product.entity';
import { CreateProductDto } from '../../dtos/create-product.dto';

@Injectable()
export class CreateProductUseCase {
  constructor(
    private readonly productRepository: ProductRepository,
  ) {}

  async execute(dto: CreateProductDto): Promise<Product> {
    const product = new Product(
      crypto.randomUUID(),
      dto.name,
      dto.description ?? null,
      dto.price,
      dto.stock,
      new Date(),
      new Date(),
    );

    return this.productRepository.save(product);
  }
}
```

```typescript
// src/application/dtos/create-product.dto.ts
export class CreateProductDto {
  constructor(
    public readonly name: string,
    public readonly description: string | undefined,
    public readonly price: number,
    public readonly stock: number,
  ) {}
}
```

```typescript
// src/application/use-cases/order/checkout.use-case.ts
import { Injectable } from '@nestjs/common';
import { OrderRepository } from '../../../domain/repositories/order.repository.interface';
import { ProductRepository } from '../../../domain/repositories/product.repository.interface';
import { Order } from '../../../domain/entities/order.entity';
import { CheckoutDto } from '../../dtos/checkout.dto';
import { DomainEventPublisher } from '../../../domain/events/event-publisher.interface';

@Injectable()
export class CheckoutUseCase {
  constructor(
    private readonly orderRepository: OrderRepository,
    private readonly productRepository: ProductRepository,
    private readonly eventPublisher: DomainEventPublisher,
  ) {}

  async execute(dto: CheckoutDto): Promise<Order> {
    const items: Array<{ productId: string; quantity: number; price: number }> = [];

    for (const item of dto.items) {
      const product = await this.productRepository.findById(item.productId);
      if (!product) {
        throw new Error(`Product ${item.productId} not found`);
      }
      product.reduceStock(item.quantity);
      await this.productRepository.update(product);
      items.push({ productId: item.productId, quantity: item.quantity, price: product.price });
    }

    const order = new Order(
      crypto.randomUUID(),
      dto.userId,
      items,
      dto.shippingAddress,
      new Date(),
    );

    const savedOrder = await this.orderRepository.save(order);

    await this.eventPublisher.publish('order.placed', {
      orderId: savedOrder.id,
      userId: dto.userId,
      totalAmount: savedOrder.totalAmount,
    });

    return savedOrder;
  }
}
```

### 3. Infrastructure Layer — Implementasi Prisma

```typescript
// src/infrastructure/persistence/prisma/repositories/product-prisma.repository.ts
import { Injectable } from '@nestjs/common';
import { ProductRepository } from '../../../../domain/repositories/product.repository.interface';
import { Product } from '../../../../domain/entities/product.entity';
import { PrismaService } from '../prisma.service';

@Injectable()
export class ProductPrismaRepository implements ProductRepository {
  constructor(private readonly prisma: PrismaService) {}

  async findById(id: string): Promise<Product | null> {
    const record = await this.prisma.product.findUnique({ where: { id } });
    if (!record) return null;

    return new Product(
      record.id,
      record.name,
      record.description,
      record.price,
      record.stock,
      record.createdAt,
      record.updatedAt,
    );
  }

  async findAll(page: number, limit: number): Promise<[Product[], number]> {
    const [records, total] = await Promise.all([
      this.prisma.product.findMany({
        skip: (page - 1) * limit,
        take: limit,
      }),
      this.prisma.product.count(),
    ]);

    const products = records.map(
      (r) => new Product(r.id, r.name, r.description, r.price, r.stock, r.createdAt, r.updatedAt),
    );

    return [products, total];
  }

  async save(product: Product): Promise<Product> {
    const record = await this.prisma.product.create({
      data: {
        id: product.id,
        name: product.name,
        description: product.description,
        price: product.price,
        stock: product.stock,
        createdAt: product.createdAt,
        updatedAt: product.updatedAt,
      },
    });

    return new Product(
      record.id,
      record.name,
      record.description,
      record.price,
      record.stock,
      record.createdAt,
      record.updatedAt,
    );
  }

  async update(product: Product): Promise<Product> {
    const record = await this.prisma.product.update({
      where: { id: product.id },
      data: {
        name: product.name,
        description: product.description,
        price: product.price,
        stock: product.stock,
        updatedAt: product.updatedAt,
      },
    });

    return new Product(
      record.id,
      record.name,
      record.description,
      record.price,
      record.stock,
      record.createdAt,
      record.updatedAt,
    );
  }

  async delete(id: string): Promise<void> {
    await this.prisma.product.delete({ where: { id } });
  }
}
```

### 4. Presentation Layer — Controller & Module

```typescript
// src/presentation/controllers/product.controller.ts
import { Controller, Post, Body, Get, Param, Query } from '@nestjs/common';
import { CreateProductUseCase } from '../../application/use-cases/product/create-product.use-case';
import { GetProductUseCase } from '../../application/use-cases/product/get-product.use-case';
import { CreateProductDto } from '../../application/dtos/create-product.dto';

@Controller('products')
export class ProductController {
  constructor(
    private readonly createProductUseCase: CreateProductUseCase,
    private readonly getProductUseCase: GetProductUseCase,
  ) {}

  @Post()
  async create(@Body() dto: CreateProductDto) {
    const product = await this.createProductUseCase.execute(dto);
    return { id: product.id, name: product.name, price: product.price };
  }

  @Get(':id')
  async findById(@Param('id') id: string) {
    const product = await this.getProductUseCase.execute(id);
    return { id: product.id, name: product.name, price: product.price };
  }
}
```

```typescript
// src/presentation/modules/product.module.ts
import { Module } from '@nestjs/common';
import { ProductController } from '../controllers/product.controller';
import { CreateProductUseCase } from '../../application/use-cases/product/create-product.use-case';
import { GetProductUseCase } from '../../application/use-cases/product/get-product.use-case';
import { ProductRepository } from '../../domain/repositories/product.repository.interface';
import { ProductPrismaRepository } from '../../infrastructure/persistence/prisma/repositories/product-prisma.repository';
import { InfrastructureModule } from '../../infrastructure/infrastructure.module';

@Module({
  imports: [InfrastructureModule],
  controllers: [ProductController],
  providers: [
    CreateProductUseCase,
    GetProductUseCase,
    {
      provide: ProductRepository,
      useClass: ProductPrismaRepository,
    },
  ],
})
export class ProductModule {}
```

## Analogi — Gedung Bertingkat

| Lapisan | Analogi Gedung |
|---------|----------------|
| **Domain Layer** | Denah inti gedung: ukuran ruangan, posisi pintu, fungsi setiap lantai — tidak bergantung pada bahan bangunan |
| **Application Layer** | SOP keamanan: bagaimana petugas merespons kebakaran, bagaimana resepsionis menyambut tamu |
| **Infrastructure Layer** | Bahan fisik: beton, pipa, kabel, AC — bisa diganti tanpa mengubah denah |
| **Presentation Layer** | Tampilan luar: cat dinding, papan nama, pintu masuk — yang dilihat pengunjung |
| **Dependency Rule** | Denah inti tidak peduli apakah pipa terbuat dari PVC atau besi. Pipa boleh tahu denah, tapi denah tidak boleh tahu pipa |

## Dipakai untuk Apa

- **Aplikasi besar dengan tim > 5 developer** — clean architecture membagi tanggung jawab jelas
- **Microservices** — setiap service bisa punya domain masing-masing
- **Long-term project ( > 2 tahun)** — framework bisa berganti, business logic tetap
- **Test-heavy project** — domain entity bisa di-test tanpa infrastructure

## Kesalahan Umum yang Sering Terjadi

1. **Over-engineering** — untuk aplikasi CRUD sederhana dengan 3 tabel, clean architecture hanya menambah kerumitan; gunakan untuk modul yang benar-benar kompleks
2. **Domain entity anemic** — entity hanya berisi field getter/setter tanpa behavior; entity harus memiliki method bisnis (`reduceStock`, `updatePrice`)
3. **Use case terlalu besar** — satu use case melakukan 5 hal; setiap use case harus punya 1 tanggung jawab
4. **Infrastructure bocor ke domain** — domain entity menggunakan `@Column()` decorator atau import Prisma; ini melanggar dependency rule
5. **Duplicate mapping** — dari Prisma → Entity → DTO → Response; mapping berulang-ulang; gunakan mapper/util untuk mengurangi boilerplate

## Soal Latihan

### Soal 1: Refactor Module ke Clean Architecture

Refactor modul berikut ke clean architecture:

```typescript
// Service saat ini (NestJS style biasa)
@Injectable()
export class UserService {
  constructor(private prisma: PrismaService) {}

  async activateUser(userId: string) {
    const user = await this.prisma.user.findUnique({ where: { id: userId } });
    if (!user) throw new NotFoundException('User not found');
    if (user.isActive) throw new BadRequestException('Already active');

    await this.prisma.user.update({
      where: { id: userId },
      data: { isActive: true, updatedAt: new Date() },
    });

    await this.emailService.sendActivationEmail(user.email);
  }
}
```

Buatlah:
1. Domain entity `User` dengan method `activate()`
2. Interface `UserRepository`
3. Use case `ActivateUserUseCase`
4. Implementasi `UserPrismaRepository`
5. Controller yang menggunakan use case

### Jawaban 1:

```typescript
// === 1. Domain Entity ===
// src/domain/entities/user.entity.ts
export class User {
  constructor(
    public readonly id: string,
    public readonly email: string,
    public readonly name: string,
    private _isActive: boolean,
    public readonly createdAt: Date,
    public updatedAt: Date,
  ) {}

  get isActive(): boolean {
    return this._isActive;
  }

  activate(): void {
    if (this._isActive) {
      throw new Error('User is already active');
    }
    this._isActive = true;
    this.updatedAt = new Date();
  }
}

// === 2. Repository Interface ===
// src/domain/repositories/user.repository.interface.ts
import { User } from '../entities/user.entity';

export interface UserRepository {
  findById(id: string): Promise<User | null>;
  update(user: User): Promise<User>;
}

// === 3. Use Case ===
// src/application/use-cases/user/activate-user.use-case.ts
import { Injectable } from '@nestjs/common';
import { UserRepository } from '../../../domain/repositories/user.repository.interface';

@Injectable()
export class ActivateUserUseCase {
  constructor(
    private readonly userRepository: UserRepository,
  ) {}

  async execute(userId: string): Promise<User> {
    const user = await this.userRepository.findById(userId);
    if (!user) {
      throw new Error('User not found');
    }

    user.activate();
    return this.userRepository.update(user);
  }
}

// === 4. Prisma Repository Implementation ===
// src/infrastructure/persistence/prisma/repositories/user-prisma.repository.ts
import { Injectable } from '@nestjs/common';
import { UserRepository } from '../../../../domain/repositories/user.repository.interface';
import { User } from '../../../../domain/entities/user.entity';
import { PrismaService } from '../prisma.service';

@Injectable()
export class UserPrismaRepository implements UserRepository {
  constructor(private readonly prisma: PrismaService) {}

  async findById(id: string): Promise<User | null> {
    const record = await this.prisma.user.findUnique({ where: { id } });
    if (!record) return null;
    return new User(record.id, record.email, record.name, record.isActive, record.createdAt, record.updatedAt);
  }

  async update(user: User): Promise<User> {
    const record = await this.prisma.user.update({
      where: { id: user.id },
      data: { isActive: user.isActive, updatedAt: user.updatedAt },
    });
    return new User(record.id, record.email, record.name, record.isActive, record.createdAt, record.updatedAt);
  }
}

// === 5. Controller ===
// src/presentation/controllers/user.controller.ts
import { Controller, Param, Post } from '@nestjs/common';
import { ActivateUserUseCase } from '../../application/use-cases/user/activate-user.use-case';

@Controller('users')
export class UserController {
  constructor(private readonly activateUser: ActivateUserUseCase) {}

  @Post(':id/activate')
  async activate(@Param('id') id: string) {
    const user = await this.activateUser.execute(id);
    return { id: user.id, email: user.email, isActive: user.isActive };
  }
}
```
