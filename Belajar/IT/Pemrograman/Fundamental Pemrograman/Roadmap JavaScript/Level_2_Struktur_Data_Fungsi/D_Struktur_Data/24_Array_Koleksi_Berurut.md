# 24. Array — Koleksi Data Berurut

**Benang Merah**: Di Level 1 kita hanya menyimpan SATU nilai per variabel (`let nama = "Budi"`). Sekarang kita belajar menyimpan **BANYAK nilai** sekaligus dalam satu wadah — Array.

---

## Penjelasan

Array adalah **koleksi data berurut** yang bisa menampung banyak nilai dalam satu variabel. Setiap posisi punya **index** (nomor urut) mulai dari 0.

```javascript
// Variabel tunggal (cara Level 1)
let murid1 = "Budi";
let murid2 = "Siti";
let murid3 = "Agus";

// Array (yang kita pelajari sekarang)
let murid = ["Budi", "Siti", "Agus"];
```

Akses data melalui index:
```javascript
console.log(murid[0]); // "Budi"  — index 0
console.log(murid[1]); // "Siti"  — index 1
console.log(murid[2]); // "Agus"  — index 2
console.log(murid[3]); // undefined — index 3 tidak ada
```

---

## Fungsi

Menyimpan, mengatur, dan memproses **kumpulan data** yang berhubungan dalam satu tempat. Tanpa array, Anda harus membuat variabel terpisah untuk setiap data — tidak praktis jika data ratusan.

---

## Cara Pengimplementasian

### Membuat Array
```javascript
let kosong = [];           // array kosong
let angka = [1, 2, 3, 4, 5];
let campur = ["Budi", 20, true]; // boleh beda tipe (tapi kurang baik)
let nested = [[1, 2], [3, 4]];   // array di dalam array
```

### CRUD pada Array
```javascript
let antrian = ["Budi", "Siti"];

// CREATE — tambah data
antrian.push("Agus");     // tambah di akhir  → ["Budi", "Siti", "Agus"]
antrian.unshift("Dewi");  // tambah di awal   → ["Dewi", "Budi", "Siti", "Agus"]

// READ — akses data
console.log(antrian[1]);  // "Budi"
console.log(antrian.length); // 4 — jumlah elemen

// UPDATE — ubah data
antrian[2] = "Siti Updated"; // ganti index 2

// DELETE — hapus data
antrian.pop();            // hapus dari akhir  → hapus "Agus"
antrian.shift();          // hapus dari awal   → hapus "Dewi"
antrian.splice(1, 1);     // hapus 1 elemen dari index 1
```

### Iterasi Array
```javascript
let buah = ["Apel", "Mangga", "Jeruk"];

// Cara 1: for loop (sudah dipelajari di Level 1)
for (let i = 0; i < buah.length; i++) {
  console.log(buah[i]);
}

// Cara 2: for...of (lebih ringkas)
for (let item of buah) {
  console.log(item);
}
```

---

## Analogi: Membangun Rumah (Rak Material)

| Array | Rak Material di Gudang |
|---|---|
| Index (0, 1, 2...) | Nomor rak (rak 1, rak 2, rak 3) |
| `array[2]` | Ambil barang dari rak nomor 3 |
| `.push()` | Taruh kotak baru di ujung rak |
| `.pop()` | Ambil kotak paling ujung |
| `.length` | Jumlah total kotak di rak |
| Array multidimensi | Rak bersusun (baris dan kolom) |

Bayangkan Anda punya **rak material** yang rapi. Setiap sekat punya nomor mulai dari 0. Anda bisa:
- **Tambah** material baru di ujung (push) atau di depan (unshift)
- **Ambil** material dari nomor rak tertentu
- **Ganti** material di rak tertentu
- **Hitung** berapa banyak material (length)

---

## Dipakai Untuk Apa

- **Antrian / daftar** — todo list, keranjang belanja, antrian customer
- **Data terstruktur** — daftar nama, daftar harga, daftar produk
- **Matrix / grid** — papan catur, pixel gambar, tabel data
- **Stack & Queue** — undo/redo, antrian proses (dibahas di Materi 31)

---

## Kesalahan Umum

| Kesalahan | Contoh | Penjelasan |
|---|---|---|
| Index mulai 1 | `arr[1]` untuk data pertama | Index mulai dari **0** |
| Lupa `length` | `for(let i=0; i<=arr.length; i++)` | Kelebihan 1, jadi `undefined` |
| `const` array dianggap tetap | `const arr = [1]; arr.push(2)` | **Ini valid!** `const` hanya melindungi referensi, bukan isi |
| Typo method | `arr.pus()` vs `arr.push()` | Method harus tepat |
| Campur tipe berlebihan | `["Budi", 20, true, null, {}]` | Sulit diprediksi, hindari |

---

## Hubungan dengan Materi Sebelumnya

Di Level 1 (Materi 16) kita belajar `for` loop. Sekarang kita gabungkan loop dengan array untuk **memproses banyak data**. Ini adalah pola yang akan dipakai terus:
- Array + loop → dikembangkan ke Array Methods (→ Materi 25)
- Array + Object → menjadi Array of Objects (→ Materi 28) — pola paling umum di dunia nyata
- Array + Stack/Queue → struktur data LIFO/FIFO (→ Materi 31)

---

## Soal Latihan

### Soal 1 (Mudah)
Buat array berisi 5 nama teman. Cetak nama pertama dan terakhir, lalu jumlah total teman.

**Jawaban**:
```javascript
let teman = ["Budi", "Siti", "Agus", "Dewi", "Rudi"];
console.log("Pertama:", teman[0]);          // Budi
console.log("Terakhir:", teman[teman.length - 1]); // Rudi
console.log("Jumlah:", teman.length);       // 5
```

### Soal 2 (Sedang)
Buat program simulasi antrian: mulailah dengan antrian `["Budi", "Siti"]`. Kemudian:
- Tambah "Agus" di belakang
- Tambah "Dewi" di depan
- Layani orang pertama (hapus dari depan)
- Cetak antrian akhir

**Jawaban**:
```javascript
let antrian = ["Budi", "Siti"];

antrian.push("Agus");        // ["Budi", "Siti", "Agus"]
antrian.unshift("Dewi");     // ["Dewi", "Budi", "Siti", "Agus"]
antrian.shift();             // ["Budi", "Siti", "Agus"]

console.log("Antrian akhir:", antrian);
// Output: ["Budi", "Siti", "Agus"]
```

### Soal 3 (Tantangan)
Buat array multidimensi (matriks 3x3) berisi angka 1-9. Cetak semua isinya dalam format:

```
1 2 3
4 5 6
7 8 9
```

**Jawaban**:
```javascript
let matriks = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

for (let i = 0; i < matriks.length; i++) {
  let baris = "";
  for (let j = 0; j < matriks[i].length; j++) {
    baris += matriks[i][j] + " ";
  }
  console.log(baris.trim());
}
```
