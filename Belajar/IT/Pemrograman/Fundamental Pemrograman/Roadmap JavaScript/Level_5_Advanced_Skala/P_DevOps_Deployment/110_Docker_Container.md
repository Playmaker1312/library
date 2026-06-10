# 110. Docker — Containerization

**Benang Merah**: Selama ini aplikasi jalan di **komputer kita**. Saat mau di-deploy, sering muncul masalah "di komputer saya bisa". Docker menyelesaikan ini dengan **container** — lingkungan yang konsisten di mana pun.

---

## Penjelasan

Docker adalah platform untuk **mengemas aplikasi + semua dependensinya** dalam satu unit bernama **container**. Container ibarat **kontainer pengiriman standar** — isinya bisa apa saja (baju, elektronik, makanan), tapi formatnya sama sehingga bisa diangkut kapal, truk, kereta tanpa modifikasi.

```dockerfile
# Dockerfile — resep untuk membuat image
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "index.js"]
```

```bash
# Build image dari Dockerfile
docker build -t my-app .

# Jalankan container
docker run -p 3000:3000 my-app
```

> **Image** = cetakan / blueprint (read-only)
> **Container** = hasil cetakan yang berjalan (read-write)

---

## Fungsi

Memastikan aplikasi **jalan sama persis** di laptop developer, server test, server staging, dan server production — menghilangkan "works on my machine".

---

## Cara Pengimplementasian

### 1. Install Docker
Download dari [docker.com](https://docker.com) dan install Docker Desktop.

### 2. Dockerfile untuk Node.js App
```dockerfile
# Gunakan Node.js versi 18 (ringan)
FROM node:18-alpine

# Tentukan folder kerja di dalam container
WORKDIR /app

# Copy package.json dulu (cache layer)
COPY package.json package-lock.json ./

# Install dependencies
RUN npm install --production

# Copy semua file aplikasi
COPY . .

# Port yang akan dibuka
EXPOSE 3000

# Perintah saat container jalan
CMD ["node", "index.js"]
```

### 3. Build & Run
```bash
# Build image (butuh waktu, terutama pertama kali)
docker build -t my-api .

# Lihat daftar image
docker images

# Jalankan container
docker run -d -p 3000:3000 --name my-api-container my-api

# Lihat container yang jalan
docker ps

# Lihat log
docker logs my-api-container

# Stop container
docker stop my-api-container

# Hapus container
docker rm my-api-container
```

### 4. .dockerignore
```dockerignore
node_modules
npm-debug.log
.env
.git
.gitignore
```

### 5. Volume — data persist
```bash
# Bind mount: folder lokal terhubung ke folder container
docker run -v /path/lokal/data:/app/data my-api

# Named volume (Docker yang manage)
docker run -v my-data:/app/data my-api
```

---

## Analogi: Membangun Rumah (Kontainer Pengiriman)

| Docker | Kontainer Pengiriman Standar |
|---|---|
| Image | Kontainer kosong (standar) |
| Container | Kontainer yang sudah diisi dan dikirim |
| Dockerfile | Resep pengemasan: "masukkan ini, lalu itu" |
| Docker Hub | Pelabuhan utama tempat kontainer disimpan |
| `docker pull` | Ambil kontainer dari pelabuhan |
| `docker push` | Kirim kontainer ke pelabuhan |
| Volume | Gudang eksternal — isi tetap ada meski kontainer dipindah |

Bayangkan Anda punya **kontainer pengiriman ukuran standar**. Anda isi dengan rumah mini lengkap — semua perabot, pipa, kabel sudah terpasang. Kontainer ini bisa diangkut kapal ke negara mana pun. Setelah sampai, tinggal colok listrik — rumah siap huni. Itulah Docker: standarisasi sehingga "di komputer saya bisa" = "di mana pun bisa".

---

## Dipakai Untuk Apa

- **Deployment** — aplikasi di server production
- **Development** — lingkungan dev yang konsisten antar tim
- **CI/CD** — test di container yang sama dengan production
- **Microservices** — setiap service di container sendiri
- **Local development** — database di container (PostgreSQL, Redis)

---

## Kesalahan Umum

| Kesalahan | Akibat |
|---|---|
| Lupa `.dockerignore` | `node_modules` ikut tercopy → image besar |
| Image terlalu besar | Pakai `node:18` (300MB) vs `node:18-alpine` (100MB) |
| Hardcode konfigurasi | Port, DB, dll seharusnya via env variable |
| Container berhenti tiba-tiba | App crash, cek log dengan `docker logs` |
| Permission issue di volume | File dari host punya owner berbeda |

---

## Hubungan dengan Materi Sebelumnya

Docker adalah **jembatan** dari development ke production:
- Materi 57 (Express) → App yang akan di-dockerize
- Materi 63 (PostgreSQL) → Database di container terpisah
- Materi 108 (Caching → Redis) → Redis di container
- Materi 111 (Docker Compose) → Gabung semua container
- Materi 112 (CI/CD) → Build & deploy dengan Docker

---

## Soal Latihan

### Soal 1 (Mudah)
Tulis Dockerfile sederhana untuk aplikasi Node.js Express yang:
- Pakai node:18-alpine
- Copy package.json, jalankan npm install
- Copy semua file
- Buka port 3000
- Jalankan dengan `node index.js`

**Jawaban**:
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "index.js"]
```

### Soal 2 (Sedang)
Jalankan container Postgres dengan Docker. Perintah apa yang digunakan? Container harus:
- Bernama `my-postgres`
- Port 5432
- Password root: `mysecretpassword`
- Data persist di volume `pgdata`

**Jawaban**:
```bash
docker run -d \
  --name my-postgres \
  -e POSTGRES_PASSWORD=mysecretpassword \
  -p 5432:5432 \
  -v pgdata:/var/lib/postgresql/data \
  postgres:16-alpine
```

### Soal 3 (Tantangan)
Buat Dockerfile multi-stage untuk aplikasi Node.js yang di-production:
- **Stage 1 (build)**: Install ALL dependencies, build
- **Stage 2 (production)**: Copy HANYA file yang diperlukan + production dependencies

**Jawaban**:
```dockerfile
# Stage 1: Build
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install              # install semua (dev + prod)
COPY . .
RUN npm run build            # misal ada build step

# Stage 2: Production
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install --production # hanya production deps
COPY --from=builder /app/dist ./dist   # copy hasil build
EXPOSE 3000
CMD ["node", "dist/index.js"]
```
Image akhir lebih kecil karena tidak mengandung devDependencies dan source files.
