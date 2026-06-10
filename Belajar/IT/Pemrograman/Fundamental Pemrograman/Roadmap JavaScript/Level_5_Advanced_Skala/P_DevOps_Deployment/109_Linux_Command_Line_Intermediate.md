# 109. Linux Command Line — Intermediate

**Benang Merah**: Materi 108 membahas caching (Redis) — semua itu berjalan di **server**. Untuk mengelola server, kita harus bisa **berkomunikasi** dengannya via terminal. Linux CLI adalah alat untuk itu.

---

## Penjelasan

Linux CLI adalah antarmuka teks untuk mengendalikan server Linux. Setelah aplikasi siap (Materi 108 caching), kita perlu server untuk menjalankannya. Server Linux umumnya **tanpa GUI** — hanya terminal. CLI intermediate mencakup manajemen proses, permission, network tools, dan shell scripting.

```bash
# Cek proses yang berjalan
ps aux

# Cek penggunaan resource real-time
top

# Matikan proses berdasarkan PID
kill -9 1234

# Cek port yang terbuka
netstat -tulpn

# Download file dari URL
wget https://example.com/file.zip

# Test koneksi ke endpoint
curl -I https://api.example.com
```

---

## Fungsi

Memberi kemampuan **penuh** untuk mengelola server: menjalankan aplikasi, mengatur siapa yang bisa mengakses file, memonitor proses, debugging jaringan, dan mengotomatisasi tugas dengan script.

---

## Cara Pengimplementasian

### 1. Navigasi & File Management
```bash
pwd                     # posisi sekarang
ls -la                  # list file detail
cd /var/www             # pindah direktori
mkdir -p project/src    # buat folder nested
cp -r app/ backup/      # copy folder
rm -rf temp/            # hapus folder (hati-hati!)
```

### 2. Permission (chmod, chown)
```bash
# Ubah permission: owner=rwx, group=rx, other=r
chmod 755 script.sh

# Ubah owner file
chown deploy:deploy app.js

# Beri executable permission
chmod +x deploy.sh

# Arti angka chmod:
# 7 = rwx (baca+tulis+eksekusi)
# 6 = rw- (baca+tulis)
# 5 = r-x (baca+eksekusi)
# 4 = r-- (baca saja)
```

### 3. Process Management
```bash
# Lihat semua proses (user-friendly)
ps aux

# Cari proses tertentu
ps aux | grep node

# Matikan proses paksa (SIGKILL)
kill -9 PID

# Matikan graceful (SIGTERM) — kasih waktu cleanup
kill -15 PID

# Monitor real-time (q untuk keluar)
top

# Alternatif lebih keren
htop
```

### 4. Network Tools
```bash
# Test koneksi HTTP (dapat response header)
curl -I https://google.com

# Download file
wget https://nodejs.org/dist/v18.18.0/node-v18.18.0-linux-x64.tar.xz

# Lihat port yang listening
netstat -tulpn

# Alternatif modern
ss -tulpn

# Test port terbuka
nc -zv localhost 3000

# DNS lookup
nslookup google.com
```

### 5. Shell Scripting Dasar
```bash
#!/bin/bash
# Script: setup-server.sh
# Fungsi: Install Node.js, Git, clone project dari nol

set -e  # stop jika ada error

echo "=== Update system ==="
apt update && apt upgrade -y

echo "=== Install Node.js 18 ==="
curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
apt install -y nodejs

echo "=== Install Git ==="
apt install -y git

echo "=== Clone project ==="
git clone https://github.com/username/my-app.git /var/www/my-app

echo "=== Install dependencies ==="
cd /var/www/my-app
npm install

echo "=== Setup environment ==="
cp .env.example .env

echo "=== Start app with PM2 ==="
npm install -g pm2
pm2 start index.js --name my-app
pm2 save
pm2 startup

echo "=== Selesai! ==="
```

Jalankan:
```bash
chmod +x setup-server.sh
./setup-server.sh
```

---

## Analogi: Membangun Rumah (Panel Kontrol Rumah)

| Linux CLI | Panel Kontrol Rumah |
|---|---|
| `ps aux` | Lihat semua perangkat yang menyala (listrik) |
| `kill` | Matikan perangkat yang error |
| `top` | Monitor pemakaian listrik real-time |
| `chmod` | Set kunci pintu — siapa bisa masuk |
| `chown` | Ganti pemilik rumah |
| `curl` | Test apakah kran air mengalir (network) |
| `wget` | Ambil barang dari luar |
| `netstat` | Cek pipa mana saja yang terhubung |
| Shell script | Remote otomatis — satu tombol nyalakan semua |

Bayangkan Anda mengelola **rumah besar** (server). Ada panel kontrol: Anda bisa lihat perangkat listrik mana yang menyala (`ps`), matikan AC yang boros (`kill`), atur siapa punya kunci pintu mana (`chmod`), dan pastikan pipa air tidak bocor (`netstat`). Shell script adalah **tombol "nyalakan semua"** — sekali pencet, lampu, AC, TV, semua menyala sesuai urutan.

---

## Dipakai Untuk Apa

- **Setup server** dari awal (install dependencies, clone project)
- **Debugging** aplikasi yang crash di production
- **Monitoring** resource server (CPU, memory, disk)
- **Otomatisasi** backup, restart, deploy dengan script
- **Audit keamanan** — cek siapa punya akses ke file apa
- **Network troubleshooting** — aplikasi tidak bisa connect ke DB?

---

## Kesalahan Umum

| Kesalahan | Akibat |
|---|---|
| `chmod 777` sembarangan | Semua orang bisa akses file — risiko keamanan |
| `kill -9` tanpa coba `-15` dulu | Aplikasi tidak sempat cleanup, data corrupt |
| Lupa `set -e` di script | Script lanjut meski error — setup gagal diam-diam |
| Tidak pakai `sudo` padahal perlu | Permission denied, bingung sendiri |
| `rm -rf` di folder salah | Hapus file penting, bisa bikin server mati |
| Lupa `chmod +x` script | Script tidak bisa dijalankan |

---

## Hubungan dengan Materi Sebelumnya/Selanjutnya

- **Materi 108 (Caching → Redis)**: Redis berjalan di server — butuh CLI untuk install dan manage
- **Materi 110 (Docker)**: Semua perintah `docker ...` dijalankan lewat CLI
- **Materi 112 (CI/CD)**: Pipeline menjalankan script CLI otomatis

---

## Soal Latihan

### Soal 1 (Mudah)
Tulis perintah untuk:
1. Melihat semua proses Node.js yang berjalan
2. Mematikan proses Node.js dengan PID 4567 secara paksa
3. Mengecek port 3000 apakah sudah dipakai

**Jawaban**:
```bash
ps aux | grep node
kill -9 4567
netstat -tulpn | grep 3000
```

### Soal 2 (Sedang)
Buat shell script `backup.sh` yang:
1. Membuat folder backup dengan timestamp (format: `backup-YYYYMMDD`)
2. Mencopy folder `/var/www/my-app` ke folder backup
3. Menghapus backup yang lebih dari 7 hari

**Jawaban**:
```bash
#!/bin/bash
set -e

TIMESTAMP=$(date +%Y%m%d)
BACKUP_DIR="/backup/backup-$TIMESTAMP"

mkdir -p "$BACKUP_DIR"
cp -r /var/www/my-app "$BACKUP_DIR"
echo "Backup selesai: $BACKUP_DIR"

# Hapus backup lebih dari 7 hari
find /backup -type d -name "backup-*" -mtime +7 -exec rm -rf {} \;
echo "Backup lama dibersihkan."
```

### Soal 3 (Tantangan)
Tulis script `deploy.sh` untuk deploy otomatis:
1. `git pull` dari repository
2. `npm install` (hanya production)
3. Build aplikasi (`npm run build`)
4. Restart aplikasi dengan PM2
5. Rollback otomatis jika ada error di langkah 3 atau 4

**Jawaban**:
```bash
#!/bin/bash
set -e

APP_DIR="/var/www/my-app"
BRANCH="main"

cd "$APP_DIR"

echo "=== Deploy start: $(date) ==="

# Simpan commit lama untuk rollback
OLD_COMMIT=$(git rev-parse HEAD)

echo "Pull latest code..."
git pull origin "$BRANCH" || {
    echo "Git pull gagal!"
    exit 1
}

echo "Install production dependencies..."
npm install --production || {
    echo "npm install gagal, rollback..."
    git reset --hard "$OLD_COMMIT"
    exit 1
}

echo "Build aplikasi..."
npm run build || {
    echo "Build gagal, rollback..."
    git reset --hard "$OLD_COMMIT"
    npm install --production
    exit 1
}

echo "Restart aplikasi..."
pm2 restart my-app || {
    echo "Restart gagal, rollback..."
    git reset --hard "$OLD_COMMIT"
    npm install --production
    npm run build
    pm2 restart my-app
    exit 1
}

echo "=== Deploy sukses! ==="
```
