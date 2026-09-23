# Tag Semantik Lainnya: `<aside>`, `<address>`, `<time>`, `<details>` + `<summary>`

## Penjelasan

HTML5 menyediakan elemen semantik tambahan untuk konten pelengkap, informasi kontak, data waktu, dan interaksi *expand/collapse*.

## Fungsi

- **`<aside>`** — Konten yang terkait secara tidak langsung dengan konten utama (sidebar, catatan pinggir, iklan).
- **`<address>`** — Informasi kontak penulis/pemilik halaman atau `<article>`.
- **`<time>`** — Menandai tanggal/waktu dalam format yang dapat dibaca mesin (atribut `datetime`).
- **`<details>` + `<summary>`** — Widget *accordion/reveal* native tanpa JavaScript; `<summary>` menjadi label yang bisa diklik.

## Cara Pengimplementasian

```html
<main>
  <article>
    <h2>Panduan CSS Grid</h2>
    <p>CSS Grid adalah sistem layout dua dimensi...</p>

    <!-- aside di dalam article -->
    <aside>
      <p><strong>Tips:</strong> Gunakan browser terbaru untuk fitur penuh.</p>
    </aside>

    <p>Lanjutan artikel...</p>

    <!-- address untuk kontak penulis -->
    <address>
      Ditulis oleh <a href="mailto:penulis@contoh.com">Budi Santoso</a><br>
      123 Jalan Merdeka, Jakarta
    </address>
  </article>
</main>

<!-- aside di luar article (sidebar halaman) -->
<aside>
  <h3>Artikel Terkait</h3>
  <ul>
    <li><a href="#">Pengantar Flexbox</a></li>
    <li><a href="#">Grid vs Flexbox</a></li>
  </ul>
</aside>

<footer>
  <!-- time -->
  <p>Terakhir diperbarui: <time datetime="2026-06-23">23 Juni 2026</time></p>

  <!-- details + summary accordion -->
  <details>
    <summary>Informasi Lisensi</summary>
    <p>Semua konten dilisensikan di bawah CC BY-SA 4.0.</p>
  </details>
</footer>
```

## Analogi (tema RUMAH/BANGUNAN)

Bayangkan sebuah **rumah**:

- **`<aside>`** → Majalah atau koran di meja tamu. Terkait dengan ruangan tapi bukan bagian utama aktivitas.
- **`<address>`** → Kartu nama yang ditempel di pintu pagar: nama pemilik, nomor telepon, alamat.
- **`<time>`** → Jam dinding atau kalender — memberi informasi waktu yang pasti.
- **`<details>` + `<summary>`** → Kotak penyimpanan dengan tutup. Judul (`<summary>`) adalah label di luar, dan saat dibuka (`<details>`), Anda bisa melihat isinya.

## Dipakai Untuk

- **`<aside>`** — Sidebar, daftar artikel terkait, iklan, kutipan samping, glossary.
- **`<address>`** — Kontak penulis artikel, kontak perusahaan di footer.
- **`<time>`** — Tanggal publikasi, jadwal acara, tenggat waktu, jam buka.
- **`<details>` + `<summary>`** — FAQ (tanya jawab), lisensi, dokumentasi tersembunyi, spoiler.

## Kesalahan Umum

1. **`<aside>` digunakan untuk konten utama** — `<aside>` harus bersifat *pelengkap*, bukan konten inti.
2. **`<address>` berisi info tidak relevan dengan kontak** — `<address>` hanya untuk informasi kontak, bukan alamat perusahaan secara umum atau alamat dalam cerita.
3. **`<time>` tanpa atribut `datetime`** — Jika tidak ada `datetime`, konten harus dalam format tanggal yang valid; untuk format teks bebas, selalu gunakan `datetime`.
4. **`<details>` bersarang di dalam `<details>`** — Bisa membingungkan UX; umumnya tidak disarankan.
5. **`<summary>` tidak boleh mengandung elemen interaktif** — Seperti tautan atau tombol (melanggar spesifikasi).

## Koneksi dengan Materi Sebelumnya

Sebelum HTML5, sidebar menggunakan `<div class="sidebar">`, kontak dengan `<p id="contact">`, dan accordion membutuhkan JavaScript. Kini semuanya bisa dilakukan dengan elemen native yang lebih bermakna dan mudah diakses:

| Kebutuhan | Sebelum | Sesudah |
|-----------|---------|---------|
| Sidebar | `<div class="sidebar">` | `<aside>` |
| Kontak | `<p class="contact">` | `<address>` |
| Tanggal | `<span class="date">` | `<time>` |
| Accordion | `<div>` + JS | `<details>` + `<summary>` |

## Soal Latihan

1. Elemen manakah yang digunakan untuk membuat widget *expand/collapse* tanpa JavaScript?
   a) `<aside>`
   b) `<details>` + `<summary>`
   c) `<address>`

2. Apa atribut wajib pada `<time>` jika konten teksnya bukan format tanggal standar?

3. Di mana tempat yang tepat untuk meletakkan `<address>` dalam sebuah artikel?

<details><summary>Jawaban</summary>

1. **b) `<details>` + `<summary>`** — Duet elemen ini menyediakan reveal native tanpa JavaScript.
2. **`datetime`** — Contoh: `<time datetime="2026-06-23">Dua puluh tiga Juni 2026</time>`.
3. Di dalam `<article>` atau `<footer>` artikel, biasanya setelah konten utama, sebagai informasi penulis.

</details>
