# Format & Ketentuan — Index

Folder ini berisi **ketentuan yang dibaca AI** sebelum membuat roadmap, materi, dan project. Tujuannya agar semua output punya struktur yang konsisten.

## Daftar Format

| # | File | Dipakai untuk |
|---|---|---|
| 1 | `Format Roadmap.md` | Menyusun **roadmap belajar bahasa pemrograman**. Memuat 21 materi fundamental + fitur khusus per bahasa. |
| 2 | `Format Roadmap Framework.md` | Menyusun **roadmap framework** (Laravel, React, NestJS, Vue, Godot). Memuat 20 kurikulum framework + fitur khas per framework. |
| 3 | `Format Roadmap Tool & Teknologi.md` | Menyusun **roadmap tool/teknologi non-bahasa** (Git, Docker, SQL/MySQL, HTML/CSS, terminal). Memuat cheatsheet + troubleshooting. |
| 4 | `Format Roadmap Non-Programming.md` | Menyusun **roadmap bidang di luar pemrograman** (Bahasa, Filsafat, Psikologi, Sejarah, Matematika, Seni). Memuat pilar inti bidang + artefak belajar. |
| 5 | `Format Penjelasan Materi.md` | Menulis **satu file materi konsep pemrograman** (isi poin roadmap) untuk folder `.../Fundamental/<Teknologi>/...`. |
| 6 | `Format Penjelasan Materi Non-Programming.md` | Menulis **satu file materi bidang non-pemrograman** (isi poin roadmap). Memuat latar/konteks, istilah kunci, sumber, & bedah contoh. |
| 7 | `Format Pembuatan Project.md` | Menyusun **roadmap project** (rencana membangun aplikasi nyata). |
| 8 | `Format Penjelasan Materi Project.md` | Menulis **satu file materi project** (langkah membangun fitur) untuk folder `.../Project/...`. |

## Memilih Format Roadmap

```
Topik yang mau dipelajari?
├── Bahasa pemrograman (JS, PHP, Python, Go, ...)  → Format Roadmap.md
├── Framework aplikasi (Laravel, React, NestJS, ...) → Format Roadmap Framework.md
├── Tool/teknologi IT non-bahasa (Git, Docker, SQL, HTML/CSS, ...) → Format Roadmap Tool & Teknologi.md
└── Bidang non-IT (Bahasa Inggris, Filsafat, Psikologi, Sejarah, Seni, ...) → Format Roadmap Non-Programming.md
```

## Memilih Format Penjelasan Materi

```
Materi apa yang mau ditulis?
├── Materi konsep pemrograman  → Format Penjelasan Materi.md
├── Materi bidang non-IT       → Format Penjelasan Materi Non-Programming.md
└── Materi langkah project     → Format Penjelasan Materi Project.md
```

## Alur Pemakaian

```
1. Format Roadmap / Roadmap Framework / Roadmap Tool & Teknologi / Roadmap Non-Programming
        │  menghasilkan
        ▼
   Roadmap/<Kategori>/Roadmap <Topik>.md
        │  tiap poin dijelaskan lewat
        ▼
2a. Format Penjelasan Materi  (pemrograman)
        │  menghasilkan
        ▼
   Belajar/IT/Pemrograman/.../Fundamental/<Teknologi>/<N. Judul>.md
        │
2b. Format Penjelasan Materi Non-Programming  (bidang lain)
        │  menghasilkan
        ▼
   Belajar/<Bidang>/<Topik>/.../<N. Judul>.md
        │  setelah materi dipahami
        ▼
3. Format Pembuatan Project
        │  menghasilkan
        ▼
   Roadmap/Proyek/Roadmap Project <X>.md
        │  tiap langkah dijelaskan lewat
        ▼
4. Format Penjelasan Materi Project
        │  menghasilkan
        ▼
   Belajar/IT/Pemrograman/Project/Project <X>/<N. Judul>.md
```

## Prinsip yang Dipegang Semua Format

- **Terstruktur** — heading, level, dan penomoran konsisten.
- **Benang Merah** — tiap bagian terhubung ke sebelum & sesudahnya.
- **Lengkap** — 21 fundamental (bahasa) / 20 kurikulum framework / pilar inti bidang (non-IT) selalu tercakup.
- **Kontekstual** — contoh memakai satu tema nyata (mis. sistem perpustakaan untuk pemrograman).
- **Telusur (Tracing/Bedah)** — materi selalu menelusuri eksekusi kode *atau* membedah contoh langkah demi langkah, dengan bahasa sederhana.
- **Aman** — aspek keamanan selalu dibahas di materi project.

## File Terkait di Luar Folder Ini

- `Roadmap/Materi Wajib.md` — daftar 21 fundamental + materi full-stack pendukung.
- `Roadmap/Proyek/Roadmap Project <X>.md` — contoh roadmap project.
- `Roadmap/Backend/Roadmap PHP.md` — contoh roadmap bahasa yang sudah mengikuti format.
- `Roadmap/Bahasa/Roadmap Bahasa Inggris.md` — contoh roadmap bidang non-programming.
