# 73 — Dynamic Programming

## Penjelasan
Dynamic Programming (DP) adalah teknik optimasi dengan memecah masalah menjadi subproblems, menyimpan hasilnya (memoization/tabulation), dan menghindari komputasi berulang. Syarat DP: **optimal substructure** (solusi optimal dari subproblems membentuk solusi optimal) dan **overlapping subproblems** (subproblems yang sama muncul berkali-kali).

## Fungsi
- Optimasi rekursi yang menghitung ulang hal yang sama (Fibonacci, factorial)
- Masalah optimasi kombinatorial (Knapsack, Edit Distance, LCS)
- Bottom-up (tabulation) lebih efisien daripada top-down (memoization) dalam banyak kasus

## Code

```javascript
// === Fibonacci — Rekursif Biasa O(2ⁿ) ===
function fibNaive(n) {
  if (n <= 1) return n;
  return fibNaive(n - 1) + fibNaive(n - 2);
}

// === Fibonacci — Memoization (Top-down DP) O(n) ===
function fibMemo(n, memo = {}) {
  if (n <= 1) return n;
  if (memo[n] !== undefined) return memo[n];
  memo[n] = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
  return memo[n];
}

// === Fibonacci — Tabulation (Bottom-up DP) O(n) ===
function fibTab(n) {
  if (n <= 1) return n;
  const dp = [0, 1];
  for (let i = 2; i <= n; i++) dp[i] = dp[i - 1] + dp[i - 2];
  return dp[n];
}

// === 0/1 Knapsack — DP Tabulation ===
function knapsack(weights, values, capacity) {
  const n = weights.length;
  const dp = Array.from({ length: n + 1 }, () => Array(capacity + 1).fill(0));

  for (let i = 1; i <= n; i++) {
    for (let w = 1; w <= capacity; w++) {
      if (weights[i - 1] <= w) {
        dp[i][w] = Math.max(
          values[i - 1] + dp[i - 1][w - weights[i - 1]],
          dp[i - 1][w]
        );
      } else {
        dp[i][w] = dp[i - 1][w];
      }
    }
  }
  return dp[n][capacity];
}

// === Benchmark ===
const n = 40;
console.time('Naive');
fibNaive(n);
console.timeEnd('Naive');

console.time('Memo');
fibMemo(n);
console.timeEnd('Memo');

console.time('Tab');
fibTab(n);
console.timeEnd('Tab');

// Knapsack example
const weights = [2, 3, 4, 5];
const values = [3, 4, 5, 6];
console.log('Knapsack (capacity=5):', knapsack(weights, values, 5)); // 7
```

## Analogi Rumah — Tukang Catat Ukuran Ruangan

| Konsep | Analogi |
|--------|---------|
| Masalah asli | Menghitung total luas seluruh ruangan di rumah |
| Subproblem | Luas satu ruangan |
| Overlapping | Ruangan yang sama muncul di perhitungan berbeda (misal dipakai 2 kontraktor) |
| Memoization | Tukang catat "Ruang tamu = 20m²" — tidak perlu ukur ulang |
| Tabulation | Tukang ukur semua ruangan dari terkecil ke terbesar, catat di tabel |
| Optimal substructure | Luas total = jumlah luas setiap ruangan |
| 0/1 Knapsack | Pilih furnitur (berat, nilai) dengan kapasitas mobil pindahan terbatas, maksimalkan nilai |

## Use Case
- **Fibonacci & factorial** — textbook classic
- **Edit Distance (Levenshtein)** — spell checker, diff tools
- **Longest Common Subsequence** — git diff, DNA sequencing
- **0/1 Knapsack** — resource allocation, portfolio optimization
- **Shortest path** — Floyd-Warshall (DP) untuk semua pasangan vertex

## Kesalahan Umum
- Menggunakan DP padahal tidak ada overlapping subproblems → DP tidak membantu
- Lupa base case → infinite recursion
- Memoization overflow (stack) vs tabulation aman untuk n besar
- Index out of bounds pada tabel DP — periksa ukuran array

## Benang Merah
**PENUTUP Algoritma Level 3.** Kita mulai dari Big-O (alat ukur), searching (O(n) → O(log n)), sorting (O(n²) → O(n log n)), struktur data (Linked List → Tree → Graph), dan diakhiri DP (optimasi rekursi). Ini adalah fondasi algoritma yang akan dipakai di Proyek Level 3 dan persiapan Level 4 (Frontend).

## Soal + Jawaban

**1. Apa dua syarat utama suatu masalah bisa diselesaikan dengan DP?**
Jawaban: Optimal substructure dan overlapping subproblems.

**2. Apa perbedaan top-down (memoization) dan bottom-up (tabulation)?**
Jawaban: Top-down = rekursif + cache. Bottom-up = iteratif dari masalah terkecil. Bottom-up lebih cepat (tanpa overhead rekursi) dan aman dari stack overflow.

**3. Berapa nilai maksimum untuk knapsack kapasitas 5 dengan items: weight=[1,2,3], value=[6,10,12]?**
Jawaban: 22 — ambil semua item (1+2+3=5, 6+10+12=22).
