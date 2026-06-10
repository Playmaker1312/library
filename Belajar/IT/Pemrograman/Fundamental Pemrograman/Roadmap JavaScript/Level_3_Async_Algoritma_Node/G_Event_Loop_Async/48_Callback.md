# 48. Callback — Pola Async Pertama

**Benang Merah**: Materi 46-47 menjelaskan Event Loop memungkinkan async. Callback adalah **pola pertama** untuk menangani async — fungsi yang dipanggil setelah operasi selesai. Dari sini lahir Callback Hell, yang mendorong Promise (Materi 49).

---

## Penjelasan

**Callback** adalah fungsi yang dikirim sebagai **argumen** ke fungsi lain, untuk dipanggil **nanti** setelah operasi async selesai.

```javascript
function orderMaterial(jenis, callback) {
  console.log(`Order ${jenis} sedang diproses...`);
  setTimeout(() => {
    console.log(`${jenis} siap!`);
    callback(jenis);
  }, 2000);
}

orderMaterial("bata", (material) => {
  console.log(`Terima ${material} — lanjut bangun`);
});
// Output:
// Order bata sedang diproses...
// (2 detik)
// bata siap!
// Terima bata — lanjut bangun
```

### Error-First Callback (Node.js style)

```javascript
function bacaFile(path, callback) {
  // error-first: callback(error, data)
  if (!path) {
    callback(new Error("Path tidak boleh kosong"), null);
    return;
  }
  const data = `Isi file ${path}`;
  callback(null, data); // error = null, data sukses
}

bacaFile("denah.txt", (err, data) => {
  if (err) {
    console.error("Gagal:", err.message);
    return;
  }
  console.log("Data:", data);
});
```

---

## Fungsi

Callback memungkinkan kita **menunda eksekusi** sampai operasi async selesai, tanpa memblokir Call Stack. Ini pola dasar semua async di JS: event listener, timer, request, dan baca file.

---

## Cara Pengimplementasian

### 1. Callback Sederhana

```javascript
function pesanMaterial(jenis, jumlah, cb) {
  console.log(`Memesan ${jumlah} ${jenis}...`);
  setTimeout(() => {
    const totalHarga = jumlah * 5000;
    cb(null, { jenis, jumlah, totalHarga });
  }, 1000);
}

pesanMaterial("bata", 200, (err, hasil) => {
  if (err) return console.error(err);
  console.log(`Pesanan: ${hasil.jumlah} ${hasil.jenis} = Rp${hasil.totalHarga}`);
});
```

### 2. Callback Hell — Masalah Utama

```javascript
// Bayangkan proses bangun rumah bertahap:
function galiPondasi(cb) {
  setTimeout(() => { console.log("1. Pondasi jadi"); cb(); }, 1000);
}
function pasangBata(cb) {
  setTimeout(() => { console.log("2. Bata terpasang"); cb(); }, 1000);
}
function pasangAtap(cb) {
  setTimeout(() => { console.log("3. Atap terpasang"); cb(); }, 1000);
}
function finishing(cb) {
  setTimeout(() => { console.log("4. Finishing selesai"); cb(); }, 1000);
}

// CALLBACK HELL — bersarang tak terkendali
galiPondasi(() => {
  pasangBata(() => {
    pasangAtap(() => {
      finishing(() => {
        console.log("Rumah selesai! Tapi lihat kodenya...");
      });
    });
  });
});
// Ini yang disebut "Pyramid of Doom" — sulit dibaca, debug, dan maintain
```

### 3. Async Ops dengan Callback Lebih Realistis

```javascript
function cekKetersediaanMaterial(item, cb) {
  console.log(`Cek ${item} di gudang...`);
  setTimeout(() => {
    const tersedia = Math.random() > 0.3;
    cb(null, { item, tersedia });
  }, 500);
}

function pesanMaterial(item, cb) {
  console.log(`Pesan ${item} dari supplier...`);
  setTimeout(() => {
    cb(null, `${item} dikirim`);
  }, 1000);
}

// Cascade callback
cekKetersediaanMaterial("semen 50kg", (err, hasil) => {
  if (err) return console.error(err);
  if (!hasil.tersedia) {
    console.log(`${hasil.item} tidak tersedia, pesan dulu`);
    pesanMaterial(hasil.item, (err, status) => {
      if (err) return console.error(err);
      console.log(status);
    });
  } else {
    console.log(`${hasil.item} tersedia, ambil dari gudang`);
  }
});
```

---

## Analogi Membangun Rumah — Telepon Balik

| Callback | Proyek Rumah |
|---|---|
| Callback function | **Telepon balik** — Anda minta nomor, tunggu dihubungi |
| Error-first callback | Telepon "Ada kendala" (error) atau "Material datang" (data) |
| Callback Hell | **Telepon berantai** — A telepon B, B telepon C, C telepon D |
| setTimeout (simulasi delay) | Waktu kirim material dari supplier |
| Main function | Anda yang memulai proses |

Bayangkan Anda seorang mandor proyek. Anda **telepon supplier** pesan bata: "Tolong telepon saya kalau barang sudah siap" (callback). Anda tidak diam — lanjut kerja lain. Supplier telepon balik: "Bata sudah siap" (callback dipanggil). 

**Callback Hell**: Supplier bilang "Tunggu, saya transfer ke bagian pengiriman". Anda tunggu telepon dari bagian pengiriman. Mereka bilang "Tunggu, saya transfer ke bagian pembayaran". Setiap langkah butuh telepon baru — prosedur berbelit dan menyebalkan.

---

## Dipakai Untuk Apa

- **setTimeout/setInterval** — fungsi callback setelah delay
- **Event listener** — `element.addEventListener("click", callback)`
- **Baca file di Node.js** — `fs.readFile(path, callback)`
- **Request HTTP** — `request(url, callback)` (legacy)
- **Middleware Express.js** — `app.use((req, res, next) => {...})`

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Callback Hell | Bersarang 5+ level | Kode sulit dibaca, debug jadi mimpi buruk |
| Lupa handle error | Tidak cek `err` di error-first callback | Error diam-diam, aplikasi rusak |
| Callback dipanggil 2x | `if(err) cb(err); cb(null, data);` | Fungsi jalan 2x, hasil ganda |
| Trust issue | Tidak tahu kapan callback dipanggil | Logika berantakan |
| Inversion of Control | Callback dikirim ke library pihak ketiga | Kita kehilangan kendali eksekusi |

---

## Hubungan dengan Materi Sebelumnya

- Materi 46 (JS Runtime) → Callback masuk Call Stack saat Event Loop memindahkannya
- Materi 47 (Event Loop) → Callback antri di Callback Queue (macrotask)
- Materi 49 (Promise) → **Solusi** dari Callback Hell
- Materi 50 (async/await) → Cara lebih bersih menulis async code

---

## Soal Latihan

### Soal 1 (Mudah)
Apa output kode berikut?
```javascript
function proses(cb) {
  console.log("Mulai proses");
  setTimeout(() => {
    cb("Data siap");
  }, 1000);
  console.log("Proses berjalan...");
}
proses((data) => console.log("Callback:", data));
```

**Jawaban**:
```
Mulai proses
Proses berjalan...
(1 detik)
Callback: Data siap
```
`console.log` sync jalan duluan. Callback async setelah 1 detik.

### Soal 2 (Sedang)
Refactor callback hell menjadi fungsi terpisah agar lebih rapi:
```javascript
step1(() => {
  step2(() => {
    step3(() => {
      console.log("Selesai");
    });
  });
});
```

**Jawaban**:
```javascript
function runStep1() {
  step1(runStep2);
}
function runStep2() {
  step2(runStep3);
}
function runStep3() {
  step3(() => console.log("Selesai"));
}
runStep1();
// Atau gunakan Promise (Materi 49)
```

### Soal 3 (Tantangan)
Buat fungsi `kirimMaterial(item, callback)` yang:
- Jika `item` falsy → panggil callback dengan error `"Item tidak valid"`
- Jika `item` truthy → tunggu 500ms, lalu callback null dengan `"Mengirim ${item}"`
- Lalu panggil fungsi untuk kirim "bata" dan "semen" **bersamaan** (parallel)

**Jawaban**:
```javascript
function kirimMaterial(item, callback) {
  if (!item) {
    callback(new Error("Item tidak valid"), null);
    return;
  }
  setTimeout(() => {
    callback(null, `Mengirim ${item}`);
  }, 500);
}

// Parallel call — keduanya jalan bersama
kirimMaterial("bata", (err, res) => {
  if (err) return console.error(err.message);
  console.log(res);
});

kirimMaterial("semen", (err, res) => {
  if (err) return console.error(err.message);
  console.log(res);
});

console.log("Kedua pengiriman diproses...");
// Output:
// Kedua pengiriman diproses...
// (500ms)
// Mengirim bata
// Mengirim semen
```
