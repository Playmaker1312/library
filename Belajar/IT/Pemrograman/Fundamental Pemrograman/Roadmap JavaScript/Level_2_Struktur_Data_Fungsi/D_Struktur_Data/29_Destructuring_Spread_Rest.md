# 29. Destructuring, Spread & Rest

**Benang Merah**: Di Materi 28 kita sering mengakses `todo.id`, `todo.tugas` — berulang-ulang. Destructuring hadir untuk **memecah** struktur data menjadi variabel terpisah secara cepat.

---

## Penjelasan

**Destructuring** = "membongkar" array/object langsung ke variabel. **Spread** (`...`) = "menyebarkan" isi array/object. **Rest** (`...`) = "mengumpulkan" sisa parameter.

```javascript
// Sebelum destructuring (Materi 28)
let todo = { id: 1, tugas: "Belajar", selesai: false };
let id = todo.id;
let tugas = todo.tugas;

// Sesudah destructuring
let { id, tugas } = todo;
```

---

## Fungsi

Mempercepat ekstraksi data dari array/object, menggabungkan data, dan menangani parameter fungsi dinamis — seperti membongkar koper (destructure) atau menggabungkan isi koper (spread).

---

## Code — Refactor Menggunakan Destructuring & Spread

```javascript
// ===== ARRAY DESTRUCTURING =====
const angka = [10, 20, 30, 40, 50];
const [a, b, ...sisa] = angka;
console.log(a);        // 10
console.log(b);        // 20
console.log(sisa);     // [30, 40, 50]

// Skip elemen dengan koma kosong
const [pertama, , ketiga] = angka;
console.log(pertama, ketiga); // 10 30

// Default value
const [x, y, z = 99] = [1, 2];
console.log(z); // 99

// ===== OBJECT DESTRUCTURING =====
const profil = { nama: "Budi", umur: 30, kota: "Jakarta" };
const { nama, umur } = profil;
console.log(nama, umur); // "Budi" 30

// Rename properti
const { nama: fullName, kota: city } = profil;
console.log(fullName); // "Budi"

// Nested destructuring
const user = {
  id: 1,
  alamat: { jalan: "Jl. A", kodePos: "12345" },
};
const { alamat: { jalan, kodePos } } = user;
console.log(jalan); // "Jl. A"

// ===== SPREAD OPERATOR =====
// Spread array — gabung
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const gabung = [...arr1, ...arr2];
console.log(gabung); // [1, 2, 3, 4, 5, 6]

// Spread array — salin (bukan referensi!)
const salinanArr = [...arr1];
salinanArr[0] = 99;
console.log(arr1[0]);    // 1 (tidak berubah)

// Spread object — gabung
const dasar = { nama: "Budi", umur: 30 };
const detail = { kota: "Jakarta", hobi: "Coding" };
const lengkap = { ...dasar, ...detail };
console.log(lengkap);
// { nama: "Budi", umur: 30, kota: "Jakarta", hobi: "Coding" }

// Override properti
const update = { ...dasar, umur: 31 };
console.log(update.umur); // 31

// ===== REST PARAMETER =====
function jumlahkan(...angka) {
  return angka.reduce((a, b) => a + b, 0);
}
console.log(jumlahkan(1, 2, 3, 4, 5)); // 15

// Rest dengan destructuring
const [first, second, ...rest] = [10, 20, 30, 40, 50];
console.log(rest); // [30, 40, 50]
```

---

## Analogi: Membangun Rumah (Membongkar & Menggabung Koper)

| Konsep | Koper di Proyek Rumah |
|---|---|
| `const {nama} = obj` | Buka koper, ambil satu alat (palu) |
| `const {a, ...sisa} = obj` | Ambil palu + meteran, sisanya tetap di koper |
| `const [a, b] = arr` | Dua alat pertama dari rak |
| `[...arr1, ...arr2]` | Gabung isi dua koper ke satu koper besar |
| `{...obj1, ...obj2}` | Gabung dua lemari arsip jadi satu |
| Rest `...args` | Terima semua alat yang dilempar ke Anda |
| Default `= "X"` | Jika koper kosong, pakai alat cadangan |

Bayangkan Anda di gudang proyek. **Destructuring** = membongkar koper dan mengambil alat tertentu langsung ke tangan. **Spread** = menggabungkan isi beberapa koper ke satu koper besar. **Rest** = ketika seseorang melempar Anda alat satu per satu, Anda tangkap semuanya dalam satu kotak.

---

## Dipakai Untuk Apa

- **Extract data dari API response** — ambil properti yang dibutuhkan
- **Refactor kode lama** — ganti akses manual `data.x` jadi destructuring
- **Parameter fungsi fleksibel** — rest parameter untuk fungsi dengan banyak argumen
- **Immutability** — spread untuk "salin" data sebelum diubah
- **Merge konfigurasi** — gabung default config dengan config user

---

## Kesalahan Umum

| Kesalahan | Contoh | Penjelasan |
|---|---|---|
| Destructuring `undefined` | `const {x} = undefined` | Error — pastikan object tidak null/undefined |
| Salah urutan di array | `const [a, b] = [1, 2, 3]` | `a=1, b=2` — ignore `3` (bukan error) |
| Lupa `{}` untuk object | `const nama = {nama: "Budi"}` | Salah syntax — harus `const {nama} = obj` |
| Spread di non-iterable | `const a = {...null}` | Object spread null → `{}` (valid) |
| Rest bukan di parameter terakhir | `function fn(a, ...rest, b)` | Error — rest harus di akhir |

---

## Hubungan dengan Materi Sebelumnya

- **Materi 28 (Array of objects)**: Destructuring sangat berguna saat memproses setiap item di `.map()` — `todos.map(({id, tugas}) => ...)`.
- **Materi 27 (Object)**: Object destructuring adalah "kebalikan" dari object literal.
- **Materi 25 (Array methods)**: Spread dipakai untuk salin array sebelum sort (karena sort in-place).
- **Materi 30 (Map & Set)**: Spread juga bisa digunakan untuk konversi Map/Set ke array.

---

## Soal Latihan

### Soal 1 (Mudah)
Dari array `[100, 200, 300, 400, 500]`, gunakan destructuring untuk ambil index 0, 2, dan sisanya.

**Jawaban**:
```javascript
const arr = [100, 200, 300, 400, 500];
const [pertama, , ketiga, ...sisa] = arr;
console.log(pertama); // 100
console.log(ketiga);  // 300
console.log(sisa);    // [400, 500]
```

### Soal 2 (Sedang)
Dari object `{judul:"JS", penulis:"Budi", tahun:2024, harga:150}` ambil `judul` dan `penulis`, lalu rename `tahun` menjadi `tahunTerbit`.

**Jawaban**:
```javascript
const buku = { judul: "JS", penulis: "Budi", tahun: 2024, harga: 150 };
const { judul, penulis, tahun: tahunTerbit } = buku;
console.log(judul);        // "JS"
console.log(penulis);      // "Budi"
console.log(tahunTerbit);  // 2024
```

### Soal 3 (Tantangan)
Buat fungsi `gabungProfil(...)` yang menerima beberapa object profil dan menggabungkannya dengan spread. Jika ada key yang sama, key dari object terakhir yang menang. Fungsi juga harus punya default `{status: "aktif"}`.

**Jawaban**:
```javascript
function gabungProfil(...profils) {
  return profils.reduce((acc, p) => ({ ...acc, ...p }), { status: "aktif" });
}

const dasar = { nama: "Budi", role: "user" };
const alamat = { kota: "Jakarta" };
const update = { role: "admin" };

const hasil = gabungProfil(dasar, alamat, update);
console.log(hasil);
// { status: "aktif", nama: "Budi", role: "admin", kota: "Jakarta" }
```
