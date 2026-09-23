# Tag `<figure>` dan `<figcaption>`

## Penjelasan

Tag `<figure>` digunakan untuk mengelompokkan konten mandiri seperti gambar, ilustrasi, diagram, cuplikan kode, atau media lainnya. Bersama `<figcaption>` yang berisi keterangan/teks penjelas, keduanya membentuk satu kesatuan konten yang bisa dipindah tanpa mengganggu alur dokumen.

## Fungsi

- Membungkus elemen media (gambar, video, kode) menjadi satu kesatuan.
- Menambahkan keterangan (caption) yang terikat secara semantik dengan media.
- Memudahkan penataan halaman — elemen `<figure>` bisa dipindah tanpa merusak struktur.
- Meningkatkan semantik dan aksesibilitas dokumen HTML.

## Cara Pengimplementasian

```html
<!-- Figure dengan gambar dan caption -->
<figure>
  <img src="rumah-modern.jpg" alt="Rumah bergaya modern minimalis">
  <figcaption>Rumah modern dengan taman depan dan atap datar.</figcaption>
</figure>

<!-- Figure dengan beberapa gambar (buffer/foto galeri) -->
<figure>
  <img src="tampak-depan.jpg" alt="Tampak depan rumah">
  <img src="tampak-samping.jpg" alt="Tampak samping rumah">
  <img src="tampak-belakang.jpg" alt="Tampak belakang rumah">
  <figcaption>Dokumentasi rumah dari berbagai sisi.</figcaption>
</figure>

<!-- Figure dengan cuplikan kode -->
<figure>
  <pre>
    &lt;h1&gt;Selamat Datang&lt;/h1&gt;
  </pre>
  <figcaption>Kode HTML untuk judul halaman.</figcaption>
</figure>
```

## Analogi (tema RUMAH/BANGUNAN)

`<figure>` adalah seperti **bingkai pigura** yang berisi foto rumah. Di bagian bawah pigura, ada plakat kecil (`<figcaption>`) yang menjelaskan foto tersebut, misalnya "Rumah Tampak Depan — 2024". Pigura dan plakat adalah satu paket; jika kamu pindahkan pigura ke ruangan lain, keterangannya ikut pindah.

## Dipakai Untuk

- Foto produk dengan keterangan
- Infografik dan diagram
- Galeri gambar mini yang bisa dipindah posisinya
- Cuplikan kode dengan label bahasa pemrograman
- Ilustrasi buku atau artikel teknis

## Kesalahan Umum

- Menggunakan `<figure>` untuk konten yang tidak mandiri (membutuhkan konteks di luar figure).
- Meletakkan `<figcaption>` di luar `<figure>`.
- Menggunakan `<figure>` hanya sebagai pengganti `<div>` untuk styling saja.
- Lupa bahwa `<figcaption>` bisa diletakkan di awal **atau** akhir `<figure>` (tidak harus setelah gambar).
- Menempatkan banyak gambar tidak terkait dalam satu `<figure>`.

## Koneksi dengan Materi Sebelumnya

- **Tag `<img>`**: `<figure>` membungkus `<img>` untuk memberikan konteks semantik.
- **Tag `<p>`**: Sebelumnya paragraf digunakan untuk mendampingi gambar; sekarang `<figcaption>` lebih semantik.
- **Semantic HTML**: `<figure>` adalah bagian dari elemen semantik HTML5 seperti `<article>`, `<section>`, `<nav>`.

## Soal Latihan

1. Buat struktur `<figure>` yang menampilkan gambar `denah.jpg` dengan caption "Denah rumah type 45".
2. Bolehkah `<figcaption>` diletakkan di atas gambar dalam `<figure>`?
3. Sebutkan perbedaan utama antara `<figure>` dan `<div>` biasa.

<details><summary>Jawaban</summary>

1. 
```html
<figure>
  <img src="denah.jpg" alt="Denah rumah type 45">
  <figcaption>Denah rumah type 45</figcaption>
</figure>
```

2. Boleh. Tag `<figcaption>` bisa diletakkan sebagai elemen pertama atau terakhir di dalam `<figure>`.

3. `<figure>` bersifat **semantik** — menandakan bahwa konten di dalamnya adalah unit mandiri yang bisa dipindah tanpa mengubah alur dokumen. `<div>` tidak memiliki makna semantik dan hanya digunakan untuk pengelompokan/ styling biasa.

</details>
