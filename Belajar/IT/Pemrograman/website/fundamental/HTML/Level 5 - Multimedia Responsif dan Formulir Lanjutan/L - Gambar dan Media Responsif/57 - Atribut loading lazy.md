# Atribut `loading="lazy"`

## Penjelasan

`loading="lazy"` adalah atribut HTML pada tag `<img>` dan `<iframe>` yang menunda pemuatan gambar hingga elemen tersebut **mendekati viewport** (biasanya dalam jarak tertentu, misalnya ~1250px). Sebaliknya, `loading="eager"` (nilai default) memuat gambar segera saat HTML diparsa, tanpa menunggu posisi scroll. `decoding="async"` adalah atribut yang memberi tahu browser bahwa decoding gambar boleh dilakukan secara asinkron agar tidak memblokir rendering konten lain.

```html
<!-- Lazy load: gambar dimuat saat mendekati viewport -->
<img src="foto.jpg" loading="lazy" alt="Foto">

<!-- Eager: gambar langsung dimuat (default) -->
<img src="hero.jpg" loading="eager" alt="Hero">

<!-- Decoding async: decoding tidak memblokir rendering -->
<img src="foto.jpg" loading="lazy" decoding="async" alt="Foto">
```

## Fungsi

- **Menghemat bandwidth** dengan tidak memuat gambar di bawah fold (belum terlihat) sampai user scroll ke area tersebut.
- **Mempercepat page load awal (LCP / Largest Contentful Paint)** karena browser tidak perlu mendownload gambar yang tidak terlihat duluan.
- **Mengurangi jumlah request jaringan** secara keseluruhan pada halaman panjang (infinite scroll, galeri, artikel panjang).
- `decoding="async"` memungkinkan browser merender teks dan layout lebih dulu sambil mendecode gambar di latar belakang.

## Cara Pengimplementasian

```html
<!-- Gambar biasa dengan lazy load -->
<img src="artikel-gambar1.jpg" loading="lazy" alt="Ilustrasi artikel">

<!-- Hero image: eager agar LCP cepat -->
<img src="hero-banner.jpg" loading="eager" fetchpriority="high" alt="Hero banner">

<!-- Kombinasi lazy + decoding async + srcset -->
<img src="foto.jpg"
     srcset="foto-400.jpg 400w, foto-800.jpg 800w"
     sizes="100vw"
     loading="lazy"
     decoding="async"
     alt="Foto responsif lazy">

<!-- iframe juga bisa lazy -->
<iframe src="https://example.com/map" loading="lazy"></iframe>
```

## Analogi (tema RUMAH/BANGUNAN)

Bayangkan **sebuah rumah besar dengan 20 kamar tidur**. Saat ada tamu datang (user membuka halaman), tidak semua kamar langsung disiapkan. Hanya kamar tamu di lantai 1 yang langsung dibersihkan (`eager` — hero image). Kamar-kamar lain hanya disiapkan ketika tamu berjalan mendekati pintu kamar itu (`lazy`). `decoding="async"` seperti menyuruh asisten menyiapkan kamar di dapur (latar belakang) sementara tuan rumah menyambut tamu di ruang tamu.

## Dipakai Untuk

- Halaman dengan banyak gambar di bawah fold (artikel panjang, blog, berita)
- Galeri gambar atau grid produk di e-commerce
- Infinite scroll feed (sosial media, marketplace)
- Halaman dengan banyak embed (iframe YouTube, Google Maps)
- **Tidak disarankan** untuk: hero image, gambar di atas fold, logo header

## Kesalahan Umum

- **Menerapkan `loading="lazy"` pada gambar hero** — ini malah memperlambat LCP karena gambar penting ditunda.
- **Mengira `loading="lazy"` akan menghemat bandwidth sepenuhnya** — tetap ada overhead karena browser perlu memeriksa posisi scroll berkala.
- **Tidak menyediakan placeholder** — saat gambar belum dimuat, area kosong menyebabkan layout shift (CLS). Gunakan `width` dan `height` atau aspect-ratio CSS.
- **Lupa atribut `width` dan `height`** — menyebabkan Cumulative Layout Shift saat gambar akhirnya dimuat.
- **Menaruh `loading="lazy"` di `<img>` di dalam `<picture>`** — tetap valid, tapi pastikan diletakkan di tag `<img>`, bukan di `<source>`.
- **Menggunakan `loading="lazy"` pada gambar yang 100% terlihat** — tidak ada manfaat, malah tambah kompleksitas.

## Koneksi dengan Materi Sebelumnya

- **Level 3 — CSS Aspect Ratio**: Memberi `aspect-ratio` di CSS mencegah layout shift saat gambar lazy dimuat.
- **Level 5 — `<picture>` & `srcset` (56)**: `loading="lazy"` dikombinasikan dengan `srcset` untuk gambar responsif yang juga ditunda muatnya.
- **Level 5 — Optimasi Gambar (58)**: Lazy loading adalah strategi yang melengkapi optimasi ukuran file — gambar kecil + lazy = performa maksimal.
- **Performance Metrics (LCP, CLS)**: Memahami Core Web Vitals penting untuk menentukan kapan pakai `eager` vs `lazy`.

## Soal Latihan

1. Tulis kode `<img>` untuk gambar artikel di bawah fold dengan `loading="lazy"`, `decoding="async"`, dan `srcset` 3 ukuran.

<details><summary>Jawaban</summary>

```html
<img src="artikel.jpg"
     srcset="artikel-400.jpg 400w, artikel-800.jpg 800w, artikel-1200.jpg 1200w"
     sizes="(max-width: 768px) 100vw, 50vw"
     loading="lazy"
     decoding="async"
     alt="Gambar artikel">
```

</details>

2. Kapan sebaiknya menggunakan `loading="eager"` secara eksplisit?

<details><summary>Jawaban</summary>

Saat gambar tersebut berada di **above the fold** dan merupakan elemen penting untuk LCP, seperti hero banner, logo utama, atau gambar pertama yang langsung terlihat tanpa scroll. Ini memastikan browser memuat gambar tersebut dengan prioritas tertinggi tanpa ditunda.

</details>
