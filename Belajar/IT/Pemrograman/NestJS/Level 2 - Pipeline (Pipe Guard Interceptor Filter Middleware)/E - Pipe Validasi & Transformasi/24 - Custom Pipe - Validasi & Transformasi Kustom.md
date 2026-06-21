# Custom Pipe — Validasi & Transformasi Kustom

## Penjelasan (Nyambung dari materi sebelumnya)

Built-in pipes (`ParseIntPipe`, `ValidationPipe`, dll) sudah mencakup kebutuhan umum. Tapi kadang kita butuh aturan **spesifik domain** yang tidak disediakan NestJS.

Misal: parsing parameter sort seperti `name:asc,price:desc` menjadi objek terstruktur. Atau validasi business rule: "pastikan stock mencukupi".

Custom pipe memberi kita kendali penuh atas transformasi dan validasi input.

---

## Fungsi

- Transformasi input kompleks yang tidak bisa ditangani `@Transform` sederhana
- Validasi business rule yang butuh akses ke service/database
- Parsing format khusus (sort string, filter expression, dll)
- Validasi yang bisa dipakai ulang di banyak endpoint
- Memisahkan logic parsing dari controller

---

## Cara Implementasi & Code

### Interface PipeTransform

```typescript
import { PipeTransform, Injectable, ArgumentMetadata, BadRequestException } from '@nestjs/common';

@Injectable()
export class MyPipe implements PipeTransform<TInput, TOutput> {
  transform(value: TInput, metadata: ArgumentMetadata): TOutput {
    // value: nilai mentah dari request
    // metadata: { type: 'body' | 'query' | 'param' | 'custom', metatype, data }
    return transformedValue;
  }
}
```

### 1. ParseSortPipe — Parsing Sort String

```typescript
// src/common/pipes/parse-sort.pipe.ts
import { PipeTransform, Injectable, BadRequestException } from '@nestjs/common';

interface SortField {
  field: string;
  direction: 'asc' | 'desc';
}

@Injectable()
export class ParseSortPipe implements PipeTransform<string, SortField[]> {
  private readonly allowedFields: string[];

  constructor(allowedFields?: string[]) {
    this.allowedFields = allowedFields ?? [];
  }

  transform(value: string): SortField[] {
    if (!value) return [];

    const parts = value.split(',');
    const result: SortField[] = [];

    for (const part of parts) {
      const [field, dir] = part.split(':');
      
      if (!field || !dir) {
        throw new BadRequestException(
          `Format sort tidak valid: "${part}". Gunakan format field:direction`,
        );
      }

      if (!['asc', 'desc'].includes(dir.toLowerCase())) {
        throw new BadRequestException(
          `Direction "${dir}" tidak valid. Gunakan "asc" atau "desc"`,
        );
      }

      if (this.allowedFields.length > 0 && !this.allowedFields.includes(field)) {
        throw new BadRequestException(
          `Field "${field}" tidak diizinkan untuk sorting. Field yang diizinkan: ${this.allowedFields.join(', ')}`,
        );
      }

      result.push({
        field,
        direction: dir.toLowerCase() as 'asc' | 'desc',
      });
    }

    return result;
  }
}
```

**Penggunaan di Controller:**

```typescript
@Get()
findAll(
  @Query('sort', new ParseSortPipe(['name', 'price', 'createdAt'])) sort: SortField[],
) {
  // GET /products?sort=name:asc,price:desc
  // → [{ field: 'name', direction: 'asc' }, { field: 'price', direction: 'desc' }]
  return this.productService.findAll({ sort });
}
```

### 2. ParseFilterPipe — Parsing Filter Expression

```typescript
// src/common/pipes/parse-filter.pipe.ts
import { PipeTransform, Injectable, BadRequestException } from '@nestjs/common';

interface FilterCondition {
  field: string;
  operator: 'eq' | 'neq' | 'gt' | 'gte' | 'lt' | 'lte' | 'in' | 'contains';
  value: string | number | string[];
}

const OPERATORS = ['eq', 'neq', 'gt', 'gte', 'lt', 'lte', 'in', 'contains'];
const PATTERN = /^(\w+):(\w+):(.+)$/;

@Injectable()
export class ParseFilterPipe implements PipeTransform<string, FilterCondition[]> {
  transform(value: string): FilterCondition[] {
    if (!value) return [];

    const parts = value.split(',');
    const result: FilterCondition[] = [];

    for (const part of parts) {
      const match = part.match(PATTERN);
      
      if (!match) {
        throw new BadRequestException(
          `Format filter tidak valid: "${part}". Gunakan field:operator:value`,
        );
      }

      const [, field, operator, rawValue] = match;

      if (!OPERATORS.includes(operator)) {
        throw new BadRequestException(
          `Operator "${operator}" tidak valid. Gunakan: ${OPERATORS.join(', ')}`,
        );
      }

      let parsedValue: string | number | string[] = rawValue;

      if (operator === 'in') {
        parsedValue = rawValue.split('|');
      } else if (!isNaN(Number(rawValue))) {
        parsedValue = Number(rawValue);
      }

      result.push({ field, operator: operator as FilterCondition['operator'], value: parsedValue });
    }

    return result;
  }
}
```

**Penggunaan:**

```typescript
@Get()
findAll(
  @Query('filter', ParseFilterPipe) filters: FilterCondition[],
) {
  // GET /products?filter=price:gt:1000,category:eq:electronics
  return this.productService.findAll({ filters });
}
```

### 3. Business Rule Validation — Cek Duplikat

```typescript
// src/product/pipes/unique-product-name.pipe.ts
import { PipeTransform, Injectable, BadRequestException } from '@nestjs/common';
import { ProductService } from '../product.service';

@Injectable()
export class UniqueProductNamePipe implements PipeTransform {
  constructor(private readonly productService: ProductService) {}

  async transform(value: any) {
    // Cek apakah nama produk sudah ada di DB
    const existing = await this.productService.findByName(value.name);
    if (existing) {
      throw new BadRequestException(`Produk dengan nama "${value.name}" sudah ada`);
    }
    return value; // Lanjutkan jika valid
  }
}
```

**Penggunaan:**

```typescript
@Post()
async create(
  @Body(UniqueProductNamePipe) dto: CreateProductDto,
) {
  // Jika nama sudah ada, pipe akan throw BadRequestException
  return this.productService.create(dto);
}
```

### 4. Custom Pipe — Transformasi Array

```typescript
// src/common/pipes/parse-tags.pipe.ts
import { PipeTransform, Injectable } from '@nestjs/common';

@Injectable()
export class ParseTagsPipe implements PipeTransform<string, string[]> {
  transform(value: string): string[] {
    if (!value) return [];
    return value
      .split(',')
      .map(tag => tag.trim().toLowerCase())
      .filter(tag => tag.length > 0);
  }
}
```

### 5. Pipe dengan Dependency Injection

Custom pipe bisa di-inject dengan module lain:

```typescript
// src/common/pipes/validate-warehouse.pipe.ts
import { PipeTransform, Injectable, BadRequestException } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { Warehouse } from '../../warehouse/entities/warehouse.entity';

@Injectable()
export class ValidateWarehousePipe implements PipeTransform {
  constructor(
    @InjectRepository(Warehouse)
    private readonly warehouseRepo: Repository<Warehouse>,
  ) {}

  async transform(value: { warehouseId: string }) {
    const warehouse = await this.warehouseRepo.findOne({
      where: { id: value.warehouseId },
    });
    
    if (!warehouse) {
      throw new BadRequestException(`Warehouse dengan ID "${value.warehouseId}" tidak ditemukan`);
    }
    
    if (!warehouse.isActive) {
      throw new BadRequestException(`Warehouse "${warehouse.name}" sedang tidak aktif`);
    }

    return value;
  }
}
```

---

## Analogi — Membangun Gedung

| Konsep | Analogi Gedung |
|--------|----------------|
| **Built-in Pipe** | Alat standar: palu, gergaji, obeng — cukup untuk 80% pekerjaan |
| **Custom Pipe** | **Alat khusus buatan sendiri**: cetakan bata khusus, alat ukir custom, bor sudut khusus |
| **ParseSortPipe** | Petugas yang mengubah "urutkan: nama naik, harga turun" → daftar instruksi untuk mandor |
| **ParseFilterPipe** | Petugas: "cari kamar luas > 20m², tipe = apartemen" → query terstruktur |
| **Business Rule Pipe** | Konsultan yang ngecek: "Apakah tanah ini sudah dipakai proyek lain?" sebelum izin bangun |
| **Pipe dengan DI** | Ahli struktur yang butuh akses ke database material untuk validasi |

---

## Dipakai Untuk Apa

- **Parsing format kompleks** — sort string, filter expression, nested query params
- **Validasi business rule** — cek duplikat, cek stok, cek status
- **Reusable validation** — pipe yang sama dipakai di banyak endpoint
- **Integrasi dengan database** — validasi eksistensi data sebelum diproses
- **Transformasi khusus** — konversi format yang tidak ditangani class-transformer

---

## Kesalahan Umum

1. **Custom pipe tidak di-inject di module** — error `Nest can't resolve dependencies` jika pipe punya dependency
2. **Pipe tidak me-return value** — request hang karena pipe tidak mengembalikan apa-apa
3. **Pipe async tidak di-await** — NestJS otomatis await, tapi pastikan return Promise
4. **BadRequestException tidak dipakai** — gunakan `throw new BadRequestException()`, jangan return error
5. **Pipe terlalu kompleks** — pisahkan logic ke service, pipe hanya sebagai "penerjemah"
6. **Pipe dipasang di method tapi tidak global** — ingat beda `@UsePipes(pipe)` vs `app.useGlobalPipes(pipe)`
7. **Lupa handle nilai null/undefined** — pipe bisa menerima `undefined` dari query param yang tidak dikirim

---

## Soal Latihan & Jawaban

### Soal

1. Buat `ParseSortPipe` seperti contoh di atas
2. Terapkan di controller dengan endpoint `GET /products`
3. Daftar field yang diizinkan: `name`, `price`, `createdAt`, `stock`
4. Format: `?sort=name:asc,price:desc`
5. Jika format salah, throw `BadRequestException`

### Jawaban

**ParseSortPipe**

```typescript
// src/common/pipes/parse-sort.pipe.ts
import { PipeTransform, Injectable, BadRequestException } from '@nestjs/common';

interface SortField {
  field: string;
  direction: 'asc' | 'desc';
}

@Injectable()
export class ParseSortPipe implements PipeTransform<string, SortField[]> {
  private readonly allowedFields: string[];

  constructor(allowedFields?: string[]) {
    this.allowedFields = allowedFields ?? [];
  }

  transform(value: string): SortField[] {
    if (!value) return [];

    const parts = value.split(',');
    const result: SortField[] = [];

    for (const part of parts) {
      if (!part.includes(':')) {
        throw new BadRequestException(
          `Format sort tidak valid: "${part}". Gunakan field:direction`,
        );
      }

      const [field, dir] = part.split(':', 2);

      if (!['asc', 'desc'].includes(dir?.toLowerCase())) {
        throw new BadRequestException(
          `Direction "${dir}" tidak valid. Gunakan "asc" atau "desc"`,
        );
      }

      if (this.allowedFields.length > 0 && !this.allowedFields.includes(field)) {
        throw new BadRequestException(
          `Field "${field}" tidak diizinkan. Diizinkan: ${this.allowedFields.join(', ')}`,
        );
      }

      result.push({
        field,
        direction: dir.toLowerCase() as 'asc' | 'desc',
      });
    }

    return result;
  }
}
```

**Controller**

```typescript
// src/product/product.controller.ts
import { Controller, Get, Query } from '@nestjs/common';
import { ProductService } from './product.service';
import { ParseSortPipe } from '../common/pipes/parse-sort.pipe';

@Controller('products')
export class ProductController {
  constructor(private readonly productService: ProductService) {}

  @Get()
  findAll(
    @Query('sort', new ParseSortPipe(['name', 'price', 'createdAt', 'stock']))
    sort: { field: string; direction: 'asc' | 'desc' }[],
  ) {
    // GET /products?sort=name:asc,price:desc
    // → [{ field: 'name', direction: 'asc' }, { field: 'price', direction: 'desc' }]
    return this.productService.findAll({ sort });
  }
}
```
