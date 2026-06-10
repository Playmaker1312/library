# 14 — Input & Output di Node.js: readline & console

---

## 1. Penjelasan

I/O (Input/Output) adalah cara program berinteraksi dengan dunia luar.

### Output — console

| Method | Fungsi |
|--------|--------|
| `console.log()` | Output standar (informasi umum) |
| `console.error()` | Output error (stderr) |
| `console.table()` | Menampilkan data array/object dalam tabel |
| `console.warn()` | Peringatan |

### Input — readline module

`readline` adalah module bawaan Node.js untuk membaca input dari terminal (stdin).

### Alur Dasar:

```
[User] → input (stdin) → [Program] → output (stdout) → [Terminal]
```

---

## 2. Fungsi

- **console.log / error / table**: Menampilkan hasil ke terminal dengan format berbeda.
- **readline module**: Membaca input dari pengguna secara sinkron (callback-based) atau dengan `readline/promises` (async/await).

---

## 3. Code — Program Sapa User dengan Format Khusus

```javascript
// === PROGRAM SAPA INTERAKTIF ===

// Versi 1: Callback-based (readline standar)
function sapaCallback() {
  const readline = require("readline");

  const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout,
  });

  console.log("Selamat datang di Program Sapa!");
  console.log("=" .repeat(35));

  rl.question("Siapa nama Anda? ", (nama) => {
    rl.question("Berapa usia Anda? ", (usia) => {
      const sapaan = `
  ╔══════════════════════╗
  ║  Halo, ${nama.toUpperCase().padEnd(15)} ║
  ║  Usia ${usia} tahun            ║
  ║  Semangat belajar! 🏠   ║
  ╚══════════════════════╝
      `;
      console.log(sapaan);
      console.log("Karakter nama Anda:", nama.trim().length);
      console.log("Karakter pertama:", nama.trim().charAt(0).toUpperCase());
      rl.close();
    });
  });

  rl.on("close", () => {
    console.log("Terima kasih! Sampai jumpa.");
    process.exit(0);
  });
}

// Versi 2: Async/await (readline/promises — lebih modern)
async function sapaAsync() {
  const readline = require("readline/promises");

  const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout,
  });

  try {
    const nama = await rl.question("Siapa nama Anda? ");
    const usia = await rl.question("Berapa usia Anda? ");

    console.log(`\nHalo ${nama.trim()}, selamat belajar JavaScript!`);
    console.log(`Usia ${usia} tahun — waktu yang tepat untuk jadi developer.`);
    console.log(`Nama Anda terdiri dari ${nama.trim().length} karakter.`);
    console.table([
      { Properti: "Nama", Nilai: nama.trim() },
      { Properti: "Usia", Nilai: usia },
      { Properti: "Huruf Depan", Nilai: nama.trim().charAt(0).toUpperCase() },
    ]);
  } catch (err) {
    console.error("Terjadi error:", err.message);
  } finally {
    rl.close();
  }
}

// Panggil salah satu:
// sapaCallback();   // versi callback
// sapaAsync();      // versi async (rekomendasi)


// === DEMO console.table ===

const dataMahasiswa = [
  { Nama: "Budi", Usia: 20, Jurusan: "Informatika" },
  { Nama: "Siti", Usia: 22, Jurusan: "Sistem Informasi" },
  { Nama: "Ali",  Usia: 21, Jurusan: "Teknik Komputer" },
];

console.log("Data Mahasiswa:");
console.table(dataMahasiswa);

// Output:
// ┌──────┬───────┬──────┬─────────────────────┐
// │ idx  │ Nama  │ Usia │      Jurusan        │
// ├──────┼───────┼──────┼─────────────────────┤
// │  0   │ Budi  │  20  │    Informatika      │
// │  1   │ Siti  │  22  │ Sistem Informasi    │
// │  2   │ Ali   │  21  │ Teknik Komputer     │
// └──────┴───────┴──────┴─────────────────────┘
```

---

## 4. Analogi Rumah (Membangun Rumah)

**I/O = interaksi antara rumah dan dunia luar.**

| Konsep I/O | Analogi di Rumah |
|------------|------------------|
| `console.log()` | Memasang papan informasi di depan rumah |
| `console.error()` | Alarm kebakaran — sinyal darurat |
| `console.table()` | Papan data statistik di dinding ruang tamu |
| `readline.question()` | Mengetuk pintu dan bertanya pada tamu |
| Input (stdin) | Tamu menjawab pertanyaan dari balik pintu |
| Output (stdout) | Tuan rumah memberikan respons |
| `process.stdin` | Bel pintu (saluran masuk) |
| `process.stdout` | Pengeras suara (saluran keluar) |
| `rl.close()` | Menutup pintu setelah tamu pergi |
| Callback | "Nanti saya kabari" — janji untuk merespons nanti |
| Async/await | "Saya tunggu jawabannya" — menunggu dengan sabar |

> **Narasi**: Rumahmu adalah program JavaScript. `console.log` seperti papan informasi yang kamu pasang di pagar: "Selamat Datang di Rumah Budi!" `console.error` seperti alarm: "KEBOCORAN PIPA!". `console.table` adalah papan statistik di dinding. Saat ingin menerima tamu (`input`), kamu mengetuk pintu (`rl.question`), tamu menjawab, dan kamu memberi respons. Proses ini adalah **dialog** — tanya, dengar, jawab. Setelah selesai, kamu tutup pintu (`rl.close`). Seluruh interaksi ini adalah I/O.

---

## 5. Use Case

| Situasi | Pendekatan I/O |
|---------|----------------|
| CLI app sederhana (sapa, kalkulator) | `readline` + `console.log` |
| Debugging variabel kompleks | `console.table()` atau `console.log(JSON.stringify(data, null, 2))` |
| Log error ke file | `console.error` diarahkan ke stderr |
| Aplikasi interaktif (Tic Tac Toe CLI) | `readline/promises` dengan loop |
| Form input sederhana di terminal | `readline.question()` serial |

---

## 6. Kesalahan Umum

❌ **Lupa menutup `readline` interface**
```javascript
const rl = require("readline").createInterface(...);
rl.question("Nama: ", (nama) => {
  console.log(`Halo ${nama}`);
  // rl.close() tidak dipanggil — program tidak berhenti!
});
```

❌ **Input readline selalu string**
```javascript
const usia = await rl.question("Usia: ");
console.log(usia + 5); // "205" — karena usia adalah string "20"!
// Benar: Number(usia) + 5
```

❌ **Callback hell saat banyak pertanyaan**
```javascript
// ❌ Jangan: nested callback berlapis
rl.question("a: ", (a) => {
  rl.question("b: ", (b) => {
    rl.question("c: ", (c) => {
      // ...semakin dalam, semakin kacau
    });
  });
});

// ✅ Pakai async/await atau promise chaining
```

❌ **Mengira `console.log` dan `console.error` sama saja**
```javascript
console.log("info");  // stdout — bisa dipipe
console.error("err"); // stderr — tetap ke terminal walau stdout dipipe
```

---

## 7. Benang Merah

```
Materi 13 (String — method esensial untuk memproses teks)
    ↓
Materi 14 (Input & Output — aplikasi string ke interaksi user nyata) ← KAMU DI SINI
    ↓
Materi 15 (Percabangan — if/else, switch — decision making setelah mendapat input)
```

String adalah bahan bakarnya, I/O adalah sistem dialognya. Setelah belajar memproses string (capitalize, split, trim), sekarang kita terapkan ke program nyata: tanya nama, format sapaan, tampilkan data. Selanjutnya kita akan menambahkan **logika percabangan** — jika user menjawab "A", lakukan X; jika "B", lakukan Y.

---

## 8. Soal

### Soal 1 (Mudah)
Apa output dari:
```javascript
console.log("Halo dunia");
console.error("Terjadi error");
console.table([{ a: 1, b: 2 }]);
```

<details>
<summary>Jawaban</summary>

```
Halo dunia                          (stdout)
Terjadi error                       (stderr)
┌──────┬─────┬─────┐
│ idx  │ a   │ b   │
├──────┼─────┼─────┤
│  0   │ 1   │ 2   │
└──────┴─────┴─────┘
```
</details>

---

### Soal 2 (Sedang)
Perbaiki kode berikut agar menjumlahkan usia dengan benar:
```javascript
const readline = require("readline/promises");
async function main() {
  const rl = readline.createInterface({ input: process.stdin, output: process.stdout });
  const usia = await rl.question("Berapa usia Anda? ");
  console.log("Tahun depan Anda berusia " + (usia + 1) + " tahun");
  rl.close();
}
```

<details>
<summary>Jawaban</summary>

```javascript
const readline = require("readline/promises");
async function main() {
  const rl = readline.createInterface({ input: process.stdin, output: process.stdout });
  const usia = await rl.question("Berapa usia Anda? ");
  const usiaNumber = Number(usia);  // ← konversi string → number
  console.log("Tahun depan Anda berusia " + (usiaNumber + 1) + " tahun");
  rl.close();
}
```
</details>

---

### Soal 3 (Tantangan)
Buat program CLI dengan `readline/promises` yang:
1. Menerima nama user
2. Menerima daftar hobi (pisahkan dengan koma)
3. Menampilkan nama dengan format kapital (pakai fungsi `capitalizeName` dari Materi 13)
4. Menampilkan jumlah hobi dan tiap hobi dalam format numbered list
5. Menampilkan pesan perpisahan yang mengandung inisial user

<details>
<summary>Jawaban</summary>

```javascript
const readline = require("readline/promises");

function capitalizeName(nama) {
  return nama
    .trim()
    .split(" ")
    .map(kata => kata.charAt(0).toUpperCase() + kata.slice(1).toLowerCase())
    .join(" ");
}

function getInitials(nama) {
  return nama
    .trim()
    .split(" ")
    .map(kata => kata.charAt(0).toUpperCase())
    .join("");
}

async function main() {
  const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout,
  });

  const nama = await rl.question("Masukkan nama Anda: ");
  const hobiInput = await rl.question("Masukkan hobi (pisahkan koma): ");

  const namaFormatted = capitalizeName(nama);
  const hobiList = hobiInput.split(",").map(h => h.trim());
  const inisial = getInitials(nama);

  console.log(`\n=== PROFIL ${namaFormatted.toUpperCase()} ===`);
  console.log(`Nama       : ${namaFormatted}`);
  console.log(`Inisial    : ${inisial}`);
  console.log(`Jumlah Hobi: ${hobiList.length}`);
  console.log("\nDaftar Hobi:");

  hobiList.forEach((hobi, i) => {
    console.log(`  ${i + 1}. ${hobi.charAt(0).toUpperCase() + hobi.slice(1)}`);
  });

  console.log(`\nTerima kasih ${namaFormatted} (${inisial})! Semoga hobimu menyenangkan.`);

  rl.close();
}

main().catch(console.error);
```
</details>
