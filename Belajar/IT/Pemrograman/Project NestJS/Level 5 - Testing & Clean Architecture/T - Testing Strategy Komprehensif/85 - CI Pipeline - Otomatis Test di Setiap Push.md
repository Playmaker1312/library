# 85 - CI Pipeline - Otomatis Test di Setiap Push

## Penjelasan

Setelah kita memiliki unit test (bab 82), integration test (bab 83), dan E2E test (bab 84), semua test itu tidak ada artinya jika tidak dijalankan secara **otomatis**. Di bab ini, kita akan mengkonfigurasi **CI/CD pipeline** menggunakan **GitHub Actions** sehingga setiap kali developer melakukan push atau pull request, seluruh rangkaian test dijalankan secara otomatis.

Pipeline yang baik terdiri dari beberapa stage:
1. **Lint** — memastikan kode mengikuti style guide
2. **Unit Test** — cepat, untuk feedback cepat
3. **Integration Test** — membutuhkan database (PostgreSQL + Redis service)
4. **E2E Test** — membutuhkan seluruh service
5. **Build** — memastikan aplikasi bisa di-compile
6. **Coverage Report** — dikirim ke Codecov; jika di bawah threshold, pipeline gagal

## Fungsi

- Menjalankan **lint → unit → integration → e2e → build** secara otomatis di setiap push
- Menggunakan **GitHub Actions service containers** untuk PostgreSQL dan Redis (tanpa Testcontainers, karena runner GitHub tidak mendukung Docker-in-Docker dengan mudah)
- Mengirim **coverage report** ke Codecov
- **Fail pipeline** jika coverage di bawah threshold (sesuai konfigurasi Jest di bab 81)
- Menyediakan **matrix testing** untuk beberapa versi Node.js dan PostgreSQL

## Cara Pengimplementasian

### 1. GitHub Actions Workflow — Complete Pipeline

```yaml
# .github/workflows/test.yml
name: CI Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  NODE_VERSION: '20.x'
  POSTGRES_VERSION: '16-alpine'
  REDIS_VERSION: '7-alpine'

jobs:
  lint:
    name: Lint
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - run: npm ci
      - run: npm run lint
        env:
          ESLINT_USE_FLAT_CONFIG: 'false'

  unit-test:
    name: Unit Tests
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - run: npm ci
      - run: npm run test:unit -- --coverage
        env:
          CI: true

      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v4
        with:
          token: ${{ secrets.CODECOV_TOKEN }}
          directory: ./coverage
          flags: unit
          name: unit-coverage

  integration-test:
    name: Integration Tests
    runs-on: ubuntu-latest
    needs: lint
    services:
      postgres:
        image: postgres:${{ env.POSTGRES_VERSION }}
        env:
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
          POSTGRES_DB: test_db
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

      redis:
        image: redis:${{ env.REDIS_VERSION }}
        ports:
          - 6379:6379
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - run: npm ci

      - name: Run migrations
        run: npx prisma migrate deploy
        env:
          DATABASE_URL: postgresql://test:test@localhost:5432/test_db

      - name: Run integration tests
        run: npm run test:integration -- --coverage
        env:
          DATABASE_URL: postgresql://test:test@localhost:5432/test_db
          REDIS_URL: redis://localhost:6379
          CI: true

      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v4
        with:
          token: ${{ secrets.CODECOV_TOKEN }}
          directory: ./coverage
          flags: integration
          name: integration-coverage

  e2e-test:
    name: E2E Tests
    runs-on: ubuntu-latest
    needs: [unit-test, integration-test]
    services:
      postgres:
        image: postgres:${{ env.POSTGRES_VERSION }}
        env:
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
          POSTGRES_DB: test_db
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

      redis:
        image: redis:${{ env.REDIS_VERSION }}
        ports:
          - 6379:6379
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - run: npm ci

      - name: Run migrations
        run: npx prisma migrate deploy
        env:
          DATABASE_URL: postgresql://test:test@localhost:5432/test_db

      - name: Build app
        run: npm run build
        env:
          DATABASE_URL: postgresql://test:test@localhost:5432/test_db

      - name: Run E2E tests
        run: npm run test:e2e -- --coverage
        env:
          DATABASE_URL: postgresql://test:test@localhost:5432/test_db
          REDIS_URL: redis://localhost:6379
          JWT_SECRET: test-secret
          CI: true

      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v4
        with:
          token: ${{ secrets.CODECOV_TOKEN }}
          directory: ./coverage
          flags: e2e
          name: e2e-coverage

  build:
    name: Build
    runs-on: ubuntu-latest
    needs: [lint, unit-test, integration-test, e2e-test]
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - run: npm ci
      - run: npm run build

      - name: Store build artifact
        uses: actions/upload-artifact@v4
        with:
          name: dist
          path: dist/

  coverage-check:
    name: Coverage Check
    runs-on: ubuntu-latest
    needs: [unit-test, integration-test]
    if: always()
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - run: npm ci

      - name: Download all coverage reports
        uses: actions/download-artifact@v4
        with:
          pattern: coverage-*

      - name: Check coverage thresholds
        run: |
          npx jest --coverage --coverageThreshold='{
            "global": {
              "branches": 80,
              "functions": 80,
              "lines": 80,
              "statements": 80
            }
          }' --passWithNoTests false
        env:
          CI: true
```

### 2. Workflow untuk Pull Request dengan Comment

```yaml
# .github/workflows/pr-check.yml
name: PR Check

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  pr-validation:
    name: PR Validation
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: actions/setup-node@v4
        with:
          node-version: '20.x'
          cache: 'npm'

      - run: npm ci

      - name: Check for TODO comments
        run: |
          if grep -r "TODO\|FIXME\|HACK" src/ --include='*.ts' --include='*.html'; then
            echo "Found TODO/FIXME in code. Please resolve before merging."
            exit 1
          fi

      - name: Run lint
        run: npm run lint

      - name: Run tests with coverage
        run: npm run test:unit -- --coverage
        env:
          CI: true

      - name: Comment coverage on PR
        uses: davelosert/vitest-coverage-report-action@v2
        if: always()
        with:
          json-summary-path: ./coverage/coverage-summary.json
```

### 3. Package.json Scripts untuk CI

```json
{
  "scripts": {
    "test": "jest",
    "test:unit": "jest --config jest.unit.config.ts",
    "test:integration": "jest --config jest.integration.config.ts",
    "test:e2e": "jest --config jest.e2e.config.ts",
    "test:ci": "npm run lint && npm run test:unit -- --coverage && npm run test:integration -- --coverage && npm run build",
    "lint": "eslint 'src/**/*.ts'",
    "lint:fix": "eslint 'src/**/*.ts' --fix"
  }
}
```

### 4. Jest Config Terpisah untuk CI

```typescript
// jest.unit.config.ts
import type { Config } from 'jest';

const config: Config = {
  moduleFileExtensions: ['js', 'json', 'ts'],
  rootDir: '.',
  testMatch: ['<rootDir>/src/**/*.spec.ts'],
  testPathIgnorePatterns: ['/node_modules/', '/test/integration/', '/test/e2e/'],
  transform: { '^.+\\.(t|j)s$': 'ts-jest' },
  collectCoverageFrom: ['src/**/*.(t|j)s', '!src/main.ts', '!src/**/*.module.ts'],
  coverageDirectory: './coverage/unit',
  coverageThreshold: {
    global: { branches: 80, functions: 80, lines: 80, statements: 80 },
  },
  testEnvironment: 'node',
};

export default config;
```

```typescript
// jest.e2e.config.ts
import type { Config } from 'jest';

const config: Config = {
  moduleFileExtensions: ['js', 'json', 'ts'],
  rootDir: '.',
  testMatch: ['<rootDir>/test/e2e/**/*.spec.ts'],
  transform: { '^.+\\.(t|j)s$': 'ts-jest' },
  testTimeout: 60_000,
  globalSetup: '<rootDir>/test/e2e/global-setup.ts',
  globalTeardown: '<rootDir>/test/e2e/global-teardown.ts',
  testEnvironment: 'node',
};

export default config;
```

## Analogi — Gedung Bertingkat

| Konsep | Analogi Gedung |
|--------|----------------|
| **CI Pipeline** | Conveyor belt di pabrik yang otomatis memeriksa setiap batu bata yang lewat |
| **Lint Stage** | Pemeriksa apakah bata ukurannya sesuai standar |
| **Unit Test Stage** | Mesin press yang menguji kekuatan satu bata |
| **Integration Test Stage** | Merakit tembok dari bata-bata yang sudah lolos, lalu digoyang |
| **E2E Stage** | Simulasi orang tinggal di rumah yang dibangun dari tembok itu |
| **Coverage Check** | Checklist yang memastikan 80% material sudah diuji |
| **Build Stage** | Mengemas rumah dalam bentuk flatpack untuk dikirim |
| **Codecov** | Papan skor di depan gedung yang menunjukkan berapa persen material sudah diperiksa |

## Dipakai untuk Apa

- **Setiap push ke branch utama** — pipeline berjalan dan memberikan feedback dalam 5-10 menit
- **Pull request** — reviewer bisa melihat apakah test passing sebelum merge
- **Rilis** — pipeline memastikan tidak ada regression sebelum deploy ke production
- **Onboarding developer baru** — developer bisa langsung push dan tahu apakah kodenya OK

## Kesalahan Umum yang Sering Terjadi

1. **Service containers tidak sehat** — pipeline gagal karena PostgreSQL belum siap; gunakan `health-cmd` dan `health-retries`
2. **Hardcode credential** — menaruh password database di file workflow; gunakan GitHub Secrets untuk production, untuk test cukup env inline
3. **Cache tidak dimanfaatkan** — setiap run install `npm ci` dari awal; gunakan `actions/setup-node` dengan `cache: 'npm'`
4. **Stage dependensi tidak benar** — E2E test jalan sebelum migration selesai; atur `needs` dengan benar
5. **Coverage report terlalu besar** — upload seluruh folder coverage tanpa flag; gunakan flags (`unit`, `integration`, `e2e`) agar Codecov bisa menggabungkan

## Soal Latihan

### Soal 1: Setup GitHub Actions Pipeline

Buat workflow GitHub Actions yang:
- Trigger: push ke branch `main` dan `develop`, serta pull request ke `main`
- Menjalankan lint terlebih dahulu
- Menjalankan unit test
- Jika lint dan unit test lolos, jalankan integration test dengan PostgreSQL service container
- Kirim coverage report ke Codecov
- Jika semua berhasil, jalankan build

### Jawaban 1:

```yaml
name: CI Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20.x'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint

  test:
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20.x'
          cache: 'npm'
      - run: npm ci
      - run: npm run test:unit -- --coverage
        env:
          CI: true
      - uses: codecov/codecov-action@v4
        with:
          token: ${{ secrets.CODECOV_TOKEN }}
          flags: unit

  integration:
    runs-on: ubuntu-latest
    needs: test
    services:
      postgres:
        image: postgres:16-alpine
        env:
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
          POSTGRES_DB: test_db
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20.x'
          cache: 'npm'
      - run: npm ci
      - run: npx prisma migrate deploy
        env:
          DATABASE_URL: postgresql://test:test@localhost:5432/test_db
      - run: npm run test:integration -- --coverage
        env:
          DATABASE_URL: postgresql://test:test@localhost:5432/test_db
          CI: true
      - uses: codecov/codecov-action@v4
        with:
          token: ${{ secrets.CODECOV_TOKEN }}
          flags: integration

  build:
    runs-on: ubuntu-latest
    needs: integration
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20.x'
          cache: 'npm'
      - run: npm ci
      - run: npm run build
```
