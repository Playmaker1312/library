# 12. CTE (Common Table Expression) — Query Lebih Readable

> **Level 3 - Modul F** | **Durasi**: 2 hari | **Prasyarat**: 11 Subquery | **MySQL 8.0+**

---

## 1. Penjelasan Singkat & Konsep Dasar

CTE = **beri nama pada subquery** via `WITH`. Sama hasilnya dengan subquery, tapi **jauh lebih readable** dan bisa dipakai berulang.

```sql
WITH stat AS (SELECT kategori, AVG(harga) AS avg_h FROM buku GROUP BY kategori)
SELECT * FROM stat WHERE avg_h > 150000;
```

**Hubungan dengan materi sebelumnya**: 11 subquery bersarang bikin pusing `SELECT FROM (SELECT FROM (SELECT...))`. CTE pecah jadi langkah bernama yang mengalir top-to-bottom.

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Fitur | Fungsi |
|-------|--------|
| `WITH cte AS (SELECT ...)` | Nama untuk subquery, reusable |
| Multiple CTE `WITH a AS (...), b AS (...)` | Pipeline bertahap |
| `WITH RECURSIVE` | Data hierarki (kategori parent→child) |

CTE tidak selalu lebih cepat, tapi **lebih maintainable** — penting untuk laporan kompleks.

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── CTE Dasar ───────────────────────────────────────────────────
WITH statistik_kategori AS (
    SELECT kategori, COUNT(*) AS jml, ROUND(AVG(harga),0) AS avg_harga, SUM(stok) AS stok FROM buku GROUP BY kategori
)
SELECT kategori, jml, avg_harga,
       CASE WHEN avg_harga>150000 THEN 'Premium' WHEN avg_harga>100000 THEN 'Menengah' ELSE 'Ekonomis' END AS segmen
FROM statistik_kategori ORDER BY avg_harga DESC;

-- ─── Multiple CTE (pipeline) ─────────────────────────────────────
WITH
buku_tersedia AS (SELECT * FROM buku WHERE stok>0 AND deleted_at IS NULL),
statistik AS (SELECT kategori, COUNT(*) AS jml, ROUND(AVG(harga),0) AS avg_h FROM buku_tersedia GROUP BY kategori),
kategori_populer AS (SELECT kategori FROM statistik WHERE jml>=2)
SELECT s.* FROM statistik s JOIN kategori_populer kp ON s.kategori=kp.kategori ORDER BY s.avg_h DESC;

-- ─── CTE vs Subquery: Top N per kategori (preview Window) ────────
WITH ranked AS (
    SELECT kategori, judul, harga, ROW_NUMBER() OVER (PARTITION BY kategori ORDER BY harga DESC) AS rn FROM buku
)
SELECT kategori, judul, harga FROM ranked WHERE rn <=2 ORDER BY kategori, harga DESC;

-- ─── RECURSIVE: hierarki kategori ────────────────────────────────
CREATE TABLE kategori (id INT PRIMARY KEY, nama VARCHAR(50), parent_id INT NULL, FOREIGN KEY (parent_id) REFERENCES kategori(id));
INSERT INTO kategori VALUES (1,'Buku',NULL),(2,'Fiksi',1),(3,'Non-Fiksi',1),(4,'Sains',3),(5,'Teknologi',3);

WITH RECURSIVE kategori_tree AS (
    SELECT id, nama, parent_id, 0 AS level, CAST(nama AS CHAR(200)) AS path FROM kategori WHERE parent_id IS NULL
    UNION ALL
    SELECT k.id, k.nama, k.parent_id, kt.level+1, CONCAT(kt.path,' → ',k.nama)
    FROM kategori k JOIN kategori_tree kt ON k.parent_id=kt.id
)
SELECT * FROM kategori_tree ORDER BY path;
```

**Skenario Nyata (E-commerce — Laporan Bertahap):**

```sql
WITH
orders_paid AS (SELECT * FROM orders WHERE status='paid'),
revenue_per_cat AS (SELECT kategori_id, SUM(total) AS omzet FROM orders_paid GROUP BY kategori_id)
SELECT k.nama, r.omzet FROM revenue_per_cat r JOIN kategori k ON r.kategori_id=k.id ORDER BY omzet DESC;
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| `WITH statistik AS (...)` | **Buat daftar ringkasan bernama "Statistik"** — tempel di papan, lalu rujuk berulang |
| Multiple CTE | **Pipeline**: Saring buku tersedia → hitung statistik → saring populer |
| `RECURSIVE` | **Telusuri rak hierarki**: Rak Buku → Non-Fiksi → Sains → Fisika |

> 💡 **Key Insight**: CTE = *beri nama langkah*. Daripada tanya bersarang, buat daftar bernama dulu.

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | Pakai |
|---------|-------|
| Query >2 subquery bersarang | CTE |
| Butuh pakai hasil subquery 2x | CTE (tidak ulang tulis) |
| Hierarki (org chart, kategori) | `WITH RECURSIVE` |
| Laporan pipeline | Multiple CTE |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ CTE reusable — tidak perlu copy-paste subquery
WITH s AS (SELECT ...) SELECT * FROM s WHERE ... UNION ALL SELECT * FROM s WHERE ...

-- ✅ Nama CTE deskriptif: statistik_kategori, buku_tersedia
-- ❌ CTE bukan optimasi otomatis — MySQL materialize, kadang JOIN lebih cepat → EXPLAIN
```

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **Lupa `RECURSIVE` keyword** | Error recursive | `WITH RECURSIVE` |
| **CTE dianggap lebih cepat selalu** | Kadang sama/lambat | EXPLAIN, bandingkan dengan subquery |
| **Infinite recursion** | Loop parent→child cycle | Pastikan base case + `WHERE parent_id IS NULL` |

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: CTE Laporan

> Buat CTE `statistik` (kategori, jml, avg_harga), lalu tampilkan hanya Premium (avg>150k).

**Jawaban:**

```sql
WITH statistik AS (
    SELECT kategori, COUNT(*) AS jml, ROUND(AVG(harga),0) AS avg_harga FROM buku GROUP BY kategori
)
SELECT kategori, jml, avg_harga FROM statistik WHERE avg_harga>150000 ORDER BY avg_harga DESC;
```

### Tantangan 2: RECURSIVE Path

> Dengan tabel `kategori` hierarki, tampilkan `path` lengkap.

**Jawaban:**

```sql
WITH RECURSIVE tree AS (
    SELECT id, nama, parent_id, CAST(nama AS CHAR(200)) AS path FROM kategori WHERE parent_id IS NULL
    UNION ALL
    SELECT k.id, k.nama, k.parent_id, CONCAT(tree.path,' → ',k.nama) FROM kategori k JOIN tree ON k.parent_id=tree.id
)
SELECT id, path FROM tree ORDER BY path;
```

**Pembahasan**: Base case ambil root, recursive join anak. Path akumulasi string.

