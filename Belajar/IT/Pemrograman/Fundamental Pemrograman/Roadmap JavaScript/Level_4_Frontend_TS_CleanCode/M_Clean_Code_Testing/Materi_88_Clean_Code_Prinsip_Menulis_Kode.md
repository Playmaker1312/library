# Materi 88: Clean Code — Prinsip Menulis Kode yang Dibaca Manusia

## 1. Penjelasan
Clean code adalah kode yang mudah dibaca, dipahami, dan dimodifikasi oleh manusia (bukan hanya mesin). Prinsip utamanya: nama variabel/fungsi ekspresif, fungsi kecil dengan satu tanggung jawab, hindari magic numbers/strings, dan tulis komentar yang menjelaskan "mengapa" bukan "apa".

## 2. Fungsi
- Meningkatkan maintainability kode
- Memudahkan kolaborasi tim
- Mengurangi bug karena kode jelas
- Mempercepat proses review dan debugging

## 3. Code

```javascript
// --- KODE JELEK ---
function a(x, y) {
  let z = x * 12.5;
  let w = y * 12.5;
  if (z > 100) {
    z = z + 5;
  }
  return z + w;
}

// --- CLEAN CODE ---
const TAX_RATE = 0.125;
const MINIMUM_AMOUNT = 100;
const SHIPPING_FEE = 5;

function calculateTotalWithShipping(price, quantity) {
  const tax = price * TAX_RATE;
  const subtotal = price * quantity + tax;

  if (subtotal > MINIMUM_AMOUNT) {
    return subtotal + SHIPPING_FEE;
  }

  return subtotal;
}
```

## 4. Analogi Rumah

| Prinsip Clean Code | Analogi Rumah |
|-------------------|---------------|
| Nama ekspresif | Setiap ruangan diberi label jelas: "Kamar Tidur", "Dapur", "Toilet" |
| Fungsi kecil satu tugas | Alat di dapur: pisau hanya untuk memotong, wajan hanya untuk menggoreng |
| Hindari magic numbers | Jangan taruh "3 meter" tanpa label — tulis "lebar_pintu = 3 meter" |
| Komentar baik vs buruk | Baik: "Pipa ini sengaja dibiarkan terbuka untuk akses air hujan". Buruk: "Ini pipa" |
| Kode rapi | Rumah rapi — setiap barang di tempatnya, label jelas, mudah dicari |

## 5. Use Case
Seorang developer baru bergabung ke tim. Dengan clean code, ia bisa langsung memahami fungsi `calculateTotalWithShipping` tanpa bertanya. Sebaliknya, kode jelek `a(x,y)` membutuhkan waktu 30 menit untuk dianalisis.

## 6. Kesalahan Umum
- Menulis komentar yang menjelaskan "apa" (padahal kode sudah jelas): `// menambah 5` pada `z + 5`
- Menggunakan singkatan tidak jelas: `calcTTL()`, `getUsrInf()`
- Fungsi terlalu panjang (> 20 baris) melakukan banyak hal
- Menggunakan magic number: `12.5` tanpa konstanta

## 7. Benang Merah
Dari Materi 87 (TypeScript + Vue) → sekarang kode harus baik kualitasnya. Clean code menjadi fondasi untuk Materi 89 (SOLID Principles).

## 8. Soal

### Soal 1
Refactor kode berikut menjadi clean code:
```javascript
function p(r) {
  let d = r * 2;
  return d + 10;
}
```
**Jawaban:**
```javascript
const BASE_FEE = 10;
const MULTIPLIER = 2;
function calculateDeliveryFee(rate) {
  return rate * MULTIPLIER + BASE_FEE;
}
```

### Soal 2
Apa masalah dari komentar berikut?
```javascript
// Mengurangi 1 dari x
x = x - 1;
```
**Jawaban:** Komentar menjelaskan "apa" yang sudah jelas dari kode. Komentar yang baik menjelaskan "mengapa": `// Diskon 1 karena pelanggan loyal`.

### Soal 3
Sebutkan 3 ciri fungsi yang bersih (clean function).
**Jawaban:** 1) Nama fungsi jelas dengan kata kerja (`calculateTotal`, bukan `ct`). 2) Satu tanggung jawab. 3) Tidak mengubah parameter input (pure jika memungkinkan).
