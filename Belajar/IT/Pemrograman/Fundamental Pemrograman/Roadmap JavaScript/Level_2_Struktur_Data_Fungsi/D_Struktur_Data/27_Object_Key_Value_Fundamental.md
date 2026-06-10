# 27. Object — Key-Value Fundamental JS

**Benang Merah**: Di Materi 26 kita memproses string — satu nilai. Sekarang kita belajar **Object**: wadah untuk menyimpan BANYAK nilai dengan nama (key), bukan nomor (index).

---

## Penjelasan

Object adalah kumpulan **pasangan key-value**. Key adalah string (nama properti), value bisa apa saja — angka, string, boolean, array, bahkan object lain.

```javascript
// Array: akses via index angka
let arr = ["Budi", 25, "Jakarta"];
console.log(arr[0]); // "Budi" — index 0

// Object: akses via key nama
let obj = { nama: "Budi", umur: 25, kota: "Jakarta" };
console.log(obj.nama);  // "Budi" — key "nama"
console.log(obj["umur"]); // 25 — bracket notation
```

**Notasi**:
- **Dot notation**: `obj.properti` — lebih ringkas, untuk key valid
- **Bracket notation**: `obj["properti"]` — untuk key dengan spasi/dinamis
- **Optional chaining** (`?.`): aman jika properti mungkin `undefined`

---

## Fungsi

Mengelompokkan data yang **berhubungan** dalam satu entitas terstruktur — seperti profil pengguna, produk, transaksi, konfigurasi.

---

## Code — Sistem Profil Pengguna

```javascript
// OBJECT LITERAL
const profil = {
  nama: "Budi Hartono",
  umur: 30,
  alamat: {
    jalan: "Jl. Merdeka No. 10",
    kota: "Jakarta",
    kodePos: "12345",
  },
  kontak: {
    email: "budi@rumah.com",
    telepon: "08123456789",
  },
  hobi: ["Coding", "Baca Buku", "Bersepeda"],
};

// DOT NOTATION
console.log(profil.nama);            // "Budi Hartono"
console.log(profil.alamat.kota);     // "Jakarta" — nested

// BRACKET NOTATION (untuk key dinamis)
const key = "umur";
console.log(profil[key]);            // 30

// OPTIONAL CHAINING — aman jika properti tidak ada
console.log(profil.alamat?.provinsi);   // undefined (tidak error)
console.log(profil.pekerjaan?.nama);    // undefined (tidak error)

// OBJECT METHODS
console.log(Object.keys(profil));
// ["nama", "umur", "alamat", "kontak", "hobi"]

console.log(Object.values(profil));
// ["Budi Hartono", 30, {...}, {...}, [...]]

console.log(Object.entries(profil));
// [["nama", "Budi Hartono"], ["umur", 30], ...]

// DESTRUCTURING (intro — detail di Materi 29)
const { nama, umur } = profil;
console.log(nama, umur); // "Budi Hartono" 30

// SPREAD OPERATOR — salin object
const profilLengkap = { ...profil, status: "Aktif" };
console.log(profilLengkap.status); // "Aktif"
```

---

## Analogi: Membangun Rumah (Lemari Arsip)

| Konsep Object | Lemari Arsip |
|---|---|
| Object `{}` | Lemari arsip |
| Key: `"nama"` | Label laci: "Nama" |
| Value: `"Budi"` | Isi laci: dokumen Budi |
| `obj.properti` | Buka laci "Nama" |
| `obj["properti"]` | Buka laci berdasarkan label di secarik kertas |
| Nested object | Laci berisi sub-laci |
| Optional chaining `?.` | Cek dulu apakah laci ada sebelum membuka |
| Spread `...` | Fotokopi seluruh isi lemari ke lemari baru |

Bayangkan **lemari arsip** di kantor kontraktor. Setiap laci punya **label** (key) dan **isi** (value). Ada laci utama (profil) dan di dalamnya ada sub-laci (alamat, kontak). Untuk mengambil data, Anda buka laci berdasarkan label — bukan nomor urut seperti array.

---

## Dipakai Untuk Apa

- **Profil / data pengguna** — nama, email, preferensi, role
- **Konfigurasi aplikasi** — pengaturan tema, bahasa, timeout
- **Data terstruktur** — produk (nama, harga, stok), transaksi
- **Parameter fungsi** — kirim banyak argumen dalam satu object
- **Response API** — JSON (JavaScript Object Notation) adalah object

---

## Kesalahan Umum

| Kesalahan | Contoh | Penjelasan |
|---|---|---|
| Dot notation dengan spasi | `obj.nama lengkap` | Error — pakai bracket `obj["nama lengkap"]` |
| Key angka | `obj.1` | Error — pakai `obj[1]` |
| Salah paham `const` object | `const a = {}; a.x = 1` | **Valid** — isi bisa diubah, referensi tetap |
| Optional chaining lupa | `obj.alamat.kota.rt` | Error jika `.alamat` undefined — pakai `?.` |
| Object.keys di array | `Object.keys(arr)` | Bekerja — tapi return index (string), bukan isi |

---

## Hubungan dengan Materi Sebelumnya

- **Materi 26 (String)**: String bisa jadi key atau value di object. Validasi form (Materi 26) menghasilkan data yang disimpan di object.
- **Materi 8 (Variabel)**: Dulu kita simpan satu nilai per variabel — sekarang satu object bisa simpan banyak nilai.
- **Materi 28 (Array of Objects)**: Object + Array = pola data paling umum di dunia nyata.

---

## Soal Latihan

### Soal 1 (Mudah)
Buat object `buku` dengan properti: judul, penulis, tahunTerbit, dan harga. Cetak judul dan penulis menggunakan dot notation.

**Jawaban**:
```javascript
const buku = {
  judul: "Belajar JavaScript",
  penulis: "Budi Hartono",
  tahunTerbit: 2024,
  harga: 150000,
};
console.log(`${buku.judul} oleh ${buku.penulis}`);
// "Belajar JavaScript oleh Budi Hartono"
```

### Soal 2 (Sedang)
Dari object profil di atas, cetak semua nama key menggunakan `Object.keys()`, lalu semua nilai alamat (jalan, kota, kodePos) menggunakan `Object.values()`.

**Jawaban**:
```javascript
const profil = {
  nama: "Budi",
  alamat: { jalan: "Jl. A", kota: "Jakarta", kodePos: "12345" },
};
console.log(Object.keys(profil));  // ["nama", "alamat"]
console.log(Object.values(profil.alamat));
// ["Jl. A", "Jakarta", "12345"]
```

### Soal 3 (Tantangan)
Buat fungsi `profilRingkas(profil)` yang menerima object profil (bisa tidak lengkap) dan mengembalikan string ringkas. Gunakan **optional chaining** dan **default value** (`||`).

**Jawaban**:
```javascript
function profilRingkas(profil) {
  const nama = profil?.nama || "Anonim";
  const kota = profil?.alamat?.kota || "Tidak diketahui";
  const hobiPertama = profil?.hobi?.[0] || "Tidak ada hobi";
  return `${nama} dari ${kota}, hobi: ${hobiPertama}`;
}

console.log(profilRingkas({ nama: "Budi", alamat: { kota: "Jakarta" }, hobi: ["Coding"] }));
// "Budi dari Jakarta, hobi: Coding"

console.log(profilRingkas({}));
// "Anonim dari Tidak diketahui, hobi: Tidak ada hobi"
```
