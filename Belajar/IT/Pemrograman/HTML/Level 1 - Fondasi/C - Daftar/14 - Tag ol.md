# Tag `<ol>` (Ordered List)

## Penjelasan

`<ol>` adalah tag untuk membuat daftar yang **urutan itemnya penting**. Setiap item ditulis dengan `<li>`. Secara default browser menampilkan angka (1, 2, 3). Bisa diubah tipe penomorannya dengan atribut `type`.

## Fungsi

Menyajikan langkah-langkah berurutan, instruksi bertahap, atau peringkat yang harus diikuti sesuai urutan.

## Cara Pengimplementasian

```html
<ol>
  <li>Gali pondasi</li>
  <li>Cor beton</li>
  <li>Pasang bata</li>
</ol>
```

Dengan atribut `type` dan `start`:

```html
<ol type="A" start="3">
  <li>Pasang keramik</li>
  <li>Cat dinding</li>
</ol>
```

Nilai `type`: `1` (angka), `A` (kapital), `a` (kecil), `I` (romawi besar), `i` (romawi kecil).

## Analogi (tema RUMAH/BANGUNAN)

Seperti **tahap pembangunan rumah** — Anda harus menggali pondasi dulu, baru memasang bata, lalu atap. Tidak bisa dibolak-balik.

## Dipakai Untuk

- Instruksi/langkah kerja
- Resep masakan
- Peringkat lomba
- Daftar isi

## Kesalahan Umum

- Menggunakan `<ol>` untuk daftar yang urutannya tidak penting (gunakan `<ul>`).
- Lupa atribut `start` padahal ingin memulai dari angka tertentu.
- Tidak menutup tag `<li>`.

## Koneksi dengan Materi Sebelumnya

Berbeda dengan `<ul>` yang tidak mementingkan urutan, `<ol>` hadir saat urutan bersifat krusial. Struktur itemnya sama-sama menggunakan `<li>`.

## Soal Latihan

Buatlah daftar berurutan 3 langkah membangun rumah menggunakan `<ol>`.

<details><summary>Jawaban</summary>

```html
<ol>
  <li>Membuat pondasi</li>
  <li>Mendirikan dinding</li>
  <li>Memasang atap</li>
</ol>
```

</details>
