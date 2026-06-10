# 35. Closure — Konsep Paling Penting di JavaScript

---

## 1. Penjelasan

**Closure** adalah fungsi yang "mengingat" scope (lingkungan) di mana ia diciptakan — bahkan setelah scope itu selesai dieksekusi.

Secara teknis: ketika sebuah fungsi **inner** mengakses variabel dari fungsi **outer**, JavaScript menyimpan referensi ke variabel-variabel tersebut. Inilah closure.

| Istilah | Definisi |
|---------|----------|
| **Lexical Scoping** | Scope ditentukan oleh posisi kode dalam kode sumber (bukan runtime) |
| **Closure** | Inner function + lexical environment tempat ia diciptakan |
| **Free Variable** | Variabel yang tidak dideklarasikan di fungsi sendiri tapi diakses |
| **Persistent Scope** | Scope outer yang tetap hidup karena dirujuk oleh inner function |

---

## 2. Fungsi Closure

- **Data privacy** — membuat variabel yang tidak bisa diakses dari luar
- **Factory function** — membuat fungsi dengan "konfigurasi" tersimpan
- **Memoization / caching** — menyimpan hasil komputasi
- **Module pattern** — sebelum ES Modules (Materi 40)
- **Event handler / callback** — "mengingat" state saat event didaftarkan

---

## 3. Code — Counter dengan Closure

```javascript
function createCounter() {
    let count = 0;           // — variabel "private"

    return {
        increment: function() {
            count++;
            return count;
        },
        decrement: function() {
            count--;
            return count;
        },
        getCount: function() {
            return count;
        }
    };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2

// count TIDAK bisa diakses langsung:
console.log(counter.count);       // undefined
```

Contoh lain — closure dengan parameter:
```javascript
function makeAdder(x) {
    return function(y) {
        return x + y;  // x "teringat" oleh closure
    };
}

const add5 = makeAdder(5);
console.log(add5(3));  // 8
console.log(add5(10)); // 15
```

---

## 4. Analogi Rumah

| Konsep | Analogi Rumah |
|--------|---------------|
| Closure | **Tukang yang ingat alat di ruangan tertentu** meski sudah pindah |
| Lexical scope | **Cetak biru rumah** — sudah ditentukan dari awal bagaimana ruangan terhubung |
| Free variable | Alat yang bukan milik tukang itu tapi **ia pinjam dari ruangan sebelumnya** |
| Outer function | **Ruang pertama** tempat tukang mulai kerja |
| Inner function | **Pekerjaan baru** yang dibawa tukang ke ruang lain |
| Data privacy | **Isi lemari pribadi** — tau isinya cuma tukang itu sendiri |
| Memory leak | Tukang **bawa semua alat dari semua ruangan** dan tidak pernah mengembalikan |

**Narasi:** Seorang tukang mulai kerja di ruang tamu (outer function). Di sana ia mengambil palu (variabel). Lalu ia pindah ke kamar tidur (inner function) dan mulai bekerja. Meski sudah di kamar tidur, ia **masih ingat** palu dari ruang tamu. Bahkan setelah ruang tamu dibongkar (outer selesai), si tukang masih bisa pakai palu itu selama ia masih ingat. Itulah closure: fungsi yang membawa serta "kenangan" akan scope tempat ia diciptakan.

---

## 5. Use Case

```javascript
// 1. Data Privacy — membuat ID unik
function createUserId(prefix) {
    let id = 0;
    return function() {
        id++;
        return `${prefix}-${id}`;
    };
}
const genId = createUserId("USER");
console.log(genId()); // USER-1
console.log(genId()); // USER-2

// 2. Debounce — function yang delay eksekusi
function debounce(fn, delay) {
    let timer;
    return function(...args) {
        clearTimeout(timer);
        timer = setTimeout(() => fn(...args), delay);
    };
}

// 3. Memoization
function memoize(fn) {
    const cache = {};
    return function(n) {
        if (n in cache) return cache[n];
        cache[n] = fn(n);
        return cache[n];
    };
}
```

---

## 6. Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|-----------|--------|--------|
| Closure dalam loop (var) | `for (var i...){ arr.push(()=>i) }` | Semua fungsi lihat `i` yang sama |
| **Memory leak** | Closure besar yang tidak diperlukan | Variabel tidak bisa di-GC |
| Lupa closure akses reference (bukan value) | Variabel berubah setelah closure dibuat | Nilai "kadaluarsa" |
| Oversharing | Closure expose variabel internal | Data bocor |
| Closure di class lama | Callback method tanpa bind | this hilang (lanjut Materi 36) |

```javascript
// Masalah closure dalam loop (var)
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
    // Semua cetak 3 — karena var function-scoped, i sudah berubah
}

// Solusi: IIFE atau let
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
    // 0, 1, 2 — let membuat binding baru tiap iterasi
}
```

---

## 7. Benang Merah

Materi 33 (scope) + Materi 34 (hoisting) → **Materi 35 (closure: fungsi ingat scope pembuatnya)** → Materi 36 (this: konteks eksekusi yang berbeda)

Closure dan `this` sering berinteraksi: closure biasanya mempertahankan `this` dari scope luar (jika pakai arrow function), atau kehilangan `this` (jika pakai function biasa).

---

## 8. Soal

### Soal 1 (Mudah)
Apa outputnya?
```javascript
function outer() {
    let x = 10;
    function inner() {
        return x;
    }
    return inner;
}
const fn = outer();
console.log(fn());
```

<details>
<summary>Jawaban</summary>
10. Closure membuat `fn` masih mengingat `x` dari outer meskipun outer sudah selesai.
</details>

### Soal 2 (Sedang)
Apa output dan kenapa?
```javascript
function createButtons() {
    const buttons = [];
    for (var i = 0; i < 3; i++) {
        buttons.push(function() {
            console.log(i);
        });
    }
    return buttons;
}
const btns = createButtons();
btns[0]();
btns[1]();
btns[2]();
```

<details>
<summary>Jawaban</summary>
3, 3, 3. Karena `var i` function-scoped, closure semua fungsi merujuk ke variabel `i` yang SAMA. Setelah loop selesai, `i = 3`. Solusi: ganti `var` dengan `let`.
</details>

### Soal 3 (Sulit)
Buat fungsi `once(fn)` yang memastikan `fn` hanya dipanggil SATU KALI. Panggilan berikutnya mengembalikan hasil pertama.

<details>
<summary>Jawaban</summary>
```javascript
function once(fn) {
    let called = false;
    let result;
    return function(...args) {
        if (!called) {
            called = true;
            result = fn(...args);
            return result;
        }
        return result;
    };
}

const init = once(() => { console.log("RUN"); return 42; });
console.log(init()); // "RUN", 42
console.log(init()); // 42 (tanpa "RUN")
console.log(init()); // 42
```
</details>
