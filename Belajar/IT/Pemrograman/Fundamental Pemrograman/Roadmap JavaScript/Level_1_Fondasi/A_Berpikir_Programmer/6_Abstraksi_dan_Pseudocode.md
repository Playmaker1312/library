# Abstraksi & Pseudocode

## Penjelasan

**Abstraksi** adalah menyembunyikan detail kompleks dan hanya menampilkan informasi yang penting. **Pseudocode** adalah cara menulis algoritma menggunakan bahasa manusia yang terstruktur — jembatan antara berpikir dan coding.

### Abstraksi

Tanpa abstraksi, untuk "menyalakan lampu" kamu harus: *alirkan listrik dari PLN ke meteran → meteran ke MCB → MCB ke saklar → saklar ke kabel → kabel ke fitting → fitting ke bohlam → bohlam menyala*. Dengan abstraksi: **"tekan saklar"**.

Dalam kode, abstraksi berarti **membuat fungsi** — sembunyikan detail eksekusi, ekspos kegunaan.

### Pseudocode

Pseudocode bukan bahasa pemrograman. Tidak ada aturan ketat. Tujuan: **komunikasi ide** — ke diri sendiri, ke programmer lain, atau ke non-teknis.

---

## Fungsi

- **Menyederhanakan kompleksitas** — kamu tidak perlu tahu cara kerja `console.log()` untuk menggunakannya
- **Merencanakan solusi** sebelum coding — mengurangi risiko salah arah
- **Komunikasi** dengan tim — tidak semua orang paham JavaScript, tapi semua paham bahasa manusia
- **Dokumentasi** — pseudocode jadi panduan saat implementasi

---

## Cara Implementasi / Code

### Contoh 1: Mencari nilai terbesar dari 3 angka

**Pseudocode:**

```
INPUT a, b, c
IF a > b AND a > c THEN
    OUTPUT a
ELSE IF b > a AND b > c THEN
    OUTPUT b
ELSE
    OUTPUT c
END IF
```

**Terjemahan ke JavaScript:**

```javascript
function nilaiTerbesar(a, b, c) {
    if (a > b && a > c) {
        return a;
    } else if (b > a && b > c) {
        return b;
    } else {
        return c;
    }
}

console.log(nilaiTerbesar(10, 25, 7)); // Output: 25
```

### Contoh 2: Cek apakah angka genap

**Pseudocode:**

```
INPUT angka
IF angka dibagi 2 sisa 0 THEN
    OUTPUT "Genap"
ELSE
    OUTPUT "Ganjil"
END IF
```

**JavaScript:**

```javascript
function cekGenap(angka) {
    if (angka % 2 === 0) {
        return "Genap";
    } else {
        return "Ganjil";
    }
}

console.log(cekGenap(7)); // Output: Ganjil
```

---

## Analogi (Membangun Rumah)

| Konsep | Analogi Rumah |
|---|---|
| **Abstraksi** | "Pasang jendela" — kamu tidak perlu tahu cara membuat kaca, memotong aluminium, atau merakit engsel. Kamu cukup pasang jendela yang sudah jadi. |
| **Fungsi** | **Mesin pabrik** — masukkan bahan baku (input), dapatkan hasil jadi (output). Detail di dalam mesin tersembunyi. |
| **Pseudocode** | **Sketsa kasar denah** — tidak serapi cetak biru (kode), tapi cukup jelas untuk tukang paham maksud arsitek. |
| **Detail kode** | **Cetak biru detail** — ukuran setiap baut, spesifikasi material. |

**Narasi:** Arsitek memberi sketsa kasar (pseudocode): "Kamar tidur utama di sini, jendela di sini, pintu menghadap sini." Tukang paham maksudnya tanpa harus membaca cetak biru detail. Tukang menggunakan **mesin pabrik** (fungsi) — ia masukkan kayu, keluar pintu jadi. Ia tidak perlu tahu cara kerja mesin itu. Abstraksi memungkinkan arsitek berpikir tentang *tata ruang*, bukan tentang *jenis paku*.

---

## Dipakai Untuk Apa

- **Perencanaan fitur:** Tulis pseudocode sebelum coding — mencegah kesalahan arsitektur
- **Wawancara kerja:** Pseudocode adalah cara standar menjelaskan algoritma di papan tulis
- **Kolaborasi:** Developer senior buat pseudocode, junior implementasikan ke kode
- **Logika kompleks:** Diagram alur + pseudocode = dokumen teknis yang kuat

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Pseudocode terlalu detail | Menulis `let x = 5;` — itu sudah kode! | Kehilangan tujuan pseudocode (abstraksi) |
| Pseudocode terlalu abstrak | "Urutkan data" tanpa menjelaskan bagaimana | Implementasi bisa salah paham |
| Melewatkan edge case | Pseudocode `BAGI a / b` tanpa cek `b = 0` | Program crash |
| Tidak konsisten gaya | Campur bahasa Indonesia-Inggris, kadang IF, kadang JIKA | Membingungkan pembaca |

---

## Benang Merah

- **Materi 5 (Computational Thinking):** CT adalah *proses berpikir* — dekomposisi, pattern recognition. Pseudocode adalah alat untuk *menuliskan* hasil CT.
- **Materi 7 (Flowchart):** Pseudocode adalah teks, flowchart adalah visual. Keduanya alat untuk algoritma.

---

## Soal Latihan + Jawaban

### Soal 1 (Mudah)
Apa perbedaan antara abstraksi dan pseudocode? Jelaskan dalam 1-2 kalimat.

<details>
<summary>Jawaban</summary>

**Abstraksi** adalah konsep menyembunyikan detail — "apa yang dilakukan". **Pseudocode** adalah alat untuk menuliskan algoritma dengan bahasa manusia. Abstraksi adalah *gagasan*, pseudocode adalah *implementasi tertulis* dari gagasan itu.
</details>

### Soal 2 (Sedang)
Tulis pseudocode dan JavaScript untuk algoritma: **"Cek apakah sebuah angka positif, negatif, atau nol"**.

<details>
<summary>Jawaban</summary>

**Pseudocode:**
```
INPUT angka
IF angka > 0 THEN
    OUTPUT "Positif"
ELSE IF angka < 0 THEN
    OUTPUT "Negatif"
ELSE
    OUTPUT "Nol"
END IF
```

**JavaScript:**
```javascript
function cekPositifNegatif(angka) {
    if (angka > 0) {
        return "Positif";
    } else if (angka < 0) {
        return "Negatif";
    } else {
        return "Nol";
    }
}

console.log(cekPositifNegatif(-5)); // Negatif
console.log(cekPositifNegatif(3));  // Positif
console.log(cekPositifNegatif(0));  // Nol
```
</details>

### Soal 3 (Tantangan)
Buatlah **pseudocode** untuk algoritma **parkir kendaraan**: 
- 1 jam pertama: Rp 5.000
- Jam berikutnya: Rp 3.000/jam
- Maksimal 24 jam
- Jika parkir > 24 jam, hitung per hari (Rp 50.000/hari) + sisa jam

Lalu terjemahkan ke JavaScript.

<details>
<summary>Jawaban</summary>

**Pseudocode:**
```
INPUT totalJam

IF totalJam <= 24 THEN
    biaya = 5000 + (totalJam - 1) * 3000
    IF totalJam == 1 THEN
        biaya = 5000
    END IF
ELSE
    hari = totalJam / 24 (dibulatkan ke bawah)
    sisaJam = totalJam % 24
    biaya = hari * 50000 + 5000 + (sisaJam - 1) * 3000
    IF sisaJam == 0 THEN
        biaya = hari * 50000
    END IF
    IF sisaJam == 1 THEN
        biaya = hari * 50000 + 5000
    END IF
END IF

OUTPUT biaya
```

**JavaScript:**
```javascript
function hitungParkir(totalJam) {
    let biaya;
    if (totalJam <= 24) {
        if (totalJam === 1) {
            biaya = 5000;
        } else {
            biaya = 5000 + (totalJam - 1) * 3000;
        }
    } else {
        let hari = Math.floor(totalJam / 24);
        let sisaJam = totalJam % 24;
        if (sisaJam === 0) {
            biaya = hari * 50000;
        } else if (sisaJam === 1) {
            biaya = hari * 50000 + 5000;
        } else {
            biaya = hari * 50000 + 5000 + (sisaJam - 1) * 3000;
        }
    }
    return biaya;
}

console.log(hitungParkir(2));    // 5000 + 3000 = 8000
console.log(hitungParkir(26));   // 1*50000 + 5000 = 55000
console.log(hitungParkir(48));   // 2*50000 = 100000
```
</details>
