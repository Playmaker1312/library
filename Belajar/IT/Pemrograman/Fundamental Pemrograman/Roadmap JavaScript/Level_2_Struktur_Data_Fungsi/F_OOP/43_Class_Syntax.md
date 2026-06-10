# 43 — Class Syntax: Cara Modern Mendefinisikan Object

## 1. Penjelasan

Sejak ES6 (2015), JavaScript memperkenalkan keyword `class`. **Class di JS bukan class murni seperti Java** — ia adalah *syntactic sugar* di atas prototype chain. Di belakang layar, class tetaplah function dengan `.prototype`.

### Syntax Class:

```javascript
class NamaClass {
  constructor(params) { /* inisialisasi */ }
  method1() { /* method biasa */ }
  static method2() { /* method milik class, bukan instance */ }
  #privateField;
  get properti() { /* getter */ }
  set properti(val) { /* setter */ }
}
```

### Fitur Class Modern:

| Fitur | Syntax | Fungsi |
|-------|--------|--------|
| **constructor()** | `constructor(nama) { this.nama = nama; }` | Dipanggil otomatis saat `new` |
| **Method** | `method() {}` | Ditambahkan ke `.prototype` |
| **Static method** | `static method() {}` | Milik class, bisa dipanggil tanpa instance |
| **Private field** | `#field` | Hanya bisa diakses dari dalam class |
| **Private method** | `#method() {}` | Method yang hanya bisa dipanggil internal |
| **Getter** | `get field() {}` | Seperti properti tapi ada logika |
| **Setter** | `set field(val) {}` | Validasi saat assignment |

---

## 2. Fungsi

- Membuat object dengan struktur yang rapi dan konsisten
- Enkapsulasi data via private field (`#`)
- Validasi data via setter
- Organisasi method yang lebih bersih daripada constructor function tradisional

---

## 3. Code

```javascript
class BankAccount {
  // Private fields
  #saldo;
  #history;

  constructor(namaPemilik, saldoAwal = 0) {
    this.namaPemilik = namaPemilik;
    this.#saldo = saldoAwal;
    this.#history = [];
    this.#catat('pembukaan', saldoAwal);
  }

  // Method biasa — ditambahkan ke BankAccount.prototype
  deposit(jumlah) {
    if (jumlah <= 0) {
      console.log('Jumlah deposit harus positif');
      return false;
    }
    this.#saldo += jumlah;
    this.#catat('deposit', jumlah);
    return true;
  }

  withdraw(jumlah) {
    if (jumlah <= 0) {
      console.log('Jumlah withdraw harus positif');
      return false;
    }
    if (jumlah > this.#saldo) {
      console.log('Saldo tidak mencukupi');
      return false;
    }
    this.#saldo -= jumlah;
    this.#catat('withdraw', jumlah);
    return true;
  }

  // Getter — akses seperti properti, bukan method
  get saldo() {
    return this.#saldo;
  }

  // Setter — validasi saat assignment
  set saldo(jumlah) {
    if (jumlah < 0) {
      console.log('Saldo tidak boleh negatif');
      return;
    }
    this.#saldo = jumlah;
  }

  // Private method
  #catat(tipe, jumlah) {
    this.#history.push({ tipe, jumlah, waktu: new Date() });
  }

  // Static method — milik class, bukan instance
  static formatRupiah(jumlah) {
    return `Rp${jumlah.toLocaleString('id-ID')}`;
  }

  static infoBank() {
    return 'Bank JavaScript — Aman & Modern';
  }
}

// ==== Penggunaan ====
const akunBudi = new BankAccount('Budi', 500000);
akunBudi.deposit(200000);
akunBudi.withdraw(100000);

console.log(akunBudi.saldo);                // 600000 — getter
console.log(BankAccount.formatRupiah(akunBudi.saldo)); // Rp600.000 — static
console.log(akunBudi.namaPemilik);          // 'Budi' — public

// akunBudi.#saldo = 0;     ❌ SyntaxError (private)
// akunBudi.#catat();       ❌ SyntaxError (private)
// akunBudi.saldo = -50000; ❌ Setter tolak saldo negatif
// BankAccount.saldo;       ❌ Static bukan milik instance
// akunBudi.infoBank();     ❌ Instance tidak punya akses static

// ==== Bukti masih berbasis prototype ====
console.log(typeof BankAccount);          // 'function'
console.log(akunBudi.__proto__ === BankAccount.prototype); // true
console.log(akunBudi.deposit === BankAccount.prototype.deposit); // true
```

---

## 4. Analogi Rumah

| Konsep Class | Analogi Cetakan Rumah |
|--------------|------------------------|
| **Class** | Cetakan / blueprint rumah di atas kertas |
| **Instance** | Rumah fisik yang dibangun dari cetakan tersebut |
| **constructor()** | Proses pembangunan: menyiapkan lahan, fondasi |
| **Property** | Fitur rumah: luas tanah, jumlah kamar, warna cat |
| **Method** | Fungsi rumah: buka jendela, nyalakan lampu |
| **Private field #** | Ruang bawah tanah atau brankas — hanya pemilik yang tahu isinya |
| **Static method** | Alat tukang bangunan — milik kontraktor (class), bukan milik rumah (instance) |
| **Getter** | Termometer di dinding — lihat suhu tanpa menyentuh AC |
| **Setter** | Thermostat — set suhu dengan validasi (jangan terlalu panas/dingin) |
| **Instance (new)** | Membangun rumah baru dari cetakan yang sama |

**Narasi:** Seorang arsitek membuat cetak biru rumah — spesifikasi detail: jumlah kamar, letak jendela, tinggi plafon. Ini adalah **class**. Kontraktor mengambil cetakan itu dan membangun rumah fisik — itu **instance** (`new Rumah()`). Dari satu cetakan bisa dibangun **banyak rumah** (banyak instance), masing-masing bisa punya warna cat berbeda (property berbeda) tapi tetap punya struktur yang sama (method sama). Static method adalah seperti **nomor telepon kontraktor** — Anda tidak perlu punya rumah untuk menghubungi kontraktor. Private field `#` adalah **brankas** di dalam rumah — hanya pemilik rumah yang tahu kombinasi dan isinya.

---

## 5. Use Case

- **DOM class** — `class Button { constructor(text) { ... } render() { ... } }`
- **Model data** — `class User`, `class Product`, `class Order`
- **API Service** — `class ApiService { static get(endpoint) { ... } }`
- **Utility class** — `class Validator { static isEmail(str) { ... } }`
- **State management** — `class Store { #state; getState() { ... } }`

---

## 6. Kesalahan Umum

1. **Lupa `new`** — `const a = BankAccount('Budi')` → `undefined` dan properti jadi global. Class HARUS dipanggil dengan `new`.
2. **Mengira class hoisted** — Class tidak di-hoist seperti function. Harus didefinisikan dulu sebelum dipakai.
3. **Private field belum didukung semua engine** — `#field` relatif baru, pastikan target environment mendukung.
4. **Getter/setter bukan method biasa** — `akun.saldo()` itu panggil method, bukan getter. Getter dipanggil tanpa `()`.
5. **Static dipanggil via instance** — `akunBudi.formatRupiah()` ❌. Static hanya bisa `ClassName.method()`.

---

## 7. Benang Merah

| Sebelumnya (Materi 42) | Di sini (Materi 43) | Selanjutnya (Materi 44) |
|------------------------|---------------------|-------------------------|
| Prototype chain — fondasi OOP JS | Class syntax — gula sintaksis untuk prototype | Inheritance — extends & super |

Prototype chain adalah fondasi (bagaimana JS bekerja di belakang layar). Class syntax adalah cara modern menulisnya. Selanjutnya kita akan lihat bagaimana class bisa saling mewarisi via `extends`.

---

## 8. Soal

### Soal 1 — Implementasi Class
Buat class `Kamar` dengan:
- Private field `#luas`, `#penghuni`
- Constructor menerima `luas`
- Getter `luas`
- Setter `penghuni` — hanya terima string, tolak jika bukan string
- Method `info()` — return `"Kamar ${luas}m², dihuni oleh ${penghuni}"`

<details><summary>Jawaban</summary>

```javascript
class Kamar {
  #luas;
  #penghuni = 'kosong';

  constructor(luas) {
    this.#luas = luas;
  }

  get luas() {
    return this.#luas;
  }

  set penghuni(nama) {
    if (typeof nama !== 'string') {
      console.log('Penghuni harus berupa nama (string)');
      return;
    }
    this.#penghuni = nama;
  }

  info() {
    return `Kamar ${this.#luas}m², dihuni oleh ${this.#penghuni}`;
  }
}
```
</details>

### Soal 2 — Static vs Instance
Apa output dari kode berikut? Jelaskan.
```javascript
class Kalkulator {
  static PI = 3.14;
  #hasil = 0;

  tambah(x) { this.#hasil += x; return this; }
  static kali(a, b) { return a * b; }
}

const calc = new Kalkulator();
console.log(calc.PI);
console.log(Kalkulator.PI);
console.log(calc.tambah(5).tambah(3));
console.log(Kalkulator.kali(4, 5));
```

<details><summary>Jawaban</summary>

1. `undefined` — `PI` adalah static, tidak bisa diakses via instance.
2. `3.14` — static diakses via class.
3. `Kalkulator { #hasil: 8 }` — method chaining dengan `return this`.
4. `20` — static method `kali(4,5)` = 20.
</details>

### Soal 3 — Refactor Constructor Function ke Class
Refactor kode berikut ke class syntax dengan private field:
```javascript
function Lemari(warna) {
  this.warna = warna;
  this._isi = [];
}
Lemari.prototype.buka = function () {
  return 'Lemari ' + this.warna + ' terbuka';
};
Lemari.prototype.simpan = function (barang) {
  this._isi.push(barang);
};
```

<details><summary>Jawaban</summary>

```javascript
class Lemari {
  #isi = [];

  constructor(warna) {
    this.warna = warna;
  }

  buka() {
    return `Lemari ${this.warna} terbuka`;
  }

  simpan(barang) {
    this.#isi.push(barang);
  }
}
```
</details>
