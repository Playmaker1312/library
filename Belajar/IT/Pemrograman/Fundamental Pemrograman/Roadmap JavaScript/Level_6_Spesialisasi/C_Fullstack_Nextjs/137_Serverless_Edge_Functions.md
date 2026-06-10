# 137: Serverless & Edge Functions

## 1. Penjelasan

Serverless (FaaS — Functions as a Service) berarti kode Anda dijalankan di server yang dikelola penyedia cloud (Vercel, AWS Lambda, Netlify). Anda **tidak perlu mengelola server** — cukup push kode, platform urus skalanya.

**Cold start vs Warm:**
- **Cold start**: Fungsi pertama kali dipanggil — butuh waktu inisialisasi (bisa 200ms–1s).
- **Warm**: Fungsi sudah aktif — respons hampir instan.

**Vercel Edge Functions**: Jalan di edge network (CDN) — lebih cepat karena dekat dengan user. Menggunakan runtime JavaScript ringan (WinterCG) — tidak support Node.js API penuh.

## 2. Fungsi

- **FaaS**: Kode backend dipecah menjadi fungsi-fungsi kecil yang di-deploy independently.
- **Edge Functions**: Kode jalan di edge — cocok untuk A/B testing, geolocation redirect, autentikasi.
- **Cold start strategy**: Jaga fungsi tetap "warm" dengan pinging periodik (cron job).
- **Database serverless**: Neon, PlanetScale, Turso — DB yang bisa scaling otomatis tanpa provisioning.

## 3. Code — Deploy Next.js Perpustakaan ke Vercel + Edge Middleware Auth

### Edge Middleware — Cek Lokasi + Auth

```tsx
// middleware.ts
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function middleware(req: NextRequest) {
  const country = req.geo?.country || 'ID'
  const token = req.cookies.get('session')

  // Redirect user non-Indonesia ke halaman internasional
  if (country !== 'ID' && req.nextUrl.pathname.startsWith('/id')) {
    return NextResponse.redirect(new URL('/intl', req.url))
  }

  // Proteksi halaman dashboard
  if (req.nextUrl.pathname.startsWith('/dashboard') && !token) {
    return NextResponse.redirect(new URL('/login', req.url))
  }

  return NextResponse.next()
}

export const config = {
  matcher: ['/id/:path*', '/dashboard/:path*']
}
```

### Serverless Function — API Buku

```tsx
// app/api/buku/route.ts
import { NextResponse } from 'next/server'
import { neon } from '@neondatabase/serverless'

const sql = neon(process.env.DATABASE_URL!)

export async function GET() {
  const buku = await sql`SELECT * FROM buku LIMIT 10`
  return NextResponse.json(buku)
}
```

### next.config.js — Edge Runtime untuk API tertentu

```js
// next.config.js
module.exports = {
  experimental: {
    runtime: 'edge' // paksa semua route pake Edge Runtime (lebih cepat)
  }
}
```

### vercel.json — Konfigurasi Deploy

```json
{
  "functions": {
    "app/api/buku/route.ts": {
      "memory": 256,
      "maxDuration": 10
    }
  }
}
```

## 4. Analogi Rumah

| Konsep Serverless | Analogi Rumah |
|------------------|---------------|
| Serverless | Sewa alat sesaat — bayar per pemakaian, bukan beli alat sendiri |
| Server dedicated | Beli mesin bor untuk dipakai 1 kali — mahal dan mubazir |
| Cold start | Mesin bor perlu dicas dulu sebelum dipakai — ada delay |
| Warm start | Mesin bor sudah tercolok — langsung siap pakai |
| Edge Functions | Gerobak dorong di setiap lantai — dekat, cepat diakses |
| Cloud Functions (biasa) | Alat di gudang luar rumah — harus jalan dulu ke gudang |
| Database serverless | Sambungan PDAM on demand — buka kran, air langsung ada |
| Vercel | Toko sewa alat — lengkap, tinggal ambil |

## 5. Use Case

- **Startup / MVP**: Bayar per request — tidak keluar biaya besar untuk server idle.
- **Aplikasi musiman**: Black Friday — scaling otomatis tanpa provisioning manual.
- **Edge personalization**: Konten dinamis berdasarkan lokasi user (geolocation middleware).
- **Auth + redirect**: Middleware di edge — cek cookie/token sebelum halaman di-load.

## 6. Kesalahan Umum

| Kesalahan | Seharusnya |
|-----------|-----------|
| Cold start di API yang butuh respons <100ms | Gunakan Edge Functions (cold start lebih cepat) atau jaga warm dengan cron |
| Akses File System di Edge | Edge runtime tidak support `fs` — gunakan database atau API eksternal |
| Query DB di middleware | Middleware di edge — koneksi DB biasanya berat. Gunakan token-based check |
| Lupa set `maxDuration` di Vercel | Fungsi bisa timeout — sesuaikan durasi dengan kompleksitas |
| Tidak handle DB connection pooling di serverless | Setiap request bisa buka koneksi baru — gunakan serverless driver (Neon, PlanetScale) |

## 7. Benang Merah

136 (tRPC) → **137 Serverless** — API siap di-deploy tanpa kelola server. Lanjut ke 138 (Database Serverless) untuk solusi penyimpanan yang cocok dengan arsitektur serverless.

## 8. Soal

### Soal 1 — Cold Start
Mengapa aplikasi serverless kadang lambat di request pertama, lalu cepat di request kedua?
**Jawaban:** Request pertama mengalami cold start — fungsi harus diinisialisasi (load runtime, import module). Request kedua memanfaatkan instance yang sudah warm.

### Soal 2 — Edge vs Serverless
Kapan sebaiknya pakai Edge Functions, bukan Serverless Function biasa?
**Jawaban:** Edge cocok untuk operasi ringan dan cepat: redirect, rewrite, auth check, header manipulation, personalisasi lokasi. Jika butuh database query berat atau Node.js API penuh (fs, buffer), gunakan Serverless Function.

### Soal 3 — Database Serverless
Apa masalah utama database tradisional (PostgreSQL/MySQL biasa) di lingkungan serverless, dan bagaimana solusinya?
**Jawaban:** Masalah: setiap fungsi serverless bisa membuka koneksi baru → overload koneksi DB (connection exhaustion). Solusi: gunakan database serverless (Neon, PlanetScale) dengan serverless driver yang mendukung connection pooling otomatis atau HTTP-based query.
