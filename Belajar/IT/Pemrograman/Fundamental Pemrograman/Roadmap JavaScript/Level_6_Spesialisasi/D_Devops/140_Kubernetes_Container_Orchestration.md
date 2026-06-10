# 140: Kubernetes — Container Orchestration

## 1) Penjelasan

Docker bagus untuk menjalankan container, tapi kalau aplikasi sudah kompleks (banyak container, perlu scaling, auto-restart, load balancing), Docker saja tidak cukup. Kubernetes (K8s) adalah **orchestrator container** yang mengatur deployment, scaling, dan management container secara otomatis.

## 2) Fungsi

- **Pod**: Unit terkecil di K8s — satu atau lebih container yang berbagi network/storage.
- **Service**: Abstraction untuk mengakses Pod (load balancer internal).
- **Deployment**: Mengatur replicas, rolling update, rollback.
- **Ingress**: Pintu masuk HTTP/HTTPS dari luar ke Service.
- **Control Plane**: Otak cluster (API Server, Scheduler, Controller Manager).
- **Worker Node**: Menjalankan Pod (kubelet, kube-proxy, container runtime).
- **kubectl**: CLI untuk berinteraksi dengan cluster.

## 3) Code/Config

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: express-api
spec:
  replicas: 3
  selector:
    matchLabels:
      app: express-api
  template:
    metadata:
      labels:
        app: express-api
    spec:
      containers:
      - name: api
        image: myapp/express-api:latest
        ports:
        - containerPort: 3000
---
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: express-service
spec:
  selector:
    app: express-api
  ports:
  - port: 80
    targetPort: 3000
  type: ClusterIP
---
# ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: express-ingress
spec:
  rules:
  - host: api.mydomain.com
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: express-service
            port:
              number: 80
```

Perintah: `kubectl apply -f deployment.yaml`

## 4) Analogi Rumah (Tabel)

| Konsep K8s | Analogi Rumah |
|------------|---------------|
| **Docker** | Kontraktor bikin satu ruangan |
| **Kubernetes** | Manajer gedung perkantoran |
| **Pod** | Satu security yang berjaga |
| **Deployment** | Aturan jumlah security per shift (3 orang) |
| **Service** | Pintu masuk utama gedung |
| **Ingress** | Resepsionis yang arahkan tamu ke lantai |
| **Control Plane** | Manajer pusat yang koordinasi semua |
| **Worker Node** | Lantai gedung tempat security bekerja |

## 5) Use Case

- Microservices dengan banyak service yang perlu scaling otomatis
- Aplikasi yang perlu zero-downtime deployment (rolling update)
- Sistem yang perlu dijalankan di multi-cloud atau hybrid cloud

## 6) Kesalahan Umum

- **Tidak set resource limits**: Pod bisa makan semua CPU/RAM node.
- **Lupa bikin Service**: Pod bisa mati kapan saja (IP berubah), tanpa Service akses jadi kacau.
- **Configmap/Secret tidak di-mount dengan benar**: Aplikasi crash karena env var hilang.

## 7) Benang Merah

Dari **Docker Compose** (Level 5) yang mengatur banyak container di satu host, naik ke **Kubernetes** yang mengatur container di banyak host dengan auto-healing dan auto-scaling. Selanjutnya: **Helm** (141) untuk package management K8s.

## 8) Soal

**1. Apa perbedaan Pod dan Deployment?**
Pod adalah instance tunggal container, Deployment mengatur jumlah dan siklus hidup Pod (replicas, rolling update).

**2. Kenapa butuh Service padahal sudah ada Pod?**
IP Pod berubah setiap restart. Service memberikan IP/stabil dan load balancing ke Pod-Pod di belakangnya.

**3. Apa yang terjadi jika `kubectl apply -f deployment.yaml` dijalankan?**
API Server menerima konfigurasi, Scheduler menempatkan Pod ke Worker Node, kubelet menjalankan container sesuai spec.
