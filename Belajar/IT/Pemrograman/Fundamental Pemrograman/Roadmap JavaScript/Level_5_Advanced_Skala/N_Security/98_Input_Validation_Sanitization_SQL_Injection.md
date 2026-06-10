# 98. Input Validation, Sanitization & SQL Injection Prevention

**Benang Merah**: Materi 97 memperkenalkan OWASP #3 Injection sebagai ancaman serius. Sekarang kita pelajari **dua lapis pertahanan pertama**: validasi (cek format input) dan sanitasi (bersihkan karakter berbahaya), plus cara mencegah SQL Injection bahkan saat ORM tidak cukup.

---

## Penjelasan

**Input Validation** adalah proses memeriksa apakah data yang masuk sesuai format yang diharapkan (email harus `@`, umur harus angka, dll). **Input Sanitization** adalah proses membersihkan atau menetralisir karakter berbahaya dari input (misal: hapus tag `<script>`).

**SQL Injection** terjadi ketika attacker menyelipkan perintah SQL melalui input user. ORM (Prisma, Sequelize) menggunakan **parameterized query** yang secara otomatis memisahkan query dari data — tapi ORM tidak 100% aman jika ada fitur **raw query**, **$queryRaw**, atau ORDER BY dinamis.

Lapisan pertahanan:
1. **Validasi** — tolak input sejak awal jika format salah
2. **Sanitasi** — bersihkan input yang lolos validasi
3. **Parameterized Query / ORM** — pisahkan query dari data
4. **Least Privilege** — user database hanya punya akses minimal

---

## Fungsi

Mencegah attacker menyusupkan kode berbahaya (SQL, NoSQL, OS command) melalui input user. Menjamin data yang masuk aman dan sesuai format sebelum diproses lebih lanjut.

---

## Code: Validasi Lengkap + Sanitasi + SQL Injection Prevention

```javascript
const express = require('express');
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();
const app = express();

app.use(express.json());

// =============================================
// VALIDASI LAYER
// =============================================

function validateEmail(email) {
  const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return re.test(email);
}

function validateAge(age) {
  return Number.isInteger(age) && age >= 0 && age <= 150;
}

function validateUsername(username) {
  return typeof username === 'string' &&
         username.length >= 3 &&
         username.length <= 30 &&
         /^[a-zA-Z0-9_]+$/.test(username); // hanya alfanumerik + underscore
}

// =============================================
// SANITASI LAYER
// =============================================

function sanitizeString(input) {
  if (typeof input !== 'string') return '';
  // Hapus tag HTML/script
  return input
    .replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '')
    .replace(/<[^>]*>/g, '')
    .trim();
}

function sanitizeForSQL(input) {
  // Hanya untuk raw query DARURAT (sebaiknya hindari)
  if (typeof input !== 'string') return '';
  return input.replace(/['";\\]/g, '');
}

// =============================================
// MIDDLEWARE VALIDASI + SANITASI
// =============================================

function validateRegisterInput(req, res, next) {
  const { email, name, age } = req.body;

  const errors = [];

  if (!email || !validateEmail(email)) {
    errors.push('Email tidak valid');
  }
  if (!name || !validateUsername(name)) {
    errors.push('Username harus 3-30 karakter, hanya huruf/angka/underscore');
  }
  if (age !== undefined && !validateAge(age)) {
    errors.push('Age harus angka 0-150');
  }

  if (errors.length > 0) {
    return res.status(400).json({ errors });
  }

  // Sanitasi setelah validasi lolos
  req.body.name = sanitizeString(req.body.name);
  req.body.bio = req.body.bio ? sanitizeString(req.body.bio) : '';

  next();
}

// =============================================
// ENDPOINT AMAN
// =============================================

// ✅ AMAN — ORM parameterized query
app.post('/api/users', validateRegisterInput, async (req, res) => {
  try {
    const user = await prisma.user.create({
      data: {
        email: req.body.email,
        name: req.body.name,
        age: req.body.age,
        bio: req.body.bio,
      },
    });
    res.status(201).json(user);
  } catch (err) {
    res.status(500).json({ error: 'Gagal membuat user' });
  }
});

// ✅ AMAN — validasi id + ORM
app.get('/api/users/:id', async (req, res) => {
  const id = parseInt(req.params.id);
  if (isNaN(id) || id < 1) {
    return res.status(400).json({ error: 'ID harus angka positif' });
  }

  const user = await prisma.user.findUnique({ where: { id } });
  if (!user) return res.status(404).json({ error: 'User tidak ditemukan' });
  res.json(user);
});

// ⚠️ KASUS RAW QUERY — HATI-HATI!
app.get('/api/users/search', async (req, res) => {
  const { q } = req.query;

  if (!q || typeof q !== 'string' || q.length > 100) {
    return res.status(400).json({ error: 'Query tidak valid' });
  }

  // Sanitasi sebelum raw query (tetap kurang aman, hindari)
  const sanitized = sanitizeForSQL(q);

  try {
    // ❌ Tidak disarankan — lebih baik pakai prisma.findMany({ where: { name: { contains: q } } })
    const users = await prisma.$queryRawUnsafe(
      `SELECT * FROM users WHERE name LIKE '%${sanitized}%'`
    );
    res.json(users);
  } catch (err) {
    res.status(500).json({ error: 'Gagal mencari' });
  }
});

// ✅ ALTERNATIF AMAN — pakai Prisma built-in search
app.get('/api/users/search-safe', async (req, res) => {
  const { q } = req.query;
  if (!q || typeof q !== 'string') {
    return res.status(400).json({ error: 'Query tidak valid' });
  }

  const users = await prisma.user.findMany({
    where: { name: { contains: q, mode: 'insensitive' } },
  });
  res.json(users);
});

app.listen(3000);
```

---

## Analogi: Membangun Rumah (Security Check di Pintu Masuk)

| Konsep | Analogi Rumah |
|---|---|
| **Validasi** | Satpam cek KTP — format nama, nomor KTP, foto harus cocok |
| **Sanitasi** | Satpam scan body — pastikan tidak bawa senjata/bahan berbahaya |
| **SQL Injection** | Tamu bilang "saya suruhan pak RT" padahal mau bobol rumah |
| **ORM / Parameterized Query** | Pintu otomatis yang hanya bisa dibuka dengan kartu khusus |
| **Raw Query** | Pintu darurat yang bisa dibuka manual — jangan dipakai kalau tidak terpaksa |
| **Least Privilege** | Tamu hanya boleh di ruang tamu, tidak ke kamar tidur |

Security check di pintu gedung: setiap tamu harus tunjukkan KTP (validasi), tidak bawa senjata (sanitasi), dan hanya boleh masuk lewat pintu utama yang dijaga (parameterized query).

---

## Dipakai Untuk Apa

- **Semua endpoint API** — validasi request body, query params, route params
- **Form registrasi/login** — validasi email, password strength
- **Search feature** — sanitasi input pencarian
- **File upload** — validasi tipe & ukuran file
- **ORM raw query** — hanya jika terpaksa, dengan sanitasi ekstra

---

## Kesalahan Umum

| Kesalahan | Dampak |
|---|---|
| Hanya validasi di client-side (frontend) | Attacker bisa bypass pakai curl/Postman |
| Percaya 100% ORM aman | `$queryRawUnsafe` tetap rentan SQL Injection |
| Validasi format saja tanpa sanitasi | XSS masih bisa masuk melalui field teks |
| Tidak validasi `req.params` | NaN atau string bisa merusak query |
| Sanitasi berlebihan (hapus karakter normal) | User experience rusak (misal: nama O'Brien) |

---

## Hubungan dengan Materi Sebelumnya

- **Materi 97 (OWASP #3 Injection)** → Validasi & sanitasi adalah mitigasi utama Injection
- **Materi 61 (SQL)** → Tanpa parameterized query, SQL Injection mudah terjadi
- **Materi 64 (Prisma)** → ORM mengurangi risiko, tapi raw query tetap berbahaya
- **Materi 99 (XSS & CSRF)** → Sanitasi juga penting untuk mencegah XSS (client-side)

---

## Soal Latihan

### Soal 1 (Mudah)
Apa perbedaan antara **validasi** dan **sanitasi** input? Beri contoh masing-masing.

**Jawaban**:
- **Validasi**: Memeriksa apakah input sesuai format. Contoh: `validateEmail('abc')` → false karena tidak ada `@`.
- **Sanitasi**: Membersihkan karakter berbahaya dari input. Contoh: `sanitizeString('<script>alert("xss")</script>')` → `''` (tag script dihapus).

Validasi **menolak** input buruk. Sanitasi **membersihkan** input yang sudah diterima.

### Soal 2 (Sedang)
Kode berikut rentan SQL Injection. Perbaiki dengan dua cara (ORM + validasi):

```javascript
app.get('/product', (req, res) => {
  const id = req.query.id;
  const product = db.query(`SELECT * FROM products WHERE id = ${id}`);
  res.json(product);
});
```

**Jawaban**:
```javascript
// Cara 1 — Validasi + ORM (Prisma)
app.get('/product', async (req, res) => {
  const id = parseInt(req.query.id);
  if (isNaN(id) || id < 1) {
    return res.status(400).json({ error: 'ID tidak valid' });
  }
  const product = await prisma.product.findUnique({ where: { id } });
  res.json(product);
});

// Cara 2 — Parameterized Query (tanpa ORM)
app.get('/product', (req, res) => {
  const id = parseInt(req.query.id);
  if (isNaN(id) || id < 1) {
    return res.status(400).json({ error: 'ID tidak valid' });
  }
  const product = db.query('SELECT * FROM products WHERE id = $1', [id]);
  res.json(product);
});
```

### Soal 3 (Tantangan)
Buat fungsi `createSafeUser(data)` yang menerima `{ username, email, bio }`. Validasi username (3-20 karakter, hanya huruf/angka), validasi email, sanitasi bio (hapus tag HTML). Return object yang sudah divalidasi & disanitasi, atau throw error jika tidak valid.

**Jawaban**:
```javascript
function createSafeUser(data) {
  const errors = [];

  // Validasi username
  if (!data.username || typeof data.username !== 'string' ||
      data.username.length < 3 || data.username.length > 20 ||
      !/^[a-zA-Z0-9]+$/.test(data.username)) {
    errors.push('Username harus 3-20 karakter, hanya huruf/angka');
  }

  // Validasi email
  if (!data.email || typeof data.email !== 'string' ||
      !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(data.email)) {
    errors.push('Email tidak valid');
  }

  if (errors.length > 0) {
    throw new Error(errors.join(', '));
  }

  // Sanitasi
  const sanitizedBio = data.bio
    ? data.bio.replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '')
              .replace(/<[^>]*>/g, '')
              .trim()
    : '';

  return {
    username: data.username,
    email: data.email,
    bio: sanitizedBio,
  };
}

// Test
try {
  const user = createSafeUser({
    username: 'rudi123',
    email: 'rudi@mail.com',
    bio: 'Halo <script>alert("xss")</script> dunia',
  });
  console.log(user); // bio: 'Halo  dunia'
} catch (e) {
  console.error(e.message);
}
```
