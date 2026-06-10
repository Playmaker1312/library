# 135: Next.js — Full-Stack React Framework

## 1. Penjelasan

Next.js adalah framework React production-grade yang menyediakan routing, data fetching, optimasi, dan deployment. Dua pendekatan utama:

- **Pages Router** (legacy, berbasis file di `pages/`)
- **App Router** (baru, berbasis folder di `app/`, Server Components by default)

**Server Components** render di server, mengirim HTML ke client — mengurangi JavaScript bundle. **Client Components** (dengan `"use client"`) tetap interaktif seperti React biasa.

## 2. Fungsi

- **Pages Router vs App Router**: Pages Router sederhana, cocok proyek kecil. App Router lebih powerfull dengan Server Components, layout nested, loading/error file.
- **Data Fetching**:
  - SSR (Server-Side Rendering) — data di-fetch tiap request.
  - SSG (Static Site Generation) — data di-fetch saat build.
  - ISR (Incremental Static Regeneration) — SSG dengan revalidate periodik.
- **API Routes**: Buat API endpoint langsung di `app/api/` (App Router) atau `pages/api/` (Pages Router).
- **Middleware**: Fungsi yang jalan sebelum request sampai ke halaman — untuk auth, redirect, rewrite.

## 3. Code — Fullstack Perpustakaan dengan App Router

```tsx
// app/perpustakaan/page.tsx — Server Component
import { prisma } from '@/lib/prisma'

interface Buku {
  id: string
  judul: string
  pengarang: string
  dipinjam: boolean
}

async function getBuku(): Promise<Buku[]> {
  const res = await fetch('http://localhost:3000/api/buku', {
    next: { revalidate: 60 } // ISR — revalidate tiap 60 detik
  })
  return res.json()
}

export default async function PerpustakaanPage() {
  const buku = await getBuku()

  return (
    <div>
      <h1>Perpustakaan</h1>
      {buku.map((b) => (
        <div key={b.id}>
          <h3>{b.judul}</h3>
          <p>{b.pengarang} — {b.dipinjam ? 'Dipinjam' : 'Tersedia'}</p>
        </div>
      ))}
    </div>
  )
}
```

```tsx
// app/api/buku/route.ts — API Route
import { NextResponse } from 'next/server'
import { prisma } from '@/lib/prisma'

export async function GET() {
  const buku = await prisma.buku.findMany()
  return NextResponse.json(buku)
}

export async function POST(req: Request) {
  const body = await req.json()
  const buku = await prisma.buku.create({ data: body })
  return NextResponse.json(buku, { status: 201 })
}
```

```tsx
// app/perpustakaan/[id]/page.tsx — Dynamic Route
export default async function DetailBuku({ params }: { params: { id: string } }) {
  const buku = await prisma.buku.findUnique({ where: { id: params.id } })
  return <div>{/* detail buku */}</div>
}
```

```tsx
// middleware.ts — Middleware auth
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function middleware(req: NextRequest) {
  const token = req.cookies.get('token')
  if (!token) {
    return NextResponse.redirect(new URL('/login', req.url))
  }
  return NextResponse.next()
}

export const config = {
  matcher: '/perpustakaan/:path*'
}
```

## 4. Analogi Rumah

| Konsep Next.js | Analogi Rumah |
|---------------|---------------|
| Next.js | Kontraktor yang urusin fondasi (routing), pipa (API), listrik (data fetching) |
| React | Interior rumah — soal tampilan dan interaksi |
| Pages Router | Denah rumah sederhana — 1 lantai, pintu langsung ke tiap ruang |
| App Router | Denah rumah bertingkat — ada ruang bersama (layout), koridor (nested layout) |
| Server Component | Dapur restoran — masakan sudah jadi sebelum diantar |
| Client Component | Meja makan — piring (interaksi) ada di sini |
| SSR | Katering — pesan, langsung dimasak, langsung diantar |
| SSG | Bekal seminggu — dimasak hari Minggu, tinggal panasin |
| ISR | Bekal yang di-refresh tiap 3 hari — isi berubah sedikit |
| API Routes | Kran air di setiap ruangan — ambil data langsung dari sumber |
| Middleware | Satpam di pintu masuk — cek KTP sebelum masuk ke area tertentu |
| `layout.tsx` | Dinding/atap yang menyatukan beberapa ruangan |
| `loading.tsx` | "Sedang menyiapkan ruangan" — tampilan sementara |
| `error.tsx` | "Ruangan rusak" — fallback error UI |

## 5. Use Case

- **E-commerce**: ISR untuk halaman produk (update stok periodik), SSR untuk halaman checkout (data real-time).
- **Blog/Portfolio**: SSG — build sekali, deploy ke CDN.
- **Dashboard admin**: Client Component + API Routes untuk CRUD.
- **Aplikasi multi-role**: Middleware untuk redirect user berdasarkan role.

## 6. Kesalahan Umum

| Kesalahan | Seharusnya |
|-----------|-----------|
| Fetch di Client Component padahal bisa Server | Pindahkan fetch ke Server Component — lebih cepat, lebih kecil bundle |
| Lupa `"use client"` untuk interaktivitas | Tambahkan `"use client"` di file yang pakai `onClick`, `useState`, dll |
| Membuat API Routes untuk data yang bisa di-fetch langsung di Server Component | Akses DB langsung di Server Component, skip API layer |
| Tidak menggunakan `next: { revalidate }` untuk ISR | Gunakan ISR agar halaman tidak perlu di-rebuild manual |
| Middleware terlalu berat (akses DB) | Middleware jalan di edge — hanya operasi ringan (cookie, header, redirect) |

## 7. Benang Merah

134 (React) → **135 Next.js** — React diperkuat dengan framework full-stack: routing, data fetching, API. Lanjut ke 136 (tRPC) untuk type-safe API tanpa REST.

## 8. Soal

### Soal 1 — Server vs Client Component
File `app/profil/page.tsx` menggunakan `useState` tanpa `"use client"`. Apa yang terjadi?
**Jawaban:** Error. Server Component tidak bisa menggunakan hooks. Tambahkan `"use client"` di awal file atau pisahkan bagian interaktif ke komponen terpisah.

### Soal 2 — ISR
Apa beda kode berikut?
```tsx
// A
const data = await fetch(url)

// B
const data = await fetch(url, { next: { revalidate: 30 } })
```
**Jawaban:** A = SSR (data baru tiap request). B = ISR (data di-cache, direvalidate tiap 30 detik). B lebih cepat untuk traffic tinggi.

### Soal 3 — Middleware
Buat middleware yang hanya mengizinkan akses ke `/dashboard` jika user punya cookie `role=admin`.
**Jawaban:**
```tsx
export function middleware(req: NextRequest) {
  const role = req.cookies.get('role')
  if (role !== 'admin') {
    return NextResponse.redirect(new URL('/unauthorized', req.url))
  }
  return NextResponse.next()
}
export const config = { matcher: '/dashboard/:path*' }
```
