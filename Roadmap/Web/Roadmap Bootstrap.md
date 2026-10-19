# 🗺️ Roadmap Bootstrap: Step-by-Step Membangun UI Profesional

## Filosofi Roadmap Ini

> **"Bootstrap bukan sekadar copy-paste class — Bootstrap adalah sistem desain yang harus dipahami logikanya agar bisa dikustomisasi dan tidak terkurung oleh batasannya"** — setiap class yang dipakai ada alasannya.

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: landing page → dashboard admin → e-commerce — satu project yang terus berkembang
- **Visual Progress**: setiap poin = perubahan nyata yang terlihat di browser
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **Dokumentasi sebagai Teman**: setiap topik disertai cara membaca dokumentasi resmi Bootstrap

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: Landing Page Dasar — grid, typography, warna
    ↓ (enhance, tidak mulai dari nol)
Level 2: + Komponen dasar — card, button, form, badge
    ↓ (enhance)
Level 3: + Navigasi, tabel, komponen interaktif — modal, accordion
    ↓ (enhance)
Level 4: + Kustomisasi Sass — warna brand, breakpoint custom
    ↓ (enhance)
Level 5: + Responsif mendalam, performa, aksesibilitas
    ↓ (enhance)
Level 6: + Dark mode, komponen custom, build system
    ↓ (enhance — pilih jalur)
Level 7: Admin dashboard lengkap / E-commerce / Portfolio pro
```

---

## 🟢 LEVEL 1: FONDASI — GRID & TYPOGRAPHY (Minggu 1-4)

> **Tema**: _"Dari halaman kosong ke layout yang terstruktur dengan Bootstrap"_  
> **Benang Merah**: Setup Bootstrap → Grid system → Typography → Warna → Spacing → Layout halaman pertama  
> **Output**: Landing page dengan hero section, fitur cards, dan footer yang responsif

---

### A. Setup & Pemahaman Dasar

> 💡 **Mengapa dimulai di sini?** Sebelum menggunakan Bootstrap, kita perlu paham cara kerjanya — Bootstrap adalah layer di atas CSS, bukan pengganti CSS. Memahami ini mencegah kebingungan saat kode "tidak mau jalan".

text

```
Benang Merah Bagian A:
HTML & CSS sudah dipahami (dari roadmap sebelumnya) →
Bootstrap: library CSS yang menyediakan utility classes dan komponen →
Setup via CDN (cepat untuk belajar) →
Setup via npm + Vite (untuk project nyata) →
Pahami struktur file Bootstrap →
Hello World Bootstrap pertama
```

1. `[[1. Apa itu Bootstrap & Mengapa Digunakan]]`
    
    - Bootstrap = framework CSS yang menyediakan sistem grid, utilitas, dan komponen siap pakai
    - Kelebihan: konsisten, responsif otomatis, dokumentasi lengkap, ekosistem besar
    - Kekurangan: ukuran file besar jika tidak dioptimasi, tampilan "generic" jika tidak dikustomisasi
    - Bootstrap 5 vs Bootstrap 4: tidak butuh jQuery, lebih banyak utility, dark mode bawaan
    - _Langkah konkret_: Buka `getbootstrap.com/docs/5.3` → familiarisasi struktur dokumentasi
2. `[[2. Setup Bootstrap via CDN — Untuk Eksperimen Cepat]]`
    
    - Tambahkan ke `<head>` semua halaman:
        
        HTML
        
        ```
        <!DOCTYPE html>
        <html lang="id" data-bs-theme="light">
        <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>Bootstrap Project</title>
          <!-- Bootstrap CSS -->
          <link
            href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css"
            rel="stylesheet"
            integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN"
            crossorigin="anonymous"
          >
          <!-- Custom CSS (setelah Bootstrap) -->
          <link rel="stylesheet" href="styles.css">
        </head>
        <body>
          <!-- konten -->
          
          <!-- Bootstrap JS Bundle (termasuk Popper) -->
          <script
            src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"
            integrity="sha384-C6RzsynM9kWDrMNeT87bh95OGNyZPhcTNXj1NW7RuBCsyN/o0jlpcV8Qyq46cDfL"
            crossorigin="anonymous"
          ></script>
        </body>
        </html>
        ```
        
    - Catatan: `bootstrap.bundle.min.js` sudah termasuk Popper.js (untuk dropdown, tooltip, dll)
    - _Langkah konkret_: Buat `index.html` dengan template di atas, buka di browser — pastikan tidak ada error di console
3. `[[3. Setup Bootstrap via npm + Vite — Untuk Project Nyata]]`
    
    - Buat project Vite:
        
        Bash
        
        ```
        npm create vite@latest bootstrap-project -- --template vanilla
        cd bootstrap-project
        npm install
        npm install bootstrap
        ```
        
    - Import di `main.js`:
        
        JavaScript
        
        ```
        // Import Bootstrap CSS
        import 'bootstrap/dist/css/bootstrap.min.css';
        // Import Bootstrap JS
        import 'bootstrap/dist/js/bootstrap.bundle.min.js';
        // Import custom CSS
        import './style.css';
        ```
        
    - Atau import Sass (untuk kustomisasi — Level 4):
        
        JavaScript
        
        ```
        // Di main.js:
        import './scss/main.scss';
        ```
        
        SCSS
        
        ```
        // Di scss/main.scss:
        @import 'bootstrap/scss/bootstrap';
        ```
        
    - _Langkah konkret_: Setup project Vite + Bootstrap, jalankan `npm run dev`
4. `[[4. Memahami File Bootstrap — CSS, JS & Bundle]]`
    
    - `bootstrap.css`: semua CSS Bootstrap (~230KB)
    - `bootstrap.min.css`: versi minified (~190KB) — gunakan di production
    - `bootstrap.bundle.js`: Bootstrap JS + Popper.js — gunakan ini (tidak perlu import Popper terpisah)
    - Di VS Code: install **Bootstrap 5 Quick Snippets** extension untuk autocomplete class
    - _Langkah konkret_: Buka DevTools → Network tab → reload halaman → cari file bootstrap — berapa ukurannya?

---

### B. Grid System — Pondasi Layout Bootstrap

> 💡 **Benang Merah ke Setup**: Bootstrap sudah ter-load. Sekarang kita pelajari grid system — sistem 12 kolom yang menjadi fondasi semua layout di Bootstrap.

text

```
Benang Merah Bagian B:
Bootstrap ter-load (Poin 2-3) →
Grid: container → row → col →
12 kolom yang bisa dibagi dengan berbagai kombinasi →
Breakpoints: tampilan berbeda di ukuran layar berbeda →
Layout hero section pertama
```

5. `[[5. Konsep Grid 12 Kolom — Container, Row & Col]]`
    
    - Tiga elemen wajib dan urutannya:
        
        HTML
        
        ```
        <!-- Container: batas kiri-kanan konten -->
        <div class="container">
          <!-- Row: baris, tempat kolom berada -->
          <div class="row">
            <!-- Col: kolom, pembagian dari 12 -->
            <div class="col-6">Kolom 1 (6/12 = 50%)</div>
            <div class="col-6">Kolom 2 (6/12 = 50%)</div>
          </div>
          <div class="row">
            <div class="col-4">4/12 ≈ 33%</div>
            <div class="col-4">4/12 ≈ 33%</div>
            <div class="col-4">4/12 ≈ 33%</div>
          </div>
        </div>
        ```
        
    - Aturan: kolom harus langsung di dalam row, row harus di dalam container
    - _Langkah konkret_: Buat 4 contoh layout berbeda dengan total kolom = 12
6. `[[6. Container Variants — container, container-fluid & container-{breakpoint}]]`
    
    - `.container`: lebar maksimum tergantung breakpoint, ada margin kiri-kanan auto
    - `.container-fluid`: selalu 100% lebar viewport
    - `.container-md`: fluid sampai breakpoint md, lalu fixed
    - _Langkah konkret_:
        
        HTML
        
        ```
        <!-- Perbandingan container variants: -->
        <div class="container bg-primary text-white p-3 mb-3">
          .container — width berubah di tiap breakpoint
        </div>
        <div class="container-fluid bg-success text-white p-3 mb-3">
          .container-fluid — selalu 100%
        </div>
        <div class="container-md bg-warning p-3">
          .container-md — fluid di bawah md, fixed di atas md
        </div>
        ```
        
    - _Langkah konkret_: Resize browser dan lihat perbedaan perilaku setiap container
7. `[[7. Breakpoints — Responsif dari Mobile ke Desktop]]`
    
    - Enam breakpoint Bootstrap 5:
        
        text
        
        ```
        xs: < 576px   (tidak ada class prefix — default)
        sm: ≥ 576px   (col-sm-*)
        md: ≥ 768px   (col-md-*)
        lg: ≥ 992px   (col-lg-*)
        xl: ≥ 1200px  (col-xl-*)
        xxl: ≥ 1400px (col-xxl-*)
        ```
        
    - Mobile-first: class tanpa prefix berlaku di SEMUA ukuran, class dengan prefix berlaku dari ukuran itu ke atas
    - _Langkah konkret_: Buat kolom responsif untuk hero section:
        
        HTML
        
        ```
        <div class="row">
          <!-- Mobile: full width, Desktop: 8 kolom -->
          <div class="col-12 col-lg-8">
            <h1>Judul Hero</h1>
            <p>Deskripsi singkat...</p>
          </div>
          <!-- Mobile: full width, Desktop: 4 kolom -->
          <div class="col-12 col-lg-4">
            <img src="hero.svg" class="img-fluid" alt="Hero illustration">
          </div>
        </div>
        ```
        
8. `[[8. Auto-layout & Equal-width Columns]]`
    
    - `.col` tanpa angka: kolom membagi rata ruang yang tersedia
    - `.col-auto`: lebar sesuai konten
    - _Langkah konkret_:
        
        HTML
        
        ```
        <!-- 3 kolom sama lebar otomatis -->
        <div class="row">
          <div class="col bg-light border p-3">Kolom 1</div>
          <div class="col bg-light border p-3">Kolom 2</div>
          <div class="col bg-light border p-3">Kolom 3</div>
        </div>
        
        <!-- Satu kolom fixed, satu kolom mengisi sisa -->
        <div class="row">
          <div class="col-auto bg-primary text-white p-3">Sidebar (auto)</div>
          <div class="col bg-light p-3">Main content (mengisi sisa)</div>
        </div>
        ```
        
9. `[[9. Gutter — Jarak Antar Kolom]]`
    
    - Gutter: spasi antar kolom, dikontrol di level `.row`
    - `g-{n}`: gutter horizontal DAN vertikal
    - `gx-{n}`: hanya horizontal, `gy-{n}`: hanya vertikal
    - Nilai 0-5 (default: `g-3` untuk normal)
    - _Langkah konkret_: Buat card grid dengan gutter yang berbeda:
        
        HTML
        
        ```
        <!-- Cards dengan gutter -->
        <div class="row g-4">
          <div class="col-12 col-md-6 col-lg-4">
            <div class="card h-100"><!-- card content --></div>
          </div>
          <!-- ... repeat -->
        </div>
        
        <!-- Gutter hanya vertikal (untuk list items) -->
        <div class="row gy-3 gx-0">
          <!-- ... -->
        </div>
        ```
        
10. `[[10. Offset, Order & Nesting — Grid Lanjutan]]`
    
    - **Offset**: geser kolom ke kanan:
        
        HTML
        
        ```
        <div class="row">
          <div class="col-4 offset-4">
            Ditengah (offset 4 dari kiri)
          </div>
        </div>
        ```
        
    - **Order**: ubah urutan tampilan tanpa ubah HTML:
        
        HTML
        
        ```
        <div class="row">
          <div class="col order-3">Pertama di HTML, ketiga di tampilan</div>
          <div class="col order-1">Kedua di HTML, pertama di tampilan</div>
          <div class="col order-2">Ketiga di HTML, kedua di tampilan</div>
        </div>
        ```
        
    - **Nesting**: row di dalam col:
        
        HTML
        
        ```
        <div class="row">
          <div class="col-8">
            <!-- Row di dalam col! -->
            <div class="row">
              <div class="col-6">Nested 1</div>
              <div class="col-6">Nested 2</div>
            </div>
          </div>
          <div class="col-4">Sidebar</div>
        </div>
        ```
        
    - _Langkah konkret_: Buat layout dengan sidebar di kanan (meski HTML sidebar di bawah konten) menggunakan `order`

---

### C. Typography & Warna

> 💡 **Benang Merah ke Grid**: Grid mengatur layout elemen. Typography dan warna mengatur tampilan teks dan latar belakang. Keduanya menggunakan class Bootstrap yang sama sistemnya.

11. `[[11. Heading, Display & Lead — Hierarki Teks]]`
    
    - _Langkah konkret_: Buat hero section dengan typography yang benar:
        
        HTML
        
        ```
        <section class="py-5">
          <div class="container">
            <!-- Display: heading yang sangat besar -->
            <h1 class="display-4 fw-bold">Selamat Datang di Bootstrap</h1>
            <!-- Lead: paragraf pengantar yang lebih besar -->
            <p class="lead text-muted">
              Belajar membangun UI profesional dengan Bootstrap 5.
            </p>
            <!-- Heading biasa -->
            <h2>Fitur Utama</h2>
            <p>Deskripsi fitur...</p>
          </div>
        </section>
        ```
        
    - Display classes: `display-1` hingga `display-6` (dari terbesar ke terkecil)
    - `.lead`: paragraf lebih besar dari default, untuk pengantar
12. `[[12. Text Utilities — Alignment, Weight & Transform]]`
    
    - _Langkah konkret_:
        
        HTML
        
        ```
        <!-- Alignment -->
        <p class="text-start">Rata kiri (default)</p>
        <p class="text-center">Rata tengah</p>
        <p class="text-end">Rata kanan</p>
        <!-- Responsif: mobile center, desktop start -->
        <p class="text-center text-md-start">Responsif</p>
        
        <!-- Font weight -->
        <p class="fw-bold">Bold (700)</p>
        <p class="fw-semibold">Semibold (600)</p>
        <p class="fw-normal">Normal (400)</p>
        <p class="fw-light">Light (300)</p>
        
        <!-- Transform -->
        <p class="text-uppercase">semua huruf kapital</p>
        <p class="text-lowercase">SEMUA HURUF KECIL</p>
        <p class="text-capitalize">setiap kata kapital</p>
        
        <!-- Truncate panjang -->
        <p class="text-truncate" style="max-width: 200px;">
          Teks yang sangat panjang akan dipotong dengan ellipsis...
        </p>
        ```
        
13. `[[13. Warna Semantik Bootstrap — Sistem Warna yang Bermakna]]`
    
    - Delapan warna semantik Bootstrap:
        
        text
        
        ```
        primary  → biru (aksi utama)
        secondary → abu-abu (aksi sekunder)
        success  → hijau (berhasil)
        danger   → merah (error/berbahaya)
        warning  → kuning (peringatan)
        info     → cyan (informasi)
        light    → abu muda (latar terang)
        dark     → hitam (latar gelap)
        ```
        
    - Text colors: `text-primary`, `text-danger`, `text-muted`, `text-white`
    - Background colors: `bg-primary`, `bg-success`, `bg-light`
    - _Langkah konkret_: Buat color swatch untuk semua warna semantik
14. `[[14. Spacing Utilities — Margin & Padding Sistem]]`
    
    - Format: `{property}{sides}-{size}` atau `{property}{sides}-{breakpoint}-{size}`
    - Property: `m` (margin) atau `p` (padding)
    - Sides: `t` (top), `b` (bottom), `s` (start/left), `e` (end/right), `x` (horizontal), `y` (vertikal), kosong (semua)
    - Size: 0-5 + auto
    - _Langkah konkret_: Section dengan spacing yang benar:
        
        HTML
        
        ```
        <!-- Section dengan padding vertikal besar, margin bawah -->
        <section class="py-5 mb-5">
          <!-- Container dengan padding horizontal -->
          <div class="container px-4 px-lg-5">
            <!-- Heading dengan margin bawah -->
            <h2 class="text-center mb-4">Fitur Kami</h2>
            <!-- Row dengan gap antar card -->
            <div class="row g-4">
              <!-- Card tanpa padding tambahan (sudah ada di card-body) -->
              <div class="col-md-4">
                <div class="card">
                  <div class="card-body p-4">
                    <h5 class="card-title mb-3">Fitur 1</h5>
                    <p class="card-text text-muted">Deskripsi fitur...</p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </section>
        ```
        

---

### 🏗️ Checkpoint Level 1

text

```
✅ Checklist sebelum lanjut ke Level 2:

PROYEK: Landing Page Tahap 1
File: index.html

SECTION YANG HARUS ADA:
├── <header>: Navbar sederhana (text logo + nav links)
├── Hero section: Grid 2 kolom (teks + gambar), responsif
├── Features section: Grid 3 kolom, responsif (col-12 col-md-4)
└── Footer: sederhana dengan text dan link

TEKNIS:
├── Container, row, col digunakan dengan benar
├── Breakpoints: mobile (1 kolom) → desktop (2-3 kolom)
├── Gutter yang konsisten (g-4 di semua row)
├── Typography: display heading di hero, lead paragraph
├── Spacing: py-5 untuk section, mb-4 untuk heading
└── Tidak ada custom CSS yang bisa diganti dengan Bootstrap utility

VISUAL CHECK:
├── Resize dari 320px hingga 1400px — layout berubah dengan benar
├── Tidak ada horizontal scroll di mobile
└── Teks terbaca dengan jelas di semua ukuran

Commit: feat: create landing page with Bootstrap grid and typography
```

---

## 🔵 LEVEL 2: KOMPONEN DASAR (Minggu 4-7)

> **Tema**: _"Melengkapi landing page dengan komponen Bootstrap yang siap pakai"_  
> **Benang Merah**: Grid dan typography ada (Level 1) → tambahkan komponen interaktif → form → navigasi navbar → halaman jadi fungsional  
> **Output**: Landing page lengkap dengan navbar, hero button, feature cards, form, dan footer

---

### D. Button & Badge — Elemen Aksi

> 💡 **Benang Merah ke Warna**: Sistem warna semantic (Poin 13) yang sama digunakan untuk button — `btn-primary`, `btn-danger`, `btn-success`. Konsistensi ini membuat UI intuitif.

15. `[[15. Button — Semua Varian dan State]]`
    
    - _Langkah konkret_: Buat button showcase:
        
        HTML
        
        ```
        <!-- Solid buttons -->
        <button type="button" class="btn btn-primary">Primary</button>
        <button type="button" class="btn btn-secondary">Secondary</button>
        <button type="button" class="btn btn-success">Berhasil</button>
        <button type="button" class="btn btn-danger">Hapus</button>
        
        <!-- Outline buttons -->
        <button type="button" class="btn btn-outline-primary">Outline Primary</button>
        <button type="button" class="btn btn-outline-danger">Outline Danger</button>
        
        <!-- Ukuran -->
        <button type="button" class="btn btn-primary btn-lg">Besar</button>
        <button type="button" class="btn btn-primary">Normal</button>
        <button type="button" class="btn btn-primary btn-sm">Kecil</button>
        
        <!-- Full width (d-grid) -->
        <div class="d-grid gap-2">
          <button type="button" class="btn btn-primary">Full Width Button</button>
        </div>
        
        <!-- State -->
        <button type="button" class="btn btn-primary" disabled>Disabled</button>
        
        <!-- Button sebagai link -->
        <a href="#" class="btn btn-primary">Link Button</a>
        ```
        
    - _Langkah konkret_: Tambahkan dua CTA button di hero section (`btn-primary` dan `btn-outline-secondary`)
16. `[[16. Badge — Label & Counter]]`
    
    - _Langkah konkret_:
        
        HTML
        
        ```
        <!-- Badge dasar -->
        <span class="badge bg-primary">New</span>
        <span class="badge bg-danger">Penting</span>
        <span class="badge bg-success">Tersedia</span>
        
        <!-- Badge di dalam heading -->
        <h3>Notifikasi <span class="badge bg-danger">3</span></h3>
        
        <!-- Badge pill (rounded) -->
        <span class="badge rounded-pill bg-warning text-dark">Sale</span>
        
        <!-- Badge posisi pada tombol (notification bubble) -->
        <button type="button" class="btn btn-primary position-relative">
          Keranjang
          <span class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger">
            5
            <span class="visually-hidden">item di keranjang</span>
          </span>
        </button>
        ```
        

---

### E. Card — Komponen Container Paling Fleksibel

> 💡 **Benang Merah ke Grid**: Card biasanya diletakkan di dalam grid column. Gabungan `row g-4` + `col-md-4` + `.card.h-100` adalah pattern paling umum untuk card grid.

17. `[[17. Card — Struktur dan Varian]]`
    
    - _Langkah konkret_: Refactor features section menggunakan card yang proper:
        
        HTML
        
        ```
        <div class="row g-4">
          <div class="col-12 col-md-6 col-lg-4">
            <div class="card h-100 border-0 shadow-sm">
              <!-- Header: ikon atau gambar -->
              <div class="card-body p-4">
                <!-- Ikon (gunakan Bootstrap Icons atau emoji) -->
                <div class="feature-icon bg-primary bg-gradient text-white rounded-3 p-3 mb-3 d-inline-flex">
                  <svg><!-- ikon --></svg>
                </div>
                <h5 class="card-title fw-bold">Judul Fitur</h5>
                <p class="card-text text-muted">
                  Deskripsi fitur yang menjelaskan manfaat bagi pengguna.
                </p>
              </div>
              <div class="card-footer bg-transparent border-0 p-4 pt-0">
                <a href="#" class="btn btn-outline-primary btn-sm">Pelajari Lebih →</a>
              </div>
            </div>
          </div>
          <!-- Repeat untuk card lainnya -->
        </div>
        ```
        
18. `[[18. Card dengan Gambar — img-top, img-bottom & Overlay]]`
    
    - _Langkah konkret_: Buat project showcase cards:
        
        HTML
        
        ```
        <!-- Card dengan gambar di atas -->
        <div class="card h-100">
          <img
            src="project.jpg"
            class="card-img-top"
            alt="Screenshot project"
            style="height: 200px; object-fit: cover;"
          >
          <div class="card-body">
            <div class="d-flex justify-content-between align-items-center mb-2">
              <h5 class="card-title mb-0">Nama Project</h5>
              <span class="badge bg-primary">Web App</span>
            </div>
            <p class="card-text text-muted small">Deskripsi singkat project...</p>
            <div class="d-flex gap-2">
              <span class="badge bg-light text-dark">HTML</span>
              <span class="badge bg-light text-dark">CSS</span>
              <span class="badge bg-light text-dark">JS</span>
            </div>
          </div>
          <div class="card-footer bg-transparent d-flex justify-content-between">
            <a href="#" class="btn btn-sm btn-outline-secondary">Demo</a>
            <a href="#" class="btn btn-sm btn-dark">GitHub</a>
          </div>
        </div>
        ```
        

---

### F. Navbar — Navigasi yang Responsif

> 💡 **Benang Merah ke Grid & Breakpoints**: Navbar menggunakan breakpoints yang sama dengan grid — `navbar-expand-lg` berarti hamburger di bawah `lg`, full navbar di atas `lg`.

19. `[[19. Navbar — Struktur & Collapse untuk Mobile]]`
    
    - _Langkah konkret_: Ganti navbar sederhana di Level 1 dengan navbar yang proper:
        
        HTML
        
        ```
        <nav class="navbar navbar-expand-lg bg-white shadow-sm sticky-top">
          <div class="container">
            <!-- Brand/Logo -->
            <a class="navbar-brand fw-bold text-primary" href="#">
              <!-- Bootstrap Icon logo -->
              <svg width="32" height="32"><!-- ... --></svg>
              MyBrand
            </a>
            
            <!-- Hamburger button (tampil di mobile) -->
            <button
              class="navbar-toggler border-0"
              type="button"
              data-bs-toggle="collapse"
              data-bs-target="#navbarMain"
              aria-controls="navbarMain"
              aria-expanded="false"
              aria-label="Toggle navigation"
            >
              <span class="navbar-toggler-icon"></span>
            </button>
            
            <!-- Nav links (collapse di mobile) -->
            <div class="collapse navbar-collapse" id="navbarMain">
              <!-- Links: ms-auto untuk dorong ke kanan -->
              <ul class="navbar-nav ms-auto mb-2 mb-lg-0 align-items-lg-center gap-lg-1">
                <li class="nav-item">
                  <a class="nav-link active" aria-current="page" href="#">Beranda</a>
                </li>
                <li class="nav-item">
                  <a class="nav-link" href="#">Fitur</a>
                </li>
                <li class="nav-item">
                  <a class="nav-link" href="#">Harga</a>
                </li>
                <li class="nav-item">
                  <a class="nav-link" href="#">Kontak</a>
                </li>
              </ul>
              <!-- CTA button -->
              <div class="ms-lg-3 mt-2 mt-lg-0">
                <a href="#" class="btn btn-primary px-4">Mulai Gratis</a>
              </div>
            </div>
          </div>
        </nav>
        ```
        
    - _Langkah konkret_: Test hamburger menu di viewport < 992px
20. `[[20. Navbar Dropdown & Active State]]`
    
    - Tambahkan dropdown ke navbar:
        
        HTML
        
        ```
        <li class="nav-item dropdown">
          <a
            class="nav-link dropdown-toggle"
            href="#"
            role="button"
            data-bs-toggle="dropdown"
            aria-expanded="false"
          >
            Layanan
          </a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Web Design</a></li>
            <li><a class="dropdown-item" href="#">Development</a></li>
            <li><hr class="dropdown-divider"></li>
            <li><a class="dropdown-item" href="#">Konsultasi</a></li>
          </ul>
        </li>
        ```
        
    - _Langkah konkret_: Tambahkan dropdown dan tandai nav link aktif dengan class `active`

---

### G. Alert & Form Dasar

21. `[[21. Alert — Pesan Feedback untuk User]]`
    
    - _Langkah konkret_:
        
        HTML
        
        ```
        <!-- Alert dengan ikon -->
        <div class="alert alert-success d-flex align-items-center" role="alert">
          <svg class="bi flex-shrink-0 me-2" width="24" height="24" role="img" aria-label="Success:">
            <!-- checkmark icon -->
          </svg>
          <div>Pesan berhasil dikirim! Kami akan menghubungi Anda segera.</div>
        </div>
        
        <!-- Dismissible alert -->
        <div class="alert alert-warning alert-dismissible fade show" role="alert">
          <strong>Perhatian!</strong> Sesi Anda akan berakhir dalam 5 menit.
          <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
        </div>
        
        <!-- Alert dengan link -->
        <div class="alert alert-info" role="alert">
          Info! Lihat <a href="#" class="alert-link">dokumentasi</a> untuk detail.
        </div>
        ```
        
22. `[[22. Form Dasar — Input, Label & Helper Text]]`
    
    - _Langkah konkret_: Buat contact form untuk landing page:
        
        HTML
        
        ```
        <form novalidate>
          <div class="row g-3">
            <!-- Nama Lengkap -->
            <div class="col-12 col-md-6">
              <label for="nama" class="form-label fw-medium">Nama Lengkap</label>
              <input
                type="text"
                class="form-control"
                id="nama"
                name="nama"
                placeholder="Masukkan nama lengkap"
                required
              >
            </div>
            
            <!-- Email -->
            <div class="col-12 col-md-6">
              <label for="email" class="form-label fw-medium">Alamat Email</label>
              <input
                type="email"
                class="form-control"
                id="email"
                name="email"
                placeholder="nama@email.com"
                required
              >
            </div>
            
            <!-- Subjek -->
            <div class="col-12">
              <label for="layanan" class="form-label fw-medium">Layanan yang Diminati</label>
              <select class="form-select" id="layanan" name="layanan" required>
                <option value="">Pilih layanan...</option>
                <option value="web">Web Development</option>
                <option value="design">UI/UX Design</option>
                <option value="konsultasi">Konsultasi</option>
              </select>
            </div>
            
            <!-- Pesan -->
            <div class="col-12">
              <label for="pesan" class="form-label fw-medium">Pesan</label>
              <textarea
                class="form-control"
                id="pesan"
                name="pesan"
                rows="4"
                placeholder="Ceritakan project atau kebutuhan Anda..."
                required
              ></textarea>
              <div class="form-text">Minimal 50 karakter.</div>
            </div>
            
            <!-- Checkbox persetujuan -->
            <div class="col-12">
              <div class="form-check">
                <input class="form-check-input" type="checkbox" id="setuju" required>
                <label class="form-check-label" for="setuju">
                  Saya setuju dengan <a href="#">kebijakan privasi</a>
                </label>
              </div>
            </div>
            
            <!-- Submit -->
            <div class="col-12">
              <button type="submit" class="btn btn-primary px-5">
                Kirim Pesan
              </button>
            </div>
          </div>
        </form>
        ```
        

---

### 🏗️ Checkpoint Level 2

text

```
✅ Checklist sebelum lanjut ke Level 3:

PROYEK: Landing Page Lengkap

SECTIONS:
├── Navbar: logo, nav links, dropdown, hamburger mobile, CTA button
├── Hero: h1 display, lead text, 2 CTA buttons, ilustrasi
├── Features: 3 cards dengan icon, title, text, link
├── Projects: 3 cards dengan gambar, badges teknologi, CTA
├── Contact: form lengkap dengan semua field yang benar
└── Footer: grid layout dengan links dan copyright

KOMPONEN:
├── btn: primary, outline, ukuran berbeda, full width
├── badge: di card title, di button (counter)
├── card: dengan gambar, tanpa gambar, card-header, card-footer
├── navbar: collapse, dropdown, active state, sticky-top
└── alert: success, dismissible

CHECKLIST:
├── Semua heading punya hierarki yang benar (h1 → h2 → h3)
├── Semua gambar punya alt text
├── Semua form input punya label yang terhubung
├── Semua button punya tipe yang benar (type="submit" atau type="button")
└── Tidak ada class duplikat yang tidak perlu

Commit: feat: add navbar, cards, buttons, badges, and contact form
```

---

## 🟡 LEVEL 3: KOMPONEN INTERAKTIF (Minggu 7-10)

> **Tema**: _"Menambahkan interaktivitas dengan komponen JavaScript Bootstrap"_  
> **Benang Merah**: Landing page statis (Level 2) → Modal untuk CTA → Accordion untuk FAQ → Table untuk data → Komponen yang butuh JS  
> **Output**: Landing page + halaman FAQ dengan accordion + halaman pricing dengan table

---

### H. Komponen yang Butuh JavaScript

> 💡 **Cara kerja JS Bootstrap**: Komponen seperti modal dan accordion diaktifkan via atribut `data-bs-*` di HTML, atau via JavaScript API (`new bootstrap.Modal(element)`). Keduanya perlu `bootstrap.bundle.min.js`.

23. `[[23. Modal — Dialog yang Muncul di Atas Halaman]]`
    
    - _Langkah konkret_: Tambahkan modal untuk detail project atau demo video:
        
        HTML
        
        ```
        <!-- Trigger Button -->
        <button
          type="button"
          class="btn btn-primary"
          data-bs-toggle="modal"
          data-bs-target="#projectModal"
        >
          Lihat Detail Project
        </button>
        
        <!-- Modal -->
        <div
          class="modal fade"
          id="projectModal"
          tabindex="-1"
          aria-labelledby="projectModalLabel"
          aria-hidden="true"
        >
          <div class="modal-dialog modal-lg modal-dialog-centered modal-dialog-scrollable">
            <div class="modal-content">
              <div class="modal-header border-0 pb-0">
                <h5 class="modal-title fw-bold" id="projectModalLabel">
                  Detail Project: Nama Project
                </h5>
                <button
                  type="button"
                  class="btn-close"
                  data-bs-dismiss="modal"
                  aria-label="Tutup"
                ></button>
              </div>
              <div class="modal-body">
                <img src="project-full.jpg" class="img-fluid rounded mb-3" alt="Screenshot project">
                <h6 class="fw-bold">Tentang Project</h6>
                <p>Deskripsi lengkap project...</p>
                <h6 class="fw-bold">Teknologi</h6>
                <div class="d-flex flex-wrap gap-2">
                  <span class="badge bg-primary">HTML5</span>
                  <span class="badge bg-secondary">CSS3</span>
                  <span class="badge bg-warning text-dark">JavaScript</span>
                </div>
              </div>
              <div class="modal-footer border-0 pt-0">
                <a href="#" class="btn btn-outline-secondary" data-bs-dismiss="modal">Tutup</a>
                <a href="https://github.com" class="btn btn-dark" target="_blank">
                  Lihat di GitHub
                </a>
                <a href="#" class="btn btn-primary">Live Demo</a>
              </div>
            </div>
          </div>
        </div>
        ```
        
    - _Langkah konkret_: Setiap project card punya tombol yang membuka modal berbeda
24. `[[24. Accordion — FAQ yang Bisa Dibuka-Tutup]]`
    
    - _Langkah konkret_: Buat halaman FAQ baru (`faq.html`):
        
        HTML
        
        ```
        <div class="accordion" id="faqAccordion">
          <!-- Item 1: terbuka secara default -->
          <div class="accordion-item border-0 mb-3 rounded-3 shadow-sm overflow-hidden">
            <h2 class="accordion-header">
              <button
                class="accordion-button fw-semibold"
                type="button"
                data-bs-toggle="collapse"
                data-bs-target="#faq1"
                aria-expanded="true"
                aria-controls="faq1"
              >
                Apa yang membuat layanan kami berbeda?
              </button>
            </h2>
            <div
              id="faq1"
              class="accordion-collapse collapse show"
              data-bs-parent="#faqAccordion"
            >
              <div class="accordion-body text-muted">
                Kami fokus pada kualitas kode yang bersih, performa optimal, dan desain yang
                benar-benar memperhatikan pengalaman pengguna...
              </div>
            </div>
          </div>
          
          <!-- Item 2: tertutup -->
          <div class="accordion-item border-0 mb-3 rounded-3 shadow-sm overflow-hidden">
            <h2 class="accordion-header">
              <button
                class="accordion-button collapsed fw-semibold"
                type="button"
                data-bs-toggle="collapse"
                data-bs-target="#faq2"
                aria-expanded="false"
                aria-controls="faq2"
              >
                Berapa lama proses pengerjaan project?
              </button>
            </h2>
            <div
              id="faq2"
              class="accordion-collapse collapse"
              data-bs-parent="#faqAccordion"
            >
              <div class="accordion-body text-muted">
                Durasi pengerjaan tergantung kompleksitas project. Landing page
                membutuhkan 3-5 hari, web app lengkap 2-4 minggu...
              </div>
            </div>
          </div>
          
          <!-- Tambah lebih banyak item -->
        </div>
        ```
        
    - `data-bs-parent="#faqAccordion"`: hanya satu accordion yang terbuka pada satu waktu
25. `[[25. Tab — Konten yang Diorganisir dalam Tab]]`
    
    - _Langkah konkret_: Buat section "Portfolio" dengan tab per kategori:
        
        HTML
        
        ```
        <!-- Nav Tabs -->
        <ul class="nav nav-tabs nav-fill mb-4" id="portfolioTab" role="tablist">
          <li class="nav-item" role="presentation">
            <button
              class="nav-link active fw-medium"
              id="semua-tab"
              data-bs-toggle="tab"
              data-bs-target="#semua"
              type="button"
              role="tab"
              aria-controls="semua"
              aria-selected="true"
            >
              Semua
            </button>
          </li>
          <li class="nav-item" role="presentation">
            <button
              class="nav-link fw-medium"
              id="web-tab"
              data-bs-toggle="tab"
              data-bs-target="#web"
              type="button"
              role="tab"
              aria-controls="web"
              aria-selected="false"
            >
              Web App
            </button>
          </li>
          <li class="nav-item" role="presentation">
            <button class="nav-link fw-medium" /* ... */ >
              Mobile
            </button>
          </li>
        </ul>
        
        <!-- Tab Content -->
        <div class="tab-content" id="portfolioTabContent">
          <div class="tab-pane fade show active" id="semua" role="tabpanel" aria-labelledby="semua-tab">
            <!-- Grid cards semua project -->
            <div class="row g-4"><!-- cards --></div>
          </div>
          <div class="tab-pane fade" id="web" role="tabpanel" aria-labelledby="web-tab">
            <!-- Grid cards web project saja -->
            <div class="row g-4"><!-- cards --></div>
          </div>
        </div>
        ```
        
26. `[[26. Toast — Notifikasi Sementara]]`
    
    - _Langkah konkret_: Toast konfirmasi setelah form berhasil dikirim:
        
        HTML
        
        ```
        <!-- Toast container (posisi fixed di pojok) -->
        <div class="toast-container position-fixed bottom-0 end-0 p-3">
          <div
            id="successToast"
            class="toast align-items-center text-bg-success border-0"
            role="alert"
            aria-live="assertive"
            aria-atomic="true"
          >
            <div class="d-flex">
              <div class="toast-body fw-medium">
                ✅ Pesan berhasil dikirim! Kami akan segera menghubungi Anda.
              </div>
              <button
                type="button"
                class="btn-close btn-close-white me-2 m-auto"
                data-bs-dismiss="toast"
                aria-label="Close"
              ></button>
            </div>
          </div>
        </div>
        
        <!-- JavaScript untuk menampilkan toast setelah form submit -->
        <script>
          document.querySelector('form').addEventListener('submit', function(e) {
            e.preventDefault();
            const toast = new bootstrap.Toast(document.getElementById('successToast'));
            toast.show();
            this.reset();
          });
        </script>
        ```
        

---

### I. Tabel & Konten Lainnya

27. `[[27. Tabel Bootstrap — Data yang Terorganisir]]`
    
    - _Langkah konkret_: Buat pricing comparison table:
        
        HTML
        
        ```
        <div class="table-responsive">
          <table class="table table-bordered table-hover align-middle">
            <thead class="table-dark">
              <tr>
                <th scope="col">Fitur</th>
                <th scope="col" class="text-center">Starter</th>
                <th scope="col" class="text-center bg-primary">Pro</th>
                <th scope="col" class="text-center">Enterprise</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>Halaman web</td>
                <td class="text-center">5</td>
                <td class="text-center bg-primary bg-opacity-10 fw-bold">Unlimited</td>
                <td class="text-center">Unlimited</td>
              </tr>
              <tr>
                <td>Custom domain</td>
                <td class="text-center text-danger">✗</td>
                <td class="text-center bg-primary bg-opacity-10 text-success fw-bold">✓</td>
                <td class="text-center text-success">✓</td>
              </tr>
              <tr class="table-success">
                <th scope="row">Harga/bulan</th>
                <td class="text-center fw-bold">Gratis</td>
                <td class="text-center bg-primary text-white fw-bold fs-5">Rp 99K</td>
                <td class="text-center fw-bold">Rp 299K</td>
              </tr>
            </tbody>
          </table>
        </div>
        ```
        
28. `[[28. Progress Bar & Spinner]]`
    
    - _Langkah konkret_: Tambahkan skill bars di bagian About:
        
        HTML
        
        ```
        <!-- Skill bars -->
        <div class="mb-3">
          <div class="d-flex justify-content-between mb-1">
            <span class="fw-medium">HTML & CSS</span>
            <span class="text-muted">95%</span>
          </div>
          <div class="progress" style="height: 8px;" role="progressbar" aria-valuenow="95" aria-valuemin="0" aria-valuemax="100">
            <div class="progress-bar bg-primary rounded-pill" style="width: 95%"></div>
          </div>
        </div>
        
        <div class="mb-3">
          <div class="d-flex justify-content-between mb-1">
            <span class="fw-medium">JavaScript</span>
            <span class="text-muted">80%</span>
          </div>
          <div class="progress" style="height: 8px;" role="progressbar" aria-valuenow="80" aria-valuemin="0" aria-valuemax="100">
            <div class="progress-bar bg-success rounded-pill" style="width: 80%"></div>
          </div>
        </div>
        
        <!-- Loading spinner untuk tombol -->
        <button class="btn btn-primary" type="button" disabled id="loadBtn">
          <span class="spinner-border spinner-border-sm me-2" role="status" aria-hidden="true"></span>
          Memproses...
        </button>
        ```
        

---

### 🏗️ Checkpoint Level 3

text

```
✅ Checklist sebelum lanjut ke Level 4:

PROYEK: Landing Page + FAQ + Pricing
├── Landing page: semua dari Level 1 & 2
├── FAQ page: accordion dengan min. 6 pertanyaan
├── Pricing page: tabel perbandingan paket

KOMPONEN BARU:
├── Modal: terbuka dari tombol, berisi konten dinamis
├── Accordion: FAQ yang bisa dibuka-tutup
├── Tab: portfolio diorganisir per kategori
├── Toast: konfirmasi form submit
├── Table: pricing comparison dengan color highlights
└── Progress bar: skill atau statistik

AKSESIBILITAS DASAR:
├── Semua modal punya aria-labelledby
├── Semua accordion punya aria-expanded dan aria-controls
├── Semua tab punya role="tab" dan aria-selected
├── Progress bar punya aria-valuenow, aria-valuemin, aria-valuemax
└── Toast punya aria-live

Commit: feat: add modal, accordion, tabs, toast, and data table
```

---

## 🟠 LEVEL 4: KUSTOMISASI DENGAN SASS (Minggu 10-14)

> **Tema**: _"Dari Bootstrap generic ke Bootstrap yang punya identitas brand sendiri"_  
> **Benang Merah**: Landing page sudah jadi (Level 1-3) → tampilan masih "Bootstrap default" → kustomisasi via Sass → brand colors, typography custom → tampilan unik  
> **Output**: Landing page yang sama tapi dengan warna brand, font, dan spacing yang dikustomisasi

---

### J. Setup Sass untuk Bootstrap

> 💡 **Mengapa Sass dan bukan CSS biasa?** Bootstrap ditulis dalam Sass. Kita bisa mengubah variabel Sass **sebelum** Bootstrap di-compile — hasilnya Bootstrap yang benar-benar kita sesuaikan, bukan override CSS di atasnya.

29. `[[29. Setup Sass + Bootstrap — Workflow yang Benar]]`
    
    - _Langkah konkret_: Setup project Vite + Bootstrap Sass:
        
        Bash
        
        ```
        npm create vite@latest portfolio-custom -- --template vanilla
        cd portfolio-custom
        npm install
        npm install bootstrap sass
        ```
        
        JavaScript
        
        ```
        // vite.config.js
        import { defineConfig } from 'vite';
        
        export default defineConfig({
          css: {
            preprocessorOptions: {
              scss: {
                quietDeps: true, // suppress Bootstrap deprecation warnings
              },
            },
          },
        });
        ```
        
        JavaScript
        
        ```
        // main.js
        import './scss/main.scss';
        // JANGAN import bootstrap.min.css — sudah di-handle oleh Sass
        ```
        
        text
        
        ```
        src/scss/
        ├── _variables.scss    ← override Bootstrap variables
        ├── _custom.scss       ← style custom kita
        └── main.scss          ← entry point
        ```
        
30. `[[30. Struktur File Sass Bootstrap — Memahami Arsitekturnya]]`
    
    - _Langkah konkret_: Lihat struktur file Sass Bootstrap di `node_modules/bootstrap/scss/`:
        
        text
        
        ```
        bootstrap/scss/
        ├── _variables.scss    ← SEMUA variabel Bootstrap (kita override ini)
        ├── _mixins.scss       ← mixin yang bisa kita gunakan
        ├── _utilities.scss    ← definisi utility classes
        ├── bootstrap.scss     ← import semua komponen
        └── ... (komponen individual)
        ```
        
    - Cara import selektif di `main.scss`:
        
        SCSS
        
        ```
        // 1. Wajib: functions (harus pertama)
        @import 'bootstrap/scss/functions';
        
        // 2. Override variables SEBELUM import variables Bootstrap
        @import 'variables';
        
        // 3. Import variables, mixins, maps Bootstrap
        @import 'bootstrap/scss/variables';
        @import 'bootstrap/scss/variables-dark';
        @import 'bootstrap/scss/maps';
        @import 'bootstrap/scss/mixins';
        @import 'bootstrap/scss/utilities';
        
        // 4. Import HANYA komponen yang diperlukan
        @import 'bootstrap/scss/root';
        @import 'bootstrap/scss/reboot';
        @import 'bootstrap/scss/type';
        @import 'bootstrap/scss/containers';
        @import 'bootstrap/scss/grid';
        @import 'bootstrap/scss/helpers';
        @import 'bootstrap/scss/utilities/api';
        
        // 5. Custom styles
        @import 'custom';
        ```
        
31. `[[31. Override Bootstrap Variables — Brand Colors]]`
    
    - _Langkah konkret_: File `_variables.scss`:
        
        SCSS
        
        ```
        // =============================================
        // BRAND COLORS
        // =============================================
        
        // Tentukan warna brand utama
        $brand-blue:      #2563EB; // primary brand color
        $brand-blue-dark: #1D4ED8;
        $brand-dark:      #0F172A;
        $brand-gray:      #64748B;
        
        // Override Bootstrap theme colors
        $primary:   $brand-blue;
        $secondary: $brand-gray;
        $dark:      $brand-dark;
        
        // =============================================
        // TYPOGRAPHY
        // =============================================
        
        // Google Fonts sudah di-load di HTML
        $font-family-sans-serif: 'Inter', system-ui, -apple-system, sans-serif;
        $font-family-monospace: 'Fira Code', 'Cascadia Code', monospace;
        
        // Font sizes
        $font-size-base: 1rem; // 16px
        $h1-font-size: clamp(2rem, 5vw, 3.5rem); // fluid
        $h2-font-size: clamp(1.5rem, 3vw, 2.5rem);
        $headings-font-weight: 700;
        $headings-line-height: 1.2;
        
        // =============================================
        // SPACING
        // =============================================
        
        $spacer: 1rem;
        $spacers: (
          0: 0,
          1: $spacer * 0.25,   // 4px
          2: $spacer * 0.5,    // 8px
          3: $spacer,          // 16px
          4: $spacer * 1.5,    // 24px
          5: $spacer * 2,      // 32px
          6: $spacer * 3,      // 48px  ← custom!
          7: $spacer * 4,      // 64px  ← custom!
          8: $spacer * 6,      // 96px  ← custom!
        );
        
        // =============================================
        // BORDER & BORDER RADIUS
        // =============================================
        
        $border-radius:    0.5rem;   // 8px
        $border-radius-lg: 0.75rem;  // 12px
        $border-radius-xl: 1rem;     // 16px (sudah ada di Bootstrap 5)
        $border-radius-xxl: 1.5rem;  // 24px (sudah ada di Bootstrap 5)
        
        // =============================================
        // SHADOWS
        // =============================================
        
        $box-shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.1), 0 1px 2px rgba(0, 0, 0, 0.06);
        $box-shadow:    0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
        $box-shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        
        // =============================================
        // COMPONENTS
        // =============================================
        
        // Navbar
        $navbar-padding-y: 1rem;
        $navbar-nav-link-padding-x: 0.75rem;
        
        // Buttons
        $btn-padding-x: 1.5rem;
        $btn-padding-y: 0.625rem;
        $btn-font-weight: 600;
        $btn-border-radius: $border-radius;
        
        // Cards
        $card-border-width: 0;
        $card-border-radius: $border-radius-lg;
        $card-box-shadow: $box-shadow-sm;
        
        // Forms
        $input-border-radius: $border-radius;
        $input-focus-border-color: $primary;
        $input-focus-box-shadow: 0 0 0 3px rgba($primary, 0.15);
        ```
        
32. `[[32. Custom Utility Classes dengan Sass — Extend Bootstrap]]`
    
    - _Langkah konkret_: File `_custom.scss`:
        
        SCSS
        
        ```
        // =============================================
        // SECTION SPACING
        // =============================================
        
        .section {
          padding-top: map-get($spacers, 8);   // 96px
          padding-bottom: map-get($spacers, 8);
          
          @include media-breakpoint-down(md) {
            padding-top: map-get($spacers, 6);  // 48px di mobile
            padding-bottom: map-get($spacers, 6);
          }
        }
        
        // =============================================
        // HERO SECTION
        // =============================================
        
        .hero {
          min-height: 90vh;
          display: flex;
          align-items: center;
          background: linear-gradient(135deg, #f0f9ff 0%, #e0f2fe 100%);
          
          @include media-breakpoint-up(lg) {
            min-height: 80vh;
          }
        }
        
        .hero-title {
          font-size: $h1-font-size;
          line-height: 1.1;
          
          .highlight {
            color: $primary;
            position: relative;
            
            &::after {
              content: '';
              position: absolute;
              bottom: 0;
              left: 0;
              right: 0;
              height: 3px;
              background: $primary;
              border-radius: 2px;
            }
          }
        }
        
        // =============================================
        // FEATURE ICON
        // =============================================
        
        .feature-icon {
          width: 56px;
          height: 56px;
          display: inline-flex;
          align-items: center;
          justify-content: center;
          border-radius: $border-radius-lg;
          
          &--primary { background: rgba($primary, 0.1); color: $primary; }
          &--success  { background: rgba($success, 0.1); color: $success; }
          &--warning  { background: rgba($warning, 0.1); color: $warning; }
        }
        
        // =============================================
        // CARD HOVER EFFECTS
        // =============================================
        
        .card-hover {
          transition: transform 0.2s ease, box-shadow 0.2s ease;
          
          &:hover {
            transform: translateY(-4px);
            box-shadow: $box-shadow-lg !important;
          }
        }
        ```
        
33. `[[33. Utility API — Tambah Utility Class Custom]]`
    
    - Bootstrap punya API untuk generate utility class secara otomatis:
        
        SCSS
        
        ```
        // Di main.scss, setelah @import 'bootstrap/scss/utilities':
        $utilities: map-merge(
          $utilities,
          (
            // Tambah gap yang lebih besar
            "gap": map-merge(
              map-get($utilities, "gap"),
              (values: map-merge(map-get(map-get($utilities, "gap"), "values"), (6: $spacer * 3, 7: $spacer * 4)))
            ),
            
            // Custom border radius
            "rounded": map-merge(
              map-get($utilities, "rounded"),
              (values: map-merge(map-get(map-get($utilities, "rounded"), "values"), (4: 1.5rem)))
            ),
            
            // Custom cursor
            "cursor": (
              property: cursor,
              class: cursor,
              values: pointer default none
            ),
          )
        );
        ```
        
    - _Langkah konkret_: Generate utility `cursor-pointer` yang belum ada di Bootstrap

---

### 🏗️ Checkpoint Level 4

text

```
✅ Checklist sebelum lanjut ke Level 5:

KUSTOMISASI SASS:
├── _variables.scss: brand colors terdefinisi
├── Primary color berubah dari biru default ke brand color
├── Font family: Inter (Google Fonts) aktif
├── Border radius dikustomisasi (lebih atau kurang rounded)
├── Shadow dikustomisasi
├── Spacer scale diperpanjang (s-6, s-7, s-8)
└── Card dan button dikustomisasi via variables

KOMPONEN CUSTOM:
├── .section: padding konsisten untuk semua section
├── .hero: background gradient, min-height
├── .feature-icon: ikon dengan background colored
├── .card-hover: animasi hover yang smooth
└── Minimal 1 custom utility via Utility API

BUILD:
├── npm run build: sukses tanpa error
├── Output CSS lebih kecil dari import penuh (pilih komponen)
└── Source map aktif untuk debugging

VISUAL:
├── Landing page terlihat berbeda dari Bootstrap default
├── Warna, font, dan spacing konsisten dengan brand
└── Tidak ada tampilan "Bootstrap generic" yang tersisa

Commit: feat: customize Bootstrap with Sass variables and custom utilities
```

---

## 🔴 LEVEL 5: RESPONSIF MENDALAM & AKSESIBILITAS (Minggu 14-18)

> **Tema**: _"Memastikan UI berfungsi sempurna di semua ukuran layar dan untuk semua pengguna"_  
> **Benang Merah**: Landing page bagus di desktop (Level 1-4) → audit mobile → perbaiki → aksesibilitas → performa  
> **Output**: Website yang lulus Lighthouse dengan skor ≥ 90 di semua kategori

---

### K. Mobile-First yang Sesungguhnya

34. `[[34. Mobile-First Audit — Review Semua Komponen di Mobile]]`
    
    - _Langkah konkret_: Buka DevTools → toggle device toolbar → cek setiap section di 375px:
        
        text
        
        ```
        Checklist audit mobile:
        ├── Hero: teks tidak terlalu kecil, CTA buttons tidak terlalu kecil (min 44×44px)
        ├── Navbar: hamburger berfungsi, dropdown tidak overflow
        ├── Cards: 1 kolom di mobile, tidak ada horizontal scroll
        ├── Table: ada horizontal scroll untuk tabel lebar
        ├── Form: input mudah diketuk, keyboard tidak menutup penting
        ├── Modal: tidak lebih besar dari viewport
        └── Footer: tidak cramped, link mudah diklik
        ```
        
35. `[[35. Responsive Spacing — Padding & Margin yang Adaptif]]`
    
    - _Langkah konkret_: Gunakan responsive spacing utilities:
        
        HTML
        
        ```
        <!-- Section spacing: sedikit di mobile, banyak di desktop -->
        <section class="py-5 py-lg-8">
          <div class="container px-3 px-md-4">
            <!-- Heading margin berbeda di mobile dan desktop -->
            <h2 class="mb-3 mb-md-4 mb-lg-5">Heading</h2>
            
            <!-- Row gap berbeda -->
            <div class="row g-3 g-md-4 g-lg-5">
              <!-- ... -->
            </div>
          </div>
        </section>
        
        <!-- Font size responsif -->
        <style>
          .hero-title {
            font-size: clamp(1.75rem, 5vw, 3.5rem);
            line-height: 1.15;
          }
          .lead {
            font-size: clamp(1rem, 2.5vw, 1.25rem);
          }
        </style>
        ```
        
36. `[[36. Display Utilities untuk Tampilan Per Breakpoint]]`
    
    - _Langkah konkret_:
        
        HTML
        
        ```
        <!-- Tampilkan teks lengkap di desktop, singkat di mobile -->
        <button type="button" class="btn btn-primary">
          <span class="d-none d-md-inline">Mulai Gratis Sekarang</span>
          <span class="d-inline d-md-none">Mulai</span>
        </button>
        
        <!-- Sidebar: tampil di desktop, tersembunyi di mobile (ada di drawer) -->
        <div class="d-none d-lg-block">
          <!-- Sidebar content -->
        </div>
        
        <!-- Order di berbagai breakpoint -->
        <div class="row align-items-center">
          <div class="col-12 col-lg-6 order-2 order-lg-1">
            <!-- Teks: di bawah gambar di mobile, di kiri di desktop -->
          </div>
          <div class="col-12 col-lg-6 order-1 order-lg-2">
            <!-- Gambar: di atas di mobile, di kanan di desktop -->
          </div>
        </div>
        ```
        

---

### L. Aksesibilitas dengan Bootstrap

37. `[[37. ARIA & Keyboard Navigation — Komponen Bootstrap yang Accessible]]`
    
    - _Langkah konkret_: Audit aksesibilitas semua komponen interaktif:
        
        HTML
        
        ```
        <!-- Skip link (harus elemen pertama di body) -->
        <a class="visually-hidden-focusable btn btn-primary" href="#main-content">
          Lewati navigasi
        </a>
        
        <nav aria-label="Navigasi utama">
          <!-- ... navbar ... -->
        </nav>
        
        <main id="main-content">
          <!-- ... konten utama ... -->
        </main>
        
        <!-- Modal dengan focus trap yang benar -->
        <div class="modal" id="myModal" aria-labelledby="modalTitle" aria-describedby="modalDesc">
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="modalTitle">Judul Modal</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Tutup dialog"></button>
              </div>
              <div class="modal-body" id="modalDesc">
                <!-- Konten yang mendeskripsikan modal -->
              </div>
            </div>
          </div>
        </div>
        ```
        
38. `[[38. visually-hidden & Kontras Warna — Aksesibilitas Visual]]`
    
    - _Langkah konkret_: Audit kontras warna dan tambahkan teks tersembunyi:
        
        HTML
        
        ```
        <!-- Teks hanya untuk screen reader -->
        <span class="visually-hidden">Ini hanya dibaca screen reader</span>
        
        <!-- Icon button dengan label tersembunyi -->
        <button type="button" class="btn btn-outline-secondary">
          <svg aria-hidden="true" focusable="false"><!-- ikon tutup --></svg>
          <span class="visually-hidden">Tutup notifikasi</span>
        </button>
        
        <!-- Badge dengan context untuk screen reader -->
        <button class="btn btn-primary position-relative">
          Notifikasi
          <span class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger">
            5
            <span class="visually-hidden">notifikasi belum dibaca</span>
          </span>
        </button>
        ```
        
    - Cek kontras: WebAIM Contrast Checker — target AA: 4.5:1 untuk teks normal
    - _Langkah konkret_: Jalankan axe DevTools extension, perbaiki semua issue

---

### M. Performa Bootstrap

39. `[[39. Optimasi CSS — Hanya Load yang Digunakan]]`
    
    - _Langkah konkret_: Import selektif komponen di `main.scss`:
        
        SCSS
        
        ```
        // Wajib (fondasi)
        @import 'bootstrap/scss/functions';
        @import 'variables'; // custom variables kita
        @import 'bootstrap/scss/variables';
        @import 'bootstrap/scss/variables-dark';
        @import 'bootstrap/scss/maps';
        @import 'bootstrap/scss/mixins';
        @import 'bootstrap/scss/utilities';
        
        // Layout
        @import 'bootstrap/scss/root';
        @import 'bootstrap/scss/reboot';
        @import 'bootstrap/scss/containers';
        @import 'bootstrap/scss/grid';
        
        // Typography
        @import 'bootstrap/scss/type';
        @import 'bootstrap/scss/images';
        
        // Components (hanya yang dipakai)
        @import 'bootstrap/scss/buttons';
        @import 'bootstrap/scss/nav';
        @import 'bootstrap/scss/navbar';
        @import 'bootstrap/scss/card';
        @import 'bootstrap/scss/accordion';
        @import 'bootstrap/scss/modal';
        @import 'bootstrap/scss/tables';
        @import 'bootstrap/scss/forms';
        @import 'bootstrap/scss/alert';
        @import 'bootstrap/scss/badge';
        @import 'bootstrap/scss/progress';
        @import 'bootstrap/scss/spinners';
        @import 'bootstrap/scss/toasts';
        
        // Tidak dipakai — comment out:
        // @import 'bootstrap/scss/carousel';
        // @import 'bootstrap/scss/offcanvas';
        // @import 'bootstrap/scss/scrollspy';
        // @import 'bootstrap/scss/tooltip';
        // @import 'bootstrap/scss/popover';
        // @import 'bootstrap/scss/list-group';
        
        // Utilities
        @import 'bootstrap/scss/helpers';
        @import 'bootstrap/scss/utilities/api';
        
        // Custom
        @import 'custom';
        ```
        
    - _Langkah konkret_: Bandingkan ukuran CSS sebelum dan sesudah selektif import
40. `[[40. Lighthouse Audit — Target Skor ≥ 90]]`
    
    - _Langkah konkret_: Jalankan Lighthouse di semua halaman:
        
        text
        
        ```
        Target skor:
        ├── Performance: ≥ 85
        ├── Accessibility: ≥ 95
        ├── Best Practices: ≥ 90
        └── SEO: ≥ 90
        
        Perbaikan umum:
        ├── Tambahkan loading="lazy" ke semua gambar kecuali LCP
        ├── Tambahkan width dan height ke semua img
        ├── Minify CSS dan JS (Vite build otomatis)
        ├── Preload font Google di <head>
        └── Tambahkan meta description di setiap halaman
        ```
        

---

### 🏗️ Checkpoint Level 5

text

```
✅ Checklist sebelum lanjut ke Level 6:

RESPONSIF:
├── Berfungsi sempurna di 320px hingga 1920px
├── Hamburger menu berfungsi di semua browser
├── Table ada horizontal scroll di mobile
├── Form mudah diisi di mobile (input tidak terlalu kecil)
└── Modal tidak overflow di mobile kecil

AKSESIBILITAS:
├── Skip link ada dan berfungsi
├── Semua image punya alt text yang deskriptif
├── Semua form input punya label
├── Semua interactive element bisa diakses dengan keyboard
├── axe DevTools: 0 critical, 0 serious issues
└── Lighthouse Accessibility: ≥ 95

PERFORMA:
├── Hanya komponen Bootstrap yang dipakai yang di-import
├── Gambar pakai loading="lazy" dan punya width/height
├── Google Fonts dengan preconnect di head
└── Lighthouse Performance: ≥ 85

Commit: feat: optimize responsive, accessibility, and performance
```

---

## ⚫ LEVEL 6: DARK MODE & KOMPONEN ADVANCED (Minggu 18-24)

> **Tema**: _"Fitur advanced yang membedakan website biasa dengan website profesional"_  
> **Benang Merah**: Landing page production-ready (Level 5) → dark mode → offcanvas → komponen custom kompleks  
> **Output**: Website dengan dark mode toggle, sidebar offcanvas, dan komponen custom

---

### N. Dark Mode Bootstrap 5.3

> 💡 **Bootstrap 5.3** memperkenalkan built-in dark mode via `data-bs-theme` attribute. Tidak perlu banyak CSS custom.

41. `[[41. Dark Mode Toggle — Implementasi Lengkap]]`
    
    - _Langkah konkret_: Tambahkan dark mode toggle ke navbar:
        
        HTML
        
        ```
        <!-- Toggle button di navbar -->
        <button
          id="darkModeToggle"
          type="button"
          class="btn btn-outline-secondary btn-sm"
          aria-label="Toggle dark mode"
          title="Toggle tema"
        >
          <!-- Ikon Sun (light mode) -->
          <svg class="d-dark-none" width="16" height="16" fill="currentColor" viewBox="0 0 16 16">
            <path d="M8 12a4 4 0 1 0 0-8 4 4 0 0 0 0 8z"/>
            <!-- path matahari -->
          </svg>
          <!-- Ikon Moon (dark mode) -->
          <svg class="d-light-none" width="16" height="16" fill="currentColor" viewBox="0 0 16 16">
            <!-- path bulan -->
          </svg>
        </button>
        
        <script>
          const toggle = document.getElementById('darkModeToggle');
          const html = document.documentElement;
          
          // Load dari localStorage
          const savedTheme = localStorage.getItem('theme') ||
            (window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light');
          html.setAttribute('data-bs-theme', savedTheme);
          
          toggle.addEventListener('click', () => {
            const current = html.getAttribute('data-bs-theme');
            const newTheme = current === 'dark' ? 'light' : 'dark';
            html.setAttribute('data-bs-theme', newTheme);
            localStorage.setItem('theme', newTheme);
          });
        </script>
        ```
        
    - `d-dark-none`: Bootstrap utility — tersembunyi saat dark mode aktif
    - `d-light-none`: tersembunyi saat light mode aktif
    - _Langkah konkret_: Test toggle — semua komponen Bootstrap otomatis berubah
42. `[[42. Kustomisasi Dark Mode dengan Sass]]`
    
    - _Langkah konkret_: Override dark mode colors di `_variables.scss`:
        
        SCSS
        
        ```
        // Bootstrap 5.3 dark mode CSS variables override
        [data-bs-theme="dark"] {
          // Background
          --bs-body-bg: #0f172a;
          --bs-body-bg-rgb: 15, 23, 42;
          
          // Surface colors
          --bs-secondary-bg: #1e293b;
          --bs-tertiary-bg: #334155;
          
          // Text
          --bs-body-color: #e2e8f0;
          --bs-secondary-color: #94a3b8;
          
          // Border
          --bs-border-color: #334155;
          --bs-border-color-translucent: rgba(255, 255, 255, 0.1);
          
          // Custom component overrides
          .hero {
            background: linear-gradient(135deg, #0f172a 0%, #1e3a5f 100%);
          }
          
          .card {
            background-color: var(--bs-secondary-bg);
            border-color: var(--bs-border-color);
          }
        }
        ```
        

---

### O. Offcanvas & Komponen Advanced

43. `[[43. Offcanvas — Sidebar yang Muncul dari Samping]]`
    
    - _Langkah konkret_: Buat mobile sidebar untuk halaman dashboard (preview Level 7):
        
        HTML
        
        ```
        <!-- Tombol trigger offcanvas -->
        <button
          class="btn btn-outline-secondary"
          type="button"
          data-bs-toggle="offcanvas"
          data-bs-target="#sidebarMenu"
          aria-controls="sidebarMenu"
        >
          <span class="navbar-toggler-icon"></span>
          Menu
        </button>
        
        <!-- Offcanvas Sidebar -->
        <div
          class="offcanvas offcanvas-start"
          tabindex="-1"
          id="sidebarMenu"
          aria-labelledby="sidebarMenuLabel"
          style="width: 280px;"
        >
          <div class="offcanvas-header border-bottom">
            <h5 class="offcanvas-title fw-bold" id="sidebarMenuLabel">Admin Panel</h5>
            <button type="button" class="btn-close" data-bs-dismiss="offcanvas" aria-label="Tutup"></button>
          </div>
          <div class="offcanvas-body p-0">
            <nav class="nav flex-column">
              <a class="nav-link px-4 py-3 active" href="#">
                <svg class="me-2"><!-- dashboard icon --></svg>
                Dashboard
              </a>
              <a class="nav-link px-4 py-3" href="#">
                <svg class="me-2"><!-- users icon --></svg>
                Pengguna
              </a>
              <!-- ... lebih banyak link -->
            </nav>
          </div>
          <div class="offcanvas-footer p-3 border-top">
            <div class="d-flex align-items-center gap-2">
              <img src="avatar.jpg" class="rounded-circle" width="36" height="36" alt="Avatar">
              <div>
                <div class="fw-medium small">Admin User</div>
                <div class="text-muted" style="font-size: 0.75rem;">admin@email.com</div>
              </div>
            </div>
          </div>
        </div>
        ```
        
44. `[[44. Bootstrap JavaScript API — Kontrol Programatik]]`
    
    - _Langkah konkret_: Kontrol komponen Bootstrap via JavaScript:
        
        JavaScript
        
        ```
        // Modal via JavaScript
        const modal = new bootstrap.Modal('#confirmModal', {
          backdrop: 'static', // tidak tutup saat klik backdrop
          keyboard: false,    // tidak tutup saat tekan Escape
        });
        
        // Buka/tutup programatik
        modal.show();
        modal.hide();
        modal.toggle();
        
        // Listen events Bootstrap
        const modalEl = document.getElementById('confirmModal');
        
        modalEl.addEventListener('show.bs.modal', function(event) {
          // event.relatedTarget = tombol yang memicu modal
          const button = event.relatedTarget;
          const productId = button.getAttribute('data-product-id');
          const productName = button.getAttribute('data-product-name');
          
          // Update modal content berdasarkan data yang dikirim
          this.querySelector('#modalProductName').textContent = productName;
          this.querySelector('#confirmDeleteBtn').setAttribute('data-id', productId);
        });
        
        modalEl.addEventListener('hidden.bs.modal', function() {
          // Reset form di dalam modal
          this.querySelector('form')?.reset();
        });
        
        // Toast dengan auto-dismiss
        function showToast(message, type = 'success') {
          const toastEl = document.getElementById('globalToast');
          toastEl.querySelector('.toast-body').textContent = message;
          toastEl.className = `toast align-items-center text-bg-${type} border-0`;
          
          const toast = bootstrap.Toast.getOrCreateInstance(toastEl, { delay: 3000 });
          toast.show();
        }
        
        // Scroll spy
        const scrollspy = new bootstrap.ScrollSpy(document.body, {
          target: '#tableOfContents',
          offset: 80,
        });
        ```
        

---

### 🏗️ Checkpoint Level 6

text

```
✅ Checklist sebelum lanjut ke Level 7:

DARK MODE:
├── Toggle dark/light mode dengan satu klik
├── Preferensi tersimpan di localStorage
├── Deteksi otomatis preferensi OS
├── Semua custom component kompatibel dark mode
└── Transisi smooth (0.2s pada semua color changes)

KOMPONEN ADVANCED:
├── Offcanvas sidebar dengan konten yang benar
├── Bootstrap JS API: minimal 3 komponen dikontrol via JS
├── Event listener Bootstrap: show.bs.modal, hidden.bs.modal
└── Dynamic toast notification via JavaScript function

KUALITAS KODE:
├── Tidak ada inline style yang bisa diganti utility
├── Class Bootstrap tidak duplikat atau konflik
├── JavaScript Bootstrap tidak duplikat inisialisasi
└── Semua komponen bersih di console (tidak ada error/warning)

Commit: feat: add dark mode, offcanvas sidebar, and Bootstrap JS API
```

---

## 🟣 LEVEL 7: MASTERY — DASHBOARD ADMIN LENGKAP (Minggu 24+)

> **Tema**: _"Membangun aplikasi admin dashboard yang production-ready"_  
> **Benang Merah**: Semua komponen dan teknik dari Level 1-6 → diintegrasikan dalam satu project kompleks  
> **Output**: Admin dashboard lengkap dengan sidebar, statistik, tabel data, charts, dan semua fitur

---

### P. Admin Dashboard — Arsitektur Layout

45. `[[45. Layout Admin Dashboard — Sidebar + Main Content]]`
    
    - _Langkah konkret_: Struktur HTML dashboard:
        
        HTML
        
        ```
        <!DOCTYPE html>
        <html lang="id" data-bs-theme="light">
        <head>
          <!-- ... meta tags ... -->
          <title>Admin Dashboard</title>
          <link rel="stylesheet" href="scss/main.scss">
          <style>
            /* Sidebar fixed */
            .sidebar {
              width: 260px;
              min-height: 100vh;
              position: fixed;
              top: 0;
              left: 0;
              background-color: var(--bs-dark);
              color: white;
              z-index: 100;
              overflow-y: auto;
            }
            
            /* Main content: offset dari sidebar */
            .main-content {
              margin-left: 260px;
              min-height: 100vh;
            }
            
            /* Mobile: sidebar tersembunyi, gunakan offcanvas */
            @media (max-width: 991px) {
              .sidebar { display: none; }
              .main-content { margin-left: 0; }
            }
          </style>
        </head>
        <body>
          <!-- Mobile: Navbar dengan hamburger untuk offcanvas -->
          <nav class="navbar navbar-expand-lg d-lg-none bg-body-tertiary border-bottom">
            <div class="container-fluid">
              <a class="navbar-brand fw-bold" href="#">Admin</a>
              <button class="btn btn-outline-secondary" data-bs-toggle="offcanvas" data-bs-target="#mobileSidebar">
                ☰
              </button>
            </div>
          </nav>
          
          <!-- Sidebar (desktop: fixed, mobile: offcanvas) -->
          <aside class="sidebar d-none d-lg-flex flex-column p-3">
            <!-- sidebar content -->
          </aside>
          
          <!-- Mobile Offcanvas Sidebar -->
          <div class="offcanvas offcanvas-start" id="mobileSidebar">
            <!-- sama dengan sidebar desktop -->
          </div>
          
          <!-- Main Content -->
          <main class="main-content">
            <!-- Topbar -->
            <nav class="navbar bg-body border-bottom px-4 py-3 sticky-top">
              <!-- breadcrumb, search, user menu, dark mode toggle -->
            </nav>
            
            <!-- Page Content -->
            <div class="container-fluid p-4">
              <div id="pageContent">
                <!-- content berubah sesuai menu -->
              </div>
            </div>
          </main>
          
          <script src="main.js" type="module"></script>
        </body>
        </html>
        ```
        
46. `[[46. Sidebar Navigation — Collapsible Menu]]`
    
    - _Langkah konkret_: Sidebar dengan nested menu:
        
        HTML
        
        ```
        <aside class="sidebar p-0 d-none d-lg-flex flex-column">
          <!-- Logo -->
          <div class="p-4 border-bottom border-secondary-subtle">
            <a href="#" class="text-white text-decoration-none d-flex align-items-center gap-2">
              <svg width="32" height="32"><!-- logo icon --></svg>
              <span class="fw-bold fs-5">AdminPro</span>
            </a>
          </div>
          
          <!-- Navigation -->
          <nav class="flex-grow-1 p-3">
            <!-- Section label -->
            <div class="text-uppercase text-secondary small mb-2 px-2 fw-semibold" style="font-size: 0.7rem; letter-spacing: 0.08em;">
              Menu Utama
            </div>
            
            <!-- Nav items -->
            <ul class="nav nav-pills flex-column gap-1 mb-4">
              <li class="nav-item">
                <a href="#" class="nav-link text-white active px-3 py-2 d-flex align-items-center gap-2">
                  <svg width="18" height="18"><!-- dashboard icon --></svg>
                  Dashboard
                </a>
              </li>
              
              <!-- Item dengan submenu (collapse) -->
              <li class="nav-item">
                <button
                  class="nav-link text-white w-100 text-start d-flex align-items-center justify-content-between px-3 py-2"
                  data-bs-toggle="collapse"
                  data-bs-target="#productSubmenu"
                  aria-expanded="false"
                >
                  <span class="d-flex align-items-center gap-2">
                    <svg width="18" height="18"><!-- product icon --></svg>
                    Produk
                  </span>
                  <svg class="ms-auto" width="12" height="12"><!-- chevron --></svg>
                </button>
                <div class="collapse" id="productSubmenu">
                  <ul class="nav flex-column ms-3 ps-3 border-start border-secondary-subtle mt-1 gap-1">
                    <li>
                      <a href="#" class="nav-link text-secondary py-1 px-2 small">Daftar Produk</a>
                    </li>
                    <li>
                      <a href="#" class="nav-link text-secondary py-1 px-2 small">Tambah Produk</a>
                    </li>
                    <li>
                      <a href="#" class="nav-link text-secondary py-1 px-2 small">Kategori</a>
                    </li>
                  </ul>
                </div>
              </li>
              
              <!-- Item lainnya -->
            </ul>
          </nav>
          
          <!-- User info di bawah -->
          <div class="p-3 border-top border-secondary-subtle">
            <div class="d-flex align-items-center gap-2">
              <img src="avatar.jpg" class="rounded-circle" width="36" height="36" alt="Admin avatar">
              <div class="flex-grow-1 overflow-hidden">
                <div class="text-white fw-medium small text-truncate">Administrator</div>
                <div class="text-secondary" style="font-size: 0.75rem;">admin@email.com</div>
              </div>
              <a href="#" class="text-secondary" title="Logout">
                <svg width="18" height="18"><!-- logout icon --></svg>
              </a>
            </div>
          </div>
        </aside>
        ```
        
47. `[[47. Dashboard Page — Stats Cards & Charts]]`
    
    - _Langkah konkret_: Halaman utama dashboard:
        
        HTML
        
        ```
        <!-- Stats Overview Cards -->
        <div class="row g-4 mb-4">
          <!-- Stat card 1 -->
          <div class="col-6 col-xl-3">
            <div class="card border-0 shadow-sm h-100">
              <div class="card-body p-4">
                <div class="d-flex align-items-center justify-content-between mb-3">
                  <div class="feature-icon feature-icon--primary rounded-3 p-2">
                    <svg width="24" height="24"><!-- icon --></svg>
                  </div>
                  <span class="badge bg-success-subtle text-success rounded-pill small">
                    ↑ 12%
                  </span>
                </div>
                <div class="display-6 fw-bold mb-1">1,234</div>
                <div class="text-muted small">Total Pengguna</div>
              </div>
            </div>
          </div>
          
          <!-- Repeat untuk stat lainnya (Pendapatan, Order, Produk) -->
        </div>
        
        <!-- Charts Row -->
        <div class="row g-4 mb-4">
          <div class="col-12 col-lg-8">
            <div class="card border-0 shadow-sm">
              <div class="card-header bg-transparent border-0 pt-4 px-4 pb-0 d-flex align-items-center justify-content-between">
                <h6 class="mb-0 fw-bold">Pendapatan Bulanan</h6>
                <!-- Filter tabs -->
                <ul class="nav nav-pills nav-sm">
                  <li class="nav-item"><a class="nav-link active py-1 px-3 small" href="#">6 Bulan</a></li>
                  <li class="nav-item"><a class="nav-link py-1 px-3 small" href="#">1 Tahun</a></li>
                </ul>
              </div>
              <div class="card-body px-4 pb-4">
                <canvas id="revenueChart" height="300"></canvas>
              </div>
            </div>
          </div>
          
          <div class="col-12 col-lg-4">
            <div class="card border-0 shadow-sm h-100">
              <div class="card-header bg-transparent border-0 pt-4 px-4 pb-0">
                <h6 class="mb-0 fw-bold">Distribusi Produk</h6>
              </div>
              <div class="card-body px-4 pb-4 d-flex align-items-center justify-content-center">
                <canvas id="categoryChart"></canvas>
              </div>
            </div>
          </div>
        </div>
        
        <!-- Recent Orders Table -->
        <div class="card border-0 shadow-sm">
          <div class="card-header bg-transparent border-0 pt-4 px-4 pb-0 d-flex align-items-center justify-content-between">
            <h6 class="mb-0 fw-bold">Order Terbaru</h6>
            <a href="#orders" class="btn btn-outline-primary btn-sm">Lihat Semua</a>
          </div>
          <div class="card-body p-0">
            <div class="table-responsive">
              <table class="table table-hover align-middle mb-0">
                <thead class="table-light">
                  <tr>
                    <th class="ps-4">Order ID</th>
                    <th>Pelanggan</th>
                    <th>Produk</th>
                    <th>Total</th>
                    <th>Status</th>
                    <th class="pe-4">Aksi</th>
                  </tr>
                </thead>
                <tbody id="ordersTableBody">
                  <!-- Diisi oleh JavaScript -->
                </tbody>
              </table>
            </div>
          </div>
        </div>
        ```
        
    - _Langkah konkret_: Integrasikan Chart.js untuk grafik pendapatan dan pie chart kategori
48. `[[48. Data Table dengan Search, Sort & Filter]]`
    
    - _Langkah konkret_: Tabel produk yang bisa difilter:
        
        HTML
        
        ```
        <!-- Toolbar tabel -->
        <div class="d-flex flex-wrap gap-3 mb-4 align-items-center">
          <!-- Search -->
          <div class="flex-grow-1" style="max-width: 300px;">
            <div class="input-group">
              <span class="input-group-text bg-transparent">🔍</span>
              <input
                type="search"
                class="form-control border-start-0"
                id="tableSearch"
                placeholder="Cari produk..."
              >
            </div>
          </div>
          
          <!-- Filter kategori -->
          <select class="form-select" style="width: auto;" id="categoryFilter">
            <option value="">Semua Kategori</option>
            <option value="elektronik">Elektronik</option>
            <option value="fashion">Fashion</option>
          </select>
          
          <!-- Filter status -->
          <select class="form-select" style="width: auto;" id="statusFilter">
            <option value="">Semua Status</option>
            <option value="aktif">Aktif</option>
            <option value="nonaktif">Nonaktif</option>
          </select>
          
          <!-- Actions -->
          <div class="ms-auto d-flex gap-2">
            <button class="btn btn-outline-secondary btn-sm">Export</button>
            <button class="btn btn-primary btn-sm" data-bs-toggle="modal" data-bs-target="#addProductModal">
              + Tambah Produk
            </button>
          </div>
        </div>
        
        <!-- Table -->
        <div class="card border-0 shadow-sm">
          <div class="table-responsive">
            <table class="table table-hover align-middle mb-0" id="productsTable">
              <thead class="table-light">
                <tr>
                  <th class="ps-4 py-3">
                    <input type="checkbox" class="form-check-input" id="selectAll">
                  </th>
                  <th class="py-3 cursor-pointer" data-sort="name">
                    Produk <span class="text-muted ms-1">↕</span>
                  </th>
                  <th class="py-3 cursor-pointer" data-sort="category">Kategori</th>
                  <th class="py-3 cursor-pointer" data-sort="price">Harga</th>
                  <th class="py-3 cursor-pointer" data-sort="stock">Stok</th>
                  <th class="py-3">Status</th>
                  <th class="py-3 pe-4">Aksi</th>
                </tr>
              </thead>
              <tbody id="productsTableBody">
                <!-- JavaScript akan mengisi ini -->
              </tbody>
            </table>
          </div>
          
          <!-- Pagination -->
          <div class="card-footer bg-transparent d-flex justify-content-between align-items-center py-3 px-4">
            <div class="text-muted small">
              Menampilkan <span id="showingFrom">1</span>-<span id="showingTo">10</span>
              dari <span id="totalItems">50</span> item
            </div>
            <nav aria-label="Table pagination">
              <ul class="pagination pagination-sm mb-0" id="tablePagination">
                <!-- Diisi oleh JavaScript -->
              </ul>
            </nav>
          </div>
        </div>
        ```
        
    - _Langkah konkret_: Implementasikan search, filter, sort, dan pagination menggunakan vanilla JavaScript

---

### 🏗️ Proyek Final Level 7

text

```
PROYEK: Admin Dashboard "ShopAdmin"
────────────────────────────────────────────────────────────────
HALAMAN:
├── dashboard.html    ← stats cards, charts, recent orders
├── products.html     ← tabel dengan search/filter/sort/pagination
├── orders.html       ← tabel order dengan status workflow
├── users.html        ← manajemen pengguna
├── analytics.html    ← charts dan statistik lengkap
├── settings.html     ← form pengaturan akun dan aplikasi
└── login.html        ← halaman login profesional

FITUR WAJIB:
├── Sidebar: fixed di desktop, offcanvas di mobile
├── Topbar: breadcrumb, search global, notifikasi, user menu
├── Dark mode toggle (persist di localStorage)
├── Stats cards dengan angka animasi saat pertama load
├── Charts dengan Chart.js (bar, line, doughnut)
├── Data table: search, filter, sort, pagination (vanilla JS)
├── CRUD modal: tambah/edit/hapus dengan konfirmasi
├── Toast notification untuk semua aksi
├── Loading skeleton untuk state loading
└── Responsive sempurna (320px - 2560px)

KUALITAS:
├── Lighthouse: Performance ≥ 85, Accessibility ≥ 95
├── axe DevTools: 0 critical issues
├── Bootstrap via Sass dengan kustomisasi brand penuh
└── Dokumentasi di README.md
```

---

## 📊 Ringkasan Progress Tracking

### Satu Project, 7 Level Enhancement

text

```
Level 1: Landing page tahap 1 — grid, typography, spacing
  + Level 2: + Navbar, card, button, form, badge, alert
  + Level 3: + Modal, accordion, tab, toast, table, progress
  + Level 4: + Kustomisasi Sass — brand colors, custom utilities
  + Level 5: + Mobile audit, aksesibilitas, performa, Lighthouse
  + Level 6: + Dark mode, offcanvas, Bootstrap JS API
  + Level 7: Admin dashboard lengkap (capstone project)
```

### Tabel Progress

|Level|Poin|Durasi|Output Konkret|
|---|---|---|---|
|🟢 **1**|1-14|Minggu 1-4|Landing page dengan grid dan typography|
|🔵 **2**|15-22|Minggu 4-7|+ Navbar, cards, buttons, form, alert|
|🟡 **3**|23-28|Minggu 7-10|+ Modal, accordion, tab, toast, table|
|🟠 **4**|29-33|Minggu 10-14|+ Brand kustomisasi via Sass|
|🔴 **5**|34-40|Minggu 14-18|+ Mobile perfect, a11y, performance|
|⚫ **6**|41-44|Minggu 18-24|+ Dark mode, offcanvas, JS API|
|🟣 **7**|45-48|Minggu 24+|Admin dashboard production-ready|

---

### Benang Merah Utama Sepanjang Roadmap

text

```
Poin 5  (Grid 12 kolom)        → Digunakan di SETIAP section
Poin 7  (Breakpoints)          → Fondasi semua responsive (mobile-first)
Poin 9  (Gutter)               → Konsisten di setiap row
Poin 13 (Sistem warna)         → Badge, button, alert, tabel semuanya pakai ini
Poin 14 (Spacing)              → py-5, mb-4 dipakai konsisten di semua level
Poin 17 (Card)                 → Komponen paling sering digunakan
Poin 19 (Navbar collapse)      → Di-enhance Level 6 dengan offcanvas
Poin 23 (Modal)                → Di-enhance Level 6 dengan JS API (event listener)
Poin 31 (Sass variables)       → Mengubah seluruh tampilan sekaligus
Poin 36 (Display utilities)    → Hide/show per breakpoint dipakai di Level 7
Poin 38 (visually-hidden)      → Aksesibilitas di semua komponen interaktif
Poin 41 (Dark mode)            → Data-bs-theme di html, semua komponen ikut
Poin 44 (Bootstrap JS API)     → Kontrol modal, toast di Level 7
```

---

## 💡 Cara Menggunakan Roadmap Ini

text

```
Setiap poin mengikuti format:
┌──────────────────────────────────────────────────────┐
│ 💡 Konteks: mengapa class/komponen ini ada           │
│ 🔗 Benang Merah: koneksi ke poin sebelum/sesudah    │
│ 📋 Kode: HTML/CSS/JS yang langsung bisa dicopy      │
│          dan dimodifikasi untuk project kita         │
│ ✅ Langkah konkret: verifikasi berhasil             │
└──────────────────────────────────────────────────────┘
```

**Aturan yang Tidak Boleh Dilanggar:**

1. **Buka dokumentasi resmi** (`getbootstrap.com/docs`) setiap kali pakai class baru
2. **Gunakan DevTools** — inspect element untuk lihat class apa yang Bootstrap tambahkan
3. **Jangan override Bootstrap dengan CSS yang sama** — gunakan variable Sass atau class yang lebih spesifik
4. **Commit setelah setiap checkpoint** — git history adalah progress tracker
5. **Test di mobile di setiap poin** — bukan hanya di akhir
6. **Jangan tambahkan custom CSS** jika Bootstrap utility sudah bisa — DRY principle

---

_Roadmap Bootstrap v1.0 — Step-by-Step, One Project, System Thinking_  
_Setiap class dipilih dengan alasan — bukan sekadar "yang penting tampil"_