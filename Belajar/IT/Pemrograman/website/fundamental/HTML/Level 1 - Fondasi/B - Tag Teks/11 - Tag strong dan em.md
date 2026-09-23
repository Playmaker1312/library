# Tag `<strong>` dan `<em>`

## Penjelasan
`<strong>` dan `<em>` adalah elemen **semantik** yang memberi penekanan pada teks. `<strong>` menandakan teks itu **penting** (urgent/serius). `<em>` menandakan teks mendapat **penekanan** (emphasis/stress). Secara visual, `<strong>` tampil **tebal** dan `<em>` tampil *miring*, mirip seperti `<b>` (bold) dan `<i>` (italic) — namun bedanya `<b>` dan `<i>` hanya **gaya visual**, tanpa makna.

## Fungsi
- `<strong>`: Menandai teks yang sangat penting, serius, atau darurat
- `<em>`: Menandai teks yang perlu ditekankan saat dibaca (intonasi berbeda)
- `<b>`: Teks tebal untuk keperluan gaya (tanpa makna semantik)
- `<i>`: Teks miring untuk istilah asing, judul karya, nama ilmiah (tanpa penekanan)

## Cara Pengimplementasian
```html
<p>
  <strong>Peringatan:</strong> Jangan menyentuh kabel listrik basah!
</p>

<p>
  Saya <em>sangat</em> menyarankan kamu untuk belajar HTML.
</p>

<p>
  <b>Catatan:</b> Bagian ini hanya untuk referensi cepat.
  Buku <i>Harry Potter</i> sangat populer di seluruh dunia.
</p>
```

## Analogi
Seperti **tanda peringatan di rumah**.
- **`<strong>`** adalah **rambu "BAHAYA"** besar dengan warna merah — kamu harus berhenti dan perhatikan. Tidak bisa diabaikan.
- **`<em>`** adalah **tanda peringatan biasa** — "Awas, lantai licin" — perlu diperhatikan tapi tidak darurat.
- **`<b>`** hanya **spidol tebal** yang kamu pakai untuk menandai kalimat — tidak memberi peringatan apa-apa.
- **`<i>`** seperti **tulisan miring di undangan** — terlihat formal/berbeda, tapi tidak menyampaikan urgensi.

## Dipakai Untuk
- `<strong>`: Pesan error, peringatan keamanan, kata kunci penting, instruksi kritis
- `<em>`: Kata yang perlu ditekankan dalam kalimat, sarkasme, intonasi khusus
- `<b>`: Kata kunci dalam ringkasan, angka dalam laporan (hanya gaya)
- `<i>`: Judul buku/film, istilah asing (*italic*), nama spesies, kata dalam bahasa lain

## Kesalahan Umum
- Menggunakan `<b>` dan `<i>` saat seharusnya `<strong>` dan `<em>` (hilang makna semantik)
- Menggunakan `<strong>` dan `<em>` hanya untuk mendapatkan tampilan tebal/miring (pakai CSS `font-weight` / `font-style`)
- Menumpuk `<strong>` dan `<em>` secara berlebihan sehingga tidak ada makna yang menonjol
- Menganggap `<b>` sama dengan `<strong>` — padahal pembaca layar membacakan `<strong>` dengan intonasi berbeda

## Koneksi dengan Materi Sebelumnya
Paragraf (`<p>`) memberikan wadah untuk teks. Sekarang kamu bisa memberi **makna** pada bagian tertentu di dalam paragraf: mana yang penting (`<strong>`) dan mana yang perlu penekanan (`<em>`).

## Soal Latihan
1. Apa perbedaan semantik antara `<strong>` dan `<b>`?
2. Tag mana yang digunakan untuk menekankan intonasi saat membaca?
3. Tulis kalimat peringatan "Awas, jangan buka pintu itu!" menggunakan tag yang tepat.
4. Kapan kita sebaiknya menggunakan `<i>` daripada `<em>`?
5. Bagaimana pembaca layar memperlakukan `<strong>` dibanding `<b>`?

<details><summary>Jawaban</summary>
1. `<strong>` punya makna **penting/serius** secara semantik, sementara `<b>` hanya efek visual tebal tanpa makna.<br>
2. `<em>` (emphasis).<br>
3. `<strong>Awas, jangan buka pintu itu!</strong>`.<br>
4. Gunakan `<i>` untuk istilah asing, judul karya, atau nama ilmiah — yaitu teks yang umumnya ditulis miring secara konvensi, bukan karena penekanan.<br>
5. Pembaca layar akan mengubah intonasi suara saat membaca `<strong>` (lebih keras/tegas), sedangkan `<b>` dibaca biasa saja tanpa penekanan.
</details>
