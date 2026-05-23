# 📚 Perpustakaan Digital: Kurikulum Node.js Komprehensif

### Panduan Belajar dari Fundamental hingga Advanced

---

## 🟢 LEVEL 1: ABSOLUTE BEGINNER (Pemula Mutlak)

### 1.1 Pengenalan Node.js

[[1. Apa itu Node.js dan sejarah perkembangannya sejak diciptakan Ryan Dahl tahun 2009]]  
[[2. Memahami Node.js sebagai JavaScript runtime environment berbasis V8 engine]]  
[[3. Perbedaan antara JavaScript di browser dan JavaScript di Node.js]]  
[[4. Memahami arsitektur event-driven dan non-blocking I/O dalam Node.js]]  
[[5. Memahami konsep single-threaded event loop dalam Node.js]]  
[[6. Keunggulan dan kelemahan Node.js dibandingkan platform server-side lainnya]]  
[[7. Kasus penggunaan Node.js yang paling tepat yaitu real-time apps, API, dan microservices]]  
[[8. Ekosistem Node.js yaitu npm, yarn, pnpm, dan berbagai framework populer]]  
[[9. Memahami perbedaan Node.js LTS dan Current version]]  
[[10. Prospek karir Node.js developer di industri saat ini]]

---

### 1.2 Instalasi dan Konfigurasi Lingkungan

[[11. Cara menginstal Node.js menggunakan installer resmi dari nodejs.org]]  
[[12. Cara menginstal Node.js menggunakan Node Version Manager atau nvm di macOS dan Linux]]  
[[13. Cara menginstal Node.js menggunakan nvm-windows di Windows]]  
[[14. Cara menggunakan nvm untuk berpindah antar versi Node.js]]  
[[15. Cara memverifikasi instalasi menggunakan perintah node --version dan npm --version]]  
[[16. Cara menjalankan JavaScript menggunakan Node.js REPL di terminal]]  
[[17. Cara menjalankan file JavaScript menggunakan perintah node nama-file.js]]  
[[18. Cara menginstal dan mengkonfigurasi VS Code untuk pengembangan Node.js]]  
[[19. Cara menginstal ekstensi VS Code yang berguna untuk Node.js development]]  
[[20. Memahami cara kerja Node.js REPL dan perintah-perintah dasarnya]]

---

### 1.3 JavaScript Modern yang Dibutuhkan untuk Node.js

[[21. Memahami konsep var, let, dan const serta perbedaan scope-nya]]  
[[22. Memahami arrow function dan penggunaannya]]  
[[23. Memahami template literals atau template strings]]  
[[24. Memahami destructuring assignment pada object dan array]]  
[[25. Memahami spread operator dan rest parameter]]  
[[26. Memahami default parameter dalam fungsi]]  
[[27. Memahami shorthand property dan method dalam object literal]]  
[[28. Memahami Promise dan cara kerjanya untuk asynchronous programming]]  
[[29. Memahami async dan await sebagai cara modern menulis asynchronous code]]  
[[30. Memahami JavaScript modules yaitu import dan export]]  
[[31. Memahami class syntax dalam JavaScript modern]]  
[[32. Memahami optional chaining operator yaitu tanda tanya titik]]  
[[33. Memahami nullish coalescing operator yaitu tanda tanya ganda]]  
[[34. Memahami Array methods penting yaitu map, filter, reduce, find, dan some]]  
[[35. Memahami Object methods yaitu Object.keys, Object.values, dan Object.entries]]

---

### 1.4 Modul Sistem Node.js

[[36. Memahami dua sistem modul dalam Node.js yaitu CommonJS dan ES Modules]]  
[[37. Cara menggunakan CommonJS yaitu require dan module.exports]]  
[[38. Cara menggunakan ES Modules yaitu import dan export dalam Node.js]]  
[[39. Perbedaan antara CommonJS dan ES Modules dalam Node.js]]  
[[40. Cara mengkonfigurasi project untuk menggunakan ES Modules menggunakan type module di package.json]]  
[[41. Memahami module resolution yaitu bagaimana Node.js mencari modul]]  
[[42. Memahami built-in modules yaitu modul bawaan yang tersedia tanpa instalasi]]  
[[43. Cara menggunakan modul path untuk manipulasi path file]]  
[[44. Cara menggunakan modul os untuk informasi sistem operasi]]  
[[45. Cara menggunakan modul util untuk utility functions]]  
[[46. Cara menggunakan modul url untuk parsing dan formatting URL]]  
[[47. Cara menggunakan modul querystring untuk parsing query string]]  
[[48. Cara menggunakan modul crypto untuk operasi kriptografi dasar]]  
[[49. Cara menggunakan modul events untuk event-driven programming]]  
[[50. Cara menggunakan modul stream untuk menangani data streaming]]

---

### 1.5 File System Module

[[51. Cara menggunakan modul fs untuk operasi file system]]  
[[52. Cara membaca file secara asynchronous menggunakan fs.readFile]]  
[[53. Cara membaca file secara synchronous menggunakan fs.readFileSync]]  
[[54. Cara menulis file secara asynchronous menggunakan fs.writeFile]]  
[[55. Cara menulis file secara synchronous menggunakan fs.writeFileSync]]  
[[56. Cara menambahkan konten ke file menggunakan fs.appendFile]]  
[[57. Cara menghapus file menggunakan fs.unlink]]  
[[58. Cara membuat direktori menggunakan fs.mkdir]]  
[[59. Cara membaca isi direktori menggunakan fs.readdir]]  
[[60. Cara memeriksa keberadaan file menggunakan fs.access atau fs.existsSync]]  
[[61. Cara mendapatkan informasi file menggunakan fs.stat]]  
[[62. Cara menggunakan fs.promises untuk operasi file dengan Promise]]  
[[63. Cara menggunakan modul fs slash promises dengan async await]]  
[[64. Cara memantau perubahan file menggunakan fs.watch]]  
[[65. Cara menggunakan modul path bersama fs untuk operasi path yang aman]]

---

## 🔵 LEVEL 2: ELEMENTARY (Dasar Lanjutan)

### 2.1 NPM dan Package Management

[[66. Memahami apa itu npm yaitu Node Package Manager dan kegunaannya]]  
[[67. Cara menginisialisasi project baru menggunakan perintah npm init]]  
[[68. Cara menggunakan npm init dengan flag -y untuk konfigurasi default]]  
[[69. Memahami file package.json dan setiap field di dalamnya]]  
[[70. Cara menginstal package menggunakan npm install nama-package]]  
[[71. Perbedaan antara dependencies dan devDependencies]]  
[[72. Cara menginstal package sebagai devDependency menggunakan flag --save-dev]]  
[[73. Cara menginstal package secara global menggunakan flag -g]]  
[[74. Memahami file package-lock.json dan kegunaannya]]  
[[75. Cara mengupdate package menggunakan npm update]]  
[[76. Cara menghapus package menggunakan npm uninstall]]  
[[77. Cara menjalankan script yang didefinisikan di package.json menggunakan npm run]]  
[[78. Cara melihat daftar package yang terinstal menggunakan npm list]]  
[[79. Cara mencari package menggunakan npm search]]  
[[80. Memahami semantic versioning yaitu major, minor, dan patch]]  
[[81. Memahami version range symbols yaitu caret dan tilde dalam package.json]]  
[[82. Cara menggunakan npx untuk menjalankan package tanpa menginstal global]]  
[[83. Pengenalan yarn sebagai alternatif npm]]  
[[84. Pengenalan pnpm sebagai package manager yang lebih efisien]]  
[[85. Cara mempublish package sendiri ke npm registry]]

---

### 2.2 Asynchronous Programming dalam Node.js

[[86. Memahami mengapa Node.js menggunakan model asynchronous]]  
[[87. Memahami callback pattern dan cara kerjanya]]  
[[88. Memahami masalah callback hell atau pyramid of doom]]  
[[89. Memahami cara kerja event loop Node.js secara mendalam]]  
[[90. Memahami call stack, callback queue, dan microtask queue]]  
[[91. Perbedaan antara macrotask dan microtask dalam event loop]]  
[[92. Memahami dan menggunakan Promise untuk asynchronous code]]  
[[93. Cara menggunakan Promise.then dan Promise.catch dan Promise.finally]]  
[[94. Cara membuat Promise sendiri menggunakan konstruktor Promise]]  
[[95. Cara menggunakan Promise.all untuk menjalankan banyak Promise secara paralel]]  
[[96. Cara menggunakan Promise.allSettled untuk menangani semua Promise terlepas hasilnya]]  
[[97. Cara menggunakan Promise.race untuk mendapatkan Promise yang selesai pertama]]  
[[98. Cara menggunakan Promise.any untuk mendapatkan Promise yang berhasil pertama]]  
[[99. Cara menggunakan async dan await untuk menulis asynchronous code yang lebih bersih]]  
[[100. Cara menangani error dalam async await menggunakan try catch]]

---

### 2.3 Event Emitter

[[101. Memahami konsep EventEmitter dalam Node.js]]  
[[102. Cara menggunakan class EventEmitter dari modul events]]  
[[103. Cara mendaftarkan listener menggunakan metode on]]  
[[104. Cara mendaftarkan listener yang hanya berjalan sekali menggunakan metode once]]  
[[105. Cara memancarkan event menggunakan metode emit]]  
[[106. Cara menghapus listener menggunakan metode off atau removeListener]]  
[[107. Cara menghapus semua listener menggunakan metode removeAllListeners]]  
[[108. Cara menggunakan metode listenerCount untuk menghitung listener]]  
[[109. Cara membuat class yang mewarisi EventEmitter]]  
[[110. Memahami event error yang harus selalu ditangani dalam EventEmitter]]

---

### 2.4 HTTP Module Dasar

[[111. Cara membuat HTTP server sederhana menggunakan modul http bawaan Node.js]]  
[[112. Memahami objek request dan response dalam HTTP server Node.js]]  
[[113. Cara membaca URL dan method dari request]]  
[[114. Cara membaca request headers]]  
[[115. Cara membaca request body dari POST request]]  
[[116. Cara mengirim response dengan status code]]  
[[117. Cara mengirim response headers]]  
[[118. Cara mengirim response body]]  
[[119. Cara membuat routing sederhana menggunakan if-else berdasarkan URL]]  
[[120. Cara membuat server HTTPS menggunakan modul https]]  
[[121. Cara melakukan HTTP request ke server lain menggunakan modul http]]  
[[122. Cara menggunakan modul https untuk request ke endpoint HTTPS]]  
[[123. Memahami konsep port dan cara server mendengarkan pada port tertentu]]  
[[124. Cara menangani error dalam HTTP server]]  
[[125. Memahami keterbatasan HTTP module bawaan dan mengapa framework seperti Express diperlukan]]

---

### 2.5 Debugging Node.js

[[126. Cara menggunakan console.log, console.error, dan console.warn untuk debugging dasar]]  
[[127. Cara menggunakan console.table untuk menampilkan data tabular]]  
[[128. Cara menggunakan console.time dan console.timeEnd untuk mengukur waktu eksekusi]]  
[[129. Cara menggunakan Node.js built-in debugger dengan flag --inspect]]  
[[130. Cara menggunakan Chrome DevTools untuk debugging Node.js]]  
[[131. Cara menggunakan VS Code debugger untuk Node.js]]  
[[132. Cara mengkonfigurasi launch.json di VS Code untuk Node.js debugging]]  
[[133. Cara menggunakan breakpoints dalam VS Code debugger]]  
[[134. Cara menggunakan watch expressions dalam debugger]]  
[[135. Memahami stack trace dan cara membacanya untuk menemukan sumber error]]

---

## 🟡 LEVEL 3: PRE-INTERMEDIATE

### 3.1 Express.js Dasar

[[136. Apa itu Express.js dan mengapa menjadi framework Node.js paling populer]]  
[[137. Cara menginstal Express.js menggunakan npm]]  
[[138. Cara membuat aplikasi Express.js dasar]]  
[[139. Cara mendefinisikan route GET menggunakan app.get]]  
[[140. Cara mendefinisikan route POST, PUT, PATCH, dan DELETE]]  
[[141. Cara menggunakan app.use untuk middleware global]]  
[[142. Cara menggunakan objek request yaitu req.params, req.query, req.body, dan req.headers]]  
[[143. Cara menggunakan objek response yaitu res.send, res.json, res.status, dan res.redirect]]  
[[144. Cara menggunakan Express Router untuk memisahkan route ke file terpisah]]  
[[145. Cara menggunakan middleware express.json untuk parsing request body JSON]]  
[[146. Cara menggunakan middleware express.urlencoded untuk parsing form data]]  
[[147. Cara menggunakan middleware express.static untuk melayani file statis]]  
[[148. Cara menjalankan Express server dan mendengarkan pada port tertentu]]  
[[149. Cara menggunakan route parameters dalam Express]]  
[[150. Cara menggunakan query string dalam Express]]

---

### 3.2 Middleware dalam Express

[[151. Memahami konsep middleware dan cara kerjanya dalam Express]]  
[[152. Memahami urutan eksekusi middleware dalam Express pipeline]]  
[[153. Cara membuat middleware kustom]]  
[[154. Cara menggunakan next function untuk meneruskan ke middleware berikutnya]]  
[[155. Cara menggunakan next dengan argument error untuk error handling]]  
[[156. Cara menggunakan middleware untuk logging request]]  
[[157. Cara menggunakan package morgan sebagai HTTP request logger]]  
[[158. Cara menggunakan middleware cors untuk menangani Cross-Origin Resource Sharing]]  
[[159. Cara menggunakan middleware helmet untuk security headers]]  
[[160. Cara menggunakan middleware compression untuk mengompresi response]]  
[[161. Cara mendefinisikan error handling middleware dengan empat parameter]]  
[[162. Cara menggunakan middleware untuk autentikasi]]  
[[163. Cara menggunakan middleware untuk validasi input]]  
[[164. Cara menggunakan middleware rate limiting menggunakan express-rate-limit]]  
[[165. Cara mengorganisasi middleware dalam project Express yang lebih besar]]

---

### 3.3 Koneksi Database

[[166. Pengenalan database yang sering digunakan bersama Node.js yaitu MongoDB, PostgreSQL, dan MySQL]]  
[[167. Cara menginstal dan menggunakan Mongoose untuk koneksi MongoDB]]  
[[168. Cara membuat koneksi MongoDB menggunakan Mongoose]]  
[[169. Cara mendefinisikan Mongoose Schema dan Model]]  
[[170. Cara melakukan operasi CRUD menggunakan Mongoose]]  
[[171. Cara menggunakan Mongoose validation pada schema]]  
[[172. Cara menggunakan Mongoose middleware yaitu pre dan post hooks]]  
[[173. Cara menginstal dan menggunakan pg library untuk koneksi PostgreSQL]]  
[[174. Cara membuat koneksi PostgreSQL menggunakan Pool]]  
[[175. Cara menjalankan query PostgreSQL menggunakan Node.js]]  
[[176. Cara menginstal dan menggunakan mysql2 library untuk koneksi MySQL]]  
[[177. Cara membuat koneksi MySQL menggunakan connection pool]]  
[[178. Cara menjalankan query MySQL menggunakan Node.js]]  
[[179. Memahami perbedaan antara SQL dan NoSQL database dalam konteks Node.js]]  
[[180. Cara menggunakan environment variables untuk menyimpan kredensial database]]

---

### 3.4 Environment Variables dan Konfigurasi

[[181. Memahami pentingnya environment variables dalam aplikasi Node.js]]  
[[182. Cara mengakses environment variables menggunakan process.env]]  
[[183. Cara menggunakan package dotenv untuk memuat file .env]]  
[[184. Cara membuat dan mengorganisasi file .env]]  
[[185. Cara menambahkan .env ke .gitignore untuk keamanan]]  
[[186. Cara membuat file .env.example sebagai template]]  
[[187. Cara menggunakan environment variables yang berbeda untuk development dan production]]  
[[188. Cara mengkonfigurasi NODE_ENV untuk membedakan environment]]  
[[189. Cara membuat modul konfigurasi terpusat dalam project Node.js]]  
[[190. Cara menggunakan package config atau convict untuk konfigurasi yang lebih terstruktur]]

---

### 3.5 Error Handling dalam Express

[[191. Memahami pentingnya error handling yang baik dalam aplikasi Node.js]]  
[[192. Cara menangani synchronous error dalam Express route handler]]  
[[193. Cara menangani asynchronous error menggunakan try catch dalam async handler]]  
[[194. Cara menggunakan wrapper function untuk menghindari try catch berulang]]  
[[195. Cara membuat class custom error yang memperluas class Error]]  
[[196. Cara membuat error handling middleware terpusat di Express]]  
[[197. Cara membedakan operational error dan programming error]]  
[[198. Cara mengirimkan response error yang konsisten dalam format JSON]]  
[[199. Cara menangani 404 not found menggunakan middleware catch-all]]  
[[200. Cara menggunakan process.on untuk menangani uncaughtException dan unhandledRejection]]

---

## 🟠 LEVEL 4: INTERMEDIATE

### 4.1 RESTful API dengan Express

[[201. Memahami prinsip-prinsip RESTful API secara mendalam]]  
[[202. Cara merencanakan struktur endpoint RESTful API yang baik]]  
[[203. Cara mengimplementasikan CRUD endpoint yang lengkap dengan Express]]  
[[204. Cara mengimplementasikan pagination pada API response]]  
[[205. Cara mengimplementasikan filtering dan sorting pada API endpoint]]  
[[206. Cara mengimplementasikan field selection atau sparse fieldsets]]  
[[207. Cara mengimplementasikan search functionality dalam API]]  
[[208. Cara membuat response format yang konsisten yaitu data, message, dan status]]  
[[209. Cara menggunakan HTTP status code yang tepat untuk setiap situasi]]  
[[210. Cara mengimplementasikan HATEOAS dalam RESTful API]]  
[[211. Cara membuat API versioning menggunakan URL path atau header]]  
[[212. Cara mendokumentasikan API menggunakan Swagger atau OpenAPI dengan swagger-jsdoc]]  
[[213. Cara menggunakan swagger-ui-express untuk tampilan dokumentasi interaktif]]  
[[214. Cara mengimplementasikan content negotiation dalam API]]  
[[215. Cara menguji API menggunakan Postman atau Thunder Client]]

---

### 4.2 Authentication dan Authorization

[[216. Memahami konsep authentication dan authorization dalam aplikasi Node.js]]  
[[217. Cara mengimplementasikan JWT yaitu JSON Web Token untuk authentication]]  
[[218. Cara menginstal dan menggunakan package jsonwebtoken]]  
[[219. Cara membuat dan menandatangani JWT menggunakan jwt.sign]]  
[[220. Cara memverifikasi JWT menggunakan jwt.verify]]  
[[221. Cara menyimpan JWT di client yaitu localStorage vs httpOnly cookie]]  
[[222. Cara mengimplementasikan access token dan refresh token strategy]]  
[[223. Cara membuat middleware autentikasi untuk melindungi route]]  
[[224. Cara mengimplementasikan password hashing menggunakan bcrypt]]  
[[225. Cara menginstal dan menggunakan package bcryptjs]]  
[[226. Cara melakukan hash password menggunakan bcrypt.hash]]  
[[227. Cara memverifikasi password menggunakan bcrypt.compare]]  
[[228. Cara mengimplementasikan sistem login dan register yang aman]]  
[[229. Cara mengimplementasikan role-based access control atau RBAC]]  
[[230. Cara mengimplementasikan OAuth2 menggunakan Passport.js]]  
[[231. Cara menggunakan passport-google-oauth20 untuk Google login]]  
[[232. Cara menggunakan passport-github2 untuk GitHub login]]  
[[233. Cara mengimplementasikan session-based authentication menggunakan express-session]]  
[[234. Cara mengimplementasikan two-factor authentication atau 2FA]]  
[[235. Cara mengimplementasikan email verification flow]]

---

### 4.3 Validasi Input

[[236. Memahami pentingnya validasi input di server-side]]  
[[237. Cara menggunakan package Joi untuk validasi schema]]  
[[238. Cara mendefinisikan schema validasi menggunakan Joi]]  
[[239. Cara membuat middleware validasi menggunakan Joi]]  
[[240. Cara menggunakan package Yup sebagai alternatif Joi]]  
[[241. Cara menggunakan package express-validator untuk validasi berbasis middleware]]  
[[242. Cara menggunakan check dan validationResult dari express-validator]]  
[[243. Cara membuat validasi kustom menggunakan custom method pada Joi]]  
[[244. Cara melakukan sanitasi input untuk mencegah XSS]]  
[[245. Cara mengimplementasikan validasi yang reusable di berbagai route]]

---

### 4.4 Mongoose Lanjutan

[[246. Cara mendefinisikan relasi antar schema menggunakan populate]]  
[[247. Cara menggunakan metode populate untuk join data antar collection]]  
[[248. Cara menggunakan virtual fields dalam Mongoose schema]]  
[[249. Cara menggunakan Mongoose static methods dan instance methods]]  
[[250. Cara menggunakan Mongoose query helpers]]  
[[251. Cara mengimplementasikan pagination menggunakan Mongoose]]  
[[252. Cara menggunakan Mongoose aggregation pipeline]]  
[[253. Cara menggunakan Mongoose text search]]  
[[254. Cara menggunakan index dalam Mongoose untuk optimasi query]]  
[[255. Cara menggunakan Mongoose transactions]]  
[[256. Cara mengimplementasikan soft delete dalam Mongoose]]  
[[257. Cara menggunakan Mongoose discriminators untuk inheritance]]  
[[258. Cara menggunakan metode lean untuk query yang lebih cepat]]  
[[259. Cara menggunakan Mongoose plugin untuk reuse functionality]]  
[[260. Cara menggunakan mongoose-paginate-v2 plugin untuk pagination]]

---

### 4.5 Testing dalam Node.js

[[261. Memahami jenis-jenis testing yaitu unit, integration, dan end-to-end]]  
[[262. Cara menginstal dan menggunakan Jest sebagai testing framework]]  
[[263. Cara menulis test case dasar menggunakan describe dan it atau test]]  
[[264. Cara menggunakan expect dan berbagai matcher dalam Jest]]  
[[265. Cara menggunakan beforeEach, afterEach, beforeAll, dan afterAll]]  
[[266. Cara membuat mock function menggunakan jest.fn]]  
[[267. Cara menggunakan jest.mock untuk mocking module]]  
[[268. Cara menggunakan jest.spyOn untuk memantau pemanggilan fungsi]]  
[[269. Cara menguji asynchronous code dalam Jest menggunakan async await]]  
[[270. Cara menggunakan Supertest untuk menguji HTTP endpoints Express]]  
[[271. Cara menginstal dan mengkonfigurasi Supertest bersama Jest]]  
[[272. Cara menulis integration test untuk CRUD endpoint]]  
[[273. Cara menggunakan database in-memory seperti mongodb-memory-server untuk testing]]  
[[274. Cara mengukur code coverage dalam Jest menggunakan flag --coverage]]  
[[275. Cara menggunakan Mocha dan Chai sebagai alternatif testing framework]]

---

## 🔴 LEVEL 5: UPPER-INTERMEDIATE

### 5.1 ORM dan Query Builder

[[276. Memahami konsep ORM dan kegunaannya dalam Node.js]]  
[[277. Pengenalan Prisma sebagai ORM modern untuk Node.js dan TypeScript]]  
[[278. Cara menginstal dan mengkonfigurasi Prisma dalam project Node.js]]  
[[279. Cara mendefinisikan schema database menggunakan Prisma schema language]]  
[[280. Cara menjalankan Prisma migrate untuk membuat tabel di database]]  
[[281. Cara menggunakan Prisma Client untuk operasi CRUD]]  
[[282. Cara menggunakan Prisma relations untuk mendefinisikan relasi antar model]]  
[[283. Cara menggunakan Prisma query yaitu findUnique, findMany, create, update, delete]]  
[[284. Cara menggunakan Prisma filtering, sorting, dan pagination]]  
[[285. Cara menggunakan Prisma transactions]]  
[[286. Cara menggunakan Prisma Studio untuk GUI database management]]  
[[287. Pengenalan Sequelize sebagai ORM alternatif untuk SQL database]]  
[[288. Cara menginstal dan mengkonfigurasi Sequelize]]  
[[289. Cara mendefinisikan model dan relasi dalam Sequelize]]  
[[290. Cara menggunakan Sequelize migrations dan seeders]]  
[[291. Pengenalan Knex.js sebagai query builder yang fleksibel]]  
[[292. Cara menginstal dan menggunakan Knex.js untuk operasi database]]  
[[293. Cara menggunakan Knex migrations dan seeds]]  
[[294. Perbedaan antara ORM dan query builder dan kapan menggunakan masing-masing]]  
[[295. Cara memilih ORM yang tepat berdasarkan kebutuhan project]]

---

### 5.2 Keamanan Aplikasi Node.js

[[296. Memahami ancaman keamanan umum pada aplikasi Node.js]]  
[[297. Cara mencegah NoSQL injection dalam aplikasi MongoDB]]  
[[298. Cara mencegah SQL injection menggunakan parameterized query]]  
[[299. Cara mencegah XSS menggunakan sanitasi input dan output]]  
[[300. Cara menggunakan helmet.js untuk mengatur security headers]]  
[[301. Cara mengimplementasikan CSRF protection menggunakan csurf atau csrf-csrf]]  
[[302. Cara mengimplementasikan rate limiting menggunakan express-rate-limit]]  
[[303. Cara mengimplementasikan brute force protection pada endpoint login]]  
[[304. Cara menyimpan secret dan credential menggunakan environment variables]]  
[[305. Cara mengaudit dependency untuk kerentanan menggunakan npm audit]]  
[[306. Cara menggunakan package hpp untuk mencegah HTTP parameter pollution]]  
[[307. Cara mengimplementasikan input sanitasi menggunakan mongoSanitize]]  
[[308. Cara mengkonfigurasi CORS dengan benar untuk membatasi origin yang diizinkan]]  
[[309. Cara mengimplementasikan logging yang aman tanpa mencatat data sensitif]]  
[[310. Cara melakukan dependency update secara rutin untuk menutup kerentanan]]

---

### 5.3 Performa dan Caching

[[311. Cara mengidentifikasi bottleneck performa dalam aplikasi Node.js]]  
[[312. Cara menggunakan Node.js profiler bawaan dengan flag --prof]]  
[[313. Cara menggunakan clinic.js untuk profiling dan diagnosis masalah performa]]  
[[314. Cara mengimplementasikan caching menggunakan Redis dalam aplikasi Node.js]]  
[[315. Cara menginstal dan menggunakan ioredis untuk koneksi Redis]]  
[[316. Cara mengimplementasikan cache-aside pattern dalam Express]]  
[[317. Cara mengimplementasikan response caching menggunakan apicache]]  
[[318. Cara menggunakan compression middleware untuk mengurangi ukuran response]]  
[[319. Cara mengoptimasi database query dengan indexing yang tepat]]  
[[320. Cara menggunakan connection pooling untuk optimasi koneksi database]]  
[[321. Cara mengimplementasikan lazy loading untuk data yang tidak selalu dibutuhkan]]  
[[322. Cara menggunakan streams untuk menangani file besar tanpa membebani memori]]  
[[323. Cara mengimplementasikan pagination yang efisien menggunakan cursor-based pagination]]  
[[324. Cara menggunakan cluster module untuk memanfaatkan multi-core CPU]]  
[[325. Cara menggunakan PM2 untuk process management dan clustering]]

---

### 5.4 File Upload dan Stream

[[326. Cara mengimplementasikan file upload menggunakan package multer]]  
[[327. Cara mengkonfigurasi multer untuk menyimpan file ke disk]]  
[[328. Cara mengkonfigurasi multer untuk menyimpan file di memori]]  
[[329. Cara memvalidasi tipe dan ukuran file dalam multer]]  
[[330. Cara mengupload file ke Amazon S3 menggunakan AWS SDK]]  
[[331. Cara mengupload file ke Cloudinary menggunakan cloudinary package]]  
[[332. Cara menggunakan Readable Stream untuk membaca data secara streaming]]  
[[333. Cara menggunakan Writable Stream untuk menulis data secara streaming]]  
[[334. Cara menggunakan Transform Stream untuk mengubah data saat streaming]]  
[[335. Cara menggunakan pipe untuk menghubungkan stream]]  
[[336. Cara mengimplementasikan file download menggunakan stream]]  
[[337. Cara mengimplementasikan video streaming menggunakan range requests]]  
[[338. Cara menggunakan busboy untuk parsing multipart form data secara streaming]]  
[[339. Cara mengimplementasikan image resizing menggunakan sharp]]  
[[340. Cara mengimplementasikan image compression dan format conversion menggunakan sharp]]

---

## ⚫ LEVEL 6: ADVANCED

### 6.1 TypeScript dengan Node.js

[[341. Memahami keuntungan menggunakan TypeScript dalam project Node.js]]  
[[342. Cara menginstal TypeScript dan ts-node dalam project Node.js]]  
[[343. Cara mengkonfigurasi tsconfig.json untuk project Node.js]]  
[[344. Memahami tipe dasar TypeScript yaitu string, number, boolean, array, dan object]]  
[[345. Memahami interface dan type alias dalam TypeScript]]  
[[346. Memahami generic types dalam TypeScript]]  
[[347. Memahami union types dan intersection types]]  
[[348. Cara menggunakan TypeScript dengan Express yaitu tipe untuk Request dan Response]]  
[[349. Cara menggunakan TypeScript dengan Mongoose menggunakan interface dan schema]]  
[[350. Cara menggunakan TypeScript dengan Prisma yang sudah type-safe secara bawaan]]  
[[351. Cara menggunakan ts-node-dev atau nodemon untuk hot reload dalam TypeScript]]  
[[352. Cara mengkompilasi TypeScript ke JavaScript menggunakan tsc]]  
[[353. Cara menggunakan path alias dalam TypeScript untuk import yang lebih bersih]]  
[[354. Memahami declaration files yaitu file .d.ts dan kegunaannya]]  
[[355. Cara menggunakan utility types TypeScript yaitu Partial, Required, Pick, Omit, dll]]

---

### 6.2 Real-time Application dengan WebSocket

[[356. Memahami konsep WebSocket dan perbedaannya dengan HTTP]]  
[[357. Cara menggunakan modul ws untuk implementasi WebSocket dasar]]  
[[358. Cara membuat WebSocket server menggunakan ws]]  
[[359. Cara membuat WebSocket client menggunakan ws]]  
[[360. Cara menggunakan Socket.io untuk real-time communication yang lebih mudah]]  
[[361. Cara menginstal dan mengkonfigurasi Socket.io bersama Express]]  
[[362. Cara menggunakan event emit dan on dalam Socket.io]]  
[[363. Cara menggunakan rooms dan namespaces dalam Socket.io]]  
[[364. Cara mengimplementasikan real-time chat application menggunakan Socket.io]]  
[[365. Cara mengimplementasikan real-time notification menggunakan Socket.io]]  
[[366. Cara mengimplementasikan real-time dashboard menggunakan Socket.io]]  
[[367. Cara menangani reconnection dan disconnection dalam Socket.io]]  
[[368. Cara menggunakan Socket.io adapter untuk scaling dengan Redis]]  
[[369. Cara mengimplementasikan autentikasi pada WebSocket connection]]  
[[370. Cara menguji WebSocket endpoint menggunakan tools yang tersedia]]

---

### 6.3 Microservices dengan Node.js

[[371. Memahami arsitektur microservices dan kapan menggunakannya]]  
[[372. Memahami perbedaan antara monolith dan microservices]]  
[[373. Cara merancang batas service dalam arsitektur microservices]]  
[[374. Cara mengimplementasikan komunikasi antar service menggunakan HTTP REST]]  
[[375. Cara menggunakan axios untuk HTTP request antar service]]  
[[376. Cara mengimplementasikan komunikasi asynchronous menggunakan message broker]]  
[[377. Cara menggunakan RabbitMQ dengan Node.js menggunakan amqplib]]  
[[378. Cara menggunakan Apache Kafka dengan Node.js menggunakan kafkajs]]  
[[379. Cara mengimplementasikan API Gateway pattern dalam microservices]]  
[[380. Cara mengimplementasikan service discovery secara sederhana]]  
[[381. Cara mengimplementasikan circuit breaker pattern dalam Node.js]]  
[[382. Cara mengimplementasikan distributed tracing menggunakan OpenTelemetry]]  
[[383. Cara mengimplementasikan health check endpoint pada setiap service]]  
[[384. Cara menggunakan Docker untuk containerisasi setiap microservice]]  
[[385. Cara menggunakan Docker Compose untuk menjalankan multiple services secara lokal]]

---

### 6.4 GraphQL dengan Node.js

[[386. Memahami konsep GraphQL dan perbedaannya dengan REST API]]  
[[387. Memahami konsep schema, type, query, mutation, dan subscription dalam GraphQL]]  
[[388. Cara menginstal dan menggunakan Apollo Server dengan Express]]  
[[389. Cara mendefinisikan GraphQL schema menggunakan SDL yaitu Schema Definition Language]]  
[[390. Cara mendefinisikan resolver untuk query dan mutation]]  
[[391. Cara mengimplementasikan GraphQL query untuk mengambil data]]  
[[392. Cara mengimplementasikan GraphQL mutation untuk memodifikasi data]]  
[[393. Cara mengimplementasikan GraphQL subscription untuk real-time data]]  
[[394. Cara menggunakan DataLoader untuk mengatasi N plus 1 problem dalam GraphQL]]  
[[395. Cara mengimplementasikan authentication dan authorization dalam GraphQL]]  
[[396. Cara melakukan pagination dalam GraphQL menggunakan cursor-based pagination]]  
[[397. Cara menggunakan GraphQL dengan Mongoose atau Prisma]]  
[[398. Cara mendokumentasikan GraphQL API menggunakan GraphQL Playground atau Apollo Studio]]  
[[399. Cara melakukan testing GraphQL API]]  
[[400. Cara menggunakan code-first approach dengan TypeGraphQL]]

---

### 6.5 Design Patterns dalam Node.js

[[401. Memahami pentingnya design patterns dalam pengembangan Node.js]]  
[[402. Mengimplementasikan Repository Pattern dalam Node.js]]  
[[403. Mengimplementasikan Service Layer Pattern untuk memisahkan business logic]]  
[[404. Mengimplementasikan Factory Pattern dalam Node.js]]  
[[405. Mengimplementasikan Singleton Pattern untuk database connection]]  
[[406. Mengimplementasikan Observer Pattern menggunakan EventEmitter]]  
[[407. Mengimplementasikan Middleware Pattern dalam Express]]  
[[408. Mengimplementasikan Strategy Pattern dalam Node.js]]  
[[409. Mengimplementasikan Dependency Injection dalam Node.js]]  
[[410. Menggunakan InversifyJS sebagai DI container dalam Node.js]]  
[[411. Memahami SOLID principles dan cara menerapkannya dalam Node.js]]  
[[412. Mengimplementasikan Clean Architecture dalam project Node.js]]  
[[413. Mengimplementasikan CQRS pattern dalam Node.js]]  
[[414. Mengimplementasikan Event Sourcing dalam Node.js]]  
[[415. Memahami dan menghindari anti-patterns umum dalam Node.js]]

---

## 🟣 LEVEL 7: MASTERY DAN SPECIALIZATION

### 7.1 NestJS Framework

[[416. Pengenalan NestJS sebagai framework Node.js yang terstruktur berbasis TypeScript]]  
[[417. Memahami arsitektur NestJS yaitu modules, controllers, providers, dan services]]  
[[418. Cara menginstal dan membuat project NestJS menggunakan Nest CLI]]  
[[419. Cara membuat module dalam NestJS menggunakan Nest CLI]]  
[[420. Cara membuat controller dalam NestJS dan mendefinisikan route]]  
[[421. Cara membuat service atau provider dalam NestJS]]  
[[422. Memahami Dependency Injection bawaan NestJS]]  
[[423. Cara menggunakan decorator dalam NestJS yaitu Get, Post, Body, Param, Query, dll]]  
[[424. Cara menggunakan middleware, guards, interceptors, dan pipes dalam NestJS]]  
[[425. Cara mengimplementasikan validasi menggunakan class-validator dan class-transformer]]  
[[426. Cara menggunakan TypeORM atau Prisma bersama NestJS]]  
[[427. Cara mengimplementasikan autentikasi menggunakan NestJS Passport]]  
[[428. Cara mengimplementasikan JWT authentication dalam NestJS]]  
[[429. Cara menggunakan NestJS dengan WebSocket menggunakan @nestjs slash websockets]]  
[[430. Cara membuat microservice menggunakan NestJS microservices module]]

---

### 7.2 Serverless Node.js

[[431. Memahami konsep serverless computing dan kegunaannya]]  
[[432. Memahami AWS Lambda dan cara menjalankan Node.js di dalamnya]]  
[[433. Cara membuat dan men-deploy Lambda function menggunakan AWS CLI]]  
[[434. Cara menggunakan Serverless Framework untuk deployment yang lebih mudah]]  
[[435. Cara menggunakan AWS API Gateway bersama Lambda untuk membuat API]]  
[[436. Cara menggunakan Vercel untuk men-deploy Node.js serverless functions]]  
[[437. Cara menggunakan Netlify Functions untuk serverless Node.js]]  
[[438. Cara menggunakan Cloudflare Workers dengan JavaScript runtime]]  
[[439. Cara mengoptimasi cold start dalam serverless Node.js function]]  
[[440. Cara menangani environment variables dalam deployment serverless]]  
[[441. Cara mengimplementasikan database connection yang efisien dalam serverless]]  
[[442. Cara menggunakan AWS DynamoDB bersama serverless Node.js]]  
[[443. Cara mengimplementasikan file storage menggunakan AWS S3 dalam serverless]]  
[[444. Cara monitoring dan debugging serverless function menggunakan AWS CloudWatch]]  
[[445. Cara melakukan testing serverless function secara lokal]]

---

### 7.3 DevOps untuk Node.js Developer

[[446. Cara membuat Dockerfile yang optimal untuk aplikasi Node.js]]  
[[447. Cara menggunakan multi-stage build dalam Dockerfile untuk ukuran image yang lebih kecil]]  
[[448. Cara menggunakan Docker Compose untuk aplikasi Node.js dengan database dan Redis]]  
[[449. Cara menggunakan PM2 untuk process management di production]]  
[[450. Cara mengkonfigurasi PM2 ecosystem file]]  
[[451. Cara menggunakan PM2 cluster mode untuk memanfaatkan semua CPU core]]  
[[452. Cara mengkonfigurasi Nginx sebagai reverse proxy untuk aplikasi Node.js]]  
[[453. Cara mengkonfigurasi SSL dan HTTPS untuk aplikasi Node.js menggunakan Certbot]]  
[[454. Cara mengimplementasikan CI atau CD pipeline menggunakan GitHub Actions untuk Node.js]]  
[[455. Cara mengimplementasikan zero-downtime deployment untuk aplikasi Node.js]]  
[[456. Cara menggunakan Kubernetes untuk orchestrasi container Node.js]]  
[[457. Cara mengimplementasikan health check endpoint yang komprehensif]]  
[[458. Cara mengimplementasikan structured logging menggunakan Winston atau Pino]]  
[[459. Cara menggunakan monitoring tools yaitu Datadog, New Relic, atau Prometheus]]  
[[460. Cara mengimplementasikan alerting untuk error dan performa degradation]]

---

### 7.4 Performa Tingkat Lanjut dan Optimasi

[[461. Memahami memory management dalam Node.js dan cara mencegah memory leak]]  
[[462. Cara mendeteksi memory leak menggunakan tools seperti node --inspect dan heap snapshot]]  
[[463. Cara menggunakan worker threads untuk CPU-intensive tasks]]  
[[464. Cara menggunakan child process untuk menjalankan proses terpisah]]  
[[465. Cara menggunakan cluster module untuk distribusi load]]  
[[466. Cara mengimplementasikan connection pooling yang optimal untuk database]]  
[[467. Cara mengoptimasi serialisasi dan deserialisasi JSON]]  
[[468. Cara menggunakan buffer dan typed array untuk operasi binary yang efisien]]  
[[469. Cara mengimplementasikan backpressure handling dalam stream]]  
[[470. Cara menggunakan benchmarking tools yaitu autocannon dan wrk untuk load testing]]  
[[471. Cara mengimplementasikan horizontal scaling untuk aplikasi Node.js]]  
[[472. Cara menggunakan caching yang berlapis yaitu in-memory, Redis, dan CDN]]  
[[473. Cara mengoptimasi startup time aplikasi Node.js]]  
[[474. Cara menggunakan V8 optimization hints untuk kode yang lebih cepat]]  
[[475. Cara mengimplementasikan graceful shutdown dalam aplikasi Node.js]]

---

### 7.5 Ekosistem dan Tools Lanjutan

[[476. Menggunakan Fastify sebagai alternatif Express yang lebih cepat]]  
[[477. Memahami arsitektur dan keunggulan Fastify dibanding Express]]  
[[478. Menggunakan Hono sebagai framework Node.js yang sangat ringan dan cepat]]  
[[479. Menggunakan Bun sebagai JavaScript runtime alternatif yang lebih cepat dari Node.js]]  
[[480. Menggunakan Deno sebagai runtime JavaScript yang lebih aman]]  
[[481. Cara menggunakan tRPC untuk type-safe API tanpa schema definition manual]]  
[[482. Cara menggunakan Zod untuk runtime type validation yang type-safe]]  
[[483. Cara menggunakan Bull atau BullMQ untuk queue management yang powerful]]  
[[484. Cara menggunakan node-cron untuk task scheduling dalam Node.js]]  
[[485. Cara menggunakan Nodemailer untuk pengiriman email dalam Node.js]]  
[[486. Cara menggunakan Stripe API dengan Node.js untuk pembayaran]]  
[[487. Cara menggunakan Twilio untuk SMS dan komunikasi dalam Node.js]]  
[[488. Cara menggunakan OpenAI API dengan Node.js untuk integrasi AI]]  
[[489. Cara menggunakan Langchain.js untuk membangun aplikasi berbasis LLM]]  
[[490. Cara menggunakan Puppeteer atau Playwright untuk web scraping dan automation]]

---

### 7.6 Pengembangan Berkelanjutan dan Sumber Daya

[[491. Membaca dokumentasi resmi Node.js di nodejs.org secara efektif]]  
[[492. Mengikuti Node.js release schedule dan changelog untuk update terbaru]]  
[[493. Mengikuti newsletter Node Weekly untuk update ekosistem Node.js]]  
[[494. Bergabung dengan komunitas Node.js di Discord dan forum resmi]]  
[[495. Mempelajari source code Express, Fastify, atau NestJS untuk pemahaman mendalam]]  
[[496. Mengikuti konferensi NodeConf dan JSConf untuk update terbaru]]  
[[497. Berkontribusi ke ekosistem Node.js dengan membuat package npm open source]]  
[[498. Membangun portofolio project Node.js yang komprehensif untuk karir]]  
[[499. Menggunakan ESLint dan Prettier untuk menjaga kualitas kode Node.js]]  
[[500. Selalu memperbarui pengetahuan Node.js seiring perkembangan ekosistem JavaScript]]

---

## 📋 PETA PERKEMBANGAN (PROGRESS MAP)

|Level|Cakupan|Estimasi Waktu|
|---|---|---|
|Level 1 Absolute Beginner|Poin 1 hingga 65|2 hingga 4 minggu|
|Level 2 Elementary|Poin 66 hingga 135|3 hingga 5 minggu|
|Level 3 Pre-Intermediate|Poin 136 hingga 200|4 hingga 6 minggu|
|Level 4 Intermediate|Poin 201 hingga 275|6 hingga 10 minggu|
|Level 5 Upper-Intermediate|Poin 276 hingga 340|8 hingga 12 minggu|
|Level 6 Advanced|Poin 341 hingga 415|10 hingga 16 minggu|
|Level 7 Mastery|Poin 416 hingga 500|16 hingga 24 minggu|

---

## 🎯 TIPS PENGGUNAAN KURIKULUM INI

|Tips|Penjelasan|
|---|---|
|Kuasai JavaScript dahulu|Node.js adalah JavaScript sehingga fondasi JavaScript modern sangat diperlukan|
|Pahami asynchronous|Asynchronous programming adalah inti dari Node.js dan harus benar-benar dipahami|
|Bangun project nyata|Buat REST API nyata sejak level intermediate untuk memperkuat pemahaman|
|Pelajari keamanan|Keamanan API adalah hal krusial yang harus dipahami sejak awal|
|Kuasai satu database|Pilih MongoDB atau PostgreSQL dan kuasai secara mendalam sebelum mempelajari yang lain|
|Gunakan TypeScript|Mulai menggunakan TypeScript sejak level advanced untuk kode yang lebih maintainable|
|Konsistensi belajar|Belajar 1 hingga 2 jam setiap hari jauh lebih efektif daripada belajar intensif sesekali|

---

> _"Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications."_  
> — Ryan Dahl

> _"The Node.js ecosystem is one of the largest and most vibrant in the world, with over 2 million packages on npm."_

**Kurikulum ini mencakup 500 poin belajar Node.js yang dirancang membawa Anda dari nol hingga level mahir. Estimasi total waktu belajar adalah 12 hingga 24 bulan dengan latihan konsisten setiap hari.**