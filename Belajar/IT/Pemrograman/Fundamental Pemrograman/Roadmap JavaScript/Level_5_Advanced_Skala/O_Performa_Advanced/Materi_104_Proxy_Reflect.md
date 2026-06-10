# 104 — Proxy & Reflect — Metaprogramming

## 1. Penjelasan

**Proxy** adalah mekanisme di JavaScript yang memungkinkan Anda membuat objek "perantara" untuk meng-intercept (menyadap) operasi-operasi dasar pada objek target. Operasi yang bisa di-intercept antara lain: membaca properti (`get`), menulis properti (`set`), mengecek keberadaan properti (`has`), menghapus properti (`deleteProperty`), dan lain-lain.

**Reflect** adalah objek built-in yang menyediakan method untuk melakukan operasi objek standar. Setiap method di Reflect memiliki pasangan dengan handler di Proxy. Reflect digunakan untuk meneruskan operasi default ke target setelah proses intercept selesai.

```javascript
const target = { name: 'Rumah' };
const handler = {
  get(target, prop, receiver) {
    console.log(`Membaca properti "${prop}"`);
    return Reflect.get(target, prop, receiver);
  },
};
const proxy = new Proxy(target, handler);
console.log(proxy.name); // Membaca properti "name" → "Rumah"
```

## 2. Fungsi

- **Validasi otomatis** — memvalidasi nilai sebelum disimpan ke objek
- **Logging & tracing** — mencatat setiap akses ke properti objek
- **Reactive system** — memicu re-render saat data berubah (Vue 3 reactivity)
- **Property normalization** — mengubah format nilai saat get/set (misal: uppercase, trim)
- **Security layer** — membatasi akses ke properti tertentu
- **Virtual properties** — properti yang nilainya dihitung saat diminta, tidak disimpan

## 3. Code

### Validation Proxy

```javascript
const validator = {
  set(target, prop, value) {
    if (prop === 'umur') {
      if (!Number.isInteger(value)) {
        throw new TypeError('Umur harus berupa angka bulat');
      }
      if (value < 0 || value > 150) {
        throw new RangeError('Umur harus antara 0-150');
      }
    }
    if (prop === 'email') {
      if (!value.includes('@')) {
        throw new Error('Email tidak valid');
      }
    }
    // Operasi default: simpan nilai
    return Reflect.set(target, prop, value);
  },

  get(target, prop) {
    if (prop in target) {
      console.log(`Akses properti "${prop}"`);
      return Reflect.get(target, prop);
    }
    return `Properti "${prop}" tidak ditemukan`;
  },

  deleteProperty(target, prop) {
    if (prop === 'id') {
      throw new Error('Tidak bisa menghapus properti id');
    }
    return Reflect.deleteProperty(target, prop);
  },
};

const user = new Proxy({ id: 1, name: 'Budi' }, validator);
user.umur = 25; // OK
user.email = 'budi@mail.com'; // OK
// user.umur = -5; // RangeError: Umur harus antara 0-150
// delete user.id; // Error: Tidak bisa menghapus properti id
console.log(user.namaLengkap); // Properti "namaLengkap" tidak ditemukan
```

### Reactive System (Mini Vue)

```javascript
function reactive(target, onChange) {
  return new Proxy(target, {
    set(target, prop, value) {
      const oldValue = target[prop];
      const result = Reflect.set(target, prop, value);
      if (oldValue !== value) {
        onChange(prop, value, oldValue);
      }
      return result;
    },
  });
}

const state = reactive({ count: 0, name: 'Aplikasi' }, (prop, newVal, oldVal) => {
  console.log(`State berubah: ${prop} dari ${oldVal} ke ${newVal}`);
  // Di Vue: trigger re-render komponen
});

state.count = 1; // State berubah: count dari 0 ke 1
state.name = 'Dashboard'; // State berubah: name dari Aplikasi ke Dashboard
```

### Logging Proxy

```javascript
function createLoggingProxy(target, label = 'Object') {
  return new Proxy(target, {
    get(target, prop, receiver) {
      console.log(`[${label}] GET ${String(prop)}`);
      return Reflect.get(target, prop, receiver);
    },
    set(target, prop, value, receiver) {
      console.log(`[${label}] SET ${String(prop)} = ${value}`);
      return Reflect.set(target, prop, value, receiver);
    },
  });
}

const config = createLoggingProxy({ theme: 'dark', lang: 'id' }, 'Config');
config.theme; // [Config] GET theme
config.lang = 'en'; // [Config] SET lang = en
```

## 4. Analogi Rumah — CCTV & Sensor

| Konsep JS | Analogi Rumah |
|-----------|---------------|
| Proxy | CCTV & sensor yang dipasang di depan pintu rumah |
| Target object | Rumah itu sendiri |
| Handler (trap) | Aturan CCTV — siapa boleh masuk, jam berapa, barang apa yang dibawa |
| `get` trap | CCTV membaca wajah orang yang mau masuk |
| `set` trap | Sensor pintu — mengecek barang yang dibawa masuk |
| `has` trap | Satpam mengecek apakah seseorang ada di daftar tamu |
| `deleteProperty` | Melarang orang membuang barang tertentu dari rumah |
| Reflect | Aksi nyata setelah lolos cek — membukakan pintu, mempersilakan masuk |
| Proxy tanpa Reflect | CCTV mendeteksi orang tapi pintu tidak terbuka — operasi default tidak terjadi |

> **Alur:** Orang datang → CCTV/Proxy menangkap kedatangan → cek aturan (handler) → lolos → Reflect membukakan pintu (operasi default) → orang masuk ke rumah (target).

## 5. Use Case di Proyek Nyata

- **Vue 3 Reactivity** — Proxy adalah fondasi sistem reaktif Vue 3 (menggantikan Object.defineProperty di Vue 2)
- **MobX** — decorator observable menggunakan Proxy
- **Immer** — immutable state management dengan Proxy (produce function)
- **Form validation** — validasi input form real-time via proxy
- **API mocking** — mock response API dengan Proxy untuk testing
- **Data binding** — sinkronisasi otomatis antara model dan view

## 6. Kesalahan Umum

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| Lupa return `Reflect.*` di handler | Operasi default tidak berjalan | Selalu panggil `Reflect.method(target, ...args)` |
| Tidak handle `receiver` dengan benar | Masalah dengan inheritance | Teruskan `receiver` ke `Reflect.*` |
| Proxy di dalam Proxy (nesting) | Performa turun drastis | Hindari nested proxy tanpa alasan kuat |
| Menganggap proxy transparan | `===` tidak bekerja, `instanceof` bermasalah | Sadari bahwa proxy adalah objek berbeda |
| Tidak handle `deleteProperty` | Operasi hapus tidak dicegah | Tambahkan trap jika perlu proteksi |

## 7. Benang Merah

```
Materi 103 (Generator & Iterator)
    ↓
Materi 104 (Proxy & Reflect — Metaprogramming) ← Anda di sini
    ↓
Materi 105 (Memory Management & Memory Leaks)
```

Setelah menguasai alur eksekusi bertahap dengan generator, Anda sekarang bisa mengontrol operasi fundamental objek dengan Proxy & Reflect. Metaprogramming memberi Anda kekuatan untuk membangun sistem yang lebih cerdas (seperti reactive framework). Selanjutnya, kekuatan ini harus diimbangi dengan pemahaman manajemen memori (Materi 105).

## 8. Soal & Jawaban

### Soal 1: Easy
Buatlah Proxy sederhana untuk objek `person = { name: 'Ana' }` yang mencatat (console.log) setiap kali properti `name` diakses (get).

<details>
<summary>Jawaban</summary>

```javascript
const person = { name: 'Ana' };
const handler = {
  get(target, prop) {
    if (prop === 'name') {
      console.log('Properti name diakses');
    }
    return Reflect.get(target, prop);
  },
};
const proxyPerson = new Proxy(person, handler);
console.log(proxyPerson.name);
// Properti name diakses
// Ana
```
</details>

### Soal 2: Medium
Buatlah Proxy `protectedObject` yang melindungi properti yang diawali dengan underscore (`_`). Jika ada akses get ke properti `_...`, lempar error "Akses ditolak". Jika ada set ke properti `_...`, lempar error juga. Properti lain boleh diakses normal.

<details>
<summary>Jawaban</summary>

```javascript
function createProtectedObject(target) {
  return new Proxy(target, {
    get(target, prop) {
      if (String(prop).startsWith('_')) {
        throw new Error(`Akses ditolak: properti "${String(prop)}" bersifat private`);
      }
      return Reflect.get(target, prop);
    },
    set(target, prop, value) {
      if (String(prop).startsWith('_')) {
        throw new Error(`Akses ditolak: tidak bisa mengubah "${String(prop)}" dari luar`);
      }
      return Reflect.set(target, prop, value);
    },
  });
}

const user = createProtectedObject({
  _secret: 'rahasia',
  name: 'Budi',
});

console.log(user.name); // Budi
// console.log(user._secret); // Error: Akses ditolak
```
</details>

### Soal 3: Hard
Buatlah fungsi `createObservable(obj, callback)` yang mengembalikan proxy. Setiap kali properti objek diubah, panggil `callback(propName, newValue, oldValue)`. Jika properti yang diubah adalah nested object, jadikan juga observable (recursive proxy).

<details>
<summary>Jawaban</summary>

```javascript
function createObservable(obj, callback) {
  return new Proxy(obj, {
    set(target, prop, value, receiver) {
      const oldValue = target[prop];
      // Jika value adalah objek, buat observable juga
      if (value !== null && typeof value === 'object') {
        value = createObservable(value, callback);
      }
      const result = Reflect.set(target, prop, value, receiver);
      if (oldValue !== value) {
        callback(prop, value, oldValue);
      }
      return result;
    },
    get(target, prop, receiver) {
      const value = Reflect.get(target, prop, receiver);
      // Lazy observable untuk nested object
      if (value !== null && typeof value === 'object' && !value.__isProxy) {
        const observed = createObservable(value, callback);
        Reflect.set(target, prop, observed);
        return observed;
      }
      return value;
    },
  });
}

const data = createObservable(
  { user: { name: 'Budi', age: 20 } },
  (prop, newVal, oldVal) => {
    console.log(`Perubahan: ${prop} = ${JSON.stringify(newVal)} (sebelum: ${JSON.stringify(oldVal)})`);
  }
);

data.user.name = 'Ana';
// Perubahan: name = "Ana" (sebelum: "Budi")
// Perubahan: user = {"name":"Ana"} (sebelum: {"name":"Budi"})
```
</details>
