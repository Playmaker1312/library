# Setup Environment Lengkap — Node.js LTS, pnpm, NestJS CLI, VS Code & Docker

## Penjelasan

Sebelum memulai membangun **gedung NestJS**, kita harus menyiapkan lahan, fondasi, dan peralatan kerja terlebih dahulu. Sama seperti kontraktor yang butuh semen, cetakan bata, dan alat ukur, kita sebagai developer butuh **runtime environment**, **package manager**, **CLI tools**, **code editor**, dan **container runtime**.

Materi ini adalah langkah **nol** yang mutlak diperlukan. Tanpa environment yang benar, kode yang kita tulis tidak bisa dijalankan atau diuji.

## Fungsi

Setiap tool dalam setup ini punya peran spesifik:

| Tool | Fungsi |
|------|--------|
| **Node.js LTS** | Runtime JavaScript di sisi server — "mesin diesel" yang menjalankan kode NestJS |
| **pnpm** | Package manager cepat dan hemat disk — "gudang material" yang menyimpan dependensi |
| **NestJS CLI** | Scaffolder dan helper untuk generate module, controller, service — "mandor" yang mempercepat pekerjaan |
| **VS Code** | Code editor dengan ekosistem extension — "meja kerja" utama |
| **Docker Desktop** | Container engine untuk database dan service lain — "ruang server prefabrikasi" |

## Cara Pengimplementasian

### 1. Install Node.js LTS

Kunjungi [https://nodejs.org](https://nodejs.org) dan unduh versi **LTS** (bukan Current).

Atau via terminal (Windows — pakai winget):

```bash
winget install OpenJS.NodeJS.LTS
```

### 2. Install pnpm

Setelah Node.js terpasang, instal pnpm secara global:

```bash
npm install -g pnpm
```

Verifikasi:

```bash
pnpm --version
```

### 3. Install NestJS CLI

```bash
npm install -g @nestjs/cli
```

Verifikasi:

```bash
nest --version
```

### 4. Install VS Code + Extensions

Unduh dari [https://code.visualstudio.com](https://code.visualstudio.com).

Extensions wajib:

- **Prettier** — formatter otomatis
- **ESLint** — linter kode
- **REST Client** — testing API langsung dari editor
- **GitLens** — insight git dalam editor

Install via terminal:

```bash
code --install-extension esbenp.prettier-vscode
code --install-extension dbaeumer.vscode-eslint
code --install-extension humao.rest-client
code --install-extension eamodio.gitlens
```

### 5. Install Docker Desktop

Unduh dari [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop).

Verifikasi:

```bash
docker --version
docker ps
```

### 6. Membuat Project NestJS Pertama

```bash
nest new my-first-project
cd my-first-project
pnpm run start:dev
```

### Perintah Dasar NestJS CLI

```bash
nest --help              # daftar semua perintah
nest new <nama-project>  # buat project baru
nest generate module <nama>
nest generate controller <nama>
nest generate service <nama>
nest g res <nama>        # generate resource lengkap (CRUD)
```

## Analogi

Menyiapkan environment seperti menyiapkan **lahan dan gudang alat** sebelum membangun gedung bertingkat:

- **Node.js** = mesin diesel utama di lokasi proyek
- **pnpm** = gudang material yang terorganisir rapi
- **NestJS CLI** = mandor yang tahu cetak biru dan bisa memerintahkan pekerja
- **VS Code** = meja kerja + kotak alat
- **Docker** = ruang server prefab yang tinggal pasang

Tanpa ini semua, kita cuma punya gambar gedung di kertas tanpa cara mewujudkannya.

## Dipakai untuk Apa

Setup ini dipakai di **setiap project NestJS** tanpa terkecuali. Baik proyek kecil API blog maupun sistem backend enterprise berskala besar semuanya butuh fondasi yang sama.

## Kesalahan Umum yang Sering Terjadi

1. **Salah versi Node.js** — Menginstall Node.js versi Current (bukan LTS) yang tidak stabil untuk production.
2. **Lupa install pnpm** — Menggunakan npm langsung yang lebih lambat dan boros disk.
3. **Tidak mengaktifkan Docker** — Menjalankan `docker ps` gagal karena Docker Desktop belum running.
4. **Global package tidak dikenali** — Path npm global tidak terdaftar di environment variable. Solusi: tambahkan `%AppData%\npm` ke PATH.
5. **Extension VS Code tidak terinstall** — Melewati extension penting seperti ESLint atau Prettier sehingga kode menjadi tidak konsisten.

## Soal Latihan Beserta Jawaban

### Soal 1
Jalankan perintah `nest --version` dan `docker ps`. Catat output dari kedua perintah tersebut.

**Jawaban:**
```bash
nest --version
# Output: 10.x.x (tergantung versi terbaru)

docker ps
# Output: CONTAINER ID   IMAGE   COMMAND   CREATED   STATUS   PORTS   NAMES
# (jika tidak ada container running, tetap muncul header tabel — berarti Docker aktif)
```

### Soal 2
Buat project NestJS baru bernama `belajar-nestjs` menggunakan NestJS CLI, lalu jalankan dalam mode development.

**Jawaban:**
```bash
nest new belajar-nestjs
cd belajar-nestjs
pnpm run start:dev
# Server akan berjalan di http://localhost:3000
```

### Soal 3
Apa perbedaan antara Node.js LTS dan Current? Mengapa kita memilih LTS?

**Jawaban:** LTS (Long Term Support) mendapat patch keamanan dan maintenance selama 30 bulan. Current hanya mendapat 6 bulan. Untuk proyek production dan belajar yang stabil, LTS adalah pilihan tepat — seperti memilih fondasi beton yang sudah teruji dibanding yang masih eksperimental.
