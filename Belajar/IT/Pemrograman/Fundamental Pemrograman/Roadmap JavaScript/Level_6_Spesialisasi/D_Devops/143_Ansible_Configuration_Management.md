# 143: Ansible — Configuration Management

## 1) Penjelasan

Jika Terraform untuk **provisioning** (bikin server), Ansible untuk **configuration management** (setup isi server). Ansible bersifat **agentless** — cukup SSH, tanpa install agent di server target. Idempotent: menjalankan playbook berkali-kali hasilnya sama.

## 2) Fungsi

- **Playbook**: File YAML berisi daftar task yang akan dijalankan.
- **Task**: Satu langkah (install package, copy file, start service).
- **Module**: Unit kerja Ansible (apt, copy, service, docker_container, dll).
- **Inventory**: Daftar server target (static file atau dynamic).
- **Idempotency**: Task hanya jalan jika state belum sesuai — tidak mengulang yang sudah benar.

## 3) Code/Config

```yaml
# playbook-setup-server.yaml
---
- name: Setup New Server
  hosts: webservers
  become: yes
  vars:
    app_repo: "https://github.com/myorg/myapp.git"
    app_dir: "/opt/myapp"

  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes
        cache_valid_time: 3600

    - name: Install Docker
      apt:
        name:
          - docker.io
          - docker-compose-v2
        state: present

    - name: Start and enable Docker
      service:
        name: docker
        state: started
        enabled: yes

    - name: Clone app repository
      git:
        repo: "{{ app_repo }}"
        dest: "{{ app_dir }}"
        version: main

    - name: Copy environment file
      copy:
        src: .env.production
        dest: "{{ app_dir }}/.env"
        mode: '0600'

    - name: Start containers with Docker Compose
      docker_compose:
        project_src: "{{ app_dir }}"
        state: present
        restarted: yes
```

```ini
# inventory.ini
[webservers]
app-01 ansible_host=192.168.1.10 ansible_user=root
app-02 ansible_host=192.168.1.11 ansible_user=root
```

Perintah: `ansible-playbook -i inventory.ini playbook-setup-server.yaml`

## 4) Analogi Rumah (Tabel)

| Konsep Ansible | Analogi Rumah |
|----------------|---------------|
| **Ansible** | Checklist pemeliharaan rumah bulanan |
| **Playbook** | Buku panduan: "cek atap, bersihkan got, ganti filter AC" |
| **Task** | Satu baris checklist: "bersihkan got" |
| **Module** | Alat spesifik: sikat got, obeng AC |
| **Idempotency** | Kalau got sudah bersih — skip, lanjut ke task berikutnya |
| **Inventory** | Daftar rumah yang perlu diperiksa |
| **Agentless** | Tukang datang langsung (tanpa pasang alat monitoring permanen) |

## 5) Use Case

- Setup server baru setelah provisioning Terraform
- Update konfigurasi di banyak server sekaligus
- Memastikan semua server memiliki konfigurasi yang sama (compliance)

## 6) Kesalahan Umum

- **Tidak idempotent**: Task `command: mkdir /foo` — error kalau sudah ada. Pakai module `file` saja.
- **Hardcode credential di playbook**: Gunakan Ansible Vault atau environment variable.
- **Lupa `become: yes`**: Task gagal karena tidak punya sudo permission padahal perlu root.

## 7) Benang Merah

Dari **Terraform** (142) yang bikin VPS. Ansible masuk ke VPS tersebut untuk install Docker, clone repo, dan start container. Selanjutnya: **Observability** (144) untuk monitoring.

## 8) Soal

**1. Apa beda Terraform dan Ansible?**
Terraform untuk provisioning infrastruktur (bikin server, network). Ansible untuk config management (setup isi server).

**2. Apa maksud Ansible bersifat agentless?**
Tidak perlu install agent/daemon di server target. Cukup SSH dan Python di server target.

**3. Kenapa idempotency penting dalam configuration management?**
Agar playbook aman dijalankan berulang kali. Kalau sudah sesuai state, tidak ada perubahan — tidak ada side effect.
