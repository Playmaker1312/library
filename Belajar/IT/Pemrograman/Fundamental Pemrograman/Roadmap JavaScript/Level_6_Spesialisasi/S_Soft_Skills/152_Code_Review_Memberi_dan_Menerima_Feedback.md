# 152 — Code Review: Memberi & Menerima Feedback

## 1. Penjelasan
Code review adalah proses pemeriksaan kode oleh developer lain sebelum kode digabung (merge). Bukan untuk "mencari kesalahan" atau "menunjukkan siapa yang lebih pintar", tapi untuk memastikan kualitas kode, berbagi pengetahuan, dan mencegah bug. Ada dua sisi: memberi feedback (reviewer) dan menerima feedback (author).

## 2. Fungsi
- Menangkap bug lebih awal (biaya perbaikan lebih murah)
- Menyebarkan pengetahuan kodebase ke seluruh tim
- Menjaga konsistensi arsitektur dan gaya kode
- Membangun budaya tanggung jawab kolektif

## 3. Tips / Cara

**Memberi feedback:**
- Spesifik: "Di baris 42, fungsi ini bisa return null jika user tidak ditemukan" — bukan "kode kamu rawan error".
- Objektif: Fokus pada kode, bukan orang. "Kode ini berpotensi memory leak" vs "Kamu lupa handle ini".
- Tawarkan solusi: "Saya sarankan pakai `try...catch` atau default value."
- Gunakan bahasa lunak: "I wonder if..." / "What do you think about..." — bukan "This is wrong".
- Fokus pada logic & design, biarkan linter urus style.

**Menerima feedback:**
- Jangan defensif. Jika reviewer salah paham, mungkin kode Anda kurang jelas.
- Tanyakan "Bisa tolong dijelaskan alternatifnya?" — bukan "Ya udah gitu aja."
- Ucapkan terima kasih. Setiap review adalah investasi waktu orang lain.

**Checklist review:**
- [ ] Apakah logic benar?
- [ ] Apakah ada edge case yang terlewat?
- [ ] Apakah ada security issue?
- [ ] Apakah mudah dibaca?
- [ ] Apakah ada test yang sesuai?

## 4. Analogi
Code review seperti **proofreading artikel**. Penulis mungkin sudah baca ulang 3 kali, tapi tetap ada typo yang terlewat. Pembaca kedua (proofreader) melihat dengan mata segar. Bedanya: proofreader menunjukkan kesalahan dengan sopan, bukan "Haha, kamu salah nulis!"

## 5. Praktik Langsung
Review PR sample berikut:

```javascript
// PR: Tambah fitur search user by email
function searchUser(email) {
    const users = db.query("SELECT * FROM users")
    for (let i = 0; i < users.length; i++) {
        if (users[i].email == email) return users[i]
    }
    return null
}
```

**Review comments:**
1. **SQL Injection**: Parameter langsung dimasukkan tanpa sanitasi. Gunakan parameterized query.
2. **Performa**: `SELECT *` lalu filter di JS — mending `SELECT * FROM users WHERE email = ?` di database.
3. **Edge case**: `searchUser("")` akan scan seluruh tabel. Tambah validasi input.
4. **Tidak async**: Operasi database sebaiknya async (gunakan promise/async-await).

## 6. Kesalahan Umum
- Review lebih dari 300 baris sekaligus → efektivitas menurun drastis. Minta author pecah PR.
- Too nitpicky: "Spasi di sini kurang satu." Biarkan linter dan formatter.
- Silent approval: Nggak dibaca, langsung approve. Ini berbahaya.
- Personal attack: "Ini jelek", "Kode kamu amburadul". Fokus ke kode, serang masalahnya.

## 7. Benang Merah
Materi 151 (komunikasi) mengajarkan bicara ke non-teknis. Materi 152 mengajarkan bicara ke sesama developer — dengan sopan, objektif, dan membangun. Dua arah: memberi dan menerima. Selanjutnya: 153 (Portfolio) — hasil kerja yang sudah direview bisa dipamerkan.

## 8. Soal + Jawaban

**Soal 1:** Apa tujuan utama code review?

**Jawaban 1:** Menangkap bug, berbagi pengetahuan, menjaga konsistensi, dan membangun tanggung jawab kolektif — bukan mencari kesalahan orang lain.

**Soal 2:** Sebutkan 3 karakteristik feedback yang baik.

**Jawaban 2:** Spesifik (tunjuk baris kode), objektif (fokus ke kode, bukan orang), tawarkan solusi alternatif.

**Soal 3:** Berapa batas maksimal baris PR agar review efektif?

**Jawaban 3:** < 300 baris. PR lebih besar harus dipecah menjadi beberapa PR kecil.
