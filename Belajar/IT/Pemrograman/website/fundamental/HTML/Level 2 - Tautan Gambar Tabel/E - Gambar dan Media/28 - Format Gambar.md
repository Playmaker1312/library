# Format Gambar

## Penjelasan

Format gambar digital menentukan cara data visual dikompresi dan disimpan. Setiap format memiliki kelebihan dan kekurangan dalam hal kualitas, ukuran file, dukungan transparansi, animasi, dan skalabilitas. Lima format yang paling umum di web adalah JPEG, PNG, SVG, WebP, dan GIF.

## Fungsi

- Menyimpan dan menampilkan grafis di halaman web.
- Menyeimbangkan kualitas visual dengan kecepatan muat halaman.
- Mendukung kebutuhan spesifik seperti transparansi, animasi, atau vektor.

## Cara Pengimplementasian

```html
<!-- JPEG: foto dengan gradasi warna kompleks -->
<img src="foto-rumah.jpg" alt="Foto rumah dari luar">

<!-- PNG: grafis dengan transparansi -->
<img src="logo-transparan.png" alt="Logo perusahaan">

<!-- SVG: ikon vektor yang bisa diperbesar -->
<img src="icon-rumah.svg" alt="Ikon rumah">

<!-- WebP: modern, kualitas tinggi ukuran kecil -->
<img src="banner.webp" alt="Banner promo rumah">

<!-- GIF: animasi sederhana -->
<img src="loading.gif" alt="Animasi loading">
```

## Tabel Perbandingan

| Format | Kompresi | Transparansi | Animasi | Skalabilitas | Cocok Untuk |
|--------|----------|--------------|---------|--------------|-------------|
| JPEG   | Lossy    | Tidak        | Tidak   | Tidak        | Foto realistik, gambar kompleks |
| PNG    | Lossless | Ya           | Tidak   | Tidak        | Logo, ikon, tangkapan layar |
| SVG    | Vektor   | Ya           | Ya (CSS) | Ya          | Ikon, ilustrasi, diagram |
| WebP   | Lossy/Lossless | Ya    | Ya (opsional) | Tidak | Format serbaguna modern |
| GIF    | Lossless (256 warna) | Ya | Ya | Tidak | Animasi pendek, stiker |

## Analogi (tema RUMAH/BANGUNAN)

Format gambar seperti **bahan bangunan** untuk rumah:

- **JPEG** = **batu bata** — kuat, murah, cocok untuk dinding besar (foto), tapi tidak bisa tembus pandang (tanpa transparansi).
- **PNG** = **kaca jendela** — bisa transparan, detail terjaga, tapi lebih mahal dan berat.
- **SVG** = **cetak biru (blueprint)** — bisa diperbesar tanpa pecah, ringan, ideal untuk denah.
- **WebP** = **bata ringan modern** — semua kelebihan batu bata dan kaca, lebih efisien.
- **GIF** = **lampu hias kedip-kedip** — menarik perhatian, tapi terbatas warna dan kualitas.

## Dipakai Untuk

- **JPEG**: Foto galeri rumah, gambar hero, banner website.
- **PNG**: Logo dengan latar transparan, ikon, screenshot aplikasi.
- **SVG**: Ikon navigasi, ilustrasi vektor, logo responsif.
- **WebP**: Banner modern, foto produk (ukuran file lebih kecil 25-34% dari JPEG).
- **GIF**: Animasi sederhana (loading spinner, stiker, meme).

## Kesalahan Umum

- Menggunakan JPEG untuk logo dengan latar transparan (akan muncul kotak putih).
- Menggunakan GIF untuk foto (kualitas buruk dan ukuran file besar).
- Tidak mengompres JPEG/PNG sebelum diunggah.
- Menggunakan PNG untuk foto besar (ukuran file 2-5× lebih besar dari JPEG).
- Tidak menggunakan WebP padahal browser mendukung.

## Koneksi dengan Materi Sebelumnya

- **Tag `<img>`**: Semua format di atas bisa ditampilkan dengan tag `<img>`.
- **Atribut `src`**: Format file ditentukan oleh ekstensi di `src` (`.jpg`, `.png`, `.svg`, dll).
- **Ukuran file & performa**: Pemahaman format berpengaruh pada kecepatan muat halaman.

## Soal Latihan

1. Anda memiliki logo perusahaan dengan latar belakang transparan. Format apa yang paling tepat?
2. Apa keunggulan WebP dibandingkan JPEG?
3. Kapan sebaiknya menggunakan GIF dan kapan menggunakan format lain untuk animasi?

<details><summary>Jawaban</summary>

1. **PNG** atau **SVG**. PNG untuk logo bitmap dengan transparansi, SVG untuk logo vektor yang bisa diperbesar tanpa batas.

2. WebP mendukung kompresi lossy maupun lossless, mendukung transparansi, animasi, dan umumnya menghasilkan ukuran file 25-34% lebih kecil dari JPEG dengan kualitas visual setara.

3. GIF cocok untuk animasi pendek dan sederhana (maks 256 warna) seperti loading spinner atau stiker. Untuk animasi yang lebih panjang, berkualitas tinggi, atau dengan banyak warna, lebih baik menggunakan video (MP4 via `<video>`) atau animasi CSS/JS.

</details>
