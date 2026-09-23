# localStorage

## Penjelasan

`localStorage` adalah API penyimpanan data di browser yang bersifat **persisten** — data tetap tersimpan meskipun browser ditutup. Data disimpan sebagai pasangan **key-value** dan hanya bisa menyimpan tipe data **string**. Untuk menyimpan objek atau array, harus dikonversi dulu menggunakan `JSON.stringify()` dan diurai kembali dengan `JSON.parse()`.

## Fungsi

- Menyimpan data pengguna secara lokal di browser tanpa kedaluwarsa
- Menyimpan progress form agar tidak hilang saat halaman direfresh
- Menyimpan preferensi tema, bahasa, atau pengaturan lainnya
- Mengurangi permintaan ke server dengan menyimpan cache data ringan
- Menyimpan state aplikasi web sederhana

## Cara Pengimplementasian

```html
<!-- form-progress.html -->
<form id="contactForm">
  <input type="text" id="name" placeholder="Nama" />
  <input type="email" id="email" placeholder="Email" />
  <textarea id="message" placeholder="Pesan"></textarea>
</form>
```

```javascript
// Simpan progress form setiap ada perubahan
const form = document.getElementById('contactForm');
const fields = ['name', 'email', 'message'];

// Muat data saat halaman dimuat
window.addEventListener('DOMContentLoaded', () => {
  fields.forEach(id => {
    const saved = localStorage.getItem(id);
    if (saved) document.getElementById(id).value = saved;
  });
});

// Simpan setiap kali input berubah
fields.forEach(id => {
  const el = document.getElementById(id);
  el.addEventListener('input', () => {
    localStorage.setItem(id, el.value);
  });
});

// Hapus data setelah form disubmit
form.addEventListener('submit', (e) => {
  e.preventDefault();
  fields.forEach(id => localStorage.removeItem(id));
  form.reset();
});

// Contoh JSON.stringify / JSON.parse untuk data kompleks
const proyek = { judul: "Web Portfolio", selesai: true, tugas: 12 };
localStorage.setItem('proyekSaya', JSON.stringify(proyek));

const ambil = JSON.parse(localStorage.getItem('proyekSaya'));
console.log(ambil.judul); // "Web Portfolio"
```

## Analogi (tema RUMAH/BANGUNAN)

`localStorage` seperti **lemari arsip pribadi di dalam rumah**. Kamu bisa menyimpan dokumen (data) ke dalam amplop yang diberi label (key). Dokumen tidak hilang meskipun kamu keluar rumah dan kembali lagi nanti. `setItem` = memasukkan amplop ke lemari, `getItem` = mengambil amplop dari lemari, `removeItem` = membuang amplop dari lemari. `JSON.stringify` seperti merapikan isi amplop ke dalam format yang bisa dimasukkan, dan `JSON.parse` seperti mengeluarkannya kembali ke bentuk semula.

## Dipakai Untuk

- Form yang membutuhkan draft otomatis (misal: komentar panjang, survey)
- Aplikasi catatan atau to-do list sederhana
- Menyimpan preferensi tema (gelap/terang)
- Menyimpan riwayat pencarian lokal
- Game progres tanpa backend

## Kesalahan Umum

- **Lupa JSON.stringify/parse** — menyimpan objek langsung menghasilkan `"[object Object]"`, bukan data sesungguhnya
- **Tidak mengecek null saat getItem** — jika key tidak ada, `getItem` mengembalikan `null`, lalu `JSON.parse(null)` menghasilkan `null`, bukan error (tapi tetap harus di-handle)
- **Menyimpan data sensitif** — `localStorage` tidak aman untuk password atau token karena bisa diakses lewat JS
- **Melebihi kuota** — sebagian besar browser membatasi ~5–10 MB per domain
- **Asumsi data selalu ada** — user bisa hapus data browser kapan saja

## Koneksi dengan Materi Sebelumnya

- **sessionStorage** — mirip localStorage tetapi data terhapus saat tab ditutup (sesi saja)
- **Cookies** — dikirim ke server tiap request, sedangkan localStorage hanya di client
- **JSON** — kemampuan mengonversi objek/array ke string adalah prasyarat menggunakan localStorage untuk data kompleks
- **Event listener** — `input`, `submit`, `DOMContentLoaded` digunakan untuk otomatisasi simpan/muat data form
- **DOM manipulation** — mengambil dan mengisi nilai elemen form (`document.getElementById().value`)

## Soal Latihan

1. Buat kode yang menyimpan nama pengguna ke localStorage saat tombol "Simpan" diklik, lalu tampilkan di halaman saat refresh.

2. Tulis kode yang menghapus semua data localStorage ketika tombol "Hapus Semua" diklik.

<details><summary>Jawaban</summary>

**Soal 1:**
```html
<input type="text" id="nama" />
<button id="simpan">Simpan</button>
<p id="output"></p>

<script>
document.getElementById('simpan').addEventListener('click', () => {
  const nama = document.getElementById('nama').value;
  localStorage.setItem('namaUser', nama);
  document.getElementById('output').textContent = nama;
});

window.addEventListener('DOMContentLoaded', () => {
  const saved = localStorage.getItem('namaUser');
  if (saved) {
    document.getElementById('output').textContent = saved;
  }
});
</script>
```

**Soal 2:**
```javascript
document.getElementById('hapusSemua').addEventListener('click', () => {
  localStorage.clear();
  // atau hapus satu per satu:
  // localStorage.removeItem('namaUser');
});
```
</details>
