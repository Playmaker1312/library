# 107 — Performance Optimization

## 1. Penjelasan

**Performance Optimization** adalah proses mengidentifikasi dan memperbaiki bottleneck (titik lambat) di aplikasi. Prinsip utamanya: **ukur dulu, baru optimasi** (profile, don't guess). Optimasi tanpa pengukuran adalah murni tebakan yang bisa memperburuk keadaan.

**Frontend Optimization** berfokus pada Core Web Vitals:
- **LCP** (Largest Contentful Paint) — kecepatan muat konten utama
- **FID** (First Input Delay) / **INP** (Interaction to Next Paint) — responsivitas interaksi
- **CLS** (Cumulative Layout Shift) — kestabilan layout

Teknik: lazy loading, code splitting, image optimization, bundle size reduction.

**Backend Optimization** berfokus pada:
- **N+1 Query Problem** — query database dalam loop (sangat lambat)
- **Database Indexing** — mempercepat pencarian data
- **Connection Pooling** — reuse koneksi database
- **Caching** — menyimpan hasil yang sering diminta

## 2. Fungsi

- Mengidentifikasi bottleneck dengan profiling (Chrome DevTools, Node.js Profiler)
- Meningkatkan Core Web Vitals untuk SEO dan user experience
- Mengurangi response time API dari detik ke milidetik
- Mengoptimalkan database query (N+1 → eager loading / batch query)
- Mengurangi bundle size JavaScript dengan code splitting

## 3. Code

### Profiling Frontend — Chrome DevTools

```javascript
// Gunakan Performance tab di DevTools
// Atau programmatic profiling:
performance.mark('start-process');
// ... kode yang mau diukur ...
const largeArray = new Array(1000000).fill('data');
const processed = largeArray.map(x => x.toUpperCase());
performance.mark('end-process');
performance.measure('Process Duration', 'start-process', 'end-process');

const measures = performance.getEntriesByType('measure');
console.log('Durasi:', measures[0].duration, 'ms');
```

### Optimasi N+1 Query (Backend)

```javascript
// ❌ N+1 Query Problem
async function getUsersWithPosts() {
  const users = await db.query('SELECT * FROM users');
  for (const user of users) {
    // Satu query per user — jika 100 user = 101 query total!
    user.posts = await db.query('SELECT * FROM posts WHERE user_id = $1', [user.id]);
  }
  return users;
}

// ✅ Optimasi: JOIN atau batch query
async function getUsersWithPostsOptimized() {
  const users = await db.query('SELECT * FROM users');
  const userIds = users.map(u => u.id);
  // Satu query untuk semua posts
  const posts = await db.query('SELECT * FROM posts WHERE user_id = ANY($1)', [userIds]);
  // Kelompokkan posts per user
  const postsByUser = {};
  posts.forEach(post => {
    if (!postsByUser[post.user_id]) postsByUser[post.user_id] = [];
    postsByUser[post.user_id].push(post);
  });
  users.forEach(user => {
    user.posts = postsByUser[user.id] || [];
  });
  return users;
  // Total: hanya 2 query, bukan 101!
}
```

### Lazy Loading & Code Splitting

```javascript
// ❌ Tanpa code splitting: satu bundle besar
import { HeavyChart } from './HeavyChart';

// ✅ Dengan code splitting: HeavyChart dimuat saat dibutuhkan
const HeavyChart = React.lazy(() => import('./HeavyChart'));

function Dashboard() {
  const [showChart, setShowChart] = useState(false);
  return (
    <div>
      <button onClick={() => setShowChart(true)}>Tampilkan Chart</button>
      {showChart && (
        <React.Suspense fallback={<div>Memuat chart...</div>}>
          <HeavyChart />
        </React.Suspense>
      )}
    </div>
  );
}
```

### Profiling & Optimasi — Fullstack Library App

```javascript
// Sebelum optimasi — bottleneck di query buku populer
// app.js (backend)
app.get('/api/popular-books', async (req, res) => {
  // ❌ Masalah 1: Tidak ada pagination
  const books = await Book.findAll({
    include: [Category, Author, Review],
    order: [['views', 'DESC']],
  });

  // ❌ Masalah 2: N+1 — Review dihitung di JavaScript
  const result = books.map(book => {
    const avgRating = book.Reviews.reduce((sum, r) => sum + r.rating, 0) / book.Reviews.length;
    return { ...book.toJSON(), avgRating };
  });

  res.json(result);
});
```

```javascript
// ✅ Setelah optimasi
app.get('/api/popular-books', async (req, res) => {
  const page = parseInt(req.query.page) || 1;
  const limit = parseInt(req.query.limit) || 20;

  // ✅ Fix 1: Pagination + eager loading terbatas
  const books = await Book.findAll({
    attributes: ['id', 'title', 'author', 'views', 'cover_url'],
    include: [
      { model: Category, attributes: ['name'] },
    ],
    order: [['views', 'DESC']],
    limit,
    offset: (page - 1) * limit,
  });

  // ✅ Fix 2: Hitung rating di database, bukan JavaScript
  const bookIds = books.map(b => b.id);
  const ratings = await Review.findAll({
    attributes: [
      'book_id',
      [sequelize.fn('AVG', sequelize.col('rating')), 'avgRating'],
    ],
    where: { book_id: { [Op.in]: bookIds } },
    group: ['book_id'],
  });
  const ratingMap = {};
  ratings.forEach(r => { ratingMap[r.book_id] = r.dataValues.avgRating; });

  const result = books.map(book => ({
    ...book.toJSON(),
    avgRating: ratingMap[book.id] || 0,
  }));

  // ✅ Fix 3: Cache hasil — tidak perlu query tiap request
  // (implementasi di Materi 108)
  res.json({ data: result, page, total: await Book.count() });
});
```

## 4. Analogi Rumah — Cari Penyebab Rumah Panas

| Konsep JS | Analogi Rumah |
|-----------|---------------|
| Profiling (ukur dulu) | Cek suhu di setiap ruangan — mana yang paling panas? |
| Bottleneck | Atap bocor, jendela terbuka, atau isolasi kurang? |
| N+1 Query | Mengecek suhu tiap ruangan dengan termometer berbeda (ribet) |
| Eager Loading | Termometer digital yang baca semua ruangan sekaligus |
| Lazy Loading | AC hanya dinyalakan di ruangan yang dipakai |
| Code Splitting | Bangun rumah per lantai — tidak perlu selesai semua baru dipakai |
| Image Optimization | Ganti jendela kaca tebal dengan double glazing — bobot ringan, fungsi sama |
| Caching | Termostat — begitu suhu ideal, AC tidak perlu terus bekerja |

> **Aturan emas:** Sebelum pasang AC (optimasi), cek dulu apakah masalahnya di atap bocor, jendela terbuka, atau isolasi kurang. Jangan pasang AC 5 PK di kamar kecil — itu over-engineering. Profile dulu, baru optimasi.

## 5. Use Case di Proyek Nyata

- **E-commerce** — optimasi halaman produk (LCP < 2.5s, CLS < 0.1)
- **SaaS Dashboard** — database query real-time dari 1M+ records
- **Social Media Feed** — infinite scroll dengan virtual scroll (hanya render item visible)
- **Video Streaming** — adaptive bitrate berdasarkan koneksi user
- **API Gateway** — response time < 100ms untuk 10k RPM (request per minute)

## 6. Kesalahan Umum

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| Optimasi tanpa profiling | Membuang waktu di tempat yang salah | Profile dulu (Lighthouse, DevTools, APM) |
| Over-optimasi awal | Code kompleks tidak perlu | Optimasi hanya saat ada bukti bottleneck |
| Lupa pagination | Transfer data besar, client lambat | Selalu pagination untuk list data |
| N+1 query tidak terdeteksi | Ribuan query untuk data sederhana | Gunakan `include` (eager loading) atau batch query |
| Bundle size tidak diperhatikan | JS besar, First Load lambat | Code splitting, tree shaking, minifikasi |
| Tidak menggunakan CDN | Asset lambat dari server jauh | CDN untuk static assets (images, fonts, JS) |

## 7. Benang Merah

```
Materi 106 (Concurrency — Web Workers)
    ↓
Materi 107 (Performance Optimization) ← Anda di sini
    ↓
Materi 108 (Caching Strategies)
```

Setelah mampu memanfaatkan banyak thread dengan worker (Materi 106), kini Anda belajar pendekatan sistematis untuk optimasi performa — dari profiling, N+1 query, hingga code splitting. Optimasi belum lengkap tanpa caching (Materi 108) — cara paling efektif untuk mengurangi beban server dan mempercepat response.

## 8. Soal & Jawaban

### Soal 1: Easy
Sebutkan 3 Core Web Vitals beserta ambang batas "baik" (good threshold) masing-masing.

<details>
<summary>Jawaban</summary>
1. **LCP** (Largest Contentful Paint) — < 2.5 detik
2. **FID** (First Input Delay) / **INP** (Interaction to Next Paint) — < 100 ms / < 200 ms
3. **CLS** (Cumulative Layout Shift) — < 0.1
</details>

### Soal 2: Medium
Kode berikut memiliki N+1 query problem. Perbaiki dengan `Promise.all` dan batch query.

```javascript
async function getProductReviews(productIds) {
  const reviews = [];
  for (const id of productIds) {
    const productReviews = await db.query(
      'SELECT * FROM reviews WHERE product_id = $1', [id]
    );
    reviews.push(...productReviews);
  }
  return reviews;
}
```

<details>
<summary>Jawaban</summary>

```javascript
async function getProductReviews(productIds) {
  // Satu query batch untuk semua product IDs
  const allReviews = await db.query(
    'SELECT * FROM reviews WHERE product_id = ANY($1)',
    [productIds]
  );
  // Kelompokkan per product jika perlu
  return allReviews;
}
// Atau jika query database tidak support ANY:
async function getProductReviewsParallel(productIds) {
  const queries = productIds.map(id =>
    db.query('SELECT * FROM reviews WHERE product_id = $1', [id])
  );
  const results = await Promise.all(queries);
  return results.flat(); // Gabungkan semua hasil
}
```
</details>

### Soal 3: Hard
Anda memiliki endpoint `/api/products` yang lambat (response 3 detik). Setelah profiling, ditemukan 3 bottleneck: (1) query tanpa pagination, (2) join ke 5 tabel yang tidak perlu, (3) transformasi data besar di JavaScript. Jelaskan langkah optimasi Anda secara berurutan, termasuk bagaimana Anda memverifikasi setiap perbaikan.

<details>
<summary>Jawaban</summary>

**Langkah 1: Ukur baseline**
Catat response time, jumlah data, dan query yang dijalankan. Gunakan `console.time()`, DevTools Network tab, atau APM tool.

**Langkah 2: Fix pagination**
```javascript
app.get('/api/products', async (req, res) => {
  const { page = 1, limit = 20 } = req.query;
  const products = await Product.findAll({ limit, offset: (page - 1) * limit });
  res.json({ data: products, page, total: await Product.count() });
});
```
Verifikasi: response time turun dari 3s → 300ms.

**Langkah 3: Kurangi join yang tidak perlu**
Hanya include tabel yang benar-benar dibutuhkan client. Jika client hanya butuh `name` dan `price`, jangan join `reviews`, `categories`, `inventory`, `suppliers`.
```javascript
const products = await Product.findAll({
  attributes: ['id', 'name', 'price', 'image'],
  limit, offset: (page - 1) * limit,
});
```
Verifikasi: 300ms → 80ms.

**Langkah 4: Pindahkan transformasi data ke database**
Gunakan raw SQL atau Sequelize aggregation untuk perhitungan (sum, avg) — jangan di JavaScript.
```javascript
const products = await db.query(`
  SELECT p.id, p.name, p.price,
         COALESCE(AVG(r.rating), 0) as avg_rating
  FROM products p
  LEFT JOIN reviews r ON r.product_id = p.id
  GROUP BY p.id, p.name, p.price
  LIMIT $1 OFFSET $2
`, [limit, (page - 1) * limit]);
```
Verifikasi: 80ms → 50ms.

**Langkah 5: Tambahkan caching layer (lanjut ke Materi 108)**
```
Cache-Control: public, max-age=60
```
Verifikasi: 50ms → 5ms (cache hit).
</details>
