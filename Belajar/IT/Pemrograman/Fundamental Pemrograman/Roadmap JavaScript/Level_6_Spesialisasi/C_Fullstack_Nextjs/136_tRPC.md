# 136: tRPC — Type-Safe API Tanpa Schema

## 1. Penjelasan

tRPC (TypeScript Remote Procedure Call) memungkinkan Anda memanggil fungsi backend dari frontend **seolah-olah kode yang sama** — tanpa REST endpoint, tanpa GraphQL schema, tanpa code generation. Semua tipe di-infer otomatis oleh TypeScript.

Cara kerja: Backend mendefinisikan router (kumpulan prosedur). Frontend meng-import tipe router dan memanggilnya langsung. TypeScript memastikan parameter dan return type cocok.

## 2. Fungsi

- **End-to-end type safety**: Satu sumber kebenaran tipe — dari DB schema ke frontend.
- **No schema language**: Tidak perlu belajar GraphQL SDL atau OpenAPI.
- **Integrasi Next.js**: Bisa dipasang sebagai API Route handler.
- **Prosedur**: Query (GET, cacheable) dan Mutation (POST, data berubah).
- **Middleware tRPC**: Auth, logging, validasi — di level router/prosedur.

## 3. Code — Todo App dengan tRPC + Next.js

### Backend — Router

```tsx
// server/trpc.ts
import { initTRPC } from '@trpc/server'
import { z } from 'zod'

const t = initTRPC.create()

export const router = t.router
export const publicProcedure = t.procedure
```

```tsx
// server/routers/todo.ts
import { router, publicProcedure } from '../trpc'
import { z } from 'zod'

interface Todo {
  id: string
  text: string
  selesai: boolean
}
const todos: Todo[] = []

export const todoRouter = router({
  getAll: publicProcedure.query(() => todos),

  create: publicProcedure
    .input(z.object({ text: z.string() }))
    .mutation(({ input }) => {
      const todo: Todo = {
        id: String(todos.length + 1),
        text: input.text,
        selesai: false,
      }
      todos.push(todo)
      return todo
    }),

  toggle: publicProcedure
    .input(z.object({ id: z.string() }))
    .mutation(({ input }) => {
      const todo = todos.find((t) => t.id === input.id)
      if (todo) todo.selesai = !todo.selesai
      return todo
    }),
})
```

```tsx
// server/_app.ts
import { router } from './trpc'
import { todoRouter } from './routers/todo'

export const appRouter = router({
  todo: todoRouter,
})

export type AppRouter = typeof appRouter
```

### Next.js API Handler

```tsx
// app/api/trpc/[trpc]/route.ts
import { fetchRequestHandler } from '@trpc/server/adapters/fetch'
import { appRouter } from '@/server/_app'

const handler = (req: Request) =>
  fetchRequestHandler({
    endpoint: '/api/trpc',
    req,
    router: appRouter,
    createContext: () => ({}),
  })

export { handler as GET, handler as POST }
```

### Frontend — Client

```tsx
// lib/trpc.ts
import { createTRPCReact } from '@trpc/react-query'
import type { AppRouter } from '@/server/_app'

export const trpc = createTRPCReact<AppRouter>()
```

```tsx
// app/todo/page.tsx
'use client'
import { trpc } from '@/lib/trpc'
import { useState } from 'react'

export default function TodoPage() {
  const [text, setText] = useState('')
  const utils = trpc.useUtils()
  const { data: todos } = trpc.todo.getAll.useQuery()
  const createMutation = trpc.todo.create.useMutation({
    onSuccess: () => utils.todo.getAll.invalidate(),
  })
  const toggleMutation = trpc.todo.toggle.useMutation({
    onSuccess: () => utils.todo.getAll.invalidate(),
  })

  return (
    <div>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <button onClick={() => createMutation.mutate({ text })}>Tambah</button>
      <ul>
        {todos?.map((t) => (
          <li key={t.id} onClick={() => toggleMutation.mutate({ id: t.id })}>
            {t.selesai ? '✓' : '○'} {t.text}
          </li>
        ))}
      </ul>
    </div>
  )
}
```

## 4. Analogi Rumah

| Konsep tRPC | Analogi Rumah |
|-------------|---------------|
| tRPC | Telepon internal rumah — pencet ekstensi, langsung nyambung |
| REST/GraphQL | Telepon via operator — "Saya mau bicara dengan si A", operator menghubungkan |
| Router | Papan telepon yang daftar ekstensi (ext. 101 = dapur, ext. 102 = kamar) |
| Procedure (query) | "Berapa suhu di kamar?" — baca data |
| Procedure (mutation) | "Nyalakan AC di kamar" — ubah data |
| Input validation (Zod) | Filter panggilan — cek nomor ekstensi valid sebelum nyambung |
| Type safety | Label di setiap tombol ekstensi — tidak mungkin salah sambung |
| Tanpa schema | Tidak perlu buku telepon terpisah — nomornya built-in di telepon |
| `useQuery` / `useMutation` | Tombol "Tanya" dan "Ubah" di telepon |

## 5. Use Case

- **Aplikasi full-stack TypeScript**: Satu bahasa, satu tipe — dari DB ke UI.
- **Internal tools**: Cepat prototyping tanpa bolak-balik definisi API.
- **Monorepo**: Backend dan frontend di satu repo — tipe dishare langsung.
- **Migrasi REST ke tRPC**: Kurangi boilerplate kode API manual.

## 6. Kesalahan Umum

| Kesalahan | Seharusnya |
|-----------|-----------|
| Tidak invalidate query setelah mutation | Panggil `utils.<query>.invalidate()` agar data tetap sinkron |
| Context berat (koneksi DB di setiap request) | Context hanya untuk auth ringan. Koneksi DB via middleware terpisah |
| Lupa Zod validation — input bisa apa saja | Selalu gunakan `.input(z.object(...))` untuk keamanan tipe |
| Menaruh logic bisnis di router | Pisahkan service layer — router hanya sebagai entry point |
| Tidak mengekspor `AppRouter` type | Client butuh type untuk infer — export `typeof appRouter` |

## 7. Benang Merah

135 (Next.js) → **136 tRPC** — API jadi lebih aman dan cepat tanpa REST/GraphQL. Lanjut ke 137 (Serverless) untuk deployment tanpa kelola server.

## 8. Soal

### Soal 1 — Query vs Mutation
Mengapa `getAll` menggunakan `.query()` bukan `.mutation()`?
**Jawaban:** Query untuk operasi baca (GET) yang cacheable dan idempoten. Mutation untuk operasi tulis (POST/PUT/DELETE) yang mengubah data.

### Soal 2 — Type Safety
Apa yang terjadi jika client memanggil `trpc.todo.create.useMutation()` tanpa argumen `text`?
**Jawaban:** TypeScript akan error saat kompilasi — tRPC meng-infer input type dari Zod schema. Argumen `text: string` wajib.

### Soal 3 — Invalidate
Mengapa setelah `createMutation` sukses kita panggil `utils.todo.getAll.invalidate()`?
**Jawaban:** Karena data todo berubah (bertambah). Invalidate memberi tahu React Query bahwa data lama sudah basi, sehingga refetch data baru.
