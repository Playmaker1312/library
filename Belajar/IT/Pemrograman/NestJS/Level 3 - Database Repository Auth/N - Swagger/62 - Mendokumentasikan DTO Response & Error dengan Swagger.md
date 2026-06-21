# 62 - Mendokumentasikan DTO Response & Error dengan Swagger

---

## Penjelasan

Setelah Swagger di-setup dengan tag dan bearer auth, kita perlu mendokumentasikan setiap endpoint secara detail: request body (DTO), response sukses, parameter path/query, dan kemungkinan error. Swagger akan membaca decorator `@ApiProperty`, `@ApiResponse`, `@ApiOperation`, dll untuk menampilkan informasi ini di UI. Dokumentasi yang baik membuat API mudah diintegrasikan tanpa perlu membaca source code.

---

## Fungsi

- Mendokumentasikan properti DTO (tipe, contoh, required/optional)
- Menampilkan response sukses dan error codes
- Memberi deskripsi operasi endpoint (summary, description)
- Mendokumentasikan parameter path, query, dan body
- Auto-generate example request/response di Swagger UI

---

## Cara Pengimplementasian

### 1. @ApiProperty & @ApiPropertyOptional

```typescript
// auth/dto/register.dto.ts
import { IsEmail, IsString, MinLength } from 'class-validator';
import { ApiProperty } from '@nestjs/swagger';

export class RegisterDto {
  @ApiProperty({
    example: 'user@example.com',
    description: 'User email address',
    format: 'email',
  })
  @IsEmail()
  email: string;

  @ApiProperty({
    example: 'strongP@ss123',
    description: 'Password (min 8 characters)',
    minLength: 8,
    format: 'password',
  })
  @IsString()
  @MinLength(8)
  password: string;

  @ApiProperty({
    example: 'John Doe',
    description: 'User display name',
  })
  @IsString()
  name: string;
}
```

```typescript
// auth/dto/login.dto.ts
import { IsEmail, IsString } from 'class-validator';
import { ApiProperty } from '@nestjs/swagger';

export class LoginDto {
  @ApiProperty({ example: 'user@example.com' })
  @IsEmail()
  email: string;

  @ApiProperty({ example: 'strongP@ss123', format: 'password' })
  @IsString()
  password: string;
}
```

### 2. Response DTO

```typescript
// auth/dto/response/login-response.dto.ts
import { ApiProperty } from '@nestjs/swagger';

export class LoginResponseDto {
  @ApiProperty({
    example: 'eyJhbGciOiJIUzI1NiIs...',
    description: 'JWT access token (expires in 15m)',
  })
  accessToken: string;

  @ApiProperty({
    example: 'dGhpcyBpcyBhIHJlZnJl...',
    description: 'JWT refresh token (expires in 7d)',
  })
  refreshToken: string;
}
```

```typescript
// auth/dto/response/message-response.dto.ts
import { ApiProperty } from '@nestjs/swagger';

export class MessageResponseDto {
  @ApiProperty({ example: 'Operation completed successfully' })
  message: string;
}
```

```typescript
// blog/dto/response/post-response.dto.ts
import { ApiProperty, ApiPropertyOptional } from '@nestjs/swagger';

export class PostResponseDto {
  @ApiProperty({ example: 'a1b2c3d4-...', description: 'Post UUID' })
  id: string;

  @ApiProperty({ example: 'My First Blog Post' })
  title: string;

  @ApiPropertyOptional({ example: 'This is the content...' })
  content?: string;

  @ApiProperty({ example: false })
  published: boolean;

  @ApiProperty({ example: '2025-01-15T10:30:00Z' })
  createdAt: Date;

  @ApiProperty({ example: '2025-01-15T10:30:00Z' })
  updatedAt: Date;

  @ApiProperty({ example: 'u1v2w3x4-...', description: 'Author UUID' })
  authorId: string;
}
```

### 3. @ApiOperation, @ApiResponse

```typescript
// auth/auth.controller.ts
import { ApiOperation, ApiResponse, ApiBody, ApiBearerAuth } from '@nestjs/swagger';
import { LoginResponseDto } from './dto/response/login-response.dto';
import { MessageResponseDto } from './dto/response/message-response.dto';

@ApiTags('Auth')
@Controller('auth')
export class AuthController {
  constructor(private readonly authService: AuthService) {}

  @Post('register')
  @ApiOperation({
    summary: 'Register a new user',
    description: 'Creates a new user account and sends verification email',
  })
  @ApiBody({ type: RegisterDto })
  @ApiResponse({
    status: 201,
    description: 'User registered successfully',
    type: MessageResponseDto,
  })
  @ApiResponse({ status: 409, description: 'Email already registered' })
  @ApiResponse({ status: 400, description: 'Validation failed' })
  async register(@Body() dto: RegisterDto) {
    return this.authService.register(dto);
  }

  @Post('login')
  @HttpCode(HttpStatus.OK)
  @ApiOperation({
    summary: 'Login with email & password',
    description: 'Authenticates user and returns JWT token pair',
  })
  @ApiBody({ type: LoginDto })
  @ApiResponse({
    status: 200,
    description: 'Login successful',
    type: LoginResponseDto,
  })
  @ApiResponse({ status: 401, description: 'Invalid credentials' })
  async login(@Body() dto: LoginDto) {
    return this.authService.login(dto);
  }
}
```

### 4. @ApiParam, @ApiQuery

```typescript
// blog/blog.controller.ts
import { ApiParam, ApiQuery } from '@nestjs/swagger';

@ApiTags('Posts')
@Controller('posts')
export class BlogController {
  @Get()
  @ApiOperation({ summary: 'List all posts', description: 'Returns paginated posts' })
  @ApiQuery({
    name: 'page',
    required: false,
    type: Number,
    example: 1,
    description: 'Page number',
  })
  @ApiQuery({
    name: 'limit',
    required: false,
    type: Number,
    example: 10,
    description: 'Items per page',
  })
  @ApiQuery({
    name: 'search',
    required: false,
    type: String,
    description: 'Search in title/content',
  })
  @ApiResponse({
    status: 200,
    description: 'List of posts',
    type: [PostResponseDto],
  })
  findAll(
    @Query('page') page?: number,
    @Query('limit') limit?: number,
    @Query('search') search?: string,
  ) {}

  @Get(':id')
  @ApiOperation({ summary: 'Get post by ID' })
  @ApiParam({ name: 'id', required: true, type: String, description: 'Post UUID' })
  @ApiResponse({ status: 200, type: PostResponseDto })
  @ApiResponse({ status: 404, description: 'Post not found' })
  findOne(@Param('id') id: string) {}
}
```

### 5. Dokumentasi Error Global

```typescript
// common/dto/error-response.dto.ts
import { ApiProperty } from '@nestjs/swagger';

export class ErrorResponseDto {
  @ApiProperty({ example: 400 })
  statusCode: number;

  @ApiProperty({ example: 'Validation failed' })
  message: string;

  @ApiPropertyOptional({
    example: ['email must be an email'],
    description: 'Array of error messages (validation)',
  })
  errors?: string[];

  @ApiProperty({ example: '2025-01-15T10:30:00.000Z' })
  timestamp: Date;

  @ApiProperty({ example: '/auth/register' })
  path: string;
}
```

### 6. Dokumentasi Lengkap Blog API

```typescript
// blog/dto/create-post.dto.ts
import { IsString, IsOptional, IsBoolean } from 'class-validator';
import { ApiProperty, ApiPropertyOptional } from '@nestjs/swagger';

export class CreatePostDto {
  @ApiProperty({ example: 'My New Post', description: 'Post title' })
  @IsString()
  title: string;

  @ApiPropertyOptional({ example: 'Post content...' })
  @IsString()
  @IsOptional()
  content?: string;

  @ApiPropertyOptional({ example: false, default: false })
  @IsBoolean()
  @IsOptional()
  published?: boolean;
}
```

```typescript
// blog/dto/update-post.dto.ts
import { PartialType } from '@nestjs/swagger';
import { CreatePostDto } from './create-post.dto';

export class UpdatePostDto extends PartialType(CreatePostDto) {}
```

```typescript
// blog/blog.controller.ts — dokumentasi lengkap
import {
  ApiTags, ApiBearerAuth, ApiOperation, ApiResponse,
  ApiParam, ApiQuery, ApiBody,
} from '@nestjs/swagger';

@ApiTags('Posts')
@Controller('posts')
export class BlogController {
  constructor(private readonly blogService: BlogService) {}

  @Post()
  @ApiBearerAuth()
  @ApiOperation({ summary: 'Create a new post' })
  @ApiBody({ type: CreatePostDto })
  @ApiResponse({ status: 201, type: PostResponseDto })
  @ApiResponse({ status: 401, description: 'Unauthorized' })
  create(@Body() dto: CreatePostDto, @CurrentUser() user: User) {
    return this.blogService.create(dto, user.id);
  }

  @Get()
  @ApiBearerAuth()
  @ApiOperation({ summary: 'Get all posts', description: 'Returns paginated results' })
  @ApiQuery({ name: 'page', type: Number, required: false, example: 1 })
  @ApiQuery({ name: 'limit', type: Number, required: false, example: 10 })
  @ApiResponse({ status: 200, type: [PostResponseDto] })
  findAll(@Query('page') page = 1, @Query('limit') limit = 10) {
    return this.blogService.findAll(page, limit);
  }

  @Get(':id')
  @ApiBearerAuth()
  @ApiOperation({ summary: 'Get a post by ID' })
  @ApiParam({ name: 'id', type: String, description: 'Post UUID' })
  @ApiResponse({ status: 200, type: PostResponseDto })
  @ApiResponse({ status: 404, description: 'Post not found' })
  findOne(@Param('id') id: string) {
    return this.blogService.findById(id);
  }

  @Patch(':id')
  @ApiBearerAuth()
  @ApiOperation({ summary: 'Update a post' })
  @ApiParam({ name: 'id', type: String })
  @ApiBody({ type: UpdatePostDto })
  @ApiResponse({ status: 200, type: PostResponseDto })
  @ApiResponse({ status: 403, description: 'Forbidden (not your post)' })
  @ApiResponse({ status: 404, description: 'Post not found' })
  update(@Param('id') id: string, @Body() dto: UpdatePostDto, @CurrentUser() user: User) {
    return this.blogService.update(id, dto, user);
  }

  @Delete(':id')
  @ApiBearerAuth()
  @ApiOperation({ summary: 'Delete a post' })
  @ApiParam({ name: 'id', type: String })
  @ApiResponse({ status: 200, type: MessageResponseDto })
  @ApiResponse({ status: 403, description: 'Forbidden (not your post)' })
  @ApiResponse({ status: 404, description: 'Post not found' })
  remove(@Param('id') id: string, @CurrentUser() user: User) {
    return this.blogService.remove(id, user);
  }
}
```

### 7. Swagger UI akan tampil seperti ini

```
┌──────────────────────────────────────────────────────────┐
│  POST /auth/register                                     │
│  ┌─ Parameters ───────────────────────────────────────┐  │
│  │ Request Body: application/json                     │  │
│  │ {                                                  │  │
│  │   "email": "user@example.com",    ← string email   │  │
│  │   "password": "strongP@ss123",    ← string pass    │  │
│  │   "name": "John Doe"              ← string         │  │
│  │ }                                                  │  │
│  └────────────────────────────────────────────────────┘  │
│                                                          │
│  ┌─ Responses ─────────────────────────────────────────┐ │
│  │ 201: { "message": "User registered successfully" }  │ │
│  │ 400: Validation error                               │ │
│  │ 409: Email already registered                       │ │
│  └────────────────────────────────────────────────────┘  │
│  [Try it out]                                            │
└──────────────────────────────────────────────────────────┘
```

---

## Analogi

Dokumentasi DTO dan response di Swagger seperti **papan nama dan petunjuk arah di setiap pintu ruangan**. `@ApiProperty` memberi label: "ini lubang kunci (email), ini gagang pintu (password), ini slot kartu akses (token)". `@ApiResponse` memberi tahu: "kalau pintu terbuka (200), Anda masuk; kalau salah kunci (401), Anda ditolak; kalau pintu sudah dipakai orang lain (409), tidak bisa masuk". Tanpa petunjuk ini, developer lain harus menebak-nebak isi setiap ruangan.

---

## Dipakai Untuk Apa

- Dokumentasi request body dan response yang jelas
- Auto-generate example request di Swagger UI
- Type safety antara dokumentasi dan actual code
- Integrasi dengan tools seperti Postman, Insomnia, Redoc
- Kontrak API untuk frontend-mobile team

---

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Lupa `@ApiProperty` di DTO properti | Swagger tidak tahu tipe data — tampil sebagai `string` tanpa contoh |
| `@ApiResponse` tidak mencakup semua status code | Dokumentasikan 200, 201, 400, 401, 403, 404, 409, 500 |
| Contoh value tidak realistis | Pakai contoh yang masuk akal (`user@example.com`, jangan `string`) |
| Tidak pakai `PartialType` untuk update DTO | Update DTO harus manual menulis ulang field opsional |
| `@ApiBearerAuth()` di controller class | Terapkan per-endpoint, bukan class-level |

---

## Soal Latihan

### Soal 1
Buat DTO `CreateUserDto` dengan dokumentasi Swagger lengkap (email, name, age, role).

### Jawaban 1
```typescript
import { IsEmail, IsString, IsInt, Min, IsOptional, IsEnum } from 'class-validator';
import { ApiProperty, ApiPropertyOptional } from '@nestjs/swagger';

enum UserRole {
  USER = 'user',
  ADMIN = 'admin',
}

export class CreateUserDto {
  @ApiProperty({ example: 'user@example.com', description: 'Email address' })
  @IsEmail()
  email: string;

  @ApiProperty({ example: 'John Doe' })
  @IsString()
  name: string;

  @ApiProperty({ example: 25, description: 'Age (min 18)' })
  @IsInt()
  @Min(18)
  age: number;

  @ApiPropertyOptional({ example: 'user', enum: UserRole })
  @IsEnum(UserRole)
  @IsOptional()
  role?: UserRole;
}
```

### Soal 2
Tambahkan dokumentasi `@ApiOperation` dan `@ApiResponse` untuk endpoint `POST /users` dengan response 201 dan 400.

### Jawaban 2
```typescript
@Post()
@ApiOperation({ summary: 'Create a new user', description: 'Register a user with email and name' })
@ApiBody({ type: CreateUserDto })
@ApiResponse({ status: 201, description: 'User created', type: UserResponseDto })
@ApiResponse({ status: 400, description: 'Validation error', type: ErrorResponseDto })
async create(@Body() dto: CreateUserDto) {
  return this.userService.create(dto);
}
```

### Soal 3
Apa fungsi `PartialType` dari `@nestjs/swagger`? Kapan menggunakannya?

### Jawaban 3
`PartialType(CreatePostDto)` membuat DTO baru dengan semua properti dari CreatePostDto menjadi optional. Digunakan untuk Update DTO karena saat update, user mungkin hanya mengirim beberapa field saja (partial update / PATCH). Ini menghindari duplikasi deklarasi properti.
