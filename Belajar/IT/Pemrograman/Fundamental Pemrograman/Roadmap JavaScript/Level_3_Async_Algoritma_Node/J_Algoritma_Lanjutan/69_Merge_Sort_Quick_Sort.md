# 69 — Merge Sort & Quick Sort

## Penjelasan
Merge Sort dan Quick Sort adalah algoritma divide & conquer dengan kompleksitas O(n log n) — jauh lebih cepat dari O(n²) untuk data besar. Merge sort stabil dan konsisten. Quick sort lebih cepat rata-rata tapi worst-case O(n²). JavaScript `Array.prototype.sort()` menggunakan Timsort (gabungan merge + insertion).

## Fungsi
- Merge sort: data sangat besar, butuh stabilitas, linked list
- Quick sort: performa rata-rata terbaik, data acak
- `.sort()`: default untuk kebutuhan sorting umum di JS

## Code

```javascript
// === Merge Sort O(n log n) ===
function mergeSort(arr) {
  if (arr.length <= 1) return arr;
  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));
  return merge(left, right);
}

function merge(left, right) {
  const result = [];
  let i = 0, j = 0;
  while (i < left.length && j < right.length) {
    if (left[i] <= right[j]) result.push(left[i++]);
    else result.push(right[j++]);
  }
  return [...result, ...left.slice(i), ...right.slice(j)];
}

// === Quick Sort O(n log n) rata-rata ===
function quickSort(arr) {
  if (arr.length <= 1) return arr;
  const pivot = arr[arr.length - 1];
  const left = [], right = [];
  for (let i = 0; i < arr.length - 1; i++) {
    if (arr[i] < pivot) left.push(arr[i]);
    else right.push(arr[i]);
  }
  return [...quickSort(left), pivot, ...quickSort(right)];
}

// === Benchmark semua sorting ===
const randomArr = (n) => Array.from({ length: n }, () => Math.floor(Math.random() * 10000));

const dataKecil = randomArr(100);
const dataBesar = randomArr(10000);

// Bubble sort — jangan dijalankan untuk 10000 data
console.time('Merge 100');
mergeSort(dataKecil);
console.timeEnd('Merge 100');

console.time('Quick 100');
quickSort(dataKecil);
console.timeEnd('Quick 100');

console.time('Merge 10000');
mergeSort(dataBesar);
console.timeEnd('Merge 10000');

console.time('Quick 10000');
quickSort(dataBesar);
console.timeEnd('Quick 10000');

// Built-in sort
console.time('JS Sort 10000');
dataBesar.sort((a, b) => a - b);
console.timeEnd('JS Sort 10000');
```

## Analogi Rumah — Pecah & Gabung vs Pilih Pivot

| Algoritma | Cara | Ilustrasi Rumah |
|-----------|------|-----------------|
| Merge Sort | Pecah tumpukan bata jadi 2, urutkan masing-masing, gabung | Belah tumpukan bata jadi dua. Urutkan masing-masing. Satukan dengan membandingkan bata teratas dari dua tumpukan |
| Quick Sort | Pilih pivot (bata tengah), pisahkan kecil ke kiri, besar ke kanan, rekursif | Ambil satu bata sebagai patokan. Semua bata lebih kecil ke kiri, lebih besar ke kanan. Ulangi tiap kelompok |

## Use Case
- Merge sort: sorting data di database eksternal (data terlalu besar untuk memory)
- Quick sort: sorting array in-memory di sistem operasi
- `.sort()`: semua kebutuhan sorting general di JavaScript

## Kesalahan Umum
- Quick sort dengan pivot tetap (first/last) → worst-case O(n²) pada data sudah terurut
- Merge sort menggunakan banyak memori ekstra (O(n)) — tidak cocok untuk memory terbatas
- Tidak membuat salinan di merge sort → data tercampur

## Benang Merah
Dari Materi 68 (O(n²) lambat untuk data besar) → sekarang punya O(n log n). Struktur data seperti linked list dan tree butuh pemahaman sorting. Lanjut ke Materi 70 (Linked List) — struktur data linear dinamis.

## Soal + Jawaban

**1. Mengapa Quick Sort rata-rata lebih cepat dari Merge Sort meski sama O(n log n)?**
Jawaban: Quick Sort memiliki konstanta lebih kecil — operasi in-place lebih sedikit alokasi memori dan cache miss.

**2. Kapan Merge Sort lebih baik dari Quick Sort?**
Jawaban: Saat stabilitas diperlukan (urutan elemen sama dipertahankan) atau data berupa linked list.

**3. Algoritma apa yang digunakan JavaScript `.sort()` di balik layar?**
Jawaban: Timsort — gabungan Merge Sort dan Insertion Sort, diperkenalkan oleh Python.
