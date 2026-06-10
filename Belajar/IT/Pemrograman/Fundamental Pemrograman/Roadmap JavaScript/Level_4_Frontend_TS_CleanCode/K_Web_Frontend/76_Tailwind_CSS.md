# 76. Tailwind CSS — Utility-First CSS

**Benang Merah**: Dari Materi 75 (CSS murni) kita lihat bahwa CSS bisa jadi panjang dan berulang. Tailwind hadir sebagai **CSS framework utility-first** — tinggal rakit tanpa nulis CSS kustom. Lanjut ke Materi 77 (DOM—sudah ada).

---

## Penjelasan

Tailwind CSS adalah **utility-first CSS framework** — alih-alih menulis CSS kustom, Anda menggunakan **class-class utility** langsung di HTML. Misalnya `padding: 16px` → `p-4`, `display: flex` → `flex`.

| Konsep | CSS Biasa | Tailwind |
|---|---|---|
| Padding 16px | `padding: 16px;` | `p-4` |
| Flex container | `display: flex;` | `flex` |
| Teks putih | `color: white;` | `text-white` |
| Border radius 8px | `border-radius: 8px;` | `rounded-lg` |

### Kenapa Tailwind?
- **Developer experience** — tidak perlu ganti file, ganti konteks
- **Konsisten** — spacing, warna, typography sudah ditentukan
- **Kecil di production** — PurgeCSS buang CSS yang tidak terpakai
- **Responsive** — `md:flex`, `lg:grid-cols-3`

---

## Fungsi

- **Mempercepat development** — styling langsung di HTML
- **Konsistensi desain** — tidak ada nilai random (semua dari design system)
- **Bundle kecil** — hanya utility yang dipakai
- **Responsive mudah** — prefix `sm:`, `md:`, `lg:`, `xl:`

---

## Cara Pengimplementasian

### 1. Setup dengan Vite

```bash
# Buat project
npm create vite@latest perpustakaan-tailwind -- --template vanilla
cd perpustakaan-tailwind

# Install Tailwind
npm install -D tailwindcss @tailwindcss/vite
```

**vite.config.js**:
```javascript
import { defineConfig } from 'vite'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [tailwindcss()],
})
```

**src/style.css**:
```css
@import "tailwindcss";
```

### 2. Utility Classes — Spacing

```html
<!-- Padding: p-{size} (0.25rem per level) -->
<div class="p-0">p-0 = 0px</div>      <!-- p-0 -->
<div class="p-1">p-1 = 4px</div>       <!-- p-1 -->
<div class="p-4">p-4 = 16px</div>      <!-- p-4 -->
<div class="p-8">p-8 = 32px</div>      <!-- p-8 -->

<!-- Margin: m-{size} -->
<div class="m-4">Margin 16px</div>
<div class="mx-auto">Margin horizontal auto (center)</div>

<!-- Gap -->
<div class="flex gap-4">gap-4 = 16px antar item</div>
```

### 3. Utility Classes — Typography

```html
<p class="text-xs">text-xs = 0.75rem</p>
<p class="text-sm">text-sm = 0.875rem</p>
<p class="text-base">text-base = 1rem</p>
<p class="text-lg">text-lg = 1.125rem</p>
<p class="text-xl">text-xl = 1.25rem</p>
<p class="text-2xl">text-2xl</p>

<!-- Berat & warna -->
<p class="font-bold text-blue-600">Bold biru</p>
<p class="font-light text-gray-500">Light abu-abu</p>
<p class="text-center text-red-500">Teks tengah merah</p>
```

### 4. Utility Classes — Flexbox & Grid

```html
<!-- Flex -->
<div class="flex justify-between items-center">
  <div>Item 1</div>
  <div>Item 2</div>
</div>

<div class="flex flex-wrap gap-4">
  <div class="flex-1">flex-1 = grow</div>
  <div class="flex-1">flex-1</div>
</div>

<!-- Grid -->
<div class="grid grid-cols-3 gap-6">
  <div class="bg-gray-100 p-4 rounded">Card 1</div>
  <div class="bg-gray-100 p-4 rounded">Card 2</div>
  <div class="bg-gray-100 p-4 rounded">Card 3</div>
</div>

<!-- Auto-fill responsive -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
  <!-- Cards -->
</div>
```

### 5. Halaman Perpustakaan dengan Tailwind

```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Perpustakaan — Tailwind</title>
  <link rel="stylesheet" href="/src/style.css">
</head>
<body class="bg-gray-100 min-h-screen grid grid-rows-[auto_auto_1fr_auto]">

  <!-- Header -->
  <header class="bg-slate-800 text-white text-center py-8 px-4">
    <h1 class="text-3xl font-bold">Perpustakaan Online</h1>
    <p class="text-slate-300 mt-2">Temukan buku favorit Anda</p>
  </header>

  <!-- Nav -->
  <nav class="bg-slate-700 flex justify-center gap-6 py-3 px-4">
    <a href="#" class="text-white hover:text-blue-300 transition px-3 py-1 rounded hover:bg-slate-600">Beranda</a>
    <a href="#" class="text-white hover:text-blue-300 transition px-3 py-1 rounded hover:bg-slate-600">Koleksi</a>
    <a href="#" class="text-white hover:text-blue-300 transition px-3 py-1 rounded hover:bg-slate-600">Tentang</a>
  </nav>

  <!-- Main -->
  <main class="max-w-6xl mx-auto w-full p-6">
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">

      <!-- Card 1 -->
      <div class="bg-white rounded-xl p-6 shadow-md hover:shadow-lg hover:-translate-y-1 transition-all">
        <h3 class="font-semibold text-lg text-slate-800">JavaScript: The Good Parts</h3>
        <p class="text-slate-600 mt-1">Douglas Crockford</p>
        <span class="inline-block bg-blue-500 text-white text-sm px-3 py-0.5 rounded-full mt-3">Pemrograman</span>
      </div>

      <!-- Card 2 -->
      <div class="bg-white rounded-xl p-6 shadow-md hover:shadow-lg hover:-translate-y-1 transition-all">
        <h3 class="font-semibold text-lg text-slate-800">Clean Code</h3>
        <p class="text-slate-600 mt-1">Robert C. Martin</p>
        <span class="inline-block bg-blue-500 text-white text-sm px-3 py-0.5 rounded-full mt-3">Pemrograman</span>
      </div>

      <!-- Card 3 -->
      <div class="bg-white rounded-xl p-6 shadow-md hover:shadow-lg hover:-translate-y-1 transition-all">
        <h3 class="font-semibold text-lg text-slate-800">HTML & CSS</h3>
        <p class="text-slate-600 mt-1">Jon Duckett</p>
        <span class="inline-block bg-green-500 text-white text-sm px-3 py-0.5 rounded-full mt-3">Web Design</span>
      </div>

    </div>
  </main>

  <!-- Footer -->
  <footer class="bg-slate-800 text-white text-center py-4">
    <p>&copy; 2026 Perpustakaan Online</p>
  </footer>

</body>
</html>
```

---

## Analogi: Membangun Rumah (Furnitur IKEA)

| Tailwind | IKEA / Furnitur Siap Pakai |
|---|---|
| `p-4` = padding 16px | Meja ukuran 120x60cm — tinggal pakai |
| `flex` | Rel pintu geser — tinggal pasang |
| `grid-cols-3` | Rak 3 kolom — tinggal susun |
| `text-blue-600` | Cat warna "Biru Laut" dari katalog |
| `rounded-lg` | Sudut meja yang sudah dibulatkan |
| `shadow-md` | Lampu dengan efek bayangan built-in |
| `hover:bg-gray-100` | Saklar yang otomatis nyalakan lampu saat disentuh |
| `md:grid-cols-2` | Meja lipat yang muat di ruangan kecil |
| CSS kustom (`style.css`) | Membuat furnitur dari kayu mentah — butuh tukang |
| Tailwind utility classes | Furnitur IKEA — tinggal rakit, langsung jadi |

**CSS murni** = Anda seorang **tukang kayu** — Anda punya kayu mentah, paku, cat. Bisa bikin apa saja, tapi butuh waktu dan keahlian.

**Tailwind** = Anda pergi ke **IKEA** — semua potongan sudah siap, tinggal ikut manual, rakit, jadi. Lebih cepat, konsisten, dan hasilnya rapi.

---

## Dipakai Untuk Apa

- **Prototype cepat** — bikin halaman dalam hitungan menit
- **Startup / MVP** — kecepatan development prioritas
- **Tim besar** — konsistensi desain tanpa bike-shedding
- **Project dengan designer** — design system Tailwind sebagai acuan

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Class terlalu panjang | `<div class="flex items-center justify-between p-4 bg-white rounded-lg shadow-md hover:shadow-lg transition-all duration-300 ...">` | HTML susah dibaca (tapi ini normal di Tailwind) |
| Tidak pakai PurgeCSS | Semua utility ikut di bundle | File CSS besar |
| Override Tailwind di `style.css` | `p-4` lalu `padding: 10px !important` | Seharusnya pakai config |
| Tidak kenal utility yang ada | Nulis `margin-top: 12px` di CSS | Padahal ada `mt-3` |

---

## Hubungan dengan Materi Sebelumnya

- Materi 75 (CSS murni) → Tailwind adalah abstraksi di atas CSS — utility class menggantikan CSS kustom
- Materi 77 (DOM) → Tailwind class bisa dimanipulasi via `classList`
- Materi 78 (Event) → `hover:` dan `focus:` di Tailwind adalah event handling dasar
- Tailwind tidak menggantikan pengetahuan CSS — Anda tetap perlu paham Flexbox, Grid, dll

---

## Soal Latihan

### Soal 1 (Mudah)
Buat tombol dengan Tailwind: background biru, teks putih, padding 12px 24px, border radius, hover jadi biru lebih gelap.

**Jawaban**:
```html
<button class="bg-blue-500 text-white px-6 py-3 rounded-lg hover:bg-blue-700 transition">
  Klik Saya
</button>
```

### Soal 2 (Sedang)
Buat card profile dengan Tailwind: gambar profil (bulat, 64px), nama (bold), deskripsi (abu), footer (2 tombol). Card di tengah halaman.

**Jawaban**:
```html
<div class="min-h-screen bg-gray-100 flex items-center justify-center p-4">
  <div class="bg-white rounded-xl p-6 shadow-lg max-w-sm text-center">
    <img src="https://via.placeholder.com/64"
         alt="Profil"
         class="w-16 h-16 rounded-full mx-auto">

    <h2 class="text-xl font-bold mt-4">Andi Pratama</h2>
    <p class="text-gray-500 mt-1">Frontend Developer</p>

    <div class="flex gap-3 mt-6 justify-center">
      <button class="bg-blue-500 text-white px-4 py-2 rounded-lg hover:bg-blue-600 transition">
        Ikuti
      </button>
      <button class="border border-gray-300 px-4 py-2 rounded-lg hover:bg-gray-50 transition">
        Pesan
      </button>
    </div>
  </div>
</div>
```

### Soal 3 (Tantangan)
Buat halaman landing page sederhana dengan Tailwind: navbar (fixed top), hero section (judul, deskripsi, CTA button), features grid (3 kolom), footer. Responsive (mobile: 1 kolom).

**Jawaban**:
```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Landing Page</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body>

  <!-- Navbar -->
  <nav class="fixed top-0 w-full bg-white shadow-md py-4 px-6 flex justify-between items-center z-10">
    <h1 class="text-xl font-bold text-blue-600">MyApp</h1>
    <div class="flex gap-6">
      <a href="#" class="text-gray-600 hover:text-blue-600">Fitur</a>
      <a href="#" class="text-gray-600 hover:text-blue-600">Harga</a>
      <a href="#" class="bg-blue-500 text-white px-4 py-2 rounded-lg hover:bg-blue-600">Daftar</a>
    </div>
  </nav>

  <!-- Hero -->
  <section class="min-h-screen flex flex-col items-center justify-center text-center px-6 bg-gradient-to-br from-blue-50 to-blue-100">
    <h2 class="text-4xl md:text-5xl font-bold text-gray-900 max-w-2xl">
      Bangun Aplikasi Lebih Cepat
    </h2>
    <p class="text-gray-600 mt-4 max-w-xl text-lg">
      Dengan Tailwind CSS, styling jadi cepat dan konsisten.
    </p>
    <button class="mt-8 bg-blue-500 text-white px-8 py-3 rounded-lg text-lg hover:bg-blue-600 transition shadow-lg">
      Mulai Sekarang
    </button>
  </section>

  <!-- Features -->
  <section class="py-16 px-6 max-w-5xl mx-auto">
    <h3 class="text-2xl font-bold text-center mb-10">Fitur Unggulan</h3>
    <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
      <div class="bg-white p-6 rounded-xl shadow-md text-center">
        <div class="text-4xl mb-4">🚀</div>
        <h4 class="font-semibold">Cepat</h4>
        <p class="text-gray-500 mt-2">Build time cepat dengan PurgeCSS</p>
      </div>
      <div class="bg-white p-6 rounded-xl shadow-md text-center">
        <div class="text-4xl mb-4">🎨</div>
        <h4 class="font-semibold">Konsisten</h4>
        <p class="text-gray-500 mt-2">Design system built-in</p>
      </div>
      <div class="bg-white p-6 rounded-xl shadow-md text-center">
        <div class="text-4xl mb-4">📱</div>
        <h4 class="font-semibold">Responsive</h4>
        <p class="text-gray-500 mt-2">Mobile-first utility classes</p>
      </div>
    </div>
  </section>

  <!-- Footer -->
  <footer class="bg-gray-800 text-white text-center py-6">
    <p>&copy; 2026 MyApp. All rights reserved.</p>
  </footer>

</body>
</html>
```
