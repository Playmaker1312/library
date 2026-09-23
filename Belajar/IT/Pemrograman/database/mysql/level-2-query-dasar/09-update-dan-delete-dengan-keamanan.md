# 9. UPDATE dan DELETE — Dengan Keamanan

> **Level 2 - Modul D** | **Durasi**: 2 hari | **Prasyarat**: 07 WHERE

---

## 1. Penjelasan Singkat & Konsep Dasar

`SELECT` membaca, `UPDATE` mengubah, `DELETE` menghapus. Keduanya **berbahaya**: tanpa `WHERE`, semua baris kena!

- `UPDATE t SET col=val WHERE id=?` → ubah baris spesifik (pakai PK paling aman).
- `DELETE FROM t WHERE ...` → hapus baris spesifik.
- **Soft delete** → `UPDATE SET deleted_at=NOW()` — data tidak hilang, hanya ditandai. Lebih aman untuk data penting.

**Hubungan dengan materi sebelumnya**: Level 1 INSERT isi data, Level 2 SELECT/WHERE ambil data. 09 melengkapi siklus CRUD: *Create-Read-Update-Delete* — tapi dengan *safety harness*.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Perintah | Fungsi |
|----------|--------|
| `UPDATE ... SET ... WHERE ...` | Ubah data — diskon, stok, status |
| `UPDATE ... SET col=col+1` | Update berbasis nilai lama (increment) |
| `DELETE FROM ... WHERE ...` | Hapus permanen |
| `TRUNCATE TABLE` | Hapus semua + reset AUTO_INCREMENT (cepat, tidak bisa rollback) |
| `Soft Delete (deleted_at)` | Tandai hapus tanpa hilang — bisa restore, audit |

Tanpa pemahaman ini: `UPDATE buku SET harga=0;` → semua buku gratis!

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── UPDATE aman (pakai PK) ──────────────────────────────────────
UPDATE buku SET stok = 7 WHERE id = 1;

-- Multi-kolom
UPDATE buku SET harga = 160000.00, stok = 10, kategori='Teknologi' WHERE id=1;

-- Ekspresi (kurangi stok saat pinjam)
UPDATE buku SET stok = stok - 1 WHERE id=1 AND stok > 0; -- cegah stok negatif!

-- Diskon 10% untuk Fiksi lama
UPDATE buku SET harga = harga * 0.9 WHERE kategori='Fiksi' AND tahun < 2010;

-- ─── Safety: test dengan SELECT dulu ─────────────────────────────
SELECT * FROM buku WHERE kategori='Fiksi' AND tahun < 2010; -- cek dulu
-- UPDATE buku SET harga = harga * 0.9 WHERE kategori='Fiksi' AND tahun < 2010;

-- ─── DELETE aman ─────────────────────────────────────────────────
DELETE FROM buku WHERE id = 11;
DELETE FROM buku WHERE stok = 0 AND tahun < 2000;

-- Test dulu!
SELECT * FROM buku WHERE stok=0 AND tahun<2000;
-- DELETE FROM buku WHERE stok=0 AND tahun<2000;

-- ─── TRUNCATE (reset) ────────────────────────────────────────────
-- TRUNCATE TABLE buku; -- HATI-HATI: hapus semua, reset AUTO_INCREMENT, tidak bisa rollback di MySQL!

-- ─── SOFT DELETE (REKOMENDASI) ───────────────────────────────────
ALTER TABLE buku ADD COLUMN deleted_at TIMESTAMP NULL DEFAULT NULL;
CREATE INDEX idx_deleted_at ON buku(deleted_at);

-- "Hapus" → tandai
UPDATE buku SET deleted_at = NOW() WHERE id = 5;

-- Query hanya aktif
SELECT * FROM buku WHERE deleted_at IS NULL;

-- Restore
UPDATE buku SET deleted_at = NULL WHERE id=5;

-- View untuk otomatis filter
CREATE VIEW buku_aktif AS SELECT * FROM buku WHERE deleted_at IS NULL;
SELECT * FROM buku_aktif;

-- Hard delete hanya untuk data soft-deleted lama
DELETE FROM buku WHERE deleted_at IS NOT NULL AND deleted_at < NOW() - INTERVAL 30 DAY;
```

**Skenario Nyata (E-commerce):**

```sql
-- Stok berkurang saat order (atomic)
UPDATE produk SET stok = stok - 2 WHERE id=101 AND stok >= 2;
-- Cek affected_rows: jika 0 → stok tidak cukup, batalkan order

-- Soft delete produk (jangan hard delete, ada histori order)
UPDATE produk SET deleted_at = NOW(), is_active=0 WHERE id=101;

-- Hard delete hanya via cron setelah 90 hari
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| `UPDATE WHERE id=1` | **Ubah harga buku nomor 1** — pustakawan cari by nomor inventaris, ubah labelnya |
| `UPDATE tanpa WHERE` | **Teriak "ubah semua harga jadi 0!"** — semua rak kena! Bencana! |
| `DELETE WHERE id=11` | **Buang buku nomor 11** ke gudang sampah |
| `DELETE tanpa WHERE` | **Bakar semua buku!** |
| `Soft Delete` | **Pindah buku ke gudang "arsip"** — tidak di rak display, tapi masih ada, bisa kembalikan |
| `SELECT dulu sebelum UPDATE` | **Cek rak dulu sebelum ubah** — pastikan buku yang mau diubah benar |

> 💡 **Key Insight**: Pustakawan cek daftar dulu sebelum eksekusi — kamu juga harus `SELECT` dulu sebelum `UPDATE/DELETE`!

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | Pakai |
|---------|-------|
| Ubah harga/stok 1 produk | `UPDATE ... WHERE id=?` (PK) |
| Diskon massal | `UPDATE ... WHERE kategori='X'` + test SELECT dulu |
| Kurangi stok saat transaksi | `UPDATE SET stok=stok-1 WHERE id=? AND stok>0` |
| Hapus data uji | `DELETE WHERE id=?` |
| Hapus data penting (user, order) | **Soft delete** `deleted_at` |
| Reset tabel dev | `TRUNCATE` (jangan di prod!) |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ SELALU WHERE, pakai PK jika bisa
UPDATE buku SET stok=7 WHERE id=1;

-- ✅ Test dengan SELECT dulu
SELECT * FROM buku WHERE kategori='Fiksi' AND tahun<2010;
-- UPDATE ...

-- ✅ Cegah stok negatif
UPDATE buku SET stok=stok-1 WHERE id=1 AND stok>0;

-- ✅ Soft delete untuk data penting
ALTER TABLE buku ADD COLUMN deleted_at TIMESTAMP NULL;

-- ✅ Transaksi untuk update kritis
START TRANSACTION;
UPDATE produk SET stok=stok-1 WHERE id=1 AND stok>0;
-- cek affected_rows, jika 0 ROLLBACK
COMMIT;

-- ❌ JANGAN: UPDATE/DELETE tanpa WHERE
-- UPDATE buku SET harga=0; -- bencana!
```

**Best Practices:**
- Aktifkan `SQL_SAFE_UPDATES` di Workbench (`SET SQL_SAFE_UPDATES=1;`) — tolak UPDATE/DELETE tanpa WHERE/KEY.
- Gunakan `LIMIT 1` untuk extra safety: `DELETE WHERE id=5 LIMIT 1`.
- Audit: tambah `updated_at TIMESTAMP ON UPDATE CURRENT_TIMESTAMP`.

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **UPDATE tanpa WHERE** | Semua baris berubah | Selalu WHERE, test SELECT dulu, `SAFE_UPDATES=1` |
| **DELETE tanpa WHERE** | Semua hilang | Sama + soft delete |
| **Stok jadi negatif** | `stok -1` tanpa cek | `WHERE stok>0` |
| **TRUNCATE di production** | Hilang permanen, tidak rollback | Jangan! Pakai DELETE + WHERE |
| **Hard delete data berelasi** | FK error / histori hilang | Soft delete |
| **Lupa `deleted_at IS NULL` di SELECT** | Data terhapus muncul | Buat VIEW `buku_aktif` atau selalu filter |

**Troubleshooting:**

```sql
SET SQL_SAFE_UPDATES=1;
UPDATE buku SET harga=0; -- ERROR 1175: You are using safe update mode

-- Cek berapa baris kena
SELECT ROW_COUNT(); -- setelah UPDATE

-- Restore soft delete
UPDATE buku SET deleted_at=NULL WHERE id=5;

-- Cek yang ter-soft-delete
SELECT id, judul, deleted_at FROM buku WHERE deleted_at IS NOT NULL;
```

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: UPDATE Aman + Validasi Stok

> (a) Update stok buku id 3 jadi 5, (b) kurangi stok id 3 sebanyak 1 hanya jika stok >0, (c) diskon 10% untuk kategori Teknologi.

**Jawaban:**

```sql
-- (a)
UPDATE buku SET stok=5 WHERE id=3;
SELECT id, judul, stok FROM buku WHERE id=3;

-- (b) atomic decrement
UPDATE buku SET stok=stok-1 WHERE id=3 AND stok>0;
SELECT ROW_COUNT(); -- 1 jika berhasil, 0 jika stok 0

-- (c) test dulu
SELECT id, judul, harga FROM buku WHERE kategori='Teknologi';
UPDATE buku SET harga = ROUND(harga*0.9,0) WHERE kategori='Teknologi';
SELECT id, judul, harga FROM buku WHERE kategori='Teknologi';
```

**Pembahasan**: (b) adalah pola *optimistic locking* untuk stok — cegah race condition tanpa transaction. Selalu test SELECT sebelum mass UPDATE.

---

### Tantangan 2: Soft Delete

> Tambah `deleted_at`, soft-delete buku dengan stok 0, tampilkan hanya buku aktif, lalu restore.

**Jawaban:**

```sql
ALTER TABLE buku ADD COLUMN deleted_at TIMESTAMP NULL DEFAULT NULL;

-- Soft delete stok 0
SELECT id, judul, stok FROM buku WHERE stok=0 AND deleted_at IS NULL;
UPDATE buku SET deleted_at=NOW() WHERE stok=0 AND deleted_at IS NULL;

-- Hanya aktif
SELECT id, judul, stok FROM buku WHERE deleted_at IS NULL ORDER BY id;

-- Restore id 7 (contoh)
UPDATE buku SET deleted_at=NULL WHERE id=7;
SELECT id, judul, deleted_at FROM buku WHERE id=7;

-- View helper
CREATE OR REPLACE VIEW buku_aktif AS
SELECT id, judul, pengarang, harga, stok, kategori FROM buku WHERE deleted_at IS NULL;
SELECT * FROM buku_aktif;
```

**Pembahasan**: Soft delete = `UPDATE`, bukan `DELETE`. Semua query produksi harus filter `deleted_at IS NULL` atau pakai VIEW. Hard delete hanya untuk cleanup data lama via cron.

