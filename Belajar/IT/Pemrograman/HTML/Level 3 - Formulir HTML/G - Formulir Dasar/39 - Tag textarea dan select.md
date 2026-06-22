# Tag `<textarea>` dan `<select>`

## Penjelasan

`<textarea>` adalah elemen input teks multi-baris — cocok untuk teks panjang seperti komentar atau alamat. `<select>` membuat dropdown (menu pilihan) yang bisa memuat banyak opsi melalui elemen `<option>` di dalamnya. Keduanya adalah alternatif dari `<input>` untuk kasus tertentu.

## Fungsi

- `<textarea>` — input teks yang bisa menampung banyak baris, bisa di-resize.
- `<select>` — daftar pilihan dropdown yang menghemat ruang halaman.
- `<option>` — setiap item di dalam `<select>`.
- `<optgroup>` — mengelompokkan opsi dalam dropdown.

## Cara Pengimplementasian

```html
<!-- Textarea: teks panjang -->
<label for="alamat">Alamat Lengkap:</label>
<textarea id="alamat" name="alamat" rows="4" cols="40">Jl. Merdeka No. 123</textarea>

<!-- Select: dropdown -->
<label for="kota">Kota:</label>
<select id="kota" name="kota">
  <option value="">-- Pilih Kota --</option>
  <option value="jkt">Jakarta</option>
  <option value="bdg">Bandung</option>
  <option value="sby">Surabaya</option>
</select>

<!-- Select dengan grup -->
<label for="jurusan">Jurusan:</label>
<select id="jurusan" name="jurusan">
  <optgroup label="IPA">
    <option value="fisika">Fisika</option>
    <option value="kimia">Kimia</option>
  </optgroup>
  <optgroup label="IPS">
    <option value="ekonomi">Ekonomi</option>
    <option value="geografi">Geografi</option>
  </optgroup>
</select>
```

## Analogi (tema RUMAH/BANGUNAN)

- **`<textarea>`** seperti **papan tulis besar** di dinding ruang tamu — bisa menulis panjang lebar, bisa dihapus, dan ukurannya bisa diatur (rows & cols = tinggi & lebar papan).
- **`<select>`** seperti **papan nama kamar yang bisa digeser (sliding label)** — Anda tarik tuasnya dan keluar daftar kamar yang tersedia. Hanya satu yang bisa dipilih, dan tidak memakan banyak tempat di dinding.
- **`<optgroup>`** seperti **sekat dalam laci** — memisahkan alat-alat dapur dari alat-alat kebersihan di laci yang sama.

## Dipakai Untuk

- Textarea: form komentar, alamat, deskripsi, pesan, catatan.
- Select: daftar negara, kota, kategori, jurusan, ukuran, warna.

## Kesalahan Umum

- Textarea ditulis sebagai `<input type="textarea">` — ini tidak valid, gunakan `<textarea>`.
- Lupa atribut `name` — data tidak terkirim.
- Textarea tanpa `rows` dan `cols` — ukuran default mungkin terlalu kecil/besar.
- Select tanpa opsi default (`<option value="">Pilih</option>`) — pengguna bingung.
- Tidak menggunakan `<label>` — aksesibilitas buruk.

## Koneksi dengan Materi Sebelumnya

Setelah mengenal `<input>` untuk teks pendek (materi 37), sekarang kita belajar alternatif untuk teks panjang (`textarea`) dan input pilihan terstruktur (`select`). Ketiganya adalah "alat tulis" yang berbeda fungsi di dalam rumah form.

## Soal Latihan

1. Tag apa yang digunakan untuk membuat input teks multi-baris?
2. Bagaimana cara memberi nilai default dalam `<textarea>`?
3. Apa fungsi atribut `rows` dan `cols` pada `<textarea>`?

<details><summary>Jawaban</summary>
1. `<textarea>` (bukan `<input type="textarea">`).<br>
2. Tulis teks di antara tag pembuka dan penutup: `<textarea>teks default</textarea>`.<br>
3. `rows` mengatur tinggi (jumlah baris) dan `cols` mengatur lebar (jumlah karakter per baris).
</details>
