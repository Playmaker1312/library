# 67 — Linear & Binary Search

## Penjelasan
Searching adalah proses menemukan elemen dalam kumpulan data. Linear search memeriksa setiap elemen satu per satu (O(n)). Binary search membagi data terurut menjadi dua setiap langkah (O(log n)) — jauh lebih cepat untuk data besar.

## Fungsi
- Linear search: data tidak terurut, data kecil, atau data real-time
- Binary search: data terurut, data besar, performa kritis

## Code

```javascript
// === Linear Search O(n) ===
function linearSearch(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === target) return i;
  }
  return -1;
}

// === Binary Search Iteratif O(log n) ===
function binarySearchIteratif(arr, target) {
  let left = 0, right = arr.length - 1;
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
  }
  return -1;
}

// === Binary Search Rekursif O(log n) ===
function binarySearchRekursif(arr, target, left = 0, right = arr.length - 1) {
  if (left > right) return -1;
  const mid = Math.floor((left + right) / 2);
  if (arr[mid] === target) return mid;
  if (arr[mid] < target) return binarySearchRekursif(arr, target, mid + 1, right);
  return binarySearchRekursif(arr, target, left, mid - 1);
}

// === Benchmark ===
const data = Array.from({ length: 1000000 }, (_, i) => i + 1);
const target = 999999;

console.time('Linear');
linearSearch(data, target);
console.timeEnd('Linear');

console.time('Binary');
binarySearchIteratif(data.sort((a, b) => a - b), target);
console.timeEnd('Binary');
```

## Analogi Rumah — Cari Buku di Rak

| Metode | Cara | Ilustrasi |
|--------|------|-----------|
| Linear | Periksa dari kiri ke kanan | Ambil buku ke-1, lihat, taruh lagi. Ambil ke-2, lihat, taruh lagi... |
| Binary | Buka tengah, tentukan kiri/kanan, ulangi | Buka buku di tengah rak — "A, ini K-O". Berarti cari di kiri. Buka tengah kiri... |

## Use Case
- Linear search: mencari user di chat list (data tidak terurut, real-time)
- Binary search: mencari produk di e-commerce (data terurut by harga/nama)
- Binary search: mencari log berdasarkan timestamp

## Kesalahan Umum
- Lupa **mensortir data** sebelum binary search — hasil tidak valid
- Overflow mid = `(left + right) / 2` pada bahasa dengan integer terbatas (JS aman)
- Binary search rekursif tanpa base case → stack overflow

## Benang Merah
Dari Materi 66 (Big-O) kita tahu O(n) vs O(log n). Searching adalah penerapan langsung. Binary search butuh data terurut — ini mengantar ke Materi 68 (Sorting Dasar) karena sorting adalah prasyarat.

## Soal + Jawaban

**1. Kapan linear search lebih baik dari binary search?**
Jawaban: Saat data tidak terurut, data sangat kecil (n < 100), atau hanya perlu sekali cari (biaya sorting tidak sebanding).

**2. Berapa langkah maksimal binary search untuk array 1.000.000 elemen?**
Jawaban: ~20 langkah (log₂ 1.000.000 ≈ 20).

**3. Apa syarat mutlak binary search?**
Jawaban: Data harus **sorted/terurut** ascending atau descending.
