# minlength, maxlength, min, max

## Penjelasan

Atribut yang membatasi panjang atau rentang nilai input. `minlength`/`maxlength` bekerja pada teks (type text, password, search, dll), sedangkan `min`/`max` bekerja pada angka (type number, date, range, dll).

## Fungsi

- **`minlength`** — Jumlah karakter minimum yang harus diketik (input teks).
- **`maxlength`** — Jumlah karakter maksimum yang boleh diketik (input teks). Browser akan mencegah input melebihi batas.
- **`min`** — Nilai minimum yang dapat dipilih/diisi (input angka/tanggal).
- **`max`** — Nilai maksimum yang dapat dipilih/diisi (input angka/tanggal).

## Cara Pengimplementasian

```html
<form>
  <!-- Batasan teks -->
  <label for="username">Username (6-20 karakter):</label>
  <input type="text" id="username" name="username"
         minlength="6" maxlength="20" required>

  <!-- Batasan angka -->
  <label for="usia">Usia (17-100):</label>
  <input type="number" id="usia" name="usia"
         min="17" max="100" required>

  <!-- Tanggal -->
  <label for="tgl_lahir">Tanggal Lahir:</label>
  <input type="date" id="tgl_lahir" name="tgl_lahir"
         min="1900-01-01" max="2010-12-31">

  <button type="submit">Kirim</button>
</form>
```

## Analogi (RUMAH/BANGUNAN)

- **`minlength`** = Pintu rumah minimal memiliki 1 engsel. Kurang dari itu tidak aman.
- **`maxlength`** = Ukuran maksimal jendela — tidak boleh lebih besar dari bingkai yang tersedia.
- **`min`** = Tinggi plafon minimal 2,5 meter. Tidak boleh lebih rendah.
- **`max`** = Lebar garasi maksimal 3 mobil. Lebih dari itu tidak muat di lahan.

## Dipakai Untuk

- `minlength`/`maxlength` — Username, password, kode pos, komentar pendek, judul.
- `min`/`max` — Usia, jumlah barang, harga, tanggal lahir, rating bintang.

## Kesalahan Umum

1. **`maxlength` mencegah input tapi tidak memberi tahu pengguna** — Tidak ada pesan error native. Gunakan JavaScript jika ingin menampilkan sisa karakter.
2. **`minlength` tidak bekerja pada `type="number"`** — `minlength`/`maxlength` hanya untuk tipe teks. Untuk angka, gunakan `min`/`max`.
3. **Mengabaikan validasi server-side** — Atribut ini hanya validasi client. User bisa mematikan JavaScript atau mengirim request langsung. Tetap validasi di server.
4. **`min`/`max` pada `type="range"` tidak mencegah input** — Pengguna tetap bisa menggeser slider, tapi nilai akan dikunci di luar rentang.

## Koneksi dengan Materi Sebelumnya

- **`required`** — Sering dipasangkan dengan `minlength`/`maxlength` untuk field wajib dengan batas panjang.
- **`pattern`** — Alternatif regex untuk validasi teks; `minlength`/`maxlength` lebih sederhana untuk batas panjang.
- **Tipe input `number`** — Hanya `min`/`max` yang berlaku, bukan `minlength`/`maxlength`.

## Soal Latihan

1. Buat input password dengan panjang minimal 8 dan maksimal 32 karakter, wajib diisi.
2. Buat input jumlah kamar tidur dengan nilai minimal 0 dan maksimal 10.
3. Sebutkan satu situasi di mana `maxlength` bisa mengecoh pengembang jika hanya mengandalkannya untuk keamanan.

<details><summary>Jawaban</summary>

1.
```html
<label for="pass">Password:</label>
<input type="password" id="pass" name="pass"
       minlength="8" maxlength="32" required>
```

2.
```html
<label for="kamar">Jumlah Kamar Tidur:</label>
<input type="number" id="kamar" name="kamar"
       min="0" max="10" value="1">
```

3. `maxlength` hanya validasi **client-side**. Seorang attacker bisa mengirim request POST langsung ke server (pakai curl/Postman) dengan nilai melebihi `maxlength`. Selalu validasi ulang panjang data di server (misal dengan PHP `strlen()` atau Express `validator`).

</details>
