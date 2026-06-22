# Membuat File HTML Pertama

## Penjelasan
Setelah menyiapkan environment, langkah selanjutnya adalah membuat file HTML pertama. File HTML berekstensi `.html` dan bisa dibuka langsung di browser. Kita akan membuat struktur minimal yang valid dan menampilkan teks "Hello World" pertama di browser.

## Fungsi
- Memulai praktik menulis HTML langsung
- Memahami struktur minimal dokumen HTML
- Verifikasi bahwa environment sudah berfungsi benar
- Menjadi template dasar untuk file HTML selanjutnya

## Cara Pengimplementasian
```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Halaman Pertamaku</title>
</head>
<body>
  <h1>Hello World!</h1>
  <p>Ini adalah halaman HTML pertamaku.</p>
</body>
</html>
```

## Analogi
Membangun file HTML pertama seperti **meletakkan batu bata pertama sebuah rumah**. Belum ada atap, cat, atau perabotan. Tapi batu bata pertama ini adalah momen penting — dimulainya proses konstruksi. Setelah batu bata pertama terpasang, kita bisa terus menambah dinding, jendela, dan pintu.

## Dipakai Untuk
- Template awal setiap halaman web baru
- Testing apakah VS Code, Live Server, dan browser berfungsi
- Memulai proyek web dari nol

## Kesalahan Umum
- Lupa memberi ekstensi `.html` → file tidak dikenali browser
- Mengetik manual `http://` padahal cukup klik kanan → "Open with Live Server"
- Salah struktur: meletakkan `<title>` di dalam `<body>` atau `<h1>` di luar `<body>`
- Tidak menggunakan DOCTYPE → browser masuk quirks mode

## Koneksi dengan Materi Sebelumnya
Ini adalah praktik pertama setelah teori (file 01-02) dan setup alat (file 03). Semua persiapan sebelumnya bertujuan untuk momen ini: menulis dan melihat kode HTML pertama berjalan di browser.

## Soal Latihan
1. Ekstensi file apa yang harus digunakan untuk dokumen HTML?
2. Tuliskan struktur HTML minimal yang valid beserta penjelasan singkat setiap bagiannya.

<details>
<summary>Jawaban</summary>
1. `.html`
2. `<!DOCTYPE html>` (deklarasi tipe dokumen), `<html lang="id">` (pembungkus root), `<head>` (metadata), `<meta charset="UTF-8">` (encoding karakter), `<title>` (judul tab browser), `<body>` (konten yang tampil), `<h1>` (heading utama).
</details>
