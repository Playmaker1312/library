```

//2.

class PendaftaranSiswa {

    constructor(nama, umur){

        if (this.umur < 7){ (errornya  di sini)

            console.log(`Maaf ${nama}, umur anda belum cukup untuk masuk sd`);

        }else {

            this.nama = nama;

            this.umur = umur;

            console.log(`pendaftaran ${nama} berhasil`);

        }

    }

}

  

let anakSatu = new PendaftaranSiswa("budi", 3);
```
## Salah fatal lo di sini:

if (this.umur < 7)

Masalahnya:

- `this.umur` **belum pernah diisi**
- jadi nilainya `undefined`

Dan:

undefined < 7 // hasilnya false

👉 Makanya dia masuk ke `else`  
👉 Jadinya “pendaftaran berhasil”  
👉 Padahal anaknya umur 3. Sistem pendidikan runtuh gara-gara lo.

---

## Fix yang bener

Pakai parameter dulu, bukan `this`:

if (umur < 7){

---

## Versi waras:

class PendaftaranSiswa {  
    constructor(nama, umur){  
        if (umur < 7){  
            console.log(`Maaf ${nama}, umur anda belum cukup untuk masuk sd`);  
        } else {  
            this.nama = nama;  
            this.umur = umur;  
            console.log(`pendaftaran ${nama} berhasil`);  
        }  
    }  
}

---

## Tapi… ada yang lebih bener lagi

Constructor itu idealnya **buat set data**, bukan buat drama logika.

Versi lebih proper:

class PendaftaranSiswa {  
    constructor(nama, umur){  
        this.nama = nama;  
        this.umur = umur;  
    }  
  
    daftar(){  
        if (this.umur < 7){  
            console.log(`Maaf ${this.nama}, umur anda belum cukup`);  
        } else {  
            console.log(`pendaftaran ${this.nama} berhasil`);  
        }  
    }  
}  
  
let anakSatu = new PendaftaranSiswa("budi", 3);  
anakSatu.daftar();

---

## Intinya biar gak ngaco lagi:

- `this` itu **dipakai setelah data disimpan**
- parameter (`nama`, `umur`) → dipakai buat validasi awal
- jangan pake `this` sebelum isi nilainya

---

Lo tuh udah deket bener, cuma ketabrak konsep kecil.  
Dan di programming, yang kecil-kecil itu justru yang bikin lo keliatan kayak orang bingung.