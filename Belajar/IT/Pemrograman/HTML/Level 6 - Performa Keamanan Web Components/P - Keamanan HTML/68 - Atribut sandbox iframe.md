# Atribut sandbox iframe

## Penjelasan

Atribut `sandbox` pada elemen `<iframe>` memberlakukan serangkaian pembatasan terhadap konten yang dimuat di dalam iframe. Dengan menerapkan `sandbox`, browser akan memblokir sejumlah fitur potensial berbahaya seperti eksekusi script, pengiriman form, akses ke parent document, dan lainnya. Atribut ini bisa dikosongkan (semuanya diblokir) atau diisi dengan nilai tertentu untuk mengizinkan fitur secara selektif.

## Fungsi

- Membatasi kemampuan konten iframe untuk menjalankan script berbahaya
- Mencegah iframe mengirimkan form ke server yang tidak dikenal
- Mencegah iframe mengakses atau memanipulasi halaman induk (parent)
- Mencegah popup, plugin, dan navigasi paksa dari dalam iframe
- Memberikan lingkungan eksekusi "terisolasi" untuk konten pihak ketiga

## Cara Pengimplementasian

```html
<!-- sandbox tanpa nilai — blokir semua fitur (paling ketat) -->
<iframe src="https://example.com" sandbox></iframe>

<!-- sandbox dengan izin script dan form -->
<iframe src="https://example.com" sandbox="allow-scripts allow-forms"></iframe>

<!-- sandbox untuk embed YouTube — perlu allow-scripts dan allow-same-origin -->
<iframe src="https://www.youtube.com/embed/VIDEO_ID"
        sandbox="allow-scripts allow-same-origin allow-popups"
        allowfullscreen></iframe>
```

## Analogi (tema RUMAH/BANGUNAN)

Sandbox pada iframe bagaikan **membangun sebuah "rumah kaca" di halaman belakang rumah utama**. Penghuni di rumah kaca (`iframe`) tidak bisa masuk ke rumah utama (`parent document`), tidak bisa membuka pintu gerbang utama (`allow-top-navigation`), tidak bisa menggunakan kompor (`allow-scripts`), dan tidak bisa menerima paket (`allow-forms`). Jika rumah kaca dikosongkan tanpa izin (`sandbox` saja), penghuninya bahkan tidak bisa bernapas. Kamu sebagai pemilik rumah bisa memilih untuk mengizinkan beberapa aktivitas dengan menambahkan "izin" seperti `allow-scripts` — setara dengan memberikan tabung oksigen ke rumah kaca.

## Dipakai Untuk

- Embed konten pihak ketiga seperti YouTube, Vimeo, Twitter, atau Instagram
- Menampilkan halaman iklan tanpa risiko terhadap situs utama
- Menjalankan kode buatan pengguna di lingkungan yang aman (code playground)
- Pratinjau URL atau file dalam aplikasi web

## Kesalahan Umum

- Menggunakan `allow-scripts` tanpa `allow-same-origin` untuk embed YouTube (player tidak berfungsi)
- Menambahkan `allow-scripts` dan `allow-same-origin` bersamaan — ini sangat berbahaya karena iframe bisa menghapus sandbox-nya sendiri
- Tidak memberikan `allow-popups` saat embed membutuhkan window baru (misal login via link YouTube)
- Menganggap sandbox melindungi dari semua serangan — sandbox hanya membatasi fitur, bukan enkripsi
- Lupa bahwa beberapa embed modern memerlukan `allow-presentation` atau `allow-fullscreen`

## Koneksi dengan Materi Sebelumnya

- **iframe Dasar (Level 4)**: Tanpa sandbox, iframe biasa memiliki akses penuh ke fitur browser — sandbox membatasi akses tersebut
- **CSP (Materi 66)**: CSP `frame-ancestors` mengontrol domain mana yang boleh me-load halaman kita sebagai iframe; sandbox mengontrol apa yang bisa dilakukan iframe itu sendiri
- **Formulir (Level 3)**: Sandbox dengan `allow-forms` mengizinkan form di dalam iframe — penting dipahami untuk mencegah form yang mengirim data ke server berbahaya

## Soal Latihan

1. Apa yang terjadi jika `<iframe src="...">` ditambahkan atribut `sandbox` tanpa nilai?

2. Mengapa kombinasi `allow-scripts` dan `allow-same-origin` sangat berbahaya?

3. Sebutkan dua nilai sandbox yang diperlukan agar embed YouTube berfungsi dengan baik!

<details><summary>Jawaban</summary>

1. Semua pembatasan diberlakukan: script tidak bisa dijalankan, form tidak bisa dikirim, tidak ada akses ke parent document, popup tidak diizinkan, navigasi top-level diblokir, plugin dinonaktifkan. Ini adalah mode sandbox paling ketat.

2. Karena iframe yang memiliki `allow-scripts` dan `allow-same-origin` bisa menjalankan kode JavaScript yang menghapus atribut `sandbox` dari elemen iframe-nya sendiri, sehingga semua pembatasan hilang. Ibaratnya, penghuni rumah kaca tiba-tiba bisa menghancurkan kacanya dan masuk ke rumah utama.

3. `allow-scripts` (untuk menjalankan player JavaScript YouTube) dan `allow-same-origin` (untuk mengakses API YouTube). Beberapa kasus juga membutuhkan `allow-popups` untuk link yang membuka tab baru.

</details>
