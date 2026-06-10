# 66 — Big-O Notation

## Penjelasan
Big-O Notation adalah cara mengukur efisiensi algoritma berdasarkan pertumbuhan waktu eksekusi (time complexity) dan penggunaan memori (space complexity) seiring bertambahnya input. Tujuannya: membandingkan algoritma secara objektif tanpa tergantung hardware.

| Notasi | Nama | Contoh |
|--------|------|--------|
| O(1) | Constant | Akses array[index] |
| O(log n) | Logarithmic | Binary search |
| O(n) | Linear | Linear search |
| O(n log n) | Linearithmic | Merge sort |
| O(n²) | Quadratic | Bubble sort |

## Fungsi
- Memprediksi performa algoritma sebelum dijalankan
- Membantu memilih algoritma terbaik untuk data besar
- Bahasa universal diskusi optimasi di tim engineer

## Code — Analisis Big-O dari 10 Fungsi Level 1-2

```javascript
// 1. O(1) — Akses properti
const getNama = (user) => user.nama;

// 2. O(n) — Cari di array
const cari = (arr, target) => arr.find((item) => item === target);

// 3. O(n) — Hitung total
const totalBelanja = (items) => items.reduce((sum, item) => sum + item.harga, 0);

// 4. O(n²) — Cek duplikat (nested loop)
const cekDuplikat = (arr) => {
  for (let i = 0; i < arr.length; i++)
    for (let j = i + 1; j < arr.length; j++)
      if (arr[i] === arr[j]) return true;
  return false;
};

// 5. O(n²) — Bubble sort
const bubbleSort = (arr) => {
  for (let i = 0; i < arr.length; i++)
    for (let j = 0; j < arr.length - i - 1; j++)
      if (arr[j] > arr[j + 1]) [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
};

// 6. O(log n) — Binary search
const binarySearch = (arr, target) => {
  let left = 0, right = arr.length - 1;
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
  }
  return -1;
};

// 7. O(n log n) — Merge sort
const mergeSort = (arr) => {
  if (arr.length <= 1) return arr;
  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));
  return merge(left, right);
};

// 8. O(n) — Filter array
const filterGenap = (arr) => arr.filter((n) => n % 2 === 0);

// 9. O(n) — Map array
const kaliDua = (arr) => arr.map((n) => n * 2);

// 10. O(2ⁿ) — Fibonacci rekursif (tanpa memo)
const fib = (n) => (n <= 1 ? n : fib(n - 1) + fib(n - 2));
```

## Analogi Rumah — Waktu Cari Alat di Gudang

| Situasi | Notasi | Penjelasan |
|---------|--------|------------|
| Palu ada di kotak "Paku & Palu" | O(1) | Langsung ambil, tahu persis lokasi |
| Gudang rapi — rak berlabel A-Z | O(log n) | Buka tengah, lihat arah, ulangi |
| Gudang berantakan — cek satu-satu | O(n) | Periksa setiap tumpukan |
| Cari 10 alat, tiap alat perlu cek semua tumpukan | O(n²) | Tiap alat butuh scan ulang seluruh gudang |

## Use Case
- Memilih algoritma sorting untuk data 1 juta baris → hindari O(n²)
- Optimasi query database dengan index → O(log n) instead of O(n)
- Menentukan apakah perlu caching (O(1) lookup) untuk fungsi berat

## Kesalahan Umum
- Menyamakan O(2n) dengan O(n²) — padahal O(2n) = O(n), konstanta diabaikan
- Menganggap O(1) berarti cepat — O(1) bisa lambat jika operasinya berat (misal hash function kompleks)
- Lupa space complexity — algoritma cepat bisa makan banyak memori

## Benang Merah
Dari Materi 65 (backend selesai dibangun) → sekarang masuk fase **optimasi**. Big-O adalah alat ukur yang akan dipakai di semua materi berikutnya. Lanjut ke Materi 67 (Linear & Binary Search) yang langsung menerapkan konsep O(n) vs O(log n).

## Soal + Jawaban

**1. Apa kompleksitas dari kode berikut?**
```javascript
for (let i = 0; i < n; i++)
  for (let j = 0; j < n; j++)
    console.log(i, j);
```
Jawaban: O(n²) — nested loop kedua iterasi sampai n.

**2. Mana yang lebih cepat untuk data 1 juta elemen? O(n) atau O(log n)?**
Jawaban: O(log n). O(n) perlu 1 juta langkah, O(log n) hanya ~20 langkah.

**3. Mengapa konstanta diabaikan dalam Big-O?**
Jawaban: Big-O mengukur **pertumbuhan** seiring n → ∞. Konstanta tidak signifikan dibandingkan skala pertumbuhan. O(100n) tetap O(n).
