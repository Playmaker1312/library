# 🏠 15 — Percabangan: if, else if, else & switch

---

## 1) Penjelasan

Percabangan adalah mekanisme untuk mengambil keputusan berdasarkan kondisi. Jika kondisi terpenuhi (`true`), satu blok kode dijalankan; jika tidak, blok lain yang dijalankan.

- **`if`** — blok utama, dijalankan jika kondisi `true`.
- **`else if`** — kondisi tambahan jika `if` gagal.
- **`else`** — blok cadangan jika semua kondisi di atas `false`.
- **`switch`** — alternatif jika kita membandingkan satu nilai dengan banyak kemungkinan.
- **Short-circuit evaluation** — memanfaatkan `&&` dan `||` untuk menulis percabangan satu baris.

---

## 2) Fungsi

| Konstruk | Fungsi |
|----------|--------|
| `if (kondisi)` | Eksekusi jika kondisi truthy |
| `else if (kondisi)` | Eksekusi jika kondisi sebelumnya false dan kondisi ini true |
| `else` | Eksekusi jika semua kondisi false |
| `switch (expr)` | Seleksi berdasarkan nilai ekspresi |
| `&&`, `\|\|` | Short-circuit: evaluasi berhenti jika hasil sudah pasti |

---

## 3) Code

### Grade Calculator

```javascript
const readline = require("readline");

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

rl.question("Masukkan nilai (0-100): ", (input) => {
  const nilai = Number(input);

  if (isNaN(nilai) || nilai < 0 || nilai > 100) {
    console.log("Nilai tidak valid.");
  } else if (nilai >= 90) {
    console.log("Grade: A");
  } else if (nilai >= 80) {
    console.log("Grade: B");
  } else if (nilai >= 70) {
    console.log("Grade: C");
  } else if (nilai >= 60) {
    console.log("Grade: D");
  } else {
    console.log("Grade: E");
  }

  rl.close();
});
```

### Short-circuit (one-liner)

```javascript
const nama = inputUser || "Tamu";
const bolehMasuk = umur >= 17 && !diblokir;
```

### switch

```javascript
switch (hari) {
  case "Senin":
    console.log("Mulai kerja");
    break;
  case "Sabtu":
  case "Minggu":
    console.log("Libur");
    break;
  default:
    console.log("Hari biasa");
}
```

---

## 4) Analogi Rumah (Tabel + Narasi)

| Konsep JS | Analogi Membangun Rumah |
|-----------|------------------------|
| `if` | "Apakah pondasi sudah kering?" — jika ya, lanjut pasang bata. |
| `else if` | "Kalau belum kering, apakah cuaca cerah?" — cek alternatif. |
| `else` | "Kalau tidak ada yang terpenuhi, istirahat dulu." — tindakan default. |
| `switch` | Papan nama ruangan: jika "Kamar 1" → tidur, "Dapur" → masak. |
| Short-circuit `\|\|` | "Ambil palu, kalau tidak ada pakai batu." — pakai nilai alternatif. |
| Short-circuit `&&` | "Kalau listrik menyala **dan** ada bor, baru bor tembok." — aman. |

### Narasi

Kamu seorang mandor. Setiap hari kamu mengambil keputusan: **"Jika** semen sudah datang, aduk; **kalau tidak**, cek apakah bata cukup untuk kerja lain; **kalau tidak ada sama sekali**, suruh pekerja istirahat." Tanpa percabangan, rumah tidak akan pernah selesai karena kamu tidak bisa menyesuaikan tindakan dengan situasi.

---

## 5) Use Case

- Validasi form input (if email kosong → error).
- Menentukan role user (admin/user/guest).
- Logika jam buka toko (switch day).
- Menampilkan pesan error bertingkat.
- Memberi nilai default dengan `||`.

---

## 6) Kesalahan Umum

| Kesalahan | Contoh Salah | Benar |
|-----------|-------------|-------|
| Pakai `=` bukan `==`/`===` | `if (x = 5)` | `if (x === 5)` |
| Lupa `break` di switch | nilai masuk ke case lain | tambah `break` |
| Urutan elseif salah | `nilai>=60` sebelum `nilai>=70` | urutkan dari besar ke kecil |
| Percabangan terlalu dalam | nested if 5 level | gunakan early return |
| Salah paham truthy/falsy | `if ("")` dianggap true padahal falsy | pahami falsy: `""`, `0`, `null`, `undefined`, `NaN`, `false` |

---

## 7) Benang Merah

- Materi 14 (I/O) + Materi 8 (Boolean) → **Materi 15** (Percabangan) → Materi 16 (Loop)
- Boolean dari Materi 8 adalah fondasi kondisi `if`. I/O dari Materi 14 memberi data yang perlu diuji.
- **Logika keputusan ini** akan dipakai terus: di loop (Materi 16), nested loop (Materi 17), dan error handling (Materi 18).

---

## 8) Soal

### Soal 1 — if/else
Buat program yang menerima angka dan mencetak "Genap" jika habis dibagi 2, "Ganjil" jika tidak.

<details>
<summary>Jawaban</summary>

```javascript
const angka = Number(prompt("Masukkan angka:"));
if (angka % 2 === 0) {
  console.log("Genap");
} else {
  console.log("Ganjil");
}
```
</details>

### Soal 2 — switch
Buat switch-case yang menerima kode warna lampu lalu lintas (`"merah"`, `"kuning"`, `"hijau"`) dan cetak aksinya.

<details>
<summary>Jawaban</summary>

```javascript
switch (warna) {
  case "merah":
    console.log("Berhenti");
    break;
  case "kuning":
    console.log("Siap-siap");
    break;
  case "hijau":
    console.log("Jalan");
    break;
  default:
    console.log("Warna tidak dikenal");
}
```
</details>

### Soal 3 — Short-circuit
Buat fungsi `sapa(nama)` yang mencetak `"Halo, {nama}"` jika nama diberikan, atau `"Halo, Tamu"` jika tidak.

<details>
<summary>Jawaban</summary>

```javascript
function sapa(nama) {
  const panggilan = nama || "Tamu";
  console.log("Halo, " + panggilan);
}
```
</details>
