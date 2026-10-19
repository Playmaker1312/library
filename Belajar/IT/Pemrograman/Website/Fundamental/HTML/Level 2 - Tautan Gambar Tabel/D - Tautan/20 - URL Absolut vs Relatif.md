# URL Absolut vs Relatif

## Penjelasan

**URL Absolut** adalah alamat lengkap yang mencakup protokol (`https://`) dan domain. **URL Relatif** adalah alamat pendek yang hanya menunjukkan lokasi file relatif terhadap halaman saat ini.

## Fungsi

- URL absolut untuk merujuk ke sumber daya di domain lain
- URL relatif untuk merujuk ke sumber daya di dalam domain yang sama — lebih portabel dan mudah dipelihara

## Cara Pengimplementasian

```html
<!-- URL Absolut -->
<a href="https://www.example.com/images/logo.png">Logo</a>

<!-- URL Relatif — file dalam folder yang sama -->
<a href="kontak.html">Kontak</a>

<!-- URL Relatif — naik satu folder (../) -->
<a href="../index.html">Beranda</a>

<!-- URL Relatif — masuk ke sub-folder -->
<a href="produk/detail.html">Detail Produk</a>
```

Struktur folder yang direkomendasikan:

```
project/
├── index.html
├── tentang.html
├── produk/
│   ├── index.html
│   └── detail.html
└── gambar/
    └── logo.png
```

## Analogi (tema RUMAH/BANGUNAN)

URL absolut seperti **alamat rumah lengkap**: "Jalan Merdeka No. 10, Jakarta". Siapa pun dari mana pun bisa menemukannya. URL relatif seperti **petunjuk di dalam rumah**: "Dari dapur, belok kiri ke ruang tamu". Petunjuk itu hanya berguna jika kamu sudah berada di dalam rumah tersebut.

## Dipakai Untuk

- Absolut: tautan ke situs lain, API eksternal, CDN
- Relatif: tautan internal website — memudahkan saat berpindah domain atau folder

## Kesalahan Umum

- Menggunakan `file:///C:/Users/...` untuk tautan internal — tidak akan berfungsi saat di-*upload* ke server
- Salah menghitung jumlah `../` sehingga tautan rusak (*broken link*)
- Mencampur URL absolut dan relatif secara tidak konsisten

## Koneksi dengan Materi Sebelumnya

Saat menggunakan tag `<a>` dan `<img>`, pemahaman URL absolut vs relatif sangat penting agar gambar dan halaman dapat ditemukan oleh browser, baik saat diakses secara lokal maupun setelah di-*hosting*.

## Soal Latihan

1. Dari halaman `produk/detail.html`, bagaimana cara menautkan ke `gambar/logo.png`?
2. Kapan sebaiknya menggunakan URL absolut?

<details><summary>Jawaban</summary>

1. `<a href="../gambar/logo.png">Logo</a>` — naik satu level dari `produk/` lalu masuk ke `gambar/`.
2. Saat menautkan ke sumber daya di domain lain, atau saat konten akan dibagikan/diembedd di situs lain.

</details>
