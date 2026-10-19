# Format Penjelasan Materi Project — Ketentuan Penulisan Materi Project

> **Dokumen ini adalah KETENTUAN untuk AI.** Dipakai saat menulis **satu file materi project** — yaitu panduan langkah demi langkah untuk membangun sebuah fitur aplikasi.
> **Berlaku untuk:** materi di folder `Belajar/IT/Pemrograman/Project/Project <X>/...`.
> **Pasangan:** cara menyusun roadmap project-nya ada di `Format Pembuatan Project.md`.

---

## 0. Beda Materi Konsep vs Materi Project

| | **Materi Konsep** (`Format Penjelasan Materi.md`) | **Materi Project** (dokumen ini) |
|---|---|---|
| Tujuan | Memahami konsep & sintaks | **Membangun fitur nyata** memakai konsep itu |
| Nada | Edukatif, dari nol | Praktis, langsung ke implementasi |
| Konten | Penjelasan, analogi, latihan | Kode real-world, keamanan, debugging, tantangan |
| Judul | Tanpa nomor di H1 | **Dengan nomor** di H1 (`# N. Judul`) |

Materi project menjawab: "OK, saya sudah paham teorinya — **sekarang bagaimana cara memasangnya di aplikasi?**"

---

## 1. Struktur Baku File Materi Project

Ikuti urutan heading berikut **persis**:

| Urutan | Heading | Isi singkat |
|---|---|---|
| 1 | `# N. <Judul>` | Judul **dengan nomor** |
| 2 | `> **Level N · <Nama Level>** — <tagline>` | Baris konteks (blockquote) |
| 3 | `---` | Pemisah |
| 4 | `## 1. Penjelasan Singkat & Konsep Dasar` | Konsep inti + istilah + benang merah |
| 5 | `## 2. Fungsi & Kegunaan Utama` | Kenapa fitur ini perlu ada di aplikasi |
| 6 | `## 3. Contoh Kode & Implementasi Real-World` | Kode nyata siap-tempel + struktur folder |
| 7 | `## 4. Tracing Kode Step-by-Step` | **Telusur alur eksekusi** (lihat Bagian 3) |
| 8 | `## 5. Analogi Sederhana` | Analogi "Sistem Administrasi Kantor"/dunia nyata |
| 9 | `## 6. Kapan Harus Digunakan & Kasus Penggunaan` | Kapan dipakai, skenario nyata |
| 10 | `## 7. Aspek Keamanan & Best Practices (Critical)` | Keamanan + praktik terbaik |
| 11 | `## 8. Kesalahan Umum & Debugging` | Error nyata + cara memperbaiki |
| 12 | `## 9. Soal Latihan & Kunci Jawaban` | `### Tantangan 1`, `### Tantangan 2`, `### Jawaban & Pembahasan` |
| 13 | `---` + `**Lanjut ke:** [[...]]` | Footer navigasi |

> **Catatan nomor di H1:** berbeda dengan materi konsep, materi project **memakai nomor** di H1. Contoh: `# 18. Class dan Object — Dasar OOP PHP`.

---

## 2. Aturan Isi Tiap Bagian

### `> **Level N · <Nama Level>** — <tagline>`
- Satu baris, menjelaskan posisi materi ini dalam perjalanan membangun aplikasi.
- Contoh: `> **Level 4 · OOP dan Database PDO** — Dari fungsi terpencar ke paket data + perilaku dalam satu unit.`

### `## 1. Penjelasan Singkat & Konsep Dasar`
- Ringkas, tidak bertele-tele. Cukup 2-4 paragraf.
- Boleh mulai dengan potongan kode mini untuk mengilustrasikan konsep.
- Sertakan tabel **istilah inti** bila ada banyak istilah.
- **Wajib ada "Benang Merah"**: kaitan ke materi/level sebelumnya dan sesudahnya.

### `## 2. Fungsi & Kegunaan Utama`
- Bullet list alasan praktis **kenapa fitur ini dipakai dalam aplikasi**.
- Fokus pada manfaat untuk aplikasi (kohesi, keamanan, maintainability), bukan teori murni.

### `## 3. Contoh Kode & Implementasi Real-World`
- Tampilkan **struktur folder** yang relevan lebih dulu.
- Berikan kode lengkap yang benar-benar bisa dipakai di aplikasi.
- Pakai konteks aplikasi (mis. sistem perpustakaan: `Buku`, `Anggota`, `Peminjaman`).
- Sertakan contoh **pemakaian** setelah definisi.
- Komentar berbahasa Indonesia pada baris penting.

### `## 5. Analogi Sederhana`
- Konsisten memakai kerangka **"Sistem Administrasi Kantor"** (atau analogi yang sudah dipakai roadmap).
- Petakan tiap komponen teknis ke komponen analogi.

### `## 6. Kapan Harus Digunakan & Kasus Penggunaan`
- Kapan fitur ini tepat dipakai, kapan tidak.
- Sebutkan **kasus nyata** dengan contoh konkret (URL, alur, atau skenario).

### `## 7. Aspek Keamanan & Best Practices (Critical)`
- **Wajib ada di setiap materi project.** Ini bukan opsional.
- Sebutkan risiko keamanan spesifik fitur ini (XSS, SQL Injection, CSRF, kebocoran data, validasi).
- Tambahkan bullet **best practices** implementasi.

### `## 8. Kesalahan Umum & Debugging`
- Daftar error nyata yang sering muncul, format: **pesan/gejala** → **penyebab** → **solusi**.
- Sertakan tips debugging.

### `## 9. Soal Latihan & Kunci Jawaban`
- Minimal **2 tantangan** (`### Tantangan 1`, `### Tantangan 2`) yang mengembangkan fitur.
- `### Jawaban & Pembahasan`: kode solusi + **pembahasan alur** (kenapa begitu).

### Footer
- `---` lalu `**Lanjut ke:** [[<materi berikutnya>]]`.

---

## 3. Bagian Baru: `## 4. Tracing Kode Step-by-Step`

Ini bagian **wajib tambahan**. Tujuannya sama seperti di materi konsep: memperlihatkan **apa yang terjadi saat kode berjalan**, langkah demi langkah.

### Aturan

1. **Bahasa sederhana, bukan teknis.** Tanpa istilah rumit.
2. **Tanpa analogi** (analogi ada di bagian 5).
3. Telusuri **urutan eksekusi** program, bukan urutan baris di file.
4. Sebutkan **perubahan nilai** setiap variabel/state.
5. Untuk loop/rekursi/request-response, gunakan **tabel atau diagram ASCII**.
6. Akhiri dengan **hasil akhir** yang persis (output, isi database, atau respons HTTP).

### Contoh (kode project: penyimpanan + pengambilan)

````markdown
## 4. Tracing Kode Step-by-Step

Misal user mengirim form tambah buku, lalu kode ini jalan:

```php
$data = bacaSemuaBuku();          // 1. ambil semua data dari file
$data[] = $bukuBaru;              // 2. tambahkan buku baru ke daftar
simpanSemuaBuku($data);           // 3. tulis ulang seluruh daftar ke file
```

**Langkah 1 — `bacaSemuaBuku()`**
PHP membuka file `data/buku.json`, membaca isinya, dan mengubah JSON menjadi array. Hasilnya misalnya array berisi 2 buku. Sekarang variabel `$data` berisi 2 buku.

**Langkah 2 — `$data[] = $bukuBaru;`**
PHP menambahkan `$bukuBaru` ke akhir array. Sekarang `$data` berisi 3 buku (2 lama + 1 baru).

**Langkah 3 — `simpanSemuaBuku($data)`**
PHP mengubah array 3 buku menjadi teks JSON, lalu menimpa isi file `data/buku.json` dengan teks itu.

| Waktu | Isi `$data` | Isi file `buku.json` |
|---|---|---|
| Sebelum | 2 buku | 2 buku |
| Setelah langkah 1 | 2 buku | 2 buku (belum berubah) |
| Setelah langkah 2 | 3 buku | 2 buku (belum berubah) |
| Setelah langkah 3 | 3 buku | **3 buku** |

**Hasil akhir:** file `buku.json` sekarang berisi 3 buku; saat halaman di-refresh, daftar menampilkan 3 buku.
````

---

## 4. Aturan Gaya Bahasa & Kode

- **Bahasa: Indonesia**, praktis dan jelas.
- Fokus **implementasi**, bukan teori panjang (teori ada di materi konsep).
- **Kode siap pakai**: bukan pseudo-code; harus bisa dijalankan di project nyata.
- **Konsisten** dengan aplikasi: nama class/variabel (`Buku`, `Anggota`, `$katalog`) nyambung antar-file.
- **Setiap materi menyentuh keamanan.**
- **Panjang wajar**: 80-200 baris per file.
- **Selalu ada contoh pemakaian**, bukan cuma definisi.

---

## 5. Template Lengkap (copy-paste)

````markdown
# N. <Judul> — <Subjudul>

> **Level N · <Nama Level>** — <tagline>

---

## 1. Penjelasan Singkat & Konsep Dasar
<konsep + istilah inti + Benang Merah>

## 2. Fungsi & Kegunaan Utama
- <manfaat 1>
- <manfaat 2>

## 3. Contoh Kode & Implementasi Real-World
```
<struktur folder>
```
```<bahasa>
<kode lengkap>
```
<contoh pemakaian>

## 4. Tracing Kode Step-by-Step
<telusur eksekusi + tabel + hasil akhir>

## 5. Analogi Sederhana
<analogi Sistem Administrasi Kantor>

## 6. Kapan Harus Digunakan & Kasus Penggunaan
- <kapan dipakai>
- <kasus nyata>

## 7. Aspek Keamanan & Best Practices (Critical)
- <risiko keamanan>
- **Best practices:**
  - <praktik 1>
  - <praktik 2>

## 8. Kesalahan Umum & Debugging
- **<pesan/gejala>** → <penyebab & solusi>

## 9. Soal Latihan & Kunci Jawaban

### Tantangan 1
<soal>

### Tantangan 2
<soal>

### Jawaban & Pembahasan
```<bahasa>
<solusi>
```
> **Pembahasan alur:** <penjelasan>

---
**Lanjut ke:** [[<materi berikutnya>]]
````

---

## 6. Checklist Kualitas Materi Project

- [ ] H1 **memakai nomor** (`# N. Judul`), sesuai nomor di roadmap project.
- [ ] Ada blockquote `> **Level N · <Nama Level>** — <tagline>`.
- [ ] Ada **semua** 9 bagian sesuai urutan.
- [ ] Ada bagian **Tracing Kode Step-by-Step** (bahasa sederhana, tanpa analogi).
- [ ] Kode **real-world & siap pakai** dengan konteks aplikasi (perpustakaan).
- [ ] Ada pembahasan **keamanan** (bagian 7).
- [ ] Ada **2 tantangan** + jawaban & pembahasan.
- [ ] Ada "Benang Merah" yang mengaitkan ke level sebelum/sesudah.
- [ ] Diakhiri `**Lanjut ke:** [[...]]`.

---

**File terkait:**
- `Format Pembuatan Project.md` — menyusun roadmap project.
- `Format Roadmap.md` — menyusun roadmap belajar.
- `Format Penjelasan Materi.md` — menulis materi konsep.
