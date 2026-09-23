# Critical Rendering Path

## Penjelasan
Critical Rendering Path (CRP) adalah urutan langkah yang dilalui browser untuk mengubah HTML, CSS, dan JavaScript menjadi piksel di layar. Ini mencakup parsing HTML, pembuatan DOM, pembuatan CSSOM, eksekusi JavaScript, pembuatan Render Tree, Layout, dan Paint. Jika ada sumber daya yang memblokir proses ini, halaman akan tertunda tampilnya.

## Fungsi
- Memahami titik-titik yang memperlambat render halaman
- Mengoptimalkan waktu muat agar pengguna bisa melihat konten lebih cepat
- Mengurangi waktu *First Contentful Paint* (FCP) dan *Largest Contentful Paint* (LCP)

## Cara Pengimplementasian
```html
<!-- ❌ BURUK: script di head memblokir parsing -->
<head>
  <script src="besar.js"></script>
  <link rel="stylesheet" href="gaya.css">
</head>

<!-- ✅ BAIK: pakai defer agar tidak blokir -->
<head>
  <link rel="stylesheet" href="gaya.css">
  <script src="besar.js" defer></script>
</head>

<!-- ✅ BAIK: preload aset penting -->
<head>
  <link rel="preload" href="hero.webp" as="image">
  <link rel="preconnect" href="https://api.example.com">
</head>
```

## Analogi (tema RUMAH/BANGUNAN)
Membangun rumah punya alur kerja: **fundasi → rangka → dinding → atap → finishing**. CRP adalah alur kerja itu. Jika tukang (browser) harus berhenti lama karena bahan bangunan (JavaScript) belum siap di tengah proses, seluruh proyek tertunda. Menaruh script di `<head>` tanpa `defer` seperti menyuruh tukang berhenti di tengah memasang bata karena harus menunggu lemari datang — padahal lemari baru dipasang di akhir.

## Dipakai Untuk
- Halaman web dengan konten di atas lipatan (*above the fold*) yang ingin tampil secepat mungkin
- Situs dengan banyak CSS dan JavaScript yang memblokir render awal
- Optimalisasi Largest Contentful Paint (LCP) dan First Contentful Paint (FCP)

## Kesalahan Umum
- Menaruh tag `<script>` di `<head>` tanpa atribut `defer` atau `async`, menyebabkan blokir parsing DOM
- Tidak memisahkan CSS kritis (inline) dengan CSS non-kritis (lazy load)
- Menganggap semua aset sama pentingnya, padahal beberapa aset menghalangi render pertama
- Tidak menggunakan `preload` untuk font atau hero image
- Membiarkan CSS eksternal besar memblokir render tanpa strategi split

## Koneksi dengan Materi Sebelumnya
- **Level 2 - CSS**: CSSOM dibangun dari CSS; CSS adalah *render-blocking resource* yang harus dioptimasi
- **Level 4 - JavaScript**: Posisi dan atribut script (defer/async) sangat memengaruhi CRP
- **Resource Hints (materi 65)**: Preload dan preconnect adalah alat langsung untuk mempercepat CRP
- **async vs defer (materi 64)**: Dua strategi mengelola script agar tidak memblokir CRP

## Soal Latihan
1. Apa yang terjadi jika `<script>` tanpa atribut ditaruh di dalam `<head>`?
2. Sebutkan urutan langkah-langkah dalam Critical Rendering Path!
3. Mengapa CSS disebut *render-blocking resource*?
4. Bagaimana cara memuat font tanpa memblokir render?

<details><summary>Jawaban</summary>
1. Browser akan menghentikan parsing HTML, mengambil dan mengeksekusi script, baru melanjutkan parsing. Ini memperlambat render halaman.
2. DOM → CSSOM → Render Tree → Layout → Paint.
3. Karena browser tidak akan merender halaman sampai CSSOM selesai dibangun, untuk menghindari *Flash of Unstyled Content* (FOUC).
4. Gunakan `<link rel="preload" href="font.woff2" as="font" crossorigin>` agar font diambil lebih awal tanpa memblokir render.
</details>
