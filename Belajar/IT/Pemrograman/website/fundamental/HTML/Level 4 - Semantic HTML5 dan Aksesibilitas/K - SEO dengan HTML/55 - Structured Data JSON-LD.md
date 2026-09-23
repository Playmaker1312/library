# Structured Data JSON-LD

## Penjelasan

**Structured Data** (data terstruktur) adalah format standar untuk memberikan informasi tentang halaman dan mengklasifikasikan kontennya kepada mesin pencari. **JSON-LD** (JavaScript Object Notation for Linked Data) adalah salah satu format data terstruktur yang direkomendasikan Google. Data ditulis dalam blok `<script type="application/ld+json">` di dalam `<head>` atau `<body>` halaman web.

## Fungsi

- **Rich Results (Hasil Kaya)**: Memungkinkan halaman ditampilkan dengan elemen khusus seperti bintang ulasan, harga, foto, dan jadwal acara langsung di SERP.
- **Knowledge Graph**: Membantu Google menghubungkan entitas (nama orang, organisasi, tempat, dll) di seluruh web.
- **Voice Search Optimization**: Data terstruktur memudahkan asisten suara (Google Assistant, Siri) memahami dan membacakan informasi.
- **Mesin Pencari Lain**: Bing, Yahoo, dan Yandex juga mendukung schema.org/JSON-LD.
- **Akurasi Konten**: Google lebih percaya pada konten yang memiliki data terstruktur karena informasinya eksplisit dan tidak ambigu.

## Cara Pengimplementasian

```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Arsitek Rumah Modern | BangunKarya</title>
    <meta name="description" content="BangunKarya adalah biro arsitek dan kontraktor rumah modern di Indonesia.">

    <!-- JSON-LD Structured Data: Person Schema -->
    <script type="application/ld+json">
    {
        "@context": "https://schema.org",
        "@type": "Person",
        "name": "Andi Pratama",
        "givenName": "Andi",
        "familyName": "Pratama",
        "jobTitle": "Arsitek Utama",
        "description": "Arsitek profesional dengan pengalaman 10+ tahun di bidang desain rumah modern dan renovasi bangunan.",
        "gender": "male",
        "birthDate": "1988-05-15",
        "nationality": {
            "@type": "Country",
            "name": "Indonesia"
        },
        "address": {
            "@type": "PostalAddress",
            "streetAddress": "Jl. Merdeka No. 123",
            "addressLocality": "Jakarta Selatan",
            "addressRegion": "DKI Jakarta",
            "postalCode": "12950",
            "addressCountry": "ID"
        },
        "email": "andi@bangunkarya.com",
        "telephone": "+62-21-1234-5678",
        "url": "https://www.bangunkarya.com/tim/andi-pratama",
        "image": "https://www.bangunkarya.com/images/andi-pratama.jpg",
        "sameAs": [
            "https://www.linkedin.com/in/andipratama",
            "https://www.instagram.com/andipratama.arstek"
        ],
        "alumniOf": {
            "@type": "CollegeOrUniversity",
            "name": "Institut Teknologi Bandung"
        },
        "worksFor": {
            "@type": "Organization",
            "name": "BangunKarya"
        },
        "knowsAbout": [
            {
                "@type": "Thing",
                "name": "Desain Rumah Modern"
            },
            {
                "@type": "Thing",
                "name": "Renovasi Bangunan"
            }
        ]
    }
    </script>

</head>
<body>
    <!-- Konten halaman -->
</body>
</html>
```

### Tipe Schema Lain yang Umum

| Schema | `@type` | Contoh Penggunaan |
|--------|---------|-------------------|
| Person | `Person` | Profil penulis, anggota tim, pekerja |
| Organization | `Organization` | Profil perusahaan, logo, kontak |
| LocalBusiness | `LocalBusiness` | Toko fisik, restoran, bengkel |
| Product | `Product` | Harga, rating, ketersediaan stok |
| Article | `Article` | Berita, blog post (headline, datePublished) |
| BreadcrumbList | `BreadcrumbList` | Navigasi breadcrumb |
| FAQPage | `FAQPage` | Halaman tanya-jawab (mainEntity) |
| Event | `Event` | Acara, workshop, seminar (date, location) |

## Analogi (tema RUMAH/BANGUNAN)

Bayangkan JSON-LD adalah **sertifikat dan dokumen lengkap rumah Anda**:

- **Data terstruktur** = Berkas IMB (Izin Mendirikan Bangunan), sertifikat tanah, denah, spesifikasi material — semua informasi teknik rumah dicatat rapi.
- **Tanpa JSON-LD** = Google hanya melihat tampilan luar rumah dan menebak-nebak apa isinya. Bisa salah tebak.
- **Dengan JSON-LD** = Google membaca dokumen lengkap: "Ini rumah Pak Andi, arsitek, dibangun tahun 2020, luas 200 m², 3 kamar, 2 kamar mandi."
- **Rich Results** = Google menampilkan rumah Anda di pameran properti dengan papan informasi lengkap — bukan cuma alamat, tapi juga foto, harga, ulasan, dan jadwal open house.

## Dipakai Untuk

- Halaman profil penulis/anggota tim (Person)
- Halaman perusahaan/kontak (Organization, LocalBusiness)
- Halaman produk (Product)
- Halaman artikel berita/blog (Article, NewsArticle)
- Halaman FAQ (FAQPage)
- Halaman resep masakan (Recipe)
- Halaman acara (Event)
- Halaman film/musik (Movie, MusicAlbum)

## Kesalahan Umum

1. **Salah format JSON (koma berlebih, kurung tidak seimbang)**: Validator Google akan menolak.
2. **Menggunakan tipe yang salah**: Misal memakai `Product` untuk halaman profil orang (harus `Person`).
3. **Data tidak cocok dengan konten halaman**: Jika JSON-LD bilang "Rating: 5" tapi halaman tidak menampilkan rating, Google bisa memberikan sanksi.
4. **Structured Data tidak terlihat oleh pengguna**: JSON-LD disembunyikan, tetapi jangan berisi spam — Google bisa mendeteksi manipulasi.
5. **Tidak menerapkan markup ke halaman yang tepat**: Misal, beri `Product` hanya di halaman produk, bukan di halaman kategori.

> **Tips**: Selalu validasi JSON-LD Anda dengan [Google Rich Results Test](https://search.google.com/test/rich-results) atau [Schema.org Validator](https://validator.schema.org/) sebelum deploy.

## Koneksi dengan Materi Sebelumnya

- **Meta Tags**: JSON-LD adalah pelengkap meta tags — jika meta tags adalah papan nama, JSON-LD adalah dokumen teknis lengkap rumah.
- **Open Graph / Twitter Card**: OG mengontrol tampilan di sosial media, JSON-LD mengontrol tampilan di mesin pencari. Keduanya bekerja bersama untuk visibilitas maksimal.
- **Semantic HTML**: Sama seperti semantic HTML yang memberi makna pada struktur halaman, JSON-LD memberi makna pada konten secara machine-readable.

## Soal Latihan

1. Apa kepanjangan JSON-LD?
2. Sebutkan 3 properti wajib untuk schema `Person`!
3. Tuliskan JSON-LD minimal untuk schema `Organization` dengan nama "BangunKarya" dan URL "https://bangunkarya.com".
4. Alat apa yang digunakan Google untuk menguji validitas rich results?
5. Apa dampak jika JSON-LD berisi data yang tidak sesuai dengan konten halaman?

<details><summary>Jawaban</summary>

1. JavaScript Object Notation for Linked Data.
2. `@context`, `@type`, dan `name` (wajib). Juga disarankan: `url`, `image`, `jobTitle`.
3. ```json
   {
       "@context": "https://schema.org",
       "@type": "Organization",
       "name": "BangunKarya",
       "url": "https://bangunkarya.com"
   }
   ```
4. Google Rich Results Test (search.google.com/test/rich-results).
5. Google dapat memberikan sanksi atau menolak menampilkan rich results, dan berpotensi terkena penalti manual karena dianggap manipulatif atau spam.

</details>
