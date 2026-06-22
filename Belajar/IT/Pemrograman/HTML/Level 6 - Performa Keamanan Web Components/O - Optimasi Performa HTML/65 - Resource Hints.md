# Resource Hints

## Penjelasan
Resource Hints adalah sekumpulan atribut pada tag `<link>` yang memberitahu browser tentang sumber daya yang akan dibutuhkan, sehingga browser bisa memuatnya lebih awal. Tiga yang utama: `preload` (butuh sekarang), `prefetch` (mungkin butuh nanti), dan `preconnect` (siapkan koneksi awal ke server lain).

## Fungsi
- `preload`: Memuat aset penting lebih awal sebelum ditemukan di HTML
- `prefetch`: Mengambil aset yang mungkin dibutuhkan di halaman berikutnya (waktu idle)
- `preconnect`: Melakukan DNS lookup, TLS handshake, dan koneksi TCP ke origin asing lebih awal

## Cara Pengimplementasian
```html
<!-- PRELOAD: aset pasti dipakai di halaman ini, muat sekarang -->
<link rel="preload" href="font.woff2" as="font" crossorigin>
<link rel="preload" href="hero.webp" as="image">
<link rel="preload" href="style.css" as="style">

<!-- PREFETCH: aset mungkin dipakai di halaman berikutnya -->
<link rel="prefetch" href="halaman-lain.html">
<link rel="prefetch" href="gambar-selanjutnya.jpg">

<!-- PRECONNECT: siapkan koneksi ke server lain -->
<link rel="preconnect" href="https://cdn.example.com">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
```

## Analogi (tema RUMAH/BANGUNAN)
Bayangkan kamu akan membangun dua rumah: Rumah A (halaman saat ini) dan Rumah B (halaman berikutnya).

- **`preload`**: Sebelum pembangunan rumah A dimulai, kamu sudah menyiapkan semen, bata, dan besi di lokasi. Tidak perlu menunggu truk datang.
- **`prefetch`**: Saat pekerja istirahat siang, kamu menyuruh mereka mengantar beberapa bahan bangunan ke lokasi Rumah B. Tidak mendesak, tapi menghemat waktu nanti.
- **`preconnect`**: Kamu telepon pemasok material dan minta mereka sudah siap di pintu. Jalur komunikasi (koneksi) sudah terbuka — tinggal ambil barang.

## Dipakai Untuk
| Hint | Skala Prioritas | Waktu Muat | Dipakai Untuk |
|------|----------------|------------|---------------|
| **preload** | Tinggi (wajib) | Segera | Font, hero image, CSS kritis, script penting |
| **prefetch** | Rendah (opsional) | Saat idle | Halaman navigasi berikutnya, aset yang mungkin dipakai |
| **preconnect** | Sedang | Secepatnya | Koneksi ke CDN, API, font server, origin pihak ketiga |

## Kesalahan Umum
- Terlalu banyak `preload` sehingga memboroskan bandwidth dan mengalahkan aset yang benar-benar penting (over-optimasi)
- Tidak menambahkan atribut `crossorigin` pada preload font, menyebabkan font di-download dua kali
- Menggunakan `prefetch` untuk halaman yang pasti dikunjungi — seharusnya `preload` atau navigasi biasa
- Mengira `prefetch` menjamin aset akan dipakai — browser bisa membuang prefetch jika bandwidth terbatas
- `preconnect` ke banyak domain tanpa alasan jelas, membuka koneksi yang tidak terpakai
- Lupa mencantumkan atribut `as` pada preload, membuat browser tidak bisa memprioritaskan dengan benar

## Koneksi dengan Materi Sebelumnya
- **Critical Rendering Path (materi 63)**: Preload adalah alat langsung untuk mempercepat CRP dengan mengambil aset kritis lebih awal
- **async vs defer (materi 64)**: Preload bisa dikombinasikan dengan script yang didefer agar script besar siap tanpa memblokir
- **Level 5 - Multimedia**: Preload digunakan untuk hero image dan video agar LCP lebih cepat

## Soal Latihan
1. Apa perbedaan utama antara `preload` dan `prefetch`?
2. Kapan sebaiknya menggunakan `preconnect`?
3. Mengapa font yang di-preload perlu atribut `crossorigin`?
4. Apa yang terjadi jika kamu menggunakan `preload` tetapi tidak menambahkan atribut `as`?

<details><summary>Jawaban</summary>
1. `preload` memuat aset dengan prioritas tinggi yang dibutuhkan di halaman saat ini. `prefetch` memuat aset dengan prioritas rendah saat browser idle untuk halaman navigasi berikutnya.
2. Saat halaman menggunakan sumber daya dari origin pihak ketiga (CDN, API eksternal, font server) dan ingin menghemat waktu koneksi (DNS + TCP + TLS).
3. Karena font biasanya diambil dari origin berbeda (cross-origin). Tanpa `crossorigin`, browser akan mengabaikan hasil preload dan mendownload font lagi saat ditemukan di CSS.
4. Browser tidak bisa menentukan prioritas aset dengan benar, dan preload mungkin diabaikan. Atribut `as` wajib agar browser tahu cara menangani dan memprioritaskan sumber daya.
</details>
