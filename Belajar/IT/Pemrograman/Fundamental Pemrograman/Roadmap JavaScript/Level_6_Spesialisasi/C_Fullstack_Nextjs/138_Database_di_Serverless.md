# 138: Database di Serverless

## 1. Penjelasan

Database tradisional (PostgreSQL/MySQL vanilla) menggunakan TCP connection pool — jumlah koneksi terbatas. Di lingkungan serverless di mana setiap request bisa memicu fungsi baru, koneksi akan cepat habis (**connection exhaustion**).

Solusi: **Serverless database** dengan **serverless driver** — driver yang menggunakan HTTP (bukan TCP) sehingga koneksi bersifat _on-demand_ tanpa pool terbatas.

| Platform | Type | Driver | Keunggulan |
|----------|------|--------|------------|
| **PlanetScale** | MySQL-compatible | `@planetscale/database` | Serverless driver HTTP, branching seperti Git |
| **Neon** | PostgreSQL-compatible | `@neondatabase/serverless` | Pooling otomatis, cold start cepat, branch |
| **Turso** | SQLite-edge | `@libsql/client` | Embedded di edge, latency sangat rendah |

## 2. Fungsi

- **Connection pooling**: Neon auto-pool koneksi — Anda tidak perlu khawatir batas koneksi.
- **HTTP-based driver**: PlanetScale dan Neon pakai HTTP — setiap request buka koneksi, tutup setelah selesai.
- **Branching DB**: PlanetScale dan Neon punya branching — buat branch untuk development/test tanpa ganggu production.
- **Edge-compatible**: Turso bisa jalan di edge (SQLite embedded).
- **Prisma + Serverless**: Prisma bisa diintegrasi dengan driver serverless untuk menghindari pool exhaustion.

## 3. Code — Integrasi Next.js + Neon + Prisma

### Setup Prisma dengan Neon

```prisma
// schema.prisma
generator client {
  provider        = "prisma-client-js"
  previewFeatures = ["driverAdapters"]
}

datasource db {
  provider  = "postgresql"
  url       = env("DATABASE_URL")
}

model Buku {
  id        String @id @default(cuid())
  judul     String
  pengarang String
  dipinjam  Boolean @default(false)
}
```

### Prisma Client dengan Neon Driver

```tsx
// lib/prisma.ts
import { PrismaClient } from '@prisma/client'
import { PrismaNeon } from '@prisma/adapter-neon'
import { neon } from '@neondatabase/serverless'

const connectionString = process.env.DATABASE_URL!
const sql = neon(connectionString)
const adapter = new PrismaNeon({ sql, pool: false })

const globalForPrisma = globalThis as unknown as { prisma: PrismaClient }

export const prisma =
  globalForPrisma.prisma ||
  new PrismaClient({ adapter })

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma
```

### API Route — CRUD Buku

```tsx
// app/api/buku/route.ts
import { NextResponse } from 'next/server'
import { prisma } from '@/lib/prisma'

export async function GET() {
  const buku = await prisma.buku.findMany({
    orderBy: { judul: 'asc' }
  })
  return NextResponse.json(buku)
}

export async function POST(req: Request) {
  const body = await req.json()
  const buku = await prisma.buku.create({
    data: {
      judul: body.judul,
      pengarang: body.pengarang,
    }
  })
  return NextResponse.json(buku, { status: 201 })
}
```

### Server Component — Rendering Data

```tsx
// app/perpustakaan/page.tsx
import { prisma } from '@/lib/prisma'

export default async function PerpustakaanPage() {
  const buku = await prisma.buku.findMany({
    where: { dipinjam: false }
  })

  return (
    <div>
      <h1>Buku Tersedia</h1>
      <ul>
        {buku.map((b) => (
          <li key={b.id}>{b.judul} — {b.pengarang}</li>
        ))}
      </ul>
    </div>
  )
}
```

### PlanetScale Alternative

```tsx
// lib/planetscale.ts
import { connect } from '@planetscale/database'

const config = {
  host: process.env.DATABASE_HOST!,
  username: process.env.DATABASE_USERNAME!,
  password: process.env.DATABASE_PASSWORD!,
}

export const db = connect(config)

// Usage
// const result = await db.execute('SELECT * FROM buku WHERE dipinjam = ?', [false])
```

## 4. Analogi Rumah

| Konsep Database | Analogi Rumah |
|----------------|---------------|
| Database tradisional (connection pool) | Tandon air sendiri di atap — kapasitas terbatas, harus diisi manual |
| Serverless database (Neon/PlanetScale) | Sambungan langsung ke PDAM — buka kran, air langsung ada, tak terbatas |
| HTTP-based driver | Kran air — buka = dapat air, tutup = berhenti. Tidak perlu ember |
| TCP connection pool | Ember-ember diisi air dari PDAM — terbatas jumlah ember |
| Connection exhaustion | Semua ember penuh — tidak ada ember kosong, antre |
| Neon branching | Cabang pipa PDAM — jika renovasi, tutup cabang itu saja, yang lain jalan |
| Turso (SQLite edge) | Galon air minum di setiap kamar — sangat dekat, cepat |
| Prisma adapter | Sambungan kran universal — cocok ke PDAM mana pun |
| `pool: false` | Kran sekali pakai — buka, pakai, tutup. Tidak disimpan |

## 5. Use Case

- **Aplikasi serverless full-stack**: Next.js + Neon + Prisma — deployment ke Vercel tanpa khawatir koneksi.
- **Multi-branch development**: PlanetScale branch untuk tiap fitur — test tanpa takut ganggu production.
- **Edge-rendered apps**: Turso untuk data yang perlu diakses sangat cepat di edge.
- **Startup dengan trafik spike**: Auto-scaling DB tanpa provisioning.

## 6. Kesalahan Umum

| Kesalahan | Seharusnya |
|-----------|-----------|
| Pakai Prisma tanpa driver adapter di serverless | Koneksi TCP default bisa overload — gunakan `@prisma/adapter-neon` atau `@prisma/adapter-planetscale` |
| Akses DB di Edge middleware | Edge runtime tidak support TCP — jika harus, gunakan Turso (SQLite HTTP) |
| Lupa environment variable di Vercel | Set `DATABASE_URL` di Vercel dashboard — jangan hardcode |
| Tidak handle error koneksi | Serverless DB bisa timeout — tambahkan try-catch dan retry logic |
| Branch production di-branch langsung | Branch dulu, test, lalu deploy — jangan main `push` ke production branch |

## 7. Benang Merah

137 (Serverless) → **138 Database Serverless** — Solusi penyimpanan yang cocok dengan arsitektur serverless. Lanjut ke 139 (Monorepo) untuk mengelola kode dalam satu repo.

## 8. Soal

### Soal 1 — Connection Pooling
Apa yang terjadi jika 100 fungsi serverless memanggil database PostgreSQL biasa secara bersamaan?
**Jawaban:** Connection pool (misal 10 koneksi) akan penuh — 90 fungsi lainnya antre atau timeout. Ini disebut connection exhaustion.

### Soal 2 — Serverless Driver
Bagaimana HTTP-based driver (Neon/PlanetScale) menyelesaikan masalah connection exhaustion?
**Jawaban:** Setiap request buka koneksi HTTP baru, query, lalu tutup. Tidak perlu pool — jadi tidak ada batas koneksi. Database serverless di sisi server yang mengelola pooling.

### Soal 3 — Edge vs Serverless DB
Mengapa Turso (SQLite) lebih cocok untuk Edge Functions dibanding Neon (PostgreSQL)?
**Jawaban:** Turso menggunakan SQLite yang bisa di-embed dan diakses via HTTP ringan — cocok untuk edge runtime yang terbatas. Neon butuh koneksi TCP/HTTP ke server pusat — latency lebih tinggi dari edge.
