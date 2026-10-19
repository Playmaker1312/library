# 🗺 Roadmap Node.js: Step-by-Step Membangun Aplikasi Web Nyata

## Filosofi Roadmap Ini

"Node.js adalah JavaScript di server-side — event-driven, non-blocking I/O, dan single-threaded async — setiap konsep dipelajari dengan pemahaman alasan mengapa async-first adalah default mindset"

---

## Prinsip Desain

- **Satu Project, Tumbuh Bersama**: Sistem manajemen perpustakaan dari CLI → Express API → Real-time dengan WebSocket → Microservices
- **Async/Await First**: Tidak ada callback hell — modern async patterns sejak awal
- **NPM Ecosystem**: Manfaatkan kekayaan npm packages, tapi pahami kapan harus custom code
- **Node.js Modern**: Fokus pada Node.js 18+, dengan ES modules, native fetch, top-level await

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: "Perpustakaan CLI" — JavaScript dasar, async/await introduction
 ↓ (enhance)
Level 2: + Node.js core modules, file system, JSON → CLI dengan file persistence
 ↓ (enhance)
Level 3: + Express.js, routing, middleware → Simple web server
 ↓ (enhance)
Level 4: + Database (MongoDB/MySQL), Mongoose/Sequelize → Data persistence
 ↓ (enhance)
Level 5: + REST API, validation, error handling → Robust API
 ↓ (enhance)
Level 6: + Testing, authentication, advanced patterns → Production-ready
 ↓ (enhance)
Level 7: + Real-time (Socket.IO), caching, deployment → Scale to production
 ↓ (pilih jalur)
Level 8: Microservices / gRPC / Serverless
```

---

# 🟢 LEVEL 1: JAVASCRIPT & ASYNC FUNDAMENTALS (Minggu 1-3)

**Tema**: "JavaScript di server — Event loop, Promises, Async/Await"

**Benang Merah**: JavaScript basics → Promises → Async/Await (modern way)

**Output**: CLI script perpustakaan yang menggunakan async operations

---

## A. Setup Node.js & JavaScript Basics

### 1. [[1. Node.js Setup dan Konsep Fundamental]]

Node.js adalah **JavaScript runtime** yang:

- ✅ Run JavaScript di server (tidak di browser)
- ✅ Event-driven, non-blocking I/O
- ✅ Single-threaded (tapi async handling)
- ✅ Huge npm ecosystem

**Install Node.js**:

Bash

```
# Download dari nodejs.org (LTS version recommended)
node --version
npm --version
```

**Perbedaan dari PHP**:

- PHP: Synchronous (tunggu setiap operasi selesai)
- Node.js: Asynchronous (lanjut ke operasi berikutnya, callback saat selesai)

**Langkah konkret**:

Bash

```
mkdir -p ~/projects/perpustakaan-nodejs
cd ~/projects/perpustakaan-nodejs
npm init -y  # Create package.json
```

### 2. [[2. Program "Hello, Perpustakaan" Pertama]]

**File: index.js**

JavaScript

```
console.log("Selamat datang di Perpustakaan Digital");
console.log("Versi: 1.0.0");
console.log("Node version:", process.version);
```

**Jalankan:**

Bash

```
node index.js
```

**Langkah konkret**: Buat program yang menampilkan welcome message dan info tentang perpustakaan.

### 3. [[3. Variables, Types, dan Operators]]

JavaScript adalah **dynamically typed** (seperti PHP).

JavaScript

```
// Variables (var, let, const)
var namaOld = "Perpustakaan Digital";  // Hindari, punya hoisting issues
let nama = "Perpustakaan Digital";     // Block-scoped, modern
const kapasitas = 1000;                // Const, tidak bisa diubah

// Tipe data
const namaPerpustakaan = "Perpustakaan Digital";  // string
const jumlahBuku = 150;                           // number
const rating = 4.8;                               // number (float)
const aktif = true;                               // boolean
const koleksi = null;                             // null
let undefined_var;                                // undefined
const buku = {};                                  // object (associative array)
const kategoris = [];                             // array

console.log(typeof namaPerpustakaan);  // "string"
console.log(typeof jumlahBuku);        // "number"
console.log(Array.isArray(kategoris)); // true

// String operations
const judul = "Clean Code";
const pengarang = "Robert Martin";
const info = `${judul} by ${pengarang}`;  // Template literals
console.log(info);  // Clean Code by Robert Martin

// Operators
const harga = 150000;
const diskon = 0.10;
const hargaFinal = harga * (1 - diskon);  // 135000

// Comparison (dalam JavaScript, gunakan === bukan ==)
console.log(1 == "1");   // true (loose comparison — BAD)
console.log(1 === "1");  // false (strict comparison — GOOD)
```

**Langkah konkret**: Buat program dengan variabel untuk data perpustakaan.

---

## B. Promises dan Async/Await

### 4. [[4. Callbacks (The Old Way — Hindari)]]

JavaScript

```
// Callback hell (tidak disarankan lagi)
function bacaFile(filename, callback) {
    setTimeout(() => {
        callback(null, "File content");
    }, 1000);
}

bacaFile("katalog.json", (err, data) => {
    if (err) {
        console.error("Error:", err);
        return;
    }
    console.log("Data:", data);
});
```

**Masalah**: Callback hell, error handling merepotkan, kode tidak linear.

### 5. [[5. Promises (The Better Way)]]

JavaScript

```
// Promise adalah abstraksi untuk async operation yang bisa resolve atau reject
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve("Success!");
        // atau reject("Error!");
    }, 1000);
});

promise
    .then(result => console.log(result))       // Handle success
    .catch(error => console.error(error))      // Handle error
    .finally(() => console.log("Done"));       // Always run
```

**Promise chaining:**

JavaScript

```
fetchBuku(1)
    .then(buku => {
        console.log("Buku:", buku);
        return fetchPeminjaman(buku.id);  // Chain promises
    })
    .then(peminjaman => {
        console.log("Peminjaman:", peminjaman);
    })
    .catch(err => console.error("Error:", err));
```

### 6. [[6. Async/Await (The Modern Way — GUNAKAN INI)]]

**Async/Await adalah syntactic sugar untuk Promises** — lebih readable dan mudah dipahami.

JavaScript

```
// async function selalu return Promise
async function getBookInfo() {
    try {
        const buku = await fetchBuku(1);        // Tunggu resolve
        console.log("Buku:", buku);
        
        const peminjaman = await fetchPeminjaman(buku.id);
        console.log("Peminjaman:", peminjaman);
        
        return { buku, peminjaman };
    } catch (error) {
        console.error("Error:", error);
        throw error;  // Propagate error
    } finally {
        console.log("Done");
    }
}

// Call async function (return Promise)
getBookInfo()
    .then(result => console.log("Result:", result))
    .catch(err => console.error("Error:", err));

// Atau di top-level (Node 14.8+):
(async () => {
    try {
        const result = await getBookInfo();
        console.log("Result:", result);
    } catch (error) {
        console.error("Error:", error);
    }
})();

// Atau di module top-level (Node 12.20+, dengan "type": "module" di package.json):
const result = await getBookInfo();
console.log("Result:", result);
```

**Langkah konkret**: Buat functions yang simulasi async operations (setTimeout, Promise).

---

## C. Array Methods dan Higher-Order Functions

### 7. [[7. Array Methods: map, filter, reduce]]

JavaScript

```
const katalog = [
    { id: 1, judul: "Clean Code", harga: 150000, stok: 5 },
    { id: 2, judul: "Design Patterns", harga: 200000, stok: 3 },
    { id: 3, judul: "Algorithms", harga: 120000, stok: 0 },
];

// map: transform setiap elemen
const judul = katalog.map(buku => buku.judul);
console.log(judul);  // ["Clean Code", "Design Patterns", "Algorithms"]

// filter: pilih elemen yang match kondisi
const tersedia = katalog.filter(buku => buku.stok > 0);
console.log(tersedia.length);  // 2

// find: cari satu elemen
const cleanCode = katalog.find(buku => buku.id === 1);
console.log(cleanCode.judul);  // "Clean Code"

// reduce: akumulasi ke satu nilai
const totalHarga = katalog.reduce((sum, buku) => sum + buku.harga, 0);
console.log(totalHarga);  // 470000

// Chain operations
const mahal = katalog
    .filter(b => b.stok > 0)
    .map(b => b.harga)
    .reduce((max, harga) => Math.max(max, harga), 0);
console.log(mahal);  // 200000

// some, every
console.log(katalog.some(b => b.stok === 0));   // true (ada yang habis)
console.log(katalog.every(b => b.harga > 0));   // true (semua berharga)
```

**Langkah konkret**: Practice dengan berbagai array operations pada data katalog.

### 8. [[8. Destructuring dan Spread Operator]]

JavaScript

```
// Destructuring object
const buku = { id: 1, judul: "Clean Code", pengarang: "Robert Martin" };
const { id, judul, pengarang } = buku;
console.log(judul);  // "Clean Code"

// Destructuring dengan rename
const { judul: bukuJudul } = buku;
console.log(bukuJudul);  // "Clean Code"

// Destructuring array
const [first, second] = katalog;
console.log(first.judul);  // "Clean Code"

// Spread operator (...)
const baru = { ...buku, rating: 4.8 };
console.log(baru);  // { id: 1, judul: "Clean Code", ..., rating: 4.8 }

const combined = [...katalog, { id: 4, judul: "New Book" }];
console.log(combined.length);  // 4
```

---

## 🏗 Checkpoint Level 1

text

```
PEMAHAMAN:
├── Perbedaan synchronous vs asynchronous JavaScript
├── Perbedaan callbacks vs Promises vs Async/Await
├── Kapan gunakan Async/Await (jawab: almost always)
├── Array methods: map, filter, find, reduce
└── Destructuring dan spread operator

PROYEK: Perpustakaan CLI (async version)
├── index.js dengan async main function
├── Simulasi async operations dengan setTimeout/Promise
├── Data katalog 10 buku dalam array
├── Fungsi: searchBuku(keyword), sortBuku(field), getTotalHarga()
├── Gunakan array methods (map, filter, reduce)
├── Proper error handling dengan try-catch
└── Console output yang formatted

HABIT:
├── SELALU gunakan async/await (bukan callbacks)
├── SELALU gunakan === (bukan ==)
├── const sebagai default (bukan let/var)
├── Template literals untuk string dengan variabel
└── Descriptive variable names
```

**Commit**: `feat: setup Node.js project with async/await basics`

---

# 🔵 LEVEL 2: NODE.JS CORE & FILE SYSTEM (Minggu 3-6)

**Tema**: "Dari JavaScript basics ke Node.js specific features"

**Benang Merah**: Async/Await patterns → Node.js modules (fs, path) → File-based data persistence

**Output**: CLI perpustakaan yang bisa menyimpan/load data dari JSON file

---

## D. Node.js Modules dan Require/Import

### 9. [[9. CommonJS vs ES Modules]]

Node.js support dua module systems. **Gunakan ES Modules** (modern standard).

**package.json setup untuk ES Modules:**

JSON

```
{
  "type": "module",
  "name": "perpustakaan",
  "version": "1.0.0"
}
```

**File: modules/buku.js**

JavaScript

```
// Export
export class Buku {
    constructor(id, judul, pengarang, harga, stok) {
        this.id = id;
        this.judul = judul;
        this.pengarang = pengarang;
        this.harga = harga;
        this.stok = stok;
    }
    
    tersedia() {
        return this.stok > 0;
    }
    
    pinjam() {
        if (!this.tersedia()) return false;
        this.stok--;
        return true;
    }
    
    kembalikan() {
        this.stok++;
    }
}

export function formatHarga(harga) {
    return `Rp ${harga.toLocaleString('id-ID')}`;
}

// Export default
export default Buku;
```

**File: index.js**

JavaScript

```
// Import
import Buku, { formatHarga } from './modules/buku.js';

const buku = new Buku(1, "Clean Code", "Robert Martin", 150000, 5);
console.log(buku.judul);           // "Clean Code"
console.log(buku.tersedia());      // true
console.log(formatHarga(150000));  // "Rp 150.000"
```

**Langkah konkret**: Refactor Level 1 code ke modules, separate concerns.

### 10. [[10. File System (fs) Module — Synchronous vs Asynchronous]]

JavaScript

```
import fs from 'fs';
import path from 'path';

// Synchronous (HINDARI untuk production)
const content = fs.readFileSync('katalog.json', 'utf-8');
console.log(content);

// Asynchronous dengan callback (old way)
fs.readFile('katalog.json', 'utf-8', (err, data) => {
    if (err) {
        console.error("Error:", err);
        return;
    }
    console.log(data);
});

// Asynchronous dengan Promise (Node 15+)
fs.promises.readFile('katalog.json', 'utf-8')
    .then(data => console.log(data))
    .catch(err => console.error("Error:", err));

// Asynchronous dengan Async/Await (GUNAKAN INI)
async function bacaKatalog() {
    try {
        const data = await fs.promises.readFile('katalog.json', 'utf-8');
        console.log(data);
        return JSON.parse(data);
    } catch (error) {
        console.error("Error:", error);
        throw error;
    }
}

// Write file
async function simpanKatalog(katalog) {
    try {
        const json = JSON.stringify(katalog, null, 2);
        await fs.promises.writeFile('katalog.json', json, 'utf-8');
        console.log("Katalog saved");
    } catch (error) {
        console.error("Error:", error);
    }
}
```

**Path module:**

JavaScript

```
import path from 'path';

const filepath = path.join(process.cwd(), 'data', 'katalog.json');
console.log(filepath);  // /home/user/perpustakaan/data/katalog.json

const dir = path.dirname(filepath);
const basename = path.basename(filepath);
const ext = path.extname(filepath);
```

**Langkah konkret**: Buat fungsi untuk load/save katalog dari JSON file.

---

## E. Building a File-Based Repository

### 11. [[11. Repository Pattern untuk Data Persistence]]

**File: modules/repository.js**

JavaScript

```
import fs from 'fs';
import path from 'path';

export class FileRepository {
    constructor(filepath) {
        this.filepath = filepath;
        this.ensureFile();
    }
    
    ensureFile() {
        if (!fs.existsSync(this.filepath)) {
            fs.writeFileSync(this.filepath, '[]', 'utf-8');
        }
    }
    
    async loadAll() {
        try {
            const data = await fs.promises.readFile(this.filepath, 'utf-8');
            return JSON.parse(data);
        } catch (error) {
            console.error("Error loading:", error);
            return [];
        }
    }
    
    async save(data) {
        try {
            const json = JSON.stringify(data, null, 2);
            await fs.promises.writeFile(this.filepath, json, 'utf-8');
            return true;
        } catch (error) {
            console.error("Error saving:", error);
            return false;
        }
    }
    
    async findById(id) {
        const all = await this.loadAll();
        return all.find(item => item.id === id);
    }
    
    async create(item) {
        const all = await this.loadAll();
        const maxId = Math.max(...all.map(i => i.id || 0), 0);
        item.id = maxId + 1;
        all.push(item);
        await this.save(all);
        return item;
    }
    
    async update(id, updates) {
        const all = await this.loadAll();
        const index = all.findIndex(item => item.id === id);
        if (index === -1) return null;
        
        all[index] = { ...all[index], ...updates };
        await this.save(all);
        return all[index];
    }
    
    async delete(id) {
        const all = await this.loadAll();
        const filtered = all.filter(item => item.id !== id);
        if (filtered.length === all.length) return false;
        
        await this.save(filtered);
        return true;
    }
}
```

**File: index.js**

JavaScript

```
import { FileRepository } from './modules/repository.js';

const repo = new FileRepository('data/katalog.json');

async function main() {
    // Create
    const buku = await repo.create({
        judul: "Clean Code",
        pengarang: "Robert Martin",
        harga: 150000,
        stok: 5
    });
    console.log("Created:", buku);
    
    // Read
    const semua = await repo.loadAll();
    console.log("Total:", semua.length);
    
    // Update
    const updated = await repo.update(buku.id, { stok: 3 });
    console.log("Updated:", updated);
    
    // Delete
    const deleted = await repo.delete(buku.id);
    console.log("Deleted:", deleted);
}

main().catch(console.error);
```

**Langkah konkret**: Test CRUD operations, verify JSON file structure.

### 12. [[12. Error Handling dan Logging]]

JavaScript

```
class FileRepository {
    // ... existing code ...
    
    async loadAll() {
        try {
            const data = await fs.promises.readFile(this.filepath, 'utf-8');
            return JSON.parse(data);
        } catch (error) {
            if (error.code === 'ENOENT') {
                console.warn(`File ${this.filepath} tidak ditemukan, create baru`);
                return [];
            }
            if (error instanceof SyntaxError) {
                console.error("JSON parsing error:", error);
                return [];
            }
            throw error;  // Propagate unknown errors
        }
    }
}

// Custom error class
export class NotFoundError extends Error {
    constructor(id) {
        super(`Item dengan ID ${id} tidak ditemukan`);
        this.name = "NotFoundError";
        this.code = "NOT_FOUND";
    }
}

// Usage
async function getBuku(id) {
    const buku = await repo.findById(id);
    if (!buku) {
        throw new NotFoundError(id);
    }
    return buku;
}

// Error handling
try {
    await getBuku(999);
} catch (error) {
    if (error instanceof NotFoundError) {
        console.error("Buku tidak ditemukan");
    } else {
        console.error("Unexpected error:", error);
    }
}
```

---

## F. Environment Variables dan Configuration

### 13. [[13. Dotenv untuk Configuration Management]]

Bash

```
npm install dotenv
```

**File: .env**

text

```
NODE_ENV=development
PORT=3000
DB_PATH=./data
PERPUSTAKAAN_NAME=Perpustakaan Digital
DEBUG=true
```

**File: config.js**

JavaScript

```
import dotenv from 'dotenv';
import path from 'path';

dotenv.config();

export const config = {
    env: process.env.NODE_ENV || 'development',
    port: parseInt(process.env.PORT || '3000'),
    dbPath: process.env.DB_PATH || './data',
    perpustakaanName: process.env.PERPUSTAKAAN_NAME || 'Perpustakaan',
    debug: process.env.DEBUG === 'true',
    
    katalogPath: () => path.join(config.dbPath, 'katalog.json'),
    userPath: () => path.join(config.dbPath, 'user.json'),
};

export default config;
```

**File: index.js**

JavaScript

```
import { config } from './config.js';

console.log(`Environment: ${config.env}`);
console.log(`Running on port: ${config.port}`);
console.log(`Perpustakaan: ${config.perpustakaanName}`);
```

**File: .gitignore**

text

```
node_modules/
.env
.env.local
.env.*.local
data/
logs/
```

---

## 🏗 Checkpoint Level 2

text

```
PEMAHAMAN:
├── ES Modules (import/export)
├── File System async operations dengan fs.promises
├── Repository pattern untuk abstraction
├── Error handling dengan try-catch
├── Environment variables dan configuration
└── Custom error classes

PROYEK: File-Based Perpustakaan Repository
├── modules/buku.js: Buku class dengan methods
├── modules/repository.js: FileRepository CRUD
├── config.js: Configuration management
├── data/katalog.json: Persisted data
├── CLI commands: list, add, find, update, delete
├── Proper error handling untuk semua operations
├── .env configuration
└── >80% file operations work correctly

STRUKTUR PROJECT:
perpustakaan/
├── index.js
├── config.js
├── modules/
│   ├── buku.js
│   └── repository.js
├── data/
│   └── katalog.json
├── .env
├── .gitignore
├── package.json
└── package-lock.json

HABIT:
├── Gunakan fs.promises (bukan callback fs)
├── Selalu handle file not found errors
├── Use path.join() untuk path operations
├── Environment variables untuk config
└── Custom error classes untuk domain errors
```

**Commit**: `feat: add file-based repository with CRUD operations`

---

# 🟡 LEVEL 3: EXPRESS.JS & HTTP SERVER (Minggu 6-9)

**Tema**: "Dari CLI ke HTTP web server dengan Express.js"

**Benang Merah**: File-based repository (Level 2) → HTTP routing → Middleware → Request/Response

**Output**: Express server dengan routes untuk perpustakaan operations

---

## G. Express.js Basics

### 14. [[14. Setup Express dan First Route]]

Bash

```
npm install express
```

**File: server.js**

JavaScript

```
import express from 'express';
import { config } from './config.js';

const app = express();

// Middleware
app.use(express.json());  // Parse JSON body
app.use(express.urlencoded({ extended: true }));  // Parse form data

// Routes
app.get('/api/buku', (req, res) => {
    res.json({ status: 'ok', message: 'Daftar buku' });
});

app.get('/api/buku/:id', (req, res) => {
    const { id } = req.params;
    res.json({ id, status: 'ok', message: `Detail buku ${id}` });
});

app.post('/api/buku', (req, res) => {
    const { judul, pengarang } = req.body;
    res.status(201).json({ judul, pengarang, id: 1 });
});

// Error handling (middleware dengan 4 params)
app.use((err, req, res, next) => {
    console.error(err);
    res.status(500).json({ error: err.message });
});

// Start server
app.listen(config.port, () => {
    console.log(`Server listening on port ${config.port}`);
});
```

**Test dengan curl:**

Bash

```
# GET
curl http://localhost:3000/api/buku

# POST
curl -X POST http://localhost:3000/api/buku \
  -H "Content-Type: application/json" \
  -d '{"judul":"Clean Code","pengarang":"Robert Martin"}'
```

### 15. [[15. Routing dan Route Parameters]]

JavaScript

```
// Route parameters
app.get('/api/buku/:id', (req, res) => {
    const { id } = req.params;
    res.json({ id });
});

// Query parameters
app.get('/api/buku', (req, res) => {
    const { search, sort } = req.query;
    res.json({ search, sort });
});

// Combining params dan query
app.get('/api/kategori/:kategori/buku', (req, res) => {
    const { kategori } = req.params;
    const { limit = 10, offset = 0 } = req.query;
    res.json({ kategori, limit, offset });
});

// Route grouping dengan router
const router = express.Router();

router.get('/', (req, res) => {
    res.json({ message: 'Daftar buku' });
});

router.get('/:id', (req, res) => {
    res.json({ id: req.params.id });
});

app.use('/api/buku', router);
```

### 16. [[16. Middleware — Request Processing Pipeline]]

JavaScript

```
// Logging middleware
app.use((req, res, next) => {
    const start = Date.now();
    res.on('finish', () => {
        const duration = Date.now() - start;
        console.log(`${req.method} ${req.path} - ${res.statusCode} (${duration}ms)`);
    });
    next();
});

// Authentication middleware
function authMiddleware(req, res, next) {
    const token = req.headers.authorization?.replace('Bearer ', '');
    if (!token) {
        return res.status(401).json({ error: 'Unauthorized' });
    }
    req.user = { id: 1, name: 'Admin' };  // Simulate user
    next();
}

// Custom middleware untuk JSON yang valid
function validateJSON(req, res, next) {
    if (!req.is('application/json')) {
        return res.status(400).json({ error: 'Content-Type must be application/json' });
    }
    next();
}

// Apply middleware ke specific routes
app.post('/api/buku', validateJSON, (req, res) => {
    // ...
});

app.delete('/api/buku/:id', authMiddleware, (req, res) => {
    // Only authenticated users
});

// Error handling middleware (MUST be last)
app.use((err, req, res, next) => {
    const status = err.status || 500;
    const message = err.message || 'Internal Server Error';
    res.status(status).json({ error: message });
});
```

---

## H. Integrating Repository dengan Express

### 17. [[17. Service Layer Architecture]]

**File: modules/bukuService.js**

JavaScript

```
import { FileRepository } from './repository.js';

export class BukuService {
    constructor(repo) {
        this.repo = repo;
    }
    
    async getAllBuku() {
        return await this.repo.loadAll();
    }
    
    async getBukuById(id) {
        const buku = await this.repo.findById(parseInt(id));
        if (!buku) {
            throw { status: 404, message: 'Buku tidak ditemukan' };
        }
        return buku;
    }
    
    async createBuku(data) {
        if (!data.judul || !data.pengarang) {
            throw { status: 400, message: 'Judul dan pengarang required' };
        }
        return await this.repo.create(data);
    }
    
    async updateBuku(id, data) {
        const buku = await this.getBukuById(id);
        return await this.repo.update(parseInt(id), data);
    }
    
    async deleteBuku(id) {
        const buku = await this.getBukuById(id);
        const success = await this.repo.delete(parseInt(id));
        if (!success) {
            throw { status: 500, message: 'Failed to delete' };
        }
        return { message: 'Buku deleted' };
    }
    
    async searchBuku(keyword) {
        const all = await this.getAllBuku();
        return all.filter(b =>
            b.judul.toLowerCase().includes(keyword.toLowerCase()) ||
            b.pengarang.toLowerCase().includes(keyword.toLowerCase())
        );
    }
}
```

**File: routes/buku.js**

JavaScript

```
import express from 'express';

export function createBukuRouter(bukuService) {
    const router = express.Router();
    
    // GET /api/buku
    router.get('/', async (req, res, next) => {
        try {
            const { search } = req.query;
            let buku;
            
            if (search) {
                buku = await bukuService.searchBuku(search);
            } else {
                buku = await bukuService.getAllBuku();
            }
            
            res.json({ data: buku, count: buku.length });
        } catch (error) {
            next(error);
        }
    });
    
    // GET /api/buku/:id
    router.get('/:id', async (req, res, next) => {
        try {
            const buku = await bukuService.getBukuById(req.params.id);
            res.json({ data: buku });
        } catch (error) {
            next(error);
        }
    });
    
    // POST /api/buku
    router.post('/', async (req, res, next) => {
        try {
            const buku = await bukuService.createBuku(req.body);
            res.status(201).json({ data: buku });
        } catch (error) {
            next(error);
        }
    });
    
    // PUT /api/buku/:id
    router.put('/:id', async (req, res, next) => {
        try {
            const buku = await bukuService.updateBuku(req.params.id, req.body);
            res.json({ data: buku });
        } catch (error) {
            next(error);
        }
    });
    
    // DELETE /api/buku/:id
    router.delete('/:id', async (req, res, next) => {
        try {
            const result = await bukuService.deleteBuku(req.params.id);
            res.json(result);
        } catch (error) {
            next(error);
        }
    });
    
    return router;
}
```

**File: server.js**

JavaScript

```
import express from 'express';
import { FileRepository } from './modules/repository.js';
import { BukuService } from './modules/bukuService.js';
import { createBukuRouter } from './routes/buku.js';
import { config } from './config.js';

const app = express();

app.use(express.json());

// Setup services
const bukuRepo = new FileRepository(config.katalogPath());
const bukuService = new BukuService(bukuRepo);

// Routes
app.use('/api/buku', createBukuRouter(bukuService));

// Error handler
app.use((err, req, res, next) => {
    const status = err.status || 500;
    const message = err.message || 'Internal Server Error';
    console.error(err);
    res.status(status).json({ error: message });
});

app.listen(config.port, () => {
    console.log(`Server listening on port ${config.port}`);
});
```

---

## 🏗 Checkpoint Level 3

text

```
PEMAHAMAN:
├── Express routing (GET, POST, PUT, DELETE)
├── Route parameters dan query parameters
├── Middleware concept dan execution order
├── Service layer architecture
├── Error handling in async routes
└── HTTP status codes

PROYEK: Express API untuk Perpustakaan
├── Routes:
│   ├── GET /api/buku — list semua (+ search query)
│   ├── GET /api/buku/:id — detail buku
│   ├── POST /api/buku — create
│   ├── PUT /api/buku/:id — update
│   └── DELETE /api/buku/:id — delete
├── Service layer (BukuService)
├── Proper error handling dengan status codes
├── Request logging middleware
├── Validation untuk POST/PUT
├── Consistent JSON response format
└── Test dengan Postman/curl

STRUKTUR:
perpustakaan/
├── server.js
├── config.js
├── routes/
│   └── buku.js
├── modules/
│   ├── buku.js
│   ├── repository.js
│   └── bukuService.js
└── data/

HABIT:
├── Service layer untuk business logic (bukan di route handler)
├── Consistent error handling
├── Proper async/await dengan try-catch
├── 404 untuk not found, 400 untuk validation, 500 untuk server errors
└── Logging di middleware layer
```

**Commit**: `feat: add Express.js API with service layer architecture`

---

# 🟠 LEVEL 4: DATABASE INTEGRATION (Minggu 9-13)

**Tema**: "Dari file-based ke proper database (MongoDB atau MySQL)"

**Benang Merah**: File repository (Level 3) → Database connection → ORM/Query builder → Data persistence

**Output**: API dengan MongoDB/MySQL backend, proper schema, queries

---

## I. Database Choice: MongoDB vs MySQL

### 18. [[18. MongoDB Setup dan Mongoose ORM]]

Bash

```
# Install Mongoose
npm install mongoose
```

**File: modules/dbConnection.js**

JavaScript

```
import mongoose from 'mongoose';

export async function connectDB() {
    try {
        const uri = process.env.MONGODB_URI || 'mongodb://localhost:27017/perpustakaan';
        await mongoose.connect(uri, {
            useNewUrlParser: true,
            useUnifiedTopology: true,
        });
        console.log('MongoDB connected');
    } catch (error) {
        console.error('MongoDB connection error:', error);
        process.exit(1);
    }
}
```

**File: modules/bukuModel.js**

JavaScript

```
import mongoose from 'mongoose';

const bukuSchema = new mongoose.Schema({
    judul: {
        type: String,
        required: [true, 'Judul harus diisi'],
        minlength: 3,
        maxlength: 200,
    },
    pengarang: {
        type: String,
        required: [true, 'Pengarang harus diisi'],
        minlength: 2,
    },
    isbn: {
        type: String,
        unique: true,
        sparse: true,
    },
    tahun: {
        type: Number,
        min: 1000,
        max: new Date().getFullYear(),
    },
    harga: {
        type: Number,
        required: true,
        min: 0,
    },
    stok: {
        type: Number,
        required: true,
        min: 0,
        default: 0,
    },
    kategori: {
        type: String,
        enum: ['Teknologi', 'Fiksi', 'Non-Fiksi', 'Sains', 'Umum'],
        default: 'Umum',
    },
}, {
    timestamps: true,  // createdAt, updatedAt
});

export const Buku = mongoose.model('Buku', bukuSchema);
```

**File: modules/bukuService.js (refactored for MongoDB)**

JavaScript

```
import { Buku } from './bukuModel.js';

export class BukuService {
    async getAllBuku() {
        return await Buku.find();
    }
    
    async getBukuById(id) {
        const buku = await Buku.findById(id);
        if (!buku) {
            throw { status: 404, message: 'Buku tidak ditemukan' };
        }
        return buku;
    }
    
    async createBuku(data) {
        const buku = new Buku(data);
        return await buku.save();
    }
    
    async updateBuku(id, data) {
        const buku = await Buku.findByIdAndUpdate(id, data, { new: true });
        if (!buku) {
            throw { status: 404, message: 'Buku tidak ditemukan' };
        }
        return buku;
    }
    
    async deleteBuku(id) {
        const buku = await Buku.findByIdAndDelete(id);
        if (!buku) {
            throw { status: 404, message: 'Buku tidak ditemukan' };
        }
        return { message: 'Buku deleted' };
    }
    
    async searchBuku(keyword) {
        return await Buku.find({
            $or: [
                { judul: { $regex: keyword, $options: 'i' } },
                { pengarang: { $regex: keyword, $options: 'i' } },
            ]
        });
    }
}
```

**File: server.js (update dengan MongoDB)**

JavaScript

```
import express from 'express';
import { connectDB } from './modules/dbConnection.js';
import { BukuService } from './modules/bukuService.js';
import { createBukuRouter } from './routes/buku.js';
import { config } from './config.js';

const app = express();

app.use(express.json());

// Connect DB
await connectDB();

// Setup services
const bukuService = new BukuService();

// Routes
app.use('/api/buku', createBukuRouter(bukuService));

// Error handler
app.use((err, req, res, next) => {
    // Mongoose validation error
    if (err.name === 'ValidationError') {
        const messages = Object.values(err.errors).map(e => e.message);
        return res.status(400).json({ error: messages });
    }
    
    // Mongoose duplicate key error
    if (err.code === 11000) {
        return res.status(400).json({ error: 'Duplicate key error' });
    }
    
    const status = err.status || 500;
    const message = err.message || 'Internal Server Error';
    res.status(status).json({ error: message });
});

app.listen(config.port, () => {
    console.log(`Server listening on port ${config.port}`);
});
```

### 19. [[19. MySQL dengan Sequelize ORM]]

**Alternative to MongoDB** — gunakan Sequelize untuk MySQL.

Bash

```
npm install sequelize mysql2
```

**File: modules/dbConnection.js**

JavaScript

```
import { Sequelize } from 'sequelize';

export const sequelize = new Sequelize({
    dialect: 'mysql',
    host: process.env.DB_HOST || 'localhost',
    port: process.env.DB_PORT || 3306,
    username: process.env.DB_USER || 'root',
    password: process.env.DB_PASSWORD || '',
    database: process.env.DB_NAME || 'perpustakaan',
    logging: false,  // Set true untuk debug SQL
});

export async function connectDB() {
    try {
        await sequelize.authenticate();
        console.log('MySQL connected');
        await sequelize.sync({ alter: true });
    } catch (error) {
        console.error('Database connection error:', error);
        process.exit(1);
    }
}
```

**File: modules/bukuModel.js**

JavaScript

```
import { DataTypes } from 'sequelize';
import { sequelize } from './dbConnection.js';

export const Buku = sequelize.define('Buku', {
    id: {
        type: DataTypes.INTEGER,
        primaryKey: true,
        autoIncrement: true,
    },
    judul: {
        type: DataTypes.STRING(200),
        allowNull: false,
    },
    pengarang: {
        type: DataTypes.STRING(100),
        allowNull: false,
    },
    isbn: {
        type: DataTypes.STRING(13),
        unique: true,
        allowNull: true,
    },
    harga: {
        type: DataTypes.DECIMAL(12, 2),
        allowNull: false,
    },
    stok: {
        type: DataTypes.INTEGER,
        defaultValue: 0,
    },
    kategori: {
        type: DataTypes.ENUM('Teknologi', 'Fiksi', 'Non-Fiksi', 'Sains', 'Umum'),
        defaultValue: 'Umum',
    },
}, {
    timestamps: true,
});
```

---

## 🏗 Checkpoint Level 4

text

```
PEMAHAMAN:
├── Database schema design
├── MongoDB vs MySQL trade-offs
├── ORM (Mongoose/Sequelize) benefits
├── Query operations: find, create, update, delete
├── Validation di model layer
└── Transactions (untuk MySQL)

PROYEK: API dengan Database Backend
├── Pilih salah satu: MongoDB atau MySQL
├── Schema/Model dengan proper types dan validation
├── BukuService adapted untuk ORM
├── All CRUD operations tested
├── Search dan filtering
├── Error handling untuk DB errors
├── Timestamps (createdAt, updatedAt)
└── Soft delete (optional, advanced)

STRUKTUR:
perpustakaan/
├── server.js
├── config.js
├── modules/
│   ├── dbConnection.js
│   ├── bukuModel.js
│   └── bukuService.js
├── routes/
│   └── buku.js
└── .env

HABIT:
├── Model validation (bukan hanya at API level)
├── Proper error messages dari DB errors
├── Use ORM (bukan raw SQL) for safety
├── Test DB operations
└── Backup data regularly (production)
```

**Commit**: `feat: integrate MongoDB/MySQL with Mongoose/Sequelize`

---

# 🔴 LEVEL 5: VALIDATION, AUTH & ADVANCED PATTERNS (Minggu 13-17)

**Tema**: "Robust API dengan input validation, authentication, dan best practices"

**Benang Merah**: Basic CRUD (Level 4) → Input validation → JWT authentication → Advanced patterns

**Output**: Production-ready API dengan validation, auth, proper error handling

---

## J. Input Validation

### 20. [[20. Joi untuk Request Validation]]

Bash

```
npm install joi
```

**File: modules/validators.js**

JavaScript

```
import Joi from 'joi';

export const bukuSchema = Joi.object({
    judul: Joi.string().min(3).max(200).required(),
    pengarang: Joi.string().min(2).max(100).required(),
    isbn: Joi.string().length(13).optional(),
    tahun: Joi.number().integer().min(1000).max(new Date().getFullYear()).optional(),
    harga: Joi.number().positive().required(),
    stok: Joi.number().integer().min(0).required(),
    kategori: Joi.string().valid('Teknologi', 'Fiksi', 'Non-Fiksi', 'Sains', 'Umum').optional(),
});

export const updateBukuSchema = bukuSchema.fork(
    ['judul', 'pengarang', 'harga', 'stok'],
    schema => schema.optional()
);

export function validate(schema) {
    return (req, res, next) => {
        const { error, value } = schema.validate(req.body, { abortEarly: false });
        if (error) {
            const messages = error.details.map(d => d.message);
            return res.status(400).json({ errors: messages });
        }
        req.validated = value;
        next();
    };
}
```

**File: routes/buku.js (with validation)**

JavaScript

```
import { validate, bukuSchema, updateBukuSchema } from '../modules/validators.js';

export function createBukuRouter(bukuService) {
    const router = express.Router();
    
    router.post('/', validate(bukuSchema), async (req, res, next) => {
        try {
            const buku = await bukuService.createBuku(req.validated);
            res.status(201).json({ data: buku });
        } catch (error) {
            next(error);
        }
    });
    
    router.put('/:id', validate(updateBukuSchema), async (req, res, next) => {
        try {
            const buku = await bukuService.updateBuku(req.params.id, req.validated);
            res.json({ data: buku });
        } catch (error) {
            next(error);
        }
    });
    
    return router;
}
```

---

## K. Authentication & JWT

### 21. [[21. JWT Authentication]]

Bash

```
npm install jsonwebtoken bcryptjs
```

**File: modules/authService.js**

JavaScript

```
import jwt from 'jsonwebtoken';
import bcryptjs from 'bcryptjs';

const JWT_SECRET = process.env.JWT_SECRET || 'your-secret-key-change-in-production';

export class AuthService {
    async hashPassword(password) {
        return await bcryptjs.hash(password, 10);
    }
    
    async verifyPassword(password, hash) {
        return await bcryptjs.compare(password, hash);
    }
    
    generateToken(user) {
        return jwt.sign(
            { id: user.id, email: user.email, role: user.role },
            JWT_SECRET,
            { expiresIn: '24h' }
        );
    }
    
    verifyToken(token) {
        try {
            return jwt.verify(token, JWT_SECRET);
        } catch (error) {
            throw { status: 401, message: 'Invalid or expired token' };
        }
    }
}
```

**File: middleware/authMiddleware.js**

JavaScript

```
import { AuthService } from '../modules/authService.js';

const authService = new AuthService();

export function authMiddleware(req, res, next) {
    const token = req.headers.authorization?.replace('Bearer ', '');
    
    if (!token) {
        return res.status(401).json({ error: 'Missing authorization token' });
    }
    
    try {
        const decoded = authService.verifyToken(token);
        req.user = decoded;
        next();
    } catch (error) {
        res.status(error.status).json({ error: error.message });
    }
}

export function adminMiddleware(req, res, next) {
    if (req.user?.role !== 'admin') {
        return res.status(403).json({ error: 'Forbidden: Admin access required' });
    }
    next();
}
```

**File: routes/auth.js**

JavaScript

```
import express from 'express';

export function createAuthRouter(authService, userService) {
    const router = express.Router();
    
    router.post('/login', async (req, res, next) => {
        try {
            const { email, password } = req.body;
            
            // Find user
            const user = await userService.findByEmail(email);
            if (!user) {
                return res.status(401).json({ error: 'Invalid credentials' });
            }
            
            // Verify password
            const valid = await authService.verifyPassword(password, user.passwordHash);
            if (!valid) {
                return res.status(401).json({ error: 'Invalid credentials' });
            }
            
            // Generate token
            const token = authService.generateToken(user);
            res.json({ token, user: { id: user.id, email: user.email } });
        } catch (error) {
            next(error);
        }
    });
    
    router.post('/register', async (req, res, next) => {
        try {
            const { email, password } = req.body;
            
            const passwordHash = await authService.hashPassword(password);
            const user = await userService.create({ email, passwordHash });
            
            const token = authService.generateToken(user);
            res.status(201).json({ token, user: { id: user.id, email: user.email } });
        } catch (error) {
            next(error);
        }
    });
    
    return router;
}
```

**File: server.js (with auth routes)**

JavaScript

```
import { authMiddleware, adminMiddleware } from './middleware/authMiddleware.js';
import { createAuthRouter } from './routes/auth.js';

// Public routes
app.use('/api/auth', createAuthRouter(authService, userService));

// Protected routes
app.use('/api/buku', createBukuRouter(bukuService));  // Public GET
app.post('/api/buku', authMiddleware, adminMiddleware, createBuku);
app.put('/api/buku/:id', authMiddleware, adminMiddleware, updateBuku);
app.delete('/api/buku/:id', authMiddleware, adminMiddleware, deleteBuku);
```

---

## L. Testing

### 22. [[22. Unit & Integration Testing dengan Jest]]

Bash

```
npm install --save-dev jest supertest @testing-library/node
```

**File: package.json**

JSON

```
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage"
  }
}
```

**File: modules/bukuService.test.js**

JavaScript

```
import { BukuService } from './bukuService.js';
import { Buku } from './bukuModel.js';

jest.mock('./bukuModel.js');

describe('BukuService', () => {
    let service;
    
    beforeEach(() => {
        service = new BukuService();
        jest.clearAllMocks();
    });
    
    describe('getAllBuku', () => {
        test('should return all buku', async () => {
            const mockData = [
                { id: 1, judul: 'Clean Code' },
                { id: 2, judul: 'Design Patterns' },
            ];
            Buku.find.mockResolvedValue(mockData);
            
            const result = await service.getAllBuku();
            
            expect(result).toEqual(mockData);
            expect(Buku.find).toHaveBeenCalled();
        });
    });
    
    describe('createBuku', () => {
        test('should create new buku', async () => {
            const data = { judul: 'New Book', pengarang: 'Author' };
            const mockBuku = { id: 1, ...data };
            Buku.prototype.save = jest.fn().mockResolvedValue(mockBuku);
            
            const result = await service.createBuku(data);
            
            expect(result).toEqual(mockBuku);
        });
        
        test('should throw error if data invalid', async () => {
            const data = { judul: 'ab' };  // Too short
            
            await expect(service.createBuku(data))
                .rejects.toThrow('Validation error');
        });
    });
});
```

**File: routes/buku.test.js**

JavaScript

```
import request from 'supertest';
import app from '../server.js';

describe('GET /api/buku', () => {
    test('should return list of buku', async () => {
        const response = await request(app)
            .get('/api/buku')
            .expect(200);
        
        expect(response.body).toHaveProperty('data');
        expect(Array.isArray(response.body.data)).toBe(true);
    });
});

describe('POST /api/buku', () => {
    test('should create new buku', async () => {
        const response = await request(app)
            .post('/api/buku')
            .set('Authorization', `Bearer ${validToken}`)
            .send({
                judul: 'Test Book',
                pengarang: 'Test Author',
                harga: 100000,
                stok: 5,
            })
            .expect(201);
        
        expect(response.body.data).toHaveProperty('id');
    });
    
    test('should return 400 for invalid data', async () => {
        const response = await request(app)
            .post('/api/buku')
            .set('Authorization', `Bearer ${validToken}`)
            .send({ judul: 'ab' })  // Too short
            .expect(400);
        
        expect(response.body).toHaveProperty('errors');
    });
});
```

---

## 🏗 Checkpoint Level 5

text

```
PEMAHAMAN:
├── Input validation dengan Joi/Yup
├── JWT token generation dan verification
├── Password hashing dengan bcryptjs
├── Role-based access control (RBAC)
├── Unit testing dengan Jest
├── Integration testing dengan supertest
└── Error handling best practices

PROYEK: Production-Ready API
├── Validation middleware untuk all requests
├── JWT auth dengan login/register routes
├── Protected routes dengan authMiddleware
├── Admin-only routes dengan adminMiddleware
├── Unit tests untuk services
├── Integration tests untuk all endpoints
├── >80% code coverage
├── Error handling untuk:
│   ├── Validation errors (400)
│   ├── Auth errors (401)
│   ├── Authorization errors (403)
│   ├── Not found (404)
│   └── Server errors (500)
└── Consistent response format

STRUKTUR:
perpustakaan/
├── server.js
├── modules/
│   ├── bukuService.js
│   ├── authService.js
│   ├── userService.js
│   ├── validators.js
│   └── *.test.js
├── middleware/
│   └── authMiddleware.js
├── routes/
│   ├── buku.js
│   ├── auth.js
│   └── *.test.js
└── __tests__/

HABIT:
├── Every route handler wrapped in try-catch
├── Validation happening at API boundary
├── Business logic in service layer
├── Tests written first (TDD) or alongside
├── Mock external dependencies in tests
└── >70% code coverage minimum
```

**Commit**: `feat: add validation, JWT auth, and comprehensive testing`

---

# ⚫ LEVEL 6: ADVANCED PATTERNS & PRODUCTION (Minggu 17-22)

**Tema**: "Production-ready features: logging, caching, documentation, optimization"

**Benang Merah**: Robust API (Level 5) → Logging → Caching → API documentation → Error tracking

**Output**: Professional API ready for production deployment

---

## M. Logging dan Monitoring

### 23. [[23. Structured Logging dengan Winston]]

Bash

```
npm install winston
```

**File: modules/logger.js**

JavaScript

```
import winston from 'winston';

const logger = winston.createLogger({
    level: process.env.LOG_LEVEL || 'info',
    format: winston.format.combine(
        winston.format.timestamp(),
        winston.format.errors({ stack: true }),
        winston.format.json()
    ),
    transports: [
        new winston.transports.File({ filename: 'logs/error.log', level: 'error' }),
        new winston.transports.File({ filename: 'logs/combined.log' }),
    ],
});

if (process.env.NODE_ENV !== 'production') {
    logger.add(new winston.transports.Console({
        format: winston.format.simple(),
    }));
}

export default logger;
```

**File: middleware/requestLogger.js**

JavaScript

```
import logger from '../modules/logger.js';

export function requestLoggerMiddleware(req, res, next) {
    const start = Date.now();
    
    res.on('finish', () => {
        const duration = Date.now() - start;
        logger.info({
            method: req.method,
            path: req.path,
            status: res.statusCode,
            duration: `${duration}ms`,
            user: req.user?.id,
        });
    });
    
    next();
}
```

---

## N. Caching dan Performance

### 24. [[24. Redis Caching]]

Bash

```
npm install redis
```

**File: modules/cacheService.js**

JavaScript

```
import { createClient } from 'redis';

const redis = createClient({
    host: process.env.REDIS_HOST || 'localhost',
    port: process.env.REDIS_PORT || 6379,
});

redis.on('error', err => console.error('Redis error:', err));

export const cache = {
    async get(key) {
        try {
            const value = await redis.get(key);
            return value ? JSON.parse(value) : null;
        } catch (error) {
            console.error('Cache get error:', error);
            return null;
        }
    },
    
    async set(key, value, ttl = 3600) {
        try {
            await redis.setEx(key, ttl, JSON.stringify(value));
        } catch (error) {
            console.error('Cache set error:', error);
        }
    },
    
    async del(key) {
        try {
            await redis.del(key);
        } catch (error) {
            console.error('Cache del error:', error);
        }
    },
};

export async function connectRedis() {
    try {
        await redis.connect();
        console.log('Redis connected');
    } catch (error) {
        console.error('Redis connection error:', error);
    }
}
```

**File: routes/buku.js (with caching)**

JavaScript

```
import { cache } from '../modules/cacheService.js';

export function createBukuRouter(bukuService) {
    const router = express.Router();
    
    router.get('/', async (req, res, next) => {
        try {
            const cacheKey = 'buku:list';
            
            // Try cache first
            let buku = await cache.get(cacheKey);
            if (buku) {
                return res.json({ data: buku, from: 'cache' });
            }
            
            // Fetch from DB
            buku = await bukuService.getAllBuku();
            
            // Store in cache
            await cache.set(cacheKey, buku, 3600);
            
            res.json({ data: buku, from: 'db' });
        } catch (error) {
            next(error);
        }
    });
    
    router.post('/', authMiddleware, adminMiddleware, async (req, res, next) => {
        try {
            const buku = await bukuService.createBuku(req.validated);
            
            // Invalidate cache
            await cache.del('buku:list');
            
            res.status(201).json({ data: buku });
        } catch (error) {
            next(error);
        }
    });
    
    return router;
}
```

---

## O. API Documentation

### 25. [[25. Swagger/OpenAPI Documentation]]

Bash

```
npm install swagger-ui-express swagger-jsdoc
```

**File: modules/swaggerDef.js**

JavaScript

```
import swaggerJsdoc from 'swagger-jsdoc';

const options = {
    definition: {
        openapi: '3.0.0',
        info: {
            title: 'Perpustakaan API',
            version: '1.0.0',
            description: 'API untuk sistem manajemen perpustakaan',
        },
        servers: [
            { url: 'http://localhost:3000', description: 'Development' },
            { url: 'https://api.perpustakaan.com', description: 'Production' },
        ],
        components: {
            securitySchemes: {
                bearerAuth: {
                    type: 'http',
                    scheme: 'bearer',
                    bearerFormat: 'JWT',
                },
            },
        },
    },
    apis: ['./routes/*.js'],
};

export const swaggerSpec = swaggerJsdoc(options);
```

**File: routes/buku.js (with Swagger comments)**

JavaScript

```
/**
 * @swagger
 * /api/buku:
 *   get:
 *     summary: Get all books
 *     tags: [Buku]
 *     parameters:
 *       - in: query
 *         name: search
 *         schema:
 *           type: string
 *         description: Search keyword
 *     responses:
 *       200:
 *         description: List of books
 *         content:
 *           application/json:
 *             schema:
 *               type: object
 *               properties:
 *                 data:
 *                   type: array
 *                   items:
 *                     $ref: '#/components/schemas/Buku'
 */
router.get('/', async (req, res, next) => { ... });

/**
 * @swagger
 * /api/buku:
 *   post:
 *     summary: Create new book
 *     tags: [Buku]
 *     security:
 *       - bearerAuth: []
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             $ref: '#/components/schemas/CreateBukuInput'
 *     responses:
 *       201:
 *         description: Book created
 */
router.post('/', authMiddleware, adminMiddleware, validate(bukuSchema), async (req, res, next) => { ... });
```

**File: server.js**

JavaScript

```
import swaggerUi from 'swagger-ui-express';
import { swaggerSpec } from './modules/swaggerDef.js';

app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec));
```

**Access documentation**: `http://localhost:3000/api-docs`

---

## 🏗 Checkpoint Level 6

text

```
PEMAHAMAN:
├── Structured logging dengan Winston
├── Caching strategies dengan Redis
├── API documentation dengan Swagger
├── Performance optimization
├── Error tracking dan monitoring
├── Rate limiting
├── CORS configuration
└── Production deployment considerations

PROYEK: Production-Ready API
├── Winston logging di semua layers
├── Redis caching untuk GET requests
├── Swagger documentation updated
├── Rate limiting middleware
├── CORS configured properly
├── Error tracking (Sentry optional)
├── Health check endpoint
├── Graceful shutdown
├── Environment-based configuration
├── API versioning (/v1/, /v2/)
└── Request/Response compression

STRUKTUR:
perpustakaan/
├── server.js
├── modules/
│   ├── logger.js
│   ├── cacheService.js
│   ├── swaggerDef.js
│   └── ...
├── middleware/
│   ├── authMiddleware.js
│   ├── requestLogger.js
│   ├── errorHandler.js
│   ├── rateLimiter.js
│   └── cors.js
├── routes/
├── logs/
│   ├── error.log
│   └── combined.log
└── .env

HABIT:
├── Log at appropriate levels (info, warn, error)
├── Cache invalidation on mutations
├── API versioning from day 1
├── Document as you code
├── Monitor production metrics
└── Keep logs rotated (production)
```

**Commit**: `feat: add logging, caching, documentation, and production setup`

---

# 🔴 LEVEL 7: REAL-TIME & DEPLOYMENT (Minggu 22-25)

**Tema**: "Real-time features dan production deployment"

**Benang Merah**: REST API (Level 6) → WebSocket for real-time → Docker containerization → Deployment

**Output**: Real-time application deployed to production

---

## P. Real-Time dengan Socket.IO

### 26. [[26. Socket.IO untuk Real-Time Communication]]

Bash

```
npm install socket.io
```

**File: modules/socketManager.js**

JavaScript

```
import { io } from 'socket.io-client';

export function setupWebSocket(io) {
    io.on('connection', (socket) => {
        console.log(`User ${socket.id} connected`);
        
        // Notify all clients
        io.emit('user:joined', { userId: socket.id });
        
        // Handle events
        socket.on('buku:pinjam', async (data) => {
            const { bukuId, anggotaId } = data;
            // Process peminjaman
            io.emit('buku:stok:updated', { bukuId, stok: newStok });
        });
        
        socket.on('disconnect', () => {
            console.log(`User ${socket.id} disconnected`);
            io.emit('user:left', { userId: socket.id });
        });
    });
}
```

**File: server.js**

JavaScript

```
import { createServer } from 'http';
import { Server as SocketIOServer } from 'socket.io';
import express from 'express';
import { setupWebSocket } from './modules/socketManager.js';

const app = express();
const server = createServer(app);
const socketIO = new SocketIOServer(server, {
    cors: {
        origin: process.env.CLIENT_URL || 'http://localhost:3000',
        methods: ['GET', 'POST'],
    },
});

setupWebSocket(socketIO);

server.listen(config.port, () => {
    console.log(`Server listening on port ${config.port}`);
});
```

**Client-side (JavaScript/React):**

JavaScript

```
import { io } from 'socket.io-client';

const socket = io('http://localhost:3000');

socket.on('user:joined', (data) => {
    console.log('User joined:', data);
});

socket.on('buku:stok:updated', (data) => {
    console.log('Stock updated:', data);
});

// Emit event
socket.emit('buku:pinjam', { bukuId: 1, anggotaId: 1 });
```

---

## Q. Docker dan Deployment

### 27. [[27. Docker Containerization]]

**File: Dockerfile**

Dockerfile

```
FROM node:18-alpine

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy application
COPY . .

# Expose port
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD node -e "require('http').get('http://localhost:3000/health', (r) => r.statusCode === 200 ? process.exit(0) : process.exit(1))"

# Start application
CMD ["node", "server.js"]
```

**File: docker-compose.yml**

YAML

```
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - MONGODB_URI=mongodb://mongo:27017/perpustakaan
      - REDIS_HOST=redis
      - JWT_SECRET=your-secret
    depends_on:
      - mongo
      - redis
    networks:
      - perpustakaan-network
  
  mongo:
    image: mongo:6.0
    ports:
      - "27017:27017"
    environment:
      - MONGO_INITDB_DATABASE=perpustakaan
    volumes:
      - mongo_data:/data/db
    networks:
      - perpustakaan-network
  
  redis:
    image: redis:7.0
    ports:
      - "6379:6379"
    networks:
      - perpustakaan-network

volumes:
  mongo_data:

networks:
  perpustakaan-network:
    driver: bridge
```

**Deploy:**

Bash

```
docker-compose build
docker-compose up -d
```

### 28. [[28. Deployment ke Cloud (Heroku/Railway/Render)]]

**File: Procfile** (untuk Heroku)

text

```
web: npm start
```

**File: .env.production**

text

```
NODE_ENV=production
PORT=3000
MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/perpustakaan
REDIS_URL=redis://...
JWT_SECRET=your-production-secret
```

**Deploy ke Heroku:**

Bash

```
# Install Heroku CLI
heroku login
heroku create perpustakaan-api
git push heroku main
heroku logs --tail
```

---

## 🏗 Checkpoint Level 7

text

```
PEMAHAMAN:
├── WebSocket protocol dan Socket.IO
├── Real-time event handling
├── Docker images dan containers
├── Docker Compose untuk multi-service
├── Cloud deployment platforms
├── CI/CD pipelines
└── Production monitoring

PROYEK: Real-Time API dengan Deployment
├── Socket.IO setup untuk real-time events
├── Real-time buku stok updates
├── Real-time notifikasi peminjaman
├── Docker image built and tested
├── Docker Compose dengan MongoDB + Redis
├── Deployed to cloud platform
├── HTTPS/SSL configured
├── Health check endpoint
├── Proper error handling in production
└── Monitoring dan alerting (optional)

DEPLOYMENT CHECKLIST:
├── All environment variables configured
├── Database migrations run
├── SSL certificates installed
├── Health checks passing
├── Logs being collected
├── Backups scheduled
├── Monitoring alerts set
├── Rate limiting active
└── CORS properly configured

HABIT:
├── CI/CD pipeline for automated tests & deployment
├── Semantic versioning for releases
├── Zero-downtime deployment strategy
├── Database backup before deployments
├── Keep dependencies updated
└── Monitor production errors
```

**Commit**: `feat: add real-time features with Socket.IO and Docker deployment`

---

# 🟣 LEVEL 8: MICROSERVICES & ADVANCED (Minggu 25+)

**Tema**: "Scale beyond monolith — Microservices, message queues, advanced patterns"

---

## R. Microservices Architecture

### 29. [[29. Service Decomposition dan Inter-Service Communication]]

**Buku Service, User Service, Peminjaman Service** sebagai separate services

Bash

```
# Setup message queue
npm install amqplib dotenv
```

**File: services/buku-service/server.js**

JavaScript

```
// Standalone Buku service
app.get('/api/buku', (req, res) => {
    // Buku operations only
});
```

**File: services/user-service/server.js**

JavaScript

```
// Standalone User service
app.get('/api/user', (req, res) => {
    // User operations only
});
```

**File: services/shared/messageQueue.js**

JavaScript

```
import amqp from 'amqplib';

let channel;

export async function connectRabbitMQ() {
    const connection = await amqp.connect('amqp://localhost');
    channel = await connection.createChannel();
    return channel;
}

export async function publishEvent(queue, message) {
    await channel.assertQueue(queue);
    await channel.sendToQueue(queue, Buffer.from(JSON.stringify(message)));
}

export async function subscribeToEvent(queue, callback) {
    await channel.assertQueue(queue);
    await channel.consume(queue, (msg) => {
        const data = JSON.parse(msg.content.toString());
        callback(data);
        channel.ack(msg);
    });
}
```

**File: services/buku-service/events.js**

JavaScript

```
import { publishEvent } from '../shared/messageQueue.js';

// When book is borrowed
export async function onBukuDipinjam(bukuId, anggotaId) {
    await publishEvent('buku.dipinjam', {
        bukuId,
        anggotaId,
        timestamp: new Date(),
    });
}
```

**File: services/peminjaman-service/server.js**

JavaScript

```
import { subscribeToEvent } from '../shared/messageQueue.js';

// Listen to book events
subscribeToEvent('buku.dipinjam', async (data) => {
    const peminjaman = await createPeminjaman(data);
    console.log('Peminjaman created:', peminjaman);
});
```

---

## S. API Gateway Pattern

### 30. [[30. API Gateway sebagai Single Entry Point]]

**File: api-gateway/server.js**

JavaScript

```
import express from 'express';
import httpProxy from 'express-http-proxy';

const app = express();

// Route ke services
app.use('/api/buku', httpProxy('http://localhost:3001'));
app.use('/api/user', httpProxy('http://localhost:3002'));
app.use('/api/peminjaman', httpProxy('http://localhost:3003'));

// Unified auth check di gateway
app.use(authMiddleware);

// Logging di gateway
app.use(requestLoggerMiddleware);

app.listen(3000, () => {
    console.log('API Gateway listening on port 3000');
});
```

---

# 📊 Ringkasan Progress Tracking

text

```
Level 1: JavaScript & Async → Async/Await patterns
 + Level 2: Node.js Core & File System → File persistence
 + Level 3: Express.js → HTTP REST API
 + Level 4: Database Integration → MySQL/MongoDB
 + Level 5: Validation & Auth → JWT, RBAC, testing
 + Level 6: Advanced Patterns → Logging, caching, docs
 + Level 7: Real-Time & Deployment → Socket.IO, Docker
 + Level 8: Microservices → Service decomposition, message queues
```

## Tabel Progress

|Level|Poin|Durasi|Output Konkret|
|---|---|---|---|
|🟢 1|1-8|Minggu 1-3|Async/Await basics|
|🔵 2|9-13|Minggu 3-6|File-based CLI app|
|🟡 3|14-17|Minggu 6-9|Express REST API|
|🟠 4|18-19|Minggu 9-13|Database integrated|
|🔴 5|20-22|Minggu 13-17|Validation + Auth|
|⚫ 6|23-25|Minggu 17-22|Production-ready|
|🔴 7|26-28|Minggu 22-25|Real-time + Deploy|
|🟣 8|29-30|Minggu 25+|Microservices|

---

# ⚠️ Aturan yang Tidak Boleh Dilanggar

1. **ALWAYS use Async/Await**: Jangan gunakan callback hell
2. **ALWAYS === untuk comparison**: Gunakan strict equality
3. **ALWAYS const as default**: Bukan let/var
4. **ALWAYS handle errors**: Try-catch di async functions, .catch() untuk promises
5. **ALWAYS validate input**: Di API boundary DAN di service layer
6. **ALWAYS hash password**: gunakan bcryptjs, jangan plain text
7. **ALWAYS use environment variables**: Untuk config, bukan hardcoded
8. **ALWAYS test business logic**: Unit tests untuk services
9. **ALWAYS use structured logging**: Winston/Pino, bukan console.log
10. **ALWAYS protect sensitive routes**: Dengan authMiddleware minimal

---

**Roadmap Node.js v1.0 — Dari Hello World ke Production Microservices**

Setiap line of code written with purpose — tidak ada copy-paste tanpa understanding. Async-first mindset, test-driven development, production-ready dari awal.