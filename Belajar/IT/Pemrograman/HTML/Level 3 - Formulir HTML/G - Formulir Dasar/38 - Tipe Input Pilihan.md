# Tipe Input Pilihan: `checkbox`, `radio`, `fieldset`, `legend`

## Penjelasan

`checkbox` dan `radio` adalah tipe input yang memungkinkan pengguna memilih dari opsi yang tersedia. Checkbox memungkinkan **banyak pilihan**, radio hanya **satu pilihan** dalam grup yang sama. `<fieldset>` dan `<legend>` digunakan untuk mengelompokkan opsi dan memberi judul grup.

## Fungsi

- `checkbox` — memilih satu atau lebih opsi (seperti daftar centang).
- `radio` — memilih tepat satu opsi dari beberapa pilihan.
- `fieldset` — membungkus sekelompok elemen form dalam satu kotak grup.
- `legend` — memberi judul/keterangan untuk fieldset.

## Cara Pengimplementasian

```html
<!-- Checkbox: bisa pilih banyak -->
<fieldset>
  <legend>Hobi (pilih semua yang sesuai)</legend>
  <label><input type="checkbox" name="hobi" value="membaca"> Membaca</label>
  <label><input type="checkbox" name="hobi" value="menulis"> Menulis</label>
  <label><input type="checkbox" name="hobi" value="coding"> Coding</label>
</fieldset>

<!-- Radio: hanya satu pilihan -->
<fieldset>
  <legend>Jenis Kelamin</legend>
  <label><input type="radio" name="jk" value="laki"> Laki-laki</label>
  <label><input type="radio" name="jk" value="perempuan"> Perempuan</label>
</fieldset>
```

## Analogi (tema RUMAH/BANGUNAN)

Bayangkan Anda sedang **memilih material bangunan** untuk sebuah rumah:

- **Checkbox** seperti **daftar belanja material**: Anda bisa memilih bata, semen, cat, dan paku sekaligus — semua boleh diambil.
- **Radio** seperti **memilih satu jenis atap**: genteng tanah OR seng OR beton — hanya satu yang bisa dipilih.
- **Fieldset** seperti **keranjang/tempat terpisah** untuk setiap kelompok material (kelompok atap, kelompok dinding, dll).
- **Legend** adalah **label keranjang** yang bertuliskan "Material Atap" atau "Material Dinding".

## Dipakai Untuk

- Checkbox: preferensi ganda (hobi, topping pizza, fitur tambahan), persetujuan syarat & ketentuan.
- Radio: pilihan tunggal eksklusif (gender, status, metode pembayaran, rating).
- Fieldset + legend: mengelompokkan bagian-bagian form yang panjang.

## Kesalahan Umum

- Radio tidak pakai `name` yang sama — pengguna bisa memilih lebih dari satu (rusak).
- Checkbox lupa atribut `value` — data yang terkirim hanya "on" bukan nilai yang diinginkan.
- Tidak membungkus radio/checkbox dengan `<label>` — area klik terlalu kecil.
- Fieldset digunakan tanpa legend — grup tidak memiliki keterangan.

## Koneksi dengan Materi Sebelumnya

Ini pengembangan dari `<input>` (materi 36). Sebelumnya input hanya teks, sekarang input berbentuk pilihan. Juga berkaitan dengan `<label>` karena label sangat penting untuk checkbox/radio.

## Soal Latihan

1. Apa yang terjadi jika dua radio button memiliki atribut `name` berbeda?
2. Kapan menggunakan checkbox dan kapan menggunakan radio?
3. Apa fungsi tag `<fieldset>`?

<details><summary>Jawaban</summary>
1. Kedua radio button akan dianggap dari grup berbeda, sehingga pengguna bisa memilih keduanya sekaligus — ini salah untuk kasus pilihan tunggal.<br>
2. Checkbox untuk pilihan yang bisa dipilih lebih dari satu (hobi, topping), radio untuk pilihan yang hanya boleh satu (jenis kelamin, metode bayar).<br>
3. `<fieldset>` mengelompokkan elemen-elemen form yang terkait secara visual dan semantik.
</details>
