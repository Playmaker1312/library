# 88 - Domain Events - Decoupling Antar Module

## Penjelasan

Setelah CQRS (bab 87) memperkenalkan EventBus untuk komunikasi internal modul, kini kita perluas konsep **Domain Events** untuk **decoupling antar module**. Dalam arsitektur monolith sekalipun, module-modul sering kali perlu bereaksi terhadap kejadian di module lain — misalnya:

- Order module: `OrderPlacedEvent`
- Payment module: `PaymentReceivedEvent`
- Notification module: kirim email saat order terbayar
- Inventory module: kurangi stok saat order dibayar

Tanpa domain events, module akan saling memanggil langsung: `OrderService` memanggil `EmailService` dan `InventoryService`. Ini menciptakan **tight coupling**. Dengan domain events, `OrderService` cukup publish `OrderPlacedEvent`, dan module lain yang berkepentingan akan mendengarkan.

## Fungsi

- **Decoupling antar module** — module tidak perlu tahu satu sama lain, cukup tahu event yang relevan
- **Extensibility** — menambah fitur baru cukup dengan membuat handler baru, tanpa mengubah kode yang ada
- **Scalability** — event handler bisa dijalankan secara asinkron (queue/background job)
- **Consistency** — event mewakili "sesuatu yang sudah terjadi", cocok untuk eventual consistency

## Cara Pengimplementasian

### 1. Definisi Domain Events (Shared)

```typescript
// src/shared/domain/events/order-placed.event.ts
export class OrderPlacedEvent {
  constructor(
    public readonly orderId: string,
    public readonly userId: string,
    public readonly totalAmount: number,
    public readonly items: Array<{ productId: string; quantity: number; price: number }>,
    public readonly occurredAt: Date = new Date(),
  ) {}
}
```

```typescript
// src/shared/domain/events/payment-received.event.ts
export class PaymentReceivedEvent {
  constructor(
    public readonly paymentId: string,
    public readonly orderId: string,
    public readonly amount: number,
    public readonly method: string,
    public readonly occurredAt: Date = new Date(),
  ) {}
}
```

```typescript
// src/shared/domain/events/order-shipped.event.ts
export class OrderShippedEvent {
  constructor(
    public readonly orderId: string,
    public readonly trackingNumber: string,
    public readonly occurredAt: Date = new Date(),
  ) {}
}
```

### 2. Event Publisher — Interface dan Implementasi

```typescript
// src/shared/domain/events/event-publisher.interface.ts
export interface EventPublisher {
  publish<T>(event: T): void;
  publishAll(events: any[]): void;
}
```

```typescript
// src/infrastructure/events/nestjs-event-publisher.ts
import { Injectable } from '@nestjs/common';
import { EventBus } from '@nestjs/cqrs';
import { EventPublisher } from '../../shared/domain/events/event-publisher.interface';

@Injectable()
export class NestjsEventPublisher implements EventPublisher {
  constructor(private readonly eventBus: EventBus) {}

  publish<T>(event: T): void {
    this.eventBus.publish(event);
  }

  publishAll(events: any[]): void {
    events.forEach((event) => this.eventBus.publish(event));
  }
}
```

### 3. Order Module — Publish Event

```typescript
// src/order/application/use-cases/place-order.use-case.ts
import { Injectable } from '@nestjs/common';
import { EventPublisher } from '../../../shared/domain/events/event-publisher.interface';
import { OrderPlacedEvent } from '../../../shared/domain/events/order-placed.event';

@Injectable()
export class PlaceOrderUseCase {
  constructor(
    private readonly orderRepository: OrderRepository,
    private readonly eventPublisher: EventPublisher,
  ) {}

  async execute(dto: PlaceOrderDto) {
    // ... business logic ...
    const order = await this.orderRepository.save(newOrder);

    // Publish event — tidak peduli siapa yang mendengarkan
    this.eventPublisher.publish(
      new OrderPlacedEvent(
        order.id,
        dto.userId,
        order.totalAmount,
        order.items.map((i) => ({
          productId: i.productId,
          quantity: i.quantity,
          price: i.price,
        })),
      ),
    );

    return order;
  }
}
```

### 4. Notification Module — Event Handler (Kirim Email)

```typescript
// src/notification/application/event-handlers/order-placed.handler.ts
import { EventsHandler, IEventHandler } from '@nestjs/cqrs';
import { OrderPlacedEvent } from '../../../shared/domain/events/order-placed.event';

@EventsHandler(OrderPlacedEvent)
export class OrderPlacedNotificationHandler implements IEventHandler<OrderPlacedEvent> {
  constructor(private readonly emailService: EmailService) {}

  async handle(event: OrderPlacedEvent) {
    // Kirim email konfirmasi ke user
    await this.emailService.send({
      to: event.userId, // akan di-resolve ke email
      subject: `Order #${event.orderId} Confirmed`,
      template: 'order-confirmation',
      data: {
        orderId: event.orderId,
        totalAmount: event.totalAmount,
        items: event.items,
      },
    });
  }
}
```

```typescript
// src/notification/application/event-handlers/payment-received.handler.ts
import { EventsHandler, IEventHandler } from '@nestjs/cqrs';
import { PaymentReceivedEvent } from '../../../shared/domain/events/payment-received.event';

@EventsHandler(PaymentReceivedEvent)
export class PaymentReceivedNotificationHandler implements IEventHandler<PaymentReceivedEvent> {
  constructor(private readonly emailService: EmailService) {}

  async handle(event: PaymentReceivedEvent) {
    await this.emailService.send({
      to: event.orderId, // akan di-resolve
      subject: `Payment Received for Order #${event.orderId}`,
      template: 'payment-receipt',
      data: {
        orderId: event.orderId,
        amount: event.amount,
        method: event.method,
      },
    });
  }
}
```

```typescript
// src/notification/notification.module.ts
import { Module } from '@nestjs/common';
import { CqrsModule } from '@nestjs/cqrs';
import { OrderPlacedNotificationHandler } from './application/event-handlers/order-placed.handler';
import { PaymentReceivedNotificationHandler } from './application/event-handlers/payment-received.handler';
import { EmailModule } from '../infrastructure/email/email.module';

@Module({
  imports: [CqrsModule, EmailModule],
  providers: [
    OrderPlacedNotificationHandler,
    PaymentReceivedNotificationHandler,
  ],
})
export class NotificationModule {}
```

### 5. Inventory Module — Event Handler (Update Stok)

```typescript
// src/inventory/application/event-handlers/payment-received.handler.ts
import { EventsHandler, IEventHandler } from '@nestjs/cqrs';
import { PaymentReceivedEvent } from '../../../shared/domain/events/payment-received.event';

@EventsHandler(PaymentReceivedEvent)
export class PaymentReceivedInventoryHandler implements IEventHandler<PaymentReceivedEvent> {
  constructor(private readonly inventoryService: InventoryService) {}

  async handle(event: PaymentReceivedEvent) {
    // Kurangi stok karena payment sudah diterima
    // Data items perlu didapatkan dari query ke order module
    // Atau lebih baik: OrderPlacedEvent sudah mengandung items
    console.log(`Reserving inventory for order ${event.orderId}`);
  }
}
```

```typescript
// src/inventory/application/event-handlers/order-placed.handler.ts
import { EventsHandler, IEventHandler } from '@nestjs/cqrs';
import { OrderPlacedEvent } from '../../../shared/domain/events/order-placed.event';

@EventsHandler(OrderPlacedEvent)
export class OrderPlacedInventoryHandler implements IEventHandler<OrderPlacedEvent> {
  constructor(private readonly inventoryService: InventoryService) {}

  async handle(event: OrderPlacedEvent) {
    // Langsung kurangi stok dari items di event
    for (const item of event.items) {
      await this.inventoryService.reduceStock(item.productId, item.quantity);
    }

    console.log(`Inventory updated for ${event.items.length} products`);
  }
}
```

### 6. Payment Module — Publish Event Sendiri

```typescript
// src/payment/application/use-cases/process-payment.use-case.ts
@Injectable()
export class ProcessPaymentUseCase {
  constructor(
    private readonly paymentGateway: PaymentGateway,
    private readonly eventPublisher: EventPublisher,
  ) {}

  async execute(dto: ProcessPaymentDto) {
    const payment = await this.paymentGateway.charge(dto.amount, dto.method);

    // Publish event setelah payment sukses
    this.eventPublisher.publish(
      new PaymentReceivedEvent(
        payment.id,
        dto.orderId,
        dto.amount,
        dto.method,
      ),
    );

    return payment;
  }
}
```

### 7. Unit Test — Domain Event Handler

```typescript
// src/notification/__tests__/order-placed-notification.handler.spec.ts
import { Test } from '@nestjs/testing';
import { OrderPlacedNotificationHandler } from '../application/event-handlers/order-placed.handler';
import { OrderPlacedEvent } from '../../shared/domain/events/order-placed.event';

describe('OrderPlacedNotificationHandler', () => {
  let handler: OrderPlacedNotificationHandler;
  let emailService: { send: jest.Mock };

  beforeEach(async () => {
    emailService = { send: jest.fn() };

    const module = await Test.createTestingModule({
      providers: [
        OrderPlacedNotificationHandler,
        { provide: EmailService, useValue: emailService },
      ],
    }).compile();

    handler = module.get(OrderPlacedNotificationHandler);
  });

  it('should send email when order is placed', async () => {
    const event = new OrderPlacedEvent(
      'order-1',
      'user-1',
      500000,
      [{ productId: 'prod-1', quantity: 2, price: 250000 }],
    );

    await handler.handle(event);

    expect(emailService.send).toHaveBeenCalledWith(
      expect.objectContaining({
        subject: expect.stringContaining('order-1'),
        template: 'order-confirmation',
      }),
    );
  });

  it('should handle event with empty items', async () => {
    const event = new OrderPlacedEvent('order-2', 'user-2', 0, []);

    await handler.handle(event);

    expect(emailService.send).toHaveBeenCalled();
  });
});
```

### 8. Struktur Module Decoupled

```
src/
├── order/
│   ├── domain/entities/
│   ├── application/use-cases/
│   └── presentation/order.controller.ts
├── payment/
│   ├── domain/entities/
│   ├── application/use-cases/
│   └── presentation/payment.controller.ts
├── notification/
│   ├── application/event-handlers/
│   │   ├── order-placed.handler.ts        ← mendengar dari order module
│   │   └── payment-received.handler.ts    ← mendengar dari payment module
│   └── notification.module.ts
├── inventory/
│   ├── application/event-handlers/
│   │   ├── order-placed.handler.ts        ← mendengar dari order module
│   │   └── payment-received.handler.ts    ← mendengar dari payment module
│   └── inventory.module.ts
└── shared/
    └── domain/events/
        ├── order-placed.event.ts
        ├── payment-received.event.ts
        └── order-shipped.event.ts
```

## Analogi — Gedung Bertingkat

| Konsep | Analogi Gedung |
|--------|----------------|
| **Domain Event** | Pengumuman lewat pengeras suara: "Kebakaran di lantai 3!" — siapa pun yang perlu bereaksi bisa bereaksi |
| **Event Publisher** | Orang yang berbicara ke mikrofon: tidak peduli siapa yang mendengar, yang penting pesan tersampaikan |
| **Event Handler (Email)** | Petugas pemadam kebakaran: langsung berlari ke lantai 3 |
| **Event Handler (Inventory)** | Petugas evakuasi: segera mengarahkan orang ke tangga darurat |
| **Tight Coupling (tanpa event)** | Petugas pemadam harus menunggu perintah langsung dari komandan, yang harus mengecek satu per satu siapa yang harus dipanggil |
| **Decoupling (dengan event)** | Komandan cukup teriak "KEBAKARAN!", semua yang berkepentingan sudah tahu tugasnya masing-masing |

## Dipakai untuk Apa

- **Notifikasi** — kirim email/SMS/push notification saat event terjadi
- **Integrasi dengan sistem eksternal** — update ERP, kirim ke warehouse, sinkronisasi ke search engine
- **Audit log** — catat setiap event yang terjadi untuk kepentingan audit
- **Eventually consistent update** — update stok, update score, update cache

## Kesalahan Umum yang Sering Terjadi

1. **Event terlalu banyak handler** — satu event ditangani oleh 15 handler; jika satu gagal, seluruh transaksi gagal. Gunakan `CqrsModule` dengan `UnhandledExceptionBus` atau implementasikan outbox pattern
2. **Handler melakukan terlalu banyak** — handler mengirim email dan update database dan panggil API eksternal. Pisahkan jadi handler terpisah yang masing-masing fokus satu tanggung jawab
3. **Event tidak idempotent** — handler dipanggil dua kali (misal retry) menyebabkan duplikasi; pastikan handler idempotent
4. **Shared event di module yang salah** — event didefinisikan di `order/domain/events/` sehingga module lain harus import dari order module; letakkan di `shared/domain/events/`
5. **Event terlalu besar atau terlalu kecil** — `OrderPlacedEvent` berisi seluruh detail order vs hanya orderId. Balance: kirim cukup data agar handler tidak perlu query lagi, tapi jangan kirim 50 field jika handler hanya butuh 3

## Soal Latihan

### Soal 1: Implementasikan Domain Event

Buat implementasi domain event untuk skenario:
1. `UserRegisteredEvent` — dipublish saat user mendaftar
2. `WelcomeEmailHandler` — kirim email selamat datang (notification module)
3. `AssignDefaultRoleHandler` — assign role default ke user (auth module)
4. `AdminNotificationHandler` — notifikasi admin ada user baru (notification module)

### Jawaban 1:

```typescript
// === SHARED EVENT ===
// src/shared/domain/events/user-registered.event.ts
export class UserRegisteredEvent {
  constructor(
    public readonly userId: string,
    public readonly email: string,
    public readonly name: string,
    public readonly occurredAt: Date = new Date(),
  ) {}
}

// === NOTIFICATION MODULE — Welcome Email ===
// src/notification/application/event-handlers/user-registered-welcome.handler.ts
import { EventsHandler, IEventHandler } from '@nestjs/cqrs';
import { UserRegisteredEvent } from '../../../shared/domain/events/user-registered.event';

@EventsHandler(UserRegisteredEvent)
export class WelcomeEmailHandler implements IEventHandler<UserRegisteredEvent> {
  constructor(private readonly emailService: EmailService) {}

  async handle(event: UserRegisteredEvent) {
    await this.emailService.send({
      to: event.email,
      subject: `Welcome to our platform, ${event.name}!`,
      template: 'welcome-email',
      data: { name: event.name },
    });
  }
}

// === AUTH MODULE — Assign Default Role ===
// src/auth/application/event-handlers/user-registered-role.handler.ts
import { EventsHandler, IEventHandler } from '@nestjs/cqrs';
import { UserRegisteredEvent } from '../../../shared/domain/events/user-registered.event';

@EventsHandler(UserRegisteredEvent)
export class AssignDefaultRoleHandler implements IEventHandler<UserRegisteredEvent> {
  constructor(private readonly roleService: RoleService) {}

  async handle(event: UserRegisteredEvent) {
    await this.roleService.assignRole(event.userId, 'CUSTOMER');
  }
}

// === NOTIFICATION MODULE — Notify Admin ===
// src/notification/application/event-handlers/user-registered-admin.handler.ts
import { EventsHandler, IEventHandler } from '@nestjs/cqrs';
import { UserRegisteredEvent } from '../../../shared/domain/events/user-registered.event';

@EventsHandler(UserRegisteredEvent)
export class AdminNotificationHandler implements IEventHandler<UserRegisteredEvent> {
  constructor(private readonly emailService: EmailService) {}

  async handle(event: UserRegisteredEvent) {
    await this.emailService.send({
      to: 'admin@platform.com',
      subject: 'New user registered',
      template: 'admin-new-user',
      data: {
        userId: event.userId,
        email: event.email,
        name: event.name,
      },
    });
  }
}

// === AUTH MODULE — Publish Event Saat Register ===
// src/auth/application/use-cases/register.use-case.ts
@Injectable()
export class RegisterUseCase {
  constructor(
    private readonly authRepository: AuthRepository,
    private readonly eventPublisher: EventPublisher,
  ) {}

  async execute(dto: RegisterDto) {
    const user = await this.authRepository.create(dto);

    this.eventPublisher.publish(
      new UserRegisteredEvent(user.id, user.email, user.name),
    );

    return user;
  }
}
```
