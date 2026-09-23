# required, placeholder, autocomplete

## Penjelasan

Tiga atribut HTML yang mengontrol bagaimana pengguna berinteraksi dengan formulir: `required` memaksa input diisi, `placeholder` memberi petunjuk singkat di dalam kolom, dan `autocomplete` membantu browser mengisi otomatis data yang pernah disimpan.

## Fungsi

- **`required`** — Mencegah form dikirim jika input kosong. Validasi bawaan browser.
- **`placeholder`** — Menampilkan teks tipis di dalam input sebagai petunjuk. Hilang saat user mengetik.
- **`autocomplete`** — Memberi tahu browser apakah boleh menyarankan nilai yang sudah pernah dimasukkan sebelumnya (misal nama, email, alamat).

## Cara Pengimplementasian

```html
<form>
  <label for="nama">Nama Lengkap:</label>
  <input type="text" id="nama" name="nama"
         required
         placeholder="Contoh: Budi Santoso"
         autocomplete="name">

  <label for="email">Email:</label>
  <input type="email" id="email" name="email"
         required
         placeholder="contoh@email.com"
         autocomplete="email">

  <button type="submit">Kirim</button>
</form>
```

Nilai `autocomplete` yang umum: `"name"`, `"email"`, `"tel"`, `"street-address"`, `"country"`, `"postal-code"`.

## Analogi (RUMAH/BANGUNAN)

- **`required`** = Pintu utama rumah **harus** ada. Rumah tanpa pintu utama tidak layak huni — form tanpa `required` mungkin kehilangan data penting.
- **`placeholder`** = Stiker kecil di sakelar lampu bertuliskan "Tekan untuk terang". Petunjuk sementara yang tidak menggantikan label permanen.
- **`autocomplete`** = Lemari arsip di ruang tamu yang otomatis mengambil dokumen yang sering dipakai. Browser "mengingat" kebiasaan penghuni.

## Dipakai Untuk

- `required` — Field wajib seperti nama, email, password, nomor telepon.
- `placeholder` — Contoh format input (misal `"YYYY-MM-DD"` atau `"08xx-xxxx-xxxx"`).
- `autocomplete` — Form registrasi, checkout, login agar pengguna tidak mengetik ulang.

## Kesalahan Umum

1. **Mengganti label dengan `placeholder`** — Placeholder hilang saat mengetik, menyulitkan aksesibilitas. Label harus tetap eksplisit.
2. **`required` tanpa pesan error kustom** — Pesan default browser berbeda-beda. Gunakan `setCustomValidity()` atau CSS `:invalid` untuk pengalaman konsisten.
3. **`autocomplete="off"` berlebihan** — Browser modern sering mengabaikan `off` pada field login/password demi keamanan. Gunakan `"new-password"` untuk field password baru.

## Koneksi dengan Materi Sebelumnya

- **Tipe input** (`email`, `url`, dll) bekerja bersama `required` — browser juga memvalidasi format.
- **Elemen `<label>`** — Wajib digunakan dengan `placeholder` karena placeholder bukan pengganti label.
- **Atribut `name`** — `autocomplete` hanya berfungsi jika `name` cocok dengan nilai yang dikenali browser.

## Soal Latihan

1. Tulis kode input nomor telepon yang: wajib diisi, punya placeholder `"08xx-xxxx-xxxx"`, dan mendukung autocomplete telepon.
2. Jelaskan perbedaan perilaku `autocomplete="off"` pada field password di Chrome vs Firefox.
3. Mengapa `placeholder` tidak boleh menjadi satu-satunya petunjuk untuk pengguna? Sebutkan dua alasan.

<details><summary>Jawaban</summary>

1.
```html
<input type="tel" id="telp" name="telp"
       required
       placeholder="08xx-xxxx-xxxx"
       autocomplete="tel">
```

2. Chrome dan Firefox umumnya **mengabaikan** `autocomplete="off"` pada field password demi mencegah password manager berhenti bekerja. Alternatif yang disarankan: gunakan `autocomplete="new-password"` jika field memang untuk password baru.

3. (a) Placeholder hilang saat pengguna mulai mengetik, sehingga petunjuk tidak lagi terlihat. (b) Pengguna screen reader tidak selalu membaca placeholder — label `<label>` eksplisit wajib untuk aksesibilitas.

</details>
