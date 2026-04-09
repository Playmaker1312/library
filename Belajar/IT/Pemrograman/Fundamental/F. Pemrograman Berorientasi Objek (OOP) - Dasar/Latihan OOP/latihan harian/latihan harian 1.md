### Soal Latihan Praktik OOP JavaScript

**1. Class Dasar: Toko Elektronik**

- Buat class `Laptop`.
    
- Constructor menerima: `merk`, `prosesor`, dan `ram`.
    
- Buat method `cekSpesifikasi()` yang menampilkan: "Laptop [merk] menggunakan prosesor [prosesor] dengan RAM [ram] GB."
    
- Instansiasi: Buat objek `laptop1` dan `laptop2`, lalu panggil method-nya.
    

**2. Method dengan Logika: Kalkulator Sederhana**

- Buat class `Kalkulator`.
    
- Constructor tidak perlu menerima parameter (kosongkan).
    
- Buat dua method: `tambah(a, b)` dan `kali(a, b)`.
    
- Masing-masing method harus mengembalikan (_return_) hasil perhitungan tersebut.
    
- Instansiasi: Buat objek `hitung`, lalu tampilkan hasil dari `hitung.tambah(10, 5)` di console.
    

**3. Update Property: Game Karakter**

- Buat class `Hero`.
    
- Constructor menerima: `nama` dan `level`.
    
	- Buat method `naikLevel()`.  Isinya adalah menambah nilai `level` sebanyak 1 poin.
    
- Instansiasi: Buat objek `pemain1`. Panggil `naikLevel()`, lalu tampilkan `pemain1.level` untuk memastikan nilainya berubah.
    

**4. Array dalam Properti: Playlist Lagu**

- Buat class `Playlist`.
    
- Constructor menerima: `namaPlaylist`. Buat juga properti `daftarLagu` yang isinya array kosong `[]`.
    
- Buat method `tambahLagu(judul)`. Isinya menambahkan `judul` ke dalam array `daftarLagu`.
    
- Instansiasi: Buat objek `myMusic`. Panggil `tambahLagu("Lagu A")` dan `tambahLagu("Lagu B")`, lalu tampilkan isi `myMusic.daftarLagu`.
    

**5. Encapsulation (Private Field): Akun Bank**

- Buat class `Rekening`.
    
- Buat properti private `#saldo` (gunakan tanda `#`).
    
- Constructor menerima `saldoAwal` untuk mengisi `#saldo`.
    
- Buat method `cekSaldo()` untuk menampilkan nilai `#saldo`.
    
- Instansiasi: Buat objek `tabunganku`. Coba akses `tabunganku.#saldo` dari luar class (harusnya error), lalu gunakan `tabunganku.cekSaldo()`.
    

**6. Inheritance (Pewarisan) Dasar: Dunia Hewan**

- Buat class induk `Hewan` dengan properti `nama` dan method `makan()` yang menampilkan "[nama] sedang makan".
    
- Buat class turunan `Kucing` yang melakukan `extends` ke `Hewan`.
    
- Instansiasi: Buat objek `meong` dari class `Kucing`. Panggil method `makan()` meskipun method tersebut tidak dibuat di dalam class `Kucing`.
    

**7. Penggunaan Keyword `super`: Karyawan Perusahaan**

- Buat class induk `Karyawan` dengan constructor yang menerima `nama`.
    
- Buat class turunan `Manager` yang `extends Karyawan`.
    
- Constructor `Manager` menerima `nama` dan `departemen`. Gunakan `super(nama)` untuk mengirim nama ke class induk.
    
- Buat method `lapor()` yang menampilkan: "Manager [nama] dari departemen [departemen] sedang melapor."
    
- Instansiasi: Buat objek `bos`.
    

**8. Getter dan Setter: Sistem Suhu**

- Buat class `Termometer`.
    
- Constructor menerima `celcius`.
    
- Buat `get fahrenheit()` yang mengembalikan hasil konversi ke fahrenheit (Rumus: `celcius * 9/5 + 32`).
    
- Instansiasi: Buat objek `suhuHariIni`. Cukup akses `suhuHariIni.fahrenheit` (tanpa tanda kurung) untuk melihat hasilnya.
    

**9. Static Method: Helper Matematika**

- Buat class `Lingkaran`.
    
- Buat **Static Method** bernama `hitungLuas(jariJari)`. (Gunakan `static`).
    
- Isi method dengan rumus: `3.14 * jariJari * jariJari`.
    
- Pemanggilan: Panggil langsung `Lingkaran.hitungLuas(10)` tanpa menggunakan keyword `new`.
    

**10. Polymorphism (Overriding): Notifikasi**

- Buat class `Notifikasi` dengan method `kirim()` yang menampilkan "Mengirim notifikasi umum...".
    
- Buat class `EmailNotif` yang `extends Notifikasi`.
    
- Di dalam `EmailNotif`, buat lagi method `kirim()` (timpa/override) yang menampilkan "Mengirim email dengan subjek..."
    
- Instansiasi: Buat objek dari `EmailNotif` dan panggil `kirim()`. Lihat mana yang muncul.
    

---

	**Tips:** Kerjakan satu per satu di editor (VS Code atau Chrome Console). Jika sudah berhasil membuat class dan memanggil method-nya sesuai instruksi, berarti Anda sudah mulai "nyambung" dengan logika OOP. Semangat!