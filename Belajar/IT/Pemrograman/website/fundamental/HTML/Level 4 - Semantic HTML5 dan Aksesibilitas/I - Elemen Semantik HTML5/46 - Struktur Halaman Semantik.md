# Struktur Halaman Semantik (`<header>`, `<nav>`, `<main>`, `<footer>`)

## Penjelasan

HTML5 menyediakan elemen semantik yang mendeskripsikan **peran** dari setiap bagian halaman, bukan hanya tampilannya. Elemen-elemen ini membantu browser, mesin pencari, dan *screen reader* memahami struktur dokumen.

## Fungsi

- **`<header>`** — Menampung konten pengantar atau navigasi pada bagian atas halaman/seksi.
- **`<nav>`** — Membungkus tautan navigasi utama.
- **`<main>`** — Konten unik dan **hanya boleh SATU** per halaman.
- **`<footer>`** — Informasi kaki halaman seperti hak cipta, tautan terkait.

## Cara Pengimplementasian

```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Contoh Struktur Semantik</title>
</head>
<body>

  <header>
    <h1>Website Saya</h1>
    <nav>
      <ul>
        <li><a href="#beranda">Beranda</a></li>
        <li><a href="#tentang">Tentang</a></li>
        <li><a href="#kontak">Kontak</a></li>
      </ul>
    </nav>
  </header>

  <!-- main hanya SATU -->
  <main>
    <article>
      <h2>Artikel Utama</h2>
      <p>Ini adalah konten utama halaman.</p>
    </article>
  </main>

  <footer>
    <p>&copy; 2026 Website Saya</p>
  </footer>

</body>
</html>
```

## Analogi (tema RUMAH/BANGUNAN)

Bayangkan sebuah **rumah**:

- **`<header>`** → Teras depan dan papan nama rumah. Menyambut pengunjung sebelum masuk.
- **`<nav>`** → Papan petunjuk arah di dalam rumah: "Kamar Tamu →" "Dapur ←".
- **`<main>`** → Ruang keluarga. **Hanya ada satu** ruang keluarga utama dalam satu rumah.
- **`<footer>`** → Halaman belakang atau fondasi rumah — informasi tambahan seperti nomor rumah, kotak pos.

## Dipakai Untuk

- **`<header>`** — Logo, judul situs, navigasi, breadcrumb.
- **`<nav>`** — Menu utama, daftar tautan navigasi.
- **`<main>`** — Konten inti yang tidak diulang di halaman lain.
- **`<footer>`** — Copyright, tautan media sosial, sitemap.

## Kesalahan Umum

1. **Meletakkan `<main>` di luar `<body>` langsung** — `<main>` harus menjadi anak langsung `<body>`.
2. **Menggunakan lebih dari satu `<main>`** — Melanggar spesifikasi; hanya boleh satu.
3. **Memasukkan `<nav>` ke dalam `<header>` secara berlebihan** — Tidak semua navigasi wajib di dalam `<header>`; navigasi sekunder bisa di tempat lain.
4. **Menaruh konten global (sidebar, copyright) di dalam `<main>`** — `<main>` hanya untuk konten unik halaman.

## Koneksi dengan Materi Sebelumnya

Sebelum HTML5, developer menggunakan `<div id="header">`, `<div id="nav">`, `<div id="content">` — semua `<div>` tanpa makna. Elemen semantik menggantikan `<div>` dengan tag yang bermakna, meningkatkan aksesibilitas dan SEO.

## Soal Latihan

1. Manakah dari berikut ini yang **tidak boleh** lebih dari satu dalam satu halaman HTML?
   a) `<header>`
   b) `<nav>`
   c) `<main>`
   d) `<footer>`

2. Di manakah tempat yang tepat untuk meletakkan `<main>`?

3. Sebutkan tiga elemen yang umum diletakkan di dalam `<header>`.

<details><summary>Jawaban</summary>

1. **c) `<main>`** — Hanya boleh satu `<main>` per halaman.
2. Sebagai anak langsung dari `<body>`, setelah elemen seperti `<header>` dan sebelum `<footer>`.
3. Logo/judul situs, navigasi utama (`<nav>`), dan breadcrumb.

</details>
