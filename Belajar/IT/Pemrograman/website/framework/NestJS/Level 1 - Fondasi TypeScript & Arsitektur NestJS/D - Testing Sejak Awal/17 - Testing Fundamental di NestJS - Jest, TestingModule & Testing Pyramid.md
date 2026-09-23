# 17 - Testing Fundamental di NestJS: Jest, TestingModule & Testing Pyramid

## Penjelasan

Sejauh ini kita sudah membangun lantai-lantai gedung: Modul (lemari arsip), Controller (resepsionis), Service (satpam yang mengecek data). Tapi bagaimana kita **memastikan setiap komponen berfungsi sebelum gedung dihuni**? Di sinilah testing berperan. NestJS lahir dengan Jest sebagai mesin testing default dan `TestingModule` untuk mengisolasi komponen yang ingin diuji. Sama seperti arsitek yang menguji model gedung di komputer sebelum membangun sungguhan, kita menguji kode sebelum masuk produksi.

---

## Fungsi

- Memvalidasi logika bisnis sebelum deployment
- Mendeteksi regresi (bug muncul lagi setelah perubahan)
- Mendokumentasikan perilaku kode
- Membantu refactoring dengan aman
- Memastikan tiap komponen (unit) bekerja sendiri-sendiri

---

## Testing Pyramid

```
        /\
       /  \
      /    \        E2E (End-to-End) — sedikit, mahal, lambat
     /______\
    /        \
   /          \    Integration — sedang, integrasi antar modul
  /____________\
 /              \
/                \  Unit Test — banyak, murah, cepat
/__________________\
```

| Level | Analogi Gedung | Contoh |
|---|---|---|
| **Unit Test** | Menguji **satu bata** apakah kuat | Test fungsi utility, service tanpa DB |
| **Integration Test** | Menguji **satu dinding** apakah kokoh | Test controller + service + DB |
| **E2E Test** | Menguji **seluruh gedung** dari pintu masuk sampai kamar mandi | Test HTTP request penuh |

**Rekomendasi**: 70% unit test, 20% integration test, 10% e2e test.

---

## Cara Pengimplementasian

### 1. Setup Jest di NestJS

NestJS sudah include Jest. File konfigurasi `package.json`:

```json
{
  "jest": {
    "moduleFileExtensions": ["js", "json", "ts"],
    "rootDir": "src",
    "testRegex": ".*\\.spec\\.ts$",
    "transform": {
      "^.+\\.(t|j)s$": "ts-jest"
    },
    "collectCoverageFrom": ["**/*.(t|j)s"],
    "coverageDirectory": "../coverage",
    "testEnvironment": "node"
  }
}
```

### 2. `.spec.ts` vs `.e2e-spec.ts`

| File | Tujuan | Lokasi |
|---|---|---|
| `coffee.service.spec.ts` | Unit test service | `src/` (sebelah file asli) |
| `coffee.e2e-spec.ts` | End-to-end test | `test/` (folder terpisah) |

### 3. Arrange-Act-Assert (AAA) Pattern

Setiap test mengikuti tiga langkah:

```typescript
describe('Calculator', () => {
  it('harus menjumlahkan dua angka dengan benar', () => {
    // Arrange — siapkan data
    const calculator = new Calculator();
    const a = 2;
    const b = 3;

    // Act — lakukan aksi
    const result = calculator.add(a, b);

    // Assert — periksa hasil
    expect(result).toBe(5);
  });
});
```

### 4. Test Pertama — Pure Utility Function

```typescript
// src/common/helpers/math.helper.ts
export function add(a: number, b: number): number {
  return a + b;
}

export function multiply(a: number, b: number): number {
  return a * b;
}

export function divide(a: number, b: number): number {
  if (b === 0) throw new Error('Division by zero');
  return a / b;
}
```

```typescript
// src/common/helpers/math.helper.spec.ts
import { add, multiply, divide } from './math.helper';

describe('MathHelper', () => {
  describe('add', () => {
    it('harus menjumlahkan dua angka positif', () => {
      // Arrange
      const a = 5;
      const b = 3;

      // Act
      const result = add(a, b);

      // Assert
      expect(result).toBe(8);
    });

    it('harus menangani angka negatif', () => {
      expect(add(-5, 3)).toBe(-2);
      expect(add(-5, -3)).toBe(-8);
    });
  });

  describe('multiply', () => {
    it('harus mengalikan dua angka', () => {
      expect(multiply(4, 3)).toBe(12);
    });

    it('harus menghasilkan 0 jika salah satu 0', () => {
      expect(multiply(0, 100)).toBe(0);
    });
  });

  describe('divide', () => {
    it('harus membagi dua angka', () => {
      expect(divide(10, 2)).toBe(5);
    });

    it('harus throw error jika dibagi 0', () => {
      expect(() => divide(10, 0)).toThrow('Division by zero');
    });

    it('harus menghasilkan bilangan desimal', () => {
      expect(divide(7, 2)).toBe(3.5);
    });
  });
});
```

### 5. Test dengan Jest Matchers

```typescript
describe('Jest Matchers', () => {
  it('toBe vs toEqual', () => {
    expect(2 + 2).toBe(4);              // ===
    expect({ a: 1 }).toEqual({ a: 1 }); // deep equality
  });

  it('truthiness', () => {
    expect(true).toBeTruthy();
    expect(0).toBeFalsy();
    expect(null).toBeNull();
    expect(undefined).toBeUndefined();
  });

  it('arrays & strings', () => {
    expect([1, 2, 3]).toContain(2);
    expect('hello world').toMatch(/world/);
    expect([{ id: 1 }, { id: 2 }]).toEqual(
      expect.arrayContaining([{ id: 1 }]),
    );
  });

  it('numbers', () => {
    expect(10).toBeGreaterThan(5);
    expect(0.1 + 0.2).toBeCloseTo(0.3); // floating point!
  });
});
```

### 6. Test.createTestingModule

Untuk komponen NestJS (service yang punya dependency), kita butuh TestingModule:

```typescript
import { Test, TestingModule } from '@nestjs/testing';

describe('CoffeeService', () => {
  let service: CoffeeService;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [CoffeeService],
    }).compile();

    service = module.get<CoffeeService>(CoffeeService);
  });

  it('harus terdefinisi', () => {
    expect(service).toBeDefined();
  });
});
```

---

## Analogi: Gedung Bertingkat

| Testing | Analogi Gedung |
|---|---|
| **Unit Test** | Menguji **setiap bata/besi** di laboratorium — apakah bata ini kuat? |
| **Integration Test** | Merakit **dinding** dari bata + semen — apakah merekat? |
| **E2E Test** | **Uji coba penghuni** — buka pintu, nyalakan lampu, siram toilet — semuanya jalan? |
| **Jest** | **Mesin uji material** yang standar di industri |
| **`Test.createTestingModule()`** | **Miniatur ruangan** untuk menguji satu komponen tanpa seluruh gedung |
| **AAA Pattern** | **Prosedur uji**: siapkan bahan → lakukan tes → catat hasil |

---

## Dipakai Untuk Apa

- Validasi logika bisnis (kalkulasi diskon, pajak, dll)
- Memastikan error handling berjalan
- Dokumentasi otomatis perilaku fungsi
- Regression testing sebelum release
- Code review — PR yang baik menyertakan test

---

## Kesalahan Umum

1. **Test tanpa assertion** — test dianggap lolos meski tidak ada `expect()`. Gunakan `expect.hasAssertions()`.
2. **Mock tidak direset** — state mock bocor antar test. Gunakan `beforeEach` untuk reset.
3. **Asynchronous tanpa await** — Promise tidak di-handle, test lolos padahal gagal.
4. **Terlalu banyak mock** — test jadi tidak bermakna karena semuanya di-mock.
5. **Test dependency urutan** — test harus bisa jalan sendiri-sendiri, tidak bergantung urutan.
6. **Menguji implementasi, bukan behavior** — test yang terlalu detil ke internal membuat refactor sulit.

---

## Soal Latihan

### Soal 1
Buat fungsi `formatCurrency(amount: number, currency: string): string` dan tulis 3 unit test:
- Test untuk IDR (format `Rp1.000,00`)
- Test untuk USD (format `$1,000.00`)
- Test untuk amount 0

<details>
<summary>Jawaban</summary>

```typescript
// src/common/helpers/currency.helper.ts
export function formatCurrency(amount: number, currency: string): string {
  const formatter = new Intl.NumberFormat(
    currency === 'IDR' ? 'id-ID' : 'en-US',
    { style: 'currency', currency },
  );
  return formatter.format(amount);
}

// src/common/helpers/currency.helper.spec.ts
import { formatCurrency } from './currency.helper';

describe('formatCurrency', () => {
  it('harus memformat IDR dengan benar', () => {
    const result = formatCurrency(15000, 'IDR');
    expect(result).toBe('Rp15.000,00');
  });

  it('harus memformat USD dengan benar', () => {
    const result = formatCurrency(15000, 'USD');
    expect(result).toBe('$15,000.00');
  });

  it('harus menangani amount 0', () => {
    expect(formatCurrency(0, 'IDR')).toBe('Rp0,00');
    expect(formatCurrency(0, 'USD')).toBe('$0.00');
  });
});
```
</details>

### Soal 2
Buat fungsi `isValidEmail(email: string): boolean` dan tulis 3 unit test:
- Test untuk email valid (`user@example.com`)
- Test untuk email tanpa `@` (`userexample.com`)
- Test untuk email kosong

<details>
<summary>Jawaban</summary>

```typescript
// src/common/helpers/email.helper.ts
export function isValidEmail(email: string): boolean {
  if (!email) return false;
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
}

// src/common/helpers/email.helper.spec.ts
import { isValidEmail } from './email.helper';

describe('isValidEmail', () => {
  it('harus return true untuk email valid', () => {
    expect(isValidEmail('user@example.com')).toBe(true);
    expect(isValidEmail('test.user@domain.co.id')).toBe(true);
  });

  it('harus return false untuk email tanpa @', () => {
    expect(isValidEmail('userexample.com')).toBe(false);
  });

  it('harus return false untuk string kosong', () => {
    expect(isValidEmail('')).toBe(false);
  });
});
```
</details>

### Soal 3
Buat fungsi `calculateDiscount(price: number, discountPercent: number): number` dan tulis 3 unit test:
- Test diskon normal (100.000 diskon 20% = 80.000)
- Test diskon 0%
- Test diskon 100% (return 0)

<details>
<summary>Jawaban</summary>

```typescript
// src/common/helpers/discount.helper.ts
export function calculateDiscount(price: number, discountPercent: number): number {
  if (discountPercent < 0 || discountPercent > 100) {
    throw new Error('Diskon harus antara 0-100');
  }
  return price * (1 - discountPercent / 100);
}

// src/common/helpers/discount.helper.spec.ts
import { calculateDiscount } from './discount.helper';

describe('calculateDiscount', () => {
  it('harus menghitung diskon normal (20%)', () => {
    expect(calculateDiscount(100000, 20)).toBe(80000);
  });

  it('harus return harga asli jika diskon 0%', () => {
    expect(calculateDiscount(50000, 0)).toBe(50000);
  });

  it('harus return 0 jika diskon 100%', () => {
    expect(calculateDiscount(75000, 100)).toBe(0);
  });
});
```
</details>
