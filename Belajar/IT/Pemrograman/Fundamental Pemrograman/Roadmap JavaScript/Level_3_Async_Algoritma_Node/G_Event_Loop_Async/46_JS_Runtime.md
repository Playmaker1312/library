# 46. JavaScript Runtime — V8, Call Stack, Memory Heap

**Benang Merah**: Dari Level 2 (Materi 31 — Stack) kita tahu Stack sebagai struktur data LIFO. Sekarang kita lihat Stack sebagai **Call Stack** — jantung eksekusi JavaScript. Ditambah Memory Heap dan V8 engine, ini fondasi sebelum masuk Event Loop (Materi 47) dan Callback (Materi 48).

---

## Penjelasan

**JavaScript Runtime** adalah lingkungan tempat kode JS dijalankan. Di browser: V8 engine. Di server: Node.js (juga V8). Tiga komponen utama:

| Komponen | Fungsi |
|---|---|
| **V8 Engine** | Compile & execute JS (Google, C++) |
| **Call Stack** | Track posisi eksekusi — tumpukan fungsi yang sedang dipanggil |
| **Memory Heap** | Alokasi memori untuk object, array, closure |

```
┌─────────────────────────────────────┐
│         V8 Engine                   │
│  ┌──────────────────┐               │
│  │   Call Stack     │   Memory      │
│  │   (tumpukan      │   Heap        │
│  │    eksekusi)     │   (penyimpan) │
│  └──────────────────┘               │
│         ▲                           │
│         │ pipe                       │
│         ▼                           │
│  ┌──────────────────┐               │
│  │   Event Loop     │   (Materi 47) │
│  │   + Web APIs     │               │
│  └──────────────────┘               │
└─────────────────────────────────────┘
```

**Single-threaded**: Satu Call Stack, satu hal dalam satu waktu.

```javascript
function a() {
  console.log("a mulai");
  b();
  console.log("a selesai");
}

function b() {
  console.log("b mulai");
  c();
  console.log("b selesai");
}

function c() {
  console.log("c mulai & selesai");
}

a();
// Call Stack saat puncak: a → b → c
// Output:
// a mulai
// b mulai
// c mulai & selesai
// b selesai
// a selesai
```

---

## Fungsi

Runtime bertanggung jawab: **parsing** kode → **kompilasi** (JIT) → **eksekusi** via Call Stack + **alokasi memori** via Heap. Semua fungsi bawaan (Array, Object, Promise) disediakan oleh engine.

---

## Cara Pengimplementasian

### 1. Call Stack — Stack Overflow

```javascript
function stackOverflow() {
  return stackOverflow(); // rekursi tak terbatas
}
// ❌ RangeError: Maximum call stack size exceeded
// Ibarat tumpukan material terlalu tinggi sampai ambruk
```

### 2. Trace Manual Call Stack

```javascript
function pesanBata(ukuran, callback) {
  console.log(`Pesan ${ukuran} bata — masuk Call Stack`);
  callback(ukuran);
}

function kirimMaterial(jumlah) {
  console.log(`Kirim ${jumlah} material — masuk Call Stack`);
  console.log("Call Stack sekarang: pesanBata → kirimMaterial");
}

pesanBata(500, kirimMaterial);
```

### 3. Memory Heap

```javascript
// Semua object tinggal di Heap
const rumah = { kamar: 3, luas: 100 }; // Heap
const bahan = ["bata", "semen", "pasir"]; // Heap
let x = 5; // primitive → Stack (nilai kecil)
```

```
Call Stack:                      Memory Heap:
┌─────────────┐                  ┌──────────────────────┐
│ main()      │                  │ {kamar:3, luas:100}  │
│ pesanBata() │                  │ ["bata","semen",...] │
│ kirimMtrl() │                  │ closure x()          │
└─────────────┘                  └──────────────────────┘
```

---

## Analogi: Membangun Rumah — Meja Kerja & Gudang

| JS Runtime | Proyek Rumah |
|---|---|
| Call Stack | **Meja kerja tukang** — hanya bisa kerjakan 1 tugas sekali |
| Memory Heap | **Gudang material** — bata, semen, kayu disimpan sampai dipakai |
| V8 Engine | **Mandor proyek** — baca cetak biru, instruksikan tukang |
| Stack Overflow | Meja ambruk karena tumpukan kerja terlalu tinggi |
| Garbage Collector | **Tukang bersih-bersih** — buang sampah material tidak terpakai |
| Single-threaded | Hanya **satu tukang** yang bekerja di meja dalam satu waktu |

Bayangkan **satu tukang** di proyek rumah. Ia hanya punya satu meja kerja (Call Stack). Ia ambil bata dari gudang (Heap), pasang, selesai, ambil lagi. Kalau tugas menumpuk (rekursi), meja ambruk. Material tidak terpakai dibuang tukang bersih-bersih (Garbage Collector). Mandor (V8) yang atur semua.

---

## Dipakai Untuk Apa

- **Debugging stack trace** — `console.trace()` lihat urutan panggilan fungsi
- **Optimasi memori** — hindari alokasi Heap berlebihan (memory leak)
- **Memahami error** — `Maximum call stack size` = stack overflow
- **Single-threaded concurrency model** — dasar Event Loop dan async

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Rekursi tanpa base case | `function f() { f(); }` | Stack overflow, aplikasi crash |
| Buat objek raksasa terus-menerus | Loop `new Array(1e7)` | Heap penuh, memory leak |
| Lupa JS single-threaded | Blokir Call Stack dengan loop berat | UI freeze, semua antrian tertahan |
| Anggap Heap tak terbatas | Simpan data terus tanpa GC | Performa turun drastis |

---

## Hubungan dengan Materi Sebelumnya

Ini **fondasi** sebelum masuk async:
- Materi 31 (Stack) → Call Stack adalah implementasi Stack LIFO
- Materi 47 (Event Loop) → Call Stack dikosongkan dulu, baru jalankan antrian
- Materi 48 (Callback) → Callback dipanggil, masuk Call Stack
- Materi 49 (Promise) → Promise.then() masuk Microtask, tunggu Call Stack kosong

---

## Soal Latihan

### Soal 1 (Mudah)
Tebak urutan output dan trace Call Stack maksimal:
```javascript
function mulai() {
  console.log("mulai");
  tengah();
  console.log("kembali ke mulai");
}
function tengah() {
  console.log("tengah");
}
mulai();
```

**Jawaban**:
```
mulai
tengah
kembali ke mulai
```
Call Stack maksimal: `mulai → tengah` (2 frame). Setelah `tengah` selesai, stack pop, lanjut ke `kembali ke mulai`.

### Soal 2 (Sedang)
Apa output dari kode ini dan apa isi Call Stack di setiap langkah?
```javascript
function satu() { dua(); }
function dua() { tiga(); }
function tiga() { console.log("tiga"); }
satu();
console.log("selesai");
```

**Jawaban**:
```
tiga
selesai
```
Call Stack:
1. `main` → push `satu` → push `dua` → push `tiga`
2. `tiga` selesai (pop), `dua` selesai (pop), `satu` selesai (pop)
3. `console.log("selesai")` push lalu pop
Stack maksimal 4 frame (`main, satu, dua, tiga`).

### Soal 3 (Tantangan)
Mengapa kode berikut menyebabkan stack overflow? Di memori mana objek `fn` disimpan?
```javascript
const fn = () => fn();
fn();
```

**Jawaban**:
- Stack overflow karena fungsi panggil dirinya sendiri tanpa henti — setiap panggilan menambah frame ke Call Stack hingga batas maksimal.
- Objek `fn` (arrow function) disimpan di **Memory Heap** karena function adalah object. Referensi `fn` di Stack menunjuk ke alokasi Heap. Tiap rekursi membuat **frame baru** di Stack, bukan objek baru di Heap.
