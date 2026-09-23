# 10 - Dependency Injection - Jantung NestJS yang Wajib Dipahami Mendalam

## Penjelasan

Setelah memahami Module sebagai unit organisasi, sekarang kita bahas **mekanisme yang menghubungkan semua komponen**: Dependency Injection (DI).

DI adalah pola desain di mana sebuah objek **tidak membuat dependensinya sendiri**, melainkan **diberikan (injected) dari luar**. Ini adalah kebalikan dari cara tradisional di mana service membuat dependency sendiri (`new Something()`).

NestJS memiliki **DI Container** — sebuah registri raksasa yang tahu cara membuat dan menyediakan semua provider. Kamu tinggal bilang "saya butuh UsersService", dan NestJS akan menyediakannya lengkap dengan dependensinya.

## Fungsi

- **Decoupling**: Kelas tidak perlu tahu cara membuat dependensinya
- **Testability**: Dependensi mudah diganti dengan mock untuk unit testing
- **Reusability**: Provider bisa dipakai ulang tanpa mengubah kode pemakai
- **Lifecycle management**: NestJS mengatur kapan provider dibuat, dipakai, dan dihancurkan

## Cara Pengimplementasian / Code

### @Injectable() & Constructor Injection

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()  // Memberi tahu NestJS: "kelas ini bisa di-inject"
export class LoggerService {
  log(message: string) {
    console.log(`[LOG]: ${message}`);
  }
}

@Injectable()
export class UsersService {
  // Constructor injection — NestJS akan menyediakan LoggerService
  constructor(private readonly logger: LoggerService) {}

  createUser(name: string) {
    this.logger.log(`User ${name} created`);
    // ... business logic
  }
}
```

Tanpa DI (cara tradisional — buruk):
```typescript
export class UsersService {
  private logger: LoggerService;

  constructor() {
    // UsersService membuat dependensinya sendiri — TIGHT COUPLING!
    this.logger = new LoggerService();
    // Masalah: sulit diganti, sulit di-test, tidak fleksibel
  }
}
```

### Provider Scope — Mengatur Siklus Hidup

NestJS memiliki 3 scope provider:

```typescript
import { Injectable, Scope } from '@nestjs/common';

// 1. DEFAULT (SINGLETON) — satu instance untuk seluruh aplikasi
@Injectable()  // secara default Scope.DEFAULT
export class ConfigService {
  private config = { apiUrl: 'https://api.example.com' };
  // Instance yang sama untuk semua request
}

// 2. REQUEST — instance baru setiap request
@Injectable({ scope: Scope.REQUEST })
export class RequestContextService {
  private requestId: string;

  constructor() {
    this.requestId = Math.random().toString(36).substring(2);
    console.log(`New RequestContext created: ${this.requestId}`);
  }

  getRequestId(): string {
    return this.requestId;
  }
}

// 3. TRANSIENT — instance baru setiap kali di-inject
@Injectable({ scope: Scope.TRANSIENT })
export class TemporaryIdService {
  private id: string;

  constructor() {
    this.id = Math.random().toString(36).substring(2);
    console.log(`New TemporaryId created: ${this.id}`);
  }

  getId(): string {
    return this.id;
  }
}
```

Cara inject service dengan scope berbeda:

```typescript
@Injectable()
export class OrdersService {
  constructor(
    private readonly config: ConfigService,              // Singleton
    @Inject(REQUEST) private readonly request: any,      // REQUEST scope
    private readonly tempId: TemporaryIdService,          // TRANSIENT scope
  ) {}
}
```

### Perbandingan Scope

```typescript
// Test scope behavior
@Controller('scope-test')
export class ScopeTestController {
  constructor(
    private readonly context1: RequestContextService,  // REQUEST
    private readonly context2: RequestContextService,  // REQUEST — SAMA dengan context1
    private readonly temp1: TemporaryIdService,        // TRANSIENT
    private readonly temp2: TemporaryIdService,        // TRANSIENT — BEDA dengan temp1
  ) {}

  @Get()
  test() {
    return {
      contextSameId: this.context1.getRequestId() === this.context2.getRequestId(),  // true
      tempSameId: this.temp1.getId() === this.temp2.getId(),                          // false
    };
  }
}
```

## Analogi (Gedung Bertingkat)

DI Container = **Gudang alat pusat** di basement gedung.

Cara tradisional (tanpa DI):
> Setiap lantai (kelas) membeli alatnya sendiri-sendiri. Lantai 2 beli bor, lantai 3 beli bor juga — duplikasi, boros, dan kalau bor rusak, tiap lantai harus ganti sendiri.

Dengan DI:
> Semua alat ada di **gudang pusat (DI Container)**. Lantai 2 tinggal bilang "saya butuh bor" → gudang mengirimkan bor yang sama (singleton). Lantai 3 juga pakai bor yang sama. Kalau bor rusang, ganti sekali di gudang, semua lantai kebagian bor baru.

**Scope** mengatur siapa saja yang memakai alat yang sama:
- **DEFAULT (Singleton)** = 1 bor untuk seluruh gedung — semua lantai pakai bor yang sama
- **REQUEST** = 1 bor per ruangan — setiap tamu (request) dapat bor sendiri
- **TRANSIENT** = 1 bor baru setiap kali dipinjam — setiap kali lantai minta bor, dikasih bor baru

## Dipakai Untuk Apa

- **Semua** komunikasi antar komponen di NestJS — Controller ke Service, Service ke Repository
- **Unit testing** — mock service dengan mudah tanpa mengubah kode
- **Cross-cutting concerns** — Logger, Config, Cache di-inject ke mana saja
- **Request-scoped data** — menyimpan data per-request seperti user context

## Kesalahan Umum

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| Constructor injection terlalu banyak (>5-6 parameter) | Kelas melanggar SRP | Refactor — mungkin perlu module/fitur terpisah |
| Lupa daftarkan provider di module | Error `Nest can't resolve dependencies` | Tambahkan ke `providers` di module |
| Pakai REQUEST/TRANSIENT tanpa sadar | Memory leak, garbage | Gunakan DEFAULT kecuali ada alasan kuat |
| @Injectable() dipasang tapi class tidak di-inject | Tidak masalah tapi mubazir | Hapus @Injectable() jika tidak di-inject |
| Tidak pakai DI, manual `new Something()` | Tight coupling, susah di-test | Pindahkan ke constructor injection |

## Soal Latihan & Jawaban

### Soal 1
Buatlah 3 service dengan scope berbeda:
- `AppConfigService` (singleton) — menyimpan konfigurasi aplikasi
- `RequestLogger` (request) — mencatat request ID setiap HTTP request
- `UuidGenerator` (transient) — menghasilkan UUID baru setiap kali di-inject

**Jawaban:**

```typescript
// app-config.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()  // DEFAULT = singleton
export class AppConfigService {
  private readonly config = {
    appName: 'MyApp',
    version: '1.0.0',
    port: 3000,
  };

  get(key: string): any {
    return this.config[key];
  }
}

// request-logger.service.ts
import { Injectable, Scope, Inject } from '@nestjs/common';
import { REQUEST } from '@nestjs/core';

@Injectable({ scope: Scope.REQUEST })
export class RequestLogger {
  private readonly timestamp: string;

  constructor(@Inject(REQUEST) private readonly request: any) {
    this.timestamp = new Date().toISOString();
  }

  log(message: string): void {
    const method = this.request?.method ?? 'UNKNOWN';
    const url = this.request?.url ?? 'UNKNOWN';
    console.log(`[${this.timestamp}] ${method} ${url} - ${message}`);
  }
}

// uuid-generator.service.ts
import { Injectable, Scope } from '@nestjs/common';
import { randomUUID } from 'crypto';

@Injectable({ scope: Scope.TRANSIENT })
export class UuidGenerator {
  private readonly uuid: string;

  constructor() {
    this.uuid = randomUUID();
  }

  generate(): string {
    return this.uuid;
  }
}
```

### Soal 2
Apa yang dimaksud dengan DI Container dalam NestJS?

**Jawaban:**
DI Container adalah registry pusat yang menyimpan semua provider yang terdaftar di module. Ketika sebuah kelas membutuhkan dependency (melalui constructor), NestJS akan mencari dependency tersebut di DI Container, membuatkannya (beserta dependensinya secara rekursif), dan menginject-kannya. Container juga mengelola scope (singleton/request/transient).

### Soal 3
Apa perbedaan antara `Scope.DEFAULT` dan `Scope.REQUEST`?

**Jawaban:**
- `Scope.DEFAULT`: Satu instance provider dibuat dan dibagikan ke seluruh aplikasi (singleton). Semua request dan semua pengguna memakai instance yang sama.
- `Scope.REQUEST`: Instance baru dibuat untuk setiap HTTP request. Setiap request punya instancenya sendiri. Cocok untuk data yang spesifik per-request (misal: user context, request ID).
