# 79. Fetch API & REST API Client

**Benang Merah**: Dari Materi 78 (event handling) kita bisa mendeteksi aksi user. Sekarang kita hubungkan frontend dengan **backend API dari Level 3** — ambil data dari server, tampilkan di DOM. Lanjut ke Materi 80 (Vue.js).

---

## Penjelasan

**Fetch API** adalah antarmuka JavaScript bawaan browser untuk membuat HTTP request. Dengan Fetch, frontend bisa:

- **GET** data dari server (daftar buku, profil user)
- **POST** data ke server (form peminjaman, registrasi)
- **PUT/PATCH** update data (edit profil)
- **DELETE** data (hapus buku)

```javascript
// GET request sederhana
fetch('https://api.perpustakaan.com/buku')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

### Async/Await — Cara Modern

```javascript
async function ambilBuku() {
  try {
    const response = await fetch('https://api.perpustakaan.com/buku');

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Gagal ambil data:', error);
  }
}
```

### CORS (Cross-Origin Resource Sharing)

Browser punya **kebijakan keamanan** — website A tidak bisa sembarangan fetch data dari website B. CORS adalah mekanisme server memberi izin:

```
# Response header dari server
Access-Control-Allow-Origin: http://localhost:5173
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type
```

---

## Fungsi

- **Menghubungkan frontend ke backend** — ambil & kirim data
- **SPA (Single Page Application)** — tanpa reload halaman
- **Integrasi API pihak ketiga** — cuaca, maps, payment gateway
- **Dynamic content** — data real-time tanpa refresh

---

## Cara Pengimplementasian

### 1. GET — Ambil Data dari API

```javascript
async function tampilkanBuku() {
  try {
    const response = await fetch('http://localhost:3000/api/buku');

    if (!response.ok) {
      throw new Error(`Gagal: ${response.status}`);
    }

    const buku = await response.json();
    renderBuku(buku);
  } catch (error) {
    console.error('Error fetch:', error);
    tampilkanError('Gagal memuat data buku');
  }
}

function renderBuku(bukuList) {
  const container = document.getElementById('bookList');
  container.innerHTML = '';

  bukuList.forEach(buku => {
    const card = document.createElement('div');
    card.className = 'card';
    card.innerHTML = `
      <h3>${buku.judul}</h3>
      <p>${buku.penulis}</p>
      <span class="kategori">${buku.kategori}</span>
    `;
    container.appendChild(card);
  });
}
```

### 2. POST — Kirim Data ke API

```javascript
const form = document.getElementById('pinjamForm');

form.addEventListener('submit', async (e) => {
  e.preventDefault();

  const data = {
    nama: document.getElementById('nama').value,
    email: document.getElementById('email').value,
    bukuId: document.getElementById('bukuId').value,
    tanggal: document.getElementById('tanggal').value
  };

  try {
    const response = await fetch('http://localhost:3000/api/pinjam', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(data)
    });

    if (!response.ok) {
      throw new Error(`Gagal: ${response.status}`);
    }

    const result = await response.json();
    alert(`Peminjaman berhasil! ID: ${result.id}`);
    form.reset();
  } catch (error) {
    console.error('Error:', error);
    alert('Gagal mengajukan peminjaman');
  }
});
```

### 3. PUT & DELETE

```javascript
// UPDATE — PUT
async function updateBuku(id, data) {
  const response = await fetch(`http://localhost:3000/api/buku/${id}`, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(data)
  });
  return response.json();
}

// DELETE
async function hapusBuku(id) {
  const response = await fetch(`http://localhost:3000/api/buku/${id}`, {
    method: 'DELETE'
  });

  if (response.ok) {
    console.log('Buku berhasil dihapus');
    // Hapus dari DOM
    document.querySelector(`[data-id="${id}"]`).remove();
  }
}
```

### 4. Frontend Perpustakaan Lengkap + Fetch API

```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Perpustakaan — Fetch API</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: sans-serif; background: #f5f6fa; min-height: 100vh; }
    header { background: #2c3e50; color: white; text-align: center; padding: 24px; }
    main { max-width: 1000px; margin: 24px auto; padding: 0 16px; }
    .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 16px; }
    .card { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); }
    .card h3 { margin-bottom: 4px; }
    .card p { color: #666; }
    .kategori { display: inline-block; background: #3498db; color: white; padding: 2px 10px; border-radius: 20px; font-size: 0.8em; margin-top: 8px; }
    .loading, .error { text-align: center; padding: 40px; font-size: 1.2em; }
    .error { color: #e74c3c; }
  </style>
</head>
<body>
  <header>
    <h1>Perpustakaan Online</h1>
    <p>Data dari API — Fetch & DOM</p>
  </header>

  <main>
    <div id="loading" class="loading">Memuat data...</div>
    <div id="error" class="error" style="display:none"></div>
    <div id="bookGrid" class="grid"></div>
  </main>

  <script>
    const API_URL = 'http://localhost:3000/api/buku';

    async function loadBooks() {
      const loading = document.getElementById('loading');
      const errorDiv = document.getElementById('error');
      const grid = document.getElementById('bookGrid');

      try {
        const response = await fetch(API_URL);

        if (!response.ok) {
          throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }

        const books = await response.json();

        loading.style.display = 'none';
        errorDiv.style.display = 'none';
        renderBooks(books);
      } catch (err) {
        loading.style.display = 'none';
        errorDiv.style.display = 'block';
        errorDiv.textContent = `Gagal memuat data: ${err.message}`;
      }
    }

    function renderBooks(books) {
      const grid = document.getElementById('bookGrid');
      grid.innerHTML = books.map(buku => `
        <div class="card" data-id="${buku.id}">
          <h3>${buku.judul}</h3>
          <p>${buku.penulis}</p>
          <span class="kategori">${buku.kategori || 'Umum'}</span>
        </div>
      `).join('');
    }

    loadBooks();
  </script>
</body>
</html>
```

---

## Analogi: Membangun Rumah (Telepon ke Toko Material)

| Fetch API | Telepon ke Toko Material |
|---|---|
| `fetch(url)` | Menelepon toko — "Saya mau pesan..." |
| HTTP Request (GET) | "Tolong kirimkan katalog produk" |
| HTTP Response | Toko kirim barang via kurir |
| `response.json()` | Membuka kotak dan mengecek isinya |
| `await` | Menunggu kurir sampai — jangan tinggal dulu |
| `try/catch` | Kurir kecelakaan? Cari solusi lain |
| `status 200 OK` | Barang sampai dengan selamat |
| `status 404` | Barang yang diminta tidak ada |
| `status 500` | Toko kebakaran — server error |
| POST | "Saya mau pesan barang ini, dikirim ke rumah" |
| PUT | "Tukar barang yang sudah dikirim dengan yang lain" |
| DELETE | "Batalkan pesanan saya" |
| CORS | Toko hanya melayani pelanggan yang terdaftar |
| Render ke DOM | Memasang barang di rumah — furnitur di ruang tamu |

**Proses lengkap**: Anda (frontend) menelepon toko (server), minta katalog (GET), toko kirim katalog (response JSON), Anda buka kotak (parse JSON), lalu pasang furnitur di rumah (render ke DOM). Tanpa Fetch API, Anda harus pergi langsung ke toko (reload halaman).

---

## Dipakai Untuk Apa

- **Menampilkan data dari database** — user, produk, artikel
- **Form submission** — kirim data registrasi, login, order
- **Autocomplete / search** — cari data saat mengetik
- **Real-time updates** — polling API tiap beberapa detik
- **Integrasi API pihak ketiga** — Google Maps, OpenAI, GitHub API

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Lupa `await` | `const data = fetch(url).json()` | Promise, bukan data |
| Tidak handle error | `fetch(url)` tanpa `.catch()` | Aplikasi crash jika server down |
| Lupa `Content-Type` | POST tanpa header JSON | Server tidak bisa parse body |
| Tidak cek `response.ok` | Asumsi response selalu 200 | Error tidak tertangani |
| CORS error | Fetch dari domain beda tanpa izin | Browser blokir request |
| Memory leak | Fetch di loop tanpa cleanup | Request menumpuk |

---

## Hubungan dengan Materi Sebelumnya

- **Level 3 (Backend API)** — Fetch API adalah client untuk backend yang kita buat
- Materi 77 (DOM) → Data dari API di-render ke DOM
- Materi 78 (Event) → Event submit form → fetch POST
- Materi 80 (Vue) → Vue menggantikan DOM manual + fetch lebih rapi

---

## Soal Latihan

### Soal 1 (Mudah)
Buat fungsi fetch GET untuk mengambil daftar user dari `https://jsonplaceholder.typicode.com/users`. Cetak nama-nama user ke console.

**Jawaban**:
```javascript
async function getUsers() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users');
    const users = await response.json();
    users.forEach(user => console.log(user.name));
  } catch (error) {
    console.error('Gagal:', error);
  }
}

getUsers();
```

### Soal 2 (Sedang)
Buat fetch POST untuk mengirim data ke `https://jsonplaceholder.typicode.com/posts`. Data: `{ title: 'foo', body: 'bar', userId: 1 }`. Tampilkan response ID yang dikembalikan server.

**Jawaban**:
```javascript
async function createPost() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        title: 'foo',
        body: 'bar',
        userId: 1
      })
    });

    const result = await response.json();
    console.log('Post berhasil dibuat, ID:', result.id);
  } catch (error) {
    console.error('Gagal:', error);
  }
}

createPost();
```

### Soal 3 (Tantangan)
Buat halaman yang menampilkan daftar post dari API `https://jsonplaceholder.typicode.com/posts`. Tampilkan dalam card: title (bold), body. Tambahkan input filter yang memfilter post berdasarkan title saat diketik. Tangani error jika fetch gagal.

**Jawaban**:
```html
<input type="text" id="filterInput" placeholder="Filter posts...">
<div id="postsContainer">Loading...</div>

<script>
  const API = 'https://jsonplaceholder.typicode.com/posts';
  const input = document.getElementById('filterInput');
  const container = document.getElementById('postsContainer');
  let allPosts = [];

  async function loadPosts() {
    try {
      const res = await fetch(API);
      if (!res.ok) throw new Error('Gagal load posts');
      allPosts = await res.json();
      renderPosts(allPosts);
    } catch (err) {
      container.innerHTML = `<p style="color:red">Error: ${err.message}</p>`;
    }
  }

  function renderPosts(posts) {
    container.innerHTML = posts.map(post => `
      <div style="background:#fff;padding:12px;margin:8px 0;border-radius:6px;box-shadow:0 1px 4px rgba(0,0,0,0.1)">
        <strong>${post.title}</strong>
        <p style="color:#666;margin-top:4px">${post.body}</p>
      </div>
    `).join('');
  }

  input.addEventListener('input', () => {
    const q = input.value.toLowerCase();
    const filtered = allPosts.filter(p => p.title.toLowerCase().includes(q));
    renderPosts(filtered);
  });

  loadPosts();
</script>
```
