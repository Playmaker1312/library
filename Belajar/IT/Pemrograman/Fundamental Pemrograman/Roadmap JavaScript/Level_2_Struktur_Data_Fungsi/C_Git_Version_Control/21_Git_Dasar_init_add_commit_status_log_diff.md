# 21 — Git Dasar — init, add, commit, status, log, diff

## 1. Penjelasan

Ini adalah fondasi Git yang akan kamu pakai tiap hari. Alur dasarnya:

```
Working Directory → git add → Staging Area → git commit → Repository
```

- **git init**: Membuat repo Git baru (satu kali per project).
- **git add**: Memasukkan file ke staging area (keranjang belanja).
- **git commit**: Menyimpan snapshot staging ke riwayat.
- **git status**: Mengecek status file (ubah/tambah/hapus).
- **git log**: Melihat riwayat commit.
- **git diff**: Melihat perubahan sebelum di-add/commit.

## 2. Fungsi

| Perintah | Fungsi |
|----------|--------|
| `git init` | Membuat repository Git baru |
| `git add` | Pindahkan file ke staging area |
| `git commit -m "pesan"` | Simpan snapshot dengan pesan |
| `git status` | Cek status file saat ini |
| `git log --oneline` | Lihat riwayat commit (ringkas) |
| `git diff` | Lihat perubahan yang belum di-add |

## 3. Code / Perintah

```bash
# 1. Inisiasi repo
mkdir rumah-impian
cd rumah-impian
git init
# Output: Initialized empty Git repository...

# 2. Buat file
echo "pondasi: selesai" > progress.txt
git status
# Output: Untracked files: progress.txt

# 3. Stage & commit
git add progress.txt
git commit -m "feat: tambah progress pondasi"
# Output: 1 file changed, 1 insertion(+)

# 4. Edit file & lihat diff
echo "dinding: selesai" >> progress.txt
git diff
# Output: +dinding: selesai (warna hijau)

# 5. Lihat riwayat
git log --oneline
# a1b2c3d feat: tambah progress pondasi
```

### Micro-exercise: Commit project Level 1 ke Git

```bash
cd project-level-1
git init
git add .
git commit -m "init: project level 1 selesai"
git log --oneline
```

## 4. Analogi Rumah

### Tabel Analogi

| Konsep Git | Analogi Rumah |
|------------|---------------|
| `git init` | Membeli buku kosong untuk catat renovasi rumah |
| Working directory | Halaman rumah — tempat semua bahan bangunan |
| `git add` (staging) | Memilih bahan apa saja yang mau difoto hari ini |
| Staging area | Meja fotografer — bahan yang siap diabadikan |
| `git commit` | Memotret dan menempel foto ke buku catatan |
| `git status` | Cek apa saja yang berubah di halaman vs yang sudah difoto |
| `git log` | Membalik halaman buku — lihat semua foto renovasi |
| `git diff` | Membandingkan dua foto sebelum dan sesudah renovasi |

### Narasi

Kamu baru beli rumah kosong. Beli buku catatan (`git init`). Hari ini bikin pondasi — ambil foto (`git add`), tempel ke buku dengan keterangan "Pondasi selesai" (`git commit`). Besok bikin dinding, foto lagi, tempel lagi. Kapan pun kamu bisa buka buku (`git log`) dan lihat progres rumahmu.

## 5. Use Case

- Memulai project baru dan ingin track perubahannya dari awal.
- Commit tiap kali fitur selesai — jadi punya checkpoint aman.
- Cek `git status` sebelum pulang kantor biar tau besok lanjut dari mana.
- Pakai `git diff` buat review ulang perubahan sebelum commit.

## 6. Kesalahan Umum

- **Lupa `git add` lalu langsung `git commit`**: Commit kosong, perubahan nggak tersimpan.
- **Pesan commit nggak jelas** (`"update"`, `"fix"`): Besok lupa apa yang diubah.
- **`git status` nggak pernah dijalankan**: Kehilangan konteks file mana yang berubah.
- **Commit sekali sehari dengan perubahan besar**: Susah di-review dan di-rollback.

## 7. Benang Merah

```
Materi 20: Konsep version control — "Gunain Git!"
  ↓ "Gimana cara pakenya?"
Materi 21: Git dasar — init, add, commit, status, log, diff
  ↓ "Udah bisa simpan project lokal. Gimana backup ke cloud?"
Materi 22: Remote repo — push, pull, clone (GitHub)
```

## 8. Soal + Jawaban

**Soal 1:** Apa beda `git add` dan `git commit`? Pakai analogi rumah.
**Jawaban:** `git add` itu memilih bahan bangunan yang mau difoto. `git commit` itu memotret dan menempelkan foto ke buku catatan.

**Soal 2:** Perintah apa yang dipakai untuk melihat riwayat commit secara ringkas?
**Jawaban:** `git log --oneline`

**Soal 3:** Kenapa harus commit dengan pesan yang jelas?
**Jawaban:** Agar bulan depan kamu (atau tim) bisa paham perubahan apa yang terjadi di commit itu tanpa harus baca ulang semua kode.
