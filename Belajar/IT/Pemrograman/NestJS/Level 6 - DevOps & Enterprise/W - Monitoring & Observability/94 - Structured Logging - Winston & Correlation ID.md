# 94 - Structured Logging - Winston & Correlation ID

## Penjelasan
Setelah aplikasi berjalan dengan health checks (materi 93), kita perlu tahu apa yang terjadi di dalamnya saat ada error. `console.log()` tidak cukup — di production kita butuh log terstruktur (JSON) yang bisa dibaca oleh tools seperti Elasticsearch, Grafana Loki, atau Datadog. Winston adalah logging library paling populer di ekosistem Node.js.

## Fungsi
- **Structured logging JSON format**: Log dalam format JSON, mudah diparsing oleh log aggregator.
- **Winston**: Console transport untuk dev (warna, readable), File transport untuk production (JSON).
- **Correlation ID**: ID unik untuk setiap request, melacak perjalanan request melewati service/middleware.
- **Log levels**: error > warn > info > debug — filter berdasarkan severity.

## Cara Pengimplementasian

### Install
```bash
npm install --save winston @nestjs/common
```

### Winston Logger Service
```typescript
import { Injectable, LoggerService } from '@nestjs/common';
import * as winston from 'winston';

@Injectable()
export class AppLogger implements LoggerService {
  private logger: winston.Logger;

  constructor() {
    this.logger = winston.createLogger({
      level: process.env.NODE_ENV === 'production' ? 'info' : 'debug',
      format: format.combine(
        format.timestamp({ format: 'ISO' }),
        format.errors({ stack: true }),
        format.json(),
      ),
      defaultMeta: { service: 'nestjs-app' },
      transports: [
        new winston.transports.Console({
          format: process.env.NODE_ENV === 'production'
            ? format.json()
            : format.combine(
                format.colorize(),
                format.printf(({ timestamp, level, message, context, trace }) => {
                  return `${timestamp} [${context}] ${level}: ${message}${trace ? '\n' + trace : ''}`;
                }),
              ),
        }),
        ...(process.env.NODE_ENV === 'production'
          ? [
              new winston.transports.File({ filename: 'logs/error.log', level: 'error' }),
              new winston.transports.File({ filename: 'logs/combined.log' }),
            ]
          : []),
      ],
    });
  }

  log(message: any, context?: string) { this.logger.info(message, { context }); }
  error(message: any, trace?: string, context?: string) { this.logger.error(message, { trace, context }); }
  warn(message: any, context?: string) { this.logger.warn(message, { context }); }
  debug(message: any, context?: string) { this.logger.debug(message, { context }); }
  verbose(message: any, context?: string) { this.logger.verbose(message, { context }); }
}
```

### Correlation ID Middleware
```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';
import { v4 as uuidv4 } from 'uuid';

export const CORRELATION_ID_HEADER = 'X-Correlation-Id';

@Injectable()
export class CorrelationIdMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    const id = (req.headers[CORRELATION_ID_HEADER.toLowerCase()] as string) || uuidv4();
    req.headers[CORRELATION_ID_HEADER.toLowerCase()] = id;
    res.setHeader(CORRELATION_ID_HEADER, id);
    next();
  }
}
```

### Async Storage untuk Correlation ID
```typescript
// correlation-id.storage.ts
import { AsyncLocalStorage } from 'async_hooks';

export const correlationIdStorage = new AsyncLocalStorage<string>();

export function getCorrelationId(): string | undefined {
  return correlationIdStorage.getStore();
}
```

### Middleware dengan AsyncLocalStorage
```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';
import { v4 as uuidv4 } from 'uuid';
import { correlationIdStorage, CORRELATION_ID_HEADER } from './correlation-id.storage';

@Injectable()
export class CorrelationIdMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    const id = req.headers[CORRELATION_ID_HEADER.toLowerCase()] as string || uuidv4();
    correlationIdStorage.run(id, () => {
      res.setHeader(CORRELATION_ID_HEADER, id);
      (req as any).correlationId = id;
      next();
    });
  }
}
```

### Integrasi dengan Winston Logger
```typescript
// Di AppLogger, tambahkan correlationId ke defaultMeta
const store = correlationIdStorage.getStore();
this.logger.defaultMeta = {
  ...this.logger.defaultMeta,
  correlationId: store,
};
```

### Register di AppModule
```typescript
import { Module, MiddlewareConsumer } from '@nestjs/common';
import { CorrelationIdMiddleware } from './common/middleware/correlation-id.middleware';

@Module({
  providers: [AppLogger],
  exports: [AppLogger],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(CorrelationIdMiddleware).forRoutes('*');
  }
}
```

### Output Log
```json
{
  "level": "info",
  "message": "User created successfully",
  "timestamp": "2026-06-10T12:00:00.000Z",
  "service": "nestjs-app",
  "context": "UsersService",
  "correlationId": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
}
```

## Analogi
Setiap orang yang masuk ke gedung (request) diberi **kartu pengunjung** unik (correlation ID). Petugas keamanan mencatat di mana saja orang itu pergi — ke lantai 2 (controller), ke ruang server (database), ke ruang arsip (Redis). Jika ada masalah (error), kita bisa lacak kartu itu: "Oh, pengunjung A1B2C3 jatuh di lift lantai 3 — kita tahu persis langkah apa yang dia ambil sebelum jatuh." Tanpa correlation ID, kita hanya tahu "seseorang jatuh di suatu tempat."

## Dipakai untuk apa
- Debugging error production — trace request dari awal sampai akhir.
- Audit trail — siapa melakukan apa dan kapan.
- Log aggregation — Elastic Stack, Grafana Loki, Datadog.
- Performance analysis — lihat log level warn/error untuk endpoint lambat.

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Hanya pakai console.log | Tidak bisa difilter, tidak ada level, tidak structured | Gunakan Winston atau Pino |
| Tidak ada correlation ID | Tidak bisa trace request di log | Implement middleware + AsyncLocalStorage |
| Log level terlalu verbose di production | Biaya storage log membengkak | Set level `info` di production, `debug` di dev |
| Log sensitive data (password, token) | Kebocoran data | Filter/redact sensitive fields sebelum log |
| File log tanpa rotation | Disk penuh | Gunakan `winston-daily-rotate-file` |

## Soal Latihan

**Soal 1:** Buat Winston logger dengan transport file yang melakukan rotasi harian (daily rotate) dan menyimpan log max 30 hari.

**Jawaban 1:**
```bash
npm install winston-daily-rotate-file
```
```typescript
import DailyRotateFile from 'winston-daily-rotate-file';

const fileTransport = new DailyRotateFile({
  filename: 'logs/application-%DATE%.log',
  datePattern: 'YYYY-MM-DD',
  maxFiles: '30d',
  zippedArchive: true,
  format: format.json(),
});

this.logger.add(fileTransport);
```

**Soal 2:** Bagaimana cara mendapatkan correlation ID di dalam service tanpa meneruskannya sebagai parameter?

**Jawaban 2:** Gunakan `AsyncLocalStorage`:

```typescript
import { correlationIdStorage } from './correlation-id.storage';

@Injectable()
export class UsersService {
  findAll() {
    const correlationId = correlationIdStorage.getStore();
    this.logger.log({ message: 'Fetching users', correlationId });
    // ...
  }
}
```

`AsyncLocalStorage` menyimpan state per async context — setiap request punya context sendiri tanpa perlu passing parameter secara eksplisit.
