# 68 — Sorting Dasar: Bubble, Selection, Insertion

## Penjelasan
Sorting adalah mengatur data dalam urutan tertentu. Tiga algoritma sorting dasar: Bubble sort (tukar tetangga), Selection sort (cari terkecil, taruh depan), Insertion sort (sisipkan ke posisi tepat). Semua O(n²) di worst case, tapi Insertion sort efisien untuk data hampir terurut O(n).

## Fungsi
- Bubble sort: edukasi (mudah dipahami)
- Selection sort: minim swap (cocok jika write cost mahal)
- Insertion sort: data hampir terurut, data kecil, atau real-time streaming

## Code

```javascript
// === Bubble Sort O(n²) ===
function bubbleSort(arr) {
  const a = [...arr];
  for (let i = 0; i < a.length; i++) {
    for (let j = 0; j < a.length - i - 1; j++) {
      if (a[j] > a[j + 1]) {
        [a[j], a[j + 1]] = [a[j + 1], a[j]];
      }
    }
  }
  return a;
}

// === Selection Sort O(n²) ===
function selectionSort(arr) {
  const a = [...arr];
  for (let i = 0; i < a.length; i++) {
    let minIdx = i;
    for (let j = i + 1; j < a.length; j++) {
      if (a[j] < a[minIdx]) minIdx = j;
    }
    if (minIdx !== i) [a[i], a[minIdx]] = [a[minIdx], a[i]];
  }
  return a;
}

// === Insertion Sort O(n²) — O(n) jika hampir terurut ===
function insertionSort(arr) {
  const a = [...arr];
  for (let i = 1; i < a.length; i++) {
    const key = a[i];
    let j = i - 1;
    while (j >= 0 && a[j] > key) {
      a[j + 1] = a[j];
      j--;
    }
    a[j + 1] = key;
  }
  return a;
}

// === Visualisasi langkah ===
const data = [64, 34, 25, 12, 22, 11, 90];
console.log('Insertion Sort:');
let step = 1;
const arr = [...data];
for (let i = 1; i < arr.length; i++) {
  const key = arr[i];
  let j = i - 1;
  while (j >= 0 && arr[j] > key) {
    arr[j + 1] = arr[j];
    j--;
  }
  arr[j + 1] = key;
  console.log(`  Langkah ${step++}: [${arr.join(', ')}]`);
}
```

## Analogi Rumah — Urutkan Bata

| Algoritma | Cara | Ilustrasi Rumah |
|-----------|------|-----------------|
| Bubble | Tukar tetangga jika salah urut | Dua tukang: "Ini bata lebih besar, tukar!" — terus dari ujung ke ujung |
| Selection | Cari bata terkecil, taruh paling kiri | Cari bata paling pendek di seluruh tumpukan, letakkan di posisi 1, ulangi |
| Insertion | Ambil bata, sisipkan ke posisi yang benar di tumpukan tangan | Ambil satu bata dari tumpukan, selipkan di antara bata yang sudah rapi di tangan |

## Use Case
- Bubble sort: tidak dipakai di production, hanya untuk teaching
- Selection sort: embedded system dengan memori terbatas (minim write)
- Insertion sort: sorting kartu di game, data real-time yang terus bertambah

## Kesalahan Umum
- Menggunakan bubble sort untuk ribuan data — sangat lambat
- Tidak membuat salinan array → memutasi data asli tanpa sadar
- Insertion sort: lupa shift elemen sebelum insert → data tertimpa

## Benang Merah
Materi 67 (searching) membutuhkan data sorted. Sorting dasar O(n²) belum efisien untuk data besar. Ini mengantar ke Materi 69 (Merge Sort & Quick Sort) yang menawarkan O(n log n).

## Soal + Jawaban

**1. Algoritma sorting mana yang paling efisien untuk data yang hampir terurut?**
Jawaban: Insertion sort — kompleksitasnya O(n) pada data hampir terurut.

**2. Berapa jumlah swap pada selection sort untuk array [3, 1, 2]?**
Jawaban: 2 swap. Langkah 1: [1, 3, 2]; Langkah 2: [1, 2, 3].

**3. Mengapa bubble sort tidak digunakan di production?**
Jawaban: O(n²) di semua kasus, sangat lambat untuk data besar. Algoritma O(n log n) jauh lebih efisien.
