# 113. Cloud Deployment — VPS & PaaS

**Benang Merah**: Materi 112 (CI/CD) otomatis build dan push Docker image. Sekarang image itu harus **dijalankan di server sungguhan** — di cloud.

---

## Penjelasan

Cloud deployment = menjalankan aplikasi di server yang bisa diakses internet 24/7. Ada 2 pendekatan:
- **PaaS** (Platform as a Service): Railway, Render, Heroku — tinggal push code, urusan server diurus penyedia.
- **VPS** (Virtual Private Server): DigitalOcean, Linode, AWS EC2 — server virtual penuh kontrol, urusan setup sendiri.

```bash
# === PaaS: Railway ===
# Cukup hubungkan GitHub repo, Railway otomatis deploy

# === VPS: DigitalOcean ===
# Setup Nginx sebagai reverse proxy
server {
    listen 80;
    server_name myapp.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

---

## Fungsi

Membuat aplikasi bisa **diakses publik** — tidak cuma di `localhost`. Memberi alamat IP/domain, SSL, dan skalabilitas.

---

## Cara Pengimplementasian

### PAAS: Railway / Render

#### Railway (paling mudah)
1. Push code ke GitHub
2. Buka [railway.app](https://railway.app) → New Project → Deploy from GitHub
3. Railway auto-detect Node.js → auto install & run
4. Tambahkan PostgreSQL/Redis dari dashboard (1 klik)
5. Domain otomatis: `myapp.up.railway.app`

#### Render
1. Dashboard → New Web Service → Connect GitHub repo
2. Set:
   - Build Command: `npm install && npm run build`
   - Start Command: `npm start`
3. Tambahkan environment variable di dashboard
4. Domain: `myapp.onrender.com`

### VPS: DigitalOcean / Linode

#### 1. Setup VPS (Ubuntu 22.04)
```bash
# SSH ke server
ssh root@IP_ADDRESS

# Update & upgrade
apt update && apt upgrade -y

# Install Docker
curl -fsSL https://get.docker.com | bash

# Install Nginx
apt install -y nginx

# Install certbot (SSL)
apt install -y certbot python3-certbot-nginx
```

#### 2. Deploy Aplikasi
```bash
# Clone project
git clone https://github.com/username/my-app.git /var/www/my-app
cd /var/www/my-app

# Setup env
cp .env.production .env

# Jalankan dengan Docker Compose
docker-compose up -d
```

#### 3. Nginx — Reverse Proxy
Buat file `/etc/nginx/sites-available/myapp`:
```nginx
server {
    listen 80;
    server_name myapp.com www.myapp.com;

    # Frontend (static files atau SPA)
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    # Backend API
    location /api {
        proxy_pass http://localhost:4000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

Aktifkan:
```bash
ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/
nginx -t           # test config
systemctl reload nginx
```

#### 4. SSL — Let's Encrypt
```bash
# Install SSL otomatis
certbot --nginx -d myapp.com -d www.myapp.com

# Ini akan:
# 1. Verifikasi domain
# 2. Generate sertifikat
# 3. Update config Nginx (listen 443, redirect 80→443)
# 4. Auto-renew setiap 90 hari

# Test renewal
certbot renew --dry-run
```

Hasil config Nginx setelah SSL:
```nginx
server {
    listen 443 ssl;
    server_name myapp.com;

    ssl_certificate /etc/letsencrypt/live/myapp.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/myapp.com/privkey.pem;

    location / {
        proxy_pass http://localhost:3000;
    }
}

server {
    listen 80;
    server_name myapp.com;
    return 301 https://$server_name$request_uri;  # redirect ke HTTPS
}
```

#### 5. Firewall
```bash
# UFW — Uncomplicated Firewall
ufw allow ssh
ufw allow http
ufw allow https
ufw enable
ufw status
```

---

## Analogi: Membangun Rumah (Pindah ke Perumahan Baru)

| Konsep | Analogi Pindah Rumah |
|---|---|
| PaaS (Railway/Render) | **Rumah jadi** — tinggal bawa koper, listrik & air sudah siap |
| VPS | **Tanah kosong** — bangun rumah sendiri dari fondasi |
| Nginx | **Satpam/pintu utama** — tamu datang ke satpam, diarahkan ke rumah yang benar |
| SSL (Let's Encrypt) | **Stempel resmi** + gembok pintu — "rumah ini aman & terverifikasi" |
| Domain | **Alamat rumah** — bukan koordinat, pakai nama jalan |
| IP Address | **Koordinat GPS** — tepat tapi susah diingat |
| Firewall (UFW) | **Pagar rumah** — hanya orang tertentu yang bisa lewat |
| SSH | **Kunci pintu belakang** — akses pribadi untuk pemilik |
| Docker | **Kontainer** untuk mindahin perabot utuh |
| Certbot | **Tukang kunci** — pasang gembok otomatis, perpanjang sendiri |

PaaS = rumah di perumahan siap huni. Tinggal tanda tangan, masuk, tinggal. VPS = beli tanah, bangun dari nol: pondasi (OS), pipa (Nginx), gembok (SSL). Nginx adalah satpam — tamu datang, satpam tanya "mau ke mana?", lalu antar ke rumah yang benar.

---

## Dipakai Untuk Apa

- **Production hosting** — aplikasi diakses pengguna sungguhan
- **Staging/QA environment** — test sebelum production
- **API server** — backend untuk mobile/web app
- **Static site hosting** — dengan Nginx + Cloudflare
- **Microservices** — beberapa server untuk service berbeda

---

## Kesalahan Umum

| Kesalahan | Akibat |
|---|---|
| Pakai PaaS untuk app berat | Biaya membengkak — VPS lebih murah untuk resource besar |
| Lupa firewall | Server bisa diakses sembarang orang |
| Nginx tidak direload setelah ubah config | Config baru tidak berlaku |
| SSL expired (lupa renew) | Browser peringatan "Not Secure" |
| `.env` bocor ke git | Credential production ada di repo publik |
| Port 3000 langsung dibuka | Byak bisa akses app langsung tanpa Nginx |
| Tidak pakai `proxy_set_header` | App tidak tahu IP asli pengguna |

---

## Hubungan dengan Materi Sebelumnya/Selanjutnya

- **Materi 112 (CI/CD)**: Pipeline deploy otomatis ke VPS ini
- **Materi 110 (Docker)**: App di-dockerize, jalan di VPS
- **Materi 111 (Docker Compose)**: Fullstack app jalan di VPS
- **Materi 114 (Monitoring)**: Setelah deployed, perlu monitoring

---

## Soal Latihan

### Soal 1 (Mudah)
Jelaskan perbedaan PaaS vs VPS dalam 3 poin:

**Jawaban**:
1. **Kontrol**: PaaS terbatas (tidak bisa akses OS), VPS penuh (root akses)
2. **Setup**: PaaS 5 menit (push code saja), VPS 1-2 jam (setup OS, Nginx, SSL, dll)
3. **Harga**: PaaS bayar per usage (bisa mahal untuk traffic besar), VPS fixed price per bulan
4. **Skalabilitas**: PaaS scale otomatis, VPS scale manual (upgrade RAM/CPU)

### Soal 2 (Sedang)
Buat konfigurasi Nginx untuk:
- Domain: `api.tokoku.com`
- Aplikasi Express di port `4000`
- Static file dari folder `/var/www/tokoku/public` untuk path `/static`
- SSL sudah terinstall
- Redirect HTTP ke HTTPS

**Jawaban**:
```nginx
server {
    listen 80;
    server_name api.tokoku.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name api.tokoku.com;

    ssl_certificate /etc/letsencrypt/live/api.tokoku.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/api.tokoku.com/privkey.pem;

    location /static {
        root /var/www/tokoku/public;
        expires 30d;
        add_header Cache-Control "public, immutable";
    }

    location / {
        proxy_pass http://localhost:4000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Soal 3 (Tantangan)
Kamu deploy app ke VPS. Buat script deploy `deploy-vps.sh` yang:
1. SSH ke server
2. Pull perubahan dari git
3. Build ulang Docker image
4. Jalankan migration database
5. Restart container dengan zero-downtime (rolling update)
6. Rollback jika health check gagal

**Jawaban**:
```bash
#!/bin/bash
set -e

SERVER="deploy@123.456.789.0"
APP_DIR="/var/www/my-app"
COMPOSE_FILE="docker-compose.prod.yml"

echo "=== Deploy ke VPS ==="

ssh "$SERVER" << 'EOF'
    set -e
    
    cd /var/www/my-app
    
    echo "1. Pull latest code..."
    git pull origin main
    
    echo "2. Build image baru..."
    docker-compose -f docker-compose.prod.yml build
    
    echo "3. Backup container lama..."
    docker-compose -f docker-compose.prod.yml ps -q app | head -1 > /tmp/old-container-id.txt
    
    echo "4. Migration database..."
    docker-compose -f docker-compose.prod.yml run --rm app npm run migrate
    
    echo "5. Rolling update (scale up + scale down)..."
    # Start container baru di samping yang lama
    docker-compose -f docker-compose.prod.yml up -d --no-deps --scale app=2 --no-recreate app
    
    sleep 5
    
    echo "6. Health check..."
    STATUS=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:3000/health)
    
    if [[ "$STATUS" != "200" ]]; then
        echo "Health check gagal! Rollback..."
        CONTAINER_OLD=$(cat /tmp/old-container-id.txt)
        docker-compose -f docker-compose.prod.yml up -d --no-deps --scale app=1 --no-recreate app
        docker stop "$(docker ps -q --filter "name=my-app" | grep -v "$CONTAINER_OLD")"
        docker rm "$(docker ps -aq --filter "name=my-app" | grep -v "$CONTAINER_OLD")" 2>/dev/null || true
        echo "Rollback selesai."
        exit 1
    fi
    
    echo "7. Matikan container lama..."
    CONTAINER_OLD=$(cat /tmp/old-container-id.txt)
    docker stop "$CONTAINER_OLD"
    docker rm "$CONTAINER_OLD" 2>/dev/null || true
    docker-compose -f docker-compose.prod.yml up -d --no-deps --scale app=1 --no-recreate app
    
    echo "8. Bersihkan image lama..."
    docker image prune -f
    
    echo "=== Deploy sukses! ==="
EOF
```
