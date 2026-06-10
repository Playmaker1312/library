# Exception Filter per Controller & Testing Exception Filter

## Penjelasan (Nyambung dari materi sebelumnya)

Setelah kita punya **Global Exception Filter** yang menangani semua error dengan format standar, kadang ada kasus di mana **controller tertentu** butuh penanganan error yang berbeda.

Misal: controller `Auth` ingin error 401 dikembalikan dengan format khusus (bukan standar global), atau controller `Payment` ingin menangkap error payment gateway sendiri.

Selain itu, exception filter juga perlu **di-test** — kita harus pastikan filter benar-benar mengubah error jadi format yang diharapkan.

---

## Fungsi

- **Per-controller filter** — override global filter untuk controller tertentu
- **Per-method filter** — override untuk method spesifik
- **Unit test exception filter** — pastikan format output sesuai harapan
- **Integration test** — pastikan filter bekerja di request nyata

---

## Cara Implementasi & Code

### 1. @UseFilters — Per Controller

```typescript
// src/auth/auth.controller.ts
import { Controller, Post, UseFilters } from '@nestjs/common';
import { AuthExceptionFilter } from '../common/filters/auth-exception.filter';

@Controller('auth')
@UseFilters(AuthExceptionFilter)
export class AuthController {
  @Post('login')
  login() {
    // Error di sini di-handle oleh AuthExceptionFilter, bukan GlobalExceptionFilter
  }
}
```

### 2. @UseFilters — Per Method

```typescript
// src/payment/payment.controller.ts
import { Controller, Post, Get, UseFilters } from '@nestjs/common';
import { PaymentExceptionFilter } from '../common/filters/payment-exception.filter';

@Controller('payments')
export class PaymentController {
  @Post()
  @UseFilters(PaymentExceptionFilter)
  async createPayment() {
    // Error method ini di-handle PaymentExceptionFilter
  }

  @Get(':id')
  async findOne() {
    // Error method ini di-handle GlobalExceptionFilter (default)
  }
}
```

### 3. AuthExceptionFilter — Contoh Filter Spesifik

```typescript
// src/common/filters/auth-exception.filter.ts
import {
  ExceptionFilter, Catch, ArgumentsHost,
  HttpException, HttpStatus, Logger,
} from '@nestjs/common';
import { Response } from 'express';

@Catch(HttpException)
export class AuthExceptionFilter implements ExceptionFilter {
  private readonly logger = new Logger(AuthExceptionFilter.name);

  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const status = exception.getStatus();

    // Auth-specific: sembunyikan detail untuk 401
    if (status === HttpStatus.UNAUTHORIZED) {
      return response.status(status).json({
        statusCode: status,
        message: 'Authentication failed',
        error: 'AUTH_FAILED',
        timestamp: new Date().toISOString(),
      });
    }

    // Format standar untuk error lain
    const exceptionResponse = exception.getResponse();
    response.status(status).json({
      statusCode: status,
      message: typeof exceptionResponse === 'string'
        ? exceptionResponse
        : (exceptionResponse as any).message || exception.message,
      error: 'AUTH_ERROR',
      timestamp: new Date().toISOString(),
    });
  }
}
```

### 4. PaymentExceptionFilter — Dengan Dependencies

```typescript
// src/common/filters/payment-exception.filter.ts
import {
  ExceptionFilter, Catch, ArgumentsHost,
  HttpException, HttpStatus, Logger,
} from '@nestjs/common';
import { Response } from 'express';
import { ConfigService } from '@nestjs/config';

@Catch()
export class PaymentExceptionFilter implements ExceptionFilter {
  private readonly logger = new Logger(PaymentExceptionFilter.name);

  constructor(private readonly configService: ConfigService) {}

  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();

    if (exception instanceof HttpException) {
      const status = exception.getStatus();
      const res = exception.getResponse();
      return response.status(status).json({
        statusCode: status,
        message: typeof res === 'string' ? res : (res as any).message,
        error: 'PAYMENT_ERROR',
        timestamp: new Date().toISOString(),
      });
    }

    this.logger.error('Unhandled payment error', exception instanceof Error ? exception.stack : '');
    response.status(HttpStatus.INTERNAL_SERVER_ERROR).json({
      statusCode: HttpStatus.INTERNAL_SERVER_ERROR,
      message: 'Payment internal error',
      error: 'PAYMENT_INTERNAL_ERROR',
      timestamp: new Date().toISOString(),
    });
  }
}
```

---

## Testing Exception Filter

### 5. Unit Test — GlobalExceptionFilter

```typescript
// test/unit/filters/global-exception.filter.spec.ts
import { GlobalExceptionFilter } from '../../../src/common/filters/global-exception.filter';
import {
  BadRequestException, NotFoundException, HttpException,
} from '@nestjs/common';

describe('GlobalExceptionFilter', () => {
  let filter: GlobalExceptionFilter;
  let mockResponse: any;
  let mockRequest: any;
  let mockHost: any;

  beforeEach(() => {
    filter = new GlobalExceptionFilter();

    mockRequest = { url: '/api/products', method: 'GET' };

    mockResponse = {
      status: jest.fn().mockReturnThis(),
      json: jest.fn().mockReturnThis(),
    };

    mockHost = {
      switchToHttp: jest.fn().mockReturnThis(),
      getResponse: jest.fn().mockReturnValue(mockResponse),
      getRequest: jest.fn().mockReturnValue(mockRequest),
    };
  });

  it('should format HttpException correctly', () => {
    const exception = new NotFoundException('Produk tidak ditemukan');

    filter.catch(exception, mockHost);

    expect(mockResponse.status).toHaveBeenCalledWith(404);
    expect(mockResponse.json).toHaveBeenCalledWith(
      expect.objectContaining({
        statusCode: 404,
        message: 'Produk tidak ditemukan',
        path: '/api/products',
        method: 'GET',
        timestamp: expect.any(String),
      }),
    );
  });

  it('should return 500 for non-HTTP exceptions', () => {
    const exception = new Error('Database connection failed');

    filter.catch(exception, mockHost);

    expect(mockResponse.status).toHaveBeenCalledWith(500);
    expect(mockResponse.json).toHaveBeenCalledWith(
      expect.objectContaining({
        statusCode: 500,
        message: 'Terjadi kesalahan internal server',
        error: 'INTERNAL_SERVER_ERROR',
      }),
    );
  });

  it('should include metadata from custom exception', () => {
    const exception = new HttpException(
      {
        statusCode: 409,
        message: 'Email sudah terdaftar',
        error: 'EMAIL_ALREADY_REGISTERED',
        email: 'user@example.com',
      },
      409,
    );

    filter.catch(exception, mockHost);

    expect(mockResponse.json).toHaveBeenCalledWith(
      expect.objectContaining({
        statusCode: 409,
        message: 'Email sudah terdaftar',
        error: 'EMAIL_ALREADY_REGISTERED',
        email: 'user@example.com',
      }),
    );
  });
});
```

### 6. Unit Test — Prisma Error Handler

```typescript
// test/unit/filters/prisma-handler.spec.ts
import { GlobalExceptionFilter } from '../../../src/common/filters/global-exception.filter';

describe('GlobalExceptionFilter - Prisma Error', () => {
  let filter: GlobalExceptionFilter;
  let mockResponse: any;
  let mockRequest: any;
  let mockHost: any;

  beforeEach(() => {
    filter = new GlobalExceptionFilter();
    mockResponse = { status: jest.fn().mockReturnThis(), json: jest.fn() };
    mockRequest = { url: '/api/users', method: 'POST' };
    mockHost = {
      switchToHttp: jest.fn().mockReturnThis(),
      getResponse: jest.fn().mockReturnValue(mockResponse),
      getRequest: jest.fn().mockReturnValue(mockRequest),
    };
  });

  it('should return 409 for Prisma P2002', () => {
    const prismaError = {
      constructor: { name: 'PrismaClientKnownRequestError' },
      code: 'P2002',
      meta: { target: ['email'] },
      message: 'Unique constraint failed',
    };

    filter.catch(prismaError as any, mockHost);

    expect(mockResponse.status).toHaveBeenCalledWith(409);
    expect(mockResponse.json).toHaveBeenCalledWith(
      expect.objectContaining({
        statusCode: 409,
        message: 'Data sudah ada',
        error: 'UNIQUE_CONSTRAINT',
      }),
    );
  });

  it('should return 404 for Prisma P2025', () => {
    const prismaError = {
      constructor: { name: 'PrismaClientKnownRequestError' },
      code: 'P2025',
      message: 'Record not found',
    };

    filter.catch(prismaError as any, mockHost);

    expect(mockResponse.status).toHaveBeenCalledWith(404);
    expect(mockResponse.json).toHaveBeenCalledWith(
      expect.objectContaining({
        statusCode: 404,
        message: 'Data tidak ditemukan',
        error: 'NOT_FOUND',
      }),
    );
  });
});
```

### 7. Integration Test — End-to-End

```typescript
// test/integration/filter.e2e-spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication, ValidationPipe } from '@nestjs/common';
import * as request from 'supertest';
import { AppModule } from '../../src/app.module';
import { GlobalExceptionFilter } from '../../src/common/filters/global-exception.filter';

describe('GlobalExceptionFilter (e2e)', () => {
  let app: INestApplication;

  beforeAll(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    app.useGlobalPipes(new ValidationPipe({ whitelist: true, forbidNonWhitelisted: true }));
    app.useGlobalFilters(new GlobalExceptionFilter());
    await app.init();
  });

  afterAll(async () => {
    await app.close();
  });

  it('GET /nonexistent -> 404 with standard format', () => {
    return request(app.getHttpServer())
      .get('/nonexistent')
      .expect(404)
      .expect((res) => {
        expect(res.body).toHaveProperty('statusCode', 404);
        expect(res.body).toHaveProperty('message');
        expect(res.body).toHaveProperty('error');
        expect(res.body).toHaveProperty('path');
        expect(res.body).toHaveProperty('method');
        expect(res.body).toHaveProperty('timestamp');
      });
  });

  it('POST /products with empty body -> 400 with standard format', () => {
    return request(app.getHttpServer())
      .post('/products')
      .send({})
      .expect(400)
      .expect((res) => {
        expect(res.body.statusCode).toBe(400);
      });
  });
});
```

### 8. Unit Test — AuthExceptionFilter

```typescript
// test/unit/filters/auth-exception.filter.spec.ts
import { AuthExceptionFilter } from '../../../src/common/filters/auth-exception.filter';
import { UnauthorizedException, ForbiddenException } from '@nestjs/common';

describe('AuthExceptionFilter', () => {
  let filter: AuthExceptionFilter;
  let mockResponse: any;
  let mockHost: any;

  beforeEach(() => {
    filter = new AuthExceptionFilter();
    mockResponse = { status: jest.fn().mockReturnThis(), json: jest.fn() };
    mockHost = {
      switchToHttp: jest.fn().mockReturnThis(),
      getResponse: jest.fn().mockReturnValue(mockResponse),
      getRequest: jest.fn().mockReturnValue({ url: '/auth/login', method: 'POST' }),
    };
  });

  it('should hide detail on 401', () => {
    filter.catch(new UnauthorizedException('invalid token'), mockHost);

    expect(mockResponse.status).toHaveBeenCalledWith(401);
    expect(mockResponse.json).toHaveBeenCalledWith(
      expect.objectContaining({
        message: 'Authentication failed',
        error: 'AUTH_FAILED',
      }),
    );
  });

  it('should not hide detail on 403', () => {
    filter.catch(new ForbiddenException('no access'), mockHost);

    expect(mockResponse.status).toHaveBeenCalledWith(403);
    expect(mockResponse.json).toHaveBeenCalledWith(
      expect.objectContaining({ message: 'Forbidden' }),
    );
  });
});
```

---

## Analogi — Membangun Gedung

| Konsep | Analogi Gedung |
|--------|----------------|
| **Global Exception Filter** | Kantor pusat keamanan — menangani semua alarm dari seluruh proyek |
| **Per-Controller Filter** | Pos keamanan khusus di lantai 3 dengan protokol sendiri |
| **Per-Method Filter** | Satu pintu tertentu punya sistem alarm berbeda |
| **Unit Test Filter** | Simulasi: "Kalau alarm berbunyi, apakah sirine di pos pusat bunyi?" |
| **Integration Test** | Latihan evakuasi penuh: asap beneran, respon beneran |
| **AuthExceptionFilter** | Pos keamanan khusus VIP — tamu penting ditangani beda |

---

## Dipakai Untuk Apa

- **Filter per controller** — Auth (format 401 khusus), Payment (tangkap error gateway)
- **Filter per method** — Satu endpoint butuh error handling berbeda
- **Unit test filter** — Memastikan format response error sesuai API contract
- **Integration test** — Memastikan filter bekerja di pipeline HTTP nyata
- **Override global** — Controller spesifik butuh format error berbeda dari standar global

---

## Kesalahan Umum

1. **@UseFilters dari module yang tidak di-import** — error `Nest cannot resolve dependencies`
2. **Multiple filter untuk satu controller** — urutan eksekusi tidak terjamin, bisa bentrok
3. **Filter dependency tidak terdaftar di module** — constructor butuh service, tapi tidak di-provide
4. **Unit test tidak mock host dengan benar** — `host.switchToHttp().getResponse()` harus return object
5. **Integration test tanpa filter** — test tidak mencerminkan production
6. **Filter terlalu spesifik, global filter jadi tidak berguna** — terlalu banyak override
7. **Lupa export filter di module** — padahal dipakai di controller module lain

---

## Soal Latihan & Jawaban

### Soal

Tulis unit test untuk `GlobalExceptionFilter` yang menguji:
1. `NotFoundException` -> status 404, format standar
2. `BadRequestException` dengan array message -> status 400, message array
3. `Error` biasa -> status 500, message generic

### Jawaban

```typescript
// test/unit/filters/global-exception.filter.spec.ts
import { GlobalExceptionFilter } from '../../../src/common/filters/global-exception.filter';
import {
  BadRequestException,
  NotFoundException,
} from '@nestjs/common';

describe('GlobalExceptionFilter', () => {
  let filter: GlobalExceptionFilter;
  let mockResponse: any;
  let mockHost: any;

  beforeEach(() => {
    filter = new GlobalExceptionFilter();

    const mockRequest = { url: '/test', method: 'GET' };

    mockResponse = {
      status: jest.fn().mockReturnThis(),
      json: jest.fn().mockReturnThis(),
    };

    mockHost = {
      switchToHttp: jest.fn(() => ({
        getResponse: () => mockResponse,
        getRequest: () => mockRequest,
      })),
      getResponse: jest.fn().mockReturnValue(mockResponse),
      getRequest: jest.fn().mockReturnValue(mockRequest),
    };
  });

  it('should return 404 with standard format for NotFoundException', () => {
    const exception = new NotFoundException('Resource tidak ditemukan');
    filter.catch(exception, mockHost);

    expect(mockResponse.status).toHaveBeenCalledWith(404);
    expect(mockResponse.json).toHaveBeenCalledWith(
      expect.objectContaining({
        statusCode: 404,
        message: 'Resource tidak ditemukan',
        path: '/test',
        method: 'GET',
        timestamp: expect.any(String),
      }),
    );
  });

  it('should return 400 with array message for BadRequestException', () => {
    const exception = new BadRequestException([
      'name should not be empty',
      'price must be positive',
    ]);

    filter.catch(exception, mockHost);

    expect(mockResponse.status).toHaveBeenCalledWith(400);
    expect(mockResponse.json).toHaveBeenCalledWith(
      expect.objectContaining({
        statusCode: 400,
        message: expect.any(Array),
      }),
    );
  });

  it('should return 500 for generic Error', () => {
    const exception = new Error('Something went wrong');
    filter.catch(exception, mockHost);

    expect(mockResponse.status).toHaveBeenCalledWith(500);
    expect(mockResponse.json).toHaveBeenCalledWith(
      expect.objectContaining({
        statusCode: 500,
        message: 'Terjadi kesalahan internal server',
        error: 'INTERNAL_SERVER_ERROR',
      }),
    );
  });
});
```
