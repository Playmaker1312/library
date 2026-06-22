# Tautan Email, Telepon, dan Download

## Penjelasan

HTML mendukung protokol khusus pada atribut `href` untuk membuka aplikasi email (`mailto:`), melakukan panggilan telepon (`tel:`), dan mengunduh file (`download`). Ini memperluas fungsi tautan di luar sekadar membuka halaman web.

## Fungsi

- `mailto:` membuka aplikasi email default pengguna
- `tel:` memicu panggilan di perangkat mobile/desktop
- `download` memaksa browser mengunduh file, bukan membukanya

## Cara Pengimplementasian

```html
<!-- Tautan Email -->
<a href="mailto:info@example.com">Kirim Email</a>

<!-- Email dengan subject dan body -->
<a href="mailto:info@example.com?subject=Halo&body=Pesan%20test">
  Kirim Email dengan Subjek
</a>

<!-- Tautan Telepon -->
<a href="tel:+6281234567890">Hubungi +62 812-3456-7890</a>

<!-- Tautan Download -->
<a href="dokumen/brosur.pdf" download>Unduh Brosur</a>

<!-- Download dengan nama file kustom -->
<a href="dokumen/brosur.pdf" download="Brosur-Promo-2025.pdf">Unduh</a>
```

## Analogi (tema RUMAH/BANGUNAN)

- `mailto:` seperti **kotak surat** di depan rumah — orang bisa mengirim surat langsung ke alamat rumah
- `tel:` seperti **interkom** atau telepon rumah — sekali tekan, langsung terhubung
- `download` seperti **lemari arsip** — kamu bisa mengambil file dan menyimpannya di tempatmu sendiri

## Dipakai Untuk

- `mailto:` tautan "Hubungi Kami" di footer atau halaman kontak
- `tel:` tombol "Call Now" di website bisnis, terutama versi mobile
- `download`: unduhan file PDF, ZIP, gambar, atau dokumen

## Kesalahan Umum

- Menulis `mailto:` tanpa alamat email atau dengan spasi setelah `:`
- Menggunakan `tel:` tanpa kode negara (`+62` untuk Indonesia) — panggilan mungkin gagal dari luar negeri
- Menggunakan atribut `download` pada file dari domain lain — browser akan membuka file, bukan mengunduhnya (kebijakan CORS)
- Lupa meng-*encode* spasi pada parameter `mailto:` (gunakan `%20`)

## Koneksi dengan Materi Sebelumnya

Ini adalah pengembangan dari tag `<a>` yang sudah dipelajari. Atribut `download` terkait dengan materi gambar dan file — memungkinkan pengguna menyimpan aset yang sebelumnya hanya bisa dilihat di browser.

## Soal Latihan

1. Buat tautan untuk mengirim email ke `admin@website.com` dengan subjek "Pertanyaan".
2. Buat tautan unduh file `laporan.pdf` dengan nama tersimpan "Laporan-Tahunan.pdf".
3. Protokol apa yang digunakan untuk tautan telepon?

<details><summary>Jawaban</summary>

1. `<a href="mailto:admin@website.com?subject=Pertanyaan">Hubungi Admin</a>`
2. `<a href="laporan.pdf" download="Laporan-Tahunan.pdf">Unduh Laporan</a>`
3. Protokol `tel:` — contoh: `<a href="tel:+6281234567890">Telepon</a>`

</details>
