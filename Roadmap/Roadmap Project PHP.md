# Roadmap PHP Native: Step-by-Step Membangun Aplikasi Web Nyata

## Filosofi Roadmap Ini

> **"PHP bukan bahasa kuno yang harus dihindari — PHP adalah bahasa yang telah berevolusi menjadi modern, ekspresif, dan powerful. Memahami PHP native sebelum framework adalah investasi terbaik seorang web developer"** — setiap konsep yang dipelajari ada alasannya, bukan sekadar hafal sintaks.

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: sistem manajemen perpustakaan dari halaman statis → dinamis → database → API → aplikasi penuh
- **Security dari Hari Pertama**: bukan topik lanjutan, tapi mindset yang dibangun sejak baris pertama kode
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **PHP Modern**: fokus pada PHP 8.x features, bukan cara lama yang sudah deprecated
- **Mengapa sebelum Bagaimana**: pahami alasan di balik setiap keputusan desain

### Prasyarat Sebelum Memulai

text

```
Sebelum roadmap ini, pastikan sudah memahami:
├── HTML dasar (tag, atribut, form)
├── CSS dasar (selector, box model)
├── Cara menggunakan text editor (VS Code direkomendasikan)
├── Konsep dasar internet (request, response, URL, browser)
├── Command line dasar (cd, ls/dir, mkdir)
└── Git version control dasar (init, add, commit, push)
```

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: "PHP Pertama" — cara kerja, setup, sintaks dasar, output ke browser
    ↓ (enhance)
Level 2: + String, array, fungsi → Katalog buku di memori
    ↓ (enhance)
Level 3: + Form, session, file → Web app perpustakaan interaktif
    ↓ (enhance)
Level 4: + OOP, PDO, MySQL → Sistem perpustakaan dengan database
    ↓ (enhance)
Level 5: + Security, validasi, autentikasi → Aplikasi yang aman
    ↓ (enhance)
Level 6: + Arsitektur MVC, REST API → Aplikasi terstruktur dan scalable
    ↓ (enhance)
Level 7: + Testing, optimasi, deployment → Production-ready
```

---

## 🟢 LEVEL 1: FONDASI PHP (Minggu 1-4)

> **Tema**: _"Dari nol ke halaman PHP pertama yang berjalan di server"_  
> **Benang Merah**: Cara PHP bekerja → Setup → Sintaks dasar → Variabel → Tipe data → Output pertama  
> **Output**: Halaman web dinamis yang menampilkan informasi perpustakaan dengan PHP

---

### A. Cara PHP Bekerja dan Setup

> 💡 **Mengapa dimulai di sini?** Berbeda dengan JavaScript yang berjalan di browser, PHP berjalan di server. Memahami siklus request-response ini mencegah kebingungan "kenapa kode PHP muncul di halaman?" atau "kenapa perubahan tidak langsung terlihat?"

text

```
Benang Merah Bagian A:
HTML dan CSS sudah dipahami (prasyarat) →
PHP: bahasa yang berjalan di SERVER, bukan di browser →
Browser kirim request → server proses PHP → kirim HTML ke browser →
Browser tidak pernah melihat kode PHP, hanya hasilnya →
Setup environment lokal → file .php pertama → lihat di browser
```

#### [[Belajar/IT/Pemrograman/project/Project PHP Native/Level 1 - Fondasi PHP/1. PHP adalah Server-Side — Cara Kerja yang Fundamental]]

- PHP dieksekusi di server, hasilnya berupa HTML yang dikirim ke browser
- Browser tidak pernah melihat kode PHP — hanya output HTML-nya

text

```
Alur lengkap setiap request:

Browser                    Web Server              PHP Engine
   │                           │                       │
   │── GET /index.php ─────────▶│                       │
   │                           │── jalankan index.php ─▶│
   │                           │                       │── baca kode PHP
   │                           │                       │── eksekusi logika
   │                           │                       │── hasilkan HTML
   │                           │◀── kirim HTML ─────────│
   │◀── terima HTML ───────────│                       │
   │                           │                       │
   │ (render HTML di browser)  │                       │
```

- **Perbedaan dengan JavaScript:**

text

```
PHP (Server-Side)              JavaScript (Client-Side)
──────────────────────────     ──────────────────────────
Berjalan di SERVER             Berjalan di BROWSER
User tidak bisa lihat          User bisa lihat kodenya
Akses ke database langsung     Perlu API untuk akses DB
Setiap request = eksekusi baru Tetap berjalan setelah load
File .php di server            File .js dikirim ke browser
```

- _Langkah konkret_: Buka `view-source:https://wikipedia.org` di browser — kamu tidak akan melihat kode PHP, hanya HTML yang sudah dihasilkan

#### [[2. Setup Environment — Laragon untuk Windows, Herd untuk macOS]]

text

```
Tools yang dibutuhkan:
├── PHP Engine: yang memproses file .php
├── Web Server: Apache atau Nginx (terima request dari browser)
├── MySQL: database (dibutuhkan di Level 4+)
└── phpMyAdmin: GUI untuk kelola database (opsional)

Pilihan setup (pilih satu):
├── Laragon (Windows) — REKOMENDASI untuk Windows
│   ├── Download: laragon.org
│   ├── Install → Start All
│   ├── Buat folder di: C:\laragon\www\perpustakaan\
│   └── Akses: http://perpustakaan.test (auto-detect!)
│
├── XAMPP (Windows/macOS/Linux) — paling universal
│   ├── Download: apachefriends.org
│   ├── Install → buka XAMPP Control Panel
│   ├── Start: Apache + MySQL
│   ├── Buat folder di: C:\xampp\htdocs\perpustakaan\
│   └── Akses: http://localhost/perpustakaan/
│
└── Herd (macOS) — paling mudah untuk macOS
    ├── Download: herd.laravel.com
    ├── Install → drag ke Applications
    ├── Buat folder di: ~/Herd/perpustakaan/
    └── Akses: http://perpustakaan.test (auto!)
```

Bash

```
# Verifikasi PHP terinstall dengan benar
# Buka terminal/command prompt:
php --version
# PHP 8.3.x (cli) ... ← harus muncul versi 8.x

# Cek ekstensi PHP yang aktif
php -m
# Harus ada: pdo, pdo_mysql, mbstring, json, openssl
```

#### [[3. File .php Pertama dan Cara Menjalankannya]]

PHP

```
<?php
// File: perpustakaan/index.php

// PHP tag pembuka — WAJIB ada di setiap file PHP
// PHP tag penutup (?>) — opsional jika file murni PHP, sebaiknya DIHILANGKAN
// (tanpa closing tag, menghindari "headers already sent" error)

// Aktifkan error reporting untuk development
error_reporting(E_ALL);
ini_set('display_errors', '1');

// Output pertama
echo "Selamat datang di Perpustakaan Digital!";
echo "<br>";
echo "PHP Version: " . PHP_VERSION;
echo "<br>";
echo "Tanggal hari ini: " . date('d F Y');
echo "<br>";
echo "Waktu server: " . date('H:i:s');
```

PHP

```
<?php
// File: perpustakaan/profil.php
// PHP + HTML yang benar — template yang akan terus dipakai

// Logika PHP di atas, sebelum HTML
$namaPerpustakaan = "Perpustakaan Digital Kota";
$jumlahKoleksi    = 12500;
$tahunBerdiri     = 1995;
$jamBuka          = "Senin-Sabtu: 08:00-17:00";
$alamat           = "Jl. Merdeka No. 1, Kota Bandung";
?>
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><?= htmlspecialchars($namaPerpustakaan) ?> — Profil</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
    <header>
        <h1><?= htmlspecialchars($namaPerpustakaan) ?></h1>
    </header>

    <main>
        <section>
            <h2>Tentang Kami</h2>
            <p>Berdiri sejak tahun <strong><?= $tahunBerdiri ?></strong>,
               kami memiliki koleksi lebih dari
               <strong><?= number_format($jumlahKoleksi, 0, ',', '.') ?></strong> buku.</p>
        </section>

        <section>
            <h2>Informasi</h2>
            <dl>
                <dt>Jam Operasional</dt>
                <dd><?= htmlspecialchars($jamBuka) ?></dd>

                <dt>Alamat</dt>
                <dd><?= htmlspecialchars($alamat) ?></dd>
            </dl>
        </section>
    </main>
</body>
</html>
```

- _Langkah konkret_: Buat kedua file di atas, buka di browser — pastikan variabel PHP muncul dengan benar di halaman HTML

#### [[4. Error Reporting — Baca Error, Jangan Takut]]

PHP

```
<?php
// File: perpustakaan/config/development.php
// Include file ini di awal SETIAP file saat development

// Tampilkan SEMUA error dan warning
error_reporting(E_ALL);
ini_set('display_errors', '1');
ini_set('display_startup_errors', '1');
ini_set('log_errors', '1');
ini_set('error_log', __DIR__ . '/../logs/php_errors.log');

// Untuk production: MATIKAN display_errors, AKTIFKAN log_errors
// error_reporting(E_ALL);
// ini_set('display_errors', '0');
// ini_set('log_errors', '1');
```

PHP

```
<?php
// Kenali jenis-jenis error PHP:

// 1. Parse Error / Syntax Error — script tidak bisa dijalankan sama sekali
// echo "lupa tanda kutip penutup; // ← ini akan gagal parse

// 2. Fatal Error — script berhenti di sini
// fungsiYangTidakAda(); // Uncaught Error: Call to undefined function

// 3. Warning — ada masalah tapi script lanjut
// include 'file_tidak_ada.php'; // Warning: include(): Failed opening...

// 4. Notice — potensi masalah kecil
// echo $variabelBelumDiisi; // Notice: Undefined variable

// 5. Deprecated — fitur yang akan dihapus di versi mendatang
// Hindari fungsi-fungsi yang sudah deprecated

// CARA MEMBACA ERROR PHP:
// Fatal error: Uncaught Error: Call to undefined function fungsiX()
//              in /var/www/perpustakaan/index.php on line 42
//              ↑ jenis error    ↑ pesan                ↑ file   ↑ baris
```

---

### B. Variabel, Tipe Data, dan Operator

> 💡 **Benang Merah ke A**: PHP sudah berjalan. Sekarang kita simpan data dalam variabel. PHP adalah _dynamically typed_ — tipe data ditentukan saat runtime, bukan saat deklarasi.

text

```
Benang Merah Bagian B:
PHP berjalan di server (A) →
Variabel: wadah penyimpan data dengan tanda $ →
PHP dynamically typed: tipe ditentukan otomatis →
Type juggling: PHP suka mengubah tipe sendiri (hati-hati!) →
var_dump(): cara terbaik melihat tipe data sebenarnya →
Operator: melakukan operasi pada data
```

#### [[5. Variabel dan Tipe Data Dasar]]

PHP

```
<?php
// Semua variabel PHP diawali dengan $
// PHP case-sensitive untuk nama variabel!

// ─── String ──────────────────────────────────────────────────────────────
$judul     = "Clean Code";           // double quote: proses variabel dan escape
$pengarang = 'Robert Martin';        // single quote: literal, lebih cepat

// Perbedaan single vs double quote:
$nama = "Perpustakaan";
echo "Selamat datang di $nama";      // "Selamat datang di Perpustakaan"
echo 'Selamat datang di $nama';      // "Selamat datang di $nama" (literal!)
echo "Tab:\t Newline:\n";            // proses escape sequence
echo 'Tab:\t Newline:\n';            // literal \t dan \n

// Heredoc: multi-baris dengan proses variabel (seperti double quote)
$deskripsi = <<<EOT
Judul: $judul
Pengarang: $pengarang
Buku ini membahas cara menulis kode yang bersih.
EOT;

// Nowdoc: multi-baris tanpa proses variabel (seperti single quote)
$template = <<<'EOT'
Judul: $judul
Pengarang: $pengarang
Variabel tidak diproses di sini.
EOT;

// ─── Number ──────────────────────────────────────────────────────────────
$harga     = 150000;     // integer
$rating    = 4.8;        // float
$stok      = 5;          // integer
$tahun     = 2008;       // integer

// PHP_INT_MAX, PHP_INT_MIN, PHP_FLOAT_MAX
echo PHP_INT_MAX; // 9223372036854775807

// ─── Boolean ─────────────────────────────────────────────────────────────
$tersedia  = true;
$habis     = false;

// Nilai yang dianggap FALSE di PHP:
// false, 0, 0.0, "", "0", [], null

// ─── Null ────────────────────────────────────────────────────────────────
$tanggalKembali = null; // belum dikembalikan
$isbn = null;           // ISBN tidak diketahui

// ─── var_dump(): teman terbaik untuk debugging ───────────────────────────
var_dump($judul);       // string(10) "Clean Code"
var_dump($harga);       // int(150000)
var_dump($rating);      // float(4.8)
var_dump($tersedia);    // bool(true)
var_dump($isbn);        // NULL

// print_r(): lebih readable untuk array dan object
print_r(['Clean Code', 'Laskar Pelangi']);
// Array ( [0] => Clean Code [1] => Laskar Pelangi )
```

#### [[6. Type Juggling — PHP yang Suka Mengubah Tipe]]

PHP

```
<?php
// Type juggling adalah fitur PHP yang paling sering menyebabkan bug!
// Pahami ini dari awal untuk menghindari masalah.

// PHP otomatis mengubah tipe saat operasi:
$stok = "5";          // string
$total = $stok + 3;   // PHP ubah "5" ke integer 5
var_dump($total);     // int(8) — sudah jadi integer!

// Ini lebih berbahaya:
$a = "5 buku";        // string dengan angka di awal
$b = $a + 2;          // PHP ambil angka "5", hasilnya int(7)

$c = "buku 5";        // string dengan angka di tengah
$d = $c + 2;          // PHP tidak ketemu angka di awal, hasilnya int(2)
// (PHP 8 mengeluarkan Warning untuk ini)

// Perbandingan yang mengejutkan:
var_dump(0 == "php");     // false di PHP 8 (true di PHP 7!) — BERUBAH!
var_dump("1" == "01");    // true (keduanya dianggap integer 1)
var_dump("" == null);     // true
var_dump("0" == false);   // true
var_dump(100 == "1e2");   // true (1e2 = 100)

// SOLUSI: SELALU gunakan === (strict comparison)
var_dump("1" === "01");   // false — tipe DAN nilai harus sama
var_dump("" === null);    // false
var_dump(0 === false);    // false

// Type casting: konversi tipe secara eksplisit
$inputHarga = "150000";   // dari form HTML, SELALU string!
$harga = (int) $inputHarga;      // 150000 sebagai integer
$harga = (float) $inputHarga;    // 150000.0 sebagai float
$harga = intval($inputHarga);    // cara lain: 150000
$harga = floatval($inputHarga);  // cara lain: 150000.0

// Untuk input dari user, selalu cast secara eksplisit:
$id      = (int) ($_GET['id'] ?? 0);
$halaman = max(1, (int) ($_GET['halaman'] ?? 1));
$harga   = (float) ($_POST['harga'] ?? 0);
```

#### [[7. Konstanta dan Operator]]

PHP

```
<?php
// ─── Konstanta ────────────────────────────────────────────────────────────
// Nilai yang tidak bisa berubah setelah didefinisikan

// Cara lama (masih valid):
define('NAMA_PERPUSTAKAAN', 'Perpustakaan Digital Kota');
define('MAKS_PINJAM', 5);
define('DENDA_PER_HARI', 1000);

// Cara modern (lebih disukai):
const VERSI_SISTEM = '2.0.0';
const KATEGORI_BUKU = ['Fiksi', 'Non-Fiksi', 'Sains', 'Teknologi', 'Sejarah'];

// PHP built-in constants:
echo PHP_VERSION;          // 8.3.x
echo PHP_EOL;              // newline sesuai OS (\n di Linux, \r\n di Windows)
echo PHP_INT_MAX;          // 9223372036854775807
echo DIRECTORY_SEPARATOR;  // / di Linux/macOS, \ di Windows
echo __FILE__;             // path lengkap file saat ini
echo __DIR__;              // direktori file saat ini
echo __LINE__;             // nomor baris saat ini

// Penggunaan:
echo NAMA_PERPUSTAKAAN;    // tidak perlu tanda $
echo "Maksimal pinjam: " . MAKS_PINJAM . " buku";

// ─── Operator ─────────────────────────────────────────────────────────────

// Aritmatika
$harga = 150000;
$diskon = 0.10;
$hargaDiskon = $harga * (1 - $diskon);   // 135000
$sisaStok    = 10 - 3;                    // 7
$totalHarga  = $harga * 3;               // 450000
$perBuku     = 450000 / 3;               // 150000
$sisa        = 10 % 3;                   // 1 (modulo)
$pangkat     = 2 ** 8;                   // 256

// Penugasan
$stok = 10;
$stok += 5;   // 15 (sama dengan $stok = $stok + 5)
$stok -= 3;   // 12
$stok *= 2;   // 24
$stok /= 4;   // 6
$stok %= 4;   // 2
$stok **= 2;  // 4

// String
$info = "Buku: " . $judul;         // concatenation
$info .= " oleh " . $pengarang;    // append

// Null coalescing — sangat berguna untuk input dari user!
$halaman  = $_GET['halaman'] ?? 1;                     // jika tidak ada, default 1
$kategori = $_GET['kategori'] ?? 'semua';
$q        = $_GET['q'] ?? '';
$nama     = $_POST['nama'] ?? null;

// Null coalescing assignment (PHP 7.4+)
$data['hit'] ??= 0;       // set ke 0 jika belum ada
$data['hit']++;

// Spaceship operator — untuk sorting
echo 1 <=> 2;   // -1 (kiri lebih kecil)
echo 2 <=> 2;   //  0 (sama)
echo 3 <=> 2;   //  1 (kiri lebih besar)
```

---

### C. Kontrol Alur — Membuat Keputusan

#### [[8. Kondisional dan Loop]]

PHP

```
<?php
// ─── if / elseif / else ───────────────────────────────────────────────────
$stok = 3;

if ($stok > 10) {
    $statusStok = "Stok banyak";
} elseif ($stok > 0) {
    $statusStok = "Stok terbatas ($stok tersisa)";
} else {
    $statusStok = "Stok habis";
}

// Alternatif syntax (direkomendasikan di dalam HTML template):
// Lebih mudah dibaca daripada { } di tengah HTML
?>
<div class="status-stok">
    <?php if ($stok > 10): ?>
        <span class="badge badge--success">Stok Banyak</span>
    <?php elseif ($stok > 0): ?>
        <span class="badge badge--warning">Terbatas (<?= $stok ?>)</span>
    <?php else: ?>
        <span class="badge badge--danger">Habis</span>
    <?php endif; ?>
</div>

<?php
// ─── match expression (PHP 8.0+) ─────────────────────────────────────────
// Lebih baik dari switch: strict comparison, tidak ada fall-through

$kategori = 'Teknologi';

$warnaBadge = match($kategori) {
    'Teknologi'  => 'blue',
    'Fiksi'      => 'purple',
    'Sains'      => 'green',
    'Sejarah'    => 'orange',
    default      => 'gray',
};

// match bisa mengembalikan nilai dan dipakai sebagai ekspresi
echo "Warna: $warnaBadge";

// match juga bisa handle multiple conditions:
$hari = 3;
$jenisHari = match(true) {
    $hari >= 1 && $hari <= 5 => 'Weekday',
    $hari === 6              => 'Sabtu',
    $hari === 7              => 'Minggu',
    default                  => 'Tidak valid',
};

// ─── Loop: for ───────────────────────────────────────────────────────────
// Ketika tahu berapa kali harus diulang
for ($i = 1; $i <= 5; $i++) {
    echo "Halaman $i<br>";
}

// Membuat pagination:
$totalHalaman = 10;
$halamanAktif = 3;

for ($i = 1; $i <= $totalHalaman; $i++) {
    $aktif = $i === $halamanAktif ? 'aktif' : '';
    echo "<a href='?halaman=$i' class='page-link $aktif'>$i</a>";
}

// ─── Loop: while ─────────────────────────────────────────────────────────
// Ketika tidak tahu berapa kali harus diulang
$angka = 1;
while ($angka <= 10) {
    if ($angka % 2 === 0) {
        echo "$angka (genap)<br>";
    }
    $angka++;
}

// ─── Loop: foreach ───────────────────────────────────────────────────────
// Untuk iterasi array — PALING SERING DIPAKAI
$katalog = [
    ['id' => 1, 'judul' => 'Clean Code',     'harga' => 150000],
    ['id' => 2, 'judul' => 'Laskar Pelangi',  'harga' => 95000],
    ['id' => 3, 'judul' => 'The Great Gatsby','harga' => 120000],
];

foreach ($katalog as $buku) {
    echo $buku['judul'] . " — Rp " . number_format($buku['harga'], 0, ',', '.') . "<br>";
}

// Dengan key:
$info = ['nama' => 'Budi', 'email' => 'budi@email.com', 'kota' => 'Bandung'];
foreach ($info as $key => $value) {
    echo "$key: $value<br>";
}

// Alternatif syntax di dalam HTML:
?>
<ul class="daftar-buku">
    <?php foreach ($katalog as $buku): ?>
        <li>
            <strong><?= htmlspecialchars($buku['judul']) ?></strong>
            — Rp <?= number_format($buku['harga'], 0, ',', '.') ?>
        </li>
    <?php endforeach; ?>
</ul>
```

---

### 🏗️ Checkpoint Level 1

text

```
✅ Checklist sebelum lanjut ke Level 2:

PEMAHAMAN:
├── Bisa jelaskan perbedaan PHP (server-side) vs JavaScript (client-side)
├── Bisa jelaskan apa yang terjadi saat browser request file .php
├── Bisa jelaskan perbedaan == vs === dengan contoh nyata
├── Bisa jelaskan type juggling dan mengapa berbahaya
└── Bisa jelaskan kapan pakai single quote vs double quote

PROYEK: Halaman Profil Perpustakaan
├── index.php: profil perpustakaan dengan variabel PHP
├── profil.php: informasi lengkap (jam buka, alamat, fasilitas)
├── hitung.php: kalkulator denda dengan operator
└── error_reporting aktif di semua file

KEBIASAAN:
├── error_reporting(E_ALL) di setiap file development
├── var_dump() untuk debug tipe data, bukan echo saja
├── Selalu === bukan ==
├── htmlspecialchars() untuk SEMUA output ke HTML
└── Nama variabel deskriptif ($hargaBuku bukan $x)

Git: feat: setup PHP project and create library profile page
```

---

## 🔵 LEVEL 2: STRING, ARRAY, DAN FUNGSI (Minggu 4-7)

> **Tema**: _"Dari data individual ke koleksi data dan fungsi yang reusable"_  
> **Benang Merah**: Variabel tunggal (Level 1) → kumpulan data (array) → fungsi untuk mengolah data → sistem katalog buku  
> **Output**: Katalog buku dengan array multidimensi, pencarian, filter, dan fungsi utility

---

### D. String — Manipulasi Teks

> 💡 **Benang Merah ke Level 1**: Di Level 1, kita simpan teks di variabel. Sekarang kita olah teks itu. Hampir semua input dari user adalah string, jadi string functions adalah yang paling sering dipakai.

#### [[9. String Functions — Yang Paling Sering Dipakai]]

PHP

```
<?php
$judul     = "  Clean Code: A Handbook of Agile Software Craftsmanship  ";
$pengarang = "Robert C. Martin";
$isbn      = "978-0-13-235088-4";

// ─── Pembersihan ─────────────────────────────────────────────────────────
echo trim($judul);          // hapus whitespace di awal dan akhir
echo ltrim($judul);         // hapus whitespace di kiri saja
echo rtrim($judul);         // hapus whitespace di kanan saja

// ─── Case ────────────────────────────────────────────────────────────────
echo strtolower("CLEAN CODE");   // clean code
echo strtoupper("clean code");   // CLEAN CODE
echo ucfirst("clean code");      // Clean code
echo ucwords("clean code book"); // Clean Code Book

// ─── Pencarian ────────────────────────────────────────────────────────────
echo strpos("Clean Code", "Code");      // 6 (index mulai dari 0)
echo strrpos("Code Code Book", "Code"); // 5 (cari dari kanan)

// PHP 8: str_contains, str_starts_with, str_ends_with — gunakan ini!
$adaKata    = str_contains("Clean Code", "Code");      // true
$mulaiDengan = str_starts_with("Clean Code", "Clean"); // true
$akhirDengan = str_ends_with("Clean Code", "Code");    // true

// ─── Penggantian ─────────────────────────────────────────────────────────
$isbnBersih = str_replace("-", "", $isbn);  // "9780132350884"
$isbnBersih = str_replace(["-", " "], "", $isbn); // hapus - dan spasi

// preg_replace: regex untuk pattern yang kompleks
$hanyaAngka = preg_replace('/[^0-9]/', '', $isbn); // "9780132350884"

// ─── Pemotongan dan Penggabungan ─────────────────────────────────────────
$bagian  = substr("Clean Code Book", 6, 4);  // "Code"
$bagian2 = substr("Clean Code", -4);         // "Code" (dari belakang)

$parts    = explode("-", $isbn);  // ["978", "0", "13", "235088", "4"]
$digabung = implode("", $parts);  // "97801323508840"

// ─── Format ──────────────────────────────────────────────────────────────
echo strlen("Clean Code");                           // 10
echo mb_strlen("Buku Bäru");                         // 10 (multi-byte safe!)
echo number_format(1500000, 0, ',', '.');            // 1.500.000
echo number_format(4.567, 2, ',', '.');              // 4,57
echo sprintf("%-20s | Rp %10s", "Clean Code", "150.000"); // padding

// sprintf untuk format yang konsisten
function formatHarga(int $harga): string {
    return 'Rp ' . number_format($harga, 0, ',', '.');
}

function formatISBN(string $isbn): string {
    // Hapus semua non-digit, lalu format jadi XXX-X-XX-XXXXXX-X
    $digits = preg_replace('/[^0-9]/', '', $isbn);
    if (strlen($digits) === 13) {
        return substr($digits, 0, 3) . '-' .
               substr($digits, 3, 1) . '-' .
               substr($digits, 4, 2) . '-' .
               substr($digits, 6, 6) . '-' .
               substr($digits, 12, 1);
    }
    return $isbn;
}
```

---

### E. Array — Koleksi Data

> 💡 **Benang Merah ke String**: String adalah satu teks. Array adalah kumpulan nilai. Array adalah struktur data paling penting di PHP dan dipakai di mana-mana.

#### [[10. Array Indexed, Associative, dan Multidimensional]]

PHP

```
<?php
// ─── Indexed Array ────────────────────────────────────────────────────────
$kategori = ['Fiksi', 'Non-Fiksi', 'Sains', 'Teknologi', 'Sejarah'];
echo $kategori[0];        // Fiksi
echo count($kategori);    // 5

// Tambah elemen
$kategori[] = 'Biografi';              // tambah di akhir
array_push($kategori, 'Motivasi');     // sama dengan di atas

// Hapus elemen
array_pop($kategori);                  // hapus dari akhir
array_shift($kategori);               // hapus dari awal
unset($kategori[2]);                  // hapus index tertentu (PERHATIAN: index tidak reset!)
$kategori = array_values($kategori);  // reset index setelah unset

// ─── Associative Array ────────────────────────────────────────────────────
$buku = [
    'id'        => 1,
    'judul'     => 'Clean Code',
    'pengarang' => 'Robert C. Martin',
    'isbn'      => '9780132350884',
    'tahun'     => 2008,
    'harga'     => 150000,
    'stok'      => 5,
    'kategori'  => 'Teknologi',
    'tersedia'  => true,
];

echo $buku['judul'];      // Clean Code
echo $buku['harga'];      // 150000

// Tambah, ubah, hapus property
$buku['rating']  = 4.8;   // tambah key baru
$buku['stok']    = 4;     // ubah nilai
unset($buku['rating']);    // hapus key

// Cek keberadaan key
if (isset($buku['isbn'])) {
    echo "ISBN: " . $buku['isbn'];
}

if (array_key_exists('rating', $buku)) {
    echo "Rating: " . $buku['rating'];
}

// ─── Multidimensional Array ───────────────────────────────────────────────
$katalog = [
    [
        'id'        => 1,
        'judul'     => 'Clean Code',
        'pengarang' => 'Robert Martin',
        'harga'     => 150000,
        'stok'      => 5,
        'kategori'  => 'Teknologi',
    ],
    [
        'id'        => 2,
        'judul'     => 'Laskar Pelangi',
        'pengarang' => 'Andrea Hirata',
        'harga'     => 95000,
        'stok'      => 3,
        'kategori'  => 'Fiksi',
    ],
    [
        'id'        => 3,
        'judul'     => 'Cosmos',
        'pengarang' => 'Carl Sagan',
        'harga'     => 200000,
        'stok'      => 0,
        'kategori'  => 'Sains',
    ],
];

// Akses array multidimensi
echo $katalog[0]['judul'];     // Clean Code
echo $katalog[1]['harga'];     // 95000
echo $katalog[2]['kategori'];  // Sains
```

#### [[11. Array Functions — Transformasi dan Manipulasi]]

PHP

```
<?php
// ─── Pencarian ────────────────────────────────────────────────────────────

// in_array: apakah nilai ada di array?
$kategoriValid = ['Fiksi', 'Non-Fiksi', 'Sains', 'Teknologi'];
$inputKategori = 'Fiksi';
if (in_array($inputKategori, $kategoriValid, true)) { // true = strict
    echo "Kategori valid";
}

// array_search: cari index/key dari nilai
$index = array_search('Sains', $kategoriValid); // 2

// ─── Filter ──────────────────────────────────────────────────────────────

// array_filter: filter berdasarkan kondisi
$bukuTersedia = array_filter($katalog, fn($b) => $b['stok'] > 0);
$bukuFiksi    = array_filter($katalog, fn($b) => $b['kategori'] === 'Fiksi');

// PENTING: array_filter mempertahankan key asli! Gunakan array_values untuk reset
$bukuTersedia = array_values(array_filter($katalog, fn($b) => $b['stok'] > 0));

// ─── Transformasi ─────────────────────────────────────────────────────────

// array_map: transformasi setiap elemen, return array baru
$judulSemua = array_map(fn($b) => $b['judul'], $katalog);
// ['Clean Code', 'Laskar Pelangi', 'Cosmos']

// Transformasi untuk tampilan:
$katalogFormatted = array_map(function($buku) {
    return [
        ...$buku,
        'harga_format' => 'Rp ' . number_format($buku['harga'], 0, ',', '.'),
        'tersedia'     => $buku['stok'] > 0,
    ];
}, $katalog);

// ─── Sorting ─────────────────────────────────────────────────────────────

// sort: sort indexed array ascending (modifikasi array asli!)
$harga = [150000, 95000, 200000];
sort($harga);    // [95000, 150000, 200000]
rsort($harga);   // [200000, 150000, 95000] (descending)

// asort: sort associative array berdasarkan value (pertahankan key)
$stokPerBuku = ['Clean Code' => 5, 'Cosmos' => 0, 'Laskar Pelangi' => 3];
asort($stokPerBuku);  // ['Cosmos' => 0, 'Laskar Pelangi' => 3, 'Clean Code' => 5]

// usort: sort dengan fungsi custom (untuk array multidimensi)
usort($katalog, fn($a, $b) => $a['harga'] <=> $b['harga']);     // sort by harga asc
usort($katalog, fn($a, $b) => $b['harga'] <=> $a['harga']);     // sort by harga desc
usort($katalog, fn($a, $b) => strcmp($a['judul'], $b['judul'])); // sort by judul

// ─── Agregasi ─────────────────────────────────────────────────────────────

// array_reduce: akumulasi semua elemen menjadi satu nilai
$totalHarga = array_reduce($katalog, fn($carry, $b) => $carry + $b['harga'], 0);
// 445000

$totalStok = array_reduce($katalog, fn($carry, $b) => $carry + $b['stok'], 0);

// array_column: ambil satu kolom dari array multidimensi
$semuaJudul = array_column($katalog, 'judul');
// ['Clean Code', 'Laskar Pelangi', 'Cosmos']

// array_column juga bisa buat indexed array dengan key tertentu
$katalogById = array_column($katalog, null, 'id');
// [1 => [...buku1...], 2 => [...buku2...], 3 => [...buku3...]]
echo $katalogById[1]['judul']; // Clean Code (akses langsung by ID!)

// ─── Fungsi pencarian dan filter untuk katalog ────────────────────────────
function cariBuku(array $katalog, string $keyword): array {
    if (empty(trim($keyword))) return $katalog;

    $keyword = strtolower(trim($keyword));
    return array_values(array_filter($katalog, function($buku) use ($keyword) {
        return str_contains(strtolower($buku['judul']), $keyword)
            || str_contains(strtolower($buku['pengarang']), $keyword);
    }));
}

function filterKategori(array $katalog, string $kategori): array {
    if ($kategori === 'semua') return $katalog;
    return array_values(array_filter($katalog, fn($b) => $b['kategori'] === $kategori));
}

function urutkanBuku(array $katalog, string $field = 'judul', string $arah = 'asc'): array {
    $fieldValid = ['judul', 'pengarang', 'harga', 'tahun'];
    if (!in_array($field, $fieldValid)) $field = 'judul';

    usort($katalog, function($a, $b) use ($field, $arah) {
        $hasil = is_string($a[$field])
            ? strcmp($a[$field], $b[$field])
            : $a[$field] <=> $b[$field];
        return $arah === 'desc' ? -$hasil : $hasil;
    });

    return $katalog;
}
```

---

### F. Fungsi — Kode yang Reusable

> 💡 **Benang Merah ke Array**: `array_filter`, `array_map`, `usort` semua menerima fungsi sebagai argument. Sekarang kita pelajari cara membuat dan menggunakan fungsi sendiri secara komprehensif.

#### [[12. Fungsi — Definisi, Parameter, Return, Type Hints]]

PHP

```
<?php
// ─── Fungsi dasar ─────────────────────────────────────────────────────────

// PHP 8: type hints sangat direkomendasikan!
// parameter_type $param: return_type
function hitungDenda(int $hariTerlambat, int $dendaPerHari = 1000): int {
    if ($hariTerlambat < 0) {
        throw new InvalidArgumentException("Hari terlambat tidak boleh negatif");
    }
    return $hariTerlambat * $dendaPerHari;
}

echo hitungDenda(7);        // 7000 (default denda 1000)
echo hitungDenda(7, 2000);  // 14000

// Named arguments (PHP 8.0+)
echo hitungDenda(dendaPerHari: 2000, hariTerlambat: 5); // 10000

// ─── Return multiple values via array ────────────────────────────────────
function statistikKatalog(array $katalog): array {
    $tersedia = array_filter($katalog, fn($b) => $b['stok'] > 0);
    $habis    = array_filter($katalog, fn($b) => $b['stok'] === 0);

    return [
        'total'    => count($katalog),
        'tersedia' => count($tersedia),
        'habis'    => count($habis),
        'rerata_harga' => count($katalog) > 0
            ? array_sum(array_column($katalog, 'harga')) / count($katalog)
            : 0,
    ];
}

$stats = statistikKatalog($katalog);
echo "Total: {$stats['total']}, Tersedia: {$stats['tersedia']}";

// ─── Nullable type ────────────────────────────────────────────────────────
// ?TypeName berarti bisa TypeName atau null
function cariBukuById(array $katalog, int $id): ?array {
    foreach ($katalog as $buku) {
        if ($buku['id'] === $id) return $buku;
    }
    return null; // tidak ditemukan
}

$buku = cariBukuById($katalog, 1);
if ($buku !== null) {
    echo $buku['judul'];
}

// ─── Union types (PHP 8.0+) ───────────────────────────────────────────────
function prosesId(int|string $id): string {
    if (is_int($id)) {
        return sprintf("ID-%05d", $id);
    }
    return strtoupper(trim($id));
}

// ─── Variadic functions: jumlah argument yang fleksibel ──────────────────
function gabungkanTag(string $separator, string ...$tags): string {
    return implode($separator, $tags);
}

echo gabungkanTag(', ', 'PHP', 'MySQL', 'HTML', 'CSS');
// PHP, MySQL, HTML, CSS

// ─── Closure dan Arrow Function ───────────────────────────────────────────

// Closure: fungsi anonim
$formatHarga = function(int $harga): string {
    return 'Rp ' . number_format($harga, 0, ',', '.');
};

echo $formatHarga(150000); // Rp 150.000

// Closure dengan use: capture variabel dari scope luar
$batasMaks = 200000;
$bukuTerjangkau = array_filter($katalog, function($buku) use ($batasMaks) {
    return $buku['harga'] <= $batasMaks;
});

// Arrow function (PHP 7.4+): otomatis capture variabel luar
$bukuTerjangkau = array_filter($katalog, fn($buku) => $buku['harga'] <= $batasMaks);

// ─── Fungsi rekursif ──────────────────────────────────────────────────────
function faktorial(int $n): int {
    if ($n <= 1) return 1;
    return $n * faktorial($n - 1);
}

// Buat nomor anggota dengan format: PERP-YYYY-XXXXX
function generateNomorAnggota(int $lastId): string {
    return sprintf('PERP-%d-%05d', date('Y'), $lastId + 1);
}

echo generateNomorAnggota(42); // PERP-2024-00043
```

---

### 🏗️ Checkpoint Level 2

text

```
✅ Checklist sebelum lanjut ke Level 3:

PROYEK: Katalog Buku (halaman statis dengan data hardcode)
├── functions/helpers.php: semua fungsi utility
├── katalog.php: tampilkan katalog dengan array_filter dan foreach
├── detail.php: tampilkan detail satu buku
├── Pencarian via query string: katalog.php?q=clean
├── Filter kategori: katalog.php?kategori=Teknologi
└── Sort: katalog.php?sort=harga&arah=asc

PEMAHAMAN:
├── Bisa jelaskan perbedaan single vs double quote
├── Bisa jelaskan mengapa array_filter perlu array_values
├── Bisa jelaskan perbedaan closure vs arrow function
├── Bisa jelaskan type hints dan mengapa penting
└── Bisa gunakan named arguments PHP 8

KEBIASAAN:
├── Semua fungsi punya type hints (parameter dan return)
├── Semua string output pakai htmlspecialchars()
├── Gunakan str_contains/str_starts_with bukan strpos (PHP 8)
└── Arrow function untuk callback singkat

Git: feat: create book catalog with arrays and utility functions
```

---

## 🟡 LEVEL 3: FORM, SESSION, DAN FILE (Minggu 7-10)

> **Tema**: _"Dari halaman statis ke aplikasi web yang menerima input dan mengingat state"_  
> **Benang Merah**: Data hardcode (Level 2) → input dari user via form → validasi dan sanitasi → session untuk login → file untuk simpan data → web app yang interaktif  
> **Output**: Sistem perpustakaan web dengan CRUD, login, dan penyimpanan file JSON

---

### G. Form Handling — Menerima Input dari User

> 💡 **Mengapa ini bagian terpenting?** Semua data yang masuk dari user HARUS dianggap berbahaya. XSS dan SQL Injection berasal dari sini. Security bukan topik terpisah — ini harus dipikirkan setiap kali handle input.

text

```
Benang Merah Bagian G:
Data hardcode di array (Level 2) →
User bisa input data via form HTML →
$_POST/$_GET: superglobal untuk menangkap data form →
Validasi: pastikan data benar dan sesuai aturan →
Sanitasi: bersihkan data berbahaya sebelum digunakan →
CSRF: lindungi form dari serangan cross-site request forgery →
Tampilkan error dan pertahankan input yang sudah diisi
```

#### [[13. $_POST, $_GET, dan Superglobal PHP]]

PHP

```
<?php
// ─── Superglobal PHP: tersedia di mana saja ──────────────────────────────
// $_GET:     data dari URL query string
// $_POST:    data dari form method POST
// $_REQUEST: gabungan $_GET, $_POST, $_COOKIE (hindari ini)
// $_SERVER:  informasi tentang server dan request
// $_FILES:   data file yang diupload
// $_SESSION: data session
// $_COOKIE:  data cookie
// $_ENV:     variabel environment

// ─── Membedakan GET vs POST ───────────────────────────────────────────────
// GET:  data tampak di URL, cocok untuk pencarian/filter (bookmarkable)
// POST: data tidak tampak, cocok untuk form yang mengubah data

// Cek method request
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // handle form submission
} else {
    // tampilkan form
}

// ─── Mengambil data dari $_GET ────────────────────────────────────────────
// URL: katalog.php?q=clean+code&kategori=Teknologi&halaman=2

$keyword  = $_GET['q'] ?? '';             // '' jika tidak ada
$kategori = $_GET['kategori'] ?? 'semua';
$halaman  = max(1, (int) ($_GET['halaman'] ?? 1));

// ─── Mengambil data dari $_POST ───────────────────────────────────────────
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $judul     = $_POST['judul'] ?? '';
    $pengarang = $_POST['pengarang'] ?? '';
    $harga     = (int) ($_POST['harga'] ?? 0);

    // JANGAN langsung pakai! Harus divalidasi dan disanitasi dulu
}

// ─── $_SERVER: informasi request dan server ───────────────────────────────
echo $_SERVER['REQUEST_METHOD'];    // GET, POST, PUT, DELETE
echo $_SERVER['REQUEST_URI'];       // /perpustakaan/katalog.php?q=clean
echo $_SERVER['HTTP_HOST'];         // localhost atau perpustakaan.test
echo $_SERVER['REMOTE_ADDR'];       // IP address user
echo $_SERVER['HTTP_USER_AGENT'];   // browser string
echo $_SERVER['HTTPS'];             // 'on' jika HTTPS, tidak ada jika HTTP

// Base URL yang dinamis:
function baseUrl(): string {
    $protokol = (!empty($_SERVER['HTTPS']) && $_SERVER['HTTPS'] !== 'off') ? 'https' : 'http';
    $host = $_SERVER['HTTP_HOST'];
    $path = dirname($_SERVER['SCRIPT_NAME']);
    return $protokol . '://' . $host . ($path !== '/' ? $path : '');
}
```

#### [[14. Validasi dan Sanitasi — Security dari Hari Pertama]]

PHP

```
<?php
// File: functions/validasi.php

// ─── Prinsip Security Input Handling ─────────────────────────────────────
// 1. JANGAN percaya input user — semua input dianggap berbahaya
// 2. Validasi: pastikan data sesuai aturan bisnis
// 3. Sanitasi: bersihkan data berbahaya
// 4. Escape: konversi karakter berbahaya saat OUTPUT
// 5. Parameterized query: gunakan prepared statement untuk database

// ─── Sanitasi input ───────────────────────────────────────────────────────

// htmlspecialchars: WAJIB untuk semua output ke HTML
// Konversi: < → &lt;, > → &gt;, " → &quot;, ' → &#039;, & → &amp;
function esc(mixed $nilai): string {
    return htmlspecialchars((string) $nilai, ENT_QUOTES | ENT_HTML5, 'UTF-8');
}

// Penggunaan:
echo esc($buku['judul']);     // aman dari XSS
echo esc($_GET['q']);         // aman dari XSS

// filter_var: sanitasi dengan filter bawaan PHP
$email    = filter_var($_POST['email'] ?? '', FILTER_SANITIZE_EMAIL);
$url      = filter_var($_POST['url'] ?? '',   FILTER_SANITIZE_URL);

// ─── Validasi input ───────────────────────────────────────────────────────
class Validator {
    private array $errors = [];
    private array $data   = [];

    public function __construct(private array $input) {}

    public function required(string $field, string $label): static {
        $nilai = trim($this->input[$field] ?? '');
        if ($nilai === '') {
            $this->errors[$field] = "$label wajib diisi.";
        } else {
            $this->data[$field] = $nilai;
        }
        return $this;
    }

    public function string(string $field, string $label, int $min = 1, int $max = 255): static {
        if (isset($this->errors[$field])) return $this;
        $nilai = $this->data[$field] ?? trim($this->input[$field] ?? '');
        if (mb_strlen($nilai) < $min) {
            $this->errors[$field] = "$label minimal $min karakter.";
        } elseif (mb_strlen($nilai) > $max) {
            $this->errors[$field] = "$label maksimal $max karakter.";
        } else {
            $this->data[$field] = $nilai;
        }
        return $this;
    }

    public function integer(string $field, string $label, int $min = PHP_INT_MIN, int $max = PHP_INT_MAX): static {
        if (isset($this->errors[$field])) return $this;
        $nilai = $this->input[$field] ?? '';
        if (!is_numeric($nilai) || (int) $nilai != $nilai) {
            $this->errors[$field] = "$label harus berupa angka bulat.";
        } elseif ((int) $nilai < $min) {
            $this->errors[$field] = "$label minimal $min.";
        } elseif ((int) $nilai > $max) {
            $this->errors[$field] = "$label maksimal $max.";
        } else {
            $this->data[$field] = (int) $nilai;
        }
        return $this;
    }

    public function email(string $field, string $label): static {
        if (isset($this->errors[$field])) return $this;
        $nilai = trim($this->input[$field] ?? '');
        if (!filter_var($nilai, FILTER_VALIDATE_EMAIL)) {
            $this->errors[$field] = "$label tidak valid.";
        } else {
            $this->data[$field] = strtolower($nilai);
        }
        return $this;
    }

    public function inArray(string $field, string $label, array $allowed): static {
        if (isset($this->errors[$field])) return $this;
        $nilai = $this->input[$field] ?? '';
        if (!in_array($nilai, $allowed, true)) {
            $this->errors[$field] = "$label tidak valid.";
        } else {
            $this->data[$field] = $nilai;
        }
        return $this;
    }

    public function gagal(): bool { return !empty($this->errors); }
    public function berhasil(): bool { return empty($this->errors); }
    public function getErrors(): array { return $this->errors; }
    public function getData(): array { return $this->data; }
    public function getError(string $field): ?string { return $this->errors[$field] ?? null; }
}

// ─── CSRF Protection ──────────────────────────────────────────────────────
function generateCsrfToken(): string {
    if (session_status() === PHP_SESSION_NONE) {
        throw new RuntimeException("Session belum dimulai");
    }
    if (empty($_SESSION['csrf_token'])) {
        $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
    }
    return $_SESSION['csrf_token'];
}

function verifyCsrfToken(string $token): bool {
    if (empty($_SESSION['csrf_token'])) return false;
    return hash_equals($_SESSION['csrf_token'], $token);
}

// Di form HTML:
// <input type="hidden" name="csrf_token" value="<?= generateCsrfToken() ?>">

// Di handler POST:
// if (!verifyCsrfToken($_POST['csrf_token'] ?? '')) {
//     http_response_code(403);
//     die('CSRF token tidak valid');
// }
```

#### [[15. Contoh Form Lengkap dengan Validasi]]

PHP

```
<?php
// File: buku/tambah.php
session_start();
require_once '../functions/helpers.php';
require_once '../functions/validasi.php';

$errors  = [];
$input   = [];
$sukses  = false;

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // 1. Verifikasi CSRF token
    if (!verifyCsrfToken($_POST['csrf_token'] ?? '')) {
        http_response_code(403);
        die('Request tidak valid');
    }

    // 2. Validasi semua input
    $validator = (new Validator($_POST))
        ->required('judul', 'Judul')
        ->string('judul', 'Judul', 2, 200)
        ->required('pengarang', 'Pengarang')
        ->string('pengarang', 'Pengarang', 2, 100)
        ->required('tahun', 'Tahun Terbit')
        ->integer('tahun', 'Tahun Terbit', 1000, (int) date('Y'))
        ->required('harga', 'Harga')
        ->integer('harga', 'Harga', 0)
        ->required('stok', 'Stok')
        ->integer('stok', 'Stok', 0, 9999)
        ->required('kategori', 'Kategori')
        ->inArray('kategori', 'Kategori', ['Fiksi', 'Non-Fiksi', 'Sains', 'Teknologi', 'Sejarah']);

    if ($validator->berhasil()) {
        $data = $validator->getData();
        // Level 3: simpan ke file JSON
        // Level 4+: simpan ke database
        $sukses = true;
        $input  = []; // reset form

        // Redirect-After-Post: cegah form resubmission saat refresh
        $_SESSION['flash_sukses'] = "Buku '{$data['judul']}' berhasil ditambahkan!";
        header('Location: /perpustakaan/katalog.php');
        exit;
    } else {
        $errors = $validator->getErrors();
        $input  = $_POST; // pertahankan input untuk re-fill form
    }
}
?>
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <title>Tambah Buku</title>
</head>
<body>
<form method="POST" action="">
    <!-- CSRF Token — WAJIB di semua form POST -->
    <input type="hidden" name="csrf_token" value="<?= generateCsrfToken() ?>">

    <div class="form-group <?= isset($errors['judul']) ? 'has-error' : '' ?>">
        <label for="judul">Judul Buku *</label>
        <input
            type="text"
            id="judul"
            name="judul"
            value="<?= esc($input['judul'] ?? '') ?>"
            maxlength="200"
            required
        >
        <?php if (isset($errors['judul'])): ?>
            <span class="error-msg"><?= esc($errors['judul']) ?></span>
        <?php endif; ?>
    </div>

    <div class="form-group">
        <label for="kategori">Kategori *</label>
        <select id="kategori" name="kategori" required>
            <option value="">-- Pilih Kategori --</option>
            <?php foreach (['Fiksi', 'Non-Fiksi', 'Sains', 'Teknologi', 'Sejarah'] as $kat): ?>
                <option
                    value="<?= esc($kat) ?>"
                    <?= ($input['kategori'] ?? '') === $kat ? 'selected' : '' ?>
                >
                    <?= esc($kat) ?>
                </option>
            <?php endforeach; ?>
        </select>
        <?php if (isset($errors['kategori'])): ?>
            <span class="error-msg"><?= esc($errors['kategori']) ?></span>
        <?php endif; ?>
    </div>

    <button type="submit">Simpan Buku</button>
</form>
</body>
</html>
```

---

### H. Session — Mengingat State User

#### [[16. Session — Sistem Login yang Aman]]

PHP

```
<?php
// File: config/session.php
// Konfigurasi session yang lebih aman — include sebelum session_start()

function setupSession(): void {
    // Keamanan session
    ini_set('session.use_strict_mode', '1');     // tolak session ID yang tidak dikenal
    ini_set('session.cookie_httponly', '1');      // cookie tidak bisa diakses JavaScript
    ini_set('session.cookie_samesite', 'Strict'); // cegah CSRF via cookie
    ini_set('session.gc_maxlifetime', '3600');    // session expire setelah 1 jam

    // Hanya di HTTPS (production):
    // ini_set('session.cookie_secure', '1');

    if (session_status() === PHP_SESSION_NONE) {
        session_start();
    }
}

// ─── Auth functions ───────────────────────────────────────────────────────

function login(array $user): void {
    // Regenerasi session ID setelah login — cegah session fixation attack
    session_regenerate_id(true);

    $_SESSION['user'] = [
        'id'    => $user['id'],
        'nama'  => $user['nama'],
        'email' => $user['email'],
        'role'  => $user['role'],
    ];
    $_SESSION['login_time']  = time();
    $_SESSION['last_active'] = time();
}

function logout(): void {
    // Hapus semua data session
    $_SESSION = [];

    // Hapus cookie session
    if (ini_get('session.use_cookies')) {
        $params = session_get_cookie_params();
        setcookie(
            session_name(), '',
            time() - 42000,
            $params['path'], $params['domain'],
            $params['secure'], $params['httponly']
        );
    }

    session_destroy();
}

function sudahLogin(): bool {
    return isset($_SESSION['user']) && !empty($_SESSION['user']['id']);
}

function cekSesiAktif(int $maxInaktif = 1800): bool {
    // Cek apakah session sudah timeout (30 menit tidak aktif)
    if (isset($_SESSION['last_active'])) {
        if (time() - $_SESSION['last_active'] > $maxInaktif) {
            logout();
            return false;
        }
    }
    $_SESSION['last_active'] = time();
    return true;
}

function requireLogin(string $redirectTo = '/perpustakaan/login.php'): void {
    if (!sudahLogin() || !cekSesiAktif()) {
        $_SESSION['redirect_after_login'] = $_SERVER['REQUEST_URI'];
        header('Location: ' . $redirectTo);
        exit;
    }
}

function requireRole(string $role): void {
    requireLogin();
    if (($_SESSION['user']['role'] ?? '') !== $role) {
        http_response_code(403);
        include '../403.php';
        exit;
    }
}

function userSaatIni(): ?array {
    return $_SESSION['user'] ?? null;
}
```

PHP

```
<?php
// File: login.php — contoh implementasi login yang aman
setupSession();

$errors = [];

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    if (!verifyCsrfToken($_POST['csrf_token'] ?? '')) {
        die('Request tidak valid');
    }

    $email    = trim($_POST['email'] ?? '');
    $password = $_POST['password'] ?? '';

    // Validasi input
    if (empty($email) || !filter_var($email, FILTER_VALIDATE_EMAIL)) {
        $errors['email'] = 'Email tidak valid.';
    }

    if (empty($password)) {
        $errors['password'] = 'Password wajib diisi.';
    }

    if (empty($errors)) {
        // Di Level 3: cek dari array hardcode
        // Di Level 4+: cek dari database dengan PDO
        $adminUser = [
            'id'    => 1,
            'nama'  => 'Admin Perpustakaan',
            'email' => 'admin@perpustakaan.id',
            // JANGAN simpan password plain text!
            // Gunakan password_hash() saat menyimpan
            'password_hash' => password_hash('admin123', PASSWORD_BCRYPT),
            'role'  => 'admin',
        ];

        if ($email === $adminUser['email'] &&
            password_verify($password, $adminUser['password_hash'])) {

            login($adminUser);

            $redirect = $_SESSION['redirect_after_login'] ?? '/perpustakaan/dashboard.php';
            unset($_SESSION['redirect_after_login']);
            header('Location: ' . $redirect);
            exit;
        } else {
            // Pesan error yang ambigu — jangan beritahu mana yang salah!
            $errors['general'] = 'Email atau password salah.';
        }
    }
}
```

---

### I. File I/O — Menyimpan Data Tanpa Database

#### [[17. Penyimpanan Data dengan File JSON]]

PHP

```
<?php
// File: functions/storage.php
// Sistem penyimpanan sederhana menggunakan file JSON
// Ini adalah bridge sebelum belajar database di Level 4

define('DATA_DIR', __DIR__ . '/../data/');

class JsonStorage {
    private string $filePath;

    public function __construct(string $namaFile) {
        // Pastikan direktori ada
        if (!is_dir(DATA_DIR)) {
            mkdir(DATA_DIR, 0755, true);
        }
        $this->filePath = DATA_DIR . $namaFile . '.json';
    }

    public function bacaSemua(): array {
        if (!file_exists($this->filePath)) {
            return [];
        }

        $json = file_get_contents($this->filePath);
        if ($json === false) {
            throw new RuntimeException("Gagal membaca file: {$this->filePath}");
        }

        return json_decode($json, true) ?? [];
    }

    public function simpanSemua(array $data): bool {
        $json = json_encode($data, JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE);
        return file_put_contents($this->filePath, $json, LOCK_EX) !== false;
        // LOCK_EX: cegah race condition saat banyak request bersamaan
    }

    public function tambah(array $item): array {
        $data = $this->bacaSemua();

        // Auto-increment ID
        $maxId = array_reduce($data, fn($carry, $d) => max($carry, $d['id'] ?? 0), 0);
        $item['id']         = $maxId + 1;
        $item['created_at'] = date('Y-m-d H:i:s');
        $item['updated_at'] = date('Y-m-d H:i:s');

        $data[] = $item;
        $this->simpanSemua($data);

        return $item;
    }

    public function update(int $id, array $dataUpdate): bool {
        $data  = $this->bacaSemua();
        $found = false;

        foreach ($data as &$item) {
            if (($item['id'] ?? null) === $id) {
                $item = array_merge($item, $dataUpdate);
                $item['updated_at'] = date('Y-m-d H:i:s');
                $found = true;
                break;
            }
        }
        unset($item); // PENTING: unset reference setelah foreach!

        if ($found) {
            $this->simpanSemua($data);
        }

        return $found;
    }

    public function hapus(int $id): bool {
        $data  = $this->bacaSemua();
        $before = count($data);
        $data  = array_values(array_filter($data, fn($d) => ($d['id'] ?? null) !== $id));

        if (count($data) < $before) {
            return $this->simpanSemua($data);
        }

        return false;
    }

    public function cariById(int $id): ?array {
        foreach ($this->bacaSemua() as $item) {
            if (($item['id'] ?? null) === $id) return $item;
        }
        return null;
    }
}

// Penggunaan:
$bukuStorage = new JsonStorage('buku');

// Tambah buku
$bukuBaru = $bukuStorage->tambah([
    'judul'     => 'Clean Code',
    'pengarang' => 'Robert Martin',
    'harga'     => 150000,
    'stok'      => 5,
    'kategori'  => 'Teknologi',
]);

// Ambil semua buku
$semuaBuku = $bukuStorage->bacaSemua();

// Update stok
$bukuStorage->update(1, ['stok' => 4]);

// Hapus buku
$bukuStorage->hapus(3);
```

---

### 🏗️ Checkpoint Level 3

text

```
✅ Checklist sebelum lanjut ke Level 4:

PROYEK: Sistem Perpustakaan Web (tanpa database)
├── login.php: form login dengan session yang aman
├── dashboard.php: halaman setelah login (protected)
├── buku/daftar.php: tampilkan buku dari JSON dengan pagination
├── buku/tambah.php: form + validasi + CSRF + simpan ke JSON
├── buku/edit.php: form edit dengan data pre-filled
├── buku/hapus.php: konfirmasi + hapus dari JSON
├── logout.php: hancurkan session dengan benar
└── Flash message berfungsi setelah redirect

KEAMANAN (WAJIB — tidak ada kompromi):
├── SEMUA output ke HTML menggunakan htmlspecialchars() / esc()
├── CSRF token di semua form POST
├── session_regenerate_id() setelah login
├── password_hash/password_verify (BUKAN MD5!)
├── Validasi semua input sebelum diproses
├── Redirect-After-Post untuk semua form sukses
└── requireLogin() di semua halaman yang butuh login

Git: feat: add login, session, CSRF, form validation, and JSON storage
```

---

## 🟠 LEVEL 4: OOP DAN DATABASE PDO (Minggu 10-15)

> **Tema**: _"Dari prosedural ke OOP yang terstruktur dan data yang persisten di MySQL"_  
> **Benang Merah**: Fungsi prosedural (Level 2-3) → OOP untuk organisasi kode → PDO untuk database → prepared statements untuk keamanan → sistem perpustakaan dengan MySQL  
> **Output**: Sistem perpustakaan lengkap dengan MySQL, OOP, PDO, dan relasi antar tabel

---

### J. OOP PHP — Organisasi Kode yang Lebih Baik

> 💡 **Mengapa OOP?** Kode prosedural sulit dikelola saat project membesar. OOP mengelompokkan data (property) dan perilaku (method) dalam satu unit (class). Semua framework PHP modern menggunakan OOP.

text

```
Benang Merah Bagian J:
Fungsi prosedural yang makin banyak (Level 2-3) →
OOP: kelompokkan data dan perilaku dalam class →
Encapsulation: property private, akses via method →
Inheritance: class anak mewarisi class induk →
Interface: kontrak yang harus dipenuhi class →
Trait: reuse kode antar class yang tidak berkaitan
```

#### [[18. Class dan Object — Dasar OOP PHP]]

PHP

```
<?php
// File: src/Models/Buku.php

class Buku {
    // Property dengan visibility
    public readonly int $id;          // readonly: hanya bisa di-set di constructor
    public string $judul;
    public string $pengarang;
    public ?string $isbn;
    public int $tahun;
    public float $harga;
    private int $_stok;               // private: akses via getter/setter
    public string $kategori;
    public ?string $deskripsi;
    public readonly \DateTimeImmutable $createdAt;

    public function __construct(
        string $judul,
        string $pengarang,
        int $tahun,
        float $harga,
        int $stok,
        string $kategori,
        ?string $isbn = null,
        ?string $deskripsi = null,
        int $id = 0,
    ) {
        $this->judul     = $judul;
        $this->pengarang = $pengarang;
        $this->isbn      = $isbn;
        $this->tahun     = $tahun;
        $this->harga     = $harga;
        $this->_stok     = $stok;
        $this->kategori  = $kategori;
        $this->deskripsi = $deskripsi;
        $this->id        = $id;
        $this->createdAt = new \DateTimeImmutable();
    }

    // Getter
    public function getStok(): int { return $this->_stok; }

    // Setter dengan validasi
    public function setStok(int $stok): void {
        if ($stok < 0) {
            throw new \InvalidArgumentException("Stok tidak boleh negatif");
        }
        $this->_stok = $stok;
    }

    // Method bisnis
    public function tersedia(): bool {
        return $this->_stok > 0;
    }

    public function pinjam(): void {
        if (!$this->tersedia()) {
            throw new \RuntimeException("Buku '{$this->judul}' tidak tersedia");
        }
        $this->_stok--;
    }

    public function kembalikan(): void {
        $this->_stok++;
    }

    public function formatHarga(): string {
        return 'Rp ' . number_format($this->harga, 0, ',', '.');
    }

    // Static factory method
    public static function fromArray(array $data): self {
        return new self(
            judul:     $data['judul'],
            pengarang: $data['pengarang'],
            tahun:     (int) $data['tahun'],
            harga:     (float) $data['harga'],
            stok:      (int) $data['stok'],
            kategori:  $data['kategori'],
            isbn:      $data['isbn'] ?? null,
            deskripsi: $data['deskripsi'] ?? null,
            id:        (int) ($data['id'] ?? 0),
        );
    }

    public function toArray(): array {
        return [
            'id'        => $this->id,
            'judul'     => $this->judul,
            'pengarang' => $this->pengarang,
            'isbn'      => $this->isbn,
            'tahun'     => $this->tahun,
            'harga'     => $this->harga,
            'stok'      => $this->_stok,
            'kategori'  => $this->kategori,
            'deskripsi' => $this->deskripsi,
        ];
    }
}
```

#### [[19. Inheritance, Interface, dan Trait]]

PHP

```
<?php
// File: src/Models/User.php

abstract class User {
    public function __construct(
        protected readonly int $id,
        protected string $nama,
        protected string $email,
        protected string $passwordHash,
    ) {}

    // Method konkret — sudah ada implementasi
    public function verifyPassword(string $password): bool {
        return password_verify($password, $this->passwordHash);
    }

    public function getId(): int   { return $this->id; }
    public function getNama(): string  { return $this->nama; }
    public function getEmail(): string { return $this->email; }

    // Method abstract — HARUS diimplementasikan child class
    abstract public function getRole(): string;
    abstract public function getBatasPinjam(): int;
}

// File: src/Models/Admin.php
class Admin extends User {
    public function getRole(): string     { return 'admin'; }
    public function getBatasPinjam(): int { return PHP_INT_MAX; }
}

// File: src/Models/Anggota.php
class Anggota extends User {
    private array $pinjaman = [];

    public function getRole(): string     { return 'anggota'; }
    public function getBatasPinjam(): int { return 5; }

    public function pinjamBuku(Buku $buku): bool {
        if (count($this->pinjaman) >= $this->getBatasPinjam()) {
            throw new \RuntimeException("Batas peminjaman sudah tercapai");
        }
        $buku->pinjam();
        $this->pinjaman[] = $buku;
        return true;
    }
}

// ─── Interface ────────────────────────────────────────────────────────────
interface Searchable {
    public function cari(string $keyword): array;
}

interface Exportable {
    public function toCsv(): string;
    public function toJson(): string;
}

// ─── Trait ───────────────────────────────────────────────────────────────
trait HasTimestamps {
    private ?\DateTimeImmutable $createdAt = null;
    private ?\DateTimeImmutable $updatedAt = null;

    public function setCreatedAt(): void {
        $this->createdAt = new \DateTimeImmutable();
    }

    public function touch(): void {
        $this->updatedAt = new \DateTimeImmutable();
    }

    public function getCreatedAt(): ?\DateTimeImmutable { return $this->createdAt; }
    public function getUpdatedAt(): ?\DateTimeImmutable { return $this->updatedAt; }
}

trait HasSoftDelete {
    private ?\DateTimeImmutable $deletedAt = null;

    public function softDelete(): void {
        $this->deletedAt = new \DateTimeImmutable();
    }

    public function restore(): void {
        $this->deletedAt = null;
    }

    public function isDeleted(): bool {
        return $this->deletedAt !== null;
    }
}
```

---

### K. Database MySQL dengan PDO

> 💡 **Mengapa PDO bukan MySQLi?** PDO (PHP Data Objects) mendukung banyak database driver (MySQL, PostgreSQL, SQLite). MySQLi hanya untuk MySQL. PDO lebih fleksibel dan ergonomis untuk prepared statements.

#### [[20. Setup Database dan Koneksi PDO]]

SQL

```
-- Buat database dan tabel di phpMyAdmin atau MySQL CLI
CREATE DATABASE perpustakaan_db
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;

USE perpustakaan_db;

CREATE TABLE buku (
    id          INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    judul       VARCHAR(200) NOT NULL,
    pengarang   VARCHAR(100) NOT NULL,
    isbn        CHAR(13) UNIQUE,
    penerbit    VARCHAR(100),
    tahun       YEAR NOT NULL,
    harga       DECIMAL(12, 2) NOT NULL DEFAULT 0,
    stok        INT UNSIGNED NOT NULL DEFAULT 0,
    kategori    ENUM('Fiksi','Non-Fiksi','Sains','Teknologi','Sejarah','Umum') DEFAULT 'Umum',
    deskripsi   TEXT,
    sampul      VARCHAR(255),
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    deleted_at  TIMESTAMP NULL DEFAULT NULL,
    INDEX idx_judul     (judul),
    INDEX idx_pengarang (pengarang),
    INDEX idx_kategori  (kategori)
);

CREATE TABLE anggota (
    id              INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nomor_anggota   VARCHAR(20) UNIQUE NOT NULL,
    nama            VARCHAR(100) NOT NULL,
    email           VARCHAR(150) UNIQUE NOT NULL,
    password_hash   VARCHAR(255) NOT NULL,
    telepon         VARCHAR(15),
    alamat          TEXT,
    role            ENUM('admin','pustakawan','anggota') DEFAULT 'anggota',
    status          ENUM('aktif','nonaktif','suspended') DEFAULT 'aktif',
    created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

CREATE TABLE peminjaman (
    id              INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    buku_id         INT UNSIGNED NOT NULL,
    anggota_id      INT UNSIGNED NOT NULL,
    tanggal_pinjam  DATE NOT NULL,
    batas_kembali   DATE NOT NULL,
    tanggal_kembali DATE NULL,
    denda           DECIMAL(10,2) DEFAULT 0,
    status          ENUM('dipinjam','dikembalikan','terlambat') DEFAULT 'dipinjam',
    catatan         TEXT,
    created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (buku_id)    REFERENCES buku(id)    ON UPDATE CASCADE ON DELETE RESTRICT,
    FOREIGN KEY (anggota_id) REFERENCES anggota(id) ON UPDATE CASCADE ON DELETE RESTRICT,
    INDEX idx_anggota_status (anggota_id, status),
    INDEX idx_tanggal_pinjam (tanggal_pinjam)
);
```

PHP

```
<?php
// File: src/Database/Connection.php

class Connection {
    private static ?PDO $instance = null;

    private function __construct() {} // cegah instantiasi langsung
    private function __clone() {}     // cegah cloning

    public static function getInstance(): PDO {
        if (self::$instance === null) {
            self::$instance = self::buatKoneksi();
        }
        return self::$instance;
    }

    private static function buatKoneksi(): PDO {
        $host     = $_ENV['DB_HOST']     ?? 'localhost';
        $port     = $_ENV['DB_PORT']     ?? '3306';
        $dbname   = $_ENV['DB_NAME']     ?? 'perpustakaan_db';
        $username = $_ENV['DB_USER']     ?? 'root';
        $password = $_ENV['DB_PASSWORD'] ?? '';

        $dsn = "mysql:host=$host;port=$port;dbname=$dbname;charset=utf8mb4";

        try {
            $pdo = new PDO($dsn, $username, $password, [
                PDO::ATTR_ERRMODE            => PDO::ERRMODE_EXCEPTION,  // throw exception saat error
                PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,        // fetch sebagai associative array
                PDO::ATTR_EMULATE_PREPARES   => false,                   // PENTING: gunakan prepared statement native
                PDO::MYSQL_ATTR_INIT_COMMAND => "SET NAMES utf8mb4 COLLATE utf8mb4_unicode_ci",
            ]);
            return $pdo;
        } catch (PDOException $e) {
            // JANGAN tampilkan detail database error ke user di production!
            error_log("Database connection failed: " . $e->getMessage());
            throw new RuntimeException("Layanan database tidak tersedia saat ini");
        }
    }
}
```

#### [[21. Prepared Statements — Query yang Aman dari SQL Injection]]

PHP

```
<?php
// File: src/Repositories/BukuRepository.php

class BukuRepository {
    private PDO $db;

    public function __construct() {
        $this->db = Connection::getInstance();
    }

    // ─── READ ─────────────────────────────────────────────────────────────

    public function findAll(
        int $halaman = 1,
        int $perHalaman = 10,
        string $keyword = '',
        string $kategori = ''
    ): array {
        $offset = ($halaman - 1) * $perHalaman;
        $params = [];

        $sql = "SELECT * FROM buku WHERE deleted_at IS NULL";

        if ($keyword !== '') {
            $sql .= " AND (judul LIKE :keyword OR pengarang LIKE :keyword2)";
            $params[':keyword']  = "%{$keyword}%";
            $params[':keyword2'] = "%{$keyword}%";
        }

        if ($kategori !== '' && $kategori !== 'semua') {
            $sql .= " AND kategori = :kategori";
            $params[':kategori'] = $kategori;
        }

        $sql .= " ORDER BY judul ASC LIMIT :limit OFFSET :offset";

        $stmt = $this->db->prepare($sql);
        $stmt->bindValue(':limit', $perHalaman, PDO::PARAM_INT);
        $stmt->bindValue(':offset', $offset, PDO::PARAM_INT);

        foreach ($params as $key => $value) {
            $stmt->bindValue($key, $value, PDO::PARAM_STR);
        }

        $stmt->execute();
        return $stmt->fetchAll();
    }

    public function hitungTotal(string $keyword = '', string $kategori = ''): int {
        $params = [];
        $sql = "SELECT COUNT(*) FROM buku WHERE deleted_at IS NULL";

        if ($keyword !== '') {
            $sql .= " AND (judul LIKE :keyword OR pengarang LIKE :keyword2)";
            $params[':keyword']  = "%{$keyword}%";
            $params[':keyword2'] = "%{$keyword}%";
        }

        if ($kategori !== '' && $kategori !== 'semua') {
            $sql .= " AND kategori = :kategori";
            $params[':kategori'] = $kategori;
        }

        $stmt = $this->db->prepare($sql);
        $stmt->execute($params);
        return (int) $stmt->fetchColumn();
    }

    public function findById(int $id): ?array {
        // ❌ SALAH — SQL Injection vulnerability!
        // $sql = "SELECT * FROM buku WHERE id = $id";

        // ✅ BENAR — Prepared statement
        $stmt = $this->db->prepare(
            "SELECT * FROM buku WHERE id = :id AND deleted_at IS NULL"
        );
        $stmt->execute([':id' => $id]);
        $result = $stmt->fetch();
        return $result !== false ? $result : null;
    }

    // ─── CREATE ───────────────────────────────────────────────────────────

    public function create(array $data): int {
        $stmt = $this->db->prepare(
            "INSERT INTO buku (judul, pengarang, isbn, tahun, harga, stok, kategori, deskripsi)
             VALUES (:judul, :pengarang, :isbn, :tahun, :harga, :stok, :kategori, :deskripsi)"
        );

        $stmt->execute([
            ':judul'     => $data['judul'],
            ':pengarang' => $data['pengarang'],
            ':isbn'      => $data['isbn'] ?? null,
            ':tahun'     => $data['tahun'],
            ':harga'     => $data['harga'],
            ':stok'      => $data['stok'],
            ':kategori'  => $data['kategori'],
            ':deskripsi' => $data['deskripsi'] ?? null,
        ]);

        return (int) $this->db->lastInsertId();
    }

    // ─── UPDATE ───────────────────────────────────────────────────────────

    public function update(int $id, array $data): bool {
        $stmt = $this->db->prepare(
            "UPDATE buku SET
                judul = :judul, pengarang = :pengarang, isbn = :isbn,
                tahun = :tahun, harga = :harga, stok = :stok,
                kategori = :kategori, deskripsi = :deskripsi,
                updated_at = NOW()
             WHERE id = :id AND deleted_at IS NULL"
        );

        $stmt->execute([
            ':judul'     => $data['judul'],
            ':pengarang' => $data['pengarang'],
            ':isbn'      => $data['isbn'] ?? null,
            ':tahun'     => $data['tahun'],
            ':harga'     => $data['harga'],
            ':stok'      => $data['stok'],
            ':kategori'  => $data['kategori'],
            ':deskripsi' => $data['deskripsi'] ?? null,
            ':id'        => $id,
        ]);

        return $stmt->rowCount() > 0;
    }

    // ─── DELETE (Soft Delete) ─────────────────────────────────────────────

    public function delete(int $id): bool {
        $stmt = $this->db->prepare(
            "UPDATE buku SET deleted_at = NOW() WHERE id = :id AND deleted_at IS NULL"
        );
        $stmt->execute([':id' => $id]);
        return $stmt->rowCount() > 0;
    }

    // ─── Transactions ─────────────────────────────────────────────────────

    public function pinjamBuku(int $bukuId, int $anggotaId): int {
        try {
            $this->db->beginTransaction();

            // Cek stok dengan locking
            $stmt = $this->db->prepare(
                "SELECT stok FROM buku WHERE id = :id AND deleted_at IS NULL FOR UPDATE"
            );
            $stmt->execute([':id' => $bukuId]);
            $buku = $stmt->fetch();

            if (!$buku || $buku['stok'] < 1) {
                throw new \RuntimeException("Stok buku tidak tersedia");
            }

            // Kurangi stok
            $this->db->prepare(
                "UPDATE buku SET stok = stok - 1 WHERE id = :id"
            )->execute([':id' => $bukuId]);

            // Buat record peminjaman
            $stmt = $this->db->prepare(
                "INSERT INTO peminjaman (buku_id, anggota_id, tanggal_pinjam, batas_kembali)
                 VALUES (:buku_id, :anggota_id, CURDATE(), DATE_ADD(CURDATE(), INTERVAL 14 DAY))"
            );
            $stmt->execute([':buku_id' => $bukuId, ':anggota_id' => $anggotaId]);

            $peminjamanId = (int) $this->db->lastInsertId();
            $this->db->commit();

            return $peminjamanId;

        } catch (\Exception $e) {
            $this->db->rollBack();
            throw $e;
        }
    }
}
```

---

### 🏗️ Checkpoint Level 4

text

```
✅ Checklist sebelum lanjut ke Level 5:

PROYEK: Sistem Perpustakaan dengan Database MySQL
├── Database: tabel buku, anggota, peminjaman dengan relasi
├── Connection.php: Singleton PDO dengan error handling
├── BukuRepository.php: CRUD dengan prepared statements
├── AnggotaRepository.php: CRUD anggota
├── PeminjamanRepository.php: CRUD + transaction peminjaman
├── Halaman CRUD buku: semua query via prepared statement
├── Pagination berfungsi: query LIMIT dan OFFSET
└── Soft delete: deleted_at diisi, data tidak benar-benar dihapus

KEAMANAN DATABASE:
├── PDO::ATTR_EMULATE_PREPARES = false (wajib!)
├── Semua query menggunakan prepared statement
├── Tidak ada query yang dibangun dari string concatenation input user
├── Transaction untuk operasi yang harus atomic
└── Error database tidak ditampilkan ke user

Git: feat: implement OOP models, PDO connection, and CRUD with database
```

---

## 🔴 LEVEL 5: KEAMANAN KOMPREHENSIF DAN AUTENTIKASI (Minggu 15-20)

> **Tema**: _"Dari aplikasi yang bekerja ke aplikasi yang aman"_  
> **Benang Merah**: Database berjalan (Level 4) → keamanan menyeluruh → autentikasi yang proper → otorisasi berbasis role → proteksi dari semua vektor serangan umum  
> **Output**: Aplikasi perpustakaan yang aman dari semua serangan web umum

---

### L. Keamanan Web PHP — Komprehensif

> 💡 **Mengapa satu level penuh untuk security?** Karena security bukan fitur yang ditambahkan — security adalah fondasi. Satu celah bisa membahayakan seluruh data pengguna.

text

```
Benang Merah Bagian L:
Database berjalan (Level 4) →
XSS: output yang tidak di-escape → berbahaya →
SQL Injection: query yang tidak di-prepared → berbahaya →
CSRF: request palsu dari situs lain → berbahaya →
File upload: file berbahaya yang diupload → berbahaya →
Password: penyimpanan yang salah → berbahaya →
Semua vektor serangan ini HARUS ditutup
```

#### [[22. XSS, SQL Injection, CSRF — Tiga Ancaman Utama]]

PHP

```
<?php
// ─── 1. XSS (Cross-Site Scripting) Prevention ───────────────────────────

// ❌ BERBAHAYA: langsung output input user
echo $_GET['nama'];  // User bisa input: <script>alert('XSS')</script>

// ✅ AMAN: selalu escape output
echo htmlspecialchars($_GET['nama'] ?? '', ENT_QUOTES | ENT_HTML5, 'UTF-8');

// Buat helper function dan gunakan KONSISTEN
function esc(mixed $nilai, int $flags = ENT_QUOTES | ENT_HTML5): string {
    return htmlspecialchars((string) $nilai, $flags, 'UTF-8');
}

// Aturan escape berdasarkan konteks:
// Di dalam tag HTML: esc($nilai)
// Di dalam attribute HTML: esc($nilai) — sama
// Di dalam JavaScript: json_encode($nilai, JSON_HEX_TAG | JSON_HEX_APOS | JSON_HEX_QUOT)
// Di dalam URL: urlencode($nilai) atau rawurlencode($nilai)
// Di dalam CSS: hindari user input di CSS

// Untuk output di JavaScript (misal: data untuk JS):
$dataUntukJs = json_encode($dataBuku, JSON_HEX_TAG | JSON_HEX_APOS | JSON_HEX_QUOT | JSON_HEX_AMP);
echo "<script>const dataBuku = {$dataUntukJs};</script>";

// Content Security Policy header — lapisan keamanan tambahan
header("Content-Security-Policy: default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'");

// ─── 2. SQL Injection Prevention ──────────────────────────────────────────
// SELALU gunakan prepared statements (sudah dibahas di Level 4)
// Tidak ada pengecualian — bahkan untuk integer!

// ❌ SALAH
$id = $_GET['id'];
$sql = "SELECT * FROM buku WHERE id = $id";

// ✅ BENAR
$id = (int) ($_GET['id'] ?? 0);
$stmt = $pdo->prepare("SELECT * FROM buku WHERE id = :id");
$stmt->execute([':id' => $id]);

// ─── 3. CSRF Prevention ───────────────────────────────────────────────────
// Sudah dibahas di Level 3, pastikan konsisten diterapkan

// Headers keamanan tambahan yang wajib:
function setSecurityHeaders(): void {
    // Cegah browser menebak tipe konten
    header('X-Content-Type-Options: nosniff');

    // Cegah clickjacking
    header('X-Frame-Options: DENY');

    // XSS Protection (untuk browser lama)
    header('X-XSS-Protection: 1; mode=block');

    // Referrer Policy
    header('Referrer-Policy: strict-origin-when-cross-origin');

    // HTTPS only (production):
    // header('Strict-Transport-Security: max-age=31536000; includeSubDomains');
}
```

#### [[23. Autentikasi yang Aman — Password Hashing dan Rate Limiting]]

PHP

```
<?php
// ─── Password Hashing ─────────────────────────────────────────────────────

// JANGAN PERNAH simpan password plain text atau dengan MD5/SHA1!
// Gunakan password_hash() dengan PASSWORD_BCRYPT atau PASSWORD_ARGON2ID

// Saat user mendaftar / set password:
function hashPassword(string $password): string {
    return password_hash($password, PASSWORD_BCRYPT, ['cost' => 12]);
    // cost: 12 = default yang baik, lebih tinggi = lebih aman tapi lebih lambat
}

// Saat user login:
function verifyPassword(string $password, string $hash): bool {
    return password_verify($password, $hash);
}

// Cek apakah hash perlu diupdate (jika cost berubah):
function passwordPerluRehash(string $hash): bool {
    return password_needs_rehash($hash, PASSWORD_BCRYPT, ['cost' => 12]);
}

// ─── Rate Limiting — batasi percobaan login ──────────────────────────────

class RateLimiter {
    private PDO $db;
    private int $maxPercobaan;
    private int $windowDetik;

    public function __construct(PDO $db, int $maxPercobaan = 5, int $windowDetik = 900) {
        $this->db = $db;
        $this->maxPercobaan = $maxPercobaan;
        $this->windowDetik  = $windowDetik;
    }

    public function cekLimit(string $identifier): bool {
        // identifier: IP address atau email
        $stmt = $this->db->prepare(
            "SELECT COUNT(*) FROM login_attempts
             WHERE identifier = :id AND attempted_at > DATE_SUB(NOW(), INTERVAL :window SECOND)"
        );
        $stmt->execute([':id' => $identifier, ':window' => $this->windowDetik]);
        $count = (int) $stmt->fetchColumn();

        return $count < $this->maxPercobaan;
    }

    public function catatGagal(string $identifier): void {
        $stmt = $this->db->prepare(
            "INSERT INTO login_attempts (identifier, attempted_at) VALUES (:id, NOW())"
        );
        $stmt->execute([':id' => $identifier]);
    }

    public function resetLimit(string $identifier): void {
        $stmt = $this->db->prepare(
            "DELETE FROM login_attempts WHERE identifier = :id"
        );
        $stmt->execute([':id' => $identifier]);
    }

    public function sisaWaktu(string $identifier): int {
        // Berapa detik lagi user bisa coba login
        $stmt = $this->db->prepare(
            "SELECT TIMESTAMPDIFF(SECOND, MIN(attempted_at), NOW()) as elapsed
             FROM login_attempts WHERE identifier = :id"
        );
        $stmt->execute([':id' => $identifier]);
        $elapsed = (int) $stmt->fetchColumn();
        return max(0, $this->windowDetik - $elapsed);
    }
}

// ─── Implementasi login dengan rate limiting ─────────────────────────────

function prosesLogin(string $email, string $password, PDO $db): array {
    $ip = $_SERVER['REMOTE_ADDR'];
    $rateLimiter = new RateLimiter($db);

    // Cek rate limit berdasarkan IP dan email
    if (!$rateLimiter->cekLimit($ip) || !$rateLimiter->cekLimit($email)) {
        $sisa = $rateLimiter->sisaWaktu($ip);
        return [
            'sukses' => false,
            'pesan'  => "Terlalu banyak percobaan login. Coba lagi dalam " . ceil($sisa / 60) . " menit.",
        ];
    }

    // Ambil user dari database
    $stmt = $db->prepare(
        "SELECT * FROM anggota WHERE email = :email AND status = 'aktif' LIMIT 1"
    );
    $stmt->execute([':email' => strtolower(trim($email))]);
    $user = $stmt->fetch();

    if (!$user || !password_verify($password, $user['password_hash'])) {
        $rateLimiter->catatGagal($ip);
        $rateLimiter->catatGagal($email);
        return ['sukses' => false, 'pesan' => 'Email atau password salah.'];
    }

    // Login berhasil
    $rateLimiter->resetLimit($ip);
    $rateLimiter->resetLimit($email);

    // Rehash password jika perlu (cost berubah)
    if (password_needs_rehash($user['password_hash'], PASSWORD_BCRYPT, ['cost' => 12])) {
        $newHash = password_hash($password, PASSWORD_BCRYPT, ['cost' => 12]);
        $db->prepare("UPDATE anggota SET password_hash = :hash WHERE id = :id")
           ->execute([':hash' => $newHash, ':id' => $user['id']]);
    }

    return ['sukses' => true, 'user' => $user];
}
```

#### [[24. File Upload yang Aman]]

PHP

```
<?php
// File upload adalah salah satu vektor serangan yang paling sering dieksploitasi

function prosesUploadSampul(array $file, int $bukuId): string {
    // ─── Validasi ──────────────────────────────────────────────────────────

    // 1. Cek error upload
    if ($file['error'] !== UPLOAD_ERR_OK) {
        $pesanError = match($file['error']) {
            UPLOAD_ERR_INI_SIZE   => 'File terlalu besar (melebihi batas server)',
            UPLOAD_ERR_FORM_SIZE  => 'File terlalu besar (melebihi batas form)',
            UPLOAD_ERR_PARTIAL    => 'Upload tidak lengkap',
            UPLOAD_ERR_NO_FILE    => 'Tidak ada file yang dipilih',
            default               => 'Error upload tidak diketahui',
        };
        throw new \RuntimeException($pesanError);
    }

    // 2. Cek ukuran file (maksimal 2MB)
    $maxSize = 2 * 1024 * 1024; // 2MB dalam bytes
    if ($file['size'] > $maxSize) {
        throw new \RuntimeException("File terlalu besar. Maksimal 2MB.");
    }

    // 3. Cek tipe file YANG SEBENARNYA (bukan dari nama file!)
    // Jangan percaya $_FILES['type'] — user bisa manipulasi!
    $finfo = new \finfo(FILEINFO_MIME_TYPE);
    $mimeType = $finfo->file($file['tmp_name']);

    $mimeTypeValid = ['image/jpeg', 'image/png', 'image/webp', 'image/gif'];
    if (!in_array($mimeType, $mimeTypeValid, true)) {
        throw new \RuntimeException("Tipe file tidak valid. Hanya JPG, PNG, WebP, GIF.");
    }

    // 4. Cek ekstensi yang diizinkan
    $ekstensiValid = ['jpg', 'jpeg', 'png', 'webp', 'gif'];
    $ekstensiAsli  = strtolower(pathinfo($file['name'], PATHINFO_EXTENSION));
    if (!in_array($ekstensiAsli, $ekstensiValid, true)) {
        throw new \RuntimeException("Ekstensi file tidak valid.");
    }

    // ─── Proses Upload ─────────────────────────────────────────────────────

    // 5. Generate nama file yang aman (JANGAN gunakan nama asli dari user!)
    $namaFile  = sprintf('buku_%d_%s.%s', $bukuId, bin2hex(random_bytes(8)), $ekstensiAsli);

    // 6. Simpan di luar web root jika butuh akses kontrol
    // Atau di dalam web root untuk akses langsung
    $uploadDir = __DIR__ . '/../../storage/sampul/';

    // Pastikan direktori ada dan aman
    if (!is_dir($uploadDir)) {
        mkdir($uploadDir, 0755, true);
    }

    $tujuan = $uploadDir . $namaFile;

    // 7. Gunakan move_uploaded_file (bukan copy!) — validasi bahwa file hasil upload
    if (!move_uploaded_file($file['tmp_name'], $tujuan)) {
        throw new \RuntimeException("Gagal menyimpan file.");
    }

    // 8. Set permission yang restrictive
    chmod($tujuan, 0644);

    return $namaFile;
}
```

---

### 🏗️ Checkpoint Level 5

text

```
✅ Checklist sebelum lanjut ke Level 6:

KEAMANAN (semua WAJIB):
├── Semua output: htmlspecialchars() / esc()
├── Semua query: prepared statement
├── Semua form POST: CSRF token
├── Password: password_hash() BCRYPT
├── Login: rate limiting aktif
├── File upload: validasi MIME type, bukan nama file
├── Security headers: X-Content-Type-Options, X-Frame-Options
└── Error: tidak ada detail error yang bocor ke user

OTORISASI:
├── requireLogin() di semua halaman protected
├── Cek role (admin/pustakawan/anggota) untuk aksi sensitif
├── User A tidak bisa akses/edit data User B
└── Soft delete: data penting tidak pernah benar-benar dihapus

Git: feat: implement comprehensive security, auth, and authorization
```

---

## ⚫ LEVEL 6: ARSITEKTUR MVC DAN REST API (Minggu 20-26)

> **Tema**: _"Dari kode yang tersebar ke arsitektur yang terstruktur dan scalable"_  
> **Benang Merah**: Kode di setiap file (Level 1-5) → MVC memisahkan concerns → Router mengarahkan request → API endpoint mengembalikan JSON → Composer untuk autoloading  
> **Output**: Aplikasi MVC yang terstruktur dan REST API perpustakaan

---

### M. Arsitektur MVC — Pemisahan Concerns

> 💡 **Mengapa MVC?** Tanpa struktur, project besar menjadi "spaghetti code" — logika, tampilan, dan akses data semua bercampur. MVC memisahkan ketiganya sehingga kode lebih mudah dipahami, ditest, dan dikembangkan.

text

```
Benang Merah Bagian M:
Kode tersebar di banyak file tanpa struktur jelas (Level 1-5) →
Model: interaksi dengan database dan business logic →
View: tampilan HTML/JSON →
Controller: koordinator antara Model dan View →
Router: arahkan request ke Controller yang tepat →
Composer: autoloading PSR-4 agar require manual tidak diperlukan
```

#### [[25. Setup Project dengan Composer dan Autoloading]]

JSON

```
// composer.json
{
    "name": "perpustakaan/sistem",
    "description": "Sistem Manajemen Perpustakaan PHP Native",
    "type": "project",
    "require": {
        "php": ">=8.2",
        "vlucas/phpdotenv": "^5.6"
    },
    "require-dev": {
        "phpunit/phpunit": "^11.0",
        "squizlabs/php_codesniffer": "^3.0",
        "phpstan/phpstan": "^1.0"
    },
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        }
    },
    "autoload-dev": {
        "psr-4": {
            "Tests\\": "tests/"
        }
    },
    "scripts": {
        "start": "php -S localhost:8000 -t public/",
        "test": "phpunit",
        "analyse": "phpstan analyse src/ --level=6",
        "lint": "phpcs src/ --standard=PSR12"
    }
}
```

text

```
Struktur Project MVC:
perpustakaan/
├── public/                    ← Web root (hanya ini yang diekspos)
│   ├── index.php              ← Front controller — semua request masuk ke sini
│   ├── .htaccess              ← URL rewriting untuk Apache
│   ├── css/
│   ├── js/
│   └── images/
│
├── src/                       ← Kode aplikasi (PSR-4 autoloaded)
│   ├── Controllers/
│   │   ├── BukuController.php
│   │   ├── AnggotaController.php
│   │   └── Api/
│   │       └── BukuApiController.php
│   ├── Models/
│   │   ├── Buku.php
│   │   └── Anggota.php
│   ├── Repositories/
│   │   ├── BukuRepository.php
│   │   └── AnggotaRepository.php
│   ├── Views/
│   │   ├── layouts/
│   │   │   └── app.php
│   │   ├── buku/
│   │   │   ├── index.php
│   │   │   ├── show.php
│   │   │   ├── create.php
│   │   │   └── edit.php
│   │   └── errors/
│   │       ├── 404.php
│   │       └── 500.php
│   ├── Core/
│   │   ├── Router.php
│   │   ├── Request.php
│   │   ├── Response.php
│   │   ├── View.php
│   │   └── Container.php      ← Dependency Injection Container
│   ├── Middleware/
│   │   ├── AuthMiddleware.php
│   │   └── CsrfMiddleware.php
│   └── Exceptions/
│       ├── NotFoundException.php
│       └── ForbiddenException.php
│
├── tests/
│   ├── Unit/
│   └── Feature/
├── storage/
│   ├── logs/
│   └── uploads/
├── .env
├── .env.example
├── .gitignore
└── composer.json
```

#### [[26. Router dan Front Controller]]

apache

```
# public/.htaccess — URL rewriting untuk Apache
Options -MultiViews -Indexes
RewriteEngine On

# Arahkan semua request ke index.php (kecuali file/folder yang ada)
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^ index.php [QSA,L]
```

PHP

```
<?php
// public/index.php — Front Controller

// Semua request masuk ke satu titik ini
// Ini adalah pola Front Controller yang dipakai semua framework modern

define('ROOT_PATH', dirname(__DIR__));
define('START_TIME', microtime(true));

// Autoloading via Composer
require ROOT_PATH . '/vendor/autoload.php';

// Load environment variables
$dotenv = Dotenv\Dotenv::createImmutable(ROOT_PATH);
$dotenv->safeLoad();

// Error handling berdasarkan environment
if ($_ENV['APP_ENV'] === 'production') {
    error_reporting(0);
    ini_set('display_errors', '0');
} else {
    error_reporting(E_ALL);
    ini_set('display_errors', '1');
}

use App\Core\Router;
use App\Core\Request;

// Buat instance router
$router = new Router();

// ─── Definisi Routes ──────────────────────────────────────────────────────
$router->get('/', 'HomeController@index');

// Web routes — buku
$router->get('/buku', 'BukuController@index');
$router->get('/buku/tambah', 'BukuController@create');
$router->post('/buku', 'BukuController@store');
$router->get('/buku/{id}', 'BukuController@show');
$router->get('/buku/{id}/edit', 'BukuController@edit');
$router->put('/buku/{id}', 'BukuController@update');
$router->delete('/buku/{id}', 'BukuController@destroy');

// Auth routes
$router->get('/login', 'AuthController@showLogin');
$router->post('/login', 'AuthController@login');
$router->post('/logout', 'AuthController@logout');

// API routes
$router->get('/api/buku', 'Api\BukuApiController@index');
$router->get('/api/buku/{id}', 'Api\BukuApiController@show');
$router->post('/api/buku', 'Api\BukuApiController@store');

// ─── Dispatch request ─────────────────────────────────────────────────────
try {
    $request = new Request();
    $router->dispatch($request);
} catch (\App\Exceptions\NotFoundException $e) {
    http_response_code(404);
    include ROOT_PATH . '/src/Views/errors/404.php';
} catch (\App\Exceptions\ForbiddenException $e) {
    http_response_code(403);
    include ROOT_PATH . '/src/Views/errors/403.php';
} catch (\Throwable $e) {
    error_log($e->getMessage() . "\n" . $e->getTraceAsString());
    http_response_code(500);
    if ($_ENV['APP_ENV'] !== 'production') {
        throw $e; // tampilkan di development
    }
    include ROOT_PATH . '/src/Views/errors/500.php';
}
```

PHP

```
<?php
// src/Core/Router.php

namespace App\Core;

class Router {
    private array $routes = [];

    public function get(string $path, string $handler): void {
        $this->addRoute('GET', $path, $handler);
    }

    public function post(string $path, string $handler): void {
        $this->addRoute('POST', $path, $handler);
    }

    public function put(string $path, string $handler): void {
        $this->addRoute('PUT', $path, $handler);
        $this->addRoute('POST', $path, $handler); // HTML form pakai method spoofing
    }

    public function delete(string $path, string $handler): void {
        $this->addRoute('DELETE', $path, $handler);
        $this->addRoute('POST', $path, $handler); // HTML form pakai method spoofing
    }

    private function addRoute(string $method, string $path, string $handler): void {
        // Konversi {id} ke regex pattern
        $pattern = preg_replace('/\{([a-z]+)\}/', '(?P<$1>[^/]+)', $path);
        $pattern = "@^{$pattern}$@";

        $this->routes[] = [
            'method'  => $method,
            'pattern' => $pattern,
            'handler' => $handler,
        ];
    }

    public function dispatch(Request $request): void {
        $method = $request->getMethod();
        $path   = $request->getPath();

        // HTML form method spoofing: <input type="hidden" name="_method" value="PUT">
        if ($method === 'POST' && isset($_POST['_method'])) {
            $method = strtoupper($_POST['_method']);
        }

        foreach ($this->routes as $route) {
            if ($route['method'] !== $method) continue;

            if (preg_match($route['pattern'], $path, $matches)) {
                // Ambil named captures (parameter route)
                $params = array_filter($matches, fn($k) => !is_int($k), ARRAY_FILTER_USE_KEY);

                // Resolve dan jalankan controller
                [$controllerName, $action] = explode('@', $route['handler']);
                $controllerClass = "App\\Controllers\\{$controllerName}";

                if (!class_exists($controllerClass)) {
                    throw new \RuntimeException("Controller {$controllerClass} tidak ditemukan");
                }

                $controller = new $controllerClass();

                if (!method_exists($controller, $action)) {
                    throw new \RuntimeException("Method {$action} tidak ada di {$controllerClass}");
                }

                $controller->$action($request, $params);
                return;
            }
        }

        throw new \App\Exceptions\NotFoundException("Route tidak ditemukan: {$method} {$path}");
    }
}
```

---

### N. REST API dengan PHP Native

#### [[27. Membangun REST API yang Proper]]

PHP

```
<?php
// src/Controllers/Api/BukuApiController.php

namespace App\Controllers\Api;

use App\Core\Request;
use App\Repositories\BukuRepository;

class BukuApiController {
    private BukuRepository $bukuRepo;

    public function __construct() {
        $this->bukuRepo = new BukuRepository();
        header('Content-Type: application/json; charset=utf-8');
    }

    // GET /api/buku
    public function index(Request $request): void {
        // Autentikasi API
        $this->requireApiToken();

        $halaman   = max(1, (int) ($request->getQuery('halaman') ?? 1));
        $perHalaman = min(100, max(1, (int) ($request->getQuery('per_halaman') ?? 15)));
        $keyword    = $request->getQuery('q') ?? '';
        $kategori   = $request->getQuery('kategori') ?? '';

        $buku  = $this->bukuRepo->findAll($halaman, $perHalaman, $keyword, $kategori);
        $total = $this->bukuRepo->hitungTotal($keyword, $kategori);

        $this->jsonResponse([
            'data' => $buku,
            'meta' => [
                'halaman_saat_ini' => $halaman,
                'per_halaman'      => $perHalaman,
                'total'            => $total,
                'total_halaman'    => (int) ceil($total / $perHalaman),
            ],
        ]);
    }

    // GET /api/buku/{id}
    public function show(Request $request, array $params): void {
        $id   = (int) ($params['id'] ?? 0);
        $buku = $this->bukuRepo->findById($id);

        if ($buku === null) {
            $this->jsonError("Buku tidak ditemukan", 404);
            return;
        }

        $this->jsonResponse(['data' => $buku]);
    }

    // POST /api/buku
    public function store(Request $request): void {
        $this->requireApiToken();
        $this->requireRole('admin');

        $body = $request->getJsonBody();

        // Validasi
        $errors = $this->validasiDataBuku($body);
        if (!empty($errors)) {
            $this->jsonError("Validasi gagal", 422, $errors);
            return;
        }

        try {
            $id = $this->bukuRepo->create($body);
            $buku = $this->bukuRepo->findById($id);

            $this->jsonResponse(['data' => $buku], 201);
        } catch (\Exception $e) {
            error_log($e->getMessage());
            $this->jsonError("Gagal menyimpan buku", 500);
        }
    }

    // ─── Helper methods ───────────────────────────────────────────────────

    private function requireApiToken(): void {
        $header = $_SERVER['HTTP_AUTHORIZATION'] ?? '';
        if (!preg_match('/^Bearer\s+(.+)$/', $header, $matches)) {
            $this->jsonError("Token autentikasi diperlukan", 401);
            exit;
        }
        // Verifikasi token di database
        // ...
    }

    private function requireRole(string $role): void {
        // Cek role user dari token
        // ...
    }

    private function validasiDataBuku(array $data): array {
        $errors = [];
        if (empty(trim($data['judul'] ?? ''))) {
            $errors['judul'] = 'Judul wajib diisi';
        }
        // ... validasi lainnya
        return $errors;
    }

    private function jsonResponse(array $data, int $statusCode = 200): void {
        http_response_code($statusCode);
        echo json_encode([
            'success' => true,
            ...$data,
        ], JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
        exit;
    }

    private function jsonError(string $pesan, int $statusCode = 400, array $errors = []): void {
        http_response_code($statusCode);
        $response = ['success' => false, 'message' => $pesan];
        if (!empty($errors)) $response['errors'] = $errors;
        echo json_encode($response, JSON_UNESCAPED_UNICODE);
        exit;
    }
}
```

---

### 🏗️ Checkpoint Level 6

text

```
✅ Checklist sebelum lanjut ke Level 7:

ARSITEKTUR MVC:
├── Composer autoloading PSR-4 berfungsi
├── Front controller: semua request ke public/index.php
├── Router: mengarahkan ke Controller yang tepat
├── Controller: tipis — hanya koordinasi
├── Repository: semua query database
├── View: hanya presentasi, tidak ada logika bisnis
└── .htaccess: URL rewriting berfungsi

REST API:
├── GET /api/buku: daftar buku dengan paginasi
├── GET /api/buku/{id}: detail buku
├── POST /api/buku: tambah buku (auth required)
├── PUT /api/buku/{id}: update buku
├── DELETE /api/buku/{id}: hapus buku
├── Semua response JSON konsisten
├── Status code yang benar (200, 201, 400, 401, 404, 422, 500)
└── Test dengan Postman atau curl

Git: feat: implement MVC architecture, Router, and REST API
```

---

## 🟣 LEVEL 7: TESTING, OPTIMASI, DAN DEPLOYMENT (Minggu 26+)

> **Tema**: _"Dari aplikasi yang bekerja ke aplikasi yang bisa dipercaya dan siap production"_  
> **Benang Merah**: Aplikasi lengkap (Level 6) → testing memastikan tetap bekerja → optimasi untuk performa → deployment ke server  
> **Output**: Aplikasi perpustakaan yang tested, optimal, dan berjalan di server production

---

### O. Testing dengan PHPUnit

#### [[28. Unit Testing dan Feature Testing]]

Bash

```
# Install PHPUnit via Composer
composer require --dev phpunit/phpunit

# Buat phpunit.xml
```

XML

```
<!-- phpunit.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<phpunit xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="vendor/phpunit/phpunit/phpunit.xsd"
         bootstrap="vendor/autoload.php"
         colors="true"
         stopOnFailure="false">

    <testsuites>
        <testsuite name="Unit">
            <directory>tests/Unit</directory>
        </testsuite>
        <testsuite name="Feature">
            <directory>tests/Feature</directory>
        </testsuite>
    </testsuites>

    <coverage>
        <include>
            <directory suffix=".php">src</directory>
        </include>
        <report>
            <html outputDirectory="coverage"/>
        </report>
    </coverage>

    <php>
        <env name="APP_ENV" value="testing"/>
        <env name="DB_NAME" value="perpustakaan_test"/>
    </php>
</phpunit>
```

PHP

```
<?php
// tests/Unit/BukuTest.php

namespace Tests\Unit;

use App\Models\Buku;
use PHPUnit\Framework\TestCase;
use PHPUnit\Framework\Attributes\Test;
use PHPUnit\Framework\Attributes\DataProvider;

class BukuTest extends TestCase {

    private Buku $buku;

    protected function setUp(): void {
        $this->buku = new Buku(
            judul: 'Clean Code',
            pengarang: 'Robert Martin',
            tahun: 2008,
            harga: 150000,
            stok: 5,
            kategori: 'Teknologi',
        );
    }

    #[Test]
    public function buku_tersedia_ketika_stok_lebih_dari_nol(): void {
        $this->assertTrue($this->buku->tersedia());
    }

    #[Test]
    public function buku_tidak_tersedia_ketika_stok_nol(): void {
        $buku = new Buku('Test', 'Author', 2024, 0, 0, 'Umum');
        $this->assertFalse($buku->tersedia());
    }

    #[Test]
    public function pinjam_mengurangi_stok(): void {
        $stokAwal = $this->buku->getStok();
        $this->buku->pinjam();
        $this->assertEquals($stokAwal - 1, $this->buku->getStok());
    }

    #[Test]
    public function pinjam_gagal_ketika_stok_nol(): void {
        $buku = new Buku('Test', 'Author', 2024, 0, 0, 'Umum');

        $this->expectException(\RuntimeException::class);
        $this->expectExceptionMessage('tidak tersedia');

        $buku->pinjam();
    }

    #[Test]
    public function kembalikan_menambah_stok(): void {
        $stokAwal = $this->buku->getStok();
        $this->buku->kembalikan();
        $this->assertEquals($stokAwal + 1, $this->buku->getStok());
    }

    #[Test]
    #[DataProvider('providerFormatHarga')]
    public function format_harga_benar(float $harga, string $expected): void {
        $buku = new Buku('Test', 'Author', 2024, $harga, 1, 'Umum');
        $this->assertEquals($expected, $buku->formatHarga());
    }

    public static function providerFormatHarga(): array {
        return [
            'nol'       => [0,        'Rp 0'],
            'seribu'    => [1000,     'Rp 1.000'],
            'standar'   => [150000,   'Rp 150.000'],
            'juta'      => [1500000,  'Rp 1.500.000'],
        ];
    }

    #[Test]
    public function set_stok_negatif_throw_exception(): void {
        $this->expectException(\InvalidArgumentException::class);
        $this->buku->setStok(-1);
    }
}
```

PHP

```
<?php
// tests/Unit/ValidatorTest.php

namespace Tests\Unit;

use App\Functions\Validator;
use PHPUnit\Framework\TestCase;
use PHPUnit\Framework\Attributes\Test;

class ValidatorTest extends TestCase {

    #[Test]
    public function required_gagal_untuk_string_kosong(): void {
        $validator = (new Validator(['nama' => '']))->required('nama', 'Nama');
        $this->assertTrue($validator->gagal());
        $this->assertArrayHasKey('nama', $validator->getErrors());
    }

    #[Test]
    public function required_berhasil_untuk_string_tidak_kosong(): void {
        $validator = (new Validator(['nama' => 'Budi']))->required('nama', 'Nama');
        $this->assertTrue($validator->berhasil());
    }

    #[Test]
    public function email_gagal_untuk_format_tidak_valid(): void {
        $inputs = ['bukan-email', '@tanpadomain', 'tanpa@', ''];
        foreach ($inputs as $input) {
            $validator = (new Validator(['email' => $input]))->email('email', 'Email');
            $this->assertTrue($validator->gagal(), "Seharusnya gagal untuk: $input");
        }
    }

    #[Test]
    public function integer_validasi_range(): void {
        $validator = (new Validator(['tahun' => '1000']))
            ->integer('tahun', 'Tahun', 1000, 2024);
        $this->assertTrue($validator->berhasil());

        $validator = (new Validator(['tahun' => '999']))
            ->integer('tahun', 'Tahun', 1000, 2024);
        $this->assertTrue($validator->gagal());
    }
}
```

Bash

```
# Jalankan tests
./vendor/bin/phpunit

# Jalankan test tertentu
./vendor/bin/phpunit tests/Unit/BukuTest.php
./vendor/bin/phpunit --filter=pinjam_mengurangi_stok

# Dengan coverage report
./vendor/bin/phpunit --coverage-html coverage/
# Buka: coverage/index.html di browser
```

---

### P. Optimasi Performa

#### [[29. Caching dan Optimasi Query]]

PHP

```
<?php
// src/Core/Cache.php — File-based cache sederhana

class Cache {
    private static string $cacheDir;

    public static function init(string $dir): void {
        self::$cacheDir = $dir;
        if (!is_dir($dir)) mkdir($dir, 0755, true);
    }

    public static function get(string $key): mixed {
        $file = self::cacheFile($key);

        if (!file_exists($file)) return null;

        $data = unserialize(file_get_contents($file));

        // Cek expiry
        if ($data['expiry'] !== null && time() > $data['expiry']) {
            unlink($file);
            return null;
        }

        return $data['value'];
    }

    public static function set(string $key, mixed $value, int $ttlDetik = 300): void {
        $data = [
            'value'  => $value,
            'expiry' => $ttlDetik > 0 ? time() + $ttlDetik : null,
        ];
        file_put_contents(self::cacheFile($key), serialize($data), LOCK_EX);
    }

    public static function remember(string $key, int $ttl, callable $callback): mixed {
        $cached = self::get($key);
        if ($cached !== null) return $cached;

        $value = $callback();
        self::set($key, $value, $ttl);
        return $value;
    }

    public static function forget(string $key): void {
        $file = self::cacheFile($key);
        if (file_exists($file)) unlink($file);
    }

    public static function flush(): void {
        foreach (glob(self::$cacheDir . '*.cache') as $file) {
            unlink($file);
        }
    }

    private static function cacheFile(string $key): string {
        return self::$cacheDir . md5($key) . '.cache';
    }
}

// Penggunaan di repository:
public function findAll(int $halaman = 1): array {
    $cacheKey = "buku_halaman_{$halaman}";

    return Cache::remember($cacheKey, 300, function() use ($halaman) {
        // Query ke database — hanya dijalankan jika tidak ada cache
        return $this->queryFindAll($halaman);
    });
}

// Invalidate cache saat data berubah:
public function create(array $data): int {
    $id = $this->queryCreate($data);
    Cache::flush(); // atau Cache::forget('buku_halaman_1'), dst.
    return $id;
}
```

#### [[30. Deployment ke VPS]]

Bash

```
# ─── Di SERVER (Ubuntu 22.04) ─────────────────────────────────────────────

# Install LEMP stack
sudo apt update && sudo apt upgrade -y
sudo apt install -y nginx mysql-server php8.3-fpm \
    php8.3-mysql php8.3-mbstring php8.3-xml \
    php8.3-curl php8.3-zip php8.3-intl

# Install Composer
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer

# Clone project
cd /var/www
sudo git clone https://github.com/username/perpustakaan.git
sudo chown -R www-data:www-data perpustakaan/
cd perpustakaan

# Install dependencies (production only, no dev)
composer install --optimize-autoloader --no-dev

# Setup environment
cp .env.example .env
# Edit .env: APP_ENV=production, APP_DEBUG=false, DB_*, dll.

# Generate APP_KEY jika ada
php artisan key:generate  # jika menggunakan key

# Setup database
mysql -u root -p -e "CREATE DATABASE perpustakaan_db CHARACTER SET utf8mb4"
php scripts/migrate.php  # jika ada migration script

# Permissions
sudo find storage/ -type d -exec chmod 755 {} \;
sudo find storage/ -type f -exec chmod 644 {} \;
sudo chown -R www-data:www-data storage/
```

nginx

```
# /etc/nginx/sites-available/perpustakaan
server {
    listen 80;
    server_name perpustakaan.kota.id www.perpustakaan.kota.id;

    root /var/www/perpustakaan/public;
    index index.php;

    # Keamanan
    server_tokens off;
    add_header X-Frame-Options "DENY" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;

    # Sembunyikan file sensitif
    location ~ /\.(env|git|htaccess|gitignore) {
        deny all;
        return 404;
    }

    # Blokir akses ke direktori selain public/
    location ~ ^/(src|tests|storage|vendor)/ {
        deny all;
        return 404;
    }

    # PHP-FPM
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
        fastcgi_hide_header X-Powered-By;
    }

    # Front Controller: semua request ke index.php
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    # Cache static assets
    location ~* \.(jpg|jpeg|png|gif|ico|css|js|woff2|svg)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

Bash

```
# Aktifkan site dan SSL
sudo ln -s /etc/nginx/sites-available/perpustakaan /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

# SSL dengan Let's Encrypt
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d perpustakaan.kota.id

# Auto-renew SSL
sudo crontab -e
# 0 0 * * * certbot renew --quiet
```

---

### 🏗️ Checkpoint Level 7 (Final)

text

```
✅ Checklist Akhir — Sistem Perpustakaan Production-Ready:

TESTING:
├── ./vendor/bin/phpunit: semua test hijau
├── Unit test: Buku, Validator, minimal 10 test case
├── Coverage: minimal 70% untuk Models dan Repositories
└── Test berjalan di CI (GitHub Actions)

OPTIMASI:
├── File-based cache untuk query yang berat
├── Lazy loading: tidak query relasi yang tidak diperlukan
├── Pagination: tidak pernah SELECT * tanpa LIMIT
└── Gzip compression aktif di Nginx

DEPLOYMENT:
├── Server: VPS Ubuntu dengan Nginx + PHP-FPM
├── SSL: HTTPS aktif dengan Let's Encrypt
├── .env: APP_DEBUG=false di production
├── Error: tidak ada detail error yang bocor
├── Security headers: semua terpasang di Nginx
└── File permissions: storage/ dimiliki www-data

GITHUB ACTIONS (CI/CD):
├── Trigger: setiap push ke main
├── Step: composer install → phpunit → deploy via SSH
└── Deploy hanya jika semua test lulus

Git: feat: production-ready system with testing, caching, and deployment
```

---

## 📊 Ringkasan Progress Tracking

### Satu Project, 7 Level Enhancement

text

```
Level 1: Halaman PHP perpustakaan — sintaks, variabel, output ke HTML
  + Level 2: + String, array, fungsi → katalog buku di memori
  + Level 3: + Form, session, file JSON → web app interaktif
  + Level 4: + OOP, PDO, MySQL → database yang persisten
  + Level 5: + Security menyeluruh, auth, otorisasi
  + Level 6: + MVC, Router, REST API → arsitektur yang scalable
  + Level 7: + PHPUnit, caching, deployment → production-ready
```

### Tabel Progress

|Level|Poin|Durasi|Output Konkret|
|---|---|---|---|
|🟢 **1**|1-8|Minggu 1-4|Halaman profil perpustakaan dinamis|
|🔵 **2**|9-12|Minggu 4-7|Katalog buku dengan array dan fungsi|
|🟡 **3**|13-17|Minggu 7-10|Web app dengan form, login, file JSON|
|🟠 **4**|18-21|Minggu 10-15|OOP + MySQL database + PDO|
|🔴 **5**|22-24|Minggu 15-20|Security komprehensif + auth|
|⚫ **6**|25-27|Minggu 20-26|MVC architecture + REST API|
|🟣 **7**|28-30|Minggu 26+|Testing + deployment + production|

---

### Benang Merah Utama Sepanjang Roadmap

text

```
Poin 1  (server-side concept)    → Fondasi pemahaman PHP seutuhnya
Poin 4  (error reporting)        → Kebiasaan debugging yang benar sejak awal
Poin 6  (type juggling)          → Mengapa SELALU pakai === bukan ==
Poin 7  (null coalescing ??)     → Dipakai di SETIAP pengambilan input
Poin 9  (string functions)       → Dasar semua pemrosesan teks
Poin 11 (array_filter + values)  → Pola yang salah sangat sering terjadi
Poin 12 (type hints)             → PHP 8 style — wajib di semua fungsi
Poin 13 (superglobal)            → Cara PHP menerima input dari browser
Poin 14 (sanitasi + CSRF)        → Security mindset dari hari pertama
Poin 15 (Validator class)        → Reusable di Level 3, 4, 5, 6
Poin 16 (session yang aman)      → session_regenerate_id, httponly cookie
Poin 17 (JsonStorage)            → Bridge antara file dan database
Poin 18 (OOP class)              → Fondasi semua Level 4+ dan framework
Poin 20 (PDO connection)         → Fondasi semua operasi database
Poin 21 (prepared statement)     → SQL injection prevention — TIDAK ADA PENGECUALIAN
Poin 22 (XSS + SQL + CSRF)       → Tiga ancaman utama yang harus selalu diingat
Poin 23 (password_hash)          → MD5 untuk password adalah kejahatan!
Poin 25 (Composer + PSR-4)       → Autoloading modern, siap untuk framework
Poin 26 (Router + Front Ctrl)    → Cara kerja semua framework PHP
Poin 27 (REST API)               → JSON API yang konsisten
Poin 28 (PHPUnit)                → Test sebelum deploy, selalu
Poin 30 (Nginx + SSL)            → Produksi yang aman dan cepat
```

---

## 💡 Cara Menggunakan Roadmap Ini

text

```
Setiap poin mengikuti format:
┌──────────────────────────────────────────────────────────┐
│ 💡 Konteks: mengapa fitur/konsep ini ada di PHP          │
│ 🔗 Benang Merah: koneksi ke poin sebelum dan sesudahnya  │
│ 📋 Kode: implementasi konkret di project perpustakaan    │
│          yang langsung bisa dicoba di server lokal       │
│ ✅ Langkah konkret: verifikasi berhasil                  │
└──────────────────────────────────────────────────────────┘
```

**Aturan yang Tidak Boleh Dilanggar:**

1. **`error_reporting(E_ALL)` selalu aktif** saat development — error adalah informasi berharga
2. **`===` bukan `==`** — type juggling adalah sumber bug tersembunyi terbesar di PHP
3. **`htmlspecialchars()` atau `esc()` untuk SETIAP output** ke HTML — zero XSS tolerance
4. **Prepared statement untuk SETIAP query** — zero SQL injection tolerance
5. **CSRF token di SETIAP form POST** — tanpa kecuali
6. **`password_hash()` untuk SEMUA password** — MD5 atau SHA1 untuk password adalah malpraktik
7. **PHP closing tag `?>` dihilangkan** di akhir file PHP murni — mencegah "headers already sent"
8. **Commit setelah setiap checkpoint** — git history adalah safety net
9. **Baca pesan error PHP dengan teliti** — pesan PHP sangat informatif
10. **Tidak ada kode production di web root** — hanya `public/` yang boleh diakses browser

---

_Roadmap PHP Native v1.0 — Step-by-Step, Security First, One Project_  
_Setiap baris kode ditulis dengan sadar — pahami mengapa sebelum bagaimana_