# 51. Pola Async Lanjutan & Error Handling

**Benang Merah**: Ini **PENUTUP** bagian async. Materi 46-50 membahas runtime, event loop, callback, promise, async/await. Sekarang kita bahas **pola produksi** — retry, timeout, error handling robust, dan async dalam loop. Lanjut ke Materi 52 (Node.js Fundamentals).

---

## Penjelasan

Di produksi, async tidak cukup dengan `.then()` dan `await` saja. Kita butuh pola untuk menangani **kegagalan jaringan**, **timeout**, **retry otomatis**, dan **error global** yang tidak tertangani.

### Jenis Error Async

```javascript
// 1. Error synchronous dalam Promise — otomatis jadi rejected
const p1 = new Promise(() => { throw new Error("Gagal"); });

// 2. Error dari reject()
const p2 = new Promise((_, reject) => reject(new Error("Ditolak")));

// 3. Error dari async function — otomatis jadi rejected Promise
async function rusak() {
  throw new Error("Async error");
}

// 4. Unhandled promise rejection — error yang tidak ada .catch()-nya
Promise.reject(new Error("Tak tertangani"));
// ❌ UnhandledPromiseRejection — aplikasi bisa crash!
```

---

## Fungsi

Pola lanjutan memastikan aplikasi **tidak crash** saat terjadi kesalahan async, bisa **pulih otomatis** (retry), dan memberikan **batas waktu** (timeout) untuk operasi yang terlalu lambat.

---

## Cara Pengimplementasian

### 1. Retry Pattern — Coba Ulang Saat Gagal

```javascript
function delay(ms) {
  return new Promise(r => setTimeout(r, ms));
}

async function fetchWithRetry(url, maxRetries = 3) {
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      console.log(`Mencoba [${attempt}/${maxRetries}]: ${url}`);
      const response = await fetch(url);
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      return await response.json();
    } catch (err) {
      console.log(`Gagal percobaan ${attempt}: ${err.message}`);
      if (attempt === maxRetries) {
        throw new Error(`Gagal setelah ${maxRetries} percobaan: ${url}`);
      }
      await delay(1000 * attempt); // backoff: 1s, 2s, 3s
    }
  }
}

// Pakai:
// const data = await fetchWithRetry("https://api.example.com/data", 3);
```

### 2. Timeout Pattern — Batas Waktu

```javascript
function timeoutPromise(promise, ms) {
  let timer;
  const timeout = new Promise((_, reject) => {
    timer = setTimeout(() => reject(new Error(`Timeout setelah ${ms}ms`)), ms);
  });
  return Promise.race([promise, timeout]).finally(() => clearTimeout(timer));
}

// Simulasi
async function cobaTimeout() {
  const lambat = new Promise(r => setTimeout(() => r("Data siap"), 2000));

  try {
    const hasil = await timeoutPromise(lambat, 1000);
    console.log(hasil);
  } catch (err) {
    console.error(err.message); // "Timeout setelah 1000ms"
  }
}
cobaTimeout();
```

### 3. Unhandled Promise Rejection — Tangkal Global

```javascript
// Di browser:
// window.addEventListener("unhandledrejection", (event) => {
//   console.error("Unhandled rejection:", event.reason);
//   event.preventDefault(); // cegah crash
// });

// Di Node.js:
process.on("unhandledRejection", (reason, promise) => {
  console.error("Unhandled Rejection at:", promise, "reason:", reason);
  // Log ke file, kirim alert, dll.
});

process.on("rejectionHandled", (promise) => {
  console.log("Rejection sudah ditangani:", promise);
});
```

### 4. Async dalam Loop — Sequential vs Parallel

```javascript
async function sequentialProcessing(items) {
  const results = [];
  for (const item of items) {
    results.push(await processItem(item)); // satu per satu
  }
  return results;
}

async function parallelProcessing(items) {
  const promises = items.map((item) => processItem(item));
  return Promise.all(promises); // semua bersamaan
}

async function parallelWithLimit(items, limit = 2) {
  const results = [];
  for (let i = 0; i < items.length; i += limit) {
    const batch = items.slice(i, i + limit);
    const batchResults = await Promise.all(batch.map(processItem));
    results.push(...batchResults);
    console.log(`Batch selesai: ${i / limit + 1}`);
  }
  return results;
}
```

### 5. Error Handling Komprehensif

```javascript
async function prosesBangunRumah() {
  try {
    const pondasi = await galiPondasi();
    const dinding = await pasangBata(pondasi);
    const atap = await pasangAtap(dinding);
    return atap;
  } catch (err) {
    // Kategorisasi error
    if (err.message.includes("material")) {
      console.error("Error material — pesan ulang");
      return await retryPesanMaterial();
    }
    if (err.message.includes("waktu")) {
      console.error("Error waktu — perpanjang jadwal");
      return await jadwalUlang();
    }
    throw err; // lempar lagi kalau tak bisa handle
  }
}

async function main() {
  try {
    const hasil = await prosesBangunRumah();
    console.log("Rumah jadi:", hasil);
  } catch (err) {
    console.error("Proyek gagal total:", err);
    // Kirim notifikasi ke mandor
  }
}
```

---

## Analogi Membangun Rumah — Retry & Timeout

| Pola Async | Proyek Rumah |
|---|---|
| **Retry** | Material rusak — **pesan ulang** ke supplier (maks 3x) |
| **Timeout** | Supplier tidak kirim dalam 7 hari — **batalkan**, cari supplier lain |
| **Unhandled rejection** | Tukang lapor masalah tapi mandor tidak dengar — **masalah membesar** |
| **Sequential loop** | Pasang bata **satu per satu** — aman, lambat |
| **Parallel loop** | Kirim **semua tukang** ke semua ruangan — cepat, tapi boros resource |
| **Parallel with limit** | **2 tukang** per kamar — seimbang antara cepat & resource |
| **Error kategorisasi** | Kebakaran (error) — panggil pemadam. Banjir — panggil pompa. **Tiap error beda solusi** |

Bayangkan proyek rumah. Supplier kirim bata rusak — Anda **retry** (pesan ulang maksimal 3 kali). Kalau tidak datang setelah batas waktu — **timeout**, cari supplier baru. Kalau mandor tidak dengar laporan tukang (unhandled rejection) — masalah kecil jadi besar. Proses bertahap: pondasi dulu, lalu bata, lalu atap — **sequential**. Tapi pasang keramik di 3 kamar bisa **parallel**. Dengan pembatasan jumlah tukang agar tidak saling tabrak.

---

## Dipakai Untuk Apa

- **API calls** — retry saat jaringan tidak stabil, timeout jika server lambat
- **File upload** — retry dengan exponential backoff
- **Database connection** — retry saat koneksi drop
- **Web scraping** — parallel with limit agar tidak kena rate limit
- **Error monitoring** — global handler untuk logging semua unhandled rejection
- **Batch processing** — proses ribuan item dalam batch paralel

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Tidak ada retry | Fetch gagal langsung crash | Aplikasi tidak resilient |
| Retry tanpa batas | `while(true) await fetch()` | Infinite loop, bombing server |
| Retry tanpa backoff | Langsung retry 10x dalam 1 detik | Memperparah masalah |
| Timeout tidak dibersihkan | `setTimeout(reject, ms)` tanpa clear | Memori bocor, reject setelah selesai |
| Abaikan unhandled rejection | Tidak pasang `process.on` | Error silent, aplikasi flaw |
| `forEach` dengan async | `arr.forEach(async x => await f(x))` | Tidak nunggu, unpredictable |

---

## Hubungan dengan Materi Sebelumnya

- Materi 46 (JS Runtime) → Error async tetap melalui Call Stack & Heap
- Materi 47 (Event Loop) → Retry/timeout melibatkan Macrotask Queue (setTimeout)
- Materi 48-49 (Callback & Promise) → Semua pola dibangun di atas Promise
- Materi 50 (async/await) → Semua contoh menggunakan async/await
- **Materi 52+ (Node.js)** → Pola ini penting di Node.js untuk file system, network, server

---

## Soal Latihan

### Soal 1 (Mudah)
Apa output kode berikut?
```javascript
async function test() {
  try {
    await Promise.reject(new Error("Gagal"));
    console.log("A");
  } catch {
    console.log("B");
  } finally {
    console.log("C");
  }
}
test();
```

**Jawaban**:
```
B
C
```
`Promise.reject` masuk catch, cetak `B`. `finally` selalu jalan, cetak `C`. `A` tidak pernah tercetak karena reject sebelum sampai ke `console.log("A")`.

### Soal 2 (Sedang)
Apa masalah kode berikut dan bagaimana memperbaikinya?
```javascript
async function kirimMaterial(items) {
  items.forEach(async (item) => {
    await process(item);
  });
  console.log("Semua selesai");
}
```

**Jawaban**:
**Masalah**: `forEach` tidak peduli async. `await` di callback tidak memengaruhi fungsi luar. `"Semua selesai"` akan dicetak **sebelum** semua `process()` selesai.

**Perbaikan** (sequential):
```javascript
async function kirimMaterial(items) {
  for (const item of items) {
    await process(item);
  }
  console.log("Semua selesai");
}
```
**Atau** (parallel):
```javascript
async function kirimMaterial(items) {
  await Promise.all(items.map(item => process(item)));
  console.log("Semua selesai");
}
```

### Soal 3 (Tantangan)
Buat fungsi `stabilRequest(url, options)` dengan:
- Maksimal 3 retry
- Exponential backoff (1s, 2s, 4s)
- Timeout 5 detik per percobaan
- Hanya retry jika error adalah `TypeError` (jaringan) atau HTTP 5xx
- Jika HTTP 4xx (client error) — langsung throw, jangan retry

**Jawaban**:
```javascript
function delay(ms) {
  return new Promise(r => setTimeout(r, ms));
}

function timeoutPromise(promise, ms) {
  let timer;
  const timeout = new Promise((_, reject) => {
    timer = setTimeout(() => reject(new Error("Timeout")), ms);
  });
  return Promise.race([promise, timeout]).finally(() => clearTimeout(timer));
}

async function stabilRequest(url, options = {}) {
  const maxRetries = 3;
  const timeoutMs = 5000;

  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      const controller = new AbortController();
      const response = await timeoutPromise(
        fetch(url, { ...options, signal: controller.signal }),
        timeoutMs
      );

      if (!response.ok) {
        if (response.status >= 400 && response.status < 500) {
          throw new Error(`Client error ${response.status} — tidak diretry`);
        }
        throw new Error(`Server error ${response.status}`);
      }

      return await response.json();
    } catch (err) {
      const isNetworkError = err instanceof TypeError;
      const isServerError = err.message.includes("Server error");
      const isTimeout = err.message === "Timeout";
      const isClientError = err.message.includes("Client error");

      if (isClientError) throw err; // 4xx — langsung gagal
      if (attempt === maxRetries) {
        throw new Error(`Gagal total setelah ${maxRetries} percobaan: ${err.message}`);
      }

      const backoff = Math.pow(2, attempt - 1) * 1000; // 1s, 2s, 4s
      console.log(`Percobaan ${attempt} gagal (${err.message}). Retry dalam ${backoff}ms...`);
      await delay(backoff);
    }
  }
}

// Pakai:
// stabilRequest("https://api.example.com/data")
//   .then(data => console.log(data))
//   .catch(err => console.error(err.message));
```
