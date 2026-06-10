# 146: GitOps — Flux & ArgoCD

## 1) Penjelasan

GitOps adalah praktik di mana **Git repository menjadi single source of truth** untuk infrastruktur dan aplikasi. Setiap perubahan dilakukan via Pull Request ke Git, lalu operator (ArgoCD/Flux) otomatis sinkronisasi cluster dengan state di Git. Pull-based deployment: operator di dalam cluster menarik perubahan dari Git.

## 2) Fungsi

- **Flux vs ArgoCD**: Flux lebih simpel dan ringan, ArgoCD punya UI Web dan fitur lebih lengkap.
- **Pull-based deployment**: Cluster menarik dari Git (bukan push dari CI ke cluster).
- **Auto-remediation**: Jika ada perubahan manual di cluster (drift), ArgoCD/Flux mengembalikan ke state Git.
- **Sync policy**: Manual, auto, atau automated with prune (hapus resource yang tidak ada di Git).
- **Multi-cluster**: Satu Git repo bisa manage banyak cluster.

## 3) Code/Config

```yaml
# argo-cd-application.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: express-api
  namespace: argocd
spec:
  project: default

  source:
    repoURL: 'https://github.com/myorg/myapp-gitops.git'
    targetRevision: main
    path: overlays/production

  destination:
    server: 'https://kubernetes.default.svc'
    namespace: production

  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

```yaml
# Structure Git repo
myapp-gitops/
├── base/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── kustomization.yaml
├── overlays/
│   ├── dev/
│   │   ├── kustomization.yaml
│   │   └── patch-replicas.yaml
│   ├── staging/
│   │   ├── kustomization.yaml
│   │   └── patch-ingress.yaml
│   └── production/
│       ├── kustomization.yaml
│       ├── patch-resources.yaml
│       └── patch-replicas.yaml
```

```bash
# Deploy ArgoCD ke cluster
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## 4) Analogi Rumah (Tabel)

| Konsep GitOps | Analogi Rumah |
|---------------|---------------|
| **Git** | SOP digital — semua prosedur tertulis |
| **ArgoCD/Flux** | Supervisor yang selalu cek: "apakah kerjaan sesuai SOP?" |
| **Drift** | Tukang bangun tembok secara manual tanpa SOP |
| **Auto-remediation** | Supervisor bongkar tembok yang tidak sesuai SOP |
| **Pull-based** | Supervisor ambil SOP dari buku (tarik), bukan dikasih tahu orang |
| **Sync Policy** | Jadwal: auto-sync tiap 3 menit atau manual |
| **Multi-cluster** | SOP yang sama diterapkan di perumahan A, B, C |

## 5) Use Case

- **Audit trail**: Semua perubahan ada riwayatnya di Git — siapa, kapan, apa yang diubah.
- **Disaster recovery**: Cluster hancur? Tinggal `kubectl apply` ArgoCD lagi, dia deploy ulang semua dari Git.
- **Compliance**: Tidak ada akses langsung ke production — semua lewat PR + approval.

## 6) Kesalahan Umum

- **Auto-sync tanpa prune**: Resource lama menumpuk karena tidak dihapus.
- **Credential di Git**: Push secret ke repo publik = bocor.
- **Merge tanpa review**: Langsung merge ke main tanpa PR review bisa deploy konfigurasi error ke production.

## 7) Benang Merah

Dari **Chaos Engineering** (145) yang menguji resilience. GitOps memastikan state cluster selalu sesuai yang diinginkan (auditable). Selanjutnya: **DevSecOps** (147) — security di setiap tahap pipeline.

## 8) Soal

**1. Apa perbedaan utama GitOps dengan deployment tradisional (push)?**
GitOps: operator di cluster tarik dari Git (pull). Tradisional: CI push ke cluster via kubectl (push).

**2. Apa maksud auto-remediation di ArgoCD?**
Jika ada perubahan manual di cluster (misal: edit deployment via kubectl), ArgoCD otomatis kembalikan ke state di Git.

**3. Kenapa GitOps bagus untuk disaster recovery?**
Karena semua konfigurasi ada di Git. Cluster baru tinggal deploy ArgoCD, dia akan recreate semua resource dari Git.
