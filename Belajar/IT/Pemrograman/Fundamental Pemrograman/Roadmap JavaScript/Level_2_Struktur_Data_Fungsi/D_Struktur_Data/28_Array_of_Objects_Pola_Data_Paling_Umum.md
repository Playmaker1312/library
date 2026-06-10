# 28. Array of Objects — Pola Data Paling Umum

**Benang Merah**: Di Materi 27 kita punya object (profil pengguna). Di Materi 25 kita punya array methods. Sekarang kita **gabungkan** — array yang setiap elemennya adalah object. Ini pola yang paling sering Anda lihat di dunia nyata.

---

## Penjelasan

**Array of Objects** adalah representasi "database" sederhana. Setiap object = satu record (baris data), array = tabel (kumpulan record).

```javascript
// Array of Objects = tabel database
const pengguna = [
  { id: 1, nama: "Budi",  role: "admin" },
  { id: 2, nama: "Siti",  role: "user"  },
  { id: 3, nama: "Agus",  role: "user"  },
];
```

**CRUD** pada array of objects adalah skill paling fundamental untuk backend, frontend, dan data processing.

---

## Fungsi

Menyimpan, mengelola, dan memproses **data terstruktur** dalam jumlah banyak — seperti tabel di database, spreadsheet, atau response API.

---

## Code — Todo List In-Memory

```javascript
let todos = [
  { id: 1, tugas: "Belajar Array", selesai: false },
  { id: 2, tugas: "Beli Bata",     selesai: true  },
  { id: 3, tugas: "Pasang Atap",   selesai: false },
];

// === CREATE ===
const tambahTodo = (tugas) => {
  const idBaru = todos.length > 0 ? Math.max(...todos.map(t => t.id)) + 1 : 1;
  todos.push({ id: idBaru, tugas, selesai: false });
};
tambahTodo("Cat Tembok");
console.log(todos);

// === READ ===
const cariById = (id) => todos.find(t => t.id === id);
console.log(cariById(2)); // { id: 2, tugas: "Beli Bata", selesai: true }

const cariByStatus = (selesai) => todos.filter(t => t.selesai === selesai);
console.log(cariByStatus(true)); // todo yang sudah selesai

// === UPDATE ===
const toggleSelesai = (id) => {
  const todo = todos.find(t => t.id === id);
  if (todo) todo.selesai = !todo.selesai;
};
toggleSelesai(1);
console.log(todos);

// === DELETE ===
const hapusTodo = (id) => {
  todos = todos.filter(t => t.id !== id);
};
hapusTodo(3);
console.log(todos);

// === SORT ===
const urutBerdasarkanTugas = todos.sort((a, b) =>
  a.tugas.localeCompare(b.tugas)
);
console.log(urutBerdasarkanTugas);

// === KOMBINASI MAP + FILTER + REDUCE ===
// Hitung jumlah todo yang belum selesai
const sisaTugas = todos.filter(t => !t.selesai).length;

// Daftar tugas saja (map)
const daftarTugas = todos.map(t => `${t.id}. ${t.tugas} [${t.selesai ? "✓" : " "}]`);
console.log(daftarTugas.join("\n"));
```

---

## Analogi: Membangun Rumah (Rak Arsip)

| Konsep | Rak Arsip di Kontraktor |
|---|---|
| Array `[]` | Rak arsip |
| Object `{}` | Satu map/arsip |
| `{ id, tugas, selesai }` | Satu lembar data di dalam map |
| `.find()` | Cari satu arsip berdasarkan nomor |
| `.filter()` | Ambil semua arsip dengan kategori tertentu |
| `.sort()` | Urutkan arsip berdasarkan abjad |
| `.map()` | Buat daftar isi semua arsip |
| `.reduce()` | Hitung total halaman semua arsip |

Bayangkan **rak arsip proyek rumah**. Setiap map (object) berisi data satu tugas. Rak (array) menampung banyak map. Anda bisa:
- **Tambah** map baru ke rak
- **Cari** map berdasarkan nomor ID
- **Saring** map yang sudah selesai vs belum
- **Urutkan** map berdasarkan nama tugas
- **Hitung** total tugas yang tersisa

---

## Dipakai Untuk Apa

- **Todo list, task manager** — daftar tugas dengan status
- **E-commerce** — daftar produk, keranjang belanja
- **Sosial media** — daftar post, komentar, pengguna
- **Dashboard** — data tabel, laporan, analytics
- **API response** — hampir semua API mengembalikan array of objects

---

## Kesalahan Umum

| Kesalahan | Contoh | Penjelasan |
|---|---|---|
| Mutasi langsung di `.map()` | `arr.map(x => { x.harga *= 2; return x; })` | Map untuk transformasi — jangan ubah asli |
| Filter tanpa simpan | `arr.filter(x => x.id !== id)` lalu lihat `arr` | Filter tidak mengubah asli — simpan hasilnya |
| Sort mutation | `arr.sort()` mengubah array asli | Sort **bekerja in-place** — salin dulu dengan `[...arr].sort()` |
| Referensi object | `let a = todos[0]; a.nama = "X"` | Mengubah todo asli! Itu referensi, bukan salinan |
| Math.max tanpa spread | `Math.max(todos.map(t => t.id))` | `map` return array → `Math.max` butuh `...` spread |

---

## Hubungan dengan Materi Sebelumnya

- **Materi 27 (Object)**: Object adalah "sel" data. Array of Objects = kumpulan sel.
- **Materi 25 (Array methods)**: `map/filter/reduce/find` jadi senjata utama untuk memproses array of objects.
- **Materi 24 (Array dasar)**: CRUD manual diperluas menjadi CRUD pada data terstruktur.
- **Materi 29 (Destructuring)**: Cara lebih bersih mengakses properti dari setiap object.

---

## Soal Latihan

### Soal 1 (Mudah)
Dari array `[{nama:"Budi", nilai:80}, {nama:"Siti", nilai:90}, {nama:"Agus", nilai:70}]`, cetak nama siswa yang nilainya >= 80.

**Jawaban**:
```javascript
const siswa = [
  { nama: "Budi", nilai: 80 },
  { nama: "Siti", nilai: 90 },
  { nama: "Agus", nilai: 70 },
];
const lulus = siswa.filter(s => s.nilai >= 80).map(s => s.nama);
console.log(lulus); // ["Budi", "Siti"]
```

### Soal 2 (Sedang)
Dari todos di atas, gunakan `reduce()` untuk membuat laporan: `{ total: 4, selesai: 1, belum: 3 }`.

**Jawaban**:
```javascript
const todos = [
  { id: 1, tugas: "Belajar Array", selesai: false },
  { id: 2, tugas: "Beli Bata",     selesai: true  },
  { id: 3, tugas: "Pasang Atap",   selesai: false },
  { id: 4, tugas: "Cat Tembok",    selesai: false },
];

const laporan = todos.reduce(
  (acc, t) => {
    acc.total++;
    t.selesai ? acc.selesai++ : acc.belum++;
    return acc;
  },
  { total: 0, selesai: 0, belum: 0 }
);
console.log(laporan); // { total: 4, selesai: 1, belum: 3 }
```

### Soal 3 (Tantangan)
Buat fungsi `urutkanBerdasarkan(daftar, properti, ascending)` yang mengurutkan array of objects berdasarkan properti tertentu. Gunakan `[...arr]` agar tidak mengubah asli.

**Jawaban**:
```javascript
function urutkanBerdasarkan(daftar, properti, ascending = true) {
  const salinan = [...daftar];
  return salinan.sort((a, b) => {
    if (a[properti] < b[properti]) return ascending ? -1 : 1;
    if (a[properti] > b[properti]) return ascending ? 1 : -1;
    return 0;
  });
}

const data = [
  { nama: "Budi", umur: 30 },
  { nama: "Siti", umur: 25 },
  { nama: "Agus", umur: 35 },
];

console.log(urutkanBerdasarkan(data, "umur"));
// Siti (25), Budi (30), Agus (35)

console.log(urutkanBerdasarkan(data, "umur", false));
// Agus (35), Budi (30), Siti (25)
```
