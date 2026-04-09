### Soal Latihan Praktik: Function & Modularitas
**1. Fungsi Dasar: Sapaan Spesifik**

- Buatlah sebuah function bernama `sapaUser`.
    
- Function ini menerima satu parameter: `nama`.
    
- Di dalamnya, tampilkan pesan: "Halo, [nama]! Selamat belajar JavaScript.".
    
- **Praktek:** Panggil fungsi tersebut dengan 3 nama berbeda.
    

**2. Arrow Function: Konversi Menit ke Detik**

- Buatlah sebuah **Arrow Function** bernama `menitKeDetik`.
    
- Parameter yang diterima: `menit`.
    
- Function harus mengembalikan (_return_) hasil perkalian menit dengan 60.
    
- **Praktek:** Simpan hasilnya di variabel `hasil`, lalu tampilkan: "[menit] menit adalah [hasil] detik.".
    

**3. Function dengan Default Parameter: Harga Diskon**

- Buatlah function `hitungHarga`.
    
- Parameter: `hargaAsli` dan `diskon` (berikan **default value** 10 jika diskon tidak diisi).
    
- Function mengembalikan harga setelah dipotong diskon.
    
- **Praktek:** Panggil `hitungHarga(100000)` (tanpa isi diskon) dan `hitungHarga(100000, 50)`.
    

**4. Rest Parameter: Penjumlah Tak Terbatas**

- Buatlah function `jumlahkanSemua` menggunakan **rest parameter** (`...angka`).
    
- Gunakan perulangan atau method `reduce` untuk menjumlahkan semua angka yang masuk.
    
- **Praktek:** Panggil fungsi dengan 3 angka: `jumlahkanSemua(1, 2, 3)` dan dengan 5 angka: `jumlahkanSemua(10, 20, 30, 40, 50)`.
    

**5. Callback Function: Olah Angka**

- Buatlah function `prosesAngka` yang menerima dua parameter: `angka` dan `callback`.
    
- Di dalam fungsi, jalankan `callback(angka)`.
    
- **Praktek:** Panggil `prosesAngka(5, (n) => console.log(n * n))` untuk kuadrat, dan `prosesAngka(10, (n) => console.log(n / 2))` untuk pembagian.
    

**6. Modularitas (Export/Import) Dasar: Modul Matematika**

- Buatlah file bernama `mathUtils.js`. **Export** sebuah fungsi `tambah(a, b)` dari sana.
    
- Di file utama (`main.js`), **Import** fungsi tersebut.
    
- **Praktek:** Jalankan fungsi `tambah` di file `main.js` dan tampilkan hasilnya.
    

**7. Named vs Default Export: Modul User**

- Buat file `userModule.js`.
    
- Gunakan **Default Export** untuk fungsi `logIn` dan **Named Export** untuk variabel `userRole = "Admin"`.
    
- **Praktek:** Import keduanya di file lain dan tampilkan: "User dengan peran [userRole] mencoba login.".
    

**8. Function Scope: Melindungi Variabel**

- Buat sebuah function `bankVault`.
    
- Di dalamnya, buat variabel `kodeRahasia = "12345"`.
    
- Buat function lagi di dalam `bankVault` (nested function) bernama `tampilkanKode` yang mencetak variabel tersebut.
    
- **Praktek:** Panggil `bankVault()`, lalu coba akses `kodeRahasia` dari luar fungsi (amati apakah error).
    

**9. Refactoring ke Modularitas: Data Produk**

- Misalkan kamu punya array `produk = ["Buku", "Pena", "Penghapus"]`.
    
- Buat folder `libs` dan file `productHelper.js`. Pindahkan logika untuk mencari nama produk ke dalam fungsi `cariProduk(nama)` di file tersebut dan export.
    
- **Praktek:** Import di file utama dan gunakan untuk mencari "Pena".
    

**10. Anonymous Function dalam Method: Filter Data**

- Buat sebuah array `angka = [1, 5, 8, 12, 15, 20]`.
    
- Gunakan method built-in `.filter()` dan masukkan **Anonymous Function** di dalamnya untuk menyaring angka yang lebih besar dari 10.
    
- **Praktek:** Simpan hasil filternya ke variabel baru dan tampilkan di console.