# 107 - Auth & Permission di GraphQL + Federation

## Penjelasan
Setelah GraphQL CRUD dan DataLoader selesai (materi 105-106), kita perlu mengamankan endpoint. Di REST, auth via Guard dan middleware sudah dibahas di Level 4. Di GraphQL, prinsipnya sama — Guard bekerja di level resolver. Tantangan tambahan: **GraphQL Federation** (menggabungkan schema dari multiple service) dan **introspection** (harus dimatikan di production).

## Fungsi
- **Guard di GraphQL resolver**: Proteksi query/mutation dengan decorator `@UseGuards()`.
- **Context (request+user)**: Parse token di context, inject user ke resolver.
- **GraphQL Federation**: Menggabungkan schema dari beberapa microservices menjadi satu graph.
- **Introspection disabled di production**: Cegah client melihat schema lengkap.

## Cara Pengimplementasian

### 1. Auth Guard untuk GraphQL

**`common/guards/gql-auth.guard.ts`**
```typescript
import { Injectable, ExecutionContext, UnauthorizedException } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';
import { GqlExecutionContext } from '@nestjs/graphql';

@Injectable()
export class GqlAuthGuard extends AuthGuard('jwt') {
  getRequest(context: ExecutionContext) {
    const ctx = GqlExecutionContext.create(context);
    return ctx.getContext().req;
  }
}

@Injectable()
export class GqlRolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const requiredRoles = this.reflector.getAllAndOverride<string[]>('roles', [
      context.getHandler(),
      context.getClass(),
    ]);
    if (!requiredRoles) return true;

    const ctx = GqlExecutionContext.create(context);
    const user = ctx.getContext().req.user;

    return requiredRoles.some(role => user.role === role);
  }
}
```

### 2. Custom Decorator — Current User
```typescript
// common/decorators/current-user.decorator.ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common';
import { GqlExecutionContext } from '@nestjs/graphql';

export const CurrentUser = createParamDecorator(
  (data: unknown, context: ExecutionContext) => {
    const ctx = GqlExecutionContext.create(context);
    return ctx.getContext().req.user;
  },
);
```

### 3. GraphQL Context — Parse Token
```typescript
// main.ts atau app.module
GraphQLModule.forRoot<ApolloDriverConfig>({
  driver: ApolloDriver,
  autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
  context: ({ req }) => ({ req }),
  // Atau dengan auth:
  // context: ({ req }) => {
  //   const user = parseToken(req.headers.authorization);
  //   return { req, user };
  // },
})
```

### 4. Implementasi di Resolver
```typescript
import { UseGuards } from '@nestjs/common';
import { Resolver, Query, Mutation, Args, Int } from '@nestjs/graphql';
import { GqlAuthGuard } from '../common/guards/gql-auth.guard';
import { GqlRolesGuard } from '../common/guards/gql-roles.guard';
import { Roles } from '../common/decorators/roles.decorator';
import { CurrentUser } from '../common/decorators/current-user.decorator';
import { User } from './models/user.model';

@Resolver(() => User)
export class UsersResolver {
  // Public — semua orang bisa akses
  @Query(() => [User])
  async users() { /* ... */ }

  // Hanya authenticated user
  @Query(() => User)
  @UseGuards(GqlAuthGuard)
  async profile(@CurrentUser() user: User) {
    return user;
  }

  // Hanya admin
  @Mutation(() => User)
  @UseGuards(GqlAuthGuard, GqlRolesGuard)
  @Roles('ADMIN')
  async deleteUser(@Args('id', { type: () => Int }) id: number) {
    // ...
  }
}
```

### 5. Disable Introspection di Production
```typescript
// app.module.ts
GraphQLModule.forRoot<ApolloDriverConfig>({
  driver: ApolloDriver,
  autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
  introspection: process.env.NODE_ENV !== 'production',
  playground: process.env.NODE_ENV !== 'production',
  // Atau filter field tertentu dari introspection
  // buildSchemaOptions: {
  //   fieldResolverEnhancers: ['guards'],
  // },
})
```

### 6. GraphQL Federation — Composing Multiple Services

Federation memungkinkan service terpisah memiliki schema masing-masing, digabung menjadi satu "supergraph".

**Service A (Users Service)**
```typescript
// Package.json perlu: @apollo/subgraph
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { ApolloFederationDriver, ApolloFederationDriverConfig } from '@nestjs/apollo';

@Module({
  imports: [
    GraphQLModule.forRoot<ApolloFederationDriverConfig>({
      driver: ApolloFederationDriver,
      autoSchemaFile: { federation: 2 },
    }),
  ],
})
export class AppModule {}
```

**Service B (Posts Service)**
```typescript
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { ApolloFederationDriver, ApolloFederationDriverConfig } from '@nestjs/apollo';

@Module({
  imports: [
    GraphQLModule.forRoot<ApolloFederationDriverConfig>({
      driver: ApolloFederationDriver,
      autoSchemaFile: { federation: 2 },
    }),
  ],
})
export class AppModule {}
```

**Gateway — Gabungkan Semua Service**
```typescript
// Di Gateway service
import { IntrospectAndCompose } from '@apollo/gateway';
import { ApolloGatewayDriver, ApolloGatewayDriverConfig } from '@nestjs/apollo';

@Module({
  imports: [
    GraphQLModule.forRoot<ApolloGatewayDriverConfig>({
      driver: ApolloGatewayDriver,
      gateway: {
        supergraphSdl: new IntrospectAndCompose({
          subgraphs: [
            { name: 'users', url: 'http://users-service:3001/graphql' },
            { name: 'posts', url: 'http://posts-service:3002/graphql' },
          ],
        }),
      },
    }),
  ],
})
export class AppModule {}
```

**Extend Type dari Service Lain**
```typescript
// Di Users Service — User punya field posts yang di-provide Posts Service
@ObjectType()
@Directive('@key(fields: "id")')
export class User {
  @Field(() => Int)
  id: number;

  @Field()
  email: string;

  // Posts service akan provide field ini
  @Field(() => [Post])
  posts?: Post[];
}

// Di Posts Service
@ObjectType()
@Directive('@key(fields: "id")')
export class Post {
  @Field(() => Int)
  id: number;

  @Field()
  title: string;

  // Reference ke User dari service lain
  @Field(() => User)
  @Directive('@provides(fields: "email")')
  author: User;
}
```

## Analogi
Gedung besar dengan banyak lantai (service). **Auth Guard** adalah satpam di pintu lift — cek KTP (token JWT). **Roles Guard** adalah satpam di lantai tertentu — cek apakah KTP punya akses lantai VIP. **GraphQL Context** adalah papan informasi yang dibawa satpam — nama, foto, role pengunjung.

**Federation**: Setiap lantai punya **papan informasi sendiri** (schema sendiri). Tapi di lobi utama, ada **papan super** (gateway) yang menggabungkan semua informasi — pengunjung bisa lihat info dari lantai 1, 2, 3 dalam satu tampilan tanpa harus naik-turun.

**Introspection disabled**: Seperti menutup peta gedung — pengunjung hanya bisa tahu apa yang mereka akses, bukan seluruh denah gedung.

## Dipakai untuk apa
- GraphQL API yang butuh authentication dan authorization.
- Multi-tenant apps — user hanya lihat data miliknya.
- Microservices dengan GraphQL — federation untuk menggabungkan schema.
- Production API — introspection dimatikan untuk keamanan.

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Lupa convert context di Guard | Error "req is undefined" | Gunakan `GqlExecutionContext.create(context).getContext().req` |
| Introspection aktif di production | Attacker bisa lihat seluruh schema | Set `introspection: false` |
| Federation version mismatch | Error kompatibilitas | Pastikan semua service pakai Apollo Federation 2 |
| Guards di field resolver | Periksa auth untuk setiap field | Auth di level query/mutation saja, bukan field resolver |
| Token parsing ganda di context dan guard | Redundant code | Parse sekali di context, guard hanya check |

## Soal Latihan

**Soal 1:** Implementasikan GraphQL auth guard yang memproteksi mutation `deleteUser` hanya untuk role ADMIN.

**Jawaban 1:**
```typescript
@Mutation(() => User)
@UseGuards(GqlAuthGuard, GqlRolesGuard)
@Roles('ADMIN')
async deleteUser(@Args('id', { type: () => Int }) id: number) {
  return this.usersService.delete(id);
}
```

**Soal 2:** Setup GraphQL Federation dengan 2 service: Users (port 3001) dan Posts (port 3002), digabung via Gateway di port 3000. Bagaimana cara Posts service mereferensi User dari Users service?

**Jawaban 2:**
- Users service: `@ObjectType() @Directive('@key(fields: "id")') class User { @Field(() => Int) id }`
- Posts service: `@ObjectType() @Directive('@key(fields: "id")') class User { @Field(() => Int) id; @Field() email }` dengan `@ResolveReference` untuk resolve User by ID
- Gateway: `IntrospectAndCompose` dengan subgraphs pointing ke kedua service
- Client query ke gateway (port 3000) bisa ambil user + posts dalam satu query
