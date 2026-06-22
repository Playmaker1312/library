# Tag `<audio>`

## Penjelasan

Tag `<audio>` digunakan untuk menyematkan file audio ke dalam halaman HTML. Tag ini bersifat *paired tag* (memiliki penutup `</audio>`). Atribut `controls` menampilkan kontrol pemutar seperti play, pause, volume, dan timeline.

## Fungsi

- Memutar file audio langsung di halaman web tanpa plugin eksternal.
- Mendukung beberapa *source* untuk kompatibilitas lintas browser.
- Mengontrol perilaku pemutaran: otomatis, hening, atau berulang.

## Cara Pengimplementasian

```html
<!-- Audio dasar dengan kontrol -->
<audio controls>
  <source src="musik.mp3" type="audio/mpeg">
  Browser Anda tidak mendukung tag audio.
</audio>

<!-- Multiple source untuk kompatibilitas -->
<audio controls>
  <source src="suara-latihan.ogg" type="audio/ogg">
  <source src="suara-latihan.mp3" type="audio/mpeg">
  Browser Anda tidak mendukung tag audio.
</audio>

<!-- Autoplay (diam) + Loop -->
<audio controls autoplay muted loop>
  <source src="backsound.mp3" type="audio/mpeg">
</audio>
```

## Atribut Penting

| Atribut     | Deskripsi                                     |
|-------------|-----------------------------------------------|
| `controls`  | Menampilkan kontrol pemutar (play, pause, dll) |
| `autoplay`  | Memutar audio otomatis saat halaman dimuat     |
| `muted`     | Membisukan audio (sering dipasangkan dg autoplay) |
| `loop`      | Mengulang audio terus-menerus                  |
| `preload`   | Mengatur perilaku pra-muat (`none`, `metadata`, `auto`) |

## Analogi (tema RUMAH/BANGUNAN)

Tag `<audio>` seperti **bel pintu elektronik** di rumah. Kamu bisa memilih jenis suara bel (`source`), mengatur volume (kontrol), membuatnya berbunyi otomatis saat ada tamu (`autoplay`), menonaktifkan suara (`muted`) jika tengah malam, atau memutar dering berulang (`loop`). Pemilik rumah bisa memilih file suara yang didukung oleh perangkat bel — sama seperti browser memilih *source* yang kompatibel.

## Dipakai Untuk

- Memutar musik latar website
- Podcast dan rekaman suara
- Preview lagu di toko musik
- Suara notifikasi atau feedback interaktif
- Audio pembelajaran / kursus online

## Kesalahan Umum

- **Autoplay tanpa muted**: Sebagian besar browser modern memblokir autoplay bersuara.
- **Tidak menyediakan multiple source**: Tidak semua browser mendukung format yang sama.
- **Lupa menambahkan `controls`**: Pengguna tidak bisa memulai/menghentikan audio.
- **Menempatkan teks fallback di luar tag `<audio>`**: Teks fallback harus di dalam tag agar muncul jika browser tidak mendukung.
- **Tidak menentukan `type` pada `<source>`**: Membuat browser bekerja lebih keras untuk mendeteksi format.

## Koneksi dengan Materi Sebelumnya

- **Tag `<img>`**: Sama-sama elemen media yang bisa disematkan di halaman.
- **Atribut `src`**: Baik `<img>` maupun `<source>` di `<audio>` menggunakan `src` untuk lokasi file.
- **Tag `<video>`**: Struktur `<video>` sangat mirip dengan `<audio>` (controls, source, atribut).

## Soal Latihan

1. Buat tag `<audio>` untuk memutar file `lagu.mp3` dengan kontrol, autoplay (tanpa suara), dan loop.
2. Mengapa `autoplay` sering dipasangkan dengan `muted`?
3. Tulis struktur `<audio>` dengan dua source untuk kompatibilitas penuh.

<details><summary>Jawaban</summary>

1. 
```html
<audio controls autoplay muted loop>
  <source src="lagu.mp3" type="audio/mpeg">
</audio>
```

2. Sebagian besar browser modern menerapkan kebijakan *autoplay policy* yang memblokir pemutaran otomatis dengan suara. Dengan menambahkan `muted`, browser memperbolehkan autoplay, lalu pengguna bisa mengaktifkan suara secara manual.

3. 
```html
<audio controls>
  <source src="audio.mp3" type="audio/mpeg">
  <source src="audio.ogg" type="audio/ogg">
  Browser Anda tidak mendukung pemutar audio.
</audio>
```

</details>
