# 149 — Menulis Dokumentasi Teknis

## 1. Penjelasan
Dokumentasi teknis adalah jembatan antara kode dan manusia. Tanpa dokumentasi, kode hanya bisa dipahami oleh penulisnya — itupun jika masih ingat. Dokumentasi mencakup README, ADR (Architecture Decision Record), wiki/Notion internal, dan docstring. Setiap jenis punya audiens berbeda: user (ingin tahu cara pakai), developer (ingin tahu cara kerja & kontribusi), maintainer (ingin tahu alasan keputusan arsitektur).

## 2. Fungsi
- Mengurangi ketergantungan pada author asli
- Mempercepat onboarding anggota baru
- Menjadi referensi saat debugging atau review
- Mendokumentasikan keputusan arsitektur (ADR) agar tidak diulang-ulang

## 3. Tips / Cara
- **README wajib**: What (nama + deskripsi 1 kalimat), Why (masalah apa yang diselesaikan), How (instalasi & penggunaan), API reference, contoh.
- **Gunakan template**: Buka github.com/othneildrew/Best-README-Template.
- **Tambah badges**: build status, coverage, version — dari shields.io.
- **ADR singkat saja**: Context → Decision → Consequences. Simpan di folder `docs/adr/`.
- **Jangan tulis yang obvious**: `// i++ increments i by 1` tidak perlu. Fokus pada *why*, bukan *what*.
- **Troubleshooting section**: Antisipasi error paling umum & solusinya.

## 4. Analogi
Dokumentasi seperti **buku manual IKEA**. Kode adalah lemari yang sudah dirakit. Tanpa manual, Anda mungkin bisa menebak cara membongkarnya, tapi butuh waktu. Dengan manual, siapapun bisa melakukannya dalam 5 menit.

## 5. Praktik Langsung
Buat README untuk project manajemen perpustakaan (seperti yang ada di Level 4-5). Struktur: judul, deskripsi, fitur, instalasi, usage, API endpoints, kontribusi, lisensi. Tulis dalam 30 menit.

## 6. Kesalahan Umum
- Dokumentasi ditulis setelah kode selesai → basi sebelum dirilis. Tulis docs bersamaan dengan kode.
- Terlalu teknis untuk user, terlalu dangkal untuk developer. Kenali audiens.
- Tidak pernah di-update. Docs yang kadaluarsa lebih buruk dari tidak ada docs.

## 7. Benang Merah
Materi 148 mengajarkan *membaca* kode orang lain. Materi 149 mengajarkan *membantu* orang lain membaca kode kita. Dokumentasi yang baik adalah bentuk empati tim. Selanjutnya: 150 (Estimasi Waktu) — setelah dokumen selesai, bagaimana memperkirakan waktu pengerjaan fitur.

## 8. Soal + Jawaban

**Soal 1:** Sebutkan 3 jenis audiens dokumentasi dan apa yang mereka butuhkan.

**Jawaban 1:**
- User: cara pakai (instalasi, usage, API)
- Developer: cara kerja (arsitektur, kontribusi, testing)
- Maintainer: alasan keputusan (ADR, trade-offs)

**Soal 2:** Kapan waktu terbaik menulis dokumentasi?

**Jawaban 2:** Bersamaan dengan menulis kode, bukan setelah selesai. Idealnya sebelum pull request diajukan.

**Soal 3:** Apa isi minimum sebuah README?

**Jawaban 3:** What (nama & deskripsi), Why (masalah), How (instalasi & usage), API reference (jika library/paket), lisensi.
