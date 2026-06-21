# 39 - Serialization Interceptor - class-transformer di Level Response

## Penjelasan

Di materi sebelumnya (36 — Response Wrapper), kita membungkus response dengan **format seragam** menggunakan `map`. Sekarang kita bungkus response pada level **isi data** — mengecualikan field sensitif, memilih field yang diekspos, dan mentransformasi struktur data.

NestJS memiliki **ClassSerializerInterceptor** bawaan yang menggunakan **class-transformer** untuk **menserialisasi response** berdasarkan decorator di class DTO. Ini memungkinkan kita:

- **Mengecualikan field** seperti password, refreshToken dari response
- **Memilih field** tertentu untuk grup user tertentu
- **Mentransformasi tipe data** sebelum dikirim

## Fungsi

- **Menyembunyikan field sensitif** — password, token, secret dari response API
- **Virtual field** — field yang dihitung/ditransformasi sebelum dikirim (@Expose dengan transform)
- **Group-based serialization** — response berbeda untuk admin vs user biasa
- **Versioning response** — field tertentu hanya untuk API version tertentu
- **Circular reference handling** — mencegah circular reference di response

## Cara Implementasi

### 1. Install Dependencies

```bash
npm install class-transformer class-validator
```

### 2. Gunakan ClassSerializerInterceptor

```typescript
// src/main.ts
import { NestFactory, Reflector } from '@nestjs/core';
import { AppModule } from './app.module';
import { ClassSerializerInterceptor } from '@nestjs/common';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Daftarkan global
  app.useGlobalInterceptors(new ClassSerializerInterceptor(app.get(Reflector)));

  await app.listen(3000);
}
```

Atau via provider:

```typescript
import { Module } from '@nestjs/common';
import { APP_INTERCEPTOR } from '@nestjs/core';
import { ClassSerializerInterceptor } from '@nestjs/common';

@Module({
  providers: [
    {
      provide: APP_INTERCEPTOR,
      useClass: ClassSerializerInterceptor,
    },
  ],
})
export class AppModule {}
```

### 3. Response DTO dengan `@Exclude` dan `@Expose`

```typescript
// src/users/dto/user-response.dto.ts
import { Exclude, Expose, Transform } from 'class-transformer';

export class UserResponseDto {
  @Expose()
  id: number;

  @Expose()
  email: string;

  @Expose()
  name: string;

  @Exclude() // Field ini tidak akan muncul di response
  password: string;

  @Exclude() // Field ini juga tidak muncul
  refreshToken: string;

  @Expose()
  role: string;

  @Expose()
  createdAt: Date;

  @Expose()
  updatedAt: Date;

  @Expose()
  @Transform(({ value }) => value?.toUpperCase())
  roleDisplay: string;

  constructor(partial: Partial<UserResponseDto>) {
    Object.assign(this, partial);
  }
}
```

### 4. Controller Menggunakan Response DTO

```typescript
// src/users/users.controller.ts
import { Controller, Get, Param, UseInterceptors, ClassSerializerInterceptor } from '@nestjs/common';
import { UsersService } from './users.service';
import { UserResponseDto } from './dto/user-response.dto';

@Controller('users')
@UseInterceptors(ClassSerializerInterceptor) // per-controller
export class UsersController {
  constructor(private usersService: UsersService) {}

  @Get()
  async findAll() {
    const users = await this.usersService.findAll();
    // Transform ke response DTO
    return users.map((user) => new UserResponseDto(user));
  }

  @Get(':id')
  async findOne(@Param('id') id: string) {
    const user = await this.usersService.findById(+id);
    return new UserResponseDto(user);
  }
}
```

**Response:**
```json
{
  "id": 1,
  "email": "user@test.com",
  "name": "Budi",
  "role": "admin",
  "roleDisplay": "ADMIN",
  "createdAt": "2025-01-01T00:00:00.000Z",
  "updatedAt": "2025-01-01T00:00:00.000Z"
}
```

Password dan refreshToken tidak muncul.

### 5. Group-Based Serialization — `@SerializeOptions`

Untuk skenario di mana **admin** melihat field berbeda dengan **user biasa**:

```typescript
// src/users/dto/user-response.dto.ts
import { Exclude, Expose } from 'class-transformer';

export class UserResponseDto {
  @Expose()
  id: number;

  @Expose()
  email: string;

  @Expose()
  name: string;

  @Exclude()
  password: string;

  @Expose({ groups: ['admin'] }) // hanya admin yang lihat
  refreshToken: string;

  @Expose({ groups: ['admin'] }) // hanya admin yang lihat
  lastLoginIp: string;

  @Expose({ groups: ['admin', 'auditor'] })
  loginAttempts: number;

  @Expose()
  role: string;

  constructor(partial: Partial<UserResponseDto>) {
    Object.assign(this, partial);
  }
}
```

```typescript
// Controller
import { SerializeOptions } from '@nestjs/common';

@Controller('users')
@UseInterceptors(ClassSerializerInterceptor)
export class UsersController {
  @Get()
  @SerializeOptions({ groups: ['user'] }) // default group untuk user biasa
  async findAll() {
    const users = await this.usersService.findAll();
    return users.map((user) => new UserResponseDto(user));
  }

  @Get('admin')
  @SerializeOptions({ groups: ['admin'] }) // admin lihat field tambahan
  async findAllAdmin() {
    const users = await this.usersService.findAll();
    return users.map((user) => new UserResponseDto(user));
  }
}
```

### 6. Transformasi dengan `@Transform`

```typescript
import { Transform } from 'class-transformer';

export class UserResponseDto {
  @Expose()
  id: number;

  @Expose()
  @Transform(({ value }) => `***${value.slice(-4)}`) // hanya tampilkan 4 digit terakhir
  phoneNumber: string;

  @Expose()
  @Transform(({ obj }) => `${obj.firstName} ${obj.lastName}`) // gabung field
  fullName: string;

  @Expose()
  @Transform(({ value }) => value?.toISOString())
  createdAt: Date;
}
```

### 7. Serialization untuk Pagination

```typescript
export class PaginatedResponseDto<T> {
  @Expose()
  data: T[];

  @Expose()
  meta: {
    page: number;
    limit: number;
    total: number;
    totalPages: number;
  };
}
```

### 8. Custom Interceptor untuk Serialization (Alternatif)

Jika Anda tidak ingin menggunakan ClassSerializerInterceptor bawaan:

```typescript
// src/common/interceptors/serialize.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
} from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';
import { plainToInstance } from 'class-transformer';

@Injectable()
export class SerializeInterceptor implements NestInterceptor {
  constructor(private dto: any) {}

  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next.handle().pipe(
      map((data) => {
        if (Array.isArray(data)) {
          return data.map((item) => plainToInstance(this.dto, item, {
            excludeExtraneousValues: true,
          }));
        }
        return plainToInstance(this.dto, data, {
          excludeExtraneousValues: true,
        });
      }),
    );
  }
}
```

```typescript
// Di controller
@Get()
@UseInterceptors(new SerializeInterceptor(UserResponseDto))
findAll() {
  return this.usersService.findAll();
}
```

## Analogi — Gedung Bertingkat

Bayangkan **Serialization Interceptor** adalah **Resepsionis yang Menyortir Dokumen** sebelum diberikan ke tamu:

- **Entity asli (User entity)** = **Dokumen lengkap** berisi: Nama, Alamat, No KTP, Password ATM, PIN, Rekening Bank
- **`@Exclude()`** = Resepsionis **menyobek bagian sensitif** — Password ATM, PIN — tidak boleh keluar
- **`@Expose()`** = Resepsionis **menampilkan field tertentu** — Nama, Alamat, No KTP
- **`@Transform()`** = Resepsionis **mengubah format** — "08123456789" jadi "***6789"
- **Groups** = **Tamu biasa** cuma lihat Nama & Alamat. **Admin** lihat semua termasuk No KTP.
- **ClassSerializerInterceptor** = **Resepsionis otomatis** — tanpa harus disuruh, dia selalu sortir dokumen sesuai aturan

Tanpa Serialization = Kita kirim dokumen **lengkap dengan password** ke semua orang — bahaya!

## Dipakai untuk Apa

- **Menyembunyikan password, token, secret** dari response API
- **API untuk role berbeda** — admin vs user biasa melihat field berbeda
- **Versioning API** — response version 1 hanya punya field A, version 2 tambah field B
- **Mobile vs Web API** — mobile butuh response lebih ringan (lebih sedikit field)
- **Menghindari circular reference** — entity yang saling referensi bisa diserialisasi dengan aman

## Kesalahan Umum

1. **Lupa return instance class**: `ClassSerializerInterceptor` bekerja dengan **instance class**, bukan plain object. Controller harus return `new UserResponseDto(user)`, bukan `user` biasa.
2. **Lupa register interceptor**: `ClassSerializerInterceptor` tidak aktif otomatis — harus register di main.ts atau controller.
3. **`@Exclude()` di entity vs DTO**: Lebih aman memiliki **DTO khusus untuk response** daripada menaruh `@Exclude()` langsung di entity. Entity seharusnya entity, DTO untuk response.
4. **`@Transform` membuat field wajib ada**: Jika `@Transform` mengakses `obj.field` yang undefined, bisa error. Beri default value.
5. **Groups tidak di-set**: Jika menggunakan groups, pastikan controller punya `@SerializeOptions({ groups: [...] })` — jika tidak, property dengan `{ groups: ['admin'] }` tidak akan muncul.

## Soal Latihan

**Soal**: Buat `UserResponseDto` yang:
1. Mengekspose: id, email, name, role
2. Mengexclude: password, refreshToken
3. Transformasi: `phoneNumber` — hanya tampilkan 4 digit terakhir
4. Gunakan dengan `ClassSerializerInterceptor` di `UsersController`

<details>
<summary>Jawaban</summary>

```typescript
// src/users/dto/user-response.dto.ts
import { Exclude, Expose, Transform } from 'class-transformer';

export class UserResponseDto {
  @Expose()
  id: number;

  @Expose()
  email: string;

  @Expose()
  name: string;

  @Exclude()
  password: string;

  @Exclude()
  refreshToken: string;

  @Expose()
  role: string;

  @Expose()
  @Transform(({ value }) => `****${value?.slice(-4) || '0000'}`)
  phoneNumber: string;

  constructor(partial: Partial<UserResponseDto>) {
    Object.assign(this, partial);
  }
}
```

```typescript
// src/users/users.controller.ts
import { Controller, Get, Param, UseInterceptors, ClassSerializerInterceptor } from '@nestjs/common';
import { UsersService } from './users.service';
import { UserResponseDto } from './dto/user-response.dto';

@Controller('users')
@UseInterceptors(ClassSerializerInterceptor)
export class UsersController {
  constructor(private usersService: UsersService) {}

  @Get()
  async findAll() {
    const users = await this.usersService.findAll();
    return users.map((user) => new UserResponseDto(user));
  }
}
```

**Response tanpa serialization:**
```json
{ "id": 1, "email": "a@a.com", "name": "Budi", "password": "123456", "refreshToken": "xxx", "role": "admin", "phoneNumber": "08123456789" }
```

**Response dengan serialization:**
```json
{ "id": 1, "email": "a@a.com", "name": "Budi", "role": "admin", "phoneNumber": "****6789" }
```
</details>
