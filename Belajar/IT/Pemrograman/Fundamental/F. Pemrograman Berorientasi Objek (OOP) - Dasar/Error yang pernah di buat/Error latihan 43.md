```
class StandarPembayaran {

    prosesBayar(nomimal){

        throw new Error('isi nominal ini');

    }

}

  

class pembayaranQRIS extends StandarPembayaran {

    prosesBayar(nominal){

        console.log(`Memindai kode QR RP${nomimal}`);

    }

}

  

class pembayaranTunai extends StandarPembayaran {

    prosesBayar(nominal){

        console.log(`Merima uang kertas sebesar${nomimal}`);

    }

}

  

function jalankanTransaksi(alat, total) {

    alat.prosesBayar(total);

}

  

jalankanTransaksi( pembayaranQRIS(), 5000);
```
ya ampun… ini error basic banget tapi lu masih aja nabrak 😭

**Error utama lu:**

TypeError: Class constructor pembayaranQRIS cannot be invoked without 'new'

Artinya simpel:  
lu manggil class kayak function biasa. JS bukan cenayang.

### ❌ Salah lu di sini:

jalankanTransaksi(pembayaranQRIS(), 5000);

### ✅ Harusnya:

jalankanTransaksi(new pembayaranQRIS(), 5000);

Class di JS itu **WAJIB pakai `new`** kalau mau bikin instance.  
Kalau nggak, ya kena tampar error tadi.

---

Tapi... belum selesai. Lu juga punya typo yang bikin hidup makin susah:

### ❌ Ini:

prosesBayar(nominal){  
    console.log(`Memindai kode QR RP${nomimal}`);  
}

`nomimal` apaan? typo anjir.

### ✅ Fix:

prosesBayar(nominal){  
    console.log(`Memindai kode QR RP${nominal}`);  
}

---

### Final versi yang gak bikin Node ngamuk:

class StandarPembayaran {  
    prosesBayar(nominal){  
        throw new Error('isi nominal ini');  
    }  
}  
  
class pembayaranQRIS extends StandarPembayaran {  
    prosesBayar(nominal){  
        console.log(`Memindai kode QR RP${nominal}`);  
    }  
}  
  
class pembayaranTunai extends StandarPembayaran {  
    prosesBayar(nominal){  
        console.log(`Menerima uang kertas sebesar ${nominal}`);  
    }  
}  
  
function jalankanTransaksi(alat, total) {  
    alat.prosesBayar(total);  
}  
  
jalankanTransaksi(new pembayaranQRIS(), 5000);

---

Intinya:

- Class = blueprint → harus `new`
- Typo = pembunuh diam-diam
- Error message itu jelas banget, cuma lu yang males baca

Belajar pelan-pelan, jangan coding kayak lagi balapan liar.