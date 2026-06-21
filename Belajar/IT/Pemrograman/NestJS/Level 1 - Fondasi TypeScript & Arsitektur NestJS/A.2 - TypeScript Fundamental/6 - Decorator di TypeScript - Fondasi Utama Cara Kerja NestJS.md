# Decorator di TypeScript — Fondasi Utama Cara Kerja NestJS

## Penjelasan

Sejauh ini kita sudah belajar tentang **bahan bangunan (types)**, **ruangan (class)**, **lemari arsip (generic repository)**, dan **lift (async/await)**. Sekarang kita akan mempelajari **alat yang paling powerful** di toolbelt TypeScript: **Decorator**.

Decorator adalah **mekanisme untuk menambahkan "label" dan "perilaku" ke kode** kita secara deklaratif. Di NestJS, decorator adalah fondasi utama — setiap `@Controller()`, `@Get()`, `@Injectable()`, `@Module()` yang kita lihat adalah decorator.

Tanpa decorator, NestJS tidak akan bisa bekerja dengan cara yang elegan seperti sekarang.

## Fungsi

- **Metaprogramming** — menambahkan metadata ke class, method, properti, atau parameter
- **Deklaratif** — kode lebih bersih dan ekspresif dibanding konfigurasi manual
- **Cross-cutting concerns** — logging, validasi, caching — sekali tulis, dipakai di banyak tempat
- **Fondasi NestJS** — semua decorator NestJS (`@Controller`, `@Get`, `@Injectable`) adalah decorator TypeScript

## Cara Pengimplementasian

### Apa Itu Decorator?

Decorator adalah **fungsi khusus** yang dipanggil saat runtime untuk memodifikasi atau menambahkan metadata ke target (class, method, properti, parameter).

Aktifkan di `tsconfig.json`:

```json
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```

### Reflect-metadata

Decorator bekerja bersama library `reflect-metadata` yang menyimpan metadata sebagai **property tersembunyi**:

```bash
npm install reflect-metadata
```

```typescript
import 'reflect-metadata';

// Menyimpan metadata
Reflect.defineMetadata('role', 'admin', target);

// Membaca metadata
const role = Reflect.getMetadata('role', target);
```

### Jenis-Jenis Decorator

#### 1. Class Decorator

Diterapkan ke class. Bisa memodifikasi constructor atau menambahkan metadata.

```typescript
function Injectable(): ClassDecorator {
  return (target: Function) => {
    Reflect.defineMetadata('injectable', true, target);
  };
}

@Injectable()
class UserService {
  findAll() {
    return [];
  }
}

console.log(Reflect.getMetadata('injectable', UserService)); // true
```

#### 2. Method Decorator

Diterapkan ke method. Bisa memodifikasi perilaku method.

```typescript
function Log(): MethodDecorator {
  return (
    target: Object,
    propertyKey: string | symbol,
    descriptor: PropertyDescriptor
  ) => {
    const originalMethod = descriptor.value;

    descriptor.value = function (...args: unknown[]) {
      console.log(`[LOG] Method ${String(propertyKey)} dipanggil`);
      console.log(`[LOG] Arguments:`, args);
      const result = originalMethod.apply(this, args);
      console.log(`[LOG] Result:`, result);
      return result;
    };

    return descriptor;
  };
}

class Calculator {
  @Log()
  add(a: number, b: number): number {
    return a + b;
  }
}

const calc = new Calculator();
calc.add(3, 4);
// Output:
// [LOG] Method add dipanggil
// [LOG] Arguments: [3, 4]
// [LOG] Result: 7
```

#### 3. Property Decorator

Diterapkan ke properti class. Biasanya untuk validasi atau serialisasi.

```typescript
function MinLength(min: number): PropertyDecorator {
  return (target: Object, propertyKey: string | symbol) => {
    Reflect.defineMetadata(
      'minLength',
      min,
      target,
      propertyKey
    );
  };
}

class User {
  @MinLength(3)
  name: string;
}
```

#### 4. Parameter Decorator

Diterapkan ke parameter method. Digunakan NestJS untuk menandai parameter yang perlu di-inject.

```typescript
function InjectParam(paramName: string): ParameterDecorator {
  return (
    target: Object,
    propertyKey: string | symbol | undefined,
    parameterIndex: number
  ) => {
    Reflect.defineMetadata(
      `param_${propertyKey}_${parameterIndex}`,
      paramName,
      target
    );
  };
}

class UserController {
  findById(
    @InjectParam('userId') id: string
  ) {
    return `Mencari user dengan id ${id}`;
  }
}
```

### Decorator Factory — Decorator dengan Parameter

Decorator factory adalah fungsi yang **mengembalikan decorator**. Ini memungkinkan decorator menerima argumen:

```typescript
function Role(role: 'admin' | 'user'): MethodDecorator {
  return (
    target: Object,
    propertyKey: string | symbol,
    descriptor: PropertyDescriptor
  ) => {
    Reflect.defineMetadata('role', role, target, propertyKey);
  };
}

class DocumentController {
  @Role('admin')
  deleteDocument(id: string) {
    // hanya admin yang bisa menghapus
  }

  @Role('user')
  viewDocument(id: string) {
    // user biasa bisa melihat
  }
}
```

### Decorator @Log() Sederhana

Mari buat decorator `@Log()` yang mencatat nama method dan waktu eksekusi:

```typescript
function Log(): MethodDecorator {
  return (
    target: Object,
    propertyKey: string | symbol,
    descriptor: PropertyDescriptor
  ) => {
    const originalMethod = descriptor.value;

    descriptor.value = function (...args: unknown[]) {
      const start = Date.now();
      console.log(`[${String(propertyKey)}] Start`);

      const result = originalMethod.apply(this, args);

      // Jika method async, tunggu hasilnya
      if (result instanceof Promise) {
        return result.then((res) => {
          const duration = Date.now() - start;
          console.log(`[${String(propertyKey)}] Selesai dalam ${duration}ms`);
          return res;
        });
      }

      const duration = Date.now() - start;
      console.log(`[${String(propertyKey)}] Selesai dalam ${duration}ms`);
      return result;
    };

    return descriptor;
  };
}

class UserService {
  @Log()
  async findUser(id: string): Promise<string> {
    await new Promise(r => setTimeout(r, 100));
    return `User #${id}`;
  }

  @Log()
  greet(name: string): string {
    return `Halo, ${name}`;
  }
}

// Output saat dipanggil:
// [findUser] Start
// [findUser] Selesai dalam 102ms
// [greet] Start
// [greet] Selesai dalam 0ms
```

### Bagaimana NestJS Menggunakan Decorator

NestJS memanfaatkan decorator untuk **membangun metadata routing**:

```typescript
// Analogi sederhana cara kerja NestJS
import 'reflect-metadata';

function Controller(prefix: string): ClassDecorator {
  return (target: Function) => {
    Reflect.defineMetadata('prefix', prefix, target);
  };
}

function Get(path: string): MethodDecorator {
  return (
    target: Object,
    propertyKey: string | symbol,
    descriptor: PropertyDescriptor
  ) => {
    Reflect.defineMetadata('method', 'GET', target, propertyKey);
    Reflect.defineMetadata('path', path, target, propertyKey);
  };
}

class AppModule {
  // NestJS akan membaca metadata ini untuk routing
}

@Controller('/users')
class UserController {
  @Get('/')
  findAll() {
    return ['Budi', 'Ani'];
  }

  @Get('/:id')
  findById(id: string) {
    return { id, name: 'Budi' };
  }
}

// "NestJS runtime" membaca metadata:
const prefix = Reflect.getMetadata('prefix', UserController);
console.log(prefix); // '/users'
```

## Analogi

Decorator dalam analogi **gedung bertingkat**:

- **Decorator** = stiker label yang ditempel di berbagai bagian gedung
- **Class decorator (`@Controller`)** = papan nama di pintu masuk lantai — "Lantai 3: HRD"
- **Method decorator (`@Get`, `@Post`)** = stiker di bel pintu — "Tutup pintu", "Dorong", "Tarik"
- **Property decorator** = label di kotak arsip — "Isi: Dokumen Pribadi, Minimal 3 lembar"
- **Parameter decorator** = label di lubang dokumen — "Masukkan KTP di sini"
- **Reflect-metadata** = catatan kecil di balik stiker yang tidak terlihat kasat mata tapi bisa dibaca petugas

NestJS adalah **gedung yang seluruhnya dibangun dari stiker dan label** — decorator adalah bahasa utama arsitektur NestJS.

## Dipakai untuk Apa

Decorator di NestJS dipakai untuk:

- **Routing** — `@Controller()`, `@Get()`, `@Post()`, `@Put()`, `@Delete()`
- **Dependency Injection** — `@Injectable()`, `@Inject()`
- **Module configuration** — `@Module()`, `@Global()`
- **Parameter extraction** — `@Param()`, `@Query()`, `@Body()`, `@Headers()`
- **Validation** — `@UsePipes()`
- **Guards** — `@UseGuards()`
- **Interceptors** — `@UseInterceptors()`
- **Custom decorator** — menggabungkan beberapa decorator

## Kesalahan Umum yang Sering Terjadi

1. **Lupa `experimentalDecorators` di tsconfig** — Decorator tidak akan bekerja dan muncul error kompilasi.
2. **Lupa import `reflect-metadata`** — `Reflect.defineMetadata` / `getMetadata` tidak dikenali.
3. **Tidak mengembalikan `descriptor` di method decorator** — Method menjadi hilang atau tidak berfungsi.
4. **Async method tidak di-handle di decorator** — Jika method asli adalah async, decorator harus menangani Promise, bukan langsung mengembalikan `originalMethod.apply()`.
5. **Urutan decorator** — Jika beberapa decorator dipasang, urutan eksekusi adalah **bottom-up** untuk dekorator dan **top-down** untuk factory.

## Soal Latihan Beserta Jawaban

### Soal 1
Buat decorator `@Log()` yang mencatat nama method yang dipanggil beserta argumentnya. Terapkan di class `Calculator` dengan method `add` dan `multiply`.

**Jawaban:**

```typescript
function Log(): MethodDecorator {
  return (
    target: Object,
    propertyKey: string | symbol,
    descriptor: PropertyDescriptor
  ) => {
    const originalMethod = descriptor.value;

    descriptor.value = function (...args: unknown[]) {
      console.log(`[LOG] Memanggil ${String(propertyKey)}`);
      console.log(`[LOG] Args:`, args);
      const result = originalMethod.apply(this, args);
      return result;
    };

    return descriptor;
  };
}

class Calculator {
  @Log()
  add(a: number, b: number): number {
    return a + b;
  }

  @Log()
  multiply(a: number, b: number): number {
    return a * b;
  }
}

const calc = new Calculator();
calc.add(5, 3);   // [LOG] Memanggil add | [LOG] Args: [5, 3]
calc.multiply(4, 2); // [LOG] Memanggil multiply | [LOG] Args: [4, 2]
```

### Soal 2
Buat decorator factory `@Timeout(ms)` yang melempar error jika method berjalan lebih dari `ms` milidetik.

**Jawaban:**

```typescript
function Timeout(ms: number): MethodDecorator {
  return (
    target: Object,
    propertyKey: string | symbol,
    descriptor: PropertyDescriptor
  ) => {
    const originalMethod = descriptor.value;

    descriptor.value = function (...args: unknown[]) {
      return new Promise((resolve, reject) => {
        const timer = setTimeout(() => {
          reject(new Error(`${String(propertyKey)} timeout setelah ${ms}ms`));
        }, ms);

        const result = originalMethod.apply(this, args);
        if (result instanceof Promise) {
          result
            .then((res) => {
              clearTimeout(timer);
              resolve(res);
            })
            .catch((err) => {
              clearTimeout(timer);
              reject(err);
            });
        } else {
          clearTimeout(timer);
          resolve(result);
        }
      });
    };

    return descriptor;
  };
}

class SlowService {
  @Timeout(500)
  async slowOperation(): Promise<string> {
    await new Promise(r => setTimeout(r, 1000)); // 1 detik
    return 'Selesai';
  }

  @Timeout(500)
  fastOperation(): string {
    return 'Langsung selesai';
  }
}

// slowOperation() akan reject dengan error timeout
// fastOperation() akan resolve normal
```

### Soal 3
Jelaskan bagaimana NestJS menggunakan decorator `@Controller()` untuk routing.

**Jawaban:** NestJS menggunakan `@Controller(prefix)` yang menempelkan metadata prefix ke class controller via `Reflect.defineMetadata`. Saat aplikasi start, NestJS scanner membaca semua metadata ini, menggabungkan prefix controller dengan path method decorator (`@Get`, `@Post`), lalu mendaftarkannya ke router internal. Tanpa decorator, kita harus mendaftarkan route secara manual satu per satu.
