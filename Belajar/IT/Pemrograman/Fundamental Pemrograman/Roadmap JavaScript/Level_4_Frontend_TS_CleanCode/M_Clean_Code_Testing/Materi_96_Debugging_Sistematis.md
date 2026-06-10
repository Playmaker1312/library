# Materi 96: Debugging Sistematis

## 1. Penjelasan
Debugging bukan coba-coba (trial & error) — ini proses sistematis dengan metodologi: observasi → hipotesis → eksperimen → konfirmasi. Tools: Chrome DevTools (breakpoint, watch, network tab), Node.js debugger (`node inspect`), dan logging efektif.

## 2. Fungsi
- Menemukan akar masalah (root cause) bukan hanya gejala
- Menghemat waktu dibanding trial & error
- Memahami aliran data dan state aplikasi

## 3. Code

```typescript
// --- 5 KODE BUGGY UNTUK DEBUG ---

// Bug 1: Off-by-one
function getMiddleIndex(arr: number[]): number {
  return Math.ceil(arr.length / 2); // Bug: harus Math.floor untuk 0-indexed
}

// Bug 2: Referensi vs value
function updateBookPrice(books: any[], discount: number) {
  books.forEach(b => {
    b = { ...b, price: b.price * discount }; // Bug: tidak mengubah array asli
  });
}

// Bug 3: Type coercion
function sum(a: number, b: number): number {
  return a + b; // Jika string dipanggil, terjadi concatenation bukan penjumlahan
}

// Bug 4: Async tidak tertangani
async function fetchBook(id: string) {
  const res = await fetch(`/api/books/${id}`);
  return res.json(); // Bug: tidak ada error handling jika fetch gagal
}

// Bug 5: Closure loop
function createButtons() {
  for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 1000); // Bug: semua log 3
  }
}
```

```typescript
// --- DEBUGGING SISTEMATIS DENGAN NODE.JS ---
// 1. Tambahkan breakpoint: debugger;
// 2. Jalankan: node inspect file.js
// 3. Commands: cont (continue), next (next line), step (masuk fungsi)
// 4. Watch: watch('variableName')

function calculateDiscountedPrice(price: number, discount: number): number {
  debugger; // breakpoint
  const result = price * (1 - discount);
  return result;
}

// --- LOGGING EFEKTIF (bukan console.log saja) ---
const DEBUG = true;

function log(level: "info" | "warn" | "error", context: string, message: string, data?: any) {
  if (!DEBUG && level === "info") return;
  const timestamp = new Date().toISOString();
  console[level](`[${timestamp}] [${level.toUpperCase()}] [${context}] ${message}`, data ?? "");
}

// Penggunaan
log("info", "BorrowService", "Borrowing book", { bookId: "123", userId: "456" });
log("error", "BorrowService", "Failed to borrow book", { error: "Book not found" });
```

## 4. Analogi Rumah

| Konsep Debugging | Analogi Rumah |
|-----------------|---------------|
| Trial & error | Tebak bocor: ganti semua pipa tanpa cek — mahal, lama, belum tentu ketemu |
| Debugging sistematis | Deteksi bocor: cek meteran air (breakpoint) → matikan kran satu per satu → lacak ke titik bocor |
| Breakpoint | Pasang sensor di setiap sambungan pipa — berhenti dan cek tekanan air |
| Watch | Pantau manometer air di titik tertentu |
| Call stack | Peta jalur pipa dari sumber ke kran |
| Logging | Buku catatan: "pukul 14:30, tekanan turun drastis di lantai 2" |

## 5. Use Case
Bug: buku yang dikembalikan tidak muncul di daftar "tersedia". Daripada trial & error ganti-ganti kode, pasang breakpoint di `processBookReturn()` → watch variabel `book.available` → ketahuan bahwa fungsi `markBookAvailable` tidak terpanggil karena conditional `daysLate` salah.

## 6. Kesalahan Umum
- Langsung ganti kode tanpa diagnosis (trial & error)
- Hanya lihat gejala, bukan root cause
- Overusing `console.log` tanpa struktur — susah dilacak
- Tidak menggunakan breakpoint/watch di DevTools
- Debugging di production langsung (tanpa staging)
- Tidak menulis test untuk bug yang ditemukan (regresi akan muncul lagi)

## 7. Benang Merah
**PENUTUP Level 4.** Dari Materi 88 (clean code) → 89 (SOLID) → 90 (DRY/KISS/YAGNI) → 91 (unit test) → 92 (integration/E2E) → 93-94 (design patterns) → 95 (refactoring) → 96 (debugging). Semua kemampuan ini membentuk developer yang tidak hanya bisa nulis kode, tapi juga bisa merawat, menguji, dan memperbaikinya. Lanjut ke **Level 5: Security**.

## 8. Soal

### Soal 1
Sebutkan langkah-langkah debugging sistematis.
**Jawaban:** 1) Reproduksi bug secara konsisten. 2) Observasi gejala. 3) Buat hipotesis penyebab. 4) Uji hipotesis (breakpoint, watch, logging). 5) Konfirmasi root cause. 6) Perbaiki. 7) Verifikasi bug hilang. 8) Tulis test regresi.

### Soal 2
Debug kode berikut secara sistematis:
```typescript
function getFirstBook(books: string[]): string {
  return books[1]; // Seharusnya books[0]
}
```
**Jawaban:** A: Hipotesis — index array salah. B: Pasang breakpoint dan watch `books`, `books[1]`, `books[0]`. C: Konfirmasi bahwa `books[1]` mengembalikan element kedua, bukan pertama. D: Perbaiki jadi `books[0]`.

### Soal 3
Apa perbedaan `console.log` sembarangan dengan logging terstruktur?
**Jawaban:** `console.log` sembarangan: tanpa timestamp, level, konteks — susah dicari dan difilter. Logging terstruktur: `[2024-01-01] [ERROR] [BorrowService] Gagal pinjam buku: {bookId: "123"}` — mudah difilter, dicari, dan dianalisis.
