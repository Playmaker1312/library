# Content Security Policy (CSP)

## Penjelasan

Content Security Policy (CSP) adalah mekanisme keamanan yang membantu mendeteksi dan mengurangi serangan seperti Cross-Site Scripting (XSS) dan data injection. CSP bekerja dengan mendefinisikan sumber daya mana yang diizinkan untuk dimuat dan dijalankan oleh browser. Kebijakan ini dikirimkan melalui header HTTP atau elemen `<meta>`.

## Fungsi

- Mencegah serangan XSS dengan membatasi sumber script yang dapat dijalankan
- Memblokir injeksi konten pihak ketiga yang tidak dikenal
- Mengontrol sumber gambar, style, font, dan frame
- Mencegah clickjacking melalui `frame-ancestors`
- Memberikan lapisan pertahanan tambahan di atas validasi input

## Cara Pengimplementasian

```html
<!-- CSP via meta tag — melarang semua sumber dari luar domain sendiri -->
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'; style-src 'self'; frame-ancestors 'none';">
```

## Analogi (tema RUMAH/BANGUNAN)

CSP bagaikan **pengawas pintu dan jendela sebuah rumah**. Kamu membuat daftar siapa saja yang boleh masuk: hanya keluargamu sendiri (`'self'`). Tukang servis AC hanya boleh masuk ke ruang AC (`script-src`), tukang cat hanya boleh ke dinding (`style-src`). Orang luar sama sekali tidak diizinkan menginjakkan kaki (`frame-ancestors 'none'`). Jika ada orang asing mencoba masuk, pengawas akan langsung mengusirnya — itulah CSP mencegah XSS.

## Dipakai Untuk

- Portal perbankan online yang sangat rentan terhadap XSS
- Dashboard admin yang memproses input pengguna
- Aplikasi web yang menyematkan konten dari CDN pihak ketiga
- Situs e-commerce yang ingin mencegah skimming kartu kredit

## Kesalahan Umum

- Menggunakan `'unsafe-inline'` yang secara tidak sengaja melemahkan kebijakan
- Tidak menyertakan `'self'` sehingga resource lokal ikut terblokir
- Kebijakan terlalu ketat sehingga memblokir resource yang sah (misal Google Fonts)
- Hanya menerapkan CSP via meta tag tanpa headers HTTP (meta tag tidak bisa report-only)
- Lupa menambahkan fallback `default-src` sehingga kebijakan tidak komprehensif

## Koneksi dengan Materi Sebelumnya

- **HTTPS (Level 6)**: CSP melengkapi enkripsi HTTPS dengan mengontrol konten yang dimuat, bukan hanya saluran transmisi
- **Formulir Aman (Level 5)**: Jika form rentan XSS, CSP menjadi jaring pengaman terakhir
- **Atribut sandbox iframe (Materi 68)**: CSP `frame-ancestors` mengontrol dari mana halaman bisa di-frame, sandbox mengontrol apa yang bisa dilakukan iframe di dalamnya

## Soal Latihan

1. Apa yang terjadi jika sebuah halaman memiliki CSP `default-src 'self'` tetapi mencoba memuat gambar dari `https://cdn.example.com`?

2. Sebutkan dua cara mengirimkan CSP ke browser!

3. Mengapa `frame-ancestors 'none'` penting untuk mencegah clickjacking?

<details><summary>Jawaban</summary>

1. Gambar dari `https://cdn.example.com` akan diblokir oleh browser karena domain tersebut tidak termasuk dalam daftar sumber yang diizinkan (`'self'` hanya mengizinkan domain asal halaman).

2. Dua cara: (a) melalui header HTTP `Content-Security-Policy`, dan (b) melalui elemen `<meta http-equiv="Content-Security-Policy" content="...">`.

3. `frame-ancestors 'none'` melarang halaman dimuat di dalam `<frame>`, `<iframe>`, atau `<embed>` dari situs manapun. Ini mencegah penyerang membuat halaman transparan di atas tombol/login page korban (clickjacking).

</details>
