# 91 - Docker Compose Production - Nginx, SSL & Semua Service

## Penjelasan
Setelah Dockerfile siap (multi-stage build), aplikasi NestJS butuh infrastruktur pendukung di production: reverse proxy (Nginx), SSL (HTTPS), database, Redis. Docker Compose mengorkestrasi semua container sebagai satu kesatuan. Sebelumnya kita hanya menjalankan `docker run` untuk satu container.

## Fungsi
- **Nginx reverse proxy**: Menerima request dari internet, meneruskan ke NestJS container.
- **SSL termination**: Nginx meng-handle HTTPS, NestJS di belakangnya tetap HTTP.
- **Let's Encrypt + Certbot**: Otomatis generate/renew SSL certificate gratis.
- **Health check**: Docker mengecek apakah container sehat sebelum menerima traffic.
- **Service orchestration**: Database, Redis, NestJS, Nginx dalam satu network.

## Cara Pengimplementasian

### Struktur Folder
```
project/
├── docker-compose.prod.yml
├── Dockerfile
├── nginx/
│   ├── nginx.conf
│   └── init-letsencrypt.sh
└── .env.prod
```

### `docker-compose.prod.yml`
```yaml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: nestjs-app
    restart: unless-stopped
    environment:
      - NODE_ENV=production
      - DB_HOST=postgres
      - REDIS_HOST=redis
    env_file:
      - .env.prod
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy
    networks:
      - backend
    healthcheck:
      test: ["CMD", "node", "-e", "require('http').get('http://localhost:3000/health', r => process.exit(r.statusCode === 200 ? 0 : 1))"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 20s

  postgres:
    image: postgres:16-alpine
    container_name: postgres-db
    restart: unless-stopped
    environment:
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
      POSTGRES_DB: ${DB_NAME}
    volumes:
      - pgdata:/var/lib/postgresql/data
    networks:
      - backend
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER} -d ${DB_NAME}"]
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:7-alpine
    container_name: redis-cache
    restart: unless-stopped
    volumes:
      - redisdata:/data
    networks:
      - backend
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5

  nginx:
    image: nginx:alpine
    container_name: nginx-proxy
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/ssl:/etc/nginx/ssl:ro
      - certbot-data:/var/www/certbot
    depends_on:
      - app
    networks:
      - backend

  certbot:
    image: certbot/certbot
    container_name: certbot-renew
    volumes:
      - certbot-data:/var/www/certbot
      - ./nginx/ssl:/etc/letsencrypt
    entrypoint: "/bin/sh -c 'trap exit TERM; while :; do certbot renew; sleep 12h & wait $${!}; done;'"

volumes:
  pgdata:
  redisdata:
  certbot-data:

networks:
  backend:
    driver: bridge
```

### `nginx/nginx.conf`
```nginx
events {}

http {
  upstream nestjs {
    server app:3000;
  }

  server {
    listen 80;
    server_name example.com;
    location / {
      return 301 https://$host$request_uri;
    }
    location /.well-known/acme-challenge/ {
      root /var/www/certbot;
    }
  }

  server {
    listen 443 ssl http2;
    server_name example.com;

    ssl_certificate /etc/nginx/ssl/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/nginx/ssl/live/example.com/privkey.pem;

    location / {
      proxy_pass http://nestjs;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
    }
  }
}
```

### Jalankan
```bash
# Generate SSL pertama kali
chmod +x nginx/init-letsencrypt.sh
./nginx/init-letsencrypt.sh

# Jalankan production stack
docker compose -f docker-compose.prod.yml up -d

# Cek health
docker compose -f docker-compose.prod.yml ps
```

## Analogi
Gedung bertingkat sudah jadi (Dockerfile). Sekarang kita pasang: **satpam di pintu masuk** (Nginx — menerima tamu, arahkan ke lantai tujuan), **pintu besi dengan RFID** (SSL — tamu harus pakai kartu akses HTTPS), **ruang server lantai dasar** (PostgreSQL), **ruang penyimpanan cepat** (Redis), dan **petugas jaga malam** (Certbot — periksa kartu akses setiap 12 jam). Semua terhubung dalam satu **koridor internal** (network backend).

## Dipakai untuk apa
- Deployment production dengan domain dan HTTPS.
- Aplikasi yang butuh database, cache, reverse proxy.
- Tim yang ingin infrastruktur konsisten antara staging dan production.
- Setup CI/CD yang auto-deploy ke VPS.

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Port database diexpose ke luar | Database bisa diakses publik | Jangan expose port DB di production |
| SSL certificate expired | Browser kasih warning "Not Secure" | Otomatiskan renewal dengan Certbot |
| Tidak ada health check | Traffic dikirim ke container yang belum siap | Tambahkan `healthcheck` di setiap service |
| Semua service di network berbeda | Service tidak bisa saling komunikasi | Gunakan satu network bersama |
| .env prod ikut commit ke git | Credentials bocor | Tambahkan ke .gitignore, pakai secrets manager |

## Soal Latihan

**Soal 1:** Buat konfigurasi Nginx untuk reverse proxy ke 2 instance NestJS (app1:3000 dan app2:3000) dengan load balancing round-robin.

**Jawaban 1:**
```nginx
upstream nestjs_cluster {
  server app1:3000;
  server app2:3000;
}

server {
  listen 443 ssl http2;
  server_name example.com;

  ssl_certificate /etc/nginx/ssl/live/example.com/fullchain.pem;
  ssl_certificate_key /etc/nginx/ssl/live/example.com/privkey.pem;

  location / {
    proxy_pass http://nestjs_cluster;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
  }
}
```

**Soal 2:** Apa perbedaan `depends_on` tanpa condition vs dengan `condition: service_healthy`?

**Jawaban 2:** Tanpa condition, Docker hanya menunggu container started (bisa jadi container mulai tapi application belum siap menerima koneksi). Dengan `condition: service_healthy`, Docker menunggu hingga health check container sukses, memastikan service benar-benar siap sebelum container dependen mulai.
