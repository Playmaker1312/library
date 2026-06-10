# 150 — Estimasi Waktu & Task Breakdown

## 1. Penjelasan
Estimasi adalah salah satu hal tersulit dalam software engineering. Hofstadter's Law: "Everything takes longer than you expect, even when you take into account Hofstadter's Law." Solusinya bukan menebak lebih akurat, tapi memecah pekerjaan menjadi bagian kecil (task breakdown) dan menambahkan buffer. Metrik: story points (relative estimate) vs hours (absolute).

## 2. Fungsi
- Membantu prioritas fitur (mana yang bisa masuk sprint)
- Memberi prediksi realistis ke stakeholder
- Mengidentifikasi risiko lebih awal
- Meningkatkan akurasi seiring waktu (data historis sprint)

## 3. Tips / Cara
- **Breakdown hirarki**: Epic (besar) → Story (fitur) → Task (≤4 jam). Jika task >4 jam, pecah lagi.
- **Planning poker**: Tim vote bersamaan pakai kartu Fibonacci (1, 2, 3, 5, 8, 13). Diskusi jika selisih terlalu jauh.
- **Buffer**: Always ×2 dari estimasi awal. Bedakan **waktu kerja** (coding) vs **waktu kalender** (rapat, review, testing, revisi).
- **Hati-hati unknown unknowns**: Hal yang Anda tidak tahu bahwa Anda tidak tahu — ini sumber keterlambatan terbesar.

## 4. Analogi
Estimasi seperti **memperkirakan waktu perjalanan**. Google Maps bilang 30 menit, tapi Anda tambah 15 menit cadangan. Kenapa? Karena ada lampu merah, macet, cari parkir. Sama dengan coding — ada bug, perubahan requirement, rapat mendadak.

## 5. Praktik Langsung
Breakdown fitur "Login with Google" menjadi micro-task:
1. Setup OAuth credentials di Google Console (1 jam)
2. Install & konfigurasi passport-google-oauth20 (30 menit)
3. Implementasi route `/auth/google` dan callback (2 jam)
4. Handle error (token expired, user ditolak) — 1 jam
5. Testing manual & edge cases (1,5 jam)
6. Tulis dokumentasi setup (30 menit)
→ Total estimasi: 6,5 jam kerja. ×2 buffer = 13 jam kalender.

## 6. Kesalahan Umum
- Estimasi dalam jam tanpa buffer langsung dikomunikasikan ke atasan → deadline tidak realistis.
- Lupa menyertakan waktu testing, review, dan rapat.
- Terlalu optimis ("ini mah gampang, 1 jam") — padahal setup saja 30 menit.
- Tidak pernah review estimasi sebelumnya → mengulang kesalahan yang sama.

## 7. Benang Merah
Materi 149 (dokumentasi) membantu tim memahami kode. Materi 150 membantu tim merencanakan kapan kode itu selesai. Estimasi yang baik membutuhkan pemahaman task yang baik — yang datang dari dokumentasi yang baik. Selanjutnya: 151 (Komunikasi Teknis) — menyampaikan estimasi ke non-teknis.

## 8. Soal + Jawaban

**Soal 1:** Sebutkan Hofstadter's Law dan mengapa itu relevan dengan estimasi.

**Jawaban 1:** "Everything takes longer than you expect, even when you take into account Hofstadter's Law." Relevan karena estimasi selalu bias optimis, dan kita lupa memperhitungkan hal-hal tak terduga (unknown unknowns).

**Soal 2:** Apa perbedaan waktu kerja dan waktu kalender?

**Jawaban 2:** Waktu kerja adalah jam yang benar-benar dipakai coding. Waktu kalender adalah total hari yang berlalu dari mulai sampai selesai, termasuk rapat, review, revisi, dan aktivitas non-coding lainnya.

**Soal 3:** Mengapa task harus dipecah sampai <4 jam?

**Jawaban 3:** Task kecil lebih mudah diestimasi akurat, lebih mudah dilacak progresnya, dan jika melebihi estimasi, kita tahu lebih awal ada masalah.
