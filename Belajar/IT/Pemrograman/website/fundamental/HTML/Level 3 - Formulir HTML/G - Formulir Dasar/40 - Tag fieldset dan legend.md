# Tag `<fieldset>` dan `<legend>`

## Penjelasan

`<fieldset>` digunakan untuk mengelompokkan elemen-elemen form yang saling terkait ke dalam satu grup visual dan semantik. `<legend>` memberikan judul atau keterangan untuk grup tersebut. Keduanya meningkatkan struktur dan aksesibilitas form, terutama untuk form yang panjang atau kompleks.

## Fungsi

- `<fieldset>` — membungkus sekelompok elemen form dalam batas visual (garis kotak).
- `<legend>` — memberi teks judul di bagian atas fieldset.

## Cara Pengimplementasian

```html
<form>
  <fieldset>
    <legend>Data Pribadi</legend>
    <label for="nama">Nama:</label>
    <input type="text" id="nama" name="nama">
    <br>
    <label for="umur">Umur:</label>
    <input type="number" id="umur" name="umur">
  </fieldset>

  <fieldset>
    <legend>Data Akun</legend>
    <label for="email">Email:</label>
    <input type="email" id="email" name="email">
    <br>
    <label for="pass">Password:</label>
    <input type="password" id="pass" name="pass">
  </fieldset>

  <fieldset>
    <legend>Metode Pembayaran</legend>
    <label><input type="radio" name="bayar" value="transfer"> Transfer Bank</label>
    <label><input type="radio" name="bayar" value="kartu"> Kartu Kredit</label>
  </fieldset>
</form>
```

## Analogi (tema RUMAH/BANGUNAN)

Bayangkan sebuah **rumah besar dengan beberapa kamar**:

- **`<fieldset>`** adalah **dinding dan pintu kamar** — memisahkan ruang tamu, dapur, kamar tidur, dan kamar mandi. Setiap ruangan punya fungsi khusus dan dibatasi oleh dinding.
- **`<legend>`** adalah **papan nama kamar** yang ditempel di pintu — "Kamar Tidur", "Dapur", "Kamar Mandi". Tanpa papan nama, penghuni bingung masuk ke ruangan mana.

Jika form adalah satu rumah utuh, maka setiap fieldset adalah ruangan yang terorganisir rapi dengan fungsinya masing-masing.

## Dipakai Untuk

- Memisahkan bagian form yang panjang (data pribadi, data alamat, data pembayaran).
- Mengelompokkan input radio/checkbox yang berhubungan.
- Meningkatkan aksesibilitas — screen reader membacakan legend terlebih dahulu.

## Kesalahan Umum

- Menggunakan `<fieldset>` tanpa `<legend>` — grup tidak punya keterangan.
- Meletakkan terlalu banyak fieldset sehingga form terlihat terlalu terpotong.
- Fieldset bersarang (fieldset di dalam fieldset) tanpa alasan jelas.
- Lupa menutup tag `</fieldset>` — elemen setelahnya ikut ke dalam grup.

## Koneksi dengan Materi Sebelumnya

Fieldset+legend sering dipakai bersama radio dan checkbox (materi 38) untuk mengelompokkan pilihan. Juga bagian dari struktur form (materi 35) seperti dinding adalah bagian dari struktur rumah.

## Soal Latihan

1. Apa perbedaan tag `<fieldset>` dan `<legend>`?
2. Sebutkan satu situasi di mana `<fieldset>` sangat berguna.
3. Bolehkah kita menggunakan lebih dari satu `<fieldset>` dalam satu form?

<details><summary>Jawaban</summary>
1. `<fieldset>` membungkus dan mengelompokkan elemen form, `<legend>` memberi judul pada grup tersebut.<br>
2. Form registrasi panjang dengan bagian "Data Pribadi", "Data Akun", dan "Alamat" — masing-masing bisa di-fieldset terpisah.<br>
3. Boleh, satu form bisa memiliki banyak fieldset untuk memisahkan bagian-bagian yang berbeda.
</details>
