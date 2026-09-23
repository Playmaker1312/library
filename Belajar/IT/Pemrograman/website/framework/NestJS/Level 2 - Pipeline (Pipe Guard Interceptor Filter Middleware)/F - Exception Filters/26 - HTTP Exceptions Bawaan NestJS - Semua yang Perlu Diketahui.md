# HTTP Exceptions Bawaan NestJS — Semua yang Perlu Diketahui

## Penjelasan (Nyambung dari materi sebelumnya)

Setelah request melewati **Pipe** (validasi & transformasi) dan diproses oleh **Controller & Service**, masih ada satu lapisan lagi: **Exception Filter**.

Kita sudah menggunakan `BadRequestException` di custom pipe — itu adalah salah satu dari banyak HTTP exception bawaan NestJS. Exception ini adalah **fondasi** dari error handling yang baik.

---

## Fungsi

- Standarisasi format error response
- Memberi kode HTTP yang tepat untuk setiap skenario error
- Memisahkan logic error dari business logic
- Memudahkan debugging dan monitoring
- Memberi pengalaman error yang konsisten ke client (frontend/mobile)

---

## Hirarki HttpException

```
HttpException (abstract)
├── BadRequestException          (400)
├── UnauthorizedException         (401)
├── ForbiddenException            (403)
├── NotFoundException             (404)
├── MethodNotAllowedException     (405)
├── NotAcceptableException        (406)
├── RequestTimeoutException       (408)
├── ConflictException             (409)
├── UnprocessableEntityException  (422)
├── TooManyRequestsException      (429)
├── InternalServerErrorException  (500)
├── NotImplementedException       (501)
├── BadGatewayException           (502)
├── ServiceUnavailableException   (503)
├── GatewayTimeoutException       (504)
```

---

## Cara Implementasi & Code

### 1. BadRequestException (400)

```typescript
// Skenario: validasi gagal, format input salah
@Post()
create(@Body() dto: CreateProductDto) {
  // ValidationPipe otomatis throw BadRequestException
  // Tapi kita juga bisa throw manual:
  if (!dto.name) {
    throw new BadRequestException('Nama produk wajib diisi');
  }
}
```

Response:
```json
{
  "statusCode": 400,
  "message": "Nama produk wajib diisi",
  "error": "Bad Request"
}
```

### 2. UnauthorizedException (401)

```typescript
// Skenario: user belum login, token tidak valid
@Get('profile')
getProfile(@Req() req: Request) {
  const user = req.user;
  if (!user) {
    throw new UnauthorizedException('Silakan login terlebih dahulu');
  }
}
```

### 3. ForbiddenException (403)

```typescript
// Skenario: user sudah login tapi tidak punya akses
@Delete(':id')
remove(@Param('id') id: string, @CurrentUser() user: UserEntity) {
  if (user.role !== 'admin') {
    throw new ForbiddenException('Hanya admin yang bisa menghapus produk');
  }
}
```

### 4. NotFoundException (404)

```typescript
// Skenario: resource tidak ditemukan
@Get(':id')
async findOne(@Param('id', ParseIntPipe) id: number) {
  const product = await this.productService.findOne(id);
  if (!product) {
    throw new NotFoundException(`Produk dengan ID ${id} tidak ditemukan`);
  }
  return product;
}
```

### 5. ConflictException (409)

```typescript
// Skenario: duplikat data, state conflict
@Post()
async create(@Body() dto: CreateProductDto) {
  const existing = await this.productService.findByName(dto.name);
  if (existing) {
    throw new ConflictException(`Produk dengan nama "${dto.name}" sudah ada`);
  }
  return this.productService.create(dto);
}
```

### 6. UnprocessableEntityException (422)

```typescript
// Skenario: data valid secara format tapi tidak bisa diproses secara logika
@Post('checkout')
async checkout(@Body() dto: CheckoutDto) {
  // Keranjang belanja kosong
  if (dto.items.length === 0) {
    throw new UnprocessableEntityException('Keranjang belanja kosong');
  }
  // Stok tidak mencukupi
  const insufficientStock = await this.checkStock(dto.items);
  if (insufficientStock) {
    throw new UnprocessableEntityException(
      'Stok tidak mencukupi untuk beberapa item',
    );
  }
}
```

### 7. InternalServerErrorException (500)

```typescript
// Skenario: error tidak terduga
@Post()
async create(@Body() dto: CreateProductDto) {
  try {
    return await this.productService.create(dto);
  } catch (error) {
    // Jangan expose detail error ke client
    throw new InternalServerErrorException('Gagal membuat produk');
  }
}
```

### 8. Custom Message & Error Object

```typescript
// 1. String message
throw new NotFoundException('Produk tidak ditemukan');

// 2. Object dengan custom field
throw new BadRequestException({
  statusCode: 400,
  message: 'Validasi gagal',
  errors: [
    { field: 'email', message: 'Format email tidak valid' },
    { field: 'password', message: 'Minimal 8 karakter' },
  ],
  timestamp: new Date().toISOString(),
});

// 3. Dengan error cause (debugging)
throw new BadRequestException('Gagal memproses', {
  cause: originalError,
  description: 'Error saat parsing CSV',
});
```

### 9. 10 Skenario Error Berbeda

```typescript
// src/product/product.controller.ts
@Controller('products')
export class ProductController {
  constructor(private readonly productService: ProductService) {}

  // 1. 400 — Input tidak valid
  @Post()
  create(@Body() dto: CreateProductDto) {
    if (!dto.price || dto.price <= 0) {
      throw new BadRequestException('Harga harus lebih dari 0');
    }
    return this.productService.create(dto);
  }

  // 2. 401 — Belum login
  @Get('profile')
  getProfile(@Req() req: Request) {
    throw new UnauthorizedException('Token tidak ditemukan');
  }

  // 3. 403 — Tidak punya akses
  @Delete(':id')
  remove(@Param('id') id: string) {
    throw new ForbiddenException('Role user tidak diizinkan');
  }

  // 4. 404 — Resource tidak ditemukan
  @Get(':id')
  findOne(@Param('id') id: string) {
    throw new NotFoundException({ productId: id, message: 'Produk tidak ditemukan' });
  }

  // 5. 405 — Method tidak diizinkan
  @Patch(':id')
  update(@Param('id') id: string) {
    throw new MethodNotAllowedException('Method PATCH tidak didukung');
  }

  // 6. 409 — Konflik (duplikat)
  @Post('with-sku')
  createWithSku(@Body() dto: any) {
    throw new ConflictException(`SKU ${dto.sku} sudah digunakan`);
  }

  // 7. 422 — Data tidak bisa diproses
  @Post('validate')
  validate(@Body() dto: any) {
    throw new UnprocessableEntityException({
      message: 'Stok tidak mencukupi',
      available: 5,
      requested: 10,
    });
  }

  // 8. 429 — Rate limit
  @Get('popular')
  getPopular() {
    throw new TooManyRequestsException('Terlalu banyak request, coba 1 menit lagi');
  }

  // 9. 500 — Internal server error
  @Post('import')
  import() {
    throw new InternalServerErrorException('Gagal mengimpor data');
  }

  // 10. 503 — Service tidak tersedia
  @Get('payment-status')
  getPaymentStatus() {
    throw new ServiceUnavailableException('Payment gateway sedang offline');
  }
}
```

### 10. Penggunaan di Service

```typescript
// src/product/product.service.ts
import { Injectable, NotFoundException, ConflictException, BadRequestException } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { Product } from './entities/product.entity';

@Injectable()
export class ProductService {
  constructor(
    @InjectRepository(Product)
    private readonly productRepo: Repository<Product>,
  ) {}

  async findOne(id: number): Promise<Product> {
    const product = await this.productRepo.findOne({ where: { id } });
    
    if (!product) {
      throw new NotFoundException(`Produk dengan ID ${id} tidak ditemukan`);
    }
    
    return product;
  }

  async update(id: number, dto: UpdateProductDto) {
    const product = await this.productRepo.findOne({ where: { id } });
    
    if (!product) {
      throw new NotFoundException(`Produk dengan ID ${id} tidak ditemukan`);
    }

    if (dto.name) {
      const existing = await this.productRepo.findOne({
        where: { name: dto.name },
      });
      if (existing && existing.id !== id) {
        throw new ConflictException(`Nama produk "${dto.name}" sudah ada`);
      }
    }

    if (dto.price && dto.price <= 0) {
      throw new BadRequestException('Harga harus lebih dari 0');
    }

    return this.productRepo.save({ ...product, ...dto });
  }
}
```

---

## Analogi — Membangun Gedung

| HTTP Exception | Analogi Gedung |
|----------------|----------------|
| **200 OK** | Proyek selesai sesuai rencana |
| **400 Bad Request** | "Cetak biru yang Anda kirimkan tidak jelas — gambarnya buram" |
| **401 Unauthorized** | "Siapa Anda? Tunjukkan KTP dulu sebelum masuk proyek" |
| **403 Forbidden** | "Anda terdaftar sebagai tamu, tapi area ini khusus kontraktor" |
| **404 Not Found** | "Lantai 7? Gedung ini cuma 5 lantai" |
| **409 Conflict** | "Maaf, nomor unit A-123 sudah dibeli orang lain" |
| **422 Unprocessable** | "Denah ruangan 5×5 meter, tapi anda minta 10×10 — tidak muat" |
| **429 Too Many** | "Anda sudah 10 kali revisi hari ini, tunggu besok" |
| **500 Internal** | **Gedung ambruk!** Ada kesalahan fondasi yang tidak terduga |
| **503 Unavailable** | "Lift sedang rusak, coba lagi nanti" |

---

## Dipakai Untuk Apa

- **400** — Validasi input gagal (salah format, tipe, range)
- **401** — Auth: token tidak ada / tidak valid / expired
- **403** — Authz: user tidak punya hak akses ke resource
- **404** — Resource tidak ditemukan
- **405** — Method HTTP tidak didukung endpoint
- **409** — Duplikat data / conflict state
- **422** — Business rule violation (stok habis, saldo kurang)
- **429** — Rate limiting / throttling
- **500** — Error internal yang tidak terduga
- **503** — Dependency eksternal down (DB, payment gateway)

---

## Kesalahan Umum

1. **Menggunakan exception yang salah kodenya** — 400 untuk not found, 409 untuk invalid input
2. **Throw InternalServerErrorException untuk error yang bisa diprediksi** — 500 seharusnya untuk error yang *tidak terduga*
3. **Tidak memberikan pesan yang informatif** — "Error" saja tidak membantu debugging
4. **Expose stack trace / detail internal** — jangan kirim `error.stack` ke client
5. **Format response tidak konsisten** — kadang string, kadang object, kadang array
6. **Tidak handle error async** — async function yang throw harus di-handle filter
7. **Lupa throw di service** — service return null, controller tidak ngecek → 500 cryptic

---

## Soal Latihan & Jawaban

### Soal

Buat 10 skenario error di sebuah controller `POST /orders`:

1. Body kosong → 400
2. user tidak login → 401
3. user bukan customer → 403
4. Product tidak ditemukan → 404
5. Method salah (GET bukan POST) → 405 (bisa diabaikan, NestJS handle otomatis)
6. Order sudah dibayar, mau dibatalkan → 409
7. Jumlah item melebihi stok → 422
8. Request terlalu cepat → 429
9. Database connection error → 500
10. Payment gateway down → 503

### Jawaban

```typescript
// src/order/order.controller.ts
import {
  Controller, Post, Body, Req, Get,
  BadRequestException, UnauthorizedException, ForbiddenException,
  NotFoundException, ConflictException, UnprocessableEntityException,
  TooManyRequestsException, InternalServerErrorException,
  ServiceUnavailableException,
} from '@nestjs/common';
import { Request } from 'express';

@Controller('orders')
export class OrderController {
  private requestCount = 0;

  @Post()
  async create(@Body() body: any, @Req() req: Request) {
    // 1. Body kosong
    if (!body || Object.keys(body).length === 0) {
      throw new BadRequestException('Body request tidak boleh kosong');
    }

    // 2. User belum login
    if (!req['user']) {
      throw new UnauthorizedException('Silakan login untuk membuat order');
    }

    // 3. User bukan customer
    if (req['user'].role !== 'customer') {
      throw new ForbiddenException('Hanya customer yang bisa membuat order');
    }

    // 4. Produk tidak ditemukan
    const product = await this.checkProduct(body.productId);
    if (!product) {
      throw new NotFoundException(`Produk dengan ID ${body.productId} tidak ditemukan`);
    }

    // 6. Order sudah dibayar
    const order = await this.findOrder(body.orderId);
    if (order && order.status === 'paid') {
      throw new ConflictException('Order yang sudah dibayar tidak bisa dibatalkan');
    }

    // 7. Stok tidak mencukupi
    if (body.quantity > product.stock) {
      throw new UnprocessableEntityException({
        message: 'Jumlah melebihi stok tersedia',
        requested: body.quantity,
        available: product.stock,
      });
    }

    // 8. Rate limiting sederhana
    this.requestCount++;
    if (this.requestCount > 5) {
      throw new TooManyRequestsException('Terlalu banyak permintaan');
    }

    try {
      // 9 & 10: Simulasi error
      const paymentResult = await this.processPayment();
      if (!paymentResult) {
        throw new ServiceUnavailableException('Payment gateway sedang sibuk');
      }
      return this.orderService.create(body);
    } catch (error) {
      if (error instanceof ServiceUnavailableException) throw error;
      if (error instanceof HttpException) throw error;
      throw new InternalServerErrorException('Gagal membuat order, coba lagi');
    }
  }

  @Get()
  findAll() {
    // 5. Method mismatch — ini GET, endpoint seharusnya POST
    // NestJS otomatis memberikan 405 MethodNotAllowed jika route tidak cocok
    throw new BadRequestException('Gunakan POST untuk membuat order');
  }

  private async checkProduct(id: string) {
    // Simulasi
    return id ? { id, stock: 10 } : null;
  }

  private async findOrder(id: string) {
    return id ? { id, status: 'paid' } : null;
  }

  private async processPayment() {
    return true;
  }
}
```
