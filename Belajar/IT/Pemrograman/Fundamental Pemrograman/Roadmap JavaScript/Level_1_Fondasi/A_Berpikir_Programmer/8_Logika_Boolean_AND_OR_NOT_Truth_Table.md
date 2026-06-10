# Logika Boolean — AND, OR, NOT & Truth Table

## Penjelasan

**Logika Boolean** adalah fondasi semua keputusan dalam pemrograman. Dinamai dari George Boole, matematikawan abad 19. Nilai boolean hanya dua: **`true`** atau **`false`**.

### Operator Boolean

| Operator | Simbol JS | Arti | Contoh | Hasil |
|---|---|---|---|---|
| **AND** | `&&` | Semua harus `true` | `true && true` | `true` |
| **OR** | `\|\|` | Salah satu `true` | `true \|\| false` | `true` |
| **NOT** | `!` | Membalikkan nilai | `!true` | `false` |

### Truth Table

| A | B | A && B | A \|\| B | !A |
|---|---|---|---|---|
| `true` | `true` | `true` | `true` | `false` |
| `true` | `false` | `false` | `true` | `false` |
| `false` | `true` | `false` | `true` | `true` |
| `false` | `false` | `false` | `false` | `true` |

### Short-circuit Evaluation

JavaScript **berhenti mengevaluasi** begitu hasil sudah pasti:

- `false && (apapun)` → langsung `false` (tanpa evaluasi bagian kanan)
- `true || (apapun)` → langsung `true` (tanpa evaluasi bagian kanan)

```javascript
console.log(false && console.log("Ini tidak dijalankan")); // false
console.log(true || console.log("Ini juga tidak dijalankan")); // true
```

---

## Fungsi

- **Membuat keputusan** dalam kode — syarat dalam `if`, loop, guard clause
- **Menggabungkan kondisi** — "Jika user login **DAN** admin"
- **Validasi input** — "Umur harus > 0 **DAN** < 150"
- **Default values** — `let nama = input || "Tamu"` (OR untuk fallback)

---

## Cara Implementasi / Code

```javascript
// AND (&&) — semua harus true
let umur = 20;
let punyaSIM = true;

if (umur >= 17 && punyaSIM) {
    console.log("Boleh mengemudi"); // Ini yang dieksekusi
} else {
    console.log("Tidak boleh mengemudi");
}

// OR (||) — salah satu true
let cash = 0;
let kartuKredit = 50000;

if (cash > 0 || kartuKredit > 0) {
    console.log("Bisa membayar"); // kartuKredit > 0 → true
} else {
    console.log("Tidak bisa membayar");
}

// NOT (!) — membalikkan
let hujan = true;
if (!hujan) {
    console.log("Boleh keluar");
} else {
    console.log("Bawa payung"); // Ini yang dieksekusi
}

// Kombinasi
let masuk = true;
let sudahBayar = false;
console.log(masuk && sudahBayar); // false
console.log(masuk || sudahBayar); // true
console.log(!masuk);              // false
```

---

## Analogi (Membangun Rumah)

| Konsep | Analogi Rumah |
|---|---|
| **Boolean (`true`/`false`)** | **Saklar listrik** — ON (`true`) atau OFF (`false`). Tidak ada di antaranya. |
| **AND (`&&`)** | **Saklar seri** — dua saklar harus ON agar lampu menyala. Satu OFF, lampu mati. |
| **OR (`\|\|`)** | **Saklar paralel** — satu saklar ON saja sudah cukup menyalakan lampu. |
| **NOT (`!`)** | **Saklar pembalik** — jika ON jadi OFF, jika OFF jadi ON. |
| **Short-circuit** | **Sensor pintu** — jika pintu sudah terbuka (true), sensor tidak perlu cek kunci. |

**Narasi:** Di rumah, **saklar** adalah perangkat boolean sederhana — ON (true) atau OFF (false). **AND** seperti dua saklar seri di tangga: kamu harus nyalakan dua-duanya biar lampu terang. **OR** seperti saklar paralel di kamar tidur: nyalakan saklar dekat pintu **ATAU** dekat tempat tidur, lampu tetap menyala. **NOT** seperti saklar pembalik: kalau tombol ditekan, lampu mati; ditekan lagi, lampu nyala. **Short-circuit** seperti sensor pintu otomatis: jika pintu sudah terbuka (true), sensor tidak perlu repot mengecek apakah kunci masih terpasang.

---

## Dipakai Untuk Apa

- **Validasi form:** `if (nama !== "" && email.includes("@"))` — semua syarat harus dipenuhi
- **Otorisasi:** `if (isAdmin || isOwner)` — salah satu role diizinkan
- **Guard clause:** `if (!data) return` — hentikan fungsi lebih awal
- **Default value:** `let nama = input || "Default"` — pakai default jika input falsy
- **Toggle:** `menyala = !menyala` — setiap panggilan membalik status

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Salah operator (`&&` vs `\|\|`) | `if (umur >= 17 \|\| punyaSIM)` padahal mau keduanya | Orang 15 tahun tanpa SIM lolos |
| Lupa tanda kurung | `if (a > 5 && b < 10 || c == 0)` | Prioritas operator tidak sesuai |
| `=` vs `==` vs `===` | `if (x = 5)` — assignment, bukan perbandingan | Selalu true (kecuali x = 0) |
| Salah paham falsy values | `if (0 || "" || null)` - semuanya false | Kondisi tidak berjalan seperti diharapkan |
| Short-circuit jebakan | `let x = y && y.umur` — jika y null, x = null | Tidak error, tapi x jadi null |

---

## Benang Merah

- **Materi 7 (Flowchart):** Setiap **decision** (◇) di flowchart adalah ekspresi boolean — hasil `true` ke satu cabang, `false` ke cabang lain.
- **Materi 9 (Variabel):** Nilai boolean bisa disimpan di variabel. Ini jembatan ke materi selanjutnya tentang tipe data dan variabel.

---

## Soal Latihan + Jawaban

### Soal 1 (Mudah)
Isi tabel kebenaran berikut dengan `true` atau `false`:

| A | B | A \|\| B | A && B | !A && B |
|---|---|---|---|---|
| `true` | `false` | ? | ? | ? |

<details>
<summary>Jawaban</summary>

| A | B | A \|\| B | A && B | !A && B |
|---|---|---|---|---|
| `true` | `false` | `true` | `false` | `false` |

Penjelasan:
- `true || false` = `true` (OR: salah satu true)
- `true && false` = `false` (AND: harus semua true)
- `!true && false` = `false && false` = `false`
</details>

### Soal 2 (Sedang)
Apa output dari kode berikut? **Jawab tanpa menjalankan.**

```javascript
let a = 10;
let b = 20;
let c = 15;

console.log(a > b && b > c);    // 1
console.log(a < b || c > b);    // 2
console.log(!(a > c));          // 3
console.log((a + b) > c && c);  // 4
```

<details>
<summary>Jawaban</summary>

```javascript
// 1: false && true → false
// 2: true || false → true  
// 3: !(false) → true
// 4: (30 > 15) && 15 → true && 15 → 15 (karena && mempertahankan last truthy value)
```

Output:
```
false
true
true
15
```
</details>

### Soal 3 (Tantangan)
Seorang developer menulis kode berikut. Ada **3 bug logika**. Temukan dan perbaiki!

```javascript
let usia = 25;
let pelajar = false;
let saldo = 100000;

// Cek diskon: usia < 12 ATAU pelajar ATAU saldo < 50000
if (usia < 12 && pelajar && saldo < 50000) {
    console.log("Dapat diskon");
}

// Cek akses: usia >= 17 DAN bukan pelajar ATAU saldo > 50000
if (usia >= 17 && !pelajar && saldo > 50000) {
    console.log("Akses penuh");
}

// Toggle status login
let login = false;
login = login && true;  // Maksudnya: toggle login
console.log(login);
```

<details>
<summary>Jawaban</summary>

**Bug 1:** Operator `&&` seharusnya `||` — diskon diberikan jika **salah satu** kondisi terpenuhi, bukan semua.
```javascript
if (usia < 12 || pelajar || saldo < 50000) {
    console.log("Dapat diskon");
}
```

**Bug 2:** Operator `&&` sebelum `||` menyebabkan prioritas salah. Maksudnya: (usia >= 17 DAN bukan pelajar) ATAU (saldo > 50000). Karena `&&` lebih tinggi dari `||`, kode asli sudah benar secara prioritas — tapi bisa dikasih kurung untuk kejelasan.
```javascript
if ((usia >= 17 && !pelajar) || saldo > 50000) {
    console.log("Akses penuh");
}
```

**Bug 3:** `login && true` menghasilkan `false && true = false` — tidak toggle. Harusnya `!`
```javascript
login = !login;  // Toggle: false → true, true → false
console.log(login); // true
```
</details>
