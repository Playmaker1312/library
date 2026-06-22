# Navigasi Keyboard

## Penjelasan
Navigasi keyboard adalah kemampuan pengguna untuk menjelajahi dan berinteraksi dengan seluruh konten halaman web hanya menggunakan keyboard, tanpa mouse. Ini merupakan fondasi aksesibilitas web karena banyak pengguna dengan disabilitas motorik, pengguna *screen reader*, dan *power users* mengandalkan keyboard untuk bernavigasi.

Dua komponen utama:
1. **Tabindex** — mengontrol urutan dan kemampuan fokus keyboard.
2. **Skip navigation link** — tautan lompat untuk melewati konten berulang.

## Fungsi
- **Tabindex**: Menentukan apakah suatu elemen bisa difokuskan (*focusable*) via keyboard dan dalam urutan apa.
- **Skip navigation link**: Memberi jalan pintas bagi pengguna keyboard/*screen reader* untuk langsung menuju konten utama, melewati navigasi yang berulang di setiap halaman.

## Cara Pengimplementasian

### Tabindex
`tabindex` memiliki tiga kategori nilai:

| Nilai | Perilaku |
|-------|----------|
| `tabindex="0"` | Elemen bisa difokuskan dalam urutan alami DOM |
| `tabindex="-1"` | Elemen bisa difokuskan secara programatis (JavaScript `.focus()`), tapi TIDAK via Tab |
| `tabindex="1"` (positif) | ⚠️ Elemen difokuskan lebih dulu dari urutan alami — **HINDARI** |

```html
<!-- tabindex="0": membuat div interaktif bisa difokuskan -->
<div role="button" tabindex="0" onclick="submit()">Kirim</div>

<!-- tabindex="-1": untuk elemen yang perlu fokus programatis -->
<div id="errorSummary" tabindex="-1" role="alert">
  Terdapat 3 error pada form.
</div>
<script>
  // Fokuskan ke ringkasan error setelah validasi
  document.getElementById('errorSummary').focus();
</script>

<!-- ❌ BURUK: tabindex positif mengacaukan urutan -->
<input tabindex="3" />
<button tabindex="1">Simpan</button>
<a tabindex="2" href="/">Beranda</a>
```

### Skip Navigation Link
Ini adalah tautan internal yang menjadi elemen pertama di halaman dan melompat ke konten utama.

```html
<!-- Skip link — harus menjadi elemen pertama setelah <body> -->
<body>
  <a href="#main-content" class="skip-link">Lompat ke konten utama</a>

  <header>
    <nav>
      <ul>
        <li><a href="/">Beranda</a></li>
        <li><a href="/tentang">Tentang</a></li>
        <li><a href="/kontak">Kontak</a></li>
      </ul>
    </nav>
  </header>

  <main id="main-content" tabindex="-1">
    <h1>Selamat Datang</h1>
    <p>Konten utama dimulai di sini...</p>
  </main>
</body>
```

```css
/* Skip link styling — muncul saat difokuskan */
.skip-link {
  position: absolute;
  top: -100%;
  left: 0;
  background: #000;
  color: #fff;
  padding: 8px 16px;
  z-index: 9999;
}

.skip-link:focus {
  top: 0;
}
```

### Urutan Fokus Alami
Urutan fokus default adalah urutan elemen di DOM (dari atas ke bawah). Elemen yang *focusable* secara native:
- `<a href="...">`
- `<button>`
- `<input>`, `<select>`, `<textarea>`
- Elemen dengan `tabindex="0"` atau `tabindex="N"` positif

### Manajemen Fokus untuk Komponen Dinamis
```html
<!-- Modal/Dialog — fokus dipindahkan ke dalam modal saat terbuka -->
<div id="modal" role="dialog" aria-labelledby="modal-title" aria-modal="true" hidden>
  <div>
    <h2 id="modal-title">Konfirmasi</h2>
    <p>Apakah Anda yakin?</p>
    <button onclick="closeModal()">Ya</button>
    <button onclick="closeModal()" autofocus>Batal</button>
  </div>
</div>

<script>
function openModal() {
  const modal = document.getElementById('modal');
  modal.hidden = false;
  // Fokus ke tombol "Batal" di dalam modal
  modal.querySelector('button:last-of-type').focus();
}
</script>
```

## Analogi (tema RUMAH/BANGUNAN)
Bayangkan rumah Anda memiliki **pintu otomatis** dan **jalan setapak**.

- **Tabindex** = **Nomor urut pintu**. Pintu utama (tabindex="0") bisa dibuka oleh siapa saja yang berjalan melewatinya secara alami. Ada pintu darurat (tabindex="-1") yang hanya bisa dibuka dari dalam (via tombol khusus, bukan dari luar). Jangan pernah memberi nomor urut sembarangan (tabindex positif) — itu seperti memaksa tamu masuk lewat pintu belakang dulu sebelum pintu depan.

- **Skip navigation link** = **Pintu lompatan**. Bayangkan setiap kali masuk rumah, Anda harus melewati 10 lorong berisi foto keluarga sebelum sampai ke ruang tamu. Skip link adalah **pintu rahasia** di pintu masuk yang langsung membawa Anda ke ruang tamu. Pengguna bisa memilih: lewati lorong (Tab) atau langsung ke ruang tamu (Enter pada skip link).

- **Fokus manajemen pada modal** = **Saat memasuki kamar khusus**. Ketika Anda masuk ke ruang aman (modal), pintu otomatis terkunci di belakang Anda — Anda tidak bisa Tab ke luar sampai ruang itu ditutup. Ini disebut *focus trapping*.

## Dipakai Untuk
- Semua halaman web — terutama halaman dengan navigasi berulang.
- Komponen interaktif kustom (dropdown, modal, slider, tree view).
- Form panjang — skip link bisa melompat ke ringkasan error.
- Aplikasi *single-page* (SPA) di mana konten berubah tanpa reload — fokus harus dipindahkan secara manual.
- Halaman dengan banyak *landmark region*.

## Kesalahan Umum
1. **Tabindex positif (1, 2, 3)**: Mengacaukan urutan fokus alami, sulit di-maintain. Gunakan tabindex="0" dan atur urutan DOM.
2. **Tidak ada skip link**: Pengguna keyboard harus Tab puluhan kali setiap halaman.
3. **Skip link tidak terlihat saat fokus**: *Keyboard-only user* tidak tahu ada skip link.
4. **Fokus hilang (lost focus)** saat konten berubah: Misal tombol di klik, konten berganti, fokus kembali ke awal halaman.
5. **Modal tanpa *focus trapping***: Fokus bisa keluar dari modal, pengguna keyboard kebingungan.
6. **Elemen dengan `tabindex="0"` tapi tidak interaktif**: `<div tabindex="0">` tanpa role/event — *dead zone*.
7. **Skip link menuju `id` yang tidak bisa difokuskan**: `<main id="main">` tanpa `tabindex="-1"` — beberapa *browser* tidak memindahkan fokus ke anchor.

## Koneksi dengan Materi Sebelumnya
- **ARIA roles** (49 - ARIA.md): Elemen dengan `role="button"` tetap perlu `tabindex="0"` untuk bisa difokuskan keyboard — ARIA hanya memberi informasi semantik, tidak membuat elemen *focusable*.
- **Semantic HTML** (Level 4): Elemen semantik seperti `<button>`, `<a>`, `<input>` sudah *focusable* secara native — alasan lain untuk memprioritaskan elemen native.
- **aria-label** (50 - aria-label.md): Skip link biasanya butuh label jika teksnya tidak deskriptif.
- **Media queries & CSS**: Skip link memanfaatkan `:focus` pseudo-class — koneksi dengan CSS interaksi.

## Soal Latihan

1. **Apa tiga kategori nilai `tabindex` dan perilaku masing-masing?**

<details><summary>Jawaban</summary>
1. `tabindex="0"` — elemen bisa difokuskan dalam urutan DOM alami.
2. `tabindex="-1"` — elemen bisa difokuskan programatis (JavaScript `.focus()`) tapi tidak via Tab.
3. `tabindex="N"` (positif) — elemen difokuskan lebih awal dari urutan alami; dihindari karena mengacaukan urutan.
</details>

2. **Mengapa `tabindex` positif (tabindex="1", "2", dst) tidak disarankan?**

<details><summary>Jawaban</summary>
Karena memisahkan urutan fokus dari urutan visual/DOM. Sulit di-maintain dan membingungkan pengguna — elemen dengan `tabindex="1"` akan difokuskan lebih dulu dari elemen `tabindex="0"` mana pun, tanpa memandang posisi visualnya. Gunakan `tabindex="0"` dan atur ulang DOM jika perlu.
</details>

3. **Buat kode sebuah skip navigation link lengkap dengan CSS yang muncul saat difokuskan.**

<details><summary>Jawaban</summary>
```html
<body>
  <a href="#main" class="skip-link">Lompat ke konten utama</a>

  <header>Navigasi...</header>

  <main id="main" tabindex="-1">
    <h1>Konten Utama</h1>
  </main>
</body>

<style>
.skip-link {
  position: absolute;
  top: -50px;
  left: 0;
  background: #000;
  color: #fff;
  padding: 8px 16px;
  z-index: 1000;
  transition: top 0.1s;
}
.skip-link:focus {
  top: 0;
}
</style>
```
</details>

4. **Apa yang dimaksud dengan *focus trapping* dan di mana teknik ini diperlukan?**

<details><summary>Jawaban</summary>
*Focus trapping* adalah teknik membatasi fokus keyboard di dalam suatu elemen (biasanya modal/dialog) saat elemen tersebut aktif. Saat pengguna Tab dan mencapai elemen terakhir di dalam modal, fokus akan kembali ke elemen pertama. Teknik ini diperlukan pada modal, dialog, menu, dan panel *slide-out* untuk mencegah pengguna keyboard terjebak di balik lapisan yang tidak terlihat.
</details>

5. **Setelah modal dialog ditutup, ke mana fokus harus dipindahkan? Mengapa?**

<details><summary>Jawaban</summary>
Fokus harus dikembalikan ke elemen yang memicu pembukaan modal (misal tombol "Buka dialog"). Ini penting agar pengguna keyboard/*screen reader* kembali ke posisi sebelumnya di halaman dan tidak "tersesat" setelah dialog ditutup. Tanpa ini, fokus bisa kembali ke awal halaman atau hilang sama sekali.
</details>
