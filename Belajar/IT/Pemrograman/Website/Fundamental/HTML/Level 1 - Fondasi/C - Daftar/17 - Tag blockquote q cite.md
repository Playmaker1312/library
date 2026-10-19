# Tag `<blockquote>`, `<q>`, `<cite>`

## Penjelasan

- `<blockquote>`: kutipan panjang (tampil sebagai blok terindentasi).
- `<q>`: kutipan pendek dalam baris (inline), otomatis ditambahkan tanda petik.
- `<cite>`: judul sumber karya (buku, film, artikel) — biasanya miring.

## Fungsi

Menandai teks yang dikutip dari sumber lain, baik panjang maupun pendek, serta menyebutkan sumbernya.

## Cara Pengimplementasian

```html
<blockquote cite="https://arsitek.com/tips">
  Rumah yang baik bukanlah rumah yang mewah, melainkan rumah yang nyaman bagi penghuninya.
</blockquote>

Arsitek terkenal berkata, <q>Bangunan adalah seni yang kita huni.</q>

Sumber: <cite>Filosofi Arsitektur Modern</cite>
```

## Analogi (tema RUMAH/BANGUNAN)

Seperti **plakat peresmian gedung** — ada nama arsitek atau sumber inspirasi yang tertulis di dinding. `<blockquote>` adalah papan besar, `<q>` adalah tulisan kecil di bingkai foto, `<cite>` adalah nama penciptanya.

## Dipakai Untuk

- Testimoni atau ulasan
- Kutipan dari buku/tokoh
- Referensi sumber artikel
- Epigraf di awal bab

## Kesalahan Umum

- Menggunakan `<blockquote>` hanya untuk efek indentasi — seharusnya pakai CSS `margin-left`.
- Lupa atribut `cite` pada `<blockquote>` atau `<q>`.
- Menempatkan `<cite>` sebagai kutipan — `<cite>` untuk judul sumber, bukan nama orang.

## Koneksi dengan Materi Sebelumnya

Seperti heading dan paragraf, ketiga tag ini adalah elemen teks. Bedanya, mereka secara semantik menandai teks sebagai kutipan atau sumber.

## Soal Latihan

Buat `<blockquote>` tentang "Rumah adalah istana kita" dengan sumber `<cite>` "Pepatah Kuno".

<details><summary>Jawaban</summary>

```html
<blockquote>
  Rumah adalah istana kita.
</blockquote>
<cite>Pepatah Kuno</cite>
```

</details>
