# Materi 91: Unit Testing dengan Vitest

## 1. Penjelasan
Unit testing menguji satu unit (fungsi/komponen) secara terisolasi. Vitest adalah framework testing modern untuk Vite/TypeScript. Pola dasar: Arrange (siapkan data) → Act (jalankan fungsi) → Assert (periksa hasil). TDD: Red (tulis test gagal) → Green (buat lulus) → Refactor.

## 2. Fungsi
- Kepercayaan diri saat refactor — test akan tangkap jika ada yang rusak
- Dokumentasi hidup — test menunjukkan bagaimana fungsi seharusnya dipakai
- Mencegah regresi — bug lama tidak kembali

## 3. Code

```typescript
// utils.ts
export function calculateLateFee(daysLate: number, dailyRate: number): number {
  if (daysLate <= 0) return 0;
  return daysLate * dailyRate;
}

export function isEligibleToBorrow(age: number, hasActiveLoan: boolean): boolean {
  if (age < 17) return false;
  if (hasActiveLoan) return false;
  return true;
}
```

```typescript
// utils.test.ts — Unit Test dengan Vitest
import { describe, it, expect } from "vitest";
import { calculateLateFee, isEligibleToBorrow } from "./utils";

describe("calculateLateFee", () => {
  // Arrange
  it("returns 0 when daysLate is 0", () => {
    const result = calculateLateFee(0, 1000); // Act
    expect(result).toBe(0); // Assert
  });

  it("returns fine when daysLate is positive", () => {
    expect(calculateLateFee(3, 500)).toBe(1500);
  });

  it("returns 0 when daysLate is negative", () => {
    expect(calculateLateFee(-1, 1000)).toBe(0);
  });
});

describe("isEligibleToBorrow", () => {
  it("rejects minors under 17", () => {
    expect(isEligibleToBorrow(16, false)).toBe(false);
  });

  it("rejects if hasActiveLoan is true", () => {
    expect(isEligibleToBorrow(20, true)).toBe(false);
  });

  it("allows eligible user", () => {
    expect(isEligibleToBorrow(20, false)).toBe(true);
  });
});
```

## 4. Analogi Rumah

| Konsep Testing | Analogi Rumah |
|---------------|---------------|
| Unit test | Uji material bangunan sebelum dipakai — tekan bata, tarik kabel, pastikan kuat |
| Arrange | Siapkan bata, mortar, alat ukur |
| Act | Pasang bata di dinding |
| Assert | Periksa: apakah tegak lurus? Apakah kokoh? |
| TDD (Red) | Tentukan standar: "bata harus tahan 100kg" — test belum lulus |
| TDD (Green) | Beli bata yang sesuai standar — test lulus |
| Refactor | Setelah bata terpasang, rapikan sisa mortar (tanpa mengubah bentuk bata) |

## 5. Use Case
Project perpustakaan: fungsi `calculateLateFee` pernah diubah logikanya (dari flat rate ke progressive rate). Tanpa test, bug tidak terdeteksi selama 2 minggu. Setelah unit test ditambahkan, refactor jadi aman.

## 6. Kesalahan Umum
- Test tergantung test lain (tidak terisolasi)
- Menguji implementasi detail (bukan behavior)
- Test terlalu banyak mock sehingga tidak menguji apapun
- Lupa menjalankan test setelah refactor
- Test coverage 100% tapi kualitas test rendah (assert yang salah)

## 7. Benang Merah
Dari Materi 90 (prinsip DRY/KISS/YAGNI). Unit test adalah alat untuk memastikan prinsip tersebut berjalan. Lanjut ke Materi 92 (Integration & E2E Testing).

## 8. Soal

### Soal 1
Tulis Arrange-Act-Assert untuk fungsi `isEligibleToBorrow(17, false)`.
**Jawaban:**
```typescript
// Arrange: age = 17, hasActiveLoan = false (remaja 17 tahun, tidak punya pinjaman aktif)
// Act: panggil fungsi
const result = isEligibleToBorrow(17, false);
// Assert: seharusnya true (17 sudah eligible)
expect(result).toBe(true);
```

### Soal 2
Apa perbedaan TDD dengan testing biasa?
**Jawaban:** TDD menulis test *sebelum* kode produksi (Red → Green → Refactor). Testing biasa menulis test *setelah* kode jadi.

### Soal 3
Mengapa mocking berlebihan berbahaya?
**Jawaban:** Mock berlebihan membuat test tidak menguji integrasi nyata dengan dependency. Test bisa lulus padahal sistem gagal di produksi karena komponen asli tidak bekerja seperti mock.
