# 141: Helm — Package Manager untuk Kubernetes

## 1) Penjelasan

Helm adalah **package manager untuk Kubernetes**. Dengan Helm, aplikasi K8s bisa di-package sebagai **Chart** (kumpulan template YAML + values) dan di-deploy dengan satu perintah. Helm memudahkan manajemen release, upgrade, rollback, dan konfigurasi multi-environment.

## 2) Fungsi

- **Chart**: Package K8s berisi template YAML dan file values.
- **Values**: File konfigurasi yang bisa diubah per environment (dev/staging/prod).
- **Release**: Instance spesifik dari Chart yang sudah di-deploy.
- **Repositori Helm**: Tempat hosting Chart (publik seperti ArtifactHub atau privat).
- **Custom Chart**: Buat Chart sendiri untuk aplikasi tim.

## 3) Code/Config

```yaml
# values.yaml — untuk aplikasi perpustakaan
# Environment: dev (default)
replicaCount: 1

image:
  repository: myrepo/library-api
  tag: latest
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 8080

ingress:
  enabled: false
  host: library.local

resources:
  requests:
    memory: "256Mi"
    cpu: "250m"
  limits:
    memory: "512Mi"
    cpu: "500m"

config:
  db_host: "localhost"
  db_name: "library_dev"
  log_level: "debug"
---
# values-staging.yaml
replicaCount: 2
image:
  tag: staging
ingress:
  enabled: true
  host: staging.library.com
config:
  db_host: "staging-db.internal"
  db_name: "library_staging"
---
# values-prod.yaml
replicaCount: 5
image:
  tag: v1.2.3
  pullPolicy: Always
ingress:
  enabled: true
  host: library.com
config:
  db_host: "prod-db.internal"
  db_name: "library_prod"
  log_level: "warn"
resources:
  requests:
    memory: "512Mi"
    cpu: "500m"
```

Perintah: `helm install library-release ./library-chart -f values-prod.yaml`

## 4) Analogi Rumah (Tabel)

| Konsep Helm | Analogi Rumah |
|-------------|---------------|
| **Helm Chart** | Paket IKEA — berisi furnitur + panduan rakit |
| **Template** | Panduan rakit universal (bisa dipakai berbagai model) |
| **Values** | Pilihan ukuran/warna furnitur sesuai ruangan |
| **Release** | Lemari yang sudah jadi dipasang di rumah |
| **helm upgrade** | Ganti pintu lemari tanpa bongkar total |
| **helm rollback** | Kembalikan ke model lemari sebelumnya |
| **Repo Helm** | Toko IKEA — tempat ambil paket |

## 5) Use Case

- Deploy aplikasi yang sama ke dev, staging, prod dengan nilai berbeda
- Distribusi aplikasi internal tim lewat repo Helm privat
- Mengelola dependency antar service (microservices) dalam satu Chart

## 6) Kesalahan Umum

- **Lupa override values**: Prod jadi pake `latest` karena lupa ganti tag image.
- **Chart terlalu besar**: Satu Chart untuk semua service bikin upgrade lambat.
- **Tidak versioning Chart**: Susah rollback karena tidak tahu versi mana yang stable.

## 7) Benang Merah

Dari **Kubernetes** (140) yang YAML-nya hardcoded. Helm membuat K8s YAML jadi reusable dan parametrik. Selanjutnya: **Terraform** (142) untuk provisioning infrastruktur di luar K8s.

## 8) Soal

**1. Apa beda Chart dan Release?**
Chart adalah template/paket, Release adalah hasil instalasi Chart di cluster.

**2. Bagaimana cara deploy aplikasi ke staging dengan Helm?**
`helm install app-release ./chart -f values-staging.yaml` atau `helm upgrade --install app-release ./chart -f values-staging.yaml`.

**3. Kenapa perlu versioning di Helm Chart?**
Agar bisa track perubahan konfigurasi, rollback ke versi sebelumnya (`helm rollback`), dan audit siapa mengubah apa.
