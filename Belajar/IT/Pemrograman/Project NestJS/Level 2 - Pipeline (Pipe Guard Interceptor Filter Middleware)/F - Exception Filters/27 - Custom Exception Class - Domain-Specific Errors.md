# Custom Exception Class — Domain-Specific Errors

## Penjelasan (Nyambung dari materi sebelumnya)

HTTP exception bawaan (`NotFoundException`, `BadRequestException`) sudah cukup untuk 80% kasus. Tapi kadang kita butuh **exception yang lebih spesifik** untuk domain bisnis kita.

Daripada menebak-nebak "404 ini karena produk tidak ditemukan atau kategori tidak ditemukan?", custom exception memberi nama yang eksplisit dan bisa membawa data tambahan (error code, metadata).

---

## Fungsi

- Memberi nama exception yang jelas sesuai domain bisnis
- Membawa data tambahan (error code, field mana yang error, nilai yang conflict)
- Memudahkan testing — filter bisa mengecek `instanceof ProductNotFoundException`
- Standarisasi format error untuk semua error di domain yang sama
- Memisahkan kode error dari pesan user

---

## Cara Implementasi & Code

### 1. Custom Exception Dasar

```typescript
// src/common/exceptions/product-not-found.exception.ts
import { HttpException, HttpStatus } from '@nestjs/common';

export class ProductNotFoundException extends HttpException {
  constructor(productId: string) {
    super(
      {
        statusCode: HttpStatus.NOT_FOUND,
        message: `Produk dengan ID "${productId}" tidak ditemukan`,
        error: 'PRODUCT_NOT_FOUND',
        productId,
        timestamp: new Date().toISOString(),
      },
      HttpStatus.NOT_FOUND,
    );
  }
}
```

### 2. Custom Exception dengan Error Code

```typescript
// src/common/exceptions/insufficient-stock.exception.ts
import { HttpException, HttpStatus } from '@nestjs/common';

export class InsufficientStockException extends HttpException {
  constructor(
    productId: string,
    requested: number,
    available: number,
  ) {
    super(
      {
        statusCode: HttpStatus.UNPROCESSABLE_ENTITY,
        message: `Stok tidak mencukupi untuk produk "${productId}"`,
        error: 'INSUFFICIENT_STOCK',
        productId,
        requested,
        available,
        timestamp: new Date().toISOString(),
      },
      HttpStatus.UNPROCESSABLE_ENTITY,
    );
  }
}
```

### 3. OrderAlreadyCancelledException

```typescript
// src/common/exceptions/order-already-cancelled.exception.ts
import { HttpException, HttpStatus } from '@nestjs/common';

export class OrderAlreadyCancelledException extends HttpException {
  constructor(orderId: string) {
    super(
      {
        statusCode: HttpStatus.CONFLICT,
        message: `Order "${orderId}" sudah dibatalkan sebelumnya`,
        error: 'ORDER_ALREADY_CANCELLED',
        orderId,
        timestamp: new Date().toISOString(),
      },
      HttpStatus.CONFLICT,
    );
  }
}
```

### 4. Domain Exception — E-commerce

```typescript
// src/common/exceptions/domain.exceptions.ts
import { HttpException, HttpStatus } from '@nestjs/common';

export class EmailAlreadyRegisteredException extends HttpException {
  constructor(email: string) {
    super(
      {
        statusCode: HttpStatus.CONFLICT,
        message: `Email "${email}" sudah terdaftar`,
        error: 'EMAIL_ALREADY_REGISTERED',
        email,
        timestamp: new Date().toISOString(),
      },
      HttpStatus.CONFLICT,
    );
  }
}

export class InvalidOrderStateException extends HttpException {
  constructor(orderId: string, currentState: string, targetState: string) {
    super(
      {
        statusCode: HttpStatus.UNPROCESSABLE_ENTITY,
        message: `Order "${orderId}" tidak bisa diubah dari "${currentState}" ke "${targetState}"`,
        error: 'INVALID_ORDER_STATE',
        orderId,
        currentState,
        targetState,
        timestamp: new Date().toISOString(),
      },
      HttpStatus.UNPROCESSABLE_ENTITY,
    );
  }
}

export class PaymentFailedException extends HttpException {
  constructor(orderId: string, reason: string) {
    super(
      {
        statusCode: HttpStatus.PAYMENT_REQUIRED,
        message: `Pembayaran untuk order "${orderId}" gagal: ${reason}`,
        error: 'PAYMENT_FAILED',
        orderId,
        reason,
        timestamp: new Date().toISOString(),
      },
      HttpStatus.PAYMENT_REQUIRED,
    );
  }
}

export class DuplicateEntryException extends HttpException {
  constructor(entity: string, field: string, value: string) {
    super(
      {
        statusCode: HttpStatus.CONFLICT,
        message: `${entity} dengan ${field} "${value}" sudah ada`,
        error: 'DUPLICATE_ENTRY',
        entity,
        field,
        value,
        timestamp: new Date().toISOString(),
      },
      HttpStatus.CONFLICT,
    );
  }
}

export class UnauthorizedActionException extends HttpException {
  constructor(action: string, resource: string) {
    super(
      {
        statusCode: HttpStatus.FORBIDDEN,
        message: `Anda tidak diizinkan untuk ${action} ${resource}`,
        error: 'UNAUTHORIZED_ACTION',
        action,
        resource,
        timestamp: new Date().toISOString(),
      },
      HttpStatus.FORBIDDEN,
    );
  }
}
```

### 5. Penggunaan di Service

```typescript
// src/product/product.service.ts
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { Product } from './entities/product.entity';
import { ProductNotFoundException } from '../common/exceptions/product-not-found.exception';
import { InsufficientStockException } from '../common/exceptions/insufficient-stock.exception';
import { DuplicateEntryException } from '../common/exceptions/domain.exceptions';

@Injectable()
export class ProductService {
  constructor(
    @InjectRepository(Product)
    private readonly productRepo: Repository<Product>,
  ) {}

  async findOne(id: string): Promise<Product> {
    const product = await this.productRepo.findOne({ where: { id } });
    if (!product) {
      throw new ProductNotFoundException(id);
    }
    return product;
  }

  async purchase(id: string, quantity: number): Promise<Product> {
    const product = await this.findOne(id);

    if (product.stock < quantity) {
      throw new InsufficientStockException(id, quantity, product.stock);
    }

    if (product.status === 'inactive') {
      throw new UnauthorizedActionException('membeli', 'produk yang tidak aktif');
    }

    product.stock -= quantity;
    return this.productRepo.save(product);
  }

  async create(dto: CreateProductDto): Promise<Product> {
    const existing = await this.productRepo.findOne({
      where: { name: dto.name },
    });

    if (existing) {
      throw new DuplicateEntryException('Product', 'name', dto.name);
    }

    const product = this.productRepo.create(dto);
    return this.productRepo.save(product);
  }
}
```

### 6. Custom Exception — Base Class untuk Domain

Buat base class agar semua exception konsisten:

```typescript
// src/common/exceptions/domain.exception.ts
import { HttpException, HttpStatus } from '@nestjs/common';

export abstract class DomainException extends HttpException {
  constructor(
    public readonly errorCode: string,
    message: string,
    status: HttpStatus,
    public readonly metadata?: Record<string, any>,
  ) {
    super(
      {
        statusCode: status,
        message,
        error: errorCode,
        ...metadata,
        timestamp: new Date().toISOString(),
      },
      status,
    );
  }
}

// Usage
export class ProductNotFoundException extends DomainException {
  constructor(productId: string) {
    super(
      'PRODUCT_NOT_FOUND',
      `Produk dengan ID "${productId}" tidak ditemukan`,
      HttpStatus.NOT_FOUND,
      { productId },
    );
  }
}

export class InsufficientStockException extends DomainException {
  constructor(productId: string, requested: number, available: number) {
    super(
      'INSUFFICIENT_STOCK',
      `Stok tidak mencukupi`,
      HttpStatus.UNPROCESSABLE_ENTITY,
      { productId, requested, available },
    );
  }
}
```

---

## Analogi — Membangun Gedung

| Konsep | Analogi Gedung |
|--------|----------------|
| **HttpException** | Alarm kebakaran standar — bunyi kalau ada api |
| **NotFoundException** | Alarm "pintu tidak ditemukan" |
| **ProductNotFoundException** | Alarm spesifik: **"Pintu utama merek X tidak ditemukan"** — lebih jelas |
| **InsufficientStockException** | Alarm: **"Bata merah ukuran 20×20 hanya tersisa 100, Anda butuh 500"** |
| **DomainException base** | **Standar alarm perusahaan**: semua alarm harus punya format suara yang sama |
| **Error code** | Kode unik seperti `DOOR-404` — teknisi langsung tahu masalahnya |
| **Metadata** | Informasi tambahan: "pintu X, lantai 3, blok A" — lokasi persis masalah |

---

## Dipakai Untuk Apa

- **Product tidak ditemukan** — lebih spesifik daripada `NotFoundException` biasa
- **Stok tidak mencukupi** — bawa data requested vs available
- **Email/username duplikat** — bawa nilai yang conflict
- **State invalid** — order sudah dibayar, tidak bisa dicancel
- **Pembayaran gagal** — bawa reason gagal
- **Aksi tidak diizinkan** — bawa action dan resource
- **Semua error yang butuh metadata tambahan** untuk debugging

---

## Kesalahan Umum

1. **Membuat terlalu banyak exception class** — 1 exception per skenario → 50 file exception. Kelompokkan!
2. **Exception tidak extend HttpException** — tidak akan ditangani NestJS exception filter
3. **Mengabaikan error code** — client susah membedakan error di kode tanpa string error code
4. **Message tidak konsisten** — kadang bahasa Indonesia, kadang Inggris
5. **Tidak membawa metadata** — exception cuma punya message, programmer harus log manual
6. **Duplicate exception** — `ProductNotFoundException` dan `ProductNotFoundError` adalah hal yang sama
7. **HttpStatus salah** — `ProductNotFoundException` pakai 409 Conflict bukan 404

---

## Soal Latihan & Jawaban

### Soal

Buat 5 custom exception untuk aplikasi e-commerce:

1. `CartItemNotFoundException` — item di keranjang tidak ditemukan (404)
2. `CouponExpiredException` — kupon sudah kedaluwarsa (422)
3. `ReviewAlreadyExistsException` — user sudah review produk ini (409)
4. `ShippingAddressIncompleteException` — alamat pengiriman tidak lengkap (422)
5. `ProductOutOfStockException` — produk habis (422, beda dengan insufficient stock yang masih ada sisa)

### Jawaban

```typescript
// src/common/exceptions/cart-item-not-found.exception.ts
import { HttpException, HttpStatus } from '@nestjs/common';

export class CartItemNotFoundException extends HttpException {
  constructor(cartItemId: string) {
    super(
      {
        statusCode: HttpStatus.NOT_FOUND,
        message: `Item keranjang dengan ID "${cartItemId}" tidak ditemukan`,
        error: 'CART_ITEM_NOT_FOUND',
        cartItemId,
        timestamp: new Date().toISOString(),
      },
      HttpStatus.NOT_FOUND,
    );
  }
}
```

```typescript
// src/common/exceptions/coupon-expired.exception.ts
import { HttpException, HttpStatus } from '@nestjs/common';

export class CouponExpiredException extends HttpException {
  constructor(couponCode: string, expiredAt: Date) {
    super(
      {
        statusCode: HttpStatus.UNPROCESSABLE_ENTITY,
        message: `Kupon "${couponCode}" sudah kedaluwarsa pada ${expiredAt.toISOString()}`,
        error: 'COUPON_EXPIRED',
        couponCode,
        expiredAt: expiredAt.toISOString(),
        timestamp: new Date().toISOString(),
      },
      HttpStatus.UNPROCESSABLE_ENTITY,
    );
  }
}
```

```typescript
// src/common/exceptions/review-already-exists.exception.ts
import { HttpException, HttpStatus } from '@nestjs/common';

export class ReviewAlreadyExistsException extends HttpException {
  constructor(productId: string, userId: string) {
    super(
      {
        statusCode: HttpStatus.CONFLICT,
        message: `Anda sudah memberikan review untuk produk "${productId}"`,
        error: 'REVIEW_ALREADY_EXISTS',
        productId,
        userId,
        timestamp: new Date().toISOString(),
      },
      HttpStatus.CONFLICT,
    );
  }
}
```

```typescript
// src/common/exceptions/shipping-address-incomplete.exception.ts
import { HttpException, HttpStatus } from '@nestjs/common';

export class ShippingAddressIncompleteException extends HttpException {
  constructor(missingFields: string[]) {
    super(
      {
        statusCode: HttpStatus.UNPROCESSABLE_ENTITY,
        message: `Alamat pengiriman tidak lengkap. Field yang kurang: ${missingFields.join(', ')}`,
        error: 'SHIPPING_ADDRESS_INCOMPLETE',
        missingFields,
        timestamp: new Date().toISOString(),
      },
      HttpStatus.UNPROCESSABLE_ENTITY,
    );
  }
}
```

```typescript
// src/common/exceptions/product-out-of-stock.exception.ts
import { HttpException, HttpStatus } from '@nestjs/common';

export class ProductOutOfStockException extends HttpException {
  constructor(productId: string, productName: string) {
    super(
      {
        statusCode: HttpStatus.UNPROCESSABLE_ENTITY,
        message: `Produk "${productName}" sedang habis`,
        error: 'PRODUCT_OUT_OF_STOCK',
        productId,
        productName,
        timestamp: new Date().toISOString(),
      },
      HttpStatus.UNPROCESSABLE_ENTITY,
    );
  }
}
```
