# Menyiapkan Environment — VS Code, Node.js, Terminal

## Penjelasan

Sebelum membangun rumah, kamu perlu **lahan, alat, dan bahan**. Sama dengan coding — kita perlu **environment** yang siap pakai.

### Yang perlu diinstall

| Alat | Fungsi | Analogi Rumah |
|---|---|---|
| **Node.js** | Runtime JavaScript — menjalankan kode JS di komputer | **Generator listrik** — sumber daya agar alat bisa bekerja |
| **npm** | Package manager — menginstall library JS | **Toko material** — ambil komponen jadi |
| **VS Code** | Text editor — menulis kode | **Meja kerja tukang** — tempat merangkai semuanya |
| **Terminal** | Command line interface — memberi perintah ke komputer | **Walkie-talkie** — memberi instruksi ke mandor |

---

## Fungsi

- Membuat komputer **mampu menjalankan kode JavaScript** (tanpa browser)
- Memberi tempat **nyaman menulis kode** dengan syntax highlighting, autocomplete
- Mengenal **terminal/command line** — skill fundamental programmer

---

## Cara Implementasi / Code

### 1. Install Node.js dan npm

Buka [nodejs.org](https://nodejs.org), download versi LTS. Install seperti biasa.

Verifikasi installasi:

```bash
node --version    # Contoh output: v18.20.0
npm --version     # Contoh output: 9.8.1
```

### 2. Install VS Code + Extension

Download di [code.visualstudio.com](https://code.visualstudio.com).

Extension penting:
- **ESLint** — pemeriksa kode otomatis
- **Prettier** — perapih format kode
- **JavaScript (ES6) code snippets** — potongan kode cepat

### 3. Membuat dan menjalankan file JS

Buat file `hello.js`:

```javascript
console.log("Halo, dunia!");
console.log("Saya sedang belajar JavaScript");
```

Jalankan di terminal:

```bash
node hello.js
```

Output:
```
Halo, dunia!
Saya sedang belajar JavaScript
```

---

## Analogi (Membangun Rumah)

| Konsep | Analogi Rumah |
|---|---|
| Install Node.js | **Memasang generator listrik** di lokasi bangunan |
| npm install | **Pesan material** dari toko — datang siap pakai |
| VS Code | **Meja kerja tukang** lengkap dengan penggaris, pensil, dan penghapus |
| Terminal | **Walkie-talkie** untuk perintah cepat ke semua pekerja |
| File `hello.js` | **Cetak biru kecil** pertama — satu dinding sederhana |
| `node hello.js` | Menyalakan generator dan **membangun dinding itu** |

**Narasi:** Kamu datang ke lahan kosong. Pertama, pasang **generator listrik** (Node.js) agar ada daya. Siapkan **meja kerja** (VS Code) dengan alat tulis dan penggaris. Ambil **walkie-talkie** (terminal) untuk memberi perintah. Lalu gambar **cetak biru kecil** (`hello.js`) — satu instruksi sederhana. Nyalakan generator dan bangun dinding itu (`node hello.js`). Rumah (program) mulai terbentuk!

---

## Dipakai Untuk Apa

- **Setiap proyek JavaScript** dimulai dengan setup environment
- **npm** digunakan setiap kali ingin install library: `npm install express`
- **Terminal** dipakai untuk git, test, build, deploy — sehari-hari programmer
- **VS Code** menjadi rumah utama coding dengan fitur debug, git integration, terminal built-in

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Tidak install Node.js | Jalankan `node hello.js` → error | `'node' is not recognized` |
| Salah folder di terminal | `node hello.js` padahal file di folder lain | `Error: Cannot find module` |
| Lupa simpan file | Edit file, jalankan ulang, output masih lama | Frustrasi karena tidak ada perubahan |
| Mengira npm = Node.js | `npm --version` error lalu bilang "Node.js error" | Padahal npm sudah terpisah sejak Node.js ≥ 0.6 |

---

## Benang Merah

- **Materi 3 (Mengenal JavaScript):** JS butuh runtime (Node.js) dan editor (VS Code). Sekarang kita siapkan.
- **Materi 5 (Computational Thinking):** Environment sudah siap. Sekarang kita belajar *cara berpikir* sebelum menulis kode.

---

## Soal Latihan + Jawaban

### Soal 1 (Mudah)
Apa perbedaan antara **Node.js** dan **npm**? Jelaskan dalam satu kalimat masing-masing.

<details>
<summary>Jawaban</summary>

- **Node.js** = runtime yang menjalankan kode JavaScript di luar browser
- **npm** = package manager untuk menginstall dan mengelola library JavaScript
</details>

### Soal 2 (Sedang)
Buat file `hello.js` yang berisi program menampilkan `"Nama saya [nama kamu]"` dan `"Saya ingin jadi programmer"`. Jalankan dengan Node.js dan tulis outputnya.

<details>
<summary>Jawaban</summary>

```javascript
// hello.js
console.log("Nama saya Budi");
console.log("Saya ingin jadi programmer");
```

Jalankan: `node hello.js`

Output:
```
Nama saya Budi
Saya ingin jadi programmer
```
</details>

### Soal 3 (Tantangan)
Kamu mendapat error berikut: `'node' is not recognized as an internal or external command`. Sebutkan 3 kemungkinan penyebab dan cara memperbaikinya.

<details>
<summary>Jawaban</summary>

1. **Node.js belum diinstall** — download dan install dari nodejs.org
2. **Node.js sudah diinstall tapi PATH tidak terdaftar** — restart terminal, atau tambahkan manual PATH: folder `C:\Program Files\nodejs\`
3. **Terminal perlu di-restart** — setelah install, restart terminal agar PATH ter-update

_Catatan: Setelah install, coba `node --version` untuk verifikasi._
</details>
