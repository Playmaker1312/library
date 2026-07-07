# 🗺️ Roadmap Tailwind CSS: Step-by-Step Membangun UI Modern

## Filosofi Roadmap Ini

> **"Tailwind bukan tentang menghafal class — Tailwind adalah cara berpikir tentang design constraints yang konsisten"** — setiap class yang ditulis adalah keputusan desain yang eksplisit, bukan magic number yang tersembunyi di file CSS.

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: SaaS landing page → dashboard → e-commerce — satu ekosistem yang berkembang
- **Play First, Build Later**: setiap konsep diuji di `play.tailwindcss.com` sebelum masuk project
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **Design Thinking**: memahami MENGAPA class tertentu dipilih, bukan sekadar copy-paste

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: Komponen pertama — button, card, typography → Play
    ↓ (mulai project)
Level 2: + Spacing, sizing, flexbox, grid → Layout landing page
    ↓ (enhance)
Level 3: + Background, shadow, transform, animasi → Visual yang menarik
    ↓ (enhance)
Level 4: + Konfigurasi, plugin, dark mode → Brand yang konsisten
    ↓ (enhance)
Level 5: + Component patterns, aksesibilitas, performa → Production-ready
    ↓ (enhance)
Level 6: + Tailwind v4, container queries, design system → Expert
    ↓ (pilih jalur)
Level 7: SaaS dashboard / E-commerce / Design system library
```

---

## 🟢 LEVEL 1: FONDASI — CARA BERPIKIR UTILITY-FIRST (Minggu 1-3)

> **Tema**: _"Mengubah cara pandang dari 'CSS dulu, HTML kemudian' ke 'desain langsung di HTML'"_  
> **Benang Merah**: Filosofi utility-first → Setup → Typography → Warna → Komponen pertama  
> **Output**: Komponen button, card, dan form yang dibangun di Tailwind Play

---

### A. Filosofi & Setup — Memulai dengan Benar

> 💡 **Mengapa filosofi dulu?** Tailwind adalah perubahan paradigma. Jika mulai dengan "belajar class"-nya dulu tanpa paham mengapa, akan selalu merasa Tailwind "tidak bersih". Pahami filosofinya → kode menjadi natural.

text

```
Benang Merah Bagian A:
HTML & CSS sudah dipahami →
Masalah dengan CSS tradisional: naming, specificity, unused CSS →
Tailwind: utility classes yang langsung di HTML →
JIT: hanya CSS yang dipakai yang di-generate →
Setup → Play → Project pertama
```

1. `[[1. Utility-First vs Component-Based — Perubahan Paradigma]]`
    
    - **CSS tradisional**: buat class → tulis CSS → apply ke HTML
    - **Utility-first**: langsung apply atomic utilities ke HTML
    - Masalah CSS tradisional yang diselesaikan Tailwind:
        - "Apa nama class yang tepat untuk ini?" → tidak perlu nama class
        - "CSS ini masih dipakai tidak?" → JIT hanya generate yang dipakai
        - Specificity wars → setiap utility di level yang sama
    - _Langkah konkret_: Buka `play.tailwindcss.com` dan tulis:
        
        HTML
        
        ```
        <!-- CSS tradisional (bayangkan) vs Tailwind -->
        
        <!-- CSS tradisional: butuh class name, butuh file CSS terpisah -->
        <button class="primary-btn">Klik Saya</button>
        
        <!-- Tailwind: desain langsung di HTML -->
        <button class="bg-blue-600 text-white px-6 py-3 rounded-lg font-medium hover:bg-blue-700 transition-colors">
          Klik Saya
        </button>
        ```
        
    - Perhatikan: tidak perlu buka file CSS, tidak perlu beri nama class
2. `[[2. Memahami JIT — Mengapa Tailwind Tidak Membuat File CSS Besar]]`
    
    - JIT (Just-In-Time): scan HTML/JS/template → generate HANYA CSS yang dipakai
    - Tanpa JIT (v2 ke bawah): generate semua kemungkinan → ~3.5MB CSS
    - Dengan JIT (v3+): hanya yang dipakai → biasanya < 50KB
    - Konsekuensi: jangan generate class secara dinamis di JavaScript!
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // ❌ SALAH — Tailwind tidak bisa scan ini:
        const color = 'blue';
        element.className = `bg-${color}-500`; // 'bg-blue-500' tidak akan ada
        
        // ✅ BENAR — class lengkap harus ada di source code:
        const colorMap = {
          blue: 'bg-blue-500',
          red: 'bg-red-500',
          green: 'bg-green-500',
        };
        element.className = colorMap[color]; // class lengkap terdeteksi
        ```
        
3. `[[3. Setup Tailwind Play — Playground Tanpa Install]]`
    
    - Buka `play.tailwindcss.com` — langsung bisa pakai Tailwind
    - Panel kiri: HTML → panel kanan: preview
    - Fitur: autocomplete, hover preview, share URL
    - VS Code extension: **Tailwind CSS IntelliSense** (autocomplete + preview)
    - _Langkah konkret_: Di Play, buat card sederhana dengan teks dan tombol — fokus pada eksplorasi, bukan hafalan
4. `[[4. Setup Project Lokal — Vite + Tailwind (Cara Modern)]]`
    
    - _Langkah konkret_:
        
        Bash
        
        ```
        npm create vite@latest saas-ui -- --template vanilla
        cd saas-ui
        npm install
        npm install -D tailwindcss postcss autoprefixer
        npx tailwindcss init -p
        ```
        
    - Update `tailwind.config.js`:
        
        JavaScript
        
        ```
        /** @type {import('tailwindcss').Config} */
        export default {
          content: [
            './index.html',
            './src/**/*.{js,ts,jsx,tsx,vue,svelte}',
          ],
          theme: {
            extend: {},
          },
          plugins: [],
        }
        ```
        
    - Buat `src/style.css`:
        
        CSS
        
        ```
        @tailwind base;
        @tailwind components;
        @tailwind utilities;
        ```
        
    - Update `main.js`: `import './style.css'`
    - Jalankan: `npm run dev`
    - _Langkah konkret_: Tulis `<h1 class="text-3xl font-bold text-blue-600">Hello Tailwind!</h1>` di `index.html` — harus terlihat styled
5. `[[5. VS Code Setup — IntelliSense & Prettier untuk Produktivitas]]`
    
    - Install extension: **Tailwind CSS IntelliSense** — autocomplete + preview warna + docs hover
    - Install extension: **Prettier** + plugin Prettier Tailwind:
        
        Bash
        
        ```
        npm install -D prettier prettier-plugin-tailwindcss
        ```
        
        JSON
        
        ```
        // .prettierrc
        {
          "plugins": ["prettier-plugin-tailwindcss"],
          "singleQuote": true,
          "semi": true
        }
        ```
        
    - Auto-sort class dengan Prettier Tailwind: class diurutkan konsisten setiap save
    - _Langkah konkret_: Tulis class tidak beraturan → save → class otomatis diurutkan

---

### B. Typography — Teks yang Ekspresif

> 💡 **Mengapa typography dulu?** Typography adalah fondasi semua UI. Setiap website dimulai dari teks. Memahami cara Tailwind menangani typography = memahami design scale.

text

```
Benang Merah Bagian B:
Tailwind siap digunakan (Poin 1-5) →
Font size scale: konsistensi dari xs hingga 9xl →
Font weight & style: hierarki visual →
Letter spacing & line height: keterbacaan →
Text overflow: teks yang tidak meluap
```

6. `[[6. Font Size — Skala yang Konsisten dari xs hingga 9xl]]`
    
    - Tailwind punya scale yang sudah dipikirkan:
        
        HTML
        
        ```
        <div class="space-y-2 p-4">
          <p class="text-xs">text-xs — 12px (caption, fine print)</p>
          <p class="text-sm">text-sm — 14px (label, helper text)</p>
          <p class="text-base">text-base — 16px (body text, default)</p>
          <p class="text-lg">text-lg — 18px (lead paragraph)</p>
          <p class="text-xl">text-xl — 20px (subheading kecil)</p>
          <p class="text-2xl">text-2xl — 24px (subheading)</p>
          <p class="text-3xl">text-3xl — 30px (heading)</p>
          <p class="text-4xl">text-4xl — 36px (heading besar)</p>
          <p class="text-5xl">text-5xl — 48px (hero title)</p>
          <p class="text-6xl">text-6xl — 60px</p>
          <p class="text-7xl">text-7xl — 72px</p>
          <p class="text-8xl">text-8xl — 96px (display)</p>
          <p class="text-9xl">text-9xl — 128px (massive display)</p>
        </div>
        ```
        
    - _Langkah konkret_: Buat hero section dengan `text-5xl` dan body dengan `text-base`
7. `[[7. Font Weight, Style & Tracking — Hierarki Visual]]`
    
    - _Langkah konkret_: Buat card dengan hierarki teks yang jelas:
        
        HTML
        
        ```
        <div class="max-w-sm rounded-xl border border-gray-200 p-6">
          <!-- Badge -->
          <span class="text-xs font-semibold uppercase tracking-widest text-blue-600">
            Fitur Unggulan
          </span>
          
          <!-- Judul: bold, tight tracking -->
          <h2 class="mt-2 text-2xl font-bold tracking-tight text-gray-900">
            Kecepatan yang Luar Biasa
          </h2>
          
          <!-- Body: normal weight, loose leading -->
          <p class="mt-3 text-base font-normal leading-relaxed text-gray-600">
            Dibangun untuk performa. Loading lebih cepat dari kompetitor mana pun.
          </p>
          
          <!-- Caption: light, muted -->
          <p class="mt-4 text-sm font-light italic text-gray-400">
            Terakhir diperbarui: 5 menit yang lalu
          </p>
        </div>
        ```
        
8. `[[8. Text Alignment, Transform & Decoration]]`
    
    - _Langkah konkret_:
        
        HTML
        
        ```
        <!-- Alignment responsif: center di mobile, left di desktop -->
        <h1 class="text-center lg:text-left text-4xl font-bold">
          Judul Halaman
        </h1>
        
        <!-- Transform untuk badge / label -->
        <span class="text-xs uppercase tracking-wider font-semibold text-blue-600">
          kategori produk
        </span>
        
        <!-- Decoration untuk link -->
        <a href="#" class="underline underline-offset-2 decoration-blue-500 hover:decoration-2">
          Baca selengkapnya
        </a>
        
        <!-- No underline dengan hover effect -->
        <a href="#" class="no-underline text-blue-600 hover:underline">
          Klik di sini
        </a>
        ```
        
9. `[[9. Line Height & Letter Spacing — Keterbacaan yang Optimal]]`
    
    - _Langkah konkret_:
        
        HTML
        
        ```
        <!-- Perbandingan line-height untuk body text -->
        <div class="space-y-6 max-w-lg">
          <p class="leading-none text-base">
            leading-none: Teks yang sangat rapat. Tidak cocok untuk paragraf panjang.
          </p>
          <p class="leading-tight text-base">
            leading-tight: Sedikit lebih baik, masih terasa sempit untuk body text.
          </p>
          <p class="leading-normal text-base">
            leading-normal: Standard browser default. Cukup untuk teks pendek.
          </p>
          <p class="leading-relaxed text-base">
            leading-relaxed: Ideal untuk artikel dan konten panjang. Nyaman dibaca.
          </p>
          <p class="leading-loose text-base">
            leading-loose: Sangat renggang. Cocok untuk konten presentasi.
          </p>
        </div>
        ```
        
10. `[[10. Text Overflow — Menangani Teks Panjang]]`
    
    - _Langkah konkret_:
        
        HTML
        
        ```
        <div class="space-y-4 max-w-xs">
          <!-- Truncate: potong di satu baris -->
          <div class="truncate text-gray-900 font-medium">
            Ini adalah judul yang sangat panjang dan akan dipotong jika melebihi container
          </div>
          
          <!-- Multi-line truncate (butuh CSS biasa atau plugin) -->
          <div class="line-clamp-2 text-gray-600 text-sm">
            Ini adalah deskripsi yang cukup panjang dan akan dibatasi dua baris saja.
            Sisanya akan tersembunyi dan diganti dengan ellipsis di akhir baris kedua.
          </div>
          
          <!-- Overflow wrap -->
          <div class="break-words text-gray-600 text-sm">
            Kata yang sangat panjang seperti: pneumonoultramicroscopicsilicovolcanoconiosis
            akan di-wrap ke baris berikutnya.
          </div>
        </div>
        ```
        

---

### C. Sistem Warna — Design Decisions yang Eksplisit

> 💡 **Benang Merah ke Typography**: Typography sudah menentukan ukuran dan berat teks. Warna melengkapinya — menentukan makna dan hierarki visual. Di Tailwind, setiap shade (100-950) punya peran.

text

```
Benang Merah Bagian C:
Typography menentukan hierarki ukuran (Poin 6-10) →
Warna menentukan hierarki makna →
Shade 50: background sangat terang →
Shade 500: warna brand utama →
Shade 700-900: teks di atas background gelap →
Opacity modifier: warna dengan transparansi
```

11. `[[11. Palet Warna Tailwind — Memahami Shade System]]`
    
    - _Langkah konkret_: Buat color showcase dan pahami polanya:
        
        HTML
        
        ```
        <!-- Blue palette — pola yang sama berlaku untuk semua warna -->
        <div class="grid grid-cols-11 gap-1 p-4">
          <div class="h-8 rounded bg-blue-50 text-center text-xs text-blue-900">50</div>
          <div class="h-8 rounded bg-blue-100 text-center text-xs text-blue-900">100</div>
          <div class="h-8 rounded bg-blue-200 text-center text-xs text-blue-900">200</div>
          <div class="h-8 rounded bg-blue-300 text-center text-xs text-white">300</div>
          <div class="h-8 rounded bg-blue-400 text-center text-xs text-white">400</div>
          <div class="h-8 rounded bg-blue-500 text-center text-xs text-white">500</div>
          <div class="h-8 rounded bg-blue-600 text-center text-xs text-white">600</div>
          <div class="h-8 rounded bg-blue-700 text-center text-xs text-white">700</div>
          <div class="h-8 rounded bg-blue-800 text-center text-xs text-white">800</div>
          <div class="h-8 rounded bg-blue-900 text-center text-xs text-white">900</div>
          <div class="h-8 rounded bg-blue-950 text-center text-xs text-white">950</div>
        </div>
        ```
        
    - Pola penggunaan:
        - **50-100**: background subtle, highlight area
        - **200-300**: border, divider
        - **400-500**: ikon, elemen sekunder
        - **500-600**: warna utama, CTA button
        - **700-800**: hover state dari 500-600
        - **900-950**: teks di background gelap
12. `[[12. Menggunakan Warna dengan Benar — Semantic Usage]]`
    
    - _Langkah konkret_: Buat alert component dengan warna yang semantik:
        
        HTML
        
        ```
        <!-- Success Alert -->
        <div class="flex items-start gap-3 rounded-lg bg-green-50 p-4 border border-green-200">
          <div class="mt-0.5 flex-shrink-0">
            <svg class="h-5 w-5 text-green-500"><!-- checkmark icon --></svg>
          </div>
          <div>
            <h4 class="text-sm font-semibold text-green-800">Berhasil!</h4>
            <p class="mt-1 text-sm text-green-700">Data telah berhasil disimpan.</p>
          </div>
        </div>
        
        <!-- Error Alert -->
        <div class="flex items-start gap-3 rounded-lg bg-red-50 p-4 border border-red-200">
          <svg class="h-5 w-5 text-red-500"><!-- x icon --></svg>
          <div>
            <h4 class="text-sm font-semibold text-red-800">Terjadi Kesalahan</h4>
            <p class="mt-1 text-sm text-red-700">Tidak dapat terhubung ke server.</p>
          </div>
        </div>
        
        <!-- Info Alert -->
        <div class="flex items-start gap-3 rounded-lg bg-blue-50 p-4 border border-blue-200">
          <svg class="h-5 w-5 text-blue-500"><!-- info icon --></svg>
          <div>
            <h4 class="text-sm font-semibold text-blue-800">Informasi</h4>
            <p class="mt-1 text-sm text-blue-700">Pembaruan tersedia, restart untuk menerapkan.</p>
          </div>
        </div>
        ```
        
13. `[[13. Opacity Modifier & Warna Transparan]]`
    
    - _Langkah konkret_:
        
        HTML
        
        ```
        <!-- Opacity modifier: warna/opacity -->
        <div class="space-y-3 p-4">
          <div class="rounded-lg bg-blue-500 p-3 text-white">bg-blue-500 (100% opacity)</div>
          <div class="rounded-lg bg-blue-500/75 p-3 text-white">bg-blue-500/75 (75%)</div>
          <div class="rounded-lg bg-blue-500/50 p-3 text-white">bg-blue-500/50 (50%)</div>
          <div class="rounded-lg bg-blue-500/25 p-3 text-blue-900">bg-blue-500/25 (25%)</div>
          <div class="rounded-lg bg-blue-500/10 p-3 text-blue-900">bg-blue-500/10 (10%)</div>
        </div>
        
        <!-- Glassmorphism effect -->
        <div class="relative h-48 overflow-hidden rounded-2xl bg-gradient-to-br from-blue-400 to-purple-500">
          <div class="absolute inset-0 flex items-center justify-center">
            <div class="rounded-xl bg-white/20 p-6 backdrop-blur-md border border-white/30">
              <p class="text-white font-semibold">Glassmorphism Card</p>
            </div>
          </div>
        </div>
        ```
        

---

### 🏗️ Checkpoint Level 1

text

```
✅ Checklist sebelum lanjut ke Level 2:

PEMAHAMAN:
├── Bisa jelaskan mengapa Tailwind utility-first (bukan sekadar "banyak class di HTML")
├── Bisa jelaskan mengapa class tidak boleh dibangun secara dinamis
├── Bisa jelaskan shade system warna (50 = apa, 500 = apa, 900 = apa)
└── Bisa jelaskan perbedaan leading-relaxed vs leading-tight (dan kapan pakai)

PRAKTIK DI PLAY (play.tailwindcss.com):
├── Button: primary, secondary, outline, destructive, disabled
├── Card: dengan image, badge, title, description, actions
├── Alert: success, error, warning, info
└── Typography showcase: semua size dan weight

SETUP LOKAL:
├── Project Vite + Tailwind berjalan
├── Prettier Tailwind plugin auto-sort class
├── VS Code IntelliSense aktif
└── Hot reload bekerja

Commit: feat: setup Tailwind project and create base components
```

---

## 🔵 LEVEL 2: LAYOUT — SPACING, FLEXBOX & GRID (Minggu 3-6)

> **Tema**: _"Dari komponen individual ke layout halaman yang terstruktur"_  
> **Benang Merah**: Komponen sudah bisa dibuat (Level 1) → susun komponen dalam layout → spacing system → flexbox → CSS Grid → halaman utama landing page  
> **Output**: Landing page dengan hero section, feature grid, testimonial, dan footer

---

### D. Spacing System — Konsistensi Jarak

> 💡 **Mengapa spacing system penting?** Tailwind punya scale 0-96 (0-384px). Dengan selalu menggunakan scale ini (bukan arbitrary pixels), UI otomatis punya ritme visual yang konsisten.

text

```
Benang Merah Bagian D:
Komponen individual sudah dibuat (Level 1) →
Spacing antar elemen menentukan "breathing room" →
Margin: jarak di luar elemen →
Padding: jarak di dalam elemen →
Space-x/y: jarak antar siblings →
Gap: jarak di dalam flex/grid
```

14. `[[14. Padding & Margin — Spacing di Dalam dan di Luar]]`
    
    - _Langkah konkret_: Pahami perbedaan dan kapan gunakan masing-masing:
        
        HTML
        
        ```
        <!-- Padding: ruang di dalam elemen -->
        <div class="bg-blue-100 p-8">
          <p>Ini di dalam dengan padding 32px (p-8)</p>
        </div>
        
        <!-- Padding per sisi -->
        <div class="bg-green-100 pt-4 pr-8 pb-4 pl-8">
          <!-- atau shorthand: py-4 px-8 -->
        </div>
        
        <!-- Margin: jarak dari elemen lain -->
        <div class="bg-red-100 p-4 mt-8">
          <p>Margin top 32px dari elemen di atasnya</p>
        </div>
        
        <!-- mx-auto: center element secara horizontal -->
        <div class="bg-yellow-100 p-4 max-w-sm mx-auto">
          <p>Card yang ter-center horizontal</p>
        </div>
        
        <!-- Margin negatif: tarik elemen keluar dari parent -->
        <div class="bg-gray-100 p-6">
          <div class="bg-white -mx-6 px-6 py-4 border-y">
            Full-width content dalam container
          </div>
        </div>
        ```
        
15. `[[15. space-x & space-y — Jarak Antar Siblings yang Elegan]]`
    
    - _Langkah konkret_: Bandingkan pendekatan margin vs space:
        
        HTML
        
        ```
        <!-- ❌ Cara lama: margin di setiap item -->
        <div class="flex">
          <button class="mr-2 ...">Button 1</button>
          <button class="mr-2 ...">Button 2</button>
          <button class="mr-2 ...">Button 3</button>
          <!-- Masalah: item terakhir punya margin yang tidak perlu -->
        </div>
        
        <!-- ✅ Cara Tailwind: space-x di parent -->
        <div class="flex space-x-2">
          <button class="...">Button 1</button>
          <button class="...">Button 2</button>
          <button class="...">Button 3</button>
          <!-- space-x otomatis skip item terakhir -->
        </div>
        
        <!-- space-y untuk stack vertikal -->
        <div class="space-y-4">
          <div class="rounded-lg bg-white p-4 shadow">Card 1</div>
          <div class="rounded-lg bg-white p-4 shadow">Card 2</div>
          <div class="rounded-lg bg-white p-4 shadow">Card 3</div>
        </div>
        ```
        

---

### E. Flexbox — Layout Satu Dimensi

> 💡 **Benang Merah ke Spacing**: Spacing mengatur jarak elemen individual. Flexbox mengatur bagaimana elemen berbagi ruang di satu baris atau kolom. `gap` menggantikan `space-x/y` di dalam flex/grid.

16. `[[16. Flexbox Fundamentals — flex, direction, wrap]]`
    
    - _Langkah konkret_: Refactor navbar landing page menggunakan flexbox:
        
        HTML
        
        ```
        <nav class="border-b border-gray-100 bg-white">
          <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
            <div class="flex h-16 items-center justify-between">
              <!-- Logo di kiri -->
              <div class="flex items-center gap-2">
                <div class="h-8 w-8 rounded-lg bg-blue-600"></div>
                <span class="text-xl font-bold text-gray-900">SaaSku</span>
              </div>
              
              <!-- Nav links di tengah (desktop) -->
              <div class="hidden md:flex items-center gap-8">
                <a href="#" class="text-sm font-medium text-gray-600 hover:text-gray-900">Fitur</a>
                <a href="#" class="text-sm font-medium text-gray-600 hover:text-gray-900">Harga</a>
                <a href="#" class="text-sm font-medium text-gray-600 hover:text-gray-900">Blog</a>
              </div>
              
              <!-- CTA di kanan -->
              <div class="flex items-center gap-3">
                <a href="#" class="text-sm font-medium text-gray-700 hover:text-gray-900">Masuk</a>
                <a href="#" class="rounded-lg bg-blue-600 px-4 py-2 text-sm font-medium text-white hover:bg-blue-700 transition-colors">
                  Coba Gratis
                </a>
              </div>
            </div>
          </div>
        </nav>
        ```
        
17. `[[17. justify-content & align-items — Distribusi & Penyelarasan]]`
    
    - _Langkah konkret_: Buat stat cards row untuk landing page:
        
        HTML
        
        ```
        <!-- Stats section: items di-center, space merata -->
        <div class="flex flex-col sm:flex-row items-center justify-center gap-8 py-12">
          <!-- Stat item -->
          <div class="flex flex-col items-center text-center">
            <div class="text-4xl font-extrabold text-blue-600">10K+</div>
            <div class="mt-1 text-sm font-medium text-gray-500">Pengguna Aktif</div>
          </div>
          <div class="hidden sm:block h-12 w-px bg-gray-200"></div>
          <div class="flex flex-col items-center text-center">
            <div class="text-4xl font-extrabold text-blue-600">99.9%</div>
            <div class="mt-1 text-sm font-medium text-gray-500">Uptime</div>
          </div>
          <div class="hidden sm:block h-12 w-px bg-gray-200"></div>
          <div class="flex flex-col items-center text-center">
            <div class="text-4xl font-extrabold text-blue-600">50+</div>
            <div class="mt-1 text-sm font-medium text-gray-500">Integrasi</div>
          </div>
        </div>
        ```
        
18. `[[18. flex-1, flex-none & grow/shrink]]`
    
    - _Langkah konkret_: Buat pricing card layout:
        
        HTML
        
        ```
        <!-- flex-col dengan spacing yang benar -->
        <div class="flex flex-col h-full rounded-2xl border border-gray-200 p-8">
          <!-- Header: tidak tumbuh -->
          <div class="flex-none">
            <h3 class="text-lg font-semibold">Pro</h3>
            <div class="mt-4 flex items-baseline gap-1">
              <span class="text-4xl font-bold">Rp 99K</span>
              <span class="text-gray-500">/bulan</span>
            </div>
          </div>
          
          <!-- Feature list: tumbuh mengisi ruang -->
          <ul class="mt-8 flex-1 space-y-3">
            <li class="flex items-center gap-2 text-sm">
              <svg class="h-4 w-4 text-green-500 flex-shrink-0"><!-- check --></svg>
              Unlimited projects
            </li>
            <!-- more features -->
          </ul>
          
          <!-- CTA: tidak tumbuh, selalu di bawah -->
          <div class="mt-8 flex-none">
            <a href="#" class="block w-full rounded-xl bg-blue-600 py-3 text-center text-sm font-semibold text-white hover:bg-blue-700">
              Mulai Sekarang
            </a>
          </div>
        </div>
        ```
        

---

### F. CSS Grid — Layout Dua Dimensi

> 💡 **Benang Merah ke Flexbox**: Flexbox = satu dimensi (baris ATAU kolom). Grid = dua dimensi (baris DAN kolom). Feature grid, card gallery, dan layout halaman butuh Grid.

19. `[[19. CSS Grid Fundamentals — grid-cols & gap]]`
    
    - _Langkah konkret_: Buat feature grid untuk landing page:
        
        HTML
        
        ```
        <!-- Feature Grid: 1 kolom mobile, 2 tablet, 3 desktop -->
        <div class="mx-auto max-w-7xl px-4 py-16 sm:px-6 lg:px-8">
          <div class="text-center">
            <h2 class="text-3xl font-bold tracking-tight text-gray-900 sm:text-4xl">
              Semua yang Anda Butuhkan
            </h2>
            <p class="mx-auto mt-4 max-w-2xl text-lg text-gray-600">
              Platform lengkap untuk tim modern yang bekerja cepat.
            </p>
          </div>
          
          <!-- Feature grid -->
          <div class="mt-16 grid gap-8 sm:grid-cols-2 lg:grid-cols-3">
            <!-- Feature card -->
            <div class="group relative rounded-2xl border border-gray-100 bg-white p-6 shadow-sm hover:shadow-md transition-shadow">
              <!-- Icon -->
              <div class="flex h-12 w-12 items-center justify-center rounded-xl bg-blue-50 text-blue-600">
                <svg class="h-6 w-6"><!-- icon --></svg>
              </div>
              <h3 class="mt-4 text-lg font-semibold text-gray-900">Kolaborasi Real-time</h3>
              <p class="mt-2 text-sm leading-relaxed text-gray-600">
                Bekerja bersama tim secara real-time. Lihat perubahan seketika.
              </p>
            </div>
            
            <!-- Repeat 5 lebih card -->
          </div>
        </div>
        ```
        
20. `[[20. col-span & Responsive Grid — Layout yang Fleksibel]]`
    
    - _Langkah konkret_: Buat bento grid yang menarik:
        
        HTML
        
        ```
        <!-- Bento Grid: card dengan ukuran berbeda -->
        <div class="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-4">
          <!-- Card besar: span 2 kolom di desktop -->
          <div class="lg:col-span-2 rounded-2xl bg-blue-600 p-8 text-white">
            <h3 class="text-2xl font-bold">Fitur Utama</h3>
            <p class="mt-2 text-blue-100">Deskripsi fitur yang lebih panjang karena card ini lebih besar.</p>
          </div>
          
          <!-- Card normal -->
          <div class="rounded-2xl bg-gray-900 p-6 text-white">
            <h3 class="font-bold">Fitur B</h3>
            <p class="mt-2 text-sm text-gray-400">Deskripsi singkat.</p>
          </div>
          
          <!-- Card normal -->
          <div class="rounded-2xl bg-purple-600 p-6 text-white">
            <h3 class="font-bold">Fitur C</h3>
            <p class="mt-2 text-sm text-purple-200">Deskripsi singkat.</p>
          </div>
          
          <!-- Card yang span penuh di mobile, normal di desktop -->
          <div class="sm:col-span-2 lg:col-span-4 rounded-2xl bg-gray-50 border p-8">
            <h3 class="font-bold text-gray-900">Banner Full Width</h3>
          </div>
        </div>
        ```
        
21. `[[21. Container & Max-Width — Membatasi Lebar Konten]]`
    
    - _Langkah konkret_: Buat section wrapper yang konsisten:
        
        HTML
        
        ```
        <!-- Pola container yang konsisten untuk semua section -->
        <section class="relative bg-white">
          <!-- Max-width container yang centered -->
          <div class="mx-auto max-w-7xl px-4 py-24 sm:px-6 lg:px-8">
            <!-- Content di sini -->
          </div>
        </section>
        
        <!-- Narrow container untuk konten text-heavy -->
        <section class="bg-gray-50">
          <div class="mx-auto max-w-3xl px-4 py-16 sm:px-6">
            <!-- Blog post, FAQ, dll -->
          </div>
        </section>
        
        <!-- Full-width dengan inner container -->
        <section class="bg-blue-600">
          <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
            <div class="grid lg:grid-cols-2 gap-12 py-20 items-center">
              <!-- Split content -->
            </div>
          </div>
        </section>
        ```
        

---

### G. Sizing & Responsif — UI yang Adaptif

22. `[[22. Width, Height & Sizing — Mengontrol Dimensi Elemen]]`
    
    - _Langkah konkret_: Buat avatar dan image components:
        
        HTML
        
        ```
        <!-- Avatar sizes -->
        <div class="flex items-center gap-4">
          <img src="avatar.jpg" class="h-8 w-8 rounded-full object-cover" alt="User">
          <img src="avatar.jpg" class="h-10 w-10 rounded-full object-cover" alt="User">
          <img src="avatar.jpg" class="h-12 w-12 rounded-full object-cover" alt="User">
          <img src="avatar.jpg" class="h-16 w-16 rounded-full object-cover" alt="User">
        </div>
        
        <!-- Responsive image -->
        <img
          src="hero.jpg"
          class="h-64 w-full object-cover sm:h-80 lg:h-96 rounded-2xl"
          alt="Hero"
        >
        
        <!-- Aspect ratio (responsive) -->
        <div class="aspect-video overflow-hidden rounded-xl bg-gray-100">
          <img src="thumbnail.jpg" class="h-full w-full object-cover" alt="Video thumbnail">
        </div>
        
        <!-- Square aspect ratio -->
        <div class="aspect-square overflow-hidden rounded-xl">
          <img src="product.jpg" class="h-full w-full object-cover" alt="Product">
        </div>
        ```
        
23. `[[23. Responsive Prefix — Mobile-First yang Sebenarnya]]`
    
    - _Langkah konkret_: Refactor hero section menjadi fully responsive:
        
        HTML
        
        ```
        <!-- Hero section: responsive dari 320px ke 1440px+ -->
        <section class="relative overflow-hidden bg-white">
          <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
            <div class="grid items-center gap-12 py-20 lg:grid-cols-2 lg:py-28">
              
              <!-- Text content -->
              <div class="text-center lg:text-left">
                <!-- Badge -->
                <span class="inline-flex items-center gap-2 rounded-full bg-blue-50 px-3 py-1 text-xs font-semibold text-blue-700">
                  🚀 Baru: Integrasi AI tersedia
                </span>
                
                <!-- Heading: kecil di mobile, besar di desktop -->
                <h1 class="mt-6 text-4xl font-extrabold tracking-tight text-gray-900 sm:text-5xl lg:text-6xl">
                  Bangun
                  <span class="text-blue-600">lebih cepat</span>
                  bersama tim Anda
                </h1>
                
                <!-- Subheading -->
                <p class="mx-auto mt-6 max-w-xl text-lg text-gray-600 lg:mx-0">
                  Platform kolaborasi terbaik untuk tim yang ingin bergerak cepat.
                  Setup dalam 5 menit.
                </p>
                
                <!-- CTA buttons -->
                <div class="mt-10 flex flex-col gap-3 sm:flex-row sm:justify-center lg:justify-start">
                  <a href="#" class="inline-flex items-center justify-center rounded-xl bg-blue-600 px-8 py-3.5 text-sm font-semibold text-white shadow-sm hover:bg-blue-700 transition-colors">
                    Mulai Gratis
                  </a>
                  <a href="#" class="inline-flex items-center justify-center rounded-xl border border-gray-200 bg-white px-8 py-3.5 text-sm font-semibold text-gray-700 shadow-sm hover:bg-gray-50 transition-colors">
                    Lihat Demo →
                  </a>
                </div>
                
                <!-- Social proof -->
                <div class="mt-10 flex items-center justify-center gap-4 lg:justify-start">
                  <div class="flex -space-x-2">
                    <!-- Avatar stack -->
                    <img src="user1.jpg" class="h-8 w-8 rounded-full border-2 border-white object-cover" alt="">
                    <img src="user2.jpg" class="h-8 w-8 rounded-full border-2 border-white object-cover" alt="">
                    <img src="user3.jpg" class="h-8 w-8 rounded-full border-2 border-white object-cover" alt="">
                  </div>
                  <p class="text-sm text-gray-600">
                    Dipercaya <span class="font-semibold text-gray-900">10,000+</span> tim
                  </p>
                </div>
              </div>
              
              <!-- Image / mockup -->
              <div class="relative hidden lg:block">
                <div class="aspect-[4/3] overflow-hidden rounded-2xl bg-gray-100 shadow-2xl">
                  <img src="app-screenshot.png" class="h-full w-full object-cover object-top" alt="App screenshot">
                </div>
              </div>
            </div>
          </div>
        </section>
        ```
        

---

### 🏗️ Checkpoint Level 2

text

```
✅ Checklist sebelum lanjut ke Level 3:

PROYEK: Landing Page Tahap 1
File: index.html di project Vite + Tailwind

SECTIONS YANG HARUS ADA:
├── Navbar: logo + nav links + CTA (flexbox, hidden/flex responsive)
├── Hero: heading + subheading + CTA buttons + social proof (grid 2 col)
├── Stats: 3-4 angka statistik (flex row dengan divider)
├── Features: 6 cards (grid cols-3 responsif)
├── Pricing: 3 tier cards (grid cols-3 responsif, card dengan flex-col)
└── Footer: multi-column (grid cols-4 responsif)

TEKNIS:
├── Konsisten pakai mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 untuk container
├── Semua section mobile-first (tidak ada horizontal scroll di 375px)
├── gap digunakan (bukan space-x/y) di dalam flex dan grid
└── aspect-ratio digunakan untuk gambar

Commit: feat: build responsive landing page with hero, features, pricing
```

---

## 🟡 LEVEL 3: VISUAL POLISH — EFEK & ANIMASI (Minggu 6-9)

> **Tema**: _"Dari layout yang fungsional ke UI yang terasa premium"_  
> **Benang Merah**: Layout sudah responsif (Level 2) → tambahkan kedalaman visual → shadow, gradient, blur → hover effects → animasi → UI yang terasa "live"  
> **Output**: Landing page dengan visual polish professional

---

### H. Background, Gradient & Shadow

24. `[[24. Gradient — Background yang Menarik]]`
    
    - _Langkah konkret_: Upgrade hero section dengan gradient:
        
        HTML
        
        ```
        <!-- Hero dengan gradient background -->
        <section class="relative overflow-hidden bg-gradient-to-br from-slate-900 via-blue-950 to-slate-900">
          <!-- Decorative orb (blur circle) -->
          <div class="absolute -top-40 right-0 h-96 w-96 rounded-full bg-blue-500/20 blur-3xl"></div>
          <div class="absolute -bottom-40 -left-20 h-96 w-96 rounded-full bg-purple-500/20 blur-3xl"></div>
          
          <div class="relative mx-auto max-w-7xl px-4 py-28 sm:px-6 lg:px-8">
            <h1 class="text-center text-5xl font-extrabold tracking-tight text-white lg:text-7xl">
              Bangun
              <!-- Gradient text -->
              <span class="bg-gradient-to-r from-blue-400 to-purple-400 bg-clip-text text-transparent">
                masa depan
              </span>
              Anda
            </h1>
          </div>
        </section>
        
        <!-- Gradient card -->
        <div class="rounded-2xl bg-gradient-to-br from-blue-500 to-blue-700 p-6 text-white shadow-lg shadow-blue-500/25">
          <h3 class="text-xl font-bold">Premium Card</h3>
          <p class="mt-2 text-blue-100">Deskripsi dengan warna yang harmonis.</p>
        </div>
        ```
        
25. `[[25. Shadow & Ring — Kedalaman Visual]]`
    
    - _Langkah konkret_: Tambahkan shadow yang tepat ke komponen:
        
        HTML
        
        ```
        <!-- Shadow scale -->
        <div class="space-y-4 p-8 bg-gray-50">
          <div class="rounded-xl bg-white p-6 shadow-sm">shadow-sm — subtle</div>
          <div class="rounded-xl bg-white p-6 shadow">shadow — default</div>
          <div class="rounded-xl bg-white p-6 shadow-md">shadow-md — medium</div>
          <div class="rounded-xl bg-white p-6 shadow-lg">shadow-lg — large</div>
          <div class="rounded-xl bg-white p-6 shadow-xl">shadow-xl — extra large</div>
          <div class="rounded-xl bg-white p-6 shadow-2xl">shadow-2xl — 2x large</div>
        </div>
        
        <!-- Colored shadow (brand shadow) -->
        <button class="rounded-xl bg-blue-600 px-8 py-3 font-semibold text-white shadow-lg shadow-blue-500/30 hover:shadow-xl hover:shadow-blue-500/40 transition-all">
          Branded Button
        </button>
        
        <!-- Ring untuk focus state -->
        <input
          type="text"
          class="rounded-lg border border-gray-200 px-4 py-2.5 outline-none focus:border-blue-500 focus:ring-2 focus:ring-blue-500/20 transition-all"
          placeholder="Email address"
        >
        ```
        
26. `[[26. Backdrop Blur & Filter — Efek Modern]]`
    
    - _Langkah konkret_: Buat navbar dengan blur effect:
        
        HTML
        
        ```
        <!-- Sticky navbar dengan backdrop blur (glassmorphism nav) -->
        <nav class="sticky top-0 z-50 border-b border-gray-200/80 bg-white/80 backdrop-blur-md">
          <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
            <!-- nav content -->
          </div>
        </nav>
        
        <!-- Image dengan grayscale hover effect -->
        <div class="group overflow-hidden rounded-xl">
          <img
            src="team.jpg"
            class="h-64 w-full object-cover grayscale transition-all duration-500 group-hover:grayscale-0 group-hover:scale-105"
            alt="Team photo"
          >
        </div>
        ```
        

---

### I. Hover, Focus & Transition — UI yang Responsif

> 💡 **Benang Merah ke Warna**: Hover state adalah perubahan warna yang smooth. Transition menghaluskan perubahan itu. Tanpa transition, UI terasa "kasar". Dengan transition, UI terasa "hidup".

27. `[[27. Hover & Focus States — Interaktivitas yang Jelas]]`
    
    - _Langkah konkret_: Buat navigation menu yang interaktif:
        
        HTML
        
        ```
        <!-- Nav link dengan underline animasi -->
        <a
          href="#"
          class="relative text-sm font-medium text-gray-600 transition-colors hover:text-gray-900 after:absolute after:-bottom-1 after:left-0 after:h-0.5 after:w-0 after:bg-blue-600 after:transition-all hover:after:w-full"
        >
          Fitur
        </a>
        
        <!-- Card dengan hover lift -->
        <div class="group cursor-pointer rounded-2xl border border-gray-100 bg-white p-6 shadow-sm transition-all hover:-translate-y-1 hover:shadow-lg">
          <h3 class="font-semibold group-hover:text-blue-600 transition-colors">Feature Card</h3>
          <p class="mt-2 text-sm text-gray-600">Klik untuk detail...</p>
          <!-- Arrow yang muncul saat hover -->
          <div class="mt-4 flex items-center gap-1 text-sm font-medium text-blue-600 opacity-0 transition-opacity group-hover:opacity-100">
            Selengkapnya
            <svg class="h-4 w-4 translate-x-0 transition-transform group-hover:translate-x-1"><!-- arrow --></svg>
          </div>
        </div>
        ```
        
28. `[[28. group & peer — State yang Memengaruhi Elemen Lain]]`
    
    - _Langkah konkret_: Buat accordion custom dan form dengan peer:
        
        HTML
        
        ```
        <!-- FAQ Accordion menggunakan peer -->
        <div class="space-y-3">
          <div class="rounded-xl border border-gray-200 overflow-hidden">
            <input type="checkbox" id="faq1" class="peer hidden">
            <label
              for="faq1"
              class="flex cursor-pointer items-center justify-between p-5 font-medium text-gray-900 hover:bg-gray-50"
            >
              Apakah ada trial gratis?
              <!-- Chevron yang rotate saat checked -->
              <svg class="h-5 w-5 shrink-0 text-gray-500 transition-transform peer-checked:rotate-180">
                <!-- chevron down icon -->
              </svg>
            </label>
            <!-- Konten yang muncul/hilang -->
            <div class="max-h-0 overflow-hidden transition-all duration-300 peer-checked:max-h-96">
              <p class="px-5 pb-5 text-sm leading-relaxed text-gray-600">
                Ya! Kami menawarkan trial 14 hari gratis tanpa kartu kredit.
              </p>
            </div>
          </div>
        </div>
        
        <!-- Form dengan peer untuk floating label -->
        <div class="relative mt-6">
          <input
            type="email"
            id="email"
            placeholder=" "
            class="peer block w-full rounded-lg border border-gray-200 px-4 pb-2.5 pt-6 text-sm focus:border-blue-500 focus:outline-none focus:ring-2 focus:ring-blue-500/20"
          >
          <label
            for="email"
            class="absolute left-4 top-4 z-10 origin-top-left -translate-y-2 scale-75 text-xs font-medium text-gray-500 transition-all peer-placeholder-shown:translate-y-0 peer-placeholder-shown:scale-100 peer-placeholder-shown:text-sm peer-focus:-translate-y-2 peer-focus:scale-75 peer-focus:text-blue-600"
          >
            Email Address
          </label>
        </div>
        ```
        
29. `[[29. Transition & Duration — Animasi yang Smooth]]`
    
    - _Langkah konkret_: Buat button dengan loading state:
        
        HTML
        
        ```
        <!-- Button dengan state transition -->
        <button
          id="submitBtn"
          class="group relative inline-flex items-center justify-center overflow-hidden rounded-xl bg-blue-600 px-8 py-3 font-semibold text-white transition-all duration-300 hover:bg-blue-700 active:scale-95 disabled:cursor-not-allowed disabled:opacity-60"
        >
          <!-- Normal state -->
          <span class="inline-flex items-center gap-2 transition-all group-disabled:opacity-0">
            <svg class="h-4 w-4"><!-- send icon --></svg>
            Kirim Sekarang
          </span>
          
          <!-- Loading state (hidden by default, shown via JS) -->
          <span class="absolute inline-flex items-center gap-2 opacity-0 transition-all group-disabled:opacity-100">
            <svg class="h-4 w-4 animate-spin"><!-- spinner --></svg>
            Memproses...
          </span>
        </button>
        
        <!-- Skeleton loading -->
        <div class="space-y-4">
          <div class="h-4 w-3/4 animate-pulse rounded-full bg-gray-200"></div>
          <div class="h-4 w-full animate-pulse rounded-full bg-gray-200"></div>
          <div class="h-4 w-1/2 animate-pulse rounded-full bg-gray-200"></div>
        </div>
        ```
        
30. `[[30. animate-* — Animasi Bawaan Tailwind]]`
    
    - _Langkah konkret_: Buat notification badge dan loading indicators:
        
        HTML
        
        ```
        <!-- Notification indicator dengan ping -->
        <div class="relative inline-flex">
          <button class="rounded-full p-2 text-gray-500 hover:bg-gray-100">
            <svg class="h-6 w-6"><!-- bell icon --></svg>
          </button>
          <!-- Ping indicator -->
          <span class="absolute top-1 right-1 flex h-2.5 w-2.5">
            <span class="absolute inline-flex h-full w-full animate-ping rounded-full bg-red-400 opacity-75"></span>
            <span class="relative inline-flex h-2.5 w-2.5 rounded-full bg-red-500"></span>
          </span>
        </div>
        
        <!-- Status badge dengan pulse -->
        <span class="inline-flex items-center gap-1.5 rounded-full bg-green-50 px-3 py-1 text-xs font-medium text-green-700">
          <span class="h-1.5 w-1.5 animate-pulse rounded-full bg-green-500"></span>
          Sistem Online
        </span>
        
        <!-- Bounce untuk scroll indicator -->
        <div class="absolute bottom-8 left-1/2 -translate-x-1/2 animate-bounce">
          <svg class="h-6 w-6 text-gray-400"><!-- chevron down --></svg>
        </div>
        ```
        

---

### 🏗️ Checkpoint Level 3

text

```
✅ Checklist sebelum lanjut ke Level 4:

VISUAL ENHANCEMENTS:
├── Hero: gradient background + decorative orbs + gradient text
├── Navbar: backdrop-blur glassmorphism effect + sticky
├── Feature cards: hover lift + shadow transition + group-hover
├── Pricing cards: brand shadow + gradient tiers
├── CTA buttons: colored shadow + hover transition
├── Input fields: ring focus state + floating label (peer)
├── FAQ: CSS-only accordion menggunakan peer
├── Loading: skeleton + spinner + ping animation
└── Status badges: animate-pulse

TEKNIS:
├── Semua transition menggunakan duration-200 atau duration-300 (bukan default)
├── group digunakan untuk setidaknya satu komponen
├── peer digunakan untuk form atau accordion
├── animate-pulse untuk skeleton loading
└── Motion-safe dipertimbangkan (prefers-reduced-motion)

Commit: feat: add visual polish with gradients, shadows, and animations
```

---

## 🟠 LEVEL 4: KONFIGURASI & CUSTOMISASI (Minggu 9-13)

> **Tema**: _"Dari Tailwind default ke Tailwind yang punya identitas brand sendiri"_  
> **Benang Merah**: Landing page dengan default Tailwind (Level 1-3) → konfigurasi brand → design tokens → dark mode → UI yang konsisten dengan brand  
> **Output**: Landing page dengan brand yang dikustomisasi dan dark mode yang sempurna

---

### J. Konfigurasi tailwind.config.js

31. `[[31. Extend vs Override — Memperluas tanpa Menghapus Default]]`
    
    - _Langkah konkret_: Setup brand configuration:
        
        JavaScript
        
        ```
        /** @type {import('tailwindcss').Config} */
        export default {
          content: ['./index.html', './src/**/*.{js,ts,jsx,tsx,vue}'],
          darkMode: 'class', // atau 'media'
          
          theme: {
            // ⚠️ EXTEND: tambah ke default, jangan hapus default
            extend: {
              // Brand colors
              colors: {
                brand: {
                  50:  '#eff6ff',
                  100: '#dbeafe',
                  200: '#bfdbfe',
                  300: '#93c5fd',
                  400: '#60a5fa',
                  500: '#3b82f6',  // ← brand primary
                  600: '#2563eb',
                  700: '#1d4ed8',
                  800: '#1e40af',
                  900: '#1e3a8a',
                  950: '#172554',
                },
              },
              
              // Font family (Google Fonts diload di HTML)
              fontFamily: {
                sans: ['Inter', 'system-ui', 'sans-serif'],
                display: ['Lexend', 'sans-serif'],
                mono: ['JetBrains Mono', 'monospace'],
              },
              
              // Extended spacing
              spacing: {
                '18': '4.5rem',  // 72px
                '88': '22rem',   // 352px
                '128': '32rem',  // 512px
              },
              
              // Border radius
              borderRadius: {
                '4xl': '2rem',   // 32px
              },
              
              // Custom shadows
              boxShadow: {
                'brand': '0 4px 14px 0 rgba(59, 130, 246, 0.25)',
                'brand-lg': '0 10px 25px 0 rgba(59, 130, 246, 0.35)',
                'soft': '0 2px 15px -3px rgba(0, 0, 0, 0.07), 0 10px 20px -2px rgba(0, 0, 0, 0.04)',
              },
              
              // Custom animation
              animation: {
                'fade-in': 'fadeIn 0.5s ease-in-out',
                'slide-up': 'slideUp 0.4s ease-out',
                'float': 'float 3s ease-in-out infinite',
              },
              keyframes: {
                fadeIn: {
                  '0%': { opacity: '0' },
                  '100%': { opacity: '1' },
                },
                slideUp: {
                  '0%': { opacity: '0', transform: 'translateY(20px)' },
                  '100%': { opacity: '1', transform: 'translateY(0)' },
                },
                float: {
                  '0%, 100%': { transform: 'translateY(0)' },
                  '50%': { transform: 'translateY(-10px)' },
                },
              },
              
              // Custom breakpoints
              screens: {
                'xs': '475px',  // extra small
                // sm, md, lg, xl, 2xl sudah ada — jangan override
              },
            },
            
            // ⚠️ OVERRIDE: hati-hati, ini MENGGANTI default!
            // Biasanya tidak perlu menggunakan ini
          },
          
          plugins: [],
        }
        ```
        
32. `[[32. @layer directive — Mengorganisasi Custom CSS]]`
    
    - _Langkah konkret_: Tambahkan custom styles yang terorganisir:
        
        CSS
        
        ```
        /* src/style.css */
        @tailwind base;
        @tailwind components;
        @tailwind utilities;
        
        @layer base {
          /* Base styles: font, selection, scrollbar */
          html {
            scroll-behavior: smooth;
            -webkit-font-smoothing: antialiased;
          }
          
          ::selection {
            @apply bg-brand-500/20 text-brand-900;
          }
          
          /* Custom scrollbar */
          ::-webkit-scrollbar {
            @apply w-1.5;
          }
          ::-webkit-scrollbar-track {
            @apply bg-gray-100;
          }
          ::-webkit-scrollbar-thumb {
            @apply rounded-full bg-gray-300 hover:bg-gray-400;
          }
        }
        
        @layer components {
          /* Reusable component styles — gunakan sparingly! */
          .btn-primary {
            @apply inline-flex items-center justify-center rounded-xl bg-brand-600 px-6 py-2.5 text-sm font-semibold text-white shadow-brand transition-all hover:bg-brand-700 hover:shadow-brand-lg active:scale-95 disabled:opacity-50;
          }
          
          .btn-secondary {
            @apply inline-flex items-center justify-center rounded-xl border border-gray-200 bg-white px-6 py-2.5 text-sm font-semibold text-gray-700 shadow-sm transition-all hover:bg-gray-50 hover:border-gray-300 active:scale-95;
          }
          
          .card {
            @apply rounded-2xl border border-gray-100 bg-white p-6 shadow-soft;
          }
          
          .section-container {
            @apply mx-auto max-w-7xl px-4 sm:px-6 lg:px-8;
          }
        }
        
        @layer utilities {
          /* Custom utilities yang tidak ada di Tailwind */
          .text-balance {
            text-wrap: balance;
          }
          
          .bg-grid {
            background-image: url("data:image/svg+xml,...");
          }
        }
        ```
        

---

### K. Dark Mode — UI untuk Semua Kondisi Cahaya

33. `[[33. Dark Mode Setup — Class Strategy yang Disarankan]]`
    
    - _Langkah konkret_: Setup dark mode dengan class strategy:
        
        JavaScript
        
        ```
        // tailwind.config.js
        export default {
          darkMode: 'class', // 'class' atau 'media'
          // ...
        }
        ```
        
        JavaScript
        
        ```
        // src/darkMode.js — toggle logic
        const THEME_KEY = 'theme';
        
        function getInitialTheme() {
          const stored = localStorage.getItem(THEME_KEY);
          if (stored) return stored;
          return window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
        }
        
        function applyTheme(theme) {
          document.documentElement.classList.toggle('dark', theme === 'dark');
          localStorage.setItem(THEME_KEY, theme);
        }
        
        // Apply immediately to prevent flash
        applyTheme(getInitialTheme());
        
        // Export untuk digunakan di toggle button
        export function toggleTheme() {
          const current = document.documentElement.classList.contains('dark') ? 'dark' : 'light';
          applyTheme(current === 'dark' ? 'light' : 'dark');
        }
        ```
        
34. `[[34. Mendesain untuk Dark Mode — Pola yang Tepat]]`
    
    - _Langkah konkret_: Refactor landing page untuk dark mode:
        
        HTML
        
        ```
        <!-- Navbar yang benar untuk dark mode -->
        <nav class="border-b border-gray-200 bg-white dark:border-gray-800 dark:bg-gray-900">
          <!-- Brand -->
          <span class="text-xl font-bold text-gray-900 dark:text-white">SaaSku</span>
          
          <!-- Nav links -->
          <a href="#" class="text-sm font-medium text-gray-600 hover:text-gray-900 dark:text-gray-400 dark:hover:text-white">
            Fitur
          </a>
        </nav>
        
        <!-- Card yang adaptif -->
        <div class="rounded-2xl border border-gray-100 bg-white p-6 shadow-soft dark:border-gray-700/50 dark:bg-gray-800">
          <h3 class="text-lg font-semibold text-gray-900 dark:text-white">Judul Card</h3>
          <p class="mt-2 text-sm text-gray-600 dark:text-gray-400">Deskripsi card...</p>
          
          <!-- Icon -->
          <div class="flex h-12 w-12 items-center justify-center rounded-xl bg-blue-50 text-blue-600 dark:bg-blue-900/30 dark:text-blue-400">
            <svg><!-- icon --></svg>
          </div>
          
          <!-- Badge -->
          <span class="rounded-full bg-green-100 px-3 py-1 text-xs font-medium text-green-700 dark:bg-green-900/30 dark:text-green-400">
            Aktif
          </span>
        </div>
        
        <!-- Toggle button -->
        <button
          onclick="toggleTheme()"
          class="rounded-full p-2 text-gray-500 transition-colors hover:bg-gray-100 hover:text-gray-900 dark:text-gray-400 dark:hover:bg-gray-800 dark:hover:text-white"
          aria-label="Toggle dark mode"
        >
          <!-- Sun icon (tampil di dark mode) -->
          <svg class="hidden dark:block h-5 w-5"><!-- sun --></svg>
          <!-- Moon icon (tampil di light mode) -->
          <svg class="block dark:hidden h-5 w-5"><!-- moon --></svg>
        </button>
        ```
        

---

### L. Plugin Tailwind — Memperluas Kemampuan

35. `[[35. Plugin Typography — Konten Blog yang Indah]]`
    - _Langkah konkret_:
        
        Bash
        
        ```
        npm install -D @tailwindcss/typography
        ```
        
        JavaScript
        
        ```
        // tailwind.config.js
        plugins: [
          require('@tailwindcss/typography'),
        ]
        ```
        
        HTML
        
        ```
        <!-- Blog post / artikel: class prose yang mengatur semua styling konten -->
        <article class="prose prose-lg prose-gray max-w-none
                        dark:prose-invert
                        prose-headings:font-bold prose-headings:tracking-tight
                        prose-a:text-blue-600 prose-a:no-underline hover:prose-a:underline
                        prose-code:rounded prose-code:bg-gray-100 prose-code:px-1 prose-code:text-sm dark:prose-code:bg-gray-800
                        prose-pre:rounded-xl">
          <!-- Konten HTML dari CMS / markdown -->
          <h1>Judul Artikel</h1>
          <p>Paragraf pertama yang akan di-style otomatis oleh prose...</p>
          <h2>Sub Judul</h2>
          <p>Konten lebih lanjut dengan <a href="#">link</a> dan <code>inline code</code>.</p>
          <pre><code>// Code block
        ```
        

const hello = 'world';</code></pre>  
</article>  
```

36. `[[36. Plugin Forms — Reset Style Form yang Konsisten]]`
    
    - _Langkah konkret_:
        
        Bash
        
        ```
        npm install -D @tailwindcss/forms
        ```
        
        JavaScript
        
        ```
        plugins: [
          require('@tailwindcss/forms'),
        ]
        ```
        
        HTML
        
        ```
        <!-- Dengan @tailwindcss/forms, form elements di-reset
             sehingga styling Tailwind bisa diterapkan lebih bersih -->
        
        <!-- Text input -->
        <input
          type="text"
          class="block w-full rounded-xl border-gray-200 px-4 py-3 text-sm shadow-sm focus:border-blue-500 focus:ring-blue-500/20 dark:border-gray-700 dark:bg-gray-800 dark:text-white"
          placeholder="Email address"
        >
        
        <!-- Select -->
        <select class="block w-full rounded-xl border-gray-200 px-4 py-3 text-sm shadow-sm focus:border-blue-500 focus:ring-blue-500/20">
          <option>Pilih opsi...</option>
        </select>
        
        <!-- Checkbox -->
        <div class="flex items-center gap-3">
          <input
            type="checkbox"
            class="h-4 w-4 rounded border-gray-300 text-blue-600 focus:ring-blue-500/20"
            id="terms"
          >
          <label for="terms" class="text-sm text-gray-700">Setuju dengan syarat</label>
        </div>
        ```
        
37. `[[37. Arbitrary Values — Ketika Default Tidak Cukup]]`
    
    - _Langkah konkret_:
        
        HTML
        
        ```
        <!-- Arbitrary value untuk nilai spesifik yang tidak ada di scale default -->
        
        <!-- Width spesifik -->
        <div class="w-[347px] h-[200px]">
          Ukuran spesifik dari design mockup
        </div>
        
        <!-- Warna brand dari design (bukan dari config) -->
        <div class="bg-[#1a1a2e] text-[#16213e]">
          Warna spesifik
        </div>
        
        <!-- Grid template yang tidak ada di default -->
        <div class="grid grid-cols-[2fr_1fr_1fr] gap-4">
          <div>Main content</div>
          <div>Sidebar 1</div>
          <div>Sidebar 2</div>
        </div>
        
        <!-- Arbitrary variant untuk selector khusus -->
        <ul class="[&>li:first-child]:mt-0 [&>li]:mt-2">
          <li>Item tanpa margin atas</li>
          <li>Item dengan margin atas</li>
        </ul>
        
        <!-- ⚠️ Gunakan arbitrary values dengan bijak -->
        <!-- Lebih baik tambahkan ke config jika dipakai berulang -->
        ```
        

---

### 🏗️ Checkpoint Level 4

text

```
✅ Checklist sebelum lanjut ke Level 5:

KONFIGURASI:
├── tailwind.config.js: brand colors, fonts, custom spacing, custom shadows
├── @layer base: html, ::selection, scrollbar
├── @layer components: .btn-primary, .btn-secondary, .card, .section-container
└── @layer utilities: .text-balance dan minimal 1 custom utility

DARK MODE:
├── darkMode: 'class' di config
├── Toggle function yang persist ke localStorage
├── Semua komponen ada dark: variant
├── Tidak ada "flash" saat pertama load (theme diterapkan sebelum render)
└── Konsistensi: background, teks, border, icon semua punya dark variant

PLUGIN:
├── @tailwindcss/typography aktif
├── Halaman blog/artikel menggunakan prose
├── @tailwindcss/forms aktif
└── Form elements konsisten di semua browser

ARBITRARY VALUES:
├── Digunakan maksimal 3-5 tempat (bukan untuk setiap nilai spesifik)
└── Nilai yang dipakai > 2 kali sudah di-config

Commit: feat: add brand config, dark mode, typography plugin, and form plugin
```

---

## 🔴 LEVEL 5: COMPONENT PATTERNS & PRODUCTION (Minggu 13-18)

> **Tema**: _"Dari satu halaman ke codebase yang scalable dan maintainable"_  
> **Benang Merah**: Landing page satu file (Level 1-4) → component patterns → conditional classes → aksesibilitas → performa → siap untuk tim  
> **Output**: Codebase yang terorganisir dengan component system, tests, dan CI

---

### M. Component Patterns — Mengelola Class Tailwind

> 💡 **Masalah yang diselesaikan**: Komponen `Button` dengan 20 class yang berulang di 50 tempat. Jika ingin mengubah radius, harus edit 50 tempat. Solusi: abstraksi yang tepat.

38. `[[38. clsx & tailwind-merge — Conditional Classes yang Aman]]`
    
    - _Langkah konkret_:
        
        Bash
        
        ```
        npm install clsx tailwind-merge
        ```
        
        JavaScript
        
        ```
        // src/utils/cn.js — utility function
        import { clsx } from 'clsx';
        import { twMerge } from 'tailwind-merge';
        
        // cn: merge classes + handle Tailwind conflicts
        export function cn(...inputs) {
          return twMerge(clsx(inputs));
        }
        
        // Contoh penggunaan:
        // Tanpa twMerge: 'px-4 px-6' → keduanya ada (konflik!)
        // Dengan twMerge: 'px-4 px-6' → hanya 'px-6' (yang terakhir menang)
        ```
        
        JavaScript
        
        ```
        // Button component dengan conditional classes
        function Button({ variant = 'primary', size = 'md', disabled, className, children }) {
          return `
            <button class="${cn(
              // Base classes
              'inline-flex items-center justify-center font-semibold rounded-xl transition-all',
              
              // Variant classes
              variant === 'primary' && 'bg-blue-600 text-white hover:bg-blue-700 shadow-brand',
              variant === 'secondary' && 'bg-white text-gray-700 border border-gray-200 hover:bg-gray-50',
              variant === 'ghost' && 'text-gray-600 hover:bg-gray-100 hover:text-gray-900',
              variant === 'destructive' && 'bg-red-600 text-white hover:bg-red-700',
              
              // Size classes
              size === 'sm' && 'text-xs px-3 py-1.5',
              size === 'md' && 'text-sm px-5 py-2.5',
              size === 'lg' && 'text-base px-8 py-3',
              
              // State classes
              disabled && 'opacity-50 cursor-not-allowed',
              
              // Caller override
              className
            )}">
              ${children}
            </button>
          `;
        }
        ```
        
39. `[[39. @apply untuk Komponen yang Benar-benar Berulang]]`
    
    - _Langkah konkret_: Kapan dan bagaimana menggunakan `@apply`:
        
        CSS
        
        ```
        /* ✅ GUNAKAN @apply untuk: */
        @layer components {
          /* Komponen yang dipakai > 5 kali dengan kombinasi class yang sama */
          .btn {
            @apply inline-flex items-center justify-center font-semibold rounded-xl transition-all;
          }
          
          .btn-primary {
            @apply btn bg-blue-600 text-white hover:bg-blue-700 shadow-brand px-6 py-2.5 text-sm;
          }
          
          .btn-sm {
            @apply btn bg-blue-600 text-white hover:bg-blue-700 px-4 py-1.5 text-xs;
          }
          
          /* Form input yang berulang di semua form */
          .form-input {
            @apply block w-full rounded-xl border-gray-200 px-4 py-3 text-sm
                   focus:border-blue-500 focus:ring-2 focus:ring-blue-500/20
                   dark:border-gray-700 dark:bg-gray-800 dark:text-white
                   transition-colors;
          }
        }
        
        /* ❌ HINDARI @apply untuk: */
        /* - One-off styles yang hanya dipakai sekali */
        /* - Menggabungkan terlalu banyak utilities (kalahkan tujuan Tailwind) */
        /* - Utility sederhana yang mudah dibaca langsung di HTML */
        ```
        

---

### N. Aksesibilitas dengan Tailwind

40. `[[40. sr-only & Focus Styles — Aksesibilitas yang Tidak Mengorbankan Desain]]`
    
    - _Langkah konkret_: Audit dan perbaiki aksesibilitas landing page:
        
        HTML
        
        ```
        <!-- Skip to main content link -->
        <a
          href="#main-content"
          class="sr-only focus:not-sr-only focus:absolute focus:top-4 focus:left-4 focus:z-50 focus:rounded-xl focus:bg-blue-600 focus:px-4 focus:py-2 focus:text-sm focus:font-semibold focus:text-white"
        >
          Skip to main content
        </a>
        
        <!-- Icon button dengan sr-only label -->
        <button
          class="rounded-full p-2 text-gray-500 hover:bg-gray-100 focus:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2"
          aria-label="Buka menu navigasi"
        >
          <svg class="h-5 w-5" aria-hidden="true"><!-- hamburger --></svg>
          <span class="sr-only">Menu</span>
        </button>
        
        <!-- Button group dengan aria -->
        <div role="group" aria-label="Social media links">
          <a href="https://twitter.com" class="..." aria-label="Twitter">
            <svg aria-hidden="true"><!-- twitter icon --></svg>
          </a>
          <a href="https://github.com" class="..." aria-label="GitHub">
            <svg aria-hidden="true"><!-- github icon --></svg>
          </a>
        </div>
        
        <!-- Kontras warna yang cukup -->
        <!-- ❌ Buruk: -->
        <p class="text-gray-300">Teks abu di background putih — kontras rendah</p>
        <!-- ✅ Baik: -->
        <p class="text-gray-600">Teks yang lebih gelap — kontras cukup</p>
        ```
        
41. `[[41. motion-reduce — Animasi yang Menghormati User]]`
    
    - _Langkah konkret_:
        
        HTML
        
        ```
        <!-- Animasi dengan fallback untuk reduced motion -->
        <div class="transition-transform duration-300 motion-reduce:transition-none hover:-translate-y-1">
          Card yang tidak bergerak jika user prefer reduced motion
        </div>
        
        <!-- Animasi yang dinonaktifkan untuk reduced motion -->
        <div class="animate-bounce motion-reduce:animate-none">
          Bounce indicator
        </div>
        
        <!-- Float animation: aktif normal, diam di reduced motion -->
        <img
          src="hero.png"
          class="animate-float motion-reduce:animate-none"
          alt="App illustration"
        >
        ```
        

---

### O. Performa & Tooling

42. `[[42. Prettier Plugin Tailwind — Class yang Selalu Rapi]]`
    
    - _Langkah konkret_: Setup ESLint + Prettier untuk Tailwind:
        
        Bash
        
        ```
        npm install -D prettier prettier-plugin-tailwindcss eslint-plugin-tailwindcss
        ```
        
        JSON
        
        ```
        // .prettierrc
        {
          "plugins": ["prettier-plugin-tailwindcss"],
          "tailwindConfig": "./tailwind.config.js",
          "singleQuote": true,
          "semi": true,
          "trailingComma": "es5"
        }
        ```
        
        JSON
        
        ```
        // .eslintrc.json
        {
          "plugins": ["tailwindcss"],
          "rules": {
            "tailwindcss/classnames-order": "warn",
            "tailwindcss/no-custom-classname": "warn",
            "tailwindcss/no-contradicting-classname": "error"
          }
        }
        ```
        
43. `[[43. Audit Performa — Ukur dan Optimasi]]`
    
    - _Langkah konkret_:
        
        Bash
        
        ```
        # Build untuk production
        npm run build
        
        # Cek ukuran CSS
        ls -lh dist/assets/*.css
        ```
        
        JavaScript
        
        ```
        // Pastikan content config benar agar tidak ada class yang hilang
        // tailwind.config.js
        content: [
          './index.html',
          './src/**/*.{js,ts,jsx,tsx,vue,svelte,astro}',
          // Jika pakai library yang punya class Tailwind:
          './node_modules/your-lib/**/*.js',
        ],
        ```
        
    - Target ukuran CSS: < 30KB (gzip)
    - _Langkah konkret_: Jalankan Lighthouse — target Performance ≥ 90

---

### 🏗️ Checkpoint Level 5

text

```
✅ Checklist sebelum lanjut ke Level 6:

COMPONENT PATTERNS:
├── cn() utility (clsx + tailwind-merge) aktif
├── Button component dengan variant dan size
├── @apply digunakan dengan benar (bukan berlebihan)
└── Tidak ada duplikasi class > 3 tempat tanpa abstraksi

AKSESIBILITAS:
├── Skip to main content link ada dan berfungsi
├── Semua icon-only button punya aria-label atau sr-only text
├── focus-visible digunakan (bukan focus saja)
├── Semua form input punya label yang terhubung
├── Kontras warna dicek: text-gray-600 di atas white = ✅
├── motion-reduce diimplementasikan untuk semua animasi
└── axe DevTools: 0 critical issues

PERFORMA:
├── Build CSS < 30KB (gzip)
├── Content config benar (tidak ada class hilang di production)
├── Lighthouse Performance ≥ 85, Accessibility ≥ 95
└── ESLint Tailwind: 0 errors

Commit: feat: add component patterns, accessibility, and performance optimization
```

---

## ⚫ LEVEL 6: ADVANCED — TAILWIND V4 & DESIGN SYSTEM (Minggu 18-24)

> **Tema**: _"Tailwind terbaru dan membangun design system yang reusable"_  
> **Benang Merah**: Tailwind v3 sudah dikuasai → pahami v4 (perubahan besar) → container queries → design system → expert-level

---

### P. Tailwind CSS v4 — Perubahan Paradigma

> 💡 **v4 adalah rewrite besar**: tidak ada lagi `tailwind.config.js`, konfigurasi pindah ke CSS file. JIT lebih cepat. Container queries built-in.

44. `[[44. Tailwind v4 — Migrasi dari v3]]`
    
    - _Langkah konkret_: Install dan setup v4:
        
        Bash
        
        ```
        npm install tailwindcss@next @tailwindcss/vite@next
        ```
        
        JavaScript
        
        ```
        // vite.config.js (v4)
        import tailwindcss from '@tailwindcss/vite';
        
        export default {
          plugins: [tailwindcss()],
        };
        ```
        
        CSS
        
        ```
        /* style.css (v4) — tidak ada tailwind.config.js! */
        @import "tailwindcss";
        
        /* Konfigurasi di CSS (bukan JS) */
        @theme {
          /* Brand colors */
          --color-brand-50: oklch(97% 0.02 260);
          --color-brand-500: oklch(60% 0.2 260);
          --color-brand-600: oklch(52% 0.22 260);
          --color-brand-700: oklch(44% 0.21 260);
          
          /* Font */
          --font-family-sans: 'Inter', system-ui, sans-serif;
          --font-family-display: 'Lexend', sans-serif;
          
          /* Custom spacing */
          --spacing-18: 4.5rem;
          --spacing-88: 22rem;
          
          /* Custom radius */
          --radius-4xl: 2rem;
          
          /* Custom shadow */
          --shadow-brand: 0 4px 14px 0 oklch(60% 0.2 260 / 0.25);
        }
        
        /* Custom variant */
        @variant dark (&:where([data-theme="dark"] *));
        ```
        
45. `[[45. Container Queries — Komponen yang Responsif terhadap Parent]]`
    
    - _Langkah konkret_: Buat card yang benar-benar responsif:
        
        HTML
        
        ```
        <!-- @tailwindcss/container-queries (v3) atau built-in (v4) -->
        
        <!-- Container yang jadi referensi ukuran -->
        <div class="@container">
          <!-- Card yang berubah berdasarkan ukuran container, bukan viewport! -->
          <div class="flex flex-col @md:flex-row gap-4 rounded-2xl border border-gray-100 bg-white p-4">
            <!-- Gambar: full width di small container, fixed di medium -->
            <img
              src="product.jpg"
              class="w-full rounded-xl object-cover @md:h-40 @md:w-40 flex-shrink-0"
              alt="Product"
            >
            <!-- Konten -->
            <div class="flex-1">
              <h3 class="font-semibold text-gray-900 @md:text-lg">Nama Produk</h3>
              <p class="mt-1 text-sm text-gray-500 @md:mt-2">Deskripsi singkat produk...</p>
              <!-- CTA: tersembunyi di small, tampil di medium -->
              <div class="mt-3 hidden @md:flex gap-2">
                <button class="text-sm font-medium text-blue-600">Detail</button>
                <button class="rounded-lg bg-blue-600 px-4 py-1.5 text-sm text-white">Beli</button>
              </div>
            </div>
          </div>
        </div>
        ```
        

---

### Q. Design System — Fondasi yang Scalable

46. `[[46. Merencanakan Design System dengan Tailwind]]`
    
    - _Langkah konkret_: Buat `design-system.html` sebagai living documentation:
        
        HTML
        
        ```
        <!DOCTYPE html>
        <html lang="id">
        <head>
          <title>SaaSku Design System</title>
          <link rel="stylesheet" href="/src/style.css">
        </head>
        <body class="bg-gray-50 dark:bg-gray-950 p-8">
          
          <!-- ===== COLORS ===== -->
          <section class="mb-16">
            <h2 class="mb-6 text-2xl font-bold text-gray-900 dark:text-white">Colors</h2>
            
            <!-- Brand palette -->
            <div class="mb-4">
              <h3 class="mb-3 text-sm font-semibold uppercase tracking-wider text-gray-500">Brand</h3>
              <div class="flex gap-2">
                <div class="flex flex-col items-center gap-1">
                  <div class="h-10 w-10 rounded-lg bg-brand-50"></div>
                  <span class="text-xs text-gray-500">50</span>
                </div>
                <!-- ... repeat untuk semua shade -->
              </div>
            </div>
          </section>
          
          <!-- ===== TYPOGRAPHY ===== -->
          <section class="mb-16">
            <h2 class="mb-6 text-2xl font-bold text-gray-900 dark:text-white">Typography</h2>
            <div class="space-y-4">
              <div class="flex items-baseline gap-4">
                <span class="w-24 text-xs text-gray-500">text-5xl</span>
                <p class="text-5xl font-extrabold text-gray-900 dark:text-white">Display</p>
              </div>
              <!-- ... repeat untuk semua sizes -->
            </div>
          </section>
          
          <!-- ===== COMPONENTS ===== -->
          <section class="mb-16">
            <h2 class="mb-6 text-2xl font-bold text-gray-900 dark:text-white">Buttons</h2>
            <div class="flex flex-wrap gap-3">
              <button class="btn-primary">Primary</button>
              <button class="btn-secondary">Secondary</button>
              <button class="btn-primary" disabled>Disabled</button>
            </div>
          </section>
          
        </body>
        </html>
        ```
        
47. `[[47. Tailwind Preset — Berbagi Konfigurasi Antar Project]]`
    
    - _Langkah konkret_: Buat preset yang bisa digunakan di banyak project:
        
        JavaScript
        
        ```
        // packages/saas-preset/index.js
        module.exports = {
          theme: {
            extend: {
              colors: {
                brand: { /* ... */ },
              },
              fontFamily: {
                sans: ['Inter', 'sans-serif'],
              },
              // ...
            },
          },
          plugins: [
            require('@tailwindcss/typography'),
            require('@tailwindcss/forms'),
          ],
        };
        
        // Di project yang menggunakan preset:
        // tailwind.config.js
        module.exports = {
          presets: [require('saas-preset')],
          // Override atau extend preset di sini
          theme: {
            extend: {
              // Override spesifik untuk project ini
            },
          },
        };
        ```
        

---

### 🏗️ Checkpoint Level 6

text

```
✅ Checklist sebelum lanjut ke Level 7:

TAILWIND V4:
├── Berhasil setup project dengan v4
├── Konfigurasi brand di @theme (bukan tailwind.config.js)
├── Custom variant dark mode dengan @variant
└── Pahami breaking changes dari v3 ke v4

CONTAINER QUERIES:
├── Minimal satu komponen menggunakan @container dan @md:
├── Komponen terlihat berbeda di sidebar (narrow) vs main (wide)
└── Pahami kapan container queries lebih baik dari media queries

DESIGN SYSTEM:
├── design-system.html sebagai living documentation
├── Semua warna brand terdokumentasi dengan swatch
├── Semua typography size terdokumentasi
├── Semua komponen utama terdokumentasi dengan semua varian
└── Preset package (bisa opsional jika single project)

Commit: feat: upgrade to Tailwind v4, add container queries, and design system docs
```

---

## 🟣 LEVEL 7: MASTERY — PROYEK SKALA BESAR (Minggu 24+)

> **Tema**: _"Mengintegrasikan semua yang dipelajari dalam project production-ready yang sesungguhnya"_

---

### Pilihan Jalur Project

48. `[[48. SaaS Dashboard Admin — Capstone Project A]]`
    
    - _Langkah konkret_: Bangun dashboard dengan Tailwind:
        
        text
        
        ```
        Pages:
        ├── dashboard.html    — stats cards, charts area, recent activities
        ├── analytics.html    — charts dan filter date range
        ├── users.html        — data table dengan search/filter/sort
        ├── settings.html     — form dengan semua input types
        └── login.html        — form login yang profesional
        
        Layout Pattern:
        ├── sidebar: fixed di desktop (260px), offcanvas/hidden di mobile
        ├── main: margin-left-0 mobile, margin-left-[260px] desktop
        ├── topbar: sticky, backdrop-blur, breadcrumb
        └── content: max-w-7xl dengan padding konsisten
        
        Components:
        ├── StatsCard: icon + angka + label + trend indicator
        ├── DataTable: sortable headers + pagination + row actions
        ├── Modal: confirm delete + add/edit form
        ├── Toast: success/error/info notification
        ├── Skeleton: loading state untuk semua section
        └── EmptyState: ilustrasi + CTA untuk tabel kosong
        ```
        
49. `[[49. E-commerce Product Pages — Capstone Project B]]`
    
    - _Langkah konkret_:
        
        text
        
        ```
        Pages:
        ├── home.html         — hero + categories + featured products
        ├── catalog.html      — product grid + filters sidebar + sort
        ├── product.html      — gallery + details + reviews + related
        ├── cart.html         — item list + order summary + promo
        └── checkout.html     — multi-step form dengan progress
        
        Advanced Patterns:
        ├── Image gallery: thumbnails + main image + zoom
        ├── Filter sidebar: accordion + checkboxes + range slider
        ├── Product card: hover overlay + quick add + wishlist
        ├── Review stars: interactive rating input
        └── Checkout wizard: step indicator + form validation
        ```
        
50. `[[50. Landing Page SaaS Profesional — Capstone Project C]]`
    
    - _Langkah konkret_:
        
        text
        
        ```
        Sections:
        ├── Navbar: transparent → solid on scroll, mobile menu
        ├── Hero: gradient + animated background + social proof
        ├── Logos: marquee/ticker of client logos
        ├── Features: bento grid + large cards
        ├── How it works: step-by-step dengan ilustrasi
        ├── Testimonials: card carousel + avatar + rating
        ├── Pricing: toggle monthly/annual + comparison table
        ├── FAQ: accordion
        ├── CTA: gradient banner
        └── Footer: mega footer dengan newsletter
        
        Teknis:
        ├── Intersection Observer untuk scroll animations
        ├── Dark mode yang sempurna di semua section
        ├── Performance: Lighthouse 95+ di semua kategori
        └── Aksesibilitas: WCAG AA compliant
        ```
        

---

## 📊 Ringkasan Progress Tracking

### Satu Project, 7 Level Enhancement

text

```
Level 1: Komponen dasar di Play + setup project lokal
  + Level 2: + Layout landing page (hero, features, pricing, footer)
  + Level 3: + Visual polish (gradient, shadow, animation, hover effects)
  + Level 4: + Brand config, dark mode, typography plugin
  + Level 5: + Component patterns, aksesibilitas, performa
  + Level 6: + Tailwind v4, container queries, design system
  + Level 7: + Capstone project (pilih jalur)
```

### Tabel Progress

|Level|Poin|Durasi|Output Konkret|
|---|---|---|---|
|🟢 **1**|1-13|Minggu 1-3|Komponen di Play + project lokal berjalan|
|🔵 **2**|14-23|Minggu 3-6|Landing page responsif (semua section)|
|🟡 **3**|24-30|Minggu 6-9|Visual polish: gradient, shadow, animasi|
|🟠 **4**|31-37|Minggu 9-13|Brand config, dark mode, plugin|
|🔴 **5**|38-43|Minggu 13-18|Component patterns, a11y, performa|
|⚫ **6**|44-47|Minggu 18-24|Tailwind v4, container queries, design system|
|🟣 **7**|48-50|Minggu 24+|Capstone project production-ready|

---

### Benang Merah Utama Sepanjang Roadmap

text

```
Poin 1  (Utility-first philosophy)  → Fondasi cara berpikir seluruh roadmap
Poin 2  (JIT & dynamic classes)     → Aturan yang tidak boleh dilanggar
Poin 3  (Tailwind Play)             → Playground untuk setiap eksperimen
Poin 5  (Prettier plugin)           → Konsistensi class di seluruh project
Poin 6  (Font size scale)           → Digunakan di semua typography
Poin 11 (Shade system 50-950)       → Pattern yang diulang untuk semua warna
Poin 14 (padding & margin)          → Konsistensi spacing di semua section
Poin 16 (Flexbox)                   → Navbar dan komponen linear
Poin 19 (CSS Grid)                  → Feature grid dan bento layout
Poin 23 (Responsive prefix)         → Mobile-first di semua komponen
Poin 27 (Hover + transition)        → Interaktivitas di semua komponen
Poin 28 (group & peer)              → Pola accordion dan floating label
Poin 31 (config extend)             → Brand yang konsisten di seluruh project
Poin 33 (dark mode)                 → dark: variant di semua komponen
Poin 38 (clsx + twMerge)           → Component system yang scalable
Poin 40 (aksesibilitas)             → sr-only, focus-visible di semua interaktif
Poin 44 (Tailwind v4)               → @theme menggantikan tailwind.config.js
Poin 45 (container queries)         → @container untuk komponen yang benar-benar responsif
```

---

## 💡 Cara Menggunakan Roadmap Ini

text

```
Setiap poin mengikuti format:
┌──────────────────────────────────────────────────────┐
│ 💡 Konteks: mengapa utility/pattern ini ada          │
│ 🔗 Benang Merah: koneksi ke poin sebelum/sesudah    │
│ 📋 Kode: HTML dengan class Tailwind yang konkret    │
│          langsung bisa dicopy ke Play atau project   │
│ ✅ Langkah konkret: verifikasi berhasil             │
└──────────────────────────────────────────────────────┘
```

**Aturan yang Tidak Boleh Dilanggar:**

1. **Test di Tailwind Play dulu** — sebelum tulis di project, eksperimen di Play
2. **Baca hover docs di IntelliSense** — setiap class punya preview nilai sebenarnya
3. **Jangan generate class secara dinamis** — selalu tulis class lengkap di source code
4. **Mobile-first always** — mulai dari tampilan mobile, tambahkan prefix untuk yang lebih besar
5. **Prettier setiap save** — class selalu terurut dan rapi
6. **Arbitrary values = last resort** — jika perlu > 2 kali, tambahkan ke config

---

_Roadmap Tailwind CSS v1.0 — Step-by-Step, Design Thinking First_  
_Setiap class adalah keputusan desain yang eksplisit — bukan magic number_