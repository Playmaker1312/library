# Tag `<dl>`, `<dt>`, `<dd>` (Description List)

## Penjelasan

`<dl>` (description list) adalah daftar berbasis **pasangan istilah:definisi**. `<dt>` (description term) untuk istilah, `<dd>` (description details) untuk definisinya. Satu `<dt>` bisa memiliki banyak `<dd>`.

## Fungsi

Menyajikan glosarium, metadata, atau pasangan key-value di mana setiap istilah punya penjelasan.

## Cara Pengimplementasian

```html
<dl>
  <dt>Pondasi</dt>
  <dd>Bagian bawah bangunan yang menahan beban.</dd>
  <dt>Kolom</dt>
  <dd>Tiang vertikal penopang lantai dan atap.</dd>
  <dd>Juga disebut pilar.</dd>
</dl>
```

## Analogi (tema RUMAH/BANGUNAN)

Seperti **papan nama di setiap ruangan** — "Kamar Tidur" (istilah) diikuti deskripsi "ruangan untuk tidur dan istirahat" (definisi). Satu nama bisa punya beberapa keterangan.

## Dipakai Untuk

- Glosarium / kamus istilah
- Metadata (key-value pairs)
- FAQ (pertanyaan + jawaban)
- Spesifikasi produk

## Kesalahan Umum

- Menaruh `<dt>` tanpa `<dd>` atau sebaliknya.
- Menggunakan `<dl>` untuk daftar biasa — gunakan `<ul>` atau `<ol>` saja.
- Menulis lebih dari satu `<dt>` tanpa `<dd>` di antaranya jika tidak diperlukan.

## Koneksi dengan Materi Sebelumnya

Berbeda dengan `<ul>`/`<ol>` yang berbasis item tunggal (`<li>`), `<dl>` menggunakan pasangan istilah–definisi. Ini adalah jenis daftar ketiga di HTML.

## Soal Latihan

Buat description list dengan istilah "Atap" dan definisi "Penutup bagian atas bangunan".

<details><summary>Jawaban</summary>

```html
<dl>
  <dt>Atap</dt>
  <dd>Penutup bagian atas bangunan.</dd>
</dl>
```

</details>
