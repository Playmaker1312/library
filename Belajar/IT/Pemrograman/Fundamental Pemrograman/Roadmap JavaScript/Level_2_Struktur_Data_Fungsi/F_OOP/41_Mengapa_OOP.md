# 41 — Mengapa OOP: Masalah yang Diselesaikan

## 1. Penjelasan

OOP (Object-Oriented Programming) adalah paradigma yang mengorganisir kode ke dalam **object** — entitas yang menggabungkan data (properti) dan perilaku (method). Tanpa OOP, program besar menjadi kumpulan fungsi dan variabel yang sulit dilacak, rawan *side effect*, dan susah dikelola.

OOP menyelesaikan 4 masalah utama program besar:
| Masalah | Tanpa OOP | Dengan OOP |
|---------|-----------|------------|
| **Kode tersebar** | Fungsi & variabel global saling mempengaruhi | Data & method terenkapsulasi dalam object |
| **Duplikasi** | Copy-paste kode serupa berkali-kali | Inheritance mewarisi kode |
| **Sulit diperluas** | Ubah satu bagian berantakan ke bagian lain | Polymorphism memungkinkan perilaku berbeda dengan antarmuka sama |
| **Tidak terstruktur** | Tidak ada batas tanggung jawab yang jelas | Abstraction menyembunyikan kompleksitas |

### 4 Pilar OOP:
1. **Encapsulation** — Data disembunyikan, hanya bisa diakses via method tertentu.
2. **Inheritance** — Sebuah class bisa mewarisi properti & method dari class lain.
3. **Polymorphism** — Method yang sama bisa berperilaku berbeda di class berbeda.
4. **Abstraction** — Detail implementasi disembunyikan, hanya interface yang ditampilkan.

---

## 2. Fungsi

- Mengorganisir kode dalam unit yang kohesif (object/class)
- Membatasi akses data (enkapsulasi) → mengurangi bug karena perubahan tak sengaja
- Memungkinkan *code reuse* via inheritance & composition
- Membuat kode lebih mudah diuji, di-debug, dan diperluas

---

## 3. Code

```javascript
// ========== TANPA OOP (Prosedural) — masalah ==========
let saldo = 1000000;
let history = [];

function deposit(jumlah) {
  saldo += jumlah;
  history.push({ tipe: 'deposit', jumlah, waktu: new Date() });
}

function withdraw(jumlah) {
  if (jumlah > saldo) {
    console.log('Saldo tidak cukup');
    return;
  }
  saldo -= jumlah;
  history.push({ tipe: 'withdraw', jumlah, waktu: new Date() });
}

function cekSaldo() {
  return saldo;
}

// Masalah: saldo dan history bisa diubah langsung dari mana saja
saldo = 0; // bug! sengaja atau tidak
console.log(cekSaldo()); // 0 — padahal seharusnya 1.000.000

// ========== DENGAN OOP — terorganisir ==========
class BankAccount {
  #saldo; // encapsulation — private field
  #history;

  constructor(saldoAwal = 0) {
    this.#saldo = saldoAwal;
    this.#history = [];
  }

  deposit(jumlah) {
    this.#saldo += jumlah;
    this.#history.push({ tipe: 'deposit', jumlah, waktu: new Date() });
  }

  withdraw(jumlah) {
    if (jumlah > this.#saldo) {
      console.log('Saldo tidak cukup');
      return false;
    }
    this.#saldo -= jumlah;
    this.#history.push({ tipe: 'withdraw', jumlah, waktu: new Date() });
    return true;
  }

  cekSaldo() {
    return this.#saldo;
  }

  getHistory() {
    return [...this.#history];
  }
}

const akun = new BankAccount(1000000);
akun.deposit(500000);
akun.withdraw(200000);
console.log(akun.cekSaldo()); // 1.300.000
// akun.#saldo = 0; ❌ Error — tidak bisa akses private field
```

---

## 4. Analogi Rumah

| Konsep OOP | Analogi Rumah |
|------------|---------------|
| **Object** | Sebuah rumah jadi |
| **Class** | Cetakan/blueprint rumah |
| **Encapsulation** | Dinding & atap — isi dalam rumah tidak bisa dilihat dari luar langsung |
| **Inheritance** | Rumah tapak → rumah mezzanine (mewarisi struktur dasar) |
| **Polymorphism** | Pintu yang sama, bisa dibuka (geser, dorong, putar) |
| **Abstraction** | Saklar lampu — kita tekan saja, tidak perlu tahu wiring listrik di dalam |

**Narasi:** Bayangkan membangun rumah tanpa cetak biru. Tukang datang, bikin tembok asal-asalan, pipa sembarangan, kabel kusut. Mau renovasi? Berantakan semua. Itu **prosedural**. Sekarang bayangkan semua ruangan punya **cetakan (blueprint)** — kamar tidur selalu sama formatnya, dapur standar, kamar mandi standar. Setiap ruangan adalah **object** dari sebuah **class**. Mau tambah ruangan? Tinggal ambil cetakan. Mau ganti denah? Tinggal ubah cetakannya. Itulah yang OOP lakukan untuk kode Anda.

---

## 5. Use Case

- **Aplikasi perbankan** — setiap nasabah adalah object `Account` dengan encapsulation saldo
- **Game** — setiap karakter (Player, Enemy, NPC) adalah object dengan method `attack()`, `move()`, `takeDamage()`
- **E-commerce** — class `Product`, `Cart`, `Order`, `User` dengan relasi jelas
- **UI Components** — class `Button`, `Modal`, `Dropdown` yang bisa di-inherit dan di-customize

---

## 6. Kesalahan Umum

1. **Memaksa OOP** — Semua kode dipaksa OOP padahal fungsi sederhana cukup pakai function biasa.
2. **Encapsulation diabaikan** — Semua properti public, tidak beda dengan kode prosedural.
3. **Hierarki terlalu dalam** — Inheritance 5+ level, bikin kode kaku dan susah diubah.
4. **Lupa bahwa JS bukan Java** — OOP di JS berbasis prototype, bukan class-based murni.

---

## 7. Benang Merah

| Sebelumnya (Materi 40) | Di sini (Materi 41) | Selanjutnya (Materi 42) |
|------------------------|---------------------|-------------------------|
| Modules — cara mengorganisir file | OOP — cara mengorganisir **kode di dalam file** ke dalam object | Prototype chain — fondasi OOP JavaScript |

Modul memisahkan file. OOP memisahkan tanggung jawab di dalam file. Keduanya bekerja bersama untuk kode yang rapi.

---

## 8. Soal

### Soal 1 — Identifikasi
```javascript
let nama = 'Budi';
let umur = 25;
function sapa() { return `Halo, ${nama}`; }
function ulangTahun() { umur++; }
```
Dari kode prosedural di atas, refactor menjadi class `Person` dengan properti private `#nama` dan `#umur`, serta method `sapa()` dan `ulangTahun()`.

<details><summary>Jawaban</summary>

```javascript
class Person {
  #nama;
  #umur;

  constructor(nama, umur) {
    this.#nama = nama;
    this.#umur = umur;
  }

  sapa() {
    return `Halo, ${this.#nama}`;
  }

  ulangTahun() {
    this.#umur++;
  }
}
```
</details>

### Soal 2 — 4 Pilar
Dari 4 pilar OOP, mana yang **paling penting** menurut Anda untuk menjaga agar program tetap aman dari modifikasi data sembarangan? Jelaskan.

<details><summary>Jawaban</summary>

**Encapsulation**. Dengan menyembunyikan data (`#properti private`) dan hanya menyediakan getter/setter atau method tertentu untuk mengubahnya, kita mencegah data diubah secara tidak sengaja dari luar class. Ini pondasi keamanan data di OOP.
</details>

### Soal 3 — Refactor
Diberikan 3 fungsi yang saling terkait:
```javascript
let items = [];
function tambah(nama) { items.push(nama); }
function hapus(nama) { items = items.filter(i => i !== nama); }
function lihat() { return [...items]; }
```
Refactor menjadi class `TodoList` dengan `#items` sebagai private field.

<details><summary>Jawaban</summary>

```javascript
class TodoList {
  #items = [];

  tambah(nama) {
    this.#items.push(nama);
  }

  hapus(nama) {
    this.#items = this.#items.filter(i => i !== nama);
  }

  lihat() {
    return [...this.#items];
  }
}
```
</details>
