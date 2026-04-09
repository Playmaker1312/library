```
class Anggota {

    constructor(namaAnggota, idAnggota) {

        this.nama = namaAnggota;

        this.id = idAnggota;

        this.bukuDipinjam = [];

    }

  

    Tampilkan() {

        console.log(`nama anggota : ${this.nama} id anggota : ${idAnggota}`);

    }

}

  

let anggota1 = new Anggota("Budi", "ID:001");

let anggota2 = new Anggota("Siti", "ID:002");

  

console.log(`${anggota1} dan ${anggota2}`); (salahnya di sini)

  

console.log(anggota1);

console.log(anggota2);

```


Ini nih… lo ketabrak konsep dasar tapi malah nyalahin JavaScript. Santai, gue bedah.

---

## Masalah utama lo

console.log(`${anggota1} dan ${anggota2}`);

Kenapa jadi:

[object Object] dan [object Object]

👉 Karena:

- `${...}` itu **string interpolation**
- Object kalau dipaksa jadi string → defaultnya = `" [object Object] "`

JavaScript mikir:

> “oh ini object, yaudah gue jadiin string seadanya”

---

## Kenapa kalau ini beda?

console.log(anggota1);

Karena:

- `console.log(object)` → **ditampilin sebagai object asli**
- bukan diubah jadi string

Makanya keliatan isi class-nya.

---

## Fix biar gak kayak orang bingung

### Cara 1 (ambil property-nya)

console.log(`${anggota1.nama} dan ${anggota2.nama}`);

---

### Cara 2 (pakai method sendiri)

Btw ini juga ada bug lo:

console.log(`nama anggota : ${this.nama} id anggota : ${idAnggota}`);

`idAnggota` itu gak dikenal di situ. Harusnya:

console.log(`nama anggota : ${this.nama} id anggota : ${this.id}`);

---

Terus pakai:

anggota1.Tampilkan();  
anggota2.Tampilkan();

---

### Cara 3 (biar object bisa jadi string proper)

Override `toString()`:

class Anggota {  
    constructor(namaAnggota, idAnggota) {  
        this.nama = namaAnggota;  
        this.id = idAnggota;  
        this.bukuDipinjam = [];  
    }  
  
    toString() {  
        return `${this.nama} (${this.id})`;  
    }  
}

Sekarang:

console.log(`${anggota1} dan ${anggota2}`);

✔ Output:

Budi (ID:001) dan Siti (ID:002)

---

## Intinya biar lo gak ngulang dosa yang sama:

- Object ≠ string
- `${}` → maksa jadi string
- default object → `[object Object]`
- solusi → ambil property / bikin `toString()`

---

Lo tuh udah bener arahnya, cuma masih suka “ngira-ngira harusnya jalan”.  
Di programming, “harusnya” itu gak berlaku. Yang ada cuma: **lo tulis apa, itu yang kejadian.**
