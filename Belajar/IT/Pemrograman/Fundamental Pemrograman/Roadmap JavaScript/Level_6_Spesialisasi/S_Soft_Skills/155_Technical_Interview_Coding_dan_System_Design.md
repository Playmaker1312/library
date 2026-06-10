# 155 — Technical Interview: Coding & System Design

## 1. Penjelasan
Technical interview adalah gerbang masuk ke perusahaan teknologi. Formatnya bervariasi: coding (LeetCode style), system design (arsitektur aplikasi), dan behavioral (STAR method). Tujuan interviewer bukan melihat *apakah* Anda bisa menyelesaikan soal, tapi *bagaimana* Anda berpikir saat menyelesaikannya.

## 2. Fungsi
- Menyaring kandidat yang punya fundamental kuat
- Menguji kemampuan problem-solving dalam tekanan
- Menilai komunikasi dan kolaborasi (pair programming interview)
- Mengecek kesesuaian teknis dengan stack perusahaan

## 3. Tips / Cara
- **Think aloud**: Suarakan proses berpikir Anda. Interviewer tidak bisa membaca pikiran.
- **Clarify dulu**: Jangan langsung coding. Tanya edge cases, input/output, constraints.
- **Brute force → optimize**: Selesaikan versi sederhana dulu, lalu optimasi setelah diskusi.
- **Minta hint jika buntu**: Tidak apa. Yang dinilai adalah bagaimana Anda merespon bantuan.
- **System design**: Fokus pada *process* (bagaimana Anda memecah masalah) bukan *detail* (nama DB, versi). Mulai dari requirement, lalu high-level diagram, baru deep dive.

**STAR method untuk behavioral:**
- **S**ituation: Konteks
- **T**ask: Tugas Anda
- **A**ction: Tindakan spesifik Anda
- **R**esult: Hasil (pakai data jika bisa)

## 4. Analogi
Technical interview seperti **ujian praktik memasak di restoran**. Anda tidak dinilai dari *apakah masakan jadi*, tapi dari *bagaimana Anda memotong, mengatur waktu, membersihkan meja, dan merespon feedback chef*. Proses > hasil akhir.

## 5. Praktik Langsung
Blind solve 1 LeetCode Easy + 1 LeetCode Medium dalam 30 menit:
1. Easy: Two Sum (pakai hash map, O(n))
2. Medium: Valid Parentheses (pakai stack)
Catat: waktu mulai, waktu klarifikasi, waktu coding, waktu testing. Evaluasi apakah Anda memenuhi 30 menit.

## 6. Kesalahan Umum
- Langsung coding tanpa klarifikasi → salah asumsi, coding ulang.
- Diam saja saat coding → interviewer tidak tahu apa yang Anda pikirkan.
- Terlalu fokus LeetCode → lupa fundamental (OOP, DB, networking) untuk system design.
- Berhenti total saat dapat soal sulit → padahal interviewer ingin lihat Anda bertahan.
- Tidak pernah latihan behavioral → padahal banyak kandidat gagal di sini.

## 7. Benang Merah
Materi 154 (open source) memberi pengalaman kolaborasi nyata yang bisa diceritakan di behavioral interview. Materi 155 mengajarkan cara mengonversi pengalaman itu (dan kemampuan coding) menjadi tawaran kerja. Selanjutnya: 156 (Continuous Learning) — setelah diterima, bagaimana terus bertumbuh.

## 8. Soal + Jawaban

**Soal 1:** Mengapa "think aloud" penting saat technical interview?

**Jawaban 1:** Interviewer tidak bisa membaca pikiran. Dengan think aloud, mereka bisa menilai proses berpikir, memberikan hint jika salah arah, dan memahami pendekatan Anda — bahkan jika kode belum selesai.

**Soal 2:** Sebutkan 3 langkah sebelum mulai coding di soal LeetCode.

**Jawaban 2:** 1) Clarify input/output dan constraints, 2) Tanya edge cases (empty, null, duplicates), 3) Diskusikan approach dengan interviewer (brute force dulu, lalu optimasi).

**Soal 3:** Apa yang lebih penting dalam system design interview: detail teknis atau proses berpikir?

**Jawaban 3:** Proses berpikir. Interviewer ingin melihat bagaimana Anda memecah masalah besar menjadi bagian kecil, membuat trade-off, dan berkomunikasi secara jelas. Detail teknis bisa dipelajari, proses berpikir tidak.
