# Atribut `alt`

## Penjelasan

Atribut `alt` (alternative text) pada tag `<img>` menyediakan teks alternatif yang akan ditampilkan jika gambar gagal dimuat. Teks ini juga dibaca oleh *screen reader* untuk membantu pengguna tunanetra memahami isi gambar.

## Fungsi

- **Aksesibilitas**: Membantu pengguna dengan disabilitas visual memahami konten gambar.
- **SEO**: Mesin pencari menggunakan `alt` untuk memahami isi gambar dan meningkatkan peringkat halaman.
- **Fallback**: Jika gambar rusak atau tidak bisa dimuat, teks `alt` akan muncul sebagai pengganti.
- **Dekoratif**: Jika `alt=""` (string kosong), *screen reader* akan melewatkan gambar karena dianggap dekoratif.

## Cara Pengimplementasian

```html
<!-- Gambar informatif: alt wajib diisi -->
<img src="denah-rumah.jpg" alt="Denah rumah type 36 dengan 2 kamar tidur dan 1 kamar mandi">

<!-- Gambar dekoratif: alt dikosongkan -->
<img src="garis-pemisah.png" alt="">

<!-- Gambar sebagai link: alt menjelaskan tujuan link -->
<a href="tentang.html">
  <img src="logo.png" alt="Halaman Tentang Kami">
</a>
```

## Analogi (tema RUMAH/BANGUNAN)

Atribut `alt` seperti **papan nama di depan rumah**. Jika rumah (gambar) tidak terlihat — entah karena gelap atau terhadap — papan nama tetap memberi tahu orang tentang apa yang ada di dalamnya. Untuk tanaman hias dekoratif di halaman (gambar dekoratif), papan nama tidak perlu dipasang (`alt=""`).

## Dipakai Untuk

- Gambar produk: `alt="Sepatu olahraga warna biru ukuran 42"`
- Foto artikel: `alt="Ilustrasi proses pembangunan rumah"`
- Ikon sosial media: `alt="Ikuti kami di Instagram"`
- Gambar dekoratif: `alt=""` (spasi kosong) agar dilewati *screen reader*
- Logo: `alt="Nama Perusahaan"`

## Kesalahan Umum

- Mengosongkan `alt` pada gambar informatif (seharusnya diisi deskripsi).
- Mengisi `alt` dengan frasa seperti "gambar" atau "image" — tidak informatif.
- Menulis `alt` terlalu panjang hingga dijadikan paragraf.
- Mengisi `alt` dengan kata kunci berlebihan (*keyword stuffing*) untuk SEO.
- Lupa mencantumkan atribut `alt` sama sekali.

## Koneksi dengan Materi Sebelumnya

- **Tag `<img>`**: Atribut `alt` adalah bagian dari tag `<img>`, berdampingan dengan `src`, `width`, `height`.
- **Aksesibilitas web**: Atribut `alt` adalah fondasi aksesibilitas konten non-teks.
- **SEO dasar**: `alt` adalah salah satu sinyal bagi mesin pencari tentang relevansi gambar.

## Soal Latihan

1. Tulis tag `<img>` yang benar untuk gambar logo perusahaan "Bangun Rumah" yang juga merupakan link ke halaman utama. Sertakan `alt` yang sesuai.
2. Apa yang terjadi jika `alt=""` (string kosong) pada *screen reader*?
3. Mengapa menulis `alt="gambar"` dianggap kesalahan?

<details><summary>Jawaban</summary>

1. `<a href="index.html"><img src="logo-bangun-rumah.png" alt="Beranda Bangun Rumah"></a>`

2. *Screen reader* akan **melewatkan gambar tersebut** karena dianggap dekoratif dan tidak perlu dibacakan.

3. Karena "gambar" tidak memberikan informasi apa pun tentang isi atau fungsi gambar tersebut. Alt text harus deskriptif, misalnya `alt="Logo Bangun Rumah dengan ikon palu dan paku"`.

</details>
