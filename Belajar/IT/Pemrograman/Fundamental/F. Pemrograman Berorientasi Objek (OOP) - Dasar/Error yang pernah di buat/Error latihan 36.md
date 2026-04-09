

```
//2.

class KarakterGame {

    constructor(namaPlayer) {

        this.nama = namaPlayer;

        this.darah = 100;

    }

  

    serang() {

        this.darah - 10; (disini letak error nya harusnya)

    }

  

    cekStatus() {

        console.log(`sisa darah ${this.darah}`);

    }

}

  

let Player1 = new KarakterGame("agamoto");

  

Player1.serang();

  

Player1.cekStatus();
```


Ini nih contoh klasik: **kode keliatan jalan, tapi sebenernya gak ngapa-ngapain**. Tragis.

Masalah lo di sini:

```js
this.darah - 10;
```

Lo cuma **ngurangin doang di angan-angan**, gak disimpan.  
JavaScript bukan cenayang yang otomatis update nilai.

---

### Fix yang bener:

```js
this.darah -= 10;
```

atau kalau mau gaya dikit:

```js
this.darah = this.darah - 10;
```

---

### Versi yang udah waras:

```js
class KarakterGame {
    constructor(namaPlayer) {
        this.nama = namaPlayer;
        this.darah = 100;
    }

    serang() {
        this.darah -= 10;
    }

    cekStatus() {
        console.log(`sisa darah ${this.darah}`);
    }
}

let Player1 = new KarakterGame("agamoto");

Player1.serang();
Player1.cekStatus();
```

---

### Kenapa tadi gagal?

Karena ini:

```js
this.darah - 10;
```

Itu cuma:

- hitung doang ✔
    
- buang hasilnya ✔
    
- gak disimpan ❌
    

Jadi darahnya tetap 100, kayak gak pernah diserang. Plot armor level dewa.

---

### Biar makin gak ngaco lagi

Kalau operasi matematika:

- mau ubah nilai → **harus assign (=)**
    
- kalau gak → ya cuma ilusi
    

---

Udah gitu doang, tapi sering banget kejadian.  
Bug kecil, efeknya bikin lo bengong 30 menit. Classic rookie move.