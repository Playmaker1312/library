# Heading `h1` hingga `h6`

## Penjelasan
Tag `<h1>` sampai `<h6>` adalah elemen heading (judul) di HTML yang membentuk hierarki konten. `<h1>` adalah level tertinggi (paling penting), `<h6>` adalah level terendah. Heading bukan sekadar teks besar — ia memberi struktur semantik pada dokumen.

## Fungsi
- Membuat judul dan sub-judul yang terstruktur
- Membantu mesin pencari (SEO) memahami hierarki konten
- Mempermudah pembaca (dan pembaca layar) menavigasi halaman
- Setiap heading memiliki ukuran default berbeda (`h1` paling besar, `h6` paling kecil)

## Cara Pengimplementasian
```html
<h1>Judul Utama Halaman</h1>
<h2>Bab 1: Pendahuluan</h2>
<h3>1.1 Latar Belakang</h3>
<h3>1.2 Rumusan Masalah</h3>
<h2>Bab 2: Pembahasan</h2>
<h3>2.1 Teori Dasar</h3>
<h4>2.1.1 Sub-Teori</h4>
```

## Analogi
Seperti **struktur bangunan bertingkat**. `<h1>` adalah atap/ lantai paling atas yang mewakili seluruh bangunan. `<h2>` adalah lantai-lantai utama. `<h3>` adalah ruangan di setiap lantai. `<h4>` adalah lemari di dalam ruangan. Hanya boleh ada SATU atap (`h1`), dan kamu tidak bisa punya lemari (`h4`) tanpa ruangan (`h3`) atau lantai (`h2`).

## Dipakai Untuk
- `<h1>` — judul utama halaman (hanya **satu** per halaman)
- `<h2>` — judul bab / section besar
- `<h3>` — sub-bab
- `<h4>` hingga `<h6>` — sub-bab yang lebih dalam

## Kesalahan Umum
- Menggunakan `<h1>` lebih dari satu kali dalam satu halaman
- Melompati level heading (misal dari `<h1>` langsung ke `<h3>`)
- Menggunakan heading hanya untuk memperbesar teks — pakai CSS `font-size` jika hanya butuh ukuran
- Meletakkan heading di dalam `<header>` atau `<footer>` tanpa struktur

## Koneksi dengan Materi Sebelumnya
Di materi **Struktur Dasar HTML** kamu sudah punya kerangka `<html>`, `<head>`, `<body>`. Heading adalah elemen pertama yang memberi "daging" pada `<body>` dan mulai membentuk outline dokumen.

## Soal Latihan
1. Berapa jumlah tag heading dalam HTML?
2. Sebutkan aturan penting tentang `<h1>`!
3. Mana yang lebih besar secara default, `<h3>` atau `<h4>`?
4. Benarkah kita boleh menggunakan `<h1>` di setiap `<section>`? Jelaskan!
5. Tulis struktur heading untuk artikel dengan judul "Sejarah Internet", punya 3 bab, dan bab 1 punya 2 sub-bab!

<details><summary>Jawaban</summary>
1. Ada 6: `h1` sampai `h6`.<br>
2. `<h1>` hanya boleh digunakan **satu kali** per halaman sebagai judul utama.<br>
3. `<h3>` lebih besar dari `<h4>`.<br>
4. **Tidak** — `<h1>` hanya satu per halaman. Gunakan `<h2>` untuk judul section di bawahnya.<br>
5. 
```html
<h1>Sejarah Internet</h1>
<h2>Bab 1: Awal Mula</h2>
<h3>1.1 ARPANET</h3>
<h3>1.2 Protokol TCP/IP</h3>
<h2>Bab 2: Perkembangan</h2>
<h2>Bab 3: Internet Modern</h2>
```
</details>
