# 56 - Register & Login - bcrypt, JWT & Secure Token Management

---

## Penjelasan

Setelah AuthModule dirancang dengan arsitektur yang bersih, sekarang kita implementasi fitur paling inti: **Register** dan **Login**. Register membuat user baru dengan password yang di-hash menggunakan bcrypt. Login memvalidasi kredensial, lalu mengeluarkan sepasang token: **access token** (15 menit) dan **refresh token** (7 hari). Refresh token di-hash sebelum disimpan di database agar aman.

---

## Fungsi

- Menerima registrasi user baru dengan email dan password
- Meng-hash password dengan bcrypt (salt rounds 10-12)
- Memvalidasi login (email + password)
- Generate access token (JWT, 15 menit) dan refresh token (JWT, 7 hari)
- Menyimpan hash refresh token di database (bukan plaintext)
- Mengembalikan token pair ke client

---

## Cara Pengimplementasian

### 1. DTO

```typescript
// auth/dto/register.dto.ts
import { IsEmail, IsString, MinLength } from 'class-validator';

export class RegisterDto {
  @IsEmail()
  email: string;

  @IsString()
  @MinLength(8)
  password: string;

  @IsString()
  name: string;
}
```

```typescript
// auth/dto/login.dto.ts
import { IsEmail, IsString } from 'class-validator';

export class LoginDto {
  @IsEmail()
  email: string;

  @IsString()
  password: string;
}
```

### 2. Entity User dengan refresh token hash

```typescript
// user/entities/user.entity.ts
import {
  Entity, PrimaryGeneratedColumn, Column,
  CreateDateColumn, UpdateDateColumn,
} from 'typeorm';

@Entity('users')
export class User {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ unique: true })
  email: string;

  @Column()
  password: string;

  @Column()
  name: string;

  @Column({ nullable: true })
  refreshTokenHash: string | null; // hash dari refresh token

  @CreateDateColumn()
  createdAt: Date;

  @UpdateDateColumn()
  updatedAt: Date;
}
```

### 3. AuthService — Register

```typescript
// auth/auth.service.ts
import {
  Injectable, ConflictException, UnauthorizedException,
} from '@nestjs/common';
import { JwtService } from '@nestjs/jwt';
import { ConfigService } from '@nestjs/config';
import * as bcrypt from 'bcrypt';
import { UserService } from '../user/user.service';
import { RegisterDto } from './dto/register.dto';
import { LoginDto } from './dto/login.dto';

@Injectable()
export class AuthService {
  private readonly saltRounds = 12;

  constructor(
    private readonly userService: UserService,
    private readonly jwtService: JwtService,
    private readonly configService: ConfigService,
  ) {}

  async register(dto: RegisterDto) {
    const existing = await this.userService.findByEmail(dto.email);
    if (existing) {
      throw new ConflictException('Email already registered');
    }

    const hashedPassword = await bcrypt.hash(dto.password, this.saltRounds);
    const user = await this.userService.create({
      email: dto.email,
      password: hashedPassword,
      name: dto.name,
    });

    return { message: 'User registered successfully', userId: user.id };
  }

  async login(dto: LoginDto) {
    const user = await this.userService.findByEmail(dto.email);
    if (!user) {
      throw new UnauthorizedException('Invalid credentials');
    }

    const isPasswordValid = await bcrypt.compare(dto.password, user.password);
    if (!isPasswordValid) {
      throw new UnauthorizedException('Invalid credentials');
    }

    const tokens = await this.generateTokens(user.id, user.email);
    const hashedRefresh = await bcrypt.hash(tokens.refreshToken, this.saltRounds);
    await this.userService.updateRefreshToken(user.id, hashedRefresh);

    return tokens;
  }

  private async generateTokens(userId: string, email: string) {
    const accessToken = this.jwtService.sign(
      { sub: userId, email },
      {
        secret: this.configService.get<string>('JWT_SECRET'),
        expiresIn: '15m',
      },
    );

    const refreshToken = this.jwtService.sign(
      { sub: userId, email },
      {
        secret: this.configService.get<string>('JWT_REFRESH_SECRET'),
        expiresIn: '7d',
      },
    );

    return { accessToken, refreshToken };
  }
}
```

### 4. AuthController — Register & Login

```typescript
// auth/auth.controller.ts
import { Controller, Post, Body, HttpCode, HttpStatus } from '@nestjs/common';
import { AuthService } from './auth.service';
import { RegisterDto } from './dto/register.dto';
import { LoginDto } from './dto/login.dto';

@Controller('auth')
export class AuthController {
  constructor(private readonly authService: AuthService) {}

  @Post('register')
  async register(@Body() dto: RegisterDto) {
    return this.authService.register(dto);
  }

  @Post('login')
  @HttpCode(HttpStatus.OK)
  async login(@Body() dto: LoginDto) {
    return this.authService.login(dto);
  }
}
```

### 5. UserService — updateRefreshToken

```typescript
// user/user.service.ts
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { User } from './entities/user.entity';

@Injectable()
export class UserService {
  constructor(
    @InjectRepository(User)
    private readonly userRepo: Repository<User>,
  ) {}

  async findByEmail(email: string): Promise<User | null> {
    return this.userRepo.findOne({ where: { email } });
  }

  async findById(id: string): Promise<User | null> {
    return this.userRepo.findOne({ where: { id } });
  }

  async create(data: Partial<User>): Promise<User> {
    const user = this.userRepo.create(data);
    return this.userRepo.save(user);
  }

  async updateRefreshToken(userId: string, hash: string): Promise<void> {
    await this.userRepo.update(userId, { refreshTokenHash: hash });
  }
}
```

---

## Analogi

Proses registrasi seperti **membuat KTP baru**: Anda datang ke kantor (POST /register), menyerahkan data diri. Petugas (AuthService) menulis data, lalu menyegelnya dengan **stempel hologram bcrypt** agar tidak bisa dipalsukan. Saat login, Anda menunjukkan KTP (email + password), petugas **memindai hologram** dengan `bcrypt.compare()` — cocok? silakan masuk. Access token adalah **tanda pengunjung** yang berlaku 15 menit. Refresh token adalah **kartu anggota** yang berlaku 7 hari — disimpan di lemari arsip dalam bentuk terenkripsi (hash), bukan plaintext.

---

## Dipakai Untuk Apa

- Registrasi pengguna baru
- Login dengan email dan password
- Mendapatkan token untuk akses API yang dilindungi
- Manajemen session berbasis JWT

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Password disimpan plaintext | Selalu hash dengan bcrypt, minimal salt rounds 10 |
| `bcrypt.compare` salah urutan parameter | `compare(plaintext, hash)` — hash dulu baru plaintext |
| Access token expire terlalu lama (24 jam) | Set maksimal 15-30 menit |
| Refresh token tidak di-hash di database | Hash refresh token sebelum disimpan |
| Tidak handle duplicate email | Gunakan `findByEmail` + `ConflictException` |

---

## Soal Latihan

### Soal 1
Implementasikan method `register` di AuthService yang menerima `RegisterDto`, mengecek email duplikat, hash password dengan salt 12, dan mengembalikan response sukses.

### Jawaban 1
```typescript
async register(dto: RegisterDto) {
  const existing = await this.userService.findByEmail(dto.email);
  if (existing) {
    throw new ConflictException('Email already registered');
  }

  const hashedPassword = await bcrypt.hash(dto.password, 12);
  const user = await this.userService.create({
    email: dto.email,
    password: hashedPassword,
    name: dto.name,
  });

  return { message: 'User registered successfully', userId: user.id };
}
```

### Soal 2
Implementasikan method `login` yang memvalidasi kredensial dan mengembalikan `{ accessToken, refreshToken }`.

### Jawaban 2
```typescript
async login(dto: LoginDto) {
  const user = await this.userService.findByEmail(dto.email);
  if (!user) {
    throw new UnauthorizedException('Invalid credentials');
  }

  const isPasswordValid = await bcrypt.compare(dto.password, user.password);
  if (!isPasswordValid) {
    throw new UnauthorizedException('Invalid credentials');
  }

  const tokens = await this.generateTokens(user.id, user.email);
  const hashedRefresh = await bcrypt.hash(tokens.refreshToken, 12);
  await this.userService.updateRefreshToken(user.id, hashedRefresh);

  return tokens;
}
```

### Soal 3
Apa fungsi `salt rounds` di bcrypt.hash? Mengapa tidak boleh terlalu kecil atau terlalu besar?

### Jawaban 3
Salt rounds menentukan berapa kali algoritma hashing diulang (cost factor). 10 = 2^10 iterasi. Terlalu kecil (<8) mudah dibrute-force. Terlalu besar (>15) memperlambat CPU (buruk untuk UX). 10-12 adalah sweet spot.
