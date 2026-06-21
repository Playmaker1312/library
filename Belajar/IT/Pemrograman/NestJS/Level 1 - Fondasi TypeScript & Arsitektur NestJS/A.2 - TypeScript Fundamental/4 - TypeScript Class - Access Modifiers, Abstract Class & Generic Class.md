# TypeScript Class — Access Modifiers, Abstract Class & Generic Class

## Penjelasan

Di materi sebelumnya kita belajar tentang **bahan bangunan (tipe)** dan **cetak biru (interface)**. Sekarang kita naik level: kita akan membuat **ruangan yang benar-benar berdiri dan berfungsi** — yaitu **Class**.

Dalam konteks NestJS, class adalah fondasi dari hampir semuanya: **Controller** (pintu masuk), **Service** (ruang kerja), **Module** (denah lantai), **Guard** (satpam), **Pipe** (penyaring). Memahami class di TypeScript berarti memahami bagaimana NestJS bekerja di dalamnya.

## Fungsi

- **Encapsulation** — menyembunyikan detail internal dengan access modifiers
- **Abstraction** — mendefinisikan kontrak dengan abstract class
- **Reusability** — membuat class generik yang bisa dipakai untuk berbagai tipe
- **Inheritance** — mewarisi properti dan method dari class lain

## Cara Pengimplementasian

### Class Dasar & Constructor

```typescript
class User {
  name: string;
  email: string;

  constructor(name: string, email: string) {
    this.name = name;
    this.email = email;
  }

  greet(): string {
    return `Halo, saya ${this.name}`;
  }
}

const user = new User('Budi', 'budi@email.com');
console.log(user.greet()); // Halo, saya Budi
```

### Access Modifiers

```typescript
class Building {
  public name: string;        // bisa diakses dari mana saja
  private floors: number;     // hanya di dalam class ini
  protected address: string;  // di dalam class + turunan
  readonly id: string;        // hanya bisa baca, tidak bisa diubah

  constructor(name: string, floors: number, address: string) {
    this.name = name;
    this.floors = floors;
    this.address = address;
    this.id = crypto.randomUUID();
  }

  private calculateTax(): number {
    return this.floors * 1000;
  }

  getTax(): number {
    return this.calculateTax(); // method private bisa dipanggil internal
  }
}

class OfficeBuilding extends Building {
  showAddress(): string {
    return this.address; // ✅ protected bisa diakses di turunan
    // return this.floors; // ❌ Error: private tidak bisa diakses
  }
}

const building = new Building('NestJS Tower', 10, 'Jl. Sudirman');
building.name;       // ✅ public
// building.floors;  // ❌ Error: private
// building.id = '2' // ❌ Error: readonly
```

### Parameter Properties — Shortcut

TypeScript menyediakan cara singkat mendeklarasi properti langsung di constructor:

```typescript
class User {
  constructor(
    public name: string,
    private email: string,
    protected role: string,
    readonly id: string = crypto.randomUUID()
  ) {}
}
```

Kode di atas setara dengan deklarasi manual properti + assignment di constructor.

### Abstract Class

Abstract class adalah **cetak biru yang belum lengkap** — class ini tidak bisa di-instance langsung, tapi mewajibkan class turunan untuk mengimplementasikan method tertentu.

```typescript
abstract class DatabaseRepository {
  // Method konkret — sudah ada implementasinya
  connect(): void {
    console.log('Terhubung ke database');
  }

  // Method abstrak — wajib diimplementasikan turunan
  abstract findById(id: string): Promise<unknown>;
  abstract findAll(): Promise<unknown[]>;
  abstract save(data: unknown): Promise<void>;
  abstract delete(id: string): Promise<void>;
}

class UserRepository extends DatabaseRepository {
  async findById(id: string): Promise<User> {
    // implementasi spesifik
    return {} as User;
  }

  async findAll(): Promise<User[]> {
    return [];
  }

  async save(data: User): Promise<void> {
    console.log('User disimpan');
  }

  async delete(id: string): Promise<void> {
    console.log('User dihapus');
  }
}

// const repo = new DatabaseRepository(); // ❌ Error: abstract class
const repo = new UserRepository(); // ✅
repo.connect(); // mewarisi method konkret
```

### Generic Class — `Repository<T>`

Generic class memungkinkan kita membuat **satu class yang bisa bekerja dengan berbagai tipe**. Ini adalah pola yang sangat umum di NestJS (misal `Repository<Entity>` di TypeORM).

```typescript
interface Entity {
  id: string;
}

class BaseRepository<T extends Entity> {
  private items: T[] = [];

  constructor(private storageKey: string) {}

  findById(id: string): T | undefined {
    return this.items.find(item => item.id === id);
  }

  findAll(): T[] {
    return [...this.items];
  }

  save(item: T): void {
    const existing = this.findById(item.id);
    if (existing) {
      this.update(item);
    } else {
      this.items.push(item);
    }
  }

  private update(item: T): void {
    const index = this.items.findIndex(i => i.id === item.id);
    if (index !== -1) {
      this.items[index] = item;
    }
  }

  delete(id: string): void {
    this.items = this.items.filter(item => item.id !== id);
  }

  count(): number {
    return this.items.length;
  }
}
```

Penggunaan:

```typescript
interface User extends Entity {
  name: string;
  email: string;
}

interface Product extends Entity {
  title: string;
  price: number;
}

const userRepo = new BaseRepository<User>('users');
userRepo.save({ id: '1', name: 'Budi', email: 'budi@email.com' });

const productRepo = new BaseRepository<Product>('products');
productRepo.save({ id: 'p1', title: 'Kursi Kantor', price: 1500000 });

const user = userRepo.findById('1'); // tipe: User | undefined
const products = productRepo.findAll(); // tipe: Product[]
```

### Generic dengan Constraint Lebih Kompleks

```typescript
interface Identifiable {
  id: string;
  createdAt: Date;
}

interface SoftDeletable {
  deletedAt?: Date | null;
}

class AdvancedRepository<
  T extends Identifiable,
  U = Partial<T> // tipe kedua opsional
> {
  save(item: T): void {}
  softDelete(id: string): void {}
}
```

## Analogi

Class di TypeScript dalam analogi **gedung bertingkat**:

- **Class** = cetak biru untuk membuat ruangan yang sama berulang kali
- **`public`** = lobi dan area umum — semua orang bisa akses
- **`private`** = ruang mesin dan panel listrik — hanya teknisi internal yang boleh menyentuh
- **`protected`** = ruang staf — staf dan manajer (turunan) bisa akses, tamu tidak
- **`readonly`** = pilar beton — sudah dipasang, tidak bisa dipindahkan
- **Abstract class** = cetak biru parsial — "semua lantai harus punya tangga darurat, tapi bentuk tangganya bebas"
- **Generic class `Repository<T>`** = lemari arsip universal — lemari yang sama bisa diisi dokumen apapun (surat, kontrak, faktur) asalkan punya nomor ID

## Dipakai untuk Apa

Di NestJS, class digunakan di:

- **Controllers** — menangani request HTTP
- **Services** — logika bisnis
- **Repositories** — akses database (via TypeORM / Prisma / Mongoose)
- **Guards** — otentikasi dan otorisasi
- **Pipes** — transformasi dan validasi data
- **Interceptors** — memodifikasi request/response
- **Decorators** — membuat dekorator kustom

## Kesalahan Umum yang Sering Terjadi

1. **Lupa access modifiers** — Semua properti jadi `public` padahal seharusnya `private` atau `protected`. Selalu tentukan access modifier.
2. **Abstract class tanpa abstract method** — Abstract class harus punya minimal satu abstract method, jika tidak sebaiknya pakai class biasa.
3. **Mengabaikan generic constraint** — `T extends Entity` memastikan `T` punya `id`. Jika lupa constraint, kode di dalam class tidak bisa mengakses `item.id`.
4. **Instance abstract class** — Mencoba membuat instance dari abstract class langsung (`new DatabaseRepository()`). Ini error, harus lewat turunan.
5. **Overcomplicate generics** — Membuat generics terlalu kompleks (3+ type parameter) untuk kasus sederhana.

## Soal Latihan Beserta Jawaban

### Soal 1
Buat `BaseRepository<T>` sendiri yang memiliki method `findById`, `findAll`, `save`, dan `delete`. Gunakan constraint `T extends { id: string }`. Simpan data di array internal.

**Jawaban:**

```typescript
interface HasId {
  id: string;
}

class BaseRepository<T extends HasId> {
  private items: T[] = [];

  findById(id: string): T | undefined {
    return this.items.find(item => item.id === id);
  }

  findAll(): T[] {
    return [...this.items];
  }

  save(item: T): void {
    const idx = this.items.findIndex(i => i.id === item.id);
    if (idx >= 0) {
      this.items[idx] = item;
    } else {
      this.items.push(item);
    }
  }

  delete(id: string): void {
    this.items = this.items.filter(item => item.id !== id);
  }
}
```

### Soal 2
Gunakan `BaseRepository` di atas untuk membuat repositori `UserRepository` dan `ProductRepository`. Simpan masing-masing 2 data.

**Jawaban:**

```typescript
interface User extends HasId {
  name: string;
  email: string;
}

interface Product extends HasId {
  title: string;
  price: number;
}

const userRepo = new BaseRepository<User>();
userRepo.save({ id: 'u1', name: 'Ani', email: 'ani@mail.com' });
userRepo.save({ id: 'u2', name: 'Budi', email: 'budi@mail.com' });

const productRepo = new BaseRepository<Product>();
productRepo.save({ id: 'p1', title: 'Laptop', price: 15000000 });
productRepo.save({ id: 'p2', title: 'Mouse', price: 250000 });

console.log(userRepo.findById('u1')); // { id: 'u1', name: 'Ani', email: 'ani@mail.com' }
console.log(productRepo.findAll()); // [Product, Product]
```

### Soal 3
Apa perbedaan `public`, `private`, dan `protected`? Berikan contoh situasi masing-masing.

**Jawaban:**

- **`public`**: Bisa diakses dari mana saja. Contoh: method `greet()` pada class `User` — semua bagian kode boleh memanggilnya.
- **`private`**: Hanya di dalam class yang sama. Contoh: method `calculateTax()` di class `Building` — hanya digunakan internal class untuk menghitung pajak, tidak boleh dipanggil dari luar.
- **`protected`**: Di dalam class dan turunannya. Contoh: properti `address` di class `Building` — class `OfficeBuilding` yang merupakan turunan boleh mengaksesnya, tetapi kode di luar hierarchy tidak boleh.
