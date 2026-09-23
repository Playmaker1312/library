# 92. CI/CD Pipeline Lengkap — GitHub Actions ke VPS

## Penjelasan

Setelah aplikasi NestJS siap dijalankan di production via Docker (Poin 90-91), langkah selanjutnya adalah mengotomatiskan proses build, test, dan deploy. CI/CD Pipeline adalah automation yang menjalankan testing dan deployment setiap kali ada perubahan kode. GitHub Actions adalah CI/CD bawaan GitHub yang gratis dan terintegrasi langsung dengan repository.

Pipeline lengkap terdiri dari: **push kode** → **lint & test** → **build Docker image** → **push ke registry** → **deploy ke VPS**.

## Fungsi

- Mengotomatiskan proses testing agar tidak ada kode rusak yang masuk ke production
- Membangun Docker image secara konsisten di environment CI
- Menyimpan image di container registry (GitHub Container Registry / Docker Hub)
- Melakukan deployment ke VPS tanpa downtime
- Menyediakan rollback strategy jika terjadi masalah

## Cara Pengimplementasian / Code

```yaml
# .github/workflows/deploy.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16-alpine
        env:
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
          POSTGRES_DB: test
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'pnpm'

      - run: pnpm install --frozen-lockfile
      - run: pnpm lint
      - run: pnpm test:cov

  build-and-push:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - uses: actions/checkout@v4

      - name: Log in to GitHub Container Registry
        uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:latest
            ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}

  deploy:
    needs: build-and-push
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to VPS via SSH
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USER }}
          key: ${{ secrets.VPS_SSH_KEY }}
          script: |
            cd /opt/nestjs-app
            docker compose pull app
            docker compose up -d --no-deps --build app
            docker image prune -f
```

```yaml
# .github/workflows/rollback.yml
name: Rollback

on:
  workflow_dispatch:
    inputs:
      tag:
        description: 'Image tag to rollback to'
        required: true

jobs:
  rollback:
    runs-on: ubuntu-latest
    steps:
      - name: Rollback VPS
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USER }}
          key: ${{ secrets.VPS_SSH_KEY }}
          script: |
            cd /opt/nestjs-app
            sed -i "s|image:.*|image: ghcr.io/myorg/myapp:${{ github.event.inputs.tag }}|" docker-compose.yml
            docker compose up -d --no-deps app
```

```bash
# Script untuk zero-downtime deployment manual (jika perlu)
#!/bin/bash
set -e

echo "Pulling latest image..."
docker compose pull app

echo "Starting new container (zero-downtime)..."
docker compose up -d --no-deps --scale app=2 --no-recreate app

echo "Waiting for health check..."
sleep 15

echo "Removing old container..."
docker compose up -d --no-deps --scale app=1 --no-recreate app

echo "Deployment successful!"
```

## Analogi

Membangun CI/CD pipeline seperti **memiliki kontraktor otomatis untuk gedung pencakar langit**:

| Tahap | Analogi |
|-------|---------|
| Push kode | Arsitek mengirim revisi desain |
| Test | Tim QA memeriksa setiap perubahan |
| Build image | Pabrik prefabrikasi komponen |
| Push ke registry | Gudang penyimpanan komponen |
| Deploy ke VPS | Derek memasang komponen ke gedung |
| Rollback | Menurunkan komponen yang rusak |
| Zero-downtime | Mengganti panel tanpa menghentikan operasi |

Jika ada komponen cacat, pipeline berhenti sebelum komponen itu sampai ke gedung. Jika komponen sudah terpasang dan ternyata bermasalah, rollback mengembalikan ke versi sebelumnya yang stabil.

## Dipakai untuk Apa

- **Automated testing**: setiap PR dan push ke main otomatis di-test
- **Continuous deployment**: kode yang sudah lulus test otomatis di-deploy
- **Consistency**: build di CI menjamin environment yang sama setiap kali
- **Audit trail**: setiap deployment tercatat dengan tag image yang spesifik
- **Rollback**: kembali ke versi stabil dalam hitungan detik
- **Team workflow**: multiple developer bisa deploy tanpa konflik

## Kesalahan Umum yang Sering Terjadi

| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Lupa set secrets di GitHub | Pipeline gagal di step deploy | Setup VPS_HOST, VPS_USER, VPS_SSH_KEY di Settings → Secrets |
| Tidak pakai `--no-deps` | Semua service restart termasuk DB | Selalu gunakan `--no-deps` saat update service tertentu |
| Image terlalu besar | Pipeline lambat, bandwidth mahal | Optimasi Dockerfile multi-stage |
| Tidak ada health check | Container baru belum siap tapi traffic sudah diarahkan | Tambahkan health check di Docker Compose |
| Rollback tidak pernah di-test | Pas emergency, rollback malah error | Test rollback secara berkala di staging |
| Hardcode credential di workflow | Security breach | Selalu gunakan GitHub Secrets |

## Soal Latihan

### Soal 1:
Buat GitHub Actions workflow yang:
- Trigger pada push ke branch `main` dan `staging`
- Menjalankan lint, unit test, dan integration test
- Hanya build dan deploy jika branch = `main`
- Menggunakan Docker BuildKit untuk build yang lebih cepat

<details>
<summary>Jawaban</summary>

```yaml
name: CI/CD

on:
  push:
    branches: [main, staging]

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16-alpine
        env:
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
          POSTGRES_DB: test
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
      redis:
        image: redis:7-alpine
        ports:
          - 6379:6379
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'pnpm'
      - run: pnpm install --frozen-lockfile
      - run: pnpm lint
      - run: pnpm test:cov
        env:
          DATABASE_URL: postgresql://test:test@localhost:5432/test
          REDIS_URL: redis://localhost:6379

  deploy:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    env:
      DOCKER_BUILDKIT: 1
    steps:
      - uses: actions/checkout@v4
      - name: Build & Deploy
        run: |
          echo "Building with BuildKit..."
          docker build -t app:${{ github.sha }} .
          echo "Deploying to production..."
          # deploy commands here
```
</details>

### Soal 2:
Jelaskan apa yang terjadi jika deployment baru gagal health check. Bagaimana strategi rollback yang aman?

<details>
<summary>Jawaban</summary>

Jika container baru gagal health check, Docker Compose akan menandainya sebagai "unhealthy" dan tidak mengarahkan traffic ke container tersebut. Strategi rollback yang aman:

1. **Jangan hapus image lama**: simpan minimal 3 versi terakhir di registry
2. **Deploy dengan blue-green**: jalankan container baru bersamaan dengan yang lama
3. **Health check gateway**: gunakan Nginx health check untuk memastikan traffic hanya ke container sehat
4. **Rollback otomatis**: dalam workflow, jika health check gagal dalam 30 detik, secara otomatis restart container lama
5. **Manual rollback via workflow_dispatch**: trigger workflow rollback dengan input tag versi

```yaml
# Dalam docker-compose.yml
healthcheck:
  test: ["CMD", "wget", "--no-verbose", "--tries=1", "--spider", "http://localhost:3000/health"]
  interval: 10s
  timeout: 5s
  retries: 3
  start_period: 15s
```
</details>
