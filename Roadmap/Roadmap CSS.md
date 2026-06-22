# 🗺️ Roadmap CSS: Step-by-Step Mendesain Halaman Web Nyata

## Filosofi Roadmap Ini

> **"CSS bukan sekadar membuat halaman terlihat cantik — CSS adalah bahasa yang menerjemahkan desain menjadi pengalaman yang nyaman di setiap perangkat, untuk setiap pengguna"** — setiap properti yang dipilih ada alasannya, bukan trial-and-error tanpa arah.

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: website portofolio dari roadmap HTML dilanjutkan dan distyling dari nol
- **Visual Progress**: setiap poin = perubahan nyata yang bisa dilihat langsung di browser
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **DevTools sebagai Teman**: setiap topik disertai cara eksplorasi di browser DevTools

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: Styling dasar — warna, tipografi, selector pertama
    ↓ (enhance, tidak mulai dari nol)
Level 2: + Box Model + Background + Layout dasar
    ↓ (enhance)
Level 3: + Cascade + Pseudo-class + Tipografi lanjutan
    ↓ (enhance)
Level 4: + Flexbox + CSS Grid + Positioning
    ↓ (enhance)
Level 5: + Responsive Design + Animasi + Transform
    ↓ (enhance)
Level 6: + CSS Variables + Fitur Modern + Arsitektur CSS
    ↓ (enhance)
Level 7: + SASS + Framework + Design System + Portfolio Published
```

---

## 🟢 LEVEL 1: FONDASI — CSS PERTAMA & TIPOGRAFI (Minggu 1-2)

> **Tema**: _"Dari halaman HTML polos ke halaman dengan warna, font, dan tampilan yang enak dilihat"_  
> **Benang Merah**: Cara CSS bekerja → Hubungkan ke HTML → Selector → Warna → Tipografi → Halaman mulai terlihat seperti desain  
> **Output**: Website portofolio dengan warna brand, font yang dipilih, dan tampilan teks yang bersih

---

### A. Cara CSS Bekerja & Setup

> 💡 **Mengapa dimulai di sini?** Banyak pemula langsung copy-paste CSS tanpa tahu urutan prioritas dan cara browser membaca stylesheet. Memahami ini dari awal mencegah kebingungan saat style "tidak mau jalan".

text

```
Benang Merah Bagian A:
HTML portofolio sudah ada (dari roadmap HTML) →
CSS memberi tampilan pada HTML tersebut →
Tiga cara menulis CSS: mana yang tepat untuk kita →
External stylesheet: satu file CSS untuk semua halaman →
DevTools: lab eksperimen CSS real-time
```

1. `[[1. Apa itu CSS & Cara Browser Memprosesnya]]`
    
    - CSS = Cascading Style Sheets — memisahkan **tampilan** dari **struktur** (HTML)
    - Browser membaca HTML dulu (DOM), lalu CSS (CSSOM), baru menggabungkan dan render
    - Analogi: HTML adalah tulang kerangka, CSS adalah kulit, rambut, dan pakaian
    - Mengapa pisahkan CSS dari HTML: satu file CSS bisa styling puluhan halaman sekaligus
    - _Langkah konkret_: Buka website portofolio dari roadmap HTML, lihat betapa polosnya tanpa CSS
2. `[[2. Tiga Cara Menulis CSS — Mana yang Tepat & Kapan]]`
    
    - **Inline style**: `<p style="color: red;">` — langsung di tag HTML
        - ❌ Sulit dipelihara, spesifisitas tertinggi (masalah), tidak reusable
        - ✅ Hanya untuk: override cepat yang sangat spesifik, styling dinamis via JavaScript
    - **Internal style**: `<style>` di dalam `<head>`
        - ✅ Cocok untuk: halaman tunggal, prototipe cepat
        - ❌ Tidak bisa dibagikan antar halaman
    - **External stylesheet**: file `.css` terpisah dihubungkan via `<link>`
        - ✅ **Ini yang akan kita gunakan** — satu file untuk semua halaman, cacheable
    - _Langkah konkret_: Buat file `styles.css` di root project, hubungkan ke semua halaman:
        
        HTML
        
        ```
        <link rel="stylesheet" href="styles.css">
        ```
        
3. `[[3. Anatomi Aturan CSS — Selector, Property & Value]]`
    
    - Tiga bagian wajib setiap aturan CSS:
        
        CSS
        
        ```
        /* Ini adalah komentar CSS */
        
        selector {        /* siapa yang distyle */
          property: value; /* apa yang diubah: menjadi apa */
        }
        
        /* Contoh nyata: */
        h1 {
          color: #1e40af;      /* warna teks biru gelap */
          font-size: 2.5rem;   /* ukuran teks */
        }
        ```
        
    - Tanda titik dua `:` memisahkan property dari value
    - Tanda titik koma `;` mengakhiri setiap deklarasi — jangan lupa!
    - Kurung kurawal `{}` membungkus semua deklarasi untuk satu selector
    - _Langkah konkret_: Tulis aturan CSS pertama — beri warna pada `<h1>` di halaman beranda
4. `[[4. Browser DevTools — Lab Eksperimen CSS Real-time]]`
    
    - Buka Chrome DevTools: `F12` atau klik kanan → "Inspect"
    - Tab **Elements**: klik elemen → lihat HTML dan CSS yang diterapkan
    - Panel **Styles**: edit CSS langsung dan lihat perubahan real-time (perubahan tidak disimpan)
    - Panel **Computed**: lihat nilai final setelah semua CSS diterapkan
    - Cara debug CSS yang "tidak mau jalan": cari di Computed apakah properti di-override
    - _Langkah konkret_: Klik `<h1>` di DevTools, ubah warna langsung di panel Styles — refresh untuk reset

---

### B. Selector — Memilih Elemen yang Ingin Distyle

> 💡 **Benang Merah ke A**: File CSS sudah terhubung. Sekarang kita tentukan **elemen mana** yang akan distyle. Pemilihan selector yang tepat membuat CSS mudah dipelihara.

text

```
Benang Merah Bagian B:
File CSS terhubung ke HTML (Poin 2) →
Type selector: pilih semua elemen dengan tag tertentu →
Class selector: pilih elemen dengan kelas tertentu →
ID selector: pilih elemen unik →
Descendant selector: pilih elemen di dalam elemen lain →
Grouping: satu aturan untuk beberapa selector
```

5. `[[5. Type & Universal Selector — Fondasi Pemilihan Elemen]]`
    
    - **Universal selector** `*`: semua elemen (digunakan untuk reset)
    - **Type selector**: pilih elemen berdasarkan nama tag
    - Tambahkan reset CSS global dulu sebelum styling apapun:
        
        CSS
        
        ```
        /* Reset: hapus gaya bawaan browser yang tidak konsisten */
        *,
        *::before,
        *::after {
          box-sizing: border-box; /* akan dipelajari di Level 2 */
          margin: 0;
          padding: 0;
        }
        
        /* Type selector: styling semua elemen tertentu */
        body {
          font-family: sans-serif;
          line-height: 1.6;
        }
        
        h1, h2, h3 {
          line-height: 1.2;
        }
        ```
        
    - _Langkah konkret_: Tambahkan reset ini — halaman akan sedikit berubah (margin hilang)
6. `[[6. Class & ID Selector — Memilih Elemen Spesifik]]`
    
    - **Class selector** `.nama-kelas`: satu class bisa dipakai di banyak elemen
    - **ID selector** `#nama-id`: unik, hanya satu per halaman
    - Kapan pakai class vs ID:
        - Class: untuk styling yang bisa berulang — card, button, section
        - ID: untuk identifikasi unik (anchor link, JavaScript) — **bukan untuk styling utama**
    - _Langkah konkret_: Tambahkan class ke HTML dan style menggunakan class:
        
        HTML
        
        ```
        <!-- Di HTML -->
        <nav class="main-nav">...</nav>
        <article class="project-card">...</article>
        ```
        
        CSS
        
        ```
        /* Di CSS */
        .main-nav {
          background-color: #1e40af;
        }
        
        .project-card {
          border: 1px solid #e5e7eb;
          border-radius: 8px;
        }
        ```
        
7. `[[7. Selector Relasional — Descendant, Child & Sibling]]`
    
    - **Descendant** (spasi): pilih elemen di dalam elemen lain (semua tingkat):
        
        CSS
        
        ```
        /* Semua a di dalam nav */
        nav a { color: white; }
        ```
        
    - **Child** (`>`): hanya anak langsung:
        
        CSS
        
        ```
        /* Hanya li yang langsung anak ul.main-nav */
        .main-nav > ul > li { display: inline-block; }
        ```
        
    - **Adjacent sibling** (`+`): elemen saudara tepat berikutnya:
        
        CSS
        
        ```
        /* h2 yang langsung diikuti p */
        h2 + p { font-size: 1.1rem; color: #6b7280; }
        ```
        
    - **General sibling** (`~`): semua elemen saudara setelahnya
    - _Langkah konkret_: Style link di navigasi menggunakan `nav a`
8. `[[8. Grouping Selector — Satu Aturan untuk Beberapa Elemen]]`
    
    - Pisahkan selector dengan koma untuk menerapkan aturan yang sama:
        
        CSS
        
        ```
        /* Tanpa grouping — berulang: */
        h1 { color: #1e40af; }
        h2 { color: #1e40af; }
        h3 { color: #1e40af; }
        
        /* Dengan grouping — efisien: */
        h1, h2, h3 {
          color: #1e40af;
          font-weight: 700;
        }
        ```
        
    - _Langkah konkret_: Kelompokkan semua heading dengan style yang sama

---

### C. Warna — Sistem Warna yang Konsisten

> 💡 **Benang Merah ke Selector**: Kita sudah bisa memilih elemen. Sekarang kita tentukan warna — dan bukan sembarang warna, tapi sistem warna yang konsisten untuk seluruh website.

text

```
Benang Merah Bagian C:
Bisa pilih elemen (Poin 5-8) →
Tentukan sistem warna brand →
Format warna: hex, rgb, hsl →
color & background-color →
CSS Custom Properties: satu tempat untuk semua warna →
Konsistensi di seluruh website
```

9. `[[9. Format Warna CSS — Hex, RGB & HSL]]`
    
    - **Hex**: `#1e40af` — 6 karakter hexadecimal (RR GG BB)
        - Paling umum digunakan, mudah di-copy dari design tool
        - Shorthand: `#fff` = `#ffffff`, `#000` = `#000000`
    - **RGB/RGBA**: `rgb(30, 64, 175)` atau `rgba(30, 64, 175, 0.5)`
        - Mudah untuk transparansi dengan `rgba`
    - **HSL/HSLA**: `hsl(224, 72%, 40%)` — Hue, Saturation, Lightness
        - Lebih intuitif untuk membuat variasi warna (gelap/terang dari warna yang sama)
    - _Langkah konkret_: Pilih warna brand utama dalam format hex, buat 3 variasi (gelap, normal, terang)
10. `[[10. CSS Custom Properties — Sistem Warna yang Terpusat]]`
    
    - Variabel CSS: definisikan sekali, gunakan di mana saja — ubah satu tempat, berubah semua:
        
        CSS
        
        ```
        /* Definisikan di :root agar global */
        :root {
          /* Brand colors */
          --color-primary: #1e40af;
          --color-primary-light: #3b82f6;
          --color-primary-dark: #1e3a8a;
          
          /* Neutral colors */
          --color-text: #111827;
          --color-text-muted: #6b7280;
          --color-background: #ffffff;
          --color-surface: #f9fafb;
          --color-border: #e5e7eb;
          
          /* Semantic colors */
          --color-success: #10b981;
          --color-error: #ef4444;
          --color-warning: #f59e0b;
        }
        
        /* Penggunaan: */
        h1 { color: var(--color-primary); }
        body { background-color: var(--color-background); }
        ```
        
    - _Langkah konkret_: Definisikan semua variabel warna di `:root`, ganti semua hardcoded color
11. `[[11. color & background-color — Dua Properti Warna Paling Dasar]]`
    
    - `color`: warna teks (dan beberapa elemen lain seperti border yang inherit)
    - `background-color`: warna latar belakang elemen
    - _Langkah konkret_: Terapkan sistem warna ke semua elemen utama:
        
        CSS
        
        ```
        body {
          color: var(--color-text);
          background-color: var(--color-background);
        }
        
        header {
          background-color: var(--color-primary);
          color: white;
        }
        
        .project-card {
          background-color: var(--color-surface);
          border-color: var(--color-border);
        }
        ```
        

---

### D. Tipografi — Teks yang Nyaman Dibaca

> 💡 **Benang Merah ke Warna**: Sistem warna sudah ada. Tipografi adalah langkah berikutnya yang paling impactful — font yang tepat membuat halaman langsung terlihat profesional.

text

```
Benang Merah Bagian D:
Sistem warna terdefinisi (Poin 9-11) →
Import Google Fonts →
font-family: tentukan font di sistem warna CSS →
font-size, font-weight: hierarki teks →
line-height, text-align: keterbacaan →
Seluruh website punya tipografi yang konsisten
```

12. `[[12. font-family & Google Fonts — Font yang Tidak Ada di Komputer Semua Orang]]`
    
    - Font stack: urutan font alternatif jika font pertama tidak tersedia:
        
        CSS
        
        ```
        /* Tanpa Google Fonts: menggunakan system fonts */
        body {
          font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 
                       Roboto, Oxygen, Ubuntu, sans-serif;
        }
        ```
        
    - Dengan Google Fonts — tambahkan di `<head>` semua halaman:
        
        HTML
        
        ```
        <link rel="preconnect" href="https://fonts.googleapis.com">
        <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
        <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Merriweather:wght@700&display=swap" rel="stylesheet">
        ```
        
    - Update CSS Variables:
        
        CSS
        
        ```
        :root {
          --font-sans: 'Inter', -apple-system, sans-serif;  /* untuk UI */
          --font-serif: 'Merriweather', Georgia, serif;      /* untuk heading dekoratif */
          --font-mono: 'Fira Code', 'Courier New', monospace; /* untuk kode */
        }
        
        body { font-family: var(--font-sans); }
        code { font-family: var(--font-mono); }
        ```
        
    - _Langkah konkret_: Import Inter dari Google Fonts, terapkan ke seluruh website
13. `[[13. font-size — Skala Tipografi yang Harmonis]]`
    
    - Satuan yang tepat untuk `font-size`:
        - `px`: absolut, tidak responsif terhadap preferensi user — **hindari untuk body text**
        - `rem`: relatif ke root (`html`), responsif terhadap preferensi browser — **gunakan ini**
        - `em`: relatif ke induk — berguna untuk komponen yang scalable
    - Buat skala tipografi menggunakan CSS Variables:
        
        CSS
        
        ```
        :root {
          /* Typographic scale (ratio 1.25 - Major Third) */
          --text-xs:   0.75rem;   /* 12px */
          --text-sm:   0.875rem;  /* 14px */
          --text-base: 1rem;      /* 16px (default browser) */
          --text-lg:   1.125rem;  /* 18px */
          --text-xl:   1.25rem;   /* 20px */
          --text-2xl:  1.5rem;    /* 24px */
          --text-3xl:  1.875rem;  /* 30px */
          --text-4xl:  2.25rem;   /* 36px */
          --text-5xl:  3rem;      /* 48px */
        }
        
        /* Terapkan ke heading */
        h1 { font-size: var(--text-5xl); }
        h2 { font-size: var(--text-3xl); }
        h3 { font-size: var(--text-2xl); }
        h4 { font-size: var(--text-xl); }
        p  { font-size: var(--text-base); }
        small { font-size: var(--text-sm); }
        ```
        
    - _Langkah konkret_: Terapkan skala tipografi ke seluruh website
14. `[[14. font-weight, font-style & text-decoration]]`
    
    - `font-weight`: ketebalan teks
        
        CSS
        
        ```
        /* Menggunakan variable */
        :root {
          --font-weight-normal: 400;
          --font-weight-medium: 500;
          --font-weight-semibold: 600;
          --font-weight-bold: 700;
        }
        
        h1, h2, h3 { font-weight: var(--font-weight-bold); }
        h4, h5     { font-weight: var(--font-weight-semibold); }
        p          { font-weight: var(--font-weight-normal); }
        ```
        
    - `font-style: italic`: untuk kutipan, penekanan halus
    - `text-decoration: underline | none | line-through`: garis pada teks
        
        CSS
        
        ```
        /* Hilangkan underline bawaan link, tambahkan kustom */
        a {
          text-decoration: none;
          color: var(--color-primary);
        }
        a:hover {
          text-decoration: underline;
        }
        ```
        
    - _Langkah konkret_: Terapkan font-weight yang tepat ke semua heading dan link
15. `[[15. line-height, text-align & text-transform — Keterbacaan Teks]]`
    
    - `line-height`: jarak antar baris — nilai ideal 1.5-1.7 untuk body, 1.2-1.3 untuk heading:
        
        CSS
        
        ```
        :root {
          --leading-tight:  1.25; /* heading */
          --leading-normal: 1.5;  /* body text */
          --leading-loose:  1.75; /* konten panjang */
        }
        
        body { line-height: var(--leading-normal); }
        h1, h2, h3 { line-height: var(--leading-tight); }
        ```
        
    - `text-align`: perataan teks
        
        CSS
        
        ```
        /* Jangan justify kecuali dengan hyphens */
        p { text-align: left; }
        .hero-title { text-align: center; }
        ```
        
    - `text-transform: uppercase | capitalize | lowercase`
    - _Langkah konkret_: Set `line-height` yang nyaman di semua elemen teks

---

### 🏗️ Checkpoint Level 1

text

```
✅ Checklist sebelum lanjut ke Level 2:

FILE: styles.css
├── Reset CSS global (*,*::before,*::after { box-sizing: border-box; margin: 0; padding: 0; })
├── CSS Custom Properties di :root untuk warna (min. 8 variabel)
├── CSS Custom Properties di :root untuk font family
├── CSS Custom Properties di :root untuk font size scale
├── CSS Custom Properties di :root untuk font weight
├── CSS Custom Properties di :root untuk line-height
├── Google Fonts terimport dan diterapkan
├── body: color, background-color, font-family, line-height
├── heading h1-h4: font-size, font-weight, line-height, color
├── a: color, text-decoration (none), hover state
├── p: font-size, line-height
├── code: font-family (monospace)
├── header: background-color, color
└── Semua class yang sudah ada di HTML terdefinisi di CSS

VISUAL CHECK:
├── Font terlihat berubah dari default browser
├── Warna brand muncul di header dan heading
├── Link tidak bergaris bawah (kecuali saat hover)
└── Teks terasa nyaman dibaca (tidak terlalu rapat/renggang)

Commit: feat: add base CSS with color system, typography, and selectors
```

---

## 🔵 LEVEL 2: BOX MODEL & LAYOUT DASAR (Minggu 2-4)

> **Tema**: _"Dari teks yang mengalir ke elemen yang punya ruang, border, dan layout yang terkontrol"_  
> **Benang Merah**: Tipografi sudah bagus (Level 1) → elemen butuh ruang → Box Model → spacing system → background → layout dasar  
> **Output**: Website dengan spacing yang konsisten, card yang terdefinisi, dan background yang menarik

---

### E. Box Model — Fondasi Semua Layout CSS

> 💡 **Mengapa ini paling penting?** Setiap hal di CSS berkaitan dengan Box Model. Jika tidak paham ini, spacing akan selalu terasa "trial and error". Memahami ini = tahu persis mengapa elemen berada di posisi tertentu.

text

```
Benang Merah Bagian E:
Elemen punya teks tapi tidak punya "ruang" yang terkontrol (Level 1) →
Box Model: setiap elemen adalah kotak →
content → padding → border → margin →
box-sizing: border-box agar logis →
Spacing system: konsistensi di seluruh website
```

16. `[[16. Konsep Box Model — Setiap Elemen adalah Kotak]]`
    
    - Setiap elemen HTML adalah kotak dengan 4 lapisan (dari dalam ke luar):
        1. **Content**: area konten (teks, gambar)
        2. **Padding**: ruang antara konten dan border (di dalam elemen)
        3. **Border**: garis tepi elemen
        4. **Margin**: ruang di luar elemen (antar elemen)
    - Visualisasikan di DevTools: pilih elemen → lihat box model diagram di panel Computed
    - _Langkah konkret_: Pilih `.project-card` di DevTools, lihat box model-nya
17. `[[17. box-sizing: border-box — CSS yang Lebih Logis]]`
    
    - **Problem** `content-box` (default): `width: 200px` + `padding: 20px` = total 240px — tidak intuitif!
    - **Solusi** `border-box`: `width: 200px` = total 200px (padding dan border masuk ke dalam)
    - Ini sudah kita tulis di reset (Poin 5), tapi perlu dipahami mengapa:
        
        CSS
        
        ```
        /* Sudah ada di reset, ini penjelasannya: */
        *,
        *::before,
        *::after {
          box-sizing: border-box;
          /* Sekarang: width = content + padding + border */
          /* Bukan: width = hanya content */
        }
        ```
        
    - _Langkah konkret_: Di DevTools, ubah `box-sizing` dari `border-box` ke `content-box` dan lihat perbedaannya
18. `[[18. padding — Ruang di Dalam Elemen]]`
    
    - Padding: ruang antara konten dan border — membesar ukuran elemen (dengan `border-box`, tidak)
    - Buat spacing system menggunakan CSS Variables:
        
        CSS
        
        ```
        :root {
          /* Spacing scale */
          --space-1:  0.25rem;  /* 4px  */
          --space-2:  0.5rem;   /* 8px  */
          --space-3:  0.75rem;  /* 12px */
          --space-4:  1rem;     /* 16px */
          --space-5:  1.25rem;  /* 20px */
          --space-6:  1.5rem;   /* 24px */
          --space-8:  2rem;     /* 32px */
          --space-10: 2.5rem;   /* 40px */
          --space-12: 3rem;     /* 48px */
          --space-16: 4rem;     /* 64px */
          --space-20: 5rem;     /* 80px */
          --space-24: 6rem;     /* 96px */
        }
        ```
        
    - Shorthand padding:
        
        CSS
        
        ```
        .card {
          padding: var(--space-6);                           /* semua sisi */
          padding: var(--space-4) var(--space-6);           /* atas-bawah kiri-kanan */
          padding: var(--space-2) var(--space-4) var(--space-6); /* atas kiri-kanan bawah */
          padding: var(--space-2) var(--space-4) var(--space-6) var(--space-8); /* atas kanan bawah kiri */
        }
        ```
        
    - _Langkah konkret_: Terapkan padding ke semua section dan card menggunakan spacing variables
19. `[[19. margin — Ruang di Luar Elemen & Margin Collapsing]]`
    
    - Margin: ruang di luar border — jarak antar elemen
    - `margin: auto`: centering elemen block secara horizontal (jika punya width)
    - **Margin collapsing**: dua margin vertikal bertemu → hanya yang lebih besar yang berlaku:
        
        CSS
        
        ```
        /* h2 margin-bottom: 24px, p margin-top: 16px */
        /* Jarak aktual: 24px (bukan 40px!) */
        h2 { margin-bottom: var(--space-6); }
        p  { margin-top: var(--space-4); }
        ```
        
    - _Langkah konkret_: Tambahkan margin yang tepat ke semua heading, paragraph, dan section
20. `[[20. Membuat Container & Centering Konten]]`
    
    - Container: elemen yang membatasi lebar konten agar tidak terlalu lebar di layar besar:
        
        CSS
        
        ```
        .container {
          width: 100%;
          max-width: 1200px;   /* tidak lebih lebar dari 1200px */
          margin-inline: auto; /* centering horizontal */
          padding-inline: var(--space-6); /* padding kiri-kanan agar tidak nempel tepi */
        }
        
        /* Versi dengan CSS modern: */
        .container {
          width: min(100% - 3rem, 75rem); /* max 1200px, min 100% - 48px */
          margin-inline: auto;
        }
        ```
        
    - Bungkus konten semua halaman dengan `.container`:
        
        HTML
        
        ```
        <main>
          <div class="container">
            <!-- konten halaman -->
          </div>
        </main>
        ```
        
    - _Langkah konkret_: Semua halaman punya container yang centered dengan padding kiri-kanan

---

### F. Border & Background

> 💡 **Benang Merah ke Box Model**: Border adalah bagian dari Box Model. Background mengisi area content + padding. Keduanya membuat card dan section terlihat terdefinisi.

text

```
Benang Merah Bagian F:
Box Model sudah dipahami (Poin 16-20) →
border: garis tepi elemen →
border-radius: sudut melengkung →
background-color: warna latar →
background-image: gambar atau gradien →
Section dengan visual yang menarik
```

21. `[[21. border & border-radius — Garis Tepi & Sudut Melengkung]]`
    
    - Border shorthand: `border: width style color`
        
        CSS
        
        ```
        :root {
          --radius-sm:   4px;
          --radius-md:   8px;
          --radius-lg:   12px;
          --radius-xl:   16px;
          --radius-full: 9999px; /* pill shape */
        }
        
        .card {
          border: 1px solid var(--color-border);
          border-radius: var(--radius-lg);
        }
        
        .badge {
          border-radius: var(--radius-full);
        }
        
        .avatar {
          border-radius: 50%;  /* lingkaran sempurna */
        }
        ```
        
    - `border-style`: `solid`, `dashed`, `dotted`, `double` — paling sering: `solid`
    - _Langkah konkret_: Terapkan border dan border-radius ke project cards dan badge
22. `[[22. outline vs border — Garis yang Tidak Menggeser Layout]]`
    
    - `outline`: mirip border tapi tidak mempengaruhi layout (tidak menambah ukuran)
    - Paling penting: **jangan hapus outline** — ini untuk aksesibilitas keyboard!
    - Ganti dengan outline yang lebih baik, bukan dihapus:
        
        CSS
        
        ```
        /* ❌ JANGAN: */
        :focus { outline: none; }
        
        /* ✅ LEBIH BAIK: kustom outline yang tetap terlihat */
        :focus-visible {
          outline: 2px solid var(--color-primary);
          outline-offset: 2px;
        }
        ```
        
    - _Langkah konkret_: Test navigasi keyboard (Tab) — fokus harus terlihat di semua elemen interaktif
23. `[[23. background-color, background-image & Gradien]]`
    
    - Background image:
        
        CSS
        
        ```
        .hero {
          background-image: url('images/hero-bg.jpg');
          background-size: cover;       /* gambar menutup seluruh area */
          background-position: center;  /* posisi tengah */
          background-repeat: no-repeat; /* tidak diulang */
        }
        ```
        
    - Gradien linear:
        
        CSS
        
        ```
        .hero {
          background-image: linear-gradient(
            135deg,
            var(--color-primary-dark) 0%,
            var(--color-primary) 100%
          );
        }
        
        /* Overlay di atas gambar: */
        .hero {
          background-image: 
            linear-gradient(rgba(0,0,0,0.5), rgba(0,0,0,0.5)),
            url('images/hero-bg.jpg');
        }
        ```
        
    - _Langkah konkret_: Tambahkan hero section dengan background gradien di halaman beranda
24. `[[24. Display: block, inline & inline-block]]`
    
    - **block**: elemen mengambil seluruh lebar — `<div>`, `<p>`, `<h1>`, `<section>`
    - **inline**: elemen mengikuti aliran teks, tidak bisa set width/height — `<span>`, `<a>`, `<em>`
    - **inline-block**: kombinasi — ikuti aliran teks tapi bisa set width/height — berguna untuk tombol
    - _Langkah konkret_: Ubah tombol menjadi `display: inline-block` dengan padding:
        
        CSS
        
        ```
        .btn {
          display: inline-block;
          padding: var(--space-3) var(--space-6);
          background-color: var(--color-primary);
          color: white;
          border-radius: var(--radius-md);
          text-decoration: none;
          font-weight: var(--font-weight-medium);
        }
        ```
        
25. `[[25. visibility & opacity — Tersembunyi Tapi Tetap Ada]]`
    
    - `display: none`: elemen hilang sepenuhnya dari layout
    - `visibility: hidden`: elemen tidak terlihat tapi tetap ada (space-nya masih ada)
    - `opacity: 0`: transparan total tapi masih ada dan interaktif
    - Kapan pakai masing-masing:
        - `display: none`: sembunyikan dan hilangkan dari layout (menu mobile, tab content)
        - `visibility: hidden`: sembunyikan tapi pertahankan ruang (efek tertentu)
        - `opacity: 0-1`: transparan (untuk transisi/animasi)

---

### 🏗️ Checkpoint Level 2

text

```
✅ Checklist sebelum lanjut ke Level 3:

CSS VARIABLES TAMBAHAN:
├── --space-1 hingga --space-24 (spacing scale)
├── --radius-sm, --radius-md, --radius-lg, --radius-xl, --radius-full
└── Semua variable digunakan secara konsisten

LAYOUT:
├── .container dengan max-width dan margin: auto
├── Semua section menggunakan .container
├── Padding yang konsisten menggunakan spacing variables
├── Margin yang tepat antar section

VISUAL:
├── .card/.project-card dengan border dan border-radius
├── Hero section dengan background gradien
├── .btn dengan padding, background, border-radius
├── :focus-visible dengan outline yang terlihat (aksesibilitas)
├── Avatar/foto profil dengan border-radius: 50%

UNDERSTANDING:
├── Bisa jelaskan perbedaan padding vs margin
├── Bisa jelaskan box-sizing: border-box
├── Bisa jelaskan display: block vs inline vs inline-block

Commit: feat: add spacing system, box model, borders, and background
```

---

## 🟡 LEVEL 3: CASCADE, PSEUDO-CLASS & TIPOGRAFI LANJUTAN (Minggu 4-6)

> **Tema**: _"Memahami mengapa CSS bekerja seperti itu — dan menggunakannya untuk interaksi"_  
> **Benang Merah**: Style dasar ada (Level 1-2) → memahami aturan prioritas → pseudo-class untuk interaksi → web fonts → tipografi halus  
> **Output**: Website dengan hover effects, focus styles, form yang stylist, dan tipografi yang polished

---

### G. Cascade & Specificity — Aturan yang Menentukan

> 💡 **Mengapa ini penting sekarang?** Pasti sudah pernah mengalami: "CSS saya tidak mau jalan!" Cascade dan specificity adalah jawabannya. Memahami ini = tidak pernah bingung lagi kenapa style tidak ter-apply.

text

```
Benang Merah Bagian G:
Style "tidak mau jalan" (sudah pasti dialami) →
Cascade: urutan stylesheet → siapa yang menang →
Specificity: seberapa spesifik selector → siapa yang menang →
Inheritance: nilai yang diwariskan →
Strategi: tulis CSS yang mudah di-maintain
```

26. `[[26. Cascade — Urutan Menentukan Pemenang]]`
    
    - Tiga faktor cascade (urutan prioritas):
        1. **Origin**: browser default → user stylesheet → author (kita) → `!important` user → `!important` author
        2. **Specificity**: semakin spesifik selector, semakin tinggi prioritas
        3. **Order**: jika sama persis, yang terakhir ditulis yang menang
    - _Langkah konkret_: Demonstrasi cascade:
        
        CSS
        
        ```
        /* Yang terakhir ditulis menang (jika specificity sama): */
        p { color: blue; }
        p { color: red; }  /* ← ini yang berlaku */
        ```
        
27. `[[27. Specificity — Cara Menghitung Prioritas Selector]]`
    
    - Sistem perhitungan specificity (dari tertinggi ke terendah):
        
        text
        
        ```
        Inline style:    1,0,0,0   (paling tinggi)
        ID selector:     0,1,0,0
        Class/Pseudo-class/Attribute: 0,0,1,0
        Type/Pseudo-element: 0,0,0,1
        Universal (*):   0,0,0,0   (paling rendah)
        ```
        
    - Contoh perhitungan:
        
        CSS
        
        ```
        p                    /* 0,0,0,1 */
        .card p              /* 0,0,1,1 */
        #hero p              /* 0,1,0,1 */
        .card .title         /* 0,0,2,0 */
        nav ul li a:hover    /* 0,0,1,4 */
        ```
        
    - _Langkah konkret_: Di DevTools, lihat panel Styles — CSS yang di-coret = di-override oleh yang lebih spesifik
28. `[[28. !important & Strategi Menghindarinya]]`
    
    - `!important`: override semua — **tanda desain CSS yang buruk** jika digunakan berlebihan
    - Kapan boleh dipakai: utility classes (`.hidden { display: none !important; }`), override third-party library
    - Strategi yang lebih baik: tulis selector yang lebih spesifik atau reorganisasi CSS
    - _Langkah konkret_: Cari semua `!important` yang sudah ditulis — bisakah dihapus dengan selector yang lebih tepat?
29. `[[29. Inheritance — Properti yang Diwariskan Otomatis]]`
    
    - Properti yang diwariskan: `color`, `font-family`, `font-size`, `font-weight`, `line-height`, `text-align` — dan beberapa lainnya
    - Properti yang tidak diwariskan: `margin`, `padding`, `border`, `background`, `width`, `height`
    - Nilai khusus untuk inheritance:
        
        CSS
        
        ```
        /* inherit: paksa mewarisi dari induk */
        .link { color: inherit; }
        
        /* initial: kembalikan ke nilai awal CSS spec */
        button { font-family: initial; }
        
        /* unset: inherit jika bisa, initial jika tidak */
        .reset-all { all: unset; }
        ```
        
    - _Langkah konkret_: Test — ubah `color` di `body`, lihat apakah semua teks dalam body ikut berubah

---

### H. Pseudo-Class — Interaksi Tanpa JavaScript

> 💡 **Benang Merah ke Cascade**: Pseudo-class menambah specificity ke selector. `a:hover` lebih spesifik dari `a`. Ini adalah cara CSS bereaksi terhadap state elemen.

text

```
Benang Merah Bagian H:
Selector statis (Level 1) →
Pseudo-class: selector berdasarkan STATE elemen →
:hover, :focus, :active: state interaksi →
:nth-child, :first-child: pola berulang →
:not: pengecualian →
Website menjadi interaktif tanpa JavaScript
```

30. `[[30. Pseudo-class Interaksi — hover, focus, active, visited]]`
    
    - State-based styling:
        
        CSS
        
        ```
        /* Tombol dengan semua state */
        .btn {
          background-color: var(--color-primary);
          color: white;
          transition: background-color 0.2s ease; /* halus (Level 5) */
        }
        
        .btn:hover {
          background-color: var(--color-primary-dark);
          cursor: pointer;
        }
        
        .btn:active {
          transform: translateY(1px); /* efek ditekan (Level 5) */
        }
        
        .btn:focus-visible {
          outline: 2px solid var(--color-primary);
          outline-offset: 2px;
        }
        
        /* Link */
        a { color: var(--color-primary); text-decoration: none; }
        a:hover { text-decoration: underline; }
        a:visited { color: var(--color-primary-dark); }
        ```
        
    - _Langkah konkret_: Tambahkan hover state ke semua tombol, link, dan card
31. `[[31. Pseudo-class Structural — nth-child, first-child, last-child]]`
    
    - Berguna untuk styling pola berulang tanpa tambah class:
        
        CSS
        
        ```
        /* Setiap baris ganjil di tabel */
        tr:nth-child(odd) { background-color: var(--color-surface); }
        
        /* Tiga item pertama */
        .project-card:nth-child(-n+3) { /* ... */ }
        
        /* Hapus border bawah item terakhir */
        .nav-item:last-child { border-bottom: none; }
        
        /* Item pertama: ukuran lebih besar */
        .blog-card:first-child {
          grid-column: span 2; /* akan belajar ini di Level 4 */
        }
        ```
        
    - _Langkah konkret_: Tambahkan alternating row color ke tabel pendidikan
32. `[[32. Pseudo-class :not() & :is() — Selector Cerdas]]`
    
    - `:not()`: pilih semua elemen kecuali yang cocok:
        
        CSS
        
        ```
        /* Tambahkan border bawah ke semua item KECUALI yang terakhir */
        .nav-item:not(:last-child) {
          border-bottom: 1px solid var(--color-border);
        }
        
        /* Semua input kecuali checkbox dan radio */
        input:not([type="checkbox"]):not([type="radio"]) {
          border: 1px solid var(--color-border);
          border-radius: var(--radius-md);
          padding: var(--space-3);
        }
        ```
        
    - `:is()`: shorthand untuk grouping dengan specificity diambil dari yang paling spesifik:
        
        CSS
        
        ```
        /* Sama seperti: h1 a, h2 a, h3 a */
        :is(h1, h2, h3) a { color: var(--color-primary); }
        ```
        
    - _Langkah konkret_: Gunakan `:not(:last-child)` untuk border di navigation items
33. `[[33. Pseudo-class Form — :focus, :valid, :invalid, :required]]`
    
    - Styling formulir berdasarkan state:
        
        CSS
        
        ```
        /* Input default */
        input, textarea, select {
          border: 2px solid var(--color-border);
          border-radius: var(--radius-md);
          padding: var(--space-3) var(--space-4);
          width: 100%;
          font-family: inherit;
          font-size: var(--text-base);
          transition: border-color 0.2s ease;
        }
        
        /* Saat diklik/difokus */
        input:focus, textarea:focus, select:focus {
          outline: none;
          border-color: var(--color-primary);
          box-shadow: 0 0 0 3px rgba(30, 64, 175, 0.15);
        }
        
        /* Validasi */
        input:valid { border-color: var(--color-success); }
        input:invalid:not(:placeholder-shown) {
          border-color: var(--color-error);
        }
        
        /* Required indicator */
        label:has(+ input:required)::after {
          content: " *";
          color: var(--color-error);
        }
        ```
        
    - _Langkah konkret_: Styling semua elemen form di halaman kontak

---

### I. Pseudo-Element — Konten Virtual

> 💡 **Benang Merah ke Pseudo-class**: Pseudo-class menyebut **state** elemen. Pseudo-element menyebut **bagian** elemen yang tidak ada di HTML — sebelum konten, sesudah konten, huruf pertama, dll.

34. `[[34. ::before & ::after — Konten yang Tidak Ada di HTML]]`
    
    - `::before` dan `::after` membutuhkan `content` property (walaupun kosong):
        
        CSS
        
        ```
        /* Dekorasi judul section */
        .section-title::before {
          content: "";
          display: block;
          width: 48px;
          height: 4px;
          background-color: var(--color-primary);
          margin-bottom: var(--space-4);
          border-radius: var(--radius-full);
        }
        
        /* Tanda kutip pada blockquote */
        blockquote::before {
          content: "\201C"; /* tanda kutip buka */
          font-size: 4rem;
          color: var(--color-primary);
          line-height: 0;
          vertical-align: -0.4em;
        }
        
        /* Badge "Baru" pada card */
        .card.is-new::after {
          content: "Baru";
          position: absolute;
          top: var(--space-3);
          right: var(--space-3);
          background-color: var(--color-success);
          color: white;
          padding: var(--space-1) var(--space-3);
          border-radius: var(--radius-full);
          font-size: var(--text-xs);
        }
        ```
        
    - _Langkah konkret_: Tambahkan dekorasi garis berwarna sebelum setiap section title
35. `[[35. ::placeholder, ::selection & ::first-letter]]`
    
    - Style placeholder text di input:
        
        CSS
        
        ```
        input::placeholder {
          color: var(--color-text-muted);
          font-style: italic;
        }
        ```
        
    - Style teks yang diseleksi user:
        
        CSS
        
        ```
        ::selection {
          background-color: var(--color-primary);
          color: white;
        }
        ```
        
    - Dropcap untuk artikel:
        
        CSS
        
        ```
        .article-content > p:first-child::first-letter {
          font-size: 3.5rem;
          font-weight: var(--font-weight-bold);
          color: var(--color-primary);
          float: left;
          line-height: 1;
          margin-right: var(--space-2);
        }
        ```
        
    - _Langkah konkret_: Tambahkan custom selection color ke seluruh website

---

### 🏗️ Checkpoint Level 3

text

```
✅ Checklist sebelum lanjut ke Level 4:

INTERAKSI:
├── .btn: hover (background gelap), active (transform), focus-visible (outline)
├── a: hover (underline), visited (warna berbeda)
├── .project-card: hover (shadow atau border warna)
├── input/textarea/select: focus (border primary + shadow)
├── input:valid dan input:invalid:not(:placeholder-shown)

STRUCTURAL PSEUDO-CLASS:
├── tr:nth-child(odd) untuk tabel
├── :not(:last-child) untuk item tanpa border terakhir

PSEUDO-ELEMENT:
├── .section-title::before dengan garis dekoratif
├── ::placeholder dengan styling
├── ::selection dengan warna brand

TIPOGRAFI:
├── Google Fonts aktif
├── Skala tipografi (xs sampai 5xl) terdefinisi dan digunakan
├── line-height yang nyaman di body dan heading
└── Web font loading dengan font-display: swap

Commit: feat: add hover effects, form styling, and pseudo-elements
```

---

## 🟠 LEVEL 4: FLEXBOX & CSS GRID (Minggu 6-10)

> **Tema**: _"Dari elemen yang mengalir ke layout dua dimensi yang terkontrol penuh"_  
> **Benang Merah**: Layout dasar (display: block/inline, container) → Flexbox untuk satu dimensi → CSS Grid untuk dua dimensi → Layout website yang sesungguhnya  
> **Output**: Website dengan layout header yang proper, grid proyek, dan footer yang terpusat

---

### J. Flexbox — Layout Satu Dimensi

> 💡 **Mengapa Flexbox dulu sebelum Grid?** Flexbox untuk satu dimensi (baris ATAU kolom). Grid untuk dua dimensi (baris DAN kolom). Navigasi, card row, centering — semua Flexbox. Layout halaman keseluruhan — Grid.

text

```
Benang Merah Bagian J:
Navigation masih menggunakan float atau display: inline (lama) →
Flexbox: parent mengontrol children →
flex-direction: baris atau kolom →
justify-content: distribusi di sumbu utama →
align-items: penyelarasan di sumbu silang →
gap: jarak antar item →
Refactor navigation dan card row ke Flexbox
```

36. `[[36. Aktivasi Flexbox & Konsep Dasar]]`
    
    - `display: flex` pada parent mengubah semua children langsung menjadi flex items:
        
        CSS
        
        ```
        /* Flex container */
        .main-nav {
          display: flex;
          align-items: center;
          justify-content: space-between;
          padding: var(--space-4) var(--space-6);
        }
        
        /* Flex items (children langsung) mengikuti aturan flex */
        .nav-links {
          display: flex;
          gap: var(--space-6);
          list-style: none;
          padding: 0;
          margin: 0;
        }
        ```
        
    - _Langkah konkret_: Refactor navigation header menggunakan Flexbox
37. `[[37. flex-direction & flex-wrap]]`
    
    - `flex-direction`:
        - `row` (default): item berjajar horizontal (kiri ke kanan)
        - `column`: item berjajar vertikal (atas ke bawah)
        - `row-reverse` / `column-reverse`: urutan terbalik
    - `flex-wrap`:
        - `nowrap` (default): semua item di satu baris, bisa overflow
        - `wrap`: item pindah ke baris berikutnya jika tidak muat
    - _Langkah konkret_: Gunakan `flex-direction: column` di mobile (nanti diterapkan di Level 5 dengan media queries)
38. `[[38. justify-content — Distribusi di Sumbu Utama]]`
    
    - Mengatur distribusi ruang di arah utama (horizontal jika `row`, vertikal jika `column`):
        
        CSS
        
        ```
        .card-row {
          display: flex;
          /* Pilih salah satu: */
          justify-content: flex-start;    /* semua ke kiri */
          justify-content: flex-end;      /* semua ke kanan */
          justify-content: center;        /* semua ke tengah */
          justify-content: space-between; /* pertama di kiri, terakhir di kanan, sisa dibagi rata */
          justify-content: space-around;  /* ruang yang sama di setiap sisi item */
          justify-content: space-evenly;  /* ruang yang benar-benar sama di semua celah */
        }
        ```
        
    - _Langkah konkret_: Logo di kiri, nav di kanan menggunakan `justify-content: space-between`
39. `[[39. align-items & gap — Penyelarasan & Jarak]]`
    
    - `align-items`: penyelarasan di sumbu **silang** (vertikal jika `row`):
        
        CSS
        
        ```
        .main-nav {
          display: flex;
          align-items: center; /* semua item rata tengah secara vertikal */
          gap: var(--space-4); /* jarak antar item */
        }
        ```
        
    - `gap`: pengganti margin antar flex items — lebih bersih:
        
        CSS
        
        ```
        /* Sebelum gap ada (lama): */
        .nav-item + .nav-item { margin-left: var(--space-6); }
        
        /* Sekarang (modern): */
        .nav-links { gap: var(--space-6); }
        ```
        
    - _Langkah konkret_: Gunakan `gap` di semua flex dan grid container
40. `[[40. flex-grow, flex-shrink & flex-basis]]`
    
    - `flex-grow`: seberapa besar item tumbuh mengisi ruang kosong
    - `flex-shrink`: seberapa besar item menyusut jika tidak cukup ruang
    - `flex-basis`: ukuran awal sebelum growing/shrinking
    - Shorthand `flex`: `flex: grow shrink basis`
    - Nilai yang sering digunakan:
        
        CSS
        
        ```
        .item { flex: 1; }          /* grow: 1, shrink: 1, basis: 0 — bagian rata */
        .item { flex: auto; }       /* grow: 1, shrink: 1, basis: auto */
        .item { flex: none; }       /* grow: 0, shrink: 0, basis: auto — ukuran tetap */
        .sidebar { flex: 0 0 250px; } /* lebar tetap 250px, tidak grow/shrink */
        .main { flex: 1; }           /* mengisi sisa ruang */
        ```
        
    - _Langkah konkret_: Buat 2-column layout: sidebar fixed 250px, konten mengisi sisa
41. `[[41. Centering dengan Flexbox — Teknik yang Paling Berguna]]`
    
    - Center secara horizontal dan vertikal:
        
        CSS
        
        ```
        /* Center apapun di dalam container */
        .hero {
          display: flex;
          justify-content: center; /* horizontal */
          align-items: center;     /* vertikal */
          min-height: 100vh;       /* tinggi minimal viewport */
          text-align: center;
        }
        
        /* Card dengan isi yang selalu di bawah */
        .card {
          display: flex;
          flex-direction: column;
        }
        
        .card-content {
          flex: 1; /* isi tumbuh mengisi ruang */
        }
        
        .card-footer {
          margin-top: auto; /* dorong ke bawah */
        }
        ```
        
    - _Langkah konkret_: Hero section terpusat dengan Flexbox, card footer selalu di bawah

---

### K. CSS Grid — Layout Dua Dimensi

> 💡 **Benang Merah ke Flexbox**: Flexbox mengontrol satu dimensi. CSS Grid mengontrol **dua dimensi sekaligus** (baris dan kolom). Grid untuk layout halaman keseluruhan dan galeri.

text

```
Benang Merah Bagian K:
Flexbox untuk satu baris/kolom (Poin 36-41) →
Grid untuk layout dua dimensi →
grid-template-columns: definisikan kolom →
fr unit: distribusi proporsional →
grid-template-areas: layout visual yang intuitif →
auto-fill/auto-fit: grid responsif otomatis
```

42. `[[42. Aktivasi Grid & Mendefinisikan Kolom]]`
    
    - `display: grid` pada parent, lalu definisikan kolom:
        
        CSS
        
        ```
        /* Grid 3 kolom sama lebar */
        .projects-grid {
          display: grid;
          grid-template-columns: 1fr 1fr 1fr;
          /* Sama dengan: */
          grid-template-columns: repeat(3, 1fr);
          gap: var(--space-6);
        }
        
        /* Grid mixed: sidebar + konten */
        .page-layout {
          display: grid;
          grid-template-columns: 250px 1fr;
          gap: var(--space-8);
        }
        ```
        
    - `fr` (fraction unit): bagian dari ruang yang tersisa — `1fr 2fr 1fr` = 25% 50% 25%
    - _Langkah konkret_: Ubah daftar proyek dari `<ul>` ke CSS Grid 3 kolom
43. `[[43. gap, minmax & auto-fill/auto-fit — Grid yang Fleksibel]]`
    
    - `minmax()`: kolom punya ukuran minimum dan maksimum:
        
        CSS
        
        ```
        /* Kolom minimal 280px, maksimal mengisi sisa */
        .projects-grid {
          grid-template-columns: repeat(3, minmax(280px, 1fr));
        }
        ```
        
    - `auto-fill` vs `auto-fit`: grid yang menyesuaikan otomatis:
        
        CSS
        
        ```
        /* MAGIC: grid responsif tanpa media query! */
        .projects-grid {
          grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
          /* auto-fill: buat kolom sebanyak mungkin */
          /* auto-fit: serupa tapi kolom kosong di-collapse */
          gap: var(--space-6);
        }
        ```
        
    - _Langkah konkret_: Ubah grid proyek ke `auto-fill` — responsif otomatis tanpa media query
44. `[[44. grid-template-areas — Layout Visual yang Intuitif]]`
    
    - Definisikan layout menggunakan nama area:
        
        CSS
        
        ```
        .page-layout {
          display: grid;
          grid-template-columns: 1fr 3fr;
          grid-template-rows: auto 1fr auto;
          grid-template-areas:
            "header  header"
            "sidebar main"
            "footer  footer";
          min-height: 100vh;
          gap: var(--space-6);
        }
        
        /* Tempatkan elemen di area */
        header  { grid-area: header; }
        .sidebar { grid-area: sidebar; }
        main    { grid-area: main; }
        footer  { grid-area: footer; }
        ```
        
    - _Langkah konkret_: Buat layout halaman portofolio dengan grid-template-areas
45. `[[45. Grid Item Placement — span & Penempatan Eksplisit]]`
    
    - Buat item mencakup beberapa kolom/baris:
        
        CSS
        
        ```
        /* Item pertama: lebih lebar */
        .project-card:first-child {
          grid-column: span 2; /* mencakup 2 kolom */
        }
        
        /* Penempatan eksplisit */
        .featured-project {
          grid-column: 1 / 3;  /* dari line 1 ke line 3 */
          grid-row: 1 / 2;
        }
        
        /* Shorthand: row-start / column-start / row-end / column-end */
        .hero-image {
          grid-area: 1 / 2 / 3 / 4;
        }
        ```
        
    - _Langkah konkret_: Proyek pertama di portofolio lebih besar (span 2 kolom)
46. `[[46. Kapan Flexbox vs Grid — Aturan Praktis]]`
    
    - **Gunakan Flexbox untuk**:
        - Navigasi (satu baris)
        - Tombol dengan ikon
        - Card content (stack vertikal)
        - Centering elemen
        - Komponen kecil
    - **Gunakan Grid untuk**:
        - Layout halaman keseluruhan
        - Galeri gambar
        - Dashboard
        - Layout yang butuh alignment di baris DAN kolom sekaligus
    - Kombinasi: Grid untuk layout macro, Flexbox untuk komponen micro di dalam grid
    - _Langkah konkret_: Audit semua layout — gunakan Flexbox atau Grid yang tepat

---

### L. Positioning — Elemen di Luar Aliran Normal

> 💡 **Benang Merah ke Layout**: Flexbox dan Grid untuk layout di dalam aliran normal. Positioning untuk elemen yang perlu berada di posisi spesifik relatif terhadap sesuatu — dropdown, tooltip, modal, sticky header.

47. `[[47. position: relative & absolute — Pasangan yang Selalu Bersama]]`
    
    - `relative`: elemen tetap di aliran normal tapi bisa di-offset, dan menjadi **containing block** untuk absolute anak-anaknya
    - `absolute`: elemen keluar dari aliran normal, posisi relatif ke **ancestor terdekat yang punya position selain static**
    - Pola yang sangat umum:
        
        CSS
        
        ```
        /* Parent sebagai anchor untuk absolute child */
        .card {
          position: relative; /* ← wajib */
        }
        
        /* Badge di sudut card */
        .card .badge {
          position: absolute;
          top: var(--space-3);
          right: var(--space-3);
        }
        ```
        
    - _Langkah konkret_: Tambahkan badge "Featured" di sudut project card tertentu
48. `[[48. position: fixed & sticky — Elemen yang Selalu Terlihat]]`
    
    - `fixed`: relatif ke viewport — tidak bergerak saat scroll:
        
        CSS
        
        ```
        /* Header yang menempel di atas saat scroll */
        header {
          position: fixed;
          top: 0;
          left: 0;
          right: 0;
          z-index: 100;
          background-color: white;
          box-shadow: 0 1px 3px rgba(0,0,0,0.1);
        }
        
        /* Kompensasi tinggi header di body */
        body {
          padding-top: 64px; /* sesuaikan dengan tinggi header */
        }
        ```
        
    - `sticky`: gabungan relative dan fixed — menempel saat melewati posisi tertentu:
        
        CSS
        
        ```
        .section-nav {
          position: sticky;
          top: var(--space-4); /* menempel di 16px dari atas viewport */
        }
        ```
        
    - _Langkah konkret_: Buat header sticky menggunakan `position: fixed` atau `sticky`
49. `[[49. z-index & Stacking Context — Siapa di Atas Siapa]]`
    
    - `z-index` hanya bekerja pada elemen yang punya `position` (bukan `static`)
    - Stacking context: elemen dengan z-index menciptakan "ruang" tersendiri
    - Strategi z-index yang jelas:
        
        CSS
        
        ```
        :root {
          --z-behind:   -1;
          --z-normal:    0;
          --z-elevated:  10;
          --z-sticky:    100;
          --z-overlay:   200;
          --z-modal:     300;
          --z-toast:     400;
        }
        
        header { z-index: var(--z-sticky); }
        .modal-overlay { z-index: var(--z-overlay); }
        .modal { z-index: var(--z-modal); }
        .toast { z-index: var(--z-toast); }
        ```
        
    - _Langkah konkret_: Tambahkan z-index variables ke `:root`, terapkan ke header dan elemen yang bertumpuk

---

### 🏗️ Checkpoint Level 4

text

```
✅ Checklist sebelum lanjut ke Level 5:

FLEXBOX:
├── Header: logo kiri, nav kanan (space-between)
├── Nav links: horizontal, gap
├── Hero section: centered (justify + align center)
├── Card: flex-direction: column, footer sticky di bawah

CSS GRID:
├── .projects-grid: auto-fill dengan minmax — responsif otomatis
├── Project pertama: grid-column: span 2 (featured)
├── Page layout: grid-template-areas untuk sidebar + main

POSITIONING:
├── .card badge: position absolute + relative parent
├── Header: position fixed atau sticky dengan z-index
├── Z-index variables di :root dan digunakan konsisten

VISUAL CHECK:
├── Header menempel saat scroll
├── Proyek tampil dalam grid yang rapi
├── Card memiliki spacing yang konsisten
└── Hover state di semua elemen interaktif

Commit: feat: implement Flexbox navigation, CSS Grid layout, and positioning
```

---

## 🔴 LEVEL 5: RESPONSIVE DESIGN, ANIMASI & TRANSFORM (Minggu 10-15)

> **Tema**: _"Website yang bekerja di semua ukuran layar, dengan gerakan yang smooth"_  
> **Benang Merah**: Layout desktop bagus (Level 4) → tambahkan responsivitas → animasi untuk feedback → transform untuk efek visual  
> **Output**: Website fully responsive dengan animasi hover yang smooth dan transisi halaman

---

### M. Responsive Web Design

> 💡 **Benang Merah ke Grid**: Grid sudah `auto-fill` yang responsif otomatis. Tapi ada banyak hal lain yang perlu disesuaikan di mobile: navigasi jadi hamburger, font lebih kecil, padding berkurang.

text

```
Benang Merah Bagian M:
Grid auto-fill sudah responsif (Poin 43) →
Tapi nav, typography, spacing masih perlu mobile adjustment →
Mobile-first: mulai dari mobile, tambahkan layout desktop →
Media queries: kondisi berdasarkan ukuran layar →
Breakpoints: titik di mana layout berubah →
clamp(): nilai yang otomatis skalabel
```

50. `[[50. Mobile-First & Media Queries]]`
    
    - **Mobile-first**: tulis CSS untuk mobile dulu, override untuk desktop:
        
        CSS
        
        ```
        /* Mobile: default (tanpa media query) */
        .projects-grid {
          display: grid;
          grid-template-columns: 1fr; /* 1 kolom di mobile */
          gap: var(--space-4);
        }
        
        /* Tablet: 768px ke atas */
        @media (min-width: 768px) {
          .projects-grid {
            grid-template-columns: repeat(2, 1fr); /* 2 kolom */
          }
        }
        
        /* Desktop: 1024px ke atas */
        @media (min-width: 1024px) {
          .projects-grid {
            grid-template-columns: repeat(3, 1fr); /* 3 kolom */
            gap: var(--space-6);
          }
        }
        ```
        
    - Definisikan breakpoints sebagai CSS Variables (tidak bisa dipakai di media query, tapi sebagai referensi):
        
        CSS
        
        ```
        /* Di komentar atau dokumentasi: */
        /* --breakpoint-sm:  640px  (smartphone landscape) */
        /* --breakpoint-md:  768px  (tablet portrait) */
        /* --breakpoint-lg:  1024px (tablet landscape / laptop) */
        /* --breakpoint-xl:  1280px (desktop) */
        /* --breakpoint-2xl: 1536px (large desktop) */
        ```
        
    - _Langkah konkret_: Refactor semua layout dari desktop-first ke mobile-first
51. `[[51. Responsive Navigation — Menu Hamburger Tanpa JavaScript Murni CSS]]`
    
    - Menu hamburger menggunakan checkbox trick:
        
        CSS
        
        ```
        /* Toggle button */
        .nav-toggle {
          display: none; /* di desktop */
        }
        
        .hamburger {
          display: none;
          flex-direction: column;
          gap: 5px;
          cursor: pointer;
          padding: var(--space-2);
        }
        
        .hamburger span {
          display: block;
          width: 24px;
          height: 2px;
          background-color: currentColor;
          transition: transform 0.3s ease;
        }
        
        @media (max-width: 767px) {
          .hamburger { display: flex; }
          
          .nav-links {
            display: none; /* tersembunyi di mobile */
            flex-direction: column;
            position: absolute;
            top: 100%;
            left: 0;
            right: 0;
            background-color: white;
            padding: var(--space-4);
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
          }
          
          /* Tampilkan saat checkbox dicentang */
          .nav-toggle:checked ~ .nav-links {
            display: flex;
          }
        }
        ```
        
    - _Langkah konkret_: Navigasi collapse di mobile menjadi hamburger menu
52. `[[52. Fluid Typography dengan clamp()]]`
    
    - `clamp(min, preferred, max)`: nilai yang skalabel tanpa media query:
        
        CSS
        
        ```
        :root {
          /* Font size yang otomatis skalabel antara mobile dan desktop */
          --text-fluid-sm:  clamp(0.875rem, 0.8rem + 0.375vw, 1rem);
          --text-fluid-base: clamp(1rem, 0.9rem + 0.5vw, 1.125rem);
          --text-fluid-lg:  clamp(1.125rem, 1rem + 0.625vw, 1.25rem);
          --text-fluid-xl:  clamp(1.25rem, 1rem + 1.25vw, 1.5rem);
          --text-fluid-2xl: clamp(1.5rem, 1.1rem + 2vw, 2rem);
          --text-fluid-3xl: clamp(1.875rem, 1.2rem + 3.375vw, 2.5rem);
          --text-fluid-4xl: clamp(2.25rem, 1.5rem + 3.75vw, 3rem);
          --text-fluid-5xl: clamp(3rem, 2rem + 5vw, 4rem);
          
          /* Fluid spacing */
          --space-fluid-sm: clamp(var(--space-4), 4vw, var(--space-8));
          --space-fluid-lg: clamp(var(--space-8), 8vw, var(--space-16));
        }
        
        h1 { font-size: var(--text-fluid-5xl); }
        h2 { font-size: var(--text-fluid-3xl); }
        ```
        
    - _Langkah konkret_: Ganti font-size heading dengan fluid variables — resize browser dan lihat scaling halus
53. `[[53. Media Feature Modern — prefers-color-scheme & prefers-reduced-motion]]`
    
    - Dark mode berdasarkan preferensi OS:
        
        CSS
        
        ```
        /* Light mode (default) */
        :root {
          --color-background: #ffffff;
          --color-text: #111827;
          --color-surface: #f9fafb;
        }
        
        /* Dark mode: otomatis jika OS menggunakan dark mode */
        @media (prefers-color-scheme: dark) {
          :root {
            --color-background: #111827;
            --color-text: #f9fafb;
            --color-surface: #1f2937;
            --color-border: #374151;
          }
        }
        ```
        
    - Hormati preferensi reduced motion:
        
        CSS
        
        ```
        @media (prefers-reduced-motion: reduce) {
          *, *::before, *::after {
            animation-duration: 0.01ms !important;
            animation-iteration-count: 1 !important;
            transition-duration: 0.01ms !important;
          }
        }
        ```
        
    - _Langkah konkret_: Test dark mode — aktifkan dark mode di OS, verifikasi website ikut berubah

---

### N. Transisi & Animasi — Gerakan yang Smooth

> 💡 **Benang Merah ke Pseudo-class**: Di Poin 30, kita tambahkan hover tapi perubahan warna masih tiba-tiba. `transition` mengubah perubahan tiba-tiba menjadi animasi yang halus.

54. `[[54. transition — Perubahan yang Halus]]`
    
    - Sintaks: `transition: property duration timing-function delay`:
        
        CSS
        
        ```
        /* Variabel transisi untuk konsistensi */
        :root {
          --transition-fast:   150ms ease;
          --transition-normal: 200ms ease;
          --transition-slow:   300ms ease;
          --ease-in-out-back:  cubic-bezier(0.34, 1.56, 0.64, 1);
        }
        
        /* Terapkan ke elemen interaktif */
        .btn {
          background-color: var(--color-primary);
          transition:
            background-color var(--transition-normal),
            transform var(--transition-fast),
            box-shadow var(--transition-normal);
        }
        
        .btn:hover {
          background-color: var(--color-primary-dark);
          box-shadow: 0 4px 12px rgba(30, 64, 175, 0.3);
        }
        
        .btn:active {
          transform: translateY(1px);
        }
        
        /* Card hover */
        .project-card {
          transition: transform var(--transition-normal), box-shadow var(--transition-normal);
        }
        
        .project-card:hover {
          transform: translateY(-4px);
          box-shadow: 0 12px 24px rgba(0, 0, 0, 0.1);
        }
        ```
        
    - _Langkah konkret_: Tambahkan transition ke semua elemen interaktif — tombol, card, link, input
55. `[[55. Animasi CSS dengan @keyframes]]`
    
    - Untuk animasi yang lebih kompleks dan berulang:
        
        CSS
        
        ```
        /* Loading spinner */
        @keyframes spin {
          from { transform: rotate(0deg); }
          to   { transform: rotate(360deg); }
        }
        
        /* Fade in dari bawah */
        @keyframes fadeInUp {
          from {
            opacity: 0;
            transform: translateY(20px);
          }
          to {
            opacity: 1;
            transform: translateY(0);
          }
        }
        
        /* Pulse untuk menarik perhatian */
        @keyframes pulse {
          0%, 100% { transform: scale(1); }
          50% { transform: scale(1.05); }
        }
        
        /* Implementasi: */
        .loading-spinner {
          width: 40px;
          height: 40px;
          border: 3px solid var(--color-border);
          border-top-color: var(--color-primary);
          border-radius: 50%;
          animation: spin 0.8s linear infinite;
        }
        
        .hero-content {
          animation: fadeInUp 0.6s ease-out;
        }
        
        .cta-button {
          animation: pulse 2s ease-in-out infinite;
        }
        ```
        
    - `animation-fill-mode: forwards`: pertahankan state akhir setelah animasi selesai
    - _Langkah konkret_: Tambahkan fadeInUp ke hero section, pulse ke tombol CTA

---

### O. Transform — Ubah Visual Tanpa Menggeser Layout

> 💡 **Benang Merah ke Transisi**: Transform **tidak menggeser elemen lain** — elemen masih menempati space aslinya tapi terlihat di posisi/skala/rotasi yang berbeda. Sangat ideal untuk animasi.

56. `[[56. translate, scale & rotate — Transformasi Paling Umum]]`
    
    - `translate`: memindahkan elemen:
        
        CSS
        
        ```
        .card:hover { transform: translateY(-4px); }
        .btn:active { transform: translateY(1px); }
        .slide-in    { transform: translateX(-100%); }
        ```
        
    - `scale`: memperbesar/memperkecil:
        
        CSS
        
        ```
        .img-zoom:hover { transform: scale(1.05); }
        .icon:hover     { transform: scale(1.2); }
        ```
        
    - `rotate`: memutar:
        
        CSS
        
        ```
        .arrow.is-open { transform: rotate(180deg); }
        .logo:hover    { transform: rotate(360deg); }
        ```
        
    - Kombinasi — urutan penting!:
        
        CSS
        
        ```
        /* Hover card: angkat DAN sedikit perbesar */
        .card:hover {
          transform: translateY(-4px) scale(1.02);
        }
        ```
        
    - _Langkah konkret_: Terapkan transform ke card hover dan icon di navigation
57. `[[57. transform-origin & Efek 3D Dasar]]`
    
    - `transform-origin`: titik acuan transformasi (default: `center center`):
        
        CSS
        
        ```
        /* Scale dari pojok kiri atas */
        .menu-item::before {
          transform-origin: left center;
          transform: scaleX(0);
          transition: transform var(--transition-normal);
        }
        
        .menu-item:hover::before {
          transform: scaleX(1);
        }
        ```
        
    - Flip card effect (menggabungkan 3D):
        
        CSS
        
        ```
        .flip-card {
          perspective: 1000px;
        }
        
        .flip-card-inner {
          position: relative;
          transition: transform 0.6s;
          transform-style: preserve-3d;
        }
        
        .flip-card:hover .flip-card-inner {
          transform: rotateY(180deg);
        }
        
        .flip-card-front,
        .flip-card-back {
          position: absolute;
          backface-visibility: hidden;
        }
        
        .flip-card-back {
          transform: rotateY(180deg);
        }
        ```
        
    - _Langkah konkret_: Buat satu skill card dengan efek flip

---

### 🏗️ Checkpoint Level 5

text

```
✅ Checklist sebelum lanjut ke Level 6:

RESPONSIVE:
├── Mobile-first: layout 1 kolom di mobile, lebih banyak di desktop
├── Navigation hamburger di mobile
├── Fluid typography dengan clamp()
├── Fluid spacing dengan clamp()
├── Dark mode otomatis dengan prefers-color-scheme
├── prefers-reduced-motion: animasi dimatikan jika user prefer

TRANSISI:
├── Semua tombol: hover + active + focus transition
├── Semua card: hover lift effect (translateY + shadow)
├── Semua input: focus border transition
├── --transition-fast, --transition-normal, --transition-slow digunakan konsisten

ANIMASI:
├── Hero section: fadeInUp animation
├── Loading spinner menggunakan @keyframes spin
├── Minimal satu animasi berulang yang relevan

TRANSFORM:
├── Card hover: translateY + scale
├── Satu flip card sebagai demo 3D transform

TEST:
├── Buka di Chrome Mobile Simulation
├── Test di ukuran: 375px, 768px, 1024px, 1280px
├── Test dark mode di OS settings
├── Test dengan prefers-reduced-motion aktif

Commit: feat: add responsive design, animations, and transform effects
```

---

## ⚫ LEVEL 6: CSS LANJUTAN & ARSITEKTUR (Minggu 15-20)

> **Tema**: _"CSS yang efisien, maintainable, dan menggunakan fitur-fitur modern"_  
> **Benang Merah**: CSS sudah besar dan mungkin mulai berantakan (Level 1-5) → CSS Variables lanjutan → fitur modern → metodologi BEM → arsitektur yang scalable  
> **Output**: CSS yang terorganisir, konsisten, dan siap untuk project tim

---

### P. CSS Custom Properties Lanjutan

> 💡 **Benang Merah ke Variables Awal**: CSS Variables sudah digunakan sejak Level 1. Sekarang kita manfaatkan kemampuan dinamisnya — ubah di JavaScript, gunakan untuk theming, buat design token system yang lengkap.

58. `[[58. Design Token System — Variabel yang Terstruktur]]`
    
    - Organisasi variables yang lebih sistematis:
        
        CSS
        
        ```
        :root {
          /* ===== PRIMITIVE TOKENS ===== */
          /* Warna dasar (jangan digunakan langsung di komponen) */
          --blue-50:  #eff6ff;
          --blue-100: #dbeafe;
          --blue-500: #3b82f6;
          --blue-600: #2563eb;
          --blue-700: #1d4ed8;
          --blue-900: #1e3a8a;
          
          /* ===== SEMANTIC TOKENS ===== */
          /* Gunakan ini di komponen */
          --color-brand-primary:    var(--blue-600);
          --color-brand-primary-hover: var(--blue-700);
          --color-brand-on-primary: white;
          
          --color-surface:    white;
          --color-surface-alt: var(--blue-50);
          --color-on-surface: #111827;
          
          --color-interactive:       var(--blue-600);
          --color-interactive-hover: var(--blue-700);
          
          /* ===== COMPONENT TOKENS ===== */
          /* Khusus untuk komponen tertentu */
          --button-padding:     var(--space-3) var(--space-6);
          --button-radius:      var(--radius-md);
          --button-font-weight: var(--font-weight-medium);
          --card-padding:       var(--space-6);
          --card-radius:        var(--radius-lg);
          --card-shadow:        0 1px 3px rgba(0,0,0,0.1);
        }
        ```
        
    - _Langkah konkret_: Reorganisasi semua CSS Variables menggunakan pola design token
59. `[[59. Dark Mode dengan CSS Variables — Satu Toggle, Semua Berubah]]`
    
    - Toggle dark mode via JavaScript:
        
        CSS
        
        ```
        /* Light mode (default) */
        :root {
          --color-bg:      #ffffff;
          --color-text:    #111827;
          --color-surface: #f9fafb;
          --color-border:  #e5e7eb;
        }
        
        /* Dark mode: ubah hanya variable, semua komponen ikut */
        [data-theme="dark"] {
          --color-bg:      #0f172a;
          --color-text:    #f8fafc;
          --color-surface: #1e293b;
          --color-border:  #334155;
        }
        
        /* Auto dark mode dari OS */
        @media (prefers-color-scheme: dark) {
          :root:not([data-theme="light"]) {
            --color-bg:      #0f172a;
            --color-text:    #f8fafc;
            --color-surface: #1e293b;
            --color-border:  #334155;
          }
        }
        ```
        
    - JavaScript toggle:
        
        JavaScript
        
        ```
        const toggle = document.getElementById('dark-mode-toggle');
        toggle.addEventListener('click', () => {
          const isDark = document.documentElement.dataset.theme === 'dark';
          document.documentElement.dataset.theme = isDark ? 'light' : 'dark';
          localStorage.setItem('theme', isDark ? 'light' : 'dark');
        });
        
        // Restore dari localStorage saat load
        const savedTheme = localStorage.getItem('theme');
        if (savedTheme) {
          document.documentElement.dataset.theme = savedTheme;
        }
        ```
        
    - _Langkah konkret_: Implementasikan dark mode toggle di header
60. `[[60. Fitur CSS Modern — aspect-ratio, object-fit & scroll-behavior]]`
    
    - `aspect-ratio`: pertahankan rasio aspek tanpa padding hack:
        
        CSS
        
        ```
        /* Gambar selalu 16:9 */
        .video-wrapper {
          aspect-ratio: 16 / 9;
          width: 100%;
        }
        
        /* Avatar selalu kotak */
        .avatar {
          aspect-ratio: 1;
          width: 64px;
          border-radius: 50%;
        }
        ```
        
    - `object-fit`: bagaimana gambar mengisi kontainernya:
        
        CSS
        
        ```
        .card-image {
          width: 100%;
          height: 200px;
          object-fit: cover;      /* cover: isi, crop jika perlu */
          object-position: center top; /* posisi gambar */
        }
        ```
        
    - `scroll-behavior: smooth`:
        
        CSS
        
        ```
        html { scroll-behavior: smooth; }
        /* Anchor link sekarang scroll halus */
        ```
        
    - _Langkah konkret_: Terapkan `aspect-ratio: 16/9` ke video embed, `object-fit: cover` ke semua card image
61. `[[61. filter & backdrop-filter — Efek Visual Canggih]]`
    
    - `filter`: efek visual pada elemen:
        
        CSS
        
        ```
        /* Avatar grayscale, berwarna saat hover */
        .team-photo {
          filter: grayscale(100%);
          transition: filter var(--transition-normal);
        }
        
        .team-photo:hover { filter: grayscale(0%); }
        
        /* Blur di belakang elemen */
        .card-overlay {
          filter: blur(4px);
        }
        ```
        
    - `backdrop-filter`: filter pada area di **belakang** elemen — efek glass morphism:
        
        CSS
        
        ```
        .glass-card {
          background-color: rgba(255, 255, 255, 0.1);
          backdrop-filter: blur(12px);
          border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        /* Header dengan glass effect */
        header {
          background-color: rgba(255, 255, 255, 0.8);
          backdrop-filter: blur(10px);
          -webkit-backdrop-filter: blur(10px); /* Safari */
        }
        ```
        
    - _Langkah konkret_: Tambahkan glass effect ke header saat scroll
62. `[[62. clip-path & mix-blend-mode — Bentuk & Efek Pencampuran]]`
    
    - `clip-path`: potong elemen dalam bentuk tertentu:
        
        CSS
        
        ```
        /* Berbagai bentuk */
        .circle      { clip-path: circle(50%); }
        .triangle    { clip-path: polygon(50% 0%, 0% 100%, 100% 100%); }
        .hexagon     { clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%); }
        .diagonal    { clip-path: polygon(0 0, 100% 0, 100% 85%, 0 100%); }
        ```
        
    - `mix-blend-mode`: cara elemen berbaur dengan yang di belakangnya:
        
        CSS
        
        ```
        /* Teks yang "mengikis" gambar */
        .text-on-image {
          mix-blend-mode: multiply;
        }
        ```
        
    - _Langkah konkret_: Tambahkan `clip-path: polygon(0 0, 100% 0, 100% 85%, 0 100%)` ke hero section untuk efek diagonal

---

### Q. Arsitektur CSS — Kode yang Terorganisir

> 💡 **Benang Merah ke semua level**: CSS sudah besar sekarang — mungkin 400-600 baris. Tanpa struktur, ini akan menjadi berantakan. BEM dan organisasi file membuat CSS yang bisa dipelihara.

63. `[[63. Metodologi BEM — Penamaan Class yang Konsisten]]`
    
    - BEM: **B**lock **E**lement **M**odifier
    - Block: komponen mandiri — `.card`, `.btn`, `.nav`
    - Element: bagian dari block — `.card__title`, `.card__image`, `.btn__icon`
    - Modifier: variasi atau state — `.btn--primary`, `.card--featured`, `.nav--dark`
    - Contoh implementasi:
        
        HTML
        
        ```
        <article class="project-card project-card--featured">
          <figure class="project-card__image-wrapper">
            <img class="project-card__image" src="..." alt="...">
          </figure>
          <div class="project-card__body">
            <h3 class="project-card__title">Nama Proyek</h3>
            <p class="project-card__description">...</p>
            <a class="btn btn--primary project-card__cta" href="#">
              Lihat Proyek
            </a>
          </div>
        </article>
        ```
        
        CSS
        
        ```
        .project-card { /* ... */ }
        .project-card--featured { border: 2px solid var(--color-primary); }
        .project-card__image { width: 100%; }
        .project-card__title { font-size: var(--text-xl); }
        ```
        
    - _Langkah konkret_: Refactor semua class nama ke konvensi BEM
64. `[[64. Organisasi File CSS — Struktur yang Scalable]]`
    
    - Struktur file yang disarankan:
        
        text
        
        ```
        css/
        ├── base/
        │   ├── _reset.css         ← reset dan normalisasi
        │   ├── _variables.css     ← semua CSS custom properties
        │   └── _typography.css    ← base typography styles
        ├── components/
        │   ├── _buttons.css       ← semua varian tombol
        │   ├── _cards.css         ← semua varian card
        │   ├── _forms.css         ← semua elemen form
        │   └── _navigation.css    ← header dan nav
        ├── layout/
        │   ├── _container.css     ← container dan grid utama
        │   ├── _header.css        ← layout header
        │   └── _footer.css        ← layout footer
        ├── pages/
        │   ├── _home.css          ← style spesifik halaman beranda
        │   ├── _portfolio.css     ← style spesifik halaman portofolio
        │   └── _contact.css       ← style spesifik halaman kontak
        └── styles.css             ← import semua file di atas (atau file utama)
        ```
        
    - Menggunakan `@import` atau gabungkan semua dalam satu file
    - _Langkah konkret_: Reorganisasi `styles.css` ke struktur folder ini
65. `[[65. @supports — CSS yang Graceful Degradation]]`
    
    - Gunakan fitur modern hanya jika browser mendukung:
        
        CSS
        
        ```
        /* Semua browser: fallback */
        .hero {
          background-color: var(--color-primary);
        }
        
        /* Browser yang support backdrop-filter: efek kaca */
        @supports (backdrop-filter: blur(10px)) {
          header {
            background-color: rgba(255, 255, 255, 0.8);
            backdrop-filter: blur(10px);
          }
        }
        
        /* Grid dengan fallback ke flex */
        @supports not (display: grid) {
          .projects-grid { display: flex; flex-wrap: wrap; }
          .project-card { width: calc(33.33% - 1rem); }
        }
        ```
        
    - _Langkah konkret_: Tambahkan `@supports` untuk `backdrop-filter` dan `clip-path`

---

### 🏗️ Checkpoint Level 6

text

```
✅ Checklist sebelum lanjut ke Level 7:

CSS VARIABLES:
├── Design token system: primitive → semantic → component
├── Dark mode toggle berfungsi (CSS + JS)
├── Dark mode via prefers-color-scheme juga berfungsi

FITUR MODERN:
├── aspect-ratio di semua gambar dan video embed
├── object-fit: cover di semua card image
├── scroll-behavior: smooth di html
├── backdrop-filter di header (glass effect)
├── clip-path di setidaknya satu section

ARSITEKTUR:
├── BEM naming di semua komponen
├── File CSS terorganisasi dalam folder
├── @supports untuk fitur yang tidak semua browser support

PERFORMA:
├── Tidak ada !important yang tidak diperlukan
├── Tidak ada CSS yang tidak digunakan (audit dengan DevTools Coverage)
├── Selector tidak terlalu panjang (max 3 level nesting)

Commit: feat: implement design tokens, dark mode, and CSS architecture
```

---

## 🟣 LEVEL 7: SASS, FRAMEWORK & PORTFOLIO PUBLISHED (Minggu 20+)

> **Tema**: _"Dari CSS manual ke workflow profesional — dan portfolio yang siap dipublish"_  
> **Benang Merah**: CSS yang terorganisir (Level 6) → SASS untuk preprocessing → Tailwind untuk project berbeda → Audit final → Deploy  
> **Output**: Website portofolio yang published dengan CSS berkualitas production

---

### R. SASS/SCSS — CSS dengan Superkekuatan

> 💡 **Mengapa SASS?** CSS sudah bagus, tapi SASS menambahkan: nesting yang bersih, mixin untuk kode yang reusable, dan kemampuan lain yang sangat berguna di project besar.

66. `[[66. Setup SASS & Nesting]]`
    
    - Install SASS: `npm install -D sass`
    - Ubah ekstensi file dari `.css` ke `.scss`
    - Nesting yang mencerminkan struktur BEM:
        
        SCSS
        
        ```
        .project-card {
          border: 1px solid var(--color-border);
          border-radius: var(--radius-lg);
          overflow: hidden;
          
          /* Element: gunakan & */
          &__image { width: 100%; aspect-ratio: 16/9; }
          
          &__body { padding: var(--space-6); }
          
          &__title {
            font-size: var(--text-xl);
            font-weight: var(--font-weight-bold);
          }
          
          /* Modifier */
          &--featured {
            border-color: var(--color-primary);
            border-width: 2px;
          }
          
          /* State */
          &:hover {
            transform: translateY(-4px);
            box-shadow: var(--shadow-lg);
          }
        }
        ```
        
    - Compile: `sass --watch scss:css` atau via build tool
    - _Langkah konkret_: Convert satu file CSS ke SCSS, gunakan nesting
67. `[[67. SASS Variables, Mixin & Function]]`
    
    - Variables SASS (`$`) vs CSS Variables (`--`): SASS di-compile, CSS Variables runtime:
        
        SCSS
        
        ```
        // SASS variables: di-compile menjadi nilai statis
        $font-primary: 'Inter', sans-serif;
        $breakpoint-md: 768px;
        
        // Mixin: reusable block CSS
        @mixin flex-center {
          display: flex;
          justify-content: center;
          align-items: center;
        }
        
        @mixin responsive($breakpoint) {
          @media (min-width: $breakpoint) {
            @content; // placeholder untuk konten yang akan dimasukkan
          }
        }
        
        // Penggunaan:
        .hero {
          @include flex-center;
          min-height: 100vh;
          
          @include responsive($breakpoint-md) {
            min-height: 80vh;
          }
        }
        ```
        
    - _Langkah konkret_: Buat mixin untuk responsive, flex-center, dan button variant
68. `[[68. SASS Partials & @use — Modularisasi yang Benar]]`
    
    - Partials: file SCSS dengan awalan `_` tidak dikompilasi sendiri:
        
        text
        
        ```
        scss/
        ├── abstracts/
        │   ├── _variables.scss  ← CSS dan SASS variables
        │   ├── _mixins.scss     ← semua mixin
        │   └── _functions.scss  ← fungsi kustom
        ├── base/
        │   ├── _reset.scss
        │   └── _typography.scss
        ├── components/
        │   ├── _buttons.scss
        │   └── _cards.scss
        └── main.scss            ← entry point
        ```
        
    - `@use` (modern, bukan `@import`):
        
        SCSS
        
        ```
        // main.scss
        @use 'abstracts/variables' as v;
        @use 'abstracts/mixins' as m;
        @use 'base/reset';
        @use 'base/typography';
        @use 'components/buttons';
        @use 'components/cards';
        ```
        
    - _Langkah konkret_: Reorganisasi semua SCSS ke struktur partial

---

### S. Pengenalan Tailwind CSS — Utility-First Approach

> 💡 **Mengapa Tailwind?** Approach yang berbeda dari CSS biasa. Sangat populer di industri sekarang. Memahami keduanya (CSS custom dan utility-first) membuat kamu lebih versatile.

69. `[[69. Setup Tailwind & Filosofi Utility-First]]`
    
    - Install di project baru: `npm install -D tailwindcss`
    - `npx tailwindcss init`
    - Buat **project baru** (bukan ganti yang lama) untuk eksperimen Tailwind:
        
        HTML
        
        ```
        <!-- Dengan custom CSS: -->
        <div class="project-card project-card--featured">...</div>
        
        <!-- Dengan Tailwind: semua class utility -->
        <div class="rounded-xl border border-blue-600 border-2 p-6 hover:-translate-y-1 transition-transform">
          ...
        </div>
        ```
        
    - Trade-off Tailwind:
        - ✅ Tidak perlu berpikir nama class, tidak ada CSS yang tidak dipakai (JIT)
        - ❌ HTML lebih panjang, perlu belajar nama utility class
    - _Langkah konkret_: Rebuild halaman beranda dengan Tailwind CSS dari nol
70. `[[70. Tailwind Configuration — Kustomisasi Design System]]`
    
    - Extend atau override default Tailwind theme:
        
        JavaScript
        
        ```
        // tailwind.config.js
        export default {
          content: ['./src/**/*.{html,js,ts,vue}'],
          theme: {
            extend: {
              colors: {
                primary: {
                  50:  '#eff6ff',
                  500: '#3b82f6',
                  600: '#2563eb',
                  700: '#1d4ed8',
                  900: '#1e3a8a',
                }
              },
              fontFamily: {
                sans: ['Inter', 'sans-serif'],
              },
            },
          },
        }
        ```
        
    - _Langkah konkret_: Konfigurasi Tailwind dengan warna brand yang sama dengan proyek CSS custom

---

### T. Audit, Optimasi & Publishing

71. `[[71. Audit Aksesibilitas CSS — Warna & Fokus]]`
    
    - Cek kontras warna: pakai browser extension "Colour Contrast Analyzer" atau WebAIM Contrast Checker
    - WCAG AA: minimal 4.5:1 untuk teks normal, 3:1 untuk teks besar
    - Cek semua teks di atas background:
        
        CSS
        
        ```
        /* Contoh yang perlu dicek: */
        /* Warna primer di background putih: */
        /* var(--color-primary): #2563eb di #ffffff → hitung rasio */
        ```
        
    - Pastikan `:focus-visible` terlihat di semua elemen interaktif
    - _Langkah konkret_: Cek semua kombinasi warna teks/background dengan WebAIM
72. `[[72. Performa CSS — Hanya Load yang Digunakan]]`
    
    - Cek CSS yang tidak terpakai: DevTools → Coverage tab → Record → lihat % unused
    - Strategi mengurangi unused CSS:
        - PurgeCSS (untuk Tailwind sudah built-in via content config)
        - CSS Modules atau scoped styles
        - Pisahkan CSS per halaman
    - Minifikasi untuk production: `sass --style=compressed`
    - _Langkah konkret_: Jalankan Coverage — target: unused CSS < 20%
73. `[[73. Audit Akhir — Lighthouse Score di Semua Halaman]]`
    
    - Jalankan Lighthouse di semua halaman setelah deployment:
        - **Performance** ≥ 90: CSS tidak memblokir render, tidak ada unused CSS besar
        - **Accessibility** ≥ 95: kontras warna, focus visible, alt text
        - **Best Practices** ≥ 90: tidak ada error di console
        - **SEO** ≥ 95: meta tags, semantic HTML
    - _Langkah konkret_: Screenshot semua skor, dokumentasikan di README
74. `[[74. Style Guide — Dokumentasi CSS]]`
    
    - Buat halaman `styleguide.html` yang menampilkan semua komponen:
        
        HTML
        
        ```
        <!DOCTYPE html>
        <html lang="id">
        <head>
          <title>Style Guide — Portfolio</title>
          <link rel="stylesheet" href="styles.css">
        </head>
        <body>
          <h1>Style Guide</h1>
          
          <section>
            <h2>Colors</h2>
            <!-- Tampilkan semua CSS Variables warna -->
          </section>
          
          <section>
            <h2>Typography</h2>
            <!-- Tampilkan semua font size, weight -->
          </section>
          
          <section>
            <h2>Buttons</h2>
            <!-- Tampilkan semua variant tombol -->
          </section>
          
          <section>
            <h2>Cards</h2>
            <!-- Tampilkan semua variant card -->
          </section>
        </body>
        </html>
        ```
        
    - _Langkah konkret_: Style guide selesai sebagai referensi CSS yang komprehensif
75. `[[75. Deploy & Publikasi — Portfolio CSS Selesai]]`
    
    - Final checklist sebelum deploy:
        - Semua file CSS terorganisasi dengan baik
        - Tidak ada selector yang tidak dipakai
        - Tidak ada `!important` yang tidak diperlukan
        - Semua animasi respek `prefers-reduced-motion`
        - Dark mode berfungsi via toggle dan OS preference
        - Responsive dari 320px hingga 1920px
    - Update README dengan Lighthouse scores dan deskripsi CSS approach
    - _Langkah konkret_: Push ke GitHub → Vercel/Netlify rebuild → website live dengan CSS yang polished

---

## 📊 Ringkasan Progress Tracking

### Satu Project, 7 Level Enhancement

text

```
Level 1: Warna brand, tipografi, selector dasar
  + Level 2: + Box Model, spacing system, container, background
  + Level 3: + Hover effects, form styling, pseudo-elements
  + Level 4: + Flexbox nav, CSS Grid layout, positioning
  + Level 5: + Mobile responsive, dark mode, animasi, transform
  + Level 6: + Design tokens, glass effect, BEM, SCSS, file organization
  + Level 7: + SASS preprocessing, Tailwind intro, audit, deploy
```

### Tabel Progress

|Level|Poin|Durasi|Output Konkret|
|---|---|---|---|
|🟢 **1**|1-15|Minggu 1-2|Warna, font, selector dasar|
|🔵 **2**|16-25|Minggu 2-4|Box model, spacing, container, button|
|🟡 **3**|26-35|Minggu 4-6|Hover, form, pseudo-element, cascade|
|🟠 **4**|36-49|Minggu 6-10|Flexbox nav, Grid gallery, positioning|
|🔴 **5**|50-57|Minggu 10-15|Fully responsive, animasi, dark mode|
|⚫ **6**|58-65|Minggu 15-20|Design tokens, BEM, SCSS, file structure|
|🟣 **7**|66-75|Minggu 20+|SASS, Tailwind, audit, portfolio live|

---

### Benang Merah Utama Sepanjang Roadmap

text

```
Poin 2  (External stylesheet)    → Satu file CSS untuk semua halaman
Poin 5  (Reset CSS)              → Fondasi semua layout yang konsisten
Poin 10 (CSS Variables)          → Digunakan di semua poin setelahnya
Poin 18 (Spacing System)         → gap, padding, margin menggunakan var()
Poin 20 (Container)              → Semua section dibungkus .container
Poin 26 (Cascade)                → Memahami mengapa style "tidak mau jalan"
Poin 30 (Hover + transition)     → Semua interaksi menggunakan ini
Poin 36 (Flexbox)                → Navigation dan komponen baris
Poin 42 (CSS Grid)               → Layout galeri dan halaman
Poin 50 (Mobile-first)           → Semua layout direvisi mobile-first
Poin 54 (Transition)             → Diaplikasikan ke hover yang sudah ada (Poin 30)
Poin 58 (Design tokens)          → Evolusi dari CSS Variables (Poin 10)
Poin 59 (Dark mode)              → Menggunakan design tokens (Poin 58)
Poin 63 (BEM)                    → Penamaan untuk SASS nesting (Poin 66)
```

---

## 💡 Cara Menggunakan Roadmap Ini

text

```
Setiap poin mengikuti format:
┌──────────────────────────────────────────────────────┐
│ 💡 Konteks: mengapa properti/teknik ini ada          │
│ 🔗 Benang Merah: koneksi ke poin sebelum/sesudah    │
│ 📋 Kode: implementasi konkret dengan CSS Variables  │
│ ✅ Langkah konkret: cara verifikasi berhasil        │
└──────────────────────────────────────────────────────┘
```

**Aturan yang Tidak Boleh Dilanggar:**

1. **Selalu buka DevTools** — setiap CSS yang ditulis, verifikasi hasilnya di DevTools
2. **Gunakan CSS Variables** dari awal — tidak ada hardcoded color atau spacing
3. **Mobile-first setelah Level 4** — resize browser setelah setiap perubahan layout
4. **Test dark mode** setelah Level 5 — aktifkan dark mode di OS dan verifikasi
5. **Commit setelah setiap checkpoint** — git history adalah progress tracker
6. **Jangan copy-paste tanpa paham** — tulis ulang manual untuk membangun muscle memory

---

_Roadmap CSS v1.0 — Step-by-Step, One Project, Design Token First_  
_Setiap properti ada alasannya — tidak ada CSS yang ditulis karena "coba-coba"_