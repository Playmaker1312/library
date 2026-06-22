# Date, Time, File

## Penjelasan

Tiga tipe input HTML5 untuk menangani data spesifik: `date` memilih tanggal, `time` memilih jam, dan `file` mengunggah berkas. Masing-masing memiliki atribut pendukung seperti `accept` (filter jenis file) dan `multiple` (unggah banyak file sekaligus).

## Fungsi

- **`type="date"`** — Menampilkan date picker untuk memilih tanggal (tahun-bulan-hari). Nilai dikirim dalam format `YYYY-MM-DD`.
- **`type="time"`** — Menampilkan time picker untuk memilih jam. Nilai dikirim dalam format `HH:MM` atau `HH:MM:SS`.
- **`type="file"`** — Menampilkan tombol "Pilih File". Membuka dialog file system OS. Data dikirim sebagai multipart/form-data.
- **`accept`** — Membatasi jenis file yang bisa dipilih (ekstensi atau MIME type).
- **`multiple`** — Mengizinkan pengguna memilih lebih dari satu file sekaligus.

## Cara Pengimplementasian

```html
<form enctype="multipart/form-data">
  <!-- Date -->
  <label for="tgl">Tanggal Kunjungan:</label>
  <input type="date" id="tgl" name="tgl"
         min="2024-01-01" max="2025-12-31">

  <!-- Time -->
  <label for="jam">Jam Kedatangan:</label>
  <input type="time" id="jam" name="jam"
         min="08:00" max="17:00">

  <!-- File (satu file gambar) -->
  <label for="ktp">Upload KTP (jpg/png):</label>
  <input type="file" id="ktp" name="ktp"
         accept=".jpg,.jpeg,.png">

  <!-- File (banyak file) -->
  <label for="dokumen">Upload Dokumen Pendukung:</label>
  <input type="file" id="dokumen" name="dokumen"
         accept="application/pdf"
         multiple>

  <button type="submit">Kirim</button>
</form>
```

**Catatan:** Untuk upload file, form WAJIB punya atribut `enctype="multipart/form-data"`.

## Analogi (RUMAH/BANGUNAN)

- **`type="date"`** = Kalender tempel di dinding dapur. Anda tinggal menunjuk tanggal untuk menandai jadwal renovasi.
- **`type="time"`** = Jam dinding di ruang tamu. Anda memutar jarum ke waktu yang diinginkan untuk janji dengan kontraktor.
- **`type="file"`** = Kotak surat di depan rumah. Anda memasukkan dokumen (foto, PDF, dll) ke dalamnya untuk dikirim ke arsitek.
- **`accept`** = Celah kotak surat yang hanya muat amplop ukuran tertentu — dokumen terlalu besar tidak bisa masuk.
- **`multiple`** = Kotak surat bertuliskan "Masukkan semua dokumen sekaligus" — muat banyak file dalam satu kiriman.

## Dipakai Untuk

- `date` — Tanggal lahir, tanggal booking, tanggal acara, tanggal pengiriman.
- `time` — Jam buka, jam janji temu, jam keberangkatan, durasi (via `step`).
- `file` — Upload foto profil, upload KTP/SIM, upload lampiran email, portfolio file.
- `accept` — Filter file agar user hanya melihat file yang relevan (misal hanya PDF).
- `multiple` — Upload galeri foto, upload banyak dokumen laporan.

## Kesalahan Umum

1. **Lupa `enctype="multipart/form-data"` pada form** — Tanpa ini, file tidak akan terkirim. Data file akan terpotong atau menjadi nama file saja.
2. **`accept` bukan pengaman** — `accept` hanya filter di dialog OS. User bisa memilih "All Files" dan upload file berbahaya. **Validasi tetap wajib di server.**
3. **Tidak mengecek ukuran file** — Atribut HTML tidak punya `maxfilesize`. Gunakan JavaScript atau validasi server untuk membatasi ukuran.
4. **`date` dan `time` tidak konsisten antar browser** — Browser lama menampilkan input teks biasa. Selalu sediakan fallback atau gunakan library date picker untuk konsistensi.
5. **`multiple` tanpa penanganan server** — Server harus dikonfigurasi menerima array file (misal `$_FILES` di PHP sudah array jika `name="dokumen[]"`).

## Koneksi dengan Materi Sebelumnya

- **`min`/`max`** — Bisa digunakan pada `type="date"` dan `type="time"` untuk membatasi rentang.
- **`required`** — Membuat input file/date/time wajib diisi.
- **Atribut `name`** — Untuk `multiple`, tambahkan `[]` di akhir `name` (misal `dokumen[]`) agar server menerima sebagai array.
- **Encoding form** — `enctype` menentukan cara data dikirim. Form biasa pakai `application/x-www-form-urlencoded`, form upload file pakai `multipart/form-data`.

## Soal Latihan

1. Buat form upload foto profil dengan: hanya menerima .jpg, .jpeg, .png, ukuran file tidak dicek di HTML, wajib diisi, dan bisa upload lebih dari satu.
2. Buat input untuk memilih tanggal lahir dengan rentang tahun 1950 sampai 2010.
3. Jelaskan isi dari atribut `enctype` pada form upload file dan apa yang terjadi jika tidak disertakan.

<details><summary>Jawaban</summary>

1.
```html
<form enctype="multipart/form-data">
  <label for="foto">Upload Foto Profil:</label>
  <input type="file" id="foto" name="foto[]"
         accept=".jpg,.jpeg,.png"
         multiple
         required>
  <button type="submit">Kirim</button>
</form>
```

2.
```html
<label for="tgl_lahir">Tanggal Lahir:</label>
<input type="date" id="tgl_lahir" name="tgl_lahir"
       min="1950-01-01" max="2010-12-31" required>
```

3. `enctype="multipart/form-data"` memberitahu browser untuk mengirim data dalam beberapa bagian (multipart) yang memisahkan metadata form dengan binary data file. Jika tidak disertakan, browser menggunakan default `application/x-www-form-urlencoded` yang hanya bisa mengirim teks — file tidak akan terkirim dengan benar (hanya nama file atau data rusak yang sampai ke server).

</details>
