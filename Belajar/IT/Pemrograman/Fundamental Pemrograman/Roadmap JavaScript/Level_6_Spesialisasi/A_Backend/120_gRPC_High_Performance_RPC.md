# 120 — gRPC: High-Performance RPC

## 1. Penjelasan

gRPC adalah framework RPC open-source dari Google. Menggunakan HTTP/2 dan Protocol Buffers (protobuf) untuk serialisasi binary.

| Karakteristik | REST (JSON) | gRPC (Protobuf) |
|--------------|-------------|-----------------|
| Format data | JSON (text) | Binary |
| Kecepatan | Lambat (parsing) | Cepat |
| Ukuran payload | Besar | Kecil (30–50% lebih kecil) |
| Kontrak | Opsional (OpenAPI) | Wajib (.proto) |
| Streaming | SSE/WebSocket | Built-in (4 jenis) |

## 2. Fungsi

gRPC mendukung 4 jenis komunikasi:
- **Unary:** Request-response biasa (seperti REST).
- **Server streaming:** Server kirim banyak respons.
- **Client streaming:** Client kirim banyak request.
- **Bidirectional streaming:** Kedua sisi kirim stream.

## 3. Code

```protobuf
// service.proto
syntax = "proto3";

service BukuService {
  rpc GetBuku (GetBukuRequest) returns (Buku);
  rpc ListBuku (Empty) returns (stream Buku); // server streaming
}

message GetBukuRequest {
  int32 id = 1;
}

message Buku {
  int32 id = 1;
  string title = 2;
  string author = 3;
}

message Empty {}
```

```javascript
// Server gRPC (Node.js)
const grpc = require('@grpc/grpc-js')
const protoLoader = require('@grpc/proto-loader')

const packageDef = protoLoader.loadSync('service.proto')
const proto = grpc.loadPackageDefinition(packageDef).BukuService

const server = new grpc.Server()
server.addService(proto.service, {
  GetBuku: (call, callback) => {
    const buku = { id: call.request.id, title: 'Laskar Pelangi', author: 'Andrea Hirata' }
    callback(null, buku)
  },
  ListBuku: (call) => {
    const bukuList = [{ id: 1, title: 'Laskar Pelangi', author: 'Andrea Hirata' }]
    bukuList.forEach(b => call.write(b))
    call.end()
  }
})
server.bindAsync('0.0.0.0:50051', grpc.ServerCredentials.createInsecure(), () => {
  console.log('gRPC running on port 50051')
})
```

```javascript
// Client gRPC
const client = new proto('localhost:50051', grpc.credentials.createInsecure())
client.GetBuku({ id: 1 }, (err, response) => {
  console.log(response)
})
```

## 4. Analogi Rumah

| Komunikasi | Analogi Rumah |
|------------|---------------|
| REST (JSON) | Ngobrol pake bahasa sehari-hari — verbose, banyak kata |
| gRPC (Protobuf) | Kode morse — ringkas, cepat, perlu decoder |
| HTTP/2 | Jalan tol multi-lane — banyak data sekaligus tanpa antre |
| .proto file | Kamus morse — definisi kode yang disepakati |

## 5. Use Case

- **Microservices internal:** Komunikasi antar service dengan latency rendah.
- **Real-time streaming:** Feed harga saham, live score.
- **Mobile (bandwidth rendah):** Data binary lebih kecil dari JSON.

## 6. Kesalahan Umum

- **Tidak pakai protobuf untuk publik:** gRPC tidak se-fleksibel REST untuk konsumen eksternal.
- **Lupa handle error:** gRPC punya status code sendiri (gRPC-status).
- **Deadline tidak diset:** Request gRPC bisa menggantung selamanya — selalu set deadline/timeout.

## 7. Benang Merah

**119 (GraphQL)** memberikan fleksibilitas query → **120 (gRPC)** memberikan performa tinggi untuk komunikasi antar service. Keduanya kritis untuk **121 (Microservices)**.

## 8. Soal & Jawaban

### Soal 1
Apa keunggulan utama gRPC dibanding REST untuk komunikasi antar service?

<details>
<summary>Jawaban</summary>
Binary serialization (Protobuf) lebih kecil dan cepat dari JSON. HTTP/2 memungkinkan multiplexing dan streaming bidirectional. Kontrak .proto ketat menghindari error tipe data.
</details>

### Soal 2
Sebutkan 4 jenis komunikasi gRPC.

<details>
<summary>Jawaban</summary>
Unary (1 req, 1 res), Server streaming (1 req, banyak res), Client streaming (banyak req, 1 res), Bidirectional streaming (banyak req, banyak res).
</details>

### Soal 3
Kapan SEBAIKNYA tidak menggunakan gRPC?

<details>
<summary>Jawaban</summary>
Saat API perlu diakses browser langsung (gRPC-web masih terbatas), atau saat konsumen eksternal membutuhkan fleksibilitas REST tanpa kontrak protobuf ketat.
</details>
