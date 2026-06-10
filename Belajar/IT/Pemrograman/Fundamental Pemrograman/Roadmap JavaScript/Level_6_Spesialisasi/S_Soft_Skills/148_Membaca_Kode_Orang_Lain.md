# 148. Membaca & Memahami Kode Orang Lain — Skill yang Sering Diabaikan

**Benang Merah**: Selama Level 1-5 kita fokus **MENULIS** kode. Di dunia nyata, programmer menghabiskan **70% waktu membaca** kode (kode sendiri yang lama maupun kode orang lain). Skill ini yang membedakan junior dan senior.

---

## Penjelasan

Membaca kode orang lain adalah kemampuan untuk **memahami logika, struktur, dan intent** dari kode yang tidak kita tulis. Ini termasuk:
- Kode tim sendiri dari 6 bulan lalu
- Open source library
- Code review PR orang lain
- Kode legacy yang tidak ada dokumentasi

```javascript
// Contoh: kode yang sulit dibaca vs mudah dibaca

// ❌ Sulit — apa ini fungsinya?
function a(b) {
  let c = 0;
  for (let d of b) {
    if (d.e > 0) c += d.e * 2;
  }
  return c / b.length;
}

// ✅ Mudah — jelas tujuannya
function hitungRataRataGajiKaryawanAktif(dataKaryawan) {
  let totalGaji = 0;
  for (let karyawan of dataKaryawan) {
    if (karyawan.status === 'AKTIF') {
      totalGaji += karyawan.gaji * 2; // bonus THR
    }
  }
  return totalGaji / dataKaryawan.length;
}
```

---

## Fungsi

- **Onboarding cepat** — pahami codebase baru dalam hari, bukan minggu
- **Code review efektif** — temukan bug dan improvement
- **Belajar dari yang terbaik** — baca source code Express, Vue, Prisma
- **Maintain kode legacy** — tidak semua kode bisa di-rewrite
- **Debugging** — temukan sumber bug dengan membaca alur kode

---

## Cara Pengimplementasian (Strategi Membaca Kode)

### 1. Start from Entry Point
```text
// Cari file utama dulu
package.json → "main": "index.js"
// atau app.js, server.js, main.js
```

### 2. Baca Test Dulu (jika ada)
```javascript
// Test file memberi tahu "apa yang seharusnya dilakukan kode"
describe('Calculator', () => {
  it('should add two numbers', () => {
    expect(add(2, 3)).toBe(5);
  });
});
// Tanpa lihat implementasi, kita sudah tahu add(2,3) = 5
```

### 3. Trace Satu Flow
```javascript
// Jangan baca semua file sekaligus
// Ikuti SATU alur: User klik → event → handler → API → database → response

// 1. Event handler
btn.addEventListener('click', handleSubmit);

// 2. handleSubmit
function handleSubmit() {
  const data = getFormData();       // ambil data
  validateForm(data);               // validasi
  saveToDatabase(data);             // simpan
  showSuccessMessage();             // feedback
}
```

### 4. Ignore Detail Kecil Dulu
```javascript
function complexProcess(data) {
  const validated = validateStep1(data);   // skip detail dulu
  const transformed = transformStep2(validated); // skip
  const enriched = enrichStep3(transformed);     // skip
  return formatOutput(enriched);                  // skip
}
// Fokus: data input → 4 langkah → output. Detail nanti.
```

### 5. Dokumentasi Saat Membaca
```javascript
// Tambah komentar untuk diri sendiri (nanti dihapus)
function mysteryFunction(x, y) {
  // TODO: cari tau: kenapa ada Math.sqrt di sini?
  return Math.sqrt(x * x + y * y);
}
// (ternyata: Euclidean distance!)
```

---

## Analogi: Membangun Rumah (Membaca Blueprint Orang Lain)

| Membaca Kode | Membaca Blueprint Arsitek Lain |
|---|---|
| Entry point | Pintu utama gedung |
| Function | Ruangan dengan fungsi tertentu |
| Variable name | Label furnitur |
| Import/module | Referensi ke gambar detail lain |
| Test | Foto "hasil jadi" — tahu tujuan tanpa baca blueprint |
| Comments | Catatan arsitek di pinggir gambar |

Bayangkan Anda datang ke proyek gedung yang sudah setengah jadi. Arsitek sebelumnya sudah tidak ada. Anda harus membaca **blueprint** untuk melanjutkan pembangunan. Anda tidak perlu baca setiap garis dalam 5 detik — Anda mulai dari: "Ini pintu utama → koridor → ruang tamu → dapur." Paham alurnya dulu, detail belakangan.

---

## Dipakai Untuk Apa

- **Code review** — tiap kali ada PR di GitHub
- **Open source contribution** — sebelum kontribusi ke library
- **Debugging** — cari akar masalah
- **Onboarding** — hari pertama di tim baru
- **Refactoring** — pahami dulu sebelum ubah

---

## Kesalahan Umum

| Kesalahan | Akibat |
|---|---|
| Baca file dari atas ke bawah tanpa strategi | Tenggelam dalam detail, kehilangan gambaran besar |
| Langsung伸手 edit kode tanpa paham | Merusak fitur yang tidak diketahui |
| Skip test | Kehilangan konteks apa yang seharusnya dilakukan kode |
| Anggap kode orang lain jelek | Bias menghalangi pemahaman |
| Tidak tanya ke author (jika ada) | Buang waktu berjam-jam padahal bisa 5 menit tanya |

---

## Hubungan dengan Materi Sebelumnya

Semua skill sebelumnya membantu membaca kode:
- Materi 32 (Fungsi) → Identifikasi unit logika
- Materi 40 (Modules) → Struktur file dan dependensi
- Materi 88 (Clean Code) → Kode bersih lebih mudah dibaca
- Materi 91 (Testing) → Test dokumen behavior
- Materi 95 (Refactoring) → Memahami sebelum refactor

---

## Soal Latihan

### Soal 1 (Mudah)
Buka salah satu file kode yang pernah Anda buat (Level 1-2). Baca ulang dan jawab:
- Apa fungsi utama file ini?
- Ada tidak variabel dengan nama tidak jelas?
- Jika orang lain membaca, apa yang akan sulit dipahami?

**Jawaban**:
(Tidak ada jawaban mutlak — ini latihan refleksi)

### Soal 2 (Sedang)
Lihat kode berikut. Tanpa menjalankan, tebak apa yang dilakukan:
```javascript
function process(data) {
  const seen = new Set();
  const result = [];
  for (const item of data) {
    if (!seen.has(item.id)) {
      seen.add(item.id);
      result.push(item);
    }
  }
  return result;
}
```

**Jawaban**:
Fungsi ini **menghapus duplikat** dari array of objects berdasarkan properti `id`. Jika ada dua item dengan `id` yang sama, hanya item pertama yang dipertahankan.

### Soal 3 (Tantangan)
Buka repositori open source kecil (cari di GitHub: library yang < 1000 baris). Baca entry point dan trace satu fitur. Catat:
- File mana yang pertama dibaca?
- Bagaimana alur fitur tersebut?
- Satu hal yang Anda pelajari dari kode tersebut.

**Jawaban**:
(Tidak ada jawaban mutlak — ini tugas praktik. Contoh jawaban: "Saya baca repository `chalk` (library color terminal). Entry point di `index.js`. Satu fitur trace: `chalk.red('teks')` → chain method → apply ANSI color codes. Saya belajar: pattern chaining dengan getter di JS.")
