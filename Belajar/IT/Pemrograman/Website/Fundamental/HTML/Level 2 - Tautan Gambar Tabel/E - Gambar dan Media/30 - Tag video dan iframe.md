# Tag `<video>` dan `<iframe>`

## Penjelasan

Tag `<video>` digunakan untuk menyematkan video mandiri dari file lokal atau URL langsung. Tag `<iframe>` digunakan untuk menyematkan halaman web eksternal di dalam halaman saat ini, termasuk video dari platform seperti YouTube.

## Fungsi

- `<video>`: Memutar file video (MP4, WebM, Ogg) langsung di halaman HTML.
- `<iframe>`: Menyematkan konten dari situs lain (YouTube, Google Maps, halaman lain).

## Cara Pengimplementasian

```html
<!-- Video mandiri dengan kontrol -->
<video controls width="640" height="360">
  <source src="tur-rumah.mp4" type="video/mp4">
  <source src="tur-rumah.webm" type="video/webm">
  Browser Anda tidak mendukung video.
</video>

<!-- Video dengan autoplay muted loop (seperti background video) -->
<video autoplay muted loop>
  <source src="suasana-rumah.mp4" type="video/mp4">
</video>

<!-- Iframe YouTube embed -->
<iframe 
  width="560" 
  height="315" 
  src="https://www.youtube.com/embed/VIDEO_ID" 
  title="Video tutorial membangun rumah"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen>
</iframe>
```

## Analogi (tema RUMAH/BANGUNAN)

- **`<video>`** seperti **TV atau proyektor di ruang keluarga** — kamu memasang file video langsung di perangkatmu.
- **`<iframe>`** seperti **jendela yang menghadap ke rumah tetangga** — kamu bisa melihat aktivitas di luar (konten YouTube), tanpa harus keluar rumah. Kamu tidak bisa mengubah apa yang terjadi di seberang, hanya bisa melihatnya melalui bingkai jendela.

## Dipakai Untuk

- **`<video>`**: 
  - Video profil perusahaan
  - Tutorial atau demo produk
  - Video latar (background video) halaman
  - Video pembelajaran offline

- **`<iframe>`**:
  - Menyematkan video YouTube/Vimeo
  - Google Maps embed
  - Halaman dokumen atau PDF viewer

## Atribut Penting `<video>`

| Atribut      | Deskripsi                                     |
|--------------|-----------------------------------------------|
| `controls`   | Menampilkan kontrol play, pause, volume       |
| `autoplay`   | Putar otomatis (harus dengan `muted`)         |
| `muted`      | Membisukan video                              |
| `loop`       | Memutar ulang terus-menerus                   |
| `poster`     | Gambar thumbnail sebelum video diputar        |
| `width/height` | Ukuran tampilan pemutar video              |

## Kesalahan Umum

- **Autoplay tanpa muted** pada video akan diblokir browser.
- **Tidak menyediakan multiple source** (MP4 + WebM) — beberapa browser mungkin tidak bisa memutar.
- Menggunakan `<iframe>` untuk menyematkan video tanpa izin (bisa kena blokir CORS).
- Lupa menambahkan `allowfullscreen` pada `<iframe>` YouTube.
- Tidak menentukan `title` pada `<iframe>` — melanggar aksesibilitas.

## Koneksi dengan Materi Sebelumnya

- **Tag `<img>`**: Baik gambar maupun video bisa dibungkus `<figure>` + `<figcaption>`.
- **Tag `<audio>`**: Struktur `<video>` hampir identik dengan `<audio>` — sama-sama punya `controls`, `source`, `autoplay`, `muted`, `loop`.
- **Atribut `src`**: Digunakan baik di `<img>`, `<audio>`, `<source>`, dan `<iframe>` untuk lokasi konten.

## Soal Latihan

1. Tulis tag `<video>` untuk memutar `tutorial.mp4` dengan lebar 720 piksel, kontrol, dan poster gambar `thumbnail.jpg`.
2. Apa perbedaan utama antara `<video>` dan `<iframe>` dalam konteks menampilkan video?
3. Tulis kode untuk menyematkan video YouTube dengan ID `abc123xyz`.

<details><summary>Jawaban</summary>

1. 
```html
<video controls width="720" poster="thumbnail.jpg">
  <source src="tutorial.mp4" type="video/mp4">
</video>
```

2. `<video>` memuat file video langsung dari server kita sendiri, sedangkan `<iframe>` menyematkan konten dari situs eksternal (seperti YouTube) tanpa harus menyimpan file video di server kita. `<iframe>` pada dasarnya memuat halaman web lain di dalam halaman saat ini.

3. 
```html
<iframe 
  width="560" 
  height="315" 
  src="https://www.youtube.com/embed/abc123xyz" 
  title="Video YouTube" 
  allowfullscreen>
</iframe>
```

</details>
