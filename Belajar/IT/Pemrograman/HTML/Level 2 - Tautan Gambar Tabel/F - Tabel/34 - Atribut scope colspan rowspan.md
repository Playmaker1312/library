# Atribut scope colspan rowspan

## Penjelasan

`scope`, `colspan`, dan `rowspan` adalah atribut pada `<th>` dan `<td>` yang mengontrol hubungan dan ukuran sel tabel.

- **`scope`** — menentukan apakah `<th>` berlaku sebagai header untuk kolom atau baris
- **`colspan`** — menggabungkan sel secara horizontal (melebar ke kanan)
- **`rowspan`** — menggabungkan sel secara vertikal (melebar ke bawah)

## Fungsi

- `scope="col"` — sel header berlaku untuk semua sel dalam kolom yang sama
- `scope="row"` — sel header berlaku untuk semua sel dalam baris yang sama
- `colspan="n"` — sel membentang sejauh `n` kolom
- `rowspan="n"` — sel membentang sejauh `n` baris

## Cara Pengimplementasian

```html
<table>
  <caption>Renovasi Gedung</caption>
  <thead>
    <tr>
      <th scope="col">Lantai</th>
      <th scope="col">Ruangan</th>
      <th scope="col">Biaya</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row" rowspan="2">Lt 1</th>
      <td>Aula</td>
      <td>Rp50jt</td>
    </tr>
    <tr>
      <td>Gudang</td>
      <td>Rp10jt</td>
    </tr>
    <tr>
      <th scope="row">Lt 2</th>
      <td>Ruang Meeting</td>
      <td>Rp30jt</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td colspan="2">Total Biaya</td>
      <td>Rp90jt</td>
    </tr>
  </tfoot>
</table>
```

## Analogi

Masih ingat **denah gedung**? 
- `colspan` seperti **ruangan yang memakan dua kolom** — misalnya aula besar yang melintang melewati batas dua kolom ruangan.
- `rowspan` seperti **ruangan dua lantai (mezzanine)** — area yang membentang dari lantai 1 ke lantai 2.
- `scope` adalah **papan petunjuk**: *scope="col"* artinya "label ini menjelaskan semua sel di kolom ini", *scope="row"* artinya "label ini menjelaskan semua sel di baris ini". Seperti menempelkan label "Lantai 1" di awal baris, yang berarti semua ruangan di baris itu ada di lantai 1.

## Dipakai Untuk

- Header yang membentang beberapa kolom/baris
- Sel data yang mencakup area lebih dari satu baris/kolom (misal: total, kategori gabungan)
- Meningkatkan aksesibilitas — *screen reader* membaca atribut `scope` untuk memahami hubungan header-data

## Kesalahan Umum

- Lupa menyesuaikan jumlah kolom setelah pakai `colspan` — selanjutnya jadi tidak rapi
- Menggunakan `rowspan` terlalu dalam sehingga tabel sulit dibaca
- Tidak memakai `scope` pada `<th>` yang ambigu — misal tabel dengan header di baris dan kolom sekaligus
- Menghitung `colspan`/`rowspan` melebihi jumlah kolom/baris yang tersedia — tabel jadi berantakan

## Koneksi dengan Materi Sebelumnya

Di file 32 kamu belajar sel individual, di file 33 kamu belajar mengelompokkan baris. Sekarang kamu belajar menggabungkan sel (`colspan`/`rowspan`) dan memperjelas hubungan header-data (`scope`). Ini adalah keterampilan terakhir untuk membuat tabel HTML yang semantik, aksesibel, dan rapi.

## Soal Latihan

1. Apa perbedaan `colspan` dan `rowspan`?
2. Kapan sebaiknya menggunakan `scope="row"`?
3. Dalam kode di atas, mengapa `<td>` di dalam `<tfoot>` memiliki `colspan="2"`?

<details><summary>Jawaban</summary>
1. `colspan` menggabungkan sel ke samping (horizontal), `rowspan` menggabungkan sel ke bawah (vertikal).  
2. Saat `<th>` berada di awal baris dan berlaku sebagai label untuk semua sel di baris tersebut (contoh: nama kategori di kolom pertama).  
3. Karena kolom pertama di baris footer ingin mencakup dua kolom (kolom "Lantai" dan "Ruangan") agar teks "Total Biaya" tidak terbelah, sementara kolom ketiga tetap menampilkan jumlah biaya.
</details>
