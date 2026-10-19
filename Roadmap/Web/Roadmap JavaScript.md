# 🗺 Roadmap JavaScript: Dari Zero to Hero dalam 12 Minggu

## Filosofi Roadmap Ini

> **"JavaScript adalah bahasa pemrograman yang demokratis — tidak perlu setup rumit, buka browser dan langsung bisa coding. Tapi kekuatannya terletak pada memahami paradigma berbeda (OOP, functional, asynchronous) dan ekosistemnya yang sangat luas."** — Roadmap ini membawa Anda dari variabel pertama hingga menguasai async/await dan manipulasi DOM, semua dalam satu project yang tumbuh bersama.

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: Membangun aplikasi **Task Manager (Aplikasi Manajemen Tugas)** dari sederhana (hanya CRUD lokal) → kompleks (localStorage, validasi, filter dinamis) → advanced (API integration, async handling).
- **Praktik Langsung di Browser**: Tidak ada setup Node.js di awal. Mulai dengan `<script>` di HTML, kemudian pahami modularitas.
- **Benang Merah Eksplisit**: Setiap konsep dihubungkan ke kasus nyata dalam Task Manager — kenapa kita perlu variabel, function, array, event listener, fetch API.
- **Fitur Khas JavaScript**: Closure, prototype chain, event loop, promise/async-await, DOM manipulation, destructuring, spread operator — semuanya praktik, bukan teori.

### Prasyarat Sebelum Memulai

Sebelum roadmap ini, pastikan sudah memahami:

- ├── Dasar HTML (tag, atribut, form)
- ├── Dasar CSS (selector, property umum, flexbox dasar)
- └── Punya browser modern (Chrome, Firefox, Safari) + text editor (VS Code)

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

**Project: Task Manager Web Application**

text

```
Level 1: "Hello World" 
  ↓ (enhance: tambah variabel, array)
Level 2: Dasar JavaScript (Variabel → Operator → Condition → Looping)
  ↓ (enhance: manipulasi DOM, tambah event)
Level 3: Function, Array, & Object (Membangun logika CRUD)
  ↓ (enhance: validasi, localStorage, filter)
Level 4: OOP & Scope (Refactor dengan class/object, scope mastery)
  ↓ (enhance: error handling, debugging)
Level 5: DOM Manipulation & Events (Interaktivitas penuh, UI update dinamis)
  ↓ (enhance: form validation, event bubbling, delegation)
Level 6: Asynchronous JavaScript (Fetch API, promise, async-await)
  ↓ (enhance: mock API, error handling async, loading state)
Level 7: Advanced Features (Destructuring, Spread, Higher-Order Function, Closure)
  ↓ (enhance: filter, sort, search dengan functional approach)
```

**Evolusi Task Manager:**

- **Week 1-2**: Aplikasi console → variabel tasks disimpan dalam array
- **Week 3-4**: UI HTML/CSS terbentuk → event listener di tombol → DOM update
- **Week 5-6**: CRUD penuh, localStorage persistence, form validation
- **Week 7-8**: Refactor ke class, scope/closure mastery, custom error handling
- **Week 9-10**: API call (mock server), async/await, loading indicator
- **Week 11-12**: Advanced JS (destructuring, filter/map/reduce, higher-order function)

---

## 🟢 LEVEL 1: Perkenalan & Setup (Minggu 1-2)

> **Tema**: _Memahami JavaScript bekerja di browser, setup environment, dan menulis kode pertama_

> **Benang Merah**: HTML adalah struktur → CSS adalah tampilan → **JavaScript adalah interaktivitas & logika**. Level ini fokus pada foundational: variabel, tipe data, operator, condition.

> **Output**: File `index.html` dengan console log menampilkan output JavaScript; paham perbedaan var/let/const dan tipe data.

---

### A. Pengenalan JavaScript & Environment Setup

> 💡 **Mengapa dimulai di sini?** Sebelum bisa membuat aplikasi, kita perlu tahu **di mana** JavaScript berjalan (browser), **bagaimana** cara menjalankannya (DevTools), dan **struktur dasar** apa yang dibutuhkan. Ini fondasi sebelum menulis satu baris kode pun.

Benang Merah Bagian A:  
HTML file → `<script>` tag → DevTools Console → First Output

1. `[[1. Apa itu JavaScript — Bahasa Pemrograman di Browser]]`
    
    - JavaScript vs bahasa pemrograman lain (Python, PHP)
    - JavaScript di browser (interpreter, event-driven)
    - Kasus penggunaan JavaScript (interaktivitas web, validasi form, animasi)
    - _Langkah konkret_: Buka browser Chrome → buka DevTools (F12) → tab Console → ketik `console.log("Hello JavaScript")`
2. `[[2. Setup Environment — Text Editor & Browser]]`
    
    - Install VS Code (atau editor pilihan)
    - Ekstensi berguna: Live Server, JavaScript (ES6) code snippets
    - File struktur: folder `js-learning/` → subfolder per level
    - _Langkah konkret_: Buat folder proyek, buat file `index.html`, buka dengan Live Server
3. `[[3. First Program — Hello World & Console Basics]]`
    
    - Menulis `<script>` inline di HTML
    - `console.log()`, `console.error()`, `console.warn()`
    - Membaca error di DevTools
    - _Langkah konkret_: Buat file `level1.html` → tambah `<script>console.log("Halo Dunia")</script>` → buka di browser → lihat output di console

---

### B. Variabel & Tipe Data Fundamental

> 💡 **Mengapa dimulai di sini?** Setelah tahu di mana kode berjalan, kita perlu **tempat menyimpan data** (variabel). Ini adalah blok bangunan logika program — tanpa variabel, data tidak bisa dimanipulasi.

Benang Merah Bagian B:  
Variabel (wadah) → Tipe Data (isi) → Operator (manipulasi)

4. `[[4. Variabel — let, const, var (Perbedaan & Best Practice)]]`
    
    - `var` (function-scoped, hoisting, sebaiknya hindari)
    - `let` (block-scoped, dapat di-reassign)
    - `const` (block-scoped, tidak dapat di-reassign, preference untuk modern JS)
    - Penamaan variabel (camelCase, deskriptif)
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let taskCount = 0;
        const APP_NAME = "Task Manager";
        console.log(APP_NAME, taskCount);
        ```
        
5. `[[5. Tipe Data — String, Number, Boolean, null, undefined]]`
    
    - Tipe primitif: string, number, boolean, null, undefined, symbol, bigint
    - `typeof` operator untuk cek tipe
    - String: single quote, double quote, backtick (template literal)
    - Number: integer, float, special values (Infinity, NaN)
    - Boolean: true/false
    - null vs undefined
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let title = "My Task Manager";
        let tasksDone = 5;
        let isCompleted = false;
        console.log(typeof title, typeof tasksDone, typeof isCompleted);
        ```
        
6. `[[6. Operator Aritmatika & Logika — Manipulasi Data]]`
    
    - Aritmatika: `+`, `-`, `*`, `/`, `%`, `**`
    - Perbandingan: `==`, `===`, `!=`, `!==`, `<`, `>`, `<=`, `>=`
    - Logika: `&&`, `||`, `!`
    - Assignment: `=`, `+=`, `-=`, `*=`
    - Ternary: `condition ? true : false`
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let x = 10, y = 5;
        console.log(x + y, x * y, x > y);
        console.log(x > 5 ? "Besar" : "Kecil");
        ```
        

---

### C. Condition & Decision Making

> 💡 **Mengapa dimulai di sini?** Program bukan hanya eksekusi linear. Kita perlu **membuat keputusan** — "jika kondisi A, lakukan X, jika tidak lakukan Y". Ini fundamental untuk logika kontrol alur.

Benang Merah Bagian C:  
Operator perbandingan → if/else → switch (kompleks) → Decision flow dalam aplikasi

7. `[[7. if, else, else if — Kontrol Alur Dasar]]`
    
    - Struktur `if (kondisi) { ... }`
    - `else if` untuk kondisi ganda
    - `else` untuk fallback
    - Common mistakes: `=` (assignment) vs `===` (perbandingan)
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let taskStatus = "pending";
        if (taskStatus === "done") {
          console.log("Tugas sudah selesai");
        } else if (taskStatus === "pending") {
          console.log("Tugas masih berjalan");
        } else {
          console.log("Status tidak dikenal");
        }
        ```
        
8. `[[8. switch & Ternary — Alternatif if-else]]`
    
    - `switch` untuk banyak kondisi (cleaner dibanding if/else beruntun)
    - `break` statement
    - `default` case
    - Ternary operator `? :`
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let priority = "high";
        switch (priority) {
          case "high": console.log("Prioritas Tinggi"); break;
          case "medium": console.log("Prioritas Sedang"); break;
          default: console.log("Prioritas Rendah");
        }
        ```
        

---

### D. Looping — Pengulangan Otomatis

> 💡 **Mengapa dimulai di sini?** Jika ada 100 tasks dan kita harus proses satu per satu dengan if/else, kode akan panjang & tidak scalable. Looping memungkinkan kita **mengulang aksi yang sama berkali-kali**. Ini membuka pintu ke array processing di level berikutnya.

Benang Merah Bagian D:  
Kondisi → Looping (otomasi kondisi berulang) → Loop control (`break`, `continue`)

9. `[[9. for Loop — Looping dengan Counter]]`
    
    - Struktur `for (inisialisasi; kondisi; update) { ... }`
    - `break` dan `continue`
    - Loop variable (i, j, k)
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        for (let i = 1; i <= 5; i++) {
          console.log("Task ke-" + i);
        }
        ```
        
10. `[[10. while & do-while — Looping dengan Kondisi]]`
    
    - `while (kondisi) { ... }`
    - `do { ... } while (kondisi)` — eksekusi minimal sekali
    - Kapan pakai while vs for
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let count = 0;
        while (count < 3) {
          console.log("Hitung: " + count);
          count++;
        }
        ```
        

---

### 🏗 Checkpoint Level 1

✅ **Checklist sebelum lanjut ke Level 2:**

**PEMAHAMAN:**

- ├── Bisa jelaskan perbedaan let, const, var
- ├── Bisa jelaskan 5+ tipe data primitif
- ├── Bisa menulis if/else dan switch statement
- └── Bisa menulis for dan while loop

**PROYEK:**

- ├── File `level1.html` + `<script>` yang menampilkan output console
- ├── Program simple: "Tampilkan angka 1-10 di console dengan loop"
- ├── Program: "Cek apakah angka genap atau ganjil dengan if/else"
- └── Program: "Input nilai 1-3, gunakan switch untuk output berbeda"

**KEBIASAAN (HABIT):**

- ├── Buka DevTools console setiap kode baru
- ├── Gunakan `console.log()` untuk debug
- └── Baca error message di console, jangan skip

**Commit:** `feat: level 1 - basic variables, operators, conditions, loops`

---

## 🔵 LEVEL 2: Array & String (Minggu 3-4)

> **Tema**: _Menyimpan & memanipulasi data koleksi; string sebagai data, bukan hanya output_

> **Benang Merah**: Looping level 1 hanya di atas angka murni. Sekarang kita tangani **kumpulan data** (array) dan **teks** (string). Ini dimulainya task manager nyata — menyimpan banyak task dalam satu struktur.

> **Output**: File `level2.html` dengan array of tasks; bisa menampilkan, menambah, dan mencari task di console.

---

### A. Array — Kumpulan Data dalam Satu Variabel

> 💡 **Mengapa dimulai di sini?** Aplikasi task manager butuh **menyimpan banyak task**. Jika `task1`, `task2`, `task3`, ... akan kacau. Array adalah struktur yang menampung data bertipe sama, indexed, mudah di-loop.

Benang Merah Bagian A:  
Variabel tunggal → Array (koleksi) → Array methods (manipulasi)

11. `[[11. Array Basics — Deklarasi, Index, Length]]`
    
    - Deklarasi: `let tasks = []`, `let tasks = ["task1", "task2"]`
    - Akses elemen: `tasks[0]`, `tasks[tasks.length - 1]`
    - Index mulai dari 0
    - `length` property
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let tasks = ["Belajar JS", "Bikin UI", "Testing"];
        console.log(tasks[0]); // "Belajar JS"
        console.log(tasks.length); // 3
        console.log(tasks[tasks.length - 1]); // "Testing"
        ```
        
12. `[[12. Array Methods — push, pop, shift, unshift, indexOf]]`
    
    - `push(element)` — tambah ke akhir
    - `pop()` — hapus dari akhir
    - `shift()` — hapus dari awal
    - `unshift(element)` — tambah ke awal
    - `indexOf(element)` — cari index elemen
    - `includes(element)` — cek ada/tidak
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let tasks = ["Belajar", "Praktik"];
        tasks.push("Deploy"); // ["Belajar", "Praktik", "Deploy"]
        tasks.pop(); // ["Belajar", "Praktik"]
        console.log(tasks.indexOf("Belajar")); // 0
        ```
        
13. `[[13. Looping Array dengan for & forEach]]`
    
    - Traditional `for` loop atas array
    - `forEach(callback)` — lebih clean, functional approach
    - Callback function di dalam loop
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let tasks = ["Task A", "Task B"];
        // for loop
        for (let i = 0; i < tasks.length; i++) {
          console.log(i, tasks[i]);
        }
        // forEach
        tasks.forEach((task, index) => {
          console.log(index, task);
        });
        ```
        

---

### B. String Manipulation — Mengelola Teks

> 💡 **Mengapa dimulai di sini?** Data task adalah string (judul, deskripsi). Kita perlu **memotong, menggabung, format** string. Ini essential untuk UI (menampilkan task), validasi (cek panjang input), dan processing data.

Benang Merah Bagian B:  
String sebagai tipe data → String methods → Template literal (modern approach)

14. `[[14. String Methods — slice, substring, split, charAt, toUpperCase]]`
    
    - `slice(start, end)` — potong string
    - `substring(start, end)` — alternatif slice (jarang)
    - `split(delimiter)` — pecah jadi array
    - `charAt(index)` — ambil karakter di index
    - `toUpperCase()`, `toLowerCase()`
    - `trim()` — hilangkan spasi awal/akhir
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let title = "  Belajar JavaScript  ";
        console.log(title.trim()); // "Belajar JavaScript"
        console.log(title.toUpperCase()); // "  BELAJAR JAVASCRIPT  "
        console.log(title.slice(2, 8)); // "Belaj"
        let words = "Hello World".split(" "); // ["Hello", "World"]
        ```
        
15. `[[15. String Methods — replace, includes, startsWith, endsWith]]`
    
    - `replace(search, replace)` — ganti substring pertama
    - `replaceAll(search, replace)` — ganti semua (modern)
    - `includes(substring)` — cek ada/tidak
    - `startsWith(str)`, `endsWith(str)`
    - `indexOf(str)` — posisi substring
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let desc = "Belajar JavaScript di JavaScript";
        console.log(desc.replace("JavaScript", "JS")); // "Belajar JS di JavaScript"
        console.log(desc.includes("Belajar")); // true
        ```
        
16. `[[16. Template Literal — String dengan Interpolasi]]`
    
    - Backtick `` ` `` vs quote biasa
    - `${variabel}` untuk interpolasi
    - Multi-line string
    - Lebih readable dibanding concatenation `+`
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let name = "User", count = 5;
        console.log(`Hello ${name}, Anda punya ${count} tasks`);
        // vs concatenation:
        console.log("Hello " + name + ", Anda punya " + count + " tasks");
        ```
        

---

### C. Array Advanced — map, filter, find

> 💡 **Mengapa dimulai di sini?** `forEach` mengulang tapi tidak return data baru. `map`, `filter`, `find` adalah **functional programming methods** yang return array/value baru — crucial untuk task manager (filter completed tasks, map ke display format).

Benang Merah Bagian C:  
forEach (loop saja) → map (transform) → filter (select) → find (first match)

17. `[[17. Array.map() — Transform Setiap Elemen]]`
    
    - Syntax: `array.map((element, index) => newElement)`
    - Return array baru dengan transformasi
    - Tidak mengubah array original
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let tasks = ["belajar", "praktik", "testing"];
        let tasksTitlecase = tasks.map(task => task.charAt(0).toUpperCase() + task.slice(1));
        console.log(tasksTitlecase); // ["Belajar", "Praktik", "Testing"]
        ```
        
18. `[[18. Array.filter() — Memilih Elemen Berdasarkan Kondisi]]`
    
    - Syntax: `array.filter((element) => kondisi)`
    - Return array baru dengan elemen yang memenuhi kondisi
    - Common: filter completed, filter by priority
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let taskScores = [45, 87, 32, 95, 60];
        let passing = taskScores.filter(score => score >= 60);
        console.log(passing); // [87, 95, 60]
        ```
        
19. `[[19. Array.find() & findIndex() — Cari Elemen Pertama]]`
    
    - `find()` return elemen pertama yang match
    - `findIndex()` return index
    - Berbeda filter — hanya return satu elemen
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let tasks = [{id: 1, title: "Task A"}, {id: 2, title: "Task B"}];
        let task = tasks.find(t => t.id === 2);
        console.log(task); // {id: 2, title: "Task B"}
        ```
        

---

### D. Object — Struktur Data Kunci-Nilai

> 💡 **Mengapa dimulai di sini?** Task bukan hanya string judul — butuh `{id, title, completed, priority, dueDate}`. Object adalah struktur kunci-nilai yang hold banyak property. Ini fondasi sebelum ke Class di level 4.

Benang Merah Bagian D:  
Array of strings → Array of objects (struktur data lebih complex) → Access property

20. `[[20. Object Literal — Deklarasi & Akses Property]]`
    
    - Deklarasi: `let obj = { key: value }`
    - Dot notation: `obj.key`
    - Bracket notation: `obj["key"]`
    - Property bisa string, number, boolean, object, array
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let task = {
          id: 1,
          title: "Belajar JS",
          completed: false,
          priority: "high"
        };
        console.log(task.title); // "Belajar JS"
        console.log(task["completed"]); // false
        ```
        
21. `[[21. Mengubah & Menambah Property]]`
    
    - Assignment: `obj.key = newValue`
    - Menambah property baru: `obj.newKey = value`
    - Delete: `delete obj.key`
    - Check property: `obj.hasOwnProperty("key")`, `"key" in obj`
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let task = {id: 1, title: "Belajar"};
        task.completed = true; // tambah
        task.title = "Belajar JS"; // ubah
        console.log(task);
        ```
        

---

### 🏗 Checkpoint Level 2

✅ **Checklist sebelum lanjut ke Level 3:**

**PEMAHAMAN:**

- ├── Bisa jelaskan array index, length, dan mutating methods
- ├── Bisa bedakan map vs filter vs forEach
- ├── Bisa manipulasi string (slice, split, replace, includes)
- └── Bisa membuat & akses object properties

**PROYEK:**

- ├── File `level2.html` dengan array of task objects:
    
    JavaScript
    
    ```
    let tasks = [
      {id: 1, title: "Belajar JS", completed: false},
      {id: 2, title: "Bikin UI", completed: true},
      {id: 3, title: "Testing", completed: false}
    ];
    ```
    
- ├── Tampilkan semua tasks dengan forEach
- ├── Filter tasks yang belum completed (use filter)
- ├── Map tasks ke format: "Task #1: Belajar JS (not done)"
- └── Cari task dengan title tertentu (use find)

**KEBIASAAN (HABIT):**

- ├── Console.log setelah setiap manipulasi array
- ├── Gunakan template literal, bukan concatenation
- └── Verify array methods tidak mutate original (kecuali push/pop)

**Commit:** `feat: level 2 - arrays, strings, objects, map/filter/find`

---

## 🔵 LEVEL 3: Function & Basic CRUD Logic (Minggu 5-6)

> **Tema**: _Menulis function reusable; membuat logika menambah, membaca, mengubah, menghapus task_

> **Benang Merah**: Kode di level 1-2 adalah eksperimen console. Sekarang kita **organize code ke function** yang handle task manager logic. Function adalah abstraksi — kita bisa panggil berkali-kali tanpa repeat code.

> **Output**: File `level3.html` + `level3.js` dengan function CRUD yang bekerja di console; localStorage untuk persistence.

---

### A. Function Basics — Deklarasi & Pemanggilan

> 💡 **Mengapa dimulai di sini?** Kode level 1-2 bersifat procedural — eksekusi linear. Aplikasi real butuh **kode terstruktur** yang bisa di-reuse. Function adalah building block. CRUD logic butuh 4 function terpisah: create, read, update, delete.

Benang Merah Bagian A:  
Kode procedural → Function (abstraksi) → Parameter & return (I/O function)

22. `[[22. Function Declaration — Sintaks & Pemanggilan]]`
    
    - Syntax: `function namaFunction() { ... }`
    - Memanggil: `namaFunction()`
    - Function bisa berada di mana saja (hoisting)
    - Naming: camelCase, deskriptif
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        function greetUser(name) {
          console.log("Hello " + name);
        }
        greetUser("Alice"); // "Hello Alice"
        ```
        
23. `[[23. Parameter & Return Value — Input & Output]]`
    
    - Parameter: input ke function
    - Return: output dari function
    - Function bisa return apa saja (string, number, object, array, function)
    - Early return
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        function addTask(taskList, newTask) {
          taskList.push(newTask);
          return taskList;
        }
        let tasks = [];
        tasks = addTask(tasks, "Belajar");
        console.log(tasks); // ["Belajar"]
        ```
        
24. `[[24. Arrow Function — Sintaks Modern ES6]]`
    
    - Syntax: `const fn = (param) => returnValue`
    - Shorter, terutama untuk callback
    - Arrow function tidak punya `this` sendiri (binding beda dengan regular function)
    - Single line vs multi-line
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        const greet = (name) => `Hello ${name}`;
        const add = (a, b) => a + b;
        const printTwice = (msg) => {
          console.log(msg);
          console.log(msg);
        };
        ```
        

---

### B. Scope & Variable Lifetime — Kuis Mengenai Jangkauan Variabel

> 💡 **Mengapa dimulai di sini?** Jika variabel di function bisa diakses di luar, atau sebaliknya, akan terjadi bug. Scope adalah **jangkauan variabel** — apa bisa diakses dari mana. Global scope, function scope, block scope fundamental untuk code yang maintainable.

Benang Merah Bagian B:  
Variable deklarasi (level 1) → Scope & visibility → Closure (advanced scope concept)

25. `[[25. Global vs Local Scope]]`
    
    - Global scope: variabel di luar function, bisa diakses mana saja
    - Local/Function scope: variabel di dalam function, hanya bisa diakses dalam function
    - Block scope: `if`, `for` block (let, const)
    - Shadowing: deklarasi variabel sama nama di scope berbeda
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let globalVar = "global";
        function test() {
          let localVar = "local";
          console.log(globalVar); // "global" (akses global)
          console.log(localVar); // "local"
        }
        console.log(globalVar); // "global"
        // console.log(localVar); // ERROR — undefined
        ```
        
26. `[[26. Closure — Function Nesting & Variable Access]]`
    
    - Closure terjadi saat function nested dapat akses ke outer function's variable
    - Khas JavaScript — function bisa "remember" environment saat dibuat
    - Pattern: factory function, private variable
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        function makeCounter() {
          let count = 0; // private variable
          return function() {
            count++;
            return count;
          };
        }
        let counter = makeCounter();
        console.log(counter()); // 1
        console.log(counter()); // 2
        // count hanya bisa diakses via returned function
        ```
        

---

### C. CRUD Operations — Logika Task Manager Inti

> 💡 **Mengapa dimulai di sini?** Task Manager butuh 4 operasi fundamental: Create (tambah task), Read (tampilkan), Update (ubah), Delete (hapus). Ini adalah core logic. Setelah paham CRUD, logic kompleks (filter, search) tinggal build di atasnya.

Benang Merah Bagian C:  
Function basics → Parameter/return → CRUD functions (4 function terpisah)

27. `[[27. CREATE — Menambah Task Baru]]`
    
    - Function untuk tambah task ke array/database
    - Generate ID unik (bisa pakai timestamp, increment counter)
    - Validasi input (tidak boleh kosong)
    - Return task yang baru atau status sukses
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let tasks = [];
        let nextId = 1;
        
        function createTask(title) {
          if (!title || title.trim() === "") {
            return null; // validasi
          }
          let newTask = {
            id: nextId++,
            title: title,
            completed: false,
            createdAt: new Date()
          };
          tasks.push(newTask);
          return newTask;
        }
        ```
        
28. `[[28. READ — Membaca dan Menampilkan Task]]`
    
    - Function untuk get semua tasks atau task spesifik by ID
    - Read semua: return tasks array
    - Read by ID: return task object atau null
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        function getAllTasks() {
          return tasks;
        }
        
        function getTaskById(id) {
          return tasks.find(t => t.id === id);
        }
        ```
        
29. `[[29. UPDATE — Mengubah Task]]`
    
    - Function untuk update task property (title, completed status)
    - Find task by ID, ubah property, return updated task atau status
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        function updateTask(id, updates) {
          let task = tasks.find(t => t.id === id);
          if (!task) return null;
          Object.assign(task, updates); // merge updates ke task
          return task;
        }
        // usage: updateTask(1, {completed: true})
        ```
        
30. `[[30. DELETE — Menghapus Task]]`
    
    - Function untuk hapus task dari array
    - Find task, remove dari array (pakai filter atau splice)
    - Return deleted task atau status sukses
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        function deleteTask(id) {
          let index = tasks.findIndex(t => t.id === id);
          if (index === -1) return null;
          let deleted = tasks.splice(index, 1);
          return deleted[0];
        }
        ```
        

---

### D. localStorage — Persistence Data

> 💡 **Mengapa dimulai di sini?** Aplikasi baru berjalan, data di memory. Refresh halaman, semuanya hilang. Browser punya `localStorage` untuk simpan data persistent di client. Ini bagian essential dari task manager — user data harus tersimpan.

Benang Merah Bagian D:  
CRUD logic (di memory) → localStorage (persistence) → Load saat app start

31. `[[31. localStorage API — setItem, getItem, removeItem]]`
    
    - `localStorage.setItem(key, value)` — simpan
    - `localStorage.getItem(key)` — ambil (return string)
    - `localStorage.removeItem(key)` — hapus
    - `localStorage.clear()` — hapus semua
    - localStorage store string saja, pakai JSON.stringify/parse untuk object
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let user = {name: "Alice", age: 25};
        localStorage.setItem("user", JSON.stringify(user));
        let retrieved = JSON.parse(localStorage.getItem("user"));
        console.log(retrieved); // {name: "Alice", age: 25}
        ```
        
32. `[[32. Serialize & Deserialize — JSON.stringify & JSON.parse]]`
    
    - `JSON.stringify(obj)` — convert object ke JSON string
    - `JSON.parse(string)` — convert JSON string ke object
    - Essential untuk simpan array/object di localStorage
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let tasks = [{id: 1, title: "Task 1"}, {id: 2, title: "Task 2"}];
        localStorage.setItem("tasks", JSON.stringify(tasks));
        // later:
        let loadedTasks = JSON.parse(localStorage.getItem("tasks"));
        ```
        
33. `[[33. Integrating localStorage dengan CRUD]]`
    
    - Tiap CREATE/UPDATE/DELETE juga update localStorage
    - Load tasks dari localStorage saat app start
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        function saveTasks() {
          localStorage.setItem("tasks", JSON.stringify(tasks));
        }
        function loadTasks() {
          let stored = localStorage.getItem("tasks");
          tasks = stored ? JSON.parse(stored) : [];
        }
        // di function createTask:
        function createTask(title) {
          // ... create logic ...
          saveTasks(); // persist
          return newTask;
        }
        ```
        

---

### 🏗 Checkpoint Level 3

✅ **Checklist sebelum lanjut ke Level 4:**

**PEMAHAMAN:**

- ├── Bisa jelaskan function parameter, return, arrow function
- ├── Bisa jelaskan scope & closure
- ├── Bisa jelaskan CRUD operations (create, read, update, delete)
- └── Bisa jelaskan localStorage dan JSON stringify/parse

**PROYEK:**

- ├── File `level3.js` dengan functions:
    - `createTask(title)`
    - `getAllTasks()`
    - `getTaskById(id)`
    - `updateTask(id, updates)`
    - `deleteTask(id)`
    - `saveTasks()` & `loadTasks()`
- ├── Test CRUD di console: buat, tampilkan, ubah, hapus task
- ├── Persist ke localStorage, refresh page, data masih ada
- └── Validasi: title tidak boleh kosong

**KEBIASAAN (HABIT):**

- ├── Test tiap function di console
- ├── Verify localStorage dengan DevTools → Application tab
- └── Use meaningful parameter names

**Commit:** `feat: level 3 - functions, CRUD logic, localStorage persistence`

---

## 🔵 LEVEL 4: Object-Oriented Programming & Class (Minggu 7-8)

> **Tema**: _Refactor CRUD logic ke class; encapsulation, inheritance, polymorphism_

> **Benang Merah**: Level 3 pakai function standalone & global `tasks` array. Sekarang kita **organize ke class** — data (properties) dan behavior (methods) jadi satu. Ini adalah OOP dalam JavaScript, dengan ES6 `class` syntax.

> **Output**: File `level4.js` dengan `TaskManager` class; create object instance; paham `this`, constructor, methods.

---

### A. Class Basics — ES6 Class Syntax

> 💡 **Mengapa dimulai di sini?** Function standalone + global variable bisa membingungkan saat proyek besar. Class mengumpulkan **data terkait** (tasks array) dan **behavior** (CRUD methods) jadi satu blueprint. Ini membuat code lebih organized dan reusable.

Benang Merah Bagian A:  
Standalone functions + global var → Class (bundled data & methods) → Instance

34. `[[34. Class Declaration & Constructor]]`
    
    - Syntax: `class ClassName { constructor(params) { ... } }`
    - Constructor dipanggil saat buat instance dengan `new`
    - Inisialisasi properties di constructor
    - `this` refer ke instance saat ini
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        class TaskManager {
          constructor() {
            this.tasks = [];
            this.nextId = 1;
          }
        }
        let manager = new TaskManager();
        console.log(manager.tasks); // []
        ```
        
35. `[[35. Methods — Function dalam Class]]`
    
    - Method adalah function dalam class
    - Akses properties dengan `this`
    - Method otomatis ke dalam prototype (shared, efficient)
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        class TaskManager {
          constructor() {
            this.tasks = [];
            this.nextId = 1;
          }
          
          addTask(title) {
            let task = {id: this.nextId++, title, completed: false};
            this.tasks.push(task);
            return task;
          }
          
          getTasks() {
            return this.tasks;
          }
        }
        ```
        
36. `[[36. Encapsulation — Private vs Public]]`
    
    - JavaScript: public (akses langsung), private (convention `_` atau `#`)
    - `#` private field (ES2022) — tidak bisa diakses dari luar
    - Convention `_` — signal ke developer "jangan akses dari luar"
    - Getter/setter untuk control akses
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        class TaskManager {
          #tasks = []; // private field
          
          getTasks() {
            return this.#tasks; // akses di dalam class OK
          }
        }
        let manager = new TaskManager();
        // manager.#tasks // ERROR
        ```
        

---

### B. Inheritance & Polymorphism

> 💡 **Mengapa dimulai di sini?** Saat aplikasi grow, kita mungkin ada `Task`, `SubTask`, `ImportantTask` dengan properties & methods berbeda. Inheritance memungkinkan **share code** antar class. Polymorphism memungkinkan **method berbeda behavior** tapi interface sama.

Benang Merah Bagian B:  
Base class (umum) → Child class (khusus, inherit dari base) → Override method

37. `[[37. Inheritance — extends & super]]`
    
    - `class Child extends Parent { ... }`
    - Child inherit properties & methods dari Parent
    - `super` memanggil parent constructor & method
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        class Task {
          constructor(title, priority = "normal") {
            this.title = title;
            this.priority = priority;
            this.completed = false;
          }
        }
        
        class UrgentTask extends Task {
          constructor(title) {
            super(title, "high"); // call parent constructor
            this.dueDate = new Date(Date.now() + 24*60*60*1000); // tomorrow
          }
        }
        ```
        
38. `[[38. Polymorphism — Override Method]]`
    
    - Child class bisa override (ubah behavior) method parent
    - Sama nama method, behavior berbeda
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        class Task {
          display() {
            return `${this.title} (${this.priority})`;
          }
        }
        
        class UrgentTask extends Task {
          display() {
            return `🔴 ${this.title} (URGENT)`;
          }
        }
        ```
        

---

### C. `this` Binding & Context

> 💡 **Mengapa dimulai di sini?** `this` di JavaScript bingung — bisa refer ke object, window, undefined tergantung konteks. Dalam class, method dipanggil from event listener atau callback, `this` bisa ilang. Paham `this` binding essential untuk debug.

Benang Merah Bagian C:  
Method akses `this` → `this` binding di callback/event → Arrow function solution

39. `[[39. this dalam Method — Regular Function vs Arrow]]`
    
    - Regular method: `this` adalah object yang panggil method
    - Arrow method: `this` bind dari scope lexical (di mana didefinisikan)
    - Problem: event listener panggil method, `this` lost
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        class TaskManager {
          constructor(name) {
            this.name = name;
          }
          
          displayName() {
            console.log(this.name);
          }
        }
        
        let manager = new TaskManager("My Manager");
        manager.displayName(); // "My Manager" — `this` adalah manager
        
        let func = manager.displayName;
        func(); // undefined — `this` lost (atau window)
        ```
        
40. `[[40. Binding Solution — bind(), arrow function method]]`
    
    - `method.bind(thisContext)` — lock `this`
    - Arrow method: `methodName = () => { ... }` — `this` lexical
    - Bind di constructor atau pakai arrow method
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        class Button {
          constructor(label) {
            this.label = label;
            // option 1: arrow method
            this.handleClick = () => console.log(this.label);
            // option 2: bind di constructor
            // this.handleClick = this.handleClick.bind(this);
          }
          
          // regular method (bind manual jika pakai event)
          handleClickRegular() {
            console.log(this.label);
          }
        }
        ```
        

---

### D. Static Methods & Properties

> 💡 **Mengapa dimulai di sini?** Kadang kita butuh method/property yang relate ke class, bukan instance. Contoh: utility function di TaskManager class, atau counter "berapa banyak task pernah dibuat". Static member shared antar instance.

Benang Merah Bagian D:  
Instance method (per object) → Static method (class-level, shared)

41. `[[41. Static Methods & Properties]]`
    - `static` keyword untuk method/property
    - Akses via class, bukan instance: `ClassName.staticMethod()`
    - Tidak ada `this` (atau `this` refer ke class)
    - Use case: utility, counter, config
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        class TaskManager {
          static totalCreated = 0;
          
          constructor() {
            this.tasks = [];
            TaskManager.totalCreated++;
          }
          
          static printStats() {
            console.log(`Total managers created: ${TaskManager.totalCreated}`);
          }
        }
        
        new TaskManager();
        new TaskManager();
        TaskManager.printStats(); // "Total managers created: 2"
        ```
        

---

### E. Refactor Level 3 Code ke Class

> 💡 **Mengapa dimulai di sini?** Teori class oke, tapi praktik **refactor CRUD logic dari level 3 ke class** itu yang penting. Ini adalah real-world refactoring — same functionality, better code structure.

Benang Merah Bagian E:  
Level 3 CRUD functions → Level 4 CRUD methods dalam TaskManager class

42. `[[42. CRUD Methods dalam Class — addTask, getTasks, updateTask, deleteTask]]`
    - Semua CRUD methods jadi method class
    - `tasks` array jadi property private
    - localStorage integration tetap (saveTasks, loadTasks jadi private methods)
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        class TaskManager {
          #tasks = [];
          #nextId = 1;
          
          addTask(title) {
            if (!title.trim()) return null;
            let task = {id: this.#nextId++, title, completed: false};
            this.#tasks.push(task);
            this.#saveTasks();
            return task;
          }
          
          getTasks() {
            return [...this.#tasks]; // return copy, bukan reference
          }
          
          updateTask(id, updates) {
            let task = this.#tasks.find(t => t.id === id);
            if (!task) return null;
            Object.assign(task, updates);
            this.#saveTasks();
            return task;
          }
          
          deleteTask(id) {
            let index = this.#tasks.findIndex(t => t.id === id);
            if (index === -1) return null;
            let deleted = this.#tasks.splice(index, 1);
            this.#saveTasks();
            return deleted[0];
          }
          
          #saveTasks() {
            localStorage.setItem("tasks", JSON.stringify(this.#tasks));
          }
          
          #loadTasks() {
            let stored = localStorage.getItem("tasks");
            this.#tasks = stored ? JSON.parse(stored) : [];
          }
        }
        ```
        

---

### 🏗 Checkpoint Level 4

✅ **Checklist sebelum lanjut ke Level 5:**

**PEMAHAMAN:**

- ├── Bisa jelaskan class, constructor, methods
- ├── Bisa jelaskan `this` binding & arrow vs regular function
- ├── Bisa jelaskan inheritance (extends, super) & polymorphism
- └── Bisa jelaskan private/public, static members

**PROYEK:**

- ├── File `level4.js` dengan class `TaskManager`:
    - Constructor: inisialisasi tasks array, load dari localStorage
    - Methods: addTask, getTasks, updateTask, deleteTask
    - Private methods: #saveTasks, #loadTasks
    - Private properties: #tasks, #nextId
- ├── Create instance: `let manager = new TaskManager()`
- ├── Test semua CRUD methods
- ├── (Optional) Buat subclass `UrgentTaskManager` yang override addTask (set priority ke high)
- └── Verify persistence: create/update/delete, refresh, data ada

**KEBIASAAN (HABIT):**

- ├── Use `this.` untuk access instance properties
- ├── Use `#` untuk private members (ES2022)
- ├── Test `this` context di console
- └── Use class untuk organize code, bukan function standalone

**Commit:** `refactor: level 4 - refactor to class-based architecture, OOP principles`

---

## 🔵 LEVEL 5: DOM Manipulation & Events (Minggu 9-10)

> **Tema**: _Bikin UI interaktif; handle user input; update DOM berdasarkan data_

> **Benang Merah**: Level 1-4 semua di console. Sekarang **connect ke HTML** — user lihat task di halaman, klik tombol, form input. Event listener menangkap interaksi user, update DOM sesuai state di TaskManager class.

> **Output**: File `index.html` dengan UI lengkap; `level5.js` dengan event listeners; aplikasi fully functional.

---

### A. DOM Basics — Select & Traverse

> 💡 **Mengapa dimulai di sini?** Sebelum update DOM, kita perlu **select elements** dari HTML. DOM adalah representasi JavaScript dari HTML tree. Select, traverse, modify adalah tiga operasi inti.

Benang Merah Bagian A:  
HTML elements → DOM API → Select elements → Modify

43. `[[43. DOM API — getElementById, querySelector, querySelectorAll]]`
    
    - `document.getElementById("id")` — by ID
    - `document.querySelector("selector")` — CSS selector, return first
    - `document.querySelectorAll("selector")` — CSS selector, return all (NodeList)
    - Modern approach: querySelector lebih flexible
    - _Langkah konkret_:
        
        HTML
        
        ```
        <input id="taskInput" />
        <button class="btn">Add Task</button>
        <div id="taskList"></div>
        ```
        
        JavaScript
        
        ```
        let input = document.getElementById("taskInput");
        let button = document.querySelector(".btn");
        let tasks = document.querySelectorAll(".task-item");
        ```
        
44. `[[44. Mengubah Content — textContent, innerHTML, innerText]]`
    
    - `element.textContent` — set/get text (safe, no HTML)
    - `element.innerHTML` — set/get HTML (risky, XSS vulnerability)
    - `element.innerText` — like textContent, tapi respect CSS visibility
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let heading = document.querySelector("h1");
        heading.textContent = "Task Manager v1.0";
        
        let taskDiv = document.getElementById("taskList");
        taskDiv.innerHTML = "<p>No tasks yet</p>";
        ```
        
45. `[[45. Mengubah Attributes — setAttribute, getAttribute, classList]]`
    
    - `element.setAttribute("attr", "value")` — set attribute
    - `element.getAttribute("attr")` — get attribute
    - `element.classList.add("class")` — tambah class
    - `element.classList.remove("class")` — hapus class
    - `element.classList.toggle("class")` — toggle
    - `element.classList.contains("class")` — check
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let task = document.querySelector(".task");
        task.setAttribute("data-id", "123");
        task.classList.add("completed");
        console.log(task.getAttribute("data-id")); // "123"
        ```
        

---

### B. Event Handling — Respond ke User Interaction

> 💡 **Mengapa dimulai di sini?** Aplikasi adalah interactive — user klik, type, submit. Event adalah signal dari browser saat user melakukan aksi. Event listener menangkap dan trigger handler function. Ini adalah jembatan antara UI dan logic.

Benang Merah Bagian B:  
User action (click, input, submit) → Event → Event listener → Handler function

46. `[[46. addEventListener — Attach Event Listener]]`
    
    - `element.addEventListener("event", callback)`
    - Common events: "click", "submit", "input", "change", "keypress", "mouseover"
    - Callback function dipanggil saat event terjadi
    - Event object di-pass ke callback: `function(event) { ... }`
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let button = document.querySelector(".add-btn");
        button.addEventListener("click", function(event) {
          console.log("Button clicked!");
          event.preventDefault(); // untuk form
        });
        
        // arrow function
        button.addEventListener("click", () => {
          console.log("Button clicked!");
        });
        ```
        
47. `[[47. Event Object — target, type, preventDefault, stopPropagation]]`
    
    - `event.target` — element yang trigger event
    - `event.type` — tipe event ("click", "input", dll)
    - `event.preventDefault()` — prevent default behavior (form submit, link navigate)
    - `event.stopPropagation()` — stop event bubbling ke parent
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let form = document.querySelector("form");
        form.addEventListener("submit", (event) => {
          event.preventDefault(); // jangan submit ke server
          console.log("Form submitted!");
        });
        
        let input = document.querySelector("input");
        input.addEventListener("input", (event) => {
          console.log("Input value:", event.target.value);
        });
        ```
        
48. `[[48. Event Delegation — Handle Multiple Elements]]`
    
    - Listener di parent, handle events dari children
    - Efficient untuk list yang banyak elements
    - Check `event.target` untuk ketahu element mana
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let taskList = document.getElementById("taskList");
        taskList.addEventListener("click", (event) => {
          let taskElement = event.target.closest(".task-item");
          if (taskElement) {
            let taskId = taskElement.getAttribute("data-id");
            console.log("Clicked task:", taskId);
          }
        });
        ```
        

---

### C. Building Task Manager UI — Connect Class ke DOM

> 💡 **Mengapa dimulai di sini?** TaskManager class jalan di memory, tapi user tidak lihat apa-apa. Sekarang **render tasks di DOM** — tampilin list, handle klik tombol add/delete/complete, update UI saat data berubah.

Benang Merah Bagian C:  
DOM structure → Event listeners → Call TaskManager methods → Update DOM → Re-render

49. `[[49. HTML Structure untuk Task Manager]]`
    
    - `<input id="taskInput">` untuk input task
    - `<button id="addBtn">Add Task</button>`
    - `<ul id="taskList">` untuk list tasks
    - Tiap task item: `<li class="task-item" data-id="1">`
    - Button delete/complete di setiap item
    - _Langkah konkret_:
        
        HTML
        
        ```
        <div class="container">
          <h1>Task Manager</h1>
          <div class="input-group">
            <input id="taskInput" placeholder="Enter task...">
            <button id="addBtn">Add Task</button>
          </div>
          <ul id="taskList"></ul>
        </div>
        ```
        
50. `[[50. Render Tasks — Populate DOM dengan Data dari TaskManager]]`
    
    - Function untuk render semua tasks dari manager.getTasks()
    - Loop tasks, buat `<li>` element untuk setiap task, append ke `<ul>`
    - Pakai template literal untuk build HTML string
    - Clear previous HTML sebelum render (avoid duplicates)
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        function renderTasks() {
          let manager = window.manager; // reference global
          let tasks = manager.getTasks();
          let taskList = document.getElementById("taskList");
          
          taskList.innerHTML = ""; // clear
          
          tasks.forEach(task => {
            let li = document.createElement("li");
            li.className = `task-item ${task.completed ? "completed" : ""}`;
            li.setAttribute("data-id", task.id);
            
            li.innerHTML = `
              <span>${task.title}</span>
              <button class="btn-complete">Complete</button>
              <button class="btn-delete">Delete</button>
            `;
            
            taskList.appendChild(li);
          });
        }
        ```
        
51. `[[51. Handle Add Task — Input → Manager → Render]]`
    
    - Listen ke "click" event di Add button
    - Get value dari input
    - Call manager.addTask(value)
    - Clear input
    - Re-render tasks
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let addBtn = document.getElementById("addBtn");
        let taskInput = document.getElementById("taskInput");
        
        addBtn.addEventListener("click", () => {
          let title = taskInput.value;
          if (!title.trim()) return;
          
          window.manager.addTask(title);
          taskInput.value = "";
          renderTasks();
        });
        ```
        
52. `[[52. Handle Delete & Complete — Event Delegation + Manager]]`
    
    - Listen ke click di taskList (event delegation)
    - Check apakah clicked element adalah .btn-delete atau .btn-complete
    - Get task ID dari data-id attribute
    - Call manager.deleteTask(id) atau manager.updateTask(id, {completed: true})
    - Re-render
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let taskList = document.getElementById("taskList");
        
        taskList.addEventListener("click", (event) => {
          let taskElement = event.target.closest(".task-item");
          if (!taskElement) return;
          
          let taskId = parseInt(taskElement.getAttribute("data-id"));
          
          if (event.target.classList.contains("btn-delete")) {
            window.manager.deleteTask(taskId);
            renderTasks();
          } else if (event.target.classList.contains("btn-complete")) {
            let task = window.manager.getTasks().find(t => t.id === taskId);
            window.manager.updateTask(taskId, {completed: !task.completed});
            renderTasks();
          }
        });
        ```
        

---

### D. Form Validation & User Feedback

> 💡 **Mengapa dimulai di sini?** Aplikasi butuh validasi input & feedback ke user. Jangan silent fail — kasih error message jika input invalid. Visual feedback (highlight, animation) improve UX.

Benang Merah Bagian D:  
Raw input → Validation → Error message → User feedback

53. `[[53. Form Validation — Client-side]]`
    
    - Cek jika input kosong, terlalu panjang, format salah
    - Show error message di DOM
    - Disable submit button jika invalid
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        function validateTaskInput(title) {
          if (!title || title.trim().length === 0) {
            return "Task cannot be empty";
          }
          if (title.trim().length > 100) {
            return "Task too long (max 100 characters)";
          }
          return null; // valid
        }
        
        addBtn.addEventListener("click", () => {
          let title = taskInput.value;
          let error = validateTaskInput(title);
          
          if (error) {
            showError(error);
            return;
          }
          
          window.manager.addTask(title);
          taskInput.value = "";
          clearError();
          renderTasks();
        });
        ```
        
54. `[[54. Show dan Clear Error Messages — User Feedback]]`
    
    - Function showError(message) — tampilkan error di DOM
    - Function clearError() — hilangkan error message
    - Add/remove class "error" untuk styling
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        function showError(message) {
          let errorDiv = document.getElementById("error") || createErrorDiv();
          errorDiv.textContent = message;
          errorDiv.style.display = "block";
          taskInput.classList.add("error");
        }
        
        function clearError() {
          let errorDiv = document.getElementById("error");
          if (errorDiv) errorDiv.style.display = "none";
          taskInput.classList.remove("error");
        }
        
        function createErrorDiv() {
          let div = document.createElement("div");
          div.id = "error";
          div.className = "error-message";
          document.querySelector(".input-group").appendChild(div);
          return div;
        }
        ```
        

---

### 🏗 Checkpoint Level 5

✅ **Checklist sebelum lanjut ke Level 6:**

**PEMAHAMAN:**

- ├── Bisa jelaskan DOM selection (querySelector, getElementById)
- ├── Bisa jelaskan event listener & event object
- ├── Bisa jelaskan event delegation
- └── Bisa jelaskan form validation & user feedback

**PROYEK:**

- ├── File `index.html` dengan complete UI (input, button, task list)
- ├── File `level5.js` dengan:
    - TaskManager class (dari level 4)
    - renderTasks() — populate DOM dari manager data
    - Event listeners: addBtn click, taskList delegation (delete/complete)
    - validateTaskInput() & error display
- ├── Aplikasi fully functional:
    - Add new task via input + button
    - Mark task as complete (visual change + checkbox)
    - Delete task
    - Persist data di localStorage
    - Validate input, show error if needed
- └── CSS styling: layout, completed task visual, button hover, error styling

**KEBIASAAN (HABIT):**

- ├── Use `querySelector` for modern DOM selection
- ├── Always add `event.preventDefault()` di form handler
- ├── Use event delegation untuk list items
- ├── Update DOM setelah setiap data change (re-render)
- └── Validate input sebelum submit

**Commit:** `feat: level 5 - DOM manipulation, event handling, full UI integration`

---

## 🟠 LEVEL 6: Asynchronous Programming & Fetch API (Minggu 11)

> **Tema**: _Menghandle operasi async (API calls, timers); promise, async/await; integrate real/mock API_

> **Benang Merah**: Level 5 aplikasi hanya lokal (localStorage). Real aplikasi butuh API — fetch data dari server, tidak block UI saat menunggu. Async/await membuat async code terlihat synchronous & readable.

> **Output**: File `level6.js` dengan fetch API integration; mock API endpoint; loading indicator; error handling.

---

### A. Callback & Async Basics

> 💡 **Mengapa dimulai di sini?** Sebagian operasi butuh waktu (file read, HTTP request). JavaScript single-threaded — operasi sync block thread. Callback adalah pattern asli untuk handle async, sebelum promise/async-await ada.

Benang Merah Bagian A:  
Sync code (blocking) → Async code (non-blocking) → Callback pattern

55. `[[55. setTimeout & setInterval — Basic Async]]`
    
    - `setTimeout(callback, delay)` — jalankan callback setelah delay (ms)
    - `setInterval(callback, interval)` — jalankan callback berkali-kali setiap interval
    - `clearTimeout(id)`, `clearInterval(id)` untuk cancel
    - Blocking vs non-blocking
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        console.log("Start");
        setTimeout(() => {
          console.log("After 2 seconds");
        }, 2000);
        console.log("End");
        // Output: Start, End, After 2 seconds
        ```
        
56. `[[56. Callback Function Pattern — Passing Function as Parameter]]`
    
    - Callback adalah function yang di-pass sebagai parameter
    - Dipanggil setelah operasi selesai (atau error)
    - Pattern lama untuk handle async (sebelum promise)
    - Callback hell: nested callbacks susah dibaca
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        function fetchData(callback) {
          setTimeout(() => {
            let data = {id: 1, title: "Task 1"};
            callback(null, data); // (error, result) pattern
          }, 1000);
        }
        
        fetchData((error, data) => {
          if (error) {
            console.error("Error:", error);
          } else {
            console.log("Data:", data);
          }
        });
        ```
        
57. `[[57. Callback Hell & Why We Need Promise]]`
    
    - Multiple async operations nested dalam callbacks
    - Hard to read, maintain, handle errors
    - Promise dan async/await solve ini
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // callback hell
        fetchUser((err, user) => {
          if (err) console.error(err);
          else {
            fetchPosts(user.id, (err, posts) => {
              if (err) console.error(err);
              else {
                console.log(posts);
              }
            });
          }
        });
        ```
        

---

### B. Promise — Better Async Pattern

> 💡 **Mengapa dimulai di sini?** Promise adalah objek yang represent hasil async operasi — pending, resolved, rejected. Jauh lebih readable & composable dibanding callback.

Benang Merah Bagian B:  
Callback pattern → Promise (pending → settled: resolved/rejected) → Chaining .then()

58. `[[58. Promise Basics — Constructor & States]]`
    
    - `new Promise((resolve, reject) => { ... })`
    - States: pending → resolved (success) atau rejected (error)
    - resolve(value) — mark as success, pass value
    - reject(error) — mark as error, pass error
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        function fetchData(shouldSucceed = true) {
          return new Promise((resolve, reject) => {
            setTimeout(() => {
              if (shouldSucceed) {
                resolve({id: 1, title: "Task 1"});
              } else {
                reject("Failed to fetch");
              }
            }, 1000);
          });
        }
        
        fetchData()
          .then(data => console.log("Data:", data))
          .catch(error => console.error("Error:", error));
        ```
        
59. `[[59. .then() & .catch() — Consume Promise]]`
    
    - `.then(onResolve, onReject)` — handle resolve atau reject
    - `.catch(onReject)` — handle error (lebih readable)
    - `.finally(onFinally)` — jalankan regardless of outcome
    - Chaining: `.then().catch().finally()`
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        fetchData()
          .then(data => {
            console.log("Success:", data);
            return data.id; // return untuk chain ke .then berikutnya
          })
          .then(id => console.log("ID:", id))
          .catch(error => console.error("Error:", error))
          .finally(() => console.log("Done"));
        ```
        
60. `[[60. Promise.all() & Promise.race() — Combine Multiple Promises]]`
    
    - `Promise.all([p1, p2, ...])` — tunggu semua selesai, reject jika ada error
    - `Promise.race([p1, p2, ...])` — return hasil yang pertama selesai
    - `Promise.allSettled()` — tunggu semua, ignore errors
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let p1 = fetchUser(1);
        let p2 = fetchPosts(1);
        
        Promise.all([p1, p2])
          .then(([user, posts]) => {
            console.log("User:", user, "Posts:", posts);
          })
          .catch(error => console.error("Error:", error));
        ```
        

---

### C. Fetch API — HTTP Requests di Browser

> 💡 **Mengapa dimulai di sini?** Modern way untuk HTTP requests di browser adalah `fetch()` API. Return promise, cleaner dari XMLHttpRequest. Essential untuk integrate task manager dengan real/mock API.

Benang Merah Bagian C:  
Promise basics → fetch() API (HTTP requests) → Parse JSON response

61. `[[61. fetch() Basics — GET Request]]`
    
    - `fetch(url)` return promise
    - `response.json()` parse JSON body (return promise)
    - Check `response.ok` untuk status success
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        fetch("https://jsonplaceholder.typicode.com/todos/1")
          .then(response => {
            if (!response.ok) throw new Error("Network error");
            return response.json();
          })
          .then(data => console.log("Data:", data))
          .catch(error => console.error("Error:", error));
        ```
        
62. `[[62. fetch() dengan Method, Headers, Body — POST, PUT, DELETE]]`
    
    - Method: GET (default), POST, PUT, DELETE, PATCH
    - Headers: `{"Content-Type": "application/json"}`
    - Body: JSON.stringify(data)
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        fetch("https://api.example.com/tasks", {
          method: "POST",
          headers: {"Content-Type": "application/json"},
          body: JSON.stringify({title: "New Task"})
        })
          .then(response => response.json())
          .then(data => console.log("Created:", data))
          .catch(error => console.error("Error:", error));
        ```
        
63. `[[63. Mock API — Test Fetch tanpa Real Server]]`
    
    - Gunakan JSONPlaceholder atau buat mock function sendiri
    - Mock function return promise resolved dengan data
    - Useful untuk development sebelum backend ready
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Mock API
        function mockFetchTasks() {
          return new Promise((resolve) => {
            setTimeout(() => {
              resolve([
                {id: 1, title: "Task 1", completed: false},
                {id: 2, title: "Task 2", completed: true}
              ]);
            }, 1000);
          });
        }
        
        // Use seperti real fetch
        mockFetchTasks()
          .then(tasks => console.log("Tasks:", tasks));
        ```
        

---

### D. async/await — Synchronous-like Async Syntax

> 💡 **Mengapa dimulai di sini?** Promise chaining `.then().catch()` ok, tapi `async/await` jauh lebih readable — terlihat seperti synchronous code. Ini modern JavaScript standard untuk handle async operasi.

Benang Merah Bagian D:  
Promise .then() → async/await (syntactic sugar over promise)

64. `[[64. async Function — Return Promise]]`
    
    - `async function() { ... }` — function yang return promise
    - `await expression` — wait untuk promise selesai (return value-nya)
    - `await` hanya bisa dalam `async` function
    - Jika return value biasa, auto-wrap dalam resolved promise
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        async function getData() {
          let response = await fetch("...");
          let data = await response.json();
          return data; // auto-wrap dalam promise
        }
        
        getData().then(data => console.log(data));
        ```
        
65. `[[65. Error Handling dengan try-catch]]`
    
    - `try { ... } catch (error) { ... }` dalam async function
    - Jauh lebih clean dibanding `.catch()`
    - `finally` block jalankan regardless
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        async function fetchData() {
          try {
            let response = await fetch("...");
            if (!response.ok) throw new Error("Failed");
            let data = await response.json();
            console.log("Data:", data);
          } catch (error) {
            console.error("Error:", error.message);
          } finally {
            console.log("Done");
          }
        }
        ```
        
66. `[[66. Parallel & Sequential Async — Promise.all dalam async-await]]`
    
    - Sequential: `await op1; await op2;` — satu setelah satu
    - Parallel: `await Promise.all([op1, op2])` — bersamaan (lebih cepat)
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Sequential
        async function sequential() {
          let user = await fetchUser(1);
          let posts = await fetchPosts(user.id);
          return {user, posts};
        }
        
        // Parallel (lebih cepat)
        async function parallel() {
          let [user, posts] = await Promise.all([fetchUser(1), fetchPosts(1)]);
          return {user, posts};
        }
        ```
        

---

### E. Integrate API dengan Task Manager — Refactor Level 5

> 💡 **Mengapa dimulai di sini?** Sekarang kita **integrate fetch** ke Task Manager class — fetch tasks dari mock API saat app start, save ke server saat add/delete/update.

Benang Merah Bagian E:  
TaskManager class (sync) → TaskManager dengan async methods (fetch API)

67. `[[67. Async CRUD Methods — loadTasks, addTask async]]`
    
    - `loadTasks()` — async method, fetch dari API/localStorage
    - `addTask()` — async, POST ke API, then save locally
    - `updateTask()` — async, PUT ke API
    - `deleteTask()` — async, DELETE ke API
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        class TaskManager {
          #tasks = [];
          #nextId = 1;
          
          async loadTasks() {
            try {
              // fetch dari mock API
              let response = await fetch("/api/tasks");
              let data = await response.json();
              this.#tasks = data;
              this.#nextId = Math.max(...data.map(t => t.id)) + 1;
            } catch (error) {
              console.error("Load failed:", error);
              // fallback ke localStorage
              this.#loadFromLocalStorage();
            }
          }
          
          async addTask(title) {
            if (!title.trim()) return null;
            
            let newTask = {
              id: this.#nextId++,
              title,
              completed: false,
              createdAt: new Date()
            };
            
            try {
              // POST ke API
              let response = await fetch("/api/tasks", {
                method: "POST",
                headers: {"Content-Type": "application/json"},
                body: JSON.stringify(newTask)
              });
              
              if (!response.ok) throw new Error("Create failed");
              this.#tasks.push(newTask);
              return newTask;
            } catch (error) {
              console.error("Add task error:", error);
              throw error;
            }
          }
          
          #loadFromLocalStorage() {
            let stored = localStorage.getItem("tasks");
            this.#tasks = stored ? JSON.parse(stored) : [];
          }
        }
        ```
        
68. `[[68. Loading Indicator — UI Feedback saat Async Waiting]]`
    
    - Show spinner/loading message saat fetch
    - Hide saat selesai
    - Prevent double-click (disable button saat loading)
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        async function handleAddTask() {
          let title = taskInput.value;
          if (!title.trim()) return;
          
          addBtn.disabled = true;
          showLoading(true);
          
          try {
            await window.manager.addTask(title);
            taskInput.value = "";
            renderTasks();
          } catch (error) {
            showError(error.message);
          } finally {
            addBtn.disabled = false;
            showLoading(false);
          }
        }
        
        function showLoading(show) {
          let loader = document.getElementById("loader");
          loader.style.display = show ? "block" : "none";
        }
        ```
        
69. `[[69. Mock API Server — JSONServer atau Mock Function]]`
    
    - Use JSONPlaceholder (existing public API)
    - Atau setup json-server locally
    - Atau buat mock function return promises
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Mock API
        const MOCK_TASKS = [
          {id: 1, title: "Belajar JS", completed: false},
          {id: 2, title: "Bikin UI", completed: true}
        ];
        
        function mockFetch(url, options = {}) {
          return new Promise((resolve, reject) => {
            setTimeout(() => {
              if (url === "/api/tasks" && options.method === "GET") {
                resolve({
                  ok: true,
                  json: () => Promise.resolve([...MOCK_TASKS])
                });
              } else if (url === "/api/tasks" && options.method === "POST") {
                let newTask = JSON.parse(options.body);
                MOCK_TASKS.push(newTask);
                resolve({
                  ok: true,
                  json: () => Promise.resolve(newTask)
                });
              }
            }, 500);
          });
        }
        
        // Override fetch globally atau use di TaskManager
        // global.fetch = mockFetch;
        ```
        

---

### 🏗 Checkpoint Level 6

✅ **Checklist sebelum lanjut ke Level 7:**

**PEMAHAMAN:**

- ├── Bisa jelaskan callback, promise, async/await
- ├── Bisa jelaskan promise states (pending, resolved, rejected)
- ├── Bisa jelaskan fetch() API & HTTP methods
- ├── Bisa jelaskan async/await vs .then().catch()
- └── Bisa jelaskan try/catch dalam async function

**PROYEK:**

- ├── File `level6.js` dengan async TaskManager methods:
    - `async loadTasks()` — fetch dari mock API
    - `async addTask(title)` — POST ke mock API
    - `async updateTask(id, updates)` — PUT
    - `async deleteTask(id)` — DELETE
- ├── Mock API server (use mock function atau json-server)
- ├── UI updates:
    - Loading spinner saat fetch
    - Disable button saat loading (prevent double-click)
    - Error message jika fetch gagal
- ├── App flow:
    - Load tasks saat app start (async)
    - Add/update/delete trigger API call (async)
    - Re-render setelah API respond
- └── Fallback: jika API down, use localStorage

**KEBIASAAN (HABIT):**

- ├── Use async/await, bukan .then() (kecuali untuk short expressions)
- ├── Always try/catch async operations
- ├── Show loading indicator untuk UX
- ├── Test dengan DevTools Network tab (throttle simulate slow network)
- └── Handle network errors gracefully

**Commit:** `feat: level 6 - async/await, fetch API, mock server, loading states`

---

## 🔴 LEVEL 7: Advanced JavaScript Features (Minggu 12)

> **Tema**: _Master destructuring, spread/rest operators, higher-order functions, filter/map/reduce, closure advanced concepts_

> **Benang Merah**: Kita sudah paham basics. Level ini adalah **advanced patterns** yang make code lebih concise & functional. Ini adalah "fluent JavaScript" — tidak hanya bisa, tapi elegant.

> **Output**: File `level7.js` dengan refactor aplikasi pakai destructuring, spread, functional methods; advanced filter, sort, search.

---

### A. Destructuring — Extract Values dari Array/Object

> 💡 **Mengapa dimulai di sini?** Destructuring adalah syntax sugar untuk extract values dari array/object jadi variabel terpisah. Bikin code lebih concise.

Benang Merah Bagian A:  
Object/array access (level 2-3) → Destructuring (modern, concise)

70. `[[70. Array Destructuring — Unpack Array ke Variables]]`
    
    - `const [a, b, c] = array` — unpack ke a, b, c
    - Skip elements: `const [a, , c] = array`
    - Rest: `const [a, ...rest] = array`
    - Default values: `const [a = 10, b = 20] = array`
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        const [first, second, ...others] = [1, 2, 3, 4, 5];
        console.log(first, second, others); // 1, 2, [3, 4, 5]
        
        const [x = 0, y = 0] = [5];
        console.log(x, y); // 5, 0
        ```
        
71. `[[71. Object Destructuring — Extract Object Properties]]`
    
    - `const {key1, key2} = obj` — extract properties
    - Rename: `const {key: newName} = obj`
    - Default: `const {key = "default"} = obj`
    - Rest: `const {a, b, ...rest} = obj`
    - Nested: `const {user: {name}} = obj`
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let task = {id: 1, title: "Belajar", completed: false, priority: "high"};
        const {id, title, ...others} = task;
        console.log(id, title, others); // 1, "Belajar", {completed: false, priority: "high"}
        
        const {title: taskTitle} = task;
        console.log(taskTitle); // "Belajar"
        ```
        
72. `[[72. Destructuring dalam Function Parameter]]`
    
    - Destructure di parameter, lebih clean
    - Tidak perlu akses param.prop
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // tanpa destructuring
        function displayTask(task) {
          console.log(task.title, task.priority);
        }
        
        // dengan destructuring
        function displayTask({title, priority = "normal"}) {
          console.log(title, priority);
        }
        
        // arrow function
        const displayTask2 = ({title, priority}) => console.log(title, priority);
        ```
        

---

### B. Spread & Rest Operators — Expand & Collect

> 💡 **Mengapa dimulai di sini?** Spread `...` expand array/object. Rest `...` collect arguments. Keduanya make code lebih flexible & readable.

Benang Merah Bagian B:  
Destructuring (extract) → Spread (expand) → Rest (collect)

73. `[[73. Spread Operator — ... untuk Array & Object]]`
    
    - Array spread: `[...arr1, ...arr2]` — combine
    - Object spread: `{...obj1, ...obj2}` — merge
    - Copy: `[...arr]`, `{...obj}` — shallow copy
    - Rest parameter di function
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let arr1 = [1, 2, 3];
        let arr2 = [4, 5, 6];
        let combined = [...arr1, ...arr2];
        console.log(combined); // [1, 2, 3, 4, 5, 6]
        
        let obj1 = {name: "Alice", age: 25};
        let obj2 = {age: 26, city: "NYC"};
        let merged = {...obj1, ...obj2};
        console.log(merged); // {name: "Alice", age: 26, city: "NYC"}
        ```
        
74. `[[74. Rest Parameter — ... untuk Function Arguments]]`
    
    - `function fn(...args) { ... }` — collect semua arguments
    - `args` adalah array
    - Use case: variadic functions
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        function sum(...numbers) {
          return numbers.reduce((a, b) => a + b, 0);
        }
        console.log(sum(1, 2, 3, 4, 5)); // 15
        
        function greet(greeting, ...names) {
          names.forEach(name => console.log(greeting + " " + name));
        }
        greet("Hello", "Alice", "Bob", "Charlie");
        ```
        

---

### C. Higher-Order Functions & map/filter/reduce

> 💡 **Mengapa dimulai di sini?** Level 2-3 kita pelajari map/filter. Sekarang **understand deeply** — ini adalah functional programming basics. Higher-order functions (function yang terima/return function) adalah powerful pattern.

Benang Merah Bagian C:  
Functional methods (map/filter dari level 2) → HOF (function yang terima function) → Compose functions

75. `[[75. map — Transform Array Elements]]`
    
    - `array.map(element => newElement)` — transform setiap elemen
    - Return array baru, tidak mutate original
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let tasks = [{id: 1, title: "Task 1"}, {id: 2, title: "Task 2"}];
        let titles = tasks.map(t => t.title);
        console.log(titles); // ["Task 1", "Task 2"]
        
        // chain operations
        let formattedTitles = tasks
          .map(t => t.title)
          .map(title => title.toUpperCase());
        ```
        
76. `[[76. filter — Select Elements Matching Condition]]`
    
    - `array.filter(element => boolean)` — keep elements yang true
    - Return array baru, tidak mutate original
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let tasks = [
          {id: 1, title: "Task 1", completed: false},
          {id: 2, title: "Task 2", completed: true},
          {id: 3, title: "Task 3", completed: false}
        ];
        
        let pendingTasks = tasks.filter(t => !t.completed);
        console.log(pendingTasks); // [task 1, task 3]
        
        // chain dengan map
        let pendingTitles = tasks
          .filter(t => !t.completed)
          .map(t => t.title);
        ```
        
77. `[[77. reduce — Aggregate Array ke Single Value]]`
    
    - `array.reduce((accumulator, element) => newAccumulator, initialValue)`
    - Powerful — bisa gunakan untuk sum, group, transform
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        let scores = [85, 90, 78, 92];
        let sum = scores.reduce((acc, score) => acc + score, 0);
        console.log(sum); // 345
        
        // group by status
        let tasks = [
          {id: 1, status: "done"},
          {id: 2, status: "pending"},
          {id: 3, status: "done"}
        ];
        
        let grouped = tasks.reduce((acc, task) => {
          acc[task.status] = (acc[task.status] || 0) + 1;
          return acc;
        }, {});
        console.log(grouped); // {done: 2, pending: 1}
        ```
        
78. `[[78. Higher-Order Function — Function yang Terima-Return Function]]`
    
    - Terima function sebagai parameter
    - Return function dari function
    - Pattern: currying, factory, decorator
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // HOF: terima function
        function applyTwice(fn, value) {
          return fn(fn(value));
        }
        
        const double = x => x * 2;
        console.log(applyTwice(double, 3)); // 12
        
        // HOF: return function
        function makeMultiplier(factor) {
          return (x) => x * factor;
        }
        
        const double2 = makeMultiplier(2);
        const triple = makeMultiplier(3);
        console.log(double2(5)); // 10
        console.log(triple(5)); // 15
        ```
        

---

### D. Advanced Patterns — Currying, Composition, Memoization

> 💡 **Mengapa dimulai di sini?** Ini adalah elegant JavaScript patterns. Tidak wajib, tapi membuat code lebih powerful & reusable.

Benang Merah Bagian D:  
HOF basics → Currying (partial application) → Composition (chain functions)

79. `[[79. Currying — Transform Function dengan Multiple Arguments]]`
    
    - Curried function: return function yang expect remaining arguments
    - Pattern: `fn(a)(b)(c)` instead of `fn(a, b, c)`
    - Useful untuk partial application & composition
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // regular function
        function add(a, b, c) {
          return a + b + c;
        }
        
        // curried version
        function curriedAdd(a) {
          return function(b) {
            return function(c) {
              return a + b + c;
            };
          };
        }
        
        console.log(curriedAdd(1)(2)(3)); // 6
        
        // partial application
        let add1 = curriedAdd(1);
        let add1And2 = add1(2);
        console.log(add1And2(3)); // 6
        ```
        
80. `[[80. Function Composition — Combine Functions]]`
    
    - Compose: `(f ∘ g)(x) = f(g(x))`
    - Chain transformations
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        const compose = (f, g) => (x) => f(g(x));
        
        const addOne = x => x + 1;
        const double = x => x * 2;
        
        const doubleThenAddOne = compose(addOne, double);
        console.log(doubleThenAddOne(5)); // addOne(double(5)) = addOne(10) = 11
        
        // pipe (reverse): g, then f
        const pipe = (g, f) => (x) => f(g(x));
        const doPipeline = pipe(double, addOne);
        console.log(doPipeline(5)); // 11 (sama)
        ```
        

---

### E. Refactor Task Manager — Advanced Patterns

> 💡 **Mengapa dimulai di sini?** Apply destructuring, spread, map/filter/reduce, HOF ke task manager — make code lebih concise, readable, functional.

Benang Merah Bagian E:  
CRUD methods + DOM (level 5-6) → Refactor dengan destructuring, functional methods

81. `[[81. Filter, Search & Sort Tasks — Using map, filter, reduce]]`
    
    - Search: filter berdasarkan title keyword
    - Filter: by status (completed/pending), priority
    - Sort: by date created, priority
    - Chain operations
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        class TaskManager {
          // ... existing code ...
          
          searchTasks(keyword) {
            return this.#tasks.filter(t => 
              t.title.toLowerCase().includes(keyword.toLowerCase())
            );
          }
          
          getTasksByStatus(status) {
            return this.#tasks.filter(t => t.completed === (status === "done"));
          }
          
          sortTasks(by = "date") {
            return [...this.#tasks].sort((a, b) => {
              if (by === "date") return b.createdAt - a.createdAt;
              if (by === "priority") {
                const priority = {high: 3, normal: 2, low: 1};
                return priority[b.priority] - priority[a.priority];
              }
              return 0;
            });
          }
          
          // Complex: filter pending, sorted by priority
          getPendingSortedByPriority() {
            return this.#tasks
              .filter(t => !t.completed)
              .sort((a, b) => ({high: 3, normal: 2, low: 1}[b.priority] - {high: 3, normal: 2, low: 1}[a.priority]));
          }
        }
        ```
        
82. `[[82. Destructuring dalam DOM Rendering]]`
    
    - Destructure task properties saat render
    - More concise, readable
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        function renderTasks() {
          let tasks = window.manager.getTasks();
          let taskList = document.getElementById("taskList");
          
          taskList.innerHTML = tasks.map(({id, title, completed}) => `
            <li class="task-item ${completed ? "completed" : ""}" data-id="${id}">
              <span>${title}</span>
              <button class="btn-complete">${completed ? "Undo" : "Complete"}</button>
              <button class="btn-delete">Delete</button>
            </li>
          `).join("");
        }
        ```
        
83. `[[83. Spread dalam Event Handling & DOM Updates]]`
    
    - Spread: combine data untuk update
    - Immutable pattern: create new object, tidak mutate original
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        async function handleCompleteTask(taskId) {
          let task = window.manager.getTasks().find(t => t.id === taskId);
          if (!task) return;
          
          // Immutable update: create new object dengan perubahan
          let updated = {...task, completed: !task.completed};
          await window.manager.updateTask(taskId, updated);
          renderTasks();
        }
        ```
        

---

### 🏗 Checkpoint Level 7

✅ **Checklist sebelum selesai (FINAL):**

**PEMAHAMAN:**

- ├── Bisa jelaskan destructuring (array & object)
- ├── Bisa jelaskan spread & rest operators
- ├── Bisa jelaskan map/filter/reduce deeply
- ├── Bisa jelaskan higher-order functions
- └── Bisa jelaskan currying & composition

**PROYEK:**

- ├── File `level7.js` dengan refactored Task Manager:
    - Use destructuring dalam methods & DOM rendering
    - Implement search, filter (by status), sort (by date/priority)
    - Use functional methods (map/filter/reduce) untuk data manipulation
    - HOF: `sortTasks()`, custom filter factory
- ├── HTML with advanced features:
    - Search input untuk find tasks by keyword
    - Filter buttons: All, Pending, Done
    - Sort dropdown: By Date, By Priority
    - Display count: "3 tasks, 1 completed"
- ├── Full-featured Task Manager:
    - CRUD operations (level 3)
    - DOM integration & events (level 5)
    - Async/API integration (level 6)
    - Advanced filtering, searching, sorting (level 7)
    - Persistent data (localStorage + mock API)
    - Input validation & error handling
- └── Test all features: add, complete, delete, search, filter, sort, persist

**KEBIASAAN (HABIT):**

- ├── Use destructuring untuk extract values
- ├── Use spread untuk immutable updates
- ├── Use map/filter/reduce untuk array transformations
- ├── Chain operations (map→filter, filter→map)
- └── Write small, composable functions

**Commit:** `refactor: level 7 - destructuring, spread/rest, functional programming, advanced features`

---

## 🏁 Final Checkpoint — Aplikasi Complete

✅ **Aplikasi Task Manager FINAL harus memiliki:**

**FEATURES:**

- ✅ Tambah task (dengan validasi input)
- ✅ Tampilkan task list (dengan formatting)
- ✅ Mark task as complete/pending
- ✅ Delete task
- ✅ Search tasks by keyword
- ✅ Filter by status (All/Pending/Completed)
- ✅ Sort by date created atau priority
- ✅ Persistent data (localStorage)
- ✅ Loading indicator (async operations)
- ✅ Error handling & user feedback

**CODE QUALITY:**

- ✅ Class-based architecture (TaskManager)
- ✅ Encapsulation (private properties/methods)
- ✅ Async/await (for API calls)
- ✅ Functional programming (map/filter/reduce)
- ✅ Destructuring & spread operators
- ✅ Event delegation (efficient event handling)
- ✅ Validation & error handling
- ✅ Comments & readable naming

**TESTING:**

- Test add/update/delete in console
- Test search, filter, sort functionality
- Test localStorage persistence
- Test error handling (empty input, API fail)
- Test on slow network (DevTools throttle)

---

## 🎓 Kurikulum Inti — 21 Materi Fundamental (JavaScript Mapping)

|No|Materi|JavaScript Implementation|Level|
|---|---|---|---|
|1|**Variabel**|`let`, `const`, `var`|L1|
|2|**Tipe Data**|string, number, boolean, null, undefined, object, symbol, bigint|L1|
|3|**Operator**|`+`, `-`, `*`, `/`, `%`, `**`, `===`, `!==`, `??`, `?.`, `&&`, `\|`|L1|
|4|**Condition**|`if/else`, `switch`, ternary `? :`|L1|
|5|**Looping**|`for`, `for-of`, `for-in`, `while`, `do-while`, `.forEach()`|L1-L2|
|6|**Array**|`[]`, `.map()`, `.filter()`, `.find()`, `.indexOf()`, `.push()`, `.pop()`|L2|
|7|**Function**|`function`, arrow `=>`, parameter, return, default parameter|L3|
|8|**OOP**|`class`, `constructor`, `this`, inheritance `extends`, polymorphism|L4|
|9|**I/O**|`console.log()`, `console.error()`, DOM API, `fetch()`|L1, L5, L6|
|10|**String Manipulation**|`.slice()`, `.split()`, `.replace()`, `.trim()`, template literal `` ` ``|L2|
|11|**Type Casting**|`Number()`, `String()`, `Boolean()`, `parseInt()`|L1|
|12|**Scope & Lifetime**|function scope, block scope (`let`/`const`), closure, hoisting|L3|
|13|**Error Handling**|`try/catch/finally`, `throw`, custom Error|L4, L6|
|14|**Data Structures**|Object `{}`, Map, Set (beyond Array)|L3|
|15|**Recursion**|Recursive function, base case|L7|
|16|**Reference vs Value**|Primitif by value, object/array by reference|L3|
|17|**Modularitas**|ESM `import/export`, CJS `require` (Node.js context)|L5-L6|
|18|**File Handling**|Browser File API, `fetch` for remote files, localStorage|L5-L6|
|19|**Asynchronous**|`Promise`, `async/await`, event loop, `fetch()`|L6|
|20|**Functional Programming**|`.map()`, `.filter()`, `.reduce()`, HOF, pure function, composition|L2, L7|
|21|**Debugging**|DevTools console, breakpoint, `debugger`, error inspection|Semua level|

---

## 🌟 Fitur Khas JavaScript (Wajib Dibahas)

- ✅ **Closure** — Function nesting, lexical scoping, private variables (L3)
- ✅ **Prototype Chain** — Object inheritance, `__proto__`, `Object.create()` (L4)
- ✅ **this Binding** — Regular vs arrow function, `.bind()`, `.call()`, `.apply()` (L4)
- ✅ **Hoisting** — Variable & function hoisting, TDZ (temporal dead zone) (L1)
- ✅ **Event Loop** — Call stack, callback queue, microtask queue, async execution (L6)
- ✅ **Promise & async-await** — Microtask, resolve/reject, error handling (L6)
- ✅ **Destructuring** — Array & object destructuring, rest parameters (L7)
- ✅ **Spread & Rest** — Array/object spread, variadic functions (L7)
- ✅ **Template Literal** — String interpolation, multi-line strings (L2)
- ✅ **Arrow Function** — Lexical `this`, concise syntax (L3, L7)
- ✅ **DOM & Web API** — querySelector, events, fetch, localStorage (L5-L6)

---

## 📊 Ringkasan Level & Estimasi Waktu

|Level|Tema|Minggu|21 Fundamental|Fitur Khas|Output|
|---|---|---|---|---|---|
|1|Basics|1-2|Variabel, Tipe Data, Operator, Condition, Looping|Hoisting|Console output|
|2|Array & String|3-4|Array, String Manipulation, Tipe Casting|Template Literal|Array operations|
|3|Function & CRUD|5-6|Function, Scope & Closure, Data Structure|Closure, HOF|CRUD logic, localStorage|
|4|OOP & Class|7-8|OOP, Scope & Lifetime, Error Handling|Prototype, this binding|TaskManager class|
|5|DOM & Events|9-10|I/O (DOM), String (template)|DOM API, Event Loop intro|Interactive UI|
|6|Async & Fetch|11|Async, Error Handling, Reference vs Value|Promise, async-await|API integration|
|7|Advanced|12|Functional, Recursion, Data Structures|Destructuring, Spread|Polished app|

---

## 🚀 Setelah Level 7 — Langkah Selanjutnya

1. **Node.js & Backend** — Use JavaScript di server (Express, NestJS)
2. **Frontend Framework** — React, Vue, Svelte untuk UI lebih kompleks
3. **TypeScript** — Add static typing untuk code reliability
4. **Testing** — Jest, Mocha untuk unit test, E2E test
5. **Performance & Optimization** — Profiling, lazy loading, code splitting
6. **Web APIs Advanced** — Service Worker, WebSocket, WebWorkers
7. **Design Patterns** — Factory, Observer, Strategy, Singleton
8. **Security** — XSS prevention, CSRF, secure authentication

---

**Selamat belajar JavaScript! Roadmap ini adalah journey dari zero hingga menguasai JavaScript fundamental. Setiap level dibangun di atas level sebelumnya, dengan satu project yang tumbuh bersama.**