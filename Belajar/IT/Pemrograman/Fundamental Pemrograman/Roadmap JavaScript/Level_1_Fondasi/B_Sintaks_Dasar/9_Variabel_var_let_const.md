# 9. Variabel — var, let, const & Perbedaan Mendasarnya

**Benang Merah**: Sebelumnya (Materi 1-8) kita belajar **CARA BERPIKIR**. Sekarang kita mulai **MENULIS KODE**. Variabel adalah wadah data — unit paling dasar dalam program.

---

## Penjelasan

Variabel adalah **tempat penyimpanan** di memori komputer yang punya nama dan bisa diisi data. Ibarat **loker yang diberi label**: Anda bisa menyimpan barang, mengambilnya, atau menggantinya dengan barang lain.

Di JavaScript ada 3 cara membuat variabel:
- `var` — cara lama (sejak 1995), punya keanehan
- `let` — cara modern (ES6, 2015), lebih aman
- `const` — untuk data yang tidak akan diubah

```javascript
var namaLama = "Budi";     // cara lama, hindari
let namaBaru = "Budi";     // bisa diubah
const NAMA_TETAP = "Budi"; // TIDAK bisa diubah
```

---

## Fungsi

Menyimpan data di memori dengan **nama yang mudah diingat** agar bisa dipakai ulang. Tanpa variabel, Anda harus mengetik nilai yang sama berulang kali.

---

## Cara Pengimplementasian

### `let` — untuk nilai yang berubah
```javascript
let umur = 17;
console.log(umur); // 17

umur = 18; // diubah
console.log(umur); // 18
```

### `const` — untuk nilai tetap (rekomendasi default)
```javascript
const TAHUN_LAHIR = 2005;
console.log(TAHUN_LAHIR); // 2005

TAHUN_LAHIR = 2006; // ERROR! const tidak bisa diubah
```

### `var` — cara lama (kenali untuk baca kode lawas)
```javascript
var nama = "Budi";
console.log(nama);
```

### Aturan Penamaan
```javascript
// ✅ Benar
let nama_lengkap;       // snake_case — jarang di JS
let namaLengkap;        // camelCase — STANDAR JS
let _private;           // underscore di awal
let $element;           // dollar sign (jQuery legacy)

// ❌ Salah
let 1nama;              // tidak boleh diawali angka
let nama-lengkap;       // tidak boleh pakai strip
let let;                // reserved word!
let class;              // reserved word!
```

---

## Analogi: Membangun Rumah (Loker Material)

| Variabel | Loker di Gudang Material |
|---|---|
| `let` | Loker biasa — bisa ganti isi kapan saja |
| `const` | Loker khusus dengan segel — isi tetap |
| Nama variabel | Label di pintu loker |
| Nilai variabel | Isi di dalam loker |
| `var` | Loker bekas yang kuncinya rusak (tidak rapi) |

Bayangkan Anda punya gudang material bangunan. Untuk **semen** Anda pasang label "semen" di satu loker — itu `let` karena isinya bisa habis dan diisi ulang. Tapi **pondasi rumah** sudah dicor dan tidak berubah — itu `const`. `var` seperti loker yang pintunya bisa terbuka sendiri karena kuncinya tidak berfungsi baik — kadang isinya hilang atau tercampur.

---

## Dipakai Untuk Apa

- **Setiap program** JavaScript pasti menggunakan variabel
- Menyimpan input pengguna
- Menampung hasil kalkulasi
- Referensi ke elemen di halaman web
- Hampir semua operasi data

---

## Kesalahan Umum

| Kesalahan | Contoh |
|---|--|
| Pakai `const` untuk nilai berubah | `const umur = 17; umur = 18;` ❌ |
| Pakai `var` di kode modern | `var x = 1;` — lebih baik `let` |
| Nama variabel tidak deskriptif | `let a = "Budi"` — siapa Budi? |
| Lupa deklarasi | `umur = 17` — tanpa `let`/`const` jadi **global** (berbahaya) |
| Typo nama variabel | `let namaLengkap = "Budi"; console.log(namaLengkap);` |

---

## Hubungan dengan Materi Sebelumnya

Materi 1-8 mengajarkan **cara berpikir**. Sekarang dengan variabel, Anda punya **alat pertama** untuk mewujudkan pikiran itu dalam kode. Setelah ini:
- Variabel akan diisi **tipe data** (→ Materi 10)
- Variabel akan dimanipulasi dengan **operator** (→ Materi 12)
- Variabel akan digunakan di **percabangan & loop** (→ Materi 15-16)

---

## Soal Latihan

### Soal 1 (Mudah)
Buat 3 variabel: `nama` (nama Anda), `usia` (usia Anda), `isPelajar` (true). Cetak semuanya.

**Jawaban**:
```javascript
const nama = "Budi Santoso";
let usia = 19;
const isPelajar = true;

console.log(nama);     // Budi Santoso
console.log(usia);     // 19
console.log(isPelajar); // true
```

### Soal 2 (Sedang)
Buat program kalkulasi sederhana: hitung luas persegi panjang (panjang = 10, lebar = 5). Simpan hasilnya di variabel `luas`.

**Jawaban**:
```javascript
const panjang = 10;
const lebar = 5;
const luas = panjang * lebar;

console.log("Luas:", luas); // Luas: 50
```

### Soal 3 (Tantangan)
Coba pahami dan perbaiki kode berikut (ada 3 error):
```javascript
const nama = "Budi"
let umur = 20
var tinggal = "Jakarta"

umur = "dua puluh"  // baris A
nama = "Siti"       // baris B
let tinggal = "Bandung" // baris C
```

**Jawaban**:
- Baris A: tidak error secara sintaks, tapi **inkonsisten tipe** (angka jadi string)
- Baris B: **ERROR** — `const` tidak bisa diubah
- Baris C: **ERROR** — variabel `tinggal` sudah dideklarasi dengan `var`

Perbaikan:
```javascript
const nama = "Budi";
let umur = 20;
let tinggal = "Jakarta"; // ganti var jadi let

umur = 21;               // tetap angka
// nama = "Siti";        // dihapus atau ganti const jadi let
tinggal = "Bandung";     // pakai variabel yang sudah ada
```
