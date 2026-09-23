# 104 - GraphQL Setup - Code-First Approach

## Penjelasan
Selama Level 1-5 kita menggunakan REST API — endpoint statis (GET /users, POST /users). REST punya kelemahan: **over-fetching** (dapat data kebanyakan) dan **under-fetching** (dapat data kurang, butuh request lagi). **GraphQL** mengatasi ini — client menentukan persis data yang dibutuhkan. NestJS mendukung GraphQL dengan *Code-First* (class TypeScript → schema) dan *Schema-First* (file .graphql).

## Fungsi
- **GraphQL vs REST**: Client tentukan field yang dimau — tidak over/under fetching.
- **Code-First**: Tulis TypeScript class → otomatis generate GraphQL schema.
- **@nestjs/graphql + @nestjs/apollo + graphql**: Library utama.
- **GraphQLModule.forRoot()**: Setup GraphQL endpoint dengan Apollo driver.

## Cara Pengimplementasian

### Install
```bash
npm install @nestjs/graphql @nestjs/apollo @apollo/server graphql
```

### Setup GraphQL Module
```typescript
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { ApolloDriver, ApolloDriverConfig } from '@nestjs/apollo';
import { join } from 'path';
import { UsersModule } from './users/users.module';

@Module({
  imports: [
    GraphQLModule.forRoot<ApolloDriverConfig>({
      driver: ApolloDriver,
      autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
      sortSchema: true,
      playground: process.env.NODE_ENV !== 'production',
      debug: process.env.NODE_ENV !== 'production',
    }),
    UsersModule,
  ],
})
export class AppModule {}
```

### User Type (ObjectType)
```typescript
import { Field, Int, ObjectType } from '@nestjs/graphql';

@ObjectType()
export class User {
  @Field(() => Int)
  id: number;

  @Field()
  email: string;

  @Field({ nullable: true })
  name?: string;

  @Field()
  role: string;

  @Field()
  createdAt: Date;

  // Tidak di-expose ke GraphQL — tidak ada @Field()
  password: string;
}
```

### Users Resolver
```typescript
import { Resolver, Query, Mutation, Args, Int } from '@nestjs/graphql';
import { User } from './models/user.model';
import { CreateUserInput } from './dto/create-user.input';

@Resolver(() => User)
export class UsersResolver {
  constructor(private readonly usersService: UsersService) {}

  @Query(() => [User], { name: 'users' })
  async findAll() {
    return this.usersService.findAll();
  }

  @Query(() => User, { name: 'user' })
  async findOne(@Args('id', { type: () => Int }) id: number) {
    return this.usersService.findOne(id);
  }

  @Mutation(() => User)
  async createUser(@Args('input') input: CreateUserInput) {
    return this.usersService.create(input);
  }

  @Mutation(() => User)
  async deleteUser(@Args('id', { type: () => Int }) id: number) {
    return this.usersService.delete(id);
  }
}
```

### Input Type
```typescript
import { InputType, Field } from '@nestjs/graphql';

@InputType()
export class CreateUserInput {
  @Field()
  email: string;

  @Field({ nullable: true })
  name?: string;

  @Field()
  password: string;
}
```

### Test Query
```graphql
query {
  users { id email name }
  user(id: 1) { id role }
}

mutation {
  createUser(input: { email: "a@b.com", password: "123" }) {
    id email
  }
}
```

## Analogi
REST seperti **menu prasmanan** — bayar tetap, ambil semua, mau dimakan atau tidak. GraphQL seperti **restoran a la carte** — pesan tepat yang dimau: "Nasi (id), Ayam (email), tanpa sayur (name)." Code-First seperti menulis resep dalam Bahasa Indonesia, chef otomatis terjemahkan ke Bahasa Prancis (GraphQL schema).

## Dipakai untuk apa
- Aplikasi dengan banyak client (web, mobile, third-party) — masing-masing butuh data berbeda.
- Dashboard / analytics — butuh fleksibilitas query tinggi.
- Relasi data kompleks — ambil user + posts + comments dalam satu request.

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Lupa @Field() decorator | Field tidak muncul di schema | Setiap field yang ingin di-expose harus @Field() |
| Tidak tentukan nullable | Semua field required — error jika null | Tambah `{ nullable: true }` |
| Playground aktif di production | Ekspos schema ke publik | Set `playground: false`, `introspection: false` |
| Mutation return type salah | Error di client | Pastikan return type sesuai ObjectType |

## Soal Latihan

**Soal 1:** Setup GraphQL code-first dengan tipe `User` (id, email, name) dan resolver untuk `users` dan `user(id: Int!)`.

**Jawaban 1:**
```typescript
// app.module.ts
GraphQLModule.forRoot<ApolloDriverConfig>({
  driver: ApolloDriver,
  autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
})

// user.model.ts
@ObjectType()
export class User {
  @Field(() => Int) id: number;
  @Field() email: string;
  @Field({ nullable: true }) name?: string;
}

// users.resolver.ts
@Resolver(() => User)
export class UsersResolver {
  @Query(() => [User]) async users() { /* ... */ }
  @Query(() => User) async user(@Args('id', { type: () => Int }) id: number) { /* ... */ }
}
```

**Soal 2:** Apa beda `@ObjectType()` dan `@InputType()`? Kapan pakai yang mana?

**Jawaban 2:** `@ObjectType()` untuk data yang **dikembalikan** ke client (response). `@InputType()` untuk data yang **diterima** dari client (argument mutation/query). ObjectType bisa berisi relasi dan field computed; InputType hanya field data mentah.
