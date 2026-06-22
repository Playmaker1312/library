# pattern (Regex)

## Penjelasan

Atribut `pattern` menerima ekspresi reguler (regex) untuk memvalidasi nilai input teks. Jika nilai tidak cocok dengan pola, form tidak bisa dikirim dan browser menampilkan pesan error.

## Fungsi

- Memvalidasi format input di sisi klien tanpa JavaScript.
- Mengurangi request tidak valid ke server.
- Memberi umpan balik langsung ke pengguna.

## Cara Pengimplementasian

```html
<form>
  <!-- Nomor telepon Indonesia -->
  <label for="telepon">Nomor Telepon:</label>
  <input type="tel" id="telepon" name="telepon"
         pattern="08[0-9]{8,11}"
         placeholder="08xxxxxxxxx"
         title="Masukkan 10-13 digit, diawali 08 (contoh: 08123456789)"
         required>

  <!-- Kode Pos Indonesia -->
  <label for="kodepos">Kode Pos:</label>
  <input type="text" id="kodepos" name="kodepos"
         pattern="[1-9][0-9]{4}"
         placeholder="12345"
         title="5 digit angka, tidak diawali 0 (contoh: 40115)"
         required>

  <button type="submit">Kirim</button>
</form>
```

**Penjelasan regex:**

| Regex | Arti |
|-------|------|
| `08[0-9]{8,11}` | "08" diikuti 8-11 digit angka |
| `[1-9][0-9]{4}` | Digit 1-9, lalu 4 digit 0-9 |

## Analogi (RUMAH/BANGUNAN)

`pattern` = Cetak biru (blueprint) rumah. Setiap ruangan harus mengikuti denah yang sudah ditentukan. Jika ukuran kamar tidak sesuai cetak biru, kontraktor (browser) menolak dan meminta diperbaiki. Atribut `title` adalah catatan pinggir di cetak biru yang menjelaskan ukuran yang benar.

## Dipakai Untuk

- Nomor telepon dengan format tertentu.
- Kode pos dengan pola angka spesifik.
- Password dengan syarat huruf besar, kecil, dan angka.
- Plat nomor kendaraan.
- Tanggal dengan format non-standar (misal `MM/DD/YYYY`).

## Kesalahan Umum

1. **Tidak menyertakan atribut `title`** — Pesan error default browser hanya menampilkan pola regex mentah (misal `"Harus cocok dengan [1-9][0-9]{4}"`). `title` memberikan penjelasan yang lebih ramah.
2. **Regex terlalu ketat** — Misal nomor telepon hanya menerima 10 digit, padahal operator Indonesia menyediakan 11-13 digit. Selalu riset format yang valid.
3. **Lupa anchor `^` dan `$`** — Sebenarnya `pattern` otomatis di-anchor (dicocokkan dari awal sampai akhir), jadi `^`/`$` tidak wajib, tapi menambah kejelasan.
4. **Mengandalkan `pattern` saja** — Sama seperti validasi client lainnya, tetap validasi di server.

## Koneksi dengan Materi Sebelumnya

- **`required`** — Sering dipasangkan agar field wajib diisi dengan format benar.
- **`minlength`/`maxlength`** — `pattern` bisa menggantikan keduanya (misal `pattern=".{6,20}"`), tapi kombinasi lebih eksplisit.
- **Tipe input `tel`/`email`** — `pattern` bisa memperketat validasi default browser. Misal `type="email"` menerima `"a@b"`; tambah `pattern="[^@]+@[^@]+\.[^@]{2,}"` untuk lebih ketat.

## Soal Latihan

1. Buat input plat nomor kendaraan Indonesia dengan pola: 1-2 huruf, spasi, 1-4 angka, spasi, 1-3 huruf (contoh: B 1234 ABC).
2. Buat input password yang membutuhkan minimal 1 huruf besar, 1 huruf kecil, 1 angka, dan panjang 8-20 karakter.
3. Jelaskan mengapa atribut `title` penting saat menggunakan `pattern`.

<details><summary>Jawaban</summary>

1.
```html
<label for="plat">Plat Nomor:</label>
<input type="text" id="plat" name="plat"
       pattern="[A-Z]{1,2} \d{1,4} [A-Z]{1,3}"
       placeholder="B 1234 ABC"
       title="Format: 1-2 huruf, spasi, 1-4 angka, spasi, 1-3 huruf"
       required>
```

2.
```html
<label for="pass">Password:</label>
<input type="password" id="pass" name="pass"
       pattern="(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{8,20}"
       title="Minimal 8 karakter, terdiri dari huruf besar, huruf kecil, dan angka"
       required>
```

3. Atribut `title` memberikan **pesan error yang terbaca manusia**. Tanpa `title`, browser menampilkan teks seperti `"Harap cocokkan format yang diminta"` atau langsung menampilkan pola regex (`[A-Z]{1,2} \d{1,4} [A-Z]{1,3}`) yang tidak dipahami pengguna awam. Dengan `title`, pengguna mendapat instruksi yang jelas seperti "Format: 1-2 huruf, spasi, 1-4 angka...".

</details>
