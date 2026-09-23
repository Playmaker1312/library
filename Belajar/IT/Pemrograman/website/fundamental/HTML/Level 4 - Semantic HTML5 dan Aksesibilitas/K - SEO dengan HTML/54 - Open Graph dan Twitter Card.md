# Open Graph dan Twitter Card

## Penjelasan

**Open Graph (OG)** adalah protokol yang dikembangkan oleh Facebook yang memungkinkan halaman web menjadi "kaya" ketika dibagikan di media sosial. Dengan OG, Anda bisa mengontrol judul, deskripsi, gambar, dan tipe konten yang muncul saat tautan dibagikan. **Twitter Card** adalah padanannya untuk platform Twitter/X, dengan tambahan format khusus seperti `summary_large_image` dan `player`.

## Fungsi

- **Meningkatkan Click-Through Rate (CTR)**: Tampilan kaya (gambar + judul + deskripsi) lebih menarik daripada tautan biasa.
- **Branding Konsisten**: Memastikan tampilan tautan seragam di semua platform sosial.
- **Kontrol Penuh**: Anda yang menentukan gambar, judul, dan deskripsi — bukan tebakan algoritma platform.
- **Dukungan Multi-Platform**: Facebook, LinkedIn, Twitter, Telegram, WhatsApp, Discord, Slack, dan lainnya membaca OG tags.
- **Twitter Khusus**: Twitter Card memungkinkan format seperti foto besar, aplikasi, atau pemutar video.

## Cara Pengimplementasian

```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rumah Impian di BSD City | BangunKarya</title>
    <meta name="description" content="Temukan rumah impian Anda di BSD City dengan desain modern dan harga terjangkau. BangunKarya siap membantu pembangunan rumah Anda.">

    <!-- Open Graph Tags -->
    <meta property="og:title" content="Rumah Impian di BSD City | BangunKarya">
    <meta property="og:description" content="Temukan rumah impian Anda di BSD City dengan desain modern dan harga terjangkau. BangunKarya siap membantu.">
    <meta property="og:image" content="https://www.bangunkarya.com/images/rumah-bsd.jpg">
    <meta property="og:image:width" content="1200">
    <meta property="og:image:height" content="630">
    <meta property="og:url" content="https://www.bangunkarya.com/rumah/bsd-city">
    <meta property="og:type" content="website">
    <meta property="og:site_name" content="BangunKarya">
    <meta property="og:locale" content="id_ID">

    <!-- Twitter Card Tags -->
    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:site" content="@bangunkarya">
    <meta name="twitter:creator" content="@bangunkarya">
    <meta name="twitter:title" content="Rumah Impian di BSD City | BangunKarya">
    <meta name="twitter:description" content="Temukan rumah impian Anda di BSD City dengan desain modern dan harga terjangkau.">
    <meta name="twitter:image" content="https://www.bangunkarya.com/images/rumah-bsd-twitter.jpg">

</head>
<body>
    <!-- Konten halaman -->
</body>
</html>
```

### Tabel Tag Utama

| Tag | Contoh | Keterangan |
|-----|--------|-----------|
| `og:title` | "Rumah Impian di BSD City" | Judul konten (boleh berbeda dengan `<title>`) |
| `og:description` | 2-3 kalimat | Ringkasan konten untuk sosial media |
| `og:image` | URL gambar | Gambar preview (rasio 1.91:1, min 1200×630 px) |
| `og:url` | URL kanonikal | Tautan permanen konten |
| `og:type` | `website`, `article`, `product` | Jenis konten |
| `og:locale` | `id_ID` | Bahasa dan region |
| `twitter:card` | `summary_large_image` | Jenis kartu Twitter |
| `twitter:site` | `@bangunkarya` | Akun Twitter pemilik situs |
| `twitter:creator` | `@bangunkarya` | Akun Twitter pembuat konten |

## Analogi (tema RUMAH/BANGUNAN)

Bayangkan tautan web Anda adalah **sebuah rumah yang akan difoto untuk iklan properti**:

- **OG tags** = Paket foto profesional rumah Anda — bukan foto asal-asalan, tapi diambil dengan komposisi, cahaya, dan sudut terbaik agar terlihat menarik di brosur (media sosial).
- **`og:title`** = Nama properti yang tercetak besar di brosur: "Rumah Impian BSD City."
- **`og:description`** = Teks pemasaran singkat yang membuat calon pembeli penasaran.
- **`og:image`** = Foto utama rumah — ini yang pertama dilihat dan paling menentukan apakah orang tertarik atau tidak.
- **`og:url`** = Alamat lengkap rumah, bukan gang belakang.
- **Twitter Card** = Versi brosur khusus untuk pameran properti di Twitter — bisa tampil beda (misal: foto lebih besar) dibanding brosur Facebook.

## Dipakai Untuk

- Halaman blog/artikel yang dibagikan di media sosial
- Halaman produk e-commerce
- Halaman portofolio atau galeri
- Halaman video (OG tag `og:video` dan Twitter `player` card)
- Halaman aplikasi (OG tag `og:app_id`)
- Konten berita (OG tag `article:published_time`, `article:author`)

## Kesalahan Umum

1. **Tidak menyertakan `og:image`**: Tautan akan tampil tanpa gambar atau menebak gambar secara acak.
2. **Ukuran `og:image` tidak sesuai**: Minimal 1200×630 px; gambar terlalu kecil akan ditolak platform.
3. **`og:title` dan `<title>` sangat berbeda**: Membingungkan pengguna saat beralih dari sosial media ke tab browser.
4. **Tidak menambahkan `twitter:card`**: Twitter akan fallback ke OG tags, tapi formatnya bisa kurang optimal.
5. **Lupa `og:url`**: Platform sosial bisa menggunakan URL berbeda yang merusak analitik.
6. **Tidak menguji hasil**: Gunakan Facebook Sharing Debugger dan Twitter Card Validator untuk verifikasi.

## Koneksi dengan Materi Sebelumnya

- **Meta Tags Esensial**: OG dan Twitter Card adalah pengembangan dari meta tags. Jika materi sebelumnya adalah papan nama rumah, OG/Twitter Card adalah brosur pemasaran untuk media sosial.
- **Semantic HTML**: OG type (`article`, `product`, `video`) mencerminkan semantic dari konten halaman.
- **Responsive Design**: Gambar OG harus dioptimasi untuk perangkat mobile, melanjutkan prinsip responsive images.

## Soal Latihan

1. Sebutkan 3 properti Open Graph yang WAJIB ada!
2. Berapa ukuran minimum gambar untuk `og:image`?
3. Apa perbedaan antara `twitter:card` dengan nilai `summary` dan `summary_large_image`?
4. Tuliskan kode OG tag untuk halaman dengan judul "Paket Renovasi Rumah Murah" dan gambar `https://example.com/renovasi.jpg`.
5. Alat apa yang digunakan untuk mengecek apakah OG tags sudah benar?

<details><summary>Jawaban</summary>

1. `og:title`, `og:description`, `og:image` (atau minimal `og:title` dan `og:image`).
2. 1200 × 630 piksel (rasio 1.91:1, minimum 200×200 px untuk square).
3. `summary` menampilkan kartu kecil dengan gambar thumbnail (persegi); `summary_large_image` menampilkan gambar besar (landscape) di bagian atas kartu.
4. ```html
   <meta property="og:title" content="Paket Renovasi Rumah Murah">
   <meta property="og:description" content="Dapatkan paket renovasi rumah murah dengan kualitas terbaik hanya di BangunKarya.">
   <meta property="og:image" content="https://example.com/renovasi.jpg">
   <meta property="og:url" content="https://example.com/paket-renovasi">
   ```
5. Facebook Sharing Debugger (developers.facebook.com/tools/debug) dan Twitter Card Validator.

</details>
