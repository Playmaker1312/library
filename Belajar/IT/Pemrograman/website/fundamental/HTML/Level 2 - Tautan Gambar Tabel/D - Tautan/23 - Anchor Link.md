# Anchor Link

## Penjelasan

Anchor link adalah tautan yang mengarah ke bagian tertentu dalam halaman yang sama menggunakan atribut `id` sebagai target. Cukup gunakan `href="#nama-id"` untuk melompat ke elemen dengan `id="nama-id"`.

## Fungsi

Memudahkan navigasi dalam halaman panjang tanpa perlu scroll manual — pengguna langsung melompat ke bagian yang diinginkan.

## Cara Pengimplementasian

```html
<!-- Daftar isi — tautan ke anchor -->
<a href="#pendahuluan">Pendahuluan</a>
<a href="#pembahasan">Pembahasan</a>
<a href="#kesimpulan">Kesimpulan</a>

<!-- Bagian tujuan — harus punya id yang cocok -->
<h2 id="pendahuluan">Pendahuluan</h2>
<p>Isi pendahuluan...</p>

<h2 id="pembahasan">Pembahasan</h2>
<p>Isi pembahasan...</p>

<h2 id="kesimpulan">Kesimpulan</h2>
<p>Isi kesimpulan...</p>
```

**Syarat:** Nilai `href` setelah `#` harus PERSIS sama dengan nilai `id` tujuan (case-sensitive).

## Analogi (tema RUMAH/BANGUNAN)

Anchor link seperti **nomor ruangan** di sebuah gedung besar. Di lobi ada papan: "Ruang 201 — Keuangan, Ruang 202 — HRD". Pengunjung tinggal mencari nomor ruangan dan langsung menuju ke sana tanpa harus berkeliling mengecek setiap pintu. `id` adalah nomor ruangan, `href="#201"` adalah petunjuk arahnya.

## Dipakai Untuk

- Daftar isi artikel atau dokumentasi panjang
- Tombol "Kembali ke atas" (`<a href="#top">` atau `<a href="#">`)
- Navigasi antar-bab dalam satu halaman
- Tab section dalam halaman

## Kesalahan Umum

- Kesalahan ketik: `id="pendahuluan"` tapi `href="#pendahuluan"` → tidak cocok
- ID mengandung spasi — HTML tidak mengizinkan spasi dalam `id`
- Dua elemen memiliki `id` yang sama — browser hanya akan merujuk ke elemen pertama
- Lupa menambahkan `id` pada elemen tujuan — tautan tidak akan bekerja

## Koneksi dengan Materi Sebelumnya

Anchor link adalah variasi dari tag `<a>` yang sudah dipelajari. Kali ini `href` tidak menunjuk ke file eksternal, melainkan ke bagian dalam halaman yang sama — memanfaatkan atribut `id` yang sebelumnya digunakan untuk styling CSS.

## Soal Latihan

1. Buat anchor link menuju bagian dengan id `fitur-unggulan`.
2. Apa yang terjadi jika id tujuan tidak ditemukan di halaman?

<details><summary>Jawaban</summary>

1. `<a href="#fitur-unggulan">Fitur Unggulan</a>` dan di bagian tujuan: `<h2 id="fitur-unggulan">Fitur Unggulan</h2>`
2. Tautan tidak akan melakukan apa-apa (halaman tidak bergerak) atau browser mungkin hanya akan scroll ke posisi paling atas.

</details>
