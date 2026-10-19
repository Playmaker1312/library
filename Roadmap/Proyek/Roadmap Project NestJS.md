# 🗺️ Roadmap NestJS: Komprehensif, Terstruktur & Berbenang Merah

## Filosofi Roadmap

> **"Belajar NestJS seperti membangun gedung bertingkat"** — setiap lantai bertumpu pada lantai di bawahnya. Setiap topik **eksplisit menyebutkan** koneksinya ke topik sebelum dan sesudahnya.

### Prinsip Desain

- **JavaScript-First, TypeScript-Native**: semua contoh konsisten menggunakan TypeScript
- **Benang Merah Eksplisit**: setiap poin terhubung ke poin sebelum/sesudahnya
- **Satu ORM, Kuasai Dalam**: Prisma sebagai pilihan utama
- **Testing Sejak Awal**: bukan fitur opsional di akhir
- **Project Terintegrasi**: setiap poin berkontribusi ke project level

---

## 📋 Peta Benang Merah Keseluruhan

text

```
TYPESCRIPT SOLID
       ↓
ARSITEKTUR NESTJS (Module, DI, Lifecycle)
       ↓
KOMPONEN INTI (Controller → Pipe → Guard → Interceptor → Filter)
       ↓
DATABASE + REPOSITORY PATTERN
       ↓
AUTH + SECURITY
       ↓
FITUR LANJUTAN (Cache, Queue, WebSocket, Email)
       ↓
TESTING (Unit → Integration → E2E)
       ↓
DEVOPS + MONITORING
       ↓
ARSITEKTUR ENTERPRISE (Microservices, GraphQL, Multi-tenant)
```

---

## 🟢 LEVEL 1: FONDASI — TYPESCRIPT & ARSITEKTUR NESTJS (Minggu 1-5)

> **Tema**: _"Memahami mengapa NestJS ada dan bagaimana cara berpikirnya"_  
> **Benang Merah**: TypeScript → Decorator → DI → Arsitektur Modular → Project pertama berjalan  
> **Output**: Project NestJS pertama dengan 1 module, controller, service, dan unit test

---

### A. Setup & TypeScript yang Wajib Dikuasai

> 💡 **Mengapa dimulai TypeScript?** NestJS adalah TypeScript-first framework. Decorator, generics, dan interface bukan sekadar fitur tambahan — mereka adalah DNA NestJS. Tanpa ini, kode NestJS hanya akan "copy-paste" tanpa pemahaman.

text

```
Benang Merah Bagian A:
Environment siap → TypeScript fundamentals → 
Decorator (fondasi NestJS) → Siap masuk arsitektur NestJS
```

**A.1 Setup Environment**

1. `[[1. Setup Environment Lengkap — Node.js LTS, pnpm, NestJS CLI, VS Code & Docker]]`
    
    - Install Node.js LTS, pnpm sebagai package manager
    - Install NestJS CLI global: `npm i -g @nestjs/cli`
    - VS Code + extension esensial: Prettier, ESLint, REST Client, GitLens
    - Docker Desktop: jalankan PostgreSQL dan Redis via Docker Compose
    - Verifikasi semua instalasi berjalan
    - _Micro-exercise_: Jalankan `nest --version` dan `docker ps`, screenshot hasilnya
2. `[[2. Database Client Setup — TablePlus atau DBeaver untuk inspeksi database]]`
    
    - Koneksi ke PostgreSQL yang berjalan di Docker
    - Navigasi database, tabel, dan query sederhana via GUI
    - _Micro-exercise_: Buat database `nestjs_learn` via GUI, buat tabel `test` secara manual

**A.2 TypeScript Fundamental untuk NestJS**

> 💡 **Koneksi ke NestJS**: Setiap konsep TypeScript di bagian ini akan langsung digunakan di NestJS. Decorator → `@Module()`, `@Injectable()`. Generics → Repository pattern. Interface → kontrak antar layer.

3. `[[3. TypeScript Core — Types, Interface, Type Alias & kapan menggunakan masing-masing]]`
    
    - Tipe primitif dan tipe kompleks
    - `interface` vs `type` alias — perbedaan dan kapan gunakan yang mana
    - Union types (`string | number`) dan intersection types (`A & B`)
    - Utility types: `Partial<T>`, `Required<T>`, `Pick<T,K>`, `Omit<T,K>`, `Record<K,V>`, `Readonly<T>`
    - _Micro-exercise_: Definisikan tipe untuk entitas `User`, `Product`, dan `Order` menggunakan interface dan utility types
4. `[[4. TypeScript Class — Access Modifiers, Abstract Class & Generic Class]]`
    
    - Class dan access modifiers: `public`, `private`, `protected`
    - `readonly` property
    - Abstract class dan abstract method
    - Generic class: `class Repository<T>`
    - _Micro-exercise_: Buat `BaseRepository<T>` dengan method `findById`, `findAll`, `save`, `delete`
5. `[[5. Async TypeScript — Promise, async/await & Error Typing]]`
    
    - `Promise<T>` dengan tipe return yang eksplisit
    - `async/await` dengan TypeScript
    - Custom error types menggunakan class
    - `try/catch` dengan typed error
    - _Micro-exercise_: Buat fungsi `fetchUser(id: string): Promise<User>` dengan error handling bertipe
6. `[[6. Decorator di TypeScript — Fondasi Utama Cara Kerja NestJS]]`
    
    - Apa itu decorator: fungsi yang memodifikasi class/method/property
    - Class decorator, method decorator, property decorator, parameter decorator
    - Mengaktifkan `experimentalDecorators` di `tsconfig.json`
    - Metadata reflection: `reflect-metadata` dan `emitDecoratorMetadata`
    - Bagaimana NestJS menggunakan decorator secara internal
    - _Micro-exercise_: Buat decorator `@Log()` sederhana yang mencatat nama method yang dipanggil
7. `[[7. TypeScript Lanjutan — Conditional Types, Mapped Types & Path Alias]]`
    
    - Conditional types: `T extends U ? X : Y`
    - Mapped types: `{ [K in keyof T]: ... }`
    - Path alias di `tsconfig.json`: `@modules/*`, `@common/*`
    - _Micro-exercise_: Buat mapped type `Nullable<T>` yang membuat semua property bisa `null`

---

### B. Arsitektur & Konsep Inti NestJS

> 💡 **Benang Merah ke A**: Decorator yang dipelajari di Poin 6 sekarang terlihat implementasinya di NestJS. `@Module()`, `@Injectable()`, `@Controller()` adalah decorator yang bekerja dengan metadata reflection.

text

```
Benang Merah Bagian B:
Decorator (Poin 6) → Module system → 
Dependency Injection → Request Lifecycle → 
Project pertama yang terstruktur
```

8. `[[8. Filosofi & Arsitektur NestJS — Mengapa Opinionated Framework Penting]]`
    
    - NestJS vs Express biasa: trade-off antara fleksibilitas dan struktur
    - Inspirasi dari Angular: module system, DI, decorator
    - Kapan NestJS cocok dan kapan tidak
    - Overview komponen: Module → Controller → Provider → Guard → Pipe → Interceptor → Filter
    - _Micro-exercise_: Gambarkan diagram arsitektur NestJS dari memory, bandingkan dengan dokumentasi resmi
9. `[[9. Module System — Unit Organisasi Kode dalam NestJS]]`
    
    - Module sebagai "bounded context" dalam aplikasi
    - `@Module()` decorator: `imports`, `controllers`, `providers`, `exports`
    - Root module (`AppModule`) vs feature module
    - Shared module: mengekspor provider agar bisa digunakan module lain
    - Global module: `@Global()` decorator dan kapan digunakan
    - _Micro-exercise_: Buat diagram dependency antar module untuk aplikasi blog sederhana
10. `[[10. Dependency Injection — Jantung NestJS yang Wajib Dipahami Mendalam]]`
    
    - DI: inversion of control, mengapa kita tidak membuat instance sendiri
    - `@Injectable()`: mendaftarkan class sebagai provider
    - Constructor injection: cara NestJS menyuntikkan dependency
    - DI container: bagaimana NestJS membangun dependency graph
    - Provider scope: `DEFAULT` (singleton), `REQUEST` (per-request), `TRANSIENT` (per-injection)
    - _Micro-exercise_: Buat 3 service dengan scope berbeda, observe perbedaan perilakunya
11. `[[11. Custom Provider — useValue, useClass, useFactory & useExisting]]`
    
    - Mengapa custom provider: fleksibilitas dalam mendaftarkan dependency
    - `useValue`: menyuntikkan nilai statis (config object, konstanta)
    - `useClass`: menentukan implementasi class yang digunakan
    - `useFactory`: membuat provider secara dinamis (async factory)
    - `useExisting`: alias untuk provider yang sudah ada
    - `@Inject()` token: injection dengan string atau symbol token
    - _Micro-exercise_: Buat `ConfigProvider` menggunakan `useFactory` yang membaca `.env` file
12. `[[12. NestJS Request Lifecycle — Urutan Eksekusi yang Wajib Dihafalkan]]`
    
    - Urutan lengkap: Middleware → Guard → Interceptor (before) → Pipe → Controller/Handler → Interceptor (after) → Exception Filter
    - Mengapa urutan ini penting untuk debugging
    - Visualisasi flow request-response
    - _Micro-exercise_: Buat diagram lifecycle, tambahkan contoh use case di setiap tahap
13. `[[13. Project NestJS Pertama — Struktur, Konfigurasi & Konvensi]]`
    
    - `nest new project-name`: memahami setiap file yang dibuat
    - `main.ts`: entry point, bootstrap, global configuration
    - `app.module.ts`: root module
    - `nest-cli.json`, `tsconfig.json`, `tsconfig.build.json`
    - `.eslintrc.js` dan `.prettierrc`: code quality dari awal
    - `nest generate` (alias `nest g`): scaffold module, controller, service, dll
    - _Micro-exercise_: Buat project baru, jalankan `nest g resource product`, observasi semua file yang dibuat

---

### C. Controller, Routing & HTTP Fundamentals

> 💡 **Benang Merah ke B**: Module yang dipelajari di Poin 9 mendaftarkan Controller. Request Lifecycle (Poin 12) dimulai dengan Middleware dan berakhir di Controller. Sekarang kita lihat bagaimana Controller bekerja secara detail.

text

```
Benang Merah Bagian C:
Module (Poin 9) → Controller terdaftar di Module →
Route handler menerima request →
Decorator parameter mengekstrak data dari request →
Response dikirim ke client
```

14. `[[14. Controller & Routing — Mendefinisikan Endpoint HTTP]]`
    
    - `@Controller('prefix')`: mendefinisikan prefix route
    - HTTP method decorators: `@Get()`, `@Post()`, `@Put()`, `@Patch()`, `@Delete()`
    - Route parameter: `@Param('id')`, `@Param()` untuk semua params
    - Query string: `@Query('page')`, `@Query()` untuk semua query
    - Request body: `@Body()`, `@Body('field')` untuk field tertentu
    - Headers: `@Headers('authorization')`, `@Headers()`
    - _Micro-exercise_: Buat controller dengan semua HTTP method, test dengan REST Client VS Code
15. `[[15. Response Handling — Status Code, Headers, Redirect & Format Standar]]`
    
    - Default response: NestJS otomatis serialize object ke JSON
    - `@HttpCode(201)`: mengubah status code
    - `@Header('key', 'value')`: menambahkan response header
    - `@Redirect('url', 301)`: redirect
    - Library mode (`@Res()`): akses langsung ke Express/Fastify response — kapan dan mengapa hindari
    - Response format standar: `{ data, message, statusCode, timestamp }`
    - _Micro-exercise_: Implementasikan response format standar untuk semua endpoint controller
16. `[[16. Custom Decorators — Membuat Shortcut untuk Logika yang Berulang]]`
    
    - `createParamDecorator`: membuat parameter decorator kustom
    - `@User()` decorator: mengekstrak user dari request
    - `@CurrentUser()` decorator untuk autentikasi (preview — diimplementasikan di Level 3)
    - Composing decorators: menggabungkan beberapa decorator
    - _Micro-exercise_: Buat `@Pagination()` decorator yang mengekstrak `page` dan `limit` dari query, return sebagai object

---

### D. Testing Sejak Awal — Unit Test untuk Service & Controller

> 💡 **Mengapa Testing di Level 1?** NestJS dirancang untuk testability. `@Injectable()` dan DI bukan hanya untuk production — mereka memudahkan mocking di test. Memulai testing dari awal membangun kebiasaan yang benar.

text

```
Benang Merah Bagian D:
DI (Poin 10) → TestingModule menggantikan DI container di production →
Mock dependency → Test service dan controller →
Confidence saat refactor
```

17. `[[17. Testing Fundamental di NestJS — Jest, TestingModule & Testing Pyramid]]`
    
    - Testing pyramid: unit (banyak) → integration (sedang) → e2e (sedikit)
    - Jest: test runner default NestJS
    - `Test.createTestingModule()`: membuat module khusus testing
    - Perbedaan `.spec.ts` (unit) dan `.e2e-spec.ts` (e2e)
    - Arrange-Act-Assert (AAA) pattern
    - _Micro-exercise_: Tulis 3 unit test untuk fungsi utility pure (tanpa NestJS)
18. `[[18. Unit Test untuk Service — Mocking Dependency & Isolasi]]`
    
    - Mengapa mock: mengisolasi unit yang ditest
    - `jest.fn()`, `jest.spyOn()`: cara membuat mock function
    - Mock provider di `TestingModule`: `{ provide: Service, useValue: mockObject }`
    - Testing semua branch: happy path, error case, edge case
    - _Micro-exercise_: Tulis unit test lengkap untuk service dengan dependency yang di-mock
19. `[[19. Unit Test untuk Controller — Testing HTTP Handler Terisolasi]]`
    
    - Mock service yang diinjeksi ke controller
    - Memverifikasi service dipanggil dengan argumen yang benar
    - Testing response yang dikembalikan controller
    - _Micro-exercise_: Tulis unit test untuk semua method di controller dari Poin 14

---

### 🏗️ Proyek Level 1

text

```
PROYEK: "NestJS Hello World API"
──────────────────────────────────────────────────
Fitur:
├── 1 module (ProductModule)
├── Controller dengan 5 endpoint CRUD (in-memory array)
├── Service dengan logika bisnis terpisah
├── Custom @Pagination() decorator
├── Response format standar di semua endpoint
├── Unit test untuk service dan controller (coverage > 80%)
└── Git repository dengan commit yang deskriptif

Konsep yang dilatih dari Poin 1-19:
TypeScript, Module, DI, Controller, Custom Decorator, 
Response Format, Unit Testing
```

---

## 🔵 LEVEL 2: KOMPONEN INTI — PIPELINE LENGKAP (Minggu 5-10)

> **Tema**: _"Memahami dan menguasai 5 komponen yang membentuk pipeline NestJS"_  
> **Benang Merah**: Request Lifecycle (Poin 12) → Implementasi nyata setiap komponen → Pipeline bekerja bersama → Aplikasi yang robust  
> **Output**: REST API dengan validation, error handling, logging, dan auth guard yang terstruktur

---

### E. Pipe — Validasi & Transformasi Data

> 💡 **Benang Merah ke Request Lifecycle**: Dalam urutan lifecycle (Poin 12), Pipe berjalan SETELAH Guard. Pipe adalah "penjaga data" — memastikan data yang masuk ke handler sudah valid dan dalam format yang benar.

text

```
Benang Merah Bagian E:
Request Lifecycle (Poin 12) → Pipe ada di tahap ke-4 →
DTO mendefinisikan shape data →
class-validator memvalidasi →
class-transformer mengubah tipe →
Handler menerima data yang sudah bersih dan bertipe benar
```

20. `[[20. DTO Pattern — Data Transfer Object sebagai Kontrak Input]]`
    
    - Mengapa DTO: memisahkan bentuk input dari domain model
    - DTO sebagai class (bukan interface) karena butuh decorator runtime
    - Penamaan konvensi: `CreateXxxDto`, `UpdateXxxDto`, `QueryXxxDto`
    - `PartialType`, `PickType`, `OmitType`, `IntersectionType` dari `@nestjs/mapped-types`
    - _Micro-exercise_: Buat `CreateProductDto`, `UpdateProductDto`, dan `QueryProductDto`
21. `[[21. Validasi dengan class-validator — Semua Decorator yang Perlu Dikuasai]]`
    
    - Install: `class-validator` dan `class-transformer`
    - Decorator string: `@IsString()`, `@IsEmail()`, `@IsUrl()`, `@MinLength()`, `@MaxLength()`
    - Decorator number: `@IsNumber()`, `@IsInt()`, `@Min()`, `@Max()`, `@IsPositive()`
    - Decorator boolean dan date: `@IsBoolean()`, `@IsDate()`, `@IsDateString()`
    - Decorator umum: `@IsNotEmpty()`, `@IsOptional()`, `@IsEnum()`, `@IsUUID()`
    - Decorator array: `@IsArray()`, `@ArrayMinSize()`, `@ArrayMaxSize()`
    - Nested object: `@ValidateNested()` + `@Type(() => NestedDto)`
    - Conditional: `@ValidateIf(condition)`
    - Custom message: `@IsString({ message: 'harus berupa string' })`
    - _Micro-exercise_: Buat DTO dengan semua jenis validasi, test dengan data valid dan invalid
22. `[[22. class-transformer — Transformasi & Serialisasi Data]]`
    
    - `@Transform()`: transformasi nilai kustom (lowercase, trim, parse)
    - `@Type()`: konversi tipe otomatis (string → number, string → Date)
    - `@Expose()` dan `@Exclude()`: kontrol properti yang diserialisasi
    - `excludeExtraneousValues`: hanya terima properti yang di-`@Expose()`
    - `@SerializeOptions()` decorator untuk konfigurasi per endpoint
    - _Micro-exercise_: Buat DTO yang secara otomatis trim string, konversi harga ke number, exclude password dari response
23. `[[23. Built-in Pipes & ValidationPipe Global]]`
    
    - Built-in pipes: `ParseIntPipe`, `ParseUUIDPipe`, `ParseBoolPipe`, `DefaultValuePipe`, `ParseArrayPipe`
    - `ValidationPipe` konfigurasi lengkap:
        - `whitelist: true` — hapus properti yang tidak ada di DTO
        - `forbidNonWhitelisted: true` — error jika ada properti asing
        - `transform: true` — transformasi tipe otomatis
        - `transformOptions: { enableImplicitConversion: true }`
    - Mendaftarkan global: `app.useGlobalPipes()`
    - Pipe per route: `@Body(new ValidationPipe())`
    - _Micro-exercise_: Daftarkan ValidationPipe global, test whitelist dan forbidNonWhitelisted
24. `[[24. Custom Pipe — Validasi & Transformasi Kustom]]`
    
    - Implementasi `PipeTransform<T, R>` interface
    - Custom pipe untuk parsing nilai kompleks
    - Custom pipe untuk validasi business rule (bukan format)
    - Error yang dilempar dari pipe: `BadRequestException`
    - _Micro-exercise_: Buat `ParseSortPipe` yang mengubah query `sort=name:asc` menjadi `{ field: 'name', order: 'asc' }`
25. `[[25. Custom Validator dengan class-validator — IsUnique & IsExists]]`
    
    - `@ValidatorConstraint()`: membuat constraint kustom
    - Async validator untuk cek ke database
    - `@IsUnique()`: validasi keunikan email/username
    - `@IsExists()`: validasi keberadaan foreign key
    - Dependency injection dalam custom validator
    - _Micro-exercise_: Buat `@IsUniqueEmail()` yang mengecek ke database apakah email sudah terdaftar

---

### F. Exception Filters — Error Handling Terpusat & Konsisten

> 💡 **Benang Merah ke Request Lifecycle**: Exception Filter adalah "jaring pengaman" terakhir dalam lifecycle. Jika exception tidak ditangkap di handler, Exception Filter yang menangkapnya dan mengubahnya menjadi response yang konsisten.

text

```
Benang Merah Bagian F:
Request Lifecycle (Poin 12) → Exception terjadi di mana saja →
Exception Filter menangkap →
Response error yang konsisten dikirim ke client →
Debugging menjadi lebih mudah
```

26. `[[26. HTTP Exceptions Bawaan NestJS — Semua yang Perlu Diketahui]]`
    
    - Hirarki exception: `HttpException` sebagai base class
    - Exception yang sering digunakan:
        - `BadRequestException` (400)
        - `UnauthorizedException` (401)
        - `ForbiddenException` (403)
        - `NotFoundException` (404)
        - `ConflictException` (409)
        - `UnprocessableEntityException` (422)
        - `InternalServerErrorException` (500)
    - Custom message dan custom response object
    - _Micro-exercise_: Gunakan exception yang tepat untuk 10 skenario error yang berbeda
27. `[[27. Custom Exception Class — Domain-Specific Errors]]`
    
    - Extending `HttpException`: `class ProductNotFoundException extends NotFoundException`
    - Mengapa custom exception: readability, tipe safety, konsistensi
    - Exception untuk business rule violation
    - Menambahkan error code selain HTTP status code
    - _Micro-exercise_: Buat 5 custom exception untuk domain bisnis (misal: `InsufficientStockException`, `OrderAlreadyCancelledException`)
28. `[[28. Global Exception Filter — Response Error yang Standar]]`
    
    - Membuat `AllExceptionsFilter` yang menangkap semua exception
    - Membuat `HttpExceptionFilter` untuk HTTP exception saja
    - Format response error yang konsisten: `{ statusCode, message, error, timestamp, path }`
    - Menangkap Prisma error dan mengubahnya ke HTTP exception
    - Logging error dalam filter
    - Mendaftarkan global: `app.useGlobalFilters()`
    - _Micro-exercise_: Buat global exception filter yang menangkap Prisma error `P2002` (unique constraint) dan ubah ke `ConflictException`
29. `[[29. Exception Filter per Controller & Testing Exception Filter]]`
    
    - `@UseFilters()` pada controller atau method
    - Kapan perlu filter spesifik per controller
    - Unit test untuk exception filter
    - _Micro-exercise_: Tulis unit test untuk global exception filter

---

### G. Guards — Authentication & Authorization

> 💡 **Benang Merah ke Request Lifecycle**: Guard berjalan SEBELUM Pipe dan Handler. Guard menjawab pertanyaan "bolehkah request ini diproses?". Jika tidak — request berhenti di sini dengan `ForbiddenException` atau `UnauthorizedException`.

text

```
Benang Merah Bagian G:
Request Lifecycle (Poin 12) → Guard di tahap ke-2 →
ExecutionContext mengakses request →
Guard return boolean atau throw exception →
Jika false: request ditolak
Jika true: lanjut ke Interceptor → Pipe → Handler
```

30. `[[30. Guard Fundamental — CanActivate & ExecutionContext]]`
    
    - `@Injectable()` + implementasi `CanActivate` interface
    - `ExecutionContext`: cara mengakses request dan response dari guard
    - `context.switchToHttp().getRequest()`: mendapatkan HTTP request
    - Guard return: `boolean | Promise<boolean> | Observable<boolean>`
    - Mendaftarkan: `@UseGuards()` atau `app.useGlobalGuards()`
    - _Micro-exercise_: Buat `ApiKeyGuard` yang memeriksa header `x-api-key`
31. `[[31. Metadata & Reflector — Komunikasi Dekorator ke Guard]]`
    
    - `@SetMetadata('key', value)`: menambahkan metadata ke route
    - Custom decorator menggunakan `SetMetadata`: `@Roles('admin', 'user')`
    - `@Public()` decorator: menandai route yang tidak perlu autentikasi
    - `Reflector.get()` dan `Reflector.getAllAndMerge()`: membaca metadata di guard
    - _Micro-exercise_: Buat `@Public()` decorator dan modifikasi `ApiKeyGuard` untuk skip route yang `@Public()`
32. `[[32. JWT Authentication Guard — Implementasi Lengkap]]`
    
    - Instalasi: `@nestjs/jwt`, `@nestjs/passport`, `passport-jwt`
    - `JwtModule.registerAsync()`: konfigurasi dengan `ConfigService`
    - `JwtStrategy`: memvalidasi dan mendekode JWT token
    - `JwtAuthGuard`: guard yang menggunakan Passport JWT strategy
    - Menyimpan user ke `request.user` setelah validasi sukses
    - `@CurrentUser()` decorator: mengekstrak user dari request (dari Poin 16)
    - _Micro-exercise_: Implementasikan JWT guard, protect semua route kecuali yang `@Public()`
33. `[[33. Role-Based Authorization Guard — RBAC]]`
    
    - `@Roles('admin', 'editor')` decorator
    - `RolesGuard`: membaca roles dari metadata, bandingkan dengan user.roles
    - Urutan guard: `JwtAuthGuard` dulu, baru `RolesGuard`
    - _Micro-exercise_: Buat RBAC guard, test dengan user role `admin`, `editor`, dan `viewer`
34. `[[34. Unit Test untuk Guard & Mocking ExecutionContext]]`
    
    - Membuat mock `ExecutionContext`
    - Testing berbagai skenario: token valid, token expired, token tidak ada, role tidak cukup
    - _Micro-exercise_: Tulis unit test lengkap untuk `JwtAuthGuard` dan `RolesGuard`

---

### H. Interceptors — Transformasi & Cross-Cutting Concerns

> 💡 **Benang Merah ke Request Lifecycle**: Interceptor membungkus eksekusi handler. Mereka berjalan DUA KALI: sebelum handler (request phase) dan setelah handler (response phase). Ini membuat interceptor ideal untuk logging, transformasi response, dan caching.

text

```
Benang Merah Bagian H:
Request Lifecycle (Poin 12) → Interceptor membungkus handler →
RxJS Observable: request → handler → response →
Intercept di kedua arah →
Transform, log, cache, atau timeout
```

35. `[[35. Interceptor Fundamental — NestInterceptor & RxJS Observable]]`
    
    - `NestInterceptor` interface: `intercept(context, next)`
    - `next.handle()`: menjalankan handler, mengembalikan `Observable`
    - RxJS operators untuk memodifikasi response: `map`, `tap`, `catchError`, `timeout`
    - Mendaftarkan: `@UseInterceptors()` atau `app.useGlobalInterceptors()`
    - _Micro-exercise_: Buat interceptor sederhana yang `console.log` sebelum dan sesudah handler
36. `[[36. Response Wrapper Interceptor — Format Response yang Konsisten]]`
    
    - Membungkus semua response dalam format standar: `{ data, message, statusCode, timestamp }`
    - Menggunakan `map(data => ({ data, ... }))` dari RxJS
    - Menangani response yang sudah dalam format yang benar (skip wrapping)
    - _Micro-exercise_: Implementasikan `ResponseWrapperInterceptor`, daftarkan global, verifikasi semua response terbungkus
37. `[[37. Logging Interceptor — Audit Trail Request & Response]]`
    
    - Mencatat: method, URL, status code, durasi, user ID
    - `Date.now()` sebelum handler, hitung durasi di response phase
    - Integrasi dengan NestJS Logger
    - _Micro-exercise_: Buat `LoggingInterceptor` yang mencatat setiap request dengan semua detail di atas
38. `[[38. Timeout Interceptor & Error Handling Interceptor]]`
    
    - `timeout(5000)` dari RxJS: otomatis throw error jika lebih dari 5 detik
    - `catchError`: menangkap error dari handler di interceptor
    - Kapan gunakan error handling di interceptor vs exception filter
    - _Micro-exercise_: Buat `TimeoutInterceptor` yang configurable per route menggunakan metadata
39. `[[39. Serialization Interceptor — class-transformer di Level Response]]`
    
    - `ClassSerializerInterceptor`: menggunakan `@Exclude()` dan `@Expose()` dari class-transformer
    - Entity vs DTO untuk response: menggunakan response DTO
    - `@SerializeOptions({ groups: ['admin'] })`: berbeda serialisasi per role
    - _Micro-exercise_: Buat `UserResponseDto` yang exclude `password` dan `refreshToken` dari response

---

### I. Middleware — Pre-Processing Request

> 💡 **Benang Merah ke Request Lifecycle**: Middleware berjalan PALING AWAL, sebelum Guard. Middleware tidak tahu apakah request akan diterima atau ditolak oleh Guard. Middleware untuk concern global: CORS, compression, logging mentah, dll.

text

```
Benang Merah Bagian I:
Request Lifecycle (Poin 12) → Middleware di tahap pertama →
Akses request dan response (Express-style) →
Memanggil next() untuk lanjut →
Tidak tahu konteks NestJS (modul, service)
```

40. `[[40. Middleware — Perbedaan dengan Guard & Interceptor & Kapan Digunakan]]`
    
    - Middleware: Express-style, tidak akses NestJS DI container secara langsung
    - Kapan middleware: CORS, compression, helmet, rate limiting global, request ID
    - Kapan guard: authentication, authorization
    - Kapan interceptor: transformasi response, logging dengan konteks NestJS
    - Functional middleware vs class middleware
    - `MiddlewareConsumer` + `.forRoutes()` + `.exclude()` di `configure()`
    - _Micro-exercise_: Buat middleware yang menambahkan `X-Request-ID` header ke setiap request
41. `[[41. Security Middleware — Helmet, CORS & Rate Limiting Global]]`
    
    - `helmet`: security headers (X-Frame-Options, XSS Protection, dll)
    - `cors`: konfigurasi origin yang diizinkan, methods, headers
    - `@nestjs/throttler`: rate limiting yang terintegrasi dengan NestJS DI
    - Konfigurasi throttler: `ttl`, `limit`, storage Redis untuk distributed
    - _Micro-exercise_: Setup semua security middleware, konfigurasi CORS hanya untuk domain tertentu
42. `[[42. Request Logging Middleware & Compression]]`
    
    - `morgan` atau custom middleware untuk HTTP access log
    - `compression` middleware untuk gzip response
    - Request ID propagation untuk tracing
    - _Micro-exercise_: Implementasikan request logging yang mencatat method, path, status, dan duration

---

### 🏗️ Proyek Level 2

text

```
PROYEK: "Product Catalog API — Pipeline Lengkap"
──────────────────────────────────────────────────
Enhancement dari Level 1:
├── DTO validation lengkap dengan class-validator
├── Global ValidationPipe, GlobalExceptionFilter, 
│   ResponseWrapperInterceptor, LoggingInterceptor
├── JWT Auth Guard + @Public() decorator
├── RBAC dengan @Roles() decorator
├── Custom exception classes (domain-specific)
├── Security middleware (helmet, cors, throttler)
├── Unit test coverage > 80% (service, controller, guard, pipe)
└── Semua response dalam format standar yang konsisten

Konsep yang dilatih dari Poin 20-42:
Pipe, DTO, Validation, Exception Filter, Guard, 
Interceptor, Middleware, Testing
```

---

## 🟡 LEVEL 3: DATABASE, REPOSITORY PATTERN & AUTH LENGKAP (Minggu 10-18)

> **Tema**: _"Dari data in-memory ke data persisten yang terstruktur dengan arsitektur yang bersih"_  
> **Benang Merah**: Kebutuhan persistensi → Prisma ORM → Repository Pattern → Auth Service lengkap → API yang siap production  
> **Output**: Blog API fullstack dengan auth, database PostgreSQL, dan test integration

---

### J. Prisma ORM — Database Layer yang Type-Safe

> 💡 **Benang Merah ke Sebelumnya**: Di Level 1-2 kita menyimpan data di array. Setiap kali server restart, data hilang. Prisma menjadi jembatan antara TypeScript dan database, dengan type safety penuh.

text

```
Benang Merah Bagian J:
Data in-memory (Level 1-2) → kebutuhan persistensi →
Prisma schema mendefinisikan model →
Prisma migrate mengubah schema ke SQL →
Prisma Client menyediakan type-safe database access →
Service menggunakan Prisma Client
```

43. `[[43. Prisma Setup — Init, Schema, Migrate & Generate]]`
    
    - `npx prisma init`: membuat `schema.prisma` dan `.env`
    - Konfigurasi datasource: `provider` dan `url`
    - `npx prisma migrate dev`: membuat dan menjalankan migration
    - `npx prisma generate`: generate Prisma Client
    - `npx prisma studio`: GUI untuk inspeksi data
    - `npx prisma db seed`: seeding data awal
    - _Micro-exercise_: Init Prisma, buat model `User` dan `Post`, jalankan migration, buka Studio
44. `[[44. Prisma Schema — Model, Field Types, Attributes & Relations]]`
    
    - Field types: `String`, `Int`, `Float`, `Boolean`, `DateTime`, `Json`, `Bytes`
    - Attributes: `@id`, `@default()`, `@unique`, `@map`, `@@map`, `@updatedAt`
    - Auto-generated values: `@default(uuid())`, `@default(cuid())`, `@default(now())`, `@default(autoincrement())`
    - Soft delete: field `deletedAt DateTime?`
    - Relasi one-to-one, one-to-many, many-to-many
    - _Micro-exercise_: Desain schema untuk Blog API: User, Post, Comment, Tag, Category
45. `[[45. PrismaService — Integrasi NestJS yang Benar]]`
    
    - Membuat `PrismaService` yang extends `PrismaClient`
    - Implementasi `OnModuleInit` untuk `$connect()`
    - Implementasi `enableShutdownHooks(app)` untuk graceful shutdown
    - `PrismaModule` sebagai global module
    - _Micro-exercise_: Implementasikan `PrismaService`, buat `PrismaModule` sebagai global module
46. `[[46. Prisma CRUD Operations — Query yang Perlu Dikuasai]]`
    
    - `findMany`: `where`, `orderBy`, `skip`, `take`, `select`, `include`
    - `findUnique` dan `findFirst`: perbedaan dan kapan digunakan
    - `create` dan `createMany`
    - `update` dan `updateMany`
    - `upsert`: create jika tidak ada, update jika ada
    - `delete` dan `deleteMany`
    - `count`, `aggregate`, `groupBy`
    - _Micro-exercise_: Implementasikan semua operasi CRUD untuk `Post` dengan filter, pagination, dan relasi
47. `[[47. Prisma Advanced — Transactions, Raw Query & Middleware]]`
    
    - `prisma.$transaction([...])`: interactive transactions
    - `prisma.$transaction(async (tx) => { ... })`: sequential transaction
    - Optimistic locking pattern dengan Prisma
    - `prisma.$executeRaw` dan `prisma.$queryRaw`: raw SQL
    - Prisma middleware: logging query, soft delete otomatis
    - _Micro-exercise_: Implementasikan transfer kredit antar akun menggunakan transaction (atomic)
48. `[[48. Database Seeding — Data Awal yang Terstruktur]]`
    
    - `prisma/seed.ts`: script seeding
    - Konfigurasi di `package.json`
    - Seed dengan faker-js untuk data realistis
    - Idempotent seed: aman dijalankan berulang
    - _Micro-exercise_: Buat seed script yang membuat 10 user, 50 post, dan 100 comment dengan relasi yang benar

---

### K. Repository Pattern — Arsitektur yang Bersih & Testable

> 💡 **Benang Merah ke Prisma**: Prisma Client sangat powerful, tapi jika digunakan langsung di service, sulit di-test dan coupling tinggi. Repository Pattern membuat lapisan abstraksi antara service (business logic) dan database access.

text

```
Benang Merah Bagian K:
Prisma Client (Poin 43-48) → langsung di service = tightly coupled →
Repository Pattern: service hanya tahu interface →
Repository yang implementasi Prisma →
Testing service: mock repository (tidak perlu database nyata)
Testing repository: integration test dengan database nyata
```

49. `[[49. Repository Pattern — Mengapa & Bagaimana di NestJS]]`
    
    - Masalah tanpa repository: service tightly coupled dengan Prisma
    - Repository sebagai abstraksi atas data access
    - Interface repository: `IUserRepository`, `IPostRepository`
    - Implementasi: `PrismaUserRepository`
    - Mendaftarkan via custom provider: `{ provide: 'IUserRepository', useClass: PrismaUserRepository }`
    - _Micro-exercise_: Buat `IProductRepository` interface dan `PrismaProductRepository`, daftarkan sebagai custom provider
50. `[[50. Generic Base Repository — Menghindari Duplikasi Kode]]`
    
    - `BaseRepository<T, CreateInput, UpdateInput>`: repository generik
    - Method umum: `findById`, `findAll`, `create`, `update`, `delete`, `count`
    - Extend untuk repository spesifik: `UserRepository extends BaseRepository<User, ...>`
    - Trade-off: generic vs specific (kapan masing-masing lebih baik)
    - _Micro-exercise_: Implementasikan `BaseRepository` generik, extend untuk `UserRepository` dan `PostRepository`
51. `[[51. Integration Test untuk Repository — Test dengan Database Nyata]]`
    
    - Mengapa integration test untuk repository: harus test query SQL yang sebenarnya
    - Setup database testing: environment terpisah, migration sebelum test
    - `beforeEach` dan `afterEach`: bersihkan data setelah setiap test
    - Testing semua query method dengan data nyata di database
    - _Micro-exercise_: Tulis integration test lengkap untuk `UserRepository`
52. `[[52. Pagination, Filter & Sort Pattern — Reusable Across Repositories]]`
    
    - `PaginationDto`: `page`, `limit` dengan default value
    - `PaginatedResult<T>`: `{ data, total, page, limit, totalPages }`
    - Generic filter pattern menggunakan Prisma `where` type
    - Sort pattern: `{ field: string, order: 'asc' | 'desc' }`
    - _Micro-exercise_: Implementasikan `PaginationDto`, `PaginatedResult`, dan fungsi `paginate()` yang reusable

---

### L. Configuration & Environment Management

> 💡 **Benang Merah ke DI**: ConfigModule adalah contoh global module (Poin 9). ConfigService diinjeksi ke berbagai service menggunakan DI (Poin 10). Ini adalah pattern yang sama, hanya untuk konfigurasi.

text

```
Benang Merah Bagian L:
DI & Global Module (Poin 9-10) → ConfigModule sebagai contoh nyata →
ConfigService terakses di seluruh aplikasi →
Konfigurasi tervalidasi saat startup →
Environment-specific configuration
```

53. `[[53. ConfigModule — Environment Variables yang Terstruktur & Tervalidasi]]`
    
    - `@nestjs/config`: `ConfigModule.forRoot()` dengan `isGlobal: true`
    - `ConfigService`: mengakses env var dengan type safety
    - Validasi env var saat startup menggunakan Joi: `validationSchema`
    - `registerAs('database', () => ({ ... }))`: namespace configuration
    - Typed configuration dengan interface
    - _Micro-exercise_: Setup ConfigModule dengan validasi Joi untuk semua env var yang wajib ada
54. `[[54. Multiple Environment — Development, Staging, Production]]`
    
    - `.env`, `.env.development`, `.env.staging`, `.env.production`
    - `ConfigModule` dengan `envFilePath` array
    - Secret management: konsep vault (AWS SSM, HashiCorp Vault)
    - Apa yang TIDAK boleh ada di git: credentials, API key, secret
    - _Micro-exercise_: Buat konfigurasi berbeda untuk development dan production, verifikasi behavior berbeda

---

### M. Authentication & Authorization Lengkap

> 💡 **Benang Merah**: JWT Guard (Poin 32) hanya memverifikasi token. Sekarang kita bangun AUTH SERVICE lengkap: register, login, token management, refresh token, dan flow email verification.

text

```
Benang Merah Bagian M:
JWT Guard (Poin 32) → AuthService: logika bisnis auth →
UserRepository (Poin 49-50): akses data user →
bcrypt: hash password →
JWT: generate dan verifikasi token →
Refresh token: session yang bisa diperpanjang →
Email verification: keamanan tambahan
```

55. `[[55. AuthModule — Arsitektur Authentication yang Bersih]]`
    
    - Komponen: `AuthController`, `AuthService`, `JwtStrategy`, `JwtRefreshStrategy`
    - Dependency: `UserModule`, `JwtModule`, `ConfigModule`
    - `JwtModule.registerAsync()` dengan `ConfigService`
    - Alur: register → login → access token + refresh token → refresh → logout
    - _Micro-exercise_: Buat diagram alur authentication, identifikasi setiap komponen yang terlibat
56. `[[56. Register & Login — bcrypt, JWT & Secure Token Management]]`
    
    - Hashing password: `bcrypt.hash()` dengan salt rounds dari config
    - Login: verifikasi password dengan `bcrypt.compare()`
    - Generate access token: payload, secret, expiry (15 menit)
    - Generate refresh token: secret berbeda, expiry panjang (7 hari)
    - Menyimpan refresh token hash di database (bukan plain text)
    - _Micro-exercise_: Implementasikan register dan login dengan semua langkah di atas
57. `[[57. Refresh Token — Perpanjangan Session yang Aman]]`
    
    - Endpoint `/auth/refresh` dengan `JwtRefreshGuard`
    - Token rotation: setiap refresh, refresh token lama diinvalidasi
    - Menyimpan refresh token di `HttpOnly cookie` (lebih aman dari localStorage)
    - Logout: hapus refresh token dari database dan clear cookie
    - _Micro-exercise_: Implementasikan token rotation, verifikasi token lama tidak bisa digunakan lagi
58. `[[58. OAuth 2.0 — Login dengan Google & GitHub]]`
    
    - Konsep OAuth 2.0: authorization code flow
    - `passport-google-oauth20` dan `passport-github2`
    - `GoogleStrategy` dan `GithubStrategy`
    - Menyimpan OAuth user ke database (upsert berdasarkan email)
    - _Micro-exercise_: Implementasikan "Login dengan Google", test flow lengkap
59. `[[59. Email Verification & Password Reset Flow]]`
    
    - Generate token verifikasi: `crypto.randomBytes(32).toString('hex')`
    - Simpan token hash + expiry di database
    - Endpoint `/auth/verify-email?token=...`
    - Forgot password: kirim reset link via email
    - Reset password: verifikasi token, hash password baru, invalidasi token
    - _Micro-exercise_: Implementasikan email verification flow end-to-end
60. `[[60. Permission-Based Authorization dengan CASL]]`
    
    - RBAC vs ABAC (Attribute-Based Access Control)
    - CASL: mendefinisikan ability berdasarkan subject dan action
    - `AbilityFactory`: membuat ability berdasarkan user role
    - `PoliciesGuard`: memeriksa ability menggunakan CASL
    - `@CheckPolicies()` decorator
    - _Micro-exercise_: Implementasikan CASL untuk Post — user hanya bisa edit/delete post miliknya sendiri

---

### N. Swagger — Dokumentasi API Sejak Project Pertama

> 💡 **Mengapa Swagger di Level 3 (bukan Level 4)?** Dokumentasi bukan sesuatu yang ditambahkan di akhir — ini adalah komunikasi. Mendokumentasikan API sejak project awal membangun kebiasaan yang benar dan memudahkan testing.

text

```
Benang Merah Bagian N:
DTO (Poin 20) → @ApiProperty di DTO →
Controller → @ApiOperation, @ApiResponse →
Swagger UI: dokumentasi interaktif →
Test API langsung dari browser
```

61. `[[61. Setup Swagger & Konvensi Dokumentasi]]`
    
    - Install `@nestjs/swagger` dan konfigurasi di `main.ts`
    - `DocumentBuilder`: title, description, version, bearerAuth
    - `@ApiTags()`: mengelompokkan endpoint
    - `@ApiBearerAuth()`: menandai endpoint yang butuh auth
    - Best practice: dokumentasikan SEMUA endpoint, bukan sebagian
    - _Micro-exercise_: Setup Swagger, tambahkan tag dan bearer auth configuration
62. `[[62. Mendokumentasikan DTO, Response & Error dengan Swagger]]`
    
    - `@ApiProperty()` dan `@ApiPropertyOptional()` pada DTO
    - `@ApiResponse({ status: 200, type: ResponseDto })`: mendokumentasikan response sukses
    - `@ApiResponse({ status: 400, description: 'Bad Request' })`: mendokumentasikan error
    - `@ApiOperation({ summary: '...', description: '...' })`: deskripsi endpoint
    - `@ApiParam()`, `@ApiQuery()`, `@ApiBody()`
    - _Micro-exercise_: Dokumentasikan semua endpoint Blog API dengan response dan error yang lengkap

---

### 🏗️ Proyek Level 3

text

```
PROYEK: "Blog API — Full Backend Production-Ready"
──────────────────────────────────────────────────────
Fitur:
├── Schema: User, Post, Comment, Tag, Category (Prisma + PostgreSQL)
├── Auth lengkap: register, login, refresh token, Google OAuth, 
│   email verification, forgot/reset password
├── CRUD Post dengan pagination, filter, sort, search
├── CRUD Comment (nested ke Post)
├── Many-to-many Post ↔ Tag
├── Permission-based auth dengan CASL (hanya author bisa edit post)
├── Image upload untuk post thumbnail (lokal atau S3)
├── Repository Pattern untuk semua entity
├── Swagger dokumentasi lengkap
├── Unit test + Integration test (coverage > 80%)
└── Docker Compose: app + PostgreSQL + Redis

Konsep yang dilatih dari Poin 43-62:
Prisma, Repository Pattern, Auth lengkap, 
CASL, Swagger, Testing, Docker
```

---

## 🟠 LEVEL 4: FITUR CANGGIH — CACHE, QUEUE, REALTIME & EMAIL (Minggu 18-28)

> **Tema**: _"Membangun fitur yang membuat aplikasi bisa diandalkan di skala lebih besar"_  
> **Benang Merah**: Blog API (Level 3) sebagai base → tambahkan fitur satu per satu → setiap fitur ada test → integrasi di Project Level 4  
> **Output**: E-Commerce API lengkap dengan semua fitur production-grade

---

### O. Caching dengan Redis

> 💡 **Benang Merah ke Interceptor**: Caching interceptor (Poin 38) adalah preview. Sekarang kita implementasikan caching yang serius dengan Redis, termasuk cache invalidation strategy.

text

```
Benang Merah Bagian O:
Response lambat karena query database berulang →
Redis sebagai layer cache di depan database →
Cache-aside pattern →
Interceptor untuk caching otomatis →
Manual cache untuk kontrol lebih granular →
Cache invalidation saat data berubah
```

63. `[[63. Redis Setup & CacheModule di NestJS]]`
    
    - Redis: in-memory data store untuk caching, session, queue
    - Install `@nestjs/cache-manager` dan `cache-manager-ioredis-yet`
    - `CacheModule.registerAsync()` dengan Redis config
    - `CACHE_MANAGER` token: inject untuk operasi manual
    - _Micro-exercise_: Setup Redis via Docker, integrasikan CacheModule, verifikasi koneksi
64. `[[64. Cache Strategy — Kapan Cache, Berapa Lama, Apa yang Di-cache]]`
    
    - Cache-aside pattern: aplikasi yang mengurus cache
    - Cache TTL: berapa lama data valid
    - Cache key design: namespace + identifier (misal: `product:123`, `products:list:page1`)
    - Apa yang cocok di-cache: data yang sering dibaca, jarang berubah
    - Apa yang TIDAK di-cache: data real-time, data sensitif per user
    - _Micro-exercise_: Identifikasi 5 endpoint yang cocok di-cache dan 5 yang tidak, berikan alasan
65. `[[65. Implementasi Caching — Dekorator & Manual Cache]]`
    
    - `@CacheKey()` dan `@CacheTTL()` pada route handler
    - `@UseInterceptors(CacheInterceptor)`: caching otomatis
    - Manual: `cacheManager.get(key)`, `cacheManager.set(key, value, ttl)`, `cacheManager.del(key)`
    - Cache invalidation: hapus cache saat data di-update atau delete
    - _Micro-exercise_: Implementasikan caching untuk endpoint `GET /products` dan invalidasi saat ada `POST/PUT/DELETE /products`
66. `[[66. Advanced Caching — Distributed Lock & Cache Warming]]`
    
    - Race condition di caching: dua request bersamaan = double computation
    - Distributed lock dengan Redis: `SETNX` pattern
    - Cache warming: isi cache saat startup sebelum ada request
    - Tag-based invalidation: invalidasi semua cache dengan tag tertentu
    - _Micro-exercise_: Implementasikan distributed lock untuk mencegah thundering herd problem

---

### P. Queue & Background Jobs dengan BullMQ

> 💡 **Benang Merah ke Async**: Di Express/NestJS, request handler harus respond cepat. Operasi berat (kirim email, resize gambar, generate laporan) tidak boleh memblokir response. Queue mengirim tugas ke background worker.

text

```
Benang Merah Bagian P:
Request handler harus respond cepat →
Operasi berat = taruh di queue →
BullMQ: Redis-backed job queue →
Producer: menambah job ke queue →
Consumer/Processor: memproses job di background →
Monitoring: Bull Board untuk visualisasi
```

67. `[[67. BullMQ Fundamentals — Queue, Job & Worker]]`
    
    - Mengapa BullMQ (bukan Bull v3 yang deprecated)
    - Install `@nestjs/bullmq` dan `bullmq`
    - Konsep: Queue (tempat job), Worker (pemroses), Scheduler (penjadwal)
    - `BullModule.registerQueue()`: mendaftarkan queue
    - _Micro-exercise_: Buat queue pertama `email-queue`, tambahkan job sederhana, buat processor
68. `[[68. Job Options — Priority, Delay, Retry & Repeat]]`
    
    - `priority`: job dengan priority tinggi diproses lebih dulu
    - `delay`: tunda eksekusi job beberapa milidetik
    - `attempts` + `backoff`: retry otomatis jika gagal (exponential backoff)
    - `repeat` dengan cron expression: job berulang terjadwal
    - `removeOnComplete` dan `removeOnFail`: manajemen memori
    - _Micro-exercise_: Buat job yang retry 3x dengan exponential backoff, dan job cron tiap malam jam 00:00
69. `[[69. Job Events & Error Handling di Queue]]`
    
    - Events: `active`, `completed`, `failed`, `progress`, `stalled`
    - Handler event di Worker: `onActive`, `onCompleted`, `onFailed`
    - Dead letter queue: job yang gagal setelah semua retry
    - Mencatat error job ke database untuk audit
    - _Micro-exercise_: Implementasikan error handling lengkap, simpan failed job ke database
70. `[[70. Bull Board — Monitoring Queue secara Visual]]`
    
    - Install `@bull-board/nestjs` dan `@bull-board/api`
    - Setup dashboard di NestJS
    - Protect dashboard dengan auth guard
    - Monitoring: job waiting, active, completed, failed
    - _Micro-exercise_: Setup Bull Board, protect dengan admin-only guard
71. `[[71. Queue Use Cases — Email, Image Processing & Notifikasi]]`
    
    - Email queue: kirim email tanpa memblokir response
    - Image processing queue: resize dan optimasi gambar setelah upload
    - Notification queue: kirim push notification atau webhook
    - Flow jobs: chain jobs yang bergantung satu sama lain
    - _Micro-exercise_: Implementasikan email queue yang dipanggil setelah user register

---

### Q. Email & Notifikasi

> 💡 **Benang Merah ke Queue**: Email tidak boleh dikirim secara synchronous di request handler (lambat, bisa gagal). Queue (Poin 67-71) adalah transport ideal untuk email. EmailService menjadi producer, email queue processor yang mengirim.

text

```
Benang Merah Bagian Q:
Kebutuhan kirim email (verify, reset password) →
Email queue (Poin 71) sebagai transport →
Nodemailer/Resend sebagai email sender →
Template HTML yang responsive →
Retry jika gagal
```

72. `[[72. Email Service — Nodemailer & Resend SDK]]`
    
    - Pilihan: Nodemailer (SMTP) atau Resend (API-based, lebih modern)
    - Setup `MailerModule` atau custom `EmailService`
    - HTML template dengan Handlebars atau EJS
    - Test email di development: Mailhog via Docker
    - _Micro-exercise_: Setup Mailhog di Docker Compose, kirim test email, verifikasi di UI Mailhog
73. `[[73. Email Templates & EmailService yang Reusable]]`
    
    - Template: welcome, email verification, password reset, order confirmation
    - HTML email yang responsif (email client berbeda dari browser)
    - `EmailService` dengan method spesifik: `sendVerificationEmail()`, `sendPasswordResetEmail()`
    - _Micro-exercise_: Buat 3 template email dan implementasikan EmailService dengan method untuk masing-masing
74. `[[74. In-App Notification System]]`
    
    - Model database `Notification`: userId, type, message, isRead, metadata
    - Service untuk create, list, mark as read, mark all read
    - Integrasi dengan WebSocket untuk real-time notification (preview Poin 75)
    - _Micro-exercise_: Implementasikan notification system, buat endpoint untuk list dan mark as read

---

### R. WebSocket & Real-time

> 💡 **Benang Merah ke Notification**: In-app notification (Poin 74) lebih powerful dengan WebSocket — user mendapat notifikasi real-time tanpa harus refresh halaman.

text

```
Benang Merah Bagian R:
HTTP: request-response, tidak bisa push dari server →
WebSocket: koneksi persisten, bidirectional →
Gateway: endpoint WebSocket di NestJS →
Auth di WebSocket: JWT di handshake →
Room: grouping client untuk broadcast selektif →
Redis adapter: scaling horizontal
```

75. `[[75. WebSocket Gateway — Dasar Real-time di NestJS]]`
    
    - Install `@nestjs/websockets` dan `@nestjs/platform-socket.io`
    - `@WebSocketGateway()`: membuat gateway
    - `@SubscribeMessage('event')`: menangani pesan dari client
    - `@WebSocketServer()`: akses server instance
    - Mengirim pesan: `socket.emit()`, `server.emit()`, `server.to(room).emit()`
    - _Micro-exercise_: Buat gateway chat sederhana: join room, kirim pesan ke room
76. `[[76. Authentication di WebSocket & Rooms]]`
    
    - JWT verification di WebSocket handshake
    - Middleware Socket.io untuk validasi token
    - Menyimpan user ID di socket data
    - Rooms: join room saat koneksi, leave saat disconnect
    - _Micro-exercise_: Implementasikan auth di WebSocket, tiap user masuk ke room pribadi (user ID sebagai room)
77. `[[77. Real-time Notification — Integrasi WebSocket + Notification Service]]`
    
    - Saat notifikasi dibuat (Poin 74), push ke WebSocket user
    - Menyimpan user's socket ID: Redis untuk multi-instance
    - Redis adapter untuk Socket.io: scaling horizontal
    - _Micro-exercise_: Integrasi Notification Service dengan WebSocket gateway, test notifikasi real-time

---

### S. File Upload & Media Management

> 💡 **Benang Merah ke Queue**: Upload file besar → simpan ke storage (S3/lokal) → proses di background (resize, optimize) menggunakan image processing queue (Poin 71).

text

```
Benang Merah Bagian S:
Kebutuhan upload file →
Multer: middleware untuk multipart/form-data →
Validasi tipe dan ukuran file →
Storage: lokal (development) atau S3 (production) →
Image processing queue untuk optimasi →
MediaModule yang terpusat
```

78. `[[78. File Upload dengan Multer — Single & Multiple Files]]`
    
    - Install `@nestjs/platform-express` (sudah include Multer)
    - `FileInterceptor` dan `FilesInterceptor`
    - `@UploadedFile()` dan `@UploadedFiles()` decorator
    - `fileFilter`: validasi tipe file (hanya image)
    - `limits`: ukuran maksimal file
    - _Micro-exercise_: Endpoint upload avatar dengan validasi: hanya jpg/png/webp, max 2MB
79. `[[79. File Storage — Local & AWS S3]]`
    
    - Local storage: `diskStorage` (development only)
    - AWS S3: `@aws-sdk/client-s3`
    - `S3Service`: upload, getSignedUrl, delete
    - Menyimpan URL file di database
    - Signed URL: akses file private dengan URL sementara
    - _Micro-exercise_: Implementasikan `S3Service`, test upload dan ambil signed URL
80. `[[80. Image Processing — Sharp untuk Optimasi & Resize]]`
    
    - Install `sharp` untuk manipulasi gambar
    - Resize image ke ukuran standar (thumbnail, medium, large)
    - Konversi ke WebP untuk ukuran lebih kecil
    - Integrasi dengan image processing queue (Poin 71)
    - _Micro-exercise_: Buat image processor queue yang resize gambar ke 3 ukuran dan simpan semua ke S3

---

### 🏗️ Proyek Level 4

text

```
PROYEK: "E-Commerce API Lengkap"
─────────────────────────────────────────────────────────
Schema: User, Product, Category, Order, OrderItem, Cart, 
        Review, Address, Notification, Media

Fitur:
├── Auth: semua dari Level 3 + 2FA dengan OTP
├── Product: CRUD + image upload (S3) + image processing queue
├── Cart: guest cart + user cart + merge saat login
├── Order: create dari cart + status flow + email notification
├── Review: rating + komentar + moderasi
├── Caching: product catalog, category list
├── Queue: email, image processing, notifikasi
├── WebSocket: real-time order status update
├── Swagger: dokumentasi lengkap semua endpoint
├── Test: unit + integration + e2e (coverage > 75%)
└── Docker Compose: app + PostgreSQL + Redis + Mailhog + MinIO (S3 lokal)
```

---

## 🔴 LEVEL 5: TESTING KOMPREHENSIF & CLEAN ARCHITECTURE (Minggu 28-36)

> **Tema**: _"Dari kode yang bekerja ke kode yang bisa dipercaya dan dipelihara"_  
> **Benang Merah**: Project e-commerce (Level 4) → Audit testing → Lengkapi test coverage → Clean Architecture → Refactor  
> **Output**: Project dengan test coverage > 85%, arsitektur bersih, CI pipeline

---

### T. Testing Strategy yang Komprehensif

> 💡 **Benang Merah**: Testing sudah dimulai dari Level 1. Sekarang kita bangun testing yang sistematis — mulai dari strategy, contract testing, hingga load testing.

text

```
Benang Merah Bagian T:
Unit test (Level 1-2) → Integration test (Level 3) →
E2E test → Testing pyramid yang seimbang →
CI pipeline otomatis test
```

81. `[[81. Testing Strategy — Piramida, Konfigurasi & Coverage Target]]`
    
    - Testing pyramid revisited: berapa banyak tiap jenis
    - Jest konfigurasi: coverage thresholds, collect coverage dari semua file
    - Testcontainers: jalankan PostgreSQL dan Redis nyata di test
    - Factory functions dengan faker-js: buat test data yang konsisten
    - _Micro-exercise_: Setup Testcontainers untuk integration test, buat 5 factory function untuk entity utama
82. `[[82. Unit Testing Lanjutan — Mocking Strategy & Edge Cases]]`
    
    - Mocking Prisma: `jest-mock-extended` untuk type-safe mock
    - Spy vs Mock vs Stub: kapan gunakan masing-masing
    - Testing edge case: null, undefined, empty array, extreme values
    - Snapshot testing: untuk response format
    - _Micro-exercise_: Lengkapi unit test semua service di project e-commerce, pastikan semua branch ter-cover
83. `[[83. Integration Test untuk API Endpoint — Supertest]]`
    
    - Setup e2e test dengan Supertest dan NestJS testing module
    - Test database nyata dengan Testcontainers
    - Seed data sebelum test, cleanup sesudahnya
    - Testing auth flow: register → login → access protected endpoint
    - _Micro-exercise_: Tulis integration test untuk auth flow dan product CRUD dengan database nyata
84. `[[84. E2E Testing — Skenario User Journey Lengkap]]`
    
    - E2E test: simulasi user journey dari awal hingga akhir
    - Skenario: user register → verify email → login → beli produk → order terbuat
    - Testing WebSocket dengan Socket.io client di test
    - _Micro-exercise_: Tulis E2E test untuk flow checkout: tambah ke cart → buat order → verifikasi email dikirim
85. `[[85. CI Pipeline — Otomatis Test di Setiap Push]]`
    
    - GitHub Actions workflow untuk NestJS
    - Jobs: lint → unit test → integration test → e2e test → build
    - Menjalankan database service di GitHub Actions
    - Coverage report ke Codecov atau GitHub
    - Fail pipeline jika coverage di bawah threshold
    - _Micro-exercise_: Setup GitHub Actions pipeline lengkap, push dan verifikasi semua job hijau

---

### U. Clean Architecture & Advanced Patterns

> 💡 **Benang Merah ke Repository Pattern**: Repository Pattern (Poin 49-51) adalah langkah pertama clean architecture. Sekarang kita formalisasikan: layering yang jelas, dependency rules, dan pattern yang membuat codebase mudah di-maintain.

text

```
Benang Merah Bagian U:
Repository Pattern (Poin 49-51) →
Layering formal: Domain → Application → Infrastructure →
Dependency rules: inner layer tidak tahu outer layer →
CQRS: pisahkan read dan write →
Event-driven untuk decoupling
```

86. `[[86. Clean Architecture di NestJS — Layering & Dependency Rules]]`
    
    - Domain layer: entity, value object, domain event, interface
    - Application layer: use case / command handler, DTO
    - Infrastructure layer: Prisma repository, email service, S3
    - Presentation layer: controller, gateway
    - Dependency rule: infrastructure → application → domain (tidak boleh terbalik)
    - _Micro-exercise_: Refactor satu module e-commerce ke clean architecture
87. `[[87. CQRS Pattern — Memisahkan Read dan Write]]`
    
    - Mengapa CQRS: read dan write punya kebutuhan berbeda
    - `@nestjs/cqrs`: `CommandBus`, `QueryBus`, `EventBus`
    - Command: mengubah state (`CreateOrderCommand`)
    - Query: membaca state (`GetOrderByIdQuery`)
    - Event: sesuatu yang sudah terjadi (`OrderCreatedEvent`)
    - _Micro-exercise_: Implementasikan CQRS untuk `OrderModule`
88. `[[88. Domain Events — Decoupling Antar Module]]`
    
    - Domain event: sesuatu yang terjadi di domain (`OrderPlaced`, `PaymentReceived`)
    - Event handler: bereaksi terhadap event (kirim email, update inventory)
    - Decoupling: `OrderModule` tidak perlu tahu tentang `EmailModule`
    - _Micro-exercise_: Implementasikan `OrderPlacedEvent` yang memicu email konfirmasi dan update inventory
89. `[[89. API Versioning — Backward Compatibility]]`
    
    - URI versioning: `/api/v1/products`, `/api/v2/products`
    - Header versioning: `Accept-Version: 1`
    - NestJS built-in versioning support
    - Strategi: kapan buat versi baru, kapan deprecate yang lama
    - _Micro-exercise_: Tambahkan versioning ke project, buat v2 endpoint yang berbeda response format

---

### 🏗️ Proyek Level 5

text

```
PROYEK: "E-Commerce API — Production Quality"
─────────────────────────────────────────────────────────
Enhancement dari Level 4:
├── Refactor ke Clean Architecture (setidaknya 2 module)
├── CQRS untuk Order dan Payment module
├── Domain Events untuk decoupling
├── API Versioning (v1 dan v2)
├── Test coverage > 85% (unit + integration + e2e)
├── CI/CD pipeline: GitHub Actions → build → test → deploy
├── Performance testing dengan k6
└── Audit security dengan npm audit dan manual review
```

---

## ⚫ LEVEL 6: DEVOPS, MONITORING & ARSITEKTUR ENTERPRISE (Minggu 36-52+)

> **Tema**: _"Dari aplikasi yang berjalan di laptop ke aplikasi yang berjalan di production"_  
> **Benang Merah**: Project production-ready (Level 5) → Container → Deploy → Monitor → Scale → Microservices  
> **Output**: Aplikasi di-deploy di cloud dengan monitoring, logging, dan pipeline otomatis

---

### V. Docker & Deployment

> 💡 **Benang Merah**: Docker Compose sudah digunakan sejak Level 3 untuk development. Sekarang kita buat Docker image untuk production dan deploy ke server nyata.

90. `[[90. Dockerfile Optimal untuk NestJS — Multi-Stage Build]]`
    
    - Multi-stage: `builder` stage (kompilasi TS) → `runner` stage (hanya JS)
    - Image yang kecil: `node:20-alpine` sebagai base
    - Non-root user untuk security
    - `.dockerignore`: exclude `node_modules`, `.git`, dll
    - _Micro-exercise_: Buat Dockerfile multi-stage, bandingkan ukuran image dengan single-stage
91. `[[91. Docker Compose Production — Nginx, SSL & Semua Service]]`
    
    - `docker-compose.prod.yml`: production configuration
    - Nginx sebagai reverse proxy: SSL termination, load balancing
    - Let's Encrypt + Certbot: SSL gratis otomatis
    - Health check untuk setiap service
    - _Micro-exercise_: Setup Docker Compose production dengan Nginx dan SSL
92. `[[92. CI/CD Pipeline Lengkap — GitHub Actions ke VPS]]`
    
    - Workflow: push → test → build Docker image → push ke registry → deploy ke VPS
    - GitHub Container Registry atau Docker Hub
    - SSH ke VPS dan pull image terbaru
    - Zero-downtime deployment: `docker compose up -d --no-deps service`
    - Rollback strategy: revert ke image sebelumnya
    - _Micro-exercise_: Setup pipeline lengkap, push ke main dan verifikasi deploy otomatis ke VPS
93. `[[93. Health Checks & Graceful Shutdown]]`
    
    - `@nestjs/terminus`: health check endpoint
    - Custom health indicator: database, Redis, disk space
    - `app.enableShutdownHooks()`: graceful shutdown
    - SIGTERM handling: selesaikan request yang sedang berjalan
    - _Micro-exercise_: Implementasikan health check yang memeriksa PostgreSQL, Redis, dan disk space

---

### W. Monitoring, Logging & Observability

94. `[[94. Structured Logging — Winston & Correlation ID]]`
    
    - Structured logging: JSON format untuk parsing mudah
    - Winston setup dengan transport: Console (dev) dan File (prod)
    - Correlation ID: trace request dari log ke log
    - Log levels: `error`, `warn`, `info`, `debug`
    - _Micro-exercise_: Setup Winston dengan correlation ID, semua log include request ID
95. `[[95. Distributed Tracing — OpenTelemetry]]`
    
    - Observability: metrics, logs, traces (tiga pilar)
    - OpenTelemetry: standar industri untuk instrumentasi
    - Auto-instrumentation: HTTP, database query ter-trace otomatis
    - Jaeger atau Tempo: backend untuk menyimpan trace
    - _Micro-exercise_: Setup OpenTelemetry, trace request dari HTTP masuk sampai database query
96. `[[96. Metrics dengan Prometheus & Dashboard Grafana]]`
    
    - `prom-client`: expose metrics endpoint `/metrics`
    - Default metrics: CPU, memory, event loop lag
    - Custom metrics: request count, order total, active user
    - Grafana dashboard: visualisasi metrics
    - Alerting: notifikasi jika error rate > threshold
    - _Micro-exercise_: Setup Prometheus + Grafana, buat dashboard untuk metrics utama

---

### X. Performance Optimization

97. `[[97. Fastify Adapter — Performa Lebih Tinggi dari Express]]`
    
    - Install `@nestjs/platform-fastify`
    - Trade-off: Fastify lebih cepat, tapi tidak semua middleware Express kompatibel
    - Benchmark: Express vs Fastify untuk project yang ada
    - _Micro-exercise_: Ganti adapter ke Fastify, jalankan benchmark, bandingkan throughput
98. `[[98. Database Optimization — Query, Index & Connection Pool]]`
    
    - N+1 problem: identify dengan `prisma.$on('query', ...)` logging
    - Solve N+1: `include` yang tepat atau DataLoader pattern
    - Database indexing: kapan buat index, trade-off
    - Connection pooling: Prisma Data Proxy atau PgBouncer
    - _Micro-exercise_: Identifikasi N+1 problem di project, fix dengan proper include atau DataLoader
99. `[[99. Load Testing — k6 untuk Identifikasi Bottleneck]]`
    
    - k6: tool load testing berbasis JavaScript
    - Skenario: ramp up, soak test, stress test
    - Metrics: response time, throughput, error rate
    - Mengidentifikasi bottleneck: database? CPU? memory?
    - _Micro-exercise_: Jalankan load test 100 virtual user selama 5 menit, identifikasi bottleneck

---

### Y. Microservices dengan NestJS

> 💡 **Mengapa Microservices di Level 6?** Microservices bukan solusi untuk semua masalah. Pahami monolith dengan baik terlebih dulu, baru pecah menjadi services jika ada alasan yang jelas.

text

```
Benang Merah Bagian Y:
Monolith yang sudah besar → kebutuhan scale independently →
Transport layer: message broker →
Service communication: sync (TCP/gRPC) vs async (event) →
API Gateway: single entry point →
Distributed challenges: eventual consistency, saga
```

100. `[[100. Microservices Fundamentals — Kapan & Mengapa (Bukan Default!)]]`
    
    - Microservices vs monolith: trade-off yang jujur
    - Kapan microservices masuk akal: team besar, service yang berbeda skala
    - NestJS transport layer: TCP, Redis, RabbitMQ, Kafka, gRPC
    - _Micro-exercise_: Analisis project e-commerce — service mana yang cocok dipisah? Berikan justifikasi
101. `[[101. Membuat Microservice — Transport TCP & RabbitMQ]]`
    
    - `NestFactory.createMicroservice()`: membuat microservice
    - `@MessagePattern()`: menangani request-response
    - `@EventPattern()`: menangani async event
    - `ClientProxy`: berkomunikasi dari service ke service
    - RabbitMQ sebagai message broker
    - _Micro-exercise_: Pisahkan Email Service menjadi microservice terpisah, kommunikasi via RabbitMQ
102. `[[102. API Gateway & Service Discovery]]`
    
    - API Gateway: single entry point untuk semua microservice
    - Routing request ke service yang tepat
    - Authentication di gateway level
    - Service registry: bagaimana service menemukan satu sama lain
    - _Micro-exercise_: Buat API Gateway NestJS yang route ke beberapa microservice
103. `[[103. Distributed Patterns — Saga & Circuit Breaker]]`
    
    - Saga pattern: distributed transaction yang eventual consistent
    - Choreography vs Orchestration saga
    - Circuit breaker: mencegah cascade failure
    - _Micro-exercise_: Implementasikan saga untuk flow order: reserve stock → process payment → confirm order

---

### Z. GraphQL dengan NestJS

104. `[[104. GraphQL Setup — Code-First Approach]]`
    
    - GraphQL vs REST: kapan GraphQL lebih baik
    - Code-first: TypeScript class → GraphQL schema (bukan schema-first)
    - Install `@nestjs/graphql`, `@nestjs/apollo`, `graphql`
    - `GraphQLModule.forRoot()` konfigurasi
    - _Micro-exercise_: Setup GraphQL, buat resolver pertama untuk `User`
105. `[[105. Types, Resolvers & Args — Core GraphQL di NestJS]]`
    
    - `@ObjectType()`, `@Field()`: mendefinisikan tipe GraphQL
    - `@Resolver()`: class yang menangani query/mutation
    - `@Query()`, `@Mutation()`, `@Subscription()`
    - `@Args()`: mengakses argument, `@InputType()` untuk input
    - _Micro-exercise_: Implementasikan CRUD penuh untuk `Post` via GraphQL
106. `[[106. N+1 Problem & DataLoader di GraphQL]]`
    
    - N+1: query beranak di GraphQL (field resolver dipanggil N kali)
    - DataLoader: batch dan cache query
    - `nestjs-dataloader`: integrasi DataLoader dengan NestJS
    - _Micro-exercise_: Identifikasi N+1 di resolver, fix dengan DataLoader
107. `[[107. Auth & Permission di GraphQL + Federation]]`
    
    - Guard di GraphQL resolver
    - Context: mengakses request dan user dari resolver
    - GraphQL Federation: composing multiple GraphQL service
    - _Micro-exercise_: Implementasikan auth guard di GraphQL resolver, test dengan introspection disabled di production

---

### 🏗️ Proyek Level 6 — Capstone

text

```
PROYEK: "Platform SaaS Multi-Tenant — Enterprise Grade"
─────────────────────────────────────────────────────────────
Pilih salah satu atau kombinasi:

OPSI A: E-Commerce Platform Lengkap
├── Multi-tenant: setiap merchant punya isolated data
├── Stripe integration: subscription + usage-based billing
├── Microservices: Product Service, Order Service, 
│   Notification Service, Auth Service
├── GraphQL API (selain REST)
├── Full observability: logs + traces + metrics
├── Kubernetes deployment dengan Helm
└── Performance: > 1000 req/s dengan latency < 200ms

OPSI B: Platform CMS Enterprise
├── Multi-tenant dengan schema per tenant
├── CASL permission granular
├── Full-text search dengan Elasticsearch
├── Content workflow (draft → review → published)
├── Webhook system
├── GraphQL API
└── CDN integration untuk media
```

---

## 📊 Ringkasan & Progress Tracking

|Level|Topik|Poin|Durasi|Output|
|---|---|---|---|---|
|🟢 **1**|TS + Arsitektur NestJS|1-19|Minggu 1-5|Hello World API + Unit Test|
|🔵 **2**|Pipeline: Pipe, Guard, Interceptor, Filter, Middleware|20-42|Minggu 5-10|Product API + Full Pipeline|
|🟡 **3**|Database, Repository, Auth, Swagger|43-62|Minggu 10-18|Blog API Production|
|🟠 **4**|Cache, Queue, WebSocket, Email, Upload|63-80|Minggu 18-28|E-Commerce API|
|🔴 **5**|Testing Komprehensif + Clean Architecture|81-89|Minggu 28-36|E-Commerce Production Quality|
|⚫ **6**|DevOps, Monitoring, Microservices, GraphQL|90-107|Minggu 36-52+|Capstone SaaS Platform|

---

## 📋 Perbandingan Roadmap Lama vs Baru

|Aspek|Roadmap Lama|Roadmap Baru|
|---|---|---|
|**Total poin**|550|107 (lebih padat, lebih bermakna)|
|**Setup environment**|15 poin terpisah|2 poin|
|**Testing**|Level 6 saja|Mulai Level 1, berlanjut|
|**Swagger**|Level 4|Level 3 (sejak project pertama)|
|**ORM**|TypeORM + Prisma (dua-duanya)|Prisma saja (kuasai dalam)|
|**Bull vs BullMQ**|Bull (deprecated)|BullMQ (current)|
|**Benang Merah**|Tidak eksplisit|Setiap poin ada koneksi ke poin lain|
|**Micro-exercise**|Tidak ada|Setiap poin ada latihan langsung|
|**Proyek**|6 proyek terpisah|6 proyek berkesinambungan (saling extend)|
|**Repository Pattern**|Tidak ada|Level 3, sebagai fondasi arsitektur|
|**CQRS**|Tidak ada|Level 5|

---

## 💡 Cara Menggunakan Roadmap Ini

text

```
Setiap poin mengikuti format:
┌─────────────────────────────────────────────────┐
│ 💡 Mengapa: konteks dan masalah yang diselesaikan│
│ 🔗 Benang Merah: koneksi ke poin sebelum/sesudah │
│ 📖 Isi: konsep yang dipelajari                   │
│ 🔧 Micro-exercise: latihan langsung              │
│ 🏗️ Kontribusi ke Project Level                  │
└─────────────────────────────────────────────────┘
```

**Aturan Tidak Boleh Dilanggar:**

1. **Selesaikan micro-exercise** sebelum lanjut ke poin berikutnya
2. **Commit ke Git** setiap selesai satu poin
3. **Tulis test** untuk setiap fitur yang dibuat (sejak Level 1)
4. **Dokumentasikan di Swagger** sejak project pertama
5. **Bangun proyek level** sebelum naik ke level berikutnya
6. **Identifikasi benang merah** — selalu tanya "poin ini membangun/menghubungkan apa?"

---

_Roadmap NestJS v2.0 — Structured, Connected, Test-First_  
_Setiap batu bata diletakkan dengan sadar di atas batu bata sebelumnya_