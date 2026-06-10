# 72 — Graph: Representasi & Traversal

## Penjelasan
Graph adalah struktur data yang terdiri dari vertex (node) dan edge (sisi). Tidak seperti tree, graph bisa memiliki siklus. Dua representasi umum: adjacency list (tiap vertex punya daftar tetangga) dan adjacency matrix. Dua traversal: BFS (Breadth-First Search — shortest path) dan DFS (Depth-First Search — explore semua jalur).

## Fungsi
- BFS: mencari jalur terpendek (dalam jumlah edge)
- DFS: deteksi siklus, topological sort, puzzle solver
- Adjacency list: hemat memori untuk graph jarang (sparse)

## Code

```javascript
// Representasi Graph dengan Adjacency List
class Graph {
  constructor() {
    this.adjacencyList = {};
  }

  addVertex(vertex) {
    if (!this.adjacencyList[vertex]) this.adjacencyList[vertex] = [];
  }

  addEdge(v1, v2) {
    this.adjacencyList[v1].push(v2);
    this.adjacencyList[v2].push(v1); // undirected
  }

  // BFS — cari jalur terpendek
  bfs(start, goal) {
    const queue = [[start]];
    const visited = new Set([start]);

    while (queue.length > 0) {
      const path = queue.shift();
      const current = path[path.length - 1];

      if (current === goal) return path;

      for (const neighbor of this.adjacencyList[current]) {
        if (!visited.has(neighbor)) {
          visited.add(neighbor);
          queue.push([...path, neighbor]);
        }
      }
    }
    return null;
  }

  // DFS — cari semua jalur
  dfs(start, goal, visited = new Set(), path = []) {
    path.push(start);
    if (start === goal) return path;

    visited.add(start);
    for (const neighbor of this.adjacencyList[start]) {
      if (!visited.has(neighbor)) {
        const result = this.dfs(neighbor, goal, visited, [...path]);
        if (result) return result;
      }
    }
    return null;
  }
}

// === Peta Kota ===
const map = new Graph();
const kota = ['Rumah', 'Toko', 'Sekolah', 'RS', 'Kantor', 'Taman', 'Pasar'];
kota.forEach((k) => map.addVertex(k));

map.addEdge('Rumah', 'Toko');
map.addEdge('Rumah', 'Sekolah');
map.addEdge('Toko', 'Kantor');
map.addEdge('Toko', 'RS');
map.addEdge('Sekolah', 'Taman');
map.addEdge('Sekolah', 'Pasar');
map.addEdge('RS', 'Kantor');
map.addEdge('Taman', 'Pasar');

console.log('BFS Rumah → Kantor:', map.bfs('Rumah', 'Kantor'));
console.log('DFS Rumah → Kantor:', map.dfs('Rumah', 'Kantor'));
```

## Analogi Rumah — Peta Kota

| Konsep | Analogi |
|--------|---------|
| Vertex | Persimpangan jalan |
| Edge | Jalan yang menghubungkan persimpangan |
| BFS | Cari rute terpendek — cek semua tetangga dulu sebelum lanjut jauh |
| DFS | Jelajah — ambil satu jalan sampai buntu, baru balik dan coba jalan lain |
| Adjacency list | Papan informasi di tiap persimpangan: "Dari sini bisa ke mana saja?" |
| Cycle | Jalan yang muter balik ke persimpangan awal (loop) |
| Shortest path | Rute paling sedikit lampu merah (paling sedikit persimpangan) |

## Use Case
- **Google Maps**: BFS untuk shortest path (dengan weight = jarak/waktu)
- **Social network**: BFS untuk "orang yang mungkin Anda kenal" (friend of friend)
- **Web crawler**: DFS untuk crawling website secara depth-first
- **GPS navigation**: A* (varian BFS dengan heuristic)
- **Deteksi siklus**: mencegah deadlock di sistem operasi

## Kesalahan Umum
- BFS menggunakan stack (LIFO) alih-alih queue (FIFO) → jadi DFS
- Tidak handle graph disconnected → traversal hanya mencakup satu komponen
- Infinite loop di graph ber-cycle tanpa visited set → stack overflow

## Benang Merah
Dari Materi 71 (Tree — graph terarah tanpa siklus) → Materi 72 (Graph — struktur data paling umum). Graph bisa menyambung ke semua materi sebelumnya: array (adjacency list), linked list (chain), tree (subgraph). Lanjut ke Materi 73 (Dynamic Programming) sebagai penutup algoritma.

## Soal + Jawaban

**1. Apa perbedaan utama BFS dan DFS dalam hasil?**
Jawaban: BFS memberi jalur terpendek (jumlah edge minimal). DFS memberi satu jalur (belum tentu terpendek).

**2. Struktur data apa yang digunakan BFS dan DFS secara internal?**
Jawaban: BFS menggunakan **queue** (FIFO). DFS menggunakan **stack** (LIFO) — bisa eksplisit atau via rekursi.

**3. Mengapa adjacency list lebih umum digunakan daripada adjacency matrix?**
Jawaban: Adjacency list hemat memori untuk graph sparse (jarang terhubung). Adjacency matrix O(V²) memori — boros untuk graph besar dengan sedikit edge.
