# 71 — Tree & Binary Search Tree

## Penjelasan
Tree adalah struktur data hierarkis terdiri dari node yang terhubung oleh edge. Binary Search Tree (BST) adalah tree di mana setiap node memiliki maksimal 2 anak: anak kiri lebih kecil, anak kanan lebih besar. Traversal: Inorder (kiri-root-kanan → sorted), Preorder (root-kiri-kanan), Postorder (kiri-kanan-root).

## Fungsi
- Representasi data hierarkis (organisasi, file system, HTML DOM)
- Pencarian cepat O(log n) rata-rata pada BST
- Parsing ekspresi matematika (expression tree)

## Code

```javascript
class TreeNode {
  constructor(data) {
    this.data = data;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }

  insert(data) {
    const newNode = new TreeNode(data);
    if (!this.root) {
      this.root = newNode;
      return;
    }
    let current = this.root;
    while (current) {
      if (data < current.data) {
        if (!current.left) { current.left = newNode; return; }
        current = current.left;
      } else {
        if (!current.right) { current.right = newNode; return; }
        current = current.right;
      }
    }
  }

  search(data) {
    let current = this.root;
    while (current) {
      if (data === current.data) return true;
      current = data < current.data ? current.left : current.right;
    }
    return false;
  }

  inorder(node = this.root, result = []) {
    if (!node) return result;
    this.inorder(node.left, result);
    result.push(node.data);
    this.inorder(node.right, result);
    return result;
  }

  preorder(node = this.root, result = []) {
    if (!node) return result;
    result.push(node.data);
    this.preorder(node.left, result);
    this.preorder(node.right, result);
    return result;
  }

  postorder(node = this.root, result = []) {
    if (!node) return result;
    this.postorder(node.left, result);
    this.postorder(node.right, result);
    result.push(node.data);
    return result;
  }
}

// Contoh penggunaan
const bst = new BinarySearchTree();
[50, 30, 70, 20, 40, 60, 80].forEach((v) => bst.insert(v));

console.log('Inorder (sorted):', bst.inorder());
// [20, 30, 40, 50, 60, 70, 80]

console.log('Preorder:', bst.preorder());
// [50, 30, 20, 40, 70, 60, 80]

console.log('Postorder:', bst.postorder());
// [20, 40, 30, 60, 80, 70, 50]

console.log('Search 60:', bst.search(60)); // true
console.log('Search 99:', bst.search(99)); // false
```

## Analogi Rumah — Struktur Organisasi Perusahaan

| Konsep | Analogi |
|--------|---------|
| Root | CEO — posisi paling atas |
| Node | Setiap posisi di perusahaan |
| Left child | Bawahan di divisi kiri (nilai lebih kecil) |
| Right child | Bawahan di divisi kanan (nilai lebih besar) |
| Leaf | Staff tanpa bawahan |
| Height | Tingkat hierarki dari CEO sampai staff paling bawah |
| Subtree | Satu divisi lengkap dengan manajer dan staffnya |
| Inorder traversal | Urutkan semua karyawan berdasarkan nama A-Z |

## Use Case
- **Autocomplete / dictionary**: BST untuk mencari kata dengan prefix
- **File system**: folder = node, file = leaf
- **Database index**: B-tree (varian BST) di MySQL/PostgreSQL
- **Expression tree**: compiler parsing `a + b * c`

## Kesalahan Umum
- Tidak menyeimbangkan BST — data terurut menghasilkan tree miring (skewed) → O(n) bukan O(log n)
- Lupa handle node null saat traversal → error
- Insert duplikat tanpa strategi (biasanya diabaikan atau dimasukkan ke kanan)

## Benang Merah
Dari Materi 70 (Linked List — linear) → Materi 71 (Tree — hierarkis). Tree adalah generalisasi dari Linked List (setiap node punya multiple pointer). Lanjut ke Materi 72 (Graph) — tree adalah graph tanpa siklus.

## Soal + Jawaban

**1. Apa hasil inorder traversal dari BST yang berisi [10, 5, 15, 3, 7]?**
Jawaban: [3, 5, 7, 10, 15] — inorder selalu terurut ascending.

**2. Kapan BST menjadi tidak efisien (O(n) bukan O(log n))?**
Jawaban: Saat data sudah terurut — tree menjadi miring (seperti linked list). Solusi: AVL Tree atau Red-Black Tree.

**3. Sebutkan 3 jenis traversal dan kegunaannya!**
Jawaban: Inorder (menampilkan data terurut), Preorder (mengkopi tree), Postorder (menghapus tree dari leaf ke root).
