# Computational Thinking — Dekomposisi & Pattern Recognition

## Penjelasan

**Computational Thinking (CT)** adalah cara berpikir yang digunakan programmer untuk memecahkan masalah. CT bukan tentang bahasa pemrograman, tapi tentang **proses berpikir** sebelum menulis kode.

### 4 Pilar CT

| Pilar | Arti | Analogi Rumah |
|---|---|---|
| **Dekomposisi** | Memecah masalah besar menjadi bagian kecil | Memecah proyek rumah: pondasi, dinding, atap, listrik |
| **Pattern Recognition** | Melihat pola dan kemiripan | Setiap rumah punya pola: pintu, jendela — kamu tidak perlu mendesain ulang |
| **Abstraksi** | Menyembunyikan detail yang tidak penting | "Pasang jendela" — tidak perlu tahu cara membuat kaca |
| **Algorithm Design** | Membuat langkah-langkah solusi | SOP membangun rumah: langkah 1, 2, 3... |

---

## Fungsi

- **Memecah masalah kompleks** menjadi potongan yang bisa ditangani
- **Mengenali pola** dari masalah yang pernah diselesaikan sebelumnya — tidak perlu reinvent the wheel
- **Berpikir terstruktur** sebelum coding — mengurangi error dan revisi

---

## Cara Implementasi / Code

### Contoh: Membuat program tebak angka

**Dekomposisi:**

```
Masalah: Buat program tebak angka
├── Bagian 1: Generate angka acak
├── Bagian 2: Minta input dari pemain
├── Bagian 3: Bandingkan tebakan dengan angka rahasia
├── Bagian 4: Beri petunjuk (terlalu besar / terlalu kecil)
└── Bagian 5: Ulangi sampai tebakan benar
```

**Pattern Recognition:** Pola "minta input → bandingkan → ulangi" sama seperti form login, verifikasi OTP, kuis.

**Abstraksi:** "Generate angka acak" — kita tidak perlu tahu algoritma random. Cukup panggil `Math.random()`.

**Algorithm Design (Pseudocode):**

```
1. Generate angka rahasia (1-100)
2. Tampilkan "Tebak angka (1-100): "
3. Baca input pemain
4. Jika tebakan == angka rahasia → tampilkan "Benar!" → selesai
5. Jika tebakan > angka rahasia → tampilkan "Terlalu besar" → ke langkah 2
6. Jika tebakan < angka rahasia → tampilkan "Terlalu kecil" → ke langkah 2
```

---

## Analogi (Membangun Rumah)

| Pilar CT | Analogi Rumah |
|---|---|
| **Dekomposisi** | Membangun rumah dipecah jadi: **pondasi → rangka → dinding → atap → listrik → finishing** |
| **Pattern Recognition** | Semua rumah punya pola **pintu, jendela, kamar tidur** — kamu tidak mendesain ulang setiap kali |
| **Abstraksi** | "Pasang instalasi listrik" — kamu tidak perlu tahu cara menambang tembaga atau membuat kabel |
| **Algorithm Design** | **SOP (Standard Operating Procedure)** — langkah pasti: 1) cor pondasi, 2) pasang rangka, 3) pasang dinding... |

**Narasi:** Membangun rumah adalah proyek raksasa. Arsitek **memecah** (dekomposisi) menjadi bagian-bagian: pondasi, rangka, dinding, atap. Tukang **mengenali pola** (pattern recognition) bahwa setiap kamar perlu pintu dan jendela — tidak perlu desain ulang. Arsitek **mengabstraksi** detail: "pasang sanitasi" tanpa perlu tahu cara membuat pipa PVC. Semua mengikuti **SOP** (algorithm design) yang sudah ditetapkan. Computational thinking adalah **proses berpikir** arsitek dan mandor sebelum pembangunan dimulai.

---

## Dipakai Untuk Apa

- **Proyek besar:** Netflix tidak dibuat dalam satu file raksasa — dipecah jadi microservices
- **Bug fixing:** Dekomposisi error → isolasi bagian yang salah
- **Code review:** Melihat pola apakah kode mengulang hal yang sama (pattern recognition → bikin function)
- **System design:** Abstraksi memungkinkan kita mendesain sistem tanpa tenggelam di detail

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Langsung coding tanpa dekomposisi | "Buat program tebak angka" langsung nulis `Math.random()` | Bingung di tengah, kode berantakan |
| Tidak mengenali pola | Menulis ulang kode sorting padahal sudah ada `.sort()` | Buang waktu, kode rentan bug |
| Abstraksi terlalu dalam | "Buka file" → abstraksi sampai ke sektor hard disk | Jadi pusing, tidak fokus |
| Algoritma loncat-loncat | Langsung ke detail sebelum pondasi selesai | Pondasi salah, seluruh rumah rubuh |

---

## Benang Merah

- **Materi 4 (Menyiapkan Environment):** Alat sudah siap. Sekarang kita belajar *cara berpikir* sebelum menggunakannya.
- **Materi 6 (Abstraksi & Pseudocode):** CT adalah *apa yang dipikirkan*, pseudocode adalah *bagaimana menuliskannya*.

---

## Soal Latihan + Jawaban

### Soal 1 (Mudah)
Sebutkan 4 pilar Computational Thinking dan jelaskan masing-masing dalam 1 kalimat.

<details>
<summary>Jawaban</summary>

1. **Dekomposisi** — memecah masalah besar menjadi bagian kecil
2. **Pattern Recognition** — melihat pola dan kemiripan dari masalah
3. **Abstraksi** — menyembunyikan detail kompleks, fokus pada yang penting
4. **Algorithm Design** — membuat langkah-langkah solusi yang terurut
</details>

### Soal 2 (Sedang)
Lakukan dekomposisi untuk masalah: **"Membuat kopi"**. Pecah menjadi minimal 4 langkah.

<details>
<summary>Jawaban</summary>

1. Siapkan alat: cangkir, sendok, gelas, kopi, gula, air panas
2. Masukkan kopi dan gula ke cangkir
3. Tuang air panas, aduk
4. Sajikan

_Variasi: bisa ditambah langkah "panaskan air" jika belum ada._
</details>

### Soal 3 (Tantangan)
Masalah: **"Buat program kalkulator sederhana (+, -, ×, ÷)".**

Lakukan dekomposisi (pecah jadi sub-masalah), lalu identifikasi:
- Pola apa yang bisa dikenali?
- Apa yang bisa diabstraksi?
- Buat algoritma dalam pseudocode

<details>
<summary>Jawaban</summary>

**Dekomposisi:**
1. Tampilkan menu operasi
2. Minta input angka pertama
3. Minta input operator (+, -, *, /)
4. Minta input angka kedua
5. Lakukan perhitungan sesuai operator
6. Tampilkan hasil

**Pattern Recognition:** Pola "minta input → proses → tampilkan" mirip dengan tebak angka, login, dan banyak program lain. Operasi matematika mengikuti pola yang sama: dua angka + satu operator.

**Abstraksi:** Perhitungan matematika bisa diabstraksi — kita tidak perlu tahu bagaimana CPU menjumlahkan. Cukup gunakan `+`, `-`, dst.

**Algorithm Design:**
```
1. Tampilkan "Pilih operasi: 1(+) 2(-) 3(*) 4(/)"
2. Baca pilihan
3. Tampilkan "Masukkan angka pertama: " → baca a
4. Tampilkan "Masukkan angka kedua: " → baca b
5. Jika pilihan == 1 → hasil = a + b
6. Jika pilihan == 2 → hasil = a - b
7. Jika pilihan == 3 → hasil = a * b
8. Jika pilihan == 4 → hasil = a / b (jika b ≠ 0)
9. Tampilkan "Hasil: " + hasil
```
</details>
