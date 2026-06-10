# Materi 89: SOLID Principles

## 1. Penjelasan
SOLID adalah lima prinsip desain OOP yang membuat kode lebih mudah dirawat, diperluas, dan diuji. S = Single Responsibility, O = Open/Closed, L = Liskov Substitution, I = Interface Segregation, D = Dependency Inversion.

## 2. Fungsi
- Mencegah kode kaku (rigid) yang sulit diubah
- Memudahkan penambahan fitur tanpa merusak kode lama
- Meningkatkan testability dan reusability

## 3. Code

```typescript
// --- PELANGGARAN SOLID ---
class ReportService {
  generateReport(data: any) {
    // generate report
  }
  sendEmail(report: any) {
    // send email — ini bukan tugas ReportService! (S)
  }
}

// --- REFACTOR (SOLID) ---
// S: Pisahkan tanggung jawab
class ReportGenerator {
  generate(data: any): string {
    return "report content";
  }
}

class EmailService {
  send(to: string, report: string): void {
    // send email
  }
}

// O: Buka untuk ekstensi, tutup untuk modifikasi
interface ReportFormat {
  format(data: any): string;
}
class PDFReport implements ReportFormat {
  format(data: any): string {
    return "PDF formatted report";
  }
}
class CSVReport implements ReportFormat {
  format(data: any): string {
    return "CSV formatted report";
  }
}

// D: Dependency Injection — tergantung abstraksi, bukan konkret
class ReportController {
  constructor(
    private generator: ReportGenerator,
    private email: EmailService,
    private format: ReportFormat
  ) {}
}
```

## 4. Analogi Rumah

| Prinsip | Analogi Rumah |
|---------|--------------|
| S — Single Responsibility | Tukang cat jangan merangkap jadi tukang listrik. Satu orang, satu peran. |
| O — Open/Closed | Rumah bisa ditambah kamar tanpa bongkar dinding yang sudah ada. |
| L — Liskov Substitution | Pintu geser harus bisa menggantikan pintu biasa tanpa mengubah cara buka-tutup. |
| I — Interface Segregation | Jangan paksa penghuni kamar mandi memiliki fungsi "masak" — buat interface kecil dan spesifik. |
| D — Dependency Inversion | Saklar lampu tidak perlu tahu jenis bohlam — cukup tahu "bisa menyala". |

## 5. Use Case
Project perpustakaan: class `LibraryManager` tadinya menangani penyimpanan, pencetakan laporan, dan pengiriman notifikasi. Setelah refactor SOLID, dipisah jadi `BookRepository`, `ReportPrinter`, `NotificationService`.

## 6. Kesalahan Umum
- S: Satu class melakukan semuanya — "God Class" (contoh: `LibraryManager` 500 baris)
- O: Mengubah kode yang sudah berjalan setiap kali mau tambah fitur
- L: Turunan class mengubah perilaku parent (misal: `PersegiPanjang` parent, `Persegi` child merusak logika)
- I: Interface raksasa dengan method yang tidak terpakai (`Animal` dengan method `fly()` untuk `Dog`)
- D: Class `BookService` membuat instance `MySQLRepository` langsung (tight coupling)

## 7. Benang Merah
Dari Materi 88 (clean code — dasar kualitas). SOLID adalah prinsip desain berikutnya. Lanjut ke Materi 90 (DRY, KISS, YAGNI).

## 8. Soal

### Soal 1
Identifikasi pelanggaran S (Single Responsibility):
```typescript
class UserService {
  createUser(data: any) { /* ... */ }
  sendWelcomeEmail(email: string) { /* ... */ }
  logActivity(action: string) { /* ... */ }
}
```
**Jawaban:** `UserService` menangani 3 tanggung jawab: manajemen user, pengiriman email, dan logging. Harus dipisah ke `UserService`, `EmailService`, `LoggerService`.

### Soal 2
Manakah yang melanggar prinsip O (Open/Closed)?
```typescript
function getDiscount(type: string): number {
  if (type === "regular") return 5;
  if (type === "premium") return 10;
  if (type === "vip") return 15;
  return 0;
}
```
**Jawaban:** Setiap kali menambah tipe diskon, fungsi harus diubah. Solusi O: gunakan polimorfisme atau strategy pattern.

### Soal 3
Apa yang dimaksud Dependency Inversion? Beri contoh di rumah.
**Jawaban:** Abstraksi tidak boleh tergantung detail, detail tergantung abstraksi. Contoh rumah: saklar (abstraksi — "bisa mati/nyala") tidak perlu tahu apakah bohlamnya LED atau pijar (detail).
