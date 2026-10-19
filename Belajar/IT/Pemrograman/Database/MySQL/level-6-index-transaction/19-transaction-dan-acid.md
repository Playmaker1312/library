# 19. Transaction dan ACID — Proses Peminjaman yang Valid

> **Level 6 - Modul K** | **Durasi**: 3 hari | **Prasyarat**: 09 UPDATE, 14 FK, 18 Index

---

## 1. Penjelasan Singkat & Konsep Dasar

Transaction = **paket operasi all-or-nothing**. Jika salah satu gagal, semua batal (rollback).

ACID:
- **Atomicity**: Semua atau tidak sama sekali.
- **Consistency**: Dari valid ke valid (FK, CHECK tetap ok).
- **Isolation**: Transaksi tidak lihat setengah-jadi transaksi lain.
- **Durability**: Setelah COMMIT, permanen meski listrik mati.

Tanpa transaction: stok berkurang tapi peminjaman gagal → data timpang.

**Hubungan dengan materi sebelumnya**: Level 2-4 update stok & insert peminjaman terpisah. 19 bungkus jadi 1 paket aman.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Fitur | Fungsi |
|-------|--------|
| `START TRANSACTION; ... COMMIT;` | Paket atomik |
| `ROLLBACK;` | Batalkan jika error |
| `SAVEPOINT` | Rollback parsial |
| `Isolation Level` (READ COMMITTED, REPEATABLE READ) | Atur seberapa terisolasi (default REPEATABLE READ InnoDB) |
| `SELECT ... FOR UPDATE` | Kunci baris agar tidak race |

InnoDB support ACID, MyISAM tidak!

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── Peminjaman valid: kurangi stok + insert peminjaman ──────────
START TRANSACTION;
-- 1. Kunci baris buku agar tidak diambil orang lain bersamaan
SELECT stok FROM buku WHERE id=1 FOR UPDATE;
-- 2. Cek stok >0 di aplikasi, lalu:
UPDATE buku SET stok = stok - 1 WHERE id=1 AND stok > 0;
-- 3. Jika affected_rows=0 → ROLLBACK (stok habis)
-- 4. Insert peminjaman
INSERT INTO peminjaman (buku_id, anggota_id, tanggal_pinjam, batas_kembali, status)
VALUES (1, 2, CURDATE(), CURDATE() + INTERVAL 14 DAY, 'dipinjam');
COMMIT;
-- Jika error di langkah mana pun: ROLLBACK;

-- ─── Contoh ROLLBACK ─────────────────────────────────────────────
START TRANSACTION;
UPDATE buku SET stok = stok -1 WHERE id=2;
-- Ops, ternyata anggota tidak aktif → batal
ROLLBACK; -- stok kembali

-- ─── SAVEPOINT ───────────────────────────────────────────────────
START TRANSACTION;
INSERT INTO anggota (nomor_anggota, nama, email) VALUES ('ANG-999','Test','test@email.com');
SAVEPOINT sp1;
INSERT INTO peminjaman (buku_id, anggota_id, tanggal_pinjam, batas_kembali) VALUES (1, LAST_INSERT_ID(), CURDATE(), CURDATE()+INTERVAL 7 DAY);
-- Jika insert peminjaman gagal → ROLLBACK TO sp1 (anggota tetap, peminjaman batal)
ROLLBACK TO sp1;
COMMIT;

-- ─── Isolation demo ──────────────────────────────────────────────
-- Transaksi A: START TRANSACTION; UPDATE buku SET stok=10 WHERE id=1;
-- Transaksi B: SELECT stok FROM buku WHERE id=1; -- di REPEATABLE READ, B tidak lihat 10 sampai A COMMIT
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
SELECT @@transaction_isolation;

-- ─── Deadlock cegah: selalu lock urutan sama ─────────────────────
-- Transaksi 1: SELECT ... FOR UPDATE id=1, id=2
-- Transaksi 2: SELECT ... FOR UPDATE id=1, id=2 (sama urutan) → tidak deadlock silang
```

**Skenario Nyata (E-commerce — Order):**

```sql
START TRANSACTION;
SELECT stok FROM produk WHERE id=101 FOR UPDATE;
UPDATE produk SET stok = stok - 2 WHERE id=101 AND stok >=2;
INSERT INTO orders (user_id, produk_id, qty, total) VALUES (5,101,2, 178000);
INSERT INTO payment (order_id, amount) VALUES (LAST_INSERT_ID(), 178000);
COMMIT;
-- Jika stok tidak cukup atau payment gagal → ROLLBACK semua
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| **Transaction** | **Proses peminjaman**: ambil buku + catat pinjam + cap tanggal — harus semua jadi, kalau cap gagal, buku dikembalikan |
| **Atomicity** | **All-or-nothing** — tidak ada "buku diambil tapi tidak tercatat" |
| **Consistency** | **Aturan tetap**: stok tidak negatif, buku harus ada |
| **Isolation** | **Dua petugas tidak bisa pinjamkan buku terakhir bersamaan** — kunci dulu (FOR UPDATE) |
| **Durability** | **Catatan permanen** — setelah COMMIT, blackout pun catatan tetap |

> 💡 **Key Insight**: Transaction = *paket hadiah* — pita tidak putus di tengah jalan.

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Wajib Transaction | Tidak Perlu |
|-------------------|-------------|
| Transfer stok, order, payment (multi-table) | SELECT saja |
| Stok decrement + insert histori | Single UPDATE |
| Batch import yang harus all-or-nothing | Log append |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ Selalu START TRANSACTION untuk multi-statement kritis
-- ✅ Pakai SELECT ... FOR UPDATE untuk cegah race
-- ✅ Cek affected_rows setelah UPDATE stok
-- ✅ COMMIT cepat, jangan tahan transaksi lama (bikin lock)
-- ✅ Set isolation READ COMMITTED jika butuh fresh read (default REPEATABLE READ ok untuk most)

-- CLI
SHOW ENGINE INNODB STATUS; -- lihat deadlock
```

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **Lupa COMMIT** | Lock tertahan, timeout | COMMIT/ROLLBACK eksplisit |
| **Tidak pakai FOR UPDATE** | Race: 2 orang pinjam stok 1 → stok -1 | `SELECT ... FOR UPDATE` |
| **Transaksi terlalu lama** | Deadlock, lock wait | Pendek, hanya yang perlu |
| **Pakai MyISAM** | Tidak ada rollback! | `ENGINE=InnoDB` |
| **Deadlock urutan lock beda** | `Deadlock found` | Lock urutan ID sama |

**Troubleshooting:**

```sql
SHOW PROCESSLIST;
SHOW ENGINE INNODB STATUS\G
-- Cari LATEST DETECTED DEADLOCK
SET innodb_print_all_deadlocks = ON;
```

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: Transaction Pinjam

> Buat transaksi pinjam buku id=3 oleh anggota 2, kurangi stok, insert peminjaman, handle stok habis.

**Jawaban:**

```sql
START TRANSACTION;
SELECT stok FROM buku WHERE id=3 FOR UPDATE;
UPDATE buku SET stok=stok-1 WHERE id=3 AND stok>0;
-- Di app: if ROW_COUNT()=0 → ROLLBACK; SELECT 'Stok habis';
INSERT INTO peminjaman (buku_id, anggota_id, tanggal_pinjam, batas_kembali) VALUES (3,2,CURDATE(),CURDATE()+INTERVAL 14 DAY);
COMMIT;
SELECT stok FROM buku WHERE id=3;
```

### Tantangan 2: Transfer Stok Antar Buku (Simulasi)

> Pindah 2 stok dari buku 1 ke buku 2 secara atomik.

**Jawaban:**

```sql
START TRANSACTION;
UPDATE buku SET stok=stok-2 WHERE id=1 AND stok>=2;
-- cek affected_rows, jika 0 ROLLBACK
UPDATE buku SET stok=stok+2 WHERE id=2;
COMMIT;
-- Jika salah satu gagal → ROLLBACK, tidak ada stok hilang
```

