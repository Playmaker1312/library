# 18 - Unit Test untuk Service: Mocking Dependency & Isolasi

## Penjelasan

Di materi sebelumnya kita menguji fungsi utility murni — seperti menguji **bata** di laboratorium. Sekarang kita menguji **satpam (Service)** yang bekerja sama dengan tim lain: resepsionis (Controller) dan kurir (Repository/Prisma/HttpService). Saat menguji satpam, kita tidak ingin terganggu apakah kurir sedang mogok atau tidak. Kita **memalsukan (mock)** kurir agar satpam bisa diuji dalam isolasi.

---

## Fungsi

- Menguji logika bisnis di service tanpa dependency sungguhan (DB, API eksternal)
- Memverifikasi cabang kondisi: happy path, error path, edge cases
- Memastikan service memanggil dependency dengan argumen yang benar
- Isolasi bug — apakah bug di service atau di dependency?

---

## Cara Pengimplementasian

### 1. Service dengan Dependency

```typescript
// src/coffee/coffee.service.ts
import { Injectable, NotFoundException } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { Coffee } from './entities/coffee.entity';
import { CreateCoffeeDto } from './dto/create-coffee.dto';

@Injectable()
export class CoffeeService {
  constructor(
    @InjectRepository(Coffee)
    private readonly coffeeRepository: Repository<Coffee>,
  ) {}

  async findAll(): Promise<Coffee[]> {
    return this.coffeeRepository.find();
  }

  async findOne(id: number): Promise<Coffee> {
    const coffee = await this.coffeeRepository.findOne({ where: { id } });
    if (!coffee) {
      throw new NotFoundException(`Coffee dengan id ${id} tidak ditemukan`);
    }
    return coffee;
  }

  async create(dto: CreateCoffeeDto): Promise<Coffee> {
    const coffee = this.coffeeRepository.create(dto);
    return this.coffeeRepository.save(coffee);
  }

  async remove(id: number): Promise<void> {
    const result = await this.coffeeRepository.delete(id);
    if (result.affected === 0) {
      throw new NotFoundException(`Coffee dengan id ${id} tidak ditemukan`);
    }
  }
}
```

### 2. Mock Object Manual dengan `jest.fn()`

```typescript
// Mock manual untuk repository
const mockCoffeeRepository = {
  find: jest.fn(),
  findOne: jest.fn(),
  create: jest.fn(),
  save: jest.fn(),
  delete: jest.fn(),
};
```

### 3. Unit Test Lengkap dengan Mock

```typescript
// src/coffee/coffee.service.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { CoffeeService } from './coffee.service';
import { getRepositoryToken } from '@nestjs/typeorm';
import { Coffee } from './entities/coffee.entity';
import { NotFoundException } from '@nestjs/common';

describe('CoffeeService', () => {
  let service: CoffeeService;
  let repository: jest.Mocked<Partial<typeof mockCoffeeRepository>>;

  const mockCoffeeRepository = {
    find: jest.fn(),
    findOne: jest.fn(),
    create: jest.fn(),
    save: jest.fn(),
    delete: jest.fn(),
  };

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        CoffeeService,
        {
          provide: getRepositoryToken(Coffee),
          useValue: mockCoffeeRepository,
        },
      ],
    }).compile();

    service = module.get<CoffeeService>(CoffeeService);
    repository = module.get(getRepositoryToken(Coffee));
  });

  afterEach(() => {
    jest.clearAllMocks(); // reset mock setelah setiap test
  });

  // --- Happy Path ---
  describe('findAll', () => {
    it('harus mengembalikan semua coffee', async () => {
      // Arrange
      const expectedCoffees = [
        { id: 1, name: 'Kopi Luwak', price: 50000 },
        { id: 2, name: 'Kopi Arabika', price: 30000 },
      ];
      mockCoffeeRepository.find.mockResolvedValue(expectedCoffees);

      // Act
      const result = await service.findAll();

      // Assert
      expect(result).toEqual(expectedCoffees);
      expect(mockCoffeeRepository.find).toHaveBeenCalledTimes(1);
    });
  });

  // --- Happy Path ---
  describe('findOne', () => {
    it('harus mengembalikan coffee berdasarkan id', async () => {
      // Arrange
      const expectedCoffee = { id: 1, name: 'Kopi Luwak', price: 50000 };
      mockCoffeeRepository.findOne.mockResolvedValue(expectedCoffee);

      // Act
      const result = await service.findOne(1);

      // Assert
      expect(result).toEqual(expectedCoffee);
      expect(mockCoffeeRepository.findOne).toHaveBeenCalledWith({
        where: { id: 1 },
      });
    });
  });

  // --- Error Path ---
  it('harus throw NotFoundException jika coffee tidak ditemukan', async () => {
    // Arrange
    mockCoffeeRepository.findOne.mockResolvedValue(null);

    // Act & Assert
    await expect(service.findOne(999)).rejects.toThrow(
      new NotFoundException('Coffee dengan id 999 tidak ditemukan'),
    );
    expect(mockCoffeeRepository.findOne).toHaveBeenCalledWith({
      where: { id: 999 },
    });
  });

  // --- Happy Path ---
  describe('create', () => {
    it('harus membuat coffee baru', async () => {
      // Arrange
      const dto = { name: 'Kopi Baru', price: 25000 };
      const createdCoffee = { id: 3, ...dto };
      mockCoffeeRepository.create.mockReturnValue(createdCoffee);
      mockCoffeeRepository.save.mockResolvedValue(createdCoffee);

      // Act
      const result = await service.create(dto);

      // Assert
      expect(result).toEqual(createdCoffee);
      expect(mockCoffeeRepository.create).toHaveBeenCalledWith(dto);
      expect(mockCoffeeRepository.save).toHaveBeenCalledWith(createdCoffee);
    });
  });

  // --- Error Path ---
  describe('remove', () => {
    it('harus menghapus coffee jika ada', async () => {
      // Arrange
      mockCoffeeRepository.delete.mockResolvedValue({ affected: 1 });

      // Act
      await service.remove(1);

      // Assert
      expect(mockCoffeeRepository.delete).toHaveBeenCalledWith(1);
    });

    it('harus throw NotFoundException jika coffee tidak ada', async () => {
      // Arrange
      mockCoffeeRepository.delete.mockResolvedValue({ affected: 0 });

      // Act & Assert
      await expect(service.remove(999)).rejects.toThrow(
        new NotFoundException('Coffee dengan id 999 tidak ditemukan'),
      );
    });
  });
});
```

### 4. `jest.spyOn()` — Memata-matai Method

Gunakan `jest.spyOn()` ketika kita ingin mengamati method asli tanpa mengganti implementasinya:

```typescript
import * as mathHelper from '../common/helpers/math.helper';

describe('CoffeeService dengan spy', () => {
  it('harus memanggil mathHelper.add saat menghitung total harga', () => {
    const spy = jest.spyOn(mathHelper, 'add');
    spy.mockReturnValue(80000);

    // service.calculateTotalPrice(50000, 30000);
    expect(mathHelper.add).toHaveBeenCalledWith(50000, 30000);

    spy.mockRestore(); // penting: kembalikan fungsi asli setelah test
  });
});
```

### 5. Mock Provider dengan useClass / useFactory

```typescript
// Mock complete provider
class MockMailService {
  sendMail = jest.fn().mockResolvedValue({ success: true });
}

const module = await Test.createTestingModule({
  providers: [
    NotificationService,
    {
      provide: MailService,
      useClass: MockMailService,
    },
    {
      provide: 'CACHE_OPTIONS',
      useFactory: () => ({ ttl: 60 }),
    },
  ],
}).compile();
```

### 6. Testing Edge Cases

```typescript
describe('CoffeeService — Edge Cases', () => {
  it('harus handle find() mengembalikan array kosong', async () => {
    mockCoffeeRepository.find.mockResolvedValue([]);
    const result = await service.findAll();
    expect(result).toEqual([]);
  });

  it('harus handle create dengan price 0 (gratis)', async () => {
    const dto = { name: 'Kopi Gratis', price: 0 };
    mockCoffeeRepository.create.mockReturnValue(dto);
    mockCoffeeRepository.save.mockResolvedValue(dto);

    const result = await service.create(dto);
    expect(result.price).toBe(0);
  });

  it('harus handle nama coffee yang sangat panjang', async () => {
    const longName = 'A'.repeat(255);
    const dto = { name: longName, price: 10000 };
    mockCoffeeRepository.create.mockReturnValue(dto);
    mockCoffeeRepository.save.mockResolvedValue(dto);

    const result = await service.create(dto);
    expect(result.name).toHaveLength(255);
  });
});
```

---

## Analogi: Gedung Bertingkat

| Mocking | Analogi Gedung |
|---|---|
| **Real Repository** | **Kurir sungguhan** yang naik turun tangga |
| **Mock Repository** | **Kurir palsu (simulasi)** — kita suruh diam atau jawab sesuai skenario |
| `jest.fn()` | Membuat **aktor pura-pura** yang hanya melakukan apa yang kita perintahkan |
| `jest.spyOn()` | Memasang **kamera pengawas** pada kurir asli — kita lihat apa yang dia lakukan |
| `mockResolvedValue()` | Menyuruh aktor pura-pura: **"kalau ditanya, jawab ini"** |
| `toHaveBeenCalledWith()` | Memeriksa **apakah surat yang dikirim berisi alamat yang benar** |
| `clearAllMocks()` | **Reset semua aktor** sebelum adegan berikutnya |
| **Isolasi** | Menguji **satpam sendirian** di ruang tertutup, tanpa gangguan kurir atau resepsionis |

---

## Dipakai Untuk Apa

- Service dengan dependency database (TypeORM, Prisma, Mongoose)
- Service yang manggil API eksternal (HttpModule, Axios)
- Service dengan queue / message broker
- Notification / email service
- Business logic yang kompleks dengan banyak cabang

---

## Kesalahan Umum

1. **Mock tidak direset** — test sebelumnya mempengaruhi test berikutnya. Selalu `clearAllMocks()` / `resetAllMocks()` di `afterEach`.
2. **Over-mocking** — mock semua hal sampai test tidak menguji apa pun yang bermakna.
3. **Lupa `await` di async test** — Promise tidak selesai, assertion tidak jalan.
4. **Mock return type tidak sesuai** — mock return object, padahal method asli return Promise.
5. **Typo nama method mock** — typo `findOne` jadi `findone` → mock tidak dipakai.
6. **Menguji private method** — jangan; uji melalui public method yang memanggilnya.
7. **Mock dependency yang tidak dipakai** — bikin testing module error. Berikan mock yang sesuai.

---

## Soal Latihan

### Soal 1
Buat service `ProductService` dengan dependency `Repository<Product>` yang memiliki method:
- `findAll()` — return semua produk
- `findOne(id)` — return produk by id, throw `NotFoundException` jika tidak ada
- `updateStock(id, quantity)` — update stok, throw error jika stok negatif

Tulis unit test lengkap yang mencakup:
- Happy path findAll
- Happy path findOne
- Error path findOne (NotFoundException)
- Happy path updateStock
- Error path updateStock (stok negatif → throw)

<details>
<summary>Jawaban</summary>

```typescript
// src/product/product.service.ts
import { Injectable, NotFoundException, BadRequestException } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { Product } from './entities/product.entity';

@Injectable()
export class ProductService {
  constructor(
    @InjectRepository(Product)
    private readonly productRepository: Repository<Product>,
  ) {}

  async findAll(): Promise<Product[]> {
    return this.productRepository.find();
  }

  async findOne(id: number): Promise<Product> {
    const product = await this.productRepository.findOne({ where: { id } });
    if (!product) {
      throw new NotFoundException(`Produk id ${id} tidak ditemukan`);
    }
    return product;
  }

  async updateStock(id: number, quantity: number): Promise<Product> {
    const product = await this.findOne(id);
    const newStock = product.stock + quantity;
    if (newStock < 0) {
      throw new BadRequestException('Stok tidak boleh negatif');
    }
    product.stock = newStock;
    return this.productRepository.save(product);
  }
}

// src/product/product.service.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { ProductService } from './product.service';
import { getRepositoryToken } from '@nestjs/typeorm';
import { Product } from './entities/product.entity';
import { NotFoundException, BadRequestException } from '@nestjs/common';

describe('ProductService', () => {
  let service: ProductService;
  const mockRepo = {
    find: jest.fn(),
    findOne: jest.fn(),
    save: jest.fn(),
  };

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        ProductService,
        { provide: getRepositoryToken(Product), useValue: mockRepo },
      ],
    }).compile();

    service = module.get<ProductService>(ProductService);
  });

  afterEach(() => jest.clearAllMocks());

  it('findAll — harus return semua produk', async () => {
    const products = [{ id: 1, name: 'Product A', stock: 10 }];
    mockRepo.find.mockResolvedValue(products);

    const result = await service.findAll();
    expect(result).toEqual(products);
    expect(mockRepo.find).toHaveBeenCalledTimes(1);
  });

  it('findOne — harus return produk jika ditemukan', async () => {
    const product = { id: 1, name: 'Product A', stock: 10 };
    mockRepo.findOne.mockResolvedValue(product);

    const result = await service.findOne(1);
    expect(result).toEqual(product);
  });

  it('findOne — harus throw NotFound jika tidak ditemukan', async () => {
    mockRepo.findOne.mockResolvedValue(null);
    await expect(service.findOne(999)).rejects.toThrow(NotFoundException);
  });

  it('updateStock — harus update stok dengan benar', async () => {
    const product = { id: 1, name: 'Product A', stock: 10 };
    mockRepo.findOne.mockResolvedValue(product);
    mockRepo.save.mockImplementation((p) => Promise.resolve(p));

    const result = await service.updateStock(1, 5);
    expect(result.stock).toBe(15);
  });

  it('updateStock — harus throw jika stok negatif', async () => {
    const product = { id: 1, name: 'Product A', stock: 2 };
    mockRepo.findOne.mockResolvedValue(product);

    await expect(service.updateStock(1, -5)).rejects.toThrow(BadRequestException);
    expect(mockRepo.save).not.toHaveBeenCalled();
  });
});
```
</details>

### Soal 2
Jelaskan perbedaan `jest.fn()` dan `jest.spyOn()` beserta contoh kapan memakai masing-masing.

<details>
<summary>Jawaban</summary>

| | `jest.fn()` | `jest.spyOn()` |
|---|---|---|
| Fungsi | Membuat fungsi mock baru dari nol | Membungkus fungsi yang **sudah ada** untuk dimata-matai |
| Default behavior | `undefined` (tidak melakukan apa-apa) | Menjalankan implementasi asli (kecuali di-mock) |
| Use case | Mock dependency penuh (repository, service) | Ingin mengamati/meng-intercept method object yang sudah ada |
| Restore | Tidak perlu | Harus panggil `.mockRestore()` setelah test |

Contoh `jest.fn()`:
```typescript
const mockRepo = { find: jest.fn().mockResolvedValue([]) };
```

Contoh `jest.spyOn()`:
```typescript
const logger = new Logger();
const spy = jest.spyOn(logger, 'log');
logger.log('test');
expect(spy).toHaveBeenCalledWith('test');
spy.mockRestore();
```
</details>
