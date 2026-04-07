# 📚 Kurikulum Komprehensif Perpustakaan Digital: TYPESCRIPT

## Panduan Penyusunan

> Kurikulum ini dirancang dengan pendekatan **spiral learning** — setiap level membangun fondasi dari level sebelumnya. **Prasyarat**: Pemahaman dasar JavaScript (ES6+) sangat disarankan sebelum memulai. Estimasi waktu bersifat fleksibel tergantung kecepatan belajar individu.

---

## 🟢 LEVEL 1: FONDASI TYPESCRIPT (Minggu 1-3)

_"Memahami mengapa TypeScript ada dan cara kerjanya"_

### A. Pengantar TypeScript

1. [[1. Apa itu TypeScript dan mengapa diciptakan]]
2. [[2. TypeScript vs JavaScript — perbedaan fundamental]]
3. [[3. Static Typing vs Dynamic Typing — konsep dan keuntungan]]
4. [[4. Cara kerja TypeScript Compiler (tsc) — dari .ts ke .js]]
5. [[5. Instalasi TypeScript (npm global vs lokal, npx)]]
6. [[6. Konfigurasi tsconfig.json dasar — opsi penting pertama]]
7. [[7. Menulis dan menjalankan program TypeScript pertama]]
8. [[8. TypeScript Playground — eksperimen online]]
9. [[9. Integrasi TypeScript dengan VS Code (extensions, settings, IntelliSense)]]

### B. Tipe Data Primitif & Dasar

10. [[10. Type Annotation — sintaks dasar (variabel colon tipe)]]
11. [[11. Tipe primitif string, number, boolean]]
12. [[12. Tipe khusus null dan undefined — strictNullChecks]]
13. [[13. Tipe any — kapan boleh dan tidak boleh digunakan]]
14. [[14. Tipe unknown — alternatif aman untuk any]]
15. [[15. Tipe void — untuk fungsi tanpa return]]
16. [[16. Tipe never — fungsi yang tidak pernah return]]
17. [[17. Type Inference — bagaimana TypeScript menebak tipe otomatis]]
18. [[18. Literal Types (string literal, number literal, boolean literal)]]
19. [[19. Type Assertion — as keyword dan angle-bracket syntax]]

### C. Array, Tuple, dan Enum

20. [[20. Array Types — dua sintaks (string[] dan Array<number>)]]
21. [[21. Readonly Arrays — immutable array]]
22. [[22. Tuple Types — array dengan panjang dan tipe tetap]]
23. [[23. Tuple dengan optional dan rest elements]]
24. [[24. Enum — Numeric Enum]]
25. [[25. Enum — String Enum]]
26. [[26. Const Enum vs Regular Enum — perbedaan dan use case]]
27. [[27. Enum alternatives — kapan tidak menggunakan enum]]

---

## 🟡 LEVEL 2: FUNGSI & OBJECT TYPES (Minggu 4-6)

_"Menguasai cara mengetik fungsi dan struktur data object"_

### D. Functions dalam TypeScript

28. [[28. Mengetik parameter dan return type]]
29. [[29. Optional Parameters — param question mark type]]
30. [[30. Default Parameters dengan type inference]]
31. [[31. Rest Parameters — spread operator dengan tipe]]
32. [[32. Function Type Expressions — mengetik variabel fungsi]]
33. [[33. Call Signatures — function type dalam object type]]
34. [[34. Void vs Never sebagai return type — perbedaan penting]]
35. [[35. Typing callback functions]]
36. [[36. Function Overloading — multiple signatures]]
37. [[37. Typing this dalam functions — ThisParameterType]]

### E. Object Types & Interfaces

38. [[38. Object Type Annotation — inline object types]]
39. [[39. Interface — deklarasi dan penggunaan dasar]]
40. [[40. Optional Properties — properti opsional dalam interface]]
41. [[41. Readonly Properties — properti yang tidak bisa diubah]]
42. [[42. Index Signatures — dynamic property keys]]
43. [[43. Extending Interfaces — inheritance antar interface]]
44. [[44. Interface Merging — Declaration Merging]]
45. [[45. Interface untuk Function Types]]
46. [[46. Hybrid Types — object yang juga callable]]
47. [[47. Excess Property Checking — strict object literals]]

### F. Type Aliases & Komposisi Tipe

48. [[48. Type Alias — deklarasi dengan keyword type]]
49. [[49. Interface vs Type Alias — perbandingan dan kapan pakai yang mana]]
50. [[50. Union Types — tipe A atau B (A pipe B)]]
51. [[51. Intersection Types — tipe A dan B (A ampersand B)]]
52. [[52. Literal Types dalam Union — membuat enum-like types]]
53. [[53. Discriminated Unions — Tagged Unions pattern]]
54. [[54. Type Narrowing — konsep fundamental]]
55. [[55. Type Guards dengan typeof]]
56. [[56. Type Guards dengan instanceof]]
57. [[57. Type Guards dengan in operator]]
58. [[58. Equality Narrowing dan Truthiness Narrowing]]
59. [[59. Nullable Types dan Non-null Assertion Operator (!)]]
60. [[60. Optional Chaining (?.) dan Nullish Coalescing (??)]]

---

## 🟠 LEVEL 3: OOP & GENERICS DASAR (Minggu 7-10)

_"Menguasai paradigma OOP dan konsep Generics"_

### G. Classes dalam TypeScript

61. [[61. Class declaration dengan type annotations]]
62. [[62. Access Modifiers — public, private, protected]]
63. [[63. Readonly modifier dalam class properties]]
64. [[64. Constructor dan Parameter Properties shorthand]]
65. [[65. Getter dan Setter dengan tipe]]
66. [[66. Class Inheritance — extends keyword]]
67. [[67. Implementing Interfaces — implements keyword]]
68. [[68. Abstract Classes dan Abstract Methods]]
69. [[69. Static Properties dan Static Methods]]
70. [[70. Class sebagai Type — structural typing]]
71. [[71. Override keyword (TS 4.3+)]]
72. [[72. Class Expressions]]
73. [[73. this type dalam class methods]]

### H. Generics — Fondasi

74. [[74. Mengapa butuh Generics — masalah yang dipecahkan]]
75. [[75. Generic Functions — sintaks angle bracket T]]
76. [[76. Generic Interfaces]]
77. [[77. Generic Classes]]
78. [[78. Generic Type Aliases]]
79. [[79. Multiple Type Parameters — T, U, V]]
80. [[80. Generic Constraints — extends keyword untuk membatasi]]
81. [[81. Generic dengan keyof operator]]
82. [[82. Default Type Parameters]]
83. [[83. Generic Higher-Order Functions]]
84. [[84. Contoh praktis Generic Repository Pattern]]

### I. User-Defined Type Guards & Assertions

85. [[85. User-Defined Type Guards — is keyword]]
86. [[86. Assertion Functions — asserts keyword]]
87. [[87. Const Assertions — as const mendalam]]
88. [[88. Satisfies Operator (TS 4.9+) — validasi tanpa widening]]
89. [[89. Type Predicates — pola lanjutan]]

---

## 🔴 LEVEL 4: TIPE LANJUTAN & UTILITY TYPES (Minggu 11-14)

_"Menguasai manipulasi tipe tingkat lanjut"_

### J. Utility Types Bawaan

90. [[90. Partial T — semua properti jadi optional]]
91. [[91. Required T — semua properti jadi required]]
92. [[92. Readonly T — semua properti jadi readonly]]
93. [[93. Record K T — membuat object type dari key dan value]]
94. [[94. Pick T K — mengambil sebagian properti]]
95. [[95. Omit T K — membuang sebagian properti]]
96. [[96. Exclude T U — membuang tipe dari union]]
97. [[97. Extract T U — mengambil tipe dari union]]
98. [[98. NonNullable T — membuang null dan undefined]]
99. [[99. ReturnType T — mendapatkan return type fungsi]]
100. [[100. Parameters T — mendapatkan tuple parameter types]]
101. [[101. ConstructorParameters T dan InstanceType T]]
102. [[102. Awaited T — unwrap Promise type]]
103. [[103. Intrinsic String Manipulation Types — Uppercase, Lowercase, Capitalize, Uncapitalize]]
104. [[104. Kombinasi Utility Types dalam praktik nyata]]

### K. Advanced Type Manipulation

105. [[105. Mapped Types — sintaks bracket K in keyof T]]
106. [[106. Mapped Type Modifiers — plus minus readonly optional]]
107. [[107. Key Remapping dalam Mapped Types — as keyword]]
108. [[108. Conditional Types — T extends U question X colon Y]]
109. [[109. Inferring within Conditional Types — infer keyword]]
110. [[110. Distributive Conditional Types]]
111. [[111. Template Literal Types — dasar]]
112. [[112. Template Literal Types — kombinasi dengan Mapped Types]]
113. [[113. Indexed Access Types — T bracket K]]
114. [[114. keyof operator — mendalam]]
115. [[115. typeof operator di type-level]]
116. [[116. Recursive Type Aliases]]
117. [[117. Variadic Tuple Types]]

### L. Advanced Patterns & Teknik

118. [[118. Branded Types atau Opaque Types]]
119. [[119. Phantom Types]]
120. [[120. Type-safe Builder Pattern]]
121. [[121. Exhaustive Checking dengan never]]
122. [[122. Covariance dan Contravariance — in out modifiers]]
123. [[123. Declaration Merging — pola lanjutan]]
124. [[124. Mixin Pattern dengan TypeScript]]
125. [[125. Fluent Interface Pattern]]

---

## 🟣 LEVEL 5: MODULES, DECORATORS & TOOLING (Minggu 15-18)

_"Menguasai ekosistem dan konfigurasi TypeScript"_

### M. Modules & Namespaces

126. [[126. ES Modules dalam TypeScript — import export]]
127. [[127. Type-Only Imports dan Exports — import type]]
128. [[128. Module Resolution Strategies — node, classic, bundler]]
129. [[129. Path Mapping — paths dan baseUrl di tsconfig]]
130. [[130. Declaration Files (.d.ts) — apa dan mengapa]]
131. [[131. Menulis Declaration Files sendiri]]
132. [[132. DefinitelyTyped dan @types packages]]
133. [[133. Ambient Declarations — declare keyword]]
134. [[134. Module Augmentation]]
135. [[135. Global Augmentation]]
136. [[136. Namespaces — dan mengapa jarang dipakai sekarang]]

### N. Decorators

137. [[137. Apa itu Decorators dan sejarahnya di TypeScript]]
138. [[138. Mengaktifkan experimentalDecorators]]
139. [[139. Class Decorators]]
140. [[140. Method Decorators]]
141. [[141. Property Decorators]]
142. [[142. Parameter Decorators]]
143. [[143. Accessor Decorators]]
144. [[144. Decorator Factories — decorator dengan argumen]]
145. [[145. Decorator Composition — urutan eksekusi]]
146. [[146. Stage 3 Decorators (TC39 — TS 5.0+)]]
147. [[147. Perbandingan Legacy vs Stage 3 Decorators]]
148. [[148. Use case Decorators — Logging, Validation, Dependency Injection]]

### O. tsconfig.json Mendalam

149. [[149. Compiler Options kategori Type Checking — strict, strictNullChecks, noImplicitAny]]
150. [[150. Compiler Options kategori Module — module, moduleResolution, esModuleInterop]]
151. [[151. Compiler Options kategori Emit — outDir, rootDir, declaration, sourceMap]]
152. [[152. Compiler Options kategori JavaScript Support — allowJs, checkJs]]
153. [[153. Compiler Options kategori Interop Constraints]]
154. [[154. Compiler Options kategori Language dan Environment — target, lib, jsx]]
155. [[155. Strict Mode — apa saja yang diaktifkan]]
156. [[156. Project References dan Composite Projects]]
157. [[157. Incremental Compilation]]
158. [[158. Extending tsconfig — extends property]]
159. [[159. Include, Exclude, dan Files options]]

### P. Tooling & Developer Experience

160. [[160. TypeScript dengan ESLint — @typescript-eslint]]
161. [[161. Prettier + TypeScript]]
162. [[162. ts-node dan tsx — menjalankan TypeScript langsung]]
163. [[163. TypeScript dengan Webpack — ts-loader dan fork-ts-checker]]
164. [[164. TypeScript dengan Vite]]
165. [[165. TypeScript dengan esbuild]]
166. [[166. TypeScript dengan SWC]]
167. [[167. Monorepo dengan TypeScript — Turborepo, Nx]]
168. [[168. Type Checking di CI CD Pipeline]]
169. [[169. Performance — mempercepat kompilasi TypeScript]]

---

## ⚫ LEVEL 6: TYPESCRIPT DALAM PRAKTIK NYATA (Minggu 19-24)

_"Mengaplikasikan TypeScript di berbagai domain"_

### Q. TypeScript + React

170. [[170. Setup project React + TypeScript — Vite, CRA, Next.js]]
171. [[171. Typing Functional Components — React.FC vs explicit]]
172. [[172. Typing Props dan Children]]
173. [[173. Typing State dengan useState generic]]
174. [[174. Typing Events — onClick, onChange, onSubmit]]
175. [[175. Typing Refs dengan useRef generic]]
176. [[176. Typing useReducer — action dan state]]
177. [[177. Typing Custom Hooks]]
178. [[178. Typing Context API — createContext generic]]
179. [[179. Generic Components]]
180. [[180. Polymorphic Components — as prop pattern]]
181. [[181. Higher-Order Components (HOC) dengan TypeScript]]
182. [[182. Render Props Pattern + TypeScript]]
183. [[183. React.ComponentProps dan React.HTMLAttributes]]
184. [[184. Typing Third-party Libraries di React]]

### R. TypeScript + Node.js / Backend

185. [[185. Setup Node.js + TypeScript]]
186. [[186. Typing Express.js — Request, Response, Middleware]]
187. [[187. Typing REST API Responses]]
188. [[188. TypeScript + Fastify]]
189. [[189. TypeScript + NestJS — framework berbasis decorator]]
190. [[190. Typing Database dengan TypeORM]]
191. [[191. Typing Database dengan Prisma — type-safe ORM]]
192. [[192. Typing Database dengan Drizzle ORM]]
193. [[193. Typing Environment Variables — type-safe env]]
194. [[194. Error Handling Patterns di Backend TypeScript]]
195. [[195. Typing WebSocket connections]]
196. [[196. Typing GraphQL — TypeGraphQL dan codegen]]

### S. TypeScript + Testing

197. [[197. TypeScript + Jest — setup dan konfigurasi]]
198. [[198. TypeScript + Vitest]]
199. [[199. Typing Test Suites dan Assertions]]
200. [[200. Typing Mocks dan Spies]]
201. [[201. Type-safe Test Fixtures dan Factories]]
202. [[202. Testing Type Definitions — tsd, expect-type]]
203. [[203. TypeScript + Cypress atau Playwright]]

### T. Membuat & Menerbitkan Library TypeScript

204. [[204. Project structure untuk library TypeScript]]
205. [[205. Menulis source code yang type-safe]]
206. [[206. Generating declaration files (.d.ts)]]
207. [[207. Dual package (CJS + ESM) dengan TypeScript]]
208. [[208. Package.json — types, typings, exports field]]
209. [[209. Dokumentasi dengan TSDoc]]
210. [[210. Publishing ke npm]]
211. [[211. Semantic Versioning untuk type changes]]
212. [[212. Testing library di project konsumer]]

---

## 💎 LEVEL 7: MASTERY & SPESIALISASI (Bulan 6+)

_"Menjadi TypeScript expert"_

### U. Type-Level Programming

213. [[213. TypeScript sebagai bahasa fungsional di level tipe]]
214. [[214. Arithmetika di level tipe — Tuple-based counting]]
215. [[215. String parsing di level tipe]]
216. [[216. Recursive Type challenges]]
217. [[217. Type-level State Machines]]
218. [[218. Higher-Kinded Types (HKT) — simulasi di TypeScript]]
219. [[219. Type Challenges dari type-challenges repository]]
220. [[220. Studi kasus — Bagaimana Prisma membangun type safety]]
221. [[221. Studi kasus — Bagaimana Zod membangun type safety]]

### V. Design Patterns dengan TypeScript

222. [[222. Creational Patterns — Singleton dengan TypeScript]]
223. [[223. Creational Patterns — Factory Method dengan TypeScript]]
224. [[224. Creational Patterns — Abstract Factory dengan TypeScript]]
225. [[225. Creational Patterns — Builder dengan TypeScript]]
226. [[226. Structural Patterns — Adapter dengan TypeScript]]
227. [[227. Structural Patterns — Decorator (OOP) dengan TypeScript]]
228. [[228. Structural Patterns — Facade dengan TypeScript]]
229. [[229. Structural Patterns — Proxy dengan TypeScript]]
230. [[230. Behavioral Patterns — Observer dengan TypeScript]]
231. [[231. Behavioral Patterns — Strategy dengan TypeScript]]
232. [[232. Behavioral Patterns — Command dengan TypeScript]]
233. [[233. Behavioral Patterns — State dengan TypeScript]]
234. [[234. Functional Patterns — Result atau Either monad]]
235. [[235. Functional Patterns — Pipe dan Compose]]
236. [[236. Dependency Injection tanpa framework]]

### W. Performa & Optimisasi Type System

237. [[237. Memahami TypeScript Performance Wiki]]
238. [[238. Menghindari complex type yang lambat]]
239. [[239. Interface vs Type — dampak performa]]
240. [[240. Profiling TypeScript compiler — diagnostics, extendedDiagnostics]]
241. [[241. Tracing type resolution — generateTrace]]
242. [[242. Tips mengurangi type instantiation]]
243. [[243. Tips menghindari deep recursive types]]
244. [[244. Optimizing large codebases]]
245. [[245. Project References untuk monorepo besar]]

### X. Migrasi & Strategi Adopsi

246. [[246. Strategi migrasi JavaScript ke TypeScript]]
247. [[247. Gradual Adoption — allowJs + checkJs]]
248. [[248. Migrating large codebases — tahapan]]
249. [[249. Mengatasi any debt]]
250. [[250. Strict mode migration — bertahap]]
251. [[251. Automating migration — ts-migrate, codemods]]
252. [[252. Convincing your team — argumen dan data]]
253. [[253. TypeScript governance dalam tim besar]]

---

## 📋 CHECKLIST PROYEK PRAKTIK PER LEVEL

|Level|Proyek yang Disarankan|
|---|---|
|**1**|Kalkulator CLI dengan type safety, program tebak angka typed|
|**2**|To-do list CLI dengan interfaces dan type guards|
|**3**|Library utility functions dengan generics|
|**4**|Custom utility types dan type-safe form validator|
|**5**|NPM package dengan declaration files|
|**6**|Fullstack app (React + Express) fully typed|
|**7**|Kontribusi ke DefinitelyTyped atau library open source|

---

## 💡 Tips Menggunakan Kurikulum Ini

1. **Pastikan JavaScript kuat** — TypeScript adalah superset, fondasi JS wajib
2. **Jangan loncat level** — setiap konsep membangun dari sebelumnya
3. **Rasio belajar ideal**: 30% teori, 70% praktik langsung di TypeScript Playground atau VS Code
4. **Satu poin = satu sesi belajar** (30 menit - 2 jam tergantung kompleksitas)
5. **Aktifkan strict mode** sejak awal untuk membangun kebiasaan baik
6. **Baca error message** — TypeScript error sangat informatif, pelajari membacanya
7. **Eksperimen di Playground** — typescriptlang.org/play sangat berguna

---

## 📌 Tracking Progress

Gunakan checklist ini untuk melacak kemajuan Anda:

- [ ]  Level 1: Fondasi TypeScript (Poin 1-27)
- [ ]  Level 2: Fungsi & Object Types (Poin 28-60)
- [ ]  Level 3: OOP & Generics Dasar (Poin 61-89)
- [ ]  Level 4: Tipe Lanjutan & Utility Types (Poin 90-125)
- [ ]  Level 5: Modules, Decorators & Tooling (Poin 126-169)
- [ ]  Level 6: TypeScript dalam Praktik Nyata (Poin 170-212)
- [ ]  Level 7: Mastery & Spesialisasi (Poin 213-253)

---

## 📚 Sumber Belajar Rekomendasi

### Dokumentasi & Platform Official

- **TypeScript Handbook** → typescriptlang.org/docs/handbook
- **TypeScript Playground** → typescriptlang.org/play
- **TypeScript Deep Dive** → basarat.gitbook.io/typescript

### Buku

- _Programming TypeScript_ — Boris Cherny (O'Reilly)
- _Effective TypeScript_ — Dan Vanderkam
- _Learning TypeScript_ — Josh Goldberg (O'Reilly)
- _TypeScript Cookbook_ — Stefan Baumgartner

### Video Course

- **Total TypeScript** → totaltypescript.com (Matt Pocock)
- **TypeScript Course** → Udemy (Maximilian Schwarzmüller)
- **No BS TS** → YouTube (Jack Herrington)

### Practice

- **Type Challenges** → github.com/type-challenges/type-challenges
- **TypeScript Exercises** → typescript-exercises.github.io
- **Exercism TypeScript Track** → exercism.org/tracks/typescript

### Tools

- **DefinitelyTyped** → github.com/DefinitelyTyped/DefinitelyTyped
- **TypeScript ESLint** → typescript-eslint.io
- **ts-reset** → github.com/total-typescript/ts-reset

---

## 🎯 Target Pencapaian

|Waktu|Milestone|
|---|---|
|3 minggu|Memahami basic types dan bisa menulis fungsi typed|
|6 minggu|Menguasai interfaces, type aliases, dan union types|
|10 minggu|Mahir menggunakan generics dan utility types|
|14 minggu|Memahami advanced type manipulation|
|18 minggu|Menguasai tooling dan konfigurasi|
|24 minggu|Bisa membangun aplikasi fullstack dengan TypeScript|
|6+ bulan|TypeScript expert — bisa menulis library dan kontribusi OSS|

---

## 🔄 Peta Ketergantungan Konsep

text

```
Fondasi JS (ES6+)
       │
       ▼
┌──────────────────┐
│  Basic Types     │
│  (primitif,      │
│   array, tuple)  │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐     ┌──────────────────┐
│  Functions       │────▶│  Interfaces &    │
│  Typing          │     │  Type Aliases    │
└────────┬─────────┘     └────────┬─────────┘
         │                        │
         └──────────┬─────────────┘
                    │
                    ▼
           ┌────────────────┐
           │    Generics    │
           └────────┬───────┘
                    │
         ┌──────────┴──────────┐
         ▼                     ▼
┌─────────────────┐   ┌─────────────────┐
│  Utility Types  │   │  Mapped &       │
│                 │   │  Conditional    │
└────────┬────────┘   └────────┬────────┘
         │                     │
         └──────────┬──────────┘
                    │
                    ▼
         ┌─────────────────────┐
         │  Advanced Patterns  │
         │  & Type-Level       │
         │  Programming        │
         └─────────────────────┘
```

---

**Catatan**: Kurikulum ini bersifat living document. Update dan sesuaikan dengan perkembangan TypeScript (versi baru, fitur baru) dan kebutuhan pribadi Anda.

**Versi TypeScript yang dicakup**: 4.x — 5.x

**Lisensi**: Silakan gunakan, modifikasi, dan bagikan untuk keperluan pembelajaran.