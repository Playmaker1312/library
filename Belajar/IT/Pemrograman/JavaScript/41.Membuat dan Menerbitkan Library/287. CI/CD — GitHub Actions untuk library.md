### Definisi Singkat

**CI/CD (Continuous Integration / Continuous Deployment)** adalah praktik mengotomatiskan build, pengujian, dan deployment kode setiap kali ada perubahan. **GitHub Actions** adalah platform CI/CD bawaan GitHub yang menggunakan file YAML (`.github/workflows/*.yml`) untuk mendefinisikan workflow — sangat populer untuk library JavaScript.

---

### Analogi

Seperti **jalur perakitan otomatis di pabrik** — setiap kali ada desain baru (push ke repo), mesin otomatis memotong bahan (install dependencies), merakit (build), memeriksa kualitas (test), dan jika lolos, mengirim produk ke gudang (publish ke npm). Tidak ada campur tangan manual.

---

### Poin-Poin Penting

- **GitHub Actions**: Workflow berbasis event (push, pull_request, release, schedule).
- **Workflow file**: `.github/workflows/ci.yml` — mendefinisikan trigger, jobs, dan steps.
- **Jobs**: Berjalan di runner (Ubuntu, Windows, macOS). Bisa paralel atau berurutan.
- **Steps**: Perintah atau actions (`actions/checkout`, `actions/setup-node`, `npm ci`, `npm test`).
- **Matrix build**: Uji di beberapa versi Node.js atau OS sekaligus.
- **Secrets**: `secrets.NPM_TOKEN` — token autentikasi, disimpan aman di GitHub.
- **Publish on release**: Trigger saat GitHub Release dibuat → build + test + publish ke npm.

---

### Contoh Implementasi

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [16, 18, 20]

    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'
      - run: npm ci
      - run: npm run lint
      - run: npm test
      - run: npm run build
```

```yaml
# .github/workflows/publish.yml
name: Publish to npm

on:
  release:
    types: [published]

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          registry-url: 'https://registry.npmjs.org'
      - run: npm ci
      - run: npm run build
      - run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

```json
// Tambahkan badge di README
// [![CI](https://github.com/user/repo/actions/workflows/ci.yml/badge.svg)](https://github.com/user/repo/actions/workflows/ci.yml)
```

---

### Koneksi

Berkaitan dengan **Package.json** (skrip lifecycle), **SemVer** (versi rilis), **npm registry** (publish otomatis), **Testing** (test otomatis di CI), **Open Source** (CI menunjukkan kualitas project), dan **GitHub** (platform).
