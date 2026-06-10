# Materi 93: Design Patterns — Creational

## 1. Penjelasan
Creational patterns mengatur bagaimana object dibuat. Tiga utama: Singleton (satu instance global), Factory (buat object tanpa spesifik class), Builder (bangun object kompleks step by step).

## 2. Fungsi
- Singleton: Mengontrol akses ke resource bersama (koneksi database, konfigurasi)
- Factory: Menyembunyikan logika pembuatan object yang kompleks
- Builder: Memudahkan pembuatan object dengan banyak parameter opsional

## 3. Code

```typescript
// --- SINGLETON ---
class DatabaseConnection {
  private static instance: DatabaseConnection;

  private constructor() {
    // connect ke database
  }

  static getInstance(): DatabaseConnection {
    if (!DatabaseConnection.instance) {
      DatabaseConnection.instance = new DatabaseConnection();
    }
    return DatabaseConnection.instance;
  }

  query(sql: string): any {
    return `Result for: ${sql}`;
  }
}

// Penggunaan — selalu object yang sama
const db1 = DatabaseConnection.getInstance();
const db2 = DatabaseConnection.getInstance();
console.log(db1 === db2); // true

// --- FACTORY ---
interface Report {
  generate(): string;
}

class PDFReport implements Report {
  generate() { return "PDF report"; }
}
class CSVReport implements Report {
  generate() { return "CSV report"; }
}

class ReportFactory {
  static create(type: "pdf" | "csv"): Report {
    switch (type) {
      case "pdf": return new PDFReport();
      case "csv": return new CSVReport();
    }
  }
}

const report = ReportFactory.create("pdf");
console.log(report.generate()); // "PDF report"

// --- BUILDER ---
class HousePlan {
  walls?: number;
  doors?: number;
  windows?: number;
  hasGarage?: boolean;
  hasPool?: boolean;

  describe() {
    return `${this.walls} walls, ${this.doors} doors, garage: ${this.hasGarage}`;
  }
}

class HouseBuilder {
  private house: HousePlan;
  constructor() { this.house = new HousePlan(); }
  setWalls(n: number) { this.house.walls = n; return this; }
  setDoors(n: number) { this.house.doors = n; return this; }
  setWindows(n: number) { this.house.windows = n; return this; }
  addGarage() { this.house.hasGarage = true; return this; }
  addPool() { this.house.hasPool = true; return this; }
  build() { return this.house; }
}

const myHouse = new HouseBuilder()
  .setWalls(8)
  .setDoors(3)
  .addGarage()
  .build();

console.log(myHouse.describe());
```

## 4. Analogi Rumah

| Pattern | Analogi Rumah |
|---------|--------------|
| Singleton | Satu panel listrik utama untuk seluruh rumah — hanya satu, semua ruangan pakai yang sama |
| Factory | Toko material yang buat barang sesuai pesanan — minta "pintu 2m x 0.8m", toko buatkan tanpa kita tahu cara buatnya |
| Builder | Arsitek bangun rumah step by step: fondasi → dinding → atap → cat |
| Anti-Singleton | Setiap kamar punya panel listrik sendiri — boros, kacau |
| Factory tanpa Factory | Kita langsung beli kayu, palu, paku, dan bikin pintu sendiri (tight coupling) |

## 5. Use Case
- Singleton: `DatabaseConnection.getInstance()` — semua service pakai koneksi DB yang sama
- Factory: `ReportFactory.create("pdf")` — aplikasi perpustakaan bisa generate laporan dalam berbagai format tanpa controller tahu detailnya
- Builder: Query builder TypeORM — `queryBuilder.select().from().where().execute()`

## 6. Kesalahan Umum
- Singleton: Digunakan berlebihan — menyembunyikan dependency, sulit di-test (global state)
- Factory: Bikin factory padahal hanya satu jenis object (over-engineering)
- Builder: Tidak menggunakan builder untuk object sederhana (2-3 parameter)
- Lupa: Singleton bukan solusi untuk semua masalah — pertimbangkan dependency injection

## 7. Benang Merah
Dari Materi 92 (testing). Design patterns membantu struktur kode yang sudah clean dan ter-test. Lanjut ke Materi 94 (Structural & Behavioral Patterns).

## 8. Soal

### Soal 1
Kapan waktu yang tepat menggunakan Singleton? Beri contoh.
**Jawaban:** Saat kita butuh tepat satu instance global dari suatu resource, misal: koneksi database, file system, logger, atau konfigurasi aplikasi.

### Soal 2
Apa keuntungan Factory pattern dibanding `new` langsung?
**Jawaban:** Factory menyembunyikan logika pembuatan, memungkinkan return subtype berbeda, dan memusatkan perubahan (jika cara pembuatan berubah, cukup ubah factory, bukan semua pemanggil).

### Soal 3
Refactor kode berikut menggunakan Builder pattern:
```typescript
const book = {
  title: "Clean Code",
  author: "Robert Martin",
  year: 2008,
  pages: 464,
  isbn: "12345",
};
```
**Jawaban:**
```typescript
class BookBuilder {
  private book: any = {};
  setTitle(t: string) { this.book.title = t; return this; }
  setAuthor(a: string) { this.book.author = a; return this; }
  setYear(y: number) { this.book.year = y; return this; }
  setPages(p: number) { this.book.pages = p; return this; }
  setIsbn(i: string) { this.book.isbn = i; return this; }
  build() { return this.book; }
}
const book = new BookBuilder()
  .setTitle("Clean Code").setAuthor("Robert Martin")
  .setYear(2008).setPages(464).setIsbn("12345").build();
```
