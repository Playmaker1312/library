# Shadow DOM

## Penjelasan

Shadow DOM adalah teknologi Web Components yang menyediakan DOM tree terisolasi yang menempel pada suatu elemen (host). DOM di dalam shadow tree tidak dapat diakses dari luar dengan selector biasa, dan gaya CSS yang dideklarasikan di luar tidak akan bocor ke dalam, begitu pula sebaliknya. Shadow DOM dibuat dengan memanggil `element.attachShadow({ mode: 'open' })`.

## Fungsi

- Mengisolasi DOM internal komponen dari halaman utama — query selector luar tidak bisa menembus shadow boundary
- Mencegah kebocoran style (CSS scoping) — style dari luar tidak memengaruhi elemen di dalam Shadow DOM
- Melindungi komponen dari benturan kode global — aman digunakan di proyek besar atau pihak ketiga
- Memungkinkan penggunaan `<slot>` untuk mendistribusikan konten dari luar ke dalam shadow tree

## Cara Pengimplementasian

```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Shadow DOM Demo</title>
  <style>
    /* Style global — tidak akan memengaruhi elemen di dalam Shadow DOM */
    p {
      color: red;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <p>Teks ini berwarna merah (style global).</p>

  <kartu-info nama="Tukang"></kartu-info>

  <script src="kartu-info.js"></script>
</body>
</html>
```

```javascript
// kartu-info.js
class KartuInfo extends HTMLElement {
  constructor() {
    super();

    // Buat shadow root dengan mode 'open'
    const shadow = this.attachShadow({ mode: 'open' });

    // Buat struktur di dalam shadow
    const wrapper = document.createElement('div');
    wrapper.setAttribute('class', 'kartu');

    const nama = document.createElement('p');
    nama.textContent = `Profil: ${this.getAttribute('nama')}`;

    const style = document.createElement('style');
    style.textContent = `
      /* Style ini hanya berlaku di dalam Shadow DOM */
      .kartu {
        border: 2px solid green;
        padding: 16px;
        border-radius: 8px;
        background: #f0fff0;
      }
      p {
        color: green;
        font-size: 1.2em;
      }
    `;

    // Lampirkan elemen ke shadow root
    shadow.appendChild(style);
    shadow.appendChild(wrapper);
    wrapper.appendChild(nama);
  }

  connectedCallback() {
    console.log('Shadow DOM terpasang!');
  }
}

customElements.define('kartu-info', KartuInfo);
```

## Analogi (tema RUMAH/BANGUNAN)

Shadow DOM seperti **rumah dengan kamar kedap suara**. Kamar-kamar di dalam rumah (Shadow DOM) memiliki dinding yang tebal sehingga suara dari luar tidak masuk ke dalam kamar, dan suara dari dalam kamar tidak terdengar keluar. Orang di ruang tamu tidak tahu apa yang terjadi di dalam kamar kedap suara — isinya terisolasi. Ini persis seperti cara Shadow DOM mengisolasi DOM dan style dari dunia luar.

## Dipakai Untuk

- Komponen UI yang perlu dijamin tidak terpengaruh style global (misalnya widget chat, date picker, video player)
- Design system/component library agar konsisten di berbagai proyek
- Micro-frontend yang di-deploy independen
- Embed widget dari pihak ketiga (tombol like, form embed)

## Kesalahan Umum

1. **Mencoba attachShadow pada elemen yang sudah memiliki shadow root** — Setiap elemen hanya bisa memiliki satu shadow root. Melakukan `attachShadow` dua kali akan error.
2. **Lupa menentukan `mode`** — Parameter `mode` ('open' atau 'closed') wajib diberikan. `element.attachShadow({})` akan error.
3. **Menganggap mode 'closed' sebagai proteksi keamanan** — Mode 'closed' hanya menyembunyikan `shadowRoot` dari akses properti publik, tapi tetap bisa diakses melalui referensi internal. Ini bukan mekanisme keamanan.
4. **Menulis style global yang justru dibutuhkan di dalam Shadow DOM** — Style dari luar tidak menembus shadow boundary. Jika komponen butuh style tertentu, harus dideklarasikan di dalam shadow tree.
5. **Menggunakan innerHTML di Shadow DOM dengan konten yang tidak aman** — Memasukkan string HTML dari luar tanpa sanitasi tetap rentan XSS meskipun di dalam Shadow DOM.

## Koneksi dengan Materi Sebelumnya

Shadow DOM adalah lapisan berikutnya setelah **Custom Elements** (materi 69). Custom Elements memberi kita elemen kustom; Shadow DOM memberi isolasi pada elemen tersebut. Dengan keduanya digabung, kita bisa membuat komponen yang benar-benar mandiri. Selanjutnya, **HTML Templates** (`<template>`) dan **`<slot>`** akan melengkapi Web Components — template untuk mendefinisikan struktur, slot untuk menyisipkan konten dari luar ke dalam shadow tree.

## Soal Latihan

1. Buat sebuah custom element `<alamat-rumah>` yang menggunakan Shadow DOM (mode open) dan menampilkan alamat dari atribut `jalan`, `kota`, dan `kodePos`.
2. Apa perbedaan antara `mode: 'open'` dan `mode: 'closed'` pada `attachShadow`?
3. Jika di halaman utama ada style `div { color: blue; }`, apakah teks di dalam Shadow DOM akan berwarna biru? Jelaskan.

<details><summary>Jawaban</summary>

1. 
```javascript
class AlamatRumah extends HTMLElement {
  constructor() {
    super();
    const shadow = this.attachShadow({ mode: 'open' });

    const div = document.createElement('div');
    div.innerHTML = `
      <p><strong>Jalan:</strong> ${this.getAttribute('jalan')}</p>
      <p><strong>Kota:</strong> ${this.getAttribute('kota')}</p>
      <p><strong>Kode Pos:</strong> ${this.getAttribute('kodePos')}</p>
    `;

    const style = document.createElement('style');
    style.textContent = `
      div { background: #f9f9f9; padding: 12px; border: 1px solid #ccc; border-radius: 6px; }
      p { margin: 4px 0; }
    `;

    shadow.appendChild(style);
    shadow.appendChild(div);
  }
}

customElements.define('alamat-rumah', AlamatRumah);
```

2. **`mode: 'open'`** — Shadow root dapat diakses dari luar melalui properti `element.shadowRoot`. **`mode: 'closed'`** — Properti `element.shadowRoot` bernilai `null`, sehingga shadow root tidak bisa diakses dari luar. Namun mode 'closed' **bukan** fitur keamanan; shadow root tetap bisa diakses jika kita menyimpan referensinya secara internal.

3. **Tidak.** Teks di dalam Shadow DOM tidak akan berwarna biru. Style global (`div { color: blue }`) tidak menembus shadow boundary. Style di dalam Shadow DOM sepenuhnya terisolasi — itulah tujuan utama Shadow DOM.

</details>
