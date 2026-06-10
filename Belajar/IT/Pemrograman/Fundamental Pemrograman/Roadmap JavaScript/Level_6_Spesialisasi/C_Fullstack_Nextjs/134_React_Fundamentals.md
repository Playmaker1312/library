# 134: React Fundamentals — Dari Vue ke React

## 1. Penjelasan

React adalah library UI berbasis komponen yang dikembangkan Facebook. Berbeda dengan Vue yang merupakan framework _progressive_ dengan banyak built-in feature, React lebih minimal — ia hanya mengurus _view layer_. Sisanya (routing, state management global, HTTP client) ditambahkan lewat library eksternal.

**React vs Vue — Perbedaan Filosofi:**

| Aspek | Vue | React |
|-------|-----|-------|
| Approach | Framework progresif | Library UI |
| Template | HTML-based (`.vue`) | JSX (JavaScript + HTML) |
| Reactivity | Proxy-based (otomatis) | Explicit (`useState`, `setState`) |
| Bentuk komponen | Options API / Composition API | Function Component + Hooks |
| Data binding | `v-model` (two-way) | Controlled components (one-way) |
| Logic reuse | Composables / Mixins | Custom Hooks |
| Learning curve | Landai — banyak kemudahan built-in | Curam — butuh keputusan arsitektur |

## 2. Fungsi

- **JSX**: Syntax extension JavaScript yang memungkinkan HTML ditulis di dalam JS.
- **Component**: Function yang mengembalikan JSX — blok bangunan UI.
- **Hooks**: `useState` (state lokal), `useEffect` (side effect), `useRef` (referensi DOM).
- **Props**: Data dari parent ke child (read-only).
- **State**: Data internal komponen yang bisa berubah.
- **Conditional rendering**: Menampilkan UI berdasarkan kondisi (`&&`, ternary, `if`).

## 3. Code — Todo List Vue → React (Side-by-Side)

### Vue (Options API)

```vue
<template>
  <div>
    <input v-model="newTodo" @keyup.enter="addTodo" />
    <ul>
      <li v-for="(todo, i) in todos" :key="i">
        {{ todo }}
        <button @click="removeTodo(i)">Hapus</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return { newTodo: '', todos: [] }
  },
  methods: {
    addTodo() {
      this.todos.push(this.newTodo)
      this.newTodo = ''
    },
    removeTodo(i) {
      this.todos.splice(i, 1)
    }
  }
}
</script>
```

### React (Function Component + Hooks)

```jsx
import { useState } from 'react'

export default function TodoApp() {
  const [newTodo, setNewTodo] = useState('')
  const [todos, setTodos] = useState([])

  const addTodo = () => {
    setTodos([...todos, newTodo])
    setNewTodo('')
  }

  const removeTodo = (i) => {
    setTodos(todos.filter((_, idx) => idx !== i))
  }

  return (
    <div>
      <input
        value={newTodo}
        onChange={(e) => setNewTodo(e.target.value)}
        onKeyDown={(e) => e.key === 'Enter' && addTodo()}
      />
      <ul>
        {todos.map((todo, i) => (
          <li key={i}>
            {todo}
            <button onClick={() => removeTodo(i)}>Hapus</button>
          </li>
        ))}
      </ul>
    </div>
  )
}
```

## 4. Analogi Rumah

| Konsep React | Analogi Rumah |
|-------------|---------------|
| React (library) | Rumah kosong tanpa isi |
| Vue (framework) | Rumah dengan furnitur built-in (lemari, lampu, karpet) |
| JSX | Cetak biru — gambar dinding, pintu, jendela |
| Component | Ruangan (kamar, dapur, ruang tamu) |
| Props | Aliran listrik dari panel utama ke setiap ruangan |
| State | Suhu AC dalam satu ruangan — lokal, bisa berubah |
| Hooks (`useState`) | Saklar lampu — Anda bisa menyalakan/mematikan |
| Hooks (`useEffect`) | Alarm kebakaran — otomatis aktif saat ada perubahan asap |
| Hooks (`useRef`) | Meteran listrik akses langsung — baca tanpa re-render |
| Custom Hooks | Alat pertukangan yang bisa dipakai di ruangan mana pun |
| Library tambahan | Beli furnitur sendiri (lemari dari IKEA, lampu dari ACE) |

## 5. Use Case

- **Dashboard interaktif**: Banyak data real-time yang perlu di-render efisien.
- **Aplikasi SPA**: Single Page Application dengan routing dinamis.
- **Mobile app** (React Native): Codebase React bisa di-port ke mobile.
- **Proyek tim besar**: Ekosistem library yang fleksibel memungkinkan tim memilih stack sendiri.

## 6. Kesalahan Umum

| Kesalahan | Seharusnya |
|-----------|-----------|
| Mutasi state langsung (`todos.push()`) | `setTodos([...todos, newItem])` — immutability |
| Panggil hooks di dalam loop/kondisi | Hooks harus dipanggil di top-level komponen (Rules of Hooks) |
| Lupa dependency array di `useEffect` | Beri `[]` untuk sekali jalan, atau isi dependensi yang relevan |
| Over-fetching dengan `useEffect` + fetch manual | Gunakan library seperti React Query atau SWR |
| Menganggap React seperti Vue (two-way binding) | React one-way — gunakan `value` + `onChange` |

## 7. Benang Merah

Level 4 (Vue) → **134 React Foundation** — Berpindah dari framework serba-ada ke library minimalis yang butuh keputusan sendiri. Ini jembatan menuju 135 (Next.js) di mana React diperkuat dengan framework full-stack.

## 8. Soal

### Soal 1 — JSX
Apa output dari kode berikut?
```jsx
function Greeting({ name }) {
  return <h1>Halo, {name.toUpperCase()}</h1>
}
```
**Jawaban:** `<h1>Halo, BUDI</h1>` — asalkan `name` adalah string, `.toUpperCase()` akan dijalankan di JSX.

### Soal 2 — State vs Props
Mengapa kita tidak boleh mengubah props di dalam child component?
**Jawaban:** Props bersifat read-only. Jika child bisa mengubah props, aliran data jadi tidak terprediksi (unidirectional flow rusak). Untuk data yang bisa berubah, gunakan state di parent, lalu turunkan sebagai props.

### Soal 3 — Conditional Rendering
Perbaiki kode berikut:
```jsx
function Alert({ error }) {
  if (error) {
    return <div>Error: {error}</div>
  }
}
```
**Jawaban:** Saat `error` falsy, komponen tidak mengembalikan apapun (undefined) — React akan error. Tambahkan `return null`:
```jsx
function Alert({ error }) {
  if (error) return <div>Error: {error}</div>
  return null
}
```
