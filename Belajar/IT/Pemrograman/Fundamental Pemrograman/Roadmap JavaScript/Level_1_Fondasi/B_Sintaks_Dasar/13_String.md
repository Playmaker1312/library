# 13 — String: Tipe Data Terpenting untuk I/O

---

## 1. Penjelasan

**String** adalah kumpulan karakter — bisa huruf, angka, simbol, atau spasi — yang membentuk teks. Di JavaScript, string bersifat **immutable** (tak bisa diubah setelah dibuat).

### Cara Membuat String:

```javascript
const pakaiKutip = 'Ini string';
const pakaiKutip2 = "Ini juga string";
const pakaiBacktick = `Ini template literal — bisa ${ekspresi}`;
```

### Escape Characters:

| Escape | Arti |
|--------|------|
| `\n` | Baris baru |
| `\t` | Tab |
| `\'` | Kutip tunggal |
| `\"` | Kutip ganda |
| `\\` | Backslash |
| `` \` `` | Backtick |

### Template Literals (backtick `` ` ``):

```javascript
const nama = "Budi";
const umur = 25;
console.log(`Nama saya ${nama}, umur ${umur} tahun.`); 
// "Nama saya Budi, umur 25 tahun."
```

---

## 2. Fungsi — Method Esensial String

| Method | Fungsi |
|--------|--------|
| `.length` | Mengembalikan jumlah karakter |
| `.toUpperCase()` | Mengubah ke huruf kapital |
| `.toLowerCase()` | Mengubah ke huruf kecil |
| `.includes(sub)` | Cek apakah mengandung substring |
| `.slice(mulai, akhir)` | Mengambil potongan string |
| `.split(pemisah)` | Memecah string jadi array |
| `.trim()` | Menghapus spasi di awal & akhir |
| `.replace(cari, ganti)` | Mengganti substring pertama |

---

## 3. Code — Fungsi Format Nama (Capitalize Setiap Kata)

```javascript
// === FUNGSI CAPITALIZE SETIAP KATA ===

function capitalizeName(nama) {
  return nama
    .trim()                             // 1. Hapus spasi berlebih di pinggir
    .split(" ")                         // 2. Pecah jadi array per kata
    .map(kata =>                         // 3. Proses setiap kata
      kata.charAt(0).toUpperCase() +    //    - Huruf pertama kapital
      kata.slice(1).toLowerCase()       //    - Sisanya huruf kecil
    )
    .join(" ");                         // 4. Gabung kembali dengan spasi
}

// Uji coba
console.log(capitalizeName("  budi  "));              // "Budi"
console.log(capitalizeName("  MUHAMMAD  ALI  "));     // "Muhammad Ali"
console.log(capitalizeName("jOHN cENA"));             // "John Cena"
console.log(capitalizeName("sRI mULYANI"));           // "Sri Mulyani"


// === DEMO METHOD LAIN ===

const kalimat = "  Halo, Selamat Datang di JavaScript!  ";

console.log(kalimat.length);                    // 43 (termasuk spasi)
console.log(kalimat.trim().length);             // 39 (setelah trim)
console.log(kalimat.trim().toUpperCase());      // "HALO, SELAMAT DATANG DI JAVASCRIPT!"
console.log(kalimat.trim().includes("Datang")); // true
console.log(kalimat.trim().slice(0, 4));        // "Halo"
console.log(kalimat.trim().split(" "));         // ["Halo,", "Selamat", "Datang", "di", "JavaScript!"]
console.log(kalimat.trim().replace("JavaScript", "Node.js")); 
                                                // "Halo, Selamat Datang di Node.js!"


// === TEMPLATE LITERALS MULTILINE ===

const alamat = `
Nama  : ${capitalizeName("budi hartono")}
Kota  : Jakarta
Usia  : ${25 + 2}
`;
console.log(alamat);
// Nama  : Budi Hartono
// Kota  : Jakarta
// Usia  : 27
```

---

## 4. Analogi Rumah (Membangun Rumah)

**String = papan nama, tulisan di tembok, dokumen kontrak bangunan.**

| Konsep String | Analogi di Rumah |
|---------------|------------------|
| String itu sendiri | Papan nama rumah — berisi teks |
| Setiap karakter | Satu paku/papan penyusun papan nama |
| `.length` | Menghitung berapa papan yang dipakai |
| `.toUpperCase()` | Mengubah papan nama jadi huruf KAPITAL semua |
| `.includes()` | Mengecek apakah ada kata "Jl." di alamat |
| `.slice()` | Memotong bagian tertentu dari papan nama |
| `.split(",")` | Memecah alamat berdasarkan koma |
| `.trim()` | Merapikan pinggiran papan yang tidak rapi |
| `.replace()` | Mengganti "Jl." dengan "Jalan" |
| Template literal `` ` ` `` | Cetak biru dinamis yang bisa diisi data |
| Escape `\n` | Papan nama yang punya baris kedua |

> **Narasi**: Setiap rumah punya papan nama (string). Papan itu terdiri dari papan-papan kecil (karakter). Kadang kamu perlu mengubahnya jadi kapital (`.toUpperCase()`), memotongnya (`.slice()`), mengecek apakah alamat mengandung kata "Jakarta" (`.includes()`), atau mengganti "Jl." jadi "Jalan" (`.replace()`). Template literal seperti cetak biru — kamu bisa menyisipkan nomor rumah, nama pemilik, dan tanggal secara otomatis. Inilah kenapa string adalah tipe data terpenting: hampir semua I/O melibatkan teks.

---

## 5. Use Case

| Situasi | Method String |
|---------|---------------|
| Validasi email (cek ada "@") | `.includes("@")` |
| Format nama untuk database | `name.trim().toLowerCase()` |
| Tampilkan preview 100 karakter | `text.slice(0, 100) + "..."` |
| Parsing CSV | `baris.split(",")` |
| Bersihkan input spasi | `.trim()` |
| Sensor kata kasar | `.replace(kasar, "***")` |
| Generate slug URL | `judul.toLowerCase().split(" ").join("-")` |

---

## 6. Kesalahan Umum

❌ **Lupa bahwa string bersifat immutable**
```javascript
let s = "Hello";
s.toUpperCase(); // "HELLO" — tapi s MASIH "Hello"!
s = s.toUpperCase(); // ✅ reassign agar berubah
```

❌ **Salah menggunakan `.slice()` dengan indeks negatif**
```javascript
"Hello".slice(-3);    // "llo" (3 dari belakang) — ini benar
"Hello".slice(-3, -1); // "ll" (dari -3 sampai -1)
```

❌ **Lupa escap sequence di dalam string kutip sama**
```javascript
let s = 'It's fine';   // ERROR
let s2 = "Dia berkata "halo""; // ERROR
// Benar:
let s3 = "It's fine";
let s4 = 'Dia berkata "halo"';
let s5 = `It's "fine" and "dandy"`;
```

❌ **Mengira `.split()` mengubah string asli**
```javascript
let s = "a,b,c";
s.split(",");  // ["a","b","c"] — s tetap "a,b,c"
// .split() mengembalikan array baru, tidak mengubah string
```

---

## 7. Benang Merah

```
Materi 12 (Operator — termasuk + untuk concatenation string)
    ↓
Materi 13 (String — tipe data utama untuk teks, method esensial) ← KAMU DI SINI
    ↓
Materi 14 (Input & Output — mengaplikasikan string ke interaksi dengan user)
```

Operator `+` bisa menggabungkan string, tapi dunia string jauh lebih luas dari sekadar concatenation. Metode seperti `.split()`, `.slice()`, `.trim()` adalah senjata utama saat bekerja dengan teks. Selanjutnya kita akan memasukkan dan mengeluarkan string lewat **I/O** (console & readline).

---

## 8. Soal

### Soal 1 (Mudah)
Apa output dari:
```javascript
let teks = "  JavaScript Itu Seru  ";
console.log(teks.trim());
console.log(teks.trim().length);
console.log(teks.trim().toLowerCase());
```

<details>
<summary>Jawaban</summary>

```
"JavaScript Itu Seru"
19 (karakter setelah trim)
"javascript itu seru"
```
</details>

---

### Soal 2 (Sedang)
Buat ekspresi untuk mengambil **inisial** dari nama "Budi Hartono" → `"BH"`.

<details>
<summary>Jawaban</summary>

```javascript
const nama = "Budi Hartono";
const inisial = nama
  .split(" ")
  .map(kata => kata.charAt(0).toUpperCase())
  .join("");

console.log(inisial); // "BH"
```
</details>

---

### Soal 3 (Tantangan)
Buat fungsi `reverseWords(kalimat)` yang membalik urutan kata — bukan huruf.

Contoh: `reverseWords("saya belajar JavaScript")` → `"JavaScript belajar saya"`

<details>
<summary>Jawaban</summary>

```javascript
function reverseWords(kalimat) {
  return kalimat.trim().split(" ").reverse().join(" ");
}

console.log(reverseWords("saya belajar JavaScript"));  // "JavaScript belajar saya"
console.log(reverseWords("satu dua tiga"));            // "tiga dua satu"
console.log(reverseWords("  halo dunia  "));           // "dunia halo"
```
</details>
