# 🗺️ Roadmap PHP: Step-by-Step Membangun Aplikasi Web Nyata

## Filosofi Roadmap Ini

> **"PHP bukan sekadar bahasa lama yang masih bertahan — PHP adalah bahasa yang telah berevolusi menjadi modern, type-safe, dan expressive"** — setiap konsep yang dipelajari ada alasannya, bukan sekadar hafal sintaks.

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: sistem manajemen perpustakaan dari CLI → web app → API → full application
- **Security dari Hari Pertama**: bukan topik lanjutan, tapi mindset yang dibangun sejak baris pertama
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **PHP Modern**: fokus pada PHP 8.x features, bukan cara lama yang sudah deprecated

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: "Perpustakaan CLI" — sintaks dasar, output ke browser
    ↓ (enhance)
Level 2: + String, array, fungsi → CRUD data buku di memori
    ↓ (enhance)
Level 3: + Form, session, file → Web app perpustakaan sederhana
    ↓ (enhance)
Level 4: + OOP, database MySQL → Sistem perpustakaan dengan database
    ↓ (enhance)
Level 5: + REST API, Composer, keamanan → API perpustakaan yang aman
    ↓ (enhance)
Level 6: + PHP 8 modern, design patterns, testing → Production-ready
    ↓ (pilih jalur)
Level 7: Laravel framework ATAU Symfony ATAU arsitektur advanced
```

---

## 🟢 LEVEL 1: FONDASI — PHP DAN CARA KERJANYA (Minggu 1-4)

> **Tema**: _"Dari nol ke halaman PHP pertama yang berjalan di server"_  
> **Benang Merah**: Cara PHP bekerja → Setup → Sintaks → Variabel → Tipe data → Output pertama  
> **Output**: Halaman web dinamis yang menampilkan informasi perpustakaan dengan PHP

---

### A. Cara PHP Bekerja & Setup

> 💡 **Mengapa dimulai di sini?** Berbeda dengan JavaScript yang berjalan di browser, PHP berjalan di server. Memahami siklus request-response mencegah kebingungan "kenapa perubahan tidak langsung terlihat" atau "kenapa kode PHP muncul di halaman".

text

```
Benang Merah Bagian A:
HTML & CSS sudah dipahami (dari roadmap sebelumnya) →
PHP: bahasa yang berjalan di SERVER, bukan di browser →
Browser kirim request → server proses PHP → kirim HTML ke browser →
Setup environment: XAMPP/Laragon →
File .php pertama → lihat hasilnya di browser
```

1. `[[1. PHP adalah Server-Side — Cara Kerja yang Fundamental]]`
    
    - PHP dieksekusi di server, hasilnya (HTML) dikirim ke browser
    - Browser tidak pernah melihat kode PHP — hanya hasilnya
    - Alur: `Browser` → request → `Web Server (Apache/Nginx)` → `PHP Engine` → `HTML` → `Browser`
    - Berbeda dengan JavaScript: PHP tidak bisa dijalankan di browser secara langsung
    - _Langkah konkret_: Buka `view-source:https://facebook.com` — tidak ada PHP, hanya HTML
    - Pertanyaan mental: "Kode ini dijalankan di mana?" → Jika PHP: di server. Jika JS: di browser.
2. `[[2. Setup Environment — XAMPP atau Laragon]]`
    
    - **Rekomendasi**: Laragon (Windows) atau Herd (macOS) — lebih modern dari XAMPP
    - **Alternatif klasik**: XAMPP — lebih familiar, banyak tutorial
    - Yang dibutuhkan: PHP engine + Apache/Nginx web server + MySQL database
    - Instalasi Laragon:
        - Download dari `laragon.org`
        - Install → Start All
        - Buat folder di `C:\laragon\www\perpustakaan\`
        - Akses di browser: `http://perpustakaan.test` (Laragon auto-detect)
    - Instalasi XAMPP:
        - Download dari `apachefriends.org`
        - Install → buka XAMPP Control Panel → Start Apache + MySQL
        - Buat folder di `C:\xampp\htdocs\perpustakaan\`
        - Akses: `http://localhost/perpustakaan/`
    - _Langkah konkret_: Buat `info.php` dengan `<?php phpinfo(); ?>` — pastikan halaman phpinfo muncul
3. `[[3. File .php Pertama — Hello World yang Sesungguhnya]]`
    
    - Buat `index.php`:
        
        PHP
        
        ```
        <?php
        // PHP tag pembuka — wajib ada
        // PHP tag penutup (?>) — opsional jika file murni PHP
        
        echo "Hello, Perpustakaan!";
        echo "<br>";
        echo "PHP Version: " . PHP_VERSION;
        echo "<br>";
        echo "Sekarang: " . date('d F Y, H:i:s');
        ?>
        ```
        
    - _Langkah konkret_: Buat file, buka di browser — pastikan teks muncul dengan tanggal yang benar
4. `[[4. Menggabungkan PHP dan HTML — Cara yang Benar]]`
    
    - PHP bisa disisipkan di dalam HTML:
        
        PHP
        
        ```
        <!DOCTYPE html>
        <html lang="id">
        <head>
          <meta charset="UTF-8">
          <title>Perpustakaan Digital</title>
        </head>
        <body>
          <h1>Selamat Datang di Perpustakaan Digital</h1>
          
          <?php
          $namaPerpustakaan = "Perpustakaan Kota";
          $jumlahBuku = 1250;
          $tanggalBuka = "Senin - Sabtu: 08:00 - 17:00";
          ?>
          
          <p>Nama: <?= $namaPerpustakaan ?></p>
          <p>Koleksi: <?= $jumlahBuku ?> buku</p>
          <p>Jam buka: <?= $tanggalBuka ?></p>
          
          <?php
          // Tag echo singkat: <?= ... ?> sama dengan <?php echo ... ?>
          // Gunakan untuk output sederhana di dalam HTML
          ?>
          
          <footer>
            <p>© <?= date('Y') ?> <?= $namaPerpustakaan ?></p>
          </footer>
        </body>
        </html>
        ```
        
    - _Langkah konkret_: Buat halaman profil perpustakaan dengan variabel yang ditampilkan di HTML
5. `[[5. Error Reporting — Baca Error, Jangan Takut]]`
    
    - Error adalah teman, bukan musuh — baca pesannya!
    - Setup untuk development — tambahkan di awal setiap file PHP atau di `php.ini`:
        
        PHP
        
        ```
        <?php
        // Tampilkan SEMUA error saat development
        error_reporting(E_ALL);
        ini_set('display_errors', 1);
        
        // ❌ Salah: ini menyembunyikan error
        // error_reporting(0);
        
        // Sengaja buat error untuk lihat tampilannya:
        echo $variabelYangBelumAda; // Undefined variable: Notice
        echo 1 / 0; // Division by zero: Warning
        // ambil_fungsi_tak_ada(); // Fatal Error
        ?>
        ```
        
    - Jenis error di PHP:
        - **Parse Error**: syntax salah → script tidak jalan sama sekali
        - **Fatal Error**: function/class tidak ada → script berhenti
        - **Warning**: ada masalah tapi script tetap jalan
        - **Notice**: potensi masalah kecil
    - _Langkah konkret_: Sengaja buat 3 jenis error berbeda, baca dan pahami pesan errornya

---

### B. Variabel & Tipe Data — Menyimpan dan Mengelola Data

> 💡 **Benang Merah ke A**: PHP sudah berjalan. Sekarang kita simpan data dalam variabel. PHP adalah dynamically typed — tipe data ditentukan saat runtime, bukan saat deklarasi (berbeda dengan Java/C#).

text

```
Benang Merah Bagian B:
PHP berjalan di server (Poin 1-5) →
Variabel: wadah penyimpan data dengan tanda $ →
PHP dynamically typed: tipe ditentukan otomatis →
Type juggling: PHP suka mengubah tipe sendiri →
var_dump: cara melihat tipe data sebenarnya
```

6. `[[6. Variabel — Wadah Data dengan Tanda Dollar]]`
    
    - Semua variabel PHP dimulai dengan `$`:
        
        PHP
        
        ```
        <?php
        // Deklarasi variabel
        $judul = "Clean Code";           // string
        $harga = 150000;                 // integer
        $rating = 4.8;                   // float
        $tersedia = true;                // boolean
        $pengarang = null;               // null (belum diisi)
        
        // Aturan penamaan:
        // ✅ Valid:
        $namaBuku = "value";
        $NamaBuku = "value";  // PHP case-sensitive untuk variabel!
        $_namaBuku = "value";
        $namaBuku2 = "value";
        
        // ❌ Tidak valid:
        // $2namaBuku = "value"; // tidak boleh mulai dengan angka
        // $nama-buku = "value"; // tidak boleh ada tanda hubung
        
        // var_dump: tampilkan tipe DAN nilai
        var_dump($judul);    // string(10) "Clean Code"
        var_dump($harga);    // int(150000)
        var_dump($rating);   // float(4.8)
        var_dump($tersedia); // bool(true)
        var_dump($pengarang); // NULL
        ?>
        ```
        
    - _Langkah konkret_: Buat variabel untuk data satu buku lengkap (judul, pengarang, tahun, ISBN, harga, stok)
7. `[[7. Type Juggling — PHP yang Suka Mengubah Tipe]]`
    
    - PHP secara otomatis mengubah tipe — ini bisa mengejutkan:
        
        PHP
        
        ```
        <?php
        // PHP mengubah string ke number saat perlu:
        $stok = "5";        // string
        $ditambah = $stok + 3; // PHP ubah "5" ke integer 5
        var_dump($ditambah); // int(8) — sudah jadi integer!
        
        // Ini berbahaya karena tidak terduga:
        $a = "5 ekor";   // string dengan angka di awal
        $b = $a + 2;     // PHP ambil angka "5", hasilnya: int(7)
        
        $c = "ekor 5";   // string dengan angka di tengah
        $d = $c + 2;     // PHP tidak ketemu angka di awal, hasilnya: int(2)
        // (dengan Warning di PHP 8)
        
        // Perbandingan yang mengejutkan (loose comparison ==):
        var_dump(0 == "php");   // true di PHP 7, false di PHP 8
        var_dump("1" == "01");  // true! (keduanya dianggap integer 1)
        var_dump("" == null);   // true!
        var_dump("0" == false); // true!
        
        // Solusi: SELALU gunakan === untuk perbandingan
        var_dump("1" === "01");  // false (strict: tipe DAN nilai harus sama)
        var_dump("" === null);   // false
        ?>
        ```
        
    - _Langkah konkret_: Test semua perbandingan di atas di console PHP, pahami hasilnya
8. `[[8. Type Casting — Konversi Tipe yang Eksplisit]]`
    
    - Lebih baik konversi tipe secara eksplisit daripada bergantung pada type juggling:
        
        PHP
        
        ```
        <?php
        $inputHarga = "150000";   // dari form HTML, selalu string!
        
        // Type casting:
        $hargaInt = (int) $inputHarga;     // 150000
        $hargaFloat = (float) $inputHarga; // 150000.0
        $hargaStr = (string) 150000;       // "150000"
        $hargaBool = (bool) $inputHarga;   // true (non-empty string)
        
        // Fungsi konversi:
        $harga2 = intval($inputHarga);     // 150000
        $rating = floatval("4.8 bintang"); // 4.8
        
        // Di PHP 8, selalu gunakan ini saat menerima input dari user:
        function ambilInputInt(string $key): int {
          return isset($_GET[$key]) ? (int) $_GET[$key] : 0;
        }
        ?>
        ```
        

---

### C. Konstanta & Operator

9. `[[9. Konstanta — Nilai yang Tidak Berubah]]`
    
    - _Langkah konkret_: Buat konstanta untuk konfigurasi perpustakaan:
        
        PHP
        
        ```
        <?php
        // Cara lama:
        define('NAMA_PERPUSTAKAAN', 'Perpustakaan Digital Kota');
        define('MAKS_PINJAM', 5);       // maksimal buku yang bisa dipinjam
        define('DENDA_PER_HARI', 1000); // rupiah per hari keterlambatan
        define('VERSI_SISTEM', '1.0.0');
        
        // Cara modern PHP 8 (menggunakan const di scope global):
        const KATEGORI_BUKU = ['Fiksi', 'Non-Fiksi', 'Sains', 'Teknologi', 'Sejarah'];
        
        // PHP built-in constants:
        echo PHP_VERSION;      // versi PHP saat ini
        echo PHP_INT_MAX;      // 9223372036854775807
        echo PHP_EOL;          // newline karakter sesuai OS
        echo DIRECTORY_SEPARATOR; // / di Unix, \ di Windows
        
        // Gunakan konstanta:
        echo NAMA_PERPUSTAKAAN;  // tidak perlu tanda $
        echo MAKS_PINJAM . " buku";
        ?>
        ```
        
10. `[[10. Operator — Semua yang Perlu Diketahui]]`
    
    - _Langkah konkret_: Buat kalkulator denda perpustakaan:
        
        PHP
        
        ```
        <?php
        // Aritmatika
        $hargaBuku = 150000;
        $diskon = 0.10; // 10%
        $hargaSetelahDiskon = $hargaBuku * (1 - $diskon); // 135000
        
        $hariTerlambat = 7;
        $dendaPerHari = DENDA_PER_HARI; // 1000
        $totalDenda = $hariTerlambat * $dendaPerHari; // 7000
        
        // Penugasan
        $stok = 10;
        $stok += 5;  // stok = 15 (sama dengan $stok = $stok + 5)
        $stok -= 3;  // stok = 12
        $stok *= 2;  // stok = 24
        $stok /= 4;  // stok = 6
        $stok %= 4;  // stok = 2 (modulo)
        
        // Perbandingan — SELALU gunakan === bukan ==
        $inputStok = "5";
        var_dump($inputStok == 5);   // true (loose — berbahaya!)
        var_dump($inputStok === 5);  // false (strict — aman)
        var_dump($inputStok === "5"); // true (strict — benar)
        
        // Null coalescing — sangat berguna untuk form input
        $pengarang = $_GET['pengarang'] ?? 'Tidak Diketahui';
        // Sama dengan: isset($_GET['pengarang']) ? $_GET['pengarang'] : 'Tidak Diketahui'
        
        // Spaceship operator (PHP 7+) — untuk sorting
        echo 1 <=> 2; // -1 (kiri lebih kecil)
        echo 2 <=> 2; //  0 (sama)
        echo 3 <=> 2; //  1 (kiri lebih besar)
        ?>
        ```
        

---

### 🏗️ Checkpoint Level 1

text

```
✅ Checklist sebelum lanjut ke Level 2:

PEMAHAMAN:
├── Bisa jelaskan perbedaan server-side vs client-side
├── Bisa jelaskan apa yang terjadi saat browser request file PHP
├── Bisa jelaskan perbedaan == vs === dengan contoh nyata
├── Bisa jelaskan type juggling dan mengapa berbahaya
└── Bisa jelaskan 5 tipe data primitif PHP

PROYEK: Halaman Profil Perpustakaan
├── index.php: profil perpustakaan dengan variabel (nama, alamat, jam buka)
├── info.php: phpinfo() untuk verifikasi PHP berjalan
├── error_test.php: contoh 3 jenis error yang berbeda
└── hitung_denda.php: kalkulator denda dengan operator dan konstanta

HABIT:
├── error_reporting(E_ALL) di setiap file development
├── var_dump() untuk debug, bukan echo $variable
├── Selalu gunakan === bukan ==
└── Beri nama variabel yang deskriptif (bukan $x, $y, $temp)

Commit: feat: setup PHP project and create library profile page
```

---

## 🔵 LEVEL 2: STRING, ARRAY & FUNGSI (Minggu 4-7)

> **Tema**: _"Dari data individual ke koleksi data dan fungsi yang reusable"_  
> **Benang Merah**: Variabel tunggal (Level 1) → kumpulan data (array) → fungsi untuk mengolah data → sistem katalog buku  
> **Output**: Katalog buku dengan array, pencarian, filter, dan fungsi utility

---

### D. String — Manipulasi Teks

> 💡 **Benang Merah ke Level 1**: Di Level 1, kita simpan teks di variabel. Sekarang kita olah teks itu — potong, cari, ganti, format. Hampir semua input dari user adalah string.

11. `[[11. String Syntax — Single Quote vs Double Quote]]`
    
    - _Langkah konkret_: Pahami perbedaan dan kapan gunakan masing-masing:
        
        PHP
        
        ```
        <?php
        $judul = "Clean Code";
        $pengarang = "Robert Martin";
        
        // Double quote: proses variabel dan escape sequence
        echo "Buku: $judul oleh $pengarang\n";  // Buku: Clean Code oleh Robert Martin
        echo "Tab:\t dan Newline:\n";            // proses \t dan \n
        
        // Single quote: TIDAK proses variabel dan escape sequence (lebih cepat)
        echo 'Buku: $judul oleh $pengarang\n';  // Buku: $judul oleh $pengarang\n (literal!)
        
        // Untuk string panjang atau yang punya tanda kutip:
        $html = '<a href="#" class="btn">Pinjam</a>'; // single quote untuk HTML
        $query = "SELECT * FROM buku WHERE id = '$id'"; // double quote untuk variabel
        
        // Heredoc: multi-baris, proses variabel
        $deskripsi = <<<EOT
        Judul: $judul
        Pengarang: $pengarang
        Ini bisa multi-baris
        dan proses variabel.
        EOT;
        
        // Nowdoc: multi-baris, TIDAK proses variabel (seperti single quote)
        $template = <<<'EOT'
        Judul: $judul
        Pengarang: $pengarang
        Variabel tidak diproses di sini.
        EOT;
        ?>
        ```
        
12. `[[12. String Functions — Yang Paling Sering Digunakan]]`
    
    - _Langkah konkret_: Buat fungsi untuk memformat data buku:
        
        PHP
        
        ```
        <?php
        $judulKotor = "  clean code  ";  // ada spasi di awal dan akhir
        $isbn = "978-0-13-235088-4";
        
        // Dasar
        echo strlen($judulKotor);         // 14 (termasuk spasi)
        echo strlen(trim($judulKotor));   // 10 (tanpa spasi)
        
        // Case
        echo strtoupper("clean code");    // CLEAN CODE
        echo strtolower("CLEAN CODE");    // clean code
        echo ucfirst("clean code");       // Clean code
        echo ucwords("clean code book");  // Clean Code Book
        
        // Pencarian
        echo strpos("Clean Code", "Code"); // 6 (index mulai dari 0)
        echo strrpos("Clean Code Book Code", "Code"); // 16 (dari kanan)
        
        $adaKata = str_contains("Clean Code Book", "Code"); // true (PHP 8)
        $mulaiDengan = str_starts_with("Clean Code", "Clean"); // true (PHP 8)
        $akhirDengan = str_ends_with("Clean Code", "Code");    // true (PHP 8)
        
        // Penggantian
        $bersih = str_replace("-", "", $isbn); // 9780132350884
        $formatted = number_format(150000, 0, ',', '.'); // 150.000
        
        // Potong dan gabung
        $bagian = substr("Clean Code Book", 6, 4); // "Code"
        $parts = explode("-", $isbn);  // array: ["978", "0", "13", "235088", "4"]
        $digabung = implode("", $parts); // "9780132350884"
        
        // sprintf untuk format yang kompleks
        $info = sprintf("%-20s | %-15s | Rp %'08s", "Clean Code", "Robert Martin", "150000");
        // Clean Code           | Robert Martin   | Rp 00150000
        ?>
        ```
        

---

### E. Array — Koleksi Data

> 💡 **Benang Merah ke String**: String adalah satu teks. Array adalah kumpulan nilai — indexed (dengan angka) atau associative (dengan key string). Ini adalah struktur data paling penting di PHP.

13. `[[13. Array Indexed & Associative — Dua Jenis Utama]]`
    
    - _Langkah konkret_: Buat sistem katalog buku:
        
        PHP
        
        ```
        <?php
        // Indexed array: key adalah 0, 1, 2, ...
        $kategori = ['Fiksi', 'Non-Fiksi', 'Sains', 'Teknologi'];
        echo $kategori[0]; // Fiksi
        echo count($kategori); // 4
        
        // Associative array: key adalah string
        $buku = [
          'id'        => 1,
          'judul'     => 'Clean Code',
          'pengarang' => 'Robert Martin',
          'isbn'      => '9780132350884',
          'tahun'     => 2008,
          'harga'     => 150000,
          'stok'      => 5,
          'kategori'  => 'Teknologi',
          'tersedia'  => true,
        ];
        
        echo $buku['judul'];     // Clean Code
        echo $buku['harga'];     // 150000
        
        // Tambah, ubah, hapus properti
        $buku['rating'] = 4.8;       // tambah key baru
        $buku['stok'] = 4;           // ubah nilai
        unset($buku['isbn']);         // hapus key
        
        // Multidimensional array: array of arrays (katalog buku)
        $katalog = [
          [
            'id' => 1,
            'judul' => 'Clean Code',
            'pengarang' => 'Robert Martin',
            'harga' => 150000,
            'stok' => 5,
          ],
          [
            'id' => 2,
            'judul' => 'The Pragmatic Programmer',
            'pengarang' => 'Andrew Hunt',
            'harga' => 175000,
            'stok' => 3,
          ],
          [
            'id' => 3,
            'judul' => 'Design Patterns',
            'pengarang' => 'Gang of Four',
            'harga' => 200000,
            'stok' => 0, // stok habis
          ],
        ];
        
        // Akses array multidimensi
        echo $katalog[0]['judul'];   // Clean Code
        echo $katalog[1]['harga'];   // 175000
        ?>
        ```
        
14. `[[14. Array Functions — Transformasi, Filter & Sort]]`
    
    - _Langkah konkret_: Implementasikan fitur pencarian dan filter katalog:
        
        PHP
        
        ```
        <?php
        // array_map: transformasi setiap elemen
        $judulSemua = array_map(fn($buku) => $buku['judul'], $katalog);
        // ['Clean Code', 'The Pragmatic Programmer', 'Design Patterns']
        
        // array_filter: filter berdasarkan kondisi
        $tersedia = array_filter($katalog, fn($buku) => $buku['stok'] > 0);
        // Hanya buku yang ada stoknya
        
        // array_filter ulang dengan reset index
        $tersedia = array_values($tersedia); // index dimulai dari 0 lagi
        
        // usort: sort dengan fungsi kustom
        usort($katalog, fn($a, $b) => $a['harga'] <=> $b['harga']); // sort by harga asc
        usort($katalog, fn($a, $b) => $b['harga'] <=> $a['harga']); // sort by harga desc
        
        // array_reduce: akumulasi satu nilai
        $totalHarga = array_reduce($katalog, fn($carry, $buku) => $carry + $buku['harga'], 0);
        // 525000
        
        // Fungsi pencarian
        function cariBuku(array $katalog, string $keyword): array {
          return array_values(array_filter($katalog, function($buku) use ($keyword) {
            $keyword = strtolower($keyword);
            return str_contains(strtolower($buku['judul']), $keyword)
                || str_contains(strtolower($buku['pengarang']), $keyword);
          }));
        }
        
        $hasil = cariBuku($katalog, "clean");
        // [['id' => 1, 'judul' => 'Clean Code', ...]]
        
        // array_column: ambil satu kolom
        $semuaJudul = array_column($katalog, 'judul');
        // ['Clean Code', 'The Pragmatic Programmer', 'Design Patterns']
        
        // array_combine: buat array dari dua array
        $keys = ['nama', 'email'];
        $values = ['Budi', 'budi@email.com'];
        $user = array_combine($keys, $values);
        // ['nama' => 'Budi', 'email' => 'budi@email.com']
        ?>
        ```
        

---

### F. Fungsi — Kode yang Reusable

> 💡 **Benang Merah ke Array**: `array_filter`, `array_map`, `usort` semua menerima fungsi sebagai argument — ini adalah Higher-Order Functions. Sekarang kita pahami cara membuat dan menggunakan fungsi sendiri.

15. `[[15. Fungsi — Definisi, Parameter & Return]]`
    
    - _Langkah konkret_: Buat library fungsi untuk sistem perpustakaan:
        
        PHP
        
        ```
        <?php
        // Fungsi dasar
        function hitungDenda(int $hariTerlambat, int $dendaPerHari = 1000): int {
          // Type hint: hariTerlambat harus integer, dendaPerHari default 1000
          // Return type: harus mengembalikan integer
          return $hariTerlambat * $dendaPerHari;
        }
        
        echo hitungDenda(7);        // 7000 (default denda 1000)
        echo hitungDenda(7, 2000);  // 14000 (denda 2000/hari)
        
        // Named arguments (PHP 8) — bisa sebut nama parameternya
        echo hitungDenda(dendaPerHari: 2000, hariTerlambat: 5); // 10000
        
        // Return multiple values (via array)
        function infoStok(array $katalog): array {
          $tersedia = array_filter($katalog, fn($b) => $b['stok'] > 0);
          $habis = array_filter($katalog, fn($b) => $b['stok'] === 0);
          
          return [
            'tersedia' => count($tersedia),
            'habis'    => count($habis),
            'total'    => count($katalog),
          ];
        }
        
        $stokInfo = infoStok($katalog);
        echo $stokInfo['tersedia']; // 2
        echo $stokInfo['habis'];    // 1
        
        // Nullable return type
        function cariBukuById(array $katalog, int $id): ?array {
          // ?array: bisa mengembalikan array atau null
          foreach ($katalog as $buku) {
            if ($buku['id'] === $id) return $buku;
          }
          return null; // tidak ditemukan
        }
        
        $buku = cariBukuById($katalog, 1);
        if ($buku !== null) {
          echo $buku['judul'];
        }
        ?>
        ```
        
16. `[[16. Closure & Arrow Function — Fungsi Anonim]]`
    
    - _Langkah konkret_:
        
        PHP
        
        ```
        <?php
        // Closure (fungsi anonim) — berguna untuk callback
        $sortByHarga = function(array $a, array $b): int {
          return $a['harga'] <=> $b['harga'];
        };
        usort($katalog, $sortByHarga);
        
        // Gunakan keyword 'use' untuk capture variabel dari scope luar
        $hargaMaks = 160000;
        $terjangkau = array_filter($katalog, function($buku) use ($hargaMaks) {
          return $buku['harga'] <= $hargaMaks;
        });
        
        // Arrow function (PHP 7.4+) — lebih singkat, otomatis capture variabel luar
        $terjangkauV2 = array_filter($katalog, fn($buku) => $buku['harga'] <= $hargaMaks);
        // Tidak perlu 'use' — arrow function otomatis capture $hargaMaks
        
        // Praktik: buat fungsi pencarian yang fleksibel
        function buatFilter(string $field, mixed $nilai): Closure {
          return fn($buku) => $buku[$field] === $nilai;
        }
        
        $filterKategori = buatFilter('kategori', 'Teknologi');
        $bukuTeknologi = array_filter($katalog, $filterKategori);
        ?>
        ```
        
17. `[[17. Scope Variabel — Perbedaan PHP dengan Bahasa Lain]]`
    
    - PHP berbeda dari JavaScript: variabel global TIDAK otomatis tersedia di dalam fungsi!
        
        PHP
        
        ```
        <?php
        $namaAdmin = "Admin Perpustakaan"; // variabel global
        
        function tampilkanNama() {
          // ❌ SALAH: PHP tidak bisa akses variabel global secara otomatis
          echo $namaAdmin; // Undefined variable!
          
          // ✅ Cara 1: gunakan keyword global (hindari jika bisa)
          global $namaAdmin;
          echo $namaAdmin; // Admin Perpustakaan
        }
        
        // ✅ Cara 2: passing sebagai parameter (lebih baik)
        function tampilkanNamaV2(string $nama): void {
          echo $nama;
        }
        tampilkanNamaV2($namaAdmin);
        
        // Static variable: mempertahankan nilai antar pemanggilan
        function hitungKunjungan(): int {
          static $count = 0; // diinisialisasi hanya sekali!
          return ++$count;
        }
        
        echo hitungKunjungan(); // 1
        echo hitungKunjungan(); // 2
        echo hitungKunjungan(); // 3
        ?>
        ```
        

---

### 🏗️ Checkpoint Level 2

text

```
✅ Checklist sebelum lanjut ke Level 3:

PROYEK: Katalog Buku (PHP CLI / Halaman Statis)
File: katalog.php, fungsi.php (require/include)

FITUR:
├── Data 10 buku dalam array multidimensi
├── Tampilkan daftar buku dalam tabel HTML
├── Fungsi cariBuku(keyword): filter by judul/pengarang
├── Fungsi urutkanBuku(field, arah): sort by field
├── Fungsi statistikKatalog(): total, tersedia, habis
├── Fungsi formatHarga(angka): Rp 150.000
└── Fungsi hitungDenda(hari, tarif): dengan default parameter

PEMAHAMAN:
├── Bisa jelaskan perbedaan single vs double quote
├── Bisa jelaskan kapan pakai array_map vs foreach
├── Bisa jelaskan perbedaan closure vs arrow function
├── Bisa jelaskan mengapa variabel global tidak tersedia di dalam fungsi PHP
└── Bisa gunakan str_contains, str_starts_with (PHP 8)

Commit: feat: create book catalog with arrays and utility functions
```

---

## 🟡 LEVEL 3: FORM, SESSION & FILE (Minggu 7-10)

> **Tema**: _"Dari halaman statis ke aplikasi web yang menerima input dan mengingat state"_  
> **Benang Merah**: Data hardcode (Level 2) → input dari user via form → validasi → session untuk login → web app yang interaktif  
> **Output**: Sistem perpustakaan web dengan CRUD buku (tanpa database, pakai file/session), login admin

---

### G. Form Handling — Menerima Input dari User

> 💡 **Mengapa ini penting?** Semua web app menerima input dari user. Di PHP, form adalah cara utama. Setiap input dari user HARUS dianggap berbahaya — validasi dan sanitasi adalah wajib.

text

```
Benang Merah Bagian G:
Data hardcode di array (Level 2) →
User bisa input data via form HTML →
$_POST/$_GET: menangkap data form →
Validasi: pastikan data benar dan aman →
Sanitasi: bersihkan data sebelum digunakan →
Tampilkan error dan pertahankan input
```

18. `[[18. $_POST & $_GET — Superglobal untuk Form Data]]`
    
    - _Langkah konkret_: Buat form tambah buku:
        
        PHP
        
        ```
        <?php
        // ============= FORM TAMBAH BUKU =============
        // File: tambah-buku.php
        
        $errors = [];
        $input = [];
        $sukses = false;
        
        if ($_SERVER['REQUEST_METHOD'] === 'POST') {
          // Ambil input dan bersihkan whitespace
          $input = [
            'judul'     => trim($_POST['judul'] ?? ''),
            'pengarang' => trim($_POST['pengarang'] ?? ''),
            'isbn'      => trim($_POST['isbn'] ?? ''),
            'tahun'     => (int) ($_POST['tahun'] ?? 0),
            'harga'     => (int) ($_POST['harga'] ?? 0),
            'stok'      => (int) ($_POST['stok'] ?? 0),
          ];
          
          // Validasi
          if (empty($input['judul'])) {
            $errors['judul'] = 'Judul buku wajib diisi';
          } elseif (strlen($input['judul']) > 200) {
            $errors['judul'] = 'Judul terlalu panjang (maks 200 karakter)';
          }
          
          if (empty($input['pengarang'])) {
            $errors['pengarang'] = 'Nama pengarang wajib diisi';
          }
          
          if (!empty($input['isbn']) && !preg_match('/^[0-9]{13}$/', $input['isbn'])) {
            $errors['isbn'] = 'ISBN harus 13 digit angka';
          }
          
          $tahunSekarang = (int) date('Y');
          if ($input['tahun'] < 1000 || $input['tahun'] > $tahunSekarang) {
            $errors['tahun'] = "Tahun harus antara 1000 dan $tahunSekarang";
          }
          
          if ($input['harga'] < 0) {
            $errors['harga'] = 'Harga tidak boleh negatif';
          }
          
          if ($input['stok'] < 0) {
            $errors['stok'] = 'Stok tidak boleh negatif';
          }
          
          // Jika tidak ada error, proses data
          if (empty($errors)) {
            // Nanti: simpan ke database
            // Sekarang: simpan ke session sebagai demo
            $input['id'] = time(); // id sementara
            $_SESSION['katalog'][] = $input;
            $sukses = true;
            $input = []; // reset form
          }
        }
        ?>
        
        <!DOCTYPE html>
        <html lang="id">
        <body>
          <?php if ($sukses): ?>
            <div class="alert alert-success">Buku berhasil ditambahkan!</div>
          <?php endif; ?>
          
          <form method="POST" action="">
            <div>
              <label for="judul">Judul *</label>
              <input
                type="text"
                id="judul"
                name="judul"
                value="<?= htmlspecialchars($input['judul'] ?? '') ?>"
                class="<?= isset($errors['judul']) ? 'error' : '' ?>"
              >
              <?php if (isset($errors['judul'])): ?>
                <span class="error-msg"><?= htmlspecialchars($errors['judul']) ?></span>
              <?php endif; ?>
            </div>
            
            <!-- Field lainnya... -->
            
            <button type="submit">Tambah Buku</button>
          </form>
        </body>
        </html>
        ```
        
19. `[[19. Sanitasi & Validasi — Keamanan Input Dasar]]`
    
    - _Langkah konkret_:
        
        PHP
        
        ```
        <?php
        // ❌ BERBAHAYA: langsung echo input user
        echo $_GET['nama']; // XSS vulnerability!
        
        // ✅ AMAN: escape output
        echo htmlspecialchars($_GET['nama'] ?? '', ENT_QUOTES, 'UTF-8');
        // Mengubah: < menjadi &lt;, > menjadi &gt;, " menjadi &quot;, dll.
        
        // Sanitasi menggunakan filter_var
        $email = filter_var($_POST['email'] ?? '', FILTER_SANITIZE_EMAIL);
        $url = filter_var($_POST['url'] ?? '', FILTER_SANITIZE_URL);
        
        // Validasi menggunakan filter_var
        $emailValid = filter_var($email, FILTER_VALIDATE_EMAIL);
        if ($emailValid === false) {
          $errors['email'] = 'Format email tidak valid';
        }
        
        $urlValid = filter_var($url, FILTER_VALIDATE_URL);
        
        // Validasi integer dengan range
        $tahun = filter_var(
          $_POST['tahun'] ?? '',
          FILTER_VALIDATE_INT,
          ['options' => ['min_range' => 1000, 'max_range' => (int)date('Y')]]
        );
        
        if ($tahun === false) {
          $errors['tahun'] = 'Tahun tidak valid';
        }
        
        // CSRF Token — lindungi form dari serangan CSRF
        function generateCsrfToken(): string {
          if (!isset($_SESSION['csrf_token'])) {
            $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
          }
          return $_SESSION['csrf_token'];
        }
        
        function verifyCsrfToken(string $token): bool {
          return isset($_SESSION['csrf_token']) &&
                 hash_equals($_SESSION['csrf_token'], $token);
        }
        
        // Di form HTML:
        // <input type="hidden" name="csrf_token" value="<?= generateCsrfToken() ?>">
        
        // Di handler POST:
        if (!verifyCsrfToken($_POST['csrf_token'] ?? '')) {
          die('Invalid CSRF token');
        }
        ?>
        ```
        

---

### H. Session — Mengingat State User

20. `[[20. Session — Sistem Login Sederhana]]`
    
    - _Langkah konkret_: Buat sistem login admin perpustakaan:
        
        PHP
        
        ```
        <?php
        // ============= session_config.php =============
        // Konfigurasi session yang lebih aman
        ini_set('session.use_strict_mode', 1);
        ini_set('session.cookie_httponly', 1);
        ini_set('session.cookie_secure', 0); // set 1 di HTTPS
        ini_set('session.gc_maxlifetime', 3600); // 1 jam
        
        session_start();
        
        // ============= login.php =============
        // Data admin (nanti: ambil dari database)
        $adminData = [
          'email'    => 'admin@perpustakaan.id',
          'password' => password_hash('admin123', PASSWORD_BCRYPT), // JANGAN simpan plain text!
          'nama'     => 'Admin Perpustakaan',
          'role'     => 'admin',
        ];
        
        $loginError = '';
        
        if ($_SERVER['REQUEST_METHOD'] === 'POST') {
          $email = trim($_POST['email'] ?? '');
          $password = $_POST['password'] ?? '';
          
          // Verifikasi dengan password_verify (bukan ==!)
          if ($email === $adminData['email'] && password_verify($password, $adminData['password'])) {
            // Regenerasi session ID setelah login (cegah session fixation)
            session_regenerate_id(true);
            
            $_SESSION['user'] = [
              'email' => $adminData['email'],
              'nama'  => $adminData['nama'],
              'role'  => $adminData['role'],
            ];
            $_SESSION['login_time'] = time();
            
            header('Location: /dashboard.php');
            exit;
          } else {
            $loginError = 'Email atau password salah';
          }
        }
        
        // ============= middleware_auth.php =============
        // Sertakan di halaman yang butuh login
        function requireLogin(): void {
          if (!isset($_SESSION['user'])) {
            header('Location: /login.php?redirect=' . urlencode($_SERVER['REQUEST_URI']));
            exit;
          }
          
          // Cek session timeout (1 jam)
          if (time() - ($_SESSION['login_time'] ?? 0) > 3600) {
            session_destroy();
            header('Location: /login.php?expired=1');
            exit;
          }
          
          // Perbarui waktu akses terakhir
          $_SESSION['login_time'] = time();
        }
        
        function requireRole(string $role): void {
          requireLogin();
          if ($_SESSION['user']['role'] !== $role) {
            http_response_code(403);
            die('Akses ditolak');
          }
        }
        
        // ============= logout.php =============
        session_start();
        $_SESSION = []; // hapus semua data session
        session_destroy(); // hancurkan session di server
        
        // Hapus cookie session
        if (isset($_COOKIE[session_name()])) {
          setcookie(session_name(), '', time() - 3600, '/');
        }
        
        header('Location: /login.php');
        exit;
        ?>
        ```
        
21. `[[21. Flash Messages — Pesan Satu Kali Pakai]]`
    
    - _Langkah konkret_:
        
        PHP
        
        ```
        <?php
        function setFlash(string $tipe, string $pesan): void {
          // tipe: 'success', 'error', 'info', 'warning'
          $_SESSION['flash'][] = ['tipe' => $tipe, 'pesan' => $pesan];
        }
        
        function getFlash(): array {
          $flash = $_SESSION['flash'] ?? [];
          unset($_SESSION['flash']); // hapus setelah dibaca
          return $flash;
        }
        
        // Penggunaan di file action (setelah simpan buku):
        setFlash('success', "Buku '{$judul}' berhasil ditambahkan");
        header('Location: /buku/daftar.php');
        exit;
        
        // Di halaman daftar.php:
        $pesan = getFlash();
        foreach ($pesan as $msg):
        ?>
          <div class="alert alert-<?= $msg['tipe'] ?>">
            <?= htmlspecialchars($msg['pesan']) ?>
          </div>
        <?php endforeach; ?>
        ```
        

---

### I. File I/O — Menyimpan Data Tanpa Database

22. `[[22. File PHP — Simpan Data sebagai JSON]]`
    - _Langkah konkret_: Buat sistem penyimpanan data buku ke file JSON (sebelum belajar database):
        
        PHP
        
        ```
        <?php
        // ============= Storage sederhana dengan JSON file =============
        
        define('DATA_FILE', __DIR__ . '/data/buku.json');
        
        function bacaSemuaBuku(): array {
          if (!file_exists(DATA_FILE)) {
            return [];
          }
          $json = file_get_contents(DATA_FILE);
          return json_decode($json, true) ?? [];
        }
        
        function simpanSemuaBuku(array $buku): bool {
          $dir = dirname(DATA_FILE);
          if (!is_dir($dir)) {
            mkdir($dir, 0755, true); // buat direktori jika belum ada
          }
          
          $json = json_encode($buku, JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE);
          return file_put_contents(DATA_FILE, $json) !== false;
        }
        
        function tambahBuku(array $dataBuku): array {
          $buku = bacaSemuaBuku();
          
          // Auto-increment ID
          $maxId = array_reduce($buku, fn($carry, $b) => max($carry, $b['id']), 0);
          $dataBuku['id'] = $maxId + 1;
          $dataBuku['created_at'] = date('Y-m-d H:i:s');
          
          $buku[] = $dataBuku;
          simpanSemuaBuku($buku);
          
          return $dataBuku;
        }
        
        function updateBuku(int $id, array $dataUpdate): bool {
          $buku = bacaSemuaBuku();
          
          foreach ($buku as &$b) {  // & untuk reference
            if ($b['id'] === $id) {
              $b = array_merge($b, $dataUpdate);
              $b['updated_at'] = date('Y-m-d H:i:s');
              return simpanSemuaBuku($buku);
            }
          }
          
          return false; // buku tidak ditemukan
        }
        
        function hapusBuku(int $id): bool {
          $buku = bacaSemuaBuku();
          $buku = array_values(array_filter($buku, fn($b) => $b['id'] !== $id));
          return simpanSemuaBuku($buku);
        }
        ?>
        ```
        

---

### 🏗️ Checkpoint Level 3

text

```
✅ Checklist sebelum lanjut ke Level 4:

PROYEK: Sistem Perpustakaan Web (tanpa database)
├── login.php: form login dengan session
├── dashboard.php: halaman setelah login (protected)
├── buku/daftar.php: tampilkan semua buku dari JSON
├── buku/tambah.php: form tambah buku dengan validasi
├── buku/edit.php: form edit buku
├── buku/hapus.php: konfirmasi dan hapus buku
└── logout.php: hancurkan session

KEAMANAN (WAJIB):
├── Semua output menggunakan htmlspecialchars()
├── CSRF token di semua form POST
├── Session regenerate_id setelah login
├── password_hash/password_verify (bukan MD5!)
├── Input divalidasi sebelum diproses
└── requireLogin() di semua halaman protected

Commit: feat: add login, session, form validation, and file storage
```

---

## 🟠 LEVEL 4: OOP & DATABASE MYSQL (Minggu 10-15)

> **Tema**: _"Dari prosedural ke OOP yang terstruktur dan data yang persisten di database"_  
> **Benang Merah**: Fungsi prosedural (Level 2-3) → OOP untuk organisasi kode → PDO untuk database → sistem perpustakaan dengan MySQL  
> **Output**: Sistem perpustakaan dengan database MySQL, OOP, dan PDO yang aman

---

### J. OOP — Organisasi Kode yang Lebih Baik

> 💡 **Mengapa OOP?** Fungsi prosedural jadi sulit dikelola saat project besar. OOP mengelompokkan data (property) dan perilaku (method) dalam satu unit (class). Di dunia PHP modern, hampir semua framework menggunakan OOP.

23. `[[23. Class & Object — Blueprint dan Instance]]`
    
    - _Langkah konkret_: Refactor sistem buku ke OOP:
        
        PHP
        
        ```
        <?php
        // ============= Buku.php =============
        
        class Buku {
          // Properties dengan type declaration (PHP 7.4+)
          public int $id;
          public string $judul;
          public string $pengarang;
          public string $isbn;
          public int $tahun;
          public float $harga;
          public int $stok;
          public string $kategori;
          
          // Constructor dengan PHP 8 constructor property promotion
          public function __construct(
            string $judul,
            string $pengarang,
            string $isbn,
            int $tahun,
            float $harga,
            int $stok = 0,
            string $kategori = 'Umum',
            int $id = 0,
          ) {
            $this->judul = $judul;
            $this->pengarang = $pengarang;
            $this->isbn = $isbn;
            $this->tahun = $tahun;
            $this->harga = $harga;
            $this->stok = $stok;
            $this->kategori = $kategori;
            $this->id = $id;
          }
          
          // Method
          public function tersedia(): bool {
            return $this->stok > 0;
          }
          
          public function pinjam(): bool {
            if (!$this->tersedia()) {
              return false; // tidak bisa dipinjam
            }
            $this->stok--;
            return true;
          }
          
          public function kembalikan(): void {
            $this->stok++;
          }
          
          public function formatHarga(): string {
            return 'Rp ' . number_format($this->harga, 0, ',', '.');
          }
          
          public function toArray(): array {
            return [
              'id'        => $this->id,
              'judul'     => $this->judul,
              'pengarang' => $this->pengarang,
              'isbn'      => $this->isbn,
              'tahun'     => $this->tahun,
              'harga'     => $this->harga,
              'stok'      => $this->stok,
              'kategori'  => $this->kategori,
            ];
          }
          
          public static function fromArray(array $data): self {
            return new self(
              judul:     $data['judul'],
              pengarang: $data['pengarang'],
              isbn:      $data['isbn'],
              tahun:     $data['tahun'],
              harga:     $data['harga'],
              stok:      $data['stok'] ?? 0,
              kategori:  $data['kategori'] ?? 'Umum',
              id:        $data['id'] ?? 0,
            );
          }
        }
        
        // Penggunaan
        $buku = new Buku(
          judul: 'Clean Code',
          pengarang: 'Robert Martin',
          isbn: '9780132350884',
          tahun: 2008,
          harga: 150000,
          stok: 5,
          kategori: 'Teknologi',
        );
        
        echo $buku->judul;          // Clean Code
        echo $buku->formatHarga();  // Rp 150.000
        echo $buku->tersedia();     // true
        $buku->pinjam();
        echo $buku->stok;           // 4
        ?>
        ```
        
24. `[[24. Inheritance — Mewarisi dan Memperluas]]`
    
    - _Langkah konkret_: Buat sistem user dengan inheritance:
        
        PHP
        
        ```
        <?php
        // ============= User.php =============
        abstract class User {
          public function __construct(
            protected int $id,
            protected string $nama,
            protected string $email,
            protected string $passwordHash,
          ) {}
          
          public function verifyPassword(string $password): bool {
            return password_verify($password, $this->passwordHash);
          }
          
          abstract public function getRole(): string;
          abstract public function getBatasPinjam(): int;
          
          public function getId(): int { return $this->id; }
          public function getNama(): string { return $this->nama; }
          public function getEmail(): string { return $this->email; }
        }
        
        // ============= Admin.php =============
        class Admin extends User {
          public function getRole(): string { return 'admin'; }
          public function getBatasPinjam(): int { return 0; } // tidak perlu pinjam
          
          public function kelolaBuku(): void {
            // Admin bisa kelola buku
          }
        }
        
        // ============= Anggota.php =============
        class Anggota extends User {
          private array $pinjaman = [];
          
          public function getRole(): string { return 'anggota'; }
          public function getBatasPinjam(): int { return 5; }
          
          public function pinjamBuku(Buku $buku): bool {
            if (count($this->pinjaman) >= $this->getBatasPinjam()) {
              return false; // sudah mencapai batas
            }
            if (!$buku->pinjam()) {
              return false; // stok habis
            }
            $this->pinjaman[] = [
              'buku'         => $buku,
              'tanggal_pinjam' => new DateTime(),
              'batas_kembali' => (new DateTime())->modify('+14 days'),
            ];
            return true;
          }
          
          public function getPinjaman(): array { return $this->pinjaman; }
        }
        ?>
        ```
        
25. `[[25. Interface & Trait — Kontrak dan Reuse Code]]`
    
    - _Langkah konkret_:
        
        PHP
        
        ```
        <?php
        // Interface: kontrak yang harus dipenuhi
        interface Searchable {
          public function cari(string $keyword): array;
        }
        
        interface Exportable {
          public function toCSV(): string;
          public function toJSON(): string;
        }
        
        // Trait: reuse code antar class yang tidak related
        trait HasTimestamps {
          private ?DateTime $createdAt = null;
          private ?DateTime $updatedAt = null;
          
          public function setCreatedAt(): void {
            $this->createdAt = new DateTime();
          }
          
          public function setUpdatedAt(): void {
            $this->updatedAt = new DateTime();
          }
          
          public function getCreatedAt(): ?DateTime { return $this->createdAt; }
          public function getUpdatedAt(): ?DateTime { return $this->updatedAt; }
        }
        
        trait HasSoftDelete {
          private ?DateTime $deletedAt = null;
          
          public function softDelete(): void {
            $this->deletedAt = new DateTime();
          }
          
          public function restore(): void {
            $this->deletedAt = null;
          }
          
          public function isDeleted(): bool {
            return $this->deletedAt !== null;
          }
        }
        
        // Class yang menggunakan interface dan trait
        class KatalogBuku implements Searchable, Exportable {
          use HasTimestamps;
          
          private array $buku = [];
          
          public function tambah(Buku $buku): void {
            $this->buku[] = $buku;
            $this->setUpdatedAt();
          }
          
          public function cari(string $keyword): array {
            return array_filter(
              $this->buku,
              fn($b) => str_contains(strtolower($b->judul), strtolower($keyword))
                     || str_contains(strtolower($b->pengarang), strtolower($keyword))
            );
          }
          
          public function toCSV(): string {
            $lines = ['ID,Judul,Pengarang,Harga,Stok'];
            foreach ($this->buku as $b) {
              $lines[] = "{$b->id},{$b->judul},{$b->pengarang},{$b->harga},{$b->stok}";
            }
            return implode("\n", $lines);
          }
          
          public function toJSON(): string {
            return json_encode(
              array_map(fn($b) => $b->toArray(), $this->buku),
              JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE
            );
          }
        }
        ?>
        ```
        

---

### K. Database MySQL dengan PDO

> 💡 **Mengapa PDO bukan MySQLi?** PDO (PHP Data Objects) mendukung 12+ database driver — jika nanti pindah ke PostgreSQL, hampir tidak perlu ubah kode. MySQLi hanya untuk MySQL. PDO juga punya prepared statements yang lebih ergonomis.

26. `[[26. PDO — Koneksi Database yang Aman]]`
    
    - _Langkah konkret_: Buat koneksi database yang proper:
        
        SQL
        
        ```
        -- Buat database dan tabel di phpMyAdmin atau MySQL CLI
        CREATE DATABASE perpustakaan_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
        USE perpustakaan_db;
        
        CREATE TABLE buku (
          id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
          judul VARCHAR(200) NOT NULL,
          pengarang VARCHAR(100) NOT NULL,
          isbn CHAR(13) UNIQUE,
          tahun YEAR NOT NULL,
          harga DECIMAL(12, 2) NOT NULL DEFAULT 0,
          stok INT UNSIGNED NOT NULL DEFAULT 0,
          kategori VARCHAR(50) DEFAULT 'Umum',
          deskripsi TEXT,
          created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
          updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
          deleted_at TIMESTAMP NULL DEFAULT NULL,
          INDEX idx_judul (judul),
          INDEX idx_pengarang (pengarang),
          INDEX idx_kategori (kategori)
        );
        ```
        
        PHP
        
        ```
        <?php
        // ============= Database.php =============
        class Database {
          private static ?PDO $instance = null;
          
          private function __construct() {} // cegah instantiasi langsung
          
          public static function getInstance(): PDO {
            if (self::$instance === null) {
              $host = $_ENV['DB_HOST'] ?? 'localhost';
              $name = $_ENV['DB_NAME'] ?? 'perpustakaan_db';
              $user = $_ENV['DB_USER'] ?? 'root';
              $pass = $_ENV['DB_PASS'] ?? '';
              
              try {
                self::$instance = new PDO(
                  "mysql:host=$host;dbname=$name;charset=utf8mb4",
                  $user,
                  $pass,
                  [
                    PDO::ATTR_ERRMODE            => PDO::ERRMODE_EXCEPTION,
                    PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
                    PDO::ATTR_EMULATE_PREPARES   => false, // PENTING untuk keamanan
                  ]
                );
              } catch (PDOException $e) {
                // JANGAN tampilkan error database ke user di production!
                error_log("Koneksi database gagal: " . $e->getMessage());
                throw new RuntimeException("Layanan tidak tersedia saat ini");
              }
            }
            
            return self::$instance;
          }
        }
        ?>
        ```
        
27. `[[27. Prepared Statements — Query yang Aman dari SQL Injection]]`
    
    - _Langkah konkret_: Buat BukuRepository dengan PDO:
        
        PHP
        
        ```
        <?php
        // ============= BukuRepository.php =============
        class BukuRepository {
          private PDO $db;
          
          public function __construct() {
            $this->db = Database::getInstance();
          }
          
          public function findAll(int $page = 1, int $perPage = 10): array {
            $offset = ($page - 1) * $perPage;
            
            // Prepared statement: ? diganti dengan nilai yang aman
            $stmt = $this->db->prepare(
              "SELECT * FROM buku WHERE deleted_at IS NULL
               ORDER BY created_at DESC
               LIMIT ? OFFSET ?"
            );
            $stmt->execute([$perPage, $offset]);
            return $stmt->fetchAll();
          }
          
          public function findById(int $id): ?array {
            $stmt = $this->db->prepare(
              "SELECT * FROM buku WHERE id = ? AND deleted_at IS NULL"
            );
            $stmt->execute([$id]);
            return $stmt->fetch() ?: null;
          }
          
          public function cari(string $keyword): array {
            // ❌ SALAH - SQL Injection!
            // $query = "SELECT * FROM buku WHERE judul LIKE '%$keyword%'";
            
            // ✅ BENAR - Prepared Statement
            $stmt = $this->db->prepare(
              "SELECT * FROM buku
               WHERE deleted_at IS NULL
               AND (judul LIKE ? OR pengarang LIKE ?)
               ORDER BY judul"
            );
            $cari = "%{$keyword}%"; // wildcard di PHP, bukan di query
            $stmt->execute([$cari, $cari]);
            return $stmt->fetchAll();
          }
          
          public function create(array $data): int {
            $stmt = $this->db->prepare(
              "INSERT INTO buku (judul, pengarang, isbn, tahun, harga, stok, kategori)
               VALUES (:judul, :pengarang, :isbn, :tahun, :harga, :stok, :kategori)"
            );
            
            $stmt->execute([
              ':judul'     => $data['judul'],
              ':pengarang' => $data['pengarang'],
              ':isbn'      => $data['isbn'] ?: null,
              ':tahun'     => $data['tahun'],
              ':harga'     => $data['harga'],
              ':stok'      => $data['stok'],
              ':kategori'  => $data['kategori'],
            ]);
            
            return (int) $this->db->lastInsertId();
          }
          
          public function update(int $id, array $data): bool {
            $stmt = $this->db->prepare(
              "UPDATE buku
               SET judul = :judul, pengarang = :pengarang, harga = :harga,
                   stok = :stok, kategori = :kategori, updated_at = NOW()
               WHERE id = :id AND deleted_at IS NULL"
            );
            
            $stmt->execute([
              ':judul'     => $data['judul'],
              ':pengarang' => $data['pengarang'],
              ':harga'     => $data['harga'],
              ':stok'      => $data['stok'],
              ':kategori'  => $data['kategori'],
              ':id'        => $id,
            ]);
            
            return $stmt->rowCount() > 0;
          }
          
          public function delete(int $id): bool {
            // Soft delete: isi deleted_at, bukan hapus dari DB
            $stmt = $this->db->prepare(
              "UPDATE buku SET deleted_at = NOW() WHERE id = ? AND deleted_at IS NULL"
            );
            $stmt->execute([$id]);
            return $stmt->rowCount() > 0;
          }
          
          public function hitungTotal(string $keyword = ''): int {
            if ($keyword) {
              $stmt = $this->db->prepare(
                "SELECT COUNT(*) FROM buku WHERE deleted_at IS NULL
                 AND (judul LIKE ? OR pengarang LIKE ?)"
              );
              $cari = "%{$keyword}%";
              $stmt->execute([$cari, $cari]);
            } else {
              $stmt = $this->db->query("SELECT COUNT(*) FROM buku WHERE deleted_at IS NULL");
            }
            return (int) $stmt->fetchColumn();
          }
          
          public function transaction(callable $callback): mixed {
            try {
              $this->db->beginTransaction();
              $result = $callback($this->db);
              $this->db->commit();
              return $result;
            } catch (Throwable $e) {
              $this->db->rollBack();
              throw $e;
            }
          }
        }
        ?>
        ```
        

---

### 🏗️ Checkpoint Level 4

text

```
✅ Checklist sebelum lanjut ke Level 5:

PROYEK: Sistem Perpustakaan dengan Database
├── Database MySQL: tabel buku, anggota, peminjaman, admin
├── Class: Buku, User, Admin, Anggota (dengan inheritance)
├── Interface: Searchable, Exportable
├── Trait: HasTimestamps, HasSoftDelete
├── Repository: BukuRepository, AnggotaRepository, PeminjamanRepository
├── CRUD buku melalui web (semua prepared statement)
├── Sistem login dengan session
├── Paginasi pada daftar buku
└── Pencarian buku (prepared statement, aman dari SQL injection)

KEAMANAN:
├── PDO::ATTR_EMULATE_PREPARES = false
├── Semua query menggunakan prepared statement
├── Password dengan password_hash/password_verify
├── Soft delete (tidak hapus data dari DB)
└── Transaction untuk operasi yang harus atomic

Commit: feat: implement OOP with PDO database and full CRUD
```

---

## 🔴 LEVEL 5: API REST, COMPOSER & KEAMANAN (Minggu 15-20)

> **Tema**: _"Dari web app ke API yang bisa dikonsumsi aplikasi lain"_  
> **Benang Merah**: Web app (Level 4) → expose data via API → Composer untuk dependency → keamanan yang komprehensif → JWT auth  
> **Output**: REST API perpustakaan dengan JWT auth, Composer, dan keamanan production-level

---

### L. REST API dengan PHP

28. `[[28. Membangun REST API — Router Sederhana]]`
    
    - _Langkah konkret_:
        
        PHP
        
        ```
        <?php
        // ============= index.php (entry point API) =============
        header('Content-Type: application/json; charset=utf-8');
        header('X-Content-Type-Options: nosniff');
        
        // Simple router
        $method = $_SERVER['REQUEST_METHOD'];
        $uri    = parse_url($_SERVER['REQUEST_URI'], PHP_URL_PATH);
        $uri    = rtrim($uri, '/');
        
        // Parse route dan ID
        // /api/buku      → resource = 'buku', id = null
        // /api/buku/5    → resource = 'buku', id = 5
        preg_match('~/api/([a-z]+)(?:/(\d+))?~', $uri, $matches);
        $resource = $matches[1] ?? null;
        $id       = isset($matches[2]) ? (int) $matches[2] : null;
        
        // Route dispatch
        match (true) {
          $method === 'GET'    && $resource === 'buku' && $id === null => BukuController::index(),
          $method === 'GET'    && $resource === 'buku' && $id !== null => BukuController::show($id),
          $method === 'POST'   && $resource === 'buku'                 => BukuController::store(),
          $method === 'PUT'    && $resource === 'buku' && $id !== null => BukuController::update($id),
          $method === 'DELETE' && $resource === 'buku' && $id !== null => BukuController::destroy($id),
          $method === 'POST'   && $resource === 'auth'                 => AuthController::login(),
          default => jsonResponse(['error' => 'Endpoint tidak ditemukan'], 404),
        };
        
        // ============= Helper functions =============
        function jsonResponse(mixed $data, int $statusCode = 200): never {
          http_response_code($statusCode);
          echo json_encode([
            'status'  => $statusCode < 400 ? 'success' : 'error',
            'data'    => $statusCode < 400 ? $data : null,
            'message' => $statusCode >= 400 ? (is_string($data) ? $data : $data['error']) : null,
          ], JSON_UNESCAPED_UNICODE);
          exit;
        }
        
        function getRequestBody(): array {
          $body = file_get_contents('php://input');
          return json_decode($body, true) ?? [];
        }
        
        function getBearerToken(): ?string {
          $header = $_SERVER['HTTP_AUTHORIZATION'] ?? '';
          if (preg_match('/Bearer\s+(.+)/', $header, $matches)) {
            return $matches[1];
          }
          return null;
        }
        ?>
        ```
        
29. `[[29. JWT Authentication — Token-Based Auth]]`
    
    - _Langkah konkret_: Instalasi dan implementasi JWT:
        
        Bash
        
        ```
        composer require firebase/php-jwt
        ```
        
        PHP
        
        ```
        <?php
        use Firebase\JWT\JWT;
        use Firebase\JWT\Key;
        
        // ============= JwtService.php =============
        class JwtService {
          private string $secret;
          private int $expiry;
          
          public function __construct() {
            $this->secret = $_ENV['JWT_SECRET'] ?? throw new RuntimeException('JWT_SECRET not set');
            $this->expiry = (int) ($_ENV['JWT_EXPIRY'] ?? 3600);
          }
          
          public function generate(array $payload): string {
            $now = time();
            $token = array_merge($payload, [
              'iat' => $now,
              'exp' => $now + $this->expiry,
            ]);
            return JWT::encode($token, $this->secret, 'HS256');
          }
          
          public function verify(string $token): array {
            try {
              $decoded = JWT::decode($token, new Key($this->secret, 'HS256'));
              return (array) $decoded;
            } catch (Exception $e) {
              throw new UnauthorizedException('Token tidak valid: ' . $e->getMessage());
            }
          }
        }
        
        // ============= AuthMiddleware.php =============
        function requireJwtAuth(): array {
          $token = getBearerToken();
          
          if ($token === null) {
            jsonResponse('Token diperlukan', 401);
          }
          
          try {
            $jwtService = new JwtService();
            return $jwtService->verify($token);
          } catch (UnauthorizedException $e) {
            jsonResponse($e->getMessage(), 401);
          }
        }
        
        // ============= AuthController.php =============
        class AuthController {
          public static function login(): never {
            $body = getRequestBody();
            $email    = $body['email'] ?? '';
            $password = $body['password'] ?? '';
            
            // Validasi input
            if (empty($email) || empty($password)) {
              jsonResponse('Email dan password wajib diisi', 422);
            }
            
            // Cari user di database
            $repo = new UserRepository();
            $user = $repo->findByEmail($email);
            
            if ($user === null || !password_verify($password, $user['password_hash'])) {
              jsonResponse('Email atau password salah', 401);
            }
            
            // Generate token
            $jwtService = new JwtService();
            $token = $jwtService->generate([
              'user_id' => $user['id'],
              'email'   => $user['email'],
              'role'    => $user['role'],
            ]);
            
            jsonResponse([
              'token' => $token,
              'user'  => [
                'id'    => $user['id'],
                'nama'  => $user['nama'],
                'email' => $user['email'],
                'role'  => $user['role'],
              ],
            ]);
          }
        }
        ?>
        ```
        

---

### M. Composer & Autoloading

30. `[[30. Composer — Dependency Management]]`
    - _Langkah konkret_: Setup project dengan Composer:
        
        Bash
        
        ```
        # Init project
        composer init
        
        # Install packages yang dibutuhkan
        composer require firebase/php-jwt     # JWT
        composer require vlucas/phpdotenv     # .env file
        composer require monolog/monolog      # Logging
        
        # Dev dependencies
        composer require --dev phpunit/phpunit # Testing
        composer require --dev squizlabs/php_codesniffer # Code style
        ```
        
        JSON
        
        ```
        // composer.json
        {
          "name": "perpustakaan/sistem",
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
            "lint": "phpcs src/ --standard=PSR12"
          }
        }
        ```
        
        text
        
        ```
        Struktur project:
        perpustakaan/
        ├── public/
        │   └── index.php       ← entry point (web root)
        ├── src/
        │   ├── Controllers/
        │   │   ├── BukuController.php
        │   │   └── AuthController.php
        │   ├── Models/
        │   │   └── Buku.php
        │   ├── Repositories/
        │   │   └── BukuRepository.php
        │   ├── Services/
        │   │   └── JwtService.php
        │   └── Core/
        │       ├── Database.php
        │       └── Router.php
        ├── tests/
        │   └── BukuTest.php
        ├── .env
        ├── .env.example
        ├── .gitignore          ← tambahkan: vendor/, .env
        └── composer.json
        ```
        

---

### N. Keamanan PHP — Komprehensif

31. `[[31. Keamanan API — Rate Limiting & Input Validation]]`
    - _Langkah konkret_:
        
        PHP
        
        ```
        <?php
        // ============= Rate Limiter sederhana menggunakan APCu atau file =============
        function checkRateLimit(string $identifier, int $maxRequests = 60, int $window = 60): bool {
          $key = "ratelimit_{$identifier}";
          
          if (function_exists('apcu_fetch')) {
            $count = apcu_fetch($key);
            if ($count === false) {
              apcu_store($key, 1, $window);
              return true;
            }
            if ($count >= $maxRequests) {
              return false; // limit tercapai
            }
            apcu_inc($key);
            return true;
          }
          
          // Fallback: file-based (lebih lambat)
          // Implementasi via file tidak direkomendasikan untuk production
          return true;
        }
        
        // Gunakan IP sebagai identifier
        $clientIp = $_SERVER['HTTP_X_FORWARDED_FOR'] ?? $_SERVER['REMOTE_ADDR'];
        if (!checkRateLimit($clientIp)) {
          jsonResponse('Terlalu banyak request. Coba lagi nanti.', 429);
        }
        
        // ============= Input Validation Class =============
        class Validator {
          private array $errors = [];
          
          public function required(mixed $value, string $field): self {
            if (empty($value) && $value !== '0') {
              $this->errors[$field][] = "$field wajib diisi";
            }
            return $this;
          }
          
          public function string(mixed $value, string $field, int $min = 0, int $max = PHP_INT_MAX): self {
            if (!is_string($value)) {
              $this->errors[$field][] = "$field harus berupa teks";
            } elseif (strlen($value) < $min) {
              $this->errors[$field][] = "$field minimal $min karakter";
            } elseif (strlen($value) > $max) {
              $this->errors[$field][] = "$field maksimal $max karakter";
            }
            return $this;
          }
          
          public function integer(mixed $value, string $field, int $min = PHP_INT_MIN, int $max = PHP_INT_MAX): self {
            if (!is_numeric($value) || (int)$value != $value) {
              $this->errors[$field][] = "$field harus berupa angka bulat";
            } elseif ((int)$value < $min) {
              $this->errors[$field][] = "$field minimal $min";
            } elseif ((int)$value > $max) {
              $this->errors[$field][] = "$field maksimal $max";
            }
            return $this;
          }
          
          public function email(string $value, string $field): self {
            if (!filter_var($value, FILTER_VALIDATE_EMAIL)) {
              $this->errors[$field][] = "$field bukan format email yang valid";
            }
            return $this;
          }
          
          public function passes(): bool { return empty($this->errors); }
          public function getErrors(): array { return $this->errors; }
          
          public function validate(): void {
            if (!$this->passes()) {
              jsonResponse(['errors' => $this->getErrors()], 422);
            }
          }
        }
        
        // Penggunaan di controller:
        $body = getRequestBody();
        $validator = new Validator();
        $validator
          ->required($body['judul'] ?? '', 'judul')
          ->string($body['judul'] ?? '', 'judul', 2, 200)
          ->required($body['pengarang'] ?? '', 'pengarang')
          ->string($body['pengarang'] ?? '', 'pengarang', 2, 100)
          ->integer($body['tahun'] ?? '', 'tahun', 1000, (int)date('Y'))
          ->integer($body['harga'] ?? '', 'harga', 0)
          ->validate(); // akan jsonResponse error jika tidak valid
        ?>
        ```
        

---

### 🏗️ Checkpoint Level 5

text

```
✅ Checklist sebelum lanjut ke Level 6:

PROYEK: REST API Perpustakaan
└── Endpoint:
    ├── POST /api/auth/login → JWT token
    ├── GET /api/buku → daftar buku (public)
    ├── GET /api/buku/:id → detail buku (public)
    ├── POST /api/buku → tambah buku (JWT required)
    ├── PUT /api/buku/:id → update buku (JWT required)
    └── DELETE /api/buku/:id → hapus buku (JWT required)

COMPOSER:
├── Autoloading PSR-4 aktif
├── .env untuk konfigurasi sensitif
├── vendor/ di .gitignore
└── firebase/php-jwt, vlucas/phpdotenv terpasang

KEAMANAN:
├── JWT untuk auth (bukan session)
├── Rate limiting pada endpoint
├── Input validation dengan class Validator
├── Prepared statement di semua query
└── .env untuk semua konfigurasi sensitif

TEST dengan Postman/Thunder Client:
├── Login → dapat token
├── Request tanpa token → 401
├── Request dengan token yang salah → 401
└── Semua CRUD endpoint berfungsi

Commit: feat: build REST API with JWT, Composer, and comprehensive security
```

---

## ⚫ LEVEL 6: PHP MODERN, TESTING & DESIGN PATTERNS (Minggu 20-26)

> **Tema**: _"Dari kode yang bekerja ke kode yang bisa dipercaya dan dipelihara"_  
> **Benang Merah**: API sudah ada (Level 5) → PHP 8 modern features → design patterns → PHPUnit → static analysis → production-ready

---

### O. PHP 8 Modern Features

32. `[[32. PHP 8 Features — Ekspresi yang Lebih Ekspresif]]`
    - _Langkah konkret_: Refactor kode menggunakan fitur PHP 8:
        
        PHP
        
        ```
        <?php
        // Named Arguments (PHP 8.0)
        function buatBuku(
          string $judul,
          string $pengarang,
          int $tahun,
          float $harga = 0,
          int $stok = 0,
        ): Buku {
          return new Buku($judul, $pengarang, $tahun, $harga, $stok);
        }
        
        // Bisa sebut nama parameter — urutan tidak penting
        $buku = buatBuku(
          stok: 5,
          tahun: 2008,
          judul: 'Clean Code',
          pengarang: 'Robert Martin',
          harga: 150000,
        );
        
        // Match Expression (PHP 8.0) — lebih ketat dari switch
        $kategoriWarna = match($buku->kategori) {
          'Teknologi' => 'blue',
          'Fiksi'     => 'purple',
          'Sains'     => 'green',
          default     => 'gray',
        };
        // match: strict comparison (===), tidak ada fall-through, HARUS exhaustive
        
        // Nullsafe Operator (PHP 8.0)
        $namaKota = $pengguna?->getAlamat()?->getKota()?->getNama();
        // Sama dengan: $pengguna && $pengguna->getAlamat() && ... (tapi lebih singkat)
        
        // Union Types (PHP 8.0)
        function prosesBuku(Buku|array $input): Buku {
          if (is_array($input)) {
            return Buku::fromArray($input);
          }
          return $input;
        }
        
        // Fibers (PHP 8.1) — cooperative multitasking
        $fiber = new Fiber(function(): void {
          $nilai = Fiber::suspend('fiber-nilai');
          echo "Fiber melanjutkan dengan nilai: $nilai\n";
        });
        
        $hasilSuspend = $fiber->start();         // 'fiber-nilai'
        $fiber->resume('nilai-yang-dikembalikan');
        
        // Enums (PHP 8.1) — sangat berguna untuk konstanta bermakna
        enum Kategori: string {
          case Teknologi = 'Teknologi';
          case Fiksi     = 'Fiksi';
          case Sains     = 'Sains';
          case Umum      = 'Umum';
          
          public function label(): string {
            return match($this) {
              self::Teknologi => '💻 Teknologi',
              self::Fiksi     => '📖 Fiksi',
              self::Sains     => '🔬 Sains',
              self::Umum      => '📚 Umum',
            };
          }
        }
        
        $kat = Kategori::Teknologi;
        echo $kat->value;    // 'Teknologi'
        echo $kat->label();  // '💻 Teknologi'
        
        // Readonly Properties (PHP 8.1) — nilai hanya bisa di-set di constructor
        class BukuImmutable {
          public function __construct(
            public readonly int $id,
            public readonly string $judul,
            public readonly string $pengarang,
          ) {}
        }
        
        $buku = new BukuImmutable(1, 'Clean Code', 'Robert Martin');
        // $buku->judul = 'lain'; // Error! readonly property tidak bisa diubah
        ?>
        ```
        

---

### P. Testing dengan PHPUnit

33. `[[33. Unit Testing — Kode yang Bisa Dipercaya]]`
    
    - _Langkah konkret_: Buat test untuk BukuRepository:
        
        PHP
        
        ```
        <?php
        // ============= tests/Unit/BukuTest.php =============
        
        namespace Tests\Unit;
        
        use App\Models\Buku;
        use PHPUnit\Framework\TestCase;
        
        class BukuTest extends TestCase {
          public function test_buku_tersedia_ketika_stok_lebih_dari_nol(): void {
            $buku = new Buku(
              judul: 'Test Book',
              pengarang: 'Test Author',
              isbn: '1234567890123',
              tahun: 2024,
              harga: 100000,
              stok: 5,
            );
            
            $this->assertTrue($buku->tersedia());
          }
          
          public function test_buku_tidak_tersedia_ketika_stok_nol(): void {
            $buku = new Buku('Test', 'Author', '123', 2024, 0, stok: 0);
            $this->assertFalse($buku->tersedia());
          }
          
          public function test_pinjam_mengurangi_stok(): void {
            $buku = new Buku('Test', 'Author', '123', 2024, 0, stok: 5);
            
            $berhasil = $buku->pinjam();
            
            $this->assertTrue($berhasil);
            $this->assertEquals(4, $buku->stok);
          }
          
          public function test_pinjam_gagal_ketika_stok_nol(): void {
            $buku = new Buku('Test', 'Author', '123', 2024, 0, stok: 0);
            
            $berhasil = $buku->pinjam();
            
            $this->assertFalse($berhasil);
            $this->assertEquals(0, $buku->stok); // stok tidak berubah
          }
          
          public function test_kembalikan_menambah_stok(): void {
            $buku = new Buku('Test', 'Author', '123', 2024, 0, stok: 3);
            
            $buku->kembalikan();
            
            $this->assertEquals(4, $buku->stok);
          }
          
          public function test_format_harga_benar(): void {
            $buku = new Buku('Test', 'Author', '123', 2024, 150000);
            
            $this->assertEquals('Rp 150.000', $buku->formatHarga());
          }
          
          /**
           * @dataProvider providerHarga
           */
          public function test_format_harga_berbagai_nilai(float $harga, string $expected): void {
            $buku = new Buku('Test', 'Author', '123', 2024, $harga);
            $this->assertEquals($expected, $buku->formatHarga());
          }
          
          public static function providerHarga(): array {
            return [
              [0,        'Rp 0'],
              [1000,     'Rp 1.000'],
              [150000,   'Rp 150.000'],
              [1500000,  'Rp 1.500.000'],
            ];
          }
        }
        ```
        
        Bash
        
        ```
        # Jalankan tests
        ./vendor/bin/phpunit tests/
        
        # Dengan code coverage (butuh Xdebug)
        ./vendor/bin/phpunit --coverage-html coverage/
        ```
        
34. `[[34. Design Patterns — Repository dan Dependency Injection]]`
    
    - _Langkah konkret_: Implementasi DI Container sederhana:
        
        PHP
        
        ```
        <?php
        // ============= Container.php =============
        class Container {
          private array $bindings = [];
          private array $instances = [];
          
          public function bind(string $abstract, callable $factory): void {
            $this->bindings[$abstract] = $factory;
          }
          
          public function singleton(string $abstract, callable $factory): void {
            $this->bind($abstract, function() use ($abstract, $factory) {
              if (!isset($this->instances[$abstract])) {
                $this->instances[$abstract] = $factory($this);
              }
              return $this->instances[$abstract];
            });
          }
          
          public function make(string $abstract): mixed {
            if (isset($this->bindings[$abstract])) {
              return ($this->bindings[$abstract])($this);
            }
            throw new RuntimeException("$abstract tidak terdaftar di container");
          }
        }
        
        // ============= bootstrap.php =============
        $container = new Container();
        
        $container->singleton(PDO::class, fn() => Database::getInstance());
        $container->bind(BukuRepository::class, fn($c) => new BukuRepository($c->make(PDO::class)));
        $container->bind(BukuService::class, fn($c) => new BukuService($c->make(BukuRepository::class)));
        
        // ============= BukuService.php =============
        class BukuService {
          public function __construct(
            private BukuRepository $repository
          ) {}
          
          public function getBukuTersedia(int $page = 1): array {
            $buku = $this->repository->findAll($page);
            return array_filter($buku, fn($b) => $b['stok'] > 0);
          }
          
          public function pinjamBuku(int $bukuId, int $anggotaId): array {
            // Business logic: validasi dan proses peminjaman
            $buku = $this->repository->findById($bukuId);
            if (!$buku) throw new NotFoundException("Buku tidak ditemukan");
            if ($buku['stok'] < 1) throw new DomainException("Buku tidak tersedia");
            
            return $this->repository->transaction(function($db) use ($bukuId, $anggotaId) {
              // Kurangi stok
              $db->prepare("UPDATE buku SET stok = stok - 1 WHERE id = ?")->execute([$bukuId]);
              
              // Tambah record peminjaman
              $stmt = $db->prepare(
                "INSERT INTO peminjaman (buku_id, anggota_id, tanggal_pinjam, batas_kembali)
                 VALUES (?, ?, NOW(), DATE_ADD(NOW(), INTERVAL 14 DAY))"
              );
              $stmt->execute([$bukuId, $anggotaId]);
              
              return ['peminjaman_id' => (int) $db->lastInsertId()];
            });
          }
        }
        ?>
        ```
        

---

### 🏗️ Checkpoint Level 6

text

```
✅ Checklist sebelum lanjut ke Level 7:

PHP 8 FEATURES:
├── Named arguments digunakan di minimal 3 tempat
├── Match expression menggantikan switch
├── Enum untuk Kategori, Status, Role
├── Readonly properties di class immutable
└── Nullsafe operator untuk chaining

TESTING:
├── PHPUnit terpasang via Composer
├── Test untuk class Buku: tersedia, pinjam, kembalikan, formatHarga
├── Test dengan data provider (berbagai nilai harga)
├── Test berjalan dengan: ./vendor/bin/phpunit
└── Minimal 80% code coverage untuk class Model

DESIGN PATTERNS:
├── Repository Pattern: BukuRepository, AnggotaRepository
├── Service Layer: BukuService (business logic)
├── DI Container: resolve dependency secara otomatis
└── Dependency Injection: semua class menerima dependency via constructor

STATIC ANALYSIS:
├── PHPStan level 5 terpasang: composer require --dev phpstan/phpstan
├── ./vendor/bin/phpstan analyse src/ --level=5
└── 0 error dari PHPStan

Commit: feat: add PHP 8 features, PHPUnit tests, and design patterns
```

---

## 🟣 LEVEL 7: FRAMEWORK & ARSITEKTUR LANJUTAN (Minggu 26+)

> **Tema**: _"Memilih jalur spesialisasi — framework atau arsitektur advanced"_  
> **Catatan**: Pilih SATU jalur, kuasai dalam sebelum eksplorasi jalur lain.

---

### Jalur A: Laravel

35. `[[35. Laravel — Framework PHP Paling Populer]]`
    - _Langkah konkret_: Setup project Laravel:
        
        Bash
        
        ```
        # Install Laravel
        composer create-project laravel/laravel perpustakaan-laravel
        cd perpustakaan-laravel
        
        # Setup database di .env
        # DB_DATABASE=perpustakaan_db
        # DB_USERNAME=root
        # DB_PASSWORD=
        
        php artisan migrate
        php artisan serve
        ```
        
        PHP
        
        ```
        <?php
        // ============= routes/api.php =============
        use App\Http\Controllers\Api\BukuController;
        
        Route::apiResource('buku', BukuController::class);
        Route::post('auth/login', [AuthController::class, 'login']);
        
        // Protected routes
        Route::middleware('auth:sanctum')->group(function () {
          Route::apiResource('peminjaman', PeminjamanController::class);
        });
        
        // ============= database/migrations/create_buku_table.php =============
        Schema::create('buku', function (Blueprint $table) {
          $table->id();
          $table->string('judul', 200);
          $table->string('pengarang', 100);
          $table->char('isbn', 13)->unique()->nullable();
          $table->year('tahun');
          $table->decimal('harga', 12, 2)->default(0);
          $table->unsignedInteger('stok')->default(0);
          $table->string('kategori', 50)->default('Umum');
          $table->text('deskripsi')->nullable();
          $table->timestamps();
          $table->softDeletes();
        });
        
        // ============= app/Models/Buku.php =============
        class Buku extends Model {
          use SoftDeletes;
          
          protected $fillable = ['judul', 'pengarang', 'isbn', 'tahun', 'harga', 'stok', 'kategori'];
          
          protected $casts = [
            'harga' => 'decimal:2',
            'stok'  => 'integer',
            'tahun' => 'integer',
          ];
          
          public function scopeTersedia(Builder $query): Builder {
            return $query->where('stok', '>', 0);
          }
          
          public function peminjaman(): HasMany {
            return $this->hasMany(Peminjaman::class);
          }
          
          public function tersedia(): bool {
            return $this->stok > 0;
          }
        }
        
        // ============= app/Http/Controllers/Api/BukuController.php =============
        class BukuController extends Controller {
          public function index(Request $request): JsonResponse {
            $buku = Buku::query()
              ->when($request->search, fn($q, $s) => $q->where(function($q) use ($s) {
                $q->where('judul', 'like', "%$s%")
                  ->orWhere('pengarang', 'like', "%$s%");
              }))
              ->when($request->kategori, fn($q, $k) => $q->where('kategori', $k))
              ->orderBy('created_at', 'desc')
              ->paginate(10);
            
            return response()->json($buku);
          }
          
          public function store(StoreBukuRequest $request): JsonResponse {
            $buku = Buku::create($request->validated());
            return response()->json($buku, 201);
          }
          
          // ... show, update, destroy
        }
        
        // ============= app/Http/Requests/StoreBukuRequest.php =============
        class StoreBukuRequest extends FormRequest {
          public function rules(): array {
            return [
              'judul'     => 'required|string|max:200',
              'pengarang' => 'required|string|max:100',
              'isbn'      => 'nullable|digits:13|unique:buku',
              'tahun'     => 'required|integer|min:1000|max:' . date('Y'),
              'harga'     => 'required|numeric|min:0',
              'stok'      => 'required|integer|min:0',
              'kategori'  => 'required|string|in:Teknologi,Fiksi,Sains,Umum',
            ];
          }
        }
        ?>
        ```
        

---

### Jalur B: Symfony

36. `[[36. Symfony — Framework Enterprise-Grade]]`
    - _Langkah konkret_: Setup Symfony API:
        
        Bash
        
        ```
        composer create-project symfony/skeleton:"7.*" perpustakaan-symfony
        cd perpustakaan-symfony
        composer require symfony/orm-pack api
        composer require --dev symfony/maker-bundle
        
        php bin/console make:entity Buku
        php bin/console make:migration
        php bin/console doctrine:migrations:migrate
        php bin/console make:controller BukuController
        ```
        

---

### Jalur C: Clean Architecture

37. `[[37. Clean Architecture — Arsitektur yang Scalable]]`
    - _Langkah konkret_: Struktur project Clean Architecture:
        
        text
        
        ```
        src/
        ├── Domain/                 ← Layer paling dalam, tidak bergantung pada apapun
        │   ├── Entities/
        │   │   └── Buku.php       ← Murni PHP, tidak ada dependency
        │   ├── Repositories/
        │   │   └── BukuRepositoryInterface.php  ← Interface saja
        │   └── ValueObjects/
        │       ├── ISBN.php
        │       └── Harga.php
        │
        ├── Application/            ← Use cases (business rules)
        │   └── UseCases/
        │       ├── TambahBuku/
        │       │   ├── TambahBukuCommand.php
        │       │   └── TambahBukuHandler.php
        │       └── CariPeminjaman/
        │           ├── CariPeminjamanQuery.php
        │           └── CariPeminjamanHandler.php
        │
        ├── Infrastructure/         ← Implementasi konkret
        │   ├── Persistence/
        │   │   └── PdoBukuRepository.php  ← implement BukuRepositoryInterface
        │   └── Http/
        │       └── Controllers/
        │
        └── Presentation/           ← Layer paling luar
            └── Api/
                └── BukuController.php
        ```
        

---

## 📊 Ringkasan Progress Tracking

### Satu Project, 7 Level Enhancement

text

```
Level 1: Halaman profil perpustakaan — PHP dasar, variabel, operator
  + Level 2: + Katalog buku dengan array dan fungsi
  + Level 3: + Form CRUD, session login, file storage
  + Level 4: + OOP (class, inheritance, interface) + database MySQL + PDO
  + Level 5: + REST API + JWT + Composer + keamanan
  + Level 6: + PHP 8 features + PHPUnit + design patterns
  + Level 7: + Laravel / Symfony / Clean Architecture (pilih jalur)
```

### Tabel Progress

|Level|Poin|Durasi|Output Konkret|
|---|---|---|---|
|🟢 **1**|1-10|Minggu 1-4|Halaman profil perpustakaan berjalan di server|
|🔵 **2**|11-17|Minggu 4-7|Katalog buku dengan array dan fungsi|
|🟡 **3**|18-22|Minggu 7-10|Web app dengan form, login, file storage|
|🟠 **4**|23-27|Minggu 10-15|Sistem dengan OOP dan database MySQL|
|🔴 **5**|28-31|Minggu 15-20|REST API dengan JWT dan Composer|
|⚫ **6**|32-34|Minggu 20-26|PHP 8 modern + testing + design patterns|
|🟣 **7**|35-37|Minggu 26+|Framework atau clean architecture|

---

### Benang Merah Utama Sepanjang Roadmap

text

```
Poin 1  (server-side concept)   → Fondasi pemahaman PHP seluruhnya
Poin 5  (error reporting)       → Kebiasaan debugging yang benar dari awal
Poin 7  (type juggling)         → Mengapa SELALU pakai === bukan ==
Poin 10 (operator null coalescing) → Digunakan di semua form handling
Poin 11 (single vs double quote)   → Konsistensi string handling
Poin 13 (array multidimensi)    → Fondasi semua data manipulation
Poin 14 (array_filter/map)      → Pola fungsional untuk data
Poin 19 (sanitasi & CSRF)       → Security mindset dari awal
Poin 20 (session & login)       → Digunakan di Level 4, 5 (JWT lebih aman)
Poin 23 (OOP class)             → Fondasi semua Level 4+ dan framework
Poin 26 (PDO connection)        → Fondasi semua database operation
Poin 27 (prepared statement)    → SQL injection prevention — wajib selalu
Poin 28 (REST API router)       → Fondasi untuk framework route
Poin 29 (JWT)                   → Auth modern, menggantikan session di API
Poin 30 (Composer PSR-4)        → Autoloading dan dependency management
Poin 33 (PHPUnit)               → Test yang dijalankan setiap commit
```

---

## 💡 Cara Menggunakan Roadmap Ini

text

```
Setiap poin mengikuti format:
┌──────────────────────────────────────────────────────┐
│ 💡 Konteks: mengapa fitur/konsep ini ada             │
│ 🔗 Benang Merah: koneksi ke poin sebelum/sesudah    │
│ 📋 Kode: implementasi konkret dengan contoh nyata   │
│          yang langsung bisa dicoba di server lokal  │
│ ✅ Langkah konkret: verifikasi berhasil             │
└──────────────────────────────────────────────────────┘
```

**Aturan yang Tidak Boleh Dilanggar:**

1. **`error_reporting(E_ALL)` selalu aktif** saat development — error adalah informasi
2. **Selalu `===` bukan `==`** — simpan diri dari type juggling bugs
3. **`htmlspecialchars()` setiap output** dari user — zero XSS tolerance
4. **Prepared statement SELALU** untuk query database — zero SQL injection tolerance
5. **Commit setelah setiap checkpoint** — git history adalah safety net
6. **Baca pesan error PHP** — pesan errornya informatif, jangan langsung Stack Overflow

---

_Roadmap PHP v1.0 — Step-by-Step, Security First, One Project_  
_Setiap baris kode ditulis dengan sadar — tidak ada copy-paste tanpa memahami_