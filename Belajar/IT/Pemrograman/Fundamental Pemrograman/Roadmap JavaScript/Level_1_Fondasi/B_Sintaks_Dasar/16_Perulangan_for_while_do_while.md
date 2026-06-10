# 🏠 16 — Perulangan: for, while, do-while

---

## 1) Penjelasan

Perulangan (loop) adalah struktur yang mengulang eksekusi blok kode selama kondisi tertentu masih `true`. Tanpa loop, kita harus menulis kode yang sama puluhan atau ratusan kali.

| Jenis | Kapan dipakai |
|-------|---------------|
| **`for`** | Ketika jumlah iterasi sudah diketahui sebelumnya. |
| **`while`** | Ketika jumlah iterasi tidak pasti, hanya tahu kondisi berhenti. |
| **`do-while`** | Sama seperti while, tapi blok **dijalankan minimal sekali** sebelum kondisi dicek. |
| **`break`** | Keluar paksa dari loop. |
| **`continue`** | Loncat ke iterasi berikutnya, lewati kode di bawahnya. |

---

## 2) Fungsi

- **`for (init; kondisi; increment)`** — loop dengan counter.
- **`while (kondisi)`** — loop berbasis kondisi (cek dulu).
- **`do { } while (kondisi)`** — loop berbasis kondisi (jalan dulu).
- **`break`** — hentikan loop sepenuhnya.
- **`continue`** — skip sisa iterasi saat ini, lanjut ke iterasi berikutnya.

---

## 3) Code

### FizzBuzz (1–100)

```javascript
for (let i = 1; i <= 100; i++) {
  if (i % 3 === 0 && i % 5 === 0) {
    console.log("FizzBuzz");
  } else if (i % 3 === 0) {
    console.log("Fizz");
  } else if (i % 5 === 0) {
    console.log("Buzz");
  } else {
    console.log(i);
  }
}
```

### while — tunggu hingga kondisi terpenuhi

```javascript
let bahan = 0;
while (bahan < 10) {
  console.log("Membeli bahan ke-" + (bahan + 1));
  bahan++;
}
```

### do-while — setidaknya sekali

```javascript
let coba = 0;
do {
  console.log("Mencoba koneksi... (" + (coba + 1) + ")");
  coba++;
} while (coba < 3);
```

### break & continue

```javascript
for (let i = 1; i <= 10; i++) {
  if (i === 5) continue; // skip angka 5
  if (i === 8) break;    // berhenti di 8
  console.log(i); // 1,2,3,4,6,7
}
```

---

## 4) Analogi Rumah (Tabel + Narasi)

| Konsep JS | Analogi Membangun Rumah |
|-----------|------------------------|
| `for` | "Pasang 100 bata." — kamu tahu persis jumlahnya. |
| `while` | "Aduk semen sampai konsistensinya pas." — kamu tidak tahu berapa kali, hanya tahu kondisi berhenti. |
| `do-while` | "Cek dulu apakah cat cukup, minimal cek satu kaleng." — lakukan dulu, baru evaluasi. |
| `break` | "Kalau hujan deras, berhenti kerja." — keluar loop di tengah jalan. |
| `continue` | "Kalau bata retak, skip, lanjut bata berikutnya." — lewati satu iterasi. |

### Narasi

Membangun rumah penuh dengan pengulangan: memaku 200 paku, mengecat 4 dinding, mengaduk semen 5 kali. Tanpa pengulangan, kamu harus menulis instruksi "paku paku ini" sebanyak 200 kali. Loop adalah **asisten mandor yang melakukan tugas berulang** dengan patuh. Kamu cukup bilang: "Ulangi 200 kali: ambil paku, ketuk, lanjut."

---

## 5) Use Case

- Menampilkan daftar produk dari database.
- Validasi input berulang sampai benar.
- FizzBuzz / tes coding interview.
- Infinite scroll / polling data.
- Iterasi array (lanjutan ke materi array nanti).

---

## 6) Kesalahan Umum

| Kesalahan | Contoh Salah | Benar |
|-----------|-------------|-------|
| Infinite loop | `while (true)` tanpa break | pastikan kondisi berubah |
| Off-by-one | `i <= 100` tertulis `< 100` | periksa batas |
| Lupa increment | `for (let i=0; i<10;)` | tambah `i++` |
| `continue` sebelum increment | loop tak berujung | tempatkan increment di tempat aman |
| Ganti variabel di dalam loop | `for (i=0; ...)` → i diubah di dalam | jangan ubah iterator |

---

## 7) Benang Merah

- Materi 15 (Percabangan) → **Materi 16** (Loop) → Materi 17 (Nested Loop)
- Percabangan (`if` di dalam FizzBuzz) adalah **syarat** agar loop berguna. Loop memberi **skala** pada logika keputusan.
- Materi 17 akan menggabungkan loop di dalam loop untuk mencetak pola.

---

## 8) Soal

### Soal 1 — for
Cetak angka kelipatan 3 dari 3 sampai 30 menggunakan `for`.

<details>
<summary>Jawaban</summary>

```javascript
for (let i = 3; i <= 30; i += 3) {
  console.log(i);
}
```
</details>

### Soal 2 — while
Buat program yang terus meminta input sampai user mengetik `"exit"`.

<details>
<summary>Jawaban</summary>

```javascript
let input = "";
while (input !== "exit") {
  input = prompt("Ketik 'exit' untuk berhenti:");
}
```
</details>

### Soal 3 — break & continue
Cetak angka 1–20, tapi lewati kelipatan 4 dan berhenti jika angka > 15.

<details>
<summary>Jawaban</summary>

```javascript
for (let i = 1; i <= 20; i++) {
  if (i % 4 === 0) continue;
  if (i > 15) break;
  console.log(i);
}
```
</details>
