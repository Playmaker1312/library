# 85. Generics

**Benang Merah**: Materi 84 mengajarkan interface untuk **satu bentuk spesifik**. Tapi bagaimana kalau kita ingin satu fungsi/class yang **bekerja untuk banyak tipe** tanpa pakai `any`? Itulah **Generics**. Lanjut ke Materi 86 (TS di Express).

---

## Penjelasan

**Generics** memungkinkan kita membuat komponen (fungsi, class, interface) yang **bekerja dengan berbagai tipe** tetapi tetap **type-safe** — tidak seperti `any` yang mematikan type checking.

### Masalah Tanpa Generics
```typescript
// ❌ Duplikat — harus bikin untuk setiap tipe
function firstString(arr: string[]): string { return arr[0]; }
function firstNumber(arr: number[]): number { return arr[0]; }

// ❌ Any — tidak type-safe
function firstAny(arr: any[]): any { return arr[0]; }
firstAny([1, 2, 3]).toUpperCase(); // ❌ Runtime error! number ga punya toUpperCase
```

### Solusi dengan Generics
```typescript
function first<T>(arr: T[]): T {
  return arr[0];
}

const a = first([1, 2, 3]);     // T → number, a: number
const b = first(["a", "b"]);    // T → string, b: string
const c = first([true, false]); // T → boolean, c: boolean
```

### Generic pada Fungsi, Interface, Class
```typescript
// Fungsi
function wrapInArray<T>(item: T): T[] { return [item]; }

// Interface
interface Box<T> { value: T; }

// Class
class Storage<T> {
  private data: T[] = [];
  add(item: T): void { this.data.push(item); }
  getAll(): T[] { return this.data; }
}
```

### Constraints (extends)
Membatasi tipe apa yang bisa dipakai:

```typescript
interface HasLength { length: number; }
function logLength<T extends HasLength>(item: T): number {
  return item.length;
}

logLength("hello"); // ✅ string punya length
logLength([1, 2]);  // ✅ array punya length
// logLength(123);  // ❌ number tidak punya length
```

---

## Fungsi

Membuat kode **reusable** untuk berbagai tipe tanpa kehilangan **type safety**. Satu fungsi, banyak tipe.

---

## Code

```typescript
// ========== GENERIC FUNCTION ==========
function identitas<T>(arg: T): T {
  return arg;
}

console.log(identitas<string>("Halo")); // Halo
console.log(identitas<number>(42));     // 42

// ========== GENERIC ARRAY — Refactor dari Level 2 ==========
function filterArray<T>(arr: T[], predicate: (item: T) => boolean): T[] {
  return arr.filter(predicate);
}

function mapArray<T, U>(arr: T[], transform: (item: T) => U): U[] {
  return arr.map(transform);
}

function reduceArray<T>(arr: T[], reducer: (acc: T, cur: T) => T, initial: T): T {
  return arr.reduce(reducer, initial);
}

// Contoh pakai
const angka = [1, 2, 3, 4, 5];
const genap = filterArray(angka, n => n % 2 === 0); // [2, 4]
const dikali2 = mapArray(angka, n => n * 2);         // [2, 4, 6, 8, 10]
const jumlah = reduceArray(angka, (a, b) => a + b, 0); // 15

// ========== GENERIC INTERFACE ==========
interface ApiResponse<T> {
  status: "success" | "error";
  data: T;
  message?: string;
}

interface User {
  id: number;
  nama: string;
}

type Product = {
  id: number;
  nama: string;
  harga: number;
};

const userResponse: ApiResponse<User[]> = {
  status: "success",
  data: [{ id: 1, nama: "Budi" }]
};

const productResponse: ApiResponse<Product> = {
  status: "success",
  data: { id: 1, nama: "Kopi", harga: 25000 }
};

// ========== GENERIC CLASS ==========
class Queue<T> {
  private items: T[] = [];

  enqueue(item: T): void {
    this.items.push(item);
  }

  dequeue(): T | undefined {
    return this.items.shift();
  }

  peek(): T | undefined {
    return this.items[0];
  }

  get length(): number {
    return this.items.length;
  }
}

const antrean = new Queue<string>();
antrean.enqueue("Budi");
antrean.enqueue("Sari");
console.log(antrean.dequeue()); // Budi

// ========== CONSTRAINTS ==========
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user = { nama: "Budi", umur: 25, email: "budi@mail.com" };
console.log(getProperty(user, "nama"));  // Budi
console.log(getProperty(user, "umur"));  // 25
// getProperty(user, "alamat"); // ❌ Error
```

---

## Analogi: Membangun Rumah (Cetakan Adjustable)

| TypeScript | Analogi Rumah |
|---|---|
| `<T>` (type parameter) | Cetakan adjustable — bisa diatur ukurannya |
| Generic function | Satu mesin cetak bata — bisa bata besar/kecil |
| Generic interface | "Lubang untuk pipa" — diameter bisa disesuaikan |
| Constraint `extends` | "Cetakan untuk material yang bisa dicetak saja" |
| `T[]` | "Satu palet berisi T" |
| `any` (tanpa generic) | "Bahan apa saja" — tukang bingung |

Bayangkan satu **mesin cetak bata** yang bisa diatur ukurannya. Mau bata besar, bata kecil, genteng, atau paving block — **satu mesin, banyak hasil berbeda**. Itulah generics. Tanpa generics, Anda harus punya mesin terpisah untuk setiap ukuran (duplikasi) atau pakai mesin "bahan apa saja" yang tidak jelas hasilnya (`any`).

---

## Use Case

- **API response wrapper** — `ApiResponse<T>` untuk semua endpoint
- **Utility functions** — filter, map, reduce yang type-safe
- **State management** — `Store<T>` untuk global state
- **Repository pattern** — `Repository<T>` untuk database
- **Form handling** — `FormState<T>` untuk validasi

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Pakai `any` instead of generic | `function a(x: any)` | Kehilangan type safety |
| Lupa constraint | `<T>` padahal butuh `.length` | Error akses properti |
| Over-generic | `<T, U, V, W>` untuk fungsi sederhana | Kode susah dibaca |
| Type parameter tidak dipakai | `<T>` tapi T tidak ada di parameter | Warning/mubazir |
| Default type salah | `<T = string>` padahal default harus number | Bug tersembunyi |

---

## Benang Merah

Materi 84 (Interface) → **Materi 85 (Generics)** → Materi 86 (TS di Express)

Interface memberi **bentuk**, Generics memberi **fleksibilitas**. Kombinasi keduanya sangat powerful. Di Materi 86, kita akan pakai **generics** untuk typing Express Request/Response.

---

## Soal Latihan

### Soal 1 (Mudah)
Buat fungsi generic `reverseArray<T>(arr: T[]): T[]` yang mengembalikan array dalam urutan terbalik.

**Jawaban**:
```typescript
function reverseArray<T>(arr: T[]): T[] {
  return [...arr].reverse();
}

console.log(reverseArray([1, 2, 3]));      // [3, 2, 1]
console.log(reverseArray(["a", "b", "c"])); // ["c", "b", "a"]
```

### Soal 2 (Sedang)
Buat generic interface `Pair<T, U>` dengan properti `first: T` dan `second: U`. Buat fungsi `swapPair` yang menukar posisi first dan second.

**Jawaban**:
```typescript
interface Pair<T, U> {
  first: T;
  second: U;
}

function swapPair<T, U>(pair: Pair<T, U>): Pair<U, T> {
  return { first: pair.second, second: pair.first };
}

const pair: Pair<string, number> = { first: "umur", second: 25 };
console.log(swapPair(pair)); // { first: 25, second: "umur" }
```

### Soal 3 (Tantangan)
Buat generic class `Stack<T>` dengan metode `push(item: T)`, `pop(): T | undefined`, `peek(): T | undefined`, dan properti `readonly size: number`. Gunakan constraint untuk memastikan method `max()` hanya tersedia jika T adalah number.

**Jawaban**:
```typescript
class Stack<T> {
  private items: T[] = [];

  push(item: T): void {
    this.items.push(item);
  }

  pop(): T | undefined {
    return this.items.pop();
  }

  peek(): T | undefined {
    return this.items[this.items.length - 1];
  }

  get size(): number {
    return this.items.length;
  }
}

class NumberStack extends Stack<number> {
  max(): number | undefined {
    if (this.size === 0) return undefined;
    return Math.max(...(this as any).items);
  }
}

const stack = new Stack<string>();
stack.push("a");
stack.push("b");
console.log(stack.pop()); // b

const numStack = new NumberStack();
numStack.push(10);
numStack.push(5);
numStack.push(20);
console.log(numStack.max()); // 20
```
