# 10 — Tipe Data Primitif JavaScript: 7 Tipe

---

## 1. Penjelasan

JavaScript memiliki **7 tipe data primitif**: `string`, `number`, `boolean`, `null`, `undefined`, `symbol`, `bigint`.

Semua tipe selain primitif disebut **object** (tipe referensi). Sifat utama primitif: **immutable** (tak bisa diubah) dan **dioperasikan berdasarkan nilai** (pass by value).

JavaScript bersifat **dynamically typed**: satu variabel bisa menyimpan tipe data apa pun dan bisa berubah kapan pun.

### 6 Falsy Values
Hanya 6 nilai yang dianggap `false` dalam konteks boolean:
1. `false`
2. `0`
3. `""` (string kosong)
4. `null`
5. `undefined`
6. `NaN`

Semua nilai lain adalah **truthy**.

---

## 2. Fungsi

| Tipe | Kegunaan |
|------|----------|
| `string` | Menyimpan teks (nama, pesan, alamat) |
| `number` | Menyimpan angka (harga, usia, ukuran) |
| `boolean` | Menyimpan kondisi true/false (saklar) |
| `null` | Menandakan nilai kosong yang disengaja |
| `undefined` | Nilai default variabel yang belum diisi |
| `symbol` | Membuat identifier unik (properti object) |
| `bigint` | Angka > 2^53 (kripto, timestamp, ID besar) |

**`typeof` operator** dipakai untuk mengecek tipe data sebuah nilai.

---

## 3. Code

```javascript
// === 7 TIPE DATA PRIMITIF ===

// 1. String
const nama = "Budi";
console.log(typeof nama);          // "string"

// 2. Number
const usia = 25;
const pi = 3.14;
console.log(typeof usia);          // "number"
console.log(typeof pi);            // "number"

// 3. Boolean
const isMenikah = false;
console.log(typeof isMenikah);     // "boolean"

// 4. Null
const halamanSaatIni = null;
console.log(typeof halamanSaatIni); // "object" (BUG sejarah JS!)

// 5. Undefined
let alamat;
console.log(typeof alamat);        // "undefined"

// 6. Symbol
const idUnik = Symbol("id");
console.log(typeof idUnik);        // "symbol"

// 7. BigInt
const angkaBesar = 9007199254740991n;
console.log(typeof angkaBesar);    // "bigint"


// === 15 EKSPRESI typeof ===

console.log(typeof "Halo");          // 1. "string"
console.log(typeof 42);              // 2. "number"
console.log(typeof 3.14);            // 3. "number"
console.log(typeof true);            // 4. "boolean"
console.log(typeof false);           // 5. "boolean"
console.log(typeof null);            // 6. "object"  (quirks JS)
console.log(typeof undefined);       // 7. "undefined"
console.log(typeof Symbol("x"));     // 8. "symbol"
console.log(typeof 100n);            // 9. "bigint"
console.log(typeof NaN);             // 10. "number" (NaN tetap number)
console.log(typeof Infinity);        // 11. "number"
console.log(typeof "123");           // 12. "string"
console.log(typeof typeof 42);       // 13. "string" (typeof hasilnya string)
console.log(typeof (5 > 3));         // 14. "boolean"
console.log(typeof [1, 2, 3]);       // 15. "object" (array adalah object)


// === TRUTHY & FALSY ===

const falsyList = [false, 0, "", null, undefined, NaN];
falsyList.forEach(val => {
  if (!val) console.log(`${val} adalah falsy`);
});

// Semua string isi adalah truthy
if ("false") console.log('"false" (string) adalah truthy!');
if ("0") console.log('"0" (string) adalah truthy!');
```

---

## 4. Analogi Rumah (Membangun Rumah)

**Setiap tipe data = jenis material bangunan.**

| Tipe Data | Analogi di Rumah |
|-----------|------------------|
| `string` | Papan nama / tulisan di denah |
| `number` | Ukuran meter (3m, 5m, 12.5m²) |
| `boolean` | Saklar lampu (ON/OFF) |
| `null` | Keranjang kosong — sengaja dikosongkan |
| `undefined` | Tempat semen yang belum diisi — belum diputuskan |
| `symbol` | Kunci unik tiap pintu (tak ada duplikat) |
| `bigint` | Luas tanah dalam skala sangat besar (hektar) |

> **Narasi**: Ibarat membangun rumah, kamu perlu berbagai material. Papan nama (`string`) untuk menulis alamat. Angka (`number`) untuk mengukur panjang, lebar, tinggi. Saklar (`boolean`) hanya dua kondisi: nyala atau mati. Ada kalanya kamu sengaja mengosongkan ember (`null`) dan ada kalanya kamu belum memutuskan apa yang akan dimasukkan (`undefined`). `Symbol` seperti kunci unik—setiap pintu punya kunci berbeda. `BigInt` seperti menghitung luas tanah dalam satuan sangat besar. `typeof` seperti alat detektor material—kamu sentuhkan ke benda, alat itu bilang "ini kayu", "ini batu", "ini besi".

---

## 5. Use Case

| Situasi | Tipe Data Dipakai |
|---------|------------------|
| Menyimpan nama user | `string` |
| Menghitung total belanja | `number` |
| Cek apakah user sudah login | `boolean` |
| Data masih kosong dari server | `null` |
| Variabel belum diinisialisasi | `undefined` |
| Property object unik (tidak tertimpa) | `symbol` |
| ID transaksi dengan angka sangat besar | `bigint` |

---

## 6. Kesalahan Umum

❌ **Mengira `typeof null` adalah `null`**
```javascript
typeof null; // "object" — ini bug JS dari awal, tidak akan diperbaiki
```

❌ **Mengira NaN itu "Not a Number" berarti bukan tipe number**
```javascript
typeof NaN; // "number" — NaN secara teknis tetap bertipe number
```

❌ **Salah paham dynamic typing sebagai "tanpa tipe"**
```javascript
let x = "halo";
x = 42; // ✅ valid, tapi bisa membingungkan kalau tak sengaja
```

❌ **Lupa bahwa string kosong itu falsy**
```javascript
let nama = "";
if (nama) {
  // Baris ini TIDAK akan dijalankan
}
```

---

## 7. Benang Merah

```
Materi 9 (Variabel sebagai wadah)
    ↓
Materi 10 (Isi wadah = Tipe Data Primitif) ← KAMU DI SINI
    ↓
Materi 11 (Type Coercion — bagaimana JS mengubah isi wadah secara otomatis)
```

Variabel adalah lahan/wadah untuk menaruh sesuatu. Sekarang kita sudah tahu **apa saja** yang bisa ditaruh di dalamnya (7 tipe primitif). Selanjutnya kita akan lihat bagaimana JavaScript **mengubah tipe secara otomatis** (type coercion) dan mengapa kadang ini berbahaya.

---

## 8. Soal

### Soal 1 (Mudah)
Apa output dari kode berikut?
```javascript
console.log(typeof "JavaScript");
console.log(typeof 2024);
console.log(typeof true);
```

<details>
<summary>Jawaban</summary>

```
"string"
"number"
"boolean"
```
</details>

---

### Soal 2 (Sedang)
Manakah dari nilai berikut yang **falsy**?
```javascript
0, "0", false, "false", null, undefined, NaN, [], {}
```

<details>
<summary>Jawaban</summary>

Falsy: `0`, `false`, `null`, `undefined`, `NaN`.

`"0"` dan `"false"` adalah string (truthy — karena string isi apa pun truthy selama tidak kosong).
Array `[]` dan object `{}` juga truthy (object selalu truthy).
</details>

---

### Soal 3 (Tantangan)
Perbaiki kode berikut yang salah dalam mengecek apakah sebuah variabel bertipe **null**:
```javascript
let data = null;
if (typeof data === "null") {
  console.log("Data kosong");
}
```

<details>
<summary>Jawaban</summary>

`typeof null` menghasilkan `"object"` (bug JS), bukan `"null"`. Cara yang benar adalah:

```javascript
let data = null;
if (data === null) {
  console.log("Data kosong");
}
```

Atau jika ingin general (null atau undefined):
```javascript
if (data == null) {
  console.log("Data null atau undefined");
}
```
</details>
