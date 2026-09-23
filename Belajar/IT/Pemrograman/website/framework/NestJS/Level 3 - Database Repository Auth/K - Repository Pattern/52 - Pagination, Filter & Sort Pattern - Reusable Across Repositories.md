# 52 - Pagination, Filter & Sort Pattern - Reusable Across Repositories

## Penjelasan

Setelah memiliki BaseRepository (Pertemuan 50), kita perlu pattern untuk pagination, filter, dan sort yang reusable. Setiap endpoint yang mengembalikan daftar data biasanya butuh pagination (page/limit), filter (where), dan sorting (orderBy). Tanpa pattern, kita akan menulis kode pagination berulang-ulang di setiap service.

Jika BaseRepository adalah **pabrik keran**, maka Pagination Pattern adalah **sistem rak gudang** — bukan mengeluarkan semua keran dari gudang (akan kewalahan), tapi mengeluarkan 10 keran per halaman dengan urutan tertentu.

## Fungsi

- **PaginationDto (page/limit)**: DTO standar untuk request pagination
- **PaginatedResult<T> (data/total/page/limit/totalPages)**: Response standar dengan metadata
- **Generic paginate()**: Fungsi reusable yang bisa dipakai semua repository
- **Filter & Sort**: Parameter dinamis untuk filtering dan sorting

## Cara Pengimplementasian

### 1. Pagination DTO

```typescript
// src/common/dto/pagination.dto.ts
import { IsOptional, IsInt, Min, Max } from 'class-validator';
import { Type } from 'class-transformer';

export class PaginationDto {
  @IsOptional()
  @Type(() => Number)
  @IsInt()
  @Min(1)
  page?: number = 1;

  @IsOptional()
  @Type(() => Number)
  @IsInt()
  @Min(1)
  @Max(100)
  limit?: number = 10;
}
```

### 2. PaginatedResult Type

```typescript
// src/common/interfaces/paginated-result.interface.ts
export interface PaginatedResult<T> {
  data: T[];
  meta: {
    total: number;
    page: number;
    limit: number;
    totalPages: number;
    hasNextPage: boolean;
    hasPreviousPage: boolean;
  };
}
```

### 3. Pagination Service / Helper

```typescript
// src/common/helpers/pagination.helper.ts
import { PaginatedResult } from '../interfaces/paginated-result.interface';

export function paginate<T>(
  data: T[],
  total: number,
  page: number,
  limit: number,
): PaginatedResult<T> {
  const totalPages = Math.ceil(total / limit);

  return {
    data,
    meta: {
      total,
      page,
      limit,
      totalPages,
      hasNextPage: page < totalPages,
      hasPreviousPage: page > 1,
    },
  };
}
```

### 4. Query Parameter DTO untuk Filter & Sort

```typescript
// src/common/dto/query-params.dto.ts
import { IsOptional, IsString, IsEnum } from 'class-validator';

export enum SortOrder {
  ASC = 'asc',
  DESC = 'desc',
}

export class SortDto {
  @IsOptional()
  @IsString()
  sortBy?: string;

  @IsOptional()
  @IsEnum(SortOrder)
  sortOrder?: SortOrder = SortOrder.DESC;
}

// Contoh konkret untuk Post
export class PostQueryDto extends PaginationDto {
  @IsOptional()
  @IsString()
  search?: string;

  @IsOptional()
  @IsString()
  status?: 'published' | 'draft';

  @IsOptional()
  @Type(() => Number)
  @IsInt()
  authorId?: number;

  @IsOptional()
  @IsString()
  sortBy?: string = 'createdAt';

  @IsOptional()
  @IsEnum(SortOrder)
  sortOrder?: SortOrder = SortOrder.DESC;
}
```

### 5. Generic Pagination di BaseRepository

```typescript
// src/common/repositories/base.repository.ts (extended)
import { PrismaService } from '../../prisma/prisma.service';
import { PaginatedResult } from '../interfaces/paginated-result.interface';
import { paginate } from '../helpers/pagination.helper';

export class BaseRepository<T, CreateDto, UpdateDto> {
  constructor(
    protected prisma: PrismaService,
    private readonly model: string,
  ) {}

  private get delegate(): any {
    return (this.prisma as any)[this.model];
  }

  async paginate(params: {
    page?: number;
    limit?: number;
    where?: any;
    orderBy?: any;
    include?: any;
    select?: any;
  }): Promise<PaginatedResult<T>> {
    const { page = 1, limit = 10, where, orderBy, include, select } = params;
    const skip = (page - 1) * limit;

    const [data, total] = await Promise.all([
      this.delegate.findMany({
        skip,
        take: limit,
        where,
        orderBy,
        include,
        select,
      }),
      this.delegate.count({ where }),
    ]);

    return paginate<T>(data, total, page, limit);
  }

  // Method dasar tetap ada
  async findAll(skip = 0, take = 10): Promise<T[]> {
    return this.delegate.findMany({ skip, take });
  }

  // ... method lainnya
}
```

### 6. Implementasi di Repository Spesifik

```typescript
// src/posts/repositories/post.repository.ts
import { Injectable } from '@nestjs/common';
import { Post, Prisma } from '@prisma/client';
import { BaseRepository } from '../../common/repositories/base.repository';
import { PrismaService } from '../../prisma/prisma.service';
import { PaginatedResult } from '../../common/interfaces/paginated-result.interface';

@Injectable()
export class PostRepository extends BaseRepository<
  Post,
  Prisma.PostCreateInput,
  Prisma.PostUpdateInput
> {
  constructor(prisma: PrismaService) {
    super(prisma, 'post');
  }

  async findPublishedPaginated(
    page: number,
    limit: number,
    search?: string,
  ): Promise<PaginatedResult<Post>> {
    const where: Prisma.PostWhereInput = {
      published: true,
      ...(search && {
        OR: [
          { title: { contains: search, mode: 'insensitive' } },
          { content: { contains: search, mode: 'insensitive' } },
        ],
      }),
    };

    return this.paginate({
      page,
      limit,
      where,
      orderBy: { createdAt: 'desc' },
      include: {
        author: { select: { id: true, name: true } },
      },
    });
  }
}
```

### 7. Controller

```typescript
// src/posts/posts.controller.ts
import { Controller, Get, Query } from '@nestjs/common';
import { PostsService } from './posts.service';
import { PostQueryDto } from './dto/post-query.dto';

@Controller('posts')
export class PostsController {
  constructor(private readonly postsService: PostsService) {}

  @Get()
  async findAll(@Query() query: PostQueryDto) {
    return this.postsService.findAll(query);
  }
}
```

### 8. Service

```typescript
// src/posts/posts.service.ts
import { Injectable } from '@nestjs/common';
import { PostRepository } from './repositories/post.repository';
import { PostQueryDto } from './dto/post-query.dto';

@Injectable()
export class PostsService {
  constructor(private readonly postRepo: PostRepository) {}

  async findAll(query: PostQueryDto) {
    const { page, limit, search, status, authorId, sortBy, sortOrder } = query;

    const where: any = {};

    if (status) where.published = status === 'published';
    if (authorId) where.authorId = authorId;
    if (search) {
      where.OR = [
        { title: { contains: search, mode: 'insensitive' } },
        { content: { contains: search, mode: 'insensitive' } },
      ];
    }

    return this.postRepo.paginate({
      page,
      limit,
      where,
      orderBy: { [sortBy || 'createdAt']: sortOrder || 'desc' },
    });
  }
}
```

## Analogi

**Membangun Gedung Bertingkat**

- **PaginationDto** = **papan "Lantai 1-10: Kamar 101-110"** — tamu tidak perlu naik ke lantai 20
- **PaginatedResult** = **daftar kamar per halaman** lengkap dengan info "halaman 1 dari 10"
- **page/limit** = **"tunjukkan kamar 1-10"** bukan semua 1000 kamar
- **totalPages** = **"ada 100 lantai"** — pengunjung tahu seberapa besar gedung
- **hasNextPage** = **"ada tangga ke lantai atas"** atau "ini lantai terakhir"
- **Filter** = **"tunjukkan hanya kamar yang sudah dicat putih"**
- **Sort** = **"urutkan kamar dari yang terbaru renovasinya"**
- **Generic paginate()** = **sistem navigasi standar** yang sama di setiap lantai

## Dipakai untuk Apa

- Semua endpoint GET yang mengembalikan daftar data (users, posts, products, dll)
- Admin panel dengan daftar data yang bisa di-filter dan di-sort
- API publik dengan resource yang banyak (ribuan+ records)
- Mobile apps yang butuh lazy loading / infinite scroll

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Tidak membatasi `limit` maksimum | Beri `@Max(100)` di DTO |
| Lupa handle `skip` = `(page - 1) * limit` | Hitung skip dengan benar |
| Filter case-sensitive di PostgreSQL | Pakai `mode: 'insensitive'` |
| Sorting by field yang tidak ada di schema | Validasi field sorting yang diizinkan (whitelist) |
| Tidak include totalPages / hasNextPage | Client susah implementasi infinite scroll |
| `Promise.all` untuk data + total tidak reuse query | Masih optimal karena count dan findMany berbeda query |

## Soal Latihan

1. Buat `PaginationDto` dengan field page dan limit (validasi)
2. Buat interface `PaginatedResult<T>` dengan meta lengkap
3. Buat fungsi `paginate()` helper
4. Tambahkan method `paginate()` di BaseRepository
5. Implementasikan di PostRepository dengan filter search + sort

### Jawaban

**pagination.dto.ts:**
```typescript
import { IsOptional, IsInt, Min, Max } from 'class-validator';
import { Type } from 'class-transformer';

export class PaginationDto {
  @IsOptional()
  @Type(() => Number)
  @IsInt()
  @Min(1)
  page?: number = 1;

  @IsOptional()
  @Type(() => Number)
  @IsInt()
  @Min(1)
  @Max(100)
  limit?: number = 10;
}
```

**paginated-result.interface.ts:**
```typescript
export interface PaginatedResult<T> {
  data: T[];
  meta: {
    total: number;
    page: number;
    limit: number;
    totalPages: number;
    hasNextPage: boolean;
    hasPreviousPage: boolean;
  };
}
```

**pagination.helper.ts:**
```typescript
import { PaginatedResult } from '../interfaces/paginated-result.interface';

export function paginate<T>(
  data: T[],
  total: number,
  page: number,
  limit: number,
): PaginatedResult<T> {
  const totalPages = Math.ceil(total / limit);
  return {
    data,
    meta: {
      total,
      page,
      limit,
      totalPages,
      hasNextPage: page < totalPages,
      hasPreviousPage: page > 1,
    },
  };
}
```

**BaseRepository — tambahan method:**
```typescript
async paginate(params: {
  page?: number;
  limit?: number;
  where?: any;
  orderBy?: any;
  include?: any;
}): Promise<PaginatedResult<T>> {
  const { page = 1, limit = 10, where, orderBy, include } = params;
  const skip = (page - 1) * limit;

  const [data, total] = await Promise.all([
    this.delegate.findMany({ skip, take: limit, where, orderBy, include }),
    this.delegate.count({ where }),
  ]);

  return paginate<T>(data, total, page, limit);
}
```

**PostQueryDto:**
```typescript
import { IsOptional, IsString, IsEnum, IsBoolean } from 'class-validator';
import { Type } from 'class-transformer';

enum SortOrder {
  ASC = 'asc',
  DESC = 'desc',
}

export class PostQueryDto extends PaginationDto {
  @IsOptional()
  @IsString()
  search?: string;

  @IsOptional()
  @IsString()
  sortBy?: string = 'createdAt';

  @IsOptional()
  @IsEnum(SortOrder)
  sortOrder?: SortOrder = SortOrder.DESC;
}
```

**Penggunaan di Controller:**
```typescript
@Get()
async findAll(@Query() query: PostQueryDto) {
  const where: any = {};
  if (query.search) {
    where.OR = [
      { title: { contains: query.search, mode: 'insensitive' } },
      { content: { contains: query.search, mode: 'insensitive' } },
    ];
  }

  return this.postRepo.paginate({
    page: query.page,
    limit: query.limit,
    where,
    orderBy: { [query.sortBy]: query.sortOrder },
  });
}
```
