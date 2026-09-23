# Cara Browser Membaca HTML

## Penjelasan
Saat kita membuka file HTML di browser, browser tidak langsung menampilkan halaman. Browser — seperti Chrome, Firefox, Edge — membaca file HTML baris per baris, lalu **mem-parse** (menganalisis) kode tersebut menjadi struktur data internal bernama **DOM Tree** (Document Object Model). Setelah DOM selesai dibangun, browser baru merender halaman secara visual.

## Fungsi
- Mengubah teks HTML mentah menjadi halaman visual yang interaktif
- Membangun DOM Tree yang bisa dimanipulasi oleh JavaScript
- Menangani error kode HTML agar tetap bisa ditampilkan (tidak crash)
- Mengintegrasikan CSS dan JavaScript ke dalam halaman

## Cara Pengimplementasian
```html
<!DOCTYPE html>
<html lang="id">
<head>
  <title>Contoh</title>
</head>
<body>
  <h1>Halo Dunia</h1>
</body>
</html>
```
Saat kode di atas dibuka di browser, prosesnya: download → parse HTML → bangun DOM → parse CSS (jika ada) → bangun Render Tree → Layout → Paint → tampil.

## Analogi
Browser seperti **kontraktor yang membaca cetak biru rumah**. Cetak biru (HTML) dibaca lembar demi lembar. Kontraktor membayangkan struktur 3D rumah di kepalanya — itulah DOM Tree. Lalu dia mulai membangun tembok (Render Tree), mengecat (Paint), dan hasilnya rumah jadi yang bisa dihuni.

## Dipakai Untuk
- Memahami mengapa tampilan kadang berbeda antar browser
- Debugging halaman web lewat DevTools
- Optimasi performa loading halaman
- Menulis kode HTML yang "browser-friendly"

## Kesalahan Umum
- Mengira semua browser membaca HTML dengan cara persis sama → ada perbedaan kecil (rendering engine berbeda)
- Tidak mengecek halaman di DevTools → padahal di Elements tab kita bisa lihat DOM asli
- Membiarkan error HTML tanpa diperbaiki → browser tetap jalan, tapi hasilnya tidak terprediksi
- Lupa memahami async/defer pada script → bisa blokir pembentukan DOM

## Koneksi dengan Materi Sebelumnya
Setelah tahu apa itu HTML (file 01), sekarang kita pahami **bagaimana HTML diproses** oleh browser. Ini penting agar kita bisa menulis kode yang efisien, tahu di mana letak masalah saat halaman error, dan mengerti DevTools.

## Soal Latihan
1. Apa itu DOM Tree dan bagaimana browser membangunnya?
2. Sebutkan langkah-langkah utama yang dilakukan browser dari membuka file HTML hingga halaman tampil.

<details>
<summary>Jawaban</summary>
1. DOM Tree adalah struktur data pohon yang merepresentasikan seluruh elemen HTML. Browser membangunnya dengan mem-parsing HTML baris per baris, mengubah tag menjadi node pohon.
2. Download HTML → Parse HTML → Bangun DOM Tree → Parse CSS → Bangun Render Tree → Layout (posisi & ukuran) → Paint (warna & gambar) → tampil di layar.
</details>
