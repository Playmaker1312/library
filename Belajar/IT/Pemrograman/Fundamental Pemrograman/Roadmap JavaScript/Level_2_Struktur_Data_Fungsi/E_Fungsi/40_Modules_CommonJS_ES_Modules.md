# 40. Modules — CommonJS & ES Modules

---

## 1. Penjelasan

**Module** adalah cara memisahkan kode JavaScript ke dalam file-file terpisah, masing-masing dengan **scope sendiri**. Sebelum modules, semua variabel global bisa saling tabrak.

| Sistem | Platform | Syntax | Ekstensi |
|--------|----------|--------|----------|
| **CommonJS (CJS)** | Node.js (default) | `require()` / `module.exports` | `.js`, `.cjs` |
| **ES Modules (ESM)** | Browser + Node.js modern | `import` / `export` | `.mjs`, `.js` (dengan `"type": "module"`) |

**Perbandingan:**

| Aspek | CommonJS | ES Modules |
|-------|----------|------------|
| Loading | **Synchronous** (require dijalankan saat itu juga) | **Asynchronous** (import di-hoist, dianalisis statis) |
| Scope | Module-scoped | Module-scoped |
| Hoisting | Tidak | **Import di-hoist** |
| Default export | `module.exports = ...` | `export default ...` |
| Named export | `exports.foo = ...` | `export { foo }` |
| Static analysis | Tidak | Ya — memungkinkan tree-shaking |
| Circular dependency | Berbahaya tapi bisa | Lebih aman (live binding) |

---

## 2. Fungsi Modules

- **Mencegah** polusi global scope
- **Enkapsulasi** — hanya expose apa yang perlu
- **Reusability** — kode bisa dipakai di banyak project
- **Maintainability** — kode terorganisir per fitur
- **Tree-shaking** (ESM) — hapus kode yang tidak dipakai saat build

---

## 3. Code — Refactor Todo List Multi-File

### Struktur Project
```
todo-app/
├── index.js      # entry point
├── todo.js       # logika todo (CRUD)
├── storage.js    # penyimpanan (localStorage simulasi)
└── ui.js         # tampilan console
```

### CommonJS Version

**todo.js:**
```javascript
let todos = [];
let nextId = 1;

function add(title) {
    const todo = { id: nextId++, title, done: false };
    todos.push(todo);
    return todo;
}

function list() {
    return [...todos]; // return copy (immutability)
}

function toggle(id) {
    const todo = todos.find(t => t.id === id);
    if (todo) todo.done = !todo.done;
    return todo;
}

function remove(id) {
    const idx = todos.findIndex(t => t.id === id);
    if (idx !== -1) return todos.splice(idx, 1)[0];
    return null;
}

module.exports = { add, list, toggle, remove };
```

**storage.js:**
```javascript
const fs = require("fs");
const path = "todos.json";

function save(todos) {
    fs.writeFileSync(path, JSON.stringify(todos));
}

function load() {
    try {
        return JSON.parse(fs.readFileSync(path, "utf-8"));
    } catch {
        return [];
    }
}

module.exports = { save, load };
```

**ui.js:**
```javascript
function showMenu() {
    console.log("\n=== TODO LIST ===");
    console.log("1. Tambah");
    console.log("2. Lihat");
    console.log("3. Toggle");
    console.log("4. Hapus");
    console.log("5. Keluar");
}

function displayTodos(todos) {
    if (todos.length === 0) return console.log("(kosong)");
    todos.forEach(t => {
        const status = t.done ? "[x]" : "[ ]";
        console.log(`${status} ${t.id}. ${t.title}`);
    });
}

module.exports = { showMenu, displayTodos };
```

**index.js:**
```javascript
const readline = require("readline");
const todo = require("./todo");
const ui = require("./ui");
const storage = require("./storage");

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

function main() {
    ui.showMenu();
    rl.question("Pilih: ", (choice) => {
        switch (choice) {
            case "1": /* tambah */ break;
            case "2": ui.displayTodos(todo.list()); break;
            // ... dll
        }
        main();
    });
}

main();
```

### ES Modules Version (sama, beda syntax)

**todo.mjs / todo.js (dengan `"type": "module"`):**
```javascript
let todos = [];
let nextId = 1;

export function add(title) { /* sama */ }
export function list() { return [...todos]; }
export function toggle(id) { /* sama */ }

export default { add, list, toggle };
```

**index.mjs:**
```javascript
import readline from "readline";
import * as todo from "./todo.js";
import { showMenu, displayTodos } from "./ui.js";
import { save } from "./storage.js";
```

---

## 4. Analogi Rumah

| Konsep | Analogi Rumah |
|--------|---------------|
| Module | **Ruangan terpisah** — dapur, kamar, ruang tamu, gudang |
| CommonJS `require` | **Ambil alat dari ruang lain** — synchronous, langsung bawa |
| ES `import` | **Daftar alat yang sudah direncanakan** sebelum mulai kerja |
| `module.exports` | **Pintu** — apa saja yang boleh keluar dari ruangan |
| `export default` | **Pintu utama** — satu akses utama ke ruangan |
| `export named` | **Jendela, pintu samping, lubang angin** — akses spesifik |
| Global scope (tanpa module) | **Rumah tanpa sekat** — semua barang campur aduk |
| Module scope | **Setiap ruangan punya lemari sendiri** — isinya tidak bocor |
| Tree-shaking | **Hanya bawa alat yang diperlukan** ke ruang lain, sisanya tetap di gudang |
| Circular dependency | **Dapur bergantung pada ruang makan, ruang makan bergantung pada dapur** — bisa kacau |
| Package manager (npm) | **Toko bangunan** — ambil modul siap pakai |

**Narasi:** Rumah yang baik punya ruangan terpisah: dapur, kamar tidur, kamar mandi, ruang tamu. Masing-masing punya alat sendiri-sendiri. Dapur punya kompor, kamar mandi punya gayung. Kalau semua alat ditumpuk di ruang tamu (global scope), kacau balau! Modules seperti memberi setiap ruangan sekat dan pintu — Anda hanya bisa mengambil alat yang **diizinkan keluar** (export). Dapur `export` kompor ke ruang makan, tapi tidak `export` sabun cuci piring. Pintu (module.exports) mengontrol apa yang terlihat dari luar.

---

## 5. Use Case

| Situasi | Module System |
|---------|--------------|
| **Aplikasi web modern** | ES Modules (Vite, Webpack, React, Vue) |
| **Node.js backend** | CommonJS (legacy) / ES Modules (modern) |
| **Library / Package npm** | Dual: CommonJS + ESM |
| **Browser (native)** | ES Modules — `<script type="module">` |
| **Monorepo** | ES Modules — import lintas package |
| **CLI tools** | CommonJS biasanya |

**Cara setup ES Modules di Node.js:**
```json
// package.json
{
    "type": "module"
}
```
Atau gunakan ekstensi `.mjs`.

---

## 6. Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|-----------|--------|--------|
| Lupa `type: "module"` | Pakai `import` di `.js` biasa | SyntaxError |
| **Circular dependency** | `A require B`, `B require A` | Partial loading, nilai undefined |
| Destructuring bukan default | `import { readline } from "readline"` | undefined (readline adalah default export) |
| `require` di ESM | Pakai `require` di `.mjs` | ReferenceError |
| `__dirname` di ESM | Tidak ada di ESM | Harus pakai `import.meta.url` |
| Named export bukan objek | `export { add }` vs `export default { add }` | Kebingungan saat import |
| Mutasi module state | Module punya state internal yang diubah dari luar | Bug tak terduga |

```javascript
// Circular dependency — BERBAHAYA
// a.js
const b = require("./b");
console.log(b); // mungkin {} (kosong)
module.exports = { name: "A" };

// b.js
const a = require("./a");
module.exports = { name: "B", friend: a };
// a mungkin belum terisi penuh!
```

---

## 7. Benang Merah

**PENUTUP Bagian Fungsi.**

Materi 33-39 (semua tentang fungsi) → **Materi 40 (modules: memisahkan kode fungsi ke file berbeda)** → Bagian OOP (Materi 41-45): class, prototype, inheritance, encapsulation

Modules menyelesaikan masalah global scope (Materi 33) dan memungkinkan enkapsulasi yang lebih baik. Ini adalah transisi dari procedural programming (fungsi sebagai unit) ke object-oriented programming (class dan objek sebagai unit).

---

## 8. Soal

### Soal 1 (Mudah)
Apa perbedaan `export default` dan `export named`?

<details>
<summary>Jawaban</summary>
`export default` — satu export utama per file, di-import tanpa kurung kurawal: `import X from "./x"`.
`export named` — bisa banyak per file, di-import dengan kurung kurawal: `import { a, b } from "./x"`.
</details>

### Soal 2 (Sedang)
File `math.js` berisi:
```javascript
export const PI = 3.14;
export function add(a, b) { return a + b; }
export default function multiply(a, b) { return a * b; }
```
Tulis 2 cara import yang benar untuk menggunakan `multiply`.

<details>
<summary>Jawaban</summary>
```javascript
// Cara 1 — import default
import multiply from "./math.js";
console.log(multiply(2, 3)); // 6

// Cara 2 — import semua
import * as math from "./math.js";
console.log(math.default(2, 3)); // 6 — default ada di .default

// Cara 3 — named alias
import { default as kali } from "./math.js";
console.log(kali(2, 3)); // 6
```
</details>

### Soal 3 (Sulit)
Mengapa kode ini error? Bagaimana memperbaikinya?
```javascript
// utils.js
module.exports = {
    add: (a, b) => a + b
};

// main.js (ES Module)
import { add } from "./utils.js";
console.log(add(2, 3));
```

<details>
<summary>Jawaban</summary>
Error karena `main.js` ES Module (`import`) mencoba import dari `utils.js` yang CommonJS (`module.exports`). Solusi:
1. Ubah `utils.js` ke ES Module: `export const add = ...`
2. Atau import default: `import utils from "./utils.js"` lalu `utils.add(2,3)`
3. Atau ganti `main.js` ke CommonJS: `const { add } = require("./utils");`
4. Atau tambah `"type": "module"` di package.json dan gunakan `createRequire` untuk interop

Node.js modern mendukung interop: `import` bisa import CJS module (default export = module.exports), tapi named export mungkin perlu cara khusus.
</details>
