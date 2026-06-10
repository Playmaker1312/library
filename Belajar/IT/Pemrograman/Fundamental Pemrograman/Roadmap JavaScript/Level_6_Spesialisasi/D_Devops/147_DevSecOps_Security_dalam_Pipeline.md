# 147: DevSecOps — Security dalam Pipeline

## 1) Penjelasan

DevSecOps mengintegrasikan **security di setiap tahap DevOps pipeline** — bukan menempel security di akhir (shift-left security). Mulai dari kode (SAST), dependencies (Snyk), container (Trivy), sampai runtime (DAST). Tujuannya: menemukan celah keamanan lebih awal, lebih murah diperbaiki.

## 2) Fungsi

- **SAST (Static Analysis)**: Analisa source code tanpa menjalankan — SonarQube, Semgrep.
- **DAST (Dynamic Analysis)**: Tes aplikasi yang sudah berjalan — OWASP ZAP, Burp Suite.
- **Dependency Scanning**: Cek library/vendor untuk CVE — Snyk, Dependabot.
- **Container Scanning**: Cek image untuk vulnerability — Trivy, Clair.
- **Supply Chain Security**: SLSA (level kepercayaan build), SBOM (bill of materials).
- **Policy as Code**: OPA/Kyverno — enforce security rules di K8s.

## 3) Code/Config

```yaml
# .github/workflows/security-scan.yml
name: Security Scan Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  sast:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run SonarQube Scan
        uses: sonarsource/sonarcloud-github-action@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}

  dependency-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Snyk Scan (Dependencies)
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
        with:
          args: --severity-threshold=high

  container-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Build Docker Image
        run: docker build -t myapp:${{ github.sha }} .

      - name: Trivy Scan Image
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: 'myapp:${{ github.sha }}'
          format: 'sarif'
          exit-code: '1'
          severity: 'CRITICAL,HIGH'
```

```yaml
# .snyk — Policy file
patch:
  'SNYK-JS-LODASH-567746':
    - 'api > lodash@4.17.15':
        patched: '2026-01-15'
```

## 4) Analogi Rumah (Tabel)

| Tahap Keamanan | Analogi Rumah |
|----------------|---------------|
| **SAST (SonarQube)** | Cek blueprint sebelum bangun — cari desain yang salah |
| **DAST (OWASP ZAP)** | Tes gedung setelah jadi — coba masuk lewat jendela |
| **Dependency Scan (Snyk)** | Cek kualitas material dari supplier — jangan pakai kayu rapuh |
| **Container Scan (Trivy)** | Cek isi peti kemas dari pabrik — jangan ada barang ilegal |
| **SLSA** | Sertifikat: "gedung ini dibangun sesuai standar" |
| **SBOM** | Daftar semua material yang dipakai: jenis baut, merek semen |
| **Shift-Left Security** | Temukan masalah di tahap blueprint, bukan setelah rumah jadi |

## 5) Use Case

- **Compliance (PCI-DSS, HIPAA)**: Bukti bahwa setiap perubahan melalui security scan.
- **Prevent supply chain attack**: Cegah library berbahaya (seperti event-stream, ua-parser-js).
- **Block vulnerable image**: Jika Trivy menemukan CVE critical di image → build gagal, image tidak di-push.

## 6) Kesalahan Umum

- **Security hanya di akhir**: Scan setelah deploy ke prod — sudah terlambat.
- **False positif diabaikan**: Tidak tuning rules — scan jadi noise, tim jadi abai.
- **Secret di CI logs**: Print environment variable di GitHub Actions — token bocor di log publik.

## 7) Benang Merah

Ini adalah **penutup jalur DevOps Level 6**. Dari K8s (140) → Helm (141) → Terraform (142) → Ansible (143) → Observability (144) → Chaos Engineering (145) → GitOps (146) → DevSecOps (147): seluruh siklus DevOps dari provisioning, deployment, monitoring, resilience, hingga security.

## 8) Soal

**1. Apa maksud "shift-left security"?**
Menggeser aktivitas security ke tahap awal pipeline (kode, build) bukan menunggu sampai production.

**2. Bedanya SAST dan DAST?**
SAST analisa source code (statis). DAST tes aplikasi yang sudah jalan (dinamis).

**3. Kenapa container scanning (Trivy) penting di pipeline CI/CD?**
Mencegah image dengan CVE critical masuk ke registry/deployment. Kalau ketahuan di production, sudah terlanjur terekspos.
