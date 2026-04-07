# 📚 Kurikulum Komprehensif Vue.js

## Dari Fundamental hingga Advanced

---

## 🟢 FASE 1: PRASYARAT (Pre-requisites)

Sebelum menyentuh Vue.js, pastikan fondasi ini kuat:

[[1. HTML5 Semantik & Aksesibilitas Dasar]]

[[2. CSS3 (Flexbox, Grid, Responsive Design)]]

[[3. JavaScript Modern (ES6+)]]

- [[3a. Let, Const, Arrow Functions]]
- [[3b. Template Literals]]
- [[3c. Destructuring (Object & Array)]]
- [[3d. Spread/Rest Operator]]
- [[3e. Array Methods (map, filter, reduce, find)]]
- [[3f. Promises & Async/Await]]
- [[3g. Modules (import/export)]]
- [[3h. Optional Chaining & Nullish Coalescing]]

[[4. Dasar Terminal/Command Line]]

[[5. Git & GitHub (Version Control dasar)]]

[[6. Node.js & npm/yarn (dasar package manager)]]

[[7. Konsep DOM (Document Object Model)]]

---

## 🟡 FASE 2: FUNDAMENTAL VUE.JS

### Modul 1 — Pengenalan & Setup

[[8. Apa itu Vue.js (Filosofi, Sejarah, Ekosistem)]]

[[9. Perbandingan Vue vs React vs Angular (kapan pilih Vue)]]

[[10. Vue 2 vs Vue 3 — Perbedaan Kunci]]

[[11. Cara Setup Project]]

- [[11a. Via CDN (untuk eksperimen cepat)]]
- [[11b. Via Vite (create-vue) — standar modern]]
- [[11c. Struktur folder project Vue]]

[[12. Vue DevTools (instalasi & penggunaan dasar)]]

### Modul 2 — Core Concepts (Options API)

[[13. Vue Instance & Lifecycle Dasar]]

[[14. Template Syntax]]

- [[14a. Interpolasi (Mustache {{ }})]]
- [[14b. Directive dasar (v-bind, v-on)]]
- [[14c. Shorthand (: dan @)]]

[[15. Data Binding]]

- [[15a. One-way binding (v-bind)]]
- [[15b. Two-way binding (v-model)]]

[[16. Computed Properties vs Methods]]

[[17. Watchers (watch)]]

[[18. Conditional Rendering]]

- [[18a. v-if / v-else-if / v-else]]
- [[18b. v-show]]
- [[18c. Kapan pakai v-if vs v-show]]

[[19. List Rendering]]

- [[19a. v-for dengan Array]]
- [[19b. v-for dengan Object]]
- [[19c. Pentingnya :key]]
- [[19d. v-for dengan range]]

[[20. Event Handling]]

- [[20a. Inline Handlers vs Method Handlers]]
- [[20b. Event Modifiers (.prevent, .stop, .once)]]
- [[20c. Key Modifiers (.enter, .tab, .esc)]]

[[21. Form Input Binding]]

- [[21a. Text, Textarea]]
- [[21b. Checkbox, Radio]]
- [[21c. Select / Dropdown]]
- [[21d. Modifiers (.lazy, .number, .trim)]]

[[22. Class & Style Binding]]

- [[22a. Object Syntax]]
- [[22b. Array Syntax]]
- [[22c. Binding inline styles]]

### Modul 3 — Component System (Fondasi)

[[23. Konsep Component-Based Architecture]]

[[24. Membuat & Mendaftarkan Component]]

- [[24a. Global vs Local Registration]]
- [[24b. Single File Component (SFC) — .vue file]]
- [[24c. Struktur template, script, style]]

[[25. Props (Parent → Child)]]

- [[25a. Deklarasi & Tipe Data Props]]
- [[25b. Prop Validation (type, required, default)]]
- [[25c. One-way Data Flow]]

[[26. Events / Emit (Child → Parent)]]

- [[26a. $emit]]
- [[26b. Defining Emits]]
- [[26c. Event dengan payload/data]]

[[27. Slots]]

- [[27a. Default Slot]]
- [[27b. Named Slots]]
- [[27c. Scoped Slots]]

[[28. Component Lifecycle Hooks]]

- [[28a. beforeCreate → created]]
- [[28b. beforeMount → mounted]]
- [[28c. beforeUpdate → updated]]
- [[28d. beforeUnmount → unmounted]]
- [[28e. Diagram Lifecycle (visual)]]

---

## 🟠 FASE 3: INTERMEDIATE VUE.JS

### Modul 4 — Composition API

[[29. Mengapa Composition API? (Motivasi & Keuntungan)]]

[[30. setup() function]]

[[31. ref() vs reactive()]]

- [[31a. Kapan pakai ref vs reactive]]
- [[31b. toRef() & toRefs()]]

[[32. computed() dalam Composition API]]

[[33. watch() & watchEffect()]]

- [[33a. Perbedaan watch vs watchEffect]]
- [[33b. Deep watch & Immediate watch]]

[[34. Lifecycle Hooks di Composition API]]

- [[34a. onMounted, onUpdated, onUnmounted, dll]]

[[35. Script Setup (<script setup>) — Syntactic Sugar]]

[[36. Composables (Custom Hooks)]]

- [[36a. Konsep & Pola]]
- [[36b. Membuat useCounter, useFetch, useLocalStorage]]
- [[36c. Best practices penamaan & struktur]]

[[37. provide() / inject() (Dependency Injection)]]

[[38. Template Refs (ref pada elemen DOM)]]

### Modul 5 — Vue Router

[[39. Instalasi & Konfigurasi Dasar]]

[[40. Definisi Routes (path, component, name)]]

[[41. RouterLink & RouterView]]

[[42. Dynamic Routes (/:id)]]

[[43. Route Params vs Query Params]]

[[44. Nested Routes (Children Routes)]]

[[45. Programmatic Navigation (router.push, router.replace)]]

[[46. Named Routes & Named Views]]

[[47. Navigation Guards]]

- [[47a. Global Guards (beforeEach, afterEach)]]
- [[47b. Per-Route Guards (beforeEnter)]]
- [[47c. In-Component Guards]]

[[48. Route Meta Fields]]

[[49. Lazy Loading Routes (Dynamic Import)]]

[[50. Redirect & Alias]]

[[51. 404 / Catch-All Route]]

[[52. Scroll Behavior]]

[[53. Route Transitions (animasi antar halaman)]]

### Modul 6 — State Management (Pinia)

[[54. Mengapa perlu State Management?]]

[[55. Pinia vs Vuex (dan mengapa Pinia jadi standar)]]

[[56. Instalasi & Setup Pinia]]

[[57. Mendefinisikan Store]]

- [[57a. Option Store]]
- [[57b. Setup Store (Composition API style)]]

[[58. State, Getters, Actions]]

[[59. Mengakses Store di Component]]

[[60. Store-to-Store Interaction]]

[[61. Pinia Plugins]]

[[62. Persist State (pinia-plugin-persistedstate)]]

[[63. Debugging Pinia dengan Vue DevTools]]

### Modul 7 — HTTP & Komunikasi API

[[64. Fetch API vs Axios]]

[[65. Instalasi & Konfigurasi Axios]]

[[66. CRUD Operations (GET, POST, PUT, DELETE)]]

[[67. Axios Instance & Base URL]]

[[68. Interceptors (Request & Response)]]

[[69. Error Handling & Loading State]]

[[70. Environment Variables (.env) untuk API URL]]

[[71. Composable: useFetch / useApi pattern]]

[[72. Integrasi API dengan Pinia]]

---

## 🔴 FASE 4: UPPER-INTERMEDIATE

### Modul 8 — Komponen Lanjutan

[[73. Dynamic Components (<component :is="">)]]

[[74. Async Components (defineAsyncComponent)]]

[[75. Teleport (<Teleport to="">)]]

[[76. Transition & TransitionGroup (Animasi)]]

- [[76a. CSS Transitions]]
- [[76b. JavaScript Hooks untuk animasi]]
- [[76c. List Transitions]]

[[77. KeepAlive (<KeepAlive>)]]

[[78. Renderless Components Pattern]]

[[79. Compound Components Pattern]]

[[80. v-model pada Custom Component (defineModel)]]

[[81. Attribute Inheritance (Fallthrough Attributes)]]

[[82. $attrs & useAttrs()]]

### Modul 9 — Forms & Validasi

[[83. Form Handling Patterns di Vue]]

[[84. VeeValidate]]

- [[84a. Instalasi & Setup]]
- [[84b. Validasi dengan Yup/Zod Schema]]
- [[84c. Field-level vs Form-level Validation]]
- [[84d. Custom Validation Rules]]

[[85. FormKit (alternatif)]]

[[86. Multi-step Form Wizard]]

[[87. File Upload Handling]]

### Modul 10 — Styling & UI Framework

[[88. Scoped Styles & CSS Modules di Vue SFC]]

[[89. Deep Selector (:deep())]]

[[90. v-bind() di CSS (Dynamic Styling)]]

[[91. Tailwind CSS + Vue Integration]]

[[92. UI Libraries]]

- [[92a. Vuetify (Material Design)]]
- [[92b. PrimeVue]]
- [[92c. Naive UI]]
- [[92d. Headless UI]]

[[93. Icon Libraries (Iconify, Lucide)]]

[[94. Dark Mode Implementation]]

### Modul 11 — Tooling & DX (Developer Experience)

[[95. Vite — Deep Dive (config, plugins)]]

[[96. TypeScript + Vue]]

- [[96a. Typing Props & Emits]]
- [[96b. Typing ref, reactive, computed]]
- [[96c. Typing Component Template Refs]]
- [[96d. Generic Components]]

[[97. ESLint + Prettier untuk Vue]]

[[98. Vitest (Unit Testing)]]

- [[98a. Testing Components]]
- [[98b. Testing Composables]]
- [[98c. Testing Pinia Stores]]

[[99. Vue Test Utils]]

[[100. Cypress / Playwright (E2E Testing)]]

[[101. Storybook untuk Vue Components]]

---

## ⚫ FASE 5: ADVANCED

### Modul 12 — Patterns & Arsitektur

[[102. Project Structure Best Practices (scalable)]]

[[103. Feature-based vs Layer-based Architecture]]

[[104. Composable Design Patterns]]

- [[104a. State Composable]]
- [[104b. Action Composable]]
- [[104c. Resource Composable]]

[[105. Dependency Injection Pattern (provide/inject advanced)]]

[[106. Plugin System (membuat Vue Plugin sendiri)]]

[[107. Custom Directives (v-focus, v-click-outside, dll)]]

[[108. Mixins (legacy) → kenapa migrasi ke Composables]]

[[109. Error Boundaries & Global Error Handling]]

[[110. Design System dengan Vue]]

### Modul 13 — Performance Optimization

[[111. Virtual DOM & Reactivity Deep Dive]]

[[112. Vue Reactivity System Under the Hood (Proxy, track, trigger)]]

[[113. Component Rendering Optimization]]

- [[113a. v-once, v-memo]]
- [[113b. shallowRef, shallowReactive]]
- [[113c. markRaw]]

[[114. Lazy Loading (Components & Routes)]]

[[115. Code Splitting Strategies]]

[[116. Tree Shaking]]

[[117. List Virtualization (vue-virtual-scroller)]]

[[118. Debounce & Throttle pada Watchers/Events]]

[[119. Image Optimization (Lazy load images)]]

[[120. Bundle Analysis (rollup-plugin-visualizer)]]

[[121. Lighthouse Audit & Web Vitals]]

### Modul 14 — Nuxt.js (Meta-Framework)

[[122. Apa itu Nuxt 3 & Mengapa Perlu]]

[[123. CSR vs SSR vs SSG vs ISR (Rendering Modes)]]

[[124. File-based Routing di Nuxt]]

[[125. Data Fetching (useFetch, useAsyncData)]]

[[126. Server API Routes (/server/api)]]

[[127. Middleware di Nuxt]]

[[128. SEO & Meta Tags (useHead, useSeoMeta)]]

[[129. Nuxt Modules Ecosystem]]

[[130. Deployment Nuxt (Vercel, Netlify, Node server)]]

[[131. Nuxt Content (Markdown-driven CMS)]]

[[132. Nuxt Layers & Extends]]

### Modul 15 — Topik-Topik Lanjutan Spesifik

[[133. Authentication & Authorization]]

- [[133a. JWT Flow]]
- [[133b. OAuth / Social Login]]
- [[133c. Route Guards untuk Auth]]
- [[133d. Refresh Token Strategy]]

[[134. Internationalization (i18n) — vue-i18n]]

[[135. WebSocket & Real-time (Socket.io + Vue)]]

[[136. PWA (Progressive Web App) dengan Vue]]

[[137. GraphQL + Vue (Apollo Client / URQL)]]

[[138. Micro-Frontend Architecture dengan Vue]]

[[139. Web Workers di Vue]]

[[140. Server-Sent Events (SSE)]]

### Modul 16 — Ekosistem & Libraries Penting

[[141. VueUse (Collection of Composables) — deep dive]]

[[142. Pinia ORM]]

[[143. Vue Query (TanStack Query for Vue)]]

[[144. Chart Libraries (Chart.js, ECharts + Vue)]]

[[145. Table Libraries (TanStack Table)]]

[[146. Drag & Drop (VueDraggable, dnd-kit)]]

[[147. PDF Generation]]

[[148. Rich Text Editor (Tiptap, Quill)]]

[[149. Date Handling (Day.js, date-fns)]]

[[150. Toast/Notification Libraries]]

### Modul 17 — Deployment & DevOps

[[151. Build & Preview (vite build)]]

[[152. Environment-based Configuration]]

[[153. Deployment Platforms]]

- [[153a. Vercel]]
- [[153b. Netlify]]
- [[153c. Firebase Hosting]]
- [[153d. Docker + Nginx]]
- [[153e. Traditional VPS]]

[[154. CI/CD Pipeline (GitHub Actions)]]

[[155. Monitoring & Error Tracking (Sentry)]]

[[156. Analytics Integration]]

---

## 🏆 FASE 6: PROYEK PORTFOLIO

[[157. 🟢 Beginner: Todo App (CRUD + LocalStorage)]]

[[158. 🟢 Beginner: Weather App (API integration)]]

[[159. 🟡 Intermediate: Blog Platform (Router + Pinia + API)]]

[[160. 🟡 Intermediate: E-commerce Product Catalog (Filter, Cart)]]

[[161. 🔴 Advanced: Full-stack Dashboard (Auth + Charts + RBAC)]]

[[162. 🔴 Advanced: Real-time Chat App (WebSocket + Auth)]]

[[163. ⚫ Expert: SaaS App dengan Nuxt 3 (SSR + Auth + Payment)]]

---

## 📋 REKOMENDASI ALUR BELAJAR

|Periode|Fase|Nomor Materi|
|---|---|---|
|Minggu 1-2|Fase 1 (Prasyarat)|No. 1-7|
|Minggu 3-4|Fase 2 (Fundamental)|No. 8-28|
|Minggu 5-7|Fase 3 (Intermediate)|No. 29-72|
|Minggu 8-10|Fase 4 (Upper-Intermediate)|No. 73-101|
|Minggu 11-14|Fase 5 (Advanced)|No. 102-156|
|Minggu 15-16|Fase 6 (Portfolio Projects)|No. 157-163|

---

> **💡 Tips:** Setiap selesai 1 modul, bangun **mini-project** kecil untuk mengaplikasikan ilmu tersebut. Jangan lanjut ke modul berikutnya sebelum benar-benar memahami modul sebelumnya. **Belajar ≠ menonton/membaca, belajar = membangun sesuatu.**
## ⚫ FASE 5: ADVANCED

### Modul 12 — Patterns & Arsitektur

text

```
102. Project Structure Best Practices (scalable)
103. Feature-based vs Layer-based Architecture
104. Composable Design Patterns:
     104a. State Composable
     104b. Action Composable
     104c. Resource Composable
105. Dependency Injection Pattern (provide/inject advanced)
106. Plugin System (membuat Vue Plugin sendiri)
107. Custom Directives (v-focus, v-click-outside, dll)
108. Mixins (legacy) → kenapa migrasi ke Composables
109. Error Boundaries & Global Error Handling
110. Design System dengan Vue
```

### Modul 13 — Performance Optimization

text

```
111. Virtual DOM & Reactivity Deep Dive
112. Vue Reactivity System Under the Hood
     (Proxy, track, trigger)
113. Component Rendering Optimization:
     113a. v-once, v-memo
     113b. shallowRef, shallowReactive
     113c. markRaw
114. Lazy Loading (Components & Routes)
115. Code Splitting Strategies
116. Tree Shaking
117. List Virtualization (vue-virtual-scroller)
118. Debounce & Throttle pada Watchers/Events
119. Image Optimization (Lazy load images)
120. Bundle Analysis (rollup-plugin-visualizer)
121. Lighthouse Audit & Web Vitals
```

### Modul 14 — Nuxt.js (Meta-Framework)

text

```
122. Apa itu Nuxt 3 & Mengapa Perlu
123. CSR vs SSR vs SSG vs ISR (Rendering Modes)
124. File-based Routing di Nuxt
125. Data Fetching (useFetch, useAsyncData)
126. Server API Routes (/server/api)
127. Middleware di Nuxt
128. SEO & Meta Tags (useHead, useSeoMeta)
129. Nuxt Modules Ecosystem
130. Deployment Nuxt (Vercel, Netlify, Node server)
131. Nuxt Content (Markdown-driven CMS)
132. Nuxt Layers & Extends
```

### Modul 15 — Topik-Topik Lanjutan Spesifik

text

```
133. Authentication & Authorization:
     133a. JWT Flow
     133b. OAuth / Social Login
     133c. Route Guards untuk Auth
     133d. Refresh Token Strategy
134. Internationalization (i18n) — vue-i18n
135. WebSocket & Real-time (Socket.io + Vue)
136. PWA (Progressive Web App) dengan Vue
137. GraphQL + Vue (Apollo Client / URQL)
138. Micro-Frontend Architecture dengan Vue
139. Web Workers di Vue
140. Server-Sent Events (SSE)
```

### Modul 16 — Ekosistem & Libraries Penting

text

```
141. VueUse (Collection of Composables) — deep dive
142. Pinia ORM
143. Vue Query (TanStack Query for Vue)
144. Chart Libraries (Chart.js, ECharts + Vue)
145. Table Libraries (TanStack Table)
146. Drag & Drop (VueDraggable, dnd-kit)
147. PDF Generation
148. Rich Text Editor (Tiptap, Quill)
149. Date Handling (Day.js, date-fns)
150. Toast/Notification Libraries
```

### Modul 17 — Deployment & DevOps

text

```
151. Build & Preview (vite build)
152. Environment-based Configuration
153. Deployment Platforms:
     153a. Vercel
     153b. Netlify
     153c. Firebase Hosting
     153d. Docker + Nginx
     153e. Traditional VPS
154. CI/CD Pipeline (GitHub Actions)
155. Monitoring & Error Tracking (Sentry)
156. Analytics Integration
```

---

## 🏆 FASE 6: PROYEK PORTFOLIO

text

```
157. 🟢 Beginner  : Todo App (CRUD + LocalStorage)
158. 🟢 Beginner  : Weather App (API integration)
159. 🟡 Intermediate: Blog Platform (Router + Pinia + API)
160. 🟡 Intermediate: E-commerce Product Catalog (Filter, Cart)
161. 🔴 Advanced  : Full-stack Dashboard (Auth + Charts + RBAC)
162. 🔴 Advanced  : Real-time Chat App (WebSocket + Auth)
163. ⚫ Expert    : SaaS App dengan Nuxt 3 (SSR + Auth + Payment)
```

---

## 📋 REKOMENDASI ALUR BELAJAR

text

```
Minggu 1-2   → Fase 1 (Prasyarat)          → No. 1-7
Minggu 3-4   → Fase 2 (Fundamental)        → No. 8-28
Minggu 5-7   → Fase 3 (Intermediate)       → No. 29-72
Minggu 8-10  → Fase 4 (Upper-Intermediate) → No. 73-101
Minggu 11-14 → Fase 5 (Advanced)           → No. 102-156
Minggu 15-16 → Fase 6 (Portfolio Projects)  → No. 157-163
```

> **💡 Tips:** Setiap selesai 1 modul, bangun **mini-project** kecil untuk  
> mengaplikasikan ilmu tersebut. Jangan lanjut ke modul berikutnya sebelum  
> benar-benar memahami modul sebelumnya. **Belajar ≠ menonton/membaca,  
> belajar = membangun sesuatu.**