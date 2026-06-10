# 87. TypeScript di Vue.js

**Benang Merah**: Materi 86 (backend TS) + Vue (Level 4). Sekarang backend dan frontend sama-sama type-safe. Lanjut ke Materi 88 (Clean Code).

---

## Penjelasan

Vue 3 dengan **Composition API** + TypeScript memberikan **type safety dari ujung ke ujung** — dari data, props, emits, hingga computed properties. Komponen menjadi **self-documenting**: Anda tahu persis bentuk props yang diterima dan events yang dikeluarkan tanpa harus membaca dokumentasi terpisah.

### Setup Vue + Vite + TypeScript
```bash
npm create vite@latest perpustakaan-frontend -- --template vue-ts
cd perpustakaan-frontend
npm install
npm install axios
```

### Type-safe Props
```typescript
// ❌ Vue 2 JS — props tanpa tipe
props: {
  judul: String,
  harga: Number
}

// ✅ Vue 3 TS — props dengan tipe
interface CardBukuProps {
  judul: string;
  penulis: string;
  tersedia: boolean;
}

const props = defineProps<CardBukuProps>();
```

### Type-safe Emits
```typescript
// ❌ JS — tebak-tebak nama event & payload
this.$emit("pilih", id);

// ✅ TS — event sudah di-definisi
interface CardBukuEmits {
  (e: "pilih-buku", id: number): void;
  (e: "hapus-buku", id: number): void;
}

const emit = defineEmits<CardBukuEmits>();
emit("pilih-buku", 1); // ✅ auto-complete!
```

### Typing ref() dan reactive()
```typescript
const daftarBuku = ref<Buku[]>([]);
const loading = ref<boolean>(true);
const form = reactive<CreateBukuBody>({
  judul: "",
  penulis: "",
  isbn: "",
  tahunTerbit: new Date().getFullYear()
});
```

---

## Fungsi

Mencegah bug di frontend — props yang salah, event typo, akses data yang undefined — semuanya tertangkap **sebelum runtime**.

---

## Code

```typescript
// src/types/index.ts
export type StatusAnggota = "aktif" | "nonaktif" | "diblokir";

export interface Buku {
  id: number;
  judul: string;
  penulis: string;
  isbn: string;
  tahunTerbit: number;
  tersedia: boolean;
}

export interface Anggota {
  id: number;
  nama: string;
  alamat: string;
  noTelepon: string;
  status: StatusAnggota;
  tanggalDaftar: Date;
}

export interface Peminjaman {
  id: number;
  anggotaId: number;
  bukuId: number;
  tanggalPinjam: Date;
  tanggalKembali?: Date;
  denda?: number;
}

export interface ApiResponse<T> {
  status: "success" | "error";
  data: T;
  message?: string;
}
```

```vue
<!-- src/components/CardBuku.vue -->
<script setup lang="ts">
import type { Buku } from "../types";

interface CardBukuEmits {
  (e: "pilih-buku", id: number): void;
  (e: "hapus-buku", id: number): void;
}

const props = defineProps<{
  buku: Buku;
}>();

const emit = defineEmits<CardBukuEmits>();

function formatTahun(tahun: number): string {
  return `(${tahun})`;
}
</script>

<template>
  <div class="card" :class="{ tersedia: buku.tersedia }">
    <h3>{{ buku.judul }} <span class="tahun">{{ formatTahun(buku.tahunTerbit) }}</span></h3>
    <p class="penulis">{{ buku.penulis }}</p>
    <p class="isbn">ISBN: {{ buku.isbn }}</p>
    <p class="status">
      {{ buku.tersedia ? "✓ Tersedia" : "✗ Dipinjam" }}
    </p>
    <div class="aksi">
      <button :disabled="!buku.tersedia" @click="emit('pilih-buku', buku.id)">
        Pinjam
      </button>
      <button class="hapus" @click="emit('hapus-buku', buku.id)">
        Hapus
      </button>
    </div>
  </div>
</template>
```

```vue
<!-- src/App.vue -->
<script setup lang="ts">
import { ref, onMounted } from "vue";
import type { Buku, ApiResponse } from "./types";
import CardBuku from "./components/CardBuku.vue";

const daftarBuku = ref<Buku[]>([]);
const loading = ref<boolean>(true);
const error = ref<string | null>(null);

async function fetchBuku(): Promise<void> {
  try {
    loading.value = true;
    error.value = null;
    const res = await fetch("http://localhost:3000/api/buku");
    const data: ApiResponse<Buku[]> = await res.json();
    if (data.status === "success") {
      daftarBuku.value = data.data;
    } else {
      error.value = data.message ?? "Gagal memuat data";
    }
  } catch (err) {
    error.value = "Gagal terhubung ke server";
  } finally {
    loading.value = false;
  }
}

function handlePilihBuku(id: number): void {
  alert(`Buku ID ${id} dipilih untuk dipinjam`);
}

function handleHapusBuku(id: number): void {
  daftarBuku.value = daftarBuku.value.filter(b => b.id !== id);
}

onMounted(fetchBuku);
</script>

<template>
  <div class="app">
    <h1>📚 Perpustakaan — TypeScript Vue</h1>

    <div v-if="loading" class="loading">Memuat data...</div>
    <div v-else-if="error" class="error">{{ error }}</div>
    <div v-else class="grid">
      <CardBuku
        v-for="buku in daftarBuku"
        :key="buku.id"
        :buku="buku"
        @pilih-buku="handlePilihBuku"
        @hapus-buku="handleHapusBuku"
      />
    </div>
  </div>
</template>
```

```typescript
// src/api/perpustakaan.ts — Type-safe API client
import type { Buku, Anggota, Peminjaman, ApiResponse } from "../types";

const BASE_URL = "http://localhost:3000/api";

async function request<T>(url: string, options?: RequestInit): Promise<T> {
  const res = await fetch(`${BASE_URL}${url}`, {
    headers: { "Content-Type": "application/json" },
    ...options,
  });
  const data: ApiResponse<T> = await res.json();
  if (data.status === "error") throw new Error(data.message);
  return data.data;
}

export const api = {
  getBuku: () => request<Buku[]>("/buku"),
  getBukuById: (id: number) => request<Buku>(`/buku/${id}`),
  createBuku: (buku: Omit<Buku, "id" | "tersedia">) =>
    request<Buku>("/buku", { method: "POST", body: JSON.stringify(buku) }),
  updateBuku: (id: number, buku: Partial<Buku>) =>
    request<Buku>(`/buku/${id}`, { method: "PUT", body: JSON.stringify(buku) }),
  deleteBuku: (id: number) =>
    request<null>(`/buku/${id}`, { method: "DELETE" }),
};
```

---

## Analogi: Membangun Rumah (Komponen dengan Instruksi Perakitan)

| TypeScript Vue | Analogi Rumah |
|---|---|
| `defineProps<T>()` | Daftar material yang dibutuhkan komponen |
| `defineEmits<T>()` | Kabel yang menghubungkan komponen |
| `ref<T>()` | Kotak penyimpanan dengan label tipe isi |
| `reactive<T>()` | Papan kontrol dengan tombol berlabel |
| `computed` | Alat ukur otomatis — hasil selalu akurat |
| Template type-checked | Cetak biru ruangan — semua ukuran pasti |

Bayangkan komponen rumah seperti **lemari IKEA**. Setiap lemari punya **instruksi perakitan** yang jelas: "Baut M8 × 4 buah, Sekrup 5cm × 8 buah, Panel samping 60×180cm × 2". TypeScript Props dan Emits persis seperti itu — **instruksi yang jelas** tentang apa yang masuk (props) dan apa yang keluar (emits). Tukang tidak perlu menebak "ini pake baut gede atau kecil?"

---

## Use Case

- **Component library** — komponen yang dipakai tim besar
- **Form complex** — typing form state & validation
- **Dashboard data** — typing data dari API
- **Large-scale SPA** — puluhan komponen dengan props kompleks

---

## Kesalahan Umum

| Kesalahan | Contoh | Akibat |
|---|---|---|
| Tidak install `vue-tsc` | Hanya pakai `tsc` | Type checking tidak optimal |
| `defineProps` tanpa tipe | `defineProps(['judul'])` | Kehilangan type safety |
| Emits tidak di-typed | `emit("apa-aja")` | Typo event tidak terdeteksi |
| `ref` tanpa generic | `ref([])` → inferred `never[]` | error saat push data |
| Lupa `lang="ts"` di script | `<script setup>` default JS | Tidak ada type checking |

---

## Benang Merah

Materi 86 (backend TS) + Vue (Level 4) → **Materi 87 (TS di Vue)** → Materi 88 (Clean Code)

Seluruh stack Anda — dari database → Express API → Vue frontend — sekarang diamankan TypeScript. Tidak ada lagi "cannot read property of undefined" di production. Materi selanjutnya: **Clean Code** — bagaimana menulis kode yang tidak hanya benar secara tipe, tapi juga **bersih dan mudah dibaca**.

---

## Soal Latihan

### Soal 1 (Mudah)
Buat komponen Vue `UserAvatar` dengan props: `nama` (string), `ukuran` (number, default 40), `online` (boolean, default false). Gunakan TypeScript `defineProps`.

**Jawaban**:
```vue
<script setup lang="ts">
interface Props {
  nama: string;
  ukuran?: number;
  online?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  ukuran: 40,
  online: false,
});
</script>

<template>
  <div class="avatar" :style="{ width: ukuran + 'px', height: ukuran + 'px' }">
    <span>{{ nama[0].toUpperCase() }}</span>
    <span v-if="online" class="dot" />
  </div>
</template>
```

### Soal 2 (Sedang)
Buat komponen `TodoList` dengan emits: `tambah(text: string)`, `hapus(id: number)`, `toggle(id: number)`. Gunakan TypeScript emits. Sertakan state `todos` dengan tipe `{ id: number; text: string; selesai: boolean }[]`.

**Jawaban**:
```vue
<script setup lang="ts">
import { ref } from "vue";

interface TodoItem {
  id: number;
  text: string;
  selesai: boolean;
}

interface TodoListEmits {
  (e: "tambah", text: string): void;
  (e: "hapus", id: number): void;
  (e: "toggle", id: number): void;
}

const todos = ref<TodoItem[]>([]);
const inputText = ref("");
const emit = defineEmits<TodoListEmits>();
let nextId = 1;

function tambahTodo(): void {
  if (!inputText.value.trim()) return;
  const todo: TodoItem = { id: nextId++, text: inputText.value, selesai: false };
  todos.value.push(todo);
  emit("tambah", inputText.value);
  inputText.value = "";
}
</script>
```

### Soal 3 (Tantangan)
Buat composable `useApi<T>(url: string)` yang mengembalikan reactive state `{ data: T | null, loading: boolean, error: string | null, refresh: () => Promise<void> }` dengan type safety penuh. Implementasikan dengan generics.

**Jawaban**:
```typescript
// src/composables/useApi.ts
import { ref, type Ref } from "vue";

interface UseApiReturn<T> {
  data: Ref<T | null>;
  loading: Ref<boolean>;
  error: Ref<string | null>;
  refresh: () => Promise<void>;
}

export function useApi<T>(url: string): UseApiReturn<T> {
  const data = ref<T | null>(null) as Ref<T | null>;
  const loading = ref<boolean>(false);
  const error = ref<string | null>(null);

  async function refresh(): Promise<void> {
    loading.value = true;
    error.value = null;
    try {
      const res = await fetch(url);
      const json = await res.json();
      data.value = json as T;
    } catch (err) {
      error.value = err instanceof Error ? err.message : "Unknown error";
    } finally {
      loading.value = false;
    }
  }

  return { data, loading, error, refresh };
}

// Penggunaan di komponen:
// const { data: buku, loading, error, refresh } = useApi<Buku[]>("/api/buku");
// onMounted(refresh);
```
