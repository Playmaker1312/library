# 47. Event Loop — Jantung Asynchronous JavaScript

**Benang Merah**: Sebelumnya (Materi 46) kita belajar bahwa JavaScript **single-threaded** — hanya bisa melakukan satu hal dalam satu waktu. Lalu bagaimana JS menangani operasi yang butuh waktu lama (baca file, request API)? Jawabannya: **Event Loop**.

---

## Penjelasan

Event Loop adalah **mekanisme** yang memungkinkan JavaScript melakukan operasi non-blocking meskipun single-threaded. Ibarat **resepsionis di gedung perkantoran** yang bisa menerima banyak tamu tanpa harus berhenti melayani satu tamu sampai selesai.

Cara kerja Event Loop:

```
┌──────────────┐     ┌─────────────────┐
│  Call Stack  │ ◄── │  Event Loop     │
│  (eksekusi   │     │  (memindahkan   │
│   sekarang)  │     │   task)         │
└──────────────┘     └───────┬─────────┘
                             │
                    ┌────────▼────────┐
                    │  Callback Queue │
                    │  (antrian task) │
                    └─────────────────┘
```

```javascript
console.log("1. Awal");          // Synchronous — langsung ke Call Stack

setTimeout(() => {
  console.log("2. Timer selesai"); // Asynchronous — ke Callback Queue dulu
}, 0);

console.log("3. Akhir");         // Synchronous — langsung ke Call Stack

// Output:
// 1. Awal
// 3. Akhir
// 2. Timer selesai
```

---

## Fungsi

Memungkinkan JavaScript menangani **operasi I/O** (file, network, timer) tanpa memblokir eksekusi kode lain. Ini yang membuat Node.js bisa melayani ribuan request bersamaan meskipun single-threaded.

---

## Cara Pengimplementasian

### Urutan Eksekusi (Resepsionis)

```javascript
console.log("Mulai");  // 1. Langsung jalan

setTimeout(() => {
  console.log("Timer 0ms"); // 3. Jalan setelah Call Stack kosong
}, 0);

setTimeout(() => {
  console.log("Timer 100ms"); // 4. Jalan setelah 100ms
}, 100);

console.log("Selesai"); // 2. Langsung jalan

// OUTPUT:
// Mulai
// Selesai
// Timer 0ms
// Timer 100ms
```

### Microtask vs Macrotask
```javascript
console.log("1");

setTimeout(() => console.log("2"), 0);        // macrotask

Promise.resolve().then(() => console.log("3")); // microtask

console.log("4");

// OUTPUT:
// 1  → Call Stack langsung
// 4  → Call Stack langsung
// 3  → Microtask Queue (prioritas lebih tinggi)
// 2  → Macrotask Queue
```

> **Prioritas**: Call Stack > Microtask Queue (Promise) > Macrotask Queue (setTimeout)

---

## Analogi: Membangun Rumah (Resepsionis Gedung)

| Event Loop | Resepsionis Gedung |
|---|---|
| Call Stack | Meja resepsionis (hanya bisa layani 1 tamu sekali) |
| Operasi async (setTimeout) | Tamu yang butuh dokumen dari lantai atas |
| Callback Queue | Antrian tamu yang dokumennya sudah siap |
| Event Loop | Resepsionis yang ngecek antrian saat meja kosong |
| Promise (microtask) | Telepon prioritas dari direktur |

Bayangkan resepsionis yang **hanya bisa melayani satu orang dalam satu waktu**. Ada tamu datang langsung (synchronous) — dilayani sekarang. Ada tamu yang butuh dokumen diambil (setTimeout) — resepsionis mencatat, lanjut ke tamu lain. Saat dokumen siap, tamu itu masuk antrian. Setiap kali meja kosong, resepsionis cek: "Ada telepon prioritas? (Promise) Ada antrian? (callback)".

---

## Dipakai Untuk Apa

- **setTimeout/setInterval** — delay, timer, polling
- **fetch / AJAX** — mengambil data dari server tanpa nge-freeze halaman
- **Baca/tulis file** di Node.js
- **Event listener** — klik, scroll, input di browser
- **Semua operasi async** di JavaScript

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Anggap `setTimeout(fn, 0)` jalan instan | Jalan setelah semua sync selesai | Urutan eksekusi tidak sesuai dugaan |
| Lupa beda microtask & macrotask | Menaruh logika penting di setTimeout | Promise jalan duluan |
| Blocking Event Loop | Loop berat `while(true)` 5 detik | Semua antrian tertahan |
| Async tanpa await | `fetch(url)` hasilnya Promise, bukan data | Dapat Promise, bukan response |

---

## Hubungan dengan Materi Sebelumnya

Event Loop adalah **kunci** untuk memahami async JavaScript:
- Materi 31 (Stack) → Call Stack adalah implementasi Stack
- Materi 46 (JS Runtime) → V8 + Event Loop + Web APIs
- Materi 48 (Callback) → Callback masuk ke Callback Queue
- Materi 49 (Promise) → Promise.then masuk ke Microtask Queue
- Materi 50 (async/await) → Syntactic sugar di atas Promise

---

## Soal Latihan

### Soal 1 (Mudah)
Tebak output kode berikut, lalu verifikasi dengan menjalankannya:
```javascript
console.log("A");
setTimeout(() => console.log("B"), 100);
console.log("C");
setTimeout(() => console.log("D"), 0);
console.log("E");
```

**Jawaban**:
```
A
C
E
D
B
```
Penjelasan: A, C, E sync langsung. D dari setTimeout 0ms setelah microtask. B dari setTimeout 100ms paling akhir.

### Soal 2 (Sedang)
Tebak output:
```javascript
console.log(1);
Promise.resolve().then(() => console.log(2));
setTimeout(() => console.log(3), 0);
console.log(4);
```

**Jawaban**:
```
1
4
2
3
```
Penjelasan: Promise (.then) masuk Microtask Queue — prioritas di atas setTimeout (Macrotask Queue).

### Soal 3 (Tantangan)
Buat fungsi `delay(ms)` yang mengembalikan Promise dan selesai setelah `ms` milidetik. Gunakan untuk mencetak "Halo" setelah 2 detik.

**Jawaban**:
```javascript
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

// Cara pakai:
delay(2000).then(() => console.log("Halo setelah 2 detik"));

// Atau dengan async/await:
async function main() {
  console.log("Menunggu...");
  await delay(2000);
  console.log("Halo setelah 2 detik");
}
main();
```
