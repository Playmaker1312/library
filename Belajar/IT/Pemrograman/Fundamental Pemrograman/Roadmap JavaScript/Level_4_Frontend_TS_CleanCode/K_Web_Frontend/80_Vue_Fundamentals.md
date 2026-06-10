# 80. Vue.js Fundamentals

**Benang Merah**: Dari Materi 79 (Fetch + DOM manual) kita lihat bahwa DOM manipulation manual itu **berulang dan rawan error**. Vue.js adalah **framework reaktif** yang membuat frontend development lebih terstruktur dan efisien. Lanjut ke Materi 81 (Router + State Management).

---

## Penjelasan

Vue.js adalah **progressive JavaScript framework** untuk membangun User Interface. Dua konsep utamanya:

### Reactivity (Reaktivitas)
Saat data berubah, UI **otomatis update** — tidak perlu `document.querySelector` + `textContent` manual.

```javascript
// Tanpa Vue (manual)
let count = 0;
const el = document.getElementById('counter');
el.textContent = count;

// Update manual setiap kali
count++;
el.textContent = count;

// Dengan Vue — otomatis!
const counter = ref(0);
// <p>{{ counter }}</p> — otomatis update saat counter berubah
```

### Component (Komponen)
UI dipecah menjadi komponen kecil yang **independen** dan **bisa dipakai ulang**.

```
App.vue
├── Header.vue
├── BookList.vue
│   └── BookCard.vue (dipakai 3x)
├── BookForm.vue
└── Footer.vue
```

### Template & Directives

```html
<template>
  <div>
    <p v-if="isLoading">Memuat...</p>
    <ul>
      <li v-for="book in books" :key="book.id">
        {{ book.title }}
      </li>
    </ul>
    <button @click="handleClick">Klik</button>
    <input v-model="searchQuery">
  </div>
</template>
```

---

## Fungsi

- **Reaktivitas** — data ↔ UI sinkron otomatis
- **Component-based** — kode modular, reusable, testable
- **Developer experience** — tooling, devtools, hot reload
- **Ekosistem** — Vue Router, Pinia (state), Vite (build tool)
- **Performa** — virtual DOM, tree-shaking

---

## Cara Pengimplementasian

### 1. Setup Vue + Vite

```bash
npm create vite@latest perpustakaan-vue -- --template vue
cd perpustakaan-vue
npm install
npm run dev
```

### 2. Composition API — ref, reactive, computed, watch

```vue
<script setup>
import { ref, reactive, computed, watch } from 'vue'

// ref — untuk primitive / single value
const count = ref(0)
const nama = ref('Budi')

// reactive — untuk object
const user = reactive({
  name: 'Budi',
  age: 25
})

// computed — derived value (otomatis update)
const umurGanda = computed(() => user.age * 2)

// watch — side effect saat data berubah
watch(count, (newVal, oldVal) => {
  console.log(`Count berubah: ${oldVal} → ${newVal}`)
})
</script>

<template>
  <p>{{ count }}</p>
  <button @click="count++">Tambah</button>

  <p>{{ user.name }} — {{ user.age }} tahun</p>
  <p>Umur x 2: {{ umurGanda }}</p>
</template>
```

### 3. Directives — v-if, v-for, v-bind, v-on, v-model

```vue
<script setup>
import { ref } from 'vue'

const books = ref([
  { id: 1, title: 'JavaScript: The Good Parts', author: 'Douglas Crockford', available: true },
  { id: 2, title: 'Clean Code', author: 'Robert C. Martin', available: false },
  { id: 3, title: 'HTML & CSS', author: 'Jon Duckett', available: true }
])

const newTitle = ref('')
const showAvailableOnly = ref(false)

function addBook() {
  if (!newTitle.value.trim()) return
  books.value.push({
    id: Date.now(),
    title: newTitle.value,
    author: 'Unknown',
    available: true
  })
  newTitle.value = ''
}
</script>

<template>
  <!-- v-model — two-way binding -->
  <input v-model="newTitle" placeholder="Judul buku baru" @keyup.enter="addBook">
  <button @click="addBook">Tambah</button>

  <!-- v-if / v-else -->
  <p v-if="books.length === 0">Tidak ada buku.</p>

  <!-- v-for — looping -->
  <ul>
    <li v-for="book in books" :key="book.id"
        :class="{ available: book.available }"   <!-- v-bind:class -->
        @click="book.available = !book.available"> <!-- v-on:click -->
      {{ book.title }} — {{ book.author }}
      <span v-if="!book.available">(Dipinjam)</span>
      <span v-else>(Tersedia)</span>
    </li>
  </ul>

  <!-- Checkbox untuk filter -->
  <label>
    <input type="checkbox" v-model="showAvailableOnly">
    Hanya tampilkan yang tersedia
  </label>
</template>

<style>
.available { color: green; }
</style>
```

### 4. Component — Props & Emits

**BookCard.vue** (child component):
```vue
<script setup>
defineProps({
  book: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['pinjam', 'hapus'])
</script>

<template>
  <div class="card" :class="{ dipinjam: !book.available }">
    <h3>{{ book.title }}</h3>
    <p>{{ book.author }}</p>
    <span v-if="book.available" class="badge">Tersedia</span>
    <span v-else class="badge badge-dipinjam">Dipinjam</span>

    <div class="aksi">
      <button v-if="book.available" @click="emit('pinjam', book.id)">
        Pinjam
      </button>
      <button class="hapus" @click="emit('hapus', book.id)">
        Hapus
      </button>
    </div>
  </div>
</template>

<style scoped>
.card { border: 1px solid #ddd; padding: 16px; border-radius: 8px; }
.dipinjam { opacity: 0.6; }
.badge { background: #27ae60; color: white; padding: 2px 10px; border-radius: 20px; font-size: 0.8em; }
.badge-dipinjam { background: #e74c3c; }
.aksi { margin-top: 12px; display: flex; gap: 8px; }
button { padding: 6px 12px; border: none; border-radius: 4px; cursor: pointer; }
.hapus { background: #e74c3c; color: white; }
</style>
```

**App.vue** (parent):
```vue
<script setup>
import { ref } from 'vue'
import BookCard from './components/BookCard.vue'

const books = ref([
  { id: 1, title: 'JavaScript: The Good Parts', author: 'Douglas Crockford', available: true },
  { id: 2, title: 'Clean Code', author: 'Robert C. Martin', available: false }
])

function handlePinjam(id) {
  const book = books.value.find(b => b.id === id)
  if (book) book.available = false
}

function handleHapus(id) {
  books.value = books.value.filter(b => b.id !== id)
}
</script>

<template>
  <div class="grid">
    <BookCard
      v-for="book in books"
      :key="book.id"
      :book="book"
      @pinjam="handlePinjam"
      @hapus="handleHapus"
    />
  </div>
</template>
```

---

## Analogi: Membangun Rumah (Rumah Prefabrikasi)

| Vue.js | Rumah Prefabrikasi |
|---|---|
| **Component** | Modul rumah siap pasang — kamar, dapur, ruang tamu |
| `props` | Spesifikasi modul — "kamar ini ukuran 4x5 meter" |
| `emits` | Saklar di modul — kirim sinyal ke rumah utama |
| `ref()` | Meteran listrik — satu sumber data untuk satu ruangan |
| `reactive()` | Panel kontrol utama — monitor seluruh rumah |
| `computed` | Termometer otomatis — "jika suhu > 30, tampilkan peringatan" |
| `watch` | Sensor — "saat pintu terbuka, kirim notifikasi" |
| `v-if` | Dinding geser — muncul/sembunyi sesuai kebutuhan |
| `v-for` | Memasang 10 jendela identik |
| `v-model` | Remote control — ubah suhu AC, tampilan langsung berubah |
| `@click` | Saklar lampu — "saat ditekan, lampu menyala" |
| **Vanilla JS** | Bangun rumah dari batu bata — manual, fleksibel tapi butuh waktu |
| **Vue.js** | Rumah prefab — komponen tinggal pasang, lebih cepat, lebih rapi |

**Vanilla JS** = Anda membangun rumah dari **batu bata, semen, dan pasir** — fleksibel, Anda kontrol penuh, tapi setiap dinding harus dibangun manual.

**Vue.js** = Anda membeli **rumah prefabrikasi** — modul kamar, dapur, ruang tamu sudah jadi. Tinggal rakit di lokasi. Lebih cepat, konsisten, dan mudah diperbaiki.

---

## Dipakai Untuk Apa

- **Single Page Application (SPA)** — dashboard, admin panel
- **Aplikasi kompleks** — e-commerce, social media, project management
- **Prototype cepat** — dengan reactivity, development 2x lebih cepat
- **Tim besar** — komponen modular, terstruktur, mudah di-test

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Mutasi props langsung | `props.book.title = 'Baru'` | Error / warning Vue |
| Lupa `.value` di template | `ref(0)` → `{{ count }}` — ini benar | Tidak perlu `.value` di template |
| `v-if` dan `v-for` di elemen sama | `<li v-for="..." v-if="...">` | Prioritas v-if lebih tinggi |
| Tidak pakai `:key` di `v-for` | `v-for="item in items"` tanpa `:key` | Performa buruk, bug |
| One-way data flow dilanggar | Child mengubah data parent langsung | State kacau |

---

## Hubungan dengan Materi Sebelumnya

- Materi 77 (DOM) → Vue menggantikan DOM manipulation manual
- Materi 78 (Event) → `@click`, `@submit` di Vue = event handling
- Materi 79 (Fetch) → Vue + Fetch = data dari API dirender reaktif
- Materi 81 (Router + Pinia) → Tambah routing & state management untuk app kompleks

---

## Soal Latihan

### Soal 1 (Mudah)
Buat komponen Vue yang menampilkan nama dan umur. Data menggunakan `ref`. Tampilkan di template dengan interpolasi `{{ }}`.

**Jawaban**:
```vue
<script setup>
import { ref } from 'vue'

const name = ref('Andi')
const age = ref(25)
</script>

<template>
  <div>
    <p>Nama: {{ name }}</p>
    <p>Umur: {{ age }} tahun</p>
  </div>
</template>
```

### Soal 2 (Sedang)
Buat counter component dengan Vue Composition API. Fitur: tampilkan angka, tombol +, tombol -, reset. Jika angka negatif, teks warna merah.

**Jawaban**:
```vue
<script setup>
import { ref, computed } from 'vue'

const count = ref(0)
const colorClass = computed(() => count.value < 0 ? 'merah' : '')
</script>

<template>
  <div>
    <p :class="colorClass">Count: {{ count }}</p>
    <button @click="count++">+</button>
    <button @click="count--">-</button>
    <button @click="count = 0">Reset</button>
  </div>
</template>

<style>
.merah { color: red; }
</style>
```

### Soal 3 (Tantangan)
Buat todo list app dengan Vue component. Komponen `TodoItem` menerima `props` (todo) dan `emit` (toggle, hapus). Komponen parent `App` mengelola daftar todo. Fitur: tambah, tandai selesai, hapus, filter (all/active/completed).

**Jawaban**:

**TodoItem.vue**:
```vue
<script setup>
defineProps({
  todo: Object
})
const emit = defineEmits(['toggle', 'hapus'])
</script>

<template>
  <li :class="{ selesai: todo.selesai }">
    <span @click="emit('toggle', todo.id)" style="cursor:pointer">
      {{ todo.selesai ? '✓' : '○' }} {{ todo.teks }}
    </span>
    <button @click="emit('hapus', todo.id)">X</button>
  </li>
</template>

<style scoped>
.selesai span:first-child { text-decoration: line-through; color: gray; }
li { margin: 4px 0; display: flex; justify-content: space-between; }
</style>
```

**App.vue**:
```vue
<script setup>
import { ref, computed } from 'vue'
import TodoItem from './components/TodoItem.vue'

const todos = ref([
  { id: 1, teks: 'Belajar Vue', selesai: false },
  { id: 2, teks: 'Baca buku', selesai: true }
])
const input = ref('')
const filter = ref('all')

function tambahTodo() {
  if (!input.value.trim()) return
  todos.value.push({
    id: Date.now(),
    teks: input.value,
    selesai: false
  })
  input.value = ''
}

function toggleTodo(id) {
  const todo = todos.value.find(t => t.id === id)
  if (todo) todo.selesai = !todo.selesai
}

function hapusTodo(id) {
  todos.value = todos.value.filter(t => t.id !== id)
}

const filteredTodos = computed(() => {
  if (filter.value === 'active') return todos.value.filter(t => !t.selesai)
  if (filter.value === 'completed') return todos.value.filter(t => t.selesai)
  return todos.value
})
</script>

<template>
  <div>
    <input v-model="input" @keyup.enter="tambahTodo" placeholder="Tambah todo...">
    <button @click="tambahTodo">Tambah</button>

    <div>
      <button @click="filter = 'all'" :class="{ aktif: filter === 'all' }">All</button>
      <button @click="filter = 'active'" :class="{ aktif: filter === 'active' }">Active</button>
      <button @click="filter = 'completed'" :class="{ aktif: filter === 'completed' }">Completed</button>
    </div>

    <ul>
      <TodoItem
        v-for="todo in filteredTodos"
        :key="todo.id"
        :todo="todo"
        @toggle="toggleTodo"
        @hapus="hapusTodo"
      />
    </ul>
  </div>
</template>

<style>
.aktif { font-weight: bold; background: #3498db; color: white; }
</style>
```
