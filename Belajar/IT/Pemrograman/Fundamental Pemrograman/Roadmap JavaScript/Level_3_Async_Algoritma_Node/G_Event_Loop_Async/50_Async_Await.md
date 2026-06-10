# 50. async/await — Async Code seperti Synchronous

**Benang Merah**: Materi 49 (Promise) menyelesaikan Callback Hell dengan `.then()` chain. Tapi chain panjang tetap kurang enak dibaca. **async/await** hadir sebagai syntactic sugar — kode async terlihat seperti synchronous. Lanjut ke Materi 51 untuk pola lanjutan.

---

## Penjelasan

**async/await** adalah sintaks ES2017 yang membuat kode Promise terlihat seperti kode synchronous biasa.

- `async function` → selalu mengembalikan Promise
- `await` → menunggu Promise selesai (hanya bisa di dalam `async`)

```javascript
async function bangunRumah() {
  const pondasi = await galiPondasi();
  const bata = await pasangBata();
  const atap = await pasangAtap();
  return "Rumah selesai!";
}

bangunRumah().then(console.log);
// Tanpa await:
// const hasil = bangunRumah(); // ❌ Promise, bukan string
```

---

## Fungsi

Membuat kode async **lebih bersih, lebih pendek, lebih mudah dibaca dan di-debug** — mirip kode synchronous. Error handling pakai `try/catch` yang familiar.

---

## Cara Pengimplementasian

### 1. Refactor Promise Chain → async/await

```javascript
// === PROMISE CHAIN (Materi 49) ===
function galiPondasi() {
  return new Promise((r) => setTimeout(() => { console.log("1. Pondasi"); r(); }, 1000));
}
function pasangBata() {
  return new Promise((r) => setTimeout(() => { console.log("2. Bata"); r(); }, 1000));
}
function pasangAtap() {
  return new Promise((r) => setTimeout(() => { console.log("3. Atap"); r(); }, 1000));
}

// Promise chain
galiPondasi()
  .then(() => pasangBata())
  .then(() => pasangAtap())
  .then(() => console.log("Selesai (Promise chain)"));

// === ASYNC/AWAIT ===
async function bangun() {
  await galiPondasi();       // tunggu selesai
  await pasangBata();        // baru lanjut
  await pasangAtap();
  console.log("Selesai (async/await)");
}
bangun();
```

### 2. Error Handling dengan try/catch

```javascript
async function cekMaterial(jenis) {
  try {
    const hasil = await pesanMaterial(jenis); // dari Promise
    console.log(`${hasil} — lanjut bangun`);
  } catch (err) {
    console.error(`Gagal pesan ${jenis}:`, err.message);
  } finally {
    console.log("Selesai cek material");
  }
}

cekMaterial("bata");
```

### 3. Parallel dengan await Promise.all()

```javascript
async function pesanSemuaMaterial() {
  console.log("Mulai pesan semua material...");

  // ❌ SEQUENTIAL — lambat, tunggu satu per satu
  // const bata = await pesanMaterial("bata", 3000);
  // const semen = await pesanMaterial("semen", 2000);
  // const kayu = await pesanMaterial("kayu", 1000);
  // Total: 6 detik

  // ✅ PARALLEL — semua jalan bersamaan
  const [bata, semen, kayu] = await Promise.all([
    pesanMaterial("bata", 3000),
    pesanMaterial("semen", 2000),
    pesanMaterial("kayu", 1000)
  ]);
  // Total: ~3 detik (terlama, bukan dijumlah)

  console.log({ bata, semen, kayu });
}
pesanSemuaMaterial();
```

### 4. await dalam Loop

```javascript
async function prosesMaterialBertahap(items) {
  for (const item of items) {
    // Sequential — tunggu satu selesai
    const hasil = await pesanMaterial(item, 500);
    console.log(hasil);
  }
}

// Parallel dengan map
async function prosesMaterialParallel(items) {
  const hasil = await Promise.all(
    items.map((item) => pesanMaterial(item, 500))
  );
  console.log(hasil);
}
```

---

## Analogi Membangun Rumah — Eskalator vs Tangga

| async/await | Proyek Rumah |
|---|---|
| `async function` | **Tukang level senior** — bisa delegasi kerja |
| `await` | **Tunggu material datang** — berdiri di lokasi sampai tiba |
| `try/catch` | **Asuransi proyek** — kalau ada masalah, klaim |
| `Promise.all()` await | Kirim **rombongan tukang** — kerja paralel, semua sekaligus |
| `return` value | Laporan **"Rumah selesai"** — walaupun fungsi baliknya Promise |
| `await` di loop sequential | Tukang pasang bata **satu per satu** — rapi, tapi lambat |
| Tanpa `await` | Tukang lanjut kerja **tanpa material** — error! |

Bayangkan **eskalator vs tangga**. Tangga (Promise chain): Anda naik satu anak tangga, berhenti, naik lagi, berhenti. Eskalator (async/await): Anda tinggal berdiri, eskalator yang bekerja — terasa seperti berjalan biasa, tapi hasilnya sama. Keduanya sampai ke lantai atas. async/await adalah eskalator — Anda naik tanpa mikir mekanisme di bawahnya (Promise & Event Loop).

---

## Dipakai Untuk Apa

- **Semua kode async modern** — lebih bersih dari Promise chain
- **API calls** — `const data = await fetch(url).then(r => r.json())`
- **Database operations** — `const user = await db.findUser(id)`
- **File operations** — `const data = await fs.promises.readFile(path)`
- **Sequential async flow** — ketika langkah berikutnya bergantung hasil sebelumnya

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Lupa `async` di function | `function f() { await x; }` | SyntaxError |
| `await` di non-Promise | `const x = await "hello"` | Berhasil (dibungkus Promise.resolve) tapi tak berguna |
| Sequential paralel yang salah | `await a(); await b()` padahal independent | Lebih lambat dari perlu |
| Lupa `try/catch` | `await promiseGagal()` | Unhandled promise rejection |
| `await` di dalam `forEach` | `arr.forEach(async x => await f(x))` | Tidak nunggu — forEach tidak tahu async |
| Lupa async return Promise | `async () => { return "x" }` | Tetap Promise — perlu await atau .then() |

---

## Hubungan dengan Materi Sebelumnya

- Materi 49 (Promise) → async/await adalah **syntactic sugar** — di belakang layar tetap Promise
- Materi 47 (Event Loop) → `await` melepaskan Call Stack, Event Loop yang melanjutkan nanti
- Materi 48 (Callback) → async/await adalah evolusi akhir pola async — dari callback → Promise → async/await
- Materi 51 (Advanced Async) → Pola retry & timeout dengan async/await

---

## Soal Latihan

### Soal 1 (Mudah)
Apa output kode berikut?
```javascript
function delay(ms) {
  return new Promise(r => setTimeout(r, ms));
}
async function main() {
  console.log("Mulai");
  await delay(1000);
  console.log("Setelah 1 detik");
  await delay(500);
  console.log("Setelah 500ms lagi");
}
main();
console.log("Sync");
```

**Jawaban**:
```
Mulai
Sync
(1 detik)
Setelah 1 detik
(500ms)
Setelah 500ms lagi
```
`main()` berjalan, `Mulai` sync, `await` melepas control → `Sync` dari luar jalan. Setelah delay, lanjut.

### Soal 2 (Sedang)
Perbaiki kode berikut yang jalan sequential padahal bisa parallel:
```javascript
async function fetchAll() {
  const a = await fetch("/api/a").then(r => r.json());
  const b = await fetch("/api/b").then(r => r.json());
  const c = await fetch("/api/c").then(r => r.json());
  console.log(a, b, c);
}
```

**Jawaban**:
```javascript
async function fetchAll() {
  const [a, b, c] = await Promise.all([
    fetch("/api/a").then(r => r.json()),
    fetch("/api/b").then(r => r.json()),
    fetch("/api/c").then(r => r.json())
  ]);
  console.log(a, b, c);
}
// Perubahan: dari sequential (~300ms tiap request = ~900ms)
// ke parallel (~300ms total — paling lambat)
```

### Soal 3 (Tantangan)
Buat fungsi async `ambilDataUser(id)` yang:
- Jika id positif: tunggu 500ms, return `{ id, name: "User " + id }`
- Jika id <= 0: throw error `"ID harus positif"`
- Jika id string: throw error `"ID harus angka"`
- Tangani error dengan try/catch dan cetak pesan error yang sesuai

**Jawaban**:
```javascript
function delay(ms) {
  return new Promise(r => setTimeout(r, ms));
}

async function ambilDataUser(id) {
  if (typeof id !== "number") throw new Error("ID harus angka");
  if (id <= 0) throw new Error("ID harus positif");

  await delay(500);
  return { id, name: `User ${id}` };
}

async function main() {
  const ids = [1, -1, "abc", 3];

  for (const id of ids) {
    try {
      const user = await ambilDataUser(id);
      console.log("Sukses:", user);
    } catch (err) {
      console.error(`Gagal id=${id}: ${err.message}`);
    }
  }
}

main();
// Output:
// Sukses: { id: 1, name: "User 1" }
// Gagal id=-1: ID harus positif
// Gagal id=abc: ID harus angka
// Sukses: { id: 3, name: "User 3" }
```
