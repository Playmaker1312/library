# 133: Animation & UX Engineering

## 1. Penjelasan
Animation dalam frontend bukan hanya estetika — ia meningkatkan UX dengan memberikan feedback visual, guide perhatian, dan membuat transisi terasa alami. Tools: CSS transitions/animations, GSAP (GreenSock) untuk animasi kompleks, dan Intersection Observer untuk scroll-triggered animation. Loading skeleton dan page transition meningkatkan persepsi performa.

## 2. Fungsi
- Feedback visual: tombol ditekan → efek ripple, form error → shake
- Page transition: navigasi terasa mulus (tidak abrupt)
- Loading state: skeleton screen mengurangi frustrasi
- Guide perhatian: elemen baru muncul dengan fade-in
- Narasi storytelling: scroll-triggered animation untuk landing page
- Meningkatkan persepsi performa (perceived performance)

## 3. Code

```vue
<!-- Page Transition Nuxt -->
<template>
  <NuxtPage />
</template>

<style>
.page-enter-active, .page-leave-active {
  transition: all 0.3s ease;
}
.page-enter-from { opacity: 0; transform: translateY(20px); }
.page-leave-to { opacity: 0; transform: translateY(-20px); }
</style>
```

```vue
<!-- Loading Skeleton -->
<template>
  <div class="skeleton-list">
    <div v-for="n in 5" :key="n" class="skeleton-item">
      <div class="skeleton-image pulse" />
      <div class="skeleton-text pulse" style="width: 80%" />
      <div class="skeleton-text pulse" style="width: 60%" />
    </div>
  </div>
</template>

<style scoped>
.pulse {
  background: linear-gradient(90deg, #eee 25%, #ddd 50%, #eee 75%);
  background-size: 200% 100%;
  animation: pulse 1.5s ease-in-out infinite;
}
@keyframes pulse {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}
.skeleton-item > div {
  height: 16px;
  margin-bottom: 8px;
  border-radius: 4px;
}
.skeleton-image { height: 200px; }
</style>
```

```ts
// GSAP Animation — List masuk dengan stagger
import { gsap } from 'gsap'

onMounted(() => {
  gsap.from('.book-card', {
    opacity: 0,
    y: 30,
    stagger: 0.05, // berurutan tiap 50ms
    duration: 0.5,
    ease: 'power2.out'
  })
})
```

```vue
<!-- Intersection Observer — Scroll Trigger -->
<script setup>
const sectionRef = ref(null)
const isVisible = ref(false)

onMounted(() => {
  const observer = new IntersectionObserver(([entry]) => {
    if (entry.isIntersecting) {
      isVisible.value = true
      observer.disconnect()
    }
  }, { threshold: 0.2 })
  observer.observe(sectionRef.value)
})
</script>

<template>
  <div ref="sectionRef" :class="{ visible: isVisible }">
    <h2 class="fade-in-up">Koleksi Buku Terbaru</h2>
  </div>
</template>
```

## 4. Analogi Rumah

| Konsep Animasi | Analogi Rumah |
|----------------|---------------|
| CSS transition | Pintu geser otomatis — mulus, konsisten |
| GSAP | Interior designer profesional — tata ruang dengan gerakan presisi |
| Loading skeleton | Kerangka rumah saat dibangun — pengunjung lihat struktur, bukan kosong |
| Page transition | Lampu menyala gradual saat masuk ruangan |
| Scroll-triggered | Lampu sensor — menyala saat orang lewat |
| Stagger animation | Furniture masuk satu per satu, bukan tiba-tiba penuh |
| Perceived performance | Meski renovasi belum selesai, ruang tamu sudah rapi (pengguna merasa cepat) |

## 5. Use Case
- Daftar buku muncul dengan stagger animation
- Page transition antara halaman buku
- Skeleton saat data API masih loading
- Scroll-triggered section (tentang kami, statistik)
- Tombol pinjam dengan micro-interaction (scale, color change)
- Toast notifikasi muncul dari atas, fade out

## 6. Kesalahan Umum
- **Animasi terlalu lambat** → durasi > 500ms terasa lelet. Batasi 200-400ms.
- **Tidak menghormati reduced motion** → gunakan `prefers-reduced-motion` media query untuk nonaktifkan animasi.
- **Animasi blocking interaksi** → properti CSS seperti `height` animasi menyebabkan layout thrashing. Animasikan `transform` dan `opacity` saja.
- **Over-animasi** → setiap elemen bergerak = mabuk laut. Gunakan animasi secukupnya, hanya untuk memberikan makna.

## 7. Benang Merah
PENUTUP Jalur Frontend Level 6. Semua topik sebelumnya bersatu di sini: animasi meningkatkan UX dari Nuxt (125), state change (126) dianimasikan, performa (127) diperhalus dengan skeleton, aksesibilitas (129) dihormati dengan `prefers-reduced-motion`. Animation adalah sentuhan akhir yang membuat aplikasi terasa hidup dan profesional.

## 8. Soal

**Soal 1:** Mengapa animasi yang baik meningkatkan perceived performance?

**Soal 2:** Bagaimana cara menghormati preferensi pengguna yang tidak suka animasi (reduced motion)?

**Soal 3:** Sebutkan 3 properti CSS yang aman dianimasikan (tidak menyebabkan layout thrashing).

---
<details>
<summary>Jawaban</summary>

**Jawaban 1:** Animasi mengalihkan perhatian pengguna saat konten dimuat (skeleton), membuat waktu tunggu terasa lebih singkat. Transisi halus juga memberi kesan aplikasi responsif walau sedang memproses.

**Jawaban 2:** Gunakan media query `@media (prefers-reduced-motion: reduce)` untuk menonaktifkan semua animasi atau ganti dengan transisi instant. Di GSAP: `gsap.ticker.lagSmoothing(0)` saat reduced motion.

**Jawaban 3:** `transform` (translate, scale, rotate), `opacity`, dan `clip-path`. Properti ini digaransi oleh compositor thread, tidak trigger layout atau paint ulang.
</details>
