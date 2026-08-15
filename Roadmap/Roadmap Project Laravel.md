# Roadmap Laravel: Dari Nol Hingga Production-Ready

## Filosofi Roadmap Ini

> **"Laravel bukan sekadar framework — Laravel adalah cara berpikir tentang membangun aplikasi web yang elegan, aman, dan dapat dipelihara. Memahami mengapa setiap fitur ada jauh lebih penting daripada sekadar hafal sintaksnya"**

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: sistem manajemen perpustakaan dari instalasi → web app → API → production-ready
- **Prasyarat PHP Native Dulu**: roadmap ini mengasumsikan PHP Native sudah dikuasai — Laravel akan terasa natural, bukan magic
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **Laravel Modern**: fokus pada Laravel 11.x — struktur terbaru, bukan cara lama
- **Mengapa sebelum Bagaimana**: pahami alasan di balik setiap keputusan desain Laravel

### Prasyarat Sebelum Memulai

text

```
Sebelum roadmap ini, pastikan sudah memahami:
├── PHP OOP: class, inheritance, interface, trait, namespace
├── Composer: dependency management dan autoloading PSR-4
├── HTTP fundamentals: request, response, method, status code
├── SQL dasar: SELECT, INSERT, UPDATE, DELETE, JOIN
├── Command line: terminal, path, environment variable
├── Git: init, add, commit, push, branch
└── HTML + CSS dasar untuk memahami view layer
```

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: "Laravel Pertama" — install, struktur, routing, Blade, Artisan
    ↓ (enhance)
Level 2: + Controller, View → Halaman katalog perpustakaan
    ↓ (enhance)
Level 3: + Migration, Eloquent → Data buku dari MySQL
    ↓ (enhance)
Level 4: + Validasi, Auth, Policy → Admin panel yang aman
    ↓ (enhance)
Level 5: + REST API, Sanctum, Queue, Event → API perpustakaan
    ↓ (enhance)
Level 6: + Testing, Service Container, Cache → Production-ready
    ↓ (enhance)
Level 7: + Arsitektur lanjutan, DevOps, Ekosistem → Enterprise-grade
```

---

## 🟢 LEVEL 1: FONDASI LARAVEL (Minggu 1-4)

> **Tema**: _"Memahami apa yang Laravel lakukan di balik layar dan mengapa"_  
> **Benang Merah**: Cara Laravel memproses request → Install → Konfigurasi → Routing → Blade → Artisan  
> **Output**: Halaman landing perpustakaan yang berjalan dengan Laravel

---

### A. Memahami Laravel Sebelum Menulis Kode

> 💡 **Mengapa dimulai di sini?** Jika kamu pernah menulis PHP native, kamu sudah membangun router sendiri, koneksi database sendiri, dan template engine sendiri. Laravel sudah melakukan semua itu — dengan jauh lebih baik. Memahami _apa yang digantikan_ Laravel membuat semuanya masuk akal.

text

```
Benang Merah Bagian A:
PHP Native: router manual, query string, require/include tersebar →
Laravel: semua itu sudah ada dan terintegrasi dengan baik →
Request masuk ke public/index.php → bootstrap → Service Container →
Pipeline middleware → Route matching → Controller → Response →
Memahami ini = memahami semua yang akan dipelajari di Level 1-7
```

#### [[1. Apa yang Laravel Gantikan dari PHP Native]]

- Jika sudah belajar PHP native, kamu sudah membangun sendiri:

text

```
PHP Native yang kamu tulis          Laravel yang menggantikannya
─────────────────────────────────   ──────────────────────────────────────
if/switch untuk routing          →  Route::get('/buku', ...)
PDO connection manual            →  Eloquent ORM
htmlspecialchars() manual        →  Blade {{ }} otomatis escaped
$_POST, $_GET manual             →  Request $request (type-hinted, validated)
session_start() + $_SESSION      →  Session facade
header('Location: ...')          →  redirect()->route('...')
require 'header.php'             →  @extends('layouts.app')
JSON encode manual               →  response()->json()
CSRF token manual                →  @csrf otomatis
password_hash manual             →  Hash::make() + Breeze/Jetstream
```

- **Alur request di Laravel — pahami ini, semua hal lain masuk akal:**

text

```
Browser kirim request ke /buku
        ↓
public/index.php          ← satu-satunya file diakses web server
        ↓
bootstrap/app.php         ← inisialisasi Application
        ↓
Service Container         ← resolusi semua dependency
        ↓
HTTP Kernel               ← pipeline middleware global
        ↓
Route Matching            ← routes/web.php atau routes/api.php
        ↓
Route Middleware           ← auth, throttle, dll.
        ↓
Controller@method          ← logika aplikasi
        ↓
Response (View/JSON)      ← HTML atau JSON
        ↓
Browser menerima response
```

- _Langkah konkret_: Buka `public/index.php` di project Laravel — perhatikan betapa sedikitnya kode di sana, tapi itulah entry point seluruh aplikasi

#### [[2. Convention over Configuration — Filosofi Inti Laravel]]

text

```
Convention Laravel yang harus dipahami sejak awal:

Model                    → Tabel Database
─────────────────────    ────────────────────────────
Model Buku               → tabel: buku
Model AnggotaPustaka     → tabel: anggota_pustakas
Model PeminjamanBuku     → tabel: peminjaman_bukus

Method Controller        → Route Resource
─────────────────────    ────────────────────────────
index()                  → GET /buku
create()                 → GET /buku/create
store()                  → POST /buku
show($buku)              → GET /buku/{buku}
edit($buku)              → GET /buku/{buku}/edit
update($buku)            → PUT/PATCH /buku/{buku}
destroy($buku)           → DELETE /buku/{buku}

Lokasi File              → Tujuan
─────────────────────    ────────────────────────────
app/Models/Buku.php      → Model Eloquent
app/Http/Controllers/    → Controller class
resources/views/buku/    → Blade template untuk buku
database/migrations/     → Blueprint struktur tabel
database/seeders/        → Data awal database
```

---

### B. Instalasi dan Konfigurasi

> 💡 **Benang Merah ke A**: Paham apa yang Laravel lakukan. Sekarang setup environment dan buat project.

text

```
Benang Merah Bagian B:
Memahami filosofi Laravel (A) →
Install PHP, Composer, dan tools yang dibutuhkan →
Buat project baru dengan Laravel Installer →
Konfigurasi .env untuk koneksi database →
Jalankan server development →
Verifikasi semua berjalan dengan benar
```

#### [[3. Instalasi Laravel dan Setup Project Perpustakaan]]

Bash

```
# Pastikan prasyarat terpenuhi
php --version      # minimal PHP 8.2
composer --version # Composer 2.x
node --version     # Node.js 18+ untuk asset bundling

# Install Laravel Installer (sekali saja)
composer global require laravel/installer

# Buat project baru
laravel new perpustakaan

# Saat ditanya pilihan:
# Starter kit: None (kita bangun manual untuk belajar)
# Testing framework: PHPUnit
# Database: MySQL

cd perpustakaan

# Atau via Composer jika tidak pakai Laravel Installer:
composer create-project laravel/laravel perpustakaan
```

Bash

```
# Setup environment lokal

# Windows: Laragon (paling direkomendasikan)
# Buat folder di C:\laragon\www\perpustakaan\
# Akses: http://perpustakaan.test

# macOS: Laravel Herd
# Download dari herd.laravel.com
# Otomatis detect folder ~/Herd/

# Semua OS: Laravel Sail (Docker)
# php artisan sail:install
# ./vendor/bin/sail up -d

# Development server manual:
php artisan serve
# Akses: http://localhost:8000
```

#### [[4. Struktur Direktori Laravel — Setiap Folder Ada Tujuannya]]

text

```
perpustakaan/
│
├── app/                        ← Kode aplikasi utama
│   ├── Http/
│   │   ├── Controllers/        ← Controller class
│   │   ├── Middleware/         ← Middleware class
│   │   └── Requests/           ← Form Request (validasi)
│   ├── Models/                 ← Eloquent model
│   └── Providers/              ← Service Provider
│
├── bootstrap/
│   └── app.php                 ← Inisialisasi aplikasi Laravel 11
│                                 (di sini daftarkan middleware, exception handler)
│
├── config/                     ← Konfigurasi (app.php, database.php, dll.)
│
├── database/
│   ├── migrations/             ← Blueprint struktur tabel
│   ├── seeders/                ← Data awal untuk database
│   └── factories/              ← Generator data palsu untuk testing
│
├── public/                     ← Web root — HANYA ini yang diekspos ke internet
│   └── index.php               ← Entry point (jangan tambah file lain di sini)
│
├── resources/
│   ├── views/                  ← Blade template (.blade.php)
│   ├── css/                    ← Source CSS (dikompilasi Vite)
│   └── js/                     ← Source JavaScript (dikompilasi Vite)
│
├── routes/
│   ├── web.php                 ← Route web (session, CSRF, cookie)
│   ├── api.php                 ← Route API (stateless, prefix /api)
│   └── console.php             ← Artisan command routes
│
├── storage/                    ← File yang dihasilkan aplikasi
│   ├── app/public/             ← File upload user (symlink ke public/storage)
│   └── logs/                   ← Log file
│
├── tests/                      ← File test (Unit dan Feature)
│
├── .env                        ← Environment variables (JANGAN commit ke git!)
├── .env.example                ← Template .env (commit ini ke git)
├── artisan                     ← CLI tool Laravel
└── composer.json               ← Dependency PHP

File yang PALING SERING disentuh saat development:
├── routes/web.php              → definisikan URL dan handler
├── app/Http/Controllers/       → logika request
├── resources/views/            → tampilan HTML
├── database/migrations/        → struktur database
├── app/Models/                 → interaksi database
└── .env                        → konfigurasi environment
```

#### [[5. Konfigurasi .env — Environment yang Benar]]

Bash

```
# .env — konfigurasi spesifik environment
# Setiap developer punya .env sendiri
# Server production punya .env sendiri
# File ini TIDAK boleh di-commit ke git!

APP_NAME="Perpustakaan Digital"
APP_ENV=local               # local | staging | production
APP_KEY=                    # generate dengan: php artisan key:generate
APP_DEBUG=true              # TRUE hanya di local! FALSE di production
APP_URL=http://localhost:8000

# Database
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=perpustakaan_db
DB_USERNAME=root
DB_PASSWORD=

# Cache dan Session
CACHE_STORE=file            # file | redis | memcached
SESSION_DRIVER=file         # file | database | redis | cookie
SESSION_LIFETIME=120        # dalam menit

# Queue
QUEUE_CONNECTION=sync       # sync (langsung) | database | redis

# Mail
MAIL_MAILER=log             # log di dev (lihat di storage/logs)
```

Bash

```
# Setelah edit .env, jalankan perintah ini:
php artisan key:generate    # generate APP_KEY — WAJIB!

# Buat database di MySQL:
# CREATE DATABASE perpustakaan_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

# Test koneksi database:
php artisan db:show

# Jika config di-cache (biasanya di production), clear dulu:
php artisan config:clear
php artisan config:cache    # hanya untuk production
```

#### [[6. Artisan CLI — Teman Terbaik Developer Laravel]]

> 💡 **Mengapa Artisan?** Di PHP native, kamu buat file secara manual. Artisan membuatkan file dengan _struktur yang benar_, di _lokasi yang benar_, dengan _namespace yang benar_. Biasakan pakai Artisan untuk semua pembuatan file.

Bash

```
# Lihat semua perintah
php artisan list

# Perintah yang PALING SERING dipakai:

# ─── Development Server ────────────────────────────────────────────
php artisan serve                    # jalankan server di localhost:8000

# ─── Generate File ────────────────────────────────────────────────
php artisan make:model Buku -mfs    # Model + Migration + Factory + Seeder
php artisan make:controller BukuController --resource  # Resource controller
php artisan make:middleware CekStatusAnggota
php artisan make:request StoreBukuRequest
php artisan make:policy BukuPolicy --model=Buku
php artisan make:event BukuDipinjam
php artisan make:listener KirimNotifikasi --event=BukuDipinjam
php artisan make:job KirimEmailKonfirmasi
php artisan make:mail KonfirmasiPeminjamanMail --markdown
php artisan make:test BukuTest --feature

# ─── Database ─────────────────────────────────────────────────────
php artisan migrate                  # jalankan migration baru
php artisan migrate:status           # lihat status migration
php artisan migrate:rollback         # rollback migration terakhir
php artisan migrate:fresh --seed     # drop semua + migrate + seed (dev only!)
php artisan db:seed                  # jalankan semua seeder

# ─── Informasi ────────────────────────────────────────────────────
php artisan route:list               # lihat semua route terdaftar
php artisan route:list --path=buku   # filter route by path
php artisan about                    # info aplikasi Laravel
php artisan config:show database     # lihat konfigurasi database

# ─── Cache dan Optimasi ───────────────────────────────────────────
php artisan optimize                 # cache semua untuk production
php artisan optimize:clear           # clear semua cache
php artisan cache:clear
php artisan config:clear
php artisan route:clear
php artisan view:clear

# ─── Tinker — REPL interaktif ─────────────────────────────────────
php artisan tinker
# Di dalam tinker:
# >>> App\Models\Buku::all()
# >>> App\Models\Buku::count()
# >>> App\Models\Buku::factory()->create()
# >>> App\Models\Buku::where('kategori', 'Teknologi')->get()
```

---

### C. Routing — Mendefinisikan URL Aplikasi

> 💡 **Benang Merah ke B**: Di PHP native, kamu menulis router sendiri dengan switch/if-else. Di Laravel, routing jauh lebih ekspresif, aman, dan powerful.

text

```
Benang Merah Bagian C:
PHP native: router manual dengan switch atau if-else →
Laravel Route: mapping URL ke handler yang elegan →
Named route: URL bisa berubah, nama tidak →
Resource route: 7 route CRUD dengan satu baris →
Route group: organisasi route yang rapi
```

#### [[7. Route Dasar dan Named Route]]

PHP

```
<?php
// routes/web.php

use Illuminate\Support\Facades\Route;

// GET route dengan closure — cocok untuk logika sederhana
Route::get('/', function () {
    return view('beranda', [
        'namaPerpustakaan' => config('app.name'),
        'tahun'            => date('Y'),
    ]);
});

// GET route dengan controller — gunakan ini untuk logika yang lebih kompleks
Route::get('/tentang', [TentangController::class, 'index']);

// Named route: SELALU beri nama pada route!
// Alasan: jika URL berubah dari /buku ke /koleksi-buku,
// kamu hanya ubah di sini, tidak perlu ubah di seluruh view
Route::get('/buku', [BukuController::class, 'index'])->name('buku.index');
Route::get('/buku/{id}', [BukuController::class, 'show'])->name('buku.show');

// Generate URL dari nama route:
// route('buku.index')           → http://localhost/buku
// route('buku.show', ['id' => 5]) → http://localhost/buku/5
// route('buku.show', 5)          → shortcut untuk satu parameter

// Route dengan constraint — validasi parameter di level routing
Route::get('/buku/{id}', [BukuController::class, 'show'])
     ->name('buku.show')
     ->where('id', '[0-9]+');   // id harus angka

Route::get('/kategori/{slug}', [KategoriController::class, 'show'])
     ->name('kategori.show')
     ->where('slug', '[a-z0-9\-]+');
```

#### [[8. Resource Route — CRUD dengan Satu Baris]]

PHP

```
<?php
// routes/web.php

// Resource route: otomatis buat 7 route CRUD
Route::resource('buku', BukuController::class);

// Equivalen dengan 7 route ini:
// GET    /buku              → BukuController@index    (name: buku.index)
// GET    /buku/create       → BukuController@create   (name: buku.create)
// POST   /buku              → BukuController@store    (name: buku.store)
// GET    /buku/{buku}       → BukuController@show     (name: buku.show)
// GET    /buku/{buku}/edit  → BukuController@edit     (name: buku.edit)
// PUT    /buku/{buku}       → BukuController@update   (name: buku.update)
// DELETE /buku/{buku}       → BukuController@destroy  (name: buku.destroy)

// Cek dengan: php artisan route:list

// Hanya route tertentu:
Route::resource('buku', BukuController::class)
     ->only(['index', 'show']);      // hanya index dan show

Route::resource('buku', BukuController::class)
     ->except(['destroy']);          // semua kecuali destroy

// API resource: tanpa create() dan edit() (tidak butuh form)
Route::apiResource('buku', Api\BukuController::class);

// Nested resource: peminjaman milik anggota
Route::resource('anggota.peminjaman', PeminjamanController::class);
// GET /anggota/{anggota}/peminjaman         → index
// POST /anggota/{anggota}/peminjaman        → store
// DELETE /anggota/{anggota}/peminjaman/{peminjaman} → destroy
```

#### [[9. Route Group — Organisasi yang Rapi]]

PHP

```
<?php
// routes/web.php

// Group dengan prefix URL
Route::prefix('admin')->group(function () {
    Route::get('/', [Admin\DashboardController::class, 'index'])
         ->name('admin.dashboard');
    Route::resource('buku', Admin\BukuController::class)
         ->names('admin.buku');
});
// Menghasilkan: /admin/, /admin/buku, /admin/buku/create, dst.

// Group dengan middleware
Route::middleware(['auth'])->group(function () {
    Route::get('/profil', [ProfilController::class, 'show'])->name('profil.show');
    Route::put('/profil', [ProfilController::class, 'update'])->name('profil.update');
});

// Group dengan prefix + middleware + name prefix sekaligus
Route::prefix('admin')
     ->middleware(['auth', 'role:admin'])
     ->name('admin.')
     ->group(function () {
         Route::get('/dashboard', [Admin\DashboardController::class, 'index'])
              ->name('dashboard');    // nama lengkap: admin.dashboard
         Route::resource('buku', Admin\BukuController::class);
         // nama: admin.buku.index, admin.buku.show, dst.
     });

// Verifikasi route yang dihasilkan:
// php artisan route:list --name=admin
```

---

### D. Blade Templating Engine

> 💡 **Benang Merah ke C**: Route mengarahkan ke handler. Handler mengembalikan view. Blade adalah template engine Laravel yang lebih ekspresif dan lebih aman dari PHP biasa di dalam HTML.

text

```
Benang Merah Bagian D:
PHP native: require 'header.php', echo htmlspecialchars($var) →
Blade: @extends, @section, @yield — layout yang elegan →
{{ $var }}: otomatis escaped (XSS protection built-in) →
Direktif: @if, @foreach, @auth — lebih readable →
Komponen: UI yang reusable dan terstruktur
```

#### [[10. Blade Dasar — Output yang Aman dan Direktif]]

PHP

```
{{-- resources/views/buku/index.blade.php --}}

{{-- KOMENTAR Blade: tidak muncul di HTML output --}}

{{-- OUTPUT VARIABEL --}}
{{-- {{ $var }}: otomatis htmlspecialchars() — SELALU gunakan ini --}}
{{ $buku->judul }}
{{ $buku->pengarang }}

{{-- {!! $var !!}: RAW output, TIDAK di-escape --}}
{{-- Gunakan HANYA jika konten sudah dipastikan aman (dari admin, bukan user) --}}
{!! $buku->deskripsiHtml !!}

{{-- KONDISIONAL --}}
@if($buku->stok > 0)
    <span class="badge bg-success">Tersedia ({{ $buku->stok }})</span>
@elseif($buku->stok === 0)
    <span class="badge bg-danger">Habis</span>
@else
    <span class="badge bg-secondary">Status Tidak Diketahui</span>
@endif

{{-- unless: kebalikan @if --}}
@unless($buku->tersedia())
    <p class="text-danger">Buku ini sedang tidak tersedia untuk dipinjam.</p>
@endunless

{{-- isset dan empty --}}
@isset($buku->isbn)
    <p>ISBN: {{ $buku->isbn }}</p>
@endisset

@empty($buku->deskripsi)
    <p class="text-muted">Tidak ada deskripsi.</p>
@endempty

{{-- LOOP --}}
@foreach($katalog as $buku)
    <tr>
        <td>{{ $loop->iteration }}</td>   {{-- 1, 2, 3, ... --}}
        <td>{{ $buku->judul }}</td>
        <td>{{ $buku->pengarang }}</td>
        <td>
            @if($loop->first) {{-- elemen pertama --}} @endif
            @if($loop->last)  {{-- elemen terakhir --}} @endif
            {{-- $loop->index: 0-based --}}
            {{-- $loop->count: total elemen --}}
            {{-- $loop->remaining: sisa --}}
        </td>
    </tr>
@endforeach

{{-- forelse: jika koleksi kosong --}}
@forelse($katalog as $buku)
    <div class="kartu-buku">{{ $buku->judul }}</div>
@empty
    <p class="text-center py-8">Belum ada buku dalam katalog.</p>
@endforelse

{{-- AUTH direktif --}}
@auth
    <p>Selamat datang, {{ auth()->user()->nama }}!</p>
    <a href="{{ route('profil.show') }}">Lihat Profil</a>
@endauth

@guest
    <a href="{{ route('login') }}">Login</a>
    <a href="{{ route('register') }}">Daftar</a>
@endguest

{{-- OTORISASI --}}
@can('create', App\Models\Buku::class)
    <a href="{{ route('buku.create') }}" class="btn btn-primary">+ Tambah Buku</a>
@endcan

@can('update', $buku)
    <a href="{{ route('buku.edit', $buku) }}" class="btn btn-secondary">Edit</a>
@endcan
```

#### [[11. Layout dengan @extends dan @section]]

PHP

```
{{-- resources/views/layouts/app.blade.php --}}
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="csrf-token" content="{{ csrf_token() }}">

    {{-- @yield: placeholder yang diisi child view --}}
    <title>@yield('title', 'Perpustakaan Digital') | {{ config('app.name') }}</title>

    {{-- Asset dengan Vite (default Laravel 11) --}}
    @vite(['resources/css/app.css', 'resources/js/app.js'])

    {{-- @stack: placeholder yang bisa di-push dari child --}}
    @stack('styles')
</head>
<body class="bg-gray-50">

    <nav class="bg-white border-b border-gray-200">
        <div class="max-w-7xl mx-auto px-4 py-3 flex justify-between items-center">
            <a href="{{ route('beranda') }}" class="font-bold text-xl">
                {{ config('app.name') }}
            </a>

            <div class="flex items-center gap-4">
                <a href="{{ route('buku.index') }}">Katalog</a>

                @auth
                    <span class="text-gray-600">{{ auth()->user()->nama }}</span>
                    <form action="{{ route('logout') }}" method="POST" class="inline">
                        @csrf
                        <button type="submit">Logout</button>
                    </form>
                @else
                    <a href="{{ route('login') }}">Login</a>
                @endauth
            </div>
        </div>
    </nav>

    {{-- Flash message --}}
    @if(session('sukses'))
        <div class="bg-green-100 border border-green-400 text-green-700 px-4 py-3 mx-4 mt-4 rounded">
            {{ session('sukses') }}
        </div>
    @endif

    @if(session('error'))
        <div class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 mx-4 mt-4 rounded">
            {{ session('error') }}
        </div>
    @endif

    <main class="max-w-7xl mx-auto px-4 py-8">
        @yield('content')   {{-- Konten dari child view masuk di sini --}}
    </main>

    <footer class="bg-white border-t mt-12 py-6 text-center text-gray-500">
        <p>&copy; {{ date('Y') }} {{ config('app.name') }}</p>
    </footer>

    @stack('scripts')   {{-- Script dari child view --}}
</body>
</html>
```

PHP

```
{{-- resources/views/buku/index.blade.php --}}
@extends('layouts.app')              {{-- Gunakan layout app.blade.php --}}

@section('title', 'Katalog Buku')   {{-- Isi @yield('title') --}}

@section('content')
<div class="flex justify-between items-center mb-6">
    <h1 class="text-3xl font-bold">Katalog Buku</h1>

    @can('create', App\Models\Buku::class)
        <a href="{{ route('buku.create') }}" class="btn btn-primary">
            + Tambah Buku
        </a>
    @endcan
</div>

<div class="grid grid-cols-1 md:grid-cols-3 gap-6">
    @forelse($buku as $item)
        <x-buku-card :buku="$item" />
    @empty
        <div class="col-span-3 text-center py-12 text-gray-500">
            Belum ada buku dalam katalog.
        </div>
    @endforelse
</div>

{{-- Pagination links --}}
<div class="mt-8">
    {{ $buku->links() }}
</div>
@endsection

@push('scripts')
<script>
    console.log('Halaman katalog buku');
</script>
@endpush
```

#### [[12. Komponen Blade — UI yang Reusable]]

Bash

```
# Buat komponen
php artisan make:component BukuCard
# Membuat:
# app/View/Components/BukuCard.php
# resources/views/components/buku-card.blade.php
```

PHP

```
<?php
// app/View/Components/BukuCard.php

namespace App\View\Components;

use App\Models\Buku;
use Illuminate\View\Component;

class BukuCard extends Component
{
    public function __construct(
        public Buku $buku,
        public bool $tampilkanHarga = true,
        public string $ukuran = 'normal',   // 'kecil' | 'normal' | 'besar'
    ) {}

    public function render()
    {
        return view('components.buku-card');
    }
}
```

PHP

```
{{-- resources/views/components/buku-card.blade.php --}}
@props([
    'buku',
    'tampilkanHarga' => true,
    'ukuran'         => 'normal',
])

<div class="bg-white rounded-lg shadow hover:shadow-md transition-shadow
            {{ $ukuran === 'kecil' ? 'p-4' : 'p-6' }}">

    @if($buku->sampul)
        <img src="{{ Storage::url($buku->sampul) }}"
             alt="Sampul {{ $buku->judul }}"
             class="w-full h-48 object-cover rounded mb-4">
    @endif

    <h3 class="font-bold text-lg">{{ $buku->judul }}</h3>
    <p class="text-gray-600">{{ $buku->pengarang }}</p>

    @if($tampilkanHarga)
        <p class="text-blue-600 font-semibold mt-2">
            Rp {{ number_format($buku->harga, 0, ',', '.') }}
        </p>
    @endif

    <div class="mt-3 flex items-center justify-between">
        @if($buku->stok > 0)
            <span class="badge bg-green-100 text-green-800">
                Tersedia ({{ $buku->stok }})
            </span>
        @else
            <span class="badge bg-red-100 text-red-800">Habis</span>
        @endif

        {{-- $slot: konten yang di-inject dari luar --}}
        {{ $slot }}
    </div>
</div>
```

PHP

```
{{-- Cara pakai komponen --}}

{{-- Minimal --}}
<x-buku-card :buku="$buku" />

{{-- Dengan prop --}}
<x-buku-card :buku="$buku" :tampilkan-harga="false" ukuran="kecil" />

{{-- Dengan slot --}}
<x-buku-card :buku="$buku">
    <a href="{{ route('buku.show', $buku) }}" class="btn btn-sm">Detail</a>
    <a href="{{ route('pinjam.store', $buku) }}" class="btn btn-sm btn-primary">Pinjam</a>
</x-buku-card>

{{-- Anonymous component: tanpa class PHP, hanya file blade --}}
{{-- Buat: resources/views/components/alert.blade.php --}}
{{-- Pakai: <x-alert type="success">Buku berhasil ditambahkan!</x-alert> --}}
```

---

### 🏗️ Checkpoint Level 1

text

```
✅ Checklist sebelum lanjut ke Level 2:

PEMAHAMAN:
├── Bisa jelaskan alur request Laravel (public/index.php → response)
├── Bisa jelaskan mengapa named route lebih baik dari hardcode URL
├── Bisa jelaskan perbedaan {{ }} vs {!! !!} dan kapan pakai masing-masing
├── Bisa jelaskan perbedaan @yield vs @stack
└── Bisa jelaskan cara kerja @extends, @section, @push

PROYEK: Landing Page Perpustakaan
├── routes/web.php: route beranda, tentang, kontak (dengan nama)
├── layouts/app.blade.php: layout utama dengan navbar dan footer
├── beranda.blade.php: halaman utama
├── tentang.blade.php: halaman tentang
├── components/alert.blade.php: anonymous component
└── php artisan route:list: semua route terdaftar dengan nama yang benar

KEBIASAAN:
├── Selalu gunakan {{ }} bukan <?= ?> di Blade
├── Selalu gunakan route() helper, bukan hardcode URL
├── Gunakan php artisan make:* untuk buat file, bukan manual
└── php artisan route:list setelah tambah route baru

Git: feat: setup Laravel project with routing, Blade layouts, and components
```

---

## 🔵 LEVEL 2: CONTROLLER DAN REQUEST (Minggu 4-7)

> **Tema**: _"Memisahkan logika dari tampilan dengan controller yang terorganisir"_  
> **Benang Merah**: Route dengan closure (Level 1) → Controller memisahkan logika → Request object yang kaya → Thin controller dengan dependency injection  
> **Output**: Halaman katalog buku dengan controller, data hardcode, dan tampilan lengkap

---

### E. Controller — Logika Request yang Terorganisir

> 💡 **Benang Merah ke Level 1**: Di Level 1, kita pakai closure di route file. Closure bekerja untuk logika sederhana, tapi cepat berantakan untuk logika yang kompleks. Controller memindahkan logika ke class tersendiri yang lebih terorganisir dan testable.

text

```
Benang Merah Bagian E:
Closure di route: cepat tapi tidak scalable →
Controller: class yang mengelompokkan handler untuk satu resource →
Resource controller: 7 method standar untuk CRUD →
Dependency injection: Laravel otomatis inject apa yang dibutuhkan →
Thin controller: controller hanya koordinasi, bukan bisnis logic
```

#### [[13. Membuat dan Menggunakan Controller]]

Bash

```
# Buat resource controller
php artisan make:controller BukuController --resource
# Menghasilkan app/Http/Controllers/BukuController.php
# dengan method: index, create, store, show, edit, update, destroy

# Atau dengan model binding sekaligus:
php artisan make:controller BukuController --resource --model=Buku
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
    // Data sementara — diganti Eloquent di Level 3
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
            'judul'     => 'Laskar Pelangi',
            'pengarang' => 'Andrea Hirata',
            'tahun'     => 2005,
            'harga'     => 95000,
            'stok'      => 3,
            'kategori'  => 'Fiksi',
        ],
        [
            'id'        => 3,
            'judul'     => 'Cosmos',
            'pengarang' => 'Carl Sagan',
            'tahun'     => 1980,
            'harga'     => 200000,
            'stok'      => 0,
            'kategori'  => 'Sains',
        ],
    ];

    /**
     * GET /buku — Daftar semua buku
     */
    public function index(Request $request): View
    {
        // Filter via query string: /buku?kategori=Teknologi&q=clean
        $keyword   = $request->query('q', '');
        $kategori  = $request->query('kategori', 'semua');

        $buku = collect($this->katalog);

        if ($keyword) {
            $buku = $buku->filter(fn($b) =>
                str_contains(strtolower($b['judul']), strtolower($keyword)) ||
                str_contains(strtolower($b['pengarang']), strtolower($keyword))
            );
        }

        if ($kategori !== 'semua') {
            $buku = $buku->filter(fn($b) => $b['kategori'] === $kategori);
        }

        return view('buku.index', [
            'buku'     => $buku->values(),
            'keyword'  => $keyword,
            'kategori' => $kategori,
        ]);
    }

    /**
     * GET /buku/{id} — Detail satu buku
     */
    public function show(string $id): View
    {
        $buku = collect($this->katalog)->firstWhere('id', (int) $id);

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
        // Validasi dasar (akan diperdalam di Level 4)
        $validated = $request->validate([
            'judul'     => 'required|string|max:200',
            'pengarang' => 'required|string|max:100',
            'tahun'     => 'required|integer|min:1000|max:' . date('Y'),
            'harga'     => 'required|numeric|min:0',
            'stok'      => 'required|integer|min:0',
            'kategori'  => 'required|string',
        ]);

        // Nanti: simpan ke database dengan Eloquent
        return redirect()
            ->route('buku.index')
            ->with('sukses', "Buku '{$validated['judul']}' berhasil ditambahkan!");
    }
}
```

#### [[14. Dependency Injection dan Route Model Binding]]

PHP

```
<?php
// app/Http/Controllers/BukuController.php

class BukuController extends Controller
{
    // Constructor injection: inject service yang selalu dibutuhkan
    public function __construct(
        private BukuService $bukuService,
    ) {}

    // Method injection: Laravel otomatis inject berdasarkan type hint
    public function index(
        Request $request,           // inject HTTP request
        BukuFilter $filter,         // inject custom service
    ): View {
        $buku = $this->bukuService->getAll($filter->fromRequest($request));
        return view('buku.index', compact('buku'));
    }

    // Route Model Binding — Laravel otomatis fetch Buku dari database!
    // Route: Route::get('/buku/{buku}', ...)
    // Laravel: SELECT * FROM buku WHERE id = {buku}
    // Jika tidak ada: otomatis abort(404)
    public function show(Buku $buku): View   // perhatikan: type-hint Model, bukan int/string!
    {
        return view('buku.show', compact('buku'));
    }

    // Ubah kolom binding (default: id)
    // Route: Route::get('/buku/{buku:isbn}', ...)
    // Laravel: SELECT * FROM buku WHERE isbn = {buku}
    public function showByIsbn(Buku $buku): View
    {
        return view('buku.show', compact('buku'));
    }
}
```

#### [[15. Request Object — Data dari HTTP Request]]

PHP

```
<?php
class BukuController extends Controller
{
    public function index(Request $request): View
    {
        // ─── Query string (/buku?kategori=Teknologi&halaman=2) ────────────
        $kategori = $request->query('kategori');          // null jika tidak ada
        $halaman  = $request->query('halaman', 1);        // default: 1
        $semuaQuery = $request->query();                  // semua sebagai array

        // ─── Cek keberadaan dan nilai ─────────────────────────────────────
        if ($request->has('kategori')) {
            // parameter ada (boleh kosong)
        }
        if ($request->filled('kategori')) {
            // parameter ada DAN tidak kosong
        }
        if ($request->missing('kategori')) {
            // parameter tidak ada
        }

        // ─── Informasi request ────────────────────────────────────────────
        $url    = $request->url();           // http://localhost/buku
        $path   = $request->path();          // buku
        $method = $request->method();        // GET
        $ip     = $request->ip();

        // ─── Apakah request mengharapkan JSON? ───────────────────────────
        if ($request->expectsJson()) {
            return response()->json(['buku' => $buku]);
        }

        return view('buku.index', compact('buku', 'kategori'));
    }

    public function store(Request $request): RedirectResponse
    {
        // ─── Data dari form POST ──────────────────────────────────────────
        $judul     = $request->input('judul');
        $pengarang = $request->input('pengarang', 'Tidak Diketahui');

        // Ambil banyak field sekaligus
        $hanya   = $request->only(['judul', 'pengarang', 'tahun']);
        $kecuali = $request->except(['_token', '_method']);
        $semua   = $request->all();

        // ─── File upload ──────────────────────────────────────────────────
        if ($request->hasFile('sampul')) {
            $file = $request->file('sampul');
            // Detail di Level 6
        }

        // ─── Old input: nilai form saat validasi gagal ─────────────────
        // Di Blade: value="{{ old('judul') }}"
        // Otomatis tersimpan jika validasi gagal
    }
}
```

---

### 🏗️ Checkpoint Level 2

text

```
✅ Checklist sebelum lanjut ke Level 3:

PROYEK: Katalog Buku dengan Controller
├── BukuController: index, show, create (data array hardcode)
├── buku/index.blade.php: grid buku dengan filter kategori
├── buku/show.blade.php: detail buku
├── buku/create.blade.php: form tambah buku
├── Filter via query string berfungsi
└── Navigasi antar halaman dengan named route

PEMAHAMAN:
├── Bisa jelaskan perbedaan Route Model Binding vs manual findOrFail
├── Bisa jelaskan $request->has() vs $request->filled()
├── Bisa jelaskan dependency injection di constructor vs method
└── Bisa jelaskan kapan gunakan compact() vs array literal

Git: feat: implement BukuController with views and data passing
```

---

## 🟡 LEVEL 3: DATABASE DAN ELOQUENT ORM (Minggu 7-12)

> **Tema**: _"Dari data hardcode ke database MySQL yang persisten dan elegan"_  
> **Benang Merah**: Array hardcode (Level 2) → Migration blueprint tabel → Eloquent sebagai jembatan PHP-MySQL → Seeder & Factory → Query yang ekspresif  
> **Output**: Sistem perpustakaan dengan MySQL, Eloquent, paginasi, dan relasi antar tabel

---

### F. Migration — Version Control untuk Database

> 💡 **Mengapa Migration?** Di PHP native, kamu mungkin jalankan SQL manual di phpMyAdmin. Migration adalah solusi profesional: struktur database bisa di-commit ke git, dijalankan ulang oleh siapa saja, dan bisa di-rollback jika ada masalah.

text

```
Benang Merah Bagian F:
PHP native: SQL manual di phpMyAdmin atau file .sql →
Migration: blueprint PHP yang bisa di-commit, dijalankan, dan di-rollback →
Schema builder: cara elegan mendefinisikan tabel dalam PHP →
php artisan migrate: semua developer punya struktur database yang sama →
Seeder dan Factory: isi database dengan data awal dan data test
```

#### [[16. Membuat dan Menjalankan Migration]]

Bash

```
# Buat migration
php artisan make:migration create_buku_table
php artisan make:migration create_anggota_table
php artisan make:migration create_peminjaman_table
php artisan make:migration add_deskripsi_to_buku_table   # modifikasi tabel
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
            $table->id();                       // UNSIGNED BIGINT AUTO_INCREMENT PRIMARY KEY

            $table->string('judul', 200);
            $table->string('pengarang', 100);
            $table->char('isbn', 13)->unique()->nullable();
            $table->string('penerbit', 100)->nullable();

            $table->year('tahun');
            $table->decimal('harga', 12, 2)->default(0);
            $table->unsignedInteger('stok')->default(0);
            $table->string('kategori', 50)->default('Umum');

            $table->text('deskripsi')->nullable();
            $table->string('sampul')->nullable();

            // timestamps(): tambah created_at dan updated_at otomatis
            $table->timestamps();

            // softDeletes(): tambah deleted_at (data tidak benar-benar dihapus)
            $table->softDeletes();

            // Index untuk kolom yang sering dipakai di WHERE
            $table->index('pengarang');
            $table->index('kategori');
            $table->index(['tahun', 'kategori']);   // composite index
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('buku');
    }
};
```

PHP

```
<?php
// database/migrations/..._create_peminjaman_table.php

Schema::create('peminjaman', function (Blueprint $table) {
    $table->id();

    // Foreign key dengan fluent syntax:
    $table->foreignId('buku_id')
          ->constrained('buku')          // referensi tabel buku.id
          ->cascadeOnUpdate()             // jika id buku berubah, ikut berubah
          ->restrictOnDelete();           // tidak bisa hapus buku yang masih dipinjam

    $table->foreignId('anggota_id')
          ->constrained('anggota')
          ->cascadeOnUpdate()
          ->restrictOnDelete();

    $table->date('tanggal_pinjam');
    $table->date('batas_kembali');
    $table->date('tanggal_kembali')->nullable();
    $table->decimal('denda', 10, 2)->default(0);
    $table->enum('status', ['dipinjam', 'dikembalikan', 'terlambat'])->default('dipinjam');

    $table->timestamps();

    $table->index(['anggota_id', 'status']);
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

# Drop semua + migrate ulang (HANYA di development!)
php artisan migrate:fresh

# Drop semua + migrate + seed
php artisan migrate:fresh --seed
```

#### [[17. Seeder dan Factory — Data Awal dan Data Test]]

Bash

```
php artisan make:factory BukuFactory --model=Buku
php artisan make:seeder BukuSeeder
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
        return [
            'judul'     => fake()->sentence(3, true),
            'pengarang' => fake()->name(),
            'isbn'      => fake()->numerify('#############'),
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
        // Data buku nyata (data tetap)
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
                'deskripsi' => 'Panduan menulis kode yang bersih dan maintainable.',
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

        // Generate buku palsu menggunakan factory
        Buku::factory()->count(40)->create();
        Buku::factory()->count(5)->habis()->create();
        Buku::factory()->count(5)->teknologi()->create();
    }
}
```

PHP

```
<?php
// database/seeders/DatabaseSeeder.php

class DatabaseSeeder extends Seeder
{
    public function run(): void
    {
        // Urutan penting: parent dulu sebelum child (foreign key)
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
# Jalankan seeder
php artisan db:seed

# Seeder tertentu
php artisan db:seed --class=BukuSeeder

# Paling sering dipakai saat development:
php artisan migrate:fresh --seed
```

---

### G. Eloquent ORM — Model yang Ekspresif

> 💡 **Benang Merah ke Migration**: Migration membuat tabel. Eloquent Model adalah jembatan PHP ke tabel tersebut. Setiap Model merepresentasikan satu tabel dan memiliki semua method untuk query, insert, update, delete.

#### [[18. Eloquent Model — Dasar]]

Bash

```
php artisan make:model Buku -mfs
# Buat: Model + Migration + Factory + Seeder sekaligus
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
use Illuminate\Database\Eloquent\Relations\BelongsToMany;

class Buku extends Model
{
    use HasFactory, SoftDeletes;

    // Mass assignment protection — WAJIB!
    // Hanya field ini yang bisa di-set via create() atau fill()
    protected $fillable = [
        'judul', 'pengarang', 'isbn', 'penerbit',
        'tahun', 'harga', 'stok', 'kategori', 'deskripsi', 'sampul',
    ];

    // Cast: otomatis konversi tipe saat akses
    protected function casts(): array
    {
        return [
            'harga'      => 'decimal:2',
            'stok'       => 'integer',
            'tahun'      => 'integer',
            'deleted_at' => 'datetime',
        ];
    }

    // ─── Accessor — modifikasi nilai saat diakses ────────────────────────
    // Akses via: $buku->harga_format
    public function getHargaFormatAttribute(): string
    {
        return 'Rp ' . number_format($this->harga, 0, ',', '.');
    }

    // ─── Method bisnis ───────────────────────────────────────────────────
    public function tersedia(): bool
    {
        return $this->stok > 0;
    }

    // ─── Local Scope: query yang reusable ───────────────────────────────
    // Pakai: Buku::tersedia()->get()
    public function scopeTersedia(Builder $query): Builder
    {
        return $query->where('stok', '>', 0);
    }

    // Pakai: Buku::kategori('Teknologi')->get()
    public function scopeKategori(Builder $query, string $kategori): Builder
    {
        return $query->where('kategori', $kategori);
    }

    // Pakai: Buku::pencarian('clean')->get()
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

    public function peminjamanAktif(): HasMany
    {
        return $this->hasMany(Peminjaman::class)
                    ->where('status', 'dipinjam');
    }
}
```

#### [[19. Eloquent Query — Dari yang Sederhana ke yang Kompleks]]

PHP

```
<?php
// app/Http/Controllers/BukuController.php

use App\Models\Buku;

public function index(Request $request): View
{
    // ─── Query dengan kondisi ─────────────────────────────────────────────

    // Ambil semua (HINDARI di production jika data banyak!)
    $semua = Buku::all();

    // Dengan kondisi
    $teknologi = Buku::where('kategori', 'Teknologi')->get();

    // Chaining kondisi
    $hasilFilter = Buku::where('kategori', 'Teknologi')
                       ->where('harga', '>', 100000)
                       ->orderBy('harga', 'desc')
                       ->get();

    // ─── Menggunakan local scope ──────────────────────────────────────────
    $tersedia   = Buku::tersedia()->get();
    $teknologi  = Buku::kategori('Teknologi')->tersedia()->get();
    $pencarian  = Buku::pencarian('clean code')->get();

    // ─── Paginasi — SELALU gunakan ini, bukan all() ──────────────────────
    $buku = Buku::latest()          // order by created_at desc
                ->paginate(12);     // 12 per halaman

    // Dengan kondisi + paginate + pertahankan query string di pagination links
    $buku = Buku::tersedia()
                ->when($request->filled('kategori'), function (Builder $q) use ($request) {
                    $q->kategori($request->kategori);
                })
                ->when($request->filled('q'), function (Builder $q) use ($request) {
                    $q->pencarian($request->q);
                })
                ->orderBy('judul')
                ->paginate(12)
                ->withQueryString();    // pertahankan ?kategori=Teknologi di pagination links

    return view('buku.index', compact('buku'));

    // Di Blade: {{ $buku->links() }} menghasilkan link paginasi otomatis!

    // ─── Ambil satu record ────────────────────────────────────────────────
    $buku = Buku::find(1);           // null jika tidak ada
    $buku = Buku::findOrFail(1);     // abort(404) jika tidak ada — GUNAKAN INI
    $buku = Buku::first();
    $buku = Buku::firstOrFail();
    $buku = Buku::where('isbn', '9780132350884')->firstOrFail();

    // firstOrCreate: ambil atau buat baru
    $buku = Buku::firstOrCreate(
        ['isbn' => '9780132350884'],          // kondisi pencarian
        ['judul' => 'Clean Code', ...]         // data jika dibuat baru
    );

    // ─── Agregasi ────────────────────────────────────────────────────────
    $jumlah    = Buku::count();
    $tersedia  = Buku::tersedia()->count();
    $rataHarga = Buku::avg('harga');
    $maxHarga  = Buku::max('harga');
    $totalStok = Buku::sum('stok');

    // Exists check
    $sudahAda = Buku::where('isbn', '9780132350884')->exists();

    // ─── when() — conditional query ───────────────────────────────────────
    // Sangat berguna untuk filter dari request
    $buku = Buku::query()
        ->when($request->filled('q'),        fn($q) => $q->pencarian($request->q))
        ->when($request->filled('kategori'), fn($q) => $q->kategori($request->kategori))
        ->when($request->filter === 'tersedia', fn($q) => $q->tersedia())
        ->paginate(12);
}
```

#### [[20. Eloquent CRUD dan Relasi]]

PHP

```
<?php
class BukuController extends Controller
{
    public function store(Request $request): RedirectResponse
    {
        // create(): hanya field di $fillable yang disimpan
        $buku = Buku::create($request->validated());

        return redirect()
            ->route('buku.show', $buku)
            ->with('sukses', "Buku '{$buku->judul}' berhasil ditambahkan!");
    }

    public function update(Request $request, Buku $buku): RedirectResponse
    {
        // update(): hanya field di $fillable yang diperbarui
        $buku->update($request->validated());

        return redirect()
            ->route('buku.show', $buku)
            ->with('sukses', 'Buku berhasil diperbarui!');
    }

    public function destroy(Buku $buku): RedirectResponse
    {
        // Soft delete: isi deleted_at, buku masih di database
        $buku->delete();

        // Eloquent otomatis exclude soft-deleted dari query
        // Untuk include: Buku::withTrashed()->get()
        // Untuk restore: $buku->restore()
        // Untuk hard delete: $buku->forceDelete()

        return redirect()
            ->route('buku.index')
            ->with('sukses', "Buku '{$buku->judul}' berhasil dihapus.");
    }
}
```

PHP

```
<?php
// app/Models/Anggota.php — Relasi Eloquent

class Anggota extends Model
{
    // Satu anggota punya banyak peminjaman
    public function peminjaman(): HasMany
    {
        return $this->hasMany(Peminjaman::class);
    }

    // Peminjaman yang masih aktif
    public function peminjamanAktif(): HasMany
    {
        return $this->hasMany(Peminjaman::class)
                    ->where('status', 'dipinjam');
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

// ─── EAGER LOADING: solusi N+1 problem ────────────────────────────────────
// ❌ N+1 — 1 query + N query untuk setiap relasi
$peminjaman = Peminjaman::all();
foreach ($peminjaman as $p) {
    echo $p->buku->judul;     // query baru setiap iterasi!
}

// ✅ Eager loading — 1 query + 1 query untuk semua relasi
$peminjaman = Peminjaman::with(['buku', 'anggota'])->get();
// Hanya 3 query, berapapun jumlah peminjaman

// Eager loading dengan kondisi
$peminjaman = Peminjaman::with([
    'buku:id,judul,pengarang',     // hanya kolom tertentu
    'anggota' => fn($q) => $q->where('status', 'aktif'),
])->where('status', 'dipinjam')->get();

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
├── Migration: buku, anggota, peminjaman (dengan foreign key)
├── Model: Buku, Anggota, Peminjaman (dengan relasi dan scope)
├── Factory: data palsu yang realistis
├── Seeder: data buku nyata + data palsu
├── Controller: CRUD menggunakan Eloquent (bukan array)
├── Paginasi: {{ $buku->links() }} berfungsi
└── php artisan migrate:fresh --seed: berjalan tanpa error

QUERY YANG HARUS BISA DITULIS:
├── Buku::tersedia()->paginate(12)
├── Buku::with('peminjaman')->findOrFail($id)
├── Buku::when(...)->orderBy(...)->paginate()
├── Eager loading untuk menghindari N+1
└── Soft delete: delete(), restore(), onlyTrashed()

KEAMANAN:
├── $fillable terdefinisi di semua model (bukan $guarded = [])
├── findOrFail() dipakai, bukan find() + cek manual
└── Soft delete aktif untuk data penting

Git: feat: implement database migrations, Eloquent models, and CRUD
```

---

## 🟠 LEVEL 4: VALIDASI, AUTENTIKASI, DAN OTORISASI (Minggu 12-18)

> **Tema**: _"Dari sistem terbuka ke sistem yang aman dengan validasi dan kontrol akses"_  
> **Benang Merah**: CRUD tanpa keamanan (Level 3) → validasi input yang komprehensif → autentikasi via Breeze → otorisasi dengan Policy → middleware kustom  
> **Output**: Admin panel perpustakaan dengan login, role-based access, dan validasi lengkap

---

### H. Validasi — Input yang Benar dan Aman

> 💡 **Benang Merah ke Level 3**: Eloquent hanya menyimpan data yang ada di $fillable. Tapi siapa yang memastikan data itu valid? Validasi adalah tanggung jawab layer sebelum data menyentuh model.

text

```
Benang Merah Bagian H:
Eloquent menyimpan data (Level 3) →
Tapi data yang masuk harus valid dulu →
$request->validate(): validasi langsung di controller →
Form Request: pisahkan logika validasi ke class tersendiri →
Error ditampilkan otomatis di Blade →
Input dipertahankan saat validasi gagal (old())
```

#### [[21. Validasi di Controller dan Form Request]]

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

    // Jika gagal: otomatis redirect back dengan $errors dan old input
    // Jika berhasil: $validated berisi data yang sudah bersih dan valid

    $buku = Buku::create($validated);
    return redirect()->route('buku.show', $buku)->with('sukses', 'Buku ditambahkan!');
}
```

Bash

```
# Form Request: validasi yang lebih terorganisir (untuk form yang kompleks)
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
        return auth()->check() && auth()->user()->can('create', Buku::class);
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
            'isbn.digits'       => 'ISBN harus tepat 13 digit angka.',
            'isbn.unique'       => 'ISBN ini sudah terdaftar di sistem.',
            'sampul.max'        => 'Ukuran sampul maksimal 2MB.',
        ];
    }

    // Modifikasi input sebelum validasi
    protected function prepareForValidation(): void
    {
        $this->merge([
            'judul'     => trim($this->judul ?? ''),
            'pengarang' => trim($this->pengarang ?? ''),
            'isbn'      => preg_replace('/[^0-9]/', '', $this->isbn ?? ''),
        ]);
    }
}

// UpdateBukuRequest: sama tapi ignore unique untuk record saat ini
class UpdateBukuRequest extends FormRequest
{
    public function rules(): array
    {
        return [
            'isbn' => [
                'nullable',
                'digits:13',
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
    @csrf  {{-- WAJIB: Laravel otomatis verifikasi CSRF token --}}

    <div class="form-group">
        <label for="judul">Judul Buku *</label>
        <input
            type="text"
            id="judul"
            name="judul"
            value="{{ old('judul') }}"
            class="{{ $errors->has('judul') ? 'is-invalid' : '' }}"
        >
        @error('judul')
            <div class="invalid-feedback">{{ $message }}</div>
        @enderror
    </div>

    <div class="form-group">
        <label for="kategori">Kategori *</label>
        <select name="kategori" id="kategori">
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

    <button type="submit">Simpan Buku</button>
</form>
```

---

### I. Authentication — Sistem Login

> 💡 **Mengapa Breeze dulu sebelum manual?** Breeze menunjukkan bagaimana Laravel merekomendasikan implementasi auth. Setelah paham strukturnya, kamu bisa kustomisasi atau buat dari scratch.

#### [[22. Laravel Breeze — Scaffolding Auth yang Cepat]]

Bash

```
composer require laravel/breeze --dev
php artisan breeze:install

# Pilih stack:
# blade    → Blade + Tailwind CSS (rekomendasi untuk belajar)
# livewire → Livewire + Blade
# react    → Inertia + React
# vue      → Inertia + Vue
# api      → API only

php artisan breeze:install blade

npm install && npm run dev

php artisan migrate
```

text

```
Breeze membuat:
├── routes/auth.php                  → login, register, logout, password reset
├── app/Http/Controllers/Auth/       → LoginController, RegisterController, dll.
├── resources/views/auth/            → halaman login, register, dll.
└── app/Models/User.php              → model User sudah di-update

Fitur yang langsung tersedia:
├── /login             → form login
├── /register          → form registrasi
├── /forgot-password   → reset password via email
├── /email/verify      → verifikasi email
└── /dashboard         → halaman setelah login
```

#### [[23. Auth Manual — Memahami di Balik Layar]]

PHP

```
<?php
use Illuminate\Support\Facades\Auth;

// Login manual
public function login(Request $request): RedirectResponse
{
    $credentials = $request->validate([
        'email'    => ['required', 'email'],
        'password' => ['required'],
    ]);

    // Auth::attempt: cek credentials + buat session jika cocok
    if (Auth::attempt($credentials, $request->boolean('ingat_saya'))) {
        $request->session()->regenerate();  // cegah session fixation

        return redirect()
            ->intended(route('dashboard'))  // ke halaman yang dituju sebelum login
            ->with('sukses', 'Selamat datang kembali!');
    }

    return back()
        ->withErrors(['email' => 'Email atau password tidak cocok.'])
        ->onlyInput('email');
}

public function logout(Request $request): RedirectResponse
{
    Auth::logout();
    $request->session()->invalidate();
    $request->session()->regenerateToken();

    return redirect()->route('login');
}

// Akses user yang login:
Auth::user()      // model User atau null
Auth::id()        // ID user atau null
Auth::check()     // true jika sudah login
Auth::guest()     // true jika belum login

// Di controller:
$user = auth()->user();
$userId = auth()->id();
```

---

### J. Authorization — Siapa Boleh Apa

> 💡 **Auth vs Authorization**: Auth menjawab "siapa kamu?" (login). Authorization menjawab "apa yang boleh kamu lakukan?" (role & permission). Keduanya berbeda tapi sama-sama penting.

#### [[24. Policy — Otorisasi Berbasis Resource]]

Bash

```
php artisan make:policy BukuPolicy --model=Buku
```

PHP

```
<?php
// app/Policies/BukuPolicy.php

namespace App\Policies;

use App\Models\User;
use App\Models\Buku;

class BukuPolicy
{
    // before: dipanggil sebelum semua method
    // Jika return true, semua pengecekan di-bypass
    public function before(User $user, string $ability): bool|null
    {
        if ($user->role === 'super_admin') {
            return true;   // super admin boleh segalanya
        }
        return null;       // lanjut ke pengecekan di bawah
    }

    public function viewAny(?User $user): bool
    {
        return true;       // semua orang bisa lihat katalog, termasuk guest
    }

    public function view(?User $user, Buku $buku): bool
    {
        return true;       // semua orang bisa lihat detail buku
    }

    public function create(User $user): bool
    {
        return in_array($user->role, ['admin', 'pustakawan']);
    }

    public function update(User $user, Buku $buku): bool
    {
        return in_array($user->role, ['admin', 'pustakawan']);
    }

    public function delete(User $user, Buku $buku): bool
    {
        return $user->role === 'admin';   // hanya admin yang bisa hapus
    }

    public function restore(User $user, Buku $buku): bool
    {
        return $user->role === 'admin';
    }
}
```

PHP

```
<?php
// Di controller: gunakan $this->authorize()
class BukuController extends Controller
{
    public function create(): View
    {
        $this->authorize('create', Buku::class);
        return view('buku.create');
    }

    public function destroy(Buku $buku): RedirectResponse
    {
        $this->authorize('delete', $buku);
        $buku->delete();
        return redirect()->route('buku.index');
    }
}

// Di Blade: @can/@cannot
// @can('create', App\Models\Buku::class)
//     <a href="{{ route('buku.create') }}">Tambah Buku</a>
// @endcan

// Di middleware route:
Route::delete('/buku/{buku}', [BukuController::class, 'destroy'])
     ->middleware('can:delete,buku');
```

#### [[25. Middleware Kustom — Logic Sebelum Request Masuk Controller]]

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
        if (auth()->check() && auth()->user()->status !== 'aktif') {
            auth()->logout();

            return redirect()
                ->route('login')
                ->with('error', 'Akun Anda tidak aktif. Hubungi pustakawan.');
        }

        return $next($request);  // lanjut ke controller atau middleware berikutnya
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
        $middleware->append(\App\Http\Middleware\LogAktivitas::class);

        // Middleware alias (untuk pakai di route)
        $middleware->alias([
            'anggota.aktif' => \App\Http\Middleware\CekStatusAnggota::class,
            'role'          => \App\Http\Middleware\CekRole::class,
        ]);
    })
    ->create();

// Pakai di routes:
Route::middleware(['auth', 'anggota.aktif'])->group(function () {
    Route::resource('peminjaman', PeminjamanController::class);
});
```

---

### 🏗️ Checkpoint Level 4

text

```
✅ Checklist sebelum lanjut ke Level 5:

PROYEK: Admin Panel Perpustakaan
├── Auth: login, register, logout (via Breeze)
├── Middleware: CekStatusAnggota aktif di semua route protected
├── Policy: BukuPolicy (viewAny, view, create, update, delete)
├── Form Request: StoreBukuRequest, UpdateBukuRequest
├── CRUD buku: semua dengan validasi dan otorisasi
├── Role: admin CRUD, pustakawan tambah/edit, anggota hanya lihat
└── Blade: @can dipakai untuk conditional UI

KEAMANAN YANG HARUS BERJALAN:
├── @csrf di semua form POST
├── Validasi via Form Request sebelum data ke model
├── authorize() di setiap method yang butuh permission
├── findOrFail() selalu dipakai (bukan find())
└── old() di semua form input (pertahankan nilai saat error)

Git: feat: implement Breeze auth, Form Request validation, and Policy
```

---

## 🔴 LEVEL 5: REST API, QUEUE, DAN EVENT (Minggu 18-24)

> **Tema**: _"Dari web app ke API yang bisa dikonsumsi aplikasi lain dan proses asynchronous"_  
> **Benang Merah**: Web app (Level 4) → expose data via JSON API → auth API via Sanctum → queue untuk proses berat → event-driven untuk decoupling  
> **Output**: REST API perpustakaan dengan Sanctum, queue email, dan event system

---

### K. REST API dengan Laravel

> 💡 **Mengapa API terpisah dari web?** API memungkinkan frontend yang berbeda (React, Vue, mobile app) menggunakan backend yang sama. Laravel memiliki tool khusus untuk ini yang membuat proses sangat elegan.

text

```
Benang Merah Bagian K:
Web app mengembalikan HTML (Level 1-4) →
API mengembalikan JSON — frontend bebas pilih teknologi →
API Resource: transformasi Eloquent ke JSON yang konsisten →
Sanctum: token-based auth yang ringan untuk API →
Error handling: response JSON yang konsisten untuk semua error
```

#### [[26. API Resource — Transformasi Data yang Konsisten]]

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
            'id'           => $this->id,
            'judul'        => $this->judul,
            'pengarang'    => $this->pengarang,
            'isbn'         => $this->isbn,
            'tahun'        => $this->tahun,
            'harga'        => $this->harga,
            'harga_format' => $this->harga_format,    // accessor
            'stok'         => $this->stok,
            'tersedia'     => $this->tersedia(),
            'kategori'     => $this->kategori,

            // Conditional: hanya tampil jika relasi di-load
            // (cegah N+1 problem di API)
            'total_peminjaman' => $this->whenCounted('peminjaman'),
            'peminjaman' => PeminjamanResource::collection(
                $this->whenLoaded('peminjaman')
            ),

            // Conditional: hanya untuk admin
            'created_at' => $this->when(
                $request->user()?->isAdmin(),
                $this->created_at?->toDateTimeString()
            ),

            'links' => [
                'self'   => route('api.v1.buku.show', $this->id),
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
    // GET /api/v1/buku
    public function index(Request $request): AnonymousResourceCollection
    {
        $buku = Buku::query()
            ->when($request->filled('q'),        fn($q) => $q->pencarian($request->q))
            ->when($request->filled('kategori'), fn($q) => $q->kategori($request->kategori))
            ->when($request->filter === 'tersedia', fn($q) => $q->tersedia())
            ->withCount('peminjaman')
            ->orderBy($request->sort ?? 'judul', $request->direction ?? 'asc')
            ->paginate($request->per_page ?? 15);

        return BukuResource::collection($buku);
        // Otomatis include: data, links (prev/next), meta (total, per_page, dll.)
    }

    // GET /api/v1/buku/{buku}
    public function show(Buku $buku): BukuResource
    {
        $buku->loadCount('peminjaman');
        return new BukuResource($buku);
    }

    // POST /api/v1/buku
    public function store(StoreBukuRequest $request): JsonResponse
    {
        $buku = Buku::create($request->validated());

        return (new BukuResource($buku))
            ->response()
            ->setStatusCode(201)
            ->header('Location', route('api.v1.buku.show', $buku));
    }

    // PUT /api/v1/buku/{buku}
    public function update(UpdateBukuRequest $request, Buku $buku): BukuResource
    {
        $buku->update($request->validated());
        return new BukuResource($buku->fresh());
    }

    // DELETE /api/v1/buku/{buku}
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
    // Public routes
    Route::apiResource('buku', Api\V1\BukuController::class)
         ->only(['index', 'show']);

    // Auth routes
    Route::post('auth/login', [Api\V1\AuthController::class, 'login']);
    Route::post('auth/register', [Api\V1\AuthController::class, 'register']);

    // Protected routes
    Route::middleware('auth:sanctum')->group(function () {
        Route::post('auth/logout', [Api\V1\AuthController::class, 'logout']);
        Route::get('auth/me', [Api\V1\AuthController::class, 'me']);

        Route::apiResource('buku', Api\V1\BukuController::class)
             ->only(['store', 'update', 'destroy']);

        Route::apiResource('peminjaman', Api\V1\PeminjamanController::class);
    });
});
```

#### [[27. Laravel Sanctum — Token Auth untuk API]]

Bash

```
composer require laravel/sanctum
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
php artisan migrate
```

PHP

```
<?php
// app/Models/User.php — tambahkan HasApiTokens
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens, HasFactory, Notifiable;
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

        $user = User::where('email', $request->email)->first();

        if (!$user || !Hash::check($request->password, $user->password)) {
            return response()->json(['message' => 'Kredensial tidak valid.'], 401);
        }

        // Buat token dengan abilities berdasarkan role
        $token = $user->createToken(
            name: $request->device_name ?? 'api',
            abilities: $user->isAdmin() ? ['*'] : ['buku:read', 'peminjaman:create'],
            expiresAt: now()->addDays(30),
        );

        return response()->json([
            'token' => $token->plainTextToken,
            'user'  => new UserResource($user),
        ]);
    }

    public function logout(Request $request): JsonResponse
    {
        $request->user()->currentAccessToken()->delete();
        return response()->json(['message' => 'Logged out.']);
    }
}
```

#### [[28. Error Handling API yang Konsisten]]

PHP

```
<?php
// bootstrap/app.php — handle semua exception untuk API

->withExceptions(function (Exceptions $exceptions) {
    $exceptions->render(function (\Throwable $e, Request $request) {
        if ($request->expectsJson() || $request->is('api/*')) {
            return match (true) {
                $e instanceof \Illuminate\Validation\ValidationException =>
                    response()->json([
                        'message' => 'Data tidak valid.',
                        'errors'  => $e->errors(),
                    ], 422),

                $e instanceof \Illuminate\Auth\AuthenticationException =>
                    response()->json(['message' => 'Tidak terautentikasi.'], 401),

                $e instanceof \Illuminate\Auth\Access\AuthorizationException =>
                    response()->json(['message' => 'Akses ditolak.'], 403),

                $e instanceof \Illuminate\Database\Eloquent\ModelNotFoundException =>
                    response()->json(['message' => 'Data tidak ditemukan.'], 404),

                $e instanceof \Symfony\Component\HttpKernel\Exception\TooManyRequestsHttpException =>
                    response()->json(['message' => 'Terlalu banyak request.'], 429),

                app()->environment('production') =>
                    response()->json(['message' => 'Terjadi kesalahan pada server.'], 500),

                default => null,
            };
        }
    });
})
```

---

### L. Queue — Proses Asynchronous

> 💡 **Mengapa Queue?** Kirim email, resize gambar, generate PDF — ini semua lambat. Jika dijalankan saat request, user harus menunggu. Queue memindahkan proses ke background sehingga response ke user tetap instan.

#### [[29. Job Queue — Proses Berat di Background]]

Bash

```
php artisan queue:table
php artisan migrate
php artisan make:job KirimEmailKonfirmasiPeminjaman
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

    public int $tries   = 3;           // max percobaan jika gagal
    public int $timeout = 60;          // timeout dalam detik
    public array $backoff = [30, 60, 120];  // delay antar retry

    public function __construct(
        private Peminjaman $peminjaman,
    ) {}

    public function handle(): void
    {
        Mail::to($this->peminjaman->anggota->email)
            ->send(new KonfirmasiPeminjamanMail($this->peminjaman));
    }

    // Dipanggil jika semua percobaan gagal
    public function failed(\Throwable $exception): void
    {
        \Log::error('Gagal kirim email konfirmasi', [
            'peminjaman_id' => $this->peminjaman->id,
            'error'         => $exception->getMessage(),
        ]);
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

    // Dispatch ke queue — tidak menunggu email terkirim
    KirimEmailKonfirmasiPeminjaman::dispatch($peminjaman);

    // Dengan delay
    KirimEmailKonfirmasiPeminjaman::dispatch($peminjaman)
        ->delay(now()->addMinutes(5));

    // Ke queue tertentu
    KirimEmailKonfirmasiPeminjaman::dispatch($peminjaman)
        ->onQueue('email');

    return redirect()
        ->route('peminjaman.show', $peminjaman)
        ->with('sukses', 'Peminjaman berhasil! Konfirmasi email akan segera dikirim.');
}
```

Bash

```
# Jalankan queue worker
php artisan queue:work

# Dengan opsi:
php artisan queue:work --queue=email,default
php artisan queue:work --tries=3 --timeout=60

# Di production: gunakan Supervisor
# Konfigurasi di: /etc/supervisor/conf.d/laravel-worker.conf
```

---

### M. Event dan Listener — Decoupling Logika

#### [[30. Event System — Arsitektur yang Loosely Coupled]]

Bash

```
php artisan make:event BukuDipinjam
php artisan make:event BukuDikembalikan
php artisan make:listener KirimNotifikasiPeminjaman --event=BukuDipinjam
php artisan make:listener UpdateStatistikPerpustakaan --event=BukuDipinjam
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
        Mail::to($event->peminjaman->anggota->email)
            ->send(new KonfirmasiPeminjamanMail($event->peminjaman));
    }
}
```

PHP

```
<?php
// Daftarkan di bootstrap/app.php atau EventServiceProvider
// Di controller: fire event
public function store(Request $request): RedirectResponse
{
    $peminjaman = Peminjaman::create([...]);
    $peminjaman->buku()->decrement('stok');

    // Fire event — semua listener dipanggil
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

REST API:
├── GET /api/v1/buku: list dengan paginasi dan filter
├── GET /api/v1/buku/{id}: detail buku
├── POST /api/v1/buku (auth): tambah buku → 201
├── PUT /api/v1/buku/{id} (auth): update buku
├── DELETE /api/v1/buku/{id} (auth admin): hapus → 204
├── Error handling: JSON konsisten untuk semua error
└── Test dengan Postman: semua endpoint berfungsi

SANCTUM:
├── POST /api/v1/auth/login → dapat token
├── Request tanpa token → 401
├── Request token salah → 401
└── Token anggota biasa → 403 untuk route admin

QUEUE:
├── php artisan queue:work berjalan
├── Email konfirmasi dikirim via queue (tidak blocking request)
├── Failed job tercatat dan bisa di-retry
└── php artisan queue:failed: bisa lihat failed jobs

Git: feat: build REST API with Sanctum, Queue, and Event system
```

---

## ⚫ LEVEL 6: TESTING, SERVICE CONTAINER, DAN CACHING (Minggu 24-32)

> **Tema**: _"Dari kode yang bekerja ke kode yang bisa dipercaya dan performa tinggi"_  
> **Benang Merah**: Aplikasi lengkap (Level 5) → testing memastikan tetap bekerja saat ada perubahan → Service Container untuk arsitektur yang bersih → Caching untuk performa  
> **Output**: Test suite lengkap, arsitektur dengan DI yang proper, dan aplikasi yang cepat

---

### N. Testing — Kode yang Bisa Dipercaya

> 💡 **Mengapa testing di Level 6?** Kamu perlu memahami cara komponen bekerja sebelum bisa men-test-nya dengan benar. Di project nyata, tulis test bersamaan dengan fitur — bahkan lebih baik lagi: TDD (Test-Driven Development).

text

```
Benang Merah Bagian N:
Semua fitur sudah ada (Level 1-5) →
Test: otomatis verifikasi fitur masih bekerja setelah ada perubahan →
Feature test: simulasi HTTP request ke controller →
Unit test: test class/method secara terisolasi →
Mocking: isolasi dependency dalam test →
CI: test otomatis berjalan di setiap commit
```

#### [[31. Feature Test — Simulasi HTTP Request]]

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

use App\Models\Buku;
use App\Models\User;
use Illuminate\Foundation\Testing\RefreshDatabase;
use Tests\TestCase;

class BukuTest extends TestCase
{
    use RefreshDatabase; // Reset database sebelum setiap test

    // ─── Public routes ────────────────────────────────────────────────────

    public function test_halaman_katalog_bisa_diakses_publik(): void
    {
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

    public function test_halaman_detail_404_jika_buku_tidak_ada(): void
    {
        $response = $this->get(route('buku.show', 99999));
        $response->assertStatus(404);
    }

    // ─── Protected routes ─────────────────────────────────────────────────

    public function test_form_tambah_buku_redirect_jika_belum_login(): void
    {
        $response = $this->get(route('buku.create'));
        $response->assertRedirect(route('login'));
    }

    public function test_admin_bisa_tambah_buku(): void
    {
        $admin = User::factory()->create(['role' => 'admin']);

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
        $anggota = User::factory()->create(['role' => 'anggota']);

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

    public function test_validasi_judul_wajib(): void
    {
        $admin = User::factory()->create(['role' => 'admin']);

        $response = $this->actingAs($admin)
            ->post(route('buku.store'), ['judul' => '']);  // judul kosong

        $response->assertSessionHasErrors('judul');
        $this->assertDatabaseEmpty('buku');
    }

    public function test_soft_delete_buku(): void
    {
        $admin = User::factory()->create(['role' => 'admin']);
        $buku  = Buku::factory()->create();

        $this->actingAs($admin)
             ->delete(route('buku.destroy', $buku))
             ->assertRedirect(route('buku.index'));

        $this->assertSoftDeleted('buku', ['id' => $buku->id]);
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

    public function test_api_list_buku_dengan_paginasi(): void
    {
        Buku::factory()->count(20)->create();

        $response = $this->getJson('/api/v1/buku');

        $response->assertStatus(200)
                 ->assertJsonStructure([
                     'data' => [
                         '*' => ['id', 'judul', 'pengarang', 'harga', 'tersedia'],
                     ],
                     'links' => ['first', 'last', 'prev', 'next'],
                     'meta'  => ['current_page', 'total', 'per_page'],
                 ]);
    }

    public function test_api_login_mengembalikan_token(): void
    {
        $user = User::factory()->create([
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
    }

    public function test_api_tambah_buku_butuh_auth(): void
    {
        $response = $this->postJson('/api/v1/buku', ['judul' => 'Test']);
        $response->assertStatus(401);
    }

    public function test_api_tambah_buku_sukses(): void
    {
        $admin = User::factory()->create(['role' => 'admin']);
        $token = $admin->createToken('test')->plainTextToken;

        Queue::fake();  // mock queue

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

#### [[32. Unit Test dan Mocking]]

PHP

```
<?php
// tests/Unit/BukuTest.php

namespace Tests\Unit;

use App\Models\Buku;
use PHPUnit\Framework\TestCase;

class BukuTest extends TestCase
{
    // Unit test: tidak butuh database
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
    public function test_format_harga_benar(float $harga, string $expected): void
    {
        $buku = new Buku();
        $buku->harga = $harga;

        $this->assertSame($expected, $buku->harga_format);
    }

    public static function providerFormatHarga(): array
    {
        return [
            'nol'     => [0,        'Rp 0'],
            'seribu'  => [1000,     'Rp 1.000'],
            'standar' => [150000,   'Rp 150.000'],
        ];
    }
}
```

Bash

```
# Jalankan semua test
php artisan test

# Test tertentu
php artisan test tests/Feature/BukuTest.php
php artisan test --filter=test_admin_bisa_tambah_buku

# Dengan coverage
php artisan test --coverage
php artisan test --coverage --min=70    # fail jika coverage < 70%

# Parallel (lebih cepat)
php artisan test --parallel
```

---

### O. Service Container — Arsitektur yang Bersih

> 💡 **Benang Merah**: Testing membutuhkan kemampuan mengganti dependency (mocking). Service Container memungkinkan ini. Memahami Service Container membuka pintu ke arsitektur yang benar-benar maintainable.

#### [[33. Dependency Injection dan Service Container]]

PHP

```
<?php
// app/Repositories/BukuRepositoryInterface.php

namespace App\Repositories;

interface BukuRepositoryInterface
{
    public function findAll(int $page = 1, int $perPage = 15): \Illuminate\Contracts\Pagination\LengthAwarePaginator;
    public function findById(int $id): \App\Models\Buku;
    public function create(array $data): \App\Models\Buku;
    public function update(\App\Models\Buku $buku, array $data): \App\Models\Buku;
    public function delete(\App\Models\Buku $buku): bool;
}

// app/Repositories/EloquentBukuRepository.php
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

    // ... implementasi lainnya
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
            BukuService::class,
            fn($app) => new BukuService(
                $app->make(BukuRepositoryInterface::class),
            ),
        );
    }
}

// app/Http/Controllers/BukuController.php
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

### P. Caching — Aplikasi yang Lebih Cepat

#### [[34. Implementasi Caching yang Strategis]]

PHP

```
<?php
use Illuminate\Support\Facades\Cache;

// Di controller atau repository
public function index(Request $request): View
{
    $cacheKey = 'buku.index.' . md5(json_encode($request->query()));

    $buku = Cache::remember($cacheKey, now()->addMinutes(5), function () use ($request) {
        return Buku::query()
            ->when($request->filled('kategori'), fn($q) => $q->kategori($request->kategori))
            ->paginate(12);
    });

    return view('buku.index', compact('buku'));
}

// Invalidate cache saat data berubah
public function store(StoreBukuRequest $request): RedirectResponse
{
    $buku = Buku::create($request->validated());

    // Hapus semua cache buku
    Cache::tags(['buku'])->flush();    // butuh Redis/Memcached
    // atau:
    Cache::forget('buku.index.' . md5(''));

    return redirect()->route('buku.index');
}

// app/Models/Buku.php — auto-invalidate via Eloquent events
class Buku extends Model
{
    protected static function booted(): void
    {
        // Otomatis clear cache setiap kali buku disimpan/dihapus
        static::saved(fn() => Cache::tags(['buku'])->flush());
        static::deleted(fn() => Cache::tags(['buku'])->flush());
    }
}

// Atomic lock: cegah race condition saat banyak request bersamaan
public function pinjamBuku(int $bukuId, int $anggotaId): Peminjaman
{
    $lock = Cache::lock("pinjam.buku.{$bukuId}", 10);

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
        $lock->release();   // selalu release lock
    }
}
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
├── Mocking: Queue::fake(), Mail::fake(), Event::fake()
└── Coverage: minimal 70% untuk controllers dan models

SERVICE CONTAINER:
├── Interface: BukuRepositoryInterface
├── Implementasi: EloquentBukuRepository
├── Binding di AppServiceProvider
└── Controller inject interface (bukan class konkret)

CACHING:
├── Cache::remember() di endpoint yang sering diakses
├── Cache invalidation saat data berubah
├── Atomic lock untuk operasi kritis (pinjam buku)
└── Cache driver Redis di production (file untuk development)

KEBIASAAN:
├── php artisan test sebelum setiap commit
├── Refactor dengan percaya diri karena ada test
└── Tulis test setelah (atau sebelum) buat fitur baru

Git: feat: add test suite, Service Container DI, and Redis caching
```

---

## 🟣 LEVEL 7: ARSITEKTUR LANJUTAN DAN DEVOPS (Minggu 32+)

> **Tema**: _"Dari aplikasi yang bekerja ke sistem enterprise-grade yang siap production"_  
> **Benang Merah**: Aplikasi complete (Level 6) → arsitektur yang scalable → monitoring → CI/CD → ekosistem Laravel lanjutan  
> **Output**: Sistem perpustakaan production-ready dengan CI/CD, monitoring, dan ekosistem

---

### Q. Arsitektur Lanjutan

#### [[35. Action Pattern dan Service Layer]]

text

```
Benang Merah Bagian Q:
Controller + Repository (Level 6) →
Action Pattern: satu class, satu aksi bisnis →
Business logic yang terisolasi dan testable →
Controller menjadi sangat tipis →
Mudah di-test, mudah dipahami, mudah diubah
```

PHP

```
<?php
// app/Actions/PinjamBukuAction.php

namespace App\Actions;

use App\Events\BukuDipinjam;
use App\Exceptions\BukuTidakTersediaException;
use App\Exceptions\BatasPinjamanTercapaiException;
use App\Models\Buku;
use App\Models\Peminjaman;
use App\Models\User;
use Illuminate\Support\Facades\DB;

class PinjamBukuAction
{
    public function __construct(
        private BukuRepositoryInterface $bukuRepo,
    ) {}

    public function execute(User $anggota, Buku $buku): Peminjaman
    {
        // Validasi business rules
        if (!$buku->tersedia()) {
            throw new BukuTidakTersediaException("Buku '{$buku->judul}' tidak tersedia");
        }

        if ($anggota->peminjamanAktif()->count() >= 5) {
            throw new BatasPinjamanTercapaiException("Batas peminjaman sudah tercapai");
        }

        if ($anggota->memilikiDenda()) {
            throw new \DomainException("Selesaikan denda sebelum meminjam");
        }

        return DB::transaction(function () use ($anggota, $buku) {
            // Kurangi stok dengan locking
            $buku->decrement('stok');

            $peminjaman = Peminjaman::create([
                'buku_id'        => $buku->id,
                'anggota_id'     => $anggota->id,
                'tanggal_pinjam' => now()->toDateString(),
                'batas_kembali'  => now()->addDays(14)->toDateString(),
            ]);

            // Fire event — listeners handle notifikasi, statistik, dll.
            BukuDipinjam::dispatch($peminjaman);

            return $peminjaman;
        });
    }
}

// Controller menjadi sangat tipis:
class PeminjamanController extends Controller
{
    public function store(Request $request, PinjamBukuAction $action): RedirectResponse
    {
        try {
            $buku      = Buku::findOrFail($request->integer('buku_id'));
            $peminjaman = $action->execute(auth()->user(), $buku);

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

### R. DevOps dan Deployment

#### [[36. Deployment ke VPS dengan Nginx]]

Bash

```
# ─── Di SERVER (Ubuntu 22.04) ─────────────────────────────────────────────

# Install dependencies
sudo apt update && sudo apt upgrade -y
sudo apt install -y nginx mysql-server php8.3-fpm \
    php8.3-mysql php8.3-mbstring php8.3-xml php8.3-curl \
    php8.3-zip php8.3-redis redis-server supervisor

# Install Composer
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer

# Setup project
cd /var/www
sudo git clone https://github.com/username/perpustakaan.git
sudo chown -R www-data:www-data perpustakaan/
cd perpustakaan

# Install production dependencies
composer install --optimize-autoloader --no-dev

# Setup .env
cp .env.example .env
# Edit .env: APP_ENV=production, APP_DEBUG=false, DB_*, REDIS_*

php artisan key:generate

# Setup database
mysql -u root -p -e "CREATE DATABASE perpustakaan_db CHARACTER SET utf8mb4"
php artisan migrate --force
php artisan db:seed --force

# Optimize untuk production
php artisan optimize          # cache config, route, view
php artisan storage:link      # symlink storage/app/public → public/storage

# Build assets
npm install && npm run build

# Permissions
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

    server_tokens off;

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;

    # Laravel routing
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    # PHP-FPM
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
        fastcgi_hide_header X-Powered-By;
    }

    # Blokir akses ke file sensitif
    location ~ /\.(env|git) {
        deny all;
        return 404;
    }

    # Cache static assets
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff2)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

Bash

```
# SSL dengan Let's Encrypt
sudo certbot --nginx -d perpustakaan.kota.id
```

ini

```
# /etc/supervisor/conf.d/perpustakaan-worker.conf
[program:perpustakaan-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /var/www/perpustakaan/artisan queue:work redis --sleep=3 --tries=3 --max-time=3600
autostart=true
autorestart=true
user=www-data
numprocs=2
stdout_logfile=/var/www/perpustakaan/storage/logs/worker.log

[program:perpustakaan-scheduler]
command=php /var/www/perpustakaan/artisan schedule:work
autostart=true
autorestart=true
user=www-data
stdout_logfile=/var/www/perpustakaan/storage/logs/scheduler.log
```

#### [[37. CI/CD dengan GitHub Actions]]

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

    steps:
      - uses: actions/checkout@v4

      - name: Setup PHP
        uses: shivammathur/setup-php@v2
        with:
          php-version: '8.3'
          extensions: dom, curl, mbstring, zip, pdo, mysql, redis
          coverage: xdebug

      - name: Cache Composer
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

      - name: Run tests
        run: php artisan test --coverage --min=70 --parallel
        env:
          DB_CONNECTION: mysql
          DB_HOST: 127.0.0.1
          DB_DATABASE: perpustakaan_test
          DB_USERNAME: root
          DB_PASSWORD: password
          REDIS_HOST: 127.0.0.1

  deploy:
    name: Deploy to Production
    needs: test                              # hanya deploy jika test lulus
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'     # hanya dari branch main

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
            npm install && npm run build
            sudo systemctl reload nginx
```

---

### S. Ekosistem Laravel Lanjutan

#### [[38. Filament — Admin Panel yang Powerful]]

Bash

```
composer require filament/filament:"^3.0"
php artisan filament:install --panels
php artisan make:filament-resource Buku --generate
```

PHP

```
<?php
// app/Filament/Resources/BukuResource.php

use Filament\Forms;
use Filament\Tables;
use Filament\Resources\Resource;

class BukuResource extends Resource
{
    protected static ?string $model = Buku::class;
    protected static ?string $navigationIcon = 'heroicon-o-book-open';
    protected static ?string $navigationLabel = 'Koleksi Buku';

    public static function form(Forms\Form $form): Forms\Form
    {
        return $form->schema([
            Forms\Components\TextInput::make('judul')->required()->maxLength(200),
            Forms\Components\TextInput::make('pengarang')->required()->maxLength(100),
            Forms\Components\Select::make('kategori')
                ->options(['Teknologi' => 'Teknologi', 'Fiksi' => 'Fiksi'])
                ->required(),
            Forms\Components\TextInput::make('harga')->numeric()->prefix('Rp'),
            Forms\Components\TextInput::make('stok')->integer()->minValue(0),
            Forms\Components\Textarea::make('deskripsi')->columnSpanFull(),
            Forms\Components\FileUpload::make('sampul')->image()->imageResizeWidth(400),
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

#### [[39. Livewire — Reactive UI Tanpa Full SPA]]

Bash

```
composer require livewire/livewire
php artisan make:livewire KatalogBuku
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

    // Property reactive: perubahan otomatis trigger re-render
    public string $pencarian = '';
    public string $kategori  = '';

    public function updatingPencarian(): void
    {
        $this->resetPage();  // kembali ke halaman 1 saat cari
    }

    public function render()
    {
        $buku = Buku::query()
            ->when($this->pencarian, fn($q) => $q->pencarian($this->pencarian))
            ->when($this->kategori,  fn($q) => $q->kategori($this->kategori))
            ->paginate(12);

        return view('livewire.katalog-buku', compact('buku'));
    }
}
```

PHP

```
{{-- resources/views/livewire/katalog-buku.blade.php --}}
<div>
    {{-- wire:model.live: real-time sync dengan property Livewire --}}
    <input
        type="text"
        wire:model.live.debounce.300ms="pencarian"
        placeholder="Cari judul atau pengarang..."
    >

    <select wire:model.live="kategori">
        <option value="">Semua Kategori</option>
        <option value="Teknologi">Teknologi</option>
        <option value="Fiksi">Fiksi</option>
    </select>

    {{-- Loading indicator --}}
    <div wire:loading>Memuat...</div>

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

#### [[40. Task Scheduling — Otomasi Tugas Berkala]]

PHP

```
<?php
// routes/console.php — Laravel 11

use Illuminate\Support\Facades\Schedule;

// Kirim pengingat ke anggota yang hampir jatuh tempo
Schedule::command('perpustakaan:kirim-pengingat')
         ->dailyAt('09:00')
         ->emailOutputOnFailure(config('mail.admin'));

// Hitung denda otomatis setiap tengah malam
Schedule::command('perpustakaan:hitung-denda')
         ->daily()
         ->withoutOverlapping();  // jangan jalankan jika yang sebelumnya masih berjalan

// Backup database mingguan
Schedule::command('backup:run --only-db')
         ->weekly()
         ->at('02:00')
         ->onSuccess(fn() => \Log::info('Backup berhasil'))
         ->onFailure(fn() => \Log::error('Backup gagal!'));

// Cleanup log bulanan
Schedule::command('perpustakaan:cleanup-log')
         ->monthly();
```

Bash

```
# Buat Artisan command kustom
php artisan make:command KirimPengingatPeminjaman

# Di production: daftarkan satu cron job di server
# crontab -e
# * * * * * cd /var/www/perpustakaan && php artisan schedule:run >> /dev/null 2>&1

# Atau gunakan supervisor (sudah dikonfigurasi di poin 36)
# schedule:work otomatis jalankan scheduled tasks tanpa cron
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
└── Controller tipis: hanya delegasi ke Action

TESTING:
├── php artisan test: semua hijau
├── Coverage: >70% untuk kode yang critical
├── CI: GitHub Actions jalankan test setiap push
└── CD: auto-deploy ke VPS jika test lulus

DEVOPS:
├── Nginx: konfigurasi benar dengan security headers
├── SSL: HTTPS aktif dengan Let's Encrypt
├── Supervisor: queue worker dan scheduler berjalan
├── Redis: cache, session, dan queue
└── Monitoring: Laravel Telescope (dev) atau Pulse (prod)

EKOSISTEM:
├── Filament: admin panel untuk pustakawan
├── Livewire: katalog buku reactive tanpa Vue/React
└── Scheduler: pengingat dan hitung denda otomatis

KEAMANAN PRODUCTION:
├── APP_DEBUG=false
├── Error tidak bocor ke user
├── Rate limiting di semua endpoint API
├── HTTPS enforced
└── File .env tidak bisa diakses dari web

Git: feat: production-ready system with CI/CD, monitoring, and ecosystem
```

---

## 📊 Ringkasan Progress Tracking

### Satu Project, 7 Level Enhancement

text

```
Level 1: Landing page perpustakaan — routing, Blade, layout, komponen
  + Level 2: + Controller, Request object, data hardcode
  + Level 3: + Migration, Eloquent ORM, relasi, seeder, factory
  + Level 4: + Validasi, Auth Breeze, Authorization Policy, Middleware
  + Level 5: + REST API, Sanctum, Queue, Event-Listener
  + Level 6: + Testing, Service Container, Redis caching
  + Level 7: + Action pattern, CI/CD, Filament, Livewire, Scheduler
```

### Tabel Progress

|Level|Poin|Durasi|Output Konkret|
|---|---|---|---|
|🟢 **1**|1-12|Minggu 1-4|Landing page dengan routing, Blade, komponen|
|🔵 **2**|13-15|Minggu 4-7|Katalog buku dengan Controller dan Request|
|🟡 **3**|16-20|Minggu 7-12|Database MySQL, Eloquent, relasi, paginasi|
|🟠 **4**|21-25|Minggu 12-18|Auth Breeze, validasi, Policy, Middleware|
|🔴 **5**|26-30|Minggu 18-24|REST API, Sanctum, Queue, Event system|
|⚫ **6**|31-34|Minggu 24-32|Test suite, DI Container, Redis caching|
|🟣 **7**|35-40|Minggu 32+|Action pattern, DevOps, Filament, Livewire|

---

### Benang Merah Utama Sepanjang Roadmap

text

```
Poin 1  (Laravel vs PHP native)  → Fondasi: mengapa Laravel ada
Poin 2  (convention)             → Ikuti convention = kerja lebih sedikit
Poin 3  (instalasi)              → Project yang tumbuh dari Level 1-7
Poin 6  (Artisan)                → Gunakan ini untuk SEMUA pembuatan file
Poin 7  (named route)            → Selalu beri nama route — URL bisa berubah
Poin 10 ({{ }} vs {!! !!})       → XSS protection built-in di Blade
Poin 11 (layout Blade)           → Satu template induk untuk semua halaman
Poin 16 (migration)              → Database yang bisa di-version-control
Poin 18 (Eloquent $fillable)     → Mass assignment protection — wajib!
Poin 19 (eager loading with())   → Hindari N+1 — cek selalu sebelum deploy
Poin 20 (findOrFail)             → Biarkan Laravel handle 404 secara otomatis
Poin 21 (Form Request)           → Validasi terpisah, reusable, testable
Poin 22 (Breeze)                 → Auth yang benar — pelajari strukturnya
Poin 24 (Policy)                 → Otorisasi yang konsisten di seluruh app
Poin 26 (API Resource)           → JSON yang konsisten dan terkontrol
Poin 27 (Sanctum)                → Token auth yang ringan untuk API
Poin 29 (Queue)                  → Proses berat ke background — UX tetap cepat
Poin 31 (Feature test)           → Confidence untuk refactor tanpa takut rusak
Poin 33 (Service Container)      → Arsitektur yang testable dan maintainable
Poin 34 (Cache)                  → Database tidak di-hit setiap request
Poin 35 (Action pattern)         → Business logic terisolasi dan mudah ditest
Poin 37 (GitHub Actions)         → Tidak ada deploy manual — otomasi segalanya
Poin 38 (Filament)               → Admin panel dalam hitungan menit
Poin 39 (Livewire)               → Reactive UI tanpa Vue atau React
```

---

## 💡 Cara Menggunakan Roadmap Ini

text

```
Setiap poin mengikuti format:
┌──────────────────────────────────────────────────────────┐
│ 💡 Konteks: mengapa fitur Laravel ini ada                │
│ 🔗 Benang Merah: koneksi ke poin sebelum dan sesudahnya  │
│ 📋 Kode: implementasi di project perpustakaan            │
│          yang bisa langsung dicoba                       │
│ ✅ Langkah konkret: verifikasi berhasil                  │
└──────────────────────────────────────────────────────────┘
```

**Aturan yang Tidak Boleh Dilanggar:**

1. **`{{ }}` selalu, `{!! !!}` hanya jika benar-benar perlu** — XSS protection gratis dari Blade
2. **`findOrFail()` bukan `find()`** — biarkan Laravel handle 404, jangan cek manual
3. **`$fillable` wajib di setiap model** — jangan pernah `$guarded = []` di production
4. **Eager loading `with()`** — cek N+1 sebelum setiap deploy dengan Laravel Debugbar
5. **Named route selalu** — `route('buku.index')` bukan `'/buku'`
6. **`@csrf` di setiap form POST** — Laravel otomatis verifikasi
7. **Form Request untuk validasi** — jangan `$request->validate()` di controller untuk form kompleks
8. **`php artisan make:*`** untuk buat file — bukan buat manual
9. **`php artisan test`** sebelum setiap commit — tidak ada excuse
10. **`APP_DEBUG=false`** di production — error message tidak boleh bocor ke user

---

_Roadmap Laravel v1.0 — Step-by-Step, Security First, One Project_  
_Setiap baris kode ditulis dengan sadar — pahami mengapa sebelum bagaimana_