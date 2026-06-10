# 126: State Management Lanjutan — Pinia Patterns

## 1. Penjelasan
Pinia adalah state management untuk Vue yang menjadi rekomendasi resmi setelah Vuex. Pinia mendukung dua pola: Options Store (mirip Vuex — state, getters, actions) dan Setup Store (mirip Composition API — ref, computed, function). Pinia juga mendukung plugin untuk persistensi, logging, dll.

## 2. Fungsi
- Menyimpan state global yang diakses banyak komponen
- Computed getters untuk derived state otomatis
- Async actions untuk operasi side-effect (API call)
- Plugin persistensi agar state tetap ada setelah reload
- TypeScript friendly dengan inferensi tipe otomatis

## 3. Code

```ts
// stores/todo.ts — Setup Store pattern
export const useTodoStore = defineStore('todo', () => {
  const todos = ref<Todo[]>([])
  const filter = ref<'all' | 'active' | 'completed'>('all')

  const filteredTodos = computed(() => {
    if (filter.value === 'active') return todos.value.filter(t => !t.done)
    if (filter.value === 'completed') return todos.value.filter(t => t.done)
    return todos.value
  })

  const stats = computed(() => ({
    total: todos.value.length,
    active: todos.value.filter(t => !t.done).length,
    completed: todos.value.filter(t => t.done).length
  }))

  async function addTodo(text: string) {
    todos.value.push({ id: Date.now(), text, done: false })
  }

  function toggleTodo(id: number) {
    const todo = todos.value.find(t => t.id === id)
    if (todo) todo.done = !todo.done
  }

  return { todos, filter, filteredTodos, stats, addTodo, toggleTodo }
})
```

```ts
// nuxt.config.ts — plugin persist
export default defineNuxtConfig({
  modules: ['@pinia/nuxt', '@pinia-plugin-persistedstate/nuxt']
})
```

```ts
// stores/todo.ts — dengan persist
export const useTodoStore = defineStore('todo', () => {
  // ... state & actions
  return { todos, filter, filteredTodos, stats, addTodo, toggleTodo }
}, {
  persist: {
    key: 'perpustakaan-todo',
    storage: persistedState.localStorage
  }
})
```

## 4. Analogi Rumah

| Konsep Pinia | Analogi Rumah |
|--------------|---------------|
| Store | Panel kontrol utama rumah |
| State | Saklar lampu saat ini (posisi on/off) |
| Getter | Kalkulasi tagihan listrik otomatis |
| Action | Remote control — tekan tombol, lampu berubah |
| Plugin persist | Panel yang ingat pengaturan terakhir meski listrik mati |
| Options Store | Panel dengan buku panduan (options API) |
| Setup Store | Panel modern tanpa buku (Composition API) |

## 5. Use Case
- Todo app dengan filter dan statistik
- Keranjang belanja dengan persistensi
- User preferences (tema, bahasa) yang tetap setelah reload
- Aplikasi perpustakaan: state buku sedang dipinjam, filter kategori, riwayat peminjaman

## 6. Kesalahan Umum
- **Mutasi state tanpa action** → Pinia mengizinkan langsung mutate (tidak seperti Vuex), tapi tetap disarankan lewat action agar terstruktur.
- **Lupa setup persist** → state hilang saat refresh. Gunakan `@pinia-plugin-persistedstate/nuxt` untuk Nuxt.
- **Getter terlalu kompleks** → getter harus ringan tanpa side-effect. Pindahkan logika berat ke action atau utility function.

## 7. Benang Merah
Nuxt (125) menyediakan framework. Pinia mengelola state di dalamnya. State yang baik diperlukan untuk Web Performance (127) — caching data, dan PWA (128) — offline state. Pola Pinia juga mirip dengan state di micro-frontend (131).

## 8. Soal

**Soal 1:** Apa perbedaan Options Store vs Setup Store di Pinia?

**Soal 2:** Bagaimana cara mempertahankan state Pinia setelah browser di-refresh?

**Soal 3:** Kapan sebaiknya menggunakan getter daripada computed biasa di komponen?

---
<details>
<summary>Jawaban</summary>

**Jawaban 1:** Options Store menggunakan objek `state`, `getters`, `actions` (mirip Vuex). Setup Store menggunakan fungsi yang mengembalikan ref/reactive (mirip Composition API). Setup Store lebih fleksibel dan mendukung closure.

**Jawaban 2:** Gunakan plugin persistensi (`@pinia-plugin-persistedstate/nuxt`) dengan konfigurasi `persist: { storage: persistedState.localStorage }` pada store.

**Jawaban 3:** Getter digunakan ketika derived state dibutuhkan oleh banyak komponen berbeda (global). Computed di komponen cukup jika hanya dipakai di satu komponen (lokal).
</details>
