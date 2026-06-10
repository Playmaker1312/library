# 105 - Types Resolvers & Args - Core GraphQL di NestJS

## Penjelasan
Setelah setup GraphQL (materi 104), kita perlu memahami building block utama: **Types** (struktur data), **Resolvers** (fungsi query/mutation), dan **Args** (parameter). Dengan CRUD Post sebagai contoh, kita akan lihat how-to setiap decorator GraphQL di NestJS.

## Fungsi
- **@ObjectType / @Field**: Mendefinisikan tipe data (schema).
- **@Resolver**: Class yang berisi query/mutation/subscription.
- **@Query / @Mutation / @Subscription**: Decorator untuk operasi GraphQL.
- **@Args / @InputType**: Parameter input untuk query/mutation.

## Cara Pengimplementasian

### 1. Model Post (ObjectType)
```typescript
import { Field, Int, ObjectType } from '@nestjs/graphql';
import { User } from '../../users/models/user.model';

@ObjectType()
export class Post {
  @Field(() => Int)
  id: number;

  @Field()
  title: string;

  @Field({ nullable: true })
  content?: string;

  @Field()
  published: boolean;

  @Field(() => Int)
  authorId: number;

  @Field(() => User)
  author: User;           // Relasi — di-resolve oleh field resolver

  @Field()
  createdAt: Date;

  @Field()
  updatedAt: Date;
}
```

### 2. Input Types untuk CRUD
```typescript
// dto/create-post.input.ts
import { InputType, Field } from '@nestjs/graphql';

@InputType()
export class CreatePostInput {
  @Field()
  title: string;

  @Field({ nullable: true })
  content?: string;

  @Field({ defaultValue: false })
  published: boolean;

  @Field(() => Int)
  authorId: number;
}

// dto/update-post.input.ts
@InputType()
export class UpdatePostInput {
  @Field({ nullable: true })
  title?: string;

  @Field({ nullable: true })
  content?: string;

  @Field({ nullable: true })
  published?: boolean;
}
```

### 3. Posts Resolver (Full CRUD)
```typescript
import { Resolver, Query, Mutation, Args, Int, ResolveField, Parent } from '@nestjs/graphql';
import { Post } from './models/post.model';
import { CreatePostInput } from './dto/create-post.input';
import { UpdatePostInput } from './dto/update-post.input';
import { PostsService } from './posts.service';
import { UsersService } from '../users/users.service';
import { User } from '../users/models/user.model';

@Resolver(() => Post)
export class PostsResolver {
  constructor(
    private readonly postsService: PostsService,
    private readonly usersService: UsersService,
  ) {}

  // ------ QUERIES ------
  @Query(() => [Post], { name: 'posts' })
  async findAll(
    @Args('skip', { type: () => Int, nullable: true }) skip?: number,
    @Args('take', { type: () => Int, nullable: true }) take?: number,
  ) {
    return this.postsService.findAll({ skip, take });
  }

  @Query(() => Post, { name: 'post' })
  async findOne(@Args('id', { type: () => Int }) id: number) {
    return this.postsService.findOne(id);
  }

  // ------ MUTATIONS ------
  @Mutation(() => Post)
  async createPost(@Args('input') input: CreatePostInput) {
    return this.postsService.create(input);
  }

  @Mutation(() => Post)
  async updatePost(
    @Args('id', { type: () => Int }) id: number,
    @Args('input') input: UpdatePostInput,
  ) {
    return this.postsService.update(id, input);
  }

  @Mutation(() => Post)
  async deletePost(@Args('id', { type: () => Int }) id: number) {
    return this.postsService.delete(id);
  }

  // ------ FIELD RESOLVER (relasi author) ------
  @ResolveField(() => User)
  async author(@Parent() post: Post) {
    return this.usersService.findOne(post.authorId);
  }
}
```

### 4. Posts Service
```typescript
import { Injectable, NotFoundException } from '@nestjs/common';
import { PrismaService } from '../prisma/prisma.service';

@Injectable()
export class PostsService {
  constructor(private prisma: PrismaService) {}

  async findAll(params: { skip?: number; take?: number }) {
    return this.prisma.post.findMany({
      skip: params.skip,
      take: params.take || 10,
      include: { author: true },
    });
  }

  async findOne(id: number) {
    const post = await this.prisma.post.findUnique({ where: { id } });
    if (!post) throw new NotFoundException(`Post #${id} not found`);
    return post;
  }

  async create(input: CreatePostInput) {
    return this.prisma.post.create({ data: input });
  }

  async update(id: number, input: UpdatePostInput) {
    await this.findOne(id); // verify exists
    return this.prisma.post.update({ where: { id }, data: input });
  }

  async delete(id: number) {
    await this.findOne(id);
    return this.prisma.post.delete({ where: { id } });
  }
}
```

### 5. Subscription (Real-time)
```typescript
import { Subscription } from '@nestjs/graphql';
import { PubSub } from 'graphql-subscriptions';

const pubSub = new PubSub();

@Resolver(() => Post)
export class PostsResolver {
  @Subscription(() => Post)
  postCreated() {
    return pubSub.asyncIterator('postCreated');
  }
}

// Di service setelah create:
pubSub.publish('postCreated', { postCreated: newPost });
```

### 6. GraphQL Query Contoh
```graphql
# Query dengan args
query {
  posts(skip: 0, take: 5) {
    id
    title
    author { id email name }
  }
}

# Mutation
mutation {
  createPost(input: {
    title: "Belajar GraphQL"
    content: "Isi post"
    authorId: 1
  }) {
    id
    title
  }
}

# Subscription
subscription {
  postCreated {
    id
    title
  }
}
```

## Analogi
Gedung punya **papan informasi** (ObjectType) — setiap papan punya format: judul (title), isi (content), penulis (author). **Resepsionis** (Resolver) melayani: "Tolong cari post #5" (@Query), "Buat post baru" (@Mutation), "Kasih tahu saya jika ada post baru" (@Subscription). **Kertas permintaan** (Args/InputType) berisi detail yang resepsionis butuhkan.

## Dipakai untuk apa
- CRUD penuh untuk resource (User, Post, Product, Order).
- Relasi data — field resolver untuk ambil data dari resource lain.
- Real-time update — subscription untuk notifikasi, chat, live feed.
- API publik / third-party developers.

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| @ResolveField tanpa n+1 handling | Setiap post panggil query user terpisah | Gunakan DataLoader (materi 106) |
| Tidak handle nullable field | Error "Cannot return null for non-nullable field" | Set `@Field({ nullable: true })` |
| Subscription tanpa PubSub provider | Error "No pubsub implementation" | Install `graphql-subscriptions` |
| Args type salah | GraphQL error "Variable mismatch" | Sesuaikan type decorator dengan data |

## Soal Latihan

**Soal 1:** Implementasikan CRUD Post lengkap: model, input types, resolver dengan query, mutation, dan field resolver untuk author.

**Jawaban 1:** Lihat kode di atas — `Post` model, `CreatePostInput`/`UpdatePostInput`, `PostsResolver` dengan `@Query`, `@Mutation`, `@ResolveField`.

**Soal 2:** Tambahkan subscription `postCreated` yang di-trigger setiap kali post baru dibuat.

**Jawaban 2:**
```typescript
import { PubSub } from 'graphql-subscriptions';
const pubSub = new PubSub();

// Di resolver
@Subscription(() => Post)
postCreated() {
  return pubSub.asyncIterator('postCreated');
}

// Di service setelah create
async create(input: CreatePostInput) {
  const post = await this.prisma.post.create({ data: input });
  pubSub.publish('postCreated', { postCreated: post });
  return post;
}
```
