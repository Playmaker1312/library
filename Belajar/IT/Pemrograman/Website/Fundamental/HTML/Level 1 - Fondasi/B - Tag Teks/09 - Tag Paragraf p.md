# Tag Paragraf `<p>`

## Penjelasan
Tag `<p>` mendefinisikan sebuah paragraf. Paragraf adalah blok teks yang membentuk satu unit pemikiran. Browser secara otomatis menambahkan margin (jarak) di atas dan bawah setiap `<p>`, sehingga paragraf terlihat terpisah secara visual.

## Fungsi
- Mengelompokkan teks ke dalam unit paragraf yang bermakna
- Memberi jarak antar-paragraf secara otomatis (tanpa CSS)
- Membantu pembaca layar mengenali jeda antar blok teks
- Sebagai wadah default untuk teks di halaman HTML

## Cara Pengimplementasian
```html
<p>Ini adalah paragraf pertama. Isinya bisa panjang atau pendek. Browser akan menambahkan margin otomatis setelah paragraf ini.</p>
<p>Ini paragraf kedua. Perhatikan bahwa ada jarak antara paragraf pertama dan kedua, tanpa perlu menambahkan tag baris baru.</p>
```

## Analogi
Seperti **ruangan dalam rumah**. Setiap ruangan (`<p>`) memiliki fungsi tertentu dan dipisahkan oleh dinding (margin). Kamu tidak menempelkan ruang tamu langsung ke kamar tidur tanpa sekat — sama seperti kamu tidak menempelkan dua paragraf tanpa jarak. Setiap ruangan punya "dinding" sendiri.

## Dipakai Untuk
- Teks artikel, cerita, deskripsi
- Konten blog, berita, dokumentasi
- Setiap blok kalimat yang membentuk satu gagasan

## Kesalahan Umum
- Menaruh `<p>` di dalam `<p>` (tidak boleh — tag `<p>` otomatis menutup `<p>` sebelumnya)
- Menggunakan `<br>` berkali-kali untuk memberi jarak antar-paragraf — gunakan `<p>` saja
- Menaruh elemen block seperti `<div>` atau heading di dalam `<p>` (tidak valid)
- Membiarkan `<p>` kosong — lebih baik tidak usah pakai `<p>` jika tidak ada teks

## Koneksi dengan Materi Sebelumnya
Heading (`h1`-`h6`) memberi judul pada bagian konten. Paragraf (`<p>`) adalah "isi" dari bagian tersebut. Setelah header bangunan (heading), kamu butuh ruangan (paragraf) untuk meletakkan konten.

## Soal Latihan
1. Apa tag yang digunakan untuk membuat paragraf di HTML?
2. Apa yang terjadi jika kita menaruh `<p>` di dalam `<p>`?
3. Sebutkan satu elemen block yang TIDAK boleh diletakkan di dalam `<p>`!
4. Tulis dua paragraf tentang hewan kucing menggunakan `<p>`!
5. Benarkah `<p>` membutuhkan tag penutup `</p>`? Jelaskan!

<details><summary>Jawaban</summary>
1. `<p>`.<br>
2. `<p>` pertama akan otomatis ditutup saat `<p>` kedua dimulai — menyebabkan struktur tidak rapi.<br>
3. `<div>`, `<h1>`-`<h6>`, `<p>` sendiri, `<ul>`, `<ol>`, `<table>`, dll.<br>
4. 
```html
<p>Kucing adalah hewan peliharaan yang lucu dan mandiri. Mereka suka tidur dan bermain.</p>
<p>Kucing juga dikenal sebagai pemburu tikus yang handal. Mereka memiliki kumis yang sensitif.</p>
```<br>
5. Ya, `<p>` membutuhkan `</p>`. Walaupun browser kadang menoleransi, HTML yang valid harus punya tag penutup.
</details>
