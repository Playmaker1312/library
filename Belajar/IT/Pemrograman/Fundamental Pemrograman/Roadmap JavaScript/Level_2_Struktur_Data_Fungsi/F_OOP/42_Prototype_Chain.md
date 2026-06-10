# 42 — Prototype Chain: Fondasi OOP JavaScript

## 1. Penjelasan

Berbeda dengan Java/C++ yang menggunakan **class-based inheritance**, JavaScript menggunakan **prototype-based inheritance**. Setiap object di JavaScript memiliki *prototype* — object lain yang diwarisi properti dan methodnya. Prototype membentuk rantai yang disebut **prototype chain**.

### Konsep Kunci:

| Istilah | Penjelasan |
|---------|------------|
| **Prototype** | Object induk yang propertinya diwarisi oleh object anak |
| **`__proto__`** | Properti pada instance yang merujuk ke prototype object induk (non-standar, `[[Prototype]]` secara internal) |
| **`.prototype`** | Properti khusus pada **function** (khususnya constructor function & class) — object yang akan menjadi `__proto__` dari instance |
| **Object.create()** | Membuat object baru dengan prototype tertentu |
| **Prototype chain** | Rantai prototype dari object → prototype → prototype berikutnya → null |

### Cara Kerja:

Ketika mengakses `obj.property`:
1. Cek apakah `obj` punya property itu sendiri (*own property*).
2. Jika tidak, naik ke `obj.__proto__` (prototype object).
3. Jika tidak ada, naik ke `__proto__` dari prototype (kakek), dan seterusnya.
4. Sampai `null` — jika tidak ditemukan, return `undefined`.

---

## 2. Fungsi

- **Code reuse** — method diwarisi otomatis tanpa perlu copy-paste
- **Dynamic inheritance** — perubahan di prototype langsung terlihat di semua instance
- **Mendasari class syntax** — class di JS hanyalah *syntactic sugar* di atas prototype

---

## 3. Code

```javascript
// ====== Prototype Chain pada Built-in Object ======
const arr = [1, 2, 3];
// arr → Array.prototype → Object.prototype → null

console.log(arr.__proto__ === Array.prototype);           // true
console.log(arr.__proto__.__proto__ === Object.prototype); // true
console.log(arr.__proto__.__proto__.__proto__);            // null

// ====== Prototype Chain pada Custom Object ======
function Rumah(ukuran) {
  this.ukuran = ukuran;
}

Rumah.prototype.nyalakanLampu = function () {
  return 'Lampu di rumah ukuran ' + this.ukuran + ' menyala';
};

const rmh1 = new Rumah('sedang');
console.log(rmh1.nyalakanLampu()); // 'Lampu di rumah ukuran sedang menyala'

// Trace chain:
console.log(rmh1.__proto__ === Rumah.prototype);           // true
console.log(rmh1.__proto__.__proto__ === Object.prototype); // true
console.log(rmh1.__proto__.__proto__.__proto__);            // null

// ====== Object.create() ======
const cetakanRumah = {
  bukaPintu() { return 'Pintu terbuka'; },
  atap: 'genteng'
};

const rumahA = Object.create(cetakanRumah);
rumahA.warna = 'merah';

console.log(rumahA.bukaPintu()); // 'Pintu terbuka' — dari prototype
console.log(rumahA.atap);        // 'genteng' — dari prototype
console.log(rumahA.warna);       // 'merah' — own property
console.log(rumahA.__proto__ === cetakanRumah); // true

// ====== __proto__ vs .prototype ======
console.log(Rumah.prototype);      // { nyalakanLampu: f } — object constructor function
console.log(rmh1.__proto__);       // { nyalakanLampu: f } — prototype dari instance
console.log(Rumah.prototype === rmh1.__proto__); // true — keduanya merujuk sama
```

### Visual Prototype Chain:

```
rmh1
  ├── .ukuran → 'sedang' (own property)
  ├── [[Prototype]] (__proto__) → Rumah.prototype
  │     ├── .nyalakanLampu → f()
  │     ├── [[Prototype]] → Object.prototype
  │           ├── .toString → f()
  │           ├── .hasOwnProperty → f()
  │           ├── [[Prototype]] → null  ← AKHIR RANTAI
```

---

## 4. Analogi Rumah

| Konsep Prototype | Analogi Keluarga |
|------------------|------------------|
| **Object instance** | Cucu (anak) |
| **`__proto__`** | "Ayah dari" — hubungan langsung ke orang tua |
| **`.prototype`** | Harta pusaka yang akan diwariskan ke anak (milik orang tua, bukan anak) |
| **Prototype chain** | Silsilah keluarga: cucu → anak → kakek → kakek buyut → ... → null |
| **Own property** | Harta pribadi (mainan milik sendiri) |
| **Inherited property** | Harta warisan (rumah dari orang tua) |
| **Object.create()** | Anak adopsi — silsilahnya bisa diatur beda |
| **Method overriding** | Cucu punya cara sendiri melakukan sesuatu, beda dari orang tuanya |

**Narasi:** Dalam sebuah keluarga, seorang anak mewarisi sifat dari ayahnya (rambut ikal, mata coklat). Ayah mewarisi dari kakek (tinggi, hidung mancung). Dan seterusnya. Ini **prototype chain** — properti tidak harus dimiliki langsung oleh seseorang, bisa diwariskan dari atas. Jika sang anak punya rambut lurus padahal ayah ikal, maka yang dipakai punya anak sendiri → **overriding**. `__proto__` adalah "ayah dari", `prototype` adalah "harta yang akan diwariskan". Ketika `Rumah.prototype` berisi method `nyalakanLampu()`, maka semua instance rumah akan mewarisi kemampuan itu, seperti semua cucu mewarisi bakat musik dari kakeknya.

---

## 5. Use Case

- **Polyfill / shim** — Menambahkan method ke `Array.prototype` atau `String.prototype` (hati-hati)
- **Object.create()** untuk delegasi method tanpa constructor function
- **Framework pattern** — React hooks, Vue reactivity memanfaatkan prototype chain
- **Monkey patching** — Override method bawaan (jarang, dan tidak disarankan di production)

---

## 6. Kesalahan Umum

1. **Mengira class = class-based** — Class JS tetap prototype di belakang layar.
2. **Lupa membedakan `__proto__` dan `.prototype`** — `__proto__` milik instance, `.prototype` milik **function**.
3. **Memodifikasi built-in prototype** — Menambah method ke `Array.prototype` bisa break kode lain.
4. **Mengira `Object.create(null)` punya method** — Object tanpa prototype tidak punya `.toString()`, `.hasOwnProperty()`, dll.
5. **Prototype chain terlalu panjang** — Performance degrade karena pencarian properti naik rantai.

---

## 7. Benang Merah

| Sebelumnya (Materi 41) | Di sini (Materi 42) | Selanjutnya (Materi 43) |
|------------------------|---------------------|-------------------------|
| Mengapa OOP — 4 pilar OOP | Prototype chain — fondasi OOP **JavaScript** | Class syntax — cara modern menggunakan prototype |

Materi 41 menjelaskan *mengapa* OOP dibutuhkan. Materi 42 menjelaskan *bagaimana* JavaScript mewujudkan OOP — melalui prototype chain. Materi 43 akan menunjukkan *gula sintaksis* yang membuat prototype lebih mudah digunakan.

---

## 8. Soal

### Soal 1 — Trace Prototype Chain
Diberikan:
```javascript
function Hewan(nama) { this.nama = nama; }
Hewan.prototype.bersuara = function() { return '...'; };

const kucing = new Hewan('Kitty');
kucing.bersuara = function() { return 'Meow!'; };

console.log(kucing.bersuara());
console.log(kucing.toString());
console.log(kucing.__proto__.__proto__ === Object.prototype);
```
Apa output dari setiap `console.log`? Jelaskan.

<details><summary>Jawaban</summary>

1. `'Meow!'` — own property `bersuara` ditemukan di instance, tidak perlu naik ke prototype.
2. `'[object Object]'` — method `toString()` tidak ada di instance, tidak ada di `Hewan.prototype`, ditemukan di `Object.prototype`.
3. `true` — prototype chain: `kucing → Hewan.prototype → Object.prototype → null`.
</details>

### Soal 2 — __proto__ vs .prototype
Jelaskan perbedaan antara `array.__proto__` dan `Array.prototype`. Apakah keduanya sama? Kenapa?

<details><summary>Jawaban</summary>

Ya, `array.__proto__ === Array.prototype`. `Array.prototype` adalah object prototype yang disediakan oleh constructor `Array`. `__proto__` adalah properti pada setiap instance yang menunjuk ke prototype tersebut. Jadi jika instance adalah hasil dari `new Array()` (atau `[]`), maka `__proto__`-nya akan sama dengan `Array.prototype`.
</details>

### Soal 3 — Object.create()
```javascript
const pondasi = { bahan: 'batu' };
const tembok = Object.create(pondasi);
tembok.bahan = 'bata';
const atap = Object.create(tembok);

console.log(atap.bahan);
console.log(atap.__proto__ === tembok);
console.log(tembok.__proto__ === pondasi);
```
Apa outputnya?

<details><summary>Jawaban</summary>

1. `'bata'` — prototype chain: `atap → tembok`. `bahan` ditemukan di `tembok` (own property), tidak perlu lanjut ke `pondasi`.
2. `true` — `Object.create(tembok)` membuat object dengan `__proto__` = `tembok`.
3. `true` — `Object.create(pondasi)` membuat object dengan `__proto__` = `pondasi`.
</details>
