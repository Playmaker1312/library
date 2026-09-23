# Service Worker

## Penjelasan
Service Worker adalah script JavaScript yang berjalan di latar belakang (background), terpisah dari halaman web. Ia bertindak sebagai proxy antara browser, jaringan, dan cache, memungkinkan fitur offline, notifikasi push, dan sinkronisasi latar belakang.

## Fungsi
- Menyimpan file ke cache saat pertama kali dikunjungi (pre-cache)
- Menyajikan file dari cache saat offline (cache-first strategy)
- Mempercepat loading dengan mengambil dari cache daripada jaringan
- Mendukung notifikasi push dan background sync

## Cara Pengimplementasian

```javascript
// register sw.js di halaman utama
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js')
    .then(() => console.log('Service Worker terdaftar'))
    .catch(err => console.error('Gagal registrasi:', err));
}
```

```javascript
// sw.js — Install event: cache file inti
const CACHE_NAME = 'my-app-v1';
const ASSETS = [
  '/',
  '/index.html',
  '/style.css',
  '/app.js',
  '/offline.html'
];

self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => cache.addAll(ASSETS))
      .then(() => self.skipWaiting())
  );
});
```

```javascript
// sw.js — Fetch event: cache-first (ambil dari cache, fallback ke jaringan)
self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request)
      .then(cached => cached || fetch(event.request))
      .catch(() => caches.match('/offline.html'))
  );
});
```

## Analogi (tema RUMAH/BANGUNAN)
Service Worker adalah **satpam atau resepsionis yang berdiri di pintu masuk rumah**. Saat tamu datang minta sesuatu (fetch), satpam cek dulu di gudang (cache). Jika barang ada di gudang, langsung diberikan tanpa perlu ke luar (offline). Jika tidak ada, barulah dia pergi ke toko (jaringan). Satpam juga sibuk menata gudang saat rumah sepi (install event) dan punya daftar barang penting yang harus selalu tersedia di gudang (ASSETS).

## Dipakai Untuk
- Website yang bisa diakses offline
- Aplikasi web yang butuh loading cepat dan hemat data
- Progressive Web App (PWA) yang memenuhi syarat installable

## Kesalahan Umum
- Service Worker hanya berjalan di HTTPS atau localhost
- Lupa memanggil `event.waitUntil()` pada install event
- Cache tidak diupdate setelah konten berubah (perlu versi baru)
- Tidak menyediakan halaman offline fallback
- Fetch event tidak menangani error sehingga halaman putih saat offline

## Koneksi dengan Materi Sebelumnya
Service Worker berkaitan erat dengan JavaScript (DOM, event listener) dan Web App Manifest — manifest membuat website bisa diinstal, service worker membuatnya bisa offline. Keduanya adalah pilar utama PWA.

## Soal Latihan
<details><summary>Jawaban</summary>

1. Apa perbedaan cache-first dan network-first?
   **Jawaban:** Cache-first mengambil dari cache dulu (cepat, offline), fallback ke jaringan. Network-first mencoba jaringan dulu (data terbaru), fallback ke cache. Cache-first cocok untuk aset statis.

2. Mengapa Service Worker butuh HTTPS?
   **Jawaban:** Karena Service Worker memiliki kekuatan tinggi (mencegat request, cache data), HTTPS mencegah serangan man-in-the-middle.

3. Apa fungsi `self.skipWaiting()` pada install event?
   **Jawaban:** Memberitahu browser untuk segera mengaktifkan Service Worker baru tanpa menunggu halaman ditutup.

</details>
