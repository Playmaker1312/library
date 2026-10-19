# 📚 Perpustakaan Digital: Kurikulum Game Dev dengan Godot Engine

**Panduan Belajar dari Fundamental hingga Advanced**

---

## 🟢 LEVEL 1: ABSOLUTE BEGINNER (Dasar Mutlak)

### 1.1 Pengenalan Dunia Game Development

[[1. Apa itu game development dan bagaimana industri game bekerja secara umum]]

[[2. Perbedaan antara game engine, game framework, dan game library]]

[[3. Mengenal Godot Engine sejarah perkembangan dan keunggulannya dibanding engine lain]]

[[4. Perbedaan Godot 3 dan Godot 4 serta alasan memilih versi yang tepat]]

[[5. Memahami jenis-jenis game berdasarkan genre dan platform target]]

[[6. Mengenal peran-peran dalam tim game development seperti programmer artist designer dan sound designer]]

[[7. Konsep dasar game loop yaitu input update dan render]]

[[8. Memahami perbedaan game 2D dan 3D serta kapan menggunakan masing-masing]]

---

### 1.2 Instalasi dan Pengenalan Antarmuka Godot

[[9. Cara mengunduh dan menginstal Godot Engine dari situs resmi]]

[[10. Mengenal tampilan antarmuka Godot yaitu Viewport Scene Panel Inspector FileSystem dan Output]]

[[11. Membuat project baru dan memahami struktur folder project Godot]]

[[12. Memahami konsep Scene dan Node sebagai fondasi utama Godot]]

[[13. Mengenal jenis-jenis Node dasar seperti Node2D Sprite2D CollisionShape2D dan Label]]

[[14. Cara menyimpan scene membuka scene dan mengatur scene utama project]]

[[15. Menggunakan fitur pencarian Node dan dokumentasi bawaan Godot]]

[[16. Memahami panel FileSystem untuk mengatur aset dan file project]]

---

### 1.3 Pengenalan GDScript (Bahasa Pemrograman Godot)

[[17. Apa itu GDScript dan mengapa GDScript dirancang khusus untuk Godot]]

[[18. Struktur dasar file GDScript yaitu extends variabel dan fungsi]]

[[19. Tipe data dasar dalam GDScript yaitu int float String bool dan null]]

[[20. Cara mendeklarasikan variabel menggunakan var dan const]]

[[21. Operator aritmatika yaitu tambah kurang kali bagi dan modulo dalam GDScript]]

[[22. Operator perbandingan yaitu sama dengan tidak sama lebih besar dan lebih kecil]]

[[23. Operator logika yaitu and or dan not dalam GDScript]]

[[24. Struktur kondisional if elif dan else dalam GDScript]]

[[25. Perulangan menggunakan for dan while dalam GDScript]]

[[26. Konsep fungsi dalam GDScript cara mendefinisikan dan memanggil fungsi]]

[[27. Fungsi bawaan Godot yang wajib diketahui yaitu ready process dan input]]

[[28. Cara mencetak teks ke Output menggunakan print untuk debugging]]

---

### 1.4 Node dan Scene System

[[29. Memahami hierarki Node dan hubungan Parent-Child dalam scene]]

[[30. Cara menambahkan Node baru ke dalam scene melalui antarmuka Godot]]

[[31. Cara mengakses Node lain dari script menggunakan dollar sign dan get node]]

[[32. Memahami konsep Scene Instancing yaitu menggunakan scene sebagai objek yang dapat dipakai ulang]]

[[33. Cara membuat dan menyimpan scene terpisah lalu menginstansiasinya di scene lain]]

[[34. Memahami Node Root dan bagaimana scene tree bekerja secara keseluruhan]]

[[35. Menggunakan onready var untuk mereferensikan Node saat scene siap]]

[[36. Memahami perbedaan Node biasa versus Node yang memiliki komponen fisika]]

---

### 1.5 Membuat Game 2D Pertama (Project Hello World Game)

[[37. Membuat scene player sederhana menggunakan CharacterBody2D dan Sprite2D]]

[[38. Menambahkan CollisionShape2D pada player untuk deteksi tabrakan]]

[[39. Menulis script pergerakan player dasar menggunakan input keyboard]]

[[40. Memahami fungsi move and slide untuk pergerakan karakter 2D]]

[[41. Membuat scene level sederhana menggunakan TileMap atau StaticBody2D]]

[[42. Menambahkan kamera menggunakan Camera2D dan mengatur batas pergerakan kamera]]

[[43. Membuat objek musuh sederhana dengan pergerakan dasar]]

[[44. Menambahkan UI sederhana menggunakan Label untuk menampilkan skor]]

[[45. Memahami cara export project ke format yang bisa dimainkan]]

---

## 🔵 LEVEL 2: ELEMENTARY (Pemula Terstruktur)

### 2.1 GDScript Lebih Dalam

[[46. Array dalam GDScript cara membuat mengakses dan memanipulasi data array]]

[[47. Dictionary dalam GDScript cara membuat mengakses dan mengiterasi key-value]]

[[48. Tipe data Vector2 dan Vector3 serta operasi matematika dasarnya]]

[[49. Menggunakan match statement sebagai alternatif if-elif yang lebih bersih]]

[[50. Konsep Class dalam GDScript mendefinisikan class inner class dan inheritance dasar]]

[[51. Memahami static variable dan static function dalam GDScript]]

[[52. Lambda function dan cara menggunakannya dalam GDScript 4]]

[[53. Menggunakan type hints untuk menulis kode GDScript yang lebih aman dan mudah dibaca]]

[[54. Memahami Enumerasi atau Enum untuk mendefinisikan kumpulan konstanta]]

[[55. Error handling dasar menggunakan assert dan push error dalam GDScript]]

---

### 2.2 Signal dan Event System

[[56. Apa itu Signal dalam Godot dan mengapa Signal penting untuk komunikasi antar Node]]

[[57. Cara membuat Signal kustom menggunakan keyword signal dalam GDScript]]

[[58. Cara menghubungkan Signal ke fungsi menggunakan panel editor Godot]]

[[59. Cara menghubungkan Signal menggunakan kode dengan metode connect]]

[[60. Cara memancarkan Signal menggunakan keyword emit]]

[[61. Menggunakan Signal bawaan Node seperti body entered area exited dan timeout]]

[[62. Memahami pola Observer melalui implementasi Signal dalam Godot]]

[[63. Menghindari kesalahan umum dalam penggunaan Signal seperti double connection]]

---

### 2.3 Input System

[[64. Memahami Input Map dan cara menambahkan action kustom di Project Settings]]

[[65. Perbedaan antara Input is action pressed is action just pressed dan is action just released]]

[[66. Menangani input keyboard mouse dan gamepad dalam GDScript]]

[[67. Membaca posisi mouse menggunakan get global mouse position]]

[[68. Memahami input event melalui fungsi unhandled input dan gui input]]

[[69. Membuat sistem input yang mendukung keyboard dan gamepad secara bersamaan]]

[[70. Menggunakan InputEventMouseButton dan InputEventKey untuk deteksi input spesifik]]

---

### 2.4 Physics 2D

[[71. Perbedaan antara StaticBody2D RigidBody2D dan CharacterBody2D]]

[[72. Memahami Layer dan Mask pada collision system Godot]]

[[73. Menggunakan Area2D untuk deteksi overlap dan trigger zone]]

[[74. Memahami gravity scale dan physics material pada RigidBody2D]]

[[75. Implementasi lompat pada karakter platformer menggunakan is on floor]]

[[76. Menggunakan RayCast2D untuk deteksi terrain dan line of sight]]

[[77. Memahami KinematicCollision2D untuk mendapatkan informasi detail saat tabrakan]]

[[78. Mengatur physics properties seperti linear velocity angular velocity dan damping]]

---

### 2.5 Animasi Dasar

[[79. Mengenal AnimationPlayer dan cara membuat animasi keyframe di Godot]]

[[80. Membuat animasi gerak berjalan berlari dan idle menggunakan AnimationPlayer]]

[[81. Menggunakan AnimatedSprite2D untuk animasi berbasis sprite sheet]]

[[82. Mengontrol animasi dari script menggunakan play stop dan is playing]]

[[83. Memahami AnimationTree untuk blending animasi yang lebih kompleks]]

[[84. Menggunakan Tween untuk animasi properti secara programatik]]

[[85. Membuat animasi UI seperti fade in fade out dan slide menggunakan Tween]]

---

### 2.6 Audio Dasar

[[86. Mengimpor file audio ke Godot dan memahami format yang didukung]]

[[87. Menggunakan AudioStreamPlayer untuk memutar musik latar]]

[[88. Menggunakan AudioStreamPlayer2D untuk efek suara yang terikat posisi]]

[[89. Mengatur volume pitch dan bus pada AudioStreamPlayer]]

[[90. Membuat Audio Bus Layout untuk mixing dan efek audio]]

[[91. Cara memutar menghentikan dan mengulang audio dari GDScript]]

---

### 2.7 Project Mini: Platformer 2D Sederhana

[[92. Merancang level platformer sederhana menggunakan TileMap]]

[[93. Mengimplementasikan sistem nyawa player dengan UI yang menampilkan health bar]]

[[94. Membuat musuh dengan AI sederhana yaitu patrol dan mengikuti player]]

[[95. Membuat sistem pengumpulan koin dan perhitungan skor]]

[[96. Implementasi game over screen dan win screen dengan navigasi antar scene]]

[[97. Menambahkan efek suara dan musik latar ke dalam game platformer]]

[[98. Melakukan playtesting dasar dan memperbaiki bug sederhana]]

---

## 🟡 LEVEL 3: PRE-INTERMEDIATE (Menengah Awal)

### 3.1 Arsitektur Game yang Baik

[[99. Memahami prinsip Single Responsibility yaitu satu node satu tanggung jawab]]

[[100. Menggunakan Autoload atau Singleton untuk data dan sistem global]]

[[101. Pola desain Game Manager untuk mengontrol state permainan secara keseluruhan]]

[[102. Memisahkan logic gameplay dari logic presentasi atau tampilan]]

[[103. Memahami konsep Scene sebagai Prefab dan manfaatnya untuk produktivitas]]

[[104. Menggunakan Resource untuk menyimpan data yang dapat digunakan ulang]]

[[105. Membuat Custom Resource untuk data item karakter atau level]]

[[106. Prinsip komposisi versus inheritance dalam desain Node Godot]]

---

### 3.2 TileMap dan Level Design

[[107. Memahami TileSet dan cara membuat tile dari texture atlas]]

[[108. Menggunakan TileMap layer untuk memisahkan foreground background dan collision]]

[[109. Menambahkan collision dan navigation polygon pada tile secara otomatis]]

[[110. Menggunakan Custom Data pada tile untuk informasi tambahan seperti damage atau speed modifier]]

[[111. Teknik level design dasar yaitu readability flow dan challenge curve]]

[[112. Menggunakan pattern dan terrain system pada TileMap Godot 4]]

[[113. Membuat level editor sederhana menggunakan tool script]]

---

### 3.3 Sistem Kamera Lanjutan

[[114. Mengatur Camera2D limits untuk mencegah kamera keluar dari batas level]]

[[115. Membuat smooth camera follow menggunakan position smoothing]]

[[116. Implementasi camera shake menggunakan noise atau random offset]]

[[117. Membuat parallax background menggunakan ParallaxBackground dan ParallaxLayer]]

[[118. Sistem multi-kamera untuk cutscene atau split-screen]]

[[119. Menggunakan RemoteTransform2D untuk mengontrol kamera dari node lain]]

---

### 3.4 Sistem Inventory dan Item

[[120. Merancang struktur data inventory menggunakan Array atau Dictionary]]

[[121. Membuat ItemData sebagai Custom Resource dengan properti nama ikon dan efek]]

[[122. Implementasi pickup system menggunakan Area2D dan Signal]]

[[123. Membuat UI inventory menggunakan GridContainer dan ItemList]]

[[124. Sistem equip dan unequip item beserta efeknya pada karakter]]

[[125. Implementasi stackable item dan quantity management]]

[[126. Menyimpan dan memuat data inventory menggunakan sistem save game]]

---

### 3.5 Sistem Save dan Load

[[127. Memahami cara kerja save game dan format data yang umum digunakan]]

[[128. Menggunakan FileAccess untuk menulis dan membaca file di Godot]]

[[129. Menyimpan data menggunakan format JSON dan cara parse-nya]]

[[130. Implementasi sistem save slot untuk multiple save files]]

[[131. Menggunakan ConfigFile untuk menyimpan pengaturan game seperti volume dan resolusi]]

[[132. Teknik auto-save dan checkpoint system dalam game]]

[[133. Mengenkripsi file save untuk mencegah cheating dasar]]

---

### 3.6 Navigasi dan Pathfinding

[[134. Memahami konsep NavigationRegion2D dan NavigationAgent2D]]

[[135. Membuat NavigationMesh untuk area yang bisa dilalui oleh AI]]

[[136. Implementasi pathfinding sederhana menggunakan NavigationAgent2D]]

[[137. Menggunakan AStar2D untuk pathfinding kustom berbasis grid]]

[[138. Membuat formasi gerakan untuk multiple enemy mengikuti path]]

[[139. Dynamic obstacle dan cara memperbarui navigation mesh secara runtime]]

---

### 3.7 UI dan UX Game

[[140. Memahami Control Node sebagai dasar semua elemen UI di Godot]]

[[141. Menggunakan Container seperti HBoxContainer VBoxContainer dan GridContainer]]

[[142. Membuat menu utama dengan tombol Play Settings dan Quit]]

[[143. Implementasi pause menu dengan teknik pause tree yang benar]]

[[144. Membuat HUD yaitu Heads Up Display yang menampilkan health mana dan skor]]

[[145. Menggunakan theme untuk konsistensi visual UI di seluruh game]]

[[146. Membuat dialog box dan sistem percakapan sederhana]]

[[147. Implementasi loading screen dengan progress bar antar scene]]

---

## 🟠 LEVEL 4: INTERMEDIATE (Menengah)

### 4.1 Desain Pattern dalam Game Dev

[[148. Memahami State Machine dan implementasinya dalam GDScript]]

[[149. Membuat Finite State Machine untuk karakter player dengan state idle run jump attack]]

[[150. Implementasi State Machine untuk AI enemy dengan state patrol chase attack]]

[[151. Pola Command Pattern untuk implementasi undo redo dan input buffering]]

[[152. Pola Observer Pattern secara mendalam menggunakan Signal Godot]]

[[153. Pola Object Pool untuk mengoptimalkan instansiasi objek berulang seperti peluru]]

[[154. Pola Strategy Pattern untuk AI behavior yang dapat diubah secara runtime]]

[[155. Event Bus yaitu sistem komunikasi global menggunakan Autoload dan Signal]]

---

### 4.2 Shader Dasar

[[156. Apa itu shader dan bagaimana GPU rendering bekerja secara konseptual]]

[[157. Mengenal Shader Language Godot yaitu syntax dasar yang mirip GLSL]]

[[158. Membuat shader 2D sederhana yaitu outline effect pada sprite]]

[[159. Membuat shader efek dissolve atau disintegration menggunakan noise texture]]

[[160. Implementasi shader untuk efek glow dan bloom sederhana]]

[[161. Menggunakan VisualShader untuk membuat shader tanpa menulis kode]]

[[162. Shader untuk efek air dan liquid sederhana dalam game 2D]]

[[163. Menggunakan ShaderMaterial dan cara mengoper parameter dari GDScript ke shader]]

---

### 4.3 Efek Partikel

[[164. Memahami GPUParticles2D dan perbedaannya dengan CPUParticles2D]]

[[165. Membuat efek partikel ledakan menggunakan GPUParticles2D]]

[[166. Membuat efek partikel hujan salju dan angin]]

[[167. Membuat efek partikel trail untuk karakter atau proyektil]]

[[168. Mengontrol emisi partikel dari GDScript secara dinamis]]

[[169. Mengoptimalkan performa partikel untuk mobile target]]

---

### 4.4 Sistem Combat dan Gameplay

[[170. Implementasi sistem hitbox dan hurtbox menggunakan Area2D]]

[[171. Membuat sistem damage number yang melayang menggunakan instansiasi dinamis]]

[[172. Implementasi knockback effect saat karakter terkena serangan]]

[[173. Membuat sistem combo attack dengan input buffer]]

[[174. Implementasi cooldown system menggunakan Timer node]]

[[175. Membuat projectile system yang efisien menggunakan object pool]]

[[176. Sistem status effect seperti poison slow dan stun]]

[[177. Implementasi lock-on target system untuk combat]]

---

### 4.5 Procedural Generation Dasar

[[178. Konsep dasar procedural generation dan manfaatnya dalam game]]

[[179. Membuat random dungeon sederhana menggunakan algoritma Binary Space Partitioning]]

[[180. Implementasi Wave Function Collapse sederhana untuk map generation]]

[[181. Menggunakan noise function FastNoiseLite untuk terrain generation]]

[[182. Procedural item generation menggunakan tabel loot dan bobot probabilitas]]

[[183. Membuat endless runner level menggunakan chunk-based generation]]

---

### 4.6 Optimasi Performa Dasar

[[184. Memahami cara menggunakan Godot Profiler untuk mengidentifikasi bottleneck]]

[[185. Perbedaan antara CPU bottleneck dan GPU bottleneck dalam game]]

[[186. Teknik Object Pooling untuk mengurangi biaya instansiasi dan penghapusan objek]]

[[187. Menggunakan VisibilityNotifier2D untuk menonaktifkan objek di luar layar]]

[[188. Memahami draw call dan cara menguranginya menggunakan atlasing]]

[[189. Teknik LOD yaitu Level of Detail untuk mengoptimalkan rendering]]

[[190. Menggunakan Thread dalam Godot untuk komputasi berat tanpa menghentikan game]]

---

### 4.7 Project Menengah: Top-Down RPG Sederhana

[[191. Merancang game design document sederhana untuk RPG top-down]]

[[192. Membuat sistem karakter dengan stats seperti health attack defense dan speed]]

[[193. Implementasi sistem quest sederhana menggunakan State Machine]]

[[194. Membuat NPC dengan dialog system dan quest trigger]]

[[195. Implementasi overworld map dengan transisi antar zone]]

[[196. Membuat sistem toko sederhana dengan transaksi beli dan jual]]

[[197. Implementasi boss fight dengan multiple phase menggunakan State Machine]]

[[198. Polishing game dengan juice yaitu screen shake sound effect dan visual feedback]]

---

## 🔴 LEVEL 5: UPPER-INTERMEDIATE (Menengah Lanjut)

### 5.1 Godot 3D Fundamentals

[[199. Memahami sistem koordinat 3D yaitu sumbu X Y Z dan transformasi dasar]]

[[200. Mengenal Node 3D utama yaitu Node3D MeshInstance3D CollisionShape3D dan Camera3D]]

[[201. Memahami MeshLibrary dan penggunaan GridMap untuk level 3D berbasis grid]]

[[202. Menggunakan CharacterBody3D untuk pergerakan karakter 3D]]

[[203. Implementasi first-person controller dengan mouse look menggunakan kode]]

[[204. Implementasi third-person controller dengan spring arm camera]]

[[205. Memahami sistem lighting 3D yaitu DirectionalLight3D OmniLight3D dan SpotLight3D]]

[[206. Menggunakan WorldEnvironment dan Sky untuk menciptakan atmosfer scene 3D]]

---

### 5.2 Physics 3D

[[207. Perbedaan StaticBody3D RigidBody3D CharacterBody3D dan AnimatableBody3D]]

[[208. Menggunakan RayCast3D untuk shooting detection dan ground detection]]

[[209. Implementasi grabbing dan throwing objek fisika menggunakan RigidBody3D]]

[[210. Membuat vehicle sederhana menggunakan VehicleBody3D]]

[[211. Menggunakan SoftBody3D untuk efek kain atau benda lentur]]

[[212. Mengoptimalkan physics 3D dengan mengatur collision layer dan mask secara tepat]]

---

### 5.3 Animasi 3D

[[213. Mengimpor model 3D beserta animasi dari software eksternal seperti Blender]]

[[214. Menggunakan AnimationTree dengan StateMachine untuk blending animasi 3D]]

[[215. Implementasi Inverse Kinematics atau IK menggunakan SkeletonIK3D]]

[[216. Membuat animasi procedural seperti footstep placement menggunakan IK]]

[[217. Menggunakan AnimationPlayer untuk cutscene berbasis keyframe di game 3D]]

[[218. Retargeting animasi untuk menggunakan animasi yang sama pada beberapa karakter]]

---

### 5.4 Shader Lanjutan

[[219. Memahami vertex shader dan fragment shader secara lebih mendalam]]

[[220. Membuat shader material PBR kustom dengan normal map roughness dan metallic]]

[[221. Implementasi shader efek air realistis dengan normal map animation]]

[[222. Membuat shader untuk efek forcefield atau shield menggunakan rim lighting]]

[[223. Implementasi outline shader berbasis normal untuk gaya cel-shading]]

[[224. Menggunakan compute shader untuk kalkulasi berat di GPU]]

[[225. Membuat post-processing effect menggunakan SubViewport dan shader]]

---

### 5.5 AI Game Lanjutan

[[226. Memahami Behavior Tree sebagai arsitektur AI yang lebih fleksibel dari State Machine]]

[[227. Implementasi Behavior Tree sederhana menggunakan Resource dan GDScript]]

[[228. Membuat AI yang menggunakan NavigationAgent3D untuk pathfinding di lingkungan 3D]]

[[229. Implementasi sistem Steering Behaviors yaitu seek flee arrive dan wander]]

[[230. Membuat AI dengan field of view dan hearing radius untuk deteksi player]]

[[231. Implementasi group AI yaitu flocking algorithm dengan separation alignment dan cohesion]]

[[232. Membuat AI yang beradaptasi terhadap perilaku player menggunakan data sederhana]]

---

### 5.6 Multiplayer Dasar

[[233. Memahami konsep multiplayer yaitu client-server peer-to-peer dan authoritative server]]

[[234. Mengenal MultiplayerAPI bawaan Godot berbasis ENet dan WebRTC]]

[[235. Implementasi lobby system sederhana untuk multiplayer lokal dan online]]

[[236. Menggunakan RPC atau Remote Procedure Call untuk sinkronisasi antar peer]]

[[237. Implementasi MultiplayerSynchronizer untuk sinkronisasi properti Node secara otomatis]]

[[238. Menangani lag compensation dan prediction dasar dalam multiplayer game]]

[[239. Memahami keamanan dasar dalam multiplayer yaitu validasi di server side]]

---

### 5.7 Tools dan Pipeline Pengembangan

[[240. Menggunakan version control Git bersama Godot secara efektif]]

[[241. Mengatur gitignore yang tepat untuk project Godot]]

[[242. Menggunakan Godot Plugin system untuk memperluas fungsionalitas editor]]

[[243. Membuat editor tool sederhana menggunakan annotation tool]]

[[244. Menggunakan GDUnit atau Gut untuk unit testing script GDScript]]

[[245. Memahami Continuous Integration dasar untuk project game]]

[[246. Teknik debugging lanjutan menggunakan Remote Debugger dan Breakpoint]]

---

## ⚫ LEVEL 6: ADVANCED (Lanjut)

### 6.1 Arsitektur Proyek Skala Besar

[[247. Merancang arsitektur plugin-based untuk game yang modular]]

[[248. Menggunakan Subproject dan Git Submodule untuk manajemen modul]]

[[249. Implementasi Dependency Injection pattern dalam GDScript]]

[[250. Merancang sistem Event Bus yang skalabel untuk game kompleks]]

[[251. Teknik code splitting dan lazy loading untuk mengurangi waktu loading]]

[[252. Dokumentasi kode menggunakan standar komentar GDScript yang konsisten]]

[[253. Membuat API internal yang bersih untuk komunikasi antar sistem game]]

---

### 6.2 Rendering Lanjutan

[[254. Memahami Rendering Pipeline Godot 4 yaitu Forward Plus Mobile dan Compatibility]]

[[255. Mengoptimalkan scene 3D menggunakan occlusion culling dan LOD]]

[[256. Menggunakan LightmapGI dan VoxelGI untuk global illumination yang efisien]]

[[257. Implementasi SDFGI yaitu Signed Distance Field Global Illumination untuk scene dinamis]]

[[258. Menggunakan Decal untuk proyeksi tekstur pada permukaan 3D]]

[[259. Implementasi custom render pipeline menggunakan RenderingServer langsung]]

[[260. Teknik batching dan instancing untuk rendering ribuan objek secara efisien]]

---

### 6.3 Optimasi Performa Lanjutan

[[261. Profiling mendalam menggunakan Godot Profiler dan RenderDoc]]

[[262. Menggunakan Thread dan WorkerThreadPool untuk komputasi paralel]]

[[263. Implementasi spatial partitioning menggunakan Octree atau QuadTree kustom]]

[[264. Mengoptimalkan GDScript dengan menghindari allocasi memori yang berlebihan]]

[[265. Kapan dan bagaimana menggunakan GDExtension untuk kode performa kritis]]

[[266. Teknik streaming level untuk open world yang besar]]

[[267. Profiling dan optimasi shader untuk mengurangi GPU overhead]]

---

### 6.4 GDExtension dan Interoperabilitas

[[268. Apa itu GDExtension dan bagaimana perbedaannya dengan GDNative di Godot 3]]

[[269. Menyiapkan lingkungan pengembangan C++ untuk GDExtension]]

[[270. Membuat class C++ sederhana yang dapat diakses dari GDScript]]

[[271. Mengintegrasikan library pihak ketiga menggunakan GDExtension]]

[[272. Memahami memory management saat bekerja dengan C++ di Godot]]

[[273. Debugging GDExtension menggunakan tools seperti GDB atau LLDB]]

---

### 6.5 Platform-Specific Development

[[274. Memahami proses export Godot ke berbagai platform yaitu Windows macOS Linux Android iOS dan Web]]

[[275. Mengoptimalkan game untuk mobile yaitu tekstur kompresi shader sederhana dan batas polygon]]

[[276. Implementasi fitur mobile native seperti touch gesture dan accelerometer]]

[[277. Mengintegrasikan Google Play Games Services dan Apple Game Center]]

[[278. Menggunakan Godot untuk pengembangan game Web dengan HTML5 export]]

[[279. Memahami persyaratan submission ke platform store seperti Steam Google Play dan App Store]]

[[280. Implementasi In-App Purchase menggunakan plugin Godot]]

---

### 6.6 Game Feel dan Polishing

[[281. Memahami konsep Game Juice dan mengapa feedback visual dan audio penting]]

[[282. Implementasi screen shake yang terasa memuaskan menggunakan noise]]

[[283. Teknik hit stop yaitu membekukan game sesaat saat serangan mengenai target]]

[[284. Implementasi coyote time dan jump buffer untuk platformer yang responsif]]

[[285. Membuat sistem particle yang bereaksi terhadap environment]]

[[286. Implementasi sound design yang mendukung game feel yaitu pitch variation dan layering]]

[[287. Teknik animasi squash and stretch untuk karakter yang ekspresif]]

[[288. Implementasi camera techniques lanjutan seperti zoom on impact dan cinematic bars]]

---

## 🟣 LEVEL 7: MASTERY DAN SPECIALIZATION (Mahir)

### 7.1 Game Design Principles Lanjutan

[[289. Memahami MDA Framework yaitu Mechanics Dynamics dan Aesthetics dalam desain game]]

[[290. Merancang game economy yang seimbang menggunakan spreadsheet dan iterasi]]

[[291. Playtesting methodology yaitu cara mengumpulkan data feedback dari pemain]]

[[292. Implementasi difficulty scaling menggunakan dynamic difficulty adjustment]]

[[293. Merancang tutorial yang mengajarkan tanpa menggurui menggunakan show not tell]]

[[294. Memahami psikologi pemain yaitu flow theory intrinsic motivation dan reward schedule]]

[[295. Merancang accessibility features yaitu color blind mode subtitle dan control remapping]]

---

### 7.2 Spesialisasi: Roguelike dan Procedural Game

[[296. Merancang sistem roguelike dengan permadeath progression dan replayability]]

[[297. Implementasi dungeon generation lanjutan menggunakan algoritma Cellular Automata]]

[[298. Membuat sistem meta-progression antara run menggunakan data persisten]]

[[299. Merancang item synergy dan build diversity untuk roguelike yang mendalam]]

[[300. Implementasi seed-based generation untuk reproducible world]]

---

### 7.3 Spesialisasi: Multiplayer Game Lanjutan

[[301. Implementasi authoritative server architecture menggunakan Godot dan server headless]]

[[302. Membuat matchmaking system sederhana menggunakan relay server]]

[[303. Implementasi rollback netcode untuk fighting game atau action game multiplayer]]

[[304. Mengintegrasikan layanan backend seperti Nakama atau PlayFab dengan Godot]]

[[305. Membuat anti-cheat system dasar pada level server]]

---

### 7.4 Spesialisasi: Narrative Game dan Visual Novel

[[306. Merancang branching narrative menggunakan grafik dan tools seperti Twine]]

[[307. Implementasi dialogue system lanjutan menggunakan Dialogic plugin atau sistem kustom]]

[[308. Membuat cutscene system menggunakan AnimationPlayer dan sinematografi dasar]]

[[309. Implementasi choice consequence system yang mempengaruhi akhir cerita]]

[[310. Mengintegrasikan voice acting dan subtitle system yang sinkron]]

---

### 7.5 Publishing dan Monetisasi

[[311. Membuat press kit dan materi marketing untuk game indie]]

[[312. Strategi wishlist campaign di Steam dan platform lainnya]]

[[313. Memahami model monetisasi yaitu premium free-to-play dan premium plus IAP]]

[[314. Implementasi analytics dasar untuk memahami perilaku pemain]]

[[315. Membangun komunitas game menggunakan Discord Reddit dan media sosial]]

[[316. Bekerja dengan publisher indie yaitu apa yang perlu diperhatikan dalam kontrak]]

---

### 7.6 Kontribusi pada Komunitas dan Open Source

[[317. Cara berkontribusi pada Godot Engine repository di GitHub]]

[[318. Membuat dan mempublikasikan Godot plugin ke Godot Asset Library]]

[[319. Menulis dokumentasi teknis dan tutorial untuk komunitas]]

[[320. Berpartisipasi dalam Game Jam menggunakan Godot sebagai media belajar intensif]]

[[321. Membangun portofolio game dev yang profesional untuk karir atau freelance]]

---

### 7.7 Sumber Daya dan Ekosistem Pembelajaran

[[322. Dokumentasi resmi Godot yaitu docs dot godotengine dot org sebagai referensi utama]]

[[323. Komunitas Godot yaitu forum Reddit r slash godot dan Discord resmi]]

[[324. Channel YouTube terpilih untuk belajar Godot seperti GDQuest Brackeys era Godot dan KidsCanCode]]

[[325. Buku referensi game dev seperti Game Programming Patterns oleh Robert Nystrom]]

[[326. Asset gratis untuk belajar yaitu Kenney dot nl OpenGameArt dan itch dot io]]

[[327. Mengikuti perkembangan Godot melalui blog resmi dan devblog contributor]]

[[328. Bergabung dengan Global Game Jam Ludum Dare dan game jam berbasis komunitas Godot]]

---

## 📋 PETA PERKEMBANGAN

text

```
Level 1 - Absolute Beginner   ██░░░░░░░░░░░░  Poin 1 hingga 45
Level 2 - Elementary          ████░░░░░░░░░░  Poin 46 hingga 98
Level 3 - Pre-Intermediate    ██████░░░░░░░░  Poin 99 hingga 198
Level 4 - Intermediate        ████████░░░░░░  Poin 148 hingga 198
Level 5 - Upper Intermediate  ██████████░░░░  Poin 199 hingga 246
Level 6 - Advanced            ████████████░░  Poin 247 hingga 288
Level 7 - Mastery             ██████████████  Poin 289 hingga 328
```

---

## 🎯 TIPS PENGGUNAAN KURIKULUM INI

|Tips|Penjelasan|
|---|---|
|Belajar sambil membuat|Setiap konsep langsung terapkan dalam mini project|
|Satu poin per sesi|Fokus kuasai satu topik sebelum lanjut ke berikutnya|
|Buat sesuatu yang kamu sukai|Motivasi terjaga jika proyek sesuai minat|
|Bergabung komunitas|Feedback dari sesama dev sangat mempercepat belajar|
|Ikuti Game Jam|Deadline mendorong kamu menyelesaikan game nyata|
|Catat dan dokumentasikan|Buat catatan solusi bug agar tidak mengulang kesalahan yang sama|
|Konsistensi|Satu jam setiap hari jauh lebih efektif dari marathon seminggu sekali|

---

> _"The best way to learn game development is to make games - lots of them."_  
> — Dikutip dari filosofi komunitas indie game dev global

**Kurikulum ini mencakup 328 poin belajar yang dirancang membawa kamu dari nol hingga mampu membuat dan mempublikasikan game menggunakan Godot Engine. Estimasi waktu: 1 hingga 3 tahun dengan latihan konsisten setiap hari.**