# 57 - Refresh Token - Perpanjangan Session yang Aman

---

## Penjelasan

Setelah register dan login berhasil menghasilkan access token (15 menit) dan refresh token (7 hari), kita perlu endpoint untuk memperpanjang session tanpa meminta user login ulang. Refresh token membutuhkan **token rotation** — setiap kali refresh, refresh token lama di-invalidasi dan diganti yang baru. Ini mencegah replay attack jika refresh token bocor. Refresh token juga bisa dikirim via **HttpOnly cookie** agar lebih aman dari XSS.

---

## Fungsi

- Menyediakan endpoint `POST /auth/refresh` untuk memperpanjang akses
- Validasi refresh token dengan secret yang berbeda dari access token
- Token rotation: invalidasi token lama, berikan token baru
- Opsi mengirim refresh token via HttpOnly cookie
- Logout menghapus refresh token dari database dan cookie

---

## Cara Pengimplementasian

### 1. JwtRefreshStrategy

```typescript
// auth/strategies/jwt-refresh.strategy.ts
import { Injectable, UnauthorizedException } from '@nestjs/common';
import { PassportStrategy } from '@nestjs/passport';
import { ExtractJwt, Strategy } from 'passport-jwt';
import { ConfigService } from '@nestjs/config';
import { Request } from 'express';

@Injectable()
export class JwtRefreshStrategy extends PassportStrategy(Strategy, 'jwt-refresh') {
  constructor(configService: ConfigService) {
    super({
      // Ambil refresh token dari body request (alternatif: dari cookie)
      jwtFromRequest: ExtractJwt.fromBodyField('refreshToken'),
      secretOrKey: configService.get<string>('JWT_REFRESH_SECRET'),
      passReqToCallback: true,
    });
  }

  async validate(req: Request, payload: { sub: string; email: string }) {
    const refreshToken = req.body?.refreshToken;
    if (!refreshToken) {
      throw new UnauthorizedException('Refresh token missing');
    }
    return { ...payload, refreshToken };
  }
}
```

### 2. JwtRefreshGuard

```typescript
// auth/guards/jwt-refresh.guard.ts
import { Injectable } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@Injectable()
export class JwtRefreshGuard extends AuthGuard('jwt-refresh') {}
```

### 3. AuthService — Refresh dengan Token Rotation

```typescript
// auth/auth.service.ts (tambahkan method)
import * as bcrypt from 'bcrypt';

@Injectable()
export class AuthService {
  // ... constructor dan method sebelumnya

  async refreshTokens(userId: string, incomingRefreshToken: string) {
    const user = await this.userService.findById(userId);
    if (!user || !user.refreshTokenHash) {
      throw new UnauthorizedException('Access denied');
    }

    // Validasi refresh token yang masuk dengan hash yang tersimpan di DB
    const isTokenValid = await bcrypt.compare(incomingRefreshToken, user.refreshTokenHash);
    if (!isTokenValid) {
      throw new UnauthorizedException('Invalid refresh token');
    }

    // Token rotation: generate token pair baru
    const tokens = await this.generateTokens(user.id, user.email);

    // Hash dan simpan refresh token baru
    const newHashedRefresh = await bcrypt.hash(tokens.refreshToken, this.saltRounds);
    await this.userService.updateRefreshToken(user.id, newHashedRefresh);

    return tokens;
  }

  async logout(userId: string) {
    await this.userService.updateRefreshToken(userId, null);
    return { message: 'Logged out successfully' };
  }
}
```

### 4. AuthController — Refresh & Logout

```typescript
// auth/auth.controller.ts (tambahkan)
import { UseGuards, Req } from '@nestjs/common';
import { JwtRefreshGuard } from './guards/jwt-refresh.guard';

@Controller('auth')
export class AuthController {
  constructor(private readonly authService: AuthService) {}

  @Post('refresh')
  @UseGuards(JwtRefreshGuard)
  @HttpCode(HttpStatus.OK)
  async refresh(@Req() req: any) {
    const { sub, refreshToken } = req.user;
    return this.authService.refreshTokens(sub, refreshToken);
  }

  @Post('logout')
  @HttpCode(HttpStatus.OK)
  async logout(@Req() req: any) {
    // Di real app, extract userId dari access token via guard
    const userId = req.user?.sub;
    if (userId) {
      return this.authService.logout(userId);
    }
    return { message: 'Logged out' };
  }
}
```

### 5. HttpOnly Cookie (Alternatif Lebih Aman)

Darima mengirim refresh token di body, kirim via HttpOnly cookie:

```typescript
// auth/auth.controller.ts — login dengan cookie
@Post('login')
@HttpCode(HttpStatus.OK)
async login(@Body() dto: LoginDto, @Res({ passthrough: true }) res: Response) {
  const tokens = await this.authService.login(dto);

  res.cookie('refreshToken', tokens.refreshToken, {
    httpOnly: true,    // Tidak bisa diakses JavaScript (aman dari XSS)
    secure: true,       // Hanya dikirim via HTTPS
    sameSite: 'strict', // Mencegah CSRF
    maxAge: 7 * 24 * 60 * 60 * 1000, // 7 hari
    path: '/auth/refresh', // Hanya dikirim ke endpoint refresh
  });

  return { accessToken: tokens.accessToken }; // refresh token tidak perlu dikirim body
}
```

```typescript
// JwtRefreshStrategy — extract dari cookie
jwtFromRequest: (req) => req?.cookies?.refreshToken,
```

```typescript
// auth/auth.controller.ts — logout hapus cookie
@Post('logout')
@HttpCode(HttpStatus.OK)
async logout(@Req() req: any, @Res({ passthrough: true }) res: Response) {
  const userId = req.user?.sub;
  if (userId) {
    await this.authService.logout(userId);
  }

  res.clearCookie('refreshToken', { path: '/auth/refresh' });
  return { message: 'Logged out' };
}
```

### 6. Flow Diagram Refresh dengan Rotation

```
Client                          Server
  │                                │
  │  POST /auth/refresh            │
  │  { refreshToken: "abc" }       │
  │ ──────────────────────────────►│
  │                                │
  │  JwtRefreshGuard:              │
  │    ✓ verifikasi signature JWT  │
  │    ✓ decode payload            │
  │                                │
  │  AuthService.refreshTokens():  │
  │    ✓ bcrypt.compare("abc",     │
  │       hash di DB)              │
  │    ✓ generate ACCESS + REFRESH │
  │    ✓ bcrypt.hash(refreshBaru)  │
  │    ✓ update hash di DB         │
  │    ◄── ROTASI SELESAI          │
  │                                │
  │  ◄── { accessToken,            │
  │         refreshToken }         │
  │        (+ cookie baru)         │
  │                                │
  │  === TOKEN LAMA INVALID ===    │
```

---

## Analogi

Bayangkan **kartu perpustakaan** yang habis dalam 7 hari. Begitu kartu habis, Anda datang ke meja sirkulasi (endpoint `/refresh`). Petugas mengecek keanggotaan Anda di buku besar (database), lalu **meninju kartu lama dan memberikan kartu baru** — kartu lama tidak bisa dipakai lagi (token rotation). Ini mencegah orang lain memakai kartu lama Anda. HttpOnly cookie adalah **kartu yang ditempel di kartu identitas utama** — tidak bisa difotokopi oleh sembarang orang (JavaScript).

---

## Dipakai Untuk Apa

- Session yang bertahan lama tanpa login ulang
- Aplikasi mobile / SPA yang perlu token terus-menerus
- Sistem keamanan tinggi yang butuh token rotation
- Mencegah refresh token stolen / replay attack

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Refresh token tidak di-rotate | Token yang bocor bisa dipakai selamanya |
| Refresh token disimpan di localStorage | Rawan XSS — gunakan HttpOnly cookie |
| Refresh token expiry terlalu lama (>30 hari) | Batasi 7 hari, atau perpendek |
| Tidak validasi hash refresh token di DB | Siapa pun yang punya token bisa refresh |
| Lupa hapus cookie saat logout | Panggil `res.clearCookie()` |

---

## Soal Latihan

### Soal 1
Implementasikan method `refreshTokens` di AuthService dengan token rotation. Validasi hash refresh token dari DB, lalu generate token baru dan simpan hash baru.

### Jawaban 1
```typescript
async refreshTokens(userId: string, incomingRefreshToken: string) {
  const user = await this.userService.findById(userId);
  if (!user || !user.refreshTokenHash) {
    throw new UnauthorizedException('Access denied');
  }

  const isTokenValid = await bcrypt.compare(incomingRefreshToken, user.refreshTokenHash);
  if (!isTokenValid) {
    throw new UnauthorizedException('Invalid refresh token');
  }

  const tokens = await this.generateTokens(user.id, user.email);
  const newHashedRefresh = await bcrypt.hash(tokens.refreshToken, this.saltRounds);
  await this.userService.updateRefreshToken(user.id, newHashedRefresh);

  return tokens;
}
```

### Soal 2
Mengapa refresh token perlu di-rotate? Jelaskan skenario serangan yang bisa terjadi jika tidak di-rotate.

### Jawaban 2
Jika refresh token tidak di-rotate, penyerang yang berhasil mencuri token (via XSS, man-in-the-middle, log server) bisa menggunakannya selamanya hingga expired. Dengan rotation, setiap penggunaan refresh token yang sah menghasilkan token baru dan yang lama menjadi invalid. Jika penyerang memakai token curian, user asli akan gagal refresh (token sudah invalid) dan langsung sadar ada yang salah.

### Soal 3
Apa kelebihan HttpOnly cookie dibanding mengirim refresh token di body response?

### Jawaban 3
HttpOnly cookie tidak bisa diakses oleh JavaScript, sehingga aman dari serangan XSS. Secure flag memastikan hanya dikirim via HTTPS. SameSite=strict mencegah CSRF. Path restriction membatasi cookie hanya dikirim ke endpoint tertentu.
