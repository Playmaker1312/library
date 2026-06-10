# 132: WebAssembly — High Performance di Browser

## 1. Penjelasan
WebAssembly (WASM) adalah binary instruction format yang berjalan di browser dengan performa mendekati native. Kode dari C++, Rust, Go, atau Zig bisa dikompilasi ke `.wasm` dan dipanggil dari JavaScript. WASM sangat cocok untuk tugas komputasi berat: game engine, video/image processing, enkripsi, kalkulasi ilmiah.

## 2. Fungsi
- Eksekusi kode hampir secepat native
- Bahasa lain (Rust, C++) bisa berjalan di browser
- Isolasi memori aman — linear memory, tidak bisa akses DOM langsung
- Komputasi paralel dengan Web Workers
- Use case: fisika game, kompresi data, rendering 3D, PDF parsing
- Bukan pengganti JS — WASM untuk tugas berat, JS untuk DOM/UX

## 3. Code

```rust
// src/lib.rs — fungsi Rust untuk komputasi (kompilasi ke WASM)
use wasm_bindgen::prelude::*;

#[wasm_bindgen]
pub fn hitung_fibonacci(n: u32) -> u64 {
    if n <= 1 { return n as u64; }
    let mut a = 0u64;
    let mut b = 1u64;
    for _ in 2..=n {
        let temp = a + b;
        a = b;
        b = temp;
    }
    b
}

#[wasm_bindgen]
pub fn proses_buku_parallel(data: &[u8]) -> Vec<u8> {
    // Simulasi komputasi berat: sorting, filter, analisis data buku
    let mut result = data.to_vec();
    result.sort();
    result
}
```

```bash
# Kompilasi Rust ke WASM
wasm-pack build --target web
```

```ts
// Panggil WASM dari Vue/Nuxt
import init, { hitung_fibonacci, proses_buku_parallel } from './pkg/wasm_utils.js'

onMounted(async () => {
  await init() // inisialisasi WASM

  // Panggil fungsi Rust
  const fib = hitung_fibonacci(50)
  console.log('Fibonacci(50):', fib) // 12586269025 — cepat!

  // Komputasi data buku
  const processed = proses_buku_parallel(new Uint8Array([5, 3, 8, 1, 9]))
  console.log('Processed:', Array.from(processed))
})
```

```vue
<!-- Komponen yang menggunakan WASM untuk pencarian fuzzy -->
<script setup>
import init, { fuzzy_search } from '~/wasm/search_wasm.js'
const results = ref([])

async function searchBooks(query: string) {
  await init()
  const books = booksData.value.map(b => b.title)
  const matches = fuzzy_search(query, books, 0.3) // threshold 30%
  results.value = matches
}
</script>
```

## 4. Analogi Rumah

| Konsep WASM | Analogi Rumah |
|-------------|---------------|
| WASM module | Mesin industri di rumah — untuk tugas berat |
| JavaScript | Palu biasa — untuk tugas ringan sehari-hari |
| Linear memory | Ruang mesin khusus — aman, terisolasi |
| `wasm-bindgen` | Adaptor mesin — menghubungkan mesin industri ke listrik rumah |
| Komputasi paralel | Beberapa mesin berjalan bersamaan |
| WASM + JS | Tukang kayu (JS) panggil forklift (WASM) untuk angkat beban berat |

## 5. Use Case
- **Pencarian fuzzy cepat** di perpustakaan ribuan buku
- **Image processing** — kompresi sampul buku di client
- **PDF generation/parsing** — generate laporan perpustakaan
- **Enkripsi/dekripsi** data anggota
- **Game** — physics engine di browser

## 6. Kesalahan Umum
- **Pindahkan semua logika ke WASM** → WASM tidak bisa akses DOM, heavy I/O tetap lambat. Hanya komputasi murni yang cocok.
- **Lupa inisialisasi WASM** → `await init()` harus dipanggil sebelum fungsi WASM. Tanpa ini, error.
- **Bundle terlalu besar** → WASM file bisa besar (> 1MB). Lazy load hanya saat dibutuhkan.
- **Tidak handle error** → WASM bisa panic tanpa pesan jelas di JS. Gunakan `try-catch` di binding.

## 7. Benang Merah
Micro-frontend (131) bisa memuat WASM sebagai service independen. Animasi (133) bisa memanfaatkan WASM untuk physics-based animation. WASM adalah alat spesifik — digunakan ketika JS tidak cukup cepat, bukan untuk setiap masalah.

## 8. Soal

**Soal 1:** Apa yang dimaksud WebAssembly dan mengapa lebih cepat dari JavaScript untuk tugas tertentu?

**Soal 2:** Kapan sebaiknya menggunakan WASM dan kapan tetap menggunakan JS?

**Soal 3:** Bagaimana cara memanggil fungsi Rust dari JavaScript di browser?

---
<details>
<summary>Jawaban</summary>

**Jawaban 1:** WebAssembly adalah binary instruction format yang berjalan di browser dengan performa mendekati native. Lebih cepat karena format binary sudah dikompilasi (tidak perlu di-parse seperti JS), tipe statis, dan eksekusi langsung ke machine code.

**Jawaban 2:** WASM untuk komputasi berat (numerik, kriptografi, image processing, game physics). JS tetap untuk DOM manipulation, event handling, I/O, dan interaksi UI.

**Jawaban 3:** Tulis fungsi Rust dengan `#[wasm_bindgen]`, kompilasi dengan `wasm-pack build --target web`, lalu import dan panggil `init()` sekali, baru gunakan fungsi Rust seperti fungsi JS biasa.
</details>
