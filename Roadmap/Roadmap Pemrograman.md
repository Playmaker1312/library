# 🗺️ Roadmap JavaScript: Komprehensif & Terstruktur

## Filosofi Roadmap Ini

> **"Belajar seperti membangun rumah"** — fondasi dulu, baru dinding, baru atap. Setiap materi adalah batu bata yang menopang materi berikutnya. Tidak ada lompatan, semua terhubung.

### Prinsip Desain

- **Benang Merah**: Setiap topik eksplisit menyebutkan koneksi ke topik sebelum dan sesudahnya
- **JavaScript-First**: Semua terminologi dan contoh konsisten menggunakan JavaScript
- **Micro-Project**: Setiap sub-bab memiliki latihan langsung
- **Spiral Learning**: Konsep diperkenalkan ringan, lalu didalami di level berikutnya

---

## 📋 Peta Benang Merah Antar Level

text

```
CARA BERPIKIR → SINTAKS DASAR → STRUKTUR DATA & FUNGSI
      ↓                ↓                    ↓
   Level 1          Level 1-2            Level 2
      
      → OOP → ALGORITMA → WEB → ARSITEKTUR → SKALA
          ↓        ↓        ↓        ↓          ↓
       Level 2   Level 3  Level 3  Level 4    Level 5-6
```

---

## 🟢 LEVEL 1: FONDASI BERPIKIR & SINTAKS (Bulan 1-2)

> **Tema Level**: _"Dari nol ke program pertama yang berjalan"_  
> **Benang Merah**: Belajar CARA BERPIKIR dulu → baru CARA MENULIS kode  
> **Output Level**: Bisa membuat program CLI interaktif sederhana

---

### A. Sebelum Menulis Kode — Cara Berpikir Seperti Programmer

> 💡 **Mengapa dimulai di sini?** Banyak pemula gagal bukan karena tidak paham sintaks, tapi karena tidak tahu _cara berpikir_ untuk memecahkan masalah. Bagian ini adalah investasi terpenting.

text

```
Benang Merah Bagian A:
Dunia Pemrograman → Cara Berpikir Komputasional → 
Visualisasi Logika → Siap Menulis Kode Pertama
```

1. `[[1. Apa itu pemrograman & mengapa penting]]`
    
    - Apa yang sebenarnya dilakukan programmer
    - Pemrograman = instruksi yang sangat spesifik untuk mesin
    - _Micro-exercise_: Tulis instruksi langkah-langkah membuat mie instan sedetail mungkin
2. `[[2. Bagaimana komputer memproses kode — dari teks ke eksekusi]]`
    
    - Source code → Parser → AST → Eksekusi
    - Mengapa komputer tidak bisa "menebak" maksud kita
    - Perbedaan bahasa compiled vs interpreted (JavaScript = interpreted/JIT)
    - _Micro-exercise_: Trace alur kode sederhana secara manual
3. `[[3. Mengenal JavaScript & posisinya di dunia pemrograman]]`
    
    - Sejarah singkat JavaScript (mengapa ada, bagaimana berkembang)
    - JavaScript untuk browser, server (Node.js), mobile, desktop
    - Ekosistem JavaScript: runtime, engine V8, npm
    - _Micro-exercise_: Research 3 aplikasi terkenal yang dibuat dengan JS
4. `[[4. Menyiapkan environment — VS Code, Node.js, Terminal]]`
    
    - Instalasi Node.js dan npm
    - Setup VS Code + extension esensial (Prettier, ESLint, GitLens)
    - Mengenal terminal: navigasi folder, membuat file
    - Menjalankan file JS pertama dengan `node`
    - _Micro-exercise_: Buat file `hello.js`, jalankan dengan Node.js
5. `[[5. Computational Thinking — Dekomposisi & Pattern Recognition]]`
    
    - Dekomposisi: memecah masalah besar menjadi kecil
    - Pattern recognition: melihat pola yang berulang
    - _Micro-exercise_: Dekomposisi masalah "buat program tebak angka" tanpa kode
6. `[[6. Abstraksi & Pseudocode]]`
    
    - Abstraksi: menyembunyikan detail, fokus ke inti masalah
    - Pseudocode: bahasa manusia yang terstruktur
    - _Micro-exercise_: Tulis pseudocode untuk algoritma mencari nilai terbesar dari 3 angka
7. `[[7. Flowchart — visualisasi alur logika]]`
    
    - Simbol-simbol flowchart (start/end, process, decision, I/O)
    - Kapan flowchart membantu vs kapan tidak perlu
    - _Micro-exercise_: Buat flowchart untuk sistem login sederhana
8. `[[8. Logika Boolean — AND, OR, NOT & Truth Table]]`
    
    - Logika boolean sebagai fondasi semua kondisi dalam program
    - Truth table untuk kombinasi AND, OR, NOT
    - Short-circuit evaluation (preview — akan didalami saat if/else)
    - _Micro-exercise_: Isi truth table 10 kombinasi ekspresi boolean

---

### B. Sintaks JavaScript — Dari Variabel ke Program Pertama

> 💡 **Mengapa urutan ini?** Kita mulai dari unit terkecil (variabel/data) → cara memanipulasinya (operator) → cara mengontrol alur (percabangan, loop) → cara menangani kesalahan. Ini adalah alur berpikir yang alami.

text

```
Benang Merah Bagian B:
Data (Variabel) → Operasi pada Data → 
Kontrol Alur → Input/Output → Error Handling
```

9. `[[9. Variabel — var, let, const & perbedaan mendasarnya]]`
    
    - Apa itu variabel: wadah penyimpan data
    - `var` vs `let` vs `const` — bukan hanya sintaks, tapi perilaku berbeda
    - Naming convention (camelCase) dan reserved words
    - Hoisting — mengapa `var` berperilaku aneh (preview, didalami di Level 2)
    - _Micro-exercise_: Eksplorasi perbedaan `var`, `let`, `const` dengan sengaja membuat error
10. `[[10. Tipe Data Primitif JavaScript — 7 tipe yang wajib dikuasai]]`
    
    - `string`, `number`, `boolean`, `null`, `undefined`, `symbol`, `bigint`
    - JavaScript adalah dynamically typed — apa artinya
    - `typeof` operator untuk mengecek tipe data
    - Truthy & Falsy values — 6 nilai falsy di JavaScript
    - _Micro-exercise_: Tebak output `typeof` dari 15 ekspresi berbeda
11. `[[11. Type Coercion & Keunikan JavaScript — == vs ===]]`
    
    - Implicit type coercion: kapan JavaScript mengubah tipe otomatis
    - `==` (loose equality) vs `===` (strict equality) — mengapa selalu gunakan `===`
    - Quirks terkenal JS: `"5" + 3`, `"5" - 3`, `null == undefined`
    - _Micro-exercise_: Prediksi dan verifikasi 20 ekspresi type coercion
12. `[[12. Operator — Aritmatika, Perbandingan, Logika, Assignment]]`
    
    - Operator aritmatika: `+`, `-`, `*`, `/`, `%`, `**`
    - Operator perbandingan: `>`, `<`, `>=`, `<=`, `===`, `!==`
    - Operator logika: `&&`, `||`, `!`, `??` (nullish coalescing)
    - Operator assignment: `=`, `+=`, `-=`, `*=`, `??=`
    - Operator ternary: `kondisi ? nilai1 : nilai2`
    - _Micro-exercise_: Buat kalkulator ekspresi kompleks dengan semua operator
13. `[[13. String — Tipe Data Terpenting untuk I/O]]`
    
    - String sebagai urutan karakter
    - Template literals: `` `Halo ${nama}` `` — cara modern menulis string
    - Escape characters: `\n`, `\t`, `\\`
    - String methods paling penting: `.length`, `.toUpperCase()`, `.toLowerCase()`, `.includes()`, `.slice()`, `.split()`, `.trim()`, `.replace()`
    - _Micro-exercise_: Buat fungsi format nama (capitalize setiap kata)
14. `[[14. Input & Output di Node.js — readline & console]]`
    
    - `console.log()`, `console.error()`, `console.table()`
    - Menerima input dari user dengan `readline` module
    - _Micro-exercise_: Program yang menerima nama user dan menyapa dengan format khusus
15. `[[15. Percabangan — if, else if, else & switch]]`
    
    - `if/else` sebagai implementasi logika boolean dari Poin 8
    - `else if` untuk kondisi berantai
    - `switch` — kapan lebih baik dari `if/else`
    - Short-circuit evaluation: `&&` dan `||` untuk kondisi ringkas
    - _Micro-exercise_: Program grade calculator (A/B/C/D/E berdasarkan nilai)
16. `[[16. Perulangan — for, while, do-while]]`
    
    - Mengapa kita butuh loop: menghindari kode berulang
    - `for` loop: saat jumlah iterasi diketahui
    - `while` loop: saat kondisi belum pasti kapan berhenti
    - `do-while` loop: minimal sekali dijalankan
    - `break` dan `continue` — mengontrol alur loop
    - _Micro-exercise_: Program FizzBuzz (1-100)
17. `[[17. Nested Loop & Nested Condition — Pola & Batasannya]]`
    
    - Nested loop: loop di dalam loop
    - Kapan nested loop wajar vs kapan harus dihindari
    - Pattern printing dengan nested loop (segitiga bintang)
    - _Micro-exercise_: Cetak tabel perkalian 1-10 dengan nested loop
18. `[[18. Error & Exception Handling — try, catch, finally, throw]]`
    
    - Jenis-jenis error di JavaScript: `TypeError`, `ReferenceError`, `SyntaxError`, `RangeError`
    - `try/catch/finally` — menangkap dan menangani error
    - `throw` — melempar error custom
    - Error handling yang baik vs buruk
    - _Micro-exercise_: Buat validator input yang melempar custom error
19. `[[19. Komentar, Code Readability & Naming Convention]]`
    
    - Komentar: kapan perlu dan kapan tidak perlu
    - Readability: kode dibaca lebih sering daripada ditulis
    - Naming convention: variabel, fungsi, konstanta
    - _Micro-exercise_: Refactor kode berantakan menjadi readable

---

### 🏗️ Proyek Level 1

text

```
PROYEK: "CLI Personal Assistant"
─────────────────────────────────
Fitur:
├── Menyapa user dengan nama yang diinput
├── Kalkulator (pilih operasi: +, -, *, /)
├── Konversi suhu (Celsius ↔ Fahrenheit ↔ Kelvin)
├── Cek apakah angka prima
└── Program berjalan terus sampai user pilih "keluar"

Konsep yang dilatih:
variables, string, I/O, if/else, loop, error handling
```

---

## 🟡 LEVEL 2: STRUKTUR DATA, FUNGSI & JAVASCRIPT MENDALAM (Bulan 2-4)

> **Tema Level**: _"Dari program yang berjalan ke kode yang terstruktur dan maintainable"_  
> **Benang Merah**: Data kompleks → Fungsi untuk mengolah data → Cara kerja JS di balik layar → OOP untuk organisasi kode  
> **Output Level**: Bisa membuat aplikasi CLI yang terstruktur dengan baik

---

### C. Git & Version Control — Mulai Sekarang, Bukan Nanti

> 💡 **Mengapa Git di Level 2?** Version control bukan tool advanced — ini adalah **kebiasaan dasar** yang harus dibangun sejak awal. Setiap proyek dari sini ke depan harus menggunakan Git.

text

```
Benang Merah Bagian C:
Masalah tanpa version control → Git dasar → 
GitHub → Workflow untuk project solo
```

20. `[[20. Mengapa Version Control — Masalah yang Diselesaikan Git]]`
    
    - Tanpa Git: "final_v2_BENERAN_FINAL.js"
    - Konsep snapshot, history, dan rollback
    - _Micro-exercise_: Buat skenario masalah tanpa version control
21. `[[21. Git Dasar — init, add, commit, status, log, diff]]`
    
    - Inisiasi repository: `git init`
    - Staging area: `git add`
    - Menyimpan snapshot: `git commit`
    - Melihat status dan history: `git status`, `git log`
    - _Micro-exercise_: Commit project Level 1 ke Git (retroaktif)
22. `[[22. GitHub — Remote Repository, Push, Pull, Clone]]`
    
    - Hubungan local repo dan remote repo
    - `git push`, `git pull`, `git clone`
    - README.md — dokumentasi project di GitHub
    - .gitignore — apa yang tidak perlu di-track
    - _Micro-exercise_: Push project Level 1 ke GitHub
23. `[[23. Branching — Feature Branch Workflow]]`
    
    - Mengapa branch: isolasi perubahan
    - `git branch`, `git checkout`, `git merge`
    - Merge conflict: cara mengidentifikasi dan menyelesaikan
    - _Micro-exercise_: Buat feature branch untuk project Level 1, merge ke main

---

### D. Struktur Data — Menyimpan & Mengorganisasi Data

> 💡 **Benang Merah ke Poin Sebelumnya**: Di Level 1 kita menyimpan data dalam variabel tunggal. Sekarang kita belajar menyimpan _koleksi_ data dan kapan menggunakan struktur yang tepat.

text

```
Benang Merah Bagian D:
Variabel tunggal (Level 1) → Array (koleksi berurut) → 
Object (koleksi key-value) → Set (koleksi unik) → 
Map (key-value fleksibel) → Kapan pakai yang mana
```

24. `[[24. Array — Koleksi Data Berurut]]`
    
    - Array sebagai ordered list
    - CRUD array: akses index, `.push()`, `.pop()`, `.shift()`, `.unshift()`, `.splice()`
    - Iterasi array: `for`, `for...of`
    - Array multidimensi
    - _Micro-exercise_: Sistem antrian sederhana (tambah/hapus orang dari antrian)
25. `[[25. Array Methods Fungsional — map, filter, reduce, find, forEach]]`
    
    - Paradigma fungsional vs imperatif untuk array
    - `.map()`: transformasi setiap elemen
    - `.filter()`: menyaring elemen berdasarkan kondisi
    - `.reduce()`: mengakumulasi nilai
    - `.find()`, `.findIndex()`, `.some()`, `.every()`
    - Chaining array methods
    - _Micro-exercise_: Olah data array produk: filter yang tersedia, hitung total harga, format untuk display
26. `[[26. String Lanjutan — Methods, Regex Intro & Parsing]]`
    
    - Review string dari Level 1 + methods lanjutan
    - `.indexOf()`, `.lastIndexOf()`, `.startsWith()`, `.endsWith()`
    - `.padStart()`, `.padEnd()`, `.repeat()`
    - Intro Regex: pattern dasar untuk validasi (email, nomor telepon)
    - _Micro-exercise_: Buat validator form (nama, email, nomor telepon)
27. `[[27. Object — Struktur Data Key-Value Fundamental JavaScript]]`
    
    - Object sebagai representasi "benda" dengan properti
    - Membuat, mengakses, mengubah properti: dot notation vs bracket notation
    - Object methods, `this` di dalam object literal (preview)
    - Iterasi object: `for...in`, `Object.keys()`, `Object.values()`, `Object.entries()`
    - Nested object dan optional chaining `?.`
    - Object destructuring
    - Spread operator untuk object: `{...obj1, ...obj2}`
    - _Micro-exercise_: Buat sistem profil pengguna dengan nested object
28. `[[28. Array of Objects — Pola Data Paling Umum di Dunia Nyata]]`
    
    - Mengapa array of objects: representasi "database" sederhana
    - CRUD pada array of objects
    - Kombinasi `.map()`, `.filter()`, `.reduce()` pada array of objects
    - Sorting array of objects dengan `.sort()`
    - _Micro-exercise_: Sistem manajemen daftar tugas (todo list) di memory
29. `[[29. Destructuring, Spread & Rest — Sintaks Modern JavaScript]]`
    
    - Array destructuring: `const [a, b] = arr`
    - Object destructuring: `const { nama, umur } = obj`
    - Default values dalam destructuring
    - Spread operator: `[...arr1, ...arr2]`
    - Rest parameter: `function(...args)`
    - _Micro-exercise_: Refactor kode sebelumnya menggunakan destructuring dan spread
30. `[[30. Map & Set — Kapan Lebih Baik dari Object & Array]]`
    
    - `Set`: koleksi nilai unik — kapan digunakan
    - `Map`: key-value dengan key bertipe apapun — perbedaan dengan Object
    - Iterasi Map dan Set
    - Use case nyata: deduplikasi data, caching sederhana
    - _Micro-exercise_: Deteksi kata duplikat dalam teks menggunakan Set dan Map
31. `[[31. Stack & Queue — Struktur Data Logis dari Array]]`
    
    - Stack (LIFO): implementasi dengan array, use case (undo/redo, call stack)
    - Queue (FIFO): implementasi dengan array, use case (antrian proses)
    - Koneksi ke: Call Stack JavaScript (preview Event Loop)
    - _Micro-exercise_: Implementasi fitur undo/redo sederhana menggunakan Stack

---

### E. Fungsi — Jantung dari JavaScript

> 💡 **Benang Merah ke Sebelumnya**: Kita sudah bisa menyimpan dan mengolah data. Sekarang kita belajar membungkus logika dalam unit yang bisa digunakan ulang. Ini adalah langkah menuju kode yang maintainable.

text

```
Benang Merah Bagian E:
Masalah kode berulang → Fungsi dasar → 
Scope & Closure → Fungsi sebagai nilai (First-class) → 
Higher-order functions → Pattern fungsi modern
```

32. `[[32. Fungsi Dasar — Deklarasi, Ekspresi, Arrow Function]]`
    
    - Function declaration vs function expression vs arrow function
    - Parameter, default parameter, dan return value
    - Perbedaan `function declaration` dan `function expression` (hoisting)
    - Arrow function: sintaks ringkas dan kapan digunakan
    - _Micro-exercise_: Refactor kode Level 1 menggunakan fungsi yang tepat
33. `[[33. Scope — Local, Global & Block Scope]]`
    
    - Scope sebagai "wilayah akses" variabel
    - Global scope, function scope, block scope
    - Mengapa `var` berbeda dengan `let` dan `const` dalam scope
    - Temporal Dead Zone (TDZ) untuk `let` dan `const`
    - Masalah scope yang umum dan cara menghindarinya
    - _Micro-exercise_: Debug 5 kode dengan masalah scope
34. `[[34. Hoisting — Perilaku Unik JavaScript yang Wajib Dipahami]]`
    
    - Apa yang sebenarnya terjadi saat JavaScript "hoisting"
    - Variable hoisting: `var` vs `let`/`const`
    - Function hoisting: declaration vs expression
    - Mengapa hoisting bisa menyebabkan bug tersembunyi
    - _Micro-exercise_: Prediksi output 10 kode dengan hoisting, verifikasi
35. `[[35. Closure — Konsep Paling Penting di JavaScript]]`
    
    - Apa itu closure: fungsi yang "mengingat" scope pembuatnya
    - Lexical scoping: aturan dasar closure
    - Use case closure: data privacy, factory function, memoization
    - Closure dan memory: potensi memory leak
    - _Micro-exercise_: Buat counter dengan closure (tanpa class, tanpa global variable)
36. `[[36. this Keyword — Konteks yang Berubah-ubah]]`
    
    - `this` bukan tentang fungsi, tapi tentang _bagaimana_ fungsi dipanggil
    - `this` di global scope, function biasa, method object, arrow function
    - Explicit binding: `.call()`, `.apply()`, `.bind()`
    - Kesalahan umum `this` dan cara menghindarinya
    - _Micro-exercise_: Debug 5 kode dengan masalah `this`
37. `[[37. First-Class Function & Higher-Order Function]]`
    
    - Fungsi sebagai nilai: disimpan di variabel, dikirim sebagai argumen, dikembalikan
    - Higher-order function: menerima atau mengembalikan fungsi
    - Koneksi ke: `.map()`, `.filter()`, `.reduce()` (yang sudah dipelajari)
    - Membuat HOF sendiri: `pipe`, `compose` sederhana
    - _Micro-exercise_: Buat fungsi `repeat(fn, n)` yang menjalankan `fn` sebanyak `n` kali
38. `[[38. Fungsi Rekursif — Fungsi yang Memanggil Dirinya Sendiri]]`
    
    - Konsep rekursi: base case dan recursive case
    - Koneksi ke: Stack (Poin 31) — setiap rekursi menambah call stack
    - Rekursi vs iterasi: kapan gunakan yang mana
    - Tail call optimization (konsep)
    - _Micro-exercise_: Faktorial, Fibonacci, flatten nested array dengan rekursi
39. `[[39. Pure Function, Side Effects & Immutability]]`
    
    - Pure function: input sama → output sama, tanpa side effects
    - Side effects: apa itu dan kapan bisa diterima
    - Immutability: mengapa tidak memodifikasi data asli
    - Koneksi ke: `.map()`, `.filter()` mengembalikan array baru (tidak mutate)
    - _Micro-exercise_: Identifikasi pure vs impure function, refactor ke pure
40. `[[40. Modules — CommonJS & ES Modules]]`
    
    - Mengapa modules: masalah global scope pollution
    - CommonJS: `require()` dan `module.exports` (Node.js)
    - ES Modules: `import` dan `export` (modern JavaScript)
    - Default export vs named export
    - _Micro-exercise_: Refactor project todo list (Poin 28) menjadi multi-file dengan modules

---

### F. Object-Oriented Programming dengan JavaScript

> 💡 **Benang Merah ke Sebelumnya**: Kita sudah paham object literal (Poin 27) dan closure (Poin 35). OOP di JavaScript dibangun di atas prototype — bukan class seperti Java/Python. Kita pelajari fondasi prototype dulu, baru class syntax.

text

```
Benang Merah Bagian F:
Object literal (Poin 27) → Factory function dengan closure →
Prototype chain → Class syntax (syntactic sugar) →
Inheritance → Pola OOP nyata
```

41. `[[41. Mengapa OOP — Masalah yang Diselesaikan]]`
    
    - Masalah saat program makin besar tanpa OOP
    - OOP sebagai cara mengorganisasi kode
    - 4 pilar OOP: Encapsulation, Inheritance, Polymorphism, Abstraction
    - JavaScript OOP vs OOP di bahasa lain: prototype-based
    - _Micro-exercise_: Identifikasi bagian kode yang bisa diorganisasi dengan OOP
42. `[[42. Prototype Chain — Fondasi OOP JavaScript yang Sebenarnya]]`
    
    - Setiap object di JS punya prototype
    - Prototype chain: pencarian properti naik ke atas
    - `__proto__` vs `prototype`
    - `Object.create()` untuk membuat object dengan prototype custom
    - Mengapa memahami prototype penting meski pakai class syntax
    - _Micro-exercise_: Trace prototype chain dari berbagai object
43. `[[43. Class Syntax — Cara Modern Mendefinisikan Object Blueprint]]`
    
    - Class sebagai syntactic sugar di atas prototype
    - `constructor()`, properti, dan method
    - Instance vs class (static) method dan properti
    - Private fields dengan `#` (enkapsulasi nyata di JS)
    - Getter dan Setter
    - _Micro-exercise_: Buat class `BankAccount` dengan deposit, withdraw, cek saldo
44. `[[44. Inheritance — extends & super]]`
    
    - Mewarisi properti dan method dengan `extends`
    - `super()` di constructor dan method
    - Method overriding — polimorfisme
    - Kapan inheritance tepat dan kapan tidak
    - _Micro-exercise_: Extend `BankAccount` menjadi `SavingsAccount` dan `CheckingAccount`
45. `[[45. Komposisi vs Inheritance — Memilih Pendekatan yang Tepat]]`
    
    - Masalah "deep inheritance chain"
    - Komposisi: "has-a" vs inheritance: "is-a"
    - Mixin pattern di JavaScript
    - Prinsip: "Favor composition over inheritance"
    - _Micro-exercise_: Refactor sistem bank menggunakan komposisi

---

### 🏗️ Proyek Level 2

text

```
PROYEK: "Sistem Manajemen Perpustakaan CLI"
───────────────────────────────────────────
Fitur:
├── Tambah, edit, hapus, cari buku (CRUD lengkap)
├── Sistem member dengan class (Member, Premium Member)
├── Peminjaman dan pengembalian buku
├── Laporan statistik (buku terpopuler, member aktif)
├── Data disimpan di file JSON (persistensi sederhana)
└── Repository di GitHub dengan commit yang rapi

Konsep yang dilatih:
Array of objects, OOP (class, inheritance), modules, 
file I/O, error handling, Git workflow
```

---

## 🟠 LEVEL 3: ASYNCHRONOUS JS, ALGORITMA & NODE.JS (Bulan 4-7)

> **Tema Level**: _"Dari program lokal ke program yang terhubung dengan dunia luar"_  
> **Benang Merah**: Cara kerja JS runtime → Async programming → Algoritma untuk efisiensi → Database & API  
> **Output Level**: Bisa membuat backend API sederhana yang terhubung ke database

---

### G. Event Loop & Asynchronous JavaScript

> 💡 **Mengapa ini di Level 3?** Ini adalah topik yang paling sering disalahpahami. Kita perlu memahami cara kerja JS runtime (Event Loop) sebelum bisa menulis async code yang benar.

text

```
Benang Merah Bagian G:
Stack (Poin 31) + JS Runtime → Event Loop →
Callback → Promise → async/await → 
Pola async lanjutan
```

46. `[[46. JavaScript Runtime — V8, Call Stack, Memory Heap]]`
    
    - Bagaimana V8 menjalankan kode JavaScript
    - Call Stack: urutan eksekusi fungsi (koneksi ke Stack Poin 31)
    - Memory Heap: di mana object disimpan
    - Single-threaded: mengapa JS hanya bisa satu hal sekaligus
    - _Micro-exercise_: Trace manual call stack dari kode bertingkat
47. `[[47. Event Loop — Jantung Asynchronous JavaScript]]`
    
    - Masalah: single-thread tapi perlu operasi yang "menunggu"
    - Web APIs / Node.js APIs: yang menghandle operasi async
    - Callback Queue (Task Queue) dan Microtask Queue
    - Event Loop: bagaimana memindahkan task ke call stack
    - `setTimeout`, `setInterval` — bukan benar-benar "timer"
    - _Micro-exercise_: Prediksi urutan output kode async yang kompleks
48. `[[48. Callback — Pola Async Pertama & Masalahnya]]`
    
    - Callback sebagai solusi asynchronous pertama
    - Callback pattern: error-first callback (Node.js style)
    - Callback hell: masalah saat callback bersarang dalam
    - _Micro-exercise_: Simulasi operasi async dengan callback, buat callback hell, rasakan masalahnya
49. `[[49. Promise — Solusi Callback Hell]]`
    
    - Promise sebagai "janji" hasil operasi async
    - State promise: pending, fulfilled, rejected
    - `.then()`, `.catch()`, `.finally()`
    - Promise chaining — menggantikan callback hell
    - `Promise.all()`, `Promise.race()`, `Promise.allSettled()`, `Promise.any()`
    - _Micro-exercise_: Refactor kode callback hell menjadi Promise chain
50. `[[50. async/await — Cara Menulis Async Code seperti Synchronous]]`
    
    - `async/await` sebagai syntactic sugar di atas Promise
    - `async` function selalu mengembalikan Promise
    - `await` hanya bisa di dalam `async` function
    - Error handling dengan `try/catch` di async function
    - Parallel execution: `await Promise.all([...])`
    - _Micro-exercise_: Refactor Promise chain menjadi async/await
51. `[[51. Pola Async Lanjutan & Error Handling yang Baik]]`
    
    - Retry pattern: mencoba ulang operasi yang gagal
    - Timeout pattern: membatalkan operasi yang terlalu lama
    - Unhandled promise rejection — bahaya tersembunyi
    - Async dalam loop: masalah umum dan solusinya
    - _Micro-exercise_: Buat fungsi `fetchWithRetry(url, maxRetries)`

---

### H. Node.js Ecosystem & Backend Fundamentals

> 💡 **Benang Merah**: Setelah paham async JavaScript, kita mulai eksplorasi Node.js — runtime yang memungkinkan JavaScript berjalan di server.

text

```
Benang Merah Bagian H:
Async JS (Poin 46-51) → Node.js core modules →
File system & streams → npm & package management →
HTTP server → REST API
```

52. `[[52. Node.js Fundamentals — global, process, Buffer, __dirname]]`
    
    - Node.js bukan browser — perbedaan environment
    - Global objects: `global`, `process`, `__dirname`, `__filename`
    - `process.argv`, `process.env` — argumen dan environment variables
    - Buffer: bekerja dengan binary data
    - _Micro-exercise_: Buat CLI tool yang menerima argumen dari command line
53. `[[53. File System (fs) Module — Baca Tulis File]]`
    
    - `fs.readFile`, `fs.writeFile` — async dengan callback dan Promise
    - `fs.readFileSync`, `fs.writeFileSync` — sync (kapan layak digunakan)
    - `fs.appendFile`, `fs.unlink`, `fs.mkdir`, `fs.readdir`
    - Stream untuk file besar
    - _Micro-exercise_: Buat sistem log sederhana yang menulis ke file
54. `[[54. npm — Package Manager & Ekosistem JavaScript]]`
    
    - npm sebagai "toko" package JavaScript
    - `package.json`: metadata dan dependency project
    - `npm install`, `npm run`, semantic versioning
    - `devDependencies` vs `dependencies`
    - `.npmrc` dan lockfile (`package-lock.json`)
    - _Micro-exercise_: Setup project dengan npm, tambahkan beberapa package
55. `[[55. HTTP & Web — Bagaimana Internet Bekerja]]`
    
    - Arsitektur Client-Server
    - HTTP: method (GET, POST, PUT, DELETE, PATCH), status code, headers
    - Request-Response cycle
    - REST: prinsip dan konvensi
    - _Micro-exercise_: Gambarkan request-response cycle untuk operasi CRUD
56. `[[56. Membuat HTTP Server dengan Node.js — Tanpa Framework]]`
    
    - `http` module: `createServer`, `req`, `res`
    - Routing manual: memeriksa `req.url` dan `req.method`
    - Mengirim response: status code, headers, body
    - Parsing request body
    - _Micro-exercise_: Buat server sederhana dengan 3 route (/, /about, /api/data)
57. `[[57. Express.js — Framework Web untuk Node.js]]`
    
    - Mengapa Express: routing, middleware, lebih mudah dari http mentah
    - Setup Express, routing dasar
    - Route parameters dan query strings
    - Middleware: fungsi yang berjalan sebelum route handler
    - `express.json()`, `express.static()`, custom middleware
    - Error handling middleware
    - _Micro-exercise_: Refactor HTTP server dari Poin 56 menggunakan Express
58. `[[58. REST API — Merancang & Implementasi CRUD API]]`
    
    - REST API design: naming convention, HTTP method yang tepat
    - Membangun CRUD API dengan Express (data in-memory dulu)
    - Request validation
    - Response format yang konsisten
    - Testing API dengan Thunder Client / Postman
    - _Micro-exercise_: Buat REST API untuk todo list (CRUD lengkap)
59. `[[59. Environment Variables & Konfigurasi]]`
    
    - Mengapa tidak boleh hardcode konfigurasi di kode
    - `dotenv` package: `.env` file
    - Konfigurasi berbeda untuk development, staging, production
    - Menyimpan secret: API key, password database
    - _Micro-exercise_: Pindahkan semua konfigurasi project ke .env

---

### I. Database — Menyimpan Data Secara Permanen

> 💡 **Benang Merah**: Kita sudah membuat REST API dengan data in-memory (hilang saat server restart). Sekarang kita integrasikan database untuk persistensi data.

text

```
Benang Merah Bagian I:
REST API (Poin 58) + kebutuhan persistensi → 
SQL fundamentals → PostgreSQL → 
ORM (Prisma) → Integrasi dengan Express API
```

60. `[[60. Pengantar Database — SQL vs NoSQL & Kapan Menggunakan]]`
    
    - Mengapa kita perlu database (vs file JSON)
    - SQL: terstruktur, relasional, ACID
    - NoSQL: fleksibel, berbagai model (document, key-value, graph)
    - Kapan pilih SQL, kapan NoSQL
    - _Micro-exercise_: Rancang skema database untuk sistem perpustakaan (Level 2 project)
61. `[[61. SQL Fundamentals — DDL & DML]]`
    
    - DDL: `CREATE TABLE`, `ALTER TABLE`, `DROP TABLE`
    - Tipe data SQL: `INTEGER`, `VARCHAR`, `TEXT`, `BOOLEAN`, `TIMESTAMP`
    - DML: `SELECT`, `INSERT`, `UPDATE`, `DELETE`
    - `WHERE`, `ORDER BY`, `LIMIT`, `OFFSET`
    - _Micro-exercise_: Buat dan isi database perpustakaan, query 10 skenario berbeda
62. `[[62. SQL Lanjutan — JOIN, Agregasi & Subquery]]`
    
    - Relasi antar tabel: one-to-one, one-to-many, many-to-many
    - `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`
    - Agregasi: `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`, `GROUP BY`, `HAVING`
    - Subquery
    - Indeks: apa itu dan mengapa penting untuk performa
    - _Micro-exercise_: Query kompleks: buku yang belum dikembalikan beserta data member
63. `[[63. PostgreSQL & Setup — Database Production-Ready]]`
    
    - Mengapa PostgreSQL (vs SQLite vs MySQL)
    - Instalasi dan setup PostgreSQL
    - `psql` CLI: perintah dasar
    - Membuat database dan user
    - _Micro-exercise_: Migrate skema perpustakaan ke PostgreSQL
64. `[[64. Prisma ORM — Jembatan JavaScript & Database]]`
    
    - Mengapa ORM: type-safe, lebih produktif, abstraksi SQL
    - Setup Prisma: `schema.prisma`, `prisma migrate`
    - Prisma Client: CRUD operations
    - Relasi di Prisma: one-to-many, many-to-many
    - _Micro-exercise_: Implementasi CRUD todo API dengan Prisma + PostgreSQL
65. `[[65. Integrasi — REST API + Express + Prisma + PostgreSQL]]`
    
    - Menggabungkan semua: Express route → Prisma → PostgreSQL
    - Repository pattern: memisahkan logika database dari route
    - Pagination dengan `skip` dan `take`
    - Filtering dan sorting dari query parameter
    - _Micro-exercise_: Bangun API lengkap untuk sistem perpustakaan

---

### J. Algoritma & Struktur Data Lanjutan

> 💡 **Benang Merah**: Dengan kemampuan membuat API dan bekerja dengan data, sekarang kita fokus pada efisiensi — bagaimana memilih algoritma dan struktur data yang tepat untuk masalah yang lebih kompleks.

text

```
Benang Merah Bagian J:
Pengalaman dengan Array/Object (Level 2) →
Analisis efisiensi (Big-O) → Algoritma searching & sorting →
Struktur data lanjutan → Penerapan dalam kode nyata
```

66. `[[66. Analisis Algoritma & Big-O Notation]]`
    
    - Mengapa analisis algoritma: kode bisa benar tapi lambat
    - Time complexity dan space complexity
    - Big-O: O(1), O(log n), O(n), O(n log n), O(n²)
    - Cara menganalisis kode: loop, nested loop, rekursi
    - _Micro-exercise_: Analisis Big-O dari 10 fungsi yang sudah pernah ditulis
67. `[[67. Algoritma Pencarian — Linear & Binary Search]]`
    
    - Linear search: O(n), kapan digunakan
    - Binary search: O(log n), syarat data harus sorted
    - Implementasi binary search (iteratif dan rekursif)
    - _Micro-exercise_: Benchmark linear vs binary search pada array besar
68. `[[68. Algoritma Pengurutan Dasar — Bubble, Selection, Insertion Sort]]`
    
    - Bubble sort: O(n²), konsep dasar
    - Selection sort: O(n²), mencari minimum setiap iterasi
    - Insertion sort: O(n²), efisien untuk data hampir terurut
    - _Micro-exercise_: Implementasi dan visualisasi langkah-langkah sorting
69. `[[69. Algoritma Pengurutan Efisien — Merge Sort & Quick Sort]]`
    
    - Merge sort: O(n log n), divide and conquer, stabil
    - Quick sort: O(n log n) rata-rata, in-place, pivot selection
    - Kapan gunakan yang mana
    - JavaScript `.sort()` di balik layar
    - _Micro-exercise_: Benchmark semua sorting algorithm pada berbagai ukuran data
70. `[[70. Linked List — Struktur Data Dinamis]]`
    
    - Masalah array: insersi/penghapusan di tengah O(n)
    - Singly Linked List: node, pointer, head
    - Operasi: insert, delete, search, traverse
    - Doubly Linked List: traversal dua arah
    - Kapan Linked List lebih baik dari Array
    - _Micro-exercise_: Implementasi Linked List dengan semua operasi
71. `[[71. Tree & Binary Search Tree]]`
    
    - Tree: hierarki, terminologi (root, node, leaf, height, depth)
    - Binary Tree: setiap node max 2 anak
    - Binary Search Tree (BST): properti dan operasi
    - Tree traversal: Inorder, Preorder, Postorder
    - _Micro-exercise_: Implementasi BST dengan insert, search, dan traversal
72. `[[72. Graph — Representasi & Traversal]]`
    
    - Graph: node (vertex) dan edge
    - Representasi: adjacency list (lebih efisien untuk sparse graph)
    - BFS: menemukan path terpendek (jumlah edge)
    - DFS: menjelajah semua path
    - Use case: social network, peta, dependency resolution
    - _Micro-exercise_: Implementasi BFS untuk mencari jalur terpendek di peta kota
73. `[[73. Dynamic Programming — Pengantar]]`
    
    - Masalah yang bisa diselesaikan DP: optimal substructure, overlapping subproblems
    - Memoization (top-down) vs Tabulation (bottom-up)
    - Contoh klasik: Fibonacci dengan DP, 0/1 Knapsack
    - _Micro-exercise_: Bandingkan Fibonacci rekursif biasa vs dengan memoization (ukur waktu)

---

### 🏗️ Proyek Level 3

text

```
PROYEK: "REST API Sistem Perpustakaan (Full Backend)"
──────────────────────────────────────────────────────
Fitur:
├── Authentication: register, login, JWT token
├── CRUD buku, anggota, peminjaman
├── Search dan filter dengan query parameter
├── Pagination
├── Role-based access (admin vs member)
├── Rate limiting (mencegah abuse)
├── Logging setiap request
├── Database: PostgreSQL + Prisma
└── Deploy ke Railway/Render

Konsep yang dilatih:
Express.js, PostgreSQL, Prisma, JWT, 
async/await, error handling, environment variables
```

---

## 🔴 LEVEL 4: FRONTEND, TYPESCRIPT & CLEAN CODE (Bulan 7-10)

> **Tema Level**: _"Dari backend ke fullstack, dari kode yang bekerja ke kode yang baik"_  
> **Benang Merah**: Web fundamentals → Frontend modern → TypeScript → Clean Code & Testing  
> **Output Level**: Bisa membuat fullstack web application dengan kode berkualitas

---

### K. Web Frontend Fundamentals

> 💡 **Benang Merah**: Kita sudah buat backend API. Sekarang buat "wajah" yang bisa berinteraksi dengan user dan mengonsumsi API yang kita buat.

text

```
Benang Merah Bagian K:
REST API (Level 3) → HTML (struktur) → CSS (tampilan) →
JavaScript DOM → Fetch API → Framework (Vue.js)
```

74. `[[74. HTML5 Semantik — Struktur Halaman Web]]`
    
    - HTML sebagai struktur, bukan tampilan
    - Tag semantik: `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<footer>`
    - Form dan input: semua tipe input HTML5
    - Accessibility dasar: `alt`, `label`, `aria-*`
    - _Micro-exercise_: Buat struktur HTML untuk halaman perpustakaan (tanpa CSS)
75. `[[75. CSS3 — Styling, Flexbox & Grid]]`
    
    - Box model: margin, border, padding, content
    - Flexbox: layout satu dimensi
    - Grid: layout dua dimensi
    - Custom properties (CSS variables)
    - Responsive design: media queries dan mobile-first
    - _Micro-exercise_: Style halaman perpustakaan dengan CSS murni
76. `[[76. Tailwind CSS — Utility-First CSS]]`
    
    - Mengapa Tailwind: developer experience yang lebih cepat
    - Setup Tailwind dengan Vite
    - Utility classes: spacing, typography, color, flexbox, grid
    - Responsive utilities
    - _Micro-exercise_: Rebuild halaman perpustakaan dengan Tailwind
77. `[[77. JavaScript di Browser — DOM Manipulation]]`
    
    - DOM: Document Object Model
    - Mengakses element: `getElementById`, `querySelector`
    - Memodifikasi DOM: `textContent`, `innerHTML`, `classList`
    - Membuat dan menghapus element
    - _Micro-exercise_: Buat todo list yang dinamis (tambah/hapus tanpa reload)
78. `[[78. Event Handling — Interaktivitas di Browser]]`
    
    - Event listener: `addEventListener`, tipe event
    - Event object: target, preventDefault, stopPropagation
    - Event delegation: satu listener untuk banyak element
    - Form submission dan validasi di browser
    - _Micro-exercise_: Tambahkan interaktivitas ke halaman perpustakaan
79. `[[79. Fetch API & Mengonsumsi REST API]]`
    
    - Fetch API: cara modern membuat HTTP request dari browser
    - Async/await dengan Fetch (koneksi ke Poin 50)
    - Error handling saat fetch gagal
    - Menampilkan data dari API ke DOM
    - CORS: mengapa ada dan cara mengatasinya
    - _Micro-exercise_: Hubungkan frontend perpustakaan dengan backend API (Level 3)
80. `[[80. Vue.js Fundamentals — Framework Frontend Progresif]]`
    
    - Mengapa framework: reaktivitas otomatis, komponen, ekosistem
    - Setup Vue dengan Vite
    - Template syntax, directives (`v-if`, `v-for`, `v-bind`, `v-on`, `v-model`)
    - Component: props, emits
    - Composition API: `ref`, `reactive`, `computed`, `watch`
    - _Micro-exercise_: Rebuild todo list menggunakan Vue component
81. `[[81. State Management & Vue Router — Aplikasi Multi-Halaman]]`
    
    - Mengapa state management: sharing data antar komponen
    - Pinia: state management modern untuk Vue
    - Vue Router: navigasi multi-halaman (SPA)
    - _Micro-exercise_: Buat frontend perpustakaan multi-halaman dengan Vue + Pinia + Router

---

### L. TypeScript — JavaScript dengan Superpower

> 💡 **Mengapa TypeScript di Level 4?** TypeScript hampir wajib di industri. Setelah mahir JS, TypeScript adalah next logical step yang meningkatkan kualitas kode secara signifikan.

text

```
Benang Merah Bagian L:
Masalah JS tanpa type → TypeScript basic types →
Interface & Type → Generics → 
TypeScript di Express & Vue
```

82. `[[82. Mengapa TypeScript — Masalah yang Diselesaikan]]`
    
    - Masalah JavaScript: tipe bisa salah di runtime
    - TypeScript: type checking saat development (compile time)
    - TypeScript bukan bahasa baru, tapi superset JavaScript
    - _Micro-exercise_: Cari 5 bug tipe di kode JavaScript yang sudah dibuat
83. `[[83. TypeScript Basic Types & Type Annotation]]`
    
    - Tipe primitif: `string`, `number`, `boolean`
    - `any`, `unknown`, `never`, `void`
    - Array types: `string[]`, `Array<string>`
    - Union types: `string | number`
    - Type inference: TypeScript bisa menebak tipe
    - _Micro-exercise_: Annotasi tipe untuk semua fungsi di project todo list
84. `[[84. Interface & Type Alias — Mendefinisikan Struktur Data]]`
    
    - `interface` untuk mendefinisikan bentuk object
    - `type` alias: lebih fleksibel dari interface
    - Perbedaan `interface` vs `type`
    - Optional properties (`?`) dan readonly properties
    - Extending interface dan intersection types
    - _Micro-exercise_: Buat interface untuk semua entity di sistem perpustakaan
85. `[[85. Generics — Fungsi & Komponen yang Fleksibel]]`
    
    - Masalah tanpa generics: kode duplikat atau pakai `any`
    - Generic function: `function identity<T>(arg: T): T`
    - Generic interface dan class
    - Constraints: `<T extends SomeType>`
    - _Micro-exercise_: Refactor fungsi Array dari Level 2 menggunakan generics
86. `[[86. TypeScript di Express.js — Backend Type-Safe]]`
    
    - Setup TypeScript di Express project
    - Typing request dan response
    - Type-safe middleware
    - _Micro-exercise_: Migrate project perpustakaan backend ke TypeScript
87. `[[87. TypeScript di Vue.js — Frontend Type-Safe]]`
    
    - Setup TypeScript di Vue + Vite
    - Composition API dengan TypeScript
    - Type-safe props dan emits
    - _Micro-exercise_: Migrate project perpustakaan frontend ke TypeScript

---

### M. Clean Code, Testing & Software Engineering Principles

> 💡 **Benang Merah**: Kode yang bekerja itu syarat minimum. Kode yang _baik_ — mudah dibaca, ditest, dan diubah — itu yang membedakan junior dan senior developer.

text

```
Benang Merah Bagian M:
Kode bekerja (semua level sebelumnya) → 
Clean Code principles → Testing → 
Design Patterns → SOLID
```

88. `[[88. Clean Code — Prinsip Menulis Kode yang Dibaca Manusia]]`
    
    - Naming: variabel, fungsi, class yang ekspresif
    - Fungsi kecil dan satu tanggung jawab
    - Menghindari magic numbers dan magic strings
    - Komentar yang baik vs komentar yang tidak perlu
    - _Micro-exercise_: Refactor kode jelek menjadi clean code
89. `[[89. SOLID Principles — 5 Prinsip OOP yang Baik]]`
    
    - S: Single Responsibility Principle
    - O: Open/Closed Principle
    - L: Liskov Substitution Principle
    - I: Interface Segregation Principle
    - D: Dependency Inversion Principle
    - _Micro-exercise_: Identifikasi pelanggaran SOLID di project perpustakaan, refactor
90. `[[90. DRY, KISS & YAGNI — Prinsip Pragmatis]]`
    
    - DRY: Don't Repeat Yourself
    - KISS: Keep It Simple, Stupid
    - YAGNI: You Aren't Gonna Need It
    - _Micro-exercise_: Audit kode project, terapkan ketiga prinsip
91. `[[91. Unit Testing dengan Vitest — Test Driven Development]]`
    
    - Mengapa testing: kepercayaan diri saat refactor
    - Unit test: test satu fungsi/unit terisolasi
    - Vitest: fast testing framework untuk Vite ecosystem
    - Arrange-Act-Assert pattern
    - Mocking: mengganti dependency dengan tiruan
    - TDD workflow: red → green → refactor
    - _Micro-exercise_: Tulis test untuk semua fungsi utility di project perpustakaan
92. `[[92. Integration Testing & E2E Testing]]`
    
    - Integration test: test beberapa unit bekerja bersama
    - Testing API endpoint dengan Supertest
    - E2E testing dengan Playwright: simulasi user di browser
    - _Micro-exercise_: Buat E2E test untuk flow utama aplikasi perpustakaan
93. `[[93. Design Patterns — Creational]]`
    
    - Singleton: satu instance untuk seluruh aplikasi
    - Factory: membuat object tanpa tahu class konkretnya
    - Builder: membangun object kompleks step by step
    - Implementasi dalam JavaScript/TypeScript
    - _Micro-exercise_: Implementasi Singleton untuk database connection
94. `[[94. Design Patterns — Structural & Behavioral]]`
    
    - Structural: Adapter, Decorator, Facade
    - Behavioral: Observer, Strategy, Command
    - Contoh nyata dalam framework yang sudah digunakan
    - _Micro-exercise_: Implementasi Observer pattern untuk sistem notifikasi
95. `[[95. Code Refactoring & Technical Debt]]`
    
    - Code smells: tanda-tanda kode yang perlu refactor
    - Teknik refactoring: extract function, rename, move, inline
    - Technical debt: kapan harus dibayar
    - _Micro-exercise_: Audit dan refactor project Level 2 atau 3 menggunakan semua prinsip
96. `[[96. Debugging Sistematis — Teknik & Tools]]`
    
    - Debugging bukan "coba-coba" — ada metodologi
    - Chrome DevTools: breakpoint, watch expressions, network tab
    - Node.js debugger
    - Logging yang efektif
    - _Micro-exercise_: Debug 5 kode buggy secara sistematis (tanpa console.log saja)

---

### 🏗️ Proyek Level 4

text

```
PROYEK: "Fullstack Aplikasi Perpustakaan"
──────────────────────────────────────────
Fitur:
├── Frontend: Vue.js + TypeScript + Tailwind
├── Backend: Express.js + TypeScript + Prisma
├── Authentication lengkap (register, login, JWT)
├── Admin dashboard: kelola buku, anggota
├── Member portal: cari buku, pinjam, lihat history
├── Test coverage minimal 70%
├── CI/CD sederhana dengan GitHub Actions
└── Dokumentasi API dengan Swagger

Konsep yang dilatih:
Fullstack integration, TypeScript, testing, 
clean code, SOLID, design patterns
```

---

## 🟣 LEVEL 5: ADVANCED — SKALA, PERFORMA & KEAMANAN (Bulan 10-14)

> **Tema Level**: _"Dari aplikasi yang berjalan ke aplikasi yang bisa dipercaya dan diskalakan"_  
> **Benang Merah**: Aplikasi fullstack (Level 4) → Keamanan → Performa → Concurrency → DevOps  
> **Output Level**: Bisa deploy dan maintain aplikasi production-ready

---

### N. Security — Keamanan Aplikasi Web

> 💡 **Mengapa Security lebih awal di Level 5?** Keamanan bukan fitur yang ditambahkan belakangan — ini harus dipikirkan sejak arsitektur. Kita pelajari ini sebelum scaling.

text

```
Benang Merah Bagian N:
Aplikasi fullstack (Level 4) → 
OWASP vulnerabilities → Authentication & Authorization →
Enkripsi → Secure coding practices
```

97. `[[97. OWASP Top 10 — Ancaman Keamanan Web Paling Umum]]`
    
    - Broken Access Control, Injection, XSS, CSRF, dll
    - Mengidentifikasi celah di aplikasi yang sudah dibuat
    - _Micro-exercise_: Audit project Level 4 terhadap OWASP Top 10
98. `[[98. Input Validation, Sanitization & SQL Injection Prevention]]`
    
    - Validasi: memastikan input sesuai format
    - Sanitasi: membersihkan input dari karakter berbahaya
    - SQL Injection: mengapa Prisma/ORM melindungi, tapi tidak 100%
    - _Micro-exercise_: Tambahkan validasi lengkap ke semua endpoint API
99. `[[99. XSS & CSRF — Serangan Client-Side]]`
    
    - XSS: memasukkan script berbahaya ke halaman web
    - Content Security Policy (CSP)
    - CSRF: memaksa user melakukan aksi tanpa sepengetahuannya
    - CSRF token dan SameSite cookie
    - _Micro-exercise_: Demonstrasi XSS dan CSRF, lalu tambahkan proteksi
100. `[[100. Authentication & Authorization Mendalam — JWT & OAuth 2.0]]`
    
    - Perbedaan authentication dan authorization
    - JWT: struktur, signing, verifikasi, refresh token
    - OAuth 2.0: login dengan Google/GitHub
    - Role-based Access Control (RBAC)
    - _Micro-exercise_: Implementasi OAuth 2.0 dan RBAC ke project perpustakaan
101. `[[101. Hashing, Enkripsi & HTTPS]]`
    
    - Hashing vs enkripsi: beda tujuan, beda cara
    - bcrypt untuk password hashing
    - Enkripsi simetris (AES) dan asimetris (RSA) — konsep
    - HTTPS, TLS: cara kerja dan mengapa wajib
    - _Micro-exercise_: Audit penyimpanan password dan data sensitif di project
102. `[[102. Secure Coding Practices & Secret Management]]`
    
    - Prinsip least privilege
    - Secrets tidak boleh di kode atau git
    - Rate limiting dan brute force protection
    - Security headers (Helmet.js)
    - _Micro-exercise_: Audit dan hardening seluruh project

---

### O. Performa & Advanced JavaScript

> 💡 **Benang Merah**: Dengan aplikasi yang aman, sekarang kita optimasi agar cepat dan efisien.

text

```
Benang Merah Bagian O:
Aplikasi aman (Poin 97-102) →
JS Advanced (Generator, Proxy) →
Memory management →
Concurrency di JavaScript →
Performance optimization
```

103. `[[103. Generator & Iterator — Kontrol Eksekusi yang Presisi]]`
    
    - Iterator protocol: `Symbol.iterator`
    - Generator function: `function*` dan `yield`
    - Use case: lazy evaluation, infinite sequences, async generator
    - _Micro-exercise_: Buat paginator data menggunakan generator
104. `[[104. Proxy & Reflect — Metaprogramming di JavaScript]]`
    
    - Proxy: intercept operasi pada object
    - Reflect: API untuk operasi object
    - Use case: validasi otomatis, logging, reactive system
    - Koneksi ke: bagaimana Vue.js reaktivitas bekerja di balik layar
    - _Micro-exercise_: Buat validation proxy untuk object
105. `[[105. Memory Management & Memory Leaks di JavaScript]]`
    
    - Garbage collector: cara kerja di V8
    - Referensi dan GC roots
    - Penyebab memory leak umum: event listener, closure, global variable
    - Mendeteksi memory leak dengan Chrome DevTools
    - WeakMap dan WeakSet: untuk referensi yang tidak menghalangi GC
    - _Micro-exercise_: Identifikasi dan fix memory leak di kode sample
106. `[[106. Concurrency di JavaScript — Web Workers & Worker Threads]]`
    
    - JavaScript single-threaded — limitasi untuk CPU-intensive task
    - Web Workers: thread terpisah di browser
    - Worker Threads: thread terpisah di Node.js
    - SharedArrayBuffer dan Atomics
    - Kapan perlu workers, kapan tidak
    - _Micro-exercise_: Pindahkan komputasi berat ke Worker Thread
107. `[[107. Performance Optimization — Frontend & Backend]]`
    
    - Frontend: Core Web Vitals, lazy loading, code splitting
    - Backend: caching strategy, N+1 query problem, database indexing
    - Profiling: menemukan bottleneck sebelum optimasi
    - _Micro-exercise_: Profile dan optimasi project fullstack perpustakaan
108. `[[108. Caching — Strategi & Implementasi]]`
    
    - Mengapa cache: mengurangi komputasi berulang
    - In-memory cache: `Map` sederhana, LRU Cache
    - Redis: distributed cache
    - HTTP caching: `Cache-Control`, `ETag`
    - Cache invalidation: masalah tersulit
    - _Micro-exercise_: Implementasi Redis caching untuk endpoint yang sering diakses

---

### P. DevOps & Deployment

> 💡 **Benang Merah**: Aplikasi yang baik dan aman harus bisa diakses oleh pengguna nyata. DevOps menghubungkan development dengan production.

text

```
Benang Merah Bagian P:
Project fullstack yang siap → Linux basics →
Docker → CI/CD → Cloud deployment → Monitoring
```

109. `[[109. Linux Command Line — Intermediate]]`
    
    - Navigasi, file management, permission
    - Process management: `ps`, `kill`, `top`
    - Network tools: `curl`, `wget`, `netstat`
    - Shell scripting dasar untuk otomasi
    - _Micro-exercise_: Buat shell script untuk setup server dari nol
110. `[[110. Docker — Containerization]]`
    
    - Masalah tanpa Docker: "di komputer saya bisa"
    - Image vs Container
    - `Dockerfile`: mendefinisikan image
    - `docker build`, `docker run`, `docker ps`, `docker logs`
    - _Micro-exercise_: Containerize backend API perpustakaan
111. `[[111. Docker Compose — Multi-Container Application]]`
    
    - Orchestrasi beberapa container (app + database + redis)
    - `docker-compose.yml`
    - Networking antar container
    - Volume untuk persistensi data
    - _Micro-exercise_: Docker Compose untuk fullstack project (frontend + backend + PostgreSQL + Redis)
112. `[[112. CI/CD — GitHub Actions]]`
    
    - CI: otomatis test setiap push
    - CD: otomatis deploy jika test lulus
    - GitHub Actions: workflow, jobs, steps
    - Secrets di GitHub Actions
    - _Micro-exercise_: Setup pipeline: push → test → build Docker → deploy
113. `[[113. Cloud Deployment — VPS & Platform-as-a-Service]]`
    
    - PaaS: Railway, Render (mudah, untuk awal)
    - VPS: DigitalOcean, Linode (lebih kontrol)
    - Nginx sebagai reverse proxy
    - SSL dengan Let's Encrypt
    - _Micro-exercise_: Deploy fullstack project ke VPS dengan Nginx dan SSL
114. `[[114. Monitoring & Logging — Mengetahui Apa yang Terjadi di Production]]`
    
    - Logging: structured logging dengan Winston/Pino
    - Error tracking: Sentry
    - Uptime monitoring
    - Basic metrics: response time, error rate, throughput
    - _Micro-exercise_: Setup monitoring dan logging untuk project production

---

### 🏗️ Proyek Level 5

text

```
PROYEK: "Perpustakaan Digital — Production Ready"
──────────────────────────────────────────────────
Enhancement dari Level 4:
├── Security hardening (semua OWASP mitigations)
├── OAuth 2.0 login (Google/GitHub)
├── Redis caching untuk performa
├── Docker + Docker Compose
├── CI/CD pipeline lengkap
├── Deploy ke VPS dengan Nginx + SSL
├── Monitoring dengan Sentry + uptime monitor
├── Load testing dengan k6
└── Dokumentasi lengkap (API docs + deployment guide)
```

---

## ⚫ LEVEL 6: SPESIALISASI (Bulan 14+)

> **Tema Level**: _"Menentukan jalur karir dan menjadi expert di bidang pilihan"_  
> **Benang Merah**: Semua fondasi dari Level 1-5 → Spesialisasi mendalam  
> **Catatan**: Pilih SATU jalur, kuasai dengan dalam sebelum eksplorasi jalur lain.

---

### 🔹 Jalur A: Backend Engineering

text

```
Benang Merah Jalur A:
REST API (Level 3) + PostgreSQL + Docker →
System Design → Microservices → Distributed Systems
```

115. `[[115-A. System Design — Prinsip Merancang Sistem Berskala Besar]]`
116. `[[116-A. Database Lanjutan — Indexing, Query Optimization, Sharding, Replication]]`
117. `[[117-A. Redis Lanjutan — Pub/Sub, Streams, Lua Scripting]]`
118. `[[118-A. Message Queue — RabbitMQ & Apache Kafka]]`
119. `[[119-A. GraphQL — Alternatif REST API]]`
120. `[[120-A. gRPC — High-Performance RPC]]`
121. `[[121-A. Microservices — Patterns & Trade-offs]]`
122. `[[122-A. Distributed Systems — CAP Theorem, Consistency, Availability]]`
123. `[[123-A. API Gateway, Rate Limiting & Load Balancing]]`
124. `[[124-A. Event-Driven Architecture & CQRS]]`

### 🔹 Jalur B: Frontend Engineering

text

```
Benang Merah Jalur B:
Vue.js + TypeScript (Level 4) → 
Advanced State Management → SSR/SSG → 
Performance → Micro-frontend
```

125. `[[125-B. Nuxt.js — Meta-framework Vue untuk SSR & SSG]]`
126. `[[126-B. State Management Lanjutan — Pinia Patterns & Persistence]]`
127. `[[127-B. Web Performance — Core Web Vitals & Optimization]]`
128. `[[128-B. Progressive Web App (PWA) — Offline-First]]`
129. `[[129-B. Accessibility (a11y) — Standar WCAG]]`
130. `[[130-B. Testing Frontend Lanjutan — Vitest + Vue Test Utils + Playwright]]`
131. `[[131-B. Micro-Frontend Architecture]]`
132. `[[132-B. WebAssembly — High Performance di Browser]]`
133. `[[133-B. Animation & UX Engineering — GSAP, Framer Motion]]`

### 🔹 Jalur C: Fullstack (Next.js Ecosystem)

text

```
Benang Merah Jalur C:
Fullstack Vue+Express (Level 4) →
Next.js (React) ecosystem →
Edge computing → Serverless
```

134. `[[134-C. React Fundamentals — Dari Vue ke React]]`
135. `[[135-C. Next.js — Full-Stack React Framework]]`
136. `[[136-C. tRPC — Type-Safe API Tanpa Schema]]`
137. `[[137-C. Serverless & Edge Functions]]`
138. `[[138-C. Database di Serverless — PlanetScale, Neon, Turso]]`
139. `[[139-C. Monorepo dengan Turborepo]]`

### 🔹 Jalur D: DevOps / Platform Engineering

text

```
Benang Merah Jalur D:
Docker + CI/CD (Level 5) →
Kubernetes → Infrastructure as Code →
Observability → Security
```

140. `[[140-D. Kubernetes — Container Orchestration]]`
141. `[[141-D. Helm — Package Manager untuk Kubernetes]]`
142. `[[142-D. Terraform — Infrastructure as Code]]`
143. `[[143-D. Ansible — Configuration Management]]`
144. `[[144-D. Observability — Prometheus, Grafana, Jaeger]]`
145. `[[145-D. Chaos Engineering — Membangun Resilience]]`
146. `[[146-D. GitOps — Flux & ArgoCD]]`
147. `[[147-D. DevSecOps — Security dalam Pipeline]]`

---

### S. Soft Skills & Professional Growth

> Ini bukan bonus — ini **multiplier** yang menentukan seberapa jauh karir Anda berkembang.

148. `[[148. Membaca & Memahami Kode Orang Lain — Skill yang Sering Diabaikan]]`
149. `[[149. Menulis Dokumentasi Teknis yang Baik — README, ADR, Wiki]]`
150. `[[150. Estimasi Waktu & Task Breakdown — Agile & Scrum]]`
151. `[[151. Komunikasi Teknis — Menjelaskan ke Non-Teknis]]`
152. `[[152. Code Review — Memberi & Menerima Feedback yang Konstruktif]]`
153. `[[153. Membangun Portfolio & Personal Branding — GitHub, Blog, LinkedIn]]`
154. `[[154. Open Source Contribution — Cara Memulai]]`
155. `[[155. Technical Interview — Coding Interview & System Design Interview]]`
156. `[[156. Continuous Learning — Cara Tetap Relevan di Industri yang Bergerak Cepat]]`

---

## 📋 Tabel Benang Merah Antar Topik

text

```
Poin 8  (Boolean Logic)     →  Poin 15 (if/else)
Poin 15 (if/else)           →  Poin 16 (loop)
Poin 16 (loop)              →  Poin 24 (Array CRUD)
Poin 24 (Array)             →  Poin 25 (Array Methods)
Poin 27 (Object)            →  Poin 28 (Array of Objects)
Poin 31 (Stack/Queue)       →  Poin 46 (Call Stack JS)
Poin 31 (Stack/Queue)       →  Poin 70 (Linked List)
Poin 33 (Scope)             →  Poin 35 (Closure)
Poin 35 (Closure)           →  Poin 41 (OOP/Factory)
Poin 38 (Rekursi)           →  Poin 71 (Tree Traversal)
Poin 47 (Event Loop)        →  Poin 48 (Callback)
Poin 48 (Callback)          →  Poin 49 (Promise)
Poin 49 (Promise)           →  Poin 50 (async/await)
Poin 42 (Prototype)         →  Poin 43 (Class)
Poin 58 (REST API)          →  Poin 65 (API + Database)
Poin 56 (HTTP Node.js)      →  Poin 57 (Express.js)
Poin 80 (Vue.js)            →  Poin 79 (Fetch API)
Poin 82 (TypeScript Why)    →  Poin 83-87 (TS Implementation)
Poin 91 (Unit Test)         →  Poin 92 (Integration/E2E)
Poin 66 (Big-O)             →  Poin 67-69 (Searching/Sorting)
Poin 110 (Docker)           →  Poin 111 (Docker Compose)
Poin 111 (Docker Compose)   →  Poin 112 (CI/CD)
```

---

## 📊 Ringkasan & Tracking Progress

|Level|Rentang Poin|Durasi|Output|
|---|---|---|---|
|🟢 Level 1|1-19|Bulan 1-2|CLI Personal Assistant|
|🟡 Level 2|20-45|Bulan 2-4|Sistem Perpustakaan CLI|
|🟠 Level 3|46-73|Bulan 4-7|REST API Full Backend|
|🔴 Level 4|74-96|Bulan 7-10|Fullstack Web Application|
|🟣 Level 5|97-114|Bulan 10-14|Production-Ready App|
|⚫ Level 6|115+|Bulan 14+|Spesialisasi|

---

## 💡 Cara Menggunakan Roadmap Ini

text

```
Setiap topik mengikuti format:
┌─────────────────────────────────────────┐
│ 📖 Konsep: Apa dan mengapa             │
│ 🔗 Koneksi: Hubungan ke topik lain     │
│ 💻 Praktik: Micro-exercise langsung    │
│ 🏗️  Proyek: Kontribusi ke proyek level │
└─────────────────────────────────────────┘
```

**Aturan Tidak Boleh Dilanggar:**

1. **Jangan skip topik** — setiap poin adalah batu bata
2. **Selesaikan micro-exercise** sebelum lanjut
3. **Commit ke Git** setiap sesi belajar
4. **Bangun proyek level** sebelum naik level
5. **Review koneksi** — selalu tanya "ini berhubungan dengan apa?"

---

_Roadmap v2.0 — JavaScript-First, Connected, Practical_  
_Didesain dengan prinsip: tidak ada materi yang berdiri sendiri_

saya ingin membuat roadmap yang efektif dan efisien, tiap sub bab itu nyambung ga ada loncat loncat, ada benang merah tiap materi, dan juga untuk beberapa materi yang sudah tercapai, saya sudah bisa membuat project kecil, coba buatkan ulang roadmap yang lebih efektif dan baik sesuai perubahan dan evaluasi

