# Tipe Input Lanjutan: range, color, search & datalist

## Penjelasan

HTML5 memperkenalkan beberapa tipe input baru yang memudahkan pengguna memasukkan data dengan cara yang lebih interaktif. **`range`** menyediakan slider untuk memilih angka dalam rentang tertentu. **`color`** menampilkan color picker untuk memilih warna. **`search`** adalah input teks yang dioptimalkan untuk pencarian. **`<datalist>`** menyediakan daftar saran otomatis (autocomplete) yang bisa dipilih pengguna saat mengetik di input.

## Fungsi

| Tipe Input | Fungsi |
|---|---|
| `type="range"` | Memilih angka dalam rentang tertentu menggunakan slider |
| `<output>` | Menampilkan hasil kalkulasi atau nilai dari input lain |
| `type="color"` | Memilih warna melalui color picker |
| `type="search"` | Input teks khusus untuk pencarian |
| `<datalist>` | Memberikan saran opsi saat pengguna mengetik |

## Cara Pengimplementasian

```html
<!-- RANGE + OUTPUT -->
<form oninput="hasil.value= parseInt(volume.value) + parseInt(ukuran.value)">
  <label>Volume: <input type="range" name="volume" min="0" max="100" value="50"></label>
  <label>Ukuran: <input type="range" name="ukuran" min="0" max="50" value="25"></label>
  <p>Total: <output name="hasil">75</output></p>
</form>

<!-- COLOR PICKER -->
<label>Pilih Warna Cat: <input type="color" name="warnaCat" value="#ff6600"></label>

<!-- SEARCH -->
<label>Cari: <input type="search" name="cari" placeholder="Ketik kata kunci..."></label>

<!-- DATALIST -->
<label>Pilih Tipe Rumah:
  <input type="text" name="tipeRumah" list="daftarTipe">
</label>
<datalist id="daftarTipe">
  <option value="Rumah Minimalis">
  <option value="Rumah Tradisional">
  <option value="Rumah Modern">
  <option value="Rumah Tropis">
</datalist>
```

## Analogi (Tema RUMAH/BANGUNAN)

Bayangkan Anda sedang **membangun rumah** dan berdiskusi dengan kontraktor:

- **`type="range"`** → Seperti **pengatur suhu AC** (slider). Anda menggeser ke kiri untuk lebih dingin, ke kanan untuk lebih panas. Nilainya bertahap, tidak perlu angka pasti.
- **`<output>`** → Seperti **monitor panel listrik** yang menampilkan total pemakaian daya secara otomatis. Ia menunjukkan hasil kalkulasi dari berbagai input.
- **`type="color"`** → Seperti **katalog cat tembok** di toko material. Anda bisa melihat dan memilih warna cat secara visual, bukan dengan menuliskan nama warna.
- **`type="search"`** → Seperti **papan pencarian** di gudang material. Begitu Anda mulai mengetik nama material, hasil yang relevan langsung muncul.
- **`<datalist>`** → Seperti **kotak saran dari tukang bangunan**: "Apakah maksud Anda bata merah, bata ringan, atau batako?" — saran muncul saat Anda mulai mengetik.

## Dipakai Untuk

- **Range**: Pengaturan volume, kecerahan, ukuran font, filter harga (e-commerce), kontrol media player.
- **Output**: Menampilkan total harga otomatis, hasil konversi satuan, kalkulator sederhana.
- **Color**: Pemilih warna cat/tema, editor grafis online, pengaturan warna latar/kustomisasi UI.
- **Search**: Kotak pencarian di website, filter data, pencarian produk.
- **Datalist**: Input provinsi/kota, merek mobil, jenis properti — data yang memiliki banyak opsi standar.

## Kesalahan Umum

1. **Lupa menambahkan atribut `id` pada `<datalist>`** → Input tidak terhubung ke datalist. Pastikan atribut `list` pada input sesuai dengan `id` pada `<datalist>`.

2. **Tidak menyertakan atribut `min` dan `max` pada range** → Nilai default range adalah 0–100, tetapi tanpa atribut eksplisit rentang menjadi ambigu. Selalu tetapkan `min` dan `max`.

3. **Menggunakan `<output>` di luar tag `<form>` dengan event `oninput`** → Output hanya akan memperbarui otomatis jika berada di dalam form yang memiliki atribut `oninput` di tag `<form>`.

4. **Mengira `<datalist>` memvalidasi input** → `<datalist>` hanya memberi saran, pengguna tetap bisa mengetik nilai di luar daftar. Untuk validasi, gunakan atribut `required` atau kombinasikan dengan pola regex.

5. **`type="search"` dianggap sama dengan `type="text"`** → Search memiliki perilaku berbeda: di beberapa browser ia menyediakan tombol "x" untuk menghapus dan dapat menyimpan riwayat pencarian.

## Koneksi dengan Materi Sebelumnya

- **Level 1 (Heading, Paragraph)**: Input search dan datalist sering digunakan bersama teks dan heading untuk membuat halaman pencarian sederhana.
- **Level 2 (Tabel)**: Data dari range dan color dapat ditampilkan dalam tabel untuk keperluan konfigurasi.
- **Level 3 (Hyperlink & Gambar)**: Color picker bisa digunakan untuk memilih warna border gambar.
- **Level 4 (Form Dasar)**: Range, color, search, dan datalist adalah pengembangan dari input tipe teks dan number yang sudah dipelajari. Jika di form dasar Anda hanya mengenal `type="text"` dan `type="number"`, sekarang Anda punya alat yang lebih spesifik.

## Soal Latihan

1. Buatlah sebuah slider range dengan nilai minimum 0 dan maksimum 100, dan tampilkan nilainya secara real-time menggunakan tag `<output>`.

2. Buatlah input warna dengan nilai default merah (#ff0000) dan sebuah datalist yang berisi 4 pilihan tipe atap rumah (Atap Genteng, Atap Seng, Atap Metal, Atap Daun).

3. Jelaskan perbedaan antara `<datalist>` dan `<select>` dalam konteks pemilihan tipe rumah.

4. Buatlah sebuah form pencarian sederhana dengan tipe `search` yang terhubung ke datalist berisi 3 kata kunci populer.

<details><summary>Jawaban</summary>

**1. Range + Output:**
```html
<form oninput="nilaiRange.value = rangeSlider.value">
  <input type="range" id="rangeSlider" min="0" max="100" value="50">
  <output id="nilaiRange">50</output>
</form>
```

**2. Color + Datalist:**
```html
<label>Warna Cat: <input type="color" value="#ff0000"></label>

<label>Tipe Atap: <input list="atapList" name="atap"></label>
<datalist id="atapList">
  <option value="Atap Genteng">
  <option value="Atap Seng">
  <option value="Atap Metal">
  <option value="Atap Daun">
</datalist>
```

**3. Perbedaan `<datalist>` dan `<select>`:**
- **`<datalist>`** bersifat sebagai saran — pengguna bisa memilih dari daftar ATAU mengetik nilai bebas. Cocok untuk input yang punya banyak opsi standar tetapi tetap perlu fleksibilitas (misal: kota tempat tinggal).
- **`<select>`** bersifat kaku — pengguna hanya bisa memilih dari opsi yang tersedia, tidak bisa mengetik nilai sendiri. Cocok untuk pilihan yang terbatas dan pasti (misal: tipe kelamin, status pernikahan).

Analogi rumah: `<datalist>` seperti seorang asisten yang bilang "beberapa orang memilih cat warna biru atau hijau" — tapi Anda tetap bebas pilih warna ungu. `<select>` seperti katalog cat yang hanya menyediakan warna-warna tertentu — Anda tidak bisa memilih di luar itu.

**4. Form pencarian dengan search + datalist:**
```html
<form>
  <input type="search" name="cari" list="populer" placeholder="Cari rumah...">
  <datalist id="populer">
    <option value="Rumah Minimalis">
    <option value="Desain Interior">
    <option value="Harga Tanah">
  </datalist>
  <button type="submit">Cari</button>
</form>
```

</details>
