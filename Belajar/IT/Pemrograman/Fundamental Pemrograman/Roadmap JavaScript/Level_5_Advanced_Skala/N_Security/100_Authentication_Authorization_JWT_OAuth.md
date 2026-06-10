# 100. Authentication & Authorization — JWT & OAuth 2.0

**Benang Merah**: Materi 99 membahas CSRF protection untuk mengamankan aksi user. Sekarang kita bahas **siapa user-nya** (Authentication) dan **apa yang boleh mereka lakukan** (Authorization). Dari cookie-based auth, kita beralih ke **token-based auth** (JWT) dan **delegated auth** (OAuth 2.0).

---

## Penjelasan

**Authentication (AuthN)** — "Siapa kamu?" Bukti identitas: login dengan email + password, Google OAuth, sidik jari.

**Authorization (AuthZ)** — "Apa yang boleh kamu lakukan?" Setelah dikenal, cek izin: user biasa bisa baca buku, admin bisa hapus buku.

### JWT (JSON Web Token)

JWT adalah token berbasis JSON yang berisi data user + signature digital. Struktur: `header.payload.signature`:
- **Header**: algoritma signing (HS256, RS256)
- **Payload**: data (userId, role, exp)
- **Signature**: verifikasi bahwa token tidak dimodifikasi

Alur JWT:
1. User login → server buat JWT → kirim ke client
2. Client simpan JWT (localStorage / httpOnly cookie)
3. Setiap request, client kirim JWT di header `Authorization: Bearer <token>`
4. Server verifikasi signature → jika valid, extract data user

**Access Token** (umur pendek, 15 menit) vs **Refresh Token** (umur panjang, 7 hari) — refresh token dipakai untuk minta access token baru tanpa login ulang.

### OAuth 2.0

OAuth 2.0 adalah protokol yang memungkinkan aplikasi pihak ketiga mengakses data user **tanpa melihat password user**. Contoh: "Login dengan Google".

Alur OAuth 2.0 (Authorization Code Grant):
1. User klik "Login with Google"
2. Redirect ke Google → user login di Google → Google kasih authorization code
3. Aplikasi kirim code ke server → server tukar code dengan access token
4. Server pakai access token untuk ambil data user dari Google API

### RBAC (Role-Based Access Control)

RBAC adalah sistem di mana setiap user punya **role** (ADMIN, USER, MODERATOR) dan setiap role punya **permission** tertentu. Cek role di setiap endpoint sensitif.

---

## Fungsi

- **AuthN**: Memastikan hanya user terdaftar yang bisa login
- **AuthZ**: Memastikan user hanya bisa mengakses resource sesuai role-nya
- **JWT**: Auth stateless (server tidak perlu simpan session)
- **OAuth 2.0**: Login tanpa password — pakai akun Google/GitHub yang sudah ada

---

## Code: Implementasi JWT + OAuth 2.0 + RBAC

```javascript
const express = require('express');
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();
const app = express();

app.use(express.json());

// =============================================
// JWT CONFIG
// =============================================

const ACCESS_SECRET = process.env.JWT_ACCESS_SECRET || 'access-secret-kunci';
const REFRESH_SECRET = process.env.JWT_REFRESH_SECRET || 'refresh-secret-kunci';
const ACCESS_EXPIRY = '15m';
const REFRESH_EXPIRY = '7d';

// =============================================
// HELPER — Generate JWT
// =============================================

function generateAccessToken(user) {
  return jwt.sign(
    { userId: user.id, role: user.role },
    ACCESS_SECRET,
    { expiresIn: ACCESS_EXPIRY }
  );
}

function generateRefreshToken(user) {
  return jwt.sign(
    { userId: user.id },
    REFRESH_SECRET,
    { expiresIn: REFRESH_EXPIRY }
  );
}

// =============================================
// MIDDLEWARE — Verify JWT
// =============================================

function authenticateToken(req, res, next) {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1]; // Bearer <token>

  if (!token) {
    return res.status(401).json({ error: 'Token tidak ditemukan' });
  }

  try {
    const decoded = jwt.verify(token, ACCESS_SECRET);
    req.user = decoded;
    next();
  } catch (err) {
    return res.status(403).json({ error: 'Token tidak valid atau expired' });
  }
}

// =============================================
// MIDDLEWARE — RBAC
// =============================================

function authorizeRole(...roles) {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Forbidden — tidak punya akses' });
    }
    next();
  };
}

// =============================================
// REGISTER & LOGIN (dengan bcrypt)
// =============================================

app.post('/auth/register', async (req, res) => {
  const { email, password, name } = req.body;

  // Validasi
  if (!email || !password || password.length < 8) {
    return res.status(400).json({ error: 'Password minimal 8 karakter' });
  }

  // Hash password
  const hashedPassword = await bcrypt.hash(password, 12);

  const user = await prisma.user.create({
    data: { email, password: hashedPassword, name, role: 'USER' },
  });

  const accessToken = generateAccessToken(user);
  const refreshToken = generateRefreshToken(user);

  res.status(201).json({ accessToken, refreshToken, user: { id: user.id, email: user.email, role: user.role } });
});

app.post('/auth/login', async (req, res) => {
  const { email, password } = req.body;

  const user = await prisma.user.findUnique({ where: { email } });
  if (!user) {
    return res.status(401).json({ error: 'Email/password salah' });
  }

  const valid = await bcrypt.compare(password, user.password);
  if (!valid) {
    return res.status(401).json({ error: 'Email/password salah' });
  }

  const accessToken = generateAccessToken(user);
  const refreshToken = generateRefreshToken(user);

  res.json({ accessToken, refreshToken });
});

// =============================================
// REFRESH TOKEN ENDPOINT
// =============================================

app.post('/auth/refresh', (req, res) => {
  const { refreshToken } = req.body;

  if (!refreshToken) {
    return res.status(401).json({ error: 'Refresh token diperlukan' });
  }

  try {
    const decoded = jwt.verify(refreshToken, REFRESH_SECRET);
    // Buat access token baru
    const accessToken = jwt.sign(
      { userId: decoded.userId },
      ACCESS_SECRET,
      { expiresIn: ACCESS_EXPIRY }
    );
    res.json({ accessToken });
  } catch (err) {
    return res.status(403).json({ error: 'Refresh token tidak valid' });
  }
});

// =============================================
// PROTECTED ENDPOINTS — Perpustakaan
// =============================================

// ✅ SEMUA user login bisa lihat buku
app.get('/api/books', authenticateToken, async (req, res) => {
  const books = await prisma.book.findMany();
  res.json(books);
});

// ✅ Hanya ADMIN yang bisa tambah buku
app.post('/api/books', authenticateToken, authorizeRole('ADMIN'), async (req, res) => {
  const { title, author, isbn } = req.body;
  const book = await prisma.book.create({ data: { title, author, isbn } });
  res.status(201).json(book);
});

// ✅ ADMIN & MODERATOR bisa hapus buku
app.delete('/api/books/:id', authenticateToken, authorizeRole('ADMIN', 'MODERATOR'), async (req, res) => {
  const id = parseInt(req.params.id);
  await prisma.book.delete({ where: { id } });
  res.status(204).end();
});

// =============================================
// OAuth 2.0 — Login dengan Google (Pseudocode)
// =============================================

/*
PASANGAN:
1. Daftar aplikasi di Google Cloud Console → dapat clientId & clientSecret
2. Set redirect URI: http://localhost:3000/auth/google/callback

IMPLEMENTASI:
*/

const axios = require('axios');
const GOOGLE_CLIENT_ID = process.env.GOOGLE_CLIENT_ID;
const GOOGLE_CLIENT_SECRET = process.env.GOOGLE_CLIENT_SECRET;
const REDIRECT_URI = 'http://localhost:3000/auth/google/callback';

// Step 1: Redirect ke Google
app.get('/auth/google', (req, res) => {
  const url = `https://accounts.google.com/o/oauth2/v2/auth?client_id=${GOOGLE_CLIENT_ID}&redirect_uri=${REDIRECT_URI}&response_type=code&scope=email%20profile`;
  res.redirect(url);
});

// Step 2: Google callback
app.get('/auth/google/callback', async (req, res) => {
  const { code } = req.query;

  // Tukar code dengan access token Google
  const tokenResponse = await axios.post('https://oauth2.googleapis.com/token', {
    code,
    client_id: GOOGLE_CLIENT_ID,
    client_secret: GOOGLE_CLIENT_SECRET,
    redirect_uri: REDIRECT_URI,
    grant_type: 'authorization_code',
  });

  const googleAccessToken = tokenResponse.data.access_token;

  // Ambil data user dari Google
  const userResponse = await axios.get('https://www.googleapis.com/oauth2/v2/userinfo', {
    headers: { Authorization: `Bearer ${googleAccessToken}` },
  });

  const { email, name } = userResponse.data;

  // Cek apakah user sudah ada di database
  let user = await prisma.user.findUnique({ where: { email } });
  if (!user) {
    user = await prisma.user.create({
      data: { email, name, password: '', role: 'USER' }, // password kosong karena OAuth
    });
  }

  // Buat JWT aplikasi kita
  const accessToken = generateAccessToken(user);
  const refreshToken = generateRefreshToken(user);

  res.json({ accessToken, refreshToken, user: { id: user.id, email: user.email, role: user.role } });
});

app.listen(3000);
```

---

## Analogi: Membangun Rumah (KTP Digital & Pakai KTP Pemerintah)

| Konsep | Analogi Rumah |
|---|---|
| **Authentication** | Satpam cek KTP — "Siapa Anda?" |
| **Authorization** | Satpam cek daftar tamu — "Anda boleh masuk ke area mana?" |
| **JWT** | KTP digital: foto + nama + tanda tangan digital. Tidak bisa dipalsukan. |
| **Access Token** | Kartu akses lift (berlaku 15 menit, harus perpanjang) |
| **Refresh Token** | Surat keterangan kerja (berlaku 7 hari, bisa minta kartu lift baru) |
| **OAuth 2.0** | "Login dengan Google" = pakai KTP yang dikeluarkan pemerintah pusat. Anda percaya pemerintah, jadi Anda percaya KTP itu. |
| **RBAC** | Warna kartu: **BIRU** = warga biasa (akses taman), **HIJAU** = petugas (akses ruang server), **MERAH** = ketua RT (akses semua ruangan) |

JWT = KTP digital yang Anda buat sendiri.
OAuth = Anda pakai KTP keluaran Dinas Kependudukan (Google).
RBAC = Setiap kartu punya warna berbeda yang menentukan akses.

---

## Dipakai Untuk Apa

- **API stateless auth** — microservices, REST API, GraphQL
- **Single sign-on (SSO)** — login dengan Google/GitHub/Facebook
- **Admin panel** — RBAC untuk membedakan akses admin vs user biasa
- **Mobile apps** — JWT lebih cocok daripada cookie untuk native apps
- **API Gateway** — verifikasi JWT di gateway, backend fokus pada bisnis

---

## Kesalahan Umum

| Kesalahan | Dampak |
|---|---|
| JWT secret disimpan di kode | Secret bocor ke git — siapa pun bisa bikin token palsu |
| Tidak pakai expiry di access token | Token bocor dipakai selamanya |
| Access token disimpan di localStorage | Rentan XSS — attacker bisa curi token |
| Tidak verifikasi signature JWT | Attacker bisa modifikasi payload (misal ganti role ke ADMIN) |
| Pakai role string tanpa RBAC middleware | Lupa cek role di satu endpoint = celah besar |
| OAuth callback tanpa validasi state | Rentan CSRF pada OAuth flow |
| Refresh token tanpa revoke | Logout tidak berguna — token lama tetap valid |

---

## Hubungan dengan Materi Sebelumnya

- **Materi 99 (XSS & CSRF)** → XSS bisa curi JWT dari localStorage. CSRF token melindungi OAuth callback.
- **Materi 97 (OWASP #1 Broken Access Control)** → RBAC mencegah user biasa akses endpoint admin.
- **Materi 101 (Hashing & Enkripsi)** → JWT menggunakan HMAC (hashing) untuk signature. Password di-hash pakai bcrypt.
- **Materi 59 (Express Middleware)** → `authenticateToken` dan `authorizeRole` adalah middleware Express.

---

## Soal Latihan

### Soal 1 (Mudah)
Apa perbedaan **Authentication** dan **Authorization**? Beri analogi rumah.

**Jawaban**:
- **Authentication (AuthN)** = "Siapa Anda?" Satpam cek KTP. Bukti identitas.
- **Authorization (AuthZ)** = "Apa yang boleh Anda lakukan?" Satpam cek: KTP Anda (AuthN) → dicek di daftar tamu: boleh masuk ke lantai 2 (AuthZ).

AuthN mendahului AuthZ. Anda harus dikenali dulu sebelum dicek izinnya.

### Soal 2 (Sedang)
Kode berikut tidak aman. Identifikasi 3 masalah keamanan dan perbaiki:

```javascript
const token = req.headers.authorization;
const decoded = jwt.decode(token);
req.user = decoded;
next();
```

**Jawaban**:
**Masalah**:
1. `jwt.decode()` tidak verifikasi signature! Attacker bisa bikin token palsu.
2. Tidak cek apakah header `Authorization` ada (token null → crash).
3. Tidak cek expiry (token expired tetap dianggap valid).

**Perbaikan**:
```javascript
function authenticateToken(req, res, next) {
  const authHeader = req.headers['authorization'];
  if (!authHeader) {
    return res.status(401).json({ error: 'Token tidak ditemukan' });
  }

  const token = authHeader.split(' ')[1]; // Bearer <token>
  if (!token) {
    return res.status(401).json({ error: 'Token tidak ditemukan' });
  }

  try {
    // ✅ verify — verifikasi signature + expiry
    const decoded = jwt.verify(token, ACCESS_SECRET);
    req.user = decoded;
    next();
  } catch (err) {
    if (err.name === 'TokenExpiredError') {
      return res.status(401).json({ error: 'Token expired' });
    }
    return res.status(403).json({ error: 'Token tidak valid' });
  }
}
```

### Soal 3 (Tantangan)
Implementasikan RBAC middleware yang bisa cek **izin spesifik** (bukan hanya role). Buat sistem permission seperti: `book:create`, `book:delete`, `user:read`. Setiap role punya daftar permission.

**Jawaban**:
```javascript
// =============================================
// PERMISSION-BASED RBAC
// =============================================

const ROLES = {
  ADMIN: ['book:create', 'book:read', 'book:delete', 'book:update', 'user:read', 'user:delete'],
  MODERATOR: ['book:create', 'book:read', 'book:update'],
  USER: ['book:read'],
};

function checkPermission(requiredPermission) {
  return (req, res, next) => {
    const userRole = req.user.role;

    // Cek apakah role miliki permission ini
    const permissions = ROLES[userRole];
    if (!permissions || !permissions.includes(requiredPermission)) {
      return res.status(403).json({
        error: `Forbidden — butuh permission: ${requiredPermission}`,
      });
    }

    next();
  };
}

// Usage:
// app.delete('/api/books/:id', authenticateToken, checkPermission('book:delete'), handler);
// app.post('/api/books', authenticateToken, checkPermission('book:create'), handler);

// =============================================
// DEMO
// =============================================

app.get('/api/books', authenticateToken, checkPermission('book:read'), async (req, res) => {
  const books = await prisma.book.findMany();
  res.json(books);
});

app.delete('/api/books/:id', authenticateToken, checkPermission('book:delete'), async (req, res) => {
  const id = parseInt(req.params.id);
  await prisma.book.delete({ where: { id } });
  res.status(204).end();
});

// User dengan role USER bisa lihat buku, tapi tidak bisa hapus.
// User dengan role ADMIN bisa semuanya.
```
