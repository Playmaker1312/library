# 98 - Database Optimization - Query Index & Connection Pool

## Penjelasan
Setelah adapter Fastify (materi 97) mempercepat HTTP layer, bottleneck berikutnya biasanya *database*. Dua masalah paling umum: **N+1 query** (terlalu banyak query) dan **koneksi database habis** (connection pool tidak optimal). NestJS dengan Prisma ORM memiliki tool untuk mengidentifikasi dan memperbaiki keduanya.

## Fungsi
- **N+1 problem**: 1 query untuk parent + N query untuk setiap child → solve dengan `include` / DataLoader.
- **Prisma query logging**: `prisma.$on('query')` untuk melihat query yang dikirim ke database.
- **Database indexing**: Mempercepat query SELECT dengan index di kolom yang sering difilter.
- **Connection pooling**: Prisma Data Proxy / PgBouncer — handle ribuan koneksi simultan tanpa membebani database.

## Cara Pengimplementasian

### Identifikasi N+1 Problem

**Setup Query Logging**
```typescript
// prisma.service.ts
import { Injectable, Logger } from '@nestjs/common';
import { PrismaClient } from '@prisma/client';

@Injectable()
export class PrismaService extends PrismaClient {
  private readonly logger = new Logger(PrismaService.name);

  constructor() {
    super({
      log: [
        { emit: 'event', level: 'query' },
        { emit: 'stdout', level: 'info' },
        { emit: 'stdout', level: 'warn' },
        { emit: 'stdout', level: 'error' },
      ],
    });

    // Log setiap query
    this.$on('query' as any, (e: any) => {
      this.logger.debug(`Query: ${e.query}`);
      this.logger.debug(`Params: ${e.params}`);
      this.logger.debug(`Duration: ${e.duration}ms`);
    });
  }
}
```

**Kode yang Mengandung N+1**
```typescript
// ❌ BURUK — N+1 problem
@Injectable()
export class PostsService {
  constructor(private prisma: PrismaService) {}

  async findAll() {
    const posts = await this.prisma.post.findMany();          // 1 query
    for (const post of posts) {
      post.author = await this.prisma.user.findUnique({       // N query
        where: { id: post.authorId },
      });
    }
    return posts;
  }
}
```

**Fix N+1 dengan `include`**
```typescript
// ✅ BAIK — Single query dengan include
async findAll() {
  return this.prisma.post.findMany({
    include: {
      author: true,            // JOIN — 1 query total
      comments: {
        include: { user: true },
      },
    },
  });
}
```

**Fix N+1 dengan DataLoader untuk Loop**
Jika butuh operasi kompleks yang tidak bisa pakai include:
```typescript
import DataLoader from 'dataloader';

@Injectable()
export class UsersLoader {
  constructor(private prisma: PrismaService) {}

  createUsersLoader() {
    return new DataLoader(async (ids: number[]) => {
      const users = await this.prisma.user.findMany({
        where: { id: { in: ids } },  // Batch: 1 query, bukan N
      });
      return ids.map(id => users.find(u => u.id === id));
    });
  }
}
```

### Database Indexing

**Prisma Schema dengan Index**
```prisma
model User {
  id        Int     @id @default(autoincrement())
  email     String  @unique          // Unique index (auto)
  role      Role    @default(USER)
  status    Status  @default(ACTIVE)
  createdAt DateTime @default(now())

  @@index([role, status])            // Composite index
  @@index([createdAt])               // Single index untuk sorting/filter
}

model Order {
  id         Int      @id @default(autoincrement())
  userId     Int
  total      Decimal
  status     OrderStatus
  createdAt  DateTime @default(now())

  @@index([userId, status])          // Query: find orders by user + status
  @@index([status, createdAt])       // Query: find recent orders by status
}
```

**Membuat Migration Index**
```bash
npx prisma migrate dev --name add-order-indexes
```

### Connection Pooling

**Prisma Connection Pool Default**
```typescript
// Prisma pooling via connection string
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

// DATABASE_URL="postgresql://user:pass@host:5432/db?connection_limit=10&pool_timeout=30"
```

**Prisma Data Proxy — Managed Pooling**
```bash
# Di Prisma schema
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

# DATABASE_URL — pakai Data Proxy URL dari Prisma Cloud
```

**PgBouncer — External Connection Pooler**
```yaml
# docker-compose.yml
services:
  pgbouncer:
    image: edoburu/pgbouncer:latest
    environment:
      - DB_HOST=postgres
      - DB_USER=myuser
      - DB_PASSWORD=mypass
      - DB_NAME=mydb
      - POOL_MODE=transaction
      - MAX_CLIENT_CONN=200    # Maks koneksi ke PgBouncer
      - DEFAULT_POOL_SIZE=20   # Koneksi ke PostgreSQL (lebih sedikit)
    ports:
      - "5432:5432"
    depends_on:
      - postgres

  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypass
      POSTGRES_DB: mydb
```

### Monitoring Slow Query di Prisma
```typescript
// Slow query threshold — log query > 1 detik
this.$on('query' as any, (e: any) => {
  if (e.duration > 1000) {
    this.logger.warn(`🚨 Slow query (${e.duration}ms): ${e.query}`);
    // Kirim ke monitoring: Sentry, OpenTelemetry, dll
  }
});
```

## Analogi
Gedung punya **ruang arsip** (database). **N+1 problem**: Petugas lantai 2 ingin mencari 100 file — dia ke ruang arsip 1x untuk daftar file, lalu bolak-balik 100x untuk ambil setiap file. Solusi: ambil semua 100 file sekaligus — cukup 1 trip (JOIN/DataLoader). 

**Index**: Lemari arsip punya **katalog alfabetis** (index). Tanpa index, petugas harus cek satu per satu 10.000 map untuk cari "Smith". Dengan index, langsung buka huruf S. 

**Connection pool**: Ada 200 petugas (request) yang semuanya ingin ke ruang arsip bersamaan. Tanpa pool, database kebanjiran. PgBouncer seperti **resepsionis** yang membatasi hanya 20 orang masuk ke ruang arsip — sisanya antre di lobi.

## Dipakai untuk apa
- Aplikasi dengan banyak relasi database (User → Post → Comment → Like).
- Endpoint yang lambat karena query N+1.
- API dengan traffic ribuan request/detik.
- Production database dengan koneksi terbatas (misal: 100 max connections).

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Tidak logging query Prisma | Tidak tahu N+1 terjadi | Aktifkan `log: ['query']` di PrismaClient |
| Index di setiap kolom | Write jadi lambat, storage membengkak | Index hanya untuk kolom yang sering di WHERE/JOIN |
| Connection pool terlalu besar | Database overload | Pool size = 2-4x CPU core database |
| Tidak pakai DataLoader untuk batch paralel | N+1 meski pakai `findMany` | Batch query dengan DataLoader |
| Lupa `@@index` di foreign key | JOIN jadi sequential scan | Selalu index foreign key (userId, postId, dll) |

## Soal Latihan

**Soal 1:** Identifikasi N+1 problem di kode berikut dan perbaiki:
```typescript
@Injectable()
export class CommentsService {
  constructor(private prisma: PrismaService) {}

  async getCommentsWithUser(postId: number) {
    const comments = await this.prisma.comment.findMany({
      where: { postId },
    });
    return Promise.all(
      comments.map(async (comment) => ({
        ...comment,
        user: await this.prisma.user.findUnique({ where: { id: comment.userId } }),
      })),
    );
  }
}
```

**Jawaban 1:**
```typescript
// ✅ Fix dengan include
async getCommentsWithUser(postId: number) {
  return this.prisma.comment.findMany({
    where: { postId },
    include: { user: true },
  });
}
```
Atau dengan DataLoader jika ada multiple kueri paralel.

**Soal 2:** Buat index di Prisma untuk kolom `email` dan `role` pada model User.

**Jawaban 2:**
```prisma
model User {
  id    Int    @id @default(autoincrement())
  email String @unique    // Unique — otomatis index
  role  Role  @default(USER)

  @@index([role])         // Index untuk filter by role
}
```
Jalankan `npx prisma migrate dev --name add-user-indexes`.
