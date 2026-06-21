# Async TypeScript — Promise, async/await & Error Typing

## Penjelasan

Setelah kita punya **ruangan (class)** dan **lemari arsip (generic repository)**, sekarang saatnya belajar bagaimana **mengirim dokumen antar lantai** secara efisien. Di dunia nyata, kita tidak bisa langsung melempar dokumen dari lantai 1 ke lantai 10 — butuh lift atau pneumatic tube. Di pemrograman, "lift" itu adalah **async/await**.

NestJS adalah framework **asynchronous dari ujung ke ujung** — setiap request ke controller, query ke database, atau panggilan ke API eksternal semuanya async. Memahami async TypeScript adalah **syarat mutlak** sebelum menulis kode NestJS.

## Fungsi

- **Non-blocking execution** — kode tidak berhenti menunggu operasi lambat (database, HTTP call, file I/O)
- **Error handling terstruktur** — dengan `try/catch` yang rapi
- **Typed errors** — menangkap error dengan tipe yang spesifik
- **Composition** — menggabungkan beberapa async operation dengan `Promise.all`

## Cara Pengimplementasian

### Promise — Janji

Promise adalah objek yang merepresentasikan **nilai di masa depan** — bisa berhasil (resolve) atau gagal (reject).

```typescript
const promise: Promise<string> = new Promise((resolve, reject) => {
  const success = true;

  setTimeout(() => {
    if (success) {
      resolve('Data berhasil dimuat');
    } else {
      reject(new Error('Gagal memuat data'));
    }
  }, 1000);
});

// Menggunakan .then/.catch
promise
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

### async/await — Sintaks Modern

`async/await` adalah **gula sintaksis** di atas Promise yang membuat kode async terlihat seperti kode synchronous.

```typescript
async function loadData(): Promise<string> {
  const result = await promise; // tunggu sampai selesai
  return result;
}

// Di dalam class
class DataService {
  async fetchData(): Promise<string> {
    const data = await promise;
    return data;
  }
}
```

### Typed Promise — `Promise<T>`

Setiap async function mengembalikan `Promise<T>`. Tipe `T` adalah tipe data yang akan di-resolve.

```typescript
interface User {
  id: string;
  name: string;
  email: string;
}

// Database mock
const users: User[] = [
  { id: '1', name: 'Budi', email: 'budi@email.com' },
  { id: '2', name: 'Ani', email: 'ani@email.com' },
];

function fetchUser(id: string): Promise<User> {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const user = users.find(u => u.id === id);
      if (user) {
        resolve(user);
      } else {
        reject(new Error(`User dengan id ${id} tidak ditemukan`));
      }
    }, 500);
  });
}
```

### Custom Error Types

Buat class error sendiri agar lebih spesifik:

```typescript
class NotFoundError extends Error {
  constructor(entity: string, id: string) {
    super(`${entity} dengan id ${id} tidak ditemukan`);
    this.name = 'NotFoundError';
  }
}

class ValidationError extends Error {
  constructor(field: string, message: string) {
    super(`Validasi gagal pada field ${field}: ${message}`);
    this.name = 'ValidationError';
  }
}

class DatabaseError extends Error {
  constructor(operation: string, cause?: unknown) {
    super(`Database error saat ${operation}: ${cause}`);
    this.name = 'DatabaseError';
  }
}
```

### try/catch dengan Typed Error

```typescript
async function fetchUser(id: string): Promise<User> {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const user = users.find(u => u.id === id);
      if (!user) {
        reject(new NotFoundError('User', id));
        return;
      }
      if (!user.email.includes('@')) {
        reject(new ValidationError('email', 'format email tidak valid'));
        return;
      }
      resolve(user);
    }, 500);
  });
}

async function displayUser(id: string): Promise<void> {
  try {
    const user = await fetchUser(id);
    console.log('User ditemukan:', user.name);
  } catch (error) {
    if (error instanceof NotFoundError) {
      console.error('404:', error.message);
    } else if (error instanceof ValidationError) {
      console.error('400:', error.message);
    } else {
      console.error('500: Internal server error');
    }
  }
}
```

### Type Guard untuk Error

Kita bisa membuat type guard untuk mendeteksi tipe error:

```typescript
function isNotFoundError(error: unknown): error is NotFoundError {
  return error instanceof NotFoundError;
}

async function getUserName(id: string): Promise<string> {
  try {
    const user = await fetchUser(id);
    return user.name;
  } catch (error) {
    if (isNotFoundError(error)) {
      return 'Anonymous';
    }
    throw error; // lempar ulang error yang tidak dikenali
  }
}
```

### Promise.all — Eksekusi Paralel

```typescript
async function getDashboardData(): Promise<{
  user: User;
  stats: number[];
  config: Record<string, unknown>;
}> {
  const [user, stats, config] = await Promise.all([
    fetchUser('1'),
    Promise.resolve([10, 20, 30]),
    Promise.resolve({ theme: 'dark' }),
  ]);

  return { user, stats, config };
}
```

### Contoh Lengkap: Service Layer

```typescript
interface User {
  id: string;
  name: string;
  email: string;
}

// Custom errors
class NotFoundError extends Error {
  constructor(entity: string, id: string) {
    super(`${entity} #${id} tidak ditemukan`);
    this.name = 'NotFoundError';
  }
}

class DuplicateError extends Error {
  constructor(entity: string, field: string, value: string) {
    super(`${entity} dengan ${field} '${value}' sudah ada`);
    this.name = 'DuplicateError';
  }
}

// Service
class UserService {
  private users: User[] = [];

  async findById(id: string): Promise<User> {
    const user = this.users.find(u => u.id === id);
    if (!user) {
      throw new NotFoundError('User', id);
    }
    return user;
  }

  async create(data: Omit<User, 'id'>): Promise<User> {
    const exists = this.users.find(u => u.email === data.email);
    if (exists) {
      throw new DuplicateError('User', 'email', data.email);
    }

    const user: User = {
      id: crypto.randomUUID(),
      ...data,
    };
    this.users.push(user);
    return user;
  }

  async findAll(): Promise<User[]> {
    return [...this.users];
  }
}

// Usage
async function main() {
  const service = new UserService();

  try {
    const newUser = await service.create({
      name: 'Budi',
      email: 'budi@email.com',
    });
    console.log('Created:', newUser);

    const found = await service.findById(newUser.id);
    console.log('Found:', found.name);

    await service.findById('nonexistent'); // throw NotFoundError
  } catch (error) {
    if (error instanceof NotFoundError) {
      console.error('Not found:', error.message);
    } else if (error instanceof DuplicateError) {
      console.error('Duplicate:', error.message);
    } else {
      console.error('Unexpected error:', error);
    }
  }
}
```

## Analogi

Async/await dalam analogi **gedung bertingkat**:

- **Promise** = "nomer antrian" untuk lift barang — kita dapat nomor, lanjut kerja lain, nanti dipanggil kalau lift sudah sampai
- **async function** = lantai yang punya sistem lift terpasang
- **`await`** = menekan tombol lift dan menunggu pintu terbuka — kode terlihat berhenti, tapi gedung (program) tetap berjalan di bagian lain
- **`Promise.all`** = memanggil 3 lift sekaligus untuk 3 lantai berbeda — semuanya berjalan paralel
- **Custom Error (`NotFoundError`)** = tanda "RUANGAN TIDAK DITEMUKAN" di pintu — spesifik, bukan hanya "ERROR" generik
- **try/catch** = jaring pengaman di sisi gedung — kalau ada yang jatuh, tertangkap dan ditangani sesuai jenisnya

Tanpa async/await, kita seperti **menatap lift sampai pintu terbuka** — semua pekerjaan lain berhenti. Dengan async/await, kita tekan tombol, lanjut kerja, dan kembali saat lift sudah tiba.

## Dipakai untuk Apa

Async/await dipakai di **setiap bagian NestJS**:

- **Controller** — handler endpoint async
- **Service** — logika bisnis async
- **Guard** — validasi token async (cek ke database)
- **Interceptor** — transformasi response async
- **Pipe** — validasi async (cek duplikasi ke database)
- **Database query** — Prisma, TypeORM, Mongoose semuanya async

## Kesalahan Umum yang Sering Terjadi

1. **Lupa `await`** — Function mengembalikan `Promise<Pending>` bukan `User`. Ini penyebab bug paling umum.
2. **Error tanpa tipe spesifik** — Selalu gunakan `throw new NotFoundError(...)` bukan `throw new Error(...)` agar penanganan error lebih granular.
3. **try/catch terlalu lebar** — Membungkus seluruh function dalam satu try/catch sehingga error tidak bisa di-handle secara spesifik.
4. **Mengabaikan error path** — Tidak semua jalur kode di-handle, misal lupa `return` setelah `reject`.
5. **`Promise.all` tanpa `catch`** — Satu reject di `Promise.all` akan menggagalkan semuanya. Gunakan `Promise.allSettled` jika perlu handle partial failure.

## Soal Latihan Beserta Jawaban

### Soal 1
Buat fungsi `fetchUser(id: string): Promise<User>` yang mengembalikan data user dari array. Jika user tidak ditemukan, lempar `NotFoundError` dengan pesan "User dengan id {id} tidak ditemukan". Tangani error di fungsi pemanggil.

**Jawaban:**

```typescript
class NotFoundError extends Error {
  constructor(entity: string, id: string) {
    super(`${entity} dengan id ${id} tidak ditemukan`);
    this.name = 'NotFoundError';
  }
}

interface User {
  id: string;
  name: string;
  email: string;
}

const users: User[] = [
  { id: '1', name: 'Budi', email: 'budi@test.com' },
  { id: '2', name: 'Ani', email: 'ani@test.com' },
];

function fetchUser(id: string): Promise<User> {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const user = users.find(u => u.id === id);
      if (user) {
        resolve(user);
      } else {
        reject(new NotFoundError('User', id));
      }
    }, 300);
  });
}

async function main() {
  try {
    const user = await fetchUser('1');
    console.log('Sukses:', user.name);
  } catch (error) {
    if (error instanceof NotFoundError) {
      console.error(error.message);
    } else {
      console.error('Error tak dikenal:', error);
    }
  }
}
```

### Soal 2
Buat fungsi `getUserWithPosts(userId: string)` yang mengambil user dan post secara paralel menggunakan `Promise.all`. Gunakan mock data.

**Jawaban:**

```typescript
interface Post {
  id: string;
  userId: string;
  title: string;
}

function fetchUser(id: string): Promise<User> {
  return Promise.resolve(users.find(u => u.id === id)!);
}

function fetchPostsByUser(userId: string): Promise<Post[]> {
  const posts: Post[] = [
    { id: 'p1', userId: '1', title: 'Belajar NestJS' },
    { id: 'p2', userId: '1', title: 'TypeScript Lanjutan' },
  ];
  return Promise.resolve(posts.filter(p => p.userId === userId));
}

async function getUserWithPosts(userId: string) {
  const [user, posts] = await Promise.all([
    fetchUser(userId),
    fetchPostsByUser(userId),
  ]);
  return { user, posts };
}
```

### Soal 3
Apa yang terjadi jika sebuah Promise di-reject tetapi tidak ada `.catch()`?

**Jawaban:** Akan terjadi **unhandled promise rejection**. Di lingkungan Node.js, ini akan memicu `process.on('unhandledRejection')` dan bisa menyebabkan process crash. Selalu tangani error dengan `.catch()` atau `try/catch`.
