# 8 - Filosofi & Arsitektur NestJS - Mengapa Opinionated Framework Penting

## Penjelasan

Setelah menguasai TypeScript dan memahami dasar OOP/FP, sekarang saatnya masuk ke **NestJS**. Bayangkan kamu punya lahan kosong (proyek Express). Dengan Express, kamu bebas membangun sesukamu — tapi tidak ada cetak biru, tidak ada aturan letak ruangan, tidak ada standar kelistrikan. Hasilnya? Tiap developer membangun dengan gaya masing-masing. Proyek besar jadi kacau.

NestJS hadir sebagai **arsitek gedung** yang memberi aturan baku: "Ruangan harus seperti ini, kabel listrik lewat sini, pipa air lewat sini." Inilah yang disebut **opinionated framework** — framework yang punya pendapat tentang bagaimana kode harus diorganisir.

## Fungsi

- Memberikan **struktur standar** untuk aplikasi server-side
- Mengadopsi pola arsitektur dari Angular (modular, DI, decorator) agar konsisten dan scalable
- Memisahkan tanggung jawab secara jelas melalui komponen arsitektural
- Meningkatkan maintainability dan testability kode

## Cara Pengimplementasian / Code

### NestJS vs Express Biasa

**Express (tanpa aturan):**
```typescript
// express-app.js — semrawut, tidak ada standar
const express = require('express');
const app = express();

app.get('/users', async (req, res) => {
  const users = await db.query('SELECT * FROM users');
  res.json(users);
  // logic, query, response — semua di satu tempat
});

app.listen(3000);
```

**NestJS (dengan arsitektur):**
```typescript
// users.controller.ts — hanya handle request/response
@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  async findAll(): Promise<User[]> {
    return this.usersService.findAll();
  }
}

// users.service.ts — hanya handle business logic
@Injectable()
export class UsersService {
  async findAll(): Promise<User[]> {
    return this.userRepository.find();
  }
}

// users.module.ts — mengikat semuanya
@Module({
  controllers: [UsersController],
  providers: [UsersService],
})
export class UsersModule {}
```

### Overview Semua Komponen

```typescript
@Module({
  imports: [DatabaseModule],      // Module lain
  controllers: [UserController],  // Handle HTTP
  providers: [UserService],       // Business logic
})
export class AppModule {}
```

| Komponen | Analogi Gedung | Fungsi |
|----------|----------------|--------|
| **Module** | Lantai gedung | Unit organisasi kode |
| **Controller** | Resepsionis | Menerima tamu (request), mengarahkan |
| **Provider** | Tukang/tenaga ahli | Menjalankan logika bisnis |
| **Guard** | Satpam | Memeriksa izin akses |
| **Pipe** | Security gate | Transformasi & validasi data masuk |
| **Interceptor** | CCTV | Membungkus request/response untuk logging, transformasi |
| **Exception Filter** | Regu pemadam | Menangani error dengan rapi |

## Analogi (Gedung Bertingkat)

NestJS adalah **arsitek gedung pencakar langit**. Express seperti memberikan kamu sebidang tanah dan sekantong semen — bebas bangun sesukamu, tapi begitu gedung makin tinggi, strukturnya ambruk karena tidak ada perencanaan.

NestJS datang dengan **cetak biru (blueprint)** yang jelas:
- **Module** = lantai gedung (setiap lantai punya fungsi spesifik)
- **Controller** = resepsionis di tiap lantai
- **Provider** = staf/pekerja yang mengerjakan tugas
- **Guard** = satpam yang periksa KTP
- **Pipe** = mesin X-Ray yang memvalidasi barang bawaan
- **Interceptor** = CCTV yang merekam semua aktivitas
- **Exception Filter** = alat pemadam kebakaran + P3K

Dengan cetak biru ini, 10 developer berbeda bisa membangun 10 lantai berbeda tanpa saling tabrak.

## Dipakai Untuk Apa

- Aplikasi **enterprise** berskala besar yang butuh struktur jelas
- Tim **banyak developer** yang perlu standar kode seragam
- Proyek **microservice** atau **monolith** yang diperkirakan akan berkembang pesat
- Aplikasi yang membutuhkan **testabilitas tinggi** (DI memudahkan mocking)

## Kesalahan Umum

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| Memaksakan NestJS untuk proyek kecil (API 3 endpoint) | Over-engineering | Gunakan Express/Fastify saja |
| Menaruh semua logic di Controller | Controller jadi gemuk | Pisahkan ke Service/Provider |
| Tidak menggunakan Module | Kode tidak terorganisir | Setiap fitur punya module sendiri |
| Menganggap NestJS "hanya Express dengan class" | Tidak memanfaatkan DI, Guard, Pipe | Pelajari filosofi NestJS dengan benar |

## Soal Latihan & Jawaban

### Soal 1
Gambarkan dari memory diagram arsitektur NestJS berikut ini: urutan dari request masuk hingga response keluar, sebutkan komponen-komponen yang terlibat.

**Jawaban:**

```
Request Masuk
    │
    ▼
┌─────────────┐
│  Middleware  │  (Logger, Cors, Helmet)
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Guard     │  (Autentikasi, Authorisasi)
└──────┬──────┘
       │
       ▼
┌──────────────────┐
│ Interceptor (before) │  (Logging, Transformasi request)
└──────┬───────────┘
       │
       ▼
┌─────────────┐
│    Pipe     │  (Validasi, Transformasi data)
└──────┬──────┘
       │
       ▼
┌──────────────────┐
│ Controller/Handler │  (Route handler)
└──────┬───────────┘
       │
       ▼
┌─────────────┐
│  Service/Provider │  (Business logic)
└──────┬──────┘
       │
       ▼
┌──────────────────┐
│ Interceptor (after) │  (Transformasi response, mapping)
└──────┬───────────┘
       │
       ▼
┌──────────────────┐
│ Exception Filter  │  (Error handling jika ada error)
└──────────────────┘
       │
       ▼
   Response Keluar
```

### Soal 2
Apa perbedaan utama antara NestJS dan Express dalam hal arsitektur?

**Jawaban:**
Express tidak memberikan aturan struktur — developer bebas mengatur file apapun. NestJS memberikan arsitektur baku berbasis Module → Controller → Provider yang memisahkan tanggung jawab secara jelas. NestJS juga memiliki system tambahan seperti Guard, Pipe, Interceptor yang tidak ada di Express secara native.

### Soal 3
Sebutkan 3 komponen arsitektur NestJS dan analoginya dalam gedung bertingkat.

**Jawaban:**
1. **Module** = Lantai gedung — membagi area sesuai fungsi (lantai HRD, lantai Keuangan)
2. **Controller** = Resepsionis — menerima tamu dan mengarahkan ke bagian yang tepat
3. **Guard** = Satpam — memeriksa izin akses sebelum tamu masuk
