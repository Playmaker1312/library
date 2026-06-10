# 39. Pure Function, Side Effects & Immutability

---

## 1. Penjelasan

**Pure Function** adalah fungsi yang memenuhi dua syarat:

| Syarat | Penjelasan |
|--------|------------|
| **Deterministic** | Input yang sama → output yang sama (selalu) |
| **No Side Effects** | Tidak mengubah state di luar fungsi (global, DOM, file, database, dll) |

**Side Effects** adalah perubahan state yang diamati di luar scope fungsi:

| Side Effect | Contoh |
|-------------|--------|
| Memodifikasi variabel global | `total += x` |
| I/O ke console | `console.log()` |
| Manipulasi DOM | `document.title = x` |
| HTTP request | `fetch(url)` |
| Math.random() | Nilai berbeda tiap panggilan |
| Date.now() | Nilai tergantung waktu |
| Memodifikasi argumen | `arr.push(5)` — mutasi array input |

**Immutability** = tidak mengubah data yang sudah ada, melainkan membuat data **baru**:

```javascript
// Mutasi (impure)
const arr = [1, 2, 3];
arr.push(4);  // arr sekarang [1,2,3,4]

// Immutable (pure)
const arr2 = [1, 2, 3];
const arr3 = [...arr2, 4]; // arr2 tetap [1,2,3], arr3 = [1,2,3,4]
```

---

## 2. Fungsi Pure Function

- **Testability** — test jadi mudah (input → output, tanpa setup state)
- **Predictability** — kode lebih mudah dipahami dan di-debug
- **Concurrency** — aman dijalankan paralel (tidak ada race condition)
- **Caching / Memoization** — hasil bisa di-cache berdasarkan input
- **Refactoring** — bisa dipindah-pindah tanpa efek samping
- **Koneksi ke map/filter/reduce** — metode ini pure (tidak mengubah array original)

---

## 3. Code — Identifikasi & Refactor

### Kode 1: Impure → Pure
```javascript
// IMPURE — mengubah global
let total = 0;
function addToTotal(x) {
    total += x;
    return total;
}

// PURE — tidak ada side effect
function add(a, b) {
    return a + b;
}
```

### Kode 2: Impure → Pure
```javascript
// IMPURE — memutasi argumen
function addItem(arr, item) {
    arr.push(item);
    return arr;
}

// PURE — membuat array baru
function addItemPure(arr, item) {
    return [...arr, item];
}
```

### Kode 3: Impure → Pure
```javascript
// IMPURE — random + console.log
function rollDice() {
    const result = Math.floor(Math.random() * 6) + 1;
    console.log(`Dadu: ${result}`);
    return result;
}

// Lebih pure — pisahkan I/O
function rollDicePure() {
    return Math.floor(Math.random() * 6) + 1;
}
// console.log tetap side effect, tapi dipisah
```

### Kode 4: Impure → Pure
```javascript
// IMPURE — membaca tanggal global
function getGreeting() {
    const hour = new Date().getHours();
    return hour < 12 ? "Selamat pagi" : "Selamat siang";
}

// PURE — input explicit
function getGreetingPure(hour) {
    return hour < 12 ? "Selamat pagi" : "Selamat siang";
}
```

### Kode 5: Identifikasi pure vs impure
```javascript
// PURE
function multiply(a, b) { return a * b; }
function capitalize(str) { return str[0].toUpperCase() + str.slice(1); }
const double = arr => arr.map(x => x * 2);

// IMPURE
let count = 0;
function increment() { count++; }
function log(msg) { console.log(msg); }
function updateUser(user) { user.name = "Baru"; return user; }
```

---

## 4. Analogi Rumah

| Konsep | Analogi Rumah |
|--------|---------------|
| Pure function | **Mesin cetak bata** — masukkan tanah liat dan air, keluar bata. Input sama → output sama |
| Impure function | **Mesin ambil bata dari tumpukan, hancurkan, lalu cetak ulang** — tumpukan berubah |
| Side effect | **Menumpahkan cat** saat mengecat — ada dampak di luar area kerja |
| Immutability | **Cetak biru** — Anda tidak menghapus cetak biru lama, Anda buat cetak biru baru dengan perubahan |
| Mutasi | **Ngecat ulang dinding yang sudah kering** tanpa bilang siapa-siapa — orang lain kaget |
| map/filter (pure) | **Mesin fotokopi** — ambil daftar, hasilkan daftar baru, daftar lama tetap utuh |
| forEach (impure) | **Mengetik ulang daftar** di kertas baru — aslinya tidak berubah, tapi Anda membuang kertas lama |
| Memoization | **Resep masakan** yang sudah dicoba — besok input sama, tinggal lihat hasil sebelumnya |
| Deterministic | **Timbangan** — 1 kg beras selalu 1 kg. Tidak peduli hari apa |
| Non-deterministic | **Kursi goyang** — bisa miring ke kiri atau kanan tergantung angin |

**Narasi:** Di pabrik bahan bangunan, ada mesin cetak bata (pure function): masukkan tanah liat + air, selalu keluar bata yang sama. Tidak ada efek lain. Bandingkan dengan mesin yang mengambil bata dari tumpukan yang sudah jadi, menghancurkannya, lalu mencetak ulang — tumpukan berubah (side effect). Immutability seperti cetak biru: jika ingin mengubah desain, Anda tidak menghapus cetak biru asli. Anda fotokopi, lalu ubah fotokopinya. Cetak biru asli tetap utuh untuk referensi.

---

## 5. Use Case

| Situasi | Pendekatan |
|---------|-----------|
| **Utility function** (math, string) | Pure — mudah di-test |
| **State management** (Redux) | Wajib pure (reducer) |
| **Array transformation** | map/filter/reduce (pure) |
| **I/O operations** (fetch, file) | Impure — diterima, tapi **dipisah** dari logika bisnis |
| **Component React** | Idealnya pure function component |
| **Caching** | Pure function mudah di-cache |

> **Catatan:** Aplikasi nyata pasti punya side effects (menyimpan data, nampilin UI). Tujuannya BUKAN menghilangkan semua side effect, melainkan **mengisolasi** mereka di tempat tertentu dan menjaga sebagian besar kode tetap pure.

---

## 6. Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|-----------|--------|--------|
| Memutasi argumen | `arr.sort()` di dalam fungsi | Array asli berubah |
| Mengandalkan state global | Fungsi tergantung variabel global | Test jadi sulit |
| **Referential transparency dilanggar** | `f(2) !== f(2)` karena pakai random | Bug tak terduga |
| Menganggap spread = deep copy | `{...obj}` hanya shallow copy | Nested objek masih referensi |
| Side effect di dalam map | `arr.map(x => { console.log(x); return x*2; })` | Map jadi impure |
| **Over-immutability** | Copy array raksasa tiap operasi | Performa turun |

```javascript
// Contoh mutasi tersembunyi
function updateInventory(inventory, item) {
    inventory[item.id] = item;  // Mutasi objek asli!
    return inventory;
}

// Perbaikan
function updateInventoryPure(inventory, item) {
    return { ...inventory, [item.id]: item };
}
```

---

## 7. Benang Merah

Materi 38 (rekursi: fungsi ideal self-contained) → **Materi 39 (pure function: prinsip fungsi ideal tanpa side effect)** → Materi 40 (modules: mengisolasi kode dalam file terpisah)

Konsep pure function adalah jembatan dari **fungsi sebagai prosedur** menuju **functional programming**. Map/filter/reduce (Materi 18-20) adalah contoh pure function yang sudah Anda gunakan.

---

## 8. Soal

### Soal 1 (Mudah)
Manakah yang pure function?
- A: `function random() { return Math.random(); }`
- B: `function greet(name) { return "Halo " + name; }`
- C: `function show(msg) { alert(msg); }`
- D: `function now() { return Date.now(); }`

<details>
<summary>Jawaban</summary>
B. Input sama → output sama. A dan D non-deterministic. C punya side effect (alert).
</details>

### Soal 2 (Sedang)
Refactor fungsi berikut menjadi pure:
```javascript
let taxRate = 0.1;
function calculateTax(price) {
    return price * taxRate;
}
```

<details>
<summary>Jawaban</summary>
```javascript
function calculateTax(price, taxRate) {
    return price * taxRate;
}
// Atau curried:
const createTaxCalc = (taxRate) => (price) => price * taxRate;
const tax10 = createTaxCalc(0.1);
```
</details>

### Soal 3 (Sulit)
Ada fungsi berikut. Identifikasi semua side effect dan refactor ke pure:
```javascript
const users = [];
function registerUser(name, age) {
    const id = users.length + 1;
    const user = { id, name, age, createdAt: new Date() };
    users.push(user);
    console.log("User registered:", user.name);
    return user;
}
```

<details>
<summary>Jawaban</summary>
Side effects:
1. Memutasi global `users` (push)
2. `console.log` — I/O
3. `new Date()` — non-deterministic
4. `id` bergantung pada `users.length` (state global)

Refactor:
```javascript
// Pure — semua input explicit, output data (no side effects)
function createUser(id, name, age, createdAt) {
    return { id, name, age, createdAt };
}

// Dipisah — side effect dikelola di sini
function registerUserPure(users, { name, age, createdAt }) {
    const id = Math.max(0, ...users.map(u => u.id)) + 1;
    const user = { id, name, age, createdAt: createdAt || new Date() };
    return { users: [...users, user], user };
}
```
</details>
