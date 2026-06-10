# 127: Web Performance — Core Web Vitals

## 1. Penjelasan
Core Web Vitals adalah metrik Google yang mengukur pengalaman pengguna di web: LCP (Largest Contentful Paint — loading), FID (First Input Delay — interaktivitas), CLS (Cumulative Layout Shift — stabilitas visual). Lighthouse mengukur dan memberi skor. Optimasi meliputi lazy loading, code splitting, image optimization, caching.

## 2. Fungsi
- LCP < 2.5s — konten utama cepat muncul
- FID < 100ms — responsif terhadap input pertama
- CLS < 0.1 — layout tidak bergeser saat dimuat
- Meningkatkan skor Lighthouse (SEO, UX)
- Lazy loading gambar & komponen agar tidak blocking
- Code splitting: muat hanya kode yang diperlukan

## 3. Code

```ts
// nuxt.config.ts — optimasi gambar & performa
export default defineNuxtConfig({
  image: {
    format: ['webp'], // otomatis konversi ke WebP
    screens: { xs: 320, sm: 640, md: 768, lg: 1024 }
  },
  app: {
    head: {
      link: [{ rel: 'preload', href: '/fonts/inter.woff2', as: 'font' }]
    }
  },
  vite: {
    build: {
      rollupOptions: {
        output: {
          manualChunks: { vendor: ['vue', 'pinia'] }
        }
      }
    }
  }
})
```

```vue
<!-- lazy load komponen berat -->
<script setup>
const BookDetail = defineAsyncComponent(() =>
  import('~/components/BookDetail.vue')
)
</script>

<template>
  <div>
    <BookCard v-for="book in books" :key="book.id" :book="book" />
    <BookDetail v-if="showDetail" />
  </div>
</template>
```

```vue
<!-- gambar dengan lazy & optimasi Nuxt Image -->
<template>
  <NuxtImg
    format="webp"
    :src="book.cover"
    :alt="book.title"
    width="300"
    height="450"
    loading="lazy"
    placeholder
  />
</template>
```

## 4. Analogi Rumah

| Konsep Web Vitals | Analogi Rumah |
|-------------------|---------------|
| LCP | Kecepatan pintu terbuka saat tamu datang |
| FID | Waktu tunggu lift setelah tombol ditekan |
| CLS | Meja bergeser saat disentuh — tidak stabil |
| Lazy loading | Lampu hanya menyala di ruangan yang dipakai |
| Code splitting | Renovasi ruangan per ruangan, bukan seluruh rumah |
| Image optimization | Foto diperkecil sebelum dipajang di dinding |

## 5. Use Case
- Situs e-commerce: LCP harus cepat untuk gambar produk
- Blog: CLS penting agar artikel tidak "lompat" saat font/iklan dimuat
- Aplikasi perpustakaan: lazy load BookDetail + optimasi sampul buku dengan WebP
- Dashboard admin: code splitting agar halaman statistik berat terpisah

## 6. Kesalahan Umum
- **Hanya fokus LCP, lupa CLS** → selalu set `width` `height` pada gambar agar layout stabil.
- **Lazy loading berlebihan** → komponen di atas fold sebaiknya tidak lazy (delay persepsi).
- **Tidak preload critical asset** → font dan hero image perlu preload agar tidak blocking render.

## 7. Benang Merah
Dari Pinia (126) yang mengelola state, performa memastikan state & komponen dimuat efisien. PWA (128) memanfaatkan caching untuk performa offline. Accessibility (129) juga terpengaruh — performa lambat menyulitkan pengguna screen reader.

## 8. Soal

**Soal 1:** Sebutkan tiga metrik Core Web Vitals dan apa yang diukur masing-masing.

**Soal 2:** Bagaimana cara mengurangi CLS pada halaman yang memuat banyak gambar?

**Soal 3:** Apa manfaat code splitting dan bagaimana implementasinya di Nuxt/Vue?

---
<details>
<summary>Jawaban</summary>

**Jawaban 1:** LCP (waktu muat konten terbesar), FID (waktu respons terhadap input pertama), CLS (pergeseran layout kumulatif).

**Jawaban 2:** Tetapkan atribut `width` dan `height` pada tag `<img>` atau `<NuxtImg>` agar browser mengalokasikan ruang sebelum gambar dimuat. Gunakan placeholder atau skeleton.

**Jawaban 3:** Code splitting memecah bundle menjadi chunk yang dimuat sesuai kebutuhan. Di Vue: `defineAsyncComponent(() => import('...'))`. Di Nuxt: rute otomatis di-split per halaman. Manfaat: initial load lebih kecil, halaman lebih cepat.
</details>
