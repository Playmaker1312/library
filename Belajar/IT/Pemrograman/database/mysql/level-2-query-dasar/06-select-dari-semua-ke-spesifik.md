# 6. SELECT — Dari Semua ke Spesifik

> **Level 2 - Modul C** | **Durasi**: 2 hari | **Prasyarat**: Level 1 (Tabel & Data)

---

## 1. Penjelasan Singkat & Konsep Dasar

`SELECT` adalah **90% pekerjaan aplikasi** — membaca data. Di Level 1 kamu pakai `SELECT *` untuk lihat semua. Di dunia nyata itu boros: ambil 50 kolom padahal butuh 3.

Inti `SELECT`:

- `SELECT kolom` → pilih kolom yang dibutuhkan (hemat bandwidth & memory)
- `AS` alias → ganti nama output agar lebih readable
- `DISTINCT` → hapus duplikat
- Ekspresi & fungsi → hitung di DB, bukan di aplikasi

**Hubungan dengan materi sebelumnya**: Level 1 mengisi rak (INSERT). Sekarang kamu belajar *cara minta buku secara spesifik* ke pustakawan — bukan "kasih semua buku", tapi "kasih judul & harga saja".

---

## 2. Fungsi & Kegunaan Utama dalam MySQL

| Fitur | Fungsi |
|-------|--------|
| `SELECT col1, col2` | Ambil kolom spesifik — kurangi I/O & network |
| `SELECT ... AS alias` | Rename output untuk API/frontend |
| `DISTINCT` | Dapatkan nilai unik (kategori, pengarang) |
| Ekspresi `harga * stok` | Hitung di DB, kirim hasil jadi |
| Fungsi agregat `COUNT, SUM, AVG` | Statistik tanpa tarik semua baris |
| Fungsi string/date `UPPER, CONCAT, DATEDIFF` | Format data di query |

Tanpa SELECT spesifik: API lambat, memory bengkak, frontend parsing berat.

---

## 3. Contoh SQL Query & Implementasi (Real-world Example)

```sql
USE perpustakaan_db;

-- ─── SELECT * (hindari di production!) ───────────────────────────
SELECT * FROM buku; -- ambil 10 kolom x 15 baris = boros jika cuma butuh 2 kolom

-- ─── SELECT kolom spesifik (SELALU lakukan ini) ──────────────────
SELECT judul, pengarang, harga FROM buku;

-- ─── Alias (AS) ──────────────────────────────────────────────────
SELECT
    judul AS judul_buku,
    pengarang AS nama_pengarang,
    harga AS harga_rupiah,
    stok AS jumlah_stok
FROM buku;

-- ─── Ekspresi ────────────────────────────────────────────────────
SELECT
    judul,
    harga,
    stok,
    harga * stok AS total_nilai_stok,
    harga * 0.9 AS harga_diskon_10_persen,
    ROUND(harga * 0.9, 0) AS harga_diskon_bulat
FROM buku;

-- ─── DISTINCT ────────────────────────────────────────────────────
SELECT DISTINCT kategori FROM buku; -- 5 kategori unik
SELECT DISTINCT pengarang FROM buku ORDER BY pengarang;

-- ─── Fungsi agregat ──────────────────────────────────────────────
SELECT
    COUNT(*) AS total_buku,
    COUNT(DISTINCT kategori) AS jumlah_kategori,
    MIN(harga) AS harga_termurah,
    MAX(harga) AS harga_termahal,
    ROUND(AVG(harga),0) AS rata_rata_harga,
    SUM(stok) AS total_stok,
    SUM(harga * stok) AS total_nilai_inventaris
FROM buku;

-- ─── Fungsi string ───────────────────────────────────────────────
SELECT
    UPPER(judul) AS judul_besar,
    LOWER(pengarang) AS pengarang_kecil,
    LENGTH(judul) AS panjang_judul,
    CONCAT(judul, ' oleh ', pengarang) AS info_lengkap,
    SUBSTRING(judul, 1, 20) AS judul_pendek,
    TRIM(kategori) AS kategori_bersih
FROM buku;

-- ─── Fungsi tanggal ──────────────────────────────────────────────
SELECT
    judul,
    created_at,
    DATE(created_at) AS tanggal_saja,
    YEAR(created_at) AS tahun_dibuat,
    DATEDIFF(NOW(), created_at) AS hari_sejak_dibuat,
    DATE_FORMAT(created_at, '%d/%m/%Y %H:%i') AS format_indonesia
FROM buku;
```

**Skenario Nyata (E-commerce — API Produk):**

```sql
-- API butuh list produk untuk katalog: hanya id, nama, harga, stok
SELECT id, nama, harga, stok, harga * 0.9 AS harga_member
FROM produk
WHERE is_active = 1;

-- Dashboard: statistik toko
SELECT
    COUNT(*) AS total_produk,
    SUM(stok) AS total_unit,
    SUM(harga * stok) AS nilai_gudang
FROM produk;
```

---

## 4. Analogi Sederhana: **Perpustakaan Kota**

| Konsep | Analogi |
|--------|---------|
| `SELECT *` | **"Kasih semua buku + semua info"** — pustakawan angkut 10 kardus, kamu cuma butuh judul |
| `SELECT judul, harga` | **"Kasih judul & harga saja"** — pustakawan tulis di kartu kecil, cepat & ringan |
| `AS alias` | **Label kartu** — ganti "harga" jadi "Harga Rupiah" agar pengunjung paham |
| `DISTINCT kategori` | **"Kategori apa saja yang ada?"** — pustakawan sebut unik, tidak ulang-ulang |
| `harga * stok` | **Pustakawan hitung total nilai** — kamu tidak hitung manual di rumah |

> 💡 **Key Insight**: Pustakawan (MySQL) lebih cepat hitung di gudang daripada kamu bawa semua buku pulang lalu hitung sendiri.

---

## 5. Kapan Harus Digunakan & Kasus Penggunaan (Use Cases)

| Situasi | Query |
|---------|-------|
| API list, tabel frontend | `SELECT id, judul, harga` — jangan `*` |
| Laporan ringkas | `SELECT COUNT(*), AVG(harga)` |
| Dropdown filter | `SELECT DISTINCT kategori` |
| Format display | `CONCAT`, `DATE_FORMAT`, `UPPER` |
| Hitung nilai inventaris | `SELECT SUM(harga*stok)` |

---

## 6. Poin-poin Penting & Best Practices

```sql
-- ✅ DO: sebut kolom, pakai alias jelas
SELECT judul, harga AS harga_rupiah FROM buku;

-- ❌ DON'T: SELECT * di production
-- SELECT * FROM buku; -- jika tabel nambah kolom BLOB, query jadi super lambat!

-- ✅ Hitung di DB jika bisa
SELECT harga * stok AS total FROM buku; -- DB optimized untuk ini

-- ✅ DISTINCT hanya jika butuh
SELECT DISTINCT kategori FROM buku;

-- ✅ Fungsi di SELECT, bukan di WHERE (jika bisa) — agar index kepakai (lihat Level 6)
```

**Best Practices:**
- Selalu explicit kolom — tahan terhadap `ALTER TABLE` tambah kolom.
- Gunakan `AS` untuk alias yang ramah API.
- `COUNT(*)` lebih cepat dari `COUNT(judul)` (hitung baris, bukan cek NULL).
- Format tanggal di DB (`DATE_FORMAT`) atau di app — konsisten satu tempat.

---

## 7. Kesalahan Umum (Common Pitfalls & Anti-patterns)

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| **`SELECT *` di API** | Transfer 10x data, N+1 query, bocor kolom sensitif | Sebut kolom eksplisit |
| **Lupa `DISTINCT`** | Kategori terduplikat di dropdown | `SELECT DISTINCT` |
| **Fungsi di WHERE bikin index tidak kepakai** | `WHERE YEAR(created_at)=2024` → full scan | `WHERE created_at BETWEEN '2024-01-01' AND '2024-12-31'` |
| **Alias tanpa `AS`** | Bingung, typo | Selalu `AS` |
| **`COUNT(kolom)` vs `COUNT(*)` salah paham** | `COUNT(kolom)` skip NULL → hasil beda | Pakai `COUNT(*)` untuk total baris |
| **Tidak pakai `ROUND` untuk harga** | `AVG` hasil 135000.333333 | `ROUND(AVG(harga),0)` |

**Troubleshooting:**

```sql
-- Cek kolom apa yang ada sebelum SELECT
DESCRIBE buku;

-- Cek apakah SELECT * bawa kolom besar
SELECT COLUMN_NAME, DATA_TYPE FROM information_schema.COLUMNS
WHERE TABLE_NAME='buku';
-- Jika ada TEXT/BLOB, jangan SELECT *
```

---

## 8. Soal Latihan & Kunci Jawaban

### Tantangan 1: SELECT Spesifik + Ekspresi

> Tampilkan `judul`, `harga`, `stok`, `total_nilai (harga*stok)`, dan `harga_diskon_15%` untuk semua buku. Urutkan nanti di materi 08, sekarang fokus SELECT.

**Jawaban:**

```sql
SELECT
    judul,
    harga,
    stok,
    harga * stok AS total_nilai,
    ROUND(harga * 0.85, 0) AS harga_diskon_15_persen
FROM buku;
```

**Pembahasan**: Ekspresi di SELECT dieksekusi per baris. `ROUND` untuk harga bulat. Ini pola untuk API katalog yang tampilkan harga member.

---

### Tantangan 2: Statistik & DISTINCT

> Hitung: (a) total buku, (b) jumlah kategori unik, (c) rata-rata harga, (d) total nilai inventaris. Lalu tampilkan daftar kategori unik.

**Jawaban:**

```sql
-- Statistik
SELECT
    COUNT(*) AS total_buku,
    COUNT(DISTINCT kategori) AS kategori_unik,
    ROUND(AVG(harga),0) AS rata_rata_harga,
    SUM(harga * stok) AS total_nilai_inventaris
FROM buku;

-- Daftar kategori
SELECT DISTINCT kategori FROM buku ORDER BY kategori;

-- Bonus: info per kategori (preview GROUP BY Level 3)
SELECT kategori, COUNT(*) AS jml FROM buku GROUP BY kategori;
```

**Pembahasan**: `COUNT(DISTINCT kategori)` = jumlah kategori tanpa GROUP BY. `SUM(harga*stok)` = nilai gudang — metrik penting untuk laporan keuangan perpustakaan/toko.

