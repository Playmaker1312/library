# Membuat Navigasi Menu

## Penjelasan

Navigasi menu adalah kumpulan tautan yang membantu pengguna berpindah antar halaman dalam sebuah website. Di HTML, navigasi dibangun dengan kombinasi `<nav>`, `<ul>`, dan `<a>`.

## Fungsi

Memberikan struktur navigasi yang semantik, terorganisir, dan mudah diakses oleh pengguna maupun mesin pencari.

## Cara Pengimplementasian

```html
<nav>
  <ul>
    <li><a href="index.html">Beranda</a></li>
    <li><a href="profil.html">Profil</a></li>
    <li><a href="layanan.html">Layanan</a></li>
    <li><a href="kontak.html">Kontak</a></li>
  </ul>
</nav>
```

Setiap halaman harus bisa dijangkau dari halaman lainnya — navigasi yang lengkap dan konsisten.

## Analogi (tema RUMAH/BANGUNAN)

Navigasi menu adalah **papan petunjuk arah** di dalam sebuah gedung. Di lobi utama, ada papan yang menunjukkan ke mana setiap lorong menuju: "Ruang Rapat", "Kantor HRD", "Toilet". `<nav>` adalah papan itu sendiri, `<ul>` adalah daftar lorongnya, dan `<a>` adalah tanda arah ke setiap ruangan.

## Dipakai Untuk

- Menu utama website (header)
- Breadcrumb navigasi
- Daftar isi halaman dokumentasi
- Navigasi footer (sitemap sederhana)

## Kesalahan Umum

- Tidak menggunakan `<nav>` — navigasi kehilangan makna semantik
- Membuat menu hanya dengan `<div>` dan `<span>` — sulit diakses pembaca layar
- Tidak menyertakan tautan ke halaman saat ini (halaman aktif) atau malah menautkan ke halaman yang sama tanpa kebutuhan
- Lupa menambahkan tautan ke salah satu halaman penting

## Koneksi dengan Materi Sebelumnya

Setelah belajar tag `<a>` dan struktur URL, navigasi menu adalah penerapan nyata yang menggabungkan keduanya. Menu ini akan diletakkan di `<header>` dan digunakan di setiap halaman.

## Soal Latihan

1. Buat navigasi menu untuk halaman: Beranda, Blog, Galeri, Kontak.
2. Apa fungsi tag `<nav>` dibandingkan `<div>` biasa?

<details><summary>Jawaban</summary>

1.
```html
<nav>
  <ul>
    <li><a href="index.html">Beranda</a></li>
    <li><a href="blog.html">Blog</a></li>
    <li><a href="galeri.html">Galeri</a></li>
    <li><a href="kontak.html">Kontak</a></li>
  </ul>
</nav>
```

2. `<nav>` memberikan makna semantik bahwa elemen tersebut berisi navigasi utama, membantu SEO dan pembaca layar untuk mengidentifikasi menu navigasi.

</details>
