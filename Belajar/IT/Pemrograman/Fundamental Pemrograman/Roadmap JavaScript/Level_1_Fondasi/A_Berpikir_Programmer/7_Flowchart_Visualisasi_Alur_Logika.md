# Flowchart — Visualisasi Alur Logika

## Penjelasan

**Flowchart** adalah diagram yang menggambarkan langkah-langkah suatu proses menggunakan simbol-simbol standar. Ini adalah **visualisasi dari algoritma** — seperti pseudocode dalam bentuk gambar.

### Simbol Flowchart Standar

| Simbol | Nama | Arti |
|---|---|---|
| ⬤→ | **Start/End** (Terminator) | Mulai atau selesai |
| ▱ | **Process** | Operasi atau tindakan |
| ◇ | **Decision** | Percabangan (Ya/Tidak) |
| ⬭ | **Input/Output** | Baca data atau tampilkan hasil |
| → | **Arrow** | Alur urutan langkah |

---

## Fungsi

- **Memvisualisasikan alur logika** — lebih mudah dipahami daripada teks
- **Menemukan kesalahan logika** sebelum coding — lihat percabangan, looping
- **Komunikasi dengan non-teknis** — klien/stakeholder lebih mudah baca diagram
- **Dokumentasi** — diagram yang bisa dipahami 5 tahun kemudian

---

## Cara Implementasi / Code

### Contoh: Flowchart Sistem Login

```
    [START]
       ↓
   [Input username & password]
       ↓
   ◇ Apakah username terdaftar?  ----Tidak----> [Tampilkan "User tidak ditemukan"]
       |                                                      ↓
      Ya                                               [KEMBALI ke Input]
       ↓
   ◇ Apakah password cocok?  -----Tidak----> [Tampilkan "Password salah"]
       |                                                    ↓
      Ya                                             [KEMBALI ke Input]
       ↓
   [Tampilkan "Login berhasil"]
       ↓
    [END]
```

### Implementasi JavaScript

```javascript
function login(username, password) {
    // Database sederhana
    const database = {
        "andi": "rahasia123",
        "budi": "pass456"
    };

    // Decision 1: username terdaftar?
    if (!database[username]) {
        return "User tidak ditemukan";
    }

    // Decision 2: password cocok?
    if (database[username] !== password) {
        return "Password salah";
    }

    return "Login berhasil";
}

console.log(login("andi", "rahasia123")); // Login berhasil
console.log(login("andi", "salah"));       // Password salah
console.log(login("caca", "apa"));         // User tidak ditemukan
```

---

## Analogi (Membangun Rumah)

| Konsep | Analogi Rumah |
|---|---|
| **Flowchart** | **Diagram alur kerja pembangunan** — dipajang di papan proyek, semua pekerja lihat |
| **Start/End** | **Pagar lokasi** — tanda mulai dan selesai pekerjaan |
| **Process** | **Stasiun kerja** — "Potong kayu 2 meter", "Pasang bata" |
| **Decision** | **Titik inspeksi** — "Apakah dinding sudah rata?" Ya → lanjut, Tidak → perbaiki |
| **Input/Output** | **Gudang material** (input) dan **hasil jadi** (output) |
| **Arrow** | **Jalur pekerja** — urutan pengerjaan rumah |

**Narasi:** Di proyek rumah besar, ada **papan alur kerja** (flowchart) di lokasi. Semua pekerja — tukang batu, tukang listrik, tukang cat — melihat papan itu untuk tahu langkah selanjutnya. Mulai dari **START** (pembukaan lahan), melewati **stasiun kerja** (process), berhenti di **titik inspeksi** (decision) untuk periksa kualitas. Jika lulus, lanjut. Jika tidak, kembali ke stasiun sebelumnya. Sampai akhirnya **END** (rumah selesai). Flowchart seperti **GPS untuk pembangunan rumah** — semua orang tahu arah tanpa bingung.

---

## Dipakai Untuk Apa

- **Sistem login / registrasi** — memastikan alur otentikasi jelas
- **Algoritma sorting** — visualisasi bubble sort, quick sort
- **Proses bisnis** — alur order, pembayaran, pengiriman barang
- **Game logic** — alur kemenangan, level, nyawa
- **Debugging** — buat flowchart untuk melacak kemungkinan bug

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Decision tidak punya 2 cabang | ◇ "Apakah > 5?" cuma panah "Ya" | Logika tidak lengkap |
| Tidak ada start/end | Langsung ke process | Pembaca bingung mulai dan selesai di mana |
| Garis saling silang tanpa label | Banyak panah bersilangan | Flowchart sulit dibaca |
| Flowchart terlalu detail | Masukkan "deklarasikan variabel i = 0" | Kehilangan gambaran besar |

---

## Benang Merah

- **Materi 6 (Abstraksi & Pseudocode):** Pseudocode adalah teks, flowchart adalah visual. Keduanya merepresentasikan algoritma.
- **Materi 8 (Logika Boolean):** Decision di flowchart = percabangan yang menggunakan logika boolean (AND, OR, NOT).

---

## Soal Latihan + Jawaban

### Soal 1 (Mudah)
Sebutkan 4 simbol flowchart standar dan fungsinya.

<details>
<summary>Jawaban</summary>

1. **Terminator** (Start/End) — mulai atau selesai
2. **Process** — tindakan atau operasi
3. **Decision** — percabangan Ya/Tidak
4. **Input/Output** — baca data atau tampilkan hasil
</details>

### Soal 2 (Sedang)
Buat flowchart (deskripsi langkah-langkah) untuk algoritma **menentukan apakah suatu angka genap atau ganjil**. Lalu implementasikan di JavaScript.

<details>
<summary>Jawaban</summary>

**Flowchart (deskripsi):**
```
[START] → [Input angka] → ◇ angka % 2 == 0? 
  Ya → [Output "Genap"] → [END]
  Tidak → [Output "Ganjil"] → [END]
```

**JavaScript:**
```javascript
function genapGanjil(angka) {
    if (angka % 2 === 0) {
        return "Genap";
    } else {
        return "Ganjil";
    }
}

console.log(genapGanjil(10)); // Genap
console.log(genapGanjil(7));  // Ganjil
```
</details>

### Soal 3 (Tantangan)
Buat flowchart (deskripsi langkah-langkah) untuk **sistem lampu lalu lintas sederhana**:
- Jika lampu **merah** → "Berhenti"
- Jika lampu **kuning** → "Hati-hati"
- Jika lampu **hijau** → "Jalan"
- Selain itu → "Lampu rusak"

Implementasikan di JavaScript dengan sebuah fungsi yang menerima parameter warna.

<details>
<summary>Jawaban</summary>

**Flowchart (deskripsi):**
```
[START] → [Input warna] → 
  ◇ warna == "merah"? 
    Ya → [Output "Berhenti"] → [END]
    Tidak → ◇ warna == "kuning"?
      Ya → [Output "Hati-hati"] → [END]
      Tidak → ◇ warna == "hijau"?
        Ya → [Output "Jalan"] → [END]
        Tidak → [Output "Lampu rusak"] → [END]
```

**JavaScript:**
```javascript
function lampuLaluLintas(warna) {
    if (warna === "merah") {
        return "Berhenti";
    } else if (warna === "kuning") {
        return "Hati-hati";
    } else if (warna === "hijau") {
        return "Jalan";
    } else {
        return "Lampu rusak";
    }
}

console.log(lampuLaluLintas("merah"));   // Berhenti
console.log(lampuLaluLintas("hijau"));   // Jalan
console.log(lampuLaluLintas("biru"));    // Lampu rusak
```
</details>
