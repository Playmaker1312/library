# 61 - Setup Swagger & Konvensi Dokumentasi

---

## Penjelasan

Setelah semua endpoint auth dan blog built, kita perlu mendokumentasikan API agar frontend, mobile, atau developer lain bisa mengintegrasikannya dengan mudah. Swagger (OpenAPI) adalah standar industri untuk dokumentasi REST API. NestJS menyediakan `@nestjs/swagger` yang secara otomatis membaca decorator TypeScript untuk menghasilkan spesifikasi OpenAPI.

---

## Fungsi

- Menghasilkan dokumentasi API interaktif (Swagger UI)
- Spesifikasi OpenAPI 3.0 yang bisa diekspor sebagai JSON/YAML
- Menampilkan semua endpoint, parameter, request body, response
- Autentikasi via Swagger UI (Authorize button)
- Menyediakan konvensi penamaan dan struktur dokumentasi

---

## Cara Pengimplementasian

### 1. Install package

```bash
npm install @nestjs/swagger
```

### 2. Setup Swagger di main.ts

```typescript
// main.ts
import { NestFactory } from '@nestjs/core';
import { ValidationPipe } from '@nestjs/common';
import { SwaggerModule, DocumentBuilder } from '@nestjs/swagger';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  app.setGlobalPrefix('api/v1');
  app.useGlobalPipes(new ValidationPipe({ whitelist: true }));

  // Setup Swagger
  const config = new DocumentBuilder()
    .setTitle('NestJS Blog API')
    .setDescription('API documentation for the NestJS Blog Application')
    .setVersion('1.0')
    .addBearerAuth(
      {
        type: 'http',
        scheme: 'bearer',
        bearerFormat: 'JWT',
        name: 'JWT',
        description: 'Enter JWT access token',
        in: 'header',
      },
      'JWT-auth', // Security scheme name
    )
    .addCookieAuth('refreshToken')
    .addTag('Auth', 'Authentication endpoints (register, login, refresh)')
    .addTag('Users', 'User management endpoints')
    .addTag('Posts', 'Blog post CRUD endpoints')
    .addServer('http://localhost:3000', 'Development server')
    .addServer('https://api.example.com', 'Production server')
    .build();

  const document = SwaggerModule.createDocument(app, config);
  SwaggerModule.setup('docs', app, document, {
    swaggerOptions: {
      persistAuthorization: true, // Remember auth token on refresh
      docExpansion: 'list',       // Show all endpoints expanded
      filter: true,               // Enable endpoint search
    },
  });

  await app.listen(3000);
  console.log('Swagger docs available at http://localhost:3000/docs');
}
bootstrap();
```

### 3. @ApiTags — Kelompokkan Endpoint

```typescript
// auth/auth.controller.ts
import { Controller, Post, Get } from '@nestjs/common';
import { ApiTags, ApiBearerAuth } from '@nestjs/swagger';

@ApiTags('Auth') // Kelompokkan endpoint Auth
@Controller('auth')
export class AuthController {
  @Post('register')
  @ApiBearerAuth() // Tidak perlu — register tidak pakai auth
  // ^ HATI-HATI: jangan taruh ApiBearerAuth di public endpoint
  async register() {}

  @Post('login')
  async login() {}

  @Post('refresh')
  @ApiBearerAuth('JWT-auth') // Pakai security scheme "JWT-auth"
  async refresh() {}

  @Post('logout')
  @ApiBearerAuth()
  async logout() {}
}
```

```typescript
// blog/blog.controller.ts
import { Controller, Get, Post, Patch, Delete, Param } from '@nestjs/common';
import { ApiTags, ApiBearerAuth } from '@nestjs/swagger';

@ApiTags('Posts')
@Controller('posts')
export class BlogController {
  @Get()
  @ApiBearerAuth() // Optional — tergantung kebutuhan
  findAll() {}

  @Post()
  @ApiBearerAuth()
  create() {}

  @Patch(':id')
  @ApiBearerAuth()
  update() {}

  @Delete(':id')
  @ApiBearerAuth()
  remove() {}
}
```

### 4. Konvensi Penamaan Tag

| Tag | Endpoint | Deskripsi |
|-----|----------|-----------|
| Auth | /auth/* | Register, login, refresh, logout |
| Users | /users/* | CRUD user, profile |
| Posts | /posts/* | CRUD blog post |
| Comments | /comments/* | CRUD komentar |
| Uploads | /uploads/* | File upload |
| Health | /health | Health check |

### 5. Swagger UI akan tampil di `http://localhost:3000/docs`

```
┌──────────────────────────────────────────────────────────┐
│  Swagger UI                                              │
│  ┌────────────────────────────────────────────────────┐  │
│  │ Authorize → [Enter JWT token]                       │  │
│  └────────────────────────────────────────────────────┘  │
│                                                          │
│  Auth ──┬── POST /auth/register                         │
│         ├── POST /auth/login                             │
│         ├── POST /auth/refresh          [🔒]             │
│         └── POST /auth/logout           [🔒]             │
│                                                          │
│  Posts ──┬── GET /posts                 [🔒]             │
│          ├── POST /posts                [🔒]             │
│          ├── GET /posts/{id}            [🔒]             │
│          ├── PATCH /posts/{id}          [🔒]             │
│          └── DELETE /posts/{id}         [🔒]             │
│                                                          │
│  [Try it out]  [Download OpenAPI JSON]                   │
└──────────────────────────────────────────────────────────┘
```

---

## Analogi

Swagger adalah **papan informasi + buku panduan** di lobi gedung. Papan informasi (Swagger UI) menunjukkan denah lantai (endpoint), nomor ruangan (path), dan aturan masuk (security). Buku panduan (OpenAPI spec) bisa dibawa pulang oleh developer lain. Tanpa papan informasi, setiap tamu harus bertanya ke resepsionis satu per satu — repot. Dengan Swagger, semua tertera jelas: "Lantai 1: Auth, Lantai 2: Posts, butuh kartu akses JWT."

---

## Dipakai Untuk Apa

- Dokumentasi API untuk frontend / mobile team
- Testing endpoint langsung dari browser
- Generate client SDK (OpenAPI Generator)
- Standarisasi kontrak API antar tim
- Onboarding developer baru

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Lupa `addBearerAuth` di DocumentBuilder | Token button tidak muncul di Swagger UI |
| `@ApiBearerAuth()` di public endpoint | Hanya tambahkan di endpoint yang butuh auth |
| Tidak pakai `@ApiTags` | Semua endpoint tercampur di satu grup "default" |
| Setup Swagger cuma di dev | Buat conditional: hanya aktif di `NODE_ENV !== 'production'` |
| Versi API tidak update | Set `setVersion()` sesuai rilis |

---

## Soal Latihan

### Soal 1
Setup Swagger di main.ts dengan:
- Title: "My App API"
- Version: "2.0"
- Bearer auth dengan nama "access-token"
- Tag: "Auth" dan "Users"
- Path: /api/docs

### Jawaban 1
```typescript
async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  const config = new DocumentBuilder()
    .setTitle('My App API')
    .setDescription('API documentation')
    .setVersion('2.0')
    .addBearerAuth(
      {
        type: 'http',
        scheme: 'bearer',
        bearerFormat: 'JWT',
        name: 'access-token',
        description: 'Enter JWT token',
        in: 'header',
      },
      'access-token',
    )
    .addTag('Auth')
    .addTag('Users')
    .build();

  const document = SwaggerModule.createDocument(app, config);
  SwaggerModule.setup('api/docs', app, document);

  await app.listen(3000);
}
```

### Soal 2
Apa fungsi `addBearerAuth` di DocumentBuilder? Kenapa perlu nama security scheme?

### Jawaban 2
`addBearerAuth` mendaftarkan security scheme (tipe HTTP Bearer JWT) ke spesifikasi OpenAPI. Nama security scheme digunakan di decorator `@ApiBearerAuth('nama-scheme')` untuk menandai endpoint mana yang membutuhkan auth. Tanpa ini, tombol "Authorize" tidak muncul di Swagger UI.

### Soal 3
Sebutkan 3 konvensi penamaan tag yang baik untuk dokumentasi Swagger.

### Jawaban 3
1. Gunakan kata benda (Noun) dalam bentuk plural: `Auth`, `Users`, `Posts`, `Comments`
2. Group berdasarkan domain/fitur, bukan berdasarkan controller
3. Gunakan PascalCase atau Title Case konsisten: `Auth`, `Blog Posts` (bukan `blog-posts`)
