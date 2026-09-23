# Roadmap JavaScript, CSS, dan HTML: Step-by-Step Membangun Aplikasi Web Nyata

## Filosofi Roadmap Ini

> **"HTML adalah struktur, CSS adalah penampilan, JavaScript adalah perilaku — ketiganya bukan tiga hal terpisah, melainkan satu kesatuan yang bekerja bersama membangun pengalaman web yang luar biasa"** — setiap konsep yang dipelajari ada alasannya, bukan sekadar hafal syntax.

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: aplikasi perpustakaan digital dari halaman statis → interaktif → data management → aplikasi penuh
- **Fundamental Sebelum Framework**: kuasai vanilla JS, CSS, HTML sebelum React/Vue — fondasi yang kuat tidak bisa digantikan
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **Web Modern**: fokus pada HTML5, CSS modern (Grid, Flexbox, Custom Properties), dan ES2022+
- **Mengapa sebelum Bagaimana**: pahami alasan di balik setiap keputusan desain

### Prasyarat Sebelum Memulai

text

```
Sebelum roadmap ini, pastikan sudah memahami:
├── Cara menggunakan komputer dan browser dengan nyaman
├── Cara membuat dan mengorganisasi file dan folder
├── Text editor sudah terinstall (VS Code direkomendasikan)
├── Browser modern terinstall (Chrome atau Firefox)
├── Konsep dasar internet: apa itu website, URL, browser
└── Tidak perlu pengalaman programming sama sekali
```

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: "Halaman Pertama" — HTML struktur, teks, link, gambar
    ↓ (enhance)
Level 2: + CSS dasar → Halaman perpustakaan yang cantik
    ↓ (enhance)
Level 3: + CSS Layout (Flexbox, Grid) → Layout yang responsif
    ↓ (enhance)
Level 4: + JavaScript dasar → Halaman yang interaktif
    ↓ (enhance)
Level 5: + DOM Manipulation → Katalog buku dinamis
    ↓ (enhance)
Level 6: + Fetch API, LocalStorage → Data dari API, tersimpan lokal
    ↓ (enhance)
Level 7: + Project Architecture → Aplikasi perpustakaan yang production-ready
```

---

## 🟢 LEVEL 1: FONDASI HTML (Minggu 1-3)

> **Tema**: _"Dari halaman kosong ke dokumen web yang terstruktur"_
> **Benang Merah**: Cara browser bekerja → HTML sebagai struktur → Elemen dan atribut → Semantik → Halaman profil perpustakaan
> **Output**: Halaman profil perpustakaan yang terstruktur dengan HTML

---

### A. Cara Browser Bekerja dan HTML Dasar

> 💡 **Mengapa dimulai di sini?** Sebelum menulis kode, pahami dulu _mengapa_ HTML ada dan _bagaimana_ browser membacanya. Ini mencegah kebingungan "kenapa tag harus ditutup?" atau "apa bedanya div dengan section?"

text

```
Benang Merah Bagian A:
Internet: jaringan komputer yang saling terhubung →
Browser: program yang meminta dan menampilkan halaman web →
Server: komputer yang menyimpan dan mengirim file →
HTML: bahasa untuk mendefinisikan STRUKTUR konten →
Browser membaca HTML dari atas ke bawah, baris per baris →
Tag: instruksi untuk browser cara menampilkan konten
```

#### [[1. Cara Browser Bekerja — Sebelum Menulis Kode]]

- Ketika kamu buka `google.com`, yang terjadi adalah:

text

```
Kamu ketik URL di browser
        ↓
Browser tanya ke DNS: "google.com itu IP berapa?"
        ↓
DNS jawab: "142.250.x.x"
        ↓
Browser kirim HTTP request ke server Google
        ↓
Server kirim balik file HTML, CSS, JavaScript
        ↓
Browser baca HTML → buat DOM (pohon struktur halaman)
Browser baca CSS  → hitung tampilan setiap elemen
Browser baca JS   → jalankan logika interaktif
        ↓
Halaman tampil di layarmu
```

- **Yang perlu dipahami sejak awal:**
  - Browser membaca HTML dari **atas ke bawah**
  - Satu halaman = minimal satu file `.html`
  - CSS dan JavaScript **terpisah** dari HTML (file berbeda)
  - Kamu tidak perlu server untuk belajar — buka file `.html` langsung di browser
- _Langkah konkret_: Buka browser → tekan F12 → lihat tab Elements → ini adalah HTML yang sedang dilihat browser

#### [[2. Setup Environment — VS Code dan Browser]]

text

```
Tools yang dibutuhkan:
├── VS Code: download dari code.visualstudio.com
├── Extension VS Code yang wajib:
│   ├── Live Server (ritwickdey): auto-reload saat file disimpan
│   ├── Prettier: format kode otomatis
│   └── Auto Rename Tag: rename opening/closing tag sekaligus
├── Browser: Chrome atau Firefox (keduanya punya DevTools yang bagus)
└── Tidak butuh terminal atau Node.js untuk Level 1-3
```

text

```
Cara mulai project:
1. Buat folder: perpustakaan-web/
2. Buka folder dengan VS Code: File → Open Folder
3. Buat file: index.html
4. Klik kanan di VS Code → "Open with Live Server"
5. Browser otomatis buka http://127.0.0.1:5500
6. Setiap simpan file → browser auto-refresh
```

#### [[3. Anatomi Dokumen HTML — Boilerplate yang Wajib Dipahami]]

HTML

```
<!DOCTYPE html>
<!-- DOCTYPE: memberitahu browser ini adalah HTML5 (bukan HTML4 atau XML) -->
<!-- SELALU tulis ini di baris pertama setiap file HTML -->

<html lang="id">
<!-- html: elemen root, semua konten ada di dalamnya -->
<!-- lang="id": bahasa halaman (penting untuk screen reader dan SEO) -->

<head>
    <!-- head: informasi TENTANG halaman, tidak ditampilkan di layar -->

    <meta charset="UTF-8">
    <!-- charset: encoding karakter — UTF-8 mendukung semua bahasa termasuk Indonesia -->
    <!-- Tanpa ini: karakter seperti "é", "ü", "ñ" bisa rusak -->

    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- viewport: agar tampilan benar di smartphone -->
    <!-- Tanpa ini: website tampak sangat kecil di HP -->

    <meta name="description" content="Perpustakaan Digital Kota — Temukan buku favoritmu">
    <!-- description: teks yang muncul di hasil pencarian Google -->

    <title>Perpustakaan Digital Kota</title>
    <!-- title: teks yang muncul di tab browser -->

    <!-- CSS akan di-link di sini nanti -->
</head>

<body>
    <!-- body: semua konten yang DITAMPILKAN di layar ada di sini -->

    <h1>Selamat Datang di Perpustakaan Digital</h1>
    <p>Temukan koleksi buku favorit Anda.</p>

</body>
</html>
```

- _Langkah konkret_: Buat file `index.html` dengan kode di atas, buka dengan Live Server — pastikan teks muncul di browser

---

### B. Elemen HTML — Blok Pembangun Halaman

> 💡 **Benang Merah ke A**: Browser sudah bisa baca HTML. Sekarang pelajari elemen-elemen HTML yang tersedia — setiap elemen punya makna dan tujuan spesifik.

text

```
Benang Merah Bagian B:
Browser membaca HTML (A) →
Elemen: tag + konten di dalamnya →
Opening tag <p>, closing tag </p>, self-closing <img> →
Atribut: informasi tambahan dalam opening tag →
Heading: h1-h6 untuk judul dengan hierarki →
Paragraf, list, link, gambar → konten halaman
```

#### [[4. Heading, Paragraf, dan Teks — Konten Dasar]]

HTML

```
<!-- Heading: h1 adalah paling penting, h6 paling kecil -->
<!-- Gunakan hierarki yang benar — jangan skip dari h1 ke h3! -->
<h1>Perpustakaan Digital Kota</h1>           <!-- Judul utama halaman — hanya SATU h1! -->
<h2>Koleksi Buku Pilihan</h2>                <!-- Sub-judul bagian -->
<h3>Kategori: Teknologi</h3>                 <!-- Sub-sub-judul -->
<h4>Buku Bestseller Minggu Ini</h4>          <!-- dan seterusnya... -->

<!-- Paragraf: blok teks -->
<p>
    Selamat datang di Perpustakaan Digital Kota. Kami memiliki koleksi
    lebih dari <strong>12.500 buku</strong> yang siap untuk dipinjam.
    Kunjungi kami setiap hari <em>Senin hingga Sabtu</em>, pukul 08.00-17.00.
</p>

<!-- strong: teks penting (bold secara default) -->
<!-- em: teks yang ditekankan (italic secara default) -->
<!-- Jangan pakai <b> dan <i> — tidak punya makna semantik -->

<!-- Line break: untuk baris baru di dalam paragraf -->
<p>
    Alamat: Jl. Merdeka No. 1<br>
    Kota Bandung, 40111<br>
    Telepon: (022) 123-4567
</p>

<!-- Horizontal rule: garis pemisah -->
<hr>

<!-- Preformatted text: teks dengan spasi dan newline yang dipertahankan -->
<pre>
Jam Operasional:
Senin - Jumat: 08:00 - 17:00
Sabtu:         08:00 - 14:00
Minggu:        Tutup
</pre>

<!-- Blockquote: kutipan dari sumber lain -->
<blockquote cite="https://example.com">
    <p>"Buku adalah jendela dunia yang tidak pernah menutup."</p>
    <footer>— Pepatah Lama</footer>
</blockquote>
```

#### [[5. List — Daftar yang Terstruktur]]

HTML

```
<!-- Unordered list: daftar tanpa urutan (pakai bullet) -->
<h2>Fasilitas Perpustakaan</h2>
<ul>
    <li>Ruang baca yang nyaman</li>
    <li>WiFi gratis</li>
    <li>Komputer akses internet</li>
    <li>Ruang diskusi kelompok</li>
    <li>Kantin dan area istirahat</li>
</ul>

<!-- Ordered list: daftar dengan urutan (pakai angka) -->
<h2>Cara Meminjam Buku</h2>
<ol>
    <li>Daftar sebagai anggota perpustakaan</li>
    <li>Temukan buku yang ingin dipinjam</li>
    <li>Tunjukkan kartu anggota ke petugas</li>
    <li>Buku dipinjam maksimal 14 hari</li>
    <li>Kembalikan tepat waktu untuk menghindari denda</li>
</ol>

<!-- Nested list: list di dalam list -->
<h2>Kategori Koleksi</h2>
<ul>
    <li>Fiksi
        <ul>
            <li>Novel Indonesia</li>
            <li>Novel Terjemahan</li>
            <li>Cerpen</li>
        </ul>
    </li>
    <li>Non-Fiksi
        <ul>
            <li>Sains dan Teknologi</li>
            <li>Sejarah</li>
            <li>Biografi</li>
        </ul>
    </li>
</ul>

<!-- Description list: istilah dan deskripsinya -->
<h2>Denda Keterlambatan</h2>
<dl>
    <dt>1-7 hari</dt>       <!-- dt: description term -->
    <dd>Rp 500/hari</dd>    <!-- dd: description detail -->

    <dt>8-14 hari</dt>
    <dd>Rp 1.000/hari</dd>

    <dt>Lebih dari 14 hari</dt>
    <dd>Rp 2.000/hari + surat peringatan</dd>
</dl>
```

#### [[6. Link dan Gambar — Navigasi dan Visual]]

HTML

```
<!-- Link: elemen yang paling fundamental di web -->
<!-- href: "hypertext reference" — tujuan link -->
<a href="https://perpustakaan.kota.id">Kunjungi Website Resmi</a>

<!-- target="_blank": buka di tab baru (selalu tambahkan rel="noopener") -->
<a href="https://google.com" target="_blank" rel="noopener noreferrer">
    Cari Buku di Google
</a>

<!-- Link ke halaman lain di project yang sama (relative path) -->
<a href="katalog.html">Lihat Katalog Buku</a>
<a href="tentang.html">Tentang Kami</a>
<a href="kontak.html">Hubungi Kami</a>

<!-- Link ke bagian di halaman yang sama (anchor) -->
<a href="#koleksi">Lihat Koleksi</a>
<!-- ... di bagian bawah halaman: -->
<h2 id="koleksi">Koleksi Buku</h2>

<!-- Link email dan telepon -->
<a href="mailto:info@perpustakaan.id">info@perpustakaan.id</a>
<a href="tel:+62221234567">(022) 123-4567</a>

<!-- Gambar -->
<!-- src: sumber gambar | alt: teks alternatif (wajib untuk aksesibilitas!) -->
<img
    src="images/gedung-perpustakaan.jpg"
    alt="Gedung Perpustakaan Digital Kota tampak dari depan"
    width="800"
    height="450"
>
<!-- width dan height: cegah layout shift saat gambar loading -->
<!-- alt yang baik: deskriptif, bukan "gambar" atau "foto" -->

<!-- Gambar sebagai link -->
<a href="index.html">
    <img src="images/logo.png" alt="Logo Perpustakaan Digital Kota" width="200" height="60">
</a>

<!-- Figure dan figcaption: gambar dengan keterangan -->
<figure>
    <img
        src="images/ruang-baca.jpg"
        alt="Ruang baca perpustakaan yang modern dan nyaman"
        width="600"
        height="400"
    >
    <figcaption>Ruang baca utama yang dilengkapi AC dan pencahayaan optimal</figcaption>
</figure>
```

---

### C. HTML Semantik — Makna di Balik Struktur

> 💡 **Mengapa Semantik?** `<div>` bisa membungkus apa saja, tapi tidak punya makna. `<nav>` memberitahu browser, search engine, dan screen reader "ini adalah navigasi". Semantik membuat web lebih accessible dan SEO-friendly.

#### [[7. Elemen Semantik — HTML yang Bermakna]]

HTML

```
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Perpustakaan Digital Kota</title>
</head>
<body>

    <!-- header: bagian atas halaman (logo, judul, navigasi) -->
    <header>
        <a href="index.html">
            <img src="images/logo.png" alt="Perpustakaan Digital Kota" width="150" height="50">
        </a>
        <h1>Perpustakaan Digital Kota</h1>

        <!-- nav: navigasi utama situs -->
        <nav aria-label="Navigasi Utama">
            <ul>
                <li><a href="index.html">Beranda</a></li>
                <li><a href="katalog.html">Katalog</a></li>
                <li><a href="anggota.html">Keanggotaan</a></li>
                <li><a href="tentang.html">Tentang</a></li>
                <li><a href="kontak.html">Kontak</a></li>
            </ul>
        </nav>
    </header>

    <!-- main: konten utama halaman — hanya SATU per halaman -->
    <main>

        <!-- section: bagian tematik dalam halaman -->
        <section id="hero" aria-labelledby="judul-hero">
            <h2 id="judul-hero">Temukan Buku Favoritmu</h2>
            <p>Akses ribuan koleksi buku digital dan fisik secara gratis.</p>
            <a href="katalog.html">Jelajahi Katalog</a>
        </section>

        <section id="statistik" aria-labelledby="judul-statistik">
            <h2 id="judul-statistik">Perpustakaan Kami dalam Angka</h2>
            <!-- article: konten yang bisa berdiri sendiri -->
            <article>
                <h3>12.500+</h3>
                <p>Koleksi Buku</p>
            </article>
            <article>
                <h3>8.200+</h3>
                <p>Anggota Aktif</p>
            </article>
            <article>
                <h3>350+</h3>
                <p>Peminjaman Per Hari</p>
            </article>
        </section>

        <section id="buku-terbaru" aria-labelledby="judul-terbaru">
            <h2 id="judul-terbaru">Buku Terbaru</h2>

            <!-- article: setiap buku adalah konten yang berdiri sendiri -->
            <article class="kartu-buku">
                <figure>
                    <img src="images/buku-1.jpg" alt="Sampul buku Clean Code">
                    <figcaption>Clean Code</figcaption>
                </figure>
                <h3>Clean Code</h3>
                <p>Robert C. Martin</p>
                <p>Kategori: Teknologi</p>
                <a href="buku/clean-code.html">Lihat Detail</a>
            </article>

            <article class="kartu-buku">
                <figure>
                    <img src="images/buku-2.jpg" alt="Sampul buku Laskar Pelangi">
                </figure>
                <h3>Laskar Pelangi</h3>
                <p>Andrea Hirata</p>
                <p>Kategori: Fiksi</p>
                <a href="buku/laskar-pelangi.html">Lihat Detail</a>
            </article>

        </section>

    </main>

    <!-- aside: konten pendukung (sidebar, info tambahan) -->
    <aside aria-label="Informasi Tambahan">
        <section>
            <h2>Jam Operasional</h2>
            <dl>
                <dt>Senin - Jumat</dt>
                <dd>08:00 - 17:00</dd>
                <dt>Sabtu</dt>
                <dd>08:00 - 14:00</dd>
                <dt>Minggu & Hari Libur</dt>
                <dd>Tutup</dd>
            </dl>
        </section>

        <section>
            <h2>Pengumuman</h2>
            <article>
                <h3>Koleksi Baru Bulan Ini</h3>
                <p>250 judul buku baru telah ditambahkan ke koleksi kami.</p>
                <a href="pengumuman.html">Baca Selengkapnya</a>
            </article>
        </section>
    </aside>

    <!-- footer: bagian bawah halaman -->
    <footer>
        <p>&copy; 2024 Perpustakaan Digital Kota. Hak cipta dilindungi.</p>
        <address>
            Jl. Merdeka No. 1, Kota Bandung |
            <a href="mailto:info@perpustakaan.id">info@perpustakaan.id</a> |
            <a href="tel:+62221234567">(022) 123-4567</a>
        </address>
        <nav aria-label="Navigasi Footer">
            <a href="kebijakan-privasi.html">Kebijakan Privasi</a>
            <a href="syarat-ketentuan.html">Syarat & Ketentuan</a>
            <a href="sitemap.html">Peta Situs</a>
        </nav>
    </footer>

</body>
</html>
```

---

### D. Form HTML — Menerima Input dari Pengguna

#### [[8. Form dan Input — Interaksi dengan Pengguna]]

HTML

```
<!-- Form pendaftaran anggota perpustakaan -->
<!-- action: ke mana data dikirim | method: GET atau POST -->
<!-- Untuk saat ini: action="#" karena belum ada backend -->
<form action="#" method="POST" novalidate>

    <!-- fieldset: grup field yang terkait -->
    <fieldset>
        <legend>Informasi Pribadi</legend>

        <!-- label: teks keterangan untuk input — WAJIB untuk aksesibilitas -->
        <!-- for harus sama dengan id input yang terkait -->
        <div class="form-group">
            <label for="nama-lengkap">Nama Lengkap *</label>
            <input
                type="text"
                id="nama-lengkap"
                name="nama"
                placeholder="Contoh: Budi Santoso"
                required
                autocomplete="name"
            >
        </div>

        <div class="form-group">
            <label for="email">Alamat Email *</label>
            <input
                type="email"
                id="email"
                name="email"
                placeholder="budi@email.com"
                required
                autocomplete="email"
            >
        </div>

        <div class="form-group">
            <label for="telepon">Nomor Telepon</label>
            <input
                type="tel"
                id="telepon"
                name="telepon"
                placeholder="08123456789"
                pattern="[0-9]{10,13}"
                autocomplete="tel"
            >
        </div>

        <div class="form-group">
            <label for="tanggal-lahir">Tanggal Lahir *</label>
            <input
                type="date"
                id="tanggal-lahir"
                name="tanggal_lahir"
                required
            >
        </div>
    </fieldset>

    <fieldset>
        <legend>Preferensi Buku</legend>

        <!-- Select dropdown -->
        <div class="form-group">
            <label for="kategori-favorit">Kategori Favorit</label>
            <select id="kategori-favorit" name="kategori">
                <option value="">-- Pilih Kategori --</option>
                <option value="fiksi">Fiksi</option>
                <option value="non-fiksi">Non-Fiksi</option>
                <option value="sains">Sains & Teknologi</option>
                <option value="sejarah">Sejarah</option>
                <option value="biografi">Biografi</option>
            </select>
        </div>

        <!-- Radio button: pilih satu dari banyak -->
        <div class="form-group">
            <p>Jenis Keanggotaan *</p>
            <label>
                <input type="radio" name="jenis-anggota" value="reguler" checked>
                Reguler (gratis)
            </label>
            <label>
                <input type="radio" name="jenis-anggota" value="premium">
                Premium (Rp 50.000/tahun)
            </label>
        </div>

        <!-- Checkbox: pilih banyak -->
        <div class="form-group">
            <p>Notifikasi yang diinginkan:</p>
            <label>
                <input type="checkbox" name="notif[]" value="buku-baru">
                Buku baru tersedia
            </label>
            <label>
                <input type="checkbox" name="notif[]" value="jatuh-tempo">
                Pengingat jatuh tempo
            </label>
            <label>
                <input type="checkbox" name="notif[]" value="promo">
                Promosi dan event
            </label>
        </div>
    </fieldset>

    <fieldset>
        <legend>Pesan Tambahan</legend>

        <!-- Textarea: input teks multi-baris -->
        <div class="form-group">
            <label for="pesan">Pertanyaan atau Pesan (opsional)</label>
            <textarea
                id="pesan"
                name="pesan"
                rows="5"
                cols="50"
                maxlength="500"
                placeholder="Tulis pertanyaan atau permintaan khusus di sini..."
            ></textarea>
        </div>
    </fieldset>

    <!-- Submit button -->
    <button type="submit">Daftar Sekarang</button>
    <button type="reset">Reset Form</button>

</form>
```

---

### 🏗️ Checkpoint Level 1

text

```
✅ Checklist sebelum lanjut ke Level 2:

PEMAHAMAN:
├── Bisa jelaskan alur: browser → request → server → HTML → tampilan
├── Bisa jelaskan perbedaan <head> vs <body>
├── Bisa jelaskan perbedaan elemen semantik vs <div>
├── Bisa jelaskan kapan pakai <section> vs <article> vs <aside>
└── Bisa jelaskan mengapa alt pada <img> penting

PROYEK: Website Perpustakaan Multi-Halaman
├── index.html: beranda dengan header, nav, main, footer semantik
├── katalog.html: daftar buku dalam article elements
├── tentang.html: profil perpustakaan
├── kontak.html: form kontak dengan semua jenis input
└── Navigasi antar halaman berfungsi dengan link

KEBIASAAN:
├── Selalu tulis DOCTYPE, charset, viewport
├── Hanya satu <h1> per halaman
├── Semua <img> punya alt yang deskriptif
├── Semua <input> punya <label> yang terhubung
└── Validasi HTML di validator.w3.org — 0 error

Git: feat: create multi-page library website with semantic HTML
```

---

## 🔵 LEVEL 2: CSS DASAR (Minggu 3-6)

> **Tema**: _"Dari dokumen hitam putih ke halaman web yang menarik secara visual"_
> **Benang Merah**: HTML memberikan struktur (Level 1) → CSS memberikan tampilan → Selector memilih elemen → Properties mengubah tampilan → Cascade dan Specificity
> **Output**: Halaman perpustakaan dengan warna, tipografi, dan spacing yang konsisten

---

### E. Cara CSS Bekerja — Fondasi

> 💡 **Mengapa dimulai dengan cara kerja?** CSS punya perilaku yang tidak intuitif (cascade, specificity, inheritance). Memahami ini dari awal mencegah frustasi "kenapa style-ku tidak mau jalan!"

text

```
Benang Merah Bagian E:
HTML memberikan struktur tanpa tampilan (Level 1) →
CSS: bahasa untuk mendeskripsikan TAMPILAN elemen →
Selector: cara memilih elemen mana yang akan di-style →
Declaration: property dan nilai yang diterapkan →
Cascade: aturan mana yang menang jika ada konflik →
Specificity: seberapa "spesifik" sebuah selector
```

#### [[9. Cara Menghubungkan CSS ke HTML]]

HTML

```
<!-- Cara 1: External CSS — SELALU gunakan ini (rekomendasi) -->
<!-- Di dalam <head>: -->
<link rel="stylesheet" href="css/style.css">

<!-- Cara 2: Internal CSS — hanya untuk prototyping cepat -->
<style>
    h1 { color: red; }
</style>

<!-- Cara 3: Inline CSS — HINDARI! Sangat susah dimaintain -->
<h1 style="color: red;">Judul</h1>

<!--
Mengapa external CSS selalu lebih baik?
├── Satu file CSS bisa dipakai banyak halaman HTML
├── Browser meng-cache CSS → halaman load lebih cepat
├── Mudah dimaintain: ubah satu tempat, berlaku di semua halaman
└── Pemisahan concerns yang bersih: HTML = struktur, CSS = tampilan
-->
```

text

```
Struktur folder yang direkomendasikan:
perpustakaan-web/
├── index.html
├── katalog.html
├── tentang.html
├── kontak.html
├── css/
│   ├── style.css         ← stylesheet utama
│   ├── reset.css         ← normalize browser default styles
│   └── components.css    ← style komponen (kartu buku, form, dll)
├── js/
│   └── main.js           ← JavaScript (nanti)
└── images/
    ├── logo.png
    └── buku/
        └── *.jpg
```

#### [[10. Selector CSS — Memilih Elemen yang Tepat]]

CSS

```
/* css/style.css */

/* ─── Selector Dasar ─────────────────────────────────────────────────── */

/* Element selector: pilih semua elemen dengan tag tertentu */
h1 { color: #2c3e50; }
p  { color: #555555; }
a  { color: #3498db; }

/* Class selector: pilih elemen dengan class tertentu */
/* Diawali titik (.) */
.kartu-buku   { background: white; }
.btn-primary  { background: #3498db; }
.teks-merah   { color: red; }   /* Hindari class seperti ini! */
                                /* Nama class harus semantik, bukan deskripsi tampilan */

/* ID selector: pilih elemen dengan id tertentu */
/* Diawali tanda pagar (#) */
/* Gunakan ID untuk JavaScript, bukan CSS — ID lebih baik untuk JS hooks */
#form-pendaftaran { max-width: 600px; }

/* Universal selector: pilih SEMUA elemen */
* {
    box-sizing: border-box; /* akan dijelaskan di Level 3 */
    margin: 0;
    padding: 0;
}

/* ─── Selector Kombinasi ─────────────────────────────────────────────── */

/* Descendant: pilih .judul-buku yang ada DI DALAM .kartu-buku */
.kartu-buku .judul-buku { font-size: 1.2rem; }

/* Child (>): hanya ANAK LANGSUNG */
nav > ul { list-style: none; }
nav > ul > li { display: inline-block; }

/* Adjacent sibling (+): elemen TEPAT setelah elemen lain */
h2 + p { font-size: 1.1rem; }  /* paragraf langsung setelah h2 */

/* General sibling (~): semua elemen setelah elemen lain */
h2 ~ p { color: #666; }

/* ─── Attribute Selector ─────────────────────────────────────────────── */

/* Elemen dengan attribute tertentu */
input[required]         { border-left: 3px solid red; }
a[target="_blank"]      { padding-right: 16px; }          /* tambah icon external */
a[href^="mailto:"]      { /* link email */ }              /* href dimulai dengan */
a[href$=".pdf"]         { /* link ke PDF */ }             /* href diakhiri dengan */
img[alt=""]             { outline: 2px solid red; }       /* gambar tanpa alt! */

/* ─── Pseudo-class: kondisi elemen ──────────────────────────────────── */

a:hover          { color: #2980b9; text-decoration: underline; }
a:focus          { outline: 2px solid #3498db; outline-offset: 2px; }
a:visited        { color: #8e44ad; }
button:disabled  { opacity: 0.5; cursor: not-allowed; }
input:focus      { border-color: #3498db; box-shadow: 0 0 0 3px rgba(52,152,219,0.2); }
input:invalid    { border-color: #e74c3c; }
input:valid      { border-color: #2ecc71; }

li:first-child   { font-weight: bold; }    /* list item pertama */
li:last-child    { border-bottom: none; }  /* list item terakhir */
li:nth-child(2)  { background: #f0f0f0; } /* item ke-2 */
li:nth-child(odd)  { background: #f9f9f9; } /* item ganjil */
li:nth-child(even) { background: white; }   /* item genap */

/* ─── Pseudo-element: bagian dari elemen ────────────────────────────── */

p::first-line    { font-weight: bold; }          /* baris pertama paragraf */
p::first-letter  { font-size: 2em; float: left; } /* huruf pertama (drop cap) */

/* ::before dan ::after: konten dekoratif */
.wajib::after {
    content: " *";
    color: red;
}

a[target="_blank"]::after {
    content: " ↗";
    font-size: 0.8em;
}
```

#### [[11. Cascade, Specificity, dan Inheritance — Aturan yang Menentukan Pemenang]]

CSS

```
/*
SPECIFICITY: seberapa spesifik sebuah selector
Dihitung sebagai (A, B, C) dimana:
A = jumlah ID selector (#)
B = jumlah class/attribute/pseudo-class
C = jumlah element/pseudo-element

Contoh:
h1              → (0,0,1) → specificity: 1
.judul          → (0,1,0) → specificity: 10
#header         → (1,0,0) → specificity: 100
h1.judul        → (0,1,1) → specificity: 11
#header h1      → (1,0,1) → specificity: 101
!important      → menang dari semua (hindari!)
*/

/* Contoh nyata: */
h1 { color: blue; }           /* specificity: 1 */
.judul { color: red; }        /* specificity: 10 → MENANG → h1 merah */
#header .judul { color: green; } /* specificity: 110 → MENANG → h1 hijau */

/*
INHERITANCE: beberapa property diwariskan ke child elements
Property yang diwariskan: color, font-family, font-size, line-height, dll
Property yang TIDAK diwariskan: margin, padding, border, background, width, dll
*/

body {
    color: #333;            /* diwariskan ke semua child */
    font-family: sans-serif; /* diwariskan ke semua child */
    background: white;      /* TIDAK diwariskan */
}

/* Paksa inherit atau reset */
.reset-color { color: inherit; }   /* ambil warna dari parent */
.reset-color { color: initial; }   /* kembali ke default browser */

/*
CASCADE ORDER (dari terendah ke tertinggi):
1. Browser default styles
2. External CSS (style.css)
3. Internal CSS (<style> tag)
4. Inline CSS (style="...")
5. !important (hindari kecuali terpaksa!)
*/
```

---

### F. Properties CSS — Mengubah Tampilan Elemen

#### [[12. Warna dan Background]]

CSS

```
/* css/style.css */

/* ─── CSS Custom Properties (Variables) — gunakan ini dari awal! ─────── */
:root {
    /* Warna brand */
    --warna-utama:    #2563eb;    /* biru */
    --warna-sekunder: #059669;    /* hijau */
    --warna-aksen:    #dc2626;    /* merah */

    /* Warna teks */
    --teks-utama:     #1e293b;
    --teks-sekunder:  #64748b;
    --teks-terbalik:  #ffffff;

    /* Warna background */
    --bg-utama:       #ffffff;
    --bg-sekunder:    #f8fafc;
    --bg-gelap:       #1e293b;

    /* Border */
    --border-warna:   #e2e8f0;
    --border-radius:  8px;

    /* Spacing */
    --spasi-xs:  4px;
    --spasi-sm:  8px;
    --spasi-md:  16px;
    --spasi-lg:  24px;
    --spasi-xl:  32px;
    --spasi-2xl: 48px;
    --spasi-3xl: 64px;
}

/* Gunakan variabel: */
.btn-utama {
    background-color: var(--warna-utama);
    color: var(--teks-terbalik);
    border-radius: var(--border-radius);
}

/* ─── Format warna ───────────────────────────────────────────────────── */
.contoh-warna {
    /* Named color */
    color: red;
    color: cornflowerblue;

    /* Hex: #RRGGBB atau #RGB */
    color: #2563eb;
    color: #369; /* sama dengan #336699 */

    /* RGB dan RGBA */
    color: rgb(37, 99, 235);
    color: rgba(37, 99, 235, 0.8); /* 0 = transparan, 1 = solid */

    /* HSL: Hue (0-360), Saturation (%), Lightness (%) */
    color: hsl(220, 91%, 54%);
    color: hsla(220, 91%, 54%, 0.8);

    /* Modern: oklch, oklcH, dll (CSS Color Level 4) */
}

/* ─── Background ─────────────────────────────────────────────────────── */
.hero-section {
    /* Warna solid */
    background-color: var(--warna-utama);

    /* Gradient linear */
    background-image: linear-gradient(
        135deg,
        #2563eb 0%,
        #059669 100%
    );

    /* Gradient radial */
    background-image: radial-gradient(
        circle at center,
        #2563eb 0%,
        #1e293b 100%
    );

    /* Gambar background */
    background-image: url('../images/hero-bg.jpg');
    background-size: cover;        /* tutupi seluruh elemen */
    background-position: center;   /* posisi gambar */
    background-repeat: no-repeat;  /* jangan ulangi gambar */
    background-attachment: fixed;  /* parallax effect */

    /* Overlay: gradient + gambar (pattern yang sangat berguna) */
    background-image:
        linear-gradient(rgba(0,0,0,0.5), rgba(0,0,0,0.5)),
        url('../images/hero-bg.jpg');
}
```

#### [[13. Tipografi — Teks yang Enak Dibaca]]

CSS

```
/* css/style.css */

/* Import Google Fonts */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Merriweather:ital,wght@0,400;0,700;1,400&display=swap');

:root {
    /* Font families */
    --font-sans:  'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
    --font-serif: 'Merriweather', Georgia, serif;
    --font-mono:  'Courier New', Courier, monospace;

    /* Font sizes — menggunakan skala yang konsisten */
    --teks-xs:   0.75rem;    /* 12px */
    --teks-sm:   0.875rem;   /* 14px */
    --teks-base: 1rem;       /* 16px — default browser */
    --teks-lg:   1.125rem;   /* 18px */
    --teks-xl:   1.25rem;    /* 20px */
    --teks-2xl:  1.5rem;     /* 24px */
    --teks-3xl:  1.875rem;   /* 30px */
    --teks-4xl:  2.25rem;    /* 36px */
    --teks-5xl:  3rem;       /* 48px */

    /* Line heights */
    --leading-tight:  1.25;
    --leading-normal: 1.5;
    --leading-loose:  1.75;
}

/* Reset dan base styles */
html {
    font-size: 16px;  /* base font size — 1rem = 16px */
    scroll-behavior: smooth;  /* smooth scroll saat klik anchor link */
}

body {
    font-family: var(--font-sans);
    font-size: var(--teks-base);
    line-height: var(--leading-normal);
    color: var(--teks-utama);
    background-color: var(--bg-utama);
}

/* Heading styles */
h1, h2, h3, h4, h5, h6 {
    font-family: var(--font-sans);
    font-weight: 700;
    line-height: var(--leading-tight);
    color: var(--teks-utama);
    margin-bottom: var(--spasi-md);
}

h1 { font-size: var(--teks-4xl); }
h2 { font-size: var(--teks-3xl); }
h3 { font-size: var(--teks-2xl); }
h4 { font-size: var(--teks-xl); }
h5 { font-size: var(--teks-lg); }
h6 { font-size: var(--teks-base); }

p {
    margin-bottom: var(--spasi-md);
    max-width: 65ch;  /* optimal: 60-75 karakter per baris */
}

/* Link styles */
a {
    color: var(--warna-utama);
    text-decoration: none;
    transition: color 0.2s ease;
}
a:hover { color: #1d4ed8; text-decoration: underline; }
a:focus-visible {
    outline: 2px solid var(--warna-utama);
    outline-offset: 2px;
    border-radius: 2px;
}

/* Typography utilities */
.teks-sm    { font-size: var(--teks-sm); }
.teks-lg    { font-size: var(--teks-lg); }
.teks-bold  { font-weight: 700; }
.teks-muted { color: var(--teks-sekunder); }
.teks-center { text-align: center; }
.teks-upper { text-transform: uppercase; letter-spacing: 0.05em; }
```

#### [[14. Box Model — Memahami Ruang Setiap Elemen]]

CSS

```
/*
BOX MODEL: setiap elemen HTML adalah kotak dengan 4 lapisan:

┌─────────────────────────────────┐  ← margin (ruang di luar border)
│  ┌───────────────────────────┐  │
│  │         BORDER            │  │
│  │  ┌─────────────────────┐  │  │
│  │  │       PADDING       │  │  │
│  │  │  ┌───────────────┐  │  │  │
│  │  │  │    CONTENT    │  │  │  │
│  │  │  │  (width x     │  │  │  │
│  │  │  │   height)     │  │  │  │
│  │  │  └───────────────┘  │  │  │
│  │  └─────────────────────┘  │  │
│  └───────────────────────────┘  │
└─────────────────────────────────┘

PERBEDAAN KRITIS:
- margin: ruang di LUAR elemen (tidak punya background)
- padding: ruang di DALAM elemen (punya background)
- border: garis di antara margin dan padding
*/

/* box-sizing: border-box — SELALU set ini! */
/* Default: width hanya menghitung content */
/* box-sizing: border-box: width menghitung content + padding + border */
* {
    box-sizing: border-box;
}

.kartu-buku {
    /* Content */
    width: 300px;
    height: auto;       /* auto: menyesuaikan konten */
    min-height: 200px;
    max-width: 100%;    /* tidak melebihi container */

    /* Padding: ruang dalam */
    padding: 24px;                  /* semua sisi */
    padding: 16px 24px;             /* atas-bawah | kiri-kanan */
    padding: 8px 16px 24px 32px;    /* atas | kanan | bawah | kiri (searah jarum jam) */
    padding-top: 16px;
    padding-right: 24px;
    padding-bottom: 16px;
    padding-left: 24px;

    /* Border */
    border: 1px solid var(--border-warna);
    border-radius: var(--border-radius);
    border-top: 3px solid var(--warna-utama); /* border hanya atas */

    /* Margin: ruang luar */
    margin: 0;              /* hapus margin */
    margin: 0 auto;         /* center horizontal (perlu width yang tetap) */
    margin-bottom: 24px;    /* jarak ke elemen di bawah */

    /* Background */
    background-color: var(--bg-utama);

    /* Shadow */
    box-shadow: 0 1px 3px rgba(0,0,0,0.1), 0 1px 2px rgba(0,0,0,0.06);

    /* Overflow: apa yang terjadi jika konten meluap */
    overflow: hidden;    /* sembunyikan konten yang meluap */
    overflow: auto;      /* tambah scrollbar jika perlu */
    overflow: visible;   /* default: konten tetap tampil walau meluap */

    /* Transisi: animasi halus saat property berubah */
    transition: box-shadow 0.2s ease, transform 0.2s ease;
}

.kartu-buku:hover {
    box-shadow: 0 10px 25px rgba(0,0,0,0.1);
    transform: translateY(-2px);  /* naik sedikit saat hover */
}

/* Margin collapsing: hal yang sering membingungkan! */
/* Ketika dua vertical margin bertemu, yang dipakai adalah yang LEBIH BESAR */
h2 { margin-bottom: 24px; }
p  { margin-top: 16px; }
/* Antara h2 dan p, jarak yang dihasilkan: 24px (bukan 24+16=40px!) */
/* Solusi: gunakan padding, atau hanya satu margin (bottom saja) */
```

---

### 🏗️ Checkpoint Level 2

text

```
✅ Checklist sebelum lanjut ke Level 3:

PROYEK: Halaman Perpustakaan yang Cantik
├── css/style.css: CSS Variables, reset, tipografi, warna
├── index.html: hero section dengan gradient, kartu statistik
├── katalog.html: kartu buku dengan hover effect
├── kontak.html: form yang di-style dengan benar
└── Semua halaman tampil konsisten

PEMAHAMAN:
├── Bisa jelaskan perbedaan margin vs padding
├── Bisa jelaskan cara kerja box-sizing: border-box
├── Bisa jelaskan cascade dan specificity dengan contoh
├── Bisa jelaskan mengapa CSS Custom Properties (--var) lebih baik
└── Bisa jelaskan perbedaan em vs rem vs px

BROWSER DEVTOOLS:
├── Bisa inspect elemen dan lihat computed styles
├── Bisa edit CSS langsung di DevTools untuk eksperimen
└── Bisa lihat box model diagram di DevTools

Git: feat: add CSS styling with variables, typography, and box model
```

---

## 🟡 LEVEL 3: CSS LAYOUT — FLEXBOX, GRID, DAN RESPONSIVE (Minggu 6-10)

> **Tema**: _"Dari elemen yang menumpuk ke layout yang sophisticated dan responsif"_
> **Benang Merah**: Styling individual (Level 2) → Flexbox untuk komponen 1D → Grid untuk layout 2D → Media queries untuk responsif → Mobile-first approach
> **Output**: Layout perpustakaan yang responsif sempurna di semua ukuran layar

---

### G. Flexbox — Layout Satu Dimensi

> 💡 **Mengapa Flexbox?** Sebelum Flexbox, membuat elemen berjejer horizontal atau memvertikal-tengahkan sesuatu adalah mimpi buruk. Flexbox memecahkan ini dengan elegan. Gunakan Flexbox untuk **komponen** (navbar, kartu, form row).

text

```
Benang Merah Bagian G:
Elemen block selalu menumpuk vertikal (Level 2) →
Flexbox: ubah cara elemen tersusun dalam satu baris/kolom →
flex-direction: baris atau kolom →
justify-content: distribusi sepanjang sumbu utama →
align-items: perataan sepanjang sumbu silang →
flex-wrap: biarkan item pindah baris jika tidak muat
```

#### [[15. Flexbox Dasar — Container dan Items]]

CSS

```
/* FLEXBOX MENTAL MODEL:
   Flex Container: parent yang punya display: flex
   Flex Items: semua direct children dari container

   Dua sumbu:
   Main axis: arah utama (default: horizontal, kiri ke kanan)
   Cross axis: arah tegak lurus (default: vertikal)
*/

/* ─── Flex Container Properties ─────────────────────────────────────── */
.navbar {
    display: flex;                      /* aktifkan flexbox */

    /* flex-direction: arah sumbu utama */
    flex-direction: row;                /* default: kiri ke kanan */
    flex-direction: row-reverse;        /* kanan ke kiri */
    flex-direction: column;             /* atas ke bawah */
    flex-direction: column-reverse;     /* bawah ke atas */

    /* justify-content: distribusi items di SUMBU UTAMA */
    justify-content: flex-start;        /* default: kiri */
    justify-content: flex-end;          /* kanan */
    justify-content: center;            /* tengah */
    justify-content: space-between;     /* ujung ke ujung, space rata */
    justify-content: space-around;      /* space sama di kiri-kanan setiap item */
    justify-content: space-evenly;      /* space benar-benar sama rata */

    /* align-items: perataan di SUMBU SILANG */
    align-items: stretch;               /* default: regangkan sesuai container */
    align-items: flex-start;            /* rata atas */
    align-items: flex-end;              /* rata bawah */
    align-items: center;                /* tengah vertikal — sangat berguna! */
    align-items: baseline;              /* rata baseline teks */

    /* flex-wrap: boleh pindah baris? */
    flex-wrap: nowrap;                  /* default: paksa satu baris */
    flex-wrap: wrap;                    /* pindah baris jika tidak muat */
    flex-wrap: wrap-reverse;            /* pindah baris ke atas */

    /* gap: jarak antar items */
    gap: 16px;                          /* semua sisi */
    gap: 16px 24px;                     /* baris | kolom */
    row-gap: 16px;
    column-gap: 24px;
}

/* ─── Flex Item Properties ───────────────────────────────────────────── */
.nav-item {
    /* flex-grow: boleh membesar untuk isi space kosong? */
    flex-grow: 0;       /* default: tidak membesar */
    flex-grow: 1;       /* membesar mengisi space kosong */

    /* flex-shrink: boleh mengecil jika space tidak cukup? */
    flex-shrink: 1;     /* default: boleh mengecil */
    flex-shrink: 0;     /* tidak boleh mengecil */

    /* flex-basis: ukuran awal sebelum grow/shrink */
    flex-basis: auto;   /* default: sesuai konten */
    flex-basis: 200px;  /* mulai dari 200px */
    flex-basis: 0;      /* mulai dari nol */

    /* Shorthand: flex: grow shrink basis */
    flex: 0 1 auto;     /* default */
    flex: 1;            /* sama dengan: flex: 1 1 0 (item mengisi space rata) */
    flex: none;         /* sama dengan: flex: 0 0 auto (tidak flex sama sekali) */

    /* align-self: override align-items untuk item ini */
    align-self: center;

    /* order: urutan tampilan (default: 0) */
    order: -1;  /* tampil pertama */
    order: 1;   /* tampil terakhir */
}
```

CSS

```
/* ─── Pola Flexbox yang Paling Sering Dipakai ───────────────────────── */

/* 1. Navbar dengan logo kiri, menu kanan */
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 var(--spasi-xl);
    height: 64px;
    background: var(--bg-utama);
    border-bottom: 1px solid var(--border-warna);
}

.navbar__logo { flex-shrink: 0; }  /* logo tidak boleh mengecil */
.navbar__menu { display: flex; gap: var(--spasi-lg); list-style: none; }

/* 2. Centered vertically and horizontally */
.hero-section {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 500px;
    flex-direction: column;
    text-align: center;
    gap: var(--spasi-lg);
}

/* 3. Kartu dengan footer yang selalu di bawah */
.kartu-buku {
    display: flex;
    flex-direction: column;
    height: 100%;   /* perlu container dengan height tetap */
}

.kartu-buku__konten { flex: 1; }   /* konten mengisi space */
.kartu-buku__footer { flex-shrink: 0; }  /* footer selalu di bawah */
```

#### [[16. CSS Grid — Layout yang Powerful]]

CSS

```
/* GRID MENTAL MODEL:
   Grid Container: parent dengan display: grid
   Grid Items: direct children container
   Grid Lines: garis horizontal (row) dan vertikal (column)
   Grid Track: ruang antara dua grid lines
   Grid Cell: satu "kotak" di grid
   Grid Area: satu atau lebih cells yang digabung
*/

/* ─── Grid Container ─────────────────────────────────────────────────── */
.katalog-grid {
    display: grid;

    /* grid-template-columns: definisikan kolom */
    grid-template-columns: 200px 200px 200px;        /* 3 kolom, masing-masing 200px */
    grid-template-columns: 1fr 1fr 1fr;              /* 3 kolom sama rata (fr = fraction) */
    grid-template-columns: repeat(3, 1fr);           /* shortcut untuk di atas */
    grid-template-columns: 250px 1fr;                /* sidebar + main content */
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); /* responsive otomatis! */

    /* grid-template-rows: definisikan baris */
    grid-template-rows: auto;                        /* default: sesuai konten */
    grid-template-rows: 64px 1fr auto;               /* header | main | footer */

    /* gap */
    gap: 24px;
    column-gap: 24px;
    row-gap: 16px;

    /* justify-items: perataan items horizontal dalam cell */
    justify-items: stretch;   /* default */
    justify-items: center;

    /* align-items: perataan items vertikal dalam cell */
    align-items: stretch;     /* default */
    align-items: center;
}

/* ─── Grid Item ──────────────────────────────────────────────────────── */
.featured-book {
    /* grid-column: mulai dari line mana, sampai line mana */
    grid-column: 1 / 3;        /* dari line 1 sampai line 3 (span 2 kolom) */
    grid-column: 1 / -1;       /* dari line 1 sampai line terakhir (full width) */
    grid-column: span 2;       /* ambil 2 kolom dari posisi saat ini */

    /* grid-row: sama tapi untuk baris */
    grid-row: 1 / 3;           /* ambil 2 baris */
    grid-row: span 2;
}

/* ─── Named Grid Areas — layout yang mudah dibaca ───────────────────── */
.halaman-utama {
    display: grid;
    grid-template-columns: 240px 1fr;
    grid-template-rows: 64px 1fr auto;
    grid-template-areas:
        "header  header"
        "sidebar main  "
        "footer  footer";
    min-height: 100vh;
}

/* Assign elemen ke named area */
.site-header  { grid-area: header; }
.site-sidebar { grid-area: sidebar; }
.site-main    { grid-area: main; }
.site-footer  { grid-area: footer; }
```

CSS

```
/* ─── Pola Grid yang Paling Berguna ─────────────────────────────────── */

/* 1. Responsive card grid — tanpa media query! */
.grid-kartu {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    /* auto-fill: isi sebanyak mungkin kolom
       minmax(280px, 1fr): setiap kolom min 280px, max 1fr
       Hasil: responsive secara otomatis! */
    gap: var(--spasi-lg);
}

/* 2. Holy Grail Layout */
.holy-grail {
    display: grid;
    grid-template-columns: 200px 1fr 200px;
    grid-template-rows: auto 1fr auto;
    grid-template-areas:
        "header  header  header "
        "sidebar main    aside  "
        "footer  footer  footer ";
    min-height: 100vh;
}

/* 3. Magazine layout */
.majalah {
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    grid-template-rows: repeat(4, 200px);
    gap: 16px;
}

.artikel-utama {
    grid-column: 1 / 4;
    grid-row: 1 / 3;
}

.artikel-kedua {
    grid-column: 4 / 7;
    grid-row: 1 / 2;
}
```

---

### I. Responsive Design — Mobile First

> 💡 **Mengapa Mobile First?** Lebih dari 60% traffic web berasal dari mobile. Mobile first berarti: desain untuk layar kecil dulu, lalu tambahkan complexity untuk layar besar. Ini lebih mudah daripada sebaliknya.

#### [[17. Media Queries dan Responsive Design]]

CSS

```
/* BREAKPOINTS yang umum dipakai: */
:root {
    /* Gunakan em untuk breakpoints (lebih reliable dari px) */
    /* 1em = 16px saat default */
}

/*
Mobile First: tulis CSS untuk mobile dulu,
lalu tambahkan min-width untuk layar lebih besar

Breakpoints:
< 640px   → mobile (default, tanpa media query)
≥ 640px   → small tablet (sm)
≥ 768px   → tablet (md)
≥ 1024px  → laptop (lg)
≥ 1280px  → desktop (xl)
≥ 1536px  → large desktop (2xl)
*/

/* ─── Mobile First Approach ─────────────────────────────────────────── */

/* Default (mobile): satu kolom */
.grid-katalog {
    display: grid;
    grid-template-columns: 1fr;
    gap: var(--spasi-md);
}

/* Small tablet (≥ 640px): dua kolom */
@media (min-width: 640px) {
    .grid-katalog {
        grid-template-columns: repeat(2, 1fr);
        gap: var(--spasi-lg);
    }
}

/* Laptop (≥ 1024px): tiga kolom */
@media (min-width: 1024px) {
    .grid-katalog {
        grid-template-columns: repeat(3, 1fr);
    }
}

/* Desktop (≥ 1280px): empat kolom */
@media (min-width: 1280px) {
    .grid-katalog {
        grid-template-columns: repeat(4, 1fr);
    }
}

/* ─── Responsive Navbar ───────────────────────────────────────────────  */

/* Mobile: hamburger menu (JS akan handle toggle) */
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: var(--spasi-md) var(--spasi-lg);
}

.navbar__menu {
    display: none;      /* sembunyikan di mobile */
    flex-direction: column;
    position: absolute;
    top: 64px;
    left: 0;
    right: 0;
    background: white;
    padding: var(--spasi-md);
    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

.navbar__menu.aktif {
    display: flex;      /* tampilkan saat hamburger diklik (JS) */
}

.navbar__toggle {
    display: block;     /* tampilkan tombol hamburger di mobile */
}

/* Desktop: menu horizontal */
@media (min-width: 768px) {
    .navbar__menu {
        display: flex;          /* selalu tampil di desktop */
        flex-direction: row;    /* horizontal */
        position: static;       /* kembali ke flow normal */
        box-shadow: none;
        padding: 0;
    }

    .navbar__toggle {
        display: none;          /* sembunyikan hamburger di desktop */
    }
}

/* ─── Media Query untuk preferensi pengguna ──────────────────────────── */

/* Dark mode: ikuti preferensi sistem operasi */
@media (prefers-color-scheme: dark) {
    :root {
        --teks-utama:    #e2e8f0;
        --teks-sekunder: #94a3b8;
        --bg-utama:      #0f172a;
        --bg-sekunder:   #1e293b;
        --border-warna:  #334155;
    }
}

/* Reduced motion: untuk pengguna yang sensitif terhadap animasi */
@media (prefers-reduced-motion: reduce) {
    *, *::before, *::after {
        animation-duration: 0.01ms !important;
        transition-duration: 0.01ms !important;
    }
}

/* Print: tampilan saat dicetak */
@media print {
    .navbar, .footer, .sidebar, .btn { display: none; }
    body { color: black; background: white; font-size: 12pt; }
    a { color: black; text-decoration: underline; }
}
```

---

### J. Animasi dan Transisi CSS

#### [[18. Transisi dan Animasi — Gerakan yang Bermakna]]

CSS

```
/* ─── CSS Transitions ────────────────────────────────────────────────── */

/* Syntax: transition: property duration timing-function delay */
.btn {
    background-color: var(--warna-utama);
    color: white;
    padding: 12px 24px;
    border: none;
    border-radius: var(--border-radius);
    cursor: pointer;

    /* Transition: animasikan property tertentu saat berubah */
    transition:
        background-color 0.2s ease,
        transform 0.2s ease,
        box-shadow 0.2s ease;

    /* Atau semua property: */
    /* transition: all 0.2s ease; */
    /* (hindari 'all' — lebih berat untuk browser) */
}

.btn:hover {
    background-color: #1d4ed8;
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(37, 99, 235, 0.4);
}

.btn:active {
    transform: translateY(0);
    box-shadow: none;
}

/* Timing functions: */
/* ease: lambat-cepat-lambat (default) */
/* linear: kecepatan konstan */
/* ease-in: mulai lambat, akhir cepat */
/* ease-out: mulai cepat, akhir lambat */
/* ease-in-out: lambat-cepat-lambat */
/* cubic-bezier(x1,y1,x2,y2): kustom */

/* ─── CSS Animations ─────────────────────────────────────────────────── */

/* Definisikan animasi dengan @keyframes */
@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

@keyframes shimmer {
    0%   { background-position: -200% 0; }
    100% { background-position:  200% 0; }
}

@keyframes rotasi {
    from { transform: rotate(0deg); }
    to   { transform: rotate(360deg); }
}

@keyframes denyut {
    0%, 100% { transform: scale(1); }
    50%       { transform: scale(1.05); }
}

/* Gunakan animasi */
.kartu-buku {
    animation: fadeIn 0.5s ease-out;
}

/* Skeleton loading — placeholder saat konten masih loading */
.skeleton {
    background: linear-gradient(
        90deg,
        #f0f0f0 25%,
        #e0e0e0 50%,
        #f0f0f0 75%
    );
    background-size: 200% 100%;
    animation: shimmer 1.5s infinite;
    border-radius: var(--border-radius);
}

.skeleton-teks {
    height: 16px;
    margin-bottom: 8px;
    border-radius: 4px;
}

/* Loading spinner */
.spinner {
    width: 40px;
    height: 40px;
    border: 4px solid var(--border-warna);
    border-top-color: var(--warna-utama);
    border-radius: 50%;
    animation: rotasi 0.8s linear infinite;
}

/* Stagger animations: animasi berurutan untuk list items */
.daftar-buku .kartu-buku:nth-child(1) { animation-delay: 0.0s; }
.daftar-buku .kartu-buku:nth-child(2) { animation-delay: 0.1s; }
.daftar-buku .kartu-buku:nth-child(3) { animation-delay: 0.2s; }
.daftar-buku .kartu-buku:nth-child(4) { animation-delay: 0.3s; }
```

---

### 🏗️ Checkpoint Level 3

text

```
✅ Checklist sebelum lanjut ke Level 4:

PROYEK: Layout Responsif Sempurna
├── Layout utama menggunakan CSS Grid (header, sidebar, main, footer)
├── Kartu buku menggunakan Flexbox
├── Grid katalog: auto-fill minmax — responsif tanpa media query
├── Navbar: hamburger di mobile, horizontal di desktop
├── Dark mode mengikuti preferensi sistem
└── Animasi: hover effect, fade in untuk kartu buku

RESPONSIVITAS (cek di DevTools):
├── Mobile (360px): satu kolom, hamburger menu
├── Tablet (768px): dua kolom, navigasi muncul
├── Desktop (1280px): tiga-empat kolom, layout lengkap
└── Tidak ada horizontal scroll di semua ukuran

PEMAHAMAN:
├── Bisa jelaskan kapan pakai Flexbox vs Grid
├── Bisa jelaskan mobile-first vs desktop-first
├── Bisa jelaskan perbedaan transition vs animation
└── Bisa buat layout Holy Grail tanpa tutorial

Git: feat: implement Flexbox, Grid, responsive design, and animations
```

---

## 🟠 LEVEL 4: JAVASCRIPT DASAR (Minggu 10-14)

> **Tema**: _"Dari halaman statis ke halaman yang bisa berpikir dan bereaksi"_
> **Benang Merah**: HTML dan CSS (Level 1-3) tidak bisa bereaksi terhadap user → JavaScript memberikan logika dan interaktivitas → variabel, kondisi, loop, fungsi → halaman yang "hidup"
> **Output**: Halaman perpustakaan yang merespons aksi pengguna

---

### K. JavaScript Dasar — Bahasa yang Membuat Web Hidup

> 💡 **Mengapa setelah HTML dan CSS?** JavaScript tanpa fondasi HTML/CSS yang kuat akan membuat kamu tidak tahu _apa_ yang ingin kamu manipulasi atau _bagaimana_ tampilannya. Urutan ini sengaja.

text

```
Benang Merah Bagian K:
HTML + CSS: tampilan yang cantik tapi statis (Level 1-3) →
JavaScript: bahasa yang berjalan di browser →
Variabel: menyimpan data →
Tipe data: number, string, boolean, array, object →
Kondisi: buat keputusan →
Loop: ulangi tindakan →
Fungsi: kode yang bisa dipanggil berulang
```

#### [[19. Cara JavaScript Bekerja di Browser]]

HTML

```
<!-- Cara menghubungkan JavaScript ke HTML -->

<!-- CARA YANG BENAR: di akhir body, atau dengan defer -->
<!-- Di akhir </body>: HTML di-parse dulu, baru JS dijalankan -->
<body>
    <!-- ... semua konten HTML ... -->
    <script src="js/main.js"></script>
</body>

<!-- Atau: di <head> dengan defer -->
<!-- defer: browser download JS tapi jalankan setelah HTML selesai di-parse -->
<head>
    <script src="js/main.js" defer></script>
</head>

<!-- HINDARI: di dalam <head> tanpa defer -->
<!-- Browser stop parsing HTML sampai JS selesai diunduh dan dijalankan -->
<!-- Halaman tampak lambat! -->
<head>
    <script src="js/main.js"></script>  <!-- ❌ blokir parsing HTML -->
</head>
```

JavaScript

```
// js/main.js

// JavaScript berjalan di browser — konsol adalah teman terbaikmu!
// Buka DevTools (F12) → Console untuk melihat output

console.log("JavaScript berjalan!");     // pesan biasa
console.warn("Ini peringatan");          // kuning
console.error("Ini error");             // merah
console.table([{ nama: "Budi" }]);       // tampilkan array/object sebagai tabel

// Komentar
// Satu baris: double slash

/*
  Multi-baris:
  antara slash-bintang
*/
```

#### [[20. Variabel — let, const, dan Mengapa Bukan var]]

JavaScript

```
// var: cara lama — JANGAN DIPAKAI
var nama = "Budi";  // function-scoped, bisa re-declare, hoisted — membingungkan!

// let: untuk nilai yang bisa berubah
let stok = 10;
stok = 8;   // ✅ bisa diubah
stok = "habis"; // ✅ bisa diubah tipenya (JavaScript tidak strict tipe)

// const: untuk nilai yang tidak berubah — SELALU gunakan ini jika bisa
const NAMA_PERPUSTAKAAN = "Perpustakaan Digital Kota";
const MAKS_PINJAM = 5;
// NAMA_PERPUSTAKAAN = "Lain"; // ❌ TypeError: Assignment to constant variable

// Perbedaan penting dengan const + object/array:
const buku = { judul: "Clean Code", stok: 5 };
buku.stok = 4;       // ✅ property object bisa diubah!
buku.baru = "nilai"; // ✅ bisa tambah property baru
// buku = {};         // ❌ tidak bisa reassign variable-nya

const katalog = ["Clean Code", "Laskar Pelangi"];
katalog.push("Harry Potter"); // ✅ bisa modifikasi array
// katalog = [];               // ❌ tidak bisa reassign

/*
Aturan:
1. Selalu pakai const
2. Jika perlu reassign, ubah ke let
3. Jangan pernah pakai var
*/
```

#### [[21. Tipe Data — Mengenal Jenis-Jenis Nilai]]

JavaScript

```
// ─── Primitive Types ─────────────────────────────────────────────────────
const teks       = "Clean Code";           // string
const angka      = 150000;                 // number (tidak ada int/float di JS)
const desimal    = 4.8;                    // number
const benar      = true;                   // boolean
const salah      = false;                  // boolean
const kosong     = null;                   // null: sengaja dikosongkan
const belumAda   = undefined;             // undefined: belum diberi nilai

// Cek tipe data
typeof "teks"      // "string"
typeof 42          // "number"
typeof true        // "boolean"
typeof undefined   // "undefined"
typeof null        // "object" ← ini bug JS yang sudah terlanjur jadi fitur!
typeof []          // "object"
typeof {}          // "object"
typeof function(){} // "function"

// ─── String Operations ───────────────────────────────────────────────────
const judul = "Clean Code";
const pengarang = "Robert Martin";

// Concatenation (cara lama)
const info = judul + " oleh " + pengarang;

// Template literal (cara modern — GUNAKAN INI)
const infoModern = `${judul} oleh ${pengarang}`;
const multiLine = `
    Judul: ${judul}
    Pengarang: ${pengarang}
    Tahun: ${2008}
`;

// String methods
"clean code".toUpperCase()          // "CLEAN CODE"
"CLEAN CODE".toLowerCase()          // "clean code"
"  clean code  ".trim()             // "clean code"
"clean code".includes("clean")      // true
"clean code".startsWith("clean")    // true
"clean code".endsWith("code")       // true
"clean code".replace("clean", "dirty") // "dirty code"
"a,b,c".split(",")                  // ["a", "b", "c"]
"clean".repeat(3)                   // "cleancleanclean"
"clean code".indexOf("code")        // 6
"clean code".slice(0, 5)            // "clean"
"clean code".padStart(15, "*")      // "*****clean code"
String(150000)                       // "150000"

// ─── Number Operations ───────────────────────────────────────────────────
const harga = 150000;
const diskon = 0.1;

harga * (1 - diskon)           // 135000
Math.round(4.7)                // 5
Math.floor(4.9)                // 4
Math.ceil(4.1)                 // 5
Math.max(10, 20, 30)           // 30
Math.min(10, 20, 30)           // 10
Math.abs(-42)                  // 42
Math.random()                  // 0 sampai 1 (eksklusif)
Math.random() * 100            // 0 sampai 100
Math.floor(Math.random() * 10) // 0 sampai 9 (integer random)
Number("150000")               // 150000
parseInt("150000 rupiah")      // 150000
parseFloat("4.8 bintang")      // 4.8

// Format angka sebagai mata uang
harga.toLocaleString("id-ID", {
    style: "currency",
    currency: "IDR",
    minimumFractionDigits: 0,
})  // "Rp 150.000"

// Hati-hati dengan floating point!
0.1 + 0.2                      // 0.30000000000000004 (bukan 0.3!)
(0.1 + 0.2).toFixed(1)         // "0.3" (tapi ini string!)
Number((0.1 + 0.2).toFixed(1)) // 0.3

// ─── Array ───────────────────────────────────────────────────────────────
const katalog = [
    { id: 1, judul: "Clean Code",        stok: 5 },
    { id: 2, judul: "Laskar Pelangi",    stok: 3 },
    { id: 3, judul: "The Great Gatsby",  stok: 0 },
];

katalog.length          // 3
katalog[0]              // { id: 1, judul: "Clean Code", stok: 5 }
katalog[0].judul        // "Clean Code"

// Modifikasi array
katalog.push({ id: 4, judul: "Harry Potter", stok: 7 });   // tambah di akhir
katalog.pop();                                               // hapus dari akhir
katalog.unshift({ id: 0, judul: "Awal", stok: 1 });        // tambah di awal
katalog.shift();                                             // hapus dari awal
katalog.splice(1, 1);   // hapus 1 elemen mulai index 1

// Spread: copy array atau gabungkan
const katalogBaru = [...katalog, { id: 5, judul: "Baru", stok: 2 }];
const gabungan    = [...katalog1, ...katalog2];

// Destructuring
const [pertama, kedua, ...sisanya] = katalog;
const { judul, stok } = katalog[0];

// ─── Object ──────────────────────────────────────────────────────────────
const buku = {
    id: 1,
    judul: "Clean Code",
    pengarang: "Robert Martin",
    tahun: 2008,
    harga: 150000,
    stok: 5,
    kategori: "Teknologi",
};

// Akses property
buku.judul          // "Clean Code"
buku["judul"]       // "Clean Code" — berguna jika nama property dinamis

// Tambah/ubah property
buku.rating = 4.8;
buku.stok   = 4;

// Hapus property
delete buku.rating;

// Cek keberadaan property
"judul" in buku           // true
buku.hasOwnProperty("judul") // true

// Object methods
Object.keys(buku)          // ["id", "judul", "pengarang", ...]
Object.values(buku)        // [1, "Clean Code", "Robert Martin", ...]
Object.entries(buku)       // [["id", 1], ["judul", "Clean Code"], ...]

// Spread dan merge object
const bukuUpdate = { ...buku, stok: 3, rating: 4.5 };
// Jika key sama, yang kanan menang

// Destructuring
const { judul: judulBuku, pengarang, ...sisanyaBuku } = buku;
```

#### [[22. Kondisi dan Perbandingan]]

JavaScript

```
// ─── Operator Perbandingan ───────────────────────────────────────────────
// SELALU gunakan === (strict equality), bukan == (loose equality)
5 === 5       // true
5 === "5"     // false ← string vs number
5 == "5"      // true ← BAHAYA! JavaScript konversi tipe dulu
null == undefined  // true ← ini perilaku yang membingungkan
null === undefined // false ← ini yang benar

// ─── if / else if / else ─────────────────────────────────────────────────
function cekKetersediaan(stok) {
    if (stok > 10) {
        return "Stok banyak";
    } else if (stok > 0) {
        return `Stok terbatas (${stok})`;
    } else {
        return "Stok habis";
    }
}

// ─── Ternary operator — untuk kondisi sederhana ──────────────────────────
const status = stok > 0 ? "Tersedia" : "Habis";
// Jika stok > 0 → "Tersedia", jika tidak → "Habis"

// ─── Nullish coalescing (??) — nilai default ─────────────────────────────
const judul = bukuDariAPI?.judul ?? "Judul Tidak Diketahui";
// Jika judul null atau undefined → pakai default

// ─── Optional chaining (?.) — akses property yang mungkin null ───────────
const kota = anggota?.alamat?.kota;  // undefined jika alamat atau kota tidak ada

// ─── Short-circuit evaluation ─────────────────────────────────────────────
// && : jika kiri falsy, kembalikan kiri; jika kiri truthy, kembalikan kanan
const pesan = stok > 0 && `Tersedia ${stok} buku`;  // false atau string
// Dipakai untuk render kondisional

// || : jika kiri truthy, kembalikan kiri; jika kiri falsy, kembalikan kanan
const namaDisplay = namaPengguna || "Tamu";

// ─── switch ──────────────────────────────────────────────────────────────
function warnaBadgeKategori(kategori) {
    switch (kategori) {
        case "Teknologi": return "#3b82f6";   // biru
        case "Fiksi":     return "#8b5cf6";   // ungu
        case "Sains":     return "#10b981";   // hijau
        default:          return "#6b7280";   // abu
    }
}
```

#### [[23. Loop — Mengulang Tindakan]]

JavaScript

```
// ─── Array methods (cara modern — GUNAKAN INI) ────────────────────────────

const katalog = [
    { id: 1, judul: "Clean Code",        stok: 5,  harga: 150000, kategori: "Teknologi" },
    { id: 2, judul: "Laskar Pelangi",    stok: 3,  harga: 95000,  kategori: "Fiksi" },
    { id: 3, judul: "The Great Gatsby",  stok: 0,  harga: 120000, kategori: "Fiksi" },
    { id: 4, judul: "Cosmos",            stok: 8,  harga: 200000, kategori: "Sains" },
];

// forEach: untuk setiap elemen, lakukan sesuatu (tidak return)
katalog.forEach((buku, index) => {
    console.log(`${index + 1}. ${buku.judul}`);
});

// map: transformasi setiap elemen, return array baru
const judulBuku = katalog.map(buku => buku.judul);
// ["Clean Code", "Laskar Pelangi", "The Great Gatsby", "Cosmos"]

const kartuHTML = katalog.map(buku => `
    <article class="kartu-buku">
        <h3>${buku.judul}</h3>
        <p>${buku.stok > 0 ? `Tersedia (${buku.stok})` : "Habis"}</p>
    </article>
`).join("");

// filter: ambil elemen yang memenuhi kondisi, return array baru
const bukuTersedia = katalog.filter(buku => buku.stok > 0);
const bukuFiksi    = katalog.filter(buku => buku.kategori === "Fiksi");
const bukuMurah    = katalog.filter(buku => buku.harga <= 100000);

// find: ambil SATU elemen pertama yang memenuhi kondisi (atau undefined)
const bukuClean = katalog.find(buku => buku.judul.includes("Clean"));

// findIndex: seperti find tapi kembalikan index
const indexClean = katalog.findIndex(buku => buku.id === 1);

// some: apakah ADA elemen yang memenuhi kondisi?
const adaYangHabis = katalog.some(buku => buku.stok === 0);  // true

// every: apakah SEMUA elemen memenuhi kondisi?
const semuaTersedia = katalog.every(buku => buku.stok > 0);  // false

// reduce: akumulasi semua elemen menjadi satu nilai
const totalStok = katalog.reduce((total, buku) => total + buku.stok, 0); // 16

// sort: urutkan (modifikasi array asli!)
const diurutHarga = [...katalog].sort((a, b) => a.harga - b.harga);  // asc
const diurutJudul = [...katalog].sort((a, b) => a.judul.localeCompare(b.judul));

// Chaining: gabungkan array methods
const hasilCari = katalog
    .filter(buku => buku.kategori === "Fiksi")
    .filter(buku => buku.stok > 0)
    .sort((a, b) => a.judul.localeCompare(b.judul))
    .map(buku => buku.judul);
// ["Laskar Pelangi"] — fiksi yang tersedia, diurutkan, diambil judulnya

// ─── for...of (untuk iterasi yang perlu break/continue) ──────────────────
for (const buku of katalog) {
    if (buku.stok === 0) continue;  // skip buku yang habis
    console.log(buku.judul);
    if (buku.id === 3) break;        // stop di id 3
}

// ─── for...in (untuk object properties) ──────────────────────────────────
const buku = { judul: "Clean Code", pengarang: "Robert Martin", harga: 150000 };
for (const key in buku) {
    console.log(`${key}: ${buku[key]}`);
}
// Lebih baik: Object.entries(buku).forEach(([key, value]) => ...)
```

#### [[24. Fungsi — Kode yang Reusable]]

JavaScript

```
// ─── Function Declaration ─────────────────────────────────────────────────
// Bisa dipanggil sebelum didefinisikan (hoisted)
function hitungDenda(hariTerlambat, dendaPerHari = 1000) {
    if (hariTerlambat < 0) throw new Error("Hari tidak boleh negatif");
    return hariTerlambat * dendaPerHari;
}

console.log(hitungDenda(7));        // 7000 (default denda 1000)
console.log(hitungDenda(7, 2000));  // 14000

// ─── Arrow Function (cara modern) ─────────────────────────────────────────
const hitungDendaArrow = (hariTerlambat, dendaPerHari = 1000) => {
    return hariTerlambat * dendaPerHari;
};

// Jika satu ekspresi, bisa tanpa {} dan return
const hitungDendaSingkat = (hari, tarif = 1000) => hari * tarif;

// Satu parameter: boleh tanpa kurung
const kuadrat = n => n * n;

// ─── Higher-Order Function — fungsi yang menerima/return fungsi ───────────
function buatFilter(field, nilai) {
    // return fungsi yang bisa dipakai sebagai callback filter
    return buku => buku[field] === nilai;
}

const filterFiksi     = buatFilter("kategori", "Fiksi");
const filterTeknologi = buatFilter("kategori", "Teknologi");

const bukuFiksi     = katalog.filter(filterFiksi);
const bukuTeknologi = katalog.filter(filterTeknologi);

// ─── Closure — fungsi yang "mengingat" scope luar ─────────────────────────
function buatCounter(awal = 0) {
    let count = awal;  // variabel ini "ditangkap" oleh closure

    return {
        increment: () => ++count,
        decrement: () => --count,
        reset:     () => { count = awal; },
        nilai:     () => count,
    };
}

const counterKunjungan = buatCounter(0);
counterKunjungan.increment(); // 1
counterKunjungan.increment(); // 2
counterKunjungan.decrement(); // 1
console.log(counterKunjungan.nilai()); // 1

// ─── Async/Await — untuk operasi yang membutuhkan waktu ──────────────────
// (akan dibahas lebih detail di Level 6)
async function ambilDataBuku() {
    try {
        const response = await fetch("/api/buku");
        const data = await response.json();
        return data;
    } catch (error) {
        console.error("Gagal ambil data:", error);
        return [];
    }
}
```

---

### 🏗️ Checkpoint Level 4

text

```
✅ Checklist sebelum lanjut ke Level 5:

PEMAHAMAN:
├── Bisa jelaskan perbedaan let vs const (dan mengapa bukan var)
├── Bisa jelaskan === vs == dengan contoh
├── Bisa jelaskan perbedaan map, filter, reduce
├── Bisa jelaskan apa itu closure dengan contoh nyata
└── Bisa jelaskan kapan pakai forEach vs map

LATIHAN (console di DevTools):
├── Filter katalog buku berdasarkan kategori
├── Sort buku berdasarkan harga
├── Hitung total nilai koleksi (jumlah harga × stok)
├── Buat fungsi pencarian buku berdasarkan keyword
└── Buat fungsi format harga: 150000 → "Rp 150.000"

KEBIASAAN:
├── Selalu const, ubah ke let jika perlu reassign
├── Selalu === bukan ==
├── Gunakan array methods (map, filter) bukan for loop manual
└── Selalu handle error dengan try/catch

Git: feat: learn JavaScript basics — variables, functions, array methods
```

---

## 🔴 LEVEL 5: DOM MANIPULATION (Minggu 14-18)

> **Tema**: _"Dari kode di konsol ke halaman yang benar-benar interaktif"_
> **Benang Merah**: JavaScript dasar (Level 4) → DOM: representasi HTML sebagai object → seleksi elemen → ubah konten dan gaya → tangani event user
> **Output**: Katalog buku dinamis dengan pencarian, filter, dan CRUD tanpa reload halaman

---

### K. DOM — Document Object Model

> 💡 **Apa itu DOM?** Saat browser membaca HTML, ia membuat representasi object-nya dalam memori — ini adalah DOM. JavaScript bisa membaca dan mengubah DOM, dan browser akan otomatis memperbarui tampilan.

text

```
Benang Merah Bagian K:
HTML di-parse oleh browser (Level 1) →
DOM: pohon object yang merepresentasikan HTML →
JavaScript bisa akses dan ubah DOM →
Perubahan DOM → browser otomatis update tampilan →
Tidak perlu reload halaman!
```

#### [[25. Seleksi Elemen — Temukan Elemen di DOM]]

JavaScript

```
// js/main.js

// ─── Seleksi Elemen ───────────────────────────────────────────────────

// querySelector: pilih elemen PERTAMA yang cocok (CSS selector)
const header    = document.querySelector("header");
const navLinks  = document.querySelector(".navbar__menu");
const formCari  = document.querySelector("#form-pencarian");
const bukuCard  = document.querySelector(".kartu-buku");

// querySelectorAll: pilih SEMUA elemen yang cocok (return NodeList)
const semuaKartu   = document.querySelectorAll(".kartu-buku");
const semuaLink    = document.querySelectorAll("nav a");
const semuaGambar  = document.querySelectorAll("img[alt='']"); // gambar tanpa alt!

// NodeList bukan Array, tapi bisa di-iterate
semuaKartu.forEach(kartu => console.log(kartu));

// Konversi ke Array jika butuh array methods
const arrayKartu = Array.from(semuaKartu);
const arrayKartu2 = [...semuaKartu];  // spread

// getElementById: pilih berdasarkan ID (lebih cepat dari querySelector)
const formPendaftaran = document.getElementById("form-pendaftaran");

// ─── Navigasi DOM ─────────────────────────────────────────────────────────
const parent    = element.parentElement;         // parent langsung
const children  = element.children;              // semua child (HTMLCollection)
const firstChild = element.firstElementChild;    // child pertama
const lastChild  = element.lastElementChild;     // child terakhir
const nextSibling    = element.nextElementSibling;     // saudara berikutnya
const prevSibling    = element.previousElementSibling; // saudara sebelumnya
```

#### [[26. Manipulasi Elemen — Ubah Konten dan Tampilan]]

JavaScript

```
// ─── Baca dan Ubah Konten ─────────────────────────────────────────────────
const judul = document.querySelector("h1");

// textContent: teks saja (tanpa HTML), lebih aman
console.log(judul.textContent);
judul.textContent = "Perpustakaan Digital Kota";

// innerHTML: teks dengan HTML — HATI-HATI XSS!
judul.innerHTML = "<span>Perpustakaan</span> Digital";
// Jangan masukkan input user langsung ke innerHTML!
// Gunakan textContent untuk input user, innerHTML untuk template yang kamu kontrol

// innerText: seperti textContent tapi perhatikan CSS (display:none disembunyikan)

// ─── Manipulasi Atribut ───────────────────────────────────────────────────
const gambar = document.querySelector(".gambar-buku");

gambar.getAttribute("src")             // baca atribut
gambar.setAttribute("alt", "Teks alt baru")  // set atribut
gambar.removeAttribute("alt")          // hapus atribut
gambar.hasAttribute("alt")             // cek keberadaan

// Atribut khusus yang bisa diakses langsung:
gambar.src       = "images/buku-baru.jpg";
gambar.alt       = "Deskripsi gambar";
const link = document.querySelector("a");
link.href        = "https://example.com";
const input = document.querySelector("input");
input.value      = "Nilai baru";
input.disabled   = true;
input.checked    = true;  // untuk checkbox/radio

// Dataset: akses data-* attributes
// HTML: <div data-id="1" data-kategori="Fiksi">
const kartu = document.querySelector(".kartu-buku");
kartu.dataset.id        // "1"
kartu.dataset.kategori  // "Fiksi"
kartu.dataset.baru = "nilai";  // tambah data attribute

// ─── Manipulasi CSS Class ─────────────────────────────────────────────────
const btn = document.querySelector(".btn");

btn.classList.add("aktif");              // tambah class
btn.classList.remove("aktif");           // hapus class
btn.classList.toggle("aktif");           // tambah jika tidak ada, hapus jika ada
btn.classList.contains("aktif");         // cek class
btn.classList.replace("lama", "baru");   // ganti class
btn.className = "btn btn-primary";       // set semua class (hapus yang lama)

// ─── Manipulasi Style Inline ──────────────────────────────────────────────
// Hindari jika bisa — lebih baik pakai classList + CSS
const elemen = document.querySelector(".hero");
elemen.style.color           = "red";          // camelCase!
elemen.style.backgroundColor = "#3498db";      // bukan background-color
elemen.style.fontSize        = "1.5rem";
elemen.style.display         = "none";         // sembunyikan
elemen.style.display         = "";             // kembalikan ke default CSS
```

#### [[27. Membuat dan Menghapus Elemen — CRUD di DOM]]

JavaScript

```
// ─── Membuat Elemen ───────────────────────────────────────────────────────
function buatKartuBuku(buku) {
    // Cara modern: template string + innerHTML (untuk konten yang dikontrol)
    const article = document.createElement("article");
    article.className = "kartu-buku";
    article.dataset.id = buku.id;

    // Gunakan textContent untuk data dari user/API (cegah XSS)
    const judul = document.createElement("h3");
    judul.textContent = buku.judul;  // aman!

    const pengarang = document.createElement("p");
    pengarang.textContent = buku.pengarang;

    const harga = document.createElement("p");
    harga.className = "kartu-buku__harga";
    harga.textContent = `Rp ${buku.harga.toLocaleString("id-ID")}`;

    const badge = document.createElement("span");
    badge.className = `badge ${buku.stok > 0 ? "badge--success" : "badge--danger"}`;
    badge.textContent = buku.stok > 0 ? `Tersedia (${buku.stok})` : "Habis";

    const btnPinjam = document.createElement("button");
    btnPinjam.className = "btn btn--primary";
    btnPinjam.textContent = "Pinjam";
    btnPinjam.disabled = buku.stok === 0;
    btnPinjam.dataset.id = buku.id;  // simpan ID untuk event handler

    article.append(judul, pengarang, harga, badge, btnPinjam);

    return article;
}

// ─── Memasukkan Elemen ke DOM ─────────────────────────────────────────────
const kontainerKatalog = document.querySelector("#katalog-grid");

// Tambah satu elemen
const kartuBaru = buatKartuBuku(dataBuku);
kontainerKatalog.appendChild(kartuBaru);        // di akhir
kontainerKatalog.prepend(kartuBaru);            // di awal
kontainerKatalog.insertBefore(kartuBaru, referensiElemen); // sebelum elemen lain

// append vs appendChild:
// append: bisa menerima Node ATAU string, tidak return value
// appendChild: hanya Node, return elemen yang ditambahkan

// Render banyak elemen sekaligus — EFISIEN
function renderKatalog(katalog) {
    // Hapus konten lama
    kontainerKatalog.innerHTML = "";

    if (katalog.length === 0) {
        kontainerKatalog.innerHTML = `
            <div class="kosong">
                <p>Tidak ada buku yang ditemukan.</p>
            </div>
        `;
        return;
    }

    // DocumentFragment: batch update untuk performa
    const fragment = document.createDocumentFragment();
    katalog.forEach(buku => fragment.appendChild(buatKartuBuku(buku)));
    kontainerKatalog.appendChild(fragment);
    // Hanya satu DOM update, bukan satu per kartu!
}

// ─── Menghapus Elemen ────────────────────────────────────────────────────
function hapusBuku(id) {
    const kartu = document.querySelector(`[data-id="${id}"]`);
    if (kartu) {
        kartu.remove();  // hapus dari DOM
    }
}
```

#### [[28. Event Handling — Merespons Aksi Pengguna]]

JavaScript

```
// ─── addEventListener — cara yang benar untuk handle event ───────────────
const btn = document.querySelector(".btn-tambah");

// Cara yang benar:
btn.addEventListener("click", function(event) {
    console.log("Tombol diklik!");
    console.log(event.target);  // elemen yang diklik
});

// Dengan arrow function:
btn.addEventListener("click", (event) => {
    event.preventDefault();  // cegah perilaku default (submit form, dll)
    event.stopPropagation(); // cegah event bubble ke parent
});

// ─── Event Delegation — satu listener untuk banyak child ─────────────────
// SANGAT penting untuk elemen yang dibuat secara dinamis!
const katalogGrid = document.querySelector("#katalog-grid");

// ❌ SALAH: attach ke setiap kartu (tidak bekerja untuk elemen dinamis)
document.querySelectorAll(".btn-pinjam").forEach(btn => {
    btn.addEventListener("click", () => { /* ... */ });
});

// ✅ BENAR: attach ke parent, cek target di dalam handler
katalogGrid.addEventListener("click", (event) => {
    // Cari elemen yang paling dekat dengan selector yang kita mau
    const btnPinjam = event.target.closest(".btn-pinjam");
    const btnHapus  = event.target.closest(".btn-hapus");
    const kartu     = event.target.closest(".kartu-buku");

    if (btnPinjam) {
        const id = parseInt(btnPinjam.dataset.id);
        pinjamBuku(id);
    }

    if (btnHapus) {
        const id = parseInt(kartu.dataset.id);
        hapusBuku(id);
    }
});

// ─── Event yang sering dipakai ────────────────────────────────────────────

// Click events
elemen.addEventListener("click", handler);
elemen.addEventListener("dblclick", handler);

// Form events
form.addEventListener("submit", (e) => {
    e.preventDefault(); // WAJIB untuk handle form dengan JS
    const formData = new FormData(form);
    const data = Object.fromEntries(formData.entries());
    console.log(data);  // { nama: "Budi", email: "..." }
});

input.addEventListener("input", (e) => {
    // Dipanggil setiap kali value berubah (real-time)
    console.log(e.target.value);
});

input.addEventListener("change", handler);   // setelah blur dan value berubah
input.addEventListener("focus", handler);    // saat dapat fokus
input.addEventListener("blur", handler);     // saat kehilangan fokus

// Keyboard events
document.addEventListener("keydown", (e) => {
    if (e.key === "Escape") tutupModal();
    if (e.key === "Enter" && e.ctrlKey) simpanForm();
    console.log(e.key);  // nama key
    console.log(e.code); // kode fisik tombol
});

// Mouse events
elemen.addEventListener("mouseenter", handler); // masuk area elemen
elemen.addEventListener("mouseleave", handler); // keluar area elemen
elemen.addEventListener("mousemove", handler);  // gerakan mouse

// Window events
window.addEventListener("scroll", handler);
window.addEventListener("resize", handler);
window.addEventListener("load", handler);     // setelah semua resource loaded

// DOMContentLoaded: lebih cepat dari load — setelah HTML di-parse
document.addEventListener("DOMContentLoaded", () => {
    // Inisialisasi aplikasi di sini
    inisialisasiAplikasi();
});
```

---

### L. Membangun Fitur Interaktif

#### [[29. Pencarian dan Filter Real-time]]

JavaScript

```
// js/katalog.js

// State aplikasi — simpan semua data dan state di satu tempat
const state = {
    semuaBuku: [],          // data asli dari server
    hasilFilter: [],        // data setelah difilter
    filter: {
        keyword: "",
        kategori: "semua",
        hanya_tersedia: false,
    },
    urutan: "judul-asc",
};

// ─── Debounce: tunda eksekusi fungsi ─────────────────────────────────────
// Berguna untuk pencarian real-time — jangan search setiap keypress!
function debounce(fungsi, delay = 300) {
    let timerId;
    return function(...args) {
        clearTimeout(timerId);
        timerId = setTimeout(() => fungsi.apply(this, args), delay);
    };
}

// ─── Filter dan Sort ─────────────────────────────────────────────────────
function filterDanSortBuku() {
    let hasil = [...state.semuaBuku];

    // Filter berdasarkan keyword
    if (state.filter.keyword) {
        const kw = state.filter.keyword.toLowerCase();
        hasil = hasil.filter(buku =>
            buku.judul.toLowerCase().includes(kw) ||
            buku.pengarang.toLowerCase().includes(kw) ||
            buku.isbn?.includes(kw)
        );
    }

    // Filter berdasarkan kategori
    if (state.filter.kategori !== "semua") {
        hasil = hasil.filter(buku => buku.kategori === state.filter.kategori);
    }

    // Filter hanya yang tersedia
    if (state.filter.hanya_tersedia) {
        hasil = hasil.filter(buku => buku.stok > 0);
    }

    // Sort
    const [field, arah] = state.urutan.split("-");
    hasil.sort((a, b) => {
        let nilaiA = a[field];
        let nilaiB = b[field];

        if (typeof nilaiA === "string") {
            nilaiA = nilaiA.toLowerCase();
            nilaiB = nilaiB.toLowerCase();
        }

        if (nilaiA < nilaiB) return arah === "asc" ? -1 : 1;
        if (nilaiA > nilaiB) return arah === "asc" ? 1 : -1;
        return 0;
    });

    state.hasilFilter = hasil;
    renderKatalog(hasil);
    updateInfoHasil(hasil.length);
}

// ─── Setup Event Listeners ───────────────────────────────────────────────
function setupFilterListeners() {
    const inputCari    = document.querySelector("#input-pencarian");
    const selectKat    = document.querySelector("#filter-kategori");
    const checkTersedia = document.querySelector("#filter-tersedia");
    const selectUrutan  = document.querySelector("#urutan");

    // Debounced search — tunggu 300ms setelah user berhenti mengetik
    const handleCari = debounce((e) => {
        state.filter.keyword = e.target.value.trim();
        filterDanSortBuku();
    }, 300);

    inputCari.addEventListener("input", handleCari);

    selectKat.addEventListener("change", (e) => {
        state.filter.kategori = e.target.value;
        filterDanSortBuku();
    });

    checkTersedia.addEventListener("change", (e) => {
        state.filter.hanya_tersedia = e.target.checked;
        filterDanSortBuku();
    });

    selectUrutan.addEventListener("change", (e) => {
        state.urutan = e.target.value;
        filterDanSortBuku();
    });

    // Reset semua filter
    document.querySelector("#btn-reset-filter")?.addEventListener("click", () => {
        state.filter = { keyword: "", kategori: "semua", hanya_tersedia: false };
        state.urutan = "judul-asc";

        inputCari.value         = "";
        selectKat.value         = "semua";
        checkTersedia.checked   = false;
        selectUrutan.value      = "judul-asc";

        filterDanSortBuku();
    });
}
```

#### [[30. Modal dan Notifikasi — UI Pattern yang Penting]]

JavaScript

```
// js/ui.js

// ─── Modal ───────────────────────────────────────────────────────────────
class Modal {
    constructor(selectorModal) {
        this.modal    = document.querySelector(selectorModal);
        this.overlay  = this.modal.querySelector(".modal__overlay");
        this.btnTutup = this.modal.querySelector(".modal__tutup");

        this.setupListeners();
    }

    setupListeners() {
        // Tutup modal saat klik overlay
        this.overlay.addEventListener("click", () => this.tutup());

        // Tutup modal saat klik tombol X
        this.btnTutup?.addEventListener("click", () => this.tutup());

        // Tutup modal saat tekan Escape
        document.addEventListener("keydown", (e) => {
            if (e.key === "Escape" && this.seddangBuka()) this.tutup();
        });
    }

    buka(data = {}) {
        // Isi konten modal jika ada data
        if (data.judul) {
            this.modal.querySelector(".modal__judul").textContent = data.judul;
        }

        this.modal.classList.add("modal--aktif");
        document.body.classList.add("no-scroll");  // cegah scroll background

        // Fokus ke modal untuk aksesibilitas
        this.modal.setAttribute("aria-hidden", "false");
        this.btnTutup?.focus();

        return this; // untuk chaining
    }

    tutup() {
        this.modal.classList.remove("modal--aktif");
        document.body.classList.remove("no-scroll");
        this.modal.setAttribute("aria-hidden", "true");

        return this;
    }

    sedangBuka() {
        return this.modal.classList.contains("modal--aktif");
    }
}

// ─── Toast Notification ──────────────────────────────────────────────────
class Notifikasi {
    static container = null;

    static setup() {
        this.container = document.createElement("div");
        this.container.id = "notifikasi-container";
        this.container.setAttribute("role", "status");
        this.container.setAttribute("aria-live", "polite");
        document.body.appendChild(this.container);
    }

    static tampilkan(pesan, tipe = "info", durasi = 3000) {
        const notif = document.createElement("div");
        notif.className = `notifikasi notifikasi--${tipe}`;
        notif.textContent = pesan;
        notif.setAttribute("role", "alert");

        this.container.appendChild(notif);

        // Animasi masuk
        requestAnimationFrame(() => {
            notif.classList.add("notifikasi--tampil");
        });

        // Auto-hapus
        setTimeout(() => {
            notif.classList.remove("notifikasi--tampil");
            notif.addEventListener("transitionend", () => notif.remove());
        }, durasi);
    }

    static sukses(pesan)     { this.tampilkan(pesan, "sukses"); }
    static error(pesan)      { this.tampilkan(pesan, "error"); }
    static peringatan(pesan) { this.tampilkan(pesan, "peringatan"); }
    static info(pesan)       { this.tampilkan(pesan, "info"); }
}

// Penggunaan:
Notifikasi.sukses("Buku berhasil dipinjam!");
Notifikasi.error("Stok buku habis.");
```

---

### 🏗️ Checkpoint Level 5

text

```
✅ Checklist sebelum lanjut ke Level 6:

PROYEK: Katalog Buku Interaktif
├── Pencarian real-time dengan debounce (tidak lag saat mengetik)
├── Filter berdasarkan kategori dan ketersediaan
├── Sorting berdasarkan beberapa field
├── Tambah buku via form modal (data disimpan di array)
├── Hapus buku dengan konfirmasi
├── Edit buku via form modal
├── Toast notification setelah setiap aksi
└── Semua berfungsi tanpa reload halaman

EVENT HANDLING:
├── Event delegation (bukan addEventListener di setiap kartu)
├── Form submit dengan preventDefault
├── Keyboard navigation (Escape menutup modal)
└── Debounce pada input pencarian

AKSESIBILITAS:
├── Modal punya aria-hidden dan focus management
├── Notifikasi punya aria-live
├── Button punya teks yang deskriptif
└── Tidak ada event handler yang hanya mengandalkan mouse

Git: feat: implement DOM manipulation, search, filter, and modal
```

---

## ⚫ LEVEL 6: FETCH API, LOCALSTORAGE, DAN ASYNC (Minggu 18-24)

> **Tema**: _"Dari data hardcode ke data dari server yang sesungguhnya"_
> **Benang Merah**: Data di array di memori (Level 5) → ambil data dari API eksternal → simpan data di browser → handle async dengan Promise dan async/await
> **Output**: Aplikasi perpustakaan yang mengambil data dari API nyata dan menyimpan preferensi user

---

### M. Async JavaScript — Promise dan Async/Await

> 💡 **Mengapa Async?** JavaScript adalah single-threaded. Tanpa async, request ke server akan membekukan seluruh halaman sampai data datang. Async memungkinkan JavaScript melakukan request sambil tetap merespons user.

#### [[31. Promise dan Async-Await — Handle Operasi Async]]

JavaScript

```
// ─── Promise ─────────────────────────────────────────────────────────────
// Promise: "janji" bahwa operasi async akan selesai (resolved) atau gagal (rejected)

const janji = new Promise((resolve, reject) => {
    // Simulasi operasi async
    setTimeout(() => {
        const berhasil = true;
        if (berhasil) {
            resolve({ data: "Buku ditemukan" }); // berhasil
        } else {
            reject(new Error("Buku tidak ditemukan")); // gagal
        }
    }, 1000);
});

// Konsumsi Promise
janji
    .then(hasil => console.log(hasil.data))
    .catch(error => console.error(error.message))
    .finally(() => console.log("Selesai, apapun hasilnya"));

// ─── Async/Await — cara modern yang lebih mudah dibaca ───────────────────
// async: tandai fungsi sebagai async (selalu return Promise)
// await: tunggu Promise selesai (hanya bisa di dalam async function)

async function ambilBukuDariAPI(id) {
    try {
        const response = await fetch(`https://openlibrary.org/works/${id}.json`);

        // Cek apakah response OK (status 200-299)
        if (!response.ok) {
            throw new Error(`HTTP error: ${response.status} ${response.statusText}`);
        }

        const data = await response.json();
        return data;

    } catch (error) {
        if (error instanceof TypeError) {
            // Network error: tidak ada koneksi internet
            throw new Error("Tidak dapat terhubung ke server. Periksa koneksi internet Anda.");
        }
        throw error; // re-throw error lain
    }
}

// ─── Fetch API — HTTP request dari browser ────────────────────────────────
async function fetchData(url, opsi = {}) {
    const defaultOpsi = {
        headers: {
            "Content-Type": "application/json",
            "Accept": "application/json",
        },
    };

    const response = await fetch(url, { ...defaultOpsi, ...opsi });

    if (!response.ok) {
        const errorData = await response.json().catch(() => ({}));
        throw new Error(errorData.message || `Error ${response.status}`);
    }

    return response.json();
}

// GET: ambil data
const buku = await fetchData("/api/buku/1");

// POST: kirim data baru
const bukuBaru = await fetchData("/api/buku", {
    method: "POST",
    body: JSON.stringify({ judul: "Buku Baru", pengarang: "Penulis" }),
});

// POST: kirim data baru
const bukuBaru = await fetchData("/api/buku", {
    method: "POST",
    body: JSON.stringify({ judul: "Buku Baru", pengarang: "Penulis" }),
});

// PUT: update data
await fetchData(`/api/buku/${id}`, {
    method: "PUT",
    body: JSON.stringify({ stok: 10 }),
});

// DELETE: hapus data
await fetchData(`/api/buku/${id}`, { method: "DELETE" });

// ─── Promise.all — jalankan banyak Promise sekaligus ─────────────────────
async function ambilSemuaData() {
    // Jalankan semua fetch bersamaan (bukan berurutan)
    const [buku, anggota, peminjaman] = await Promise.all([
        fetchData("/api/buku"),
        fetchData("/api/anggota"),
        fetchData("/api/peminjaman"),
    ]);
    // Semua selesai dalam waktu: max(buku, anggota, peminjaman) — jauh lebih cepat!

    return { buku, anggota, peminjaman };
}

// ─── AbortController — batalkan fetch ─────────────────────────────────────
let controller = new AbortController();

async function cariBukuDenganBatal(keyword) {
    controller.abort(); // batalkan request sebelumnya
    controller = new AbortController();

    try {
        const data = await fetchData(
            `/api/buku?q=${keyword}`,
            { signal: controller.signal }
        );
        return data;
    } catch (error) {
        if (error.name === "AbortError") return; // request dibatalkan — OK
        throw error;
    }
}
```

---

### N. LocalStorage dan SessionStorage — Simpan Data di Browser

#### [[32. Web Storage — Persisten Data Tanpa Server]]

JavaScript

```
// ─── localStorage vs sessionStorage ──────────────────────────────────────
// localStorage:    data bertahan hingga dihapus eksplisit (bahkan setelah browser ditutup)
// sessionStorage:  data hilang saat tab/browser ditutup

// localStorage hanya bisa menyimpan STRING
// Gunakan JSON.stringify dan JSON.parse untuk object/array

// ─── Operasi Dasar ────────────────────────────────────────────────────────
localStorage.setItem("nama", "Budi");
localStorage.getItem("nama");          // "Budi"
localStorage.removeItem("nama");
localStorage.clear();                  // hapus semua

// ─── Utility class untuk localStorage yang lebih nyaman ──────────────────
const Storage = {
    simpan(key, nilai) {
        try {
            localStorage.setItem(key, JSON.stringify(nilai));
        } catch (error) {
            // localStorage bisa penuh (quota exceeded)
            console.error("Gagal simpan ke localStorage:", error);
        }
    },

    ambil(key, defaultValue = null) {
        try {
            const item = localStorage.getItem(key);
            return item ? JSON.parse(item) : defaultValue;
        } catch (error) {
            console.error("Gagal ambil dari localStorage:", error);
            return defaultValue;
        }
    },

    hapus(key) { localStorage.removeItem(key); },
    bersihkan() { localStorage.clear(); },
    ada(key)   { return localStorage.getItem(key) !== null; },
};

// ─── Penggunaan di aplikasi perpustakaan ─────────────────────────────────

// Simpan preferensi user
function simpanPreferensi(preferensi) {
    Storage.simpan("preferensi_perpustakaan", preferensi);
}

function ambilPreferensi() {
    return Storage.ambil("preferensi_perpustakaan", {
        tema: "terang",
        bahasa: "id",
        tampilan: "grid",
        filter: { kategori: "semua" },
    });
}

// Simpan buku favorit
function tambahFavorit(bukuId) {
    const favorit = Storage.ambil("buku_favorit", []);
    if (!favorit.includes(bukuId)) {
        favorit.push(bukuId);
        Storage.simpan("buku_favorit", favorit);
        Notifikasi.sukses("Ditambahkan ke favorit!");
    }
}

function hapusFavorit(bukuId) {
    const favorit = Storage.ambil("buku_favorit", []);
    Storage.simpan("buku_favorit", favorit.filter(id => id !== bukuId));
}

function adalahFavorit(bukuId) {
    return Storage.ambil("buku_favorit", []).includes(bukuId);
}

// Cache data dari API
const Cache = {
    simpan(key, data, ttlDetik = 300) {
        Storage.simpan(`cache_${key}`, {
            data,
            expired: Date.now() + (ttlDetik * 1000),
        });
    },

    ambil(key) {
        const cached = Storage.ambil(`cache_${key}`);
        if (!cached) return null;
        if (Date.now() > cached.expired) {
            Storage.hapus(`cache_${key}`);
            return null;  // cache sudah kadaluarsa
        }
        return cached.data;
    },
};

// Contoh penggunaan cache:
async function ambilKatalog() {
    // Coba ambil dari cache dulu
    const cached = Cache.ambil("katalog");
    if (cached) return cached;

    // Jika tidak ada di cache, fetch dari API
    const data = await fetchData("/api/buku");
    Cache.simpan("katalog", data, 300);  // cache 5 menit
    return data;
}
```

---

### O. Loading States dan Error Handling UI

#### [[33. Pengalaman Pengguna yang Baik saat Loading dan Error]]

JavaScript

```
// js/ui-states.js

// ─── Loading State ────────────────────────────────────────────────────────
const UIState = {
    loading(kontainer, pesan = "Memuat...") {
        kontainer.innerHTML = `
            <div class="loading-state" role="status" aria-label="${pesan}">
                <div class="spinner" aria-hidden="true"></div>
                <p>${pesan}</p>
            </div>
        `;
    },

    error(kontainer, pesan, onRetry = null) {
        kontainer.innerHTML = `
            <div class="error-state" role="alert">
                <p class="error-state__icon" aria-hidden="true">⚠️</p>
                <p class="error-state__pesan">${pesan}</p>
                ${onRetry ? `<button class="btn btn--outline" id="btn-retry">Coba Lagi</button>` : ""}
            </div>
        `;

        if (onRetry) {
            kontainer.querySelector("#btn-retry")?.addEventListener("click", onRetry);
        }
    },

    kosong(kontainer, pesan = "Tidak ada data yang ditemukan.") {
        kontainer.innerHTML = `
            <div class="empty-state">
                <p class="empty-state__icon" aria-hidden="true">📚</p>
                <p>${pesan}</p>
            </div>
        `;
    },
};

// ─── Wrapper yang handle semua state ─────────────────────────────────────
async function loadData(kontainer, fetchFn, renderFn) {
    UIState.loading(kontainer);

    try {
        const data = await fetchFn();

        if (!data || (Array.isArray(data) && data.length === 0)) {
            UIState.kosong(kontainer);
            return;
        }

        renderFn(data);

    } catch (error) {
        console.error(error);
        UIState.error(
            kontainer,
            error.message || "Terjadi kesalahan. Silakan coba lagi.",
            () => loadData(kontainer, fetchFn, renderFn),
        );
    }
}

// Penggunaan:
const kontainerKatalog = document.querySelector("#katalog-grid");

await loadData(
    kontainerKatalog,
    () => fetchData("/api/buku"),
    (data) => renderKatalog(data),
);
```

---

### 🏗️ Checkpoint Level 6

text

```
✅ Checklist sebelum lanjut ke Level 7:

PROYEK: Aplikasi Perpustakaan dengan API Nyata
├── Fetch data buku dari Open Library API (openlibrary.org)
├── Loading skeleton saat data sedang diambil
├── Error state dengan tombol "Coba Lagi"
├── Empty state saat tidak ada hasil pencarian
├── Simpan preferensi user di localStorage (tema, tampilan)
├── Simpan buku favorit di localStorage
├── Cache hasil API selama 5 menit
└── Abort fetch saat input pencarian berubah (tidak ada race condition)

ASYNC PATTERNS:
├── Semua fetch menggunakan async/await
├── Semua error ditangkap dengan try/catch
├── Promise.all untuk fetch paralel
└── AbortController untuk batalkan request lama

KEAMANAN:
├── Tidak ada innerHTML dengan data dari API (pakai textContent)
├── Sanitasi URL parameter sebelum digunakan
└── Tidak simpan data sensitif di localStorage

Git: feat: integrate Fetch API, localStorage, and handle loading/error states
```

---

## 🟣 LEVEL 7: ARSITEKTUR PROJECT DAN PRODUCTION (Minggu 24+)

> **Tema**: _"Dari kode yang bekerja ke aplikasi yang maintainable dan production-ready"_
> **Benang Merah**: Fitur sudah lengkap (Level 6) → organisasi kode yang bersih → performa → aksesibilitas → build tools → deploy
> **Output**: Aplikasi perpustakaan production-ready yang cepat, accessible, dan mudah dikembangkan

---

### P. Arsitektur Kode — Organisasi yang Scalable

#### [[34. Pola Arsitektur untuk Vanilla JS]]

JavaScript

```
// ─── Module Pattern menggunakan ES Modules ────────────────────────────────

// js/modules/api.js — semua operasi HTTP
export const API_BASE = "https://openlibrary.org";

export async function fetchBuku(query) {
    const url = `${API_BASE}/search.json?q=${encodeURIComponent(query)}&limit=20`;
    const response = await fetch(url);
    if (!response.ok) throw new Error(`API error: ${response.status}`);
    return response.json();
}

export async function fetchDetailBuku(key) {
    const response = await fetch(`${API_BASE}${key}.json`);
    if (!response.ok) throw new Error(`API error: ${response.status}`);
    return response.json();
}

// js/modules/storage.js — semua operasi localStorage
export const Storage = { /* ... */ };

// js/modules/ui.js — semua manipulasi DOM
export function renderKartuBuku(buku) { /* ... */ }
export function tampilkanLoading(kontainer) { /* ... */ }
export function tampilkanError(kontainer, pesan) { /* ... */ }

// js/modules/state.js — state management sederhana
class StateManager {
    #state;
    #listeners = [];

    constructor(stateAwal) {
        this.#state = stateAwal;
    }

    get() {
        return { ...this.#state }; // return copy, bukan reference
    }

    set(updater) {
        const stateLama = this.#state;
        this.#state = typeof updater === "function"
            ? updater(this.#state)
            : { ...this.#state, ...updater };

        // Notify semua listeners
        this.#listeners.forEach(fn => fn(this.#state, stateLama));
    }

    subscribe(listener) {
        this.#listeners.push(listener);
        // Return fungsi unsubscribe
        return () => {
            this.#listeners = this.#listeners.filter(fn => fn !== listener);
        };
    }
}

export const appState = new StateManager({
    buku: [],
    filter: { keyword: "", kategori: "semua" },
    loading: false,
    error: null,
    favorit: Storage.ambil("favorit", []),
});

// js/main.js — entry point, hubungkan semua modul
import { fetchBuku } from "./modules/api.js";
import { appState }  from "./modules/state.js";
import { renderKatalog, tampilkanLoading, tampilkanError } from "./modules/ui.js";
import { setupEventListeners } from "./modules/events.js";

async function inisialisasi() {
    setupEventListeners();

    // Subscribe ke perubahan state
    appState.subscribe((state) => {
        if (state.loading) {
            tampilkanLoading(kontainer);
        } else if (state.error) {
            tampilkanError(kontainer, state.error);
        } else {
            renderKatalog(state.buku);
        }
    });

    // Load data awal
    try {
        appState.set({ loading: true, error: null });
        const data = await fetchBuku("perpustakaan");
        appState.set({ buku: data.docs, loading: false });
    } catch (error) {
        appState.set({ error: error.message, loading: false });
    }
}

document.addEventListener("DOMContentLoaded", inisialisasi);
```

---

### Q. Performa Web — Aplikasi yang Cepat

#### [[35. Optimasi Performa — Teknik yang Paling Berdampak]]

JavaScript

```
// ─── Lazy Loading Gambar ──────────────────────────────────────────────────
// HTML:
// <img src="placeholder.jpg" data-src="buku-asli.jpg" loading="lazy" alt="...">

// Intersection Observer: load gambar saat mendekati viewport
const observer = new IntersectionObserver(
    (entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                const img = entry.target;
                img.src = img.dataset.src;
                img.removeAttribute("data-src");
                observer.unobserve(img);
            }
        });
    },
    { rootMargin: "200px" } // mulai load 200px sebelum masuk viewport
);

document.querySelectorAll("img[data-src]").forEach(img => observer.observe(img));

// ─── Virtual Scrolling untuk daftar panjang ───────────────────────────────
// Untuk daftar 10.000+ item: hanya render yang terlihat di viewport
class VirtualList {
    constructor(kontainer, items, itemHeight, renderFn) {
        this.kontainer  = kontainer;
        this.items      = items;
        this.itemHeight = itemHeight;
        this.renderFn   = renderFn;

        this.setup();
    }

    setup() {
        // Set total height agar scrollbar benar
        this.spacer = document.createElement("div");
        this.spacer.style.height = `${this.items.length * this.itemHeight}px`;
        this.kontainer.appendChild(this.spacer);

        this.kontainer.addEventListener("scroll", () => this.render());
        this.render();
    }

    render() {
        const scrollTop     = this.kontainer.scrollTop;
        const viewportHeight = this.kontainer.clientHeight;

        const startIndex = Math.floor(scrollTop / this.itemHeight);
        const endIndex   = Math.min(
            this.items.length - 1,
            Math.ceil((scrollTop + viewportHeight) / this.itemHeight),
        );

        // Render hanya item yang terlihat + buffer
        const buffer = 5;
        const from = Math.max(0, startIndex - buffer);
        const to   = Math.min(this.items.length - 1, endIndex + buffer);

        // ... render items dari from sampai to
    }
}

// ─── Debounce dan Throttle ────────────────────────────────────────────────
function throttle(fn, delay) {
    // Throttle: pastikan fn tidak dipanggil lebih sering dari delay ms
    // Berguna untuk scroll event, resize event
    let lastTime = 0;
    return function(...args) {
        const now = Date.now();
        if (now - lastTime >= delay) {
            lastTime = now;
            fn.apply(this, args);
        }
    };
}

window.addEventListener("scroll", throttle(() => {
    // Update navbar shadow saat scroll
    const navbar = document.querySelector(".navbar");
    navbar.classList.toggle("navbar--scrolled", window.scrollY > 10);
}, 100));

// ─── requestAnimationFrame untuk animasi yang smooth ─────────────────────
function animasiSmoothScroll(target) {
    const targetY = target.getBoundingClientRect().top + window.scrollY;
    const mulaiY  = window.scrollY;
    const jarak   = targetY - mulaiY;
    let mulaiWaktu;

    function step(waktuSekarang) {
        if (!mulaiWaktu) mulaiWaktu = waktuSekarang;
        const progress = Math.min((waktuSekarang - mulaiWaktu) / 500, 1);
        // Easing: ease-in-out
        const ease = progress < 0.5 ? 2 * progress ** 2 : 1 - (-2 * progress + 2) ** 2 / 2;

        window.scrollTo(0, mulaiY + jarak * ease);

        if (progress < 1) requestAnimationFrame(step);
    }

    requestAnimationFrame(step);
}
```

---

### R. Aksesibilitas — Web untuk Semua Orang

#### [[36. Aksesibilitas (a11y) — Praktik Wajib]]

HTML

```
<!-- ─── ARIA Attributes ──────────────────────────────────────────────────── -->

<!-- Roles: beri tahu screen reader peran elemen -->
<div role="dialog" aria-modal="true" aria-labelledby="modal-judul" aria-describedby="modal-deskripsi">
    <h2 id="modal-judul">Detail Buku</h2>
    <p id="modal-deskripsi">Informasi lengkap tentang buku yang dipilih.</p>
</div>

<!-- aria-label: label untuk elemen tanpa teks yang terlihat -->
<button aria-label="Tutup dialog">×</button>
<nav aria-label="Navigasi utama">...</nav>
<nav aria-label="Navigasi breadcrumb">...</nav>

<!-- aria-hidden: sembunyikan dari screen reader -->
<span aria-hidden="true">★★★★☆</span>
<span class="sr-only">Rating: 4 dari 5 bintang</span>

<!-- aria-expanded: status elemen yang bisa dibuka/tutup -->
<button aria-expanded="false" aria-controls="menu-mobile">☰ Menu</button>
<nav id="menu-mobile" hidden>...</nav>

<!-- aria-live: announce perubahan dinamis -->
<div aria-live="polite" id="status-pencarian">
    <!-- JS akan update teks di sini -->
    12 buku ditemukan
</div>

<!-- aria-current: tandai halaman aktif di navigasi -->
<nav>
    <a href="/beranda" aria-current="page">Beranda</a>
    <a href="/katalog">Katalog</a>
</nav>
```

CSS

```
/* ─── CSS untuk Aksesibilitas ──────────────────────────────────────────── */

/* Skip link: untuk pengguna keyboard, loncat ke konten utama */
.skip-link {
    position: absolute;
    top: -100px;
    left: 0;
    padding: 8px 16px;
    background: var(--warna-utama);
    color: white;
    z-index: 9999;
    transition: top 0.2s;
}

.skip-link:focus {
    top: 0;  /* muncul saat difokus dengan Tab */
}

/* Focus indicator yang jelas — jangan pernah hapus outline tanpa alternatif! */
:focus-visible {
    outline: 3px solid var(--warna-utama);
    outline-offset: 2px;
}

/* Sembunyikan secara visual tapi tetap bisa dibaca screen reader */
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
}

/* Jangan sembunyikan dengan display:none atau visibility:hidden — screen reader tidak bisa baca */
```

JavaScript

```
// ─── Focus Management ────────────────────────────────────────────────────

// Saat modal dibuka: pindahkan fokus ke dalam modal
function bukaModal(modal) {
    modal.removeAttribute("hidden");
    modal.setAttribute("aria-hidden", "false");

    // Fokus ke elemen pertama yang bisa difokus dalam modal
    const elemenFokusable = modal.querySelectorAll(
        'a, button, input, textarea, select, [tabindex]:not([tabindex="-1"])'
    );
    elemenFokusable[0]?.focus();

    // Trap fokus di dalam modal
    modal.addEventListener("keydown", trapFokus);
}

function trapFokus(event) {
    if (event.key !== "Tab") return;

    const elemenFokusable = this.querySelectorAll(
        'a, button, input, textarea, select, [tabindex]:not([tabindex="-1"])'
    );
    const pertama = elemenFokusable[0];
    const terakhir = elemenFokusable[elemenFokusable.length - 1];

    if (event.shiftKey) {
        // Tab + Shift: mundur
        if (document.activeElement === pertama) {
            terakhir.focus();
            event.preventDefault();
        }
    } else {
        // Tab: maju
        if (document.activeElement === terakhir) {
            pertama.focus();
            event.preventDefault();
        }
    }
}

// Saat modal ditutup: kembalikan fokus ke trigger
let elemenSebelumModal;

function sebelumBukaModal() {
    elemenSebelumModal = document.activeElement;
}

function tutupModal(modal) {
    modal.setAttribute("hidden", "");
    modal.setAttribute("aria-hidden", "true");
    modal.removeEventListener("keydown", trapFokus);
    elemenSebelumModal?.focus(); // kembalikan fokus
}
```

---

### S. Build Tools dan Deploy

#### [[37. Vite — Build Tool Modern untuk Vanilla JS]]

Bash

```
# Setup project baru dengan Vite
npm create vite@latest perpustakaan-web -- --template vanilla
cd perpustakaan-web
npm install
npm run dev    # development server dengan HMR
npm run build  # production build
npm run preview # preview production build

# Struktur project dengan Vite:
perpustakaan-web/
├── index.html          ← entry point HTML
├── public/             ← file statis (tidak diproses Vite)
│   ├── favicon.ico
│   └── robots.txt
├── src/
│   ├── main.js         ← entry point JavaScript
│   ├── style.css
│   └── modules/
│       ├── api.js
│       ├── ui.js
│       └── state.js
├── vite.config.js
└── package.json
```

JavaScript

```
// vite.config.js
import { defineConfig } from "vite";

export default defineConfig({
    // Base URL untuk deployment
    base: "/",

    // Build optimizations
    build: {
        outDir: "dist",
        minify: "terser",           // minifikasi JavaScript
        rollupOptions: {
            output: {
                // Pisahkan vendor libraries dari kode aplikasi
                manualChunks: {
                    vendor: ["lodash-es"],
                },
            },
        },
    },

    // Development server
    server: {
        port: 3000,
        open: true,  // auto-buka browser
        // Proxy API untuk development (hindari CORS)
        proxy: {
            "/api": {
                target: "http://localhost:8000",
                changeOrigin: true,
            },
        },
    },
});
```

#### [[38. Deploy ke Netlify dan Vercel]]

Bash

```
# ─── Deploy ke Netlify ────────────────────────────────────────────────────

# Cara 1: Drag and drop folder dist/ ke netlify.com (paling mudah!)

# Cara 2: CLI
npm install -g netlify-cli
netlify login
netlify deploy --prod --dir=dist

# Cara 3: Hubungkan ke GitHub repository
# netlify.com → New site → Import from Git → pilih repo
```

toml

```
# netlify.toml — konfigurasi Netlify
[build]
  command = "npm run build"  # perintah build
  publish = "dist"           # folder output

# Redirect: untuk Single Page Application (SPA)
[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

YAML

```
# .github/workflows/deploy.yml — CI/CD dengan GitHub Actions

name: Build dan Deploy

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "20"
          cache: "npm"

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Deploy ke Netlify
        if: github.ref == 'refs/heads/main'
        uses: netlify/actions/cli@master
        with:
          args: deploy --prod --dir=dist
        env:
          NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}
          NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
```

---

### 🏗️ Checkpoint Level 7 (Final)

text

```
✅ Checklist Akhir — Aplikasi Perpustakaan Production-Ready:

ARSITEKTUR:
├── ES Modules: setiap modul punya tanggung jawab yang jelas
├── State management: perubahan state → UI update otomatis
├── Separation of concerns: api.js, ui.js, state.js, events.js
└── Tidak ada kode global yang berantakan

PERFORMA:
├── Lighthouse score: Performance > 90
├── Lazy loading gambar (loading="lazy")
├── Debounce pada input, throttle pada scroll
└── Cache API response di localStorage

AKSESIBILITAS:
├── Lighthouse score: Accessibility > 90
├── Keyboard navigation bekerja sempurna
├── Screen reader: NVDA atau VoiceOver bisa navigasi
├── Focus management di modal
├── Semua gambar punya alt yang deskriptif
└── Warna kontras memenuhi WCAG AA

PRODUKSI:
├── npm run build: berhasil tanpa error atau warning
├── Deployed ke Netlify atau Vercel
├── Domain custom (opsional)
└── GitHub Actions: auto-deploy saat push ke main

Git: feat: production-ready app with modules, a11y, performance, CI/CD
```

---

## 📊 Ringkasan Progress Tracking

### Satu Project, 7 Level Enhancement

text

```
Level 1: Halaman HTML perpustakaan — struktur semantik, form, multi-halaman
  + Level 2: + CSS (warna, tipografi, box model) → halaman yang cantik
  + Level 3: + Flexbox, Grid, responsive → layout sempurna di semua layar
  + Level 4: + JavaScript dasar → konsol, variabel, fungsi, array methods
  + Level 5: + DOM manipulation → pencarian, filter, modal, notifikasi
  + Level 6: + Fetch API, localStorage → data dari API, persisten
  + Level 7: + Arsitektur, performa, aksesibilitas, deploy → production-ready
```

### Tabel Progress

|Level|Topik|Durasi|Output Konkret|
|---|---|---|---|
|🟢 **1**|1-8|Minggu 1-3|Website multi-halaman dengan HTML semantik|
|🔵 **2**|9-14|Minggu 3-6|Halaman yang cantik dengan CSS modern|
|🟡 **3**|