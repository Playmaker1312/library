# 84. Interface & Type Alias

**Benang Merah**: Materi 83 memberi kita **tipe dasar** (string, number, dll). Sekarang kita belajar **membentuk tipe-tipe itu menjadi struktur data yang jelas**. Lanjut ke Materi 85 (Generics).

---

## Penjelasan

**Interface** dan **Type Alias** adalah cara mendefinisikan **bentuk (shape)** dari sebuah data. Ini seperti **cetakan kue** — sekali cetakan jadi, semua kue yang dicetak akan punya bentuk yang sama persis.

### Interface
Mirip kontrak: "Semua object bertipe `User` harus punya `nama` (string) dan `umur` (number)."

```typescript
interface User {
  nama: string;
  umur: number;
}
```

### Type Alias
Lebih fleksibel — bisa untuk union, primitive, tuple, dll.

```typescript
type ID = string | number;
type Status = "aktif" | "nonaktif";
```

### Fitur Interface
- **Optional** (`?`) — properti boleh ada atau tidak
- **readonly** — properti hanya bisa dibaca, tidak bisa diubah setelah dibuat
- **Extending** — interface bisa mewarisi interface lain
- **Declaration merging** — interface bisa dideklarasikan ulang (bertambah propertinya)

### Intersection Types
Menggabungkan dua type/interface dengan `&`:

```typescript
type Admin = User & { role: "admin" };
```

### Interface vs Type

| Aspek | Interface | Type Alias |
|---|---|---|
| Extend | `extends` | `&` (intersection) |
| Union | ❌ | ✅ `string \| number` |
| Primitive | ❌ | ✅ `type Nama = string` |
| Declaration merging | ✅ | ❌ |
| Performance | Lebih cepat | Sedikit lebih lambat |

---

## Fungsi

Mendefinisikan **kontrak** untuk object, parameter, dan return value — membuat kode lebih **self-documenting** dan **type-safe**.

---

## Code

```typescript
// ========== INTERFACE DASAR ==========
interface User {
  id: number;
  nama: string;
  email: string;
}

// ========== OPTIONAL & READONLY ==========
interface Config {
  readonly apiKey: string;   // tidak bisa diubah setelah inisialisasi
  baseUrl: string;
  timeout?: number;          // optional
}

// ========== EXTENDING INTERFACE ==========
interface Admin extends User {
  role: "admin" | "superadmin";
  permissions: string[];
}

// ========== TYPE ALIAS ==========
type ID = string | number;
type StatusAnggota = "aktif" | "nonaktif" | "diblokir";
type Point = { x: number; y: number };

// ========== INTERSECTION ==========
type Employee = User & {
  jabatan: string;
  gaji: number;
};

// ========== PERPUSTAKAAN — Interface untuk semua entity ==========
interface Buku {
  id: number;
  judul: string;
  penulis: string;
  isbn: string;
  tahunTerbit: number;
  tersedia: boolean;
}

interface Anggota {
  id: number;
  nama: string;
  alamat: string;
  noTelepon: string;
  status: StatusAnggota;
  tanggalDaftar: Date;
}

interface Peminjaman {
  id: number;
  anggotaId: number;
  bukuId: number;
  tanggalPinjam: Date;
  tanggalKembali?: Date;
  denda?: number;
}

interface Perpustakaan {
  nama: string;
  alamat: string;
  daftarBuku: Buku[];
  daftarAnggota: Anggota[];
  daftarPeminjaman: Peminjaman[];
}

// Fungsi dengan interface
function tambahBuku(perpustakaan: Perpustakaan, buku: Buku): Perpustakaan {
  return {
    ...perpustakaan,
    daftarBuku: [...perpustakaan.daftarBuku, buku]
  };
}

function cariBuku(daftar: Buku[], judul: string): Buku[] {
  return daftar.filter(b => b.judul.toLowerCase().includes(judul.toLowerCase()));
}
```

---

## Analogi: Membangun Rumah (Cetakan Kue)

| TypeScript | Analogi Rumah |
|---|---|
| Interface | Cetakan kue — bentuk pasti, semua hasil sama |
| Type Alias | Cetakan fleksibel — bisa untuk berbagai bentuk |
| `?` (optional) | "Cerobong asap opsional" — boleh ada atau tidak |
| `readonly` | "Tiang penyangga" — tidak bisa dipindah setelah dipasang |
| `extends` | "Rumah mewah" mewarisi semua properti "Rumah standar" |
| Intersection (`&`) | Menggabungkan cetakan "Pintu" + "Jendela" |
| Declaration merging | Dua arsitek menambah detail ke cetakan yang sama |

Cetakan kue adalah **interface**. Anda buat satu cetakan bentuk bintang — semua kue yang dicetak pasti berbentuk bintang. Type alias seperti **cetakan silicon yang bisa melentur** — bisa dipakai untuk bentuk yang berbeda-beda. Interface memberikan **kepastian bentuk** yang sangat penting ketika tim besar bekerja bersama.

---

## Use Case

- **DTO (Data Transfer Object)** — bentuk data API
- **Model database** — struktur koleksi/tabel
- **Props & State di Vue/React** — type-safe component
- **Config object** — konfigurasi aplikasi
- **API response typing**

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Interface vs Type salah pilih | Pakai type untuk object shape | Kehilangan declaration merging |
| Lupa `?` | Property jadi required padahal opsional | Error runtime |
| Over-extending | Extends 5 level dalam | Sulit dilacak asal properti |
| Readonly diabaikan | Coba ubah `readonly` property | Error kompilasi |
| Interface untuk union | `interface X = string \| number` ❌ | Harus pakai type |

---

## Benang Merah

Materi 83 (Basic Types) → **Materi 84 (Interface & Type)** → Materi 85 (Generics)

Interface memberi kita **shape untuk data spesifik**. Tapi bagaimana kalau kita ingin satu fungsi yang **bekerja untuk berbagai tipe**? Itulah gunanya **Generics** — Materi 85.

---

## Soal Latihan

### Soal 1 (Mudah)
Buat interface `Smartphone` dengan properti: `merk` (string), `model` (string), `tahunRilis` (number), `masihBerfungsi?` (boolean, optional).

**Jawaban**:
```typescript
interface Smartphone {
  merk: string;
  model: string;
  tahunRilis: number;
  masihBerfungsi?: boolean;
}

const hp1: Smartphone = { merk: "Xiaomi", model: "Redmi Note 10", tahunRilis: 2021 };
const hp2: Smartphone = { merk: "Samsung", model: "S23", tahunRilis: 2023, masihBerfungsi: true };
```

### Soal 2 (Sedang)
Buat interface `Karyawan` (id, nama, jabatan) dan `KaryawanTetap` yang `extends Karyawan` dengan tambahan `gajiBulanan` (number) dan `tunjangan` (number). Buat fungsi `hitungGajiTahunan(k: KaryawanTetap): number`.

**Jawaban**:
```typescript
interface Karyawan {
  id: number;
  nama: string;
  jabatan: string;
}

interface KaryawanTetap extends Karyawan {
  gajiBulanan: number;
  tunjangan: number;
}

function hitungGajiTahunan(k: KaryawanTetap): number {
  return (k.gajiBulanan + k.tunjangan) * 12;
}

const budi: KaryawanTetap = { id: 1, nama: "Budi", jabatan: "Developer", gajiBulanan: 8000000, tunjangan: 1000000 };
console.log(hitungGajiTahunan(budi)); // 108000000
```

### Soal 3 (Tantangan)
Buat type `Response<T>` yang memiliki properti `status: "success" | "error"`, `data: T`, dan `message?: string`. Buat interface `User` dan implementasi fungsi yang mengembalikan `Response<User[]>`. Lalu buat intersection type `AdminUser` yang menggabungkan `User` dengan `{ role: "admin" }`.

**Jawaban**:
```typescript
type StatusResponse = "success" | "error";

interface Response<T> {
  status: StatusResponse;
  data: T;
  message?: string;
}

interface User {
  id: number;
  nama: string;
  email: string;
}

type AdminUser = User & { role: "admin" };

function getUsers(): Response<User[]> {
  return {
    status: "success",
    data: [
      { id: 1, nama: "Budi", email: "budi@mail.com" },
      { id: 2, nama: "Sari", email: "sari@mail.com" }
    ]
  };
}

function getAdmin(): Response<AdminUser> {
  return {
    status: "success",
    data: { id: 1, nama: "Admin", email: "admin@mail.com", role: "admin" }
  };
}
```
