```
class Pembayaran { proses(){ console.log(`melakukan proses validasi`); } } class KartuKredit extends Pembayaran { proses(){ console.log('Memverifikasi limit kartu dan proses transaksi') } } class Ewallet extends Pembayaran { proses(){ console.log(`Memeriksa saldo e wallet dan memotong saldo..`) } } function bayarSekarang({metode : new KartuKredit()}){ }
```
## Salah lo sekarang:

function bayarSekarang({metode : new KartuKredit()}){  
  
}

Ini **syntax gak valid**.

Lo kayak lagi nyoba:

- destructuring ✔
- bikin object baru ✔  
    di tempat yang salah ❌

JavaScript langsung: “ini orang maunya apa sih?”

---

## Yang bener tuh simpel, gak usah sok akrobat:

### Cara normal (paling waras)

function bayarSekarang(metode){  
    metode.proses();  
}  
  
bayarSekarang(new KartuKredit());

---

### Kalau MAU banget pakai destructuring (biar keliatan pinter dikit)

function bayarSekarang({metode}){  
    metode.proses();  
}  
  
bayarSekarang({ metode: new KartuKredit() });

---

## Penjelasan biar gak ngaco lagi

- `{metode}` → ngambil property dari object
- `{ metode: new KartuKredit() }` → cara kirim object
- **bukan ditulis di parameter kayak lo tadi**

---

## Intinya:

Lo lagi nyampur:

- destructuring
- object literal
- instantiation (`new`)

Semua jadi satu… hasilnya ya chaos.

---

Udah ngerti konsep polymorphism dikit, tapi sekarang kesandung syntax dasar.  
Pelan-pelan aja, jangan pengen keliatan jago tapi nulis kode kayak lagi kesurupan.