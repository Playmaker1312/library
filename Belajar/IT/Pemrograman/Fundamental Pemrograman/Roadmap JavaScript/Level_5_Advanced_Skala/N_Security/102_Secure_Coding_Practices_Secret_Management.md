# 102. Secure Coding Practices & Secret Management

**Benang Merah**: Materi 97-101 membahas ancaman (OWASP) dan mitigasi spesifik (validasi, XSS, JWT, hashing). Sekarang kita rangkum semua **praktik secure coding** sebagai SOP keamanan menyeluruh — dan bagaimana **mengelola secrets** (API key, password, token) tanpa bocor ke git.

---

## Penjelasan

**Secure Coding** adalah praktik menulis kode dengan mempertimbangkan keamanan sejak awal, bukan sebagai afterthought. Ini mencakup:

1. **Least Privilege** — beri akses minimal yang diperlukan
2. **Secret Management** — jangan simpan secrets di kode/git
3. **Rate Limiting & Brute Force Protection** — batasi percobaan login
4. **Security Headers** — Helmet.js mengamankan HTTP response
5. **Audit Logging** — catat semua aksi penting (siapa, kapan, apa)
6. **Defense in Depth** — banyak lapisan pertahanan, jangan bergantung pada satu lapis

**Secret Management** adalah praktik menyimpan dan mengakses informasi sensitif (API key, database password, JWT secret, encryption key) dengan aman:
- Jangan di kode → `.env` atau vault
- Jangan di git → `.gitignore`
- Rotasi berkala
- Akses terbatas (hanya service yang perlu)

---

## Fungsi

Membangun **budaya keamanan** dalam kode: setiap developer otomatis menulis kode yang aman, bukan karena dipaksa, tapi karena praktiknya sudah menjadi kebiasaan.

---

## Code: Hardening Penuh — Helmet, Rate Limit, Secret Management, Audit Log

```javascript
const express = require('express');
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');
const jwt = require('jsonwebtoken');
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();

// =============================================
// 1. SECRET MANAGEMENT
// =============================================
//
// 🔴 JANGAN PERNAH lakukan ini:
// const JWT_SECRET = 'my-super-secret-key-123';
//
// ✅ AMAN — dari environment variable:
const JWT_SECRET = process.env.JWT_SECRET;
const DB_URL = process.env.DATABASE_URL;
const ENCRYPTION_KEY = process.env.ENCRYPTION_KEY;

// Validasi secrets di startup
if (!JWT_SECRET || !DB_URL) {
  console.error('❌ MISSING REQUIRED ENVIRONMENT VARIABLES');
  process.exit(1); // Gagal startup jika secret tidak ada
}

// =============================================
// 2. HELMET — SECURITY HEADERS
// =============================================

const app = express();
app.use(express.json());

// Helmet set 15+ security headers otomatis:
// - X-Content-Type-Options: nosniff
// - X-Frame-Options: DENY (cegah clickjacking)
// - Strict-Transport-Security (paksa HTTPS)
// - X-XSS-Protection
// - dll
app.use(helmet());

// Kustom CSP jika perlu
app.use(
  helmet.contentSecurityPolicy({
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      imgSrc: ["'self'", "data:"],
    },
  })
);

// =============================================
// 3. RATE LIMITING — BRUTE FORCE PROTECTION
// =============================================

// Global limiter — semua endpoint
const globalLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 menit
  max: 100, // max 100 request per IP per window
  standardHeaders: true,
  legacyHeaders: false,
  message: { error: 'Terlalu banyak request, coba lagi nanti' },
});
app.use(globalLimiter);

// Strict limiter — khusus login (brute force protection)
const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 menit
  max: 5, // max 5 percobaan login
  message: { error: 'Terlalu banyak percobaan login. Coba 15 menit lagi' },
  skipSuccessfulRequests: true, // jangan hitung login berhasil
});

// =============================================
// 4. LEAST PRIVILEGE — Database User
// =============================================
//
// ❌ JANGAN pakai root/admin user untuk aplikasi
// ✅ Buat user khusus dengan akses minimal:
//    - hanya bisa SELECT/INSERT/UPDATE/DELETE
//    - hanya di schema/tabel yang diperlukan
//    - tidak bisa CREATE/DROP/ALTER tabel
//
// Contoh SQL:
// CREATE USER app_user WITH PASSWORD 'secure-password';
// GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO app_user;

// =============================================
// 5. AUDIT LOGGING
// =============================================

async function logAudit(userId, action, details, req) {
  // Catat ke database
  await prisma.auditLog.create({
    data: {
      userId,
      action,           // 'LOGIN', 'DELETE_BOOK', 'CHANGE_PASSWORD'
      details,          // JSON dengan info tambahan
      ipAddress: req.ip,
      userAgent: req.headers['user-agent'],
      timestamp: new Date(),
    },
  });

  // Juga log ke console (struktur untuk log aggregator seperti ELK)
  console.log(JSON.stringify({
    level: 'audit',
    userId,
    action,
    details,
    ip: req.ip,
    time: new Date().toISOString(),
  }));
}

// =============================================
// 6. IMPLEMENTASI — Auth dengan semua proteksi
// =============================================

// Login — dengan rate limiter STRICT
app.post('/auth/login', loginLimiter, async (req, res) => {
  const { email, password } = req.body;

  // Validasi
  if (!email || !password) {
    return res.status(400).json({ error: 'Email dan password diperlukan' });
  }

  const user = await prisma.user.findUnique({ where: { email } });
  if (!user) {
    await logAudit(null, 'LOGIN_FAILED', { email, reason: 'user_not_found' }, req);
    return res.status(401).json({ error: 'Email/password salah' });
  }

  const valid = await bcrypt.compare(password, user.password);
  if (!valid) {
    await logAudit(user.id, 'LOGIN_FAILED', { reason: 'wrong_password' }, req);
    return res.status(401).json({ error: 'Email/password salah' });
  }

  const token = jwt.sign({ userId: user.id, role: user.role }, JWT_SECRET, {
    expiresIn: '15m',
  });

  // Audit log — login berhasil
  await logAudit(user.id, 'LOGIN_SUCCESS', {}, req);

  res.json({ token });
});

// Hapus buku — dengan audit log
app.delete('/api/books/:id', authenticateToken, authorizeRole('ADMIN'), async (req, res) => {
  const id = parseInt(req.params.id);

  const book = await prisma.book.findUnique({ where: { id } });
  if (!book) {
    return res.status(404).json({ error: 'Buku tidak ditemukan' });
  }

  await prisma.book.delete({ where: { id } });

  // Audit log — hapus buku
  await logAudit(req.user.userId, 'DELETE_BOOK', { bookId: id, title: book.title }, req);

  res.status(204).end();
});

// =============================================
// 7. ERROR HANDLING — Jangan bocorkan detail
// =============================================

// ❌ JANGAN:
// catch (err) { res.status(500).json({ error: err.message }) }
// — err.message bisa bocor stack trace, path server, dll

// ✅ AMAN:
app.use((err, req, res, next) => {
  console.error('❌ Internal error:', err);
  res.status(500).json({
    error: 'Terjadi kesalahan internal',
    requestId: req.id, // untuk tracking tanpa bocor detail
  });
});

// =============================================
// 8. .gitignore — Jangan commit secrets!
// =============================================
//
// Tambahkan di .gitignore:
// .env
// *.log
// ssl/
// node_modules/
// config/production.json

// =============================================
// 9. STARTUP — dengan validasi
// =============================================

app.listen(3000, () => {
  console.log('🔒 Server aman berjalan di port 3000');
  console.log('📋 Helmet aktif — security headers terpasang');
  console.log('🚦 Rate limiter aktif');
  console.log('📝 Audit logging aktif');
});
```

```bash
# =============================================
# Contoh .env FILE (JANGAN DI-COMMIT!)
# =============================================

# === Database ===
DATABASE_URL="postgresql://app_user:secure-password@localhost:5432/library?schema=public"

# === JWT ===
JWT_SECRET="a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b"
JWT_REFRESH_SECRET="b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2"

# === Encryption ===
ENCRYPTION_KEY="c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3"

# === OAuth ===
GOOGLE_CLIENT_ID="123456789-abc123.apps.googleusercontent.com"
GOOGLE_CLIENT_SECRET="GOCSPX-abc123secret"

# === Environment ===
NODE_ENV="production"
```

---

## Analogi: Membangun Rumah (SOP Keamanan Gedung)

| Konsep | Analogi Rumah |
|---|---|
| **Least Privilege** | Tukang listrik hanya boleh di ruang panel listrik, tidak ke kamar tidur. |
| **Secret Management** | Kunci utama gedung disimpan di brankas manajer — bukan ditempel di papan pengumuman. |
| **Rate Limiting** | Pintu putar — hanya 1 orang masuk per 3 detik. Mencegah 100 orang masuk serempak. |
| **Helmet / Security Headers** | CCTV, lampu sensor, alarm — semua standar keamanan yang aktif 24/7. |
| **Audit Log** | Buku tamu — setiap orang yang masuk/keluar dicatat: nama, jam, keperluan. |
| **Defense in Depth** | Pagar → pintu utama → alarm → CCTV → brankas. Jika satu lapis tembus, masih ada lapisan lain. |
| **Error Handling** | Jika alarm bunyi, yang terdengar di dalam gedung cuma "terjadi gangguan" — bukan "alarm di lantai 3 ruang 304 mati." |
| **Input Validation** | Security check di pintu — KTP, scan body, metal detector. |

SOP keamanan gedung: ada alarm (Helmet), antrean masuk (rate limit), kunci rahasia (secret management), buku tamu (audit log), dan banyak lapisan (defense in depth).

---

## Dipakai Untuk Apa

- **Semua aplikasi production** — hardening wajib sebelum deploy
- **CI/CD pipeline** — scan secrets di git pre-commit (git-secrets, truffleHog)
- **Compliance** — PCI DSS, HIPAA, GDPR butuh audit log & secret management
- **Microservices** — setiap service punya secret sendiri, akses terbatas
- **Open source project** — contoh `.env.example` tanpa nilai asli

---

## Kesalahan Umum

| Kesalahan | Dampak |
|---|---|
| Secret di hardcode / git history | Bobol selamanya — ganti semua secret & key |
| Tidak ada rate limiting | Brute force login berhasil dalam hitungan menit |
| Tidak ada audit log | Tidak tahu siapa yang bobol & kapan |
| Hanya satu lapis keamanan | Jika tembus, semua data terbuka |
| Error message bocor detail server | Attacker dapat info untuk serangan lanjutan |
| Database pakai root user | Jika SQL injection terjadi, attacker punya akses penuh |
| .env di-commit ke git | Semua secret bocor ke semua developer & riwayat git |

---

## Hubungan dengan Materi Sebelumnya

- **Materi 97 (OWASP Top 10)** → Secure coding mencegah hampir semua item OWASP
- **Materi 98 (Validation)** → Validasi adalah bagian dari input security
- **Materi 99 (XSS & CSRF)** → Helmet + CSP mencegah XSS
- **Materi 100 (JWT & OAuth)** → Secret management untuk JWT secret & OAuth client ID
- **Materi 101 (Hashing & Enkripsi)** → Secret management untuk encryption key

🔚 **Ini adalah materi PENUTUP Security Level 5. Lanjut ke Materi 103 (Generator & Iterator) untuk masuk ke konsep lanjutan JavaScript.**

---

## Soal Latihan

### Soal 1 (Mudah)
Sebutkan 3 komponen **Helmet.js** dan apa fungsinya.

**Jawaban**:
1. **X-Content-Type-Options: nosniff** — mencegah browser MIME-type sniffing (attacker ubah file .txt jadi .html berisi script).
2. **X-Frame-Options: DENY** — mencegah clickjacking (halaman kita tidak bisa di-embed di iframe situs lain).
3. **Strict-Transport-Security** — paksa browser selalu pakai HTTPS, jangan HTTP.

Helmet memasang 15+ security headers hanya dengan `app.use(helmet())`.

### Soal 2 (Sedang)
Kode berikut memiliki beberapa masalah keamanan. Identifikasi minimal 4:

```javascript
const dbPassword = 'admin123';

app.post('/login', async (req, res) => {
  const user = await prisma.user.findUnique({ where: { email: req.body.email } });
  if (req.body.password === user.password) {
    res.json({ token: jwt.sign({ userId: user.id }, 'my-secret') });
  } else {
    res.status(401).json({ error: 'Login gagal: ' + req.body.email });
  }
});
```

**Jawaban**:
1. **🔴 Secret di hardcode** — `dbPassword` dan `'my-secret'` di kode
2. **🔴 Password plaintext comparison** — bukan bcrypt.compare
3. **🔴 Error message bocor email** — attacker tahu email mana yang terdaftar
4. **🔴 Tidak ada rate limiter** — brute force login
5. **🔴 Tidak validasi input** — `req.body.email` langsung dipakai tanpa validasi
6. **🔴 Tidak cek user null** — jika user tidak ditemukan, `user.password` error

**Perbaikan**:
```javascript
const JWT_SECRET = process.env.JWT_SECRET;

app.post('/login', loginLimiter, async (req, res) => {
  const { email, password } = req.body;
  if (!email || !password) {
    return res.status(400).json({ error: 'Email dan password diperlukan' });
  }

  const user = await prisma.user.findUnique({ where: { email } });
  if (!user) {
    return res.status(401).json({ error: 'Email/password salah' });
  }

  const valid = await bcrypt.compare(password, user.password);
  if (!valid) {
    return res.status(401).json({ error: 'Email/password salah' });
  }

  const token = jwt.sign({ userId: user.id }, JWT_SECRET, { expiresIn: '15m' });
  res.json({ token });
});
```

### Soal 3 (Tantangan)
Buat middleware Express yang secara otomatis **mendeteksi dan mencegah** akses ke file `.env` melalui route statis. Juga buat fungsi pre-commit hook (pseudocode di bash) yang cek apakah ada file `.env` yang akan di-commit.

**Jawaban**:
```javascript
// =============================================
// MIDDLEWARE — Proteksi file sensitif
// =============================================

const path = require('path');
const SENSITIVE_FILES = ['.env', '.env.local', 'config.json', '*.pem', '*.key'];

function protectSensitiveFiles(req, res, next) {
  const requestedPath = req.path.toLowerCase();

  const isSensitive = SENSITIVE_FILES.some(pattern => {
    if (pattern.startsWith('*.')) {
      return requestedPath.endsWith(pattern.slice(1)); // *.ext
    }
    return requestedPath === '/' + pattern || requestedPath.includes('/' + pattern);
  });

  if (isSensitive && req.path.startsWith('/static')) {
    return res.status(403).json({
      error: 'Akses ke file sensitif dilarang',
      code: 'SENSITIVE_FILE_BLOCKED',
    });
  }

  next();
}

// Pasang sebelum static file middleware
app.use(protectSensitiveFiles);
app.use('/static', express.static('public'));
```

```bash
#!/bin/bash
# =============================================
# PRE-COMMIT HOOK — Cek .env sebelum commit
# Simpan di .git/hooks/pre-commit
# =============================================

echo "🔍 Checking for secrets in commit..."

# Cek apakah .env akan di-commit
if git diff --cached --name-only | grep -q '\.env$'; then
  echo "❌ ERROR: .env file terdeteksi akan di-commit!"
  echo "   Tambahkan .env ke .gitignore dan gunakan .env.example"
  exit 1
fi

# Cek apakah ada secret pattern di file yang akan di-commit
if git diff --cached | grep -qE '(password|secret|api_key|token)\s*[:=]\s*["'"'"']?[A-Za-z0-9_!@#$%^&*()-]{8,}'; then
  echo "⚠️  PERINGATAN: Terdeteksi kemungkinan secret di commit!"
  echo "   Periksa ulang sebelum push."
  echo "   Jalankan: git diff --cached | grep -E '(password|secret|key|token)'"
fi

echo "✅ Secret check selesai"
```
