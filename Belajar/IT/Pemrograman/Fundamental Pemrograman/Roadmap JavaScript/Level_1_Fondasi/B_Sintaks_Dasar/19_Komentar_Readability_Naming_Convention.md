# 🏠 19 — Komentar, Code Readability & Naming Convention

---

## 1) Penjelasan

Kode ditulis untuk **manusia**, bukan hanya untuk mesin. Kode yang baik adalah kode yang bisa dibaca dan dipahami oleh orang lain (atau dirimu sendiri 6 bulan kemudian).

| Aspek | Tujuan |
|-------|--------|
| **Komentar** | Menjelaskan **mengapa**, bukan **apa**. Kode yang baik sudah menjelaskan **apa**. |
| **Readability** | Format, spasi, indentasi, panjang baris — membuat kode enak dibaca. |
| **Naming Convention** | Aturan penamaan: `camelCase`, `PascalCase`, `UPPER_CASE`. |

### Aturan Emas Komentar

> "Kode yang baik adalah dokumentasi yang paling baik." — Robert C. Martin

Tulis komentar hanya jika kode tidak bisa menjelaskan diri sendiri.

---

## 2) Fungsi

- **Komentar** — menjelaskan alasan keputusan bisnis, workaround bug, atau konteks non-trivial.
- **Readability** — mengurangi biaya maintenance, mempercepat code review.
- **Naming Convention** — konsistensi, memudahkan pencarian dan refactoring.

---

## 3) Code

### ❌ Sebelum — kode berantakan

```javascript
function a(b,c){let d=0;for(let e=0;e<b.length;e++){for(let f=0;f<c;f++){d+=b[e]}}return d}
```

### ✅ Sesudah — readable & proper naming

```javascript
function hitungTotalNilai(nilaiSiswa, jumlahHari) {
  let total = 0;
  for (let i = 0; i < nilaiSiswa.length; i++) {
    for (let j = 0; j < jumlahHari; j++) {
      total += nilaiSiswa[i];
    }
  }
  return total;
}
```

### Komentar yang baik vs buruk

```javascript
// ❌ Komentar buruk: menjelaskan yang sudah jelas
let x = 5; // set x ke 5

// ✅ Komentar baik: menjelaskan alasan
// Pake Math.floor karena JS pake floating point,
// dan kita butuh integer untuk indeks array
const indeks = Math.floor(nilai / 10);
```

### Naming Convention

```javascript
// camelCase — variabel, fungsi
const namaLengkap = "Budi";
function hitungLuas() {}

// PascalCase — class, constructor
class Rumah {
  constructor(luas) {
    this.luasTanah = luas;
  }
}

// UPPER_CASE — konstanta (nilai tetap)
const PI = 3.14159;
const MAX_SISWA = 30;
```

---

## 4) Analogi Rumah (Tabel + Narasi)

| Konsep JS | Analogi Membangun Rumah |
|-----------|------------------------|
| Kode berantakan | Rumah tanpa cetak biru, kabel berantakan, pipa asal-asalan. |
| Kode readable | Rumah dengan instalasi rapi, kabel diurut, pipa diberi label. |
| Naming convention | Setiap ruangan diberi papan nama: "Kamar Mandi", "Dapur" — bukan "Ruangan A". |
| Komentar | Stiker di panel listrik: "Hati-hati, 220V" — bukan stiker "Ini pintu" di pintu. |
| camelCase | `lebarRumah` — kata pertama lowercase, sisanya uppercase di awal kata. |
| PascalCase | `class RumahMewah` — semua kata diawali huruf besar. |
| UPPER_CASE | `MAKS_LANTAI = 2` — nilai tetap yang tidak berubah. |

### Narasi

Bayangkan masuk ke rumah yang sedang dibangun. Kabel listrik tidak diberi label, pipa air tidak dicat, tidak ada cetak biru. Mandor bilang: "Ya, saya tahu maksudnya." Itulah **kode berantakan**.

Sekarang bayangkan rumah dengan **setiap kabel diberi label** (naming), **pipa ditata rapi** (readability), dan **stiker peringatan di tempat berbahaya** (komentar). Itulah kode berkualitas. Orang lain bisa masuk dan langsung paham tanpa harus bertanya ke mandor.

---

## 5) Use Case

- *Code review* — tim bisa memahami PR dengan cepat.
- *Debugging* — mencari bug lebih mudah.
- *Onboarding* — anggota baru cepat produktif.
- *Refactoring* — berani ubah kode karena tahu efeknya.
- *Open source* — kontributor dari seluruh dunia perlu kode yang konsisten.

---

## 6) Kesalahan Umum

| Kesalahan | Contoh | Solusi |
|-----------|--------|--------|
| Komentar basi | `i++ // increment i` | Hapus komentar |
| Nama tidak deskriptif | `let a = 5` | `let jumlahSiswa = 5` |
| Inkonsisten gaya | campur camelCase dan snake_case | Pilih satu, patuhi |
| Baris terlalu panjang | 200 karakter per baris | Maks 80–100 karakter |
| Tidak pakai spasi | `if(x>5){y++}` | `if (x > 5) { y++ }` |
| Komentar kode mati | `// const lama = 5;` | Hapus, jangan dikomentari |

---

## 7) Benang Merah

- Materi 18 (Error Handling) → **Materi 19** (Readability) → **PENUTUP Level 1**
- Error handling membuat kode **tidak crash**. Readability membuat kode **tidak menyiksa** untuk dibaca.
- **Semua materi sebelumnya** (variabel, tipe data, operator, percabangan, loop, error) sekarang harus ditulis dengan **rapi**.
- Ini adalah **penutup Level 1**: Fondasi yang kuat, dibangun dengan cetak biru yang jelas.

---

## 8) Soal

### Soal 1 — Refactor kode
Refactor kode ini:

```javascript
function c(b){let h=0;for(let i=0;i<b.length;i++){h+=b[i]}return h/b.length}
```

<details>
<summary>Jawaban</summary>

```javascript
function hitungRataRata(angka) {
  let total = 0;
  for (let i = 0; i < angka.length; i++) {
    total += angka[i];
  }
  return total / angka.length;
}
```
</details>

### Soal 2 — Perbaiki komentar
```javascript
const diskon = 0.1; // diskon 10%
```

<details>
<summary>Jawaban</summary>
Komentar tidak perlu karena kode sudah jelas. Hapus saja:

```javascript
const diskon = 0.1;
```

Atau jika ingin konteks:

```javascript
// Diskon 10% untuk pelanggan baru
const diskon = 0.1;
```
</details>

### Soal 3 — Perbaiki naming
```javascript
const a = ["Budi", "Siti"];
for (let i = 0; i < a.length; i++) {
  console.log("Halo, " + a[i]);
}
```

<details>
<summary>Jawaban</summary>

```javascript
const namaSiswa = ["Budi", "Siti"];
for (let i = 0; i < namaSiswa.length; i++) {
  console.log("Halo, " + namaSiswa[i]);
}
```
</details>
