# Validasi HTML W3C

## Penjelasan
Validasi HTML W3C adalah proses mengecek apakah kode HTML yang kita tulis sesuai dengan standar resmi dari World Wide Web Consortium (W3C). Validasi dilakukan melalui validator.w3.org. Target: **0 error**.

## Fungsi
- Memastikan HTML ditulis sesuai standar internasional
- Menghindari bug rendering di berbagai browser
- Meningkatkan Accessibility dan SEO
- Mempermudah debugging kode

## Cara Pengimplementasian

```html
<!-- Cara 1: Validasi via URL -->
Buka https://validator.w3.org → masukkan URL website → klik Check

<!-- Cara 2: Upload file HTML -->
Buka validator.w3.org → tab "Validate by File Upload" → pilih file

<!-- Cara 3: Copy-paste kode -->
Buka validator.w3.org → tab "Validate by Direct Input" → paste kode
```

```html
<!-- Contoh error umum: heading skip -->
<!-- SALAH: lompat dari h1 ke h3 -->
<h1>Judul</h1>
<h3>Subjudul</h3>

<!-- BENAR: berurutan -->
<h1>Judul</h1>
<h2>Subjudul</h2>
```

```html
<!-- Contoh error umum: alt hilang pada gambar -->
<!-- SALAH -->
<img src="foto.jpg">

<!-- BENAR -->
<img src="foto.jpg" alt="Deskripsi foto">

<!-- Untuk gambar dekoratif -->
<img src="dekorasi.jpg" alt="" role="presentation">
```

## Error Umum yang Sering Muncul

| Error                           | Penyebab                     | Solusi                         |
|----------------------------------|------------------------------|--------------------------------|
| Heading skip                     | h1 ke h3 tanpa h2           | Gunakan heading berurutan       |
| Alt attribute missing            | `<img>` tanpa `alt`          | Tambah `alt=""` minimal         |
| Tag tidak ditutup                | `<p>` tanpa `</p>`           | Tutup semua tag                 |
| Element tidak diizinkan          | `<center>` (deprecated)      | Gunakan CSS                     |
| Duplicate ID                     | Dua elemen `id="nama"`       | Gunakan class atau ID unik      |

## Analogi (tema RUMAH/BANGUNAN)
Validasi W3C adalah **sertifikat kelayakan bangunan dari dinas PUPR**. Sebelum rumah dihuni, harus diperiksa apakah fondasi sesuai standar, apakah tiang kokoh, apakah atap miringnya benar. Validator W3C memeriksa setiap "bata" kode HTML — jika ada bata salah pasang (error), rumah (website) berpotensi ambruk di browser tertentu. Target 0 error = rumah **100% sesuai standar bangunan**.

## Dipakai Untuk
- Semua website yang akan dipublikasikan
- Project sekolah/kuliah yang dinilai
- Website perusahaan yang profesional

## Kesalahan Umum
- Menganggap validasi tidak penting karena website "berfungsi saja"
- Tidak mengecek validasi setelah menambahkan kode baru
- Mengabaikan warning (peringatan) — meski tidak sefatal error, tetap perlu diperbaiki
- Bingung membedakan error dan warning

## Koneksi dengan Materi Sebelumnya
Validasi W3C adalah quality control dari semua materi HTML sebelumnya — dari struktur dasar, semantic HTML, form, gambar, hingga multimedia. Semua yang dipelajari dari Level 1 sampai Level 6 akan diperiksa di sini.

## Soal Latihan
<details><summary>Jawaban</summary>

1. Apa yang dimaksud "heading skip" dan mengapa itu error?
   **Jawaban:** Heading skip adalah lompatan level heading (misal h1 langsung ke h3 tanpa h2). Ini error karena merusak struktur dokumen, membingungkan screen reader, dan melanggar aturan W3C.

2. Apakah gambar dekoratif tetap harus punya `alt`?
   **Jawaban:** Ya, tetapi dengan `alt=""` (string kosong) agar screen reader mengabaikannya. Jangan dihilangkan sama sekali.

3. Sebutkan tiga cara validasi di validator.w3.org!
   **Jawaban:** Via URL, via upload file, via direct input (copy-paste).

</details>
