# 106 - N+1 Problem & DataLoader di GraphQL

## Penjelasan
Di GraphQL, masalah N+1 lebih terasa dibanding REST. Kenapa? Karena **field resolver** — setiap kali resolver memanggil database untuk field relasi, GraphQL memanggilnya **per parent object**. Contoh: query `posts { author { name } }` — jika ada 10 post, GraphQL panggil `author` resolver 10x → 10 query database. Ini N+1. **DataLoader** dari Facebook adalah solusi standar: batch + cache.

## Fungsi
- **N+1 problem**: Field resolver dipanggil N kali untuk N parent — inefisien.
- **DataLoader**: Batch queries + cache per request — 1 query untuk semua parent.
- **Batch**: Kumpulkan semua ID, query sekali dengan `WHERE id IN (...)`.
- **Cache**: DataLoader cache per request — jika data yang sama diminta dua kali, ambil dari cache.

## Cara Pengimplementasian

### Identifikasi N+1 — Query Posts dengan Author
```graphql
query {
  posts {
    title
    author {      # ← Ini trigger field resolver 'author' per post
      name
      email
    }
  }
}
```

### ❌ Tanpa DataLoader — N+1
```typescript
@Resolver(() => Post)
export class PostsResolver {
  constructor(private usersService: UsersService) {}

  @ResolveField(() => User)
  async author(@Parent() post: Post) {
    // Dipanggil N kali — untuk setiap post!
    return this.usersService.findOne(post.authorId);
  }
}
```
Jika 100 post → 100 query `SELECT * FROM user WHERE id = ?`.

### ✅ Dengan DataLoader

**Install**
```bash
npm install dataloader
```

**Buat UsersLoader**
```typescript
// users/users.loader.ts
import * as DataLoader from 'dataloader';
import { Injectable } from '@nestjs/common';
import { PrismaService } from '../prisma/prisma.service';
import { User } from './models/user.model';

@Injectable()
export class UsersLoader {
  constructor(private prisma: PrismaService) {}

  createUsersLoader() {
    return new DataLoader<number, User>(async (ids: number[]) => {
      const users = await this.prisma.user.findMany({
        where: { id: { in: [...ids] as number[] } },
      });

      // Map — pastikan urutan sesuai input IDs
      const userMap = new Map(users.map(u => [u.id, u]));
      return ids.map(id => userMap.get(id) || new Error(`User ${id} not found`));
    });
  }
}
```

**Integrasi di Module — Request Scope**
```typescript
// posts/posts.module.ts
import { Module, Scope } from '@nestjs/common';
import { PostsResolver } from './posts.resolver';
import { PostsService } from './posts.service';
import { UsersLoader } from '../users/users.loader';

@Module({
  providers: [
    PostsResolver,
    PostsService,
    UsersLoader,
    {
      provide: 'USERS_LOADER',
      useFactory: (loader: UsersLoader) => loader.createUsersLoader(),
      scope: Scope.REQUEST,   // NEW instance per request
      inject: [UsersLoader],
    },
  ],
})
export class PostsModule {}
```

**Resolver dengan DataLoader**
```typescript
@Resolver(() => Post)
export class PostsResolver {
  constructor(
    private readonly postsService: PostsService,
    @Inject('USERS_LOADER') private readonly usersLoader: DataLoader<number, User>,
  ) {}

  @Query(() => [Post])
  async posts() {
    return this.postsService.findAll();
  }

  @ResolveField(() => User)
  async author(@Parent() post: Post) {
    // DataLoader akan batch semua ID → 1 query
    return this.usersLoader.load(post.authorId);
  }
}
```

### Multiple Field Resolvers — Comments + Author
```typescript
@Resolver(() => Post)
export class PostsResolver {
  constructor(
    @Inject('USERS_LOADER') private usersLoader: DataLoader<number, User>,
    @Inject('COMMENTS_LOADER') private commentsLoader: DataLoader<number, Comment[]>,
  ) {}

  @ResolveField(() => User)
  async author(@Parent() post: Post) {
    return this.usersLoader.load(post.authorId);
  }

  @ResolveField(() => [Comment])
  async comments(@Parent() post: Post) {
    return this.commentsLoader.load(post.id);
  }
}
```

**CommentsLoader**
```typescript
new DataLoader<number, Comment[]>(async (postIds: number[]) => {
  const comments = await this.prisma.comment.findMany({
    where: { postId: { in: [...postIds] } },
  });

  const commentsByPost = new Map<number, Comment[]>();
  postIds.forEach(id => commentsByPost.set(id, []));
  comments.forEach(c => commentsByPost.get(c.postId)?.push(c));

  return postIds.map(id => commentsByPost.get(id) || []);
});
```

### DataLoader Lifecycle
```typescript
// Middleware — buat DataLoader baru per request, hapus setelah selesai
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class DataLoaderMiddleware implements NestMiddleware {
  constructor(
    private usersLoader: UsersLoader,
    private commentsLoader: CommentsLoader,
  ) {}

  use(req: Request, res: Response, next: NextFunction) {
    req['loaders'] = {
      users: this.usersLoader.createUsersLoader(),
      comments: this.commentsLoader.createCommentsLoader(),
    };
    next();
  }
}
```

### Debug N+1 — Logging
```typescript
// Tambahkan logging di Prisma untuk lihat jumlah query
this.$on('query', (e: any) => {
  console.log(`[Query] ${e.query} (${e.duration}ms)`);
});

// Sebelum DataLoader: 1 + 100 query = 101 query
// Sesudah DataLoader: 1 + 1 query = 2 query
```

## Analogi
Gedung punya 100 ruang (posts), setiap ruang punya nomor kontak pengelola (authorId). Tanpa DataLoader, petugas berlari ke ruang arsip 100 kali — setiap kali ambil satu kontak. Dengan DataLoader, petugas kumpulkan semua 100 nomor, lalu ke ruang arsip **sekali** — "Saya perlu kontak nomor 1, 2, 3, ..., 100" — sekaligus dapat semua.

DataLoader juga punya **papan catatan** (cache) — jika ada dua ruang dengan kontak yang sama (authorId duplikat), petugas tidak perlu ke arsip dua kali.

## Dipakai untuk apa
- GraphQL API dengan field resolver yang akses database.
- REST API dengan batch loading (tidak eksklusif GraphQL).
- Query yang punya nested array: `posts → comments → user`.
- Aplikasi dengan relasi database many-to-one, one-to-many.

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| DataLoader singleton (global) | Data dari request sebelumnya ikut terbawa | Gunakan `Scope.REQUEST` |
| Tidak handle missing key | Error "User not found" untuk authorId yang sudah dihapus | Return `null` atau `Error` untuk key tidak ditemukan |
| Tidak preserve urutan | DataLoader asumsikan urutan output = urutan input | Map manual dengan `ids.map(id => map.get(id))` |
| Batch function terlalu kompleks | Cache dan batch jadi lambat | Jaga batch function sederhana |
| Field resolver tanpa DataLoader di query besar | Ribuan query tak terduga | Gunakan DataLoader sebagai default |

## Soal Latihan

**Soal 1:** Identifikasi N+1 problem di resolver berikut dan perbaiki dengan DataLoader:
```typescript
@Resolver(() => Comment)
export class CommentsResolver {
  @ResolveField(() => User)
  async user(@Parent() comment: Comment) {
    return this.usersService.findOne(comment.userId);
  }
}
```

**Jawaban 1:**
```typescript
@Injectable()
export class UsersLoader {
  createLoader() {
    return new DataLoader<number, User>(async (ids) => {
      const users = await this.prisma.user.findMany({
        where: { id: { in: [...ids] } },
      });
      const map = new Map(users.map(u => [u.id, u]));
      return ids.map(id => map.get(id) || new Error(`Not found: ${id}`));
    });
  }
}

// Di resolver
@ResolveField(() => User)
async user(@Parent() comment: Comment) {
  return this.usersLoader.load(comment.userId); // Batch → 1 query
}
```

**Soal 2:** Apa perbedaan utama DataLoader dengan `include` Prisma?

**Jawaban 2:** `include` Prisma (JOIN) baik untuk relasi yang pasti dibutuhkan di semua request. DataLoader lebih fleksibel — hanya batch query jika field di-request oleh client. Kombinasi: gunakan `include` untuk data yang sering diminta, DataLoader untuk field yang jarang atau opsional.
