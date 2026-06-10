# 139: Monorepo dengan Turborepo

## 1. Penjelasan

**Monorepo**: Satu repository berisi banyak proyek (packages). Berbeda dengan **multirepo** di mana setiap proyek punya repo sendiri.

**Turborepo** adalah tool build system untuk monorepo JavaScript/TypeScript yang mengoptimalkan waktu build dengan:
- **Caching**: Hasil build di-cache — jika kode tidak berubah, pakai cache.
- **Parallel execution**: Build package independen secara paralel.
- **Task orchestration**: Tentukan urutan task (build dulu, baru test).

## 2. Fungsi

- **Shared packages**: `packages/shared` (types), `packages/ui` (komponen), `packages/configs` (eslint, tsconfig).
- **Publishing packages**: Package bisa di-publish ke npm atau digunakan internal.
- **Apps**: `apps/web` (Next.js), `apps/api` (Express) — konsumsi shared packages.
- **Single versioning**: Satu `node_modules`, satu lockfile — lebih konsisten.
- **Atomic commits**: Commit yang mengubah shared types + consumer dalam satu PR.

## 3. Code — Monorepo Perpustakaan

### Struktur Folder

```
monorepo-perpustakaan/
├── apps/
│   ├── web/               # Next.js (frontend)
│   └── api/               # Express (backend API)
├── packages/
│   ├── shared/            # Types & utilities
│   ├── ui/                # Shared components
│   ├── eslint-config/     # ESLint config
│   └── tsconfig/          # TypeScript base config
├── turbo.json             # Turborepo config
└── package.json           # Root workspace
```

### Root package.json

```json
{
  "name": "monorepo-perpustakaan",
  "private": true,
  "workspaces": ["apps/*", "packages/*"],
  "scripts": {
    "dev": "turbo dev",
    "build": "turbo build",
    "lint": "turbo lint",
    "test": "turbo test"
  },
  "devDependencies": {
    "turbo": "^2.0.0"
  }
}
```

### turbo.json

```json
{
  "$schema": "https://turbo.build/schema.json",
  "tasks": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": [".next/**", "dist/**"]
    },
    "dev": {
      "cache": false,
      "persistent": true
    },
    "lint": {},
    "test": {
      "dependsOn": ["build"]
    }
  }
}
```

### packages/shared — Types

```tsx
// packages/shared/src/index.ts
export interface Buku {
  id: string
  judul: string
  pengarang: string
  isbn: string
  tahunTerbit: number
  dipinjam: boolean
}

export type CreateBukuInput = Omit<Buku, 'id' | 'dipinjam'>
```

### packages/ui — Shared Component

```tsx
// packages/ui/src/Card.tsx
export function Card({ children }: { children: React.ReactNode }) {
  return (
    <div className="rounded-lg border p-4 shadow-sm">
      {children}
    </div>
  )
}
```

```tsx
// packages/ui/src/index.ts
export { Card } from './Card'
export { Button } from './Button'
```

### apps/web — Next.js Consumer

```tsx
// apps/web/package.json
{
  "name": "web",
  "dependencies": {
    "shared": "*",
    "ui": "*",
    "next": "^14.0.0"
  }
}
```

```tsx
// apps/web/app/buku/[id]/page.tsx
import { Card } from 'ui'
import type { Buku } from 'shared'

async function getBuku(id: string): Promise<Buku> {
  const res = await fetch(`http://api:3001/buku/${id}`)
  return res.json()
}

export default async function BukuDetail({ params }: { params: { id: string } }) {
  const buku = await getBuku(params.id)

  return (
    <Card>
      <h1>{buku.judul}</h1>
      <p>{buku.pengarang} ({buku.tahunTerbit})</p>
      <p>ISBN: {buku.isbn}</p>
      <p>Status: {buku.dipinjam ? 'Dipinjam' : 'Tersedia'}</p>
    </Card>
  )
}
```

### apps/api — Express Backend

```tsx
// apps/api/src/index.ts
import express from 'express'
import type { Buku, CreateBukuInput } from 'shared'

const app = express()
app.use(express.json())

const bukuDB: Buku[] = []

app.get('/buku/:id', (req, res) => {
  const buku = bukuDB.find((b) => b.id === req.params.id)
  res.json(buku)
})

app.post('/buku', (req, res) => {
  const input: CreateBukuInput = req.body
  const buku: Buku = { id: String(bukuDB.length + 1), ...input, dipinjam: false }
  bukuDB.push(buku)
  res.status(201).json(buku)
})

app.listen(3001)
```

### Publishing Package

```bash
# Build shared package
npm run build --workspace=packages/shared

# Publish ke npm
npm publish --workspace=packages/shared

# Atau gunakan changeset untuk versioning otomatis
npx changeset
npx changeset version
npx changeset publish
```

## 4. Analogi Rumah

| Konsep Monorepo | Analogi Rumah |
|----------------|---------------|
| Monorepo | Satu gudang material untuk seluruh kompleks perumahan |
| Multirepo | Setiap rumah punya gudang sendiri — boros, duplikasi |
| Turborepo | Manajer logistik yang atur pengiriman material ke tiap rumah |
| Cache Turborepo | Catatan stok gudang — jika material sudah ada, tidak beli lagi |
| `turbo.json` | Jadwal kerja — "Pondasi dulu, baru dinding, baru atap" |
| `packages/shared` | Pipa standar — semua rumah pakai ukuran yang sama |
| `packages/ui` | Pintu dan jendela siap pasang — produksi massal |
| `apps/web` | Rumah model A — butuh material (pipa, pintu) dari gudang yang sama |
| `apps/api` | Rumah model B — tetap pakai material dari gudang |
| `workspaces` | Kompleks perumahan — semua rumah terdaftar di satu RT |
| `@repo/shared` | Label "produksi kompleks perumahan" — asli dari pabrik sendiri |

## 5. Use Case

- **Aplikasi multi-platform**: Web (Next.js) + Mobile (React Native) + API (Express) — semua pakai shared types.
- **Design system**: Tim UI/UX buat komponen di `packages/ui`, dikonsumsi semua apps.
- **Microservices**: Banyak service kecil — satu repo memudahkan koordinasi perubahan.
- **Open source library**: Contoh — Turborepo, Next.js, Prisma sendiri pakai monorepo.

## 6. Kesalahan Umum

| Kesalahan | Seharusnya |
|-----------|-----------|
| Monorepo terlalu besar (100+ apps) | Pisah jika sudah tidak manageable — pertimbangkan Bazel atau Nx |
| Dependency hell — semua package saling depend | Atur `dependsOn` di `turbo.json`, buat dependency graph jelas |
| Ignore caching — build ulang tiap kali | Pastikan `outputs` di `turbo.json` benar agar cache bekerja |
| Lupa update shared package — consumer error | Selalu test semua consumer setelah update shared package |
| Tidak pakai TypeScript project references | Aktifkan `composite: true` di tsconfig untuk type-checking cepat |
| Versioning semrawut | Gunakan `changeset` untuk versioning terstruktur dan changelog otomatis |

## 7. Benang Merah

138 (Database Serverless) → **139 Monorepo** — Penutup Jalur Fullstack Level 6. Semua komponen (React, Next.js, tRPC, serverless, database) dikelola dalam satu repo dengan Turborepo. Ini adalah puncak arsitektur fullstack modern.

## 8. Soal

### Soal 1 — Monorepo vs Multirepo
Sebutkan 2 keuntungan monorepo dibanding multirepo.
**Jawaban:**
1. **Atomic commit**: Satu commit bisa mengubah shared types dan consumer — tidak perlu sinkronisasi antar repo.
2. **Konsistensi**: Satu versi dependency, satu tooling (eslint, tsconfig) untuk semua proyek.

### Soal 2 — Cache Turborepo
Apa yang terjadi jika file di `packages/shared/src/index.ts` tidak berubah, lalu kita jalankan `turbo build`?
**Jawaban:** Turborepo akan menggunakan cache — tidak menjalankan build untuk `packages/shared`. Namun, `apps/web` yang bergantung pada shared tetap akan di-build (karena `dependsOn: ["^build"]` memastikan shared sudah di-cache, lalu web perlu di-build ulang).

### Soal 3 — Task Orchestration
Dalam `turbo.json` berikut, apa urutan eksekusi task?
```json
{
  "tasks": {
    "build": { "dependsOn": ["^build"] },
    "test": { "dependsOn": ["build"] },
    "lint": {}
  }
}
```
**Jawaban:**
1. `lint` — bisa jalan kapan saja (paralel, tidak depend)
2. `build` — jalan setelah dependensi `^build` selesai (build dulu package yang jadi dependensi)
3. `test` — jalan setelah `build` selesai untuk package tersebut
