# 33. Scope — Local, Global & Block Scope

---

## 1. Penjelasan

**Scope** adalah "wilayah akses" suatu variabel — di mana variabel bisa diakses dan di mana tidak. JavaScript memiliki tiga jenis scope:

| Scope | Tempat Dideklarasikan | Bisa Diakses Dari |
|-------|----------------------|-------------------|
| **Global** | Di luar semua fungsi/kurung kurawal | Mana saja |
| **Function (Local)** | Di dalam fungsi dengan `var` | Hanya dalam fungsi itu |
| **Block** | Di dalam `{}` dengan `let`/`const` | Hanya dalam blok itu |

Perilaku `var` vs `let`/`const` dalam scope:
- `var` — **function-scoped**, tidak peduli blok
- `let`/`const` — **block-scoped**, peduli kurung kurawal
- `let`/`const` memiliki **Temporal Dead Zone (TDZ)**: error jika diakses sebelum deklarasi dalam blok yang sama

---

## 2. Fungsi Scope

- **Mengisolasi** variabel agar tidak saling tabrak antar bagian kode
- **Melindungi** data internal fungsi/objek
- **Mencegah** polusi namespace global
- **Memungkinkan** closure bekerja (materi 35)

---

## 3. Code — Debug 5 Kode dengan Masalah Scope

### Kode 1: `var` tidak block-scoped
```javascript
if (true) {
    var x = 10;  // var — bocor ke global!
}
console.log(x);   // 10 (seharusnya error!)
```

### Kode 2: `let` block-scoped
```javascript
if (true) {
    let y = 20;
}
console.log(y);   // ❌ ReferenceError: y is not defined
```

### Kode 3: TDZ dengan `let`
```javascript
console.log(a);   // ❌ ReferenceError: Cannot access 'a' before initialization
let a = 5;
```

### Kode 4: TDZ dengan `const` (sama)
```javascript
console.log(b);   // ❌ ReferenceError
const b = 10;
```

### Kode 5: `var` tidak punya TDZ (hoisting)
```javascript
console.log(c);   // undefined (bukan error — hoisting!)
var c = 15;
```

---

## 4. Analogi Rumah

| Konsep | Analogi Rumah |
|--------|---------------|
| Global scope | **Ruang tamu** — semua penghuni bisa akses |
| Function scope | **Kamar tidur** — hanya penghuni kamar itu |
| Block scope | **Laci dalam lemari** — hanya kode dalam blok itu |
| `var` | Lemari yang **pintunya selalu terbuka** |
| `let`/`const` | Lemari yang **terkunci** untuk kamar itu saja |
| TDZ | **Lorong gelap** sebelum lampu dinyalakan |

**Narasi:** Dalam rumah, ruang tamu (global) bisa dipakai semua orang. Tapi kamar tidur (function scope) hanya untuk penghuninya. Di dalam kamar ada lemari (block scope) yang hanya terbuka dari dalam kamar. `var` seperti lemari yang pintunya ke ruang tamu juga — tidak aman. `let`/`const` seperti lemari berkunci. TDZ adalah masa antara Anda masuk kamar dan menyalakan lampu — Anda tahu ada lemari tapi belum bisa lihat isinya.

---

## 5. Use Case

- **Global scope**: Konfigurasi aplikasi, environment variable
- **Function scope**: Variabel helper dalam fungsi yang tidak perlu bocor ke luar
- **Block scope**: Variabel sementara dalam loop `for (let i = 0; ...)` agar tiap iterasi punya `i` sendiri

---

## 6. Kesalahan Umum

| Kesalahan | Contoh | Dampak |
|-----------|--------|--------|
| Lupa deklarasi | `x = 5` (tanpa keyword) | Jadi global otomatis (strict mode: error) |
| `var` dalam loop | `for (var i...){...}` | `i` bocor ke luar |
| Akses sebelum deklarasi | `console.log(x); let x = 1;` | TDZ error |
| Shadowing tak sengaja | Parameter fungsi sama nama dengan global | Global ketutup (shadowed) |
| Nested function akses var | Inner function pakai `var` outer | Masih bisa akses (closure) |

---

## 7. Benang Merah

Materi 32 (fungsi dasar) → **Materi 33 (scope: fungsi punya scope sendiri)** → Materi 34 (hoisting: bagaimana var/let/const diangkat)

Scope adalah fondasi yang memungkinkan closure (Materi 35), this keyword (Materi 36), dan modules (Materi 40) bekerja.

---

## 8. Soal

### Soal 1 (Mudah)
Apa output kode berikut?
```javascript
let a = 1;
if (true) {
    let a = 2;
    console.log(a);
}
console.log(a);
```

<details>
<summary>Jawaban</summary>
2, lalu 1. Block scope: `a` di dalam `if` berbeda dengan `a` di luar.
</details>

### Soal 2 (Sedang)
Apa outputnya?
```javascript
var x = 1;
function foo() {
    console.log(x);
    var x = 2;
}
foo();
```

<details>
<summary>Jawaban</summary>
`undefined`. Karena hoisting, var x diangkat ke atas scope fungsi, tapi belum di-assign.
</details>

### Soal 3 (Sulit)
Apa yang terjadi?
```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}
```
Bagaimana cara memperbaikinya?

<details>
<summary>Jawaban</summary>
Output: 3, 3, 3. Karena `var i` function-scoped, setelah loop selesai `i = 3`. Semua setTimeout membaca `i` yang sama.
Perbaikan: ganti `var i` jadi `let i` (block-scoped, tiap iterasi punya binding sendiri).
</details>
