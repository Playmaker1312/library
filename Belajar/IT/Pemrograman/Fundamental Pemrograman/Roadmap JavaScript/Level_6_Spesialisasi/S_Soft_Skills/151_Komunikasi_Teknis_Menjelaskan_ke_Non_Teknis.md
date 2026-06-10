# 151 — Komunikasi Teknis: Menjelaskan ke Non-Teknis

## 1. Penjelasan
Developer paling sering gagal bukan karena coding, tapi karena komunikasi. Jargon teknis (API, endpoint, database migration, refactor) membuat non-teknis (produk, desain, manajemen) bingung atau salah paham. Akibatnya: ekspektasi tidak selaras, fitur salah prioritas, deadline tidak disepakati. Tujuan: membuat orang non-teknis mengerti **mengapa** keputusan teknis diambil dan **apa dampaknya** ke produk/bisnis.

## 2. Fungsi
- Menyelaraskan ekspektasi antara tim teknis dan non-teknis
- Mempercepat pengambilan keputusan
- Membangun kepercayaan: tim non-teknis percaya technical lead karena mereka paham alasannya
- Menghindari miskomunikasi yang berujung rework

## 3. Tips / Cara
- **Teknik Feynman**: Jelaskan seperti ke anak SD. Jika tidak bisa, Anda belum paham sepenuhnya.
- **"Why" dulu, baru "How"**: Stakeholder peduli *dampak bisnis*, bukan teknis. "Kita perlu migrasi database karena sistem akan lambat saat pengguna 1 juta" — bukan "kita ganti PostgreSQL ke Cassandra karena sharding".
- **Analogi**: Kayak roadmap ini — API seperti kasir, database seperti gudang.
- **Law of 3**: Maksimal 3 poin utama. Otak manusia susah mencerna lebih.
- **Visualisasi**: Diagram > 1000 kata. Pakai Mermaid, Draw.io, atau whiteboard.
- **Cek pemahaman**: "Apakah yang saya jelaskan masuk akal? Ada yang mau ditanyakan?"

## 4. Analogi
Menjelaskan teknis ke non-teknis seperti **menerjemahkan bahasa asing ke bahasa sehari-hari**. Anda tahu kata "database migration" seperti kata asing. Terjemahkan: "Kita pindahkan gudang barang ke tempat yang lebih besar, tapi selama pindahan, toko tetap buka".

## 5. Praktik Langsung
Jelaskan "migrasi database" ke stakeholder non-teknis dalam 3 kalimat:
1. "Bayangkan toko buku kita mulai kehabisan rak karena pelanggan makin banyak."
2. "Kita perlu pindahkan semua buku ke gudang baru yang lebih besar (database baru) selama beberapa jam di malam hari."
3. "Hasilnya nanti: aplikasi lebih cepat, tidak lemot saat ramai, dan kita bisa tambah fitur baru dengan mudah."

## 6. Kesalahan Umum
- Membahas teknis terlalu detail ("Tabel users akan di-join dengan tabel orders via foreign key…") → audiens mati gaya.
- Menganggap mereka paham karena tidak bertanya → padahal malu bertanya.
- Menggunakan singkatan (ACID, CRUD, REST, etc.) tanpa dijelaskan.
- Marah saat mereka tidak paham — ingat, mereka bukan developer.

## 7. Benang Merah
Materi 150 (estimasi) memberi angka. Materi 151 mengajarkan cara menyampaikan angka itu (dan alasan di baliknya) ke non-teknis. Komunikasi yang baik = karier yang baik. Selanjutnya: 152 (Code Review) — komunikasi antar developer.

## 8. Soal + Jawaban

**Soal 1:** Jelaskan "refactoring" ke non-teknis dalam 1 kalimat.

**Jawaban 1:** "Kita merapikan dapur — tidak mengubah menu, tapi juru masak jadi lebih cepat dan aman bekerja."

**Soal 2:** Sebutkan 3 komponen Law of 3 dalam komunikasi.

**Jawaban 2:** Maksimal 3 poin utama, 3 kalimat per poin, 3 kata kunci per kalimat.

**Soal 3:** Mengapa "Why" harus disebut sebelum "How"?

**Jawaban 3:** Non-teknis peduli dampak bisnis, bukan mekanisme teknis. Jika mereka tahu *mengapa* sesuatu penting, mereka lebih percaya dan mendukung keputusan teknis.
