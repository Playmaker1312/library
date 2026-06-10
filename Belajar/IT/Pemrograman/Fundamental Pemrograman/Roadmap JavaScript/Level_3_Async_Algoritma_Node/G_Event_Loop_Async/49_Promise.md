# 49. Promise — Solusi Callback Hell

**Benang Merah**: Materi 48 (Callback) memperkenalkan pola async pertama, tapi berakhir dengan **Callback Hell**. Promise hadir sebagai solusi — objek yang merepresentasikan **nilai di masa depan** (janji). Lanjut ke Materi 50 (async/await) yang menyempurnakannya.

---

## Penjelasan

**Promise** adalah objek yang menjanjikan akan memberikan **satu nilai di masa depan**: sukses (fulfilled) atau gagal (rejected).

```javascript
const janji = new Promise((resolve, reject) => {
  const selesai = true;
  if (selesai) {
    resolve("Rumah selesai dibangun");
  } else {
    reject(new Error("Proyek gagal"));
  }
});
```

### States (3 status)

```
┌─────────┐
│ pending │ ← awal — masih nunggu
└────┬────┘
     │
     ├── ✅ fulfilled → resolve() dipanggil
     │
     └── ❌ rejected  → reject() dipanggil
```

```javascript
const pesanBata = new Promise((resolve) => {
  setTimeout(() => resolve("Bata terkirim"), 1000);
});
console.log(pesanBata); // Promise { <pending> }
// 1 detik kemudian → Promise { <fulfilled>: "Bata terkirim" }
```

---

## Fungsi

Promise memberikan **kontrol** atas operasi async: chain operasi, tangani error terpusat dengan `.catch()`, dan jalankan operasi paralel dengan `.all()`, `.race()`, `.allSettled()`, `.any()`.

---

## Cara Pengimplementasian

### 1. Promise Sederhana

```javascript
function pesanMaterial(jenis, lama = 1000) {
  return new Promise((resolve) => {
    setTimeout(() => resolve(`${jenis} siap`), lama);
  });
}

pesanMaterial("bata")
  .then((hasil) => console.log(hasil)); // "bata siap"

pesanMaterial("semen")
  .then(console.log); // "semen siap"
```

### 2. Refactor Callback Hell → Promise Chain

```javascript
// === CALLBACK HELL (Materi 48) ===
function galiPondasi(cb) { setTimeout(() => { console.log("1"); cb(); }, 1000); }
function pasangBata(cb) { setTimeout(() => { console.log("2"); cb(); }, 1000); }
function pasangAtap(cb) { setTimeout(() => { console.log("3"); cb(); }, 1000); }

galiPondasi(() => {
  pasangBata(() => {
    pasangAtap(() => console.log("Selesai (callback hell)"));
  });
});

// === PROMISE CHAIN ===
function galiPondasiP() {
  return new Promise((r) => setTimeout(() => { console.log("1"); r(); }, 1000));
}
function pasangBataP() {
  return new Promise((r) => setTimeout(() => { console.log("2"); r(); }, 1000));
}
function pasangAtapP() {
  return new Promise((r) => setTimeout(() => { console.log("3"); r(); }, 1000));
}

galiPondasiP()
  .then(() => pasangBataP())
  .then(() => pasangAtapP())
  .then(() => console.log("Selesai (Promise chain)"));
```

### 3. .then(), .catch(), .finally()

```javascript
function ambilDataUser(id) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (id > 0) {
        resolve({ id, nama: "Budi", peran: "tukang" });
      } else {
        reject(new Error("ID tidak valid"));
      }
    }, 500);
  });
}

ambilDataUser(1)
  .then((user) => console.log(user.nama))       // "Budi"
  .catch((err) => console.error(err.message))    // skip if success
  .finally(() => console.log("Selesai"));        // selalu jalan
```

### 4. Promise.all(), .race(), .allSettled(), .any()

```javascript
const pesanBata = pesanMaterial("bata", 3000);
const pesanSemen = pesanMaterial("semen", 2000);
const pesanKayu = pesanMaterial("kayu", 1000);

// Semua selesai
Promise.all([pesanBata, pesanSemen, pesanKayu])
  .then((hasil) => console.log("Semua:", hasil));
  // ~3 detik: ["bata siap", "semen siap", "kayu siap"]

// Yang paling cepat
Promise.race([pesanBata, pesanSemen, pesanKayu])
  .then((pertama) => console.log("Tercepat:", pertama));
  // "kayu siap" (1000ms)

// Tunggu semua selesai (gagal tetap direkam)
Promise.allSettled([pesanBata, Promise.reject("Gagal"), pesanKayu])
  .then((hasil) => console.log("All settled:", hasil));
  // [{status:"fulfilled",value:"bata siap"}, {status:"rejected",reason:"Gagal"}, ...]

// Ambil yang pertama fulfilled
const p1 = Promise.reject("Gagal 1");
const p2 = pesanMaterial("semen", 500);
const p3 = Promise.reject("Gagal 2");
Promise.any([p1, p2, p3])
  .then((pertamaOK) => console.log("Pertama berhasil:", pertamaOK));
  // "semen siap" (abaikan yang reject)
```

---

## Analogi Membangun Rumah — Surat Pesanan Material

| Promise | Proyek Rumah |
|---|---|
| `new Promise()` | **Surat pesanan** material — Anda order, dapat tanda terima |
| `pending` | Material **dalam perjalanan** — belum sampai |
| `fulfilled` / `resolve()` | Material **diterima** — tandatangan terima |
| `rejected` / `reject()` | Material **rusak/cancel** — komplain ke supplier |
| `.then()` | Langkah **selanjutnya** setelah material datang |
| `.catch()` | **Protes/klaim** kalau ada masalah |
| `.finally()` | **Catat administrasi** — apa pun hasilnya, tetap dicatat |
| `Promise.all()` | Kirim **semua** material sekaligus, tunggu semua sampai |
| `Promise.race()` | Ambil material yang **paling cepat** sampai |
| `Promise.allSettled()` | Cek **semua pesanan** — yang sampai dan yang gagal |
| `Promise.any()` | Ambil **satu saja** yang berhasil duluan |

Bayangkan Anda mandor proyek. Anda buat **surat pesanan** (Promise) ke supplier bata. Surat itu adalah **janji** — bukan barangnya. Anda bisa lanjut kerja lain. Supplier kirim bata → surat terpenuhi (fulfilled). Bata rusak → klaim (rejected). Kalau pesan banyak supplier bersamaan, `Promise.all()` menunggu semua datang sebelum melanjutkan pembangunan.

---

## Dipakai Untuk Apa

- **fetch API** — `fetch(url).then(res => res.json())`
- **Database query** — `db.query("SELECT *").then(rows => ...)`
- **File I/O modern** — `fs.promises.readFile(path)`
- **Semua operasi async yang butuh chaining**
- **Paralelisasi** — multiple request API bersamaan

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Lupa return Promise di chain | `.then(() => { promiseFunc(); })` | Chain putus, undefined |
| Tidak pakai .catch() | Promise.reject("err") tanpa catch | Unhandled promise rejection |
| Nested Promise (Promise Hell) | `.then(() => { new Promise(...) })` | Sama jeleknya dengan Callback Hell |
| Promise.all gagal total | Satu reject, semua reject | Perlu .allSettled() jika ingin toleran |
| Lupa Promise itu async | `const x = promise; console.log(x)` | Dapat Promise object, bukan nilai |

---

## Hubungan dengan Materi Sebelumnya

- Materi 47 (Event Loop) → `.then()` masuk **Microtask Queue** — prioritas di atas setTimeout
- Materi 48 (Callback) → Promise adalah **abstraksi** callback — resolve/reject internal
- Materi 50 (async/await) → Sintaks **sugar** di atas Promise
- Materi 51 (Advanced Async) → `Promise.allSettled()` untuk retry pattern

---

## Soal Latihan

### Soal 1 (Mudah)
Apa output kode berikut?
```javascript
const janji = new Promise((resolve) => {
  console.log("Eksekusi dalam Promise");
  resolve("Selesai");
});
console.log("Setelah Promise");
janji.then(console.log);
```

**Jawaban**:
```
Eksekusi dalam Promise
Setelah Promise
Selesai
```
Kode dalam Promise (executor) jalan **synchronous** saat Promise dibuat. `.then()` async.

### Soal 2 (Sedang)
Refactor kode callback berikut menjadi Promise chain:
```javascript
getUserId("budi", (id) => {
  getUserProfile(id, (profile) => {
    getUserPosts(profile.id, (posts) => {
      console.log(posts);
    });
  });
});
```

**Jawaban**:
```javascript
getUserIdP("budi")
  .then((id) => getUserProfileP(id))
  .then((profile) => getUserPostsP(profile.id))
  .then((posts) => console.log(posts))
  .catch((err) => console.error(err));
```
Asumsi setiap fungsi `getXxxP` mengembalikan Promise. Error terpusat di `.catch()`.

### Soal 3 (Tantangan)
Buat fungsi `timeout(ms)` yang mengembalikan Promise **reject** setelah `ms` (untuk timeout). Lalu buat `denganTimeout(promise, ms)` yang balapan antara promise asli dengan timeout — jika melebihi ms, tolak dengan `"Timeout"`.

**Jawaban**:
```javascript
function timeout(ms) {
  return new Promise((_, reject) => {
    setTimeout(() => reject(new Error("Timeout")), ms);
  });
}

function denganTimeout(promise, ms) {
  return Promise.race([promise, timeout(ms)]);
}

// Simulasi operasi lambat (2 detik) dengan timeout 1 detik
const lambat = new Promise((r) => setTimeout(() => r("Data siap"), 2000));

denganTimeout(lambat, 1000)
  .then(console.log)
  .catch((err) => console.error("Gagal:", err.message)); // "Gagal: Timeout"

// Simulasi operasi cepat (500ms) dengan timeout 1 detik
const cepat = new Promise((r) => setTimeout(() => r("Data cepat"), 500));

denganTimeout(cepat, 1000)
  .then(console.log) // "Data cepat"
  .catch((err) => console.error(err.message));
```
