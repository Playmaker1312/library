# 103 — Generator & Iterator

## 1. Penjelasan

**Iterator Protocol** adalah kontrak di JavaScript yang memungkinkan sebuah objek menghasilkan urutan nilai satu per satu melalui method `next()`. Sebuah objek dianggap iterable jika memiliki properti `Symbol.iterator`.

**Generator Function** adalah fungsi khusus yang ditulis dengan `function*` dan menggunakan keyword `yield` untuk menghasilkan nilai secara bertahap. Eksekusinya bisa dihentikan sementara (pause) dan dilanjutkan (resume) sesuai permintaan.

**Lazy Evaluation** berarti nilai hanya dihasilkan saat dibutuhkan, tidak sebelumnya. Ini memungkinkan pembuatan infinite sequences (deret tak terbatas) tanpa menghabiskan memori.

**Async Generator** menggabungkan generator dengan Promise/async-await untuk menghasilkan nilai secara asinkron (misal: pagination API, stream data).

| Konsep | Iterator Biasa | Generator |
|--------|----------------|-----------|
| Inisialisasi | Manual next() | `function*` + `yield` |
| State | Kelola manual | Otomatis (pause/resume) |
| Lazy | Ya | Ya, built-in |

## 2. Fungsi

- Membuat data stream yang dievaluasi secara lazy
- Pagination data dari API/database tanpa memuat semua sekaligus
- Infinite sequence (deret Fibonacci tak terbatas, ID generator)
- Async data processing (stream file, chunked response)
- Implementasi custom iterator untuk struktur data kompleks

## 3. Code

### Iterator Protocol Manual

```javascript
const range = {
  from: 1,
  to: 5,
  [Symbol.iterator]() {
    let current = this.from;
    const last = this.to;
    return {
      next() {
        if (current <= last) {
          return { value: current++, done: false };
        }
        return { value: undefined, done: true };
      },
    };
  },
};

for (const num of range) {
  console.log(num); // 1 2 3 4 5
}
```

### Generator Paginator Data

```javascript
// Simulasi data dari database
const database = Array.from({ length: 100 }, (_, i) => ({
  id: i + 1,
  name: `User ${i + 1}`,
}));

function* paginate(data, pageSize = 10) {
  let index = 0;
  while (index < data.length) {
    yield data.slice(index, index + pageSize);
    index += pageSize;
  }
}

const paginator = paginate(database, 10);

for (const page of paginator) {
  console.log(`Page dengan ${page.length} user:`, page);
}
```

### Async Generator (Fetch Pagination)

```javascript
async function* fetchPaginated(endpoint, maxPages = 3) {
  let page = 1;
  while (page <= maxPages) {
    const response = await fetch(`${endpoint}?page=${page}`);
    const data = await response.json();
    yield data;
    page++;
  }
}

(async () => {
  for await (const page of fetchPaginated('https://api.example.com/users')) {
    console.log('Data page:', page);
  }
})();
```

### Infinite Sequence (Fibonacci)

```javascript
function* fibonacci() {
  let a = 0,
    b = 1;
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

const fib = fibonacci();
console.log(fib.next().value); // 0
console.log(fib.next().value); // 1
console.log(fib.next().value); // 1
console.log(fib.next().value); // 2
console.log(fib.next().value); // 3
// Tidak pernah habis, tapi tidak boros memori
```

## 4. Analogi Rumah — Generator Air PDAM

| Konsep JS | Analogi Rumah |
|-----------|---------------|
| Generator function (`function*`) | Pipa PDAM dari sumber air |
| `yield` | Kran air — buka = keluar air, tutup = berhenti |
| `next()` | Memutar kran |
| Lazy evaluation | Air keluar hanya saat kran dibuka |
| Array (tandon penuh) | Tandon air di atap — semua air sudah siap dari awal |
| Infinite sequence | Sumber PDAM tak terbatas — tapi pakai kran, bukan tandon |
| `for...of` | Memutar kran terus sampai tidak mau lagi |
| Async generator | Pompa air otomatis — prosesnya butuh waktu, baru keluar setelah siap |

> **Perbedaan krusial:** Array seperti tandon penuh → semua data sudah dimuat di memori. Generator seperti kran PDAM → data mengalir saat diminta. Untuk 1 miliar data, tandon akan meledak (memory overflow), kran tetap aman.

## 5. Use Case di Proyek Nyata

- **Pagination API response** — mengambil data dari database 1000 baris, dikirim per 25 baris
- **File streaming** — membaca file besar (log, video) per-chunk, bukan sekaligus
- **Infinite scroll frontend** — data baru dimuat saat user scroll ke bawah
- **ID generator** — menghasilkan ID unik berurutan tanpa array
- **Web scraping** — crawling halaman satu per satu secara lazy

## 6. Kesalahan Umum

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| Mengubah array jadi generator tanpa alasan | Overhead tidak perlu | Hanya pakai generator untuk lazy evaluation |
| Lupa `yield` di dalam generator | Generator tidak menghasilkan apa-apa | Pastikan ada `yield` dalam loop |
| Menggunakan `return` bukan `yield` | Generator berhenti total | `return` mengakhiri generator, `yield` mengeluarkan nilai |
| Tidak handle `done: true` di iterator manual | Infinite loop | Selalu kembalikan `{ done: true }` saat selesai |
| Async generator tanpa `for await...of` | Promise tidak ter-resolve | Gunakan `for await` atau manual `await gen.next()` |

## 7. Benang Merah

```
Materi 102 (Security)
    ↓
Materi 103 (Generator & Iterator) ← Anda di sini
    ↓
Materi 104 (Proxy & Reflect — Metaprogramming)
```

Generator adalah pintu masuk ke fitur JavaScript tingkat lanjut setelah menguasai fundamental keamanan di materi sebelumnya. Konsep lazy evaluation dan kontrol alur eksekusi ini menjadi fondasi untuk memahami metaprogramming (Proxy & Reflect) di materi 104.

## 8. Soal & Jawaban

### Soal 1: Easy
Buatlah generator function `rangeGenerator(start, end)` yang menghasilkan angka dari `start` hingga `end` (inklusif).

<details>
<summary>Jawaban</summary>

```javascript
function* rangeGenerator(start, end) {
  for (let i = start; i <= end; i++) {
    yield i;
  }
}

const gen = rangeGenerator(3, 6);
console.log([...gen]); // [3, 4, 5, 6]
```
</details>

### Soal 2: Medium
Buatlah fungsi `paginationGenerator` yang menerima array data dan ukuran halaman. Gunakan generator untuk menghasilkan halaman demi halaman. Jika array memiliki 25 item dan pageSize = 10, hasilkan 3 halaman (10, 10, 5).

<details>
<summary>Jawaban</summary>

```javascript
function* paginationGenerator(data, pageSize = 10) {
  for (let i = 0; i < data.length; i += pageSize) {
    yield data.slice(i, i + pageSize);
  }
}

const data = Array.from({ length: 25 }, (_, i) => `Item ${i + 1}`);
const pages = paginationGenerator(data, 10);

let pageNum = 1;
for (const page of pages) {
  console.log(`Halaman ${pageNum}:`, page);
  pageNum++;
}
// Halaman 1: [Item 1 ... Item 10]
// Halaman 2: [Item 11 ... Item 20]
// Halaman 3: [Item 21 ... Item 25]
```
</details>

### Soal 3: Hard
Buatlah async generator `fetchAllPages(baseUrl, maxPages)` yang mengambil data dari API pagination. Setiap halaman di-fetch menggunakan `fetch()`, generator mengeluarkan (`yield`) data setiap halaman. Jika ada error di satu halaman, lempar error dan hentikan generator.

<details>
<summary>Jawaban</summary>

```javascript
async function* fetchAllPages(baseUrl, maxPages = 5) {
  for (let page = 1; page <= maxPages; page++) {
    const response = await fetch(`${baseUrl}?page=${page}&limit=10`);
    if (!response.ok) {
      throw new Error(`Gagal fetch halaman ${page}: ${response.status}`);
    }
    const data = await response.json();
    yield data;
  }
}

(async () => {
  try {
    for await (const pageData of fetchAllPages('https://api.example.com/users', 3)) {
      console.log('Menerima data:', pageData);
    }
  } catch (err) {
    console.error('Error pagination:', err.message);
  }
})();
```
</details>
