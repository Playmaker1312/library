# Atribut `target="_blank"`

## Penjelasan

Atribut `target="_blank"` pada tag `<a>` memberitahu browser untuk membuka tautan di tab atau jendela baru. Atribut ini harus selalu disertai `rel="noopener noreferrer"` untuk alasan keamanan.

## Fungsi

Memungkinkan pengguna membuka tautan tanpa meninggalkan halaman asal, sehingga pengguna tetap berada di website kita.

## Cara Pengimplementasian

```html
<!-- Cara yang benar dan aman -->
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
  Kunjungi Example (tab baru)
</a>

<!-- Jangan lupa sertakan rel -->
<!-- JANGAN: <a href="https://example.com" target="_blank"> -->
```

**Kenapa `rel="noopener noreferrer"`?**
- `noopener`: mencegah halaman baru mengakses `window.opener` (celah keamanan *tabnapping*)
- `noreferrer`: menyembunyikan informasi *referrer* (asal tautan) dari halaman tujuan

## Analogi (tema RUMAH/BANGUNAN)

`target="_blank"` seperti **membuka pintu belakang** saat tamu datang. Tamu masuk melalui pintu baru tanpa harus menutup pintu depan. Tapi tanpa `rel="noopener noreferrer"`, tamu bisa menarik pintu depan dari dalam — tamu (halaman baru) bisa memanipulasi halaman asal kita.

## Dipakai Untuk

- Tautan ke situs eksternal
- Tautan unduhan file PDF/doc
- Tautan yang membuka video atau media
- Tautan referensi yang ingin tetap dipertahankan di tab asal

## Kesalahan Umum

- Tidak menyertakan `rel="noopener noreferrer"` — celah keamanan *tabnapping*
- Menggunakan `target="_blank"` untuk tautan internal — tidak perlu, buka di tab yang sama saja
- Lupa memberi indikasi visual atau ikon bahwa tautan akan dibuka di tab baru — membingungkan pengguna

## Koneksi dengan Materi Sebelumnya

Setelah memahami tag `<a>` dasar, atribut `target="_blank"` memberikan kontrol lebih atas *user experience* saat meninggalkan website, terutama untuk tautan eksternal yang membutuhkan hubungan aman antara halaman asal dan halaman tujuan.

## Soal Latihan

1. Buat tautan ke https://google.com yang terbuka di tab baru dengan aman.
2. Apa yang terjadi jika `rel="noopener"` tidak disertakan?

<details><summary>Jawaban</summary>

1. `<a href="https://google.com" target="_blank" rel="noopener noreferrer">Google (tab baru)</a>`
2. Halaman baru bisa mengakses objek `window.opener` dan memanipulasi halaman asal (misalnya mengubah konten atau mengarahkan ke URL phishing) — dikenal sebagai serangan *tabnapping*.

</details>
