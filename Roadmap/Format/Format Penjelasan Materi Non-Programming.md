# Format Penjelasan Materi Non-Programming — Ketentuan Penulisan Isi Materi Bidang di Luar Pemrograman

> **Dokumen ini adalah KETENTUAN untuk AI.** Dipakai saat menulis **satu file materi** yang menjadi isi dari satu poin roadmap **bidang non-pemrograman**.
> **Berlaku untuk:** materi bidang Bahasa, Filsafat, Psikologi, Sejarah, Matematika/Sains, Seni/PixelArt, Ekonomi, dll.
> **Pasangan:** cara menyusun roadmap-nya ada di `Format Roadmap Non-Programming.md`.
> **Untuk materi pemrograman → `Format Penjelasan Materi.md`. Untuk materi project → `Format Penjelasan Materi Project.md`.**

---

## 0. Tujuan & Kapan Dipakai

Setiap poin di roadmap (mis. `6. Fonem Bahasa Inggris — 44 Bunyi`) harus punya **satu file materi** yang menjelaskannya sampai paham.

File materi ditulis ketika:
- AI diminta "buatkan materi nomor N dari roadmap X", atau
- Melengkapi poin roadmap yang belum punya file.

**Contoh lokasi & nama file:**

```
Belajar/Bahasa/Bahasa Inggris/
├── Level 1 - Fondasi/
│   ├── A. Mindset & Strategi/
│   │   ├── 1. Mengapa Bahasa Inggris Penting — Motivasi Diri.md
│   │   └── 2. Mitos Belajar Bahasa Inggris — Yang Harus Dibuang.md
```

Pola path: `Belajar/<Bidang>/<Topik>/Level N - <Nama>/<Bagian>. <Nama Bagian>/<N. Judul — Subjudul>.md`

- **Nomor file** (`1.`, `2.`, …) = nomor global yang sama dengan nomor di roadmap.
- **Judul file** = sama dengan judul poin roadmap, memakai pemisah ` — ` (em dash).

---

## 1. Struktur Baku File Materi (urutan heading wajib)

Ikuti urutan ini **persis**, jangan diacak, jangan dihilangkan (boleh singkat bila tidak relevan, tapi jangan hapus heading-nya):

| Urutan | Heading | Isi singkat |
|---|---|---|
| 1 | `# <Judul Materi>` | Judul saja (tanpa nomor di dalam H1) |
| 2 | `## Penjelasan` | Apa itu, konsep inti, cara kerja |
| 3 | `## Latar & Konteks` | Sejarah singkat, tokoh, mengapa ini muncul |
| 4 | `## Istilah Kunci` | Daftar istilah penting + definisi singkat |
| 5 | `## Penerapan / Contoh` | Contoh nyata (kalimat, kasus, argumen, soal, karya) |
| 6 | `## Bedah Contoh (Step-by-Step)` | **Penelusuran langkah demi langkah** (lihat Bagian 3) |
| 7 | `## Analogi` | Satu analogi dunia nyata yang relevan |
| 8 | `## Dipakai untuk Apa` | Relevansi & penerapan di kehidupan/pekerjaan |
| 9 | `## Kesalahpahaman Umum` | Miskonsepsi + koreksinya |
| 10 | `## Hubungan dengan Materi Sebelumnya` | Kaitan ke materi sebelumnya & sesudahnya |
| 11 | `## Sumber & Rujukan` | Buku/artikel/jurnal kredibel |
| 12 | `## Soal Latihan & Jawaban` | `### Soal` + `### Jawaban` |

> **Nomor di dalam H1 tidak ditulis.** Nomor hanya ada di **nama file** dan di **link roadmap**.
> Contoh: file `6. Fonem Bahasa Inggris — 44 Bunyi.md` → H1-nya `# Fonem Bahasa Inggris — 44 Bunyi`.

---

## 2. Aturan Isi Tiap Bagian

### `## Penjelasan`
- Mulai dengan definisi 1-2 kalimat yang jelas.
- Lanjutkan konsep dan cara kerjanya.
- Gunakan bullet point untuk daftar aturan/fakta penting.

### `## Latar & Konteks`
- Sejarah singkat / asal-usul konsep, tokoh atau karya yang memperkenalkannya.
- Boleh singkat (2-4 kalimat) bila konsepnya teknis. Untuk Filsafat/Sejarah, bagian ini lebih panjang.

### `## Istilah Kunci`
- Daftar istilah penting yang dipakai di materi ini, format: **istilah** — definisi satu kalimat.
- Tujuannya: pembaca punya "kamus kecil" sebelum masuk contoh.

### `## Penerapan / Contoh`
- Beri **contoh konkret** yang realistis, bukan contoh kosong.
- Sesuaikan bentuk contoh dengan bidang:
  - **Bahasa**: kalimat/percakapan dengan penanda (bold/kurung) pada struktur yang dibahas.
  - **Filsafat**: teks/argumen pendek dari pemikir.
  - **Psikologi**: kasus/perilaku nyata.
  - **Matematika/Sains**: contoh soal + data.
  - **Sejarah**: peristiwa nyata + tanggal.
  - **Seni/PixelArt**: referensi karya/gambar + catatan proses.
- Pakai konteks yang **konsisten** di seluruh materi satu topik (lihat Bagian 4).

### `## Bedah Contoh (Step-by-Step)` (lihat Bagian 3)
- Telusuri contoh di atas langkah demi langkah, dengan bahasa sederhana.
- Sebut **mengapa** setiap langkah diambil, bukan hanya apa.

### `## Analogi`
- Satu analogi saja, yang benar-benar terkait. Hindari analogi berlapis yang menambah kebingungan.

### `## Dipakai untuk Apa`
- Bullet list manfaat/penerapan nyata (bukan teori).

### `## Kesalahpahaman Umum`
- 3-6 miskonsepsi yang paling sering, format: **Miskonsepsi** → kenyataannya + cara menghindari.

### `## Hubungan dengan Materi Sebelumnya`
- Sebutkan nomor & judul materi terkait, jelaskan benang merahnya.
- Contoh: "Setelah tahu konsep X (materi 3), sekarang kita lihat penerapannya (materi 6)."

### `## Sumber & Rujukan`
- Minimal 2-3 sumber kredibel. Format:
  `- **<Penulis>** — *<Judul>* (<tahun>). <catatan singkat relevansinya>`
- Untuk Filsafat/Sejarah/Psikologi, utamakan buku & jurnal; untuk Bahasa, boleh kamus/ korpus/kursus resmi.

### `## Soal Latihan & Jawaban`
- **`### Soal`**: satu soal kontekstual (bisa berupa pertanyaan esai, latihan kalimat, soal hitung, atau latihan penerapan).
- **`### Jawaban`**: jawaban lengkap + penjelasan singkat mengapa demikian.

---

## 3. Bagian Baru: `## Bedah Contoh (Step-by-Step)`

Ini **pengganti "Tracing Code"** pada bidang pemrograman. Tujuannya: pembaca melihat **bagaimana sebuah contoh diproses langkah demi langkah** — kalimat diurai, argumen diuji, kasus dianalisis, soal dikerjakan.

### Aturan penulisan bedah contoh

1. **Bahasa sederhana, bukan teknis.** Tulis seperti menjelaskan ke teman.
2. **Jangan pakai analogi di bagian ini.** Analogi sudah ada di bagian `Analogi`. Di sini murni alur/penalaran.
3. **Telusuri langkah demi langkah** sesuai urutan logis (bukan urutan hafalan).
4. **Sebutkan alasan tiap langkah** — kenapa langkah ini diambil, apa yang dilihat.
5. **Untuk proses panjang/berulang**, gunakan **tabel langkah** agar jelas.
6. **Akhiri dengan kesimpulan/hasil** yang persis.

### Kerangka bedah contoh

```markdown
## Bedah Contoh (Step-by-Step)

Kita bedah contoh di atas langkah demi langkah.

**Langkah 1 — <apa yang dilihat/dilakukan>**
<penjelasan sederhana + mengapa langkah ini diambil>

**Langkah 2 — <...>**
<...>

| Langkah | Yang diamati | Alasan/kesimpulan |
|---|---|---|
| 1 | <...> | <...> |
| 2 | <...> | <...> |

**Kesimpulan:**
<hasil akhir yang jelas>
```

### Contoh penerapan per bidang
- **Bahasa:** uraikan satu kalimat — tentukan subjek, predikat, objek, tense, dan mengapa bentuk itu dipakai.
- **Filsafat:** bedah satu argumen — pisahkan premis 1, premis 2, kesimpulan; uji apakah valid dan apakah premisnya benar.
- **Psikologi:** bedah satu perilaku/kasus — pemicu → proses mental → respons → konsekuensi.
- **Matematika/Sains:** bedah satu soal — apa yang diketahui, rumus/konsep yang dipakai, tiap langkah hitung, hasil.
- **Sejarah:** bedah satu peristiwa — sebab jauh → sebab dekat → peristiwa → akibat.
- **Seni/PixelArt:** bedah satu karya — konsep → sketsa → warna/komposisi → detail akhir.

---

## 4. Aturan Analogi & Konteks

- Analogi wajib **relevan dan konsisten** sepanjang satu roadmap.
- Pilih **satu tema konteks** per topik dan pakai terus. Contoh:
  - Roadmap Bahasa Inggris → konteks **kehidupan sehari-hari** (rutinitas, pekerjaan, perjalanan).
  - Roadmap Filsafat → konteks **pergolakan batin & kehidupan publik**.
  - Roadmap Psikologi → konteks **hubungan sosial & pengambilan keputusan**.
- Satu analogi per materi. Hindari analogi teknis-ke-teknis yang menambah istilah baru.
- Jika konsep sulit dianalogikan, boleh 1-2 kalimat singkat; jangan dipaksakan sampai membingungkan.

---

## 5. Aturan Gaya Bahasa

- **Bahasa: Indonesia**, santai tapi rapi (boleh "kamu"), tidak kaku, tidak lebay.
- **Istilah asing/teknis boleh dipakai**, tapi beri arti singkat saat pertama muncul (dan masukkan ke `## Istilah Kunci`).
- **Jangan menumpuk jargon** dalam satu kalimat. Satu ide per kalimat.
- **Netral & berbasis sumber.** Hindari klaim mutlak tanpa rujukan, terutama di Filsafat/Sejarah/Psikologi.
- **Panjang wajar**: 50-120 baris per file. Jangan terlalu pendek (tidak lengkap) atau terlalu panjang (membosankan).

---

## 6. Template Lengkap (copy-paste)

```markdown
# <Judul Materi — Subjudul>

## Penjelasan
<definisi + konsep + cara kerja + bullet aturan>

## Latar & Konteks
<sejarah singkat / tokoh / asal-usul>

## Istilah Kunci
- **<istilah>** — <definisi singkat>
- **<istilah>** — <definisi singkat>

## Penerapan / Contoh
<contoh konkret sesuai bidang>

## Bedah Contoh (Step-by-Step)
<uraikan contoh baris demi baris, sebut alasan tiap langkah, tabel bila perlu, kesimpulan>

## Analogi
<analogi dunia nyata yang relevan>

## Dipakai untuk Apa
- <manfaat 1>
- <manfaat 2>

## Kesalahpahaman Umum
- **<Miskonsepsi>**: <kenyataan + cara menghindari>
- **<Miskonsepsi>**: <kenyataan + cara menghindari>

## Hubungan dengan Materi Sebelumnya
<materi sebelumnya → materi ini → materi selanjutnya>

## Sumber & Rujukan
- **<Penulis>** — *<Judul>* (<tahun>). <catatan>
- **<Penulis>** — *<Judul>* (<tahun>). <catatan>

## Soal Latihan & Jawaban
### Soal
<soal kontekstual>

### Jawaban
<jawaban + penjelasan singkat>
```

---

## 7. Checklist Kualitas Materi

- [ ] H1 = judul tanpa nomor, sama dengan judul di roadmap.
- [ ] Ada **semua** 12 bagian sesuai urutan.
- [ ] Ada bagian **Bedah Contoh (Step-by-Step)** dengan bahasa sederhana (tanpa analogi).
- [ ] Ada **Istilah Kunci** yang membantu sebelum masuk contoh.
- [ ] Contoh konkret & konsisten dengan konteks topik.
- [ ] Ada minimal 3 **Kesalahpahaman Umum**.
- [ ] Ada minimal 2 **Sumber & Rujukan** kredibel.
- [ ] Ada `### Soal` dan `### Jawaban`.
- [ ] Bagian "Hubungan dengan Materi Sebelumnya" menyebut nomor/judul materi terkait.
- [ ] Tidak ada istilah teknis yang muncul mendadak tanpa penjelasan.

---

**File terkait:**
- `Format Roadmap Non-Programming.md` — induk yang menomori poin & menautkan ke file ini.
- `Format Penjelasan Materi.md` — format materi untuk pemrograman.
- `Format Penjelasan Materi Project.md` — format materi untuk project terapan.
