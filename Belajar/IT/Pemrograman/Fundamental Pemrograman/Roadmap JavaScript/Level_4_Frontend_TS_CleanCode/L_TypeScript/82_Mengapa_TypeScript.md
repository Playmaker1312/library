# 82. Mengapa TypeScript — Masalah yang Diselesaikan

**Benang Merah**: Selama Level 1-3 kita menggunakan **JavaScript murni (dynamic typing)**. Semakin besar project, semakin sering kita menemui **bug tipe** yang seharusnya bisa dicegah. TypeScript adalah solusinya.

---

## Penjelasan

TypeScript adalah **superset JavaScript** yang menambahkan **type checking static**. Artinya: Anda menulis kode seperti biasa, tapi bisa menambahkan **tipe** (string, number, object, dll) ke variabel, parameter, dan return value. Kesalahan tipe akan **terdeteksi saat development**, bukan saat runtime.

```javascript
// JavaScript — error baru ketahuan saat dijalanin
function jumlah(a, b) {
  return a + b;
}

jumlah(5, "10"); // "510" — bukan error, tapi SALAH!
// Tidak ada yang memberitahu Anda sampai program jalan
```

```typescript
// TypeScript — error ketahuan saat ngetik
function jumlah(a: number, b: number): number {
  return a + b;
}

jumlah(5, "10"); // ❌ ERROR: "10" bukan number
// Langsung muncul garis merah di editor!
```

---

## Fungsi

Menangkap **kesalahan tipe** sebelum kode dijalankan. Ini mengubah debugging dari **"cari error di runtime"** menjadi **"cari error di editor"** — jauh lebih cepat dan murah.

---

## Cara Pengimplementasian

### 1. Install TypeScript
```bash
npm install -g typescript   # global
npm install --save-dev typescript  # atau di project
```

### 2. File .ts vs .js
```typescript
// index.ts
const nama: string = "Budi";
const umur: number = 25;
const isAktif: boolean = true;

function sapa(nama: string): string {
  return `Halo, ${nama}!`;
}
```

### 3. Compile ke JavaScript
```bash
tsc index.ts      # hasil: index.js
tsc --init        # buat tsconfig.json
```

### 4. Type Inference — TS bisa nebak tipe
```typescript
let nama = "Budi";  // TS otomatis tahu ini string
nama = 123;         // ❌ Error! udah didekteksi sebagai string

// Ini salah satu fitur terbaik: tanpa annotasi pun, TS tetap melindungi!
```

### 5. Contoh: Menangkap Bug Sebelum Jalan
```typescript
// Bug 1: typo nama properti
const user = { name: "Budi", age: 25 };
console.log(user.nama); // ❌ Error: 'nama' ga ada di object

// Bug 2: salah tipe parameter
function double(n: number): number {
  return n * 2;
}
double("5"); // ❌ Error

// Bug 3: lupa handle null
function getLength(s: string): number {
  return s.length;
}
getLength(null); // ❌ Error: null not assignable to string
```

---

## Analogi: Membangun Rumah (Inspektur Bangunan)

| TypeScript | Inspektur Bangunan |
|---|---|
| Type annotation | Spesifikasi material di blueprint |
| Type checking | Inspektur cek sebelum dibangun |
| Compile error | Inspektur bilang "ini tidak sesuai spesifikasi" |
| `any` | "Gunakan bahan apa saja" — berbahaya |
| `.ts` → `.js` | Blueprint → bangunan jadi |
| JavaScript runtime | Bangunan yang sudah berdiri |

Bayangkan Anda membangun rumah. Anda bisa langsung bangun tanpa blueprint (JavaScript) — cepat, tapi risiko tembok miring atau pintu tidak muat. Atau Anda bikin blueprint dulu dengan **spesifikasi jelas** (TypeScript), dan ada **inspektur** yang periksa sebelum dibangun. Inspektur tidak membangun rumah — dia hanya memastikan tidak ada kesalahan di blueprint. Setelah dicek, blueprint diubah menjadi bangunan nyata (compile ke JS).

---

## Dipakai Untuk Apa

- **Project skala besar** — kode ribuan file, tim banyak orang
- **Library / framework** — Vue, React, Angular, Express (semua pake TS)
- **API backend** — type safety dari database sampai response
- **Codebase yang panjang umur** — lebih mudah di-refactor
- **Hampir semua project production** di industri

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Over-annotasi | `const x: number = 5` — padahal TS bisa infer | Kode berlebihan |
| Pakai `any` terus | `function a(x: any)` | Matikan TypeScript entirely |
| Abaikan error TS | Paksa compile meskipun error | Sama aja pake JS |
| Terlalu strict di awal | Setup strict mode bikin frustrasi | Matikan dulu, hidupkan bertahap |

---

## Hubungan dengan Materi Sebelumnya

TypeScript adalah **peningkatan kualitas** untuk semua kode yang sudah kita tulis:
- Materi 9-19 (JavaScript dasar) → Semua bisa diketik dengan TS
- Materi 24-31 (Array & Object) → Interface untuk struktur data
- Materi 32-39 (Fungsi) → Typing parameter dan return
- Materi 43 (Class) → Class dengan access modifier TS
- Materi 57-58 (Express API) → Backend type-safe
- Materi 80 (Vue) → Frontend type-safe

---

## Soal Latihan

### Soal 1 (Mudah)
Ubah kode JavaScript berikut ke TypeScript dengan menambahkan tipe:
```javascript
function greet(name) {
  return "Hello, " + name;
}
const age = 25;
const isStudent = true;
```

**Jawaban**:
```typescript
function greet(name: string): string {
  return "Hello, " + name;
}
const age: number = 25;
const isStudent: boolean = true;
```

### Soal 2 (Sedang)
Cari dan perbaiki 3 bug di kode TypeScript berikut:
```typescript
function calculateTotal(price, tax) {
  return price + tax;
}

let itemPrice = "10000";
let taxAmount = 500;
let total = calculateTotal(itemPrice, taxAmount);
console.log(total);
```

**Jawaban**:
```typescript
// Bug 1: price dan tax tidak punya tipe
// Bug 2: itemPrice seharusnya number, bukan string
// Bug 3: return type tidak didefinisikan

function calculateTotal(price: number, tax: number): number {
  return price + tax;
}

let itemPrice: number = 10000;   // string → number
let taxAmount: number = 500;
let total: number = calculateTotal(itemPrice, taxAmount);
console.log(total); // 10500
```

### Soal 3 (Tantangan)
Buat interface untuk object `Product` dengan properti: `id` (number), `name` (string), `price` (number), `category?` (optional string). Lalu buat fungsi `formatProduct(product: Product): string` yang mengembalikan string `"Nama — Rp Harga"`.

**Jawaban**:
```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  category?: string;  // optional
}

function formatProduct(product: Product): string {
  return `${product.name} — Rp ${product.price.toLocaleString()}`;
}

const product1: Product = {
  id: 1,
  name: "Kopi Arabika",
  price: 25000
};

const product2: Product = {
  id: 2,
  name: "Teh Hijau",
  price: 15000,
  category: "Minuman"
};

console.log(formatProduct(product1)); // "Kopi Arabika — Rp 25.000"
console.log(formatProduct(product2)); // "Teh Hijau — Rp 15.000"
```
