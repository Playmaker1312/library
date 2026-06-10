# 128: Progressive Web App (PWA)

## 1. Penjelasan
PWA adalah aplikasi web yang bisa diinstal di perangkat seperti aplikasi native. Komponen utama: Service Worker (proxy jaringan untuk caching & offline), Web App Manifest (ikon, nama, tema), dan Workbox (library untuk strategi caching). PWA mendukung offline-first, push notification, dan install prompt.

## 2. Fungsi
- Bekerja offline atau koneksi buruk
- Installable — pengguna bisa tambahkan ke home screen
- Load instan berkat cache strategi (Cache First, Network First, Stale-While-Revalidate)
- Push notification untuk re-engagement
- Meningkatkan retensi & performa

## 3. Code

```ts
// nuxt.config.ts — PWA module
export default defineNuxtConfig({
  modules: ['@vite-pwa/nuxt'],
  pwa: {
    registerType: 'autoUpdate',
    manifest: {
      name: 'Perpustakaan Online',
      short_name: 'Perpus',
      description: 'Aplikasi perpustakaan digital',
      theme_color: '#1a73e8',
      icons: [
        { src: '/icon-192.png', sizes: '192x192', type: 'image/png' },
        { src: '/icon-512.png', sizes: '512x512', type: 'image/png' }
      ]
    },
    workbox: {
      globPatterns: ['**/*.{js,css,html,png,svg,ico,woff2}']
    },
    registerWebManifestInRouteRules: true
  }
})
```

```ts
// service worker strategi — sw.ts (otomatis oleh @vite-pwa/nuxt)
// Konfigurasi runtime caching untuk API
workbox.routing.registerRoute(
  ({ url }) => url.pathname.startsWith('/api/'),
  new workbox.strategies.StaleWhileRevalidate({
    cacheName: 'api-cache',
    plugins: [
      new workbox.expiration.ExpirationPlugin({ maxEntries: 50, maxAgeSeconds: 86400 })
    ]
  })
)

workbox.routing.registerRoute(
  ({ request }) => request.destination === 'image',
  new workbox.strategies.CacheFirst({
    cacheName: 'image-cache',
    plugins: [
      new workbox.expiration.ExpirationPlugin({ maxEntries: 100, maxAgeSeconds: 604800 })
    ]
  })
)
```

```vue
<!-- Install prompt komponen -->
<script setup>
const deferredPrompt = ref(null)
const showInstall = ref(false)

onMounted(() => {
  window.addEventListener('beforeinstallprompt', (e) => {
    e.preventDefault()
    deferredPrompt.value = e
    showInstall.value = true
  })
})

async function installApp() {
  deferredPrompt.value?.prompt()
  const result = await deferredPrompt.value?.userChoice
  if (result?.outcome === 'accepted') showInstall.value = false
}
</script>

<template>
  <div v-if="showInstall" class="install-banner">
    <p>Install aplikasi Perpustakaan</p>
    <button @click="installApp">Install</button>
  </div>
</template>
```

## 4. Analogi Rumah

| Konsep PWA | Analogi Rumah |
|------------|---------------|
| Service Worker | Genset rumah — saat listrik padam (offline), genset nyala otomatis |
| Cache | Stok makanan di dapur — tidak perlu ke pasar tiap kali lapar |
| Manifest | Papan nama & cat rumah — identitas rumah di home screen |
| Install prompt | Tukang kunci yang menawarkan kunci cadangan |
| Offline-first | Rumah yang tetap nyaman tanpa listrik (offline) |
| Push notification | Bel pintu yang bisa dipencet dari luar rumah |

## 5. Use Case
- E-commerce: pengguna bisa lihat produk offline
- Berita: baca artikel yang sudah dikunjungi tanpa internet
- Perpustakaan: cari buku dari cache saat sinyal jelek
- Dashboard: akses data terakhir tanpa koneksi

## 6. Kesalahan Umum
- **Cache terlalu banyak** → storage browser terbatas. Batasi entri dengan `ExpirationPlugin`.
- **Tidak handle update service worker** → pengguna pakai versi lama. Gunakan `registerType: 'autoUpdate'` atau notifikasi update.
- **Lupa test offline** → selalu uji dengan DevTools > Network > Offline.

## 7. Benang Merah
Performa (127) adalah prasyarat PWA — halaman cepat membuat pengalaman offline terasa mulus. Aksesibilitas (129) juga penting di PWA agar semua pengguna bisa menginstal & pakai aplikasi. PWA juga bisa menjadi container untuk micro-frontend (131).

## 8. Soal

**Soal 1:** Apa peran Service Worker dalam PWA?

**Soal 2:** Jelaskan perbedaan strategi caching Cache First vs Network First. Kapan masing-masing digunakan?

**Soal 3:** Bagaimana cara menampilkan tombol install PWA ke pengguna?

---
<details>
<summary>Jawaban</summary>

**Jawaban 1:** Service Worker adalah script yang berjalan di background browser, bertindak sebagai proxy jaringan. Ia meng-intercept request, menyajikan cache untuk offline, dan memungkinkan push notification.

**Jawaban 2:** Cache First — cek cache dulu, fallback ke jaringan (aset statis seperti gambar, CSS). Network First — coba jaringan dulu, fallback ke cache (data API, konten dinamis). Pilih Network First untuk data yang sering berubah.

**Jawaban 3:** Tangkap event `beforeinstallprompt`, simpan deferred prompt, lalu panggil `.prompt()` saat tombol diklik. Tampilkan tombol hanya jika event tersebut ada (browser mendukung).
</details>
