# Tag `<template>`

## Penjelasan

Elemen `<template>` adalah wadah untuk menyimpan konten HTML yang **tidak langsung dirender** di halaman. Konten di dalamnya baru akan muncul ketika digandakan (di-clone) menggunakan JavaScript lalu disisipkan ke DOM. Ini berguna untuk membuat cetakan (template) elemen yang bisa dipakai berulang kali tanpa menulis ulang HTML.

## Fungsi

- Menyimpan cuplikan HTML untuk digunakan kembali (reusable)
- Membuat cetakan kartu, daftar, atau item berulang tanpa duplikasi kode HTML
- Meningkatkan performa karena konten tidak diproses browser sampai di-clone
- Memisahkan struktur template dari logika JavaScript
- Mempermudah rendering data dinamis dari array/API

## Cara Pengimplementasian

```html
<template id="cardProyek">
  <div class="card">
    <h3 class="judul"></h3>
    <p class="deskripsi"></p>
    <span class="status"></span>
  </div>
</template>

<div id="daftarProyek"></div>
```

```javascript
const proyek = [
  { judul: "Web Portfolio", deskripsi: "Situs portofolio pribadi", status: "Selesai" },
  { judul: "E-Commerce", deskripsi: "Toko online dengan React", status: "Proses" },
  { judul: "Weather App", deskripsi: "Aplikasi cuaca real-time", status: "Selesai" },
];

const template = document.getElementById('cardProyek');
const container = document.getElementById('daftarProyek');

proyek.forEach(item => {
  // Clone konten template (true = clone seluruh anak-anaknya)
  const clone = template.content.cloneNode(true);

  // Isi data ke elemen clone
  clone.querySelector('.judul').textContent = item.judul;
  clone.querySelector('.deskripsi').textContent = item.deskripsi;
  clone.querySelector('.status').textContent = item.status;

  // Sisipkan ke DOM
  container.appendChild(clone);
});
```

## Analogi (tema RUMAH/BANGUNAN)

`<template>` seperti **cetakan cetak biru (blueprint) rumah di dalam lemari arsip**. Cetak biru ini tidak membangun rumah sungguhan sampai kamu benar-benar mengeluarkannya dan membangunnya. Setiap kali kamu butuh rumah baru dengan tipe sama, kamu cukup fotokopi (clone) cetak biru itu lalu isi detailnya (cat warna, ukuran jendela, dll). `cloneNode(true)` adalah mesin fotokopi yang menggandakan seluruh detail cetakan. Tanpa template, kamu harus menggambar ulang cetak biru dari nol setiap kali.

## Dipakai Untuk

- Menampilkan daftar kartu (project cards, product cards, user cards)
- Membuat daftar item dari data array (list rendering)
- Komponen web (Web Components) bersama `customElements`
- Menyimpan struktur popup, tooltip, atau modal yang dipakai berulang
- Alternatif ringan dari framework front-end untuk rendering daftar dinamis

## Kesalahan Umum

- **Lupa `cloneNode(true)`** — tanpa `true`, hanya node pembungkus yang di-clone tanpa anak-anaknya, sehingga hasilnya kosong
- **Menggunakan `innerHTML` pada template** — template bukan elemen biasa; gunakan `content` properti untuk mengakses isinya
- **Menyisipkan template itu sendiri** — jangan sisipkan `<template>` ke DOM, sisipkan hasil clone dari `template.content`
- **Gagal querySelector setelah clone** — pastikan querySelector dipanggil pada clone, bukan pada template (template tidak ada di DOM)
- **Cache template di luar loop** — panggil `template.content.cloneNode(true)` di dalam iterasi, jangan di luar, agar setiap clone independen

## Koneksi dengan Materi Sebelumnya

- **DOM Manipulation** — `cloneNode()`, `querySelector()`, `appendChild()` adalah metode dasar yang digunakan bersama template
- **Array & Looping** — `forEach` atau `map` untuk mengiterasi data dan membuat banyak clone sekaligus
- **Event Delegation** — karena elemen hasil clone tidak ada di HTML awal, event handler perlu didelegasikan ke parent
- **Web Components** — `<template>` adalah fondasi dari `<slot>` dan `Shadow DOM`
- **innerHTML** — template adalah alternatif yang lebih aman dan performan dibanding menyusun HTML lewat string concatenation

## Soal Latihan

1. Buat template untuk kartu produk (gambar, nama, harga) lalu render 3 produk menggunakan array data.

2. Jelaskan apa yang terjadi jika `cloneNode()` dipanggil tanpa argumen `true`.

<details><summary>Jawaban</summary>

**Soal 1:**
```html
<template id="cardProduk">
  <div class="produk">
    <img class="gambar" src="" alt="" />
    <h4 class="nama"></h4>
    <p class="harga"></p>
  </div>
</template>
<div id="produkContainer"></div>

<script>
const produkData = [
  { nama: "Sepatu", harga: "Rp150.000", gambar: "sepatu.jpg" },
  { nama: "Tas", harga: "Rp200.000", gambar: "tas.jpg" },
  { nama: "Topi", harga: "Rp50.000", gambar: "topi.jpg" },
];

const tmpl = document.getElementById('cardProduk');
const container = document.getElementById('produkContainer');

produkData.forEach(item => {
  const clone = tmpl.content.cloneNode(true);
  clone.querySelector('.gambar').src = item.gambar;
  clone.querySelector('.nama').textContent = item.nama;
  clone.querySelector('.harga').textContent = item.harga;
  container.appendChild(clone);
});
</script>
```

**Soal 2:**
Tanpa argumen `true`, `cloneNode()` hanya meng-clone node itu sendiri tanpa anak-anak (deep copy vs shallow copy). Artinya konten di dalam `<template>` (seperti `<div>`, `<h3>`, `<p>`) tidak ikut tergandakan, sehingga hasilnya node kosong tanpa isi.

</details>
