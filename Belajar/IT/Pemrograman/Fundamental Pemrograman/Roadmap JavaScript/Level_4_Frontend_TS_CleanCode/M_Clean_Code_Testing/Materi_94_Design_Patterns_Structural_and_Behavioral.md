# Materi 94: Design Patterns — Structural & Behavioral

## 1. Penjelasan
Structural patterns mengatur komposisi class/object (Adapter, Decorator, Facade). Behavioral patterns mengatur komunikasi antar object (Observer, Strategy, Command).

## 2. Fungsi
- Structural: Membuat class/object bekerja bersama secara fleksibel
- Behavioral: Mendefinisikan pola interaksi yang jelas antar komponen

## 3. Code

```typescript
// --- ADAPTER (Structural) ---
interface USBTypeC {
  charge(): string;
}
class USBTypeCCharger implements USBTypeC {
  charge() { return "Charging via USB-C"; }
}

// Perangkat lama hanya punya USB-A
class MicroUSBDevice {
  chargeOld() { return "Charging via Micro-USB"; }
}

// Adapter
class USBAdapter implements USBTypeC {
  constructor(private device: MicroUSBDevice) {}
  charge() { return this.device.chargeOld() + " (adapted to USB-C)"; }
}

// --- OBSERVER (Behavioral) ---
interface Observer {
  update(event: string): void;
}

class EmailNotifier implements Observer {
  constructor(private email: string) {}
  update(event: string) {
    console.log(`Email to ${this.email}: ${event}`);
  }
}

class BookReturnNotifier {
  private observers: Observer[] = [];

  subscribe(obs: Observer) { this.observers.push(obs); }
  unsubscribe(obs: Observer) {
    this.observers = this.observers.filter(o => o !== obs);
  }

  notifyBookReturned(bookTitle: string) {
    this.observers.forEach(o => o.update(`Book returned: ${bookTitle}`));
  }
}

// Penggunaan Observer
const notifier = new BookReturnNotifier();
notifier.subscribe(new EmailNotifier("user@example.com"));
notifier.notifyBookReturned("Clean Code");

// --- STRATEGY (Behavioral) ---
interface SortingStrategy {
  sort(data: number[]): number[];
}

class BubbleSort implements SortingStrategy {
  sort(data: number[]) {
    return [...data].sort((a, b) => a - b); // simplified
  }
}
class QuickSort implements SortingStrategy {
  sort(data: number[]) {
    return [...data].sort((a, b) => a - b); // simplified
  }
}

class Sorter {
  constructor(private strategy: SortingStrategy) {}
  setStrategy(s: SortingStrategy) { this.strategy = s; }
  sort(data: number[]) { return this.strategy.sort(data); }
}
```

## 4. Analogi Rumah

| Pattern | Analogi Rumah |
|---------|--------------|
| **Adapter** | Colokan listrik converter — stopkontak Eropa (USB-C) → colokan Indonesia (Micro-USB) |
| **Decorator** | Lemari polos + cat + hiasan — fungsi tetap menyimpan baju, tapi tampilan bertambah |
| **Facade** | Remote TV — satu tombol "nyalakan" yang menjalankan: alirkan listrik, aktifkan panel, cari saluran (sembunyikan kompleksitas) |
| **Observer** | Bel pintu — semua orang di rumah dengar, masing-masing bereaksi sesuai peran |
| **Strategy** | Pilih bor sesuai material: bor kayu, bor beton, bor besi — beda strategi, satu tujuan (melubangi) |
| **Command** | Remote universal — setiap tombol adalah perintah yang bisa diprogram ulang |

## 5. Use Case
- Adapter: Integrasi sistem perpustakaan lama dengan API REST baru
- Observer: Sistem notifikasi — buku dikembalikan → kirim email + SMS + in-app notification
- Strategy: Laporan perpustakaan — user pilih format PDF/CSV/Excel tanpa mengubah logika bisnis

## 6. Kesalahan Umum
- Adapter: Membuat adapter padahal bisa langsung refactor class lama
- Decorator: Terlalu banyak decorator sehingga sulit dilacak (callback hell)
- Observer: Lupa unsubscribe → memory leak
- Strategy: Berganti strategi terlalu sering di runtime (padahal tidak perlu dinamis)
- Command: Command pattern untuk operasi sederhana yang cukup dipanggil langsung

## 7. Benang Merah
Dari Materi 93 (creational patterns — cara buat object). Sekarang structural (komposisi) dan behavioral (komunikasi). Lanjut ke Materi 95 (Refactoring & Technical Debt).

## 8. Soal

### Soal 1
Buat studi kasus: kapan Observer pattern cocok digunakan di aplikasi perpustakaan?
**Jawaban:** Saat buku dipinjam/dikembalikan, sistem perlu memberi tahu beberapa komponen: 1) EmailService kirim konfirmasi, 2) LoggerService catat aktivitas, 3) DashboardService update statistik. Observer memisahkan notifier dari observers.

### Soal 2
Apa perbedaan Adapter dan Decorator?
**Jawaban:** Adapter mengubah interface lama agar cocok dengan interface baru (kompatibilitas). Decorator menambah fungsionalitas tanpa mengubah interface asli.

### Soal 3
Implementasi sederhana Strategy pattern untuk menghitung denda keterlambatan (flat vs progressive).
**Jawaban:**
```typescript
interface FineStrategy {
  calculate(daysLate: number): number;
}
class FlatFine implements FineStrategy {
  calculate(days: number) { return days * 1000; }
}
class ProgressiveFine implements FineStrategy {
  calculate(days: number) {
    return days <= 7 ? days * 500 : 7 * 500 + (days - 7) * 1000;
  }
}
class FineCalculator {
  constructor(private strategy: FineStrategy) {}
  setStrategy(s: FineStrategy) { this.strategy = s; }
  calc(days: number) { return this.strategy.calculate(days); }
}
```
