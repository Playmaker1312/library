# 106 — Concurrency — Web Workers & Worker Threads

## 1. Penjelasan

JavaScript adalah **single-threaded** — hanya satu thread yang mengeksekusi kode (main thread). Operasi berat (komputasi, loop besar, parsing data) akan memblokir thread utama, menyebabkan UI freeze di browser atau request blocking di server.

**Web Workers** (browser) dan **Worker Threads** (Node.js) memungkinkan JavaScript menjalankan kode di thread terpisah (background thread). Komunikasi antara main thread dan worker dilakukan via **message passing** (`postMessage` / `onmessage`), bukan shared memory. Data dikirim sebagai salinan (structured clone), bukan referensi.

**SharedArrayBuffer** memungkinkan berbagi memori antara threads untuk performa maksimal, tetapi memerlukan sinkronisasi dengan **Atomics** (atomic operations) untuk menghindari race condition.

| Karakteristik | Main Thread | Worker Thread |
|---------------|-------------|---------------|
| UI Access | Ya | Tidak |
| DOM Access | Ya | Tidak |
| Komputasi Berat | Memblokir UI | Tidak memblokir |
| Komunikasi | - | Message passing |
| Shared Memory | - | SharedArrayBuffer + Atomics |

## 2. Fungsi

- Menjalankan komputasi berat tanpa memblokir UI (browser)
- Parallel processing data besar (image processing, data transformation)
- Real-time data processing (audio/video encoding, WebSocket aggregation)
- Isolasi kode yang tidak stabil (crash di worker tidak mempengaruhi main thread)
- Memanfaatkan multi-core CPU di Node.js (Worker Threads)

## 3. Code

### Web Worker — Browser

```javascript
// main.js
const worker = new Worker('worker.js');

worker.postMessage({ data: new Array(10000000).fill(1) });

worker.onmessage = (event) => {
  console.log('Hasil dari worker:', event.data);
  worker.terminate();
};

worker.onerror = (error) => {
  console.error('Error di worker:', error.message);
};
```

```javascript
// worker.js
self.onmessage = (event) => {
  const { data } = event;
  const result = data.map(x => x * 2).filter(x => x > 10).reduce((a, b) => a + b, 0);
  self.postMessage(result);
};
```

### Worker Threads — Node.js

```javascript
// main-thread.js
const { Worker } = require('worker_threads');
const path = require('path');

function processInWorker(data) {
  return new Promise((resolve, reject) => {
    const worker = new Worker(path.resolve(__dirname, 'worker-thread.js'), {
      workerData: data,
    });

    worker.on('message', resolve);
    worker.on('error', reject);
    worker.on('exit', (code) => {
      if (code !== 0) reject(new Error(`Worker exit code ${code}`));
    });
  });
}

(async () => {
  const largeArray = Array.from({ length: 10000000 }, (_, i) => i + 1);
  console.log('Memulai komputasi berat di worker...');
  const result = await processInWorker(largeArray);
  console.log('Hasil:', result);
})();
```

```javascript
// worker-thread.js
const { parentPort, workerData } = require('worker_threads');

// Komputasi berat: proses 10 juta data
const result = workerData
  .filter(n => n % 2 === 0)  // Genap saja
  .map(n => n * 3)            // Kali 3
  .reduce((sum, n) => sum + n, 0); // Jumlahkan

parentPort.postMessage(result);
```

### SharedArrayBuffer + Atomics

```javascript
// main.js
const buffer = new SharedArrayBuffer(4 * 100); // 100 integer (4 byte each)
const sharedArray = new Int32Array(buffer);

// Isi data awal
sharedArray[0] = 1;
sharedArray[1] = 2;

const worker = new Worker('worker-sab.js');
worker.postMessage(buffer); // Kirim buffer (bukan salinan)

setTimeout(() => {
  console.log('Counter dari main thread:', Atomics.load(sharedArray, 99));
}, 1000);
```

```javascript
// worker-sab.js
self.onmessage = (event) => {
  const sharedArray = new Int32Array(event.data);

  // Operasi atomik: aman dari race condition
  for (let i = 0; i < 100; i++) {
    Atomics.store(sharedArray, i, i * 10);
  }

  self.postMessage('Selesai mengisi shared array');
};
```

## 4. Analogi Rumah — Satu Tukang vs Subkontraktor

| Konsep JS | Analogi Rumah |
|-----------|---------------|
| Main thread | Satu tukang utama untuk seluruh rumah |
| Single-threaded | Hanya satu orang yang bisa bekerja di ruang utama |
| UI freeze | Tukang sedang cor pondasi → semua aktivitas lain berhenti |
| Web Worker | Subkontraktor — tukang listrik, tukang cat, bekerja sendiri |
| `postMessage` | Subkontraktor kirim laporan lewat HT (handy talky) |
| `onmessage` | Tukang utama terima laporan dari HT |
| Worker terminate | Kontrak subkontraktor dihentikan |
| SharedArrayBuffer | Papan pengumuman bersama — semua pekerja bisa lihat |
| Atomics | Aturan: hanya satu orang boleh menulis di papan dalam satu waktu |
| Structured clone | Fotokopi gambar kerja — subkontraktor punya salinan sendiri |

> **Kapan perlu worker:** Jika Anda membuat tukang utama menunggu komputasi >50ms, saatnya panggil subkontraktor. Contoh: memproses 10 juta data array, image processing, PDF generation.

## 5. Use Case di Proyek Nyata

- **Image/Video processing di browser** — crop, filter, resize tanpa freeze UI
- **WebSocket data aggregation** — worker menerima stream data, mengolah, kirim hasil
- **PDF generation server-side** — worker thread untuk generate PDF tanpa block request lain
- **Code syntax highlighting** — highlight 10000 baris kode di worker
- **Database migration** — worker thread memproses migrasi data dalam batch
- **Real-time dashboard** — worker mengolah data real-time, main thread hanya render

## 6. Kesalahan Umum

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| Worker terlalu kecil tugas | Overhead komunikasi > manfaat | Hanya worker untuk tugas >50ms komputasi |
| Lupa terminate worker | Memory leak (worker tetap hidup) | Panggil `worker.terminate()` atau `worker.unref()` |
| Akses DOM dari worker | Error — worker tidak punya DOM | Worker hanya untuk logika/data, bukan UI |
| Transfer data besar tiap pesan | Lambat karena structured clone | Gunakan Transferable objects atau SharedArrayBuffer |
| Race condition SharedArrayBuffer | Data korup | Gunakan `Atomics.load/store/wait/notify` |
| Worker tidak di-handle error-nya | Aplikasi silent crash | Selalu pasang `worker.onerror` / `worker.on('error')` |

## 7. Benang Merah

```
Materi 105 (Memory Management)
    ↓
Materi 106 (Concurrency — Web Workers & Worker Threads) ← Anda di sini
    ↓
Materi 107 (Performance Optimization)
```

Setelah memahami bagaimana memori dikelola (Materi 105), kini Anda belajar memanfaatkan banyak thread untuk komputasi paralel. Concurrency adalah alat utama untuk meningkatkan performa — dan ini menjadi jembatan langsung ke materi optimasi performa (Materi 107) yang lebih luas.

## 8. Soal & Jawaban

### Soal 1: Easy
Apa yang terjadi jika Anda menjalankan loop `while (true) {}` di main thread browser? Bagaimana cara mencegahnya?

<details>
<summary>Jawaban</summary>
Browser akan freeze — tab tidak bisa di-scroll, tombol tidak responsif, karena main thread terblokir. Solusi: pindahkan loop ke Web Worker, atau gunakan teknik chunking (break loop jadi bagian kecil dengan `requestIdleCallback`/`setTimeout`).
</details>

### Soal 2: Medium
Buatlah fungsi `runHeavyComputation(data)` yang menerima array dan mengembalikan Promise. Fungsi ini membuat Worker Thread untuk menghitung jumlah seluruh elemen array (sum). Gunakan modul `worker_threads` di Node.js.

<details>
<summary>Jawaban</summary>

```javascript
// main.js
const { Worker } = require('worker_threads');
const path = require('path');

function runHeavyComputation(data) {
  return new Promise((resolve, reject) => {
    const worker = new Worker(path.resolve(__dirname, 'sum-worker.js'), {
      workerData: data,
    });
    worker.on('message', resolve);
    worker.on('error', reject);
    worker.on('exit', (code) => {
      if (code !== 0) reject(new Error(`Worker berhenti dengan kode ${code}`));
    });
  });
}

// sum-worker.js
const { parentPort, workerData } = require('worker_threads');
const sum = workerData.reduce((acc, val) => acc + val, 0);
parentPort.postMessage(sum);
```
</details>

### Soal 3: Hard
Buatlah implementasi **thread pool** sederhana dengan Worker Threads. Pool memiliki maksimal 4 worker. Fungsi `pool.exec(data)` mengirim data ke worker yang tersedia. Jika semua worker sibuk, antrikan tugas. Pastikan worker di-reuse (tidak dibuat ulang setiap tugas).

<details>
<summary>Jawaban</summary>

```javascript
// pool.js
const { Worker } = require('worker_threads');
const path = require('path');
const { EventEmitter } = require('events');

class WorkerPool extends EventEmitter {
  constructor(workerFile, maxWorkers = 4) {
    super();
    this.workerFile = workerFile;
    this.maxWorkers = maxWorkers;
    this.workers = [];
    this.queue = [];
    this.activeCount = 0;
    this._init();
  }

  _init() {
    for (let i = 0; i < this.maxWorkers; i++) {
      this._createWorker();
    }
  }

  _createWorker() {
    const worker = new Worker(path.resolve(__dirname, this.workerFile));
    const entry = { worker, busy: false };

    worker.on('message', (result) => {
      entry.busy = false;
      this.activeCount--;
      if (entry.callback) {
        entry.callback.resolve(result);
        entry.callback = null;
      }
      this._processQueue();
    });

    worker.on('error', (err) => {
      entry.busy = false;
      this.activeCount--;
      if (entry.callback) {
        entry.callback.reject(err);
        entry.callback = null;
      }
      // Buat worker baru untuk mengganti yang error
      const idx = this.workers.indexOf(entry);
      if (idx !== -1) {
        this.workers.splice(idx, 1);
        this._createWorker();
      }
      this._processQueue();
    });

    this.workers.push(entry);
  }

  _processQueue() {
    while (this.queue.length > 0 && this.activeCount < this.maxWorkers) {
      const idleWorker = this.workers.find(w => !w.busy);
      if (!idleWorker) break;
      const task = this.queue.shift();
      idleWorker.busy = true;
      idleWorker.callback = task.callback;
      this.activeCount++;
      idleWorker.worker.postMessage(task.data);
    }
  }

  exec(data) {
    return new Promise((resolve, reject) => {
      const callback = { resolve, reject };
      this.queue.push({ data, callback });
      this._processQueue();
    });
  }

  terminate() {
    this.workers.forEach(w => w.worker.terminate());
    this.workers = [];
    this.queue = [];
    this.activeCount = 0;
  }
}

module.exports = WorkerPool;

// worker-task.js
const { parentPort, workerData } = require('worker_threads');

function processData(data) {
  // Contoh: komputasi berat
  return data
    .filter(n => n % 2 === 0)
    .map(n => n ** 2)
    .reduce((sum, n) => sum + n, 0);
}

parentPort.on('message', (data) => {
  const result = processData(data);
  parentPort.postMessage(result);
});

// usage.js
const pool = new WorkerPool('worker-task.js', 4);
const tasks = Array.from({ length: 10 }, (_, i) =>
  Array.from({ length: 100000 }, (_, j) => j + i * 100000)
);

Promise.all(tasks.map(data => pool.exec(data)))
  .then(results => {
    console.log('Semua selesai:', results);
    pool.terminate();
  });
```
</details>
