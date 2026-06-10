# Mengenal JavaScript & Posisinya di Dunia Pemrograman

## Penjelasan

JavaScript (JS) lahir pada **1995** dibuat oleh Brendan Eich hanya dalam 10 hari. Awalnya hanya untuk browser (Netscape). Kini JS adalah **salah satu bahasa paling populer di dunia**.

### Bagaimana JS berevolusi?

| Era | Kemampuan |
|---|---|
| 1995–2009 | Hanya jalan di browser, untuk animasi & validasi form |
| 2009 (Node.js) | JS bisa jalan di **server** — Ryan Dahl membuat runtime JS di luar browser |
| 2015 (ES6/ES2015) | Lompatan besar: `class`, `arrow function`, `let/const`, `Promise` |
| Sekarang | Mobile (React Native), Desktop (Electron), IoT, Machine Learning |

### Ekosistem JavaScript

- **Engine V8** (Google) — mesin yang menjalankan JS di Chrome dan Node.js
- **npm** — package manager terbesar di dunia ( > 2 juta package)
- **Runtime** — Node.js (server), Deno, Bun
- **Framework** — React, Vue, Angular (frontend); Express, Next.js (fullstack)

---

## Fungsi

- Mengetahui **di mana JS bisa dipakai** — tidak terbatas di browser
- Memahami **mengapa JS layak dipelajari** — permintaan tinggi, komunitas besar
- Membedakan **JavaScript** (bahasa) vs **Node.js** (runtime) vs **React** (framework)

---

## Cara Implementasi / Code

```javascript
// Program JavaScript pertama — Hello World!
console.log("Hello World");

// Variasi di berbagai tempat
// Browser:   alert("Hello World");
// Node.js:   console.log("Hello World");
// React:     <h1>Hello World</h1>
```

**Cara menjalankan:**
1. Buka browser → F12 (DevTools) → tab Console → ketik `console.log("Hello World")` → Enter
2. Atau install Node.js, buat file `hello.js`, jalankan `node hello.js`

---

## Analogi (Membangun Rumah)

| Konsep | Analogi Rumah |
|---|---|
| JavaScript | **Tukang serba bisa** — bisa memasang bata, mengecat, memasang listrik |
| Engine V8 | **Motor bor listrik** — sumber tenaga di balik alat tukang |
| Node.js | **Toolbox** — berisi semua alat yang bisa dibawa ke mana saja |
| npm | **Toko material online** — ambil paket material jadi, pasang di proyek |
| Browser | **Rumah klien** — tempat tukang bekerja untuk tampilan depan |
| Server (Node.js) | **Gudang belakang** — tempat tukang bekerja untuk logika & data |
| Framework (React) | **Cetakan triplek** — pola siap pakai yang mempercepat kerja |

**Narasi:** JavaScript adalah **tukang serba bisa**. Awalnya ia hanya bekerja di **rumah klien** (browser). Lalu ada yang membuatkan **toolbox portabel** (Node.js) agar tukang bisa bekerja di **gudang belakang** (server) juga. **Toko material online** (npm) menyediakan ribuan alat dan komponen jadi yang tinggal dipasang. Dengan cetakan seperti **React** (triplek cetak), tukang bisa membuat banyak rumah lebih cepat.

---

## Dipakai Untuk Apa

- **Frontend Web:** Setiap website modern menggunakan JS — interaksi, animasi, validasi
- **Backend Server:** REST API dengan Express.js, real-time chat dengan Socket.io
- **Mobile App:** React Native (Instagram, Discord), Ionic
- **Desktop App:** VS Code, Slack, Discord, Spotify — semua pakai Electron (JS)
- **IoT:** Johnny-Five untuk robotika dengan JavaScript
- **Machine Learning:** TensorFlow.js — ML berjalan di browser

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Menyamakan Java dan JavaScript | "Belajar JavaScript dulu, baru Java" | Dua bahasa berbeda total! |
| Mengira JS cuma buat browser | `console.log` tidak jalan di browser | Salah konteks eksekusi |
| Lupa beda versi ES | Pakai `import` di Node.js versi lama | SyntaxError |
| Mengira JS = jQuery | "JS susah, pakai jQuery aja" | jQuery hanya library, ilmu JS tetap penting |

---

## Benang Merah

- **Materi 2 (Bagaimana Komputer Memproses Kode):** JS adalah bahasa **interpreted/JIT** — kita sudah tahu alur teks → eksekusi. Sekarang kita kenali bahasanya.
- **Materi 4 (Menyiapkan Environment):** JS butuh runtime. Kita akan install Node.js dan VS Code.

---

## Soal Latihan + Jawaban

### Soal 1 (Mudah)
Sebutkan 3 tempat atau platform di mana JavaScript bisa dijalankan.

<details>
<summary>Jawaban</summary>

1. **Browser** (Chrome, Firefox, Edge) — console DevTools
2. **Node.js** — runtime server
3. **Mobile** (React Native) atau **Desktop** (Electron)
</details>

### Soal 2 (Sedang)
Apa perbedaan antara **JavaScript** (bahasa), **Node.js** (runtime), dan **React** (framework)? Jelaskan dengan analogi rumah.

<details>
<summary>Jawaban</summary>

- **JavaScript** = tukang serba bisa (skill / bahasa)
- **Node.js** = toolbox tempat tukang menyimpan alat (runtime / lingkungan)
- **React** = cetakan triplek untuk membuat jendela lebih cepat (framework / library)

Tukang (JS) tetap punya skill yang sama di mana pun. Toolbox (Node.js) membantunya bekerja di luar rumah klien. Cetakan (React) mempercepat pekerjaan spesifik.
</details>

### Soal 3 (Tantangan)
Cari 3 aplikasi terkenal yang dibuat dengan JavaScript (stacknya). Untuk setiap aplikasi, sebutkan **bagian mana** yang menggunakan JS.

<details>
<summary>Jawaban</summary>

1. **VS Code** — Desktop app (Electron + JS/TS). Bagian editor, ekstensi, dan UI semuanya JS.
2. **Netflix** — Frontend dengan React JS. Bagian antarmuka pengguna.
3. **Discord** — Desktop app (Electron) + server dengan Node.js. Bagian chat real-time dan UI.

_Catatan: Ini contoh. Kamu bisa cari aplikasi lain seperti PayPal (React), LinkedIn (Node.js), atau Uber (Node.js)._
</details>
