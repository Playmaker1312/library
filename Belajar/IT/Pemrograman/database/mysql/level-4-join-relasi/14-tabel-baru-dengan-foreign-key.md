# 14. Tabel Baru dengan Foreign Key

> **Level 4 - Modul H** | **Durasi**: 2 hari | **Prasyarat**: Level 3, 03 Membuat Tabel

---

## 1. Penjelasan Singkat & Konsep Dasar

Satu tabel `buku` tidak cukup untuk dunia nyata. Perpustakaan butuh `anggota` dan `peminjaman`. **Foreign Key (FK)** = *jaminan* bahwa `peminjaman.buku_id` harus ada di `buku.id`. Tanpa FK, bisa pinjam buku yang tidak ada (orphan data).

FK + `ON UPDATE/DELETE` atur apa terjadi jika data induk berubah/dihapus: `CASCADE`, `RESTRICT`, `SET NULL`.

**Hubungan dengan materi sebelumnya**: Level 1-3 satu rak (buku). Level 4 bangun *rak baru* yang saling terhubung — sistem relasional sejati.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Fitur | Fungsi |
|-------|--------|
| `FOREIGN KEY (col) REFERENCES parent(id)` | Jaga integritas referensial — tolak data yatim |
| `ON UPDATE CASCADE` | Jika PK induk berubah, FK ikut berubah |
| `ON DELETE RESTRICT` | Tolak hapus induk jika masih dipakai anak |
| `ON DELETE CASCADE` | Hapus anak jika induk dihapus (hati-hati!) |
| `ON DELETE SET NULL` | Set FK jadi NULL jika induk dihapus |
| `INDEX` otomatis di FK | Percepat JOIN & cek FK |

Tanpa FK: data tidak konsisten, JOIN hasilkan NULL, laporan salah.

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── Tabel Anggota ───────────────────────────────────────────────
CREATE TABLE anggota (
    id              INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nomor_anggota   VARCHAR(20) UNIQUE NOT NULL,
    nama            VARCHAR(100) NOT NULL,
    email           VARCHAR(150) UNIQUE NOT NULL,
    telepon         VARCHAR(15),
    tanggal_daftar  DATE NOT NULL,
    status          ENUM('aktif','nonaktif','suspended') DEFAULT 'aktif',
    created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX idx_nama (nama),
    INDEX idx_status (status)
) ENGINE=InnoDB;

-- ─── Tabel Peminjaman ────────────────────────────────────────────
CREATE TABLE peminjaman (
    id              INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    buku_id         INT UNSIGNED NOT NULL,
    anggota_id      INT UNSIGNED NOT NULL,
    tanggal_pinjam  DATE NOT NULL,
    batas_kembali   DATE NOT NULL,
    tanggal_kembali DATE NULL,
    denda           DECIMAL(10,2) DEFAULT 0.00,
    status          ENUM('dipinjam','dikembalikan','terlambat') DEFAULT 'dipinjam',
    created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (buku_id) REFERENCES buku(id) ON UPDATE CASCADE ON DELETE RESTRICT,
    FOREIGN KEY (anggota_id) REFERENCES anggota(id) ON UPDATE CASCADE ON DELETE RESTRICT,
    INDEX idx_anggota_status (anggota_id, status),
    INDEX idx_tanggal_pinjam (tanggal_pinjam)
) ENGINE=InnoDB;

-- ─── Insert data ─────────────────────────────────────────────────
INSERT INTO anggota (nomor_anggota, nama, email, telepon, tanggal_daftar) VALUES
    ('ANG-2024-001','Budi Santoso','budi@email.com','081234567890','2024-01-15'),
    ('ANG-2024-002','Siti Rahayu','siti@email.com','081234567891','2024-02-20'),
    ('ANG-2024-003','Andi Wijaya','andi@email.com','081234567892','2024-03-10');

INSERT INTO peminjaman (buku_id, anggota_id, tanggal_pinjam, batas_kembali, status) VALUES
    (1,1,'2024-06-01','2024-06-15','dikembalikan'),
    (2,2,'2024-06-05','2024-06-19','dipinjam'),
    (3,1,'2024-06-10','2024-06-24','dipinjam');

-- ─── Test FK ─────────────────────────────────────────────────────
INSERT INTO peminjaman (buku_id, anggota_id, tanggal_pinjam, batas_kembali) VALUES (999,1,'2024-06-20','2024-07-04');
-- ERROR 1452: Cannot add or update a child row: a foreign key constraint fails

DELETE FROM buku WHERE id=1;
-- ERROR 1451: Cannot delete or update a parent row: a foreign key constraint fails (RESTRICT)

-- ─── Alter FK ────────────────────────────────────────────────────
-- Lihat FK
SELECT CONSTRAINT_NAME, TABLE_NAME, REFERENCED_TABLE_NAME FROM information_schema.KEY_COLUMN_USAGE
WHERE TABLE_SCHEMA='perpustakaan_db' AND REFERENCED_TABLE_NAME IS NOT NULL;
```

**Skenario Nyata (E-commerce):**

```sql
CREATE TABLE orders (
    id BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    user_id INT UNSIGNED NOT NULL,
    produk_id INT UNSIGNED NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE RESTRICT,
    FOREIGN KEY (produk_id) REFERENCES produk(id) ON DELETE RESTRICT
);
-- RESTRICT cegah hapus user/produk yang masih ada order — jaga histori!
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| `Tabel anggota` | **Rak kartu anggota** — tiap kartu punya nomor unik |
| `Tabel peminjaman` | **Buku catatan peminjaman** — tiap baris tulis "Buku X dipinjam Anggota Y" |
| `FK buku_id → buku.id` | **Cap "Buku harus ada di rak"** — tidak bisa pinjam buku hantu |
| `ON DELETE RESTRICT` | **"Tidak bisa buang rak buku jika masih dipinjam"** — tolak! |
| `ON DELETE CASCADE` | **"Buang rak → buang catatan pinjamnya juga"** — berbahaya! |
| `ON UPDATE CASCADE` | **Ganti nomor inventaris → catatan pinjam ikut update** |

> 💡 **Key Insight**: FK = *pustakawan penjaga* — dia cek tiap catatan, pastikan buku & anggota nyata.

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | ON DELETE |
|---------|-----------|
| `peminjaman → buku` | `RESTRICT` (jangan hapus buku yang dipinjam) |
| `order → user` | `RESTRICT` (jaga histori) |
| `buku_kategori pivot` | `CASCADE` (hapus buku → hapus relasi) |
| `kategori parent` | `SET NULL` (hapus parent → anak jadi root) |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ Selalu InnoDB (MyISAM tidak support FK!)
ENGINE=InnoDB

-- ✅ Tipe FK harus persis sama: INT UNSIGNED → INT UNSIGNED
-- ❌ INT → BIGINT = error

-- ✅ Beri index di FK (MySQL auto, tapi explicit lebih jelas)
INDEX idx_buku (buku_id)

-- ✅ Gunakan RESTRICT untuk data penting, CASCADE untuk pivot
-- ✅ Cek FK sebelum hapus
SELECT * FROM peminjaman WHERE buku_id=1; -- jika ada, jangan DELETE buku
```

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **Engine MyISAM** | FK diabaikan! | `ENGINE=InnoDB` |
| **Tipe tidak sama** | `ERROR 1215: Cannot add foreign key` | Samakan `INT UNSIGNED` |
| **`ON DELETE CASCADE` sembarangan** | Hapus 1 buku → hapus 1000 peminjaman! | Pakai `RESTRICT` untuk master |
| **Lupa index FK** | JOIN lambat | Index `FK` |
| **Hapus induk tanpa cek** | Error 1451 | Cek anak dulu, atau soft delete |

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: Buat Tabel + Test FK

> Buat `anggota` & `peminjaman` sesuai skema, insert 2 anggota & 1 peminjaman, lalu coba insert `buku_id=999` (harus gagal).

**Jawaban:**

```sql
CREATE TABLE anggota (id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY, nomor_anggota VARCHAR(20) UNIQUE, nama VARCHAR(100) NOT NULL) ENGINE=InnoDB;
CREATE TABLE peminjaman (id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY, buku_id INT UNSIGNED NOT NULL, anggota_id INT UNSIGNED NOT NULL, FOREIGN KEY (buku_id) REFERENCES buku(id) ON DELETE RESTRICT) ENGINE=InnoDB;
INSERT INTO anggota VALUES (1,'ANG-001','Budi'),(2,'ANG-002','Siti');
INSERT INTO peminjaman (buku_id, anggota_id) VALUES (1,1); -- ok
INSERT INTO peminjaman (buku_id, anggota_id) VALUES (999,1); -- ERROR 1452
```

### Tantangan 2: Pilih ON DELETE

> Untuk `buku_kategori` pivot (M:N), apa ON DELETE yang tepat? Jelaskan.

**Jawaban:**

```sql
CREATE TABLE buku_kategori (
    buku_id INT UNSIGNED NOT NULL,
    kategori_id INT UNSIGNED NOT NULL,
    PRIMARY KEY (buku_id,kategori_id),
    FOREIGN KEY (buku_id) REFERENCES buku(id) ON DELETE CASCADE,
    FOREIGN KEY (kategori_id) REFERENCES kategori(id) ON DELETE CASCADE
);
-- CASCADE tepat: hapus buku → relasinya tidak relevan lagi, hapus baris pivot.
-- Beda dengan peminjaman (histori) → RESTRICT.
```

