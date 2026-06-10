# 97. OWASP Top 10 — Ancaman Keamanan Web Paling Umum

**Benang Merah**: Selama Level 1-4 kita fokus **membuat aplikasi berfungsi**. Sekarang kita harus memastikan aplikasi itu **aman**. OWASP Top 10 adalah daftar ancaman keamanan web paling kritis — checklist wajib sebelum production.

---

## Penjelasan

OWASP (Open Web Application Security Project) adalah organisasi nirlaba yang menerbitkan **Top 10 Web Security Risks** — 10 celah keamanan paling umum dan berbahaya di aplikasi web. Ibarat **daftar pemeriksaan keselamatan gedung**: sebelum dihuni, pastikan tidak ada kabel terbuka, pintu darurat berfungsi, sprinkler aktif.

Top 10 OWASP (2021):

1. **Broken Access Control** — user bisa akses data orang lain
2. **Cryptographic Failures** — data sensitif tidak dienkripsi
3. **Injection** — SQL, NoSQL, OS command injection
4. **Insecure Design** — arsitektur yang tidak aman dari awal
5. **Security Misconfiguration** — server salah konfigurasi
6. **Vulnerable & Outdated Components** — pakai library usang
7. **Identification & Authentication Failures** — login lemah
8. **Software & Data Integrity Failures** — update tidak diverifikasi
9. **Security Logging & Monitoring Failures** — tidak ada log serangan
10. **Server-Side Request Forgery (SSRF)** — server dipaksa request ke internal

```javascript
// Contoh: Broken Access Control
// ❌ JELAS! User biasa bisa lihat data admin
app.get('/api/admin/users', (req, res) => {
  const users = db.getUsers(); // siapa pun bisa akses!
  res.json(users);
});

// ✅ AMAN! Cek role dulu
app.get('/api/admin/users', (req, res) => {
  if (req.user.role !== 'ADMIN') {
    return res.status(403).json({ error: 'Forbidden' });
  }
  const users = db.getUsers();
  res.json(users);
});
```

---

## Fungsi

Memberi **awareness dan panduan** tentang ancaman keamanan yang harus diantisipasi saat membangun aplikasi web. Bukan untuk membuat paranoid, tapi untuk membangun **defense-in-depth**.

---

## Cara Pengimplementasian (Mitigasi)

### 1. Injection Prevention
```javascript
// ❌ RENTAN SQL Injection
const query = `SELECT * FROM users WHERE email = '${req.body.email}'`;

// ✅ AMAN — parameterized query (Prisma ORM)
const user = await prisma.user.findUnique({
  where: { email: req.body.email }
});
```

### 2. XSS Prevention
```javascript
// ❌ RENTAN — innerHTML bisa dimasuki script
document.getElementById('output').innerHTML = userInput;

// ✅ AMAN — textContent tidak parse HTML
document.getElementById('output').textContent = userInput;
```

### 3. Helmet.js — Security Headers
```javascript
const helmet = require('helmet');
const app = express();
app.use(helmet()); // tambah security headers otomatis
```

### 4. Rate Limiting
```javascript
const rateLimit = require('express-rate-limit');
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 menit
  max: 100 // max 100 request per window
});
app.use(limiter);
```

### 5. Input Validation
```javascript
// ❌ Langsung pakai input user
const email = req.body.email;

// ✅ Validasi dulu
function isValidEmail(email) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}
if (!isValidEmail(req.body.email)) {
  return res.status(400).json({ error: 'Email tidak valid' });
}
```

---

## Analogi: Membangun Rumah (Inspeksi Keamanan)

| OWASP Top 10 | Inspeksi Keamanan Gedung |
|---|---|
| Broken Access Control | Pintu tanpa kunci — siapa pun bisa masuk |
| Injection | Pipa tanpa filter — kotoran masuk ke air bersih |
| XSS | Orang bisa tempel stiker beracun di papan pengumuman |
| Security Misconfig | Pintu darurat terkunci dari luar |
| Rate Limiting | Batasi jumlah tamu masuk per menit |

OWASP Top 10 seperti **daftar periksa keamanan gedung** sebelum dihuni. Bukan untuk membuat Anda takut, tapi untuk memastikan tidak ada celah yang bisa dimanfaatkan orang jahat.

---

## Dipakai Untuk Apa

- **Security audit** — memeriksa celah aplikasi sebelum launch
- **Code review checklist** — pastikan tidak ada OWASP violation
- **Penetration testing** — simulasi serangan
- **Security training** — belajar dari kesalahan orang lain
- **Compliance** — standar industri (PCI DSS, HIPAA)

---

## Kesalahan Umum

| Kesalahan | Dampak |
|---|---|
| "Aplikasi saya kecil, gak bakal di-hack" | Semua aplikasi jadi target bot |
| Hanya fokus di satu celah | Attacker cari celah TERLEMAH |
| Security dipikir belakangan | Retrofit lebih mahal dari design |
| Percaya 100% pada ORM | ORM mengurangi, tapi tidak menghilangkan risiko |

---

## Hubungan dengan Materi Sebelumnya

Security adalah **lapisan terakhir** yang melindungi semua kerja keras kita:
- Materi 58 (REST API) → Setiap endpoint bisa jadi vektor serangan
- Materi 61 (SQL) → Injection adalah ancaman #3 OWASP
- Materi 64 (Prisma) → ORM membantu mencegah injection
- Materi 77 (DOM) → XSS ancaman #2 di client-side
- Materi 98-102 → Kita akan perdalam mitigasi masing-masing

---

## Soal Latihan

### Soal 1 (Mudah)
Sebutkan 3 dari OWASP Top 10 dan beri contoh sederhana masing-masing.

**Jawaban**:
1. **Injection**: `SELECT * FROM users WHERE id = '1; DROP TABLE users'`
2. **Broken Access Control**: User biasa akses `GET /api/admin`
3. **XSS**: `<script>alert('Hacked')</script>` dimasukkan di kolom komentar

### Soal 2 (Sedang)
Kode berikut memiliki celah keamanan. Identifikasi dan perbaiki:
```javascript
app.get('/user/:id', (req, res) => {
  const user = db.query(`SELECT * FROM users WHERE id = ${req.params.id}`);
  res.json(user);
});
```

**Jawaban**:
- **Celah**: SQL Injection — `req.params.id` langsung dimasukkan ke query
- **Perbaikan**: Gunakan parameterized query
```javascript
app.get('/user/:id', (req, res) => {
  const user = db.query('SELECT * FROM users WHERE id = $1', [req.params.id]);
  res.json(user);
});
// Atau dengan ORM:
// const user = await prisma.user.findUnique({ where: { id: parseInt(req.params.id) } });
```

### Soal 3 (Tantangan)
Buat middleware Express sederhana untuk **rate limiting** (max 5 request per menit per IP). Simpan data di Map.

**Jawaban**:
```javascript
const rateLimitMap = new Map();

function rateLimiter(req, res, next) {
  const ip = req.ip;
  const now = Date.now();
  const windowMs = 60 * 1000; // 1 menit
  const maxReq = 5;

  if (!rateLimitMap.has(ip)) {
    rateLimitMap.set(ip, []);
  }

  const timestamps = rateLimitMap.get(ip).filter(t => now - t < windowMs);

  if (timestamps.length >= maxReq) {
    return res.status(429).json({ error: 'Too many requests' });
  }

  timestamps.push(now);
  rateLimitMap.set(ip, timestamps);
  next();
}

app.use(rateLimiter);
```
