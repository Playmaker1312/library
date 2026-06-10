# 1. Apa itu Pemrograman & Mengapa Penting

**Benang Merah**: Ini adalah materi pertama — fondasi dari semua yang akan datang. Tidak ada prasyarat.

---

## Penjelasan

Pemrograman adalah seni memberi instruksi ke komputer untuk melakukan tugas tertentu. Sama seperti arsitek memberi instruksi ke tukang bangunan melalui **blueprint**, programmer memberi instruksi ke komputer melalui **kode**.

Bedanya: komputer sangat **bodoh tapi sangat taat**. Dia tidak bisa menebak maksud Anda — Anda harus memberi instruksi yang **spesifik, terurut, dan tidak ambigu**.

```javascript
// Contoh instruksi ke komputer:
// "Hitung 2 + 2, lalu simpan hasilnya"
console.log(2 + 2); // Output: 4
```

---

## Fungsi

Pemrograman berfungsi sebagai **jembatan komunikasi** antara manusia dan komputer. Manusia berpikir abstrak, komputer bekerja dengan instruksi konkret. Pemrograman menerjemahkan yang abstrak menjadi spesifik.

Seperti **arsitek yang menerjemahkan ide rumah impian klien ke dalam gambar teknis** yang bisa dipahami tukang.

---

## Cara Pengimplementasian

Tulis instruksi dalam file teks (source code), lalu berikan ke komputer untuk dijalankan:

1. Buat file `instruksi.js`
2. Tulis kode JavaScript
3. Jalankan dengan Node.js

```javascript
// instruksi.js
console.log("Halo, dunia!");
console.log(2 + 3);
console.log("Pemrograman itu mudah!");
```

Jalankan:
```
node instruksi.js
```

Output:
```
Halo, dunia!
5
Pemrograman itu mudah!
```

---

## Analogi: Membangun Rumah

| Pemrograman | Membangun Rumah |
|---|---|
| Source code | Blueprint / gambar teknis |
| Komputer | Mandor bangunan |
| Program yang berjalan | Rumah jadi |
| Bug/Error | Kesalahan di blueprint |
| Debugging | Revisi gambar |

Seperti Anda tidak bisa langsung membangun rumah tanpa gambar, Anda tidak bisa membuat program tanpa menulis kode yang terstruktur. Komputer akan membaca instruksi Anda **baris per baris**, seperti tukang membaca gambar **detail per detail**.

---

## Dipakai Untuk Apa

- **Automasi tugas** — menghitung ribuan data dalam 1 detik
- **Membangun aplikasi** — website, game, mobile app
- **Analisis data** — menemukan pola dari data besar
- **Kontrol perangkat** — IoT, robot, smart home
- **Hampir semua yang melibatkan komputer**

---

## Kesalahan Umum

| Kesalahan | Contoh |
|---|---|
| Instruksi terlalu ambigu | "Hitung angka" — angka mana? diproses bagaimana? |
| Urutan salah | "Masukkan telur lalu pecahkan" — pecahkan dulu baru masukkan |
| Tidak detail | "Buatkan rumah" — berapa lantai? warna apa? |
| Lewati fondasi | Langsung mau bikin aplikasi complex tanpa tau variabel |

> **Aturan Emas Komputer**: Komputer tidak akan melakukan apa yang Anda **maksud**, tapi apa yang Anda **perintahkan**.

---

## Hubungan dengan Materi Sebelumnya

Ini adalah **batu bata pertama**. Semua materi berikutnya akan dibangun di atas pemahaman bahwa:
- Komputer butuh instruksi spesifik (→ Materi 2: bagaimana komputer membaca kode)
- Kita perlu alat yang tepat (→ Materi 4: setup environment)
- Masalah harus dipecah (→ Materi 5: computational thinking)

---

## Soal Latihan

### Soal 1 (Mudah)
Tulis instruksi langkah demi langkah (seperti resep) untuk membuat secangkir kopi instan. Minimal 5 langkah. Ini melatih cara berpikir seperti programmer.

**Jawaban**:
```
1. Siapkan cangkir bersih
2. Masukkan 1 sachet kopi instan ke cangkir
3. Didihkan air (100°C)
4. Tuang air panas ke cangkir (200ml)
5. Aduk selama 10 detik
6. Diamkan 2 menit sebelum diminum
```

### Soal 2 (Sedang)
Buat file `hitung.js` yang mencetak:
- Nama lengkap Anda
- Hasil dari 15 x 7
- Teks "Saya sedang belajar pemrograman!"

**Jawaban**:
```javascript
console.log("Budi Santoso");
console.log(15 * 7);     // 105
console.log("Saya sedang belajar pemrograman!");
```

Output:
```
Budi Santoso
105
Saya sedang belajar pemrograman!
```

### Soal 3 (Tantangan)
Tuliskan 3 instruksi untuk komputer yang **sengaja ambigu**, lalu jelaskan mengapa komputer akan gagal menjalankannya. Ini melatih Anda melihat celah dalam logika.

**Jawaban** (contoh):
1. "Cetak angka besar" — komputer tidak tahu ukuran "besar"
2. "Jika hujan, bawa payung, jika tidak jangan" — bagaimana jika gerimis?
3. "Hitung semua" — hitung apa? dari mana?
