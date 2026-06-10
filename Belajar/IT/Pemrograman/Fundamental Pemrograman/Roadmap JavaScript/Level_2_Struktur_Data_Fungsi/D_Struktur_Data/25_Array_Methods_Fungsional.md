# 25. Array Methods Fungsional — map, filter, reduce, find

**Benang Merah**: Di Materi 24 kita memanipulasi array secara manual (push, pop, for loop). Sekarang kita belajar metode **fungsional** — lebih deklaratif, ringkas, dan sulit salah.

---

## Penjelasan

**Paradigma imperatif** = kita bilang "bagaimana" (for loop, index). **Paradigma fungsional** = kita bilang "apa" (map, filter, reduce). Metode fungsional tidak mengubah array asli, melainkan mengembalikan array baru.

```javascript
// Imperatif (Materi 24)
let hasil = [];
for (let i = 0; i < angka.length; i++) {
  hasil.push(angka[i] * 2);
}

// Fungsional (sekarang)
let hasil = angka.map(x => x * 2);
```

Method utama:
- `.map(fn)` → transformasi setiap elemen
- `.filter(fn)` → menyaring elemen berdasarkan kondisi
- `.reduce(fn, awal)` → mereduksi array jadi satu nilai
- `.find(fn)` → cari elemen pertama yang cocok
- `.findIndex(fn)` → cari index elemen pertama yang cocok
- `.some(fn)` → apakah ada yang memenuhi? (true/false)
- `.every(fn)` → apakah SEMUA memenuhi? (true/false)
- **Chaining** — menggabung beberapa method berantai

---

## Fungsi

Memproses data array secara **deklaratif, ringkas, dan tanpa efek samping** (tidak mengubah data asli). Pola ini adalah fondasi paradigma functional programming di JavaScript.

---

## Code — Olah Data Array Produk

```javascript
const produk = [
  { nama: "Bata Merah", harga: 500, tersedia: true },
  { nama: "Semen", harga: 1200, tersedia: false },
  { nama: "Cat Tembok", harga: 2500, tersedia: true },
  { nama: "Paku", harga: 200, tersedia: true },
];

// FILTER — ambil yang tersedia saja
const tersedia = produk.filter(p => p.tersedia);
console.log(tersedia);

// MAP — format display (nama + harga)
const display = tersedia.map(p => `${p.nama}: Rp${p.harga}`);
console.log(display);
// ["Bata Merah: Rp500", "Cat Tembok: Rp2500", "Paku: Rp200"]

// REDUCE — hitung total harga
const total = tersedia.reduce((akum, p) => akum + p.harga, 0);
console.log("Total:", total); // 3200

// FIND — cari produk pertama dengan harga > 1000
const mahal = produk.find(p => p.harga > 1000);
console.log(mahal.nama); // "Semen"

// SOME & EVERY
console.log(produk.some(p => p.harga > 2000)); // true
console.log(produk.every(p => p.tersedia));     // false

// CHAINING — filter → map → reduce dalam satu baris
const totalTersedia = produk
  .filter(p => p.tersedia)
  .map(p => p.harga)
  .reduce((a, b) => a + b, 0);
console.log(totalTersedia); // 3200
```

---

## Analogi: Membangun Rumah (Konveyor Pabrik)

| Method | Stasiun di Konveyor Pabrik |
|---|---|
| `.filter()` | Penyortir — hanya barang yang lolos质检 yang lanjut |
| `.map()` | Mesin transformasi — setiap bata diberi label harga |
| `.reduce()` | Pengepakan akhir — semua barang dihitung totalnya |
| `.find()` | Detektor — cari satu barang pertama yang cocok |
| `.some()` | Lampu indikator — "apakah ada barang cacat?" (ya/tidak) |
| `.every()` | QC inspeksi — "apakah SEMUA barang lolos?" (ya/tidak) |
| Chaining | Konveyor berantai — barang melewati stasiun 1 → 2 → 3 |

Bayangkan **pabrik rumah** dengan ban berjalan (konveyor). Bata mentah masuk ke konveyor:
1. Stasiun **filter** — hanya bata utuh yang lolos
2. Stasiun **map** — setiap bata dicat dan diberi stiker harga
3. Stasiun **reduce** — semua bata ditimbang total beratnya

Setiap stasiun tidak mengubah bata asli — ia membuat bata baru yang sudah diproses.

---

## Dipakai Untuk Apa

- Olah data dari **API / database** — filter, transform, agregasi
- **Dashboard analytics** — hitung total, rata-rata, maksimum
- **Search / filter** — cari produk, user, transaksi
- **Format ulang data** — ubah struktur data untuk ditampilkan di UI
- **Validasi kolektif** — apakah semua item memenuhi syarat? (every)

---

## Kesalahan Umum

| Kesalahan | Contoh | Penjelasan |
|---|---|---|
| Lupa return di arrow function | `arr.map(x => { x * 2 })` | Kurung kurawal butuh `return` eksplisit |
| Mengubah array asli | `arr.map(x => arr[x] = x*2)` | Map tidak untuk mutasi — buat array baru |
| `find` vs `filter` | `arr.find(x => x > 5)` hanya dapat 1 | `find` berhenti setelah 1 temuan |
| Lupa nilai awal `reduce` | `arr.reduce((a,b) => a+b)` | Error jika array kosong — beri nilai awal `0` |
| Chaining berlebihan | 10 method berantai tanpa variabel | Sulit di-debug — simpan tiap langkah | 
| `some` vs `includes` | Salah paham `some` untuk cek nilai | `some` pakai fungsi, `includes` pakai nilai langsung |

---

## Hubungan dengan Materi Sebelumnya

- **Materi 24 (Array dasar)**: Sekarang kita tidak lagi manual for loop — method fungsional mengotomatisasi pola yang sama.
- **Materi 16 (Function)**: Arrow function `=>` dipakai di sini sebagai callback.
- **Materi 26 (String lanjutan)**: String juga punya methods canggih — pola yang sama.

---

## Soal Latihan

### Soal 1 (Mudah)
Dari array `[3, 7, 1, 9, 4]`, gunakan `.filter()` untuk ambil angka genap, lalu `.map()` untuk mengalikannya dengan 10.

**Jawaban**:
```javascript
const angka = [3, 7, 1, 9, 4];
const hasil = angka.filter(x => x % 2 === 0).map(x => x * 10);
console.log(hasil); // [40]
```

### Soal 2 (Sedang)
Dari array produk berikut, hitung **total harga stok** (harga * stok) menggunakan `.reduce()`:
```javascript
const produk = [
  { nama: "Bata", harga: 500, stok: 10 },
  { nama: "Semen", harga: 1200, stok: 5 },
  { nama: "Cat", harga: 2500, stok: 3 },
];
```

**Jawaban**:
```javascript
const totalNilai = produk.reduce((total, p) => total + (p.harga * p.stok), 0);
console.log("Total nilai stok:", totalNilai);
// 500*10 + 1200*5 + 2500*3 = 5000 + 6000 + 7500 = 18500
```

### Soal 3 (Tantangan)
Dari array angka `[2, 5, 8, 3, 6, 9]`, gunakan **chaining** untuk: filter angka genap → kalikan 3 → cari angka pertama yang hasilnya > 15.

**Jawaban**:
```javascript
const angka = [2, 5, 8, 3, 6, 9];
const hasil = angka
  .filter(x => x % 2 === 0)   // [2, 8, 6]
  .map(x => x * 3)             // [6, 24, 18]
  .find(x => x > 15);          // 24
console.log(hasil); // 24
```
