# 32. Fungsi Dasar — Deklarasi, Ekspresi, Arrow Function

**Benang Merah**: Selama ini kita menulis kode **baris per baris** tanpa struktur. Fungsi adalah **loncatan besar** — kita belajar membungkus logika dalam unit yang bisa dipakai ulang.

---

## Penjelasan

Fungsi adalah **blok kode** yang diberi nama, bisa menerima input (parameter), dan bisa mengembalikan output (return value). Ibarat **mesin di pabrik**: Anda masukkan bahan baku, mesin memprosesnya, keluar hasil jadi.

```javascript
// Tanpa fungsi — kode berulang
let a1 = 5 * 2;
console.log(a1);

let a2 = 8 * 2;
console.log(a2);

// Dengan fungsi — sekali tulis, berkali-kali pakai
function kaliDua(angka) {
  return angka * 2;
}

console.log(kaliDua(5)); // 10
console.log(kaliDua(8)); // 16
```

Ada 3 cara membuat fungsi di JavaScript:

### 1. Function Declaration
```javascript
function sapa(nama) {
  return "Halo, " + nama + "!";
}
```

### 2. Function Expression
```javascript
const sapa = function(nama) {
  return "Halo, " + nama + "!";
};
```

### 3. Arrow Function (ES6+)
```javascript
const sapa = (nama) => "Halo, " + nama + "!";
```

---

## Fungsi

Membungkus logika ke dalam **unit yang reusable**, membuat kode:
- **Tidak berulang** (tulis sekali, pakai berkali-kali)
- **Terorganisir** (setiap fungsi punya satu tugas)
- **Mudah dites** (test satu fungsi saja)

---

## Cara Pengimplementasian

### Function Declaration — paling standar
```javascript
function luasPersegiPanjang(panjang, lebar) {
  return panjang * lebar;
}

console.log(luasPersegiPanjang(10, 5)); // 50
```

### Arrow Function — ringkas, modern
```javascript
const luasPersegiPanjang = (panjang, lebar) => panjang * lebar;
```

### Default Parameter
```javascript
function sapa(nama = "Tamu") {
  return "Halo, " + nama + "!";
}

console.log(sapa("Budi")); // "Halo, Budi!"
console.log(sapa());       // "Halo, Tamu!"
```

### Fungsi yang Tidak Mengembalikan Nilai (void)
```javascript
function cetakPesan(pesan) {
  console.log("PESAN: " + pesan);
  // tidak ada return — hasilnya undefined
}

let hasil = cetakPesan("Halo"); 
console.log(hasil); // undefined
```

---

## Analogi: Membangun Rumah (Mesin Pabrik)

| Fungsi | Mesin di Pabrik Material |
|---|---|
| Parameter | Bahan baku masuk |
| Body fungsi | Proses produksi |
| Return value | Hasil jadi keluar |
| Function declaration | Mesin permanen (bisa dipanggil dari mana saja) |
| Function expression | Mesin rakitan (hanya ada setelah dirakit) |
| Arrow function | Mesin modern, ringkas |

Bayangkan Anda punya **mesin pemotong besi** di pabrik. Anda tinggal memasukkan besi mentah (parameter), mesin memotong (proses), dan mengeluarkan besi yang sudah jadi (return). Tanpa mesin, Anda potong manual setiap kali. Dengan mesin, Anda tinggal tekan tombol.

---

## Dipakai Untuk Apa

- **Setiap** program JavaScript yang terstruktur
- Mengelompokkan logika bisnis (hitung diskon, validasi email, format tanggal)
- Event handler (klik tombol, submit form)
- Callback (nanti di Level 3)

---

## Kesalahan Umum

| Kesalahan | Contoh | Perbaikan |
|---|---|---|
| Lupa `return` | `function a(){ let x=2+2; }` | Tambah `return x;` |
| Panggil fungsi tanpa `()` | `console.log(kaliDua)` | `console.log(kaliDua(5))` |
| Argumen terbalik | `bagi(2, 10)` padahal bagi(a,b) | `bagi(10, 2)` |
| Nested function terlalu dalam | Fungsi di dalam fungsi di dalam fungsi | Refactor |
| Arrow function tanpa `{}` untuk multi-baris | `const a = () => baris1 baris2` | Pakai `{}` + `return` |

---

## Hubungan dengan Materi Sebelumnya

Ini adalah **titik balik** dalam roadmap:
- Level 1: variabel + operator + if/else + loop → **baris per baris**
- Level 2: fungsi → **logika terstruktur dan reusable**

Setelah fungsi, kita bisa:
- Memahami **scope** variabel di dalam fungsi (→ Materi 33)
- Membuat **closure** dengan fungsi di dalam fungsi (→ Materi 35)
- Fungsi sebagai nilai → **higher-order function** (→ Materi 37)
- Fungsi sebagai method → **OOP** (→ Materi 41-45)

---

## Soal Latihan

### Soal 1 (Mudah)
Buat fungsi `kaliLima` yang menerima satu angka dan mengembalikan angka tersebut dikali 5.

**Jawaban**:
```javascript
// Function declaration
function kaliLima(angka) {
  return angka * 5;
}

// Arrow function
const kaliLima = (angka) => angka * 5;

console.log(kaliLima(3)); // 15
console.log(kaliLima(7)); // 35
```

### Soal 2 (Sedang)
Buat fungsi `cekGanjilGenap` yang menerima angka dan mengembalikan string "Ganjil" atau "Genap". Gunakan fungsi dari materi percabangan (Level 1).

**Jawaban**:
```javascript
function cekGanjilGenap(angka) {
  if (angka % 2 === 0) {
    return "Genap";
  } else {
    return "Ganjil";
  }
}

console.log(cekGanjilGenap(4)); // Genap
console.log(cekGanjilGenap(7)); // Ganjil

// Versi arrow + ternary
const cekGanjilGenap = (angka) => angka % 2 === 0 ? "Genap" : "Ganjil";
```

### Soal 3 (Tantangan)
Buat fungsi `hitungDiskon(harga, persen)` yang mengembalikan harga setelah diskon. Jika parameter `persen` tidak diberikan, gunakan default 10%. Buat dalam 3 versi (declaration, expression, arrow).

**Jawaban**:
```javascript
// Versi 1: Function Declaration
function hitungDiskon(harga, persen = 10) {
  return harga - (harga * persen / 100);
}

// Versi 2: Function Expression
const hitungDiskon = function(harga, persen = 10) {
  return harga - (harga * persen / 100);
};

// Versi 3: Arrow Function
const hitungDiskon = (harga, persen = 10) => harga - (harga * persen / 100);

console.log(hitungDiskon(100000, 20)); // 80000 (diskon 20%)
console.log(hitungDiskon(50000));      // 45000 (diskon 10% default)
```
