# 131: Micro-Frontend Architecture

## 1. Penjelasan
Micro-frontend memecah aplikasi frontend besar menjadi beberapa bagian kecil yang dikembangkan, di-deploy, dan di-maintain secara independen oleh tim berbeda. Integrasi bisa via iframe, Web Components, atau Module Federation (Webpack 5 / Vite). Setiap micro-frontend bisa menggunakan framework berbeda, asalkan diisolasi dengan baik.

## 2. Fungsi
- Tim besar bisa kerja paralel tanpa konflik
- Deploy independen — tim rilis fitur sendiri
- Isolasi gaya (CSS) agar tidak bentrok antar tim
- Framework-agnostic — tim pakai Vue, React, atau Svelte
- Skalabilitas organisasi — kodebase tidak monolitik
- Module Federation memungkinkan berbagi dependency (vendor sama tidak di-download ulang)

## 3. Code

```ts
// Shell app (host) — nuxt.config.ts Module Federation
import { ModuleFederationPlugin } from '@module-federation/vite'

export default defineNuxtConfig({
  vite: {
    plugins: [
      ModuleFederationPlugin({
        name: 'shell',
        remotes: {
          'books-app': 'http://localhost:3001/assets/remoteEntry.js',
          'members-app': 'http://localhost:3002/assets/remoteEntry.js'
        },
        shared: ['vue', 'pinia'] // shared dependency
      })
    ]
  }
})
```

```vue
<!-- Shell app — memuat remote micro-frontend -->
<template>
  <div>
    <header>
      <nav>
        <NuxtLink to="/">Beranda</NuxtLink>
        <NuxtLink to="/books">Buku</NuxtLink>
        <NuxtLink to="/members">Anggota</NuxtLink>
      </nav>
    </header>

    <main>
      <RouterView />
    </main>

    <footer>© Perpustakaan Terpadu</footer>
  </div>
</template>
```

```vue
<!-- Remote app: Books (Vue, port 3001) -->
<template>
  <div class="books-app" shadow="isolate">
    <h2>Manajemen Buku</h2>
    <BookList />
  </div>
</template>

<script>
// Module Federation expose
export default {
  name: 'BooksApp',
  expose: './BooksApp'
}
</script>
```

```ts
// CSS isolation — shadow DOM atau scoped styles
// Opsi 1: Shadow DOM (Web Components)
<template>
  <div :shadowroot="'open'">
    <style>
      /* styles hanya berlaku di sini */
    </style>
    <slot />
  </div>
</template>

// Opsi 2: CSS Modules / Scoped
<style scoped>
  .books-app { font-family: Arial; }
</style>
```

## 4. Analogi Rumah

| Konsep Micro-Frontend | Analogi Rumah |
|-----------------------|---------------|
| Shell app | Kompleks perumahan — jalan utama, taman, gerbang |
| Remote app | Setiap rumah dibangun kontraktor berbeda |
| Module Federation | Jalan penghubung antar rumah — saling berbagi utilitas |
| Shared dependencies | Pipa gas & air bersama — tidak perlu pasang sendiri |
| CSS isolation | Pagar rumah — gaya rumah A tidak mengganggu rumah B |
| Independent deploy | Renovasi rumah B tanpa ganggu rumah A |

## 5. Use Case
- Organisasi besar dengan banyak tim (e-commerce: tim produk, tim cart, tim checkout)
- Aplikasi legacy bertahap migrasi ke framework baru
- Aplikasi perpustakaan: tim buku, tim anggota, tim laporan — micro-frontend terpisah
- SaaS platform dengan modul berbeda

## 6. Kesalahan Umum
- **Terlalu kecil fragmentasi** → setiap komponen jadi micro-frontend (overhead). Batasi per domain bisnis.
- **CSS global bentrok** → gunakan scoped styles, CSS Modules, atau shadow DOM.
- **Shared dependency versi beda** → pastikan versi Vue/Pinia sama antar remote agar tidak double-load.
- **Testing integration kompleks** → tambahkan E2E test yang mencakup shell + semua remote.

## 7. Benang Merah
Testing (130) harus mencakup integration test antar micro-frontend. WebAssembly (132) bisa dijadikan remote service dalam micro-frontend. Animasi (133) harus konsisten di semua fragment. Pattern ini adalah puncak skalabilitas frontend engineering.

## 8. Soal

**Soal 1:** Apa keuntungan utama micro-frontend dibanding monolith frontend?

**Soal 2:** Sebutkan tiga metode integrasi micro-frontend.

**Soal 3:** Bagaimana cara menghindari bentrokan CSS antar micro-frontend?

---
<details>
<summary>Jawaban</summary>

**Jawaban 1:** Deploy independen (tim rilis sendiri tanpa koordinasi), paralelisasi kerja tim, isolasi kegagalan (satu remote error tidak menjatuhkan seluruh app), dan migrasi bertahap (bisa campur framework).

**Jawaban 2:** iframe (isolasi total tapi berat), Web Components (native isolation), Module Federation (berbagi dependency, performa baik).

**Jawaban 3:** Gunakan scoped styles framework (`<style scoped>` di Vue), CSS Modules, atau shadow DOM untuk isolasi total. Hindari global CSS selector. Prefix class jika terpaksa pakai global.
</details>
