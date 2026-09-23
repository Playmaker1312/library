# 19 - Unit Test untuk Controller: Testing HTTP Handler Terisolasi

## Penjelasan

Kita sudah menguji satpam (Service) sendirian. Sekarang saatnya menguji **resepsionis (Controller)** — apakah ia menerima tamu dengan benar, mencatat data yang masuk, dan menyampaikan instruksi yang tepat ke satpam. Sama seperti resepsionis, controller tidak perlu menguji apakah satpam benar-benar bekerja; ia cukup memastikan **perintah yang diberikan ke satpam sudah benar**.

---

## Fungsi

- Memverifikasi controller memanggil service dengan argumen yang tepat
- Memastikan response format sesuai (status code, body)
- Menguji parsing parameter (`@Param`, `@Query`, `@Body`)
- Menguji error handling di layer controller
- Isolasi: kita tahu bug bukan dari service, tapi dari cara controller memanggilnya

---

## Cara Pengimplementasian

### 1. Service yang akan di-mock

```typescript
// src/coffee/coffee.service.ts
@Injectable()
export class CoffeeService {
  async findAll(): Promise<Coffee[]> {
    return this.coffeeRepository.find();
  }

  async findOne(id: number): Promise<Coffee> {
    const coffee = await this.coffeeRepository.findOne({ where: { id } });
    if (!coffee) throw new NotFoundException(`Coffee ${id} tidak ditemukan`);
    return coffee;
  }

  async create(dto: CreateCoffeeDto): Promise<Coffee> {
    const coffee = this.coffeeRepository.create(dto);
    return this.coffeeRepository.save(coffee);
  }

  async remove(id: number): Promise<void> {
    const result = await this.coffeeRepository.delete(id);
    if (result.affected === 0) {
      throw new NotFoundException(`Coffee ${id} tidak ditemukan`);
    }
  }
}
```

### 2. Controller yang akan di-test

```typescript
// src/coffee/coffee.controller.ts
import { Controller, Get, Post, Delete, Param, Body, HttpCode } from '@nestjs/common';
import { CoffeeService } from './coffee.service';
import { CreateCoffeeDto } from './dto/create-coffee.dto';

@Controller('coffee')
export class CoffeeController {
  constructor(private readonly coffeeService: CoffeeService) {}

  @Get()
  async findAll() {
    const data = await this.coffeeService.findAll();
    return { data, message: 'Berhasil', statusCode: 200 };
  }

  @Get(':id')
  async findOne(@Param('id') id: string) {
    const data = await this.coffeeService.findOne(+id);
    return { data, message: 'Berhasil', statusCode: 200 };
  }

  @Post()
  @HttpCode(201)
  async create(@Body() dto: CreateCoffeeDto) {
    const data = await this.coffeeService.create(dto);
    return { data, message: 'Coffee dibuat', statusCode: 201 };
  }

  @Delete(':id')
  @HttpCode(204)
  async remove(@Param('id') id: string): Promise<void> {
    await this.coffeeService.remove(+id);
  }
}
```

### 3. Mock Service

```typescript
const mockCoffeeService = {
  findAll: jest.fn(),
  findOne: jest.fn(),
  create: jest.fn(),
  remove: jest.fn(),
};
```

### 4. Unit Test Controller Lengkap

```typescript
// src/coffee/coffee.controller.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { CoffeeController } from './coffee.controller';
import { CoffeeService } from './coffee.service';
import { NotFoundException } from '@nestjs/common';

describe('CoffeeController', () => {
  let controller: CoffeeController;
  let service: typeof mockCoffeeService;

  const mockCoffeeService = {
    findAll: jest.fn(),
    findOne: jest.fn(),
    create: jest.fn(),
    remove: jest.fn(),
  };

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      controllers: [CoffeeController],
      providers: [
        {
          provide: CoffeeService,
          useValue: mockCoffeeService,
        },
      ],
    }).compile();

    controller = module.get<CoffeeController>(CoffeeController);
    service = module.get(CoffeeService);
  });

  afterEach(() => {
    jest.clearAllMocks();
  });

  // --- Test findAll ---
  describe('findAll', () => {
    it('harus return semua coffee dengan format response', async () => {
      // Arrange
      const expectedCoffees = [
        { id: 1, name: 'Kopi Luwak', price: 50000 },
      ];
      mockCoffeeService.findAll.mockResolvedValue(expectedCoffees);

      // Act
      const result = await controller.findAll();

      // Assert
      expect(result).toEqual({
        data: expectedCoffees,
        message: 'Berhasil',
        statusCode: 200,
      });
      expect(service.findAll).toHaveBeenCalledTimes(1);
    });

    it('harus return array kosong jika tidak ada coffee', async () => {
      mockCoffeeService.findAll.mockResolvedValue([]);

      const result = await controller.findAll();

      expect(result.data).toEqual([]);
    });
  });

  // --- Test findOne ---
  describe('findOne', () => {
    it('harus return coffee berdasarkan id', async () => {
      // Arrange
      const expectedCoffee = { id: 1, name: 'Kopi Luwak', price: 50000 };
      mockCoffeeService.findOne.mockResolvedValue(expectedCoffee);

      // Act
      const result = await controller.findOne('1');

      // Assert — verifikasi controller benar mengirim +id (conversion)
      expect(result).toEqual({
        data: expectedCoffee,
        message: 'Berhasil',
        statusCode: 200,
      });
      expect(service.findOne).toHaveBeenCalledWith(1); // angka, bukan string
    });

    it('harus throw error jika service throw', async () => {
      // Arrange
      mockCoffeeService.findOne.mockRejectedValue(
        new NotFoundException('Coffee 999 tidak ditemukan'),
      );

      // Act & Assert
      await expect(controller.findOne('999')).rejects.toThrow(NotFoundException);
      expect(service.findOne).toHaveBeenCalledWith(999);
    });
  });

  // --- Test create ---
  describe('create', () => {
    it('harus membuat coffee baru dan return 201', async () => {
      // Arrange
      const dto = { name: 'Kopi Baru', price: 25000 };
      const createdCoffee = { id: 3, ...dto };
      mockCoffeeService.create.mockResolvedValue(createdCoffee);

      // Act
      const result = await controller.create(dto);

      // Assert
      expect(result).toEqual({
        data: createdCoffee,
        message: 'Coffee dibuat',
        statusCode: 201,
      });
      expect(service.create).toHaveBeenCalledWith(dto);
      expect(service.create).toHaveBeenCalledTimes(1);
    });

    it('harus throw error jika create gagal di service', async () => {
      mockCoffeeService.create.mockRejectedValue(new Error('DB error'));

      await expect(controller.create({ name: 'X', price: 0 })).rejects.toThrow('DB error');
    });
  });

  // --- Test remove ---
  describe('remove', () => {
    it('harus menghapus coffee dan return void (204)', async () => {
      // Arrange
      mockCoffeeService.remove.mockResolvedValue(undefined);

      // Act
      const result = await controller.remove('1');

      // Assert
      expect(result).toBeUndefined();
      expect(service.remove).toHaveBeenCalledWith(1);
    });

    it('harus throw NotFound jika id tidak ada', async () => {
      mockCoffeeService.remove.mockRejectedValue(
        new NotFoundException('Coffee 999 tidak ditemukan'),
      );

      await expect(controller.remove('999')).rejects.toThrow(NotFoundException);
    });
  });
});
```

### 5. Testing Response dengan Argument Verification Detail

```typescript
describe('Argument verification', () => {
  it('harus mengkonversi string id ke number', async () => {
    mockCoffeeService.findOne.mockResolvedValue({ id: 1, name: 'Test' });

    await controller.findOne('42');

    // Verifikasi bahwa controller mengirim number, bukan string
    expect(service.findOne).toHaveBeenCalledWith(42);
    expect(service.findOne).not.toHaveBeenCalledWith('42');
  });

  it('harus mengirim DTO yang sama ke service', async () => {
    const dto = { name: 'Kopi Spesial', price: 100000 };
    mockCoffeeService.create.mockResolvedValue({ id: 1, ...dto });

    await controller.create(dto);

    // Pastikan object yang sama (referensi) dikirim
    expect(service.create).toHaveBeenCalledWith(dto);
  });
});
```

### 6. Testing dengan Query & Custom Decorator

```typescript
// Controller dengan query parameters
@Get()
async findAll(@Query('page') page: string, @Query('limit') limit: string) {
  return this.coffeeService.findAll(+page, +limit);
}

// Test
it('harus mengirim query parameter sebagai number', async () => {
  mockCoffeeService.findAll.mockResolvedValue([]);

  await controller.findAll('2', '10');

  expect(service.findAll).toHaveBeenCalledWith(2, 10);
});
```

---

## Analogi: Gedung Bertingkat

| Controller Test | Analogi Gedung |
|---|---|
| **Mock Service** | Resepsionis berbicara dengan **satpam palsu** yang selalu menjawab sesuai skenario |
| `toHaveBeenCalledWith(arg)` | Memeriksa **apakah resepsionis menyebutkan nama dan lantai yang benar** ke satpam |
| `findOne('42')` → `+id` | Resepsionis mengkonversi **"nol empat dua"** (string) jadi **42** (angka) |
| `mockResolvedValue(data)` | Satpam pura-pura bilang **"Data aman, ini dia"** |
| `mockRejectedValue(error)` | Satpam pura-pura bilang **"Ada masalah! Data hilang!"** |
| **Format response `{data, message, statusCode}`** | Resepsionis **menulis surat balasan dengan format baku** |
| **`@HttpCode(204)`** | Resepsionis menjawab **tanpa surat** (void) — hanya isyarat "selesai" |

---

## Dipakai Untuk Apa

- Memastikan controller memanggil service dengan benar
- Validasi transformasi parameter (`string` ke `number`, parsing)
- Testing response format
- Memastikan error dari service diteruskan ke client
- Verifikasi status code berbeda tiap endpoint

---

## Kesalahan Umum

1. **Lupa mock service** — test menggunakan service asli yang butuh DB → error.
2. **Tidak reset mock** — test sebelumnya mempengaruhi test berikutnya. Gunakan `jest.clearAllMocks()`.
3. **Menguji service, bukan controller** — focus di cara controller **memanggil** service, bukan hasil service.
4. **Parameter string vs number** — `@Param()` selalu string. Pastikan test mengirim string dan controller melakukan konversi.
5. **Tidak menguji error path** — pastikan error dari service di-throw ulang oleh controller.
6. **Over-specification** — terlalu spesifik memeriksa implementasi internal yang sering berubah.

---

## Soal Latihan

### Soal 1
Buat controller `ProductController` dengan:
- `GET /products` → panggil `productService.findAll()`, return format `{ data, message, statusCode: 200 }`
- `GET /products/:id` → panggil `productService.findOne(+id)`, return format yang sama
- `POST /products` → panggil `productService.create(dto)`, return format `{ data, message, statusCode: 201 }`
- `DELETE /products/:id` → panggil `productService.remove(+id)`, return void (204)

Tulis unit test lengkap yang mencakup happy path dan error path untuk setiap method. Sertakan juga test untuk verifikasi konversi id string → number.

<details>
<summary>Jawaban</summary>

```typescript
// src/product/product.controller.ts
import { Controller, Get, Post, Delete, Param, Body, HttpCode } from '@nestjs/common';
import { ProductService } from './product.service';
import { CreateProductDto } from './dto/create-product.dto';

@Controller('products')
export class ProductController {
  constructor(private readonly productService: ProductService) {}

  @Get()
  async findAll() {
    const data = await this.productService.findAll();
    return { data, message: 'Berhasil', statusCode: 200 };
  }

  @Get(':id')
  async findOne(@Param('id') id: string) {
    const data = await this.productService.findOne(+id);
    return { data, message: 'Berhasil', statusCode: 200 };
  }

  @Post()
  @HttpCode(201)
  async create(@Body() dto: CreateProductDto) {
    const data = await this.productService.create(dto);
    return { data, message: 'Produk dibuat', statusCode: 201 };
  }

  @Delete(':id')
  @HttpCode(204)
  async remove(@Param('id') id: string): Promise<void> {
    await this.productService.remove(+id);
  }
}

// src/product/product.controller.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { ProductController } from './product.controller';
import { ProductService } from './product.service';
import { NotFoundException } from '@nestjs/common';

describe('ProductController', () => {
  let controller: ProductController;
  let service: any;

  const mockProductService = {
    findAll: jest.fn(),
    findOne: jest.fn(),
    create: jest.fn(),
    remove: jest.fn(),
  };

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      controllers: [ProductController],
      providers: [
        { provide: ProductService, useValue: mockProductService },
      ],
    }).compile();

    controller = module.get<ProductController>(ProductController);
    service = module.get(ProductService);
  });

  afterEach(() => jest.clearAllMocks());

  // FIND ALL
  describe('findAll', () => {
    it('harus return daftar produk', async () => {
      const products = [{ id: 1, name: 'A', price: 1000 }];
      mockProductService.findAll.mockResolvedValue(products);

      const result = await controller.findAll();

      expect(result).toEqual({ data: products, message: 'Berhasil', statusCode: 200 });
      expect(service.findAll).toHaveBeenCalledTimes(1);
    });

    it('harus return array kosong jika tidak ada produk', async () => {
      mockProductService.findAll.mockResolvedValue([]);
      const result = await controller.findAll();
      expect(result.data).toEqual([]);
    });

    it('harus throw error jika service gagal', async () => {
      mockProductService.findAll.mockRejectedValue(new Error('DB down'));
      await expect(controller.findAll()).rejects.toThrow('DB down');
    });
  });

  // FIND ONE
  describe('findOne', () => {
    it('harus return produk berdasarkan id', async () => {
      const product = { id: 1, name: 'A', price: 1000 };
      mockProductService.findOne.mockResolvedValue(product);

      const result = await controller.findOne('1');

      expect(result).toEqual({ data: product, message: 'Berhasil', statusCode: 200 });
      expect(service.findOne).toHaveBeenCalledWith(1); // number, bukan string
    });

    it('harus throw NotFound jika produk tidak ditemukan', async () => {
      mockProductService.findOne.mockRejectedValue(
        new NotFoundException('Produk 999 tidak ditemukan'),
      );
      await expect(controller.findOne('999')).rejects.toThrow(NotFoundException);
      expect(service.findOne).toHaveBeenCalledWith(999);
    });
  });

  // CREATE
  describe('create', () => {
    it('harus membuat produk baru', async () => {
      const dto = { name: 'Produk Baru', price: 50000 };
      const created = { id: 1, ...dto };
      mockProductService.create.mockResolvedValue(created);

      const result = await controller.create(dto);

      expect(result).toEqual({ data: created, message: 'Produk dibuat', statusCode: 201 });
      expect(service.create).toHaveBeenCalledWith(dto);
    });

    it('harus throw error jika create gagal', async () => {
      mockProductService.create.mockRejectedValue(new Error('Validation error'));
      await expect(controller.create({ name: '', price: 0 })).rejects.toThrow();
    });
  });

  // REMOVE
  describe('remove', () => {
    it('harus menghapus produk dan return void', async () => {
      mockProductService.remove.mockResolvedValue(undefined);

      const result = await controller.remove('1');

      expect(result).toBeUndefined();
      expect(service.remove).toHaveBeenCalledWith(1);
    });

    it('harus throw NotFound jika produk tidak ada', async () => {
      mockProductService.remove.mockRejectedValue(
        new NotFoundException('Produk 999 tidak ditemukan'),
      );
      await expect(controller.remove('999')).rejects.toThrow(NotFoundException);
    });
  });

  // KONVERSI ID
  describe('konversi parameter', () => {
    it('harus mengkonversi string id ke number untuk findOne', async () => {
      mockProductService.findOne.mockResolvedValue({ id: 42, name: 'Test' });
      await controller.findOne('42');
      expect(service.findOne).toHaveBeenCalledWith(42);
    });

    it('harus mengkonversi string id ke number untuk remove', async () => {
      mockProductService.remove.mockResolvedValue(undefined);
      await controller.remove('99');
      expect(service.remove).toHaveBeenCalledWith(99);
    });
  });
});
```
</details>

### Soal 2
Apa yang dimaksud dengan **isolasi** dalam unit test controller? Mengapa kita membutuhkan mock service?

<details>
<summary>Jawaban</summary>

**Isolasi** berarti kita menguji **satu unit** (controller) tanpa melibatkan dependency aslinya (service, database, API). Kita menggunakan **mock service** agar:

1. **Fokus** — test hanya gagal jika controller salah, bukan karena service error.
2. **Kecepatan** — tidak perlu koneksi DB, test berjalan milidetik.
3. **Kontrol** — kita bisa mensimulasikan skenario apa pun (service return data, throw error, return null, dll) tanpa setup data riil.
4. **Deterministik** — hasil test selalu sama, tidak tergantung keadaan DB.

Contoh tanpa isolasi: controller panggil service asli → service butuh DB → DB sedang mati → test gagal. Padahal controller-nya benar. Dengan mock, kita tahu pasti apakah controller yang salah atau bukan.
</details>
