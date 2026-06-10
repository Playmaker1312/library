# 36. this Keyword — Konteks yang Berubah-ubah

---

## 1. Penjelasan

**`this`** adalah kata kunci khusus di JavaScript yang merujuk pada **konteks eksekusi** — objek yang "memiliki" fungsi saat ini. Nilai `this` **tidak tetap**; ia bergantung pada **bagaimana** fungsi DIPANGGIL, bukan di mana dideklarasikan.

| Cara Pemanggilan | Nilai `this` |
|------------------|--------------|
| Global (non-strict) | `window` (browser) / `global` (Node) |
| Global (strict mode) | `undefined` |
| Method objek | Objek pemilik method |
| Fungsi biasa (strict) | `undefined` |
| Constructor (`new`) | Instance baru |
| Arrow function | Scope **lexical** (dari luar) |
| `.call()` / `.apply()` / `.bind()` | Argumen yang diberikan |

---

## 2. Fungsi `this`

- **Mengidentifikasi** objek pemilik konteks eksekusi
- **Memungkinkan** method berbagi logika dengan objek berbeda (via call/apply/bind)
- **Membedakan** regular function vs arrow function
- **Dasar** OOP JavaScript (Materi 41-45)

---

## 3. Code — Debug 5 Kode dengan Masalah `this`

### Kode 1: this di global (non-strict)
```javascript
console.log(this); // window (browser) / {} (Node)
```

### Kode 2: Method object — this hilang
```javascript
const user = {
    name: "Budi",
    greet: function() {
        console.log(`Halo, ${this.name}`);
    }
};
const greetFn = user.greet;
greetFn(); // "Halo, undefined" — this hilang!

// Perbaikan: .bind()
const boundGreet = user.greet.bind(user);
boundGreet(); // "Halo, Budi"
```

### Kode 3: Arrow function tidak punya this sendiri
```javascript
const user2 = {
    name: "Siti",
    greet: () => {
        console.log(`Halo, ${this.name}`);
    }
};
user2.greet(); // "Halo, undefined" — arrow ambil this dari global!
```

### Kode 4: setTimeout — this hilang
```javascript
const timer = {
    count: 0,
    start: function() {
        setInterval(function() {
            this.count++; // this = global/undefined!
            console.log(this.count); // NaN
        }, 1000);
    }
};
// Perbaikan: arrow function
const timer2 = {
    count: 0,
    start: function() {
        setInterval(() => {
            this.count++; // this dari start (lexical)
            console.log(this.count);
        }, 1000);
    }
};
```

### Kode 5: .call() dan .apply() mengganti this
```javascript
function introduce(greeting) {
    console.log(`${greeting}, saya ${this.name}`);
}
const person = { name: "Eko" };
introduce.call(person, "Halo");     // "Halo, saya Eko"
introduce.apply(person, ["Hai"]);  // "Hai, saya Eko"
const bound = introduce.bind(person, "Hey");
bound();                           // "Hey, saya Eko"
```

---

## 4. Analogi Rumah

| Konsep | Analogi Rumah |
|--------|---------------|
| `this` | Kata **"saya"** — artinya tergantung siapa yang bicara |
| Method | "Saya" diucapkan **penghuni rumah** → artinya penghuni itu |
| Fungsi biasa | "Saya" diucapkan **tanpa konteks** → bingung siapa |
| Arrow function | "Saya" diucapkan **anak kecil** → selalu merujuk ke orang tua yang bicara duluan |
| `.call()` / `.bind()` | Meminjam **seragam orang lain** lalu bicara "saya" — semua orang kira Anda orang itu |
| Constructor (`new`) | **Arsitek** membuat rumah baru, "saya" artinya rumah baru itu |
| Event handler | "Saya" artinya **elemen HTML** yang diklik |

**Narasi:** Kata "saya" di rumah punya arti berbeda tergantung konteks. "Saya" dari ayah berarti ayah, "saya" dari ibu berarti ibu. Tapi jika ada orang asing masuk dan bilang "saya", kita bingung. Regular function seperti orang asing — "saya"-nya tidak jelas. Arrow function seperti anak kecil — "saya"-nya selalu merujuk ke orang yang menggendongnya (lexical scope). `.bind()` seperti meminjam seragam — Anda bicara "saya" tapi dilihat sebagai pemilik seragam.

---

## 5. Use Case

| Use Case | Cara |
|----------|------|
| Event listener DOM | `this` = elemen yang dipicu |
| Method chaining | Return `this` dari method |
| Constructor | `this` = instance baru |
| Reusable utility | `.call()` dengan objek berbeda |
| Class methods (OOP) | `this` = instance class |
| React class component | Bind method atau pakai arrow |

---

## 6. Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|-----------|--------|--------|
| Lupa bind callback | `setTimeout(obj.method, 100)` | `this` jadi global |
| Arrow function sebagai method | `obj.method = () => {...}` | `this` bukan objek |
| Callback tanpa bind | `arr.map(obj.method)` | `this` salah |
| Nested function | `function(){ function(){ this } }` | `this` dalam hilang |
| Strict mode lupa | Fungsi standalone di strict → `this` = undefined | Error akses properti |

---

## 7. Benang Merah

Materi 35 (closure: fungsi ingat scope) → **Materi 36 (this: konteks eksekusi)** → Materi 37 (first-class function: fungsi sebagai nilai)

Closure dan `this` adalah dua konsep yang sering membingungkan. Closure berkaitan dengan **scope variable**, `this` berkaitan dengan **konteks objek**.

---

## 8. Soal

### Soal 1 (Mudah)
Apa outputnya?
```javascript
const obj = {
    name: "Test",
    fn: function() {
        console.log(this.name);
    }
};
obj.fn();
```

<details>
<summary>Jawaban</summary>
"Test". Dipanggil sebagai method objek, `this` = objek itu sendiri.
</details>

### Soal 2 (Sedang)
Apa outputnya?
```javascript
const obj = {
    name: "Outer",
    inner: {
        name: "Inner",
        greet: function() {
            console.log(this.name);
        }
    }
};
obj.inner.greet();
```

<details>
<summary>Jawaban</summary>
"Inner". `this` merujuk pada objek **sebelah kiri titik saat pemanggilan**, yaitu `obj.inner`.
</details>

### Soal 3 (Sulit)
Jelaskan output kode berikut:
```javascript
const handler = {
    id: "handler1",
    init: function() {
        ["click", "keyup"].forEach(function(event) {
            console.log(this.id, event);
        });
    }
};
handler.init();
```
Bagaimana cara memperbaikinya agar mencetak "handler1 click" dan "handler1 keyup"?

<details>
<summary>Jawaban</summary>
Output: `undefined click`, `undefined keyup`. Karena callback `forEach` adalah fungsi biasa, `this` di dalamnya bukan `handler` (global/strict undefined).
Perbaikan:
1. Tambah argumen kedua `forEach(this)` — `forEach(fn, thisArg)`
2. Arrow function: `forEach((event) => {...})`
3. Simpan `const self = this` sebelum forEach
</details>
