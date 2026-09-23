# thead tbody tfoot

## Penjelasan

`<thead>`, `<tbody>`, dan `<tfoot>` adalah elemen semantik yang mengelompokkan baris tabel berdasarkan perannya: header, data, dan footer. Mereka tidak mengubah tampilan, tapi membuat struktur tabel lebih jelas bagi browser, CSS, dan *screen reader*.

## Fungsi

- `<thead>` — membungkus satu atau lebih baris header (biasanya berisi `<th>`)
- `<tbody>` — membungkus baris-baris data utama
- `<tfoot>` — membungkus baris footer (ringkasan, total, catatan)
- Browser boleh merender `<tfoot>` di akhir tabel meskipun ditulis sebelum `<tbody>` dalam kode

## Cara Pengimplementasian

```html
<table>
  <caption>Laporan Keuangan Bulanan</caption>
  <thead>
    <tr>
      <th>Bulan</th>
      <th>Pemasukan</th>
      <th>Pengeluaran</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Januari</td>
      <td>Rp10.000.000</td>
      <td>Rp7.000.000</td>
    </tr>
    <tr>
      <td>Februari</td>
      <td>Rp12.000.000</td>
      <td>Rp8.000.000</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td>Total</td>
      <td>Rp22.000.000</td>
      <td>Rp15.000.000</td>
    </tr>
  </tfoot>
</table>
```

## Analogi

Anggap tabel adalah **struktur gedung bertingkat**:
- `<thead>` adalah **atap/panel depan** — label nama lantai dan ruangan
- `<tbody>` adalah **semua lantai isi** — ruangan-ruangan dengan isinya
- `<tfoot>` adalah **pondasi atau ringkasan di dasar gedung** — total keseluruhan

Dengan memisahkan ketiganya, tukang (browser/CSS) tahu mana yang harus diulang jika tabel di-*print* halaman banyak, mana yang di-scroll, dan mana yang tetap.

## Dipakai Untuk

- Tabel dengan banyak baris data yang perlu dikelompokkan
- Laporan keuangan atau statistik yang punya total/ringkasan
- Tabel besar yang di-scroll — `tbody` bisa di-scroll terpisah dengan CSS
- Cetak (*print*) — `thead` dan `tfoot` bisa diulang otomatis di setiap halaman

## Kesalahan Umum

- Menaruh `<tfoot>` di dalam `<tbody>` — harus terpisah
- Memiliki lebih dari satu `<thead>` atau `<tfoot>` dalam satu tabel (hanya boleh satu masing-masing)
- Tidak menggunakan `<thead>` padahal tabel punya header — menyulitkan aksesibilitas
- Menganggap `<tbody>` wajib — padahal browser otomatis membuatnya jika tidak ditulis, tapi lebih baik ditulis eksplisit

## Koneksi dengan Materi Sebelumnya

Setelah menguasai `<tr>`, `<th>`, `<td>` dasar (file 32), sekarang kamu bisa mengelompokkan baris. Ini penting sebelum belajar `colspan` dan `rowspan` (file 34), karena pengelompokan membantu saat sel menggabungkan area dari beberapa baris/kolom.

## Soal Latihan

1. Sebutkan tiga elemen semantik pengelompokan baris tabel.
2. Apa keuntungan menggunakan `<thead>` dan `<tfoot>` saat mencetak tabel?
3. Berapa banyak `<tbody>` yang boleh ada dalam satu tabel?

<details><summary>Jawaban</summary>
1. `<thead>`, `<tbody>`, `<tfoot>`.  
2. Browser bisa mengulang `<thead>` dan `<tfoot>` di setiap halaman cetak, sehingga header dan ringkasan selalu terlihat.  
3. Boleh lebih dari satu `<tbody>` jika ingin mengelompokkan beberapa bagian data terpisah. Tapi `<thead>` dan `<tfoot>` hanya boleh satu.
</details>
