# Optimasi Gambar

## Penjelasan

Optimasi gambar adalah proses mengurangi ukuran file gambar tanpa mengurangi kualitas visual secara signifikan. Ini mencakup dua hal utama: **kompresi** (memperkecil ukuran byte) dan **resize** (memperkecil dimensi piksel). Konversi ke format modern seperti **WebP** bisa mengecilkan ukuran 25–35% dibanding JPEG pada kualitas yang sama. Alat seperti **Squoosh.app** (buatan Google) memudahkan eksperimen kompresi secara visual. Target umum: setiap gambar di bawah **200 KB**, sementara gambar hero bisa sampai **300–500 KB**.

```html
<!-- Contoh: gambar sudah dioptimasi sebelum di-deploy -->
<img src="hero-1200.webp"
     srcset="hero-400.webp 400w, hero-800.webp 800w, hero-1200.webp 1200w"
     sizes="100vw"
     alt="Hero teroptimasi">
```

## Fungsi

- **Mempercepat waktu muat halaman** — ukuran file lebih kecil = lebih cepat download.
- **Menghemat bandwidth pengunjung** — penting untuk pengguna seluler dengan kuota terbatas.
- **Meningkatkan skor Lighthouse / Core Web Vitals** — terutama LCP (waktu muat gambar terbesar).
- **Mengurangi biaya hosting dan CDN** — semakin kecil file, semakin murah transfer data.
- **Format WebP/AVIF** memberikan kualitas setara JPEG dengan ukuran 25–35% lebih kecil.

## Cara Pengimplementasian

```html
<!-- WebP dengan fallback JPEG via picture -->
<picture>
  <source srcset="foto.webp" type="image/webp">
  <img src="foto.jpg" alt="Foto teroptimasi">
</picture>

<!-- Atau kirim WebP langsung jika user base modern -->
<img src="foto.webp" alt="Foto teroptimasi">
```

### Workflow optimasi (rekomendasi):
1. **Resize** dimensi gambar ke ukuran maksimum yang dibutuhkan (misal 1920px untuk desktop)
2. **Kompres** dengan quality 75–85%
3. **Konversi** ke WebP (atau AVIF untuk penghematan lebih besar)
4. **Cek ukuran akhir** — target <200 KB per gambar
5. **Siapkan beberapa versi** untuk `srcset` (400w, 800w, 1200w)

### Tools:
- **Squoosh.app** — GUI online untuk kompresi & konversi (Google)
- **cwebp** — CLI encoder WebP
- **ImageOptim** — desktop app (Mac)
- **sharp** — library Node.js untuk otomatisasi build

## Analogi (tema RUMAH/BANGUNAN)

Bayangkan **sebuah rumah yang akan dijual**. Foto rumah di iklan bisa saja dalam bentuk file RAW 50 MB (setara gambar asli 4000px). Tapi agen properti tidak akan mengirim file RAW ke calon pembeli — mereka akan resize ke 1200px dan kompres jadi JPEG 150 KB. Ini tetap cukup jelas untuk melihat detail rumah, tapi muat dikirim lewat WhatsApp. WebP seperti punya tukang foto yang bisa mengepak album foto rumah dalam format lebih rapat — ukuran koper lebih kecil, isi tetap lengkap.

## Dipakai Untuk

- Semua gambar di website produksi — tidak ada alasan mengirim gambar mentah dari kamera
- Hero image, thumbnail produk, foto artikel, ikon, ilustrasi
- Gambar sebelum diunggah ke CMS / static site generator
- Pipeline build (otomatis dengan sharp/imagemin) di proyek React, Vue, atau static site
- Semua gambar yang akan melewati CDN dengan transform on-the-fly (Imgix, Cloudinary)

## Kesalahan Umum

- **Quality 100%** — tidak ada gunanya untuk web, file membengkak tanpa peningkatan visual yang kasat mata.
- **Tidak resize** — mengirim gambar 4000px untuk ditampilkan di thumbnail 200px adalah pemborosan besar.
- **Hanya sediakan satu ukuran** — tidak memanfaatkan `srcset`, sehingga layar kecil download gambar besar.
- **Mengabaikan metadata** — foto dari kamera mengandung EXIF (GPS, kamera, dll) yang bisa dihapus untuk memperkecil file.
- **Tidak memeriksa ukuran final** — mengira sudah optimasi padahal file masih 500 KB+.
- **Kompresi berlebihan** — quality di bawah 50% menyebabkan artefak blocky yang sangat terlihat.

### Tabel Referensi Kualitas & Ukuran

| Quality | JPEG (1200px) | WebP (1200px) | Keterangan Visual |
|---------|--------------|--------------|-------------------|
| 85%     | ~180 KB      | ~120 KB      | Hampir lossless |
| 75%     | ~120 KB      | ~80 KB       | Baik, sedikit kompresi |
| 50%     | ~60 KB       | ~40 KB       | Artefak mulai terlihat |

## Koneksi dengan Materi Sebelumnya

- **Level 3 — Image Resolution**: Memahami DPR membantu menentukan ukuran gambar yang perlu disiapkan (1x, 2x).
- **Level 5 — `<picture>` & `srcset` (56)**: Optimasi gambar adalah langkah sebelum menerapkan `srcset` — optimasi dulu, baru buat variant ukuran.
- **Level 5 — `loading="lazy"` (57)**: Gambar yang sudah dioptimasi + lazy loading = performa maksimal.
- **Build Tools (Webpack/Vite)**: Plugin `imagemin` atau `sharp` otomatis mengoptimasi gambar saat build.

## Soal Latihan

1. Sebutkan 3 langkah utama dalam workflow optimasi gambar untuk web.

<details><summary>Jawaban</summary>

1. **Resize** gambar ke dimensi maksimum yang dibutuhkan (misal 1920px).
2. **Kompres** dengan quality 75–85%.
3. **Konversi** ke format modern (WebP/AVIF) dan sediakan fallback.

</details>

2. Jika sebuah JPEG 1200px dengan quality 85% berukuran 180 KB, berapa kira-kira ukuran WebP setara kualitasnya?

<details><summary>Jawaban</summary>

Sekitar **100–130 KB** (25–35% lebih kecil dari JPEG). Rumus kasar: 180 KB × 0,7 = ~126 KB. Ini tergantung konten gambar (gambar dengan banyak detail datar lebih kecil).

</details>
