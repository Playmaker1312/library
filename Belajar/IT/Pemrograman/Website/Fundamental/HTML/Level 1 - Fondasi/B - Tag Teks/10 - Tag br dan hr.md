# Tag `<br>` dan `<hr>`

## Penjelasan
`<br>` (line break) dan `<hr>` (horizontal rule) adalah elemen HTML yang bersifat **self-closing** (tidak punya tag penutup). `<br>` membuat baris baru di dalam teks. `<hr>` menggambar garis horizontal yang menandakan perpindahan topik.

## Fungsi
- `<br>`: Memberi jeda baris di dalam paragraf atau teks (seperti Enter di dokumen biasa)
- `<hr>`: Memisahkan dua topik berbeda secara visual dengan garis horizontal

## Cara Pengimplementasian
```html
<p>
  Jl. Merdeka No. 10<br>
  Jakarta Pusat<br>
  Indonesia
</p>

<hr>

<p>
  Ini paragraf setelah garis pemisah. Topiknya sudah berbeda.
</p>

<p>
  Puisi pendek:<br>
  Kucingku lucu sekali<br>
  Tidur di sofa setiap hari<br>
  Bulunya lembut dan bersih
</p>
```

## Analogi
- **`<br>`** seperti **pintu dalam satu ruangan** — kamu pindah dari satu titik ke titik lain tanpa keluar ruangan. Masih dalam ruang yang sama (paragraf yang sama), tapi posisimu berubah.
- **`<hr>`** seperti **sekat pemisah antar ruangan** — menandakan bahwa area sebelumnya sudah selesai dan area baru dimulai. Bahkan secara visual ada garis pembatas (dinding tipis).

## Dipakai Untuk
- `<br>`: Alamat, puisi, lirik lagu, baris kode, format teks yang memang harus pindah baris
- `<hr>`: Pemisah antar bab dalam artikel, transisi topik, pemisah footer/konten

## Kesalahan Umum
- Menggunakan `<br>` untuk memberi jarak antar paragraf — gunakan `<p>` saja
- Menggunakan `<br>` untuk mengatur tata letak/spasi vertikal — itu tugas CSS (`margin`, `padding`)
- Menggunakan `<hr>` hanya sebagai hiasan tanpa makna semantik
- Lupa bahwa `<br>` dan `<hr>` adalah self-closing — jangan ditulis `<br></br>`

## Koneksi dengan Materi Sebelumnya
Setelah belajar heading (`<h1>`-`<h6>`) sebagai judul dan paragraf (`<p>`) sebagai isi, kini kamu tahu cara mengontrol **jeda** di dalam konten: `<br>` untuk pindah baris dalam satu paragraf, `<hr>` untuk pemisah topik besar.

## Soal Latihan
1. Apa perbedaan utama antara `<br>` dan `<hr>`?
2. Apakah `<br>` punya tag penutup?
3. Tulis alamat berikut menggunakan `<br>`: "Jl. Sudirman No. 99, Jakarta"
4. Kapan waktu yang tepat menggunakan `<hr>`?
5. Benarkah `<br>` bisa digunakan untuk mengatur jarak vertikal antar elemen? Jelaskan!

<details><summary>Jawaban</summary>
1. `<br>` membuat baris baru **di dalam konten yang sama**. `<hr>` membuat garis pemisah horizontal yang menandakan **pergantian topik**.<br>
2. Tidak — `<br>` adalah self-closing tag.<br>
3. 
```html
<p>
  Jl. Sudirman No. 99<br>
  Jakarta
</p>
```<br>
4. Saat ingin memisahkan dua bagian konten yang topiknya berbeda secara signifikan, misal antara artikel utama dan bagian komentar.<br>
5. **Salah** — `<br>` hanya untuk pindah baris dalam teks. Untuk jarak vertikal, gunakan CSS seperti `margin` atau `padding`.
</details>
