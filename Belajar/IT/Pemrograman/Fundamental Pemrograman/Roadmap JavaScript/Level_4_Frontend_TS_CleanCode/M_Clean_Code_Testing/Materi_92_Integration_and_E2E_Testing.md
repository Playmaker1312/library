# Materi 92: Integration & E2E Testing

## 1. Penjelasan
Integration test menguji beberapa unit bekerja bersama (misal: API endpoint + database). E2E (End-to-End) test menguji seluruh sistem dari sudut pandang user — simulasi klik, input, navigasi. Tools: Supertest (API), Playwright (E2E).

## 2. Fungsi
- Integration: Pastikan komponen saling terhubung dengan benar
- E2E: Pastikan user flow berfungsi dari awal sampai akhir
- Menangkap bug yang tidak terdeteksi unit test (misal: kesalahan format data antar modul)

## 3. Code

```typescript
// backend/__tests__/api.integration.test.ts
import { describe, it, expect } from "vitest";
import request from "supertest";
import { app } from "../app";

describe("POST /api/books (Integration)", () => {
  it("creates a new book and returns 201", async () => {
    const res = await request(app)
      .post("/api/books")
      .send({ title: "Clean Code", author: "Robert Martin", year: 2008 });

    expect(res.status).toBe(201);
    expect(res.body.title).toBe("Clean Code");
    expect(res.body).toHaveProperty("id");
  });

  it("returns 400 for missing title", async () => {
    const res = await request(app)
      .post("/api/books")
      .send({ author: "Robert Martin" });

    expect(res.status).toBe(400);
  });
});
```

```typescript
// e2e/borrow-book.spec.ts — Playwright E2E
import { test, expect } from "@playwright/test";

test("user borrows a book successfully", async ({ page }) => {
  await page.goto("http://localhost:3000");
  await page.click("text=Login");
  await page.fill("[name=email]", "user@test.com");
  await page.fill("[name=password]", "password123");
  await page.click("button:has-text('Login')");

  await page.click("text=Clean Code");
  await page.click("button:has-text('Borrow')");

  await expect(page.locator(".alert-success")).toContainText("Book borrowed");
});
```

## 4. Analogi Rumah

| Jenis Test | Analogi Rumah |
|-----------|---------------|
| Unit test | Uji bata sendiri: kuat tekan 100kg |
| Integration test | Tes pintu + engsel + kunci bekerja bersama: pasang, buka, tutup |
| E2E test | Simulasi orang masuk rumah: buka pintu, nyalakan lampu, duduk di sofa, buka lemari |
| API test | Tes apakah kran dapur mengalirkan air saat diputar |
| Playwright | Robot yang masuk rumah dan melakukan semua aktivitas penghuni |

## 5. Use Case
Fitur "pinjam buku" di perpustakaan: unit test menguji `BorrowService.borrow()` secara terisolasi. Integration test menguji endpoint `POST /api/borrow` dengan database nyata. E2E test menguji user login → cari buku → klik pinjam → lihat konfirmasi.

## 6. Kesalahan Umum
- Integration test menggunakan database in-memory yang berbeda dari produksi
- E2E test terlalu lambat sehingga jarang dijalankan
- Test bergantung pada data yang sudah ada (flaky test)
- Mocking database di integration test — padahal gunanya justru menguji koneksi nyata
- Tidak membersihkan data test setelah selesai (polluting database)

## 7. Benang Merah
Dari Materi 91 (unit test — test terkecil). Sekarang test yang lebih besar: integrasi antar komponen, lalu simulasi user penuh. Lanjut ke Materi 93 (Design Patterns — Creational).

## 8. Soal

### Soal 1
Apa perbedaan utama integration test dan E2E test?
**Jawaban:** Integration test menguji beberapa modul bekerja sama (misal: controller + database). E2E test menguji seluruh sistem dari UI user — termasuk frontend, backend, database, dan jaringan.

### Soal 2
Mengapa E2E test sering disebut "flaky" (tidak stabil)?
**Jawaban:** Karena E2E test bergantung pada banyak faktor: jaringan, waktu rendering, state browser, API rate limit, dan data di database. Salah satu faktor saja berubah, test bisa gagal padahal kode benar.

### Soal 3
Tulis integration test sederhana untuk GET /api/books yang mengembalikan daftar buku.
**Jawaban:**
```typescript
import request from "supertest";
import { app } from "../app";

it("GET /api/books returns book list", async () => {
  const res = await request(app).get("/api/books");
  expect(res.status).toBe(200);
  expect(Array.isArray(res.body)).toBe(true);
});
```
