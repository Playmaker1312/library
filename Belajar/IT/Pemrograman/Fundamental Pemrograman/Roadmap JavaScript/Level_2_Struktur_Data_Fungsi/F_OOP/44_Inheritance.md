# 44 — Inheritance: extends & super

## 1. Penjelasan

**Inheritance (pewarisan)** memungkinkan sebuah class (child) mewarisi properti dan method dari class lain (parent). Di JavaScript, ini diimplementasikan dengan keyword `extends` dan `super`.

### Keyword:

| Keyword | Fungsi |
|---------|--------|
| `extends` | Menyatakan class anak mewarisi dari class induk |
| `super()` | Memanggil constructor parent dari constructor child (WAJIB dipanggil sebelum `this`) |
| `super.method()` | Memanggil method parent yang sudah di-override |

### Aturan Penting:
- Wajib panggil `super(params)` di constructor child sebelum akses `this`
- `super()` otomatis memanggil constructor parent
- Method parent bisa di-override di child, dan tetap bisa dipanggil via `super.method()`
- Inheritance menciptakan prototype chain: `child → Child.prototype → Parent.prototype → Object.prototype`

---

## 2. Fungsi

- **Code reuse** — method yang sama tidak perlu ditulis ulang
- **Hierarki** — membuat hubungan "is-a" (CheckingAccount **is a** BankAccount)
- **Extension** — menambah fitur baru tanpa mengubah parent
- **Polymorphism** — method yang sama bisa punya implementasi berbeda di child

---

## 3. Code

```javascript
// ====== PARENT CLASS ======
class BankAccount {
  #saldo;

  constructor(namaPemilik, saldoAwal = 0) {
    this.namaPemilik = namaPemilik;
    this.#saldo = saldoAwal;
  }

  deposit(jumlah) {
    if (jumlah <= 0) return false;
    this.#saldo += jumlah;
    return true;
  }

  withdraw(jumlah) {
    if (jumlah <= 0 || jumlah > this.#saldo) return false;
    this.#saldo -= jumlah;
    return true;
  }

  get saldo() {
    return this.#saldo;
  }

  info() {
    return `Rekening ${this.namaPemilik}: Saldo ${this.#saldo}`;
  }
}

// ====== CHILD 1: SavingsAccount ======
class SavingsAccount extends BankAccount {
  #bunga;

  constructor(namaPemilik, saldoAwal = 0, bunga = 0.03) {
    super(namaPemilik, saldoAwal); // WAJIB — panggil constructor parent
    this.#bunga = bunga;
  }

  // Method BARU — tidak ada di parent
  terapkanBunga() {
    const bunga = this.saldo * this.#bunga; // pakai getter, bukan #saldo
    this.deposit(bunga); // pakai method deposit parent
    return `Bunga Rp${bunga} telah ditambahkan`;
  }

  // Override method info()
  info() {
    return `${super.info()} | Bunga: ${(this.#bunga * 100)}% per tahun`;
  }
}

// ====== CHILD 2: CheckingAccount ======
class CheckingAccount extends BankAccount {
  #limitOverdraft;

  constructor(namaPemilik, saldoAwal = 0, limitOverdraft = 500000) {
    super(namaPemilik, saldoAwal);
    this.#limitOverdraft = limitOverdraft;
  }

  // Override withdraw — boleh minus sampai limit
  withdraw(jumlah) {
    if (jumlah <= 0) return false;
    if (jumlah > this.saldo + this.#limitOverdraft) return false;
    // Panggil super.withdraw(), tapi jika saldo kurang, manual kurangi
    if (jumlah > this.saldo) {
      const sisa = this.saldo - jumlah;
      // Kita perlu akses saldo dari parent — gunakan metode deposit/withdraw
      // Alternatif: parent#saldo tidak bisa diakses, jadi kita override logika
      return false; // Sederhanakan: tolak jika melebihi saldo untuk demo
    }
    return super.withdraw(jumlah);
  }

  info() {
    return `${super.info()} | Limit Overdraft: ${this.#limitOverdraft}`;
  }
}

// ====== Penggunaan ======
const tabungan = new SavingsAccount('Budi', 1000000, 0.05);
tabungan.deposit(500000);
console.log(tabungan.info());
// "Rekening Budi: Saldo 1500000 | Bunga: 5% per tahun"
console.log(tabungan.terapkanBunga());
// "Bunga Rp75000 telah ditambahkan"

const giro = new CheckingAccount('Ani', 2000000);
giro.withdraw(500000);
console.log(giro.info());
// "Rekening Ani: Saldo 1500000 | Limit Overdraft: 500000"

// ====== instanceof ======
console.log(tabungan instanceof SavingsAccount);  // true
console.log(tabungan instanceof BankAccount);      // true
console.log(tabungan instanceof Object);           // true
console.log(tabungan instanceof CheckingAccount);  // false
```

---

## 4. Analogi Rumah

| Konsep Inheritance | Analogi Rumah |
|--------------------|---------------|
| **Parent class** | Rumah tapak standar — pondasi, tembok, atap standar |
| **Child class** | Rumah turunan — Rumah Mezzanine, Rumah Loft |
| **extends** | "Dibangun berdasarkan desain..." |
| **super() constructor** | Bangun pondasi & struktur dasar dulu, baru tambah fitur spesifik |
| **super.method()** | "Seperti biasa, tapi..." — pakai cara lama lalu tambah modifikasi |
| **Override** | Rumah loft tetap punya dapur, tapi atapnya diganti kaca |
| **Method baru** | Rumah mezzanine punya tangga tambahan — fitur yang tidak ada di rumah standar |
| **instanceof** | "Apakah rumah ini termasuk kategori rumah tapak?" |
| **Private field (#)** | Ruang bawah tanah — anak tidak bisa akses langsung, harus lewat pintu (getter) |

**Narasi:** Sebuah pengembang perumahan memiliki **desain rumah tapak standar** — 2 kamar, 1 kamar mandi, dapur, ruang tamu. Itu adalah **parent class `RumahTapak`**. Kemudian pengembang ingin membuat **Rumah Mezzanine** — mewarisi semua fitur rumah tapak, tapi ditambah lantai mezzanine dan tangga. Inilah **inheritance**: `class RumahMezzanine extends RumahTapak`. Di dalam constructor, panggil `super()` dulu untuk membangun struktur dasarnya (pondasi, tembok, atap), baru tambahkan lantai mezzanine. Child bisa **override** method asli — misalnya `hitungLuas()` milik parent (luas lantai dasar) di-override jadi luas total (lantai dasar + mezzanine). Tapi child tetap bisa akses method parent via `super.hitungLuas()` untuk menghitung luas dasar dulu, baru tambah luas mezzanine.

---

## 5. Use Case

- **UI Components** — `class Button extends Component`, `class Modal extends Component`
- **Model bisnis** — `class User extends Model`, `class Admin extends User`
- **Error handling** — `class HttpError extends Error`, `class ValidationError extends Error`
- **Klasifikasi** — `class Vehicle`, `class Car extends Vehicle`, `class Motorcycle extends Vehicle`

---

## 6. Kesalahan Umum

1. **Lupa `super()`** — Error: `Must call super constructor in derived class before accessing 'this'`.
2. **Hierarki terlalu dalam** — `A → B → C → D → E`. Sulit di-debug, kaku.
3. **Memaksa inheritance untuk "has-a"** — `class Engine extends Car` ❌ (Engine bukan Car, Car **has** Engine).
4. **Override tanpa `super` jika masih butuh** — Kadang override harus tetap panggil parent method.
5. **Private field tidak diwarisi** — Anak tidak bisa akses `#field` parent secara langsung.

---

## 7. Benang Merah

| Sebelumnya (Materi 43) | Di sini (Materi 44) | Selanjutnya (Materi 45) |
|------------------------|---------------------|-------------------------|
| Class syntax — membuat class | Inheritance — mewarisi class | Komposisi vs Inheritance — kapan pakai inheritance, kapan komposisi |

Class memberi cetakan. Inheritance memberi hubungan hierarki antara cetakan. Tapi inheritance bukan satu-satunya cara — selanjutnya kita lihat komposisi sebagai alternatif yang lebih fleksibel.

---

## 8. Soal

### Soal 1 — Implementasi Inheritance
Buat class `Hewan` dengan method `bersuara()` return `'...'` dan `makan()` return `'Nyam nyam'`. Lalu class `Kucing extends Hewan` override `bersuara()` return `'Meow!'`. Buat juga class `Anjing extends Hewan` override `bersuara()` return `'Guk!'`.

<details><summary>Jawaban</summary>

```javascript
class Hewan {
  bersuara() { return '...'; }
  makan() { return 'Nyam nyam'; }
}

class Kucing extends Hewan {
  bersuara() { return 'Meow!'; }
}

class Anjing extends Hewan {
  bersuara() { return 'Guk!'; }
}

const kucing = new Kucing();
console.log(kucing.bersuara()); // 'Meow!'
console.log(kucing.makan());    // 'Nyam nyam' — dari parent

const anjing = new Anjing();
console.log(anjing.bersuara()); // 'Guk!'
```
</details>

### Soal 2 — super()
```javascript
class Induk {
  constructor(nama) { this.nama = nama; }
  sapa() { return `Halo, saya ${this.nama}`; }
}

class Anak extends Induk {
  constructor(nama, hobi) {
    // ???
    this.hobi = hobi;
  }
  sapa() {
    return `${super.sapa()} dan saya suka ${this.hobi}`;
  }
}
```
Apa yang salah? Perbaiki.

<details><summary>Jawaban</summary>

```javascript
class Anak extends Induk {
  constructor(nama, hobi) {
    super(nama); // WAJIB panggil super() sebelum this
    this.hobi = hobi;
  }
  sapa() {
    return `${super.sapa()} dan saya suka ${this.hobi}`;
  }
}
```

Error asli: `Must call super constructor in derived class before accessing 'this'`.
</details>

### Soal 3 — instanceof
```javascript
class A {}
class B extends A {}
class C extends B {}

const obj = new C();
// 1. obj instanceof C ?
// 2. obj instanceof A ?
// 3. obj instanceof Object ?
// 4. ({}).__proto__ === obj.__proto__.__proto__.__proto__.__proto__ ?
```
Jawab untuk setiap poin.

<details><summary>Jawaban</summary>

1. `true` — obj adalah instance C langsung.
2. `true` — C → B → A, jadi obj termasuk turunan A.
3. `true` — semua object turunan Object.
4. `true` — prototype chain: obj → C.prototype → B.prototype → A.prototype → Object.prototype → null. Jadi `obj.__proto__.__proto__.__proto__.__proto__` adalah `Object.prototype`, dan `({}).__proto__` juga `Object.prototype`.
</details>
