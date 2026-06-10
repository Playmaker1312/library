# 99. XSS & CSRF — Serangan Client-Side

**Benang Merah**: Materi 98 membahas injection ke server (SQL). Sekarang kita bahas **serangan dari sisi client**: XSS (inject script berbahaya ke halaman web) dan CSRF (memaksa user melakukan aksi tanpa sadar). Dari validasi input server-side, kita beralih ke **pertahanan client-side + browser security**.

---

## Penjelasan

### XSS (Cross-Site Scripting)

XSS adalah celah di mana attacker menyisipkan **script berbahaya** ke halaman web yang dilihat user lain. Script itu jalan di **browser korban** dengan akses penuh ke cookies, localStorage, dan DOM.

Tiga jenis XSS:
1. **Stored XSS** — script disimpan di database, dimuat setiap user buka halaman (paling berbahaya)
2. **Reflected XSS** — script dari URL/query string, langsung di-render tanpa disimpan
3. **DOM-based XSS** — script murni dari sisi client (JavaScript memanipulasi DOM dari input user)

**CSP (Content Security Policy)** adalah header HTTP yang memberi tahu browser sumber script mana yang diperbolehkan. Ini seperti **daftar putih** — hanya script dari domain terpercaya yang jalan.

### CSRF (Cross-Site Request Forgery)

CSRF adalah serangan di mana attacker memaksa user yang sudah login untuk menjalankan aksi tertentu (transfer uang, ganti password) tanpa sepengetahuan user.

Cara kerja: user login ke bank.com → dapat cookie. User buka situs jahat → situs jahat kirim request ke bank.com dengan cookie otomatis terbawa.

**Mitigasi CSRF**:
1. **CSRF Token** — token unik di setiap form yang dicek server
2. **SameSite Cookie** — `SameSite=Strict` atau `SameSite=Lax` mencegah cookie dikirim dari situs lain
3. **Same Origin Check** — cek header `Origin` / `Referer`

---

## Fungsi

Melindungi pengguna dari serangan yang memanfaatkan **kepercayaan browser**:
- XSS: mencegah script jahat jalan di browser korban
- CSRF: mencegah request palsu mengatasnamakan user

---

## Code: Demonstrasi XSS + Proteksi + Helmet CSP

```javascript
const express = require('express');
const helmet = require('helmet');
const crypto = require('crypto');
const app = express();

app.use(express.urlencoded({ extended: true }));

// =============================================
// HELMET — Security Headers OTOMATIS
// =============================================

app.use(helmet()); // Termasuk CSP, X-Frame-Options, dll

// =============================================
// CSP KUSTOM
// =============================================

app.use(
  helmet.contentSecurityPolicy({
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'", "https://trusted-cdn.com"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      imgSrc: ["'self'", "data:"],
      objectSrc: ["'none'"],
      upgradeInsecureRequests: [],
    },
  })
);

// =============================================
// DEMO XSS — Tanpa Proteksi
// =============================================

// ❌ RENTAN XSS — langsung render input user
app.get('/xss-demo-vulnerable', (req, res) => {
  const name = req.query.name || 'Tamu';
  res.send(`
    <html>
      <body>
        <h1>Selamat datang, ${name}!</h1>
        <p>Ini rentan XSS — coba?name=<script>alert('XSS')</script></p>
      </body>
    </html>
  `);
});

// ✅ AMAN — escape output
function escapeHtml(str) {
  return str
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;')
    .replace(/'/g, '&#039;');
}

app.get('/xss-demo-safe', (req, res) => {
  const name = escapeHtml(req.query.name || 'Tamu');
  res.send(`
    <html>
      <body>
        <h1>Selamat datang, ${name}!</h1>
        <p>Ini aman — coba?name=<script>alert('XSS')</script></p>
      </body>
    </html>
  `);
});

// =============================================
// CSRF TOKEN IMPLEMENTATION
// =============================================

// Simpan CSRF token di session (sederhana)
const csrfTokens = new Map();

function generateCsrfToken(userId) {
  const token = crypto.randomBytes(32).toString('hex');
  csrfTokens.set(userId, token);
  return token;
}

function csrfProtection(req, res, next) {
  const token = req.body._csrf || req.headers['x-csrf-token'];
  const userId = req.body.userId || req.session?.userId;

  if (!token || csrfTokens.get(userId) !== token) {
    return res.status(403).json({ error: 'CSRF token tidak valid' });
  }
  next();
}

// Form dengan CSRF token
app.get('/transfer', (req, res) => {
  const userId = req.session?.userId || 'anonymous';
  const token = generateCsrfToken(userId);
  res.send(`
    <form action="/transfer" method="POST">
      <input type="hidden" name="_csrf" value="${token}">
      <input type="hidden" name="userId" value="${userId}">
      Rekening tujuan: <input name="to"><br>
      Jumlah: <input name="amount" type="number"><br>
      <button type="submit">Transfer</button>
    </form>
  `);
});

// ✅ AMAN — dicek CSRF token
app.post('/transfer', csrfProtection, (req, res) => {
  const { to, amount } = req.body;
  // Proses transfer...
  res.send('Transfer berhasil!');
});

// =============================================
// SAMESITE COOKIE
// =============================================

app.use((req, res, next) => {
  // Set cookie dengan SameSite=Strict
  res.cookie('sessionId', 'abc123', {
    httpOnly: true,
    secure: true,
    sameSite: 'strict', // atau 'lax'
    maxAge: 3600000,
  });
  next();
});

// =============================================
// DEMO STORED XSS — Komentar
// =============================================

let comments = [];

app.post('/comment', (req, res) => {
  // ✅ Sanitasi sebelum disimpan
  const text = escapeHtml(req.body.text || '');
  comments.push({ text, date: new Date() });
  res.redirect('/comments');
});

app.get('/comments', (req, res) => {
  const list = comments.map(
    c => `<li>${c.text} <small>(${c.date.toISOString()})</small></li>`
  ).join('');
  res.send(`<html><body><ul>${list}</ul><a href="/comment-form">Tambah komentar</a></body></html>`);
});

app.listen(3000);
```

```html
<!-- ============================================= -->
<!-- DEMO XSS DI BROWSER — BUKA index.html -->
<!-- ============================================= -->
<!DOCTYPE html>
<html>
<head>
  <!-- ✅ CSP lewat meta tag -->
  <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'">
</head>
<body>
  <h1>Demo XSS</h1>

  <!-- ❗ RENTAN — innerHTML -->
  <div id="vulnerable"></div>

  <!-- ✅ AMAN — textContent -->
  <div id="safe"></div>

  <script>
    // Simulasi input user
    const userInput = '<img src=x onerror="alert(\'XSS\')">';

    // ❌ RENTAN
    document.getElementById('vulnerable').innerHTML = userInput;
    // Script jalan! 🚨

    // ✅ AMAN
    document.getElementById('safe').textContent = userInput;
    // Tampil sebagai teks biasa — img tidak di-render
  </script>
</body>
</html>
```

---

## Analogi: Membangun Rumah (Papan Pengumuman & Cek)

| Konsep | Analogi Rumah |
|---|---|
| **Stored XSS** | Orang tempel stiker berisi pesan berbahaya di papan pengumuman. Setiap orang yang baca papan itu kena dampaknya. |
| **Reflected XSS** | Orang kirim surat berisi pesan berbahaya — hanya pembaca surat itu yang kena. |
| **CSP** | Peraturan rumah: hanya stiker resmi dari pengurus yang boleh ditempel di papan. |
| **CSRF** | Orang suruh Anda tanda tangan cek padahal Anda tidak sadar — Anda tanda tangan karena badan Anda yang bergerak, bukan kemauan Anda. |
| **CSRF Token** | Setiap cek punya nomor unik yang hanya Anda tahu — orang lain tidak bisa membuat cek palsu. |
| **SameSite Cookie** | KTP Anda hanya berlaku di dalam kompleks perumahan — tidak bisa dipakai di luar. |

XSS = stiker beracun di papan pengumuman.
CSRF = Anda disuruh tanda tangan cek tanpa sadar, tangan Anda bergerak sendiri.

---

## Dipakai Untuk Apa

- **Aplikasi web dengan input user** — komentar, forum, profile bio
- **Sistem dengan cookie/session** — banking, e-commerce, admin panel
- **Single Page Application** — rentan DOM-based XSS jika tidak hati-hati
- **Form aksi penting** — transfer, hapus akun, ganti password

---

## Kesalahan Umum

| Kesalahan | Dampak |
|---|---|
| Pakai `innerHTML` / `dangerouslySetInnerHTML` | Langsung membuka celah XSS |
| Tidak sanitasi input di server | Stored XSS — semua user terpapar |
| Hanya sanitasi client-side | Attacker bypass pakai curl |
| Tidak pakai CSRF token di form penting | CSRF — user bisa transfer uang tanpa sadar |
| SameSite cookie tidak diset | Cookie dikirim dari situs lain |
| Percaya CSP sudah cukup | CSP mengurangi risiko tapi tidak 100% mencegah |

---

## Hubungan dengan Materi Sebelumnya

- **Materi 98 (Validation & SQL Injection)** → Sanitasi juga mencegah XSS. Dari injection server ke client-side.
- **Materi 97 (OWASP #3 Injection)** → XSS adalah salah satu bentuk injection (HTML injection).
- **Materi 77 (DOM Manipulasi)** → `innerHTML` vs `textContent` adalah garis depan XSS.
- **Materi 100 (JWT & OAuth)** → CSRF token sering dipakai bareng JWT untuk endpoint sensitif.

---

## Soal Latihan

### Soal 1 (Mudah)
Jelaskan perbedaan **Stored XSS** dan **Reflected XSS**. Mana yang lebih berbahaya?

**Jawaban**:
- **Stored XSS**: Script disimpan di database, semua user yang buka halaman kena. Contoh: komentar berisi `<script>` di kolom komentar blog.
- **Reflected XSS**: Script dari URL, hanya user yang klik link tertentu yang kena. Contoh: `<script>` di query string `?search=` yang langsung di-render.
- **Lebih berbahaya**: Stored XSS, karena menjangkau lebih banyak korban tanpa perlu trik sosial engineering.

### Soal 2 (Sedang)
Aplikasi Express.js berikut rentan CSRF. Identifikasi celahnya dan perbaiki dengan CSRF token:

```javascript
app.post('/delete-account', (req, res) => {
  const userId = req.cookies.userId;
  db.query('DELETE FROM users WHERE id = $1', [userId]);
  res.send('Akun dihapus');
});
```

**Jawaban**:
**Celah**: Tidak ada CSRF protection — attacker bisa buat form di situs lain yang auto-submit ke `/delete-account`. Cookie userId terbawa otomatis.

**Perbaikan**:
```javascript
const crypto = require('crypto');
const csrfTokens = new Map();

function generateToken(userId) {
  const token = crypto.randomBytes(32).toString('hex');
  csrfTokens.set(userId, token);
  return token;
}

function csrfMiddleware(req, res, next) {
  const token = req.body._csrf || req.headers['x-csrf-token'];
  const userId = req.cookies.userId;

  if (!token || csrfTokens.get(userId) !== token) {
    return res.status(403).json({ error: 'CSRF token tidak valid' });
  }
  next();
}

// Sebelum form delete-account
app.get('/delete-form', (req, res) => {
  const token = generateToken(req.cookies.userId);
  res.send(`<form method="POST" action="/delete-account">
    <input type="hidden" name="_csrf" value="${token}">
    <button type="submit">Hapus akun saya</button>
  </form>`);
});

app.post('/delete-account', csrfMiddleware, (req, res) => {
  const userId = req.cookies.userId;
  db.query('DELETE FROM users WHERE id = $1', [userId]);
  res.send('Akun dihapus');
});
```

### Soal 3 (Tantangan)
Buat middleware Express untuk **CSP reporting**. Log semua pelanggaran CSP yang dilaporkan browser ke konsol. Lalu set header CSP yang melaporkan ke endpoint `/csp-report`.

**Jawaban**:
```javascript
const express = require('express');
const helmet = require('helmet');
const app = express();

app.use(express.json({ type: 'application/csp-report' }));

// Endpoint untuk menerima laporan CSP
app.post('/csp-report', (req, res) => {
  const report = req.body?.['csp-report'];
  if (report) {
    console.log('⚠️  CSP Violation:', {
      violatedDirective: report['violated-directive'],
      blockedURI: report['blocked-uri'],
      documentURI: report['document-uri'],
      timestamp: new Date().toISOString(),
    });
  }
  res.status(204).end();
});

// Middleware CSP dengan report
app.use(
  helmet.contentSecurityPolicy({
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'"],
      reportUri: '/csp-report',
    },
    reportOnly: false, // true = hanya lapor, tidak blokir
  })
);

// Root
app.get('/', (req, res) => {
  res.send(`
    <html>
      <body>
        <h1>CSP Report Demo</h1>
        <script>alert('inline script — akan diblokir CSP')</script>
      </body>
    </html>
  `);
});

app.listen(3000);
// Output di console:
// ⚠️  CSP Violation: { violatedDirective: 'script-src', blockedURI: '', documentURI: 'http://localhost:3000/' }
```
