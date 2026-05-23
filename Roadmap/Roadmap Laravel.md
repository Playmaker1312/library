# 📚 Perpustakaan Digital: Kurikulum Laravel Komprehensif

### Panduan Belajar dari Fundamental hingga Advanced

---

## 🟢 LEVEL 1: ABSOLUTE BEGINNER (Pemula Mutlak)

### 1.1 Pengenalan Laravel

[[1. Apa itu Laravel dan sejarah perkembangannya dari versi 1 hingga Laravel 11]]  
[[2. Memahami filosofi Laravel yaitu elegant syntax dan developer happiness]]  
[[3. Keunggulan Laravel dibandingkan framework PHP lainnya]]  
[[4. Ekosistem Laravel yaitu Forge, Vapor, Nova, Cashier, Sanctum, Passport, dll]]  
[[5. Memahami versi Laravel dan kebijakan Long Term Support atau LTS]]  
[[6. Komunitas Laravel yaitu Laracasts, Laravel News, dan forum resmi]]  
[[7. Memahami peran Taylor Otwell dan kontributor utama Laravel]]  
[[8. Pengenalan tools yang digunakan bersama Laravel yaitu Composer, Artisan, dan npm]]  
[[9. Memahami Laravel sebagai full-stack framework dan sebagai API backend]]  
[[10. Prospek karir Laravel developer di industri saat ini]]

---

### 1.2 Instalasi dan Konfigurasi Awal

[[11. Persyaratan sistem untuk menjalankan Laravel yaitu PHP versi dan ekstensi yang dibutuhkan]]  
[[12. Cara menginstal Laravel menggunakan Laravel Installer via Composer]]  
[[13. Cara menginstal Laravel menggunakan perintah composer create-project]]  
[[14. Cara menginstal dan menggunakan Laravel Herd untuk lingkungan lokal di macOS]]  
[[15. Cara menginstal dan menggunakan Laragon untuk lingkungan lokal di Windows]]  
[[16. Cara menggunakan Laravel Sail untuk lingkungan development berbasis Docker]]  
[[17. Memahami struktur direktori Laravel secara menyeluruh]]  
[[18. Memahami kegunaan setiap folder yaitu app, bootstrap, config, database, public, resources, routes, storage, dan tests]]  
[[19. Cara mengkonfigurasi file env untuk environment development]]  
[[20. Cara menjalankan Laravel development server menggunakan perintah php artisan serve]]

---

### 1.3 Memahami File Konfigurasi

[[21. Memahami peran file env dan cara kerjanya dalam Laravel]]  
[[22. Cara mengakses nilai konfigurasi menggunakan helper config]]  
[[23. Memahami file konfigurasi di dalam folder config yaitu app, database, mail, cache, dll]]  
[[24. Cara membuat file konfigurasi kustom]]  
[[25. Memahami konsep environment dan cara Laravel membedakan development, testing, dan production]]  
[[26. Cara menggunakan perintah php artisan config cache untuk production]]  
[[27. Cara menggunakan perintah php artisan config clear]]  
[[28. Memahami APP KEY dan kegunaannya untuk enkripsi]]  
[[29. Cara menghasilkan APP KEY menggunakan perintah php artisan key generate]]  
[[30. Memahami konfigurasi timezone dan locale dalam Laravel]]

---

### 1.4 Artisan CLI

[[31. Memahami apa itu Artisan dan kegunaannya dalam workflow Laravel]]  
[[32. Cara menampilkan daftar semua perintah Artisan menggunakan php artisan list]]  
[[33. Cara mendapatkan bantuan perintah menggunakan php artisan help nama-perintah]]  
[[34. Cara membuat controller menggunakan php artisan make controller]]  
[[35. Cara membuat model menggunakan php artisan make model]]  
[[36. Cara membuat migration menggunakan php artisan make migration]]  
[[37. Cara membuat seeder menggunakan php artisan make seeder]]  
[[38. Cara membuat factory menggunakan php artisan make factory]]  
[[39. Cara membuat middleware menggunakan php artisan make middleware]]  
[[40. Cara membuat request menggunakan php artisan make request]]  
[[41. Cara membuat command kustom menggunakan php artisan make command]]  
[[42. Cara menjalankan tinker untuk berinteraksi dengan aplikasi secara interaktif]]  
[[43. Cara menggunakan perintah php artisan route list untuk melihat semua route]]  
[[44. Cara menggunakan perintah php artisan optimize untuk production]]  
[[45. Cara membuat komponen Artisan kustom dengan argumen dan opsi]]

---

### 1.5 Routing Dasar

[[46. Memahami konsep routing dalam Laravel]]  
[[47. Cara mendefinisikan route GET menggunakan Route get]]  
[[48. Cara mendefinisikan route POST menggunakan Route post]]  
[[49. Cara mendefinisikan route PUT, PATCH, dan DELETE]]  
[[50. Cara mendefinisikan route yang merespons semua HTTP method menggunakan Route any]]  
[[51. Cara menggunakan route dengan closure sebagai handler]]  
[[52. Cara menggunakan route dengan controller sebagai handler]]  
[[53. Cara mendefinisikan route parameters yaitu parameter wajib dan opsional]]  
[[54. Cara menggunakan route constraints menggunakan method where]]  
[[55. Cara memberi nama pada route menggunakan method name]]  
[[56. Cara membuat route group menggunakan Route group]]  
[[57. Cara menggunakan prefix pada route group]]  
[[58. Cara menggunakan middleware pada route dan route group]]  
[[59. Memahami file routing yaitu web.php, api.php, console.php, dan channels.php]]  
[[60. Cara menggunakan helper route untuk menghasilkan URL dari nama route]]

---

## 🔵 LEVEL 2: ELEMENTARY (Dasar Lanjutan)

### 2.1 Controller

[[61. Memahami peran controller dalam arsitektur MVC Laravel]]  
[[62. Cara membuat basic controller menggunakan Artisan]]  
[[63. Cara mendefinisikan method dalam controller]]  
[[64. Cara menghubungkan route ke controller method]]  
[[65. Cara membuat resource controller menggunakan flag resource]]  
[[66. Memahami tujuh method dalam resource controller yaitu index, create, store, show, edit, update, dan destroy]]  
[[67. Cara mendaftarkan resource controller menggunakan Route resource]]  
[[68. Cara membuat API resource controller menggunakan flag api]]  
[[69. Cara menggunakan single action controller menggunakan method invoke]]  
[[70. Cara menggunakan dependency injection dalam constructor controller]]  
[[71. Cara menggunakan dependency injection dalam method controller]]  
[[72. Cara membuat controller yang hanya mengekspos sebagian resource menggunakan only dan except]]  
[[73. Memahami konsep thin controller dan fat model dalam Laravel]]  
[[74. Cara menggunakan route model binding dalam controller]]  
[[75. Cara membuat nested resource controller untuk relasi parent-child]]

---

### 2.2 Blade Templating Engine

[[76. Memahami apa itu Blade dan keunggulannya sebagai templating engine]]  
[[77. Cara membuat file Blade dengan ekstensi blade.php]]  
[[78. Cara menampilkan variabel menggunakan double curly braces]]  
[[79. Perbedaan antara double curly braces yang escaped dan tanda seru double curly braces yang unescaped]]  
[[80. Cara menggunakan direktif blade-if, blade-elseif, dan blade-else]]  
[[81. Cara menggunakan direktif blade-unless sebagai kebalikan if]]  
[[82. Cara menggunakan direktif blade-isset dan blade-empty]]  
[[83. Cara menggunakan direktif blade-for, blade-foreach, blade-while, dan blade-forelse]]  
[[84. Cara menggunakan variabel loop dalam blade-foreach yaitu loop->index, loop->first, loop->last, dll]]  
[[85. Cara menggunakan direktif blade-switch dan blade-case]]  
[[86. Cara membuat layout menggunakan blade-extends dan blade-section]]  
[[87. Cara menggunakan blade-yield untuk mendefinisikan area konten]]  
[[88. Cara menggunakan blade-include untuk menyertakan partial view]]  
[[89. Cara menggunakan blade-component untuk komponen yang lebih terstruktur]]  
[[90. Cara menggunakan blade-slot dalam komponen Blade]]  
[[91. Cara menggunakan direktif blade-auth dan blade-guest]]  
[[92. Cara menggunakan direktif blade-can dan blade-cannot untuk otorisasi]]  
[[93. Cara menggunakan direktif blade-csrf untuk CSRF protection]]  
[[94. Cara menggunakan direktif blade-method untuk HTTP method spoofing]]  
[[95. Cara membuat direktif Blade kustom menggunakan Blade directive]]

---

### 2.3 View dan Asset

[[96. Cara mengembalikan view dari controller menggunakan helper view]]  
[[97. Cara passing data ke view menggunakan array atau metode with]]  
[[98. Cara menggunakan View make dan View share]]  
[[99. Cara menggunakan view composer untuk sharing data ke multiple views]]  
[[100. Cara menggunakan helper asset untuk merujuk ke file di folder public]]  
[[101. Memahami Vite sebagai asset bundler default di Laravel]]  
[[102. Cara mengkonfigurasi vite.config.js dalam project Laravel]]  
[[103. Cara menggunakan direktif blade-vite untuk me-load asset yang dikompilasi Vite]]  
[[104. Cara mengkompilasi CSS dan JavaScript menggunakan npm run dev]]  
[[105. Cara membangun asset untuk production menggunakan npm run build]]  
[[106. Cara mengintegrasikan Tailwind CSS dengan Laravel menggunakan Vite]]  
[[107. Cara mengintegrasikan Bootstrap dengan Laravel menggunakan Vite]]  
[[108. Cara menggunakan helper url dan helper secure url]]  
[[109. Cara menggunakan helper redirect untuk melakukan redirect]]  
[[110. Cara menggunakan flash message menggunakan session flash]]

---

### 2.4 Request dan Response

[[111. Memahami objek Request dalam Laravel]]  
[[112. Cara mengakses data input menggunakan metode input pada Request]]  
[[113. Cara mengakses data query string menggunakan metode query]]  
[[114. Cara mengakses data form menggunakan metode post]]  
[[115. Cara menggunakan metode all, only, dan except pada Request]]  
[[116. Cara menggunakan metode has, filled, dan missing pada Request]]  
[[117. Cara mengakses file upload menggunakan metode file pada Request]]  
[[118. Cara memeriksa tipe request menggunakan metode isMethod, expectsJson, dll]]  
[[119. Cara mendapatkan informasi request yaitu path, url, ip, dan userAgent]]  
[[120. Memahami objek Response dalam Laravel]]  
[[121. Cara mengembalikan response JSON menggunakan helper response json]]  
[[122. Cara mengembalikan response dengan status code tertentu]]  
[[123. Cara mengembalikan response dengan header kustom]]  
[[124. Cara mengembalikan response download file]]  
[[125. Cara mengembalikan response stream untuk file besar]]

---

## 🟡 LEVEL 3: PRE-INTERMEDIATE

### 3.1 Database dan Migration

[[126. Memahami konsep database migration dalam Laravel]]  
[[127. Cara membuat migration menggunakan Artisan dan memahami nama file migration]]  
[[128. Cara menggunakan Schema facade di dalam migration]]  
[[129. Cara membuat tabel menggunakan metode Schema create]]  
[[130. Cara mendefinisikan kolom dengan berbagai tipe yaitu string, integer, text, boolean, timestamp, dll]]  
[[131. Cara menggunakan modifier kolom yaitu nullable, default, unique, dan unsigned]]  
[[132. Cara membuat foreign key constraint dalam migration]]  
[[133. Cara menggunakan metode Schema table untuk memodifikasi tabel yang sudah ada]]  
[[134. Cara menjalankan migration menggunakan php artisan migrate]]  
[[135. Cara melakukan rollback migration menggunakan php artisan migrate rollback]]  
[[136. Cara menggunakan php artisan migrate fresh untuk drop semua tabel dan re-migrate]]  
[[137. Cara menggunakan php artisan migrate refresh untuk rollback dan re-migrate]]  
[[138. Cara menggunakan php artisan migrate status untuk melihat status migration]]  
[[139. Cara menggunakan kolom timestamps yaitu created at dan updated at]]  
[[140. Cara menggunakan softDeletes untuk implementasi soft delete]]

---

### 3.2 Eloquent ORM Dasar

[[141. Memahami konsep ORM dan bagaimana Eloquent bekerja]]  
[[142. Cara membuat Eloquent model menggunakan Artisan]]  
[[143. Memahami konvensi nama tabel Eloquent yaitu snake case plural]]  
[[144. Cara mengkonfigurasi nama tabel kustom menggunakan properti table]]  
[[145. Cara mengkonfigurasi primary key kustom menggunakan properti primaryKey]]  
[[146. Cara mengambil semua record menggunakan metode all]]  
[[147. Cara mengambil record berdasarkan primary key menggunakan metode find]]  
[[148. Cara menggunakan metode where untuk query dengan kondisi]]  
[[149. Cara menggunakan metode first, firstOrFail, findOrFail]]  
[[150. Cara menggunakan metode get untuk mendapatkan collection hasil query]]  
[[151. Cara membuat record baru menggunakan metode create]]  
[[152. Cara menggunakan metode save untuk membuat atau memperbarui record]]  
[[153. Cara memperbarui record menggunakan metode update]]  
[[154. Cara menghapus record menggunakan metode delete dan destroy]]  
[[155. Memahami mass assignment dan cara mengkonfigurasi fillable dan guarded]]

---

### 3.3 Eloquent Query Builder

[[156. Cara menggunakan metode select untuk memilih kolom tertentu]]  
[[157. Cara menggunakan metode where, orWhere, dan whereNot]]  
[[158. Cara menggunakan metode whereBetween, whereIn, dan whereNull]]  
[[159. Cara menggunakan metode orderBy dan latest serta oldest]]  
[[160. Cara menggunakan metode limit dan offset untuk pagination manual]]  
[[161. Cara menggunakan metode paginate untuk pagination otomatis Laravel]]  
[[162. Cara menggunakan metode simplePaginate dan cursorPaginate]]  
[[163. Cara menggunakan metode count, sum, avg, min, dan max]]  
[[164. Cara menggunakan metode groupBy dan having]]  
[[165. Cara menggunakan metode join, leftJoin, dan rightJoin]]  
[[166. Cara menggunakan metode with untuk eager loading relasi]]  
[[167. Cara menggunakan metode withCount untuk menghitung relasi]]  
[[168. Cara menggunakan metode exists dan doesntExist]]  
[[169. Cara menggunakan metode chunk untuk memproses data besar]]  
[[170. Cara menggunakan metode when untuk conditional query]]

---

### 3.4 Eloquent Relationships

[[171. Memahami jenis-jenis relasi dalam Eloquent]]  
[[172. Cara mendefinisikan relasi hasOne yaitu satu ke satu]]  
[[173. Cara mendefinisikan relasi belongsTo yaitu kebalikan hasOne dan hasMany]]  
[[174. Cara mendefinisikan relasi hasMany yaitu satu ke banyak]]  
[[175. Cara mendefinisikan relasi hasManyThrough yaitu relasi melalui model perantara]]  
[[176. Cara mendefinisikan relasi belongsToMany yaitu banyak ke banyak]]  
[[177. Cara menggunakan pivot table dalam relasi belongsToMany]]  
[[178. Cara mengakses data pivot menggunakan metode withPivot]]  
[[179. Cara mendefinisikan relasi morphOne dan morphMany yaitu polymorphic]]  
[[180. Cara mendefinisikan relasi morphToMany yaitu banyak ke banyak polymorphic]]  
[[181. Cara menggunakan eager loading dengan metode with untuk menghindari N plus 1]]  
[[182. Cara menggunakan lazy eager loading menggunakan metode load]]  
[[183. Cara menggunakan constrained eager loading untuk memfilter relasi yang di-load]]  
[[184. Cara menggunakan metode attach, detach, dan sync pada relasi belongsToMany]]  
[[185. Cara menggunakan metode associate dan dissociate pada relasi belongsTo]]

---

### 3.5 Seeder dan Factory

[[186. Memahami kegunaan seeder untuk mengisi database dengan data awal]]  
[[187. Cara membuat seeder menggunakan Artisan]]  
[[188. Cara menulis logika seeding di dalam metode run]]  
[[189. Cara menjalankan seeder menggunakan php artisan db seed]]  
[[190. Cara menjalankan seeder spesifik menggunakan flag class]]  
[[191. Cara memanggil seeder lain di dalam seeder menggunakan metode call]]  
[[192. Memahami kegunaan factory untuk membuat data palsu dalam jumlah besar]]  
[[193. Cara membuat factory menggunakan Artisan]]  
[[194. Cara mendefinisikan state default dalam factory menggunakan metode definition]]  
[[195. Cara menggunakan Faker library untuk menghasilkan data palsu yang realistis]]  
[[196. Cara membuat factory state untuk variasi data]]  
[[197. Cara menggunakan factory dalam seeder menggunakan metode factory]]  
[[198. Cara menggunakan factory dalam testing]]  
[[199. Cara mendefinisikan relasi dalam factory menggunakan metode for dan has]]  
[[200. Cara menggunakan metode count pada factory untuk membuat banyak record]]

---

## 🟠 LEVEL 4: INTERMEDIATE

### 4.1 Validasi

[[201. Memahami sistem validasi dalam Laravel]]  
[[202. Cara melakukan validasi menggunakan metode validate pada Request]]  
[[203. Cara mendefinisikan rules validasi seperti required, string, email, min, max, dll]]  
[[204. Cara menggunakan rule unique untuk validasi keunikan data di database]]  
[[205. Cara menggunakan rule exists untuk validasi keberadaan data di database]]  
[[206. Cara menampilkan pesan error validasi di Blade menggunakan direktif errors]]  
[[207. Cara membuat Form Request class untuk memisahkan logika validasi dari controller]]  
[[208. Cara mendefinisikan rules dan messages dalam Form Request]]  
[[209. Cara menggunakan metode authorize dalam Form Request]]  
[[210. Cara membuat custom validation rule menggunakan Artisan make rule]]  
[[211. Cara menggunakan Rule facade untuk rule yang lebih kompleks]]  
[[212. Cara melakukan validasi array dan nested data]]  
[[213. Cara menggunakan conditional validation menggunakan sometimes dan required if]]  
[[214. Cara mengkustomisasi pesan error validasi secara global di file lang]]  
[[215. Cara menggunakan validator facade untuk validasi manual di luar controller]]

---

### 4.2 Authentication

[[216. Memahami sistem autentikasi dalam Laravel]]  
[[217. Cara menginstal Laravel Breeze untuk scaffolding autentikasi sederhana]]  
[[218. Memahami fitur yang disediakan Laravel Breeze yaitu login, register, reset password, dll]]  
[[219. Cara menginstal Laravel Jetstream untuk autentikasi yang lebih lengkap]]  
[[220. Perbedaan antara Laravel Breeze dan Laravel Jetstream]]  
[[221. Cara menggunakan facade Auth untuk operasi autentikasi manual]]  
[[222. Cara melakukan login manual menggunakan Auth attempt]]  
[[223. Cara melakukan logout menggunakan Auth logout]]  
[[224. Cara mendapatkan user yang sedang login menggunakan Auth user]]  
[[225. Cara memeriksa apakah user sudah login menggunakan Auth check]]  
[[226. Cara mengimplementasikan remember me functionality]]  
[[227. Cara mengimplementasikan reset password]]  
[[228. Cara mengimplementasikan email verification]]  
[[229. Cara menggunakan middleware auth untuk melindungi route]]  
[[230. Cara mengkonfigurasi guard dan provider dalam config auth]]

---

### 4.3 Authorization

[[231. Memahami perbedaan antara autentikasi dan otorisasi]]  
[[232. Memahami dua cara otorisasi dalam Laravel yaitu Gate dan Policy]]  
[[233. Cara mendefinisikan Gate menggunakan facade Gate di AuthServiceProvider]]  
[[234. Cara menggunakan Gate allows, denies, dan check]]  
[[235. Cara menggunakan Gate dalam controller dan Blade]]  
[[236. Cara membuat Policy menggunakan Artisan make policy]]  
[[237. Cara mendaftarkan Policy di AuthServiceProvider]]  
[[238. Cara mendefinisikan method dalam Policy seperti viewAny, view, create, update, delete]]  
[[239. Cara menggunakan Policy dalam controller menggunakan metode authorize]]  
[[240. Cara menggunakan Policy dalam Blade menggunakan direktif can dan cannot]]  
[[241. Cara menggunakan middleware can pada route]]  
[[242. Cara mengimplementasikan role-based access control atau RBAC sederhana]]  
[[243. Cara menggunakan package Spatie Laravel Permission untuk manajemen role dan permission]]  
[[244. Cara mendefinisikan super admin yang melewati semua pengecekan otorisasi]]  
[[245. Cara menggunakan before hook pada Gate untuk super admin]]

---

### 4.4 Middleware

[[246. Memahami konsep middleware dan cara kerjanya dalam Laravel]]  
[[247. Cara membuat middleware kustom menggunakan Artisan make middleware]]  
[[248. Cara mendefinisikan logika di dalam metode handle middleware]]  
[[249. Cara mendaftarkan middleware global di bootstrap app.php pada Laravel 11]]  
[[250. Cara mendaftarkan middleware sebagai route middleware alias]]  
[[251. Cara menggunakan middleware pada route dan route group]]  
[[252. Cara menggunakan middleware parameter]]  
[[253. Memahami middleware pipeline dan urutan eksekusi]]  
[[254. Cara menggunakan after middleware yang berjalan setelah response dikirim]]  
[[255. Memahami middleware bawaan Laravel seperti Authenticate, VerifyCsrfToken, dll]]  
[[256. Cara menggunakan middleware throttle untuk rate limiting]]  
[[257. Cara membuat middleware untuk logging request]]  
[[258. Cara membuat middleware untuk memeriksa maintenance mode]]  
[[259. Cara menggunakan terminable middleware]]  
[[260. Cara mengecualikan route dari middleware tertentu]]

---

### 4.5 Eloquent Lanjutan

[[261. Memahami Eloquent events yaitu creating, created, updating, updated, deleting, deleted, dll]]  
[[262. Cara menggunakan Eloquent observers untuk menangani events secara terpisah]]  
[[263. Cara membuat observer menggunakan Artisan make observer]]  
[[264. Cara mendaftarkan observer di ServiceProvider]]  
[[265. Memahami dan menggunakan Eloquent scopes yaitu local scope dan global scope]]  
[[266. Cara mendefinisikan local scope menggunakan prefix scope pada method]]  
[[267. Cara mendefinisikan global scope untuk menerapkan kondisi ke semua query]]  
[[268. Cara membuat global scope sebagai class tersendiri]]  
[[269. Cara menggunakan metode withoutGlobalScope]]  
[[270. Memahami dan menggunakan Eloquent casting untuk mengubah tipe atribut]]  
[[271. Cara mendefinisikan casting di properti casts dalam model]]  
[[272. Cara menggunakan built-in casts yaitu integer, boolean, array, json, datetime, dll]]  
[[273. Cara membuat custom cast class]]  
[[274. Cara menggunakan Eloquent accessor untuk memodifikasi nilai saat diakses]]  
[[275. Cara menggunakan Eloquent mutator untuk memodifikasi nilai saat disimpan]]

---

## 🔴 LEVEL 5: UPPER-INTERMEDIATE

### 5.1 API Development dengan Laravel

[[276. Memahami pendekatan Laravel untuk membangun RESTful API]]  
[[277. Cara mengkonfigurasi routing API di file routes api.php]]  
[[278. Memahami perbedaan antara web middleware group dan api middleware group]]  
[[279. Cara menggunakan API Resource untuk mentransformasi data Eloquent]]  
[[280. Cara membuat API Resource menggunakan Artisan make resource]]  
[[281. Cara mendefinisikan transformasi data di dalam metode toArray pada Resource]]  
[[282. Cara membuat Resource Collection untuk koleksi data]]  
[[283. Cara menambahkan metadata menggunakan metode with pada Resource]]  
[[284. Cara menggunakan conditional attributes dalam Resource]]  
[[285. Cara mengimplementasikan autentikasi API menggunakan Laravel Sanctum]]  
[[286. Cara menginstal dan mengkonfigurasi Laravel Sanctum]]  
[[287. Cara mengimplementasikan token-based authentication menggunakan Sanctum]]  
[[288. Cara mengimplementasikan SPA authentication menggunakan Sanctum]]  
[[289. Cara mengimplementasikan autentikasi API menggunakan Laravel Passport]]  
[[290. Perbedaan antara Laravel Sanctum dan Laravel Passport]]

---

### 5.2 Queue dan Jobs

[[291. Memahami konsep queue dan mengapa digunakan dalam aplikasi web]]  
[[292. Cara mengkonfigurasi queue connection di file config queue.php]]  
[[293. Memahami queue drivers yaitu sync, database, redis, sqs, dll]]  
[[294. Cara membuat job menggunakan Artisan make job]]  
[[295. Cara mendefinisikan logika dalam metode handle pada job]]  
[[296. Cara mendispatch job menggunakan metode dispatch]]  
[[297. Cara mendispatch job ke queue tertentu menggunakan metode onQueue]]  
[[298. Cara menggunakan delay pada job menggunakan metode delay]]  
[[299. Cara menjalankan queue worker menggunakan php artisan queue work]]  
[[300. Cara menggunakan php artisan queue listen untuk development]]  
[[301. Cara mengimplementasikan job retry dan menentukan jumlah maksimal percobaan]]  
[[302. Cara mengimplementasikan job timeout]]  
[[303. Cara menangani job yang gagal menggunakan failed jobs table]]  
[[304. Cara menggunakan php artisan queue failed untuk melihat failed jobs]]  
[[305. Cara menggunakan job chaining dengan metode withChain]]  
[[306. Cara menggunakan job batching untuk memproses banyak job sekaligus]]  
[[307. Cara menggunakan event dan callback pada job batch]]  
[[308. Cara menggunakan Laravel Horizon untuk monitoring queue berbasis Redis]]  
[[309. Cara mengkonfigurasi supervisor untuk menjaga queue worker tetap berjalan]]  
[[310. Cara menggunakan queue untuk mengirim email secara asynchronous]]

---

### 5.3 Event dan Listener

[[311. Memahami konsep event-driven programming dalam Laravel]]  
[[312. Cara membuat event menggunakan Artisan make event]]  
[[313. Cara mendefinisikan data dalam event class]]  
[[314. Cara membuat listener menggunakan Artisan make listener]]  
[[315. Cara mendefinisikan logika dalam metode handle pada listener]]  
[[316. Cara mendaftarkan event dan listener di EventServiceProvider]]  
[[317. Cara menggunakan event discovery otomatis di Laravel]]  
[[318. Cara mendispatch event menggunakan helper event atau metode dispatch]]  
[[319. Cara membuat listener yang berjalan di queue]]  
[[320. Cara menggunakan built-in events Laravel seperti Registered, Login, Logout, dll]]  
[[321. Cara membuat event subscriber untuk mengelompokkan banyak listener]]  
[[322. Cara menggunakan model events melalui sistem event Laravel]]  
[[323. Cara menggunakan Notification sebagai alternatif untuk notifikasi lintas channel]]  
[[324. Cara membuat notifikasi menggunakan Artisan make notification]]  
[[325. Cara mengirim notifikasi via email, database, SMS, dan Slack]]

---

### 5.4 Mail

[[326. Memahami sistem mail dalam Laravel menggunakan Mailable class]]  
[[327. Cara membuat Mailable menggunakan Artisan make mail]]  
[[328. Cara mendefinisikan konten email dalam Mailable menggunakan metode content]]  
[[329. Cara menggunakan Blade view sebagai template email]]  
[[330. Cara menggunakan Markdown mail untuk email yang lebih mudah ditulis]]  
[[331. Cara mengirim email menggunakan facade Mail]]  
[[332. Cara mengirim email ke banyak penerima menggunakan metode to, cc, dan bcc]]  
[[333. Cara menambahkan attachment pada email]]  
[[334. Cara mengkonfigurasi SMTP driver menggunakan Mailtrap untuk development]]  
[[335. Cara menggunakan Mailgun, Postmark, atau SES sebagai mail driver production]]  
[[336. Cara mengirim email secara asynchronous menggunakan metode queue]]  
[[337. Cara menggunakan metode later untuk mengirim email dengan delay]]  
[[338. Cara menguji pengiriman email dalam testing menggunakan Mail fake]]  
[[339. Cara mengkustomisasi komponen Markdown mail]]  
[[340. Cara menggunakan metode attach untuk melampirkan file yang dihasilkan secara dinamis]]

---

## ⚫ LEVEL 6: ADVANCED

### 6.1 Service Container dan Service Provider

[[341. Memahami konsep Service Container dan Dependency Injection dalam Laravel]]  
[[342. Memahami cara Laravel menggunakan Service Container untuk resolusi dependency]]  
[[343. Cara melakukan binding sederhana menggunakan metode bind]]  
[[344. Cara melakukan singleton binding menggunakan metode singleton]]  
[[345. Cara melakukan instance binding menggunakan metode instance]]  
[[346. Cara melakukan contextual binding untuk binding yang bergantung pada konteks]]  
[[347. Cara menggunakan metode make untuk me-resolve dependency dari container]]  
[[348. Memahami automatic injection yaitu cara Laravel otomatis menginjeksi dependency]]  
[[349. Cara membuat Service Provider menggunakan Artisan make provider]]  
[[350. Cara mendaftarkan Service Provider di config app.php atau bootstrap app.php]]  
[[351. Memahami metode register dan boot dalam Service Provider]]  
[[352. Cara menggunakan deferred provider untuk optimasi performa]]  
[[353. Cara membuat facade kustom menggunakan Service Container]]  
[[354. Memahami cara kerja facade di balik layar]]  
[[355. Cara menggunakan helper app untuk mengakses Service Container]]

---

### 6.2 Caching

[[356. Memahami konsep caching dan kegunaannya dalam meningkatkan performa Laravel]]  
[[357. Cara mengkonfigurasi cache driver yaitu file, database, redis, memcached, dll]]  
[[358. Cara menyimpan data ke cache menggunakan facade Cache put]]  
[[359. Cara mengambil data dari cache menggunakan Cache get]]  
[[360. Cara menggunakan Cache remember untuk menyimpan dan mengambil sekaligus]]  
[[361. Cara menggunakan Cache rememberForever]]  
[[362. Cara menghapus item dari cache menggunakan Cache forget]]  
[[363. Cara menghapus semua cache menggunakan Cache flush]]  
[[364. Cara menggunakan cache tags untuk mengelompokkan cache]]  
[[365. Cara mengimplementasikan response caching]]  
[[366. Cara menggunakan atomic lock menggunakan Cache lock]]  
[[367. Cara mengimplementasikan query caching dalam Eloquent]]  
[[368. Cara menggunakan Redis sebagai cache driver dengan konfigurasi optimal]]  
[[369. Cara mengimplementasikan cache warming strategy]]  
[[370. Cara menggunakan Cache increment dan decrement untuk counter]]

---

### 6.3 Task Scheduling

[[371. Memahami konsep task scheduling dalam Laravel menggunakan Kernel]]  
[[372. Cara mendefinisikan scheduled task di dalam metode schedule]]  
[[373. Cara menggunakan berbagai frequency method yaitu everyMinute, hourly, daily, weekly, dll]]  
[[374. Cara menggunakan cron expression kustom menggunakan metode cron]]  
[[375. Cara menjadwalkan Artisan command menggunakan metode command]]  
[[376. Cara menjadwalkan queued job menggunakan metode job]]  
[[377. Cara menjadwalkan shell command menggunakan metode exec]]  
[[378. Cara menjadwalkan closure menggunakan metode call]]  
[[379. Cara menggunakan constraint yaitu weekdays, weekends, environments, dan when]]  
[[380. Cara menggunakan metode between dan unlessBetween]]  
[[381. Cara mencegah task overlap menggunakan metode withoutOverlapping]]  
[[382. Cara menjalankan task di background menggunakan metode runInBackground]]  
[[383. Cara menggunakan output redirect untuk menyimpan output task ke file]]  
[[384. Cara mengkonfigurasi cron job di server untuk menjalankan scheduler Laravel]]  
[[385. Cara menggunakan Laravel Schedule Monitor untuk memantau scheduled tasks]]

---

### 6.4 Testing dalam Laravel

[[386. Memahami pendekatan testing dalam Laravel menggunakan PHPUnit dan Pest]]  
[[387. Memahami struktur folder tests yaitu Unit dan Feature]]  
[[388. Cara membuat test menggunakan Artisan make test]]  
[[389. Cara menjalankan semua test menggunakan php artisan test]]  
[[390. Cara menggunakan metode get, post, put, patch, dan delete dalam feature test]]  
[[391. Cara menggunakan assertion yaitu assertStatus, assertJson, assertRedirect, dll]]  
[[392. Cara menggunakan metode actingAs untuk mensimulasikan user yang sudah login]]  
[[393. Cara menggunakan database testing dengan trait RefreshDatabase]]  
[[394. Cara menggunakan trait WithFaker dalam test]]  
[[395. Cara melakukan mocking menggunakan metode mock dan spy]]  
[[396. Cara menggunakan Mail fake, Event fake, Queue fake, dan Notification fake]]  
[[397. Cara menggunakan Http fake untuk mocking HTTP request eksternal]]  
[[398. Cara menulis unit test untuk Eloquent model dan service class]]  
[[399. Cara mengukur code coverage menggunakan flag coverage]]  
[[400. Cara mengintegrasikan testing ke dalam CI atau CD pipeline menggunakan GitHub Actions]]

---

### 6.5 Storage dan File Management

[[401. Memahami sistem storage dalam Laravel menggunakan Storage facade]]  
[[402. Cara mengkonfigurasi disk storage yaitu local, public, dan s3]]  
[[403. Cara menyimpan file menggunakan Storage put dan putFile]]  
[[404. Cara mengambil file menggunakan Storage get]]  
[[405. Cara memeriksa keberadaan file menggunakan Storage exists]]  
[[406. Cara menghapus file menggunakan Storage delete]]  
[[407. Cara membuat symbolic link menggunakan php artisan storage link]]  
[[408. Cara menghasilkan URL untuk file publik menggunakan Storage url]]  
[[409. Cara menghasilkan temporary URL untuk file private menggunakan Storage temporaryUrl]]  
[[410. Cara mengintegrasikan Laravel dengan Amazon S3]]  
[[411. Cara mengintegrasikan Laravel dengan Cloudflare R2 atau DigitalOcean Spaces]]  
[[412. Cara melakukan file upload yang aman dengan validasi tipe dan ukuran]]  
[[413. Cara mengimplementasikan image resizing menggunakan Intervention Image]]  
[[414. Cara mengimplementasikan file download yang dilindungi autentikasi]]  
[[415. Cara menggunakan Storage disk kustom]]

---

## 🟣 LEVEL 7: MASTERY DAN SPECIALIZATION

### 7.1 Performance Optimization

[[416. Cara mengidentifikasi bottleneck performa dalam aplikasi Laravel]]  
[[417. Cara menggunakan Laravel Debugbar untuk debugging dan profiling]]  
[[418. Cara menggunakan Laravel Telescope untuk monitoring aplikasi]]  
[[419. Cara mengoptimasi Eloquent query untuk menghindari N plus 1 problem]]  
[[420. Cara menggunakan eager loading secara strategis]]  
[[421. Cara mengimplementasikan database indexing yang tepat]]  
[[422. Cara menggunakan query builder untuk query yang sangat kompleks]]  
[[423. Cara mengimplementasikan caching strategy yang komprehensif]]  
[[424. Cara menggunakan Redis untuk session, cache, dan queue sekaligus]]  
[[425. Cara mengoptimasi asset loading menggunakan Vite dan CDN]]  
[[426. Cara menggunakan OPcache untuk mengoptimasi eksekusi PHP]]  
[[427. Cara mengimplementasikan horizontal scaling untuk aplikasi Laravel]]  
[[428. Cara menggunakan read dan write database connection]]  
[[429. Cara mengoptimasi autoloading Composer untuk production]]  
[[430. Cara menggunakan perintah php artisan optimize untuk production]]

---

### 7.2 Advanced Eloquent dan Database

[[431. Memahami dan mengimplementasikan Repository Pattern dalam Laravel]]  
[[432. Memahami dan mengimplementasikan Service Layer Pattern dalam Laravel]]  
[[433. Cara menggunakan database transactions dalam Eloquent dan Query Builder]]  
[[434. Cara menggunakan DB transaction dengan closure]]  
[[435. Cara mengimplementasikan optimistic locking menggunakan version column]]  
[[436. Cara mengimplementasikan pessimistic locking menggunakan lockForUpdate]]  
[[437. Cara menggunakan raw expressions dalam Eloquent menggunakan DB raw]]  
[[438. Cara mengimplementasikan full-text search menggunakan Laravel Scout]]  
[[439. Cara mengintegrasikan Laravel Scout dengan Algolia atau Meilisearch]]  
[[440. Cara mengimplementasikan multi-tenancy dalam Laravel]]  
[[441. Cara menggunakan package Spatie Laravel Multitenancy]]  
[[442. Cara mengimplementasikan database sharding secara konseptual]]  
[[443. Cara menggunakan multiple database connection dalam satu aplikasi]]  
[[444. Cara mengimplementasikan data archiving strategy]]  
[[445. Cara mengoptimasi query untuk dataset yang sangat besar]]

---

### 7.3 Arsitektur Laravel Tingkat Lanjut

[[446. Memahami dan mengimplementasikan Domain-Driven Design dalam Laravel]]  
[[447. Cara mengorganisasi kode menggunakan structure berbasis domain bukan berbasis layer]]  
[[448. Memahami dan mengimplementasikan Action pattern dalam Laravel]]  
[[449. Cara menggunakan package Loris Leiva laravel-actions]]  
[[450. Memahami dan mengimplementasikan CQRS dalam Laravel]]  
[[451. Cara memisahkan Command dan Query dalam arsitektur Laravel]]  
[[452. Memahami konsep Event Sourcing dan implementasinya dalam Laravel]]  
[[453. Cara menggunakan package Spatie laravel-event-sourcing]]  
[[454. Cara mengimplementasikan Hexagonal Architecture atau Ports and Adapters]]  
[[455. Cara mengorganisasi project Laravel berskala enterprise]]  
[[456. Cara mengimplementasikan module system dalam Laravel]]  
[[457. Cara menggunakan package nWidart laravel-modules]]  
[[458. Cara merancang API yang versioned dalam Laravel]]  
[[459. Cara mengimplementasikan GraphQL API menggunakan Lighthouse PHP]]  
[[460. Cara mengimplementasikan WebSocket menggunakan Laravel Reverb]]

---

### 7.4 DevOps dan Deployment Laravel

[[461. Cara men-deploy Laravel ke shared hosting dengan konfigurasi yang benar]]  
[[462. Cara men-deploy Laravel ke VPS Ubuntu menggunakan Nginx dan PHP-FPM]]  
[[463. Cara mengkonfigurasi Nginx sebagai web server untuk Laravel]]  
[[464. Cara mengkonfigurasi SSL menggunakan Let's Encrypt dan Certbot]]  
[[465. Cara menggunakan Laravel Forge untuk server provisioning dan deployment]]  
[[466. Cara menggunakan Laravel Vapor untuk serverless deployment di AWS]]  
[[467. Cara membuat Dockerfile untuk containerisasi aplikasi Laravel]]  
[[468. Cara membuat docker-compose.yml untuk Laravel dengan MySQL, Redis, dan Nginx]]  
[[469. Cara mengkonfigurasi CI atau CD menggunakan GitHub Actions untuk Laravel]]  
[[470. Cara mengimplementasikan zero-downtime deployment menggunakan Deployer PHP]]  
[[471. Cara mengkonfigurasi supervisor untuk queue worker di production]]  
[[472. Cara mengkonfigurasi cron job untuk Laravel scheduler di production]]  
[[473. Cara mengimplementasikan monitoring menggunakan Laravel Pulse]]  
[[474. Cara mengkonfigurasi log management menggunakan stack ELK atau Papertrail]]  
[[475. Cara mengimplementasikan backup otomatis menggunakan Spatie laravel-backup]]

---

### 7.5 Ekosistem Laravel Lanjutan

[[476. Memahami dan menggunakan Laravel Livewire untuk reactive UI tanpa banyak JavaScript]]  
[[477. Cara membuat Livewire component menggunakan Artisan]]  
[[478. Memahami lifecycle Livewire component]]  
[[479. Cara menggunakan Livewire untuk membuat data table interaktif]]  
[[480. Cara menggunakan Livewire untuk membuat form yang reactive]]  
[[481. Memahami dan menggunakan Inertia.js untuk membangun SPA dengan Laravel]]  
[[482. Cara mengintegrasikan Inertia.js dengan React di Laravel]]  
[[483. Cara mengintegrasikan Inertia.js dengan Vue.js di Laravel]]  
[[484. Cara menggunakan Laravel Cashier untuk integrasi pembayaran Stripe]]  
[[485. Cara mengimplementasikan subscription billing menggunakan Cashier]]  
[[486. Cara menggunakan Laravel Socialite untuk OAuth login yaitu Google, GitHub, dll]]  
[[487. Cara menggunakan Laravel Dusk untuk browser automation testing]]  
[[488. Cara menggunakan Laravel Nova untuk admin panel]]  
[[489. Cara menggunakan Filament sebagai alternatif admin panel open source]]  
[[490. Cara menggunakan Laravel Pint untuk code formatting otomatis]]

---

### 7.6 Pengembangan Berkelanjutan dan Sumber Daya

[[491. Membaca dokumentasi resmi Laravel di laravel.com secara efektif]]  
[[492. Mengikuti Laracasts sebagai sumber video learning Laravel terbaik]]  
[[493. Mengikuti Laravel News untuk update berita ekosistem Laravel]]  
[[494. Mengikuti Laravel Daily untuk tips dan tutorial singkat]]  
[[495. Bergabung dengan komunitas Laravel di Discord dan forum resmi]]  
[[496. Mengikuti konferensi Laracon online dan offline]]  
[[497. Mempelajari source code Laravel di GitHub untuk pemahaman mendalam]]  
[[498. Berkontribusi ke ekosistem Laravel dengan membuat package atau pull request]]  
[[499. Membangun portofolio project Laravel yang komprehensif untuk karir]]  
[[500. Selalu memperbarui pengetahuan Laravel seiring rilis versi baru setiap tahun]]

---

## 📋 PETA PERKEMBANGAN (PROGRESS MAP)

|Level|Cakupan|Estimasi Waktu|
|---|---|---|
|Level 1 Absolute Beginner|Poin 1 hingga 60|2 hingga 4 minggu|
|Level 2 Elementary|Poin 61 hingga 125|4 hingga 6 minggu|
|Level 3 Pre-Intermediate|Poin 126 hingga 200|5 hingga 8 minggu|
|Level 4 Intermediate|Poin 201 hingga 275|6 hingga 10 minggu|
|Level 5 Upper-Intermediate|Poin 276 hingga 340|8 hingga 12 minggu|
|Level 6 Advanced|Poin 341 hingga 415|10 hingga 16 minggu|
|Level 7 Mastery|Poin 416 hingga 500|16 hingga 24 minggu|

---

## 🎯 TIPS PENGGUNAAN KURIKULUM INI

|Tips|Penjelasan|
|---|---|
|Kuasai PHP dahulu|Laravel adalah framework PHP sehingga fondasi PHP yang kuat sangat diperlukan|
|Belajar dengan project|Bangun project nyata sejak awal karena Laravel paling baik dipelajari melalui praktek|
|Manfaatkan Artisan|Biasakan menggunakan Artisan CLI karena sangat mempercepat workflow pengembangan|
|Baca source code|Membaca source code Laravel mengajarkan banyak pola dan best practice yang tidak ada di dokumentasi|
|Pahami dokumentasi|Dokumentasi Laravel sangat lengkap dan selalu menjadi referensi utama|
|Ikuti konvensi|Laravel memiliki konvensi yang kuat sehingga mengikutinya membuat kode lebih mudah dipahami tim|
|Konsistensi belajar|Belajar 1 hingga 2 jam setiap hari jauh lebih efektif daripada belajar intensif sesekali|

---

> _"Laravel is not just a framework. It is a philosophy of software craftsmanship that believes elegant code leads to better products."_  
> — Taylor Otwell

**Kurikulum ini mencakup 500 poin belajar Laravel yang dirancang membawa Anda dari nol hingga level mahir. Estimasi total waktu belajar adalah 12 hingga 24 bulan dengan latihan konsisten setiap hari.**