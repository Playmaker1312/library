# Kapan Gunakan Tabel

## Penjelasan

Tabel HTML digunakan untuk menyajikan data yang bersifat baris dan kolom — data tabular. Contoh data tabular: jadwal pelajaran, daftar harga, laporan keuangan, perbandingan produk, dan hasil survei. Tabel **bukan** alat untuk mengatur tata letak (layout) halaman.

## Fungsi

- Menampilkan data yang memiliki hubungan antar baris dan kolom
- Membantu pengguna membandingkan informasi secara terstruktur
- Meningkatkan aksesibilitas bagi pengguna *screen reader* jika digunakan dengan benar

## Cara Pengimplementasian

```html
<table>
  <caption>Daftar Harga Tiket</caption>
  <tr>
    <th>Jenis</th>
    <th>Harga</th>
  </tr>
  <tr>
    <td>Dewasa</td>
    <td>Rp50.000</td>
  </tr>
  <tr>
    <td>Anak-anak</td>
    <td>Rp25.000</td>
  </tr>
</table>
```

## Analogi

Tabel itu seperti **denah ruangan pada sebuah bangunan**. Setiap baris adalah lantai, setiap kolom adalah ruangan. Kamu bisa melihat isi setiap ruangan dengan cepat karena semuanya tersusun rapi dalam kotak-kotak. Denah rumah tidak pernah digunakan sebagai fondasi — itu tugas beton (CSS). Begitu pula tabel, jangan dipakai untuk membangun layout halaman.

## Dipakai Untuk

- Data keuangan, statistik, jadwal
- Matriks perbandingan
- Hasil olah data (scores, rapor, rekap)
- Kalender atau jadwal kegiatan

## Kesalahan Umum

- Menggunakan tabel untuk layout halaman (misal: membagi halaman jadi sidebar + konten). Ini menyulitkan *screen reader* dan tidak responsif.
- Menyusun tabel terlalu dalam (*nested table*) sehingga kode sulit dibaca.
- Tidak menggunakan `<caption>` sehingga pengguna alat bantu kehilangan konteks.

## Koneksi dengan Materi Sebelumnya

Di Level 1 kamu belajar heading, paragraf, dan teks. Tabel adalah bentuk penyajian data yang lebih terstruktur dari paragraf biasa. Nanti di CSS kamu akan belajar mempercantik tabel dengan border, padding, dan warna.

## Soal Latihan

1. Sebutkan tiga contoh data yang cocok ditampilkan dalam tabel.
2. Mengapa tabel tidak boleh digunakan untuk layout halaman?
3. Apa keuntungan menggunakan `<caption>` dalam tabel?

<details><summary>Jawaban</summary>
1. Jadwal pelajaran, daftar harga, laporan keuangan.  
2. Karena menyulitkan screen reader, tidak responsif, dan melanggar prinsip pemisahan konten (HTML) dan tampilan (CSS).  
3. Memberikan judul/deskripsi tabel yang dibacakan screen reader, sehingga pengguna tahu konteks data sebelum membaca isinya.
</details>
