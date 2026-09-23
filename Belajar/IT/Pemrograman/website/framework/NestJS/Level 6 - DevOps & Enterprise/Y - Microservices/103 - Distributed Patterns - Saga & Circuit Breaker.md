# 103 - Distributed Patterns - Saga & Circuit Breaker

## Penjelasan
Di microservices, satu operasi bisnis bisa melibatkan banyak service. Misalnya **Order Flow**: Reserve Stock (Inventory Service) → Process Payment (Payment Service) → Confirm Order (Order Service). Jika payment gagal setelah stock di-reserve, kita harus mengembalikan stock. Inilah masalah **distributed transaction** — tidak ada rollback seperti di database monolith. **Saga pattern** dan **Circuit Breaker** adalah dua pola penting untuk mengatasinya.

## Fungsi
- **Saga pattern**: Distributed transaction dengan kompensasi — setiap langkah punya *compensating action* jika langkah berikutnya gagal.
- **Choreography vs Orchestration**: Choreography (event-driven, service saling memantau) vs Orchestration (coordinator service yang mengatur).
- **Circuit breaker**: Mencegah cascade failure — jika service downstream lambat/error, circuit breaker "memutus" aliran request sementara.
- **Cascade failure**: Satu service down → semua service yang bergantung padanya ikut down.

## Cara Pengimplementasian

### 1. Saga Pattern — Choreography (Event-Driven)

**Flow:**
```
Order Service → event 'order_created'
    ↓
Inventory Service → reserve stock → event 'stock_reserved' / 'stock_reserve_failed'
    ↓
Payment Service → process payment → event 'payment_success' / 'payment_failed'
    ↓
Order Service → confirm order → event 'order_confirmed'
```

**Inventory Service**
```typescript
@Injectable()
export class InventorySagaService {
  constructor(
    @Inject('ORDER_SERVICE') private orderClient: ClientProxy,
  ) {}

  @EventPattern('order_created')
  async handleOrderCreated(@Payload() data: { orderId: number; items: OrderItem[] }) {
    try {
      // Reserve stock
      for (const item of data.items) {
        await this.prisma.product.update({
          where: { id: item.productId },
          data: { stock: { decrement: item.quantity } },
        });
      }

      // Emit success
      this.orderClient.emit('stock_reserved', { orderId: data.orderId });
    } catch (error) {
      // Emit failure → trigger compensating action
      this.orderClient.emit('stock_reserve_failed', {
        orderId: data.orderId,
        reason: error.message,
      });
    }
  }

  // Compensating action: restore stock jika payment gagal
  @EventPattern('payment_failed')
  async handlePaymentFailed(@Payload() data: { orderId: number; items: OrderItem[] }) {
    for (const item of data.items) {
      await this.prisma.product.update({
        where: { id: item.productId },
        data: { stock: { increment: item.quantity } },
      });
    }
    console.log(`[Saga] Stock restored for order ${data.orderId}`);
  }
}
```

**Payment Service**
```typescript
@Injectable()
export class PaymentSagaService {
  constructor(
    @Inject('ORDER_SERVICE') private orderClient: ClientProxy,
  ) {}

  @EventPattern('stock_reserved')
  async handleStockReserved(@Payload() data: { orderId: number; amount: number }) {
    try {
      const result = await this.processPayment(data.orderId, data.amount);
      this.orderClient.emit('payment_success', { orderId: data.orderId, transactionId: result.id });
    } catch (error) {
      // Payment gagal → inventory harus restore stock (compensating)
      this.orderClient.emit('payment_failed', { orderId: data.orderId, reason: error.message });
    }
  }

  private async processPayment(orderId: number, amount: number) {
    // Integrasi payment gateway
    return { id: 'txn_123' };
  }
}
```

### 2. Saga Pattern — Orchestration (Central Coordinator)

**`apps/saga-coordinator/src/saga/saga-orchestrator.service.ts`**
```typescript
import { Injectable } from '@nestjs/common';

interface SagaStep {
  name: string;
  action: (data: any) => Promise<any>;
  compensate: (data: any) => Promise<any>;
}

@Injectable()
export class SagaOrchestratorService {
  async execute(steps: SagaStep[], context: any) {
    const executedSteps: { step: SagaStep; data: any }[] = [];

    for (const step of steps) {
      try {
        const result = await step.action(context);
        executedSteps.push({ step, data: result });
      } catch (error) {
        // Rollback: jalankan compensate untuk step yang sudah sukses
        console.log(`[Saga] Step ${step.name} failed. Rolling back...`);
        for (const executed of executedSteps.reverse()) {
          await executed.step.compensate(executed.data);
        }
        throw error;
      }
    }

    return context;
  }
}
```

**Implementasi Order Flow**
```typescript
@Injectable()
export class OrderOrchestrator {
  constructor(
    private saga: SagaOrchestratorService,
    private inventoryService: InventoryService,
    private paymentService: PaymentService,
  ) {}

  async createOrder(dto: CreateOrderDto) {
    const steps: SagaStep[] = [
      {
        name: 'reserveStock',
        action: (ctx) => this.inventoryService.reserve(ctx.items),
        compensate: (ctx) => this.inventoryService.restore(ctx.items),
      },
      {
        name: 'processPayment',
        action: (ctx) => this.paymentService.charge(ctx.total, ctx.paymentMethod),
        compensate: (ctx) => this.paymentService.refund(ctx.transactionId),
      },
      {
        name: 'confirmOrder',
        action: (ctx) => this.orderService.confirm(ctx.orderId),
        compensate: (ctx) => this.orderService.cancel(ctx.orderId),
      },
    ];

    return this.saga.execute(steps, {
      items: dto.items,
      total: dto.total,
      paymentMethod: dto.paymentMethod,
      orderId: generatedOrderId,
    });
  }
}
```

### 3. Circuit Breaker Pattern

**Implementasi Manual**
```typescript
import { Injectable } from '@nestjs/common';

enum CircuitState {
  CLOSED,    // Normal — request boleh lewat
  OPEN,      // Rusak — request langsung ditolak
  HALF_OPEN, // Setengah — tes satu request
}

@Injectable()
export class CircuitBreakerService {
  private state: CircuitState = CircuitState.CLOSED;
  private failureCount = 0;
  private successCount = 0;
  private lastFailureTime: number = 0;
  private readonly threshold = 5;           // 5 gagal → OPEN
  private readonly timeout = 30000;         // 30 detik → HALF_OPEN
  private readonly successThreshold = 3;    // 3 sukses → CLOSED

  async call<T>(serviceName: string, fn: () => Promise<T>): Promise<T> {
    if (this.state === CircuitState.OPEN) {
      if (Date.now() - this.lastFailureTime > this.timeout) {
        this.state = CircuitState.HALF_OPEN;
        console.log(`[CircuitBreaker] ${serviceName} → HALF_OPEN`);
      } else {
        throw new Error(`Circuit breaker OPEN for ${serviceName}`);
      }
    }

    try {
      const result = await fn();

      if (this.state === CircuitState.HALF_OPEN) {
        this.successCount++;
        if (this.successCount >= this.successThreshold) {
          this.state = CircuitState.CLOSED;
          this.failureCount = 0;
          this.successCount = 0;
          console.log(`[CircuitBreaker] ${serviceName} → CLOSED (recovered)`);
        }
      }

      return result;
    } catch (error) {
      this.failureCount++;
      this.lastFailureTime = Date.now();

      if (this.failureCount >= this.threshold) {
        this.state = CircuitState.OPEN;
        console.log(`[CircuitBreaker] ${serviceName} → OPEN (${this.failureCount} failures)`);
      }

      throw error;
    }
  }
}
```

**Integrasi dengan Service**
```typescript
@Injectable()
export class OrderService {
  constructor(private circuitBreaker: CircuitBreakerService) {}

  async getOrders() {
    return this.circuitBreaker.call('order-db', async () => {
      return this.prisma.order.findMany();
    });
  }

  async createOrder(dto: CreateOrderDto) {
    // Multiple circuit breaker calls
    await this.circuitBreaker.call('inventory-service', () =>
      this.inventoryClient.send({ cmd: 'reserve_stock' }, dto.items).toPromise(),
    );

    await this.circuitBreaker.call('payment-service', () =>
      this.paymentClient.send({ cmd: 'process_payment' }, dto.payment).toPromise(),
    );
  }
}
```

## Analogi
**Saga**: Di gedung, karyawan baru (order) harus:
1. Ambil laptop (Inventory — reserve) → jika gagal, stop
2. Aktivasi akun IT (Payment) → jika gagal, kembalikan laptop (compensate)
3. Beri meja (Confirm) → jika gagal, batalkan akun IT + kembalikan laptop

Jika langkah 2 gagal, kita tidak bisa "rollback" seperti Ctrl+Z — kita harus lakukan **aksi kompensasi** manual: kembalikan laptop. Inilah Saga.

**Circuit Breaker**: Lift rusak. Jika 5 orang mencoba lift dan gagal, satpam pasang **tanda "LIFT RUSAK"** (circuit OPEN). Tidak ada yang boleh coba lift selama 30 menit. Setelah 30 menit, satpam coba sekali (HALF_OPEN). Jika berhasil, buka lagi lift (CLOSED). Jika gagal, tutup lagi 30 menit. Ini mencegah 1000 orang mencoba lift rusak dan akhirnya tangga darurat juga ikut macet (cascade failure).

## Dipakai untuk apa
- Saga: E-commerce (order→payment→shipping), booking system (hotel→flight→car), financial transaction.
- Circuit Breaker: API yang bergantung pada service eksternal (payment gateway, SMS gateway, third-party API).
- Cascade failure prevention: Satu service lambat jangan buat semua service ikut lambat.

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Saga tanpa compensating action | Data inkonsisten — stock berkurang tapi payment gagal | Setiap langkah harus punya compensate |
| Orchestrator terlalu kompleks | Coordinator jadi single point of failure | Jaga orchestrator tetap sederhana, atau gunakan choreography |
| Circuit breaker timeout terlalu pendek | Service sehat dianggap gagal (false positive) | Set threshold berdasarkan SLA (misal: 5 gagal dalam 1 menit) |
| Tidak ada monitoring circuit breaker | Tidak tahu kalau circuit sedang OPEN | Ekspos state circuit breaker sebagai metric Prometheus |
| Lupa handle partial failure | Error tidak tertangani, state jadi inconsistent | Selalu wrap distributed call dengan try-catch |

## Soal Latihan

**Soal 1:** Implementasikan saga untuk **user registration flow**: 1) Create user di Auth DB, 2) Send welcome email via Email Service. Jika email gagal, user harus di-rollback.

**Jawaban 1:**
```typescript
// Choreography approach
// Auth Service emits 'user_created'
// Email Service listens → if fails, emit 'welcome_email_failed'
// Auth Service listens → delete user (compensate)

@EventPattern('welcome_email_failed')
async handleEmailFailed(@Payload() data: { userId: number }) {
  await this.prisma.user.delete({ where: { id: data.userId } });
  console.log(`[Saga] User ${data.userId} rolled back due to email failure`);
}
```

**Soal 2:** Implementasikan circuit breaker untuk external payment gateway. Jika gateway gagal 3 kali dalam 1 menit, circuit open selama 60 detik.

**Jawaban 2:**
```typescript
@Injectable()
export class PaymentCircuitBreaker {
  private failures: number[] = [];
  private openUntil: number = 0;

  async call(fn: () => Promise<any>) {
    if (Date.now() < this.openUntil) {
      throw new Error('Payment circuit breaker OPEN');
    }

    try {
      const result = await fn();
      this.failures = [];
      return result;
    } catch (error) {
      this.failures.push(Date.now());
      // Filter failures in last 1 minute
      this.failures = this.failures.filter(t => Date.now() - t < 60000);

      if (this.failures.length >= 3) {
        this.openUntil = Date.now() + 60000;
        console.log('[CircuitBreaker] Payment gateway → OPEN for 60s');
      }
      throw error;
    }
  }
}
```
