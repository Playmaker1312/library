# 16. Normal Form 1-3 — Aturan Desain Database

> **Level 5 - Modul I** | **Durasi**: 3 hari | **Prasyarat**: Level 4 (FK, JOIN)

---

## 1. Penjelasan Singkat & Konsep Dasar

Normalisasi = **hilangkan redundansi**, simpan tiap fakta di 1 tempat.

- **1NF**: Kolom atomik — tidak ada `kategori="Teknologi, Sains"` dalam 1 kolom.
- **2NF**: Sudah 1NF + tidak ada *partial dependency* (kolom tergantung sebagian PK komposit).
- **3NF**: Sudah 2NF + tidak ada *transitive dependency* (kolom tergantung kolom non-key lain, cth `buku.pengarang → negara_pengarang`).

**Hubungan dengan materi sebelumnya**: Level 4 buat tabel terhubung tapi masih duplikat (nama pengarang diulang per buku). 16 pecah agar update 1 tempat berlaku di mana-mana.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| NF | Aturan | Masalah Diselesaikan |
|----|--------|----------------------|
| 1NF | Atomik, tidak ada multi-value | Hindari `FIND_IN_SET`, update parsial |
| 2NF | Full dependency pada PK komposit | Hindari data buku duplikat di `peminjaman_detail` |
| 3NF | Tidak ada transitif | `negara_pengarang` pindah ke `pengarang` |

Tanpa normalisasi: update nama pengarang harus 50 baris → inkonsisten jika 1 terlewat.

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── 1NF: pisahkan multi-value ───────────────────────────────────
-- ❌ buku.kategori = 'Teknologi, Programming'
-- ✅ buku_kategori pivot
CREATE TABLE kategori (id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY, nama VARCHAR(50) UNIQUE) ENGINE=InnoDB;
CREATE TABLE buku_kategori (
    buku_id INT UNSIGNED NOT NULL, kategori_id INT UNSIGNED NOT NULL,
    PRIMARY KEY (buku_id,kategori_id),
    FOREIGN KEY (buku_id) REFERENCES buku(id) ON DELETE CASCADE,
    FOREIGN KEY (kategori_id) REFERENCES kategori(id) ON DELETE CASCADE
) ENGINE=InnoDB;

-- ─── 3NF: pisahkan pengarang ─────────────────────────────────────
CREATE TABLE pengarang (id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY, nama VARCHAR(100) UNIQUE NOT NULL, negara VARCHAR(50)) ENGINE=InnoDB;
INSERT INTO pengarang (nama) SELECT DISTINCT pengarang FROM buku;

ALTER TABLE buku ADD COLUMN pengarang_id INT UNSIGNED AFTER pengarang,
                 ADD FOREIGN KEY (pengarang_id) REFERENCES pengarang(id);
UPDATE buku b JOIN pengarang p ON b.pengarang=p.nama SET b.pengarang_id=p.id;
-- Setelah verifikasi: ALTER TABLE buku DROP COLUMN pengarang;

-- Update 1 tempat → semua buku ter-update
UPDATE pengarang SET nama='Robert C. Martin' WHERE nama='Robert C. Martin';

-- Query dengan 3NF
SELECT b.judul, p.nama AS pengarang, p.negara FROM buku b JOIN pengarang p ON b.pengarang_id=p.id;
```

**Skenario Nyata (E-commerce):**

```sql
-- ❌ orders: user_nama, user_email (duplikat per order)
-- ✅ users terpisah, orders hanya user_id
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| **Tanpa normalisasi** | **Tulis nama pengarang di setiap buku** — 50 buku = 50x tulis, salah 1 = inkonsisten |
| **1NF** | **Satu label satu nilai** — tidak tulis "Fiksi, Sains" di 1 label |
| **3NF** | **Buku catatan pengarang terpisah** — ganti nama cukup 1x di catatan pengarang, semua buku ikut |

> 💡 **Key Insight**: Satu fakta satu tempat. Ubah sekali, berlaku di mana-mana.

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | Aksi |
|---------|------|
| Kolom isi koma-separated | 1NF → tabel pivot |
| Data duplikat di banyak baris | 2NF/3NF → tabel terpisah + FK |
| Update 1 entitas harus banyak baris | Normalisasi |

**Kapan denormalisasi?** Untuk *read-heavy* laporan — tambah `total_pinjam` cache di `buku` agar tidak JOIN tiap query (trade-off: butuh trigger/sync).

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ Minimal 3NF untuk OLTP
-- ✅ Tiap tabel punya PK
-- ✅ FK untuk relasi
-- ✅ Pilih normalisasi dulu, denormalisasi jika terbukti lambat (ukur via EXPLAIN)
```

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **Over-normalisasi** | JOIN 10 tabel untuk 1 query | Denormalisasi selektif + index |
| **Simpan CSV di 1 kolom** | Tidak bisa JOIN, update susah | Pivot table |
| **Tidak pakai FK setelah pecah** | Orphan data | FK + RESTRICT |

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: 3NF Pengarang

> Pecah `buku.pengarang` ke tabel `pengarang` dan migrasi data.

**Jawaban:** Lihat contoh di atas — `CREATE pengarang`, `INSERT DISTINCT`, `ADD pengarang_id`, `UPDATE JOIN`, verifikasi, `DROP COLUMN`.

### Tantangan 2: Denormalisasi

> Kapan tambah `jumlah_buku` di `pengarang` sebagai cache?

**Jawaban:**

```sql
ALTER TABLE pengarang ADD COLUMN jumlah_buku INT UNSIGNED DEFAULT 0;
-- Update via trigger atau cron, bukan manual
-- Gunakan jika query COUNT per pengarang sering & lambat, dan toleran staleness
```

