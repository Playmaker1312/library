# 38. Fungsi Rekursif

---

## 1. Penjelasan

**Rekursi** adalah teknik di mana fungsi **memanggil dirinya sendiri** untuk menyelesaikan masalah dengan cara memecahnya menjadi sub-masalah yang lebih kecil.

Setiap fungsi rekursif harus memiliki dua bagian:

| Bagian | Fungsi |
|--------|--------|
| **Base Case** | Kondisi berhenti — jika terpenuhi, rekursi berhenti |
| **Recursive Case** | Panggilan ke diri sendiri dengan argumen yang lebih kecil (mendekati base case) |

**Stack dan Rekursi:**
- Setiap panggilan rekursif ditambahkan ke **call stack** (Materi 31)
- Jika base case tidak tercapai → **stack overflow**
- **Tail Call Optimization (TCO)**: beberapa engine mengoptimasi rekursi yang return-nya langsung berupa panggilan fungsi (tanpa operasi tambahan)

---

## 2. Fungsi Rekursif

- **Menyederhanakan** masalah yang punya struktur berulang (tree, nested data)
- **Alternatif** iterasi untuk masalah tertentu (lebih intuitif)
- **Memproses** struktur data rekursif (pohon, graph, nested array/objek)
- **Algoritma** divide-and-conquer (merge sort, quick sort, binary search)

---

## 3. Code

### 3a. Faktorial
```javascript
// Rekursif
function faktorial(n) {
    if (n <= 1) return 1;          // base case
    return n * faktorial(n - 1);   // recursive case
}

// Iteratif (perbandingan)
function faktorialIteratif(n) {
    let hasil = 1;
    for (let i = 2; i <= n; i++) hasil *= i;
    return hasil;
}

console.log(faktorial(5)); // 120
console.log(faktorialIteratif(5)); // 120
```

**Visualisasi Stack untuk `faktorial(4)`:**
```
faktorial(4) → 4 * faktorial(3)
                    → 3 * faktorial(2)
                        → 2 * faktorial(1)
                            → 1 (base case)
                        → 2 * 1 = 2
                    → 3 * 2 = 6
                → 4 * 6 = 24
```

### 3b. Fibonacci
```javascript
// Rekursif naif (O(2^n))
function fib(n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}

// Rekursif dengan memoization (O(n))
function fibMemo(n, memo = {}) {
    if (n <= 1) return n;
    if (n in memo) return memo[n];
    memo[n] = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
    return memo[n];
}

console.log(fibMemo(10));  // 55
console.log(fibMemo(50));  // 12586269025 (tanpa memo: hang)
```

### 3c. Flatten Nested Array
```javascript
function flatten(arr) {
    let result = [];
    for (let item of arr) {
        if (Array.isArray(item)) {
            result = result.concat(flatten(item));
        } else {
            result.push(item);
        }
    }
    return result;
}

console.log(flatten([1, [2, [3, 4], 5], 6]));
// [1, 2, 3, 4, 5, 6]
```

### 3d. Deep Clone (rekursif untuk objek)
```javascript
function deepClone(obj) {
    if (obj === null || typeof obj !== "object") return obj;
    const clone = Array.isArray(obj) ? [] : {};
    for (let key in obj) {
        clone[key] = deepClone(obj[key]);
    }
    return clone;
}
```

---

## 4. Analogi Rumah

| Konsep | Analogi Rumah |
|--------|---------------|
| Rekursi | **Cermin menghadap cermin lain** — gambar di dalam gambar |
| Base case | **Cermin terkecil** yang tidak punya pantulan lagi |
| Recursive case | Cermin besar yang memantulkan bayangan cermin lebih kecil |
| Call stack | **Tumpukan cetak biru** — gambar rumah besar, di dalamnya ada gambar ruang tamu, di dalamnya ada gambar meja, dst |
| Stack overflow | Tumpukan cetak biru **terlalu tinggi** sampai ambruk |
| Tail call optimization | Arsitek yang **langsung gambar detail tanpa bolak-balik** ke gambar utama |
| Rekursi vs iterasi | **Rekursi** = minta orang lain gambar detail (mereka minta orang lain lagi). **Iterasi** = gambar sendiri dari atas ke bawah |
| Tree traversal (DOM) | **Navigasi ruangan** — buka pintu, lihat ruangan, buka pintu lemari, lihat isinya |

**Narasi:** Rekursi seperti cermin yang menghadap cermin lain — di dalam bayangan ada bayangan lagi. Atau seperti cetak biru rumah: di halaman 1 ada gambar rumah. Di halaman 5 ada detail ruang tamu. Di halaman 12 ada detail lemari ruang tamu. Untuk memahami rumah sepenuhnya, Anda harus buka halaman demi halaman, masuk lebih dalam (recursive case), sampai Anda sampai ke detail terkecil yang tidak perlu dipecah lagi (base case). Lalu Anda kembali naik, memahami setiap level.

---

## 5. Use Case

| Masalah | Solusi Rekursif | Alternatif Iteratif |
|---------|----------------|-------------------|
| Faktorial | `n * faktorial(n-1)` | Loop |
| Fibonacci | `fib(n-1) + fib(n-2)` | Loop + array |
| Tree traversal (DOM) | Rekursif natural | Stack manual |
| Directory traversal | Rekursif natural | Queue/stack |
| JSON.stringify | Rekursif internal | Tidak praktis |
| Fractal / Sierpinski | Rekursif natural | Sangat kompleks |
| Merge sort, Quick sort | Divide & conquer rekursif | Tidak praktis |
| Parsing expression (AST) | Rekursif natural | Stack manual |

---

## 6. Kesalahan Umum

| Kesalahan | Contoh | Dampak |
|-----------|--------|--------|
| **Lupa base case** | `function f(n) { return f(n-1); }` | Stack overflow |
| **Base case salah** | `n > 0` malah `n >= 0` dengan `n = 0` | Infinite loop |
| **Tidak mengurangi** | `f(n)` bukan `f(n-1)` | Tidak pernah sampai base case |
| **Rekursi terlalu dalam** | `f(100000)` tanpa TCO | Stack overflow |
| **Mutasi data** | Array/objek dimutasi antar panggilan | Bug state |
| **Rekursi naif** | `fib(50)` tanpa memo | Sangat lambat |

```javascript
// Contoh stack overflow
function infinite() {
    return infinite();
}
// console.log(infinite()); // ❌ RangeError: Maximum call stack size exceeded
```

---

## 7. Benang Merah

Materi 37 (HOF: fungsi sebagai nilai) + Materi 31 (Stack: LIFO) → **Materi 38 (rekursif: stack + fungsi panggil diri)** → Materi 39 (pure function: fungsi ideal tanpa side effect)

Rekursi mengajarkan kita tentang **base case** dan **recursive case** — pola yang sama akan muncul lagi di OOP (inheritance chain), Promise chaining, dan struktur data tree.

---

## 8. Soal

### Soal 1 (Mudah)
Apa outputnya?
```javascript
function countdown(n) {
    if (n <= 0) {
        console.log("GO!");
        return;
    }
    console.log(n);
    countdown(n - 1);
}
countdown(3);
```

<details>
<summary>Jawaban</summary>
3, 2, 1, "GO!". Fungsi mencetak n, lalu panggil dirinya dengan n-1, sampai n = 0.
</details>

### Soal 2 (Sedang)
Buat fungsi `sumArray(arr)` yang menjumlahkan semua elemen array secara rekursif.
```javascript
sumArray([1, 2, 3, 4, 5]); // 15
```

<details>
<summary>Jawaban</summary>
```javascript
function sumArray(arr) {
    if (arr.length === 0) return 0;           // base case
    return arr[0] + sumArray(arr.slice(1));   // recursive case
}
// Atau dengan destructuring:
function sumArr([first, ...rest]) {
    if (first === undefined) return 0;
    return first + sumArr(rest);
}
```
</details>

### Soal 3 (Sulit)
Buat fungsi `getNestedValue(obj, path)` yang mengambil nilai dari objek bertingkat menggunakan string path, secara rekursif.

```javascript
const data = { a: { b: { c: 42 } } };
getNestedValue(data, "a.b.c"); // 42
getNestedValue(data, "a.x.y"); // undefined
```

<details>
<summary>Jawaban</summary>
```javascript
function getNestedValue(obj, path) {
    const [key, ...rest] = path.split(".");
    if (obj == null || !(key in obj)) return undefined;
    if (rest.length === 0) return obj[key];
    return getNestedValue(obj[key], rest.join("."));
}

// Versi lain dengan reduce
function getNestedValue2(obj, path) {
    return path.split(".").reduce((acc, key) =>
        acc != null ? acc[key] : undefined, obj);
}
```
</details>
