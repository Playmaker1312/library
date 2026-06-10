# 130: Testing Frontend Lanjutan

## 1. Penjelasan
Testing frontend mencakup 3 level: Unit test (komponen individu dengan Vitest + Vue Test Utils), Component test (mount komponen dengan props, stub dependencies, mock API), dan E2E test (simulasi pengguna nyata dengan Playwright — navigasi, klik, assertion, screenshot). Setiap level punya fokus dan trade-off berbeda.

## 2. Fungsi
- Unit test: memverifikasi logika komponen secara terisolasi
- Component test: memverifikasi rendering, interaksi, dan integrasi komponen
- E2E test: memverifikasi flow pengguna dari awal hingga akhir
- Mock API: menghindari ketergantungan backend
- Snapshot/screenshot test: mendeteksi perubahan visual yang tidak diinginkan

## 3. Code

```ts
// Unit test — komponen BookCard
import { mount } from '@vue/test-utils'
import { describe, it, expect } from 'vitest'
import BookCard from '~/components/BookCard.vue'

describe('BookCard', () => {
  const book = { id: 1, title: 'Clean Code', author: 'Robert Martin' }

  it('menampilkan judul dan penulis', () => {
    const wrapper = mount(BookCard, { props: { book } })
    expect(wrapper.text()).toContain('Clean Code')
    expect(wrapper.text()).toContain('Robert Martin')
  })

  it('memancarkan event borrow saat tombol diklik', async () => {
    const wrapper = mount(BookCard, { props: { book } })
    await wrapper.find('button').trigger('click')
    expect(wrapper.emitted('borrow')).toBeTruthy()
    expect(wrapper.emitted('borrow')[0]).toEqual([book])
  })
})
```

```ts
// E2E test — Playwright
import { test, expect } from '@playwright/test'

test('pengguna bisa mencari dan meminjam buku', async ({ page }) => {
  await page.goto('/')

  // Cari buku
  await page.fill('[data-testid="search-input"]', 'Clean Code')
  await page.click('[data-testid="search-button"]')

  // Buku muncul
  await expect(page.locator('[data-testid="book-card"]')).toHaveCount(1)

  // Pinjam buku
  await page.click('[data-testid="borrow-button"]')
  await expect(page.locator('[data-testid="success-toast"]')).toBeVisible()
})
```

```ts
// vitest.config.ts
import { defineConfig } from 'vitest/config'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  test: {
    environment: 'jsdom',
    globals: true,
    setupFiles: ['./test/setup.ts']
  }
})
```

## 4. Analogi Rumah

| Konsep Testing | Analogi Rumah |
|----------------|---------------|
| Unit test | Tes buka-tutup satu pintu — fungsi individu |
| Component test | Tes satu ruangan — lampu, saklar, jendela berfungsi |
| E2E test | Simulasi orang tinggal seminggu — dari masuk, masak, tidur, keluar |
| Mock | Maket furnitur — bukan asli, cukup untuk tes posisi |
| Stub | Pintu palsu — cukup untuk melihat apakah bingkai cocok |
| Snapshot test | Foto rumah setelah jadi — bandingkan dengan foto sebelumnya |

## 5. Use Case
- Komponen kritis (form login, payment) → unit + component test
- Flow multi-step (checkout, booking) → E2E test
- Regresi visual → screenshot test Playwright
- Aplikasi perpustakaan: tes form tambah buku, flow pinjam-kembali, pencarian

## 6. Kesalahan Umum
- **Terlalu banyak unit test, sedikit E2E** → unit test murah tapi tidak deteksi bug integrasi. Seimbangkan (test trophy, bukan pyramid).
- **Test dependen pada API nyata** → gunakan mock/MSW (Mock Service Worker) agar test deterministik.
- **Selector rapuh (class-based)** → gunakan `data-testid` agar test tidak rusak saat styling berubah.

## 7. Benang Merah
a11y (129) perlu di-test otomatis (axe-core di Playwright). Micro-frontend (131) perlu integration test antar fragment. Testing memastikan semua layer sebelumnya (125-129) bekerja dengan benar bersama-sama.

## 8. Soal

**Soal 1:** Apa perbedaan unit test, component test, dan E2E test?

**Soal 2:** Mengapa sebaiknya menggunakan `data-testid` daripada class CSS untuk selector di test?

**Soal 3:** Bagaimana cara mock API di component test dengan Vitest?

---
<details>
<summary>Jawaban</summary>

**Jawaban 1:** Unit test — menguji fungsi/komponen secara terisolasi (cepat, murah). Component test — menguji komponen dengan mount & interaksi (sedang). E2E test — menguji flow pengguna di browser nyata (lambat, mahal, tapi paling akurat).

**Jawaban 2:** `data-testid` tidak berubah saat refaktor CSS atau class name berubah. Ini membuat test lebih stabil dan tidak merusak snapshot styling.

**Jawaban 3:** Gunakan `vi.mock()` dari Vitest untuk mock modul API. Contoh: `vi.mock('~/api', () => ({ fetchBooks: vi.fn().mockResolvedValue(mockBooks) }))`. Atau gunakan MSW (Mock Service Worker) untuk intercept HTTP.
</details>
