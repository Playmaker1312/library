# 45 — Komposisi vs Inheritance

## 1. Penjelasan

Ada dua cara utama untuk menggunakan kode dari class lain:

| Konsep | Definisi | Analogi |
|--------|----------|---------|
| **Inheritance ("is-a")** | Anak **adalah** jenis dari induk. `class Mobil extends Kendaraan` | Rumah tapak → rumah mezzanine |
| **Komposisi ("has-a")** | Sebuah object **memiliki** object lain. `class Mobil { mesin = new Mesin() }` | Rumah LEGO — susun dari blok-blok kecil |

### Masalah Inheritance yang Dalam (Deep Inheritance Hierarchy):

```
Animal → Mammal → Dog → Poodle → ToyPoodle → ... (KAKU)
```

Setiap level menambah ketergantungan. Ubah di level atas? Berpotensi rusak semua level bawah.

### Komposisi dengan Mixin Pattern:

Mixin adalah object yang berisi method-method yang bisa "dicampur" ke class manapun.

**Prinsip:** *Favor composition over inheritance* — lebih utamakan komposisi daripada pewarisan.

---

## 2. Fungsi

- Menghindari hierarki yang kaku dan dalam
- Memungkinkan *code reuse* tanpa hubungan "is-a"
- Lebih fleksibel — susun ulang fitur seperti LEGO
- Testing lebih mudah — tiap komponen independen

---

## 3. Code

```javascript
// ========== MASALAH: Inheritance terlalu dipaksakan ==========
class BankAccount {
  constructor(saldo = 0) { this.saldo = saldo; }
  deposit(j) { this.saldo += j; }
  withdraw(j) { if (j <= this.saldo) { this.saldo -= j; return true; } return false; }
}

class SavingsAccount extends BankAccount {
  constructor(saldo = 0, bunga = 0.03) { super(saldo); this.bunga = bunga; }
  terapkanBunga() { this.saldo += this.saldo * this.bunga; }
}

// Mau bikin akun yang bisa deposit crypto? Harus bikin class baru lagi?
// Mau bikin akun dengan fitur notifikasi? Extends lagi?
// Ujungnya: CryptoNotifSavingsAccount extends SavingsAccount — RIBET!

// ========== SOLUSI: Komposisi dengan Mixin ==========
// 1. Buat mixin (object berisi method)
const BungaMixin = {
  setBunga(persen) {
    this.bunga = persen;
  },
  terapkanBunga() {
    if (this.bunga && this.saldo !== undefined) {
      this.saldo += this.saldo * this.bunga;
      console.log(`Bunga ${this.bunga * 100}% diterapkan. Saldo: ${this.saldo}`);
    }
  }
};

const NotifikasiMixin = {
  setNotif(mode) {
    this.notifMode = mode;
  },
  kirimNotif(pesan) {
    if (this.notifMode === 'email') console.log(`📧 ${pesan}`);
    else if (this.notifMode === 'sms') console.log(`📱 ${pesan}`);
    else console.log(`🔔 ${pesan}`);
  }
};

const CryptoMixin = {
  setDompet(alamat) {
    this.dompetCrypto = alamat;
  },
  depositCrypto(jumlah, koin) {
    console.log(`${jumlah} ${koin} dikirim ke ${this.dompetCrypto}`);
  }
};

// 2. Fungsi untuk menggabungkan mixin ke class
function mix(targetClass, ...mixins) {
  Object.assign(targetClass.prototype, ...mixins);
}

// 3. Class dasar — sederhana, tanpa hierarki
class BankAccount {
  constructor(nama, saldo = 0) {
    this.nama = nama;
    this.saldo = saldo;
  }
  deposit(j) { this.saldo += j; }
  withdraw(j) { if (j <= this.saldo) { this.saldo -= j; return true; } return false; }
  info() { return `Saldo ${this.nama}: Rp${this.saldo}`; }
}

// 4. Komposisi — pilih fitur yang dibutuhkan
class SavingsAccount extends BankAccount {}
mix(SavingsAccount, BungaMixin, NotifikasiMixin);

class CryptoAccount extends BankAccount {}
mix(CryptoAccount, CryptoMixin, NotifikasiMixin);

class FullFeatureAccount extends BankAccount {}
mix(FullFeatureAccount, BungaMixin, CryptoMixin, NotifikasiMixin);

// ====== Penggunaan ======
const tabungan = new SavingsAccount('Budi');
tabungan.setBunga(0.05);
tabungan.setNotif('email');
tabungan.deposit(1000000);
tabungan.terapkanBunga();  // Bunga 5% diterapkan
tabungan.kirimNotif('Saldo Anda bertambah');
// 📧 Saldo Anda bertambah
console.log(tabungan.info()); // Saldo Budi: Rp1050000

const crypto = new CryptoAccount('Ani');
crypto.setDompet('0xABC123');
crypto.setNotif('sms');
crypto.depositCrypto(0.5, 'ETH');
// 0.5 ETH dikirim ke 0xABC123
crypto.kirimNotif('Deposit crypto berhasil');
// 📱 Deposit crypto berhasil

console.log(crypto.info()); // Saldo Ani: Rp0 (saldo rupiah)
```

---

## 4. Analogi Rumah

| Konsep | Analogi Rumah |
|--------|---------------|
| **Inheritance** | **Rumah cetakan** — cetakan rumah tapak menghasilkan rumah tapak. Ingin rumah mezzanine? Buat cetakan baru yang extends cetakan lama. Mau ubah bentuk jendela semua tipe? Ubah cetakan induk — beresiko semua rumah berubah. |
| **Komposisi** | **Rumah LEGO** — satu blok adalah komponen kecil (dapur blok, kamar blok, atap blok). Tinggal susun sesuai kebutuhan. Ingin rumah dengan 2 dapur? Tinggal tambah blok dapur lagi. Mau ganti atap? Lepas blok atap, pasang yang baru. |
| **Mixin** | **Modul pre-fab** — modul dapur instan, modul kamar mandi, modul balkon. Ambil modul yang dibutuhkan, tempelkan ke rumah. |
| **Deep inheritance** | Rumah yang dibangun di atas rumah lain di atas rumah lain — kalau pondasi bawah goyang, semua goyang. |

**Narasi:** Bayangkan Anda seorang arsitek yang diminta membuat berbagai tipe rumah. Dengan **inheritance**, Anda membuat cetakan "Rumah Dasar", lalu "Rumah Dengan Garasi extends Rumah Dasar", lalu "Rumah Dengan Garasi Dan Kolam extends Rumah Dengan Garasi". Besok klien minta rumah dengan kolam tapi tanpa garasi — Anda harus buat cabang inheritance baru. Kaku. Dengan **komposisi** (LEGO), Anda punya blok: `Dapur`, `KamarTidur`, `Garasi`, `Kolam`, `Balkon`. Klien minta rumah dengan garasi dan kolam? Tinggal susun: `rumah.tambah(Garasi).tambah(Kolam)`. Klien minta tanpa garasi? Tinggal lepas blok garasi. Fleksibel. **Mixin** seperti modul pre-fab — ambil modul "Atap Miring", tempelkan ke rumah mana pun. Ini prinsip *favor composition over inheritance*: jangan memaksa hierarki kaku jika menyusun komponen lebih masuk akal.

---

## 5. Use Case

| Situasi | Pakai Inheritance | Pakai Komposisi |
|---------|-------------------|-----------------|
| "Kucing **adalah** Hewan" | ✅ | ❌ |
| "Mobil **memiliki** Mesin" | ❌ | ✅ |
| Hierarki stabil, beda tipis | ✅ | Bisa juga |
| Banyak kombinasi fitur | ❌ (ledakan class) | ✅ |
| Butuh fleksibilitas tinggi | ❌ | ✅ |

- **React** — komposisi (children, props) lebih utama daripada inheritance
- **Game development** — entity component system (ECS): Player punya `HealthComponent`, `MovementComponent`, `RenderComponent`
- **Middleware/plugin system** — Express middleware adalah komposisi fungsi

---

## 6. Kesalahan Umum

1. **Memaksakan "is-a"** — `class Employee extends Person` oke, tapi `class Manager extends Employee` sering bermasalah karena role bisa berubah.
2. **Deep inheritance** — 5+ level. Solusi: komposisi.
3. **Lupa bahwa komposisi lebih sederhana** — Inheritance sering membuat masalah baru.
4. **Mixin conflict** — Dua mixin punya method nama sama → yang terakhir menang.
5. **Over-engineering** — Komposisi juga bisa berlebihan. Untuk dua class mirip, inheritance lebih cepat.

---

## 7. Benang Merah

| Sebelumnya (Materi 44) | Di sini (Materi 45) | Selanjutnya |
|------------------------|---------------------|-------------|
| Inheritance — mewarisi class | Komposisi vs Inheritance — kapan pilih mana | **Level 2 SELESAI → Level 3: Event Loop** |

Inheritance dan komposisi bukan musuh — keduanya alat. Inheritance untuk hubungan "is-a" yang stabil. Komposisi untuk fleksibilitas. Pilih yang tepat. Materi ini menutup F_OOP dan Level 2 secara keseluruhan. Selanjutnya Anda akan masuk ke Level 3: Event Loop — jantung asinkron JavaScript.

---

## 8. Soal

### Soal 1 — Identifikasi: Inheritance atau Komposisi?
Klasifikasikan mana yang lebih cocok pakai inheritance dan mana yang komposisi:
1. `Robot` dan `Manusia` → keduanya punya method `bergerak()` dan `makan()` (berbeda)
2. `MobilSport` dan `Mobil` → `MobilSport` punya fitur `turbo()` tambahan
3. `Meja` dan `Kursi` → keduanya punya properti `bahan` dan `warna`

<details><summary>Jawaban</summary>

1. **Komposisi** — `Robot` dan `Manusia` tidak punya hubungan "is-a". Lebih baik buat mixin `BisaBergerak` dan `BisaMakan` lalu campur ke kedua class.
2. **Inheritance** — `MobilSport extends Mobil` karena "MobilSport **adalah** Mobil". Hubungan hierarkis jelas dan stabil.
3. **Komposisi** — `Meja` dan `Kursi` adalah *furniture* yang berbeda. Buat class `Furniture` sebagai induk atau komposisi properti `bahan` dan `warna`.
</details>

### Soal 2 — Refactor Inheritance ke Komposisi
```javascript
class Mesin {
  nyalakan() { return 'Mesin menyala'; }
}
class Mobil extends Mesin {
  jalan() { return 'Mobil berjalan'; }
}
```
Apa masalahnya? Refactor pakai komposisi.

<details><summary>Jawaban</summary>

Masalah: `Mobil extends Mesin` berarti "Mobil **adalah** Mesin" — salah secara logika. Seharusnya "Mobil **memiliki** Mesin". Dengan komposisi:

```javascript
class Mesin {
  nyalakan() { return 'Mesin menyala'; }
}

class Mobil {
  constructor() {
    this.mesin = new Mesin(); // komposisi: Mobil HAS-A Mesin
  }
  jalan() { return 'Mobil berjalan'; }
  nyalakanMesin() { return this.mesin.nyalakan(); }
}
```
</details>

### Soal 3 — Buat Mixin
Buat dua mixin: `BisaTerbang` (method `terbang()`) dan `BisaBerenang` (method `berenang()`). Lalu buat class `Bebek` yang menggunakan kedua mixin tersebut, dan class `Pesawat` yang hanya menggunakan `BisaTerbang`.

<details><summary>Jawaban</summary>

```javascript
const BisaTerbang = {
  terbang() {
    return `${this.nama || 'Sesuatu'} terbang di langit`;
  }
};

const BisaBerenang = {
  berenang() {
    return `${this.nama || 'Sesuatu'} berenang di air`;
  }
};

function mix(targetClass, ...mixins) {
  Object.assign(targetClass.prototype, ...mixins);
}

class Bebek {
  constructor(nama) { this.nama = nama; }
}
mix(Bebek, BisaTerbang, BisaBerenang);

class Pesawat {
  constructor(nama) { this.nama = nama; }
}
mix(Pesawat, BisaTerbang);

const donal = new Bebek('Donal');
console.log(donal.terbang());  // Donal terbang di langit
console.log(donal.berenang()); // Donal berenang di air

const garuda = new Pesawat('Garuda');
console.log(garuda.terbang());  // Garuda terbang di langit
// garuda.berenang(); ❌ Tidak punya method berenang
```
</details>
