# 34. Hoisting — Perilaku Unik JavaScript

---

## 1. Penjelasan

**Hoisting** adalah mekanisme JavaScript di mana deklarasi variabel dan fungsi "diangkat" ke atas scope-nya **sebelum** kode dieksekusi. Yang diangkat hanya **deklarasi**, bukan **inisialisasi** (assignment).

Perbedaan hoisting:

| Deklarasi | Hoisted? | Nilai Awal | Akses Sebelum Deklarasi |
|-----------|----------|------------|------------------------|
| `var x` | ✅ Ya | `undefined` | ✅ undefined (tidak error) |
| `let x` | ✅ Ya (tapi TDZ) | Tidak diinisialisasi | ❌ ReferenceError |
| `const x` | ✅ Ya (tapi TDZ) | Tidak diinisialisasi | ❌ ReferenceError |
| `function foo(){}` | ✅ Ya + **body** | Definisi fungsi | ✅ Bisa dipanggil |
| `const foo = () => {}` | ❌ Tidak sebagai fungsi | — | ❌ TDZ error |

---

## 2. Fungsi Hoisting

- **Memungkinkan** fungsi dipanggil SEBELUM dideklarasikan (function declaration)
- **Mencegah** error tak terduga dengan memunculkan `undefined` (var)
- **Membantu** memahami mengapa urutan deklarasi penting (let/const)
- **Membedakan** function declaration vs expression

---

## 3. Code — Prediksi Output + Verifikasi

### Kode 1: Function Declaration di-hoist
```javascript
sayHalo(); // "Halo!"
function sayHalo() {
    console.log("Halo!");
}
```

### Kode 2: Function Expression TIDAK di-hoist
```javascript
sayHai(); // ❌ TypeError: sayHai is not a function
var sayHai = function() {
    console.log("Hai!");
};
```

### Kode 3: Hoisting var
```javascript
console.log(a); // undefined
var a = 10;
```
**Eksekusi sebenarnya:**
```javascript
var a;
console.log(a); // undefined
a = 10;
```

### Kode 4: let — TDZ
```javascript
console.log(b); // ❌ ReferenceError
let b = 20;
```

### Kode 5: Function declaration setelah return
```javascript
function test() {
    return double(5);
    function double(n) { return n * 2; }
}
console.log(test()); // 10 (hoisting berhasil)
```

### Kode 6: Hoisting dalam blok
```javascript
console.log(c); // ❌ ReferenceError (TDZ)
const c = 30;
```

### Kode 7: var dalam blok di-hoist ke fungsi
```javascript
function demo() {
    console.log(d); // undefined (hoisted ke atas fungsi)
    if (true) {
        var d = 40;
    }
}
demo();
```

### Kode 8: Fungsi dalam blok (strict vs non-strict)
```javascript
"use strict";
if (true) {
    function foo() { return 1; }
}
console.log(foo); // ❌ ReferenceError di strict mode
```

### Kode 9: Class tidak di-hoist
```javascript
const obj = new Person(); // ❌ ReferenceError
class Person {}
```

### Kode 10: Prioritas hoisting — fungsi vs var
```javascript
console.log(typeof foo); // "function" (fungsi di-hoist lebih tinggi)
var foo = "string";
function foo() { return 1; }
```

---

## 4. Analogi Rumah

| Konsep | Analogi Rumah |
|--------|---------------|
| Hoisting | **Tukang naikin semua material ke lantai 2** sebelum mulai kerja |
| Function declaration | Tukang bawa **lemari utuh** — langsung bisa dipakai |
| `var` | Tukang bawa **rangka lemari** (kelihatan tapi kosong = undefined) |
| `let`/`const` | Tukang bawa **lemari terbungkus plastik** (ada tapi belum bisa dibuka = TDZ) |
| Function expression | Tukang bawa **papan kayu** — harus dirakit dulu jadi lemari |
| TDZ | Masa antara material diangkat dan benar-benar siap dipasang |

**Narasi:** Bayangin tukang naikin semua bahan bangunan ke lantai 2 sebelum mulai kerja. Lemari utuh (function declaration) langsung bisa dipakai. Rangka lemari (var) kelihatan tapi kosong. Lemari terbungkus plastik (let/const) ada tapi belum bisa dibuka. Papan kayu (function expression) harus dirakit dulu. Masa ketika material sudah di lantai 2 tapi belum siap pakai itulah TDZ.

---

## 5. Use Case

- **Function declaration** di hoist → bisa diletakkan di **bawah** kode yang menggunakannya (lebih bersih, seperti "daftar isi")
- **let/const** → memaksa programmer mendeklarasi SEBELUM pakai (kode lebih rapi, lebih aman)
- **var** → dihindari karena perilaku hoisting yang membingungkan

---

## 6. Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|-----------|--------|--------|
| Anggap function expression seperti declaration | `foo()` sebelum `const foo = () => {}` | TypeError |
| Lupa TDZ let/const | Akses variabel sebelum deklarasi di blok | ReferenceError |
| Kira var di-hoist dengan nilai | `console.log(x); var x = 5;` → undefined | Bug logika |
| Hoisting class | `new Person()` sebelum `class Person {}` | ReferenceError |
| Override fungsi dengan var | Fungsi tiba-tiba jadi string | Bug runtime |

```javascript
// Contoh override berbahaya:
var myFunc = 5;
function myFunc() { return 1; }
console.log(typeof myFunc); // "number" — fungsi sudah ketimpa!
```

---

## 7. Benang Merah

Materi 33 (scope + var/let/const) → **Materi 34 (hoisting: bagaimana deklarasi "diangkat")** → Materi 35 (closure: bagaimana fungsi "mengingat" scope saat diciptakan)

Hoisting adalah mekanisme internal JavaScript. Closure adalah bagaimana fungsi mempertahankan akses ke scope di mana ia **diciptakan** — bukan di mana ia **dipanggil**.

---

## 8. Soal

### Soal 1 (Mudah)
Apa outputnya?
```javascript
console.log(x);
var x = 5;
console.log(x);
```

<details>
<summary>Jawaban</summary>
`undefined`, lalu `5`. var x di-hoist ke atas → `undefined`, lalu assignment `x = 5`.
</details>

### Soal 2 (Sedang)
Apa outputnya?
```javascript
foo();
var foo = function() {
    console.log("A");
};
function foo() {
    console.log("B");
}
foo();
```

<details>
<summary>Jawaban</summary>
"B", lalu "A". Function declaration di-hoist duluan, lalu var foo (tapi var tidak timpa fungsi karena fungsi sudah ada). Setelah assignment, `foo` jadi function expression.
</details>

### Soal 3 (Sulit)
Apa output kode berikut? Jelaskan!
```javascript
let a = 1;
{
    console.log(a);
    let a = 2;
}
```

<details>
<summary>Jawaban</summary>
ReferenceError: Cannot access 'a' before initialization.
Di dalam blok, `let a = 2` di-hoist ke atas blok. Karena TDZ, `console.log(a)` tidak bisa mengakses `a` dari blok luar — `a` lokal sudah "mengambil alih" scope blok tapi belum diinisialisasi. Fenomena ini disebut **temporal dead zone**.
</details>
