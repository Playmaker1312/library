# Tag `<img>`

## Penjelasan

Tag `<img>` digunakan untuk menyisipkan gambar ke dalam halaman HTML. Tag ini bersifat **self-closing**, artinya tidak memerlukan tag penutup (`</img>`). Gambar ditautkan melalui atribut `src` (source) dan `alt` (alternate text).

## Fungsi

- Menampilkan gambar dari file lokal atau URL eksternal.
- Memperkaya tampilan halaman dengan elemen visual.
- Mendukung pengaturan ukuran tampilan melalui `width` dan `height`.

## Cara Pengimplementasian

```html
<!-- Gambar dari file lokal -->
<img src="foto.jpg" alt="Foto pemandangan" width="600" height="400">

<!-- Gambar dari URL -->
<img src="https://example.com/gambar.png" alt="Logo website" style="width: 100%; max-width: 800px;">
```

## Analogi (tema RUMAH/BANGUNAN)

Bayangkan kamu sedang memasang **bingkai foto** di dinding rumah. Bingkai itu adalah tag `<img>`, dan foto yang kamu masukkan adalah file gambar yang ditunjuk oleh `src`. Ukuran bingkai (lebar dan tinggi) kamu atur sesuai keinginan, sama seperti atribut `width` dan `height`.

## Dipakai Untuk

- Menampilkan foto produk di toko online
- Ilustrasi artikel atau blog
- Logo perusahaan
- Ikon dan elemen dekoratif
- Banner atau hero section website

## Kesalahan Umum

- Lupa menambahkan `src` sehingga gambar tidak muncul.
- Tidak mencantumkan `alt`, yang melanggar standar aksesibilitas.
- Menggunakan `width` dan `height` yang tidak proporsional sehingga gambar terlihat gepeng.
- Merujuk path file yang salah (misalnya `images/foto.jpg` padahal file ada di folder lain).
- Menggunakan gambar berukuran besar tanpa kompresi, memperlambat halaman.

## Koneksi dengan Materi Sebelumnya

- **Tag `<a>` (tautan)**: Gambar bisa dibungkus `<a>` agar bisa diklik sebagai link.
- **Tag `<p>` (paragraf)**: Gambar biasanya diletakkan di dalam paragraf atau `<div>` untuk tata letak.
- **CSS styling**: `width`, `height`, `border`, `border-radius` bisa ditambahkan melalui CSS.

## Soal Latihan

1. Tuliskan tag `<img>` untuk menampilkan gambar bernama `rumah.jpg` dengan lebar 500 piksel dan teks alternatif "Rumah impian".
2. Sebutkan **tiga** atribut wajib atau penting pada tag `<img>` dan jelaskan fungsinya masing-masing.
3. Benarkah tag `<img>` memerlukan tag penutup `</img>`? Jelaskan.

<details><summary>Jawaban</summary>

1. `<img src="rumah.jpg" alt="Rumah impian" width="500">`

2. - `src`: menentukan lokasi/sumber file gambar.
   - `alt`: teks alternatif jika gambar gagal dimuat; penting untuk aksesibilitas.
   - `width` / `height`: mengatur lebar dan tinggi tampilan gambar (opsional, tapi disarankan).

3. Tidak. Tag `<img>` adalah **self-closing tag**, jadi ditulis hanya sebagai `<img ...>` tanpa `</img>` (pada HTML5). Pada XHTML boleh ditulis `<img ... />`.

</details>
