# 17. ERD dan Relasi — Visualisasi Desain Database

> **Level 5 - Modul I** | **Durasi**: 2 hari | **Prasyarat**: 16 Normal Form

---

## 1. Penjelasan Singkat & Konsep Dasar

ERD = **peta relasi** antar entitas. Sebelum code, gambar ERD untuk sepakati desain.

Jenis relasi: `1:1` (anggota→profil), `1:N` (pengarang→buku, paling umum), `M:N` (buku↔kategori via pivot), `Self-ref` (kategori parent).

**Hubungan dengan materi sebelumnya**: 16 pecah tabel. 17 visualisasikan pecahan itu agar tim paham tanpa baca SQL.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Relasi | Implementasi |
|--------|--------------|
| 1:1 | FK UNIQUE di salah satu tabel |
| 1:N | FK di sisi N (`buku.pengarang_id`) |
| M:N | Tabel pivot `buku_kategori (buku_id, kategori_id)` PK komposit |
| Self | FK ke tabel sendiri `kategori.parent_id → kategori.id` |

ERD cegah salah desain: lupa pivot → data koma-separated, lupa FK → orphan.

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── 1:N sudah ada: pengarang → buku ─────────────────────────────
-- buku.pengarang_id → pengarang.id

-- ─── M:N: buku ↔ kategori ────────────────────────────────────────
CREATE TABLE kategori (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(50) NOT NULL, slug VARCHAR(50) UNIQUE NOT NULL,
    parent_id INT UNSIGNED NULL,
    FOREIGN KEY (parent_id) REFERENCES kategori(id) ON DELETE SET NULL
) ENGINE=InnoDB;

CREATE TABLE buku_kategori (
    buku_id INT UNSIGNED NOT NULL,
    kategori_id INT UNSIGNED NOT NULL,
    PRIMARY KEY (buku_id, kategori_id),
    FOREIGN KEY (buku_id) REFERENCES buku(id) ON DELETE CASCADE,
    FOREIGN KEY (kategori_id) REFERENCES kategori(id) ON DELETE CASCADE
) ENGINE=InnoDB;

INSERT INTO kategori (nama,slug) VALUES ('Fiksi','fiksi'),('Teknologi','teknologi'),('Programming','programming');
INSERT INTO buku_kategori VALUES (1,2),(1,3); -- Clean Code: Teknologi + Programming

-- Query M:N
SELECT b.judul, GROUP_CONCAT(k.nama SEPARATOR ', ') AS kategori
FROM buku b LEFT JOIN buku_kategori bk ON b.id=bk.buku_id LEFT JOIN kategori k ON bk.kategori_id=k.id
GROUP BY b.id;

-- ─── 1:1: anggota → profil ───────────────────────────────────────
CREATE TABLE profil_anggota (
    anggota_id INT UNSIGNED PRIMARY KEY,
    alamat TEXT, tanggal_lahir DATE,
    FOREIGN KEY (anggota_id) REFERENCES anggota(id) ON DELETE CASCADE
) ENGINE=InnoDB;
-- PRIMARY KEY = FK → jamin 1:1

-- ─── ERD Text ────────────────────────────────────────────────────
-- pengarang 1──< buku >── buku_kategori >── kategori
-- kategori 1──< kategori (self)
-- anggota 1──< peminjaman >── buku
-- anggota 1──1 profil_anggota
```

**Skenario Nyata (E-commerce):** `produk M:N tag` via `produk_tag`, `user 1:1 profile`, `kategori self parent`.

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| **1:N** | **Satu pengarang banyak buku** — satu kartu pengarang → banyak buku di rak |
| **M:N** | **Buku banyak kategori, kategori banyak buku** — butuh *daftar silang* (pivot) |
| **1:1** | **Satu anggota satu profil** — kartu anggota + lembar profil (1-1) |
| **ERD** | **Denah gedung** — gambar rak & jalur penghubung, bukan tumpukan buku |

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | Relasi |
|---------|--------|
| User–Orders | 1:N |
| Buku–Kategori | M:N pivot |
| Kategori hierarki | Self-ref |
| Anggota–Profil | 1:1 |

---

## 6. Poin-poin Penting & Best Practices

- Gambar ERD dulu (Workbench, dbdiagram.io, draw.io) sebelum CREATE.
- Pivot PK komposit cegah duplikat.
- `ON DELETE CASCADE` untuk pivot, `RESTRICT` untuk master.

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **M:N tanpa pivot** | CSV kolom | Pivot |
| **1:1 pakai FK biasa** | Bisa 1:N tidak sengaja | FK + UNIQUE / PK=FK |
| **ERD tidak diupdate** | Code & diagram beda | ERD = source of truth |

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: M:N Query

> Tampilkan buku + daftar kategorinya.

**Jawaban:** Lihat query `GROUP_CONCAT` di atas.

### Tantangan 2: Self-ref

> Tampilkan kategori + parent-nya.

**Jawaban:**

```sql
SELECT c.nama AS kategori, p.nama AS parent FROM kategori c LEFT JOIN kategori p ON c.parent_id=p.id;
```

