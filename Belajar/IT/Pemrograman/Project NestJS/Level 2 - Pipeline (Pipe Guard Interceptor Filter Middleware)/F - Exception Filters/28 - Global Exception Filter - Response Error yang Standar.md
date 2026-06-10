# Global Exception Filter — Response Error yang Standar

## Penjelasan (Nyambung dari materi sebelumnya)

Setelah kita punya **custom exception** yang spesifik per domain, kita perlu satu tempat untuk **menangkap semua exception** dan mengubahnya menjadi response JSON yang seragam.

Tanpa exception filter, tiap error punya format berbeda:
- `BadRequestException` → `{ statusCode, message, error }`
- `NotFoundException` dari Prisma → format berbeda
- Error server 500 → HTML string (di production)

**Global Exception Filter** adalah lapisan terakhir yang memastikan **semua error** keluar dengan format yang sama.

---

## Fungsi

- Format error response yang konsisten untuk semua endpoint
- Menangkap error dari library eksternal (Prisma, TypeORM, Redis, dll)
- Logging error untuk debugging
- Menyembunyikan detail error internal di production
- Mengirim error ke monitoring service (Sentry, Datadog)
- Memberi path/timestamp di setiap error untuk tracing

---

## Cara Implementasi & Code

### 1. AllExceptionsFilter — Tangkap Semua Error

```typescript
// src/common/filters/all-exceptions.filter.ts
import {
  ExceptionFilter,
  Catch,
  ArgumentsHost,
  HttpException,
  HttpStatus,
  Logger,
} from '@nestjs/common';
import { Request, Response } from 'express';

@Catch() // Tangkap SEMUA exception (tanda kurung kosong)
export class AllExceptionsFilter implements ExceptionFilter {
  private readonly logger = new Logger(AllExceptionsFilter.name);

  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();

    let status: number;
    let message: string | object;

    // 1. HttpException — exception NestJS
    if (exception instanceof HttpException) {
      status = exception.getStatus();
      message = exception.getResponse();
    } 
    // 2. Error lain — Prisma, Redis, dll
    else {
      status = HttpStatus.INTERNAL_SERVER_ERROR;
      message = 'Terjadi kesalahan internal server';
    }

    // Logging untuk debugging
    this.logger.error(
      `[${request.method}] ${request.url} → ${status}`,
      exception instanceof Error ? exception.stack : '',
    );

    // Format response standar
    response.status(status).json({
      statusCode: status,
      message: typeof message === 'string' ? message : (message as any).message || message,
      error: typeof message === 'string' ? 'INTERNAL_ERROR' : (message as any).error || 'UNKNOWN_ERROR',
      path: request.url,
      method: request.method,
      timestamp: new Date().toISOString(),
    });
  }
}
```

### 2. HttpExceptionFilter — Khusus HTTP Exception

```typescript
// src/common/filters/http-exception.filter.ts
import {
  ExceptionFilter,
  Catch,
  ArgumentsHost,
  HttpException,
  Logger,
} from '@nestjs/common';
import { Request, Response } from 'express';

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  private readonly logger = new Logger(HttpExceptionFilter.name);

  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();

    const status = exception.getStatus();
    const exceptionResponse = exception.getResponse();

    // Logging
    this.logger.warn(
      `[${request.method}] ${request.url} → ${status}: ${JSON.stringify(exceptionResponse)}`,
    );

    // Format response standar
    response.status(status).json({
      statusCode: status,
      message: typeof exceptionResponse === 'string'
        ? exceptionResponse
        : (exceptionResponse as any).message || exception.message,
      error: typeof exceptionResponse === 'string'
        ? exceptionResponse
        : (exceptionResponse as any).error || 'HTTP_ERROR',
      path: request.url,
      method: request.method,
      timestamp: new Date().toISOString(),
    });
  }
}
```

### 3. Filter — Tangkap Prisma Error

Prisma punya error codes sendiri. Kita perlu menerjemahkannya ke format NestJS.

```typescript
// src/common/filters/prisma-exception.filter.ts
import {
  ExceptionFilter,
  Catch,
  ArgumentsHost,
  HttpStatus,
  Logger,
} from '@nestjs/common';
import { Request, Response } from 'express';
import { Prisma } from '@prisma/client';

@Catch(Prisma.PrismaClientKnownRequestError)
export class PrismaExceptionFilter implements ExceptionFilter {
  private readonly logger = new Logger(PrismaExceptionFilter.name);

  catch(exception: Prisma.PrismaClientKnownRequestError, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();

    let status = HttpStatus.INTERNAL_SERVER_ERROR;
    let message = 'Database error';
    let errorCode = 'DATABASE_ERROR';

    switch (exception.code) {
      case 'P2000': // Value too long
        status = HttpStatus.BAD_REQUEST;
        message = `Nilai terlalu panjang untuk field ${exception.meta?.column_name || 'tidak diketahui'}`;
        errorCode = 'VALUE_TOO_LONG';
        break;

      case 'P2002': // Unique constraint violation
        status = HttpStatus.CONFLICT;
        const target = (exception.meta?.target as string[])?.join(', ') || 'unknown';
        message = `Data sudah ada: ${target}`;
        errorCode = 'UNIQUE_CONSTRAINT';
        break;

      case 'P2003': // Foreign key constraint
        status = HttpStatus.UNPROCESSABLE_ENTITY;
        message = `Referensi data tidak ditemukan`;
        errorCode = 'FOREIGN_KEY_CONSTRAINT';
        break;

      case 'P2025': // Record not found
        status = HttpStatus.NOT_FOUND;
        message = 'Data tidak ditemukan';
        errorCode = 'RECORD_NOT_FOUND';
        break;

      default:
        this.logger.error(`[Prisma Error ${exception.code}] ${exception.message}`);
        status = HttpStatus.INTERNAL_SERVER_ERROR;
        message = 'Terjadi kesalahan database';
        errorCode = `PRISMA_${exception.code}`;
        break;
    }

    this.logger.warn(`[${request.method}] ${request.url} → Prisma ${exception.code}`);

    response.status(status).json({
      statusCode: status,
      message,
      error: errorCode,
      path: request.url,
      method: request.method,
      timestamp: new Date().toISOString(),
    });
  }
}
```

### 4. Global Filter — Gabungan Semua

```typescript
// src/common/filters/global-exception.filter.ts
import {
  ExceptionFilter,
  Catch,
  ArgumentsHost,
  HttpException,
  HttpStatus,
  Logger,
} from '@nestjs/common';
import { Request, Response } from 'express';
import { Prisma } from '@prisma/client';

@Catch()
export class GlobalExceptionFilter implements ExceptionFilter {
  private readonly logger = new Logger(GlobalExceptionFilter.name);

  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();

    let status = HttpStatus.INTERNAL_SERVER_ERROR;
    let body: Record<string, any> = {
      statusCode: status,
      message: 'Terjadi kesalahan internal server',
      error: 'INTERNAL_SERVER_ERROR',
      path: request.url,
      method: request.method,
      timestamp: new Date().toISOString(),
    };

    // Handle berdasarkan tipe exception
    if (exception instanceof HttpException) {
      const exceptionResponse = exception.getResponse();
      status = exception.getStatus();

      body = {
        statusCode: status,
        message: typeof exceptionResponse === 'string'
          ? exceptionResponse
          : (exceptionResponse as any).message || exception.message,
        error: typeof exceptionResponse === 'string'
          ? 'HTTP_ERROR'
          : (exceptionResponse as any).error || exception.name,
        path: request.url,
        method: request.method,
        timestamp: new Date().toISOString(),
      };

      // Bawa metadata jika ada
      if (typeof exceptionResponse === 'object' && !Array.isArray(exceptionResponse)) {
        const { message, error, ...rest } = exceptionResponse as any;
        body = { ...body, ...rest };
      }
    } 
    else if (exception instanceof Prisma.PrismaClientKnownRequestError) {
      status = this.handlePrismaError(exception, body);
      body.statusCode = status;
    }
    else if (exception instanceof Prisma.PrismaClientValidationError) {
      status = HttpStatus.BAD_REQUEST;
      body = {
        ...body,
        statusCode: status,
        message: 'Invalid query format',
        error: 'PRISMA_VALIDATION',
      };
    }
    else if (exception instanceof Error) {
      this.logger.error(`Unhandled error: ${exception.message}`, exception.stack);
    }

    this.logger.warn(
      `[${request.method}] ${request.url} → ${status}`,
    );

    response.status(status).json(body);
  }

  private handlePrismaError(
    exception: Prisma.PrismaClientKnownRequestError,
    body: Record<string, any>,
  ): number {
    switch (exception.code) {
      case 'P2002':
        body.statusCode = HttpStatus.CONFLICT;
        body.message = 'Data sudah ada';
        body.error = 'UNIQUE_CONSTRAINT';
        body.fields = exception.meta?.target;
        return HttpStatus.CONFLICT;

      case 'P2025':
        body.statusCode = HttpStatus.NOT_FOUND;
        body.message = 'Data tidak ditemukan';
        body.error = 'NOT_FOUND';
        return HttpStatus.NOT_FOUND;

      default:
        body.statusCode = HttpStatus.INTERNAL_SERVER_ERROR;
        body.message = 'Database error';
        body.error = `PRISMA_${exception.code}`;
        return HttpStatus.INTERNAL_SERVER_ERROR;
    }
  }
}
```

### 5. Register di main.ts

```typescript
// src/main.ts
import { NestFactory } from '@nestjs/core';
import { ValidationPipe } from '@nestjs/common';
import { AppModule } from './app.module';
import { GlobalExceptionFilter } from './common/filters/global-exception.filter';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Global pipes
  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true,
      forbidNonWhitelisted: true,
      transform: true,
    }),
  );

  // Global exception filter — harus setelah pipes
  app.useGlobalFilters(new GlobalExceptionFilter());

  await app.listen(3000);
}
bootstrap();
```

### 6. Production vs Development — Conditional

```typescript
// src/common/filters/global-exception.filter.ts (bagian logging)
import { ConfigService } from '@nestjs/config';

@Catch()
export class GlobalExceptionFilter implements ExceptionFilter {
  constructor(private readonly configService: ConfigService) {}

  catch(exception: unknown, host: ArgumentsHost) {
    const isProduction = this.configService.get('NODE_ENV') === 'production';

    // Di production: sembunyikan stack trace
    if (isProduction && !(exception instanceof HttpException)) {
      this.logger.error('Internal server error');
      message = 'Terjadi kesalahan, silakan coba lagi';
    }

    // Di development: tampilkan detail
    if (!isProduction) {
      response.status(status).json({
        ...body,
        stack: exception instanceof Error ? exception.stack : undefined,
      });
    }
  }
}
```

### 7. Logger — Format Detail

```typescript
// src/common/filters/global-exception.filter.ts (logging)
catch(exception: unknown, host: ArgumentsHost) {
  const ctx = host.switchToHttp();
  const request = ctx.getRequest<Request>();

  // Structured logging
  this.logger.log({
    level: 'error',
    timestamp: new Date().toISOString(),
    method: request.method,
    url: request.url,
    status,
    body: request.body,
    query: request.query,
    params: request.params,
    userId: (request as any).user?.id,
    error: exception instanceof Error ? {
      name: exception.name,
      message: exception.message,
      stack: exception.stack?.split('\n').slice(0, 5).join('\n'),
    } : exception,
  });

  // Kirim ke Sentry jika ada
  // Sentry.captureException(exception);
}
```

---

## Analogi — Membangun Gedung

| Konsep | Analogi Gedung |
|--------|----------------|
| **Exception** | **Masalah** di proyek: Bata retak, Cat salah warna, Atap bocor |
| **Exception Filter** | **Tim penanganan masalah** — mereka yang turun tangan |
| **GlobalExceptionFilter** | **Kepala proyek** — menangani SEMUA masalah, dari sepele sampai darurat |
| **PrismaExceptionFilter** | **Spesialis fondasi** — hanya menangani masalah struktur/beton |
| **Format standar** | Format laporan masalah: (kode masalah, lokasi, tanggal, deskripsi) |
| **Logging** | Buku catatan proyek — semua masalah dicatat untuk evaluasi |
| **Stack trace** | **CCTV konstruksi** — melihat persis kejadian langkah demi langkah |
| **Production vs Dev** | Laporan ke klien (rapi) vs laporan internal (detail full) |

---

## Dipakai Untuk Apa

- **Standarisasi format error** — semua response error punya struktur `{ statusCode, message, error, path, timestamp }`
- **Menangkap Prisma error** — `P2002` (unique) → 409, `P2025` (not found) → 404, dsb
- **Logging otomatis** — setiap error tercatat di log tanpa tambahan kode di service
- **Monitoring** — kirim error ke Sentry / Datadog dari satu tempat
- **Security** — sembunyikan stack trace di production
- **Tracing** — path + timestamp memudahkan tracking error

---

## Kesalahan Umum

1. **Filter tidak terdaftar** — `app.useGlobalFilters()` tidak dipanggil → error tetap format default NestJS
2. **Filter mendahului middleware** — deklarasi urutan penting: filter setelah pipes
3. **@Catch() vs @Catch(HttpException)** — `@Catch()` menangkap semua, `@Catch(HttpException)` hanya HTTP exception
4. **Mengabaikan Prisma error** — Prisma error tidak tertangkap → 500 dengan HTML stack trace
5. **Tidak handle async error** — async handler yang throw harus tertangkap filter
6. **Multiple filter tanpa prioritas** — urutan filter penting, filter spesifik harus didahulukan
7. **Message tidak konsisten** — kadang string, kadang array, kadang object

---

## Soal Latihan & Jawaban

### Soal

Buat global exception filter yang:
1. Menangkap semua exception
2. Menangkap Prisma error P2002 (unique constraint) → 409
3. Menangkap Prisma error P2025 (not found) → 404
4. Format response: `{ statusCode, message, error, path, method, timestamp }`
5. Logging setiap error
6. Register di `main.ts`

### Jawaban

**GlobalExceptionFilter**

```typescript
// src/common/filters/global-exception.filter.ts
import {
  ExceptionFilter,
  Catch,
  ArgumentsHost,
  HttpException,
  HttpStatus,
  Logger,
} from '@nestjs/common';
import { Request, Response } from 'express';

@Catch()
export class GlobalExceptionFilter implements ExceptionFilter {
  private readonly logger = new Logger(GlobalExceptionFilter.name);

  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();

    let status = HttpStatus.INTERNAL_SERVER_ERROR;
    let message = 'Terjadi kesalahan internal server';
    let error = 'INTERNAL_SERVER_ERROR';

    if (exception instanceof HttpException) {
      status = exception.getStatus();
      const res = exception.getResponse();
      message = typeof res === 'string' ? res : (res as any).message || exception.message;
      error = typeof res === 'string' ? 'HTTP_ERROR' : (res as any).error || exception.name;
    } else if (this.isPrismaError(exception)) {
      const prismaError = exception as any;
      if (prismaError.code === 'P2002') {
        status = HttpStatus.CONFLICT;
        message = 'Data sudah ada';
        error = 'UNIQUE_CONSTRAINT';
      } else if (prismaError.code === 'P2025') {
        status = HttpStatus.NOT_FOUND;
        message = 'Data tidak ditemukan';
        error = 'NOT_FOUND';
      }
    }

    this.logger.error(
      `[${request.method}] ${request.url} → ${status} - ${message}`,
      exception instanceof Error ? exception.stack : '',
    );

    response.status(status).json({
      statusCode: status,
      message,
      error,
      path: request.url,
      method: request.method,
      timestamp: new Date().toISOString(),
    });
  }

  private isPrismaError(exception: unknown): boolean {
    return (
      exception instanceof Object &&
      (exception as any).constructor?.name === 'PrismaClientKnownRequestError'
    );
  }
}
```

**main.ts**

```typescript
// src/main.ts
import { NestFactory } from '@nestjs/core';
import { ValidationPipe } from '@nestjs/common';
import { AppModule } from './app.module';
import { GlobalExceptionFilter } from './common/filters/global-exception.filter';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true,
      forbidNonWhitelisted: true,
      transform: true,
    }),
  );

  app.useGlobalFilters(new GlobalExceptionFilter());

  await app.listen(3000);
}
bootstrap();
```
