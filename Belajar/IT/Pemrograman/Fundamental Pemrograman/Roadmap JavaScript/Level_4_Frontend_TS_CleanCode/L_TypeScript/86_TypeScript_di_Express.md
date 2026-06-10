# 86. TypeScript di Express.js

**Benang Merah**: Materi 85 (Generics) + Express backend (Level 3). Setelah ini lanjut ke Materi 87 (TS di Vue) — backend & frontend semuanya type-safe.

---

## Penjelasan

Mengintegrasikan TypeScript ke Express.js memberi kita **type safety** dari request masuk hingga response keluar. Dengan TS, kita tahu persis bentuk `req.body`, `req.params`, `req.query`, dan struktur response — tanpa harus console.log atau baca dokumentasi setiap kali.

### Setup TS di Express
```bash
npm install express
npm install -D typescript @types/node @types/express ts-node nodemon
npx tsc --init
```

Ubah `tsconfig.json`:
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  }
}
```

### Typing Request, Response, Next
```typescript
import express, { Request, Response, NextFunction } from "express";

const app = express();
app.use(express.json());

app.get("/api/users", (req: Request, res: Response, next: NextFunction) => {
  // ...
});
```

### Request Body Typing
Kita perlu mendefinisikan bentuk body yang diharapkan:

```typescript
interface CreateUserBody {
  nama: string;
  email: string;
  umur: number;
}

app.post("/api/users", (req: Request<{}, {}, CreateUserBody>, res: Response) => {
  const { nama, email, umur } = req.body; // ✅ semua sudah ber-tipe
});
```

### Type Response
Response juga bisa di-typed:

```typescript
import { Response } from "express";

interface ApiResponse<T> {
  status: "success" | "error";
  data: T;
  message?: string;
}

function sendSuccess<T>(res: Response, data: T, message?: string) {
  const response: ApiResponse<T> = { status: "success", data, message };
  res.json(response);
}
```

---

## Fungsi

**Mencegah bug tipe di API** — bentuk request, response, dan error sudah diketahui saat compile time. Tidak ada lagi "cannot read property of undefined" karena lupa field.

---

## Code

```typescript
// src/index.ts
import express, { Request, Response, NextFunction } from "express";
import { Buku, Anggota, Peminjaman } from "./types";

const app = express();
app.use(express.json());

// ========== DATABASE SEMENTARA ==========
let daftarBuku: Buku[] = [];
let daftarAnggota: Anggota[] = [];
let daftarPeminjaman: Peminjaman[] = [];
let nextId = 1;

// ========== HELPER RESPONSE ==========
interface ApiResponse<T> {
  status: "success" | "error";
  data: T;
  message?: string;
}

function success<T>(res: Response, data: T, message?: string) {
  const result: ApiResponse<T> = { status: "success", data, message };
  res.json(result);
}

function error<T>(res: Response, statusCode: number, message: string) {
  const result: ApiResponse<null> = { status: "error", data: null, message };
  res.status(statusCode).json(result);
}

// ========== CRUD BUKU ==========
interface CreateBukuBody {
  judul: string;
  penulis: string;
  isbn: string;
  tahunTerbit: number;
}

app.get("/api/buku", (_req: Request, res: Response) => {
  success(res, daftarBuku);
});

app.get("/api/buku/:id", (req: Request<{ id: string }>, res: Response) => {
  const buku = daftarBuku.find(b => b.id === parseInt(req.params.id));
  if (!buku) return error(res, 404, "Buku tidak ditemukan");
  success(res, buku);
});

app.post("/api/buku", (req: Request<{}, {}, CreateBukuBody>, res: Response) => {
  const { judul, penulis, isbn, tahunTerbit } = req.body;
  const bukuBaru: Buku = { id: nextId++, judul, penulis, isbn, tahunTerbit, tersedia: true };
  daftarBuku.push(bukuBaru);
  success(res, bukuBaru, "Buku berhasil ditambahkan");
});

app.put("/api/buku/:id", (req: Request<{ id: string }, {}, Partial<CreateBukuBody>>, res: Response) => {
  const index = daftarBuku.findIndex(b => b.id === parseInt(req.params.id));
  if (index === -1) return error(res, 404, "Buku tidak ditemukan");
  daftarBuku[index] = { ...daftarBuku[index], ...req.body };
  success(res, daftarBuku[index], "Buku berhasil diupdate");
});

app.delete("/api/buku/:id", (req: Request<{ id: string }>, res: Response) => {
  const index = daftarBuku.findIndex(b => b.id === parseInt(req.params.id));
  if (index === -1) return error(res, 404, "Buku tidak ditemukan");
  daftarBuku.splice(index, 1);
  success(res, null, "Buku berhasil dihapus");
});

// ========== MIDDLEWARE TYPE-SAFE ==========
function logger(req: Request, _res: Response, next: NextFunction): void {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.path}`);
  next();
}

function errorHandler(err: Error, _req: Request, res: Response, _next: NextFunction): void {
  console.error("Error:", err.message);
  error(res, 500, "Terjadi kesalahan internal");
}

app.use(logger);
app.use(errorHandler);

const PORT = 3000;
app.listen(PORT, () => console.log(`Server jalan di port ${PORT}`));
```

```typescript
// src/types.ts
export type StatusAnggota = "aktif" | "nonaktif" | "diblokir";

export interface Buku {
  id: number;
  judul: string;
  penulis: string;
  isbn: string;
  tahunTerbit: number;
  tersedia: boolean;
}

export interface Anggota {
  id: number;
  nama: string;
  alamat: string;
  noTelepon: string;
  status: StatusAnggota;
  tanggalDaftar: Date;
}

export interface Peminjaman {
  id: number;
  anggotaId: number;
  bukuId: number;
  tanggalPinjam: Date;
  tanggalKembali?: Date;
  denda?: number;
}
```

---

## Analogi: Membangun Rumah (Blueprint Digital)

| TypeScript Express | Analogi Rumah |
|---|---|
| `Request<Params, ResBody, ReqBody>` | Blueprint detail ukuran pintu, jendela |
| `Response<ApiResponse<T>>` | Spesifikasi hasil akhir yang dijamin |
| `next()` | Serah terima antar tukang |
| Middleware typed | Tahapan work breakdown structure (WBS) |
| Compiler cek sebelum run | Inspektur periksa sebelum bangun |
| `@types/express` | Buku standar ukuran material nasional |

Bayangkan Anda punya **blueprint digital lengkap** — setiap ruangan, setiap material, setiap ukuran sudah tertulis detail. Tukang (compiler) bisa memeriksa apakah blueprint masuk akal **sebelum** pembangunan dimulai. Tidak ada lagi tebak-tebakan di lapangan. Itulah yang TypeScript lakukan untuk Express API Anda.

---

## Use Case

- REST API production dengan tim besar
- Microservices dengan kontrak antar service
- API yang perlu di-refactor sering
- Dokumentasi hidup — tipe sudah menjelaskan bentuk data

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Tidak install `@types/express` | Lupa dev dependency | Error TS tidak kenal Express |
| `any` di req.body | `req: Request` tanpa generic | Body tidak ter-validasi |
| Tidak typing response | `res.json({})` tanpa interface | Response bisa sembarangan |
| Lupa `esModuleInterop` | `import * as express` | Import bermasalah |
| Strict mode mati | `strict: false` di tsconfig | Banyak bug lolos |

---

## Benang Merah

Materi 85 (Generics) + Express (Level 3) → **Materi 86 (TS di Express)** → Materi 87 (TS di Vue)

Backend sudah type-safe dengan Express + TS. Sekarang saatnya membuat **frontend Vue juga type-safe** — sehingga seluruh stack Anda diamankan TypeScript.

---

## Soal Latihan

### Soal 1 (Mudah)
Buat route GET `/api/hello` yang menerima query parameter `nama: string` dan mengembalikan `{ message: "Halo, {nama}!" }` menggunakan TypeScript.

**Jawaban**:
```typescript
import express, { Request, Response } from "express";

const app = express();

app.get("/api/hello", (req: Request<{}, {}, {}, { nama: string }>, res: Response) => {
  const { nama } = req.query;
  res.json({ message: `Halo, ${nama}!` });
});

app.listen(3000);
```

### Soal 2 (Sedang)
Buat interface `CreateBookBody` dan route POST `/api/books` yang menerima body dengan properti: `title` (string), `author` (string), `year` (number). Validasi jika `year` kurang dari 1800, return error 400.

**Jawaban**:
```typescript
interface CreateBookBody {
  title: string;
  author: string;
  year: number;
}

app.post("/api/books", (req: Request<{}, {}, CreateBookBody>, res: Response) => {
  const { title, author, year } = req.body;
  if (year < 1800) {
    return res.status(400).json({ error: "Tahun harus >= 1800" });
  }
  res.json({ message: `Buku "${title}" oleh ${author} (${year}) berhasil ditambahkan` });
});
```

### Soal 3 (Tantangan)
Buat generic type-safe middleware `validateBody<T>` yang memvalidasi keberadaan properti yang diberikan. Middleware menerima array key yang harus ada di `req.body` dan mengembalikan 400 jika ada yang kurang. Implementasikan dengan TypeScript generics.

**Jawaban**:
```typescript
import { Request, Response, NextFunction } from "express";

function validateBody<T extends object>(...keys: (keyof T)[]) {
  return (req: Request<{}, {}, T>, res: Response, next: NextFunction): void => {
    const missing = keys.filter(k => !(k in req.body));
    if (missing.length > 0) {
      res.status(400).json({ error: `Field wajib: ${missing.join(", ")}` });
      return;
    }
    next();
  };
}

interface UserBody {
  nama: string;
  email: string;
  umur: number;
}

app.post("/api/users", validateBody<UserBody>("nama", "email", "umur"), (req: Request<{}, {}, UserBody>, res: Response) => {
  res.json({ message: `User ${req.body.nama} dibuat` });
});
```
