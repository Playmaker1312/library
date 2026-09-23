# Tag `<form>`, `action`, `method` GET vs POST

## Penjelasan

Tag `<form>` adalah wadah utama untuk membuat formulir di HTML. Semua elemen input, tombol, dan label ditempatkan di dalam tag `form`. Atribut `action` menentukan ke mana data dikirim, sedangkan `method` menentukan cara pengiriman data (GET atau POST).

## Fungsi

- `<form>` — membungkus seluruh elemen formulir.
- `action` — URL tujuan pengiriman data formulir.
- `method` — metode HTTP yang digunakan saat mengirim data.

## Cara Pengimplementasian

```html
<form action="/proses.php" method="POST">
  <label for="nama">Nama:</label>
  <input type="text" id="nama" name="nama">
  <button type="submit">Kirim</button>
</form>

<form action="/cari" method="GET">
  <input type="text" name="q" placeholder="Cari...">
  <button type="submit">Cari</button>
</form>
```

## Analogi (tema RUMAH/BANGUNAN)

Bayangkan `<form>` adalah **pintu utama** sebuah rumah. Setiap kali pengunjung ingin menyerahkan sesuatu (data) ke dalam rumah, mereka harus melewati pintu ini. Atribut `action` adalah **alamat rumah** yang dituju, dan `method` adalah **cara kurir mengantar paket**: GET seperti menempelkan paket di papan pengumuman depan rumah (data terlihat di URL), POST seperti kurir masuk lewat belakang dan menyerahkan langsung ke penghuni (data tidak terlihat).

## Dipakai Untuk

Mengirim data dari pengguna ke server untuk diproses — login, registrasi, pencarian, upload, dll.

## Kesalahan Umum

- Lupa menambahkan atribut `name` pada input — data tidak akan terkirim.
- Menggunakan GET untuk data sensitif (password) — password akan terlihat di URL.
- Tidak menyertakan tag `<form>` sama sekali, hanya input tanpa pembungkus.
- action kosong atau salah URL.

## Koneksi dengan Materi Sebelumnya

Sebelumnya kita belajar tag dasar dan atribut HTML. `<form>` adalah salah satu tag yang membutuhkan banyak atribut dan elemen anak, jadi pemahaman tentang struktur tag dan atribut sangat penting.

## Soal Latihan

1. Apa perbedaan utama method GET dan POST?
2. Sebutkan dua atribut wajib yang biasanya ada pada tag `<form>`.
3. Jika Anda membuat form login, method apa yang paling tepat? Mengapa?

<details><summary>Jawaban</summary>
1. GET menempelkan data di URL (terlihat, terbatas 2048 karakter), POST mengirim data di body request (tidak terlihat, tanpa batas kapasitas).<br>
2. `action` (tujuan pengiriman) dan `method` (cara pengiriman).<br>
3. POST, karena data login (username & password) bersifat sensitif dan tidak boleh terlihat di URL.
</details>
