# 20 — Mengapa Version Control — Masalah yang Diselesaikan Git

## 1. Penjelasan

Version Control System (VCS) adalah sistem yang mencatat setiap perubahan pada file dari waktu ke waktu. Git adalah VCS paling populer saat ini. Tanpa Git, developer menyimpan file dengan nama kacau seperti `final_v2_BENERAN_FINAL.js` — dan tetap kehilangan pekerjaan saat file terhapus atau salah edit.

Git menyelesaikan tiga masalah inti:
- **Snapshot**: Rekam keadaan seluruh project di titik tertentu.
- **History**: Lihat siapa mengubah apa dan kapan.
- **Rollback**: Kembali ke versi kapan saja.

## 2. Fungsi

| Fungsi | Manfaat |
|--------|---------|
| Version tracking | Catat setiap perubahan |
| Collaboration | Banyak orang kerja di project sama |
| Backup | Project aman di banyak tempat |
| Experimentation | Coba fitur baru tanpa takut rusak |

## 3. Code / Perintah

Simulasi masalah tanpa version control (narasi + command dummy):

```bash
# Tanpa Git — bencana
ls -la project/
# Output: final.js, final_v2.js, final_v3.js, final_BENERAN.js,
#         final_BENERAN_lagian.js, final_FIX.js

# Dengan Git — cukup satu perintah
git log --oneline
# a1b2c3d feat: tambah validasi input
# e4f5g6h fix: perbaiki bug login
# i7j8k9l init: project awal

# Rollback ke versi lama
git checkout a1b2c3d
```

## 4. Analogi Rumah

### Tabel Analogi

| Konsep Git | Analogi Rumah |
|------------|---------------|
| Snapshot | Foto kondisi rumah setelah renovasi |
| History | Buku catatan semua renovasi yang pernah dilakukan |
| Rollback | Kembali ke desain rumah versi tahun lalu |
| Working directory | Rumah dalam kondisi saat ini |
| Repository | Gudang penyimpanan semua blueprint rumah |

### Narasi

Kamu membangun rumah. Setiap kali selesai satu ruangan, kamu foto dan catat di buku biru. Suatu hari cat tembok jelek — tinggal buka buku, lihat foto lama, cat ulang. Tanpa sistem ini? Kamu cuma ingat "dulu pakai warna krem... atau putih?" Git adalah buku biru dan kamera digital untuk project coding kamu.

## 5. Use Case

- Seorang freelancer ingin mengembalikan project ke kondisi sebelum klien minta fitur yang ternyata nggak jadi.
- Tim 5 orang mengerjakan aplikasi yang sama tanpa saling timpa file.
- Developer ingin coba refactor besar — kalau gagal, tinggal `git checkout` kembali.

## 6. Kesalahan Umum

- **Beri nama file `final_BENERAN.js`**: Tanda putus asa. Pakai Git.
- **Simpan project cuma di satu folder lokal**: Harddisk rusak = semua hilang.
- **Edit file tanpa commit**: Nggak ada checkpoint. Mau balik nggak bisa.
- **Bingung `git checkout` vs `git reset`**: Nanti dipelajari — yang penting tau Git punya safety net.

## 7. Benang Merah

```
Level 1 (Project JavaScript selesai)
  ↓ "Gimana caranya project ini nggak hilang?"
Level 2 — Materi 20: Mengapa Version Control
  ↓ "Oke, pakai Git. Tapi gimana mulainya?"
Level 2 — Materi 21: Git Dasar (init, add, commit)
```

Kamu sudah bisa bikin project. Sekarang kamu belajar cara **melindungi** project itu.

## 8. Soal + Jawaban

**Soal 1:** Sebutkan 3 masalah utama yang diselesaikan oleh Git.
**Jawaban:** Snapshot (rekam kondisi project), History (catat semua perubahan), Rollback (kembali ke versi sebelumnya).

**Soal 2:** Apa analogi yang tepat untuk "snapshot" di Git?
**Jawaban:** Foto rumah setelah renovasi — merekam kondisi tepat di titik tertentu.

**Soal 3:** Kenapa menyimpan file dengan nama `final_v2_BENERAN.js` adalah praktik buruk?
**Jawaban:** Karena membingungkan, tidak ada riwayat perubahan, dan sangat rentan kehilangan data jika file terhapus atau salah edit.
