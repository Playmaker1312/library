# 48 - Database Seeding - Data Awal yang Terstruktur

## Penjelasan

Setelah memiliki schema, migration, dan service CRUD (Pertemuan 43-47), kita perlu data awal untuk development dan testing. Menulis data manual lewat studio tidak scalable. Database seeding adalah script yang mengisi database dengan data dummy atau data awal secara terstruktur, reproducible, dan idempotent.

Jika semua komponen sebelumnya adalah **struktur bangunan**, maka seeding adalah **proses "furnishing"** — mengisi setiap ruangan dengan furniture, dekorasi, dan penghuni agar gedung terlihat hidup dan bisa diuji fungsinya.

## Fungsi

- **prisma/seed.ts**: Script TypeScript untuk mengisi data awal
- **Konfigurasi di package.json**: Memberi tahu Prisma cara menjalankan seed
- **@faker-js/faker**: Library untuk generate data dummy realistis
- **Idempotent Seed**: Seed bisa dijalankan berulang kali tanpa duplikasi

## Cara Pengimplementasian

### 1. Install Dependencies

```bash
npm install -D @faker-js/faker ts-node @types/node
```

### 2. Konfigurasi package.json

```json
{
  "prisma": {
    "seed": "ts-node --compiler-options {\"module\":\"CommonJS\"} prisma/seed.ts"
  }
}
```

Atau untuk TypeScript modern:

```json
{
  "prisma": {
    "seed": "ts-node prisma/seed.ts"
  }
}
```

### 3. Membuat Seed Script

```typescript
// prisma/seed.ts
import { PrismaClient } from '@prisma/client';
import { faker } from '@faker-js/faker';

const prisma = new PrismaClient();

async function main() {
  console.log('Seeding database...');

  // Hapus data lama (urutan penting karena foreign key)
  await prisma.comment.deleteMany();
  await prisma.post.deleteMany();
  await prisma.user.deleteMany();

  // Seed 10 Users
  const users = [];
  for (let i = 0; i < 10; i++) {
    const user = await prisma.user.create({
      data: {
        email: faker.internet.email(),
        name: faker.person.fullName(),
        password: faker.internet.password(),
      },
    });
    users.push(user);
  }
  console.log(`Created ${users.length} users`);

  // Seed 50 Posts (5 per user)
  const posts = [];
  for (let i = 0; i < 50; i++) {
    const post = await prisma.post.create({
      data: {
        title: faker.lorem.sentence(),
        content: faker.lorem.paragraphs(3),
        published: faker.datatype.boolean(0.7), // 70% published
        authorId: faker.helpers.arrayElement(users).id,
      },
    });
    posts.push(post);
  }
  console.log(`Created ${posts.length} posts`);

  // Seed 100 Comments
  for (let i = 0; i < 100; i++) {
    await prisma.comment.create({
      data: {
        text: faker.lorem.sentence(),
        postId: faker.helpers.arrayElement(posts).id,
        authorId: faker.helpers.arrayElement(users).id,
      },
    });
  }
  console.log('Created 100 comments');

  console.log('Seeding completed!');
}

main()
  .catch((e) => {
    console.error(e);
    process.exit(1);
  })
  .finally(async () => {
    await prisma.$disconnect();
  });
```

### 4. Idempotent Seed Pattern

```typescript
// prisma/seed.ts — version idempotent
async function main() {
  // Upsert untuk data master yang tetap
  const adminRole = await prisma.role.upsert({
    where: { name: 'ADMIN' },
    update: {},
    create: { name: 'ADMIN' },
  });

  const userRole = await prisma.role.upsert({
    where: { name: 'USER' },
    update: {},
    create: { name: 'USER' },
  });

  // Seed admin user only if not exists
  const admin = await prisma.user.upsert({
    where: { email: 'admin@example.com' },
    update: {},
    create: {
      email: 'admin@example.com',
      name: 'Admin',
      password: 'hashed_password_here',
      roleId: adminRole.id,
    },
  });

  // Seed dummy data — hapus dulu baru buat ulang
  await prisma.comment.deleteMany();
  await prisma.post.deleteMany();

  // ... create dummy posts and comments
}
```

### 5. Menjalankan Seed

```bash
# Jalankan seed
npx prisma db seed

# Reset database + seed
npx prisma migrate reset

# Seed saja tanpa migration
npx prisma db seed
```

### 6. Seed dengan Environment Specific

```typescript
// prisma/seed.ts
async function main() {
  if (process.env.NODE_ENV === 'production') {
    // Di production, hanya seed data master
    await seedMasterData();
  } else {
    // Di development, seed dummy data lengkap
    await seedMasterData();
    await seedDummyData();
  }
}
```

## Analogi

**Membangun Gedung Bertingkat**

- **Seed script** = **proses furnishing** — mengisi gedung dengan meja, kursi, lampu, penghuni
- **Faker.js** = **pabrik furniture otomatis** — bikin ribuan meja dengan warna dan ukuran berbeda
- **deleteMany di awal** = **mengosongkan ruangan** sebelum furnishing ulang
- **Idempotent seed** = **"sudah ada meja ini? skip. Belum ada? buat."**
- **prisma db seed** = **tombol "furnish now"** satu kali pencetan
- **prisma migrate reset** = **bongkar total gedung + furnish ulang dari awal**

## Dipakai untuk Apa

- Development environment dengan data realistis
- Integration testing dengan data yang konsisten
- Demo aplikasi yang langsung terlihat hidup
- Data master (roles, permissions, admin account) di production

## Kesalahan Umum

| Kesalahan | Solusi |
|-----------|--------|
| Tidak hapus data lama → duplikasi setiap seed | Delete existing data di awal atau pakai upsert |
| Foreign key error saat hapus data | Hapus dari tabel anak ke induk (comment → post → user) |
| Password tidak di-hash | Gunakan bcrypt untuk hash password di seed |
| Data faker dalam bahasa Inggris | Konfigurasi locale: `faker.locale = 'id_ID'` |
| Lupa `await prisma.$disconnect()` | Pakai `.finally()` untuk disconnect |

## Soal Latihan

Buat seed script yang:
1. Hapus data lama (comments → posts → users)
2. Seed 10 user dengan faker
3. Seed 50 post (5 per user, random published status)
4. Seed 100 comment (random post + user)
5. Pastikan idempotent — bisa dijalankan berulang kali

### Jawaban

```typescript
// prisma/seed.ts
import { PrismaClient } from '@prisma/client';
import { faker } from '@faker-js/faker';

const prisma = new PrismaClient();

async function main() {
  console.log('Seeding started...');

  // Clean existing data
  await prisma.comment.deleteMany();
  await prisma.post.deleteMany();
  await prisma.user.deleteMany();

  // Create 10 users
  const users = [];
  for (let i = 0; i < 10; i++) {
    const user = await prisma.user.create({
      data: {
        email: faker.internet.email(),
        name: faker.person.fullName(),
        password: faker.internet.password({ length: 10 }),
      },
    });
    users.push(user);
  }
  console.log(`Created ${users.length} users`);

  // Create 50 posts
  const posts = [];
  for (let i = 0; i < 50; i++) {
    const author = faker.helpers.arrayElement(users);
    const post = await prisma.post.create({
      data: {
        title: faker.lorem.sentence({ min: 5, max: 10 }),
        content: faker.lorem.paragraphs(2),
        published: faker.datatype.boolean(0.8),
        authorId: author.id,
      },
    });
    posts.push(post);
  }
  console.log(`Created ${posts.length} posts`);

  // Create 100 comments
  for (let i = 0; i < 100; i++) {
    await prisma.comment.create({
      data: {
        text: faker.lorem.sentence(),
        postId: faker.helpers.arrayElement(posts).id,
        authorId: faker.helpers.arrayElement(users).id,
      },
    });
  }
  console.log('Created 100 comments');

  console.log('Seeding completed!');
}

main()
  .catch((e) => {
    console.error('Seed failed:', e);
    process.exit(1);
  })
  .finally(async () => {
    await prisma.$disconnect();
  });
```

**package.json:**
```json
{
  "prisma": {
    "seed": "ts-node prisma/seed.ts"
  }
}
```
