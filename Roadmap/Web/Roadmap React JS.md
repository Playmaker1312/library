# 🗺 Roadmap React: Step-by-Step Membangun Aplikasi Web Modern

## Filosofi Roadmap Ini

"React adalah library untuk membangun user interfaces dengan components — setiap konsep dipelajari dengan fokus pada composition, state management, dan reactive programming mindset"

---

## Prinsip Desain

- **Satu Project, Tumbuh Bersama**: Perpustakaan dari static HTML → Interactive components → Full app dengan routing → Advanced patterns
- **Component-First Thinking**: Berpikir dalam components, bukan pages
- **Composition Over Configuration**: Prefer props dan composition daripada inheritance
- **Modern React**: Fokus pada Hooks (bukan Class components), Functional components, React 18+
- **Developer Experience First**: Tooling, debugging, performance dari awal

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: "Hello React" — JSX, Components, Props
 ↓ (enhance)
Level 2: + State (useState), Forms → Interactive components
 ↓ (enhance)
Level 3: + Effects (useEffect), Lifecycle → Data fetching dari API
 ↓ (enhance)
Level 4: + Routing (React Router) → Multi-page app
 ↓ (enhance)
Level 5: + State Management (Context/Redux) → Scalable state
 ↓ (enhance)
Level 6: + Advanced Hooks, Performance → Production-ready
 ↓ (enhance)
Level 7: + Testing (Jest, RTL), TypeScript → Type-safe, tested code
 ↓ (choose path)
Level 8: Next.js / Tanstack Query / Advanced patterns
```

---

# 🟢 LEVEL 1: REACT BASICS (Minggu 1-3)

**Tema**: "JSX, Components, Props — Fundamental React concepts"

**Benang Merah**: JavaScript basics → JSX syntax → Functional components → Props flow

**Output**: Interactive UI dengan reusable components

---

## A. Setup React Environment

### 1. [[1. React Philosophy dan Setup]]

React adalah **JavaScript library** untuk building UIs dengan:

- ✅ Component-based architecture
- ✅ Declarative (describe UI, React handles updates)
- ✅ Unidirectional data flow (props down, events up)
- ✅ Virtual DOM untuk efficient rendering

**Setup dengan Create React App (easiest to start):**

Bash

```
# Create new React app
npx create-react-app perpustakaan-react
cd perpustakaan-react
npm start  # Start development server

# Structure
perpustakaan-react/
├── src/
│   ├── App.js
│   ├── App.css
│   ├── index.js
│   └── components/
├── public/
│   └── index.html
├── package.json
└── .gitignore
```

**Alternative: Vite (faster):**

Bash

```
npm create vite@latest perpustakaan-react -- --template react
cd perpustakaan-react
npm install
npm run dev
```

**Langkah konkret**: Setup project dengan Create React App atau Vite.

### 2. [[2. JSX — JavaScript XML]]

JSX adalah syntax extension yang looks like HTML tapi actually JavaScript.

React

```
// JSX adalah syntactic sugar untuk React.createElement()
// Keduanya equivalent:

// JSX
const element = <h1>Hello, Perpustakaan!</h1>;

// JavaScript
const element = React.createElement('h1', null, 'Hello, Perpustakaan!');

// JSX dengan expressions
const nama = "Perpustakaan Digital";
const element2 = <h1>Selamat datang di {nama}</h1>;

// JSX dengan multiple children
const profile = (
    <div>
        <h1>{nama}</h1>
        <p>Versi: 1.0.0</p>
    </div>
);

// JSX rules:
// 1. Single root element
// ✅ <div><h1>Title</h1><p>Text</p></div>
// ❌ <h1>Title</h1><p>Text</p>

// 2. className (bukan class)
<div className="container">

// 3. camelCase for attributes
<button onClick={handleClick}>
<input onChange={handleChange} />
<div style={{ color: 'red' }}>

// 4. Self-closing tags
<img src="photo.jpg" alt="Photo" />
<input type="text" />

// 5. Comments
{/* This is a comment */}
// This is NOT a comment (will be rendered)
```

**Langkah konkret**: Write JSX untuk perpustakaan profile page.

---

## B. Functional Components dan Props

### 3. [[3. Functional Components (Modern React)]]

React components adalah **functions that return JSX**.

React

```
// Simple component
function Welcome() {
    return <h1>Selamat datang di Perpustakaan</h1>;
}

// Component with JSX returned
function BukuCard() {
    return (
        <div className="card">
            <h2>Clean Code</h2>
            <p>Robert Martin</p>
            <p>Rp 150.000</p>
        </div>
    );
}

// Arrow function (modern style)
const Header = () => {
    return <header>Perpustakaan Digital</header>;
};

// Implicit return (if only JSX)
const Footer = () => <footer>© 2024 Perpustakaan</footer>;

// Export component
export default Welcome;
export { BukuCard, Header, Footer };
```

**File: src/App.js**

React

```
import React from 'react';
import './App.css';
import Welcome from './Welcome';
import { BukuCard, Header, Footer } from './components';

function App() {
    return (
        <div className="App">
            <Header />
            <Welcome />
            <BukuCard />
            <Footer />
        </div>
    );
}

export default App;
```

**Langkah konkret**: Create 3-5 components untuk perpustakaan.

### 4. [[4. Props — Passing Data to Components]]

Props adalah cara mengpass data dari parent ke child component.

React

```
// Component menerima props
function BukuCard(props) {
    return (
        <div className="card">
            <h2>{props.judul}</h2>
            <p>{props.pengarang}</p>
            <p>Rp {props.harga.toLocaleString('id-ID')}</p>
            <p className={props.stok > 0 ? 'tersedia' : 'habis'}>
                Stok: {props.stok}
            </p>
        </div>
    );
}

// Destructuring props (cleaner)
function BukuCard({ judul, pengarang, harga, stok }) {
    return (
        <div className="card">
            <h2>{judul}</h2>
            <p>{pengarang}</p>
            <p>Rp {harga.toLocaleString('id-ID')}</p>
            <p className={stok > 0 ? 'tersedia' : 'habis'}>
                Stok: {stok}
            </p>
        </div>
    );
}

// Using component with props
function KatalogBuku() {
    const buku = [
        { id: 1, judul: 'Clean Code', pengarang: 'Robert Martin', harga: 150000, stok: 5 },
        { id: 2, judul: 'Design Patterns', pengarang: 'Gang of Four', harga: 200000, stok: 3 },
        { id: 3, judul: 'Algorithms', pengarang: 'Thomas Cormen', harga: 120000, stok: 0 },
    ];

    return (
        <div className="katalog">
            {buku.map(b => (
                <BukuCard
                    key={b.id}
                    judul={b.judul}
                    pengarang={b.pengarang}
                    harga={b.harga}
                    stok={b.stok}
                />
            ))}
        </div>
    );
}

export { BukuCard, KatalogBuku };
```

**Props best practices:**

React

```
// ✅ Destructure props di parameter
function BukuCard({ judul, pengarang, harga, stok, onPinjam }) {
    return (
        <div>
            <h2>{judul}</h2>
            <button onClick={onPinjam}>Pinjam</button>
        </div>
    );
}

// ✅ Spread operator untuk passing banyak props
const bukuData = { judul: 'Clean Code', pengarang: 'Robert Martin', harga: 150000, stok: 5 };
<BukuCard {...bukuData} onPinjam={handlePinjam} />

// ❌ Hindari prop drilling terlalu dalam
// Lebih baik gunakan Context API untuk deep props (nanti di Level 5)
```

**Langkah konkret**: Buat component dengan props dari parent, render list dinamis.

### 5. [[5. Conditional Rendering]]

React

```
// if-else di dalam render
function BukuDetail({ buku }) {
    if (!buku) {
        return <p>Loading...</p>;
    }

    return (
        <div>
            <h2>{buku.judul}</h2>
            <p>{buku.pengarang}</p>
        </div>
    );
}

// Ternary operator
function StatusBuku({ stok }) {
    return (
        <p>
            {stok > 0 ? '✅ Tersedia' : '❌ Habis'}
        </p>
    );
}

// Logical AND (&&)
function AdminPanel({ isAdmin, data }) {
    return (
        <div>
            {isAdmin && <button>Edit</button>}
        </div>
    );
}

// Switch statement (untuk multiple conditions)
function CategoryBadge({ kategori }) {
    switch (kategori) {
        case 'teknologi':
            return <span className="badge tech">💻 Teknologi</span>;
        case 'fiksi':
            return <span className="badge fiction">📖 Fiksi</span>;
        case 'sains':
            return <span className="badge science">🔬 Sains</span>;
        default:
            return <span className="badge">Lainnya</span>;
    }
}

// Early return (clean pattern)
function BukuCard({ buku, stok }) {
    if (stok === 0) {
        return <div className="card disabled">Habis</div>;
    }

    return (
        <div className="card">
            <h2>{buku.judul}</h2>
            <button>Pinjam</button>
        </div>
    );
}
```

**Langkah konkret**: Add conditional rendering untuk status buku (tersedia/habis).

### 6. [[6. Lists dan Keys]]

React

```
// Render list dengan map()
function BukuList({ buku }) {
    return (
        <ul>
            {buku.map(b => (
                <li key={b.id}>
                    {b.judul} - {b.pengarang}
                </li>
            ))}
        </ul>
    );
}

// ❌ JANGAN gunakan index sebagai key (jika list bisa berubah)
{buku.map((b, index) => (
    <BukuCard key={index} buku={b} />  // BURUK!
))}

// ✅ Gunakan unique identifier sebagai key
{buku.map(b => (
    <BukuCard key={b.id} buku={b} />  // BAIK!
))}

// Keys berguna untuk:
// 1. Membantu React identify mana yang berubah
// 2. Maintain component state saat list berubah
// 3. Performance optimization

// Complex list dengan nested components
function KatalogBukuPerKategori({ kategoriData }) {
    return (
        <div>
            {kategoriData.map(kategori => (
                <div key={kategori.id} className="kategori-section">
                    <h2>{kategori.nama}</h2>
                    <div className="buku-grid">
                        {kategori.buku.map(b => (
                            <BukuCard key={b.id} buku={b} />
                        ))}
                    </div>
                </div>
            ))}
        </div>
    );
}
```

**Langkah konkret**: Render katalog buku dengan array.map() dan proper keys.

---

## C. Styling React Components

### 7. [[7. Styling Approaches in React]]

React

```
// 1. Inline styles (object notation)
function StyledButton() {
    const buttonStyle = {
        backgroundColor: '#007bff',
        color: 'white',
        padding: '10px 20px',
        border: 'none',
        borderRadius: '4px',
        cursor: 'pointer',
        fontSize: '16px',
    };

    return <button style={buttonStyle}>Click me</button>;
}

// 2. CSS Classes (recommended)
// App.css
function App() {
    return (
        <div className="container">
            <header className="header">
                <h1 className="header-title">Perpustakaan</h1>
            </header>
        </div>
    );
}

// 3. CSS Modules (scoped styling)
// BukuCard.module.css
.card {
    border: 1px solid #ddd;
    padding: 16px;
    border-radius: 8px;
}

// BukuCard.jsx
import styles from './BukuCard.module.css';

function BukuCard({ buku }) {
    return (
        <div className={styles.card}>
            <h2>{buku.judul}</h2>
        </div>
    );
}

// 4. Tailwind CSS (utility-first, recommended for modern React)
// Requires: npm install -D tailwindcss
function BukuCard({ buku }) {
    return (
        <div className="border rounded-lg p-4 shadow-md hover:shadow-lg">
            <h2 className="text-xl font-bold mb-2">{buku.judul}</h2>
            <p className="text-gray-600">{buku.pengarang}</p>
            <p className="text-green-600 font-semibold">
                Rp {buku.harga.toLocaleString('id-ID')}
            </p>
        </div>
    );
}

// 5. Styled Components (CSS-in-JS, requires: npm install styled-components)
import styled from 'styled-components';

const CardContainer = styled.div`
    border: 1px solid #ddd;
    padding: 16px;
    border-radius: 8px;
    transition: box-shadow 0.3s;

    &:hover {
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    }
`;

const CardTitle = styled.h2`
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 8px;
    color: ${props => props.color || '#333'};
`;

function BukuCard({ buku }) {
    return (
        <CardContainer>
            <CardTitle>{buku.judul}</CardTitle>
            <p>{buku.pengarang}</p>
        </CardContainer>
    );
}
```

**Langkah konkret**: Style components dengan salah satu approach (Tailwind recommended).

---

## 🏗 Checkpoint Level 1

text

```
PEMAHAMAN:
├── JSX syntax dan rules
├── Functional components
├── Props dan prop passing
├── Destructuring props
├── Conditional rendering
├── Rendering lists dengan map()
├── Keys untuk list rendering
└── Styling approaches

PROYEK: Perpustakaan Component Library
├── App.js — Main component
├── components/Header.jsx
├── components/Footer.jsx
├── components/BukuCard.jsx — Card with props
├── components/KatalogBuku.jsx — List of books
├── components/CategoryBadge.jsx — Conditional badge
├── components/SearchBar.jsx — Input (not interactive yet)
├── Proper styling (Tailwind atau CSS)
├── Static data (hardcoded array)
└── Proper folder structure

STRUKTUR:
src/
├── App.js
├── App.css
├── index.js
├── components/
│   ├── Header.jsx
│   ├── Footer.jsx
│   ├── BukuCard.jsx
│   ├── KatalogBuku.jsx
│   └── CategoryBadge.jsx
└── data/
    └── buku.js (hardcoded data)

DATA:
const buku = [
    {
        id: 1,
        judul: 'Clean Code',
        pengarang: 'Robert Martin',
        harga: 150000,
        stok: 5,
        kategori: 'teknologi'
    },
    // ... more books
];

HABIT:
├── Component names PascalCase (BukuCard, not bukuCard)
├── Destructure props di parameter
├── Use key={id} dalam list rendering
├── One component per file (mostly)
├── CSS classes for styling, bukan inline styles
├── Meaningful prop names
└── Proper folder organization
```

**Commit**: `feat: setup React project with components and props`

---

# 🔵 LEVEL 2: STATE & INTERACTIVITY (Minggu 3-6)

**Tema**: "useState hook — Making components interactive"

**Benang Merah**: Props flow (Level 1) → State management → Event handling → Form inputs

**Output**: Interactive UI dengan state updates dan user input handling

---

## D. State Management with Hooks

### 8. [[8. useState Hook — Intro to State]]

State adalah data yang dapat berubah dan trigger re-render.

React

```
import React, { useState } from 'react';

// Basic useState
function Counter() {
    // Destructure: [currentValue, functionToUpdate]
    const [count, setCount] = useState(0);  // Initial value: 0

    return (
        <div>
            <p>Count: {count}</p>
            <button onClick={() => setCount(count + 1)}>
                Increment
            </button>
            <button onClick={() => setCount(count - 1)}>
                Decrement
            </button>
            <button onClick={() => setCount(0)}>
                Reset
            </button>
        </div>
    );
}

// Multiple states
function BukuForm() {
    const [judul, setJudul] = useState('');
    const [pengarang, setPengarang] = useState('');
    const [harga, setHarga] = useState(0);

    return (
        <form>
            <input
                value={judul}
                onChange={e => setJudul(e.target.value)}
                placeholder="Judul"
            />
            <input
                value={pengarang}
                onChange={e => setPengarang(e.target.value)}
                placeholder="Pengarang"
            />
            <input
                type="number"
                value={harga}
                onChange={e => setHarga(Number(e.target.value))}
                placeholder="Harga"
            />
        </form>
    );
}

// State dengan object (better for related data)
function BukuForm() {
    const [formData, setFormData] = useState({
        judul: '',
        pengarang: '',
        harga: 0,
        stok: 0,
    });

    const handleChange = (e) => {
        const { name, value } = e.target;
        setFormData({
            ...formData,  // Spread operator untuk keep other properties
            [name]: value
        });
    };

    return (
        <form>
            <input
                name="judul"
                value={formData.judul}
                onChange={handleChange}
                placeholder="Judul"
            />
            <input
                name="pengarang"
                value={formData.pengarang}
                onChange={handleChange}
                placeholder="Pengarang"
            />
            <input
                type="number"
                name="harga"
                value={formData.harga}
                onChange={handleChange}
                placeholder="Harga"
            />
            <input
                type="number"
                name="stok"
                value={formData.stok}
                onChange={handleChange}
                placeholder="Stok"
            />
        </form>
    );
}

// State updates are asynchronous
function UpdateCounter() {
    const [count, setCount] = useState(0);

    const handleClick = () => {
        setCount(count + 1);
        console.log(count);  // Still logs old value!
        // React batches state updates
    };

    return <button onClick={handleClick}>Count: {count}</button>;
}
```

**Langkah konkret**: Buat interactive counter dan form dengan useState.

### 9. [[9. Event Handling in React]]

React

```
// Event handler functions
function Button1() {
    const handleClick = () => {
        console.log('Button clicked!');
    };

    return <button onClick={handleClick}>Click me</button>;
}

// Inline handler (less preferred, creates new function each render)
function Button2() {
    return (
        <button onClick={() => console.log('Clicked!')}>
            Click me
        </button>
    );
}

// Passing arguments
function KatalogBuku({ buku }) {
    const handlePinjam = (bukuId, bukuJudul) => {
        console.log(`Pinjam buku: ${bukuJudul} (ID: ${bukuId})`);
    };

    return (
        <div>
            {buku.map(b => (
                <button
                    key={b.id}
                    onClick={() => handlePinjam(b.id, b.judul)}
                >
                    Pinjam {b.judul}
                </button>
            ))}
        </div>
    );
}

// Synthetic events (React wraps native events)
function Form() {
    const handleSubmit = (event) => {
        event.preventDefault();  // Prevent default form submission
        console.log('Form submitted');
    };

    return (
        <form onSubmit={handleSubmit}>
            <input type="text" />
            <button type="submit">Submit</button>
        </form>
    );
}

// Common events
function EventDemo() {
    return (
        <div>
            <button onClick={() => console.log('Click')}>
                onClick
            </button>
            <input
                onChange={e => console.log(e.target.value)}
                placeholder="onChange"
            />
            <input
                onFocus={() => console.log('Focused')}
                placeholder="onFocus"
            />
            <input
                onBlur={() => console.log('Blurred')}
                placeholder="onBlur"
            />
            <form onSubmit={e => { e.preventDefault(); }}>
                onSubmit
            </form>
        </div>
    );
}
```

**Langkah konkret**: Add event handlers untuk forms dan buttons.

### 10. [[10. Controlled Components]]

Controlled components are components whose values are controlled by React state.

React

```
// Controlled input
function SearchBuku() {
    const [searchTerm, setSearchTerm] = useState('');

    return (
        <input
            type="text"
            value={searchTerm}  // Controlled by state
            onChange={e => setSearchTerm(e.target.value)}
            placeholder="Cari buku..."
        />
    );
}

// Controlled checkbox
function FilterByCategory() {
    const [selected, setSelected] = useState({
        teknologi: false,
        fiksi: false,
        sains: false,
    });

    const handleChange = (e) => {
        const { name, checked } = e.target;
        setSelected({
            ...selected,
            [name]: checked
        });
    };

    return (
        <div>
            <label>
                <input
                    type="checkbox"
                    name="teknologi"
                    checked={selected.teknologi}
                    onChange={handleChange}
                />
                Teknologi
            </label>
            <label>
                <input
                    type="checkbox"
                    name="fiksi"
                    checked={selected.fiksi}
                    onChange={handleChange}
                />
                Fiksi
            </label>
            <label>
                <input
                    type="checkbox"
                    name="sains"
                    checked={selected.sains}
                    onChange={handleChange}
                />
                Sains
            </label>
        </div>
    );
}

// Controlled select
function SortBuku() {
    const [sortBy, setSortBy] = useState('judul');

    return (
        <select value={sortBy} onChange={e => setSortBy(e.target.value)}>
            <option value="judul">Sort by Judul</option>
            <option value="harga">Sort by Harga</option>
            <option value="terbaru">Sort by Terbaru</option>
        </select>
    );
}

// Why controlled components?
// 1. Single source of truth (React state)
// 2. Easy to validate input in real-time
// 3. Easy to implement features like auto-save
// 4. Better integration dengan React
```

**Langkah konkret**: Buat controlled form dengan validation.

### 11. [[11. Lifting State Up]]

Jika multiple components perlu share state, lift state ke parent.

React

```
// ❌ BURUK: State terpisah di setiap component
function Parent() {
    return (
        <div>
            <SearchComponent />
            <KatalogComponent />
        </div>
    );
}

function SearchComponent() {
    const [searchTerm, setSearchTerm] = useState('');
    return <input value={searchTerm} onChange={e => setSearchTerm(e.target.value)} />;
}

function KatalogComponent() {
    // searchTerm tidak tersedia di sini!
}

// ✅ BAIK: Lift state ke parent
function Parent() {
    const [searchTerm, setSearchTerm] = useState('');

    const filteredBuku = allBuku.filter(b =>
        b.judul.toLowerCase().includes(searchTerm.toLowerCase())
    );

    return (
        <div>
            <SearchComponent
                searchTerm={searchTerm}
                onSearchChange={setSearchTerm}
            />
            <KatalogComponent buku={filteredBuku} />
        </div>
    );
}

function SearchComponent({ searchTerm, onSearchChange }) {
    return (
        <input
            value={searchTerm}
            onChange={e => onSearchChange(e.target.value)}
            placeholder="Cari buku..."
        />
    );
}

function KatalogComponent({ buku }) {
    return (
        <div>
            {buku.map(b => <BukuCard key={b.id} buku={b} />)}
        </div>
    );
}
```

**Langkah konkret**: Implement search dengan lifting state ke parent.

---

## E. Advanced State Patterns

### 12. [[12. useReducer untuk Complex State]]

Untuk state yang kompleks, gunakan `useReducer` (lebih powerful dari useState).

React

```
import { useReducer } from 'react';

// Reducer function (pure function)
function bukuReducer(state, action) {
    switch (action.type) {
        case 'ADD_BUKU':
            return {
                ...state,
                buku: [...state.buku, action.payload],
            };
        case 'REMOVE_BUKU':
            return {
                ...state,
                buku: state.buku.filter(b => b.id !== action.payload),
            };
        case 'UPDATE_BUKU':
            return {
                ...state,
                buku: state.buku.map(b =>
                    b.id === action.payload.id ? action.payload : b
                ),
            };
        case 'SET_FILTER':
            return {
                ...state,
                filter: action.payload,
            };
        default:
            return state;
    }
}

// Initial state
const initialState = {
    buku: [],
    filter: 'semua',
};

// Component dengan useReducer
function KatalogManager() {
    const [state, dispatch] = useReducer(bukuReducer, initialState);

    const addBuku = (bukuBaru) => {
        dispatch({
            type: 'ADD_BUKU',
            payload: bukuBaru,
        });
    };

    const removeBuku = (bukuId) => {
        dispatch({
            type: 'REMOVE_BUKU',
            payload: bukuId,
        });
    };

    const updateBuku = (bukuUpdate) => {
        dispatch({
            type: 'UPDATE_BUKU',
            payload: bukuUpdate,
        });
    };

    return (
        <div>
            <button
                onClick={() =>
                    addBuku({
                        id: Date.now(),
                        judul: 'New Book',
                        stok: 5,
                    })
                }
            >
                Add Book
            </button>
            {state.buku.map(b => (
                <div key={b.id}>
                    {b.judul}
                    <button onClick={() => removeBuku(b.id)}>Delete</button>
                </div>
            ))}
        </div>
    );
}
```

**Langkah konkret**: Implement CRUD dengan useReducer.

---

## 🏗 Checkpoint Level 2

text

```
PEMAHAMAN:
├── useState hook fundamentals
├── Event handling di React
├── Controlled components
├── Form handling dan validation
├── Lifting state up untuk sharing
├── useReducer untuk complex state
└── State updates trigger re-render

PROYEK: Interactive Perpustakaan App
├── components/SearchBar.jsx — Search dengan useState
├── components/FilterPanel.jsx — Filter buku (kategori, harga)
├── components/BukuList.jsx — Display filtered results
├── components/AddBukuForm.jsx — Add buku form
├── Lifting state ke App.js
├── Filtering dan sorting functionality
├── Form validation
├── useReducer untuk CRUD operations
└── All state-based features working

FITUR:
├── Search buku by judul/pengarang
├── Filter by kategori, harga range
├── Sort by berbagai field
├── Add buku form dengan validation
├── Delete buku
├── Update stok
└── Real-time filtering

STRUKTUR:
src/
├── App.js (main state management)
├── components/
│   ├── SearchBar.jsx
│   ├── FilterPanel.jsx
│   ├── SortOptions.jsx
│   ├── BukuList.jsx
│   ├── BukuCard.jsx
│   └── AddBukuForm.jsx
├── reducers/
│   └── bukuReducer.js
└── data/
    └── initialData.js

HABIT:
├── Lift state up when needed
├── Use useReducer untuk complex state
├── Controlled components untuk inputs
├── Validate form input
├── Good prop names dan documentation
└── Memoize handlers jika needed (nanti)
```

**Commit**: `feat: add state management and interactive forms`

---

# 🟡 LEVEL 3: EFFECTS & DATA FETCHING (Minggu 6-9)

**Tema**: "useEffect hook — Side effects, lifecycle, API calls"

**Benang Merah**: State (Level 2) → useEffect untuk side effects → Data fetching → Async operations

**Output**: App yang bisa fetch data dari backend API

---

## F. Side Effects dengan useEffect

### 13. [[13. useEffect Hook Basics]]

useEffect adalah cara untuk perform side effects (fetch data, update DOM, subscribe, etc).

React

```
import { useEffect, useState } from 'react';

// Run effect after EVERY render
function ComponentWithEffect() {
    useEffect(() => {
        console.log('Component rendered or updated');
        // Cleanup function (optional)
        return () => {
            console.log('Cleanup before unmount or before next effect');
        };
    });

    return <div>Hello</div>;
}

// Run effect ONCE (empty dependency array)
function ComponentWithInitEffect() {
    useEffect(() => {
        console.log('Run once on mount');
        return () => console.log('Cleanup on unmount');
    }, []);  // Empty dependency array = run once

    return <div>Hello</div>;
}

// Run effect when specific dependencies change
function ComponentWithDeps() {
    const [count, setCount] = useState(0);
    const [name, setName] = useState('');

    useEffect(() => {
        console.log('Count changed to:', count);
    }, [count]);  // Run when count changes

    useEffect(() => {
        console.log('Name changed to:', name);
    }, [name]);  // Run when name changes

    return (
        <div>
            <button onClick={() => setCount(count + 1)}>Count: {count}</button>
            <input value={name} onChange={e => setName(e.target.value)} />
        </div>
    );
}

// Fetch data dari API
function BukuList() {
    const [buku, setBuku] = useState([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);

    useEffect(() => {
        // Define async function
        const fetchBuku = async () => {
            try {
                setLoading(true);
                const response = await fetch('http://localhost:3000/api/buku');
                if (!response.ok) {
                    throw new Error('Failed to fetch');
                }
                const data = await response.json();
                setBuku(data.data);
                setError(null);
            } catch (err) {
                setError(err.message);
                setBuku([]);
            } finally {
                setLoading(false);
            }
        };

        // Call function
        fetchBuku();
    }, []);  // Fetch once on mount

    if (loading) return <p>Loading...</p>;
    if (error) return <p>Error: {error}</p>;

    return (
        <div>
            {buku.map(b => (
                <BukuCard key={b.id} buku={b} />
            ))}
        </div>
    );
}
```

**Langkah konkret**: Fetch data dari API dan display di komponen.

### 14. [[14. Data Fetching Patterns]]

React

```
import { useState, useEffect } from 'react';

// Pattern 1: Loading, Error, Data states
function BukuList() {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);

    useEffect(() => {
        const fetchData = async () => {
            try {
                const response = await fetch('/api/buku');
                if (!response.ok) throw new Error(`HTTP ${response.status}`);
                setData(await response.json());
            } catch (e) {
                setError(e.message);
            } finally {
                setLoading(false);
            }
        };

        fetchData();
    }, []);

    if (loading) return <div className="spinner">Loading...</div>;
    if (error) return <div className="error">Error: {error}</div>;
    if (!data) return <div>No data</div>;

    return (
        <div>
            {data.map(item => (
                <div key={item.id}>{item.judul}</div>
            ))}
        </div>
    );
}

// Pattern 2: Custom hook untuk data fetching
function useFetch(url) {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);

    useEffect(() => {
        const fetchData = async () => {
            try {
                const response = await fetch(url);
                if (!response.ok) throw new Error(`HTTP ${response.status}`);
                setData(await response.json());
            } catch (e) {
                setError(e);
            } finally {
                setLoading(false);
            }
        };

        fetchData();
    }, [url]);

    return { data, loading, error };
}

// Usage
function App() {
    const { data: buku, loading, error } = useFetch('/api/buku');

    if (loading) return <p>Loading...</p>;
    if (error) return <p>Error: {error.message}</p>;

    return <div>{buku?.map(b => <div key={b.id}>{b.judul}</div>)}</div>;
}

// Pattern 3: Fetch dengan query parameters
function SearchBuku() {
    const [searchTerm, setSearchTerm] = useState('');
    const [results, setResults] = useState([]);
    const [loading, setLoading] = useState(false);

    useEffect(() => {
        if (!searchTerm) {
            setResults([]);
            return;
        }

        const fetchResults = async () => {
            setLoading(true);
            try {
                const query = new URLSearchParams({ search: searchTerm });
                const response = await fetch(`/api/buku/search?${query}`);
                const data = await response.json();
                setResults(data.data);
            } catch (e) {
                console.error(e);
            } finally {
                setLoading(false);
            }
        };

        // Debounce: tunggu 300ms sebelum fetch
        const timer = setTimeout(fetchResults, 300);
        return () => clearTimeout(timer);
    }, [searchTerm]);

    return (
        <div>
            <input
                value={searchTerm}
                onChange={e => setSearchTerm(e.target.value)}
                placeholder="Cari buku..."
            />
            {loading && <p>Searching...</p>}
            {results.map(b => (
                <div key={b.id}>{b.judul}</div>
            ))}
        </div>
    );
}
```

**Langkah konkret**: Create useFetch hook, implement search dengan debounce.

### 15. [[15. useEffect Cleanup and Dependencies]]

React

```
// Memory leaks: unsub dari subscriptions
function UserProfile() {
    const [user, setUser] = useState(null);

    useEffect(() => {
        // Subscribe ke user data
        const unsubscribe = subscribeToUser(userId, (userData) => {
            setUser(userData);
        });

        // Cleanup: unsubscribe saat component unmount
        return () => {
            unsubscribe();
        };
    }, [userId]);  // Re-subscribe jika userId berubah

    return <div>{user?.nama}</div>;
}

// Abort fetch request jika unmount sebelum selesai
function BukuDetail({ bukuId }) {
    const [buku, setBuku] = useState(null);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        const controller = new AbortController();

        const fetchBuku = async () => {
            try {
                const response = await fetch(`/api/buku/${bukuId}`, {
                    signal: controller.signal,
                });
                const data = await response.json();
                setBuku(data);
            } catch (e) {
                if (e.name === 'AbortError') {
                    console.log('Fetch aborted');
                } else {
                    console.error(e);
                }
            } finally {
                setLoading(false);
            }
        };

        fetchBuku();

        // Cleanup: abort fetch if unmount
        return () => controller.abort();
    }, [bukuId]);

    if (loading) return <p>Loading...</p>;
    return <div>{buku?.judul}</div>;
}

// Dependency array gotchas
function Example() {
    const [count, setCount] = useState(0);

    // ❌ INFINITE LOOP: no dependency array
    // useEffect(() => {
    //     setCount(count + 1);  // Re-render → effect runs → count changes → re-render
    // });

    // ✅ CORRECT: empty dependency array
    useEffect(() => {
        console.log('Run once');
    }, []);

    // ✅ CORRECT: track specific dependency
    useEffect(() => {
        console.log('Count changed:', count);
    }, [count]);

    // ⚠️ Gotcha: object as dependency
    const config = { theme: 'dark' };

    // ❌ RUNS EVERY RENDER: new object reference each time
    // useEffect(() => {
    //     console.log(config);
    // }, [config]);

    // ✅ CORRECT: specify primitive value
    const theme = 'dark';
    useEffect(() => {
        console.log(theme);
    }, [theme]);
}
```

**Langkah konkret**: Implement proper cleanup dalam effects.

---

## G. Advanced Effects

### 16. [[16. Custom Hooks]]

Custom hooks adalah functions yang start dengan "use" dan call other hooks.

React

```
// useFetch custom hook
function useFetch(url) {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);

    useEffect(() => {
        const controller = new AbortController();

        const fetchData = async () => {
            try {
                const response = await fetch(url, { signal: controller.signal });
                if (!response.ok) throw new Error(`HTTP ${response.status}`);
                setData(await response.json());
            } catch (e) {
                if (e.name !== 'AbortError') {
                    setError(e);
                }
            } finally {
                setLoading(false);
            }
        };

        fetchData();
        return () => controller.abort();
    }, [url]);

    return { data, loading, error };
}

// useAsync custom hook
function useAsync(asyncFunction, immediate = true) {
    const [status, setStatus] = useState('idle');
    const [result, setResult] = useState(null);
    const [error, setError] = useState(null);

    const execute = useCallback(async () => {
        setStatus('pending');
        try {
            const response = await asyncFunction();
            setResult(response);
            setStatus('success');
            return response;
        } catch (e) {
            setError(e);
            setStatus('error');
        }
    }, [asyncFunction]);

    useEffect(() => {
        if (immediate) {
            execute();
        }
    }, [execute, immediate]);

    return { execute, status, result, error };
}

// Usage
function BukuSearch() {
    const [searchTerm, setSearchTerm] = useState('');
    const { data, loading, error } = useFetch(
        `/api/buku/search?q=${searchTerm}`
    );

    return (
        <div>
            <input
                value={searchTerm}
                onChange={e => setSearchTerm(e.target.value)}
            />
            {loading && <p>Loading...</p>}
            {error && <p>Error: {error.message}</p>}
            {data?.map(b => <div key={b.id}>{b.judul}</div>)}
        </div>
    );
}
```

**Langkah konkret**: Create reusable custom hooks.

---

## 🏗 Checkpoint Level 3

text

```
PEMAHAMAN:
├── useEffect hook dan lifecycle
├── Data fetching di useEffect
├── Dependency arrays
├── Cleanup functions
├── Aborting requests
├── Custom hooks
├── Async/await dalam components
└── Error handling di data fetching

PROYEK: Connected to Backend API
├── useFetch custom hook
├── BukuList component fetch dari API
├── Search dengan debounce
├── Loading dan error states
├── BukuDetail component
├── Add/Edit/Delete dengan API calls
├── Proper error handling
├── No memory leaks
├── Abort requests on unmount
└── All CRUD operations via API

API INTEGRATION:
├── GET /api/buku — Get all books
├── GET /api/buku/:id — Get single book
├── POST /api/buku — Create book
├── PUT /api/buku/:id — Update book
├── DELETE /api/buku/:id — Delete book
├── GET /api/buku/search — Search books
└── Proper HTTP methods and status codes

STRUKTUR:
src/
├── App.js
├── hooks/
│   ├── useFetch.js
│   └── useAsync.js
├── components/
│   ├── BukuList.jsx
│   ├── BukuDetail.jsx
│   ├── AddBukuForm.jsx
│   └── SearchBuku.jsx
└── utils/
    └── api.js (API constants)

HABIT:
├── Always handle loading state
├── Always handle error state
├── Use AbortController untuk cleanup
├── Create custom hooks untuk reusability
├── Debounce expensive operations
├── Proper error messages to user
└── No console errors or warnings
```

**Commit**: `feat: add data fetching with useEffect and custom hooks`

---

# 🟠 LEVEL 4: ROUTING (Minggu 9-12)

**Tema**: "React Router — Multi-page application"

**Benang Merah**: Single component app → Multiple pages/routes → Navigation → Nested routes

**Output**: Full multi-page perpustakaan application

---

## H. React Router v6

### 17. [[17. React Router Setup dan Basic Routing]]

Bash

```
npm install react-router-dom
```

**File: src/App.jsx**

React

```
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Layout from './components/Layout';
import Home from './pages/Home';
import KatalogPage from './pages/KatalogPage';
import BukuDetailPage from './pages/BukuDetailPage';
import NotFoundPage from './pages/NotFoundPage';

function App() {
    return (
        <BrowserRouter>
            <Routes>
                <Route element={<Layout />}>
                    <Route path="/" element={<Home />} />
                    <Route path="/katalog" element={<KatalogPage />} />
                    <Route path="/buku/:id" element={<BukuDetailPage />} />
                    <Route path="*" element={<NotFoundPage />} />
                </Route>
            </Routes>
        </BrowserRouter>
    );
}

export default App;
```

**File: src/components/Layout.jsx**

React

```
import { Outlet, Link } from 'react-router-dom';

function Layout() {
    return (
        <div className="app">
            <header className="header">
                <nav className="navbar">
                    <Link to="/">Home</Link>
                    <Link to="/katalog">Katalog</Link>
                    <Link to="/peminjaman">Peminjaman Saya</Link>
                </nav>
            </header>
            <main className="main">
                <Outlet />  {/* Render current page component */}
            </main>
            <footer className="footer">
                <p>© 2024 Perpustakaan Digital</p>
            </footer>
        </div>
    );
}

export default Layout;
```

**File: src/pages/Home.jsx**

React

```
function Home() {
    return (
        <div className="home">
            <h1>Selamat datang di Perpustakaan Digital</h1>
            <p>Jelajahi koleksi buku kami</p>
        </div>
    );
}

export default Home;
```

**Langkah konkret**: Setup routing dengan React Router.

### 18. [[18. URL Parameters dan Navigation]]

React

```
import { useParams, useNavigate } from 'react-router-dom';

// Get URL parameters
function BukuDetailPage() {
    const { id } = useParams();  // Get ID from URL
    const [buku, setBuku] = useState(null);

    useEffect(() => {
        fetch(`/api/buku/${id}`)
            .then(r => r.json())
            .then(d => setBuku(d.data));
    }, [id]);

    return (
        <div>
            {buku && (
                <>
                    <h1>{buku.judul}</h1>
                    <p>{buku.pengarang}</p>
                </>
            )}
        </div>
    );
}

// Programmatic navigation
function BukuCard({ buku }) {
    const navigate = useNavigate();

    const handlePinjam = async () => {
        const response = await fetch(`/api/buku/${buku.id}/pinjam`, {
            method: 'POST',
        });
        if (response.ok) {
            navigate(`/peminjaman`);  // Navigate after action
        }
    };

    return (
        <div>
            <h2>{buku.judul}</h2>
            <button onClick={handlePinjam}>Pinjam</button>
        </div>
    );
}

// Link dengan styling (active state)
import { NavLink } from 'react-router-dom';

function Navigation() {
    return (
        <nav>
            <NavLink
                to="/katalog"
                className={({ isActive }) => isActive ? 'active' : ''}
            >
                Katalog
            </NavLink>
            <NavLink
                to="/peminjaman"
                className={({ isActive }) => isActive ? 'active' : ''}
            >
                Peminjaman
            </NavLink>
        </nav>
    );
}
```

**Langkah konkret**: Implement dynamic routes dan navigation.

### 19. [[19. Nested Routes dan Protected Routes]]

React

```
// Nested routes
<Routes>
    <Route element={<Layout />}>
        <Route path="/" element={<Home />} />
        <Route path="/katalog" element={<KatalogLayout />}>
            <Route index element={<KatalogPage />} />
            <Route path=":id" element={<BukuDetailPage />} />
            <Route path="edit/:id" element={<EditBukuPage />} />
        </Route>
    </Route>
</Routes>

// Protected route (requires auth)
function ProtectedRoute({ element }) {
    const { user } = useContext(AuthContext);

    if (!user) {
        return <Navigate to="/login" replace />;
    }

    return element;
}

// Usage
<Routes>
    <Route element={<Layout />}>
        <Route path="/" element={<Home />} />
        <Route path="/login" element={<LoginPage />} />
        <Route
            path="/admin"
            element={<ProtectedRoute element={<AdminPanel />} />}
        />
    </Route>
</Routes>

// Query parameters (search, filters)
import { useSearchParams } from 'react-router-dom';

function KatalogPage() {
    const [searchParams, setSearchParams] = useSearchParams();

    const kategori = searchParams.get('kategori') || '';
    const sortBy = searchParams.get('sort') || 'judul';

    const handleFilterChange = (newKategori) => {
        setSearchParams({ kategori: newKategori, sort: sortBy });
    };

    return (
        <div>
            <select onChange={e => handleFilterChange(e.target.value)}>
                <option value="">Semua</option>
                <option value="teknologi">Teknologi</option>
            </select>
        </div>
    );
}
```

**Langkah konkret**: Implement nested routes dan protected routes.

---

## 🏗 Checkpoint Level 4

text

```
PEMAHAMAN:
├── BrowserRouter dan Routes setup
├── Basic routing dengan Route
├── URL parameters (:id)
├── Query parameters (?kategori=tech)
├── Link dan NavLink components
├── useParams, useNavigate, useSearchParams
├── Nested routes
├── Protected routes
├── 404 handling
└── Route-based code splitting (optional)

PROYEK: Multi-Page Perpustakaan App
├── pages/
│   ├── Home.jsx
│   ├── KatalogPage.jsx
│   ├── BukuDetailPage.jsx
│   ├── PeminjamanPage.jsx
│   ├── ProfilePage.jsx
│   ├── LoginPage.jsx
│   └── NotFoundPage.jsx
├── components/
│   ├── Layout.jsx
│   ├── Navigation.jsx
│   └── ProtectedRoute.jsx
├── App.jsx (routing setup)
└── index.js

ROUTES:
├── / — Home page
├── /katalog — Books list (with filters)
├── /katalog/:id — Book detail
├── /peminjaman — My loans
├── /peminjaman/:id — Loan detail
├── /profile — User profile
├── /login — Login page
├── /logout — Logout
├── /admin — Admin panel (protected)
└── * — 404 page

FEATURES:
├── Navigation between pages
├── URL parameters for dynamic content
├── Query parameters for filters
├── Active link highlighting
├── Back button navigation
├── 404 page for invalid routes
├── Programmatic navigation after actions
└── Proper routing for all features

HABIT:
├── Use component-based page structure
├── Link/NavLink untuk internal navigation
├── useParams untuk dynamic routes
├── useSearchParams untuk filters
├── Protected routes untuk auth
├── Clear URL structure
└── Proper 404 handling
```

**Commit**: `feat: add React Router with multi-page navigation`

---

# 🔴 LEVEL 5: STATE MANAGEMENT (Minggu 12-15)

**Tema**: "Beyond Props — Context API, Redux/Zustand untuk scalable state"

**Benang Merah**: Prop drilling problem → Context API → Redux/Zustand for complex state

**Output**: Proper state management across entire app

---

## I. Context API

### 20. [[20. useContext dan Context Provider]]

React

```
import { createContext, useContext, useState } from 'react';

// 1. Create context
const BukuContext = createContext();

// 2. Create provider component
function BukuProvider({ children }) {
    const [buku, setBuku] = useState([]);
    const [loading, setLoading] = useState(false);
    const [error, setError] = useState(null);

    const fetchBuku = async () => {
        setLoading(true);
        try {
            const response = await fetch('/api/buku');
            const data = await response.json();
            setBuku(data.data);
        } catch (e) {
            setError(e.message);
        } finally {
            setLoading(false);
        }
    };

    const addBuku = async (bukuData) => {
        const response = await fetch('/api/buku', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(bukuData),
        });
        const newBuku = await response.json();
        setBuku([...buku, newBuku.data]);
    };

    const updateBuku = async (id, updates) => {
        const response = await fetch(`/api/buku/${id}`, {
            method: 'PUT',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(updates),
        });
        const updated = await response.json();
        setBuku(buku.map(b => b.id === id ? updated.data : b));
    };

    const deleteBuku = async (id) => {
        await fetch(`/api/buku/${id}`, { method: 'DELETE' });
        setBuku(buku.filter(b => b.id !== id));
    };

    const value = {
        buku,
        loading,
        error,
        fetchBuku,
        addBuku,
        updateBuku,
        deleteBuku,
    };

    return (
        <BukuContext.Provider value={value}>
            {children}
        </BukuContext.Provider>
    );
}

// 3. Custom hook untuk use context
function useBuku() {
    const context = useContext(BukuContext);
    if (!context) {
        throw new Error('useBuku must be used within BukuProvider');
    }
    return context;
}

// 4. Use in App
function App() {
    return (
        <BukuProvider>
            <MainApp />
        </BukuProvider>
    );
}

// 5. Use in components
function KatalogPage() {
    const { buku, loading, error, fetchBuku } = useBuku();

    useEffect(() => {
        fetchBuku();
    }, []);

    if (loading) return <p>Loading...</p>;
    if (error) return <p>Error: {error}</p>;

    return (
        <div>
            {buku.map(b => (
                <BukuCard key={b.id} buku={b} />
            ))}
        </div>
    );
}
```

**Langkah konkret**: Implement Context untuk global buku state.

### 21. [[21. User/Auth Context]]

React

```
import { createContext, useContext, useState, useEffect } from 'react';

const AuthContext = createContext();

function AuthProvider({ children }) {
    const [user, setUser] = useState(null);
    const [loading, setLoading] = useState(true);

    // Check if user already logged in
    useEffect(() => {
        const checkAuth = async () => {
            try {
                const response = await fetch('/api/me');
                if (response.ok) {
                    const data = await response.json();
                    setUser(data.data);
                }
            } catch (e) {
                console.error(e);
            } finally {
                setLoading(false);
            }
        };

        checkAuth();
    }, []);

    const login = async (email, password) => {
        const response = await fetch('/api/login', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ email, password }),
        });
        const data = await response.json();
        setUser(data.user);
        localStorage.setItem('token', data.token);
        return data;
    };

    const logout = async () => {
        await fetch('/api/logout', { method: 'POST' });
        setUser(null);
        localStorage.removeItem('token');
    };

    const value = {
        user,
        loading,
        login,
        logout,
        isAuthenticated: !!user,
    };

    return (
        <AuthContext.Provider value={value}>
            {children}
        </AuthContext.Provider>
    );
}

function useAuth() {
    const context = useContext(AuthContext);
    if (!context) {
        throw new Error('useAuth must be used within AuthProvider');
    }
    return context;
}

// Usage
function Profile() {
    const { user, logout } = useAuth();

    return (
        <div>
            <p>Hello, {user?.nama}</p>
            <button onClick={logout}>Logout</button>
        </div>
    );
}
```

**Langkah konkret**: Implement Auth context dengan login/logout.

---

## J. Redux atau Zustand (Choose One)

### 22. [[22. Zustand (Modern, Simpler Alternative to Redux)]]

Bash

```
npm install zustand
```

React

```
import { create } from 'zustand';

// Create store
const useBukuStore = create((set, get) => ({
    // State
    buku: [],
    loading: false,
    error: null,

    // Actions
    fetchBuku: async () => {
        set({ loading: true });
        try {
            const response = await fetch('/api/buku');
            const data = await response.json();
            set({ buku: data.data, error: null });
        } catch (e) {
            set({ error: e.message });
        } finally {
            set({ loading: false });
        }
    },

    addBuku: (bukuBaru) => {
        const current = get().buku;
        set({ buku: [...current, bukuBaru] });
    },

    removeBuku: (id) => {
        set(state => ({
            buku: state.buku.filter(b => b.id !== id),
        }));
    },

    updateBuku: (id, updates) => {
        set(state => ({
            buku: state.buku.map(b =>
                b.id === id ? { ...b, ...updates } : b
            ),
        }));
    },
}));

// Usage in components
function KatalogPage() {
    const { buku, loading, fetchBuku } = useBukuStore();

    useEffect(() => {
        fetchBuku();
    }, []);

    return (
        <div>
            {loading && <p>Loading...</p>}
            {buku.map(b => (
                <BukuCard key={b.id} buku={b} />
            ))}
        </div>
    );
}

// Multiple stores (separate concerns)
const useAuthStore = create((set) => ({
    user: null,
    login: (userData) => set({ user: userData }),
    logout: () => set({ user: null }),
}));

const usePeminjamanStore = create((set) => ({
    peminjaman: [],
    addPeminjaman: (peminjaman) =>
        set(state => ({
            peminjaman: [...state.peminjaman, peminjaman],
        })),
}));
```

**Langkah konkret**: Implement Zustand stores untuk main features.

---

## 🏗 Checkpoint Level 5

text

```
PEMAHAMAN:
├── Context API fundamentals
├── useContext hook
├── Custom hooks wrapping useContext
├── Provider pattern
├── Multiple contexts
├── Global state vs component state
├── Zustand (or Redux) basics
├── Async actions dalam stores
└── State management patterns

PROYEK: Global State Management
├── contexts/ atau stores/
│   ├── BukuContext/Store
│   ├── AuthContext/Store
│   ├── PeminjamanContext/Store
│   └── UIContext/Store (dark mode, etc)
├── Providers wrapping App
├── Custom hooks (useBuku, useAuth, usePeminjaman)
├── All CRUD operations via context/store
├── Auth flows (login, logout, token)
├── Persisting state to localStorage
└── No prop drilling

ARCHITECTURE:
├── Context/Zustand untuk global state
├── useState untuk local component state
├── useEffect untuk side effects
├── Custom hooks untuk reusability
└── Clear separation of concerns

HABIT:
├── Global state only untuk truly global data
├── Keep context focused (not mega context)
├── Custom hooks for accessing context
├── Error boundaries around providers
├── DevTools untuk debugging (Zustand/Redux)
└── Good naming conventions
```

**Commit**: `feat: add global state management with Context/Zustand`

---

# ⚫ LEVEL 6: ADVANCED PATTERNS & PERFORMANCE (Minggu 15-19)

**Tema**: "Performance optimization, memoization, code splitting"

**Benang Merah**: Functional app (Level 5) → Optimization → Advanced patterns → Production-ready

**Output**: Performant, optimized React application

---

## K. Performance Optimization

### 23. [[23. React.memo dan useCallback]]

React

```
import { memo, useCallback, useMemo } from 'react';

// Re-render happens when:
// 1. Component's state changes
// 2. Component's props change
// 3. Parent re-renders

// Problem: Child re-renders even if props don't change
function Parent() {
    const [count, setCount] = useState(0);

    const handlePinjam = () => {
        console.log('Pinjam buku');
    };

    return (
        <div>
            <button onClick={() => setCount(count + 1)}>
                Count: {count}
            </button>
            {/* BukuCard re-renders saat count berubah, meski bukuId sama! */}
            <BukuCard bukuId={1} onPinjam={handlePinjam} />
        </div>
    );
}

// Solution 1: React.memo (memoize component)
const BukuCard = memo(function BukuCard({ bukuId, onPinjam }) {
    console.log('BukuCard rendered');

    return (
        <div>
            <h3>Buku {bukuId}</h3>
            <button onClick={onPinjam}>Pinjam</button>
        </div>
    );
});

// Tapi memo tidak membantu jika handlePinjam berubah setiap render
// handlePinjam adalah function baru setiap kali Parent render

// Solution 2: useCallback (memoize function)
function Parent() {
    const [count, setCount] = useState(0);

    // Memoize function — hanya buat ulang jika dependencies berubah
    const handlePinjam = useCallback(() => {
        console.log('Pinjam buku');
    }, []);  // No dependencies = sama function instance selamanya

    return (
        <div>
            <button onClick={() => setCount(count + 1)}>
                Count: {count}
            </button>
            {/* Now BukuCard won't re-render! */}
            <BukuCard bukuId={1} onPinjam={handlePinjam} />
        </div>
    );
}

// Solution 3: useMemo (memoize value)
function KatalogPage() {
    const [buku, setBuku] = useState([]);
    const [filter, setFilter] = useState('');

    // This is expensive operation
    const filteredBuku = buku.filter(b =>
        b.judul.toLowerCase().includes(filter.toLowerCase())
    );

    // Memoize the result
    const memoizedFiltered = useMemo(
        () => buku.filter(b =>
            b.judul.toLowerCase().includes(filter.toLowerCase())
        ),
        [buku, filter]  // Only recompute if buku or filter changes
    );

    return (
        <div>
            <input value={filter} onChange={e => setFilter(e.target.value)} />
            {memoizedFiltered.map(b => (
                <BukuCard key={b.id} buku={b} />
            ))}
        </div>
    );
}
```

**Langkah konkret**: Profile app, identify bottlenecks, apply memoization.

### 24. [[24. Code Splitting dan Lazy Loading]]

React

```
import { lazy, Suspense } from 'react';
import { Routes, Route } from 'react-router-dom';

// Lazy load components — bundle only pada demand
const Home = lazy(() => import('./pages/Home'));
const KatalogPage = lazy(() => import('./pages/KatalogPage'));
const AdminPanel = lazy(() => import('./pages/AdminPanel'));

// Loading fallback
function Loading() {
    return <div className="spinner">Loading...</div>;
}

function App() {
    return (
        <Routes>
            <Route path="/" element={
                <Suspense fallback={<Loading />}>
                    <Home />
                </Suspense>
            } />
            <Route path="/katalog" element={
                <Suspense fallback={<Loading />}>
                    <KatalogPage />
                </Suspense>
            } />
            <Route path="/admin" element={
                <Suspense fallback={<Loading />}>
                    <AdminPanel />
                </Suspense>
            } />
        </Routes>
    );
}
```

**Langkah konkret**: Implement code splitting untuk routes.

---

## L. Advanced Patterns

### 25. [[25. Higher-Order Components (HOC) dan Render Props]]

React

```
// HOC Example: withAuth (redirect jika tidak login)
function withAuth(Component) {
    return function Protected(props) {
        const { isAuthenticated, loading } = useAuth();

        if (loading) return <p>Loading...</p>;
        if (!isAuthenticated) {
            return <Navigate to="/login" replace />;
        }

        return <Component {...props} />;
    };
}

// Usage
const ProtectedAdmin = withAuth(AdminPanel);

// Render Props Example
function withData(Component, url) {
    return function DataWrapper(props) {
        const { data, loading, error } = useFetch(url);

        return (
            <Component
                {...props}
                data={data}
                loading={loading}
                error={error}
            />
        );
    };
}

const BukuListWithData = withData(BukuList, '/api/buku');
```

**Langkah konkret**: Create HOC untuk common patterns.

---

## 🏗 Checkpoint Level 6

text

```
PEMAHAMAN:
├── React.memo untuk preventing re-renders
├── useCallback untuk memoizing functions
├── useMemo untuk memoizing values
├── Code splitting dengan lazy/Suspense
├── Performance profiling
├── Bundle analysis
├── HOC pattern
├── Error boundaries
└── Optimistic updates

PROYEK: Optimized Production App
├── Applied React.memo to list items
├── useCallback for event handlers
├── useMemo untuk expensive computations
├── Code splitting per route
├── Lazy loading untuk modals/components
├── Error boundaries implemented
├── Loading states untuk async operations
├── Optimistic updates untuk UX
└── <1 second initial load target

PERFORMANCE:
├── Lighthouse score > 80
├── Core Web Vitals good
├── Bundle size < 150KB (gzipped)
├── Time to Interactive < 3s
├── No unnecessary re-renders
└── Smooth animations at 60fps

HABIT:
├── Profile regularly dengan DevTools
├── Don't optimize prematurely
├── memo/useCallback only when needed
├── Code splitting per route
├── Error boundaries for safety
└── Monitor real-world performance
```

**Commit**: `feat: optimize performance with memoization and code splitting`

---

# 🔴 LEVEL 7: TESTING & TYPESCRIPT (Minggu 19-23)

**Tema**: "Testing React components, TypeScript for type safety"

**Benang Merah**: Untested app → Unit tests → Integration tests → TypeScript all the way

**Output**: Fully tested, type-safe React application

---

## M. Testing React

### 26. [[26. Jest dan React Testing Library]]

Bash

```
npm install --save-dev @testing-library/react @testing-library/jest-dom jest
```

React

```
// Example component
function BukuCard({ buku, onPinjam }) {
    return (
        <div className="card">
            <h2>{buku.judul}</h2>
            <p>{buku.pengarang}</p>
            <p>Rp {buku.harga.toLocaleString('id-ID')}</p>
            <button onClick={() => onPinjam(buku.id)}>Pinjam</button>
        </div>
    );
}

// Test file
import { render, screen, fireEvent } from '@testing-library/react';
import BukuCard from './BukuCard';

describe('BukuCard', () => {
    const mockBuku = {
        id: 1,
        judul: 'Clean Code',
        pengarang: 'Robert Martin',
        harga: 150000,
    };

    it('renders book information', () => {
        render(<BukuCard buku={mockBuku} onPinjam={() => {}} />);

        expect(screen.getByText('Clean Code')).toBeInTheDocument();
        expect(screen.getByText('Robert Martin')).toBeInTheDocument();
    });

    it('calls onPinjam when button clicked', () => {
        const mockOnPinjam = jest.fn();
        render(<BukuCard buku={mockBuku} onPinjam={mockOnPinjam} />);

        const button = screen.getByRole('button', { name: /pinjam/i });
        fireEvent.click(button);

        expect(mockOnPinjam).toHaveBeenCalledWith(1);
    });

    it('formats price correctly', () => {
        render(<BukuCard buku={mockBuku} onPinjam={() => {}} />);

        expect(screen.getByText('Rp 150.000')).toBeInTheDocument();
    });
});
```

**Langkah konkret**: Write tests untuk main components.

### 27. [[27. TypeScript in React]]

Bash

```
npm install --save-dev typescript @types/react @types/react-dom
npx tsc --init
```

React

```
// Type definitions
interface Buku {
    id: number;
    judul: string;
    pengarang: string;
    harga: number;
    stok: number;
    kategori: 'teknologi' | 'fiksi' | 'sains' | 'non-fiksi';
}

interface BukuCardProps {
    buku: Buku;
    onPinjam: (bukuId: number) => void;
    variant?: 'normal' | 'compact';
}

// Typed component
const BukuCard: React.FC<BukuCardProps> = ({
    buku,
    onPinjam,
    variant = 'normal',
}) => {
    return (
        <div className={`card ${variant}`}>
            <h2>{buku.judul}</h2>
            <p>{buku.pengarang}</p>
            <p>Rp {buku.harga.toLocaleString('id-ID')}</p>
            <button onClick={() => onPinjam(buku.id)}>
                Pinjam
            </button>
        </div>
    );
};

// Typed hooks
interface UseBukuReturn {
    buku: Buku[];
    loading: boolean;
    error: string | null;
    fetchBuku: () => Promise<void>;
}

function useBuku(): UseBukuReturn {
    const [buku, setBuku] = useState<Buku[]>([]);
    const [loading, setLoading] = useState(false);
    const [error, setError] = useState<string | null>(null);

    const fetchBuku = async () => {
        setLoading(true);
        try {
            const response = await fetch('/api/buku');
            const data: { data: Buku[] } = await response.json();
            setBuku(data.data);
        } catch (e) {
            setError(e instanceof Error ? e.message : 'Unknown error');
        } finally {
            setLoading(false);
        }
    };

    return { buku, loading, error, fetchBuku };
}

// Typed Context
interface AuthContextType {
    user: User | null;
    login: (email: string, password: string) => Promise<void>;
    logout: () => void;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);
```

**Langkah konkret**: Migrate main components to TypeScript.

---

## 🏗 Checkpoint Level 7

text

```
PEMAHAMAN:
├── Jest testing framework
├── React Testing Library basics
├── Unit tests untuk components
├── Integration tests
├── Mock functions dan API calls
├── TypeScript fundamentals
├── Type definitions untuk components
├── Interface dan type definitions
└── Generic types

PROYEK: Tested & Type-Safe App
├── Unit tests untuk all components
├── Integration tests untuk features
├── API mocking dengan Jest
├── Snapshot tests (optional)
├── >80% code coverage
├── TypeScript for entire codebase
├── Proper type definitions
├── Typed props dan state
├── Typed custom hooks
└── Typed context

TESTING:
├── tests/ directory structure
├── *.test.tsx for component tests
├── Mock data factories
├── API mocks (Mock Service Worker)
├── Component interaction tests
└── Error state tests

TYPESCRIPT:
├── tsconfig.json configured
├── Interface for each component props
├── Type for API responses
├── Generic components
├── Strict mode enabled
└── No implicit any

HABIT:
├── Test user interactions, not implementation
├── Mock external dependencies
├── Type-first development
├── Use strict TypeScript
├── Good test naming
└── Maintain >70% coverage
```

**Commit**: `feat: add comprehensive tests and TypeScript migration`

---

# 🟣 LEVEL 8: ADVANCED TOPICS (Minggu 23+)

**Tema**: "Choose your path — Next.js, TanStack Query, or specialized patterns"

---

## N. Advanced Paths (Choose One)

### 28. [[28. Next.js — Full-Stack React Framework]]

Bash

```
npx create-next-app@latest perpustakaan-nextjs
```

Next.js provides:

- ✅ Server-Side Rendering (SSR)
- ✅ Static Generation (SSG)
- ✅ File-based routing
- ✅ API routes (backend dalam Next.js)
- ✅ Incremental Static Regeneration (ISR)

React

```
// pages/index.tsx
export async function getStaticProps() {
    const buku = await fetch('http://localhost:3000/api/buku')
        .then(r => r.json());
    return { props: { buku }, revalidate: 60 };
}

export default function Home({ buku }) {
    return <div>{buku.map(b => <div key={b.id}>{b.judul}</div>)}</div>;
}

// pages/api/buku.ts (API endpoint)
import { NextApiRequest, NextApiResponse } from 'next';

export default function handler(
    req: NextApiRequest,
    res: NextApiResponse
) {
    if (req.method === 'GET') {
        res.status(200).json({ data: [] });
    }
}
```

### 29. [[29. TanStack Query (Data Fetching Revolution)]]

Bash

```
npm install @tanstack/react-query
```

TanStack Query handles:

- ✅ Automatic caching
- ✅ Background refetching
- ✅ Request deduplication
- ✅ Stale data management

React

```
import { useQuery } from '@tanstack/react-query';

function BukuList() {
    const { data, isLoading, error } = useQuery({
        queryKey: ['buku'],
        queryFn: async () => {
            const response = await fetch('/api/buku');
            return response.json();
        },
    });

    if (isLoading) return <p>Loading...</p>;
    if (error) return <p>Error: {error.message}</p>;

    return (
        <div>
            {data?.map(b => (
                <BukuCard key={b.id} buku={b} />
            ))}
        </div>
    );
}
```

### 30. [[30. Form Libraries — React Hook Form + Zod]]

Bash

```
npm install react-hook-form zod @hookform/resolvers
```

React

```
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import * as z from 'zod';

const bukuSchema = z.object({
    judul: z.string().min(3),
    pengarang: z.string().min(2),
    harga: z.number().positive(),
    stok: z.number().int().min(0),
});

type BukuInput = z.infer<typeof bukuSchema>;

function AddBukuForm() {
    const { register, handleSubmit, formState: { errors } } = useForm<BukuInput>({
        resolver: zodResolver(bukuSchema),
    });

    const onSubmit = (data: BukuInput) => {
        console.log(data);
    };

    return (
        <form onSubmit={handleSubmit(onSubmit)}>
            <input {...register('judul')} />
            {errors.judul && <span>{errors.judul.message}</span>}

            <input type="number" {...register('harga', { valueAsNumber: true })} />
            {errors.harga && <span>{errors.harga.message}</span>}

            <button type="submit">Add Book</button>
        </form>
    );
}
```

---

# 📊 Ringkasan Progress Tracking

text

```
Level 1: Components & Props → Reusable UI building blocks
 + Level 2: State & Events → Interactive components
 + Level 3: Effects & Data Fetching → API integration
 + Level 4: Routing → Multi-page applications
 + Level 5: State Management → Scalable architecture
 + Level 6: Performance → Optimized production apps
 + Level 7: Testing & TypeScript → Quality & safety
 + Level 8: Advanced (Next.js/TanStack Query) → Specialized tools
```

## Tabel Progress

|Level|Poin|Durasi|Output Konkret|
|---|---|---|---|
|🟢 1|1-7|Minggu 1-3|Component library|
|🔵 2|8-12|Minggu 3-6|Interactive UI|
|🟡 3|13-16|Minggu 6-9|API integration|
|🟠 4|17-19|Minggu 9-12|Multi-page app|
|🔴 5|20-21|Minggu 12-15|Scalable state|
|⚫ 6|23-25|Minggu 15-19|Optimized app|
|🟡 7|26-27|Minggu 19-23|Tested + TypeScript|
|🟣 8|28-30|Minggu 23+|Advanced tools|

---

# ⚠️ React Best Practices

1. **ALWAYS lift state up when multiple components need same state**
2. **ALWAYS use keys in lists** — with stable, unique identifiers
3. **ALWAYS handle loading and error states**
4. **NEVER mutate state directly** — use setState/setters
5. **NEVER create new functions in render** — causes re-renders
6. **DON'T abuse useEffect** — it's not componentDidMount
7. **DO use custom hooks** for reusable logic
8. **DO separate components by concern** — not by route
9. **DO think in components** — not pages
10. **DO optimize for user experience** — not premature optimization

---

**Roadmap React v1.0 — Dari Hello World ke Production Application**

React adalah library untuk thinking in components, managing UI state, dan building fast, responsive applications. Learn these concepts deeply, practice building real projects, and master the modern React ecosystem.