# 47 - Prisma Advanced - Transactions, Raw Query & Middleware

## Penjelasan

Setelah menguasai operasi CRUD dasar (Pertemuan 46), kita perlu fitur-fitur lanjutan untuk menangani skenario kompleks. Transaction memastikan beberapa operasi database berhasil semua atau gagal semua (atomicity). Raw query digunakan ketika query Prisma tidak mencukupi. Middleware memungkinkan kita menyisipkan logika di setiap operasi database.

Jika CRUD dasar adalah **saklar lampu biasa**, maka transaction adalah **sistem kelistrikan terintegrasi** — jika satu mati, semua mati (biar aman). Raw query adalah **kabel jumper darurat**. Middleware adalah **sensor otomatis** yang aktif setiap ada aktivitas listrik.

## Fungsi

- **$transaction([...])**: Batch transaction — array of query, atomic
- **$transaction(async(tx))**: Interactive transaction — control penuh
- **Optimistic Locking**: Mencegah race condition dengan version field
- **$executeRaw / $queryRaw**: Query SQL mentah untuk skenario kompleks
- **Middleware (interactive)**: Hook sebelum/sesudah query (logging, soft delete)

## Cara Pengimplementasian

### 1. Batch Transaction

```typescript
import { PrismaService } from '../prisma/prisma.service';

async transferCredit(prisma: PrismaService, fromId: number, toId: number, amount: number) {
  const [fromAccount, toAccount] = await prisma.$transaction([
    prisma.account.update({
      where: { id: fromId },
      data: { balance: { decrement: amount } },
    }),
    prisma.account.update({
      where: { id: toId },
      data: { balance: { increment: amount } },
    }),
  ]);

  return { fromAccount, toAccount };
}
```

### 2. Interactive Transaction

```typescript
async transferCreditInteractive(
  prisma: PrismaService,
  fromId: number,
  toId: number,
  amount: number,
) {
  return prisma.$transaction(async (tx) => {
    // 1. Cek saldo pengirim
    const fromAccount = await tx.account.findUnique({
      where: { id: fromId },
    });

    if (!fromAccount || fromAccount.balance < amount) {
      throw new Error('Insufficient balance');
    }

    // 2. Kurangi saldo pengirim
    await tx.account.update({
      where: { id: fromId },
      data: { balance: { decrement: amount } },
    });

    // 3. Tambah saldo penerima
    await tx.account.update({
      where: { id: toId },
      data: { balance: { increment: amount } },
    });

    // 4. Catat transaction log
    await tx.transactionLog.create({
      data: {
        fromId,
        toId,
        amount,
        type: 'TRANSFER',
      },
    });

    return { message: 'Transfer successful' };
  });
}
```

### 3. Optimistic Locking

```prisma
model Account {
  id      Int  @id @default(autoincrement())
  balance Int  @default(0)
  version Int  @default(1) // untuk optimistic locking
}
```

```typescript
async updateWithOptimisticLock(prisma: PrismaService, accountId: number, amount: number) {
  // Baca current version
  const account = await prisma.account.findUnique({
    where: { id: accountId },
  });

  // Update hanya jika version masih sama
  const result = await prisma.account.updateMany({
    where: {
      id: accountId,
      version: account.version, // guard: pastikan tidak ada perubahan
    },
    data: {
      balance: { increment: amount },
      version: { increment: 1 },
    },
  });

  if (result.count === 0) {
    throw new Error('Conflict: data was modified by another request');
  }

  return prisma.account.findUnique({ where: { id: accountId } });
}
```

### 4. Raw Query ($executeRaw / $queryRaw)

```typescript
// $executeRaw — untuk INSERT, UPDATE, DELETE
const result = await prisma.$executeRaw`
  UPDATE posts
  SET views = views + 1
  WHERE id = ${postId}
`;

// $queryRaw — untuk SELECT
const posts = await prisma.$queryRaw`
  SELECT p.*, u.name as author_name
  FROM posts p
  JOIN users u ON u.id = p."authorId"
  WHERE p.published = true
  ORDER BY p."createdAt" DESC
  LIMIT ${limit} OFFSET ${offset}
`;

// Hati-hati dengan SQL injection — Prisma tagged template aman
```

### 5. Middleware (Prisma Middleware — versi lama)

Di versi Prisma 4+, middleware sudah diganti dengan `$extends`. Untuk versi sebelumnya:

```typescript
// prisma.service.ts
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

// Middleware untuk logging
prisma.$use(async (params, next) => {
  const before = Date.now();
  const result = await next(params);
  const after = Date.now();

  console.log(`Query ${params.model}.${params.action} took ${after - before}ms`);
  return result;
});

// Middleware untuk soft delete
prisma.$use(async (params, next) => {
  // Intercept findMany/findFirst/findUnique pada model tertentu
  if (['Post', 'Product'].includes(params.model)) {
    if (params.action === 'findMany' || params.action === 'findFirst') {
      if (!params.args) params.args = {};
      if (!params.args.where) params.args.where = {};
      params.args.where.deletedAt = null;
    }
  }
  return next(params);
});
```

### Prisma $extends (Prisma 5+)

```typescript
// prisma.service.ts
import { PrismaClient } from '@prisma/client';

export const prisma = new PrismaClient().$extends({
  query: {
    post: {
      async $allOperations({ model, operation, args, query }) {
        const before = Date.now();
        const result = await query(args);
        const after = Date.now();
        console.log(`${model}.${operation} took ${after - before}ms`);
        return result;
      },
    },
  },
});
```

## Analogi

**Membangun Gedung Bertingkat**

- **$transaction([...])** = **saklar master** — kalau lampu di 1 ruangan mati, seluruh lantai mati (biar aman dari korsleting)
- **Interactive transaction** = **inspektur proyek** yang mengawasi setiap langkah renovasi — "sudah pasang kabel? sudah tes grounding? ok lanjut"
- **Optimistic Locking** = **sistem antrian material** — "kamu pikir masih ada 10 sak semen, tapi ternyata sudah dipakai orang lain"
- **$executeRaw** = **memanggil tukang spesialis** untuk perbaikan yang terlalu rumit untuk alat standar
- **Middleware** = **CCTV dan sensor** yang merekam setiap aktivitas di gedung

## Dipakai untuk Apa

- Transfer dana antar akun (harus atomic)
- Booking sistem (cek ketersediaan + booking dalam satu transaksi)
- Audit log yang harus konsisten
- Report kompleks yang butuh SQL khusus
- Soft delete otomatis tanpa ubah query manual

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Tidak pakai transaction untuk operasi multi-step | Data tidak konsisten jika salah satu step gagal |
| Query raw tanpa parameter binding | Risiko SQL injection — selalu pakai tagged template |
| Middleware terlalu lambat | Hanya gunakan middleware untuk operasi yang perlu |
| Lupa handle rollback di interactive transaction | `$transaction` otomatis rollback jika throw error |
| Optimistic locking tanpa retry logic | Implementasikan retry mechanism di application layer |

## Soal Latihan

Implementasikan transfer kredit antar akun dengan ketentuan:
1. Gunakan interactive transaction
2. Cek saldo pengirim sebelum transfer
3. Kurangi saldo pengirim, tambah saldo penerima
4. Catat log transaksi di tabel terpisah
5. Jika saldo tidak cukup, throw error

### Jawaban

**Schema:**
```prisma
model Account {
  id        Int    @id @default(autoincrement())
  name      String
  balance   Int    @default(0)
}

model TransactionLog {
  id     Int      @id @default(autoincrement())
  fromId Int
  toId   Int
  amount Int
  type   String
  createdAt DateTime @default(now())
}
```

**Service:**
```typescript
import { Injectable } from '@nestjs/common';
import { PrismaService } from '../prisma/prisma.service';

@Injectable()
export class AccountService {
  constructor(private prisma: PrismaService) {}

  async transfer(fromId: number, toId: number, amount: number) {
    if (amount <= 0) {
      throw new Error('Amount must be positive');
    }

    return this.prisma.$transaction(async (tx) => {
      const fromAccount = await tx.account.findUnique({
        where: { id: fromId },
      });

      if (!fromAccount || fromAccount.balance < amount) {
        throw new Error('Insufficient balance');
      }

      await tx.account.update({
        where: { id: fromId },
        data: { balance: { decrement: amount } },
      });

      await tx.account.update({
        where: { id: toId },
        data: { balance: { increment: amount } },
      });

      await tx.transactionLog.create({
        data: { fromId, toId, amount, type: 'TRANSFER' },
      });

      return { message: `Transferred ${amount} from account ${fromId} to ${toId}` };
    });
  }
}
```
