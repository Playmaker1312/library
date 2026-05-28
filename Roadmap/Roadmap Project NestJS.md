
# 📚 Perpustakaan Digital: Kurikulum Project Website Menggunakan NestJS Komprehensif

### Panduan Belajar dari Fundamental hingga Advanced

---

## 🟢 LEVEL 1: FONDASI NESTJS SEBELUM MEMBUAT PROJECT

### 1.1 Persiapan Lingkungan Kerja

[[1. Cara menginstal Node.js versi LTS terbaru sebagai runtime utama NestJS]]  
[[2. Cara menginstal npm atau yarn atau pnpm sebagai package manager]]  
[[3. Cara menginstal NestJS CLI secara global menggunakan perintah npm install -g @nestjs/cli]]  
[[4. Cara memverifikasi instalasi NestJS CLI menggunakan perintah nest --version]]  
[[5. Cara menginstal VS Code sebagai code editor utama untuk pengembangan NestJS]]  
[[6. Cara menginstal ekstensi NestJS Snippets di VS Code untuk produktivitas]]  
[[7. Cara menginstal ekstensi TypeScript Hero di VS Code untuk manajemen import]]  
[[8. Cara menginstal ekstensi Prettier dan ESLint di VS Code untuk kode yang konsisten]]  
[[9. Cara menginstal ekstensi REST Client di VS Code untuk testing API]]  
[[10. Cara menginstal dan menggunakan Postman atau Insomnia untuk testing API]]  
[[11. Cara menginstal Docker Desktop untuk kebutuhan containerisasi database]]  
[[12. Cara menjalankan PostgreSQL menggunakan Docker untuk development]]  
[[13. Cara menjalankan MongoDB menggunakan Docker untuk development]]  
[[14. Cara menjalankan Redis menggunakan Docker untuk caching dan queue]]  
[[15. Cara menginstal TablePlus atau DBeaver sebagai GUI database client]]

---

### 1.2 TypeScript yang Wajib Dikuasai Sebelum NestJS

[[16. Memahami tipe dasar TypeScript yaitu string, number, boolean, array, tuple, dan enum]]  
[[17. Memahami interface dan type alias serta kapan menggunakan masing-masing]]  
[[18. Memahami class dan access modifiers yaitu public, private, dan protected]]  
[[19. Memahami generic types dan cara menggunakannya]]  
[[20. Memahami union types dan intersection types]]  
[[21. Memahami utility types yaitu Partial, Required, Pick, Omit, Record, dan Readonly]]  
[[22. Memahami decorators dalam TypeScript sebagai fondasi utama NestJS]]  
[[23. Memahami experimentalDecorators dan cara mengaktifkannya di tsconfig.json]]  
[[24. Memahami abstract class dan abstract method dalam TypeScript]]  
[[25. Memahami namespace dan module dalam TypeScript]]  
[[26. Memahami conditional types dan mapped types untuk type yang lebih kompleks]]  
[[27. Memahami declaration merging dan augmentation]]  
[[28. Memahami async dan await dengan TypeScript]]  
[[29. Memahami error handling dengan TypeScript menggunakan try catch dan custom error types]]  
[[30. Memahami path alias dalam tsconfig.json untuk import yang lebih bersih]]

---

### 1.3 Arsitektur dan Konsep Inti NestJS

[[31. Memahami filosofi NestJS yaitu opinionated framework yang terinspirasi Angular]]  
[[32. Memahami arsitektur modular NestJS dan mengapa penting untuk scalability]]  
[[33. Memahami peran Module sebagai unit organisasi kode dalam NestJS]]  
[[34. Memahami peran Controller sebagai handler HTTP request dalam NestJS]]  
[[35. Memahami peran Provider dan Service sebagai business logic dalam NestJS]]  
[[36. Memahami Dependency Injection secara mendalam dalam konteks NestJS]]  
[[37. Memahami decorator utama NestJS yaitu Module, Controller, Injectable, dan Get Post Put Patch Delete]]  
[[38. Memahami lifecycle hooks NestJS yaitu OnModuleInit, OnApplicationBootstrap, OnModuleDestroy]]  
[[39. Memahami cara NestJS membangun dependency graph secara internal]]  
[[40. Memahami perbedaan antara NestJS dengan framework Express dan Fastify biasa]]

---

### 1.4 Membuat Project NestJS Pertama

[[41. Cara membuat project NestJS baru menggunakan perintah nest new nama-project]]  
[[42. Memahami struktur folder default project NestJS yaitu src, test, dan file konfigurasi]]  
[[43. Memahami kegunaan file main.ts sebagai entry point aplikasi NestJS]]  
[[44. Memahami kegunaan file app.module.ts sebagai root module]]  
[[45. Memahami kegunaan file app.controller.ts sebagai root controller]]  
[[46. Memahami kegunaan file app.service.ts sebagai root service]]  
[[47. Cara menjalankan NestJS dalam mode development menggunakan npm run start:dev]]  
[[48. Cara menjalankan NestJS dalam mode production menggunakan npm run start:prod]]  
[[49. Cara menjalankan NestJS dalam mode debug menggunakan npm run start:debug]]  
[[50. Memahami file konfigurasi tsconfig.json dan tsconfig.build.json dalam NestJS]]  
[[51. Memahami file konfigurasi nest-cli.json dan kegunaannya]]  
[[52. Memahami file konfigurasi .eslintrc.js dan .prettierrc dalam project NestJS]]  
[[53. Cara menggunakan perintah nest generate atau nest g untuk membuat file baru]]  
[[54. Cara membuat module baru menggunakan nest g module nama-module]]  
[[55. Cara membuat controller baru menggunakan nest g controller nama-controller]]

---

### 1.5 Routing dan Controller Dasar

[[56. Cara mendefinisikan route GET menggunakan decorator Get dalam controller]]  
[[57. Cara mendefinisikan route POST menggunakan decorator Post]]  
[[58. Cara mendefinisikan route PUT menggunakan decorator Put]]  
[[59. Cara mendefinisikan route PATCH menggunakan decorator Patch]]  
[[60. Cara mendefinisikan route DELETE menggunakan decorator Delete]]  
[[61. Cara menggunakan decorator Param untuk mengakses route parameter]]  
[[62. Cara menggunakan decorator Query untuk mengakses query string]]  
[[63. Cara menggunakan decorator Body untuk mengakses request body]]  
[[64. Cara menggunakan decorator Headers untuk mengakses request headers]]  
[[65. Cara menggunakan decorator Req dan Res untuk akses langsung ke objek request dan response]]  
[[66. Cara menggunakan decorator HttpCode untuk mengatur HTTP status code response]]  
[[67. Cara menggunakan decorator Header untuk menambahkan response header]]  
[[68. Cara menggunakan decorator Redirect untuk melakukan redirect]]  
[[69. Cara menggunakan prefix route pada controller menggunakan decorator Controller dengan string]]  
[[70. Cara menggunakan wildcard route dalam NestJS]]

---

## 🔵 LEVEL 2: FONDASI PROJECT (Komponen Inti NestJS)

### 2.1 Providers dan Dependency Injection

[[71. Memahami konsep provider dalam NestJS yaitu service, repository, factory, helper]]  
[[72. Cara membuat service menggunakan decorator Injectable]]  
[[73. Cara menginjeksi service ke dalam controller melalui constructor]]  
[[74. Cara menginjeksi service ke dalam service lain]]  
[[75. Memahami scope provider yaitu DEFAULT, REQUEST, dan TRANSIENT]]  
[[76. Cara menggunakan custom provider dengan useValue]]  
[[77. Cara menggunakan custom provider dengan useClass]]  
[[78. Cara menggunakan custom provider dengan useFactory]]  
[[79. Cara menggunakan custom provider dengan useExisting]]  
[[80. Cara menggunakan token injection menggunakan decorator Inject]]  
[[81. Cara menggunakan optional dependency menggunakan decorator Optional]]  
[[82. Cara mengekspor provider dari module agar bisa digunakan module lain]]  
[[83. Cara menggunakan global module menggunakan decorator Global]]  
[[84. Cara menggunakan forwardRef untuk mengatasi circular dependency]]  
[[85. Memahami module re-exporting untuk menyederhanakan import]]

---

### 2.2 Middleware

[[86. Memahami konsep middleware dalam NestJS dan perbedaannya dengan Express middleware]]  
[[87. Cara membuat middleware class yang mengimplementasikan NestMiddleware]]  
[[88. Cara mendefinisikan logika dalam metode use pada middleware]]  
[[89. Cara mendaftarkan middleware di module menggunakan configure dan MiddlewareConsumer]]  
[[90. Cara menerapkan middleware ke route tertentu menggunakan forRoutes]]  
[[91. Cara mengecualikan route dari middleware menggunakan exclude]]  
[[92. Cara membuat functional middleware sebagai alternatif class middleware]]  
[[93. Cara menerapkan middleware secara global menggunakan app.use]]  
[[94. Cara membuat middleware untuk logging request yang masuk]]  
[[95. Cara membuat middleware untuk mengukur response time]]  
[[96. Cara menggunakan middleware untuk kompresi menggunakan package compression]]  
[[97. Cara menggunakan middleware helmet untuk security headers]]  
[[98. Cara menggunakan middleware cors untuk Cross-Origin Resource Sharing]]  
[[99. Cara membuat middleware untuk validasi API key sederhana]]  
[[100. Cara menggunakan middleware morgan untuk HTTP request logging]]

---

### 2.3 Exception Filters

[[101. Memahami konsep exception handling dalam NestJS]]  
[[102. Memahami built-in HTTP exceptions yaitu NotFoundException, BadRequestException, UnauthorizedException, dll]]  
[[103. Cara melempar HTTP exception menggunakan throw new NotFoundException]]  
[[104. Cara membuat custom exception class yang memperluas HttpException]]  
[[105. Cara membuat custom exception filter menggunakan decorator Catch]]  
[[106. Cara mengimplementasikan interface ExceptionFilter dalam custom filter]]  
[[107. Cara mendaftarkan exception filter pada controller menggunakan UseFilters]]  
[[108. Cara mendaftarkan exception filter secara global menggunakan app.useGlobalFilters]]  
[[109. Cara membuat global exception filter yang menangkap semua jenis exception]]  
[[110. Cara memformat response error secara konsisten menggunakan exception filter]]  
[[111. Cara menangkap exception Prisma atau TypeORM dalam exception filter]]  
[[112. Cara menggunakan ArgumentsHost untuk mendapatkan konteks request dan response]]  
[[113. Cara membuat exception filter yang berbeda untuk REST dan GraphQL]]  
[[114. Cara menambahkan logging pada exception filter]]  
[[115. Cara membuat exception filter dengan response format yang sesuai standar API]]

---

### 2.4 Pipes

[[116. Memahami konsep pipes dalam NestJS untuk transformasi dan validasi data]]  
[[117. Memahami built-in pipes yaitu ValidationPipe, ParseIntPipe, ParseBoolPipe, ParseUUIDPipe, dll]]  
[[118. Cara menggunakan ParseIntPipe untuk mengonversi parameter string ke integer]]  
[[119. Cara menggunakan ParseUUIDPipe untuk memvalidasi format UUID]]  
[[120. Cara menggunakan DefaultValuePipe untuk memberikan nilai default pada parameter]]  
[[121. Cara menggunakan ValidationPipe untuk validasi otomatis menggunakan class-validator]]  
[[122. Cara menginstal class-validator dan class-transformer untuk validasi]]  
[[123. Cara membuat DTO yaitu Data Transfer Object menggunakan class]]  
[[124. Cara menggunakan decorator class-validator yaitu IsString, IsEmail, IsNumber, IsNotEmpty, dll]]  
[[125. Cara menggunakan decorator class-transformer yaitu Transform dan Type]]  
[[126. Cara mendaftarkan ValidationPipe secara global menggunakan app.useGlobalPipes]]  
[[127. Cara mengkonfigurasi ValidationPipe yaitu whitelist, forbidNonWhitelisted, dan transform]]  
[[128. Cara membuat custom pipe yang mengimplementasikan interface PipeTransform]]  
[[129. Cara membuat pipe untuk sanitasi input]]  
[[130. Cara menggunakan pipes pada level method dan parameter secara spesifik]]

---

### 2.5 Guards

[[131. Memahami konsep guards dalam NestJS untuk authentication dan authorization]]  
[[132. Cara membuat guard yang mengimplementasikan interface CanActivate]]  
[[133. Cara menggunakan ExecutionContext untuk mengakses request dalam guard]]  
[[134. Cara mendaftarkan guard pada route menggunakan decorator UseGuards]]  
[[135. Cara mendaftarkan guard secara global menggunakan app.useGlobalGuards]]  
[[136. Cara membuat JWT authentication guard]]  
[[137. Cara menggunakan decorator SetMetadata untuk menambahkan metadata pada route]]  
[[138. Cara membuat decorator kustom menggunakan Reflector dan SetMetadata]]  
[[139. Cara membuat decorator Public untuk menandai route yang tidak perlu autentikasi]]  
[[140. Cara menggunakan Reflector dalam guard untuk membaca metadata route]]  
[[141. Cara membuat role-based authorization guard]]  
[[142. Cara membuat decorator Roles untuk menentukan role yang diizinkan]]  
[[143. Cara mengimplementasikan permission-based guard yang lebih granular]]  
[[144. Cara membuat guard yang memeriksa ownership resource]]  
[[145. Cara menggabungkan beberapa guard menggunakan array dalam UseGuards]]

---

### 2.6 Interceptors

[[146. Memahami konsep interceptors dalam NestJS dan kegunaannya]]  
[[147. Cara membuat interceptor yang mengimplementasikan interface NestInterceptor]]  
[[148. Cara menggunakan Observable dan rxjs dalam interceptor]]  
[[149. Cara mendaftarkan interceptor pada controller menggunakan UseInterceptors]]  
[[150. Cara mendaftarkan interceptor secara global menggunakan app.useGlobalInterceptors]]  
[[151. Cara membuat interceptor untuk transformasi response data]]  
[[152. Cara membuat response wrapper interceptor untuk format response yang konsisten]]  
[[153. Cara membuat interceptor untuk logging durasi request]]  
[[154. Cara membuat interceptor untuk caching response]]  
[[155. Cara membuat interceptor untuk timeout request]]  
[[156. Cara membuat interceptor untuk menangani error secara terpusat]]  
[[157. Cara membuat interceptor untuk serialisasi data menggunakan class-transformer]]  
[[158. Cara menggunakan ExecutionContext dalam interceptor untuk konteks request]]  
[[159. Cara menggabungkan interceptor dengan guard dan pipe dalam pipeline]]  
[[160. Memahami urutan eksekusi middleware, guard, interceptor, pipe, dan exception filter]]

---

## 🟡 LEVEL 3: PROJECT DASAR (REST API Lengkap)

### 3.1 Project 1 - REST API CRUD Sederhana dengan NestJS

[[161. Merencanakan arsitektur API CRUD untuk entitas Product dengan semua endpoint]]  
[[162. Membuat project NestJS baru dan mengkonfigurasi ESLint dan Prettier]]  
[[163. Membuat ProductModule sebagai feature module untuk manajemen produk]]  
[[164. Membuat ProductController dengan semua route CRUD yaitu GET POST PUT PATCH DELETE]]  
[[165. Membuat ProductService dengan logika bisnis CRUD menggunakan in-memory array]]  
[[166. Membuat CreateProductDto dengan validasi menggunakan class-validator]]  
[[167. Membuat UpdateProductDto menggunakan PartialType dari CreateProductDto]]  
[[168. Mengimplementasikan endpoint GET products dengan pagination dan filter]]  
[[169. Mengimplementasikan endpoint GET product by ID dengan error handling]]  
[[170. Mengimplementasikan endpoint POST create product dengan validasi]]  
[[171. Mengimplementasikan endpoint PUT update product dengan validasi]]  
[[172. Mengimplementasikan endpoint DELETE product dengan soft delete]]  
[[173. Mengimplementasikan global ValidationPipe dan global exception filter]]  
[[174. Mengimplementasikan response wrapper interceptor untuk format yang konsisten]]  
[[175. Menguji semua endpoint menggunakan Postman atau REST Client]]

---

### 3.2 Integrasi Database dengan TypeORM

[[176. Memahami konsep ORM dan keunggulan TypeORM dalam ekosistem NestJS]]  
[[177. Cara menginstal TypeORM dan driver database yang sesuai]]  
[[178. Cara mengkonfigurasi TypeORM menggunakan TypeOrmModule.forRoot di AppModule]]  
[[179. Cara membuat entity TypeORM menggunakan decorator Entity dan Column]]  
[[180. Cara menggunakan decorator Column dengan tipe data yang beragam]]  
[[181. Cara menggunakan decorator PrimaryGeneratedColumn untuk auto-increment dan UUID]]  
[[182. Cara menggunakan decorator CreateDateColumn, UpdateDateColumn, dan DeleteDateColumn]]  
[[183. Cara menggunakan TypeOrmModule.forFeature untuk mendaftarkan entity di feature module]]  
[[184. Cara menginjeksi repository menggunakan decorator InjectRepository]]  
[[185. Cara melakukan operasi CRUD menggunakan TypeORM repository methods]]  
[[186. Cara menggunakan find, findOne, findOneBy, save, update, delete, dan softDelete]]  
[[187. Cara menggunakan QueryBuilder untuk query yang lebih kompleks]]  
[[188. Cara menjalankan TypeORM migration menggunakan CLI]]  
[[189. Cara membuat migration secara manual dan otomatis menggunakan TypeORM]]  
[[190. Cara menggunakan TypeORM subscriber untuk event-driven database operations]]

---

### 3.3 Integrasi Database dengan Prisma

[[191. Memahami keunggulan Prisma dibanding TypeORM dalam konteks NestJS]]  
[[192. Cara menginstal Prisma dan Prisma Client dalam project NestJS]]  
[[193. Cara menginisialisasi Prisma menggunakan perintah npx prisma init]]  
[[194. Cara mendefinisikan schema database dalam file schema.prisma]]  
[[195. Cara mendefinisikan model Prisma dengan field dan tipe data yang benar]]  
[[196. Cara menjalankan Prisma migrate menggunakan npx prisma migrate dev]]  
[[197. Cara menghasilkan Prisma Client menggunakan npx prisma generate]]  
[[198. Cara membuat PrismaService yang memperluas PrismaClient]]  
[[199. Cara mengimplementasikan OnModuleInit dan enableShutdownHooks dalam PrismaService]]  
[[200. Cara menginjeksi PrismaService ke dalam service lain]]  
[[201. Cara melakukan operasi CRUD menggunakan Prisma Client]]  
[[202. Cara menggunakan Prisma findMany dengan filter, orderBy, skip, dan take]]  
[[203. Cara menggunakan Prisma relations yaitu include dan select]]  
[[204. Cara menggunakan Prisma transactions dalam NestJS]]  
[[205. Cara menggunakan Prisma Studio untuk manajemen data secara visual]]

---

### 3.4 Project 2 - REST API dengan PostgreSQL dan Prisma

[[206. Merencanakan schema database untuk aplikasi Blog API dengan User, Post, Comment, dan Tag]]  
[[207. Mendefinisikan schema Prisma untuk semua model dengan relasi yang tepat]]  
[[208. Menjalankan migration dan seed data awal menggunakan Prisma]]  
[[209. Membangun UserModule dengan CRUD endpoint dan relasi ke Post]]  
[[210. Membangun PostModule dengan endpoint create, read, update, dan delete]]  
[[211. Mengimplementasikan pagination pada endpoint GET all posts]]  
[[212. Mengimplementasikan filter berdasarkan kategori, tag, dan status pada posts]]  
[[213. Membangun CommentModule dengan nested resource routing]]  
[[214. Mengimplementasikan relasi many-to-many antara Post dan Tag]]  
[[215. Mengimplementasikan soft delete menggunakan field deletedAt pada semua model]]  
[[216. Mengimplementasikan endpoint search post berdasarkan keyword]]  
[[217. Mengimplementasikan slug generation otomatis untuk post]]  
[[218. Mengimplementasikan image upload untuk post thumbnail menggunakan Multer]]  
[[219. Mengimplementasikan response transformation menggunakan class-transformer]]  
[[220. Mendokumentasikan semua endpoint menggunakan Swagger]]

---

### 3.5 Konfigurasi dan Environment

[[221. Cara menginstal dan menggunakan ConfigModule dari NestJS]]  
[[222. Cara mengkonfigurasi ConfigModule.forRoot dengan file .env]]  
[[223. Cara menggunakan ConfigService untuk mengakses environment variables]]  
[[224. Cara membuat konfigurasi yang tervalidasi menggunakan Joi]]  
[[225. Cara menggunakan validationSchema dalam ConfigModule untuk validasi env]]  
[[226. Cara membuat namespace configuration menggunakan registerAs]]  
[[227. Cara menggunakan ConfigModule.forFeature untuk konfigurasi per module]]  
[[228. Cara membuat custom configuration file untuk setiap environment]]  
[[229. Cara mengakses nested configuration menggunakan dot notation]]  
[[230. Cara menggunakan InjectValue atau Get decorator untuk injeksi nilai konfigurasi]]  
[[231. Cara membuat typed configuration dengan interface]]  
[[232. Cara menggunakan process.env langsung versus ConfigService dan trade-off-nya]]  
[[233. Cara mengkonfigurasi environment berbeda yaitu development, staging, dan production]]  
[[234. Cara menyimpan secret menggunakan vault atau AWS Parameter Store secara konseptual]]  
[[235. Cara membuat config module yang reusable untuk digunakan di berbagai module]]

---

## 🟠 LEVEL 4: PROJECT INTERMEDIATE (Fitur Lengkap API)

### 4.1 Authentication dan Authorization

[[236. Merencanakan sistem autentikasi lengkap menggunakan JWT dalam NestJS]]  
[[237. Cara menginstal package Passport dan NestJS Passport adapter]]  
[[238. Cara menginstal package jsonwebtoken dan NestJS JWT module]]  
[[239. Cara mengkonfigurasi JwtModule menggunakan registerAsync dengan ConfigService]]  
[[240. Cara membuat AuthModule yang mengintegrasikan semua komponen autentikasi]]  
[[241. Cara membuat AuthService dengan metode register, login, dan refresh token]]  
[[242. Cara mengimplementasikan hashing password menggunakan bcrypt dalam NestJS]]  
[[243. Cara membuat LocalStrategy menggunakan passport-local untuk login dengan email dan password]]  
[[244. Cara membuat JwtStrategy menggunakan passport-jwt untuk validasi token]]  
[[245. Cara membuat JwtRefreshStrategy untuk validasi refresh token]]  
[[246. Cara membuat AuthController dengan endpoint register, login, logout, dan refresh]]  
[[247. Cara mengimplementasikan access token dengan expiry pendek yaitu 15 menit]]  
[[248. Cara mengimplementasikan refresh token dengan expiry panjang yaitu 7 hari]]  
[[249. Cara menyimpan refresh token di database untuk invalidasi]]  
[[250. Cara mengimplementasikan token rotation yaitu refresh token baru setiap kali digunakan]]  
[[251. Cara membuat JwtAuthGuard yang memperluas AuthGuard dari Passport]]  
[[252. Cara mengimplementasikan decorator Public untuk route yang tidak perlu autentikasi]]  
[[253. Cara mengimplementasikan RolesGuard berbasis decorator Roles]]  
[[254. Cara mengimplementasikan email verification flow]]  
[[255. Cara mengimplementasikan forgot password dan reset password flow]]

---

### 4.2 File Upload dan Media Management

[[256. Cara menginstal dan mengkonfigurasi Multer dalam NestJS menggunakan @nestjs/platform-express]]  
[[257. Cara menggunakan decorator UseInterceptors dengan FileInterceptor untuk single file upload]]  
[[258. Cara menggunakan decorator UseInterceptors dengan FilesInterceptor untuk multiple files]]  
[[259. Cara menggunakan decorator UploadedFile untuk mengakses file yang diupload]]  
[[260. Cara mengkonfigurasi Multer storage untuk menyimpan file ke disk]]  
[[261. Cara mengkonfigurasi Multer storage untuk menyimpan file di memori]]  
[[262. Cara mengimplementasikan file filter untuk membatasi tipe file yang diizinkan]]  
[[263. Cara mengimplementasikan size limit untuk file upload]]  
[[264. Cara mengintegrasikan AWS S3 menggunakan AWS SDK v3 dalam NestJS]]  
[[265. Cara membuat S3Service untuk operasi upload, download, dan delete file]]  
[[266. Cara mengintegrasikan Cloudinary untuk image management dalam NestJS]]  
[[267. Cara mengimplementasikan image resizing menggunakan Sharp dalam NestJS]]  
[[268. Cara membuat MediaModule yang mengelola semua operasi file secara terpusat]]  
[[269. Cara mengimplementasikan endpoint upload yang mengembalikan URL file]]  
[[270. Cara mengimplementasikan file serving yang dilindungi autentikasi]]

---

### 4.3 Validation dan Transformation Lanjutan

[[271. Cara menggunakan decorator IsEnum untuk validasi nilai enum]]  
[[272. Cara menggunakan decorator IsDate dan IsDateString untuk validasi tanggal]]  
[[273. Cara menggunakan decorator IsArray dan ArrayMinSize untuk validasi array]]  
[[274. Cara menggunakan decorator IsOptional untuk field yang tidak wajib]]  
[[275. Cara menggunakan decorator ValidateNested dan Type untuk validasi nested object]]  
[[276. Cara menggunakan decorator ValidateIf untuk validasi kondisional]]  
[[277. Cara membuat custom decorator validasi menggunakan ValidatorConstraint]]  
[[278. Cara membuat decorator IsUnique untuk validasi keunikan data di database]]  
[[279. Cara membuat decorator IsExists untuk validasi keberadaan data di database]]  
[[280. Cara menggunakan decorator Expose dan Exclude dari class-transformer]]  
[[281. Cara menggunakan decorator Transform untuk transformasi data kustom]]  
[[282. Cara menggunakan decorator Type untuk konversi tipe otomatis]]  
[[283. Cara membuat serialization interceptor menggunakan class-transformer]]  
[[284. Cara menggunakan decorator SerializeOptions untuk konfigurasi serialisasi]]  
[[285. Cara mengimplementasikan DTO inheritance menggunakan PartialType, PickType, dan OmitType]]

---

### 4.4 Project 3 - API E-Commerce Lengkap

[[286. Merencanakan arsitektur API e-commerce dengan semua module yang diperlukan]]  
[[287. Mendefinisikan schema Prisma untuk User, Product, Category, Order, OrderItem, Cart, Review, dan Address]]  
[[288. Membangun AuthModule dengan register, login, refresh token, dan email verification]]  
[[289. Membangun UserModule dengan profile management dan address book]]  
[[290. Membangun CategoryModule dengan nested categories atau tree structure]]  
[[291. Membangun ProductModule dengan CRUD, image upload, dan inventory management]]  
[[292. Mengimplementasikan product variants yaitu size dan color dengan harga berbeda]]  
[[293. Mengimplementasikan product search dengan full-text search]]  
[[294. Mengimplementasikan product filtering dan sorting yang komprehensif]]  
[[295. Membangun CartModule dengan add, remove, update quantity, dan clear cart]]  
[[296. Mengimplementasikan cart persistence yaitu guest cart dan user cart yang bisa di-merge]]  
[[297. Membangun OrderModule dengan order creation dari cart]]  
[[298. Mengimplementasikan order status flow yaitu pending, processing, shipped, delivered, dan cancelled]]  
[[299. Membangun ReviewModule dengan rating dan komentar per produk]]  
[[300. Mengimplementasikan coupon dan discount management]]

---

### 4.5 Swagger dan API Documentation

[[301. Memahami kegunaan Swagger untuk dokumentasi API yang interaktif]]  
[[302. Cara menginstal package @nestjs/swagger dan swagger-ui-express]]  
[[303. Cara mengkonfigurasi SwaggerModule dan DocumentBuilder di main.ts]]  
[[304. Cara menggunakan decorator ApiTags untuk mengelompokkan endpoint]]  
[[305. Cara menggunakan decorator ApiOperation untuk deskripsi endpoint]]  
[[306. Cara menggunakan decorator ApiResponse untuk mendokumentasikan response]]  
[[307. Cara menggunakan decorator ApiBody untuk mendokumentasikan request body]]  
[[308. Cara menggunakan decorator ApiParam untuk mendokumentasikan route parameter]]  
[[309. Cara menggunakan decorator ApiQuery untuk mendokumentasikan query parameter]]  
[[310. Cara menggunakan decorator ApiBearerAuth untuk endpoint yang butuh autentikasi]]  
[[311. Cara menggunakan decorator ApiProperty dan ApiPropertyOptional pada DTO]]  
[[312. Cara menggunakan decorator ApiExtraModels untuk mendokumentasikan model yang tidak langsung digunakan]]  
[[313. Cara membuat dokumentasi yang terpisah untuk API internal dan publik]]  
[[314. Cara mengekspor dokumentasi Swagger sebagai file JSON atau YAML]]  
[[315. Cara menggunakan Swagger UI untuk testing API secara interaktif]]

---

## 🔴 LEVEL 5: PROJECT UPPER-INTERMEDIATE (Fitur Canggih)

### 5.1 Caching dengan Redis

[[316. Memahami konsep caching dan mengapa Redis menjadi pilihan utama dalam NestJS]]  
[[317. Cara menginstal package @nestjs/cache-manager dan cache-manager-redis-store]]  
[[318. Cara mengkonfigurasi CacheModule menggunakan Redis store]]  
[[319. Cara menggunakan decorator CacheInterceptor untuk caching response otomatis]]  
[[320. Cara menggunakan decorator CacheTTL untuk mengatur durasi cache per route]]  
[[321. Cara menggunakan decorator CacheKey untuk mengatur key cache per route]]  
[[322. Cara menginjeksi CACHE_MANAGER untuk operasi cache manual]]  
[[323. Cara menggunakan cache.get dan cache.set dan cache.del secara manual]]  
[[324. Cara mengimplementasikan cache invalidation saat data diupdate]]  
[[325. Cara mengimplementasikan cache-aside pattern dalam service]]  
[[326. Cara menggunakan Redis untuk menyimpan session dan blacklist token]]  
[[327. Cara mengimplementasikan distributed lock menggunakan Redis dalam NestJS]]  
[[328. Cara menggunakan ioredis langsung untuk operasi Redis yang lebih kompleks]]  
[[329. Cara membuat CacheService yang reusable di berbagai module]]  
[[330. Cara mengimplementasikan cache warming strategy saat aplikasi startup]]

---

### 5.2 Queue dan Background Jobs

[[331. Memahami konsep message queue dan kegunaannya dalam aplikasi NestJS]]  
[[332. Cara menginstal package @nestjs/bull dan bull untuk queue management]]  
[[333. Cara mengkonfigurasi BullModule dengan koneksi Redis]]  
[[334. Cara mendaftarkan queue menggunakan BullModule.registerQueue]]  
[[335. Cara membuat processor menggunakan decorator Processor dan Process]]  
[[336. Cara mendispatch job ke queue menggunakan InjectQueue dan metode add]]  
[[337. Cara mengimplementasikan job dengan prioritas menggunakan opsi priority]]  
[[338. Cara mengimplementasikan delayed job menggunakan opsi delay]]  
[[339. Cara mengimplementasikan job retry menggunakan opsi attempts dan backoff]]  
[[340. Cara mengimplementasikan job yang berjalan berulang menggunakan opsi repeat]]  
[[341. Cara menangani job events yaitu active, completed, failed, dan progress]]  
[[342. Cara menggunakan Bull Board untuk monitoring queue secara visual]]  
[[343. Cara mengimplementasikan email sending queue untuk pengiriman email asynchronous]]  
[[344. Cara mengimplementasikan image processing queue untuk resize gambar]]  
[[345. Cara mengimplementasikan notification queue untuk pengiriman notifikasi]]  
[[346. Cara menggunakan BullMQ sebagai versi terbaru Bull dalam NestJS]]  
[[347. Cara membuat flow job yaitu job yang saling bergantung]]  
[[348. Cara mengimplementasikan worker yang dapat diskalakan secara horizontal]]  
[[349. Cara menangani dead letter queue untuk job yang gagal]]  
[[350. Cara mengintegrasikan queue monitoring ke dalam dashboard admin]]

---

### 5.3 WebSocket dan Real-time

[[351. Memahami konsep WebSocket dan kegunaannya dalam aplikasi real-time NestJS]]  
[[352. Cara menginstal package @nestjs/websockets dan @nestjs/platform-socket.io]]  
[[353. Cara membuat Gateway menggunakan decorator WebSocketGateway]]  
[[354. Cara menggunakan decorator WebSocketServer untuk mengakses server instance]]  
[[355. Cara menggunakan decorator SubscribeMessage untuk menangani pesan dari client]]  
[[356. Cara menggunakan decorator MessageBody untuk mengakses payload pesan]]  
[[357. Cara menggunakan decorator ConnectedSocket untuk mengakses socket client]]  
[[358. Cara mengirim pesan ke client menggunakan metode emit pada socket]]  
[[359. Cara broadcast pesan ke semua client menggunakan server.emit]]  
[[360. Cara menggunakan rooms dalam Socket.io untuk grouping client]]  
[[361. Cara mengimplementasikan autentikasi pada WebSocket connection]]  
[[362. Cara menggunakan JWT untuk memverifikasi WebSocket connection]]  
[[363. Cara mengimplementasikan namespace dalam Socket.io dengan NestJS]]  
[[364. Cara menggunakan adapter Redis untuk Socket.io scaling horizontal]]  
[[365. Cara mengintegrasikan WebSocket dengan sistem notifikasi real-time]]

---

### 5.4 Email dan Notifikasi

[[366. Cara menginstal dan mengkonfigurasi @nestjs-modules/mailer untuk email]]  
[[367. Cara mengkonfigurasi MailerModule dengan SMTP transport]]  
[[368. Cara menggunakan Handlebars atau EJS sebagai template engine untuk email]]  
[[369. Cara membuat email template yang responsif untuk berbagai kebutuhan]]  
[[370. Cara menginjeksi MailerService dan menggunakan metode sendMail]]  
[[371. Cara membuat EmailService yang mewrap MailerService dengan metode spesifik]]  
[[372. Cara mengimplementasikan email verifikasi akun dengan token]]  
[[373. Cara mengimplementasikan email reset password dengan token]]  
[[374. Cara mengimplementasikan email notifikasi order kepada pembeli]]  
[[375. Cara menggunakan Nodemailer transport untuk layanan email seperti SendGrid dan Mailgun]]  
[[376. Cara mengimplementasikan email queue untuk pengiriman email asynchronous]]  
[[377. Cara mengimplementasikan retry mechanism untuk email yang gagal terkirim]]  
[[378. Cara membuat in-app notification system menggunakan database]]  
[[379. Cara membuat push notification menggunakan Firebase Cloud Messaging atau FCM]]  
[[380. Cara mengintegrasikan Twilio untuk SMS notification dalam NestJS]]

---

### 5.5 Project 4 - Sistem Manajemen Konten atau CMS API

[[381. Merencanakan arsitektur CMS API dengan multi-tenant dan role-based access]]  
[[382. Mendefinisikan schema untuk User, Role, Permission, Content, Category, Tag, Media, dan Settings]]  
[[383. Mengimplementasikan sistem permission yang granular menggunakan CASL]]  
[[384. Cara menginstal dan mengkonfigurasi CASL dalam NestJS]]  
[[385. Cara mendefinisikan abilities menggunakan CASL ability factory]]  
[[386. Cara membuat PoliciesGuard yang menggunakan CASL untuk pengecekan izin]]  
[[387. Membangun ContentModule dengan versioning konten dan draft system]]  
[[388. Mengimplementasikan workflow publish yaitu draft, review, scheduled, dan published]]  
[[389. Membangun MediaModule dengan upload ke S3 dan image optimization]]  
[[390. Mengimplementasikan SEO metadata management pada setiap konten]]  
[[391. Membangun sistem tag dan kategorisasi yang fleksibel]]  
[[392. Mengimplementasikan full-text search menggunakan Elasticsearch atau PostgreSQL FTS]]  
[[393. Membangun webhook system untuk notifikasi event ke sistem eksternal]]  
[[394. Mengimplementasikan API key management untuk akses programatik]]  
[[395. Mengimplementasikan audit log untuk setiap perubahan data]]

---

## ⚫ LEVEL 6: PROJECT ADVANCED (Arsitektur Enterprise)

### 6.1 Testing Komprehensif dalam NestJS

[[396. Memahami strategi testing dalam NestJS yaitu unit, integration, dan end-to-end]]  
[[397. Cara menggunakan Jest sebagai testing framework default NestJS]]  
[[398. Cara membuat unit test untuk service menggunakan Jest mock]]  
[[399. Cara membuat mock untuk dependency yang diinjeksi dalam unit test]]  
[[400. Cara menggunakan Test.createTestingModule untuk membuat testing module]]  
[[401. Cara menggunakan jest.spyOn untuk memantau pemanggilan metode]]  
[[402. Cara membuat unit test untuk guard, pipe, interceptor, dan filter]]  
[[403. Cara membuat integration test untuk controller dengan database nyata]]  
[[404. Cara menggunakan TestingModule untuk integration test]]  
[[405. Cara membuat end-to-end test menggunakan Supertest bersama NestJS]]  
[[406. Cara mengkonfigurasi e2e test dalam file jest-e2e.json]]  
[[407. Cara menggunakan database testing dengan Prisma dan database terpisah]]  
[[408. Cara mengimplementasikan database seeding khusus untuk testing]]  
[[409. Cara mengukur code coverage dan menetapkan threshold minimum]]  
[[410. Cara mengintegrasikan testing ke dalam GitHub Actions CI pipeline]]  
[[411. Cara menggunakan Testcontainers untuk menjalankan database nyata dalam test]]  
[[412. Cara membuat factory functions untuk membuat test data yang konsisten]]  
[[413. Cara menggunakan faker-js untuk generate test data yang realistis]]  
[[414. Cara mengimplementasikan contract testing menggunakan Pact]]  
[[415. Cara membuat testing strategy document untuk project tim]]

---

### 6.2 Microservices dengan NestJS

[[416. Memahami arsitektur microservices dan kapan menggunakannya dengan NestJS]]  
[[417. Memahami transport layer dalam NestJS microservices yaitu TCP, Redis, RabbitMQ, Kafka, dll]]  
[[418. Cara membuat microservice menggunakan NestJS dengan transport TCP]]  
[[419. Cara menggunakan ClientProxy untuk berkomunikasi dengan microservice]]  
[[420. Cara menggunakan decorator MessagePattern untuk menangani pesan di microservice]]  
[[421. Cara menggunakan decorator EventPattern untuk menangani event di microservice]]  
[[422. Cara mengimplementasikan request-response pattern menggunakan send]]  
[[423. Cara mengimplementasikan event-based pattern menggunakan emit]]  
[[424. Cara mengintegrasikan RabbitMQ sebagai message broker dalam NestJS microservices]]  
[[425. Cara mengkonfigurasi RmqOptions untuk koneksi RabbitMQ]]  
[[426. Cara mengintegrasikan Apache Kafka dalam NestJS microservices]]  
[[427. Cara mengimplementasikan saga pattern untuk distributed transactions]]  
[[428. Cara mengimplementasikan API Gateway menggunakan NestJS]]  
[[429. Cara mengimplementasikan service discovery sederhana]]  
[[430. Cara mengimplementasikan circuit breaker pattern dalam NestJS microservices]]

---

### 6.3 GraphQL dengan NestJS

[[431. Memahami konsep GraphQL dan kegunaannya sebagai alternatif REST dalam NestJS]]  
[[432. Cara menginstal package @nestjs/graphql dan @nestjs/apollo]]  
[[433. Cara mengkonfigurasi GraphQLModule menggunakan code-first approach]]  
[[434. Cara membuat Object Type menggunakan decorator ObjectType dan Field]]  
[[435. Cara membuat Query resolver menggunakan decorator Query dalam Resolver]]  
[[436. Cara membuat Mutation resolver menggunakan decorator Mutation]]  
[[437. Cara membuat Subscription resolver menggunakan decorator Subscription]]  
[[438. Cara membuat Input Type menggunakan decorator InputType untuk mutation args]]  
[[439. Cara menggunakan decorator Args untuk mengakses argument dalam resolver]]  
[[440. Cara mengimplementasikan pagination dalam GraphQL menggunakan cursor-based approach]]  
[[441. Cara mengimplementasikan DataLoader untuk mengatasi N plus 1 problem]]  
[[442. Cara menginstal dan menggunakan NestJS DataLoader package]]  
[[443. Cara mengimplementasikan authentication dan authorization dalam GraphQL resolver]]  
[[444. Cara menggunakan decorator Context untuk mengakses request context]]  
[[445. Cara mengimplementasikan file upload dalam GraphQL menggunakan graphql-upload]]  
[[446. Cara mengintegrasikan GraphQL dengan Prisma dalam NestJS]]  
[[447. Cara menggunakan Pothos atau TypeGraphQL sebagai alternatif code-first]]  
[[448. Cara mendokumentasikan GraphQL schema menggunakan GraphQL Playground]]  
[[449. Cara melakukan testing GraphQL endpoint menggunakan Supertest]]  
[[450. Cara mengimplementasikan GraphQL federation untuk microservices]]

---

### 6.4 Performance Optimization dalam NestJS

[[451. Cara menggunakan Fastify sebagai HTTP adapter pengganti Express untuk performa lebih baik]]  
[[452. Cara mengkonfigurasi NestJS dengan Fastify adapter]]  
[[453. Cara mengimplementasikan response compression menggunakan middleware]]  
[[454. Cara mengoptimasi database query menggunakan Prisma select dan include yang efisien]]  
[[455. Cara mengimplementasikan database connection pooling yang optimal]]  
[[456. Cara menggunakan caching berlapis yaitu in-memory dan Redis]]  
[[457. Cara mengimplementasikan HTTP caching menggunakan cache-control headers]]  
[[458. Cara menggunakan lazy loading untuk module yang tidak selalu diperlukan]]  
[[459. Cara mengoptimasi startup time NestJS application]]  
[[460. Cara menggunakan cluster mode dengan PM2 untuk memanfaatkan semua CPU core]]  
[[461. Cara mengimplementasikan rate limiting yang efektif menggunakan @nestjs/throttler]]  
[[462. Cara menggunakan @nestjs/throttler dengan storage Redis untuk distributed rate limiting]]  
[[463. Cara mengidentifikasi memory leak dalam aplikasi NestJS]]  
[[464. Cara menggunakan Node.js --inspect untuk profiling aplikasi NestJS]]  
[[465. Cara menggunakan Clinic.js untuk diagnosa masalah performa]]

---

### 6.5 Project 5 - Platform Manajemen Keuangan API

[[466. Merencanakan arsitektur platform keuangan dengan keamanan dan audit yang ketat]]  
[[467. Mendefinisikan schema untuk Account, Transaction, Category, Budget, Report, dan AuditLog]]  
[[468. Mengimplementasikan autentikasi yang sangat aman dengan 2FA menggunakan speakeasy]]  
[[469. Mengimplementasikan enkripsi data sensitif menggunakan AES dalam NestJS]]  
[[470. Membangun TransactionModule dengan double-entry bookkeeping]]  
[[471. Mengimplementasikan transaction idempotency untuk mencegah transaksi duplikat]]  
[[472. Mengimplementasikan database transactions untuk menjamin konsistensi data keuangan]]  
[[473. Membangun BudgetModule dengan tracking dan alert saat mendekati limit]]  
[[474. Membangun ReportModule dengan aggregasi data keuangan yang kompleks]]  
[[475. Mengimplementasikan export laporan ke format PDF menggunakan puppeteer atau pdfmake]]  
[[476. Mengimplementasikan export laporan ke format Excel menggunakan exceljs]]  
[[477. Membangun AuditLogModule yang mencatat setiap perubahan data secara otomatis]]  
[[478. Mengimplementasikan webhook untuk notifikasi transaksi ke sistem eksternal]]  
[[479. Mengimplementasikan rate limiting yang ketat pada endpoint sensitif]]  
[[480. Mengimplementasikan IP whitelist untuk akses API yang lebih aman]]

---

## 🟣 LEVEL 7: MASTERY DAN DEPLOYMENT

### 7.1 Project 6 - Platform SaaS Multi-Tenant

[[481. Merencanakan arsitektur SaaS multi-tenant dengan isolasi data yang baik]]  
[[482. Memahami strategi multi-tenancy yaitu shared database, schema per tenant, dan database per tenant]]  
[[483. Mengimplementasikan tenant identification menggunakan subdomain atau header]]  
[[484. Membuat TenantMiddleware untuk menentukan tenant dari setiap request]]  
[[485. Mengimplementasikan tenant context menggunakan AsyncLocalStorage]]  
[[486. Membuat PrismaService yang dinamis berdasarkan tenant]]  
[[487. Mengimplementasikan tenant provisioning yaitu pembuatan tenant baru dengan schema database]]  
[[488. Mengimplementasikan tenant management API untuk super admin]]  
[[489. Membangun subscription dan billing system menggunakan Stripe]]  
[[490. Cara menginstal dan mengkonfigurasi Stripe dalam NestJS]]  
[[491. Mengimplementasikan Stripe webhook handling untuk event subscription]]  
[[492. Mengimplementasikan usage-based billing dengan Stripe Metering]]  
[[493. Membangun feature flag system untuk mengontrol fitur berdasarkan plan]]  
[[494. Mengimplementasikan tenant-aware caching menggunakan Redis]]  
[[495. Mengimplementasikan tenant-aware rate limiting]]

---

### 7.2 DevOps dan Deployment NestJS

[[496. Cara membuat Dockerfile yang optimal untuk aplikasi NestJS]]  
[[497. Cara menggunakan multi-stage build untuk menghasilkan Docker image yang lebih kecil]]  
[[498. Cara mengkonfigurasi Docker Compose untuk NestJS dengan PostgreSQL, Redis, dan Nginx]]  
[[499. Cara membuat docker-compose.yml untuk environment development yang lengkap]]  
[[500. Cara mengkonfigurasi Nginx sebagai reverse proxy untuk NestJS]]  
[[501. Cara mengkonfigurasi SSL menggunakan Let's Encrypt dan Certbot]]  
[[502. Cara men-deploy NestJS ke VPS Ubuntu menggunakan Docker]]  
[[503. Cara menggunakan PM2 untuk process management NestJS di production]]  
[[504. Cara mengkonfigurasi PM2 ecosystem file untuk NestJS]]  
[[505. Cara mengimplementasikan health check endpoint menggunakan @nestjs/terminus]]  
[[506. Cara mengkonfigurasi readiness dan liveness probe untuk Kubernetes]]  
[[507. Cara membuat GitHub Actions workflow untuk CI atau CD pipeline NestJS]]  
[[508. Cara mengimplementasikan automated testing dalam CI pipeline]]  
[[509. Cara mengimplementasikan zero-downtime deployment menggunakan rolling update]]  
[[510. Cara men-deploy NestJS ke Kubernetes menggunakan Helm chart]]

---

### 7.3 Monitoring dan Observability

[[511. Cara mengimplementasikan structured logging menggunakan Winston dalam NestJS]]  
[[512. Cara membuat custom logger yang mengimplementasikan LoggerService NestJS]]  
[[513. Cara mengkonfigurasi log levels yang berbeda untuk setiap environment]]  
[[514. Cara menggunakan Pino sebagai alternatif logger yang lebih cepat]]  
[[515. Cara mengintegrasikan logging dengan layanan seperti Datadog atau Papertrail]]  
[[516. Cara mengimplementasikan distributed tracing menggunakan OpenTelemetry]]  
[[517. Cara mengkonfigurasi OpenTelemetry SDK dalam NestJS]]  
[[518. Cara mengintegrasikan trace dengan Jaeger atau Zipkin]]  
[[519. Cara mengimplementasikan metrics menggunakan Prometheus dan prom-client]]  
[[520. Cara membuat custom metrics untuk business-specific monitoring]]  
[[521. Cara mengkonfigurasi Grafana dashboard untuk visualisasi metrics]]  
[[522. Cara menggunakan @nestjs/terminus untuk health checks yang komprehensif]]  
[[523. Cara membuat custom health indicator untuk pengecekan dependency]]  
[[524. Cara mengimplementasikan alerting menggunakan PagerDuty atau OpsGenie]]  
[[525. Cara melakukan incident response berdasarkan monitoring alerts]]

---

### 7.4 Keamanan Aplikasi NestJS

[[526. Memahami OWASP Top 10 dalam konteks API NestJS]]  
[[527. Cara mengimplementasikan input validation yang komprehensif untuk mencegah injection]]  
[[528. Cara mengimplementasikan output encoding untuk mencegah XSS dalam response]]  
[[529. Cara mengkonfigurasi CORS dengan benar untuk membatasi origin yang diizinkan]]  
[[530. Cara mengimplementasikan rate limiting per endpoint menggunakan @nestjs/throttler]]  
[[531. Cara mengimplementasikan request size limiting untuk mencegah payload terlalu besar]]  
[[532. Cara menggunakan helmet untuk mengatur security headers yang tepat]]  
[[533. Cara mengimplementasikan SQL injection prevention menggunakan parameterized query]]  
[[534. Cara mengimplementasikan NoSQL injection prevention dalam MongoDB]]  
[[535. Cara mengaudit dependency menggunakan npm audit dan Snyk]]  
[[536. Cara mengimplementasikan secrets rotation untuk database credentials]]  
[[537. Cara menggunakan environment-specific configuration untuk keamanan]]  
[[538. Cara mengimplementasikan API versioning untuk backward compatibility]]  
[[539. Cara melakukan penetration testing dasar pada API NestJS]]  
[[540. Cara membuat security checklist untuk deployment NestJS ke production]]

---

### 7.5 Pengembangan Berkelanjutan dan Ekosistem

[[541. Cara mengikuti best practices dan style guide resmi NestJS]]  
[[542. Cara menggunakan NestJS CLI plugins untuk meningkatkan produktivitas]]  
[[543. Cara berkontribusi ke ekosistem NestJS dengan membuat custom module]]  
[[544. Cara menggunakan @golevelup/nestjs-testing untuk testing yang lebih mudah]]  
[[545. Cara menggunakan nestjs-pino untuk structured logging berbasis Pino]]  
[[546. Cara menggunakan nestjs-cls yaitu Continuation Local Storage untuk request context]]  
[[547. Cara mengintegrasikan NestJS dengan tRPC untuk type-safe API]]  
[[548. Cara menggunakan Zod bersama NestJS sebagai alternatif class-validator]]  
[[549. Cara membangun dan mempublish NestJS library ke npm registry]]  
[[550. Cara selalu mengikuti perkembangan NestJS melalui dokumentasi resmi dan blog]]

---

## 📋 PETA PERKEMBANGAN (PROGRESS MAP)

|Level|Cakupan|Estimasi Waktu|
|---|---|---|
|Level 1 Fondasi NestJS|Poin 1 hingga 70|3 hingga 5 minggu|
|Level 2 Fondasi Project|Poin 71 hingga 160|4 hingga 6 minggu|
|Level 3 Project Dasar|Poin 161 hingga 235|5 hingga 8 minggu|
|Level 4 Project Intermediate|Poin 236 hingga 315|8 hingga 12 minggu|
|Level 5 Project Upper-Intermediate|Poin 316 hingga 395|10 hingga 14 minggu|
|Level 6 Project Advanced|Poin 396 hingga 480|12 hingga 18 minggu|
|Level 7 Mastery|Poin 481 hingga 550|10 hingga 16 minggu|

---

## 🗺️ DAFTAR PROJECT DALAM KURIKULUM INI

|No|Nama Project|Level|Teknologi Utama|
|---|---|---|---|
|1|REST API CRUD Sederhana|Dasar|NestJS dan ValidationPipe dan in-memory|
|2|Blog API dengan PostgreSQL|Dasar|NestJS dan Prisma dan PostgreSQL|
|3|API E-Commerce Lengkap|Intermediate|NestJS dan Prisma dan JWT dan S3|
|4|Sistem Manajemen Konten|Upper-Intermediate|NestJS dan CASL dan Elasticsearch dan Bull|
|5|Platform Manajemen Keuangan|Advanced|NestJS dan enkripsi dan audit log dan PDF|
|6|Platform SaaS Multi-Tenant|Mastery|NestJS dan multi-tenant dan Stripe dan K8s|

---

## 🎯 TIPS PENGGUNAAN KURIKULUM INI

|Tips|Penjelasan|
|---|---|
|Kuasai TypeScript dahulu|NestJS sangat bergantung pada TypeScript dan decorator sehingga fondasi TypeScript wajib kuat|
|Pahami Dependency Injection|DI adalah jantung NestJS dan harus benar-benar dipahami sebelum project besar|
|Ikuti konvensi NestJS|NestJS memiliki konvensi yang kuat sehingga mengikutinya membuat kode lebih mudah dipahami|
|Tulis test sejak awal|Testing bukan opsional dalam NestJS karena framework ini dirancang untuk testability|
|Baca source code NestJS|Membaca source code NestJS di GitHub mengajarkan pola yang tidak ada di dokumentasi|
|Gunakan Swagger|Dokumentasikan setiap endpoint menggunakan Swagger dari awal pengembangan|
|Konsistensi belajar|Belajar 1 hingga 2 jam setiap hari jauh lebih efektif daripada belajar intensif sesekali|

---

> _"NestJS provides an out-of-the-box application architecture which allows developers and teams to create highly testable, scalable, loosely coupled, and easily maintainable applications."_  
> — Kamil Mysliwiec

> _"The architecture of NestJS draws heavily from Angular, bringing the same structured and scalable approach to the backend world."_

**Kurikulum ini mencakup 550 poin belajar dan 6 project nyata berskala enterprise yang dirancang membawa Anda dari nol hingga mampu membangun backend profesional menggunakan NestJS. Estimasi total waktu belajar adalah 18 hingga 30 bulan dengan latihan konsisten setiap hari.**