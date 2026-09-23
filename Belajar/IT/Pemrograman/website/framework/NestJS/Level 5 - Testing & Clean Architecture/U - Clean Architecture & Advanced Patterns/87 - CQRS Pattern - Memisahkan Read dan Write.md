# 87 - CQRS Pattern - Memisahkan Read dan Write

## Penjelasan

Setelah menerapkan Clean Architecture (bab 86) yang memisahkan kode berdasarkan lapisan, kini kita ekstrem dengan **CQRS (Command Query Responsibility Segregation)** yang memisahkan model **Read** dan **Write** secara total.

Konsep ini lahir dari pengamatan sederhana: **operasi baca (query) dan operasi tulis (command) memiliki kebutuhan yang berbeda**:
- **Command** (create, update, delete) — butuh validasi ketat, transaksi, domain logic, event
- **Query** (list, detail, search) — butuh kecepatan, aggregasi, denormalisasi, caching

Dengan CQRS, kita menggunakan `@nestjs/cqrs` yang menyediakan **CommandBus**, **QueryBus**, dan **EventBus**.

Perbedaan dengan arsitektur sebelumnya:
- Clean Architecture: pisah **layer** (domain, application, infrastructure, presentation)
- CQRS: pisah **model** (command model vs query model) — bisa diterapkan di dalam application layer Clean Architecture

## Fungsi

- **Memisahkan command (ubah state) dan query (baca state)** — masing-masing punya model optimal sendiri
- **Menggunakan CommandBus/QueryBus/EventBus** dari `@nestjs/cqrs`
- **Command** — satu command satu use case, return void atau id
- **Query** — satu query satu return data, tanpa efek samping
- **Event** — sesuatu yang sudah terjadi, bisa trigger handler lain secara asinkron

## Cara Pengimplementasian

### 1. Install dan Setup CQRS Module

```typescript
// src/order/order.module.ts
import { Module } from '@nestjs/common';
import { CqrsModule } from '@nestjs/cqrs';
import { OrderController } from './presentation/order.controller';
import { PrismaModule } from '../infrastructure/prisma/prisma.module';
import { CreateOrderHandler } from './application/commands/create-order.handler';
import { GetOrderHandler } from './application/queries/get-order.handler';
import { OrderPlacedHandler } from './application/events/order-placed.handler';
import { ORDER_REPOSITORY } from './domain/repositories/order.repository.interface';
import { OrderPrismaRepository } from './infrastructure/order-prisma.repository';

const CommandHandlers = [CreateOrderHandler];
const QueryHandlers = [GetOrderHandler];
const EventHandlers = [OrderPlacedHandler];

@Module({
  imports: [CqrsModule, PrismaModule],
  controllers: [OrderController],
  providers: [
    {
      provide: ORDER_REPOSITORY,
      useClass: OrderPrismaRepository,
    },
    ...CommandHandlers,
    ...QueryHandlers,
    ...EventHandlers,
  ],
})
export class OrderModule {}
```

### 2. Command — Membuat Order

```typescript
// src/order/application/commands/create-order.command.ts
import { ICommand } from '@nestjs/cqrs';

export class CreateOrderCommand implements ICommand {
  constructor(
    public readonly userId: string,
    public readonly items: Array<{ productId: string; quantity: number }>,
    public readonly shippingAddress: string,
    public readonly paymentMethod: string,
  ) {}
}
```

```typescript
// src/order/application/commands/create-order.handler.ts
import { CommandHandler, ICommandHandler, EventBus } from '@nestjs/cqrs';
import { Inject } from '@nestjs/common';
import { CreateOrderCommand } from './create-order.command';
import { ORDER_REPOSITORY, OrderRepository } from '../../domain/repositories/order.repository.interface';
import { Order } from '../../domain/entities/order.entity';
import { OrderPlacedEvent } from '../../domain/events/order-placed.event';

@CommandHandler(CreateOrderCommand)
export class CreateOrderHandler implements ICommandHandler<CreateOrderCommand> {
  constructor(
    @Inject(ORDER_REPOSITORY)
    private readonly orderRepository: OrderRepository,
    private readonly eventBus: EventBus,
  ) {}

  async execute(command: CreateOrderCommand): Promise<string> {
    const order = new Order(
      crypto.randomUUID(),
      command.userId,
      command.items.map((item) => ({
        productId: item.productId,
        quantity: item.quantity,
        price: 0, // akan di-set oleh repository/saga
      })),
      command.shippingAddress,
      'PENDING',
      new Date(),
    );

    const savedOrder = await this.orderRepository.save(order);

    // Publish event setelah command berhasil
    this.eventBus.publish(
      new OrderPlacedEvent(
        savedOrder.id,
        command.userId,
        savedOrder.totalAmount,
        command.paymentMethod,
      ),
    );

    return savedOrder.id;
  }
}
```

### 3. Query — Mendapatkan Order

```typescript
// src/order/application/queries/get-order.query.ts
import { IQuery } from '@nestjs/cqrs';

export class GetOrderQuery implements IQuery {
  constructor(public readonly orderId: string) {}
}
```

```typescript
// src/order/application/queries/get-order.handler.ts
import { QueryHandler, IQueryHandler } from '@nestjs/cqrs';
import { Inject } from '@nestjs/common';
import { GetOrderQuery } from './get-order.query';
import { ORDER_REPOSITORY, OrderRepository } from '../../domain/repositories/order.repository.interface';

interface OrderResponse {
  id: string;
  userId: string;
  status: string;
  totalAmount: number;
  items: Array<{ productId: string; quantity: number; price: number }>;
  createdAt: Date;
}

@QueryHandler(GetOrderQuery)
export class GetOrderHandler implements IQueryHandler<GetOrderQuery> {
  constructor(
    @Inject(ORDER_REPOSITORY)
    private readonly orderRepository: OrderRepository,
  ) {}

  async execute(query: GetOrderQuery): Promise<OrderResponse> {
    const order = await this.orderRepository.findById(query.orderId);
    if (!order) {
      throw new Error('Order not found');
    }

    return {
      id: order.id,
      userId: order.userId,
      status: order.status,
      totalAmount: order.totalAmount,
      items: order.items,
      createdAt: order.createdAt,
    };
  }
}
```

### 4. Event — Order Placed

```typescript
// src/order/domain/events/order-placed.event.ts
import { IEvent } from '@nestjs/cqrs';

export class OrderPlacedEvent implements IEvent {
  constructor(
    public readonly orderId: string,
    public readonly userId: string,
    public readonly totalAmount: number,
    public readonly paymentMethod: string,
  ) {}
}
```

```typescript
// src/order/application/events/order-placed.handler.ts
import { EventsHandler, IEventHandler } from '@nestjs/cqrs';
import { OrderPlacedEvent } from '../../domain/events/order-placed.event';

@EventsHandler(OrderPlacedEvent)
export class OrderPlacedHandler implements IEventHandler<OrderPlacedEvent> {
  async handle(event: OrderPlacedEvent) {
    // Kirim email konfirmasi
    console.log(`Sending confirmation email for order ${event.orderId}`);

    // Update inventory
    console.log(`Reserving inventory for order ${event.orderId}`);

    // Notifikasi admin
    console.log(`Notifying admin about new order ${event.orderId}`);
  }
}
```

### 5. Controller — Menghubungkan Command dan Query

```typescript
// src/order/presentation/order.controller.ts
import { Controller, Post, Body, Get, Param } from '@nestjs/common';
import { CommandBus, QueryBus } from '@nestjs/cqrs';
import { CreateOrderCommand } from '../application/commands/create-order.command';
import { GetOrderQuery } from '../application/queries/get-order.query';

@Controller('orders')
export class OrderController {
  constructor(
    private readonly commandBus: CommandBus,
    private readonly queryBus: QueryBus,
  ) {}

  @Post()
  async create(@Body() dto: any) {
    const orderId = await this.commandBus.execute(
      new CreateOrderCommand(
        dto.userId,
        dto.items,
        dto.shippingAddress,
        dto.paymentMethod,
      ),
    );

    return { orderId };
  }

  @Get(':id')
  async findById(@Param('id') id: string) {
    return this.queryBus.execute(new GetOrderQuery(id));
  }
}
```

### 6. Query Terpisah untuk Read Model (Optimized)

```typescript
// src/order/application/queries/get-order-summary.query.ts
export class GetOrderSummaryQuery implements IQuery {
  constructor(
    public readonly userId: string,
    public readonly page: number,
    public readonly limit: number,
  ) {}
}

// Handler dengan query optimized (Prisma raw query atau view terpisah)
@QueryHandler(GetOrderSummaryQuery)
export class GetOrderSummaryHandler implements IQueryHandler<GetOrderSummaryQuery> {
  constructor(private readonly prisma: PrismaService) {}

  async execute(query: GetOrderSummaryQuery) {
    // Query optimized untuk read — bisa pake raw SQL, materialized view, atau cache
    const [orders, total] = await Promise.all([
      this.prisma.order.findMany({
        where: { userId: query.userId },
        select: {
          id: true,
          status: true,
          totalAmount: true,
          createdAt: true,
          items: {
            select: {
              quantity: true,
              product: { select: { name: true } },
            },
          },
        },
        skip: (query.page - 1) * query.limit,
        take: query.limit,
        orderBy: { createdAt: 'desc' },
      }),
      this.prisma.order.count({ where: { userId: query.userId } }),
    ]);

    return { data: orders, meta: { total, page: query.page, limit: query.limit } };
  }
}
```

### 7. Saga — Orchestrating Multiple Commands

```typescript
// src/order/application/sagas/order.saga.ts
import { Injectable } from '@nestjs/common';
import { ICommand, Saga, ofType } from '@nestjs/cqrs';
import { Observable, map, delay } from 'rxjs';
import { OrderPlacedEvent } from '../../domain/events/order-placed.event';
import { ProcessPaymentCommand } from '../../payment/application/commands/process-payment.command';

@Injectable()
export class OrderSaga {
  @Saga()
  orderPlaced = (events$: Observable<any>): Observable<ICommand> => {
    return events$.pipe(
      ofType(OrderPlacedEvent),
      delay(1000), // Simulasi delay
      map(
        (event) =>
          new ProcessPaymentCommand(
            event.orderId,
            event.totalAmount,
            event.paymentMethod,
          ),
      ),
    );
  };
}
```

## Analogi — Gedung Bertingkat

| Konsep | Analogi Gedung |
|--------|----------------|
| **Command (Write)** | Tukang bangunan yang memasang bata, mengecat dinding, memasang pipa — mengubah keadaan gedung |
| **Query (Read)** | Pengawas yang melihat cetak biru, memeriksa ukuran, menghitung jumlah bata — hanya membaca, tidak mengubah |
| **CommandBus** | Komandan lapangan yang menerima perintah "pasang pintu di lantai 3" dan menugaskan tukang yang tepat |
| **QueryBus** | Petugas arsip yang melayani permintaan "berapa banyak bata yang sudah terpasang?" |
| **EventBus** | Pengeras suara: "ORDER PLAED!" — semua orang yang berkepentingan bisa mendengar dan bereaksi |
| **Saga** | Mandor yang mengawal urutan: pasang pondasi → tunggu kering → pasang tiang → tunggu kering → pasang dinding |
| **Read Model** | Maket gedung — ringan, cepat dilihat, tidak harus detail 100% seperti aslinya |

## Dipakai untuk Apa

- **Aplikasi dengan kompleksitas tinggi** — banyak validasi, transaksi, event, dan multiple side effects
- **Event-driven architecture** — microservices yang berkomunikasi via event
- **Read-heavy application** — dashboard, laporan, analytics; query model bisa di-cache dan di-denormalisasi
- **Audit log** — setiap command bisa dicatat, event membentuk history tak terubah

## Kesalahan Umum yang Sering Terjadi

1. **CQRS untuk CRUD sederhana** — membuat command dan query untuk create-read-update-delete standar hanya menambah boilerplate tanpa manfaat
2. **Command mengembalikan data** — command seharusnya return void atau id saja; jika perlu data, lakukan query terpisah
3. **Event handler melakukan terlalu banyak** — handler mengirim email, update inventory, dan kirim notifikasi dalam satu handler; pisahkan jadi handler terpisah
4. **Lupa daftarkan handler di module** — handler tidak terdaftar di `providers`, sehingga event tidak diproses
5. **Saga tidak handle error** — jika `ProcessPaymentCommand` gagal, order harus di-rollback; implementasikan compensating transaction di saga

## Soal Latihan

### Soal 1: Implementasikan CQRS untuk CartModule

Buat CQRS untuk modul keranjang belanja dengan:
1. `AddItemCommand` — menambahkan item ke cart
2. `RemoveItemCommand` — menghapus item dari cart
3. `GetCartQuery` — mendapatkan isi cart
4. `CartUpdatedEvent` — dipublish saat cart berubah

### Jawaban 1:

```typescript
// === 1. COMMAND: Add Item ===
// cart/application/commands/add-item.command.ts
export class AddItemCommand implements ICommand {
  constructor(
    public readonly userId: string,
    public readonly productId: string,
    public readonly quantity: number,
  ) {}
}

// cart/application/commands/add-item.handler.ts
@CommandHandler(AddItemCommand)
export class AddItemHandler implements ICommandHandler<AddItemCommand> {
  constructor(
    @Inject(CART_REPOSITORY)
    private readonly cartRepository: CartRepository,
    private readonly eventBus: EventBus,
  ) {}

  async execute(command: AddItemCommand): Promise<void> {
    let cart = await this.cartRepository.findByUserId(command.userId);
    if (!cart) {
      cart = new Cart(crypto.randomUUID(), command.userId, []);
    }

    cart.addItem(command.productId, command.quantity);
    await this.cartRepository.save(cart);
    this.eventBus.publish(new CartUpdatedEvent(cart.id, command.userId));
  }
}

// === 2. COMMAND: Remove Item ===
// cart/application/commands/remove-item.command.ts
export class RemoveItemCommand implements ICommand {
  constructor(
    public readonly userId: string,
    public readonly productId: string,
  ) {}
}

// cart/application/commands/remove-item.handler.ts
@CommandHandler(RemoveItemCommand)
export class RemoveItemHandler implements ICommandHandler<RemoveItemCommand> {
  constructor(
    @Inject(CART_REPOSITORY)
    private readonly cartRepository: CartRepository,
    private readonly eventBus: EventBus,
  ) {}

  async execute(command: RemoveItemCommand): Promise<void> {
    const cart = await this.cartRepository.findByUserId(command.userId);
    if (!cart) return;

    cart.removeItem(command.productId);
    await this.cartRepository.save(cart);
    this.eventBus.publish(new CartUpdatedEvent(cart.id, command.userId));
  }
}

// === 3. QUERY: Get Cart ===
// cart/application/queries/get-cart.query.ts
export class GetCartQuery implements IQuery {
  constructor(public readonly userId: string) {}
}

// cart/application/queries/get-cart.handler.ts
@QueryHandler(GetCartQuery)
export class GetCartHandler implements IQueryHandler<GetCartQuery> {
  constructor(
    @Inject(CART_REPOSITORY)
    private readonly cartRepository: CartRepository,
  ) {}

  async execute(query: GetCartQuery) {
    const cart = await this.cartRepository.findByUserId(query.userId);
    if (!cart) return { items: [], totalAmount: 0 };
    return { id: cart.id, items: cart.items, totalAmount: cart.totalAmount };
  }
}

// === 4. EVENT: Cart Updated ===
// cart/domain/events/cart-updated.event.ts
export class CartUpdatedEvent implements IEvent {
  constructor(
    public readonly cartId: string,
    public readonly userId: string,
  ) {}
}

// === 5. CONTROLLER ===
@Controller('cart')
export class CartController {
  constructor(
    private readonly commandBus: CommandBus,
    private readonly queryBus: QueryBus,
  ) {}

  @Post('items')
  async addItem(@Body() dto: any) {
    await this.commandBus.execute(
      new AddItemCommand(dto.userId, dto.productId, dto.quantity),
    );
    return { message: 'Item added to cart' };
  }

  @Delete('items/:productId')
  async removeItem(@Param('productId') productId: string, @Body('userId') userId: string) {
    await this.commandBus.execute(new RemoveItemCommand(userId, productId));
    return { message: 'Item removed from cart' };
  }

  @Get()
  async getCart(@Query('userId') userId: string) {
    return this.queryBus.execute(new GetCartQuery(userId));
  }
}
```
