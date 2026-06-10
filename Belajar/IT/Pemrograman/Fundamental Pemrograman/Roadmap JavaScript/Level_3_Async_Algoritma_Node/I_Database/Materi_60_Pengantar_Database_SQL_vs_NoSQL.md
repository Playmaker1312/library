# Materi 60: Pengantar Database — SQL vs NoSQL

---

## 1. Penjelasan

Database adalah sistem terpusat untuk menyimpan, mengelola, dan mengambil data secara efisien. Bayangkan Anda punya 1000 data anggota perpustakaan dalam file JSON — mencari satu anggota butuh baca seluruh file. Database menyelesaikan ini dengan indeks, query language, dan konkurensi.

### SQL (Relational Database)
- Data terstruktur dalam tabel (baris & kolom).
- Schema tetap (harus ditentukan di awal).
- Mendukung **ACID** (Atomicity, Consistency, Isolation, Durability).
- Contoh: PostgreSQL, MySQL, SQLite.

### NoSQL (Non-Relational Database)
- Schema fleksibel (dokumen bisa berbeda struktur).
- Berbagai model: document (MongoDB), key-value (Redis), graph (Neo4j), column-family (Cassandra).
- Skalabilitas horizontal lebih mudah.
- Contoh: MongoDB, Firestore, Redis.

### Kapan Pilih?
| Situasi | SQL | NoSQL |
|---------|-----|-------|
| Data relasional (user → order → product) | ✅ | ❌ |
| Butuh transaksi kompleks / ACID | ✅ | ❌ (sebagian) |
| Schema sering berubah | ❌ | ✅ |
| Data non-struktural / dokumen | ❌ | ✅ |
| Join antar data | ✅ (native) | ❌ (manual) |
| Skala horizontal besar | ❌ (vertikal) | ✅ |

---

## 2. Fungsi

- **SQL**: Menyimpan data dengan relasi ketat, transaksi aman, query kompleks.
- **NoSQL**: Menyimpan data fleksibel, prototipe cepat, volume besar.
- **Keduanya**: CRUD (Create, Read, Update, Delete), indexing, backup/recovery.

---

## 3. Code

Rancangan skema database perpustakaan (SQL):

```sql
-- Skema Database Perpustakaan (SQL)

-- Tabel Buku
CREATE TABLE buku (
    id         INT PRIMARY KEY AUTO_INCREMENT,
    judul      VARCHAR(200) NOT NULL,
    penulis    VARCHAR(100) NOT NULL,
    isbn       VARCHAR(20) UNIQUE,
    tahun      INT,
    stok       INT DEFAULT 1
);

-- Tabel Anggota
CREATE TABLE anggota (
    id          INT PRIMARY KEY AUTO_INCREMENT,
    nama        VARCHAR(100) NOT NULL,
    email       VARCHAR(100) UNIQUE NOT NULL,
    no_telepon  VARCHAR(20),
    tanggal_daftar DATE DEFAULT CURRENT_DATE
);

-- Tabel Peminjaman
CREATE TABLE peminjaman (
    id            INT PRIMARY KEY AUTO_INCREMENT,
    buku_id       INT NOT NULL,
    anggota_id    INT NOT NULL,
    tanggal_pinjam DATE DEFAULT CURRENT_DATE,
    tanggal_kembali DATE,
    status        ENUM('dipinjam', 'dikembalikan') DEFAULT 'dipinjam',
    FOREIGN KEY (buku_id) REFERENCES buku(id),
    FOREIGN KEY (anggota_id) REFERENCES anggota(id)
);

-- Diagram relasi (dalam komentar):
--
--  [buku] 1 --- M [peminjaman] M --- 1 [anggota]
--    |                                    |
--    +--------(buku_id)-------(anggota_id)-+
--
-- Relasi: buku 1:M peminjaman : M:1 anggota
```

---

## 4. Analogi Rumah

| Konsep Database | Analogi Rumah |
|----------------|---------------|
| SQL Database | Gudang dengan rak berlabel rapi — setiap rak punya kategori, ukuran, dan aturan isi yang tetap. |
| NoSQL Database | Gudang dengan kotak besar isi barang campur — tiap kotak bisa berisi apa saja, bebas, tapi butuh effort lebih saat mencari barang spesifik. |
| Tabel / Collection | Sebuah rak / lemari khusus. |
| Baris / Dokumen | Satu barang di dalam rak/kotak. |
| Kolom / Field | Label properti barang (warna, berat, harga). |
| Primary Key | Nomor inventaris unik tiap barang. |
| Foreign Key | Tanda "barang ini milik ruangan X" yang tertera di stiker. |
| Query | Aktivitas mencari barang di gudang dengan daftar permintaan. |
| Index | Papan indeks di pintu gudang: "Rak A: cat, Rak B: paku". |

---

## 5. Use Case

| Use Case | Pilihan | Alasan |
|----------|---------|--------|
| Sistem perpustakaan | SQL | Relasi buku-anggota-peminjaman kuat, butuh transaksi. |
| Log event real-time | NoSQL | Volume besar, schema dinamis, write-heavy. |
| E-commerce (keranjang, order) | SQL | Transaksi pembayaran butuh ACID. |
| CMS/blog | NoSQL | Posting bisa berbeda struktur tiap artikel. |
| Chat/messaging | NoSQL | Data tidak terstruktur, skalabilitas tinggi. |

---

## 6. Kesalahan Umum

1. **Memilih NoSQL untuk data relasional** — Akhirnya implementasi join manual di kode, repot dan rawan bug.
2. **Memilih SQL untuk log/data sementara** — Overhead schema dan migrasi tidak sebanding dengan kegunaan.
3. **Tidak menentukan indeks** — Query lambat karena full table scan.
4. **Normalisasi berlebihan** — Terlalu banyak tabel kecil sehingga query jadi 8 JOIN.
5. **Denormalisasi tanpa strategi** — Duplikasi data tidak konsisten.

---

## 7. Benang Merah

```
Level 1-2      →  Materi 59 (Environment Variables)
                       ↓
                  Butuh penyimpanan data yang andal
                       ↓
    ┌─── *Materi 60: Pengantar Database (SQL vs NoSQL)* ←──┐
    │                                                       │
    ↓ Teori database                                        │ Pilihan arsitektur
    Materi 61: SQL DDL & DML (konkret)                      │
    ↓                                                       │
    Materi 62: JOIN, Agregasi, Subquery                     │
    ↓                                                       │
    Materi 63: PostgreSQL Setup                             │
    ↓                                                       │
    Materi 64: Prisma ORM                                   │
    ↓                                                       │
    Materi 65: Express + Prisma + PostgreSQL ───────────────┘
                       ↓
              Level 3 Puncak Backend
```

---

## 8. Soal & Jawaban

### Soal 1
**Mengapa sistem perpustakaan lebih cocok menggunakan SQL daripada NoSQL? Sebutkan minimal 2 alasan.**

<details>
<summary>Jawaban</summary>
1. Data sangat relasional — buku berelasi dengan peminjaman, peminjaman dengan anggota. SQL memiliki JOIN native untuk ini.
2. Butuh ACID — saat meminjam buku, stok harus berkurang dan record peminjaman harus tercatat dalam satu transaksi atomic. NoSQL tidak menjamin ini secara default.
</details>

### Soal 2
**Jika Anda membangun sistem log jutaan request per detik untuk aplikasi chat, database mana yang Anda pilih? Jelaskan.**

<details>
<summary>Jawaban</summary>
NoSQL (misal MongoDB atau Cassandra). Alasannya: volume data sangat besar (write-heavy), schema log bisa berbeda-beda (timestamp, user_id, message, metadata), skalabilitas horizontal lebih mudah dengan sharding, dan tidak butuh relasi kompleks atau ACID untuk log.
</details>

### Soal 3
**Apa fungsi PRIMARY KEY dan FOREIGN KEY dalam konteks analogi gudang/rumah?**

<details>
<summary>Jawaban</summary>
- PRIMARY KEY = Nomor inventaris unik tiap barang. Tidak ada dua barang dengan nomor yang sama, dan setiap barang pasti punya nomor.
- FOREIGN KEY = Stiker yang bertuliskan "barang ini milik ruangan A-123". Stiker ini memastikan barang selalu terhubung ke ruangan yang valid (referential integrity).
</details>
