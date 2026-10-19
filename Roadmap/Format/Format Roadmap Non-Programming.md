# Format Roadmap Non-Programming — Ketentuan Penyusunan Roadmap Bidang di Luar Pemrograman

> **Dokumen ini adalah KETENTUAN untuk AI.** Dipakai saat menyusun roadmap untuk **bidang non-pemrograman**.
> **Hasil akhir:** satu file `Roadmap/<Kategori>/Roadmap <Bidang>.md`.
> **Struktur umum** (Filosofi, Prinsip Desain, level, benang merah, checkpoint) **mengacu ke `Format Roadmap.md`** — dokumen ini hanya mengatur **perbedaan untuk bidang non-pemrograman**.
>
> **Cakupan:** Bahasa (Inggris, Sunda), Filsafat, Psikologi, Sejarah, Matematika/Sains, Seni/PixelArt, Ekonomi/Bisnis, dan bidang ilmu lain.
> **Untuk bahasa pemrograman → `Format Roadmap.md`. Untuk framework → `Format Roadmap Framework.md`. Untuk tool IT → `Format Roadmap Tool & Teknologi.md`.**

---

## 0. Tujuan & Kapan Dipakai

Dokumen ini dipakai saat AI diminta **membuat roadmap baru** untuk satu bidang **di luar pemrograman**.

Tujuan roadmap:
1. Menjadi **peta belajar** yang urut dan tidak membingungkan.
2. Memastikan **pilar inti bidang** (Bagian 3) tercakup — bukan "21 fundamental bahasa".
3. Memastikan **kekhasan bidang** ikut dipelajari (mis. pelafalan untuk Bahasa, aliran pemikiran untuk Filsafat, pendekatan untuk Psikologi).
4. Menjadi **induk** dari file materi (lihat `Format Penjelasan Materi Non-Programming.md`).

> **Aturan penempatan file:** roadmap disimpan di folder kategori yang sesuai. Contoh:
> - `Roadmap/Bahasa/Roadmap Bahasa Inggris.md`
> - `Roadmap/Psikologi/Roadmap Psikologi Umum.md`
> - `Roadmap/Lainnya/Roadmap Learn To Learn.md`

---

## 1. Prinsip Wajib (tidak boleh dilanggar)

Sebuah roadmap bidang non-pemrograman HARUS memenuhi prinsip berikut:

| #   | Prinsip                          | Artinya                                                                                                                       |
| --- | -------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 1   | **Terstruktur**                  | Ada level/fase yang jelas, tiap level punya tema, bagian, dan checkpoint.                                                     |
| 2   | **Menyeluruh**                   | Menyentuh **pilar inti bidang** (Bagian 3) + kekhasan bidang (Bagian 4).                                                      |
| 3   | **Benang Merah**                 | Setiap bagian menjelaskan kaitannya dengan bagian sebelum & sesudahnya.                                                       |
| 4   | **Urut secara Dependency**       | Materi A dikuasai sebelum B. Tidak ada materi yang butuh konsep yang belum diajarkan.                                         |
| 5   | **Satu Artefak, Tumbuh Bersama** | Semua level membangun SATU artefak yang sama, dari sederhana ke kompleks (mis. satu jurnal filsafat, satu portofolio bahasa). |
| 6   | **Mengapa sebelum Bagaimana**    | Setiap bagian diawali alasan "mengapa ini penting" dan "mengapa sekarang".                                                    |
| 7   | **Output Nyata**                 | Tiap level punya artefak/hasil yang bisa dilihat + checklist pemahaman.                                                       |
| 8   | **Aktif, Bukan Hafalan**         | Belajar lewat praktik & mengingat kembali (retrieval), menjelaskan ulang (Feynman), refleksi — bukan sekadar membaca.         |
| 9   | **Sumber Terpercaya**            | Setiap materi merujuk buku/artikel/jurnal kredibel, bukan opini tanpa dasar.                                                  |

> **Perbedaan utama dari roadmap bahasa pemrograman:**
> - Tidak ada "21 materi fundamental". Diganti **pilar inti bidang** yang bisa berbeda tiap bidang.
> - "Satu project/aplikasi jadi" diganti **"satu artefak belajar tumbuh bersama"** (jurnal, esai, portofolio, peta konsep).

---

## 2. Struktur Baku File Roadmap Non-Programming

Ikuti urutan heading berikut **persis**. Struktur sama dengan `Format Roadmap.md` Bagian 2; perbedaannya ditandai **(delta)**.

### 2.1 Kepala file

```markdown
# 🗺️ Roadmap <Bidang>: <Subjudul Menarik>

## Filosofi Roadmap Ini

> **"<kutipan singkat yang menjelaskan sudut pandang/urgensi bidang>"** — satu paragraf alasan.

### Prinsip Desain

- **Satu Artefak, Tumbuh Bersama**: <artefak apa yang dibangun, dari X → Y → Z>
- **Aktif, Bukan Hafalan**: <praktik apa yang dilakukan tiap level>
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **<Prinsip 4>**: <penjelasan singkat>

### Prasyarat Sebelum Memulai

```
Sebelum roadmap ini, sebaiknya sudah:
├── <prasyarat 1>
├── <prasyarat 2>
└── <prasyarat n>
```

### Spesifikasi Artefak  ← (delta) pengganti "Spesifikasi Aplikasi"

| Aspek | Keterangan |
|---|---|
| Nama Artefak | <mis. "Jurnal Filsafat Pribadi", "Portofolio Bahasa Inggris"> |
| Bentuk | <esai / jurnal / peta konsep / portofolio / karya> |
| Tujuan | <1-2 kalimat> |
| Yang Terkumpul | <daftar isi artefak> |
| Target Akhir | <bentuk final artefak> |
```

### 2.2 Gambaran besar

```markdown
## 📋 Gambaran Besar — Apa yang Akan Dibangun/Dikuasai

```
Level 1: "<nama>" — <materi inti ringkas>
    ↓ (bertambah)
Level 2: + <tambahan> → <hasil>
    ↓ (bertambah)
...
Level N: + <tambahan> → <hasil akhir>
```
```

### 2.3 Tiap level

```markdown
## 🟢 LEVEL N: <TEMA> (Minggu a-b / Bulan a-b)

> **Tema**: _<satu kalimat tujuan level>_
> **Benang Merah**: <Bagian sebelumnya> → <bagian ini> → <bagian sesudahnya>
> **Output**: <artefak/hasil nyata setelah level ini>
> **Praktik & Artefak**: <praktik utama & bagian artefak yang diselesaikan>  ← (delta)

---

### A. <Nama Bagian>

> 💡 **Mengapa dimulai di sini?** <alasan, kaitkan dengan materi sebelum/sesudah>

```
Benang Merah Bagian A:
<poin sebelumnya> →
<konsep bagian ini> →
<hasil/poin berikutnya>
```

1. `[[<Nomor>. <Judul Materi>]]`

    - <poin penjelasan>
    - **Istilah kunci**: <istilah yang dikenalkan di materi ini>  ← (delta)
    - _Latihan/praktik_: <aksi nyata yang harus dilakukan pembaca>  ← (delta)
    - _Sumber_: <buku/artikel/jurnal rujukan>  ← (delta)

2. `[[<Nomor>. <Judul Materi>]]`
    ...
```

> Emoji level: 🟢 Level dasar, 🔵 Level menengah, 🟡/🟠/🔴 Level lanjut, 🟣/⚫ spesialisasi. Konsisten antar-file.

### 2.4 Checkpoint tiap level

```markdown
### 🏗️ Checkpoint Level N

```
✅ Checklist sebelum lanjut ke Level N+1:

PEMAHAMAN:
├── Bisa jelaskan <konsep 1>
├── Bisa jelaskan <konsep 2>
└── Bisa jelaskan <konsep 3>

PRAKTIK / ARTEFAK:
├── <bagian artefak yang harus selesai>
└── <praktik/latihan yang harus dijalankan>

REFLEKSI:  ← (delta)
├── <tulis 1 paragraf refleksi: apa yang paling sulit & mengapa>
└── <hubungkan materi ini dengan cara berpikir/kehidupanmu>

SUMBER:  ← (delta)
├── <buku/artikel yang sudah dibaca & dicatat>
└── <catatan/ringkasan yang sudah dibuat>
```
```

> **(delta)** Bagian "Artefak Level" menggantikan "Proyek Level" pada format bahasa:
>
> ```markdown
> ### 🏗️ Artefak Level N
>
> ```
> ARTEFAK : "<nama>"
> ───────────────────
> Komponen :
> ├── <...>
> └── <...>
>
> Konsep yang dilatih :
> <...>
> ```
> ```

---

## 3. Pilar Inti Bidang Non-Programming (WAJIB tercakup)

Ini pengganti "21 materi fundamental". Karena tiap bidang berbeda, pilar di bawah bersifat **umum** dan **wajib disesuaikan**. Tidak semua pilar harus ada — pilih yang relevan, dan jelaskan yang tidak berlaku.

| No | Pilar | Yang harus dibahas |
|---|---|---|
| 1 | **Terminologi & Konsep Dasar** | Istilah kunci bidang, definisi, konsep fondasi. |
| 2 | **Sejarah & Tokoh Kunci** | Asal-usul bidang, tokoh penting, karya besar. |
| 3 | **Aliran / Teori Utama** | Mazhab/teori yang berbeda, perdebatan antar-aliran. |
| 4 | **Metode & Cara Kerja** | Bagaimana bidang ini bekerja: metode analisis, pendekatan, prosedur. |
| 5 | **Keterampilan Inti** | Skill yang harus terlatih (mis. menulis, analisis, observasi, pelafalan). |
| 6 | **Contoh & Studi Kasus** | Kasus nyata untuk menerapkan konsep. |
| 7 | **Keterampilan Pendukung** | Bahasa asing, statistika, logika, literasi digital, dsb. |
| 8 | **Penerapan Nyata** | Bagaimana diterapkan di kehidupan/pekerjaan. |
| 9 | **Etika & Isu Kontemporer** | Perdebatan etis, perkembangan terkini, kontroversi. |
| 10 | **Sumber & Literasi** | Buku, jurnal, komunitas, cara menilai sumber. |
| 11 | **Komunikasi & Berbagi** | Menulis/ menjelaskan/mengajar ulang apa yang dipelajari. |
| 12 | **Refleksi & Metakognisi** | Menilai pemahaman sendiri, memperbaiki cara belajar. |

---

## 4. Contoh Penyesuaian Pilar per Bidang

| Bidang | Pilar yang ditonjolkan (contoh) |
|---|---|
| **Bahasa** (Inggris, Sunda) | Bunyi/fonologi, tata bahasa, kosakata, 4 skills (listening/speaking/reading/writing), budaya, register (formal/informal). |
| **Filsafat** | Argumen & logika, epistemologi/metafisika/etika, tokoh & aliran, membaca teks asli, menulis esai argumentatif. |
| **Psikologi** | Teori kepribadian, kognitif, sosial, perkembangan, psikopatologi, metode riset & etika, terapi, observasi perilaku. |
| **Sejarah** | Kronologi, sebab-akibat, sumber primer/sekunder, historiografi, analisis bias, narasi. |
| **Matematika/Sains** | Aksioma & definisi, pembuktian, konsep (aljabar, kalkulus, dsb.), pemodelan, pemecahan masalah, metode ilmiah. |
| **Seni / PixelArt** | Dasar visual (garis, warna, bentuk), komposisi, teori warna, teknik media, sejarah seni, kritik, berkarya. |
| **Ekonomi/Bisnis** | Mikro/makro, akuntansi dasar, strategi, pemasaran, keuangan, studi kasus perusahaan, etika bisnis. |

> Bidang baru cukup memilih pilar dari Bagian 3 dan menyesuaikan jumlah/namanya.

---

## 5. Output / Artefak Belajar (pengganti "aplikasi")

Bentuk artefak yang tumbuh bersama sepanjang roadmap (pilih 1 sebagai artefak utama):

| Artefak | Cocok untuk | Isi |
|---|---|---|
| **Jurnal belajar/refleksi** | semua bidang | catatan harian, refleksi, pertanyaan |
| **Peta konsep (concept map)** | Filsafat, Sains, Sejarah | hubungan antar-konsep |
| **Esai/artikel** | Filsafat, Sejarah, Sastra | argumen & analisis tertulis |
| **Glosarium** | Bahasa, Teknik | daftar istilah + definisi sendiri |
| **Portofolio karya** | Seni/PixelArt, Desain | kumpulan karya terbaik + catatan proses |
| **Rekaman audio/percakapan** | Bahasa | bukti listening/speaking |
| **Ditembok/catatan Feynman** | semua bidang | penjelasan ulang dengan bahasa sendiri |
| **Proyek terapan kecil** | Psikologi, Ekonomi | observasi/analisis kasus nyata |

> Artefak harus **berkembang tiap level**, bukan dikumpulkan sekaligus di akhir.

---

## 6. Aturan Penomoran, Link, dan Konsistensi

1. **Penomoran materi** berurutan global (1, 2, 3, …) lintas level — bukan reset tiap level. Ini nomor yang dipakai untuk menautkan ke file materi.
2. **Judul materi** seragam: `<Nomor>. <Judul> — <Subjudul Penjelas>`.
   - Contoh: `6. Fonem Bahasa Inggris — 44 Bunyi yang Perlu Dikuasai`.
3. **Link ke materi** memakai wiki-link: `` `[[6. Fonem Bahasa Inggris — 44 Bunyi yang Perlu Dikuasai]]` `` (tanpa path, aman karena nama unik).
4. **Setiap nomor di roadmap harus punya file materinya** (dibuat memakai `Format Penjelasan Materi Non-Programming.md`). Jika belum ada, tandai eksplisit "belum dibuat".
5. **Nama level** seragam: `Level N - <Nama>`; bagian: `A.`, `B.`, `C.`, …
6. **Konsisten antar-roadmap**: struktur, emoji, dan istilah (Tema/Benang Merah/Output/Praktik/Checkpoint) selalu sama.
7. **Sertakan sumber** pada tiap poin (buku/artikel/jurnal) — ini pembeda utama dari roadmap pemrograman.

---

## 7. Level, Durasi, dan Output

- Setiap level punya **estimasi waktu** (mis. `(Minggu 1-4)` atau `(Bulan 1-2)`).
- Jumlah level wajar: 5-7 level. Lebih dari itu, pecah jadi beberapa roadmap atau tambahkan "Jalur Spesialisasi".
- Setiap level punya **Output** nyata (bagian artefak yang selesai), bukan sekadar "paham teori".
- Setiap level diakhiri **Checkpoint** dengan 4 blok: PEMAHAMAN, PRAKTIK/ARTEFAK, REFLEKSI, SUMBER.

---

## 8. Template Roadmap Non-Programming (copy-paste)

```markdown
# 🗺️ Roadmap <Bidang>: <Subjudul>

## Filosofi Roadmap Ini

> **"<kutipan>"** — <alasan singkat>

### Prinsip Desain

- **Satu Artefak, Tumbuh Bersama**: <deskripsi>
- **Aktif, Bukan Hafalan**: <deskripsi>
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **<Prinsip 4>**: <deskripsi>

### Prasyarat Sebelum Memulai

```
Sebelum roadmap ini, sebaiknya sudah:
├── <prasyarat>
```

### Spesifikasi Artefak

| Aspek | Keterangan |
|---|---|
| Nama Artefak | <...> |
| Bentuk | <...> |
| Tujuan | <...> |
| Yang Terkumpul | <...> |
| Target Akhir | <...> |

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun/Dikuasai

```
Level 1: ...
    ↓ (bertambah)
Level N: ...
```

---

## 🟢 LEVEL 1: <TEMA> (Minggu 1-x)

> **Tema**: _<...>_
> **Benang Merah**: <...>
> **Output**: <...>
> **Praktik & Artefak**: <...>

---

### A. <Bagian>

> 💡 **Mengapa dimulai di sini?** <...>

```
Benang Merah Bagian A:
<...> → <...> → <...>
```

1. `[[1. <Judul> — <Subjudul>]]`

    - <poin>
    - **Istilah kunci**: <...>
    - _Latihan/praktik_: <aksi>
    - _Sumber_: <buku/artikel>

---

### 🏗️ Artefak Level 1

```
ARTEFAK : "<nama>"
─────────────────
Komponen :
├── <...>
└── <...>

Konsep yang dilatih :
<...>
```

---

### 🏗️ Checkpoint Level 1

```
✅ Checklist sebelum lanjut ke Level 2:

PEMAHAMAN:
├── <...>

PRAKTIK / ARTEFAK:
├── <...>

REFLEKSI:
├── <...>

SUMBER:
├── <...>
```
```

---

## 9. Checklist Kualitas Roadmap Non-Programming

- [ ] Ada **Filosofi**, **Prinsip Desain**, dan **Prasyarat**.
- [ ] Ada **Spesifikasi Artefak** yang akan dibangun.
- [ ] Ada **Gambaran Besar** (alur level).
- [ ] Semua level punya **Tema**, **Benang Merah**, **Output**, **Praktik & Artefak**.
- [ ] **Pilar inti bidang** (Bagian 3) tercakup atau dijelaskan kenapa tidak berlaku.
- [ ] Ada **penyesuaian khas bidang** (Bagian 4).
- [ ] Setiap poin punya **istilah kunci**, **latihan/praktik**, dan **sumber**.
- [ ] Setiap level punya **Checkpoint** (pemahaman + praktik/artefak + refleksi + sumber).
- [ ] Ada **Artefak Level** yang tumbuh dari level pertama, bukan dikumpulkan di akhir.
- [ ] Penomoran berurutan global & setiap nomor punya link `[[...]]`.

---

**File terkait:**
- `Format Roadmap.md` — struktur umum & roadmap bahasa pemrograman.
- `Format Roadmap Framework.md` — roadmap framework.
- `Format Roadmap Tool & Teknologi.md` — roadmap tool/teknologi non-bahasa.
- `Format Penjelasan Materi Non-Programming.md` — cara menulis isi tiap poin roadmap non-pemrograman.
- `Format Pembuatan Project.md` — cara menyusun roadmap project (untuk bidang yang punya proyek terapan).
