# Nested List (Daftar Bersarang)

## Penjelasan

Nested list adalah daftar di dalam daftar. Sebuah `<li>` bisa berisi `<ul>` atau `<ol>` lain. Maksimal disarankan 3 level agar mudah dibaca. Struktur ini menciptakan hierarki bertingkat.

## Fungsi

Membuat kategori, subkategori, dan sub-subkategori dalam satu daftar — misalnya daftar isi buku, struktur organisasi, atau silsilah.

## Cara Pengimplementasian

```html
<ul>
  <li>Rumah</li>
  <li>
    Kamar
    <ul>
      <li>Kamar tidur</li>
      <li>
        Kamar mandi
        <ul>
          <li>Shower</li>
          <li>Toilet</li>
        </ul>
      </li>
    </ul>
  </li>
  <li>Dapur</li>
</ul>
```

## Analogi (tema RUMAH/BANGUNAN)

Seperti **denah rumah** — Rumah (level 1) berisi Kamar (level 2), kamar berisi Lemari (level 3). Semakin dalam level, semakin detail bagiannya.

## Dipakai Untuk

- Daftar isi bab, sub-bab
- Menu dropdown bertingkat
- Struktur organisasi
- Sitemap

## Kesalahan Umum

- Level terlalu dalam (>3) sehingga sulit dibaca.
- Salah menempatkan `<ul>` di luar `<li>` — daftar bersarang **harus** berada di dalam `<li>`.
- Lupa menutup tag, menyebabkan struktur kacau.

## Koneksi dengan Materi Sebelumnya

Setelah menguasai `<ul>` dan `<ol>` (datar), nested list memperluasnya menjadi struktur dua atau tiga dimensi dengan prinsip yang sama.

## Soal Latihan

Buat nested list 2 level: "Alat" → ("Palu", "Obeng", "Gergaji") menggunakan `<ul>`.

<details><summary>Jawaban</summary>

```html
<ul>
  <li>
    Alat
    <ul>
      <li>Palu</li>
      <li>Obeng</li>
      <li>Gergaji</li>
    </ul>
  </li>
</ul>
```

</details>
