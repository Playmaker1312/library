# 142: Terraform — Infrastructure as Code

## 1) Penjelasan

Terraform adalah alat **Infrastructure as Code (IaC)** dari HashiCorp. Dengan Terraform, infrastruktur (server, network, DNS, firewall) didefinisikan dalam file konfigurasi (declarative), bukan diklik manual di dashboard. Terraform bersifat **provider-agnostic** — bisa untuk AWS, DigitalOcean, GCP, Azure, dll.

## 2) Fungsi

- **Declarative vs Imperative**: Terraform declarative — kita bilang "ingin 3 server", bukan langkah demi langkah.
- **State Management**: Terraform menyimpan state file yang berisi snapshot infrastruktur saat ini.
- **Plan → Apply**: `terraform plan` untuk preview perubahan, `terraform apply` untuk eksekusi.
- **Destroy**: `terraform destroy` untuk hapus semua infrastruktur yang dibuat.

## 3) Code/Config

```hcl
# main.tf — DigitalOcean VPS + Firewall + DNS
terraform {
  required_providers {
    digitalocean = {
      source = "digitalocean/digitalocean"
      version = "~> 2.0"
    }
  }
}

provider "digitalocean" {
  token = var.do_token
}

# Buat VPS (Droplet)
resource "digitalocean_droplet" "app_server" {
  name     = "app-server"
  region   = "sgp1"
  size     = "s-2vcpu-4gb"
  image    = "ubuntu-22-04-x64"
  tags     = ["production", "app"]
}

# Firewall
resource "digitalocean_firewall" "web" {
  name = "web-firewall"

  droplet_ids = [digitalocean_droplet.app_server.id]

  inbound_rule {
    protocol         = "tcp"
    port_range       = "22"
    source_addresses = ["0.0.0.0/0"]
  }
  inbound_rule {
    protocol         = "tcp"
    port_range       = "80"
    source_addresses = ["0.0.0.0/0"]
  }
  inbound_rule {
    protocol         = "tcp"
    port_range       = "443"
    source_addresses = ["0.0.0.0/0"]
  }
}

# DNS Record
resource "digitalocean_record" "app_dns" {
  domain = "mydomain.com"
  type   = "A"
  name   = "app"
  value  = digitalocean_droplet.app_server.ipv4_address
}

# Output
output "server_ip" {
  value = digitalocean_droplet.app_server.ipv4_address
}
```

```hcl
# variables.tf
variable "do_token" {
  description = "DigitalOcean API Token"
  type        = string
  sensitive   = true
}
```

Perintah: `terraform init && terraform plan && terraform apply`

## 4) Analogi Rumah (Tabel)

| Konsep Terraform | Analogi Rumah |
|------------------|---------------|
| **Terraform** | Blueprint lengkap perumahan |
| **Resource** | Satu rumah, jalan, got, listrik |
| **State** | Foto terbaru kondisi perumahan saat ini |
| **Provider** | Kontraktor spesifik (PLN untuk listrik, PDAM untuk air) |
| **Plan** | Lihat blueprint sebelum bangun — "akan bangun 3 rumah" |
| **Apply** | Eksekusi pembangunan |
| **Destroy** | Hancurkan seluruh perumahan sampai tanah kosong |
| **Variable** | Pilihan warna cat, jenis atap |

## 5) Use Case

- Setup infrastruktur baru dari nol dengan konsisten
- Multi-environment (dev/staging/prod) dengan kode yang sama
- Disaster recovery — buat ulang infrastruktur di region lain dengan cepat

## 6) Kesalahan Umum

- **State file tidak di-backup**: Rusak/lost = Terraform tidak tahu infrastruktur apa yang sudah dibuat.
- **Hardcode credential di file**: Token/provider credentials seharusnya via environment variable.
- **Tidak pakai remote state**: State di lokal — anggota tim lain bisa konflik.

## 7) Benang Merah

Dari **Helm** (141) yang deploy ke K8s yang sudah ada. Terraform bikin K8s-nya dulu (atau VPS-nya). Selanjutnya: **Ansible** (143) untuk configuration management setelah server jadi.

## 8) Soal

**1. Apa perbedaan `terraform plan` dan `terraform apply`?**
Plan hanya preview perubahan tanpa eksekusi. Apply mengeksekusi perubahan yang sudah di-plan.

**2. Kenapa Terraform disebut declarative?**
Kita mendeklarasikan _hasil akhir_ yang diinginkan (3 server), bukan langkah-langkah untuk mencapainya.

**3. Apa risiko kehilangan file state?**
Terraform tidak bisa mapping antara konfigurasi dan resources yang sudah ada. Bisa menyebabkan duplikasi resource atau gagal manage infrastruktur.
