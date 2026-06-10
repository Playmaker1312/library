# 122 — Distributed Systems: CAP Theorem & Consistency

## 1. Penjelasan

Distributed system adalah sekelompok komputer yang bekerja bersama sebagai satu sistem.

**CAP Theorem** (Eric Brewer): Dalam distributed system, kamu hanya bisa memilih 2 dari 3:
- **C**onsistency: Semua node melihat data yang sama.
- **A**vailability: Setiap request mendapat respons (sukses/error).
- **P**artition Tolerance: Sistem tetap berjalan meski ada node terputus.

## 2. Fungsi

| Konsep | Fungsi |
|--------|--------|
| Strong Consistency | Semua node harus sinkron sebelum respons — pakai Raft/Paxos |
| Eventual Consistency | Node bisa tidak sinkron sementara — akhirnya sama |
| Distributed Consensus | Algoritma untuk menyetujui satu nilai (Raft, Paxos, Zab) |
| Gossip Protocol | Propagasi informasi antar node secara eventual |

## 3. Code

```javascript
// Simulasi CP (Consistency + Partition Tolerance)
// Semua node harus setuju sebelum baca/tulis
class CPNode {
  constructor() {
    this.data = new Map()
    this.locked = false
  }

  async write(key, value) {
    // Blokir semua baca sampai konsensus tercapai
    this.locked = true
    const consensus = await Promise.all([
      this.rpcWrite(key, value),
      this.rpcWrite(key, value), // replica
    ])
    if (consensus.every(Boolean)) {
      this.locked = false
      return true
    }
    // Rollback jika gagal
    return false
  }

  async read(key) {
    // Jika partition terjadi, tidak bisa baca (C > A)
    if (this.locked) throw new Error('Cluster unavailable')
    return this.data.get(key)
  }
}

// Simulasi AP (Availability + Partition Tolerance)
// Setiap node bisa baca/tulis lokal, sinkron eventual
class APNode {
  constructor() {
    this.data = new Map()
  }

  async write(key, value) {
    this.data.set(key, value)
    // Propagasi async — bisa gagal, tetap return sukses
    this.propagateAsync(key, value)
    return true
  }

  async read(key) {
    // Selalu respons — meski data mungkin outdated
    return this.data.get(key) || 'stale data'
  }
}
```

## 4. Analogi Rumah

| CAP Property | Analogi Rumah |
|--------------|---------------|
| Consistency (C) | Semua kamar suhu AC sama |
| Availability (A) | AC selalu nyala di setiap kamar |
| Partition Tolerance (P) | Listrik terpusat — jika listrik padam di satu titik |

**Skenario listrik padam (Partition):**
- Pilih **CP**: Matikan AC di semua kamar agar suhu tetap sama (consistency).
- Pilih **AP**: Biarkan AC menyala, tapi suhu setiap kamar bisa berbeda (available).

## 5. Use Case

- **CP System:** Sistem banking — konsistensi > ketersediaan. Jika ATM offline, jangan tampilkan saldo.
- **AP System:** Media sosial — lebih baik tampilkan feed lama (stale) daripada error.
- **CA (tidak mungkin dalam distributed):** Database standalone — satu node, tidak ada partition.

## 6. Kesalahan Umum

- **Mengira CAP adalah pilihan mutlak:** CAP adalah spektrum. Kamu bisa eventual consistency.
- **Mengabaikan latency:** Consensus (Raft) butuh waktu — tidak cocok untuk write-heavy.
- **P axos terlalu kompleks:** Raft lebih mudah diimplementasikan.

## 7. Benang Merah

**121 (Microservices)** menciptakan distributed system → **122 (CAP Theorem)** menjelaskan trade-off yang harus dibuat. Ini memengaruhi desain **123 (API Gateway)** dan **124 (Event-Driven/CQRS)**.

## 8. Soal & Jawaban

### Soal 1
Jelaskan CAP Theorem dengan kata-kata sendiri.

<details>
<summary>Jawaban</summary>
Dalam distributed system, kamu tidak bisa punya Consistency (data sama), Availability (selalu respons), dan Partition Tolerance (tahan node putus) sekaligus. Kamu harus memilih 2 dari 3.
</details>

### Soal 2
Mengapa sistem banking cenderung memilih CP daripada AP?

<details>
<summary>Jawaban</summary>
Karena konsistensi data (saldo akurat) lebih penting daripada ketersediaan. Lebih baik tampilkan error daripada menampilkan saldo salah yang bisa menyebabkan transaksi ilegal.
</details>

### Soal 3
Apa perbedaan Raft dan Paxos?

<details>
<summary>Jawaban</summary>
Keduanya algoritma distributed consensus. Raft lebih mudah dipahami dengan konsep leader election dan log replication. Paxos lebih kompleks dan sulit diimplementasi.
</details>
