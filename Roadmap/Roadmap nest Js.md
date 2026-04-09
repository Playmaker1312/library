Max

# 📚 Kurikulum Komprehensif NestJS

## Dari Fundamental hingga Advanced

---

## 🟢 FASE 1: PRA-SYARAT (Foundation)

_Pastikan fondasi ini kuat sebelum masuk ke NestJS_

### 1. JavaScript Modern (ES6+)

- [[1.1 Variabel (let, const, var) & Scope]]
- [[1.2 Arrow Functions & Regular Functions]]
- [[1.3 Destructuring (Object & Array)]]
- [[1.4 Spread & Rest Operator]]
- [[1.5 Template Literals]]
- [[1.6 Promises & Callback]]
- [[1.7 Async or Await]]
- [[1.8 Modules (import or export)]]
- [[1.9 Array Methods (map, filter, reduce, forEach)]]
- [[1.10 Optional Chaining & Nullish Coalescing]]

### 2. TypeScript (Wajib)

- [[2.1 Mengapa TypeScript — Static Typing]]
- [[2.1 Mengapa TypeScript — Static TypingDefinisi unknown, void, never)]]
- [[2.3 Interface & Type Alias]]
- [[2.4 Enum]]
- [[2.5 Generics]]
- [[2.6 Union & Intersection Types]]
- [[2.7 Utility Types (Partial, Pick, Omit, Record)]]
- [[2.8 Decorators (eksperimental)]] ← Sangat penting untuk NestJS
- [[2.9 Abstract Class & Access Modifiers]]
- [[2.10 tsconfig.json — Konfigurasi proyek]]

### 3. Node.js Fundamentals

- [[3.1 Apa itu Node.js & Event Loop]]
- [[3.2 Modul bawaan (fs, path, http, os)]]
- [[3.3 NPM - Yarn - PNPM — Package Manager]]
- [[3.4 package.json & package-lock.json]]
- [[3.5 Environment Variables (.env)]]
- [[3.6 Membuat HTTP server sederhana (tanpa framework)]]
- [[3.7 Middleware concept]]
- [[3.8 Streams & Buffers (dasar)]]

### 4. Konsep Dasar Web & API

- [[4.1 HTTP Methods (GET, POST, PUT, PATCH, DELETE)]]
- [[4.2 Status Codes (2xx, 3xx, 4xx, 5xx)]]
- [[4.3 REST API Principles]]
- [[4.4 Request or Response Cycle]]
- [[4.5 Headers, Body, Query Params, Route Params]]
- [[4.6 JSON sebagai format data]]
- [[4.7 Postman - Insomnia — API Testing Tool]]

---

## 🔵 FASE 2: NESTJS DASAR (Beginner)

_Memahami arsitektur dan building blocks NestJS_

### 5. Pengenalan NestJS

- [[5.1 Apa itu NestJS & Mengapa menggunakannya]]
- [[5.2 Arsitektur NestJS (terinspirasi Angular)]]
- [[5.3 NestJS vs Express vs Fastify — Perbandingan]]
- [[5.4 Instalasi NestJS CLI ]]
- [[5.5 Membuat project pertama (nest new project-name)]]
- [[5.6 Struktur folder & file project NestJS]]
- [[5.7 Menjalankan server development]]
- [[5.8 Memahami file main.ts sebagai entry point]]

### 6. Modules

- [[6.1 Konsep Modular Architecture]]
- [[6.2 Root Module (AppModule)]]
- [[6.3 Membuat Module (nest g module)]]
- [[6.4 imports, controllers, providers, exports]]
- [[6.5 Feature Modules]]
- [[6.6 Shared Modules]]
- [[6.7 Global Modules (@Global())]]
- [[6.8 Dynamic Modules (pengenalan)]]

### 7. Controllers

- [[7.1 Apa itu Controller & tugasnya]]
- [[7.2 Membuat Controller (nest g controller)]]
- [[7.3 Decorator @Controller() dan route prefix]]
- [[7.4 HTTP Method Decorators (@Get, @Post, @Put, @Patch, @Delete)]]
- [[7.5 @Param() — Route Parameters]]
- [[7.6 @Query() — Query Parameters]]
- [[7.7 @Body() — Request Body]]
- [[7.8 @Headers() — Request Headers]]
- [[7.9 @Res() dan @Req() — Akses langsung ke Response/Request]]
- [[7.10 @HttpCode() — Custom status code]]
- [[7.11 @Redirect() — Redirect response]]
- [[7.12 Route Wildcards & Sub-routing]]

### 8. Providers & Services

- [[8.1 Konsep Dependency Injection (DI)]]
- [[8.2 Apa itu Provider]]
- [[8.3 Membuat Service (nest g service)]]
- [[8.4 @Injectable() Decorator]]
- [[8.5 Mendaftarkan Provider di Module]]
- [[8.6 Constructor-based Injection]]
- [[8.7 Scope Provider (DEFAULT, REQUEST, TRANSIENT)]]
- [[8.8 Custom Providers (useClass, useValue, useFactory)]]
- [[8.9 Optional Providers (@Optional())]]

### 9. DTO & Validation

- [[9.1 Apa itu DTO (Data Transfer Object)]]
- [[9.2 Membuat class DTO]]
- [[9.3 Instalasi class-validator & class-transformer]]
- [[9.4 Validation Decorators (@IsString, @IsEmail, @IsNotEmpty, dll.)]]
- [[9.5 Global Validation Pipe (ValidationPipe)]]
- [[9.6 whitelist, forbidNonWhitelisted, transform]]
- [[9.7 Nested Object Validation]]
- [[9.8 Array Validation]]
- [[9.9 Conditional Validation]]
- [[9.10 Custom Validation Decorator]]

---

## 🟡 FASE 3: NESTJS MENENGAH (Intermediate)

_Fitur inti yang digunakan di hampir semua project nyata_

### 10. Exception Handling

- [[10.1 Built-in HTTP Exceptions (NotFoundException, BadRequestException, dll.)]]
- [[10.2 Throw exception di Service]]
- [[10.3 Custom Exception class]]
- [[10.4 Exception Filters (@Catch())]]
- [[10.5 Global Exception Filter (useGlobalFilters)]]
- [[10.6 Menangani error secara konsisten (standard error response)]]

### 11. Pipes

- [[11.1 Apa itu Pipe & kegunaannya]]
- [[11.2 Built-in Pipes (ParseIntPipe, ParseBoolPipe, ParseUUIDPipe, dll.)]]
- [[11.3 Pipe Level (parameter, method, controller, global)]]
- [[11.4 ValidationPipe — Deep Dive]]
- [[11.5 Custom Pipe (@Injectable() + PipeTransform)]]
- [[11.6 Transformasi data di Pipe]]

### 12. Middleware

- [[12.1 Konsep Middleware di NestJS]]
- [[12.2 Class Middleware (implements NestMiddleware)]]
- [[12.3 Functional Middleware]]
- [[12.4 Mendaftarkan Middleware (configure() di Module)]]
- [[12.5 Route-specific vs Global Middleware]]
- [[12.6 Menggunakan middleware Express/Fastify (cors, helmet, morgan)]]

### 13. Guards

- [[13.1 Apa itu Guard & perbedaannya dengan Middleware]]
- [[13.2 Membuat Guard (@Injectable() + CanActivate)]]
- [[13.3 Execution Context]]
- [[13.4 Guard Level (method, controller, global)]]
- [[13.5 Role-based Access Control (RBAC) dengan Guard]]
- [[13.6 Kombinasi Guard & Custom Decorator]]

### 14. Interceptors

- [[14.1 Apa itu Interceptor & use cases]]
- [[14.2 Membuat Interceptor (NestInterceptor + intercept())]]
- [[14.3 RxJS Observable dalam Interceptor]]
- [[14.4 Response Mapping/Transformation]]
- [[14.5 Logging Interceptor]]
- [[14.6 Timeout Interceptor]]
- [[14.7 Cache Interceptor (pengenalan)]]
- [[14.8 Interceptor Level (method, controller, global)]]

### 15. Custom Decorators

- [[15.1 Mengapa Custom Decorator]]
- [[15.2 createParamDecorator() — Parameter Decorator]]
- [[15.3 Decorator Composition (applyDecorators)]]
- [[15.4 SetMetadata() & Reflector]]
- [[15.5 Contoh @CurrentUser(), @Roles(), @Public()]]

### 16. Database — TypeORM

- [[16.1 Instalasi & konfigurasi @nestjs/typeorm]]
- [[16.2 Koneksi ke database (PostgreSQL - MySQL)]]
- [[16.3 Entity — Definisi tabel]]
- [[16.4 Column Types & Decorators]]
- [[16.5 Repository Pattern]]
- [[16.6 CRUD Operations dengan Repository]]
- [[16.7 Relations - One-to-One]]
- [[16.8 Relations - One-to-Many - Many-to-One]]
- [[16.9 Relations - Many-to-Many]]
- [[16.10 Eager vs Lazy Loading]]
- [[16.11 QueryBuilder]]
- [[16.12 Migrations — Membuat & menjalankan]]
- [[16.13 Seeding data]]
- [[16.14 Transactions]]
- [[16.15 Indexing & Performance dasar]]

### 17. Database — Prisma (Alternatif)

- [[17.1 Instalasi & inisialisasi Prisma]]
- [[17.2 schema.prisma — Definisi model]]
- [[17.3 Prisma Client & PrismaService di NestJS]]
- [[17.4 CRUD Operations dengan Prisma]]
- [[17.5 Relations di Prisma]]
- [[17.6 Migrations dengan Prisma]]
- [[17.7 Prisma Studio]]
- [[17.8 Type Safety Prisma vs TypeORM]]

### 18. Konfigurasi & Environment

- [[18.1 @nestjs/config — ConfigModule]]
- [[18.2 .env file & environment variables]]
- [[18.3 Validasi konfigurasi (Joi - class-validator)]]
- [[18.4 Configuration Namespaces]]
- [[18.5 Konfigurasi per environment (dev, staging, production)]]
- [[18.6 ConfigService — Mengambil nilai konfigurasi]]

---

## 🟠 FASE 4: NESTJS LANJUTAN (Upper-Intermediate)

_Fitur-fitur untuk aplikasi production-grade_

### 19. Authentication

- [[19.1 Konsep Authentication vs Authorization]]
- [[19.2 @nestjs/passport — Integrasi Passport.js]]
- [[19.3 Local Strategy (username/password)]]
- [[19.4 Password Hashing (bcrypt)]]
- [[19.5 JWT Strategy (@nestjs/jwt)]]
- [[19.6 Access Token & Refresh Token]]
- [[19.7 Auth Guard (JwtAuthGuard)]]
- [[19.8 Protecting Routes]]
- [[19.9 User Registration & Login Flow]]
- [[19.10 Session-based Authentication (opsional)]]

### 20. Authorization

- [[20.1 Role-based Access Control (RBAC)]]
- [[20.2 @Roles() Custom Decorator]]
- [[20.3 RolesGuard Implementation]]
- [[20.4 Claims-based Authorization]]
- [[20.5 Policy-based Authorization (CASL integration)]]
- [[20.6 Permission-based Access Control]]

### 21. Serialization & Response Shaping

- [[21.1 ClassSerializerInterceptor]]
- [[21.2 @Exclude() & @Expose() dari class-transformer]]
- [[21.3 @Transform() Decorator]]
- [[21.4 Serialization Groups]]
- [[21.5 Custom Serialization Interceptor]]
- [[21.6 Pagination Response Pattern]]

### 22. File Upload & Storage

- [[22.1 Multer Integration (@UseInterceptors + FileInterceptor)]]
- [[22.2 Single & Multiple File Upload]]
- [[22.3 File Validation (size, type)]]
- [[22.4 Menyimpan file ke disk]]
- [[22.5 Upload ke Cloud Storage (AWS S3 - GCS)]]
- [[22.6 Streaming file response]]

### 23. Logging

- [[23.1 Built-in Logger (Logger class)]]
- [[23.2 Log Levels (log, error, warn, debug, verbose)]]
- [[23.3 Custom Logger Service]]
- [[23.4 Integrasi Winston - Pino]]
- [[23.5 Request Logging Middleware]]
- [[23.6 Structured Logging (JSON format)]]
- [[23.7 Log Rotation]]

### 24. Caching

- [[24.1 @nestjs/cache-manager]]
- [[24.2 In-memory Caching]]
- [[24.3 @CacheKey() & @CacheTTL()]]
- [[24.4 Cache Interceptor]]
- [[24.5 Redis sebagai Cache Store]]
- [[24.6 Cache Invalidation Strategies]]
- [[24.7 Custom Cache Decorator]]

### 25. Task Scheduling & Queues

- [[25.1 @nestjs/schedule — Cron Jobs]]
- [[25.2 @Cron() Decorator]]
- [[25.3 @Interval() & @Timeout()]]
- [[25.4 Dynamic Cron Jobs]]
- [[25.5 @nestjs/bull — Queue System]]
- [[25.6 Producer & Consumer Pattern]]
- [[25.7 Job Priority, Retry, Delay]]
- [[25.8 Queue Dashboard (Bull Board)]]
- [[25.9 Use Case - Email Queue, Image Processing]]

### 26. Email & Notifications

- [[26.1 @nestjs-modules/mailer — Setup]]
- [[26.2 SMTP Configuration]]
- [[26.3 Email Templates (Handlebars/Pug)]]
- [[26.4 Sending Email (welcome, reset password)]]
- [[26.5 Email Queue Integration]]
- [[26.6 Push Notification (pengenalan)]]

### 27. Event-driven Architecture

- [[27.1 @nestjs/event-emitter]]
- [[27.2 EventEmitter2 Integration]]
- [[27.3 @OnEvent() Decorator]]
- [[27.4 Emit Events dari Service]]
- [[27.5 Synchronous vs Asynchronous Events]]
- [[27.6 Event Payload Typing]]
- [[27.7 Wildcard Events]]

---

## 🔴 FASE 5: NESTJS ADVANCED (Expert Level)

_Arsitektur kompleks dan skalabilitas_

### 28. API Documentation — Swagger/OpenAPI

- [[28.1 @nestjs/swagger — Instalasi & Setup]]
- [[28.2 SwaggerModule.setup()]]
- [[28.3 @ApiTags() — Grouping endpoints]]
- [[28.4 @ApiOperation() & @ApiResponse()]]
- [[28.5 @ApiProperty() di DTO]]
- [[28.6 @ApiBearerAuth() — Auth di Swagger]]
- [[28.7 @ApiQuery(), @ApiParam(), @ApiBody()]]
- [[28.8 Multiple Swagger Documents]]
- [[28.9 Swagger UI Customization]]

### 29. Testing

- [[29.1 Filosofi Testing di NestJS]]
- [[29.2 Unit Testing — Jest Setup]]
- [[29.3 Testing Service (mocking dependencies)]]
- [[29.4 Testing Controller]]
- [[29.5 Testing Guard, Pipe, Interceptor]]
- [[29.6 Test.createTestingModule() — Testing Module]]
- [[29.7 Integration Testing (E2E)]]
- [[29.8 Supertest untuk HTTP E2E Test]]
- [[29.9 Database Testing (in-memory - test DB)]]
- [[29.10 Coverage Report & Best Practices]]

### 30. WebSockets & Real-time Communication

- [[30.1 @nestjs/websockets & @nestjs/platform-socket.io]]
- [[30.2 Gateway (@WebSocketGateway)]]
- [[30.3 @SubscribeMessage() Decorator]]
- [[30.4 WebSocket Lifecycle Hooks]]
- [[30.5 Broadcasting Messages]]
- [[30.6 Rooms & Namespaces]]
- [[30.7 WebSocket Authentication]]
- [[30.8 Scaling WebSockets (Redis Adapter)]]

### 31. GraphQL

- [[31.1 REST vs GraphQL — Kapan menggunakan apa]]
- [[31.2 @nestjs/graphql — Setup (Code-first vs Schema-first)]]
- [[31.3 Resolvers (Query, Mutation, Subscription)]]
- [[31.4 ObjectType & Field Decorators]]
- [[31.5 Input Types & Args]]
- [[31.6 GraphQL Validation]]
- [[31.7 Relations di GraphQL]]
- [[31.8 DataLoader (N+1 Problem Solution)]]
- [[31.9 GraphQL Authentication & Guards]]
- [[31.10 Apollo Studio - Playground]]

### 32. Microservices

- [[32.1 Monolith vs Microservices — Kapan memilih]]
- [[32.2 @nestjs/microservices — Pengenalan]]
- [[32.3 Transport Layers (TCP, Redis, NATS, RabbitMQ, Kafka, gRPC)]]
- [[32.4 Message Pattern (@MessagePattern)]]
- [[32.5 Event Pattern (@EventPattern)]]
- [[32.6 Hybrid Application (HTTP + Microservice)]]
- [[32.7 ClientProxy & ClientModule]]
- [[32.8 Error Handling di Microservices]]
- [[32.9 Inter-service Communication]]
- [[32.10 Service Discovery (pengenalan)]]

### 33. gRPC

- [[33.1 Apa itu gRPC & Protocol Buffers]]
- [[33.2 Membuat .proto file]]
- [[33.3 gRPC Server di NestJS]]
- [[33.4 gRPC Client di NestJS]]
- [[33.5 Streaming (Server, Client, Bidirectional)]]
- [[33.6 Error Handling gRPC]]
- [[33.7 gRPC vs REST Performance]]

### 34. CQRS & Event Sourcing

- [[34.1 Apa itu CQRS Pattern]]
- [[34.2 @nestjs/cqrs — Instalasi]]
- [[34.3 Commands & Command Handlers]]
- [[34.4 Queries & Query Handlers]]
- [[34.5 Events & Event Handlers]]
- [[34.6 Sagas]]
- [[34.7 Event Sourcing (pengenalan)]]
- [[34.8 Aggregate Root Pattern]]

### 35. Security Best Practices

- [[35.1 Helmet — HTTP Security Headers]]
- [[35.2 CORS Configuration]]
- [[35.3 Rate Limiting (@nestjs/throttler)]]
- [[35.4 CSRF Protection]]
- [[35.5 SQL Injection Prevention]]
- [[35.6 Input Sanitization]]
- [[35.7 HTTPS & SSL/TLS]]
- [[35.8 Secrets Management]]
- [[35.9 OAuth2 & Social Login (Google, GitHub)]]
- [[35.10 Two-Factor Authentication (2FA)]]

### 36. Performance & Optimization

- [[36.1 Compression (gzip/brotli)]]
- [[36.2 Fastify sebagai HTTP Adapter (pengganti Express)]]
- [[36.3 Lazy Loading Modules]]
- [[36.4 Database Query Optimization]]
- [[36.5 Connection Pooling]]
- [[36.6 Memory Profiling]]
- [[36.7 Benchmarking (autocannon, clinic.js)]]
- [[36.8 Payload Size Optimization]]

---

## ⚫ FASE 6: PRODUCTION & DEVOPS

_Menyiapkan aplikasi untuk dunia nyata_

### 37. Deployment

- [[37.1 Build Production (npm run build)]]
- [[37.2 PM2 — Process Manager]]
- [[37.3 Docker — Containerization]]
- [[37.4 Dockerfile untuk NestJS]]
- [[37.5 Docker Compose (App + DB + Redis)]]
- [[37.6 CI/CD Pipeline (GitHub Actions - GitLab CI)]]
- [[37.7 Deploy ke Cloud (AWS - GCP - DigitalOcean - Railway)]]
- [[37.8 Nginx sebagai Reverse Proxy]]
- [[37.9 SSL Certificate (Let's Encrypt)]]
- [[37.10 Zero-downtime Deployment]]

### 38. Monitoring & Observability

- [[38.1 Health Checks (@nestjs/terminus)]]
- [[38.2 Custom Health Indicators]]
- [[38.3 Prometheus Metrics]]
- [[38.4 Grafana Dashboard]]
- [[38.5 Distributed Tracing (OpenTelemetry - Jaeger)]]
- [[38.6 Centralized Logging (ELK Stack - Loki)]]
- [[38.7 Error Tracking (Sentry)]]
- [[38.8 APM (Application Performance Monitoring)]]

### 39. Architecture Patterns & Best Practices

- [[39.1 Clean Architecture di NestJS]]
- [[39.2 Domain-Driven Design (DDD) Principles]]
- [[39.3 Repository Pattern (abstraksi database)]]
- [[39.4 Service Layer Pattern]]
- [[39.5 SOLID Principles dalam konteks NestJS]]
- [[39.6 Hexagonal Architecture]]
- [[39.7 Monorepo dengan NestJS (Nx - Turborepo)]]
- [[39.8 API Versioning]]
- [[39.9 Feature Flags]]
- [[39.10 Graceful Shutdown]]

### 40. Project Capstone (Proyek Akhir)

- [[40.1 🛒 E-Commerce API (Auth, Products, Orders, Payments)]]
- [[40.2 💬 Real-time Chat App (WebSocket + Auth + DB)]]
- [[40.3 📝 Blog/CMS Platform (CRUD + Roles + File Upload)]]
- [[40.4 🏦 Banking Microservices (CQRS + Event Sourcing)]]
- [[40.5 📊 Analytics Dashboard API (Caching + Queue + Scheduling)]]

---

## 🗺️ PETA PROGRES BELAJAR

|Periode|Fase|Fokus|
|---|---|---|
|Minggu 1-2|Fase 1|Pra-syarat|
|Minggu 3-4|Fase 2|NestJS Dasar|
|Minggu 5-7|Fase 3|Intermediate|
|Minggu 8-11|Fase 4|Upper-Intermediate|
|Minggu 12-16|Fase 5|Advanced|
|Minggu 17-20|Fase 6|Production & Capstone|

---

## 📖 SUMBER BELAJAR PENDUKUNG

|Sumber|Keterangan|
|---|---|
|[docs.nestjs.com](https://docs.nestjs.com/)|Dokumentasi resmi (UTAMA)|
|NestJS GitHub|Source code & contoh|
|NestJS Discord|Komunitas & tanya jawab|
|YouTube — Marius Espejo|Tutorial NestJS terbaik|
|Udemy — Ariel Weinberger|Kursus NestJS lengkap|
|TypeScript Handbook|Referensi TypeScript|

---

> 💡 **Tips:** Jangan loncat fase. Setiap poin bernomor adalah satu sesi belajar. Tandai ✅ setiap kali selesai mempelajari satu poin. Bangun mini-project di setiap akhir fase untuk memperkuat pemahaman.