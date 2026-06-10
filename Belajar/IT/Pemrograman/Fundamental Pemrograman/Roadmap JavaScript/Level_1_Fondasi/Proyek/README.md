# 🏗️ Proyek Level 1: CLI Personal Assistant

**Gunakan semua yang dipelajari dari Materi 1-19.**

---

## Deskripsi

Program CLI (Command Line Interface) yang berfungsi sebagai asisten pribadi. Program berjalan terus sampai user memilih untuk keluar.

## Fitur

| Fitur | Konsep yang Dilatih |
|---|---|
| Menyapa user dengan nama | Variabel, string, input/output |
| Kalkulator (+, -, *, /) | Operator, if/else, fungsi |
| Konversi suhu (C ↔ F ↔ K) | Operator, return value |
| Cek bilangan prima | Loop, percabangan, logika boolean |
| Loop menu utama | while loop, do-while |

### Contoh Output
```
=== PERSONAL ASSISTANT ===
Masukkan nama Anda: Budi
Halo Budi! Ada yang bisa saya bantu?

1. Kalkulator
2. Konversi Suhu
3. Cek Bilangan Prima
4. Keluar
Pilih menu (1-4): 1

--- KALKULATOR ---
Angka 1: 10
Angka 2: 5
Operasi (+, -, *, /): +
Hasil: 10 + 5 = 15

Tekan Enter untuk kembali ke menu...
```

---

## Kode Solusi

```javascript
const readline = require('readline');

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

function tanya(pertanyaan) {
  return new Promise(resolve => rl.question(pertanyaan, resolve));
}

function kalkulator(a, b, operasi) {
  switch (operasi) {
    case '+': return a + b;
    case '-': return a - b;
    case '*': return a * b;
    case '/': return b !== 0 ? a / b : "Error: pembagi 0";
    default: return "Operasi tidak valid";
  }
}

function konversiSuhu(nilai, dari, ke) {
  let celsius;
  if (dari === 'C') celsius = nilai;
  else if (dari === 'F') celsius = (nilai - 32) * 5 / 9;
  else if (dari === 'K') celsius = nilai - 273.15;
  else return "Skala tidak valid";

  if (ke === 'C') return celsius;
  if (ke === 'F') return celsius * 9 / 5 + 32;
  if (ke === 'K') return celsius + 273.15;
  return "Skala tidak valid";
}

function cekPrima(angka) {
  if (angka < 2) return false;
  for (let i = 2; i <= Math.sqrt(angka); i++) {
    if (angka % i === 0) return false;
  }
  return true;
}

async function main() {
  const nama = await tanya("Masukkan nama Anda: ");
  console.log(`Halo ${nama}! Ada yang bisa saya bantu?\n`);

  let menu = true;
  while (menu) {
    console.log("\n1. Kalkulator\n2. Konversi Suhu\n3. Cek Bilangan Prima\n4. Keluar");
    const pilihan = await tanya("Pilih menu (1-4): ");

    switch (pilihan) {
      case '1': {
        const a = parseFloat(await tanya("Angka 1: "));
        const b = parseFloat(await tanya("Angka 2: "));
        const op = await tanya("Operasi (+, -, *, /): ");
        console.log(`Hasil: ${a} ${op} ${b} = ${kalkulator(a, b, op)}`);
        break;
      }
      case '2': {
        const nilai = parseFloat(await tanya("Nilai suhu: "));
        const dari = (await tanya("Skala asal (C/F/K): ")).toUpperCase();
        const ke = (await tanya("Skala tujuan (C/F/K): ")).toUpperCase();
        const hasil = konversiSuhu(nilai, dari, ke);
        console.log(`${nilai}°${dari} = ${hasil}°${ke}`);
        break;
      }
      case '3': {
        const angka = parseInt(await tanya("Masukkan angka: "));
        const prima = cekPrima(angka);
        console.log(`${angka} ${prima ? "adalah" : "BUKAN"} bilangan prima`);
        break;
      }
      case '4': {
        console.log(`Sampai jumpa ${nama}!`);
        menu = false;
        break;
      }
      default:
        console.log("Pilihan tidak valid, coba lagi.");
    }

    if (pilihan !== '4') {
      await tanya("Tekan Enter untuk kembali ke menu...");
    }
  }

  rl.close();
}

main().catch(console.error);
```

---

## Yang Dipelajari dari Proyek Ini

| Materi | Implementasi di Proyek |
|---|---|
| Variabel (Materi 9) | `nama`, `pilihan`, `a`, `b` — semua variabel |
| Tipe Data (Materi 10) | string, number, boolean |
| Type Coercion (Materi 11) | `parseFloat()` — explicit conversion |
| Operator (Materi 12) | Aritmatika, perbandingan, assignment |
| String (Materi 13) | Template literals, concatenation |
| I/O (Materi 14) | `readline`, `console.log` |
| Percabangan (Materi 15) | `switch`, `if/else`, ternary |
| Loop (Materi 16) | `while` loop untuk menu |
| Fungsi (Materi 32) | `kalkulator`, `konversiSuhu`, `cekPrima` |
| Error Handling (Materi 18) | Pembagi 0, input tidak valid |

---

## Latihan Tambahan

1. **Tambah fitur** `hitung faktorial` (nomor 5 di menu)
2. **Tambah fitur** `generate password acak` (nomor 6)
3. **Ganti readline** dengan package `inquirer` dari npm
4. **Simpan history** perhitungan ke array, tampilkan dengan menu "Riwayat"

**Selamat! Anda telah menyelesaikan Level 1. 🎉**
