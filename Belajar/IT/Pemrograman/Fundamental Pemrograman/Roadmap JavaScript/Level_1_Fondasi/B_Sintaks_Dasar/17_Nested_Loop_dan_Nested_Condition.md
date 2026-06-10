# 🏠 17 — Nested Loop & Nested Condition

---

## 1) Penjelasan

**Nested loop** adalah loop di dalam loop. Setiap iterasi loop luar, loop dalam berjalan dari awal sampai selesai.

**Nested condition** adalah kondisi di dalam kondisi (if di dalam if).

Keduanya berguna untuk memproses data **2 dimensi** (tabel, matriks, pola bintang). Tapi perlu hati-hati: kompleksitas waktu bisa melonjak.

---

## 2) Fungsi

- Membuat pola segitiga, persegi, diamond.
- Iterasi array 2 dimensi.
- Mencetak tabel perkalian.
- Mencari pasangan data (brute force).
- Logika bersyarat bersarang (validasi bertingkat).

---

## 3) Code

### Tabel Perkalian 1–10

```javascript
for (let i = 1; i <= 10; i++) {
  let baris = "";
  for (let j = 1; j <= 10; j++) {
    baris += (i * j).toString().padStart(4);
  }
  console.log(baris);
}
```

### Segitiga Bintang

```javascript
const tinggi = 5;
for (let i = 1; i <= tinggi; i++) {
  let bintang = "";
  for (let j = 1; j <= i; j++) {
    bintang += "*";
  }
  console.log(bintang);
}
```

Output:
```
*
**
***
****
*****
```

### Nested Condition

```javascript
const peran = "mandor";
const cuaca = "hujan";

if (peran === "mandor") {
  if (cuaca === "hujan") {
    console.log("Mandor: kerja di dalam rumah");
  } else {
    console.log("Mandor: kerja di luar");
  }
} else {
  console.log("Bukan mandor, ikuti instruksi mandor");
}
```

---

## 4) Analogi Rumah (Tabel + Narasi)

| Konsep JS | Analogi Membangun Rumah |
|-----------|------------------------|
| Loop luar | Setiap **lantai** rumah. |
| Loop dalam | Setiap **kamar** di lantai itu. |
| Nested condition | "Kalau lantai 1 → jika ruang tamu → pasang ubin, jika kamar mandi → pasang keramik." |
| Kompleksitas tinggi | Membangun 10 lantai × 10 kamar = 100 kamar. 10× lebih lama dari 1 lantai 10 kamar. |

### Narasi

Kamu membangun rumah 2 lantai. Untuk **setiap lantai** (loop luar), kamu harus **membangun semua kamar** di lantai itu (loop dalam). Lantai 1: kamar tamu, kamar mandi, dapur. Selesai? Lanjut ke lantai 2: kamar tidur, balkon, kamar mandi lagi.

Nested condition terjadi saat kamu memutuskan: "Jika ini lantai 1 **dan** ini kamar mandi, pasang keramik anti-air. Jika lantai 2 **dan** ini kamar tidur, pasang vinyl."

---

## 5) Use Case

- Papan catur / game board (grid 8×8).
- Tabel data dari API.
- Algoritma sorting (bubble sort punya nested loop).
- Filter produk berdasarkan kategori + harga.
- Pattern printing soal wawancara.

---

## 6) Kesalahan Umum

| Kesalahan | Dampak |
|-----------|--------|
| Menggunakan nested loop 3+ level | Kode susah dibaca, O(n³) |
| Lupa reset variabel inner loop | Akumulasi data dari iterasi sebelumnya |
| Nested if terlalu dalam | Gunakan early return / guard clause |
| Memproses grid besar dengan nested loop | Bisa freeze browser (50.000+ iterasi) |

> **Aturan praktik**: Jika nested loop > 2 level, tanya diri sendiri: "Apa bisa saya pakai pendekatan lain?"

---

## 7) Benang Merah

- Materi 16 (Loop dasar) → **Materi 17** (Nested Loop) → Materi 18 (Error Handling)
- Nested loop adalah **penggabungan dua loop** — logika yang sama, cuma diskalakan.
- Kode yang kompleks rentan error → Materi 18 mengajarkan cara menangani error.

---

## 8) Soal

### Soal 1 — Segitiga terbalik
Buat program yang mencetak:

```
*****
****
***
**
*
```

<details>
<summary>Jawaban</summary>

```javascript
for (let i = 5; i >= 1; i--) {
  let bintang = "";
  for (let j = 1; j <= i; j++) {
    bintang += "*";
  }
  console.log(bintang);
}
```
</details>

### Soal 2 — Tabel perkalian custom
Minta input `n`, lalu cetak tabel perkalian `n × n`.

<details>
<summary>Jawaban</summary>

```javascript
const n = Number(prompt("Masukkan ukuran tabel:"));
for (let i = 1; i <= n; i++) {
  let baris = "";
  for (let j = 1; j <= n; j++) {
    baris += (i * j).toString().padStart(4);
  }
  console.log(baris);
}
```
</details>

### Soal 3 — Pola kotak
Cetak kotak 5×5 dengan bintang, tapi bagian dalam kosong (hanya tepi).

<details>
<summary>Jawaban</summary>

```javascript
const ukuran = 5;
for (let i = 1; i <= ukuran; i++) {
  let baris = "";
  for (let j = 1; j <= ukuran; j++) {
    if (i === 1 || i === ukuran || j === 1 || j === ukuran) {
      baris += "*";
    } else {
      baris += " ";
    }
  }
  console.log(baris);
}
```
</details>
