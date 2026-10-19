# Roadmap Vibes Coding: Step-by-Step Menjadi AI-Assisted Developer

## Filosofi Roadmap Ini

> **"Vibes coding bukan tentang membiarkan AI menulis semua kode tanpa pengawasan — vibes coding adalah tentang menjadi arsitek yang memahami sistem secara mendalam, lalu menggunakan AI sebagai multiplier untuk mengeksekusi visi kamu 10x lebih cepat"** — kamu tetap pilot, AI adalah co-pilot.

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: membangun aplikasi SaaS perpustakaan digital dari nol hingga production menggunakan AI-assisted workflow
- **Fondasi Dulu, AI Kemudian**: kamu tidak bisa "vibes" jika tidak paham apa yang sedang dibangun — AI memperkuat skill, bukan menggantikan
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **Kualitas di Atas Kecepatan**: AI bisa menghasilkan kode cepat, tapi _kamu_ yang bertanggung jawab atas kualitasnya
- **Mengapa sebelum Bagaimana**: pahami kapan AI membantu dan kapan AI menyesatkan

### Prasyarat Sebelum Memulai

text

```
Sebelum roadmap ini, pastikan sudah memahami:
├── Minimal satu bahasa pemrograman (JavaScript, Python, PHP, dll.)
├── Dasar-dasar web development (HTML, CSS, HTTP)
├── Git version control dasar
├── Command line / terminal dasar
├── Cara membaca dan memahami kode (tidak harus menulis dari nol)
└── Akun AI coding assistant (Cursor, GitHub Copilot, atau Claude)
```

### Apa Itu Vibes Coding?

text

```
Vibes Coding (Andrej Karpathy, 2025):
"Gaya pemrograman di mana kamu sepenuhnya menyerah pada vibes,
merangkul eksponensial, dan melupakan bahwa kode itu bahkan ada."

REALITAS yang lebih akurat:
├── Kamu mendeskripsikan INTENSI dengan jelas
├── AI menghasilkan IMPLEMENTASI
├── Kamu memverifikasi HASIL
├── Kamu mengoreksi ARAH
└── Siklus ini berulang hingga selesai

Vibes coding BUKAN:
❌ "Tulis prompt, terima kode, deploy tanpa baca"
❌ Mengandalkan AI untuk hal yang tidak kamu pahami
❌ Mengabaikan testing dan review

Vibes coding ADALAH:
✅ Memahami arsitektur, membiarkan AI handle boilerplate
✅ Menulis prompt yang spesifik dan kontekstual
✅ Membaca output AI dengan kritis
✅ Iterasi cepat: prompt → review → refine → commit
✅ Fokus pada "what" dan "why", delegasi "how" ke AI
```

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: "AI Pertama" — setup tools, prompt dasar, generate kode sederhana
    ↓ (enhance)
Level 2: + Context Management → AI yang paham project kamu
    ↓ (enhance)
Level 3: + Multi-File Workflow → Refactor dan fitur lintas file
    ↓ (enhance)
Level 4: + Debugging dengan AI → Fix bug, baca error, trace masalah
    ↓ (enhance)
Level 5: + Arsitektur dan Planning → AI sebagai thinking partner
    ↓ (enhance)
Level 6: + Testing dan Quality → AI-generated test suite
    ↓ (enhance)
Level 7: + Full-Stack Production → Deploy SaaS dengan AI-assisted workflow
```

---

## 🟢 LEVEL 1: FONDASI VIBES CODING (Minggu 1-2)

> **Tema**: _"Dari mengetik setiap baris ke mendeskripsikan setiap fitur"_  
> **Benang Merah**: Mengapa AI coding → Setup tools → Prompt dasar → Iterasi pertama → Kapan AI membantu vs menyesatkan  
> **Output**: Halaman web perpustakaan yang dibangun 80% oleh AI, 20% oleh kamu (arah dan review)

---

### A. Memahami AI-Assisted Development

> 💡 **Mengapa dimulai di sini?** Sebelum membuka Cursor atau Copilot, pahami dulu _model mental_ yang benar tentang AI coding. Developer yang salah paham akan menghasilkan kode yang salah tanpa menyadarinya.

text

```
Benang Merah Bagian A:
Kamu sudah bisa coding (prasyarat) →
AI coding: bukan autocomplete biasa, tapi reasoning engine →
LLM memahami konteks, bukan hanya syntax →
Model mental: kamu = arsitek, AI = kontraktor →
Setup tools → prompt pertama → lihat hasilnya
```

#### [[1. Model Mental yang Benar tentang AI Coding]]

text

```
┌─────────────────────────────────────────────────────────┐
│           HIERARKI KOMPETENSI VIBES CODING              │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  Level 5: ARSITEK (kamu di sini seharusnya)             │
│  ├── Memahami sistem secara holistik                    │
│  ├── Mendesain arsitektur dan data model                │
│  ├── Menentukan teknologi dan trade-off                 │
│  └── AI handle: implementasi detail, boilerplate        │
│                                                         │
│  Level 4: REVIEWER                                      │
│  ├── Bisa membaca dan memahami kode yang dihasilkan AI  │
│  ├── Bisa spot bug, security issue, dan bad pattern     │
│  └── AI handle: penulisan kode, testing                 │
│                                                         │
│  Level 3: DIREKTUR                                      │
│  ├── Bisa mendeskripsikan fitur dengan spesifik         │
│  ├── Bisa iterasi prompt hingga hasil benar             │
│  └── AI handle: coding, debugging                       │
│                                                         │
│  Level 2: OPERATOR                                      │
│  ├── Bisa pakai AI tools (Cursor, Copilot)              │
│  ├── Bisa accept/reject suggestion                      │
│  └── AI handle: hampir semua coding                     │
│                                                         │
│  Level 1: PENONTON (BAHAYA!)                            │
│  ├── Copy-paste dari ChatGPT tanpa baca                 │
│  ├── Tidak paham apa yang kode lakukan                  │
│  └── Hasil: kode yang "jalan" tapi tidak maintainable   │
│                                                         │
└─────────────────────────────────────────────────────────┘

ATURAN EMAS:
"Kamu tidak boleh membiarkan AI menulis kode yang tidak bisa
kamu jelaskan kepada developer junior dalam 5 menit."

Jika kamu tidak bisa menjelaskan KENAPA kode itu benar,
kamu belum siap membiarkan AI menulisnya.
```

#### [[2. Kapan AI Membantu vs Menyesatkan]]

text

```
✅ AI SANGAT BAGUS untuk:
├── Boilerplate code (CRUD, form, config)
├── Regex dan string manipulation yang kompleks
├── Unit test dan test case generation
├── Refactoring kode yang sudah ada
├── Dokumentasi dan komentar
├── Konversi antar bahasa/framework
├── Explaining error messages
├── SQL query generation dari deskripsi natural
├── CSS/Tailwind styling dari deskripsi visual
├── Git commit messages
└── Menulis README dan changelog

⚠️ AI CUKUP BAGUS tapi perlu review ketat:
├── Business logic yang kompleks
├── Database schema design
├── API endpoint implementation
├── Authentication dan authorization
├── State management di frontend
└── Integration antar service

❌ AI SERING SALAH untuk:
├── Arsitektur sistem berskala besar
├── Performance optimization yang spesifik
├── Security-critical code (crypto, auth tokens)
├── Kode yang bergantung pada state runtime yang kompleks
├── Framework/library versi terbaru (training data mungkin outdated)
├── Edge cases yang sangat spesifik domain kamu
└── Kode yang harus comply dengan regulasi tertentu
```

---

### B. Setup Tools AI Coding

> 💡 **Benang Merah ke A**: Paham model mental yang benar. Sekarang setup tools yang akan menjadi "co-pilot" kamu setiap hari.

text

```
Benang Merah Bagian B:
Memahami peran AI (A) →
Pilih AI coding assistant yang tepat →
Setup IDE dengan AI integration →
Konfigurasi context dan rules →
Prompt pertama → lihat perbedaan produktivitas
```

#### [[3. Memilih dan Setup AI Coding Tools]]

text

```
┌─────────────────────────────────────────────────────────────┐
│              PERBANDINGAN AI CODING TOOLS                    │
├──────────────┬──────────────┬───────────────┬───────────────┤
│ Tool         │ Terbaik Untuk│ Harga         │ Kekuatan      │
├──────────────┼──────────────┼───────────────┼───────────────┤
│ Cursor       │ Full project │ $20/bulan     │ Multi-file    │
│ (REKOMENDASI)│ context      │ (Pro)         │ editing,      │
│              │              │               │ codebase-aware│
├──────────────┼──────────────┼───────────────┼───────────────┤
│ GitHub       │ Inline       │ $10/bulan     │ Terintegrasi  │
│ Copilot      │ completion   │ (Individual)  │ dengan GitHub │
├──────────────┼──────────────┼───────────────┼───────────────┤
│ Claude       │ Reasoning    │ $20/bulan     │ Deep thinking │
│ (Artifacts/  │ dan planning │ (Pro)         │ arsitektur,   │
│  Projects)   │              │               │ long context  │
├──────────────┼──────────────┼───────────────┼───────────────┤
│ Windsurf     │ Agentic      │ $15/bulan     │ Auto-run      │
│              │ workflow     │ (Pro)         │ commands      │
├──────────────┼──────────────┼───────────────┼───────────────┤
│ Aider        │ Terminal     │ Gratis (API   │ Git-aware,    │
│              │ based        │ key sendiri)  │ CLI workflow  │
├──────────────┼──────────────┼───────────────┼───────────────┤
│ Continue.dev │ Open source  │ Gratis (API   │ Customizable, │
│              │              │ key sendiri)  │ local models  │
└──────────────┴──────────────┴───────────────┴───────────────┘

REKOMENDASI UNTUK PEMULA:
1. Cursor (utama) — paling powerful untuk full project context
2. Claude (pelengkap) — untuk planning dan reasoning mendalam
3. GitHub Copilot (opsional) — untuk inline completion saat coding manual
```

Bash

```
# Setup Cursor (REKOMENDASI UTAMA)
# 1. Download dari cursor.com
# 2. Install dan buka
# 3. Import settings dari VS Code (jika sudah pakai VS Code)
# 4. Login ke akun Cursor
# 5. Buka project perpustakaan:
cursor perpustakaan-digital/

# Setup GitHub Copilot (opsional)
# 1. Install extension "GitHub Copilot" di VS Code/Cursor
# 2. Login ke GitHub
# 3. Aktifkan di settings

# Setup Aider (untuk terminal lovers)
pip install aider-chat
export ANTHROPIC_API_KEY="sk-ant-..."
aider --model claude-3.5-sonnet
```

#### [[4. Konfigurasi Project untuk AI — Rules dan Context]]

text

```
File konfigurasi yang membuat AI lebih pintar tentang project kamu:
```

Markdown

```
<!-- .cursorrules — instruksi untuk Cursor AI -->
<!-- Taruh di root project -->

# Project: Perpustakaan Digital

## Tech Stack
- Backend: Laravel 11 (PHP 8.3)
- Frontend: Blade + Livewire + Tailwind CSS
- Database: MySQL 8
- Queue: Redis
- Testing: PHPUnit + Pest

## Coding Standards
- Gunakan type hints PHP 8 di semua function
- Selalu gunakan Form Request untuk validasi, bukan $request->validate() di controller
- Controller harus tipis — delegasi business logic ke Action class
- Gunakan Eloquent scope untuk query yang reusable
- Semua response API menggunakan ApiResource
- Named route selalu, jangan hardcode URL

## Security Rules
- Semua output Blade menggunakan {{ }} bukan {!! !!}
- Semua query menggunakan prepared statement (Eloquent otomatis)
- CSRF token di semua form POST
- Policy untuk authorization, bukan manual check di controller
- $fillable wajib di semua model

## File Structure
- Actions: app/Actions/
- Repositories: app/Repositories/
- Resources: app/Http/Resources/
- Requests: app/Http/Requests/
- Policies: app/Policies/

## Testing
- Feature test untuk semua endpoint
- Unit test untuk Action dan Service class
- Gunakan RefreshDatabase trait
- Factory untuk data test

## Language
- Komentar dan dokumentasi dalam bahasa Indonesia
- Variable dan function name dalam bahasa Inggris
- Commit message dalam bahasa Inggris
```

Markdown

```
<!-- CLAUDE.md — instruksi untuk Claude -->
<!-- Taruh di root project jika menggunakan Claude Projects -->

# Perpustakaan Digital

Sistem manajemen perpustakaan berbasis web dengan fitur:
- Katalog buku dengan pencarian dan filter
- Sistem peminjaman dan pengembalian
- Manajemen anggota dengan role (admin, pustakawan, anggota)
- Notifikasi email untuk jatuh tempo
- Dashboard statistik

## Aturan
1. Jangan pernah generate kode yang menghapus data tanpa konfirmasi
2. Selalu include error handling
3. Gunakan bahasa Indonesia untuk user-facing text
4. Prioritaskan keamanan di atas kenyamanan
```

---

### C. Prompt Engineering untuk Coding

> 💡 **Benang Merah ke B**: Tools sudah siap. Sekarang skill paling penting dalam vibes coding: **menulis prompt yang menghasilkan kode yang benar di percobaan pertama (atau kedua, bukan kesepuluh)**.

text

```
Benang Merah Bagian C:
Tools ter-setup (B) →
Prompt = instruksi ke AI →
Prompt buruk = kode buruk = waktu terbuang →
Prompt baik = kode benar = 10x lebih cepat →
Framework prompt: konteks + instruksi + constraint + format →
Iterasi: jarang benar di percobaan pertama, dan itu OK
```

#### [[5. Framework Prompt untuk Coding]]

text

```
┌─────────────────────────────────────────────────────────┐
│         ANATOMI PROMPT CODING YANG EFEKTIF              │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  1. KONTEKS (siapa, apa, di mana)                       │
│     "Saya sedang membangun REST API perpustakaan        │
│      menggunakan Laravel 11 dengan Sanctum auth."       │
│                                                         │
│  2. INSTRUKSI (apa yang harus dilakukan)                │
│     "Buatkan Form Request class untuk validasi           │
│      data buku saat create dan update."                 │
│                                                         │
│  3. CONSTRAINT (batasan dan aturan)                     │
│     "- ISBN harus 13 digit dan unique                   │
│      - Harga minimal 0                                  │
│      - Kategori harus dari enum yang sudah ada          │
│      - Gunakan Rule::unique()->ignore() untuk update"   │
│                                                         │
│  4. FORMAT (bagaimana output yang diinginkan)            │
│     "Buat dua file terpisah: StoreBukuRequest dan       │
│      UpdateBukuRequest. Include custom messages."       │
│                                                         │
│  5. CONTOH (opsional tapi sangat membantu)              │
│     "Contoh field: judul, pengarang, isbn, tahun,       │
│      harga, stok, kategori, deskripsi, sampul"          │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

#### [[6. Contoh Prompt: Buruk vs Baik]]

text

```
❌ PROMPT BURUK (terlalu vague):
"Buatkan CRUD buku"

Hasil: AI akan generate kode generik yang mungkin tidak cocok
dengan stack kamu, tanpa validasi, tanpa auth, tanpa error handling.
Kamu akan spend lebih banyak waktu memperbaiki daripada menulis sendiri.

────────────────────────────────────────────────────────────

✅ PROMPT BAIK (spesifik dan kontekstual):
"Saya punya model Buku di Laravel 11 dengan field:
judul (string), pengarang (string), isbn (char 13, unique, nullable),
tahun (integer), harga (decimal), stok (integer), kategori (string),
deskripsi (text, nullable), sampul (string, nullable).
Model sudah pakai SoftDeletes dan HasFactory.

Buatkan:
1. Resource controller BukuController dengan 7 method standar
2. Gunakan Route Model Binding
3. Validasi menggunakan Form Request terpisah (Store dan Update)
4. ISBN harus unique tapi ignore current record saat update
5. Kategori harus salah satu dari: Teknologi, Fiksi, Sains, Sejarah, Umum
6. Return redirect dengan flash message untuk web routes
7. Gunakan $request->validated() untuk create dan update

Jangan buat migration dan model, saya sudah punya."

Hasil: Kode yang langsung bisa dipakai, sesuai konvensi project kamu,
dengan validasi yang benar. Mungkin perlu sedikit tweak, tapi 90% ready.
```

#### [[7. Teknik Prompt Lanjutan]]

text

```
TEKNIK 1: Chain of Thought — minta AI berpikir step by step
─────────────────────────────────────────────────────────────
"Sebelum menulis kode, jelaskan dulu langkah-langkah yang akan
kamu ambil untuk mengimplementasikan sistem peminjaman buku.
Pertimbangkan edge cases seperti: stok habis, batas peminjaman
tercapai, anggota memiliki denda. Setelah itu, baru tulis kodenya."

TEKNIK 2: Role Playing — beri AI persona spesifik
─────────────────────────────────────────────────────────────
"Bertindaklah sebagai senior Laravel developer dengan 10 tahun
pengalaman. Review kode controller berikut dan identifikasi:
1. Potensi security vulnerability
2. N+1 query problem
3. Pelanggaran SOLID principles
4. Cara membuat kode lebih testable"

TEKNIK 3: Few-Shot — beri contoh output yang diinginkan
─────────────────────────────────────────────────────────────
"Buatkan API Resource untuk model Peminjaman.
Format output yang saya inginkan seperti ini:
{
  'data': {
    'id': 1,
    'buku': {'id': 5, 'judul': 'Clean Code'},
    'anggota': {'id': 12, 'nama': 'Budi'},
    'tanggal_pinjam': '2024-01-15',
    'batas_kembali': '2024-01-29',
    'status': 'dipinjam',
    'terlambat': false
  }
}
Buatkan PeminjamanResource yang menghasilkan format di atas."

TEKNIK 4: Negative Prompt — jelaskan apa yang TIDAK diinginkan
─────────────────────────────────────────────────────────────
"Buatkan middleware untuk rate limiting API.
JANGAN gunakan package tambahan.
JANGAN gunakan Redis — pakai database saja.
JANGAN buat terlalu kompleks — cukup per-IP, per-minute.
JANGAN include caching — saya akan tambahkan nanti."

TEKNIK 5: Iterative Refinement — perbaiki bertahap
─────────────────────────────────────────────────────────────
Prompt 1: "Buatkan controller untuk CRUD buku"
Prompt 2: "Tambahkan validasi menggunakan Form Request"
Prompt 3: "Tambahkan authorization menggunakan Policy"
Prompt 4: "Tambahkan eager loading untuk relasi peminjaman"
Prompt 5: "Refactor store method menggunakan Action pattern"
```

---

### 🏗️ Checkpoint Level 1

text

```
✅ Checklist sebelum lanjut ke Level 2:

SETUP:
├── Cursor terinstall dan terkonfigurasi
├── .cursorrules sudah dibuat dengan tech stack project
├── Project perpustakaan sudah di-init (Laravel/Next.js/dll)
└── AI sudah bisa "melihat" file-file project kamu

PROMPT SKILL:
├── Bisa tulis prompt dengan konteks + instruksi + constraint
├── Bisa bedakan prompt buruk vs prompt baik
├── Bisa iterasi prompt hingga hasil memuaskan
└── Bisa gunakan teknik chain-of-thought untuk masalah kompleks

PROYEK:
├── Generate landing page perpustakaan via AI prompt
├── Generate model dan migration via AI prompt
├── Review kode yang dihasilkan AI — identifikasi 3 hal yang perlu diperbaiki
└── Commit: "feat: initial project setup with AI-assisted scaffolding"

MINDSET:
├── Selalu baca kode yang dihasilkan AI sebelum accept
├── Selalu test kode yang dihasilkan AI sebelum commit
└── Jangan pernah deploy kode yang tidak kamu pahami

Git: feat: setup AI coding tools and generate initial scaffolding
```

---

## 🔵 LEVEL 2: CONTEXT MANAGEMENT (Minggu 2-4)

> **Tema**: _"Membuat AI benar-benar memahami project kamu, bukan hanya satu file"_  
> **Benang Merah**: Prompt per-file (Level 1) → AI butuh konteks project → @-reference files → codebase indexing → AI yang "tahu" arsitektur kamu  
> **Output**: AI yang bisa generate kode yang konsisten dengan seluruh codebase

---

### D. Memberikan Konteks yang Tepat ke AI

> 💡 **Mengapa ini game-changer?** AI yang tidak punya konteks akan menghasilkan kode yang "benar secara teknis" tapi "salah secara arsitektural". Contoh: AI generate query SQL mentah padahal project kamu pakai Eloquent. Konteks adalah perbedaan antara AI yang membantu dan AI yang membuat mess.

text

```
Benang Merah Bagian D:
Prompt per-file: AI hanya lihat satu file (Level 1) →
Realitas: kode saling terhubung antar file →
AI perlu lihat model, controller, route, dan view sekaligus →
Context management: cara "memberi makan" AI dengan info yang tepat →
Hasil: kode yang konsisten dengan seluruh project
```

#### [[8. Teknik Context Management di Cursor]]

text

```
Di Cursor, kamu bisa reference file, folder, dan simbol ke AI:

┌─────────────────────────────────────────────────────────┐
│         CARA REFERENCE KONTEKS DI CURSOR                │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  @filename     → Reference satu file                    │
│  @folder/      → Reference seluruh folder               │
│  @symbol       → Reference function/class tertentu      │
│  @codebase     → Search seluruh codebase                │
│  @web          → Reference dokumentasi online           │
│  @docs         → Reference docs yang sudah di-index     │
│                                                         │
│  Contoh prompt dengan context:                          │
│  "Lihat @app/Models/Buku.php dan                        │
│   @app/Http/Controllers/BukuController.php.             │
│   Tambahkan method scope untuk filter berdasarkan       │
│   status peminjaman di model, lalu gunakan di           │
│   controller index method."                             │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

text

```
PRAKTIK TERBAIK: Context yang harus selalu disertakan

Saat minta AI buat controller baru:
  @app/Models/Buku.php          ← AI tahu field dan relasi
  @app/Http/Requests/            ← AI tahu pola validasi kamu
  @routes/web.php                ← AI tahu naming convention route
  @.cursorrules                  ← AI tahu coding standard

Saat minta AI buat migration:
  @database/migrations/          ← AI tahu struktur tabel yang sudah ada
  @app/Models/                   ← AI tahu relasi antar model

Saat minta AI fix bug:
  @file-yang-error.php           ← File yang bermasalah
  @error-log.txt                 ← Pesan error lengkap
  @file-terkait.php              ← File yang berhubungan

Saat minta AI refactor:
  @file-lama.php                 ← Kode yang mau di-refactor
  @test-file.php                 ← Test yang harus tetap lulus
  @.cursorrules                  ← Standard yang harus diikuti
```

#### [[9. Membangun "Memory" AI untuk Project Kamu]]

Markdown

```
<!-- .cursor/rules/laravel.mdc — Rules khusus Laravel -->

# Laravel Conventions

## Controller Pattern
- Gunakan constructor injection untuk repository/service
- Method harus return View atau RedirectResponse (typed return)
- Jangan query database langsung di controller — gunakan repository

## Model Pattern
- $fillable wajib, jangan $guarded = []
- Gunakan local scope untuk query yang berulang
- Relasi harus typed return (HasMany, BelongsTo, dll)
- Accessor menggunakan get{Attribute}Attribute pattern

## Route Pattern
- Selalu named route
- Resource route untuk CRUD
- Group dengan prefix dan middleware
- API routes di routes/api.php dengan prefix v1

## Validation Pattern
- Form Request class untuk semua validasi
- authorize() method untuk permission check
- prepareForValidation() untuk sanitasi input
- Custom messages untuk user-facing error

## Error Handling
- Gunakan findOrFail() bukan find() + manual check
- Policy untuk authorization, bukan if-else di controller
- Custom exception class untuk domain errors
```

Markdown

```
<!-- .cursor/rules/database.mdc — Rules khusus database -->

# Database Conventions

## Migration
- Gunakan foreignId()->constrained() untuk foreign key
- Selalu include timestamps()
- Gunakan softDeletes() untuk data yang tidak boleh hilang
- Tambah index untuk kolom yang sering di-WHERE

## Query
- Selalu eager load relasi yang akan diakses (with())
- Gunakan scope untuk query yang kompleks
- Jangan gunakan raw query kecuali terpaksa
- Paginate semua list, jangan all()

## Naming
- Tabel: snake_case plural (buku, anggota, peminjaman)
- Kolom: snake_case (tanggal_pinjam, batas_kembali)
- Foreign key: {table}_id (buku_id, anggota_id)
```

#### [[10. Menggunakan Dokumentasi sebagai Konteks]]

text

```
AI sering hallucinate API yang tidak ada atau sudah deprecated.
Solusi: beri AI akses ke dokumentasi yang benar.

CARA 1: @docs di Cursor (index dokumentasi online)
─────────────────────────────────────────────────────
1. Buka Cursor Settings → Features → Docs
2. Tambah URL dokumentasi:
   - https://laravel.com/docs/11.x
   - https://tailwindcss.com/docs
   - https://livewire.laravel.com/docs
3. Saat prompt, gunakan: @docs Laravel "cara buat custom cast"

CARA 2: Copy-paste dari dokumentasi ke prompt
─────────────────────────────────────────────────────
"Berikut adalah dokumentasi Laravel Sanctum untuk SPA authentication:
[paste dari docs]

Berdasarkan dokumentasi di atas, implementasikan auth untuk
SPA React yang berjalan di domain yang sama dengan Laravel API."

CARA 3: @web untuk search real-time
─────────────────────────────────────────────────────
"@web Laravel 11 middleware registration bootstrap/app.php
Bagaimana cara mendaftarkan middleware global di Laravel 11?
Buatkan contoh implementasinya di bootstrap/app.php."
```

---

### 🏗️ Checkpoint Level 2

text

```
✅ Checklist sebelum lanjut ke Level 3:

CONTEXT MANAGEMENT:
├── .cursorrules terkonfigurasi dengan tech stack dan conventions
├── Minimal 3 rules files di .cursor/rules/
├── Dokumentasi Laravel/framework sudah di-index di Cursor
└── Bisa reference file yang tepat saat prompt

PRAKTIK:
├── Generate CRUD lengkap dengan context model + route + request
├── AI menghasilkan kode yang konsisten dengan existing codebase
├── Tidak ada "hallucinated API" karena docs sudah di-reference
└── Iterasi prompt maksimal 2-3x untuk hasil yang benar (bukan 10x)

Git: feat: add AI context rules and generate context-aware CRUD
```

---

## 🟡 LEVEL 3: MULTI-FILE WORKFLOW (Minggu 4-6)

> **Tema**: _"Dari edit satu file ke transformasi seluruh project"_  
> **Benang Merah**: Single-file prompt (Level 2) → fitur nyata melibatkan banyak file → Composer mode di Cursor → apply changes lintas file → review diff  
> **Output**: Fitur peminjaman buku yang lengkap (model + migration + controller + view + test) yang di-generate AI dalam satu sesi

---

### E. Multi-File Editing dengan AI

> 💡 **Mengapa ini level tersendiri?** Fitur nyata tidak pernah satu file. "Tambahkan fitur peminjaman" berarti: migration baru, model baru, controller baru, route baru, view baru, policy baru, test baru. AI yang bisa handle ini sekaligus adalah game-changer.

text

```
Benang Merah Bagian E:
Single file: AI edit satu file (Level 2) →
Realitas: satu fitur = 5-10 file →
Composer/Agent mode: AI bisa buat dan edit banyak file →
Review diff: kamu cek setiap perubahan →
Accept/reject per file → commit sebagai satu unit
```

#### [[11. Composer/Agent Mode — AI yang Bisa Edit Banyak File]]

text

```
Di Cursor, gunakan Composer (Cmd+I / Ctrl+I) untuk multi-file editing:

PROMPT COMPOSER — Fitur Peminjaman Buku:
─────────────────────────────────────────
"Buatkan fitur peminjaman buku yang lengkap. Berikut konteksnya:

Model yang sudah ada:
@app/Models/Buku.php (punya scope tersedia(), relasi peminjaman())
@app/Models/User.php (punya role: admin, pustakawan, anggota)

Yang perlu dibuat:
1. Migration: create_peminjaman_table (buku_id, anggota_id,
   tanggal_pinjam, batas_kembali, tanggal_kembali, denda, status)
2. Model: Peminjaman (relasi ke Buku dan User, scope aktif/terlambat)
3. Controller: PeminjamanController (store, show, index, kembalikan)
4. Form Request: StorePeminjamanRequest (validasi stok, batas pinjam)
5. Policy: PeminjamanPolicy (anggota bisa pinjam, admin bisa lihat semua)
6. Routes: tambahkan ke routes/web.php di dalam group auth
7. Views: peminjaman/index.blade.php dan peminjaman/show.blade.php
   (extend layouts.app, gunakan Tailwind)
8. Event: BukuDipinjam (dispatch saat pinjam berhasil)
9. Test: Feature test untuk store dan kembalikan

Aturan:
- Gunakan Action pattern untuk logika pinjam (PinjamBukuAction)
- Transaction untuk operasi pinjam (kurangi stok + buat record)
- Eager load relasi di semua query
- Soft delete tidak perlu untuk peminjaman"

HASIL: Cursor akan membuat/edit 9+ file sekaligus.
Kamu review setiap file di diff view, accept/reject per file.
```

#### [[12. Strategi Iterasi Multi-File]]

text

```
JANGAN minta AI buat semuanya sekaligus untuk fitur yang sangat besar.
Pecah menjadi beberapa iterasi:

ITERASI 1: Data Layer
─────────────────────
Prompt: "Buatkan migration, model, dan factory untuk Peminjaman.
@app/Models/Buku.php @app/Models/User.php
Include relasi, scope, dan cast yang tepat."
→ Review: cek migration, cek relasi, cek foreign key
→ Test: php artisan migrate:fresh --seed

ITERASI 2: Business Logic
─────────────────────────
Prompt: "Buatkan PinjamBukuAction dan KembalikanBukuAction.
@database/migrations/..._create_peminjaman_table.php
@app/Models/Buku.php @app/Models/Peminjaman.php
Include validasi business rules dan transaction."
→ Review: cek transaction, cek edge cases
→ Test: unit test untuk Action

ITERASI 3: HTTP Layer
─────────────────────
Prompt: "Buatkan PeminjamanController, FormRequest, Policy, dan routes.
@app/Actions/PinjamBukuAction.php
@app/Models/Peminjaman.php
Gunakan constructor injection untuk Action."
→ Review: cek authorization, cek validasi
→ Test: feature test untuk endpoint

ITERASI 4: View Layer
─────────────────────
Prompt: "Buatkan Blade views untuk peminjaman.
@resources/views/layouts/app.blade.php
@resources/views/buku/show.blade.php
Gunakan Tailwind, extend layout yang sudah ada."
→ Review: cek tampilan, cek responsive
→ Test: manual di browser
```

#### [[13. Review dan Quality Control untuk Multi-File Changes]]

text

```
CHECKLIST REVIEW SETELAH AI GENERATE MULTI-FILE:
─────────────────────────────────────────────────

□ Konsistensi antar file
  ├── Apakah nama relasi di model match dengan controller?
  ├── Apakah route name match dengan redirect di controller?
  ├── Apakah field di FormRequest match dengan $fillable di model?
  └── Apakah foreign key di migration match dengan relasi di model?

□ Security
  ├── Apakah $fillable benar (tidak terlalu permissive)?
  ├── Apakah Policy check ada di semua method yang perlu?
  ├── Apakah CSRF token ada di semua form?
  └── Apakah input divalidasi sebelum diproses?

□ Performance
  ├── Apakah ada N+1 query (cek relasi yang di-akses di view)?
  ├── Apakah paginasi digunakan untuk list?
  └── Apakah eager loading sudah tepat?

□ Error Handling
  ├── Apakah findOrFail() digunakan, bukan find()?
  ├── Apakah transaction digunakan untuk operasi multi-step?
  └── Apakah flash message ada setelah redirect?

□ Testing
  ├── Apakah test cover happy path?
  ├── Apakah test cover error cases?
  └── Apakah test bisa dijalankan tanpa error?
```

---

### 🏗️ Checkpoint Level 3

text

```
✅ Checklist sebelum lanjut ke Level 4:

MULTI-FILE WORKFLOW:
├── Bisa generate fitur lengkap (5+ file) dalam satu sesi Composer
├── Bisa pecah fitur besar menjadi iterasi yang manageable
├── Selalu review diff sebelum accept changes
└── Bisa spot inkonsistensi antar file yang di-generate AI

PROYEK:
├── Fitur peminjaman buku lengkap (migration → model → action → controller → view)
├── Fitur pengembalian buku dengan hitung denda
├── Semua test hijau: php artisan test
└── Tidak ada N+1 query (cek dengan Laravel Debugbar)

Git: feat: implement book borrowing system with AI-assisted multi-file workflow
```

---

## 🟠 LEVEL 4: DEBUGGING DENGAN AI (Minggu 6-8)

> **Tema**: _"Dari stuck berjam-jam ke fix dalam hitungan menit"_  
> **Benang Merah**: Bug terjadi (tidak terhindarkan) → copy error ke AI → AI trace root cause → AI suggest fix → kamu verifikasi dan apply  
> **Output**: Kemampuan debug 10x lebih cepat dengan AI sebagai pair debugger

---

### F. AI-Assisted Debugging

> 💡 **Mengapa ini skill paling berharga?** Developer menghabiskan 50-70% waktu untuk debugging. AI bisa memangkas ini secara drastis — jika kamu tahu cara memberikannya konteks yang tepat.

text

```
Benang Merah Bagian F:
Bug muncul: error message, perilaku aneh, test gagal →
Copy error + konteks ke AI →
AI analisis root cause (bukan hanya gejala) →
AI suggest beberapa solusi →
Kamu pilih solusi yang paling tepat →
Verifikasi fix tidak menyebabkan bug baru
```

#### [[14. Framework Prompt untuk Debugging]]

text

```
PROMPT DEBUGGING YANG EFEKTIF:
──────────────────────────────

" Saya mendapat error berikut saat [aksi yang dilakukan]:

ERROR:
[paste full error message + stack trace]

KONTEKS:
- Laravel 11, PHP 8.3, MySQL 8
- Error terjadi di [halaman/endpoint mana]
- Data yang dikirim: [jika relevan]
- Ini mulai terjadi setelah [perubahan terakhir yang dilakukan]

FILE TERKAIT:
@file-yang-error.php
@file-terkait-1.php
@file-terkait-2.php

YANG SUDAH SAYA COBA:
1. [langkah 1] → hasil: [masih error / error berubah]
2. [langkah 2] → hasil: [...]

Pertanyaan:
1. Apa root cause dari error ini?
2. Apa solusi yang paling aman (bukan quick hack)?
3. Apakah fix ini bisa menyebabkan masalah lain?"
```

#### [[15. Skenario Debugging Umum dengan AI]]

text

```
SKENARIO 1: N+1 Query Problem
──────────────────────────────
Prompt: "Halaman katalog buku sangat lambat (3+ detik).
@app/Http/Controllers/BukuController.php
@resources/views/buku/index.blade.php
Saya curiga ada N+1 query. Bisa identifikasi dan perbaiki?
Jelaskan juga mengapa ini terjadi."

AI akan:
1. Identifikasi relasi yang di-akses di view tanpa eager load
2. Suggest ->with(['relasi']) di controller
3. Jelaskan perbedaan query count sebelum dan sesudah fix

SKENARIO 2: Mass Assignment Exception
──────────────────────────────────────
Prompt: "Saya dapat error 'Add [field] to fillable' saat create buku.
@app/Models/Buku.php
@app/Http/Controllers/BukuController.php
Saya sudah tambah field baru di migration tapi lupa update model."

AI akan:
1. Jelaskan konsep mass assignment protection
2. Suggest update $fillable
3. Ingatkan untuk juga update FormRequest

SKENARIO 3: CSRF Token Mismatch
────────────────────────────────
Prompt: "Form POST saya selalu return 419 Page Expired.
@resources/views/buku/create.blade.php
@routes/web.php
Ini terjadi setelah saya pindah form ke modal Livewire."

AI akan:
1. Jelaskan mengapa CSRF token expired di modal
2. Suggest @csrf atau @csrf_field() di form
3. Jika Livewire: suggest wire:submit yang handle CSRF otomatis

SKENARIO 4: Migration Error
────────────────────────────
Prompt: "php artisan migrate gagal dengan error:
'SQLSTATE[42S01]: Base table or view already exists'
@database/migrations/
Saya sudah jalankan migrate sebelumnya tapi ada yang error di tengah."

AI akan:
1. Jelaskan bahwa sebagian migration sudah jalan
2. Suggest php artisan migrate:rollback lalu migrate lagi
3. Atau suggest php artisan migrate:fresh (dev only)

SKENARIO 5: "Kode AI Tidak Jalan"
──────────────────────────────────
Prompt: "Kode yang kamu generate sebelumnya tidak jalan.
Error: [paste error]
Ini kode yang kamu buat: [paste kode]
Sepertinya kamu menggunakan API yang sudah deprecated di Laravel 11.
Cek dokumentasi terbaru dan perbaiki."

AI akan:
1. Akui kesalahan (biasanya pakai API Laravel 9/10)
2. Update ke syntax Laravel 11 yang benar
3. Jelaskan perubahannya
```

#### [[16. Debugging Test yang Gagal]]

text

```
PROMPT UNTUK FIX FAILING TEST:
──────────────────────────────

"Test berikut gagal:

TEST: test_admin_bisa_tambah_buku
ERROR: Expected response status [200] but received [422].

TEST CODE:
@tests/Feature/BukuTest.php (method test_admin_bisa_tambah_buku)

CONTROLLER:
@app/Http/Controllers/BukuController.php

FORM REQUEST:
@app/Http/Requests/StoreBukuRequest.php

Kemungkinan penyebab:
- Data test tidak match dengan validasi rules
- FormRequest authorize() return false
- Field yang dikirim kurang

Analisis dan perbaiki. Jangan ubah test — perbaiki kode aplikasinya."
```

---

### 🏗️ Checkpoint Level 4

text

```
✅ Checklist sebelum lanjut ke Level 5:

DEBUGGING SKILL:
├── Bisa copy error + konteks ke AI dan dapat solusi dalam 1-2 prompt
├── Bisa bedakan root cause vs gejala dari penjelasan AI
├── Selalu verifikasi fix AI tidak menyebabkan bug baru
├── Bisa debug N+1, CSRF, migration, dan mass assignment dengan AI
└── Bisa fix failing test dengan bantuan AI

KEBIASAAN:
├── Jangan langsung apply fix AI — baca dan pahami dulu
├── Selalu test setelah fix (manual + automated)
├── Catat root cause untuk referensi masa depan
└── Jika AI fix tidak jalan, beri feedback dan iterasi

Git: feat: fix bugs with AI-assisted debugging workflow
```

---

## 🔴 LEVEL 5: ARSITEKTUR DAN PLANNING (Minggu 8-12)

> **Tema**: _"Dari implementer menjadi arsitek — menggunakan AI sebagai thinking partner"_  
> **Benang Merah**: AI untuk coding (Level 1-4) → AI untuk thinking → planning arsitektur → design database → evaluasi trade-off → technical decision  
> **Output**: Arsitektur sistem perpustakaan yang dirancang bersama AI, dengan keputusan yang terdokumentasi

---

### G. AI sebagai Thinking Partner

> 💡 **Mengapa ini level tertinggi?** Coding adalah bagian termudah dari software engineering. Bagian tersulit adalah memutuskan _apa_ yang harus dibangun dan _bagaimana_ strukturnya. AI bisa menjadi sounding board yang luar biasa untuk ini.

text

```
Benang Merah Bagian G:
AI untuk coding: sudah dikuasai (Level 1-4) →
AI untuk thinking: gunakan AI untuk brainstorming arsitektur →
Planning sebelum coding: AI bantu identifikasi edge cases →
Design review: AI critique keputusan kamu →
Dokumentasi: AI bantu tulis ADR (Architecture Decision Record)
```

#### [[17. Planning Fitur dengan AI]]

text

```
PROMPT PLANNING — Sebelum Tulis Kode:
──────────────────────────────────────

"Saya ingin menambahkan fitur sistem denda otomatis untuk
keterlambatan pengembalian buku di aplikasi perpustakaan.

KONTEKS:
- Laravel 11, MySQL, Redis queue
- Sudah ada: model Peminjaman, Buku, User
- Sudah ada: fitur pinjam dan kembalikan manual
- Pengguna: 8000+ anggota aktif, 350+ peminjaman/hari

TOLONG BANTU SAYA:
1. Identifikasi semua edge cases yang perlu dipertimbangkan
2. Rancang flow sistem denda (kapan dihitung, kapan dikenakan)
3. Tentukan apakah perlu cron job, event-driven, atau keduanya
4. Identifikasi risiko dan mitigasi
5. Buat estimasi complexity (simple/medium/complex)
6. Rekomendasikan urutan implementasi

JANGAN tulis kode dulu. Saya ingin diskusi desain terlebih dahulu."
```

#### [[18. Design Database dengan AI]]

text

```
PROMPT DATABASE DESIGN:
───────────────────────

"Saya sedang merancang database untuk sistem perpustakaan digital.
Berikut entitas yang saya butuhkan:

- Buku (judul, pengarang, ISBN, kategori, stok)
- Anggota (nama, email, role, status)
- Peminjaman (buku, anggota, tanggal, status, denda)
- Kategori (nama, slug, parent kategori)
- Review (buku, anggota, rating, komentar)
- Notifikasi (anggota, pesan, status baca)

TOLONG:
1. Rancang ERD (Entity Relationship Diagram) dalam bentuk teks
2. Tentukan relasi antar entitas (1:1, 1:N, M:N)
3. Identifikasi index yang perlu ditambahkan
4. Sarankan field yang sering dilupakan
5. Identifikasi potential bottleneck untuk 100K+ records
6. Rekomendasikan apakah ada tabel yang sebaiknya dipisah

Format output:
- Untuk setiap tabel: kolom, tipe data, constraint, index
- Untuk setiap relasi: foreign key, cascade behavior
- Untuk setiap keputusan: alasan (why, bukan hanya what)"
```

#### [[19. Code Review dengan AI]]

text

```
PROMPT CODE REVIEW:
───────────────────

"Review pull request berikut sebagai senior developer.
Fokus pada:

1. SECURITY: vulnerability, injection, authorization bypass
2. PERFORMANCE: N+1, missing index, unnecessary query
3. MAINTAINABILITY: naming, structure, DRY violations
4. CORRECTNESS: edge cases, race conditions, error handling
5. LARAVEL BEST PRACTICES: convention, Eloquent usage

FILES:
@diff-pull-request.patch

Untuk setiap issue yang ditemukan:
- Severity: Critical / High / Medium / Low
- File dan line number
- Penjelasan masalah
- Suggested fix
- Mengapa ini penting"
```

#### [[20. Architecture Decision Record (ADR) dengan AI]]

text

```
PROMPT ADR:
───────────

"Buatkan Architecture Decision Record (ADR) untuk keputusan berikut:

KEPUTUSAN: Menggunakan Redis untuk queue dan cache, bukan database.

KONTEKS:
- Aplikasi perpustakaan dengan 350+ peminjaman/hari
- Email notifikasi harus terkirim dalam < 1 menit
- Cache katalog buku yang diakses 10K+ kali/hari
- Tim: 3 developer, 1 DevOps

TOLONG BUAT ADR dengan format:
1. Title
2. Status (proposed/accepted/deprecated)
3. Context (mengapa keputusan ini perlu dibuat)
4. Decision (apa yang diputuskan)
5. Consequences (positif dan negatif)
6. Alternatives Considered (opsi lain dan mengapa ditolak)
7. References"
```

---

### 🏗️ Checkpoint Level 5

text

```
✅ Checklist sebelum lanjut ke Level 6:

PLANNING SKILL:
├── Bisa gunakan AI untuk brainstorming sebelum coding
├── Bisa identifikasi edge cases dengan bantuan AI
├── Bisa rancang database schema bersama AI
├── Bisa gunakan AI untuk code review yang komprehensif
└── Bisa dokumentasikan keputusan arsitektur (ADR) dengan AI

PROYEK:
├── ADR untuk minimal 3 keputusan arsitektur
├── Database schema yang sudah di-review AI
├── Planning doc untuk fitur besar berikutnya
└── Code review AI untuk semua kode yang sudah ditulis

Git: docs: add architecture decision records and design documents
```

---

## ⚫ LEVEL 6: TESTING DAN QUALITY (Minggu 12-16)

> **Tema**: _"AI-generated test suite yang membuat kamu percaya diri deploy"_  
> **Benang Merah**: Kode sudah jalan (Level 1-5) → tapi apakah benar? → AI generate test → AI generate edge cases → CI pipeline → confidence untuk deploy  
> **Output**: Test suite lengkap yang di-generate AI, coverage >70%, CI pipeline aktif

---

### H. AI-Generated Testing

> 💡 **Mengapa AI sangat bagus untuk testing?** Menulis test itu membosankan tapi penting. AI bisa generate test case yang komprehensif — termasuk edge cases yang sering kamu lupakan — dalam hitungan detik.

text

```
Benang Merah Bagian H:
Kode sudah jalan (Level 1-5) →
Tapi "jalan" ≠ "benar" →
Test: verifikasi kode benar secara otomatis →
AI sangat bagus generate test (lebih bagus dari generate kode!) →
Unit test + Feature test + Edge cases →
CI pipeline: test otomatis setiap commit
```

#### [[21. Generate Test Suite dengan AI]]

text

```
PROMPT UNTUK GENERATE TEST:
────────────────────────────

"Buatkan test suite lengkap untuk PinjamBukuAction.

FILE YANG DI-TEST:
@app/Actions/PinjamBukuAction.php

MODEL TERKAIT:
@app/Models/Buku.php
@app/Models/User.php
@app/Models/Peminjaman.php

TEST CASES YANG HARUS DI-COVER:

Happy Path:
1. Anggota berhasil pinjam buku yang tersedia
2. Stok berkurang setelah pinjam
3. Record peminjaman terbentuk dengan benar
4. Event BukuDipinjam di-dispatch

Error Cases:
5. Gagal pinjam buku yang stoknya 0
6. Gagal pinjam buku yang stoknya sudah di-pinjam orang lain (race condition)
7. Gagal pinjam jika anggota sudah mencapai batas (5 buku)
8. Gagal pinjam jika anggota memiliki denda yang belum dibayar
9. Gagal pinjam jika anggota statusnya nonaktif

Edge Cases:
10. Pinjam buku terakhir (stok = 1 → 0)
11. Pinjam oleh admin (batas tidak berlaku)
12. Concurrent pinjam: 2 anggota pinjam buku yang sama bersamaan

Gunakan:
- PHPUnit dengan RefreshDatabase trait
- Factory untuk data test
- Event::fake() untuk verify event dispatch
- assertDatabaseHas/assertDatabaseMissing
- Data provider untuk variasi test"
```

#### [[22. Generate Edge Cases yang Kamu Lupakan]]

text

```
PROMPT EDGE CASE DISCOVERY:
────────────────────────────

"Saya sudah menulis test untuk fitur pengembalian buku:
@tests/Feature/PengembalianTest.php

Tolong identifikasi edge cases yang BELUM saya cover.
Pikirkan dari perspektif:

1. Timing: pengembalian tepat di batas waktu, 1 detik setelah batas
2. Data integrity: buku sudah dihapus (soft delete) saat dikembalikan
3. Concurrent: 2 request kembalikan buku yang sama bersamaan
4. State: peminjaman sudah pernah dikembalikan (double return)
5. Authorization: anggota A kembalikan peminjaman anggota B
6. Business rules: denda dihitung saat hari libur
7. System: database connection lost saat tengah transaksi

Untuk setiap edge case yang belum di-cover:
- Tulis test case lengkap
- Jelaskan mengapa ini penting
- Estimasi severity jika bug ini lolos ke production"
```

#### [[23. CI Pipeline dengan AI]]

text

```
PROMPT CI/CD SETUP:
───────────────────

"Buatkan GitHub Actions workflow untuk project Laravel ini.

KONTEKS:
@composer.json
@phpunit.xml
@.env.example

REQUIREMENTS:
1. Trigger: setiap push ke main dan pull request
2. PHP 8.3, MySQL 8, Redis 7
3. Steps:
   - Install dependencies (cache composer)
   - Setup .env.testing
   - Run migrations
   - Run PHPUnit dengan coverage
   - Run PHPStan level 6
   - Run Laravel Pint (code style check)
4. Fail jika:
   - Ada test yang gagal
   - Coverage < 70%
   - PHPStan ada error
   - Code style tidak sesuai PSR-12
5. Deploy ke production hanya jika semua pass dan branch = main"
```

---

### 🏗️ Checkpoint Level 6

text

```
✅ Checklist sebelum lanjut ke Level 7:

TESTING:
├── AI-generated test suite untuk semua Action dan Controller
├── Coverage > 70% (cek dengan php artisan test --coverage)
├── Edge cases yang diidentifikasi AI sudah di-cover
├── CI pipeline aktif di GitHub Actions
└── Semua test hijau di CI

QUALITY:
├── PHPStan level 6: 0 error
├── Laravel Pint: code style konsisten
├── Tidak ada test yang di-skip atau di-mark incomplete
└── Test bisa dijalankan secara parallel tanpa conflict

Git: feat: add comprehensive AI-generated test suite and CI pipeline
```

---

## 🟣 LEVEL 7: FULL-STACK PRODUCTION (Minggu 16+)

> **Tema**: _"Dari prototype ke production SaaS dengan AI sebagai co-pilot penuh"_  
> **Benang Merah**: Semua skill sudah dikuasai (Level 1-6) → full-stack development → deployment → monitoring → maintenance → continuous improvement  
> **Output**: Aplikasi SaaS perpustakaan yang live, monitored, dan terus berkembang

---

### I. Full-Stack Development dengan AI

#### [[24. Frontend + Backend dalam Satu Flow]]

text

```
PROMPT FULL-STACK FEATURE:
───────────────────────────

"Buatkan fitur dashboard statistik perpustakaan yang lengkap.

BACKEND (Laravel):
- Endpoint API: GET /api/v1/dashboard/statistik
- Data: total buku, total anggota, peminjaman hari ini,
  buku paling populer (top 10), grafik peminjaman 30 hari
- Cache response selama 10 menit (Redis)
- Authorization: hanya admin dan pustakawan

FRONTEND (Livewire + Tailwind):
- Komponen Livewire: DashboardStatistik
- 4 kartu statistik di atas (total buku, anggota, pinjam hari ini, denda)
- Grafik garis: peminjaman 30 hari terakhir (gunakan Chart.js)
- Tabel: top 10 buku paling dipinjam
- Auto-refresh setiap 5 menit
- Loading skeleton saat data belum siap

DATABASE QUERY:
- Gunakan aggregate queries (COUNT, SUM, GROUP BY)
- Optimasi untuk 100K+ records peminjaman
- Jangan N+1!

Buatkan semua file yang diperlukan: controller, route,
Livewire component, Blade view, dan test."
```

#### [[25. Deployment dengan AI Assistance]]

text

```
PROMPT DEPLOYMENT:
──────────────────

"Bantu saya deploy aplikasi Laravel ini ke production.

SERVER: VPS Ubuntu 22.04, 2GB RAM, 1 CPU
DOMAIN: perpustakaan.kota.id
REGISTRAR: Cloudflare

YANG SUDAH ADA:
- Kode di GitHub
- CI pipeline di GitHub Actions (test pass)
- Domain sudah pointing ke IP server

YANG PERLU DIBUAT:
1. Server setup script (Nginx, PHP 8.3, MySQL, Redis)
2. Nginx config untuk Laravel
3. SSL dengan Let's Encrypt
4. Supervisor config untuk queue worker
5. Cron job untuk scheduler
6. GitHub Actions deploy step (SSH)
7. .env production template
8. Checklist keamanan production

Buatkan step-by-step yang bisa saya jalankan satu per satu.
Untuk setiap step, jelaskan APA yang dilakukan dan MENGAPA."
```

#### [[26. Monitoring dan Maintenance dengan AI]]

text

```
PROMPT MONITORING:
──────────────────

"Buatkan sistem monitoring untuk aplikasi perpustakaan yang
sudah live dengan 8000+ pengguna.

YANG PERLU DIMONITOR:
1. Error rate (target: < 0.1%)
2. Response time (target: < 500ms p95)
3. Queue backlog (target: < 100 jobs)
4. Database slow queries (> 1 detik)
5. Disk usage untuk storage dan log
6. Failed jobs count

TOOLS YANG TERSEDIA:
- Laravel Telescope (development)
- Laravel Pulse (production)
- Sentry (error tracking)
- UptimeRobot (uptime monitoring)

Buatkan:
1. Konfigurasi Laravel Pulse untuk dashboard production
2. Setup Sentry integration
3. Alert rules (kapan harus notifikasi)
4. Runbook: apa yang harus dilakukan saat alert trigger
5. Weekly health check script"
```

---

### J. Workflow Vibes Coding yang Mature

#### [[27. Daily Workflow Vibes Coding]]

text

```
┌─────────────────────────────────────────────────────────┐
│          DAILY VIBES CODING WORKFLOW                     │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  09:00 — PLANNING (15 menit)                            │
│  ├── Buka project di Cursor                             │
│  ├── Review TODO list / GitHub Issues                   │
│  ├── Pilih 1-2 fitur untuk hari ini                     │
│  └── Prompt AI: "Bantu saya plan implementasi fitur X"  │
│                                                         │
│  09:15 — IMPLEMENTASI (2-3 jam)                         │
│  ├── Composer mode: generate kode fitur                 │
│  ├── Review setiap file yang di-generate                │
│  ├── Iterasi prompt jika ada yang kurang                │
│  ├── Test manual di browser                             │
│  └── Commit per logical unit                            │
│                                                         │
│  12:00 — TESTING (30 menit)                             │
│  ├── Prompt AI: "Generate test untuk fitur yang baru"   │
│  ├── Jalankan php artisan test                          │
│  ├── Fix failing test (dengan bantuan AI)               │
│  └── Commit test                                        │
│                                                         │
│  13:00 — REVIEW (30 menit)                              │
│  ├── Prompt AI: "Review kode yang saya buat hari ini"   │
│  ├── Cek security, performance, edge cases              │
│  ├── Apply suggested improvements                       │
│  └── Commit improvements                                │
│                                                         │
│  14:00 — LANJUT IMPLEMENTASI / FITUR BERIKUTNYA         │
│  ├── Ulangi siklus di atas                              │
│  └── ...                                                │
│                                                         │
│  17:00 — WRAP UP (15 menit)                             │
│  ├── Push ke GitHub                                     │
│  ├── Cek CI pipeline                                    │
│  ├── Update TODO list                                   │
│  └── Prompt AI: "Buatkan commit message dan PR desc"    │
│                                                         │
└─────────────────────────────────────────────────────────┘

METRIK PRODUKTIVITAS:
├── Sebelum AI: 1-2 fitur kecil per hari
├── Dengan AI (Level 1-3): 3-5 fitur per hari
├── Dengan AI (Level 4-6): 5-10 fitur per hari
└── Dengan AI (Level 7): entire SaaS dalam 2-4 minggu
```

#### [[28. Anti-Pattern Vibes Coding — Yang Harus Dihindari]]

text

```
❌ ANTI-PATTERN 1: "Accept All" Syndrome
─────────────────────────────────────────
Gejala: Klik "Accept" di semua suggestion AI tanpa baca
Bahaya: Kode yang tidak kamu pahami, bug tersembunyi, security hole
Fix: BACA setiap baris sebelum accept. Jika tidak paham, tanya AI.

❌ ANTI-PATTERN 2: "Prompt and Pray"
─────────────────────────────────────
Gejala: Tulis prompt vague, harap AI baca pikiran kamu
Bahaya: Hasil tidak sesuai, iterasi 10x, lebih lambat dari manual
Fix: Gunakan framework prompt: konteks + instruksi + constraint.

❌ ANTI-PATTERN 3: "AI Knows Best"
──────────────────────────────────
Gejala: Terima arsitektur AI tanpa kritik
Bahaya: AI bisa suggest pattern yang outdated atau tidak cocok
Fix: Selalu cross-check dengan dokumentasi resmi dan best practices.

❌ ANTI-PATTERN 4: "No Context"
───────────────────────────────
Gejala: Prompt tanpa reference file atau codebase context
Bahaya: AI generate kode yang tidak kompatibel dengan project
Fix: Selalu @-reference file yang relevan.

❌ ANTI-PATTERN 5: "Skip Testing"
──────────────────────────────────
Gejala: "Kode AI sudah jalan, tidak perlu test"
Bahaya: Bug muncul di production, regression tidak terdeteksi
Fix: AI-generate test SETELAH AI-generate kode. Selalu.

❌ ANTI-PATTERN 6: "Cargo Cult AI"
──────────────────────────────────
Gejala: Gunakan AI untuk hal yang lebih cepat ditulis manual
Bahaya: Overhead prompt > waktu tulis sendiri
Fix: Gunakan AI untuk hal yang kompleks, repetitif, atau unfamiliar.
      Untuk kode 3 baris yang sudah kamu hafal, tulis sendiri.

❌ ANTI-PATTERN 7: "Forget the Fundamentals"
────────────────────────────────────────────
Gejala: Tidak belajar konsep dasar karena "AI bisa handle"
Bahaya: Kamu tidak bisa review, debug, atau arsitek tanpa fondasi
Fix: Terus belajar fondasi. AI memperkuat skill, bukan menggantikan.
```

---

### 🏗️ Checkpoint Level 7 (Final)

text

```
✅ Checklist Akhir — AI-Assisted Developer yang Mature:

WORKFLOW:
├── Daily workflow: plan → implement → test → review → commit
├── Setiap fitur dimulai dengan planning prompt
├── Setiap fitur diakhiri dengan test dan review
├── CI/CD pipeline aktif dan hijau
└── Deployment otomatis setelah test pass

KUALITAS KODE:
├── Semua kode AI-generated sudah di-review
├── Test coverage > 70%
├── 0 critical security vulnerability
├── 0 N+1 query di production
└── Response time < 500ms p95

PRODUKSI:
├── Aplikasi live di VPS dengan SSL
├── Monitoring aktif (Pulse/Sentry)
├── Queue worker berjalan (Supervisor)
├── Backup otomatis
└── Error rate < 0.1%

MINDSET:
├── Kamu adalah arsitek, AI adalah kontraktor
├── Kamu bisa jelaskan setiap baris kode di project
├── Kamu tahu kapan AI membantu dan kapan AI menyesatkan
├── Kamu terus belajar fondasi, bukan hanya prompt
└── Kamu tidak takut untuk override keputusan AI

Git: feat: production-ready SaaS with full AI-assisted development workflow
```

---

## 📊 Ringkasan Progress Tracking

### Satu Project, 7 Level Enhancement

text

```
Level 1: Setup AI tools + prompt dasar + generate kode pertama
  + Level 2: + Context management → AI yang paham project
  + Level 3: + Multi-file workflow → fitur lengkap lintas file
  + Level 4: + Debugging dengan AI → fix bug 10x lebih cepat
  + Level 5: + Planning & arsitektur → AI sebagai thinking partner
  + Level 6: + Testing & quality → AI-generated test suite + CI
  + Level 7: + Full-stack production → SaaS live dengan AI co-pilot
```

### Tabel Progress

|Level|Topik|Durasi|Output Konkret|
|---|---|---|---|
|🟢 **1**|1-7|Minggu 1-2|Setup tools, prompt framework, kode pertama|
|🔵 **2**|8-10|Minggu 2-4|Context management, AI yang codebase-aware|
|🟡 **3**|11-13|Minggu 4-6|Multi-file editing, fitur lengkap|
|🟠 **4**|14-16|Minggu 6-8|AI-assisted debugging, fix bug cepat|
|🔴 **5**|17-20|Minggu 8-12|Planning, arsitektur, code review|
|⚫ **6**|21-23|Minggu 12-16|Test suite, CI pipeline, quality gates|
|🟣 **7**|24-28|Minggu 16+|Full-stack SaaS, deployment, monitoring|

---

### Benang Merah Utama Sepanjang Roadmap

text

```
Poin 1  (model mental)           → Fondasi: kamu = arsitek, AI = kontraktor
Poin 2  (kapan AI bagus/buruk)   → Peta mental untuk setiap situasi
Poin 3  (setup tools)            → Cursor sebagai IDE utama
Poin 4  (.cursorrules)           → AI yang konsisten dengan standard kamu
Poin 5  (framework prompt)       → Konteks + instruksi + constraint + format
Poin 6  (prompt buruk vs baik)   → Perbedaan antara 1x iterasi vs 10x
Poin 8  (context management)     → @-reference file yang relevan
Poin 10 (dokumentasi sebagai ctx) → Cegah AI hallucinate API
Poin 11 (Composer mode)          → Multi-file editing dalam satu sesi
Poin 13 (review checklist)       → Jangan pernah accept tanpa review
Poin 14 (debugging framework)    → Error + konteks + file + yang sudah dicoba
Poin 17 (planning sebelum code)  → AI sebagai thinking partner
Poin 21 (AI-generated test)      → AI lebih bagus generate test daripada kode
Poin 23 (CI pipeline)            → Otomasi quality gate
Poin 27 (daily workflow)         → Siklus: plan → implement → test → review
Poin 28 (anti-pattern)           → 7 jebakan yang harus dihindari
```

---

## 💡 Cara Menggunakan Roadmap Ini

text

```
Setiap poin mengikuti format:
┌──────────────────────────────────────────────────────────┐
│ 💡 Konteks: mengapa skill ini penting untuk vibes coding │
│ 🔗 Benang Merah: koneksi ke poin sebelum dan sesudahnya  │
│ 📋 Prompt: contoh prompt yang bisa langsung dipakai      │
│ ✅ Langkah konkret: verifikasi berhasil                  │
└──────────────────────────────────────────────────────────┘
```

**Aturan yang Tidak Boleh Dilanggar:**

1. **BACA setiap baris kode AI sebelum accept** — tidak ada "Accept All" tanpa review
2. **TEST setiap fitur AI-generated** — "jalan di laptop" ≠ "siap production"
3. **PAHAMI setiap keputusan arsitektur** — jika tidak bisa jelaskan, jangan pakai
4. **BERI KONTEKS yang cukup** — AI tanpa konteks = AI yang hallucinate
5. **ITERASI prompt, bukan terima hasil pertama** — prompt pertama jarang sempurna
6. **JANGAN skip fondasi** — AI memperkuat skill, bukan menggantikan pembelajaran
7. **CROSS-CHECK dengan dokumentasi resmi** — AI bisa outdated atau salah
8. **COMMIT per logical unit** — jangan satu commit raksasa untuk 20 file
9. **REVIEW diff sebelum merge** — AI bisa introduce subtle bugs
10. **TERUS BELAJAR** — teknologi berubah, AI tools berubah, kamu harus adaptif

---

_Roadmap Vibes Coding v1.0 — Step-by-Step, Quality First, AI as Co-Pilot_  
_Kamu tetap pilot — AI adalah co-pilot yang sangat cepat tapi butuh arahan yang jelas_