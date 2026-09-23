# 90 - Dockerfile Optimal untuk NestJS - Multi-Stage Build

## Penjelasan
Setelah aplikasi NestJS selesai dibangun dan siap di-deploy, kita perlu mengemasnya ke dalam container Docker. Sebelumnya kita menjalankan aplikasi langsung via `npm run start:dev`. Docker memungkinkan aplikasi berjalan konsisten di lingkungan mana pun — dari laptop developer hingga server production. Tantangannya: image Docker default terlalu besar karena membawa node_modules, TypeScript compiler, dan file sumber.

## Fungsi
- **Multi-stage build**: Memisahkan proses *build* (compile TS ke JS) dari *runtime* (hanya menjalankan JS).
- **Alpine base image**: Ukuran minimal (~120 MB vs ~1.2 GB node:20 full).
- **Non-root user**: Keamanan — container tidak berjalan sebagai root.
- **.dockerignore**: Mencegah file tidak perlu (node_modules, .git, test) ikut masuk image.

## Cara Pengimplementasian

### `.dockerignore`
```dockerignore
node_modules
dist
.git
.gitignore
*.md
.env
.env.local
.env.*.local
coverage
test
```

### `Dockerfile` — Multi-Stage Build
```dockerfile
# ========== STAGE 1: Builder ==========
FROM node:20-alpine AS builder

WORKDIR /app

# Copy dependency files
COPY package*.json ./
RUN npm ci --only=production && \
    npm cache clean --force

COPY tsconfig*.json ./
COPY src ./src

# Build TypeScript → JavaScript
RUN npm run build

# ========== STAGE 2: Runner ==========
FROM node:20-alpine AS runner

# Security: non-root user
RUN addgroup -g 1001 -S appgroup && \
    adduser -S appuser -u 1001 -G appgroup

WORKDIR /app

# Copy only production artifacts from builder
COPY --from=builder --chown=appuser:appgroup /app/dist ./dist
COPY --from=builder --chown=appuser:appgroup /app/node_modules ./node_modules
COPY --from=builder --chown=appuser:appgroup /app/package*.json ./

# Non-root user
USER appuser

EXPOSE 3000

CMD ["node", "dist/main"]
```

### Build & Tag
```bash
docker build -t nestjs-app:latest .
docker images nestjs-app  # Lihat ukuran
```

## Analogi
Membangun gedung bertingkat. Tahap *builder* adalah kontraktor yang membawa semua alat berat, semen, besi — area kerja kotor dan besar. Setelah gedung jadi, kontraktor pergi. Tahap *runner* adalah penghuni yang masuk ke gedung jadi — hanya bawa furnitur (JS files), tanpa alat berat (TypeScript compiler, dev dependencies). Gedung tetap kokoh tapi jauh lebih ringan.

## Dipakai untuk apa
- Deployment production — image kecil = download cepat, startup cepat.
- CI/CD pipeline — push image ke registry (Docker Hub / GHCR).
- Environment konsisten — dev, staging, production pakai image sama.

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Tidak pakai multi-stage | Image >1.5 GB | Pisahkan builder dan runner stage |
| `npm install` bukan `npm ci` | Versi dependency tidak deterministik | Gunakan `npm ci` |
| Root user di container | Jika container di-hack, attacker punya akses root host | Buat non-root user |
| COPY seluruh folder tanpa .dockerignore | Image membawa .git, node_modules asli | Buat .dockerignore ketat |
| Lupa `npm cache clean` | Image layer cache besar | Hapus cache di stage builder |

## Soal Latihan

**Soal 1:** Buat Dockerfile multi-stage untuk NestJS yang menggunakan `node:20-alpine`, non-root user UID 1001, dan hanya mengkopi folder `dist` dan `node_modules` ke stage final. Berapa ukuran image sebelum dan sesudah?

**Jawaban 1:**
```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY tsconfig*.json ./
COPY src ./src
RUN npm run build

FROM node:20-alpine AS runner
RUN addgroup -g 1001 -S appgroup && adduser -S appuser -u 1001 -G appgroup
WORKDIR /app
COPY --from=builder --chown=appuser:appgroup /app/dist ./dist
COPY --from=builder --chown=appuser:appgroup /app/node_modules ./node_modules
COPY --from=builder --chown=appuser:appgroup /app/package*.json ./
USER appuser
EXPOSE 3000
CMD ["node", "dist/main"]
```
**Perbandingan ukuran:**
- Tanpa multi-stage (satu stage): ~1.2 GB
- Dengan multi-stage + alpine: ~200-250 MB
- Penghematan: ~80%

**Soal 2:** Apa yang terjadi jika kita lupa menambahkan `.dockerignore` dan ada folder `node_modules` lokal?

**Jawaban 2:** Folder `node_modules` lokal (arsitektur host Windows/Mac) akan di-copy ke image, menimpa hasil `npm ci` di builder stage. Image menjadi lebih besar, dan bisa error karena binary native tidak cocok dengan arsitektur Linux container. Selalu buat `.dockerignore` yang mencantumkan `node_modules`.
