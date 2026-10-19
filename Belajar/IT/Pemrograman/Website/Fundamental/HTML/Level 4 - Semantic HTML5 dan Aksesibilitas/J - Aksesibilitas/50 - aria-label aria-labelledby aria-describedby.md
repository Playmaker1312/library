# aria-label, aria-labelledby, aria-describedby

## Penjelasan
Ketiga atribut ini adalah mekanisme ARIA untuk memberikan **nama** (*accessible name*) dan **deskripsi** (*accessible description*) pada elemen HTML, terutama elemen interaktif yang tidak memiliki teks visual yang jelas. Ketiganya tidak mengubah tampilan visual elemen, hanya memengaruhi bagaimana *assistive technology* (seperti *screen reader*) membacakannya.

| Atribut | Fungsi | Sumber Teks |
|---------|--------|-------------|
| `aria-label` | Memberi label langsung via string | Nilai atribut itu sendiri |
| `aria-labelledby` | Merujuk elemen lain sebagai label | ID elemen lain |
| `aria-describedby` | Memberi deskripsi tambahan | ID elemen lain |

## Fungsi
- **aria-label**: Memberi nama pada elemen yang tidak memiliki teks visual (misal tombol icon saja).
- **aria-labelledby**: Menghubungkan elemen dengan label yang sudah ada di halaman (mirip `<label for="">` tapi bisa untuk elemen non-form).
- **aria-describedby**: Memberikan deskripsi tambahan atau petunjuk yang lebih panjang.

## Cara Pengimplementasian

### aria-label — label langsung
```html
<!-- Tombol dengan icon saja, tanpa teks -->
<button aria-label="Tutup dialog" onclick="closeDialog()">
  ✕
</button>

<!-- Input pencarian tanpa label visual -->
<input type="search" aria-label="Cari artikel" />

<!-- Navigasi dengan banyak landmark -->
<nav aria-label="Navigasi utama">
  <a href="/">Beranda</a>
</nav>
<nav aria-label="Navigasi footer">
  <a href="/privacy">Kebijakan Privasi</a>
</nav>
```

### aria-labelledby — merujuk label yang sudah ada
```html
<!-- Section dengan judul di luar -->
<section aria-labelledby="artikel-title">
  <h2 id="artikel-title">Cara Memasak Nasi</h2>
  <p>Isi artikel...</p>
</section>

<!-- Radio group dengan label -->
<div role="radiogroup" aria-labelledby="pilihan-label">
  <h3 id="pilihan-label">Pilih metode pembayaran</h3>
  <label><input type="radio" name="pay" /> Transfer</label>
  <label><input type="radio" name="pay" /> Kartu</label>
</div>
```

### aria-describedby — deskripsi tambahan
```html
<!-- Tombol dengan deskripsi -->
<button aria-describedby="save-desc" onclick="save()">Simpan</button>
<p id="save-desc" hidden>Menyimpan perubahan ke database. Proses ini bisa dicek di menu Riwayat.</p>

<!-- Input dengan petunjuk format -->
<label for="telp">No. Telepon</label>
<input id="telp" type="tel" aria-describedby="telp-hint" />
<p id="telp-hint">Gunakan format: +62 xxx xxxx xxxx</p>
```

### Prioritas Perhitungan Nama (*Accessible Name Calculation*)

*Screen reader* menghitung nama aksesibel dengan urutan prioritas:
1. `aria-labelledby` (jika ada)
2. `aria-label` (jika ada)
3. Atribut native (misal `<label for="">` pada form, `alt` pada gambar, `title`)
4. Konten elemen (teks di dalam elemen)
5. `placeholder` (paling rendah)

## Analogi (tema RUMAH/BANGUNAN)
Bayangkan rumah Anda adalah sebuah museum.

- **aria-label** = **Plang nama yang ditempel langsung di benda**. Ketika sebuah patung abstrak tidak memiliki nama di bagian dasarnya, Anda tempel kertas bertuliskan "Patung Garuda" langsung ke patung itu.

- **aria-labelledby** = **Plang nama yang merujuk ke buku panduan**. Di pintu masuk ruangan ada plang "Ruang 3 — lihat keterangan di papan informasi sebelah pintu". Pengunjung melihat papan informasi (elemen lain) untuk mengetahui nama ruangan.

- **aria-describedby** = **Stiker penjelasan tambahan**. Di samping lukisan ada stiker "DILARANG SENTUH — cat mudah rusak" — deskripsi yang tidak mengubah nama lukisan, tapi memberi informasi tambahan.

## Dipakai Untuk
- Tombol icon tanpa teks (hamburger menu, close, search).
- Landmark region yang sama jenisnya (beberapa `<nav>` dibedakan dengan `aria-label`).
- Elemen non-form yang perlu label (section, panel, dialog).
- Input form dengan petunjuk format (aria-describedby).
- Group elemen (radio group, checkbox group) yang labelnya ada di heading terdekat.
- Gambar dekoratif vs informatif.

## Kesalahan Umum
1. **aria-label di elemen non-interaktif yang tidak perlu**: `<p aria-label="paragraf">` — hanya membingungkan.
2. **aria-labelledby merujuk ke ID yang tidak ada**: *Screen reader* akan membaca fallback tapi hasilnya tidak terduga.
3. **Mengganti label native dengan aria-label di form**: `<input>` sudah punya `<label for="">` — `aria-label` akan **menimpa** nama native.
4. **aria-describedby berisi konten yang di-*hidden***: Hati-hati, konten hidden tetap dibaca sebagai deskripsi.
5. **Menggunakan aria-label dan aria-labelledby bersamaan**: `aria-labelledby` menang, `aria-label` diabaikan — ini jarang diinginkan.
6. **Teks label terlalu panjang**: `aria-label` sebaiknya singkat (1-3 kata), deskripsi panjang gunakan `aria-describedby`.

## Koneksi dengan Materi Sebelumnya
- **ARIA Fundamentals** (49 - ARIA.md): Ketiga atribut ini adalah bagian dari *ARIA states & properties* — fondasi pemberian label aksesibel.
- **HTML Label Element** (Level 2/3): `<label for="">` adalah cara native untuk form — `aria-label` adalah alternatif untuk non-form.
- **alt attribute** (Level 2): Konsep *accessible name* — mirip dengan `alt` pada gambar yang memberi nama untuk *screen reader*.
- **Semantic HTML5**: Elemen semantik memiliki *accessible name* bawaan (misal `<nav>` punya role "navigation") — tinggal ditambahi `aria-label` jika perlu dibedakan.

## Soal Latihan

1. **Apa perbedaan utama antara `aria-label` dan `aria-labelledby`?**

<details><summary>Jawaban</summary>
`aria-label` menerima teks langsung sebagai nilai string. `aria-labelledby` menerima satu atau lebih ID elemen lain yang teksnya akan dijadikan label. `aria-labelledby` memiliki prioritas lebih tinggi dalam *accessible name calculation*.
</details>

2. **Buat kode sebuah tombol dengan ikon "mata" yang berfungsi toggle password visibility. Beri label aksesibel yang berubah antara "Tampilkan password" dan "Sembunyikan password".**

<details><summary>Jawaban</summary>
```html
<button id="togglePass" aria-label="Tampilkan password" onclick="toggleVisibility()">
  👁️
</button>
<input type="password" id="password" />

<script>
function toggleVisibility() {
  const input = document.getElementById('password');
  const btn = document.getElementById('togglePass');
  const isPassword = input.type === 'password';
  input.type = isPassword ? 'text' : 'password';
  btn.setAttribute('aria-label', isPassword ? 'Sembunyikan password' : 'Tampilkan password');
}
</script>
```
</details>

3. **Apa yang terjadi jika `aria-labelledby` merujuk ke ID yang tidak ditemukan di halaman?**

<details><summary>Jawaban</summary>
*Screen reader* akan mengabaikan `aria-labelledby` dan turun ke prioritas berikutnya dalam *accessible name calculation* (`aria-label` → label native → konten elemen). Tidak ada error di konsol, sehingga bug ini sulit dideteksi.
</details>

4. **Kapan sebaiknya menggunakan `aria-describedby` daripada `aria-label`?**

<details><summary>Jawaban</summary>
`aria-describedby` digunakan ketika informasi bersifat deskriptif atau penjelasan panjang, bukan label singkat. Contoh: petunjuk format input, deskripsi fungsi tombol yang kompleks, atau catatan hukum. `aria-describedby` dibaca sebagai deskripsi setelah nama, sedangkan `aria-label` menjadi nama utama.
</details>

5. **Perbaiki kode berikut dengan memberi label yang tepat:**
```html
<nav>
  <a href="/">Beranda</a>
</nav>
<nav>
  <a href="/sosmed">Instagram</a>
  <a href="/sosmed">Twitter</a>
</nav>
```

<details><summary>Jawaban</summary>
```html
<nav aria-label="Navigasi utama">
  <a href="/">Beranda</a>
</nav>
<nav aria-label="Navigasi media sosial">
  <a href="/sosmed">Instagram</a>
  <a href="/sosmed">Twitter</a>
</nav>
```
Dengan `aria-label`, pengguna *screen reader* bisa membedakan kedua `<nav>` saat menggunakan navigasi landmark.
</details>
