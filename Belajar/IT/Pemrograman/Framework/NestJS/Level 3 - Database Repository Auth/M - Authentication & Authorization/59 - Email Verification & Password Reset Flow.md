# 59 - Email Verification & Password Reset Flow

---

## Penjelasan

Setelah user bisa register dan login, kita perlu memastikan email yang didaftarkan benar-benar milik user (email verification). Selain itu, user yang lupa password harus bisa meresetnya melalui email. Kedua fitur ini menggunakan token satu kali (one-time token) yang digenerate dengan `crypto.randomBytes`, di-hash, dan disimpan di database bersama expiry date. Token dikirim via email (transaksional email service seperti Nodemailer, SendGrid, atau Mailgun).

---

## Fungsi

- Membangkitkan token verifikasi email saat register
- Endpoint `POST /auth/verify-email` untuk memverifikasi token
- Mengirim email reset password (forgot password)
- Endpoint `POST /auth/reset-password` untuk mengganti password dengan token

---

## Cara Pengimplementasian

### 1. Entity — Verification & Reset Token

```typescript
// user/entities/user.entity.ts (tambahkan field)
export class User {
  // ... field sebelumnya

  @Column({ nullable: true })
  isEmailVerified: boolean; // default false

  @Column({ nullable: true })
  verificationTokenHash: string | null;

  @Column({ nullable: true })
  verificationTokenExpiry: Date | null;

  @Column({ nullable: true })
  resetTokenHash: string | null;

  @Column({ nullable: true })
  resetTokenExpiry: Date | null;
}
```

### 2. DTOs

```typescript
// auth/dto/verify-email.dto.ts
import { IsString } from 'class-validator';

export class VerifyEmailDto {
  @IsString()
  token: string;
}
```

```typescript
// auth/dto/forgot-password.dto.ts
import { IsEmail } from 'class-validator';

export class ForgotPasswordDto {
  @IsEmail()
  email: string;
}
```

```typescript
// auth/dto/reset-password.dto.ts
import { IsString, MinLength } from 'class-validator';

export class ResetPasswordDto {
  @IsString()
  token: string;

  @IsString()
  @MinLength(8)
  newPassword: string;
}
```

### 3. AuthService — Generate & Verify Token

```typescript
// auth/auth.service.ts (tambahkan)
import * as crypto from 'crypto';

@Injectable()
export class AuthService {
  private readonly tokenExpiryMinutes = 60; // 1 jam

  constructor(
    private readonly userService: UserService,
    private readonly jwtService: JwtService,
    private readonly configService: ConfigService,
    private readonly mailService: MailService, // mailer service
  ) {}

  // === EMAIL VERIFICATION ===

  async sendVerificationEmail(userId: string) {
    const user = await this.userService.findById(userId);
    if (!user) throw new NotFoundException('User not found');

    const { token, hashedToken } = this.generateSecureToken();
    const expiry = new Date(Date.now() + this.tokenExpiryMinutes * 60 * 1000);

    await this.userService.update(userId, {
      verificationTokenHash: hashedToken,
      verificationTokenExpiry: expiry,
    });

    const verificationUrl =
      `${this.configService.get('FRONTEND_URL')}/verify-email?token=${token}`;

    await this.mailService.send({
      to: user.email,
      subject: 'Verify your email address',
      text: `Click this link to verify: ${verificationUrl}`,
    });

    return { message: 'Verification email sent' };
  }

  async verifyEmail(token: string) {
    const users = await this.userService.findAllWithToken();
    // Cari user yang verificationTokenHash cocok dengan token
    for (const user of users) {
      if (!user.verificationTokenHash || !user.verificationTokenExpiry) continue;

      const isMatch = await bcrypt.compare(token, user.verificationTokenHash);
      if (!isMatch) continue;

      if (new Date() > user.verificationTokenExpiry) {
        throw new BadRequestException('Token expired');
      }

      await this.userService.update(user.id, {
        isEmailVerified: true,
        verificationTokenHash: null,
        verificationTokenExpiry: null,
      });

      return { message: 'Email verified successfully' };
    }

    throw new BadRequestException('Invalid token');
  }

  // === PASSWORD RESET ===

  async forgotPassword(dto: ForgotPasswordDto) {
    const user = await this.userService.findByEmail(dto.email);
    if (!user) {
      // Jangan kasih tahu apakah email terdaftar (security)
      return { message: 'If email exists, reset link has been sent' };
    }

    const { token, hashedToken } = this.generateSecureToken();
    const expiry = new Date(Date.now() + this.tokenExpiryMinutes * 60 * 1000);

    await this.userService.update(user.id, {
      resetTokenHash: hashedToken,
      resetTokenExpiry: expiry,
    });

    const resetUrl =
      `${this.configService.get('FRONTEND_URL')}/reset-password?token=${token}`;

    await this.mailService.send({
      to: user.email,
      subject: 'Reset your password',
      text: `Click this link to reset: ${resetUrl}`,
    });

    return { message: 'If email exists, reset link has been sent' };
  }

  async resetPassword(dto: ResetPasswordDto) {
    const users = await this.userService.findAllWithToken();
    for (const user of users) {
      if (!user.resetTokenHash || !user.resetTokenExpiry) continue;

      const isMatch = await bcrypt.compare(dto.token, user.resetTokenHash);
      if (!isMatch) continue;

      if (new Date() > user.resetTokenExpiry) {
        throw new BadRequestException('Token expired');
      }

      const hashedPassword = await bcrypt.hash(dto.newPassword, this.saltRounds);
      await this.userService.update(user.id, {
        password: hashedPassword,
        resetTokenHash: null,
        resetTokenExpiry: null,
      });

      return { message: 'Password reset successfully' };
    }

    throw new BadRequestException('Invalid or expired token');
  }

  // === HELPER ===

  private generateSecureToken() {
    const token = crypto.randomBytes(32).toString('hex'); // 64 karakter hex
    const hashedToken = bcrypt.hashSync(token, 10);
    return { token, hashedToken };
  }
}
```

### 4. UserService — findAllWithToken

```typescript
// user/user.service.ts
async findAllWithToken(): Promise<User[]> {
  return this.userRepo.find({
    where: [
      { verificationTokenHash: Not(IsNull()) },
      { resetTokenHash: Not(IsNull()) },
    ],
  });
}
```

### 5. AuthController — Endpoints

```typescript
// auth/auth.controller.ts (tambahkan)
@Controller('auth')
export class AuthController {
  @Post('register')
  async register(@Body() dto: RegisterDto) {
    const result = await this.authService.register(dto);
    // Kirim email verifikasi setelah register
    await this.authService.sendVerificationEmail(result.userId);
    return result;
  }

  @Post('verify-email')
  @HttpCode(HttpStatus.OK)
  async verifyEmail(@Body() dto: VerifyEmailDto) {
    return this.authService.verifyEmail(dto.token);
  }

  @Post('forgot-password')
  @HttpCode(HttpStatus.OK)
  async forgotPassword(@Body() dto: ForgotPasswordDto) {
    return this.authService.forgotPassword(dto);
  }

  @Post('reset-password')
  @HttpCode(HttpStatus.OK)
  async resetPassword(@Body() dto: ResetPasswordDto) {
    return this.authService.resetPassword(dto);
  }
}
```

### 6. Flow Diagram

```
REGISTER
  Client ──POST /auth/register──► Server
     │                              ├── Buat user (isEmailVerified: false)
     │                              ├── Generate token → hash → simpan
     │                              └── Kirim email: "Verify: /verify-email?token=xxx"
     ◄── { message, userId }

VERIFY EMAIL
  Client ──GET /verify-email?token=xxx──► Server
     │                                      ├── Cari user dengan token hash
     │                                      ├── bcrypt.compare(token, hash)
     │                                      ├── Cek expiry
     │                                      ├── Set isEmailVerified = true
     │                                      └── Hapus token hash
     ◄── "Email verified"

FORGOT PASSWORD
  Client ──POST /auth/forgot-password──► Server
     │   { email }                        ├── Generate token → hash → simpan
     │                                    └── Kirim email: "Reset: /reset-password?token=xxx"
     ◄── "If email exists, link sent"

RESET PASSWORD
  Client ──POST /auth/reset-password──► Server
     │   { token, newPassword }           ├── bcrypt.compare(token, hash)
     │                                    ├── Cek expiry
     │                                    ├── Hash password baru → simpan
     │                                    └── Hapus token hash
     ◄── "Password reset"
```

---

## Analogi

Email verification seperti **kartu anggota yang perlu diaktivasi**. Setelah daftar, Anda dikirimi amplop berisi kode aktivasi (token). Anda harus membawa amplop itu ke kantor (endpoint verify) untuk menempelkan stempel "TERVERIFIKASI" di kartu Anda. Password reset seperti **kunci cadangan** yang disimpan di brankas. Lupa kunci utama? Anda minta kunci cadangan dikirim via kurir (email). Kunci cadangan itu hanya bisa dipakai sekali dan akan rusak setelah 1 jam (expiry).

---

## Dipakai Untuk Apa

- Verifikasi email setelah registrasi
- Reset password lupa
- Konfirmasi tindakan penting (ubah email, hapus akun)
- Mencegah pendaftaran dengan email palsu/temporary

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Token disimpan plaintext di database | Hash token dengan bcrypt sebelum simpan |
| Token tidak punya expiry | Set expiry 30-60 menit, validasi di endpoint |
| Token hanya 8 karakter (mudah ditebak) | Gunakan `crypto.randomBytes(32)` → 64 hex chars |
| Forgot password mengungkap apakah email terdaftar | Selalu kembalikan pesan yang sama terlepas dari hasil |
| Token tidak dihapus setelah dipakai | Set field token hash dan expiry ke null setelah sukses |

---

## Soal Latihan

### Soal 1
Implementasikan method `generateSecureToken` menggunakan `crypto.randomBytes` dan bcrypt.

### Jawaban 1
```typescript
private generateSecureToken() {
  const token = crypto.randomBytes(32).toString('hex');
  const hashedToken = bcrypt.hashSync(token, 10);
  return { token, hashedToken };
}
```

### Soal 2
Implementasikan method `verifyEmail` yang mencari user berdasarkan token hash, validasi expiry, dan menandai email sebagai terverifikasi.

### Jawaban 2
```typescript
async verifyEmail(token: string) {
  const users = await this.userService.findAllWithToken();
  for (const user of users) {
    if (!user.verificationTokenHash || !user.verificationTokenExpiry) continue;
    const isMatch = await bcrypt.compare(token, user.verificationTokenHash);
    if (!isMatch) continue;
    if (new Date() > user.verificationTokenExpiry) {
      throw new BadRequestException('Token expired');
    }
    await this.userService.update(user.id, {
      isEmailVerified: true,
      verificationTokenHash: null,
      verificationTokenExpiry: null,
    });
    return { message: 'Email verified successfully' };
  }
  throw new BadRequestException('Invalid token');
}
```

### Soal 3
Mengapa kita perlu mengembalikan pesan "If email exists, link sent" pada forgot password, bukan "Email found, link sent"?

### Jawaban 3
Untuk mencegah **email enumeration attack**. Jika pesan berbeda untuk email terdaftar vs tidak terdaftar, penyerang bisa menebak email mana yang terdaftar di aplikasi kita.
