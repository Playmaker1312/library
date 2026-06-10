# 105 — Memory Management & Memory Leaks

## 1. Penjelasan

**Garbage Collector (GC)** adalah komponen di runtime JavaScript (V8, SpiderMonkey) yang secara otomatis mengelola alokasi dan dealokasi memori. GC menggunakan algoritma **Mark-and-Sweep**: menandai objek yang masih bisa diakses dari **GC roots** (global object, DOM references, stack variables), lalu menyapu bersih objek yang tidak tertandai.

**Memory Leak** terjadi ketika objek yang sudah tidak diperlukan masih memiliki referensi aktif, sehingga GC tidak bisa membersihkannya. Memori terus bertambah hingga aplikasi melambat atau crash (Out of Memory).

**WeakMap & WeakSet** adalah struktur data yang menyimpan referensi "lemah" (weak reference) ke objek. Jika tidak ada referensi lain ke objek tersebut, GC bisa membersihkannya meskipun masih ada di WeakMap/WeakSet.

| Struktur | Referensi | Dicegah GC? | Iterable? |
|----------|-----------|-------------|-----------|
| Map | Kuat | Ya | Ya |
| WeakMap | Lemah | Tidak | Tidak |
| Set | Kuat | Ya | Ya |
| WeakSet | Lemah | Tidak | Tidak |

## 2. Fungsi

- Mencegah memory leak dengan memahami GC roots dan reference chain
- Membersihkan event listener, timer, dan closure yang tidak terpakai
- Menggunakan WeakMap/WeakSet untuk cache dan metadata tanpa bocor
- Mendeteksi dan mendiagnosis memory leak via Chrome DevTools (Heap Snapshot, Allocation Timeline)
- Optimalisasi memori untuk aplikasi long-running (SPA, server)

## 3. Code

### Memory Leak — Event Listener

```javascript
// ❌ LEAK: Event listener tidak pernah dihapus
class ChatWidget {
  constructor(container) {
    this.container = container;
    window.addEventListener('resize', () => {
      this.adjustLayout();
    });
    // Setiap instance ChatWidget menambah listener baru
    // Meskipun widget dihapus dari DOM, listener tetap aktif
  }
  adjustLayout() {
    console.log('Menyesuaikan layout...');
  }
}

// ✅ FIX: Hapus event listener saat cleanup
class ChatWidgetFixed {
  constructor(container) {
    this.container = container;
    this.boundHandler = this.adjustLayout.bind(this);
    window.addEventListener('resize', this.boundHandler);
  }
  adjustLayout() {
    console.log('Menyesuaikan layout...');
  }
  destroy() {
    window.removeEventListener('resize', this.boundHandler);
    this.container = null;
  }
}
```

### Memory Leak — Closure & Global Variable

```javascript
// ❌ LEAK: Data besar tersimpan di closure global
const cache = [];

function processLargeData() {
  const heavyData = new Array(1000000).fill('data');
  cache.push(heavyData); // Tersimpan selamanya!
}

// ✅ FIX: Hapus referensi setelah selesai
function processLargeDataFixed() {
  const heavyData = new Array(1000000).fill('data');
  const result = heavyData.map(item => item.toUpperCase());
  // heavyData otomatis di-GC setelah fungsi selesai
  // (tidak ada referensi lagi)
  return result;
}

// Atau gunakan WeakMap untuk cache
const weakCache = new WeakMap();

function cacheResult(obj, result) {
  weakCache.set(obj, result);
  // Jika obj dihapus, result ikut di-GC
}
```

### Deteksi Memory Leak — Simulation

```javascript
// Simulasi leak untuk deteksi
const leakyArray = [];

function createLeak() {
  const largeObj = new Array(50000).fill('leak');
  leakyArray.push(largeObj);
  console.log(`Leak size: ${leakyArray.length} items`);
}

// Buka DevTools → Performance → Memory
// Rekam heap snapshot, jalankan createLeak() beberapa kali
// Ambil snapshot lagi → bandingkan: objek tidak di-GC = leak
```

### WeakMap untuk Metadata

```javascript
// ✅ Menggunakan WeakMap agar metadata tidak mencegah GC
const metadata = new WeakMap();

class User {
  constructor(name) {
    this.name = name;
    metadata.set(this, { created: Date.now(), lastAccess: null });
  }
  access() {
    const meta = metadata.get(this);
    meta.lastAccess = Date.now();
  }
}

let user = new User('Budi');
user.access();
user = null;
// Ketika user dihapus, metadata di WeakMap ikut hilang
// Tidak seperti Map biasa yang masih menyimpan entry
```

## 4. Analogi Rumah — Sampah Rumah

| Konsep JS | Analogi Rumah |
|-----------|---------------|
| Garbage Collector | Tukang sampah yang datang rutin |
| GC Roots | Orang-orang di rumah — barang yang masih dipakai oleh mereka |
| Mark-and-Sweep | Tukang sampah ngecek: ini masih dipakai? kalau tidak → dibuang |
| Memory Leak | Anda menyimpan semua koran bekas (referensi) di gudang |
| Event listener leak | Memasang sensor pintu, tapi saat sensor dilepas, kabelnya masih nyambung |
| Closure leak | Buka kulkas, ambil makanan, pintu kulkas dibiarkan terbuka |
| WeakMap | Kotak penyimpanan di lemari — jika barang hilang, kotak kosong otomatis dibuang |
| Global variable leak | Barang berserakan di ruang tamu, tidak ada yang berani menyentuh |
| Out of Memory | Rumah penuh sampah, tidak bisa masuk lagi |

> **Mengapa WeakMap aman:** Bayangkan Anda punya buku catatan yang mencatat barang-barang di rumah. Jika barangnya sudah dibuang, catatan tentang barang itu otomatis terhapus (WeakMap). Map biasa seperti buku catatan permanen — barang sudah dibuang, catatannya tetap ada, memenuhi rak buku Anda.

## 5. Use Case di Proyek Nyata

- **Cache DOM elements** — menyimpan referensi DOM node tanpa mencegah node dihapus
- **Private data per instance** — menyimpan data private tanpa properti enumerable
- **Object pool** — mengelola objek yang sering dibuat/dihancurkan
- **Event delegation cleanup** — SPA yang berganti halaman, pastikan listener lama dibersihkan
- **Long-running server** — Express/Node.js server yang tidak restart perlu manajemen memori ketat

## 6. Kesalahan Umum

| Kesalahan | Dampak | Solusi |
|-----------|--------|--------|
| Event listener tidak di-remove | Listener menumpuk, callback menahan referensi | Simpan referensi handler, remove di `destroy()` / `useEffect` cleanup |
| Global variable menumpuk | Tidak pernah di-GC | Gunakan local scope, atau reset manual |
| Closure yang tidak sengaja menahan scope | Objek besar tidak di-GC | Batasi scope closure, jangan referensi objek besar tanpa perlu |
| Timer/setInterval tidak dibersihkan | Callback terus berjalan meski komponen unmount | Simpan `intervalId`, clear di cleanup |
| Cache Map tumbuh tak terbatas | Memori habis | Gunakan WeakMap atau implementasi LRU cache |
| Detached DOM nodes | Node DOM yang dihapus masih direferensi JS | Hapus referensi saat node di-remove |

## 7. Benang Merah

```
Materi 104 (Proxy & Reflect)
    ↓
Materi 105 (Memory Management & Memory Leaks) ← Anda di sini
    ↓
Materi 106 (Concurrency — Web Workers & Worker Threads)
```

Setelah membangun sistem yang cerdas dengan metaprogramming (Proxy/Reflect), Anda harus memahami bagaimana memori dikelola. Tanpa ini, aplikasi canggih sekalipun akan lambat dan crash. Manajemen memori yang baik adalah syarat untuk masuk ke materi concurrency (Materi 106) — karena worker threads pun perlu mengelola memori dengan hati-hati.

## 8. Soal & Jawaban

### Soal 1: Easy
Apa output dan jelaskan mengapa?

```javascript
let a = { name: 'Objek A' };
let b = { name: 'Objek B' };
let map = new Map();
map.set(a, 'data A');
a = null;
// Apakah objek { name: 'Objek A' } masih ada di memori?
```

<details>
<summary>Jawaban</summary>
Ya, objek masih ada di memori. Meskipun `a = null`, Map memiliki referensi **kuat** ke objek tersebut. GC tidak bisa membersihkannya karena masih bisa diakses melalui `map`. Jika menggunakan WeakMap, objek akan di-GC.
</details>

### Soal 2: Medium
Perbaiki kode berikut yang memiliki memory leak:

```javascript
function startNotifications() {
  setInterval(() => {
    fetch('/api/notifications')
      .then(res => res.json())
      .then(data => {
        const container = document.getElementById('notif-container');
        data.forEach(n => {
          const el = document.createElement('div');
          el.textContent = n.message;
          container.appendChild(el);
        });
      });
  }, 5000);
}
startNotifications();
```

<details>
<summary>Jawaban</summary>

```javascript
let intervalId = null;

function startNotifications(containerId = 'notif-container') {
  if (intervalId) return; // Cegah multiple interval
  intervalId = setInterval(() => {
    fetch('/api/notifications')
      .then(res => res.json())
      .then(data => {
        const container = document.getElementById(containerId);
        if (!container) {
          stopNotifications(); // Container sudah tidak ada
          return;
        }
        // Hanya tampilkan notifikasi baru, bersihkan yang lama
        container.innerHTML = '';
        data.forEach(n => {
          const el = document.createElement('div');
          el.textContent = n.message;
          container.appendChild(el);
        });
      })
      .catch(() => stopNotifications());
  }, 5000);
}

function stopNotifications() {
  if (intervalId) {
    clearInterval(intervalId);
    intervalId = null;
  }
}
```
</details>

### Soal 3: Hard
Buatlah fungsi `createSafeCache()` yang menggunakan WeakMap untuk menyimpan hasil komputasi berat berdasarkan input objek. Jika objek input dihapus dari luar, cache harus otomatis ikut terhapus. Sertakan mekanisme pembatasan ukuran cache (max 100 entry) dengan membuang entry tertua.

<details>
<summary>Jawaban</summary>

```javascript
function createSafeCache(maxSize = 100) {
  const cache = new Map(); // Map untuk entry terbaru (iterator)
  const weakRefs = new WeakMap(); // WeakMap untuk actual cache

  return {
    get(key) {
      if (weakRefs.has(key)) {
        // Refresh posisi di Map (paling baru)
        cache.delete(key);
        cache.set(key, true);
        return weakRefs.get(key);
      }
      return undefined;
    },
    set(key, value) {
      if (weakRefs.has(key)) {
        cache.delete(key);
      }
      // Evict jika penuh: hapus entry tertua (first item Map)
      while (cache.size >= maxSize) {
        const oldestKey = cache.keys().next().value;
        cache.delete(oldestKey);
        // Jangan hapus dari WeakMap — key objek mungkin masih dipakai
        // Tapi kita hapus referensi agar entry bisa di-GC
        // (kita simpan key di cache Map, bukan WeakMap)
      }
      cache.set(key, true);
      weakRefs.set(key, value);
    },
    delete(key) {
      cache.delete(key);
      weakRefs.delete(key);
    },
    get size() {
      return cache.size;
    },
  };
}

// Usage
const safeCache = createSafeCache(3);
let obj1 = { id: 1 };
let obj2 = { id: 2 };
let obj3 = { id: 3 };
let obj4 = { id: 4 };

safeCache.set(obj1, 'Hasil komputasi A');
safeCache.set(obj2, 'Hasil komputasi B');
safeCache.set(obj3, 'Hasil komputasi C');
console.log(safeCache.get(obj1)); // 'Hasil komputasi A'

safeCache.set(obj4, 'Hasil komputasi D'); // Evict yang tertua
console.log(safeCache.get(obj2)); // undefined (ter-evict)

obj1 = null; // Jika tidak ada referensi lain ke obj1
// entry di WeakMap akan di-GC secara otomatis
```
</details>
