# 119 — GraphQL: Alternatif REST API

## 1. Penjelasan

GraphQL adalah query language untuk API yang dikembangkan Facebook.

| Konsep | REST | GraphQL |
|--------|------|---------|
| Data fetching | Over/under-fetching | Ambil persis yang diminta |
| Endpoint | Banyak endpoint (/users, /books) | Satu endpoint /graphql |
| Versioning | /v1/, /v2/ | Tidak perlu — tambah field tanpa hapus lama |

## 2. Fungsi

- **Query:** Mengambil data (GET).
- **Mutation:** Mengubah data (POST/PUT/DELETE).
- **Subscription:** Real-time via WebSocket.
- **Schema + Resolver:** Kontrak tipe data + fungsi pengambilan data.

## 3. Code

```javascript
// Apollo Server Express
const { ApolloServer, gql } = require('apollo-server-express')
const express = require('express')

// Schema
const typeDefs = gql`
  type Book { id: ID!, title: String!, author: Author! }
  type Author { id: ID!, name: String!, books: [Book] }
  type Query {
    books: [Book]
    book(id: ID!): Book
  }
  type Mutation {
    addBook(title: String!, authorId: ID!): Book
  }
`

// Resolver
const resolvers = {
  Query: {
    books: () => db.books.findMany({ include: { author: true } }),
    book: (_, { id }) => db.books.findUnique({ where: { id }, include: { author: true } })
  },
  Mutation: {
    addBook: (_, { title, authorId }) => db.books.create({ data: { title, authorId } })
  }
}

// Server
const server = new ApolloServer({ typeDefs, resolvers })
const app = express()
await server.start()
server.applyMiddleware({ app })
app.listen(4000)
```

```graphql
# Query client — ambil hanya yang dibutuhkan
query {
  books {
    title
    author {
      name
    }
  }
}
```

## 4. Analogi Rumah

| API Approach | Analogi Rumah |
|--------------|---------------|
| REST | Menu paket — dapat nasi+ayam+sayur meski mau ayam saja |
| GraphQL | Prasmanan — ambil yang Anda mau saja |
| Resolver | Koki yang menyiapkan lauk sesuai pesanan |
| Schema | Daftar menu — apa saja yang tersedia |

## 5. Use Case

- **Dashboard dengan banyak view:** Setiap komponen ambil data berbeda dari endpoint sama.
- **Mobile apps:** Bandwidth terbatas — query minimal.
- **BFF (Backend For Frontend):** GraphQL gateway menghubungkan beberapa microservices.

## 6. Kesalahan Umum

- **N+1 problem:** Resolver author dipanggil per buku. Solusi: DataLoader (batch + cache).
- **Query terlalu dalam:** Client bisa nested 10 level — batasi depth dengan `maxDepth`.
- **Subscriptions tidak aman:** Subscription tanpa autentikasi bisa diakses siapa saja.

## 7. Benang Merah

Setelah memahami komunikasi asynchronous via **118 (Message Queue)**, kita perlu cara efisien untuk mengambil data — **119 (GraphQL)**. Lanjut ke **120 (gRPC)** yang lebih performant untuk komunikasi antar service.

## 8. Soal & Jawaban

### Soal 1
Apa masalah utama REST yang dipecahkan GraphQL?

<details>
<summary>Jawaban</summary>
Over-fetching (mendapat data berlebih) dan under-fetching (butuh banyak endpoint untuk satu view). GraphQL memungkinkan client menentukan persis data yang dibutuhkan dalam satu request.
</details>

### Soal 2
Apa itu N+1 problem di GraphQL dan bagaimana solusinya?

<details>
<summary>Jawaban</summary>
Terjadi ketika resolver list mengembalikan N item, lalu masing-masing memicu query database lagi (1+N query). Solusinya menggunakan DataLoader untuk batch dan cache database request.
</details>

### Soal 3
Kapan sebaiknya TIDAK menggunakan GraphQL?

<details>
<summary>Jawaban</summary>
Untuk API sederhana (CRUD dasar), file upload besar, atau caching HTTP yang kompleks — REST lebih sederhana dan matang.
</details>
