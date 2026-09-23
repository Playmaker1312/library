# Custom Validator dengan class-validator — IsUnique & IsExists

## Penjelasan (Nyambung dari materi sebelumnya)

Decorator `class-validator` bawaan (`@IsString`, `@IsEmail`, dll) hanya memvalidasi **format dan tipe data**. Tapi seringkali kita butuh validasi **business rule** yang melibatkan database:

- "Email ini sudah terdaftar belum?"
- "Apakah categoryId yang dikirim benar-benar ada di tabel Category?"
- "Apakah nomor invoice sudah dipakai?"

Untuk ini kita buat **custom validator** dengan `@ValidatorConstraint`.

---

## Fungsi

- Validasi unique constraint — "field ini harus unik di database"
- Validasi referential integrity — "foreign key ini harus merujuk ke data yang ada"
- Validasi async yang butuh query ke database
- Validasi dengan dependency injection (akses ke Repository/Service)
- Dapat dipakai ulang di banyak DTO

---

## Cara Implementasi & Code

### 1. Struktur Custom Validator

```typescript
import {
  ValidatorConstraint,
  ValidatorConstraintInterface,
  ValidationArguments,
  registerDecorator,
  ValidationOptions,
} from 'class-validator';
import { Injectable } from '@nestjs/common';

@ValidatorConstraint({ name: 'IsUnique', async: true })
@Injectable()
export class IsUniqueConstraint implements ValidatorConstraintInterface {
  async validate(value: any, args: ValidationArguments): Promise<boolean> {
    // args.object → instance DTO
    // args.property → nama field
    // args.constraints → argumen yang dikirim ke decorator
    return true; // atau false jika validasi gagal
  }

  defaultMessage(args: ValidationArguments): string {
    return `${args.property} "${args.value}" sudah digunakan`;
  }
}
```

### 2. @IsUnique — Cek Uniqueness di Database

```typescript
// src/common/validators/is-unique.validator.ts
import {
  ValidatorConstraint,
  ValidatorConstraintInterface,
  ValidationArguments,
  registerDecorator,
  ValidationOptions,
} from 'class-validator';
import { Injectable } from '@nestjs/common';
import { InjectDataSource } from '@nestjs/typeorm';
import { DataSource } from 'typeorm';

@ValidatorConstraint({ name: 'IsUnique', async: true })
@Injectable()
export class IsUniqueConstraint implements ValidatorConstraintInterface {
  constructor(@InjectDataSource() private readonly dataSource: DataSource) {}

  async validate(value: any, args: ValidationArguments): Promise<boolean> {
    const [entityClass] = args.constraints;
    const repository = this.dataSource.getRepository(entityClass);

    const existing = await repository.findOne({
      where: { [args.property]: value },
    });

    return !existing; // true jika tidak ada (valid), false jika ada (invalid)
  }

  defaultMessage(args: ValidationArguments): string {
    return `${args.property} "${args.value}" sudah digunakan`;
  }
}

// Decorator factory
export function IsUnique(
  entityClass: any,
  validationOptions?: ValidationOptions,
) {
  return function (object: any, propertyName: string) {
    registerDecorator({
      target: object.constructor,
      propertyName,
      options: validationOptions,
      constraints: [entityClass],
      validator: IsUniqueConstraint,
    });
  };
}
```

### 3. Register di Module

```typescript
// src/common/common.module.ts
import { Module } from '@nestjs/common';
import { IsUniqueConstraint } from './validators/is-unique.validator';

@Module({
  providers: [IsUniqueConstraint],
  exports: [IsUniqueConstraint],
})
export class CommonModule {}
```

**Dengan TypeORM:**

```typescript
// src/common/validators/is-unique.validator.ts
@ValidatorConstraint({ name: 'IsUnique', async: true })
@Injectable()
export class IsUniqueConstraint implements ValidatorConstraintInterface {
  constructor(
    @InjectRepository(User) private readonly userRepo: Repository<User>,
  ) {}

  async validate(value: any, args: ValidationArguments): Promise<boolean> {
    const [entity] = args.constraints;
    const repo = args.object['getRepository']
      ? args.object['getRepository']()
      : null;

    // Alternatif: gunakan entity manager
    const existing = await this.userRepo.findOne({
      where: { [args.property]: value },
    });
    return !existing;
  }
}
```

### 4. @IsUniqueEmail — Contoh Konkrit

```typescript
// src/common/validators/is-unique-email.validator.ts
import {
  ValidatorConstraint,
  ValidatorConstraintInterface,
  ValidationArguments,
  registerDecorator,
  ValidationOptions,
} from 'class-validator';
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { User } from '../../user/entities/user.entity';

@ValidatorConstraint({ name: 'IsUniqueEmail', async: true })
@Injectable()
export class IsUniqueEmailConstraint implements ValidatorConstraintInterface {
  constructor(
    @InjectRepository(User)
    private readonly userRepository: Repository<User>,
  ) {}

  async validate(email: string, args: ValidationArguments): Promise<boolean> {
    const user = await this.userRepository.findOne({
      where: { email },
      select: ['id'],
    });
    return !user; // true jika email belum dipakai
  }

  defaultMessage(args: ValidationArguments): string {
    return `Email "${args.value}" sudah terdaftar`;
  }
}

export function IsUniqueEmail(validationOptions?: ValidationOptions) {
  return function (object: any, propertyName: string) {
    registerDecorator({
      target: object.constructor,
      propertyName,
      options: validationOptions,
      constraints: [],
      validator: IsUniqueEmailConstraint,
    });
  };
}
```

**Penggunaan di DTO:**

```typescript
// src/user/dto/register-user.dto.ts
import { IsEmail, IsString, MinLength } from 'class-validator';
import { IsUniqueEmail } from '../../common/validators/is-unique-email.validator';

export class RegisterUserDto {
  @IsEmail()
  @IsUniqueEmail({ message: 'Email $value sudah dipakai akun lain' })
  email: string;

  @IsString()
  @MinLength(8)
  password: string;
}
```

### 5. @IsExists — Cek Foreign Key

```typescript
// src/common/validators/is-exists.validator.ts
import {
  ValidatorConstraint,
  ValidatorConstraintInterface,
  ValidationArguments,
  registerDecorator,
  ValidationOptions,
} from 'class-validator';
import { Injectable } from '@nestjs/common';
import { InjectDataSource } from '@nestjs/typeorm';
import { DataSource, EntityTarget } from 'typeorm';

@ValidatorConstraint({ name: 'IsExists', async: true })
@Injectable()
export class IsExistsConstraint implements ValidatorConstraintInterface {
  constructor(@InjectDataSource() private readonly dataSource: DataSource) {}

  async validate(value: any, args: ValidationArguments): Promise<boolean> {
    const [entityClass] = args.constraints;
    const repository = this.dataSource.getRepository(entityClass);

    const existing = await repository.findOne({
      where: { id: value },
      select: ['id'],
    });

    return !!existing; // true jika ada (valid), false jika tidak ada
  }

  defaultMessage(args: ValidationArguments): string {
    return `${args.property} dengan nilai "${args.value}" tidak ditemukan`;
  }
}

export function IsExists(
  entityClass: EntityTarget<any>,
  validationOptions?: ValidationOptions,
) {
  return function (object: any, propertyName: string) {
    registerDecorator({
      target: object.constructor,
      propertyName,
      options: validationOptions,
      constraints: [entityClass],
      validator: IsExistsConstraint,
    });
  };
}
```

**Penggunaan:**

```typescript
// src/product/dto/create-product.dto.ts
import { IsString, IsNumber, IsPositive } from 'class-validator';
import { IsExists } from '../../common/validators/is-exists.validator';
import { Category } from '../../category/entities/category.entity';

export class CreateProductDto {
  @IsString()
  name: string;

  @IsNumber()
  @IsPositive()
  price: number;

  @IsExists(Category, { message: 'Category dengan ID $value tidak ditemukan' })
  categoryId: string;
}
```

### 6. Validator dengan Field Comparison

Kadang kita perlu validasi yang melibatkan field lain, misal: password confirmation.

```typescript
// src/common/validators/match.validator.ts
import {
  ValidatorConstraint,
  ValidatorConstraintInterface,
  ValidationArguments,
  registerDecorator,
  ValidationOptions,
} from 'class-validator';

@ValidatorConstraint({ name: 'Match', async: false })
export class MatchConstraint implements ValidatorConstraintInterface {
  validate(value: any, args: ValidationArguments): boolean {
    const [relatedProperty] = args.constraints;
    const relatedValue = (args.object as any)[relatedProperty];
    return value === relatedValue;
  }

  defaultMessage(args: ValidationArguments): string {
    const [relatedProperty] = args.constraints;
    return `${args.property} harus sama dengan ${relatedProperty}`;
  }
}

export function Match(
  property: string,
  validationOptions?: ValidationOptions,
) {
  return function (object: any, propertyName: string) {
    registerDecorator({
      target: object.constructor,
      propertyName,
      options: validationOptions,
      constraints: [property],
      validator: MatchConstraint,
    });
  };
}
```

**Penggunaan:**

```typescript
export class RegisterUserDto {
  @IsString()
  @MinLength(8)
  password: string;

  @Match('password', { message: 'Konfirmasi password tidak cocok' })
  confirmPassword: string;
}
```

---

## Analogi — Membangun Gedung

| Konsep | Analogi Gedung |
|--------|----------------|
| **ValidatorConstraint** | **Papan nama spesialis**: "Saya khusus cek keunikan alamat" |
| **@IsUnique** | Petugas: "Cek di database, apakah Jalan Merdeka No.1 sudah dipakai proyek lain?" |
| **@IsExists** | Petugas: "Pastikan ID material #123 benar-benar ada di gudang sebelum dipesan" |
| **Async validator** | **Butuh waktu**: petugas harus ke gudang dulu (query DB) untuk cek stok |
| **registerDecorator** | Membuat stempel khusus: "STAMPEL: EMAIL UNIK" yang bisa dipakai di mana saja |
| **Dependency Injection** | Petugas punya akses ke **arsip database** — tidak bisa kerja tanpa data |
| **@Match('password')** | Dua kunci ruangan harus identik sebelum pintu dibuka |

---

## Dipakai Untuk Apa

- **Registrasi user** — cek email unik sebelum akun dibuat
- **Buat produk** — pastikan categoryId merujuk ke kategori yang valid
- **Buat order** — pastikan productId dan customerId ada di database
- **Update profile** — cek keunikan nomor telepon (kecuali milik user yang sedang update)
- **Validasi password confirmation** — `@Match('password')`
- **Cek referensi** — semua foreign key divalidasi sebelum disimpan

---

## Kesalahan Umum

1. **Custom validator tidak terdaftar di module** — error `ValidatorConstraint [...] is not registered`
2. **Async validator tapi lupa inject dependency** — error null pointer karena repository undefined
3. **Validator dipakai di DTO yang tidak diproses ValidationPipe** — validasi tidak jalan
4. **Tidak handle case insensitive** — "User@Email.com" vs "user@email.com" dianggap berbeda
5. **Selector `select: ['id']` diabaikan** — tetap return seluruh kolom (boros query)
6. **Constraint param tidak dikirim** — `@IsUnique(User)` bukan `@IsUnique()`
7. **Validator di-test tanpa database** — perlu setup integration test, bukan unit test biasa

---

## Soal Latihan & Jawaban

### Soal

Buat custom validator `@IsUniqueEmail`:
1. Buat constraint class `IsUniqueEmailConstraint` dengan inject `Repository<User>`
2. Buat decorator `IsUniqueEmail`
3. Gunakan di `RegisterUserDto`
4. Daftarkan di module

### Jawaban

**1. Constraint & Decorator**

```typescript
// src/common/validators/is-unique-email.validator.ts
import {
  ValidatorConstraint,
  ValidatorConstraintInterface,
  ValidationArguments,
  registerDecorator,
  ValidationOptions,
} from 'class-validator';
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { User } from '../../user/entities/user.entity';

@ValidatorConstraint({ name: 'IsUniqueEmail', async: true })
@Injectable()
export class IsUniqueEmailConstraint implements ValidatorConstraintInterface {
  constructor(
    @InjectRepository(User)
    private readonly userRepository: Repository<User>,
  ) {}

  async validate(email: string, args: ValidationArguments): Promise<boolean> {
    const user = await this.userRepository.findOne({
      where: { email },
      select: ['id'],
    });
    return !user;
  }

  defaultMessage(args: ValidationArguments): string {
    return `Email "${args.value}" sudah terdaftar`;
  }
}

export function IsUniqueEmail(validationOptions?: ValidationOptions) {
  return function (object: any, propertyName: string) {
    registerDecorator({
      target: object.constructor,
      propertyName,
      options: validationOptions,
      constraints: [],
      validator: IsUniqueEmailConstraint,
    });
  };
}
```

**2. DTO**

```typescript
// src/user/dto/register-user.dto.ts
import { IsEmail, IsString, MinLength } from 'class-validator';
import { IsUniqueEmail } from '../../common/validators/is-unique-email.validator';

export class RegisterUserDto {
  @IsEmail()
  @IsUniqueEmail({ message: 'Alamat email sudah terdaftar' })
  email: string;

  @IsString()
  @MinLength(8)
  password: string;

  @IsString()
  name: string;
}
```

**3. Module**

```typescript
// src/user/user.module.ts
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { UserController } from './user.controller';
import { UserService } from './user.service';
import { User } from './entities/user.entity';
import { IsUniqueEmailConstraint } from '../common/validators/is-unique-email.validator';

@Module({
  imports: [TypeOrmModule.forFeature([User])],
  controllers: [UserController],
  providers: [UserService, IsUniqueEmailConstraint],
})
export class UserModule {}
```
