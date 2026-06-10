# 125: Nuxt.js — Meta-framework Vue

## 1. Penjelasan
Nuxt.js adalah meta-framework di atas Vue yang menyediakan SSR (Server-Side Rendering), SSG (Static Site Generation), dan SPA dalam satu project. SSR merender halaman di server setiap request, SSG membangun semua halaman saat build time, SPA merender di client. Nuxt menyederhanakan routing, data fetching, dan deployment.

## 2. Fungsi
- Routing otomatis berbasis folder `pages/`
- SSR untuk SEO & performa awal lebih cepat
- SSG untuk situs statis (blog, dokumentasi)
- Auto-import komponen, composables, utilities
- Data fetching dengan `useFetch` / `useAsyncData`
- Middleware, layouts, dan plugin terstruktur

## 3. Code

```vue
<!-- pages/index.vue -->
<script setup>
const { data: books } = await useFetch('/api/books', {
  lazy: true, // tidak blocking render
  server: true // fetch di server saat SSR
})
</script>

<template>
  <div>
    <h1>Perpustakaan Online</h1>
    <BookList :books="books" />
  </div>
</template>
```

```vue
<!-- pages/books/[id].vue -->
<script setup>
const route = useRoute()
const { data: book } = await useAsyncData('book-detail', () =>
  $fetch(`/api/books/${route.params.id}`)
)
</script>

<template>
  <BookDetail v-if="book" :book="book" />
  <LoadingState v-else />
</template>
```

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  ssr: true, // aktifkan SSR
  nitro: {
    preset: 'node-server' // deploy ke Node
  },
  app: {
    pageTransition: { name: 'page', mode: 'out-in' }
  }
})
```

## 4. Analogi Rumah

| Konsep Nuxt | Analogi Rumah |
|-------------|---------------|
| SSR | Rumah *catering* — makanan dikirim siap santap (halaman siap dari server) |
| SSG | Rumah *buffet* — semua makanan sudah tersaji di meja (halaman siap saat build) |
| SPA | Restoran — masak saat dipesan (render di client) |
| `pages/` | Denah ruangan — setiap file = satu ruangan |
| `layouts/` | Tema interior — kerangka yang sama untuk tiap ruangan |
| Auto-import | Lemari serbaguna — alat langsung bisa dipakai tanpa ambil dari gudang |

## 5. Use Case
- Situs e-commerce butuh SEO tinggi → Nuxt SSR
- Blog / dokumentasi → Nuxt SSG (deploy statis)
- Dashboard admin → mode SPA (tidak butuh SEO)
- Aplikasi perpustakaan → SSR untuk halaman publik, SPA untuk dashboard anggota

## 6. Kesalahan Umum
- **Lupa `lazy: true`** → fetch blocking halaman (hydrate lama). Gunakan lazy untuk data non-kritis.
- **Fetch di client saja tapi mengaku SSR** → gunakan `useFetch` bukan `fetch` biasa agar jalan di server.
- **SSR tanpa handle error** → pakai `error` dari `useAsyncData` untuk fallback.

## 7. Benang Merah
Vue (Level 4) menyediakan komponen & reaktivitas. Nuxt menambahkan struktur meta-framework. Level ini jadi fondasi untuk Pinia Patterns (126), Web Performance (127), dan PWA (128) — semuanya berjalan di atas Nuxt.

## 8. Soal

**Soal 1:** Apa perbedaan utama antara SSR dan SSG di Nuxt? Kapan masing-masing digunakan?

**Soal 2:** Mengapa `useFetch` lebih baik daripada `fetch` biasa di Nuxt?

**Soal 3:** Sebutkan tiga folder inti dalam struktur Nuxt dan fungsinya.

---
<details>
<summary>Jawaban</summary>

**Jawaban 1:** SSR merender di server setiap ada request — cocok untuk data dinamis (e-commerce, dashboard). SSG membangun semua halaman saat build time — cocok untuk konten statis (blog, landing page). SSR butuh server runtime, SSG bisa di-host di CDN statis.

**Jawaban 2:** `useFetch` otomatis berjalan di server saat SSR (tidak perlu fetch ulang client), mendukung deduplication, caching, dan reaktivitas. `fetch` biasa hanya jalan di client dan tidak terintegrasi dengan Nuxt lifecycle.

**Jawaban 3:** `pages/` (routing otomatis), `components/` (komponen reusable, auto-import), `layouts/` (kerahan halaman seperti header/footer).
</details>
