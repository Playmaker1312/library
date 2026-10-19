# 🗺️ Roadmap Project Vue.js: Step-by-Step Membangun Aplikasi Nyata

## Filosofi Roadmap Ini

> **"Belajar Vue.js dengan membangun satu aplikasi yang terus berkembang"** — setiap sesi menghasilkan fitur yang nyata dan bisa dilihat di browser. Kita tidak hafal API dulu baru praktik — kita **praktik sambil memahami mengapa setiap fitur ada**.

### Prinsip Desain

- **One Project, Growing**: satu aplikasi tumbuh dari sederhana ke kompleks — bukan project baru dari nol setiap level
- **Visible Progress**: setiap poin = sesuatu yang bisa dilihat di browser
- **Benang Merah Eksplisit**: setiap langkah terhubung ke langkah sebelum dan sesudahnya
- **TypeScript dari Awal**: bukan ditambahkan belakangan, tapi dibangun dari hari pertama

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: "BookStore UI" — Tampilan statis, fondasi Vue
    ↓ (enhance, tidak mulai dari nol)
Level 2: + Reaktivitas + Komponen + Interaktivitas
    ↓ (enhance)
Level 3: + Routing Multi-halaman + State Management (Pinia)
    ↓ (enhance)
Level 4: + Integrasi API (NestJS dari roadmap sebelumnya) + Auth
    ↓ (enhance)
Level 5: + Form Validasi + Upload + Fitur Advanced
    ↓ (enhance)
Level 6: + Testing + Optimasi Performa + TypeScript Penuh
    ↓ (enhance)
Level 7: + Deployment + CI/CD + Nuxt.js (SSR)
```

---

## 🟢 LEVEL 1: FONDASI — SETUP & TEMPLATE VUE (Minggu 1-2)

> **Tema**: _"Dari nol ke halaman Vue pertama yang berjalan di browser"_  
> **Benang Merah**: Setup project → Pahami struktur file `.vue` → Template syntax → Styling → Git  
> **Output**: Halaman katalog buku statis yang tampil di browser dengan Vue

---

### A. Inisialisasi Project

> 💡 **Mengapa dimulai di sini?** Keputusan setup di awal (Vite, TypeScript, Tailwind) mempengaruhi seluruh perjalanan. Kita setup semuanya di awal agar tidak refactor besar-besaran nanti.

text

```
Benang Merah Bagian A:
Tidak ada project → Scaffold dengan create-vue →
Pahami setiap file → Tambahkan Tailwind CSS →
Git dari hari pertama → Siap menulis komponen pertama
```

1. `[[1. Membuat Project Vue.js — create-vue dengan Vite & TypeScript]]`
    
    - Jalankan: `npm create vue@latest bookstore-ui`
    - Pilihan yang dipilih saat setup:
        
        text
        
        ```
        ✅ TypeScript         → Yes
        ✅ JSX Support        → No
        ✅ Vue Router         → Yes
        ✅ Pinia              → Yes
        ✅ Vitest             → Yes
        ✅ ESLint             → Yes
        ✅ Prettier           → Yes
        ```
        
    - Jalankan: `cd bookstore-ui && npm install && npm run dev`
    - Buka `http://localhost:5173` — halaman default Vue
    - _Langkah konkret_: Screenshot halaman default, commit: `feat: initialize Vue project with Vite and TypeScript`
2. `[[2. Memahami Setiap File yang Dibuat create-vue]]`
    
    - `src/main.ts`: entry point, mount Vue app ke `#app`
    - `src/App.vue`: root component
    - `src/router/index.ts`: konfigurasi Vue Router
    - `src/stores/`: folder Pinia stores
    - `src/components/`: folder komponen
    - `src/views/`: folder halaman/views
    - `vite.config.ts`: konfigurasi Vite
    - `tsconfig.json`: konfigurasi TypeScript
    - _Langkah konkret_: Hapus semua file contoh di `components/` dan `views/`, kita mulai bersih
3. `[[3. Bersihkan Project — Hapus Boilerplate & Siapkan Struktur]]`
    
    - Hapus isi `App.vue` menjadi minimal:
        
        vue
        
        ```
        <template>
          <RouterView />
        </template>
        ```
        
    - Hapus file contoh: `HelloWorld.vue`, `TheWelcome.vue`, dll
    - Hapus CSS default di `assets/main.css`
    - Buat struktur folder:
        
        text
        
        ```
        src/
        ├── assets/
        ├── components/
        │   ├── common/     ← komponen yang dipakai di mana-mana
        │   └── features/   ← komponen spesifik fitur
        ├── composables/    ← custom composables
        ├── router/
        ├── stores/
        ├── types/          ← TypeScript interfaces & types
        ├── utils/          ← helper functions
        └── views/
        ```
        
    - _Langkah konkret_: Verifikasi app masih berjalan setelah dibersihkan
4. `[[4. Setup Tailwind CSS — Styling Utility-First dari Awal]]`
    
    - Install: `npm install -D tailwindcss postcss autoprefixer`
    - Jalankan: `npx tailwindcss init -p`
    - Konfigurasi `tailwind.config.js`:
        
        JavaScript
        
        ```
        export default {
          content: ['./index.html', './src/**/*.{vue,js,ts,jsx,tsx}'],
          theme: {
            extend: {
              // custom colors, fonts, dll
            },
          },
        }
        ```
        
    - Update `src/assets/main.css`:
        
        CSS
        
        ```
        @tailwind base;
        @tailwind components;
        @tailwind utilities;
        ```
        
    - Import di `main.ts`: `import './assets/main.css'`
    - _Langkah konkret_: Tambahkan `class="text-blue-500 text-2xl"` di `App.vue`, verifikasi warna muncul
5. `[[5. Setup ESLint, Prettier & Path Alias]]`
    
    - Review konfigurasi ESLint yang sudah dibuat create-vue
    - Setup VS Code: `editor.formatOnSave: true`
    - Tambahkan path alias di `vite.config.ts`:
        
        TypeScript
        
        ```
        import path from 'path'
        
        export default defineConfig({
          resolve: {
            alias: {
              '@': path.resolve(__dirname, './src'),
            },
          },
        })
        ```
        
    - Update `tsconfig.json` untuk alias:
        
        JSON
        
        ```
        {
          "compilerOptions": {
            "paths": {
              "@/*": ["./src/*"]
            }
          }
        }
        ```
        
    - _Langkah konkret_: Import komponen dengan `@/components/...` bukan `../../components/...`
6. `[[6. Setup Git & Konvensi Commit]]`
    
    - Repository sudah di-init create-vue, setup remote ke GitHub
    - Install: `npm install -D husky lint-staged`
    - Setup husky: `npx husky init`
    - Konfigurasi `lint-staged` di `package.json`
    - Konvensi commit: `feat:`, `fix:`, `style:`, `chore:`
    - _Langkah konkret_: Push ke GitHub, verifikasi lint berjalan sebelum commit

---

### B. Memahami File `.vue` & Template Syntax

> 💡 **Benang Merah ke A**: Project sudah bersih dan siap. Sekarang kita pelajari cara kerja file `.vue` — ini adalah "bahasa" utama yang akan digunakan selama roadmap ini.

text

```
Benang Merah Bagian B:
Project siap (Poin 1-6) →
File .vue: template + script + style →
Template syntax: cara Vue merender HTML →
Buat halaman statis pertama →
Lihat hasilnya di browser
```

7. `[[7. Anatomi File .vue — Template, Script Setup & Style]]`
    
    - Struktur file `.vue`:
        
        vue
        
        ```
        <template>
          <!-- HTML dengan Vue template syntax -->
          <div class="container">
            <h1>BookStore</h1>
          </div>
        </template>
        
        <script setup lang="ts">
        // Composition API dengan TypeScript
        // Semua yang dideklarasikan di sini otomatis tersedia di template
        </script>
        
        <style scoped>
        /* CSS yang hanya berlaku untuk komponen ini */
        /* scoped: class tidak bocor ke komponen lain */
        </style>
        ```
        
    - Mengapa `<script setup>`: lebih ringkas dari Options API, lebih sedikit boilerplate
    - Mengapa `lang="ts"`: TypeScript dari awal
    - Mengapa `scoped`: mencegah konflik CSS antar komponen
    - _Langkah konkret_: Buat `src/views/HomeView.vue` dengan struktur di atas
8. `[[8. Template Syntax Dasar — Interpolasi & Direktif]]`
    
    - Interpolasi: `{{ variabel }}` — tampilkan nilai variabel
    - `v-bind:attr` atau `:attr` — bind atribut HTML ke data
    - `v-on:event` atau `@event` — handle event
    - Perbedaan `{{ }}` (text) vs `:attr` (attribute):
        
        vue
        
        ```
        <template>
          <!-- Interpolasi: nilai sebagai teks -->
          <p>{{ bookTitle }}</p>
          
          <!-- v-bind: nilai sebagai attribute -->
          <img :src="bookCover" :alt="bookTitle" />
          
          <!-- Tidak perlu kurung kurawal di dalam atribut -->
          <!-- SALAH: <img src="{{ bookCover }}" /> -->
        </template>
        
        <script setup lang="ts">
        const bookTitle = 'Clean Code'
        const bookCover = '/images/clean-code.jpg'
        </script>
        ```
        
    - _Langkah konkret_: Buat halaman dengan judul buku, penulis, dan gambar cover menggunakan interpolasi
9. `[[9. Membuat HomeView — Halaman Katalog Buku Statis]]`
    
    - Buat data statis sebagai array TypeScript:
        
        TypeScript
        
        ```
        interface Book {
          id: number
          title: string
          author: string
          cover: string
          price: number
          rating: number
        }
        
        const books: Book[] = [
          { id: 1, title: 'Clean Code', author: 'Robert Martin', cover: '...', price: 150000, rating: 4.8 },
          { id: 2, title: 'The Pragmatic Programmer', author: 'Hunt & Thomas', cover: '...', price: 175000, rating: 4.9 },
          // ...
        ]
        ```
        
    - Render daftar buku menggunakan `v-for`:
        
        vue
        
        ```
        <div v-for="book in books" :key="book.id" class="...">
          <img :src="book.cover" :alt="book.title" />
          <h3>{{ book.title }}</h3>
          <p>{{ book.author }}</p>
        </div>
        ```
        
    - _Langkah konkret_: Daftar 6 buku muncul di browser dengan styling Tailwind
10. `[[10. Conditional Rendering — v-if & v-show]]`
    
    - `v-if`: elemen tidak ada di DOM jika false
    - `v-show`: elemen ada di DOM tapi tersembunyi (display: none)
    - Kapan pakai `v-if` vs `v-show`:
        - `v-if`: kondisi jarang berubah, atau elemen berat
        - `v-show`: kondisi sering toggle, seperti dropdown
    - Tambahkan badge "Out of Stock" jika stok = 0:
        
        vue
        
        ```
        <span v-if="book.stock === 0" class="bg-red-100 text-red-800 text-xs px-2 py-1 rounded">
          Out of Stock
        </span>
        <span v-else class="bg-green-100 text-green-800 text-xs px-2 py-1 rounded">
          In Stock
        </span>
        ```
        
    - _Langkah konkret_: Beberapa buku tampilkan badge berbeda berdasarkan stok
11. `[[11. Class & Style Binding — Styling Dinamis]]`
    
    - Object syntax untuk class:
        
        vue
        
        ```
        <div
          :class="{
            'opacity-50': book.stock === 0,
            'ring-2 ring-blue-500': isSelected,
            'hover:shadow-lg': true
          }"
        >
        ```
        
    - Array syntax:
        
        vue
        
        ```
        <div :class="['p-4 rounded', book.featured ? 'bg-yellow-50' : 'bg-white']">
        ```
        
    - Inline style binding:
        
        vue
        
        ```
        <div :style="{ backgroundColor: book.color, fontSize: fontSize + 'px' }">
        ```
        
    - _Langkah konkret_: Buku dengan rating > 4.5 tampilkan dengan border emas

---

### C. Komponen Pertama — BookCard

> 💡 **Benang Merah ke B**: Kita sudah punya HomeView dengan daftar buku langsung di satu file. Masalahnya: kode akan makin panjang dan sulit dipelihara. Solusi: pecah menjadi komponen.

text

```
Benang Merah Bagian C:
HomeView panjang dan sulit dipelihara (Poin 9) →
Pecah menjadi komponen kecil →
BookCard: tampilkan satu buku →
Props: data buku dikirim dari HomeView ke BookCard →
HomeView lebih bersih: hanya list, detail di BookCard
```

12. `[[12. Membuat Komponen BookCard — Komponen Pertama]]`
    
    - Buat `src/components/features/BookCard.vue`:
        
        vue
        
        ```
        <template>
          <div class="bg-white rounded-xl shadow-md overflow-hidden hover:shadow-xl transition-shadow">
            <img :src="book.cover" :alt="book.title" class="w-full h-48 object-cover" />
            <div class="p-4">
              <h3 class="font-bold text-lg text-gray-800 line-clamp-2">{{ book.title }}</h3>
              <p class="text-gray-500 text-sm mt-1">{{ book.author }}</p>
              <div class="flex justify-between items-center mt-3">
                <span class="font-bold text-blue-600">
                  {{ formatPrice(book.price) }}
                </span>
                <span class="text-yellow-400 text-sm">
                  ⭐ {{ book.rating }}
                </span>
              </div>
            </div>
          </div>
        </template>
        
        <script setup lang="ts">
        import type { Book } from '@/types/book.types'
        
        const props = defineProps<{
          book: Book
        }>()
        
        const formatPrice = (price: number) => {
          return new Intl.NumberFormat('id-ID', {
            style: 'currency',
            currency: 'IDR',
          }).format(price)
        }
        </script>
        ```
        
    - Buat `src/types/book.types.ts` untuk interface `Book`
    - _Langkah konkret_: Komponen `BookCard` berdiri sendiri dan bisa dirender secara independen
13. `[[13. Props — Mengirim Data dari Parent ke Child]]`
    
    - `defineProps<T>()`: mendefinisikan props dengan TypeScript
    - Props validation: tipe, required, default value
    - Props dengan default value:
        
        TypeScript
        
        ```
        const props = withDefaults(
          defineProps<{
            book: Book
            showBadge?: boolean
            size?: 'sm' | 'md' | 'lg'
          }>(),
          {
            showBadge: true,
            size: 'md',
          }
        )
        ```
        
    - One-way data flow: props hanya dari parent → child, tidak boleh diubah dari child
    - _Langkah konkret_: Update `HomeView` untuk menggunakan `BookCard` dengan props
14. `[[14. Update HomeView — Gunakan Komponen BookCard]]`
    
    - Update `HomeView.vue`:
        
        vue
        
        ```
        <template>
          <div class="container mx-auto px-4 py-8">
            <h1 class="text-3xl font-bold text-gray-900 mb-8">Katalog Buku</h1>
            <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
              <BookCard
                v-for="book in books"
                :key="book.id"
                :book="book"
              />
            </div>
          </div>
        </template>
        
        <script setup lang="ts">
        import BookCard from '@/components/features/BookCard.vue'
        import type { Book } from '@/types/book.types'
        
        const books: Book[] = [ /* data buku */ ]
        </script>
        ```
        
    - HomeView sekarang jauh lebih bersih
    - _Langkah konkret_: Tampilan sama seperti sebelumnya, tapi kode lebih terstruktur
15. `[[15. Membuat Layout Component — AppHeader & AppFooter]]`
    
    - Buat `src/components/common/AppHeader.vue`:
        
        vue
        
        ```
        <template>
          <header class="bg-white shadow-sm sticky top-0 z-10">
            <div class="container mx-auto px-4 py-4 flex items-center justify-between">
              <RouterLink to="/" class="text-2xl font-bold text-blue-600">
                📚 BookStore
              </RouterLink>
              <nav class="flex gap-6">
                <RouterLink to="/" class="text-gray-600 hover:text-blue-600 transition-colors">
                  Beranda
                </RouterLink>
                <RouterLink to="/cart" class="text-gray-600 hover:text-blue-600 transition-colors">
                  Keranjang
                </RouterLink>
              </nav>
            </div>
          </header>
        </template>
        ```
        
    - Update `App.vue`:
        
        vue
        
        ```
        <template>
          <AppHeader />
          <main class="min-h-screen bg-gray-50">
            <RouterView />
          </main>
          <AppFooter />
        </template>
        ```
        
    - _Langkah konkret_: Header dan footer muncul di semua halaman

---

### 🏗️ Checkpoint Level 1

text

```
✅ Checklist sebelum lanjut ke Level 2:
├── Project Vue.js berjalan dengan Vite + TypeScript + Tailwind
├── Struktur folder yang rapi
├── File .vue dipahami (template, script setup, style scoped)
├── Template syntax: interpolasi, v-for, v-if/v-show
├── Class & style binding dinamis
├── Komponen BookCard dengan typed props
├── AppHeader dan AppFooter sebagai layout
├── Path alias @ berfungsi
├── Git history dengan commit yang rapi
└── README.md menjelaskan cara menjalankan project

Commit: feat: complete static book catalog with Vue components
```

---

## 🔵 LEVEL 2: REAKTIVITAS & INTERAKTIVITAS (Minggu 2-4)

> **Tema**: _"Dari tampilan statis ke aplikasi yang merespons interaksi user"_  
> **Benang Merah**: Data statis (Level 1) → data reaktif → user bisa berinteraksi → komponen berkomunikasi → aplikasi hidup  
> **Output**: Katalog buku dengan filter, search, counter keranjang, dan dark mode

---

### D. Reaktivitas — ref & reactive

> 💡 **Benang Merah ke Level 1**: Di Level 1, data buku adalah array biasa (`const books: Book[] = [...]`). Kalau kita ubah array ini, tampilan tidak akan berubah. `ref` dan `reactive` adalah "sinyal" yang memberi tahu Vue: "data berubah, tolong update tampilan".

text

```
Benang Merah Bagian D:
Data statis tidak memicu update UI (Level 1) →
ref(): data primitif yang reaktif →
reactive(): object yang reaktif →
Ubah data → Vue otomatis update tampilan →
computed(): nilai turunan yang reaktif
```

16. `[[16. ref() — Reaktivitas untuk Nilai Primitif]]`
    
    - `ref()` membungkus nilai dalam object reaktif:
        
        TypeScript
        
        ```
        import { ref } from 'vue'
        
        const count = ref(0)          // number
        const searchQuery = ref('')   // string
        const isLoading = ref(false)  // boolean
        const selectedBook = ref<Book | null>(null)  // typed ref
        
        // Di script: akses dengan .value
        count.value++
        searchQuery.value = 'clean code'
        
        // Di template: Vue otomatis unwrap, tidak perlu .value
        // {{ count }}  ← bukan {{ count.value }}
        ```
        
    - Tambahkan search input yang reaktif di HomeView:
        
        vue
        
        ```
        <input
          v-model="searchQuery"
          type="text"
          placeholder="Cari buku..."
          class="w-full px-4 py-2 border rounded-lg"
        />
        <p>Mencari: {{ searchQuery }}</p>
        ```
        
    - _Langkah konkret_: Ketik di input, teks di bawahnya berubah real-time
17. `[[17. v-model — Two-Way Binding yang Sesungguhnya]]`
    
    - `v-model` adalah shorthand untuk `:value` + `@input`:
        
        vue
        
        ```
        <!-- Ini: -->
        <input v-model="searchQuery" />
        
        <!-- Sama dengan ini: -->
        <input :value="searchQuery" @input="searchQuery = $event.target.value" />
        ```
        
    - `v-model` untuk berbagai tipe input:
        
        vue
        
        ```
        <!-- Text -->
        <input v-model="name" type="text" />
        
        <!-- Checkbox (boolean) -->
        <input v-model="isInStock" type="checkbox" />
        
        <!-- Radio (string) -->
        <input v-model="selectedCategory" type="radio" value="fiction" />
        <input v-model="selectedCategory" type="radio" value="non-fiction" />
        
        <!-- Select -->
        <select v-model="sortBy">
          <option value="title">Judul</option>
          <option value="price">Harga</option>
          <option value="rating">Rating</option>
        </select>
        ```
        
    - Modifiers: `.lazy` (update saat blur), `.number` (konversi ke number), `.trim` (hapus whitespace)
    - _Langkah konkret_: Tambahkan filter kategori dan sort dengan v-model
18. `[[18. computed() — Nilai yang Dihitung Otomatis]]`
    
    - `computed()` adalah nilai yang dihitung dari data reaktif lain:
        
        TypeScript
        
        ```
        import { ref, computed } from 'vue'
        
        const searchQuery = ref('')
        const selectedCategory = ref('all')
        const sortBy = ref('title')
        const books = ref<Book[]>([ /* data */ ])
        
        // computed secara otomatis re-calculate saat dependency berubah
        const filteredBooks = computed(() => {
          let result = books.value
          
          // Filter by search
          if (searchQuery.value) {
            const query = searchQuery.value.toLowerCase()
            result = result.filter(
              book =>
                book.title.toLowerCase().includes(query) ||
                book.author.toLowerCase().includes(query)
            )
          }
          
          // Filter by category
          if (selectedCategory.value !== 'all') {
            result = result.filter(book => book.category === selectedCategory.value)
          }
          
          // Sort
          return [...result].sort((a, b) => {
            if (sortBy.value === 'price') return a.price - b.price
            if (sortBy.value === 'rating') return b.rating - a.rating
            return a.title.localeCompare(b.title)
          })
        })
        
        // Gunakan di template:
        // v-for="book in filteredBooks"
        ```
        
    - Perbedaan `computed` vs `method`: computed di-cache, method dipanggil ulang setiap render
    - _Langkah konkret_: Search dan filter bekerja real-time, tampilkan jumlah hasil: `{{ filteredBooks.length }} buku ditemukan`
19. `[[19. reactive() — Reaktivitas untuk Object Kompleks]]`
    
    - `reactive()` untuk object yang punya banyak properti terkait:
        
        TypeScript
        
        ```
        import { reactive } from 'vue'
        
        // Daripada banyak ref:
        // const searchQuery = ref('')
        // const selectedCategory = ref('all')
        // const sortBy = ref('title')
        // const page = ref(1)
        
        // Gunakan reactive untuk state yang terkait:
        const filterState = reactive({
          search: '',
          category: 'all',
          sortBy: 'title',
          page: 1,
          limit: 12,
        })
        
        // Akses langsung tanpa .value
        filterState.search = 'clean code'
        filterState.page++
        ```
        
    - Kapan `ref` vs `reactive`:
        - `ref`: nilai tunggal, primitive, atau saat perlu pass/return dari composable
        - `reactive`: object yang propertinya saling terkait
    - _Langkah konkret_: Refactor filter state ke `reactive`, verifikasi masih berfungsi
20. `[[20. watch() — Bereaksi terhadap Perubahan Data]]`
    
    - `watch()` menjalankan side effect saat data berubah:
        
        TypeScript
        
        ```
        import { watch, ref } from 'vue'
        
        const searchQuery = ref('')
        
        // Watch satu ref
        watch(searchQuery, (newValue, oldValue) => {
          console.log(`Search berubah dari "${oldValue}" ke "${newValue}"`)
          // Misalnya: save ke URL params
        })
        
        // Watch dengan options
        watch(searchQuery, (newValue) => {
          // Reset halaman saat search berubah
          filterState.page = 1
        }, {
          immediate: false,  // false: tidak jalankan saat mount
          deep: false,       // false: tidak watch nested property
        })
        
        // Watch reactive object (butuh deep: true)
        watch(filterState, (newState) => {
          // Simpan filter ke localStorage
          localStorage.setItem('bookFilter', JSON.stringify(newState))
        }, { deep: true })
        ```
        
    - _Langkah konkret_: Reset halaman ke 1 saat search atau filter berubah
21. `[[21. Implementasi Dark Mode — State Global Pertama]]`
    
    - Dark mode sebagai pengenalan state yang perlu diakses dari mana-mana:
        
        TypeScript
        
        ```
        // src/composables/useDarkMode.ts
        import { ref, watchEffect } from 'vue'
        
        const isDark = ref(false)
        
        // watchEffect: jalankan segera dan watch dependency otomatis
        watchEffect(() => {
          document.documentElement.classList.toggle('dark', isDark.value)
          localStorage.setItem('darkMode', isDark.value.toString())
        })
        
        export function useDarkMode() {
          const toggle = () => { isDark.value = !isDark.value }
          return { isDark, toggle }
        }
        ```
        
    - Konfigurasi Tailwind untuk dark mode:
        
        JavaScript
        
        ```
        // tailwind.config.js
        export default {
          darkMode: 'class', // toggle via class di <html>
          // ...
        }
        ```
        
    - Tambahkan tombol toggle di `AppHeader`
    - _Langkah konkret_: Klik tombol → seluruh halaman berganti ke dark mode

---

### E. Komunikasi Antar Komponen

> 💡 **Benang Merah ke Komponen**: Di Level 1, `BookCard` hanya menerima data (props). Tapi bagaimana jika kita klik "Tambah ke Keranjang" di `BookCard`? Kita perlu cara bagi child untuk memberi tahu parent. Itulah fungsi `emit`.

text

```
Benang Merah Bagian E:
Props: parent → child (sudah ada, Poin 13) →
Emit: child → parent →
BookCard emit event "add-to-cart" →
HomeView mendengar dan update state keranjang →
Keranjang counter di header diupdate
```

22. `[[22. Emit — Child Berkomunikasi ke Parent]]`
    
    - Tambahkan tombol ke `BookCard` dan emit event:
        
        vue
        
        ```
        <template>
          <div class="...">
            <!-- ... konten card ... -->
            <button
              @click="handleAddToCart"
              :disabled="!book.inStock"
              class="w-full mt-3 bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 disabled:opacity-50 disabled:cursor-not-allowed transition-colors"
            >
              {{ book.inStock ? 'Tambah ke Keranjang' : 'Stok Habis' }}
            </button>
          </div>
        </template>
        
        <script setup lang="ts">
        import type { Book } from '@/types/book.types'
        
        const props = defineProps<{ book: Book }>()
        
        // Definisikan emit yang tersedia
        const emit = defineEmits<{
          'add-to-cart': [book: Book]
          'toggle-wishlist': [bookId: number]
        }>()
        
        function handleAddToCart() {
          emit('add-to-cart', props.book)
        }
        </script>
        ```
        
    - Listen di `HomeView`:
        
        vue
        
        ```
        <BookCard
          v-for="book in filteredBooks"
          :key="book.id"
          :book="book"
          @add-to-cart="handleAddToCart"
        />
        ```
        
    - _Langkah konkret_: Klik "Tambah ke Keranjang" di card → di-handle di HomeView
23. `[[23. Implementasi Cart State — Counter di Header]]`
    
    - Buat state keranjang di HomeView:
        
        TypeScript
        
        ```
        const cartItems = ref<CartItem[]>([])
        
        const cartCount = computed(() => 
          cartItems.value.reduce((total, item) => total + item.quantity, 0)
        )
        
        function handleAddToCart(book: Book) {
          const existingItem = cartItems.value.find(item => item.bookId === book.id)
          if (existingItem) {
            existingItem.quantity++
          } else {
            cartItems.value.push({ bookId: book.id, book, quantity: 1 })
          }
        }
        ```
        
    - Masalah: bagaimana `AppHeader` tahu jumlah item di cart?
    - Solusi sementara (akan diperbaiki dengan Pinia di Level 3): props drilling
    - _Langkah konkret_: Counter di header bertambah saat "Tambah ke Keranjang" diklik
24. `[[24. Slots — Komponen yang Fleksibel]]`
    
    - Buat komponen card generik dengan slot:
        
        vue
        
        ```
        <!-- src/components/common/BaseCard.vue -->
        <template>
          <div class="bg-white dark:bg-gray-800 rounded-xl shadow-md overflow-hidden">
            <!-- Header slot (opsional) -->
            <div v-if="$slots.header" class="border-b p-4">
              <slot name="header" />
            </div>
            
            <!-- Default slot -->
            <div class="p-4">
              <slot />
            </div>
            
            <!-- Footer slot (opsional) -->
            <div v-if="$slots.footer" class="border-t p-4 bg-gray-50">
              <slot name="footer" />
            </div>
          </div>
        </template>
        ```
        
    - Gunakan di `BookCard`:
        
        vue
        
        ```
        <BaseCard>
          <template #header>
            <img :src="book.cover" class="w-full h-48 object-cover" />
          </template>
          
          <!-- Default slot -->
          <h3>{{ book.title }}</h3>
          
          <template #footer>
            <button @click="handleAddToCart">Tambah ke Keranjang</button>
          </template>
        </BaseCard>
        ```
        
    - _Langkah konkret_: `BookCard` menggunakan `BaseCard` dengan slot yang fleksibel

---

### F. Lifecycle Hooks & Composables Pertama

> 💡 **Benang Merah ke Reaktivitas**: `ref` dan `computed` mengelola state. Tapi kapan kita harus fetch data? Kapan kita cleanup event listener? Lifecycle hooks menjawab pertanyaan "kapan".

text

```
Benang Merah Bagian F:
State reaktif (Poin 16-20) →
Kapan harus fetch data? → onMounted →
Bagaimana bungkus logika yang dipakai ulang? → composable →
useFetch: composable untuk fetching data →
useLocalStorage: composable untuk persist data
```

25. `[[25. Lifecycle Hooks — onMounted, onUnmounted & Lainnya]]`
    
    - Lifecycle hooks di Composition API:
        
        TypeScript
        
        ```
        import { onMounted, onUnmounted, onUpdated, ref } from 'vue'
        
        const count = ref(0)
        
        // Dijalankan setelah komponen ter-mount ke DOM
        onMounted(() => {
          console.log('Komponen sudah di DOM')
          // Aman untuk: akses DOM, fetch data, setup event listener
          document.title = 'BookStore - Beranda'
        })
        
        // Dijalankan saat komponen akan di-unmount
        onUnmounted(() => {
          // Cleanup: hapus event listener, batalkan timer
        })
        ```
        
    - Tambahkan `onMounted` untuk load data buku dari localStorage:
        
        TypeScript
        
        ```
        onMounted(() => {
          const saved = localStorage.getItem('bookFilter')
          if (saved) {
            Object.assign(filterState, JSON.parse(saved))
          }
        })
        ```
        
    - _Langkah konkret_: Filter tersimpan saat refresh halaman
26. `[[26. Membuat Composable Pertama — useLocalStorage]]`
    
    - Composable adalah fungsi yang menggunakan Composition API:
        
        TypeScript
        
        ```
        // src/composables/useLocalStorage.ts
        import { ref, watch } from 'vue'
        
        export function useLocalStorage<T>(key: string, defaultValue: T) {
          // Load dari localStorage, gunakan defaultValue jika tidak ada
          const stored = localStorage.getItem(key)
          const data = ref<T>(stored ? JSON.parse(stored) : defaultValue)
          
          // Simpan ke localStorage setiap kali data berubah
          watch(data, (newValue) => {
            localStorage.setItem(key, JSON.stringify(newValue))
          }, { deep: true })
          
          function remove() {
            localStorage.removeItem(key)
            data.value = defaultValue
          }
          
          return { data, remove }
        }
        ```
        
    - Gunakan di HomeView:
        
        TypeScript
        
        ```
        const { data: filterState } = useLocalStorage('bookFilter', {
          search: '',
          category: 'all',
          sortBy: 'title',
        })
        ```
        
    - _Langkah konkret_: Filter otomatis tersimpan dan diload dari localStorage
27. `[[27. Membuat Composable useBookFilter — Logika Filter Buku]]`
    
    - Pindahkan semua logika filter dari HomeView ke composable:
        
        TypeScript
        
        ```
        // src/composables/useBookFilter.ts
        import { computed } from 'vue'
        import { useLocalStorage } from './useLocalStorage'
        import type { Book } from '@/types/book.types'
        
        export function useBookFilter(books: Ref<Book[]>) {
          const { data: filter } = useLocalStorage('bookFilter', {
            search: '',
            category: 'all',
            sortBy: 'title',
            page: 1,
            limit: 12,
          })
          
          const filteredBooks = computed(() => {
            // ... logika filter yang sudah ada
          })
          
          const totalPages = computed(() => 
            Math.ceil(filteredBooks.value.length / filter.value.limit)
          )
          
          function resetFilters() {
            filter.value = { search: '', category: 'all', sortBy: 'title', page: 1, limit: 12 }
          }
          
          return { filter, filteredBooks, totalPages, resetFilters }
        }
        ```
        
    - HomeView menjadi sangat bersih:
        
        TypeScript
        
        ```
        const books = ref<Book[]>([ /* data */ ])
        const { filter, filteredBooks, totalPages, resetFilters } = useBookFilter(books)
        ```
        
    - _Langkah konkret_: HomeView jauh lebih kecil, semua logika filter di composable

---

### 🏗️ Checkpoint Level 2

text

```
✅ Checklist sebelum lanjut ke Level 3:
├── ref(), reactive(), computed() dipahami dan digunakan
├── v-model untuk semua tipe input
├── Search dan filter bekerja real-time dengan computed
├── Emit dari BookCard ke HomeView
├── Cart counter di header terupdate
├── Dark mode toggle berfungsi
├── Slots di BaseCard
├── onMounted untuk inisialisasi data
├── useLocalStorage composable
├── useBookFilter composable
└── HomeView lebih bersih berkat composables

Commit: feat: add reactivity, filtering, cart counter, and dark mode
```

---

## 🟡 LEVEL 3: ROUTING & STATE MANAGEMENT (Minggu 4-7)

> **Tema**: _"Dari satu halaman ke aplikasi multi-halaman dengan state terpusat"_  
> **Benang Merah**: Props drilling masalah (Level 2) → Pinia untuk state terpusat → Router untuk navigasi → Aplikasi terasa seperti aplikasi sungguhan  
> **Output**: BookStore dengan halaman beranda, detail buku, keranjang, dan wishlist — state konsisten di semua halaman

---

### G. Vue Router — Navigasi Multi-Halaman

> 💡 **Benang Merah ke Level 2**: Cart state di Level 2 hanya ada di `HomeView`. Saat user pindah ke halaman "Keranjang", data hilang. Kita butuh routing yang proper dan state yang persisten.

text

```
Benang Merah Bagian G:
Single page (Level 1-2) →
Butuh halaman: Beranda, Detail Buku, Keranjang, Profil →
Vue Router: definisikan URL untuk setiap halaman →
RouterLink: navigasi tanpa reload →
RouterView: tampilkan komponen sesuai URL →
Dynamic route: /books/:id untuk detail buku
```

28. `[[28. Review & Konfigurasi Router yang Sudah Ada]]`
    
    - Router sudah dibuat oleh create-vue, review `src/router/index.ts`
    - Tambahkan semua route yang dibutuhkan:
        
        TypeScript
        
        ```
        import { createRouter, createWebHistory } from 'vue-router'
        
        const router = createRouter({
          history: createWebHistory(import.meta.env.BASE_URL),
          routes: [
            {
              path: '/',
              name: 'home',
              component: () => import('@/views/HomeView.vue'),
            },
            {
              path: '/books/:id',
              name: 'book-detail',
              component: () => import('@/views/BookDetailView.vue'),
            },
            {
              path: '/cart',
              name: 'cart',
              component: () => import('@/views/CartView.vue'),
            },
            {
              path: '/wishlist',
              name: 'wishlist',
              component: () => import('@/views/WishlistView.vue'),
              meta: { requiresAuth: true }, // akan digunakan di Level 4
            },
            {
              path: '/profile',
              name: 'profile',
              component: () => import('@/views/ProfileView.vue'),
              meta: { requiresAuth: true },
            },
            {
              path: '/:pathMatch(.*)*',
              name: 'not-found',
              component: () => import('@/views/NotFoundView.vue'),
            },
          ],
          scrollBehavior(to, from, savedPosition) {
            if (savedPosition) return savedPosition
            return { top: 0, behavior: 'smooth' }
          },
        })
        ```
        
    - _Langkah konkret_: Semua route terdefinisi, navigasi ke URL yang tidak ada menampilkan 404
29. `[[29. Dynamic Routes — Halaman Detail Buku]]`
    
    - Buat `src/views/BookDetailView.vue`:
        
        vue
        
        ```
        <script setup lang="ts">
        import { ref, computed, onMounted } from 'vue'
        import { useRoute, useRouter } from 'vue-router'
        import type { Book } from '@/types/book.types'
        
        const route = useRoute()
        const router = useRouter()
        
        // Ambil parameter :id dari URL
        const bookId = computed(() => route.params.id as string)
        
        const book = ref<Book | null>(null)
        const isLoading = ref(true)
        
        onMounted(async () => {
          // Nanti akan diganti dengan API call
          // Sekarang cari dari data statis
          book.value = mockBooks.find(b => b.id === Number(bookId.value)) ?? null
          
          if (!book.value) {
            router.push({ name: 'not-found' })
            return
          }
          
          isLoading.value = false
        })
        </script>
        ```
        
    - _Langkah konkret_: Klik buku di HomeView → navigasi ke `/books/1` dengan detail buku
30. `[[30. RouterLink & Navigasi Programatik]]`
    
    - Update `BookCard` untuk navigasi ke detail:
        
        vue
        
        ```
        <!-- RouterLink: deklaratif -->
        <RouterLink :to="{ name: 'book-detail', params: { id: book.id } }">
          <img :src="book.cover" class="w-full h-48 object-cover cursor-pointer" />
        </RouterLink>
        ```
        
    - Navigasi programatik di tombol:
        
        TypeScript
        
        ```
        const router = useRouter()
        
        function goToDetail(bookId: number) {
          router.push({ name: 'book-detail', params: { id: bookId } })
        }
        
        function goBack() {
          router.back()
        }
        ```
        
    - _Langkah konkret_: Klik cover buku → navigasi ke detail. Tombol "Kembali" berfungsi
31. `[[31. Navigation Guards — Proteksi Route]]`
    
    - Global navigation guard untuk proteksi halaman yang butuh auth:
        
        TypeScript
        
        ```
        // Di router/index.ts
        router.beforeEach((to, from) => {
          const isAuthenticated = false // nanti dari Pinia store
          
          if (to.meta.requiresAuth && !isAuthenticated) {
            return {
              name: 'login',
              query: { redirect: to.fullPath }, // simpan URL tujuan
            }
          }
        })
        ```
        
    - _Langkah konkret_: Akses `/wishlist` → redirect ke `/login` (halaman login belum ada, tapi redirect berfungsi)
32. `[[32. Membuat NotFoundView & Transisi Halaman]]`
    
    - Buat `src/views/NotFoundView.vue` yang menarik:
        
        vue
        
        ```
        <template>
          <div class="flex flex-col items-center justify-center min-h-[60vh] text-center">
            <h1 class="text-9xl font-bold text-gray-200">404</h1>
            <p class="text-2xl text-gray-600 mt-4">Halaman tidak ditemukan</p>
            <RouterLink
              to="/"
              class="mt-8 bg-blue-600 text-white px-6 py-3 rounded-lg hover:bg-blue-700"
            >
              Kembali ke Beranda
            </RouterLink>
          </div>
        </template>
        ```
        
    - Tambahkan transisi antar halaman di `App.vue`:
        
        vue
        
        ```
        <RouterView v-slot="{ Component }">
          <Transition name="page" mode="out-in">
            <component :is="Component" />
          </Transition>
        </RouterView>
        ```
        
    - _Langkah konkret_: Navigasi antar halaman dengan animasi fade yang smooth

---

### H. Pinia — State Management Terpusat

> 💡 **Masalah yang Dipecahkan**: Cart state ada di `HomeView`, tapi `AppHeader` perlu tahu jumlah item. Kita bisa pass sebagai props, tapi semakin dalam komponen, semakin banyak "props drilling". Pinia menyimpan state di luar komponen — semua komponen bisa akses langsung.

text

```
Benang Merah Bagian H:
Props drilling untuk cart state (Level 2) →
Pinia: store di luar komponen →
useCartStore: akses dari komponen mana saja →
AppHeader: akses cartCount tanpa props →
BookDetailView: add to cart tanpa props drilling →
CartView: tampilkan dan kelola isi keranjang
```

33. `[[33. Membuat Cart Store — Pinia Store Pertama]]`
    
    - Buat `src/stores/cart.store.ts`:
        
        TypeScript
        
        ```
        import { defineStore } from 'pinia'
        import { ref, computed } from 'vue'
        import type { Book, CartItem } from '@/types/book.types'
        
        export const useCartStore = defineStore('cart', () => {
          // State
          const items = ref<CartItem[]>([])
          
          // Getters (computed)
          const totalItems = computed(() => 
            items.value.reduce((sum, item) => sum + item.quantity, 0)
          )
          
          const totalPrice = computed(() =>
            items.value.reduce((sum, item) => sum + item.book.price * item.quantity, 0)
          )
          
          const isEmpty = computed(() => items.value.length === 0)
          
          // Actions
          function addItem(book: Book) {
            const existing = items.value.find(item => item.bookId === book.id)
            if (existing) {
              existing.quantity++
            } else {
              items.value.push({ bookId: book.id, book, quantity: 1 })
            }
          }
          
          function removeItem(bookId: number) {
            const index = items.value.findIndex(item => item.bookId === bookId)
            if (index > -1) items.value.splice(index, 1)
          }
          
          function updateQuantity(bookId: number, quantity: number) {
            const item = items.value.find(item => item.bookId === bookId)
            if (item) {
              if (quantity <= 0) {
                removeItem(bookId)
              } else {
                item.quantity = quantity
              }
            }
          }
          
          function clearCart() {
            items.value = []
          }
          
          return { items, totalItems, totalPrice, isEmpty, addItem, removeItem, updateQuantity, clearCart }
        })
        ```
        
    - _Langkah konkret_: Store terdefinisi, siap digunakan dari komponen mana saja
34. `[[34. Menggunakan Cart Store — Dari Berbagai Komponen]]`
    
    - Di `AppHeader.vue` — counter keranjang:
        
        vue
        
        ```
        <script setup lang="ts">
        import { useCartStore } from '@/stores/cart.store'
        
        const cart = useCartStore()
        // Langsung akses cart.totalItems tanpa props!
        </script>
        
        <template>
          <RouterLink to="/cart" class="relative">
            🛒
            <span
              v-if="cart.totalItems > 0"
              class="absolute -top-2 -right-2 bg-red-500 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center"
            >
              {{ cart.totalItems }}
            </span>
          </RouterLink>
        </template>
        ```
        
    - Di `BookCard.vue` — tambah ke keranjang:
        
        TypeScript
        
        ```
        const cart = useCartStore()
        
        function handleAddToCart() {
          cart.addItem(props.book)
          // Tampilkan toast notifikasi (nanti)
        }
        ```
        
    - _Langkah konkret_: Klik "Tambah ke Keranjang" dari mana saja → counter di header berubah
35. `[[35. Membuat CartView — Halaman Keranjang Belanja]]`
    
    - Buat `src/views/CartView.vue`:
        
        vue
        
        ```
        <template>
          <div class="container mx-auto px-4 py-8">
            <h1 class="text-2xl font-bold mb-6">Keranjang Belanja</h1>
            
            <!-- Empty State -->
            <div v-if="cart.isEmpty" class="text-center py-16">
              <p class="text-6xl mb-4">🛒</p>
              <p class="text-gray-500 text-lg">Keranjang kamu kosong</p>
              <RouterLink to="/" class="mt-4 inline-block bg-blue-600 text-white px-6 py-2 rounded-lg">
                Belanja Sekarang
              </RouterLink>
            </div>
            
            <!-- Cart Items -->
            <div v-else class="grid grid-cols-1 lg:grid-cols-3 gap-8">
              <div class="lg:col-span-2 space-y-4">
                <CartItem
                  v-for="item in cart.items"
                  :key="item.bookId"
                  :item="item"
                  @update-quantity="cart.updateQuantity(item.bookId, $event)"
                  @remove="cart.removeItem(item.bookId)"
                />
              </div>
              <OrderSummary :total="cart.totalPrice" />
            </div>
          </div>
        </template>
        ```
        
    - _Langkah konkret_: Halaman cart menampilkan semua item dengan total harga
36. `[[36. Persist Cart Store — Data Tersimpan setelah Refresh]]`
    
    - Install: `npm install pinia-plugin-persistedstate`
    - Setup di `main.ts`:
        
        TypeScript
        
        ```
        import { createPinia } from 'pinia'
        import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'
        
        const pinia = createPinia()
        pinia.use(piniaPluginPersistedstate)
        ```
        
    - Update cart store:
        
        TypeScript
        
        ```
        export const useCartStore = defineStore('cart', () => {
          // ...state dan actions...
        }, {
          persist: {
            key: 'bookstore-cart',
            storage: localStorage,
            // Hanya persist items, bukan computed
            pick: ['items'],
          },
        })
        ```
        
    - _Langkah konkret_: Tambah buku ke cart, refresh halaman — cart tetap terisi
37. `[[37. Membuat Wishlist Store — Store Kedua]]`
    
    - Buat `src/stores/wishlist.store.ts` dengan pola yang sama
    - Toggle wishlist (tambah/hapus):
        
        TypeScript
        
        ```
        function toggleWishlist(book: Book) {
          const index = items.value.findIndex(b => b.id === book.id)
          if (index > -1) {
            items.value.splice(index, 1)
          } else {
            items.value.push(book)
          }
        }
        
        function isInWishlist(bookId: number) {
          return items.value.some(b => b.id === bookId)
        }
        ```
        
    - Tambahkan tombol wishlist (❤️) di `BookCard`
    - _Langkah konkret_: Klik ❤️ di card → ikon berubah, counter wishlist di header berubah

---

### 🏗️ Checkpoint Level 3

text

```
✅ Checklist sebelum lanjut ke Level 4:
├── Vue Router dengan semua route terdefinisi
├── Dynamic route /books/:id untuk detail buku
├── RouterLink dan navigasi programatik
├── Navigation guard untuk route yang butuh auth
├── Transisi halaman dengan Transition
├── NotFoundView (404)
├── Cart store dengan semua operasi CRUD
├── Wishlist store
├── Cart count di header terupdate dari store
├── CartView yang lengkap
├── Persist cart dan wishlist ke localStorage
└── Semua halaman dapat diakses dari navigation

Commit: feat: add multi-page routing and Pinia state management
```

---

## 🟠 LEVEL 4: INTEGRASI API & AUTENTIKASI (Minggu 7-12)

> **Tema**: _"Dari data statis ke data dari server — koneksi ke backend nyata"_  
> **Benang Merah**: Data hardcode (Level 1-3) → API NestJS → fetch real data → Auth login/register → User-specific features  
> **Output**: BookStore terhubung ke backend NestJS dengan auth JWT penuh

---

### I. HTTP & API Integration

> 💡 **Benang Merah ke Data Statis**: Selama ini data buku di-hardcode di JavaScript. Di dunia nyata, data dari database via API. Kita akan connect ke backend NestJS yang dibuat di roadmap sebelumnya.

text

```
Benang Merah Bagian I:
Data hardcode (Level 1-3) →
Axios: HTTP client yang powerful →
Axios instance: base URL + interceptor →
useApi composable: abstraksi fetch data →
Loading dan error state →
Ganti semua data statis dengan API call
```

38. `[[38. Setup Axios — HTTP Client Konfigurasi]]`
    
    - Install: `npm install axios`
    - Buat `src/api/axios.instance.ts`:
        
        TypeScript
        
        ```
        import axios from 'axios'
        
        const apiClient = axios.create({
          baseURL: import.meta.env.VITE_API_BASE_URL || 'http://localhost:3000/api/v1',
          timeout: 10000,
          headers: {
            'Content-Type': 'application/json',
          },
        })
        
        // Request interceptor: tambahkan token ke setiap request
        apiClient.interceptors.request.use((config) => {
          const token = localStorage.getItem('accessToken')
          if (token) {
            config.headers.Authorization = `Bearer ${token}`
          }
          return config
        })
        
        // Response interceptor: handle error secara terpusat
        apiClient.interceptors.response.use(
          (response) => response.data, // langsung return data
          async (error) => {
            if (error.response?.status === 401) {
              // Token expired, coba refresh (diimplementasikan nanti)
            }
            return Promise.reject(error)
          }
        )
        
        export default apiClient
        ```
        
    - Buat `src/api/` folder dengan file per resource:
        
        text
        
        ```
        src/api/
        ├── axios.instance.ts
        ├── books.api.ts
        ├── auth.api.ts
        ├── cart.api.ts
        └── users.api.ts
        ```
        
    - _Langkah konkret_: Axios instance siap dengan base URL dari environment variable
39. `[[39. Membuat Books API — Semua Endpoint Buku]]`
    
    - Buat `src/api/books.api.ts`:
        
        TypeScript
        
        ```
        import apiClient from './axios.instance'
        import type { Book, PaginatedResult, QueryParams } from '@/types'
        
        export const booksApi = {
          getAll: (params?: QueryParams): Promise<PaginatedResult<Book>> =>
            apiClient.get('/books', { params }),
          
          getById: (id: string): Promise<Book> =>
            apiClient.get(`/books/${id}`),
          
          search: (query: string): Promise<Book[]> =>
            apiClient.get('/books', { params: { search: query } }),
        }
        ```
        
    - _Langkah konkret_: Test di browser console: `booksApi.getAll()` mengembalikan data dari server
40. `[[40. Membuat useApi Composable — Fetch dengan Loading & Error State]]`
    
    - Buat `src/composables/useApi.ts`:
        
        TypeScript
        
        ```
        import { ref } from 'vue'
        
        export function useApi<T>() {
          const data = ref<T | null>(null)
          const isLoading = ref(false)
          const error = ref<string | null>(null)
          
          async function execute(apiCall: () => Promise<T>) {
            isLoading.value = true
            error.value = null
            
            try {
              data.value = await apiCall()
            } catch (err: any) {
              error.value = err.response?.data?.message || 'Terjadi kesalahan'
              console.error(err)
            } finally {
              isLoading.value = false
            }
          }
          
          return { data, isLoading, error, execute }
        }
        ```
        
    - _Langkah konkret_: Gunakan di komponen: `const { data: books, isLoading, execute } = useApi<PaginatedResult<Book>>()`
41. `[[41. Update HomeView — Fetch Buku dari API]]`
    
    - Ganti data statis dengan API call:
        
        TypeScript
        
        ```
        const booksApi = useBooksApi()
        const { data: booksData, isLoading, error, execute } = useApi<PaginatedResult<Book>>()
        
        onMounted(async () => {
          await execute(() => booksApi.getAll(filter.value))
        })
        
        // Re-fetch saat filter berubah
        watch(filter, async () => {
          await execute(() => booksApi.getAll(filter.value))
        }, { deep: true })
        ```
        
    - Tambahkan loading skeleton:
        
        vue
        
        ```
        <template>
          <!-- Loading Skeleton -->
          <div v-if="isLoading" class="grid grid-cols-4 gap-6">
            <div
              v-for="n in 8"
              :key="n"
              class="bg-gray-200 rounded-xl h-72 animate-pulse"
            />
          </div>
          
          <!-- Error State -->
          <div v-else-if="error" class="text-center text-red-500 py-16">
            {{ error }}
            <button @click="execute(...)" class="block mt-4 ...">Coba Lagi</button>
          </div>
          
          <!-- Data -->
          <div v-else class="grid grid-cols-4 gap-6">
            <BookCard v-for="book in booksData?.data" :key="book.id" :book="book" />
          </div>
        </template>
        ```
        
    - _Langkah konkret_: Data buku muncul dari backend NestJS, loading skeleton saat fetch
42. `[[42. Update BookDetailView — Fetch Detail Buku dari API]]`
    
    - Fetch buku berdasarkan ID dari route params:
        
        TypeScript
        
        ```
        const route = useRoute()
        const { data: book, isLoading, execute } = useApi<Book>()
        
        onMounted(async () => {
          const id = route.params.id as string
          await execute(() => booksApi.getById(id))
        })
        
        // Watch jika user navigasi dari detail buku ke detail buku lain
        watch(() => route.params.id, async (newId) => {
          if (newId) {
            await execute(() => booksApi.getById(newId as string))
          }
        })
        ```
        
    - _Langkah konkret_: Detail buku menampilkan data lengkap dari API

---

### J. Autentikasi — Login, Register & JWT

> 💡 **Benang Merah ke Router Guards**: Di Poin 31, kita buat `meta: { requiresAuth: true }` dan guard yang cek `isAuthenticated`. Sekarang kita implementasikan auth yang sesungguhnya — user bisa login, token disimpan, dan guard menggunakan token nyata.

text

```
Benang Merah Bagian J:
Route guard dengan isAuthenticated palsu (Poin 31) →
Auth store: simpan user dan token →
Login/Register page →
Interceptor: kirim token di setiap request →
Guard: cek token di auth store →
Refresh token: perpanjang session otomatis
```

43. `[[43. Membuat Auth Store — User & Token Management]]`
    
    - Buat `src/stores/auth.store.ts`:
        
        TypeScript
        
        ```
        import { defineStore } from 'pinia'
        import { ref, computed } from 'vue'
        import type { User } from '@/types/user.types'
        
        export const useAuthStore = defineStore('auth', () => {
          const user = ref<User | null>(null)
          const accessToken = ref<string | null>(null)
          const refreshToken = ref<string | null>(null)
          
          const isAuthenticated = computed(() => !!accessToken.value)
          const isAdmin = computed(() => user.value?.role === 'ADMIN')
          
          function setAuth(data: { user: User; accessToken: string; refreshToken: string }) {
            user.value = data.user
            accessToken.value = data.accessToken
            refreshToken.value = data.refreshToken
          }
          
          function clearAuth() {
            user.value = null
            accessToken.value = null
            refreshToken.value = null
          }
          
          return { user, accessToken, refreshToken, isAuthenticated, isAdmin, setAuth, clearAuth }
        }, {
          persist: {
            key: 'bookstore-auth',
            storage: localStorage,
            pick: ['accessToken', 'refreshToken', 'user'],
          },
        })
        ```
        
    - _Langkah konkret_: Auth store siap, isAuthenticated bisa digunakan di router guard
44. `[[44. Membuat Halaman Login & Register]]`
    
    - Buat `src/views/LoginView.vue`:
        
        vue
        
        ```
        <template>
          <div class="min-h-screen flex items-center justify-center bg-gray-50">
            <div class="max-w-md w-full bg-white rounded-2xl shadow-lg p-8">
              <h1 class="text-2xl font-bold text-center mb-8">Masuk ke BookStore</h1>
              
              <form @submit.prevent="handleLogin" class="space-y-4">
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">Email</label>
                  <input
                    v-model="form.email"
                    type="email"
                    required
                    class="w-full border rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
                    placeholder="email@contoh.com"
                  />
                </div>
                
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">Password</label>
                  <input
                    v-model="form.password"
                    type="password"
                    required
                    class="w-full border rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
                  />
                </div>
                
                <div v-if="error" class="bg-red-50 text-red-600 text-sm px-4 py-3 rounded-lg">
                  {{ error }}
                </div>
                
                <button
                  type="submit"
                  :disabled="isLoading"
                  class="w-full bg-blue-600 text-white py-3 rounded-lg hover:bg-blue-700 disabled:opacity-50 transition-colors"
                >
                  {{ isLoading ? 'Memproses...' : 'Masuk' }}
                </button>
              </form>
              
              <p class="text-center text-sm text-gray-500 mt-6">
                Belum punya akun?
                <RouterLink to="/register" class="text-blue-600 hover:underline">
                  Daftar sekarang
                </RouterLink>
              </p>
            </div>
          </div>
        </template>
        
        <script setup lang="ts">
        import { reactive, ref } from 'vue'
        import { useRouter, useRoute } from 'vue-router'
        import { useAuthStore } from '@/stores/auth.store'
        import { authApi } from '@/api/auth.api'
        
        const router = useRouter()
        const route = useRoute()
        const auth = useAuthStore()
        
        const form = reactive({ email: '', password: '' })
        const isLoading = ref(false)
        const error = ref('')
        
        async function handleLogin() {
          isLoading.value = true
          error.value = ''
          
          try {
            const response = await authApi.login(form)
            auth.setAuth(response.data)
            
            // Redirect ke halaman yang dituju sebelum login
            const redirect = route.query.redirect as string || '/'
            router.push(redirect)
          } catch (err: any) {
            error.value = err.response?.data?.message || 'Login gagal'
          } finally {
            isLoading.value = false
          }
        }
        </script>
        ```
        
    - _Langkah konkret_: Login dengan akun yang dibuat di backend → redirect ke beranda
45. `[[45. Update Router Guard — Gunakan Auth Store yang Nyata]]`
    
    - Update guard di `router/index.ts`:
        
        TypeScript
        
        ```
        router.beforeEach((to) => {
          const auth = useAuthStore()
          
          if (to.meta.requiresAuth && !auth.isAuthenticated) {
            return {
              name: 'login',
              query: { redirect: to.fullPath },
            }
          }
          
          // Redirect ke home jika sudah login tapi akses halaman auth
          if (to.meta.guestOnly && auth.isAuthenticated) {
            return { name: 'home' }
          }
        })
        ```
        
    - Tambahkan `meta: { guestOnly: true }` ke route login dan register
    - _Langkah konkret_: User yang sudah login tidak bisa akses /login (redirect ke home)
46. `[[46. Refresh Token — Session yang Tidak Mudah Expired]]`
    
    - Update Axios response interceptor untuk handle 401:
        
        TypeScript
        
        ```
        let isRefreshing = false
        let failedQueue: Array<{ resolve: Function; reject: Function }> = []
        
        apiClient.interceptors.response.use(
          (response) => response.data,
          async (error) => {
            const originalRequest = error.config
            
            if (error.response?.status === 401 && !originalRequest._retry) {
              if (isRefreshing) {
                // Jika sedang refresh, antri request ini
                return new Promise((resolve, reject) => {
                  failedQueue.push({ resolve, reject })
                })
              }
              
              originalRequest._retry = true
              isRefreshing = true
              
              try {
                const auth = useAuthStore()
                const response = await authApi.refresh(auth.refreshToken!)
                auth.setAuth(response.data)
                
                // Proses queue
                failedQueue.forEach(({ resolve }) => resolve())
                failedQueue = []
                
                // Retry original request
                return apiClient(originalRequest)
              } catch {
                failedQueue.forEach(({ reject }) => reject())
                failedQueue = []
                
                // Refresh gagal: logout
                useAuthStore().clearAuth()
                router.push({ name: 'login' })
              } finally {
                isRefreshing = false
              }
            }
            
            return Promise.reject(error)
          }
        )
        ```
        
    - _Langkah konkret_: Access token expired → otomatis refresh → request dilanjutkan tanpa user tahu
47. `[[47. Update Header — User Menu & Logout]]`
    
    - Update `AppHeader.vue` untuk tampilkan user yang login:
        
        vue
        
        ```
        <template>
          <header class="...">
            <div class="... flex items-center justify-between">
              <!-- Logo -->
              <RouterLink to="/" class="...">📚 BookStore</RouterLink>
              
              <div class="flex items-center gap-4">
                <!-- Cart -->
                <RouterLink to="/cart" class="relative">...</RouterLink>
                
                <!-- Auth Section -->
                <template v-if="auth.isAuthenticated">
                  <!-- User Dropdown -->
                  <div class="relative" ref="dropdownRef">
                    <button @click="isDropdownOpen = !isDropdownOpen" class="flex items-center gap-2">
                      <img
                        :src="auth.user?.avatar || '/default-avatar.png'"
                        class="w-8 h-8 rounded-full object-cover"
                      />
                      <span class="hidden md:block">{{ auth.user?.name }}</span>
                    </button>
                    
                    <div v-if="isDropdownOpen" class="absolute right-0 mt-2 w-48 bg-white rounded-lg shadow-lg">
                      <RouterLink to="/profile" class="block px-4 py-2 hover:bg-gray-50">Profil</RouterLink>
                      <RouterLink to="/wishlist" class="block px-4 py-2 hover:bg-gray-50">Wishlist</RouterLink>
                      <button @click="handleLogout" class="block w-full text-left px-4 py-2 text-red-600 hover:bg-red-50">
                        Keluar
                      </button>
                    </div>
                  </div>
                </template>
                
                <template v-else>
                  <RouterLink to="/login" class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700">
                    Masuk
                  </RouterLink>
                </template>
              </div>
            </div>
          </header>
        </template>
        ```
        
    - _Langkah konkret_: Header tampilkan avatar user atau tombol Login berdasarkan status auth

---

### 🏗️ Checkpoint Level 4

text

```
✅ Checklist sebelum lanjut ke Level 5:
├── Axios instance dengan interceptor
├── Books API terhubung ke backend NestJS
├── useApi composable dengan loading dan error state
├── HomeView fetch data dari API dengan loading skeleton
├── BookDetailView fetch detail dari API
├── Auth store dengan persist
├── LoginView dan RegisterView
├── Router guard menggunakan auth store nyata
├── Refresh token otomatis
├── User menu di header (login/logout)
└── Redirect setelah login ke halaman yang dituju

Commit: feat: integrate with NestJS backend API and add authentication
```

---

## 🔴 LEVEL 5: FORM VALIDASI & FITUR ADVANCED (Minggu 12-18)

> **Tema**: _"Form yang robust dan fitur-fitur yang membuat aplikasi terasa profesional"_  
> **Benang Merah**: API sudah terhubung (Level 4) → user perlu input data → validasi form → upload file → komponen advanced  
> **Output**: Checkout flow lengkap, form register dengan validasi, upload avatar, toast notification

---

### K. Form Validasi dengan VeeValidate & Zod

> 💡 **Benang Merah ke Form Login**: Form login di Level 4 menggunakan validasi HTML biasa (`required`, `type="email"`). Ini tidak cukup untuk form kompleks. VeeValidate + Zod memberikan validasi yang powerful dan type-safe.

text

```
Benang Merah Bagian K:
Form login sederhana (Poin 44) →
VeeValidate: manajemen form state →
Zod: definisikan schema validasi →
Field-level error: tampilkan error di bawah input →
Form tidak bisa submit jika ada error →
Refactor form login menggunakan VeeValidate
```

48. `[[48. Setup VeeValidate & Zod — Validasi Form yang Proper]]`
    
    - Install: `npm install vee-validate @vee-validate/zod zod`
    - Definisikan schema dengan Zod:
        
        TypeScript
        
        ```
        // src/schemas/auth.schema.ts
        import { z } from 'zod'
        
        export const loginSchema = z.object({
          email: z
            .string()
            .min(1, 'Email wajib diisi')
            .email('Format email tidak valid'),
          password: z
            .string()
            .min(1, 'Password wajib diisi')
            .min(8, 'Password minimal 8 karakter'),
        })
        
        export const registerSchema = z.object({
          name: z.string().min(1, 'Nama wajib diisi').min(2, 'Nama minimal 2 karakter'),
          email: z.string().min(1, 'Email wajib diisi').email('Format email tidak valid'),
          password: z
            .string()
            .min(8, 'Password minimal 8 karakter')
            .regex(/[A-Z]/, 'Harus ada huruf kapital')
            .regex(/[0-9]/, 'Harus ada angka'),
          confirmPassword: z.string(),
        }).refine(
          data => data.password === data.confirmPassword,
          {
            message: 'Password tidak sama',
            path: ['confirmPassword'],
          }
        )
        
        export type LoginFormData = z.infer<typeof loginSchema>
        export type RegisterFormData = z.infer<typeof registerSchema>
        ```
        
    - _Langkah konkret_: Schema terdefinisi dan type-safe
49. `[[49. Membuat FormField Component — Reusable Input dengan Error]]`
    
    - Buat `src/components/common/FormField.vue`:
        
        vue
        
        ```
        <template>
          <div class="space-y-1">
            <label v-if="label" :for="fieldId" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
              {{ label }}
              <span v-if="required" class="text-red-500 ml-1">*</span>
            </label>
            
            <input
              :id="fieldId"
              v-bind="$attrs"
              :value="value"
              @input="$emit('update:modelValue', ($event.target as HTMLInputElement).value)"
              :class="[
                'w-full border rounded-lg px-3 py-2 transition-colors',
                'focus:outline-none focus:ring-2',
                errorMessage
                  ? 'border-red-500 focus:ring-red-200'
                  : 'border-gray-300 focus:ring-blue-200 focus:border-blue-500',
              ]"
            />
            
            <p v-if="errorMessage" class="text-red-500 text-sm flex items-center gap-1">
              <span>⚠️</span> {{ errorMessage }}
            </p>
          </div>
        </template>
        
        <script setup lang="ts">
        import { computed } from 'vue'
        import { useId } from 'vue'
        
        const props = withDefaults(
          defineProps<{
            label?: string
            modelValue?: string
            errorMessage?: string
            required?: boolean
          }>(),
          { required: false }
        )
        
        defineEmits<{ 'update:modelValue': [value: string] }>()
        defineOptions({ inheritAttrs: false })
        
        const fieldId = useId()
        const value = computed(() => props.modelValue ?? '')
        </script>
        ```
        
    - _Langkah konkret_: FormField menampilkan error dengan styling merah
50. `[[50. Refactor LoginView — Menggunakan VeeValidate & Zod]]`
    
    - Update `LoginView.vue` dengan VeeValidate:
        
        vue
        
        ```
        <script setup lang="ts">
        import { useForm } from 'vee-validate'
        import { toTypedSchema } from '@vee-validate/zod'
        import { loginSchema, type LoginFormData } from '@/schemas/auth.schema'
        
        const { handleSubmit, isSubmitting, errors, defineField } = useForm<LoginFormData>({
          validationSchema: toTypedSchema(loginSchema),
        })
        
        const [email, emailAttrs] = defineField('email')
        const [password, passwordAttrs] = defineField('password')
        
        const onSubmit = handleSubmit(async (values) => {
          try {
            const response = await authApi.login(values)
            auth.setAuth(response.data)
            router.push(redirect || '/')
          } catch (err: any) {
            setErrors({ email: err.response?.data?.message })
          }
        })
        </script>
        
        <template>
          <form @submit="onSubmit">
            <FormField
              label="Email"
              v-model="email"
              v-bind="emailAttrs"
              type="email"
              :error-message="errors.email"
              required
            />
            
            <FormField
              label="Password"
              v-model="password"
              v-bind="passwordAttrs"
              type="password"
              :error-message="errors.password"
              required
            />
            
            <button type="submit" :disabled="isSubmitting">
              {{ isSubmitting ? 'Memproses...' : 'Masuk' }}
            </button>
          </form>
        </template>
        ```
        
    - _Langkah konkret_: Submit form kosong → error muncul di bawah setiap field
51. `[[51. Checkout Form — Form Multi-Step yang Kompleks]]`
    
    - Buat `src/views/CheckoutView.vue` dengan 3 langkah:
        - Step 1: Informasi pengiriman
        - Step 2: Metode pembayaran
        - Step 3: Konfirmasi pesanan
    - State management antar step:
        
        TypeScript
        
        ```
        const currentStep = ref(1)
        const totalSteps = 3
        
        const checkoutData = reactive({
          shipping: {
            name: '',
            address: '',
            city: '',
            phone: '',
          },
          payment: {
            method: 'transfer', // 'transfer' | 'cod' | 'ewallet'
          },
        })
        
        function nextStep() {
          if (currentStep.value < totalSteps) currentStep.value++
        }
        
        function prevStep() {
          if (currentStep.value > 1) currentStep.value--
        }
        ```
        
    - Progress indicator di atas form
    - _Langkah konkret_: Form 3 langkah dengan navigasi prev/next dan progress bar

---

### L. Komponen Advanced & Fitur UI

> 💡 **Benang Merah ke Komponen**: Semua komponen yang sudah dibuat menggunakan pola dasar. Sekarang kita buat komponen yang lebih advanced: modal, toast, infinite scroll.

text

```
Benang Merah Bagian L:
Komponen dasar (Level 1-4) →
Toast: notifikasi yang muncul dan menghilang →
Modal: dialog konfirmasi →
Infinite Scroll: load lebih banyak data →
Teleport: render di luar komponen parent →
Async Component: load komponen secara lazy
```

52. `[[52. Toast Notification System — Feedback untuk User]]`
    
    - Buat `src/composables/useToast.ts`:
        
        TypeScript
        
        ```
        import { ref } from 'vue'
        
        interface Toast {
          id: string
          type: 'success' | 'error' | 'info' | 'warning'
          message: string
          duration: number
        }
        
        const toasts = ref<Toast[]>([])
        
        export function useToast() {
          function show(message: string, type: Toast['type'] = 'info', duration = 3000) {
            const id = Date.now().toString()
            toasts.value.push({ id, type, message, duration })
            
            setTimeout(() => {
              dismiss(id)
            }, duration)
          }
          
          function dismiss(id: string) {
            const index = toasts.value.findIndex(t => t.id === id)
            if (index > -1) toasts.value.splice(index, 1)
          }
          
          return {
            toasts,
            success: (msg: string) => show(msg, 'success'),
            error: (msg: string) => show(msg, 'error'),
            info: (msg: string) => show(msg, 'info'),
            warning: (msg: string) => show(msg, 'warning'),
            dismiss,
          }
        }
        ```
        
    - Buat `src/components/common/ToastContainer.vue` menggunakan `<Teleport to="body">`:
        
        vue
        
        ```
        <Teleport to="body">
          <div class="fixed bottom-4 right-4 z-50 space-y-2">
            <TransitionGroup name="toast">
              <div
                v-for="toast in toasts"
                :key="toast.id"
                :class="['px-4 py-3 rounded-lg shadow-lg text-white flex items-center gap-2 min-w-72', {
                  'bg-green-500': toast.type === 'success',
                  'bg-red-500': toast.type === 'error',
                  'bg-blue-500': toast.type === 'info',
                  'bg-yellow-500': toast.type === 'warning',
                }]"
              >
                {{ toast.message }}
                <button @click="dismiss(toast.id)" class="ml-auto">✕</button>
              </div>
            </TransitionGroup>
          </div>
        </Teleport>
        ```
        
    - Update `handleAddToCart` di `BookCard`:
        
        TypeScript
        
        ```
        const { success } = useToast()
        
        function handleAddToCart() {
          cart.addItem(props.book)
          success(`"${props.book.title}" ditambahkan ke keranjang`)
        }
        ```
        
    - _Langkah konkret_: Tambah buku ke cart → toast hijau muncul di kanan bawah selama 3 detik
53. `[[53. Modal Konfirmasi — Dialog yang Reusable]]`
    
    - Buat `src/components/common/ConfirmModal.vue`:
        
        vue
        
        ```
        <Teleport to="body">
          <Transition name="modal">
            <div
              v-if="isOpen"
              class="fixed inset-0 z-50 flex items-center justify-center"
            >
              <!-- Backdrop -->
              <div class="absolute inset-0 bg-black/50" @click="$emit('cancel')" />
              
              <!-- Modal -->
              <div class="relative bg-white rounded-2xl shadow-xl p-6 max-w-md w-full mx-4">
                <h3 class="text-lg font-bold mb-2">{{ title }}</h3>
                <p class="text-gray-600 mb-6">{{ message }}</p>
                
                <div class="flex gap-3 justify-end">
                  <button @click="$emit('cancel')" class="px-4 py-2 border rounded-lg hover:bg-gray-50">
                    Batal
                  </button>
                  <button @click="$emit('confirm')" :class="['px-4 py-2 rounded-lg text-white', dangerButtonClass]">
                    {{ confirmText }}
                  </button>
                </div>
              </div>
            </div>
          </Transition>
        </Teleport>
        ```
        
    - Composable `useConfirm`:
        
        TypeScript
        
        ```
        export function useConfirm() {
          const isOpen = ref(false)
          const resolve = ref<((value: boolean) => void) | null>(null)
          
          function confirm(options: ConfirmOptions): Promise<boolean> {
            isOpen.value = true
            return new Promise(res => { resolve.value = res })
          }
          
          function onConfirm() {
            isOpen.value = false
            resolve.value?.(true)
          }
          
          function onCancel() {
            isOpen.value = false
            resolve.value?.(false)
          }
          
          return { isOpen, confirm, onConfirm, onCancel }
        }
        ```
        
    - Penggunaan:
        
        TypeScript
        
        ```
        const { confirm } = useConfirm()
        
        async function handleDeleteFromCart(bookId: number) {
          const confirmed = await confirm({
            title: 'Hapus dari Keranjang',
            message: 'Yakin ingin menghapus buku ini dari keranjang?',
            confirmText: 'Hapus',
            danger: true,
          })
          
          if (confirmed) {
            cart.removeItem(bookId)
            success('Buku dihapus dari keranjang')
          }
        }
        ```
        
    - _Langkah konkret_: Klik hapus dari cart → modal konfirmasi muncul
54. `[[54. Upload Avatar — File Upload dengan Preview]]`
    
    - Buat `src/components/features/AvatarUpload.vue`:
        
        vue
        
        ```
        <template>
          <div class="flex flex-col items-center gap-4">
            <!-- Preview -->
            <div class="relative w-24 h-24">
              <img
                :src="previewUrl || auth.user?.avatar || '/default-avatar.png'"
                class="w-24 h-24 rounded-full object-cover border-4 border-blue-100"
              />
              <button
                @click="triggerFileInput"
                class="absolute bottom-0 right-0 bg-blue-600 text-white rounded-full p-1.5 hover:bg-blue-700"
              >
                ✏️
              </button>
            </div>
            
            <!-- Hidden File Input -->
            <input
              ref="fileInputRef"
              type="file"
              accept="image/jpeg,image/png,image/webp"
              class="hidden"
              @change="handleFileSelect"
            />
            
            <button
              v-if="selectedFile"
              @click="uploadAvatar"
              :disabled="isUploading"
              class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 disabled:opacity-50"
            >
              {{ isUploading ? 'Mengupload...' : 'Simpan Foto' }}
            </button>
          </div>
        </template>
        
        <script setup lang="ts">
        const fileInputRef = ref<HTMLInputElement | null>(null)
        const selectedFile = ref<File | null>(null)
        const previewUrl = ref<string | null>(null)
        const isUploading = ref(false)
        
        function triggerFileInput() {
          fileInputRef.value?.click()
        }
        
        function handleFileSelect(event: Event) {
          const input = event.target as HTMLInputElement
          const file = input.files?.[0]
          if (!file) return
          
          // Validasi
          if (file.size > 2 * 1024 * 1024) {
            error('Ukuran file maksimal 2MB')
            return
          }
          
          selectedFile.value = file
          // Buat preview URL
          previewUrl.value = URL.createObjectURL(file)
        }
        
        async function uploadAvatar() {
          if (!selectedFile.value) return
          
          isUploading.value = true
          const formData = new FormData()
          formData.append('avatar', selectedFile.value)
          
          try {
            const response = await usersApi.uploadAvatar(formData)
            auth.user!.avatar = response.data.avatarUrl
            success('Foto profil berhasil diperbarui')
            selectedFile.value = null
          } catch {
            error('Gagal mengupload foto')
          } finally {
            isUploading.value = false
          }
        }
        </script>
        ```
        
    - _Langkah konkret_: Klik foto → pilih file → preview muncul → klik Simpan → avatar ter-update
55. `[[55. Infinite Scroll — Load Lebih Banyak Buku]]`
    
    - Install: `npm install @vueuse/core`
    - Gunakan `useIntersectionObserver` dari VueUse:
        
        vue
        
        ```
        <template>
          <div>
            <div class="grid grid-cols-4 gap-6">
              <BookCard v-for="book in books" :key="book.id" :book="book" />
            </div>
            
            <!-- Trigger element di bawah -->
            <div ref="infiniteScrollTrigger" class="h-10 flex items-center justify-center">
              <span v-if="isLoadingMore" class="text-gray-400">Memuat lebih banyak...</span>
            </div>
          </div>
        </template>
        
        <script setup lang="ts">
        import { useIntersectionObserver } from '@vueuse/core'
        
        const books = ref<Book[]>([])
        const page = ref(1)
        const hasMore = ref(true)
        const isLoadingMore = ref(false)
        const infiniteScrollTrigger = ref<HTMLElement | null>(null)
        
        useIntersectionObserver(
          infiniteScrollTrigger,
          async ([entry]) => {
            if (entry.isIntersecting && hasMore.value && !isLoadingMore.value) {
              await loadMore()
            }
          }
        )
        
        async function loadMore() {
          isLoadingMore.value = true
          page.value++
          
          const response = await booksApi.getAll({ page: page.value, limit: 12 })
          books.value.push(...response.data)
          hasMore.value = page.value < response.meta.totalPages
          
          isLoadingMore.value = false
        }
        </script>
        ```
        
    - _Langkah konkret_: Scroll ke bawah → buku baru otomatis dimuat

---

### 🏗️ Checkpoint Level 5

text

```
✅ Checklist sebelum lanjut ke Level 6:
├── VeeValidate + Zod untuk semua form
├── FormField component yang reusable
├── LoginView dan RegisterView dengan validasi proper
├── Checkout flow 3 langkah
├── Toast notification system
├── Modal konfirmasi yang reusable
├── Upload avatar dengan preview
├── Infinite scroll di halaman beranda
├── Semua form tidak bisa submit saat ada error
└── Error message muncul di bawah setiap field

Commit: feat: add form validation, toast, modal, upload, and infinite scroll
```

---

## ⚫ LEVEL 6: TYPESCRIPT PENUH, TESTING & OPTIMASI (Minggu 18-26)

> **Tema**: _"Dari aplikasi yang berjalan ke aplikasi yang bisa dipercaya dan dipelihara"_  
> **Benang Merah**: Semua fitur yang dibangun (Level 1-5) → TypeScript strict → Unit test → E2E test → Optimasi performa  
> **Output**: Aplikasi dengan TypeScript strict, test coverage > 80%, performa teroptimasi

---

### M. TypeScript Strict — Type Safety Menyeluruh

> 💡 **Mengapa TypeScript strict sekarang?** Kita sudah memahami cara kerja aplikasi. Sekarang tambahkan lapisan keamanan yang mencegah bug sejak development, bukan saat production.

56. `[[56. Aktifkan TypeScript Strict Mode & Audit Types]]`
    
    - Update `tsconfig.json`:
        
        JSON
        
        ```
        {
          "compilerOptions": {
            "strict": true,
            "noImplicitAny": true,
            "strictNullChecks": true,
            "noUnusedLocals": true,
            "noUnusedParameters": true
          }
        }
        ```
        
    - Jalankan `npm run type-check` — identifikasi semua error TypeScript
    - Perbaiki error satu per satu (biasanya ada `any` yang perlu diganti)
    - _Langkah konkret_: Tidak ada TypeScript error setelah audit
57. `[[57. Membuat Type System yang Komprehensif]]`
    
    - Audit dan lengkapi semua types di `src/types/`:
        
        TypeScript
        
        ```
        // src/types/api.types.ts
        export interface ApiResponse<T> {
          statusCode: number
          message: string
          data: T
        }
        
        export interface PaginatedResponse<T> {
          statusCode: number
          message: string
          data: {
            data: T[]
            meta: PaginationMeta
          }
        }
        
        export interface PaginationMeta {
          total: number
          page: number
          limit: number
          totalPages: number
          hasNextPage: boolean
          hasPrevPage: boolean
        }
        
        // src/types/book.types.ts
        export interface Book {
          id: string
          title: string
          author: string
          isbn: string
          year: number
          price: number
          stock: number
          rating: number
          coverUrl: string | null
          category: BookCategory
          createdAt: string
          updatedAt: string
        }
        
        export type BookCategory = 'fiction' | 'non-fiction' | 'science' | 'technology' | 'history'
        
        export interface CartItem {
          bookId: string
          book: Book
          quantity: number
        }
        ```
        
    - _Langkah konkret_: Semua response dari API punya tipe yang explicit
58. `[[58. Generic Composables — useApi yang Lebih Type-Safe]]`
    
    - Refactor `useApi` dengan generics:
        
        TypeScript
        
        ```
        import { ref, type Ref } from 'vue'
        
        interface UseApiReturn<T> {
          data: Ref<T | null>
          isLoading: Ref<boolean>
          error: Ref<string | null>
          execute: (fn: () => Promise<T>) => Promise<void>
          reset: () => void
        }
        
        export function useApi<T>(): UseApiReturn<T> {
          const data = ref<T | null>(null) as Ref<T | null>
          const isLoading = ref(false)
          const error = ref<string | null>(null)
          
          async function execute(fn: () => Promise<T>): Promise<void> {
            isLoading.value = true
            error.value = null
            try {
              data.value = await fn()
            } catch (err: unknown) {
              if (err instanceof Error) {
                error.value = err.message
              }
            } finally {
              isLoading.value = false
            }
          }
          
          function reset() {
            data.value = null
            error.value = null
            isLoading.value = false
          }
          
          return { data, isLoading, error, execute, reset }
        }
        ```
        
    - _Langkah konkret_: TypeScript bisa infer return type dari setiap API call

---

### N. Testing — Unit, Component & E2E

> 💡 **Benang Merah ke Composables**: Composables (Poin 26, 27) mudah di-test karena tidak bergantung pada komponen. Kita test composable secara terisolasi, lalu test komponen yang menggunakannya.

59. `[[59. Unit Test Composables — useLocalStorage & useBookFilter]]`
    
    - Buat `src/composables/__tests__/useLocalStorage.spec.ts`:
        
        TypeScript
        
        ```
        import { describe, it, expect, beforeEach, vi } from 'vitest'
        import { useLocalStorage } from '../useLocalStorage'
        
        describe('useLocalStorage', () => {
          beforeEach(() => {
            localStorage.clear()
          })
          
          it('should return default value when key not in storage', () => {
            const { data } = useLocalStorage('test-key', { name: 'default' })
            expect(data.value).toEqual({ name: 'default' })
          })
          
          it('should save value to localStorage when changed', async () => {
            const { data } = useLocalStorage('test-key', '')
            data.value = 'new value'
            
            await nextTick()
            
            expect(localStorage.getItem('test-key')).toBe('"new value"')
          })
        })
        ```
        
    - _Langkah konkret_: Test composable tanpa komponen, hanya logic
60. `[[60. Component Testing — BookCard & AppHeader]]`
    
    - Buat `src/components/features/__tests__/BookCard.spec.ts`:
        
        TypeScript
        
        ```
        import { describe, it, expect } from 'vitest'
        import { mount } from '@vue/test-utils'
        import BookCard from '../BookCard.vue'
        import { createPinia } from 'pinia'
        
        const mockBook = {
          id: '1',
          title: 'Clean Code',
          author: 'Robert Martin',
          price: 150000,
          rating: 4.8,
          stock: 5,
          coverUrl: '/test.jpg',
        }
        
        describe('BookCard', () => {
          it('renders book title and author', () => {
            const wrapper = mount(BookCard, {
              props: { book: mockBook },
              global: {
                plugins: [createPinia()],
              },
            })
            
            expect(wrapper.text()).toContain('Clean Code')
            expect(wrapper.text()).toContain('Robert Martin')
          })
          
          it('shows "Stok Habis" when stock is 0', () => {
            const wrapper = mount(BookCard, {
              props: { book: { ...mockBook, stock: 0 } },
              global: { plugins: [createPinia()] },
            })
            
            expect(wrapper.text()).toContain('Stok Habis')
          })
          
          it('emits add-to-cart when button clicked', async () => {
            const wrapper = mount(BookCard, {
              props: { book: mockBook },
              global: { plugins: [createPinia()] },
            })
            
            await wrapper.find('button').trigger('click')
            
            // Cek toast atau Pinia store (bukan emit langsung)
            const cartStore = useCartStore()
            expect(cartStore.totalItems).toBe(1)
          })
        })
        ```
        
    - _Langkah konkret_: Test komponen secara terisolasi dengan mock store
61. `[[61. Testing Pinia Stores — Cart & Auth Store]]`
    
    - Buat `src/stores/__tests__/cart.store.spec.ts`:
        
        TypeScript
        
        ```
        import { describe, it, expect, beforeEach } from 'vitest'
        import { setActivePinia, createPinia } from 'pinia'
        import { useCartStore } from '../cart.store'
        
        describe('useCartStore', () => {
          beforeEach(() => {
            setActivePinia(createPinia())
          })
          
          it('should add item to cart', () => {
            const cart = useCartStore()
            cart.addItem(mockBook)
            
            expect(cart.items).toHaveLength(1)
            expect(cart.totalItems).toBe(1)
          })
          
          it('should increment quantity if same book added twice', () => {
            const cart = useCartStore()
            cart.addItem(mockBook)
            cart.addItem(mockBook)
            
            expect(cart.items).toHaveLength(1)
            expect(cart.items[0].quantity).toBe(2)
            expect(cart.totalItems).toBe(2)
          })
          
          it('should calculate correct total price', () => {
            const cart = useCartStore()
            cart.addItem({ ...mockBook, price: 100000 })
            cart.addItem({ ...mockBook2, price: 50000 })
            
            expect(cart.totalPrice).toBe(150000)
          })
        })
        ```
        
    - _Langkah konkret_: Test semua actions dan getters di cart store
62. `[[62. E2E Testing dengan Playwright — User Journey Lengkap]]`
    
    - Install: `npm install -D @playwright/test`
    - Buat `e2e/shopping.spec.ts`:
        
        TypeScript
        
        ```
        import { test, expect } from '@playwright/test'
        
        test.describe('Shopping Flow', () => {
          test('user can browse and add book to cart', async ({ page }) => {
            await page.goto('/')
            
            // Verifikasi halaman beranda
            await expect(page.locator('h1')).toContainText('Katalog Buku')
            
            // Cari buku
            await page.fill('[placeholder="Cari buku..."]', 'Clean Code')
            await expect(page.locator('.book-card')).toHaveCount(1)
            
            // Tambah ke cart
            await page.click('[data-testid="add-to-cart-btn"]')
            
            // Verifikasi toast muncul
            await expect(page.locator('.toast')).toContainText('ditambahkan ke keranjang')
            
            // Verifikasi cart counter bertambah
            await expect(page.locator('[data-testid="cart-count"]')).toContainText('1')
            
            // Navigasi ke cart
            await page.click('[href="/cart"]')
            
            // Verifikasi item ada di cart
            await expect(page.locator('.cart-item')).toHaveCount(1)
          })
        })
        ```
        
    - _Langkah konkret_: E2E test simulasi user yang sesungguhnya

---

### O. Optimasi Performa

> 💡 **Benang Merah ke Semua Level**: Semua fitur yang dibangun harus dimuat dengan cepat. Sekarang kita audit dan optimasi.

63. `[[63. Lazy Loading Komponen & Route]]`
    
    - Route sudah menggunakan dynamic import (dari Poin 28), verifikasi semua route:
        
        TypeScript
        
        ```
        // ✅ Benar: lazy loading
        component: () => import('@/views/CheckoutView.vue')
        
        // ❌ Salah: eager loading (import di atas file)
        import CheckoutView from '@/views/CheckoutView.vue'
        ```
        
    - Lazy load komponen berat:
        
        vue
        
        ```
        <script setup lang="ts">
        import { defineAsyncComponent } from 'vue'
        
        // Load hanya saat dibutuhkan
        const RichTextEditor = defineAsyncComponent(() =>
          import('@/components/common/RichTextEditor.vue')
        )
        </script>
        ```
        
    - _Langkah konkret_: Bundle lebih kecil, initial load lebih cepat
64. `[[64. v-memo & shallowRef — Optimasi Rendering List]]`
    
    - `v-memo`: skip re-render jika dependency tidak berubah:
        
        vue
        
        ```
        <!-- Hanya re-render BookCard jika book.id atau isSelected berubah -->
        <BookCard
          v-for="book in books"
          :key="book.id"
          :book="book"
          v-memo="[book.id, isSelected(book.id)]"
        />
        ```
        
    - `shallowRef` untuk list besar yang jarang berubah struktur dalamnya:
        
        TypeScript
        
        ```
        // Untuk array besar: shallowRef lebih efisien
        const books = shallowRef<Book[]>([])
        
        // Untuk update: replace array (bukan mutate)
        books.value = newBooks // ← Vue mendeteksi perubahan
        ```
        
    - _Langkah konkret_: Profile dengan Vue DevTools — kurangi unnecessary re-renders
65. `[[65. Bundle Analysis & Tree Shaking]]`
    
    - Install: `npm install -D rollup-plugin-visualizer`
    - Setup di `vite.config.ts`:
        
        TypeScript
        
        ```
        import { visualizer } from 'rollup-plugin-visualizer'
        
        export default defineConfig({
          plugins: [
            vue(),
            visualizer({
              filename: 'dist/stats.html',
              open: true,
              gzipSize: true,
            }),
          ],
        })
        ```
        
    - Jalankan `npm run build` → buka `dist/stats.html`
    - Identifikasi package yang terlalu besar
    - _Langkah konkret_: Screenshot bundle analysis, identifikasi 3 package terbesar
66. `[[66. Image Optimization — Lazy Load & WebP]]`
    
    - Lazy loading gambar:
        
        vue
        
        ```
        <img
          :src="book.coverUrl"
          :alt="book.title"
          loading="lazy"
          decoding="async"
          class="w-full h-48 object-cover"
        />
        ```
        
    - Placeholder saat image loading:
        
        vue
        
        ```
        <img
          :src="isImageLoaded ? book.coverUrl : '/placeholder.svg'"
          @load="isImageLoaded = true"
          loading="lazy"
        />
        ```
        
    - _Langkah konkret_: Lighthouse score > 85 untuk Performance

---

### 🏗️ Checkpoint Level 6

text

```
✅ Checklist sebelum lanjut ke Level 7:
├── TypeScript strict mode aktif
├── Tidak ada `any` yang tidak disengaja
├── Type system lengkap untuk semua entity
├── Generic composables yang type-safe
├── Unit test composables (coverage > 80%)
├── Component test BookCard, AppHeader
├── Pinia store test (cart dan auth)
├── E2E test shopping flow
├── Lazy loading semua route
├── Bundle analysis — tidak ada dependency yang tidak perlu
├── Lighthouse Performance score > 85
└── CI pipeline menjalankan semua test

Commit: feat: add TypeScript strict, comprehensive tests, and performance optimization
```

---

## 🟣 LEVEL 7: DEPLOYMENT & NUXT.JS (Minggu 26+)

> **Tema**: _"Dari localhost ke internet — dan evolusi ke SSR"_  
> **Benang Merah**: Project yang sudah selesai dan tested (Level 6) → Build → Deploy → CI/CD → Migrasi ke Nuxt untuk SEO  
> **Output**: Aplikasi live di internet dengan deployment otomatis dan versi Nuxt.js untuk SSR

---

### P. Build & Deployment

67. `[[67. Build Production & Preview Lokal]]`
    
    - Jalankan: `npm run build`
    - Preview hasil build: `npm run preview`
    - Analisis output di folder `dist/`
    - Buat `src/utils/env.ts` untuk validasi environment variables:
        
        TypeScript
        
        ```
        const requiredEnvVars = ['VITE_API_BASE_URL'] as const
        
        for (const envVar of requiredEnvVars) {
          if (!import.meta.env[envVar]) {
            throw new Error(`Missing required environment variable: ${envVar}`)
          }
        }
        
        export const env = {
          API_BASE_URL: import.meta.env.VITE_API_BASE_URL,
        } as const
        ```
        
    - _Langkah konkret_: Build sukses tanpa error, preview berjalan dengan baik
68. `[[68. Deploy ke Vercel — Platform Paling Mudah untuk Vue SPA]]`
    
    - Buat akun Vercel, hubungkan ke GitHub repository
    - Konfigurasi di Vercel dashboard:
        - Framework: Vite
        - Build command: `npm run build`
        - Output directory: `dist`
    - Tambahkan environment variables di Vercel dashboard
    - Konfigurasi SPA fallback (semua route → `index.html`):
        - Buat `vercel.json`:
            
            JSON
            
            ```
            {
              "rewrites": [
                { "source": "/(.*)", "destination": "/index.html" }
              ]
            }
            ```
            
    - _Langkah konkret_: Push ke main → Vercel otomatis build dan deploy dalam 2 menit
69. `[[69. CI/CD dengan GitHub Actions — Test Sebelum Deploy]]`
    
    - Buat `.github/workflows/ci.yml`:
        
        YAML
        
        ```
        name: CI/CD
        
        on:
          push:
            branches: [main]
          pull_request:
            branches: [main]
        
        jobs:
          test:
            runs-on: ubuntu-latest
            steps:
              - uses: actions/checkout@v4
              - uses: actions/setup-node@v4
                with:
                  node-version: '20'
                  cache: 'npm'
              
              - run: npm ci
              - run: npm run type-check
              - run: npm run lint
              - run: npm run test --coverage
          
          e2e:
            needs: test
            runs-on: ubuntu-latest
            steps:
              - uses: actions/checkout@v4
              - uses: actions/setup-node@v4
              - run: npm ci
              - run: npx playwright install --with-deps
              - run: npm run test:e2e
        ```
        
    - _Langkah konkret_: PR tidak bisa di-merge jika test gagal
70. `[[70. Setup Error Monitoring — Sentry untuk Production]]`
    
    - Install: `npm install @sentry/vue`
    - Setup di `main.ts`:
        
        TypeScript
        
        ```
        import * as Sentry from '@sentry/vue'
        
        Sentry.init({
          app,
          dsn: import.meta.env.VITE_SENTRY_DSN,
          environment: import.meta.env.MODE,
          integrations: [
            Sentry.browserTracingIntegration({ router }),
          ],
          tracesSampleRate: 0.1, // 10% di production
        })
        ```
        
    - _Langkah konkret_: Error di production otomatis muncul di Sentry dashboard

---

### Q. Pengenalan Nuxt.js — SSR untuk SEO

> 💡 **Mengapa Nuxt?** BookStore kita adalah SPA (Single Page Application). Google bisa crawl-nya, tapi lambat. Buku dengan data dari server tidak ter-index dengan baik. Nuxt mengubahnya menjadi SSR — halaman di-render di server, lebih cepat dan SEO-friendly.

71. `[[71. Buat Project Nuxt 3 Baru — Migrasi dari Vue SPA]]`
    
    - Buat project Nuxt terpisah: `npx nuxi@latest init bookstore-nuxt`
    - Install dependencies yang sama: Tailwind, Pinia, dll
    - Bedakan struktur folder Nuxt vs Vue:
        
        text
        
        ```
        bookstore-nuxt/
        ├── pages/          ← otomatis jadi route (bukan src/views/)
        │   ├── index.vue   ← route /
        │   ├── books/
        │   │   └── [id].vue ← route /books/:id (dynamic)
        │   └── cart.vue    ← route /cart
        ├── components/     ← sama seperti Vue
        ├── composables/    ← sama, tapi auto-imported!
        ├── stores/         ← sama, Pinia auto-imported
        ├── server/
        │   └── api/        ← API routes server-side
        └── nuxt.config.ts  ← konfigurasi Nuxt
        ```
        
    - _Langkah konkret_: Project Nuxt berjalan di `http://localhost:3000`
72. `[[72. File-Based Routing di Nuxt — Pindahkan Halaman]]`
    
    - Copy views dari Vue project ke `pages/` di Nuxt:
        - `src/views/HomeView.vue` → `pages/index.vue`
        - `src/views/BookDetailView.vue` → `pages/books/[id].vue`
        - `src/views/CartView.vue` → `pages/cart.vue`
    - Akses route params di Nuxt:
        
        TypeScript
        
        ```
        // pages/books/[id].vue
        const route = useRoute()
        const bookId = route.params.id // sama seperti Vue Router
        ```
        
    - _Langkah konkret_: Semua halaman berfungsi dengan file-based routing
73. `[[73. useFetch & useAsyncData — Data Fetching di Nuxt (SSR-Aware)]]`
    
    - Nuxt punya composable khusus untuk SSR:
        
        TypeScript
        
        ```
        // ❌ Jangan gunakan ini di Nuxt pages (tidak SSR-friendly)
        onMounted(async () => {
          books.value = await booksApi.getAll()
        })
        
        // ✅ Gunakan useFetch (berjalan di server DAN client)
        const { data: books, pending, error } = await useFetch('/api/books', {
          query: { page: 1, limit: 12 }
        })
        
        // Data books sudah tersedia saat halaman di-render di server
        // Tidak ada loading state saat pertama kali buka halaman
        ```
        
    - _Langkah konkret_: Buka halaman beranda → konten muncul INSTAN (bukan loading spinner dulu)
74. `[[74. Server API Routes — Backend Ringan di Nuxt]]`
    
    - Buat `server/api/books.get.ts`:
        
        TypeScript
        
        ```
        export default defineEventHandler(async (event) => {
          const query = getQuery(event)
          
          // Call backend NestJS dari server-side
          const response = await $fetch(`${process.env.API_BASE_URL}/books`, {
            query,
          })
          
          return response
        })
        ```
        
    - Keuntungan: API key tidak terekspose ke client
    - _Langkah konkret_: Request dari Nuxt ke `/api/books` → diteruskan ke NestJS
75. `[[75. SEO dengan useHead & useSeoMeta — Meta Tags untuk Setiap Halaman]]`
    
    - Setup SEO di setiap halaman:
        
        TypeScript
        
        ```
        // pages/books/[id].vue
        useSeoMeta({
          title: () => `${book.value?.title} - BookStore`,
          description: () => `Beli ${book.value?.title} karya ${book.value?.author}`,
          ogTitle: () => book.value?.title,
          ogImage: () => book.value?.coverUrl,
          ogType: 'product',
        })
        ```
        
    - _Langkah konkret_: Buka source code halaman → meta tags ada di HTML (bukan hanya JavaScript)
76. `[[76. Deploy Nuxt ke Vercel — SSR di Production]]`
    
    - Nuxt otomatis terdeteksi oleh Vercel sebagai SSR app
    - Update `vercel.json` (tidak perlu rewrite rules untuk Nuxt)
    - Tambahkan environment variables server-side (bukan `VITE_*`, tapi tanpa prefix)
    - _Langkah konkret_: Site live dengan SSR — cek dengan Google Search Console

---

### 🏗️ Proyek Level 7 — Selesai

text

```
PROYEK FINAL: "BookStore — Fullstack Vue Application"
──────────────────────────────────────────────────────
Vue.js SPA (bookstore-ui):
├── Katalog buku dengan search, filter, sort
├── Halaman detail buku
├── Keranjang belanja dengan persist
├── Wishlist
├── Auth lengkap (login, register, OAuth)
├── Checkout flow 3 langkah
├── Upload avatar
├── Toast notification
├── Modal konfirmasi
├── Infinite scroll
├── Dark mode
├── TypeScript strict
├── Test coverage > 80%
├── Deploy di Vercel
└── CI/CD dengan GitHub Actions

Nuxt.js SSR (bookstore-nuxt):
├── Semua fitur SPA +
├── Server-side rendering
├── SEO meta tags
├── Server API routes
└── Deploy di Vercel dengan SSR
```

---

## 📊 Ringkasan Progress Tracking

### Satu Project, 7 Level Enhancement

text

```
Level 1: BookStore UI — komponen statis, template syntax
  + Level 2: + Reaktivitas, filter, cart counter, dark mode
  + Level 3: + Router multi-halaman, Pinia state management
  + Level 4: + API integration, autentikasi JWT
  + Level 5: + Form validasi, toast, modal, upload, infinite scroll
  + Level 6: + TypeScript strict, testing komprehensif, optimasi
  + Level 7: + Deployment, CI/CD, Nuxt.js SSR
```

### Tabel Progress

|Level|Poin|Durasi|Output Konkret|
|---|---|---|---|
|🟢 **1**|1-15|Minggu 1-2|Katalog buku statis di browser|
|🔵 **2**|16-27|Minggu 2-4|Filter, search, cart counter, dark mode|
|🟡 **3**|28-37|Minggu 4-7|Multi-halaman, cart di semua halaman|
|🟠 **4**|38-47|Minggu 7-12|Terhubung ke NestJS, login berfungsi|
|🔴 **5**|48-55|Minggu 12-18|Form valid, toast, modal, upload, scroll|
|⚫ **6**|56-66|Minggu 18-26|Test coverage > 80%, performance A|
|🟣 **7**|67-76|Minggu 26+|Live di Vercel + SSR dengan Nuxt|

---

### Benang Merah Utama Sepanjang Roadmap

text

```
Poin 3  (Struktur folder)     → Semua komponen mengikuti struktur ini
Poin 7  (Anatomi .vue file)   → Fondasi semua komponen yang dibuat
Poin 12 (BookCard + Props)    → Pola yang diulang di semua komponen
Poin 16 (ref & reaktivitas)   → Dasar semua state di seluruh project
Poin 18 (computed)            → filteredBooks, totalPrice, isAuthenticated
Poin 26 (useLocalStorage)     → Digunakan di useBookFilter, dark mode
Poin 33 (Cart Store)          → Diakses dari AppHeader, BookCard, CartView
Poin 38 (Axios instance)      → Semua API call menggunakan instance ini
Poin 43 (Auth Store)          → Router guard, header, API interceptor
Poin 52 (Toast)               → Digunakan di setiap action yang berhasil
Poin 59-62 (Testing)          → Test setiap composable dan komponen penting
Poin 68 (Deploy Vercel)       → CI/CD pipeline mengarah ke sini
```

---

## 💡 Cara Menggunakan Roadmap Ini

text

```
Setiap poin mengikuti format:
┌──────────────────────────────────────────────────────┐
│ 💡 Konteks: mengapa langkah ini ada                  │
│ 🔗 Benang Merah: koneksi ke poin sebelum/sesudah     │
│ 📋 Kode: implementasi konkret yang bisa langsung      │
│          dicopy dan dimodifikasi                     │
│ ✅ Langkah konkret: cara verifikasi berhasil         │
└──────────────────────────────────────────────────────┘
```

**Aturan yang Tidak Boleh Dilanggar:**

1. **Satu project dari awal hingga akhir** — jangan buat project baru setiap level
2. **Verifikasi setiap `Langkah konkret`** — jika tidak berhasil, jangan lanjut
3. **Commit setelah setiap poin** — gunakan conventional commits
4. **Buka browser DevTools** — pantau error console dan network request
5. **Selesaikan Checkpoint Level** sebelum naik ke level berikutnya
6. **Baca kode yang ditulis** — jangan copy-paste tanpa memahami

---

_Roadmap Vue.js v1.0 — Step-by-Step, One Project, Connected_  
_Setiap baris kode ada karena alasan yang jelas dan terhubung ke yang lain_