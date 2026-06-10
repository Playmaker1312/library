# Materi 95: Code Refactoring & Technical Debt

## 1. Penjelasan
Refactoring adalah mengubah struktur kode tanpa mengubah perilaku eksternal. Technical debt adalah akumulasi kode buruk yang harus "dibayar" dengan waktu ekstra di masa depan. Teknik refactoring: Extract Method, Rename Variable, Move Function, Inline Temp.

## 2. Fungsi
- Mengurangi technical debt sebelum tidak terkendali
- Meningkatkan maintainability tanpa mengubah fitur
- Membayar "bunga" technical debt secara bertahap

## 3. Code

```typescript
// --- SEBELUM REFACTOR (banyak code smells) ---
function processReturn(data: any) {
  let x = data.book;
  let y = data.member;
  let z = new Date();
  let a = x.dueDate;
  let b = (z - a) / (1000 * 60 * 60 * 24);
  if (b > 0) {
    let c = b * 1000;
    // update member balance
    y.balance = (y.balance || 0) + c;
  }
  x.available = true;
  // save
  console.log("Book returned", x.title);
}

// --- SESUDAH REFACTOR ---
interface Book { title: string; dueDate: Date; available: boolean; }
interface Member { balance: number; }

function calculateLateFee(daysLate: number): number {
  return daysLate > 0 ? daysLate * 1000 : 0;
}

function applyLateFee(member: Member, fee: number): void {
  member.balance = (member.balance || 0) + fee;
}

function markBookAvailable(book: Book): void {
  book.available = true;
}

function processBookReturn(book: Book, member: Member): void {
  const today = new Date();
  const daysLate = Math.ceil((today.getTime() - book.dueDate.getTime()) / (1000 * 60 * 60 * 24));
  const fee = calculateLateFee(daysLate);

  applyLateFee(member, fee);
  markBookAvailable(book);
  console.log("Book returned", book.title);
}
```

## 4. Analogi Rumah

| Konsep | Analogi Rumah |
|--------|--------------|
| Technical debt | Pipa bocor dibiarkan, ditutup selotip — cepat sekarang, tapi tiap bulan makin bocor |
| Refactoring | Renovasi rumah — cat ulang, ganti kran bocor, perbaiki fondasi retak |
| Extract Method | Pisahkan instalasi listrik per ruangan (dari satu panel besar jadi sub-panel) |
| Rename Variable | Ganti label "kotak A" menjadi "Kotak Alat Listrik" |
| Bunga technical debt | Pipa bocor → tembok lembab → cat mengelupas → jamur → kesehatan terganggu |
| Kapan bayar | Setiap habis sprint, sisihkan 20% waktu untuk refactoring |

## 5. Use Case
Project Level 2 (perpustakaan) memiliki fungsi `processReturn` yang panjang, variabel bernama `x`, `y`, `z`, `a`, `b`, `c`. Technical debt ini membuat bug tidak terlihat: fee dihitung salah karena tidak ada validasi `daysLate`. Refactor memisahkan logika, menambahkan tipe, dan memberi nama jelas.

## 6. Kesalahan Umum
- Refactor sambil menambah fitur baru (harus terpisah)
- Tidak punya test sebelum refactor → tidak tahu apakah merusak sesuatu
- Over-refactoring: mengubah kode yang sudah baik "karena bisa lebih bagus"
- Menunda terus technical debt → "kita refactor nanti" → tidak pernah
- Refactor tanpa komunikasi tim → konflik merge

## 7. Benang Merah
Puncak dari Materi 88-94: semua prinsip (clean code, SOLID, DRY/KISS/YAGNI, patterns) diterapkan dalam refactoring. Lanjut ke Materi 96 (Debugging Sistematis — penutup Level 4).

## 8. Soal

### Soal 1
Apa saja code smells yang ada di kode `processReturn` sebelum refactor?
**Jawaban:** 1) Nama variabel tidak ekspresif (`x`, `y`, `z`, `a`, `b`, `c`). 2) Fungsi terlalu panjang melakukan banyak hal. 3) Magic number `1000` tanpa nama. 4) Komentar tidak jelas (`// save`). 5) Tidak ada TypeScript type.

### Soal 2
Mengapa refactoring sebaiknya dilakukan terpisah dari penambahan fitur?
**Jawaban:** Mencampur refactor dan fitur baru membuat sulit di-review dan sulit di-rollback. Jika ada bug, tidak jelas apakah dari refactor atau fitur baru. Prinsip: "refactor only" commit vs "feature" commit.

### Soal 3
Apa yang dimaksud "bunga technical debt"? Beri contoh.
**Jawaban:** Bunga adalah biaya tambahan akibat kode buruk yang terus ditunda. Contoh: fungsi `processReturn` tanpa type dan nama jelas — ketika ada fitur "denda progressive", developer butuh 3 jam untuk memahami kode lama (padahal cukup 15 menit jika bersih).
