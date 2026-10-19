# 🗺 Roadmap Next.js: Step-by-Step Membangun Full-Stack Applications

## Filosofi Roadmap Ini

"Next.js adalah React framework yang menghilangkan kompleksitas dan memberikan struktur — dari file-based routing hingga server-side rendering, setiap fitur ada untuk memecahkan masalah nyata"

---

## Prinsip Desain

- **Satu Project, Tumbuh Bersama**: Perpustakaan dari static pages → full-stack app → API → advanced patterns
- **Full-Stack Mindset**: Frontend dan backend dalam satu project
- **File-Based Routing**: Think about file structure, not route configuration
- **Modern Next.js**: Fokus pada App Router (bukan Pages Router lagi), Server Components, Streaming
- **Convention Over Configuration**: Less boilerplate, more productivity

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: "Next.js Fundamentals" — Setup, Pages, Routing, Static Generation
 ↓ (enhance)
Level 2: + Dynamic Routes, Layouts, Styling → Structured pages
 ↓ (enhance)
Level 3: + API Routes, Database → Full-stack app
 ↓ (enhance)
Level 4: + Server Components, Server Actions → Modern Next.js patterns
 ↓ (enhance)
Level 5: + Image Optimization, Performance → Production-ready
 ↓ (enhance)
Level 6: + Authentication, Advanced patterns → Enterprise features
 ↓ (enhance)
Level 7: + Deployment, Monitoring, Scaling → Production operations
 ↓ (choose path)
Level 8: Specialized (E-commerce, SaaS, Headless CMS)
```

---

# 🟢 LEVEL 1: NEXT.JS FUNDAMENTALS (Minggu 1-3)

**Tema**: "File-based routing, pages, static generation — Cara Next.js yang benar"

**Benang Merah**: Create Next App → File structure → Routing → Static Generation → Deployment

**Output**: Static website perpustakaan yang di-deploy

---

## A. Setup dan Konsep Dasar

### 1. [[1. Mengapa Next.js dan Perbedaan dari React]]

Next.js adalah **React framework** yang menyediakan:

- ✅ File-based routing (bukan React Router config)
- ✅ Server-side rendering dan static generation built-in
- ✅ API routes (backend di Next.js)
- ✅ Optimized images dan fonts
- ✅ Built-in development server dan production builds
- ✅ Edge functions dan middleware
- ✅ Automatic code splitting dan optimization

**Perbedaan Next.js vs React SPA:**

|Aspek|React SPA|Next.js|
|---|---|---|
|Routing|React Router (config)|File-based (file system)|
|Rendering|Client-side (CSR)|Multiple: SSR, SSG, ISR, CSR|
|Backend|Terpisah (Node/Express)|Built-in API routes|
|SEO|Sulit (client-rendered)|Mudah (server-rendered)|
|Initial Load|Lambat (download JS)|Cepat (pre-rendered HTML)|
|Deployment|Frontend hosting|Full-stack hosting|

**Langkah konkret**: Understand kapan pakai React vs Next.js.

### 2. [[2. Create Next.js Project dan Project Structure]]

Bash

```
# Create Next.js app dengan TypeScript (recommended)
npx create-next-app@latest perpustakaan-nextjs --typescript --tailwind --app

# Navigate to project
cd perpustakaan-nextjs

# Start development server
npm run dev

# Visit http://localhost:3000
```

**Project Structure (App Router):**

text

```
perpustakaan-nextjs/
├── app/                    # App directory (new App Router)
│   ├── layout.tsx         # Root layout
│   ├── page.tsx           # Home page (/)
│   ├── globals.css        # Global styles
│   ├── (catalog)/         # Route group (tidak di URL)
│   │   ├── layout.tsx     # Catalog layout
│   │   ├── page.tsx       # /catalog
│   │   └── [id]/
│   │       └── page.tsx   # /catalog/[id]
│   ├── api/               # API routes
│   │   └── books/
│   │       └── route.ts   # /api/books
│   └── dashboard/
│       ├── layout.tsx
│       └── page.tsx       # /dashboard
├── components/            # Reusable components
├── lib/                   # Utility functions
├── public/                # Static assets
├── package.json
├── next.config.js         # Next.js configuration
├── tsconfig.json
└── tailwind.config.js
```

**Langkah konkret**: Setup project, explore folder structure, run dev server.

### 3. [[3. Pages dan File-Based Routing]]

Next.js menggunakan file system untuk routing — **tidak perlu mendefinisikan routes di config**.

text

```
Struktur File                  →  URL Route
─────────────────────────────────────────────
app/page.tsx                  →  /
app/about/page.tsx            →  /about
app/catalog/page.tsx          →  /catalog
app/catalog/[id]/page.tsx     →  /catalog/123
app/admin/settings/page.tsx   →  /admin/settings
```

**File: app/page.tsx (Home page)**

React

```
export default function Home() {
    return (
        <main>
            <h1>Selamat datang di Perpustakaan Digital</h1>
            <p>Temukan buku favorit Anda</p>
        </main>
    );
}
```

**File: app/about/page.tsx**

React

```
export default function About() {
    return (
        <div>
            <h1>Tentang Perpustakaan</h1>
            <p>Perpustakaan digital terlengkap...</p>
        </div>
    );
}
```

**File: app/catalog/page.tsx**

React

```
export default function Catalog() {
    return (
        <div>
            <h1>Katalog Buku</h1>
            <p>Jelajahi koleksi buku kami</p>
        </div>
    );
}
```

**Dynamic Routes dengan brackets:**

React

```
// app/catalog/[id]/page.tsx
interface CatalogDetailProps {
    params: {
        id: string;
    };
}

export default function CatalogDetail({ params }: CatalogDetailProps) {
    return (
        <div>
            <h1>Buku ID: {params.id}</h1>
        </div>
    );
}

// URL: /catalog/123 → params.id = "123"
// URL: /catalog/clean-code → params.id = "clean-code"
```

**Langkah konkret**: Create 5-7 pages dengan berbagai routing patterns.

### 4. [[4. Layouts — Struktur Halaman Konsisten]]

Layouts di Next.js automatically wrap child pages tanpa re-render (penting untuk state preservation).

React

```
// app/layout.tsx (Root Layout)
import type { Metadata } from 'next';
import './globals.css';

export const metadata: Metadata = {
    title: 'Perpustakaan Digital',
    description: 'Sistem manajemen perpustakaan online',
};

export default function RootLayout({
    children,
}: {
    children: React.ReactNode;
}) {
    return (
        <html lang="id">
            <body>
                <header>
                    <nav>
                        <a href="/">Home</a>
                        <a href="/catalog">Katalog</a>
                        <a href="/about">Tentang</a>
                    </nav>
                </header>
                
                <main>
                    {children}  {/* Page content dirender di sini */}
                </main>
                
                <footer>
                    <p>© 2024 Perpustakaan Digital</p>
                </footer>
            </body>
        </html>
    );
}
```

**Nested Layouts (layout sharing):**

React

```
// app/catalog/layout.tsx
export default function CatalogLayout({
    children,
}: {
    children: React.ReactNode;
}) {
    return (
        <div className="catalog-layout">
            <aside className="sidebar">
                <h3>Filter</h3>
                <ul>
                    <li><a href="/catalog?category=tech">Teknologi</a></li>
                    <li><a href="/catalog?category=fiction">Fiksi</a></li>
                </ul>
            </aside>
            
            <section className="content">
                {children}
            </section>
        </div>
    );
}

// Struktur:
// /catalog        → CatalogLayout + CatalogPage
// /catalog/123    → CatalogLayout + CatalogDetail
```

**Route Groups (untuk organization tanpa URL impact):**

text

```
app/
├── (marketing)/       # Tidak di URL, hanya untuk grouping
│   ├── layout.tsx     # Marketing layout (header, footer khusus)
│   ├── page.tsx       # /
│   └── about/
│       └── page.tsx   # /about
├── (dashboard)/       # Admin area layout terpisah
│   ├── layout.tsx     # Dashboard layout
│   ├── page.tsx       # /dashboard
│   └── settings/
│       └── page.tsx   # /dashboard/settings
└── api/

// Route groups memungkinkan multiple root layouts!
// (marketing) punya layout sendiri, (dashboard) punya layout berbeda
```

**Langkah konkret**: Create layouts untuk different sections.

### 5. [[5. Static Generation (SSG) dan ISR]]

**Static Generation**: HTML dibuild saat build time (tercepat, best for SEO).

React

```
// app/catalog/page.tsx
import Link from 'next/link';

interface Book {
    id: number;
    judul: string;
    pengarang: string;
}

async function getBooks(): Promise<Book[]> {
    // Ini berjalan di server saat build time
    const res = await fetch('https://api.example.com/books', {
        // Revalidate setiap 60 detik
        next: { revalidate: 60 }
    });
    
    if (!res.ok) {
        throw new Error('Failed to fetch books');
    }
    
    return res.json();
}

export default async function CatalogPage() {
    const books = await getBooks();

    return (
        <div>
            <h1>Katalog Buku</h1>
            <div className="grid">
                {books.map(book => (
                    <Link key={book.id} href={`/catalog/${book.id}`}>
                        <div className="card">
                            <h3>{book.judul}</h3>
                            <p>{book.pengarang}</p>
                        </div>
                    </Link>
                ))}
            </div>
        </div>
    );
}
```

**Incremental Static Regeneration (ISR):**

React

```
// app/blog/[slug]/page.tsx
export const revalidate = 3600; // Revalidate setiap 1 jam

export async function generateStaticParams() {
    // Pre-generate halaman untuk slugs ini saat build
    return [
        { slug: 'belajar-nextjs' },
        { slug: 'golang-tutorial' },
        { slug: 'react-best-practices' },
    ];
}

async function getPost(slug: string) {
    const res = await fetch(`https://api.example.com/posts/${slug}`);
    return res.json();
}

export default async function PostPage({
    params,
}: {
    params: { slug: string };
}) {
    const post = await getPost(params.slug);

    return (
        <article>
            <h1>{post.title}</h1>
            <p>{post.content}</p>
        </article>
    );
}
```

**Langkah konkret**: Setup static generation untuk catalog pages.

---

## B. Styling dan Components

### 6. [[6. Styling di Next.js (Tailwind CSS Recommended)]]

Next.js support berbagai styling approach.

React

```
// 1. Tailwind CSS (recommended) — sudah setup di create-next-app
// app/components/BukuCard.tsx
export default function BukuCard() {
    return (
        <div className="bg-white rounded-lg shadow-md p-4 hover:shadow-lg transition">
            <h3 className="text-lg font-bold text-gray-900">Clean Code</h3>
            <p className="text-sm text-gray-600">Robert Martin</p>
            <p className="mt-2 text-green-600 font-semibold">Rp 150.000</p>
            <button className="mt-4 w-full bg-blue-500 hover:bg-blue-600 text-white py-2 rounded">
                Pinjam
            </button>
        </div>
    );
}

// 2. CSS Modules (untuk scoped styles)
// app/components/BukuCard.module.css
.card {
    background: white;
    border-radius: 8px;
    padding: 16px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.card:hover {
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.title {
    font-size: 18px;
    font-weight: bold;
    margin-bottom: 8px;
}

// Usage:
// import styles from './BukuCard.module.css';
// <div className={styles.card}>

// 3. Inline styles (React style objects)
const cardStyle: React.CSSProperties = {
    backgroundColor: 'white',
    borderRadius: '8px',
    padding: '16px',
    boxShadow: '0 2px 4px rgba(0, 0, 0, 0.1)',
};
```

**Langkah konkret**: Style all pages dengan Tailwind CSS.

### 7. [[7. Components dan Component Organization]]

React

```
// components/Header.tsx
import Link from 'next/link';

export default function Header() {
    return (
        <header className="bg-white shadow">
            <nav className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 flex justify-between items-center">
                <Link href="/" className="text-2xl font-bold">
                    📚 Perpustakaan
                </Link>
                <ul className="flex gap-6">
                    <li><Link href="/catalog">Katalog</Link></li>
                    <li><Link href="/about">Tentang</Link></li>
                    <li><Link href="/contact">Kontak</Link></li>
                </ul>
            </nav>
        </header>
    );
}

// components/BukuCard.tsx
interface BukuCardProps {
    id: number;
    judul: string;
    pengarang: string;
    harga: number;
}

export default function BukuCard({ id, judul, pengarang, harga }: BukuCardProps) {
    return (
        <div className="bg-white rounded-lg shadow-md p-4">
            <h3 className="text-lg font-bold">{judul}</h3>
            <p className="text-gray-600">{pengarang}</p>
            <p className="text-green-600 font-semibold">Rp {harga.toLocaleString('id-ID')}</p>
            <button className="mt-4 w-full bg-blue-500 text-white py-2 rounded hover:bg-blue-600">
                Detail
            </button>
        </div>
    );
}

// components/SearchBar.tsx
'use client';  // Client component (interaktif)

import { useState } from 'react';

export default function SearchBar() {
    const [query, setQuery] = useState('');

    const handleSearch = (e: React.FormEvent) => {
        e.preventDefault();
        // Handle search
    };

    return (
        <form onSubmit={handleSearch} className="flex gap-2">
            <input
                type="text"
                value={query}
                onChange={(e) => setQuery(e.target.value)}
                placeholder="Cari buku..."
                className="flex-1 px-4 py-2 border rounded"
            />
            <button type="submit" className="px-6 py-2 bg-blue-500 text-white rounded">
                Search
            </button>
        </form>
    );
}
```

**Component Organization Best Practices:**

text

```
components/
├── Header.tsx
├── Footer.tsx
├── Navigation.tsx
├── cards/
│   ├── BukuCard.tsx
│   └── AuthorCard.tsx
├── forms/
│   ├── SearchBar.tsx
│   └── AddBukuForm.tsx
├── layout/
│   └── Container.tsx
└── common/
    ├── Button.tsx
    ├── Input.tsx
    └── Modal.tsx
```

**Langkah konkret**: Create reusable components untuk catalog.

---

## 🏗 Checkpoint Level 1

text

```
PEMAHAMAN:
├── Next.js vs React SPA
├── File-based routing
├── Pages dan layouts
├── Static generation (SSG)
├── Incremental Static Regeneration (ISR)
├── Route groups
├── Server vs Client components
└── Basic styling dengan Tailwind

PROYEK: Static Perpustakaan Website
├── app/
│   ├── layout.tsx — Root layout dengan Header/Footer
│   ├── page.tsx — Home page
│   ├── about/page.tsx
│   ├── catalog/
│   │   ├── layout.tsx
│   │   ├── page.tsx — Catalog dengan SSG
│   │   └── [id]/page.tsx — Book detail
│   └── contact/page.tsx
├── components/
│   ├── Header.tsx
│   ├── Footer.tsx
│   ├── BukuCard.tsx
│   ├── SearchBar.tsx ('use client')
│   └── Container.tsx
├── Tailwind CSS styling
└── Responsive design

FITUR:
├── Home page dengan intro
├── Catalog page dengan list books (SSG)
├── Book detail page dengan dynamic routes
├── About dan Contact pages
├── Header navigation
├── Footer
├── Responsive layout
└── No interactivity yet (static pages)

STRUKTUR:
perpustakaan-nextjs/
├── app/
├── components/
├── lib/
├── public/
├── styles/
├── package.json
├── next.config.js
└── tsconfig.json

HABIT:
├── Use 'use client' di components yang interaktif
├── Use 'use server' untuk server actions
├── Keep components small dan focused
├── Use TypeScript untuk prop types
├── Folder organization sesuai fitur
└── Tailwind CSS over custom CSS
```

**Commit**: `feat: setup Next.js with pages, layouts, and static generation`

---

# 🔵 LEVEL 2: INTERACTIVITY & DATA (Minggu 3-6)

**Tema**: "Client Components, Interactive Features, Fetching Data"

**Benang Merah**: Static pages (Level 1) → Client components untuk interactivity → Data fetching dari external API

**Output**: Perpustakaan website dengan search, filter, dan interaktif

---

## C. Client Components dan Interactivity

### 8. [[8. 'use client' Directive dan Client Components]]

Server Components adalah default di Next.js 13+. Untuk interactivity, gunakan Client Components.

React

```
// ✅ Server Component (default)
// Berjalan hanya di server, tidak dikirim ke browser
// Bisa akses database, API secrets, file system
export default async function BookList() {
    const books = await fetch('https://api.example.com/books').then(r => r.json());
    
    return (
        <div>
            {books.map(book => (
                <div key={book.id}>{book.judul}</div>
            ))}
        </div>
    );
}

// ❌ Client Component (eksplisit)
// Butuh 'use client' directive
// Bisa akses hooks (useState, useEffect, etc)
// Dikirim ke browser sebagai JavaScript
'use client';

import { useState } from 'react';

export default function SearchBooks() {
    const [query, setQuery] = useState('');
    const [results, setResults] = useState([]);

    const handleSearch = async () => {
        const res = await fetch(`/api/books/search?q=${query}`);
        const data = await res.json();
        setResults(data);
    };

    return (
        <div>
            <input
                value={query}
                onChange={(e) => setQuery(e.target.value)}
                placeholder="Cari buku..."
            />
            <button onClick={handleSearch}>Search</button>
            
            {results.map(book => (
                <div key={book.id}>{book.judul}</div>
            ))}
        </div>
    );
}

// 🎯 Best Practice: Hybrid Approach
// Server component untuk data fetching
// Client component hanya untuk interactivity

// app/catalog/page.tsx (Server Component)
import SearchBooks from '@/components/SearchBooks';

async function getBooks() {
    const res = await fetch('https://api.example.com/books');
    return res.json();
}

export default async function CatalogPage() {
    const books = await getBooks();

    return (
        <div>
            <SearchBooks />  {/* Client component untuk search */}
            
            {books.map(book => (
                <BukuCard key={book.id} {...book} />
            ))}
        </div>
    );
}
```

**Kapan gunakan Server vs Client Components:**

|Kebutuhan|Server Component|Client Component|
|---|---|---|
|Fetch data dari API|✅|❌ (gunakan useEffect)|
|Akses API secrets|✅|❌|
|Simpan data sensitif|✅|❌|
|Gunakan hooks|❌|✅|
|Interactivity (onClick, etc)|❌|✅|
|SEO optimization|✅|❌|

**Langkah konkret**: Create search component sebagai client component.

### 9. [[9. Data Fetching di Server Components]]

React

```
// app/catalog/page.tsx
import { Suspense } from 'react';
import BukuCard from '@/components/BukuCard';
import LoadingSpinner from '@/components/LoadingSpinner';

// Fetch data di server component
async function BooksGrid() {
    // Ini berjalan di server, sebelum dikirim ke browser
    const res = await fetch('https://api.example.com/books', {
        next: { revalidate: 60 } // ISR: revalidate setiap 60 detik
    });

    if (!res.ok) {
        throw new Error('Failed to fetch books');
    }

    const books = await res.json();

    return (
        <div className="grid grid-cols-3 gap-4">
            {books.map((book: any) => (
                <BukuCard key={book.id} buku={book} />
            ))}
        </div>
    );
}

export default function CatalogPage() {
    return (
        <div>
            <h1>Katalog Buku</h1>
            
            {/* Suspense: show loading saat data belum ready */}
            <Suspense fallback={<LoadingSpinner />}>
                <BooksGrid />
            </Suspense>
        </div>
    );
}

// app/components/LoadingSpinner.tsx
export default function LoadingSpinner() {
    return (
        <div className="flex justify-center items-center min-h-96">
            <div className="animate-spin rounded-full h-12 w-12 border-b-2 border-blue-500" />
        </div>
    );
}
```

**Error Handling:**

React

```
// app/catalog/page.tsx
async function BooksGrid() {
    try {
        const res = await fetch('https://api.example.com/books', {
            next: { revalidate: 60 }
        });

        if (!res.ok) {
            throw new Error(`API error: ${res.status}`);
        }

        return <div>{/* content */}</div>;
    } catch (error) {
        return (
            <div className="text-red-500">
                <p>Gagal memuat buku</p>
                <p className="text-sm">{error instanceof Error ? error.message : 'Unknown error'}</p>
            </div>
        );
    }
}

// Better: Error boundary
// app/catalog/error.tsx
'use client';

export default function Error({
    error,
    reset,
}: {
    error: Error & { digest?: string };
    reset: () => void;
}) {
    return (
        <div>
            <h2>Terjadi kesalahan!</h2>
            <p>{error.message}</p>
            <button onClick={() => reset()}>Coba lagi</button>
        </div>
    );
}
```

**Langkah konkret**: Implement data fetching dengan error handling dan Suspense.

### 10. [[10. Client-Side Data Fetching dengan useEffect]]

Untuk data yang real-time atau sering berubah, fetch di client dengan useEffect.

React

```
// components/SearchBooks.tsx
'use client';

import { useState, useEffect } from 'react';
import BukuCard from '@/components/BukuCard';

interface Book {
    id: number;
    judul: string;
    pengarang: string;
    harga: number;
}

export default function SearchBooks() {
    const [query, setQuery] = useState('');
    const [results, setResults] = useState<Book[]>([]);
    const [loading, setLoading] = useState(false);
    const [error, setError] = useState<string | null>(null);

    useEffect(() => {
        if (!query.trim()) {
            setResults([]);
            return;
        }

        const fetchResults = async () => {
            setLoading(true);
            setError(null);

            try {
                const res = await fetch(`/api/books/search?q=${encodeURIComponent(query)}`);
                if (!res.ok) throw new Error('Search failed');
                
                const data = await res.json();
                setResults(data.results);
            } catch (err) {
                setError(err instanceof Error ? err.message : 'Unknown error');
            } finally {
                setLoading(false);
            }
        };

        // Debounce: tunggu 300ms sebelum search
        const timer = setTimeout(fetchResults, 300);
        return () => clearTimeout(timer);
    }, [query]);

    return (
        <div>
            <input
                type="text"
                value={query}
                onChange={(e) => setQuery(e.target.value)}
                placeholder="Cari buku..."
                className="w-full px-4 py-2 border rounded"
            />

            {loading && <p>Mencari...</p>}
            {error && <p className="text-red-500">{error}</p>}

            <div className="grid grid-cols-3 gap-4 mt-4">
                {results.map(book => (
                    <BukuCard key={book.id} buku={book} />
                ))}
            </div>
        </div>
    );
}
```

**Langkah konkret**: Implement search dengan client-side fetching dan debounce.

---

## D. Link dan Navigation

### 11. [[11. Next.js Link Component dan Prefetching]]

`<Link>` adalah Next.js replacement untuk `<a>` — dengan automatic prefetching.

React

```
// ❌ JANGAN gunakan <a> untuk internal navigation
<a href="/catalog/123">Buku Detail</a>

// ✅ GUNAKAN <Link>
import Link from 'next/link';

export default function BukuCard({ id, judul }: any) {
    return (
        <Link href={`/catalog/${id}`}>
            <div className="cursor-pointer">
                <h3>{judul}</h3>
            </div>
        </Link>
    );
}

// Link benefits:
// 1. Automatic code splitting
// 2. Prefetching ketika link terlihat (faster navigation)
// 3. Client-side navigation (tidak reload page)
// 4. Scroll restoration
```

**Dynamic Routes dengan Link:**

React

```
// components/BukuCard.tsx
import Link from 'next/link';

interface Book {
    id: number;
    judul: string;
    slug: string;
}

export default function BukuCard({ id, judul, slug }: Book) {
    return (
        <Link href={`/catalog/${slug}`} className="block p-4 hover:shadow-lg">
            <h3>{judul}</h3>
        </Link>
    );
}

// app/catalog/[slug]/page.tsx
interface BookDetailProps {
    params: {
        slug: string;
    };
}

export default async function BookDetailPage({ params }: BookDetailProps) {
    const res = await fetch(`/api/books/slug/${params.slug}`);
    const book = await res.json();

    return (
        <div>
            <h1>{book.judul}</h1>
            <p>{book.pengarang}</p>
            <Link href="/catalog">← Kembali ke Katalog</Link>
        </div>
    );
}
```

**Langkah konkret**: Replace semua navigasi dengan `<Link>` component.

---

## 🏗 Checkpoint Level 2

text

```
PEMAHAMAN:
├── Server Components vs Client Components
├── 'use client' directive
├── Server-side data fetching
├── Client-side data fetching dengan useEffect
├── Error handling dan error boundaries
├── Suspense untuk loading states
├── Next.js Link component
└── Prefetching behavior

PROYEK: Interactive Perpustakaan
├── SearchBooks component dengan client-side search
├── BukuCard dengan Link navigation
├── Book detail page dengan dynamic routes
├── Loading states dengan Suspense
├── Error boundaries
├── Responsive grid layout
└── Real-time search dengan debounce

FITUR:
├── Search buku (client-side)
├── Filter by kategori
├── Sort options
├── Book detail page
├── Back navigation
├── Loading indicators
├── Error handling
└── Smooth client-side navigation

STRUKTUR:
app/
├── layout.tsx
├── page.tsx
├── catalog/
│   ├── page.tsx (SSG dengan Suspense)
│   └── [slug]/page.tsx
├── api/
│   └── books/
│       └── route.ts (API route)
└── error.tsx

components/
├── SearchBooks.tsx ('use client')
├── BukuCard.tsx
├── BukuGrid.tsx
└── LoadingSpinner.tsx

HABIT:
├── Server components by default
├── 'use client' only saat perlu interactivity
├── Error boundaries untuk error handling
├── Suspense untuk async operations
├── Link untuk internal navigation
└── Proper TypeScript types
```

**Commit**: `feat: add interactive search and client-side navigation`

---

# 🟡 LEVEL 3: API ROUTES & DATABASE (Minggu 6-9)

**Tema**: "Build Backend di Next.js — API Routes, Database, dan Full-Stack"

**Benang Merah**: Client app (Level 2) → API routes di Next.js → Database integration → CRUD operations

**Output**: Full-stack perpustakaan app dengan database backend

---

## E. API Routes

### 12. [[12. API Routes Basics]]

Next.js bisa handle backend requests langsung — no separate backend server needed.

React

```
// app/api/books/route.ts (GET /api/books)
import { NextRequest, NextResponse } from 'next/server';

export async function GET(request: NextRequest) {
    try {
        const books = [
            { id: 1, judul: 'Clean Code', pengarang: 'Robert Martin', harga: 150000 },
            { id: 2, judul: 'Design Patterns', pengarang: 'Gang of Four', harga: 200000 },
        ];

        return NextResponse.json({
            success: true,
            data: books,
        });
    } catch (error) {
        return NextResponse.json(
            { success: false, error: 'Failed to fetch books' },
            { status: 500 }
        );
    }
}

export async function POST(request: NextRequest) {
    try {
        const body = await request.json();

        // Validasi
        if (!body.judul || !body.pengarang) {
            return NextResponse.json(
                { success: false, error: 'Judul dan pengarang wajib' },
                { status: 400 }
            );
        }

        // Simpan ke database (nanti)
        const newBook = {
            id: Math.random(),
            ...body,
        };

        return NextResponse.json(
            { success: true, data: newBook },
            { status: 201 }
        );
    } catch (error) {
        return NextResponse.json(
            { success: false, error: 'Failed to create book' },
            { status: 500 }
        );
    }
}
```

**Dynamic API Routes:**

React

```
// app/api/books/[id]/route.ts (GET /api/books/123)
import { NextRequest, NextResponse } from 'next/server';

interface RouteParams {
    params: {
        id: string;
    };
}

export async function GET(
    request: NextRequest,
    { params }: RouteParams
) {
    const { id } = params;

    // Fetch dari database
    const book = {
        id: parseInt(id),
        judul: 'Clean Code',
        pengarang: 'Robert Martin',
        harga: 150000,
    };

    if (!book) {
        return NextResponse.json(
            { success: false, error: 'Book not found' },
            { status: 404 }
        );
    }

    return NextResponse.json({ success: true, data: book });
}

export async function PUT(
    request: NextRequest,
    { params }: RouteParams
) {
    const { id } = params;
    const body = await request.json();

    // Update di database
    return NextResponse.json({
        success: true,
        data: { id, ...body },
    });
}

export async function DELETE(
    request: NextRequest,
    { params }: RouteParams
) {
    const { id } = params;

    // Delete dari database
    return NextResponse.json({ success: true });
}
```

**Query Parameters:**

React

```
// app/api/books/search/route.ts (GET /api/books/search?q=clean)
export async function GET(request: NextRequest) {
    const searchParams = request.nextUrl.searchParams;
    const query = searchParams.get('q');
    const category = searchParams.get('category');

    if (!query) {
        return NextResponse.json(
            { success: false, error: 'Query parameter required' },
            { status: 400 }
        );
    }

    // Search di database
    const results = [
        { id: 1, judul: 'Clean Code', category: 'teknologi' },
        { id: 2, judul: 'Clean Architecture', category: 'teknologi' },
    ].filter(book =>
        book.judul.toLowerCase().includes(query.toLowerCase()) &&
        (!category || book.category === category)
    );

    return NextResponse.json({ success: true, results });
}
```

**Langkah konkret**: Create API routes untuk CRUD operations.

### 13. [[13. Database Integration — Prisma ORM]]

Prisma adalah type-safe ORM yang bekerja great dengan Next.js.

Bash

```
npm install @prisma/client
npm install -D prisma

npx prisma init
```

**Setup Database (PostgreSQL recommended):**

text

```
# .env.local
DATABASE_URL="postgresql://user:password@localhost:5432/perpustakaan"
```

**Define Models:**

prisma

```
// prisma/schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model Buku {
  id        Int     @id @default(autoincrement())
  judul     String  @db.VarChar(200)
  pengarang String  @db.VarChar(100)
  isbn      String? @unique
  tahun     Int?
  harga     Int
  stok      Int     @default(0)
  kategori  String  @default("Umum")

  // Relations
  peminjaman Peminjaman[]

  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@fulltext([judul, pengarang])
}

model User {
  id      Int     @id @default(autoincrement())
  email   String  @unique
  nama    String
  password String

  peminjaman Peminjaman[]

  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Peminjaman {
  id            Int     @id @default(autoincrement())
  bukuId        Int
  userId        Int
  tanggalPinjam DateTime @default(now())
  batasPinjam   DateTime
  tanggalKembali DateTime?

  buku Buku @relation(fields: [bukuId], references: [id], onDelete: Cascade)
  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@unique([bukuId, userId, tanggalKembali])
}
```

**Migration:**

Bash

```
npx prisma migrate dev --name init
npx prisma studio  # GUI untuk browse data
```

**Use Prisma di API Routes:**

React

```
// app/api/books/route.ts
import { prisma } from '@/lib/prisma';
import { NextResponse } from 'next/server';

export async function GET() {
    try {
        const books = await prisma.buku.findMany({
            orderBy: { createdAt: 'desc' },
        });

        return NextResponse.json({ success: true, data: books });
    } catch (error) {
        return NextResponse.json(
            { success: false, error: 'Failed to fetch books' },
            { status: 500 }
        );
    }
}

export async function POST(request: Request) {
    try {
        const body = await request.json();

        const book = await prisma.buku.create({
            data: {
                judul: body.judul,
                pengarang: body.pengarang,
                isbn: body.isbn,
                tahun: body.tahun,
                harga: body.harga,
                stok: body.stok,
                kategori: body.kategori,
            },
        });

        return NextResponse.json({ success: true, data: book }, { status: 201 });
    } catch (error) {
        return NextResponse.json(
            { success: false, error: 'Failed to create book' },
            { status: 400 }
        );
    }
}

// app/api/books/[id]/route.ts
export async function GET(
    request: Request,
    { params }: { params: { id: string } }
) {
    try {
        const book = await prisma.buku.findUnique({
            where: { id: parseInt(params.id) },
            include: { peminjaman: true },
        });

        if (!book) {
            return NextResponse.json(
                { success: false, error: 'Book not found' },
                { status: 404 }
            );
        }

        return NextResponse.json({ success: true, data: book });
    } catch (error) {
        return NextResponse.json(
            { success: false, error: 'Failed to fetch book' },
            { status: 500 }
        );
    }
}

export async function PUT(
    request: Request,
    { params }: { params: { id: string } }
) {
    try {
        const body = await request.json();

        const book = await prisma.buku.update({
            where: { id: parseInt(params.id) },
            data: body,
        });

        return NextResponse.json({ success: true, data: book });
    } catch (error) {
        return NextResponse.json(
            { success: false, error: 'Failed to update book' },
            { status: 400 }
        );
    }
}

export async function DELETE(
    request: Request,
    { params }: { params: { id: string } }
) {
    try {
        await prisma.buku.delete({
            where: { id: parseInt(params.id) },
        });

        return NextResponse.json({ success: true });
    } catch (error) {
        return NextResponse.json(
            { success: false, error: 'Failed to delete book' },
            { status: 400 }
        );
    }
}
```

**Langkah konkret**: Setup database, create models, implement CRUD API routes.

### 14. [[14. Server Actions — Modern Alternative to API Routes]]

Server Actions adalah cara lebih modern untuk backend logic di Next.js 13+.

React

```
// app/actions/books.ts
'use server';

import { prisma } from '@/lib/prisma';
import { revalidatePath } from 'next/cache';

export async function createBook(data: {
    judul: string;
    pengarang: string;
    harga: number;
    stok: number;
}) {
    try {
        const book = await prisma.buku.create({
            data,
        });

        revalidatePath('/catalog');
        return { success: true, data: book };
    } catch (error) {
        return { success: false, error: 'Failed to create book' };
    }
}

export async function deleteBook(id: number) {
    try {
        await prisma.buku.delete({ where: { id } });
        revalidatePath('/catalog');
        return { success: true };
    } catch (error) {
        return { success: false, error: 'Failed to delete book' };
    }
}

export async function updateBook(id: number, data: any) {
    try {
        const book = await prisma.buku.update({
            where: { id },
            data,
        });

        revalidatePath(`/catalog/${id}`);
        return { success: true, data: book };
    } catch (error) {
        return { success: false, error: 'Failed to update book' };
    }
}

// Gunakan di client component:
// components/AddBukuForm.tsx
'use client';

import { createBook } from '@/app/actions/books';
import { useState } from 'react';

export default function AddBukuForm() {
    const [loading, setLoading] = useState(false);

    const handleSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
        e.preventDefault();
        setLoading(true);

        const formData = new FormData(e.currentTarget);
        const result = await createBook({
            judul: formData.get('judul') as string,
            pengarang: formData.get('pengarang') as string,
            harga: parseInt(formData.get('harga') as string),
            stok: parseInt(formData.get('stok') as string),
        });

        setLoading(false);

        if (result.success) {
            alert('Buku berhasil ditambahkan');
            e.currentTarget.reset();
        } else {
            alert(result.error);
        }
    };

    return (
        <form onSubmit={handleSubmit}>
            <input name="judul" required placeholder="Judul" />
            <input name="pengarang" required placeholder="Pengarang" />
            <input type="number" name="harga" required placeholder="Harga" />
            <input type="number" name="stok" required placeholder="Stok" />
            <button type="submit" disabled={loading}>
                {loading ? 'Menyimpan...' : 'Tambah Buku'}
            </button>
        </form>
    );
}
```

**Langkah konkret**: Implement Server Actions untuk CRUD operations.

---

## 🏗 Checkpoint Level 3

text

```
PEMAHAMAN:
├── API Routes (GET, POST, PUT, DELETE)
├── Dynamic API routes dengan [id]
├── HTTP methods dan status codes
├── Request/response handling
├── Prisma ORM basics
├── Database schema design
├── Server Actions
└── Revalidation strategies

PROYEK: Full-Stack Perpustakaan dengan Database
├── app/api/books/route.ts
├── app/api/books/[id]/route.ts
├── app/api/books/search/route.ts
├── app/actions/books.ts (Server Actions)
├── Prisma setup dengan PostgreSQL
├── Database migrations
├── CRUD operations
├── Search and filter
└── Full frontend + backend integration

FITUR:
├── List buku dari database
├── Create buku (form + API/Server Action)
├── Read buku (detail page)
├── Update buku
├── Delete buku
├── Search dengan database query
├── Filter dan sorting
└── Proper error handling

DATABASE:
├── Buku model
├── User model
├── Peminjaman model
├── Relationships (one-to-many)
└── Indexes dan optimasi

STRUKTUR:
app/
├── api/
│   └── books/
│       ├── route.ts
│       ├── search/
│       │   └── route.ts
│       └── [id]/
│           └── route.ts
├── actions/
│   └── books.ts
├── catalog/
│   ├── page.tsx
│   └── [slug]/
│       └── page.tsx
└── components/
    ├── AddBukuForm.tsx
    ├── BukuCard.tsx
    └── SearchBooks.tsx

prisma/
├── schema.prisma
└── migrations/

lib/
└── prisma.ts

HABIT:
├── Prisma client singleton
├── Type-safe database queries
├── Proper error handling di API
├── Input validation
├── SQL injection prevention (Prisma handles this)
└── Revalidate cache saat data berubah
```

**Commit**: `feat: add API routes, Prisma, and database integration`

---

# 🟠 LEVEL 4: SERVER COMPONENTS & ADVANCED (Minggu 9-12)

**Tema**: "Modern Next.js Patterns — Server Components, Streaming, Advanced Data Fetching"

---

## F. Advanced Next.js Patterns

### 15. [[15. Server Components Deep Dive]]

React

```
// app/catalog/page.tsx (Server Component)
import { Suspense } from 'react';
import { prisma } from '@/lib/prisma';
import BukuCard from '@/components/BukuCard';
import LoadingSkeletons from '@/components/LoadingSkeletons';

// ✅ Server Component: fetch data
async function BooksGrid({ category }: { category?: string }) {
    const books = await prisma.buku.findMany({
        where: category ? { kategori: category } : {},
        orderBy: { createdAt: 'desc' },
    });

    return (
        <div className="grid grid-cols-3 gap-4">
            {books.map(book => (
                <BukuCard key={book.id} buku={book} />
            ))}
        </div>
    );
}

// Server Component dengan Suspense Boundary
export default function CatalogPage({
    searchParams,
}: {
    searchParams: { category?: string };
}) {
    return (
        <div>
            <h1>Katalog Buku</h1>
            
            <Suspense fallback={<LoadingSkeletons />}>
                <BooksGrid category={searchParams.category} />
            </Suspense>
        </div>
    );
}
```

### 16. [[16. Streaming dan Progressive Enhancement]]

Streaming membuat halaman muncul lebih cepat — sebagian konten bisa ditampilkan sementara bagian lain masih loading.

React

```
// app/catalog/page.tsx
import { Suspense } from 'react';

function FeaturedBooks() {
    // Loading cepat (cached)
    const featured = [/* ... */];
    return <div>{/* render featured */}</div>;
}

function AllBooks() {
    // Loading lambat (fetch dari database)
    return <BooksGrid />;
}

export default function CatalogPage() {
    return (
        <div>
            <h1>Katalog</h1>
            
            {/* Ini tampil langsung */}
            <FeaturedBooks />
            
            {/* Ini tampil sambil loading */}
            <Suspense fallback={<div>Memuat semua buku...</div>}>
                <AllBooks />
            </Suspense>
        </div>
    );
}
```

### 17. [[17. Middleware dan Authentication]]

Middleware berjalan di Edge — sebelum request masuk ke route handler.

React

```
// middleware.ts (di root project)
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function middleware(request: NextRequest) {
    // Check auth token
    const token = request.cookies.get('token')?.value;

    // Redirect ke login jika tidak ada token
    if (!token && request.nextUrl.pathname.startsWith('/dashboard')) {
        return NextResponse.redirect(new URL('/login', request.url));
    }

    return NextResponse.next();
}

export const config = {
    matcher: ['/dashboard/:path*', '/admin/:path*'],
};

// app/api/auth/login/route.ts
export async function POST(request: Request) {
    const { email, password } = await request.json();

    // Verify credentials
    const user = await prisma.user.findUnique({ where: { email } });

    if (!user || !(await verifyPassword(password, user.password))) {
        return NextResponse.json({ error: 'Invalid credentials' }, { status: 401 });
    }

    // Create session/JWT
    const response = NextResponse.json({
        success: true,
        user,
    });

    response.cookies.set('token', 'jwt-token-here', {
        httpOnly: true,
        secure: true,
        sameSite: 'lax',
    });

    return response;
}
```

**Langkah konkret**: Add authentication dengan login/logout functionality.

---

# 🔴 LEVEL 5: PERFORMANCE & DEPLOYMENT (Minggu 12-15)

**Tema**: "Optimization, Caching, dan Deploy ke Production"

---

## G. Performance dan Deployment

### 18. [[18. Image Optimization]]

React

```
import Image from 'next/image';

// Automatic optimization
export default function BukuCard({ buku }: any) {
    return (
        <div>
            <Image
                src={buku.coverImage}
                alt={buku.judul}
                width={300}
                height={400}
                priority={false}
                placeholder="blur"
                blurDataURL="data:image/jpeg;base64,..."
            />
        </div>
    );
}
```

### 19. [[19. Deployment ke Vercel]]

Bash

```
# Push ke GitHub
git push origin main

# Deploy di Vercel
# Option 1: Vercel Dashboard → Import repository
# Option 2: CLI
npm install -g vercel
vercel

# Setup environment variables
vercel env add DATABASE_URL
vercel env add JWT_SECRET
```

---

# 🟡 LEVEL 6: ADVANCED FEATURES (Minggu 15-18)

**Tema**: "TypeScript strict mode, testing, advanced patterns"

---

## H. Testing dan Quality

### 20. [[20. Testing dengan Jest]]

Bash

```
npm install -D jest @testing-library/react
```

React

```
// app/actions/books.test.ts
import { createBook } from '@/app/actions/books';

describe('createBook', () => {
    it('should create a book with valid data', async () => {
        const result = await createBook({
            judul: 'Test Book',
            pengarang: 'Test Author',
            harga: 100000,
            stok: 5,
        });

        expect(result.success).toBe(true);
        expect(result.data.judul).toBe('Test Book');
    });
});
```

---

# 🟣 LEVEL 7-8: SPECIALIZATION

Choose your path:

- **E-commerce**: Shopping cart, payments (Stripe), inventory
- **SaaS**: Multi-tenancy, subscription, billing
- **CMS**: Content management, rich editor, publishing workflow

---

## 🏗 Complete Checkpoint

text

```
FINAL CHECKLIST:
✅ Full-stack app (frontend + backend)
✅ Database with Prisma ORM
✅ Authentication & authorization
✅ API routes atau Server Actions
✅ Performance optimized (images, caching)
✅ Error handling & validation
✅ Tests (unit & integration)
✅ Deployed ke production
✅ Monitoring & logging
✅ Type-safe dengan TypeScript
```

## 📊 Timeline

|Level|Focus|Durasi|Project Milestone|
|---|---|---|---|
|1|Fundamentals & Routing|3 minggu|Static website|
|2|Interactivity|3 minggu|Interactive pages|
|3|Backend & Database|3 minggu|Full-stack app|
|4|Advanced patterns|3 minggu|Production-ready|
|5|Performance|3 minggu|Optimized|
|6|Quality|3 minggu|Tested & monitored|
|7-8|Specialization|4-8 minggu|Domain expert|

---

**Roadmap Next.js v1.0 — Dari Learning hingga Production**

Next.js bukan sekadar framework — ini adalah ecosystem yang menghilangkan repetisi dan memberikan best practices by default. Setiap level dirancang untuk membangun confidence dan production-ready skills.