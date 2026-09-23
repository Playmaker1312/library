# Meta Tags Esensial

## Penjelasan

Meta tags adalah potongan kode HTML yang ditempatkan di dalam elemen `<head>` untuk memberikan informasi terstruktur tentang halaman web kepada mesin pencari dan browser. Meta tags tidak terlihat di halaman, tetapi dibaca oleh robot搜索引擎 (search engine crawler) untuk memahami, mengindeks, dan menampilkan konten di hasil pencarian.

## Fungsi

- **SEO (Search Engine Optimization)**: Membantu mesin pencari memahami relevansi dan konteks halaman.
- **Kontrol Tampilan Hasil Pencarian**: Menentukan judul dan deskripsi yang muncul di SERP (Search Engine Results Page).
- **Mencegah Duplikasi Konten**: Mengarahkan mesin pencari ke versi kanonikal halaman.
- **Mengatur Perilaku Crawler**: Memberi instruksi apakah halaman boleh diindeks atau diikuti tautannya.
- **Multi-Bahasa & Multi-Lokasi**: Memberi tahu Google tentang halaman alternatif dalam bahasa/region berbeda.

## Cara Pengimplementasian

```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <!-- 1. Title Tag (50-60 karakter) -->
    <title>Jasa Renovasi Rumah Profesional | BangunKarya</title>

    <!-- 2. Meta Description (150-160 karakter) -->
    <meta name="description"
          content="BangunKarya melayani jasa renovasi rumah, pembangunan baru, dan desain interior di Jakarta. Gratis konsultasi dan survei lokasi. Hubungi kami sekarang!">

    <!-- 3. Meta Robots -->
    <meta name="robots" content="index, follow">

    <!-- 4. Canonical Tag -->
    <link rel="canonical" href="https://www.bangunkarya.com/jasa-renovasi-rumah">

    <!-- 5. Hreflang Tags -->
    <link rel="alternate" hreflang="id" href="https://www.bangunkarya.com/jasa-renovasi-rumah">
    <link rel="alternate" hreflang="en" href="https://www.bangunkarya.com/en/home-renovation-services">
    <link rel="alternate" hreflang="x-default" href="https://www.bangunkarya.com/">

</head>
<body>
    <!-- Konten halaman -->
</body>
</html>
```

### Penjelasan Tag:

| Tag | Contoh Nilai | Fungsi |
|-----|-------------|--------|
| `title` | `Jasa Renovasi Rumah Profesional` | Judul yang muncul di SERP (wajib 50-60 karakter) |
| `description` | 150–160 karakter | Cuplikan deskripsi di bawah judul SERP |
| `robots` | `index, follow` | Perintah untuk diindeks dan ikuti tautan |
| `canonical` | URL utama | Cegah duplikasi dengan menyatakan URL resmi |
| `hreflang` | `id`, `en`, `x-default` | Tautkan ke halaman versi bahasa lain |

## Analogi (tema RUMAH/BANGUNAN)

Bayangkan meta tags adalah **papan nama dan brosur depan rumah Anda**:

- **Title** = Nama rumah yang tertera di papan besar — jelas, singkat, langsung menggambarkan identitas bangunan.
- **Description** = Brosur singkat di lobi depan yang menjelaskan apa yang ada di dalam rumah.
- **Robots** = Petunjuk untuk kurir: "Silakan masuk" (`index, follow`) atau "Jangan tinggalkan paket di sini" (`noindex`).
- **Canonical** = Alamat resmi rumah. Jika ada pintu samping atau belakang (URL duplikat), semuanya diarahkan ke pintu utama.
- **Hreflang** = Papan petunjuk multibahasa: "Untuk tamu berbahasa Inggris, silakan ke pintu sebelah."

## Dipakai Untuk

- Halaman blog, artikel, dan landing page
- Situs e-commerce (halaman produk/kategori)
- Situs multibahasa
- Halaman yang rentan duplikasi konten (misal: parameter URL, filter, sorting)
- Semua halaman web yang ingin tampil di hasil pencarian Google

## Kesalahan Umum

1. **Title terlalu pendek/panjang**: `< 30` karakter terbuang; `> 60` karakter dipotong Google.
2. **Deskripsi terlalu pendek/panjang**: `< 120` karakter tidak informatif; `> 160` terpotong.
3. **Meta robots `noindex` tidak sengaja**: Halaman tidak akan muncul di Google sama sekali.
4. **Canonical menunjuk ke URL yang salah**: Akibatnya halaman yang benar tidak terindeks.
5. **Hreflang tidak resiprokal**: Jika halaman A menunjuk ke B, B harus menunjuk balik ke A.
6. **Duplicate title & description**: Setiap halaman harus punya title/description unik.

## Koneksi dengan Materi Sebelumnya

- **HTML Dasar**: Meta tags ditempatkan di `<head>`, yang sudah dipelajari di materi HTML struktur dasar.
- **Semantic HTML**: Penggunaan `<title>` yang benar melengkapi struktur heading (`<h1>`–`<h6>`) untuk hierarki informasi yang konsisten.
- **Aksesibilitas**: Meta description membantu pengguna screen reader mendapatkan ringkasan halaman sebelum memutuskan membuka tautan.

## Soal Latihan

1. Sebutkan 3 meta tags esensial yang WAJIB ada di setiap halaman web!
2. Berapa panjang ideal untuk title tag? Dan meta description?
3. Apa fungsi tag `<link rel="canonical">`?
4. Jika Anda memiliki website dengan bahasa Indonesia dan Inggris, tag apa yang harus ditambahkan?
5. Tuliskan contoh meta robots yang memberitahu crawler untuk TIDAK mengindeks halaman tetapi tetap mengikuti tautan.

<details><summary>Jawaban</summary>

1. `<title>`, `<meta name="description">`, dan `<meta name="robots">`.
2. Title: 50–60 karakter. Description: 150–160 karakter.
3. Menunjukkan URL kanonikal (resmi/utama) dari halaman untuk mencegah masalah konten duplikat.
4. Tag `<link rel="alternate" hreflang="...">` untuk setiap versi bahasa, termasuk `x-default`.
5. `<meta name="robots" content="noindex, follow">`

</details>
