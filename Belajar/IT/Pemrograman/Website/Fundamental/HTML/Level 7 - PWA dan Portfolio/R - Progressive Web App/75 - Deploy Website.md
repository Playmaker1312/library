# Deploy Website

## Penjelasan
Deploy adalah proses mengunggah website ke server agar bisa diakses publik melalui internet. Ada banyak platform hosting gratis untuk website statis, seperti GitHub Pages, Netlify, dan Vercel. Kita juga bisa menggunakan custom domain (domain sendiri).

## Fungsi
- Membuat website bisa diakses oleh siapa saja, 24/7
- Memberikan URL publik untuk portofolio
- Memungkinkan integrasi CI/CD (otomatis deploy saat push)
- Mendukung HTTPS dan custom domain

## Cara Pengimplementasian

### GitHub Pages

```bash
# 1. Buat repository di GitHub dengan nama username.github.io
# 2. Push file website ke branch main
git init
git add .
git commit -m "Deploy website"
git remote add origin https://github.com/username/username.github.io.git
git push -u origin main

# 3. Website langsung bisa diakses di https://username.github.io
```

```json
// Untuk project biasa (bukan username.github.io):
// Settings → Pages → Pilih branch main → folder /root atau /docs
// URL: https://username.github.io/nama-repository
```

### Netlify Drop

```bash
# 1. Buka https://app.netlify.com/drop
# 2. Seret folder project ke area drop
# 3. Dapatkan URL: https://random-name-1234.netlify.app
```

### Vercel

```bash
# 1. Install Vercel CLI
npm i -g vercel

# 2. Deploy dari terminal
vercel --prod

# 3. Ikuti wizard → dapatkan URL
```

### Custom Domain

```javascript
// 1. Beli domain (Niagahoster, Namecheap, dll)
// 2. Di penyedia domain, tambahkan CNAME record ke platform hosting
//    GitHub:  username.github.io
//    Netlify: random-name-1234.netlify.app
//    Vercel:  project.vercel.app
// 3. Tambahkan custom domain di dashboard hosting
```

## Analogi (tema RUMAH/BANGUNAN)
Deploy adalah **proses pindah rumah dari tempat kerja (localhost) ke perumahan (internet)**. GitHub Pages, Netlify, Vercel adalah **kompleks perumahan** yang menyediakan tanah gratis. Custom domain adalah **nomor rumah dan nama jalan pilihan Anda sendiri**, bukan nomor acak dari pengembang. Push ke repository = mengirim kontainer berisi semua barang (file) ke rumah baru.

## Dipakai Untuk
- Mempublikasikan website portofolio
- Menampilkan project ke klien atau recruiter
- Meluncurkan website produksi

## Kesalahan Umum
- Lupa mengubah path file dari absolut (`/`) ke relatif (`./`) untuk GitHub Pages
- Root folder salah pilih (harusnya `/root` bukan `/docs`)
- Custom domain tidak diarahkan dengan CNAME/record yang benar
- Tidak menunggu propagasi DNS (bisa 1–48 jam)
- File `index.html` tidak ada di root folder

## Koneksi dengan Materi Sebelumnya
Deploy adalah **tahap akhir** setelah semua materi selesai. HTML, CSS, JavaScript, aset, manifest, service worker — semuanya dikemas dan dikirim ke server. Tanpa deploy, website hanya ada di komputer kita sendiri.

## Soal Latihan
<details><summary>Jawaban</summary>

1. Apa perbedaan GitHub Pages, Netlify, dan Vercel?
   **Jawaban:** GitHub Pages gratis dan terintegrasi dengan GitHub, Netlify unggul di drag-and-drop (Netlify Drop), Vercel unggul untuk frontend framework (Next.js, React). Semuanya gratis untuk proyek statis.

2. Mengapa perlu custom domain?
   **Jawaban:** Agar URL lebih profesional (contoh.com) dibanding URL bawaan (username.github.io). Juga memudahkan branding dan SEO.

3. Apa itu propagasi DNS?
   **Jawaban:** Waktu yang dibutuhkan agar perubahan DNS (pengaturan domain) menyebar ke seluruh server internet. Bisa 1–48 jam.

</details>
