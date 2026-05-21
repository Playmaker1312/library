# 📚 Kurikulum Komprehensif Perpustakaan Digital: JAVASCRIPT

## Panduan Penyusunan

Kurikulum ini dirancang dengan pendekatan **spiral learning** — setiap level membangun fondasi dari level sebelumnya. **Prasyarat:** Pemahaman dasar HTML & CSS sangat disarankan sebelum memulai. Estimasi waktu bersifat fleksibel tergantung kecepatan belajar individu.

---

## 🟢 LEVEL 1: FONDASI JAVASCRIPT (Minggu 1-3)

_"Memahami apa itu JavaScript dan cara kerjanya di dalam browser & runtime"_

### A. Pengantar JavaScript

[[1. Apa itu JavaScript dan sejarah singkatnya (Brendan Eich, ECMAScript)]]  
[[2. JavaScript vs Java — kesalahpahaman umum]]  
[[3. Bagaimana JavaScript dijalankan — Engine (V8, SpiderMonkey)]]  
[[4. JavaScript di Browser vs di Server (Node.js, Deno, Bun)]]  
[[5. ECMAScript — standar dan versinya (ES5, ES6 or ES2015, ES2020+)]]  
[[6. Cara menjalankan JavaScript — Browser Console, file .js, Node.js]]  
[[7. Menulis program Hello World pertama]]  
[[8. Setup Development Environment — VS Code, ekstensi penting]]  
[[9. Debugging dasar — console.log, console.table, console.error]]  
[[10. Penggunaan DevTools Browser (Chrome or Firefox)]]

### B. Sintaks Dasar & Variabel

[[11. Statement, Expression, dan Semicolon]]  
[[12. Komentar — single-line dan multi-line]]  
[[13. Deklarasi variabel — var, let, const]]  
[[14. Perbedaan var vs let vs const — scope dan hoisting]]  
[[15. Naming Conventions — camelCase, PascalCase, UPPER_CASE]]  
[[16. Reserved Keywords dalam JavaScript]]  
[[17. Strict Mode — use strict]]

### C. Tipe Data & Operator

[[18. Tipe data primitif — string, number, boolean, null, undefined]]  
[[19. Tipe data primitif baru — symbol dan bigint]]  
[[20. Tipe data referensi — object, array, function]]  
[[21. typeof operator — mengecek tipe data]]  
[[22. Type Coercion — implicit dan explicit conversion]]  
[[23. Operator Aritmatika]]  
[[24. Operator Assignment ]]  
[[25. Operator Perbandingan ]]  
[[26. Operator Logika]]  
[[27. Operator Ternary — kondisi]]  
[[28. Nullish Coalescing Operator]]  
[[29. Optional Chaining Operator ]]

---

## 🟡 LEVEL 2: KONTROL ALUR & STRUKTUR DATA (Minggu 4-6)

_"Mengontrol logika program dan bekerja dengan kumpulan data"_

### D. Control Flow

[[30. if statement — kondisi tunggal]]  
[[31. if-else dan else-if]]  
[[32. Nested if — kondisi bersarang]]  
[[33. switch-case statement]]  
[[34. Truthy dan Falsy values]]
[[35. Short-circuit evaluation dengan && dan]]
[[35. Short-circuit evaluation dengan && dan ||]]

### E. Looping / Perulangan

[[36. for loop — sintaks klasik]]  
[[37. while loop]]  
[[38. do-while loop]]  
[[39. for...of — iterasi nilai array]]  
[[40. for...in — iterasi key object]]  
[[41. break dan continue statement]]  
[[42. Nested loops — perulangan bersarang]]  
[[43. Infinite loop — bahaya dan cara menghindari]]

### F. Strings Mendalam

[[44. String literal — single quote, double quote, template literal]]  
[[45. Template Literals dan String Interpolation]]  
[[46. String methods — length, indexOf, includes, slice, substring]]  
[[47. String methods — toUpperCase, toLowerCase, trim, replace]]  
[[48. String methods — split, concat, repeat, padStart, padEnd]]  
[[49. String immutability — konsep penting]]  
[[50. Escape character]]

### G. Numbers & Math

[[51. Number methods — toFixed, toString, toPrecision]]  
[[52. parseInt dan parseFloat]]  
[[53. Number.isInteger, Number.isNaN, Number.isFinite]]  
[[54. NaN dan Infinity]]  
[[55. Math object — Math.round, Math.floor, Math.ceil, Math.random]]  
[[56. Math methods — Math.max, Math.min, Math.pow, Math.sqrt, Math.abs]]  
[[57. Floating point precision issues]]

### H. Arrays

[[58. Membuat array — literal dan constructor]]  
[[59. Mengakses elemen array — indexing dan length]]  
[[60. Array methods — push, pop, shift, unshift]]  
[[61. Array methods — slice vs splice]]  
[[62. Array methods — concat, join, reverse, sort]]  
[[63. Array methods — indexOf, includes, find, findIndex]]  
[[64. Higher-Order Array methods — forEach, map, filter, reduce]]  
[[65. Array methods — some, every, flat, flatMap]]  
[[66. Array destructuring]]  
[[67. Spread dan Rest operator pada array]]  
[[68. Array.from dan Array.of]]

### I. Objects

[[69. Membuat object — literal dan constructor]]  
[[70. Mengakses properti — dot notation vs bracket notation]]  
[[71. Menambah, mengubah, dan menghapus properti]]  
[[72. Object methods — Object.keys, Object.values, Object.entries]]  
[[73. Object methods — Object.assign, Object.freeze, Object.seal]]  
[[74. Object destructuring]]  
[[75. Spread operator pada object]]  
[[76. Shorthand property dan method]]  
[[77. Computed property names]]  
[[78. Nested objects — object di dalam object]]  
[[79. JSON — parse dan stringify]]

---

## 🟠 LEVEL 3: FUNGSI & SCOPE (Minggu 7-9)

_"Menguasai fungsi sebagai first-class citizen dalam JavaScript"_

### J. Functions Dasar

[[80. Function declaration vs Function expression]]  
[[81. Parameters vs Arguments]]  
[[82. Return statement]]  
[[83. Default parameters]]  
[[84. Rest parameters]]  
[[85. Arguments object — vs rest parameter]]  
[[86. Arrow Functions — sintaks dan use case]]  
[[87. Perbedaan Arrow Function vs Regular Function]]  
[[88. Immediately Invoked Function Expression (IIFE)]]

### K. Scope & Closure

[[89. Global Scope vs Local Scope]]  
[[90. Function Scope vs Block Scope]]  
[[91. Lexical Scope — konsep fundamental]]  
[[92. Hoisting — variable hoisting dan function hoisting]]  
[[93. Temporal Dead Zone (TDZ)]]  
[[94. Closure — apa itu dan cara kerjanya]]  
[[95. Closure use case — Data Privacy, Currying, Memoization]]  
[[96. Module pattern dengan closure]]

### L. Advanced Functions

[[97. Higher-Order Functions — fungsi yang menerima or mengembalikan fungsi]]  
[[98. Callback Functions — konsep dan penggunaan]]  
[[99. Pure Functions vs Impure Functions]]  
[[100. Recursion — fungsi rekursif]]  
[[101. Currying — function transformation]]  
[[102. Function Composition]]  
[[103. Memoization — caching hasil fungsi]]  
[[104. this keyword dalam berbagai konteks]]  
[[105. call, apply, dan bind methods]]

---

## 🔴 LEVEL 4: OOP & PROTOTYPE (Minggu 10-13)

_"Menguasai paradigma Object-Oriented dalam JavaScript"_

### M. Prototype & Inheritance

[[106. Prototype-based language — konsep dasar]]  
[[107. **proto** vs prototype]]  
[[108. Prototype Chain — bagaimana lookup bekerja]]  
[[109. Constructor Function — pola lama membuat object]]  
[[110. new keyword — apa yang terjadi di balik layar]]  
[[111. Object.create — membuat object dengan prototype tertentu]]  
[[112. hasOwnProperty dan in operator]]  
[[113. Inheritance dengan prototype]]

### N. Classes (ES6+)

[[114. Class declaration — sintaks modern]]  
[[115. Constructor method]]  
[[116. Instance methods dan properties]]  
[[117. Static methods dan properties]]  
[[118. Getters dan Setters]]  
[[119. Class Inheritance — extends dan super]]  
[[120. Method Overriding]]  
[[121. Private fields dengan # (ES2022)]]  
[[122. Public, Private, dan Protected (konvensi)]]  
[[123. instanceof operator]]  
[[124. Class vs Constructor Function — perbandingan]]

### O. Encapsulation & Design Patterns Dasar

[[125. Encapsulation — konsep dan implementasi]]  
[[126. Abstraction dalam JavaScript]]  
[[127. Polymorphism dalam JavaScript]]  
[[128. Composition over Inheritance]]  
[[129. Factory Pattern]]  
[[130. Singleton Pattern]]  
[[131. Observer Pattern dasar]]  
[[132. Module Pattern (ES6 Modules)]]

---

## 🟣 LEVEL 5: ASYNCHRONOUS JAVASCRIPT (Minggu 14-17)

_"Menguasai pemrograman asinkron — jantung JavaScript modern"_

### P. Event Loop & Asynchronous Concepts

[[133. Synchronous vs Asynchronous — konsep dasar]]  
[[134. Single-threaded nature of JavaScript]]  
[[135. Call Stack — bagaimana kode dieksekusi]]  
[[136. Web APIs — setTimeout, setInterval, fetch]]  
[[137. Callback Queue (Task Queue) dan Microtask Queue]]  
[[138. Event Loop — bagaimana semuanya bekerja sama]]  
[[139. Blocking vs Non-blocking code]]

### Q. Callbacks

[[140. Callback function — review mendalam]]  
[[141. Asynchronous callback — setTimeout, setInterval]]  
[[142. Callback Hell — masalah pyramid of doom]]  
[[143. Error-first callback pattern (Node.js style)]]

### R. Promises

[[144. Apa itu Promise dan mengapa diciptakan]]  
[[145. Membuat Promise — new Promise(resolve, reject)]]  
[[146. Promise states — pending, fulfilled, rejected]]  
[[147. Konsumsi Promise — .then, .catch, .finally]]  
[[148. Promise chaining]]  
[[149. Error handling pada Promise]]  
[[150. Promise.all — paralel execution]]  
[[151. Promise.allSettled]]  
[[152. Promise.race]]  
[[153. Promise.any]]  
[[154. Mengubah callback menjadi Promise (Promisification)]]

### S. Async/Await

[[155. async function — sintaks dan semantik]]  
[[156. await keyword — pause execution]]  
[[157. Error handling dengan try-catch]]  
[[158. Sequential vs Parallel execution dengan async/await]]  
[[159. Top-level await (ES2022)]]  
[[160. async/await vs Promise chain — kapan pakai yang mana]]

### T. Fetch API & HTTP Requests

[[161. XMLHttpRequest (XHR) — pendahulu fetch]]  
[[162. Fetch API — sintaks dasar]]  
[[163. Response object — json, text, blob]]  
[[164. Request configuration — method, headers, body]]  
[[165. Mengirim data — GET, POST, PUT, DELETE]]  
[[166. Handling errors di Fetch]]  
[[167. AbortController — membatalkan request]]  
[[168. CORS — Cross-Origin Resource Sharing]]  
[[169. Authentication — Bearer Token, API Keys]]  
[[170. Library alternatif — Axios overview]]

---

## ⚫ LEVEL 6: BROWSER & DOM MANIPULATION (Minggu 18-21)

_"Mengaplikasikan JavaScript untuk membangun UI interaktif"_

### U. DOM Fundamentals

[[171. Apa itu DOM (Document Object Model)]]  
[[172. DOM Tree — Node, Element, Document]]  
[[173. Mengakses elemen — getElementById, getElementsByClassName]]  
[[174. Mengakses elemen modern — querySelector, querySelectorAll]]  
[[175. NodeList vs HTMLCollection]]  
[[176. Traversing DOM — parentNode, children, siblings]]  
[[177. Membaca dan mengubah konten — textContent, innerHTML, innerText]]  
[[178. Mengubah atribut — getAttribute, setAttribute, removeAttribute]]  
[[179. Dataset API — data-* attributes]]  
[[180. Mengubah style — style property dan classList]]  
[[181. classList methods — add, remove, toggle, contains]]

### V. DOM Manipulation

[[182. Membuat elemen — createElement, createTextNode]]  
[[183. Menambahkan elemen — appendChild, append, prepend]]  
[[184. Menyisipkan elemen — insertBefore, insertAdjacentHTML]]  
[[185. Menghapus elemen — removeChild, remove]]  
[[186. Cloning elements — cloneNode]]  
[[187. Template element dan DocumentFragment]]  
[[188. Performance considerations dalam DOM manipulation]]

### W. Events

[[189. Event-driven programming concept]]  
[[190. addEventListener vs onclick attribute]]  
[[191. Event object — properties dan methods]]  
[[192. Event types — click, submit, change, input, keydown, keyup]]  
[[193. Mouse events — mouseenter, mouseleave, mousemove]]  
[[194. Form events — focus, blur, change, submit]]  
[[195. Keyboard events — keydown, keyup, keypress]]  
[[196. Event Bubbling dan Capturing]]  
[[197. Event Delegation — pola penting]]  
[[198. preventDefault dan stopPropagation]]  
[[199. Custom Events — CustomEvent constructor]]  
[[200. removeEventListener]]

### X. Form Handling & Validation

[[201. Form elements — input, select, textarea, checkbox, radio]]  
[[202. Mengakses dan mengubah nilai form]]  
[[203. Form submission handling]]  
[[204. FormData API]]  
[[205. Client-side validation]]  
[[206. HTML5 validation API]]  
[[207. Custom validation logic]]  
[[208. Real-time validation]]

### Y. Browser APIs

[[209. localStorage dan sessionStorage]]  
[[210. Cookies — set, get, delete]]  
[[211. IndexedDB — overview]]  
[[212. History API — pushState, replaceState]]  
[[213. Location object dan navigation]]  
[[214. Geolocation API]]  
[[215. Notification API]]  
[[216. Clipboard API]]  
[[217. File API dan FileReader]]  
[[218. Drag and Drop API]]  
[[219. Intersection Observer API]]  
[[220. Mutation Observer API]]  
[[221. Web Workers — multi-threading di browser]]

---

## 💎 LEVEL 7: JAVASCRIPT MODERN & TOOLING (Minggu 22-25)

_"Menguasai ekosistem JavaScript modern"_

### Z. Modules

[[222. Mengapa butuh modules]]  
[[223. CommonJS — require dan module.exports (Node.js)]]  
[[224. ES Modules — import dan export]]  
[[225. Default export vs Named export]]  
[[226. Dynamic import — import() function]]  
[[227. Module bundlers — overview]]  
[[228. Tree shaking — konsep dan manfaat]]

### AA. Error Handling

[[229. Error types — Error, TypeError, ReferenceError, SyntaxError]]  
[[230. try-catch-finally]]  
[[231. throw statement — melempar error custom]]  
[[232. Custom Error classes]]  
[[233. Error handling pada async code]]  
[[234. Global error handling — window.onerror, unhandledrejection]]  
[[235. Debugging strategy]]

### AB. Iterators & Generators

[[236. Iterable dan Iterator protocol]]  
[[237. Symbol.iterator]]  
[[238. Membuat custom iterator]]  
[[239. Generator functions — function* dan yield]]  
[[240. Generator use cases — lazy evaluation, infinite sequences]]  
[[241. Async Iterators dan for await...of]]

### AC. Advanced Concepts

[[242. Symbol — primitive baru dan use case]]  
[[243. Map dan WeakMap]]  
[[244. Set dan WeakSet]]  
[[245. Proxy — intercepting object operations]]  
[[246. Reflect API]]  
[[247. Tagged Template Literals]]  
[[248. Regular Expressions (RegEx) — sintaks dasar]]  
[[249. RegEx methods — test, exec, match, replace]]  
[[250. Regex flags — g, i, m, s, u, y]]  
[[251. Capturing groups dan lookahead/lookbehind]]

### AD. Tooling & Ecosystem

[[252. NPM — package manager dasar]]  
[[253. package.json — struktur dan field penting]]  
[[254. NPM scripts — automation]]  
[[255. Yarn dan PNPM — alternatif npm]]  
[[256. Node.js — runtime dan REPL]]  
[[257. Bundlers — Webpack overview]]  
[[258. Bundlers — Vite (modern dan cepat)]]  
[[259. Bundlers — esbuild dan Rollup]]  
[[260. Transpiler — Babel]]  
[[261. Linter — ESLint setup dan rules]]  
[[262. Formatter — Prettier]]  
[[263. Git hooks dengan Husky]]  
[[264. Environment variables — .env files]]

---

## 🌟 LEVEL 8: JAVASCRIPT DALAM PRAKTIK NYATA (Minggu 26-30)

_"Membangun aplikasi nyata dengan JavaScript"_

### AE. Functional Programming

[[265. Functional Programming paradigm — overview]]  
[[266. First-class functions]]  
[[267. Pure functions — review mendalam]]  
[[268. Immutability — konsep dan praktik]]  
[[269. Side effects — apa dan bagaimana menghindari]]  
[[270. Function composition — pipe dan compose]]  
[[271. Currying dan Partial Application]]  
[[272. Map, Filter, Reduce — paradigma fungsional]]  
[[273. Declarative vs Imperative programming]]

### AF. Design Patterns

[[274. Module Pattern]]  
[[275. Revealing Module Pattern]]  
[[276. Singleton Pattern]]  
[[277. Factory Pattern]]  
[[278. Constructor Pattern]]  
[[279. Observer Pattern]]  
[[280. Pub-Sub Pattern]]  
[[281. Mediator Pattern]]  
[[282. Decorator Pattern]]  
[[283. Strategy Pattern]]  
[[284. MVC, MVP, MVVM patterns]]

### AG. Testing

[[285. Mengapa testing penting]]  
[[286. Jenis testing — Unit, Integration, E2E]]  
[[287. TDD (Test-Driven Development) — konsep]]  
[[288. Jest — framework testing populer]]  
[[289. Vitest — alternatif modern]]  
[[290. Mocha + Chai]]  
[[291. Mocking dan Spying]]  
[[292. Code Coverage]]  
[[293. End-to-end testing — Cypress, Playwright]]

### AH. Performance & Optimization

[[294. Memory management dan Garbage Collection]]  
[[295. Memory leaks — penyebab dan deteksi]]  
[[296. Debouncing dan Throttling]]  
[[297. Lazy loading — gambar dan modul]]  
[[298. Code splitting]]  
[[299. Web Vitals — LCP, FID, CLS]]  
[[300. Performance API — measuring code execution]]  
[[301. Caching strategies]]  
[[302. requestAnimationFrame dan requestIdleCallback]]

### AI. Security

[[303. XSS (Cross-Site Scripting) — pencegahan]]  
[[304. CSRF (Cross-Site Request Forgery)]]  
[[305. Content Security Policy (CSP)]]  
[[306. Sanitizing user input]]  
[[307. Secure handling of sensitive data]]  
[[308. HTTPS dan Same-Origin Policy]]  
[[309. JWT — JSON Web Tokens]]

### AJ. Framework & Library Pengantar

[[310. Mengapa butuh framework]]  
[[311. React — overview dan filosofi]]  
[[312. Vue.js — overview dan filosofi]]  
[[313. Svelte — overview dan filosofi]]  
[[314. Angular — overview dan filosofi]]  
[[315. Memilih framework yang tepat]]  
[[316. Vanilla JS vs Framework — kapan pakai apa]]

---

## 🏆 LEVEL 9: MASTERY & SPESIALISASI (Bulan 7+)

_"Menjadi JavaScript expert"_

### AK. JavaScript Engine Internals

[[317. V8 Engine — bagaimana bekerja]]  
[[318. JIT Compilation — Ignition dan TurboFan]]  
[[319. Hidden Classes dan Inline Caching]]  
[[320. Memory Heap dan Stack]]  
[[321. Garbage Collection algorithms]]  
[[322. Performance optimization based on engine internals]]

### AL. Node.js Mendalam

[[323. Node.js architecture — libuv, V8, bindings]]  
[[324. Event Loop di Node.js — phases]]  
[[325. Built-in modules — fs, path, http, os, events]]  
[[326. Streams — Readable, Writable, Duplex, Transform]]  
[[327. Buffer dan Binary data]]  
[[328. Child Processes dan Cluster]]  
[[329. Worker Threads]]  
[[330. Building HTTP server dari scratch]]  
[[331. Express.js fundamentals]]

### AM. Advanced Async Patterns

[[332. Async iterators mendalam]]  
[[333. Observables — RxJS overview]]  
[[334. Reactive Programming]]  
[[335. Channels dan async coordination]]  
[[336. Backpressure handling]]

### AN. Web Platform Lanjutan

[[337. Service Workers — offline-first apps]]  
[[338. Progressive Web Apps (PWA)]]  
[[339. WebSockets — real-time communication]]  
[[340. WebRTC — peer-to-peer]]  
[[341. WebAssembly — interop dengan JS]]  
[[342. WebGL dan Canvas API]]  
[[343. Web Components — Custom Elements, Shadow DOM]]

### AO. Membuat & Menerbitkan Library

[[344. Project structure untuk library JavaScript]]  
[[345. Bundling untuk berbagai format — UMD, ESM, CJS]]  
[[346. Semantic Versioning (SemVer)]]  
[[347. Documentation dengan JSDoc]]  
[[348. Publishing ke NPM]]  
[[349. Maintaining open source library]]

### AP. Kontribusi & Komunitas

[[350. Membaca source code library populer]]  
[[351. Berkontribusi ke open source]]  
[[352. Mengikuti TC39 proposal]]  
[[353. Memahami spec ECMAScript]]  
[[354. Mentoring dan knowledge sharing]]

---

## 📋 CHECKLIST PROYEK PRAKTIK PER LEVEL

|Level|Proyek yang Disarankan|
|---|---|
|1|Kalkulator sederhana di console, program konversi suhu|
|2|To-do list di console, program manajemen kontak|
|3|Library utility functions (map, filter, reduce custom)|
|4|Sistem manajemen perpustakaan dengan OOP|
|5|Aplikasi cuaca dengan API, GitHub user finder|
|6|To-do list interaktif, Quiz app, Calculator UI|
|7|Markdown editor, Note-taking app dengan localStorage|
|8|E-commerce mini, Chat app dengan WebSocket|
|9|Membuat library/framework sendiri, kontribusi OSS|

---

## 💡 Tips Menggunakan Kurikulum Ini

- **Pastikan HTML & CSS kuat** — JavaScript bekerja erat dengan keduanya untuk web
- **Jangan loncat level** — setiap konsep membangun dari sebelumnya
- **Rasio belajar ideal:** 20% teori, 80% praktik langsung di Browser Console atau Node.js
- **Satu poin = satu sesi belajar** (30 menit - 2 jam tergantung kompleksitas)
- **Tulis kode setiap hari** — konsistensi lebih penting daripada durasi
- **Baca error message** — pesan error JavaScript sangat informatif
- **Eksperimen di DevTools** — Chrome DevTools adalah teman terbaik
- **Bangun proyek nyata** — teori tanpa praktik akan cepat hilang

---

## 📌 Tracking Progress

Gunakan checklist ini untuk melacak kemajuan Anda:

- [ ]  Level 1: Fondasi JavaScript (Poin 1-29)
- [ ]  Level 2: Kontrol Alur & Struktur Data (Poin 30-79)
- [ ]  Level 3: Fungsi & Scope (Poin 80-105)
- [ ]  Level 4: OOP & Prototype (Poin 106-132)
- [ ]  Level 5: Asynchronous JavaScript (Poin 133-170)
- [ ]  Level 6: Browser & DOM Manipulation (Poin 171-221)
- [ ]  Level 7: JavaScript Modern & Tooling (Poin 222-264)
- [ ]  Level 8: JavaScript dalam Praktik Nyata (Poin 265-316)
- [ ]  Level 9: Mastery & Spesialisasi (Poin 317-354)

---

## 📚 Sumber Belajar Rekomendasi

### Dokumentasi & Platform Official

- **MDN Web Docs** → developer.mozilla.org
- **JavaScript.info** → javascript.info (sangat komprehensif)
- **ECMAScript Specification** → tc39.es/ecma262

### Buku

- **Eloquent JavaScript** — Marijn Haverbeke (gratis online)
- **You Don't Know JS Yet** — Kyle Simpson (series)
- **JavaScript: The Good Parts** — Douglas Crockford
- **JavaScript: The Definitive Guide** — David Flanagan (O'Reilly)
- **Secrets of the JavaScript Ninja** — John Resig

### Video Course

- **JavaScript Course** → freeCodeCamp YouTube
- **The Complete JavaScript Course** → Udemy (Jonas Schmedtmann)
- **JavaScript: Understanding the Weird Parts** → Udemy (Anthony Alicea)
- **JavaScript Mastery** → YouTube
- **Web Programming UNPAS** → YouTube (Bahasa Indonesia)

### Practice

- **Codewars** → codewars.com
- **LeetCode** → leetcode.com (JavaScript track)
- **Exercism JavaScript Track** → exercism.org/tracks/javascript
- **JavaScript30** → javascript30.com (Wes Bos — 30 proyek)
- **Frontend Mentor** → frontendmentor.io

### Tools

- **Node.js** → nodejs.org
- **NPM** → npmjs.com
- **Can I Use** → caniuse.com (browser compatibility)
- **JS Bin / CodePen / JSFiddle** → online playgrounds

---

## 🎯 Target Pencapaian

|Waktu|Milestone|
|---|---|
|3 minggu|Memahami sintaks dasar dan bisa menulis program sederhana|
|6 minggu|Menguasai kontrol alur, array, dan object|
|9 minggu|Mahir dengan fungsi, scope, dan closure|
|13 minggu|Memahami OOP dan prototype-based inheritance|
|17 minggu|Menguasai async programming (Promise, async/await)|
|21 minggu|Bisa membangun aplikasi web interaktif dengan DOM|
|25 minggu|Memahami modules, tooling, dan ekosistem modern|
|30 minggu|Bisa membangun aplikasi production-ready|
|7+ bulan|JavaScript expert — siap belajar framework atau Node.js mendalam|

---

## 🔄 Peta Ketergantungan Konsep

text

```
Fondasi HTML & CSS
       │
       ▼
┌──────────────────┐
│  Sintaks Dasar   │
│  (variabel,      │
│   tipe data)     │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐     ┌──────────────────┐
│  Control Flow &  │────▶│  Functions &     │
│  Data Structure  │     │  Scope           │
└────────┬─────────┘     └────────┬─────────┘
         │                        │
         └──────────┬─────────────┘
                    │
                    ▼
           ┌────────────────┐
           │  OOP &         │
           │  Prototype     │
           └────────┬───────┘
                    │
         ┌──────────┴──────────┐
         ▼                     ▼
┌─────────────────┐   ┌─────────────────┐
│  Async JS       │   │  DOM &          │
│  (Promise,      │   │  Browser APIs   │
│   async/await)  │   │                 │
└────────┬────────┘   └────────┬────────┘
         │                     │
         └──────────┬──────────┘
                    │
                    ▼
         ┌─────────────────────┐
         │  Modern JS, Tooling │
         │  & Real-world Apps  │
         └──────────┬──────────┘
                    │
                    ▼
         ┌─────────────────────┐
         │  Mastery: Engine    │
         │  Internals, Node.js │
         │  & Web Platform     │
         └─────────────────────┘
```

---

**Catatan:** Kurikulum ini bersifat _living document_. Update dan sesuaikan dengan perkembangan JavaScript (versi ECMAScript baru, fitur baru) dan kebutuhan pribadi Anda.

**Versi ECMAScript yang dicakup:** ES5 — ES2024+

**Lisensi:** Silakan gunakan, modifikasi, dan bagikan untuk keperluan pembelajaran.