# Subresource Integrity (SRI)

## Penjelasan

Subresource Integrity (SRI) adalah fitur keamanan yang memungkinkan browser memverifikasi bahwa file yang diambil dari server pihak ketiga (CDN) belum dimodifikasi. SRI bekerja dengan mencocokkan hash kriptografi dari file yang diunduh dengan hash yang disediakan dalam atribut `integrity` pada elemen `<script>` atau `<link>`.

## Fungsi

- Memastikan file dari CDN tidak dirusak atau diganti dengan versi berbahaya
- Melindungi pengguna dari serangan rantai pasok (supply chain attack)
- Memberikan lapisan kepercayaan pada library eksternal seperti Bootstrap, jQuery, atau Font Awesome
- Membantu developer mendeteksi perubahan tak terduga pada resource pihak ketiga

## Cara Pengimplementasian

```html
<!-- SRI pada Bootstrap CSS via CDN -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css"
      integrity="sha384-YVtbVcVIQRVgN5Q0u6G6l1P1jRgGfI5jF5Y5F5f5f5f5f5f5f5f5f5f5f5f5f5f5"
      crossorigin="anonymous">

<!-- SRI pada script Bootstrap -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"
        integrity="sha384-YVtbVcVIQRVgN5Q0u6G6l1P1jRgGfI5jF5Y5F5f5f5f5f5f5f5f5f5f5f5f5f5f5"
        crossorigin="anonymous"></script>
```

> **Catatan**: Hash di atas adalah contoh. Hash asli bisa didapatkan dari [srihash.org](https://www.srihash.org/) atau dokumentasi resmi CDN.

## Analogi (tema RUMAH/BANGUNAN)

SRI bagaikan **segelenci stempel resmi pada paket material bangunan yang dikirim dari toko langganan**. Kamu memesan "semen merek A" dari toko bangunan langganan (CDN). Saat paket tiba, kamu memeriksa stempel keaslian di karung semen — jika stempelnya cocok dengan yang kamu catat sebelumnya (`integrity` hash), berarti isinya benar-benar semen merek A. Jika stempel rusak atau tidak cocok, kamu tolak paketnya — jangan sampai karung diisi pasir oleh oknum di tengah jalan.

## Dipakai Untuk

- Framework CSS/JS yang di-host di CDN publik (Bootstrap, Tailwind, React, Vue, Angular)
- Library utilitas seperti Lodash, Axios, Moment.js dari CDN
- Font dan icon via CDN (Font Awesome, Google Fonts)
- Aplikasi web yang memprioritaskan keamanan rantai pasok

## Kesalahan Umum

- Menggunakan hash yang salah atau hash untuk versi file yang berbeda
- Lupa menambahkan atribut `crossorigin` saat resource berasal dari origin berbeda
- Tidak memperbarui hash saat memperbarui versi library (menyebabkan resource diblokir)
- Menganggap SRI sudah cukup tanpa HTTPS — SRI tetap membutuhkan koneksi aman
- Mendapatkan hash dari sumber tidak resmi; selalu gunakan srihash.org atau dokumentasi resmi CDN

## Koneksi dengan Materi Sebelumnya

- **CSP (Materi 66)**: SRI dan CSP saling melengkapi — CSP mengontrol *dari mana* resource boleh dimuat, SRI memverifikasi *apa* yang dimuat dari sumber yang diizinkan
- **HTTPS (Level 6)**: SRI membutuhkan koneksi HTTPS agar hash tidak diintersepsi dan dimodifikasi saat transit
- **CDN (Level 4)**: SRI adalah pengaman penting saat menggunakan CDN yang di luar kendali langsung developer

## Soal Latihan

1. Apa yang terjadi jika hash `integrity` tidak cocok dengan file yang diunduh dari CDN?

2. Mengapa atribut `crossorigin` diperlukan saat menggunakan SRI pada resource lintas domain?

3. Di situs mana kamu bisa menghasilkan hash SRI untuk file dari CDN?

<details><summary>Jawaban</summary>

1. Browser akan memblokir eksekusi/penerapan file tersebut. File tidak akan dijalankan atau diterapkan sama sekali. Konsol browser akan menampilkan error SRI mismatch.

2. Atribut `crossorigin` diperlukan untuk mengakses response body secara transparan (tanpa cors mode). Tanpa atribut ini, browser tidak bisa membaca konten file untuk memverifikasi hash, sehingga SRI tidak akan berfungsi.

3. [srihash.org](https://www.srihash.org/) — situs resmi yang menyediakan generator hash SRI. Developer cukup memasukkan URL file CDN dan situs akan mengembalikan tag lengkap dengan hash yang sesuai.

</details>
