# Tag `<label>` dan `<input>`

## Penjelasan

Tag `<label>` berfungsi sebagai keterangan atau label untuk elemen `<input>`. Hubungan keduanya dibuat melalui atribut `for` pada label yang mencocokkan atribut `id` pada input. Saat label diklik, fokus akan otomatis pindah ke input yang terhubung.

## Fungsi

- `<label>` — memberi teks penjelas pada input dan memperluas area klik.
- `<input>` — elemen interaktif tempat pengguna memasukkan data.
- `for` (label) + `id` (input) — pasangan kunci yang menghubungkan label dan input.

## Cara Pengimplementasian

```html
<!-- Cara 1: atribut for + id -->
<label for="email">Alamat Email:</label>
<input type="email" id="email" name="email">

<!-- Cara 2: bungkus input di dalam label -->
<label>
  Nama Lengkap:
  <input type="text" name="nama">
</label>

<!-- Klik teks "Setuju" akan mencentang checkbox -->
<label for="setuju">
  <input type="checkbox" id="setuju" name="setuju">
  Saya setuju dengan syarat dan ketentuan
</label>
```

## Analogi (tema RUMAH/BANGUNAN)

Bayangkan `<label>` adalah **papan nama** di samping setiap pintu kamar di sebuah rumah besar. Tanpa papan nama, Anda harus membuka setiap pintu untuk tahu isinya. Dengan papan nama (label), Anda langsung tahu kamar mana yang Anda tuju. Ketika Anda menyentuh papan nama itu (klik label), pintu kamar otomatis terbuka (input mendapat fokus). Hubungan `for` dan `id` seperti **nomor kamar** yang sama di papan nama dan di pintu.

## Dipakai Untuk

- Mempermudah pengguna mengklik input — area klik lebih besar (terutama untuk checkbox dan radio).
- Meningkatkan aksesibilitas untuk pengguna screen reader.
- Menghubungkan teks keterangan dengan input secara eksplisit.

## Kesalahan Umum

- Menggunakan `for` di label tapi lupa memberi `id` yang sama di input.
- Membiarkan input tanpa label sama sekali.
- Meletakkan lebih dari satu input di dalam satu label — membingungkan.
- Menggunakan `placeholder` sebagai pengganti label (placeholder hilang saat mengetik).

## Koneksi dengan Materi Sebelumnya

Di materi sebelumnya kita belajar tag `<form>` sebagai wadah. `<label>` dan `<input>` adalah isi dari wadah tersebut — seperti perabot di dalam rumah.

## Soal Latihan

1. Apa fungsi atribut `for` pada tag `<label>`?
2. Sebutkan dua cara menghubungkan `<label>` dengan `<input>`.
3. Mengapa `<label>` penting untuk checkbox?

<details><summary>Jawaban</summary>
1. `for` mencocokkan `id` pada `<input>` agar saat label diklik, input mendapat fokus.<br>
2. (a) Gunakan `for` di label dan `id` di input. (b) Bungkus `<input>` di dalam tag `<label>`.<br>
3. Karena area klik menjadi lebih besar — pengguna tidak harus mengklik kotak kecil, cukup klik teks labelnya.
</details>
