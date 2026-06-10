# 31. Stack & Queue — Struktur Data Logis dari Array

**Benang Merah**: Di Materi 30 kita belajar Set & Map (struktur data ES6). Sekarang kita kembali ke Array, tapi dengan **pola akses khusus**: Stack (LIFO) dan Queue (FIFO) — ini adalah fondasi dari Event Loop dan manajemen memori JavaScript.

---

## Penjelasan

**Stack (LIFO — Last In, First Out)**: Data yang terakhir masuk adalah yang pertama keluar. Seperti tumpukan piring — ambil selalu dari atas.

**Queue (FIFO — First In, First Out)**: Data yang pertama masuk adalah yang pertama keluar. Seperti antrian kasir — yang pertama datang dilayani pertama.

```javascript
// Stack — LIFO: push (tambah) + pop (ambil)
let stack = [];
stack.push("Piring 1"); // masuk pertama
stack.push("Piring 2"); // masuk kedua
stack.push("Piring 3"); // masuk ketiga → terakhir
console.log(stack.pop()); // "Piring 3" — keluar pertama (LIFO)

// Queue — FIFO: push (tambah) + shift (ambil)
let queue = [];
queue.push("Orang 1"); // masuk pertama
queue.push("Orang 2"); // masuk kedua
queue.push("Orang 3"); // masuk ketiga → terakhir
console.log(queue.shift()); // "Orang 1" — keluar pertama (FIFO)
```

**Perbedaan utama**: Stack pakai `push/pop` (keduanya di ujung — O(1)). Queue pakai `push/shift` (tambah di ujung, ambil dari depan — shift O(n)).

---

## Fungsi

Mengatur alur data berdasarkan **urutan waktu**: Stack untuk undo/redo, backtrack, call stack. Queue untuk antrian, scheduling, BFS.

---

## Code — Fitur Undo/Redo Sederhana dengan Stack

```javascript
class UndoManager {
  constructor() {
    this.undoStack = [];   // Stack untuk undo
    this.redoStack = [];   // Stack untuk redo
  }

  // Simpan state baru
  simpan(state) {
    this.undoStack.push(state);
    this.redoStack = [];   // Redo direset saat ada aksi baru
    console.log(`Menyimpan: "${state}"`);
    this.tampilkanStatus();
  }

  // Undo — ambil dari undoStack, pindahkan ke redoStack
  undo() {
    if (this.undoStack.length <= 1) {
      console.log("Tidak bisa undo lagi");
      return null;
    }
    const state = this.undoStack.pop();
    this.redoStack.push(state);
    const current = this.undoStack[this.undoStack.length - 1];
    console.log(`Undo: kembali ke "${current}"`);
    this.tampilkanStatus();
    return current;
  }

  // Redo — ambil dari redoStack, kembalikan ke undoStack
  redo() {
    if (this.redoStack.length === 0) {
      console.log("Tidak bisa redo lagi");
      return null;
    }
    const state = this.redoStack.pop();
    this.undoStack.push(state);
    console.log(`Redo: maju ke "${state}"`);
    this.tampilkanStatus();
    return state;
  }

  tampilkanStatus() {
    console.log(`  UndoStack: [${this.undoStack.join(" | ")}]`);
    console.log(`  RedoStack: [${this.redoStack.join(" | ")}]`);
  }
}

// Demo
const editor = new UndoManager();
editor.simpan("Rumah: pondasi");          // State 1
editor.simpan("Rumah: tembok");           // State 2
editor.simpan("Rumah: atap");             // State 3
editor.undo();  // Kembali ke "tembok"
editor.undo();  // Kembali ke "pondasi"
editor.redo();  // Maju ke "tembok"
editor.simpan("Rumah: cat");              // Aksi baru → redoStack reset

// Contoh Queue — Antrian Pekerjaan
class JobQueue {
  constructor() {
    this.queue = [];
  }

  tambah(job) {
    this.queue.push(job);
    console.log(`Job "${job}" ditambahkan. Antrian: ${this.queue.length}`);
  }

  proses() {
    if (this.queue.length === 0) {
      console.log("Semua job selesai");
      return null;
    }
    const job = this.queue.shift();
    console.log(`Memproses: "${job}". Sisa: ${this.queue.length}`);
    return job;
  }

  lihatAntrian() {
    return [...this.queue];
  }
}

const antrian = new JobQueue();
antrian.tambah("Pasang bata");
antrian.tambah("Pasang atap");
antrian.tambah("Cat tembok");
antrian.proses(); // "Pasang bata" — pertama masuk, pertama keluar
antrian.proses(); // "Pasang atap"
antrian.proses(); // "Cat tembok"
antrian.proses(); // Semua selesai
```

---

## Analogi: Membangun Rumah (Tumpukan Piring & Antrian Kasir)

| Konsep | Stack — Tumpukan Piring | Queue — Antrian Kasir |
|---|---|---|
| `push()` | Taruh piring baru di atas tumpukan | Orang datang dan ambil nomor antrian (di belakang) |
| `pop()` | Ambil piring paling atas | — |
| `shift()` | — | Orang dipanggil sesuai nomor (dari depan) |
| LIFO | Piring terakhir ditaruh = pertama diambil | — |
| FIFO | — | Orang pertama datang = pertama dilayani |
| Top / Front | Paling atas (tumpukan) | Paling depan (antrian) |
| **Use case** | Undo/redo, Call Stack, Back button | Event loop, Printer queue, Task scheduler |

Bayangkan di **proyek rumah**:
- **Stack** = tumpukan piring di dapur proyek. Piring terakhir yang dicuci ditaruh paling atas — dan akan dipakai duluan.
- **Queue** = antrian di kasir toko material. Pembeli pertama yang datang akan dilayani duluan, tanpa potong antrian.

---

## Dipakai Untuk Apa

**Stack**:
- **Undo/Redo** — editor teks, desain, gambar
- **Call Stack** — bagaimana JS melacak fungsi yang dipanggil
- **Back button browser** — history navigasi
- **Matching bracket** — validasi kurung `{}`, `()`, `[]`
- **DFS** (Depth-First Search) — algoritma pencarian

**Queue**:
- **Event Loop** — antrian callback di JavaScript
- **Printer / task scheduler** — job diproses sesuai urutan
- **BFS** (Breadth-First Search) — algoritma pencarian
- **Antrian aplikasi** — customer service, pemesanan tiket
- **Streaming data** — buffer data yang diterima berurutan

---

## Kesalahan Umum

| Kesalahan | Contoh | Penjelasan |
|---|---|---|
| Queue pakai `pop()` | `queue.pop()` untuk ambil antrian | Salah — Queue harus ambil dari depan (`shift`) |
| Stack pakai `shift()` | `stack.shift()` untuk ambil | Salah — Stack harus ambil dari atas (`pop`) |
| Lupa cek kosong | `stack.pop()` saat array kosong | Return `undefined` — selalu cek `length > 0` |
| Queue dengan `unshift/pop` | Tambah di depan, ambil dari belakang | Bisa tapi tidak intuitif — konsistenlah |
| Shift itu O(n) | Queue besar dengan shift | `shift` lambat untuk data besar — pertimbangkan Queue sesungguhnya (linked list) |

---

## Hubungan dengan Materi Sebelumnya

- **Materi 24 (Array)**: Stack & Queue adalah **pola penggunaan khusus** dari array — method `push/pop/shift/unshift` yang sudah dipelajari.
- **Materi 30 (Map & Set)**: Semua struktur data ini adalah tools untuk mengatur data — pilih sesuai kebutuhan akses.
- **Materi 32 (Function)**: Call Stack JavaScript adalah implementasi Stack nyata — setiap fungsi dipanggil, masuk ke stack.
- **Level 3 (Event Loop)**: Event Loop menggunakan Queue untuk antrian callback — materi ini jadi fondasi penting.

---

## Soal Latihan

### Soal 1 (Mudah)
Buat Stack manual dengan array. Push angka 1, 2, 3. Pop 1 kali. Cetak isi stack dan nilai yang di-pop.

**Jawaban**:
```javascript
const stack = [];
stack.push(1);
stack.push(2);
stack.push(3);
console.log("Pop:", stack.pop()); // 3
console.log("Isi stack:", stack); // [1, 2]
```

### Soal 2 (Sedang)
Buat Queue manual dengan array. Push "A", "B", "C". Shift 2 kali. Cetak isi queue dan nilai yang di-shift.

**Jawaban**:
```javascript
const queue = [];
queue.push("A");
queue.push("B");
queue.push("C");
console.log("Layan:", queue.shift()); // "A"
console.log("Layan:", queue.shift()); // "B"
console.log("Sisa antrian:", queue);  // ["C"]
```

### Soal 3 (Tantangan)
Buat fungsi `balikkanStack(arr)` yang menerima array, lalu menggunakan **Stack** (push/pop) untuk membalikkan urutannya. Jangan pakai `.reverse()`.

**Jawaban**:
```javascript
function balikkanStack(arr) {
  const stack = [];
  // Push semua elemen ke stack
  for (const item of arr) {
    stack.push(item);
  }
  // Pop semua — otomatis terbalik (LIFO)
  const hasil = [];
  while (stack.length > 0) {
    hasil.push(stack.pop());
  }
  return hasil;
}

console.log(balikkanStack([1, 2, 3, 4, 5])); // [5, 4, 3, 2, 1]
console.log(balikkanStack(["a", "b", "c"]));  // ["c", "b", "a"]
```
