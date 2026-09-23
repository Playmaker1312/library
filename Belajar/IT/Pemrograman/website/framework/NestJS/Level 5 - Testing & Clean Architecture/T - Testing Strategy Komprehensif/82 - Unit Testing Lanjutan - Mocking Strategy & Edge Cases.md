# 82 - Unit Testing Lanjutan - Mocking Strategy & Edge Cases

## Penjelasan

Setelah piramida testing ditegakkan (bab 81), sekarang kita masuk ke lapisan paling bawah dan paling banyak: **unit test**. Jika sebelumnya kita menggunakan factory functions dan Testcontainers untuk integration test, di unit test kita justru **menghindari** database asli. Kita menggantinya dengan **mock** dan **stub** agar test berjalan sangat cepat (ms-level) dan benar-benar terisolasi.

Tantangan di level ini bukan menulis test untuk happy path, melainkan **menulis test untuk edge cases** — null, undefined, array kosong, nilai ekstrem, dan error handling. Service yang baik harus tetap behave meskipun inputnya kacau.

## Fungsi

- Menggunakan **jest-mock-extended** untuk mocking Prisma dengan type safety
- Memahami perbedaan **Spy vs Mock vs Stub**
- Menulis test untuk **edge cases** (null/undefined/array kosong/extreme values)
- Menggunakan **snapshot testing** untuk memastikan output tidak berubah secara tidak sengaja

## Cara Pengimplementasian

### 1. Setup jest-mock-extended untuk Prisma Mock

```typescript
// src/__tests__/mocks/prisma.mock.ts
import { mockDeep, mockReset } from 'jest-mock-extended';
import { PrismaClient } from '@prisma/client';
import { DeepMockProxy } from 'jest-mock-extended';

export const prismaMock = mockDeep<PrismaClient>() as unknown as DeepMockProxy<PrismaClient>;

beforeEach(() => {
  mockReset(prismaMock);
});
```

```typescript
// src/__tests__/providers/prisma-mock-provider.ts
import { Provider } from '@nestjs/common';
import { PrismaService } from '../../prisma/prisma.service';
import { prismaMock } from '../mocks/prisma.mock';

export const PrismaMockProvider: Provider = {
  provide: PrismaService,
  useValue: prismaMock,
};
```

### 2. Spy vs Mock vs Stub

```typescript
// Contoh perbedaan dalam satu test suite
import { jest } from '@jest/globals';

class EmailService {
  send(to: string, subject: string, body: string): boolean {
    console.log(`Sending email to ${to}`);
    return true;
  }
}

describe('Spy vs Mock vs Stub', () => {
  // STUB: menyediakan nilai yang sudah ditentukan
  it('stub — mengganti fungsi dengan nilai tetap', () => {
    const stub = jest.fn().mockReturnValue('fixed-value');
    expect(stub()).toBe('fixed-value');
  });

  // MOCK: object tiruan dengan ekspektasi
  it('mock — object tiruan yang bisa dicek interaksinya', () => {
    const mockFn = jest.fn();
    mockFn('hello');
    expect(mockFn).toHaveBeenCalledWith('hello');
    expect(mockFn).toHaveBeenCalledTimes(1);
  });

  // SPY: membungkus fungsi asli, tetap berjalan tapi bisa dimonitor
  it('spy — memonitor fungsi asli tanpa menghentikannya', () => {
    const service = new EmailService();
    const spy = jest.spyOn(service, 'send');

    service.send('test@test.com', 'Hello', 'Body');

    expect(spy).toHaveBeenCalledWith('test@test.com', 'Hello', 'Body');
    expect(spy).toHaveReturnedWith(true);

    spy.mockRestore();
  });
});
```

### 3. Unit Test dengan Edge Cases Lengkap

```typescript
// src/__tests__/services/product.service.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { ProductService } from '../../product/product.service';
import { PrismaMockProvider } from '../providers/prisma-mock-provider';
import { prismaMock } from '../mocks/prisma.mock';
import { NotFoundException } from '@nestjs/common';
import { makeProduct } from '../../../test/factories/product.factory';

describe('ProductService', () => {
  let service: ProductService;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [ProductService, PrismaMockProvider],
    }).compile();

    service = module.get<ProductService>(ProductService);
  });

  describe('findAll', () => {
    it('should return paginated products', async () => {
      const products = [makeProduct(), makeProduct()];
      prismaMock.product.findMany.mockResolvedValue(products);
      prismaMock.product.count.mockResolvedValue(2);

      const result = await service.findAll({ page: 1, limit: 10 });
      expect(result.data).toHaveLength(2);
      expect(result.meta.total).toBe(2);
    });

    it('should return empty array when no products exist', async () => {
      prismaMock.product.findMany.mockResolvedValue([]);
      prismaMock.product.count.mockResolvedValue(0);

      const result = await service.findAll({ page: 1, limit: 10 });
      expect(result.data).toEqual([]);
      expect(result.meta.total).toBe(0);
    });
  });

  describe('findById', () => {
    it('should return product when found', async () => {
      const product = makeProduct({ id: 'prod-1' });
      prismaMock.product.findUnique.mockResolvedValue(product);

      const result = await service.findById('prod-1');
      expect(result.id).toBe('prod-1');
    });

    it('should throw NotFoundException when product not found', async () => {
      prismaMock.product.findUnique.mockResolvedValue(null);

      await expect(service.findById('nonexistent')).rejects.toThrow(
        NotFoundException,
      );
    });

    it('should throw when id is empty string', async () => {
      await expect(service.findById('')).rejects.toThrow();
    });

    it('should handle undefined id gracefully', async () => {
      await expect(service.findById(undefined as any)).rejects.toThrow();
    });
  });

  describe('update stock with extreme values', () => {
    it('should handle negative stock', async () => {
      const product = makeProduct({ id: 'prod-1', stock: 5 });
      prismaMock.product.findUnique.mockResolvedValue(product);
      prismaMock.product.update.mockResolvedValue({
        ...product,
        stock: -1,
      });

      // Pastikan service mengizinkan stock negatif atau menolak
      const result = await service.updateStock('prod-1', -6);
      expect(result.stock).toBe(-1);
    });

    it('should handle maximum integer stock', async () => {
      const product = makeProduct({ id: 'prod-1', stock: 0 });
      prismaMock.product.findUnique.mockResolvedValue(product);
      prismaMock.product.update.mockResolvedValue({
        ...product,
        stock: 2_147_483_647,
      });

      const result = await service.updateStock('prod-1', 2_147_483_647);
      expect(result.stock).toBe(2_147_483_647);
    });

    it('should handle zero stock', async () => {
      const product = makeProduct({ id: 'prod-1', stock: 10 });
      prismaMock.product.findUnique.mockResolvedValue(product);
      prismaMock.product.update.mockResolvedValue({ ...product, stock: 0 });

      const result = await service.updateStock('prod-1', -10);
      expect(result.stock).toBe(0);
    });
  });

  describe('create with null/undefined fields', () => {
    it('should reject product creation with null name', async () => {
      const invalid = makeProduct({ name: null as any });
      prismaMock.product.create.mockRejectedValue(
        new Error('null value in column "name"'),
      );

      await expect(service.create(invalid)).rejects.toThrow();
    });

    it('should handle missing optional fields', async () => {
      const minimal = { name: 'Minimal Product', price: 1000, categoryId: 'cat-1' };
      const created = makeProduct({ ...minimal, description: null });
      prismaMock.product.create.mockResolvedValue(created as any);

      const result = await service.create(minimal as any);
      expect(result.name).toBe('Minimal Product');
    });
  });
});
```

### 4. Snapshot Testing

```typescript
// src/__tests__/services/order.service.spec.ts
describe('OrderService — Snapshot Testing', () => {
  it('should match snapshot for order response DTO', async () => {
    const order = {
      id: 'order-1',
      userId: 'user-1',
      status: 'PENDING',
      totalAmount: 250000,
      items: [
        { productId: 'prod-1', quantity: 2, price: 125000 },
      ],
      createdAt: new Date('2026-01-01T00:00:00Z'),
    };

    prismaMock.order.findUnique.mockResolvedValue(order);

    const result = await service.findById('order-1');
    expect(result).toMatchSnapshot();
  });

  it('should match snapshot for error response', async () => {
    prismaMock.order.findUnique.mockResolvedValue(null);

    try {
      await service.findById('order-1');
    } catch (error) {
      expect(error.response).toMatchSnapshot({
        statusCode: 404,
        message: expect.any(String),
        error: 'Not Found',
      });
    }
  });
});
```

### 5. Testing Guard dan Middleware

```typescript
// src/__tests__/guards/roles.guard.spec.ts
import { RolesGuard } from '../../auth/guards/roles.guard';
import { Reflector } from '@nestjs/core';
import { ExecutionContext, ForbiddenException } from '@nestjs/common';
import { Role } from '@prisma/client';

describe('RolesGuard', () => {
  let guard: RolesGuard;
  let reflector: Reflector;

  beforeEach(() => {
    reflector = new Reflector();
    guard = new RolesGuard(reflector);
  });

  const mockExecutionContext = (user?: any): ExecutionContext =>
    ({
      switchToHttp: () => ({
        getRequest: () => ({ user }),
      }),
      getHandler: () => () => {},
      getClass: () => class {},
    }) as any;

  it('should allow access when no roles are required', () => {
    jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue(undefined);
    const ctx = mockExecutionContext({ role: Role.CUSTOMER });

    expect(guard.canActivate(ctx)).toBe(true);
  });

  it('should allow admin to access admin-only route', () => {
    jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue([Role.ADMIN]);
    const ctx = mockExecutionContext({ role: Role.ADMIN });

    expect(guard.canActivate(ctx)).toBe(true);
  });

  it('should deny customer from accessing admin route', () => {
    jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue([Role.ADMIN]);
    const ctx = mockExecutionContext({ role: Role.CUSTOMER });

    expect(guard.canActivate(ctx)).toBe(false);
  });

  it('should throw ForbiddenException when user is null', () => {
    jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue([Role.ADMIN]);
    const ctx = mockExecutionContext(null);

    expect(guard.canActivate(ctx)).toBe(false);
  });
});
```

## Analogi — Gedung Bertingkat

| Konsep | Analogi Gedung |
|--------|----------------|
| **Mock** | Boneka pekerja yang bisa disuruh melakukan apa saja tanpa benar-benar bekerja |
| **Stub** | Alat ukur palsu yang selalu menunjukkan angka tetap untuk menguji alat lain |
| **Spy** | CCTV yang merekam apa yang dilakukan pekerja tanpa mengganggu pekerjaannya |
| **Edge Case** | Uji lift saat listrik padam, saat penuh 20 orang, saat kosong, saat tombol dipencet 100 kali |
| **Snapshot Test** | Foto gedung setelah selesai; jika ada perubahan, kita bisa lihat fotonya dan bandingkan |

## Dipakai untuk Apa

- **Menguji logic bisnis murni** tanpa ketergantungan infrastruktur
- **Regression testing** — snapshot testing menangkap perubahan output tak terduga
- **Keamanan** — edge case testing memastikan validasi berjalan di skenario ekstrem
- **Code coverage** — semakin banyak edge case, semakin tinggi coverage

## Kesalahan Umum yang Sering Terjadi

1. **Mock semua sesuatu** — semua dependency di-mock, padahal ada yang aman dipakai asli (seperti `class-validator`)
2. **Lupa mock reset** — mock state bocor antar test, menyebabkan false positive/negative
3. **Snapshot terlalu besar** — snapshot file jadi 1000+ baris; sulit di-review saat update
4. **Edge case tidak realistis** — test kasus `null` untuk field yang di Prisma sudah `@required` dan di DTO sudah di-validate oleh pipe
5. **Over-mocking Prisma** — mock setiap query padahal bisa pakai `prismaMock.$transaction` untuk test transaksi

## Soal Latihan

### Soal 1: Unit Test dengan Edge Cases

Buat unit test untuk `AuthService.login()` yang menerima `{ email, password }`. Edge cases yang harus di-cover:
- Email tidak terdaftar → throw `UnauthorizedException`
- Password salah → throw `UnauthorizedException`
- User belum verified → throw `ForbiddenException`
- Email null → throw `BadRequestException`
- Password empty string → throw `BadRequestException`

### Jawaban 1:

```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { AuthService } from '../../auth/auth.service';
import { PrismaMockProvider } from '../providers/prisma-mock-provider';
import { JwtService } from '@nestjs/jwt';
import { ConfigService } from '@nestjs/config';
import * as bcrypt from 'bcrypt';
import {
  UnauthorizedException,
  ForbiddenException,
  BadRequestException,
} from '@nestjs/common';
import { Role } from '@prisma/client';

describe('AuthService.login', () => {
  let service: AuthService;
  let jwtService: JwtService;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        AuthService,
        PrismaMockProvider,
        {
          provide: JwtService,
          useValue: { signAsync: jest.fn().mockResolvedValue('token') },
        },
        {
          provide: ConfigService,
          useValue: { get: jest.fn().mockReturnValue('secret') },
        },
      ],
    }).compile();

    service = module.get<AuthService>(AuthService);
    jwtService = module.get<JwtService>(JwtService);
  });

  const mockUser = {
    id: 'user-1',
    email: 'test@test.com',
    password: bcrypt.hashSync('password123', 10),
    name: 'Test User',
    role: Role.CUSTOMER,
    isVerified: true,
  };

  it('should return token for valid credentials', async () => {
    prismaMock.user.findUnique.mockResolvedValue(mockUser);

    const result = await service.login({
      email: 'test@test.com',
      password: 'password123',
    });

    expect(result).toHaveProperty('accessToken');
    expect(result.accessToken).toBe('token');
  });

  it('should throw UnauthorizedException when email not found', async () => {
    prismaMock.user.findUnique.mockResolvedValue(null);

    await expect(
      service.login({ email: 'unknown@test.com', password: 'password123' }),
    ).rejects.toThrow(UnauthorizedException);
  });

  it('should throw UnauthorizedException when password is wrong', async () => {
    prismaMock.user.findUnique.mockResolvedValue(mockUser);

    await expect(
      service.login({ email: 'test@test.com', password: 'wrongpass' }),
    ).rejects.toThrow(UnauthorizedException);
  });

  it('should throw ForbiddenException when user not verified', async () => {
    prismaMock.user.findUnique.mockResolvedValue({
      ...mockUser,
      isVerified: false,
    });

    await expect(
      service.login({ email: 'test@test.com', password: 'password123' }),
    ).rejects.toThrow(ForbiddenException);
  });

  it('should throw BadRequestException when email is null', async () => {
    await expect(
      service.login({ email: null as any, password: 'password123' }),
    ).rejects.toThrow(BadRequestException);
  });

  it('should throw BadRequestException when password is empty', async () => {
    await expect(
      service.login({ email: 'test@test.com', password: '' }),
    ).rejects.toThrow(BadRequestException);
  });
});
```
