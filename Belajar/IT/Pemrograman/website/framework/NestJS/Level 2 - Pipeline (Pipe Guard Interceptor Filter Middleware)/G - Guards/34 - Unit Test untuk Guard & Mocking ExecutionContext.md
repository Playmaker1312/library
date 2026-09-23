# 34 - Unit Test untuk Guard & Mocking ExecutionContext

## Penjelasan

Di materi sebelumnya (33 — RBAC), kita sudah punya **JwtAuthGuard** dan **RolesGuard** yang kompleks. Sekarang kita perlu memastikan mereka bekerja dengan benar melalui **unit test**.

Guard adalah lapisan keamanan kritis — satu bug di Guard bisa membuka akses ke data sensitif. Unit test membantu kita memverifikasi: token valid diterima, token expired ditolak, role tidak cukup ditolak, endpoint publik bisa diakses tanpa token.

Kita akan menggunakan **Jest** (built-in di NestJS) dan **mocking ExecutionContext** untuk mensimulasikan berbagai skenario.

## Fungsi

- **Memverifikasi logika Guard** tanpa perlu menjalankan server
- **Mensimulasikan berbagai kondisi** token (valid, expired, tidak ada)
- **Memastikan error yang tepat** dilempar untuk setiap skenario
- **Memastikan metadata/public decorator** diproses dengan benar
- **Regression testing** — kode baru tidak merusak Guard yang sudah ada

## Cara Implementasi

### 1. Mock ExecutionContext

```typescript
// test/mocks/execution-context.mock.ts
import { ExecutionContext } from '@nestjs/common';
import { Reflector } from '@nestjs/core';

export function createMockExecutionContext(options?: {
  user?: any;
  isPublic?: boolean;
  roles?: string[];
}): ExecutionContext {
  const { user = null, isPublic = false, roles = [] } = options || {};

  const request: any = {
    headers: {},
    user,
  };

  const handler = () => {};
  // Tempel metadata di handler
  if (isPublic) Reflect.defineMetadata('isPublic', true, handler);
  if (roles.length > 0) Reflect.defineMetadata('roles', roles, handler);

  return {
    switchToHttp: () => ({
      getRequest: () => request,
    }),
    getHandler: () => handler,
    getClass: () => class MockController {},
  } as unknown as ExecutionContext;
}
```

### 2. Unit Test JwtAuthGuard

```typescript
// src/auth/guards/jwt-auth.guard.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { JwtAuthGuard } from './jwt-auth.guard';
import { Reflector } from '@nestjs/core';
import { ExecutionContext, UnauthorizedException } from '@nestjs/common';

describe('JwtAuthGuard', () => {
  let guard: JwtAuthGuard;
  let reflector: Reflector;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        JwtAuthGuard,
        {
          provide: Reflector,
          useValue: {
            getAllAndOverride: jest.fn(),
          },
        },
      ],
    }).compile();

    guard = module.get<JwtAuthGuard>(JwtAuthGuard);
    reflector = module.get<Reflector>(Reflector);
  });

  // Helper untuk bikin mock context
  const mockContext = (overrides?: any) =>
    ({
      switchToHttp: () => ({
        getRequest: () => overrides?.request || { headers: {} },
      }),
      getHandler: () => overrides?.handler || (() => {}),
      getClass: () => overrides?.class || (() => {}),
    }) as ExecutionContext;

  describe('canActivate', () => {
    it('harus return true jika endpoint @Public()', () => {
      jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue(true);

      const result = guard.canActivate(mockContext());

      expect(result).toBe(true);
    });

    it('harus panggil super.canActivate jika tidak @Public()', () => {
      jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue(false);

      // Guard akan panggil Passport AuthGuard — kita test bahwa dia return Promise
      const result = guard.canActivate(mockContext());

      expect(result).toBeInstanceOf(Promise);
    });
  });

  describe('handleRequest', () => {
    it('harus return user jika valid', () => {
      const user = { id: 1, email: 'test@test.com', role: 'admin' };
      const result = guard.handleRequest(null, user, null);
      expect(result).toEqual(user);
    });

    it('harus throw UnauthorizedException jika user null', () => {
      expect(() => guard.handleRequest(null, null, { message: 'No auth token' }))
        .toThrow(UnauthorizedException);
    });

    it('harus throw error yang diberikan', () => {
      const error = new Error('Token expired');
      expect(() => guard.handleRequest(error, null, null))
        .toThrow('Token expired');
    });
  });
});
```

### 3. Unit Test RolesGuard

```typescript
// src/auth/guards/roles.guard.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { RolesGuard } from './roles.guard';
import { Reflector } from '@nestjs/core';
import { ExecutionContext, ForbiddenException } from '@nestjs/common';

describe('RolesGuard', () => {
  let guard: RolesGuard;
  let reflector: Reflector;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        RolesGuard,
        {
          provide: Reflector,
          useValue: {
            getAllAndOverride: jest.fn(),
          },
        },
      ],
    }).compile();

    guard = module.get<RolesGuard>(RolesGuard);
    reflector = module.get<Reflector>(Reflector);
  });

  const mockContext = (user?: any) =>
    ({
      switchToHttp: () => ({
        getRequest: () => ({ user }),
      }),
      getHandler: () => ({}),
      getClass: () => ({}),
    }) as ExecutionContext;

  it('harus return true jika tidak ada @Roles()', () => {
    jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue(undefined);

    const result = guard.canActivate(mockContext({ role: 'viewer' }));

    expect(result).toBe(true);
  });

  it('harus return true jika role user cocok', () => {
    jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue(['admin']);

    const result = guard.canActivate(mockContext({ role: 'admin' }));

    expect(result).toBe(true);
  });

  it('harus throw ForbiddenException jika role tidak cocok', () => {
    jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue(['admin']);

    expect(() => guard.canActivate(mockContext({ role: 'viewer' })))
      .toThrow(ForbiddenException);
  });

  it('harus throw ForbiddenException jika user tidak ada', () => {
    jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue(['admin']);

    expect(() => guard.canActivate(mockContext(null)))
      .toThrow(ForbiddenException);
  });

  it('harus return true untuk salah satu role yang cocok', () => {
    jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue(['admin', 'editor']);

    expect(guard.canActivate(mockContext({ role: 'editor' }))).toBe(true);
    expect(guard.canActivate(mockContext({ role: 'admin' }))).toBe(true);
    expect(() => guard.canActivate(mockContext({ role: 'viewer' })))
      .toThrow(ForbiddenException);
  });
});
```

### 4. Menjalankan Test

```bash
# Test semua guard
npx jest --testPathPattern="guard"

# Test dengan coverage
npx jest --testPathPattern="guard" --coverage
```

## Analogi — Gedung Bertingkat

Bayangkan **Unit Test Guard** seperti **Simulasi Keamanan** gedung:

- **Mock ExecutionContext** = **Aktor pemeriksa keamanan** yang datang dengan berbagai skenario
- **Test token valid** = Aktor datang dengan **kartu akses asli** — satpam harus buka pintu
- **Test token expired** = Aktor datang dengan **kartu kadaluwarsa** — satpam harus tolak
- **Test role tidak cukup** = Aktor datang dengan **kartu level 1**, mau masuk ruang level 5 — satpam harus tolak
- **Test @Public()** = Aktor datang **tanpa kartu** ke pintu darurat — satpam harus izinkan

Tanpa unit test, kita harus **tes manual setiap skenario** — repot dan rawan lupa. Dengan unit test, kita punya **50 aktor berbeda** yang datang bergantian, dan kita pastikan satpam (Guard) bereaksi benar.

## Dipakai untuk Apa

- **CI/CD pipeline** — Guard otomatis di-test setiap deploy
- **Regression testing** — pastikan perubahan tidak merusak keamanan
- **TDD (Test-Driven Development)** — tulis test dulu sebelum implementasi Guard
- **Documentation by example** — test case menjelaskan behavior Guard
- **Code review** — reviewer bisa lihat test untuk paham skenario yang di-handle

## Kesalahan Umum

1. **Tidak mock `Reflector` dengan benar**: Guard punya dependency ke `Reflector` — harus di-provide di testing module.
2. **Mock ExecutionContext tidak lengkap**: Guard panggil `getHandler()` dan `getClass()` — pastikan mock menyediakan keduanya.
3. **Lupa test edge case**: Token expired, token tanpa user, role undefined, metadata tidak ada.
4. **Test dependency terhadap module lain**: Gunakan mocking, jangan import module asli.
5. **Async test tidak di-await**: Guard bisa return Promise — pastikan test menggunakan `await` atau `resolves`.

## Soal Latihan

**Soal**: Tulis unit test untuk `JwtAuthGuard` dan `RolesGuard`:
1. Test JwtAuthGuard: token valid, expired, tidak ada
2. Test RolesGuard: role cocok, tidak cocok, user tidak ada
3. Test @Public() endpoint — harus bypass JwtAuthGuard

<details>
<summary>Jawaban</summary>

```typescript
// src/auth/guards/jwt-auth.guard.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { JwtAuthGuard } from './jwt-auth.guard';
import { Reflector } from '@nestjs/core';
import { ExecutionContext, UnauthorizedException } from '@nestjs/common';

describe('JwtAuthGuard', () => {
  let guard: JwtAuthGuard;
  let reflector: Reflector;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        JwtAuthGuard,
        { provide: Reflector, useValue: { getAllAndOverride: jest.fn() } },
      ],
    }).compile();

    guard = module.get<JwtAuthGuard>(JwtAuthGuard);
    reflector = module.get<Reflector>(Reflector);
  });

  const mockCtx = () => ({ switchToHttp: () => ({ getRequest: () => ({}) }), getHandler: () => ({}), getClass: () => ({}) }) as ExecutionContext;

  it('return true untuk @Public()', () => {
    jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue(true);
    expect(guard.canActivate(mockCtx())).toBe(true);
  });

  it('throw untuk user null', () => {
    expect(() => guard.handleRequest(null, null, { message: 'token tidak ada' }))
      .toThrow(UnauthorizedException);
  });

  it('return user untuk user valid', () => {
    const user = { id: 1, email: 'a@a.com', role: 'admin' };
    expect(guard.handleRequest(null, user, null)).toEqual(user);
  });
});
```

```typescript
// src/auth/guards/roles.guard.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { RolesGuard } from './roles.guard';
import { Reflector } from '@nestjs/core';
import { ExecutionContext, ForbiddenException } from '@nestjs/common';

describe('RolesGuard', () => {
  let guard: RolesGuard;
  let reflector: Reflector;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        RolesGuard,
        { provide: Reflector, useValue: { getAllAndOverride: jest.fn() } },
      ],
    }).compile();

    guard = module.get<RolesGuard>(RolesGuard);
    reflector = module.get<Reflector>(Reflector);
  });

  const mockCtx = (user?: any) =>
    ({ switchToHttp: () => ({ getRequest: () => ({ user }) }), getHandler: () => ({}), getClass: () => ({}) }) as ExecutionContext;

  it('return true jika tidak ada @Roles()', () => {
    jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue(undefined);
    expect(guard.canActivate(mockCtx({ role: 'viewer' }))).toBe(true);
  });

  it('return true jika role cocok', () => {
    jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue(['admin']);
    expect(guard.canActivate(mockCtx({ role: 'admin' }))).toBe(true);
  });

  it('throw ForbiddenException jika role tidak cocok', () => {
    jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue(['admin']);
    expect(() => guard.canActivate(mockCtx({ role: 'viewer' }))).toThrow(ForbiddenException);
  });

  it('throw jika user tidak ada', () => {
    jest.spyOn(reflector, 'getAllAndOverride').mockReturnValue(['admin']);
    expect(() => guard.canActivate(mockCtx(null))).toThrow(ForbiddenException);
  });
});
```
</details>
