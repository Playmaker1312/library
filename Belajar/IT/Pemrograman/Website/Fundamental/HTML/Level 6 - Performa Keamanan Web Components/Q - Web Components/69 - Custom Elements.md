# Custom Elements

## Penjelasan

Custom Elements adalah API Web Components yang memungkinkan kita mendefinisikan elemen HTML kustom dengan perilaku dan fungsionalitas sendiri. Elemen kustom dibuat dengan mendefinisikan kelas JavaScript yang mewarisi (`extends`) dari `HTMLElement`, lalu didaftarkan ke browser menggunakan `customElements.define()`.

## Fungsi

- Membuat elemen HTML baru yang tidak tersedia di HTML standar
- Membungkus logika, template, dan gaya ke dalam satu komponen yang dapat digunakan ulang
- Memungkinkan interaksi dengan siklus hidup elemen (lifecycle hooks) seperti `connectedCallback`, `disconnectedCallback`, `attributeChangedCallback`
- Meningkatkan modularitas dan organisasi kode front-end

## Cara Pengimplementasian

```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Custom Elements Demo</title>
</head>
<body>
  <kartu-profil nama="Budi" jabatan="Arsitek"></kartu-profil>
  <kartu-profil nama="Siti" jabatan="Mandor"></kartu-profil>

  <script src="kartu-profil.js"></script>
</body>
</html>
```

```javascript
// kartu-profil.js
class KartuProfil extends HTMLElement {
  constructor() {
    super();
    this.innerHTML = `
      <div style="border: 2px solid #333; padding: 16px; margin: 8px 0; border-radius: 8px;">
        <h3>${this.getAttribute('nama')}</h3>
        <p>${this.getAttribute('jabatan')}</p>
      </div>
    `;
  }

  connectedCallback() {
    console.log(`Elemen <kartu-profil> dipasang ke DOM — ${this.getAttribute('nama')} sudah siap bekerja!`);
  }
}

customElements.define('kartu-profil', KartuProfil);
```

## Analogi (tema RUMAH/BANGUNAN)

Custom Elements seperti **cetakan (bekisting) beton** di proyek konstruksi. Cetakan ini adalah template yang bisa digunakan berulang kali untuk menghasilkan kolom atau balok beton yang bentuknya persis sama. Setiap kali kita menuang beton ke cetakan yang sama, hasilnya akan konsisten — sama seperti setiap kali kita menulis `<kartu-profil nama="...">` di HTML, elemen yang muncul akan memiliki struktur dan perilaku yang seragam.

## Dipakai Untuk

- Komponen UI yang dipakai di banyak tempat (tombol, card, modal, navbar)
- Widget pihak ketiga yang perlu diisolasi dari halaman utama
- Aplikasi berbasis micro-frontend
- Pembuatan design system atau component library

## Kesalahan Umum

1. **Nama tidak mengandung strip (`-`)** — HTML mewajibkan nama elemen kustom memiliki setidaknya satu tanda hubung. `customElements.define('kartuprofil', ...)` akan error. Harus `'kartu-profil'`.
2. **Mendaftar elemen dua kali** — `customElements.define()` hanya bisa dipanggil sekali per nama. Memanggil ulang akan melempar `DOMException`.
3. **Lupa extends HTMLElement** — Kelas harus mewarisi `HTMLElement`, jika tidak browser tidak mengenalinya sebagai elemen HTML yang valid.
4. **Menggunakan constructor untuk mengakses DOM anak atau atribut** — Di `constructor`, elemen belum terpasang ke DOM. Gunakan `connectedCallback` untuk manipulasi DOM atau atribut yang membutuhkan dokumen.

## Koneksi dengan Materi Sebelumnya

Custom Elements adalah fondasi dari Web Components. Sebelumnya kita belajar tentang DOM manipulation dan class JavaScript — keduanya digunakan langsung di sini. Setelah menguasai Custom Elements, kita bisa menggabungkannya dengan **Shadow DOM** (materi 70) untuk mengisolasi gaya dan DOM internal, serta **HTML Templates** (`<template>`) untuk mendefinisikan markup yang lebih rapi.

## Soal Latihan

1. Buat sebuah custom element bernama `<tombol-aksi>` yang menampilkan tombol dengan teks "Klik Saya" dan ketika diklik, muncul alert "Tombol ditekan!".
2. Apa yang terjadi jika kita memanggil `customElements.define('tombol', TombolAksi)`? Mengapa?
3. Dalam lifecycle custom elements, di method manakah sebaiknya kita menambahkan event listener?

<details><summary>Jawaban</summary>

1. 
```javascript
class TombolAksi extends HTMLElement {
  constructor() {
    super();
    this.innerHTML = '<button>Klik Saya</button>';
  }

  connectedCallback() {
    this.querySelector('button').addEventListener('click', () => {
      alert('Tombol ditekan!');
    });
  }
}

customElements.define('tombol-aksi', TombolAksi);
```

2. Akan terjadi error `SyntaxError`. Nama elemen kustom **wajib** mengandung setidaknya satu tanda hubung (`-`). `'tombol'` tanpa strip tidak diizinkan oleh spesifikasi HTML.

3. Event listener sebaiknya ditambahkan di `connectedCallback()`, bukan di `constructor()`, karena pada saat `constructor` dijalankan, elemen belum terpasang ke DOM sehingga `querySelector` mungkin tidak menemukan elemen anak.

</details>
