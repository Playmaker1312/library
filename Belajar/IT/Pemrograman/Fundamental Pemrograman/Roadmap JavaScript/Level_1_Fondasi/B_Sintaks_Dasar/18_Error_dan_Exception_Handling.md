# 🏠 18 — Error & Exception Handling: try, catch, finally, throw

---

## 1) Penjelasan

Error di JavaScript bisa terjadi karena kesalahan sintaks (`SyntaxError`), referensi variabel yang tidak ada (`ReferenceError`), tipe tidak cocok (`TypeError`), atau nilai di luar batas (`RangeError`).

**Exception handling** adalah mekanisme untuk menangkap error agar program tidak crash, tapi bisa memberikan respons yang baik.

| Konstruk | Fungsi |
|----------|--------|
| `try` | Bungkus kode yang berpotensi error. |
| `catch(error)` | Tangkap error dan tangani. |
| `finally` | Kode yang **selalu** dijalankan (sukses/gagal). |
| `throw` | Lemparkan error buatan sendiri (custom). |

---

## 2) Fungsi

- Mencegah aplikasi crash total.
- Memberi pesan error yang ramah pengguna.
- Validasi input yang melempar custom error.
- Logging error ke server.
- Membersihkan resource di `finally` (tutup file, koneksi DB).

---

## 3) Code

### Validator Input dengan Custom Error

```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = "ValidationError";
  }
}

function validasiUmur(umur) {
  if (typeof umur !== "number") {
    throw new TypeError("Umur harus berupa angka");
  }
  if (umur < 0) {
    throw new RangeError("Umur tidak boleh negatif");
  }
  if (umur < 17) {
    throw new ValidationError("Belum cukup umur untuk membangun rumah");
  }
  return "Umur valid, boleh kerja di proyek";
}

try {
  const result = validasiUmur(15);
  console.log(result);
} catch (err) {
  if (err instanceof ValidationError) {
    console.log("Validasi gagal:", err.message);
  } else if (err instanceof TypeError) {
    console.log("Tipe salah:", err.message);
  } else {
    console.log("Error tak dikenal:", err.message);
  }
} finally {
  console.log("Proses validasi selesai.");
}
```

### Contoh error bawaan

```javascript
try {
  // ReferenceError
  console.log(variabelTakAda);
} catch (e) {
  console.log(e.name); // ReferenceError
}

try {
  // TypeError
  null.toUpperCase();
} catch (e) {
  console.log(e.name); // TypeError
}
```

---

## 4) Analogi Rumah (Tabel + Narasi)

| Konsep JS | Analogi Membangun Rumah |
|-----------|------------------------|
| `try` | "Coba bor tembok ini." — kamu mulai tugas yang bisa gagal. |
| `catch` | "Jika bor macet, ganti mata bor." — tangani masalah. |
| `finally` | "Matikan bor, simpan alat, apa pun yang terjadi." — bersih-bersih. |
| `throw` | "Jika kabel putus, teriakkan 'BAHAYA!'" — lempar sinyal error. |
| `ValidationError` | Pelanggaran spesifik: "semen basi", "bata retak". |
| `TypeError` | Pakai palu untuk memotong kayu — alat salah tipe. |
| `ReferenceError` | Minta cetak biru yang tidak pernah dibuat. |
| `RangeError` | Minta paku sepanjang 1 meter — di luar batas wajar. |

### Narasi

Di proyek konstruksi, banyak hal bisa salah: bor listrik mati, paku habis, ukuran salah potong. Mandor yang baik tidak panik — dia **menangkap** masalah (`catch`), memperbaikinya atau ganti alat, lalu **tetap membersihkan** area kerja (`finally`). Kalau ada bahaya serius, dia **berteriak** (`throw`) agar semua orang berhenti.

Tanpa error handling, satu kesalahan kecil bisa menghentikan seluruh proyek. Sama seperti program tanpa `try/catch` yang langsung crash.

---

## 5) Use Case

- Validasi form login.
- Parsing JSON dari API.
- Koneksi database gagal.
- File tidak ditemukan.
- Semua kode yang bergantung pada input pengguna.

---

## 6) Kesalahan Umum

| Kesalahan | Contoh Salah | Benar |
|-----------|-------------|-------|
| Catch kosong | `catch (e) {}` — diam saja | minimal log error |
| Throw non-Error | `throw "error"` | `throw new Error("error")` |
| Try terlalu besar | bungkus seluruh program | try hanya di bagian rawan error |
| Lupa finally untuk cleanup | koneksi database tidak ditutup | cleanup di finally |
| Melempar error di catch tanpa rethrow | error tertelan padahal perlu naik | rethrow jika tidak bisa ditangani |

---

## 7) Benang Merah

- Materi 17 (Kode kompleks nested) → **Materi 18** (Error Handling) → Materi 19 (Readability)
- Semakin kompleks kode (nested loop, banyak logika), semakin besar potensi error. Error handling adalah **jaring pengaman**.
- Materi 19 akan mengajarkan cara menulis kode yang **rapi**, sehingga error handling lebih mudah dibaca.

---

## 8) Soal

### Soal 1 — try/catch dasar
Bungkus kode `JSON.parse("{invalid json}")` dalam try/catch dan cetak pesan error.

<details>
<summary>Jawaban</summary>

```javascript
try {
  JSON.parse("{invalid json}");
} catch (error) {
  console.log("JSON tidak valid:", error.message);
}
```
</details>

### Soal 2 — throw custom error
Buat fungsi `bagi(a, b)` yang melempar error jika `b === 0`.

<details>
<summary>Jawaban</summary>

```javascript
function bagi(a, b) {
  if (b === 0) {
    throw new Error("Pembagi tidak boleh nol");
  }
  return a / b;
}

try {
  console.log(bagi(10, 0));
} catch (e) {
  console.log(e.message);
}
```
</details>

### Soal 3 — finally
Buat fungsi yang mencoba membuka file (simulasi dengan console.log), lalu di finally catat "Operasi selesai" apa pun hasilnya.

<details>
<summary>Jawaban</summary>

```javascript
function bacaFile(nama) {
  try {
    if (nama !== "data.txt") {
      throw new Error("File tidak ditemukan");
    }
    console.log("Isi file: ...");
  } catch (e) {
    console.log(e.message);
  } finally {
    console.log("Operasi selesai");
  }
}

bacaFile("data.txt");
bacaFile("salah.txt");
```
</details>
