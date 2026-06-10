# 11 — Type Coercion & `==` vs `===`

---

## 1. Penjelasan

**Type coercion** adalah perubahan tipe data secara **otomatis** oleh JavaScript saat operasi melibatkan dua tipe berbeda.

Dua jenis coercion:

- **Implicit coercion**: JS mengubah tipe di belakang layar tanpa diminta.
- **Explicit coercion**: Programmer sengaja mengubah tipe (misal: `Number("5")`).

### Perbandingan:

| Operator | Aturan |
|----------|--------|
| `==` (loose equality) | Lakukan coercion **dulu**, baru bandingkan |
| `===` (strict equality) | Langsung bandingkan — **tanpa coercion** |

### Quirks Terkenal:

| Ekspresi | Hasil | Alasan |
|----------|-------|--------|
| `"5" + 3` | `"53"` | `+` dengan string → **concatenation** |
| `"5" - 3` | `2` | `-` hanya untuk angka → coercion ke number |
| `null == undefined` | `true` | Aturan khusus: mereka dianggap sama (`==`) |
| `null === undefined` | `false` | Tipe berbeda |
| `" " == 0` | `true` | String kosong spasi dikonversi ke 0 |
| `[] == ![]` | `true` | ... ya, JavaScript aneh |

---

## 2. Fungsi

- **`==`**: Dipakai saat kita ingin membandingkan nilai saja tanpa peduli tipe (jarang disarankan).
- **`===`**: Dipakai untuk perbandingan ketat — nilai **dan** tipe harus sama (**standar utama**).
- Memahami coercion penting untuk **debugging** dan membaca kode orang lain.

> Aturan praktik: **Selalu pakai `===`, kecuali kamu benar-benar paham kenapa pakai `==`.**

---

## 3. Code (20 Ekspresi Coercion)

```javascript
// === 20 EKSPRESI COERCION — coba tebak hasilnya! ===

const ekspresi = [
  // ---- STRING + ANGKA ----
  '"5" + 3',              // 1
  '5 + "3"',              // 2
  '"5" - 3',              // 3
  '"5" * "3"',            // 4
  '"hello" - 1',          // 5

  // ---- == vs === ----
  '5 == "5"',             // 6
  '5 === "5"',            // 7
  '0 == false',           // 8
  '0 === false',          // 9
  '"" == false',          // 10

  // ---- null & undefined ----
  'null == undefined',    // 11
  'null === undefined',   // 12
  'null == 0',            // 13
  'undefined == 0',       // 14

  // ---- QUIRKS ----
  '" " == 0',             // 15
  '[] == false',          // 16
  '[] == ![]',            // 17
  '"5" + null',           // 18
  'true + true',          // 19
  '"10" - "5" + "3"',     // 20
];

ekspresi.forEach((e, i) => {
  try {
    console.log(`${String(i + 1).padStart(2)}. ${e.padEnd(20)} → ${eval(e)}`);
  } catch (err) {
    console.log(`${String(i + 1).padStart(2)}. ${e.padEnd(20)} → ERROR`);
  }
});
```

### Hasil (verifikasi):

```
 1. "5" + 3              → 53
 2. 5 + "3"              → 53
 3. "5" - 3              → 2
 4. "5" * "3"            → 15
 5. "hello" - 1          → NaN
 6. 5 == "5"             → true
 7. 5 === "5"            → false
 8. 0 == false           → true
 9. 0 === false          → false
10. "" == false          → true
11. null == undefined    → true
12. null === undefined   → false
13. null == 0            → false
14. undefined == 0       → false
15. " " == 0             → true
16. [] == false          → true
17. [] == ![]            → true
18. "5" + null           → "5null"
19. true + true          → 2
20. "10" - "5" + "3"     → "53"
```

---

## 4. Analogi Rumah (Membangun Rumah)

**Type coercion = memaksakan material yang berbeda agar bisa disambung.**

| Ekspresi JS | Analogi di Rumah |
|-------------|------------------|
| `"5" + 3` → `"53"` | Menempelkan papan angka 3 di samping papan 5 → "53" |
| `"5" - 3` → `2` | Gergaji hanya untuk kayu, papan "5" diukur ulang jadi angka 5, lalu dipotong 3 → sisa 2 |
| `==` | "Kira-kira cocok" — paku 5cm dan paku "5cm" ya sama saja |
| `===` | "Harus persis" — paku 5cm (besi) berbeda dengan paku 5cm (kayu) |
| `null == undefined` | Keranjang kosong sengaja (`null`) dan keranjang belum diisi (`undefined`) dianggap sama longgar |
| `[] == ![]` | Lemari kosong dianggap sama dengan "bukan lemari" — di dunia bangunan ini kacau balau |

> **Narasi**: Saat membangun rumah, kamu mungkin perlu menyambung besi dengan kayu. Kadang bisa dipaksakan (`==`), kadang hasilnya aneh (`"5" + null` jadi `"5null"` seperti menempelkan papan nama di atas ember kosong). Tukang bangunan yang baik selalu pakai material yang **benar-benar cocok** (`===`). Aturan emas: jangan biarkan JavaScript menebak-nebak. Kamu harus eksplisit.

---

## 5. Use Case

| Situasi | Pendekatan yang Benar |
|---------|-----------------------|
| Membandingkan input form (`"5"` vs `5`) | Konversi dulu: `Number(input) === 5` |
| Cek null/undefined sekaligus | `data == null` (satu-satunya use case `==` yang wajar) |
| Operasi aritmatika dari string | Eksplisit: `Number("5") + 3` |
| Concatenation number + string | Eksplisit: `"Harga: " + String(harga)` |

---

## 6. Kesalahan Umum

❌ **Membandingkan input form dengan `==`**
```javascript
const input = "5"; // dari form
if (input == 5) { /* true — tapi tersembunyi */ }
// Lebih baik: Number(input) === 5
```

❌ **Mengira `==` selalu lebih "toleran" dengan null**
```javascript
null == 0          // false — null tidak coercion ke 0!
null == undefined  // true  — aturan khusus
```

❌ **Menjumlahkan angka tanpa sadar jadi string**
```javascript
let total = 10;
total = total + "" + 5; // "105", bukan 15
// Maksudnya total += 5
```

❌ **Mengira semua object falsy**
```javascript
if ([]) console.log("jalan"); // Array kosong truthy!
```

---

## 7. Benang Merah

```
Materi 10 (Tipe Data — isi wadah)
    ↓
Materi 11 (Type Coercion — bagaimana JS otomatis mengubah isi wadah saat operasi) ← KAMU DI SINI
    ↓
Materi 12 (Operator — alat untuk mengolah isi wadah)
```

Tipe data adalah jenis isi wadah. Sekarang kita paham bahwa saat dua wadah berisi material berbeda dicampur, JavaScript punya cara "kreatif" untuk menggabungkannya (terkadang kacau). Selanjutnya kita belajar **operator** — alat-alat seperti palu, gergaji, meteran — untuk mengolah material tersebut.

---

## 8. Soal

### Soal 1 (Mudah)
Apa output dari:
```javascript
console.log(10 + "20");
console.log("10" - 20);
console.log("10" - "5");
```

<details>
<summary>Jawaban</summary>

```
"1020"   (string concatenation karena ada string)
-10      ("10" dic coercion ke number: 10 - 20 = -10)
5        (kedua string dic coercion ke number: 10 - 5 = 5)
```
</details>

---

### Soal 2 (Sedang)
Apa perbedaan output dari kedua baris ini?
```javascript
console.log(5 == "5");
console.log(5 === "5");
```

<details>
<summary>Jawaban</summary>

`5 == "5"` → `true` (setelah coercion, nilai sama-sama 5).
`5 === "5"` → `false` (tipe number vs string — tanpa coercion).
</details>

---

### Soal 3 (Tantangan)
Tanpa menjalankan, tebak output:
```javascript
console.log([] + []);
console.log([] + {});
console.log({} + []);
```

<details>
<summary>Jawaban</summary>

1. `[] + []` → `""` (array kosong dikonversi ke string kosong, digabung)
2. `[] + {}` → `"[object Object]"` (array → `""`, object → `"[object Object]"`)
3. `{} + []` → `0` atau `"[object Object]"` tergantung konteks (ambigu!)
   - Jika dianggap blok kode: `{}` adalah blok kosong, lalu `+ []` → `0`
   - Jika dianggap ekspresi: sama seperti no.2

Ini sebabnya banyak developer bilang type coercion JS itu **quirky** — dan kenapa `===` hampir selalu lebih aman.
</details>
