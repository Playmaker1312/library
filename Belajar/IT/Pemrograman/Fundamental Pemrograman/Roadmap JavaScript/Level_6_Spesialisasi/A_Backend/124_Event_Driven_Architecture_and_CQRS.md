# 124 — Event-Driven Architecture & CQRS

## 1. Penjelasan

**Event-Driven Architecture (EDA):** Sistem bereaksi terhadap event — publish event → consumer merespons.

**CQRS (Command Query Responsibility Segregation):** Pisahkan model untuk menulis (Command) dan membaca (Query).

**Event Sourcing:** Simpan semua event (perubahan state), bukan state akhir.

## 2. Fungsi

| Konsep | Fungsi |
|--------|--------|
| Event-Driven | Decoupling — producer tidak tahu consumer |
| CQRS | Optimasi — command pakai database normalisasi, query pakai denormalisasi (read model) |
| Event Sourcing | Audit trail lengkap — bisa replay event untuk rekonstruksi state |

## 3. Code

```javascript
// Event Sourcing — simpan event, bukan state
class EventStore {
  constructor() {
    this.events = []
  }

  append(streamId, event) {
    this.events.push({ streamId, ...event, timestamp: new Date() })
  }

  getEvents(streamId) {
    return this.events.filter(e => e.streamId === streamId)
  }
}

// Rekonstruksi state dari event
function buildState(events) {
  let state = { saldo: 0, bukuDipinjam: [] }
  for (const event of events) {
    switch (event.type) {
      case 'BUKU_DIPINJAM':
        state.bukuDipinjam.push(event.bukuId)
        break
      case 'BUKU_DIKEMBALIKAN':
        state.bukuDipinjam = state.bukuDipinjam.filter(b => b !== event.bukuId)
        break
    }
  }
  return state
}

// CQRS — Command
app.post('/api/pinjam', async (req, res) => {
  const { userId, bukuId } = req.body
  eventStore.append(`user:${userId}`, { type: 'BUKU_DIPINJAM', bukuId })
  // Update read model (denormalized)
  await dbReadModel.upsert({
    where: { userId_bukuId: { userId, bukuId } },
    create: { userId, bukuId, status: 'DIPINJAM' },
    update: { status: 'DIPINJAM' }
  })
  res.json({ status: 'ok' })
})

// CQRS — Query (dari read model)
app.get('/api/pinjaman/:userId', async (req, res) => {
  const pinjaman = await dbReadModel.findMany({
    where: { userId: req.params.userId, status: 'DIPINJAM' }
  })
  res.json(pinjaman)
})
```

## 4. Analogi Rumah

| Konsep | Analogi Rumah |
|--------|---------------|
| Event-Driven | Pak RT announce — warga dengar, masing-masing bereaksi sesuai peran |
| CQRS | Pintu masuk terpisah — satu untuk pengiriman barang (command), satu untuk display (query) |
| Event Sourcing | Buku catatan semua transaksi, bukan cuma saldo akhir |
| Read Model | Papan pengumuman yang selalu diperbarui dari catatan transaksi |

## 5. Use Case

- **Event-Driven:** Sistem notifikasi — event `BUKU_DIPINJAM` → trigger email, trigger update stok, trigger log.
- **CQRS:** Dashboard analytics — query model berbeda dari command model (table denormalized).
- **Event Sourcing:** Sistem keuangan — audit trail setiap perubahan saldo.

## 6. Kesalahan Umum

- **CQRS tanpa perlu:** Hampir semua aplikasi bisa pakai CRUD biasa. CQRS untuk skala kompleks.
- **Event Sourcing terlalu besar:** Semua event disimpan — bisa membengkak. Butuh snapshot.
- **Event versioning:** Schema event berubah — event lama tidak bisa diproses. Gunakan upcast/downcast.

## 7. Benang Merah

**123 (API Gateway)** mengatur lalu lintas → **124 (EDA/CQRS)** adalah puncak pola arsitektur backend. Ini menutup **Level 6 Spesialisasi — Backend**. Sebagai penutup, EDA menggabungkan semua konsep sebelumnya: database (116), messaging (117–118), API (119–120), distributed (121–122), dan gateway (123).

## 8. Soal & Jawaban

### Soal 1
Apa perbedaan antara CRUD biasa dan CQRS?

<details>
<summary>Jawaban</summary>
CRUD menggunakan model yang sama untuk baca dan tulis. CQRS memisahkan Command (tulis — validasi ketat) dan Query (baca — optimasi untuk tampilan). Query bisa menggunakan read model denormalized.
</details>

### Soal 2
Apa keunggulan Event Sourcing dibanding menyimpan state langsung?

<details>
<summary>Jawaban</summary>
Audit trail lengkap — bisa lihat history perubahan. Bisa replay event untuk debugging atau rekonstruksi state kapan saja. Memungkinkan temporal query (state pada waktu tertentu).
</details>

### Soal 3
Kapan SEBAIKNYA tidak menggunakan Event-Driven Architecture?

<details>
<summary>Jawaban</summary>
Saat aplikasi sederhana dengan flow linear — EDA menambah kompleksitas (eventual consistency, debugging sulit, butuh infrastructure tambahan).
</details>
