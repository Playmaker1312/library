# Bagaimana Komputer Memproses Kode — Dari Teks ke Eksekusi

## Penjelasan

Komputer tidak mengerti bahasa manusia. Ia hanya mengerti **biner** (0 dan 1). Karena itu, kode yang kita tulis (source code) harus **diterjemahkan** dulu menjadi instruksi yang bisa dimengerti mesin.

Prosesnya:

```
Source Code (teks) → Parser → AST (Abstract Syntax Tree) → Eksekusi
```

1. **Source Code** — Teks yang kita tulis di editor
2. **Parser** — Membaca teks baris per baris, memeriksa apakah sintaksnya valid
3. **AST** — Struktur pohon yang merepresentasikan maksud kode
4. **Eksekusi** — Instruksi dijalankan oleh CPU

### Compiled vs Interpreted

| Compiled (C, Go, Rust) | Interpreted (JavaScript, Python) |
|---|---|
| Diterjemahkan sekaligus sebelum dijalankan | Diterjemahkan baris per baris saat dijalankan |
| Lebih cepat eksekusi | Lebih lambat (tapi JIT membantu) |
| Error muncul saat kompilasi | Error muncul saat baris itu dijalankan |

JavaScript modern menggunakan **JIT (Just-In-Time) Compilation** — campuran interpretasi dan kompilasi untuk performa lebih baik.

---

## Fungsi

- Memahami **mengapa komputer tidak bisa menebak** maksud kita — setiap titik koma, kurung, dan ejaan penting
- Mengetahui **alur eksekusi** membantu kita debug dengan lebih efektif
- Membedakan kapan error terjadi: **syntax error** (salah grammar) vs **runtime error** (logika salah)

---

## Cara Implementasi / Code

```javascript
// JavaScript membaca kode baris per baris (top to bottom)
let nama = "Andi";       // Baris 1: deklarasi variabel
console.log(nama);       // Baris 2: cetak "Andi"

// Baris 3: error — kita tidak mendeklarasikan function ini
// sapa();                // ReferenceError: sapa is not defined

// Baris 4: tidak akan pernah sampai sini karena error di baris 3
console.log("Selesai");
```

**Latihan trace manual:**

```javascript
let x = 5;               // Langkah 1: x = 5
let y = x + 3;           // Langkah 2: y = 5 + 3 = 8
let z = y * 2;           // Langkah 3: z = 8 * 2 = 16
console.log(z);          // Langkah 4: cetak 16
```

Coba tebak: apa output dari kode di atas? (Jawaban: `16`)

---

## Analogi (Membangun Rumah)

| Konsep | Analogi Rumah |
|---|---|
| Source Code | **Cetak biru (blueprint)** — gambar denah rumah di atas kertas |
| Parser | **Arsitek** — membaca cetak biru, memeriksa apakah ukuran dan struktur masuk akal |
| AST | **Diagram struktur** — pohon yang menunjukkan hubungan antar ruangan |
| Eksekusi | **Tukang membangun** — mengikuti cetak biru untuk membuat rumah sungguhan |
| Syntax Error | Arsitek menemukan **ukuran pintu tidak masuk akal** (negatif) — bangunan dihentikan |
| Runtime Error | Tukang sedang memasang jendela, ternyata **kacanya pecah** — hanya bagian itu yang bermasalah |
| JIT Compilation | Tukang yang **semakin cepat** karena sudah hafal pola pekerjaan |

**Narasi:** Kamu adalah arsitek yang memberi cetak biru ke tukang. Tukang membaca gambar, lalu membangun. Kalau cetak biru salah (syntax error), tukang tidak akan mulai. Kalau ada masalah di tengah pembangunan (runtime error), hanya bagian itu yang terganggu. Tukang yang berpengalaman (JIT) bisa menyelesaikan lebih cepat karena sudah hafal pola.

---

## Dipakai Untuk Apa

- **Debugging:** Mengetahui bahwa error muncul karena kode belum dibaca sampai baris tertentu
- **Optimasi:** Memahami hoisting, scope, dan execution context di JS
- **Memilih bahasa:** Proyek real-time (game, sistem) butuh compiled; prototipe cepat cocok dengan interpreted

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Typo nama variabel | `console.lo("halo")` | Syntax Error — kode tidak jalan |
| Lupa titik koma | `let x = 5` (tanpa `;`) | Kebanyakan aman (ASI), tapi bisa jebakan |
| Panggil fungsi sebelum didefinisikan | `jalan(); function jalan(){}` | Aman karena **hoisting** — tapi tidak untuk `const`/`let` |
| Asumsi kode jalan dari kiri ke kanan | Padahal JS baris per baris top-to-bottom | Salah urutan logika |

---

## Benang Merah

- **Materi 1 (Programming = Instruksi Spesifik):** Komputer hanya menjalankan instruksi yang kita tulis. Sekarang kita pahami *bagaimana* instruksi itu diproses.
- **Materi 3 (Mengenal JavaScript):** JavaScript adalah bahasa interpreted/JIT. Ini menentukan cara kita menulis dan men-debug kode.

---

## Soal Latihan + Jawaban

### Soal 1 (Mudah)
Jelaskan dengan kata-katamu sendiri perbedaan antara **syntax error** dan **runtime error**. Beri contoh masing-masing.

<details>
<summary>Jawaban</summary>

**Syntax error:** Kesalahan grammar kode. Contoh: `console.log("halo"` (kurung tutup hilang). Akibat: kode tidak bisa dijalankan sama sekali.

**Runtime error:** Kesalahan yang muncul saat kode berjalan. Contoh: memanggil `namaVariableYangTidakAda`. Akibat: program berhenti di baris itu.
</details>

### Soal 2 (Sedang)
Trace manual kode berikut. Tulis nilai setiap variabel di setiap langkah.

```javascript
let a = 10;
let b = a - 4;
let c = a + b;
let d = c / 2;
console.log(d);
```

<details>
<summary>Jawaban</summary>

```
Langkah 1: a = 10
Langkah 2: b = 10 - 4 = 6
Langkah 3: c = 10 + 6 = 16
Langkah 4: d = 16 / 2 = 8
Output: 8
```
</details>

### Soal 3 (Tantangan)
Apa output dari kode berikut? **Jawab tanpa menjalankan.**

```javascript
console.log("Mulai");
function hitung(x) {
    return x * 2;
}
let hasil = hitung(5) + hitung(3);
console.log(hasil);
console.log("Selesai");
```

<details>
<summary>Jawaban</summary>

```
"Mulai"
hasil = 10 + 6 = 16
16
"Selesai"
```

Urutan: JS mendeklarasikan function `hitung` (hoisting), lalu baris 4 mendefinisikan `hasil` dengan memanggil `hitung(5)` = 10 dan `hitung(3)` = 6, lalu menjumlahkannya jadi 16.
</details>
