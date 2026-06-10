# 26. String Lanjutan — Methods & Regex Intro

**Benang Merah**: Di Materi 25 kita melihat array punya banyak methods bawaan. String juga punya! Kita akan belajar tools presisi untuk memeriksa dan memanipulasi teks.

---

## Penjelasan

String di JavaScript adalah **primitive** namun diperlakukan seperti **object** — ia punya properti `.length` dan methods bawaan. Methods string **tidak mengubah string asli** (string itu immutable), melainkan mengembalikan string baru.

```javascript
let kata = "  JavaScript  ";
console.log(kata.length);        // 14 (termasuk spasi)
console.log(kata.trim());        // "JavaScript" — hapus spasi ujung
```

**Intro Regex (Regular Expression)**: Pola pencarian teks dengan simbol khusus. Kita mulai dengan yang dasar — `test()` dan beberapa pattern sederhana.

---

## Fungsi

Memanipulasi, memvalidasi, dan mencari pola dalam teks dengan presisi — seperti alat ukur di tukang kayu.

---

## Code — Validator Form

```javascript
// METHODS STRING
function validasiForm(nama, email, telepon) {
  // .trim() — hapus spasi di ujung
  nama = nama.trim();
  email = email.trim();
  telepon = telepon.trim();

  // .startsWith() & .endsWith()
  if (!nama.startsWith("Mr.") && !nama.startsWith("Ms.")) {
    return "Nama harus diawali Mr. atau Ms.";
  }

  // .indexOf() — cari posisi karakter
  if (email.indexOf("@") === -1) {
    return "Email harus mengandung @";
  }

  // .lastIndexOf() — cari dari belakang
  const titikTerakhir = email.lastIndexOf(".");
  const setelahTitik = email.slice(titikTerakhir + 1);
  if (setelahTitik.length < 2) {
    return "Domain email minimal 2 karakter (.com, .id, dll)";
  }

  // .padStart() & .padEnd() — format nomor telepon
  if (telepon.length < 10) {
    telepon = telepon.padStart(12, "62");
  }
  telepon = telepon.padEnd(15, ".");

  // .repeat() — buat garis pemisah
  const garis = "=".repeat(30);

  return `${garis}\nNama: ${nama}\nEmail: ${email}\nTelepon: ${telepon}\n${garis}`;
}

console.log(validasiForm("Mr. Budi", "budi@rumah.com", "0812345678"));

// INTRO REGEX — basic pattern test
const regexEmail = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
console.log(regexEmail.test("budi@rumah.com")); // true
console.log(regexEmail.test("budi@rumah"));     // false (tanpa domain)

const regexTelepon = /^08[0-9]{8,11}$/;
console.log(regexTelepon.test("08123456789"));  // true
console.log(regexTelepon.test("12345"));        // false
```

---

## Analogi: Membangun Rumah (Alat Ukur Tukang Kayu)

| String Method | Alat Ukur di Tukang Kayu |
|---|---|
| `.indexOf()` | Meteran — cari posisi paku di papan |
| `.lastIndexOf()` | Meteran dari ujung lain |
| `.startsWith()` | Cek ujung papan — "apakah rata?" |
| `.endsWith()` | Cek ujung satunya |
| `.padStart()` | Tambah bantalan di awal agar pas |
| `.padEnd()` | Tambah bantalan di akhir |
| `.repeat()` | Potong 10 papan dengan ukuran sama |
| `.trim()` | Amplas — hapus serpihan di tepi |
| Regex | Template ukir presisi — pola yang dicocokkan |

Seperti tukang kayu yang punya **meteran, siku, jangka sorong**, seorang programmer punya methods string untuk mengukur, memotong, dan memformat teks dengan presisi.

---

## Dipakai Untuk Apa

- **Validasi form** — nama, email, telepon, password, kode pos
- **Parsing data** — ekstrak informasi dari teks (CSV, log file)
- **Format display** — nomor rekening, KTP, tanggal, mata uang
- **Autocomplete / search** — cek apakah input cocok dengan data
- **Sanitasi input** — hapus spasi, karakter berbahaya

---

## Kesalahan Umum

| Kesalahan | Contoh | Penjelasan |
|---|---|---|
| String itu mutable | `str.trim()` lalu lihat `str` asli | String **immutable** — simpan hasilnya |
| Lupa case-sensitive | `"Budi".startsWith("budi")` | Hasil `false` — JS case-sensitive |
| `indexOf` return -1 | `if (str.indexOf("x"))` | `-1` itu truthy! Pakai `=== -1` |
| Regex overkill | Regex untuk split sederhana | Kadang `.split()` + `.trim()` lebih bersih |
| `.repeat(0)` | `"a".repeat(0)` | Return `""`, valid — bukan error |

---

## Hubungan dengan Materi Sebelumnya

- **Materi 25 (Array methods)**: Sama seperti array, string punya methods bawaan yang tidak mengubah data asli.
- **Materi 10 (String dasar)**: Di Level 1 kita hanya gabung string dengan `+`. Sekarang kita punya alat presisi.
- **Materi 27 (Object)**: String juga bisa jadi properti object — data form akan disimpan sebagai object.

---

## Soal Latihan

### Soal 1 (Mudah)
Gunakan `.startsWith()` dan `.endsWith()` untuk mengecek apakah string `"JavaScript"` diawali "Java" dan diakhiri "Script".

**Jawaban**:
```javascript
const kata = "JavaScript";
console.log(kata.startsWith("Java"));   // true
console.log(kata.endsWith("Script"));   // true
```

### Soal 2 (Sedang)
Format angka `7` menjadi `"0007"` menggunakan `.padStart()`, lalu ulangi sebanyak 3 kali dengan `.repeat()`.

**Jawaban**:
```javascript
const angka = 7;
const format = String(angka).padStart(4, "0");
console.log(format);           // "0007"
console.log(format.repeat(3)); // "000700070007"
```

### Soal 3 (Tantangan)
Buat fungsi `validasiPassword(pw)` yang:
- Minimal 8 karakter (`.length`)
- Diawali huruf kapital (`.charAt(0)` + regex / huruf besar)
- Mengandung angka (regex `/\d/`)
- Kembalikan "Valid" atau pesan error pertama yang ditemukan

**Jawaban**:
```javascript
function validasiPassword(pw) {
  if (pw.length < 8) return "Minimal 8 karakter";
  if (pw.charAt(0) !== pw.charAt(0).toUpperCase())
    return "Harus diawali huruf kapital";
  if (!/[0-9]/.test(pw)) return "Harus mengandung angka";
  return "Valid";
}

console.log(validasiPassword("Budi1234")); // Valid
console.log(validasiPassword("budi1234")); // Harus diawali huruf kapital
console.log(validasiPassword("Budi"));     // Minimal 8 karakter
```
