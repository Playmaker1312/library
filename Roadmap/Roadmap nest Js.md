# 🗺️ Roadmap Project NestJS: Step-by-Step Membangun Aplikasi Nyata

## Filosofi Roadmap Ini

> **"Belajar dengan membangun"** — setiap sesi menghasilkan sesuatu yang nyata dan bisa dijalankan. Kita tidak belajar teori dulu baru praktik — kita **praktik sambil memahami mengapa**.

### Prinsip Desain

- **Project-Driven**: setiap poin = satu langkah konkret dalam membangun project
- **Incremental**: project berkembang dari yang sederhana ke kompleks
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **Satu Project, Tumbuh Bersama**: project yang sama terus di-enhance, bukan project baru dari nol setiap level

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: "Perpustakaan API" — CRUD sederhana, fondasi project
    ↓ (enhance, tidak mulai dari nol)
Level 2: + Database PostgreSQL + Validasi + Error Handling
    ↓ (enhance)
Level 3: + Authentication + Authorization + Dokumentasi
    ↓ (enhance)
Level 4: + File Upload + Caching + Background Jobs + Email
    ↓ (enhance)
Level 5: + Testing Komprehensif + Clean Architecture
    ↓ (enhance)
Level 6: + Docker + CI/CD + Monitoring + Deploy Production
    ↓ (enhance — pilih jalur)
Level 7: Microservices ATAU GraphQL ATAU Multi-tenant SaaS
```

---

## 🟢 LEVEL 1: MEMBANGUN FONDASI PROJECT (Minggu 1-3)

> **Tema**: _"Dari nol ke API yang berjalan dengan struktur yang benar"_  
> **Benang Merah**: Setup → Struktur project → Module pertama → Controller → Service → Response standar → Git  
> **Output**: REST API Perpustakaan dengan CRUD buku (data in-memory), berjalan di local

---

### A. Inisialisasi & Struktur Project

> 💡 **Mengapa dimulai di sini?** Struktur project yang baik dari awal menghemat banyak waktu refactor nantinya. Keputusan di bagian ini mempengaruhi semua level berikutnya.

text

```
Benang Merah Bagian A:
Tidak ada project → Scaffold dengan NestJS CLI →
Pahami setiap file → Tambahkan tooling kualitas kode →
Git dari hari pertama → Siap menulis fitur pertama
```

1. `[[1. Membuat Project NestJS Baru — nest new & Memilih Package Manager]]`
    
    - Jalankan `nest new perpustakaan-api` dan pilih `pnpm`
    - Pahami pertanyaan yang diajukan CLI: nama project, package manager
    - Verifikasi project berjalan: `pnpm run start:dev`
    - Buka `http://localhost:3000` — lihat "Hello World!"
    - _Langkah konkret_: Project berjalan, commit pertama: `feat: initial NestJS project`
2. `[[2. Memahami Setiap File yang Dibuat NestJS CLI]]`
    
    - `src/main.ts`: entry point, bootstrap aplikasi
    - `src/app.module.ts`: root module, induk semua module
    - `src/app.controller.ts`: controller default
    - `src/app.service.ts`: service default
    - `nest-cli.json`: konfigurasi CLI (sourceRoot, compilerOptions)
    - `tsconfig.json`: konfigurasi TypeScript
    - `package.json`: scripts, dependencies
    - _Langkah konkret_: Hapus `app.controller.spec.ts` default, kita akan buat ulang nanti dengan benar
3. `[[3. Konfigurasi main.ts — Port, Prefix Global & Pengaturan Dasar]]`
    
    - Ganti port ke `3000` via environment variable: `process.env.PORT || 3000`
    - Tambahkan global prefix: `app.setGlobalPrefix('api/v1')`
    - Enable shutdown hooks: `app.enableShutdownHooks()`
    - _Langkah konkret_: Test endpoint sekarang di `http://localhost:3000/api/v1`
4. `[[4. Setup ESLint & Prettier — Kualitas Kode dari Hari Pertama]]`
    
    - Review `.eslintrc.js` default NestJS
    - Review `.prettierrc` default NestJS
    - Tambahkan rules: `no-console` (warn), `no-unused-vars` (error)
    - Setup VS Code: `editor.formatOnSave: true`, `editor.defaultFormatter: esbenp.prettier-vscode`
    - Jalankan `pnpm run lint` dan `pnpm run format`
    - _Langkah konkret_: Semua file lulus lint, commit: `chore: configure eslint and prettier`
5. `[[5. Setup Git & Struktur Commit yang Baik — Conventional Commits]]`
    
    - Inisialisasi git sudah dilakukan CLI, setup remote ke GitHub
    - Install `husky` dan `lint-staged`: lint otomatis sebelum commit
    - Konvensi commit: `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`, `test:`
    - Buat `.gitignore` yang lengkap: `node_modules`, `dist`, `.env`
    - _Langkah konkret_: Push ke GitHub, verifikasi husky berjalan saat commit
6. `[[6. Merencanakan Struktur Folder Project — Arsitektur yang Akan Kita Gunakan]]`
    
    - Struktur yang akan kita bangun:
        
        text
        
        ```
        src/
        ├── common/          ← shared utilities, decorators, filters, guards
        │   ├── decorators/
        │   ├── filters/
        │   ├── guards/
        │   ├── interceptors/
        │   └── pipes/
        ├── config/          ← konfigurasi aplikasi
        ├── modules/         ← feature modules
        │   └── books/       ← module pertama kita
        └── main.ts
        ```
        
    - Buat folder structure ini sekarang (kosong)
    - _Langkah konkret_: Buat semua folder, commit: `chore: setup project folder structure`

---

### B. Module Pertama — Books

> 💡 **Benang Merah ke A**: Struktur folder sudah siap (Poin 6). Sekarang kita isi `src/modules/books/` dengan module pertama. Ini adalah pola yang akan diulang untuk SEMUA module di project ini.

text

```
Benang Merah Bagian B:
Folder structure siap (Poin 6) →
Generate module dengan CLI →
Definisikan entity/interface →
Buat DTO untuk input →
Controller untuk routing →
Service untuk logika bisnis →
Daftarkan di module →
Test endpoint berjalan
```

7. `[[7. Generate Books Module — Menggunakan NestJS CLI dengan Benar]]`
    
    - Jalankan: `nest g module modules/books`
    - Jalankan: `nest g controller modules/books --no-spec`
    - Jalankan: `nest g service modules/books --no-spec`
    - Perhatikan `app.module.ts` otomatis terupdate
    - _Langkah konkret_: Verifikasi `BooksModule`, `BooksController`, `BooksService` terbuat dan terhubung
8. `[[8. Mendefinisikan Entity Book — Interface & Type untuk Data]]`
    
    - Buat `src/modules/books/entities/book.entity.ts`
    - Definisikan interface `Book`:
        
        TypeScript
        
        ```
        export interface Book {
          id: string;
          title: string;
          author: string;
          isbn: string;
          year: number;
          stock: number;
          createdAt: Date;
          updatedAt: Date;
        }
        ```
        
    - Menggunakan `uuid` package untuk generate ID
    - _Langkah konkret_: Install `uuid` dan `@types/uuid`, import di service
9. `[[9. Membuat DTO — Kontrak Input yang Jelas & Tervalidasi]]`
    
    - Buat `src/modules/books/dto/create-book.dto.ts`
    - Buat `src/modules/books/dto/update-book.dto.ts` menggunakan `PartialType`
    - Buat `src/modules/books/dto/query-book.dto.ts` untuk filter dan pagination
    - Install `@nestjs/mapped-types` untuk `PartialType`
    - _Langkah konkret_:
        
        TypeScript
        
        ```
        // create-book.dto.ts
        export class CreateBookDto {
          title: string;
          author: string;
          isbn: string;
          year: number;
          stock: number;
        }
        ```
        
10. `[[10. Membangun BooksService — CRUD dengan In-Memory Array]]`
    
    - Gunakan `private books: Book[] = []` sebagai storage sementara
    - Implementasikan method:
        - `findAll(query: QueryBookDto): Book[]`
        - `findById(id: string): Book`
        - `create(dto: CreateBookDto): Book`
        - `update(id: string, dto: UpdateBookDto): Book`
        - `remove(id: string): void`
    - Throw `NotFoundException` jika buku tidak ditemukan
    - _Langkah konkret_: Semua method terimplementasi, termasuk error handling
11. `[[11. Membangun BooksController — Routing & HTTP Method yang Benar]]`
    
    - Prefix controller: `@Controller('books')`
    - `GET /books` → `findAll()` dengan query params
    - `GET /books/:id` → `findById()` dengan `:id` parameter
    - `POST /books` → `create()` dengan request body
    - `PATCH /books/:id` → `update()` dengan `:id` dan body
    - `DELETE /books/:id` → `remove()` return 204 No Content
    - _Langkah konkret_: Test semua endpoint dengan REST Client atau Postman
12. `[[12. Menguji Semua Endpoint — REST Client File di VS Code]]`
    
    - Buat `requests/books.http` di root project
    - Tulis request untuk semua endpoint:
        
        http
        
        ```
        ### Create Book
        POST http://localhost:3000/api/v1/books
        Content-Type: application/json
        
        {
          "title": "Clean Code",
          "author": "Robert C. Martin",
          "isbn": "978-0132350884",
          "year": 2008,
          "stock": 5
        }
        ```
        
    - _Langkah konkret_: Semua request berhasil, simpan response untuk referensi

---

### C. Response Standar & Error Handling Dasar

> 💡 **Benang Merah ke B**: Controller mengembalikan data mentah. Tapi di production, kita perlu format yang konsisten untuk SEMUA response — baik sukses maupun error. Ini yang membuat API kita "professional".

text

```
Benang Merah Bagian C:
Controller mengembalikan data mentah (Poin 11) →
Response wrapper interceptor membungkus semua response →
Exception filter menangkap semua error →
Format konsisten: { data, message, statusCode, timestamp }
```

13. `[[13. Membuat Response Wrapper Interceptor — Format Sukses yang Konsisten]]`
    
    - Buat `src/common/interceptors/response-wrapper.interceptor.ts`
    - Bungkus semua response dalam format:
        
        TypeScript
        
        ```
        {
          statusCode: number,
          message: string,
          data: T,
          timestamp: string
        }
        ```
        
    - Daftarkan sebagai global interceptor di `main.ts`
    - _Langkah konkret_: Test endpoint, verifikasi semua response terbungkus
14. `[[14. Membuat Global Exception Filter — Format Error yang Konsisten]]`
    
    - Buat `src/common/filters/global-exception.filter.ts`
    - Tangkap semua `HttpException` dan format menjadi:
        
        TypeScript
        
        ```
        {
          statusCode: number,
          message: string,
          error: string,
          timestamp: string,
          path: string
        }
        ```
        
    - Tangkap juga error yang bukan `HttpException` (500 Internal Server Error)
    - Daftarkan sebagai global filter di `main.ts`
    - _Langkah konkret_: Test dengan request buku yang tidak ada, verifikasi format error konsisten
15. `[[15. Membuat Logging Interceptor — Setiap Request Tercatat]]`
    
    - Buat `src/common/interceptors/logging.interceptor.ts`
    - Gunakan `NestJS Logger` (bukan `console.log`)
    - Log: `[METHOD] /path → STATUS (Xms)`
    - _Langkah konkret_: Setiap request tampil di console dengan format yang rapi
16. `[[16. Validasi Input Pertama — ValidationPipe Global]]`
    
    - Install `class-validator` dan `class-transformer`
    - Tambahkan decorator ke `CreateBookDto`:
        
        TypeScript
        
        ```
        @IsString() @IsNotEmpty() title: string;
        @IsString() @IsNotEmpty() author: string;
        @IsISBN() isbn: string;
        @IsInt() @Min(1900) @Max(2024) year: number;
        @IsInt() @Min(0) stock: number;
        ```
        
    - Daftarkan `ValidationPipe` global di `main.ts` dengan config:  
        `whitelist: true`, `forbidNonWhitelisted: true`, `transform: true`
    - _Langkah konkret_: Test dengan data invalid, verifikasi error 400 dengan pesan yang jelas

---

### 🏗️ Checkpoint Level 1

text

```
✅ Checklist sebelum lanjut ke Level 2:
├── Project berjalan dengan pnpm run start:dev
├── 5 endpoint Books CRUD berfungsi
├── Semua response dalam format standar
├── Error handling terpusat
├── Validasi input dengan class-validator
├── Logging setiap request
├── File requests/books.http lengkap
├── Git history dengan commit yang rapi
└── README.md menjelaskan cara menjalankan project

Commit: feat: complete books CRUD with in-memory storage
```

---

## 🔵 LEVEL 2: INTEGRASI DATABASE & KONFIGURASI (Minggu 3-8)

> **Tema**: _"Dari data sementara ke data persisten dengan PostgreSQL dan Prisma"_  
> **Benang Merah**: In-memory storage (Level 1) → kebutuhan persistensi → Prisma setup → Migrasi schema → Repository pattern → Service menggunakan repository  
> **Output**: Perpustakaan API dengan database PostgreSQL, env configuration, dan arsitektur repository yang bersih

---

### D. Setup Database & Prisma

> 💡 **Benang Merah ke Level 1**: Di Level 1, data hilang setiap restart. Sekarang kita hubungkan ke PostgreSQL menggunakan Prisma — semua data persisten dan type-safe.

text

```
Benang Merah Bagian D:
Data hilang saat restart (Level 1) →
Docker Compose: jalankan PostgreSQL lokal →
Prisma init: schema.prisma →
Definisikan model Book →
Migrate: schema → tabel PostgreSQL →
PrismaService: koneksi ke database →
Test: data tetap ada setelah restart
```

17. `[[17. Menjalankan PostgreSQL dengan Docker Compose]]`
    
    - Buat `docker-compose.yml` di root project:
        
        YAML
        
        ```
        version: '3.8'
        services:
          postgres:
            image: postgres:16-alpine
            environment:
              POSTGRES_DB: perpustakaan_db
              POSTGRES_USER: postgres
              POSTGRES_PASSWORD: postgres
            ports:
              - "5432:5432"
            volumes:
              - postgres_data:/var/lib/postgresql/data
          
          redis:
            image: redis:7-alpine
            ports:
              - "6379:6379"
        
        volumes:
          postgres_data:
        ```
        
    - Jalankan: `docker compose up -d`
    - Verifikasi: `docker compose ps` — status "running"
    - _Langkah konkret_: Connect via TablePlus, lihat database `perpustakaan_db` kosong
18. `[[18. Inisialisasi Prisma — Schema, .env & Konfigurasi Awal]]`
    
    - Install: `pnpm add prisma @prisma/client`
    - Jalankan: `npx prisma init`
    - Update `.env`:
        
        text
        
        ```
        DATABASE_URL="postgresql://postgres:postgres@localhost:5432/perpustakaan_db"
        ```
        
    - Update `.gitignore`: pastikan `.env` sudah diabaikan
    - Buat `.env.example` dengan nilai placeholder (ini yang di-commit)
    - _Langkah konkret_: `npx prisma validate` — tidak ada error
19. `[[19. Mendefinisikan Prisma Schema — Model Book & Konfigurasi]]`
    
    - Tulis model `Book` di `prisma/schema.prisma`:
        
        prisma
        
        ```
        generator client {
          provider = "prisma-client-js"
        }
        
        datasource db {
          provider = "postgresql"
          url      = env("DATABASE_URL")
        }
        
        model Book {
          id        String   @id @default(cuid())
          title     String
          author    String
          isbn      String   @unique
          year      Int
          stock     Int      @default(0)
          createdAt DateTime @default(now())
          updatedAt DateTime @updatedAt
          deletedAt DateTime?
        
          @@map("books")
        }
        ```
        
    - _Langkah konkret_: Pahami setiap line — apa arti `@default(cuid())`, `@@map`, `@updatedAt`
20. `[[20. Menjalankan Migrasi Pertama — Schema ke Database]]`
    
    - Jalankan: `npx prisma migrate dev --name init`
    - Pahami apa yang terjadi:
        - Prisma membuat folder `prisma/migrations/`
        - File SQL migration dibuat
        - Migration dijalankan ke database
        - Prisma Client di-generate
    - Verifikasi di TablePlus: tabel `books` terbuat dengan kolom yang benar
    - _Langkah konkret_: Buka file migration SQL, baca dan pahami SQL yang dihasilkan
21. `[[21. Membuat PrismaService — Koneksi Database yang Dikelola NestJS]]`
    
    - Buat `src/database/prisma.service.ts`:
        
        TypeScript
        
        ```
        @Injectable()
        export class PrismaService extends PrismaClient implements OnModuleInit {
          async onModuleInit() {
            await this.$connect();
          }
        
          async enableShutdownHooks(app: INestApplication) {
            process.on('beforeExit', async () => {
              await app.close();
            });
          }
        }
        ```
        
    - Buat `src/database/database.module.ts` sebagai global module
    - Import `DatabaseModule` di `AppModule`
    - _Langkah konkret_: Inject `PrismaService` ke `BooksService`, log jumlah buku dari database
22. `[[22. Membuat Seeder — Data Awal untuk Development]]`
    
    - Install `faker-js`: `pnpm add -D @faker-js/faker`
    - Buat `prisma/seed.ts`:
        
        TypeScript
        
        ```
        const main = async () => {
          const prisma = new PrismaClient();
          
          // Hapus data lama
          await prisma.book.deleteMany();
          
          // Buat 20 buku dummy
          const books = Array.from({ length: 20 }, () => ({
            title: faker.lorem.words(3),
            author: faker.person.fullName(),
            isbn: faker.string.numeric(13),
            year: faker.number.int({ min: 1990, max: 2024 }),
            stock: faker.number.int({ min: 0, max: 20 }),
          }));
          
          await prisma.book.createMany({ data: books });
          console.log('✅ Seeded 20 books');
        };
        ```
        
    - Tambahkan ke `package.json`: `"prisma": { "seed": "ts-node prisma/seed.ts" }`
    - Jalankan: `npx prisma db seed`
    - _Langkah konkret_: Verifikasi 20 buku muncul di TablePlus

---

### E. Repository Pattern — Arsitektur yang Bersih

> 💡 **Benang Merah ke Prisma**: Prisma Client sudah tersedia via `PrismaService`. Tapi jika kita gunakan langsung di `BooksService`, maka:
> 
> - Unit test `BooksService` butuh database nyata (lambat)
> - Jika kita ganti Prisma dengan ORM lain, harus ubah semua service  
>     **Repository Pattern** membuat `BooksService` tidak tahu bahwa kita pakai Prisma.

text

```
Benang Merah Bagian E:
PrismaService tersedia (Poin 21) →
Interface repository: kontrak tanpa implementasi →
PrismaBooksRepository: implementasi dengan Prisma →
BooksService menggunakan interface (bukan Prisma langsung) →
Testing service: mock interface, tanpa database
```

23. `[[23. Membuat Interface Repository — Kontrak Data Access]]`
    
    - Buat `src/modules/books/repositories/books.repository.interface.ts`:
        
        TypeScript
        
        ```
        export interface IBooksRepository {
          findAll(params: FindAllParams): Promise<PaginatedResult<Book>>;
          findById(id: string): Promise<Book | null>;
          findByIsbn(isbn: string): Promise<Book | null>;
          create(data: CreateBookData): Promise<Book>;
          update(id: string, data: UpdateBookData): Promise<Book>;
          softDelete(id: string): Promise<void>;
          count(where?: BookWhereInput): Promise<number>;
        }
        
        export const BOOKS_REPOSITORY = 'BOOKS_REPOSITORY';
        ```
        
    - Definisikan juga type: `FindAllParams`, `PaginatedResult<T>`, `CreateBookData`
    - _Langkah konkret_: Interface selesai — ini adalah "kontrak" yang harus dipenuhi implementasi
24. `[[24. Implementasi PrismaBooksRepository — Query Database]]`
    
    - Buat `src/modules/books/repositories/prisma-books.repository.ts`
    - Implementasikan semua method dari interface:
        
        TypeScript
        
        ```
        @Injectable()
        export class PrismaBooksRepository implements IBooksRepository {
          constructor(private readonly prisma: PrismaService) {}
        
          async findAll({ page, limit, search, sortBy, sortOrder }: FindAllParams) {
            const where = search ? {
              OR: [
                { title: { contains: search, mode: 'insensitive' } },
                { author: { contains: search, mode: 'insensitive' } },
              ],
              deletedAt: null,
            } : { deletedAt: null };
        
            const [data, total] = await Promise.all([
              this.prisma.book.findMany({
                where,
                skip: (page - 1) * limit,
                take: limit,
                orderBy: { [sortBy]: sortOrder },
              }),
              this.prisma.book.count({ where }),
            ]);
        
            return {
              data,
              total,
              page,
              limit,
              totalPages: Math.ceil(total / limit),
            };
          }
          // ... implementasi method lainnya
        }
        ```
        
    - _Langkah konkret_: Semua method terimplementasi dengan query Prisma yang optimal
25. `[[25. Mendaftarkan Repository sebagai Custom Provider di Module]]`
    
    - Update `books.module.ts`:
        
        TypeScript
        
        ```
        @Module({
          providers: [
            BooksService,
            {
              provide: BOOKS_REPOSITORY,
              useClass: PrismaBooksRepository,
            },
            PrismaBooksRepository,
          ],
          controllers: [BooksController],
        })
        export class BooksModule {}
        ```
        
    - Update `BooksService` untuk inject via token:
        
        TypeScript
        
        ```
        constructor(
          @Inject(BOOKS_REPOSITORY)
          private readonly booksRepository: IBooksRepository,
        ) {}
        ```
        
    - _Langkah konkret_: Service sekarang menggunakan interface, tidak ada import Prisma di service
26. `[[26. Merewrite BooksService — Menggunakan Repository]]`
    
    - Ganti semua in-memory logic dengan repository calls:
        
        TypeScript
        
        ```
        async findAll(query: QueryBookDto) {
          return this.booksRepository.findAll({
            page: query.page ?? 1,
            limit: query.limit ?? 10,
            search: query.search,
            sortBy: query.sortBy ?? 'createdAt',
            sortOrder: query.sortOrder ?? 'desc',
          });
        }
        
        async findById(id: string) {
          const book = await this.booksRepository.findById(id);
          if (!book) throw new NotFoundException(`Book with id ${id} not found`);
          return book;
        }
        
        async create(dto: CreateBookDto) {
          const existing = await this.booksRepository.findByIsbn(dto.isbn);
          if (existing) throw new ConflictException('ISBN already exists');
          return this.booksRepository.create(dto);
        }
        ```
        
    - _Langkah konkret_: Test semua endpoint, data sekarang dari database PostgreSQL

---

### F. Konfigurasi & Environment

> 💡 **Benang Merah ke Setup**: Di Poin 18 kita hardcode `DATABASE_URL` di `.env`. Sekarang kita kelola semua konfigurasi secara terstruktur dan tervalidasi — sehingga aplikasi tidak bisa berjalan dengan konfigurasi yang salah.

text

```
Benang Merah Bagian F:
Hardcode config di .env (Poin 18) →
ConfigModule: akses env var via service →
Validasi Joi: crash at startup jika config kurang →
Namespace config: kelompokkan config terkait →
Type-safe: tidak ada typo nama env var
```

27. `[[27. Setup ConfigModule — Akses Environment Variable yang Terstruktur]]`
    
    - Install: `pnpm add @nestjs/config joi`
    - Update `app.module.ts`:
        
        TypeScript
        
        ```
        ConfigModule.forRoot({
          isGlobal: true,
          envFilePath: '.env',
          validationSchema: Joi.object({
            NODE_ENV: Joi.string()
              .valid('development', 'production', 'test')
              .default('development'),
            PORT: Joi.number().default(3000),
            DATABASE_URL: Joi.string().required(),
          }),
        }),
        ```
        
    - _Langkah konkret_: Hapus satu env var wajib dari `.env`, verifikasi aplikasi crash dengan pesan jelas
28. `[[28. Membuat Namespace Configuration — Kelompokkan Config Terkait]]`
    
    - Buat `src/config/database.config.ts`:
        
        TypeScript
        
        ```
        export default registerAs('database', () => ({
          url: process.env.DATABASE_URL,
          maxConnections: parseInt(process.env.DB_MAX_CONNECTIONS, 10) || 10,
        }));
        ```
        
    - Buat `src/config/app.config.ts`:
        
        TypeScript
        
        ```
        export default registerAs('app', () => ({
          port: parseInt(process.env.PORT, 10) || 3000,
          nodeEnv: process.env.NODE_ENV || 'development',
          globalPrefix: process.env.GLOBAL_PREFIX || 'api/v1',
        }));
        ```
        
    - Daftarkan di `ConfigModule.forRoot({ load: [databaseConfig, appConfig] })`
    - _Langkah konkret_: Gunakan `ConfigService.get<string>('database.url')` di `PrismaService`
29. `[[29. Update main.ts — Gunakan ConfigService untuk Semua Config]]`
    
    - Ambil port dari config: `const port = app.get(ConfigService).get<number>('app.port')`
    - Ambil prefix dari config
    - _Langkah konkret_: Tidak ada nilai hardcode di `main.ts`, semua dari `ConfigService`

---

### G. Pagination, Filter & Sort yang Reusable

> 💡 **Benang Merah ke Repository**: `findAll` di repository sudah menerima `FindAllParams`. Sekarang kita buat `QueryBookDto` yang proper dan utility pagination yang bisa dipakai di semua module nanti.

text

```
Benang Merah Bagian G:
FindAllParams interface (Poin 23) →
QueryBookDto: validasi query params →
PaginationDto: reusable di semua module →
PaginatedResult: format response pagination standar →
Controller: gunakan @Query() dengan DTO yang tervalidasi
```

30. `[[30. Membuat PaginationDto — Reusable di Semua Module]]`
    
    - Buat `src/common/dto/pagination.dto.ts`:
        
        TypeScript
        
        ```
        export class PaginationDto {
          @IsOptional()
          @Type(() => Number)
          @IsInt()
          @Min(1)
          page?: number = 1;
        
          @IsOptional()
          @Type(() => Number)
          @IsInt()
          @Min(1)
          @Max(100)
          limit?: number = 10;
        }
        ```
        
    - _Langkah konkret_: `QueryBookDto extends PaginationDto` — tidak perlu definisikan `page` dan `limit` lagi
31. `[[31. Membuat QueryBookDto — Filter & Sort Spesifik Buku]]`
    
    - Extend `PaginationDto` dan tambahkan:
        
        TypeScript
        
        ```
        export class QueryBookDto extends PaginationDto {
          @IsOptional()
          @IsString()
          search?: string;
        
          @IsOptional()
          @IsEnum(['title', 'author', 'year', 'createdAt'])
          sortBy?: string = 'createdAt';
        
          @IsOptional()
          @IsEnum(['asc', 'desc'])
          sortOrder?: 'asc' | 'desc' = 'desc';
        
          @IsOptional()
          @Type(() => Number)
          @IsInt()
          @Min(1900)
          year?: number;
        }
        ```
        
    - _Langkah konkret_: Test endpoint `GET /books?search=clean&sortBy=title&year=2008`
32. `[[32. Membuat PaginatedResult Type — Format Response Pagination Standar]]`
    
    - Buat `src/common/types/paginated-result.type.ts`:
        
        TypeScript
        
        ```
        export class PaginatedResult<T> {
          data: T[];
          meta: {
            total: number;
            page: number;
            limit: number;
            totalPages: number;
            hasNextPage: boolean;
            hasPrevPage: boolean;
          };
        }
        ```
        
    - Update repository untuk menggunakan tipe ini
    - _Langkah konkret_: Response `GET /books` sekarang memiliki `data` dan `meta` yang lengkap

---

### 🏗️ Checkpoint Level 2

text

```
✅ Checklist sebelum lanjut ke Level 3:
├── PostgreSQL berjalan via Docker Compose
├── Prisma schema dengan model Book
├── Migration berhasil dijalankan
├── Seed data 20 buku berhasil
├── Repository Pattern terimplementasi
├── ConfigModule dengan validasi Joi
├── Namespace configuration
├── Pagination dan filter berfungsi
├── Soft delete terimplementasi
├── Data persisten setelah restart
└── Semua endpoint masih berfungsi seperti Level 1

Commit: feat: integrate PostgreSQL with Prisma and repository pattern
```

---

## 🟡 LEVEL 3: AUTHENTICATION & AUTHORIZATION (Minggu 8-15)

> **Tema**: _"Siapa yang boleh mengakses apa — sistem auth lengkap dari register hingga JWT"_  
> **Benang Merah**: API publik (Level 2) → tambahkan identity (Auth) → tentukan akses (Authorization) → dokumentasikan (Swagger)  
> **Output**: API dengan auth lengkap: register, login, JWT, refresh token, role-based access

---

### H. Menyiapkan Module Baru — Users & Auth

> 💡 **Benang Merah ke Module Pattern**: Pola generate module (Poin 7) yang sama, tapi kali ini untuk `Users` dan `Auth`. Perbedaannya: `AuthModule` bergantung pada `UsersModule`.

text

```
Benang Merah Bagian H:
Pola module (Poin 7) →
UsersModule: CRUD user, kelola data user →
AuthModule: bergantung pada UsersModule →
Dependency antar module via exports/imports →
JwtModule: generate dan verifikasi token
```

33. `[[33. Membuat User Schema di Prisma — Tambah Model User]]`
    
    - Update `prisma/schema.prisma`, tambahkan model `User`:
        
        prisma
        
        ```
        model User {
          id           String    @id @default(cuid())
          email        String    @unique
          password     String
          name         String
          role         Role      @default(USER)
          isVerified   Boolean   @default(false)
          refreshToken String?
          createdAt    DateTime  @default(now())
          updatedAt    DateTime  @updatedAt
          deletedAt    DateTime?
        
          @@map("users")
        }
        
        enum Role {
          USER
          LIBRARIAN
          ADMIN
        }
        ```
        
    - Jalankan: `npx prisma migrate dev --name add-user-model`
    - _Langkah konkret_: Verifikasi tabel `users` dengan enum `role` di database
34. `[[34. Membuat UsersModule — CRUD User & Repository]]`
    
    - Generate: `nest g module modules/users`, `controller`, `service`
    - Buat interface `IUsersRepository` dengan method yang dibutuhkan
    - Buat `PrismaUsersRepository`
    - Implementasikan `UsersService` dengan method:
        - `findByEmail(email: string): Promise<User | null>`
        - `findById(id: string): Promise<User | null>`
        - `create(data: CreateUserData): Promise<User>`
        - `updateRefreshToken(id: string, token: string | null): Promise<void>`
    - **Export `UsersService`** dari `UsersModule` agar bisa digunakan `AuthModule`
    - _Langkah konkret_: `UsersModule` export `UsersService`, verifikasi tidak ada error circular dependency
35. `[[35. Menyiapkan AuthModule — Dependensi & Konfigurasi]]`
    
    - Generate: `nest g module modules/auth`, `controller`, `service`
    - Install packages:
        
        Bash
        
        ```
        pnpm add @nestjs/jwt @nestjs/passport passport passport-jwt passport-local bcrypt
        pnpm add -D @types/passport-jwt @types/passport-local @types/bcrypt
        ```
        
    - Import di `AuthModule`:
        
        TypeScript
        
        ```
        @Module({
          imports: [
            UsersModule,
            PassportModule,
            JwtModule.registerAsync({
              inject: [ConfigService],
              useFactory: (config: ConfigService) => ({
                secret: config.get('auth.accessTokenSecret'),
                signOptions: { expiresIn: '15m' },
              }),
            }),
          ],
        })
        ```
        
    - Tambahkan namespace config `auth.config.ts`:
        
        TypeScript
        
        ```
        export default registerAs('auth', () => ({
          accessTokenSecret: process.env.JWT_ACCESS_SECRET,
          refreshTokenSecret: process.env.JWT_REFRESH_SECRET,
          accessTokenExpiry: process.env.JWT_ACCESS_EXPIRY || '15m',
          refreshTokenExpiry: process.env.JWT_REFRESH_EXPIRY || '7d',
        }));
        ```
        
    - _Langkah konkret_: Tambahkan env var baru ke `.env`, validasi di Joi schema

---

### I. Implementasi Register & Login

> 💡 **Benang Merah ke UsersModule**: `AuthService` menggunakan `UsersService` (dari `UsersModule`) untuk mencari dan membuat user. Ini adalah contoh nyata dependency antar module via `exports`.

text

```
Benang Merah Bagian I:
UsersService (Poin 34) diinjeksi ke AuthService →
Register: validasi → hash password → simpan user →
Login: cari user → verifikasi password → generate tokens →
Kedua token disimpan/dikirim ke client →
Refresh: gunakan refresh token untuk dapat access token baru
```

36. `[[36. Membuat Register Endpoint — Validasi, Hash Password & Simpan User]]`
    
    - Buat `RegisterDto`:
        
        TypeScript
        
        ```
        export class RegisterDto {
          @IsString() @IsNotEmpty() name: string;
          @IsEmail() email: string;
          @IsString() @MinLength(8) password: string;
        }
        ```
        
    - Implementasikan `AuthService.register()`:
        
        TypeScript
        
        ```
        async register(dto: RegisterDto) {
          // 1. Cek apakah email sudah terdaftar
          const existing = await this.usersService.findByEmail(dto.email);
          if (existing) throw new ConflictException('Email already registered');
        
          // 2. Hash password
          const hashedPassword = await bcrypt.hash(dto.password, 10);
        
          // 3. Simpan user
          const user = await this.usersService.create({
            ...dto,
            password: hashedPassword,
          });
        
          // 4. Return user tanpa password
          const { password, ...result } = user;
          return result;
        }
        ```
        
    - _Langkah konkret_: Test register dengan email valid dan invalid (duplikat)
37. `[[37. Membuat Login Endpoint — Verifikasi & Generate JWT Tokens]]`
    
    - Buat `LoginDto`:
        
        TypeScript
        
        ```
        export class LoginDto {
          @IsEmail() email: string;
          @IsString() @IsNotEmpty() password: string;
        }
        ```
        
    - Implementasikan `AuthService.login()`:
        
        TypeScript
        
        ```
        async login(dto: LoginDto) {
          // 1. Cari user
          const user = await this.usersService.findByEmail(dto.email);
          if (!user) throw new UnauthorizedException('Invalid credentials');
        
          // 2. Verifikasi password
          const isValid = await bcrypt.compare(dto.password, user.password);
          if (!isValid) throw new UnauthorizedException('Invalid credentials');
        
          // 3. Generate tokens
          const tokens = await this.generateTokens(user.id, user.email, user.role);
        
          // 4. Simpan refresh token hash
          const hashedRefreshToken = await bcrypt.hash(tokens.refreshToken, 10);
          await this.usersService.updateRefreshToken(user.id, hashedRefreshToken);
        
          return tokens;
        }
        
        private async generateTokens(userId: string, email: string, role: Role) {
          const [accessToken, refreshToken] = await Promise.all([
            this.jwtService.signAsync(
              { sub: userId, email, role },
              { secret: this.config.get('auth.accessTokenSecret'), expiresIn: '15m' }
            ),
            this.jwtService.signAsync(
              { sub: userId, email },
              { secret: this.config.get('auth.refreshTokenSecret'), expiresIn: '7d' }
            ),
          ]);
          return { accessToken, refreshToken };
        }
        ```
        
    - _Langkah konkret_: Test login, simpan `accessToken` dan `refreshToken` dari response
38. `[[38. Membuat Refresh Token Endpoint — Perpanjangan Session]]`
    
    - Implementasikan `AuthService.refreshTokens()`:
        
        TypeScript
        
        ```
        async refreshTokens(userId: string, refreshToken: string) {
          // 1. Cari user dan cek refresh token tersimpan
          const user = await this.usersService.findById(userId);
          if (!user?.refreshToken)
            throw new ForbiddenException('Access denied');
        
          // 2. Verifikasi refresh token
          const isValid = await bcrypt.compare(refreshToken, user.refreshToken);
          if (!isValid) throw new ForbiddenException('Access denied');
        
          // 3. Generate tokens baru (rotation)
          const tokens = await this.generateTokens(user.id, user.email, user.role);
        
          // 4. Update refresh token di database
          const hashedRefreshToken = await bcrypt.hash(tokens.refreshToken, 10);
          await this.usersService.updateRefreshToken(user.id, hashedRefreshToken);
        
          return tokens;
        }
        ```
        
    - _Langkah konkret_: Test refresh — access token baru didapat, refresh token lama tidak bisa dipakai lagi
39. `[[39. Membuat Logout Endpoint — Invalidasi Refresh Token]]`
    
    - Implementasikan `AuthService.logout()`:
        
        TypeScript
        
        ```
        async logout(userId: string) {
          await this.usersService.updateRefreshToken(userId, null);
        }
        ```
        
    - _Langkah konkret_: Test logout, verifikasi refresh token tidak bisa dipakai lagi

---

### J. JWT Guards & Proteksi Endpoint

> 💡 **Benang Merah ke Guards (Level 2 konsep)**: Sekarang kita implementasikan JWT Guard nyata yang melindungi endpoint Books. Guard memeriksa token, mengekstrak user, dan menyimpannya di request.

text

```
Benang Merah Bagian J:
Token berhasil dibuat (Poin 37) →
JwtStrategy: validasi token dan ekstrak payload →
JwtAuthGuard: guard yang menggunakan JwtStrategy →
@Public() decorator: pengecualian route →
@CurrentUser() decorator: ambil user dari request →
Protected routes: Books CRUD butuh auth
```

40. `[[40. Membuat JwtStrategy — Validasi Token & Ekstrak User]]`
    
    - Buat `src/modules/auth/strategies/jwt.strategy.ts`:
        
        TypeScript
        
        ```
        @Injectable()
        export class JwtStrategy extends PassportStrategy(Strategy, 'jwt') {
          constructor(config: ConfigService) {
            super({
              jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
              secretOrKey: config.get('auth.accessTokenSecret'),
              ignoreExpiration: false,
            });
          }
        
          async validate(payload: JwtPayload) {
            return {
              id: payload.sub,
              email: payload.email,
              role: payload.role,
            };
          }
        }
        ```
        
    - _Langkah konkret_: Strategi mendekode token dan return user object yang disimpan di `req.user`
41. `[[41. Membuat JwtRefreshStrategy — Validasi Refresh Token]]`
    
    - Buat `src/modules/auth/strategies/jwt-refresh.strategy.ts`
    - Ambil refresh token dari Authorization header
    - Validasi menggunakan `refreshTokenSecret` yang berbeda
    - Return `{ userId, refreshToken }` sebagai payload
    - _Langkah konkret_: Gunakan di endpoint `/auth/refresh`
42. `[[42. Membuat JwtAuthGuard & Decorator @Public()]]`
    
    - Buat `src/common/guards/jwt-auth.guard.ts`:
        
        TypeScript
        
        ```
        @Injectable()
        export class JwtAuthGuard extends AuthGuard('jwt') {
          constructor(private reflector: Reflector) {
            super();
          }
        
          canActivate(context: ExecutionContext) {
            const isPublic = this.reflector.getAllAndOverride<boolean>('isPublic', [
              context.getHandler(),
              context.getClass(),
            ]);
            if (isPublic) return true;
            return super.canActivate(context);
          }
        }
        ```
        
    - Buat `src/common/decorators/public.decorator.ts`:
        
        TypeScript
        
        ```
        export const Public = () => SetMetadata('isPublic', true);
        ```
        
    - Daftarkan `JwtAuthGuard` sebagai **global guard** di `main.ts`
    - _Langkah konkret_: Semua route butuh token kecuali yang diberi `@Public()`
43. `[[43. Membuat @CurrentUser() Decorator — Akses User dari Request]]`
    
    - Buat `src/common/decorators/current-user.decorator.ts`:
        
        TypeScript
        
        ```
        export const CurrentUser = createParamDecorator(
          (data: keyof RequestUser | undefined, ctx: ExecutionContext) => {
            const request = ctx.switchToHttp().getRequest();
            const user = request.user as RequestUser;
            return data ? user?.[data] : user;
          },
        );
        ```
        
    - Penggunaan di controller:
        
        TypeScript
        
        ```
        @Get('profile')
        getProfile(@CurrentUser() user: RequestUser) {
          return user;
        }
        
        @Get('profile/email')
        getEmail(@CurrentUser('email') email: string) {
          return { email };
        }
        ```
        
    - _Langkah konkret_: Endpoint `/users/profile` mengembalikan data user yang sedang login
44. `[[44. Update AuthController — Semua Endpoint Auth Lengkap]]`
    
    - `POST /auth/register` → `@Public()` → register
    - `POST /auth/login` → `@Public()` → login
    - `POST /auth/refresh` → `@UseGuards(JwtRefreshGuard)` → refresh
    - `POST /auth/logout` → dilindungi JWT guard → logout
    - `GET /auth/me` → dilindungi JWT guard → return current user
    - _Langkah konkret_: Test semua endpoint dengan urutan: register → login → akses protected → refresh → logout

---

### K. Role-Based Authorization

> 💡 **Benang Merah ke Guards**: `JwtAuthGuard` memastikan user teridentifikasi. `RolesGuard` memastikan user memiliki role yang tepat. Keduanya bekerja bersama dalam pipeline.

text

```
Benang Merah Bagian K:
JwtAuthGuard: siapa user ini? (Poin 42) →
RolesGuard: apakah user punya izin yang cukup? →
@Roles() decorator: tentukan role yang diizinkan per endpoint →
Update BooksController: CRUD butuh role berbeda
```

45. `[[45. Membuat @Roles() Decorator & RolesGuard]]`
    
    - Buat `src/common/decorators/roles.decorator.ts`:
        
        TypeScript
        
        ```
        export const Roles = (...roles: Role[]) => SetMetadata('roles', roles);
        ```
        
    - Buat `src/common/guards/roles.guard.ts`:
        
        TypeScript
        
        ```
        @Injectable()
        export class RolesGuard implements CanActivate {
          constructor(private reflector: Reflector) {}
        
          canActivate(context: ExecutionContext): boolean {
            const requiredRoles = this.reflector.getAllAndOverride<Role[]>('roles', [
              context.getHandler(),
              context.getClass(),
            ]);
            if (!requiredRoles) return true;
        
            const { user } = context.switchToHttp().getRequest();
            return requiredRoles.includes(user.role);
          }
        }
        ```
        
    - Daftarkan sebagai global guard (urutan penting: JwtAuth dulu, baru Roles)
    - _Langkah konkret_: Test akses endpoint admin dengan user biasa — harus dapat 403
46. `[[46. Update BooksController — Terapkan Auth & Roles]]`
    
    - Atur akses per endpoint:
        
        TypeScript
        
        ```
        @Controller('books')
        export class BooksController {
          // GET /books — semua boleh akses
          @Public()
          @Get()
          findAll() {}
        
          // GET /books/:id — semua boleh akses
          @Public()
          @Get(':id')
          findById() {}
        
          // POST /books — hanya LIBRARIAN dan ADMIN
          @Roles(Role.LIBRARIAN, Role.ADMIN)
          @Post()
          create() {}
        
          // PATCH /books/:id — hanya LIBRARIAN dan ADMIN
          @Roles(Role.LIBRARIAN, Role.ADMIN)
          @Patch(':id')
          update() {}
        
          // DELETE /books/:id — hanya ADMIN
          @Roles(Role.ADMIN)
          @Delete(':id')
          remove() {}
        }
        ```
        
    - _Langkah konkret_: Test semua kombinasi role dan endpoint, dokumentasikan hasilnya

---

### L. Swagger — Dokumentasi API yang Lengkap

> 💡 **Benang Merah**: Semua DTO sudah dibuat. Sekarang kita tambahkan dekorator Swagger ke DTO dan controller — dokumentasi selesai tanpa menulis ulang apapun.

text

```
Benang Merah Bagian L:
DTO sudah ada (Poin 9, 36, 37) →
@ApiProperty() di DTO: dokumentasikan field →
@ApiOperation() di controller: dokumentasikan endpoint →
@ApiResponse() di controller: dokumentasikan response →
@ApiBearerAuth(): endpoint yang butuh token →
Swagger UI: dokumentasi interaktif siap digunakan
```

47. `[[47. Setup Swagger di main.ts — Konfigurasi Dasar]]`
    
    - Install: `pnpm add @nestjs/swagger`
    - Setup di `main.ts`:
        
        TypeScript
        
        ```
        const config = new DocumentBuilder()
          .setTitle('Perpustakaan API')
          .setDescription('REST API untuk sistem manajemen perpustakaan')
          .setVersion('1.0')
          .addBearerAuth(
            { type: 'http', scheme: 'bearer', bearerFormat: 'JWT' },
            'access-token',
          )
          .build();
        
        const document = SwaggerModule.createDocument(app, config);
        SwaggerModule.setup('api/docs', app, document);
        ```
        
    - _Langkah konkret_: Buka `http://localhost:3000/api/docs` — Swagger UI muncul
48. `[[48. Mendokumentasikan DTO dengan @ApiProperty()]]`
    
    - Tambahkan ke `CreateBookDto`:
        
        TypeScript
        
        ```
        export class CreateBookDto {
          @ApiProperty({ example: 'Clean Code', description: 'Judul buku' })
          @IsString() @IsNotEmpty()
          title: string;
        
          @ApiProperty({ example: 'Robert C. Martin' })
          @IsString() @IsNotEmpty()
          author: string;
        
          @ApiPropertyOptional({ example: 2008, minimum: 1900, maximum: 2024 })
          @IsOptional() @IsInt() @Min(1900) @Max(2024)
          year?: number;
        }
        ```
        
    - Lakukan hal sama untuk semua DTO (Auth, User, Query)
    - _Langkah konkret_: Swagger UI menampilkan schema DTO dengan contoh yang benar
49. `[[49. Mendokumentasikan Controller dengan @ApiTags, @ApiOperation & @ApiResponse]]`
    
    - Update `BooksController`:
        
        TypeScript
        
        ```
        @ApiTags('Books')
        @Controller('books')
        export class BooksController {
          @ApiOperation({ summary: 'Mendapatkan daftar semua buku' })
          @ApiResponse({ status: 200, description: 'Daftar buku berhasil diambil', type: PaginatedBooksResponseDto })
          @ApiResponse({ status: 400, description: 'Parameter tidak valid' })
          @Get()
          findAll() {}
        
          @ApiOperation({ summary: 'Menambahkan buku baru' })
          @ApiResponse({ status: 201, description: 'Buku berhasil ditambahkan' })
          @ApiResponse({ status: 409, description: 'ISBN sudah terdaftar' })
          @ApiBearerAuth('access-token')
          @Post()
          create() {}
        }
        ```
        
    - _Langkah konkret_: Semua endpoint terdokumentasi, bisa di-test dari Swagger UI
50. `[[50. Test API dari Swagger UI — Authorize & Test Endpoint]]`
    
    - Login via `POST /auth/login` di Swagger
    - Klik "Authorize", masukkan access token
    - Test semua endpoint dari Swagger UI
    - _Langkah konkret_: Dokumentasikan skenario test di Swagger (screenshot)

---

### 🏗️ Checkpoint Level 3

text

```
✅ Checklist sebelum lanjut ke Level 4:
├── Model User di database dengan enum Role
├── Register dan login berfungsi
├── JWT access token (15 menit) dan refresh token (7 hari)
├── Token rotation pada refresh
├── JwtAuthGuard global (semua route protected)
├── @Public() decorator untuk route publik
├── RolesGuard global dengan @Roles() decorator
├── @CurrentUser() decorator berfungsi
├── Books endpoint dengan role yang tepat
├── Swagger UI lengkap dan bisa digunakan untuk test
└── .env.example diupdate dengan variabel baru

Commit: feat: add authentication and role-based authorization
```

---

## 🟠 LEVEL 4: FITUR PRODUCTION-GRADE (Minggu 15-24)

> **Tema**: _"Menambahkan fitur yang membuat aplikasi siap untuk pengguna nyata"_  
> **Benang Merah**: API yang sudah aman (Level 3) → tambahkan fitur dunia nyata → Caching → File upload → Background jobs → Email  
> **Output**: Perpustakaan API dengan fitur lengkap: upload cover buku, email konfirmasi, notifikasi, peminjaman buku

---

### M. Tambah Fitur Domain — Sistem Peminjaman Buku

> 💡 **Mengapa ini dulu sebelum cache/queue?** Kita perlu fitur yang kompleks dulu, baru kita optimize. Sistem peminjaman butuh:
> 
> - Cek stok sebelum pinjam (business rule)
> - Email konfirmasi setelah pinjam (queue)
> - Notifikasi jatuh tempo (scheduled job)

text

```
Benang Merah Bagian M:
Books CRUD sudah ada (Level 1-2) →
Tambah model Loan di schema Prisma →
LoanModule dengan repository pattern →
Business logic: cek stok, update stok →
Endpoint: pinjam, kembalikan, history
```

51. `[[51. Tambah Model Loan di Prisma Schema]]`
    
    - Update `schema.prisma`:
        
        prisma
        
        ```
        model Loan {
          id         String     @id @default(cuid())
          userId     String
          bookId     String
          borrowedAt DateTime   @default(now())
          dueDate    DateTime
          returnedAt DateTime?
          status     LoanStatus @default(ACTIVE)
        
          user User @relation(fields: [userId], references: [id])
          book Book @relation(fields: [bookId], references: [id])
        
          @@map("loans")
        }
        
        enum LoanStatus {
          ACTIVE
          RETURNED
          OVERDUE
        }
        ```
        
    - Tambahkan relasi di model `User` dan `Book`:
        
        prisma
        
        ```
        // di model User
        loans Loan[]
        
        // di model Book
        loans Loan[]
        ```
        
    - Jalankan: `npx prisma migrate dev --name add-loan-model`
    - _Langkah konkret_: Verifikasi tabel `loans` dengan foreign keys yang benar
52. `[[52. Membuat LoansModule — Service, Repository & Controller]]`
    
    - Generate: `nest g module modules/loans`, `controller`, `service`
    - Buat `ILoansRepository` interface
    - Buat `PrismaLoansRepository` dengan query yang menginclude relasi `user` dan `book`
    - Implementasikan `LoansService`:
        
        TypeScript
        
        ```
        async borrowBook(userId: string, dto: CreateLoanDto) {
          // 1. Cek buku ada
          const book = await this.booksService.findById(dto.bookId);
        
          // 2. Cek stok
          if (book.stock < 1) {
            throw new BadRequestException('Book is out of stock');
          }
        
          // 3. Cek user belum pinjam buku yang sama
          const existing = await this.loansRepository.findActiveByUserAndBook(
            userId, dto.bookId
          );
          if (existing) {
            throw new ConflictException('You already have this book');
          }
        
          // 4. Buat loan dan kurangi stok (dalam transaction)
          const loan = await this.loansRepository.createWithStockUpdate(
            userId, dto.bookId
          );
        
          return loan;
        }
        ```
        
    - _Langkah konkret_: Test pinjam buku, verifikasi stok berkurang di database
53. `[[53. Endpoint Peminjaman — Pinjam, Kembalikan & Riwayat]]`
    
    - Definisikan routes di `LoansController`:
        - `POST /loans` — pinjam buku (butuh auth, role USER)
        - `PATCH /loans/:id/return` — kembalikan buku
        - `GET /loans/my` — riwayat peminjaman saya
        - `GET /loans` — semua peminjaman (admin/librarian only)
        - `GET /loans/overdue` — daftar yang overdue (admin/librarian)
    - _Langkah konkret_: Test semua endpoint, verifikasi business rules diterapkan

---

### N. Caching dengan Redis

> 💡 **Benang Merah ke Database**: Setiap request `GET /books` memukul database. Dengan 1000 request/menit, database bisa kewalahan. Redis menyimpan hasil query sehingga database tidak perlu dipukul berulang.

text

```
Benang Merah Bagian N:
Database query berulang (Level 2) →
Redis sudah berjalan di Docker Compose (Poin 17) →
CacheModule: integrasikan Redis ke NestJS →
Cache books list: query database 1x, sisanya dari cache →
Cache invalidation: clear cache saat data berubah
```

54. `[[54. Setup CacheModule dengan Redis]]`
    
    - Install: `pnpm add @nestjs/cache-manager cache-manager-ioredis-yet`
    - Setup di `AppModule`:
        
        TypeScript
        
        ```
        CacheModule.registerAsync({
          isGlobal: true,
          inject: [ConfigService],
          useFactory: (config: ConfigService) => ({
            store: redisStore,
            host: config.get('redis.host'),
            port: config.get('redis.port'),
            ttl: 60, // default 60 detik
          }),
        }),
        ```
        
    - Tambahkan namespace config `redis.config.ts`
    - _Langkah konkret_: Test koneksi Redis, verifikasi tidak ada error saat startup
55. `[[55. Implementasi Cache di BooksService — Cache-Aside Pattern]]`
    
    - Inject `CACHE_MANAGER` di `BooksService`:
        
        TypeScript
        
        ```
        constructor(
          @Inject(BOOKS_REPOSITORY) private readonly booksRepository: IBooksRepository,
          @Inject(CACHE_MANAGER) private readonly cacheManager: Cache,
        ) {}
        
        async findAll(query: QueryBookDto) {
          const cacheKey = `books:${JSON.stringify(query)}`;
        
          // 1. Cek cache
          const cached = await this.cacheManager.get(cacheKey);
          if (cached) return cached;
        
          // 2. Query database
          const result = await this.booksRepository.findAll(query);
        
          // 3. Simpan ke cache (5 menit)
          await this.cacheManager.set(cacheKey, result, 300);
        
          return result;
        }
        ```
        
    - _Langkah konkret_: Gunakan Redis CLI atau GUI untuk lihat key yang tersimpan
56. `[[56. Cache Invalidation — Clear Cache saat Data Berubah]]`
    
    - Buat helper method `clearBooksCache()`:
        
        TypeScript
        
        ```
        private async clearBooksCache() {
          const keys = await this.cacheManager.store.keys('books:*');
          await Promise.all(keys.map(key => this.cacheManager.del(key)));
        }
        ```
        
    - Panggil di setiap mutation:
        
        TypeScript
        
        ```
        async create(dto: CreateBookDto) {
          const book = await this.booksRepository.create(dto);
          await this.clearBooksCache();
          return book;
        }
        ```
        
    - _Langkah konkret_: Verifikasi cache ter-invalidate setelah create/update/delete buku
57. `[[57. Cache untuk Endpoint yang Spesifik — @CacheKey & @CacheTTL]]`
    
    - Gunakan `@UseInterceptors(CacheInterceptor)` untuk caching otomatis
    - `@CacheKey('all-books')`: key tetap (tidak bergantung query params)
    - `@CacheTTL(60)`: override TTL default
    - Kapan pakai automatic vs manual cache
    - _Langkah konkret_: Buat endpoint `GET /books/featured` dengan cache 1 jam

---

### O. Background Jobs dengan BullMQ

> 💡 **Benang Merah ke Peminjaman**: Saat user meminjam buku, kita perlu:
> 
> 1. Kirim email konfirmasi (tidak boleh memblokir response)
> 2. Jadwalkan reminder 1 hari sebelum jatuh tempo
> 3. Cron job harian untuk cek dan update buku overdue  
>     Queue adalah solusinya.

text

```
Benang Merah Bagian O:
Redis sudah berjalan (Poin 17) →
BullMQ menggunakan Redis sebagai storage →
Queue: tempat menampung jobs →
Worker/Processor: yang mengerjakan jobs →
Loans service: dispatch job setelah pinjam →
Jobs: email konfirmasi, reminder, update overdue
```

58. `[[58. Setup BullMQ di NestJS]]`
    
    - Install: `pnpm add @nestjs/bullmq bullmq`
    - Buat `QueuesModule` yang mendaftarkan semua queue:
        
        TypeScript
        
        ```
        @Module({
          imports: [
            BullModule.forRootAsync({
              inject: [ConfigService],
              useFactory: (config: ConfigService) => ({
                connection: {
                  host: config.get('redis.host'),
                  port: config.get('redis.port'),
                },
              }),
            }),
            BullModule.registerQueue(
              { name: 'email' },
              { name: 'notification' },
            ),
          ],
          exports: [BullModule],
        })
        export class QueuesModule {}
        ```
        
    - _Langkah konkret_: Import `QueuesModule` di `AppModule`, verifikasi tidak ada error
59. `[[59. Membuat Email Queue Processor — Konsumer Job Email]]`
    
    - Buat `src/modules/email/processors/email.processor.ts`:
        
        TypeScript
        
        ```
        @Processor('email')
        export class EmailProcessor extends WorkerHost {
          private readonly logger = new Logger(EmailProcessor.name);
        
          @Process('send-loan-confirmation')
          async handleLoanConfirmation(job: Job<LoanConfirmationPayload>) {
            this.logger.log(`Processing email job ${job.id}`);
            const { userEmail, userName, bookTitle, dueDate } = job.data;
        
            await this.emailService.sendLoanConfirmation({
              to: userEmail,
              userName,
              bookTitle,
              dueDate,
            });
        
            this.logger.log(`Email sent to ${userEmail}`);
          }
        
          @OnQueueFailed()
          onFailed(job: Job, error: Error) {
            this.logger.error(`Job ${job.id} failed: ${error.message}`);
          }
        }
        ```
        
    - _Langkah konkret_: Processor terdaftar, job yang masuk ke queue `email` akan diproses
60. `[[60. Dispatch Job dari LoansService — Kirim Email Setelah Pinjam]]`
    
    - Inject `email` queue di `LoansService`:
        
        TypeScript
        
        ```
        constructor(
          @InjectQueue('email') private readonly emailQueue: Queue,
          // ...
        ) {}
        
        async borrowBook(userId: string, dto: CreateLoanDto) {
          // ... logika borrowBook yang sudah ada
        
          // Dispatch email job (non-blocking)
          await this.emailQueue.add('send-loan-confirmation', {
            userEmail: user.email,
            userName: user.name,
            bookTitle: book.title,
            dueDate: loan.dueDate,
          }, {
            attempts: 3,
            backoff: { type: 'exponential', delay: 5000 },
          });
        
          return loan;
        }
        ```
        
    - _Langkah konkret_: Pinjam buku, verifikasi job masuk ke queue (lihat di Redis)
61. `[[61. Scheduled Job — Cron untuk Update Status Overdue]]`
    
    - Install: `pnpm add @nestjs/schedule`
    - Buat `src/modules/loans/tasks/overdue-checker.task.ts`:
        
        TypeScript
        
        ```
        @Injectable()
        export class OverdueCheckerTask {
          constructor(private readonly loansService: LoansService) {}
        
          @Cron(CronExpression.EVERY_DAY_AT_MIDNIGHT)
          async checkOverdueLoans() {
            const overdueLoans = await this.loansService.findActiveOverdue();
            
            for (const loan of overdueLoans) {
              await this.loansService.markAsOverdue(loan.id);
              // Dispatch notifikasi overdue ke queue
            }
          }
        }
        ```
        
    - _Langkah konkret_: Test dengan cron setiap menit untuk verifikasi, ubah ke tengah malam setelah konfirmasi
62. `[[62. Setup Bull Board — Dashboard Monitoring Queue]]`
    
    - Install: `pnpm add @bull-board/nestjs @bull-board/api @bull-board/express`
    - Setup di `AppModule`
    - Protect dashboard dengan guard (hanya admin)
    - _Langkah konkret_: Buka `/queues` di browser, lihat job yang sudah diproses dan yang gagal

---

### P. Email Service

> 💡 **Benang Merah ke Queue**: Processor di Poin 59 memanggil `emailService.sendLoanConfirmation()`. Sekarang kita implementasikan EmailService yang sebenarnya — dengan template HTML dan Mailhog untuk development.

text

```
Benang Merah Bagian P:
Email processor memanggil EmailService (Poin 59) →
Nodemailer: engine pengiriman email →
Mailhog: email catcher di development →
Template Handlebars: HTML email yang konsisten →
EmailService: abstraksi di atas Nodemailer →
Metode spesifik per tipe email
```

63. `[[63. Setup Mailhog di Docker Compose — Email Catcher Development]]`
    
    - Update `docker-compose.yml`:
        
        YAML
        
        ```
        mailhog:
          image: mailhog/mailhog
          ports:
            - "1025:1025"  # SMTP
            - "8025:8025"  # Web UI
        ```
        
    - Jalankan: `docker compose up -d mailhog`
    - Buka `http://localhost:8025` — Mailhog UI
    - _Langkah konkret_: UI Mailhog kosong, siap menerima email
64. `[[64. Membuat EmailModule — Nodemailer & Konfigurasi]]`
    
    - Install: `pnpm add nodemailer handlebars`
    - Install: `pnpm add -D @types/nodemailer`
    - Buat `src/modules/email/email.module.ts`
    - Buat `src/modules/email/email.service.ts`:
        
        TypeScript
        
        ```
        @Injectable()
        export class EmailService {
          private transporter: Mail;
        
          constructor(private readonly config: ConfigService) {
            this.transporter = createTransport({
              host: config.get('email.host'),
              port: config.get('email.port'),
              auth: {
                user: config.get('email.user'),
                pass: config.get('email.pass'),
              },
            });
          }
        }
        ```
        
    - Tambahkan namespace config `email.config.ts`
    - _Langkah konkret_: Test koneksi Nodemailer ke Mailhog
65. `[[65. Membuat Template Email — HTML dengan Handlebars]]`
    
    - Buat folder `src/modules/email/templates/`
    - Buat `loan-confirmation.hbs`:
        
        HTML
        
        ```
        <!DOCTYPE html>
        <html>
        <body>
          <h1>Konfirmasi Peminjaman Buku</h1>
          <p>Halo {{userName}},</p>
          <p>Anda berhasil meminjam buku <strong>{{bookTitle}}</strong>.</p>
          <p>Batas pengembalian: <strong>{{dueDate}}</strong></p>
          <p>Terima kasih telah menggunakan perpustakaan kami!</p>
        </body>
        </html>
        ```
        
    - Buat template: `welcome.hbs`, `password-reset.hbs`, `overdue-reminder.hbs`
    - _Langkah konkret_: Test render template dengan data dummy
66. `[[66. Implementasi Method EmailService — Send Berbagai Tipe Email]]`
    
    - Implementasikan semua method:
        
        TypeScript
        
        ```
        async sendLoanConfirmation(data: LoanConfirmationData) {
          const html = await this.renderTemplate('loan-confirmation', data);
          await this.transporter.sendMail({
            from: this.config.get('email.from'),
            to: data.to,
            subject: `Konfirmasi Peminjaman: ${data.bookTitle}`,
            html,
          });
        }
        
        async sendWelcome(data: WelcomeData) { ... }
        async sendPasswordReset(data: PasswordResetData) { ... }
        async sendOverdueReminder(data: OverdueReminderData) { ... }
        ```
        
    - _Langkah konkret_: Pinjam buku, verifikasi email konfirmasi muncul di Mailhog

---

### Q. File Upload — Cover Buku

> 💡 **Benang Merah ke Books**: Setiap buku bisa punya foto cover. Upload file → simpan ke storage → update URL di database.

text

```
Benang Merah Bagian Q:
Model Book sudah ada (Level 2) →
Tambah field coverUrl di schema →
Multer: handle multipart upload →
Storage: simpan file lokal (dev) atau S3 (prod) →
Update Book: simpan URL cover →
Sharp: resize dan optimasi gambar
```

67. `[[67. Update Schema Book — Tambahkan Field coverUrl]]`
    
    - Update model `Book` di Prisma:
        
        prisma
        
        ```
        model Book {
          // ... field yang sudah ada
          coverUrl String?
        }
        ```
        
    - Jalankan: `npx prisma migrate dev --name add-book-cover`
    - _Langkah konkret_: Field `cover_url` nullable di database
68. `[[68. Konfigurasi Multer — File Upload dengan Validasi]]`
    
    - Install: `pnpm add multer sharp`
    - Install: `pnpm add -D @types/multer`
    - Buat `src/common/config/multer.config.ts`:
        
        TypeScript
        
        ```
        export const multerConfig = {
          limits: { fileSize: 2 * 1024 * 1024 }, // 2MB
          fileFilter: (req, file, callback) => {
            const allowedMimes = ['image/jpeg', 'image/png', 'image/webp'];
            if (allowedMimes.includes(file.mimetype)) {
              callback(null, true);
            } else {
              callback(
                new BadRequestException('Only JPEG, PNG, and WebP are allowed'),
                false,
              );
            }
          },
        };
        ```
        
    - _Langkah konkret_: Test upload file yang salah tipe, verifikasi error 400
69. `[[69. Endpoint Upload Cover Buku & Image Processing]]`
    
    - Tambahkan endpoint di `BooksController`:
        
        TypeScript
        
        ```
        @Roles(Role.LIBRARIAN, Role.ADMIN)
        @Post(':id/cover')
        @UseInterceptors(FileInterceptor('cover', multerConfig))
        async uploadCover(
          @Param('id') id: string,
          @UploadedFile() file: Express.Multer.File,
        ) {
          return this.booksService.uploadCover(id, file);
        }
        ```
        
    - Implementasikan `BooksService.uploadCover()`:
        
        TypeScript
        
        ```
        async uploadCover(bookId: string, file: Express.Multer.File) {
          // 1. Resize gambar
          const resized = await sharp(file.buffer)
            .resize(400, 600, { fit: 'cover' })
            .webp({ quality: 80 })
            .toBuffer();
        
          // 2. Simpan ke disk (nanti ganti ke S3)
          const filename = `${bookId}-cover.webp`;
          const filepath = path.join('uploads', filename);
          await fs.writeFile(filepath, resized);
        
          // 3. Update URL di database
          const coverUrl = `/uploads/${filename}`;
          return this.booksRepository.update(bookId, { coverUrl });
        }
        ```
        
    - _Langkah konkret_: Upload cover, verifikasi gambar ter-resize dan URL tersimpan di database

---

### 🏗️ Checkpoint Level 4

text

```
✅ Checklist sebelum lanjut ke Level 5:
├── Sistem peminjaman (Loan) dengan business rules
├── Redis caching untuk daftar buku
├── Cache invalidation yang benar
├── BullMQ queue untuk email
├── Email confirmation dikirim saat pinjam
├── Scheduled cron untuk update overdue
├── Bull Board dashboard (protected)
├── Email service dengan Nodemailer + Mailhog
├── Template email HTML dengan Handlebars
├── Upload cover buku dengan validasi dan resize
├── File tersimpan dan URL ter-update di database
└── Semua fitur baru terdokumentasi di Swagger

Commit: feat: add caching, background jobs, email, and file upload
```

---

## 🔴 LEVEL 5: TESTING KOMPREHENSIF (Minggu 24-32)

> **Tema**: _"Dari kode yang berjalan ke kode yang bisa dipercaya"_  
> **Benang Merah**: Semua fitur yang dibangun (Level 1-4) → audit test coverage → tulis test yang hilang → CI pipeline  
> **Output**: Test coverage > 80%, CI pipeline yang otomatis menjalankan test

---

### R. Unit Testing — Isolasi & Mock

> 💡 **Benang Merah ke Repository Pattern**: Repository pattern (Poin 23-25) bukan hanya untuk arsitektur yang bersih — ini yang membuat unit test mudah. `BooksService` menggunakan interface, bukan Prisma langsung. Jadi kita bisa mock interface tanpa butuh database.

text

```
Benang Merah Bagian R:
IBooksRepository interface (Poin 23) →
Unit test BooksService: mock interface →
Tidak perlu database untuk test service →
Test setiap branch: happy path, error case, edge case →
Coverage report: identifikasi yang belum di-test
```

70. `[[70. Setup Testing Infrastructure — Jest Config & Coverage]]`
    
    - Review `jest.config.ts` yang ada di project
    - Tambahkan coverage configuration:
        
        TypeScript
        
        ```
        // jest.config.ts
        module.exports = {
          // ...
          collectCoverageFrom: [
            'src/**/*.ts',
            '!src/**/*.module.ts',
            '!src/main.ts',
            '!src/**/*.interface.ts',
            '!src/**/*.dto.ts',
          ],
          coverageThresholds: {
            global: {
              branches: 70,
              functions: 80,
              lines: 80,
              statements: 80,
            },
          },
        };
        ```
        
    - Jalankan `pnpm test --coverage` untuk lihat baseline coverage
    - _Langkah konkret_: Screenshot coverage report awal, identifikasi yang perlu ditingkatkan
71. `[[71. Unit Test BooksService — Mock Repository dengan Jest]]`
    
    - Buat `books.service.spec.ts`:
        
        TypeScript
        
        ```
        describe('BooksService', () => {
          let service: BooksService;
          let booksRepository: jest.Mocked<IBooksRepository>;
          let cacheManager: jest.Mocked<Cache>;
        
          beforeEach(async () => {
            const mockBooksRepository = {
              findAll: jest.fn(),
              findById: jest.fn(),
              findByIsbn: jest.fn(),
              create: jest.fn(),
              update: jest.fn(),
              softDelete: jest.fn(),
            };
        
            const module = await Test.createTestingModule({
              providers: [
                BooksService,
                { provide: BOOKS_REPOSITORY, useValue: mockBooksRepository },
                { provide: CACHE_MANAGER, useValue: { get: jest.fn(), set: jest.fn(), del: jest.fn() } },
              ],
            }).compile();
        
            service = module.get<BooksService>(BooksService);
            booksRepository = module.get(BOOKS_REPOSITORY);
            cacheManager = module.get(CACHE_MANAGER);
          });
        
          describe('findById', () => {
            it('should return book when found', async () => {
              const mockBook = { id: '1', title: 'Clean Code', /* ... */ };
              booksRepository.findById.mockResolvedValue(mockBook);
        
              const result = await service.findById('1');
              
              expect(result).toEqual(mockBook);
              expect(booksRepository.findById).toHaveBeenCalledWith('1');
            });
        
            it('should throw NotFoundException when book not found', async () => {
              booksRepository.findById.mockResolvedValue(null);
        
              await expect(service.findById('999')).rejects.toThrow(NotFoundException);
            });
          });
        
          describe('create', () => {
            it('should throw ConflictException when ISBN exists', async () => {
              booksRepository.findByIsbn.mockResolvedValue({ id: '1', isbn: '123' } as any);
        
              await expect(service.create({ isbn: '123' } as any)).rejects.toThrow(ConflictException);
            });
        
            it('should create book and clear cache', async () => {
              booksRepository.findByIsbn.mockResolvedValue(null);
              booksRepository.create.mockResolvedValue({ id: '1' } as any);
        
              await service.create({ isbn: '123' } as any);
        
              expect(booksRepository.create).toHaveBeenCalled();
              expect(cacheManager.store.keys).toHaveBeenCalled();
            });
          });
        });
        ```
        
    - _Langkah konkret_: Coverage BooksService harus > 90%
72. `[[72. Unit Test AuthService — Mock UsersService & JwtService]]`
    
    - Mock semua dependency: `UsersService`, `JwtService`, `ConfigService`
    - Test semua skenario register: sukses, email duplikat
    - Test semua skenario login: sukses, email tidak ada, password salah
    - Test logout, refresh token
    - _Langkah konkret_: Coverage AuthService harus > 85%
73. `[[73. Unit Test Guards — Mock ExecutionContext]]`
    
    - Test `JwtAuthGuard`:
        
        TypeScript
        
        ```
        it('should allow access to @Public() routes', () => {
          const mockContext = createMock<ExecutionContext>();
          reflector.getAllAndOverride.mockReturnValue(true); // isPublic = true
        
          const result = guard.canActivate(mockContext);
          
          expect(result).toBe(true);
        });
        ```
        
    - Test `RolesGuard`: role cukup, role tidak cukup, tidak ada required roles
    - _Langkah konkret_: Semua branch di guard ter-cover
74. `[[74. Unit Test Exception Filter & Interceptors]]`
    
    - Test `GlobalExceptionFilter`: HTTP exception, non-HTTP exception, Prisma error
    - Test `ResponseWrapperInterceptor`: bungkus response dengan format benar
    - Test `LoggingInterceptor`: log ditulis dengan format yang benar
    - _Langkah konkret_: Filter dan interceptor ter-test secara terisolasi

---

### S. Integration Testing — Database Nyata

> 💡 **Benang Merah ke Repository**: Unit test service menggunakan mock repository. Integration test **repository** menggunakan database nyata — karena kita ingin memastikan query Prisma yang ditulis benar-benar berjalan dengan benar.

text

```
Benang Merah Bagian S:
Unit test: mock database (cepat, terisolasi) →
Integration test: database nyata (lebih lambat, lebih akurat) →
Setup database test: environment terpisah →
Testcontainers: PostgreSQL nyata di dalam test →
Seed data sebelum test, cleanup sesudah →
Test setiap query di repository
```

75. `[[75. Setup Database untuk Integration Test]]`
    
    - Buat `.env.test` dengan database URL berbeda:
        
        text
        
        ```
        DATABASE_URL="postgresql://postgres:postgres@localhost:5432/perpustakaan_test"
        ```
        
    - Update `jest.config.ts` untuk e2e test:
        
        TypeScript
        
        ```
        // jest-e2e.config.ts
        module.exports = {
          ...
          testEnvironment: 'node',
          setupFiles: ['dotenv/config'],
        };
        ```
        
    - Jalankan migration ke database test: `DATABASE_URL=... npx prisma migrate deploy`
    - _Langkah konkret_: Database test terpisah dari development
76. `[[76. Integration Test PrismaBooksRepository]]`
    
    - Buat `prisma-books.repository.spec.ts`:
        
        TypeScript
        
        ```
        describe('PrismaBooksRepository (Integration)', () => {
          let repository: PrismaBooksRepository;
          let prisma: PrismaService;
        
          beforeAll(async () => {
            const module = await Test.createTestingModule({
              providers: [PrismaService, PrismaBooksRepository],
            }).compile();
        
            repository = module.get(PrismaBooksRepository);
            prisma = module.get(PrismaService);
          });
        
          beforeEach(async () => {
            // Bersihkan database sebelum setiap test
            await prisma.book.deleteMany();
          });
        
          afterAll(async () => {
            await prisma.$disconnect();
          });
        
          it('should create and find a book', async () => {
            const created = await repository.create({
              title: 'Test Book',
              author: 'Test Author',
              isbn: '123456789',
              year: 2024,
              stock: 5,
            });
        
            const found = await repository.findById(created.id);
        
            expect(found).toMatchObject({
              title: 'Test Book',
              author: 'Test Author',
            });
          });
        });
        ```
        
    - _Langkah konkret_: Test berjalan dengan database nyata

---

### T. E2E Testing — Simulasi User Nyata

> 💡 **Benang Merah**: Unit test menguji fungsi terisolasi. Integration test menguji repository. E2E test menguji **seluruh alur dari HTTP request ke response** — termasuk middleware, guard, interceptor, service, repository, dan database.

text

```
Benang Merah Bagian T:
Unit test (Poin 70-74) + Integration test (Poin 75-76) →
E2E test: simulasi request HTTP nyata →
Supertest: buat HTTP request dalam test →
Test flow lengkap: register → login → CRUD →
Test auth: akses tanpa token, akses dengan role salah
```

77. `[[77. Setup E2E Test — NestJS App di Test Environment]]`
    
    - Buat `test/app.e2e-spec.ts`:
        
        TypeScript
        
        ```
        describe('App E2E', () => {
          let app: INestApplication;
          let prisma: PrismaService;
        
          beforeAll(async () => {
            const moduleFixture = await Test.createTestingModule({
              imports: [AppModule],
            }).compile();
        
            app = moduleFixture.createNestApplication();
            
            // Pasang semua global middleware, pipe, filter, interceptor
            app.setGlobalPrefix('api/v1');
            app.useGlobalPipes(new ValidationPipe({ whitelist: true, transform: true }));
            // ...
        
            await app.init();
        
            prisma = app.get(PrismaService);
          });
        
          afterAll(async () => {
            await prisma.user.deleteMany();
            await prisma.book.deleteMany();
            await app.close();
          });
        });
        ```
        
    - _Langkah konkret_: Setup e2e test environment, verifikasi app bisa di-init
78. `[[78. E2E Test Auth Flow — Register, Login, Refresh, Logout]]`
    
    - Tulis test untuk full auth flow:
        
        TypeScript
        
        ```
        describe('Auth Flow', () => {
          it('should register → login → get profile → logout', async () => {
            // Register
            const registerRes = await request(app.getHttpServer())
              .post('/api/v1/auth/register')
              .send({ name: 'Test User', email: 'test@test.com', password: 'Password123!' })
              .expect(201);
        
            expect(registerRes.body.data).toHaveProperty('id');
        
            // Login
            const loginRes = await request(app.getHttpServer())
              .post('/api/v1/auth/login')
              .send({ email: 'test@test.com', password: 'Password123!' })
              .expect(200);
        
            const { accessToken, refreshToken } = loginRes.body.data;
            expect(accessToken).toBeDefined();
        
            // Get Profile
            await request(app.getHttpServer())
              .get('/api/v1/auth/me')
              .set('Authorization', `Bearer ${accessToken}`)
              .expect(200);
        
            // Logout
            await request(app.getHttpServer())
              .post('/api/v1/auth/logout')
              .set('Authorization', `Bearer ${accessToken}`)
              .expect(200);
          });
        });
        ```
        
    - _Langkah konkret_: E2E test auth flow berhasil
79. `[[79. E2E Test Books — CRUD dengan Auth & Roles]]`
    
    - Test tanpa token → 401
    - Test create dengan USER role → 403
    - Test create dengan LIBRARIAN role → 201
    - Test pagination dan search
    - Test soft delete
    - _Langkah konkret_: Semua skenario auth dan roles ter-test via HTTP
80. `[[80. CI Pipeline — GitHub Actions untuk Otomatis Test]]`
    
    - Buat `.github/workflows/ci.yml`:
        
        YAML
        
        ```
        name: CI
        
        on: [push, pull_request]
        
        jobs:
          test:
            runs-on: ubuntu-latest
            
            services:
              postgres:
                image: postgres:16-alpine
                env:
                  POSTGRES_DB: perpustakaan_test
                  POSTGRES_USER: postgres
                  POSTGRES_PASSWORD: postgres
                ports:
                  - 5432:5432
              
              redis:
                image: redis:7-alpine
                ports:
                  - 6379:6379
        
            steps:
              - uses: actions/checkout@v4
              - uses: actions/setup-node@v4
                with:
                  node-version: '20'
                  cache: 'pnpm'
              
              - run: pnpm install
              - run: pnpm run lint
              - run: pnpm run test --coverage
              - run: pnpm run test:e2e
              
              - uses: codecov/codecov-action@v4
        ```
        
    - _Langkah konkret_: Push ke GitHub, verifikasi semua job hijau

---

### 🏗️ Checkpoint Level 5

text

```
✅ Checklist sebelum lanjut ke Level 6:
├── Unit test semua service (> 80% coverage)
├── Unit test guards, filters, interceptors
├── Integration test repository dengan database nyata
├── E2E test auth flow lengkap
├── E2E test books CRUD dengan auth dan roles
├── Coverage threshold dikonfigurasi di Jest
├── GitHub Actions CI berjalan otomatis
├── Build tidak bisa merge jika test gagal
└── Coverage report di-upload ke Codecov

Commit: test: add comprehensive unit, integration, and e2e tests
```

---

## ⚫ LEVEL 6: DEPLOYMENT & PRODUCTION (Minggu 32-42)

> **Tema**: _"Dari laptop ke server — aplikasi berjalan di production"_  
> **Benang Merah**: Project yang sudah tested (Level 5) → Docker container → Deploy ke VPS → Nginx → SSL → Monitoring  
> **Output**: Aplikasi live di internet dengan monitoring dan deployment otomatis

---

### U. Containerisasi dengan Docker

> 💡 **Benang Merah**: Di development kita jalankan `pnpm run start:dev`. Di production, kita tidak bisa begitu — butuh proses yang stabil, reproducible, dan terisolasi. Docker adalah solusinya.

text

```
Benang Merah Bagian U:
Development: pnpm run start:dev →
Production: butuh environment yang stabil →
Dockerfile: definisikan cara build image →
Multi-stage: image kecil dan aman →
Docker Compose production: semua service bersama →
Nginx: reverse proxy di depan NestJS
```

81. `[[81. Membuat Dockerfile untuk NestJS — Multi-Stage Build]]`
    
    - Buat `Dockerfile` di root project:
        
        Dockerfile
        
        ```
        # Stage 1: Builder
        FROM node:20-alpine AS builder
        
        WORKDIR /app
        
        # Install pnpm
        RUN corepack enable && corepack prepare pnpm@latest --activate
        
        # Copy package files
        COPY package.json pnpm-lock.yaml ./
        
        # Install dependencies (include devDependencies untuk build)
        RUN pnpm install --frozen-lockfile
        
        # Copy source code
        COPY . .
        
        # Generate Prisma Client
        RUN npx prisma generate
        
        # Build TypeScript ke JavaScript
        RUN pnpm run build
        
        # ──────────────────────────────────────
        # Stage 2: Runner (image final yang kecil)
        FROM node:20-alpine AS runner
        
        WORKDIR /app
        
        RUN corepack enable && corepack prepare pnpm@latest --activate
        
        # Copy hanya yang diperlukan dari builder
        COPY package.json pnpm-lock.yaml ./
        RUN pnpm install --frozen-lockfile --prod
        
        COPY --from=builder /app/dist ./dist
        COPY --from=builder /app/node_modules/.prisma ./node_modules/.prisma
        COPY prisma ./prisma
        
        # Non-root user untuk security
        RUN addgroup -g 1001 -S nodejs && adduser -S nestjs -u 1001
        USER nestjs
        
        EXPOSE 3000
        
        CMD ["node", "dist/main.js"]
        ```
        
    - Buat `.dockerignore`:
        
        text
        
        ```
        node_modules
        dist
        .git
        .env
        coverage
        test
        ```
        
    - _Langkah konkret_: `docker build -t perpustakaan-api .` — image berhasil dibuild
82. `[[82. Test Docker Image Secara Lokal]]`
    
    - Update `docker-compose.yml` dengan service `app`:
        
        YAML
        
        ```
        app:
          build: .
          ports:
            - "3000:3000"
          environment:
            DATABASE_URL: postgresql://postgres:postgres@postgres:5432/perpustakaan_db
            REDIS_HOST: redis
            # ... env lainnya
          depends_on:
            - postgres
            - redis
          networks:
            - app-network
        ```
        
    - Jalankan: `docker compose up --build`
    - _Langkah konkret_: App berjalan via Docker, test endpoint dari localhost
83. `[[83. Setup Nginx sebagai Reverse Proxy]]`
    
    - Tambahkan service `nginx` di `docker-compose.prod.yml`:
        
        YAML
        
        ```
        nginx:
          image: nginx:alpine
          ports:
            - "80:80"
            - "443:443"
          volumes:
            - ./nginx/nginx.conf:/etc/nginx/nginx.conf
            - ./nginx/certs:/etc/nginx/certs
          depends_on:
            - app
        ```
        
    - Buat `nginx/nginx.conf`:
        
        nginx
        
        ```
        events { worker_connections 1024; }
        
        http {
          upstream nestjs {
            server app:3000;
          }
        
          server {
            listen 80;
            server_name your-domain.com;
            
            # Redirect ke HTTPS
            return 301 https://$server_name$request_uri;
          }
        
          server {
            listen 443 ssl;
            server_name your-domain.com;
        
            ssl_certificate /etc/nginx/certs/cert.pem;
            ssl_certificate_key /etc/nginx/certs/key.pem;
        
            location / {
              proxy_pass http://nestjs;
              proxy_set_header Host $host;
              proxy_set_header X-Real-IP $remote_addr;
              proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            }
          }
        }
        ```
        
    - _Langkah konkret_: Request masuk ke port 80/443 → Nginx → NestJS di port 3000
84. `[[84. Health Check Endpoint — @nestjs/terminus]]`
    
    - Install: `pnpm add @nestjs/terminus`
    - Buat `src/modules/health/health.module.ts`
    - Buat `HealthController`:
        
        TypeScript
        
        ```
        @Controller('health')
        export class HealthController {
          constructor(
            private health: HealthCheckService,
            private db: PrismaHealthIndicator,
            private redis: MicroserviceHealthIndicator,
          ) {}
        
          @Public()
          @Get()
          @HealthCheck()
          check() {
            return this.health.check([
              () => this.db.pingCheck('database'),
              () => this.redis.pingCheck('redis'),
            ]);
          }
        }
        ```
        
    - _Langkah konkret_: `GET /api/v1/health` mengembalikan status semua dependency

---

### V. Logging & Monitoring

> 💡 **Benang Merah ke Logging Interceptor**: Di Level 1 kita buat `LoggingInterceptor` dengan `console.log`. Di production, kita butuh structured logging yang bisa di-query dan di-monitor.

text

```
Benang Merah Bagian V:
LoggingInterceptor dengan console.log (Poin 15) →
Production: butuh structured logging →
Winston: JSON logging ke file dan console →
Correlation ID: trace request across logs →
Sentry: error tracking di production →
PM2: process manager
```

85. `[[85. Setup Winston — Structured Logging untuk Production]]`
    
    - Install: `pnpm add winston nest-winston`
    - Setup `WinstonModule` di `AppModule`:
        
        TypeScript
        
        ```
        WinstonModule.forRoot({
          transports: [
            new winston.transports.Console({
              format: winston.format.combine(
                winston.format.timestamp(),
                winston.format.colorize(),
                winston.format.printf(({ timestamp, level, message, context, ...meta }) => {
                  return `${timestamp} [${level}] [${context}] ${message} ${JSON.stringify(meta)}`;
                }),
              ),
            }),
            new winston.transports.File({
              filename: 'logs/error.log',
              level: 'error',
              format: winston.format.combine(
                winston.format.timestamp(),
                winston.format.json(),
              ),
            }),
          ],
        }),
        ```
        
    - _Langkah konkret_: Log muncul dalam format terstruktur di console dan file
86. `[[86. Correlation ID — Trace Request dari Awal hingga Akhir]]`
    
    - Install: `pnpm add nestjs-cls`
    - Setup `ClsModule` sebagai global
    - Update middleware untuk generate dan simpan Correlation ID:
        
        TypeScript
        
        ```
        @Injectable()
        export class CorrelationIdMiddleware implements NestMiddleware {
          constructor(private readonly cls: ClsService) {}
        
          use(req: Request, res: Response, next: NextFunction) {
            const correlationId = req.headers['x-correlation-id'] as string || uuidv4();
            this.cls.set('correlationId', correlationId);
            res.setHeader('X-Correlation-ID', correlationId);
            next();
          }
        }
        ```
        
    - Update logger untuk menyertakan Correlation ID di setiap log
    - _Langkah konkret_: Setiap log memiliki `correlationId` yang sama untuk request yang sama
87. `[[87. Setup Sentry — Error Tracking di Production]]`
    
    - Install: `pnpm add @sentry/node @sentry/profiling-node`
    - Inisialisasi Sentry di `main.ts` sebelum semua yang lain
    - Update `GlobalExceptionFilter` untuk capture error ke Sentry
    - _Langkah konkret_: Trigger error 500, verifikasi muncul di Sentry dashboard

---

### W. CI/CD Pipeline — Deployment Otomatis

> 💡 **Benang Merah ke CI (Poin 80)**: CI pipeline sudah berjalan untuk test. Sekarang kita tambahkan CD — Continuous Deployment: jika test sukses dan merge ke `main`, otomatis deploy ke server.

text

```
Benang Merah Bagian W:
CI pipeline (Poin 80): test otomatis →
Tambahkan CD: build Docker image →
Push ke registry (GitHub Container Registry) →
Deploy ke VPS: pull image terbaru + restart container →
Zero-downtime: rolling update
```

88. `[[88. Update GitHub Actions — Build & Push Docker Image]]`
    
    - Update `.github/workflows/ci.yml`:
        
        YAML
        
        ```
        jobs:
          test:
            # ... (sudah ada dari Poin 80)
        
          build-and-push:
            needs: test
            runs-on: ubuntu-latest
            if: github.ref == 'refs/heads/main'
            
            steps:
              - uses: actions/checkout@v4
              
              - name: Login to GitHub Container Registry
                uses: docker/login-action@v3
                with:
                  registry: ghcr.io
                  username: ${{ github.actor }}
                  password: ${{ secrets.GITHUB_TOKEN }}
              
              - name: Build and Push
                uses: docker/build-push-action@v5
                with:
                  push: true
                  tags: ghcr.io/${{ github.repository }}:latest
        ```
        
    - _Langkah konkret_: Merge ke main → image ter-push ke GitHub Container Registry
89. `[[89. Deploy ke VPS — SSH & Docker Pull]]`
    
    - Tambahkan job `deploy` ke GitHub Actions:
        
        YAML
        
        ```
        deploy:
          needs: build-and-push
          runs-on: ubuntu-latest
          if: github.ref == 'refs/heads/main'
          
          steps:
            - name: Deploy to VPS
              uses: appleboy/ssh-action@master
              with:
                host: ${{ secrets.VPS_HOST }}
                username: ${{ secrets.VPS_USER }}
                key: ${{ secrets.VPS_SSH_KEY }}
                script: |
                  cd /app/perpustakaan
                  docker compose -f docker-compose.prod.yml pull
                  docker compose -f docker-compose.prod.yml up -d --no-deps app
                  docker compose -f docker-compose.prod.yml exec app npx prisma migrate deploy
                  docker image prune -f
        ```
        
    - Setup VPS: install Docker, clone repo, setup `docker-compose.prod.yml` dan `.env`
    - _Langkah konkret_: Push ke main → test → build image → deploy otomatis ke VPS
90. `[[90. SSL dengan Let's Encrypt & Certbot]]`
    
    - Tambahkan Certbot di Docker Compose:
        
        YAML
        
        ```
        certbot:
          image: certbot/certbot
          volumes:
            - ./certbot/conf:/etc/letsencrypt
            - ./certbot/www:/var/www/certbot
        ```
        
    - Jalankan: `docker compose run certbot certonly --webroot -w /var/www/certbot -d your-domain.com`
    - Update Nginx config untuk menggunakan certificate dari Certbot
    - Setup auto-renewal: cron job untuk `certbot renew`
    - _Langkah konkret_: Website bisa diakses via HTTPS dengan certificate yang valid

---

### 🏗️ Checkpoint Level 6

text

```
✅ Checklist sebelum lanjut ke Level 7 (opsional):
├── Dockerfile multi-stage yang optimal
├── Docker Compose untuk production
├── Nginx sebagai reverse proxy
├── SSL dengan Let's Encrypt
├── Health check endpoint
├── Structured logging dengan Winston
├── Correlation ID di semua log
├── Sentry untuk error tracking
├── CI/CD pipeline: test → build → deploy otomatis
├── Zero-downtime deployment
├── VPS berjalan dengan HTTPS
└── Dokumentasi deployment di README.md

Commit: feat: containerize and deploy to production with CI/CD
```

---

## 🟣 LEVEL 7: SPESIALISASI (Pilih Jalur) (Minggu 42+)

> **Tema**: _"Mengembangkan project ke arsitektur yang lebih advanced"_  
> **Catatan**: Pilih SATU jalur, kuasai dalam. Project perpustakaan menjadi base untuk dikembangkan.

---

### Jalur A: Performance & Scaling

> 💡 **Benang Merah**: Aplikasi sudah berjalan di production (Level 6). Sekarang kita optimize untuk menangani lebih banyak user.

text

```
Benang Merah Jalur A:
Aplikasi production (Level 6) →
Load test: temukan bottleneck →
Fastify: ganti adapter untuk throughput lebih tinggi →
Database optimization: index, query analysis →
Horizontal scaling: multiple instance + Redis untuk state →
CDN: static assets tidak perlu ke server
```

91. `[[91. Load Testing dengan k6 — Temukan Bottleneck]]`
    
    - Install k6 sebagai tool load testing
    - Buat `tests/load/books.test.js`:
        
        JavaScript
        
        ```
        import http from 'k6/http';
        import { check, sleep } from 'k6';
        
        export const options = {
          stages: [
            { duration: '30s', target: 50 },   // Ramp up ke 50 user
            { duration: '1m', target: 100 },   // Pertahankan 100 user
            { duration: '30s', target: 0 },    // Ramp down
          ],
          thresholds: {
            http_req_duration: ['p(95)<500'],  // 95% request < 500ms
            http_req_failed: ['rate<0.01'],    // Error rate < 1%
          },
        };
        
        export default function () {
          const res = http.get('https://your-domain.com/api/v1/books');
          check(res, { 'status was 200': (r) => r.status === 200 });
          sleep(1);
        }
        ```
        
    - Analisis hasil: mana yang paling lambat?
    - _Langkah konkret_: Identifikasi 3 endpoint paling lambat
92. `[[92. Ganti ke Fastify Adapter — Throughput Lebih Tinggi]]`
    
    - Install: `pnpm add @nestjs/platform-fastify`
    - Update `main.ts`:
        
        TypeScript
        
        ```
        const app = await NestFactory.create<NestFastifyApplication>(
          AppModule,
          new FastifyAdapter(),
        );
        ```
        
    - Perbaiki incompatibility: beberapa Multer config berbeda
    - Benchmark: Express vs Fastify (jalankan k6 lagi)
    - _Langkah konkret_: Dokumentasikan peningkatan performa
93. `[[93. Database Indexing & Query Optimization]]`
    
    - Aktifkan query logging Prisma untuk identifikasi slow query
    - Tambahkan index di schema untuk field yang sering di-filter/sort:
        
        prisma
        
        ```
        model Book {
          // ...
          @@index([title])
          @@index([author])
          @@index([createdAt])
        }
        ```
        
    - Gunakan `EXPLAIN ANALYZE` di PostgreSQL untuk analisis query plan
    - Fix N+1 problem jika ada
    - _Langkah konkret_: Query yang lambat sekarang < 10ms

---

### Jalur B: Microservices

> 💡 **Benang Merah**: Perpustakaan API sudah ada. Kita pecah menjadi microservices: `books-service`, `users-service`, `loans-service`, `notification-service`.

94. `[[94. Rancang Arsitektur Microservices — Identifikasi Service Boundaries]]`
    
    - Buat diagram: service apa saja, komunikasi apa
    - API Gateway: entry point tunggal
    - Service komunikasi: sinkron (gRPC/TCP) vs asinkron (RabbitMQ)
    - Data isolation: setiap service punya database sendiri
    - _Langkah konkret_: Architectural Decision Record (ADR) untuk setiap keputusan
95. `[[95. Buat Books Microservice — NestJS Transport TCP]]`
    
    - Clone project, buat sebagai microservice:
        
        TypeScript
        
        ```
        // books-service/src/main.ts
        const app = await NestFactory.createMicroservice<MicroserviceOptions>(AppModule, {
          transport: Transport.TCP,
          options: { host: '0.0.0.0', port: 3001 },
        });
        ```
        
    - Ganti controller dengan message pattern handler:
        
        TypeScript
        
        ```
        @MessagePattern('books.findAll')
        findAll(@Payload() query: QueryBookDto) {
          return this.booksService.findAll(query);
        }
        ```
        
    - _Langkah konkret_: Books service berjalan sebagai microservice
96. `[[96. API Gateway — Route ke Semua Microservices]]`
    
    - Buat project baru sebagai API Gateway
    - Register microservice clients:
        
        TypeScript
        
        ```
        ClientsModule.register([
          {
            name: 'BOOKS_SERVICE',
            transport: Transport.TCP,
            options: { host: 'books-service', port: 3001 },
          },
        ])
        ```
        
    - Route request dari gateway ke service yang tepat
    - _Langkah konkret_: Request ke gateway `GET /books` → diteruskan ke books-service
97. `[[97. Event-Driven dengan RabbitMQ — Komunikasi Async Antar Service]]`
    
    - Install RabbitMQ di Docker Compose
    - Saat buku dipinjam di loans-service → emit event `loan.created`
    - Notification-service subscribe event → kirim email konfirmasi
    - Decoupling: loans-service tidak tahu tentang email
    - _Langkah konkret_: Pinjam buku → event dikirim → email terkirim tanpa dependency langsung

---

### Jalur C: GraphQL

> 💡 **Benang Merah**: Perpustakaan API sudah REST. Tambahkan GraphQL sebagai alternatif API yang lebih fleksibel untuk client.

98. `[[98. Setup GraphQL di NestJS — Code-First Approach]]`
    
    - Install: `pnpm add @nestjs/graphql @nestjs/apollo @apollo/server graphql`
    - Setup `GraphQLModule` di `AppModule`:
        
        TypeScript
        
        ```
        GraphQLModule.forRoot<ApolloDriverConfig>({
          driver: ApolloDriver,
          autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
          sortSchema: true,
          playground: process.env.NODE_ENV !== 'production',
          context: ({ req }) => ({ req }),
        }),
        ```
        
    - _Langkah konkret_: GraphQL Playground tersedia di `/graphql`
99. `[[99. Membuat Book Type & Resolver — Query dan Mutation]]`
    
    - Buat `book.type.ts`:
        
        TypeScript
        
        ```
        @ObjectType()
        export class BookType {
          @Field(() => ID) id: string;
          @Field() title: string;
          @Field() author: string;
          @Field(() => Int) stock: number;
          @Field() createdAt: Date;
        }
        ```
        
    - Buat `books.resolver.ts`:
        
        TypeScript
        
        ```
        @Resolver(() => BookType)
        export class BooksResolver {
          constructor(private readonly booksService: BooksService) {}
        
          @Query(() => [BookType], { name: 'books' })
          findAll(@Args() query: QueryBooksArgs) {
            return this.booksService.findAll(query);
          }
        
          @Mutation(() => BookType)
          @UseGuards(GqlJwtAuthGuard, GqlRolesGuard)
          @Roles(Role.LIBRARIAN, Role.ADMIN)
          createBook(@Args('createBookInput') dto: CreateBookInput) {
            return this.booksService.create(dto);
          }
        }
        ```
        
    - _Langkah konkret_: Query books via GraphQL Playground
100. `[[100. DataLoader — Solve N+1 Problem di GraphQL]]`
    
    - Install: `pnpm add dataloader`
    - Buat `BooksDataLoader`:
        
        TypeScript
        
        ```
        @Injectable()
        export class BooksDataLoader {
          constructor(private readonly booksService: BooksService) {}
        
          readonly batchBooks = new DataLoader<string, Book>(
            async (bookIds) => {
              const books = await this.booksService.findByIds([...bookIds]);
              return bookIds.map(id => books.find(b => b.id === id));
            }
          );
        }
        ```
        
    - Gunakan DataLoader di resolver untuk field yang bisa menyebabkan N+1
    - _Langkah konkret_: Query loans dengan buku — tidak ada N+1 query ke database

---

## 📊 Ringkasan Project & Progress Tracking

### Satu Project, 7 Level Enhancement

text

```
Level 1: Perpustakaan API (CRUD in-memory, struktur dasar)
  + Level 2: + PostgreSQL + Prisma + Repository Pattern
  + Level 3: + Auth (JWT) + Roles + Swagger
  + Level 4: + Caching + Queue + Email + Upload
  + Level 5: + Testing Komprehensif + CI Pipeline
  + Level 6: + Docker + Nginx + SSL + CD Pipeline
  + Level 7: + Scaling / Microservices / GraphQL (pilih jalur)
```

### Progress Tracking

|Level|Poin|Durasi|Output Konkret|
|---|---|---|---|
|🟢 **1**|1-16|Minggu 1-3|API CRUD berjalan di local|
|🔵 **2**|17-32|Minggu 3-8|Database PostgreSQL + Repository|
|🟡 **3**|33-50|Minggu 8-15|Auth lengkap + Swagger|
|🟠 **4**|51-69|Minggu 15-24|Cache + Queue + Email + Upload|
|🔴 **5**|70-80|Minggu 24-32|Test coverage > 80% + CI|
|⚫ **6**|81-90|Minggu 32-42|Live di internet via HTTPS|
|🟣 **7**|91-100|Minggu 42+|Spesialisasi pilihan|

---

### Benang Merah Utama Sepanjang Roadmap

text

```
Poin 6  (Folder structure)    → Semua module mengikuti struktur ini
Poin 7  (Pola generate module)→ Poin 34, 35, 52 (module baru)
Poin 17 (Docker Compose)      → Poin 54 (Redis), 58 (BullMQ), 63 (Mailhog)
Poin 23 (Interface Repository)→ Poin 71 (unit test dengan mock)
Poin 27 (ConfigModule)        → Semua service gunakan ConfigService
Poin 42 (JwtAuthGuard global) → Semua route protected by default
Poin 54 (CacheModule)         → Poin 58 (BullMQ pakai Redis yang sama)
Poin 70 (Jest setup)          → Poin 80 (CI pipeline jalankan test)
Poin 81 (Dockerfile)          → Poin 88, 89 (CI/CD build dan deploy image)
```

---

## 💡 Cara Menggunakan Roadmap Ini

text

```
Setiap poin mengikuti format:
┌──────────────────────────────────────────────────────┐
│ 💡 Konteks: mengapa langkah ini perlu dilakukan      │
│ 🔗 Benang Merah: koneksi ke poin sebelum/sesudah     │
│ 📋 Langkah: kode konkret yang harus ditulis          │
│ ✅ Langkah konkret: verifikasi bahwa berhasil        │
└──────────────────────────────────────────────────────┘
```

**Aturan yang Tidak Boleh Dilanggar:**

1. **Verifikasi setiap `Langkah konkret`** sebelum lanjut ke poin berikutnya
2. **Commit setelah setiap poin** — Git history adalah progress tracker
3. **Jangan copy-paste buta** — pahami setiap baris kode yang ditulis
4. **Test endpoint setelah setiap perubahan** — gunakan file `requests/*.http`
5. **Selesaikan Checkpoint Level** sebelum naik ke level berikutnya
6. **Satu project, berkembang terus** — jangan mulai project baru dari nol

---

_Roadmap Project NestJS v1.0 — Step-by-Step, Incremental, Connected_  
_Setiap langkah menghasilkan sesuatu yang bisa dilihat dan dijalankan_