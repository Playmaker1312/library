# Format Pembuatan Project — Ketentuan Penyusunan Roadmap Project

> **Dokumen ini adalah KETENTUAN untuk AI.** Dipakai saat menyusun **roadmap latihan project** — yaitu rancangan aplikasi nyata yang dibangun bertahap untuk mengimplementasikan materi yang sudah dipelajari.
> **Pasangan:** `Format Roadmap.md` (roadmap belajar konsep) dan `Format Penjelasan Materi Project.md` (isi tiap langkah project).

---

## 0. Konsep Dasar: Roadmap Belajar vs Roadmap Project

| | **Roadmap Belajar** (`Format Roadmap.md`) | **Roadmap Project** (dokumen ini) |
|---|---|---|
| Fokus | Menguasai konsep & sintaks | Menerapkan konsep jadi aplikasi nyata |
| Isi materi | Teori, sintaks, latihan kecil | Implementasi fitur, arsitektur, integrasi |
| Hasil | Paham 21 fundamental + fitur bahasa | Satu aplikasi utuh yang jalan & dideploy |
| Sumber | 21 materi fundamental | Poin-poin dari roadmap belajar |

**Alurnya:**

```
Format Roadmap  →  Format Penjelasan Materi  →  BELAJAR  →  Format Pembuatan Project  →  Format Penjelasan Materi Project  →  PROYEK JADI
   (rencana)          (bahan belajar)                      (rencana aplikasi)              (panduan membangun)
```

Project dibuat **setelah** roadmap belajar dan materinya dipahami. Tujuan akhirnya: aplikasi nyata yang mengimplementasikan materi-materi tersebut.

---

## 1. Prinsip Wajib Roadmap Project

| # | Prinsip | Artinya |
|---|---|---|
| 1 | **Satu Aplikasi, Tumbuh Bersama** | Semua level membangun SATU aplikasi yang sama (mis. sistem perpustakaan), bukan proyek terpisah-pisah. |
| 2 | **Mengimplementasikan Materi** | Setiap langkah memakai konsep dari roadmap belajar. Sebutkan materi sumbernya. |
| 3 | **Terstruktur** | Ada level/fase, tema, bagian, dan checkpoint — kurang lebih sama dengan roadmap belajar. |
| 4 | **Benang Merah** | Setiap level menambah/meningkatkan fitur level sebelumnya. |
| 5 | **Bisa Dijalankan** | Setiap level menghasilkan aplikasi yang **bisa dijalankan** dan diuji. |
| 6 | **Versioned** | Tiap checkpoint diakhiri **commit Git** dengan pesan konvensional. |
| 7 | **Spesifik & Konkret** | Ada tech stack, struktur folder, definisi "selesai", dan fitur target. |

---

## 2. Struktur Baku File Roadmap Project

### 2.1 Kepala file

```markdown
# Roadmap <X> Native: Step-by-Step Membangun <Nama Aplikasi>

## Filosofi Roadmap Ini

> **"<kutipan tentang pentingnya praktik/project>"** — <alasan singkat>

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: <aplikasi apa> dari <level awal> → <level akhir>
- **Security dari Hari Pertama**: <penjelasan>
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **<Prinsip 4>**: <penjelasan>

### Prasyarat Sebelum Memulai

```
Sebelum roadmap project ini, pastikan sudah:
├── Menyelesaikan [[Roadmap <X>]] (materi konsep)
├── Menguasai <materi fundamental terkait>
├── <tool yang harus terpasang>
└── <pengetahuan dasar lain>
```

### Spesifikasi Aplikasi

| Aspek | Keterangan |
|---|---|
| **Nama Aplikasi** | <mis. Sistem Perpustakaan Digital> |
| **Deskripsi** | <1-2 kalimat> |
| **Fitur Utama** | <daftar fitur yang akan dibangun> |
| **Tech Stack** | <bahasa, framework, database, tools> |
| **Target Akhir** | <aplikasi terdeploy yang bisa diakses> |
```

### 2.2 Gambaran besar

```markdown
## 📋 Gambaran Besar — Apa yang Akan Dibangun

```
Level 1: "<nama>" — <fitur paling dasar>
    ↓ (enhance)
Level 2: + <tambahan> → <fitur baru>
    ↓ (enhance)
...
Level N: + <tambahan> → <aplikasi final + deploy>
```
```

### 2.3 Tiap level

```markdown
## 🟢 LEVEL N: <TEMA> (Minggu a-b)

> **Tema**: _<tujuan level>_
> **Benang Merah**: <level sebelumnya> → <level ini> → <level berikutnya>
> **Output**: <aplikasi bisa melakukan apa setelah level ini>
> **Materi yang Diimplementasikan**: <materi/roadmap terkait>

---

### A. <Nama Bagian>

> 💡 **Mengapa ini penting?** <alasan, kaitkan ke materi>

```
Benang Merah Bagian A:
<...> → <...> → <...>
```

#### [[Belajar/IT/Pemrograman/Project/Project <X>/Level N - <Nama>/<N. Judul>]]

- <penjelasan langkah>
- <penjelasan langkah>

```<bahasa>
<kode/struktur>
```

- _Langkah konkret_: <aksi nyata yang harus dijalankan>
```

> **Catatan link:** roadmap project memakai **path lengkap** menuju folder `Project/`, karena nama file project bisa bentrok dengan file materi biasa. Contoh:
> `[[Belajar/IT/Pemrograman/Project/Project PHP Native/Level 1 - Fondasi PHP/1. PHP adalah Server-Side — Cara Kerja yang Fundamental]]`

### 2.4 Checkpoint tiap level

```markdown
### 🏗️ Checkpoint Level N

```
✅ Checklist sebelum lanjut ke Level N+1:

FITUR:
├── <fitur yang harus jalan>
└── <fitur lain>

KUALITAS:
├── <aspek keamanan/struktur yang wajib>
└── <best practice>

Git: <conventional commit, mis. feat: create book catalog>
```
```

### 2.5 Penutup (opsional tapi disarankan)

```markdown
## 🚀 Langkah Setelah Roadmap

- Deploy ke <platform/hosting>
- Tulis dokumentasi (README, Swagger/OpenAPI)
- Ide pengembangan lanjutan / portofolio
```

---

## 3. Aturan Penyusunan

1. **Satu link = satu langkah materi project** yang detail dan bisa diikuti.
2. Setiap langkah project **merujuk** ke materi belajar yang relevan (mis. "materi OOP nomor 23-25").
3. **Level bertingkat fitur**, bukan bertingkat teori. Contoh: Level 1 CLI → Level 2 katalog in-memory → Level 3 form & session → Level 4 database → Level 5 API & keamanan → Level 6 arsitektur → Level 7 testing & deploy.
4. Setiap level **diakhiri aplikasi yang bisa dijalankan**.
5. **Checkpoint** wajib punya blok FITUR, KUALITAS, dan pesan **Git commit**.
6. **Konsisten** dengan roadmap belajar: istilah, emoji, gaya, penomoran.
7. **Jangan mengulang teori panjang**; fokus pada implementasi. Teori sudah ada di materi belajar.
8. Sertakan **struktur folder** aplikasi yang terus diperbarui tiap level.

---

## 4. Lokasi & Penamaan File

**File roadmap project:**
```
Roadmap/Proyek/Roadmap Project <X>.md
```
Contoh: `Roadmap/Proyek/Roadmap Project PHP.md`, `Roadmap/Proyek/Roadmap Project Laravel.md`.

**File materi project (isi tiap langkah):**
```
Belajar/IT/Pemrograman/Project/Project <X>/
├── Level 1 - <Nama Level>/
│   ├── 1. <Judul>.md
│   └── ...
├── Level 2 - <Nama Level>/
│   └── ...
```

- Nama folder: `Project <X>` dengan `<X>` mengikuti nama roadmap (mis. `Project PHP Native`, `Project Laravel`, `Project NestJS`).
- Nama level: `Level N - <Nama>` (sama seperti roadmap belajar).
- Nomor materi **berurutan global** lintas level (1, 2, 3, …).
- Cara menulis tiap file materi project → lihat `Format Penjelasan Materi Project.md`.

---

## 5. Template Roadmap Project (copy-paste)

```markdown
# Roadmap <X> Native: Step-by-Step Membangun <Nama Aplikasi>

## Filosofi Roadmap Ini

> **"<kutipan>"** — <alasan>

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: <deskripsi>
- **Security dari Hari Pertama**: <deskripsi>
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya

### Prasyarat Sebelum Memulai

```
Sebelum roadmap project ini, pastikan sudah:
├── Menyelesaikan [[Roadmap <X>]]
└── <prasyarat lain>
```

### Spesifikasi Aplikasi

| Aspek | Keterangan |
|---|---|
| Nama Aplikasi | <...> |
| Deskripsi | <...> |
| Fitur Utama | <...> |
| Tech Stack | <...> |
| Target Akhir | <...> |

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

```
Level 1: ...
    ↓ (enhance)
Level N: ...
```

---

## 🟢 LEVEL 1: <TEMA> (Minggu 1-x)

> **Tema**: _<...>_
> **Benang Merah**: <...>
> **Output**: <...>
> **Materi yang Diimplementasikan**: <...>

---

### A. <Bagian>

> 💡 **Mengapa ini penting?** <...>

```
Benang Merah Bagian A:
<...> → <...> → <...>
```

#### [[Belajar/IT/Pemrograman/Project/Project <X>/Level 1 - <Nama>/1. <Judul>]]

- <langkah>
- _Langkah konkret_: <aksi>

```<bahasa>
<kode>
```

---

### 🏗️ Checkpoint Level 1

```
✅ Checklist sebelum lanjut ke Level 2:

FITUR:
├── <...>

KUALITAS:
├── <...>

Git: feat: <...>
```
```

---

## 6. Checklist Kualitas Roadmap Project

- [ ] Ada **Filosofi**, **Prinsip Desain**, **Prasyarat**, dan **Spesifikasi Aplikasi**.
- [ ] Ada **Gambaran Besar** (alur level).
- [ ] Satu aplikasi yang sama tumbuh dari level pertama sampai akhir.
- [ ] Setiap level punya Tema, Benang Merah, Output, dan **Materi yang Diimplementasikan**.
- [ ] Setiap langkah memakai **path lengkap** `[[Belajar/.../Project/...]]`.
- [ ] Setiap level punya **Checkpoint** dengan FITUR + KUALITAS + Git commit.
- [ ] Setiap level menghasilkan aplikasi yang **bisa dijalankan**.
- [ ] Ada keamanan sejak level awal (bukan ditumpuk di akhir).
- [ ] Ada penutup: deploy/dokumentasi/pengembangan lanjutan.

---

**File terkait:**
- `Format Roadmap.md` — menyusun roadmap belajar konsep.
- `Format Penjelasan Materi.md` — menulis materi konsep.
- `Format Penjelasan Materi Project.md` — menulis materi tiap langkah project.
