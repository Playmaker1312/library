# 65 - Implementasi Caching — Dekorator & Manual Cache

## Penjelasan
Setelah tahu strategi caching (file 64), sekarang kita terapkan langsung ke kode. NestJS menyediakan dua cara: **dekorator** (mudah, cepat) dan **manual** (fleksibel). Ibarat gedung, dekorator itu seperti **dispenser air otomatis** — tinggal pencet, air keluar. Manual cache seperti **keran di wastafel dapur** — Anda bisa atur debit dan suhu air sesuai kebutuhan.

## Fungsi
- `@CacheKey(key)` — menentukan custom key untuk cache
- `@CacheTTL(ttl)` — menentukan TTL per endpoint
- `@UseInterceptors(CacheInterceptor)` — cache otomatis response GET
- `cacheManager.get(key)` — baca dari cache
- `cacheManager.set(key, value, ttl)` — tulis ke cache
- `cacheManager.del(key)` — hapus dari cache (invalidate)
- `CacheInterceptor` — hanya bekerja pada **GET** endpoint secara default

## Cara Pengimplementasian

### 1. Auto-Cache dengan Dekorator

```typescript
import { Controller, Get, Param, UseInterceptors } from '@nestjs/common';
import { CacheKey, CacheTTL, CacheInterceptor } from '@nestjs/cache-manager';

@Controller('products')
@UseInterceptors(CacheInterceptor) // auto-cache semua GET
export class ProductController {
  constructor(private readonly productService: ProductService) {}

  @Get()
  @CacheKey('products:all') // custom key
  @CacheTTL(60) // override TTL 60 detik
  async findAll() {
    return this.productService.findAll();
  }

  @Get(':id')
  @CacheKey('product:detail')
  @CacheTTL(120)
  async findOne(@Param('id') id: string) {
    return this.productService.findOne(+id);
  }
}
```

### 2. Manual Cache (Lebih Kontrol)

```typescript
@Injectable()
export class ProductService {
  constructor(
    @Inject(CACHE_MANAGER) private cacheManager: Cache,
    private prisma: PrismaService,
  ) {}

  async findOne(id: number) {
    const key = `product:${id}`;
    const cached = await this.cacheManager.get<Product>(key);
    if (cached) return cached;

    const product = await this.prisma.product.findUnique({ where: { id } });
    await this.cacheManager.set(key, product, 120);
    return product;
  }

  async findAll(page = 1) {
    const key = `products:page:${page}`;
    const cached = await this.cacheManager.get<Product[]>(key);
    if (cached) return cached;

    const products = await this.prisma.product.findMany({
      skip: (page - 1) * 10,
      take: 10,
    });
    await this.cacheManager.set(key, products, 60);
    return products;
  }

  async create(data: CreateProductDto) {
    const product = await this.prisma.product.create({ data });
    // Invalidate cache daftar produk
    await this.cacheManager.del('products:all');
    return product;
  }

  async update(id: number, data: UpdateProductDto) {
    const product = await this.prisma.product.update({ where: { id }, data });
    // Invalidate cache spesifik + daftar
    await this.cacheManager.del(`product:${id}`);
    await this.cacheManager.del('products:all');
    return product;
  }

  async remove(id: number) {
    await this.prisma.product.delete({ where: { id } });
    await this.cacheManager.del(`product:${id}`);
    await this.cacheManager.del('products:all');
  }
}
```

### 3. Cache Invalidation Pattern

Buat helper untuk invalidasi terpusat:

```typescript
@Injectable()
export class CacheInvalidationService {
  constructor(@Inject(CACHE_MANAGER) private cacheManager: Cache) {}

  async invalidateProduct(id: number) {
    await Promise.all([
      this.cacheManager.del(`product:${id}`),
      this.cacheManager.del('products:all'),
      this.cacheManager.del('products:page:1'),
      this.cacheManager.del('products:page:2'),
    ]);
  }
}
```

## Analogi
- **CacheInterceptor + dekorator** = **lift otomatis** — kamu masuk, tekan tombol, lift langsung antar ke lantai tujuan. Simple, terbatas.
- **Manual cache** = **tangga darurat** — lebih lambat, tapi kamu bisa mampir ke lantai mana saja, buka pintu darurat, atau ambil jalur berbeda.
- **Invalidasi** = **ganti brosur di meja resepsionis** — setiap kali ada info baru, brosur lama harus dibuang.

## Dipakai untuk Apa
- GET endpoint dengan data yang jarang berubah
- API publik dengan traffic tinggi
- Halaman homepage, daftar produk, kategori
- Data aggregasi (jumlah user, total orders)

## Kesalahan Umum
- **CacheInterceptor tanpa pengecualian** — endpoint POST ikut di-cache (tidak bekerja sih, tapi membingungkan)
- **Lupa invalidasi** — setelah create/update, data lama masih ditampilkan
- **CacheKey bentrok antar controller** — dua endpoint beda controller pakai key sama
- **CacheTTL terlalu pendek/panjang** — tidak optimal
- **Cache data yang butuh authorization** — user A lihat data user B

## Soal Latihan

**Soal:**
Buat implementasi caching untuk endpoint GET `/products/:id` (cache 2 menit) dan invalidasi cache saat POST `/products` (create) dan PUT `/products/:id` (update). Gunakan kombinasi dekorator dan manual injection.

**Jawaban:**

```typescript
// product.controller.ts
@Controller('products')
export class ProductController {
  constructor(private readonly productService: ProductService) {}

  @Get(':id')
  @CacheKey('product:detail')
  @CacheTTL(120)
  async findOne(@Param('id') id: string) {
    return this.productService.findOne(+id);
  }

  @Post()
  async create(@Body() dto: CreateProductDto) {
    return this.productService.create(dto);
  }

  @Put(':id')
  async update(@Param('id') id: string, @Body() dto: UpdateProductDto) {
    return this.productService.update(+id, dto);
  }
}

// product.service.ts
@Injectable()
export class ProductService {
  constructor(
    @Inject(CACHE_MANAGER) private cacheManager: Cache,
    private prisma: PrismaService,
  ) {}

  async findOne(id: number) {
    // CacheInterceptor handle ini, tapi tetap panggil DB
    return this.prisma.product.findUnique({ where: { id } });
  }

  async create(dto: CreateProductDto) {
    const product = await this.prisma.product.create({ data: dto });
    await Promise.all([
      this.cacheManager.del('product:detail'),
      this.cacheManager.del('products:all'),
    ]);
    return product;
  }

  async update(id: number, dto: UpdateProductDto) {
    const product = await this.prisma.product.update({ where: { id }, data: dto });
    await Promise.all([
      this.cacheManager.del(`product:${id}`),
      this.cacheManager.del('product:detail'),
      this.cacheManager.del('products:all'),
    ]);
    return product;
  }
}
```
