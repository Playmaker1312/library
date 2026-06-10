# 30. Map & Set — Koleksi Data Modern

**Benang Merah**: Di Materi 29 kita belajar spread/rest — cara modern manipulasi data. Sekarang kita kenalan dengan dua struktur data modern ES6: **Set** (koleksi unik) dan **Map** (key-value dengan key apapun).

---

## Penjelasan

**Set** adalah kumpulan nilai **unik** (tidak boleh duplikat). **Map** adalah kumpulan key-value di mana key bisa **berupa tipe data apa pun** (tidak hanya string seperti Object).

```javascript
// Set — otomatis hapus duplikat
const setAngka = new Set([1, 2, 2, 3, 3, 3]);
console.log(setAngka); // Set { 1, 2, 3 }

// Map — key bisa object, number, dll
const mapKontak = new Map();
mapKontak.set("Budi", "08123456789");
mapKontak.set(123, "Nomor ID");
```

**Perbedaan Map vs Object**:
| Object | Map |
|---|---|
| Key hanya string/symbol | Key: any type (object, number, function) |
| Tidak ada method `.size` (pakai `Object.keys().length`) | Punya `.size` properti |
| Tidak ada urutan | Urutan sesuai insertion |
| Iterasi manual | Punya `.forEach()`, `for...of` bawaan |

---

## Fungsi

**Set**: Menyimpan data **unik**, deduplikasi, cek keberadaan dengan cepat. **Map**: Menyimpan key-value dengan **key fleksibel**, caching, lookup cepat.

---

## Code — Deteksi Kata Duplikat dalam Teks

```javascript
// ===== SET — Deteksi kata unik & duplikat =====
function analisisTeks(teks) {
  const kata = teks.toLowerCase().split(/\s+/);
  const setUnik = new Set(kata);

  console.log("Total kata:", kata.length);
  console.log("Kata unik:", setUnik.size);
  console.log("Kata duplikat:", kata.length - setUnik.size);

  // Cek kata tertentu
  console.log("Ada 'bata'?", setUnik.has("bata"));   // true/false

  // Hapus dari Set
  setUnik.delete("yang");

  // Iterasi Set
  for (const k of setUnik) {
    console.log(">>", k);
  }

  // Konversi Set ke Array (pakai spread)
  const arrUnik = [...setUnik];
  console.log(arrUnik);
}

analisisTeks("bata semen bata cat paku semen bata");

// ===== MAP — Hitung frekuensi kata =====
function hitungFrekuensi(teks) {
  const kata = teks.toLowerCase().split(/\s+/);
  const frekuensi = new Map();

  for (const k of kata) {
    if (frekuensi.has(k)) {
      frekuensi.set(k, frekuensi.get(k) + 1);
    } else {
      frekuensi.set(k, 1);
    }
  }

  // Iterasi Map
  frekuensi.forEach((jumlah, kata) => {
    console.log(`${kata}: ${jumlah}x`);
  });

  // Ukuran Map
  console.log("Jenis kata:", frekuensi.size);

  // Konversi Map ke Array
  console.log([...frekuensi]); // [[kata, jumlah], ...]
  console.log(Array.from(frekuensi)); // sama

  // Map ke Object (jika key semua string)
  const obj = Object.fromEntries(frekuensi);
  console.log(obj);
}

hitungFrekuensi("bata semen bata cat paku semen bata");
// bata: 3x, semen: 2x, cat: 1x, paku: 1x
```

---

## Analogi: Membangun Rumah (Rak & Lemari)

| Struktur Data | Analogi Rumah |
|---|---|
| **Set** | **Rak tanpa stok ganda** — setiap barang hanya boleh satu. Jika ada 3 bata, hanya 1 yang masuk rak. |
| `set.add()` | Taruh barang ke rak (jika sudah ada, diabaikan) |
| `set.has()` | Cek apakah barang ada di rak |
| `set.size` | Hitung berapa jenis barang berbeda |
| `set.delete()` | Ambil barang dari rak |
| **Map** | **Lemari dengan kunci apa saja** — kuncinya bisa nomor, stiker, bahkan benda fisik. |
| `map.set(key, val)` | Masukkan barang dengan kunci unik |
| `map.get(key)` | Ambil barang berdasarkan kuncinya |
| `map.has(key)` | Cek apakah kunci ada |
| `map.size` | Hitung jumlah laci |

Bayangkan di gudang material:
- **Set** = rak khusus untuk "satu sampel setiap jenis". Anda tidak perlu dua sampel bata yang sama.
- **Map** = lemari tempat Anda bisa menyimpan **catatan stok** dengan key berupa ID barang (nomor) atau bahkan objek referensi.

---

## Dipakai Untuk Apa

**Set**:
- **Deduplikasi** — hapus duplikat dari array `[...new Set(arr)]`
- **Cek keanggotaan cepat** — izinkan role tertentu `set.has("admin")`
- **Tag / kategori unik** — kumpulkan semua tag tanpa duplikat
- **Tracking item yang sudah diproses** — hindari proses ulang

**Map**:
- **Caching** — simpan hasil kalkulasi dengan key argumen
- **Frekuensi counter** — hitung kemunculan setiap kata/elemen
- **Lookup table** — data lookup dengan key non-string
- **Metadata** — simpan data tambahan ke object tanpa mengubah object asli

---

## Kesalahan Umum

| Kesalahan | Contoh | Penjelasan |
|---|---|---|
| `set[0]` untuk akses | `const s = new Set([1,2,3]); s[0]` | Set tidak punya index — pakai `.has()` atau iterasi |
| Object key di Map pakai string | `map.set({}, "val")` lalu `map.get("{}")` | Key **object** berbeda dengan key string "{}" |
| Lupa konversi Set ke array | `[...new Set(arr)].map(...)` | Set tidak punya `.map()` — konversi dulu |
| Map vs Object untuk JSON | `JSON.stringify(map)` hasil `{}` | Map tidak otomatis serialize ke JSON |
| `NaN` di Set | `new Set([NaN, NaN])` | Hanya 1 NaN — Set anggap sama |

---

## Hubungan dengan Materi Sebelumnya

- **Materi 29 (Destructuring/Spread)**: Spread `[...set]` dan `[...map]` untuk konversi ke array. `Object.fromEntries(map)` untuk konversi Map ke Object.
- **Materi 27 (Object)**: Map adalah alternatif Object dengan key lebih fleksibel.
- **Materi 25 (Array methods)**: Set tidak punya map/filter — konversi ke array dulu.
- **Materi 31 (Stack & Queue)**: Struktur data juga bisa diimplementasikan dengan Map/Set.

---

## Soal Latihan

### Soal 1 (Mudah)
Dari array `[1, 2, 2, 3, 3, 3, 4, 5, 5]`, gunakan Set untuk menghapus duplikat dan hitung jumlah unik.

**Jawaban**:
```javascript
const arr = [1, 2, 2, 3, 3, 3, 4, 5, 5];
const unik = [...new Set(arr)];
console.log(unik);       // [1, 2, 3, 4, 5]
console.log(unik.length); // 5
```

### Soal 2 (Sedang)
Gunakan Map untuk membuat daftar kontak sederhana: tambahkan 3 kontak (nama → telepon), cari nomor "Budi", hapus "Siti", lalu cetak semua kontak.

**Jawaban**:
```javascript
const kontak = new Map();
kontak.set("Budi", "08123456789");
kontak.set("Siti", "08234567890");
kontak.set("Agus", "08345678901");

console.log("Telepon Budi:", kontak.get("Budi")); // 08123456789

kontak.delete("Siti");

kontak.forEach((telp, nama) => {
  console.log(`${nama}: ${telp}`);
});
// Budi: 08123456789
// Agus: 08345678901
```

### Soal 3 (Tantangan)
Dari teks `"satu dua tiga satu dua satu"`, gunakan **Map** untuk hitung frekuensi, lalu **Set** untuk ambil kata yang muncul lebih dari 1 kali (duplikat).

**Jawaban**:
```javascript
function cariDuplikat(teks) {
  const kata = teks.split(" ");
  const frek = new Map();

  for (const k of kata) {
    frek.set(k, (frek.get(k) || 0) + 1);
  }

  const duplikat = [...frek].filter(([k, v]) => v > 1).map(([k]) => k);
  return new Set(duplikat);
}

const hasil = cariDuplikat("satu dua tiga satu dua satu");
console.log(hasil); // Set { "satu", "dua" }
```
