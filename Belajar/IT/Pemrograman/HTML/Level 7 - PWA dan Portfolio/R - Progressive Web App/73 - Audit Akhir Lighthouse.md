# Audit Akhir Lighthouse

## Penjelasan
Lighthouse adalah tool otomatis bawaan Google Chrome yang mengaudit kualitas website dari lima aspek: Performance, Accessibility, Best Practices, SEO, dan PWA. Setiap aspek diberi skor 0–100. Target kelulusan adalah ≥90 untuk empat kategori pertama dan PWA pass.

## Fungsi
- Mengukur seberapa cepat website dimuat (Performance)
- Memastikan website ramah bagi penyandang disabilitas (Accessibility)
- Mengecek praktik coding yang aman dan modern (Best Practices)
- Memastikan website mudah ditemukan di mesin pencari (SEO)
- Memverifikasi persyaratan Progressive Web App (PWA)

## Cara Pengimplementasian

```html
<!-- Contoh optimasi Performance: lazy loading gambar -->
<img src="hero.jpg" loading="lazy" alt="Hero Image">
```

```html
<!-- Contoh optimasi Accessibility: label pada form -->
<label for="email">Alamat Email</label>
<input type="email" id="email" name="email">
```

```html
<!-- Contoh optimasi SEO: meta description -->
<meta name="description" content="Toko online sepatu terbaik di Indonesia">
```

```html
<!-- Contoh optimasi Best Practices: HTTPS dan ukuran gambar -->
<!-- Gunakan HTTPS, kompres gambar, jangan gunakan HTTP -->
```

## Target Skor Lighthouse

| Kategori         | Target Minimal |
|------------------|----------------|
| Performance      | ≥ 90           |
| Accessibility    | ≥ 90           |
| Best Practices   | ≥ 90           |
| SEO              | ≥ 90           |
| PWA              | Pass           |

## Analogi (tema RUMAH/BANGUNAN)
Lighthouse adalah **tim inspektur bangunan** yang memeriksa rumah dari berbagai sisi. Performance = seberapa cepat pintu terbuka dan lampu menyala. Accessibility = apakah kursi roda bisa masuk, apakah rambu jelas. Best Practices = apakah kabel listrik rapi dan aman. SEO = apakah alamat rumah mudah ditemukan di peta. PWA = apakah rumah punya papan nama dan satpam (manifest & service worker). Skor ≥90 artinya rumah **layak huni dan dijual**.

## Dipakai Untuk
- Quality assurance sebelum website diluncurkan
- Mengecek kelayakan PWA
- Memastikan website inklusif dan cepat

## Kesalahan Umum
- Fokus hanya pada Performance, mengabaikan aksesibilitas
- Gambar tidak memiliki `alt` text (pengaruh besar ke Accessibility)
- Ukuran gambar terlalu besar (menurunkan Performance)
- Tidak menggunakan meta viewport (pengaruh ke SEO & Accessibility)
- Lupa cek tab PWA padahal sudah pasang manifest dan service worker

## Koneksi dengan Materi Sebelumnya
Audit Lighthouse adalah puncak evaluasi dari semua materi sebelumnya — dari HTML semantik (Accessibility), meta tags (SEO), optimasi gambar (Performance), hingga manifest dan service worker (PWA). Ini seperti ujian komprehensif.

## Soal Latihan
<details><summary>Jawaban</summary>

1. Apa yang terjadi jika skor Accessibility di bawah 90?
   **Jawaban:** Website mungkin sulit diakses oleh pengguna dengan disabilitas (tunanetra, tunarungu, dll). Lighthouse akan memberikan daftar pelanggaran seperti kurangnya label form atau kontras warna rendah.

2. Sebutkan tiga cara meningkatkan skor Performance!
   **Jawaban:** (1) Kompres dan resize gambar, (2) Gunakan lazy loading, (3) Minify CSS/JS.

3. Apa bedanya audit Lighthouse dengan validasi W3C?
   **Jawaban:** Lighthouse mengaudit kualitas (kecepatan, aksesibilitas, praktik terbaik), sedangkan validasi W3C mengecek apakah kode HTML sesuai standar sintaks resmi. Keduanya saling melengkapi.

</details>
