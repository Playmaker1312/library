# 70 — Linked List

## Penjelasan
Linked List adalah struktur data linear yang terdiri dari node. Setiap node menyimpan data dan pointer ke node berikutnya. Berbeda dengan array, Linked List unggul dalam insert/delete di tengah (O(1)) tapi lemah dalam akses index acak (O(n)). Ada Singly Linked List (satu arah) dan Doubly Linked List (dua arah).

## Fungsi
- Insert/delete sering di tengah data
- Ukuran data dinamis (tidak perlu alokasi ulang seperti array)
- Implementasi queue, stack, dan graph adjacency list

## Code

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class SinglyLinkedList {
  constructor() {
    this.head = null;
    this.length = 0;
  }

  insertFirst(data) {
    const newNode = new Node(data);
    newNode.next = this.head;
    this.head = newNode;
    this.length++;
  }

  insertLast(data) {
    const newNode = new Node(data);
    if (!this.head) {
      this.head = newNode;
    } else {
      let current = this.head;
      while (current.next) current = current.next;
      current.next = newNode;
    }
    this.length++;
  }

  insertAt(data, index) {
    if (index < 0 || index > this.length) return false;
    if (index === 0) return this.insertFirst(data);
    const newNode = new Node(data);
    let prev = this.head;
    for (let i = 0; i < index - 1; i++) prev = prev.next;
    newNode.next = prev.next;
    prev.next = newNode;
    this.length++;
    return true;
  }

  delete(data) {
    if (!this.head) return false;
    if (this.head.data === data) {
      this.head = this.head.next;
      this.length--;
      return true;
    }
    let current = this.head;
    while (current.next && current.next.data !== data) current = current.next;
    if (!current.next) return false;
    current.next = current.next.next;
    this.length--;
    return true;
  }

  search(data) {
    let current = this.head;
    let index = 0;
    while (current) {
      if (current.data === data) return index;
      current = current.next;
      index++;
    }
    return -1;
  }

  toArray() {
    const result = [];
    let current = this.head;
    while (current) {
      result.push(current.data);
      current = current.next;
    }
    return result;
  }
}

// Contoh penggunaan
const list = new SinglyLinkedList();
list.insertLast('Pondasi');
list.insertLast('Dinding');
list.insertLast('Atap');
list.insertAt('Tiang', 1);
console.log(list.toArray()); // ['Pondasi', 'Tiang', 'Dinding', 'Atap']
console.log(list.search('Atap')); // 3
list.delete('Tiang');
console.log(list.toArray()); // ['Pondasi', 'Dinding', 'Atap']
```

## Analogi Rumah — Rantai

| Konsep | Analogi Rantai |
|--------|----------------|
| Node | Satu mata rantai |
| Pointer (next) | Sambungan ke mata rantai berikutnya |
| Head | Mata rantai pertama |
| Tail | Mata rantai terakhir |
| Singly Linked | Tahu mata rantai berikutnya, tapi tidak tahu sebelumnya |
| Doubly Linked | Tahu mata rantai sebelumnya dan berikutnya |
| Insert tengah | Selingkan mata rantai baru, sambungkan kiri-kanan |

## Use Case
- **Undo/Redo** di editor: Doubly Linked List — setiap state adalah node
- **Music player playlist**: insert/delete lagu di posisi manapun O(1)
- **Hash table chaining**: node-node di bucket yang sama (collision handling)
- **Queue**: Linked List untuk FIFO — enqueue di tail, dequeue di head

## Kesalahan Umum
- Lupa update pointer saat insert/delete → list putus
- Infinite loop karena pointer tidak di-set null di tail
- Tidak handle kasus empty list → akses `head.next` error

## Benang Merah
Dari Materi 69 (sorting — algoritma murni) → Materi 70 (struktur data pertama). Linked List adalah fondasi untuk struktur data yang lebih kompleks. Lanjut ke Materi 71 (Tree) — Linked List adalah tree dengan satu cabang.

## Soal + Jawaban

**1. Apa kelebihan Linked List dibanding array?**
Jawaban: Insert dan delete di tengah O(1) (tanpa shifting). Ukuran dinamis tanpa realokasi.

**2. Apa kekurangan Linked List?**
Jawaban: Akses index acak O(n) (harus traverse dari head). Memori lebih besar karena menyimpan pointer.

**3. Dalam skenario apa Doubly Linked List lebih baik dari Singly Linked List?**
Jawaban: Saat perlu traversal mundur (misal undo/redo, browser back/forward).
