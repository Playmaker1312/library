# 77. JavaScript di Browser — DOM Manipulation

**Benang Merah**: Sebelumnya kita bikin backend API dengan Express. Sekarang kita beralih ke **frontend** — bagaimana JavaScript berinteraksi dengan halaman web melalui **DOM** (Document Object Model).

---

## Penjelasan

DOM adalah **representasi struktur HTML** dalam bentuk object tree yang bisa dimanipulasi oleh JavaScript. Ibarat **blueprint interaktif rumah**: Anda bisa melihat semua ruangan (elemen HTML), memindahkan furnitur (mengubah konten), menambah dinding (menambah elemen), atau mengecat ulang (mengubah style).

```html
<!-- HTML -->
<p id="judul">Halo Dunia</p>
```

```javascript
// JavaScript — memanipulasi DOM
const elemen = document.getElementById('judul');
elemen.textContent = 'Halo JavaScript!';    // ubah teks
elemen.style.color = 'red';                 // ubah warna
elemen.classList.add('highlight');          // tambah kelas CSS
```

Tanpa DOM manipulation, halaman web statis — tidak bisa berubah setelah dimuat. Dengan DOM, halaman jadi **hidup dan interaktif**.

---

## Fungsi

Memungkinkan JavaScript membaca dan mengubah **struktur, konten, dan style** halaman web secara dinamis — tanpa perlu reload halaman.

---

## Cara Pengimplementasian

### 1. Mengakses Elemen
```javascript
// Single element
const header = document.getElementById('header');
const pertama = document.querySelector('.item'); // CSS selector
const link = document.querySelector('nav a');

// Multiple elements
const semuaItem = document.querySelectorAll('.item'); // NodeList
const paragraf = document.getElementsByTagName('p'); // HTMLCollection
```

### 2. Memodifikasi Konten
```javascript
const judul = document.querySelector('h1');

judul.textContent = 'Judul Baru';        // teks doang (lebih cepat, aman)
judul.innerText = 'Judul Baru';          // teks + CSS-aware (lebih berat)
judul.innerHTML = '<span>Judul</span>';  // parse HTML (hati-hati XSS!)
```

### 3. Memodifikasi Style & Class
```javascript
const kotak = document.querySelector('.kotak');

// Style langsung
kotak.style.backgroundColor = 'blue';
kotak.style.fontSize = '20px';

// ClassList — recommended
kotak.classList.add('aktif');
kotak.classList.remove('non-aktif');
kotak.classList.toggle('gelap'); // tambah jika belum, hapus jika sudah
kotak.classList.contains('aktif'); // true/false
```

### 4. Membuat & Menghapus Elemen
```javascript
// CREATE
const divBaru = document.createElement('div');
divBaru.textContent = 'Elemen Baru';
divBaru.className = 'card';
document.body.appendChild(divBaru); // taruh di akhir body

// atau taruh di tengah
const container = document.querySelector('.container');
container.insertBefore(divBaru, container.firstChild);

// DELETE
const hapus = document.querySelector('.lama');
hapus.remove(); // langsung hapus

// atau dari parent
hapus.parentNode.removeChild(hapus); // cara lama
```

### 5. Todo List Dinamis (Contoh Lengkap)
```html
<!-- HTML -->
<input id="taskInput" placeholder="Tambah tugas...">
<button id="addBtn">Tambah</button>
<ul id="todoList"></ul>
```

```javascript
const input = document.getElementById('taskInput');
const btn = document.getElementById('addBtn');
const list = document.getElementById('todoList');

btn.addEventListener('click', () => {
  const task = input.value.trim();
  if (!task) return;

  const li = document.createElement('li');
  li.textContent = task;

  const deleteBtn = document.createElement('button');
  deleteBtn.textContent = 'Hapus';
  deleteBtn.addEventListener('click', () => li.remove());
  li.appendChild(deleteBtn);

  list.appendChild(li);
  input.value = '';
});
```

---

## Analogi: Membangun Rumah (Blueprint Interaktif)

| DOM Manipulation | Arsitek dengan Blueprint Digital |
|---|---|
| `document.querySelector` | Cari ruangan tertentu di blueprint |
| `.textContent` | Ganti nama ruangan |
| `.style` | Ganti warna dinding |
| `.classList` | Tambah/hapus stiker kategori |
| `createElement` | Tambah ruangan baru |
| `.remove()` | Bongkar ruangan |
| Event listener (Materi 78) | Sensor gerak: "saat pintu dibuka, lakukan X" |

Bayangkan Anda seorang arsitek dengan **blueprint digital** di tablet. Anda bisa tap ruangan → ganti nama → ganti warna → tambah jendela → hapus dinding — semua langsung terlihat, tanpa harus cetak ulang blueprint. Itulah DOM manipulation.

---

## Dipakai Untuk Apa

- **Single Page Application (SPA)** — update halaman tanpa reload
- **Form validation real-time** — cek input saat diketik
- **Dynamic content** — menampilkan data dari API
- **Animasi & interaksi** — dropdown, modal, carousel
- **Semua frontend framework** — Vue, React, Angular (di belakang layar)

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| QuerySelector typo | `document.querySelector('#myId')` vs `'.myClass'` | null → error |
| innerHTML untuk input user | `el.innerHTML = userInput` | **Rentan XSS!** |
| Lupa `#` untuk ID | `querySelector('judul')` padahal `#judul` | null |
| Append ke element yang belum ada | `document.body.appendChild(x)` sebelum body siap | error |
| Event listener kebanyakan | Tambah listener di loop → 100 listener | Memory leak |

---

## Hubungan dengan Materi Sebelumnya

DOM manipulation adalah **jembatan ke frontend**:
- Materi 74 (HTML) → DOM adalah representasi HTML
- Materi 75-76 (CSS) → `classList` + `style` untuk styling
- Materi 78 (Event Handling) → Saatnya bikin interaktif!
- Materi 79 (Fetch API) → Ambil data dari backend (Level 3 API) → tampilkan di DOM

---

## Soal Latihan

### Soal 1 (Mudah)
Buat halaman dengan tombol "Klik Saya". Saat diklik, teks di `<p id="output">` berubah menjadi "Tombol telah diklik!".

**Jawaban**:
```html
<button id="btn">Klik Saya</button>
<p id="output">Belum diklik</p>

<script>
  document.getElementById('btn').addEventListener('click', () => {
    document.getElementById('output').textContent = 'Tombol telah diklik!';
  });
</script>
```

### Soal 2 (Sedang)
Buat counter: tampilkan angka (mulai 0), 2 tombol "+" dan "-". Saat diklik, angka berubah. Jika angka negatif, warnanya merah.

**Jawaban**:
```html
<p id="counter">0</p>
<button id="plus">+</button>
<button id="minus">-</button>

<script>
  const counter = document.getElementById('counter');
  let nilai = 0;

  document.getElementById('plus').addEventListener('click', () => {
    nilai++;
    counter.textContent = nilai;
    counter.style.color = nilai < 0 ? 'red' : 'black';
  });

  document.getElementById('minus').addEventListener('click', () => {
    nilai--;
    counter.textContent = nilai;
    counter.style.color = nilai < 0 ? 'red' : 'black';
  });
</script>
```

### Soal 3 (Tantangan)
Buat daftar nama: input teks + tombol "Tambah". Setiap nama ditampilkan sebagai item list. Jika item diklik, coret teksnya (toggle strikethrough). Jika nama duplikat, tampilkan alert "Nama sudah ada".

**Jawaban**:
```html
<input id="namaInput" placeholder="Masukkan nama">
<button id="tambahBtn">Tambah</button>
<ul id="daftarNama"></ul>

<script>
  const input = document.getElementById('namaInput');
  const btn = document.getElementById('tambahBtn');
  const list = document.getElementById('daftarNama');

  btn.addEventListener('click', () => {
    const nama = input.value.trim();
    if (!nama) return;

    // Cek duplikat
    const items = list.querySelectorAll('li');
    for (let item of items) {
      if (item.textContent === nama) {
        alert('Nama sudah ada!');
        return;
      }
    }

    const li = document.createElement('li');
    li.textContent = nama;
    li.addEventListener('click', () => li.classList.toggle('selesai'));
    list.appendChild(li);
    input.value = '';
  });
</script>

<style>
  .selesai { text-decoration: line-through; color: gray; }
</style>
```
