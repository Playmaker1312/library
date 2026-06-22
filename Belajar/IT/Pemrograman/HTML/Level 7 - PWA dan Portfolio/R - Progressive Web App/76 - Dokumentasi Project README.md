# Dokumentasi Project README

## Penjelasan
README.md adalah file Markdown yang menjadi halaman depan repository di GitHub. README menjelaskan apa project ini, cara menggunakannya, fitur-fiturnya, dan informasi penting lainnya. README yang baik membuat project terlihat profesional.

## Fungsi
- Memperkenalkan project ke pengunjung repository
- Menjelaskan cara instalasi dan penggunaan
- Menampilkan fitur dan skor Lighthouse (nilai jual)
- Mencantumkan teknologi yang digunakan
- Memudahkan kolaborasi dengan kontributor lain

## Struktur README yang Baik

```markdown
# Nama Project

![Lighthouse Score](https://img.shields.io/badge/Performance-95-brightgreen)
![Lighthouse Accessibility](https://img.shields.io/badge/Accessibility-92-brightgreen)
![Lighthouse Best Practices](https://img.shields.io/badge/Best%20Practices-90-brightgreen)
![Lighthouse SEO](https://img.shields.io/badge/SEO-100-brightgreen)

## 📋 Deskripsi
Penjelasan singkat tentang project ini.

## ✨ Fitur
- ✅ Fitur 1
- ✅ Fitur 2
- ✅ Fitur 3

## 🚀 Demo
[Lihat Demo](https://username.github.io/nama-repo)

## 🛠 Teknologi
- HTML5
- CSS3
- JavaScript
- Web App Manifest
- Service Worker

## 📸 Screenshot
![Screenshot](screenshot.png)

## ⚙️ Cara Menjalankan
1. Clone repository
   ```bash
   git clone https://github.com/username/nama-repo.git
   ```
2. Buka `index.html` di browser

## 📊 Hasil Lighthouse
| Kategori         | Skor |
|------------------|------|
| Performance      | 95   |
| Accessibility    | 92   |
| Best Practices   | 90   |
| SEO              | 100  |
| PWA              | Pass |

## 👨‍💻 Author
Nama Anda - [@username](https://github.com/username)
```

## Analogi (tema RUMAH/BANGUNAN)
README adalah **brosur properti dan buku panduan rumah**. Saat seseorang datang ke kompleks (repository), brosur ini menjelaskan: ini rumah tipe apa (deskripsi), apa saja fiturnya (3 kamar, kolam renang), berapa skor kualitasnya (Lighthouse score), teknologi apa yang dipakai (bata, kayu jati), dan bagaimana cara memasukinya (instalasi). Tanpa README, rumah hanyalah bangunan tanpa papan informasi.

## Dipakai Untuk
- Semua project yang diupload ke GitHub
- Project portofolio yang akan dilamar ke perusahaan
- Project open-source yang butuh kontributor

## Kesalahan Umum
- README terlalu panjang dan tidak terstruktur
- Tidak mencantumkan skor Lighthouse (padahal itu nilai jual)
- Tidak ada link demo live
- Bahasa campur aduk (Indonesia-Inggris tidak konsisten)
- Screenshot rusak atau tidak ada
- Tidak menyertakan cara menjalankan project

## Koneksi dengan Materi Sebelumnya
README adalah **dokumentasi akhir** yang merangkum seluruh perjalanan dari Level 1 (HTML dasar) hingga Level 7 (PWA dan deploy). Semua fitur yang dibuat, diuji (Lighthouse), dan dideploy akan didokumentasikan di README. Ini adalah **wajah project** di dunia luar.

## Soal Latihan
<details><summary>Jawaban</summary>

1. Sebutkan 5 bagian penting dalam README!
   **Jawaban:** (1) Judul dan badge, (2) Deskripsi, (3) Fitur, (4) Teknologi, (5) Cara menjalankan.

2. Mengapa skor Lighthouse perlu dicantumkan di README?
   **Jawaban:** Sebagai bukti kualitas website — menunjukkan bahwa project tidak asal jadi, tapi sudah melalui audit dan optimasi. Ini nilai jual untuk portofolio.

3. Apa itu badge di README dan fungsinya?
   **Jawaban:** Badge adalah lencana kecil (shields.io) yang menampilkan informasi seperti skor, lisensi, atau status build. Fungsinya sebagai indikator visual cepat tanpa perlu membaca detail.

</details>
