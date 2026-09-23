# 32 - JWT Authentication Guard - Implementasi Lengkap

## Penjelasan

Di materi sebelumnya (31 — Metadata & Reflector), kita belajar **komunikasi antara decorator dan Guard** — API key masih statis. Sekarang kita naik ke level sebenarnya: **JWT (JSON Web Token)**.

JWT adalah standar autentikasi modern — seperti **kartu akses elektronik** yang diterbitkan saat login, lalu client kirimkan kembali di setiap request. Guard kita perlu memverifikasi: "Apakah kartu ini asli? Apakah masih berlaku? Siapa pemiliknya?"

NestJS menyediakan integrasi mulus dengan **Passport** (strategi autentikasi populer) melalui `@nestjs/passport` dan `@nestjs/jwt`.

## Fungsi

- **Memverifikasi token JWT** dari header Authorization
- **Mengekstrak payload** (user info) dari token dan menyimpannya di `request.user`
- **Menolak request** jika token tidak valid, expired, atau tidak ada
- **Menyediakan user context** untuk controller dan service selanjutnya

## Cara Implementasi

### 1. Install Dependencies

```bash
npm install @nestjs/jwt @nestjs/passport passport passport-jwt
npm install -D @types/passport-jwt
```

### 2. JwtModule — Registrasi

```typescript
// src/auth/auth.module.ts
import { Module } from '@nestjs/common';
import { JwtModule } from '@nestjs/jwt';
import { PassportModule } from '@nestjs/passport';
import { JwtStrategy } from './strategies/jwt.strategy';
import { AuthService } from './auth.service';
import { AuthController } from './auth.controller';

@Module({
  imports: [
    PassportModule.register({ defaultStrategy: 'jwt' }),
    JwtModule.registerAsync({
      useFactory: () => ({
        secret: process.env.JWT_SECRET,
        signOptions: {
          expiresIn: '1d',
        },
      }),
    }),
  ],
  providers: [AuthService, JwtStrategy],
  controllers: [AuthController],
  exports: [JwtModule, PassportModule],
})
export class AuthModule {}
```

### 3. JwtStrategy — Strategy untuk Validasi Token

```typescript
// src/auth/strategies/jwt.strategy.ts
import { Injectable, UnauthorizedException } from '@nestjs/common';
import { PassportStrategy } from '@nestjs/passport';
import { ExtractJwt, Strategy } from 'passport-jwt';
import { UsersService } from '../../users/users.service';

interface JwtPayload {
  sub: number;   // user ID
  email: string;
  role: string;
}

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor(private usersService: UsersService) {
    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      ignoreExpiration: false,
      secretOrKey: process.env.JWT_SECRET,
    });
  }

  async validate(payload: JwtPayload) {
    // Passport otomatis memverifikasi signature & expiration JWT
    // Kita tinggal validasi payload (misal: user masih aktif?)
    const user = await this.usersService.findById(payload.sub);

    if (!user) {
      throw new UnauthorizedException('User tidak ditemukan');
    }

    // Return value ini akan disimpan di request.user
    return {
      id: payload.sub,
      email: payload.email,
      role: payload.role,
    };
  }
}
```

### 4. JwtAuthGuard — Guard Wrapper

```typescript
// src/auth/guards/jwt-auth.guard.ts
import { Injectable, ExecutionContext, UnauthorizedException } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';
import { Reflector } from '@nestjs/core';
import { IS_PUBLIC_KEY } from '../../common/decorators/public.decorator';

@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {
  constructor(private reflector: Reflector) {
    super();
  }

  canActivate(context: ExecutionContext): boolean | Promise<boolean> {
    const isPublic = this.reflector.getAllAndOverride<boolean>(IS_PUBLIC_KEY, [
      context.getHandler(),
      context.getClass(),
    ]);

    if (isPublic) {
      return true; // skip autentikasi
    }

    return super.canActivate(context); // jalankan Passport JWT
  }

  handleRequest(err: any, user: any, info: any) {
    // info.message = 'No auth token' / 'jwt expired' / etc.
    if (err || !user) {
      throw err || new UnauthorizedException(
        info?.message || 'Autentikasi diperlukan',
      );
    }
    return user;
  }
}
```

### 5. `@CurrentUser()` — Custom Decorator untuk Akses User

```typescript
// src/common/decorators/current-user.decorator.ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common';
import { Request } from 'express';

export const CurrentUser = createParamDecorator(
  (data: string | undefined, context: ExecutionContext) => {
    const request = context.switchToHttp().getRequest<Request>();
    const user = request.user as any;

    // Jika data diberikan, ambil property tertentu (e.g., @CurrentUser('email'))
    return data ? user?.[data] : user;
  },
);
```

### 6. Implementasi di Controller

```typescript
// src/cats/cats.controller.ts
import { Controller, Get, Post, UseGuards } from '@nestjs/common';
import { JwtAuthGuard } from '../auth/guards/jwt-auth.guard';
import { Public } from '../common/decorators/public.decorator';
import { CurrentUser } from '../common/decorators/current-user.decorator';

@Controller('cats')
@UseGuards(JwtAuthGuard)
export class CatsController {
  @Get()
  findAll(@CurrentUser() user: any) {
    console.log('Diakses oleh:', user.email);
    return ['kucing1', 'kucing2'];
  }

  @Post('login')
  @Public() // tidak perlu JWT
  login() {
    return 'login endpoint';
  }

  @Get('profile')
  getProfile(@CurrentUser('email') email: string) {
    return `Email kamu: ${email}`;
  }
}
```

### 7. Login Endpoint — Menerbitkan Token

```typescript
// src/auth/auth.service.ts
import { Injectable } from '@nestjs/common';
import { JwtService } from '@nestjs/jwt';

@Injectable()
export class AuthService {
  constructor(private jwtService: JwtService) {}

  login(user: { id: number; email: string; role: string }) {
    const payload = { sub: user.id, email: user.email, role: user.role };

    return {
      accessToken: this.jwtService.sign(payload),
    };
  }
}
```

## Analogi — Gedung Bertingkat

Bayangkan **JWT Authentication** seperti sistem **Kartu Akses Elektronik** di gedung:

- **Login** = Karyawan daftar ke **HRD**, dapat **kartu akses** (JWT token) yang berisi data: nama, ID, level akses
- **Header Authorization: Bearer <token>** = Karyawan **menempelkan kartu** ke reader di pintu masuk
- **JwtStrategy.validate()** = **Sistem reader** ngecek: "Kartu asli? (signature valid) Masih berlaku? (expired?) Data karyawan masih aktif?"
- **request.user** = Setelah lolos, **reader menampilkan data**: "Selamat datang, Budi (Admin)"
- **`@Public()`** = **Pintu darurat / pintu tamu** — siapa pun bisa lewat tanpa kartu
- **`@CurrentUser()`** = Karyawan tinggal **scan ID card** — datanya langsung muncul

JWT adalah **kartu akses yang tidak bisa dipalsukan** karena ada signature digital. Satpam (Guard) cukup verifikasi signature tanpa perlu telepon HRD tiap kali.

## Dipakai untuk Apa

- **Autentikasi API modern** — SPA, mobile app, microservices
- **Single sign-on (SSO)** — Satu token digunakan di banyak service
- **Stateless authentication** — Server tidak perlu menyimpan session
- **Integrasi dengan third-party** via OAuth2 + JWT
- **Microservices communication** — Service-to-service auth via JWT

## Kesalahan Umum

1. **Lupa install `@nestjs/passport` dan `passport-jwt`**: Error `Passport.Strategy is not a constructor`.
2. **Secret key hardcoded**: Selalu pakai environment variable. Jangan commit secret ke Git.
3. **Tidak handle `expiresIn`**: Token akan berlaku selamanya — risiko keamanan besar.
4. **Lupa register JwtStrategy di providers**: Strategy harus di-provide agar Passport mengenal strategi 'jwt'.
5. **`ExtractJwt.fromAuthHeaderAsBearerToken()` — client kirim token di header**: Jika client kirim token di cookie atau query param, gunakan extractor yang berbeda.
6. **Over-fetching di `validate()`**: `validate()` dijalankan **setiap request** — jangan query database berat di sini. Cache user jika perlu.
7. **Lupa default strategy**: `PassportModule.register({ defaultStrategy: 'jwt' })` — biar tidak perlu sebut 'jwt' setiap `@UseGuards()`.

## Soal Latihan

**Soal**: Implementasikan JWT guard lengkap:
1. Buat `JwtModule` dengan `registerAsync` di `AuthModule`
2. Buat `JwtStrategy` yang mengambil user dari `UsersService`
3. Buat `JwtAuthGuard` yang extend `AuthGuard('jwt')`
4. Buat `@CurrentUser()` decorator
5. Terapkan di `CatsController` dengan satu endpoint `@Public()`

<details>
<summary>Jawaban</summary>

```typescript
// src/auth/auth.module.ts
import { Module } from '@nestjs/common';
import { JwtModule } from '@nestjs/jwt';
import { PassportModule } from '@nestjs/passport';
import { JwtStrategy } from './strategies/jwt.strategy';
import { UsersModule } from '../users/users.module';

@Module({
  imports: [
    PassportModule.register({ defaultStrategy: 'jwt' }),
    JwtModule.registerAsync({
      useFactory: () => ({
        secret: process.env.JWT_SECRET,
        signOptions: { expiresIn: '1d' },
      }),
    }),
    UsersModule,
  ],
  providers: [JwtStrategy],
  exports: [JwtModule, PassportModule],
})
export class AuthModule {}
```

```typescript
// src/auth/strategies/jwt.strategy.ts
import { Injectable, UnauthorizedException } from '@nestjs/common';
import { PassportStrategy } from '@nestjs/passport';
import { ExtractJwt, Strategy } from 'passport-jwt';
import { UsersService } from '../../users/users.service';

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor(private usersService: UsersService) {
    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      ignoreExpiration: false,
      secretOrKey: process.env.JWT_SECRET,
    });
  }

  async validate(payload: { sub: number; email: string; role: string }) {
    const user = await this.usersService.findById(payload.sub);
    if (!user) throw new UnauthorizedException('User tidak ditemukan');
    return { id: payload.sub, email: payload.email, role: payload.role };
  }
}
```

```typescript
// src/auth/guards/jwt-auth.guard.ts
import { Injectable, ExecutionContext, UnauthorizedException } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';
import { Reflector } from '@nestjs/core';
import { IS_PUBLIC_KEY } from '../../common/decorators/public.decorator';

@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {
  constructor(private reflector: Reflector) {
    super();
  }

  canActivate(context: ExecutionContext) {
    const isPublic = this.reflector.getAllAndOverride<boolean>(
      IS_PUBLIC_KEY, [context.getHandler(), context.getClass()],
    );
    if (isPublic) return true;
    return super.canActivate(context);
  }

  handleRequest(err: any, user: any, info: any) {
    if (err || !user) {
      throw err || new UnauthorizedException(info?.message || 'Token tidak valid');
    }
    return user;
  }
}
```

```typescript
// src/common/decorators/current-user.decorator.ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common';
import { Request } from 'express';

export const CurrentUser = createParamDecorator(
  (data: string | undefined, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest<Request>();
    const user = request.user as any;
    return data ? user?.[data] : user;
  },
);
```

```typescript
// src/cats/cats.controller.ts
import { Controller, Get, Post, UseGuards } from '@nestjs/common';
import { JwtAuthGuard } from '../auth/guards/jwt-auth.guard';
import { Public } from '../common/decorators/public.decorator';
import { CurrentUser } from '../common/decorators/current-user.decorator';

@Controller('cats')
@UseGuards(JwtAuthGuard)
export class CatsController {
  @Get()
  findAll(@CurrentUser() user: any) {
    return `Halo ${user.email}, ini daftar kucing`;
  }

  @Post('register')
  @Public()
  register() {
    return 'pendaftaran publik';
  }
}
```
</details>
