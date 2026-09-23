# 64 - Cache Strategy — Kapan Cache, Berapa Lama, Apa yang Dicache

## Penjelasan
Redis sudah terpasang seperti meja resepsionis yang siap pakai. Tapi pertanyaannya: **dokumen mana yang boleh ditaruh di meja? Berapa lama?** Tidak semua dokumen cocok ditaruh di meja resepsionis — dokumen rahasia (password) jangan, dokumen yang berubah tiap detik (sensor suhu) percuma. Kita perlu strategi caching yang matang.

## Fungsi
- Menentukan **kebijakan cache** agar tepat sasaran
- Menerapkan **Cache-Aside Pattern** (application reads from cache first)
- Menentukan **TTL (Time To Live)** yang optimal
- Merancang **Cache Key** dengan format `namespace:identifier`
- Membedakan data yang **cacheable** vs **non-cacheable**

## Cara Pengimplementasian

### Cache-Aside Pattern

```
1. Cek cache → jika ada (hit), return data
2. Jika tidak ada (miss), query DB
3. Simpan hasil query ke cache
4. Return data
```

```typescript
async getProduct(id: number) {
  const key = `product:${id}`;
  let product = await this.cacheManager.get(key);

  if (!product) {
    product = await this.prisma.product.findUnique({ where: { id } });
    await this.cacheManager.set(key, product, 120); // TTL 2 menit
  }

  return product;
}
```

### TTL Strategy

| Tipe Data       | TTL      | Contoh                                |
|-----------------|----------|---------------------------------------|
| Statis          | 1-24 jam | Daftar kategori, konfigurasi app      |
| Semi-dinamis    | 1-10 mnt | Produk, artikel blog                  |
| Dinamis         | 10-60 dtk | Stock terbaru, harga cryptocurrency  |
| Sangat dinamis  | Jangan di-cache | Saldo akun, status pesanan        |

### Cache Key Design

Format: `namespace:id` atau `namespace:operation:params`

```
product:1                → produk id 1
product:list:page:1      → halaman 1 daftar produk
user:5:profile           → profil user id 5
category:electronics     → kategori elektronik
```

### Yang Cocok vs Tidak di-Cache

**COCOK (Cacheable):**
1. GET `/products` — daftar produk
2. GET `/products/:id` — detail produk
3. GET `/categories` — daftar kategori
4. GET `/articles` — artikel blog
5. GET `/config/public` — konfigurasi publik

**TIDAK COCOK (Non-Cacheable):**
1. POST `/orders` — mutation, harus real-time
2. GET `/users/me/balance` — saldo harus akurat
3. POST `/auth/login` — kredensial sensitif
4. GET `/admin/dashboard/realtime` — data real-time
5. PATCH `/products/:id` — update data, harus invalidate

## Analogi
Meja resepsionis di gedung bertingkat punya aturan:
- **Brosur event** (daftar produk) → taruh di meja, ganti tiap 2 jam (TTL 120 menit)
- **Denah lantai** (kategori) → tempel di dinding, ganti tiap bulan
- **Password karyawan** → jangan pernah taruh di meja!
- **Jumlah pengunjung real-time** → tidak berguna karena basi sebelum dicetak

## Dipakai untuk Apa
- Mengurangi beban database
- Mempercepat response time API
- Menghemat biaya komputasi
- Meningkatkan user experience

## Kesalahan Umum
- **Over-caching** — semua endpoint di-cache termasuk yang sensitif
- **Cache key bentrok** — `product:1` vs `user:1` tertimpa karena key sama
- **TTL terlalu panjang** — data basi ditampilkan
- **Cache everything (termasuk pagination tanpa parameter)** — list tidak berubah saat ada data baru
- **Tidak handle cache miss dengan baik** — N+1 query karena miss terus

## Soal Latihan

**Soal:**
Buat fungsi `shouldCache(endpoint: string, method: string): boolean` yang mengidentifikasi apakah suatu endpoint sebaiknya di-cache. Berikan 5 contoh endpoint yang cocok dan 5 yang tidak dengan alasannya.

**Jawaban:**

```typescript
type Endpoint = { method: string; path: string };

function shouldCache(ep: Endpoint): { cacheable: boolean; reason: string } {
  const nonCacheablePatterns = [
    { method: 'POST', pattern: /^\/orders/ },
    { method: 'POST', pattern: /^\/auth/ },
    { method: 'GET', pattern: /\/balance$/ },
    { method: 'PATCH', pattern: /^\/products/ },
    { method: 'DELETE', pattern: /^\/products/ },
  ];

  const hit = nonCacheablePatterns.find(
    (p) => p.method === ep.method && p.pattern.test(ep.path),
  );

  if (hit) {
    return { cacheable: false, reason: 'Data mutation atau sensitif' };
  }

  return { cacheable: true, reason: 'Read-only, data relatif statis' };
}

// Contoh penggunaan:
// GET /products          → cacheable (daftar produk jarang berubah)
// GET /products/1        → cacheable (detail produk)
// GET /categories        → cacheable (kategori statis)
// GET /articles          → cacheable (artikel blog)
// GET /config/public     → cacheable (config publik)

// POST /orders           → tidak (mutation)
// GET /users/me/balance  → tidak (data sensitif akurat)
// POST /auth/login       → tidak (kredensial)
// PATCH /products/1      → tidak (update)
// DELETE /products/1     → tidak (delete)
```
