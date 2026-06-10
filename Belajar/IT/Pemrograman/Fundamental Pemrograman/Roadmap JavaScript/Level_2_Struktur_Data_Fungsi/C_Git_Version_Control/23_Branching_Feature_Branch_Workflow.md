# 23 — Branching — Feature Branch Workflow

## 1. Penjelasan

Branch adalah **cabang** — salinan terpisah dari project yang bisa kamu ubah tanpa memengaruhi branch utama (`main` atau `master`). Ini memungkinkan kamu dan tim mengerjakan fitur berbeda secara paralel.

Konsep utama:
- **git branch**: Buat, lihat, hapus cabang.
- **git checkout** atau **git switch**: Pindah ke cabang lain.
- **git merge**: Gabung perubahan dari satu cabang ke cabang lain.
- **Merge conflict**: Terjadi ketika dua cabang mengubah baris yang sama. Git bingung yang mana yang dipakai.

## 2. Fungsi

| Perintah | Fungsi |
|----------|--------|
| `git branch` | Lihat daftar branch |
| `git branch <nama>` | Buat branch baru |
| `git checkout <nama>` | Pindah ke branch lain |
| `git switch <nama>` | Alternatif checkout (lebih aman) |
| `git merge <branch>` | Gabung branch ke branch aktif |
| `git branch -d <nama>` | Hapus branch |

## 3. Code / Perintah

### Buat branch → commit → merge

```bash
# 1. Cek branch aktif
git branch
# * main

# 2. Buat branch fitur
git branch fitur-pintu

# 3. Pindah ke branch fitur
git switch fitur-pintu

# 4. Kerja di branch fitur
echo "pintu: model sliding" > pintu.txt
git add .
git commit -m "feat: tambah pintu sliding"

# 5. Kembali ke main & merge
git switch main
git merge fitur-pintu
# Output: Fast-forward atau merge commit

# 6. Hapus branch (sudah nggak diperlukan)
git branch -d fitur-pintu
```

### Merge conflict (simulasi)

```bash
# Di branch main — ubah baris yang sama
echo "warna cat: biru" > catatan.txt
git add . && git commit -m "fix: ganti warna cat jadi biru"

# Di branch eksperimen — ubah baris yang sama
git switch -c eksperimen-warna
echo "warna cat: hijau" > catatan.txt
git add . && git commit -m "feat: coba warna hijau"

# Merge — konflik!
git switch main
git merge eksperimen-warna
# CONFLICT: catatan.txt — harus diselesaikan manual
```

Cara selesaikan: buka file `catatan.txt`, pilih salah satu atau gabung, lalu:

```bash
git add catatan.txt
git commit -m "fix: resolve conflict warna cat"
```

### Micro-exercise: Feature branch

```bash
git switch -c fitur-dashboard
# bikin fitur dashboard baru...
git add .
git commit -m "feat: dashboard layout"
git switch main
git merge fitur-dashboard
```

## 4. Analogi Rumah

### Tabel Analogi

| Konsep Git | Analogi Rumah |
|------------|---------------|
| Branch | Kamar terpisah — kamu renovasi kamar tidur tanpa ganggu ruang tamu |
| `main` branch | Ruang utama — kondisi rumah yang sudah jadi dan stabil |
| Feature branch | Kamar baru yang sedang dibangun di lantai 2 |
| `git switch` | Pindah dari ruang tamu ke kamar yang direnovasi |
| `git merge` | Menyatukan kamar baru ke denah rumah utama |
| Merge conflict | Dua tukang pasang pintu di lubang yang sama — harus diskusi |

### Narasi

Rumahmu punya ruang tamu (`main`) — kondisi rapi, tamu bisa datang kapan saja. Kamu ingin bikin kamar tidur baru (`branch: kamar-tidur`). Kamu pindah ke kamar itu (`git switch`), bangun dinding, cat, pasang jendela (commit). Sementara itu ruang tamu tetap rapi — tamu nggak terganggu. Setelah kamar jadi, kamu gabung ke denah utama (`git merge`). Kalau dua tukang barengan pasang pintu di tempat sama, konflik — kamu harus mutusin pake pintu siapa.

## 5. Use Case

- Kerja fitur baru tanpa ganggu branch `main` yang stabil.
- Dua developer kerjakan dua fitur berbeda secara paralel.
- Coba eksperimen di branch — kalau gagal, tinggal hapus branch.
- Bugfix urgent: buat `branch hotfix` dari `main`, fix, merge — tanpa bawa fitur yang belum selesai.

## 6. Kesalahan Umum

- **Langsung commit ke `main`**: Di tim besar, ini chaos. Selalu bikin branch.
- **Lupa pindah branch**: Kerja keras 3 jam — ternyata masih di `main`. Pake `git status` atau `git branch` buat cek.
- **Merge conflict panik**: Tenang — buka file, cari marker `<<<<<<<`, `=======`, `>>>>>>>`, edit manual, lalu `git add` + `git commit`.
- **Bikin branch dari branch lain yang belum di-merge**: Bisa, tapi siap-siap conflict berantai.

## 7. Benang Merah

```
Materi 22: Remote repo — push project ke GitHub
  ↓ "Gimana caranya kerja fitur baru tanpa rusakin project utama?"
Materi 23: Branching — feature branch workflow
  ↓ "Struktur data & fungsi udah. Materi selanjutnya: array"
```

## 8. Soal + Jawaban

**Soal 1:** Kenapa kita perlu branch? Beri analogi rumah.
**Jawaban:** Biar renovasi kamar baru (fitur) nggak ganggu ruang tamu (main) yang sedang dipakai tamu (production).

**Soal 2:** Apa yang terjadi kalau dua branch mengubah baris kode yang sama?
**Jawaban:** Terjadi merge conflict — Git tidak tahu perubahan mana yang dipakai. Harus diselesaikan manual dengan memilih salah satu atau menggabungkan.

**Soal 3:** Perintah apa untuk membuat branch baru dan langsung pindah ke branch itu?
**Jawaban:** `git switch -c <nama-branch>` atau `git checkout -b <nama-branch>`.
