# ARIA (Accessible Rich Internet Applications)

## Penjelasan
ARIA adalah kumpulan atribut khusus yang ditambahkan ke elemen HTML untuk meningkatkan aksesibilitas konten web dinamis dan komponen antarmuka pengguna (UI) yang kompleks. ARIA memberikan informasi tambahan kepada *assistive technology* (seperti *screen reader*) tentang peran, keadaan, dan properti suatu elemen.

## Fungsi
- Menambahkan makna semantik pada elemen yang tidak memiliki semantik bawaan (seperti `<div>` atau `<span>`).
- Memberi tahu *screen reader* tentang peran suatu elemen (misalnya apakah itu tombol, tab, atau dialog).
- Menyampaikan keadaan (apakah checkbox sedang dicentang, apakah menu sedang terbuka).
- Menyediakan navigasi yang lebih baik untuk pengguna *assistive technology*.

## Cara Pengimplementasian
```html
<!-- role: memberi tahu peran elemen -->
<div role="button" tabindex="0" onclick="submitForm()">Kirim</div>

<!-- aria-checked: keadaan checkbox kustom -->
<div role="checkbox" aria-checked="true" tabindex="0">Setuju</div>

<!-- aria-expanded: apakah menu sedang terbuka -->
<button aria-expanded="false" onclick="toggleMenu()">Menu</button>
<div id="menu" hidden>Isi menu...</div>

<!-- aria-hidden: sembunyikan elemen dekoratif dari screen reader -->
<i class="icon" aria-hidden="true"></i>
```

### Aturan #1 ARIA: Jangan pakai ARIA jika HTML native sudah cukup
```html
<!-- ❌ BURUK: pakai ARIA padahal HTML native sudah ada -->
<div role="button" tabindex="0" onclick="submit()">Kirim</div>

<!-- ✅ BAIK: pakai elemen HTML native -->
<button onclick="submit()">Kirim</button>
```

```html
<!-- ❌ BURUK: pakai role="navigation" di <div> -->
<div role="navigation">...</div>

<!-- ✅ BAIK: pakai <nav> -->
<nav>...</nav>
```

### Aturan Penting ARIA Lainnya
- Jangan mengubah semantik native: `<h1 role="button">` — tetap dibacakan sebagai heading.
- Semua elemen interaktif harus bisa dijangkau keyboard (tabindex).
- ARIA tidak mengubah fungsionalitas, hanya informasi untuk *screen reader*.

## Analogi (tema RUMAH/BANGUNAN)
ARIA itu seperti **stiker atau plang informasi** yang ditempelkan di berbagai bagian rumah.

Bayangkan rumah Anda:
- **Pintu** sudah jelas fungsinya sebagai pintu (HTML native `<button>`). Tidak perlu stiker.
- Tapi ada **panel sentuh di dinding** yang fungsinya seperti saklar — bentuknya tidak seperti saklar pada umumnya. Anda tempelkan stiker "SAKLAR LAMPU" (role="button", aria-pressed).
- **Jendela** yang bisa dibuka/tutup — Anda tempel stiker "JENDELA — TERBUKA" (aria-expanded="true").
- **Lemari tersembunyi** di balik panel dinding — Anda tempel stiker "LEMARI — TERSEMBUNYI" (aria-hidden).

Stiker tidak mengubah fungsi bangunan, tapi membantu penghuni (pengguna *assistive technology*) memahami apa yang ada di depan mereka.

## Dipakai Untuk
- Komponen UI kustom (tombol kustom, checkbox kustom, slider, tab panel, dialog/modal, tree view, menu dropdown, progress bar).
- Area *live region* yang kontennya berubah secara dinamis (misal notifikasi, error form).
- Landmark region untuk navigasi cepat.
- Memberi label ke elemen yang tidak memiliki teks visual.
- Situasi ketika HTML5 semantik belum menyediakan elemen yang tepat.

## Kesalahan Umum
1. **ARIA berlebihan**: Pakai ARIA padahal `<button>`, `<nav>`, `<header>` sudah cukup.
2. **Mengubah semantik native**: `<h2 role="button">` — *screen reader* tetap baca sebagai heading level 2.
3. **Lupa menambahkan keyboard support**: ARIA role="button" tanpa tabindex dan event keyboard.
4. **aria-hidden="true" pada elemen fokus**: Menyembunyikan elemen yang bisa difokuskan — membuat *dead zone*.
5. **Salah menggunakan role**: role="alert" dipakai di elemen yang tidak *live*.
6. **Tidak mengupdate state**: aria-expanded tetap "false" padahal menu sudah terbuka.

## Koneksi dengan Materi Sebelumnya
- **HTML5 Semantic Elements** (Level 4): elemen semantik seperti `<nav>`, `<main>`, `<button>` adalah fondasi — ARIA digunakan sebagai pelengkap ketika elemen semantik tidak mencukupi.
- **aria-label / aria-labelledby** (materi selanjutnya): cara memberi nama elemen yang tidak punya teks visual — bagian dari aturan *Name Calculation* ARIA.
- **CSS**: ARIA bisa dikombinasikan dengan *attribute selector* CSS seperti `[aria-expanded="true"]` untuk *styling*.

## Soal Latihan

1. **Apa aturan #1 dalam penggunaan ARIA? Mengapa?**

<details><summary>Jawaban</summary>
Aturan #1: Jangan gunakan ARIA jika elemen HTML native sudah cukup. Karena elemen native sudah memiliki semantik, perilaku keyboard, dan dukungan *assistive technology* bawaan. ARIA hanya *polyfill* semantik, bukan pengganti semantik native.
</details>

2. **Perbaiki kode berikut agar tidak menggunakan ARIA yang tidak perlu:**
```html
<div role="navigation">
  <a href="/">Beranda</a>
  <a href="/tentang">Tentang</a>
</div>
```

<details><summary>Jawaban</summary>
```html
<nav>
  <a href="/">Beranda</a>
  <a href="/tentang">Tentang</a>
</nav>
```
</details>

3. **Apa fungsi dari `aria-expanded` dan pada elemen apa biasanya digunakan?**

<details><summary>Jawaban</summary>
`aria-expanded` menyatakan apakah suatu elemen (seperti menu dropdown atau accordion panel) sedang terbuka (true) atau tertutup (false). Biasanya digunakan pada tombol yang mengontrol menu, accordion, atau tree view.
</details>

4. **Mengapa `aria-hidden="true"` berbahaya jika diterapkan pada elemen yang bisa menerima fokus keyboard?**

<details><summary>Jawaban</summary>
Jika elemen yang bisa difokuskan (seperti link, tombol, input) disembunyikan dengan `aria-hidden="true"`, *screen reader* tidak akan membacakannya, tetapi pengguna masih bisa menavigasi ke elemen tersebut dengan keyboard (Tab). Ini menciptakan *dead zone* — fokus keyboard pindah ke elemen yang tidak terdengar, membingungkan pengguna.
</details>

5. **Buat kode sebuah tombol kustom (div) yang berfungsi sebagai tombol "Play/Pause" dengan menggunakan ARIA yang tepat, ditambah dukungan keyboard (Spasi dan Enter).**

<details><summary>Jawaban</summary>
```html
<div
  role="button"
  tabindex="0"
  aria-pressed="false"
  aria-label="Play"
  id="playBtn"
  onclick="togglePlay()"
  onkeydown="if(event.key==='Enter'||event.key===' '){togglePlay();event.preventDefault()}"
>
  ▶
</div>

<script>
function togglePlay() {
  const btn = document.getElementById('playBtn');
  const isPlaying = btn.getAttribute('aria-pressed') === 'true';
  btn.setAttribute('aria-pressed', !isPlaying);
  btn.setAttribute('aria-label', isPlaying ? 'Play' : 'Pause');
  btn.textContent = isPlaying ? '▶' : '⏸';
}
</script>
```
</details>
