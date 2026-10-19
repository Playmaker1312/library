# Web App Manifest

## Penjelasan
Web App Manifest adalah file JSON yang memberi tahu browser tentang bagaimana aplikasi web Anda harus berperilaku ketika diinstal ke layar beranda perangkat pengguna. Manifest memungkinkan website berjalan seperti aplikasi native.

## Fungsi
- Mengubah website menjadi aplikasi yang dapat diinstal (installable)
- Menentukan ikon, nama, dan tema aplikasi
- Mengontrol mode tampilan (standalone, fullscreen, dll)
- Menentukan URL awal saat aplikasi dibuka

## Cara Pengimplementasian

```html
<!-- Link manifest di index.html -->
<link rel="manifest" href="/manifest.json">
```

```json
{
  "name": "My Web App",
  "short_name": "MyApp",
  "start_url": "/index.html",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#2196f3",
  "icons": [
    {
      "src": "/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

## Analogi (tema RUMAH/BANGUNAN)
Web App Manifest bagaikan **papan nama dan brosur rumah**. `name` adalah nama rumah, `short_name` adalah nama panggilan, `start_url` adalah alamat pintu masuk, `display:standalone` membuat rumah terlihat seperti bangunan mandiri (bukan bagian dari kompleks browser), dan `icons` adalah foto-foto rumah yang muncul di galeri (layar beranda).

## Dipakai Untuk
- Progressive Web App (PWA) yang bisa diinstal di HP/desktop
- Aplikasi yang ingin tampil tanpa elemen browser (address bar)
- Website yang ingin memiliki kehadiran di layar beranda pengguna

## Kesalahan Umum
- Lupa menambahkan tag `<link rel="manifest">` di HTML
- Ukuran ikon tidak sesuai standar (min. 192x192 dan 512x512)
- `short_name` lebih panjang dari 12 karakter
- Typo JSON (koma berlebih atau kurang)

## Koneksi dengan Materi Sebelumnya
Sebelumnya kita belajar struktur HTML dasar dan meta tags. Manifest adalah lanjutan dari meta tags — meta tags mengontrol tampilan di browser, manifest mengontrol tampilan di luar browser (layar beranda).

## Soal Latihan
<details><summary>Jawaban</summary>

1. Apa fungsi `display:standalone` pada manifest?
   **Jawaban:** Menghilangkan elemen browser seperti address bar dan tab strip, sehingga aplikasi tampil seperti aplikasi native.

2. Sebutkan properti wajib di manifest.json!
   **Jawaban:** `name` atau `short_name`, `icons`, `start_url`, `display`.

3. Mengapa kita butuh dua ukuran ikon (192x192 dan 512x512)?
   **Jawaban:** Android dan beberapa platform membutuhkan ikon 192x192 untuk splash screen, sementara 512x512 untuk ikon tinggi di layar beranda.

</details>
