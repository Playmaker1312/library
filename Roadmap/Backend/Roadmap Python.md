# 🗺 Roadmap Python: Step-by-Step Membangun Aplikasi Web & Data Science Nyata

## Filosofi Roadmap Ini

"Python adalah bahasa yang indah dan ekspresif — dari sistem scripting hingga machine learning — setiap konsep dipelajari dengan pemahaman filosofi 'There should be one obvious way to do it' dan 'Readability counts'"

---

## Prinsip Desain

- **Satu Project, Tumbuh Bersama**: Sistem manajemen perpustakaan dari CLI → Django web app → Data analysis → Machine learning
- **Pythonic Code**: Mengerti idiom Python (list comprehension, context managers, generators), bukan hanya syntax
- **Benang Merah Eksplisit**: Setiap langkah connected logis dan praktis
- **Python Modern**: Fokus pada Python 3.10+, dengan type hints, modern libraries

---

## 📋 Gambaran Besar — Apa yang Akan Dibangun

text

```
Level 1: "Perpustakaan CLI" — Python basics, data structures, functions
 ↓ (enhance)
Level 2: + File I/O, libraries, virtual environment → Perpustakaan manager dengan file persistence
 ↓ (enhance)
Level 3: + OOP (classes, inheritance) → Structured code dengan OOP principles
 ↓ (enhance)
Level 4: + Django framework, models, views → Web application
 ↓ (enhance)
Level 5: + Database (PostgreSQL), migrations → Data persistence yang robust
 ↓ (enhance)
Level 6: + REST API, testing, authentication → API production-ready
 ↓ (enhance)
Level 7: + Data Science (Pandas, NumPy), analysis → Analytics dashboard
 ↓ (enhance/choose path)
Level 8: + Machine Learning / Advanced web patterns
```

---

# 🟢 LEVEL 1: PYTHON FUNDAMENTALS (Minggu 1-3)

**Tema**: "Python basics — Syntax, data structures, functions"

**Benang Merah**: Syntax → Data types → Functions → Control flow

**Output**: CLI script perpustakaan yang bisa manipulasi data

---

## A. Setup Python & IDE

### 1. [[1. Python Installation dan Environment]]

Python adalah bahasa **interpreted, dynamically typed** dengan syntax yang sangat readable.

**Install Python**:

Bash

```
# Download dari python.org (3.10+ recommended)
python --version
pip --version
```

**Perbedaan dari JavaScript**:

- Python: Interpreter (bukan JIT), strongly typed semantically
- JavaScript: Interpreter/JIT, dynamically typed
- Python: Whitespace matters (indentation)
- JavaScript: Curly braces

**IDE Recommendations**:

- VS Code + Python extension
- PyCharm (Community Edition gratis)
- Jupyter Notebook untuk data science

**Langkah konkret**:

Bash

```
mkdir ~/projects/perpustakaan-python
cd ~/projects/perpustakaan-python
python3 -m venv venv  # Create virtual environment
source venv/bin/activate  # Activate (Linux/Mac)
# atau: venv\Scripts\activate (Windows)
```

### 2. [[2. Program "Hello, Perpustakaan" Pertama]]

**File: main.py**

Python

```
#!/usr/bin/env python3
"""
Sistem Manajemen Perpustakaan
Dokumentasi module: digunakan untuk deskripsi
"""

def main():
    """Entry point aplikasi"""
    print("Selamat datang di Perpustakaan Digital")
    print("Versi: 1.0.0")
    print(f"Python version: {__import__('sys').version}")

if __name__ == "__main__":  # Idiom Python untuk entry point
    main()
```

**Jalankan:**

Bash

```
python main.py
```

**Langkah konkret**: Buat program dengan print statements dan basic output.

---

## B. Data Types & Variables

### 3. [[3. Variables, Types, dan Type Hints]]

Python adalah **dynamically typed** tapi modern Python punya **type hints** untuk clarity.

Python

```
# Variables (Python tidak perlu deklarasi eksplisit)
nama_perpustakaan: str = "Perpustakaan Digital"
kapasitas: int = 1000
rating: float = 4.8
aktif: bool = True
koleksi: None = None

# Collections
kategoris: list = ["Teknologi", "Fiksi", "Sains", "Non-Fiksi"]
buku_ids: set = {1, 2, 3}  # Set (unique values)
metadata: dict = {
    "id": 1,
    "judul": "Clean Code",
    "pengarang": "Robert Martin"
}

# Type hints (annotation, bukan enforcement saat runtime)
def hitung_denda(hari_terlambat: int, tarif_per_hari: int = 1000) -> int:
    """
    Hitung denda keterlambatan.
    
    Args:
        hari_terlambat: Jumlah hari terlambat
        tarif_per_hari: Tarif denda per hari (default: 1000)
    
    Returns:
        Total denda dalam rupiah
    """
    return hari_terlambat * tarif_per_hari

# Calling dengan type hints
denda: int = hitung_denda(7)
print(f"Denda: Rp {denda:,}")  # Formatting dengan separator

# String operations
judul: str = "Clean Code"
pengarang: str = "Robert Martin"
info: str = f"{judul} by {pengarang}"  # f-string (Python 3.6+)
print(info)

# Type checking (optional, gunakan mypy)
# mypy main.py
```

**Python Type Hints Best Practices**:

Python

```
from typing import List, Dict, Optional, Union

buku_list: List[str] = ["Clean Code", "Design Patterns"]
metadata: Dict[str, any] = {"id": 1, "name": "Buku"}
optional_value: Optional[str] = None  # Bisa None atau str
multiple_types: Union[int, str] = "123"

# Gunakan type hints untuk code clarity, bukan enforcement
```

**Langkah konkret**: Buat program dengan berbagai tipe data dan type hints.

### 4. [[4. Collections: List, Tuple, Set, Dictionary]]

Python

```
# LIST (mutable, ordered)
kategoris: list = ["Teknologi", "Fiksi", "Sains"]
kategoris.append("Non-Fiksi")
kategoris.remove("Teknologi")
print(kategoris[0])  # "Fiksi"
print(len(kategoris))  # 3

# TUPLE (immutable, ordered)
koordinat: tuple = (10, 20)
# koordinat[0] = 15  # Error! Tuple immutable
print(koordinat)  # (10, 20)

# SET (unordered, unique)
unique_ids: set = {1, 2, 3, 3, 2}  # {1, 2, 3}
unique_ids.add(4)
print(len(unique_ids))  # 4

# DICTIONARY (key-value pairs)
buku: dict = {
    "id": 1,
    "judul": "Clean Code",
    "pengarang": "Robert Martin",
    "harga": 150000,
    "stok": 5
}
print(buku["judul"])  # "Clean Code"
print(buku.get("rating"))  # None (tidak error)

# Dictionary methods
buku.update({"rating": 4.8})
del buku["rating"]
print(buku.keys())
print(buku.values())

# List comprehension (Pythonic!)
kategoris_upper: list = [k.upper() for k in kategoris]
# ['TEKNOLOGI', 'FIKSI', 'SAINS', 'NON-FIKSI']

# Dictionary comprehension
harga_list: dict = {"buku_1": 150000, "buku_2": 200000}
harga_formatted: dict = {k: f"Rp {v:,}" for k, v in harga_list.items()}
```

**Langkah konkret**: Buat katalog buku dalam list of dictionaries, manipulasi dengan comprehensions.

---

## C. Functions dan Control Flow

### 5. [[5. Functions — DRY (Don't Repeat Yourself)]]

Python

```
from typing import List, Dict, Optional

def cari_buku(buku_list: List[Dict], buku_id: int) -> Optional[Dict]:
    """
    Cari buku berdasarkan ID.
    
    Args:
        buku_list: Daftar buku
        buku_id: ID buku yang dicari
    
    Returns:
        Buku jika ditemukan, None jika tidak
    """
    for buku in buku_list:
        if buku["id"] == buku_id:
            return buku
    return None

def filter_buku_tersedia(buku_list: List[Dict]) -> List[Dict]:
    """Filter buku yang masih tersedia (stok > 0)"""
    return [b for b in buku_list if b["stok"] > 0]

def hitung_total_harga(buku_list: List[Dict]) -> float:
    """Hitung total harga semua buku"""
    return sum(b["harga"] for b in buku_list)

# *args dan **kwargs (flexible arguments)
def format_buku(*buku_list: Dict, separator: str = " | ") -> str:
    """Format multiple books dengan custom separator"""
    return separator.join([b["judul"] for b in buku_list])

# Default values
def pinjam_buku(buku_id: int, durasi_hari: int = 14) -> str:
    """Pinjam buku dengan durasi default 14 hari"""
    return f"Buku {buku_id} dipinjam selama {durasi_hari} hari"

# Multiple returns (tuple unpacking)
def get_buku_info(buku_id: int) -> tuple:
    """Return multiple values"""
    return ("Clean Code", "Robert Martin", 150000)

judul, pengarang, harga = get_buku_info(1)
print(f"{judul} by {pengarang}")
```

**Langkah konkret**: Buat functions untuk CRUD operations pada katalog.

### 6. [[6. Control Flow — if, for, while, match]]

Python

```
# IF statements
stok: int = 5
if stok > 0:
    print("Tersedia")
elif stok == 0:
    print("Habis")
else:
    print("Error")

# Ternary operator (conditional expression)
status = "Tersedia" if stok > 0 else "Habis"

# FOR loop
buku_list: list = [1, 2, 3, 4, 5]
for i, buku_id in enumerate(buku_list):  # enumerate untuk index
    print(f"Index {i}: Buku {buku_id}")

# Dictionary iteration
metadata: dict = {"id": 1, "judul": "Clean Code", "harga": 150000}
for key, value in metadata.items():
    print(f"{key}: {value}")

# WHILE loop
count: int = 0
while count < 5:
    print(count)
    count += 1

# MATCH statement (Python 3.10+, seperti switch)
kategori: str = "Teknologi"
match kategori:
    case "Teknologi":
        print("💻 Tech books")
    case "Fiksi":
        print("📖 Fiction")
    case _:  # Default case
        print("Unknown category")

# Break, continue
for i in range(10):
    if i == 3:
        continue  # Skip ini
    if i == 7:
        break  # Exit loop
    print(i)
```

**Langkah konkret**: Buat menu-driven CLI dengan if-elif-else dan user input.

---

## D. Error Handling & Exceptions

### 7. [[7. Try-Except-Finally (Exception Handling)]]

Python

```
from typing import Optional

def baca_stok(buku_id: int) -> Optional[int]:
    """Baca stok buku dengan error handling"""
    try:
        # Simulasi akses database
        buku_data = {"1": 5, "2": 0, "3": 3}
        stok = buku_data[str(buku_id)]
        return stok
    except KeyError:
        print(f"Error: Buku {buku_id} tidak ditemukan")
        return None
    except Exception as e:
        print(f"Unexpected error: {e}")
        return None
    finally:
        print("Selesai membaca stok")  # Selalu dijalankan

# Custom exceptions
class BukuNotFound(Exception):
    """Custom exception untuk buku tidak ditemukan"""
    pass

class StokHabis(Exception):
    """Custom exception untuk stok habis"""
    pass

def pinjam_buku(buku_id: int, katalog: dict):
    """Pinjam buku dengan custom exceptions"""
    if buku_id not in katalog:
        raise BukuNotFound(f"Buku {buku_id} tidak ditemukan")
    
    if katalog[buku_id]["stok"] == 0:
        raise StokHabis(f"Buku '{katalog[buku_id]['judul']}' habis")
    
    katalog[buku_id]["stok"] -= 1
    return f"Berhasil pinjam buku {katalog[buku_id]['judul']}"

# Usage
katalog = {
    1: {"judul": "Clean Code", "stok": 5},
    2: {"judul": "Design Patterns", "stok": 0},
}

try:
    result = pinjam_buku(1, katalog)
    print(result)
except BukuNotFound as e:
    print(f"BukuNotFound: {e}")
except StokHabis as e:
    print(f"StokHabis: {e}")
except Exception as e:
    print(f"Unexpected: {e}")
```

**Langkah konkret**: Add error handling untuk semua functions.

---

## 🏗 Checkpoint Level 1

text

```
PEMAHAMAN:
├── Python syntax dan indentation
├── Type hints untuk code clarity
├── Collections: list, tuple, set, dict
├── List/dict comprehensions
├── Functions dengan type hints
├── Control flow (if, for, while, match)
├── Exception handling dengan try-except
└── Custom exceptions

PROYEK: Perpustakaan CLI (versi 1)
├── main.py dengan entry point
├── katalog.py dengan data structures
├── functions.py dengan CRUD functions:
│   ├── list_buku()
│   ├── cari_buku(id)
│   ├── pinjam_buku(id)
│   ├── kembalikan_buku(id)
│   └── hitung_denda(hari)
├── CLI menu dengan user input
├── Proper error handling
├── Type hints untuk semua functions
└── Docstrings untuk semua functions

STRUKTUR:
perpustakaan-python/
├── main.py
├── katalog.py
├── functions.py
├── venv/ (virtual environment)
└── requirements.txt (empty for now)

HABIT:
├── Type hints sebagai default
├── List/dict comprehensions untuk transformasi
├── Custom exceptions untuk domain errors
├── Docstrings untuk semua functions
├── f-strings untuk formatting
└── Virtual environment ALWAYS
```

**Commit**: `feat: setup Python project with CLI and basic functions`

---

# 🔵 LEVEL 2: LIBRARIES & FILE I/O (Minggu 3-6)

**Tema**: "Dari isolated script ke reusable code dengan libraries dan file persistence"

**Benang Merah**: Basic Python (Level 1) → Libraries dan modules → File I/O → JSON serialization

**Output**: Perpustakaan manager yang bisa save/load data dari JSON file

---

## E. Modules dan Packages

### 8. [[8. Python Modules dan Imports]]

Python organize code dengan **modules** (`.py` files) dan **packages** (folders dengan `__init__.py`).

**File: perpustakaan/**init**.py**

Python

```
"""
Perpustakaan Package

Main package untuk sistem manajemen perpustakaan
"""

__version__ = "1.0.0"
__author__ = "Your Name"
```

**File: perpustakaan/models.py**

Python

```
"""Models untuk data structures perpustakaan"""
from typing import TypedDict

class BukuData(TypedDict):
    """TypedDict untuk struktur data Buku"""
    id: int
    judul: str
    pengarang: str
    harga: float
    stok: int
    kategori: str

class Buku:
    """Class untuk representasi Buku"""
    def __init__(self, id: int, judul: str, pengarang: str, harga: float, stok: int = 0):
        self.id = id
        self.judul = judul
        self.pengarang = pengarang
        self.harga = harga
        self.stok = stok
    
    def tersedia(self) -> bool:
        """Check apakah buku tersedia"""
        return self.stok > 0
    
    def pinjam(self) -> bool:
        """Pinjam buku, return True jika berhasil"""
        if not self.tersedia():
            return False
        self.stok -= 1
        return True
    
    def kembalikan(self) -> None:
        """Kembalikan buku"""
        self.stok += 1
    
    def __repr__(self) -> str:
        """String representation"""
        return f"Buku(id={self.id}, judul='{self.judul}', stok={self.stok})"
```

**File: perpustakaan/repository.py**

Python

```
"""Repository untuk file-based data persistence"""
from typing import List, Optional
from models import Buku

class Repository:
    """Base repository class"""
    
    def __init__(self):
        self.data: List[Buku] = []
    
    def find_by_id(self, id: int) -> Optional[Buku]:
        """Cari buku berdasarkan ID"""
        for buku in self.data:
            if buku.id == id:
                return buku
        return None
    
    def find_all(self) -> List[Buku]:
        """Return semua buku"""
        return self.data
    
    def add(self, buku: Buku) -> None:
        """Tambah buku"""
        self.data.append(buku)
    
    def remove(self, id: int) -> bool:
        """Hapus buku"""
        buku = self.find_by_id(id)
        if buku:
            self.data.remove(buku)
            return True
        return False

# Singleton pattern (global instance)
repo = Repository()
```

**File: main.py**

Python

```
#!/usr/bin/env python3
"""Main entry point aplikasi perpustakaan"""

from perpustakaan.models import Buku
from perpustakaan.repository import repo

def main():
    """Main application"""
    # Add sample data
    buku1 = Buku(1, "Clean Code", "Robert Martin", 150000, 5)
    buku2 = Buku(2, "Design Patterns", "Gang of Four", 200000, 3)
    
    repo.add(buku1)
    repo.add(buku2)
    
    # List buku
    print("Katalog Perpustakaan:")
    for buku in repo.find_all():
        print(f"  {buku}")
    
    # Pinjam buku
    buku = repo.find_by_id(1)
    if buku:
        buku.pinjam()
        print(f"Berhasil pinjam {buku.judul}, stok sekarang: {buku.stok}")

if __name__ == "__main__":
    main()
```

**Import styles:**

Python

```
# Import specific item
from perpustakaan.models import Buku

# Import module
import perpustakaan.models

# Import dengan alias
from perpustakaan.repository import Repository as Repo
```

**Langkah konkret**: Refactor Level 1 code menjadi modules dengan proper structure.

### 9. [[9. Virtual Environment dan Requirements.txt]]

**Virtual environment** isolate project dependencies.

Bash

```
# Create virtual environment
python3 -m venv venv

# Activate
source venv/bin/activate  # Linux/Mac
# atau venv\Scripts\activate (Windows)

# Install package (example)
pip install requests

# Save dependencies
pip freeze > requirements.txt

# Install dari requirements
pip install -r requirements.txt
```

**File: requirements.txt**

text

```
requests==2.31.0
python-dotenv==1.0.0
```

**File: .gitignore**

text

```
venv/
__pycache__/
*.pyc
.env
.DS_Store
dist/
build/
*.egg-info/
```

---

## F. File I/O dan JSON

### 10. [[10. File Operations dengan Context Manager]]

Python

```
# Context manager (with statement) untuk file handling
with open("data.txt", "r") as f:  # Otomatis close file
    content = f.read()
    print(content)

# Write ke file
with open("output.txt", "w") as f:
    f.write("Hello, Perpustakaan!")
    f.writelines(["Line 1\n", "Line 2\n"])

# Append ke file
with open("log.txt", "a") as f:
    f.write("New log entry\n")

# Read line by line
with open("large_file.txt", "r") as f:
    for line in f:
        print(line.strip())

# Path handling (pathlib - modern way)
from pathlib import Path

file_path = Path("data") / "katalog.json"  # Cross-platform path
print(file_path.exists())
print(file_path.absolute())
print(file_path.parent)
```

**Custom Context Manager:**

Python

```
class FileRepository:
    """Context manager untuk file operations"""
    
    def __init__(self, filepath: str):
        self.filepath = filepath
        self.file = None
    
    def __enter__(self):
        """Called saat masuk with block"""
        self.file = open(self.filepath, "r")
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        """Called saat keluar with block"""
        if self.file:
            self.file.close()
        return False

# Usage
with FileRepository("data.txt") as f:
    content = f.read()
    print(content)
```

**Langkah konkret**: Buat functions untuk read/write file operations.

### 11. [[11. JSON Serialization dan Deserialization]]

Python

```
import json
from pathlib import Path
from typing import List, Dict, Any

class JSONRepository:
    """Repository yang persist data ke JSON file"""
    
    def __init__(self, filepath: str):
        self.filepath = Path(filepath)
        self.ensure_file()
    
    def ensure_file(self):
        """Create file jika belum ada"""
        if not self.filepath.exists():
            self.filepath.parent.mkdir(parents=True, exist_ok=True)
            with open(self.filepath, "w") as f:
                json.dump([], f)
    
    def load(self) -> List[Dict[str, Any]]:
        """Load data dari JSON file"""
        try:
            with open(self.filepath, "r") as f:
                return json.load(f)
        except json.JSONDecodeError:
            return []
    
    def save(self, data: List[Dict[str, Any]]) -> None:
        """Save data ke JSON file"""
        with open(self.filepath, "w") as f:
            json.dump(data, f, indent=2, ensure_ascii=False)
    
    def add(self, item: Dict[str, Any]) -> None:
        """Add item"""
        data = self.load()
        data.append(item)
        self.save(data)
    
    def update(self, item_id: int, updates: Dict[str, Any]) -> bool:
        """Update item by ID"""
        data = self.load()
        for item in data:
            if item.get("id") == item_id:
                item.update(updates)
                self.save(data)
                return True
        return False
    
    def delete(self, item_id: int) -> bool:
        """Delete item by ID"""
        data = self.load()
        new_data = [item for item in data if item.get("id") != item_id]
        if len(new_data) < len(data):
            self.save(new_data)
            return True
        return False

# Usage
repo = JSONRepository("data/katalog.json")

# Create
repo.add({
    "id": 1,
    "judul": "Clean Code",
    "pengarang": "Robert Martin",
    "harga": 150000,
    "stok": 5
})

# Read
katalog = repo.load()
print(katalog)

# Update
repo.update(1, {"stok": 3})

# Delete
repo.delete(1)
```

**JSON serialization untuk custom objects:**

Python

```
import json
from datetime import datetime

class BukuEncoder(json.JSONEncoder):
    """Custom JSON encoder untuk Buku objects"""
    
    def default(self, obj):
        if isinstance(obj, Buku):
            return {
                "id": obj.id,
                "judul": obj.judul,
                "pengarang": obj.pengarang,
                "harga": obj.harga,
                "stok": obj.stok,
            }
        if isinstance(obj, datetime):
            return obj.isoformat()
        return super().default(obj)

# Serialize custom object
buku = Buku(1, "Clean Code", "Robert Martin", 150000, 5)
json_str = json.dumps(buku, cls=BukuEncoder)
```

---

## G. Environment Variables dan Configuration

### 12. [[12. Python-dotenv untuk Configuration]]

Bash

```
pip install python-dotenv
```

**File: .env**

text

```
PERPUSTAKAAN_NAME=Perpustakaan Digital
DATA_PATH=./data
LOG_LEVEL=INFO
DEBUG=True
```

**File: config.py**

Python

```
"""Configuration management"""
import os
from pathlib import Path
from dotenv import load_dotenv

# Load environment variables dari .env
load_dotenv()

class Config:
    """Base configuration"""
    PERPUSTAKAAN_NAME = os.getenv("PERPUSTAKAAN_NAME", "Perpustakaan")
    DATA_PATH = Path(os.getenv("DATA_PATH", "./data"))
    LOG_LEVEL = os.getenv("LOG_LEVEL", "INFO")
    DEBUG = os.getenv("DEBUG", "False").lower() in ("true", "1", "yes")
    
    # Derived paths
    KATALOG_PATH = DATA_PATH / "katalog.json"
    USER_PATH = DATA_PATH / "user.json"

# Usage
print(Config.PERPUSTAKAAN_NAME)
print(Config.KATALOG_PATH)
```

---

## 🏗 Checkpoint Level 2

text

```
PEMAHAMAN:
├── Python modules dan packages structure
├── Imports (relative dan absolute)
├── Virtual environments dan pip
├── Context managers (with statement)
├── File I/O operations
├── JSON serialization/deserialization
├── JSONRepository pattern
└── Configuration management dengan .env

PROYEK: File-Based Perpustakaan Repository
├── perpustakaan/
│   ├── __init__.py
│   ├── models.py — Buku class
│   ├── repository.py — JSONRepository
│   └── config.py — Configuration
├── main.py — Entry point
├── requirements.txt
├── .env
├── .gitignore
└── data/
    └── katalog.json

CRUD OPERATIONS:
├── Load katalog dari JSON
├── List semua buku
├── Search buku by ID/judul
├── Add buku (auto-increment ID)
├── Update buku
├── Delete buku
├── Persist ke JSON file
└── All operations tested

HABIT:
├── ALWAYS use virtual environment
├── ALWAYS use context managers untuk file ops
├── ALWAYS type hints di functions
├── ALWAYS docstrings untuk modules/classes/functions
├── .env untuk configuration, bukan hardcoded
└── pathlib untuk path operations (bukan string)
```

**Commit**: `feat: add modules, file I/O, and JSON persistence`

---

# 🟡 LEVEL 3: OBJECT-ORIENTED PROGRAMMING (Minggu 6-9)

**Tema**: "Dari procedural ke proper OOP — Classes, inheritance, polymorphism"

**Benang Merah**: Functions (Level 2) → Classes → Inheritance → Polymorphism → Design patterns

**Output**: Perpustakaan system dengan proper OOP architecture

---

## H. Classes dan Objects

### 13. [[13. Class Definition dan Initialization]]

Python

```
from typing import Optional
from datetime import datetime, timedelta

class Buku:
    """Class untuk representasi Buku"""
    
    # Class variable (shared across instances)
    total_buku = 0
    kategori_valid = {"Teknologi", "Fiksi", "Non-Fiksi", "Sains", "Umum"}
    
    def __init__(self, id: int, judul: str, pengarang: str, harga: float, stok: int = 0):
        """Constructor (initializer)"""
        # Instance variables
        self.id = id
        self.judul = judul
        self.pengarang = pengarang
        self.harga = harga
        self.stok = stok
        self.kategori = "Umum"
        self.dibuat_pada = datetime.now()
        
        Buku.total_buku += 1
    
    def tersedia(self) -> bool:
        """Check ketersediaan buku"""
        return self.stok > 0
    
    def pinjam(self) -> bool:
        """Pinjam buku"""
        if not self.tersedia():
            return False
        self.stok -= 1
        return True
    
    def kembalikan(self) -> None:
        """Kembalikan buku"""
        self.stok += 1
    
    def format_harga(self) -> str:
        """Format harga dengan separator"""
        return f"Rp {self.harga:,.0f}"
    
    # Special methods (dunder methods)
    def __str__(self) -> str:
        """String representation untuk user"""
        return f"{self.judul} by {self.pengarang} ({self.format_harga()})"
    
    def __repr__(self) -> str:
        """String representation untuk developer"""
        return f"Buku(id={self.id}, judul='{self.judul}', stok={self.stok})"
    
    def __eq__(self, other: "Buku") -> bool:
        """Equality comparison"""
        if not isinstance(other, Buku):
            return False
        return self.id == other.id
    
    def __lt__(self, other: "Buku") -> bool:
        """Less than comparison (untuk sorting)"""
        return self.harga < other.harga
    
    def __len__(self) -> int:
        """Len function"""
        return self.stok
    
    @staticmethod
    def validate_judul(judul: str) -> bool:
        """Static method — tidak akses instance/class"""
        return 3 <= len(judul) <= 200
    
    @classmethod
    def from_dict(cls, data: dict) -> "Buku":
        """Class method — factory method"""
        return cls(
            id=data["id"],
            judul=data["judul"],
            pengarang=data["pengarang"],
            harga=data["harga"],
            stok=data.get("stok", 0)
        )
    
    @property
    def status(self) -> str:
        """Property — akses seperti attribute, computed value"""
        if self.stok > 0:
            return "Tersedia"
        return "Habis"
    
    @status.setter
    def status(self, value: str) -> None:
        """Property setter"""
        if value == "Habis":
            self.stok = 0

# Usage
buku = Buku(1, "Clean Code", "Robert Martin", 150000, 5)
print(str(buku))  # Clean Code by Robert Martin (Rp 150,000)
print(repr(buku))  # Buku(id=1, judul='Clean Code', stok=5)
print(buku.tersedia())  # True
print(buku.status)  # "Tersedia"

# Factory method
buku2 = Buku.from_dict({
    "id": 2,
    "judul": "Design Patterns",
    "pengarang": "Gang of Four",
    "harga": 200000,
    "stok": 3
})

# Comparison
print(buku < buku2)  # True (150000 < 200000)

print(Buku.total_buku)  # 2 (class variable)
```

**Langkah konkret**: Create Buku, User, Peminjaman classes dengan proper dunder methods.

### 14. [[14. Inheritance dan Method Overriding]]

Python

```
from abc import ABC, abstractmethod
from datetime import datetime

# Abstract base class
class Entity(ABC):
    """Abstract base class untuk semua entities"""
    
    def __init__(self, id: int):
        self.id = id
        self.created_at = datetime.now()
    
    @abstractmethod
    def to_dict(self) -> dict:
        """Subclasses HARUS implement method ini"""
        pass

class Buku(Entity):
    """Buku entity"""
    
    def __init__(self, id: int, judul: str, pengarang: str, harga: float):
        super().__init__(id)  # Call parent constructor
        self.judul = judul
        self.pengarang = pengarang
        self.harga = harga
    
    def to_dict(self) -> dict:
        """Implement abstract method"""
        return {
            "id": self.id,
            "judul": self.judul,
            "pengarang": self.pengarang,
            "harga": self.harga,
            "created_at": self.created_at.isoformat(),
        }

class Pinjaman(Entity):
    """Pinjaman entity"""
    
    def __init__(self, id: int, buku_id: int, anggota_id: int, durasi_hari: int = 14):
        super().__init__(id)
        self.buku_id = buku_id
        self.anggota_id = anggota_id
        self.durasi_hari = durasi_hari
        self.batas_kembali = datetime.now() + timedelta(days=durasi_hari)
    
    def is_overdue(self) -> bool:
        """Check apakah terlambat"""
        return datetime.now() > self.batas_kembali
    
    def to_dict(self) -> dict:
        return {
            "id": self.id,
            "buku_id": self.buku_id,
            "anggota_id": self.anggota_id,
            "batas_kembali": self.batas_kembali.isoformat(),
            "is_overdue": self.is_overdue(),
        }

# Usage
buku = Buku(1, "Clean Code", "Robert Martin", 150000)
pinjaman = Pinjaman(1, 1, 100)

print(buku.to_dict())
print(pinjaman.to_dict())
print(pinjaman.is_overdue())  # False
```

**Langkah konkret**: Create inheritance hierarchy dengan Entity base class.

### 15. [[15. Composition Over Inheritance]]

Python

```
# Composition: object memiliki objects lain
class Peminjaman:
    """Peminjaman dengan composition"""
    
    def __init__(self, id: int, buku: Buku, anggota: "User", durasi_hari: int = 14):
        self.id = id
        self.buku = buku  # Composition: has-a relationship
        self.anggota = anggota
        self.durasi_hari = durasi_hari
        self.tanggal_pinjam = datetime.now()
        self.batas_kembali = datetime.now() + timedelta(days=durasi_hari)
    
    def hitung_denda(self) -> float:
        """Hitung denda jika terlambat"""
        if datetime.now() <= self.batas_kembali:
            return 0
        
        hari_terlambat = (datetime.now() - self.batas_kembali).days
        tarif_per_hari = 1000
        return hari_terlambat * tarif_per_hari
    
    def __str__(self) -> str:
        return f"{self.buku.judul} dipinjam oleh {self.anggota.nama}"

# Usage
buku = Buku(1, "Clean Code", "Robert Martin", 150000, 5)
user = User(1, "John Doe", "john@email.com")
pinjaman = Peminjaman(1, buku, user)

print(pinjaman)  # Clean Code dipinjam oleh John Doe
print(f"Denda: Rp {pinjaman.hitung_denda():,.0f}")
```

---

## I. Design Patterns

### 16. [[16. Common Design Patterns in Python]]

**Singleton Pattern:**

Python

```
class PerpustakaanManager:
    """Singleton pattern — hanya satu instance"""
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._instance._initialized = False
        return cls._instance
    
    def __init__(self):
        if self._initialized:
            return
        self.katalog = []
        self._initialized = True

# Hanya satu instance yang dibuat
manager1 = PerpustakaanManager()
manager2 = PerpustakaanManager()
print(manager1 is manager2)  # True

# Atau dengan decorator (lebih Pythonic)
def singleton(cls):
    instances = {}
    def get_instance(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    return get_instance

@singleton
class Config:
    pass
```

**Factory Pattern:**

Python

```
class RepositoryFactory:
    """Factory untuk create berbagai repository types"""
    
    @staticmethod
    def create(repo_type: str):
        if repo_type == "json":
            return JSONRepository()
        elif repo_type == "memory":
            return MemoryRepository()
        elif repo_type == "database":
            return DatabaseRepository()
        else:
            raise ValueError(f"Unknown repository type: {repo_type}")

# Usage
repo = RepositoryFactory.create("json")
```

**Strategy Pattern:**

Python

```
from abc import ABC, abstractmethod

class SortStrategy(ABC):
    """Abstract strategy untuk sorting"""
    
    @abstractmethod
    def sort(self, data: list) -> list:
        pass

class SortByHarga(SortStrategy):
    def sort(self, buku_list: list) -> list:
        return sorted(buku_list, key=lambda b: b.harga)

class SortByJudul(SortStrategy):
    def sort(self, buku_list: list) -> list:
        return sorted(buku_list, key=lambda b: b.judul)

class Katalog:
    def __init__(self, strategy: SortStrategy):
        self.strategy = strategy
        self.buku = []
    
    def get_sorted(self) -> list:
        return self.strategy.sort(self.buku)
    
    def set_strategy(self, strategy: SortStrategy) -> None:
        self.strategy = strategy

# Usage
katalog = Katalog(SortByHarga())
print(katalog.get_sorted())

katalog.set_strategy(SortByJudul())
print(katalog.get_sorted())
```

---

## 🏗 Checkpoint Level 3

text

```
PEMAHAMAN:
├── Class definition dan constructor
├── Instance vs class variables
├── Dunder methods (__str__, __repr__, __eq__, etc)
├── Properties dan setters
├── Static methods dan class methods
├── Inheritance dan method overriding
├── Abstract base classes (ABC)
├── Composition over inheritance
└── Common design patterns (Singleton, Factory, Strategy)

PROYEK: OOP-Based Perpustakaan System
├── Entity abstract base class
├── Buku(Entity)
├── User(Entity)
├── Peminjaman(Entity)
├── Denda(Entity) for tracking late returns
├── Proper dunder methods untuk semua classes
├── Properties untuk computed values
├── Factory methods (from_dict)
├── Strategy pattern untuk filtering/sorting
├── Composition di Peminjaman (has Buku dan User)
└── All classes properly documented

STRUKTUR:
perpustakaan/
├── models/
│   ├── __init__.py
│   ├── base.py — Entity abstract class
│   ├── buku.py — Buku class
│   ├── user.py — User class
│   ├── peminjaman.py — Peminjaman class
│   └── denda.py — Denda class
├── repository.py — CRUD operations
└── main.py

HABIT:
├── ALWAYS inherit dari base class/interface
├── ALWAYS use type hints
├── ALWAYS implement dunder methods untuk equality, comparison
├── Composition > inheritance (prefer has-a over is-a)
├── Design patterns untuk testability
└── Docstrings dengan examples
```

**Commit**: `feat: implement OOP with inheritance, composition, and design patterns`

---

# 🟠 LEVEL 4: DJANGO & WEB FRAMEWORK (Minggu 9-13)

**Tema**: "Dari CLI ke web — Django framework untuk building web applications"

**Benang Merah**: OOP classes (Level 3) → Django models → Views and templates → Database integration

**Output**: Web application untuk perpustakaan dengan HTML templates dan database

---

## J. Django Basics

### 17. [[17. Django Project Setup]]

Bash

```
pip install django
django-admin startproject perpustakaan_project
cd perpustakaan_project
python manage.py startapp perpustakaan
```

**File: perpustakaan/models.py**

Python

```
from django.db import models
from django.utils import timezone

class Buku(models.Model):
    """Model untuk Buku"""
    
    KATEGORI_CHOICES = [
        ('teknologi', 'Teknologi'),
        ('fiksi', 'Fiksi'),
        ('non_fiksi', 'Non-Fiksi'),
        ('sains', 'Sains'),
    ]
    
    id = models.AutoField(primary_key=True)
    judul = models.CharField(max_length=200)
    pengarang = models.CharField(max_length=100)
    isbn = models.CharField(max_length=13, unique=True, blank=True, null=True)
    harga = models.DecimalField(max_digits=10, decimal_places=2)
    stok = models.IntegerField(default=0)
    kategori = models.CharField(max_length=50, choices=KATEGORI_CHOICES)
    deskripsi = models.TextField(blank=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    class Meta:
        ordering = ['-created_at']
        indexes = [
            models.Index(fields=['judul']),
            models.Index(fields=['pengarang']),
        ]
    
    def __str__(self) -> str:
        return f"{self.judul} ({self.stok})"
    
    def tersedia(self) -> bool:
        return self.stok > 0

class User(models.Model):
    """Model untuk User (Anggota)"""
    
    nama = models.CharField(max_length=100)
    email = models.EmailField(unique=True)
    no_anggota = models.CharField(max_length=20, unique=True)
    created_at = models.DateTimeField(auto_now_add=True)
    
    def __str__(self) -> str:
        return self.nama

class Peminjaman(models.Model):
    """Model untuk Peminjaman"""
    
    buku = models.ForeignKey(Buku, on_delete=models.PROTECT)
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    tanggal_pinjam = models.DateTimeField(auto_now_add=True)
    batas_kembali = models.DateTimeField()
    tanggal_kembali = models.DateTimeField(null=True, blank=True)
    
    class Meta:
        ordering = ['-tanggal_pinjam']
    
    def __str__(self) -> str:
        return f"{self.buku.judul} - {self.user.nama}"
    
    def is_overdue(self) -> bool:
        if self.tanggal_kembali:
            return False
        return timezone.now() > self.batas_kembali
```

**Migrate database:**

Bash

```
python manage.py makemigrations
python manage.py migrate
```

### 18. [[18. Django Views dan URL Routing]]

**File: perpustakaan/views.py**

Python

```
from django.shortcuts import render, get_object_or_404, redirect
from django.views.generic import ListView, DetailView, CreateView
from django.contrib.auth.mixins import LoginRequiredMixin
from django.urls import reverse_lazy
from .models import Buku, Peminjaman

# Function-based view
def buku_list(request):
    """List semua buku"""
    buku_list = Buku.objects.all()
    
    # Filter
    kategori = request.GET.get('kategori')
    if kategori:
        buku_list = buku_list.filter(kategori=kategori)
    
    # Search
    search = request.GET.get('search')
    if search:
        buku_list = buku_list.filter(
            Q(judul__icontains=search) | 
            Q(pengarang__icontains=search)
        )
    
    context = {
        'buku_list': buku_list,
        'search': search,
        'kategori': kategori,
    }
    return render(request, 'perpustakaan/buku_list.html', context)

def buku_detail(request, pk):
    """Detail satu buku"""
    buku = get_object_or_404(Buku, pk=pk)
    return render(request, 'perpustakaan/buku_detail.html', {'buku': buku})

# Class-based view (lebih Pythonic)
class BukuListView(ListView):
    """Class-based view untuk list buku"""
    model = Buku
    template_name = 'perpustakaan/buku_list.html'
    context_object_name = 'buku_list'
    paginate_by = 20

class BukuDetailView(DetailView):
    """Class-based view untuk detail buku"""
    model = Buku
    template_name = 'perpustakaan/buku_detail.html'
    context_object_name = 'buku'

class PeminjamanCreateView(LoginRequiredMixin, CreateView):
    """Class-based view untuk create peminjaman"""
    model = Peminjaman
    fields = ['buku']
    template_name = 'perpustakaan/peminjaman_form.html'
    success_url = reverse_lazy('peminjaman_list')
    
    def form_valid(self, form):
        form.instance.user = self.request.user
        form.instance.batas_kembali = timezone.now() + timedelta(days=14)
        return super().form_valid(form)
```

**File: perpustakaan/urls.py**

Python

```
from django.urls import path
from . import views

urlpatterns = [
    path('buku/', views.BukuListView.as_view(), name='buku_list'),
    path('buku/<int:pk>/', views.BukuDetailView.as_view(), name='buku_detail'),
    path('peminjaman/create/', views.PeminjamanCreateView.as_view(), name='peminjaman_create'),
]
```

**File: perpustakaan_project/urls.py**

Python

```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('perpustakaan/', include('perpustakaan.urls')),
]
```

### 19. [[19. Django Templates dan Forms]]

**File: perpustakaan/templates/perpustakaan/buku_list.html**

HTML

```
{% extends "base.html" %}
{% load static %}

{% block title %}Daftar Buku{% endblock %}

{% block content %}
<div class="container">
    <h1>Katalog Perpustakaan</h1>
    
    <!-- Search Form -->
    <form method="get" class="search-form">
        <input type="text" name="search" placeholder="Cari buku..." 
               value="{{ search }}">
        <select name="kategori">
            <option value="">Semua kategori</option>
            <option value="teknologi" {% if kategori == 'teknologi' %}selected{% endif %}>
                Teknologi
            </option>
        </select>
        <button type="submit">Search</button>
    </form>
    
    <!-- Buku List -->
    <div class="buku-grid">
        {% for buku in buku_list %}
        <div class="buku-card">
            <h2>{{ buku.judul }}</h2>
            <p>{{ buku.pengarang }}</p>
            <p class="price">{{ buku.harga|floatformat:0 }} Rupiah</p>
            <p class="stok">
                {% if buku.tersedia %}
                    <span class="badge badge-success">Tersedia</span>
                {% else %}
                    <span class="badge badge-danger">Habis</span>
                {% endif %}
            </p>
            <a href="{% url 'buku_detail' buku.id %}" class="btn btn-primary">
                Detail
            </a>
        </div>
        {% endfor %}
    </div>
    
    <!-- Pagination -->
    {% if is_paginated %}
    <nav>
        {% if page_obj.has_previous %}
        <a href="?page=1">First</a>
        <a href="?page={{ page_obj.previous_page_number }}">Previous</a>
        {% endif %}
        
        Page {{ page_obj.number }} of {{ page_obj.paginator.num_pages }}
        
        {% if page_obj.has_next %}
        <a href="?page={{ page_obj.next_page_number }}">Next</a>
        <a href="?page={{ page_obj.paginator.num_pages }}">Last</a>
        {% endif %}
    </nav>
    {% endif %}
</div>
{% endblock %}
```

**File: perpustakaan/forms.py**

Python

```
from django import forms
from .models import Peminjaman, Buku

class PeminjamanForm(forms.ModelForm):
    """Form untuk membuat peminjaman"""
    
    buku = forms.ModelChoiceField(
        queryset=Buku.objects.filter(stok__gt=0),
        label="Pilih Buku"
    )
    
    class Meta:
        model = Peminjaman
        fields = ['buku']
```

**Run server:**

Bash

```
python manage.py runserver
# Visit http://localhost:8000/perpustakaan/buku/
```

---

## 🏗 Checkpoint Level 4

text

```
PEMAHANAN:
├── Django project structure
├── Django models dan fields
├── Model relationships (ForeignKey, ManyToMany)
├── Django ORM queries
├── Function-based views
├── Class-based views (ListView, DetailView, CreateView)
├── Django URL routing
├── Template language dan template inheritance
├── Django forms
└── Admin interface

PROYEK: Django Web Application
├── Models: Buku, User, Peminjaman, Denda
├── Views: List, Detail, Create, Update, Delete (CRUD)
├── Templates: base, list, detail, form
├── Forms untuk input validation
├── Admin interface configured
├── Filtering dan searching functionality
├── Pagination untuk list views
├── Proper relationships (ForeignKey, ManyToMany)
└── All models registered in admin

STRUKTUR:
perpustakaan_project/
├── manage.py
├── perpustakaan_project/
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── perpustakaan/
│   ├── models.py
│   ├── views.py
│   ├── forms.py
│   ├── urls.py
│   ├── admin.py
│   ├── migrations/
│   └── templates/
│       └── perpustakaan/
│           ├── base.html
│           ├── buku_list.html
│           ├── buku_detail.html
│           └── peminjaman_form.html
└── db.sqlite3

HABIT:
├── Use class-based views (cleaner code)
├── ORM queries, bukan raw SQL
├── Form validation di model dan form layer
├── Template inheritance untuk DRY
├── Admin interface utk quick access ke data
└── Pagination untuk large datasets
```

**Commit**: `feat: create Django web application with models, views, templates`

---

# 🔴 LEVEL 5: DATABASE & MIGRATIONS (Minggu 13-17)

**Tema**: "Robust database with PostgreSQL, migrations, relationships"

**Benang Merah**: SQLite (easy) → PostgreSQL (production) → Migrations → Complex relationships

**Output**: Production-ready database with proper schema and migrations

---

## K. PostgreSQL Setup

### 20. [[20. PostgreSQL dan Django Configuration]]

Bash

```
# Install PostgreSQL adapter
pip install psycopg2-binary
```

**File: perpustakaan_project/settings.py**

Python

```
import os
from pathlib import Path
from dotenv import load_dotenv

load_dotenv()

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.getenv('DB_NAME', 'perpustakaan'),
        'USER': os.getenv('DB_USER', 'postgres'),
        'PASSWORD': os.getenv('DB_PASSWORD', ''),
        'HOST': os.getenv('DB_HOST', 'localhost'),
        'PORT': os.getenv('DB_PORT', '5432'),
    }
}

# Installed apps
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'perpustakaan',
]

# Middleware
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
]

# Internationalization
LANGUAGE_CODE = 'id-id'
TIME_ZONE = 'Asia/Jakarta'
USE_I18N = True
USE_TZ = True
```

**File: .env**

text

```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=perpustakaan
DB_USER=postgres
DB_PASSWORD=password
DB_HOST=localhost
DB_PORT=5432
```

### 21. [[21. Django Migrations]]

Python

```
# Create migration
python manage.py makemigrations perpustakaan

# See migration files
# perpustakaan/migrations/0001_initial.py

# Apply migration
python manage.py migrate

# Show migration status
python manage.py showmigrations

# Revert migration
python manage.py migrate perpustakaan 0001_initial  # Go back to initial
```

**File: perpustakaan/migrations/0001_initial.py** (auto-generated)

Python

```
from django.db import migrations, models
import django.db.models.deletion

class Migration(migrations.Migration):
    initial = True
    
    dependencies = []
    
    operations = [
        migrations.CreateModel(
            name='Buku',
            fields=[
                ('id', models.BigAutoField(primary_key=True, serialize=False)),
                ('judul', models.CharField(max_length=200)),
                # ... more fields
            ],
        ),
    ]
```

### 22. [[22. Complex Relationships]]

Python

```
from django.db import models

class Author(models.Model):
    """Author model"""
    nama = models.CharField(max_length=100)
    bio = models.TextField(blank=True)
    
    def __str__(self):
        return self.nama

class Buku(models.Model):
    """Buku with authors (many-to-many)"""
    judul = models.CharField(max_length=200)
    authors = models.ManyToManyField(Author, related_name='books')
    publisher = models.ForeignKey('Publisher', on_delete=models.SET_NULL, null=True)
    harga = models.DecimalField(max_digits=10, decimal_places=2)
    stok = models.IntegerField(default=0)
    
    def __str__(self):
        return self.judul

class Publisher(models.Model):
    """Publisher model"""
    nama = models.CharField(max_length=100)
    negara = models.CharField(max_length=100)
    
    def __str__(self):
        return self.nama

class Review(models.Model):
    """Review dengan relationships"""
    buku = models.ForeignKey(Buku, on_delete=models.CASCADE, related_name='reviews')
    user = models.ForeignKey('auth.User', on_delete=models.CASCADE)
    rating = models.IntegerField(choices=[(i, str(i)) for i in range(1, 6)])
    komentar = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    
    class Meta:
        unique_together = ['buku', 'user']  # One review per user per book

# Django ORM queries dengan relationships
# Get all books by author
books = Author.objects.get(nama="Robert Martin").books.all()

# Get all reviews for a book
reviews = Buku.objects.get(pk=1).reviews.all()

# Count reviews
review_count = Buku.objects.get(pk=1).reviews.count()

# Filter dengan relationships
books_with_reviews = Buku.objects.filter(reviews__isnull=False).distinct()

# Prefetch related (optimize queries)
from django.db.models import Prefetch
books = Buku.objects.prefetch_related('authors', 'reviews')
```

---

## L. Django ORM Advanced

### 23. [[23. Query Optimization dan Aggregation]]

Python

```
from django.db.models import Count, Avg, Sum, Q, F

# Count, Average
book_stats = Buku.objects.aggregate(
    total=Count('id'),
    avg_harga=Avg('harga'),
    total_stok=Sum('stok'),
)
# {'total': 50, 'avg_harga': 125000.0, 'total_stok': 500}

# Count per author
authors = Author.objects.annotate(
    book_count=Count('books')
).filter(book_count__gt=0)

# Complex queries with Q objects
buku_murah_atau_populer = Buku.objects.filter(
    Q(harga__lt=100000) | Q(reviews__isnull=False)
)

# Exclude
buku_tanpa_review = Buku.objects.exclude(reviews__isnull=False)

# F expressions (database-level operations)
from django.db.models import F
Buku.objects.filter(stok__gt=F('stok_minimal')).update(status='available')

# Bulk operations
books_to_create = [
    Buku(judul=f"Book {i}", harga=100000) for i in range(1000)
]
Buku.objects.bulk_create(books_to_create, batch_size=500)

# Bulk update
Buku.objects.filter(kategori='teknologi').update(diskon=0.1)
```

---

## 🏗 Checkpoint Level 5

text

```
PEMAHAMAN:
├── PostgreSQL setup dan configuration
├── Django ORM dengan relationships
├── Migrations dan schema evolution
├── Query optimization (prefetch_related, select_related)
├── Aggregation dan annotation
├── Complex filters dengan Q objects
└── Bulk operations untuk performance

PROYEK: Production-Ready Database
├── PostgreSQL configured
├── Models with relationships:
│   ├── Author (one-to-many)
│   ├── Buku (many-to-many dengan Author)
│   ├── Publisher (one-to-many)
│   ├── Peminjaman
│   ├── Review
│   └── Denda
├── Migrations tracked in git
├── Data fixtures untuk development
├── Admin interface optimized dengan list_display, list_filter
├── Query performance optimized
└── Proper indexing

STRUKTUR:
perpustakaan/
├── models.py — All models defined
├── migrations/ — Migration files
├── fixtures/ — Sample data
├── admin.py — Admin customization
└── tests/ — Model tests

HABIT:
├── Always use migrations (never alter DB directly)
├── Optimize queries (select_related, prefetch_related)
├── Use aggregation untuk stats, bukan Python loops
├── Proper indexing untuk frequently queried fields
├── Bulk operations untuk large datasets
└── Fixtures untuk reproducible test data
```

**Commit**: `feat: add PostgreSQL, complex relationships, migrations`

---

# 🟡 LEVEL 6: REST API & TESTING (Minggu 17-21)

**Tema**: "Transform web app menjadi REST API dengan Django REST Framework"

**Benang Merah**: Django views (Level 4) → Serializers → REST API → Testing

**Output**: Production-ready REST API dengan tests dan documentation

---

## M. Django REST Framework

### 24. [[24. DRF Serializers dan ViewSets]]

Bash

```
pip install djangorestframework
```

**File: perpustakaan/serializers.py**

Python

```
from rest_framework import serializers
from .models import Buku, Author, Peminjaman

class AuthorSerializer(serializers.ModelSerializer):
    """Serializer untuk Author"""
    
    class Meta:
        model = Author
        fields = ['id', 'nama', 'bio']

class BukuSerializer(serializers.ModelSerializer):
    """Serializer untuk Buku"""
    
    authors = AuthorSerializer(many=True, read_only=True)
    author_ids = serializers.PrimaryKeyRelatedField(
        queryset=Author.objects.all(),
        write_only=True,
        many=True,
        source='authors'
    )
    
    class Meta:
        model = Buku
        fields = [
            'id', 'judul', 'harga', 'stok',
            'authors', 'author_ids', 'tersedia'
        ]
        read_only_fields = ['id', 'tersedia']
    
    def create(self, validated_data):
        authors = validated_data.pop('authors')
        buku = Buku.objects.create(**validated_data)
        buku.authors.set(authors)
        return buku
    
    def update(self, instance, validated_data):
        authors = validated_data.pop('authors', None)
        for attr, value in validated_data.items():
            setattr(instance, attr, value)
        instance.save()
        if authors:
            instance.authors.set(authors)
        return instance

class PeminjamanSerializer(serializers.ModelSerializer):
    """Serializer untuk Peminjaman"""
    
    buku = BukuSerializer(read_only=True)
    buku_id = serializers.PrimaryKeyRelatedField(
        queryset=Buku.objects.all(),
        write_only=True,
        source='buku'
    )
    
    class Meta:
        model = Peminjaman
        fields = ['id', 'buku', 'buku_id', 'user', 'tanggal_pinjam', 'batas_kembali']
        read_only_fields = ['id', 'tanggal_pinjam', 'user']
    
    def create(self, validated_data):
        peminjaman = Peminjaman.objects.create(
            user=self.context['request'].user,
            **validated_data
        )
        return peminjaman
```

**File: perpustakaan/views.py (DRF views)**

Python

```
from rest_framework import viewsets, status, filters
from rest_framework.decorators import action
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated
from django_filters.rest_framework import DjangoFilterBackend
from .models import Buku, Peminjaman
from .serializers import BukuSerializer, PeminjamanSerializer

class BukuViewSet(viewsets.ModelViewSet):
    """ViewSet untuk Buku"""
    
    queryset = Buku.objects.all()
    serializer_class = BukuSerializer
    permission_classes = [IsAuthenticated]
    filter_backends = [DjangoFilterBackend, filters.SearchFilter, filters.OrderingFilter]
    filterset_fields = ['kategori', 'stok__gt']
    search_fields = ['judul', 'pengarang']
    ordering_fields = ['harga', 'created_at']
    ordering = ['-created_at']
    
    @action(detail=False, methods=['get'])
    def tersedia(self, request):
        """Get hanya buku yang tersedia"""
        buku = self.get_queryset().filter(stok__gt=0)
        serializer = self.get_serializer(buku, many=True)
        return Response(serializer.data)
    
    @action(detail=True, methods=['post'])
    def pinjam(self, request, pk=None):
        """Pinjam buku"""
        buku = self.get_object()
        
        if not buku.tersedia():
            return Response(
                {'error': 'Stok habis'},
                status=status.HTTP_400_BAD_REQUEST
            )
        
        peminjaman = Peminjaman.objects.create(
            buku=buku,
            user=request.user
        )
        
        serializer = PeminjamanSerializer(peminjaman)
        return Response(serializer.data, status=status.HTTP_201_CREATED)

class PeminjamanViewSet(viewsets.ModelViewSet):
    """ViewSet untuk Peminjaman"""
    
    serializer_class = PeminjamanSerializer
    permission_classes = [IsAuthenticated]
    filter_backends = [DjangoFilterBackend]
    filterset_fields = ['buku', 'user']
    
    def get_queryset(self):
        """User hanya bisa lihat peminjaman mereka sendiri"""
        return Peminjaman.objects.filter(user=self.request.user)
    
    @action(detail=True, methods=['post'])
    def kembalikan(self, request, pk=None):
        """Kembalikan buku"""
        peminjaman = self.get_object()
        
        if peminjaman.tanggal_kembali:
            return Response(
                {'error': 'Buku sudah dikembalikan'},
                status=status.HTTP_400_BAD_REQUEST
            )
        
        peminjaman.tanggal_kembali = timezone.now()
        peminjaman.save()
        
        serializer = self.get_serializer(peminjaman)
        return Response(serializer.data)
```

**File: perpustakaan/urls.py (DRF routing)**

Python

```
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from . import views

router = DefaultRouter()
router.register(r'buku', views.BukuViewSet)
router.register(r'peminjaman', views.PeminjamanViewSet, basename='peminjaman')

urlpatterns = [
    path('', include(router.urls)),
    path('api-auth/', include('rest_framework.urls')),
]
```

### 25. [[25. Testing dengan Django]]

Bash

```
pip install pytest pytest-django factory-boy
```

**File: perpustakaan/tests/test_models.py**

Python

```
import pytest
from django.utils import timezone
from datetime import timedelta
from perpustakaan.models import Buku, User, Peminjaman
from factory import DjangoModelFactory, Faker

class BukuFactory(DjangoModelFactory):
    class Meta:
        model = Buku
    
    judul = Faker('sentence')
    pengarang = Faker('name')
    harga = 150000
    stok = 5

class UserFactory(DjangoModelFactory):
    class Meta:
        model = User
    
    nama = Faker('name')
    email = Faker('email')

@pytest.mark.django_db
class TestBuku:
    """Tests untuk Buku model"""
    
    def test_create_buku(self):
        buku = BukuFactory(judul="Clean Code")
        assert buku.id
        assert buku.judul == "Clean Code"
    
    def test_tersedia(self):
        buku = BukuFactory(stok=5)
        assert buku.tersedia() == True
        
        buku_habis = BukuFactory(stok=0)
        assert buku_habis.tersedia() == False
    
    def test_pinjam(self):
        buku = BukuFactory(stok=5)
        assert buku.stok == 5
        
        buku.pinjam()
        assert buku.stok == 4
    
    def test_pinjam_habis(self):
        buku = BukuFactory(stok=0)
        result = buku.pinjam()
        assert result == False
        assert buku.stok == 0

@pytest.mark.django_db
class TestPeminjaman:
    """Tests untuk Peminjaman model"""
    
    def test_create_peminjaman(self):
        buku = BukuFactory()
        user = UserFactory()
        
        peminjaman = Peminjaman.objects.create(
            buku=buku,
            user=user,
            batas_kembali=timezone.now() + timedelta(days=14)
        )
        
        assert peminjaman.id
        assert peminjaman.buku == buku
        assert peminjaman.user == user
    
    def test_is_overdue(self):
        buku = BukuFactory()
        user = UserFactory()
        
        peminjaman = Peminjaman.objects.create(
            buku=buku,
            user=user,
            batas_kembali=timezone.now() - timedelta(days=1)
        )
        
        assert peminjaman.is_overdue() == True
```

**File: perpustakaan/tests/test_api.py**

Python

```
import pytest
from rest_framework.test import APIClient
from perpustakaan.models import Buku
from perpustakaan.tests.test_models import BukuFactory

@pytest.mark.django_db
class TestBukuAPI:
    """Tests untuk Buku API endpoints"""
    
    def setup_method(self):
        self.client = APIClient()
        self.buku = BukuFactory(judul="Test Book")
    
    def test_get_buku_list(self):
        response = self.client.get('/api/buku/')
        assert response.status_code == 200
        assert len(response.data) >= 1
    
    def test_get_buku_detail(self):
        response = self.client.get(f'/api/buku/{self.buku.id}/')
        assert response.status_code == 200
        assert response.data['judul'] == "Test Book"
    
    def test_create_buku_unauthorized(self):
        response = self.client.post('/api/buku/', {
            'judul': 'New Book',
            'pengarang': 'Author',
            'harga': 100000,
            'stok': 5
        })
        assert response.status_code == 401  # Unauthorized
```

**Run tests:**

Bash

```
# Run all tests
pytest

# Run specific test
pytest perpustakaan/tests/test_models.py::TestBuku::test_tersedia

# With coverage
pytest --cov=perpustakaan
```

---

## 🏗 Checkpoint Level 6

text

```
PEMAHAMAN:
├── DRF serializers dan validation
├── ViewSets dan routers
├── Permissions dan authentication
├── Filtering, searching, ordering
├── Custom actions (@action decorator)
├── Unit testing dengan pytest
├── Factory pattern untuk test data
├── API documentation (Swagger optional)
└── Error handling dan status codes

PROYEK: Production-Ready REST API
├── BukuViewSet dengan CRUD
├── PeminjamanViewSet dengan custom actions
├── Proper permissions (IsAuthenticated, IsAdminUser)
├── Search, filter, ordering di endpoints
├── Custom actions (pinjam, kembalikan, tersedia)
├── Comprehensive tests:
│   ├── Model tests
│   ├── Serializer tests
│   ├── API endpoint tests
│   └── Permission tests
├── >80% code coverage
├── Error responses dengan proper status codes
└── API documentation (docstrings)

ENDPOINTS:
├── GET /api/buku/ — List with filters
├── GET /api/buku/:id/ — Detail
├── POST /api/buku/ — Create
├── PUT /api/buku/:id/ — Update
├── DELETE /api/buku/:id/ — Delete
├── POST /api/buku/:id/pinjam/ — Custom action
├── GET /api/buku/tersedia/ — Custom list action
├── POST /api/peminjaman/:id/kembalikan/ — Return book
└── GET /api/peminjaman/ — My loans

HABIT:
├── Serializers for validation
├── ViewSets for consistency
├── Permissions first
├── Test-driven development
├── Factory pattern untuk tests
└── Good error messages
```

**Commit**: `feat: build REST API with DRF, serializers, and comprehensive tests`

---

# ⚫ LEVEL 7: DATA SCIENCE & ANALYTICS (Minggu 21-25)

**Tema**: "Python's strength — Data manipulation, analysis, visualization"

**Benang Merah**: Web app data (Level 6) → Data extraction → Analysis → Visualization

**Output**: Analytics dashboard dengan insights dari perpustakaan data

---

## N. Data Science Libraries

### 26. [[26. Pandas untuk Data Manipulation]]

Bash

```
pip install pandas numpy matplotlib seaborn plotly
```

**File: analytics/analysis.py**

Python

```
import pandas as pd
import numpy as np
from perpustakaan.models import Buku, Peminjaman

# Load data dari database ke Pandas
buku_data = Buku.objects.values_list('id', 'judul', 'harga', 'stok', 'kategori')
df_buku = pd.DataFrame(buku_data, columns=['id', 'judul', 'harga', 'stok', 'kategori'])

# Basic operations
print(df_buku.head())  # First 5 rows
print(df_buku.info())  # Data types dan info
print(df_buku.describe())  # Statistics

# Filtering
buku_murah = df_buku[df_buku['harga'] < 100000]
buku_teknologi = df_buku[df_buku['kategori'] == 'teknologi']

# Groupby dan aggregation
stats_kategori = df_buku.groupby('kategori').agg({
    'harga': ['mean', 'min', 'max'],
    'stok': 'sum',
    'id': 'count',
}).round(2)

# Sorting
buku_termahal = df_buku.sort_values('harga', ascending=False)

# Calculate new columns
df_buku['nilai_stok'] = df_buku['harga'] * df_buku['stok']
df_buku['kategori_upper'] = df_buku['kategori'].str.upper()

# Merge datasets
peminjaman_data = Peminjaman.objects.filter(
    tanggal_kembali__isnull=True
).values('buku_id', 'user__nama')
df_peminjaman = pd.DataFrame(peminjaman_data)

df_combined = pd.merge(
    df_buku,
    df_peminjaman,
    left_on='id',
    right_on='buku_id',
    how='left'
)

# Pivot table
tabel_pivot = df_buku.pivot_table(
    index='kategori',
    values=['harga', 'stok'],
    aggfunc={'harga': 'mean', 'stok': 'sum'}
)

# Save to CSV
df_buku.to_csv('data/buku_export.csv', index=False)
```

### 27. [[27. Data Visualization dengan Matplotlib & Plotly]]

Python

```
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px
import plotly.graph_objects as go

# Set style
sns.set_style("darkgrid")
plt.rcParams['figure.figsize'] = (12, 6)

# Bar chart — Jumlah buku per kategori
kategori_count = df_buku['kategori'].value_counts()
plt.figure()
kategori_count.plot(kind='bar', color='skyblue')
plt.title('Jumlah Buku per Kategori')
plt.xlabel('Kategori')
plt.ylabel('Jumlah')
plt.tight_layout()
plt.savefig('charts/kategori_count.png')

# Histogram — Distribusi harga
plt.figure()
plt.hist(df_buku['harga'], bins=20, color='green', alpha=0.7)
plt.title('Distribusi Harga Buku')
plt.xlabel('Harga (Rp)')
plt.ylabel('Frekuensi')
plt.savefig('charts/price_distribution.png')

# Interactive chart dengan Plotly
fig = px.bar(
    df_buku.groupby('kategori')['id'].count().reset_index(),
    x='kategori',
    y='id',
    title='Jumlah Buku per Kategori',
    labels={'id': 'Jumlah', 'kategori': 'Kategori'}
)
fig.write_html('charts/kategori_interactive.html')

# Scatter plot — Harga vs Stok
fig = px.scatter(
    df_buku,
    x='harga',
    y='stok',
    color='kategori',
    size='stok',
    title='Harga vs Stok Buku'
)
fig.show()

# Time series — Peminjaman per bulan
df_peminjaman['tanggal_pinjam'] = pd.to_datetime(
    df_peminjaman['tanggal_pinjam']
)
peminjaman_per_bulan = df_peminjaman.set_index(
    'tanggal_pinjam'
).resample('M').size()

plt.figure()
peminjaman_per_bulan.plot(kind='line')
plt.title('Peminjaman per Bulan')
plt.xlabel('Bulan')
plt.ylabel('Jumlah Peminjaman')
plt.savefig('charts/peminjaman_timeline.png')
```

### 28. [[28. Django View untuk Analytics]]

**File: perpustakaan/views.py (add analytics view)**

Python

```
from django.shortcuts import render
from django.db.models import Count, Avg, Sum, F
from django.db.models.functions import TruncMonth
import json
import pandas as pd

def analytics_dashboard(request):
    """Dashboard dengan analytics"""
    
    # Stats
    total_buku = Buku.objects.count()
    total_stok = Buku.objects.aggregate(Sum('stok'))['stok__sum'] or 0
    total_peminjaman = Peminjaman.objects.count()
    avg_harga = Buku.objects.aggregate(Avg('harga'))['harga__avg'] or 0
    
    # Top books
    top_books = Buku.objects.annotate(
        review_count=Count('reviews')
    ).order_by('-review_count')[:5]
    
    # Books per category
    kategori_stats = Buku.objects.values('kategori').annotate(
        count=Count('id'),
        avg_harga=Avg('harga')
    )
    
    # Peminjaman timeline
    peminjaman_timeline = Peminjaman.objects.annotate(
        bulan=TruncMonth('tanggal_pinjam')
    ).values('bulan').annotate(
        count=Count('id')
    ).order_by('bulan')
    
    # Prepare data untuk chart
    kategori_labels = [k['kategori'] for k in kategori_stats]
    kategori_counts = [k['count'] for k in kategori_stats]
    
    timeline_dates = [p['bulan'].strftime('%Y-%m') for p in peminjaman_timeline]
    timeline_counts = [p['count'] for p in peminjaman_timeline]
    
    context = {
        'total_buku': total_buku,
        'total_stok': total_stok,
        'total_peminjaman': total_peminjaman,
        'avg_harga': f'{avg_harga:,.0f}',
        'top_books': top_books,
        'kategori_labels': json.dumps(kategori_labels),
        'kategori_counts': json.dumps(kategori_counts),
        'timeline_dates': json.dumps(timeline_dates),
        'timeline_counts': json.dumps(timeline_counts),
    }
    
    return render(request, 'perpustakaan/analytics.html', context)
```

**File: perpustakaan/templates/perpustakaan/analytics.html**

HTML

```
{% extends "base.html" %}
{% load static %}

{% block content %}
<div class="dashboard">
    <h1>Analytics Dashboard</h1>
    
    <!-- Stats Cards -->
    <div class="stats-grid">
        <div class="stat-card">
            <h3>Total Buku</h3>
            <p class="number">{{ total_buku }}</p>
        </div>
        <div class="stat-card">
            <h3>Total Stok</h3>
            <p class="number">{{ total_stok }}</p>
        </div>
        <div class="stat-card">
            <h3>Total Peminjaman</h3>
            <p class="number">{{ total_peminjaman }}</p>
        </div>
        <div class="stat-card">
            <h3>Rata-rata Harga</h3>
            <p class="number">{{ avg_harga }}</p>
        </div>
    </div>
    
    <!-- Charts -->
    <div class="charts-grid">
        <div class="chart-container">
            <canvas id="kategoriChart"></canvas>
        </div>
        <div class="chart-container">
            <canvas id="timelineChart"></canvas>
        </div>
    </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
    // Kategori chart
    new Chart(document.getElementById('kategoriChart'), {
        type: 'bar',
        data: {
            labels: {{ kategori_labels|safe }},
            datasets: [{
                label: 'Jumlah Buku',
                data: {{ kategori_counts|safe }},
                backgroundColor: 'rgba(75, 192, 192, 0.8)',
            }]
        }
    });
    
    // Timeline chart
    new Chart(document.getElementById('timelineChart'), {
        type: 'line',
        data: {
            labels: {{ timeline_dates|safe }},
            datasets: [{
                label: 'Peminjaman',
                data: {{ timeline_counts|safe }},
                borderColor: 'rgb(75, 192, 192)',
                tension: 0.1
            }]
        }
    });
</script>
{% endblock %}
```

---

## 🏗 Checkpoint Level 7

text

```
PEMAHAMAN:
├── Pandas DataFrame operations
├── Data cleaning dan transformation
├── Aggregation dan groupby
├── Merging datasets
├── Data visualization with Matplotlib/Seaborn
├── Interactive charts dengan Plotly
├── Analytics views di Django
└── Dashboard creation

PROYEK: Analytics Dashboard
├── Django view untuk dashboard
├── Statistics cards (total, average, count)
├── Bar chart — Books per category
├── Line chart — Loans per month
├── Top books ranking
├── Heatmap (optional) — Activity matrix
├── Export data ke CSV
├── Responsive design
└── Real-time data (optional)

FEATURES:
├── Total books, stock, loans statistics
├── Category distribution
├── Price analysis (min, max, average)
├── Loan trends over time
├── Top borrowed books
├── Most active members
├── Revenue analysis (optional)
└── Forecast (optional with Prophet)

HABIT:
├── Use Pandas untuk data manipulation
├── Matplotlib/Seaborn untuk static charts
├── Plotly untuk interactive charts
├── Cache dashboard data (expensive operations)
├── Separate data analysis dari views
└── Good labeling dan titles di charts
```

**Commit**: `feat: add analytics dashboard with data visualization`

---

# 🟣 LEVEL 8: ADVANCED TOPICS (Minggu 25+)

**Tema**: "Machine Learning, async tasks, deployment"

---

## O. Advanced Topics (Choose One Path)

### 29. [[29. Machine Learning dengan Scikit-Learn (Optional)]]

Bash

```
pip install scikit-learn
```

**Recommendation engine example:**

Python

```
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity
import pandas as pd

# Load buku data
buku_df = pd.DataFrame(list(Buku.objects.values()))

# Create TF-IDF matrix dari deskripsi
vectorizer = TfidfVectorizer()
tfidf_matrix = vectorizer.fit_transform(buku_df['deskripsi'].fillna(''))

# Compute similarity
similarity = cosine_similarity(tfidf_matrix)

def recommend_books(buku_id, n=5):
    """Recommend similar books"""
    idx = buku_df[buku_df['id'] == buku_id].index[0]
    scores = similarity[idx]
    similar_indices = scores.argsort()[-n-1:-1][::-1]
    return buku_df.iloc[similar_indices]
```

### 30. [[30. Celery untuk Async Tasks]]

Bash

```
pip install celery redis
```

Python

```
from celery import shared_task
from django.core.mail import send_mail

@shared_task
def send_reminder_email(peminjaman_id):
    """Send overdue reminder asynchronously"""
    peminjaman = Peminjaman.objects.get(id=peminjaman_id)
    
    if peminjaman.is_overdue():
        send_mail(
            subject='Reminder: Buku sudah overdue',
            message=f'Buku "{peminjaman.buku.judul}" sudah terlambat 3 hari',
            from_email='noreply@perpustakaan.com',
            recipient_list=[peminjaman.user.email],
        )

# Call task
send_reminder_email.delay(peminjaman_id=1)
```

---

# 📊 Ringkasan Progress Tracking

text

```
Level 1: Python Fundamentals → Data types, functions, control flow
 + Level 2: Libraries & File I/O → Modules, virtual env, JSON
 + Level 3: OOP → Classes, inheritance, design patterns
 + Level 4: Django & Web → Models, views, templates
 + Level 5: Database & Migrations → PostgreSQL, relationships
 + Level 6: REST API & Testing → DRF, serializers, pytest
 + Level 7: Data Science → Pandas, visualization, analytics
 + Level 8: Advanced → Machine Learning / Async Tasks
```

## Tabel Progress

|Level|Poin|Durasi|Output Konkret|
|---|---|---|---|
|🟢 1|1-7|Minggu 1-3|CLI app dengan functions|
|🔵 2|8-12|Minggu 3-6|Modules dengan file persistence|
|🟡 3|13-16|Minggu 6-9|OOP dengan design patterns|
|🟠 4|17-19|Minggu 9-13|Django web app|
|🔴 5|20-23|Minggu 13-17|PostgreSQL dengan migrations|
|⚫ 6|24-25|Minggu 17-21|REST API dengan tests|
|🟡 7|26-28|Minggu 21-25|Analytics dashboard|
|🟣 8|29-30|Minggu 25+|ML atau advanced patterns|

---

# ⚠️ Aturan yang Tidak Boleh Dilanggar

1. **ALWAYS use virtual environment**: `python -m venv venv`
2. **ALWAYS use type hints**: Clarity over cleverness
3. **ALWAYS validate input**: At API boundary AND service layer
4. **ALWAYS use migrations**: Never alter DB directly
5. **ALWAYS write tests**: Test-driven development
6. **ALWAYS use `.env`** untuk config, bukan hardcoded values
7. **ALWAYS paginate large results**: Performance matters
8. **ALWAYS optimize queries**: select_related, prefetch_related
9. **ALWAYS handle exceptions**: Try-except dengan specific exceptions
10. **ALWAYS document code**: Docstrings dan comments are not optional

---

**Roadmap Python v1.0 — Dari Hello World ke Production ML Applications**

Setiap baris kode ditulis dengan tujuan dan pemahaman. Python adalah bahasa yang indah — hargai keindahannya dengan menulis code yang Pythonic, readable, dan maintainable.