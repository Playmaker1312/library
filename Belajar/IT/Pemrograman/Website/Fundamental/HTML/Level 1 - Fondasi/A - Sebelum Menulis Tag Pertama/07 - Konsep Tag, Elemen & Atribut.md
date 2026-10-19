# Konsep Tag, Elemen & Atribut

## Penjelasan
Dalam HTML ada tiga konsep fundamental yang harus dipahami:
- **Tag**: penanda yang diapit `< >`. Terdiri dari tag pembuka (`<p>`) dan tag penutup (`</p>`). Beberapa tag tidak perlu penutup (self-closing), seperti `<br>` atau `<img>`.
- **Elemen**: kesatuan utuh dari tag pembuka + konten + tag penutup. Contoh: `<p>Halo</p>` adalah satu elemen paragraf.
- **Atribut**: informasi tambahan pada tag pembuka yang memberikan konfigurasi atau identitas. Contoh: `class`, `id`, `src`, `href`.

## Fungsi
- **Tag**: menandai awal dan akhir suatu jenis konten (judul, paragraf, gambar)
- **Elemen**: unit struktural terkecil dalam DOM yang bisa ditata dan dimanipulasi
- **Atribut**: memberikan metadata, styling hook, atau perilaku khusus pada elemen
- **Self-closing tag**: efisiensi untuk elemen yang tidak memiliki konten teks

## Cara Pengimplementasian
```html
<!-- Tag pembuka + konten + tag penutup = elemen -->
<p class="teks-biru">Ini adalah elemen paragraf</p>
<!-- ^tag^   ^atribut^   ^-------kontent------^  ^tag^ -->

<!-- Self-closing tag (tidak perlu penutup) -->
<img src="foto.jpg" alt="Pemandangan">
<br>
<input type="text" placeholder="Nama">

<!-- Atribut bisa multiple -->
<div id="header" class="container utama" data-info="halaman">
  Konten di sini
</div>
```

## Analogi
Tag, elemen, dan atribut ibarat **proses pembuatan jendela rumah**:
- **Tag** = cetakan atau mold — menentukan bentuk jendela (persegi, lingkaran). `<p>` mencetak paragraf, `<h1>` mencetak judul besar.
- **Elemen** = jendela jadi yang sudah terpasang di dinding (cetakan + kaca + bingkai). `<p>Halo</p>` adalah jendela utuh.
- **Atribut** = spesifikasi jendela — warna bingkai (class), ukuran (id), jenis kaca (style). Satu jendela bisa punya banyak spesifikasi.
- **Self-closing tag** = lubang ventilasi kecil yang tidak perlu bingkai dan kaca, cukup lubang saja (`<br>`, `<img>`).

## Dipakai Untuk
- Setiap baris kode HTML yang kita tulis selalu menggunakan kombinasi tag, elemen, dan atribut
- Memberi struktur dan makna pada konten
- Menghubungkan HTML dengan CSS (via class/id)
- Menghubungkan HTML dengan JavaScript (via id/event handler)

## Kesalahan Umum
- Mencampur istilah tag dan elemen secara salah → `<p>` adalah tag, `<p>Halo</p>` adalah elemen
- Tidak menutup tag → elemen jadi rusak, browser bingung
- Self-closing tag yang salah: `<br>` benar, `<br></br>` salah (tapi browser tetap mentolerir untuk beberapa tag)
- Atribut tidak konsisten: kadang pakai tanda kutip kadang tidak → selalu gunakan tanda kutip
- Menulis atribut yang tidak dikenal browser → tidak akan error tapi tidak berpengaruh

## Koneksi dengan Materi Sebelumnya
Setelah memahami struktur wajib HTML (file 06), sekarang kita pelajari **bahan baku** dari setiap bagian struktur tersebut. Tag, elemen, dan atribut adalah "kata" dan "kalimat" dalam bahasa HTML — tanpa menguasainya kita tidak bisa menulis konten apa pun di dalam `<body>`.

## Soal Latihan
1. Apa perbedaan antara tag dan elemen HTML? Berikan contoh.
2. Sebutkan tiga contoh self-closing tag dan jelaskan mengapa mereka tidak perlu tag penutup.

<details>
<summary>Jawaban</summary>
1. Tag adalah penanda `<p>` (pembuka) dan `</p>` (penutup). Elemen adalah kesatuan tag pembuka + konten + tag penutup, misalnya `<p>Halo dunia</p>`.
2. `<br>` (baris baru — tidak punya konten), `<img>` (menyematkan gambar — kontennya dari src), `<input>` (field input — kontennya dari value). Semuanya tidak punya konten teks di antara tag, jadi tidak perlu penutup.
</details>
