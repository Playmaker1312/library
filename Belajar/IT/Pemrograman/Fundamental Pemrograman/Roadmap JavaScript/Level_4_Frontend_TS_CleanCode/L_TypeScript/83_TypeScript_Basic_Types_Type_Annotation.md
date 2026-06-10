# 83. TypeScript Basic Types & Type Annotation

**Benang Merah**: Materi 82 menjelaskan **mengapa TS diperlukan**. Sekarang kita pelajari **tipe-tipe dasar** yang menjadi fondasi semua kode TypeScript. Lanjut ke Materi 84 (Interface & Type).

---

## Penjelasan

TypeScript menyediakan sistem tipe yang bisa kita tempelkan ke variabel, parameter fungsi, dan return value. Ini seperti memberi **label spesifikasi** pada setiap bahan bangunan — tidak perlu tebak-tebak ukuran atau jenis material.

### Primitive Types
- **`string`** — teks
- **`number`** — angka (integer & float)
- **`boolean`** — true/false

### Special Types
- **`any`** — matikan type checking (hindari!)
- **`unknown`** — seperti any, tapi harus dicek dulu sebelum dipakai
- **`never`** — tidak pernah terjadi (fungsi throw error / infinite loop)
- **`void`** — fungsi tidak mengembalikan nilai

### Compound Types
- **Array types**: `string[]` atau `Array<number>`
- **Union types**: `string | number` — bisa salah satu

### Type Inference
TS otomatis menebak tipe dari nilai yang diberikan:

```typescript
let nama = "Budi"; // TS infer: string
let umur = 25;     // TS infer: number
```

---

## Fungsi

Memberi **kejelasan** dan **keamanan** pada setiap nilai dalam kode. Compiler bisa langsung mendeteksi jika ada tipe yang tidak cocok.

---

## Code

```typescript
// ========== PRIMITIVE TYPES ==========
const nama: string = "Budi";
const umur: number = 25;
const isActive: boolean = true;

// ========== SPECIAL TYPES ==========
let data: any = "bisa apa saja"; // ❌ hindari
let input: unknown = "cek dulu"; // ✅ aman, harus dicek
function error(): never { throw new Error("gagal"); }
function log(msg: string): void { console.log(msg); }

// ========== ARRAY TYPES ==========
const buah: string[] = ["apel", "mangga"];
const angka: Array<number> = [1, 2, 3];

// ========== UNION TYPES ==========
let id: string | number = "ABC123";
id = 456; // ✅ valid

// ========== TYPE INFERENCE ==========
let kota = "Jakarta"; // TS tahu ini string
// kota = 123; // ❌ Error

// ========== TODO LIST — Semua fungsi di-annotasi ==========
type StatusTodo = "pending" | "done" | "archived";

interface Todo {
  id: number;
  judul: string;
  status: StatusTodo;
}

function tambahTodo(judul: string): Todo {
  return { id: Date.now(), judul, status: "pending" };
}

function selesaiTodo(todos: Todo[], id: number): Todo[] {
  return todos.map(t => t.id === id ? { ...t, status: "done" } : t);
}

function tampilkanTodo(todos: Todo[]): void {
  todos.forEach(t => console.log(`[${t.status}] ${t.judul}`));
}
```

---

## Analogi: Membangun Rumah (Label Spesifikasi Material)

| TypeScript | Analogi Rumah |
|---|---|
| `string` | Label "Paku 5cm" |
| `number` | Label "Besi 12mm" |
| `boolean` | Stiker "ON/OFF" |
| `string[]` | Kardus berisi "Paku 5cm × 100" |
| `string \| number` | "Baut atau Mur" — bisa salah satu |
| `any` | "Bahan apa saja" — berbahaya, tidak jelas |
| Type inference | Tukang lihat bahannya langsung tahu itu paku |
| Union type | Gerbang yang bisa "terbuka" atau "tertutup" |

Bayangkan Anda ke toko material. Setiap barang punya **label spesifikasi**: "Paku beton 5cm", "Besi hollow 4×4", "Cat tembok putih". Anda tidak perlu membuka bungkus atau nebak — label sudah memberi tahu persis apa isinya. Type annotation persis seperti label itu.

---

## Use Case

- Validasi input API (req.body, params)
- State management (status enum)
- Konfigurasi aplikasi
- Semua project TypeScript

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Over-annotation | `const x: number = 5` | Kode bertele-tele (TS sudah infer) |
| Pakai `any` | `function a(x: any)` | Matikan type safety |
| Salah union | `const x: string \| number = true` | Error tipe ketiga |
| Lupa void | `function a() {}` tanpa return type | Sebenarnya `void`, tapi tidak eksplisit |

---

## Benang Merah

Materi 82 (Mengapa TS) → **Materi 83 (Basic Types)** → Materi 84 (Interface & Type Alias)

Basic types adalah **fondasi** TypeScript. Setelah ini kita akan membungkus tipe-tipe ini ke dalam **bentuk yang lebih terstruktur** menggunakan Interface dan Type Alias.

---

## Soal Latihan

### Soal 1 (Mudah)
Buat variabel dengan tipe: `nama` (string), `usia` (number), `hobi` (array of strings), dan `menikah` (boolean).

**Jawaban**:
```typescript
const nama: string = "Sari";
const usia: number = 28;
const hobi: string[] = ["membaca", "coding", "lari"];
const menikah: boolean = false;
```

### Soal 2 (Sedang)
Buat fungsi `hitungDiskon(harga: number, diskon: number): number` yang mengembalikan harga setelah diskon. Gunakan union type untuk parameter diskon yang bisa berupa `number` (persen) atau `"gratis"`.

**Jawaban**:
```typescript
function hitungDiskon(harga: number, diskon: number | "gratis"): number {
  if (diskon === "gratis") return 0;
  return harga - (harga * diskon / 100);
}
```

### Soal 3 (Tantangan)
Buat type `StatusPesanan` yang merupakan union dari `"dipesan" | "diproses" | "dikirim" | "selesai"`. Buat array `pesanan: { id: number, status: StatusPesanan }[]` dan fungsi `filterByStatus` yang mengembalikan pesanan berdasarkan status tertentu.

**Jawaban**:
```typescript
type StatusPesanan = "dipesan" | "diproses" | "dikirim" | "selesai";

interface Pesanan {
  id: number;
  status: StatusPesanan;
}

const pesanan: Pesanan[] = [
  { id: 1, status: "dipesan" },
  { id: 2, status: "dikirim" },
  { id: 3, status: "diproses" },
];

function filterByStatus(list: Pesanan[], status: StatusPesanan): Pesanan[] {
  return list.filter(p => p.status === status);
}

console.log(filterByStatus(pesanan, "dikirim")); // [{ id: 2, status: "dikirim" }]
```
