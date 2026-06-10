# 112. CI/CD — GitHub Actions

**Benang Merah**: Materi 111 (Docker Compose) membuat aplikasi siap di-container. Tapi setiap kali ada perubahan kode, kita harus **manual** build dan deploy. CI/CD mengotomatisasi semuanya.

---

## Penjelasan

CI/CD = **Continuous Integration / Continuous Deployment**. CI: setiap push ke repo, kode di-test otomatis. CD: jika test lulus, deploy otomatis ke server. GitHub Actions adalah platform CI/CD bawaan GitHub.

```yaml
# .github/workflows/deploy.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 18
      - run: npm ci
      - run: npm test

  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploy to server..."
```

---

## Fungsi

**Otomatisasi** — dari commit ke production tanpa sentuhan manual. Setiap perubahan kode: test otomatis → build → deploy. Jika ada error, pipeline gagal dan developer dapat notifikasi.

---

## Cara Pengimplementasian

### 1. Struktur Folder Workflow
```
.github/
└── workflows/
    └── deploy.yml
```

### 2. Workflow Lengkap: Test → Build Docker → Deploy
```yaml
name: Deploy to Production

# Trigger: push ke branch main
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  # ===== JOB 1: TEST =====
  test:
    name: Run Tests
    runs-on: ubuntu-latest

    services:
      postgres:
        image: postgres:16-alpine
        env:
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
          POSTGRES_DB: testdb
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run lint
        run: npm run lint

      - name: Run tests
        run: npm test
        env:
          DATABASE_URL: postgresql://test:test@localhost:5432/testdb

  # ===== JOB 2: BUILD & PUSH DOCKER IMAGE =====
  build:
    name: Build & Push Docker Image
    needs: test            # hanya jalan jika test sukses
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build & Push Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            username/my-app:latest
            username/my-app:${{ github.sha }}

  # ===== JOB 3: DEPLOY KE SERVER =====
  deploy:
    name: Deploy to VPS
    needs: build           # hanya jalan jika build sukses
    runs-on: ubuntu-latest

    steps:
      - name: Deploy via SSH
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SERVER_SSH_KEY }}
          script: |
            cd /var/www/my-app
            docker-compose pull
            docker-compose up -d --force-recreate
            docker system prune -f
```

### 3. Secrets yang Perlu Diset di GitHub
| Secret | Isi |
|---|---|
| `DOCKER_USERNAME` | Username Docker Hub |
| `DOCKER_PASSWORD` | Token / password Docker Hub |
| `SERVER_HOST` | IP address VPS |
| `SERVER_USER` | Username SSH (biasanya `deploy`) |
| `SERVER_SSH_KEY` | Private key SSH (isi penuh) |

Set di: repo → Settings → Secrets and variables → Actions

### 4. Workflow Status Badge
Tambahkan di `README.md`:
```markdown
![CI/CD](https://github.com/username/repo/actions/workflows/deploy.yml/badge.svg)
```

---

## Analogi: Membangun Rumah (Pabrik Quality Control)

| CI/CD | Pabrik Quality Control |
|---|---|
| Commit / Push | Produk baru dari lini produksi |
| `on: push` | Sensor di pintu masuk pabrik |
| Job: Test | Meja inspeksi — ukur, timbang, uji kualitas |
| Job: Build | Bagian pengepakan — masukkan ke kotak (Docker) |
| Job: Deploy | Truk pengiriman ke toko (server) |
| `needs: test` | Barang yang gagal inspeksi dibuang, tidak dikirim |
| Secrets | Kunci gudang — hanya orang tertentu tahu |
| Failed pipeline | Alarm pabrik — "ada yang salah, berhenti!" |
| `actions/checkout` | Ambil produk dari rak ke meja inspeksi |

Bayangkan pabrik: setiap produk baru masuk lewat konveyor. Pertama, produk diperiksa kualitasnya di meja inspeksi (test). Jika lolos, masuk ke pengepakan (build Docker). Terakhir, dikirim ke toko (deploy ke server). Semua otomatis — tak perlu orang ngecek satu-satu. Dan jika ada produk cacat, pabrik berhenti dan alarm bunyi.

---

## Dipakai Untuk Apa

- **Auto test** — setiap pull request di-test, jadi tahu sebelum merge
- **Auto deploy** — push ke main langsung deploy ke production
- **Release management** — build version, upload ke registry
- **Lint & format check** — pastikan kode konsisten
- **Security scan** — cek dependency vulnerability otomatis
- **Multi-environment** — deploy ke staging dulu, kalau ok baru production

---

## Kesalahan Umum

| Kesalahan | Akibat |
|---|---|
| Lupa cache `npm ci` | Install dependency tiap kali — pipeline lambat |
| Secrets hardcode di workflow | Bocor ke repo — **gunakan GitHub Secrets** |
| Tidak pakai `needs` | Deploy jalan meski test gagal |
| SSH key tanpa passphrase | Aman, tapi pastikan key terenkripsi |
| Trigger di semua branch | Setiap push ke branch lain juga deploy — kacau |
| Lupa `docker system prune` | Disk server penuh karena image lama |

---

## Hubungan dengan Materi Sebelumnya/Selanjutnya

- **Materi 111 (Docker Compose)**: Pipeline build image dan deploy dengan compose
- **Materi 113 (Cloud Deployment)**: Hasil deploy pipeline → server di cloud

---

## Soal Latihan

### Soal 1 (Mudah)
Jelaskan perbedaan kata kunci berikut di GitHub Actions:
1. `on`
2. `jobs`
3. `steps`
4. `needs`
5. `uses`
6. `run`

**Jawaban**:
1. `on` — kapan workflow jalan (trigger)
2. `jobs` — kumpulan tugas (jalan parallel default)
3. `steps` — langkah-langkah dalam satu job (berurutan)
4. `needs` — job ini menunggu job lain selesai dulu
5. `uses` — pakai action dari marketplace (bisa buatan sendiri atau orang)
6. `run` — jalankan perintah shell langsung

### Soal 2 (Sedang)
Buat workflow GitHub Actions dengan 2 job:
- **Job 1 (lint)**: Checkout, setup Node 18, `npm ci`, `npm run lint`. Pakai cache.
- **Job 2 (deploy-staging)**: Hanya jalan jika lint sukses. Jalankan script via SSH ke staging server.

**Jawaban**:
```yaml
name: Staging Pipeline

on:
  push:
    branches: [develop]

jobs:
  lint:
    name: Lint Code
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 18
          cache: 'npm'
      - run: npm ci
      - run: npm run lint

  deploy-staging:
    name: Deploy to Staging
    needs: lint
    runs-on: ubuntu-latest
    steps:
      - name: SSH and deploy
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.STAGING_HOST }}
          username: ${{ secrets.STAGING_USER }}
          key: ${{ secrets.STAGING_KEY }}
          script: |
            cd /var/www/staging
            git pull origin develop
            npm ci --production
            npm run build
            pm2 restart my-app-staging
```

### Soal 3 (Tantangan)
Buat workflow **Blue-Green Deployment** dengan GitHub Actions:
- 2 job: test, deploy
- Di job deploy: tarik image Docker terbaru, jalan di port alternatif (blue = 3001, green = 3002), test health endpoint, jika ok, balik Nginx ke port baru. Jika tidak ok, rollback ke port lama.
- (Tulis logic dalam bentuk script bash di step deploy)

**Jawaban**:
```yaml
name: Blue-Green Deploy

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 18
      - run: npm ci
      - run: npm test

  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Deploy Blue-Green
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SERVER_SSH_KEY }}
          script: |
            # Cek container yang sedang aktif
            CURRENT=$(docker ps --format '{{.Names}}' | grep my-app | head -1)
            
            if [[ "$CURRENT" == "my-app-blue" ]]; then
              NEW="green"
              NEW_PORT=3002
              OLD="blue"
              OLD_PORT=3001
            else
              NEW="blue"
              NEW_PORT=3001
              OLD="green"
              OLD_PORT=3002
            fi
            
            echo "Deploy ke $NEW (port $NEW_PORT)..."
            
            # Pull image terbaru
            docker pull username/my-app:latest
            
            # Jalanin container baru
            docker run -d \
              --name my-app-$NEW \
              -p $NEW_PORT:3000 \
              username/my-app:latest
            
            # Test health endpoint
            sleep 5
            STATUS=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:$NEW_PORT/health)
            
            if [[ "$STATUS" != "200" ]]; then
              echo "Health check gagal! Rollback..."
              docker stop my-app-$NEW
              docker rm my-app-$NEW
              exit 1
            fi
            
            # Update Nginx ke port baru
            sed -i "s/proxy_pass http:\/\/localhost:$OLD_PORT/proxy_pass http:\/\/localhost:$NEW_PORT/" /etc/nginx/sites-enabled/my-app
            nginx -s reload
            
            # Matikan container lama
            docker stop my-app-$OLD
            docker rm my-app-$OLD
            
            echo "Deploy $NEW sukses! Container $OLD dimatikan."
```
