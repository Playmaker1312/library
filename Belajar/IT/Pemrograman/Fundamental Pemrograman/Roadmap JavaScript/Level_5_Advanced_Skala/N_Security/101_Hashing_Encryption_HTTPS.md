# 101. Hashing, Enkripsi & HTTPS

**Benang Merah**: Materi 100 menggunakan JWT (signature dengan HMAC) dan bcrypt (hashing password). Sekarang kita bedah **perbedaan fundamental hashing vs enkripsi**, kapan pakai yang mana, dan bagaimana **HTTPS/TLS** melindungi data dalam perjalanan.

---

## Penjelasan

### Hashing

Hashing adalah proses **satu arah** (one-way): input → hash output yang panjangnya tetap. Tidak bisa balik dari hash ke input. Gunakan: password, checksum, digital signature.

Ciri-ciri hashing:
- **Deterministik**: input sama → hash sama
- **Satu arah**: tidak bisa decrypt
- **Fixed length**: SHA-256 selalu 64 karakter (hex) berapa pun inputnya
- **Avalanche effect**: beda 1 huruf → hash total berbeda

Password hashing (bcrypt, argon2) sengaja **lambat** untuk mempersulit brute force. Bukan SHA/MD5 (yang cepat).

### Enkripsi

Enkripsi adalah proses **dua arah** (two-way): data → ciphertext (dengan kunci) → bisa balik ke data asli. Gunakan: penyimpanan data sensitif (KTP, alamat), komunikasi aman.

Dua jenis:
1. **Simetris (AES)**: satu kunci untuk encrypt & decrypt. Cepat, cocok untuk data besar. Tantangan: kirim kunci dengan aman.
2. **Asimetris (RSA, ECDSA)**: dua kunci — public (encrypt) + private (decrypt). Lambat, cocok untuk kunci sesi & digital signature. Tidak perlu kirim private key.

### HTTPS & TLS

HTTPS = HTTP + TLS (Transport Layer Security). TLS adalah protokol yang mengenkripsi komunikasi antara browser dan server.

Cara kerja TLS:
1. **Handshake**: client minta koneksi aman → server kirim sertifikat TLS (berisi public key)
2. **Verifikasi**: client cek sertifikat (apakah dikeluarkan oleh CA terpercaya?)
3. **Key Exchange**: client buat kunci sesi (symmetric key), encrypt dengan public key server, kirim ke server
4. **Secure Communication**: semua data dienkripsi dengan kunci sesi (AES)

---

## Fungsi

- **Hashing**: menyimpan password dengan aman, verifikasi integritas data
- **Enkripsi**: melindungi data sensitif saat disimpan (at rest) dan dikirim (in transit)
- **HTTPS**: melindungi komunikasi dari eavesdropping, tampering, impersonation

---

## Code: Audit Password + Implementasi bcrypt

```javascript
const express = require('express');
const bcrypt = require('bcrypt');
const crypto = require('crypto');
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();
const app = express();

app.use(express.json());

// =============================================
// SEBELUM — ❌ Hash LEMAH (MD5)
// =============================================
//
// 🔴 MASALAH:
// 1. MD5 sangat cepat — 10 MILYAR hash per detik dengan GPU
// 2. Tidak pakai salt — hash sama untuk password sama
// 3. Rentan rainbow table attack
//
// function hashPasswordMD5(password) {
//   return crypto.createHash('md5').update(password).digest('hex');
// }
// User password: "password123" → hash: "482c811da5d5b4bc6d497ffa98491e38"

// =============================================
// SESUDAH — ✅ bcrypt
// =============================================

const SALT_ROUNDS = 12; // 12 = ~250ms per hash (balance security & speed)

async function hashPassword(password) {
  return bcrypt.hash(password, SALT_ROUNDS);
}

async function verifyPassword(password, hash) {
  return bcrypt.compare(password, hash);
}

// =============================================
// REGISTER — Hash password dengan bcrypt
// =============================================

app.post('/auth/register', async (req, res) => {
  const { email, password, name } = req.body;

  // Validasi password strength
  if (!password || password.length < 8) {
    return res.status(400).json({ error: 'Password minimal 8 karakter' });
  }

  // Hash dengan bcrypt
  const hashedPassword = await hashPassword(password);

  // Simpan hash (BUKAN password asli)
  const user = await prisma.user.create({
    data: { email, password: hashedPassword, name },
  });

  res.status(201).json({ id: user.id, email: user.email });
});

// =============================================
// LOGIN — Verifikasi password
// =============================================

app.post('/auth/login', async (req, res) => {
  const { email, password } = req.body;

  const user = await prisma.user.findUnique({ where: { email } });
  if (!user) {
    return res.status(401).json({ error: 'Email/password salah' });
  }

  // bcrypt.compare(memasukkan, hash) — verifikasi
  const valid = await verifyPassword(password, user.password);
  if (!valid) {
    return res.status(401).json({ error: 'Email/password salah' });
  }

  // Login berhasil — generate JWT dll
  res.json({ message: 'Login berhasil', userId: user.id });
});

// =============================================
// ENKRIPSI SIMETRIS — AES-256-GCM
// =============================================

// Untuk data sensitif (nomor KTP, alamat) PENTING: simpan key di env variable!

function encryptData(text, keyHex) {
  const key = Buffer.from(keyHex, 'hex'); // 32 bytes untuk AES-256
  const iv = crypto.randomBytes(16); // initialization vector — unik setiap encrypt

  const cipher = crypto.createCipheriv('aes-256-gcm', key, iv);
  let encrypted = cipher.update(text, 'utf8', 'hex');
  encrypted += cipher.final('hex');
  const authTag = cipher.getAuthTag().toString('hex');

  // Simpan iv + authTag + ciphertext (iv dan authTag diperlukan untuk decrypt)
  return JSON.stringify({ iv: iv.toString('hex'), encrypted, authTag });
}

function decryptData(encryptedData, keyHex) {
  const { iv, encrypted, authTag } = JSON.parse(encryptedData);
  const key = Buffer.from(keyHex, 'hex');

  const decipher = crypto.createDecipheriv('aes-256-gcm', key, Buffer.from(iv, 'hex'));
  decipher.setAuthTag(Buffer.from(authTag, 'hex'));

  let decrypted = decipher.update(encrypted, 'hex', 'utf8');
  decrypted += decipher.final('utf8');

  return decrypted;
}

// Demo enkripsi
const ENCRYPTION_KEY = crypto.randomBytes(32).toString('hex'); // Simpan di .env!
const sensitiveData = '0878-1234-5678';

const encrypted = encryptData(sensitiveData, ENCRYPTION_KEY);
console.log('🔒 Encrypted:', encrypted);

const decrypted = decryptData(encrypted, ENCRYPTION_KEY);
console.log('🔓 Decrypted:', decrypted);

// =============================================
// AUDIT — Cek password yang masih pakai hash lemah
// =============================================

// Fungsi untuk mendeteksi hash MD5/SHA1 (untuk migrasi)
function detectHashType(hash) {
  if (hash.length === 32 && /^[a-f0-9]{32}$/i.test(hash)) {
    return 'MD5';
  }
  if (hash.length === 40 && /^[a-f0-9]{40}$/i.test(hash)) {
    return 'SHA1';
  }
  if (hash.startsWith('$2b$') || hash.startsWith('$2a$') || hash.startsWith('$2y$')) {
    return 'bcrypt';
  }
  return 'unknown';
}

// Endpoint audit (hanya ADMIN)
app.get('/api/audit/passwords', async (req, res) => {
  const users = await prisma.user.findMany({
    select: { id: true, email: true, password: true },
  });

  const weakPasswords = users.filter(u => detectHashType(u.password) !== 'bcrypt');

  res.json({
    total: users.length,
    weak: weakPasswords.length,
    details: weakPasswords.map(u => ({
      id: u.id,
      email: u.email,
      hashType: detectHashType(u.password),
    })),
  });
});

app.listen(3000);
```

```javascript
// =============================================
// HTTPS — Konfigurasi Server
// =============================================

const https = require('https');
const fs = require('fs');
const express = require('express');
const app = express();

// Sertifikat TLS — dari Let's Encrypt (free) atau certificate authority
const options = {
  key: fs.readFileSync('./ssl/private.key'),
  cert: fs.readFileSync('./ssl/certificate.crt'),
  // opsional:
  // ca: fs.readFileSync('./ssl/ca_bundle.crt'), // chain sertifikat
};

https.createServer(options, app).listen(443, () => {
  console.log('🔒 HTTPS server running on port 443');
});

// ❌ JANGAN pakai HTTP untuk production
// app.listen(80) — hanya untuk development lokal
```

---

## Analogi: Membangun Rumah (Blender & Brankas)

| Konsep | Analogi Rumah |
|---|---|
| **Hashing (bcrypt)** | Blender — Anda masukkan apel utuh → keluar jus bubur. Tidak bisa balikkan jus menjadi apel utuh lagi. |
| **Enkripsi Simetris (AES)** | Brankas dengan satu kunci. Anda kunci surat di brankas → pakai kunci yang sama untuk buka. |
| **Enkripsi Asimetris (RSA)** | Kotak surat: **siapa pun** bisa masukkan surat (public key), tapi hanya Anda yang punya **kunci kotak surat** (private key) yang bisa buka. |
| **Salt (bcrypt)** | Setiap apel diberi bumbu rahasia berbeda sebelum diblender — meskipun apelnya sama, jusnya berbeda. |
| **HTTPS/TLS** | Kurir berseragam resmi yang antar surat dalam amplop tertutup. Tidak ada yang bisa mengintip isinya di tengah jalan. |
| **Sertifikat TLS** | KTP kurir — Anda cek dulu: "Apakah benar ini kurir resmi?" sebelum kasih surat. |

Hashing = blender — satu arah, tidak bisa balik.
Enkripsi = brankas — dua arah, bisa buka/tutup dengan kunci.
HTTPS = amplop tertutup yang diantar kurir resmi — data amat dari ujung ke ujung.

---

## Dipakai Untuk Apa

- **bcrypt** — hash password user di database
- **AES** — enkripsi data sensitif di database (KTP, nomor kartu, alamat)
- **RSA / ECDSA** — JWT signing (RS256), TLS handshake, digital signature
- **HTTPS** — semua aplikasi web production
- **HMAC** — verifikasi integritas data / JWT signature

---

## Kesalahan Umum

| Kesalahan | Dampak |
|---|---|
| Pakai MD5/SHA1 untuk hash password | Brute force mudah — miliaran hash per detik |
| Tidak pakai salt (bcrypt otomatis salt) | Hash sama untuk password sama — rainbow table |
| Enkripsi data tapi kunci di kode | Secret di git — enkripsi tidak berguna |
| Simpan password asli di log/database | Pelanggaran GDPR, user bisa comp lain |
| Pakai HTTP (tanpa HTTPS) | Data dikirim plaintext — siapapun bisa baca di jaringan yang sama |
| Tidak verifikasi sertifikat TLS | Man-in-the-middle — attacker pasang sertifikat palsu |
| Reuse IV (initialization vector) | Pola terlihat — enkripsi bisa dipecahkan |

---

## Hubungan dengan Materi Sebelumnya

- **Materi 100 (JWT)** → JWT signature pakai HMAC (hashing) atau RSA (asymmetric). Refresh token butuh penyimpanan aman.
- **Materi 64 (Prisma)** → Data sensitif di database perlu dienkripsi sebelum disimpan.
- **Materi 97 (OWASP #2 Cryptographic Failures)** → Password tanpa hash, data tanpa enkripsi = celah besar.
- **Materi 102 (Secure Coding)** → Secret management: simpan encryption key di env variable, bukan di kode.

---

## Soal Latihan

### Soal 1 (Mudah)
Jelaskan beda **hashing** dan **enkripsi**. Kapan pakai yang mana?

**Jawaban**:
- **Hashing**: satu arah, tidak bisa balik. Pakai untuk: password (bcrypt), checksum file.
- **Enkripsi**: dua arah, bisa decrypt dengan kunci. Pakai untuk: data sensitif yang perlu dibaca kembali (KTP, alamat, nomor kartu).
- Analogi: Hashing = blender (buah jadi jus, tidak bisa balik). Enkripsi = brankas (bisa kunci dan buka dengan kunci).

Aturan praktis: **Password → hash. Data pribadi → encrypt.**

### Soal 2 (Sedang)
Anda menemukan file `config.json` berisi:
```json
{
  "DB_PASSWORD": "supersecret",
  "ENCRYPTION_KEY": "aes-key-12345",
  "JWT_SECRET": "jwt-secret-2024"
}
```

Apa masalahnya? Bagaimana cara aman menyimpan ini?

**Jawaban**:
**Masalah**:
1. Secret/API key di kode — akan tercatat di git history selamanya
2. ENCRYPTION_KEY terlalu pendek dan mudah ditebak
3. Semua developer punya akses ke production secret

**Cara aman**:
1. Pindahkan ke **environment variables** (`.env`) atau **vault** (HashiCorp Vault, AWS Secrets Manager)
2. `.env` jangan di-commit ke git (tambah di `.gitignore`)
3. Contoh file `.env`:
```
DB_PASSWORD=supersecret
ENCRYPTION_KEY=a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b
JWT_SECRET=a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6
```
4. Akses di kode: `process.env.DB_PASSWORD`

### Soal 3 (Tantangan)
Buat fungsi `safeStorage` — class untuk enkripsi/dekripsi data sensitif menggunakan AES-256-GCM. Harus handle: generate key, encrypt dengan IV unik, decrypt, dan **rotasi kunci** (re-encrypt data dengan kunci baru).

**Jawaban**:
```javascript
const crypto = require('crypto');

class SafeStorage {
  constructor(keyHex = null) {
    this.key = keyHex ? Buffer.from(keyHex, 'hex') : crypto.randomBytes(32);
  }

  encrypt(plaintext) {
    const iv = crypto.randomBytes(16);
    const cipher = crypto.createCipheriv('aes-256-gcm', this.key, iv);

    let encrypted = cipher.update(plaintext, 'utf8', 'hex');
    encrypted += cipher.final('hex');
    const authTag = cipher.getAuthTag().toString('hex');

    // Format: iv:encrypted:authTag
    return `${iv.toString('hex')}:${encrypted}:${authTag}`;
  }

  decrypt(ciphertext) {
    const [ivHex, encrypted, authTag] = ciphertext.split(':');
    const iv = Buffer.from(ivHex, 'hex');
    const decipher = crypto.createDecipheriv('aes-256-gcm', this.key, iv);
    decipher.setAuthTag(Buffer.from(authTag, 'hex'));

    let decrypted = decipher.update(encrypted, 'hex', 'utf8');
    decrypted += decipher.final('utf8');
    return decrypted;
  }

  // Rotasi kunci — decrypt dengan kunci lama, re-encrypt dengan kunci baru
  rotateKey(oldKeyHex, ciphertext) {
    const oldKey = Buffer.from(oldKeyHex, 'hex');
    const oldStorage = new SafeStorage(oldKeyHex);
    const plaintext = oldStorage.decrypt(ciphertext);

    const newStorage = new SafeStorage(); // generate key baru
    const newCiphertext = newStorage.encrypt(plaintext);

    return {
      newKey: newStorage.key.toString('hex'),
      newCiphertext,
    };
  }

  // Helper: generate key aman
  static generateKey() {
    return crypto.randomBytes(32).toString('hex');
  }
}

// Demo
const storage = new SafeStorage();
const ciphertext = storage.encrypt('0878-1234-5678');
console.log('🔒 Encrypted:', ciphertext);

const plaintext = storage.decrypt(ciphertext);
console.log('🔓 Decrypted:', plaintext);

// Rotasi kunci
const rotated = storage.rotateKey(storage.key.toString('hex'), ciphertext);
console.log('🔄 New key:', rotated.newKey);
console.log('🔄 New ciphertext:', rotated.newCiphertext);

// Verifikasi — decrypt dengan kunci baru
const newStorage = new SafeStorage(rotated.newKey);
console.log('✅ Verifikasi rotasi:', newStorage.decrypt(rotated.newCiphertext));
```
