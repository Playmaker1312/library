# 55 - AuthModule - Arsitektur Authentication yang Bersih

---

## Penjelasan

Setelah kita punya UserModule (dengan entity, service, repository) dan ConfigModule (dengan environment variables), sekarang kita bangun AuthModule. Authentication adalah lapisan paling kritis di aplikasi. Arsitektur yang bersih akan memisahkan tanggung jawab: AuthController (HTTP), AuthService (logika), JwtStrategy (validasi token), dan JwtRefreshStrategy (perpanjangan session). Module ini membutuhkan dependency ke UserModule, JwtModule, dan ConfigModule.

---

## Fungsi

- Menyediakan endpoint register, login, refresh token, logout
- Memvalidasi kredensial user
- Mengeluarkan JWT access token & refresh token
- Melindungi route dengan Guard
- Memisahkan strategi autentikasi (JWT, OAuth, dll) dari controller

---

## Cara Pengimplementasian

### 1. Struktur folder

```
src/auth/
├── auth.module.ts
├── auth.controller.ts
├── auth.service.ts
├── strategies/
│   ├── jwt.strategy.ts
│   └── jwt-refresh.strategy.ts
├── guards/
│   ├── jwt-auth.guard.ts
│   └── jwt-refresh.guard.ts
├── dto/
│   ├── register.dto.ts
│   ├── login.dto.ts
│   └── refresh.dto.ts
└── decorators/
    └── current-user.decorator.ts
```

### 2. AuthModule

```typescript
// auth.module.ts
import { Module } from '@nestjs/common';
import { JwtModule } from '@nestjs/jwt';
import { PassportModule } from '@nestjs/passport';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { AuthController } from './auth.controller';
import { AuthService } from './auth.service';
import { JwtStrategy } from './strategies/jwt.strategy';
import { JwtRefreshStrategy } from './strategies/jwt-refresh.strategy';
import { UserModule } from '../user/user.module';

@Module({
  imports: [
    UserModule, // untuk validasi user di database
    PassportModule.register({ defaultStrategy: 'jwt' }),
    JwtModule.registerAsync({
      imports: [ConfigModule],
      inject: [ConfigService],
      useFactory: (config: ConfigService) => ({
        secret: config.get<string>('JWT_SECRET'),
        signOptions: {
          expiresIn: '15m',
        },
      }),
    }),
  ],
  controllers: [AuthController],
  providers: [AuthService, JwtStrategy, JwtRefreshStrategy],
  exports: [AuthService],
})
export class AuthModule {}
```

### 3. Alur lengkap

```
┌──────────┐     ┌──────────────┐     ┌───────────┐     ┌──────────────┐
│  Client  │ ──► │AuthController│ ──► │AuthService │ ──► │  UserModule  │
└──────────┘     └──────────────┘     └───────────┘     └──────────────┘
    │                  │                    │                   │
    │ POST /register   │                    │                   │
    │ POST /login      │   validasi krdsial │                   │
    │ POST /refresh    │   generate token   │   bcrypt compare  │
    │ POST /logout     │                    │   save refresh    │
    │                  │                    │                   │
    ▼                                    ┌───────────────────┐
┌──────────┐                             │  JwtStrategy      │
│  JWT     │ ◄───────────────────────────│  (validate token) │
│  Guard   │                             └───────────────────┘
└──────────┘
```

### 4. Diagram auth flow

```
REGISTER
  Client ──POST /auth/register──► AuthService ──► UserService.create()
     │                                      │
     └──────────────────────────────────────┘
              response: { message }

LOGIN
  Client ──POST /auth/login──► AuthService
     │                           ├── UserService.findByEmail()
     │                           ├── bcrypt.compare()
     │                           ├── generateAccessToken()
     │                           ├── generateRefreshToken()
     │                           └── saveRefreshHashToDB()
     │
     ◄── { accessToken, refreshToken }

REFRESH
  Client ──POST /auth/refresh──► JwtRefreshGuard
     │                              ├── validasi refresh token
     │                              └── AuthService.refresh()
     │                                   ├── rotate token
     │                                   └── new { accessToken, refreshToken }
     ◄── { accessToken, refreshToken }

LOGOUT
  Client ──POST /auth/logout──► AuthService.removeRefreshToken()
     ◄── { message: "logged out" }
```

---

## Analogi

AuthModule adalah **resepsionis + satpam** di lobi gedung. Resepsionis (AuthController) menerima tamu, mencatat kedatangan (register). Satpam (Guard) memeriksa ID card (JWT) sebelum tamu naik ke lantai atas. JwtStrategy adalah **mesin pemindai KTP** — membaca dan memvalidasi data di ID card. JwtRefreshStrategy adalah **meja perpanjangan ID card sementara** — kalau ID habis masa berlaku, tamu bisa perpanjang tanpa harus daftar ulang.

---

## Dipakai Untuk Apa

- Semua aplikasi yang perlu login/logout
- API yang dilindungi JWT
- Sistem multi-role (admin, user, author)
- Aplikasi yang butuh session tahan lama dengan refresh token
- Integrasi OAuth atau SSO

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| AuthModule import berantakan (circular dependency) | Gunakan `forwardRef()` jika perlu |
| JwtStrategy dan logic auth tercampur di controller | Pisahkan: controller → service → strategy |
| Lupa register JwtStrategy di `providers` | Guard tidak akan berfungsi |
| Access token terlalu lama (lebih dari 15-30 menit) | Set `expiresIn: '15m'` dan gunakan refresh token |
| Refresh token disimpan plaintext di DB | Hash pakai bcrypt sebelum simpan |

---

## Soal Latihan

### Soal 1
Buat struktur module auth (folder dan file) lengkap dengan dependency injection-nya.

### Jawaban 1
```
src/auth/
├── auth.module.ts           # imports: UserModule, JwtModule, PassportModule, ConfigModule
├── auth.controller.ts       # @Controller('auth')
├── auth.service.ts          # @Injectable()
├── strategies/
│   ├── jwt.strategy.ts      # extends PassportStrategy(Strategy)
│   └── jwt-refresh.strategy.ts
├── guards/
│   ├── jwt-auth.guard.ts    # extends AuthGuard('jwt')
│   └── jwt-refresh.guard.ts
├── dto/
│   ├── register.dto.ts
│   ├── login.dto.ts
│   └── refresh.dto.ts
└── decorators/
    └── current-user.decorator.ts
```

### Soal 2
Jelaskan dengan analogi gedung peran `JwtStrategy` dan `JwtRefreshStrategy`.

### Jawaban 2
JwtStrategy adalah **mesin pemindai KTP** — membaca expiry date dan data pemilik di kartu ID. JwtRefreshStrategy adalah **meja perpanjangan ID card sementara** — kalau kartu utama habis, tamu bisa perpanjang tanpa harus registrasi ulang.

### Soal 3
Mengapa `JwtModule` perlu di-register dengan `registerAsync`?

### Jawaban 3
Karena secret key JWT berasal dari environment variable (`ConfigService.get('JWT_SECRET')`) yang baru tersedia saat runtime. `registerAsync` memungkinkan kita mengakses `ConfigService` sebelum JwtModule dikonfigurasi.
