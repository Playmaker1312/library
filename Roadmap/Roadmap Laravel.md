# oadmap Laravel: Step-by-Step Membangun Aplikasi Web Nyata

## Filosofi Roadmap Ini

> **"Laravel bukan sekadar framework — Laravel adalah opinionated way of building PHP applications yang mengutamakan developer happiness tanpa mengorbankan kualitas"** — setiap konsep yang dipelajari ada alasannya, bukan sekadar hafal method.

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: sistem manajemen perpustakaan dari Laravel install → web app → API → production-ready
- **Security dari Hari Pertama**: bukan topik lanjutan, tapi mindset yang dibangun sejak awal
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **Laravel Modern**: fokus pada Laravel 11.x features, bukan cara lama yang sudah deprecated
- **Mengapa sebelum Bagaimana**: pahami alasan di balik setiap keputusan desain Laravel

### Prasyarat Sebelum Memulai

text

```
Sebelum roadmap ini, pastikan sudah memahami:
├── PHP dasar hingga menengah (variabel, array, fungsi, OOP)
├── HTML & CSS dasar
├── SQL dasar (SELECT, INSERT, UPDATE, DELETE, JOIN)
├── Command line / terminal dasar
├── Git version control dasar
├── Composer sebagai dependency manager PHP
└── Konsep HTTP (request, response, method, status code)
```

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: "Laravel Pertama" — install, konfigurasi, routing, Blade, Artisan
    ↓ (enhance)
Level 2: + Controller, View lengkap → Halaman katalog perpustakaan
    ↓ (enhance)
Level 3: + Database, Eloquent, Migration → Data buku dari MySQL
    ↓ (enhance)
Level 4: + Form, Validasi, Auth → Admin panel perpustakaan
    ↓ (enhance)
Level 5: + REST API, Sanctum, Queue, Event → API perpustakaan
    ↓ (enhance)
Level 6: + Testing, Service Container, Caching → Production-ready
    ↓ (enhance)
Level 7: + Arsitektur lanjutan, DevOps, Ekosistem → Enterprise-grade
```

---

## 🟢 LEVEL 1: FONDASI LARAVEL (Minggu 1-4)

> **Tema**: _"Dari instalasi hingga halaman pertama yang berjalan di Laravel"_  
> **Benang Merah**: Cara Laravel bekerja → Install → Konfigurasi → Routing → Blade → Artisan  
> **Output**: Halaman landing perpustakaan yang dibangun dengan Laravel, routing, dan Blade

---

### A. Memahami Laravel dan Cara Kerjanya

> 💡 **Mengapa dimulai di sini?** Sebelum menulis kode, pahami dulu _mengapa_ Laravel ada dan _bagaimana_ request diproses. Ini mencegah kebingungan "kenapa harus pakai route?" atau "apa bedanya controller dengan function biasa?".

text

```
Benang Merah Bagian A:
PHP murni sudah dipahami (prasyarat) →
Laravel: framework yang menambahkan struktur di atas PHP →
Request masuk → bootstrap → route → middleware → controller → response →
Service Container: jantung Laravel yang mengelola dependency →
Setup environment → file pertama → lihat hasilnya di browser
```

#### [[1. Laravel adalah Framework PHP — Bukan Magic, tapi Struktur]]

- Laravel bukan bahasa baru — ini PHP dengan struktur yang sangat opinionated
- **Yang Laravel tambahkan di atas PHP murni:**

text

```
PHP Murni                    Laravel
─────────────────────────    ──────────────────────────────
Router manual (switch)    →  Route::get('/buku', ...)
Query string manual       →  Eloquent ORM
$_POST manual             →  Request $request (type-hinted)
echo json_encode()        →  response()->json()
session_start() manual    →  Session facade
include/require manual    →  Autoloading PSR-4 via Composer
```

- **Alur request di Laravel** — pahami ini, semua hal lain akan masuk akal:

text

```
Browser kirim request ke /buku
        ↓
public/index.php          ← satu-satunya file yang diakses web server
        ↓
bootstrap/app.php         ← inisialisasi Application & Service Providers
        ↓
HTTP Kernel               ← pipeline middleware global
        ↓
Route Matching            ← routes/web.php atau routes/api.php
        ↓
Route Middleware           ← middleware spesifik route
        ↓
Controller@method          ← logika aplikasi
        ↓
Response                  ← HTML / JSON / redirect
        ↓
Browser menerima response
```

- _Langkah konkret_: Buka `public/index.php` di project Laravel — lihat hanya ada beberapa baris, tapi itu entry point seluruh aplikasi

#### [[2. Filosofi Laravel — Convention over Configuration]]

- Laravel membuat keputusan default yang masuk akal sehingga kamu tidak perlu konfigurasi hal-hal kecil
- **Contoh convention yang harus dipahami sejak awal:**

text

```
Convention                              Artinya
──────────────────────────────────────  ──────────────────────────────────
Model Buku                           →  tabel: buku (otomatis, plural snake_case)
Model AnggotaPerpustakaan            →  tabel: anggota_perpustakaans
kolom created_at & updated_at        →  dikelola Laravel otomatis
method index() di BukuController     →  untuk GET /buku (resource convention)
file BukuController.php              →  di app/Http/Controllers/
file buku/index.blade.php            →  di resources/views/buku/
```

- **Mengapa ini penting?** Mengikuti convention = kode lebih mudah dibaca orang lain dan framework bekerja tanpa konfigurasi ekstra

---

### B. Instalasi dan Konfigurasi

> 💡 **Benang Merah ke A**: Setelah paham apa itu Laravel, sekarang kita setup environment dan buat project pertama.

text

```
Benang Merah Bagian B:
Paham cara Laravel bekerja (A) →
Install tools yang dibutuhkan →
Buat project baru →
Konfigurasi .env untuk database →
Jalankan server development →
Verifikasi instalasi berhasil
```

#### [[3. Instalasi Laravel — Setup Project Perpustakaan]]

- **Prasyarat sistem:**

Bash

```
# Cek versi yang dibutuhkan
php --version    # minimal PHP 8.2
composer --version
node --version   # untuk asset bundling
```

- **Cara install** (pilih salah satu):

Bash

```
# Cara 1: Laravel Installer (rekomendasi)
composer global require laravel/installer
laravel new perpustakaan

# Cara 2: via Composer create-project
composer create-project laravel/laravel perpustakaan

# Pilih stack saat ditanya (untuk project ini):
# - Starter kit: None (kita buat manual untuk belajar)
# - Testing framework: Pest
# - Database: MySQL
```

- **Setup environment lokal:**

Bash

```
# Windows: gunakan Laragon
# Buat folder di: C:\laragon\www\perpustakaan\
# Akses: http://perpustakaan.test

# macOS: gunakan Herd
# Otomatis detect folder di ~/Herd/

# Semua OS: Laravel Sail (Docker)
# php artisan sail:install
# ./vendor/bin/sail up
```

- _Langkah konkret_: Jalankan `php artisan serve` → buka `http://localhost:8000` → pastikan halaman welcome Laravel muncul

#### [[4. Struktur Direktori Laravel — Setiap Folder Ada Tujuannya]]

text

```
perpustakaan/
├── app/                    ← Kode aplikasi utama
│   ├── Http/
│   │   ├── Controllers/    ← Controller class
│   │   ├── Middleware/     ← Middleware class
│   │   └── Requests/       ← Form Request validation
│   ├── Models/             ← Eloquent model
│   └── Providers/          ← Service Provider
│
├── bootstrap/
│   └── app.php             ← Inisialisasi aplikasi (di sini daftarkan middleware, dll)
│
├── config/                 ← File konfigurasi (app.php, database.php, mail.php, dll)
│
├── database/
│   ├── migrations/         ← Blueprint struktur tabel
│   ├── seeders/            ← Data awal untuk database
│   └── factories/          ← Generator data palsu untuk testing
│
├── public/                 ← Satu-satunya folder yang diekspos ke web
│   └── index.php           ← Entry point (jangan taruh file lain di sini sembarangan)
│
├── resources/
│   ├── views/              ← File Blade template (.blade.php)
│   ├── css/                ← Source CSS (dikompilasi Vite)
│   └── js/                 ← Source JavaScript (dikompilasi Vite)
│
├── routes/
│   ├── web.php             ← Route untuk web (punya session, CSRF)
│   ├── api.php             ← Route untuk API (stateless, prefix /api)
│   └── console.php         ← Route untuk Artisan command
│
├── storage/                ← File yang dihasilkan aplikasi (log, cache, upload)
├── tests/                  ← File test (Unit dan Feature)
├── .env                    ← Konfigurasi environment (JANGAN commit ke git!)
├── .env.example            ← Template .env (commit ini ke git)
├── artisan                 ← CLI tool Laravel
└── composer.json           ← Dependency PHP
```

- **Yang paling sering disentuh saat development:**

text

```
routes/web.php           → definisikan URL
app/Http/Controllers/    → logika request
resources/views/         → tampilan HTML
database/migrations/     → struktur database
app/Models/              → interaksi database
.env                     → konfigurasi environment
```

#### [[5. Konfigurasi .env — Environment yang Benar]]

Bash

```
# .env — konfigurasi untuk environment spesifik
# JANGAN commit file ini ke git (sudah ada di .gitignore default Laravel)

APP_NAME="Perpustakaan Digital"
APP_ENV=local            # local | staging | production
APP_KEY=                 # generate dengan: php artisan key:generate
APP_DEBUG=true           # TRUE di local, FALSE di production!
APP_URL=http://localhost:8000

# Database
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=perpustakaan_db
DB_USERNAME=root
DB_PASSWORD=

# Cache & Session
CACHE_STORE=file         # file | redis | memcached
SESSION_DRIVER=file      # file | cookie | database | redis
SESSION_LIFETIME=120     # menit

# Queue
QUEUE_CONNECTION=sync    # sync (langsung) | database | redis

# Mail
MAIL_MAILER=log          # log (dev) | smtp | mailgun | ses
```

Bash

```
# Generate APP_KEY — WAJIB sebelum apapun
php artisan key:generate

# Buat database
# Di phpMyAdmin atau MySQL CLI:
# CREATE DATABASE perpustakaan_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

# Test koneksi database
php artisan db:show

# Clear dan cache config
php artisan config:clear
php artisan config:cache  # hanya untuk production
```

- _Langkah konkret_: Setup `.env`, buat database, jalankan `php artisan db:show` — pastikan menampilkan info database tanpa error

#### [[6. Artisan CLI — Swiss Army Knife Laravel]]

> 💡 **Mengapa Artisan?** Artisan mengotomasi tugas repetitif. Buat file dengan Artisan, bukan manual — karena Artisan membuat file dengan _boilerplate yang benar dan di lokasi yang benar_.

Bash

```
# Lihat semua perintah yang tersedia
php artisan list

# Bantuan untuk perintah tertentu
php artisan help make:controller

# Perintah yang paling sering digunakan:
php artisan serve                    # Jalankan development server
php artisan tinker                   # REPL interaktif (sangat berguna!)
php artisan route:list               # Lihat semua route yang terdaftar
php artisan migrate                  # Jalankan migration
php artisan db:seed                  # Jalankan seeder

# Generate file
php artisan make:controller BukuController --resource
php artisan make:model Buku --migration --factory --seeder
php artisan make:middleware CekStatusAnggota
php artisan make:request StoreBukuRequest
php artisan make:policy BukuPolicy --model=Buku

# Maintenance
php artisan optimize                 # Cache semua untuk production
php artisan optimize:clear           # Clear semua cache
php artisan about                    # Info aplikasi Laravel
```

- **Tinker — REPL yang sangat berguna untuk eksperimen:**

Bash

```
php artisan tinker

# Di dalam tinker:
>>> App\Models\Buku::all()          # ambil semua buku
>>> App\Models\Buku::count()        # hitung buku
>>> App\Models\Buku::find(1)        # cari buku id 1
>>> App\Models\Buku::create(['judul' => 'Test', ...])
>>> DB::table('buku')->get()        # query langsung
>>> route('buku.index')             # generate URL dari nama route
```

- _Langkah konkret_: Jalankan `php artisan about` dan `php artisan route:list` — pahami output masing-masing perintah

---

### C. Routing — Mendefinisikan URL Aplikasi

> 💡 **Benang Merah ke B**: Project sudah berjalan. Sekarang kita definisikan URL mana yang bisa diakses dan apa yang terjadi saat URL tersebut diakses.

text

```
Benang Merah Bagian C:
Laravel berjalan (B) →
Route: mapping URL ke handler →
Closure untuk logika sederhana →
Controller untuk logika yang lebih kompleks →
Named route: URL bisa berubah, nama tidak →
Route group: DRY untuk prefix dan middleware →
Resource route: CRUD dengan satu baris
```

#### [[7. Route Dasar — Mapping URL ke Handler]]

PHP

```
<?php
// routes/web.php

use Illuminate\Support\Facades\Route;

// GET request ke / → tampilkan welcome page
Route::get('/', function () {
    return view('welcome');
});

// GET request ke /tentang → tampilkan halaman tentang
Route::get('/tentang', function () {
    return view('tentang', [
        'namaPerpustakaan' => 'Perpustakaan Digital Kota',
        'tahunBerdiri'     => 1995,
        'jumlahKoleksi'    => 12500,
    ]);
});

// POST request → untuk form submission
Route::post('/kontak', function (Illuminate\Http\Request $request) {
    // $request berisi semua data dari form
    return redirect('/')->with('sukses', 'Pesan terkirim!');
});

// Route dengan parameter wajib
Route::get('/buku/{id}', function (string $id) {
    return "Detail buku dengan ID: " . htmlspecialchars($id);
});

// Route dengan parameter opsional (ada default value)
Route::get('/buku/kategori/{kategori?}', function (string $kategori = 'semua') {
    return "Kategori: " . htmlspecialchars($kategori);
});

// Constraint: id harus angka, slug hanya huruf/angka/dash
Route::get('/buku/{id}', function (int $id) {
    return "Buku ID: $id";
})->where('id', '[0-9]+');

Route::get('/artikel/{slug}', function (string $slug) {
    return "Artikel: $slug";
})->where('slug', '[a-z0-9\-]+');
```

#### [[8. Named Route — URL yang Fleksibel]]

PHP

```
<?php
// Named route: beri nama pada route
Route::get('/buku', [BukuController::class, 'index'])->name('buku.index');
Route::get('/buku/{id}', [BukuController::class, 'show'])->name('buku.show');
Route::get('/buku/tambah', [BukuController::class, 'create'])->name('buku.create');

// Mengapa named route?
// Jika URL berubah dari /buku ke /koleksi-buku,
// kamu hanya ubah di routes/web.php, tidak perlu ubah di seluruh view!

// Cara generate URL dari nama route:
route('buku.index')           // → http://localhost:8000/buku
route('buku.show', ['id' => 5]) // → http://localhost:8000/buku/5
route('buku.show', 5)         // → shortcut jika hanya satu parameter

// Di Blade:
// <a href="{{ route('buku.index') }}">Katalog Buku</a>
// <a href="{{ route('buku.show', $buku->id) }}">{{ $buku->judul }}</a>

// Redirect ke named route:
return redirect()->route('buku.index');
return redirect()->route('buku.show', ['id' => $buku->id]);
```

#### [[9. Resource Route — CRUD dengan Satu Baris]]

PHP

```
<?php
// Resource route: otomatis buat 7 route CRUD standar
Route::resource('buku', BukuController::class);

// Equivalen dengan:
// GET    /buku              → index()   (daftar semua buku)
// GET    /buku/create       → create()  (form tambah buku)
// POST   /buku              → store()   (simpan buku baru)
// GET    /buku/{buku}       → show()    (detail satu buku)
// GET    /buku/{buku}/edit  → edit()    (form edit buku)
// PUT    /buku/{buku}       → update()  (update buku)
// DELETE /buku/{buku}       → destroy() (hapus buku)

// Verifikasi dengan:
// php artisan route:list --path=buku

// Jika tidak butuh semua:
Route::resource('buku', BukuController::class)
     ->only(['index', 'show']);           // hanya index dan show

Route::resource('buku', BukuController::class)
     ->except(['destroy']);              // semua kecuali destroy

// API resource: tanpa create() dan edit() (tidak perlu form)
Route::apiResource('buku', Api\BukuController::class);
```

#### [[10. Route Group — Organisasi Route yang Rapi]]

PHP

```
<?php
// Group dengan prefix URL
Route::prefix('admin')->group(function () {
    Route::get('/', [Admin\DashboardController::class, 'index'])->name('admin.dashboard');
    Route::resource('buku', Admin\BukuController::class)->names('admin.buku');
    Route::resource('anggota', Admin\AnggotaController::class)->names('admin.anggota');
});
// Menghasilkan: /admin/, /admin/buku, /admin/anggota

// Group dengan middleware
Route::middleware(['auth'])->group(function () {
    Route::get('/profil', [ProfilController::class, 'show'])->name('profil.show');
    Route::put('/profil', [ProfilController::class, 'update'])->name('profil.update');
});

// Group dengan prefix DAN middleware DAN name prefix
Route::prefix('admin')
     ->middleware(['auth', 'role:admin'])
     ->name('admin.')
     ->group(function () {
         Route::get('/dashboard', [Admin\DashboardController::class, 'index'])
              ->name('dashboard');  // → nama: admin.dashboard
         Route::resource('buku', Admin\BukuController::class);
         // → nama: admin.buku.index, admin.buku.show, dst.
     });

// Nested group
Route::prefix('api/v1')
     ->middleware('api')
     ->name('api.v1.')
     ->group(function () {
         Route::apiResource('buku', Api\V1\BukuController::class);
         Route::apiResource('anggota', Api\V1\AnggotaController::class);
     });
```

---

### D. Blade Templating Engine

> 💡 **Benang Merah ke C**: Route sudah definisikan URL. Sekarang kita buat tampilan (view) yang dikembalikan oleh route/controller. Blade adalah PHP template engine yang lebih ekspresif dan lebih aman.

text

```
Benang Merah Bagian D:
Route mengembalikan view() (C) →
Blade: template engine yang compile ke PHP murni →
Layout: satu template induk, banyak halaman →
Komponen: UI yang reusable →
Direktif: if, foreach, auth, can → logika di template
```

#### [[11. Blade Dasar — Syntax yang Lebih Aman dari PHP Murni]]

PHP

```
{{-- resources/views/buku/index.blade.php --}}

{{-- OUTPUT VARIABEL: selalu escaped (aman dari XSS) --}}
{{ $buku->judul }}
{{-- Menghasilkan: htmlspecialchars($buku->judul, ENT_QUOTES) secara otomatis --}}

{{-- Jika BENAR-BENAR perlu output raw HTML (hati-hati XSS!) --}}
{!! $buku->deskripsiHtml !!}
{{-- Gunakan ini HANYA jika konten sudah dipastikan aman (dari admin, bukan user) --}}

{{-- Komentar Blade: tidak akan muncul di HTML hasil --}}
{{-- Ini komentar Blade, tidak tampil di page source --}}

{{-- Kondisional --}}
@if($buku->stok > 0)
    <span class="badge bg-success">Tersedia ({{ $buku->stok }})</span>
@elseif($buku->stok === 0)
    <span class="badge bg-danger">Stok Habis</span>
@else
    <span class="badge bg-secondary">Status Tidak Diketahui</span>
@endif

{{-- unless: kebalikan @if --}}
@unless($buku->tersedia())
    <p>Buku ini sedang tidak tersedia untuk dipinjam.</p>
@endunless

{{-- Loop --}}
@foreach($katalog as $buku)
    <tr>
        <td>{{ $loop->iteration }}</td>   {{-- 1, 2, 3, ... (bukan index 0-based) --}}
        <td>{{ $buku->judul }}</td>
        <td>{{ $buku->pengarang }}</td>
        <td>
            @if($loop->first) {{-- elemen pertama --}} @endif
            @if($loop->last)  {{-- elemen terakhir --}} @endif
            {{-- $loop->index: 0-based index --}}
            {{-- $loop->count: total elemen --}}
            {{-- $loop->remaining: sisa elemen --}}
        </td>
    </tr>
@endforeach

{{-- forelse: jika array kosong --}}
@forelse($katalog as $buku)
    <p>{{ $buku->judul }}</p>
@empty
    <p>Belum ada buku dalam katalog.</p>
@endforelse

{{-- Akses nested data --}}
{{ $buku->pengarang->nama ?? 'Tidak Diketahui' }}
```

#### [[12. Layout dengan Blade — Satu Template, Banyak Halaman]]

PHP

```
{{-- resources/views/layouts/app.blade.php --}}
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>@yield('title', 'Perpustakaan Digital') | {{ config('app.name') }}</title>

    {{-- Asset menggunakan Vite (Laravel 11 default) --}}
    @vite(['resources/css/app.css', 'resources/js/app.js'])

    {{-- Stack: bisa di-push dari child view --}}
    @stack('styles')
</head>
<body>
    <nav>
        <a href="{{ route('beranda') }}">{{ config('app.name') }}</a>

        <div>
            @auth
                {{-- Hanya tampil jika sudah login --}}
                <span>Halo, {{ auth()->user()->nama }}!</span>
                <a href="{{ route('profil.show') }}">Profil</a>

                <form action="{{ route('logout') }}" method="POST" style="display:inline">
                    @csrf
                    <button type="submit">Logout</button>
                </form>
            @else
                {{-- Hanya tampil jika belum login --}}
                <a href="{{ route('login') }}">Login</a>
                <a href="{{ route('register') }}">Daftar</a>
            @endauth
        </div>
    </nav>

    {{-- Flash message --}}
    @if(session('sukses'))
        <div class="alert alert-success">{{ session('sukses') }}</div>
    @endif

    @if(session('error'))
        <div class="alert alert-danger">{{ session('error') }}</div>
    @endif

    <main>
        @yield('content')   {{-- Konten dari child view masuk di sini --}}
    </main>

    <footer>
        <p>&copy; {{ date('Y') }} Perpustakaan Digital</p>
    </footer>

    @stack('scripts')   {{-- Script dari child view masuk di sini --}}
</body>
</html>
```

PHP

```
{{-- resources/views/buku/index.blade.php --}}
@extends('layouts.app')   {{-- Gunakan layout app.blade.php --}}

@section('title', 'Katalog Buku')   {{-- Isi @yield('title') --}}

@section('content')
<div class="container">
    <h1>Katalog Buku</h1>

    @forelse($buku as $item)
        <div class="card">
            <h3>{{ $item->judul }}</h3>
            <p>{{ $item->pengarang }}</p>
            <a href="{{ route('buku.show', $item) }}">Lihat Detail</a>
        </div>
    @empty
        <p>Belum ada buku.</p>
    @endforelse

    {{-- Pagination --}}
    {{ $buku->links() }}
</div>
@endsection

@push('scripts')
<script>
    // Script spesifik halaman ini
    console.log('Halaman katalog buku');
</script>
@endpush
```

#### [[13. Komponen Blade — UI yang Reusable]]

PHP

```
{{-- resources/views/components/buku-card.blade.php --}}
{{-- Komponen untuk kartu buku -- reusable di berbagai halaman --}}

@props([
    'buku',                    // required
    'tampilkanHarga' => true,  // optional dengan default
    'ukuran' => 'normal',      // 'normal' | 'kecil' | 'besar'
])

<div class="buku-card buku-card--{{ $ukuran }}">
    <h3 class="buku-card__judul">{{ $buku->judul }}</h3>
    <p class="buku-card__pengarang">{{ $buku->pengarang }}</p>

    @if($tampilkanHarga)
        <p class="buku-card__harga">{{ $buku->formatHarga() }}</p>
    @endif

    <div class="buku-card__status">
        @if($buku->tersedia())
            <span class="badge badge--success">Tersedia</span>
        @else
            <span class="badge badge--danger">Habis</span>
        @endif
    </div>

    {{-- Slot: konten yang di-inject dari luar --}}
    {{ $slot }}
</div>
```

PHP

```
{{-- Cara pakai komponen di view lain --}}

{{-- Cara 1: anonymous component --}}
<x-buku-card :buku="$buku" />

<x-buku-card :buku="$buku" :tampilkan-harga="false" ukuran="kecil" />

{{-- Dengan slot --}}
<x-buku-card :buku="$buku">
    <a href="{{ route('buku.show', $buku) }}" class="btn">Lihat Detail</a>
    <a href="{{ route('pinjam.store', $buku) }}" class="btn btn--primary">Pinjam</a>
</x-buku-card>

{{-- Komponen untuk alert --}}
{{-- resources/views/components/alert.blade.php --}}
@props(['tipe' => 'info', 'judul' => null])

<div class="alert alert--{{ $tipe }}" role="alert">
    @if($judul)
        <strong>{{ $judul }}</strong>
    @endif
    {{ $slot }}
</div>

{{-- Penggunaan: --}}
<x-alert tipe="success" judul="Berhasil!">
    Buku berhasil ditambahkan ke katalog.
</x-alert>

<x-alert tipe="danger">
    Stok buku tidak tersedia.
</x-alert>
```

---

### 🏗️ Checkpoint Level 1

text

```
✅ Checklist sebelum lanjut ke Level 2:

PEMAHAMAN:
├── Bisa jelaskan alur request di Laravel (browser → response)
├── Bisa jelaskan perbedaan web.php vs api.php
├── Bisa jelaskan mengapa named route lebih baik dari hardcode URL
├── Bisa jelaskan perbedaan {{ }} vs {!! !!} dan kapan pakai masing-masing
└── Bisa jelaskan cara kerja @extends, @section, @yield

PROYEK: Landing Page Perpustakaan
├── routes/web.php: route beranda, tentang, kontak
├── layouts/app.blade.php: layout utama dengan navbar dan footer
├── beranda.blade.php: halaman utama dengan info perpustakaan
├── tentang.blade.php: halaman tentang
├── components/: minimal 2 komponen (alert, card)
└── php artisan route:list: semua route terdaftar dengan benar

KEBIASAAN:
├── Selalu gunakan {{ }} bukan <?= ?> di Blade
├── Selalu gunakan route() helper bukan hardcode URL
├── Gunakan Artisan untuk generate file, bukan buat manual
└── php artisan route:list setelah tambah route baru

Git: feat: setup Laravel project with routing and Blade layouts
```

---

## 🔵 LEVEL 2: CONTROLLER DAN VIEW LENGKAP (Minggu 4-7)

> **Tema**: _"Dari closure di route ke controller yang terorganisir"_  
> **Benang Merah**: Route dengan closure (Level 1) → controller untuk memisahkan logika → passing data ke view → request object  
> **Output**: Halaman katalog buku dengan controller, data hardcode, dan tampilan lengkap

---

### E. Controller — Logika Request yang Terorganisir

> 💡 **Mengapa Controller?** Closure di route file bekerja untuk logika sederhana, tapi akan cepat berantakan. Controller memisahkan logika ke class tersendiri, membuat kode lebih terorganisir dan testable.

text

```
Benang Merah Bagian E:
Route dengan closure (Level 1) →
Controller: class yang mengelompokkan handler untuk resource yang sama →
Resource controller: 7 method standar CRUD →
Request: object yang merepresentasikan HTTP request →
Dependency injection: Laravel otomatis inject apa yang dibutuhkan
```

#### [[14. Membuat dan Menggunakan Controller]]

Bash

```
# Buat resource controller
php artisan make:controller BukuController --resource

# Buat single-action controller
php artisan make:controller LandingController --invokable
```

PHP

```
<?php
// app/Http/Controllers/BukuController.php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Http\RedirectResponse;
use Illuminate\View\View;

class BukuController extends Controller
{
    // Data sementara (nanti diganti Eloquent)
    private array $katalog = [
        [
            'id'        => 1,
            'judul'     => 'Clean Code',
            'pengarang' => 'Robert Martin',
            'tahun'     => 2008,
            'harga'     => 150000,
            'stok'      => 5,
            'kategori'  => 'Teknologi',
        ],
        [
            'id'        => 2,
            'judul'     => 'The Pragmatic Programmer',
            'pengarang' => 'Andrew Hunt',
            'tahun'     => 1999,
            'harga'     => 175000,
            'stok'      => 3,
            'kategori'  => 'Teknologi',
        ],
        [
            'id'        => 3,
            'judul'     => 'Laskar Pelangi',
            'pengarang' => 'Andrea Hirata',
            'tahun'     => 2005,
            'harga'     => 95000,
            'stok'      => 0,  // habis
            'kategori'  => 'Fiksi',
        ],
    ];

    /**
     * GET /buku — Daftar semua buku
     */
    public function index(Request $request): View
    {
        // Filter berdasarkan query string: /buku?kategori=Teknologi
        $kategori = $request->query('kategori');

        $buku = $this->katalog;
        if ($kategori) {
            $buku = array_filter($buku, fn($b) => $b['kategori'] === $kategori);
            $buku = array_values($buku);
        }

        // Kirim data ke view dengan array
        return view('buku.index', [
            'buku'     => $buku,
            'kategori' => $kategori,
            'judul'    => 'Katalog Buku',
        ]);
    }

    /**
     * GET /buku/{id} — Detail satu buku
     */
    public function show(string $id): View
    {
        // Cari buku berdasarkan ID
        $buku = collect($this->katalog)->firstWhere('id', (int) $id);

        // Jika tidak ditemukan, kembalikan 404
        if (!$buku) {
            abort(404, 'Buku tidak ditemukan');
        }

        return view('buku.show', compact('buku'));
    }

    /**
     * GET /buku/create — Form tambah buku
     */
    public function create(): View
    {
        $kategori = ['Teknologi', 'Fiksi', 'Sains', 'Sejarah', 'Umum'];
        return view('buku.create', compact('kategori'));
    }

    /**
     * POST /buku — Simpan buku baru
     */
    public function store(Request $request): RedirectResponse
    {
        // Validasi (detail di Level 4)
        $validated = $request->validate([
            'judul'     => 'required|string|max:200',
            'pengarang' => 'required|string|max:100',
            'tahun'     => 'required|integer|min:1000|max:' . date('Y'),
            'harga'     => 'required|numeric|min:0',
            'stok'      => 'required|integer|min:0',
            'kategori'  => 'required|string',
        ]);

        // Nanti: simpan ke database
        // Sekarang: redirect dengan flash message
        return redirect()
            ->route('buku.index')
            ->with('sukses', "Buku '{$validated['judul']}' berhasil ditambahkan!");
    }
}
```

#### [[15. Dependency Injection di Controller — Laravel yang Pintar]]

PHP

```
<?php
// Laravel otomatis inject dependency melalui type hint
// Tidak perlu new Request() — Laravel yang handle

class BukuController extends Controller
{
    // Constructor injection: inject service yang dibutuhkan
    public function __construct(
        private BukuRepository $repository,  // inject custom service
    ) {}

    // Method injection: inject per-method
    public function index(
        Request $request,        // inject HTTP request
        BukuService $service,    // inject custom service
    ): View {
        $buku = $service->getAll($request->query('kategori'));
        return view('buku.index', compact('buku'));
    }

    // Route Model Binding: Laravel otomatis fetch model dari database!
    // Route: Route::get('/buku/{buku}', ...)
    // Laravel otomatis SELECT * FROM buku WHERE id = {buku}
    public function show(Buku $buku): View  // otomatis 404 jika tidak ada
    {
        return view('buku.show', compact('buku'));
    }

    // Jika parameter route berbeda dari nama model:
    // Route: Route::get('/artikel/{post}', ...)
    // public function show(Buku $post): View  // bind {post} ke model Buku
}
```

#### [[16. Request Object — Data dari HTTP Request]]

PHP

```
<?php
class BukuController extends Controller
{
    public function index(Request $request): View
    {
        // Query string: /buku?kategori=Teknologi&halaman=2
        $kategori = $request->query('kategori');          // 'Teknologi'
        $halaman  = $request->query('halaman', 1);        // 2 (default: 1)

        // Semua query string
        $semuaQuery = $request->query();                  // array

        // Cek keberadaan
        if ($request->has('kategori')) {
            // ada parameter kategori di query string
        }

        if ($request->filled('kategori')) {
            // ada dan tidak kosong
        }

        // Informasi request
        $url    = $request->url();           // http://localhost/buku
        $path   = $request->path();          // buku
        $method = $request->method();        // GET
        $ip     = $request->ip();            // 127.0.0.1

        // Apakah request expect JSON?
        if ($request->expectsJson()) {
            return response()->json(['buku' => $buku]);
        }

        return view('buku.index', compact('buku', 'kategori'));
    }

    public function store(Request $request): RedirectResponse
    {
        // Data dari form POST
        $judul     = $request->input('judul');
        $pengarang = $request->input('pengarang', 'Tidak Diketahui'); // dengan default

        // Semua input
        $semuaInput = $request->all();

        // Hanya field tertentu
        $hanya = $request->only(['judul', 'pengarang', 'tahun']);

        // Semua kecuali field tertentu
        $kecuali = $request->except(['_token', '_method']);

        // File upload
        if ($request->hasFile('sampul')) {
            $file = $request->file('sampul');
            // Detail di Level 6
        }
    }
}
```

---

### F. View — Menampilkan Data ke Pengguna

> 💡 **Benang Merah ke E**: Controller sudah mengolah data dan memilih view. Sekarang kita buat view yang menampilkan data tersebut dengan Blade yang lebih lengkap.

#### [[17. Passing Data ke View — Cara yang Tepat]]

PHP

```
<?php
// app/Http/Controllers/BukuController.php

public function index(): View
{
    $buku      = $this->katalog;
    $kategori  = ['Teknologi', 'Fiksi', 'Sains'];
    $jumlah    = count($buku);

    // Cara 1: array (paling eksplisit — recommended)
    return view('buku.index', [
        'buku'     => $buku,
        'kategori' => $kategori,
        'jumlah'   => $jumlah,
    ]);

    // Cara 2: compact() — dari nama variabel ke array
    return view('buku.index', compact('buku', 'kategori', 'jumlah'));

    // Cara 3: with() chaining
    return view('buku.index')
        ->with('buku', $buku)
        ->with('kategori', $kategori)
        ->with('jumlah', $jumlah);
}

// Share data ke SEMUA view (di AppServiceProvider)
// Baik untuk data global seperti nama situs, user info, dll.
// app/Providers/AppServiceProvider.php
public function boot(): void
{
    // Ini akan tersedia di semua view
    View::share('namaAplikasi', config('app.name'));

    // View composer: hanya untuk view tertentu
    View::composer('layouts.app', function (View $view) {
        $view->with('kategoriNavigasi', Kategori::all());
    });
}
```

PHP

```
{{-- resources/views/buku/index.blade.php --}}
@extends('layouts.app')

@section('title', "Katalog Buku ({$jumlah} buku)")

@section('content')
<div class="container mx-auto px-4 py-8">
    <div class="flex justify-between items-center mb-6">
        <h1 class="text-3xl font-bold">Katalog Buku</h1>
        <a href="{{ route('buku.create') }}"
           class="btn btn-primary">
            + Tambah Buku
        </a>
    </div>

    {{-- Filter kategori --}}
    <div class="mb-4 flex gap-2">
        <a href="{{ route('buku.index') }}"
           class="badge {{ !request('kategori') ? 'badge-primary' : 'badge-secondary' }}">
            Semua
        </a>
        @foreach($kategori as $kat)
            <a href="{{ route('buku.index', ['kategori' => $kat]) }}"
               class="badge {{ request('kategori') === $kat ? 'badge-primary' : 'badge-secondary' }}">
                {{ $kat }}
            </a>
        @endforeach
    </div>

    {{-- Daftar buku --}}
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        @forelse($buku as $item)
            <x-buku-card :buku="$item">
                <div class="mt-4 flex gap-2">
                    <a href="{{ route('buku.show', $item['id']) }}"
                       class="btn btn-sm btn-outline">
                        Detail
                    </a>
                    @if($item['stok'] > 0)
                        <a href="#" class="btn btn-sm btn-primary">Pinjam</a>
                    @endif
                </div>
            </x-buku-card>
        @empty
            <div class="col-span-3 text-center py-12">
                <p class="text-gray-500">Tidak ada buku yang ditemukan.</p>
                @if(request('kategori'))
                    <a href="{{ route('buku.index') }}" class="btn btn-outline mt-4">
                        Lihat Semua Buku
                    </a>
                @endif
            </div>
        @endforelse
    </div>
</div>
@endsection
```

---

### 🏗️ Checkpoint Level 2

text

```
✅ Checklist sebelum lanjut ke Level 3:

PROYEK: Katalog Buku dengan Controller
├── BukuController: index, show, create (data hardcode array)
├── buku/index.blade.php: grid kartu buku dengan filter kategori
├── buku/show.blade.php: halaman detail buku
├── buku/create.blade.php: form tambah buku (belum submit ke DB)
├── Filter kategori via query string berfungsi
└── Navigasi antar halaman dengan named route

PEMAHAMAN:
├── Bisa jelaskan mengapa controller lebih baik dari closure di route
├── Bisa jelaskan cara kerja Route Model Binding
├── Bisa jelaskan perbedaan $request->input() vs $request->query()
├── Bisa jelaskan kapan pakai compact() vs array literal
└── Bisa jelaskan cara View::share dan View::composer bekerja

Git: feat: implement BukuController with views and data passing
```

---

## 🟡 LEVEL 3: DATABASE DAN ELOQUENT ORM (Minggu 7-12)

> **Tema**: _"Dari data hardcode ke database MySQL yang persisten"_  
> **Benang Merah**: Data array (Level 2) → Migration membuat tabel → Eloquent sebagai jembatan PHP-MySQL → Seeder & Factory untuk data awal → query yang ekspresif  
> **Output**: Sistem perpustakaan dengan database MySQL, Eloquent, paginasi, dan relasi dasar

---

### G. Migration — Blueprint Struktur Database

> 💡 **Mengapa Migration?** Migration adalah version control untuk database. Tim bisa sync struktur database hanya dengan `php artisan migrate`. Tidak ada lagi "oh, kamu harus tambah kolom X di tabel Y secara manual."

text

```
Benang Merah Bagian G:
Data hardcode di array (Level 2) →
Migration: blueprint tabel yang bisa di-run dan di-rollback →
Schema builder: cara PHP mendefinisikan struktur tabel →
Jalankan migrate → tabel terbentuk di MySQL →
Seeder: isi tabel dengan data awal →
Factory: generate data palsu untuk testing
```

#### [[18. Migration — Definisikan Struktur Tabel]]

Bash

```
# Buat migration
php artisan make:migration create_buku_table
php artisan make:migration create_anggota_table
php artisan make:migration create_peminjaman_table
php artisan make:migration add_deskripsi_to_buku_table  # modifikasi tabel
```

PHP

```
<?php
// database/migrations/2024_01_01_000001_create_buku_table.php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('buku', function (Blueprint $table) {
            // Primary key: otomatis UNSIGNED BIGINT AUTO_INCREMENT
            $table->id();

            // String columns
            $table->string('judul', 200);
            $table->string('pengarang', 100);
            $table->char('isbn', 13)->unique()->nullable();
            $table->string('penerbit', 100)->nullable();

            // Numeric columns
            $table->year('tahun');
            $table->decimal('harga', 12, 2)->default(0);
            $table->unsignedInteger('stok')->default(0);

            // Enum via string dengan check constraint
            $table->string('kategori', 50)->default('Umum');

            // Text untuk konten panjang
            $table->text('deskripsi')->nullable();

            // File path untuk cover
            $table->string('sampul')->nullable();

            // Timestamps: otomatis tambah created_at dan updated_at
            $table->timestamps();

            // Soft delete: tambah deleted_at (buku tidak benar-benar dihapus)
            $table->softDeletes();

            // Index untuk kolom yang sering dicari/difilter
            $table->index('pengarang');
            $table->index('kategori');
            $table->index(['tahun', 'kategori']); // composite index
        });
    }

    public function down(): void
    {
        // Rollback: hapus tabel
        Schema::dropIfExists('buku');
    }
};
```

PHP

```
<?php
// database/migrations/2024_01_01_000002_create_anggota_table.php

Schema::create('anggota', function (Blueprint $table) {
    $table->id();
    $table->string('nomor_anggota', 20)->unique();
    $table->string('nama', 100);
    $table->string('email')->unique();
    $table->string('password');
    $table->string('telepon', 15)->nullable();
    $table->text('alamat')->nullable();
    $table->date('tanggal_daftar');
    $table->date('tanggal_expired')->nullable();
    $table->enum('status', ['aktif', 'nonaktif', 'suspended'])->default('aktif');
    $table->timestamp('email_verified_at')->nullable();
    $table->rememberToken();
    $table->timestamps();
    $table->softDeletes();
});

// database/migrations/2024_01_01_000003_create_peminjaman_table.php
Schema::create('peminjaman', function (Blueprint $table) {
    $table->id();

    // Foreign key ke tabel buku
    $table->foreignId('buku_id')
          ->constrained('buku')      // referensi tabel buku
          ->cascadeOnUpdate()         // jika id buku berubah, ikut berubah
          ->restrictOnDelete();       // tidak bisa hapus buku yang masih dipinjam

    // Foreign key ke tabel anggota
    $table->foreignId('anggota_id')
          ->constrained('anggota')
          ->cascadeOnUpdate()
          ->restrictOnDelete();

    $table->date('tanggal_pinjam');
    $table->date('batas_kembali');
    $table->date('tanggal_kembali')->nullable(); // null = belum dikembalikan
    $table->decimal('denda', 10, 2)->default(0);
    $table->enum('status', ['dipinjam', 'dikembalikan', 'terlambat'])->default('dipinjam');
    $table->text('catatan')->nullable();
    $table->timestamps();

    // Index untuk query yang sering
    $table->index(['anggota_id', 'status']);
    $table->index('tanggal_pinjam');
});
```

Bash

```
# Jalankan semua migration yang belum dijalankan
php artisan migrate

# Lihat status migration
php artisan migrate:status

# Rollback migration terakhir
php artisan migrate:rollback

# Drop semua tabel dan re-migrate (development only!)
php artisan migrate:fresh

# migrate:fresh + seeder
php artisan migrate:fresh --seed
```

#### [[19. Seeder dan Factory — Data Awal dan Data Palsu]]

Bash

```
# Buat factory dan seeder
php artisan make:factory BukuFactory --model=Buku
php artisan make:seeder BukuSeeder
php artisan make:seeder DatabaseSeeder  # sudah ada, tinggal edit
```

PHP

```
<?php
// database/factories/BukuFactory.php

namespace Database\Factories;

use Illuminate\Database\Eloquent\Factories\Factory;

class BukuFactory extends Factory
{
    public function definition(): array
    {
        // Faker menghasilkan data palsu yang realistis
        return [
            'judul'     => fake()->sentence(3, true),  // "Lorem Ipsum Dolor"
            'pengarang' => fake()->name(),
            'isbn'      => fake()->numerify('#############'),  // 13 digit
            'penerbit'  => fake()->company(),
            'tahun'     => fake()->year(),
            'harga'     => fake()->numberBetween(50000, 500000),
            'stok'      => fake()->numberBetween(0, 20),
            'kategori'  => fake()->randomElement(['Teknologi', 'Fiksi', 'Sains', 'Sejarah', 'Umum']),
            'deskripsi' => fake()->paragraphs(2, true),
        ];
    }

    // Factory state: variasi data
    public function habis(): static
    {
        return $this->state(['stok' => 0]);
    }

    public function teknologi(): static
    {
        return $this->state(['kategori' => 'Teknologi']);
    }

    public function mahal(): static
    {
        return $this->state([
            'harga' => fake()->numberBetween(300000, 1000000),
        ]);
    }
}
```

PHP

```
<?php
// database/seeders/BukuSeeder.php

namespace Database\Seeders;

use App\Models\Buku;
use Illuminate\Database\Seeder;

class BukuSeeder extends Seeder
{
    public function run(): void
    {
        // Data buku nyata (fixed data)
        $bukuNyata = [
            [
                'judul'     => 'Clean Code',
                'pengarang' => 'Robert C. Martin',
                'isbn'      => '9780132350884',
                'penerbit'  => 'Prentice Hall',
                'tahun'     => 2008,
                'harga'     => 150000,
                'stok'      => 5,
                'kategori'  => 'Teknologi',
                'deskripsi' => 'Panduan untuk menulis kode yang bersih dan maintainable.',
            ],
            [
                'judul'     => 'Laskar Pelangi',
                'pengarang' => 'Andrea Hirata',
                'isbn'      => '9789793062792',
                'penerbit'  => 'Bentang Pustaka',
                'tahun'     => 2005,
                'harga'     => 95000,
                'stok'      => 8,
                'kategori'  => 'Fiksi',
                'deskripsi' => 'Novel tentang semangat belajar anak-anak Belitung.',
            ],
        ];

        foreach ($bukuNyata as $data) {
            Buku::create($data);
        }

        // Generate 50 buku palsu menggunakan factory
        Buku::factory()->count(40)->create();

        // Factory dengan state
        Buku::factory()->count(5)->habis()->create();
        Buku::factory()->count(5)->teknologi()->create();
    }
}
```

PHP

```
<?php
// database/seeders/DatabaseSeeder.php

namespace Database\Seeders;

use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    public function run(): void
    {
        // Urutan seeder: pastikan foreign key terpenuhi
        $this->call([
            BukuSeeder::class,
            AnggotaSeeder::class,
            PeminjamanSeeder::class,
        ]);
    }
}
```

Bash

```
# Jalankan semua seeder
php artisan db:seed

# Jalankan seeder spesifik
php artisan db:seed --class=BukuSeeder

# Fresh migrate + seed (paling sering dipakai saat development)
php artisan migrate:fresh --seed
```

---

### H. Eloquent ORM — Model yang Ekspresif

> 💡 **Mengapa Eloquent?** Eloquent mengubah query SQL menjadi method PHP yang ekspresif. `Buku::where('kategori', 'Teknologi')->orderBy('judul')->get()` jauh lebih mudah dibaca dan ditulis daripada query SQL mentah.

#### [[20. Eloquent Model — Dasar]]

Bash

```
# Buat model dengan migration, factory, seeder sekaligus
php artisan make:model Buku --migration --factory --seeder
# Atau shortcut:
php artisan make:model Buku -mfs
```

PHP

```
<?php
// app/Models/Buku.php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\SoftDeletes;
use Illuminate\Database\Eloquent\Builder;
use Illuminate\Database\Eloquent\Relations\HasMany;

class Buku extends Model
{
    use HasFactory, SoftDeletes;

    // Kolom yang boleh diisi via create() atau fill()
    // Security: mencegah mass assignment vulnerability
    protected $fillable = [
        'judul', 'pengarang', 'isbn', 'penerbit',
        'tahun', 'harga', 'stok', 'kategori', 'deskripsi', 'sampul',
    ];

    // Cast: otomatis konversi tipe saat akses
    protected function casts(): array
    {
        return [
            'harga'     => 'decimal:2',
            'stok'      => 'integer',
            'tahun'     => 'integer',
            'deleted_at' => 'datetime',
        ];
    }

    // ─── Accessor: modifikasi nilai saat diakses ───────────────────────────

    // Akses via: $buku->harga_format
    public function getHargaFormatAttribute(): string
    {
        return 'Rp ' . number_format($this->harga, 0, ',', '.');
    }

    // Cara modern (PHP 8 + Laravel 9+):
    // use Illuminate\Database\Eloquent\Casts\Attribute;
    // protected function hargaFormat(): Attribute
    // {
    //     return Attribute::get(fn() => 'Rp ' . number_format($this->harga, 0, ',', '.'));
    // }

    // ─── Method ──────────────────────────────────────────────────────────

    public function tersedia(): bool
    {
        return $this->stok > 0;
    }

    // ─── Local Scope: query yang bisa direuse ────────────────────────────

    // Penggunaan: Buku::tersedia()->get()
    public function scopeTersedia(Builder $query): Builder
    {
        return $query->where('stok', '>', 0);
    }

    // Penggunaan: Buku::kategori('Teknologi')->get()
    public function scopeKategori(Builder $query, string $kategori): Builder
    {
        return $query->where('kategori', $kategori);
    }

    // Penggunaan: Buku::pencarian('clean')->get()
    public function scopePencarian(Builder $query, string $keyword): Builder
    {
        return $query->where(function (Builder $q) use ($keyword) {
            $q->where('judul', 'like', "%{$keyword}%")
              ->orWhere('pengarang', 'like', "%{$keyword}%")
              ->orWhere('isbn', 'like', "%{$keyword}%");
        });
    }

    // ─── Relasi ──────────────────────────────────────────────────────────

    public function peminjaman(): HasMany
    {
        return $this->hasMany(Peminjaman::class);
    }
}
```

#### [[21. Eloquent Query — Ambil, Filter, Sort, Paginate]]

PHP

```
<?php
// app/Http/Controllers/BukuController.php

use App\Models\Buku;

public function index(Request $request): View
{
    // ─── Query dasar ──────────────────────────────────────────────────────

    // Ambil semua (HINDARI di production jika data banyak!)
    $semuaBuku = Buku::all();

    // Dengan kondisi
    $bukuTeknologi = Buku::where('kategori', 'Teknologi')->get();

    // Multiple kondisi
    $bukuMahal = Buku::where('kategori', 'Teknologi')
                     ->where('harga', '>', 100000)
                     ->orderBy('harga', 'desc')
                     ->get();

    // ─── Menggunakan local scope ──────────────────────────────────────────

    $tersedia   = Buku::tersedia()->get();
    $teknologi  = Buku::kategori('Teknologi')->tersedia()->get();
    $pencarian  = Buku::pencarian('clean code')->get();

    // ─── Paginasi (selalu gunakan ini, bukan all()) ───────────────────────

    $buku = Buku::latest()          // order by created_at desc
                ->paginate(12);     // 12 per halaman

    // Dengan kondisi dan paginate
    $buku = Buku::tersedia()
                ->when($request->filled('kategori'), function (Builder $q) use ($request) {
                    $q->kategori($request->kategori);
                })
                ->when($request->filled('cari'), function (Builder $q) use ($request) {
                    $q->pencarian($request->cari);
                })
                ->orderBy('judul')
                ->paginate(12)
                ->withQueryString(); // pertahankan query string di link paginasi

    return view('buku.index', compact('buku'));

    // ─── Ambil satu record ────────────────────────────────────────────────

    $buku = Buku::find(1);           // null jika tidak ada
    $buku = Buku::findOrFail(1);     // abort(404) jika tidak ada — paling sering dipakai
    $buku = Buku::first();           // record pertama
    $buku = Buku::firstOrFail();     // abort(404) jika tidak ada

    $buku = Buku::where('isbn', '9780132350884')->first();

    // firstOrCreate: ambil atau buat baru
    $buku = Buku::firstOrCreate(
        ['isbn' => '9780132350884'],                    // kondisi pencarian
        ['judul' => 'Clean Code', 'pengarang' => 'Robert Martin', ...] // data jika dibuat baru
    );

    // ─── Agregasi ────────────────────────────────────────────────────────

    $jumlah   = Buku::count();
    $tersedia = Buku::tersedia()->count();
    $rataHarga = Buku::avg('harga');
    $maxHarga  = Buku::max('harga');
    $totalStok = Buku::sum('stok');

    // Exists check
    $ada = Buku::where('isbn', '9780132350884')->exists();  // true/false
}
```

#### [[22. Eloquent CRUD — Create, Update, Delete]]

PHP

```
<?php
class BukuController extends Controller
{
    public function store(Request $request): RedirectResponse
    {
        // Create: hanya field yang ada di $fillable yang tersimpan
        $buku = Buku::create($request->validated());

        // Atau: buat instance lalu save
        $buku = new Buku();
        $buku->judul = $request->judul;
        $buku->pengarang = $request->pengarang;
        $buku->save();

        return redirect()
            ->route('buku.show', $buku)
            ->with('sukses', "Buku '{$buku->judul}' berhasil ditambahkan!");
    }

    public function update(Request $request, Buku $buku): RedirectResponse
    {
        // Update: hanya field yang ada di $fillable yang diupdate
        $buku->update($request->validated());

        // Atau update satu field:
        $buku->stok += 5;
        $buku->save();

        // Update tanpa fetch model dulu (untuk batch update):
        Buku::where('kategori', 'Lama')->update(['kategori' => 'Umum']);

        return redirect()
            ->route('buku.show', $buku)
            ->with('sukses', 'Buku berhasil diperbarui!');
    }

    public function destroy(Buku $buku): RedirectResponse
    {
        // Soft delete: isi deleted_at, buku masih ada di database
        // Eloquent otomatis exclude soft-deleted records dari query
        $buku->delete();

        // Hard delete: benar-benar hapus dari database
        $buku->forceDelete();

        // Restore soft-deleted record
        $buku->restore();

        // Query termasuk soft-deleted:
        Buku::withTrashed()->get();

        // Query hanya soft-deleted:
        Buku::onlyTrashed()->get();

        return redirect()
            ->route('buku.index')
            ->with('sukses', "Buku '{$buku->judul}' berhasil dihapus.");
    }
}
```

#### [[23. Eloquent Relationships — Relasi Antar Tabel]]

PHP

```
<?php
// app/Models/Anggota.php

class Anggota extends Model
{
    // Satu anggota punya banyak peminjaman
    public function peminjaman(): HasMany
    {
        return $this->hasMany(Peminjaman::class);
    }

    // Peminjaman yang sedang aktif
    public function peminjamanAktif(): HasMany
    {
        return $this->hasMany(Peminjaman::class)
                    ->where('status', 'dipinjam');
    }

    // Buku yang pernah dipinjam (via peminjaman)
    public function bukuDipinjam(): BelongsToMany
    {
        return $this->belongsToMany(Buku::class, 'peminjaman')
                    ->withPivot('tanggal_pinjam', 'batas_kembali', 'status')
                    ->withTimestamps();
    }
}

// app/Models/Peminjaman.php
class Peminjaman extends Model
{
    // Peminjaman milik satu buku
    public function buku(): BelongsTo
    {
        return $this->belongsTo(Buku::class);
    }

    // Peminjaman milik satu anggota
    public function anggota(): BelongsTo
    {
        return $this->belongsTo(Anggota::class);
    }
}
```

PHP

```
<?php
// EAGER LOADING: solusi N+1 problem

// ❌ N+1 problem: 1 query untuk daftar + N query untuk setiap relasi
$peminjaman = Peminjaman::all();
foreach ($peminjaman as $p) {
    echo $p->buku->judul;     // query baru setiap iterasi!
    echo $p->anggota->nama;   // query baru setiap iterasi!
}
// Jika 100 peminjaman: 1 + 100 + 100 = 201 queries!

// ✅ Eager loading: ambil relasi sekaligus
$peminjaman = Peminjaman::with(['buku', 'anggota'])->get();
// Hanya 3 queries: 1 untuk peminjaman, 1 untuk semua buku, 1 untuk semua anggota

// Eager loading dengan kondisi
$peminjaman = Peminjaman::with([
    'buku:id,judul,pengarang',  // hanya kolom tertentu
    'anggota' => function ($query) {
        $query->where('status', 'aktif');
    },
])->where('status', 'dipinjam')
  ->get();

// withCount: hitung relasi tanpa load datanya
$buku = Buku::withCount('peminjaman')->get();
// Akses: $buku->peminjaman_count
```

---

### 🏗️ Checkpoint Level 3

text

```
✅ Checklist sebelum lanjut ke Level 4:

PROYEK: Sistem Perpustakaan dengan Database
├── Migration: buku, anggota, peminjaman (dengan relasi)
├── Model: Buku, Anggota, Peminjaman (dengan relasi Eloquent)
├── Factory & Seeder: data awal + data palsu
├── Controller: CRUD buku menggunakan Eloquent
├── View: daftar buku dengan paginasi dan filter
├── Paginasi berfungsi: {{ $buku->links() }} di view
└── php artisan migrate:fresh --seed berjalan tanpa error

QUERY YANG HARUS BISA DITULIS:
├── Buku::tersedia()->paginate(12)
├── Buku::with('peminjaman')->findOrFail($id)
├── Buku::when(...)->orderBy(...)->paginate()
├── Eager loading untuk menghindari N+1
└── Soft delete: delete(), restore(), onlyTrashed()

KEAMANAN:
├── $fillable terdefinisi di semua model
├── findOrFail() dipakai, bukan find() + manual check
└── Soft delete dipakai, data tidak benar-benar dihapus

Git: feat: implement database, Eloquent models, and CRUD with pagination
```

---

## 🟠 LEVEL 4: VALIDASI, AUTH, DAN AUTHORIZATION (Minggu 12-18)

> **Tema**: _"Dari sistem terbuka ke sistem yang aman dengan autentikasi dan otorisasi"_  
> **Benang Merah**: CRUD tanpa keamanan (Level 3) → validasi input → autentikasi user → otorisasi akses → middleware  
> **Output**: Admin panel perpustakaan dengan login, role-based access, dan validasi lengkap

---

### I. Validasi — Input yang Benar dan Aman

> 💡 **Mengapa validasi di awal level ini?** Sebelum auth, kita harus paham validasi karena form login sendiri butuh validasi. Selain itu, validasi adalah lini pertama keamanan.

text

```
Benang Merah Bagian I:
CRUD berfungsi (Level 3) →
Validasi: pastikan data benar sebelum diproses →
Form Request: pisahkan logika validasi dari controller →
Error handling: tampilkan pesan error yang helpful →
Sanitasi: data sudah bersih sebelum ke database
```

#### [[24. Validasi di Controller dan Form Request]]

PHP

```
<?php
// Cara 1: Validasi langsung di controller (untuk form sederhana)
public function store(Request $request): RedirectResponse
{
    $validated = $request->validate([
        'judul'     => ['required', 'string', 'max:200'],
        'pengarang' => ['required', 'string', 'max:100'],
        'isbn'      => ['nullable', 'digits:13', 'unique:buku,isbn'],
        'tahun'     => ['required', 'integer', 'min:1000', 'max:' . date('Y')],
        'harga'     => ['required', 'numeric', 'min:0'],
        'stok'      => ['required', 'integer', 'min:0'],
        'kategori'  => ['required', 'string', 'in:Teknologi,Fiksi,Sains,Sejarah,Umum'],
        'deskripsi' => ['nullable', 'string', 'max:2000'],
        'sampul'    => ['nullable', 'image', 'mimes:jpg,png,webp', 'max:2048'],
    ]);

    // Jika gagal: otomatis redirect back dengan errors dan old input
    // Jika sukses: $validated berisi data yang sudah tervalidasi

    $buku = Buku::create($validated);
    return redirect()->route('buku.show', $buku)->with('sukses', 'Buku ditambahkan!');
}
```

Bash

```
# Form Request: validasi yang lebih terorganisir (rekomendasi)
php artisan make:request StoreBukuRequest
php artisan make:request UpdateBukuRequest
```

PHP

```
<?php
// app/Http/Requests/StoreBukuRequest.php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;
use Illuminate\Validation\Rule;

class StoreBukuRequest extends FormRequest
{
    // Apakah user berhak melakukan request ini?
    public function authorize(): bool
    {
        // true = semua boleh
        // Atau cek permission: return auth()->user()->can('create', Buku::class);
        return auth()->check() && auth()->user()->isAdmin();
    }

    public function rules(): array
    {
        return [
            'judul'     => ['required', 'string', 'max:200'],
            'pengarang' => ['required', 'string', 'max:100'],
            'isbn'      => ['nullable', 'digits:13', Rule::unique('buku', 'isbn')],
            'tahun'     => ['required', 'integer', 'min:1000', 'max:' . date('Y')],
            'harga'     => ['required', 'numeric', 'min:0'],
            'stok'      => ['required', 'integer', 'min:0'],
            'kategori'  => ['required', Rule::in(['Teknologi', 'Fiksi', 'Sains', 'Sejarah', 'Umum'])],
            'deskripsi' => ['nullable', 'string', 'max:2000'],
            'sampul'    => ['nullable', 'image', 'mimes:jpg,jpeg,png,webp', 'max:2048'],
        ];
    }

    // Custom pesan error
    public function messages(): array
    {
        return [
            'judul.required'    => 'Judul buku wajib diisi.',
            'judul.max'         => 'Judul tidak boleh lebih dari :max karakter.',
            'isbn.digits'       => 'ISBN harus tepat 13 digit angka.',
            'isbn.unique'       => 'ISBN ini sudah terdaftar di sistem.',
            'sampul.image'      => 'File sampul harus berupa gambar.',
            'sampul.max'        => 'Ukuran sampul maksimal 2MB.',
        ];
    }

    // Modifikasi input sebelum validasi
    protected function prepareForValidation(): void
    {
        $this->merge([
            'judul'     => trim($this->judul ?? ''),
            'pengarang' => trim($this->pengarang ?? ''),
            // Hapus tanda hubung dari ISBN: 978-013-235-0884 → 9780132350884
            'isbn'      => preg_replace('/[^0-9]/', '', $this->isbn ?? ''),
        ]);
    }
}
```

PHP

```
<?php
// app/Http/Requests/UpdateBukuRequest.php
class UpdateBukuRequest extends FormRequest
{
    public function rules(): array
    {
        return [
            'judul'  => ['required', 'string', 'max:200'],
            'isbn'   => [
                'nullable',
                'digits:13',
                // Unique kecuali untuk record yang sedang diedit
                Rule::unique('buku', 'isbn')->ignore($this->route('buku')),
            ],
            // ... rules lainnya
        ];
    }
}
```

PHP

```
{{-- resources/views/buku/create.blade.php --}}

<form action="{{ route('buku.store') }}" method="POST" enctype="multipart/form-data">
    @csrf  {{-- WAJIB: Laravel otomatis cek CSRF token --}}

    <div class="form-group">
        <label for="judul">Judul Buku *</label>
        <input
            type="text"
            id="judul"
            name="judul"
            value="{{ old('judul') }}"  {{-- Pertahankan input jika validasi gagal --}}
            class="form-control {{ $errors->has('judul') ? 'is-invalid' : '' }}"
            required
        >
        @error('judul')
            <div class="invalid-feedback">{{ $message }}</div>
        @enderror
    </div>

    <div class="form-group">
        <label for="kategori">Kategori *</label>
        <select name="kategori" id="kategori" class="form-control">
            @foreach(['Teknologi', 'Fiksi', 'Sains', 'Sejarah', 'Umum'] as $kat)
                <option
                    value="{{ $kat }}"
                    {{ old('kategori') === $kat ? 'selected' : '' }}
                >{{ $kat }}</option>
            @endforeach
        </select>
        @error('kategori')
            <div class="invalid-feedback">{{ $message }}</div>
        @enderror
    </div>

    <button type="submit" class="btn btn-primary">Simpan Buku</button>
</form>
```

---

### J. Authentication — Sistem Login

> 💡 **Benang Merah ke I**: Validasi sudah berjalan. Sekarang kita bangun sistem autentikasi — siapa yang boleh mengakses sistem.

#### [[25. Laravel Breeze — Scaffolding Auth yang Cepat]]

Bash

```
# Install Breeze (auth scaffolding paling sederhana)
composer require laravel/breeze --dev
php artisan breeze:install

# Pilih stack:
# blade       → Blade + Tailwind CSS (paling sederhana, recommended untuk belajar)
# livewire    → Livewire + Blade
# react       → Inertia + React
# vue         → Inertia + Vue
# api         → API only (tanpa UI)

php artisan breeze:install blade

npm install
npm run dev

php artisan migrate
```

text

```
Breeze akan buat:
├── routes/auth.php                → route login, register, logout, dll
├── app/Http/Controllers/Auth/    → LoginController, RegisterController, dll
├── resources/views/auth/         → halaman login, register, dll
└── Middleware auth terdaftar di bootstrap/app.php
```

#### [[26. Autentikasi Manual — Memahami di Balik Layar]]

PHP

```
<?php
// Memahami cara Auth facade bekerja

use Illuminate\Support\Facades\Auth;

// Login manual (tanpa Breeze)
public function login(Request $request): RedirectResponse
{
    $credentials = $request->validate([
        'email'    => ['required', 'email'],
        'password' => ['required'],
    ]);

    // Auth::attempt: cek email + password, buat session jika cocok
    if (Auth::attempt($credentials, $request->boolean('ingat_saya'))) {
        $request->session()->regenerate();  // cegah session fixation

        return redirect()
            ->intended(route('dashboard'))  // ke halaman yang dituju sebelum login
            ->with('sukses', 'Selamat datang kembali!');
    }

    // Jika gagal: redirect back dengan error
    return back()
        ->withErrors(['email' => 'Email atau password tidak cocok.'])
        ->onlyInput('email');  // pertahankan hanya input email (bukan password)
}

public function logout(Request $request): RedirectResponse
{
    Auth::logout();

    $request->session()->invalidate();   // hancurkan session
    $request->session()->regenerateToken(); // generate CSRF token baru

    return redirect()->route('login');
}

// Akses user yang sedang login:
Auth::user()          // model User atau null
Auth::id()            // ID user atau null
Auth::check()         // true jika login
Auth::guest()         // true jika belum login

// Di controller:
$user = auth()->user();
$userId = auth()->id();

// Di Blade:
// {{ auth()->user()->nama }}
// @auth ... @endauth
// @guest ... @endguest
```

#### [[27. Middleware Auth — Proteksi Route]]

PHP

```
<?php
// routes/web.php

// Lindungi route dengan middleware auth
Route::middleware(['auth'])->group(function () {
    Route::get('/dashboard', [DashboardController::class, 'index'])
         ->name('dashboard');

    Route::resource('buku', BukuController::class)
         ->except(['index', 'show']); // index & show bisa diakses publik

    Route::get('/profil', [ProfilController::class, 'show'])->name('profil');
});

// Route yang hanya untuk guest (belum login)
Route::middleware(['guest'])->group(function () {
    Route::get('/login', [AuthController::class, 'showLogin'])->name('login');
    Route::post('/login', [AuthController::class, 'login']);
    Route::get('/register', [AuthController::class, 'showRegister'])->name('register');
});
```

---

### K. Authorization — Siapa Boleh Apa

> 💡 **Benang Merah ke J**: Auth menentukan SIAPA kamu. Authorization menentukan APA yang boleh kamu lakukan. Contoh: semua yang login adalah "anggota", tapi hanya "admin" yang boleh hapus buku.

#### [[28. Policy — Otorisasi Berbasis Resource]]

Bash

```
php artisan make:policy BukuPolicy --model=Buku
```

PHP

```
<?php
// app/Policies/BukuPolicy.php

namespace App\Policies;

use App\Models\Anggota;
use App\Models\Buku;

class BukuPolicy
{
    // Sebelum semua pengecekan: super admin bypass semua
    public function before(Anggota $user, string $ability): bool|null
    {
        if ($user->role === 'super_admin') {
            return true; // izinkan semua tanpa cek lebih lanjut
        }
        return null; // lanjut ke pengecekan policy
    }

    // Siapa yang boleh lihat daftar buku?
    public function viewAny(?Anggota $user): bool
    {
        return true; // semua orang, termasuk guest
    }

    // Siapa yang boleh lihat detail buku?
    public function view(?Anggota $user, Buku $buku): bool
    {
        return true; // semua orang
    }

    // Siapa yang boleh tambah buku?
    public function create(Anggota $user): bool
    {
        return in_array($user->role, ['admin', 'pustakawan']);
    }

    // Siapa yang boleh edit buku?
    public function update(Anggota $user, Buku $buku): bool
    {
        return in_array($user->role, ['admin', 'pustakawan']);
    }

    // Siapa yang boleh hapus buku?
    public function delete(Anggota $user, Buku $buku): bool
    {
        return $user->role === 'admin'; // hanya admin
    }
}
```

PHP

```
<?php
// app/Http/Controllers/BukuController.php

class BukuController extends Controller
{
    public function index(): View
    {
        // Tidak perlu auth, semua bisa lihat katalog
        return view('buku.index', ['buku' => Buku::paginate(12)]);
    }

    public function create(): View
    {
        // Cek izin menggunakan policy
        $this->authorize('create', Buku::class);

        return view('buku.create');
    }

    public function store(StoreBukuRequest $request): RedirectResponse
    {
        // StoreBukuRequest->authorize() sudah cek
        $buku = Buku::create($request->validated());

        return redirect()->route('buku.show', $buku)->with('sukses', 'Buku ditambahkan!');
    }

    public function destroy(Buku $buku): RedirectResponse
    {
        $this->authorize('delete', $buku);  // cek apakah user boleh hapus BUKU INI

        $buku->delete();
        return redirect()->route('buku.index')->with('sukses', 'Buku dihapus.');
    }
}
```

PHP

```
{{-- Di Blade: tampilkan tombol hanya jika punya izin --}}

@can('create', App\Models\Buku::class)
    <a href="{{ route('buku.create') }}" class="btn btn-primary">+ Tambah Buku</a>
@endcan

@can('update', $buku)
    <a href="{{ route('buku.edit', $buku) }}" class="btn btn-secondary">Edit</a>
@endcan

@can('delete', $buku)
    <form action="{{ route('buku.destroy', $buku) }}" method="POST"
          onsubmit="return confirm('Yakin hapus buku ini?')">
        @csrf
        @method('DELETE')
        <button type="submit" class="btn btn-danger">Hapus</button>
    </form>
@endcan
```

---

### L. Middleware Kustom

#### [[29. Membuat Middleware — Logic yang Berjalan Sebelum Controller]]

Bash

```
php artisan make:middleware CekStatusAnggota
php artisan make:middleware LogAktivitas
```

PHP

```
<?php
// app/Http/Middleware/CekStatusAnggota.php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;
use Symfony\Component\HttpFoundation\Response;

class CekStatusAnggota
{
    public function handle(Request $request, Closure $next): Response
    {
        // Cek apakah anggota sudah login dan statusnya aktif
        if (auth()->check() && auth()->user()->status !== 'aktif') {
            // Logout user yang tidak aktif
            auth()->logout();

            return redirect()
                ->route('login')
                ->with('error', 'Akun Anda tidak aktif. Hubungi pustakawan.');
        }

        // Lanjutkan ke controller
        return $next($request);
    }
}

// app/Http/Middleware/LogAktivitas.php
class LogAktivitas
{
    public function handle(Request $request, Closure $next): Response
    {
        // Sebelum controller
        $mulai = microtime(true);

        $response = $next($request); // proses controller

        // Setelah controller (sebelum response dikirim)
        $durasi = microtime(true) - $mulai;

        \Log::info('Request', [
            'user'   => auth()->id(),
            'method' => $request->method(),
            'path'   => $request->path(),
            'status' => $response->getStatusCode(),
            'durasi' => round($durasi * 1000, 2) . 'ms',
        ]);

        return $response;
    }
}
```

PHP

```
<?php
// bootstrap/app.php — daftarkan middleware (Laravel 11)

return Application::configure(basePath: dirname(__DIR__))
    ->withMiddleware(function (Middleware $middleware) {
        // Middleware global (semua request)
        $middleware->append(LogAktivitas::class);

        // Middleware dengan alias (untuk pakai di route)
        $middleware->alias([
            'anggota.aktif' => CekStatusAnggota::class,
            'role'          => CekRole::class,
        ]);

        // Group middleware
        $middleware->appendToGroup('web', [
            CekStatusAnggota::class,
        ]);
    })
    ->create();

// routes/web.php — pakai middleware di route
Route::middleware(['auth', 'anggota.aktif'])->group(function () {
    Route::get('/dashboard', [DashboardController::class, 'index']);
    Route::resource('peminjaman', PeminjamanController::class);
});
```

---

### 🏗️ Checkpoint Level 4

text

```
✅ Checklist sebelum lanjut ke Level 5:

PROYEK: Admin Panel Perpustakaan
├── Auth: login, register, logout (via Breeze atau manual)
├── Middleware: CekStatusAnggota, LogAktivitas
├── Policy: BukuPolicy (viewAny, view, create, update, delete)
├── Form Request: StoreBukuRequest, UpdateBukuRequest
├── CRUD buku: semua dengan validasi dan otorisasi
├── Role: admin bisa CRUD, anggota hanya bisa lihat
└── Blade: @can dipakai untuk conditional UI

ERROR HANDLING:
├── 404 page: untuk buku tidak ditemukan (findOrFail)
├── 403 page: untuk akses ditolak (authorize)
├── Validation error tampil di form dengan old() input
└── Flash message setelah setiap operasi

Git: feat: implement validation, Breeze auth, policy, and custom middleware
```

---

## 🔴 LEVEL 5: REST API, QUEUE, DAN EVENT (Minggu 18-24)

> **Tema**: _"Dari web app ke API yang bisa dikonsumsi aplikasi lain dan proses async"_  
> **Benang Merah**: Web app (Level 4) → expose API → autentikasi API via Sanctum → queue untuk proses berat → event-driven untuk decoupling  
> **Output**: REST API perpustakaan dengan Sanctum, queue untuk email, event system

---

### M. REST API dengan Laravel

> 💡 **Mengapa pisahkan API dari web?** API memungkinkan frontend lain (React, Vue, mobile app) menggunakan backend yang sama. Laravel memiliki tool khusus untuk ini.

text

```
Benang Merah Bagian M:
Web app dengan session auth (Level 4) →
API: response JSON, stateless, auth via token →
API Resource: transformasi Eloquent ke JSON yang konsisten →
Sanctum: token-based auth yang ringan →
Rate limiting: proteksi dari abuse
```

#### [[30. API Resource — Transformasi Data yang Konsisten]]

Bash

```
php artisan make:resource BukuResource
php artisan make:resource BukuCollection
php artisan make:controller Api/BukuController --api
```

PHP

```
<?php
// app/Http/Resources/BukuResource.php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;

class BukuResource extends JsonResource
{
    public function toArray(Request $request): array
    {
        return [
            'id'         => $this->id,
            'judul'      => $this->judul,
            'pengarang'  => $this->pengarang,
            'isbn'       => $this->isbn,
            'tahun'      => $this->tahun,
            'harga'      => $this->harga,
            'harga_format' => $this->harga_format, // accessor
            'stok'       => $this->stok,
            'tersedia'   => $this->tersedia(),
            'kategori'   => $this->kategori,
            'deskripsi'  => $this->deskripsi,

            // Conditional: hanya tampil jika relasi di-load (no N+1)
            'total_peminjaman' => $this->whenCounted('peminjaman'),
            'peminjaman'       => PeminjamanResource::collection(
                                    $this->whenLoaded('peminjaman')
                                  ),

            // Conditional: hanya untuk admin
            'created_at' => $this->when(
                $request->user()?->isAdmin(),
                $this->created_at?->toDateTimeString()
            ),

            'url' => [
                'self'  => route('api.v1.buku.show', $this->id),
                'pinjam' => route('api.v1.peminjaman.store'),
            ],
        ];
    }
}
```

PHP

```
<?php
// app/Http/Controllers/Api/V1/BukuController.php

namespace App\Http\Controllers\Api\V1;

use App\Http\Controllers\Controller;
use App\Http\Requests\StoreBukuRequest;
use App\Http\Resources\BukuResource;
use App\Models\Buku;
use Illuminate\Http\JsonResponse;
use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\AnonymousResourceCollection;

class BukuController extends Controller
{
    /**
     * GET /api/v1/buku
     */
    public function index(Request $request): AnonymousResourceCollection
    {
        $buku = Buku::query()
            ->when($request->filled('cari'), fn($q) => $q->pencarian($request->cari))
            ->when($request->filled('kategori'), fn($q) => $q->kategori($request->kategori))
            ->when($request->filter === 'tersedia', fn($q) => $q->tersedia())
            ->withCount('peminjaman')
            ->orderBy($request->sort ?? 'judul', $request->direction ?? 'asc')
            ->paginate($request->per_page ?? 15);

        return BukuResource::collection($buku);
        // Otomatis include: data, links (prev/next), meta (total, per_page, dll)
    }

    /**
     * GET /api/v1/buku/{buku}
     */
    public function show(Buku $buku): BukuResource
    {
        $buku->loadCount('peminjaman');
        return new BukuResource($buku);
    }

    /**
     * POST /api/v1/buku
     */
    public function store(StoreBukuRequest $request): JsonResponse
    {
        $buku = Buku::create($request->validated());
        return (new BukuResource($buku))
            ->response()
            ->setStatusCode(201)
            ->header('Location', route('api.v1.buku.show', $buku));
    }

    /**
     * PUT /api/v1/buku/{buku}
     */
    public function update(UpdateBukuRequest $request, Buku $buku): BukuResource
    {
        $buku->update($request->validated());
        return new BukuResource($buku->fresh());
    }

    /**
     * DELETE /api/v1/buku/{buku}
     */
    public function destroy(Buku $buku): JsonResponse
    {
        $this->authorize('delete', $buku);
        $buku->delete();
        return response()->json(null, 204);
    }
}
```

PHP

```
<?php
// routes/api.php

use Illuminate\Support\Facades\Route;

Route::prefix('v1')->name('api.v1.')->group(function () {
    // Public routes (tidak butuh auth)
    Route::apiResource('buku', Api\V1\BukuController::class)
         ->only(['index', 'show']);

    // Auth routes
    Route::post('auth/login', [Api\V1\AuthController::class, 'login']);
    Route::post('auth/register', [Api\V1\AuthController::class, 'register']);

    // Protected routes (butuh Sanctum token)
    Route::middleware('auth:sanctum')->group(function () {
        Route::post('auth/logout', [Api\V1\AuthController::class, 'logout']);
        Route::get('auth/me', [Api\V1\AuthController::class, 'me']);

        Route::apiResource('buku', Api\V1\BukuController::class)
             ->only(['store', 'update', 'destroy']);

        Route::apiResource('peminjaman', Api\V1\PeminjamanController::class);
    });
});
```

#### [[31. Laravel Sanctum — Token-Based Auth untuk API]]

Bash

```
composer require laravel/sanctum
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
php artisan migrate
```

PHP

```
<?php
// app/Models/Anggota.php — tambahkan trait HasApiTokens
use Laravel\Sanctum\HasApiTokens;

class Anggota extends Authenticatable
{
    use HasApiTokens, HasFactory, Notifiable;
    // ...
}

// app/Http/Controllers/Api/V1/AuthController.php
class AuthController extends Controller
{
    public function login(Request $request): JsonResponse
    {
        $request->validate([
            'email'    => ['required', 'email'],
            'password' => ['required'],
        ]);

        $anggota = Anggota::where('email', $request->email)->first();

        if (!$anggota || !Hash::check($request->password, $anggota->password)) {
            return response()->json([
                'message' => 'Kredensial tidak valid.',
            ], 401);
        }

        // Hapus token lama (optional: satu device, satu token)
        // $anggota->tokens()->delete();

        // Buat token baru dengan nama dan kemampuan (abilities)
        $token = $anggota->createToken(
            name: $request->device_name ?? 'api',
            abilities: $anggota->isAdmin() ? ['*'] : ['buku:read', 'peminjaman:create'],
            expiresAt: now()->addDays(30),
        );

        return response()->json([
            'token' => $token->plainTextToken,
            'user'  => new AnggotaResource($anggota),
        ]);
    }

    public function logout(Request $request): JsonResponse
    {
        // Hapus token yang sedang digunakan
        $request->user()->currentAccessToken()->delete();

        return response()->json(['message' => 'Logged out successfully.']);
    }

    public function me(Request $request): AnggotaResource
    {
        return new AnggotaResource($request->user());
    }
}
```

#### [[32. Error Handling API — Response yang Konsisten]]

PHP

```
<?php
// bootstrap/app.php — konfigurasi exception handler

->withExceptions(function (Exceptions $exceptions) {
    // Handle semua exception untuk API request
    $exceptions->render(function (\Throwable $e, Request $request) {
        if ($request->expectsJson() || $request->is('api/*')) {
            return match (true) {
                $e instanceof \Illuminate\Validation\ValidationException =>
                    response()->json([
                        'message' => 'Data tidak valid.',
                        'errors'  => $e->errors(),
                    ], 422),

                $e instanceof \Illuminate\Auth\AuthenticationException =>
                    response()->json([
                        'message' => 'Tidak terautentikasi. Silakan login.',
                    ], 401),

                $e instanceof \Illuminate\Auth\Access\AuthorizationException =>
                    response()->json([
                        'message' => 'Anda tidak memiliki izin untuk aksi ini.',
                    ], 403),

                $e instanceof \Illuminate\Database\Eloquent\ModelNotFoundException =>
                    response()->json([
                        'message' => 'Data yang diminta tidak ditemukan.',
                    ], 404),

                $e instanceof \Symfony\Component\HttpKernel\Exception\TooManyRequestsHttpException =>
                    response()->json([
                        'message' => 'Terlalu banyak request. Coba lagi nanti.',
                    ], 429),

                app()->environment('production') =>
                    response()->json([
                        'message' => 'Terjadi kesalahan pada server.',
                    ], 500),

                default => null, // fallback ke handler default Laravel
            };
        }
    });
})
```

---

### N. Queue — Proses Asynchronous

> 💡 **Mengapa Queue?** Beberapa proses membutuhkan waktu lama (kirim email, resize gambar, generate PDF). Jika dijalankan saat request, user harus menunggu. Queue memindahkan proses ke background, response ke user tetap cepat.

#### [[33. Queue — Setup dan Penggunaan Dasar]]

Bash

```
# Setup queue dengan database driver
php artisan queue:table
php artisan migrate

# Buat job
php artisan make:job KirimEmailKonfirmasiPeminjaman
php artisan make:job GenerateLaporanBulanan
```

PHP

```
<?php
// app/Jobs/KirimEmailKonfirmasiPeminjaman.php

namespace App\Jobs;

use App\Mail\KonfirmasiPeminjamanMail;
use App\Models\Peminjaman;
use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Queue\SerializesModels;
use Illuminate\Support\Facades\Mail;

class KirimEmailKonfirmasiPeminjaman implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    // Berapa kali dicoba jika gagal
    public int $tries = 3;

    // Timeout dalam detik
    public int $timeout = 60;

    // Delay antar retry (dalam detik)
    public array $backoff = [30, 60, 120];

    public function __construct(
        private Peminjaman $peminjaman,
    ) {}

    public function handle(): void
    {
        // Ini berjalan di background, bukan saat request
        Mail::to($this->peminjaman->anggota->email)
            ->send(new KonfirmasiPeminjamanMail($this->peminjaman));
    }

    // Dipanggil jika semua percobaan gagal
    public function failed(\Throwable $exception): void
    {
        \Log::error('Gagal kirim email konfirmasi peminjaman', [
            'peminjaman_id' => $this->peminjaman->id,
            'error'         => $exception->getMessage(),
        ]);

        // Kirim notifikasi ke admin
        // Notification::send(User::admins(), new EmailGagalNotification($this->peminjaman));
    }
}
```

PHP

```
<?php
// app/Http/Controllers/PeminjamanController.php

public function store(Request $request): RedirectResponse
{
    $peminjaman = Peminjaman::create([...]);

    // Dispatch job ke queue — TIDAK menunggu email terkirim
    KirimEmailKonfirmasiPeminjaman::dispatch($peminjaman);

    // Dispatch dengan delay
    KirimEmailKonfirmasiPeminjaman::dispatch($peminjaman)
        ->delay(now()->addMinutes(5));

    // Dispatch ke queue tertentu
    KirimEmailKonfirmasiPeminjaman::dispatch($peminjaman)
        ->onQueue('email');

    return redirect()
        ->route('peminjaman.show', $peminjaman)
        ->with('sukses', 'Peminjaman berhasil! Email konfirmasi akan segera dikirim.');
}
```

Bash

```
# Jalankan worker (process queue jobs)
php artisan queue:work

# Worker dengan opsi:
php artisan queue:work --queue=email,default  # proses queue tertentu
php artisan queue:work --tries=3              # max retry
php artisan queue:work --timeout=60           # timeout per job
php artisan queue:work --sleep=3              # sleep jika tidak ada job

# Di production: gunakan Supervisor untuk menjaga worker tetap berjalan
# Konfigurasi di /etc/supervisor/conf.d/laravel-worker.conf
```

---

### O. Event dan Listener — Decoupling Logic

#### [[34. Event-Driven Architecture di Laravel]]

Bash

```
php artisan make:event BukuDipinjam
php artisan make:event BukuDikembalikan
php artisan make:listener KirimNotifikasiPeminjaman --event=BukuDipinjam
php artisan make:listener UpdateStokBuku --event=BukuDikembalikan
php artisan make:listener CatatAktivitasPeminjaman
```

PHP

```
<?php
// app/Events/BukuDipinjam.php

namespace App\Events;

use App\Models\Peminjaman;
use Illuminate\Broadcasting\InteractsWithSockets;
use Illuminate\Foundation\Events\Dispatchable;
use Illuminate\Queue\SerializesModels;

class BukuDipinjam
{
    use Dispatchable, InteractsWithSockets, SerializesModels;

    public function __construct(
        public readonly Peminjaman $peminjaman,
    ) {}
}

// app/Listeners/KirimNotifikasiPeminjaman.php
class KirimNotifikasiPeminjaman implements ShouldQueue
{
    use InteractsWithQueue;

    public function handle(BukuDipinjam $event): void
    {
        $peminjaman = $event->peminjaman;

        // Kirim email ke anggota
        Mail::to($peminjaman->anggota->email)
            ->send(new KonfirmasiPeminjamanMail($peminjaman));
    }
}

// app/Listeners/CatatAktivitasPeminjaman.php
class CatatAktivitasPeminjaman
{
    public function handle(BukuDipinjam $event): void
    {
        \Log::info('Buku dipinjam', [
            'buku_id'    => $event->peminjaman->buku_id,
            'anggota_id' => $event->peminjaman->anggota_id,
        ]);
    }
}
```

PHP

```
<?php
// app/Providers/EventServiceProvider.php atau bootstrap/app.php

// Daftarkan event-listener mapping
protected $listen = [
    BukuDipinjam::class => [
        KirimNotifikasiPeminjaman::class,
        CatatAktivitasPeminjaman::class,
        UpdateStatistikPerpustakaan::class,
    ],
    BukuDikembalikan::class => [
        UpdateStokBuku::class,
        HitungDenda::class,
        KirimKonfirmasiKembali::class,
    ],
];

// Di controller: fire event
public function store(Request $request): RedirectResponse
{
    $peminjaman = Peminjaman::create([...]);
    $peminjaman->buku()->decrement('stok');

    // Fire event — semua listener akan dipanggil
    BukuDipinjam::dispatch($peminjaman);
    // atau: event(new BukuDipinjam($peminjaman));

    return redirect()->route('peminjaman.show', $peminjaman);
}
```

---

### 🏗️ Checkpoint Level 5

text

```
✅ Checklist sebelum lanjut ke Level 6:

PROYEK: REST API Perpustakaan
├── routes/api.php: endpoint buku, peminjaman, auth
├── API Resource: BukuResource, PeminjamanResource
├── Sanctum: login → token, protected routes
├── Queue: job email konfirmasi berjalan di background
├── Event: BukuDipinjam dan listener-nya
└── Error handling: response JSON konsisten untuk semua exception

TEST DENGAN POSTMAN/INSOMNIA:
├── POST /api/v1/auth/login → dapat token
├── GET /api/v1/buku → daftar buku (public)
├── POST /api/v1/buku (tanpa token) → 401
├── POST /api/v1/buku (dengan token) → 201
├── DELETE /api/v1/buku/1 (dengan token anggota biasa) → 403
└── Rate limiting: lebih dari 60 req/menit → 429

Git: feat: build REST API with Sanctum, Queue jobs, and Event system
```

---

## ⚫ LEVEL 6: TESTING, SERVICE CONTAINER, DAN CACHING (Minggu 24-32)

> **Tema**: _"Dari kode yang bekerja ke kode yang bisa dipercaya dan performa tinggi"_  
> **Benang Merah**: Aplikasi sudah lengkap (Level 5) → testing memastikan tetap bekerja → Service Container untuk arsitektur yang bersih → Caching untuk performa  
> **Output**: Test suite lengkap, arsitektur dengan DI, dan aplikasi yang cepat dengan caching

---

### P. Testing — Kode yang Bisa Dipercaya

> 💡 **Mengapa testing di Level 6 bukan lebih awal?** Kamu perlu memahami cara komponen bekerja sebelum bisa men-test-nya. Tapi di project nyata, tulis test bersamaan dengan fitur — TDD (Test-Driven Development) adalah praktik yang disarankan.

text

```
Benang Merah Bagian P:
Fitur sudah ada (Level 1-5) →
Test: otomatis verifikasi fitur bekerja setelah perubahan →
Feature test: test HTTP request ke controller →
Unit test: test class/method secara terisolasi →
Mocking: isolasi dependency dalam test
```

#### [[35. Feature Test — Test HTTP Request]]

Bash

```
php artisan make:test BukuTest
php artisan make:test Api/BukuApiTest
php artisan make:test Auth/LoginTest
```

PHP

```
<?php
// tests/Feature/BukuTest.php

namespace Tests\Feature;

use App\Models\Anggota;
use App\Models\Buku;
use Illuminate\Foundation\Testing\RefreshDatabase;
use Tests\TestCase;

class BukuTest extends TestCase
{
    use RefreshDatabase; // Reset database sebelum setiap test

    // ─── Public routes ────────────────────────────────────────────────────

    public function test_halaman_katalog_bisa_diakses_publik(): void
    {
        // Buat data buku menggunakan factory
        Buku::factory()->count(5)->create();

        $response = $this->get(route('buku.index'));

        $response->assertStatus(200);
        $response->assertViewIs('buku.index');
        $response->assertViewHas('buku');
    }

    public function test_halaman_detail_tampilkan_info_buku(): void
    {
        $buku = Buku::factory()->create([
            'judul'     => 'Clean Code',
            'pengarang' => 'Robert Martin',
        ]);

        $response = $this->get(route('buku.show', $buku));

        $response->assertStatus(200);
        $response->assertSee('Clean Code');
        $response->assertSee('Robert Martin');
    }

    public function test_halaman_detail_return_404_jika_buku_tidak_ada(): void
    {
        $response = $this->get(route('buku.show', 99999));
        $response->assertStatus(404);
    }

    // ─── Protected routes ─────────────────────────────────────────────────

    public function test_tambah_buku_redirect_jika_belum_login(): void
    {
        $response = $this->get(route('buku.create'));
        $response->assertRedirect(route('login'));
    }

    public function test_admin_bisa_tambah_buku(): void
    {
        $admin = Anggota::factory()->create(['role' => 'admin']);

        $response = $this->actingAs($admin)
            ->post(route('buku.store'), [
                'judul'     => 'Test Buku Baru',
                'pengarang' => 'Penulis Test',
                'tahun'     => 2024,
                'harga'     => 100000,
                'stok'      => 10,
                'kategori'  => 'Teknologi',
            ]);

        $response->assertRedirect();
        $response->assertSessionHas('sukses');

        $this->assertDatabaseHas('buku', [
            'judul'     => 'Test Buku Baru',
            'pengarang' => 'Penulis Test',
        ]);
    }

    public function test_anggota_biasa_tidak_bisa_tambah_buku(): void
    {
        $anggota = Anggota::factory()->create(['role' => 'anggota']);

        $response = $this->actingAs($anggota)
            ->post(route('buku.store'), [
                'judul'     => 'Test',
                'pengarang' => 'Test',
                'tahun'     => 2024,
                'harga'     => 0,
                'stok'      => 0,
                'kategori'  => 'Umum',
            ]);

        $response->assertStatus(403);
        $this->assertDatabaseMissing('buku', ['judul' => 'Test', 'pengarang' => 'Test']);
    }

    public function test_validasi_judul_wajib_diisi(): void
    {
        $admin = Anggota::factory()->create(['role' => 'admin']);

        $response = $this->actingAs($admin)
            ->post(route('buku.store'), [
                'judul' => '',  // kosong — harus gagal validasi
                'pengarang' => 'Test',
                'tahun' => 2024,
                'harga' => 0,
                'stok' => 0,
                'kategori' => 'Umum',
            ]);

        $response->assertSessionHasErrors('judul');
        $this->assertDatabaseEmpty('buku');
    }

    public function test_soft_delete_buku(): void
    {
        $admin = Anggota::factory()->create(['role' => 'admin']);
        $buku  = Buku::factory()->create();

        $response = $this->actingAs($admin)
            ->delete(route('buku.destroy', $buku));

        $response->assertRedirect(route('buku.index'));
        $this->assertSoftDeleted('buku', ['id' => $buku->id]);
        $this->assertDatabaseHas('buku', ['id' => $buku->id]); // masih ada, tapi soft-deleted
    }
}
```

PHP

```
<?php
// tests/Feature/Api/BukuApiTest.php

class BukuApiTest extends TestCase
{
    use RefreshDatabase;

    public function test_api_buku_mengembalikan_list_dengan_paginasi(): void
    {
        Buku::factory()->count(20)->create();

        $response = $this->getJson('/api/v1/buku');

        $response->assertStatus(200)
                 ->assertJsonStructure([
                     'data' => [
                         '*' => ['id', 'judul', 'pengarang', 'harga', 'stok', 'tersedia'],
                     ],
                     'links' => ['first', 'last', 'prev', 'next'],
                     'meta'  => ['current_page', 'total', 'per_page'],
                 ]);
    }

    public function test_api_login_berhasil_dan_mengembalikan_token(): void
    {
        $anggota = Anggota::factory()->create([
            'email'    => 'test@perpustakaan.id',
            'password' => bcrypt('password123'),
        ]);

        $response = $this->postJson('/api/v1/auth/login', [
            'email'    => 'test@perpustakaan.id',
            'password' => 'password123',
        ]);

        $response->assertStatus(200)
                 ->assertJsonStructure(['token', 'user'])
                 ->assertJsonPath('user.email', 'test@perpustakaan.id');

        $this->assertNotNull($response->json('token'));
    }

    public function test_api_tambah_buku_butuh_auth(): void
    {
        $response = $this->postJson('/api/v1/buku', [
            'judul' => 'Test',
        ]);

        $response->assertStatus(401);
    }

    public function test_api_tambah_buku_dengan_token_valid(): void
    {
        $admin = Anggota::factory()->create(['role' => 'admin']);
        $token = $admin->createToken('test')->plainTextToken;

        // Mock queue: pastikan job didispatch tanpa benar-benar kirim email
        Queue::fake();

        $response = $this->withToken($token)
            ->postJson('/api/v1/buku', [
                'judul'     => 'API Test Book',
                'pengarang' => 'API Author',
                'tahun'     => 2024,
                'harga'     => 150000,
                'stok'      => 5,
                'kategori'  => 'Teknologi',
            ]);

        $response->assertStatus(201)
                 ->assertJsonPath('data.judul', 'API Test Book');

        $this->assertDatabaseHas('buku', ['judul' => 'API Test Book']);
    }
}
```

#### [[36. Unit Test dan Mocking]]

PHP

```
<?php
// tests/Unit/BukuTest.php

namespace Tests\Unit;

use App\Models\Buku;
use PHPUnit\Framework\TestCase;

class BukuTest extends TestCase
{
    // Unit test: tidak butuh database, test logika murni
    public function test_buku_tersedia_jika_stok_lebih_dari_nol(): void
    {
        $buku = new Buku();
        $buku->stok = 5;

        $this->assertTrue($buku->tersedia());
    }

    public function test_buku_tidak_tersedia_jika_stok_nol(): void
    {
        $buku = new Buku();
        $buku->stok = 0;

        $this->assertFalse($buku->tersedia());
    }

    /**
     * @dataProvider providerFormatHarga
     */
    public function test_format_harga(float $harga, string $expected): void
    {
        $buku = new Buku();
        $buku->harga = $harga;

        $this->assertSame($expected, $buku->harga_format);
    }

    public static function providerFormatHarga(): array
    {
        return [
            'nol'     => [0,       'Rp 0'],
            'seribu'  => [1000,    'Rp 1.000'],
            'standar' => [150000,  'Rp 150.000'],
            'mahal'   => [1500000, 'Rp 1.500.000'],
        ];
    }
}

// tests/Unit/Services/PeminjamanServiceTest.php
class PeminjamanServiceTest extends TestCase
{
    public function test_tidak_bisa_pinjam_jika_stok_habis(): void
    {
        // Mock BukuRepository
        $bukuRepo = $this->createMock(BukuRepository::class);
        $bukuRepo->method('findById')
                 ->willReturn(['id' => 1, 'judul' => 'Test', 'stok' => 0]);

        $service = new PeminjamanService($bukuRepo, $this->createMock(AnggotaRepository::class));

        $this->expectException(\DomainException::class);
        $this->expectExceptionMessage('Buku tidak tersedia');

        $service->pinjamBuku(bukuId: 1, anggotaId: 1);
    }
}
```

Bash

```
# Jalankan semua test
php artisan test

# Jalankan test tertentu
php artisan test tests/Feature/BukuTest.php
php artisan test --filter=test_admin_bisa_tambah_buku

# Dengan coverage report (butuh Xdebug atau pcov)
php artisan test --coverage
php artisan test --coverage --min=80  # fail jika coverage < 80%

# Parallel testing (lebih cepat)
php artisan test --parallel
```

---

### Q. Service Container — Arsitektur yang Bersih

> 💡 **Benang Merah**: Testing membutuhkan kemampuan mengganti dependency (mocking). Service Container adalah yang memungkinkan ini. Memahami Service Container membuka pintu ke arsitektur yang lebih bersih.

#### [[37. Service Container dan Dependency Injection]]

PHP

```
<?php
// app/Repositories/BukuRepository.php — Interface

namespace App\Repositories;

interface BukuRepositoryInterface
{
    public function findAll(int $page, int $perPage): \Illuminate\Contracts\Pagination\LengthAwarePaginator;
    public function findById(int $id): \App\Models\Buku;
    public function create(array $data): \App\Models\Buku;
    public function update(\App\Models\Buku $buku, array $data): \App\Models\Buku;
    public function delete(\App\Models\Buku $buku): bool;
}

// app/Repositories/EloquentBukuRepository.php — Implementasi
class EloquentBukuRepository implements BukuRepositoryInterface
{
    public function findAll(int $page = 1, int $perPage = 15): LengthAwarePaginator
    {
        return Buku::latest()->paginate($perPage, page: $page);
    }

    public function findById(int $id): Buku
    {
        return Buku::findOrFail($id);
    }

    public function create(array $data): Buku
    {
        return Buku::create($data);
    }

    // ... dll
}

// app/Providers/AppServiceProvider.php — binding
class AppServiceProvider extends ServiceProvider
{
    public function register(): void
    {
        // Bind interface ke implementasi konkret
        $this->app->bind(
            BukuRepositoryInterface::class,
            EloquentBukuRepository::class,
        );

        // Singleton: satu instance untuk seluruh request
        $this->app->singleton(
            PeminjamanService::class,
            fn($app) => new PeminjamanService(
                $app->make(BukuRepositoryInterface::class),
                $app->make(AnggotaRepositoryInterface::class),
            ),
        );
    }
}

// app/Http/Controllers/BukuController.php — gunakan interface
class BukuController extends Controller
{
    // Inject interface, bukan implementasi konkret
    // → mudah di-swap atau di-mock saat testing
    public function __construct(
        private BukuRepositoryInterface $bukuRepository,
    ) {}

    public function index(Request $request): View
    {
        $buku = $this->bukuRepository->findAll(
            page: $request->integer('page', 1),
        );
        return view('buku.index', compact('buku'));
    }
}
```

---

### R. Caching — Aplikasi yang Lebih Cepat

#### [[38. Implementasi Caching yang Strategis]]

PHP

```
<?php
// app/Http/Controllers/BukuController.php

use Illuminate\Support\Facades\Cache;

public function index(Request $request): View
{
    // Cache key yang unik per kombinasi parameter
    $cacheKey = 'buku.index.' . md5(json_encode($request->query()));

    // remember: ambil dari cache, atau jalankan closure dan simpan hasilnya
    $buku = Cache::remember($cacheKey, now()->addMinutes(5), function () use ($request) {
        return Buku::query()
            ->when($request->filled('kategori'), fn($q) => $q->kategori($request->kategori))
            ->when($request->filled('cari'), fn($q) => $q->pencarian($request->cari))
            ->paginate(12);
    });

    return view('buku.index', compact('buku'));
}

// Invalidate cache saat data berubah
public function store(StoreBukuRequest $request): RedirectResponse
{
    $buku = Buku::create($request->validated());

    // Hapus semua cache yang terkait dengan listing buku
    Cache::forget('buku.index.*');
    // Atau dengan tags (butuh Redis/Memcached):
    Cache::tags(['buku'])->flush();

    return redirect()->route('buku.index');
}

// app/Models/Buku.php — auto-invalidate via Eloquent events
class Buku extends Model
{
    protected static function booted(): void
    {
        // Otomatis clear cache setiap kali buku dibuat/diupdate/dihapus
        static::saved(fn() => Cache::tags(['buku'])->flush());
        static::deleted(fn() => Cache::tags(['buku'])->flush());
    }
}

// Atomic locks: cegah race condition
public function pinjamBuku(int $bukuId, int $anggotaId): Peminjaman
{
    $lock = Cache::lock("pinjam.buku.{$bukuId}", 10); // lock 10 detik

    if (!$lock->get()) {
        throw new \RuntimeException('Permintaan sedang diproses. Coba lagi.');
    }

    try {
        $buku = Buku::lockForUpdate()->findOrFail($bukuId);

        if ($buku->stok < 1) {
            throw new \DomainException('Buku tidak tersedia.');
        }

        $buku->decrement('stok');

        return Peminjaman::create([
            'buku_id'        => $bukuId,
            'anggota_id'     => $anggotaId,
            'tanggal_pinjam' => now()->toDateString(),
            'batas_kembali'  => now()->addDays(14)->toDateString(),
        ]);
    } finally {
        $lock->release(); // selalu release lock
    }
}
```

PHP

```
<?php
// config/.env — konfigurasi cache driver
CACHE_STORE=redis  // file | database | redis | memcached

// Redis untuk cache dan session: performa optimal
REDIS_HOST=127.0.0.1
REDIS_PORT=6379
SESSION_DRIVER=redis
CACHE_STORE=redis
QUEUE_CONNECTION=redis
```

---

### 🏗️ Checkpoint Level 6

text

```
✅ Checklist sebelum lanjut ke Level 7:

TESTING:
├── php artisan test: semua test hijau
├── Feature test: BukuTest, Api/BukuApiTest, Auth/LoginTest
├── Unit test: BukuTest (tersedia, formatHarga)
├── Mocking: Queue::fake(), Mail::fake() dipakai di test
└── Coverage: minimal 70% untuk controllers dan models

SERVICE CONTAINER:
├── Interface: BukuRepositoryInterface
├── Implementasi: EloquentBukuRepository
├── Binding di AppServiceProvider
├── Controller inject interface, bukan class konkret
└── Unit test bisa mock repository via interface

CACHING:
├── Cache::remember() di controller untuk query berat
├── Cache invalidation saat data berubah
├── Atomic lock untuk operasi kritis (pinjam buku)
└── Cache driver: redis (atau file untuk development)

KEBIASAAN:
├── Tulis test setelah buat fitur
├── php artisan test sebelum commit
└── Refactor dengan percaya diri karena ada test

Git: feat: add test suite, Service Container DI, and Redis caching
```

---

## 🟣 LEVEL 7: ARSITEKTUR LANJUTAN DAN DEVOPS (Minggu 32+)

> **Tema**: _"Dari aplikasi yang bekerja ke sistem enterprise-grade yang siap production"_  
> **Benang Merah**: Aplikasi complete (Level 6) → arsitektur yang scalable → DevOps → monitoring → ekosistem Laravel lanjutan  
> **Output**: Sistem perpustakaan production-ready dengan CI/CD, monitoring, dan arsitektur yang bersih

---

### S. Arsitektur Lanjutan

#### [[39. Repository dan Service Pattern — Arsitektur yang Maintainable]]

text

```
Benang Merah Bagian S:
Service Container dan DI (Level 6) →
Repository Pattern: pisahkan akses data dari business logic →
Service Layer: business logic yang tidak bergantung pada framework →
Action Pattern: satu class, satu tanggung jawab →
Scalable: mudah diganti, ditest, dan diextend
```

PHP

```
<?php
// Struktur yang direkomendasikan untuk project menengah-besar:

app/
├── Actions/                    ← Satu class, satu aksi bisnis
│   ├── Buku/
│   │   ├── TambahBukuAction.php
│   │   ├── UpdateBukuAction.php
│   │   └── PinjamBukuAction.php
│   └── Anggota/
│       ├── DaftarAnggotaAction.php
│       └── RenewalKeanggotaanAction.php
│
├── Data/                       ← Data Transfer Object (DTO)
│   ├── BukuData.php
│   └── PeminjamanData.php
│
├── Repositories/
│   ├── Contracts/
│   │   └── BukuRepositoryInterface.php
│   └── Eloquent/
│       └── EloquentBukuRepository.php
│
└── Services/
    ├── PeminjamanService.php
    └── NotifikasiService.php

// app/Actions/Buku/PinjamBukuAction.php
class PinjamBukuAction
{
    public function __construct(
        private BukuRepositoryInterface    $bukuRepo,
        private AnggotaRepositoryInterface $anggotaRepo,
        private PeminjamanRepositoryInterface $peminjamanRepo,
    ) {}

    public function execute(int $bukuId, int $anggotaId): Peminjaman
    {
        // Semua business logic di sini
        $buku    = $this->bukuRepo->findById($bukuId);
        $anggota = $this->anggotaRepo->findById($anggotaId);

        if (!$buku->tersedia()) {
            throw new \DomainException('Buku tidak tersedia untuk dipinjam.');
        }

        if ($anggota->peminjamanAktif()->count() >= $anggota->getBatasPinjam()) {
            throw new \DomainException('Anggota telah mencapai batas peminjaman.');
        }

        if ($anggota->memilikiDenda()) {
            throw new \DomainException('Anggota memiliki denda yang belum dibayar.');
        }

        return DB::transaction(function () use ($buku, $anggota) {
            $this->bukuRepo->kurangiStok($buku->id);

            $peminjaman = $this->peminjamanRepo->create([
                'buku_id'        => $buku->id,
                'anggota_id'     => $anggota->id,
                'tanggal_pinjam' => now()->toDateString(),
                'batas_kembali'  => now()->addDays(14)->toDateString(),
            ]);

            BukuDipinjam::dispatch($peminjaman);

            return $peminjaman;
        });
    }
}

// Controller jadi sangat tipis:
class PeminjamanController extends Controller
{
    public function store(Request $request, PinjamBukuAction $action): RedirectResponse
    {
        try {
            $peminjaman = $action->execute(
                bukuId:    $request->integer('buku_id'),
                anggotaId: auth()->id(),
            );

            return redirect()
                ->route('peminjaman.show', $peminjaman)
                ->with('sukses', 'Buku berhasil dipinjam!');

        } catch (\DomainException $e) {
            return back()->with('error', $e->getMessage());
        }
    }
}
```

---

### T. DevOps dan Deployment

#### [[40. Deployment ke VPS — Langkah Demi Langkah]]

Bash

```
# ─── Di SERVER (Ubuntu 22.04) ─────────────────────────────────────────────

# Install dependencies
sudo apt update && sudo apt upgrade -y
sudo apt install -y nginx mysql-server php8.3-fpm php8.3-mysql \
    php8.3-mbstring php8.3-xml php8.3-curl php8.3-zip php8.3-redis \
    composer git supervisor redis-server certbot python3-certbot-nginx

# Setup project
cd /var/www
sudo git clone https://github.com/username/perpustakaan.git
sudo chown -R www-data:www-data perpustakaan/
cd perpustakaan

composer install --optimize-autoloader --no-dev
cp .env.example .env
php artisan key:generate

# Konfigurasi .env untuk production:
# APP_ENV=production
# APP_DEBUG=false
# DB_*, REDIS_*, MAIL_* sesuaikan

php artisan migrate --force
php artisan db:seed --class=DatabaseSeeder --force

# Optimize untuk production
php artisan optimize          # cache config, route, view
php artisan storage:link      # buat symlink storage → public

# Set permission
sudo chown -R www-data:www-data storage/ bootstrap/cache/
sudo chmod -R 775 storage/ bootstrap/cache/
```

nginx

```
# /etc/nginx/sites-available/perpustakaan
server {
    listen 80;
    server_name perpustakaan.kota.id www.perpustakaan.kota.id;

    root /var/www/perpustakaan/public;
    index index.php;

    # Keamanan: sembunyikan versi server
    server_tokens off;

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;

    # Laravel routing: semua ke index.php
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    # PHP processing
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
        fastcgi_hide_header X-Powered-By;
    }

    # Sembunyikan file sensitif
    location ~ /\.(env|git|htaccess) {
        deny all;
    }

    # Cache static files
    location ~* \.(jpg|jpeg|png|gif|ico|css|js|woff2)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

Bash

```
# SSL dengan Let's Encrypt
sudo certbot --nginx -d perpustakaan.kota.id -d www.perpustakaan.kota.id
# Certbot otomatis modifikasi nginx config untuk HTTPS
```

ini

```
# /etc/supervisor/conf.d/perpustakaan-worker.conf
# Supervisor menjaga queue worker tetap berjalan

[program:perpustakaan-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /var/www/perpustakaan/artisan queue:work redis --sleep=3 --tries=3 --max-time=3600
autostart=true
autorestart=true
stopasgroup=true
killasgroup=true
user=www-data
numprocs=2        ; jalankan 2 worker process
redirect_stderr=true
stdout_logfile=/var/www/perpustakaan/storage/logs/worker.log
stopwaitsecs=3600

[program:perpustakaan-scheduler]
process_name=%(program_name)s
command=php /var/www/perpustakaan/artisan schedule:work
autostart=true
autorestart=true
user=www-data
redirect_stderr=true
stdout_logfile=/var/www/perpustakaan/storage/logs/scheduler.log
```

#### [[41. CI/CD dengan GitHub Actions]]

YAML

```
# .github/workflows/ci.yml

name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    name: Test Suite
    runs-on: ubuntu-latest

    services:
      mysql:
        image: mysql:8.0
        env:
          MYSQL_ROOT_PASSWORD: password
          MYSQL_DATABASE: perpustakaan_test
        ports: ['3306:3306']
        options: --health-cmd="mysqladmin ping" --health-retries=3

      redis:
        image: redis:7
        ports: ['6379:6379']
        options: --health-cmd="redis-cli ping" --health-retries=3

    steps:
      - uses: actions/checkout@v4

      - name: Setup PHP
        uses: shivammathur/setup-php@v2
        with:
          php-version: '8.3'
          extensions: dom, curl, libxml, mbstring, zip, pdo, mysql, redis
          coverage: xdebug

      - name: Cache Composer dependencies
        uses: actions/cache@v3
        with:
          path: vendor
          key: ${{ runner.os }}-composer-${{ hashFiles('composer.lock') }}

      - name: Install dependencies
        run: composer install --prefer-dist --no-progress

      - name: Setup environment
        run: |
          cp .env.example .env.testing
          php artisan key:generate --env=testing

      - name: Run migrations
        run: php artisan migrate --env=testing
        env:
          DB_CONNECTION: mysql
          DB_HOST: 127.0.0.1
          DB_DATABASE: perpustakaan_test
          DB_USERNAME: root
          DB_PASSWORD: password

      - name: Run tests with coverage
        run: php artisan test --coverage --min=70 --parallel
        env:
          DB_CONNECTION: mysql
          DB_HOST: 127.0.0.1
          DB_DATABASE: perpustakaan_test
          DB_USERNAME: root
          DB_PASSWORD: password
          REDIS_HOST: 127.0.0.1

      - name: Run static analysis
        run: ./vendor/bin/phpstan analyse app tests --level=5

  deploy:
    name: Deploy to Production
    needs: test                    # Hanya deploy jika test lulus
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'  # Hanya dari branch main

    steps:
      - name: Deploy via SSH
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SERVER_SSH_KEY }}
          script: |
            cd /var/www/perpustakaan
            git pull origin main
            composer install --optimize-autoloader --no-dev
            php artisan migrate --force
            php artisan optimize
            php artisan queue:restart
            sudo systemctl reload nginx
```

---

### U. Monitoring dan Pemeliharaan

#### [[42. Laravel Telescope dan Pulse — Monitoring Aplikasi]]

Bash

```
# Telescope: debugging dan monitoring (development)
composer require laravel/telescope --dev
php artisan telescope:install
php artisan migrate

# Pulse: monitoring production
composer require laravel/pulse
php artisan vendor:publish --provider="Laravel\Pulse\PulseServiceProvider"
php artisan migrate
```

PHP

```
<?php
// Jadwal task otomatis dengan Scheduler
// routes/console.php

use Illuminate\Support\Facades\Schedule;

// Kirim pengingat ke anggota yang belum mengembalikan buku
Schedule::command('perpustakaan:kirim-pengingat')
         ->dailyAt('09:00')
         ->emailOutputOnFailure(config('mail.admin'));

// Hitung denda otomatis setiap tengah malam
Schedule::command('perpustakaan:hitung-denda')
         ->daily()
         ->withoutOverlapping(); // jangan jalankan jika yang sebelumnya masih berjalan

// Backup database mingguan
Schedule::command('backup:run --only-db')
         ->weekly()
         ->at('02:00')
         ->onSuccess(function () {
             \Log::info('Backup database berhasil');
         })
         ->onFailure(function () {
             \Log::error('Backup database gagal!');
             // kirim notifikasi ke admin
         });

// Cleanup log bulanan
Schedule::command('perpustakaan:cleanup-log')
         ->monthly();
```

---

### V. Ekosistem Laravel Lanjutan

#### [[43. Filament — Admin Panel yang Powerful]]

Bash

```
composer require filament/filament:"^3.0"
php artisan filament:install --panels
php artisan make:filament-user    # buat admin user
php artisan make:filament-resource Buku --generate  # buat resource CRUD otomatis
```

PHP

```
<?php
// app/Filament/Resources/BukuResource.php

namespace App\Filament\Resources;

use App\Models\Buku;
use Filament\Forms;
use Filament\Resources\Resource;
use Filament\Tables;

class BukuResource extends Resource
{
    protected static ?string $model = Buku::class;
    protected static ?string $navigationIcon = 'heroicon-o-book-open';
    protected static ?string $navigationLabel = 'Koleksi Buku';

    public static function form(Forms\Form $form): Forms\Form
    {
        return $form->schema([
            Forms\Components\TextInput::make('judul')
                ->required()
                ->maxLength(200),

            Forms\Components\TextInput::make('pengarang')
                ->required()
                ->maxLength(100),

            Forms\Components\Select::make('kategori')
                ->options(['Teknologi' => 'Teknologi', 'Fiksi' => 'Fiksi'])
                ->required(),

            Forms\Components\TextInput::make('harga')
                ->numeric()
                ->prefix('Rp')
                ->required(),

            Forms\Components\TextInput::make('stok')
                ->numeric()
                ->minValue(0)
                ->required(),

            Forms\Components\Textarea::make('deskripsi')
                ->columnSpanFull(),

            Forms\Components\FileUpload::make('sampul')
                ->image()
                ->imageResizeWidth(400),
        ]);
    }

    public static function table(Tables\Table $table): Tables\Table
    {
        return $table
            ->columns([
                Tables\Columns\TextColumn::make('judul')->searchable()->sortable(),
                Tables\Columns\TextColumn::make('pengarang')->searchable(),
                Tables\Columns\TextColumn::make('kategori')->badge(),
                Tables\Columns\TextColumn::make('harga')->money('IDR')->sortable(),
                Tables\Columns\TextColumn::make('stok')
                    ->badge()
                    ->color(fn($state) => $state > 0 ? 'success' : 'danger'),
            ])
            ->filters([
                Tables\Filters\SelectFilter::make('kategori')
                    ->options(['Teknologi' => 'Teknologi', 'Fiksi' => 'Fiksi']),
            ])
            ->actions([
                Tables\Actions\EditAction::make(),
                Tables\Actions\DeleteAction::make(),
            ]);
    }
}
```

#### [[44. Livewire — Reactive UI Tanpa Full SPA]]

Bash

```
composer require livewire/livewire
php artisan make:livewire KatalogBuku
php artisan make:livewire FormPeminjaman
```

PHP

```
<?php
// app/Livewire/KatalogBuku.php

namespace App\Livewire;

use App\Models\Buku;
use Livewire\Component;
use Livewire\WithPagination;

class KatalogBuku extends Component
{
    use WithPagination;

    // Property ini reactive: perubahan otomatis trigger re-render
    public string $pencarian = '';
    public string $kategori = '';
    public string $urutan = 'judul';

    // Saat $pencarian berubah, reset ke halaman 1
    public function updatingPencarian(): void
    {
        $this->resetPage();
    }

    public function render()
    {
        $buku = Buku::query()
            ->when($this->pencarian, fn($q) => $q->pencarian($this->pencarian))
            ->when($this->kategori, fn($q) => $q->kategori($this->kategori))
            ->orderBy($this->urutan)
            ->paginate(12);

        return view('livewire.katalog-buku', compact('buku'));
    }
}
```

PHP

```
{{-- resources/views/livewire/katalog-buku.blade.php --}}
<div>
    {{-- Input pencarian: wire:model = bind ke property Livewire --}}
    <input
        type="text"
        wire:model.live.debounce.300ms="pencarian"  {{-- debounce 300ms --}}
        placeholder="Cari judul atau pengarang..."
        class="form-input"
    >

    {{-- Filter kategori --}}
    <select wire:model.live="kategori" class="form-select">
        <option value="">Semua Kategori</option>
        <option value="Teknologi">Teknologi</option>
        <option value="Fiksi">Fiksi</option>
    </select>

    {{-- Loading indicator --}}
    <div wire:loading class="spinner">Memuat...</div>

    {{-- Daftar buku --}}
    <div class="grid grid-cols-3 gap-4" wire:loading.class="opacity-50">
        @forelse($buku as $item)
            <x-buku-card :buku="$item" />
        @empty
            <p>Tidak ada hasil.</p>
        @endforelse
    </div>

    {{ $buku->links() }}
</div>
```

---

### 🏗️ Checkpoint Level 7 (Final)

text

```
✅ Checklist Akhir — Sistem Perpustakaan Production-Ready:

ARSITEKTUR:
├── Action pattern: PinjamBukuAction, KembalikanBukuAction
├── Repository pattern dengan interface
├── Service Container binding di AppServiceProvider
└── Controller tipis: hanya delegate ke Action

TESTING:
├── php artisan test: 100% hijau
├── Coverage: >70% untuk code yang critical
├── CI: GitHub Actions menjalankan test setiap push
└── CD: Auto-deploy ke VPS jika test lulus

DEVOPS:
├── Nginx dikonfigurasi dengan security headers
├── SSL/HTTPS aktif dengan Let's Encrypt
├── Supervisor menjalankan queue worker
├── Scheduler berjalan untuk task otomatis
└── Monitoring via Telescope (dev) atau Pulse (prod)

EKOSISTEM:
├── Filament: admin panel untuk pustakawan
├── Livewire: katalog buku yang reactive (search tanpa reload)
└── Backup otomatis via spatie/laravel-backup

KEAMANAN PRODUCTION:
├── APP_DEBUG=false
├── Error tidak tampil ke user
├── .env tidak bisa diakses dari web
├── Rate limiting di semua endpoint API
└── HTTPS enforced

Git: feat: production-ready system with CI/CD, monitoring, and ecosystem
```

---

## 📊 Ringkasan Progress Tracking

### Satu Project, 7 Level Enhancement

text

```
Level 1: Landing page perpustakaan — routing, Blade, layout, komponen
  + Level 2: + Controller, View lengkap, Request object
  + Level 3: + Migration, Eloquent ORM, relasi, seeder, factory
  + Level 4: + Validasi, Auth (Breeze), Authorization (Policy), Middleware
  + Level 5: + REST API, Sanctum, Queue, Event-Listener
  + Level 6: + Testing (Feature + Unit), Service Container, Caching
  + Level 7: + Action pattern, DevOps (CI/CD, Nginx, SSL), Filament, Livewire
```

### Tabel Progress

|Level|Poin|Durasi|Output Konkret|
|---|---|---|---|
|🟢 **1**|1-13|Minggu 1-4|Landing page dengan routing, Blade layout, komponen|
|🔵 **2**|14-17|Minggu 4-7|Katalog buku dengan Controller dan View lengkap|
|🟡 **3**|18-23|Minggu 7-12|Database MySQL, Eloquent, relasi, paginasi|
|🟠 **4**|24-29|Minggu 12-18|Auth, validasi, policy, middleware kustom|
|🔴 **5**|30-34|Minggu 18-24|REST API, Sanctum, Queue, Event system|
|⚫ **6**|35-38|Minggu 24-32|Test suite, DI/Service Container, Redis caching|
|🟣 **7**|39-44|Minggu 32+|CI/CD, deployment, Filament, Livewire|

---

### Benang Merah Utama Sepanjang Roadmap

text

```
Poin 1  (request lifecycle)      → Fondasi memahami SEMUA hal di Laravel
Poin 3  (instalasi)              → Project perpustakaan yang tumbuh hingga Level 7
Poin 7  (named route)            → Dipakai di setiap view dan controller
Poin 11 (blade escaped output)   → {{ }} SELALU di semua view — zero XSS
Poin 12 (layout Blade)           → Template induk yang dipakai semua halaman
Poin 14 (resource controller)    → Pola CRUD yang konsisten hingga Level 5+
Poin 18 (migration)              → Database yang bisa di-version-control
Poin 20 (Eloquent model)         → $fillable: mass assignment protection
Poin 21 (eager loading)          → Hindari N+1 — SELALU ingat with()
Poin 24 (Form Request)           → Validasi terpisah dari controller, reusable
Poin 25 (Breeze auth)            → Foundation auth untuk Level 5 (Sanctum)
Poin 28 (Policy)                 → Otorisasi yang konsisten di controller & Blade
Poin 30 (API Resource)           → Response JSON yang konsisten dan terkontrol
Poin 31 (Sanctum)                → Token auth yang aman untuk API
Poin 33 (Queue)                  → Proses berat ke background, UX tetap cepat
Poin 35 (Feature test)           → Confidence untuk refactor tanpa takut rusak
Poin 37 (Service Container)      → Arsitektur yang testable dan maintainable
Poin 38 (Cache)                  → Database tidak di-hit setiap request
Poin 39 (Action pattern)         → Business logic yang terisolasi dan testable
Poin 41 (GitHub Actions)         → Otomasi test dan deploy — tidak ada deploy manual
```

---

## 💡 Cara Menggunakan Roadmap Ini

text

```
Setiap poin mengikuti format:
┌──────────────────────────────────────────────────────────┐
│ 💡 Konteks: mengapa fitur/konsep ini ada di Laravel      │
│ 🔗 Benang Merah: koneksi ke poin sebelum dan sesudahnya  │
│ 📋 Kode: implementasi konkret di project perpustakaan    │
│          yang langsung bisa dicoba                       │
│ ✅ Langkah konkret: verifikasi berhasil                  │
└──────────────────────────────────────────────────────────┘
```

**Aturan yang Tidak Boleh Dilanggar:**

1. **`{{ }}` selalu untuk output** di Blade — jangan pernah `{!! !!}` kecuali benar-benar perlu
2. **`findOrFail()` bukan `find()`** — biarkan Laravel yang handle 404
3. **`$fillable` wajib** di setiap model — jangan pernah `protected $guarded = []`
4. **Eager loading `with()`** — selalu cek apakah ada N+1 sebelum push ke production
5. **Form Request** untuk validasi — jangan validasi di controller langsung untuk form yang kompleks
6. **`php artisan test`** sebelum setiap commit — tidak ada excuse
7. **`.env` tidak boleh di-commit** — secret harus di server, bukan di git
8. **`APP_DEBUG=false`** di production — error message tidak boleh bocor ke user

---

_Roadmap Laravel v1.0 — Step-by-Step, Security First, One Project_  
_Setiap baris kode ditulis dengan sadar — pahami mengapa sebelum bagaimana_