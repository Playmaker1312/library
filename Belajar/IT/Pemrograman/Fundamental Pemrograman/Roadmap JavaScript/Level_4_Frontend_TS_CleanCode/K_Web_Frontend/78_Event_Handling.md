# 78. Event Handling — Interaktivitas

**Benang Merah**: Dari Materi 77 (DOM manipulation) kita bisa mengubah elemen. Tapi perubahan itu **belum otomatis** — perlu dipicu oleh aksi pengguna. Event handling adalah **saklar dan sensor** yang membuat rumah merespon interaksi. Lanjut ke Materi 79 (Fetch API).

---

## Penjelasan

**Event** adalah sinyal bahwa sesuatu telah terjadi di halaman web: klik, ketik keyboard, submit form, gerakan mouse, resize layar, dll. **Event Handling** adalah cara JavaScript "mendengarkan" event tersebut dan menjalankan kode sebagai respons.

```javascript
// Dengar event klik → jalankan fungsi
element.addEventListener('click', () => {
  console.log('Elemen diklik!');
});
```

### Tipe-Tipe Event

| Kategori | Event | Dipicu Saat |
|---|---|---|
| Mouse | `click`, `dblclick`, `mouseenter`, `mouseleave` | Interaksi mouse |
| Keyboard | `keydown`, `keyup`, `keypress` | Tombol keyboard ditekan |
| Form | `submit`, `focus`, `blur`, `change`, `input` | Interaksi form |
| Window | `load`, `resize`, `scroll` | Perubahan window |
| Touch | `touchstart`, `touchend`, `touchmove` | Layar sentuh |

### Event Object

Setiap event membawa **event object** — berisi informasi detail tentang event yang terjadi.

```javascript
element.addEventListener('click', (event) => {
  console.log(event.target);      // elemen yang diklik
  console.log(event.type);        // 'click'
  console.log(event.clientX);     // posisi X mouse
  console.log(event.clientY);     // posisi Y mouse
});
```

---

## Fungsi

- **Interaktivitas** — halaman merespon tindakan pengguna
- **Form handling** — validasi, submit, autocomplete
- **Animasi & transisi** — trigger berdasarkan event
- **Game & aplikasi interaktif** — keyboard/mouse input
- **Event delegation** — efisien untuk banyak elemen

---

## Cara Pengimplementasian

### 1. addEventListener — Cara Modern

```javascript
const btn = document.getElementById('tombol');

btn.addEventListener('click', () => {
  alert('Tombol diklik!');
});

// Bisa multiple listener
btn.addEventListener('click', () => {
  console.log('Listener kedua juga jalan');
});

// Remove listener
const handler = () => console.log('Halo');
btn.addEventListener('click', handler);
btn.removeEventListener('click', handler); // tidak lagi dipanggil
```

### 2. Event Object — target, preventDefault, stopPropagation

```javascript
document.querySelector('a').addEventListener('click', (e) => {
  e.preventDefault();    // cegah navigasi
  console.log('Link diklik, tapi tidak pindah halaman');
});

document.querySelector('.parent').addEventListener('click', () => {
  console.log('Parent juga ke klik');
});

document.querySelector('.child').addEventListener('click', (e) => {
  e.stopPropagation();  // cegah event bubbling ke parent
  console.log('Hanya child');
});
```

### 3. Event Delegation

Daripada pasang listener ke 100 item, pasang satu listener ke **parent**:

```javascript
// Tanpa delegation — 100 listener
document.querySelectorAll('.item').forEach(item => {
  item.addEventListener('click', () => item.classList.toggle('active'));
});

// Dengan delegation — 1 listener
document.querySelector('.list').addEventListener('click', (e) => {
  const item = e.target.closest('.item');  // cari parent terdekat dengan class .item
  if (item) {
    item.classList.toggle('active');
  }
});
```

### 4. Form Submission

```javascript
const form = document.getElementById('pinjamForm');

form.addEventListener('submit', (e) => {
  e.preventDefault();  // cegah reload

  const formData = new FormData(form);
  const data = Object.fromEntries(formData);

  console.log('Data dikirim:', data);

  // Validasi sederhana
  if (!data.nama || data.nama.length < 3) {
    alert('Nama minimal 3 karakter');
    return;
  }

  // Kirim ke server (Materi 79)
  alert(`Terima kasih ${data.nama}, peminjaman diajukan!`);
});
```

### 5. Interaktivitas Lengkap — Halaman Perpustakaan

```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Perpustakaan Interaktif</title>
  <style>
    .card { padding: 16px; margin: 8px; border: 1px solid #ddd; border-radius: 8px; cursor: pointer; transition: 0.2s; }
    .card:hover { background: #f5f5f5; }
    .card.selected { border-color: blue; background: #e3f2fd; }
    .hidden { display: none; }
  </style>
</head>
<body>
  <header>
    <h1>Perpustakaan Interaktif</h1>
    <input type="text" id="searchInput" placeholder="Cari buku..." aria-label="Cari buku">
  </header>

  <div id="bookList">
    <div class="card" data-id="1">
      <h3>JavaScript: The Good Parts</h3>
      <p>Douglas Crockford</p>
      <button class="pinjamBtn" data-id="1">Pinjam</button>
    </div>
    <div class="card" data-id="2">
      <h3>Clean Code</h3>
      <p>Robert C. Martin</p>
      <button class="pinjamBtn" data-id="2">Pinjam</button>
    </div>
  </div>

  <form id="pinjamForm">
    <h2>Form Peminjaman</h2>
    <input type="text" id="nama" placeholder="Nama" required>
    <input type="email" id="email" placeholder="Email" required>
    <button type="submit">Ajukan</button>
  </form>

  <div id="notification" class="hidden"></div>

  <script>
    // 1. Klik card → toggle selected
    document.getElementById('bookList').addEventListener('click', (e) => {
      const card = e.target.closest('.card');
      if (card && !e.target.matches('.pinjamBtn')) {
        card.classList.toggle('selected');
      }
    });

    // 2. Event delegation untuk tombol pinjam
    document.getElementById('bookList').addEventListener('click', (e) => {
      if (e.target.matches('.pinjamBtn')) {
        const bookTitle = e.target.closest('.card').querySelector('h3').textContent;
        showNotification(`Buku "${bookTitle}" berhasil dipinjam!`);
      }
    });

    // 3. Keyboard event — cari buku saat mengetik
    document.getElementById('searchInput').addEventListener('input', (e) => {
      const query = e.target.value.toLowerCase();
      document.querySelectorAll('.card').forEach(card => {
        const title = card.querySelector('h3').textContent.toLowerCase();
        card.style.display = title.includes(query) ? 'block' : 'none';
      });
    });

    // 4. Form submit
    document.getElementById('pinjamForm').addEventListener('submit', (e) => {
      e.preventDefault();
      const nama = document.getElementById('nama').value;
      showNotification(`Terima kasih ${nama}, pengajuan diproses!`);
      e.target.reset();
    });

    // Utility
    function showNotification(msg) {
      const notif = document.getElementById('notification');
      notif.textContent = msg;
      notif.classList.remove('hidden');
      setTimeout(() => notif.classList.add('hidden'), 3000);
    }
  </script>
</body>
</html>
```

---

## Analogi: Membangun Rumah (Saklar, Sensor, Interaksi)

| Event Handling | Rumah Pintar |
|---|---|
| `addEventListener('click', handler)` | Saklar lampu — "saat ditekan, nyalakan lampu" |
| `click` | Sentuhan tangan ke saklar |
| `mouseenter` / `mouseleave` | Sensor gerak — lampu otomatis saat ada orang |
| `keydown` | Sensor ketukan pintu |
| `submit` | Menekan bel pintu — kirim pesan ke dalam rumah |
| `preventDefault()` | Kunci pintu — cegah orang masuk tanpa izin |
| `stopPropagation()` | Suara hanya di satu ruangan — tidak ke seluruh rumah |
| `event.target` | Identifikasi saklar mana yang ditekan |
| Event delegation | Satu sensor pusat yang memonitor semua ruangan |
| `removeEventListener` | Mencabut saklar — tidak lagi merespon |

Rumah tanpa sensor/saklar = rumah mati. Anda harus colok/ cabut peralatan langsung. Dengan event handling, rumah Anda **hidup** — penghuni cukup sentuh, tekan, atau suara, dan rumah merespon.

---

## Dipakai Untuk Apa

- **Semua interaksi web** — klik tombol, submit form, hover menu
- **Form validation real-time** — cek input saat diketik (`input` event)
- **Drag & drop** — `mousedown` + `mousemove` + `mouseup`
- **Infinite scroll** — `scroll` event, load data saat mendekati bawah
- **Keyboard shortcuts** — `keydown` untuk shortcut (Ctrl+S, dll)
- **Game** — keyboard, mouse, touch input

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| `onclick` di HTML | `<button onclick="handler()">` | Sulit di-maintain, cuma 1 listener |
| Lupa `e.preventDefault()` di form | Form submit → halaman reload | State hilang |
| `stopPropagation()` berlebihan | Semua event dihentikan | Fitur lain (modal, dropdown) rusak |
| Listener di loop tanpa handle | 100 item → 100 listener | Memory leak, lambat |
| Event delegation tanpa `closest` | `e.target` bisa bukan elemen yang diharapkan | Bug tak terduga |

---

## Hubungan dengan Materi Sebelumnya

- Materi 77 (DOM) → Event handling memicu perubahan DOM
- Materi 79 (Fetch API) → Event submit form → fetch ke API
- Materi 75 (CSS) → `classList.toggle` + CSS = animasi interaktif
- Level 3 (Backend API) → Event handling adalah jembatan user → API

---

## Soal Latihan

### Soal 1 (Mudah)
Buat tombol "Klik" yang setiap diklik, counter di sampingnya bertambah 1. Gunakan `addEventListener`.

**Jawaban**:
```html
<button id="klikBtn">Klik</button>
<span id="counter">0</span>

<script>
  let count = 0;
  document.getElementById('klikBtn').addEventListener('click', () => {
    count++;
    document.getElementById('counter').textContent = count;
  });
</script>
```

### Soal 2 (Sedang)
Buat form login dengan validasi: email harus mengandung `@`, password minimal 6 karakter. Tampilkan pesan error di bawah masing-masing input. Gunakan event `submit`.

**Jawaban**:
```html
<form id="loginForm">
  <div>
    <input type="text" id="email" placeholder="Email">
    <span id="emailError" style="color:red"></span>
  </div>
  <div>
    <input type="password" id="password" placeholder="Password">
    <span id="passError" style="color:red"></span>
  </div>
  <button type="submit">Login</button>
</form>

<script>
  document.getElementById('loginForm').addEventListener('submit', (e) => {
    e.preventDefault();

    const email = document.getElementById('email').value;
    const password = document.getElementById('password').value;
    const emailError = document.getElementById('emailError');
    const passError = document.getElementById('passError');

    let valid = true;

    if (!email.includes('@')) {
      emailError.textContent = 'Email harus mengandung @';
      valid = false;
    } else {
      emailError.textContent = '';
    }

    if (password.length < 6) {
      passError.textContent = 'Password minimal 6 karakter';
      valid = false;
    } else {
      passError.textContent = '';
    }

    if (valid) alert('Login berhasil!');
  });
</script>
```

### Soal 3 (Tantangan)
Buat daftar tugas (todo list) dengan event delegation. Fitur: tambah tugas (input + tombol), tandai selesai (klik item), hapus tugas (tombol X). Gunakan **satu** event listener di parent `<ul>`.

**Jawaban**:
```html
<input type="text" id="todoInput" placeholder="Tambah tugas...">
<button id="addBtn">Tambah</button>
<ul id="todoList"></ul>

<script>
  const input = document.getElementById('todoInput');
  const addBtn = document.getElementById('addBtn');
  const list = document.getElementById('todoList');

  addBtn.addEventListener('click', () => {
    const text = input.value.trim();
    if (!text) return;

    const li = document.createElement('li');
    li.innerHTML = `<span class="text">${text}</span> <button class="deleteBtn">X</button>`;
    list.appendChild(li);
    input.value = '';
  });

  // Event delegation — satu listener untuk semua item
  list.addEventListener('click', (e) => {
    const target = e.target;

    if (target.classList.contains('deleteBtn')) {
      // Hapus item
      target.closest('li').remove();
    } else if (target.classList.contains('text') || target.matches('li')) {
      // Tandai selesai — toggle class
      const li = target.closest('li');
      li.querySelector('.text').classList.toggle('done');
    }
  });
</script>

<style>
  .done { text-decoration: line-through; color: gray; }
  li { cursor: pointer; margin: 4px 0; }
  .deleteBtn { cursor: pointer; margin-left: 8px; }
</style>
```
