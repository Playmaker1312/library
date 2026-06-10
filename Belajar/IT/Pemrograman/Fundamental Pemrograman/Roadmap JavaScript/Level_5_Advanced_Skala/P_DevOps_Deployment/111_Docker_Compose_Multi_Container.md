# 111. Docker Compose — Multi-Container Orchestration

**Benang Merah**: Materi 110 mengajarkan satu container. Tapi aplikasi nyata tidak cuma satu — ada **app + database + Redis + frontend**. Docker Compose mengatur semuanya dengan satu perintah.

---

## Penjelasan

Docker Compose adalah alat untuk **mendefinisikan dan menjalankan banyak container** sebagai satu kesatuan. Dengan satu file `docker-compose.yml`, kita tentukan: container apa saja, network antar mereka, volume untuk persistensi, dan environment variable masing-masing.

```yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    build: ./app
    ports:
      - "3000:3000"
    depends_on:
      - db
      - redis
  db:
    image: postgres:16-alpine
    volumes:
      - pgdata:/var/lib/postgresql/data
  redis:
    image: redis:7-alpine

volumes:
  pgdata:
```

```bash
# Jalankan semua container
docker-compose up -d

# Lihat log semua service
docker-compose logs -f

# Hentikan semua
docker-compose down
```

---

## Fungsi

Mengorkestrasi **multi-container** dalam satu definisi — menghilangkan ribetnya menjalankan `docker run` berkali-kali dengan network dan volume manual.

---

## Cara Pengimplementasian

### 1. Struktur Project Fullstack
```
project/
├── frontend/          # React / Vue app
├── backend/           # Express / Fastify API
├── docker-compose.yml
└── .env
```

### 2. docker-compose.yml — Fullstack App
```yaml
version: '3.8'

services:
  # ===== FRONTEND =====
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "80:80"              # Nginx di port 80
    depends_on:
      - backend
    networks:
      - app-network

  # ===== BACKEND API =====
  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    env_file:
      - .env
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/myapp
      - REDIS_URL=redis://redis:6379
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_started
    volumes:
      - ./backend:/app        # bind mount untuk dev (hot reload)
      - /app/node_modules
    networks:
      - app-network

  # ===== POSTGRESQL =====
  db:
    image: postgres:16-alpine
    restart: always
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: myapp
    volumes:
      - pgdata:/var/lib/postgresql/data
      - ./db/init.sql:/docker-entrypoint-initdb.d/init.sql
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user -d myapp"]
      interval: 5s
      timeout: 5s
      retries: 5
    networks:
      - app-network

  # ===== REDIS =====
  redis:
    image: redis:7-alpine
    restart: always
    volumes:
      - redis-data:/data
    networks:
      - app-network

networks:
  app-network:
    driver: bridge

volumes:
  pgdata:
  redis-data:
```

### 3. Dockerfile Backend
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "run", "dev"]    # untuk development
```

### 4. Dockerfile Frontend (Nginx)
```dockerfile
# Stage 1: Build
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Serve with Nginx
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### 5. Perintah Penting
```bash
# Build & jalankan semua
docker-compose up --build -d

# Lihat status
docker-compose ps

# Log service tertentu
docker-compose logs -f backend

# Masuk ke container
docker-compose exec backend sh

# Reset database (hapus volume)
docker-compose down -v

# Scale service (backend 3 instance)
docker-compose up -d --scale backend=3
```

---

## Analogi: Membangun Rumah (Kompleks Perumahan)

| Docker Compose | Kompleks Perumahan |
|---|---|
| `docker-compose.yml` | Denah kompleks — ada rumah apa saja, di mana |
| Frontend container | Rumah tamu — yang dilihat pengunjung |
| Backend container | Rumah utama — tempat kerja & logika bisnis |
| PostgreSQL container | Gudang penyimpanan dokumen |
| Redis container | Kotak surat cepat — catatan sementara |
| Network (bridge) | Jalan di kompleks — setiap rumah terhubung |
| Volume | Utilitas bersama (PDAM, listrik) — data tetap ada |
| `depends_on` | Urutan pembangunan — fondasi dulu baru tembok |
| Port mapping (`3000:3000`) | Nomor rumah — alamat di dunia luar |
| `docker-compose up` | Tombol "bangun kompleks" — semua rumah jadi sekaligus |

Bayangkan kompleks perumahan: setiap rumah punya fungsi spesifik (tempat tinggal, kantor, gudang). Ada jalan yang menghubungkan semua rumah (network). Utilitas air dan listrik (volume) tetap ada meski rumah direnovasi. Semua diatur dalam satu denah (`docker-compose.yml`) — sekali eksekusi, semua rumah berdiri.

---

## Dipakai Untuk Apa

- **Fullstack app development** — frontend + backend + DB + cache lokal
- **Microservices** — banyak service kecil yang saling komunikasi
- **Local development** — environment yang sama dengan production
- **Testing** — spin up container test, matikan setelah selesai
- **Data pipeline** — worker + queue + database + scheduler

---

## Kesalahan Umum

| Kesalahan | Akibat |
|---|---|
| Lupa `depends_on` | Container start bareng, app crash karena DB belum siap |
| Hardcode credential di compose file | Bocor ke git — gunakan `.env` |
| Tidak pakai healthcheck | App crash karena koneksi DB ditolak |
| Lupa `.dockerignore` node_modules | Bind mount timpa isi container |
| Volume tidak di-declare di root `volumes:` | Error "service "db" refers to undefined volume" |
| Lupa `--build` setelah ubah Dockerfile | Container pakai image lama |

---

## Hubungan dengan Materi Sebelumnya/Selanjutnya

- **Materi 110 (Docker)**: Basis — Docker Compose adalah lapisan di atas Docker
- **Materi 63 (PostgreSQL)**: Database di container sendiri
- **Materi 108 (Caching → Redis)**: Redis sebagai container terpisah
- **Materi 112 (CI/CD)**: Pipeline build & deploy dengan Docker Compose

---

## Soal Latihan

### Soal 1 (Mudah)
Apa perbedaan perintah berikut?
1. `docker-compose up`
2. `docker-compose up -d`
3. `docker-compose up --build`
4. `docker-compose down`

**Jawaban**:
1. `up` — build (jika perlu), create, start semua container (foreground)
2. `up -d` — sama tapi **detached** (background)
3. `up --build` — **paksa build ulang** image sebelum start
4. `down` — stop & hapus semua container + network (volume tetap kecuali `-v`)

### Soal 2 (Sedang)
Buat `docker-compose.yml` untuk:
- **API**: Express di port 4000, butuh environment `DATABASE_URL` dan `REDIS_URL`
- **PostgreSQL**: user `admin`, pass `secret`, db `shop`
- **Redis**: default
- Semua dalam satu network bernama `shop-network`
- Data postgres persist di volume `shop-db-data`

**Jawaban**:
```yaml
version: '3.8'

services:
  api:
    build: .
    ports:
      - "4000:4000"
    environment:
      - DATABASE_URL=postgresql://admin:secret@db:5432/shop
      - REDIS_URL=redis://redis:6379
    depends_on:
      - db
      - redis
    networks:
      - shop-network

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: shop
    volumes:
      - shop-db-data:/var/lib/postgresql/data
    networks:
      - shop-network

  redis:
    image: redis:7-alpine
    networks:
      - shop-network

networks:
  shop-network:
    driver: bridge

volumes:
  shop-db-data:
```

### Soal 3 (Tantangan)
Kamu punya 3 service: `auth-service`, `order-service`, `notification-service`. Setiap service perlu:
- Redis untuk cache
- Database sendiri (auth → MongoDB, order → PostgreSQL, notification → SQLite file-based)
- `auth-service` harus jalan duluan sebelum `order-service`
- Buat healthcheck untuk setiap database

**Jawaban**:
```yaml
version: '3.8'

services:
  auth-service:
    build: ./auth
    ports:
      - "4001:4001"
    depends_on:
      auth-db:
        condition: service_healthy
      redis:
        condition: service_started
    networks:
      - backend

  order-service:
    build: ./order
    ports:
      - "4002:4002"
    depends_on:
      auth-service:
        condition: service_started
      order-db:
        condition: service_healthy
      redis:
        condition: service_started
    networks:
      - backend

  notification-service:
    build: ./notification
    ports:
      - "4003:4003"
    depends_on:
      redis:
        condition: service_started
    volumes:
      - notifications-data:/app/data
    networks:
      - backend

  auth-db:
    image: mongo:7
    healthcheck:
      test: echo 'db.runCommand("ping").ok' | mongosh --quiet
      interval: 5s
      retries: 5
    volumes:
      - auth-data:/data/db
    networks:
      - backend

  order-db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: order
      POSTGRES_PASSWORD: orderpass
      POSTGRES_DB: orders
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U order"]
      interval: 5s
      retries: 5
    volumes:
      - order-data:/var/lib/postgresql/data
    networks:
      - backend

  redis:
    image: redis:7-alpine
    networks:
      - backend

networks:
  backend:

volumes:
  auth-data:
  order-data:
  notifications-data:
```
