# Materi 90: DRY, KISS & YAGNI

## 1. Penjelasan
Tiga prinsip praktis pengembangan: DRY (Don't Repeat Yourself — hindari duplikasi), KISS (Keep It Simple — jangan bikin rumit kalau sederhana sudah cukup), YAGNI (You Ain't Gonna Need It — jangan buat fitur yang belum dibutuhkan).

## 2. Fungsi
- DRY: Mengurangi bug karena perubahan cukup di satu tempat
- KISS: Mempercepat pengembangan dan onboarding
- YAGNI: Menghemat waktu dengan tidak membuat fitur spekulatif

## 3. Code

```typescript
// --- SEBELUM (melanggar DRY) ---
function validateEmail(email: string): boolean {
  return email.includes("@") && email.includes(".");
}
function validatePhone(phone: string): boolean {
  return phone.length >= 10 && /^\d+$/.test(phone);
}
function registerUser(email: string, phone: string) {
  if (email.includes("@") && email.includes(".")) { /* DRY violation */ }
  if (phone.length >= 10 && /^\d+$/.test(phone)) { /* DRY violation */ }
}

// --- SESUDAH (DRY + KISS) ---
const validators = {
  email: (v: string) => v.includes("@") && v.includes("."),
  phone: (v: string) => v.length >= 10 && /^\d+$/.test(v),
};

function validate(field: string, value: string): boolean {
  return validators[field]?.(value) ?? false;
}

// --- YAGNI: JANGAN BUAT INI ---
class PremiumFeatureManager {
  // Fitur ini belum diminta user, belum ada di requirement
  // Tapi kita buat "karena nanti mungkin butuh" — YAGNI!
  constructor() {
    console.log("Premium feature ready");
  }
}
```

## 4. Analogi Rumah

| Prinsip | Analogi Rumah |
|---------|--------------|
| DRY | Satu cetakan untuk semua bata — tidak perlu bikin cetakan baru tiap bata |
| KISS | Jangan bikin atap berbentuk kubah kalau cukup genteng biasa berlapis |
| YAGNI | Jangan pasang kolam renang "kalau nanti butuh" — pasang saja saat benar-benar mau renovasi |
| Pelanggaran DRY | Punya 3 buku manual AC yang isinya sama, di 3 laci berbeda |
| Pelanggaran KISS | Pasang sistem smart home dengan 20 sensor padahal hanya butuh lampu otomatis |
| Pelanggaran YAGNI | Bangun ruang server di rumah padahal baru punya satu laptop |

## 5. Use Case
Tim membuat fitur "dark mode" yang kompleks dengan tema kustom, jadwal otomatis, sinkronasi cloud — padahal user hanya minta tombol "mode gelap" sederhana. Ini melanggar KISS dan YAGNI sekaligus.

## 6. Kesalahan Umum
- DRY: Copy-paste kode validasi di 10 tempat berbeda
- KISS: Menggunakan pattern/abstraksi berlebihan untuk masalah sederhana (misal: bikin 5 class hanya untuk filter array)
- YAGNI: Membuat API endpoint yang tidak ada di spesifikasi "karena nanti ada aplikasi mobile"
- Over-engineering: Terapkan semua prinsip sekaligus tanpa konteks

## 7. Benang Merah
Dari Materi 89 (SOLID) yang fokus pada desain, sekarang prinsip praktis DRY/KISS/YAGNI. Lanjut ke Materi 91 (Unit Testing).

## 8. Soal

### Soal 1
Refactor kode berikut agar tidak melanggar DRY:
```typescript
function hitungLuasPersegi(s: number) { return s * s; }
function hitungLuasKubus(s: number) { return 6 * s * s; }
```
**Jawaban:**
```typescript
function luasPersegi(s: number) { return s * s; }
function hitungLuasKubus(s: number) { return 6 * luasPersegi(s); }
```

### Soal 2
Apa masalah dari pendekatan ini berdasarkan KISS?
```typescript
abstract class AbstractFilter {
  abstract apply<T>(data: T[], predicate: (item: T) => boolean): T[];
}
class NumberFilter extends AbstractFilter {
  apply<T>(data: T[], predicate: (item: T) => boolean) {
    return data.filter(predicate);
  }
}
```
**Jawaban:** Over-engineering. Cukup pakai `Array.filter()` langsung tanpa hierarki class.

### Soal 3
Berikan contoh nyata pelanggaran YAGNI di project perpustakaan.
**Jawaban:** Membuat sistem autentikasi biometrik (sidik jari, wajah) untuk aplikasi perpustakaan sederhana yang hanya dipakai 3 pustakawan — padahal cukup pakai username & password.
