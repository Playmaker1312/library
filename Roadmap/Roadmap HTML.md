# 🗺️ Roadmap HTML: Step-by-Step Membangun Halaman Web Nyata

## Filosofi Roadmap Ini

> **"HTML bukan sekadar tag — HTML adalah bahasa yang menceritakan makna konten kepada browser, mesin pencari, dan manusia"** — setiap tag yang dipilih ada alasannya, bukan sekadar "yang penting tampil".

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: satu halaman web berkembang dari kosong ke portfolio lengkap
- **Semantic dari Awal**: bukan `<div>` untuk segalanya — pilih tag yang tepat sejak baris pertama
- **Visible Progress**: setiap poin = sesuatu yang bisa dilihat dan dibuka di browser
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: Halaman "Tentang Saya" — struktur dasar, teks, list
    ↓ (enhance, tidak mulai dari nol)
Level 2: + Navigasi, gambar, tabel, link antar halaman
    ↓ (enhance)
Level 3: + Formulir kontak yang lengkap dan tervalidasi
    ↓ (enhance)
Level 4: + Semantic HTML5, aksesibilitas, dan SEO dasar
    ↓ (enhance)
Level 5: + Multimedia responsif, formulir canggih, Web APIs
    ↓ (enhance)
Level 6: + Optimasi performa, keamanan, Web Components
    ↓ (enhance)
Level 7: + PWA, SSG, standar web — Portfolio siap publish
```

---

## 🟢 LEVEL 1: FONDASI — DOKUMEN HTML PERTAMA (Minggu 1-2)

> **Tema**: _"Dari file kosong ke halaman web pertama yang terbuka di browser"_  
> **Benang Merah**: Apa itu HTML → Cara kerja browser → Struktur dokumen → Tag teks → Halaman pertama  
> **Output**: Halaman "Tentang Saya" dengan teks, heading, list, dan bisa dibuka di browser

---

### A. Sebelum Menulis Tag Pertama

> 💡 **Mengapa dimulai di sini?** Banyak orang langsung menulis `<div>` tanpa tahu mengapa. Memahami cara kerja browser mengubah cara kita memilih tag — bukan asal tulis, tapi tulis dengan makna.

text

```
Benang Merah Bagian A:
Tidak ada file → Pahami apa itu HTML →
Pahami cara browser memproses HTML →
Setup editor → Buat file pertama →
Lihat hasilnya di browser
```

1. `[[1. Apa itu HTML — Bahasa Markup, Bukan Bahasa Pemrograman]]`
    
    - HTML = HyperText Markup Language — memberi **makna** pada konten, bukan instruksi
    - Perbedaan fundamental: HTML (konten & struktur), CSS (tampilan), JavaScript (perilaku)
    - Analogi: HTML adalah **kerangka** bangunan, CSS adalah cat dan dekorasi, JS adalah listrik
    - Evolusi singkat: HTML 1 → HTML 4 → XHTML → HTML5 (yang kita pakai sekarang)
    - _Langkah konkret_: Buka website apapun, klik kanan → "View Page Source" — lihat HTML mentahnya
2. `[[2. Cara Browser Membaca HTML — Dari File ke Tampilan]]`
    
    - Browser tidak "menjalankan" HTML — browser **mem-parse** dan membangun DOM tree
    - Urutan kerja browser: download HTML → parse → bangun DOM → download CSS → render → tampil
    - Mengapa urutan tag penting: browser membaca dari atas ke bawah
    - Error HTML tidak crash seperti error Python/JavaScript — browser mencoba "menebak"
    - _Langkah konkret_: Buka Chrome DevTools (F12) → tab Elements → lihat DOM tree dari halaman apapun
3. `[[3. Setup Environment — VS Code, Extension & Browser]]`
    
    - Install VS Code sebagai editor
    - Extension yang wajib: **Live Server** (preview real-time), **Prettier** (format kode)
    - Extension opsional: **HTML CSS Support**, **Auto Rename Tag**
    - Browser utama: Chrome (DevTools paling lengkap)
    - Atur VS Code: `editor.formatOnSave: true`
    - _Langkah konkret_: Install VS Code + Live Server, buka VS Code, siap untuk langkah berikutnya
4. `[[4. Membuat File HTML Pertama — hello.html]]`
    
    - Buat folder `belajar-html/` di Desktop
    - Buat file `hello.html` (ekstensi `.html` wajib)
    - Tulis satu baris: `Hello, World!`
    - Buka di browser: double-click file atau klik kanan → "Open with" → Chrome
    - Perhatikan: teks muncul tanpa format apapun
    - _Langkah konkret_: File terbuka di browser, teks "Hello, World!" tampil
5. `[[5. DOCTYPE Declaration — "Bahasa" yang Kita Gunakan]]`
    
    - `<!DOCTYPE html>` WAJIB ada di baris pertama setiap file HTML
    - Tanpa DOCTYPE: browser masuk "quirks mode" — perilaku tidak terduga
    - `<!DOCTYPE html>` artinya: "ini adalah dokumen HTML5, gunakan aturan HTML5"
    - Bukan tag HTML — ini instruksi untuk browser sebelum parsing dimulai
    - _Langkah konkret_: Tambahkan `<!DOCTYPE html>` di baris pertama `hello.html`
6. `[[6. Struktur Wajib Dokumen HTML — html, head, body]]`
    
    - Tambahkan struktur wajib ke `hello.html`:
        
        HTML
        
        ```
        <!DOCTYPE html>
        <html lang="id">
          <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Halaman Pertamaku</title>
          </head>
          <body>
            Hello, World!
          </body>
        </html>
        ```
        
    - `<html lang="id">`: root element, `lang` penting untuk screen reader dan SEO
    - `<head>`: metadata — tidak terlihat di halaman, tapi penting untuk browser
    - `<body>`: semua yang terlihat di halaman ada di sini
    - `charset="UTF-8"`: mendukung semua karakter termasuk emoji dan karakter khusus
    - _Langkah konkret_: Simpan dan reload browser — tab sekarang bertuliskan "Halaman Pertamaku"
7. `[[7. Konsep Tag, Elemen & Atribut — Tiga Istilah Berbeda]]`
    
    - **Tag**: `<p>` (opening) dan `</p>` (closing) — seperti kurung buka dan tutup
    - **Elemen**: keseluruhan `<p>Hello</p>` — tag + konten
    - **Atribut**: `<a href="url">` — informasi tambahan di dalam tag pembuka
    - Self-closing tag: `<br>`, `<img>`, `<input>` — tidak punya konten, tidak perlu closing tag
    - Aturan penulisan yang benar: atribut ditulis di dalam tag pembuka, nilai dalam tanda kutip
    - _Langkah konkret_: Identifikasi tag, elemen, dan atribut dari kode yang sudah dibuat

---

### B. Tag Teks — Memberi Makna pada Konten

> 💡 **Benang Merah ke A**: Struktur dokumen sudah ada (Poin 6). Sekarang kita isi `<body>` dengan konten. Setiap tag teks yang dipilih membawa **makna semantik** — bukan sekadar tampilan.

text

```
Benang Merah Bagian B:
Dokumen HTML siap (Poin 6) →
Isi body dengan konten bermakna →
Heading: hierarki judul konten →
Paragraf: blok teks →
Format teks: tebal, miring, penting →
Halaman mulai terbentuk
```

8. `[[8. Heading h1 hingga h6 — Hierarki Judul Konten]]`
    
    - Heading bukan tentang ukuran teks — tapi tentang **hierarki konten**
    - `<h1>`: judul utama halaman — **hanya SATU per halaman**
    - `<h2>`: sub-judul bagian utama
    - `<h3>` sampai `<h6>`: hierarki yang makin dalam
    - Aturan penting: jangan skip level (dari h1 langsung ke h3)
    - Analoginya: daftar isi buku — Bab (h1), Sub-bab (h2), Sub-sub-bab (h3)
    - _Langkah konkret_: Mulai membangun halaman "Tentang Saya":
        
        HTML
        
        ```
        <body>
          <h1>Halo, Saya [Nama Kamu]</h1>
          <h2>Tentang Saya</h2>
          <h2>Keahlian</h2>
          <h2>Pengalaman</h2>
          <h2>Kontak</h2>
        </body>
        ```
        
9. `[[9. Tag p — Paragraf sebagai Unit Teks Dasar]]`
    
    - `<p>` bukan sekadar "blok teks" — ini adalah unit **paragraf** yang bermakna
    - Browser otomatis tambahkan margin atas dan bawah — bukan dari `<br>`
    - Jangan gunakan `<br>` untuk membuat jarak antar paragraf — gunakan `<p>` baru
    - _Langkah konkret_: Tambahkan deskripsi diri di bagian "Tentang Saya":
        
        HTML
        
        ```
        <h2>Tentang Saya</h2>
        <p>Nama saya [Nama] dan saya adalah seorang [profesi/pelajar] yang tertarik dengan dunia web development.</p>
        <p>Saya sedang belajar HTML dan ini adalah halaman web pertama yang saya buat sendiri.</p>
        ```
        
10. `[[10. Tag br & hr — Jeda Baris & Pemisah Konten]]`
    
    - `<br>`: jeda baris — gunakan **hanya** untuk konten yang memang perlu baris baru (alamat, puisi, lirik)
    - **JANGAN** gunakan `<br><br>` untuk membuat jarak antar paragraf — itu tugas CSS
    - `<hr>`: garis horizontal — pemisah **konten yang berganti topik**, bukan dekorasi
    - _Langkah konkret_: Tambahkan `<hr>` antar seksi di halaman
11. `[[11. Tag strong & em — Penekanan Bermakna]]`
    
    - `<strong>`: **penting secara semantik** — screen reader membacanya dengan penekanan
    - `<em>`: **penekanan** — intonasi berbeda saat dibaca screen reader
    - `<b>` dan `<i>`: hanya visual, tanpa makna semantik — gunakan untuk styling
    - Kapan pakai `<strong>`: peringatan, kata kunci penting, informasi kritis
    - Kapan pakai `<em>`: kata yang memang perlu penekanan intonasi
    - _Langkah konkret_:
        
        HTML
        
        ```
        <p>Keahlian utama saya adalah <strong>HTML dan CSS</strong>.</p>
        <p>Saya <em>sangat</em> antusias belajar web development.</p>
        ```
        
12. `[[12. Tag Teks Lainnya — mark, small, del, sup, sub]]`
    
    - `<mark>`: teks yang disorot/highlighted — seperti stabilo kuning
    - `<small>`: teks kecil — hak cipta, catatan kaki, disclaimer
    - `<del>` + `<ins>`: teks dihapus dan ditambahkan — berguna untuk changelog/revisi
    - `<sup>`: superscript — pangkat matematika (x²), catatan kaki (¹)
    - `<sub>`: subscript — rumus kimia (H₂O), formula matematika
    - _Langkah konkret_:
        
        HTML
        
        ```
        <small>© 2024 [Nama Kamu]. Semua hak dilindungi.</small>
        ```
        

---

### C. Daftar — Mengorganisasi Konten

> 💡 **Benang Merah ke B**: Paragraf untuk teks berkesinambungan. Daftar untuk konten yang punya pola berulang: keahlian, pengalaman, langkah-langkah. Pilihan tag yang benar (ul vs ol) menyampaikan makna.

text

```
Benang Merah Bagian C:
Konten teks sudah ada (Poin 9-12) →
Beberapa konten lebih cocok sebagai daftar →
ul: daftar tanpa urutan → daftar keahlian →
ol: daftar berurutan → langkah-langkah →
dl: daftar definisi → istilah dan penjelasannya
```

13. `[[13. Tag ul — Daftar Tanpa Urutan & Kapan Digunakan]]`
    
    - `<ul>` = unordered list — urutan **tidak penting**
    - Gunakan untuk: keahlian, fitur produk, navigasi menu, daftar belanja
    - Setiap item menggunakan `<li>` (list item)
    - _Langkah konkret_: Tambahkan daftar keahlian:
        
        HTML
        
        ```
        <h2>Keahlian</h2>
        <ul>
          <li>HTML — struktur halaman web</li>
          <li>CSS — tampilan dan styling</li>
          <li>JavaScript — interaktivitas</li>
          <li>Git — version control</li>
        </ul>
        ```
        
14. `[[14. Tag ol — Daftar Berurutan & Atribut type]]`
    
    - `<ol>` = ordered list — urutan **penting dan bermakna**
    - Gunakan untuk: langkah-langkah tutorial, peringkat, resep masakan, instruksi
    - Atribut `type`: `"1"` (angka), `"A"` (huruf kapital), `"a"` (huruf kecil), `"I"` (romawi)
    - Atribut `start`: mulai dari angka berapa
    - _Langkah konkret_: Tambahkan langkah belajar yang kamu tempuh:
        
        HTML
        
        ```
        <h3>Urutan Belajar Web Development</h3>
        <ol>
          <li>Belajar HTML — struktur dan semantik</li>
          <li>Belajar CSS — styling dan layout</li>
          <li>Belajar JavaScript — logika dan interaktivitas</li>
          <li>Belajar framework (Vue, React, dll)</li>
        </ol>
        ```
        
15. `[[15. Nested List — Daftar di Dalam Daftar]]`
    
    - Daftar bisa bersarang: `<ul>` di dalam `<li>` dari `<ul>` lain
    - Gunakan dengan hemat — terlalu dalam (lebih dari 3 level) menyulitkan pembaca
    - Indentasi kode mencerminkan hierarki konten
    - _Langkah konkret_:
        
        HTML
        
        ```
        <ul>
          <li>Frontend
            <ul>
              <li>HTML</li>
              <li>CSS</li>
              <li>JavaScript</li>
            </ul>
          </li>
          <li>Backend
            <ul>
              <li>Node.js</li>
              <li>Database</li>
            </ul>
          </li>
        </ul>
        ```
        
16. `[[16. Tag dl, dt, dd — Daftar Definisi yang Sering Dilupakan]]`
    
    - `<dl>` = description list — untuk pasangan **istilah : definisi**
    - Gunakan untuk: glossary, FAQ (pertanyaan + jawaban), metadata (nama : nilai)
    - `<dt>` = description term (istilah)
    - `<dd>` = description details (definisi/penjelasan)
    - _Langkah konkret_: Tambahkan daftar keahlian dengan level:
        
        HTML
        
        ```
        <h3>Detail Keahlian</h3>
        <dl>
          <dt>HTML</dt>
          <dd>Bahasa markup untuk struktur halaman web. Level: Pemula</dd>
          
          <dt>CSS</dt>
          <dd>Bahasa styling untuk tampilan halaman web. Level: Pemula</dd>
          
          <dt>JavaScript</dt>
          <dd>Bahasa pemrograman untuk interaktivitas web. Level: Sedang dipelajari</dd>
        </dl>
        ```
        
17. `[[17. Tag blockquote, q & cite — Kutipan yang Semantik]]`
    
    - `<blockquote>`: kutipan panjang dari sumber eksternal — tampil sebagai blok terindentasi
    - `<q>`: kutipan singkat inline — browser otomatis tambahkan tanda kutip
    - `<cite>`: judul karya atau nama sumber yang dikutip
    - Atribut `cite` pada `<blockquote>`: URL sumber kutipan (tidak terlihat, tapi semantik)
    - _Langkah konkret_:
        
        HTML
        
        ```
        <blockquote cite="https://example.com">
          <p>The best way to learn web development is by building things.</p>
          <footer>— <cite>Web Development Best Practices</cite></footer>
        </blockquote>
        ```
        
18. `[[18. Tag code, pre & kbd — Konten Teknis]]`
    
    - `<code>`: potongan kode inline — `gunakan <code>const x = 5</code> untuk variabel`
    - `<pre>`: teks dengan format asli (whitespace dipertahankan) — untuk blok kode besar
    - `<kbd>`: tombol keyboard — "tekan <kbd>Ctrl</kbd>+<kbd>C</kbd> untuk copy"
    - `<samp>`: output dari program
    - Kombinasi `<pre><code>` = standar untuk blok kode di halaman web
    - _Langkah konkret_:
        
        HTML
        
        ```
        <p>Tag HTML pertama yang dipelajari adalah <code>&lt;p&gt;</code> untuk paragraf.</p>
        
        <pre><code><!DOCTYPE html>
        ```
        

<html lang="id">  
<head>  
<title>Contoh</title>  
</head>  
</html></code></pre>  
```

---

### 🏗️ Checkpoint Level 1

text

```
✅ Checklist sebelum lanjut ke Level 2:

FILE: tentang-saya.html
├── DOCTYPE dan struktur html/head/body yang benar
├── lang="id" di tag html
├── charset UTF-8 dan viewport meta
├── title yang deskriptif
├── Satu h1 sebagai judul utama
├── Beberapa h2 sebagai seksi (Tentang Saya, Keahlian, Pengalaman, Kontak)
├── Paragraf dengan tag p (bukan br untuk jarak)
├── strong dan em digunakan dengan benar (bukan b dan i untuk makna)
├── ul untuk daftar keahlian (urutan tidak penting)
├── ol untuk langkah belajar (urutan penting)
├── dl untuk istilah dan definisi keahlian
├── blockquote untuk kutipan inspirasi
├── code untuk menampilkan tag HTML di halaman
└── Halaman terbuka dengan baik di Chrome dan Firefox

Commit: docs: create about-me page with semantic HTML structure
```

---

## 🔵 LEVEL 2: TAUTAN, GAMBAR & TABEL (Minggu 2-4)

> **Tema**: _"Dari satu halaman ke beberapa halaman yang saling terhubung"_  
> **Benang Merah**: Satu halaman statis (Level 1) → tambahkan navigasi → tambahkan gambar → tambahkan tabel pengalaman → halaman terkoneksi  
> **Output**: Website mini 3 halaman: Beranda, Portofolio, Kontak — semua terhubung dengan navigasi

---

### D. Tautan — Menghubungkan Halaman & Konten

> 💡 **Benang Merah ke Level 1**: Di Level 1, semua konten ada di satu file. Web yang sesungguhnya adalah jaringan halaman yang terhubung — itulah makna "HyperText". Tag `<a>` adalah jiwa dari web.

text

```
Benang Merah Bagian D:
Satu halaman terisolasi (Level 1) →
Tag a: buat tautan ke halaman lain →
URL relatif: hubungkan file di folder yang sama →
URL absolut: hubungkan ke website eksternal →
Anchor dalam halaman: navigasi ke seksi tertentu →
Buat navigasi menu pertama
```

19. `[[19. Tag a — Hyperlink yang Membuat Web Jadi Web]]`
    
    - `<a>` = anchor — elemen yang mengubah teks menjadi tautan yang bisa diklik
    - Atribut `href` (hypertext reference): **wajib** — tanpanya, `<a>` tidak berfungsi sebagai tautan
    - Konten `<a>` bisa berupa teks, gambar, atau elemen lain
    - Aturan aksesibilitas: teks tautan harus deskriptif — **bukan** "klik di sini" atau "baca selengkapnya"
    - _Langkah konkret_: Buat file `index.html` (ini akan jadi halaman utama) dan `portofolio.html`
20. `[[20. URL Absolut vs URL Relatif — Dua Cara Berbeda]]`
    
    - **URL Absolut**: alamat lengkap — `https://github.com/username`
        - Gunakan untuk: link ke website **lain** yang berbeda domain
    - **URL Relatif**: alamat relatif terhadap lokasi file saat ini — `portofolio.html` atau `../images/foto.jpg`
        - Gunakan untuk: link ke file **dalam project yang sama**
    - Struktur folder yang akan kita buat:
        
        text
        
        ```
        website-portofolio/
        ├── index.html          ← halaman beranda
        ├── portofolio.html     ← halaman portofolio
        ├── kontak.html         ← halaman kontak
        └── images/
            └── foto-saya.jpg
        ```
        
    - _Langkah konkret_: Buat ketiga file dan folder images
21. `[[21. Membuat Navigasi Menu — Tautan Antar Halaman]]`
    
    - Tambahkan navigasi di semua halaman:
        
        HTML
        
        ```
        <nav>
          <ul>
            <li><a href="index.html">Beranda</a></li>
            <li><a href="portofolio.html">Portofolio</a></li>
            <li><a href="kontak.html">Kontak</a></li>
          </ul>
        </nav>
        ```
        
    - Gunakan `<nav>` (bukan `<div>`) — semantik dan penting untuk aksesibilitas
    - `<nav>` di dalam list `<ul>` adalah pola yang benar untuk menu navigasi
    - _Langkah konkret_: Navigasi berfungsi di ketiga halaman, klik antar halaman tanpa error
22. `[[22. Atribut target="_blank" — Tab Baru & Keamanannya]]`
    
    - `target="_blank"`: buka tautan di tab/jendela baru
    - **WAJIB** tambahkan `rel="noopener noreferrer"` saat `target="_blank"`:
        
        HTML
        
        ```
        <a href="https://github.com" target="_blank" rel="noopener noreferrer">
          GitHub Saya
        </a>
        ```
        
    - Tanpa `rel="noopener"`: halaman yang dibuka bisa mengakses dan memodifikasi halaman asli (security vulnerability)
    - Kapan pakai `_blank`: link ke website eksternal, dokumen PDF, file download
    - _Langkah konkret_: Tambahkan link ke GitHub/LinkedIn dengan `target="_blank"` yang aman
23. `[[23. Anchor Link — Navigasi ke Dalam Halaman]]`
    
    - Tambahkan `id` ke elemen tujuan, lalu href dengan `#id`:
        
        HTML
        
        ```
        <!-- Di atas halaman: -->
        <nav>
          <a href="#keahlian">Keahlian</a>
          <a href="#pengalaman">Pengalaman</a>
          <a href="#kontak">Kontak</a>
        </nav>
        
        <!-- Di seksi yang dituju: -->
        <section id="keahlian">
          <h2>Keahlian</h2>
          <!-- ... -->
        </section>
        ```
        
    - Berguna untuk halaman panjang dengan satu halaman (one-page website)
    - _Langkah konkret_: Navigasi klik → halaman scroll ke seksi yang tepat
24. `[[24. Tautan Email, Telepon & Download]]`
    
    - Email: `<a href="mailto:email@domain.com">Kirim Email</a>`
    - Telepon: `<a href="tel:+6281234567890">Hubungi Saya</a>`
    - Download: `<a href="dokumen/cv.pdf" download>Download CV</a>`
    - `download` attribute: paksa browser download file alih-alih membukanya
    - _Langkah konkret_: Halaman kontak memiliki link email yang membuka aplikasi email saat diklik

---

### E. Gambar & Media — Konten Visual

> 💡 **Benang Merah ke Tautan**: `<a>` membuat tautan teks. Gambar juga bisa dibuat sebagai tautan: `<a href="..."><img src="..." /></a>`. Kita mulai dari gambar mandiri, lalu gabungkan dengan tautan.

text

```
Benang Merah Bagian E:
Tautan teks sudah ada (Poin 19-24) →
Tambahkan gambar ke halaman →
alt: teks alternatif wajib →
figure + figcaption: gambar + keterangannya →
Gambar sebagai tautan →
Audio dan video untuk konten multimedia
```

25. `[[25. Tag img — Menyisipkan Gambar ke Halaman]]`
    
    - `<img>` adalah self-closing tag: tidak perlu `</img>`
    - Dua atribut **wajib**: `src` (sumber gambar) dan `alt` (teks alternatif)
    - Tambahkan foto profil ke halaman "Tentang Saya":
        
        HTML
        
        ```
        <img
          src="images/foto-saya.jpg"
          alt="Foto profil [Nama] — sedang tersenyum di depan laptop"
          width="200"
          height="200"
        >
        ```
        
    - `width` dan `height`: **wajib disertakan** — mencegah layout shift saat gambar loading
    - _Langkah konkret_: Simpan foto di folder `images/`, tampilkan di halaman
26. `[[26. Atribut alt — Lebih Penting dari Tampilan Gambar]]`
    
    - `alt` bukan optional — ini **wajib** untuk aksesibilitas dan SEO
    - `alt=""` (kosong): untuk gambar dekoratif yang tidak membawa informasi
    - `alt="[deskripsi]"`: untuk gambar yang membawa informasi
    - Aturan menulis alt yang baik:
        - Spesifik: bukan "gambar buku" tapi "Sampul buku Clean Code karya Robert Martin"
        - Tidak perlu "gambar dari..." — screen reader sudah tahu ini gambar
        - Panjang ideal: di bawah 125 karakter
    - _Langkah konkret_: Audit semua gambar yang ada — apakah alt-nya sudah deskriptif?
27. `[[27. Tag figure & figcaption — Gambar dengan Konteks]]`
    
    - `<figure>`: elemen yang membungkus gambar + keterangannya sebagai satu unit
    - `<figcaption>`: keterangan gambar yang terhubung secara semantik
    - Bukan hanya untuk gambar — bisa untuk kode, diagram, quote, dll
    - _Langkah konkret_: Update semua gambar menggunakan figure:
        
        HTML
        
        ```
        <figure>
          <img
            src="images/proyek-1.jpg"
            alt="Screenshot halaman beranda website portofolio dengan tema gelap"
            width="600"
            height="400"
          >
          <figcaption>
            Proyek 1: Website Portofolio — dibangun dengan HTML dan CSS murni
          </figcaption>
        </figure>
        ```
        
28. `[[28. Format Gambar — Memilih Format yang Tepat]]`
    
    - **JPEG/JPG**: foto, gambar dengan banyak warna — ukuran kecil, tidak support transparansi
    - **PNG**: gambar dengan transparansi, screenshot, logo — kualitas tinggi, ukuran lebih besar
    - **SVG**: ikon, logo, ilustrasi — vektor, skalabel tanpa buram, ukuran sangat kecil
    - **WebP**: format modern — lebih kecil dari JPEG/PNG, kualitas sama/lebih baik
    - **GIF**: animasi sederhana — kualitas rendah, gunakan video untuk animasi kompleks
    - _Langkah konkret_: Konversi foto ke WebP menggunakan Squoosh (squoosh.app), bandingkan ukuran
29. `[[29. Tag audio — Menyematkan Suara]]`
    
    - HTML
        
        ```
        <audio controls>
          <source src="audio/intro.mp3" type="audio/mpeg">
          <source src="audio/intro.ogg" type="audio/ogg">
          Browser Anda tidak mendukung audio HTML5.
        </audio>
        ```
        
    - `controls`: tampilkan kontrol play/pause (tanpa ini, audio tidak bisa dikontrol user)
    - `autoplay`: mulai otomatis — **hindari** kecuali ada alasan kuat (mengganggu user)
    - `muted`: mulai dalam kondisi mute
    - `loop`: putar ulang terus-menerus
    - Multiple `<source>`: sediakan beberapa format sebagai fallback
30. `[[30. Tag video — Menyematkan Video & iframe untuk YouTube]]`
    
    - Tag `<video>` untuk video yang di-host sendiri:
        
        HTML
        
        ```
        <video controls width="640" height="360" poster="images/thumbnail-video.jpg">
          <source src="video/demo.mp4" type="video/mp4">
          <source src="video/demo.webm" type="video/webm">
          Browser Anda tidak mendukung video HTML5.
        </video>
        ```
        
    - `poster`: gambar yang ditampilkan sebelum video diputar
    - `<iframe>` untuk embed video YouTube (bukan download, hanya embed):
        
        HTML
        
        ```
        <iframe
          width="560"
          height="315"
          src="https://www.youtube.com/embed/VIDEO_ID"
          title="Judul video untuk aksesibilitas"
          frameborder="0"
          allowfullscreen
          loading="lazy"
        ></iframe>
        ```
        
    - `loading="lazy"`: iframe tidak dimuat sampai mendekati viewport — performa lebih baik
    - _Langkah konkret_: Embed video demo proyek dari YouTube di halaman portofolio

---

### F. Tabel — Data Tabular yang Benar

> 💡 **Benang Merah ke Konten**: Kita sudah punya daftar pengalaman sebagai `<ul>`. Tapi data pengalaman kerja punya kolom (posisi, perusahaan, tahun, deskripsi) — ini adalah **data tabular** yang tepat menggunakan `<table>`.

text

```
Benang Merah Bagian F:
Daftar pengalaman dengan ul (Level 1) →
Data dengan kolom = tabel yang tepat →
thead, tbody, tfoot: struktur tabel semantik →
th: header kolom/baris →
colspan dan rowspan: sel yang span beberapa kolom/baris →
caption: judul tabel
```

31. `[[31. Kapan Gunakan Tabel — Data Tabular, Bukan Layout]]`
    
    - Tabel untuk data yang punya **baris dan kolom** yang bermakna
    - Tabel **BUKAN** untuk layout halaman (itu tugas CSS Grid/Flexbox)
    - Test sederhana: "Apakah data ini masuk akal jika dibaca dengan screen reader baris per baris?" → ya = gunakan tabel
    - Contoh tepat: jadwal, data keuangan, perbandingan fitur, daftar nilai
    - _Langkah konkret_: Identifikasi konten di halaman yang cocok jadi tabel
32. `[[32. Struktur Tabel Dasar — table, tr, th, td]]`
    
    - Buat tabel pengalaman kerja/pendidikan:
        
        HTML
        
        ```
        <table>
          <caption>Riwayat Pendidikan</caption>
          <thead>
            <tr>
              <th scope="col">Institusi</th>
              <th scope="col">Jurusan</th>
              <th scope="col">Tahun</th>
              <th scope="col">Status</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>Universitas XYZ</td>
              <td>Teknik Informatika</td>
              <td>2022 – Sekarang</td>
              <td>Sedang Berjalan</td>
            </tr>
            <tr>
              <td>SMA ABC</td>
              <td>IPA</td>
              <td>2019 – 2022</td>
              <td>Lulus</td>
            </tr>
          </tbody>
        </table>
        ```
        
    - _Langkah konkret_: Tabel tampil di browser dengan header yang berbeda dari data
33. `[[33. thead, tbody, tfoot — Struktur Semantik Tabel]]`
    
    - `<thead>`: baris header — browser tahu baris ini adalah kepala tabel
    - `<tbody>`: baris data — konten utama tabel
    - `<tfoot>`: baris footer — total, ringkasan, catatan (ditampilkan setelah tbody)
    - Mengapa penting: screen reader menggunakan ini untuk navigasi tabel; browser bisa mengulang thead di setiap halaman saat print
    - `<caption>`: **wajib ada** untuk aksesibilitas — judul tabel sebelum isinya
    - _Langkah konkret_: Semua tabel menggunakan thead, tbody, dan caption
34. `[[34. Atribut scope, colspan & rowspan]]`
    
    - `scope="col"`: header untuk kolom di bawahnya
    - `scope="row"`: header untuk baris di sampingnya
    - `colspan`: sel melebar ke beberapa kolom
    - `rowspan`: sel memanjang ke beberapa baris
    - _Langkah konkret_: Buat tabel perbandingan keahlian dengan colspan untuk kategori:
        
        HTML
        
        ```
        <tr>
          <th colspan="2">Frontend</th>
          <th colspan="2">Backend</th>
        </tr>
        <tr>
          <th scope="col">Teknologi</th>
          <th scope="col">Level</th>
          <th scope="col">Teknologi</th>
          <th scope="col">Level</th>
        </tr>
        ```
        

---

### 🏗️ Checkpoint Level 2

text

```
✅ Checklist sebelum lanjut ke Level 3:

STRUKTUR PROJECT:
website-portofolio/
├── index.html        (beranda dengan foto, bio, keahlian)
├── portofolio.html   (daftar proyek dengan gambar dan link)
├── kontak.html       (info kontak dengan link email dan telepon)
└── images/           (semua gambar dalam format optimal)

CHECKLIST TEKNIS:
├── Navigasi menu di semua halaman dengan tag nav
├── Link antar halaman menggunakan URL relatif
├── Link eksternal dengan target="_blank" dan rel="noopener noreferrer"
├── Anchor link untuk navigasi dalam halaman
├── Semua gambar punya alt yang deskriptif
├── Photo profil menggunakan figure + figcaption
├── Gambar dalam format WebP dengan JPEG sebagai fallback
├── width dan height di setiap img untuk mencegah layout shift
├── Tabel riwayat pendidikan dengan thead, tbody, caption
├── scope pada setiap th
└── Halaman valid di W3C Validator (validator.w3.org)

Commit: feat: add navigation, images, and education table
```

---

## 🟡 LEVEL 3: FORMULIR HTML (Minggu 4-6)

> **Tema**: _"Menerima input dari pengguna — formulir yang lengkap dan mudah digunakan"_  
> **Benang Merah**: Website statif (Level 2) → tambahkan formulir kontak → berbagai tipe input → validasi bawaan HTML5  
> **Output**: Halaman kontak dengan formulir lengkap: nama, email, subjek, pesan, dan validasi

---

### G. Formulir Dasar — Menerima Input User

> 💡 **Benang Merah ke Level 2**: Kita sudah punya halaman `kontak.html` dengan link email. Tapi link `mailto:` tidak selalu nyaman. Formulir HTML memungkinkan user mengirim pesan langsung dari halaman web.

text

```
Benang Merah Bagian G:
Halaman kontak dengan mailto link (Level 2) →
Formulir: wadah input yang terorganisir →
input: elemen masukan berbagai tipe →
label: aksesibilitas dan usability formulir →
fieldset + legend: grup elemen terkait
```

35. `[[35. Tag form — Wadah dan Konfigurasi Formulir]]`
    
    - `<form>` membungkus semua elemen input yang berkaitan
    - Atribut penting:
        - `action`: URL tujuan data dikirim (kosongkan untuk sekarang)
        - `method`: `GET` (data di URL) atau `POST` (data di body request)
    - Kapan `GET`: pencarian, filter — data boleh terlihat di URL
    - Kapan `POST`: login, pendaftaran, kirim pesan — data sensitif atau besar
    - _Langkah konkret_: Buat kerangka formulir kontak di `kontak.html`:
        
        HTML
        
        ```
        <main>
          <h1>Hubungi Saya</h1>
          <form action="#" method="POST">
            <!-- elemen form akan ditambahkan di sini -->
          </form>
        </main>
        ```
        
36. `[[36. Tag label & input — Pasangan yang Tidak Bisa Dipisah]]`
    
    - Setiap `<input>` **wajib** punya `<label>` — untuk aksesibilitas dan UX
    - Dua cara menghubungkan label ke input:
        
        HTML
        
        ```
        <!-- Cara 1: for + id (paling umum) -->
        <label for="nama">Nama Lengkap</label>
        <input type="text" id="nama" name="nama">
        
        <!-- Cara 2: label membungkus input (implicit) -->
        <label>
          Nama Lengkap
          <input type="text" name="nama">
        </label>
        ```
        
    - Manfaat: klik label → input otomatis fokus; screen reader membaca label saat input difokus
    - _Langkah konkret_: Tambahkan field nama ke formulir dengan label yang terhubung
37. `[[37. Tipe Input Teks — text, email, password, tel, url]]`
    
    - Setiap tipe input punya validasi dan tampilan keyboard berbeda (terutama di mobile):
        
        HTML
        
        ```
        <!-- Teks biasa: keyboard standar -->
        <input type="text" id="nama" name="nama" placeholder="Masukkan nama lengkap">
        
        <!-- Email: keyboard dengan @ dan titik -->
        <input type="email" id="email" name="email" placeholder="email@domain.com">
        
        <!-- Password: teks tersembunyi -->
        <input type="password" id="password" name="password">
        
        <!-- Telepon: keyboard angka di mobile -->
        <input type="tel" id="telepon" name="telepon" placeholder="+62 xxx xxxx xxxx">
        
        <!-- URL: keyboard dengan .com dan / -->
        <input type="url" id="website" name="website" placeholder="https://...">
        ```
        
    - _Langkah konkret_: Tambahkan field nama, email, dan telepon ke formulir kontak
38. `[[38. Tipe Input Pilihan — checkbox & radio]]`
    
    - **Checkbox** `type="checkbox"`: pilih **satu atau lebih** opsi
    - **Radio** `type="radio"`: pilih **tepat satu** dari kelompok
    - Radio harus punya `name` yang **sama** dalam satu kelompok:
        
        HTML
        
        ```
        <!-- Checkbox: bisa pilih lebih dari satu -->
        <fieldset>
          <legend>Topik yang Diminati</legend>
          <label>
            <input type="checkbox" name="topik" value="frontend"> Frontend Development
          </label>
          <label>
            <input type="checkbox" name="topik" value="backend"> Backend Development
          </label>
          <label>
            <input type="checkbox" name="topik" value="mobile"> Mobile Development
          </label>
        </fieldset>
        
        <!-- Radio: hanya bisa pilih satu -->
        <fieldset>
          <legend>Tujuan Kontak</legend>
          <label>
            <input type="radio" name="tujuan" value="kolaborasi"> Kolaborasi Proyek
          </label>
          <label>
            <input type="radio" name="tujuan" value="pertanyaan"> Pertanyaan
          </label>
          <label>
            <input type="radio" name="tujuan" value="lainnya"> Lainnya
          </label>
        </fieldset>
        ```
        
    - _Langkah konkret_: Tambahkan pilihan topik dan tujuan kontak ke formulir
39. `[[39. Tag textarea & select — Input Teks Panjang & Dropdown]]`
    
    - `<textarea>`: area teks multi-baris — untuk pesan panjang:
        
        HTML
        
        ```
        <label for="pesan">Pesan</label>
        <textarea
          id="pesan"
          name="pesan"
          rows="6"
          cols="50"
          placeholder="Tulis pesan Anda di sini..."
        ></textarea>
        ```
        
    - `<select>`: dropdown/daftar pilihan:
        
        HTML
        
        ```
        <label for="layanan">Jenis Layanan</label>
        <select id="layanan" name="layanan">
          <option value="">-- Pilih Layanan --</option>
          <optgroup label="Desain">
            <option value="ui-design">UI Design</option>
            <option value="ux-research">UX Research</option>
          </optgroup>
          <optgroup label="Pengembangan">
            <option value="web-dev">Web Development</option>
            <option value="mobile-dev">Mobile Development</option>
          </optgroup>
        </select>
        ```
        
    - _Langkah konkret_: Tambahkan textarea untuk pesan dan dropdown untuk layanan
40. `[[40. Tag fieldset & legend — Grup Elemen Formulir]]`
    
    - `<fieldset>`: mengelompokkan elemen formulir yang berkaitan secara semantik
    - `<legend>`: judul kelompok — dibaca screen reader sebagai konteks
    - Gunakan untuk: data pribadi, preferensi, detail pembayaran
    - Setelah menggunakan `<fieldset>`, formulir jadi lebih terstruktur:
        
        HTML
        
        ```
        <form method="POST">
          <fieldset>
            <legend>Informasi Pribadi</legend>
            <!-- nama, email, telepon -->
          </fieldset>
          
          <fieldset>
            <legend>Detail Pesan</legend>
            <!-- subjek, layanan, textarea pesan -->
          </fieldset>
          
          <fieldset>
            <legend>Preferensi</legend>
            <!-- checkbox dan radio -->
          </fieldset>
          
          <button type="submit">Kirim Pesan</button>
        </form>
        ```
        
    - _Langkah konkret_: Kelompokkan semua field formulir menggunakan fieldset

---

### H. Validasi & Atribut Input

> 💡 **Benang Merah ke Formulir Dasar**: Formulir sudah ada tapi user bisa submit dengan data kosong atau salah. HTML5 punya validasi bawaan — tanpa JavaScript sekalipun.

text

```
Benang Merah Bagian H:
Formulir tanpa validasi (Poin 35-40) →
required: wajib diisi →
minlength, maxlength: batasan panjang →
pattern: validasi dengan regex →
type email/url: validasi format otomatis →
Formulir tidak bisa submit jika ada error
```

41. `[[41. Atribut required, placeholder & autocomplete]]`
    
    - `required`: field wajib diisi — browser menolak submit jika kosong
    - `placeholder`: teks petunjuk yang hilang saat user mulai mengetik
    - `autocomplete`: hint untuk browser menawarkan isian otomatis:
        
        HTML
        
        ```
        <input
          type="text"
          id="nama"
          name="nama"
          required
          placeholder="Contoh: Budi Santoso"
          autocomplete="name"
        >
        
        <input
          type="email"
          id="email"
          name="email"
          required
          placeholder="email@domain.com"
          autocomplete="email"
        >
        ```
        
    - Nilai autocomplete yang berguna: `name`, `email`, `tel`, `address-line1`, `postal-code`
    - _Langkah konkret_: Tambahkan required dan autocomplete ke semua field yang sesuai
42. `[[42. Atribut minlength, maxlength, min, max]]`
    
    - Batasan panjang teks:
        
        HTML
        
        ```
        <input
          type="text"
          name="nama"
          minlength="2"
          maxlength="100"
          required
        >
        
        <textarea
          name="pesan"
          minlength="10"
          maxlength="1000"
          rows="6"
        ></textarea>
        ```
        
    - Batasan nilai angka:
        
        HTML
        
        ```
        <input
          type="number"
          name="umur"
          min="17"
          max="100"
          step="1"
        >
        ```
        
    - _Langkah konkret_: Tambahkan batasan yang masuk akal ke setiap field
43. `[[43. Atribut pattern — Validasi dengan Regex]]`
    
    - `pattern`: validasi format menggunakan regular expression
    - Contoh yang berguna:
        
        HTML
        
        ```
        <!-- Nomor telepon Indonesia -->
        <input
          type="tel"
          name="telepon"
          pattern="(\+62|62|0)8[1-9][0-9]{6,10}"
          title="Format: +628xxxxxxxxxx atau 08xxxxxxxxxx"
          placeholder="+628123456789"
        >
        
        <!-- Kode pos Indonesia (5 digit) -->
        <input
          type="text"
          name="kodepos"
          pattern="[0-9]{5}"
          title="Kode pos terdiri dari 5 digit angka"
          placeholder="12345"
        >
        ```
        
    - `title`: pesan yang muncul saat pattern tidak cocok
    - _Langkah konkret_: Tambahkan validasi nomor telepon ke formulir kontak
44. `[[44. Atribut disabled, readonly & autofocus]]`
    
    - `disabled`: elemen tidak bisa diinteraksi — datanya tidak dikirim ke server
    - `readonly`: bisa dibaca tapi tidak bisa diubah — datanya tetap dikirim
    - `autofocus`: elemen ini langsung fokus saat halaman dimuat — hanya satu per halaman
    - Kapan `disabled` vs `readonly`:
        - Disabled: field yang belum relevan (misal: kolom diskon sebelum ada promo)
        - Readonly: data yang ditampilkan dari sistem tapi tidak boleh diubah user
    - _Langkah konkret_: Tambahkan `autofocus` ke field nama agar user langsung bisa mengetik
45. `[[45. Tipe Input Date, Time & file]]`
    
    - Input tanggal dan waktu:
        
        HTML
        
        ```
        <!-- Tanggal: picker kalender di browser modern -->
        <input type="date" name="tanggal-lahir" min="1900-01-01" max="2024-12-31">
        
        <!-- Waktu: picker jam dan menit -->
        <input type="time" name="waktu-pertemuan" min="09:00" max="17:00">
        
        <!-- Tanggal dan waktu lokal -->
        <input type="datetime-local" name="jadwal">
        ```
        
    - Upload file:
        
        HTML
        
        ```
        <!-- Hanya gambar -->
        <input type="file" name="foto" accept="image/jpeg,image/png,image/webp">
        
        <!-- Beberapa file sekaligus -->
        <input type="file" name="dokumen" accept=".pdf,.doc,.docx" multiple>
        ```
        
    - _Langkah konkret_: Tambahkan field upload CV ke halaman kontak

---

### 🏗️ Checkpoint Level 3

text

```
✅ Checklist sebelum lanjut ke Level 4:

HALAMAN: kontak.html
├── form dengan method POST
├── Semua input punya label yang terhubung (for + id)
├── field: nama (text), email (email), telepon (tel), pesan (textarea)
├── Dropdown layanan dengan optgroup
├── Checkbox topik (multi-pilih) dan radio tujuan (satu pilihan)
├── Upload CV dengan accept yang dibatasi
├── Semua field wajib punya required
├── Email menggunakan type="email" (validasi otomatis)
├── Telepon menggunakan pattern untuk format Indonesia
├── minlength dan maxlength pada textarea pesan
├── fieldset dan legend untuk pengelompokan
├── autofocus pada field nama
├── autocomplete pada field yang relevan
├── Formulir tidak bisa disubmit jika field required kosong
└── Tombol submit yang deskriptif (bukan hanya "Submit")

Commit: feat: add comprehensive contact form with HTML5 validation
```

---

## 🟠 LEVEL 4: SEMANTIC HTML5 & AKSESIBILITAS (Minggu 6-9)

> **Tema**: _"Dari HTML yang bekerja ke HTML yang bermakna — untuk manusia, mesin, dan teknologi bantuan"_  
> **Benang Merah**: HTML yang fungsional (Level 1-3) → audit semantic → ganti div dengan tag bermakna → tambahkan ARIA → optimasi SEO  
> **Output**: Website yang sama tapi dengan HTML semantik penuh, skor aksesibilitas tinggi, dan SEO yang baik

---

### I. Elemen Semantik HTML5 — Tag yang Menceritakan Makna

> 💡 **Mengapa sekarang?** Kita sudah tahu cara membuat konten. Sekarang kita audit dan perbaiki — ganti semua `<div>` generik dengan tag yang menceritakan **fungsi** elemen tersebut.

text

```
Benang Merah Bagian I:
HTML yang bekerja tapi penuh div (Level 1-3) →
Audit: identifikasi div yang bisa diganti →
header, nav, main, footer: struktur halaman →
article, section, aside: organisasi konten →
Struktur yang jelas = lebih mudah dipelihara
```

46. `[[46. Struktur Halaman Semantik — header, nav, main, footer]]`
    
    - Bukan soal tampilan — tapi soal **makna dan fungsi** setiap bagian:
        
        HTML
        
        ```
        <!DOCTYPE html>
        <html lang="id">
        <head>...</head>
        <body>
        
          <!-- Kepala halaman: logo, judul site, nav utama -->
          <header>
            <h1>Nama Saya</h1>
            <nav aria-label="Navigasi utama">
              <ul>
                <li><a href="index.html">Beranda</a></li>
                <li><a href="portofolio.html">Portofolio</a></li>
                <li><a href="kontak.html">Kontak</a></li>
              </ul>
            </nav>
          </header>
          
          <!-- Konten utama: satu per halaman, tidak repeat di setiap halaman -->
          <main>
            <!-- konten halaman di sini -->
          </main>
          
          <!-- Bawah halaman: copyright, link sekunder -->
          <footer>
            <p>© 2024 Nama Saya. Semua hak dilindungi.</p>
          </footer>
          
        </body>
        </html>
        ```
        
    - `<main>`: hanya satu per halaman — konten inti yang berbeda di setiap halaman
    - `<header>` dan `<footer>` bisa muncul di dalam `<article>` atau `<section>` juga
    - _Langkah konkret_: Refactor semua halaman menggunakan struktur semantik ini
47. `[[47. article vs section vs div — Kapan Menggunakan Masing-masing]]`
    
    - `<article>`: konten yang **berdiri sendiri** dan bisa didistribusikan ulang
        - Blog post, artikel berita, komentar, kartu produk — bisa dicabut dan masih bermakna
    - `<section>`: pengelompokan konten **tematik** yang tidak berdiri sendiri
        - Bagian dari halaman: Keahlian, Pengalaman, Kontak — hanya bermakna dalam konteks halaman
    - `<div>`: pengelompokan **tanpa makna semantik** — hanya untuk kebutuhan CSS/JS
    - Aturan praktis: jika bisa diganti `<article>` atau `<section>`, ganti. Sisanya pakai `<div>`
    - _Langkah konkret_: Refactor `portofolio.html` — setiap item proyek jadi `<article>`:
        
        HTML
        
        ```
        <main>
          <h1>Portofolio Proyek</h1>
          
          <section aria-labelledby="proyek-web">
            <h2 id="proyek-web">Proyek Web</h2>
            
            <article>
              <h3>Website Portofolio</h3>
              <p>Deskripsi proyek...</p>
              <figure>
                <img src="images/porto-1.jpg" alt="Screenshot halaman beranda">
                <figcaption>Halaman beranda website portofolio</figcaption>
              </figure>
              <a href="https://github.com/..." target="_blank" rel="noopener noreferrer">
                Lihat di GitHub
              </a>
            </article>
            
            <!-- artikel proyek lainnya -->
          </section>
        </main>
        ```
        
48. `[[48. Tag aside, address, time & details]]`
    
    - `<aside>`: konten terkait tapi tidak inti — sidebar, info tambahan, iklan
        
        HTML
        
        ```
        <aside aria-label="Informasi tambahan">
          <h2>Teknologi yang Dipelajari</h2>
          <ul>...</ul>
        </aside>
        ```
        
    - `<address>`: informasi kontak — nama, email, lokasi
        
        HTML
        
        ```
        <address>
          <a href="mailto:nama@email.com">nama@email.com</a><br>
          <a href="tel:+6281234567890">+62 812 3456 7890</a><br>
          Jakarta, Indonesia
        </address>
        ```
        
    - `<time>`: tanggal/waktu yang dapat dibaca mesin:
        
        HTML
        
        ```
        <time datetime="2024-01-15">15 Januari 2024</time>
        <time datetime="PT30M">30 menit</time>
        ```
        
    - `<details>` + `<summary>`: accordion bawaan HTML, tanpa JavaScript:
        
        HTML
        
        ```
        <details>
          <summary>Teknologi yang Digunakan dalam Proyek Ini</summary>
          <ul>
            <li>HTML5</li>
            <li>CSS3</li>
            <li>JavaScript ES6+</li>
          </ul>
        </details>
        ```
        
    - _Langkah konkret_: Tambahkan `<address>` di halaman kontak, `<time>` di riwayat pendidikan, `<details>` di setiap proyek

---

### J. Aksesibilitas — Web untuk Semua Orang

> 💡 **Benang Merah ke Semantic**: Tag semantik (Poin 46-48) adalah fondasi aksesibilitas. ARIA (Accessible Rich Internet Applications) adalah lapisan tambahan untuk situasi di mana semantic HTML tidak cukup.

text

```
Benang Merah Bagian J:
Semantic HTML sudah benar (Poin 46-48) →
ARIA: tambahkan informasi yang tidak ada di HTML →
aria-label: label untuk elemen tanpa teks visible →
aria-labelledby: hubungkan elemen dengan heading →
tabindex: atur urutan navigasi keyboard →
Test: coba tanpa mouse, hanya keyboard
```

49. `[[49. ARIA — Kapan Gunakan dan Kapan Tidak]]`
    
    - Aturan pertama ARIA: **jangan gunakan ARIA jika HTML native sudah cukup**
    - `<nav>` sudah bermakna "navigasi" — tidak perlu `role="navigation"`
    - `<button>` sudah bermakna "tombol" — tidak perlu `role="button"`
    - Gunakan ARIA **hanya** ketika HTML tidak punya elemen yang tepat
    - Contoh yang tepat: custom dropdown yang dibuat dari `<div>` memerlukan ARIA
    - _Langkah konkret_: Audit — identifikasi mana yang butuh ARIA dan mana yang tidak
50. `[[50. aria-label, aria-labelledby & aria-describedby]]`
    
    - `aria-label`: memberi nama pada elemen yang tidak punya teks terlihat:
        
        HTML
        
        ```
        <!-- Tombol dengan ikon, tanpa teks -->
        <button aria-label="Buka menu navigasi">☰</button>
        
        <!-- Nav dengan label yang membedakan dari nav lain -->
        <nav aria-label="Navigasi utama">...</nav>
        <nav aria-label="Navigasi footer">...</nav>
        ```
        
    - `aria-labelledby`: rujuk ke elemen lain sebagai label:
        
        HTML
        
        ```
        <section aria-labelledby="judul-keahlian">
          <h2 id="judul-keahlian">Keahlian Teknis</h2>
          <!-- konten seksi -->
        </section>
        ```
        
    - `aria-describedby`: deskripsi tambahan (lebih detail dari label):
        
        HTML
        
        ```
        <input
          type="password"
          id="password"
          aria-describedby="password-hint"
        >
        <p id="password-hint" class="hint">
          Password minimal 8 karakter, harus mengandung huruf kapital dan angka.
        </p>
        ```
        
    - _Langkah konkret_: Tambahkan ARIA yang tepat ke tombol ikon dan section di semua halaman
51. `[[51. Navigasi Keyboard — tabindex & Skip Link]]`
    
    - Semua interaksi harus bisa dilakukan dengan keyboard saja
    - `tabindex="0"`: tambahkan ke elemen non-interaktif yang perlu bisa difokus
    - `tabindex="-1"`: bisa difokus via JavaScript tapi tidak masuk tab order
    - **Hindari** `tabindex` dengan nilai positif — mengacaukan urutan alami
    - Skip navigation link — wajib ada di setiap halaman:
        
        HTML
        
        ```
        <body>
          <!-- Skip link: tersembunyi tapi muncul saat difokus keyboard -->
          <a href="#konten-utama" class="skip-link">
            Langsung ke konten utama
          </a>
          
          <header>
            <!-- navigasi panjang -->
          </header>
          
          <main id="konten-utama">
            <!-- konten halaman -->
          </main>
        </body>
        ```
        
    - _Langkah konkret_: Tambahkan skip link di semua halaman, test dengan Tab key dari browser baru
52. `[[52. Menguji Aksesibilitas — Tools & Cara Pengujian]]`
    
    - **Lighthouse** (built-in Chrome): F12 → Lighthouse → pilih Accessibility → Analyze
    - **axe DevTools**: extension Chrome — temukan issue aksesibilitas lebih detail
    - **Tab key test**: navigasi seluruh halaman tanpa mouse — apakah semua elemen bisa dicapai?
    - **Screen reader test**: aktifkan NVDA (Windows gratis) atau VoiceOver (Mac)
    - Target skor aksesibilitas Lighthouse: minimal **90**
    - _Langkah konkret_: Jalankan Lighthouse di semua halaman, perbaiki semua issue aksesibilitas

---

### K. SEO dengan HTML — Ditemukan di Mesin Pencari

> 💡 **Benang Merah ke Semantik**: Semantic HTML sudah membantu SEO — mesin pencari memahami hierarki konten dari heading, `<article>`, dll. Sekarang kita tambahkan meta tags dan structured data.

text

```
Benang Merah Bagian K:
Semantic HTML membantu SEO (Poin 46-48) →
Meta tags: informasi untuk mesin pencari →
Open Graph: tampilan saat dibagikan di sosmed →
Structured data: data terstruktur untuk rich results →
Canonical: hindari duplikat konten
```

53. `[[53. Meta Tags Esensial untuk SEO]]`
    
    - Update `<head>` semua halaman:
        
        HTML
        
        ```
        <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          
          <!-- SEO -->
          <title>Budi Santoso — Web Developer | Jakarta</title>
          <meta name="description" content="Portofolio Budi Santoso, web developer yang berfokus pada HTML, CSS, dan JavaScript. Lihat proyek dan hubungi saya.">
          <meta name="author" content="Budi Santoso">
          <meta name="robots" content="index, follow">
          
          <!-- Canonical: beritahu Google halaman mana yang "asli" -->
          <link rel="canonical" href="https://budisantoso.com/index.html">
          
          <!-- Bahasa -->
          <link rel="alternate" hreflang="id" href="https://budisantoso.com/">
          
          <title>...</title>
        </head>
        ```
        
    - Title ideal: 50-60 karakter, mengandung keyword utama
    - Description: 150-160 karakter, deskriptif dan menarik (ini yang muncul di hasil Google)
    - _Langkah konkret_: Update meta tags semua halaman dengan title dan description unik
54. `[[54. Open Graph & Twitter Card — Tampilan di Media Sosial]]`
    
    - Open Graph: tampilan saat link dibagikan di Facebook, WhatsApp, LinkedIn:
        
        HTML
        
        ```
        <!-- Open Graph -->
        <meta property="og:type" content="website">
        <meta property="og:url" content="https://budisantoso.com/">
        <meta property="og:title" content="Budi Santoso — Web Developer">
        <meta property="og:description" content="Portfolio web developer berfokus pada frontend">
        <meta property="og:image" content="https://budisantoso.com/images/og-image.jpg">
        <meta property="og:locale" content="id_ID">
        
        <!-- Twitter Card -->
        <meta name="twitter:card" content="summary_large_image">
        <meta name="twitter:title" content="Budi Santoso — Web Developer">
        <meta name="twitter:description" content="Portfolio web developer berfokus pada frontend">
        <meta name="twitter:image" content="https://budisantoso.com/images/og-image.jpg">
        ```
        
    - Buat gambar OG: idealnya 1200×630px
    - Test dengan: Facebook Sharing Debugger, Twitter Card Validator
    - _Langkah konkret_: Tambahkan OG tags, buat og-image.jpg, test di debugger
55. `[[55. Structured Data — JSON-LD untuk Rich Results]]`
    
    - Structured data membantu Google menampilkan rich results (bintang rating, FAQ, dll)
    - Tambahkan di `<head>` menggunakan `<script type="application/ld+json">`:
        
        HTML
        
        ```
        <script type="application/ld+json">
        {
          "@context": "https://schema.org",
          "@type": "Person",
          "name": "Budi Santoso",
          "jobTitle": "Web Developer",
          "email": "budi@domain.com",
          "url": "https://budisantoso.com",
          "sameAs": [
            "https://github.com/budisantoso",
            "https://linkedin.com/in/budisantoso"
          ]
        }
        </script>
        ```
        
    - Test di: Google Rich Results Test (search.google.com/test/rich-results)
    - _Langkah konkret_: Tambahkan Person schema di `index.html`, test di Google Rich Results Test

---

### 🏗️ Checkpoint Level 4

text

```
✅ Checklist sebelum lanjut ke Level 5:

SEMANTIC HTML:
├── header, nav, main, footer di semua halaman
├── article untuk setiap proyek di portofolio
├── section dengan aria-labelledby untuk setiap bagian
├── aside untuk konten sekunder
├── address di halaman kontak
├── time dengan atribut datetime di riwayat
├── details + summary untuk info tambahan proyek
└── Tidak ada div yang seharusnya bisa jadi tag semantik

AKSESIBILITAS:
├── Skip navigation link di semua halaman
├── aria-label untuk nav yang ada lebih dari satu
├── aria-describedby untuk hint field formulir
├── Semua ikon-only button punya aria-label
├── Navigasi keyboard berfungsi di semua halaman
└── Skor Lighthouse Accessibility ≥ 90

SEO:
├── Title unik dan deskriptif di setiap halaman (50-60 karakter)
├── Meta description unik di setiap halaman (150-160 karakter)
├── rel canonical di setiap halaman
├── Open Graph tags lengkap
├── Twitter Card tags
├── JSON-LD Person schema
└── Validasi di W3C Validator dan Rich Results Test

Commit: feat: add semantic HTML5, accessibility improvements, and SEO meta tags
```

---

## 🔴 LEVEL 5: MULTIMEDIA RESPONSIF & FORMULIR LANJUTAN (Minggu 9-13)

> **Tema**: _"Gambar dan formulir yang beradaptasi dengan semua ukuran layar dan kebutuhan"_  
> **Benang Merah**: Gambar statis satu ukuran (Level 2) → gambar responsif → Web APIs → formulir canggih  
> **Output**: Galeri proyek responsif yang muat cepat di semua perangkat

---

### L. Gambar & Media Responsif

56. `[[56. Tag picture & srcset — Gambar yang Tepat untuk Layar yang Tepat]]`
    
    - `<picture>`: container untuk kondisi gambar berbeda
    - `srcset`: daftar sumber gambar dengan lebar yang berbeda
    - Browser memilih gambar yang paling efisien berdasarkan ukuran layar:
        
        HTML
        
        ```
        <picture>
          <!-- Format modern: WebP dengan fallback JPEG -->
          <source
            type="image/webp"
            srcset="
              images/proyek-400.webp 400w,
              images/proyek-800.webp 800w,
              images/proyek-1200.webp 1200w
            "
            sizes="(max-width: 600px) 400px, (max-width: 1000px) 800px, 1200px"
          >
          
          <!-- Fallback untuk browser lama -->
          <source
            type="image/jpeg"
            srcset="
              images/proyek-400.jpg 400w,
              images/proyek-800.jpg 800w,
              images/proyek-1200.jpg 1200w
            "
            sizes="(max-width: 600px) 400px, (max-width: 1000px) 800px, 1200px"
          >
          
          <!-- img wajib ada sebagai final fallback -->
          <img
            src="images/proyek-800.jpg"
            alt="Screenshot proyek website portofolio"
            width="800"
            height="600"
            loading="lazy"
          >
        </picture>
        ```
        
    - `sizes`: memberi tahu browser, pada kondisi layar ini, gambar akan berukuran sekian
    - _Langkah konkret_: Update semua gambar proyek di portofolio menggunakan `<picture>`
57. `[[57. Atribut loading="lazy" — Performa Gambar]]`
    
    - `loading="lazy"`: gambar tidak dimuat sampai mendekati viewport
    - `loading="eager"`: dimuat segera (default) — untuk gambar di atas fold
    - Aturan:
        - Gambar pertama yang terlihat saat halaman dibuka → `loading="eager"` (atau tidak perlu ditulis)
        - Semua gambar lain (di bawah fold) → `loading="lazy"`
    - `decoding="async"`: decode gambar secara asinkron, tidak memblokir rendering
    - _Langkah konkret_: Tambahkan `loading="lazy" decoding="async"` ke semua gambar kecuali hero image
58. `[[58. Optimasi Gambar — Kompresi & Format Modern]]`
    
    - Gunakan Squoosh (squoosh.app) untuk:
        - Konversi ke WebP — biasanya 25-35% lebih kecil dari JPEG dengan kualitas sama
        - Kompresi: quality 80% sudah cukup untuk web
        - Resize: jangan upload gambar 3000px jika hanya tampil 800px
    - Checklist optimasi gambar:
        - Format: WebP dengan JPEG fallback
        - Ukuran: sesuai ukuran tampil maksimum (tidak lebih besar)
        - Kompresi: quality 75-85%
        - `width` dan `height`: selalu ada untuk mencegah layout shift
    - _Langkah konkret_: Audit semua gambar, ukuran file setelah optimasi harus < 200KB per gambar

---

### M. Web Storage API — Simpan Data di Browser

> 💡 **Benang Merah ke Formulir**: Formulir kontak sudah ada. Bagaimana jika user mengisi setengah lalu tidak sengaja refresh? Web Storage bisa menyimpan progress pengisian formulir.

59. `[[59. localStorage — Simpan Preferensi User]]`
    
    - `localStorage`: simpan data yang tidak hilang meski browser ditutup
    - Gunakan untuk: preferensi user, draft pesan, history
    - _Langkah konkret_: Simpan progress formulir ke localStorage:
        
        HTML
        
        ```
        <script>
          // Simpan nilai field saat berubah
          document.getElementById('nama').addEventListener('input', function() {
            localStorage.setItem('form-nama', this.value);
          });
          
          // Restore nilai saat halaman dimuat
          document.addEventListener('DOMContentLoaded', function() {
            const savedNama = localStorage.getItem('form-nama');
            if (savedNama) {
              document.getElementById('nama').value = savedNama;
            }
          });
          
          // Hapus saat formulir berhasil disubmit
          document.querySelector('form').addEventListener('submit', function() {
            localStorage.removeItem('form-nama');
          });
        </script>
        ```
        
    - _Langkah konkret_: Isi formulir, refresh halaman — nilai masih ada
60. `[[60. Tag template — Blueprint HTML yang Tidak Dirender]]`
    
    - `<template>`: konten HTML yang tidak dirender sampai diaktifkan via JavaScript
    - Berguna untuk: konten yang dibuat dinamis, komponen yang diulang
    - _Langkah konkret_: Buat template kartu proyek yang bisa di-clone:
        
        HTML
        
        ```
        <template id="kartu-proyek-template">
          <article class="kartu-proyek">
            <figure>
              <img src="" alt="" width="400" height="300" loading="lazy">
              <figcaption></figcaption>
            </figure>
            <h3></h3>
            <p></p>
            <a href="#" target="_blank" rel="noopener noreferrer">Lihat Proyek</a>
          </article>
        </template>
        
        <script>
          const proyek = [
            { judul: 'Website Portofolio', deskripsi: '...', link: '#', gambar: 'porto.jpg' },
          ];
          
          const template = document.getElementById('kartu-proyek-template');
          const container = document.getElementById('daftar-proyek');
          
          proyek.forEach(p => {
            const clone = template.content.cloneNode(true);
            clone.querySelector('h3').textContent = p.judul;
            clone.querySelector('p').textContent = p.deskripsi;
            clone.querySelector('img').src = p.gambar;
            clone.querySelector('img').alt = `Screenshot ${p.judul}`;
            clone.querySelector('a').href = p.link;
            container.appendChild(clone);
          });
        </script>
        ```
        

---

### N. Formulir Lanjutan HTML5

61. `[[61. Tipe Input Lanjutan — range, color, search, datalist]]`
    
    - Tambahkan ke halaman kontak atau buat halaman preferensi:
        
        HTML
        
        ```
        <!-- Slider rating kepuasan -->
        <label for="rating">
          Seberapa puas Anda? 
          <output id="rating-output">5</output>/10
        </label>
        <input
          type="range"
          id="rating"
          name="rating"
          min="1"
          max="10"
          value="5"
          oninput="document.getElementById('rating-output').value = this.value"
        >
        
        <!-- Pilih warna tema preferensi -->
        <label for="warna-tema">Warna Tema Favorit</label>
        <input type="color" id="warna-tema" name="warna-tema" value="#3B82F6">
        
        <!-- Search dengan suggestion -->
        <input type="search" name="cari" list="teknologi-list" placeholder="Cari teknologi...">
        <datalist id="teknologi-list">
          <option value="HTML">
          <option value="CSS">
          <option value="JavaScript">
          <option value="Vue.js">
          <option value="React">
          <option value="Node.js">
        </datalist>
        ```
        
62. `[[62. tag meter & progress — Visualisasi Level dan Kemajuan]]`
    
    - `<meter>`: nilai dalam rentang yang diketahui — level keahlian, kapasitas
    - `<progress>`: kemajuan dari proses yang belum diketahui akhirnya
    - Tambahkan ke halaman "Tentang Saya" untuk level keahlian:
        
        HTML
        
        ```
        <dl>
          <dt>HTML</dt>
          <dd>
            <meter
              value="80"
              min="0"
              max="100"
              low="30"
              high="70"
              optimum="90"
              title="80 dari 100"
            >
              80%
            </meter>
            <small>80/100</small>
          </dd>
          
          <dt>CSS</dt>
          <dd>
            <meter value="70" min="0" max="100">70%</meter>
            <small>70/100</small>
          </dd>
        </dl>
        ```
        

---

### 🏗️ Checkpoint Level 5

text

```
✅ Checklist sebelum lanjut ke Level 6:
├── Semua gambar menggunakan <picture> dengan WebP + JPEG fallback
├── srcset dan sizes yang tepat di semua gambar
├── loading="lazy" pada semua gambar kecuali hero image
├── Semua gambar sudah dioptimasi (< 200KB per gambar)
├── localStorage menyimpan progress pengisian formulir
├── <template> digunakan untuk render kartu proyek dinamis
├── Input type="range" dengan output real-time
├── Input type="color" dengan preview
├── datalist untuk auto-suggest di search
├── meter untuk visualisasi level keahlian
└── Semua halaman lolos Lighthouse Performance ≥ 80

Commit: feat: add responsive images, Web Storage, and advanced form inputs
```

---

## ⚫ LEVEL 6: PERFORMA, KEAMANAN & WEB COMPONENTS (Minggu 13-18)

> **Tema**: _"Dari website yang berfungsi ke website yang cepat, aman, dan modular"_  
> **Benang Merah**: Website lengkap (Level 5) → optimasi loading → tambahkan keamanan → buat komponen yang reusable  
> **Output**: Website dengan Lighthouse score ≥ 90 di semua kategori, komponen Web Component pertama

---

### O. Optimasi Performa HTML

63. `[[63. Critical Rendering Path — Apa yang Memblokir Halaman]]`
    
    - Browser tidak bisa render konten sampai HTML dan CSS selesai diproses
    - JavaScript yang dimuat di `<head>` memblokir parsing HTML
    - CSS yang dimuat di `<head>` memblokir rendering (diperlukan untuk style)
    - Solusi:
        
        HTML
        
        ```
        <head>
          <!-- CSS: di head, akan memblokir render tapi diperlukan -->
          <link rel="stylesheet" href="styles.css">
          
          <!-- Preload: minta browser download lebih awal -->
          <link rel="preload" href="images/hero.webp" as="image">
          <link rel="preload" href="fonts/inter.woff2" as="font" type="font/woff2" crossorigin>
          
          <!-- Preconnect: buka koneksi ke domain eksternal lebih awal -->
          <link rel="preconnect" href="https://fonts.googleapis.com">
        </head>
        
        <body>
          <!-- konten halaman -->
          
          <!-- JavaScript: di akhir body atau gunakan defer/async -->
          <script src="app.js" defer></script>
        </body>
        ```
        
    - _Langkah konkret_: Pindahkan semua `<script>` ke akhir body atau tambahkan `defer`
64. `[[64. async vs defer — Cara Memuat Script yang Tidak Memblokir]]`
    
    - Tanpa atribut: download + parse + execute → **memblokir** HTML parsing
    - `async`: download paralel, execute segera setelah selesai download → **memblokir** sesaat
    - `defer`: download paralel, execute setelah HTML selesai diparse → **tidak memblokir**
    - Kapan pakai:
        - `defer`: script yang bergantung pada DOM (paling umum digunakan)
        - `async`: script independen (analytics, iklan)
        - Tanpa atribut: script yang harus jalan segera (sangat jarang)
    - _Langkah konkret_: Audit semua `<script>` — tambahkan `defer` atau pindahkan ke akhir body
65. `[[65. Resource Hints — Preload, Prefetch & Preconnect]]`
    
    - `preload`: sumber daya yang **pasti** dibutuhkan di halaman saat ini — muat lebih awal:
        
        HTML
        
        ```
        <!-- Preload font yang dipakai di atas fold -->
        <link rel="preload" href="fonts/inter-v13-latin-regular.woff2" as="font" type="font/woff2" crossorigin>
        
        <!-- Preload hero image -->
        <link rel="preload" href="images/hero.webp" as="image">
        ```
        
    - `prefetch`: sumber daya yang **mungkin** dibutuhkan di halaman berikutnya:
        
        HTML
        
        ```
        <!-- Prefetch halaman portofolio jika user kemungkinan ke sana -->
        <link rel="prefetch" href="portofolio.html">
        ```
        
    - `preconnect`: buka koneksi ke domain eksternal lebih awal:
        
        HTML
        
        ```
        <link rel="preconnect" href="https://fonts.googleapis.com">
        <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
        ```
        
    - _Langkah konkret_: Tambahkan preload untuk hero image dan font utama

---

### P. Keamanan HTML

66. `[[66. Content Security Policy — Mencegah XSS via Meta Tag]]`
    
    - XSS (Cross-Site Scripting): penyerang menyisipkan script berbahaya ke halaman
    - CSP membatasi dari mana script, style, dan gambar boleh dimuat:
        
        HTML
        
        ```
        <meta
          http-equiv="Content-Security-Policy"
          content="
            default-src 'self';
            script-src 'self';
            style-src 'self' https://fonts.googleapis.com;
            font-src 'self' https://fonts.gstatic.com;
            img-src 'self' data: https:;
            frame-ancestors 'none';
          "
        >
        ```
        
    - `'self'`: hanya boleh dari domain yang sama
    - `frame-ancestors 'none'`: mencegah halaman diembed di iframe situs lain (anti-clickjacking)
    - _Langkah konkret_: Tambahkan CSP meta tag, test di browser (periksa Console errors)
67. `[[67. Subresource Integrity — Verifikasi File dari CDN]]`
    
    - SRI: pastikan file dari CDN tidak dimodifikasi oleh pihak ketiga
    - Tambahkan `integrity` dan `crossorigin` ke script/style dari CDN:
        
        HTML
        
        ```
        <!-- Bootstrap dari CDN dengan SRI -->
        <link
          rel="stylesheet"
          href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css"
          integrity="sha384-9ndCyUaIbzAi2FUVXJi0CjmCapSmO7SnpJef0486qhLnuZ2cdeRhO02iuK6FUUVM"
          crossorigin="anonymous"
        >
        ```
        
    - Generate hash SRI di: srihash.org
    - _Langkah konkret_: Jika menggunakan library dari CDN, tambahkan SRI hash
68. `[[68. Atribut sandbox pada iframe — Membatasi Konten Eksternal]]`
    
    - `sandbox`: batasi apa yang bisa dilakukan konten di dalam iframe:
        
        HTML
        
        ```
        <!-- iframe YouTube yang aman -->
        <iframe
          src="https://www.youtube.com/embed/VIDEO_ID"
          sandbox="allow-scripts allow-same-origin"
          loading="lazy"
          title="Demo video proyek website portofolio"
        ></iframe>
        
        <!-- iframe untuk preview dokumen eksternal: sangat ketat -->
        <iframe
          src="https://docs.example.com/preview"
          sandbox
          title="Preview dokumen"
        ></iframe>
        ```
        
    - Izin sandbox: `allow-scripts`, `allow-forms`, `allow-same-origin`, `allow-popups`
    - Tanpa nilai = semua diblokir
    - _Langkah konkret_: Tambahkan `sandbox` yang tepat ke semua iframe di halaman

---

### Q. Web Components — Komponen Reusable Tanpa Framework

> 💡 **Benang Merah ke Template**: `<template>` (Poin 60) adalah fondasi Web Components. Sekarang kita buat Custom Element yang reusable — bisa digunakan seperti tag HTML biasa.

69. `[[69. Custom Elements — Membuat Tag HTML Sendiri]]`
    
    - Buat komponen kartu proyek yang bisa dipakai seperti tag HTML:
        
        HTML
        
        ```
        <!-- Penggunaan setelah didefinisikan -->
        <kartu-proyek
          judul="Website Portofolio"
          teknologi="HTML, CSS"
          gambar="images/porto.jpg"
          link="https://github.com/..."
        ></kartu-proyek>
        
        <script>
          class KartuProyek extends HTMLElement {
            connectedCallback() {
              const judul = this.getAttribute('judul');
              const teknologi = this.getAttribute('teknologi');
              const gambar = this.getAttribute('gambar');
              const link = this.getAttribute('link');
              
              this.innerHTML = `
                <article>
                  <figure>
                    <img src="${gambar}" alt="Screenshot ${judul}" loading="lazy">
                  </figure>
                  <h3>${judul}</h3>
                  <p>Teknologi: ${teknologi}</p>
                  <a href="${link}" target="_blank" rel="noopener noreferrer">
                    Lihat Proyek →
                  </a>
                </article>
              `;
            }
          }
          
          customElements.define('kartu-proyek', KartuProyek);
        </script>
        ```
        
    - _Langkah konkret_: Refactor halaman portofolio menggunakan `<kartu-proyek>`
70. `[[70. Shadow DOM — Enkapsulasi Style Komponen]]`
    
    - Shadow DOM: DOM terisolasi di dalam elemen — style tidak bocor ke luar atau ke dalam:
        
        JavaScript
        
        ```
        class KartuProyek extends HTMLElement {
          constructor() {
            super();
            // Buat shadow DOM
            this.attachShadow({ mode: 'open' });
          }
          
          connectedCallback() {
            this.shadowRoot.innerHTML = `
              <style>
                /* Style ini HANYA berlaku di dalam komponen ini */
                article {
                  border: 1px solid #e5e7eb;
                  border-radius: 8px;
                  overflow: hidden;
                }
                img {
                  width: 100%;
                  height: 200px;
                  object-fit: cover;
                }
              </style>
              
              <article>
                <img src="${this.getAttribute('gambar')}" alt="...">
                <div class="konten">
                  <h3>${this.getAttribute('judul')}</h3>
                </div>
              </article>
            `;
          }
        }
        ```
        
    - _Langkah konkret_: Update `KartuProyek` menggunakan Shadow DOM

---

### 🏗️ Checkpoint Level 6

text

```
✅ Checklist sebelum lanjut ke Level 7:

PERFORMA:
├── Semua script menggunakan defer atau di akhir body
├── preload untuk hero image dan font utama
├── prefetch untuk halaman yang kemungkinan dikunjungi
├── preconnect untuk domain font eksternal
└── Lighthouse Performance score ≥ 85

KEAMANAN:
├── Content Security Policy via meta tag
├── sandbox pada semua iframe
├── SRI hash pada semua resource CDN
├── rel="noopener noreferrer" pada semua target="_blank"
└── Tidak ada inline script yang tidak diperlukan

WEB COMPONENTS:
├── Custom element <kartu-proyek> terdefinisi dan berfungsi
├── Shadow DOM untuk enkapsulasi style
├── Halaman portofolio menggunakan custom element
└── Komponen bisa reuse hanya dengan menulis tag HTML

Commit: feat: optimize performance, add security headers, and create web component
```

---

## 🟣 LEVEL 7: PWA & PORTFOLIO SIAP PUBLISH (Minggu 18+)

> **Tema**: _"Dari website ke aplikasi web yang bisa diinstall dan ditemukan"_  
> **Benang Merah**: Website production-ready (Level 6) → Progressive Web App → Deploy → Portfolio published  
> **Output**: Website portofolio yang bisa diinstall di smartphone, live di internet, skor Lighthouse ≥ 90 di semua kategori

---

### R. Progressive Web App — Web yang Terasa seperti App

71. `[[71. Web App Manifest — Metadata Instalasi PWA]]`
    
    - Buat `manifest.json` di root project:
        
        JSON
        
        ```
        {
          "name": "Portofolio Budi Santoso",
          "short_name": "BudiPorto",
          "description": "Website portofolio web developer",
          "start_url": "/",
          "display": "standalone",
          "background_color": "#ffffff",
          "theme_color": "#3B82F6",
          "orientation": "portrait-primary",
          "icons": [
            {
              "src": "icons/icon-72.png",
              "sizes": "72x72",
              "type": "image/png"
            },
            {
              "src": "icons/icon-192.png",
              "sizes": "192x192",
              "type": "image/png",
              "purpose": "any maskable"
            },
            {
              "src": "icons/icon-512.png",
              "sizes": "512x512",
              "type": "image/png"
            }
          ],
          "screenshots": [
            {
              "src": "images/screenshot-desktop.jpg",
              "sizes": "1280x720",
              "type": "image/jpeg",
              "form_factor": "wide"
            },
            {
              "src": "images/screenshot-mobile.jpg",
              "sizes": "390x844",
              "type": "image/jpeg",
              "form_factor": "narrow"
            }
          ]
        }
        ```
        
    - Hubungkan ke HTML di semua halaman:
        
        HTML
        
        ```
        <link rel="manifest" href="/manifest.json">
        <meta name="theme-color" content="#3B82F6">
        ```
        
    - _Langkah konkret_: Buat ikon di berbagai ukuran menggunakan realfavicongenerator.net
72. `[[72. Service Worker — Halaman Tersedia Offline]]`
    
    - Service Worker: script yang berjalan di background browser, bisa intercept request
    - Daftarkan di `index.html`:
        
        HTML
        
        ```
        <script>
          if ('serviceWorker' in navigator) {
            navigator.serviceWorker
              .register('/sw.js')
              .then(() => console.log('Service Worker terdaftar'))
              .catch(err => console.error('Registrasi gagal:', err));
          }
        </script>
        ```
        
    - Buat `sw.js` di root project (strategi Cache First):
        
        JavaScript
        
        ```
        const CACHE_NAME = 'portofolio-v1';
        const URLS_TO_CACHE = [
          '/',
          '/index.html',
          '/portofolio.html',
          '/kontak.html',
          '/styles.css',
          '/images/hero.webp',
        ];
        
        self.addEventListener('install', event => {
          event.waitUntil(
            caches.open(CACHE_NAME)
              .then(cache => cache.addAll(URLS_TO_CACHE))
          );
        });
        
        self.addEventListener('fetch', event => {
          event.respondWith(
            caches.match(event.request)
              .then(response => response || fetch(event.request))
          );
        });
        ```
        
    - _Langkah konkret_: Test di Lighthouse → PWA section harus hijau; coba akses offline di DevTools
73. `[[73. Audit Akhir — Lighthouse di Semua Kategori]]`
    
    - Jalankan Lighthouse audit di `index.html`:
        - ✅ **Performance**: ≥ 90
        - ✅ **Accessibility**: ≥ 90
        - ✅ **Best Practices**: ≥ 90
        - ✅ **SEO**: ≥ 90
        - ✅ **PWA**: Semua kriteria terpenuhi
    - Perbaiki semua issue yang ditunjukkan Lighthouse
    - _Langkah konkret_: Screenshot hasil Lighthouse — ini adalah bukti kualitas website
74. `[[74. Validasi HTML — W3C Validator]]`
    
    - Validasi semua halaman di validator.w3.org
    - Perbaiki semua error (bukan hanya warning)
    - Error umum yang sering muncul:
        - Heading yang ter-skip (h1 → h3 tanpa h2)
        - `alt` yang hilang di `<img>`
        - Element yang tidak boleh bersarang (block element di dalam inline element)
        - Atribut yang sudah deprecated
    - _Langkah konkret_: Semua halaman 0 error di W3C Validator
75. `[[75. Deploy Website — GitHub Pages (Gratis)]]`
    
    - Upload project ke GitHub repository
    - Aktifkan GitHub Pages: Settings → Pages → Source: Deploy from branch → main
    - URL website: `https://username.github.io/nama-repo`
    - Custom domain (opsional): beli domain → tambahkan CNAME record
    - Alternatif gratis: Netlify Drop (drag & drop folder), Vercel
    - _Langkah konkret_: Website live dan bisa diakses dari internet
76. `[[76. Dokumentasi Project — README yang Baik]]`
    
    - Buat `README.md` di repository:
        
        Markdown
        
        ```
        # Portofolio Budi Santoso
        
        Website portofolio yang dibangun dengan HTML5, CSS3, dan JavaScript vanilla.
        
        ## 🌐 Demo
        [budisantoso.github.io/portofolio](https://budisantoso.github.io/portofolio)
        
        ## ✨ Fitur
        - Desain responsif untuk semua ukuran layar
        - Progressive Web App — bisa diinstall di smartphone
        - Aksesibilitas tinggi (Lighthouse Accessibility: 98)
        - SEO optimized dengan structured data
        - Formulir kontak dengan validasi HTML5
        
        ## 📊 Lighthouse Score
        | Kategori | Skor |
        |----------|------|
        | Performance | 94 |
        | Accessibility | 98 |
        | Best Practices | 95 |
        | SEO | 100 |
        
        ## 🛠️ Teknologi
        - HTML5 dengan semantic markup
        - CSS3 (Flexbox & Grid)
        - JavaScript ES6+ (Vanilla)
        - Web Components
        - Service Worker (PWA)
        ```
        
    - _Langkah konkret_: README selesai, repository siap ditampilkan di portofolio

---

### 🏗️ Proyek Level 7 — Selesai

text

```
PROYEK FINAL: "Portfolio Website — HTML Mastery"
──────────────────────────────────────────────────
STRUKTUR FILE:
website-portofolio/
├── index.html          (beranda: bio, foto, keahlian, kutipan)
├── portofolio.html     (daftar proyek dengan gambar responsif)
├── kontak.html         (formulir kontak dengan validasi penuh)
├── manifest.json       (PWA manifest)
├── sw.js               (Service Worker untuk offline)
├── styles.css          (styling — tidak dibahas di roadmap HTML)
├── app.js              (JavaScript — minimal, untuk komponen)
├── images/             (semua gambar dioptimasi, WebP format)
└── icons/              (ikon PWA berbagai ukuran)

SKOR TARGET:
├── Lighthouse Performance: ≥ 90
├── Lighthouse Accessibility: ≥ 95
├── Lighthouse Best Practices: ≥ 90
├── Lighthouse SEO: ≥ 95
├── Lighthouse PWA: Pass
├── W3C Validator: 0 error
└── Website live di GitHub Pages atau Netlify
```

---

## 📊 Ringkasan Progress Tracking

### Satu Project, 7 Level Enhancement

text

```
Level 1: tentang-saya.html — struktur dan teks dasar
  + Level 2: + navigasi, gambar, tabel, website 3 halaman
  + Level 3: + formulir kontak dengan validasi HTML5
  + Level 4: + semantic HTML5, aksesibilitas, SEO
  + Level 5: + gambar responsif, Web Storage, formulir lanjutan
  + Level 6: + optimasi performa, keamanan, Web Components
  + Level 7: + PWA, deploy ke internet, portfolio published
```

### Tabel Progress

|Level|Poin|Durasi|Output Konkret|
|---|---|---|---|
|🟢 **1**|1-18|Minggu 1-2|Halaman "Tentang Saya" terbuka di browser|
|🔵 **2**|19-34|Minggu 2-4|3 halaman terhubung dengan nav, gambar, tabel|
|🟡 **3**|35-45|Minggu 4-6|Formulir kontak dengan validasi HTML5|
|🟠 **4**|46-55|Minggu 6-9|Semantic penuh, aksesibilitas, SEO meta tags|
|🔴 **5**|56-62|Minggu 9-13|Gambar responsif, localStorage, formulir lanjutan|
|⚫ **6**|63-70|Minggu 13-18|Performa ≥ 85, keamanan, custom web component|
|🟣 **7**|71-76|Minggu 18+|PWA, live di internet, Lighthouse ≥ 90 semua|

---

### Benang Merah Utama Sepanjang Roadmap

text

```
Poin 4  (File html pertama)      → Fondasi semua halaman yang dibuat
Poin 6  (Struktur html/head/body)→ Template yang diulang di setiap halaman
Poin 8  (Heading hierarchy)      → Fondasi aksesibilitas dan SEO (Poin 51-55)
Poin 12 (BookCard + figure)      → Pola figure+figcaption dipakai di Level 5
Poin 21 (Navigasi menu)          → Direfactor ke semantic (Poin 46)
Poin 26 (alt yang deskriptif)    → Fondasi aksesibilitas gambar (Poin 49-50)
Poin 35 (Form dasar)             → Divalidasi (Poin 41-45), lanjutan (Poin 61-62)
Poin 46 (Semantic HTML5)         → Fondasi aksesibilitas (Poin 49-52) dan SEO (Poin 53-55)
Poin 56 (picture + srcset)       → Dioptimasikan lebih lanjut (Poin 58)
Poin 60 (Template tag)           → Fondasi Web Components (Poin 69-70)
Poin 65 (Resource hints)         → Bagian optimasi performa PWA (Poin 71-73)
Poin 71 (manifest.json)          → SW (Poin 72) → Audit (Poin 73) → Deploy (Poin 75)
```

---

## 💡 Cara Menggunakan Roadmap Ini

text

```
Setiap poin mengikuti format:
┌──────────────────────────────────────────────────────┐
│ 💡 Konteks: mengapa tag/fitur ini ada               │
│ 🔗 Benang Merah: koneksi ke poin sebelum/sesudah    │
│ 📋 Kode: contoh implementasi konkret                │
│ ✅ Langkah konkret: cara verifikasi berhasil        │
└──────────────────────────────────────────────────────┘
```

**Aturan yang Tidak Boleh Dilanggar:**

1. **Satu project dari awal hingga akhir** — jangan buat project baru setiap level
2. **Buka di browser setelah setiap perubahan** — visual feedback adalah guru terbaik
3. **Validasi di W3C Validator** setelah setiap level selesai
4. **Gunakan tab key** untuk test navigasi keyboard setelah Level 4
5. **Jalankan Lighthouse** setelah Level 5 dan perbaiki sebelum lanjut

---

_Roadmap HTML v1.0 — Step-by-Step, One Project, Semantic First_  
_Setiap tag dipilih dengan alasan — bukan sekadar "yang penting tampil"_