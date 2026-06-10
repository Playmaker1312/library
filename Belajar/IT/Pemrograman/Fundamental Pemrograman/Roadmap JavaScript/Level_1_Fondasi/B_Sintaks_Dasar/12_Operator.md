# 12 — Operator: Aritmatika, Perbandingan, Logika, Assignment

---

## 1. Penjelasan

**Operator** adalah simbol yang melakukan operasi pada satu atau lebih nilai (operand).

### Kategori:

| Kategori | Operator |
|----------|----------|
| **Aritmatika** | `+`, `-`, `*`, `/`, `%`, `**` |
| **Perbandingan** | `>`, `<`, `>=`, `<=`, `===`, `!==` |
| **Logika** | `&&`, `||`, `!`, `??` |
| **Assignment** | `=`, `+=`, `-=`, `*=`, `/=`, `%=` |
| **Ternary** | `kondisi ? nilai1 : nilai2` |

### Operator `??` (Nullish Coalescing)
Mengembalikan operand kanan jika operand kiri `null` atau `undefined`.

---

## 2. Fungsi

| Operator | Fungsi |
|----------|--------|
| `+` | Penjumlahan / concatenation |
| `-` | Pengurangan |
| `*` | Perkalian |
| `/` | Pembagian |
| `%` | Sisa bagi (modulo) — bagus untuk cek ganjil/genap |
| `**` | Eksponensial (pangkat) |
| `>` `<` `>=` `<=` | Membandingkan besar/kecil |
| `===` `!==` | Membandingkan kesetaraan ketat |
| `&&` | DAN logika (semua true) |
| `||` | ATAU logika (salah satu true) |
| `!` | NEGASI (membalik boolean) |
| `??` | Nullish coalescing (fallback untuk null/undefined) |
| `+=` dll | Operator assignment gabungan |
| `(cond) ? a : b` | If-else dalam satu baris |

---

## 3. Code — Kalkulator Ekspresi Kompleks

```javascript
// === KALKULATOR EKSPRESI KOMPLEKS ===

function kalkulator(a, b) {
  console.log("Nilai awal: a =", a, ", b =", b);

  // Aritmatika
  console.log("--- ARITMATIKA ---");
  console.log(`${a} + ${b} = ${a + b}`);
  console.log(`${a} - ${b} = ${a - b}`);
  console.log(`${a} * ${b} = ${a * b}`);
  console.log(`${a} / ${b} = ${a / b}`);
  console.log(`${a} % ${b} = ${a % b}`);
  console.log(`${a} ** ${b} = ${a ** b}`);

  // Perbandingan
  console.log("--- PERBANDINGAN ---");
  console.log(`${a} > ${b}  = ${a > b}`);
  console.log(`${a} < ${b}  = ${a < b}`);
  console.log(`${a} >= ${b} = ${a >= b}`);
  console.log(`${a} <= ${b} = ${a <= b}`);
  console.log(`${a} === ${b} = ${a === b}`);
  console.log(`${a} !== ${b} = ${a !== b}`);

  // Logika
  console.log("--- LOGIKA ---");
  console.log(`(${a} > 0) && (${b} > 0) = ${(a > 0) && (b > 0)}`);
  console.log(`(${a} > 0) || (${b} > 0) = ${(a > 0) || (b > 0)}`);
  console.log(`!(${a} > 0) = ${!(a > 0)}`);

  // Ternary
  console.log("--- TERNARY ---");
  console.log(`${a} ${a % 2 === 0 ? "genap" : "ganjil"}`);
  console.log(`${b} ${b % 2 === 0 ? "genap" : "ganjil"}`);

  // Nullish Coalescing
  console.log("--- NULLISH COALESCING ---");
  let x = null;
  let y = undefined;
  let z = 10;
  console.log(`null ?? "default"   = ${null ?? "default"}`);
  console.log(`undefined ?? "default" = ${undefined ?? "default"}`);
  console.log(`${z} ?? "default"    = ${z ?? "default"}`);

  // Assignment gabungan
  console.log("--- ASSIGNMENT GABUNGAN ---");
  let c = a;
  c += b;
  console.log(`a += b → ${c}`);
  c -= b;
  console.log(`c -= b → ${c}`);
  c *= b;
  console.log(`c *= b → ${c}`);
  c /= b;
  console.log(`c /= b → ${c}`);
}

kalkulator(10, 3);
```

### Output:

```
Nilai awal: a = 10 , b = 3
--- ARITMATIKA ---
10 + 3 = 13
10 - 3 = 7
10 * 3 = 30
10 / 3 = 3.333...
10 % 3 = 1
10 ** 3 = 1000
--- PERBANDINGAN ---
10 > 3  = true
10 < 3  = false
10 >= 3 = true
10 <= 3 = false
10 === 3 = false
10 !== 3 = true
--- LOGIKA ---
(10 > 0) && (3 > 0) = true
(10 > 0) || (3 > 0) = true
!(10 > 0) = false
--- TERNARY ---
10 genap
3 ganjil
--- NULLISH COALESCING ---
null ?? "default"   = default
undefined ?? "default" = default
10 ?? "default"    = 10
--- ASSIGNMENT GABUNGAN ---
a += b → 13
c -= b → 10
c *= b → 30
c /= b → 10
```

---

## 4. Analogi Rumah (Membangun Rumah)

**Operator = alat tukang bangunan.**

| Operator | Alat Tukang | Fungsi di Rumah |
|----------|-------------|-----------------|
| `+` | Gabung material | Menyambung 2 papan |
| `-` | Gergaji | Memotong kayu |
| `*` | Kali luas | Menghitung luas ruangan (p x l) |
| `/` | Bagi | Membagi tanah jadi beberapa bagian |
| `%` | Sisa potongan | Sisa papan setelah dipotong |
| `**` | Skala | Menghitung volume kubik (s³) |
| `>` `<` | Waterpass / meteran | Memeriksa apakah papan A lebih panjang dari B |
| `===` | Jangka sorong | Memeriksa apakah dua papan identik |
| `&&` | Kunci ganda | "Pintu terbuka **dan** lampu nyala" |
| `||` | Salah satu | "Pintu terbuka **atau** jendela terbuka" |
| `!` | Kebalikan | "**Tidak** hujan" |
| `? :` | Keputusan cepat | "Jika hujan → pakai atap, jika tidak → biarkan terbuka" |
| `??` | Cadangan | "Jika papan utama kosong, pakai papan cadangan" |

> **Narasi**: Tukang bangunan punya kotak alat. Ingin menggabungkan dua papan? Pakai `+`. Ingin memotong kayu? Pakai `-`. Mengecek apakah dua baut sama persis? Pakai `===`. Membuat keputusan "Jika ini, maka itu"? Pakai ternary `?:`. Alat-alat ini dipakai setiap hari — sama seperti operator dipakai di setiap baris kode JavaScript.

---

## 5. Use Case

| Situasi | Operator |
|---------|----------|
| Hitung diskon harga | `harga * (1 - diskon/100)` |
| Cek bilangan genap | `angka % 2 === 0` |
| Validasi akses (login AND admin) | `isLogin && isAdmin` |
| Fallback nama default | `nama ?? "Tamu"` |
| Grade nilai | `nilai >= 80 ? "A" : "B"` |
| Increment counter | `count += 1` atau `count++` |

---

## 6. Kesalahan Umum

❌ **Tertukar `=` dengan `===`**
```javascript
if (x = 5) { // BUKAN perbandingan! Ini assignment — selalu true!
```
> Assignment `x = 5` mengembalikan 5 (truthy), jadi kondisi selalu masuk.

❌ **Lupa bahwa `||` mengembalikan nilai, bukan boolean**
```javascript
let nama = "" || "Tamu"; // "" falsy, jadi "Tamu"
let umur = 0 || 25;      // 0 falsy, jadi 25 — MUNGKIN TIDAK DIINGINKAN
// Gunakan ?? jika hanya null/undefined:
let umur2 = 0 ?? 25;     // 0 (karena 0 bukan null/undefined)
```

❌ **Modulo dengan bilangan negatif**
```javascript
-5 % 3;  // -2 (bukan 1) — hasil mengikuti tanda pembilang
```

❌ **Salah prioritas operator (lupa kurung)**
```javascript
let hasil = 5 + 3 * 2;   // 11 (karena * dulu)
let hasil2 = (5 + 3) * 2; // 16
```

---

## 7. Benang Merah

```
Materi 11 (Type Coercion — bagaimana JS mengubah tipe saat operasi)
    ↓
Materi 12 (Operator — alat untuk mengolah data dengan semua operator) ← KAMU DI SINI
    ↓
Materi 13 (String — material paling penting untuk I/O, dan bagaimana operator + bekerja padanya)
```

Coercion adalah mekanisme di balik layar. Operator adalah alat yang kita pakai sehari-hari. Dari sini kita lanjut ke **string** — tipe data yang paling sering dioperasikan (concatenation, method chaining, template literals).

---

## 8. Soal

### Soal 1 (Mudah)
Apa output dari:
```javascript
console.log(15 % 4);
console.log(2 ** 5);
console.log(10 + "5" - 2);
```

<details>
<summary>Jawaban</summary>

```
3    (15 ÷ 4 = 3 sisa 3)
32   (2⁵ = 32)
52   (10 + "5" = "105", lalu "105" - 2 = 103)
```
</details>

---

### Soal 2 (Sedang)
Apa output dari:
```javascript
let a = 10;
let b = "10";
console.log(a == b);
console.log(a === b);
console.log(a !== b);
```

<details>
<summary>Jawaban</summary>

```
true    (== dengan coercion)
false   (=== tanpa coercion — number vs string)
true    (memang berbeda karena tipenya beda)
```
</details>

---

### Soal 3 (Tantangan)
Gunakan operator ternary dan `%` untuk membuat fungsi yang mengembalikan `"Fizz"` jika angka habis dibagi 3, `"Buzz"` jika habis dibagi 5, `"FizzBuzz"` jika habis dibagi 3 dan 5, dan angka itu sendiri jika tidak memenuhi semua.

<details>
<summary>Jawaban</summary>

```javascript
function fizzBuzz(n) {
  return (n % 3 === 0 && n % 5 === 0) ? "FizzBuzz"
       : (n % 3 === 0) ? "Fizz"
       : (n % 5 === 0) ? "Buzz"
       : n;
}

console.log(fizzBuzz(3));   // "Fizz"
console.log(fizzBuzz(5));   // "Buzz"
console.log(fizzBuzz(15));  // "FizzBuzz"
console.log(fizzBuzz(7));   // 7
```
</details>
