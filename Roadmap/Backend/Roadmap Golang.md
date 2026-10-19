# 🗺 Roadmap Golang: Step-by-Step Membangun Aplikasi Nyata

## Filosofi Roadmap Ini

"Go adalah bahasa yang dirancang untuk kesederhanaan, kecepatan, dan concurrency — setiap konsep yang dipelajari ada alasannya, dari syntax sederhana hingga goroutines yang powerful"

---

## Prinsip Desain

- **Satu Project, Tumbuh Bersama**: Sistem manajemen perpustakaan dari CLI → REST API → distributed system
- **Concurrency dari Hari Pertama**: Go terkenal dengan goroutines; ini bukan topik lanjutan
- **Benang Merah Eksplisit**: Setiap langkah terhubung logis
- **Go Modern**: Fokus pada Go 1.21+ features, praktik production-ready

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: "Perpustakaan CLI" — syntax dasar, setup Go
 ↓ (enhance)
Level 2: + Struct, error handling, file I/O → CRUD data buku di file
 ↓ (enhance)
Level 3: + Goroutines, channels → Concurrent CLI app
 ↓ (enhance)
Level 4: + Database MySQL, ORM → Sistem perpustakaan dengan database
 ↓ (enhance)
Level 5: + REST API (http package), testing → API yang robust
 ↓ (enhance)
Level 6: + Framework (Gin/Echo), logging, middleware → Production-ready
 ↓ (pilih jalur)
Level 7: Microservices / gRPC / Advanced patterns
```

---

# 🟢 LEVEL 1: FONDASI GO (Minggu 1-3)

**Tema**: "Dari zero ke program Go pertama yang berjalan"

**Benang Merah**: Go terasa familiar untuk yang sudah tahu bahasa lain, tapi syntax-nya unik

**Output**: Program CLI perpustakaan sederhana yang menampilkan katalog buku

---

## A. Setup & Konsep Dasar Go

### 1. [[1. Mengapa Go — Filosofi dan Karakteristik]]

Go adalah bahasa **compiled** statically-typed dengan syntax simple:

- ✅ Fast compilation, fast execution
- ✅ Built-in concurrency (goroutines, channels)
- ✅ Cross-platform single binary
- ✅ Garbage collected (tidak perlu manual memory)
- ✅ No frameworks "wajib" — standard library sangat kaya

**Perbedaan dari PHP**:

- PHP: interpreted, runtime di server
- Go: compiled binary, tidak perlu runtime

**Langkah konkret**:

Bash

```
# Download dari golang.org
# Verifikasi: 
go version
```

### 2. [[2. Setup Workspace dan Project Pertama]]

Bash

```
# Go 1.21+ menggunakan workspaces, tapi tidak wajib
mkdir -p ~/projects/perpustakaan-go
cd ~/projects/perpustakaan-go

# Init go module
go mod init perpustakaan

# Struktur project:
perpustakaan/
├── main.go
├── go.mod
├── go.sum
├── cmd/
│   └── perpustakaan/
│       └── main.go
└── internal/
    └── models/
        └── buku.go
```

### 3. [[3. Program "Hello, Perpustakaan" Pertama]]

**File: main.go**

Go

```
package main

import (
    "fmt"
)

func main() {
    fmt.Println("Selamat datang di Perpustakaan Digital")
    fmt.Println("Versi: 1.0.0")
}
```

**Jalankan:**

Bash

```
go run main.go
# atau
go build -o perpustakaan && ./perpustakaan
```

**Langkah konkret**: Buat program yang menampilkan welcome message dan statistik perpustakaan sederhana.

---

## B. Syntax Dasar & Tipe Data

### 4. [[4. Variabel, Konstanta, dan Tipe Data]]

Go **strongly typed** — tipe data ditentukan saat compile-time (berbeda dari PHP).

Go

```
package main

import "fmt"

func main() {
    // Deklarasi dengan type eksplisit
    var nama string = "Perpustakaan Digital"
    var kapasitas int = 1000
    var rating float64 = 4.8
    var aktif bool = true
    
    // Short declaration (hanya di dalam fungsi)
    jumlahBuku := 150  // tipe diinfer dari value
    
    // Konstanta (tidak bisa diubah)
    const DENDA_PER_HARI = 1000
    const MAX_PINJAM = 5
    
    fmt.Println(nama)
    fmt.Println("Kapasitas:", kapasitas)
    fmt.Println("Rating:", rating)
    fmt.Printf("Buku ada? %v\n", aktif)
    fmt.Printf("Total: %d\n", jumlahBuku)
}
```

**Tipe Data Go**:

Go

```
// Number types
int, int8, int16, int32, int64  // signed
uint, uint8, uint16, uint32, uint64  // unsigned
float32, float64
complex64, complex128

// String & rune
string, rune (single character), byte

// Boolean
bool

// Composite types (nanti)
array, slice, map, struct, interface
```

**Langkah konkret**: Buat program dengan variabel untuk data perpustakaan (nama, kapasitas, rating, jumlah buku, admin).

### 5. [[5. String Manipulation & Formatting]]

Go

```
package main

import (
    "fmt"
    "strings"
)

func main() {
    judul := "Clean Code"
    pengarang := "Robert Martin"
    harga := 150000
    
    // String concatenation
    info := judul + " by " + pengarang
    
    // Printf (seperti PHP sprintf)
    formatted := fmt.Sprintf("%-30s | %s | Rp %d", judul, pengarang, harga)
    fmt.Println(formatted)
    
    // String functions
    fmt.Println(strings.ToUpper(judul))  // CLEAN CODE
    fmt.Println(strings.ToLower(judul))  // clean code
    fmt.Println(len(judul))               // 10
    fmt.Println(strings.Contains(judul, "Code"))  // true
    
    // Raw string (multi-line)
    deskripsi := `Buku tentang clean code principles:
- Readable
- Maintainable
- Testable`
    fmt.Println(deskripsi)
}
```

**Langkah konkret**: Buat fungsi yang memformat info buku dengan padding dan berbagai format output.

### 6. [[6. Operator dan Kontrol Alir]]

Go

```
package main

import "fmt"

func main() {
    // Operator aritmatika
    harga := 150000
    diskon := 0.10
    hargaFinal := harga * (1 - diskon)  // 135000
    
    // Operator perbandingan (selalu gunakan ==, tidak perlu ===)
    stok := 5
    if stok > 0 {
        fmt.Println("Tersedia")
    } else if stok == 0 {
        fmt.Println("Habis")
    } else {
        fmt.Println("Error")
    }
    
    // Switch (lebih clean dari Go)
    kategori := "Teknologi"
    switch kategori {
    case "Teknologi":
        fmt.Println("💻 Teknologi")
    case "Fiksi":
        fmt.Println("📖 Fiksi")
    default:
        fmt.Println("Kategori lain")
    }
    
    // Loop
    for i := 0; i < 5; i++ {
        fmt.Printf("Buku ke %d\n", i+1)
    }
    
    // Infinite loop dengan break
    x := 0
    for {
        if x > 3 {
            break
        }
        fmt.Println(x)
        x++
    }
}
```

**Langkah konkret**: Program yang menghitung denda, cek ketersediaan buku, dan loop untuk menampilkan katalog.

---

## C. Array dan Slice (Sangat Penting di Go)

### 7. [[7. Array vs Slice — Perbedaan Krusial]]

Go punya dua konsep untuk koleksi: **array** (fixed size) dan **slice** (dynamic).

Go

```
package main

import "fmt"

func main() {
    // Array: fixed size, harus ditentukan di awal
    var kategori [4]string
    kategori[0] = "Fiksi"
    kategori[1] = "Non-Fiksi"
    kategori[2] = "Sains"
    kategori[3] = "Teknologi"
    
    fmt.Println(kategori)     // [Fiksi Non-Fiksi Sains Teknologi]
    fmt.Println(len(kategori)) // 4
    
    // Slice: dynamic size (LEBIH SERING DIGUNAKAN)
    var buku []string  // tanpa size
    buku = append(buku, "Clean Code")
    buku = append(buku, "Design Patterns")
    
    fmt.Println(buku)
    fmt.Println(len(buku))   // 2
    fmt.Println(cap(buku))   // capacity (for efficiency)
    
    // Slice dari array
    subset := kategori[1:3]  // index 1 sampai 2 (3 eksklusif)
    fmt.Println(subset)      // [Non-Fiksi Sains]
    
    // Slice dengan make
    numbers := make([]int, 5, 10)  // length 5, capacity 10
    fmt.Println(len(numbers), cap(numbers))
}
```

**Penting**: Di Go, gunakan **slice** untuk 99% kasus (lebih fleksibel dari array).

**Langkah konkret**: Buat katalog buku menggunakan slice, tambah/hapus buku secara dinamis.

### 8. [[8. Map (Dictionary/Associative Array)]]

Go

```
package main

import "fmt"

func main() {
    // Map: key-value pairs
    buku := map[string]interface{}{
        "id":        1,
        "judul":     "Clean Code",
        "pengarang": "Robert Martin",
        "harga":     150000,
        "stok":      5,
        "tersedia":  true,
    }
    
    fmt.Println(buku["judul"])   // Clean Code
    fmt.Println(buku["harga"])   // 150000
    
    // Tambah key baru
    buku["rating"] = 4.8
    
    // Hapus key
    delete(buku, "rating")
    
    // Cek key ada
    if _, ok := buku["rating"]; ok {
        fmt.Println("Rating ada")
    } else {
        fmt.Println("Rating tidak ada")
    }
    
    // Iterate
    for key, value := range buku {
        fmt.Printf("%s: %v\n", key, value)
    }
}
```

**Langkah konkret**: Buat slice of maps untuk katalog buku, dengan info lengkap tiap buku.

---

## 🏗 Checkpoint Level 1

**Checklist sebelum lanjut ke Level 2:**

text

```
PEMAHAMAN:
├── Bisa jelaskan perbedaan compiled vs interpreted
├── Bisa jelaskan kenapa Go bukan OOP "tradisional"
├── Bisa jelaskan array vs slice vs map dengan contoh
├── Bisa jelaskan goroutine apa itu (basic)
└── Paham error handling dengan multiple return values

PROYEK: Katalog Perpustakaan CLI (versi 1)
├── main.go: program utama
├── Tampilkan 10 buku hardcoded dalam slice of maps
├── Fitur: search buku by judul/pengarang (string matching)
├── Fitur: sort buku by harga ascending/descending
├── Fitur: hitung total harga semua buku
└── Output: formatted table dengan columns

HABIT:
├── Pakai gofmt otomatis (IDE akan handle)
├── Error checking: if err != nil { ... } di setiap I/O
├── Komentari setiap fungsi dengan "// FunctionName ..."
└── Go build sukses tanpa warning
```

**Commit**: `feat: setup Go project with basic CLI and book catalog`

---

# 🔵 LEVEL 2: STRUCT, ERROR HANDLING, FILE I/O (Minggu 3-6)

**Tema**: "Dari hardcoded data ke file-based persistence"

**Benang Merah**: CLI statis → struct untuk organize data → error handling yang proper → baca/tulis file

**Output**: Sistem perpustakaan yang bisa menyimpan data ke file JSON dan membacanya kembali

---

## D. Struct dan Methods

### 9. [[9. Struct — Organize Data Seperti Class (Tapi Bukan)]]

Go **tidak ada inheritance**, tapi punya **struct** (composite types) dan **methods** (fungsi yang attached ke struct).

Go

```
package main

import "fmt"

// Struct adalah type baru
type Buku struct {
    ID        int
    Judul     string
    Pengarang string
    ISBN      string
    Tahun     int
    Harga     float64
    Stok      int
    Kategori  string
}

// Method adalah fungsi dengan "receiver"
// (b Buku) adalah receiver — struct yang method ini operate pada
func (b Buku) Tersedia() bool {
    return b.Stok > 0
}

func (b Buku) FormatHarga() string {
    return fmt.Sprintf("Rp %.0f", b.Harga)
}

// Pointer receiver (bisa modify struct)
func (b *Buku) Pinjam() bool {
    if !b.Tersedia() {
        return false
    }
    b.Stok--
    return true
}

func (b *Buku) Kembalikan() {
    b.Stok++
}

func main() {
    buku := Buku{
        ID:        1,
        Judul:     "Clean Code",
        Pengarang: "Robert Martin",
        ISBN:      "9780132350884",
        Tahun:     2008,
        Harga:     150000,
        Stok:      5,
        Kategori:  "Teknologi",
    }
    
    fmt.Println(buku.Judul)          // Clean Code
    fmt.Println(buku.Tersedia())     // true
    fmt.Println(buku.FormatHarga())  // Rp 150000
    
    buku.Pinjam()
    fmt.Println(buku.Stok)  // 4
    
    buku.Kembalikan()
    fmt.Println(buku.Stok)  // 5
}
```

**Penting**:

- Value receiver (receiver tanpa `*`) — method tidak bisa modify struct
- Pointer receiver (receiver dengan `*`) — method bisa modify struct

**Langkah konkret**: Buat struct Buku, User, Peminjaman dengan methods untuk business logic.

### 10. [[10. Interface — Kontrak Tanpa Inheritance]]

Go punya **interface** (mirip PHP interface) untuk mendefinisikan behavior.

Go

```
package main

import "fmt"

// Interface: kontrak — jika struct implement semua method, auto implement interface
type Pinjamable interface {
    Pinjam() bool
    Kembalikan()
    Tersedia() bool
}

type Buku struct {
    Judul string
    Stok  int
}

func (b *Buku) Pinjam() bool {
    if b.Stok > 0 {
        b.Stok--
        return true
    }
    return false
}

func (b *Buku) Kembalikan() {
    b.Stok++
}

func (b *Buku) Tersedia() bool {
    return b.Stok > 0
}

// Fungsi yang accept interface (polymorphism)
func ProsesPermintaan(item Pinjamable) {
    if item.Pinjam() {
        fmt.Println("Pinjam berhasil")
    } else {
        fmt.Println("Stok habis")
    }
}

func main() {
    buku := &Buku{Judul: "Clean Code", Stok: 5}
    ProsesPermintaan(buku)  // Buku *implement* Pinjamable
}
```

**Langkah konkret**: Buat interface untuk repository operations (Create, Read, Update, Delete).

---

## E. Error Handling — Go Way

### 11. [[11. Multiple Return Values dan Error Pattern]]

Go **tidak punya exceptions** — gunakan **multiple return values** untuk return data + error.

Go

```
package main

import (
    "errors"
    "fmt"
)

// Fungsi yang return (result, error)
func CariBuku(id int, katalog []map[string]interface{}) (map[string]interface{}, error) {
    for _, buku := range katalog {
        if buku["id"] == id {
            return buku, nil  // nil = no error
        }
    }
    return nil, errors.New("buku tidak ditemukan")
}

func main() {
    katalog := []map[string]interface{}{
        {"id": 1, "judul": "Clean Code"},
        {"id": 2, "judul": "Design Patterns"},
    }
    
    // Pattern: assign hasil dan error terpisah
    buku, err := CariBuku(1, katalog)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    
    fmt.Println("Ditemukan:", buku["judul"])
    
    // Error jika tidak ditemukan
    _, err = CariBuku(999, katalog)
    if err != nil {
        fmt.Println("Error:", err)  // Error: buku tidak ditemukan
    }
}
```

**Go Pattern**:

Go

```
// Selalu handle error:
if err != nil {
    // return err (propagate)
    // or log dan continue
}
```

**Langkah konkret**: Buat custom error types untuk domain-specific errors (BukuNotFound, StokHabis, etc).

Go

```
type BukuNotFoundError struct {
    ID int
}

func (e BukuNotFoundError) Error() string {
    return fmt.Sprintf("buku dengan ID %d tidak ditemukan", e.ID)
}
```

### 12. [[12. Panic dan Recover (Untuk Exception Cases)]]

Gunakan `panic` hanya untuk truly unrecoverable situations (bukan untuk business logic errors).

Go

```
package main

import (
    "fmt"
)

func BacaDatabase() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered from panic:", r)
        }
    }()
    
    // Simulasi panic
    panic("Database connection failed")
}

func main() {
    BacaDatabase()
    fmt.Println("Program continues")  // Tetap dijalankan
}
```

**Best Practice**: Gunakan error return values untuk business logic, panic hanya untuk programmer errors.

---

## F. File I/O & Persistence

### 13. [[13. Membaca dan Menulis File]]

Go

```
package main

import (
    "fmt"
    "os"
)

func main() {
    // Tulis ke file
    data := []byte("Hello, Perpustakaan!\n")
    err := os.WriteFile("perpustakaan.txt", data, 0644)
    if err != nil {
        fmt.Println("Error write:", err)
        return
    }
    
    // Baca file
    content, err := os.ReadFile("perpustakaan.txt")
    if err != nil {
        fmt.Println("Error read:", err)
        return
    }
    
    fmt.Println("File content:")
    fmt.Println(string(content))
}
```

**Langkah konkret**: Buat system untuk save/load katalog buku dari text file.

### 14. [[14. JSON Marshaling/Unmarshaling]]

Go punya built-in JSON support — sangat powerful!

Go

```
package main

import (
    "encoding/json"
    "fmt"
    "os"
)

type Buku struct {
    ID        int     `json:"id"`
    Judul     string  `json:"judul"`
    Pengarang string  `json:"pengarang"`
    Harga     float64 `json:"harga"`
    Stok      int     `json:"stok"`
}

func main() {
    // Go struct → JSON (Marshal)
    buku := Buku{
        ID:        1,
        Judul:     "Clean Code",
        Pengarang: "Robert Martin",
        Harga:     150000,
        Stok:      5,
    }
    
    jsonData, err := json.Marshal(buku)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    
    fmt.Println(string(jsonData))
    // {"id":1,"judul":"Clean Code","pengarang":"Robert Martin","harga":150000,"stok":5}
    
    // JSON → Go struct (Unmarshal)
    var bukuLagi Buku
    err = json.Unmarshal(jsonData, &bukuLagi)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    
    fmt.Println(bukuLagi.Judul)  // Clean Code
    
    // Praktis: Save slice of buku ke file JSON
    katalog := []Buku{
        {1, "Clean Code", "Robert Martin", 150000, 5},
        {2, "Design Patterns", "Gang of Four", 200000, 3},
    }
    
    data, _ := json.MarshalIndent(katalog, "", "  ")
    os.WriteFile("katalog.json", data, 0644)
    
    // Load dari file JSON
    fileData, _ := os.ReadFile("katalog.json")
    var loadedKatalog []Buku
    json.Unmarshal(fileData, &loadedKatalog)
    fmt.Println("Loaded:", loadedKatalog)
}
```

**struct tags** adalah penting untuk JSON: `json:"fieldName"`

**Langkah konkret**: Buat repository yang save/load katalog buku dari JSON file.

---

## G. Package Organization

### 15. [[15. Membagi Kode ke dalam Package]]

Go mengorganisir kode dengan **packages** (mirip namespaces).

text

```
perpustakaan/
├── main.go                 # package main
├── go.mod
├── internal/
│   ├── models/
│   │   └── buku.go         # package models
│   ├── repository/
│   │   └── file_repo.go    # package repository
│   └── storage/
│       └── storage.go      # package storage
```

**File: internal/models/buku.go**

Go

```
package models

type Buku struct {
    ID        int
    Judul     string
    Pengarang string
    Harga     float64
    Stok      int
}

func (b *Buku) Pinjam() bool {
    if b.Stok > 0 {
        b.Stok--
        return true
    }
    return false
}
```

**File: internal/repository/file_repo.go**

Go

```
package repository

import (
    "encoding/json"
    "os"
    "perpustakaan/internal/models"
)

type FileRepository struct {
    filepath string
}

func NewFileRepository(filepath string) *FileRepository {
    return &FileRepository{filepath: filepath}
}

func (r *FileRepository) LoadBuku() ([]models.Buku, error) {
    data, err := os.ReadFile(r.filepath)
    if err != nil {
        return nil, err
    }
    
    var buku []models.Buku
    err = json.Unmarshal(data, &buku)
    return buku, err
}

func (r *FileRepository) SaveBuku(buku []models.Buku) error {
    data, _ := json.MarshalIndent(buku, "", "  ")
    return os.WriteFile(r.filepath, data, 0644)
}
```

**File: main.go**

Go

```
package main

import (
    "fmt"
    "perpustakaan/internal/models"
    "perpustakaan/internal/repository"
)

func main() {
    repo := repository.NewFileRepository("katalog.json")
    
    buku, err := repo.LoadBuku()
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    
    fmt.Println("Total buku:", len(buku))
    for _, b := range buku {
        fmt.Printf("%s by %s (Rp %.0f)\n", b.Judul, b.Pengarang, b.Harga)
    }
}
```

**Best Practice**: Simpan business logic di `internal/`, keep `main.go` clean.

---

## 🏗 Checkpoint Level 2

text

```
PEMAHAMAN:
├── Bisa jelaskan value receiver vs pointer receiver
├── Bisa jelaskan error pattern di Go
├── Bisa jelaskan interface (implicit implementation)
└── Bisa marshal/unmarshal JSON

PROYEK: File-based Perpustakaan Repository
├── Struct: Buku, User, Peminjaman
├── Repository: LoadBuku, SaveBuku, AddBuku, UpdateBuku, DeleteBuku
├── Business logic di Buku methods (Pinjam, Kembalikan, tersedia, dll)
├── Simpan/load dari katalog.json
├── Package organization: internal/models, internal/repository
├── CRUD buku dari CLI interface
└── Error handling proper di setiap operasi

HABIT:
├── Pakai `if err != nil` pattern otomatis
├── gofmt -s -w . (auto format code)
├── Tulis docstring untuk exported functions
└── Test berjalan: go test ./...
```

**Commit**: `feat: add struct, error handling, and file-based persistence`

---

# 🟡 LEVEL 3: GOROUTINES, CHANNELS, & CONCURRENT CLI (Minggu 6-9)

**Tema**: "Go's superpower — Concurrency made simple"

**Benang Merah**: Synchronous operations → goroutines untuk parallel execution → channels untuk communication

**Output**: CLI app yang load katalog async, process requests concurrent, dengan proper synchronization

---

## H. Goroutines dan Channels

### 16. [[16. Goroutine — Lightweight Threads]]

Go

```
package main

import (
    "fmt"
    "time"
)

func printBuku(id int, judul string) {
    for i := 0; i < 3; i++ {
        fmt.Printf("Buku %d: %s (iteration %d)\n", id, judul, i)
        time.Sleep(100 * time.Millisecond)
    }
}

func main() {
    // Tanpa goroutine — sequential (selesai satu, baru lanjut)
    // printBuku(1, "Clean Code")
    // printBuku(2, "Design Patterns")
    
    // Dengan goroutine — parallel (jalan bersamaan)
    go printBuku(1, "Clean Code")
    go printBuku(2, "Design Patterns")
    
    // Main goroutine tunggu goroutines lain selesai
    time.Sleep(1 * time.Second)
    fmt.Println("Done")
}
```

**Penting**: Goroutines sangat lightweight (bisa jutaan), berbeda dari OS threads.

### 17. [[17. Channel — Komunikasi antar Goroutine]]

Go

```
package main

import "fmt"

func kirimBuku(ch chan string) {
    ch <- "Clean Code"      // Send
    ch <- "Design Patterns"
    close(ch)               // Signal selesai
}

func main() {
    ch := make(chan string)
    
    go kirimBuku(ch)
    
    // Receive dari channel
    for buku := range ch {  // Loop sampai channel closed
        fmt.Println("Terima:", buku)
    }
    
    fmt.Println("Selesai")
}
```

**Channel Types**:

Go

```
make(chan Type)           // unbuffered (send/recv harus simultan)
make(chan Type, capacity) // buffered (bisa send capacity kali sebelum block)
```

**Langkah konkret**: Buat concurrent file loader — load katalog.json sambil process requests.

Go

```
func loadKatalogAsync(filepath string, ch chan []Buku) {
    // Simulasi slow I/O
    time.Sleep(2 * time.Second)
    
    // Load dari file
    repo := repository.NewFileRepository(filepath)
    buku, _ := repo.LoadBuku()
    
    ch <- buku  // Send hasil
}

func main() {
    ch := make(chan []Buku)
    
    go loadKatalogAsync("katalog.json", ch)
    
    fmt.Println("Loading katalog...")
    katalog := <-ch  // Wait untuk hasil
    fmt.Println("Katalog loaded:", len(katalog), "buku")
}
```

### 18. [[18. Select dan Timeout]]

Go

```
package main

import (
    "fmt"
    "time"
)

func cariBuku(hasil chan string) {
    time.Sleep(2 * time.Second)
    hasil <- "Found: Clean Code"
}

func main() {
    hasil := make(chan string)
    
    go cariBuku(hasil)
    
    // Select: tunggu channel mana yang ready terlebih dahulu
    select {
    case buku := <-hasil:
        fmt.Println(buku)
    case <-time.After(1 * time.Second):
        fmt.Println("Timeout!")
    }
}
```

**Langkah konkret**: Implementasikan timeout untuk database queries, API calls.

### 19. [[19. WaitGroup untuk Synchronization]]

Go

```
package main

import (
    "fmt"
    "sync"
    "time"
)

func processBuku(id int, wg *sync.WaitGroup) {
    defer wg.Done()  // Signal when done
    
    time.Sleep(1 * time.Second)
    fmt.Printf("Processed buku %d\n", id)
}

func main() {
    var wg sync.WaitGroup
    
    for i := 1; i <= 5; i++ {
        wg.Add(1)  // Add counter
        go processBuku(i, &wg)
    }
    
    wg.Wait()  // Wait until counter = 0
    fmt.Println("All done!")
}
```

---

## I. Context dan Cancellation

### 20. [[20. Context — Manage Goroutine Lifecycle]]

Go

```
package main

import (
    "context"
    "fmt"
    "time"
)

func longRunningTask(ctx context.Context, name string) {
    for {
        select {
        case <-ctx.Done():
            fmt.Printf("%s: context cancelled\n", name)
            return
        default:
            fmt.Printf("%s: working...\n", name)
            time.Sleep(500 * time.Millisecond)
        }
    }
}

func main() {
    // Create context dengan timeout
    ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
    defer cancel()
    
    go longRunningTask(ctx, "Task 1")
    go longRunningTask(ctx, "Task 2")
    
    time.Sleep(3 * time.Second)
    fmt.Println("Main done")
}
```

**Use Cases**:

- `context.WithTimeout()` — set deadline
- `context.WithCancel()` — manual cancellation
- `context.WithValue()` — pass data between goroutines

---

## 🏗 Checkpoint Level 3

text

```
PEMAHAMAN:
├── Bisa jelaskan goroutine vs OS threads
├── Bisa jelaskan unbuffered vs buffered channels
├── Bisa jelaskan select, WaitGroup, Context
└── Bisa debug race conditions (go run -race)

PROYEK: Concurrent Perpustakaan
├── LoadKatalogAsync: load JSON concurrently
├── ProcessRequestsConcurrent: handle multiple requests simultan
├── PerformanceBenchmark: concurrent vs sequential timing
├── Proper cancellation dengan context
├── No data races (verified dengan -race flag)
└── CLI untuk: search, pinjam, kembalikan (all async)

HABIT:
├── Selalu handle context.Done() di goroutines
├── Gunakan WaitGroup untuk coordinate
├── Test dengan: go run -race main.go
└── Prefer channels over shared memory
```

**Commit**: `feat: add goroutines, channels, and concurrent operations`

---

# 🟠 LEVEL 4: DATABASE MYSQL & ORM (Minggu 9-13)

**Tema**: "Dari file-based ke proper database dengan Go SQL/GORM"

**Benang Merah**: File persistence (Level 2) → Database dengan queries → ORM untuk less boilerplate

**Output**: Perpustakaan system dengan MySQL backend, proper schema, transactions

---

## J. Database Setup dan Connection

### 21. [[21. MySQL Setup dan Go SQL Driver]]

Bash

```
# Install MySQL driver
go get github.com/go-sql-driver/mysql

# MySQL connection string:
# username:password@tcp(host:port)/dbname?charset=utf8mb4&parseTime=true
```

Go

```
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/go-sql-driver/mysql"
)

func main() {
    dsn := "root:password@tcp(localhost:3306)/perpustakaan_db?charset=utf8mb4&parseTime=true"
    db, err := sql.Open("mysql", dsn)
    if err != nil {
        fmt.Println("Error opening DB:", err)
        return
    }
    defer db.Close()
    
    // Test koneksi
    err = db.Ping()
    if err != nil {
        fmt.Println("Error connecting:", err)
        return
    }
    
    fmt.Println("Connected to MySQL!")
}
```

### 22. [[22. Basic SQL Queries dengan database/sql]]

Go

```
type Buku struct {
    ID        int
    Judul     string
    Pengarang string
    Harga     float64
    Stok      int
}

// SELECT
func GetAllBuku(db *sql.DB) ([]Buku, error) {
    rows, err := db.Query("SELECT id, judul, pengarang, harga, stok FROM buku")
    if err != nil {
        return nil, err
    }
    defer rows.Close()
    
    var buku []Buku
    for rows.Next() {
        var b Buku
        err := rows.Scan(&b.ID, &b.Judul, &b.Pengarang, &b.Harga, &b.Stok)
        if err != nil {
            return nil, err
        }
        buku = append(buku, b)
    }
    
    return buku, rows.Err()
}

// INSERT
func CreateBuku(db *sql.DB, b Buku) (int, error) {
    result, err := db.Exec(
        "INSERT INTO buku (judul, pengarang, harga, stok) VALUES (?, ?, ?, ?)",
        b.Judul, b.Pengarang, b.Harga, b.Stok,
    )
    if err != nil {
        return 0, err
    }
    
    id, _ := result.LastInsertId()
    return int(id), nil
}

// UPDATE
func UpdateBuku(db *sql.DB, b Buku) error {
    _, err := db.Exec(
        "UPDATE buku SET judul=?, pengarang=?, harga=?, stok=? WHERE id=?",
        b.Judul, b.Pengarang, b.Harga, b.Stok, b.ID,
    )
    return err
}

// DELETE
func DeleteBuku(db *sql.DB, id int) error {
    _, err := db.Exec("DELETE FROM buku WHERE id=?", id)
    return err
}
```

**Penting**: Selalu gunakan `?` placeholders (bukan string interpolation) untuk **SQL injection prevention**.

### 23. [[23. Transactions]]

Go

```
func PinjamBuku(db *sql.DB, bukuID int, anggotaID int) error {
    tx, err := db.Begin()
    if err != nil {
        return err
    }
    defer tx.Rollback()
    
    // Step 1: Cek stok buku
    var stok int
    err = tx.QueryRow("SELECT stok FROM buku WHERE id=?", bukuID).Scan(&stok)
    if err != nil || stok < 1 {
        return fmt.Errorf("buku tidak tersedia")
    }
    
    // Step 2: Kurangi stok
    _, err = tx.Exec("UPDATE buku SET stok=stok-1 WHERE id=?", bukuID)
    if err != nil {
        return err
    }
    
    // Step 3: Insert peminjaman record
    _, err = tx.Exec(
        "INSERT INTO peminjaman (buku_id, anggota_id, tanggal_pinjam, batas_kembali) VALUES (?, ?, NOW(), DATE_ADD(NOW(), INTERVAL 14 DAY))",
        bukuID, anggotaID,
    )
    if err != nil {
        return err
    }
    
    // Commit
    return tx.Commit().Error
}
```

---

## K. GORM (Go's Most Popular ORM)

### 24. [[24. GORM — ORM untuk Less Boilerplate]]

Bash

```
go get -u gorm.io/gorm
go get -u gorm.io/driver/mysql
```

Go

```
package main

import (
    "gorm.io/driver/mysql"
    "gorm.io/gorm"
)

type Buku struct {
    ID        uint      `gorm:"primaryKey"`
    Judul     string
    Pengarang string
    Harga     float64
    Stok      int
    Peminjaman []Peminjaman  // Relationship
}

type Peminjaman struct {
    ID           uint
    BukuID       uint
    AnggotaID    uint
    TanggalPinjam time.Time
    BatasKembali time.Time
    Buku         Buku
}

func main() {
    dsn := "root:password@tcp(localhost:3306)/perpustakaan_db?charset=utf8mb4&parseTime=true"
    db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{})
    if err != nil {
        panic(err)
    }
    
    // Auto migrate (create tables)
    db.AutoMigrate(&Buku{}, &Peminjaman{})
    
    // CREATE
    db.Create(&Buku{Judul: "Clean Code", Pengarang: "Robert Martin", Harga: 150000, Stok: 5})
    
    // READ
    var buku Buku
    db.First(&buku, 1)  // Get by ID
    
    var semuaBuku []Buku
    db.Find(&semuaBuku)  // Get all
    
    // WHERE
    var tekno []Buku
    db.Where("kategori = ?", "Teknologi").Find(&tekno)
    
    // UPDATE
    db.Model(&buku).Update("stok", 4)
    
    // DELETE
    db.Delete(&buku)
}
```

**GORM Features**:

- Auto migrations
- Relationships (HasMany, BelongsTo, etc)
- Query builders
- Hooks (BeforeSave, AfterCreate, etc)
- Transactions built-in

**Langkah konkret**: Refactor Level 2 repository ke use GORM.

---

## 🏗 Checkpoint Level 4

text

```
PEMAHAMAN:
├── Bisa write proper SQL dengan placeholders
├── Bisa implement transactions
├── Paham GORM query builder
└── Paham relationships (HasMany, BelongsTo)

PROYEK: Perpustakaan dengan MySQL + GORM
├── Models: Buku, User, Peminjaman dengan relationships
├── Migration: Auto-create schema
├── Repository: BukuRepository, PeminjamanRepository (using GORM)
├── Business logic: PinjamBuku (with transaction), KembalikanBuku
├── CLI interface untuk CRUD buku, pinjam/kembalikan
├── Error handling untuk constraint violations
└── Connection pooling setup

HABIT:
├── Always use prepared statements (GORM does this)
├── Test transactions: rollback on error
└── Monitor slow queries
```

**Commit**: `feat: add MySQL database and GORM ORM`

---

# 🔴 LEVEL 5: REST API & TESTING (Minggu 13-17)

**Tema**: "Dari CLI ke HTTP REST API yang robust"

**Benang Merah**: Database layer (Level 4) → HTTP handlers → JSON responses → testing

**Output**: REST API untuk perpustakaan dengan proper error handling, validation, testing

---

## L. HTTP Server & Routing

### 25. [[25. Go net/http Package — Build HTTP Server]]

Go

```
package main

import (
    "encoding/json"
    "net/http"
)

type BukuResponse struct {
    ID        int     `json:"id"`
    Judul     string  `json:"judul"`
    Pengarang string  `json:"pengarang"`
    Harga     float64 `json:"harga"`
}

// Handler function
func GetBukuHandler(w http.ResponseWriter, r *http.Request) {
    w.Header().Set("Content-Type", "application/json")
    
    if r.Method != http.MethodGet {
        http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
        return
    }
    
    // Simulasi data dari database
    buku := BukuResponse{1, "Clean Code", "Robert Martin", 150000}
    
    json.NewEncoder(w).Encode(buku)
}

func main() {
    http.HandleFunc("/api/buku", GetBukuHandler)
    
    http.ListenAndServe(":8000", nil)
}
```

**Test dengan**:

Bash

```
curl http://localhost:8000/api/buku
```

### 26. [[26. Routing dengan Gorilla/Mux (atau chi)]]

Go standard library http sederhana tapi terbatas. Gunakan router library untuk production.

Bash

```
go get github.com/gorilla/mux
```

Go

```
package main

import (
    "encoding/json"
    "net/http"
    "github.com/gorilla/mux"
)

func GetAllBukuHandler(w http.ResponseWriter, r *http.Request) {
    w.Header().Set("Content-Type", "application/json")
    json.NewEncoder(w).Encode(map[string]string{"status": "ok"})
}

func GetBukuByIDHandler(w http.ResponseWriter, r *http.Request) {
    vars := mux.Vars(r)
    id := vars["id"]  // Dari URL parameter
    
    w.Header().Set("Content-Type", "application/json")
    json.NewEncoder(w).Encode(map[string]string{"id": id, "status": "ok"})
}

func CreateBukuHandler(w http.ResponseWriter, r *http.Request) {
    if r.Method != http.MethodPost {
        http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
        return
    }
    
    // Parse JSON body
    var buku map[string]interface{}
    json.NewDecoder(r.Body).Decode(&buku)
    
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(http.StatusCreated)
    json.NewEncoder(w).Encode(buku)
}

func main() {
    r := mux.NewRouter()
    
    r.HandleFunc("/api/buku", GetAllBukuHandler).Methods("GET")
    r.HandleFunc("/api/buku/{id}", GetBukuByIDHandler).Methods("GET")
    r.HandleFunc("/api/buku", CreateBukuHandler).Methods("POST")
    
    http.ListenAndServe(":8000", r)
}
```

### 27. [[27. Middleware & Authentication]]

Go

```
type MiddlewareFunc func(http.Handler) http.Handler

// Logging middleware
func LoggingMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        fmt.Printf("%s %s\n", r.Method, r.RequestURI)
        next.ServeHTTP(w, r)
    })
}

// Auth middleware
func AuthMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        token := r.Header.Get("Authorization")
        if token == "" {
            http.Error(w, "Unauthorized", http.StatusUnauthorized)
            return
        }
        
        next.ServeHTTP(w, r)
    })
}

func main() {
    r := mux.NewRouter()
    
    // Apply middleware
    r.Use(LoggingMiddleware)
    
    // Protected route
    r.Handle("/api/buku", AuthMiddleware(
        http.HandlerFunc(CreateBukuHandler),
    )).Methods("POST")
    
    http.ListenAndServe(":8000", r)
}
```

---

## M. Testing

### 28. [[28. Unit Testing dengan testing Package]]

**File: main.go**

Go

```
package main

func HitungDenda(hariTerlambat int) int {
    return hariTerlambat * 1000
}
```

**File: main_test.go**

Go

```
package main

import "testing"

func TestHitungDenda(t *testing.T) {
    tests := []struct {
        hari     int
        expected int
    }{
        {1, 1000},
        {7, 7000},
        {0, 0},
    }
    
    for _, tt := range tests {
        result := HitungDenda(tt.hari)
        if result != tt.expected {
            t.Errorf("HitungDenda(%d) = %d; want %d", tt.hari, result, tt.expected)
        }
    }
}
```

**Run tests**:

Bash

```
go test ./...
go test -v ./...  # verbose
go test -cover ./...  # code coverage
```

### 29. [[29. Table-Driven Tests]]

Go

```
func TestBuku_Pinjam(t *testing.T) {
    tests := []struct {
        name      string
        stok      int
        wantOk    bool
        wantStok  int
    }{
        {"Tersedia", 5, true, 4},
        {"Habis", 0, false, 0},
        {"Satu copy", 1, true, 0},
    }
    
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            b := &Buku{Stok: tt.stok}
            ok := b.Pinjam()
            
            if ok != tt.wantOk {
                t.Errorf("Pinjam() = %v, want %v", ok, tt.wantOk)
            }
            if b.Stok != tt.wantStok {
                t.Errorf("Stok = %d, want %d", b.Stok, tt.wantStok)
            }
        })
    }
}
```

### 30. [[30. HTTP Testing dengan httptest]]

Go

```
func TestGetBukuHandler(t *testing.T) {
    // Create request
    req, err := http.NewRequest("GET", "/api/buku/1", nil)
    if err != nil {
        t.Fatal(err)
    }
    
    // Record response
    rr := httptest.NewRecorder()
    
    // Call handler
    GetBukuByIDHandler(rr, req)
    
    // Check status code
    if status := rr.Code; status != http.StatusOK {
        t.Errorf("handler returned %v, want %v", status, http.StatusOK)
    }
    
    // Check response body
    expected := `{"id":"1","status":"ok"}`
    if rr.Body.String() != expected {
        t.Errorf("handler returned %q, want %q", rr.Body.String(), expected)
    }
}
```

---

## 🏗 Checkpoint Level 5

text

```
PEMAHAMAN:
├── Bisa design RESTful API (GET/POST/PUT/DELETE)
├── Bisa implement middleware (logging, auth)
├── Paham HTTP status codes
└── Bisa write unit + integration tests

PROYEK: REST API Perpustakaan
├── Endpoints:
│   ├── GET /api/buku — daftar semua
│   ├── GET /api/buku/:id — detail
│   ├── POST /api/buku — create
│   ├── PUT /api/buku/:id — update
│   ├── DELETE /api/buku/:id — delete
│   ├── POST /api/pinjam — pinjam buku
│   └── POST /api/kembalikan — kembalikan buku
├── Middleware: logging, error handling
├── Input validation
├── Proper HTTP status codes
├── Error responses dengan consistent format
├── Unit tests for all handlers
├── Integration tests dengan test DB
└── >80% code coverage

HABIT:
├── Table-driven tests sebagai default
├── Test both happy path & error cases
├── httptest.Server untuk integration tests
└── Benchmark: go test -bench=. ./...
```

**Commit**: `feat: build REST API with proper testing`

---

# ⚫ LEVEL 6: FRAMEWORK & PRODUCTION (Minggu 17-22)

**Tema**: "Production-ready application dengan framework dan best practices"

**Benang Merah**: Raw HTTP API (Level 5) → framework untuk less boilerplate → logging, validation, docs

**Output**: Professional API dengan Gin/Echo, structured logging, API documentation, deployment ready

---

## N. Gin Framework

### 31. [[31. Gin — High-Performance HTTP Web Framework]]

Bash

```
go get -u github.com/gin-gonic/gin
```

Go

```
package main

import (
    "github.com/gin-gonic/gin"
    "net/http"
)

func main() {
    r := gin.Default()
    
    // GET
    r.GET("/api/buku", func(c *gin.Context) {
        c.JSON(http.StatusOK, gin.H{"status": "ok"})
    })
    
    // GET dengan parameter
    r.GET("/api/buku/:id", func(c *gin.Context) {
        id := c.Param("id")
        c.JSON(http.StatusOK, gin.H{"id": id})
    })
    
    // POST dengan JSON body
    r.POST("/api/buku", func(c *gin.Context) {
        var buku map[string]interface{}
        if err := c.BindJSON(&buku); err != nil {
            c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
            return
        }
        c.JSON(http.StatusCreated, buku)
    })
    
    r.Run(":8000")
}
```

### 32. [[32. Validation dengan Gin]]

Go

```
type CreateBukuRequest struct {
    Judul     string  `json:"judul" binding:"required,min=3,max=200"`
    Pengarang string  `json:"pengarang" binding:"required,min=2"`
    Harga     float64 `json:"harga" binding:"required,gt=0"`
    Stok      int     `json:"stok" binding:"required,gte=0"`
}

r.POST("/api/buku", func(c *gin.Context) {
    var req CreateBukuRequest
    
    // Validate and bind
    if err := c.ShouldBindJSON(&req); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{"errors": err.Error()})
        return
    }
    
    // Process valid request
    c.JSON(http.StatusCreated, req)
})
```

### 33. [[33. Logging dengan Structured Logging]]

Bash

```
go get -u go.uber.org/zap
```

Go

```
import "go.uber.org/zap"

func init() {
    // Production logger
    logger, _ := zap.NewProduction()
    defer logger.Sync()
    
    logger.Info("Perpustakaan API started",
        zap.String("version", "1.0.0"),
        zap.Int("port", 8000),
    )
}
```

---

## O. Advanced Topics

### 34. [[34. Dependency Injection & Wire]]

Bash

```
go get github.com/google/wire/cmd/wire
```

Go

```
// Wire providers
func NewBukuRepository(db *gorm.DB) *BukuRepository {
    return &BukuRepository{db: db}
}

func NewBukuService(repo *BukuRepository) *BukuService {
    return &BukuService{repo: repo}
}

func NewBukuHandler(service *BukuService) *BukuHandler {
    return &BukuHandler{service: service}
}

// wire.go
//go:build wireinject
// +build wireinject

func BuildBukuHandler(db *gorm.DB) *BukuHandler {
    wire.Build(
        NewBukuRepository,
        NewBukuService,
        NewBukuHandler,
    )
    return nil
}
```

### 35. [[35. Configuration Management]]

Bash

```
go get github.com/joho/godotenv
go get github.com/spf13/viper
```

Go

```
import "github.com/spf13/viper"

func init() {
    viper.SetDefault("PORT", 8000)
    viper.SetDefault("DB_HOST", "localhost")
    
    viper.BindEnv("PORT", "PORT")
    viper.BindEnv("DB_HOST", "DB_HOST")
}

func main() {
    port := viper.GetInt("PORT")
    dbHost := viper.GetString("DB_HOST")
}
```

---

## 🏗 Checkpoint Level 6

text

```
PEMAHAMAN:
├── Framework architecture (MVC-like)
├── Dependency injection patterns
├── Structured logging
├── Configuration management
└── Deployment considerations

PROYEK: Production-Ready API
├── Framework: Gin atau Echo
├── Database migrations
├── Error handling dengan proper status codes
├── Input validation dengan binding tags
├── Structured logging (zap atau logrus)
├── Configuration (via .env)
├── Request/Response middleware
├── CORS handling
├── Rate limiting
├── API documentation (Swagger)
├── Health check endpoint
├── Graceful shutdown
├── Docker support
└── >85% test coverage

DEPLOYMENT:
├── Binary compiled for target OS/architecture
├── Environment variables configured
├── Database migrations run
├── Listening on correct port
└── Proper error messages to clients
```

**Commit**: `feat: production-ready API with Gin, logging, validation`

---

# 🟣 LEVEL 7: MICROSERVICES & ADVANCED (Minggu 22+)

**Tema**: "Scale beyond monolith — Microservices, gRPC, Event-driven architecture"

**Catatan**: Pilih SATU jalur untuk mendalam.

---

## Jalur A: Microservices dengan gRPC

### 36. [[36. Protocol Buffers & gRPC Basics]]

Bash

```
# Install protoc compiler
# Then: go install google.golang.org/protobuf/cmd/protoc-gen-go@latest

go get google.golang.org/grpc
go get google.golang.org/protobuf
```

**File: proto/buku.proto**

protobuf

```
syntax = "proto3";

package perpustakaan;

service BukuService {
  rpc GetBuku(GetBukuRequest) returns (BukuResponse);
  rpc ListBuku(Empty) returns (BukuList);
  rpc CreateBuku(CreateBukuRequest) returns (BukuResponse);
}

message GetBukuRequest {
  int32 id = 1;
}

message BukuResponse {
  int32 id = 1;
  string judul = 2;
  string pengarang = 3;
  double harga = 4;
  int32 stok = 5;
}

message CreateBukuRequest {
  string judul = 1;
  string pengarang = 2;
  double harga = 3;
}

message BukuList {
  repeated BukuResponse buku = 1;
}

message Empty {}
```

### 37. [[37. Service Mesh Introduction (Istio/Linkerd)]]

---

## Jalur B: Event-Driven Architecture

### 38. [[38. Message Broker (RabbitMQ, Kafka)]]

---

## Jalur C: Clean Architecture at Scale

### 39. [[39. Hexagonal Architecture in Go]]

---

# 📊 Ringkasan Progress Tracking

text

```
Level 1: Go basics → Hello World yang berjalan
 + Level 2: Struct + error handling + JSON → File-based perpustakaan
 + Level 3: Goroutines + channels → Concurrent CLI
 + Level 4: Database MySQL + GORM → Data persistence
 + Level 5: REST API + testing → HTTP server dengan tests
 + Level 6: Framework (Gin) + production setup → Ready untuk production
 + Level 7: Microservices / gRPC / Event-driven → Scale beyond monolith
```

## Tabel Progress

|Level|Poin|Durasi|Output Konkret|
|---|---|---|---|
|🟢 1|1-5|Minggu 1-3|Setup Go + Hello World|
|🔵 2|6-15|Minggu 3-6|File-based CLI app|
|🟡 3|16-20|Minggu 6-9|Concurrent operations|
|🟠 4|21-24|Minggu 9-13|MySQL + GORM|
|🔴 5|25-30|Minggu 13-17|REST API + tests|
|⚫ 6|31-35|Minggu 17-22|Production-ready dengan Gin|
|🟣 7|36-39|Minggu 22+|Microservices atau advanced|

---

# 💡 Benang Merah Utama Sepanjang Roadmap

1. **Poin 1** (Go philosophy) → Fondasi: simplicity, fast compilation, built-in concurrency
2. **Poin 4** (Strong typing) → Type safety dari awal → less runtime surprises
3. **Poin 7** (Slice vs Array) → Fundamental untuk data manipulation
4. **Poin 11** (Error pattern) → Go's `if err != nil` everywhere — bukan exceptions
5. **Poin 16** (Goroutines) → Go's superpower — lightweight concurrency
6. **Poin 17** (Channels) → Safe communication between goroutines
7. **Poin 26** (HTTP routing) → Foundation untuk API development
8. **Poin 28** (Testing) → Test-driven development becomes natural
9. **Poin 31** (Framework) → Gin/Echo untuk less boilerplate, more productivity

---

# ⚠️ Aturan yang Tidak Boleh Dilanggar

1. **Always handle errors**: `if err != nil` is not optional
2. **Use prepared statements** (GORM/sqlc does this): SQL injection prevention
3. **Never share memory**: Prefer channels over shared state for goroutines
4. **Test concurrent code**: `go test -race ./...`
5. **Keep goroutines traceable**: Use context for cancellation and deadlines
6. **Structure your packages**: `cmd/`, `internal/`, `pkg/` conventions
7. **Version your API**: `/v1/` in routes from day 1
8. **Log structured data**: Use zap/logrus, not fmt.Println
9. **Validate input**: Always, both at API boundary and business logic
10. **Graceful shutdown**: Handle OS signals (SIGTERM, SIGINT)

---

**Roadmap Go v1.0 — Dari Hello World ke Production-Ready Microservices**

Setiap baris kode ditulis dengan purpose — tidak ada copy-paste tanpa understanding.