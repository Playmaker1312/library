# 58 - OAuth 2.0 - Login dengan Google & GitHub

---

## Penjelasan

Setelah implementasi login dengan email-password (local strategy), kita perlu opsi login dengan akun Google atau GitHub. OAuth 2.0 memungkinkan user login tanpa perlu membuat password baru. NestJS menyediakan integrasi dengan Passport.js melalui `@nestjs/passport`. Kita akan implementasi **authorization code flow**: user diarahkan ke provider (Google/GitHub), login di sana, lalu diarahkan kembali ke aplikasi kita dengan authorization code yang ditukar dengan token.

---

## Fungsi

- Login dengan akun Google / GitHub tanpa password
- Authorization code flow (redirect → consent → callback)
- Upsert user: jika email sudah terdaftar, update data; jika belum, buat user baru
- Mengembalikan JWT (access + refresh token) setelah OAuth sukses

---

## Cara Pengimplementasian

### 1. Install package

```bash
npm install passport-google-oauth20 passport-github2
npm install -D @types/passport-google-oauth20 @types/passport-github2
```

### 2. Google Strategy

```typescript
// auth/strategies/google.strategy.ts
import { Injectable } from '@nestjs/common';
import { PassportStrategy } from '@nestjs/passport';
import { Strategy, VerifyCallback } from 'passport-google-oauth20';
import { ConfigService } from '@nestjs/config';

@Injectable()
export class GoogleStrategy extends PassportStrategy(Strategy, 'google') {
  constructor(configService: ConfigService) {
    super({
      clientID: configService.get<string>('GOOGLE_CLIENT_ID'),
      clientSecret: configService.get<string>('GOOGLE_CLIENT_SECRET'),
      callbackURL: configService.get<string>('GOOGLE_CALLBACK_URL'),
      scope: ['email', 'profile'],
    });
  }

  async validate(
    accessToken: string,
    refreshToken: string,
    profile: any,
    done: VerifyCallback,
  ): Promise<any> {
    const { name, emails, photos } = profile;
    const user = {
      email: emails[0].value,
      name: name.givenName + ' ' + name.familyName,
      picture: photos[0].value,
      provider: 'google',
      providerId: profile.id,
    };
    done(null, user);
  }
}
```

### 3. GitHub Strategy

```typescript
// auth/strategies/github.strategy.ts
import { Injectable } from '@nestjs/common';
import { PassportStrategy } from '@nestjs/passport';
import { Strategy, Profile } from 'passport-github2';
import { ConfigService } from '@nestjs/config';
import { VerifyCallback } from 'passport-oauth2';

@Injectable()
export class GithubStrategy extends PassportStrategy(Strategy, 'github') {
  constructor(configService: ConfigService) {
    super({
      clientID: configService.get<string>('GITHUB_CLIENT_ID'),
      clientSecret: configService.get<string>('GITHUB_CLIENT_SECRET'),
      callbackURL: configService.get<string>('GITHUB_CALLBACK_URL'),
      scope: ['user:email'],
    });
  }

  async validate(
    accessToken: string,
    refreshToken: string,
    profile: Profile,
    done: VerifyCallback,
  ): Promise<any> {
    const { username, emails, photos } = profile;
    const user = {
      email: emails?.[0]?.value || `${profile.id}@github.local`,
      name: username || profile.displayName,
      picture: photos?.[0]?.value || null,
      provider: 'github',
      providerId: profile.id,
    };
    done(null, user);
  }
}
```

### 4. AuthController — OAuth endpoint

```typescript
// auth/auth.controller.ts
import { Get, UseGuards, Req, Res } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@Controller('auth')
export class AuthController {
  constructor(
    private readonly authService: AuthService,
    private readonly configService: ConfigService,
  ) {}

  // === GOOGLE ===
  @Get('google')
  @UseGuards(AuthGuard('google'))
  async googleAuth() {
    // Guard akan redirect ke Google consent screen
  }

  @Get('google/callback')
  @UseGuards(AuthGuard('google'))
  async googleCallback(@Req() req: any, @Res() res: any) {
    const tokens = await this.authService.handleOAuthLogin(req.user);
    // Redirect ke frontend dengan token
    return res.redirect(
      `${this.configService.get('FRONTEND_URL')}/oauth/callback?accessToken=${tokens.accessToken}&refreshToken=${tokens.refreshToken}`
    );
  }

  // === GITHUB ===
  @Get('github')
  @UseGuards(AuthGuard('github'))
  async githubAuth() {}

  @Get('github/callback')
  @UseGuards(AuthGuard('github'))
  async githubCallback(@Req() req: any, @Res() res: any) {
    const tokens = await this.authService.handleOAuthLogin(req.user);
    return res.redirect(
      `${this.configService.get('FRONTEND_URL')}/oauth/callback?accessToken=${tokens.accessToken}&refreshToken=${tokens.refreshToken}`
    );
  }
}
```

### 5. AuthService — Handle OAuth (Upsert)

```typescript
// auth/auth.service.ts (tambahkan)
@Injectable()
export class AuthService {
  constructor(
    private readonly userService: UserService,
    private readonly jwtService: JwtService,
    private readonly configService: ConfigService,
  ) {}

  async handleOAuthLogin(oauthUser: {
    email: string;
    name: string;
    picture: string;
    provider: string;
    providerId: string;
  }) {
    // Upsert: cari user berdasarkan email
    let user = await this.userService.findByEmail(oauthUser.email);

    if (user) {
      // User sudah ada — update profil OAuth
      await this.userService.update(user.id, {
        name: oauthUser.name,
        picture: oauthUser.picture,
        provider: oauthUser.provider,
        providerId: oauthUser.providerId,
      });
    } else {
      // User baru — buat akun
      user = await this.userService.create({
        email: oauthUser.email,
        name: oauthUser.name,
        picture: oauthUser.picture,
        provider: oauthUser.provider,
        providerId: oauthUser.providerId,
        password: null, // OAuth user tidak punya password
      });
    }

    // Generate token pair
    const tokens = await this.generateTokens(user.id, user.email);
    const hashedRefresh = await bcrypt.hash(tokens.refreshToken, this.saltRounds);
    await this.userService.updateRefreshToken(user.id, hashedRefresh);

    return tokens;
  }
}
```

### 6. Update User Entity

```typescript
// user/entities/user.entity.ts (tambahkan field)
export class User {
  // ... field sebelumnya

  @Column({ nullable: true })
  provider: string | null; // 'google' | 'github' | null

  @Column({ nullable: true })
  providerId: string | null; // ID dari provider

  @Column({ nullable: true })
  picture: string | null;

  @Column({ nullable: true })
  password: string | null; // null untuk OAuth user
}
```

### 7. Environment variables

```bash
# .env.development
GOOGLE_CLIENT_ID=xxxxxxxxx.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=GOCSPX-xxxxx
GOOGLE_CALLBACK_URL=http://localhost:3000/auth/google/callback

GITHUB_CLIENT_ID=Ov23lixxxxxx
GITHUB_CLIENT_SECRET=xxxxxxxxx
GITHUB_CALLBACK_URL=http://localhost:3000/auth/github/callback

FRONTEND_URL=http://localhost:5173
```

### 8. Daftarkan Strategi di Module

```typescript
// auth/auth.module.ts
import { GoogleStrategy } from './strategies/google.strategy';
import { GithubStrategy } from './strategies/github.strategy';

@Module({
  providers: [
    AuthService,
    JwtStrategy,
    JwtRefreshStrategy,
    GoogleStrategy,   // <-- tambahkan
    GithubStrategy,   // <-- tambahkan
  ],
})
export class AuthModule {}
```

---

## Analogi

OAuth 2.0 seperti **pintu masuk mal dengan sistem parkir terpusat**. Anda tidak perlu membuat kartu parkir baru untuk setiap toko. Google/GitHub adalah **pusat parkir** — Anda cukup menunjukkan kartu parkir (akun Google) di pintu utama mal. Petugas (app kita) mengecek kartu Anda ke pusat parkir (Google), lalu memberikan **tanda pengunjung (JWT)** yang berlaku di seluruh toko. Upsert adalah petugas yang **mengecek daftar pengunjung**: kalau sudah tercatat, perbarui data; kalau belum, catat sebagai pengunjung baru.

---

## Dipakai Untuk Apa

- Login sosial (Google, GitHub, Facebook, Apple)
- Mengurangi friction registrasi (tidak perlu isi form panjang)
- Mendapatkan data profil user (nama, email, foto) dari provider
- Aplikasi yang membutuhkan verifikasi email tanpa OTP
- Integrasi dengan layanan pihak ketiga melalui access token OAuth

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Lupa set callback URL di Google Cloud Console | URL harus persis sama dengan di strategy |
| Tidak handle user yang OAuth lalu login pakai password | Buat field `provider` untuk membedakan |
| Tidak upsert, selalu buat user baru | Upsert: cari email dulu, baru buat jika tidak ada |
| Refresh token OAuth dari provider diabaikan | Simpan jika perlu akses API Google/GitHub |
| Redirect token di URL (terlihat di history) | Gunakan fragment (#) atau server-side session |

---

## Soal Latihan

### Soal 1
Buat GoogleStrategy dengan scope email dan profile. Callback: setelah validasi, panggil `done(null, user)`.

### Jawaban 1
```typescript
import { Injectable } from '@nestjs/common';
import { PassportStrategy } from '@nestjs/passport';
import { Strategy, VerifyCallback } from 'passport-google-oauth20';
import { ConfigService } from '@nestjs/config';

@Injectable()
export class GoogleStrategy extends PassportStrategy(Strategy, 'google') {
  constructor(configService: ConfigService) {
    super({
      clientID: configService.get<string>('GOOGLE_CLIENT_ID'),
      clientSecret: configService.get<string>('GOOGLE_CLIENT_SECRET'),
      callbackURL: configService.get<string>('GOOGLE_CALLBACK_URL'),
      scope: ['email', 'profile'],
    });
  }

  async validate(accessToken: string, refreshToken: string, profile: any, done: VerifyCallback) {
    const { name, emails, photos } = profile;
    done(null, {
      email: emails[0].value,
      name: name.givenName + ' ' + name.familyName,
      picture: photos[0].value,
      provider: 'google',
      providerId: profile.id,
    });
  }
}
```

### Soal 2
Implementasikan method `handleOAuthLogin` yang melakukan upsert user dan mengembalikan token pair.

### Jawaban 2
```typescript
async handleOAuthLogin(oauthUser: { email: string; name: string; picture: string; provider: string; providerId: string }) {
  let user = await this.userService.findByEmail(oauthUser.email);

  if (user) {
    await this.userService.update(user.id, {
      name: oauthUser.name,
      picture: oauthUser.picture,
      provider: oauthUser.provider,
      providerId: oauthUser.providerId,
    });
  } else {
    user = await this.userService.create({
      email: oauthUser.email,
      name: oauthUser.name,
      picture: oauthUser.picture,
      provider: oauthUser.provider,
      providerId: oauthUser.providerId,
      password: null,
    });
  }

  const tokens = await this.generateTokens(user.id, user.email);
  const hashedRefresh = await bcrypt.hash(tokens.refreshToken, 12);
  await this.userService.updateRefreshToken(user.id, hashedRefresh);

  return tokens;
}
```

### Soal 3
Apa itu authorization code flow? Sebutkan langkah-langkahnya.

### Jawaban 3
Authorization code flow adalah OAuth flow di mana:
1. User diklik "Login dengan Google"
2. Aplikasi redirect ke Google consent screen
3. User login di Google dan memberikan izin
4. Google redirect kembali ke aplikasi dengan `?code=xxx`
5. Aplikasi menukar code dengan access token (server-to-server)
6. Aplikasi memvalidasi token dan mendapatkan data user
