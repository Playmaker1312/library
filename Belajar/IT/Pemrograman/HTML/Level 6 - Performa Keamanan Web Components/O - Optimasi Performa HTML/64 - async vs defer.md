# async vs defer

## Penjelasan
Atribut `async` dan `defer` pada tag `<script>` mengontrol bagaimana dan kapan JavaScript dimuat serta dieksekusi relatif terhadap proses parsing HTML. Tanpa atribut, script memblokir parser. Dengan `async`, script diunduh paralel dan dieksekusi begitu siap. Dengan `defer`, script diunduh paralel tapi dieksekusi setelah parsing HTML selesai.

## Fungsi
- Mencegah JavaScript memblokir pembuatan DOM
- Mengontrol urutan eksekusi script
- Mempercepat waktu render halaman dengan mem-parsing HTML tanpa hambatan

## Cara Pengimplementasian
```html
<!-- ❌ BLOKIR: parsing berhenti sampai script diunduh & dijalankan -->
<script src="analytics.js"></script>

<!-- ⚡ ASYNC: unduh paralel, eksekusi langsung saat siap (tidak urut) -->
<script src="iklan.js" async></script>

<!-- ✅ DEFER: unduh paralel, eksekusi setelah HTML selesai di-parsing (urut) -->
<script src="app.js" defer></script>
<script src="vendor.js" defer></script>

<!-- Kapan pakai apa? -->
<script src="tracker.js" async></script>       <!-- mandiri, urutan tidak penting -->
<script src="ui-library.js" defer></script>    <!-- tergantung DOM, perlu urutan -->
```

## Analogi (tema RUMAH/BANGUNAN)
Bayangkan proyek bangun rumah dengan tiga mandor berbeda:

- **Tanpa atribut (blokir)**: Mandor A berhenti total, duduk menunggu satu truk semen (script) datang dan dibongkar, baru melanjutkan kerja. Semua pekerja lain menganggur.
- **`async`**: Mandor B memesan material secara paralel. Saat material tiba, mandor langsung menggunakannya di mana pun dia berada — meskipun fondasi (HTML) belum selesai. Cepat, tapi berantakan.
- **`defer`**: Mandor C memesan material paralel juga, tapi semua material ditumpuk rapi di gudang. Mandor baru mulai menggunakannya setelah fondasi dan rangka (DOM) benar-benar selesai. Rapi dan terprediksi.

## Dipakai Untuk
| Atribut | Cocok untuk |
|---------|-------------|
| **async** | Analytics, iklan, tracker, widget pihak ketiga yang berdiri sendiri |
| **defer** | Script yang memanipulasi DOM, library UI, kode aplikasi utama |
| **tanpa atribut** | Hanya untuk script kecil inline atau script yang harus jalan sebelum DOM siap (sangat jarang) |

## Kesalahan Umum
- Menggunakan `async` untuk script yang bergantung pada DOM (`document.getElementById` akan gagal jika DOM belum siap)
- Menggunakan `defer` untuk script yang harus dieksekusi segera tanpa menunggu DOM (misal redirect dini)
- Menaruh `defer` pada script yang tidak perlu (tidak masalah, tapi berlebihan)
- Tidak memberi atribut sama sekali untuk script besar di `<head>`, memperlambat render secara signifikan
- Mengira `async` dan `defer` bisa dipakai di script inline — keduanya hanya bekerja untuk script dengan atribut `src`

## Koneksi dengan Materi Sebelumnya
- **Level 4 - JavaScript**: Dasar cara kerja `<script>` di HTML
- **Critical Rendering Path (materi 63)**: Tanpa defer/async, script memblokir CRP; dengan defer/async, CRP tidak terhambat
- **Resource Hints (materi 65)**: Preload/preconnect bisa dikombinasikan dengan async/defer untuk memuat script lebih efisien

## Soal Latihan
1. Apa perbedaan `async` dan `defer` dalam hal urutan eksekusi?
2. Jika kamu punya dua script `a.js` dan `b.js` dengan `defer`, dalam urutan apa keduanya dieksekusi?
3. Kapan sebaiknya menggunakan `async`?
4. Bisakah atribut `defer` digunakan pada script inline `<script>console.log('test')</script>`?

<details><summary>Jawaban</summary>
1. `async` mengeksekusi script begitu siap diunduh tanpa menjamin urutan. `defer` mengeksekusi script setelah parsing HTML selesai dan menjamin urutan sesuai urutan tag `<script>`.
2. Sesuai urutan di HTML: `a.js` dulu, baru `b.js`.
3. Untuk script yang mandiri dan tidak bergantung pada DOM atau script lain, seperti analytics tracker, iklan, atau widget pihak ketiga.
4. Tidak. Atribut `defer` dan `async` hanya berpengaruh pada script dengan atribut `src` (file eksternal). Script inline selalu dieksekusi langsung.
</details>
