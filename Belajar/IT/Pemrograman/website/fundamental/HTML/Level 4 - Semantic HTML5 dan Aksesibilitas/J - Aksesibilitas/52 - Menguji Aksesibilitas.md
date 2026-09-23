# Menguji Aksesibilitas

## Penjelasan
Menguji aksesibilitas adalah proses memeriksa apakah suatu halaman web dapat digunakan oleh semua orang, termasuk pengguna dengan disabilitas. Pengujian dilakukan dengan kombinasi alat otomatis (tools) dan pengujian manual. Tidak ada satu alat pun yang bisa menemukan semua masalah — pendekatan *multi-tool* sangat penting.

## Fungsi
- **Lighthouse**: Alat otomatis dari Google untuk mengaudit performa, aksesibilitas, SEO, dan *best practices* — langsung di Chrome DevTools.
- **axe DevTools**: Ekstensi browser (Chrome/Firefox) untuk audit aksesibilitas mendalam dengan aturan deteksi yang lebih banyak dari Lighthouse.
- **Tab test**: Pengujian manual navigasi keyboard — Tab untuk maju, Shift+Tab untuk mundur.
- **Screen reader**: Pengujian manual menggunakan *screen reader* (NVDA, JAWS, VoiceOver, TalkBack) untuk mendengar bagaimana konten dibacakan.

## Cara Pengimplementasian

### 1. Lighthouse (Chrome DevTools)
```text
Langkah:
1. Buka halaman di Chrome
2. Klik kanan → Inspect → tab Lighthouse
3. Pilih "Accessibility" (matikan lainnya untuk hasil cepat)
4. Pilih mode "Navigation" (default)
5. Klik "Generate report"
6. Lihat skor (target: 90+) dan pelajari setiap "Failed audits"
```

### 2. axe DevTools
```text
Langkah:
1. Install ekstensi axe DevTools dari Chrome Web Store
2. Buka halaman, klik icon axe di toolbar
3. Klik "Analyze all violations"
4. Hasil dikelompokkan:
   - Violations (wajib diperbaiki)
   - Passes (sudah lulus)
   - Incomplete (butuh verifikasi manual)
   - Inapplicable (tidak relevan)
```

### 3. Tab Test (Navigasi Keyboard)
```text
Langkah:
1. Klik address bar (fokus ada di address bar)
2. Tekan Tab → lihat apakah ada indikator fokus visual
3. Lanjutkan Tab melewati semua elemen interaktif
4. Periksa:
   - Apakah skip link muncul pertama kali?
   - Apakah semua elemen interaktif terjangkau?
   - Apakah urutan fokus logis (kiri-ke-kanan, atas-ke-bawah)?
   - Apakah ada elemen yang "hilang" (tidak bisa di-Tab)?
   - Apakah ada *focus trap* yang tidak semestinya?
5. Shift+Tab untuk mundur
6. Uji Enter dan Spasi pada tombol/link
7. Uji panah pada komponen seperti select, radio, slider
```

### 4. Screen Reader Test
```text
Langkah (dengan NVDA - gratis di Windows):
1. Install NVDA (nvaccess.org)
2. Buka halaman
3. Mulai NVDA (Ctrl+Alt+N)
4. Navigasi dengan:
   - Arrow Up/Down: baca baris per baris
   - H: navigasi antar heading
   - D: navigasi antar landmark
   - Tab: navigasi antar elemen interaktif
   - Insert+Down: baca dari posisi kursor
5. Perhatikan apakah:
   - Heading dibacakan dengan levelnya
   - Gambar dengan alt="" dilewati dengan benar
   - Tombol dibacakan fungsinya
   - Error form diumumkan
   - Konten dinamis diumumkan
```

### Checklist Pengujian Aksesibilitas
```markdown
# Automated (Tools)
- [ ] Lighthouse — skor aksesibilitas ≥ 90
- [ ] axe DevTools — tidak ada violation
- [ ] HTML Validator — tidak ada error markup

# Keyboard (Tab Test)
- [ ] Skip link tersedia dan berfungsi
- [ ] Indikator fokus visual terlihat jelas
- [ ] Urutan fokus logis
- [ ] Semua elemen interaktif terjangkau
- [ ] Tidak ada focus trap
- [ ] Modal: fokus terkunci di dalam, kembali setelah ditutup

# Screen Reader
- [ ] Heading: struktur logis, level benar
- [ ] Gambar: alt informatif, alt="" untuk dekoratif
- [ ] Form: label terhubung dengan input
- [ ] Error: diumumkan (aria-live atau focus management)
- [ ] Landmark: nav, main, aside terbaca
- [ ] Konten dinamis: diumumkan dengan role="alert" atau aria-live

# Kontras & Visual
- [ ] Rasio kontras ≥ 4.5:1 (teks normal)
- [ ] Teks bisa diperbesar 200% tanpa terpotong
- [ ] Elemen non-teks (ikon) punya label
```

## Analogi (tema RUMAH/BANGUNAN)
Menguji aksesibilitas seperti **inspeksi bangunan sebelum dihuni**.

- **Lighthouse** = **Tim inspektur cepat**. Mereka datang, melakukan pengecekan standar dalam 30 menit: apakah ada pintu darurat? Apakah lebar lorong sesuai standar? Mereka memberi laporan awal dengan skor.

- **axe DevTools** = **Inspektur detail**. Mereka memeriksa setiap sudut: apakah gagang pintu sesuai aturan? Apakah ram dipasang dengan benar? Apakah tanda darurat bercahaya? Mereka menemukan hal-hal yang terlewat oleh inspektur cepat.

- **Tab Test** = **Menyusuri seluruh bangunan hanya dengan senter**. Anda matikan semua lampu dan berjalan dengan senter (keyboard) melewati setiap ruangan, lorong, pintu. Apakah Anda bisa mencapai semua ruangan? Apakah ada pintu yang macet? Apakah ada tangga tanpa pegangan?

- **Screen Reader Test** = **Menutup mata dan berjalan dengan tongkat**. Anda tutup mata dan berjalan dengan tongkat (screen reader) — apakah lorong terdengar jelas? Apakah ada meja yang menghalangi? Apakah ada papan informasi yang bisa diraba?

Tidak cukup hanya dengan satu metode. Rumah yang aman perlu ketiganya: inspeksi cepat (Lighthouse), inspeksi detail (axe), simulasi berjalan dalam gelap (Tab test), dan simulasi berjalan dengan mata tertutup (screen reader).

## Dipakai Untuk
- Setiap halaman web sebelum rilis.
- *Pull request* — setiap perubahan fitur baru harus lulus audit aksesibilitas.
- Audit berkala (bulanan/quartalan) untuk menjaga kualitas.
- Pengecekan regresi setelah perubahan CSS/JS besar.
- Pengajuan sertifikasi aksesibilitas (misal WCAG 2.1 AA).

## Kesalahan Umum
1. **Hanya mengandalkan alat otomatis**: Lighthouse dan axe tidak bisa mendeteksi masalah konteks (misal label tidak deskriptif, urutan fokus yang tidak logis).
2. **Mengabaikan peringatan "Passes"**: Alat otomatis bisa *false positive* — periksa manual.
3. **Tidak menguji dengan screen reader sungguhan**: Simulasi alat tidak cukup — NVDA atau JAWS memberikan gambaran nyata.
4. **Tab test hanya dengan Tab, lupa Shift+Tab**: Mundur juga penting untuk deteksi *focus trap*.
5. **Tidak menguji di berbagai browser**: Tiap browser menangani fokus dan ARIA sedikit berbeda.
6. **Memperbaiki skor Lighthouse tanpa memahami masalah**: Misal nambah `aria-label` asal-asalan hanya untuk lulus audit.
7. **Skip link hanya ada di laporan tapi tidak berfungsi**: Link menuju ID yang tidak ada atau tidak *focusable*.

## Koneksi dengan Materi Sebelumnya
- **ARIA** (49 - ARIA.md): Alat otomatis mengecek penggunaan ARIA yang benar — apakah `aria-label` ada tapi elemen sudah punya label native?
- **aria-label/labelledby/describedby** (50): Alat seperti axe akan memeriksa apakah label aksesibel sudah diisi dengan benar.
- **Navigasi Keyboard** (51): Tab test adalah implementasi langsung dari prinsip tabindex dan skip link.
- **Semantic HTML** (Level 4): Lighthouse dan axe memberikan saran mengganti `<div class="nav">` dengan `<nav>`.

## Soal Latihan

1. **Mengapa kita tidak boleh hanya mengandalkan alat otomatis (Lighthouse/axe) untuk menguji aksesibilitas? Berikan dua contoh masalah yang hanya bisa dideteksi manual.**

<details><summary>Jawaban</summary>
Alat otomatis tidak bisa menilai konteks dan pengalaman pengguna. Contoh masalah yang hanya terdeteksi manual:
1. **Label tidak deskriptif**: Alat melihat `<button>✕</button>` dengan `aria-label="Tutup"` — lolos otomatis. Tapi manual test mendeteksi bahwa tombol tidak punya indikator fokus visual.
2. **Urutan fokus tidak logis**: DOM mungkin urutannya benar, tapi setelah CSS *flexbox/grid*, urutan visual berbeda dari urutan DOM. Alat otomatis tidak tahu ini; tab test manual menemukannya.
</details>

2. **Sebutkan tiga hal yang harus diperiksa saat melakukan Tab Test.**

<details><summary>Jawaban</summary>
1. Apakah skip link muncul sebagai elemen pertama yang bisa di-Tab?
2. Apakah semua elemen interaktif (tombol, link, input) terjangkau dengan Tab?
3. Apakah indikator fokus visual terlihat jelas saat elemen difokuskan?
</details>

3. **Apa perbedaan utama antara pengujian dengan Lighthouse dan axe DevTools?**

<details><summary>Jawaban</summary>
Lighthouse adalah alat audit bawaan Chrome yang mencakup performa, SEO, *best practices*, dan aksesibilitas — auditnya lebih umum. axe DevTools adalah ekstensi khusus aksesibilitas dengan aturan deteksi lebih banyak (+50% lebih banyak dari Lighthouse), kategori hasil (violations/passes/incomplete/inapplicable), dan rekomendasi perbaikan yang lebih detail.
</details>

4. **Jelaskan langkah-langkah melakukan screen reader test menggunakan NVDA untuk memeriksa struktur heading halaman.**

<details><summary>Jawaban</summary>
1. Buka halaman di Chrome/Firefox.
2. Aktifkan NVDA (Ctrl+Alt+N).
3. Tekan tombol `H` berulang kali untuk melompat antar heading.
4. Perhatikan: apakah urutan heading logis (tidak lompat dari h1 ke h3)? Apakah teks heading deskriptif? Apakah ada konten yang seharusnya jadi heading tapi terlewat?
5. Gunakan Insert+Down untuk membaca konten dari posisi heading.
</details>

5. **Buat sebuah daftar periksa (checklist) minimum untuk pengujian aksesibilitas yang bisa digunakan sebelum *deploy* halaman baru.**

<details><summary>Jawaban</summary>
```markdown
## Checklist Aksesibilitas Minimum
- [ ] Lighthouse Accessibility score ≥ 90
- [ ] axe DevTools: 0 violations
- [ ] Skip link berfungsi — tekan Tab pertama langsung muncul
- [ ] Semua elemen interaktif bisa dijangkau Tab
- [ ] Indikator fokus terlihat jelas (outline/ring)
- [ ] Heading: satu h1, urutan logis (h1 → h2 → h3)
- [ ] Semua gambar punya alt text (alt="" untuk dekoratif)
- [ ] Form: setiap input punya label
- [ ] Error form diumumkan ke screen reader
- [ ] Kontras teks ≥ 4.5:1 untuk teks normal
```
</details>
