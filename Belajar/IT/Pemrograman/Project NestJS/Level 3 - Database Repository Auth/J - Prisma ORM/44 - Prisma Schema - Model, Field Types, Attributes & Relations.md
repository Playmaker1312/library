# 44 - Prisma Schema - Model, Field Types, Attributes & Relations

## Penjelasan

Setelah kita berhasil melakukan inisialisasi Prisma dan migration pertama (Pertemuan 43), sekarang kita perlu memahami detail tentang bagaimana mendesain schema yang baik. Schema Prisma adalah sumber kebenaran (source of truth) untuk struktur database kita. Pemahaman tentang field types, attributes, dan relasi akan menentukan seberapa baik aplikasi kita dapat berkembang.

Jika di pertemuan sebelumnya kita baru **membuat blueprint dasar**, sekarang kita belajar **notasi-notasi detail dalam blueprint** — jenis material (field types), simbol-simbol khusus (attributes), dan bagaimana setiap ruangan terhubung (relations).

## Fungsi

- **Field Types**: Menentukan tipe data yang bisa disimpan (String, Int, Float, dll)
- **Attributes (@id, @default, @unique, dll)**: Memberi properti khusus pada field
- **@map / @@map**: Mapping nama field/tabel ke database
- **@updatedAt**: Otomatis update timestamp
- **UUID / CUID / autoincrement**: Strategi generate ID
- **Soft Delete**: Teknik "menghapus" data tanpa benar-benar menghapus
- **Relasi 1-1, 1-N, N-N**: Menghubungkan antar model

## Cara Pengimplementasian

### Field Types

```prisma
model Example {
  // String types
  name     String    // VARCHAR, wajib diisi
  bio      String?   // VARCHAR, nullable (opsional)

  // Numeric
  age      Int       // INTEGER
  score    Float     // FLOAT/REAL
  decimal  Decimal   // @db.Decimal(10,2) untuk presisi tinggi

  // Boolean
  active   Boolean   // BOOLEAN

  // DateTime
  bornAt   DateTime  // TIMESTAMP/TIMESTAMPTZ

  // JSON
  metadata Json      // JSON/JSONB

  // Bytes
  avatar   Bytes     // BYTEA untuk binary data
}
```

### Attributes

```prisma
model User {
  id        String   @id @default(uuid())         // UUID strategy
  // alternatif: @default(cuid()), @default(autoincrement()) untuk Int

  email     String   @unique                       // Tidak boleh duplikat
  username  String   @unique                       // Unique constraint

  role      String   @default("USER")              // Default value
  points    Int      @default(0)

  createdAt DateTime @default(now())               // Otomatis waktu dibuat
  updatedAt DateTime @updatedAt                    // Otomatis waktu diupdate

  deletedAt DateTime?                              // Soft delete field

  @map("full_name")  // mapping nama field di DB
  fullName String
}

// Mapping nama tabel di DB
@@map("users")
```

### Soft Delete Pattern

```prisma
model Product {
  id        Int       @id @default(autoincrement())
  name      String
  deletedAt DateTime?

  // Query default harus filter deletedAt is null
}
```

### Relasi 1-1

```prisma
model User {
  id      Int     @id @default(autoincrement())
  profile Profile?
}

model Profile {
  id     Int  @id @default(autoincrement())
  userId Int  @unique
  user   User @relation(fields: [userId], references: [id])
  bio    String
}
```

### Relasi 1-N

```prisma
model User {
  id    Int    @id @default(autoincrement())
  posts Post[]
}

model Post {
  id       Int  @id @default(autoincrement())
  authorId Int
  author   User @relation(fields: [authorId], references: [id])
}
```

### Relasi N-N (Many-to-Many)

```prisma
model Post {
  id       Int          @id @default(autoincrement())
  title    String
  tags     TagOnPost[]
}

model Tag {
  id    Int          @id @default(autoincrement())
  name  String       @unique
  posts TagOnPost[]
}

model TagOnPost {
  postId Int
  post   Post @relation(fields: [postId], references: [id])
  tagId  Int
  tag    Tag  @relation(fields: [tagId], references: [id])

  @@id([postId, tagId])
}
```

Alternatif dengan implicit many-to-many:

```prisma
model Post {
  id   Int   @id @default(autoincrement())
  tags Tag[]
}

model Tag {
  id    Int    @id @default(autoincrement())
  name  String @unique
  posts Post[]
}
```

### Schema Blog Lengkap

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String
  posts     Post[]
  comments  Comment[]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@map("users")
}

model Category {
  id     Int    @id @default(autoincrement())
  name   String @unique
  posts  Post[]
}

model Post {
  id         Int       @id @default(autoincrement())
  title      String
  content    String?
  published  Boolean   @default(false)
  authorId   Int
  author     User      @relation(fields: [authorId], references: [id])
  categoryId Int?
  category   Category? @relation(fields: [categoryId], references: [id])
  comments   Comment[]
  tags       TagOnPost[]
  createdAt  DateTime  @default(now())
  updatedAt  DateTime  @updatedAt

  @@map("posts")
}

model Comment {
  id       Int      @id @default(autoincrement())
  text     String
  postId   Int
  post     Post     @relation(fields: [postId], references: [id])
  authorId Int
  author   User     @relation(fields: [authorId], references: [id])
  createdAt DateTime @default(now())

  @@map("comments")
}

model Tag {
  id    Int          @id @default(autoincrement())
  name  String       @unique
  posts TagOnPost[]

  @@map("tags")
}

model TagOnPost {
  postId Int
  post   Post @relation(fields: [postId], references: [id])
  tagId  Int
  tag    Tag  @relation(fields: [tagId], references: [id])

  @@id([postId, tagId])
  @@map("tag_on_posts")
}
```

## Analogi

**Membangun Gedung Bertingkat**

- **Field Types** = **jenis material** (String = kayu, Int = bata, DateTime = beton, Json = kaca)
- **@id** = **pintu utama** setiap ruangan, identifier unik
- **@unique** = **nomor KTP** — tidak boleh ada yang sama
- **@default** = **settingan standar** pabrik (lampu menyala saat pertama dipasang)
- **@updatedAt** = **sensor otomatis** yang mencatat kapan terakhir ada perbaikan
- **Relasi 1-1** = **satu kamar mandi untuk satu kamar tidur utama**
- **Relasi 1-N** = **satu lantai punya banyak ruangan**
- **Relasi N-N** = **hubungan antara ruangan dan AC** — satu ruangan bisa punya banyak AC, satu AC bisa dipasang di banyak ruangan
- **Soft Delete** = **menutup ruangan dengan papan "RENOVASI"** daripada membongkar fisiknya

## Dipakai untuk Apa

- Mendesain struktur database sebelum coding
- Memastikan konsistensi data melalui constraint (@unique, @default)
- Mengoptimalkan query dengan relasi yang tepat
- Implementasi fitur seperti soft delete tanpa data loss

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Lupa menambahkan `@unique` di field yang seharusnya unik | Tambahkan `@unique` pada level schema, lalu migrate |
| Relasi N-N tanpa implicit atau explicit join table | Gunakan implicit `Post[]` dan `Tag[]` di kedua model |
| Menggunakan `Int` autoincrement untuk ID di distributed system | Gunakan `String @id @default(uuid())` atau `cuid()` |
| Tidak pakai `@@map` sehingga nama tabel tidak konsisten | Tambahkan `@@map("snake_case")` untuk konvensi database |
| Soft delete tapi lupa filter di query | Buat middleware atau reusable function untuk filter `deletedAt: null` |

## Soal Latihan

Desain schema untuk Blog API dengan fitur:
1. User bisa membuat banyak Post
2. Post bisa memiliki banyak Category (hanya 1 category per post)
3. Post bisa memiliki banyak Tag (N-N)
4. User bisa comment di Post
5. Setiap model memiliki createdAt dan updatedAt
6. Implementasikan soft delete di Post

### Jawaban

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        Int       @id @default(autoincrement())
  email     String    @unique
  name      String
  posts     Post[]
  comments  Comment[]
  createdAt DateTime  @default(now())
  updatedAt DateTime  @updatedAt

  @@map("users")
}

model Category {
  id   Int    @id @default(autoincrement())
  name String @unique
  posts Post[]

  @@map("categories")
}

model Post {
  id         Int       @id @default(autoincrement())
  title      String
  content    String?
  published  Boolean   @default(false)
  deletedAt  DateTime?
  authorId   Int
  author     User      @relation(fields: [authorId], references: [id])
  categoryId Int?
  category   Category? @relation(fields: [categoryId], references: [id])
  comments   Comment[]
  tags       TagOnPost[]
  createdAt  DateTime  @default(now())
  updatedAt  DateTime  @updatedAt

  @@map("posts")
}

model Comment {
  id        Int      @id @default(autoincrement())
  text      String
  postId    Int
  post      Post     @relation(fields: [postId], references: [id])
  authorId  Int
  author    User     @relation(fields: [authorId], references: [id])
  createdAt DateTime @default(now())

  @@map("comments")
}

model Tag {
  id    Int          @id @default(autoincrement())
  name  String       @unique
  posts TagOnPost[]

  @@map("tags")
}

model TagOnPost {
  postId Int
  post   Post @relation(fields: [postId], references: [id])
  tagId  Int
  tag    Tag  @relation(fields: [tagId], references: [id])

  @@id([postId, tagId])
  @@map("tag_on_posts")
}
```
