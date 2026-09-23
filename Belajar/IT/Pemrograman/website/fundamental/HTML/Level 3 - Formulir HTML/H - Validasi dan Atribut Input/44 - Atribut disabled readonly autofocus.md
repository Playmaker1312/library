# disabled, readonly, autofocus

## Penjelasan

Tiga atribut yang memengaruhi interaktivitas dan fokus elemen input. `disabled` membuat input tidak bisa diisi dan datanya **tidak** dikirim. `readonly` membuat input tidak bisa diedit tapi datanya **tetap** dikirim. `autofocus` langsung memindahkan kursor ke input saat halaman dimuat.

## Fungsi

- **`disabled`** — Input tidak dapat diakses/tidak bisa diedit. Form tidak mengirim data dari elemen ini. Elemen juga tidak kena tab navigation.
- **`readonly`** — Input tampak normal tetapi tidak bisa diedit. Data **tetap** terkirim saat submit. Kursor bisa masuk, teks bisa dipilih/disalin.
- **`autofocus`** — Memberi fokus ke input segera setelah halaman siap. Hanya **satu** elemen per halaman yang boleh punya atribut ini.

## Cara Pengimplementasian

```html
<form>
  <!-- readonly: data tetap terkirim -->
  <label for="email">Email (terverifikasi):</label>
  <input type="email" id="email" name="email"
         value="user@example.com"
         readonly>

  <!-- disabled: data tidak terkirim -->
  <label for="role">Role:</label>
  <input type="text" id="role" name="role"
         value="Member"
         disabled>

  <!-- autofocus: langsung fokus -->
  <label for="nama">Nama:</label>
  <input type="text" id="nama" name="nama"
         autofocus
         required>

  <button type="submit">Kirim</button>
</form>
```

## Analogi (RUMAH/BANGUNAN)

- **`disabled`** = Ruangan yang **dikunci dan ditutup pita pengaman**. Tidak ada yang bisa masuk, konten di dalamnya tidak diikutkan dalam "inventaris rumah" saat dijual.
- **`readonly`** = Ruangan dengan **pintu kaca transparan**. Anda bisa melihat isinya (membaca) dan menyalin catatan ditempel, tetapi tidak bisa mengubah furnitur di dalamnya. Ruangan tetap dihitung dalam luas total rumah.
- **`autofocus`** = Lampu sensor yang langsung menyala menerangi pintu depan saat Anda masuk rumah. Hanya satu pintu masuk utama yang memiliki sensor ini.

## Dipakai Untuk

- `disabled` — Field yang tidak relevan berdasarkan pilihan sebelumnya (misal metode pembayaran "COD" maka field nomor kartu disabled), tombol submit setelah form dikirim.
- `readonly` — Data akun yang sudah diverifikasi (email, ID pengguna), perhitungan otomatis (total harga).
- `autofocus` — Field pencarian utama, field pertama pada form login/registrasi.

## Kesalahan Umum

1. **Mengira `readonly` mencegah pengiriman data** — Tidak. `readonly` tetap mengirim data. Gunakan `disabled` jika tidak ingin data dikirim.
2. **Lebih dari satu `autofocus` dalam satu halaman** — Browser hanya akan memfokuskan elemen `autofocus` yang pertama ditemukan. Lainnya diabaikan. Ini melanggar aksesibilitas.
3. **`disabled` tidak bisa diberi style CSS standar** — Browser memberi warna abu-abu default. Jika ingin style khusus, gunakan `[disabled]` selector di CSS.
4. **Menggunakan `pointer-events: none` sebagai ganti `disabled`** — Data tetap terkirim dan tab navigation tetap jalan. Tidak setara.

## Koneksi dengan Materi Sebelumnya

- **`required`** — `disabled` akan mengesampingkan `required` (karena data tidak dikirim, validasi required diabaikan). `readonly` tetap mempertahankan `required`.
- **Atribut `value`** — Input `readonly` biasanya punya `value` tetap. Input `disabled` juga bisa punya `value` tapi tidak dikirim.
- **CSS pseudo-class** — `:disabled`, `:read-only`, `:focus` untuk styling.
- **Navigasi keyboard** — `disabled` melompati tab index; `readonly` tetap bisa di-tab.

## Soal Latihan

1. Buat form registrasi dengan: field email (readonly, sudah terisi), field password (autofocus), dan tombol submit (disabled sampai ada input di field lain).
2. Sebutkan perbedaan utama antara `disabled` dan `readonly` dalam hal: (a) pengiriman data, (b) kemampuan diedit, (c) tab navigation.
3. Mengapa hanya boleh ada satu `autofocus` per halaman?

<details><summary>Jawaban</summary>

1.
```html
<form>
  <label for="email">Email:</label>
  <input type="email" id="email" name="email"
         value="user@domain.com" readonly>

  <label for="pass">Password Baru:</label>
  <input type="password" id="pass" name="pass"
         autofocus required>

  <button type="submit" id="submit" disabled>Daftar</button>
</form>
```

2.
| Aspek | `disabled` | `readonly` |
|-------|-----------|-----------|
| Data dikirim? | Tidak | Ya |
| Bisa diedit? | Tidak | Tidak |
| Tab navigation? | Dilewati | Tetap bisa di-tab |
| Nilai bisa disalin? | Tidak (di beberapa browser) | Ya |

3. Hanya boleh satu `autofocus` karena halaman hanya bisa memiliki satu fokus dalam satu waktu. Lebih dari satu akan membingungkan pengguna (terutama pengguna screen reader) dan browser hanya akan menghormati elemen `autofocus` pertama yang ditemukan. Secara aksesibilitas, `autofocus` juga harus digunakan hati-hati karena bisa memindahkan fokus paksa tanpa persetujuan pengguna.

</details>
