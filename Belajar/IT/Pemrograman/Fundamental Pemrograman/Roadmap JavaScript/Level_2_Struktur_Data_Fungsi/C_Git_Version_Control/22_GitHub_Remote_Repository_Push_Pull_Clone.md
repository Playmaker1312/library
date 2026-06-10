# 22 — GitHub — Remote Repository, Push, Pull, Clone

## 1. Penjelasan

Git di lokal komputer itu seperti buku catatan di rumah kamu. GitHub adalah **perpustakaan online** tempat kamu nyimpan salinan buku itu. Orang lain bisa pinjam, baca, bahkan bantu nulis — asal kamu izinkan.

Konsep utama:
- **Remote repo**: Repositori di server (GitHub, GitLab, Bitbucket).
- **Local repo**: Repositori di komputer kamu.
- **Push**: Kirim commit dari lokal ke remote.
- **Pull**: Ambil commit dari remote ke lokal.
- **Clone**: Copy remote repo ke lokal (pertama kali).
- **README.md**: Halaman depan repo.
- **.gitignore**: Daftar file yang diabaikan Git (misal `node_modules/`).

## 2. Fungsi

| Fitur | Fungsi |
|-------|--------|
| `git push` | Upload commit lokal ke GitHub |
| `git pull` | Download & merge perubahan dari GitHub |
| `git clone <url>` | Copy repo dari GitHub ke lokal |
| `README.md` | Dokumentasi project |
| `.gitignore` | Abaikan file sampah (cache, env, dll) |

## 3. Code / Perintah

### Setup awal

```bash
# Set identitas (sekali aja)
git config --global user.name "Nama Kamu"
git config --global user.email "email@example.com"
```

### Buat repo di GitHub → Push project

```bash
# 1. Buat repo baru di GitHub (jangan centang README)
# 2. Di terminal lokal:
git remote add origin https://github.com/username/rumah-impian.git
git branch -M main
git push -u origin main
```

### Clone repo orang lain

```bash
git clone https://github.com/username/project-keren.git
cd project-keren
```

### Pull update terbaru

```bash
git pull origin main
```

### .gitignore

```bash
# File .gitignore
node_modules/
.env
*.log
.DS_Store
dist/
```

### Micro-exercise: Push project Level 1 ke GitHub

```bash
cd project-level-1
git remote add origin https://github.com/username/level-1.git
git branch -M main
git push -u origin main
# Buka GitHub — project-mu sudah online!
```

## 4. Analogi Rumah

### Tabel Analogi

| Konsep Git | Analogi Rumah |
|------------|---------------|
| Local repo | Buku catatan renovasi di rumah kamu |
| Remote repo (GitHub) | Perpustakaan online — salinan buku disimpan di cloud |
| Push | Fotokopi buku catatan, kirim ke perpustakaan |
| Pull | Ambil catatan renovasi tetangga dari perpustakaan |
| Clone | Pinjam full buku orang lain, copy semua halaman |
| README.md | Papan informasi di depan rumah |
| .gitignore | Kotak khusus di gudang — isinya sampah yang nggak usah difoto |

### Narasi

Kamu punya buku catatan renovasi rumah di meja kerja (local repo). Biar aman, kamu fotokopi dan titip di perpustakaan kota (GitHub). Besok ada tetangga mau bantu renovasi — dia clone buku dari perpustakaan, bikin catatan sendiri, lalu push perubahannya. Kamu pull catatannya — sekarang rumahmu lebih bagus berkat bantuan tetangga.

## 5. Use Case

- Backup project ke cloud — harddisk rusak? Tenang, masih ada di GitHub.
- Kolaborasi dengan tim — masing-masing push, pull, selesai.
- Portofolio — GitHub jadi tempat展示 project ke recruiter.
- Open source — orang dari seluruh dunia bisa clone dan kontribusi.

## 6. Kesalahan Umum

- **Push ke repo yang belum di-set remote**: Ditolak. Jalankan `git remote add origin <url>` dulu.
- **Lupa `.gitignore`**: `node_modules/` ikut terpush — repo jadi berat.
- **Pull padahal ada perubahan lokal belum di-commit**: Konflik. Selalu commit atau stash dulu.
- **Push langsung ke `main` tanpa branch**: Di tim kecil ok, di tim besar — pakai branch (Materi 23).

## 7. Benang Merah

```
Materi 21: Git lokal — init, add, commit
  ↓ "Gimana backup ke cloud biar aman?"
Materi 22: GitHub — remote, push, pull, clone
  ↓ "Udah bisa push ke cloud. Tapi kerja tim gimana caranya?"
Materi 23: Branching — feature branch workflow
```

## 8. Soal + Jawaban

**Soal 1:** Bedakan `git push` dan `git pull` dengan analogi perpustakaan.
**Jawaban:** Push = fotokopi buku catatanmu lalu kirim ke perpustakaan. Pull = ambil buku pinjaman dari perpustakaan dan update catatanmu.

**Soal 2:** File apa yang digunakan untuk memberi tahu Git agar mengabaikan folder `node_modules/`?
**Jawaban:** `.gitignore` — isi dengan `node_modules/`.

**Soal 3:** Perintah apa untuk meng-copy repo dari GitHub ke lokal pertama kali?
**Jawaban:** `git clone <url-repo>`.
