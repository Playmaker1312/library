### **Soal 1: Fitur Chat Otomatis (Parameter Default)**

Bayangkan Anda membuat fungsi untuk mengirim pesan di aplikasi chat. Jika pengguna tidak menulis pesan, kita ingin ada pesan standar yang terkirim.

**Tugas di VS Code:**

1. Buat fungsi bernama `kirimPesan`.
    
2. Berikan dua parameter: `teks` (default: `"Halo!"`) dan `pengirim` (default: `"Anonim"`).
    
3. Di dalam fungsi, tampilkan: `"[pengirim] mengirim pesan: [teks]"`.
    
4. Panggil fungsi tersebut dengan 3 skenario:
    
    - Isi kedua argumennya (misal: `"Apa kabar?"`, `"Budi"`).
        
    - Isi hanya argumen pertama saja (teks saja).
        
    - Jangan isi argumen sama sekali (kosongkan kurungnya).
        
5. Amati bagaimana nilai default bekerja saat Anda mengosongkan argumen.
    

---

### **Soal 2: Konfigurasi Mobil (Keyword Arguments / Object)**

Gunakan teknik _Object Destructuring_ agar urutan argumen tidak menjadi masalah.

**Tugas di VS Code:**

1. Buat fungsi bernama `spesifikasiMobil` yang menerima satu parameter berupa **Object** `{ merk, warna, transmisi = "Manual" }`.
    
2. Di dalam fungsi, tampilkan: `"Mobil [merk] berwarna [warna] dengan transmisi [transmisi]."`
    
3. Panggil fungsi tersebut dengan mengirimkan sebuah Object.
    
4. **Tantangan:** Masukkan `warna` terlebih dahulu baru `merk` di dalam Object tersebut.
    
    - Contoh: `spesifikasiMobil({ warna: "Merah", merk: "Toyota" });`
        
5. Lihat apakah hasilnya tetap benar meskipun urutannya acak.
    

---

### **Soal 3: Kalkulator Pajak Fleksibel (Kombinasi)**

Mari kita buat fungsi yang menghitung pajak, tapi persentase pajaknya bisa berubah-ubah namun memiliki standar tetap.

**Tugas di VS Code:**

1. Buat fungsi `hitungPajak` yang menerima parameter `{ harga, persenPajak = 11 }`. (Gunakan 11% sebagai default pajak PPN Indonesia).
    
2. Di dalam fungsi, hitung nilai pajak: `(harga * persenPajak) / 100`.
    
3. Kembalikan (_return_) nilai pajak tersebut.
    
4. Panggil fungsi untuk barang seharga `500000` dengan pajak default.
    
5. Panggil lagi untuk barang seharga `1000000` tapi kali ini tentukan `persenPajak` nya sebesar `5` (pajak khusus).
    
6. Tampilkan kedua hasilnya.
    

---

### **Pesan dari Mentor IT:**

Di JavaScript modern, penggunaan **Object** sebagai parameter fungsi (simulasi Keyword Arguments) adalah **standar wajib** jika fungsi Anda memiliki lebih dari 2 atau 3 parameter. Ini memudahkan tim Anda untuk membaca kode tanpa harus bolak-balik melihat definisi fungsi aslinya.
