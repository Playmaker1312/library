# 37. First-Class Function & Higher-Order Function

---

## 1. Penjelasan

**First-Class Function** artinya fungsi diperlakukan **sebagai nilai** — sama seperti number, string, atau object. Fungsi bisa:

| Kemampuan | Contoh |
|-----------|--------|
| Disimpan ke variabel | `const fn = function() {};` |
| Dikirim sebagai argumen | `arr.map(fn)` |
| Dikembalikan dari fungsi | `return function() {};` |
| Disimpan dalam array/objek | `obj.method = fn;` |

**Higher-Order Function (HOF)** adalah fungsi yang:
- Menerima fungsi sebagai **argument**, ATAU
- Mengembalikan fungsi sebagai **return value**

> Semua HOF dimungkinkan karena JavaScript memiliki first-class function.

---

## 2. Fungsi First-Class & HOF

- **Abstraksi** — membuat fungsi generik yang bisa dikustomisasi
- **Komposisi** — menggabungkan fungsi kecil jadi fungsi kompleks
- **Code reuse** — pola seperti map/filter/reduce
- **Callback & async** — fungsi dikirim untuk dijalankan nanti
- **Dekorator** — membungkus fungsi dengan perilaku tambahan

---

## 3. Code

### 3a. Fungsi sebagai nilai
```javascript
const say = function(name) {
    return `Halo ${name}`;
};
const fn = say;   // simpan fungsi ke variabel lain
console.log(fn("Budi")); // "Halo Budi"

// Fungsi dalam array
const operations = [
    (x) => x + 1,
    (x) => x * 2,
    (x) => x - 3
];
console.log(operations[0](5)); // 6
```

### 3b. HOF — menerima fungsi
```javascript
function repeat(fn, n) {
    for (let i = 0; i < n; i++) {
        fn(i);
    }
}
repeat((i) => console.log(`Iterasi ${i}`), 3);
```

### 3c. HOF — mengembalikan fungsi
```javascript
function multiplyBy(factor) {
    return function(x) {
        return x * factor;
    };
}
const double = multiplyBy(2);
const triple = multiplyBy(3);
console.log(double(5));  // 10
console.log(triple(5));  // 15
```

### 3d. compose dan pipe
```javascript
// compose: eksekusi dari kanan ke kiri
function compose(...fns) {
    return function(x) {
        return fns.reduceRight((acc, fn) => fn(acc), x);
    };
}

// pipe: eksekusi dari kiri ke kanan
function pipe(...fns) {
    return function(x) {
        return fns.reduce((acc, fn) => fn(acc), x);
    };
}

const add1 = (x) => x + 1;
const times2 = (x) => x * 2;
const add1ThenTimes2 = pipe(add1, times2);
const times2ThenAdd1 = compose(add1, times2);

console.log(add1ThenTimes2(5)); // (5+1)*2 = 12
console.log(times2ThenAdd1(5)); // (5*2)+1 = 11
```

### 3e. Array HOF — map, filter, reduce
```javascript
const nums = [1, 2, 3, 4, 5];
const doubled = nums.map(x => x * 2);
const evens = nums.filter(x => x % 2 === 0);
const sum = nums.reduce((a, b) => a + b, 0);
console.log(doubled, evens, sum);
// [2,4,6,8,10], [2,4], 15
```

---

## 4. Analogi Rumah

| Konsep | Analogi Rumah |
|--------|---------------|
| First-class function | **Mesin yang bisa dipindah-pindah** — sama seperti palu, gergaji, bor |
| HOF (terima fungsi) | **Kontraktor** yang terima mesin dari berbagai tukang dan koordinasi |
| HOF (kembalikan fungsi) | **Pabrik** yang bikin mesin baru berdasarkan spesifikasi |
| `map` | Mesin **cat tembok** — aplikasikan fungsi ke setiap elemen |
| `filter` | **Saringan pasir** — hanya loloskan yang memenuhi syarat |
| `reduce` | **Mesin press** — kumpulkan semua bahan jadi satu balok |
| `compose` / `pipe` | **Lini perakitan** — mesin A → mesin B → mesin C |
| Callback | **Telepon tukang** — "nanti kalau sudah selesai, kasih tahu saya" |

**Narasi:** Di proyek rumah, alat-alat (fungsi) bisa dipindahkan ke mana saja — first-class function. Kontraktor (HOF) menerima alat dari berbagai tukang, menggabungkannya. Ada juga pabrik yang bikin alat baru berdasarkan spesifikasi. Map seperti mesin cat yang mewarnai setiap bata. Filter seperti saringan pasir. Reduce seperti mesin press yang mengompres semua jadi satu. Pipe/compose seperti lini perakitan — bahan masuk di satu ujung, rumah jadi di ujung lain.

---

## 5. Use Case

| Situasi | HOF yang Cocok |
|---------|---------------|
| Transformasi array | `map()` |
| Seleksi data | `filter()` |
| Agregasi data | `reduce()` |
| Sorting custom | `.sort((a,b) => ...)` |
| Validasi | `every()`, `some()` |
| Pencarian | `find()`, `findIndex()` |
| Middleware (Express/Koa) | Fungsi dalam pipeline |
| Event handler | Callback function |
| Debounce/throttle | HOF yang wrapping fungsi |
| Decorator pattern | Logging, caching wrapper |

---

## 6. Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|-----------|--------|--------|
| **Lupa return** | `arr.map(x => { x*2 })` tapi tanpa return di `{}` | Array `[undefined, ...]` |
| **Mutasi argumen** | HOF mengubah array original | Side effect tak terduga |
| **Callback hell** | Callback bersarang 5 level | Kode susah dibaca (→ async/await) |
| **Over-abstraksi** | Bikin HOF untuk operasi sederhana | Kode makin kompleks |
| **Referensi fungsi vs eksekusi** | `setTimeout(fn(), 100)` bukan `setTimeout(fn, 100)` | Langsung dieksekusi |
| **Lupa kurung di HOF** | `pipe(fn1, fn2())` bukan `pipe(fn1, fn2)` | Argumen kedua adalah hasil, bukan fungsi |

---

## 7. Benang Merah

Materi 36 (this: fungsi sebagai nilai → this bisa diubah) → **Materi 37 (first-class function & HOF)** → Materi 38 (rekursif: fungsi panggil dirinya sendiri — juga bentuk HOF)

HOF adalah fondasi untuk:
- Functional programming (Materi 39)
- Async programming (Promise, callback)
- Array methods (map/filter/reduce sudah dipelajari di Materi 18-20)

---

## 8. Soal

### Soal 1 (Mudah)
Apa outputnya?
```javascript
function greet(greeting) {
    return function(name) {
        return `${greeting}, ${name}!`;
    };
}
const sayHalo = greet("Halo");
console.log(sayHalo("Budi"));
```

<details>
<summary>Jawaban</summary>
"Halo, Budi!" — closure + HOF: `sayHalo` adalah fungsi yang mengingat `greeting = "Halo"`.
</details>

### Soal 2 (Sedang)
Buat fungsi `mapWith(fn)` yang mengembalikan fungsi baru yang menerima array dan memetakan `fn` ke array itu.

<details>
<summary>Jawaban</summary>
```javascript
function mapWith(fn) {
    return function(arr) {
        return arr.map(fn);
    };
}
const doubleAll = mapWith(x => x * 2);
console.log(doubleAll([1, 2, 3])); // [2, 4, 6]
```
</details>

### Soal 3 (Sulit)
Buat fungsi `curry(fn)` yang mengubah fungsi dengan n parameter menjadi fungsi yang bisa dipanggil dengan 1 parameter setiap kali.

Contoh: `curry((a,b,c) => a+b+c)(1)(2)(3)` → 6

<details>
<summary>Jawaban</summary>
```javascript
function curry(fn) {
    return function curried(...args) {
        if (args.length >= fn.length) {
            return fn(...args);
        }
        return (...next) => curried(...args, ...next);
    };
}
const sum = (a, b, c) => a + b + c;
const curriedSum = curry(sum);
console.log(curriedSum(1)(2)(3)); // 6
console.log(curriedSum(1, 2)(3)); // 6
console.log(curriedSum(1, 2, 3)); // 6
```
</details>
