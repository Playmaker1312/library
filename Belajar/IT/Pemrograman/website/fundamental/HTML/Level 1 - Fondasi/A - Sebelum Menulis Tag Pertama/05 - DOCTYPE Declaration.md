# DOCTYPE Declaration

## Penjelasan
`<!DOCTYPE html>` adalah deklarasi yang **wajib** ditulis di baris pertama dokumen HTML. Dia memberitahu browser versi HTML yang digunakan. Untuk HTML5, deklarasinya cukup `<!DOCTYPE html>` (tidak perlu versi panjang seperti HTML4). DOCTYPE **bukan tag HTML**, melainkan instruksi khusus untuk browser.

## Fungsi
- Memberi tahu browser untuk menggunakan **standards mode** (render sesuai spesifikasi web modern)
- Mencegah browser masuk ke **quirks mode** (mode lama untuk kompatibilitas ke belakang yang tidak konsisten)
- Memastikan halaman dirender secara konsisten di semua browser

## Cara Pengimplementasian
```html
<!-- WAJIB: baris pertama, sebelum tag <html> -->
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Contoh DOCTYPE</title>
</head>
<body>
  <p>Halaman ini menggunakan standards mode.</p>
</body>
</html>
```

## Analogi
Deklarasi DOCTYPE seperti **menyerahkan surat izin mendirikan bangunan (IMB) sebelum kontraktor mulai membangun rumah**. Surat IMB memberitahu kontraktor: "Ini aturan dan standar bangunan modern yang harus kamu ikuti." Tanpa IMB, kontraktor bingung — apakah pakai standar lama atau baru? Akibatnya, bangunan bisa asal-asalan (quirks mode).

## Dipakai Untuk
- Setiap dokumen HTML tanpa terkecuali
- Memastikan kompatibilitas lintas browser
- Mengaktifkan fitur CSS modern (Flexbox, Grid, dll)

## Kesalahan Umum
- Lupa menulis DOCTYPE → browser masuk quirks mode, tata letak kacau
- Menulis DOCTYPE versi lama (`<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN">`) → tidak perlu untuk HTML5
- Meletakkan spasi atau komentar sebelum DOCTYPE → DOCTYPE harus baris pertama
- Menganggap DOCTYPE sebagai tag HTML → padaha ini instruksi, bukan tag

## Koneksi dengan Materi Sebelumnya
Setelah berhasil membuat file HTML pertama (file 04), kita perlu tahu bahwa deklarasi `<!DOCTYPE html>` sangat penting. Tanpa ini, struktur yang sudah kita pelajari bisa dirender secara tidak konsisten oleh browser. DOCTYPE adalah "pintu gerbang" yang harus dilewati sebelum browser membaca seluruh dokumen.

## Soal Latihan
1. Apa yang terjadi jika kita tidak menulis `<!DOCTYPE html>` di baris pertama?
2. Sebutkan perbedaan standards mode dan quirks mode.

<details>
<summary>Jawaban</summary>
1. Browser akan masuk ke quirks mode — mode render lama untuk situs jadul. Layout, CSS, dan box model bisa tidak konsisten antar browser.
2. Standards mode: browser mengikuti spesifikasi web modern secara ketat. Quirks mode: browser meniru perilaku browser lama (IE5/Netscape) untuk kompatibilitas, menyebabkan hasil render tidak standar dan tidak konsisten.
</details>
