# Tag `<picture>` dan `srcset`

## Penjelasan

`<picture>` adalah elemen HTML yang memungkinkan developer menyediakan beberapa versi gambar untuk satu slot, lalu browser memilih versi terbaik berdasarkan **lebar viewport**, **resolusi layar (DPR)**, atau **dukungan format**. `srcset` adalah atribut pada `<img>` atau `<source>` yang mendefinisikan daftar file gambar beserta ukuran intrinsiknya (dalam satuan `w` atau `x`). `sizes` adalah atribut yang memberi tahu browser berapa lebar gambar akan ditampilkan di layout (sehingga browser bisa memilih file yang tepat dari `srcset`).

```html
<picture>
  <source srcset="hero.avif" type="image/avif" />
  <source srcset="hero.webp" type="image/webp" />
  <img src="hero.jpg" srcset="hero-400.jpg 400w, hero-800.jpg 800w, hero-1200.jpg 1200w"
       sizes="(max-width: 600px) 100vw, (max-width: 1200px) 50vw, 33vw"
       alt="Hero banner" />
</picture>
```

## Fungsi

- **Mengirimkan gambar dengan resolusi sesuai ukuran viewport** — tidak perlu download file 1200px di layar 360px.
- **Mendukung format modern (WebP, AVIF)** dengan fallback ke JPEG/PNG jika browser belum mendukung.
- **Menghemat bandwidth** dan mempercepat load time, terutama di koneksi seluler.
- **Mempertahankan kualitas visual** karena gambar diskalakan sesuai kemampuan layar.

## Cara Pengimplementasian

```html
<!-- Srcset sederhana dengan width descriptor -->
<img src="foto.jpg"
     srcset="foto-400.jpg 400w, foto-800.jpg 800w, foto-1200.jpg 1200w"
     sizes="(max-width: 768px) 100vw, 50vw"
     alt="Foto pemandangan">

<!-- Picture dengan format fallback -->
<picture>
  <source srcset="hero.webp" type="image/webp">
  <source srcset="hero.jpg" type="image/jpeg">
  <img src="hero.jpg" alt="Hero banner">
</picture>

<!-- Picture dengan art direction (crop berbeda) -->
<picture>
  <source media="(min-width: 1024px)" srcset="hero-lanscape.jpg">
  <source media="(min-width: 640px)" srcset="hero-square.jpg">
  <img src="hero-portrait.jpg" alt="Hero responsive">
</picture>
```

## Analogi (tema RUMAH/BANGUNAN)

Bayangkan sebuah **rumah dengan beberapa pintu masuk dengan ukuran berbeda**. Pintu besar (1200w) untuk tamu rombongan, pintu sedang (800w) untuk keluarga, dan pintu kecil (400w) untuk kurir. Di setiap pintu ada sensor yang mengecek siapa yang datang — jika tamu rombongan datang, pintu besar terbuka; jika kurir, pintu kecil yang terbuka. `srcset` adalah kumpulan pintu itu, `sizes` adalah aturan siapa lewat pintu mana, dan `<picture>` adalah rumah yang juga punya pintu belakang (`<source>`) untuk format gambar khusus WebP jika pintu depan tidak muat.

## Dipakai Untuk

- Website dengan banyak gambar (e-commerce, portofolio, berita)
- Landing page dengan hero image full-width
- Galeri foto yang perlu kualitas tinggi di layar Retina
- Situasi mendukung format modern (WebP/AVIF) tanpa merusak kompatibilitas
- Art direction: crop berbeda untuk mobile vs desktop

## Kesalahan Umum

- **Tidak menyertakan `sizes`** — browser tidak bisa memilih `srcset` secara optimal tanpa `sizes`.
- **Menggunakan `x` dan `w` secara bersamaan dalam satu `srcset`** — campur aduk yang membingungkan browser.
- **Menulis urutan `srcset` tidak berurutan** — browser optimal jika urutan dari kecil ke besar.
- **Melupakan `<img>` fallback di dalam `<picture>`** — tanpanya gambar tidak akan tampil.
- **Path gambar salah** — file tidak ditemukan, gambar pecah.
- **Ukuran di `w` tidak sesuai dimensi asli gambar** — misal `800w` tapi gambar asli 400px.

## Koneksi dengan Materi Sebelumnya

- **Level 2 — Tag `<img>` dasar**: `srcset` dan `<picture>` adalah pengembangan dari `<img>`, menambahkan dimensi responsif.
- **Level 3 — CSS Media Queries**: `media` pada `<source>` dan `sizes` menggunakan sintaks yang mirip dengan media queries CSS.
- **Level 4 — Viewport & DPR**: `srcset` dengan `x` descriptor memanfaatkan pengetahuan tentang Device Pixel Ratio.
- **Level 5 — Performance**: Teknik ini adalah fondasi optimasi gambar yang akan dibahas di materi Optimasi Gambar (58).

## Soal Latihan

1. Tulis kode `<picture>` yang menyediakan gambar format WebP, dengan fallback JPEG, dan di dalamnya `srcset` tiga ukuran (400w, 800w, 1200w).

<details><summary>Jawaban</summary>

```html
<picture>
  <source srcset="gambar.webp" type="image/webp" />
  <img src="gambar.jpg"
       srcset="gambar-400.jpg 400w, gambar-800.jpg 800w, gambar-1200.jpg 1200w"
       sizes="(max-width: 600px) 100vw, 50vw"
       alt="Gambar responsif" />
</picture>
```

</details>

2. Apa perbedaan antara `w` descriptor dan `x` descriptor di `srcset`?

<details><summary>Jawaban</summary>

- **`w`** (width descriptor): mendefinisikan lebar intrinsik gambar dalam piksel. Digunakan bersama atribut `sizes`. Browser memilih gambar berdasarkan lebar viewport dan `sizes`.
- **`x`** (device-pixel-ratio descriptor): mendefinisikan faktor DPR (1x, 2x, 3x). Browser memilih gambar berdasarkan resolusi layar (Retina vs non-Retina), tanpa perlu `sizes`.

</details>
