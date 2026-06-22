# Tag `<a>` Hyperlink

## Penjelasan

Tag `<a>` (anchor) adalah elemen HTML yang digunakan untuk membuat hyperlink ke halaman web, file, alamat email, atau bagian tertentu dalam halaman yang sama. Tag ini merupakan fondasi dari navigasi web.

## Fungsi

Menghubungkan satu dokumen HTML dengan dokumen lain, baik internal maupun eksternal, sehingga pengguna bisa berpindah dari satu halaman ke halaman lain dengan satu klik.

## Cara Pengimplementasian

```html
<!-- Wajib menyertakan atribut href -->
<a href="https://example.com">Kunjungi Example</a>

<!-- Teks deskriptif yang jelas -->
<a href="profil.html">Lihat Profil Saya</a>

<!-- Hindari teks tidak deskriptif -->
<!-- JANGAN: <a href="profil.html">Klik di sini</a> -->
```

## Analogi (tema RUMAH/BANGUNAN)

Tag `<a>` seperti **pintu** dalam sebuah rumah. Setiap pintu memiliki alamat tujuan (`href`) dan label yang menjelaskan ke mana pintu itu mengarah (teks deskriptif). Tanpa label yang jelas, penghuni rumah tidak tahu ke mana pintu itu menuju.

## Dipakai Untuk

- Navigasi antar halaman website
- Tautan ke situs eksternal
- Unduhan file
- Tautan email dan telepon
- Navigasi dalam satu halaman (anchor link)

## Kesalahan Umum

- Tidak menyertakan atribut `href` — tautan tidak bisa diklik
- Menggunakan teks tautan seperti "Klik di sini" atau "Baca selengkapnya" tanpa konteks — buruk untuk aksesibilitas dan SEO
- Lupa menutup tag `</a>` — seluruh konten setelahnya ikut menjadi tautan

## Koneksi dengan Materi Sebelumnya

Setelah mempelajari teks, gambar, dan tabel, tag `<a>` menjadi jembatan yang menghubungkan halaman-halaman HTML yang telah dibuat menjadi sebuah website yang utuh.

## Soal Latihan

1. Buatlah sebuah tautan ke halaman `tentang.html` dengan teks "Tentang Kami".
2. Apa yang terjadi jika atribut `href` tidak diisi?

<details><summary>Jawaban</summary>

1. `<a href="tentang.html">Tentang Kami</a>`
2. Tag `<a>` tanpa `href` akan dianggap sebagai placeholder (tautan tidak bisa diklik) atau hanya teks biasa.

</details>
