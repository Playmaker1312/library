# Tag Teks Lainnya: `<mark>`, `<small>`, `<del>`, `<ins>`, `<sup>`, `<sub>`

## Penjelasan
HTML menyediakan tag-tag teks khusus untuk kebutuhan spesifik:
- `<mark>` — teks yang **disorot** (seperti stabilo)
- `<small>` — teks **kecil** untuk informasi sampingan
- `<del>` — teks yang **dihapus** (dicoret tengah)
- `<ins>` — teks yang **disisipkan** (garis bawah)
- `<sup>` — teks **superscript** (di atas garis normal)
- `<sub>` — teks **subscript** (di bawah garis normal)

## Fungsi
- `<mark>`: Menyorot bagian penting dalam konteks tertentu (misal hasil pencarian)
- `<small>`: Menampilkan teks dengan ukuran lebih kecil — untuk hak cipta, disclaimer, catatan kaki
- `<del>`: Menunjukkan teks yang sudah dihapus/diganti dalam dokumen revisi
- `<ins>`: Menunjukkan teks yang baru ditambahkan dalam dokumen revisi
- `<sup>`: Untuk pangkat, nomor urut (1^st^), catatan kaki
- `<sub>`: Untuk rumus kimia, indeks, notasi ilmiah

## Cara Pengimplementasian
```html
<p>
  Kata kunci pencarian: <mark>"HTML Dasar"</mark>
</p>

<p>
  <small>&copy; 2025 Belajar HTML. All rights reserved.</small>
</p>

<p>
  Harga: <del>Rp100.000</del> <ins>Rp75.000</ins>
</p>

<p>
  Rumus kimia air: H<sub>2</sub>O
  <br>
  Luas tanah: 100 m<sup>2</sup>
  <br>
  Tanggal: 20<sup>th</sup> April 2025
</p>
```

## Analogi
Seperti **berbagai alat tulis dan perlengkapan di rumah**:
- **`<mark>`** = **stabilo** — menyorot kata penting di buku
- **`<small>`** = **tulisan kecil di pojok dokumen** — "Isi di luar tanggung jawab"
- **`<del>`** = **coretan tipe-x** — mencoret kata yang salah
- **`<ins>`** = **tambahan tulisan dengan garis bawah** — menyisipkan kata baru
- **`<sup>`** = **angka di atas garis** seperti pangkat di papan tulis
- **`<sub>`** = **angka di bawah garis** seperti indeks dalam rumus kimia

## Dipakai Untuk
- `<mark>`: Hasil pencarian, kata kunci, kode yang disorot dalam dokumentasi
- `<small>`: Hak cipta, lisensi, syarat & ketentuan, catatan kaki
- `<del>`+`<ins>`: Harga diskon, log perubahan (changelog), revisi dokumen, TODO yang sudah selesai
- `<sup>`: Pangkat (x²), nomor urut (ke-1^st^), footnote reference
- `<sub>`: Rumus kimia (H₂O, CO₂), indeks matematika

## Kesalahan Umum
- Menggunakan `<small>` untuk mengecilkan teks tanpa alasan semantik — pakai CSS `font-size`
- Menggunakan `<del>` hanya untuk efek coret visual — pakai CSS `text-decoration: line-through`
- Lupa bahwa `<mark>` bukan hanya soal warna kuning — browser bisa mengubah tampilan sesuai tema
- Menumpuk `<sub>` di dalam `<sup>` atau sebaliknya secara tidak hati-hati
- Tidak menggunakan `<ins>` bersama `<del>` saat menunjukkan perubahan (revisi)

## Koneksi dengan Materi Sebelumnya
Setelah mengenal tag teks dasar (`<p>`, `<strong>`, `<em>`), tag-tag ini melengkapi "kotak alat" teks HTML-mu. Kamu sekarang bisa: menyorot (`<mark>`), mengecilkan (`<small>`), mencoret (`<del>`), menyisip (`<ins>`), naikkan (`<sup>`), dan turunkan (`<sub>`) teks sesuai kebutuhan.

## Soal Latihan
1. Tag apa yang digunakan untuk membuat efek stabilo pada teks?
2. Tulis rumus kimia karbon dioksida menggunakan tag yang tepat!
3. Apa perbedaan fungsi `<del>` dan `<ins>`?
4. Tulis kalimat yang menunjukkan harga diskon: harga asli Rp200.000, harga promo Rp150.000!
5. Kapan waktu yang tepat menggunakan `<small>`?

<details><summary>Jawaban</summary>
1. `<mark>`.<br>
2. CO<sub>2</sub> — kode: `CO<sub>2</sub>`.<br>
3. `<del>` menunjukkan teks yang **dihapus** (dicoret), `<ins>` menunjukkan teks yang **disisipkan** (garis bawah). Biasanya digunakan berpasangan untuk menunjukkan revisi.<br>
4. `Harga: <del>Rp200.000</del> <ins>Rp150.000</ins>`.<br>
5. Untuk informasi sampingan seperti hak cipta, disclaimer, catatan kaki, atau lisensi — yaitu teks yang kurang penting dibanding konten utama.
</details>
