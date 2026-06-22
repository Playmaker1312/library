# `<article>`, `<section>`, `<div>` — Konten Mandiri, Tematik, dan Netral

## Penjelasan

Ketiga elemen ini membedakan tingkat **kemaknaan** konten:

- **`<article>`** — Konten yang **berdiri sendiri** dan dapat didistribusikan/digunakan secara independen.
- **`<section>`** — Konten **tematik** yang merupakan pengelompokan logis dari satu tema.
- **`<div>`** — Tidak memiliki makna semantik; digunakan semata-mata untuk pengelompokan gaya atau scripting.

## Fungsi

- **`<article>`** — Membungkus entitas independen: posting blog, berita, komentar, widget produk.
- **`<section>`** — Membagi halaman menjadi bagian-bagian bertema (misal: "Fitur", "Harga", "Testimoni").
- **`<div>`** — Wadah netral untuk keperluan CSS atau JavaScript.

## Cara Pengimplementasian

```html
<main>
  <!-- article: berdiri sendiri -->
  <article>
    <h2>Berita Terkini: Gempa di Lombok</h2>
    <p>Gempa berkekuatan 5.2 SR mengguncang Lombok pada Senin pagi...</p>
  </article>

  <!-- section: tematik -->
  <section>
    <h2>Fitur Produk</h2>
    <ul>
      <li>Ringan dan portabel</li>
      <li>Baterai tahan 12 jam</li>
    </ul>
  </section>

  <!-- div: tanpa makna, untuk styling -->
  <div class="highlight-box">
    <p>Promo akhir tahun!</p>
  </div>
</main>
```

## Analogi (tema RUMAH/BANGUNAN)

Bayangkan sebuah **kompleks perumahan**:

- **`<article>`** → Satu rumah utuh yang bisa dijual atau disewa secara independen. Isinya lengkap dan bisa berdiri sendiri tanpa rumah lain.
- **`<section>`** → Kamar di dalam rumah. Misalnya "Kamar Tidur" — area dengan fungsi spesifik dan tematik, tapi tidak bisa berdiri sendiri sebagai rumah.
- **`<div>`** → Catatan tempel atau sekat sementara yang bisa dipindah-pindah. Tidak punya fungsi struktural, hanya untuk estetika atau pembatas visual.

## Dipakai Untuk

- **`<article>`** — Posting blog, berita, forum post, komentar, kartu produk, widget cuaca.
- **`<section>`** — Bab dalam artikel, tab panel, bagian "Tentang Kami", bagian "Fitur".
- **`<div>`** — Wrapper CSS, container flex/grid, placeholder JavaScript.

## Kesalahan Umum

1. **Menggunakan `<section>` hanya untuk styling** — `<section>` memiliki makna tematik; jika hanya untuk gaya, gunakan `<div>`.
2. **Membuat `<article>` di dalam `<article>` tanpa alasan** — Boleh, asalkan setiap `<article>` tetap independen (misal: komentar di dalam posting blog).
3. **Mengganti `<article>` dengan `<div>` untuk konten independen** — Menghilangkan makna semantik dan merusak aksesibilitas.
4. **`<section>` tanpa judul/heading** — Sebaiknya setiap `<section>` memiliki `<h1>`–`<h6>` sebagai penanda tema.

## Koneksi dengan Materi Sebelumnya

Di Level 1–3, semua konten dikelompokkan dengan `<div>`. Sekarang kita bedakan:

| Elemen | Makna | Contoh Sebelumnya |
|--------|-------|-------------------|
| `<div>` | Tidak ada | `<div class="post">` |
| `<section>` | Tematik | `<div class="features">` |
| `<article>` | Independen | `<div class="blog-entry">` |

Dengan elemen semantik, *screen reader* bisa langsung melompat ke artikel atau bagian tertentu tanpa perlu memproses kelas CSS.

## Soal Latihan

1. Mana yang tepat untuk membungkus satu berita lengkap?
   a) `<div>`
   b) `<section>`
   c) `<article>`

2. Kapan kita tetap menggunakan `<div>` meskipun sudah ada `<article>` dan `<section>`?

3. Apakah boleh `<article>` berisi `<section>` di dalamnya? Jelaskan.

<details><summary>Jawaban</summary>

1. **c) `<article>`** — Berita adalah konten independen yang dapat didistribusikan sendiri.
2. Saat tidak ada makna semantik yang diperlukan — hanya untuk styling (`class`, `id`), layout CSS, atau hook JavaScript.
3. **Ya, boleh.** `<article>` bisa berisi beberapa `<section>` untuk membagi konten artikel menjadi sub-tema. Ini adalah nesting yang valid dan umum.

</details>
