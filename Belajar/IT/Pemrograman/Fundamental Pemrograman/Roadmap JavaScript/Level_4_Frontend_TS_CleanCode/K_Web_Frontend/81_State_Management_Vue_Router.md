# 81. State Management & Vue Router

**Benang Merah**: Dari Materi 80 (Vue fundamentals) kita bisa bikin komponen. Tapi aplikasi besar butuh **state management** (sharing data antar komponen) dan **routing** (multi-halaman). Pinia & Vue Router adalah solusinya. **PENUTUP Frontend** — lanjut ke TypeScript (Materi 82-87).

---

## Penjelasan

### Kenapa State Management?

Di aplikasi kompleks, banyak komponen butuh **data yang sama**:
- User login info → header, sidebar, halaman profil
- Keranjang belanja → navbar (jumlah item), halaman cart, checkout
- Preferensi (dark mode) → semua komponen

**Masalah tanpa state management**: props drilling — data dikirim dari parent → child → grandchild → ... → jengkel.

```
App → Header → Navbar → UserAvatar (butuh user data)
   ↘ HalamanProfil (butuh user data)
```

**Solusi dengan Pinia**: Store global — semua komponen akses langsung.

```
Pinia Store (user)
  → Header → Navbar → UserAvatar (akses langsung)
  → HalamanProfil (akses langsung)
```

### Kenapa Vue Router?

Aplikasi web biasanya punya **banyak halaman**: beranda, koleksi buku, detail buku, about. Vue Router memungkinkan navigasi **tanpa reload halaman** (SPA).

---

## Fungsi

| Pinia (State Management) | Vue Router |
|---|---|
| Menyimpan & berbagi data global | Navigasi antar halaman |
| Actions (mutasi data) | Route parameters (`/buku/:id`) |
| Getters (data turunan / computed) | Nested routes |
| DevTools untuk debugging | Navigation guards (auth) |
| Persist state (localStorage plugin) | Lazy loading (code splitting) |

---

## Cara Pengimplementasian

### 1. Setup Pinia

```bash
npm install pinia
```

**main.js**:
```javascript
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'

const app = createApp(App)
app.use(createPinia())
app.mount('#app')
```

### 2. Store dengan Pinia (Composition API)

**stores/counter.js**:
```javascript
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'

export const useCounterStore = defineStore('counter', () => {
  // State
  const count = ref(0)

  // Getter — computed
  const doubleCount = computed(() => count.value * 2)

  // Action — method
  function increment() {
    count.value++
  }

  function decrement() {
    count.value--
  }

  return { count, doubleCount, increment, decrement }
})
```

**Component memakai store**:
```vue
<script setup>
import { useCounterStore } from './stores/counter'

const store = useCounterStore()
</script>

<template>
  <p>Count: {{ store.count }}</p>
  <p>Double: {{ store.doubleCount }}</p>
  <button @click="store.increment">+</button>
  <button @click="store.decrement">-</button>
</template>
```

### 3. Store Perpustakaan — Pinia

**stores/perpustakaan.js**:
```javascript
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'

export const usePerpusStore = defineStore('perpustakaan', () => {
  const books = ref([
    { id: 1, title: 'JavaScript: The Good Parts', author: 'Douglas Crockford', available: true },
    { id: 2, title: 'Clean Code', author: 'Robert C. Martin', available: true },
    { id: 3, title: 'HTML & CSS', author: 'Jon Duckett', available: false }
  ])

  const searchQuery = ref('')

  const filteredBooks = computed(() => {
    if (!searchQuery.value) return books.value
    const q = searchQuery.value.toLowerCase()
    return books.value.filter(b =>
      b.title.toLowerCase().includes(q) ||
      b.author.toLowerCase().includes(q)
    )
  })

  function pinjamBuku(id) {
    const book = books.value.find(b => b.id === id)
    if (book && book.available) {
      book.available = false
    }
  }

  function kembalikanBuku(id) {
    const book = books.value.find(b => b.id === id)
    if (book && !book.available) {
      book.available = true
    }
  }

  return { books, searchQuery, filteredBooks, pinjamBuku, kembalikanBuku }
})
```

### 4. Setup Vue Router

```bash
npm install vue-router
```

**router/index.js**:
```javascript
import { createRouter, createWebHistory } from 'vue-router'
import Beranda from '../views/Beranda.vue'

const routes = [
  {
    path: '/',
    name: 'Beranda',
    component: Beranda
  },
  {
    path: '/buku',
    name: 'KoleksiBuku',
    component: () => import('../views/KoleksiBuku.vue') // lazy load
  },
  {
    path: '/buku/:id',
    name: 'DetailBuku',
    component: () => import('../views/DetailBuku.vue'),
    props: true
  },
  {
    path: '/tentang',
    name: 'Tentang',
    component: () => import('../views/Tentang.vue')
  },
  {
    path: '/:pathMatch(.*)*',
    name: 'NotFound',
    component: () => import('../views/NotFound.vue')
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

// Navigation guard — cek auth
router.beforeEach((to, from, next) => {
  // Contoh: cek login
  const isAuthenticated = localStorage.getItem('token')
  if (to.meta.requiresAuth && !isAuthenticated) {
    next({ name: 'Beranda' })
  } else {
    next()
  }
})

export default router
```

**main.js**:
```javascript
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import router from './router'
import App from './App.vue'

const app = createApp(App)
app.use(createPinia())
app.use(router)
app.mount('#app')
```

### 5. Multi-Halaman dengan Vue + Pinia + Router

**App.vue**:
```vue
<script setup>
import { usePerpusStore } from './stores/perpustakaan'

const store = usePerpusStore()
</script>

<template>
  <header>
    <h1>Perpustakaan Online</h1>
    <nav>
      <router-link to="/">Beranda</router-link>
      <router-link to="/buku">Koleksi</router-link>
      <router-link to="/tentang">Tentang</router-link>
    </nav>
    <!-- Search global — akses store langsung -->
    <input v-model="store.searchQuery" placeholder="Cari buku...">
  </header>

  <!-- Halaman akan berubah di sini tanpa reload -->
  <main>
    <router-view />
  </main>

  <footer>
    <p>&copy; 2026 Perpustakaan Online</p>
  </footer>
</template>
```

**views/Beranda.vue**:
```vue
<script setup>
import { usePerpusStore } from '../stores/perpustakaan'

const store = usePerpusStore()
</script>

<template>
  <section>
    <h2>Selamat Datang di Perpustakaan Online</h2>
    <p>Total buku tersedia: {{ store.books.filter(b => b.available).length }}</p>
    <p>Total buku dipinjam: {{ store.books.filter(b => !b.available).length }}</p>
    <router-link to="/buku" class="btn">Lihat Koleksi</router-link>
  </section>
</template>
```

**views/KoleksiBuku.vue**:
```vue
<script setup>
import { usePerpusStore } from '../stores/perpustakaan'

const store = usePerpusStore()
</script>

<template>
  <section>
    <h2>Koleksi Buku</h2>
    <div v-if="store.filteredBooks.length === 0" class="empty">
      Buku tidak ditemukan.
    </div>
    <div class="grid">
      <div v-for="book in store.filteredBooks" :key="book.id" class="card">
        <h3>
          <router-link :to="`/buku/${book.id}`">{{ book.title }}</router-link>
        </h3>
        <p>{{ book.author }}</p>
        <span v-if="book.available" class="badge">Tersedia</span>
        <span v-else class="badge badge-out">Dipinjam</span>
        <button
          v-if="book.available"
          @click="store.pinjamBuku(book.id)"
          class="btn-pinjam">
          Pinjam
        </button>
        <button
          v-else
          @click="store.kembalikanBuku(book.id)"
          class="btn-kembali">
          Kembalikan
        </button>
      </div>
    </div>
  </section>
</template>
```

**views/DetailBuku.vue**:
```vue
<script setup>
import { useRoute, useRouter } from 'vue-router'
import { usePerpusStore } from '../stores/perpustakaan'
import { computed } from 'vue'

const route = useRoute()
const router = useRouter()
const store = usePerpusStore()

const book = computed(() =>
  store.books.find(b => b.id === Number(route.params.id))
)

if (!book.value) {
  router.push('/404')
}
</script>

<template>
  <section v-if="book">
    <h2>{{ book.title }}</h2>
    <p>Penulis: {{ book.author }}</p>
    <p>Status: {{ book.available ? 'Tersedia' : 'Dipinjam' }}</p>
    <button @click="$router.back()" class="btn">Kembali</button>
  </section>
</template>
```

**views/NotFound.vue**:
```vue
<template>
  <section class="not-found">
    <h2>404 — Halaman Tidak Ditemukan</h2>
    <router-link to="/" class="btn">Kembali ke Beranda</router-link>
  </section>
</template>
```

---

## Analogi: Membangun Rumah (Panel Kontrol + Denah)

| Pinia / Vue Router | Rumah |
|---|---|
| **Pinia Store** | Panel kontrol listrik pusat — semua ruangan terhubung |
| **State** (data) | Suhu, lampu status setiap ruangan |
| **Getter** (computed) | Rata-rata suhu rumah — otomatis dihitung |
| **Action** | Saklar "Matikan semua lampu" — satu tombol, efek di seluruh rumah |
| **Props drilling** | Kabel listrik dari ruang tamu → dapur → kamar → ... (ribet) |
| **Pinia** | Satu panel kontrol di ruang tengah — semua ruangan akses langsung |
| **Vue Router** | Peta / denah rumah — tahu ruangan apa saja dan cara ke sana |
| **Route `/`** | Pintu depan — halaman utama |
| **Route `/buku/:id`** | Kamar tamu "Kamar 101" — halaman dinamis |
| **`<router-link>`** | Papan petunjuk arah — "Kamar 101 → belok kiri" |
| **`<router-view>`** | Ruang kosong yang isinya berubah — "sekarang di ruang tamu" |
| **Navigation guard** | Kunci pintu — hanya yang punya akses bisa masuk |
| **Lazy loading** | Membuka kamar hanya saat dibutuhkan — hemat listrik |
| **SPA tanpa router** | Rumah satu ruangan besar — semua aktivitas di situ |
| **SPA dengan router** | Rumah dengan banyak ruangan — masing-masing punya fungsi |

**Props drilling** = Anda harus pasang kabel dari ruang tamu ke dapur ke kamar ke gudang hanya untuk nyalakan lampu kamar. **Pinia** = panel kontrol pusat — cukup pencet tombol di panel, lampu kamar menyala, tanpa kabel panjang.

**Vue Router** = peta rumah. Saat Anda bilang "saya mau ke kamar 101", Anda tidak perlu bongkar dinding — cukup jalan sesuai peta (router), dan ruangan (component) muncul di hadapan Anda.

---

## Dipakai Untuk Apa

- **Aplikasi multi-halaman** — dashboard, e-commerce, admin panel
- **State global** — user auth, cart, theme, notifications
- **SPA dengan routing kompleks** — nested routes, modal sebagai route
- **Authentication flow** — login → redirect ke halaman sebelumnya

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Store terlalu besar | Semua data di satu store | Sulit di-maintain |
| Mutasi state tanpa action | `store.count = 5` di luar store | Tidak ter-track devtools |
| Lupa `.value` di Pinia | `store.count + 1` padahal ref | NaN / error |
| Route tanpa `:key` | Component tidak re-render saat route params berubah | Bug |
| Navigation guard infinite loop | Redirect salah | Halaman blank / error |

---

## Hubungan dengan Materi Sebelumnya

- Materi 80 (Vue) → Pinia & Router adalah ekosistem Vue untuk aplikasi kompleks
- Materi 79 (Fetch) → Fetch di Pinia actions — data dari API masuk state
- Materi 77 (DOM) → Vue + Pinia + Router menggantikan DOM manual
- **PENUTUP Frontend** → Lanjut ke TypeScript (Materi 82-87)

---

## Soal Latihan

### Soal 1 (Mudah)
Buat Pinia store untuk counter dengan state `count`, action `increment`, getter `doubleCount`. Gunakan di komponen.

**Jawaban**:

**stores/counter.js**:
```javascript
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'

export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)
  const doubleCount = computed(() => count.value * 2)

  function increment() {
    count.value++
  }

  return { count, doubleCount, increment }
})
```

**Component**:
```vue
<script setup>
import { useCounterStore } from '../stores/counter'
const store = useCounterStore()
</script>

<template>
  <p>Count: {{ store.count }}</p>
  <p>Double: {{ store.doubleCount }}</p>
  <button @click="store.increment">+1</button>
</template>
```

### Soal 2 (Sedang)
Buat dua route: `/` (home) dan `/about` (about). Masing-masing tampilkan halaman berbeda. Gunakan `router-link` untuk navigasi.

**Jawaban**:

**router/index.js**:
```javascript
import { createRouter, createWebHistory } from 'vue-router'
import Home from '../views/Home.vue'
import About from '../views/About.vue'

const routes = [
  { path: '/', name: 'Home', component: Home },
  { path: '/about', name: 'About', component: About }
]

export default createRouter({
  history: createWebHistory(),
  routes
})
```

**App.vue**:
```vue
<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </nav>
  <router-view />
</template>
```

**views/Home.vue**: `<h1>Halaman Beranda</h1>`
**views/About.vue**: `<h1>Tentang Kami</h1>`

### Soal 3 (Tantangan)
Buat aplikasi mini perpustakaan dengan: Pinia store untuk daftar buku (id, title, author), Vue Router untuk halaman daftar buku (`/books`) dan detail buku (`/books/:id`). Di halaman daftar, setiap buku adalah link ke detail. Di halaman detail, tampilkan info buku dan tombol back.

**Jawaban**:

**stores/books.js**:
```javascript
import { defineStore } from 'pinia'
import { ref } from 'vue'

export const useBooksStore = defineStore('books', () => {
  const books = ref([
    { id: 1, title: 'JavaScript: The Good Parts', author: 'Douglas Crockford' },
    { id: 2, title: 'Clean Code', author: 'Robert C. Martin' }
  ])

  function getBookById(id) {
    return books.value.find(b => b.id === id)
  }

  return { books, getBookById }
})
```

**router/index.js**:
```javascript
import { createRouter, createWebHistory } from 'vue-router'
import BookList from '../views/BookList.vue'
import BookDetail from '../views/BookDetail.vue'

const routes = [
  { path: '/books', name: 'BookList', component: BookList },
  { path: '/books/:id', name: 'BookDetail', component: BookDetail, props: true }
]

export default createRouter({ history: createWebHistory(), routes })
```

**views/BookList.vue**:
```vue
<script setup>
import { useBooksStore } from '../stores/books'
const store = useBooksStore()
</script>

<template>
  <h1>Daftar Buku</h1>
  <ul>
    <li v-for="book in store.books" :key="book.id">
      <router-link :to="`/books/${book.id}`">{{ book.title }}</router-link>
    </li>
  </ul>
</template>
```

**views/BookDetail.vue**:
```vue
<script setup>
import { useRoute, useRouter } from 'vue-router'
import { useBooksStore } from '../stores/books'

const route = useRoute()
const router = useRouter()
const store = useBooksStore()
const book = store.getBookById(Number(route.params.id))
</script>

<template>
  <div v-if="book">
    <h1>{{ book.title }}</h1>
    <p>Penulis: {{ book.author }}</p>
    <button @click="router.push('/books')">Kembali</button>
  </div>
  <div v-else>
    <p>Buku tidak ditemukan.</p>
    <router-link to="/books">Kembali ke daftar</router-link>
  </div>
</template>
```
