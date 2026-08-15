# Roadmap TypeScript: Step-by-Step Membangun Aplikasi Nyata

## Filosofi Roadmap Ini

> **"TypeScript bukan JavaScript yang dipersulit — TypeScript adalah JavaScript yang diberi keamanan tipe sehingga bug tertangkap saat coding, bukan saat production"** — setiap konsep yang dipelajari ada alasannya, bukan sekadar hafal syntax.

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: sistem manajemen perpustakaan dari CLI → web app → API → full-stack application
- **Type Safety dari Hari Pertama**: bukan topik lanjutan, tapi mindset yang dibangun sejak baris pertama
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **TypeScript Modern**: fokus pada TypeScript 5.x features, bukan cara lama yang sudah deprecated
- **Mengapa sebelum Bagaimana**: pahami alasan di balik setiap keputusan desain TypeScript

### Prasyarat Sebelum Memulai

text

```
Sebelum roadmap ini, pastikan sudah memahami:
├── JavaScript dasar hingga menengah (variabel, array, fungsi, object)
├── ES6+ features (arrow function, destructuring, spread, template literal)
├── Promise dan async/await
├── Node.js dasar (bisa jalankan file .js di terminal)
├── npm atau pnpm sebagai package manager
├── HTML & CSS dasar
├── Command line / terminal dasar
└── Git version control dasar
```

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: "TypeScript Pertama" — setup, tipe dasar, compile, CLI perpustakaan
    ↓ (enhance)
Level 2: + Object, Interface, Type Alias → Struktur data buku yang typed
    ↓ (enhance)
Level 3: + Class, OOP, Generics → Sistem perpustakaan berorientasi objek
    ↓ (enhance)
Level 4: + Advanced Types, Utility Types → Type system yang ekspresif
    ↓ (enhance)
Level 5: + Node.js, Express API → REST API perpustakaan yang typed
    ↓ (enhance)
Level 6: + Testing, Design Patterns → Production-ready backend
    ↓ (enhance)
Level 7: + React/Next.js, Full-Stack → Aplikasi perpustakaan lengkap
```

---

## 🟢 LEVEL 1: FONDASI TYPESCRIPT (Minggu 1-3)

> **Tema**: _"Dari JavaScript biasa ke TypeScript yang type-safe"_  
> **Benang Merah**: Mengapa TypeScript → Setup → Tipe dasar → Compile → CLI pertama  
> **Output**: Program CLI perpustakaan yang berjalan dengan TypeScript

---

### A. Memahami TypeScript dan Cara Kerjanya

> 💡 **Mengapa dimulai di sini?** Sebelum menulis kode, pahami dulu _mengapa_ TypeScript ada dan _bagaimana_ ia bekerja. Ini mencegah kebingungan "kenapa harus compile?" atau "apa bedanya `any` dengan `unknown`?"

text

```
Benang Merah Bagian A:
JavaScript sudah dipahami (prasyarat) →
TypeScript: superset JavaScript yang menambahkan static typing →
Browser/Node tidak mengerti TypeScript → harus di-compile ke JavaScript →
Type checker: menangkap bug SEBELUM runtime →
Setup environment → file pertama → compile → jalankan
```

#### [[1. TypeScript adalah JavaScript dengan Tipe — Bukan Bahasa Baru]]

- TypeScript **bukan** bahasa yang berbeda — ia adalah JavaScript dengan anotasi tipe
- Semua kode JavaScript valid adalah kode TypeScript yang valid
- **Yang TypeScript tambahkan di atas JavaScript:**

text

```
JavaScript                          TypeScript
─────────────────────────────────   ──────────────────────────────────────
function tambah(a, b) {          →  function tambah(a: number, b: number): number {
  return a + b;                        return a + b;
}                                   }
tambah("5", 3) // "53" ← bug!      tambah("5", 3) // ❌ Error saat compile!

let buku = {};                   →  interface Buku { judul: string; stok: number; }
buku.judl = "Clean Code" // typo!  let buku: Buku = { judul: "Clean Code", stok: 5 };
                                    buku.judl = "Clean Code" // ❌ Error: 'judl' tidak ada!
```

- **Alur kerja TypeScript:**

text

```
Kamu tulis         TypeScript compile      Browser/Node jalankan
.ts file      →    tsc / transpiler    →   .js file
(TypeScript)       (type checking)         (JavaScript murni)

Jika ada error tipe → compile GAGAL → bug tertangkap sebelum runtime
```

- _Langkah konkret_: Lihat [typescriptlang.org/play](https://typescriptlang.org/play) — tulis kode TypeScript, lihat output JavaScript di sebelah kanan

#### [[2. Mengapa TypeScript? — Masalah yang Dipecahkan]]

TypeScript

```
// ❌ JavaScript: bug hanya ketahuan saat runtime (di production!)
function hitungDenda(hariTerlambat, dendaPerHari) {
    return hariTerlambat * dendaPerHari;
}

hitungDenda("7", 1000);   // "7000" ← string, bukan number! Bug!
hitungDenda(undefined, 1000); // NaN ← crash tersembunyi!

// ✅ TypeScript: bug ketahuan saat nulis kode (di editor!)
function hitungDenda(hariTerlambat: number, dendaPerHari: number): number {
    return hariTerlambat * dendaPerHari;
}

hitungDenda("7", 1000);       // ❌ Error: Argument of type 'string' is not assignable to 'number'
hitungDenda(undefined, 1000); // ❌ Error: Argument of type 'undefined' is not assignable to 'number'
hitungDenda(7, 1000);         // ✅ 7000 — benar!
```

- **Keuntungan nyata TypeScript:**
    - Autocomplete yang akurat di editor (VS Code)
    - Refactoring yang aman — rename variabel, TypeScript tahu semua yang harus diubah
    - Dokumentasi yang hidup — tipe adalah dokumentasi yang selalu update
    - Bug tertangkap sebelum deploy ke production

---

### B. Setup dan Konfigurasi

> 💡 **Benang Merah ke A**: Paham mengapa TypeScript, sekarang setup environment dan buat project pertama.

#### [[3. Instalasi dan Setup Project Perpustakaan]]

Bash

```
# Pastikan Node.js sudah terinstall
node --version    # minimal Node.js 18
npm --version

# Install TypeScript secara global (untuk tsc CLI)
npm install -g typescript

# Verifikasi
tsc --version     # TypeScript 5.x.x

# Buat project baru
mkdir perpustakaan-ts
cd perpustakaan-ts
npm init -y

# Install TypeScript sebagai dev dependency (rekomendasi untuk project)
npm install --save-dev typescript
npm install --save-dev @types/node    # type definitions untuk Node.js

# Install ts-node: jalankan TypeScript langsung tanpa compile manual
npm install --save-dev ts-node

# Install nodemon + ts-node untuk auto-reload saat development
npm install --save-dev nodemon
```

#### [[4. tsconfig.json — Konfigurasi Compiler TypeScript]]

Bash

```
# Generate tsconfig.json dengan nilai default
npx tsc --init
```

JSON

```
// tsconfig.json — konfigurasi yang direkomendasikan untuk project baru

{
  "compilerOptions": {
    // ─── Target dan Module ─────────────────────────────────────────────
    "target": "ES2022",           // JavaScript versi yang dihasilkan
    "module": "CommonJS",         // sistem module (CommonJS untuk Node.js)
    "lib": ["ES2022"],            // library built-in yang tersedia

    // ─── Output ────────────────────────────────────────────────────────
    "outDir": "./dist",           // folder output JavaScript
    "rootDir": "./src",           // folder source TypeScript

    // ─── Type Checking — semakin strict, semakin aman ──────────────────
    "strict": true,               // aktifkan SEMUA strict checks (recommended!)
    // strict = true mengaktifkan semua ini:
    // "strictNullChecks": true,      — null/undefined harus eksplisit
    // "strictFunctionTypes": true,   — function type checking yang ketat
    // "noImplicitAny": true,         — tidak boleh ada implicit 'any'
    // "strictPropertyInitialization": true — property class harus diinisialisasi

    "noUnusedLocals": true,       // error jika ada variabel tidak terpakai
    "noUnusedParameters": true,   // error jika ada parameter tidak terpakai
    "noImplicitReturns": true,    // semua code path harus return value
    "noFallthroughCasesInSwitch": true, // switch case harus ada break/return

    // ─── Module Resolution ─────────────────────────────────────────────
    "moduleResolution": "node",   // cara TypeScript resolve import
    "esModuleInterop": true,      // kompatibilitas import CommonJS/ESM
    "resolveJsonModule": true,    // bisa import file JSON
    "forceConsistentCasingInFileNames": true,

    // ─── Source Maps — untuk debugging ────────────────────────────────
    "sourceMap": true,            // generate .map file untuk debugging

    // ─── Paths — alias import ─────────────────────────────────────────
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]            // import dari '@/models/Buku' → 'src/models/Buku'
    }
  },
  "include": ["src/**/*"],        // compile semua file di folder src
  "exclude": ["node_modules", "dist", "**/*.test.ts"]
}
```

JSON

```
// package.json — tambahkan scripts

{
  "scripts": {
    "build": "tsc",
    "build:watch": "tsc --watch",
    "start": "node dist/index.js",
    "dev": "nodemon --exec ts-node src/index.ts",
    "type-check": "tsc --noEmit"
  }
}
```

- _Langkah konkret_: Buat `src/index.ts` dengan `console.log("Hello TypeScript!")`, jalankan `npx ts-node src/index.ts` — pastikan muncul output

---

### C. Tipe Dasar — Fondasi Type System

> 💡 **Benang Merah ke B**: Project sudah berjalan. Sekarang pelajari sistem tipe TypeScript — ini adalah fondasi segalanya.

text

```
Benang Merah Bagian C:
TypeScript berjalan (B) →
Primitive types: number, string, boolean, null, undefined →
Type annotation: cara memberitahu TypeScript tipe sebuah nilai →
Type inference: TypeScript cukup cerdas untuk menebak tipe →
Union types: nilai bisa salah satu dari beberapa tipe →
Literal types: nilai spesifik sebagai tipe
```

#### [[5. Primitive Types — Tipe Paling Dasar]]

TypeScript

```
// src/types/dasar.ts

// ─── Type Annotation ───────────────────────────────────────────────────────
// Eksplisit: kamu memberitahu TypeScript tipenya
let judul: string = "Clean Code";
let harga: number = 150000;
let tersedia: boolean = true;
let pengarang: string | null = null;   // bisa string atau null

// ─── Type Inference ────────────────────────────────────────────────────────
// TypeScript cukup cerdas untuk menebak tipe dari nilai awal
// Tidak perlu tulis tipe jika nilainya jelas
let judulBuku = "Clean Code";    // TypeScript tahu: string
let hargaBuku = 150000;          // TypeScript tahu: number
let stokBuku  = 5;               // TypeScript tahu: number

// ❌ Ini error karena TypeScript sudah tahu judulBuku adalah string
judulBuku = 123; // Error: Type 'number' is not assignable to type 'string'

// ─── Primitive Types ───────────────────────────────────────────────────────
const nama: string    = "Perpustakaan Kota";
const tahun: number   = 2024;
const aktif: boolean  = true;
const kosong: null    = null;
const belumDiisi: undefined = undefined;

// Perbedaan null vs undefined:
// null: sengaja dikosongkan ("tidak ada nilai")
// undefined: belum pernah diberi nilai

// ─── Special Types ─────────────────────────────────────────────────────────

// any: nonaktifkan type checking — HINDARI sebisa mungkin!
let apaSaja: any = "teks";
apaSaja = 123;          // tidak ada error — tapi kamu kehilangan manfaat TypeScript
apaSaja = true;         // masih tidak ada error
apaSaja.metodeYangTidakAda(); // tidak ada error — bahaya!

// unknown: lebih aman dari any — harus dicek tipenya sebelum digunakan
let nilaiUnknown: unknown = "mungkin string";
// nilaiUnknown.toUpperCase(); // ❌ Error: harus cek tipe dulu!
if (typeof nilaiUnknown === "string") {
    nilaiUnknown.toUpperCase(); // ✅ aman setelah dicek
}

// never: fungsi yang tidak pernah return (throw atau infinite loop)
function lemparError(pesan: string): never {
    throw new Error(pesan); // tidak pernah return
}

// void: fungsi yang tidak return nilai berarti
function cetakJudul(judul: string): void {
    console.log(judul);
    // tidak return apapun (atau return undefined)
}
```

#### [[6. Union Types dan Literal Types — Tipe yang Lebih Ekspresif]]

TypeScript

```
// src/types/union-literal.ts

// ─── Union Types: nilai bisa salah satu dari beberapa tipe ────────────────
let idBuku: number | string;
idBuku = 1;          // ✅
idBuku = "ISBN-001"; // ✅
idBuku = true;       // ❌ Error: tidak dalam union

// Union dengan null — sangat umum!
let pengarang: string | null = null;
pengarang = "Robert Martin"; // ✅
// Sebelum pakai, harus cek null:
if (pengarang !== null) {
    console.log(pengarang.toUpperCase()); // ✅ aman
}
// Atau dengan optional chaining:
console.log(pengarang?.toUpperCase()); // ✅ undefined jika null

// ─── Literal Types: nilai spesifik sebagai tipe ───────────────────────────
// Bukan hanya "string" — tapi string dengan nilai tertentu
let kategori: "Fiksi" | "Non-Fiksi" | "Sains" | "Teknologi" | "Sejarah";
kategori = "Fiksi";       // ✅
kategori = "Teknologi";   // ✅
kategori = "Komik";       // ❌ Error: "Komik" bukan nilai yang valid!

let statusPeminjaman: "dipinjam" | "dikembalikan" | "terlambat";
statusPeminjaman = "dipinjam";     // ✅
statusPeminjaman = "hilang";       // ❌ Error!

// Literal type untuk number
let prioritas: 1 | 2 | 3 = 1;
prioritas = 2;  // ✅
prioritas = 5;  // ❌ Error!

// ─── Template Literal Types (TypeScript 4.1+) ─────────────────────────────
type EventBuku = `buku:${"ditambah" | "diupdate" | "dihapus"}`;
// Menghasilkan: "buku:ditambah" | "buku:diupdate" | "buku:dihapus"

let event: EventBuku = "buku:ditambah";  // ✅
let event2: EventBuku = "buku:rusak";    // ❌ Error!

// ─── Type Narrowing — mempersempit tipe dalam kondisi ─────────────────────
function prosesId(id: number | string): string {
    if (typeof id === "number") {
        // Di sini TypeScript tahu id adalah number
        return `ID-${id.toFixed(0)}`;
    }
    // Di sini TypeScript tahu id adalah string
    return id.toUpperCase();
}
```

#### [[7. Array dan Tuple — Koleksi Data yang Typed]]

TypeScript

```
// src/types/koleksi.ts

// ─── Array ────────────────────────────────────────────────────────────────
// Cara 1: Type[]
const judulBuku: string[] = ["Clean Code", "The Pragmatic Programmer"];
const hargaBuku: number[] = [150000, 175000, 200000];

// Cara 2: Array<Type> — sama persis
const stokBuku: Array<number> = [5, 3, 0];

// Array readonly: tidak bisa dimodifikasi
const kategoriTetap: readonly string[] = ["Fiksi", "Non-Fiksi", "Sains"];
// kategoriTetap.push("Baru"); // ❌ Error: tidak bisa modifikasi readonly array

// Array of objects
const katalog: Array<{ id: number; judul: string; stok: number }> = [
    { id: 1, judul: "Clean Code", stok: 5 },
    { id: 2, judul: "Laskar Pelangi", stok: 3 },
];

// ─── Tuple — array dengan panjang dan tipe yang fixed ─────────────────────
// Berbeda dengan array: posisi dan tipe setiap elemen sudah ditentukan
type InfoBuku = [number, string, boolean]; // [id, judul, tersedia]

const buku1: InfoBuku = [1, "Clean Code", true];   // ✅
const buku2: InfoBuku = ["Clean Code", 1, true];   // ❌ Error: urutan salah!
const buku3: InfoBuku = [1, "Clean Code"];          // ❌ Error: kurang elemen!

// Destructuring tuple
const [id, judul, tersedia] = buku1;
console.log(id);       // 1 — TypeScript tahu: number
console.log(judul);    // "Clean Code" — TypeScript tahu: string
console.log(tersedia); // true — TypeScript tahu: boolean

// Named tuple (TypeScript 4.0+) — lebih readable
type KoordinatPeta = [latitude: number, longitude: number];
const lokasi: KoordinatPeta = [-6.2088, 106.8456];

// Tuple dengan rest elements
type LogEntry = [Date, string, ...string[]]; // tanggal, level, pesan-pesan
const log: LogEntry = [new Date(), "ERROR", "File tidak ditemukan", "Coba lagi"];
```

---

### 🏗️ Checkpoint Level 1

text

```
✅ Checklist sebelum lanjut ke Level 2:

PEMAHAMAN:
├── Bisa jelaskan perbedaan TypeScript vs JavaScript
├── Bisa jelaskan perbedaan type annotation vs type inference
├── Bisa jelaskan perbedaan any vs unknown dan mengapa any berbahaya
├── Bisa jelaskan kapan pakai union type vs literal type
└── Bisa jelaskan perbedaan array vs tuple

PROYEK: CLI Perpustakaan (src/index.ts)
├── tsconfig.json dengan strict: true
├── Variabel data perpustakaan dengan tipe yang benar
├── Fungsi hitungDenda(hari: number, tarif: number): number
├── Fungsi formatHarga(harga: number): string
├── Array katalog buku dengan tipe yang tepat
└── npm run dev berjalan tanpa error TypeScript

KEBIASAAN:
├── strict: true selalu aktif — tidak ada kompromi
├── Hindari any — pakai unknown jika perlu
├── Biarkan TypeScript inference bekerja (jangan annotasi berlebihan)
└── Baca error TypeScript dengan tenang — pesannya informatif

Git: feat: setup TypeScript project with strict config and basic types
```

---

## 🔵 LEVEL 2: OBJECT, INTERFACE, DAN TYPE ALIAS (Minggu 3-6)

> **Tema**: _"Dari tipe primitif ke struktur data yang kompleks dan reusable"_  
> **Benang Merah**: Tipe primitif (Level 1) → Object types → Interface untuk kontrak → Type alias untuk reuse → Union yang kompleks  
> **Output**: Sistem data buku yang fully typed dengan interface dan type alias

---

### D. Object Types — Mendeskripsikan Struktur Object

> 💡 **Mengapa ini penting?** Hampir semua data di aplikasi nyata adalah object. TypeScript memiliki tiga cara untuk mendeskripsikan struktur object: inline, interface, dan type alias. Memilih yang tepat membuat kode lebih mudah dipahami.

text

```
Benang Merah Bagian D:
Array dan tipe primitif (Level 1) →
Object: kumpulan property dengan nama dan tipe →
Interface: cara mendefinisikan "bentuk" object yang reusable →
Type alias: nama untuk tipe yang kompleks →
Optional property: property yang tidak wajib ada →
Readonly property: property yang tidak bisa diubah
```

#### [[8. Interface — Kontrak Struktur Object]]

TypeScript

```
// src/interfaces/index.ts

// ─── Interface dasar ───────────────────────────────────────────────────────
interface Buku {
    id: number;
    judul: string;
    pengarang: string;
    isbn: string;
    tahun: number;
    harga: number;
    stok: number;
    kategori: string;
    deskripsi?: string;        // ? = optional: boleh ada, boleh tidak
    readonly createdAt: Date;  // readonly: tidak bisa diubah setelah dibuat
}

// Penggunaan:
const buku: Buku = {
    id: 1,
    judul: "Clean Code",
    pengarang: "Robert Martin",
    isbn: "9780132350884",
    tahun: 2008,
    harga: 150000,
    stok: 5,
    kategori: "Teknologi",
    // deskripsi: tidak wajib
    createdAt: new Date(),
};

// buku.createdAt = new Date(); // ❌ Error: readonly tidak bisa diubah!
buku.stok = 4;                  // ✅ bisa diubah

// ─── Interface Extending — mewarisi interface lain ────────────────────────
interface BukuDenganRating extends Buku {
    rating: number;    // 1-5
    jumlahUlasan: number;
}

// BukuDenganRating memiliki semua property Buku + rating + jumlahUlasan
const bukuRated: BukuDenganRating = {
    ...buku,           // spread semua property Buku
    rating: 4.8,
    jumlahUlasan: 234,
};

// ─── Interface untuk function ─────────────────────────────────────────────
interface FungsiCari {
    (keyword: string, kategori?: string): Buku[];
}

const cariBuku: FungsiCari = (keyword, kategori) => {
    // implementasi
    return [];
};

// ─── Interface dengan index signature ─────────────────────────────────────
// Untuk object dengan key yang dinamis
interface KatalogPerKategori {
    [kategori: string]: Buku[];
}

const katalog: KatalogPerKategori = {
    "Teknologi": [buku],
    "Fiksi": [],
};

// ─── Interface merging — TypeScript menggabungkan interface dengan nama sama
interface Anggota {
    id: number;
    nama: string;
}

interface Anggota {
    email: string;  // ditambahkan ke interface Anggota yang sudah ada
}

// Hasil: Anggota memiliki id, nama, DAN email
const anggota: Anggota = { id: 1, nama: "Budi", email: "budi@email.com" };
```

#### [[9. Type Alias — Nama untuk Tipe yang Kompleks]]

TypeScript

```
// src/types/aliases.ts

// ─── Type alias dasar ─────────────────────────────────────────────────────
type ID = number;
type NamaBuku = string;
type HargaRupiah = number;
type Stok = number;

// Sekarang kode lebih readable:
function cariById(id: ID): Buku | null { return null; }
function formatHarga(harga: HargaRupiah): string {
    return `Rp ${harga.toLocaleString("id-ID")}`;
}

// ─── Type alias untuk union yang sering dipakai ───────────────────────────
type Kategori = "Fiksi" | "Non-Fiksi" | "Sains" | "Teknologi" | "Sejarah" | "Umum";
type StatusPeminjaman = "dipinjam" | "dikembalikan" | "terlambat" | "hilang";
type RolePengguna = "admin" | "pustakawan" | "anggota";

// Penggunaan:
interface Peminjaman {
    id: number;
    bukuId: ID;
    anggotaId: ID;
    tanggalPinjam: Date;
    batasKembali: Date;
    tanggalKembali: Date | null;
    status: StatusPeminjaman;   // hanya nilai yang valid!
    denda: HargaRupiah;
}

// ─── Type alias vs Interface: kapan pakai yang mana? ─────────────────────
//
// Gunakan INTERFACE untuk:
// ✅ Mendefinisikan "bentuk" object atau class
// ✅ Saat ingin extend (inheritance)
// ✅ Saat butuh interface merging (library)
//
// Gunakan TYPE ALIAS untuk:
// ✅ Union types: type Status = "aktif" | "nonaktif"
// ✅ Intersection types: type Admin = User & AdminPermission
// ✅ Mapped types, conditional types (Level 4)
// ✅ Tipe primitif yang diberi nama
//
// Aturan praktis: untuk object shape → interface, untuk lainnya → type

// ─── Intersection Types: gabungkan beberapa tipe ─────────────────────────
interface HasTimestamps {
    createdAt: Date;
    updatedAt: Date;
}

interface HasSoftDelete {
    deletedAt: Date | null;
}

// Gabungkan: BukuLengkap memiliki semua dari Buku + HasTimestamps + HasSoftDelete
type BukuLengkap = Buku & HasTimestamps & HasSoftDelete;

const bukuLengkap: BukuLengkap = {
    ...buku,
    updatedAt: new Date(),
    deletedAt: null,
};
```

#### [[10. Optional Chaining dan Nullish Coalescing — Handle Nilai Null dengan Aman]]

TypeScript

```
// src/utils/null-safety.ts

// Contoh: data dari database bisa null
interface AnggotaLengkap {
    id: number;
    nama: string;
    alamat?: {           // opsional
        jalan: string;
        kota: string;
        kodePos?: string; // opsional dalam opsional
    };
    preferensi?: {
        kategori?: Kategori;
        notifikasiEmail?: boolean;
    };
}

const anggota: AnggotaLengkap = {
    id: 1,
    nama: "Budi Santoso",
    // alamat tidak diisi
};

// ❌ Cara lama — verbose dan mudah lupa cek
if (anggota.alamat !== undefined && anggota.alamat.kota !== undefined) {
    console.log(anggota.alamat.kota);
}

// ✅ Optional chaining (?.) — aman dan concise
console.log(anggota.alamat?.kota);              // undefined jika alamat tidak ada
console.log(anggota.alamat?.kodePos);           // undefined
console.log(anggota.preferensi?.kategori);      // undefined

// ✅ Nullish coalescing (??) — nilai default jika null/undefined
const kota = anggota.alamat?.kota ?? "Tidak Diketahui";
const kategoriDefault = anggota.preferensi?.kategori ?? "Umum";

// Perbedaan ?? vs ||:
const stok = 0;
console.log(stok || 10);   // 10 — salah! 0 adalah falsy, tapi 0 stok valid
console.log(stok ?? 10);   // 0 — benar! ?? hanya cek null/undefined, bukan falsy

// ✅ Optional chaining dengan function call
const panjangNama = anggota.alamat?.kota?.length; // undefined jika tidak ada

// ✅ Non-null assertion operator (!) — gunakan dengan hati-hati!
// Memberitahu TypeScript "percayalah, ini tidak null"
// Gunakan HANYA jika benar-benar yakin tidak null
function prosesAnggota(id: number): void {
    const anggotaDitemukan = cariAnggotaById(id);
    // Jika yakin anggota pasti ada (karena sudah divalidasi sebelumnya):
    const nama = anggotaDitemukan!.nama; // ! = "ini pasti tidak null"
    // Tapi lebih aman:
    if (!anggotaDitemukan) throw new Error(`Anggota ${id} tidak ditemukan`);
    const namaAnggota = anggotaDitemukan.nama; // TypeScript tahu tidak null
}

function cariAnggotaById(id: number): AnggotaLengkap | null {
    return null; // placeholder
}
```

---

### E. Type Guards dan Narrowing

> 💡 **Benang Merah ke D**: Interface dan type alias mendefinisikan tipe. Type guards membantu TypeScript memahami tipe yang lebih spesifik saat runtime.

#### [[11. Type Guards — Mempersempit Tipe Saat Runtime]]

TypeScript

```
// src/utils/type-guards.ts

type Buku = { id: number; judul: string; tipe: "fisik" | "digital" };
type BukuFisik   = Buku & { tipe: "fisik";   lokasi: string; berat: number };
type BukuDigital = Buku & { tipe: "digital"; formatFile: "PDF" | "EPUB"; ukuranMB: number };

type ItemKoleksi = BukuFisik | BukuDigital;

// ─── Cara 1: typeof guard ──────────────────────────────────────────────────
function prosesId(id: number | string): string {
    if (typeof id === "number") {
        return `#${id.toString().padStart(5, "0")}`; // id adalah number di sini
    }
    return id.trim().toUpperCase(); // id adalah string di sini
}

// ─── Cara 2: in operator guard ────────────────────────────────────────────
function tampilkanInfoBuku(buku: ItemKoleksi): string {
    if ("lokasi" in buku) {
        // TypeScript tahu: buku adalah BukuFisik
        return `${buku.judul} — Rak: ${buku.lokasi} (${buku.berat}gr)`;
    }
    // TypeScript tahu: buku adalah BukuDigital
    return `${buku.judul} — ${buku.formatFile} (${buku.ukuranMB}MB)`;
}

// ─── Cara 3: discriminated union ──────────────────────────────────────────
// Paling direkomendasikan untuk union types yang kompleks
function prosesBerdasarkanTipe(buku: ItemKoleksi): string {
    switch (buku.tipe) {
        case "fisik":
            // TypeScript tahu: buku adalah BukuFisik
            return `Fisik: ${buku.lokasi}`;
        case "digital":
            // TypeScript tahu: buku adalah BukuDigital
            return `Digital: ${buku.formatFile}`;
        // TypeScript akan error jika ada case yang tidak di-handle (exhaustive check)
    }
}

// ─── Cara 4: instanceof guard ─────────────────────────────────────────────
class ErrorValidasi extends Error {
    constructor(public field: string, message: string) {
        super(message);
    }
}

class ErrorDatabase extends Error {
    constructor(public query: string, message: string) {
        super(message);
    }
}

function tanganiError(error: unknown): string {
    if (error instanceof ErrorValidasi) {
        return `Validasi gagal pada field '${error.field}': ${error.message}`;
    }
    if (error instanceof ErrorDatabase) {
        return `Database error: ${error.message}`;
    }
    if (error instanceof Error) {
        return `Error: ${error.message}`;
    }
    return "Terjadi kesalahan yang tidak diketahui";
}

// ─── Cara 5: User-defined type guard — is keyword ─────────────────────────
function adalahBukuDigital(buku: ItemKoleksi): buku is BukuDigital {
    return buku.tipe === "digital";
}

function prosesKoleksi(koleksi: ItemKoleksi[]): void {
    koleksi.forEach(item => {
        if (adalahBukuDigital(item)) {
            // TypeScript tahu: item adalah BukuDigital
            console.log(`Download ${item.formatFile}`);
        }
    });
}
```

---

### 🏗️ Checkpoint Level 2

text

```
✅ Checklist sebelum lanjut ke Level 3:

PROYEK: Sistem Data Perpustakaan Fully Typed
├── src/interfaces/buku.ts: Interface Buku, BukuFisik, BukuDigital
├── src/interfaces/anggota.ts: Interface Anggota, AnggotaLengkap
├── src/interfaces/peminjaman.ts: Interface Peminjaman
├── src/types/aliases.ts: Type alias Kategori, Status, Role
├── src/utils/type-guards.ts: Type guard functions
└── src/katalog.ts: Array katalog dengan tipe yang benar

PEMAHAMAN:
├── Bisa jelaskan perbedaan interface vs type alias
├── Bisa jelaskan kapan pakai intersection (&) vs union (|)
├── Bisa jelaskan perbedaan optional (?) vs union dengan undefined
├── Bisa jelaskan 4 cara type narrowing
└── Bisa jelaskan perbedaan ?. vs ?? vs ||

Git: feat: implement typed interfaces, type aliases, and type guards
```

---

## 🟡 LEVEL 3: CLASS, OOP, DAN GENERICS (Minggu 6-10)

> **Tema**: _"Dari tipe data ke objek yang memiliki perilaku dan kode yang reusable"_  
> **Benang Merah**: Interface (Level 2) → Class mengimplementasikan interface → OOP patterns → Generics untuk kode yang reusable  
> **Output**: Sistem perpustakaan berorientasi objek dengan generics

---

### F. Class — Object yang Memiliki Perilaku

> 💡 **Mengapa Class?** Interface hanya mendefinisikan _bentuk_ data. Class mendefinisikan _perilaku_ — method yang bisa dipanggil, validasi, business logic. TypeScript menambahkan access modifier (`private`, `protected`, `public`) dan type safety ke class JavaScript.

text

```
Benang Merah Bagian F:
Interface: kontrak bentuk data (Level 2) →
Class: implementasi yang memiliki data DAN perilaku →
Access modifier: kontrol akses ke property dan method →
Constructor property promotion: cara singkat definisi property →
Abstract class: template untuk class turunan
```

#### [[12. Class TypeScript — Lebih dari Class JavaScript]]

TypeScript

```
// src/models/Buku.ts

import type { Kategori } from "../types/aliases";

class Buku {
    // Access modifiers:
    // public: bisa diakses dari mana saja (default)
    // private: hanya bisa diakses dari dalam class ini
    // protected: bisa diakses dari dalam class ini dan turunannya
    // readonly: tidak bisa diubah setelah constructor

    readonly id: number;
    public judul: string;
    public pengarang: string;
    public isbn: string;
    public tahun: number;
    public harga: number;
    private _stok: number;      // private: gunakan getter/setter
    public kategori: Kategori;
    public deskripsi?: string;
    private readonly createdAt: Date;

    constructor(
        id: number,
        judul: string,
        pengarang: string,
        isbn: string,
        tahun: number,
        harga: number,
        stok: number,
        kategori: Kategori,
        deskripsi?: string,
    ) {
        this.id        = id;
        this.judul     = judul;
        this.pengarang = pengarang;
        this.isbn      = isbn;
        this.tahun     = tahun;
        this.harga     = harga;
        this._stok     = stok;
        this.kategori  = kategori;
        this.deskripsi = deskripsi;
        this.createdAt = new Date();
    }

    // Getter: akses _stok dengan validasi
    get stok(): number {
        return this._stok;
    }

    // Setter: validasi sebelum set
    set stok(nilai: number) {
        if (nilai < 0) throw new Error("Stok tidak bisa negatif");
        this._stok = nilai;
    }

    // Method
    tersedia(): boolean {
        return this._stok > 0;
    }

    pinjam(): void {
        if (!this.tersedia()) {
            throw new Error(`Buku "${this.judul}" tidak tersedia`);
        }
        this._stok--;
    }

    kembalikan(): void {
        this._stok++;
    }

    formatHarga(): string {
        return `Rp ${this.harga.toLocaleString("id-ID")}`;
    }

    // Static method: dipanggil dari class, bukan instance
    static fromJSON(data: unknown): Buku {
        if (!adalahDataBuku(data)) {
            throw new Error("Data buku tidak valid");
        }
        return new Buku(
            data.id, data.judul, data.pengarang,
            data.isbn, data.tahun, data.harga,
            data.stok, data.kategori as Kategori,
        );
    }

    // toJSON: konversi ke plain object
    toJSON(): Record<string, unknown> {
        return {
            id: this.id,
            judul: this.judul,
            pengarang: this.pengarang,
            isbn: this.isbn,
            tahun: this.tahun,
            harga: this.harga,
            stok: this._stok,
            kategori: this.kategori,
            deskripsi: this.deskripsi,
        };
    }
}

function adalahDataBuku(data: unknown): data is {
    id: number; judul: string; pengarang: string; isbn: string;
    tahun: number; harga: number; stok: number; kategori: string;
} {
    return (
        typeof data === "object" && data !== null &&
        "id" in data && typeof (data as any).id === "number" &&
        "judul" in data && typeof (data as any).judul === "string"
        // ... dst
    );
}
```

#### [[13. Constructor Property Promotion dan Access Modifiers]]

TypeScript

```
// src/models/Anggota.ts

// Cara lama: verbose
class AnggotaLama {
    public readonly id: number;
    public nama: string;
    public email: string;
    private _password: string;
    protected role: RolePengguna;

    constructor(id: number, nama: string, email: string, password: string, role: RolePengguna) {
        this.id        = id;
        this.nama      = nama;
        this.email     = email;
        this._password = password;
        this.role      = role;
    }
}

// Cara modern: Constructor Property Promotion (TypeScript 4.0+)
// Lebih singkat, hasil identik!
class Anggota {
    constructor(
        public readonly id: number,
        public nama: string,
        public email: string,
        private _password: string,
        protected role: RolePengguna = "anggota",
        private pinjaman: Buku[] = [],
    ) {}

    verifikasiPassword(password: string): boolean {
        // Simulasi bcrypt compare
        return this._password === password;
    }

    getBatasPinjam(): number {
        return this.role === "admin" ? Infinity : 5;
    }

    pinjamBuku(buku: Buku): void {
        if (this.pinjaman.length >= this.getBatasPinjam()) {
            throw new Error(`Batas peminjaman (${this.getBatasPinjam()}) sudah tercapai`);
        }
        buku.pinjam();
        this.pinjaman.push(buku);
    }

    getPinjaman(): readonly Buku[] {
        return [...this.pinjaman]; // return copy, bukan reference asli
    }
}

// Class turunan — Admin extends Anggota
class Admin extends Anggota {
    constructor(id: number, nama: string, email: string, password: string) {
        super(id, nama, email, password, "admin"); // panggil constructor parent
    }

    hapusBuku(buku: Buku, katalog: Buku[]): Buku[] {
        // Admin bisa hapus buku dari katalog
        return katalog.filter(b => b.id !== buku.id);
    }
}

// Abstract class: template yang tidak bisa di-instantiate langsung
abstract class Pengguna {
    constructor(
        public readonly id: number,
        public nama: string,
    ) {}

    // Method konkret: sudah ada implementasi
    sapa(): string {
        return `Halo, ${this.nama}!`;
    }

    // Method abstract: HARUS diimplementasikan oleh turunan
    abstract getRole(): RolePengguna;
    abstract getBatasPinjam(): number;
}

type RolePengguna = "admin" | "pustakawan" | "anggota";
```

---

### G. Generics — Kode yang Reusable untuk Berbagai Tipe

> 💡 **Mengapa Generics?** Tanpa generics, kamu harus tulis fungsi terpisah untuk setiap tipe (`cariBuku`, `cariAnggota`, `cariPeminjaman`). Dengan generics, satu fungsi bisa bekerja untuk semua tipe dengan tetap type-safe.

#### [[14. Generics Dasar — Fungsi yang Bekerja untuk Berbagai Tipe]]

TypeScript

```
// src/utils/generics.ts

// ─── Masalah tanpa generics ───────────────────────────────────────────────
function ambilPertamaBuku(array: Buku[]): Buku | undefined {
    return array[0];
}
function ambilPertamaAnggota(array: Anggota[]): Anggota | undefined {
    return array[0];
}
// Harus tulis fungsi baru untuk setiap tipe! → tidak DRY

// ─── Solusi: Generic function ─────────────────────────────────────────────
// <T> adalah type parameter: placeholder untuk tipe yang akan ditentukan saat dipanggil
function ambilPertama<T>(array: T[]): T | undefined {
    return array[0];
}

// TypeScript otomatis inference tipe T dari argument
const bukuPertama  = ambilPertama(katalogBuku);   // T = Buku
const anggotaPertama = ambilPertama(daftarAnggota); // T = Anggota
const angkaPertama   = ambilPertama([1, 2, 3]);     // T = number

// Atau eksplisit:
const hasil = ambilPertama<Buku>(katalogBuku);

// ─── Generic function yang lebih kompleks ─────────────────────────────────
function cariDalam<T>(
    array: T[],
    predikat: (item: T) => boolean,
): T | undefined {
    return array.find(predikat);
}

const bukuDitemukan = cariDalam(katalogBuku, buku => buku.judul.includes("Clean"));
const anggotaDitemukan = cariDalam(daftarAnggota, a => a.email === "budi@email.com");

// ─── Generic dengan constraints — T harus memiliki property tertentu ──────
interface HasId {
    id: number;
}

function cariById<T extends HasId>(array: T[], id: number): T | undefined {
    return array.find(item => item.id === id);
}

// T extends HasId: T harus memiliki property id (Buku ✅, Anggota ✅, number ❌)
const buku = cariById(katalogBuku, 1);      // ✅ Buku punya id
const anggota = cariById(daftarAnggota, 2); // ✅ Anggota punya id
// cariById([1, 2, 3], 1); // ❌ Error: number tidak punya property id

// ─── Generic dengan multiple type parameters ──────────────────────────────
function petakan<T, U>(array: T[], transformasi: (item: T) => U): U[] {
    return array.map(transformasi);
}

const judulBuku = petakan(katalogBuku, buku => buku.judul); // U = string
const hargaBuku = petakan(katalogBuku, buku => buku.harga); // U = number

// ─── keyof operator — type-safe property access ───────────────────────────
function ambilNilai<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

const buku1: Buku = { /* ... */ } as Buku;
const judul = ambilNilai(buku1, "judul");     // TypeScript tahu: string
const harga  = ambilNilai(buku1, "harga");    // TypeScript tahu: number
// ambilNilai(buku1, "tidakAda"); // ❌ Error: "tidakAda" bukan key dari Buku
```

#### [[15. Generic Class dan Interface]]

TypeScript

```
// src/utils/repository.ts

// Generic interface untuk Repository Pattern
interface Repository<T extends HasId> {
    findAll(): T[];
    findById(id: number): T | undefined;
    create(data: Omit<T, "id">): T;
    update(id: number, data: Partial<T>): T | undefined;
    delete(id: number): boolean;
}

// Generic class — implementasi in-memory repository
class InMemoryRepository<T extends HasId> implements Repository<T> {
    protected items: T[] = [];
    private nextId = 1;

    findAll(): T[] {
        return [...this.items]; // return copy
    }

    findById(id: number): T | undefined {
        return this.items.find(item => item.id === id);
    }

    create(data: Omit<T, "id">): T {
        const newItem = { ...data, id: this.nextId++ } as T;
        this.items.push(newItem);
        return newItem;
    }

    update(id: number, data: Partial<T>): T | undefined {
        const index = this.items.findIndex(item => item.id === id);
        if (index === -1) return undefined;

        this.items[index] = { ...this.items[index], ...data };
        return this.items[index];
    }

    delete(id: number): boolean {
        const index = this.items.findIndex(item => item.id === id);
        if (index === -1) return false;

        this.items.splice(index, 1);
        return true;
    }
}

// Extend untuk Buku — tambahkan method spesifik
class BukuRepository extends InMemoryRepository<Buku> {
    cariBerdasarkanKategori(kategori: Kategori): Buku[] {
        return this.items.filter(b => b.kategori === kategori);
    }

    cariBerdasarkanKeyword(keyword: string): Buku[] {
        const kw = keyword.toLowerCase();
        return this.items.filter(b =>
            b.judul.toLowerCase().includes(kw) ||
            b.pengarang.toLowerCase().includes(kw),
        );
    }

    cariTersedia(): Buku[] {
        return this.items.filter(b => b.stok > 0);
    }
}

// Penggunaan:
const bukuRepo = new BukuRepository();
const anggotaRepo = new InMemoryRepository<Anggota>();

const buku1 = bukuRepo.create({
    judul: "Clean Code",
    pengarang: "Robert Martin",
    isbn: "9780132350884",
    tahun: 2008,
    harga: 150000,
    stok: 5,
    kategori: "Teknologi",
    createdAt: new Date(),
});

// Generic Result type untuk error handling
type Result<T, E = Error> =
    | { success: true; data: T }
    | { success: false; error: E };

function cariAnggotaAman(id: number): Result<Anggota> {
    const anggota = anggotaRepo.findById(id);
    if (!anggota) {
        return { success: false, error: new Error(`Anggota ${id} tidak ditemukan`) };
    }
    return { success: true, data: anggota };
}

const hasil = cariAnggotaAman(1);
if (hasil.success) {
    console.log(hasil.data.nama); // TypeScript tahu: Anggota
} else {
    console.error(hasil.error.message); // TypeScript tahu: Error
}
```

---

### 🏗️ Checkpoint Level 3

text

```
✅ Checklist sebelum lanjut ke Level 4:

PROYEK: Sistem Perpustakaan OOP
├── src/models/Buku.ts: class Buku dengan getter/setter
├── src/models/Anggota.ts: class Anggota, Admin (extends)
├── src/models/Peminjaman.ts: class Peminjaman
├── src/repositories/InMemoryRepository.ts: generic repository
├── src/repositories/BukuRepository.ts: extends generic repo
├── src/index.ts: demo CRUD menggunakan repository
└── Semua type-safe tanpa any

PEMAHAMAN:
├── Bisa jelaskan perbedaan public, private, protected, readonly
├── Bisa jelaskan constructor property promotion
├── Bisa jelaskan kapan pakai abstract class vs interface
├── Bisa jelaskan mengapa generics lebih baik dari any
└── Bisa jelaskan constraints pada generics (extends keyof)

Git: feat: implement OOP classes, generics, and repository pattern
```

---

## 🟠 LEVEL 4: ADVANCED TYPES DAN UTILITY TYPES (Minggu 10-14)

> **Tema**: _"Menjadi master type system TypeScript"_  
> **Benang Merah**: Generics dasar (Level 3) → Utility types bawaan → Mapped types → Conditional types → Template literal types  
> **Output**: Type system yang sangat ekspresif untuk seluruh aplikasi perpustakaan

---

### H. Utility Types — Transformasi Tipe yang Sudah Ada

> 💡 **Mengapa Utility Types?** Sering kita butuh versi modifikasi dari tipe yang sudah ada — "semua property Buku jadi optional" atau "hanya properti tertentu dari Buku". Utility types adalah shortcut yang sudah disediakan TypeScript.

#### [[16. Built-in Utility Types — Yang Paling Sering Dipakai]]

TypeScript

```
// src/types/utility-examples.ts

interface Buku {
    id: number;
    judul: string;
    pengarang: string;
    isbn: string;
    tahun: number;
    harga: number;
    stok: number;
    kategori: Kategori;
    deskripsi?: string;
}

// ─── Partial<T>: semua property jadi optional ────────────────────────────
type BukuUpdate = Partial<Buku>;
// Sama dengan:
// { id?: number; judul?: string; pengarang?: string; ... }

function updateBuku(id: number, data: Partial<Buku>): Buku {
    // Bisa update hanya sebagian field
    const buku = bukuRepo.findById(id)!;
    return { ...buku, ...data };
}
updateBuku(1, { harga: 200000 });         // ✅ hanya update harga
updateBuku(1, { stok: 0, harga: 0 });     // ✅ update dua field

// ─── Required<T>: semua property jadi required ───────────────────────────
type BukuLengkap = Required<Buku>;
// Semua property termasuk deskripsi? jadi required

// ─── Readonly<T>: semua property jadi readonly ───────────────────────────
type BukuImmutable = Readonly<Buku>;
const bukuTetap: BukuImmutable = { /* ... */ } as BukuImmutable;
// bukuTetap.harga = 200000; // ❌ Error: readonly

// ─── Pick<T, K>: ambil hanya property tertentu ───────────────────────────
type InfoDasar = Pick<Buku, "id" | "judul" | "pengarang">;
// { id: number; judul: string; pengarang: string }

type KartuBuku = Pick<Buku, "id" | "judul" | "pengarang" | "harga" | "stok">;

// ─── Omit<T, K>: ambil semua kecuali property tertentu ───────────────────
type BukuBaru = Omit<Buku, "id">; // untuk create: belum ada id
// { judul: string; pengarang: string; isbn: string; ... }

type BukuPublik = Omit<Buku, "harga" | "stok">; // tidak tampilkan harga dan stok

// ─── Record<K, V>: object dengan key K dan value V ───────────────────────
type StatistikPerKategori = Record<Kategori, number>;
// { Fiksi: number; Non-Fiksi: number; Sains: number; Teknologi: number; ... }

const statistik: StatistikPerKategori = {
    "Fiksi": 45,
    "Non-Fiksi": 32,
    "Sains": 28,
    "Teknologi": 67,
    "Sejarah": 21,
    "Umum": 15,
};

type KatalogPerKategori = Record<Kategori, Buku[]>;

// ─── Exclude<T, U>: hapus tipe U dari union T ────────────────────────────
type StatusAktif = Exclude<StatusPeminjaman, "dikembalikan" | "hilang">;
// "dipinjam" | "terlambat"

// ─── Extract<T, U>: ambil hanya tipe U dari union T ─────────────────────
type StatusSelesai = Extract<StatusPeminjaman, "dikembalikan" | "hilang">;
// "dikembalikan" | "hilang"

// ─── NonNullable<T>: hapus null dan undefined ─────────────────────────────
type BukuPasti = NonNullable<Buku | null | undefined>;
// Buku

// ─── ReturnType<T>: dapatkan return type dari fungsi ─────────────────────
function cariBuku(keyword: string): Buku[] { return []; }
type HasilCari = ReturnType<typeof cariBuku>; // Buku[]

// ─── Parameters<T>: dapatkan parameter types dari fungsi ─────────────────
type ParameterCari = Parameters<typeof cariBuku>; // [keyword: string]

// ─── Awaited<T>: unwrap Promise type ─────────────────────────────────────
async function ambilBukuDariAPI(): Promise<Buku[]> { return []; }
type HasilAsync = Awaited<ReturnType<typeof ambilBukuDariAPI>>; // Buku[]
```

#### [[17. Mapped Types dan Conditional Types]]

TypeScript

```
// src/types/advanced.ts

// ─── Mapped Types: transformasi setiap property dalam tipe ───────────────
// Membuat semua property jadi string (contoh sederhana)
type StringifiedBuku = {
    [K in keyof Buku]: string;
};

// Membuat form validation errors untuk setiap field
type FormErrors<T> = {
    [K in keyof T]?: string;  // setiap field punya pesan error opsional
};

type BukuFormErrors = FormErrors<Buku>;
// { id?: string; judul?: string; pengarang?: string; ... }

const errors: BukuFormErrors = {
    judul: "Judul wajib diisi",
    isbn: "ISBN harus 13 digit",
    // field lain tidak wajib diisi
};

// Membuat semua method dalam class menjadi async
type Asyncify<T> = {
    [K in keyof T]: T[K] extends (...args: infer A) => infer R
        ? (...args: A) => Promise<R>
        : T[K];
};

// ─── Conditional Types: tipe yang bergantung pada kondisi ─────────────────
// T extends U ? TrueType : FalseType
type IsString<T> = T extends string ? true : false;

type CekJudul = IsString<string>; // true
type CekHarga = IsString<number>; // false

// Conditional type yang lebih berguna
type Flatten<T> = T extends Array<infer Item> ? Item : T;

type ItemBuku = Flatten<Buku[]>;  // Buku
type ItemString = Flatten<string>;  // string (bukan array, return as-is)

// infer: ekstrak tipe dari dalam tipe lain
type UnwrapPromise<T> = T extends Promise<infer U> ? U : T;

type HasilFetch = UnwrapPromise<Promise<Buku[]>>; // Buku[]
type BukanPromise = UnwrapPromise<string>;          // string

// ─── Template Literal Types yang lebih advanced ───────────────────────────
type EventName<T extends string> = `on${Capitalize<T>}`;

type EventBukuTambah = EventName<"bukuTambah">;   // "onBukuTambah"
type EventBukuHapus  = EventName<"bukuHapus">;    // "onBukuHapus"

// Membuat event handler types otomatis
type Aksi = "tambah" | "update" | "hapus";
type EventHandler = {
    [K in Aksi as `on${Capitalize<K>}`]: (id: number) => void;
};
// { onTambah: (id: number) => void; onUpdate: (id: number) => void; onHapus: (id: number) => void }
```

---

### 🏗️ Checkpoint Level 4

text

```
✅ Checklist sebelum lanjut ke Level 5:

TIPE YANG HARUS BISA DIBUAT:
├── Partial<Buku> untuk update operations
├── Omit<Buku, "id"> untuk create operations
├── Pick<Buku, "id" | "judul"> untuk response ringkas
├── Record<Kategori, Buku[]> untuk grouping
├── FormErrors<Buku> dengan mapped type
└── Result<T, E> generic untuk error handling

PEMAHAMAN:
├── Bisa jelaskan kapan pakai Partial vs Required vs Readonly
├── Bisa jelaskan kapan pakai Pick vs Omit
├── Bisa jelaskan cara kerja mapped type
├── Bisa jelaskan cara kerja conditional type dengan infer
└── Bisa membuat utility type sendiri

Git: feat: implement advanced types and utility type transformations
```

---

## 🔴 LEVEL 5: NODE.JS DAN EXPRESS API (Minggu 14-20)

> **Tema**: _"Dari TypeScript di CLI ke REST API yang fully typed"_  
> **Benang Merah**: TypeScript di Node.js CLI (Level 1-4) → Express dengan TypeScript → Request/Response yang typed → Error handling yang konsisten → Middleware yang type-safe  
> **Output**: REST API perpustakaan dengan Express, TypeScript strict, dan error handling production-level

---

### I. Setup Express dengan TypeScript

> 💡 **Mengapa Express?** Express adalah framework Node.js paling populer. TypeScript + Express = API yang type-safe dari request hingga response.

#### [[18. Setup Express TypeScript Project]]

Bash

```
# Install dependencies
npm install express
npm install --save-dev @types/express @types/node

# Install tambahan untuk API
npm install zod                    # runtime validation
npm install cors helmet            # security middleware
npm install --save-dev @types/cors
```

text

```
Struktur project:
src/
├── index.ts                  ← entry point
├── app.ts                    ← express app setup
├── config/
│   └── env.ts                ← environment variables (typed)
├── types/
│   ├── index.ts              ← semua type exports
│   └── express.d.ts          ← extend Express types
├── middleware/
│   ├── errorHandler.ts
│   ├── validateRequest.ts
│   └── authenticate.ts
├── routes/
│   ├── index.ts
│   ├── buku.routes.ts
│   └── anggota.routes.ts
├── controllers/
│   ├── buku.controller.ts
│   └── anggota.controller.ts
├── services/
│   ├── buku.service.ts
│   └── anggota.service.ts
└── repositories/
    ├── buku.repository.ts
    └── anggota.repository.ts
```

#### [[19. Express App yang Fully Typed]]

TypeScript

```
// src/types/express.d.ts — extend Express types
import type { AnggotaData } from "./index";

declare global {
    namespace Express {
        interface Request {
            user?: AnggotaData;          // ditambahkan oleh auth middleware
            requestId?: string;          // ditambahkan oleh logging middleware
        }
    }
}

// src/app.ts
import express, { Application, Request, Response, NextFunction } from "express";
import cors from "cors";
import helmet from "helmet";
import { bukuRouter }    from "./routes/buku.routes";
import { anggotaRouter } from "./routes/anggota.routes";
import { errorHandler }  from "./middleware/errorHandler";

export function createApp(): Application {
    const app = express();

    // ─── Middleware global ─────────────────────────────────────────────
    app.use(helmet());          // security headers
    app.use(cors({ origin: process.env.ALLOWED_ORIGINS?.split(",") }));
    app.use(express.json({ limit: "10mb" }));
    app.use(express.urlencoded({ extended: true }));

    // Request ID middleware
    app.use((req: Request, _res: Response, next: NextFunction) => {
        req.requestId = crypto.randomUUID();
        next();
    });

    // ─── Routes ────────────────────────────────────────────────────────
    app.use("/api/v1/buku",    bukuRouter);
    app.use("/api/v1/anggota", anggotaRouter);

    // 404 handler
    app.use((_req: Request, res: Response) => {
        res.status(404).json({ success: false, message: "Endpoint tidak ditemukan" });
    });

    // Error handler HARUS paling terakhir
    app.use(errorHandler);

    return app;
}

// src/index.ts
import { createApp } from "./app";
import { env }       from "./config/env";

const app = createApp();

app.listen(env.PORT, () => {
    console.log(`🚀 Server berjalan di http://localhost:${env.PORT}`);
});
```

#### [[20. Typed Request Validation dengan Zod]]

TypeScript

```
// src/schemas/buku.schema.ts
import { z } from "zod";

// Zod schema = runtime validation + TypeScript types sekaligus!
export const CreateBukuSchema = z.object({
    judul:     z.string().min(1, "Judul wajib diisi").max(200),
    pengarang: z.string().min(1, "Pengarang wajib diisi").max(100),
    isbn:      z.string().length(13, "ISBN harus 13 digit").optional(),
    tahun:     z.number().int().min(1000).max(new Date().getFullYear()),
    harga:     z.number().min(0, "Harga tidak boleh negatif"),
    stok:      z.number().int().min(0, "Stok tidak boleh negatif"),
    kategori:  z.enum(["Fiksi", "Non-Fiksi", "Sains", "Teknologi", "Sejarah", "Umum"]),
    deskripsi: z.string().max(2000).optional(),
});

export const UpdateBukuSchema = CreateBukuSchema.partial(); // semua jadi optional

// Extract TypeScript type dari Zod schema:
export type CreateBukuDTO = z.infer<typeof CreateBukuSchema>;
export type UpdateBukuDTO = z.infer<typeof UpdateBukuSchema>;

// src/middleware/validateRequest.ts
import { ZodSchema } from "zod";
import { Request, Response, NextFunction } from "express";

export function validateBody<T>(schema: ZodSchema<T>) {
    return (req: Request, res: Response, next: NextFunction): void => {
        const result = schema.safeParse(req.body);

        if (!result.success) {
            const errors = result.error.flatten().fieldErrors;
            res.status(422).json({
                success: false,
                message: "Data tidak valid",
                errors,
            });
            return;
        }

        req.body = result.data; // ganti dengan data yang sudah divalidasi
        next();
    };
}
```

#### [[21. Controller dan Error Handling yang Type-safe]]

TypeScript

```
// src/types/api.ts — response types yang konsisten

export interface ApiResponse<T> {
    success: true;
    data: T;
    message?: string;
}

export interface ApiError {
    success: false;
    message: string;
    errors?: Record<string, string[]>;
    stack?: string;      // hanya di development
}

export type ApiResult<T> = ApiResponse<T> | ApiError;

// Helper functions
export function sukses<T>(data: T, message?: string): ApiResponse<T> {
    return { success: true, data, message };
}

export function gagal(message: string, errors?: Record<string, string[]>): ApiError {
    return { success: false, message, errors };
}

// src/errors/AppError.ts
export class AppError extends Error {
    constructor(
        public message: string,
        public statusCode: number = 500,
        public isOperational: boolean = true, // error yang "diharapkan" vs bug
    ) {
        super(message);
        Object.setPrototypeOf(this, AppError.prototype);
    }
}

export class NotFoundError extends AppError {
    constructor(resource: string, id: number | string) {
        super(`${resource} dengan ID ${id} tidak ditemukan`, 404);
    }
}

export class ValidationError extends AppError {
    constructor(message: string) {
        super(message, 422);
    }
}

export class UnauthorizedError extends AppError {
    constructor(message = "Tidak terautentikasi") {
        super(message, 401);
    }
}

export class ForbiddenError extends AppError {
    constructor(message = "Tidak memiliki izin") {
        super(message, 403);
    }
}

// src/middleware/errorHandler.ts
import { Request, Response, NextFunction } from "express";
import { AppError } from "../errors/AppError";
import { ZodError } from "zod";

export function errorHandler(
    error: unknown,
    req: Request,
    res: Response,
    _next: NextFunction,  // harus ada 4 parameter agar Express tahu ini error handler
): void {
    if (error instanceof AppError) {
        res.status(error.statusCode).json({
            success: false,
            message: error.message,
        });
        return;
    }

    if (error instanceof ZodError) {
        res.status(422).json({
            success: false,
            message: "Data tidak valid",
            errors: error.flatten().fieldErrors,
        });
        return;
    }

    // Error yang tidak dikenal — jangan ekspos detail ke user
    console.error("Unhandled error:", error);
    res.status(500).json({
        success: false,
        message: "Terjadi kesalahan pada server",
        ...(process.env.NODE_ENV === "development" && {
            stack: error instanceof Error ? error.stack : String(error),
        }),
    });
}

// src/controllers/buku.controller.ts
import { Request, Response, NextFunction } from "express";
import { BukuService }   from "../services/buku.service";
import { CreateBukuDTO } from "../schemas/buku.schema";
import { sukses }        from "../types/api";

export class BukuController {
    constructor(private bukuService: BukuService) {}

    index = async (req: Request, res: Response, next: NextFunction): Promise<void> => {
        try {
            const page    = parseInt(req.query.page as string) || 1;
            const perPage = parseInt(req.query.per_page as string) || 15;
            const cari    = req.query.cari as string | undefined;

            const hasil = await this.bukuService.getAll({ page, perPage, cari });

            res.json(sukses(hasil));
        } catch (error) {
            next(error); // teruskan ke errorHandler middleware
        }
    };

    show = async (req: Request, res: Response, next: NextFunction): Promise<void> => {
        try {
            const id   = parseInt(req.params.id);
            const buku = await this.bukuService.getById(id);

            res.json(sukses(buku));
        } catch (error) {
            next(error);
        }
    };

    store = async (req: Request, res: Response, next: NextFunction): Promise<void> => {
        try {
            const data: CreateBukuDTO = req.body; // sudah divalidasi middleware
            const buku = await this.bukuService.create(data);

            res.status(201).json(sukses(buku, "Buku berhasil ditambahkan"));
        } catch (error) {
            next(error);
        }
    };
}

// src/routes/buku.routes.ts
import { Router } from "express";
import { BukuController }    from "../controllers/buku.controller";
import { BukuService }       from "../services/buku.service";
import { BukuRepository }    from "../repositories/buku.repository";
import { validateBody }      from "../middleware/validateRequest";
import { authenticate }      from "../middleware/authenticate";
import { CreateBukuSchema, UpdateBukuSchema } from "../schemas/buku.schema";

const bukuRepo       = new BukuRepository();
const bukuService    = new BukuService(bukuRepo);
const bukuController = new BukuController(bukuService);

export const bukuRouter = Router();

// Public routes
bukuRouter.get("/",     bukuController.index);
bukuRouter.get("/:id",  bukuController.show);

// Protected routes
bukuRouter.post(
    "/",
    authenticate,
    validateBody(CreateBukuSchema),
    bukuController.store,
);

bukuRouter.put(
    "/:id",
    authenticate,
    validateBody(UpdateBukuSchema),
    bukuController.update,
);

bukuRouter.delete("/:id", authenticate, bukuController.destroy);
```

---

### 🏗️ Checkpoint Level 5

text

```
✅ Checklist sebelum lanjut ke Level 6:

PROYEK: REST API Perpustakaan
├── Endpoint: GET/POST /api/v1/buku, GET/PUT/DELETE /api/v1/buku/:id
├── Zod validation pada semua POST/PUT request
├── JWT authentication (buat sendiri atau gunakan library)
├── Error handling: AppError, NotFoundError, ValidationError
├── Response format konsisten: { success, data, message }
└── TypeScript strict: 0 error, 0 any

TEST DENGAN POSTMAN:
├── GET /api/v1/buku → 200 dengan data array
├── POST /api/v1/buku (tanpa auth) → 401
├── POST /api/v1/buku (data invalid) → 422 dengan field errors
├── POST /api/v1/buku (valid) → 201
└── GET /api/v1/buku/99999 → 404

Git: feat: build typed Express API with Zod validation and error handling
```

---

## ⚫ LEVEL 6: TESTING DAN DESIGN PATTERNS (Minggu 20-28)

> **Tema**: _"Dari API yang bekerja ke API yang bisa dipercaya dan dipelihara"_  
> **Benang Merah**: API sudah berjalan (Level 5) → testing memastikan tetap bekerja → design patterns untuk arsitektur yang bersih → type-safe di semua layer  
> **Output**: Test suite lengkap, arsitektur layered yang bersih, siap production

---

### J. Testing TypeScript dengan Vitest

> 💡 **Mengapa Vitest?** Vitest adalah test runner modern yang mendukung TypeScript secara native, jauh lebih cepat dari Jest, dan API-nya kompatibel dengan Jest sehingga mudah dipelajari.

#### [[22. Setup dan Feature Test]]

Bash

```
npm install --save-dev vitest @vitest/coverage-v8 supertest @types/supertest
```

TypeScript

```
// vitest.config.ts
import { defineConfig } from "vitest/config";

export default defineConfig({
    test: {
        globals: true,           // tidak perlu import describe, it, expect
        environment: "node",
        coverage: {
            provider: "v8",
            reporter: ["text", "html"],
            thresholds: { lines: 70, functions: 70 },
        },
    },
});
```

TypeScript

```
// tests/integration/buku.test.ts
import { describe, it, expect, beforeAll, afterAll, beforeEach } from "vitest";
import request from "supertest";
import { createApp } from "../../src/app";
import type { Application } from "express";

describe("Buku API", () => {
    let app: Application;

    beforeAll(() => {
        app = createApp();
    });

    describe("GET /api/v1/buku", () => {
        it("mengembalikan daftar buku dengan struktur yang benar", async () => {
            const response = await request(app).get("/api/v1/buku");

            expect(response.status).toBe(200);
            expect(response.body.success).toBe(true);
            expect(Array.isArray(response.body.data)).toBe(true);
        });

        it("mendukung filter berdasarkan kategori", async () => {
            const response = await request(app)
                .get("/api/v1/buku")
                .query({ kategori: "Teknologi" });

            expect(response.status).toBe(200);
            response.body.data.forEach((buku: any) => {
                expect(buku.kategori).toBe("Teknologi");
            });
        });
    });

    describe("POST /api/v1/buku", () => {
        const dataBukuValid = {
            judul: "TypeScript Deep Dive",
            pengarang: "Basarat Ali Syed",
            isbn: "9781234567890",
            tahun: 2023,
            harga: 200000,
            stok: 10,
            kategori: "Teknologi",
        };

        it("menolak request tanpa autentikasi", async () => {
            const response = await request(app)
                .post("/api/v1/buku")
                .send(dataBukuValid);

            expect(response.status).toBe(401);
            expect(response.body.success).toBe(false);
        });

        it("menolak data yang tidak valid", async () => {
            const token = await dapatkanToken(app, "admin");

            const response = await request(app)
                .post("/api/v1/buku")
                .set("Authorization", `Bearer ${token}`)
                .send({ judul: "" }); // judul kosong

            expect(response.status).toBe(422);
            expect(response.body.success).toBe(false);
            expect(response.body.errors).toHaveProperty("judul");
        });

        it("berhasil membuat buku baru dengan data valid", async () => {
            const token = await dapatkanToken(app, "admin");

            const response = await request(app)
                .post("/api/v1/buku")
                .set("Authorization", `Bearer ${token}`)
                .send(dataBukuValid);

            expect(response.status).toBe(201);
            expect(response.body.success).toBe(true);
            expect(response.body.data).toMatchObject({
                judul:     dataBukuValid.judul,
                pengarang: dataBukuValid.pengarang,
                kategori:  dataBukuValid.kategori,
            });
            expect(response.body.data.id).toBeDefined();
        });
    });
});

async function dapatkanToken(app: Application, role: string): Promise<string> {
    const response = await request(app)
        .post("/api/v1/auth/login")
        .send({ email: `${role}@test.com`, password: "password123" });
    return response.body.data.token;
}
```

#### [[23. Unit Test — Test Service dan Repository]]

TypeScript

```
// tests/unit/buku.service.test.ts
import { describe, it, expect, vi, beforeEach } from "vitest";
import { BukuService }     from "../../src/services/buku.service";
import { NotFoundError }   from "../../src/errors/AppError";
import type { BukuRepository } from "../../src/repositories/buku.repository";

describe("BukuService", () => {
    let bukuService: BukuService;
    let mockBukuRepo: BukuRepository; // mock object

    const bukuContoh = {
        id: 1,
        judul: "Clean Code",
        pengarang: "Robert Martin",
        isbn: "9780132350884",
        tahun: 2008,
        harga: 150000,
        stok: 5,
        kategori: "Teknologi" as const,
        createdAt: new Date(),
        updatedAt: new Date(),
        deletedAt: null,
    };

    beforeEach(() => {
        // Buat mock repository
        mockBukuRepo = {
            findAll: vi.fn(),
            findById: vi.fn(),
            create: vi.fn(),
            update: vi.fn(),
            delete: vi.fn(),
        } as unknown as BukuRepository;

        bukuService = new BukuService(mockBukuRepo);
    });

    describe("getById", () => {
        it("mengembalikan buku jika ditemukan", async () => {
            vi.mocked(mockBukuRepo.findById).mockResolvedValue(bukuContoh);

            const hasil = await bukuService.getById(1);

            expect(hasil).toEqual(bukuContoh);
            expect(mockBukuRepo.findById).toHaveBeenCalledWith(1);
            expect(mockBukuRepo.findById).toHaveBeenCalledOnce();
        });

        it("melempar NotFoundError jika buku tidak ada", async () => {
            vi.mocked(mockBukuRepo.findById).mockResolvedValue(null);

            await expect(bukuService.getById(999))
                .rejects
                .toThrow(NotFoundError);

            await expect(bukuService.getById(999))
                .rejects
                .toThrow("Buku dengan ID 999 tidak ditemukan");
        });
    });

    describe("create", () => {
        it("berhasil membuat buku baru", async () => {
            const dataBaru = {
                judul: "New Book", pengarang: "Author",
                tahun: 2024, harga: 100000, stok: 5,
                kategori: "Umum" as const,
            };

            vi.mocked(mockBukuRepo.create).mockResolvedValue({ ...bukuContoh, ...dataBaru });

            const hasil = await bukuService.create(dataBaru);

            expect(hasil.judul).toBe(dataBaru.judul);
            expect(mockBukuRepo.create).toHaveBeenCalledWith(dataBaru);
        });
    });
});
```

---

### K. Design Patterns dalam TypeScript

#### [[24. Pattern yang Paling Berguna di TypeScript]]

TypeScript

```
// src/patterns/builder.ts — Builder Pattern untuk query yang kompleks

class BukuQueryBuilder {
    private kondisi: string[] = [];
    private urutanField: string = "judul";
    private urutanArah: "asc" | "desc" = "asc";
    private limitNilai?: number;
    private offsetNilai?: number;

    kategori(kategori: Kategori): this {
        this.kondisi.push(`kategori = '${kategori}'`);
        return this; // return this untuk chaining
    }

    tersedia(): this {
        this.kondisi.push("stok > 0");
        return this;
    }

    cari(keyword: string): this {
        this.kondisi.push(`(judul LIKE '%${keyword}%' OR pengarang LIKE '%${keyword}%')`);
        return this;
    }

    urutkan(field: keyof Buku, arah: "asc" | "desc" = "asc"): this {
        this.urutanField = field as string;
        this.urutanArah  = arah;
        return this;
    }

    halaman(page: number, perPage: number): this {
        this.limitNilai  = perPage;
        this.offsetNilai = (page - 1) * perPage;
        return this;
    }

    build(): string {
        let query = "SELECT * FROM buku";
        if (this.kondisi.length > 0) {
            query += ` WHERE ${this.kondisi.join(" AND ")}`;
        }
        query += ` ORDER BY ${this.urutanField} ${this.urutanArah}`;
        if (this.limitNilai !== undefined) {
            query += ` LIMIT ${this.limitNilai} OFFSET ${this.offsetNilai ?? 0}`;
        }
        return query;
    }
}

// Penggunaan yang ekspresif:
const query = new BukuQueryBuilder()
    .kategori("Teknologi")
    .tersedia()
    .cari("clean")
    .urutkan("harga", "asc")
    .halaman(1, 10)
    .build();

// src/patterns/observer.ts — Observer Pattern untuk event system
type Handler<T> = (data: T) => void | Promise<void>;

class EventEmitter<Events extends Record<string, unknown>> {
    private handlers = new Map<keyof Events, Handler<any>[]>();

    on<K extends keyof Events>(event: K, handler: Handler<Events[K]>): void {
        if (!this.handlers.has(event)) {
            this.handlers.set(event, []);
        }
        this.handlers.get(event)!.push(handler);
    }

    off<K extends keyof Events>(event: K, handler: Handler<Events[K]>): void {
        const handlers = this.handlers.get(event) ?? [];
        this.handlers.set(event, handlers.filter(h => h !== handler));
    }

    async emit<K extends keyof Events>(event: K, data: Events[K]): Promise<void> {
        const handlers = this.handlers.get(event) ?? [];
        await Promise.all(handlers.map(h => h(data)));
    }
}

// Definisikan events yang type-safe
interface PerpustakaanEvents {
    "buku:dipinjam":    { bukuId: number; anggotaId: number; tanggal: Date };
    "buku:dikembalikan": { peminjamanId: number; denda: number };
    "anggota:daftar":   { anggotaId: number; email: string };
}

const emitter = new EventEmitter<PerpustakaanEvents>();

// Handler yang fully typed
emitter.on("buku:dipinjam", async ({ bukuId, anggotaId }) => {
    // TypeScript tahu: bukuId dan anggotaId adalah number
    await kirimEmailKonfirmasi(anggotaId, bukuId);
});

emitter.on("buku:dipinjam", ({ tanggal }) => {
    console.log(`Dipinjam pada: ${tanggal.toLocaleDateString()}`);
});

// Emit event
await emitter.emit("buku:dipinjam", {
    bukuId: 1,
    anggotaId: 42,
    tanggal: new Date(),
});

async function kirimEmailKonfirmasi(anggotaId: number, bukuId: number): Promise<void> {
    // implementasi
}
```

---

### 🏗️ Checkpoint Level 6

text

```
✅ Checklist sebelum lanjut ke Level 7:

TESTING:
├── npx vitest run: semua test hijau
├── Integration test: semua endpoint API
├── Unit test: Service dan Repository
├── Coverage: minimal 70%
└── Mock dengan vi.fn() dan vi.mocked()

DESIGN PATTERNS:
├── Builder pattern: BukuQueryBuilder
├── Observer pattern: type-safe EventEmitter
├── Repository pattern: interface + implementasi
└── Dependency Injection: constructor injection di semua class

ARSITEKTUR:
├── Layer: Controller → Service → Repository
├── Error hierarchy: AppError → NotFoundError, ValidationError, dll
├── Response format: ApiResponse<T> yang konsisten
└── 0 any, strict mode: true, 0 TypeScript error

Git: feat: add Vitest tests, design patterns, and clean architecture
```

---

## 🟣 LEVEL 7: REACT, NEXT.JS, DAN FULL-STACK (Minggu 28+)

> **Tema**: _"Dari backend API ke full-stack application dengan TypeScript end-to-end"_  
> **Benang Merah**: Backend API (Level 6) → TypeScript di React → Next.js full-stack → Type sharing antara frontend dan backend  
> **Output**: Aplikasi perpustakaan full-stack dengan TypeScript di seluruh stack

---

### L. TypeScript di React

#### [[25. React dengan TypeScript — Component yang Type-safe]]

Bash

```
# Buat project React + TypeScript
npm create vite@latest perpustakaan-frontend -- --template react-ts
cd perpustakaan-frontend
npm install

# Install tambahan
npm install axios react-query @tanstack/react-query
npm install --save-dev @types/react @types/react-dom
```

TypeScript

```
// src/types/api.ts — SHARE tipe dengan backend (monorepo atau copy)
export interface Buku {
    id: number;
    judul: string;
    pengarang: string;
    harga: number;
    stok: number;
    kategori: string;
    tersedia: boolean;
}

export interface ApiResponse<T> {
    success: true;
    data: T;
    message?: string;
}

// src/components/BukuCard.tsx — Typed React Component
import type { FC } from "react";
import type { Buku } from "../types/api";

// Props interface: definisikan apa yang diterima komponen
interface BukuCardProps {
    buku: Buku;
    onPinjam?: (bukuId: number) => void;   // optional callback
    tampilkanHarga?: boolean;               // optional dengan default
    ukuran?: "sm" | "md" | "lg";           // literal type
}

// FC = FunctionComponent — sudah include return type JSX.Element
export const BukuCard: FC<BukuCardProps> = ({
    buku,
    onPinjam,
    tampilkanHarga = true,
    ukuran = "md",
}) => {
    const handlePinjam = (): void => {
        onPinjam?.(buku.id); // optional chaining untuk callback
    };

    return (
        <div className={`card card--${ukuran}`}>
            <h3>{buku.judul}</h3>
            <p>{buku.pengarang}</p>

            {tampilkanHarga && (
                <p className="harga">
                    Rp {buku.harga.toLocaleString("id-ID")}
                </p>
            )}

            <span className={`badge badge--${buku.tersedia ? "success" : "danger"}`}>
                {buku.tersedia ? `Tersedia (${buku.stok})` : "Habis"}
            </span>

            {buku.tersedia && onPinjam && (
                <button onClick={handlePinjam} className="btn btn--primary">
                    Pinjam
                </button>
            )}
        </div>
    );
};

// src/hooks/useBuku.ts — Custom Hook yang Typed
import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";
import { bukuApi } from "../api/buku.api";
import type { CreateBukuDTO } from "../types/dto";

export function useBukuList(page: number = 1, kategori?: string) {
    return useQuery({
        queryKey: ["buku", page, kategori],
        queryFn: () => bukuApi.getAll({ page, kategori }),
        staleTime: 5 * 60 * 1000, // 5 menit
    });
}

export function useBukuDetail(id: number) {
    return useQuery({
        queryKey: ["buku", id],
        queryFn: () => bukuApi.getById(id),
        enabled: id > 0, // hanya fetch jika id valid
    });
}

export function useCreateBuku() {
    const queryClient = useQueryClient();

    return useMutation({
        mutationFn: (data: CreateBukuDTO) => bukuApi.create(data),
        onSuccess: () => {
            // Invalidate cache setelah buku baru dibuat
            queryClient.invalidateQueries({ queryKey: ["buku"] });
        },
    });
}

// src/api/buku.api.ts — Typed API client
import axios from "axios";
import type { Buku, ApiResponse } from "../types/api";

const client = axios.create({
    baseURL: import.meta.env.VITE_API_URL ?? "http://localhost:3000/api/v1",
    headers: { "Content-Type": "application/json" },
});

// Interceptor untuk auth token
client.interceptors.request.use(config => {
    const token = localStorage.getItem("token");
    if (token) config.headers.Authorization = `Bearer ${token}`;
    return config;
});

export const bukuApi = {
    getAll: async (params?: { page?: number; kategori?: string }): Promise<Buku[]> => {
        const { data } = await client.get<ApiResponse<Buku[]>>("/buku", { params });
        return data.data;
    },

    getById: async (id: number): Promise<Buku> => {
        const { data } = await client.get<ApiResponse<Buku>>(`/buku/${id}`);
        return data.data;
    },

    create: async (bukuData: CreateBukuDTO): Promise<Buku> => {
        const { data } = await client.post<ApiResponse<Buku>>("/buku", bukuData);
        return data.data;
    },

    update: async (id: number, bukuData: Partial<CreateBukuDTO>): Promise<Buku> => {
        const { data } = await client.put<ApiResponse<Buku>>(`/buku/${id}`, bukuData);
        return data.data;
    },

    delete: async (id: number): Promise<void> => {
        await client.delete(`/buku/${id}`);
    },
};

type CreateBukuDTO = {
    judul: string; pengarang: string; tahun: number;
    harga: number; stok: number; kategori: string; isbn?: string;
};
```

---

### M. Next.js — Full-Stack TypeScript

#### [[26. Next.js App Router dengan TypeScript]]

Bash

```
npx create-next-app@latest perpustakaan-nextjs --typescript --tailwind --app --src-dir
cd perpustakaan-nextjs
```

TypeScript

```
// src/app/buku/page.tsx — Server Component (default di App Router)

import type { Metadata } from "next";
import { BukuGrid } from "@/components/BukuGrid";
import { ambilBuku } from "@/lib/buku";

// Metadata yang typed
export const metadata: Metadata = {
    title: "Katalog Buku | Perpustakaan Digital",
    description: "Temukan buku favorit Anda",
};

// Props untuk page dengan searchParams
interface HalamanBukuProps {
    searchParams: {
        halaman?: string;
        kategori?: string;
        cari?: string;
    };
}

// Server Component: fetch data langsung di server
export default async function HalamanBuku({ searchParams }: HalamanBukuProps) {
    const halaman  = parseInt(searchParams.halaman ?? "1");
    const kategori = searchParams.kategori;
    const cari     = searchParams.cari;

    // Fetch di server — tidak ada loading state, data sudah ada saat render
    const { buku, total } = await ambilBuku({ halaman, kategori, cari });

    return (
        <main>
            <h1>Katalog Buku</h1>
            <p>{total} buku ditemukan</p>
            <BukuGrid buku={buku} />
        </main>
    );
}

// src/app/api/buku/route.ts — API Route Handler
import { NextRequest, NextResponse } from "next/server";
import { z } from "zod";
import { db } from "@/lib/database";

// GET /api/buku
export async function GET(request: NextRequest): Promise<NextResponse> {
    const { searchParams } = request.nextUrl;
    const halaman  = parseInt(searchParams.get("halaman") ?? "1");
    const perHalaman = 15;

    const buku = await db.buku.findMany({
        skip: (halaman - 1) * perHalaman,
        take: perHalaman,
        orderBy: { judul: "asc" },
    });

    return NextResponse.json({ success: true, data: buku });
}

// POST /api/buku
export async function POST(request: NextRequest): Promise<NextResponse> {
    const body = await request.json();

    const schema = z.object({
        judul: z.string().min(1),
        pengarang: z.string().min(1),
        tahun: z.number().int(),
        harga: z.number().min(0),
        stok: z.number().int().min(0),
    });

    const result = schema.safeParse(body);
    if (!result.success) {
        return NextResponse.json(
            { success: false, errors: result.error.flatten().fieldErrors },
            { status: 422 },
        );
    }

    const bukuBaru = await db.buku.create({ data: result.data });
    return NextResponse.json({ success: true, data: bukuBaru }, { status: 201 });
}
```

---

### 🏗️ Checkpoint Level 7 (Final)

text

```
✅ Checklist Akhir — Perpustakaan Full-Stack TypeScript:

FRONTEND (React atau Next.js):
├── Typed components: BukuCard, BukuGrid, FormBuku
├── Custom hooks: useBukuList, useBukuDetail, useCreateBuku
├── Typed API client: bukuApi dengan response types
├── React Query: caching dan loading states
└── TypeScript strict: 0 error di seluruh frontend

BACKEND:
├── Express API atau Next.js API routes
├── Zod validation pada semua input
├── JWT authentication
├── Error handling yang konsisten
└── Test suite dengan coverage > 70%

TYPE SAFETY END-TO-END:
├── Tipe di-share antara frontend dan backend (shared package atau copy)
├── API response selalu match dengan ApiResponse<T>
├── Tidak ada any di seluruh codebase
└── tsc --noEmit: 0 error di semua package

DEVOPS:
├── GitHub Actions: type-check + test setiap push
├── Build berhasil: npm run build tanpa error
└── Deploy ke Vercel (Next.js) atau Railway (Express)

Git: feat: complete full-stack TypeScript with React and type-safe API
```

---

## 📊 Ringkasan Progress Tracking

### Satu Project, 7 Level Enhancement

text

```
Level 1: CLI perpustakaan — tipe dasar, compile, setup strict
  + Level 2: + Interface, type alias, union, type guards
  + Level 3: + Class OOP, inheritance, generics, repository pattern
  + Level 4: + Utility types, mapped types, conditional types
  + Level 5: + Express API, Zod validation, JWT, error handling
  + Level 6: + Vitest testing, design patterns, clean architecture
  + Level 7: + React/Next.js full-stack, type-safe end-to-end
```

### Tabel Progress

|Level|Topik|Durasi|Output Konkret|
|---|---|---|---|
|🟢 **1**|1-7|Minggu 1-3|CLI perpustakaan dengan tipe dasar|
|🔵 **2**|8-11|Minggu 3-6|Sistem data fully typed dengan interface|
|🟡 **3**|12-15|Minggu 6-10|OOP classes, generics, repository|
|🟠 **4**|16-17|Minggu 10-14|Advanced types, utility types|
|🔴 **5**|18-21|Minggu 14-20|REST API Express dengan Zod|
|⚫ **6**|22-24|Minggu 20-28|Test suite, design patterns|
|🟣 **7**|25-26|Minggu 28+|Full-stack React/Next.js|

---

### Benang Merah Utama Sepanjang Roadmap

text

```
Poin 1  (TS vs JS)              → Fondasi: mengapa TypeScript layak dipelajari
Poin 4  (tsconfig strict: true) → Aktif dari awal, tidak pernah dimatikan
Poin 5  (unknown vs any)        → any = matikan TypeScript, unknown = aman
Poin 6  (literal types)         → Kategori, Status — nilai yang terkontrol
Poin 8  (interface)             → Fondasi semua struktur data Level 2+
Poin 11 (type narrowing)        → Cara TypeScript tahu tipe yang lebih spesifik
Poin 12 (class + access modifier) → OOP yang type-safe, fondasi Level 3+
Poin 14 (generics)              → Repository, Result<T> — reusable code
Poin 15 (generic class)         → InMemoryRepository yang dipakai di Level 5+
Poin 16 (Partial, Omit, Pick)   → Dipakai di setiap update/create operation
Poin 18 (Express + TypeScript)  → Foundation API yang dibangun di Level 5-7
Poin 20 (Zod)                   → Runtime validation + compile-time types
Poin 21 (error handling)        → AppError hierarchy yang konsisten
Poin 22 (Vitest)                → Test sebelum deploy — tidak ada excuse
Poin 24 (Observer pattern)      → Type-safe event system
Poin 25 (React typed)           → Props interface, hooks yang typed
```

---

## 💡 Cara Menggunakan Roadmap Ini

text

```
Setiap poin mengikuti format:
┌──────────────────────────────────────────────────────────┐
│ 💡 Konteks: mengapa fitur TypeScript ini ada             │
│ 🔗 Benang Merah: koneksi ke poin sebelum dan sesudahnya  │
│ 📋 Kode: implementasi di project perpustakaan            │
│          yang langsung bisa dicoba di local              │
│ ✅ Langkah konkret: verifikasi berhasil                  │
└──────────────────────────────────────────────────────────┘
```

**Aturan yang Tidak Boleh Dilanggar:**

1. **`strict: true` selalu aktif** di `tsconfig.json` — tidak ada kompromi
2. **Hindari `any`** — jika perlu type yang fleksibel, pakai `unknown` lalu narrow
3. **Biarkan inference bekerja** — jangan annotasi tipe yang sudah jelas dari nilai
4. **Gunakan Zod untuk input eksternal** — JSON dari API, form input, env vars
5. **`tsc --noEmit`** sebelum setiap commit — pastikan 0 TypeScript error
6. **Baca error TypeScript** — pesannya sangat informatif, jangan langsung Stack Overflow
7. **Generic bukan `any`** — jika butuh reusable code, pakai generics
8. **Type-share antara frontend dan backend** — satu source of truth untuk kontrak API

---

_Roadmap TypeScript v1.0 — Step-by-Step, Type Safety First, One Project_  
_Setiap anotasi tipe ditulis dengan sadar — TypeScript bekerja untuk kamu, bukan sebaliknya_