# 66 - Advanced Caching — Distributed Lock & Cache Warming

## Penjelasan
Di file sebelumnya kita pakai cache-aside sederhana. Tapi ada masalah: ketika 1000 request masuk bersamaan untuk data yang **belum ada di cache**, semuanya akan langsung ke database — ini disebut **thundering herd** (kawanan kuda berlari bersama). Ditambah, saat aplikasi restart, cache kosong melompong. Ibarat gedung, thundering herd adalah 1000 tamu datang bersamaan dan semuanya antre ke gudang arsip karena brosur di meja resepsionis habis. Distributed lock adalah **satpam yang hanya mengizinkan 1 orang** masuk gudang, sisanya tunggu.

## Fungsi
- **Distributed Lock** dengan `SETNX` mencegah race condition / thundering herd
- **Cache Warming** mengisi cache saat startup aplikasi
- **Tag-based Invalidation** menghapus cache berdasarkan grup/tag

## Cara Pengimplementasian

### 1. Distributed Lock Pattern

```typescript
import { Injectable, Inject } from '@nestjs/common';
import { CACHE_MANAGER } from '@nestjs/cache-manager';
import { Cache } from 'cache-manager';

@Injectable()
export class LockService {
  constructor(@Inject(CACHE_MANAGER) private cacheManager: Cache) {}

  async acquireLock(key: string, ttl = 5): Promise<boolean> {
    // SETNX: set if not exists — hanya 1 yang berhasil
    const result = await (this.cacheManager as any).store.client.set(
      `lock:${key}`,
      'locked',
      'NX',
      'EX',
      ttl,
    );
    return result === 'OK';
  }

  async releaseLock(key: string): Promise<void> {
    await this.cacheManager.del(`lock:${key}`);
  }
}
```

### 2. Thundering Herd Prevention

```typescript
async getProductWithLock(id: number): Promise<Product> {
  const cacheKey = `product:${id}`;
  const lockKey = `lock:product:${id}`;

  // 1. Cek cache dulu
  let product = await this.cacheManager.get<Product>(cacheKey);
  if (product) return product;

  // 2. Coba acquire lock
  const locked = await this.lockService.acquireLock(lockKey);
  if (!locked) {
    // 3. Gagal lock — request lain sedang ambil data, tunggu sebentar
    await new Promise((resolve) => setTimeout(resolve, 100));
    // 4. Coba baca cache lagi (request lain sudah isi)
    product = await this.cacheManager.get<Product>(cacheKey);
    if (product) return product;
    throw new Error('Gagal获取数据, coba lagi');
  }

  try {
    // 5. Double check cache setelah lock
    product = await this.cacheManager.get<Product>(cacheKey);
    if (product) return product;

    // 6. Query DB dan simpan ke cache
    product = await this.prisma.product.findUnique({ where: { id } });
    await this.cacheManager.set(cacheKey, product, 120);
    return product;
  } finally {
    // 7. Lepas lock
    await this.lockService.releaseLock(lockKey);
  }
}
```

### 3. Cache Warming (Startup)

```typescript
@Injectable()
export class CacheWarmingService implements OnApplicationBootstrap {
  constructor(
    @Inject(CACHE_MANAGER) private cacheManager: Cache,
    private productService: ProductService,
    private categoryService: CategoryService,
  ) {}

  async onApplicationBootstrap() {
    await this.warmCategories();
    await this.warmTopProducts();
    console.log('Cache warming selesai');
  }

  private async warmCategories() {
    const categories = await this.categoryService.findAll();
    await this.cacheManager.set('categories:all', categories, 3600);
  }

  private async warmTopProducts() {
    const topProducts = await this.productService.findTop(10);
    await this.cacheManager.set('products:top:10', topProducts, 300);
  }
}
```

### 4. Tag-Based Invalidation

```typescript
@Injectable()
export class TaggedCacheService {
  constructor(@Inject(CACHE_MANAGER) private cacheManager: Cache) {}

  async setWithTag(key: string, value: any, tag: string, ttl = 60) {
    await this.cacheManager.set(key, value, ttl);
    // Simpan daftar key per tag
    const tagKey = `tag:${tag}`;
    const keys = await this.cacheManager.get<string[]>(tagKey) || [];
    keys.push(key);
    await this.cacheManager.set(tagKey, [...new Set(keys)], ttl + 60);
  }

  async invalidateTag(tag: string) {
    const tagKey = `tag:${tag}`;
    const keys = await this.cacheManager.get<string[]>(tagKey) || [];
    await Promise.all([
      ...keys.map((k) => this.cacheManager.del(k)),
      this.cacheManager.del(tagKey),
    ]);
  }
}

// Contoh penggunaan:
// await cacheService.setWithTag('product:1', product, 'product');
// await cacheService.setWithTag('product:2', product2, 'product');
// await cacheService.invalidateTag('product'); // hapus semua product cache
```

## Analogi
- **Distributed Lock (SETNX)** = Satpam di pintu gudang arsip yang cuma kasih 1 orang masuk. Sisanya nunggu di luar sampai orang pertama keluar.
- **Thundering Herd** = 1000 orang gratisan berebut masuk mall yang baru buka.
- **Cache Warming** = Sebelum mall buka, petugas sudah isi brosur, denah, dan daftar toko ke meja informasi.
- **Tag-based Invalidation** = "Hapus semua brosur tentang lantai 3" — tidak perlu hapus satu-satu.

## Dipakai untuk Apa
- Endpoint dengan traffic sangat tinggi
- Data yang mahal untuk di-query (aggregation, join 10 tabel)
- Startup aplikasi yang ingin performa optimal sejak detik pertama
- Sistem e-commerce saat flash sale

## Kesalahan Umum
- **Lock tidak di-release** — menyebabkan deadlock, app hang
- **Lock TTL terlalu pendek** — lock lepas sebelum query selesai
- **Lock TTL terlalu panjang** — request lain keburu timeout
- **Cache warming terlalu berat** — app jadi lambat startup
- **Tidak handle edge case lock gagal** — request throw error mentah-mentah

## Soal Latihan

**Soal:**
Buat implementasi distributed lock untuk endpoint GET `/products/:id` dengan thundering herd protection. Gunakan SETNX dan double-check pattern.

**Jawaban:**

```typescript
@Injectable()
export class ProductService {
  constructor(
    @Inject(CACHE_MANAGER) private cacheManager: Cache,
    private prisma: PrismaService,
  ) {}

  async getProductWithLock(id: number): Promise<Product> {
    const cacheKey = `product:${id}`;
    const lockKey = `lock:product:${id}`;
    const maxRetries = 5;
    let retries = 0;

    while (retries < maxRetries) {
      // Cek cache
      let product = await this.cacheManager.get<Product>(cacheKey);
      if (product) return product;

      // Acquire lock (SETNX via ioredis)
      const acquired = await (this.cacheManager.store as any).client.set(
        lockKey,
        '1',
        'NX',
        'EX',
        3,
      );

      if (acquired === 'OK') {
        try {
          // Double check
          product = await this.cacheManager.get<Product>(cacheKey);
          if (product) return product;

          product = await this.prisma.product.findUnique({ where: { id } });
          if (!product) return null;

          await this.cacheManager.set(cacheKey, product, 120);
          return product;
        } finally {
          await this.cacheManager.del(lockKey);
        }
      }

      // Gagal lock — tunggu sebentar
      retries++;
      await new Promise((r) => setTimeout(r, 50 * retries));
    }

    // Fallback: ambil langsung dari DB (tanpa cache)
    return this.prisma.product.findUnique({ where: { id } });
  }
}
```
