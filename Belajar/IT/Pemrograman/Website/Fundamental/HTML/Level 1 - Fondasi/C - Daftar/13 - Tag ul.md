# Tag `<ul>` (Unordered List)

## Penjelasan

`<ul>` adalah tag untuk membuat daftar yang urutannya **tidak penting**. Setiap item ditulis dengan tag `<li>` (list item). Secara default, browser menampilkannya dengan bullet point (lingkaran hitam).

## Fungsi

Mengelompokkan sekumpulan item yang tidak memerlukan urutan tertentu, seperti daftar belanja, daftar fitur, atau menu navigasi.

## Cara Pengimplementasian

```html
<ul>
  <li>Bata merah</li>
  <li>Pasir</li>
  <li>Semen</li>
</ul>
```

## Analogi (tema RUMAH/BANGUNAN)

Seperti **kotak peralatan tukang** — isinya bisa diambil dalam urutan apa pun, tidak masalah mana yang dipakai duluan. Palu, obeng, paku semuanya setara.

## Dipakai Untuk

- Daftar fitur produk
- Daftar bahan bangunan
- Navigasi menu
- Checklist item

## Kesalahan Umum

- Meletakkan `<li>` di luar `<ul>` → tidak valid secara HTML.
- Menulis teks langsung di dalam `<ul>` tanpa `<li>`.
- Menggunakan `<ul>` untuk daftar yang urutannya penting (harusnya `<ol>`).

## Koneksi dengan Materi Sebelumnya

Setelah mengenal heading (`<h1>`–`<h6>`) dan paragraf (`<p>`), kini kita belajar daftar. Daftar tidak penting (`<ul>`) adalah kebalikan dari daftar penting (`<ol>`).

## Soal Latihan

Buatlah daftar tidak terurut berisi 3 bahan bangunan menggunakan `<ul>` dan `<li>`.

<details><summary>Jawaban</summary>

```html
<ul>
  <li>Batu bata</li>
  <li>Keramik</li>
  <li>Cat tembok</li>
</ul>
```

</details>
