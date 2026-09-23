# TypeScript Core — Types, Interface, Type Alias

## Penjelasan

Setelah lahan siap (setup environment) dan kita punya jendela untuk melihat database, sekarang saatnya mempelajari **bahan bangunan utama** TypeScript. NestJS dibangun di atas TypeScript, dan fondasi utamanya adalah **sistem tipe**.

Anggap TypeScript sebagai **arsitek yang memeriksa cetak biru** sebelum gedung dibangun. Ia memastikan bahwa tiang (variabel) yang seharusnya beton bertulang tidak diisi dengan kayu lapis. Ini mencegah kesalahan struktural sebelum gedung berdiri.

## Fungsi

Sistem tipe TypeScript berfungsi untuk:

- **Mencegah bug** dengan menangkap error tipe data saat kompilasi
- **Mendokumentasikan kode** secara eksplisit — tipe adalah dokumen hidup
- **Memberi autocomplete** yang akurat di editor
- **Memudahkan refactoring** — compiler akan memberi tahu bagian mana yang perlu diperbaiki

## Cara Pengimplementasian

### Tipe Primitif

```typescript
const name: string = 'NestJS';
const version: number = 10;
const isProduction: boolean = false;
const id: symbol = Symbol('unique');
const bigNumber: bigint = 100n;
const nullable: null = null;
const notDefined: undefined = undefined;
```

### Tipe Kompleks — Array & Tuple

```typescript
// Array
const items: string[] = ['module', 'controller', 'service'];
const numbers: Array<number> = [1, 2, 3];

// Tuple — array dengan panjang dan tipe fixed
const pair: [string, number] = ['version', 10];
```

### Interface

Interface mendefinisikan **bentuk (shape)** dari sebuah objek:

```typescript
interface User {
  id: string;
  name: string;
  email: string;
  age?: number; // properti opsional
  readonly createdAt: Date; // hanya bisa dibaca, tidak bisa diubah
}
```

### Type Alias

Type alias memberi nama baru pada suatu tipe. Mirip interface, tapi lebih fleksibel:

```typescript
type UserId = string;

type User = {
  id: UserId;
  name: string;
  email: string;
  age?: number;
  readonly createdAt: Date;
};
```

### Interface vs Type — Kapan Pakai Apa?

| Aspek | Interface | Type Alias |
|-------|-----------|------------|
| **Extends** | `extends` | Intersection (`&`) |
| **Union/Intersection** | Tidak bisa | Bisa (`string \| number`) |
| **Mapped Types** | Tidak bisa | Bisa |
| **Declaration Merging** | Bisa | Tidak bisa |
| **Primitive Types** | Tidak bisa | Bisa (`type ID = string`) |

```typescript
// Interface extends
interface BaseEntity {
  id: string;
  createdAt: Date;
}

interface User extends BaseEntity {
  name: string;
}

// Type intersection
type BaseEntity = {
  id: string;
  createdAt: Date;
};

type User = BaseEntity & {
  name: string;
};
```

### Union & Intersection Types

```typescript
// Union — bisa salah satu
type Status = 'active' | 'inactive' | 'pending';
type StringOrNumber = string | number;

// Intersection — gabungan semua
type WithTimestamp = {
  createdAt: Date;
  updatedAt: Date;
};

type Product = {
  id: string;
  name: string;
  price: number;
} & WithTimestamp;

// Guard dengan union
function processStatus(status: Status): void {
  if (status === 'active') {
    console.log('Aktif');
  } else if (status === 'inactive') {
    console.log('Non-aktif');
  } else {
    console.log('Pending');
  }
}
```

### Utility Types

Utility types adalah **tipe bawaan TypeScript** yang memanipulasi tipe yang sudah ada:

```typescript
interface User {
  id: string;
  name: string;
  email: string;
  password: string;
  age: number;
  createdAt: Date;
}

// Partial — semua properti jadi opsional
type PartialUser = Partial<User>;

// Required — semua properti jadi wajib (termasuk yang opsional)
type RequiredUser = Required<Partial<User>>;

// Pick — memilih properti tertentu
type UserPublic = Pick<User, 'id' | 'name' | 'email'>;

// Omit — menghilangkan properti tertentu
type UserWithoutPassword = Omit<User, 'password'>;

// Record — objek dengan key-value tertentu
type UserMap = Record<string, User>;

// Readonly — semua properti jadi readonly
type FrozenUser = Readonly<User>;

// ReturnType — mengambil tipe return dari suatu fungsi
function getConfig() {
  return { port: 3000, host: 'localhost' };
}
type Config = ReturnType<typeof getConfig>;
// { port: number; host: string }

// Parameters — mengambil tipe parameter dari suatu fungsi
type GetConfigParams = Parameters<typeof getConfig>;
```

### Contoh Lengkap Semua Utility Types

```typescript
interface Task {
  id: string;
  title: string;
  description?: string;
  completed: boolean;
  createdAt: Date;
}

// 1. Partial — untuk update (semua field opsional)
function updateTask(id: string, changes: Partial<Task>): Task {
  // implementation
  return {} as Task;
}

// 2. Required — semua field termasuk description wajib
function createTask(data: Required<Task>): Task {
  return data;
}

// 3. Pick — ambil field yang ditampilkan di list
type TaskPreview = Pick<Task, 'id' | 'title' | 'completed'>;

// 4. Omit — hapus field yang tidak perlu dikirim client
type TaskInput = Omit<Task, 'id' | 'createdAt'>;

// 5. Record — map task berdasarkan id
const taskMap: Record<string, Task> = {};

// 6. Readonly — cegah modifikasi
const frozenTask: Readonly<Task> = {
  id: '1',
  title: 'Belajar NestJS',
  completed: false,
  createdAt: new Date(),
};
// frozenTask.title = 'ubah'; // Error!
```

## Analogi

Membangun gedung bertingkat dengan TypeScript:

- **Tipe primitif (`string`, `number`)** = bahan dasar: semen, pasir, batu bata
- **Interface** = cetak biru yang mendefinisikan bentuk sebuah ruangan (harus punya pintu, jendela, dinding)
- **Type Alias** = label untuk bahan: "semen tipe-A" adalah alias untuk campuran tertentu
- **Union type (`A | B`)** = pintu bisa kayu ATAU besi
- **Intersection (`A & B`)** = tiang yang harus kuat DAN tahan api
- **Utility types** = alat modifikasi: `Partial` = "ruangan ini boleh tidak punya jendela", `Readonly` = "tiang ini tidak boleh dipotong"

## Dipakai untuk Apa

Semua kode NestJS menggunakan sistem tipe ini:

- **DTO (Data Transfer Object)** — menggunakan `Pick`, `Omit`, `Partial`
- **Entity / Model** — mendefinisikan bentuk data dengan `interface`
- **Service & Controller** — parameter dan return value diberi tipe
- **Decorator & Guard** — union types untuk status, generic types untuk fleksibilitas

## Kesalahan Umum yang Sering Terjadi

1. **Menggunakan `type` untuk objek yang bisa di-extends** — Lebih baik `interface` karena mendukung declaration merging.
2. **Tidak memanfaatkan utility types** — Menulis ulang tipe manual padahal `Pick`/`Omit` sudah cukup.
3. **Overuse `any`** — Membuat TypeScript tidak berguna. Gunakan `unknown` jika benar-benar tidak tahu tipenya.
4. **Confused antara `|` (union) dan `&` (intersection)** — Union = salah satu, Intersection = gabungan.
5. **Lupa `readonly` pada properti yang tidak boleh diubah** — Seperti `id` dan `createdAt`.

## Soal Latihan Beserta Jawaban

### Soal 1
Definisikan tiga tipe berikut menggunakan `interface` atau `type`:

1. **User** — memiliki `id: string`, `name: string`, `email: string`, `role: 'admin' | 'user'`, `isActive?: boolean`
2. **Product** — memiliki `id: string`, `name: string`, `price: number`, `stock: number`, `category: string`
3. **Order** — memiliki `id: string`, `user: User`, `products: Product[]`, `total: number`, `createdAt: Date`

**Jawaban:**

```typescript
interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'user';
  isActive?: boolean;
}

interface Product {
  id: string;
  name: string;
  price: number;
  stock: number;
  category: string;
}

interface Order {
  id: string;
  user: User;
  products: Product[];
  total: number;
  createdAt: Date;
}
```

### Soal 2
Gunakan `Pick` dan `Omit` untuk membuat:

- `UserPublic` — hanya berisi `id`, `name`, `email`
- `ProductInput` — semua field `Product` kecuali `id`

**Jawaban:**

```typescript
type UserPublic = Pick<User, 'id' | 'name' | 'email'>;
type ProductInput = Omit<Product, 'id'>;
```

### Soal 3
Buat tipe `OrderStatus` yang merupakan union dari `'pending' | 'processing' | 'shipped' | 'delivered' | 'cancelled'`.

**Jawaban:**

```typescript
type OrderStatus = 'pending' | 'processing' | 'shipped' | 'delivered' | 'cancelled';
```
