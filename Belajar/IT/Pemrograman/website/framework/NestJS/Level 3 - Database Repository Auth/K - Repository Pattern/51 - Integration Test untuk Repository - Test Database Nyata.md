# 51 - Integration Test untuk Repository - Test Database Nyata

## Penjelasan

Setelah membuat repository (Pertemuan 49 & 50), kita perlu memastikan bahwa implementasi repository benar-benar bekerja dengan database nyata. Unit test dengan mock tidak cukup karena tidak menguji query SQL, relasi, constraint, dan behavior database asli. Integration test menggunakan database sungguhan (bisa pakai database testing terpisah atau SQLite in-memory).

Jika repository adalah **keran yang sudah diproduksi**, maka integration test adalah **uji coba keran dengan air beneran** — bukan pakai simulator, tapi beneran dialiri air, dicek apakah bocor, apakah tekanan sesuai, apakah klep berfungsi.

## Fungsi

- **Test database nyata**: Menggunakan environment testing dengan database terpisah
- **Setup environment**: Konfigurasi terpisah untuk testing
- **Migration sebelum test**: Menjamin schema selalu up-to-date
- **beforeEach / afterEach**: Isolasi test — data bersih setiap test
- **Cleanup**: Hapus data setelah test agar tidak mengganggu test lain

## Cara Pengimplementasian

### 1. Setup Environment Testing

**`.env.test`:**
```
DATABASE_URL="postgresql://user:password@localhost:5432/test_db?schema=public"
```

**Konfigurasi di `package.json`:**
```json
{
  "scripts": {
    "test": "jest",
    "test:e2e": "jest --config ./test/jest-e2e.json",
    "test:integration": "dotenv -e .env.test -- jest --testPathPattern='\\.integration\\.spec\\.ts$'"
  }
}
```

### 2. Test Module Setup

```typescript
// src/users/repositories/__tests__/user-repository.integration.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { PrismaService } from '../../../prisma/prisma.service';
import { UserRepository } from '../user.repository';
import { PrismaModule } from '../../../prisma/prisma.module';

describe('UserRepository (Integration)', () => {
  let repository: UserRepository;
  let prisma: PrismaService;

  beforeAll(async () => {
    const module: TestingModule = await Test.createTestingModule({
      imports: [PrismaModule],
      providers: [UserRepository],
    }).compile();

    repository = module.get<UserRepository>(UserRepository);
    prisma = module.get<PrismaService>(PrismaService);

    // Jalankan migration sebelum test
    await prisma.$executeRawUnsafe('DROP SCHEMA IF EXISTS public CASCADE');
    await prisma.$executeRawUnsafe('CREATE SCHEMA public');
    // Atau jalankan prisma migrate melalui exec
  });

  beforeEach(async () => {
    // Bersihkan data sebelum setiap test
    await prisma.user.deleteMany();
  });

  afterAll(async () => {
    await prisma.$disconnect();
  });

  it('should create a user', async () => {
    const user = await repository.create({
      email: 'test@example.com',
      name: 'Test User',
      password: 'hashed_password',
    });

    expect(user).toBeDefined();
    expect(user.id).toBeDefined();
    expect(user.email).toBe('test@example.com');
    expect(user.name).toBe('Test User');
  });

  it('should find user by id', async () => {
    const created = await repository.create({
      email: 'find@example.com',
      name: 'Find Me',
      password: 'pass123',
    });

    const found = await repository.findById(created.id);
    expect(found).not.toBeNull();
    expect(found?.email).toBe('find@example.com');
  });

  it('should find user by email', async () => {
    await repository.create({
      email: 'unique@example.com',
      name: 'Unique',
      password: 'pass',
    });

    const found = await repository.findByEmail('unique@example.com');
    expect(found).not.toBeNull();
    expect(found?.name).toBe('Unique');
  });

  it('should return null when user not found', async () => {
    const found = await repository.findById(999);
    expect(found).toBeNull();
  });

  it('should update a user', async () => {
    const user = await repository.create({
      email: 'update@example.com',
      name: 'Before',
      password: 'pass',
    });

    const updated = await repository.update(user.id, { name: 'After' });
    expect(updated.name).toBe('After');
  });

  it('should delete a user', async () => {
    const user = await repository.create({
      email: 'delete@example.com',
      name: 'Delete Me',
      password: 'pass',
    });

    await repository.delete(user.id);
    const found = await repository.findById(user.id);
    expect(found).toBeNull();
  });

  it('should enforce unique email constraint', async () => {
    await repository.create({
      email: 'duplicate@example.com',
      name: 'First',
      password: 'pass',
    });

    await expect(
      repository.create({
        email: 'duplicate@example.com',
        name: 'Second',
        password: 'pass',
      }),
    ).rejects.toThrow();
  });
});
```

### 3. Setup Lebih Baik dengan Testcontainers (Opsional)

```typescript
// test/setup.ts
import { PostgreSqlContainer, StartedPostgreSqlContainer } from '@testcontainers/postgresql';

let container: StartedPostgreSqlContainer;

beforeAll(async () => {
  container = await new PostgreSqlContainer()
    .withDatabase('test_db')
    .withUsername('test')
    .withPassword('test')
    .start();

  process.env.DATABASE_URL = container.getConnectionUri();
});

afterAll(async () => {
  await container.stop();
});
```

### 4. Helper untuk Factory Data

```typescript
// test/factories/user.factory.ts
import { PrismaService } from '../../src/prisma/prisma.service';

export class UserFactory {
  constructor(private prisma: PrismaService) {}

  async create(overrides: Partial<any> = {}) {
    return this.prisma.user.create({
      data: {
        email: `user-${Date.now()}@example.com`,
        name: 'Default Name',
        password: 'hashed_password',
        ...overrides,
      },
    });
  }
}
```

### 5. Testing Service yang Menggunakan Repository

```typescript
// src/users/__tests__/users.service.integration.spec.ts
describe('UsersService (Integration)', () => {
  let service: UsersService;

  beforeAll(async () => {
    const module = await Test.createTestingModule({
      imports: [PrismaModule],
      providers: [UsersService, UserRepository],
    }).compile();

    service = module.get(UsersService);
  });

  it('should create and return user', async () => {
    const user = await service.createUser({
      email: 'service-test@example.com',
      name: 'Service Test',
      password: 'pass',
    });

    const found = await service.getUserById(user.id);
    expect(found.email).toBe('service-test@example.com');
  });
});
```

## Analogi

**Membangun Gedung Bertingkat**

- **Integration test dengan database nyata** = **uji coba keran dengan air beneran dari PDAM**, bukan air ember
- **beforeAll + migration** = **membangun instalasi pipa** dulu sebelum uji coba
- **beforeEach (deleteMany)** = **menguras air** setelah setiap uji coba biar tidak tercampur
- **Unique constraint test** = **sengaja pasang dua keran di lubang yang sama** — harusnya error/ tidak muat
- **Factory** = **pabrik komponen** yang produksi part standar untuk testing

## Dipakai untuk Apa

- Memvalidasi query database berfungsi sesuai skema
- Mengetes constraint unik, foreign key, relasi
- Mendeteksi perubahan schema yang merusak query
- Regression testing sebelum deploy
- CI/CD pipeline yang butuh kepastian database layer berfungsi

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Data antar test saling mempengaruhi | `beforeEach` hapus data (deleteMany) |
| Koneksi database testing sama dengan development | Pakai `.env.test` terpisah |
| Migration tidak dijalankan sebelum test | Running `prisma migrate deploy` di beforeAll |
| Test lambat karena connect/disconnect tiap test | Connect sekali di beforeAll, disconnect di afterAll |
| Lupa handle error database | Test juga skenario error (unique constraint, foreign key) |

## Soal Latihan

Tulis integration test untuk UserRepository dengan skenario:
1. Create user berhasil
2. Find user by id mengembalikan user
3. Find user by email mengembalikan user
4. Find by id dengan id tidak ada mengembalikan null
5. Update user berhasil
6. Delete user berhasil
7. Unique email constraint bekerja

### Jawaban

```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { PrismaModule } from '../../../prisma/prisma.module';
import { PrismaService } from '../../../prisma/prisma.service';
import { UserRepository } from '../user.repository';

describe('UserRepository Integration Test', () => {
  let repository: UserRepository;
  let prisma: PrismaService;

  beforeAll(async () => {
    const module: TestingModule = await Test.createTestingModule({
      imports: [PrismaModule],
      providers: [UserRepository],
    }).compile();

    repository = module.get<UserRepository>(UserRepository);
    prisma = module.get<PrismaService>(PrismaService);
  });

  beforeEach(async () => {
    await prisma.user.deleteMany();
  });

  afterAll(async () => {
    await prisma.$disconnect();
  });

  it('should create a user', async () => {
    const user = await repository.create({
      email: 'create@test.com',
      name: 'Create Test',
      password: 'hash123',
    });
    expect(user).toHaveProperty('id');
    expect(user.email).toBe('create@test.com');
  });

  it('should find user by id', async () => {
    const created = await repository.create({
      email: 'findbyid@test.com',
      name: 'Find By ID',
      password: 'hash123',
    });
    const found = await repository.findById(created.id);
    expect(found).not.toBeNull();
    expect(found?.email).toBe('findbyid@test.com');
  });

  it('should find user by email', async () => {
    await repository.create({
      email: 'findbyemail@test.com',
      name: 'Find By Email',
      password: 'hash123',
    });
    const found = await repository.findByEmail('findbyemail@test.com');
    expect(found).not.toBeNull();
    expect(found?.name).toBe('Find By Email');
  });

  it('should return null for non-existent id', async () => {
    const found = await repository.findById(99999);
    expect(found).toBeNull();
  });

  it('should update user', async () => {
    const user = await repository.create({
      email: 'update@test.com',
      name: 'Old Name',
      password: 'hash123',
    });
    const updated = await repository.update(user.id, { name: 'New Name' });
    expect(updated.name).toBe('New Name');
  });

  it('should delete user', async () => {
    const user = await repository.create({
      email: 'delete@test.com',
      name: 'Delete Test',
      password: 'hash123',
    });
    await repository.delete(user.id);
    const found = await repository.findById(user.id);
    expect(found).toBeNull();
  });

  it('should reject duplicate email', async () => {
    await repository.create({
      email: 'duplicate@test.com',
      name: 'First',
      password: 'hash123',
    });
    await expect(
      repository.create({
        email: 'duplicate@test.com',
        name: 'Second',
        password: 'hash123',
      }),
    ).rejects.toThrow();
  });
});
```
