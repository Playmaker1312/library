# 129: Accessibility (a11y) — Standar WCAG

## 1. Penjelasan
Accessibility (a11y) memastikan aplikasi web dapat digunakan oleh semua orang, termasuk penyandang disabilitas. WCAG 2.1 memiliki 4 prinsip: Perceivable (dapat dilihat/didengar), Operable (dapat dioperasikan), Understandable (dapat dipahami), Robust (kompatibel dengan assistive technology). Implementasi meliputi ARIA labels, roles, landmarks, keyboard navigation, color contrast.

## 2. Fungsi
- Navigasi keyboard untuk pengguna motorik terbatas
- Screen reader compatibility (ARIA, semantic HTML)
- Color contrast minimal 4.5:1 untuk teks normal
- Skip navigation link untuk lompat ke konten utama
- Focus trap untuk modal/dialog
- Meningkatkan SEO & user experience semua pengguna

## 3. Code

```vue
<!-- Skip Link + ARIA Landmark -->
<template>
  <a href="#main-content" class="skip-link">Langsung ke konten utama</a>

  <header role="banner">
    <nav aria-label="Navigasi utama">
      <ul>
        <li><NuxtLink to="/">Beranda</NuxtLink></li>
        <li><NuxtLink to="/books">Buku</NuxtLink></li>
      </ul>
    </nav>
  </header>

  <main id="main-content" role="main">
    <h1>Daftar Buku</h1>

    <div
      v-for="book in books"
      :key="book.id"
      role="article"
      :aria-label="`Buku: ${book.title} oleh ${book.author}`"
    >
      <h2>{{ book.title }}</h2>
      <p>{{ book.description }}</p>
      <button
        :aria-label="`Pinjam buku ${book.title}`"
        @click="borrowBook(book)"
      >
        Pinjam
      </button>
    </div>
  </main>
</template>
```

```vue
<!-- Focus Trap untuk Modal -->
<script setup>
const modalRef = ref(null)
const isOpen = ref(false)

watch(isOpen, (open) => {
  if (open) {
    nextTick(() => modalRef.value?.focus())
  }
})

function handleKeydown(e) {
  if (e.key === 'Escape') isOpen.value = false
  // focus trap logic untuk Tab cycle
}
</script>

<template>
  <div
    v-if="isOpen"
    ref="modalRef"
    role="dialog"
    aria-modal="true"
    :aria-label="'Detail buku'"
    tabindex="-1"
    @keydown="handleKeydown"
  >
    <button aria-label="Tutup dialog" @click="isOpen = false">X</button>
    <!-- konten modal -->
  </div>
</template>
```

```css
/* Skip link styling */
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #1a73e8;
  color: white;
  padding: 8px;
  z-index: 100;
}
.skip-link:focus {
  top: 0;
}
```

## 4. Analogi Rumah

| Konsep a11y | Analogi Rumah |
|-------------|---------------|
| Keyboard navigation | Pintu lebar untuk kursi roda |
| ARIA labels | Braille di tombol lift — label untuk screen reader |
| Skip navigation | Ramp untuk kursi roda — langsung ke ruang utama |
| Color contrast | Cat dinding kontras — mudah dibaca semua orang |
| Focus trap | Pagar pengaman di balkon — pengguna tidak "jatuh" keluar modal |
| Landmarks (`role`) | Plang ruangan — "Kamar Tidur", "Dapur", "Kamar Mandi" |

## 5. Use Case
- Situs pemerintah — wajib WCAG AA
- E-commerce — pengguna tunanetra belanja dengan screen reader
- Aplikasi perpustakaan — form pencarian pakai keyboard, navigasi tab
- Modal konfirmasi — focus trap agar keyboard user tidak terperangkap

## 6. Kesalahan Umum
- **Hanya andalkan ARIA tanpa semantic HTML** → lebih baik `<nav>` daripada `<div role="navigation">`. ARIA hanya tambahan.
- **Lupa aria-label pada icon button** → screen reader baca "button" tanpa konteks.
- **Color contrast terlalu rendah** → teks abu-abu muda di putih (rasio < 3:1). Gunakan alat kontras checker.

## 7. Benang Merah
PWA (128) yang offline harus tetap aksesibel. Testing (130) mencakup automated a11y testing. Micro-frontend (131) harus konsisten aksesibilitas antar fragment. Animasi (133) juga perlu reduced motion preference.

## 8. Soal

**Soal 1:** Sebutkan 4 prinsip WCAG 2.1.

**Soal 2:** Apa fungsi `aria-label` dan kapan harus digunakan?

**Soal 3:** Mengapa skip navigation penting dan bagaimana implementasinya?

---
<details>
<summary>Jawaban</summary>

**Jawaban 1:** Perceivable (dapat dilihat/didengar), Operable (dapat dioperasikan), Understandable (dapat dipahami), Robust (kompatibel teknologi bantu).

**Jawaban 2:** `aria-label` memberi label pada elemen yang tidak memiliki teks visual (misal tombol ikon). Contoh: `<button aria-label="Cari buku">🔍</button>`. Gunakan ketika label visual tidak ada atau tidak cukup deskriptif.

**Jawaban 3:** Skip navigation memungkinkan pengguna keyboard langsung ke konten utama tanpa melewati puluhan link navigasi. Implementasi: tautan tersembunyi di awal halaman yang muncul saat di-focus (`position: absolute; top: -40px`, saat `:focus` jadi `top: 0`).
</details>
