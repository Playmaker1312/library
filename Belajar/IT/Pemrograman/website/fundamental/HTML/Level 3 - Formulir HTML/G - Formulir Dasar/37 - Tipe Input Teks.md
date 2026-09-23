# Tipe Input Teks: `text`, `email`, `password`, `tel`, `url`

## Penjelasan

HTML menyediakan beberapa variasi input teks melalui atribut `type`. Masing-masing memiliki validasi dan perilaku bawaan yang berbeda. Semua tipe ini menerima input berupa teks dari pengguna, tapi dengan aturan dan tujuan yang spesifik.

## Fungsi

| Tipe | Fungsi |
|------|--------|
| `text` | Input teks biasa, tanpa validasi khusus |
| `email` | Menerima alamat email, validasi otomatis (harus mengandung `@`) |
| `password` | Teks disembunyikan (bintang/bulet), tidak boleh ditampilkan |
| `tel` | Nomor telepon — tidak ada validasi otomatis, tapi memicu keypad numerik di HP |
| `url` | Alamat URL — validasi otomatis (harus diawali `http://` atau `https://`) |

## Cara Pengimplementasian

```html
<label for="nama">Nama Lengkap:</label>
<input type="text" id="nama" name="nama" placeholder="Masukkan nama">

<label for="email">Email:</label>
<input type="email" id="email" name="email" placeholder="contoh@email.com">

<label for="pass">Password:</label>
<input type="password" id="pass" name="pass">

<label for="telp">No. Telepon:</label>
<input type="tel" id="telp" name="telp" placeholder="0812-xxxx-xxxx">

<label for="web">Website:</label>
<input type="url" id="web" name="web" placeholder="https://contoh.com">
```

## Analogi (tema RUMAH/BANGUNAN)

Setiap tipe input seperti **berbagai jenis kotak surat** di depan rumah:

- `text` — kotak surat biasa, semua jenis surat bisa masuk.
- `email` — kotak surat khusus yang **hanya menerima amplop bertanda @** — surat tanpa @ akan ditolak.
- `password` — kotak surat **tertutup kain** — isinya tidak terlihat dari luar.
- `tel` — kotak surat dengan **lubang berbentuk khusus untuk kertas lipat** (nomor telepon).
- `url` — kotak surat yang **hanya menerima surat tercatat resmi** (harus alamat lengkap diawali http/https).

## Dipakai Untuk

- `text` — nama, alamat, subjek, komentar singkat.
- `email` — field login email, pendaftaran, newsletter.
- `password` — form login dan registrasi.
- `tel` — nomor HP/kantor, kontak darurat.
- `url` — input alamat website, portofolio, media sosial.

## Kesalahan Umum

- Menggunakan `type="text"` untuk password — password akan terlihat jelas.
- Menganggap `type="email"` memvalidasi 100% — hanya memeriksa keberadaan `@`.
- Lupa `type="tel"` di mobile — pengguna harus beralih keypad manual.
- Tidak menyertakan `placeholder` untuk memberi petunjuk format input.
- Menggunakan `type="password"` tanpa HTTPS — data tetap terkirim tidak aman.

## Koneksi dengan Materi Sebelumnya

Setelah memahami `<input>` secara umum (materi 36), sekarang kita bedah variasi `type` untuk teks. Ini seperti mempelajari berbagai model pintu setelah tahu apa itu pintu.

## Soal Latihan

1. Tipe input apa yang otomatis menyembunyikan karakter yang diketik?
2. Apa perbedaan `type="email"` dan `type="text"`?
3. Tipe input apa yang paling cocok untuk field "Nomor HP" di halaman mobile?

<details><summary>Jawaban</summary>
1. `type="password"`<br>
2. `type="email"` melakukan validasi bawaan (memastikan ada `@`), sedangkan `type="text"` tidak ada validasi sama sekali.<br>
3. `type="tel"`, karena di perangkat mobile akan memunculkan keypad numerik.
</details>
