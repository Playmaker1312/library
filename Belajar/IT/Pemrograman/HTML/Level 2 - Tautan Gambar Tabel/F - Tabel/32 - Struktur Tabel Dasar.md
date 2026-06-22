# Struktur Tabel Dasar

## Penjelasan

Tabel HTML dasar dibangun dengan empat elemen utama: `<table>` sebagai wadah, `<tr>` untuk baris, `<th>` untuk sel header, dan `<td>` untuk sel data. `<caption>` memberi judul pada tabel.

## Fungsi

- `<table>` — membungkus seluruh konten tabel
- `<tr>` (table row) — mendefinisikan satu baris
- `<th>` (table header) — sel yang berisi judul kolom/baris (tebal dan居中 secara default)
- `<td>` (table data) — sel yang berisi data biasa
- `<caption>` — judul/deskripsi tabel, ditempatkan langsung setelah `<table>`

## Cara Pengimplementasian

```html
<table>
  <caption>Daftar Kamar Kos</caption>
  <tr>
    <th>No</th>
    <th>Tipe Kamar</th>
    <th>Harga/Bulan</th>
  </tr>
  <tr>
    <td>1</td>
    <td>Standar</td>
    <td>Rp1.000.000</td>
  </tr>
  <tr>
    <td>2</td>
    <td>VIP</td>
    <td>Rp2.500.000</td>
  </tr>
</table>
```

## Analogi

Bayangkan **papan informasi di lobby apartemen**. ` <table>` adalah papan itu sendiri. `<tr>` adalah satu baris daftar penghuni. `<th>` adalah label kolom seperti "Nama", "No Unit", "Status". `<td>` adalah isi datanya. `<caption>` adalah judul di atas papan: "Daftar Penghuni Apartemen".

## Dipakai Untuk

- Semua bentuk data tabular sederhana
- Daftar item dengan beberapa kolom atribut
- Informasi berulang yang perlu dibandingkan baris per baris

## Kesalahan Umum

- Lupa menutup tag `<tr>`, `<th>`, atau `<td>` — bisa merusak struktur seluruh tabel
- Menaruh `<th>` di tengah baris data, bukan di baris header
- Tidak menyertakan `<caption>` sehingga konteks tabel hilang
- Menggunakan `<td>` untuk semua sel termasuk header

## Koneksi dengan Materi Sebelumnya

Di materi **Teks** (Level 1) kamu belajar daftar menggunakan `<ul>`/`<ol>`. Tabel adalah daftar dua dimensi — selain ke bawah (baris), juga ke samping (kolom). Setelah ini, kamu akan belajar `<thead>`, `<tbody>`, dan `<tfoot>` untuk mengelompokkan baris.

## Soal Latihan

1. Apa perbedaan `<th>` dan `<td>`?
2. Buat tabel dengan 3 baris dan 2 kolom tentang 3 jenis kamar hotel.
3. Di mana letak `<caption>` dalam struktur `<table>`?

<details><summary>Jawaban</summary>
1. `<th>` adalah sel header (judul kolom/baris), tampil tebal dan居中. `<td>` adalah sel data biasa.  
2. 
```html
<table>
  <tr><th>Kamar</th><th>Harga</th></tr>
  <tr><td>Standar</td><td>Rp500k</td></tr>
  <tr><td>Deluxe</td><td>Rp1.000k</td></tr>
  <tr><td>Suite</td><td>Rp2.000k</td></tr>
</table>
```
3. Langsung setelah `<table>`, sebelum `<tr>` pertama.
</details>
