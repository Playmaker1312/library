# 108 — Caching Strategies

## 1. Penjelasan

**Caching** adalah teknik menyimpan salinan data yang sering diakses di lokasi yang lebih cepat dijangkau, sehingga permintaan berikutnya tidak perlu memproses dari sumber asli (database, API eksternal, komputasi berat).

**In-Memory Cache** — menyimpan data di RAM proses aplikasi (Map, LRU Cache). Paling cepat tapi terbatas memori dan hilang saat server restart.

**Redis** — distributed cache server di luar proses. Data disimpan di RAM, bisa diakses banyak instance aplikasi. Mendukung TTL (Time-To-Live), pub/sub, dan berbagai struktur data.

**HTTP Caching** — cache di level browser/network (Cache-Control, ETag). Mengurangi jumlah request ke server.

**Cache Invalidation** — masalah tersulit dalam caching. Bagaimana memastikan cache tetap sinkron dengan data asli? Strategi: TTL, write-through, cache-aside, event-driven invalidation.

| Level Cache | Kecepatan | Kapasitas | Persistensi | Biaya |
|-------------|-----------|-----------|-------------|-------|
| L1 (CPU) | 1 ns | KB | Volatile | Tinggi |
| L2 (Memory) | 100 ns | MB-GB | Volatile | Sedang |
| L3 (Redis) | 1 ms | GB | Bisa persist | Rendah |
| L4 (HTTP/CDN) | 10-100 ms | GB-TB | Persisten | Sangat rendah |

## 2. Fungsi

- Mengurangi response time API dari detik ke milidetik
- Mengurangi beban database (kurang query, koneksi lebih tersedia)
- Menghemat biaya komputasi (hasil komputasi berat tidak perlu diulang)
- Meningkatkan throughput server (melayani lebih banyak request)
- Menstabilkan aplikasi saat traffic spike (cache melindungi database)

## 3. Code

### In-Memory Cache (Map + LRU sederhana)

```javascript
class LRUCache {
  constructor(maxSize = 100) {
    this.maxSize = maxSize;
    this.cache = new Map();
  }

  get(key) {
    if (!this.cache.has(key)) return undefined;
    const value = this.cache.get(key);
    // Pindahkan ke paling baru (delete lalu set)
    this.cache.delete(key);
    this.cache.set(key, value);
    return value;
  }

  set(key, value, ttlMs = 0) {
    // Jika sudah penuh, hapus entry terlama (first item Map)
    if (this.cache.size >= this.maxSize) {
      const oldestKey = this.cache.keys().next().value;
      this.cache.delete(oldestKey);
    }
    this.cache.set(key, value);
    if (ttlMs > 0) {
      setTimeout(() => this.cache.delete(key), ttlMs);
    }
  }

  has(key) {
    return this.cache.has(key);
  }

  delete(key) {
    this.cache.delete(key);
  }

  clear() {
    this.cache.clear();
  }

  get size() {
    return this.cache.size;
  }
}

// Usage
const userCache = new LRUCache(100);
userCache.set('user:1', { name: 'Budi' }, 60000); // TTL 60 detik
console.log(userCache.get('user:1')); // { name: 'Budi' }
```

### Redis Caching — Express API

```javascript
const express = require('express');
const Redis = require('ioredis');
const app = express();

const redis = new Redis({
  host: 'localhost',
  port: 6379,
  retryStrategy: (times) => Math.min(times * 50, 2000),
});

// Middleware cache
function cacheMiddleware(ttlSeconds = 60) {
  return async (req, res, next) => {
    const cacheKey = `cache:${req.originalUrl}`;

    try {
      const cachedData = await redis.get(cacheKey);
      if (cachedData) {
        console.log('CACHE HIT:', cacheKey);
        return res.json({
          data: JSON.parse(cachedData),
          fromCache: true,
          cachedAt: new Date().toISOString(),
        });
      }
    } catch (err) {
      console.warn('Redis error, bypass cache:', err.message);
    }

    console.log('CACHE MISS:', cacheKey);
    // Simpan response asli
    const originalJson = res.json.bind(res);
    res.json = function (body) {
      // Simpan ke Redis (jangan simpan error)
      if (res.statusCode < 400) {
        redis.setex(cacheKey, ttlSeconds, JSON.stringify(body.data || body))
          .catch(err => console.warn('Gagal simpan cache:', err.message));
      }
      return originalJson(body);
    };
    next();
  };
}

// Endpoint yang sering diakses — cache 60 detik
app.get('/api/popular-books', cacheMiddleware(60), async (req, res) => {
  const books = await Book.findAll({
    attributes: ['id', 'title', 'author', 'views'],
    order: [['views', 'DESC']],
    limit: 50,
  });
  res.json({ data: books });
});

// Endpoint dinamis — hapus cache saat data berubah
app.post('/api/books', async (req, res) => {
  const book = await Book.create(req.body);
  // Invalidate cache yang terkait
  await redis.del('cache:/api/popular-books');
  res.status(201).json({ data: book });
});
```

### HTTP Caching

```javascript
// Server-side: Set Cache-Control header
app.get('/api/static-config', (req, res) => {
  const config = { theme: 'dark', version: '1.0' };
  res.set('Cache-Control', 'public, max-age=3600'); // 1 jam
  res.set('ETag', `"config-v1"`);
  res.json(config);
});

// Client-side: fetch dengan cache
async function getConfig() {
  const response = await fetch('/api/static-config', {
    headers: { 'If-None-Match': '"config-v1"' },
  });
  if (response.status === 304) {
    console.log('Cache masih fresh, pakai local cache');
    return cachedConfig;
  }
  const config = await response.json();
  return config;
}
```

### Cache-Aside Pattern

```javascript
// Cache-Aside (paling umum)
async function getUser(id) {
  // 1. Cek cache dulu
  const cached = await redis.get(`user:${id}`);
  if (cached) return JSON.parse(cached);

  // 2. Cache miss → ambil dari database
  const user = await db.query('SELECT * FROM users WHERE id = $1', [id]);
  if (!user) return null;

  // 3. Simpan ke cache untuk request berikutnya
  await redis.setex(`user:${id}`, 300, JSON.stringify(user));
  return user;
}

// Write-Through
async function updateUser(id, data) {
  // 1. Update database dulu
  const user = await db.query(
    'UPDATE users SET name = $1 WHERE id = $2 RETURNING *',
    [data.name, id]
  );
  // 2. Update cache langsung
  await redis.setex(`user:${id}`, 300, JSON.stringify(user));
  return user;
}
```

## 4. Analogi Rumah — Dapur Tambahan di Setiap Lantai

| Konsep JS | Analogi Rumah |
|-----------|---------------|
| Cache | Dapur kecil di setiap lantai rumah |
| Database | Gudang bawah tanah — semua bahan makanan ada di sini |
| Cache Hit | Ambil garam dari dapur lantai 2 — cepat, 2 detik |
| Cache Miss | Garam habis di dapur → turun ke gudang bawah tanah — 2 menit |
| TTL (Time-To-Live) | Garam di dapur kadaluarsa seminggu lagi → buang, ambil baru |
| Cache Invalidation | Gula di gudang diganti merek baru → gula di semua dapur harus diganti |
| Redis | Dapur bersama di area kompleks perumahan — semua rumah bisa akses |
| In-Memory Cache | Dapur di dalam rumah Anda sendiri — paling cepat, hanya untuk Anda |
| HTTP Cache (Browser) | Kotak bekal yang Anda bawa — untuk Anda sendiri, di perangkat Anda |
| LRU Eviction | Dapur kecil — jika penuh, buang bumbu paling jarang dipakai |
| Cache Stampede | Semua penghuni rumah turun ke gudang saat garam habis bersamaan — antri panjang |

> **Masalah Cache Invalidation:** "There are only two hard things in Computer Science: cache invalidation and naming things." — Phil Karlton. Bayangkan Anda punya 5 dapur di 5 lantai. Jika stok gula di gudang berubah, Anda harus memberi tahu semua dapur — kalau tidak, ada yang pakai gula lama (stale data).

## 5. Use Case di Proyek Nyata

- **E-commerce product catalog** — produk tidak berubah tiap detik, cache 5 menit
- **Social media feed** — cache timeline user selama 30 detik
- **API rate limiting** — Redis untuk counter request per IP (TTL 1 menit)
- **Session store** — Redis menyimpan session user (cepat, shared across instances)
- **Database query cache** — query yang sama dalam 10 detik tidak perlu dijalankan ulang
- **CDN static assets** — images, CSS, JS di-cache CDN selama 1 tahun (dengan hash di filename)

## 6. Kesalahan Umum

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| Cache TTL terlalu panjang | Data basi (stale) ditampilkan | Sesuaikan TTL dengan frekuensi perubahan data |
| Tidak ada cache invalidation | User melihat data lama | Implementasi write-through atau event-driven invalidation |
| Cache stampede | Database kena beban tinggi mendadak | Gunakan mutex/lock atau "probabilistic early expiration" |
| Cache semua data tanpa prioritas | Redis penuh dengan data tidak penting | Cache hanya data yang sering diakses dan mahal diproduksi |
| Redis tanpa fallback | Aplikasi crash saat Redis down | Gunakan in-memory cache sebagai fallback |
| Lupa set TTL | Cache tumbuh tak terbatas | Selalu set TTL, kecuali ada alasan kuat |
| Cache key tidak konsisten | Cache miss terus (tidak efektif) | Standarisasi format key: `entity:id:field` |

## 7. Benang Merah

```
Materi 107 (Performance Optimization)
    ↓
Materi 108 (Caching Strategies) ← Anda di sini (PENUTUP Bagian Performa)
    ↓
DevOps (Materi 109-114)
```

Caching adalah puncak dari strategi performa. Setelah mempelajari profiling, bottleneck, dan optimasi database (Materi 107), caching adalah senjata paling ampuh untuk mengurangi latensi. Ini menutup bagian **Performa** dengan sempurna. Selanjutnya: DevOps — deployment, CI/CD, monitoring, scaling (Materi 109-114).

## 8. Soal & Jawaban

### Soal 1: Easy
Apa perbedaan antara **Cache Hit** dan **Cache Miss**? Mana yang lebih cepat?

<details>
<summary>Jawaban</summary>
**Cache Hit** — data ditemukan di cache (langsung dikembalikan, cepat). **Cache Miss** — data tidak ada di cache, harus ambil dari sumber asli (database/API, lambat). Cache Hit lebih cepat (milidetik vs detik).
</details>

### Soal 2: Medium
Buatlah fungsi `cachedFetch(url, ttlSeconds)` yang mengambil data dari URL dan menyimpannya di Map cache. Jika dalam TTL data sudah ada di cache, gunakan data cache (jangan fetch ulang). Sertakan mekanisme untuk mengosongkan cache.

<details>
<summary>Jawaban</summary>

```javascript
const cache = new Map();

async function cachedFetch(url, ttlSeconds = 60) {
  const cacheKey = `fetch:${url}`;
  const cached = cache.get(cacheKey);

  if (cached && Date.now() < cached.expiry) {
    console.log('Cache hit:', url);
    return cached.data;
  }

  console.log('Cache miss:', url);
  const response = await fetch(url);
  if (!response.ok) throw new Error(`HTTP ${response.status}`);
  const data = await response.json();

  cache.set(cacheKey, {
    data,
    expiry: Date.now() + ttlSeconds * 1000,
  });

  return data;
}

function clearCache() {
  cache.clear();
  console.log('Cache dibersihkan');
}

// Usage
async function main() {
  const data1 = await cachedFetch('https://api.example.com/products', 30);
  const data2 = await cachedFetch('https://api.example.com/products', 30); // Cache hit
}
```
</details>

### Soal 3: Hard
Anda mengelola endpoint `/api/dashboard-stats` yang membutuhkan 3 query berat (total 5 detik). Implementasikan caching Redis dengan strategi berikut:
1. Cache data selama 5 menit
2. Jika Redis down, gunakan in-memory cache sebagai fallback
3. Saat admin mengubah data (POST `/api/dashboard-stats/refresh`), cache harus di-invalidate
4. Gunakan cache stampede protection (hanya satu proses yang mengisi cache)

<details>
<summary>Jawaban</summary>

```javascript
const express = require('express');
const Redis = require('ioredis');
const crypto = require('crypto');

const app = express();
const redis = new Redis({ host: 'localhost', port: 6379 });
const memoryCache = new Map();
const CACHE_TTL = 300; // 5 menit
const STALE_TTL = 600; // 10 menit — serve stale jika Redis down

// Cache stampede protection
const fetchLocks = new Map();

async function getOrCompute(cacheKey, computeFn, ttl = CACHE_TTL) {
  // 1. Cek Redis
  try {
    const cached = await redis.get(cacheKey);
    if (cached) {
      const parsed = JSON.parse(cached);
      // Probabilistic early expiration: refresh jika >80% TTL
      const age = (Date.now() - parsed.cachedAt) / 1000;
      if (age < ttl * 0.8) {
        return parsed.data;
      }
    }
  } catch (err) {
    // Redis down → fallback ke memory cache
    const memCached = memoryCache.get(cacheKey);
    if (memCached && Date.now() < memCached.expiry) {
      return memCached.data;
    }
  }

  // 2. Cache stampede protection — hanya satu proses yang compute
  if (fetchLocks.has(cacheKey)) {
    return fetchLocks.get(cacheKey);
  }

  const promise = (async () => {
    const data = await computeFn();
    const cacheEntry = { data, cachedAt: Date.now() };

    // Simpan ke Redis
    try {
      await redis.setex(cacheKey, ttl, JSON.stringify(cacheEntry));
    } catch {
      // Fallback ke memory cache
      memoryCache.set(cacheKey, {
        data,
        expiry: Date.now() + STALE_TTL * 1000,
      });
    }

    return data;
  })();

  fetchLocks.set(cacheKey, promise);
  try {
    return await promise;
  } finally {
    fetchLocks.delete(cacheKey);
  }
}

// Query berat
async function computeDashboardStats() {
  const [totalUsers, totalOrders, revenue] = await Promise.all([
    db.query('SELECT COUNT(*) FROM users'),
    db.query('SELECT COUNT(*) FROM orders'),
    db.query('SELECT SUM(total) FROM orders WHERE status = \'paid\''),
  ]);
  return {
    totalUsers: parseInt(totalUsers[0].count),
    totalOrders: parseInt(totalOrders[0].count),
    revenue: parseFloat(revenue[0].sum) || 0,
    computedAt: new Date().toISOString(),
  };
}

app.get('/api/dashboard-stats', async (req, res) => {
  try {
    const stats = await getOrCompute('dashboard:stats', computeDashboardStats);
    res.json({ data: stats, fromCache: true });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

app.post('/api/dashboard-stats/refresh', async (req, res) => {
  // Invalidasi cache
  try {
    await redis.del('dashboard:stats');
  } catch { /* Redis down */ }
  memoryCache.delete('dashboard:stats');

  // Hitung ulang langsung
  const stats = await computeDashboardStats();
  res.json({ data: stats, refreshed: true });
});
```
</details>
