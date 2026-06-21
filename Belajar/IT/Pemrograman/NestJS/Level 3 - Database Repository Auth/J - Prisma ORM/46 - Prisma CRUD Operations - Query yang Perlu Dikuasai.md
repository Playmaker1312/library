# 46 - Prisma CRUD Operations - Query yang Perlu Dikuasai

## Penjelasan

Setelah PrismaService terintegrasi (Pertemuan 45), kita sekarang bisa melakukan operasi database. Prisma Client menyediakan API yang sangat intuitif untuk Create, Read, Update, Delete. Namun ada banyak varian query yang perlu dikuasai agar kita bisa menulis kode yang efisien dan sesuai kebutuhan.

Jika PrismaService adalah **panel listrik utama**, maka query-query ini adalah **saklar dan colokan di setiap ruangan** — alat yang kita gunakan setiap hari untuk berinteraksi dengan isi gedung.

## Fungsi

- **findMany**: Mengambil banyak data dengan filter, sorting, pagination
- **findUnique vs findFirst**: Mencari satu record (unique by ID/criteria)
- **create / createMany**: Membuat data baru
- **update / updateMany**: Mengupdate data
- **upsert**: Update jika ada, create jika tidak
- **delete / deleteMany**: Menghapus data
- **count / aggregate / groupBy**: Operasi agregasi

## Cara Pengimplementasian

Semua contoh menggunakan model `Post`:

### Create

```typescript
// create satu record
const post = await prisma.post.create({
  data: {
    title: 'Belajar Prisma',
    content: 'Isi konten...',
    authorId: 1,
  },
});

// createMany — batch insert
const posts = await prisma.post.createMany({
  data: [
    { title: 'Post 1', authorId: 1 },
    { title: 'Post 2', authorId: 1 },
    { title: 'Post 3', authorId: 2 },
  ],
  skipDuplicates: true, // skip record yang duplicate
});
```

### Read (Find)

```typescript
// findMany — dengan filter, sorting, pagination
const posts = await prisma.post.findMany({
  where: {
    published: true,
    authorId: 1,
    title: { contains: 'Prisma' },
  },
  orderBy: { createdAt: 'desc' },
  skip: 0,
  take: 10,
  select: {
    id: true,
    title: true,
    author: {
      select: { name: true },
    },
  },
  // include: { author: true }, // alternatif: ambil semua field + relasi
});

// findUnique — harus unique field (id, email, dll)
const post = await prisma.post.findUnique({
  where: { id: 1 },
  include: { author: true, comments: true },
});

// findFirst — bisa pakai field non-unique
const firstPost = await prisma.post.findFirst({
  where: { published: true },
  orderBy: { createdAt: 'desc' },
});
```

### Update

```typescript
// update satu record
const updated = await prisma.post.update({
  where: { id: 1 },
  data: {
    title: 'Judul Baru',
    published: true,
  },
});

// updateMany — multiple records (return count, bukan data)
const result = await prisma.post.updateMany({
  where: { authorId: 1 },
  data: { published: true },
});
// result: { count: 5 }
```

### Upsert

```typescript
const post = await prisma.post.upsert({
  where: { id: 1 },
  update: {
    title: 'Judul Diupdate',
    content: 'Konten baru',
  },
  create: {
    title: 'Judul Baru',
    content: 'Konten baru',
    authorId: 1,
  },
});
```

### Delete

```typescript
// delete satu record
await prisma.post.delete({
  where: { id: 1 },
});

// deleteMany — multiple records
const result = await prisma.post.deleteMany({
  where: {
    published: false,
    createdAt: { lt: new Date('2024-01-01') },
  },
});
// result: { count: 3 }
```

### Count / Aggregate / GroupBy

```typescript
// count
const totalPosts = await prisma.post.count();
const publishedPosts = await prisma.post.count({
  where: { published: true },
});

// aggregate
const stats = await prisma.post.aggregate({
  _count: { id: true },
  _avg: { id: true },
  _sum: { id: true },
  _min: { id: true },
  _max: { id: true },
});

// groupBy
const grouped = await prisma.post.groupBy({
  by: ['authorId'],
  _count: { id: true },
  _avg: { id: true },
  having: {
    id: { _count: { gte: 2 } },
  },
});
```

### Complete CRUD Service Example

```typescript
import { Injectable } from '@nestjs/common';
import { PrismaService } from '../prisma/prisma.service';
import { Prisma } from '@prisma/client';

@Injectable()
export class PostsService {
  constructor(private prisma: PrismaService) {}

  async create(data: Prisma.PostCreateInput) {
    return this.prisma.post.create({ data });
  }

  async findAll(params: {
    skip?: number;
    take?: number;
    where?: Prisma.PostWhereInput;
    orderBy?: Prisma.PostOrderByWithRelationInput;
  }) {
    const { skip, take, where, orderBy } = params;
    return this.prisma.post.findMany({ skip, take, where, orderBy });
  }

  async findOne(id: number) {
    return this.prisma.post.findUnique({ where: { id } });
  }

  async update(id: number, data: Prisma.PostUpdateInput) {
    return this.prisma.post.update({ where: { id }, data });
  }

  async remove(id: number) {
    return this.prisma.post.delete({ where: { id } });
  }

  async count(where?: Prisma.PostWhereInput) {
    return this.prisma.post.count({ where });
  }
}
```

## Analogi

**Membangun Gedung Bertingkat**

- **create** = **membangun ruangan baru** sesuai blueprint
- **findMany** = **jalan-jalan lihat semua ruangan** dengan filter tertentu (lantai 3 saja, yang sudah dicat)
- **findUnique** = **cari kamar nomor 201** langsung, tanpa keliling
- **findFirst** = **cari ruangan pertama** yang memenuhi syarat (misal: toilet terdekat)
- **update** = **renovasi ruangan** — ganti cat, perbaiki pintu
- **updateMany** = **renovasi massal** semua ruangan di satu lantai
- **upsert** = **"kalau ruangannya ada, renovasi. Kalau nggak ada, bangun baru"**
- **delete** = **bongkar satu ruangan**
- **deleteMany** = **bongkar semua ruangan** yang memenuhi kriteria
- **count** = **hitung jumlah ruangan** di gedung
- **aggregate** = **hitung total luas, rata-rata luas, dll**
- **groupBy** = **kelompokkan ruangan per lantai, hitung jumlah per lantai**

## Dipakai untuk Apa

- Semua operasi database CRUD di aplikasi
- Report dan statistik (count, aggregate, groupBy)
- Batch operations untuk data processing
- Idempotent create/update dengan upsert

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| `findUnique` pakai field non-unique (error) | Ganti ke `findFirst` |
| Lupa `include` relasi sehingga data relasi undefined | Tambah `include: { author: true }` atau pakai `select` |
| `updateMany` dan `deleteMany` return count, bukan data | Jangan expecting return data |
| `createMany` tidak return created records | Gunakan `create` dalam `$transaction` jika perlu return |
| N+1 problem — panggil query dalam loop | Gunakan `include` atau `select` untuk eager loading |

## Soal Latihan

Implementasikan semua operasi CRUD untuk Post di service berikut:

1. `createPost` — membuat post baru
2. `getAllPosts` — pagination (skip/take), filter by published
3. `getPostById` — include author dan comments
4. `updatePost` — update title/content/published
5. `deletePost` — delete by id
6. `getPostStats` — count total, count published, group by author

### Jawaban

```typescript
import { Injectable } from '@nestjs/common';
import { PrismaService } from '../prisma/prisma.service';

@Injectable()
export class PostsService {
  constructor(private prisma: PrismaService) {}

  async createPost(data: { title: string; content?: string; authorId: number }) {
    return this.prisma.post.create({ data });
  }

  async getAllPosts(skip = 0, take = 10, published?: boolean) {
    return this.prisma.post.findMany({
      skip,
      take,
      where: published !== undefined ? { published } : {},
      orderBy: { createdAt: 'desc' },
    });
  }

  async getPostById(id: number) {
    return this.prisma.post.findUnique({
      where: { id },
      include: {
        author: { select: { id: true, name: true, email: true } },
        comments: { include: { author: { select: { name: true } } } },
      },
    });
  }

  async updatePost(id: number, data: { title?: string; content?: string; published?: boolean }) {
    return this.prisma.post.update({ where: { id }, data });
  }

  async deletePost(id: number) {
    return this.prisma.post.delete({ where: { id } });
  }

  async getPostStats() {
    const total = await this.prisma.post.count();
    const published = await this.prisma.post.count({ where: { published: true } });
    const perAuthor = await this.prisma.post.groupBy({
      by: ['authorId'],
      _count: { id: true },
    });

    return { total, published, perAuthor };
  }
}
```
