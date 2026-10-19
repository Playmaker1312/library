# TypeScript Lanjutan — Conditional Types, Mapped Types & Path Alias

## Penjelasan

Setelah kita menguasai **bahan bangunan (types)**, **ruangan (class)**, **lemari arsip (generic)**, **lift (async)**, dan **stiker label (decorator)**, saatnya mempelajari **teknik konstruksi tingkat lanjut**.

Di materi ini kita akan belajar:

- **Conditional Types** — membuat tipe yang berubah tergantung kondisi
- **Mapped Types** — mengubah bentuk tipe secara massal
- **Path Alias** — jalan pintas di proyek agar import tidak berantakan

Ini adalah fondasi yang membuat kita bisa menulis kode NestJS yang **type-safe, rapi, dan scalable**.

## Fungsi

- **Conditional Types** — membuat tipe dinamis seperti ternary di level type
- **Mapped Types** — transformasi tipe massal (setiap properti diubah)
- **Path Alias** — menghindari `../../../` yang berantakan di import

## Cara Pengimplementasian

### Conditional Types

Conditional types menggunakan sintaks `T extends U ? X : Y` — mirip ternary operator tapi di **level tipe**:

```typescript
// Conditional type dasar
type IsString<T> = T extends string ? 'yes' : 'no';

type A = IsString<string>;  // 'yes'
type B = IsString<number>;  // 'no'

// Conditional dalam fungsi
type ExtractId<T> = T extends { id: infer IdType } ? IdType : never;

interface User { id: string; name: string; }
interface Product { id: number; title: string; }
interface Tag { label: string; }

type UserId = ExtractId<User>;     // string
type ProductId = ExtractId<Product>; // number
type TagId = ExtractId<Tag>;       // never
```

### Conditional Types Lanjutan

```typescript
// Infer — mengambil tipe dari dalam struktur
type ReturnOf<T> = T extends (...args: unknown[]) => infer R ? R : never;

type Fn = (x: number) => string;
type FnReturn = ReturnOf<Fn>; // string

// Distributive conditional types
type ToArray<T> = T extends unknown ? T[] : never;

type Result = ToArray<string | number>;
// (string | number)[] — karena distribusi, hasilnya string[] | number[]

// Filter dari union type
type ExtractString<T> = T extends string ? T : never;

type OnlyStrings = ExtractString<string | number | boolean | Date>;
// string — hanya tipe yang extends string yang lolos
```

### Mapped Types

Mapped types memungkinkan kita **mengiterasi properti** suatu tipe dan mengubahnya:

```typescript
type Nullable<T> = {
  [K in keyof T]: T[K] | null;
};

interface User {
  id: string;
  name: string;
  email: string;
}

type NullableUser = Nullable<User>;
// { id: string | null; name: string | null; email: string | null }
```

Mapped type yang lebih kompleks:

```typescript
// Membuat semua properti menjadi opsional
type MyPartial<T> = {
  [K in keyof T]?: T[K];
};

// Membuat semua properti menjadi readonly
type MyReadonly<T> = {
  readonly [K in keyof T]: T[K];
};

// Mengubah tipe value berdasarkan key
type StringifyValues<T> = {
  [K in keyof T]: string;
};

type StringifiedUser = StringifyValues<User>;
// { id: string; name: string; email: string }

// Mapped type dengan filter key
type RemoveId<T> = {
  [K in keyof T as K extends 'id' ? never : K]: T[K];
};

type UserWithoutId = RemoveId<User>;
// { name: string; email: string }
```

### Utility Types Buatan Sendiri

Kombinasi conditional + mapped types:

```typescript
// DeepPartial — membuat semua properti (termasuk nested) partial
type DeepPartial<T> = {
  [K in keyof T]?: T[K] extends object ? DeepPartial<T[K]> : T[K];
};

interface Config {
  database: {
    host: string;
    port: number;
    credentials: {
      username: string;
      password: string;
    };
  };
  server: {
    port: number;
  };
}

type PartialConfig = DeepPartial<Config>;
// Semua field jadi opsional, termasuk nested

// NonNullable — menghapus null dan undefined dari tipe
type MyNonNullable<T> = T extends null | undefined ? never : T;

type MaybeString = string | null | undefined;
type DefinitelyString = MyNonNullable<MaybeString>; // string

// PickByType — memilih properti berdasarkan tipe valuenya
type PickByType<T, ValueType> = {
  [K in keyof T as T[K] extends ValueType ? K : never]: T[K];
};

interface Entity {
  id: string;
  createdAt: Date;
  updatedAt: Date;
  name: string;
  version: number;
}

type DateFields = PickByType<Entity, Date>;
// { createdAt: Date; updatedAt: Date }
```

### Path Alias — Jalan Pintas Import

Path alias menghindari import yang panjang seperti `../../../common/decorators`.

#### Konfigurasi di `tsconfig.json`:

```json
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
      "@modules/*": ["src/modules/*"],
      "@common/*": ["src/common/*"],
      "@config/*": ["src/config/*"],
      "@database/*": ["src/database/*"]
    }
  }
}
```

#### Konfigurasi di `nest-cli.json` (agar NestJS juga paham):

```json
{
  "compilerOptions": {
    "tsConfigPath": "tsconfig.build.json"
  }
}
```

#### Penggunaan:

```typescript
// Sebelum — tanpa path alias
import { UserService } from '../../modules/users/user.service';
import { Logger } from '../../../common/logger';
import { JwtGuard } from '../../../common/guards/jwt.guard';

// Sesudah — dengan path alias
import { UserService } from '@modules/users/user.service';
import { Logger } from '@common/logger';
import { JwtGuard } from '@common/guards/jwt.guard';
```

#### Untuk Build (tsconfig.build.json):

```json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "outDir": "./dist"
  },
  "include": ["src/**/*"]
}
```

#### Path alias untuk Jest:

```json
// package.json atau jest.config
{
  "jest": {
    "moduleNameMapper": {
      "^@modules/(.*)$": "<rootDir>/src/modules/$1",
      "^@common/(.*)$": "<rootDir>/src/common/$1",
      "^@config/(.*)$": "<rootDir>/src/config/$1"
    }
  }
}
```

### Contoh Lengkap: Generic Response Type

Kombinasi semua konsep:

```typescript
// Conditional type untuk response wrapper
type ApiResponse<T> = T extends unknown[]
  ? { data: T; total: number; page: number }
  : { data: T };

// Mapped type untuk membuat semua field Date jadi string
type SerializeDates<T> = {
  [K in keyof T]: T[K] extends Date ? string : T[K];
};

interface User {
  id: string;
  name: string;
  createdAt: Date;
  updatedAt: Date;
}

// Response untuk single user
type UserResponse = ApiResponse<SerializeDates<User>>;
// { data: { id: string; name: string; createdAt: string; updatedAt: string } }

// Response untuk array user
type UsersResponse = ApiResponse<SerializeDates<User>[]>;
// { data: SerializeDates<User>[]; total: number; page: number }
```

## Analogi

Conditional types, mapped types, dan path alias dalam analogi **gedung bertingkat**:

- **Conditional types** = "Jika lantai ini untuk parkir, gunakan material anti-air. Jika untuk kantor, gunakan karpet." Tipe berubah tergantung kondisi.
- **Mapped types (`Nullable<T>`)** = "Semua pintu di lantai ini harus dipasang gagang pintu jenis standar." Satu aturan, diterapkan ke semua properti.
- **Path alias (`@modules/*`)** = papan petunjuk di lobi: "Lift kanan menuju lantai 5-10 (Modul), lift kiri menuju lantai 1-4 (Common)." Tanpa ini, kita harus jalan memutar lewat tangga darurat (`../../../`).

## Dipakai untuk Apa

- **Conditional types** — Membuat tipe response API yang dinamis, generic types untuk query builder
- **Mapped types** — Utility types kustom (`DeepPartial`, `PickByType`), serialization types, DTO transformation
- **Path alias** — Struktur folder NestJS dengan module terisolasi (setiap module punya `@modules/auth/*`, `@modules/users/*`)

## Kesalahan Umum yang Sering Terjadi

1. **Conditional type tidak terdistribusi** — Saat menggunakan generic dengan union, conditional type berdistribusi secara otomatis. Jika tidak diinginkan, bungkus dengan `[T] extends [U]`.
2. **Mapped type tidak handle `readonly` dan `optional` modifier** — Gunakan `-readonly` atau `+readonly` untuk mengontrol.
3. **Path alias tidak di-setup di `jest`** — Test gagal karena Jest tidak tahu cara me-resolve `@modules/*`. Solusi: tambahkan `moduleNameMapper`.
4. **Path alias tidak di-setup di `nest-cli.json`** — Build error karena NestJS build system tidak paham path alias.
5. **`baseUrl` lupa di-set** — Path alias tidak bekerja tanpa `baseUrl`.

## Soal Latihan Beserta Jawaban

### Soal 1
Buat mapped type `Nullable<T>` yang mengubah setiap properti dari T menjadi `T[K] | null`.

**Jawaban:**

```typescript
type Nullable<T> = {
  [K in keyof T]: T[K] | null;
};

interface User {
  id: string;
  name: string;
  email: string;
  age: number;
}

type NullableUser = Nullable<User>;
// { id: string | null; name: string | null; email: string | null; age: number | null; }
```

### Soal 2
Buat conditional type `IsArray<T>` yang mengembalikan `'array'` jika T adalah array, dan `'not_array'` jika bukan.

**Jawaban:**

```typescript
type IsArray<T> = T extends unknown[] ? 'array' : 'not_array';

type Test1 = IsArray<string[]>;    // 'array'
type Test2 = IsArray<number>;      // 'not_array'
type Test3 = IsArray<Array<User>>; // 'array'
```

### Soal 3
Konfigurasikan path alias `@modules/*` dan `@common/*` di `tsconfig.json` untuk aplikasi NestJS dengan struktur folder `src/modules/` dan `src/common/`.

**Jawaban:**

```json
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
      "@modules/*": ["src/modules/*"],
      "@common/*": ["src/common/*"]
    }
  }
}
```

Setelah dikonfigurasi, kode berikut bisa digunakan:

```typescript
// Daripada:
import { AuthModule } from '../../../modules/auth/auth.module';
import { Logger } from '../../../common/logger';

// Jadi:
import { AuthModule } from '@modules/auth/auth.module';
import { Logger } from '@common/logger';
```

### Soal 4
Buat mapped type `ReadonlyDeep<T>` yang membuat semua properti (termasuk nested object) menjadi readonly.

**Jawaban:**

```typescript
type ReadonlyDeep<T> = {
  readonly [K in keyof T]: T[K] extends object
    ? T[K] extends Function
      ? T[K]
      : ReadonlyDeep<T[K]>
    : T[K];
};

interface Config {
  database: {
    host: string;
    port: number;
  };
  server: {
    port: number;
  };
}

type FrozenConfig = ReadonlyDeep<Config>;
// Semua properti di semua level jadi readonly
```
