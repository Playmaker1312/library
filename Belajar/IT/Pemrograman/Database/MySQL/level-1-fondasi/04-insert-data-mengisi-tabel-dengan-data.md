# 4. Insert Data — Mengisi Tabel dengan Data

> **Level 1 - Modul A** | **Durasi**: 1 hari | **Prasyarat**: 03 Membuat Database & Tabel

---

## 1. Penjelasan Singkat & Konsep Dasar

Tabel `buku` sudah jadi (rak sudah terpasang). Sekarang kita **taruh buku di rak** via `INSERT`.

- `INSERT INTO ... VALUES` = perintah untuk menambah baris baru.
- **Urutan kolom explicit** (`INSERT INTO buku (judul, pengarang) VALUES (...)`) = wajib — jangan andalkan urutan default tabel.
- **Bulk insert** = satu `INSERT` banyak `VALUES` — jauh lebih cepat daripada 100x `INSERT` satu-satu.
- **Validasi otomatis**: MySQL tolak jika `NOT NULL` dilanggar, `UNIQUE` duplikat, atau tipe tidak cocok.

**Hubungan dengan materi sebelumnya**: 03 mendefinisikan *sekat rak* (skema). 04 menguji sekat itu: apakah tipe & constraint bekerja? Data nyata ini akan jadi bahan query di Level 2-3.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Fitur | Fungsi |
|-------|--------|
| `INSERT INTO ... (kolom) VALUES (...)` | Tambah data dengan mapping kolom jelas |
| `INSERT ... VALUES (...), (...), (...)` | Bulk insert — 1 round-trip untuk banyak baris (10x lebih cepat) |
| `INSERT ... ON DUPLICATE KEY UPDATE` | Upsert — jika `UNIQUE` sudah ada, update, bukan error |
| `INSERT IGNORE` | Abaikan error duplikat, lanjut baris lain |
| `DEFAULT / NULL` handling | Isi otomatis untuk `TIMESTAMP`, `DEFAULT 0`, `AUTO_INCREMENT` |

Masalah yang diselesaikan: tanpa `INSERT` yang benar, kamu akan dapat `Column count doesn't match`, duplikat `PRIMARY KEY`, atau performa insert lambat untuk seed data ribuan baris.

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── 1. Insert satu baris (explicit kolom) ───────────────────────
INSERT INTO buku (judul, pengarang, tahun, harga, stok, kategori, isbn)
VALUES ('Clean Code', 'Robert C. Martin', 2008, 150000.00, 5, 'Teknologi', '9780132350884');

-- ─── 2. Bulk insert (REKOMENDASI untuk seed) ─────────────────────
INSERT INTO buku (judul, pengarang, tahun, harga, stok, kategori, isbn)
VALUES
    ('Laskar Pelangi', 'Andrea Hirata', 2005, 95000.00, 8, 'Fiksi', '9789793062792'),
    ('The Pragmatic Programmer', 'Andrew Hunt', 1999, 175000.00, 3, 'Teknologi', '9780201616224'),
    ('Cosmos', 'Carl Sagan', 1980, 200000.00, 2, 'Sains', '9780345539434'),
    ('Bumi Manusia', 'Pramoedya Ananta Toer', 1980, 85000.00, 6, 'Fiksi', '9789799731234'),
    ('Sapiens', 'Yuval Noah Harari', 2011, 135000.00, 4, 'Sejarah', '9780099590088'),
    ('The Great Gatsby', 'F. Scott Fitzgerald', 1925, 110000.00, 0, 'Fiksi', '9780743273565'),
    ('A Brief History of Time', 'Stephen Hawking', 1988, 180000.00, 1, 'Sains', '9780553380163'),
    ('Design Patterns', 'Gang of Four', 1994, 220000.00, 2, 'Teknologi', '9780201633610'),
    ('Filosofi Teras', 'Henry Manampiring', 2018, 79000.00, 10, 'Umum', '9786020652056');

-- Verifikasi
SELECT id, judul, stok, harga FROM buku ORDER BY id;

-- ─── 3. Insert tanpa sebut kolom (JANGAN!) ───────────────────────
-- Rapuh: jika urutan kolom berubah (ALTER TABLE), query ini salah
-- INSERT INTO buku VALUES (NULL, 'Test', 'Author', 2024, 50000, 1, 'Umum', '123', NOW(), NOW());

-- ─── 4. Upsert: ON DUPLICATE KEY UPDATE ──────────────────────────
-- Kasus: stok buku datang lagi, ISBN sudah ada → tambah stok
INSERT INTO buku (judul, pengarang, tahun, harga, stok, kategori, isbn)
VALUES ('Clean Code', 'Robert C. Martin', 2008, 150000.00, 3, 'Teknologi', '9780132350884')
ON DUPLICATE KEY UPDATE
    stok = stok + VALUES(stok),
    harga = VALUES(harga),
    updated_at = NOW();
-- Jika isbn 9780132350884 sudah ada → stok 5+3=8, bukan error duplicate

-- ─── 5. Abaikan duplikat ─────────────────────────────────────────
INSERT IGNORE INTO buku (judul, pengarang, tahun, harga, stok, isbn)
VALUES ('Duplikat', 'X', 2024, 50000, 1, '9780132350884');
-- Tidak error, tapi baris di-skip (warning)

-- ─── 6. Insert dengan SELECT (copy data) ─────────────────────────
CREATE TABLE buku_backup LIKE buku;
INSERT INTO buku_backup SELECT * FROM buku WHERE stok > 0;
```

**Skenario Nyata (E-commerce — Seed Produk):**

```sql
-- Import 1000 produk dari CSV via bulk insert (generate dari script)
INSERT INTO produk (sku, nama, harga, stok, kategori_id)
VALUES
    ('SKU-001', 'Kaos Hitam M', 89000.00, 100, 1),
    ('SKU-002', 'Kaos Putih L', 89000.00, 50, 1)
    -- ... 998 baris lagi dalam 1 query = 1 transaksi, jauh lebih cepat
    ;

-- Untuk file besar, gunakan LOAD DATA INFILE (paling cepat)
LOAD DATA INFILE '/tmp/produk.csv'
INTO TABLE produk
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(sku, nama, harga, stok);
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| `INSERT INTO buku (judul...) VALUES (...)` | **Taruh buku baru ke rak** sesuai label sekat — judul di sekat judul, harga di sekat harga |
| `Bulk VALUES (...),(...),(...)` | **Datang dengan kardus berisi 10 buku** — taruh sekaligus, bukan bolak-balik 10x |
| `ON DUPLICATE KEY UPDATE` | **Buku dengan nomor inventaris sudah ada** → bukan tambah rak baru, tapi *tambah stok* di rak yang sama |
| `INSERT IGNORE` | **Buku kembar ditolak diam-diam** — pustakawan skip tanpa marah |
| `AUTO_INCREMENT id` | **Stempel nomor inventaris otomatis** — kamu tidak perlu tulis manual, mesin yang cap |

> 💡 **Key Insight**: Selalu sebut nama sekat (`(judul, pengarang)`) saat taruh buku — jangan asal lempar ke rak!

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | Teknik |
|---------|--------|
| Seed data awal / testing | `INSERT ... VALUES (...),(...),(...)` bulk |
| Import dari aplikasi (form tambah buku) | `INSERT INTO buku (judul,...) VALUES (?, ?, ...)` dengan prepared statement |
| Sinkronisasi data (ISBN sudah ada → update stok) | `ON DUPLICATE KEY UPDATE stok = stok + VALUES(stok)` |
| Import massal CSV ribuan baris | `LOAD DATA INFILE` (100x lebih cepat) |
| Copy data antar tabel | `INSERT INTO backup SELECT * FROM buku WHERE ...` |
| Data duplikat tidak fatal | `INSERT IGNORE` |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ DO: explicit kolom, bulk, prepared statement
INSERT INTO buku (judul, pengarang, tahun, harga, stok, kategori, isbn)
VALUES (?, ?, ?, ?, ?, ?, ?); -- di aplikasi: pakai placeholder, cegah SQL Injection!

-- ❌ DON'T: tanpa kolom, satu-per-satu, string concat
-- INSERT INTO buku VALUES (NULL, 'judul', ...); -- rapuh!
-- "INSERT INTO buku VALUES ('" + judul + "')" -- SQL Injection!

-- ✅ Transaksi untuk bulk penting
START TRANSACTION;
INSERT INTO buku (...) VALUES (...), (...), ...;
COMMIT; -- semua berhasil atau semua gagal (atomic)

-- ✅ Cek warning setelah INSERT IGNORE
SHOW WARNINGS;
```

**Best Practices:**
- **Selalu sebut kolom** — tahan terhadap `ALTER TABLE`.
- **Bulk insert** untuk seed >10 baris — kurangi round-trip.
- **Gunakan prepared statement** di app (Laravel `DB::insert`, Node `mysql2/promise`) — cegah SQL Injection.
- **Validasi harga/stok di app + DB** — defense in depth.
- **Jangan insert `id` manual** — biarkan `AUTO_INCREMENT`.

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **Column count doesn't match** | `INSERT VALUES` tanpa kolom, jumlah tidak pas | Selalu `INSERT INTO t (a,b) VALUES (1,2)` |
| **Duplicate entry for key 'PRIMARY/UNIQUE'** | Insert `id`/`isbn` yang sudah ada | Pakai `ON DUPLICATE KEY UPDATE` atau cek dulu |
| **Incorrect integer value / Data truncated** | Tipe salah (`harga='murah'`) | Validasi tipe di app, MySQL strict mode ON |
| **Tidak pakai transaksi untuk bulk** | Setengah masuk, setengah gagal → data timpang | `START TRANSACTION; ... COMMIT;` |
| **N+1 Insert (loop 1000x query)** | Lambat (1000 round-trip) | Bulk `VALUES (...),(...)` atau `LOAD DATA` |
| **SQL Injection via string concat** | `'; DROP TABLE buku; --` | Prepared statement `?` placeholder |

**Troubleshooting:**

```sql
-- Cek mode SQL (strict = tolak data jelek)
SELECT @@sql_mode;
-- Harus ada STRICT_TRANS_TABLES

-- Lihat warning setelah INSERT IGNORE
SHOW WARNINGS;

-- Cek baris yang gagal
SELECT * FROM buku WHERE isbn = '9780132350884';

-- Performance: berapa lama insert 1000 baris?
SET profiling = 1;
INSERT INTO buku (...) VALUES (...), (...); -- bulk
SHOW PROFILES;
```

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: Bulk Insert & Verifikasi

> Insert 5 buku baru sekaligus, lalu tampilkan `id, judul, stok` yang baru saja dimasukkan. Pastikan `isbn` unik.

**Jawaban:**

```sql
INSERT INTO buku (judul, pengarang, tahun, harga, stok, kategori, isbn)
VALUES
    ('Atomic Habits', 'James Clear', 2018, 99000.00, 12, 'Umum', '9780735211292'),
    ('Deep Work', 'Cal Newport', 2016, 110000.00, 7, 'Teknologi', '9781455586691'),
    ('Educated', 'Tara Westover', 2018, 120000.00, 5, 'Sejarah', '9780399590504'),
    ('Dune', 'Frank Herbert', 1965, 130000.00, 3, 'Fiksi', '9780441013593'),
    ('Thinking Fast and Slow', 'Daniel Kahneman', 2011, 145000.00, 6, 'Sains', '9780374533557');

-- Verifikasi 5 terakhir
SELECT id, judul, stok, isbn FROM buku ORDER BY id DESC LIMIT 5;

-- Cek duplikat isbn (harus 0)
SELECT isbn, COUNT(*) c FROM buku GROUP BY isbn HAVING c > 1;
```

**Pembahasan**: Bulk insert = 1 statement, 1 parse, 1 transaction — jauh lebih efisien. `ORDER BY id DESC LIMIT 5` ambil yang baru karena `AUTO_INCREMENT` naik.

---

### Tantangan 2: Upsert Stok

> Buku `isbn='9780132350884'` (Clean Code) datang 5 eks lagi. Buat query yang: jika ISBN ada, tambah stok 5, jika belum ada, insert baru dengan stok 5. Cek hasilnya.

**Jawaban:**

```sql
-- Sebelum
SELECT judul, stok FROM buku WHERE isbn='9780132350884'; -- stok 5

INSERT INTO buku (judul, pengarang, tahun, harga, stok, kategori, isbn)
VALUES ('Clean Code', 'Robert C. Martin', 2008, 150000.00, 5, 'Teknologi', '9780132350884')
ON DUPLICATE KEY UPDATE
    stok = stok + VALUES(stok),
    updated_at = NOW();

-- Sesudah
SELECT judul, stok FROM buku WHERE isbn='9780132350884'; -- stok 10
-- Jika ISBN baru, akan jadi INSERT biasa dengan stok 5
```

**Pembahasan**: `ON DUPLICATE KEY UPDATE` manfaatkan `UNIQUE(isbn)`. `VALUES(stok)` ambil nilai yang dicoba di-insert (5). Pola ini ideal untuk sinkronisasi inventory tanpa `SELECT` dulu (hindari race condition). Alternatif MySQL 8: `INSERT ... AS new ON DUPLICATE KEY UPDATE stok = stok + new.stok`.

