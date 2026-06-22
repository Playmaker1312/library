# 🗺️ Roadmap JavaScript: Step-by-Step Membangun Aplikasi Web Nyata

## Filosofi Roadmap Ini

> **"JavaScript bukan sekadar bahasa — JavaScript adalah cara berpikir tentang interaksi, data, dan waktu"** — setiap konsep yang dipelajari ada alasannya, bukan sekadar hafal sintaks.

### Prinsip Desain

- **Satu Project, Tumbuh Bersama**: website portofolio dari roadmap HTML & CSS dilanjutkan dan dibuat interaktif
- **Console sebagai Playground**: setiap konsep diuji di browser console sebelum diterapkan ke project
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **Error adalah Guru**: setiap checkpoint sengaja membuat error untuk dipahami dan diperbaiki

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: Fondasi — sintaks, variabel, tipe data → Console playground
    ↓ (enhance project)
Level 2: + Kontrol alur, array, object → To-do list di console
    ↓ (enhance)
Level 3: + Fungsi, scope, closure → Library utility functions
    ↓ (enhance)
Level 4: + OOP, prototype, class → Sistem manajemen buku
    ↓ (enhance)
Level 5: + Async, Promise, fetch → Integrasi API nyata
    ↓ (enhance)
Level 6: + DOM, Events, Form → UI interaktif lengkap
    ↓ (enhance)
Level 7: + Module, tooling, testing → Project production-ready
    ↓ (enhance — pilih jalur)
Level 8+: Node.js / Framework / Advanced patterns
```

---

## 🟢 LEVEL 1: FONDASI — CARA BERPIKIR JAVASCRIPT (Minggu 1-3)

> **Tema**: _"Dari nol ke program pertama yang berjalan — memahami cara JavaScript berpikir"_  
> **Benang Merah**: Apa itu JS → Cara menjalankan → Variabel → Tipe data → Operator → Program pertama  
> **Output**: Bisa menulis program kalkulator sederhana di console dan memahami error message

---

### A. Pengantar & Setup

> 💡 **Mengapa dimulai di sini?** JavaScript sudah ada di browser — tidak perlu install apapun untuk mulai. Memahami di mana JS berjalan menjelaskan banyak "mengapa" yang akan ditemui sepanjang belajar.

text

```
Benang Merah Bagian A:
JavaScript ada di browser (HTML & CSS sudah dipelajari) →
Engine V8 mengeksekusi JS →
Console adalah REPL — tulis, eksekusi, lihat hasilnya →
Node.js: JS di server →
Setup VS Code untuk produktivitas →
Hello World pertama
```

1. `[[1. Apa itu JavaScript & Di Mana Ia Berjalan]]`
    
    - JavaScript = bahasa yang berjalan di browser (dan server dengan Node.js)
    - Lahir 1995, dibuat Brendan Eich dalam 10 hari — itulah mengapa ada "keanehan"
    - ECMAScript: standar resmi — ES5 (2009), ES6/ES2015 (revolusi), ES2020+ (fitur modern)
    - **JavaScript ≠ Java** — nama yang menyesatkan, dua bahasa yang sama sekali berbeda
    - _Langkah konkret_: Buka Chrome → F12 → Console tab → ketik `1 + 1` → Enter → lihat `2`
2. `[[2. Cara Menjalankan JavaScript — Tiga Cara]]`
    
    - **Browser Console**: langsung di DevTools — untuk eksperimen dan debugging
    - **File `.js` di HTML**: `<script src="app.js"></script>` atau `<script>...</script>`
    - **Node.js**: `node nama-file.js` di terminal — untuk server dan tooling
    - Urutan yang kita gunakan: Console dulu → file `.js` terhubung ke HTML → nanti Node.js
    - _Langkah konkret_: Buat `script.js` di folder portofolio, hubungkan ke `index.html` sebelum `</body>`:
        
        HTML
        
        ```
        <script src="script.js"></script>
        ```
        
3. `[[3. console.log & Teman-temannya — Debug dari Hari Pertama]]`
    
    - `console.log()`: tampilkan nilai — paling sering digunakan
    - `console.error()`: tampilkan error (merah)
    - `console.warn()`: tampilkan peringatan (kuning)
    - `console.table()`: tampilkan array/object dalam bentuk tabel
    - `console.time()` / `console.timeEnd()`: ukur waktu eksekusi
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        console.log('Hello, World!');
        console.log(42, 'teks', true, [1, 2, 3]);
        console.table([{ nama: 'Budi', umur: 25 }, { nama: 'Ani', umur: 22 }]);
        console.error('Ini adalah error');
        ```
        
4. `[[4. VS Code Setup — Editor yang Produktif]]`
    
    - Extension wajib: **ESLint** (deteksi error), **Prettier** (format kode)
    - Extension berguna: **JavaScript (ES6) code snippets**, **GitLens**
    - Setting VS Code: `editor.formatOnSave: true`
    - Cara debug di VS Code: breakpoint + Run and Debug panel
    - _Langkah konkret_: Install ESLint dan Prettier, buat `.eslintrc.json` sederhana

---

### B. Variabel & Tipe Data — Fondasi Semua Program

> 💡 **Benang Merah ke A**: Sudah bisa menjalankan JS. Sekarang kita belajar menyimpan data — karena semua program pada dasarnya adalah tentang data yang diolah.

text

```
Benang Merah Bagian B:
Bisa menjalankan JS (Poin 1-4) →
Variabel: wadah untuk menyimpan data →
let vs const: yang satu bisa diubah, yang satu tidak →
Tipe data: jenis data yang bisa disimpan →
typeof: cek jenis data →
Type coercion: JavaScript mengubah tipe sendiri
```

5. `[[5. var, let, const — Tiga Cara Deklarasi Variabel]]`
    
    - **`const`**: nilai tidak bisa diubah setelah inisialisasi — **gunakan ini secara default**
    - **`let`**: nilai bisa diubah — gunakan hanya saat perlu diubah
    - **`var`**: hindari — punya masalah scope yang akan dipelajari nanti
    - Aturan penamaan: camelCase (`namaLengkap`), dimulai huruf/underscore/dollar
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        const nama = 'Budi Santoso';
        let umur = 25;
        let skor = 0;
        
        umur = 26;     // ✅ let bisa diubah
        // nama = 'Ani'; // ❌ TypeError: Assignment to constant variable
        
        console.log(nama, umur);
        ```
        
6. `[[6. Tipe Data Primitif — Tujuh Tipe yang Wajib Dipahami]]`
    
    - **`string`**: teks — `'teks'`, `"teks"`, `` `template literal` ``
    - **`number`**: angka — `42`, `3.14`, `-7`, `NaN`, `Infinity`
    - **`boolean`**: benar/salah — `true`, `false`
    - **`null`**: sengaja kosong — "tidak ada nilai, dan itu disengaja"
    - **`undefined`**: belum diberi nilai — "variabel ada tapi belum punya nilai"
    - **`symbol`**: unik (jarang digunakan di awal)
    - **`bigint`**: angka sangat besar (jarang digunakan di awal)
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        const nama = 'Budi';          // string
        const umur = 25;              // number
        const aktif = true;           // boolean
        const pasangan = null;        // null (sengaja kosong)
        let alamat;                   // undefined (belum diisi)
        
        console.log(typeof nama);     // "string"
        console.log(typeof umur);     // "number"
        console.log(typeof aktif);    // "boolean"
        console.log(typeof null);     // "object" ← BUG JavaScript yang terkenal!
        console.log(typeof alamat);   // "undefined"
        ```
        
7. `[[7. String — Tipe Data yang Paling Sering Digunakan]]`
    
    - Tiga cara membuat string:
        
        JavaScript
        
        ```
        const s1 = 'single quote';
        const s2 = "double quote";
        const s3 = `template literal — bisa interpolasi ${1 + 1}`;
        ```
        
    - **Template literal** (backtick) adalah cara modern — gunakan ini:
        
        JavaScript
        
        ```
        const nama = 'Budi';
        const umur = 25;
        
        // Cara lama (string concatenation):
        console.log('Halo, ' + nama + '! Umurmu ' + umur + ' tahun.');
        
        // Cara modern (template literal):
        console.log(`Halo, ${nama}! Umurmu ${umur} tahun.`);
        ```
        
    - _Langkah konkret_: Buat greeting message menggunakan template literal dengan nama dan umur
8. `[[8. Type Coercion — JavaScript yang Sok Pintar]]`
    
    - JavaScript sering mengubah tipe secara otomatis — ini sumber banyak bug!
    - **Implicit coercion** (otomatis):
        
        JavaScript
        
        ```
        console.log('5' + 3);    // '53' (number diubah ke string!)
        console.log('5' - 3);    // 2 (string diubah ke number)
        console.log(true + 1);   // 2 (true = 1)
        console.log(false + 1);  // 1 (false = 0)
        console.log(null + 1);   // 1 (null = 0)
        ```
        
    - **Explicit conversion** (sengaja):
        
        JavaScript
        
        ```
        const strAngka = '42';
        console.log(Number(strAngka));   // 42
        console.log(parseInt(strAngka)); // 42
        console.log(String(42));         // '42'
        console.log(Boolean(0));         // false
        console.log(Boolean(''));        // false
        console.log(Boolean('teks'));    // true
        ```
        
    - _Langkah konkret_: Prediksi output 10 ekspresi, verifikasi di console

---

### C. Operator — Alat untuk Mengolah Data

> 💡 **Benang Merah ke Tipe Data**: Data sudah bisa disimpan. Operator adalah cara mengolah data — menghitung, membandingkan, menggabungkan.

9. `[[9. Operator Aritmatika & Assignment]]`
    
    - Aritmatika: `+`, `-`, `*`, `/`, `%` (modulo/sisa bagi), `**` (pangkat)
    - Assignment: `=`, `+=`, `-=`, `*=`, `/=`, `%=`
    - Increment/Decrement: `++`, `--` (pre vs post)
    - _Langkah konkret_: Buat kalkulator suhu (Celsius ke Fahrenheit):
        
        JavaScript
        
        ```
        const celsius = 100;
        const fahrenheit = (celsius * 9/5) + 32;
        console.log(`${celsius}°C = ${fahrenheit}°F`);
        ```
        
10. `[[10. Operator Perbandingan — == vs ===]]`
    
    - **`===`** (strict equality): bandingkan nilai DAN tipe — **selalu gunakan ini**
    - **`==`** (loose equality): bandingkan nilai saja, coerce tipe dulu — **hindari**
    - Perbandingan lain: `!==`, `>`, `<`, `>=`, `<=`
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        console.log(5 === 5);     // true
        console.log(5 === '5');   // false (tipe berbeda)
        console.log(5 == '5');    // true (coercion — BERBAHAYA!)
        console.log(null == undefined);   // true (loose)
        console.log(null === undefined);  // false (strict)
        ```
        
11. `[[11. Operator Logika — &&, ||, !, ??, ?.]]`
    
    - `&&` (AND): keduanya harus true
    - `||` (OR): salah satunya cukup true
    - `!` (NOT): membalik boolean
    - `??` (Nullish Coalescing): nilai default jika `null` atau `undefined`
    - `?.` (Optional Chaining): akses properti tanpa error jika null/undefined
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        const user = null;
        
        // Tanpa optional chaining: ERROR
        // console.log(user.name); // TypeError!
        
        // Dengan optional chaining: aman
        console.log(user?.name);          // undefined (bukan error)
        console.log(user?.name ?? 'Tamu'); // 'Tamu' (nullish coalescing)
        
        // Nullish coalescing vs OR
        const nilai = 0;
        console.log(nilai || 'default');  // 'default' (0 adalah falsy!)
        console.log(nilai ?? 'default');  // 0 (hanya null/undefined yang di-replace)
        ```
        
12. `[[12. Truthy & Falsy — Nilai yang "Dianggap" True atau False]]`
    
    - **Falsy values** (6 nilai yang dianggap false): `false`, `0`, `''`, `null`, `undefined`, `NaN`
    - Semua nilai lain adalah **truthy** (termasuk `[]`, `{}`, `'0'`, `-1`)
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Test di console:
        const falsyValues = [false, 0, '', null, undefined, NaN];
        const truthyValues = [true, 1, 'teks', [], {}, -1, '0'];
        
        falsyValues.forEach(val => console.log(val, '->', Boolean(val)));
        truthyValues.forEach(val => console.log(val, '->', Boolean(val)));
        ```
        

---

### 🏗️ Checkpoint Level 1

text

```
✅ Checklist sebelum lanjut ke Level 2:

PEMAHAMAN:
├── Bisa jelaskan perbedaan let, const, var
├── Bisa jelaskan 7 tipe data primitif
├── Bisa jelaskan perbedaan == vs ===
├── Bisa prediksi hasil type coercion
└── Bisa jelaskan 6 falsy values

PRAKTIK:
├── Kalkulator suhu (Celsius ↔ Fahrenheit ↔ Kelvin)
├── Kalkulator BMI sederhana
├── Program yang menggunakan template literal untuk output
└── Test semua operator di console

HABIT:
├── Selalu gunakan const (kecuali perlu diubah)
├── Selalu gunakan === (bukan ==)
├── Baca error message sebelum tanya
└── Test di console sebelum tulis di file

Commit: feat: add JavaScript foundation - variables, types, operators
```

---

## 🟡 LEVEL 2: KONTROL ALUR & STRUKTUR DATA (Minggu 4-6)

> **Tema**: _"Dari program yang berjalan lurus ke program yang bisa memutuskan dan mengulang"_  
> **Benang Merah**: Data bisa disimpan (Level 1) → program butuh membuat keputusan → loop untuk pengulangan → array & object untuk data koleksi  
> **Output**: To-do list fungsional di console dengan semua operasi CRUD

---

### D. Control Flow — Program yang Bisa Memutuskan

> 💡 **Benang Merah ke Level 1**: Kita sudah bisa menyimpan dan membandingkan data. Sekarang kita gunakan perbandingan itu untuk membuat keputusan — "jika ini, lakukan itu".

text

```
Benang Merah Bagian D:
Operator perbandingan sudah ada (Poin 10-12) →
if/else: eksekusi kode berdasarkan kondisi →
switch: kondisi dengan banyak pilihan →
Ternary: kondisi ringkas dalam satu baris →
Short-circuit: kondisi yang efisien
```

13. `[[13. if, else if, else — Cabang Eksekusi]]`
    
    - _Langkah konkret_: Buat sistem grade calculator:
        
        JavaScript
        
        ```
        function tentukanGrade(nilai) {
          if (nilai >= 90) {
            return 'A';
          } else if (nilai >= 80) {
            return 'B';
          } else if (nilai >= 70) {
            return 'C';
          } else if (nilai >= 60) {
            return 'D';
          } else {
            return 'E';
          }
        }
        
        console.log(tentukanGrade(85));  // 'B'
        console.log(tentukanGrade(55));  // 'E'
        ```
        
14. `[[14. switch-case — Kondisi dengan Banyak Pilihan]]`
    
    - Lebih bersih dari banyak `else if` ketika membandingkan satu nilai dengan banyak opsi:
        
        JavaScript
        
        ```
        function namaHari(angka) {
          switch (angka) {
            case 0: return 'Minggu';
            case 1: return 'Senin';
            case 2: return 'Selasa';
            case 3: return 'Rabu';
            case 4: return 'Kamis';
            case 5: return 'Jumat';
            case 6: return 'Sabtu';
            default: return 'Tidak valid';
          }
        }
        
        // Kapan switch lebih baik dari if/else:
        // - Membandingkan SATU nilai dengan BANYAK kemungkinan
        // - Kondisi berdasarkan equality (===), bukan range
        ```
        
    - **Jangan lupa `break`!** Atau gunakan `return` dalam fungsi
    - _Langkah konkret_: Buat fungsi yang menentukan musim berdasarkan bulan
15. `[[15. Ternary Operator — Kondisi dalam Satu Baris]]`
    
    - `kondisi ? nilaiJikaTrue : nilaiJikaFalse`
    - Gunakan untuk kondisi **sederhana** yang menghasilkan nilai — bukan untuk logika kompleks:
        
        JavaScript
        
        ```
        const umur = 17;
        const status = umur >= 18 ? 'Dewasa' : 'Anak-anak';
        
        // Jangan nested ternary — susah dibaca:
        // const kategori = umur < 13 ? 'Anak' : umur < 18 ? 'Remaja' : 'Dewasa';
        
        // Lebih baik gunakan if/else untuk kondisi kompleks
        ```
        
    - _Langkah konkret_: Refactor beberapa kondisi sederhana ke ternary

---

### E. Loop — Pengulangan yang Efisien

> 💡 **Benang Merah ke Kondisi**: `if` menjalankan kode **sekali** berdasarkan kondisi. Loop menjalankan kode **berulang** — selama kondisi terpenuhi atau untuk setiap elemen dalam koleksi.

16. `[[16. for Loop — Perulangan dengan Kontrol Penuh]]`
    
    - Tiga bagian: `for (inisialisasi; kondisi; update)`
    - _Langkah konkret_: Hitung jumlah angka 1-100:
        
        JavaScript
        
        ```
        let total = 0;
        for (let i = 1; i <= 100; i++) {
          total += i;
        }
        console.log(`Jumlah 1-100: ${total}`); // 5050
        
        // FizzBuzz: klasik
        for (let i = 1; i <= 20; i++) {
          if (i % 15 === 0) console.log('FizzBuzz');
          else if (i % 3 === 0) console.log('Fizz');
          else if (i % 5 === 0) console.log('Buzz');
          else console.log(i);
        }
        ```
        
17. `[[17. while & do-while — Loop dengan Kondisi Dinamis]]`
    
    - `while`: cek kondisi dulu, baru eksekusi
    - `do-while`: eksekusi dulu, baru cek (minimal sekali jalan)
    - _Langkah konkret_: Program tebak angka di console:
        
        JavaScript
        
        ```
        // Simulasi game tebak angka (di Node.js atau dengan prompt())
        const target = Math.floor(Math.random() * 100) + 1;
        let tebakan;
        let percobaan = 0;
        
        // Di browser dengan prompt():
        while (tebakan !== target) {
          tebakan = Number(prompt(`Tebak angka 1-100 (percobaan ${percobaan + 1}):`));
          percobaan++;
          
          if (tebakan < target) console.log('Terlalu kecil!');
          else if (tebakan > target) console.log('Terlalu besar!');
          else console.log(`Benar! Butuh ${percobaan} percobaan.`);
        }
        ```
        
18. `[[18. break & continue — Kontrol Alur Loop]]`
    
    - `break`: keluar dari loop sepenuhnya
    - `continue`: lewati iterasi ini, lanjut ke berikutnya
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Cari angka pertama yang habis dibagi 7 di atas 50
        for (let i = 51; i <= 100; i++) {
          if (i % 7 === 0) {
            console.log(`Angka pertama: ${i}`);
            break; // keluar setelah menemukan
          }
        }
        
        // Tampilkan angka 1-10, skip yang genap
        for (let i = 1; i <= 10; i++) {
          if (i % 2 === 0) continue; // skip genap
          console.log(i); // 1, 3, 5, 7, 9
        }
        ```
        

---

### F. Array — Koleksi Data Berurut

> 💡 **Benang Merah ke Loop**: Loop sering digunakan bersama array — untuk memproses setiap elemen. Array adalah struktur data yang paling sering digunakan di JavaScript.

19. `[[19. Array Dasar — Buat, Akses, Modifikasi]]`
    
    - _Langkah konkret_: Mulai bangun to-do list:
        
        JavaScript
        
        ```
        // Membuat array
        const tugas = ['Belajar JavaScript', 'Buat project', 'Review kode'];
        
        // Akses elemen
        console.log(tugas[0]);      // 'Belajar JavaScript'
        console.log(tugas.length);  // 3
        console.log(tugas[tugas.length - 1]); // elemen terakhir
        
        // Modifikasi
        tugas[1] = 'Buat to-do list'; // ubah elemen
        tugas.push('Deploy ke GitHub'); // tambah di akhir
        tugas.unshift('Setup VS Code'); // tambah di awal
        
        const dihapus = tugas.pop();   // hapus dan ambil dari akhir
        const pertama = tugas.shift(); // hapus dan ambil dari awal
        
        console.log(tugas);
        ```
        
20. `[[20. Array Methods — forEach, map, filter, reduce]]`
    
    - Empat method paling penting — pahami ini dan 80% kebutuhan array tercakup:
        
        JavaScript
        
        ```
        const angka = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
        
        // forEach: lakukan sesuatu untuk setiap elemen (tidak return nilai)
        angka.forEach(n => console.log(n * 2));
        
        // map: ubah setiap elemen → array baru
        const dikali2 = angka.map(n => n * 2);
        console.log(dikali2); // [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]
        
        // filter: saring elemen → array baru (hanya yang lolos kondisi)
        const genap = angka.filter(n => n % 2 === 0);
        console.log(genap); // [2, 4, 6, 8, 10]
        
        // reduce: akumulasi nilai → satu nilai
        const total = angka.reduce((akumulator, n) => akumulator + n, 0);
        console.log(total); // 55
        
        // Chain methods:
        const totalGenapDikali2 = angka
          .filter(n => n % 2 === 0)
          .map(n => n * 2)
          .reduce((acc, n) => acc + n, 0);
        console.log(totalGenapDikali2); // 60
        ```
        
    - _Langkah konkret_: Proses array data to-do menggunakan map, filter, dan reduce
21. `[[21. Array Methods Pencarian & Transformasi]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        const produk = [
          { id: 1, nama: 'Laptop', harga: 15000000, stok: 5 },
          { id: 2, nama: 'Mouse', harga: 250000, stok: 0 },
          { id: 3, nama: 'Keyboard', harga: 800000, stok: 3 },
          { id: 4, nama: 'Monitor', harga: 4500000, stok: 0 },
        ];
        
        // find: elemen pertama yang cocok
        const mouse = produk.find(p => p.nama === 'Mouse');
        
        // findIndex: index elemen pertama yang cocok
        const indexKeyboard = produk.findIndex(p => p.nama === 'Keyboard');
        
        // some: apakah ADA yang memenuhi kondisi?
        const adaYangHabis = produk.some(p => p.stok === 0); // true
        
        // every: apakah SEMUA memenuhi kondisi?
        const semuaTersedia = produk.every(p => p.stok > 0); // false
        
        // includes: apakah nilai ada di array?
        const angka = [1, 2, 3, 4, 5];
        console.log(angka.includes(3)); // true
        
        // sort: urutkan (MUTASI array asli!)
        const produkTerurut = [...produk].sort((a, b) => a.harga - b.harga);
        
        // Produk yang tersedia, diurutkan dari termurah
        const tersedia = produk
          .filter(p => p.stok > 0)
          .sort((a, b) => a.harga - b.harga);
        ```
        
22. `[[22. Array Destructuring & Spread/Rest]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Destructuring: ambil nilai dari array ke variabel
        const koordinat = [10.5, 117.3, 45];
        const [latitude, longitude, altitude] = koordinat;
        console.log(latitude, longitude); // 10.5, 117.3
        
        // Skip elemen:
        const [pertama, , ketiga] = [1, 2, 3];
        
        // Swap tanpa variabel temp:
        let a = 1, b = 2;
        [a, b] = [b, a];
        console.log(a, b); // 2, 1
        
        // Spread: sebar elemen array
        const arr1 = [1, 2, 3];
        const arr2 = [4, 5, 6];
        const gabung = [...arr1, ...arr2]; // [1, 2, 3, 4, 5, 6]
        
        // Copy array (bukan reference):
        const kopian = [...arr1]; // bukan kopian = arr1
        
        // Rest: kumpulkan sisa elemen
        const [kepala, ...ekor] = [1, 2, 3, 4, 5];
        console.log(kepala); // 1
        console.log(ekor);   // [2, 3, 4, 5]
        ```
        

---

### G. Object — Data dengan Struktur Bermakna

> 💡 **Benang Merah ke Array**: Array menyimpan data berurut (indexed by number). Object menyimpan data dengan label/kunci (indexed by string/symbol). Bersama-sama: array of objects adalah pola data paling umum di JavaScript.

23. `[[23. Object Dasar — Buat, Akses, Modifikasi]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Membuat object
        const pengguna = {
          id: 1,
          nama: 'Budi Santoso',
          email: 'budi@email.com',
          umur: 25,
          aktif: true,
          alamat: {           // nested object
            kota: 'Jakarta',
            provinsi: 'DKI Jakarta'
          }
        };
        
        // Akses
        console.log(pengguna.nama);           // dot notation
        console.log(pengguna['email']);       // bracket notation
        console.log(pengguna.alamat.kota);   // nested access
        
        // Modifikasi
        pengguna.umur = 26;
        pengguna.telepon = '+62812345'; // tambah properti baru
        delete pengguna.aktif;         // hapus properti
        
        // Cek keberadaan properti
        console.log('nama' in pengguna);            // true
        console.log(pengguna.hasOwnProperty('id')); // true
        ```
        
24. `[[24. Object Methods & Iterasi]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        const siswa = [
          { nama: 'Budi', nilai: 85 },
          { nama: 'Ani', nilai: 92 },
          { nama: 'Cici', nilai: 78 },
        ];
        
        // Object.keys(): array dari keys
        const obj = { a: 1, b: 2, c: 3 };
        console.log(Object.keys(obj));   // ['a', 'b', 'c']
        console.log(Object.values(obj)); // [1, 2, 3]
        console.log(Object.entries(obj)); // [['a', 1], ['b', 2], ['c', 3]]
        
        // Iterasi object
        for (const [kunci, nilai] of Object.entries(obj)) {
          console.log(`${kunci}: ${nilai}`);
        }
        
        // Object.assign: merge objects (shallow copy)
        const defaults = { tema: 'light', bahasa: 'id', notif: true };
        const preferensi = { tema: 'dark' };
        const config = Object.assign({}, defaults, preferensi);
        // { tema: 'dark', bahasa: 'id', notif: true }
        
        // Spread (cara modern untuk merge):
        const configModern = { ...defaults, ...preferensi };
        ```
        
25. `[[25. Object Destructuring & Shorthand]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        const pengguna = {
          nama: 'Budi',
          email: 'budi@email.com',
          umur: 25,
          alamat: { kota: 'Jakarta' }
        };
        
        // Destructuring
        const { nama, email } = pengguna;
        const { alamat: { kota } } = pengguna; // nested
        const { nama: fullName, umur = 18 } = pengguna; // rename + default
        
        // Shorthand property (jika nama variabel = nama key):
        const x = 10, y = 20;
        const titik = { x, y }; // sama dengan { x: x, y: y }
        
        // Shorthand method:
        const utils = {
          // Cara lama:
          hitung: function(a, b) { return a + b; },
          // Cara modern:
          hitungBaru(a, b) { return a + b; }
        };
        
        // Computed property name:
        const key = 'nama';
        const obj = { [key]: 'Budi' }; // { nama: 'Budi' }
        ```
        
26. `[[26. JSON — Format Data untuk Komunikasi]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        const data = {
          pengguna: 'Budi',
          skor: [85, 92, 78],
          aktif: true
        };
        
        // Object → JSON string (untuk kirim ke server atau simpan)
        const jsonString = JSON.stringify(data);
        const jsonRapi = JSON.stringify(data, null, 2); // dengan indentasi
        console.log(jsonString);
        
        // JSON string → Object (data dari server)
        const parsed = JSON.parse(jsonString);
        console.log(parsed.pengguna); // 'Budi'
        
        // Simpan ke localStorage:
        localStorage.setItem('pengguna', JSON.stringify(data));
        const retrieved = JSON.parse(localStorage.getItem('pengguna'));
        ```
        

---

### 🏗️ Checkpoint Level 2

text

```
✅ Checklist sebelum lanjut ke Level 3:

PROYEK: To-do List di Console
├── Array untuk menyimpan daftar tugas
├── Fungsi: tambahTugas, hapusTugas, tandaiSelesai, tampilkanSemua
├── Filter: tampilkan hanya yang selesai / belum selesai
├── Data tersimpan di localStorage (JSON.stringify/parse)
└── Input dari user (menggunakan prompt() atau readline)

PEMAHAMAN:
├── Bisa jelaskan perbedaan map, filter, reduce
├── Bisa jelaskan kapan pakai array vs object
├── Bisa jelaskan apa itu destructuring
├── Bisa membaca dan menulis JSON
└── Bisa jelaskan perbedaan dot notation vs bracket notation

Commit: feat: add control flow, arrays, objects - todo list console
```

---

## 🟠 LEVEL 3: FUNGSI & SCOPE (Minggu 7-9)

> **Tema**: _"Menguasai fungsi sebagai fondasi semua kode yang reusable"_  
> **Benang Merah**: Sudah menulis fungsi sederhana (Level 1-2) → pahami fungsi lebih dalam → scope → closure → higher-order functions  
> **Output**: Library utility functions yang reusable, dipahami sepenuhnya

---

### H. Fungsi — First-Class Citizen

> 💡 **Mengapa fungsi penting?** Di JavaScript, fungsi bisa disimpan di variabel, dikirim sebagai argument, dan dikembalikan dari fungsi lain. Ini membuat JavaScript sangat powerful dan fleksibel.

text

```
Benang Merah Bagian H:
Sudah menulis fungsi dasar (Level 1-2) →
Function declaration vs expression: perbedaan hoisting →
Arrow function: sintaks modern →
Parameters: default, rest →
Return: nilai dan early return
```

27. `[[27. Function Declaration vs Expression vs Arrow]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // 1. Function Declaration — di-hoist (bisa dipanggil sebelum dideklarasi)
        function sapa(nama) {
          return `Halo, ${nama}!`;
        }
        
        // 2. Function Expression — tidak di-hoist
        const sapaDua = function(nama) {
          return `Selamat datang, ${nama}!`;
        };
        
        // 3. Arrow Function — sintaks singkat, TIDAK punya this sendiri
        const sapaTiga = (nama) => `Hai, ${nama}!`;
        const kali = (a, b) => a * b; // satu expression, no return needed
        const tanpaParam = () => 'Hello!';
        
        // Kapan pakai arrow vs regular function:
        // Arrow: callback, method array, fungsi singkat
        // Regular: method object (butuh this), constructor (Level 4)
        
        // Contoh perbedaan this (preview Level 4):
        const obj = {
          nama: 'Budi',
          sapaBiasa: function() { return `Hai, ${this.nama}`; }, // ✅ this = obj
          sapaArrow: () => `Hai, ${this.nama}`, // ❌ this = global/undefined
        };
        ```
        
28. `[[28. Parameters — Default, Rest & Destructuring]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Default parameter
        function buatProfil(nama, umur = 0, kota = 'Tidak diketahui') {
          return { nama, umur, kota };
        }
        buatProfil('Budi');              // { nama: 'Budi', umur: 0, kota: 'Tidak diketahui' }
        buatProfil('Ani', 25, 'Jakarta'); // { nama: 'Ani', umur: 25, kota: 'Jakarta' }
        
        // Rest parameter: kumpulkan argumen sisa menjadi array
        function jumlahkan(pertama, ...sisanya) {
          return sisanya.reduce((acc, n) => acc + n, pertama);
        }
        console.log(jumlahkan(1, 2, 3, 4, 5)); // 15
        
        // Destructuring sebagai parameter
        function tampilkanPengguna({ nama, email, umur = 18 }) {
          console.log(`${nama} (${email}) - ${umur} tahun`);
        }
        tampilkanPengguna({ nama: 'Budi', email: 'budi@email.com' });
        ```
        

---

### I. Scope & Closure — Konsep Paling Penting

> 💡 **Mengapa closure penting?** Closure adalah fondasi dari module pattern, React hooks, dan banyak pattern JavaScript modern. Memahami ini = naik level signifikan.

text

```
Benang Merah Bagian I:
Fungsi sudah dipahami (Poin 27-28) →
Scope: di mana variabel bisa diakses →
Lexical scope: fungsi "ingat" scope tempat ia dibuat →
Closure: fungsi yang membawa scope-nya ke mana-mana →
Module pattern: data private dengan closure
```

29. `[[29. Scope — Di Mana Variabel Bisa Diakses]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Global scope: bisa diakses dari mana saja
        const globalVar = 'Saya global';
        
        function outer() {
          // Function scope: hanya di dalam outer
          const outerVar = 'Saya outer';
          
          function inner() {
            // Lexical scope: inner bisa akses outerVar (tapi bukan sebaliknya)
            const innerVar = 'Saya inner';
            console.log(globalVar); // ✅
            console.log(outerVar);  // ✅
            console.log(innerVar);  // ✅
          }
          
          inner();
          // console.log(innerVar); // ❌ ReferenceError
        }
        
        // Block scope dengan let/const:
        {
          let blockVar = 'Di dalam block';
          console.log(blockVar); // ✅
        }
        // console.log(blockVar); // ❌ ReferenceError (block scope)
        ```
        
30. `[[30. Hoisting — Perilaku "Aneh" JavaScript]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Function declaration di-hoist PENUH:
        console.log(sapa('Budi')); // ✅ 'Halo, Budi!' (meski dideklarasi di bawah)
        function sapa(nama) { return `Halo, ${nama}!`; }
        
        // var di-hoist tapi nilainya undefined:
        console.log(x); // undefined (bukan error, tapi tidak ada nilainya)
        var x = 10;
        console.log(x); // 10
        
        // let/const di-hoist tapi TIDAK bisa diakses (Temporal Dead Zone):
        // console.log(y); // ❌ ReferenceError: Cannot access 'y' before initialization
        let y = 20;
        
        // Kesimpulan: pakai const/let, jangan var — perilaku lebih predictable
        ```
        
31. `[[31. Closure — Fungsi yang Membawa "Tas" Variabel-nya]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Closure: fungsi inner yang "ingat" variabel di scope outer-nya
        // bahkan setelah outer function selesai dieksekusi
        
        function buatCounter() {
          let count = 0; // variabel ini "ter-capture" oleh closure
          
          return {
            increment() { return ++count; },
            decrement() { return --count; },
            getCount()  { return count; },
          };
        }
        
        const counter = buatCounter();
        console.log(counter.increment()); // 1
        console.log(counter.increment()); // 2
        console.log(counter.decrement()); // 1
        console.log(counter.getCount());  // 1
        
        // 'count' tidak bisa diakses dari luar — data privacy!
        // console.log(counter.count); // undefined
        
        // Closure use case 2: partial application
        function kali(faktor) {
          return (angka) => angka * faktor;
        }
        
        const kaliDua = kali(2);
        const kaliSepuluh = kali(10);
        console.log(kaliDua(5));    // 10
        console.log(kaliSepuluh(7)); // 70
        ```
        

---

### J. Higher-Order Functions & Functional Programming Dasar

> 💡 **Benang Merah ke Array**: Di Level 2, kita pakai `map`, `filter`, `reduce` tanpa benar-benar memahami. Sekarang kita pahami mengapa mereka disebut Higher-Order Functions dan cara membuat HOF sendiri.

32. `[[32. Higher-Order Functions — Fungsi yang Menerima/Mengembalikan Fungsi]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Higher-order function: menerima fungsi sebagai argument
        function lakukanDua kali(fn, nilai) {
          return fn(fn(nilai));
        }
        
        const tambahSatu = x => x + 1;
        console.log(lakukanDuaKali(tambahSatu, 5)); // 7
        
        // Membuat map sendiri:
        function myMap(array, fn) {
          const hasil = [];
          for (const item of array) {
            hasil.push(fn(item));
          }
          return hasil;
        }
        
        console.log(myMap([1, 2, 3], x => x * 2)); // [2, 4, 6]
        
        // Membuat filter sendiri:
        function myFilter(array, fn) {
          const hasil = [];
          for (const item of array) {
            if (fn(item)) hasil.push(item);
          }
          return hasil;
        }
        ```
        
33. `[[33. Callback Functions & Pola Umum]]`
    
    - Callback: fungsi yang dikirim sebagai argument ke fungsi lain:
        
        JavaScript
        
        ```
        // setTimeout adalah HOF yang menerima callback
        setTimeout(() => {
          console.log('Ini dijalankan setelah 1 detik');
        }, 1000);
        
        // Array methods adalah HOF
        const angka = [1, 2, 3, 4, 5];
        angka.forEach(n => console.log(n)); // forEach menerima callback
        
        // Membuat fungsi dengan callback sendiri:
        function prosesData(data, onBerhasil, onGagal) {
          try {
            const hasil = data.map(x => x * 2);
            onBerhasil(hasil);
          } catch (error) {
            onGagal(error);
          }
        }
        
        prosesData(
          [1, 2, 3],
          hasil => console.log('Berhasil:', hasil),
          error => console.error('Gagal:', error)
        );
        ```
        
34. `[[34. Pure Functions & Immutability — Kode yang Predictable]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Impure function: bergantung pada atau mengubah state luar
        let total = 0;
        function tambahImpure(nilai) {
          total += nilai; // mengubah state luar — SIDE EFFECT
          return total;
        }
        
        // Pure function: output hanya bergantung pada input
        function tambahPure(totalSekarang, nilai) {
          return totalSekarang + nilai; // tidak mengubah apapun di luar
        }
        
        // Pure function mudah di-test, predictable, reusable
        console.log(tambahPure(0, 5)); // selalu 5
        console.log(tambahPure(0, 5)); // selalu 5
        
        // Immutability: jangan mutasi data asli
        const asli = [1, 2, 3];
        
        // ❌ Mutasi (ubah array asli):
        asli.push(4); // asli berubah!
        
        // ✅ Return baru:
        const baru = [...asli, 4]; // asli tidak berubah
        
        // Contoh pattern immutable update object:
        const pengguna = { nama: 'Budi', umur: 25 };
        const diupdate = { ...pengguna, umur: 26 }; // pengguna tidak berubah
        ```
        
35. `[[35. Recursion — Fungsi yang Memanggil Dirinya Sendiri]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Setiap rekursi butuh: base case dan recursive case
        
        // Faktorial: n! = n * (n-1)!
        function faktorial(n) {
          if (n <= 1) return 1; // base case: berhenti rekursi
          return n * faktorial(n - 1); // recursive case
        }
        
        console.log(faktorial(5)); // 120
        
        // Flatten nested array:
        function flatten(arr) {
          return arr.reduce((flat, item) => {
            if (Array.isArray(item)) {
              return [...flat, ...flatten(item)]; // rekursi jika nested
            }
            return [...flat, item];
          }, []);
        }
        
        console.log(flatten([1, [2, [3, [4]], 5]])); // [1, 2, 3, 4, 5]
        
        // Kapan rekursi? Saat masalah punya struktur yang berulang (tree, nested structure)
        ```
        

---

### 🏗️ Checkpoint Level 3

text

```
✅ Checklist sebelum lanjut ke Level 4:

PROYEK: Library Utility Functions
File: utils.js — kumpulan fungsi reusable:
├── chunk(array, size): pecah array menjadi chunks
├── groupBy(array, key): kelompokkan berdasarkan property
├── debounce(fn, delay): delay eksekusi fungsi
├── memoize(fn): cache hasil fungsi
├── pipe(...fns): chain fungsi dari kiri ke kanan
└── deepClone(obj): copy object dalam (rekursif)

TEST di console:
├── Setiap fungsi dengan berbagai input
├── Edge case: array kosong, null, undefined
└── Bandingkan dengan lodash untuk verifikasi

PEMAHAMAN:
├── Bisa jelaskan apa itu closure dengan contoh
├── Bisa jelaskan perbedaan pure vs impure function
├── Bisa jelaskan mengapa `this` berbeda di arrow vs regular function
└── Bisa membuat HOF sendiri

Commit: feat: create utility library with closures and HOF
```

---

## 🔴 LEVEL 4: OOP & PROTOTYPE (Minggu 10-13)

> **Tema**: _"Mengorganisasi kode yang kompleks dengan Object-Oriented Programming"_  
> **Benang Merah**: Object literal (Level 2) + Closure untuk privacy (Level 3) → Class untuk struktur yang jelas → Inheritance → Sistem buku yang terorganisir  
> **Output**: Sistem manajemen perpustakaan dengan OOP — Book, Member, Library classes

---

### K. Prototype — Fondasi OOP JavaScript

> 💡 **Mengapa memahami prototype?** Class di JavaScript adalah "syntactic sugar" di atas prototype. Memahami prototype = tidak bingung saat `class` tidak berperilaku seperti yang diharapkan.

36. `[[36. Prototype Chain — Bagaimana JavaScript Mencari Property]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Setiap object punya prototype (blueprint-nya)
        const arr = [1, 2, 3];
        
        // Bagaimana arr.push() bisa ada?
        // arr tidak punya push di dirinya sendiri
        // JavaScript cari di arr.__proto__ = Array.prototype
        // Ketemu! Array.prototype.push ada
        
        console.log(arr.__proto__ === Array.prototype); // true
        console.log(Array.prototype.isPrototypeOf(arr)); // true
        
        // Prototype chain:
        // arr → Array.prototype → Object.prototype → null
        
        // Kita bisa tambah method ke semua array (tapi jangan lakukan ini di production!):
        Array.prototype.jumlahkan = function() {
          return this.reduce((acc, n) => acc + n, 0);
        };
        console.log([1, 2, 3].jumlahkan()); // 6
        ```
        
37. `[[37. Constructor Function & new Keyword]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Constructor function (cara lama, sebelum class)
        function Buku(judul, penulis, tahun) {
          this.judul = judul;
          this.penulis = penulis;
          this.tahun = tahun;
        }
        
        // Method ditaruh di prototype (dibagi semua instance, hemat memori)
        Buku.prototype.info = function() {
          return `${this.judul} oleh ${this.penulis} (${this.tahun})`;
        };
        
        // 'new' melakukan 4 hal:
        // 1. Buat object baru {}
        // 2. Set prototype-nya ke Buku.prototype
        // 3. Jalankan constructor dengan this = object baru
        // 4. Return object baru
        const buku1 = new Buku('Clean Code', 'Robert Martin', 2008);
        console.log(buku1.info()); // 'Clean Code oleh Robert Martin (2008)'
        console.log(buku1 instanceof Buku); // true
        ```
        

---

### L. Class — Sintaks Modern OOP

> 💡 **Benang Merah ke Constructor**: Class adalah cara modern menulis constructor function + prototype methods. Di balik layar, mekanismenya sama — tapi kode lebih mudah dibaca.

38. `[[38. Class Declaration — Sintaks Modern]]`
    
    - _Langkah konkret_: Mulai bangun sistem perpustakaan:
        
        JavaScript
        
        ```
        class Buku {
          // Private fields (ES2022)
          #stok;
          
          constructor(id, judul, penulis, tahun, stok = 1) {
            this.id = id;
            this.judul = judul;
            this.penulis = penulis;
            this.tahun = tahun;
            this.#stok = stok;
            this.dipinjam = false;
          }
          
          // Instance method
          info() {
            return `[${this.id}] "${this.judul}" - ${this.penulis} (${this.tahun})`;
          }
          
          // Getter
          get stok() { return this.#stok; }
          
          // Setter dengan validasi
          set stok(nilai) {
            if (nilai < 0) throw new Error('Stok tidak boleh negatif');
            this.#stok = nilai;
          }
          
          // Static method: tidak perlu instance
          static dariJSON(json) {
            const data = JSON.parse(json);
            return new Buku(data.id, data.judul, data.penulis, data.tahun, data.stok);
          }
        }
        
        const buku1 = new Buku('B001', 'Clean Code', 'Robert Martin', 2008, 3);
        console.log(buku1.info());
        console.log(buku1.stok); // 3
        buku1.stok = 5;
        // buku1.#stok; // ❌ SyntaxError: private field
        ```
        
39. `[[39. Class Inheritance — extends & super]]`
    
    - _Langkah konkret_: Lanjutkan sistem perpustakaan:
        
        JavaScript
        
        ```
        class Anggota {
          #pinjamanAktif = [];
          
          constructor(id, nama, email) {
            this.id = id;
            this.nama = nama;
            this.email = email;
            this.tanggalDaftar = new Date();
          }
          
          get pinjamanAktif() { return [...this.#pinjamanAktif]; }
          
          pinjam(buku) {
            if (this.#pinjamanAktif.length >= this.batasPinjaman) {
              throw new Error(`${this.nama} sudah mencapai batas pinjaman`);
            }
            this.#pinjamanAktif.push(buku);
          }
          
          kembalikan(bukuId) {
            const index = this.#pinjamanAktif.findIndex(b => b.id === bukuId);
            if (index === -1) throw new Error('Buku tidak ditemukan dalam pinjaman');
            return this.#pinjamanAktif.splice(index, 1)[0];
          }
          
          get batasPinjaman() { return 3; } // default
          
          info() {
            return `[${this.id}] ${this.nama} (${this.email})`;
          }
        }
        
        // AnggotaPremium extends Anggota
        class AnggotaPremium extends Anggota {
          constructor(id, nama, email, subscription) {
            super(id, nama, email); // panggil constructor parent
            this.subscription = subscription;
          }
          
          get batasPinjaman() { return 10; } // override — polimorfisme!
          
          info() {
            return `${super.info()} [PREMIUM - ${this.subscription}]`;
          }
        }
        
        const anggota = new Anggota('A001', 'Budi', 'budi@email.com');
        const premium = new AnggotaPremium('A002', 'Ani', 'ani@email.com', 'Gold');
        
        console.log(anggota.batasPinjaman);  // 3
        console.log(premium.batasPinjaman);  // 10
        console.log(premium instanceof Anggota);        // true
        console.log(premium instanceof AnggotaPremium); // true
        ```
        
40. `[[40. Composition over Inheritance — Alternatif yang Sering Lebih Baik]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Masalah inheritance: 'Diamond problem', terlalu tightly coupled
        // Solusi: composition — gabungkan kemampuan dari berbagai sumber
        
        // Mixin: objek yang berisi methods untuk di-mix-in ke class
        const CanSearch = (Base) => class extends Base {
          cari(query) {
            return this.items.filter(item =>
              Object.values(item).some(v =>
                String(v).toLowerCase().includes(query.toLowerCase())
              )
            );
          }
        };
        
        const CanSort = (Base) => class extends Base {
          urutkan(field, arah = 'asc') {
            return [...this.items].sort((a, b) => {
              if (arah === 'asc') return a[field] > b[field] ? 1 : -1;
              return a[field] < b[field] ? 1 : -1;
            });
          }
        };
        
        class KoleksiBuku extends CanSearch(CanSort(class {
          constructor() { this.items = []; }
          tambah(item) { this.items.push(item); }
        })) {}
        
        const koleksi = new KoleksiBuku();
        // koleksi sekarang punya: tambah, cari, urutkan
        ```
        
41. `[[41. Sistem Perpustakaan Lengkap — Integrasi Semua Konsep]]`
    
    - _Langkah konkret_: Bangun class `Perpustakaan`:
        
        JavaScript
        
        ```
        class Perpustakaan {
          #buku = new Map();  // id → Buku
          #anggota = new Map(); // id → Anggota
          #riwayatPinjam = [];
          
          // CRUD Buku
          tambahBuku(buku) {
            if (this.#buku.has(buku.id)) throw new Error(`ID ${buku.id} sudah ada`);
            this.#buku.set(buku.id, buku);
            return buku;
          }
          
          cariBuku(query) {
            return [...this.#buku.values()].filter(b =>
              b.judul.toLowerCase().includes(query.toLowerCase()) ||
              b.penulis.toLowerCase().includes(query.toLowerCase())
            );
          }
          
          // CRUD Anggota
          daftarAnggota(anggota) {
            this.#anggota.set(anggota.id, anggota);
            return anggota;
          }
          
          // Proses Peminjaman
          pinjamBuku(anggotaId, bukuId) {
            const anggota = this.#anggota.get(anggotaId);
            const buku = this.#buku.get(bukuId);
            
            if (!anggota) throw new Error('Anggota tidak ditemukan');
            if (!buku) throw new Error('Buku tidak ditemukan');
            if (buku.stok < 1) throw new Error('Buku tidak tersedia');
            
            anggota.pinjam(buku);
            buku.stok--;
            
            const transaksi = {
              id: Date.now(),
              anggotaId,
              bukuId,
              tanggalPinjam: new Date(),
              tanggalKembali: null,
            };
            
            this.#riwayatPinjam.push(transaksi);
            return transaksi;
          }
          
          // Statistik
          get statistik() {
            const totalBuku = this.#buku.size;
            const totalAnggota = this.#anggota.size;
            const totalDipinjam = [...this.#buku.values()].filter(b => b.dipinjam).length;
            
            return { totalBuku, totalAnggota, totalDipinjam };
          }
        }
        ```
        

---

### 🏗️ Checkpoint Level 4

text

```
✅ Checklist sebelum lanjut ke Level 5:

PROYEK: Sistem Manajemen Perpustakaan
├── class Buku (private fields, getter/setter, static method)
├── class Anggota (pinjaman aktif, batas)
├── class AnggotaPremium (extends Anggota, override batasPinjaman)
├── class Perpustakaan (CRUD buku & anggota, proses pinjam-kembalikan)
├── Error handling yang tepat di setiap operasi
├── Data tersimpan/dimuat dari localStorage (JSON)
└── Demo semua fitur di console

PEMAHAMAN:
├── Bisa jelaskan perbedaan class vs constructor function
├── Bisa jelaskan kapan gunakan inheritance vs composition
├── Bisa jelaskan apa itu prototype chain
├── Bisa jelaskan `this` dalam class
└── Bisa jelaskan perbedaan instance method vs static method

Commit: feat: implement OOP library management system
```

---

## 🟣 LEVEL 5: ASYNCHRONOUS JAVASCRIPT (Minggu 14-17)

> **Tema**: _"Memahami dan menguasai pemrograman asinkron — jantung JavaScript modern"_  
> **Benang Merah**: Sistem perpustakaan butuh data dari API → async programming → Promise → async/await → integrasi API nyata  
> **Output**: Aplikasi cuaca yang fetch data dari API OpenWeatherMap secara real

---

### M. Event Loop — Memahami Cara Kerja JS

> 💡 **Mengapa memahami ini?** Async bukan tentang menjalankan dua hal sekaligus — JavaScript single-threaded. Async tentang "lakukan ini, dan saat selesai, beritahu saya". Event Loop yang mengatur ini.

text

```
Benang Merah Bagian M:
JavaScript single-threaded (satu hal sekaligus) →
Tapi butuh operasi yang "menunggu" (network, timer) →
Event Loop: mengelola antrian task →
Call Stack + Task Queue + Microtask Queue →
Promise dan async/await memanfaatkan ini
```

42. `[[42. Event Loop — Cara JavaScript Mengelola Async]]`
    
    - _Langkah konkret_: Prediksi urutan output:
        
        JavaScript
        
        ```
        console.log('1 - Start');
        
        setTimeout(() => console.log('2 - setTimeout 0ms'), 0);
        
        Promise.resolve().then(() => console.log('3 - Promise'));
        
        console.log('4 - End');
        
        // Output: 1, 4, 3, 2
        // Mengapa? 
        // - Synchronous (call stack): 1, 4
        // - Microtask queue (Promise): 3
        // - Task queue (setTimeout): 2
        // Microtask diproses SEBELUM task!
        ```
        
    - _Langkah konkret_: Test berbagai kombinasi setTimeout + Promise di console
43. `[[43. Callback & Callback Hell — Masalah yang Perlu Dipecahkan]]`
    
    - _Langkah konkret_: Simulasikan masalah callback hell:
        
        JavaScript
        
        ```
        // Simulasi operasi async dengan setTimeout
        function ambilData(id, callback) {
          setTimeout(() => {
            if (id <= 0) callback(new Error('ID tidak valid'), null);
            else callback(null, { id, nama: 'Data ' + id });
          }, 500);
        }
        
        // Callback Hell (Pyramid of Doom):
        ambilData(1, (err1, data1) => {
          if (err1) return console.error(err1);
          ambilData(2, (err2, data2) => {
            if (err2) return console.error(err2);
            ambilData(3, (err3, data3) => {
              if (err3) return console.error(err3);
              console.log(data1, data2, data3); // susah dibaca, error handling berulang
            });
          });
        });
        // Ini yang akan kita perbaiki dengan Promise
        ```
        

---

### N. Promise — Solusi Elegan untuk Async

44. `[[44. Membuat & Menggunakan Promise]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Membuat Promise
        function ambilData(id) {
          return new Promise((resolve, reject) => {
            setTimeout(() => {
              if (id <= 0) reject(new Error('ID tidak valid'));
              else resolve({ id, nama: 'Data ' + id });
            }, 500);
          });
        }
        
        // Konsumsi dengan .then()/.catch()/.finally()
        ambilData(1)
          .then(data => {
            console.log('Berhasil:', data);
            return data.id + 1; // nilai return menjadi input .then() berikutnya
          })
          .then(idBaru => ambilData(idBaru)) // Promise chaining
          .catch(error => console.error('Error:', error.message))
          .finally(() => console.log('Selesai (selalu jalan)'));
        ```
        
45. `[[45. Promise Kombinasi — all, allSettled, race, any]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        const promise1 = ambilData(1); // selesai dalam 500ms
        const promise2 = ambilData(2); // selesai dalam 500ms
        const promise3 = ambilData(3); // selesai dalam 500ms
        
        // Promise.all: tunggu SEMUA selesai, GAGAL jika satu pun gagal
        Promise.all([promise1, promise2, promise3])
          .then(([data1, data2, data3]) => console.log('Semua:', data1, data2, data3))
          .catch(err => console.error('Salah satu gagal:', err));
        
        // Promise.allSettled: tunggu semua, TIDAK gagal meski ada yang error
        Promise.allSettled([promise1, ambilData(-1), promise3])
          .then(results => results.forEach(r => {
            if (r.status === 'fulfilled') console.log('✅', r.value);
            else console.log('❌', r.reason.message);
          }));
        
        // Promise.race: ambil yang PERTAMA selesai (fulfilled atau rejected)
        Promise.race([ambilData(1), ambilData(2)])
          .then(data => console.log('Tercepat:', data));
        
        // Promise.any: ambil yang PERTAMA fulfilled (abaikan yang rejected)
        Promise.any([ambilData(-1), ambilData(2), ambilData(3)])
          .then(data => console.log('Pertama yang berhasil:', data));
        ```
        

---

### O. async/await — Menulis Async seperti Synchronous

46. `[[46. async/await — Sintaks yang Lebih Bersih]]`
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Refactor callback hell menggunakan async/await
        async function ambilSemuaData() {
          try {
            const data1 = await ambilData(1);
            const data2 = await ambilData(2);
            const data3 = await ambilData(3);
            console.log(data1, data2, data3);
            // Tapi ini sequential (total ~1500ms)
          } catch (error) {
            console.error('Error:', error.message);
          }
        }
        
        // Lebih baik: parallel execution
        async function ambilSemuaParalel() {
          try {
            // Semua mulai bersamaan, total ~500ms
            const [data1, data2, data3] = await Promise.all([
              ambilData(1),
              ambilData(2),
              ambilData(3),
            ]);
            console.log(data1, data2, data3);
          } catch (error) {
            console.error('Error:', error.message);
          }
        }
        
        ambilSemuaParalel();
        ```
        

---

### P. Fetch API & HTTP — Terhubung ke Dunia Nyata

47. `[[47. Fetch API — HTTP Request dari JavaScript]]`
    
    - _Langkah konkret_: Bangun aplikasi cuaca menggunakan OpenWeatherMap API:
        
        JavaScript
        
        ```
        // Setup: daftar di openweathermap.org, dapatkan API key gratis
        const API_KEY = 'your_api_key';
        const BASE_URL = 'https://api.openweathermap.org/data/2.5';
        
        async function getCuaca(kota) {
          const url = `${BASE_URL}/weather?q=${kota}&appid=${API_KEY}&units=metric&lang=id`;
          
          const response = await fetch(url);
          
          // fetch() tidak throw error untuk status 4xx/5xx!
          if (!response.ok) {
            throw new Error(`HTTP Error: ${response.status} - Kota tidak ditemukan`);
          }
          
          const data = await response.json();
          return data;
        }
        
        async function tampilkanCuaca(kota) {
          try {
            const cuaca = await getCuaca(kota);
            console.log(`🌡️  ${cuaca.name}: ${cuaca.main.temp}°C`);
            console.log(`☁️  ${cuaca.weather[0].description}`);
            console.log(`💧 Kelembaban: ${cuaca.main.humidity}%`);
            console.log(`💨 Angin: ${cuaca.wind.speed} m/s`);
          } catch (error) {
            console.error('Gagal memuat cuaca:', error.message);
          }
        }
        
        tampilkanCuaca('Jakarta');
        tampilkanCuaca('Bali');
        ```
        
48. `[[48. Fetch dengan POST, PUT, DELETE — Kirim Data ke Server]]`
    
    - _Langkah konkret_: Menggunakan JSONPlaceholder (fake REST API untuk testing):
        
        JavaScript
        
        ```
        const BASE = 'https://jsonplaceholder.typicode.com';
        
        // Helper function: hindari duplikasi kode
        async function fetchJSON(url, options = {}) {
          const response = await fetch(url, {
            headers: { 'Content-Type': 'application/json' },
            ...options,
          });
          
          if (!response.ok) {
            const error = await response.json().catch(() => ({}));
            throw new Error(error.message || `HTTP ${response.status}`);
          }
          
          return response.json();
        }
        
        // GET
        const post = await fetchJSON(`${BASE}/posts/1`);
        
        // POST: buat data baru
        const postBaru = await fetchJSON(`${BASE}/posts`, {
          method: 'POST',
          body: JSON.stringify({ title: 'Judul Baru', body: 'Konten...', userId: 1 }),
        });
        
        // PUT: update lengkap
        const diupdate = await fetchJSON(`${BASE}/posts/1`, {
          method: 'PUT',
          body: JSON.stringify({ id: 1, title: 'Judul Updated', body: 'Konten baru', userId: 1 }),
        });
        
        // PATCH: update sebagian
        const dipatch = await fetchJSON(`${BASE}/posts/1`, {
          method: 'PATCH',
          body: JSON.stringify({ title: 'Hanya Judul yang Berubah' }),
        });
        
        // DELETE
        await fetchJSON(`${BASE}/posts/1`, { method: 'DELETE' });
        ```
        
49. `[[49. Error Handling & Loading State — UX yang Baik]]`
    
    - _Langkah konkret_: Buat wrapper dengan state management:
        
        JavaScript
        
        ```
        // Abstraksi untuk mengelola async state
        function buatAsyncState() {
          return {
            isLoading: false,
            error: null,
            data: null,
            
            async execute(asyncFn) {
              this.isLoading = true;
              this.error = null;
              
              try {
                this.data = await asyncFn();
              } catch (err) {
                this.error = err.message;
              } finally {
                this.isLoading = false;
              }
              
              return this;
            }
          };
        }
        
        const state = buatAsyncState();
        await state.execute(() => getCuaca('Jakarta'));
        
        if (state.isLoading) console.log('Loading...');
        else if (state.error) console.error('Error:', state.error);
        else console.log('Data:', state.data);
        ```
        

---

### 🏗️ Checkpoint Level 5

text

```
✅ Checklist sebelum lanjut ke Level 6:

PROYEK: Aplikasi Cuaca
├── Fetch data dari OpenWeatherMap API
├── GET cuaca berdasarkan nama kota
├── GET forecast 5 hari
├── Error handling: kota tidak ditemukan, network error, API error
├── Loading state: tampilkan loading saat fetch
├── Simpan kota favorit ke localStorage
└── Tampilkan riwayat pencarian

PEMAHAMAN:
├── Bisa jelaskan Event Loop dengan gambar/diagram
├── Bisa jelaskan perbedaan Promise.all vs Promise.allSettled
├── Bisa jelaskan sequential vs parallel async
├── Bisa jelaskan mengapa fetch() tidak throw untuk 4xx/5xx
└── Bisa refactor callback ke Promise ke async/await

Commit: feat: add async programming and weather API integration
```

---

## ⚫ LEVEL 6: DOM MANIPULATION & INTERAKTIVITAS (Minggu 18-21)

> **Tema**: _"Membawa semua yang dipelajari ke UI — membuat halaman web yang benar-benar interaktif"_  
> **Benang Merah**: Semua logika sudah ada (Level 1-5) → hubungkan ke HTML → DOM manipulation → Events → Form handling → Aplikasi web nyata  
> **Output**: To-do list web app yang fully interactive dengan localStorage

---

### Q. DOM — Jembatan antara JavaScript dan HTML

> 💡 **Benang Merah ke HTML/CSS**: DOM adalah representasi HTML sebagai object JavaScript. Semua yang kita pelajari tentang object (Level 2) bisa diaplikasikan ke elemen DOM.

50. `[[50. Mengakses Elemen DOM]]`
    
    - _Langkah konkret_: Tambahkan interaktivitas ke halaman portofolio:
        
        JavaScript
        
        ```
        // Cara modern: querySelector (satu elemen pertama yang cocok)
        const header = document.querySelector('header');
        const navLinks = document.querySelectorAll('.nav-links a');
        const projectCards = document.querySelectorAll('.project-card');
        
        // Cara lama (tapi masih berguna):
        const heroTitle = document.getElementById('hero-title');
        
        // NodeList (dari querySelectorAll) — butuh spread untuk array methods:
        const cards = [...projectCards]; // convert ke array
        cards.forEach(card => console.log(card.textContent));
        
        // Traversal DOM:
        const nav = document.querySelector('nav');
        console.log(nav.parentElement);      // elemen induk
        console.log(nav.children);           // elemen anak langsung
        console.log(nav.nextElementSibling); // saudara berikutnya
        ```
        
51. `[[51. Mengubah DOM — Konten, Atribut & Style]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        const judul = document.querySelector('h1');
        
        // Ubah konten
        judul.textContent = 'Nama Baru'; // aman, tidak parse HTML
        judul.innerHTML = '<span>Nama</span> Baru'; // parse HTML — hati-hati XSS!
        
        // Ubah atribut
        const link = document.querySelector('a');
        link.getAttribute('href');
        link.setAttribute('href', 'https://github.com');
        link.removeAttribute('target');
        
        // Data attributes
        const card = document.querySelector('.project-card');
        card.dataset.id = '001';      // set: <div data-id="001">
        card.dataset.id;              // get: '001'
        
        // Ubah class
        card.classList.add('featured');
        card.classList.remove('hidden');
        card.classList.toggle('active');
        card.classList.contains('featured'); // true/false
        
        // Ubah style langsung (lebih baik gunakan class!)
        card.style.backgroundColor = '#f0f0f0';
        card.style.cssText = 'background: red; color: white;';
        ```
        
52. `[[52. Membuat & Menambahkan Elemen DOM]]`
    
    - _Langkah konkret_: Buat function untuk render project card:
        
        JavaScript
        
        ```
        function buatCardProyek(proyek) {
          const card = document.createElement('article');
          card.className = 'project-card';
          card.dataset.id = proyek.id;
          
          // Template literal untuk HTML kompleks
          card.innerHTML = `
            <figure class="project-card__image-wrapper">
              <img
                src="${proyek.gambar}"
                alt="Screenshot ${proyek.judul}"
                loading="lazy"
              >
            </figure>
            <div class="project-card__body">
              <h3 class="project-card__title">${proyek.judul}</h3>
              <p class="project-card__desc">${proyek.deskripsi}</p>
              <div class="project-card__tags">
                ${proyek.teknologi.map(t => `<span class="tag">${t}</span>`).join('')}
              </div>
              <a href="${proyek.link}" target="_blank" rel="noopener noreferrer" class="btn btn--outline">
                Lihat Proyek →
              </a>
            </div>
          `;
          
          return card;
        }
        
        // Render semua proyek
        const container = document.querySelector('.projects-grid');
        const proyek = [ /* array data proyek */ ];
        
        // DocumentFragment: batch insert (lebih efisien dari insert satu per satu)
        const fragment = document.createDocumentFragment();
        proyek.forEach(p => fragment.appendChild(buatCardProyek(p)));
        container.appendChild(fragment);
        ```
        

---

### R. Events — Merespons Aksi User

> 💡 **Benang Merah ke Callback**: Event listener menggunakan callback function — konsep yang sudah dipahami di Level 3.

53. `[[53. addEventListener & Event Object]]`
    
    - _Langkah konkret_: Tambahkan interaktivitas ke navigasi:
        
        JavaScript
        
        ```
        // addEventListener: cara yang benar
        const btn = document.querySelector('.hamburger-btn');
        const nav = document.querySelector('.nav-links');
        
        btn.addEventListener('click', function(event) {
          // event object berisi informasi tentang event
          console.log(event.type);    // 'click'
          console.log(event.target);  // elemen yang diklik
          console.log(event.currentTarget); // elemen yang punya listener
          
          nav.classList.toggle('is-open');
          
          // Update aria untuk aksesibilitas
          const isOpen = nav.classList.contains('is-open');
          btn.setAttribute('aria-expanded', isOpen);
        });
        
        // Beberapa event type yang penting:
        // click, dblclick, mouseenter, mouseleave, mousemove
        // keydown, keyup, keypress
        // focus, blur, change, input, submit
        // scroll, resize
        // touchstart, touchend (mobile)
        ```
        
54. `[[54. Event Delegation — Teknik Penting untuk Performa]]`
    
    - _Langkah konkret_: Handle klik pada banyak card sekaligus:
        
        JavaScript
        
        ```
        const grid = document.querySelector('.projects-grid');
        
        // ❌ Cara naif: listener di SETIAP card
        // projectCards.forEach(card => card.addEventListener('click', handler));
        // Masalah: banyak listener, card baru tidak punya listener
        
        // ✅ Event Delegation: SATU listener di parent
        grid.addEventListener('click', function(event) {
          // Cari apakah yang diklik ada di dalam .project-card
          const card = event.target.closest('.project-card');
          if (!card) return; // klik di luar card
          
          const id = card.dataset.id;
          console.log('Card diklik, ID:', id);
          
          // Handle klik tombol di dalam card
          if (event.target.matches('.btn--outline')) {
            event.preventDefault(); // cegah navigasi jika perlu
            console.log('Tombol diklik');
          }
        });
        ```
        
55. `[[55. preventDefault & stopPropagation]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // preventDefault: cegah perilaku default browser
        document.querySelector('form').addEventListener('submit', function(event) {
          event.preventDefault(); // cegah page reload (default form submit)
          // Proses form dengan JavaScript
        });
        
        document.querySelector('a').addEventListener('click', function(event) {
          event.preventDefault(); // cegah navigasi
          // Lakukan sesuatu terlebih dahulu
        });
        
        // stopPropagation: hentikan event bubbling ke parent
        const modal = document.querySelector('.modal');
        const modalContent = document.querySelector('.modal-content');
        
        modal.addEventListener('click', () => modal.close()); // klik backdrop = tutup
        modalContent.addEventListener('click', (event) => {
          event.stopPropagation(); // klik isi modal = TIDAK tutup
        });
        ```
        

---

### S. Form Handling — Input dari User

56. `[[56. Membaca & Memvalidasi Form]]`
    
    - _Langkah konkret_: Handle form kontak:
        
        JavaScript
        
        ```
        const form = document.querySelector('#contact-form');
        
        form.addEventListener('submit', async function(event) {
          event.preventDefault();
          
          // Ambil semua nilai form
          const formData = new FormData(form);
          const data = Object.fromEntries(formData.entries());
          // { nama: '...', email: '...', pesan: '...' }
          
          // Validasi
          const errors = validasiForm(data);
          if (Object.keys(errors).length > 0) {
            tampilkanErrors(errors);
            return;
          }
          
          // Kirim ke API
          try {
            tampilkanLoading(true);
            await kirimPesan(data);
            tampilkanSukses('Pesan berhasil dikirim!');
            form.reset();
          } catch (error) {
            tampilkanError(error.message);
          } finally {
            tampilkanLoading(false);
          }
        });
        
        function validasiForm({ nama, email, pesan }) {
          const errors = {};
          if (!nama.trim()) errors.nama = 'Nama wajib diisi';
          if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) errors.email = 'Email tidak valid';
          if (pesan.length < 10) errors.pesan = 'Pesan minimal 10 karakter';
          return errors;
        }
        ```
        
57. `[[57. Real-time Validation & User Feedback]]`
    
    - _Langkah konkret_: Validasi saat user mengetik:
        
        JavaScript
        
        ```
        const emailInput = document.querySelector('#email');
        
        // Debounce: tunggu user berhenti mengetik sebelum validasi
        function debounce(fn, delay) {
          let timeoutId;
          return function(...args) {
            clearTimeout(timeoutId);
            timeoutId = setTimeout(() => fn.apply(this, args), delay);
          };
        }
        
        const validasiEmail = debounce(function(event) {
          const nilai = event.target.value;
          const isValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(nilai);
          
          const errorEl = document.querySelector('#email-error');
          if (!isValid && nilai.length > 0) {
            errorEl.textContent = 'Format email tidak valid';
            emailInput.setAttribute('aria-invalid', 'true');
          } else {
            errorEl.textContent = '';
            emailInput.setAttribute('aria-invalid', 'false');
          }
        }, 300);
        
        emailInput.addEventListener('input', validasiEmail);
        ```
        

---

### T. Browser APIs

58. `[[58. localStorage & sessionStorage]]`
    
    - _Langkah konkret_: Simpan preferensi dan state aplikasi:
        
        JavaScript
        
        ```
        // LocalStorage helper dengan error handling
        const storage = {
          set(key, value) {
            try {
              localStorage.setItem(key, JSON.stringify(value));
            } catch (error) {
              console.error('Gagal menyimpan:', error);
            }
          },
          
          get(key, defaultValue = null) {
            try {
              const item = localStorage.getItem(key);
              return item ? JSON.parse(item) : defaultValue;
            } catch (error) {
              console.error('Gagal membaca:', error);
              return defaultValue;
            }
          },
          
          remove(key) { localStorage.removeItem(key); },
          clear() { localStorage.clear(); },
        };
        
        // Penggunaan:
        storage.set('tema', 'dark');
        storage.set('tugas', [{ id: 1, teks: 'Belajar JS', selesai: false }]);
        const tema = storage.get('tema', 'light');
        const tugas = storage.get('tugas', []);
        ```
        
59. `[[59. Intersection Observer — Lazy Load & Scroll Animation]]`
    
    - _Langkah konkret_: Animasi muncul saat scroll ke project card:
        
        JavaScript
        
        ```
        // Intersection Observer: callback saat elemen masuk/keluar viewport
        const observer = new IntersectionObserver((entries) => {
          entries.forEach(entry => {
            if (entry.isIntersecting) {
              entry.target.classList.add('is-visible');
              observer.unobserve(entry.target); // stop observe setelah terlihat sekali
            }
          });
        }, {
          threshold: 0.1,   // callback saat 10% elemen terlihat
          rootMargin: '0px 0px -50px 0px', // trigger 50px sebelum masuk viewport
        });
        
        // Observe semua project card
        document.querySelectorAll('.project-card').forEach(card => {
          observer.observe(card);
        });
        
        // Di CSS tambahkan:
        // .project-card { opacity: 0; transform: translateY(20px); transition: ... }
        // .project-card.is-visible { opacity: 1; transform: translateY(0); }
        ```
        

---

### 🏗️ Checkpoint Level 6

text

```
✅ Checklist sebelum lanjut ke Level 7:

PROYEK: Portofolio Interaktif Lengkap
├── Data proyek dari JavaScript (bukan hardcode di HTML)
├── Proyek di-render menggunakan DOM manipulation + template
├── Filter proyek berdasarkan teknologi (event delegation)
├── Dark mode toggle (localStorage persist)
├── Form kontak dengan validasi real-time
├── Animasi scroll menggunakan IntersectionObserver
├── Hamburger menu untuk mobile
├── Search/filter dengan debounce
└── Semua interaksi accessible (keyboard, ARIA)

PEMAHAMAN:
├── Bisa jelaskan perbedaan textContent vs innerHTML
├── Bisa jelaskan event bubbling dan kapan stopPropagation
├── Bisa jelaskan event delegation dan mengapa lebih baik
├── Bisa jelaskan perbedaan localStorage vs sessionStorage
└── Bisa membuat Intersection Observer untuk lazy load

Commit: feat: add DOM manipulation, events, form handling - interactive portfolio
```

---

## 💎 LEVEL 7: MODULE, TOOLING & TESTING (Minggu 22-25)

> **Tema**: _"Dari script tag ke project yang terstruktur dengan tooling profesional"_  
> **Benang Merah**: Semua kode di satu file (Level 1-6) → modules untuk organisasi → npm untuk dependency → testing untuk kepercayaan → build untuk production  
> **Output**: Project dengan module system, npm packages, unit tests, dan build pipeline

---

### U. ES Modules — Organisasi Kode

> 💡 **Benang Merah ke Closure**: Module pattern dengan closure (Level 3) adalah cara manual untuk privasi. ES Modules memberikan privasi secara native — setiap file adalah module dengan scope sendiri.

60. `[[60. ES Modules — import & export]]`
    
    - _Langkah konkret_: Pecah kode monolith ke modul:
        
        JavaScript
        
        ```
        // utils/storage.js
        export const storage = {
          set(key, value) { /* ... */ },
          get(key, defaultValue) { /* ... */ },
        };
        
        // utils/api.js
        const BASE_URL = 'https://api.openweathermap.org/data/2.5';
        
        async function fetchJSON(url, options = {}) { /* ... */ }
        
        export async function getCuaca(kota, apiKey) { /* ... */ }
        export async function getForecast(kota, apiKey) { /* ... */ }
        
        // components/weatherCard.js
        export function buatWeatherCard(data) { /* ... */ }
        
        // main.js — entry point
        import { storage } from './utils/storage.js';
        import { getCuaca, getForecast } from './utils/api.js';
        import { buatWeatherCard } from './components/weatherCard.js';
        
        // Default export vs Named export:
        // Named: export const fn = ... → import { fn } from '...'
        // Default: export default fn  → import namaApa from '...'
        ```
        
    - Di HTML, gunakan `type="module"`:
        
        HTML
        
        ```
        <script type="module" src="main.js"></script>
        ```
        
61. `[[61. Dynamic Import — Lazy Loading Modul]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Import saat dibutuhkan, bukan di awal
        async function tampilkanGrafik() {
          // Library berat hanya dimuat saat user klik "Tampilkan Grafik"
          const { Chart } = await import('./lib/chart.js');
          const chart = new Chart(/* ... */);
        }
        
        document.querySelector('#show-chart').addEventListener('click', tampilkanGrafik);
        ```
        

---

### V. NPM & Tooling — Ekosistem Modern

62. `[[62. NPM — Package Manager & Vite Setup]]`
    
    - _Langkah konkret_: Setup project modern dengan Vite:
        
        Bash
        
        ```
        # Buat project baru dengan Vite
        npm create vite@latest portfolio-js -- --template vanilla
        cd portfolio-js
        npm install
        npm run dev
        ```
        
    - Struktur project Vite:
        
        text
        
        ```
        portfolio-js/
        ├── index.html
        ├── main.js        ← entry point
        ├── style.css
        ├── package.json
        ├── vite.config.js
        └── src/
            ├── utils/
            ├── components/
            └── api/
        ```
        
    - `package.json` penting untuk dipahami:
        
        JSON
        
        ```
        {
          "name": "portfolio-js",
          "scripts": {
            "dev": "vite",
            "build": "vite build",
            "preview": "vite preview",
            "lint": "eslint src",
            "test": "vitest"
          },
          "dependencies": { /* diinstall di production */ },
          "devDependencies": { /* hanya untuk development */ }
        }
        ```
        
63. `[[63. ESLint & Prettier — Kode yang Konsisten]]`
    
    - _Langkah konkret_:
        
        Bash
        
        ```
        npm install -D eslint prettier eslint-config-prettier
        npx eslint --init
        ```
        
        JavaScript
        
        ```
        // .eslintrc.json
        {
          "env": { "browser": true, "es2022": true },
          "extends": ["eslint:recommended", "prettier"],
          "rules": {
            "no-console": "warn",
            "no-unused-vars": "error",
            "prefer-const": "error",
            "no-var": "error"
          }
        }
        ```
        
        JSON
        
        ```
        // .prettierrc
        {
          "singleQuote": true,
          "semi": true,
          "tabWidth": 2,
          "trailingComma": "es5"
        }
        ```
        
    - _Langkah konkret_: Jalankan `npm run lint`, perbaiki semua warning/error

---

### W. Testing — Kode yang Bisa Dipercaya

> 💡 **Benang Merah ke Utility Functions**: Di Level 3 kita buat utility library. Sekarang kita test setiap fungsi — memastikan bekerja dengan benar untuk semua kasus.

64. `[[64. Unit Testing dengan Vitest]]`
    
    - _Langkah konkret_:
        
        Bash
        
        ```
        npm install -D vitest
        ```
        
        JavaScript
        
        ```
        // src/utils/storage.test.js
        import { describe, it, expect, beforeEach } from 'vitest';
        import { storage } from './storage.js';
        
        describe('storage', () => {
          beforeEach(() => {
            localStorage.clear(); // bersihkan sebelum setiap test
          });
          
          it('harus menyimpan dan mengambil nilai', () => {
            storage.set('kunci', { nama: 'Budi' });
            expect(storage.get('kunci')).toEqual({ nama: 'Budi' });
          });
          
          it('harus return default value jika key tidak ada', () => {
            expect(storage.get('tidak-ada', 'default')).toBe('default');
          });
          
          it('harus handle nilai null dan undefined', () => {
            storage.set('null', null);
            expect(storage.get('null')).toBeNull();
          });
        });
        ```
        
        Bash
        
        ```
        npm run test
        npm run test -- --coverage
        ```
        
65. `[[65. Test Utility Functions — TDD Approach]]`
    
    - _Langkah konkret_: Test utility library dari Level 3:
        
        JavaScript
        
        ```
        // src/utils/array.test.js
        import { describe, it, expect } from 'vitest';
        import { chunk, groupBy, debounce } from './array.js';
        
        describe('chunk', () => {
          it('memecah array menjadi bagian berukuran n', () => {
            expect(chunk([1, 2, 3, 4, 5], 2)).toEqual([[1, 2], [3, 4], [5]]);
          });
          
          it('handle array kosong', () => {
            expect(chunk([], 2)).toEqual([]);
          });
          
          it('handle size lebih besar dari array', () => {
            expect(chunk([1, 2], 5)).toEqual([[1, 2]]);
          });
        });
        
        describe('groupBy', () => {
          const data = [
            { tipe: 'A', nilai: 1 },
            { tipe: 'B', nilai: 2 },
            { tipe: 'A', nilai: 3 },
          ];
          
          it('mengelompokkan berdasarkan key', () => {
            const result = groupBy(data, 'tipe');
            expect(result.A).toHaveLength(2);
            expect(result.B).toHaveLength(1);
          });
        });
        ```
        

---

### 🏗️ Checkpoint Level 7

text

```
✅ Checklist sebelum lanjut ke Level 8:

PROYEK: Portfolio Refactored (Vite + Modules + Tests)
├── Struktur modul: utils/, components/, api/, pages/
├── Semua import/export menggunakan ES Modules
├── ESLint: 0 errors, minimal warnings
├── Prettier: semua file terformat
├── Vitest: unit test untuk semua utility functions
├── Coverage: minimal 70%
├── npm run build: build berhasil tanpa error
└── npm run preview: preview production build

PEMAHAMAN:
├── Bisa jelaskan perbedaan default vs named export
├── Bisa jelaskan perbedaan dependencies vs devDependencies
├── Bisa tulis test dengan describe, it, expect
├── Bisa jalankan dan baca coverage report
└── Bisa konfigurasi ESLint rules

Commit: feat: migrate to ES modules, add Vite, ESLint, and unit tests
```

---

## 🌟 LEVEL 8: JAVASCRIPT DALAM PRAKTIK NYATA (Minggu 26-30)

> **Tema**: _"Pola, performa, dan keamanan untuk aplikasi production-ready"_  
> **Benang Merah**: Project sudah terstruktur (Level 7) → design patterns → performa → keamanan → siap untuk framework  
> **Output**: Mini e-commerce dengan pattern yang benar, performa optimal, dan keamanan dasar

---

### X. Design Patterns — Solusi untuk Masalah Umum

66. `[[66. Observer Pattern — State yang Disinkronkan]]`
    
    - _Langkah konkret_: Buat simple state management:
        
        JavaScript
        
        ```
        // Event Emitter / Observer Pattern
        class EventEmitter {
          #listeners = new Map();
          
          on(event, listener) {
            if (!this.#listeners.has(event)) {
              this.#listeners.set(event, new Set());
            }
            this.#listeners.get(event).add(listener);
            return () => this.off(event, listener); // return unsubscribe function
          }
          
          off(event, listener) {
            this.#listeners.get(event)?.delete(listener);
          }
          
          emit(event, data) {
            this.#listeners.get(event)?.forEach(listener => listener(data));
          }
        }
        
        // Simple State Store (mirip Vuex/Redux sederhana)
        class Store extends EventEmitter {
          #state;
          
          constructor(initialState) {
            super();
            this.#state = initialState;
          }
          
          get state() { return { ...this.#state }; }
          
          setState(updates) {
            this.#state = { ...this.#state, ...updates };
            this.emit('change', this.#state);
          }
        }
        
        // Penggunaan:
        const store = new Store({ cart: [], total: 0, isLoading: false });
        
        store.on('change', (state) => {
          renderCart(state.cart);
          updateTotal(state.total);
        });
        
        store.setState({ cart: [...store.state.cart, produkBaru] });
        ```
        
67. `[[67. Factory Pattern — Membuat Object dengan Konsisten]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Factory: fungsi yang membuat dan mengembalikan object
        function buatProduk({ id, nama, harga, kategori, stok = 0 }) {
          if (!id || !nama || harga <= 0) {
            throw new Error('Data produk tidak valid');
          }
          
          return Object.freeze({
            id,
            nama,
            harga,
            kategori,
            stok,
            get tersedia() { return this.stok > 0; },
            toJSON() {
              return { id: this.id, nama: this.nama, harga: this.harga };
            }
          });
        }
        
        // Penggunaan:
        const laptop = buatProduk({ id: 'P001', nama: 'Laptop', harga: 15000000, stok: 5 });
        console.log(laptop.tersedia); // true
        ```
        

---

### Y. Performa — Kode yang Efisien

68. `[[68. Debounce & Throttle — Kontrol Frekuensi Eksekusi]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Debounce: tunggu X ms setelah event terakhir
        // Use case: search, validasi form, resize handler
        function debounce(fn, delay) {
          let timeoutId;
          return function(...args) {
            clearTimeout(timeoutId);
            timeoutId = setTimeout(() => fn.apply(this, args), delay);
          };
        }
        
        // Throttle: maksimal sekali setiap X ms
        // Use case: scroll handler, mousemove, API rate limit
        function throttle(fn, limit) {
          let lastRun = 0;
          return function(...args) {
            const now = Date.now();
            if (now - lastRun >= limit) {
              lastRun = now;
              fn.apply(this, args);
            }
          };
        }
        
        // Penggunaan:
        const searchInput = document.querySelector('#search');
        searchInput.addEventListener('input', debounce(cariProduk, 300));
        
        window.addEventListener('scroll', throttle(updateScrollProgress, 100));
        ```
        
69. `[[69. Lazy Loading & Code Splitting]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Lazy load komponen besar hanya saat dibutuhkan
        async function tampilkanGrafik() {
          const { default: Chart } = await import('./Chart.js');
          const chart = new Chart(document.querySelector('#chart'));
          return chart;
        }
        
        // Lazy load gambar dengan IntersectionObserver
        const lazyImages = document.querySelectorAll('img[data-src]');
        
        const imageObserver = new IntersectionObserver((entries) => {
          entries.forEach(entry => {
            if (entry.isIntersecting) {
              const img = entry.target;
              img.src = img.dataset.src;
              img.removeAttribute('data-src');
              imageObserver.unobserve(img);
            }
          });
        });
        
        lazyImages.forEach(img => imageObserver.observe(img));
        ```
        

---

### Z. Keamanan — Kode yang Aman

70. `[[70. XSS Prevention — Input yang Aman]]`
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // ❌ BERBAHAYA: innerHTML dengan input user
        function tampilkanKomentar(komentar) {
          div.innerHTML = komentar; // jika komentar = '<script>...' → XSS!
        }
        
        // ✅ AMAN: gunakan textContent atau sanitasi
        function tampilkanKomentar(komentar) {
          div.textContent = komentar; // otomatis escape HTML
        }
        
        // ✅ Jika butuh render HTML: sanitasi dulu
        function sanitasiHTML(html) {
          const div = document.createElement('div');
          div.textContent = html; // escape
          return div.innerHTML;   // baca sebagai HTML yang sudah di-escape
        }
        
        // ✅ Untuk link: validasi URL
        function sanitasiURL(url) {
          try {
            const parsed = new URL(url);
            // Hanya izinkan http dan https
            if (!['http:', 'https:'].includes(parsed.protocol)) {
              return '#';
            }
            return url;
          } catch {
            return '#';
          }
        }
        ```
        

---

### 🏗️ Proyek Final Level 8

text

```
PROYEK: Mini E-Commerce
─────────────────────────────────────────────────────────────────
Fitur:
├── Katalog produk (fetch dari API atau data lokal)
├── Filter dan search (debounced)
├── Keranjang belanja (localStorage persist)
├── Checkout form dengan validasi
├── State management dengan Observer pattern
├── Lazy load gambar produk
├── XSS prevention untuk semua user input
├── Unit tests: utils, store, validators
└── Build production dengan Vite

Struktur:
├── src/
│   ├── api/          products.js
│   ├── components/   ProductCard, Cart, FilterBar
│   ├── store/        cartStore.js (Observer pattern)
│   ├── utils/        validation.js, storage.js, debounce.js
│   └── main.js
├── tests/
│   └── *.test.js
└── package.json
```

---

## 🏆 LEVEL 9: MASTERY — LANJUTKAN KE SPESIALISASI (Bulan 7+)

> **Tema**: _"Memilih jalur spesialisasi berdasarkan minat dan tujuan karir"_

---

### Jalur A: Frontend Framework

71. `[[71. Mengapa Framework — Masalah yang Diselesaikan]]`
    - _Langkah konkret_: Identifikasi masalah di kode vanilla yang diselesaikan framework:
        
        text
        
        ```
        Masalah: DOM manipulation manual = rentan bug, verbose
        Solusi React/Vue: declarative UI — describe "apa yang ditampilkan" bukan "bagaimana"
        
        Masalah: State yang tersebar → susah disinkronkan
        Solusi: State management terpusat
        
        Masalah: Routing manual → repot
        Solusi: Router yang terintegrasi
        ```
        
    - _Langkah konkret_: Rebuild to-do list menggunakan Vue.js (sesuai roadmap Vue yang sudah ada)

---

### Jalur B: Node.js & Backend

72. `[[72. Node.js Fundamentals — JavaScript di Server]]`
    - _Langkah konkret_: Hello World HTTP server:
        
        JavaScript
        
        ```
        // server.js
        import http from 'http';
        
        const server = http.createServer((req, res) => {
          const { url, method } = req;
          
          if (url === '/api/health' && method === 'GET') {
            res.writeHead(200, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ status: 'ok', timestamp: new Date() }));
          } else {
            res.writeHead(404);
            res.end('Not found');
          }
        });
        
        server.listen(3000, () => console.log('Server berjalan di port 3000'));
        ```
        
    - _Langkah konkret_: Lanjut ke roadmap NestJS yang sudah ada

---

### Jalur C: Advanced JavaScript

73. `[[73. Generator & Iterator — Kontrol Eksekusi yang Presisi]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Generator: fungsi yang bisa di-pause dan di-resume
        function* hitungMundur(dari) {
          while (dari > 0) {
            yield dari--; // pause di sini, kembalikan nilai
          }
        }
        
        const counter = hitungMundur(5);
        console.log(counter.next()); // { value: 5, done: false }
        console.log(counter.next()); // { value: 4, done: false }
        
        // for...of bekerja dengan generator
        for (const n of hitungMundur(3)) {
          console.log(n); // 3, 2, 1
        }
        
        // Infinite sequence (lazy evaluation):
        function* angkaPrima() {
          function isPrima(n) {
            for (let i = 2; i <= Math.sqrt(n); i++) {
              if (n % i === 0) return false;
            }
            return n > 1;
          }
          
          let n = 2;
          while (true) {
            if (isPrima(n)) yield n;
            n++;
          }
        }
        
        // Ambil 10 prima pertama:
        const prima = angkaPrima();
        const sepuluhPrima = Array.from({ length: 10 }, () => prima.next().value);
        console.log(sepuluhPrima); // [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
        ```
        
74. `[[74. Proxy & Reflect — Metaprogramming]]`
    
    - _Langkah konkret_:
        
        JavaScript
        
        ```
        // Proxy: intercept operasi pada object
        function buatValidator(target, validasi) {
          return new Proxy(target, {
            set(obj, prop, nilai) {
              if (validasi[prop] && !validasi[prop](nilai)) {
                throw new TypeError(`Nilai tidak valid untuk ${prop}`);
              }
              obj[prop] = nilai;
              return true;
            }
          });
        }
        
        const pengguna = buatValidator({}, {
          umur: (v) => typeof v === 'number' && v >= 0 && v <= 150,
          email: (v) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(v),
        });
        
        pengguna.umur = 25;    // ✅
        pengguna.email = 'ok@email.com'; // ✅
        pengguna.umur = -1;   // ❌ TypeError: Nilai tidak valid untuk umur
        ```
        

---

## 📊 Ringkasan Progress Tracking

### Satu Project, 8 Level Enhancement

text

```
Level 1: Console playground — variabel, tipe data, operator
  + Level 2: + Control flow, array, object → To-do list console
  + Level 3: + Fungsi, closure, HOF → Utility library
  + Level 4: + OOP, class → Sistem perpustakaan
  + Level 5: + Async, fetch → Integrasi API nyata
  + Level 6: + DOM, events → Portfolio interaktif
  + Level 7: + Modules, Vite, testing → Project terstruktur
  + Level 8: + Design patterns, performa, keamanan → Mini e-commerce
```

### Tabel Progress

|Level|Poin|Durasi|Output Konkret|
|---|---|---|---|
|🟢 **1**|1-12|Minggu 1-3|Kalkulator di console, paham tipe data|
|🟡 **2**|13-26|Minggu 4-6|To-do list console dengan localStorage|
|🟠 **3**|27-35|Minggu 7-9|Utility library dengan closure + HOF|
|🔴 **4**|36-41|Minggu 10-13|Sistem perpustakaan OOP|
|🟣 **5**|42-49|Minggu 14-17|Aplikasi cuaca dari API nyata|
|⚫ **6**|50-59|Minggu 18-21|Portfolio interaktif dengan DOM|
|💎 **7**|60-65|Minggu 22-25|Project Vite + modules + tests|
|🌟 **8**|66-70|Minggu 26-30|Mini e-commerce production-ready|
|🏆 **9**|71-74|Bulan 7+|Spesialisasi pilihan|

---

### Benang Merah Utama Sepanjang Roadmap

text

```
Poin 5  (const/let)         → Digunakan di semua level berikutnya
Poin 6  (Tipe data)         → Fondasi type coercion (Poin 8)
Poin 11 (?? dan ?.)         → Pattern aman untuk optional data
Poin 20 (map/filter/reduce) → Core FP yang didalami di Poin 32
Poin 22 (Array destructuring) → Digunakan di async/await (Poin 46)
Poin 31 (Closure)           → Fondasi module pattern, HOF, async
Poin 33 (Callback)          → Fondasi Promise (Poin 44)
Poin 44 (Promise)           → Fondasi async/await (Poin 46)
Poin 47 (Fetch API)         → Digunakan di semua DOM project
Poin 52 (DOM rendering)     → Dioptimasi di Poin 59 (IntersectionObserver)
Poin 53 (Event delegation)  → Pola untuk semua event handling
Poin 58 (localStorage)      → Digunakan di semua project dari Level 2
Poin 60 (ES Modules)        → Dasar semua project Level 7+
Poin 66 (Observer pattern)  → Dasar state management (preview Vue/React)
```

---

## 💡 Cara Menggunakan Roadmap Ini

text

```
Setiap poin mengikuti format:
┌──────────────────────────────────────────────────────┐
│ 💡 Konteks: mengapa konsep ini ada                   │
│ 🔗 Benang Merah: koneksi ke poin sebelum/sesudah    │
│ 📋 Kode: implementasi konkret yang langsung bisa    │
│          dicoba di console atau di project           │
│ ✅ Langkah konkret: verifikasi berhasil             │
└──────────────────────────────────────────────────────┘
```

**Aturan yang Tidak Boleh Dilanggar:**

1. **Buka browser console** — setiap konsep baru, test dulu di console
2. **Baca error message** — error JavaScript sangat informatif, baca sebelum panik
3. **Tulis kode, jangan copy** — muscle memory dibangun dengan mengetik
4. **Selesaikan checkpoint** — jangan lanjut sebelum proyek level selesai
5. **Commit setiap progres** — git history adalah jejak pembelajaran
6. **Breakpoint, bukan console.log** — gunakan debugger untuk bug yang susah

---

_Roadmap JavaScript v1.0 — Step-by-Step, One Project, Understanding First_  
_Setiap baris kode ditulis karena alasan yang jelas, bukan karena "sudah kebiasaan"_