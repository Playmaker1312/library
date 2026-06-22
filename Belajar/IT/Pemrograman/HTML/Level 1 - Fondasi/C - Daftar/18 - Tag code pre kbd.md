# Tag `<code>`, `<pre>`, `<kbd>`, `<samp>`

## Penjelasan

- `<code>`: teks kode program (inline), tampil dengan font monospace.
- `<pre>`: teks preformatted — spasi dan baris baru dipertahankan.
- `<kbd>`: input keyboard (inline), menandai ketikan pengguna.
- `<samp>`: output program (inline), menandai hasil keluaran.

## Fungsi

Menampilkan kode, masukan keyboard, atau keluaran sistem dengan makna semantik yang jelas.

## Cara Pengimplementasian

```html
<p>Gunakan <code>&lt;div&gt;</code> untuk membungkus bagian halaman.</p>

<pre>
function buatRumah() {
  return "Rumah selesai";
}
</pre>

<p>Tekan <kbd>Ctrl</kbd> + <kbd>S</kbd> untuk menyimpan.</p>

<p>Hasil: <samp>Rumah berhasil dibangun.</samp></p>
```

## Analogi (tema RUMAH/BANGUNAN)

- `<code>`: seperti **tulisan pada cetak biru (blueprint)** — kecil, teknis, dan presisi.
- `<pre>`: seperti **papan ukuran di lapangan** — semua spasi dan jarak asli tertulis apa adanya.
- `<kbd>`: seperti **tombol saklar lampu** — sesuatu yang ditekan oleh penghuni rumah.
- `<samp>`: seperti **layar meteran listrik** — menampilkan hasil/keluaran dari sistem rumah.

## Dipakai Untuk

- Menampilkan potongan kode di tutorial
- Menulis puisi atau teks dengan spasi tetap (`<pre>`)
- Menandai shortcut keyboard (`<kbd>`)
- Menampilkan output terminal (`<samp>`)

## Kesalahan Umum

- Menggunakan `<code>` untuk blok kode panjang — sebaiknya bungkus dengan `<pre>` juga.
- Menggunakan `<pre>` tanpa `<code>` di dalamnya untuk kode (tidak salah, tapi kurang semantik).
- Menulis spasi berlebihan di `<code>` karena sifat inline-nya tidak mempertahankan whitespace.
- Mengabaikan karakter `&`, `<`, `>` yang harus di-encode sebagai `&amp;`, `&lt;`, `&gt;`.

## Koneksi dengan Materi Sebelumnya

Setelah mengenal heading, paragraf, daftar, dan kutipan, kini kita melihat elemen teks teknis. Ini berguna saat dokumentasi kode atau materi programming.

## Soal Latihan

Buat satu paragraf yang berisi `<code>` untuk tag `<p>`, `<kbd>` untuk tombol Enter, dan `<samp>` untuk teks "Berhasil".

<details><summary>Jawaban</summary>

```html
<p>Gunakan <code>&lt;p&gt;</code> lalu tekan <kbd>Enter</kbd>. Hasil: <samp>Berhasil</samp>.</p>
```

</details>
