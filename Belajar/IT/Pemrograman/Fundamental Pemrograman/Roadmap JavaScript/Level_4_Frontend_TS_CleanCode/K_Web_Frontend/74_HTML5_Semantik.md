# 74. HTML5 Semantik — Struktur Halaman Web

**Benang Merah**: Dari Level 3 (backend API dengan Express) kita butuh **"wajah"** — halaman web yang bisa dilihat pengguna. HTML adalah kerangka dasarnya. Lanjut ke Materi 75 (CSS) untuk styling.

---

## Penjelasan

HTML5 Semantik adalah cara menulis HTML menggunakan **tag yang bermakna** (semantic tags) — bukan sekadar `<div>` untuk segalanya. Tag semantik memberi arti pada struktur halaman, membantu browser, search engine (SEO), dan assistive technology (screen reader) memahami konten.

| Tag Non-Semantik | Tag Semantik |
|---|---|
| `<div class="header">` | `<header>` |
| `<div class="nav">` | `<nav>` |
| `<div class="main">` | `<main>` |
| `<div class="footer">` | `<footer>` |

**Form & Input** memungkinkan pengguna mengirim data. **Accessibility** (a11y) memastikan halaman bisa digunakan oleh semua orang, termasuk penyandang disabilitas.

---

## Fungsi

- Memberi **struktur hierarkis** pada halaman web
- Meningkatkan **SEO** (mesin pencari paham konten)
- Memudahkan **aksesibilitas** (screen reader navigasi lebih baik)
- **Form** sebagai pintu masuk data dari pengguna ke server

---

## Cara Pengimplementasian

### 1. Struktur Dasar HTML5 Semantik

```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Perpustakaan Online</title>
</head>
<body>
  <header>
    <h1>Perpustakaan Online</h1>
    <p>Baca. Pinjam. Kembalikan.</p>
  </header>

  <nav>
    <ul>
      <li><a href="/">Beranda</a></li>
      <li><a href="/buku">Daftar Buku</a></li>
      <li><a href="/tentang">Tentang</a></li>
    </ul>
  </nav>

  <main>
    <section id="buku-populer">
      <h2>Buku Populer</h2>
      <article>
        <h3>JavaScript: The Good Parts</h3>
        <p>Penulis: Douglas Crockford</p>
      </article>
      <article>
        <h3>Clean Code</h3>
        <p>Penulis: Robert C. Martin</p>
      </article>
    </section>

    <section id="form-peminjaman">
      <h2>Form Peminjaman</h2>
      <form action="/pinjam" method="POST">
        <div>
          <label for="nama">Nama Lengkap:</label>
          <input type="text" id="nama" name="nama" required
                 aria-describedby="nama-hint">
          <span id="nama-hint">Minimal 3 karakter</span>
        </div>

        <div>
          <label for="email">Email:</label>
          <input type="email" id="email" name="email" required>
        </div>

        <div>
          <label for="buku">Pilih Buku:</label>
          <select id="buku" name="buku">
            <option value="js-good-parts">JavaScript: The Good Parts</option>
            <option value="clean-code">Clean Code</option>
          </select>
        </div>

        <div>
          <label for="tanggal">Tanggal Pinjam:</label>
          <input type="date" id="tanggal" name="tanggal">
        </div>

        <button type="submit" aria-label="Kirim formulir peminjaman">Ajukan Peminjaman</button>
      </form>
    </section>
  </main>

  <aside>
    <h3>Info Perpustakaan</h3>
    <p>Jam buka: Senin-Jumat, 08:00-16:00</p>
  </aside>

  <footer>
    <p>&copy; 2026 Perpustakaan Online. All rights reserved.</p>
    <address>Jl. Buku Indah No. 1, Jakarta</address>
  </footer>
</body>
</html>
```

### 2. Accessibility (alt, label, aria-*)

```html
<!-- alt pada gambar -->
<img src="cover-buku.jpg" alt="Sampul buku JavaScript: The Good Parts">

<!-- label menghubungkan teks dengan input -->
<label for="search">Cari buku:</label>
<input type="text" id="search" name="search">

<!-- aria-label untuk tombol icon -->
<button aria-label="Tutup modal">X</button>

<!-- aria-describedby untuk petunjuk -->
<input type="password" aria-describedby="password-rules">
<span id="password-rules">Minimal 8 karakter, ada huruf besar</span>
```

### 3. Form Elements Lengkap

```html
<form>
  <!-- Text -->
  <input type="text" placeholder="Nama">

  <!-- Email dengan validasi otomatis -->
  <input type="email" required>

  <!-- Number dengan batasan -->
  <input type="number" min="1" max="10" step="1">

  <!-- Checkbox -->
  <input type="checkbox" id="setuju">
  <label for="setuju">Saya setuju syarat & ketentuan</label>

  <!-- Radio -->
  <input type="radio" name="jenis" value="fiksi" id="fiksi">
  <label for="fiksi">Fiksi</label>
  <input type="radio" name="jenis" value="nonfiksi" id="nonfiksi">
  <label for="nonfiksi">Non-Fiksi</label>

  <!-- Textarea -->
  <textarea rows="4" cols="50" placeholder="Catatan..."></textarea>

  <!-- Submit -->
  <button type="submit">Kirim</button>
</form>
```

---

## Analogi: Membangun Rumah (Rangka Rumah)

| HTML5 Semantik | Rangka Rumah |
|---|---|
| `<header>` | Atap rumah — identitas bangunan |
| `<nav>` | Pintu & rambu — jalan masuk ke ruangan |
| `<main>` | Ruang tamu — area utama aktivitas |
| `<section>` | Kamar — area spesifik dengan fungsi tertentu |
| `<article>` | Perabot di dalam kamar — konten mandiri |
| `<aside>` | Balkon / pojokan — informasi tambahan |
| `<footer>` | Pondasi — informasi dasar & legal |
| `<form>` | Kotak surat — pengirim pesan dari tamu |
| `alt` / `label` | Papan braille — aksesibel untuk semua penghuni |
| `aria-*` | Stiker petunjuk di setiap ruangan |
| Tag non-semantik (`<div>`) | Kardus bekas — bisa dipakai, tapi tidak ideal |

Bayangkan rumah tanpa ruang yang jelas — semua barang bertumpuk di satu ruangan besar. HTML semantik memberikan **denah yang rapi**: tamu (browser) tahu mana ruang tamu, mana dapur, mana kamar tidur. Screen reader bisa "berjalan" dari ruang ke ruang dengan lancar.

---

## Dipakai Untuk Apa

- **SEO** — Google memberi peringkat lebih tinggi pada halaman semantic
- **Screen reader** — pengguna tunanetra navigasi lebih mudah
- **Halaman web modern** — semua framework (Vue, React) pakai HTML5
- **Form** — login, registrasi, pencarian, checkout
- **Accessibility compliance** — memenuhi standar WCAG (Web Content Accessibility Guidelines)

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| `<div>` untuk semuanya | `<div class="header">` | Tidak bermakna, SEO jelek |
| Lupa `<label>` pada input | `<input>` tanpa `id` + `<label>` | Screen reader bingung |
| `<button>` di luar `<form>` untuk submit | `<button>` tanpa `type="submit"` | Form tidak terkirim |
| Tidak pakai `alt` di `<img>` | `<img src="foto.jpg">` | Gambar tidak terbaca screen reader |
| `aria-*` tanpa kebutuhan | `aria-label="Tombol"` pada `<button>Kirim</button>` | Redundan |

---

## Hubungan dengan Materi Sebelumnya

- Level 3 (Backend API) → butuh "wajah" untuk ditampilkan → HTML sebagai struktur
- Materi 75 (CSS) → HTML tanpa CSS itu "mentah" — CSS memberi styling
- Materi 77 (DOM Manipulation) → JavaScript memanipulasi DOM dari HTML
- Form → data dikirim ke backend API (Level 3)

---

## Soal Latihan

### Soal 1 (Mudah)
Buat struktur HTML5 semantik untuk halaman profil pribadi. Harus ada: header (foto profil + nama), nav (menu), main (bio + pengalaman), footer (kontak).

**Jawaban**:
```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Profil Saya</title>
</head>
<body>
  <header>
    <img src="foto.jpg" alt="Foto profil">
    <h1>Andi Pratama</h1>
    <p>Web Developer</p>
  </header>

  <nav>
    <ul>
      <li><a href="#bio">Bio</a></li>
      <li><a href="#pengalaman">Pengalaman</a></li>
      <li><a href="#kontak">Kontak</a></li>
    </ul>
  </nav>

  <main>
    <section id="bio">
      <h2>Bio</h2>
      <p>Frontend developer dengan 3 tahun pengalaman.</p>
    </section>

    <section id="pengalaman">
      <h2>Pengalaman</h2>
      <article>
        <h3>Frontend Dev di PT. Web</h3>
        <p>2024 - sekarang</p>
      </article>
    </section>
  </main>

  <footer>
    <p>Email: andi@email.com</p>
  </footer>
</body>
</html>
```

### Soal 2 (Sedang)
Buat form registrasi dengan: nama (text), email (email), password (password, min 8 karakter), jenis kelamin (radio), setuju syarat (checkbox).

**Jawaban**:
```html
<form>
  <div>
    <label for="name">Nama Lengkap:</label>
    <input type="text" id="name" name="name" required>
  </div>

  <div>
    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>
  </div>

  <div>
    <label for="pass">Password:</label>
    <input type="password" id="pass" name="pass"
           minlength="8" required
           aria-describedby="pass-hint">
    <span id="pass-hint">Minimal 8 karakter</span>
  </div>

  <fieldset>
    <legend>Jenis Kelamin</legend>
    <input type="radio" id="pria" name="gender" value="pria">
    <label for="pria">Pria</label>
    <input type="radio" id="wanita" name="gender" value="wanita">
    <label for="wanita">Wanita</label>
  </fieldset>

  <div>
    <input type="checkbox" id="setuju" required>
    <label for="setuju">Saya setuju syarat dan ketentuan</label>
  </div>

  <button type="submit">Daftar</button>
</form>
```

### Soal 3 (Tantangan)
Buat halaman artikel berita dengan struktur semantik lengkap. Gunakan `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>`. Artikel harus memiliki judul, penulis, tanggal, konten paragraf, dan sidebar terkait.

**Jawaban**:
```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Berita Teknologi</title>
</head>
<body>
  <header>
    <h1>TechNews</h1>
    <nav>
      <ul>
        <li><a href="/">Beranda</a></li>
        <li><a href="/teknologi">Teknologi</a></li>
        <li><a href="/sains">Sains</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <article>
      <header>
        <h2>JavaScript 2026: Fitur Baru yang Wajib Diketahui</h2>
        <p>Ditulis oleh <a href="/penulis/budi">Budi Santoso</a> | 10 Juni 2026</p>
      </header>

      <section>
        <h3>Pendahuluan</h3>
        <p>EcmaScript terus berkembang. Tahun ini ada beberapa fitur baru...</p>
      </section>

      <section>
        <h3>Fitur Utama</h3>
        <p>Pertama, pattern matching. Kedua, pipeline operator...</p>
      </section>
    </article>

    <aside>
      <h3>Artikel Terkait</h3>
      <ul>
        <li><a href="#">Sejarah JavaScript</a></li>
        <li><a href="#">TypeScript vs JavaScript</a></li>
      </ul>
    </aside>
  </main>

  <footer>
    <p>&copy; 2026 TechNews. All rights reserved.</p>
    <address>redaksi@technews.com</address>
  </footer>
</body>
</html>
```
