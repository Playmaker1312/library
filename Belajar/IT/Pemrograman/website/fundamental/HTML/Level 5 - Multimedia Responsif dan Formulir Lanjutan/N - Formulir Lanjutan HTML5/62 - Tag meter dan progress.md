# Tag meter dan progress

## Penjelasan

**`<meter>`** dan **`<progress>`** adalah elemen semantik HTML5 yang merepresentasikan data numerik secara visual. **`<meter>`** digunakan untuk menunjukkan nilai dalam rentang yang diketahui (seperti level baterai, skor, atau kapasitas penyimpanan) dengan indikator visual seperti warna. **`<progress>`** digunakan untuk menunjukkan kemajuan suatu tugas yang sedang berlangsung (seperti pengunduhan file, pengisian formulir bertahap, atau instalasi). Keduanya memberikan pengalaman visual yang lebih informatif dibanding sekadar teks angka.

## Fungsi

| Elemen | Atribut | Fungsi |
|---|---|---|
| `<meter>` | `value`, `min`, `max`, `low`, `high`, `optimum` | Mengukur nilai dalam rentang tetap dengan ambang batas tertentu |
| `<progress>` | `value`, `max` | Menunjukkan kemajuan progres suatu proses |
| `<progress>` (tanpa value) | — | Indeterminate — proses berlangsung tanpa diketahui batas akhirnya |

## Cara Pengimplementasian

```html
<!-- METER: Level Kapasitas Rumah -->
<p>Kapasitas Listrik:
  <meter value="65" min="0" max="100" low="30" high="80" optimum="50">65%</meter>
</p>
<p>Ketinggian Air Sumur:
  <meter value="85" min="0" max="100" low="20" high="90" optimum="70">85%</meter>
</p>

<!-- PROGRESS: Proses Pembangunan -->
<label>Progres Pembangunan Rumah:</label>
<progress value="75" max="100">75%</progress>

<!-- PROGRESS INDETERMINATE -->
<label>Sedang memeriksa material...</label>
<progress>Memeriksa...</progress>
```

## Analogi (Tema RUMAH/BANGUNAN)

- **`<meter>`** → Seperti **meteran listrik** di rumah. Anda bisa melihat seberapa besar pemakaian daya saat ini (value). Ada indikator "aman" (hijau), "waspada" (kuning), dan "bahaya" (merah). `low` dan `high` menentukan zona aman, sementara `optimum` adalah nilai paling ideal. Ini adalah **alat ukur** — ia menunjukkan status terkini.

- **`<progress>`** → Seperti **grafik kemajuan pembangunan rumah** di papan proyek. Jika sudah 75% selesai, progress bar akan terisi 75%. Ini menunjukkan **sejauh mana suatu proses telah berjalan**.

- **`<progress>`** indeterminate → Seperti **tukang yang sedang mengecat** — Anda tahu ada aktivitas yang berlangsung, tapi tidak tahu kapan selesainya. Tidak ada batas pasti.

## Dipakai Untuk

- **`<meter>`**: Indikator kapasitas penyimpanan (disk usage), level baterai, skor ujian, rating, suhu ruangan, nilai tukar, kapasitas daya listrik.
- **`<progress>`**: Progress bar upload/download file, pengisian data multi-step, instalasi software, loading screen dengan durasi diketahui.
- **`<progress>` indeterminate**: Loading spinner/skeleton saat durasi tidak diketahui, sinkronisasi data, koneksi ke server.

## Kesalahan Umum

1. **Menggunakan `<meter>` untuk progres** → `<meter>` adalah alat ukur, bukan indikator progres. Salah kaprah yang paling umum. Ingat: meter = status saat ini, progress = kemajuan tugas.

2. **Tidak menyertakan teks di dalam tag** → Elemen `<meter>` dan `<progress>` perlu teks fallback untuk browser lama atau pembaca layar (accessibility). Selalu sertakan teks di antara tag pembuka dan penutup.

3. **Nilai `value` di luar rentang `min`–`max`** → Browser akan mengklamp (memotong) nilai ke rentang terdekat, tetapi ini membingungkan. Pastikan value selalu di antara min dan max.

4. **Menggunakan atribut `low`/`high`/`optimum` tanpa urutan yang logis** → Urutan atribut harus konsisten: `min < low < optimum < high < max`. Jika tidak, perilaku visual browser bisa salah.

5. **Lupa menambahkan teks alternatif untuk aksesibilitas** → Tanpa teks di dalam elemen, pengguna screen reader tidak mendapatkan informasi apa pun. Selalu beri teks.

## Koneksi dengan Materi Sebelumnya

- **Level 5 (Multimedia)**: `<progress>` sering dipasangkan dengan elemen `<video>` atau `<audio>` untuk menunjukkan buffering. Saat video di-load, progress bar menunjukkan seberapa banyak data yang sudah tersedia untuk diputar.
- **Level 5 (Formulir Lanjutan)**: `<meter>` berkaitan erat dengan `type="range"`. Range digunakan untuk **mengubah** nilai, sedangkan meter untuk **menampilkan** nilai. Keduanya sering dipasangkan.
- **Level 4 (Form Dasar)**: Atribut `value`, `min`, `max` sudah diperkenalkan di input number dan range. Di sini atribut yang sama digunakan dengan konteks yang berbeda.

## Soal Latihan

1. Buatlah elemen `<meter>` yang menampilkan penggunaan CPU sebesar 72% dengan nilai minimum 0, maksimum 100, low di 30, high di 85, dan optimum di 50.

2. Buatlah elemen `<progress>` yang menunjukkan progres pembangunan rumah telah mencapai 120 dari 200 hari kerja.

3. Jelaskan perbedaan fundamental antara `<meter>` dan `<progress>` menggunakan analogi pembangunan rumah.

4. Buatlah sebuah progress bar indeterminate di dalam paragraf yang memberi tahu pengguna bahwa "Sedang memeriksa fondasi rumah...".

<details><summary>Jawaban</summary>

**1. Meter penggunaan CPU:**
```html
<p>Penggunaan CPU: <meter value="72" min="0" max="100" low="30" high="85" optimum="50">72%</meter></p>
```
Zona: value (72) berada di antara low (30) dan high (85), sehingga indikator akan menampilkan warna kuning/waspada.

**2. Progress pembangunan rumah:**
```html
<label>Progres Pembangunan:</label>
<progress value="120" max="200">120 dari 200 hari kerja</progress>
```

**3. Perbedaan fundamental:**
- **`<meter>`** = **Termometer rumah**. Ia selalu menunjukkan suhu ruangan saat ini, tidak peduli apakah Anda sedang memanaskan atau mendinginkan ruangan. Ia mengukur **keadaan statis** dalam rentang yang sudah diketahui.
- **`<progress>`** = **Papan proyek pembangunan**. Ia menunjukkan seberapa jauh proses pembangunan telah berjalan dari awal hingga selesai. Ia mengukur **kemajuan dinamis** menuju tujuan akhir.

**4. Progress indeterminate:**
```html
<p>Sedang memeriksa fondasi rumah... <progress></progress></p>
```
Progress bar tanpa atribut `value` akan menampilkan animasi indeterminate (bergerak terus tanpa henti) yang menandakan proses sedang berjalan tanpa durasi pasti.

</details>
