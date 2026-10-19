# Struktur Wajib Dokumen HTML

## Penjelasan
Setiap dokumen HTML yang valid memiliki struktur wajib yang terdiri dari beberapa elemen inti: `<html>` sebagai root, `<head>` untuk metadata, dan `<body>` untuk konten. Di dalam `<head>` terdapat `<meta charset>` dan `<meta viewport>` yang penting. Elemen `<title>` juga wajib ada untuk aksesibilitas dan SEO. Atribut `lang` pada `<html>` membantu mesin pencari dan pembaca layar.

## Fungsi
- `<html>`: elemen root yang membungkus seluruh dokumen
- `<head>`: wadah metadata (data tentang data) yang tidak tampil di halaman
- `<body>`: wadah semua konten yang tampil di browser
- `<meta charset="UTF-8">`: menentukan encoding karakter agar huruf/aksen terbaca benar
- `<meta name="viewport">`: mengatur skala halaman di perangkat mobile
- `<title>`: judul yang muncul di tab browser dan hasil pencarian
- `lang` attribute: memberi tahu bahasa halaman untuk aksesibilitas & SEO

## Cara Pengimplementasian
```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Judul Halaman Saya</title>
</head>
<body>
  <h1>Selamat Datang</h1>
  <p>Ini konten halaman.</p>
</body>
</html>
```

## Analogi
Bayangkan struktur HTML wajib seperti **denah rumah yang lengkap**:
- `<html>` = batas tanah/sertifikat rumah (segala sesuatu di dalamnya adalah milik rumah ini)
- `<head>` = lemari arsip dan dokumen penting (tidak terlihat di ruang tamu, tapi berisi informasi vital)
- `<meta charset>` = bahasa yang digunakan di rumah (Indonesia, Inggris, dll)
- `<meta viewport>` = cara rumah terlihat dari berbagai sudut (zoom in/out)
- `<title>` = papan nama rumah di depan pagar
- `<body>` = ruang-ruang di dalam rumah (dapur, kamar, ruang tamu — yang bisa dilihat dan dihuni)
- `lang` = tulisan "RUMAH" di papan informasi yang bisa dibaca siapa pun

## Dipakai Untuk
- Setiap halaman HTML tanpa terkecuali
- SEO (Search Engine Optimization)
- Aksesibilitas (screen reader)
- Tampilan responsif di perangkat mobile

## Kesalahan Umum
- Lupa menambahkan `<meta charset="UTF-8">` → karakter seperti é, ñ, atau emoji bisa rusak
- Tidak menyertakan `<meta name="viewport">` → halaman terlihat kecil di HP
- Meletakkan konten di dalam `<head>` → konten tidak akan tampil
- Tidak memberi atribut `lang` → screen reader salah membaca pengucapan
- Mengisi `<title>` dengan teks tidak deskriptif → buruk untuk SEO

## Koneksi dengan Materi Sebelumnya
Setelah tahu DOCTYPE (file 05), kita sekarang belajar struktur wajib yang mengikutinya. DOCTYPE + struktur `<html>`, `<head>`, `<body>` adalah fondasi yang harus ada di setiap file HTML. Materi selanjutnya akan mengisi detail di dalam ketiga bagian ini.

## Soal Latihan
1. Sebutkan tiga elemen utama yang wajib ada di setiap dokumen HTML dan jelaskan fungsi masing-masing.
2. Mengapa `<meta charset="UTF-8">` penting?

<details>
<summary>Jawaban</summary>
1. `<html>` = elemen root pembungkus seluruh dokumen. `<head>` = wadah metadata (tidak tampil di halaman). `<body>` = wadah semua konten yang tampil.
2. UTF-8 mendukung hampir semua karakter di dunia — huruf latin, aksara Asia, emoji, simbol. Tanpanya, karakter khusus bisa tampil kacau (mojibake).
</details>
