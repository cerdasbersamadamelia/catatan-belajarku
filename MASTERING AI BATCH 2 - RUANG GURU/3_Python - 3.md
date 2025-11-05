# Python - 3 🐍

---

## 📋 Agenda Hari Ini

1. Control Structure & Looping (lanjutan)
2. List Comprehension
3. Filter & Map dengan Lambda
4. Module & Package
5. Working with Files (CSV)
6. Virtual Environment
7. Object-Oriented Programming (OOP) - Intro

---

## 🔄 Control Structure & Looping Review

### Latihan: Menjumlahkan Bilangan Genap

**Soal:** Jumlahkan semua bilangan genap dari 1 sampai 50

**Cara 1: Loop Biasa**
```python
n = 50
sum_genap = 0

for i in range(n + 1):  # Ingat: range(n) hanya sampai n-1, jadi pakai n+1
    if i % 2 == 0:  # Cek apakah genap (sisa bagi 2 = 0)
        sum_genap += i  # Sama dengan: sum_genap = sum_genap + i

print(sum_genap)  # Output: 650
```

**Penting:**
- `range(n)` hanya sampai `n-1`, jadi pakai `range(n + 1)` untuk sampai `n`
- Modulo `%` adalah operasi sisa bagi
- `i % 2 == 0` → bilangan genap
- `i % 2 == 1` → bilangan ganjil

---

### Cara 2: List Comprehension

```python
n = 50
sum_genap = sum([i for i in range(n + 1) if i % 2 == 0])
print(sum_genap)  # Output: 650
```

**Lebih singkat dan Pythonic!** 🎯

---

### Cara 3: Filter dengan Lambda

```python
n = 50
numbers = range(n + 1)
even_numbers = filter(lambda x: x % 2 == 0, numbers)
sum_genap = sum(list(even_numbers))
print(sum_genap)  # Output: 650
```

---

## 🐛 Tips Debugging

### 1. Print untuk Debug
```python
for i in range(n + 1):
    print(f"i: {i}")  # Lihat nilai i
    if i % 2 == 0:
        sum_genap += i
        print(f"sum: {sum_genap}")  # Lihat sum bertambah
```

### 2. Pakai Debugger (Google Colab)
- Klik di sebelah kiri nomor baris → bikin breakpoint
- Jalankan dalam debug mode
- Step by step lihat nilai variabel
- Bisa add to Watch untuk variabel tertentu

**Kelebihan Print:** Cepat, langsung kelihatan semua  
**Kelebihan Debugger:** Lebih detail, bisa pause & inspect

---

## 🚨 Warning Penting!

### Jangan Gunakan Nama Variabel = Nama Function Bawaan

**SALAH:**
```python
sum = 0  # ❌ Ini akan override function sum() bawaan Python!
numbers = [1, 2, 3]
total = sum(numbers)  # Error: 'int' object is not callable
```

**BENAR:**
```python
total = 0  # ✅ Gunakan nama lain
jumlah = 0  # ✅ Atau pakai bahasa Indonesia
sum_nilai = 0  # ✅ Atau kombinasi
```

**Solusi kalau sudah terlanjur:**
Restart notebook/kernel buat reset semua variabel

---

## 🎨 List Comprehension

### Apa itu List Comprehension?

Cara singkat bikin list dengan kondisi/transformasi

**Sintaks:**
```python
[expression for item in iterable if condition]
```

### Contoh Sederhana

**Cara Lama (Loop Biasa):**
```python
numbers = []
for i in range(10):
    numbers.append(i)
print(numbers)  # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

**Cara Baru (List Comprehension):**
```python
numbers = [i for i in range(10)]
print(numbers)  # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### Dengan Kondisi

```python
# Hanya bilangan genap
even_numbers = [i for i in range(10) if i % 2 == 0]
print(even_numbers)  # [0, 2, 4, 6, 8]

# Hanya bilangan ganjil
odd_numbers = [i for i in range(10) if i % 2 == 1]
print(odd_numbers)  # [1, 3, 5, 7, 9]
```

### Dengan Transformasi

```python
# Kalikan 2
doubles = [i * 2 for i in range(10)]
print(doubles)  # [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

# Kuadratkan
squares = [i ** 2 for i in range(10)]
print(squares)  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

---

## 🔍 Filter & Map dengan Lambda

### Lambda Function

**Apa itu Lambda?**
Anonymous function (fungsi tanpa nama) untuk operasi singkat

**Sintaks:**
```python
lambda parameter: expression
```

**Contoh:**
```python
# Function biasa
def double(x):
    return x * 2

# Lambda (lebih singkat)
double = lambda x: x * 2

print(double(5))  # Output: 10
```

**Kapan pakai Lambda?**
- Operasi simpel (1 baris)
- Dipakai sekali aja
- Sebagai parameter function lain (filter, map, dll)

**PENTING:** Lambda tidak pakai `return`!

---

### Filter

**Tujuan:** Menyaring/filter data berdasarkan kondisi

**Sintaks:**
```python
filter(function, iterable)
```

**Contoh:**
```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Filter bilangan genap
even = filter(lambda x: x % 2 == 0, numbers)
even_list = list(even)  # Jangan lupa convert ke list!
print(even_list)  # [2, 4, 6, 8, 10]

# Filter bilangan > 5
greater_than_5 = filter(lambda x: x > 5, numbers)
print(list(greater_than_5))  # [6, 7, 8, 9, 10]
```

**Ingat:** `filter()` mengembalikan filter object, harus di-convert ke `list()` dulu!

---

### Map

**Tujuan:** Transformasi/ubah setiap elemen dalam list

**Sintaks:**
```python
map(function, iterable)
```

**Contoh:**
```python
numbers = [1, 2, 3, 4, 5]

# Kalikan setiap elemen dengan 2
doubles = map(lambda x: x * 2, numbers)
print(list(doubles))  # [2, 4, 6, 8, 10]

# Kuadratkan setiap elemen
squares = map(lambda x: x ** 2, numbers)
print(list(squares))  # [1, 4, 9, 16, 25]
```

---

### Kombinasi Filter + Map

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# 1. Kalikan semua dengan 2
doubled = map(lambda x: x * 2, numbers)

# 2. Filter yang < 15
filtered = filter(lambda x: x < 15, doubled)

# 3. Hasil akhir
result = list(filtered)
print(result)  # [2, 4, 6, 8, 10, 12, 14]
```

**Step by step:**
1. `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`
2. Setelah map: `[2, 4, 6, 8, 10, 12, 14, 16, 18, 20]`
3. Setelah filter: `[2, 4, 6, 8, 10, 12, 14]`

---

## 📦 Module & Package

### Apa itu Module?

**Module** = File Python (`.py`) yang berisi function, class, atau variabel

**Tujuan:**
- Organisasi code yang rapi
- Reusability (pakai ulang)
- Kolaborasi lebih mudah
- Modular & maintainable

**Analogi:** Seperti main Lego - bikin part by part, terus sambung-sambung!

---

### Membuat Module

**Contoh Struktur:**
```
project/
├── area.py       # Module untuk hitung luas
├── volume.py     # Module untuk hitung volume
└── main.py       # File utama
```

**File: `area.py`**
```python
# area.py - Module untuk menghitung luas

def circle(radius):
    return 3.14 * radius ** 2

def square(side):
    return side ** 2

def rectangle(length, width):
    return length * width

def triangle(base, height):
    return 0.5 * base * height
```

**File: `volume.py`**
```python
# volume.py - Module untuk menghitung volume

def cube(side):
    return side ** 3

def sphere(radius):
    return (4/3) * 3.14 * radius ** 3

def cylinder(radius, height):
    return 3.14 * radius ** 2 * height
```

---

### Menggunakan Module

**File: `main.py`**

**Cara 1: Import Module**
```python
import area

# Pakai dengan prefix nama module
luas = area.rectangle(2, 3)
print(luas)  # Output: 6
```

**Cara 2: Import Specific Function**
```python
from area import rectangle, circle, square

# Pakai langsung tanpa prefix
luas = rectangle(2, 3)
print(luas)  # Output: 6

luas_circle = circle(5)
print(luas_circle)  # Output: 78.5
```

**Cara 3: Import Semua (Tidak Disarankan!)**
```python
from area import *

# Pakai semua function
luas = rectangle(2, 3)
lingkaran = circle(5)
```

**⚠️ Warning:** `import *` bisa bikin konflik nama function!

---

### Import dari Subfolder

**Struktur:**
```
project/
├── statistics/
│   ├── __init__.py       # ← PENTING! Penanda ini package
│   └── basic.py
└── main.py
```

**Import dari subfolder:**
```python
from statistics.basic import mean, median

# Atau
import statistics.basic

rata_rata = statistics.basic.mean([1, 2, 3, 4, 5])
```

**PENTING:** Folder harus punya file `__init__.py` (bisa kosong) biar dianggap sebagai package!

---

### Install Package dari Internet

**Gunakan pip (Package Manager Python):**

```bash
# Install package
pip install manim

# Uninstall package
pip uninstall manim

# List semua package terinstall
pip list
```

**Contoh Pakai Package External:**
```python
from manim import *

# Bikin animasi lingkaran
class CreateCircle(Scene):
    def construct(self):
        circle = Circle()
        circle.set_fill(PINK, opacity=0.5)
        self.play(Create(circle))
```

---

### Requirements.txt

**Untuk share dependencies project:**

**Bikin `requirements.txt`:**
```bash
pip freeze > requirements.txt
```

**Install dari `requirements.txt`:**
```bash
pip install -r requirements.txt
```

**Isi `requirements.txt`:**
```
numpy==1.24.0
pandas==1.5.3
matplotlib==3.7.0
```

---

## 📄 Working with Files

### Membaca File

**Sintaks:**
```python
with open('filename.txt', 'r') as file:
    data = file.read()
    print(data)
```

**Mode File:**
- `'r'` - Read (baca)
- `'w'` - Write (tulis, timpa isi lama)
- `'a'` - Append (tambah di akhir)
- `'x'` - Create (bikin baru, error kalau sudah ada)

**Contoh:**
```python
# Baca file
with open('file.txt', 'r') as file:
    content = file.read()
    print(content)
```

---

### Menulis File

```python
# Tulis (timpa isi lama)
with open('file.txt', 'w') as file:
    file.write('Hello World!')

# Tambah di akhir (tidak timpa)
with open('file.txt', 'a') as file:
    file.write('\nBaris baru')
```

**PENTING:** Gunakan `with open()` supaya file otomatis ditutup!

---

## 📊 Working with CSV Files

### Apa itu CSV?

**CSV = Comma Separated Values**

Contoh isi file `users.csv`:
```
user_id,name,email,age
1,John Doe,john@email.com,25
2,Jane Smith,jane@email.com,30
3,James Brown,james@email.com,28
```

**Struktur:**
- Baris pertama = header (nama kolom)
- Baris selanjutnya = data
- Dipisah dengan koma (`,`)

---

### Membaca CSV

```python
import csv

# Baca CSV
with open('users.csv', 'r') as file:
    reader = csv.DictReader(file)
    
    # Ambil header
    fieldnames = reader.fieldnames
    print(fieldnames)  # ['user_id', 'name', 'email', 'age']
    
    # Baca setiap baris
    for row in reader:
        print(row['name'], row['age'])
```

**Output:**
```
John Doe 25
Jane Smith 30
James Brown 28
```

---

### Menulis CSV

```python
import csv

# Data
fieldnames = ['user_id', 'name', 'email', 'age']
data = [
    {'user_id': 1, 'name': 'John', 'email': 'john@email.com', 'age': 25},
    {'user_id': 2, 'name': 'Jane', 'email': 'jane@email.com', 'age': 30}
]

# Tulis CSV
with open('output.csv', 'w', newline='') as file:
    writer = csv.DictWriter(file, fieldnames=fieldnames)
    
    # Tulis header
    writer.writeheader()
    
    # Tulis data
    for row in data:
        writer.writerow(row)
```

---

### Filter Data CSV

**Contoh: Filter user umur ≥ 30**

```python
import csv

input_file = 'users.csv'
output_file = 'filtered_users.csv'

with open(input_file, 'r') as infile, open(output_file, 'w') as outfile:
    reader = csv.DictReader(infile)
    fieldnames = reader.fieldnames
    
    writer = csv.DictWriter(outfile, fieldnames=fieldnames)
    writer.writeheader()
    
    for row in reader:
        if int(row['age']) >= 30:  # Filter kondisi
            writer.writerow(row)
```

**Hasil:** File baru berisi hanya user umur ≥ 30

---

## 🌍 Virtual Environment

### Apa itu Virtual Environment?

**Virtual Environment** = Isolated Python environment untuk setiap project

**Analogi:** Seperti sandbox/kotak pasir terpisah untuk setiap project

**Kenapa Penting?**
- Setiap project punya dependencies sendiri
- Tidak konflik antar project
- Bisa pakai versi package berbeda per project
- Production-ready

---

### Membuat Virtual Environment

**Command:**
```bash
# Bikin venv
python -m venv nama_folder

# Contoh:
python -m venv env
```

**Struktur folder setelah dibuat:**
```
project/
├── env/              # ← Virtual environment
│   ├── bin/         # (Mac/Linux)
│   ├── Scripts/     # (Windows)
│   ├── lib/
│   └── ...
└── main.py
```

---

### Aktivasi Virtual Environment

**Mac/Linux:**
```bash
source env/bin/activate
```

**Windows:**
```bash
env\Scripts\activate
```

**Tanda sudah aktif:**
```bash
(env) user@computer:~/project$
```
↑ Ada `(env)` di depan!

---

### Deaktivasi

```bash
deactivate
```

---

### Install Package di Virtual Environment

```bash
# Pastikan venv sudah aktif
(env) $ pip install numpy

# Package hanya terinstall di venv ini
# Project lain tidak terpengaruh!
```

---

### Best Practice Virtual Environment

✅ **DO:**
- Bikin venv untuk setiap project
- Simpan `requirements.txt`
- Aktifkan sebelum coding

❌ **DON'T:**
- Commit folder `env/` ke Git
- Install package global kalau bisa pakai venv
- Lupa deactivate sebelum ganti project

---

### Anaconda vs Virtualenv

| Anaconda | Virtualenv |
|----------|------------|
| All-in-one (banyak package pre-installed) | Minimal, install sendiri |
| Lebih besar (storage) | Lebih kecil |
| Cocok untuk pemula | Cocok untuk production |
| GUI tersedia (Anaconda Navigator) | Command-line based |

**Rekomendasi:**
- **Pemula:** Pakai Anaconda
- **Advanced:** Pakai virtualenv/venv

---

## 🎯 Object-Oriented Programming (OOP) - Intro

### Apa itu OOP?

**OOP** = Cara programming yang mengelompokkan data dan function dalam "objek"

**Analogi Dunia Nyata:**
- **Mobil** → punya warna, model, tahun → bisa jalan, parkir, klakson
- **Hewan** → punya kaki, mata, ekor → bisa jalan, makan, tidur
- **Siswa** → punya nama, umur, nilai → bisa belajar, ujian, lulus

---

### Class vs Object

**Class** = Blueprint/cetakan  
**Object** = Instance/hasil dari cetakan

**Analogi:**
- Class = Desain rumah
- Object = Rumah yang sudah dibangun

---

### Membuat Class Sederhana

```python
class Car:
    def drive(self):
        print("Driving")
    
    def park(self):
        print("Parking")
    
    def honk(self):
        print("Honking")
```

**Menggunakan Class:**
```python
# Bikin object dari class Car
my_car = Car()

# Panggil method
my_car.drive()   # Output: Driving
my_car.park()    # Output: Parking
my_car.honk()    # Output: Honking
```

---

### Class dengan Atribut

**Atribut** = Data/properti yang dimiliki object

```python
class Car:
    def __init__(self, color, model, year):
        self.color = color
        self.model = model
        self.year = year
    
    def info(self):
        print(f"{self.color} {self.model} ({self.year})")
```

**Menggunakan:**
```python
# Bikin object dengan atribut
car1 = Car("Red", "Toyota", 2020)
car2 = Car("Blue", "Honda", 2021)

# Akses atribut
print(car1.color)   # Output: Red
print(car2.model)   # Output: Honda

# Panggil method
car1.info()  # Output: Red Toyota (2020)
car2.info()  # Output: Blue Honda (2021)
```

---

### Apa itu `__init__`?

**`__init__`** = Constructor (dipanggil otomatis saat bikin object)

**Analogi:**
- Seperti "setting awal" saat mobil pertama kali dibuat
- Langsung set warna, model, tahun

```python
class Student:
    def __init__(self, name, age):
        self.name = name  # Set atribut
        self.age = age
        self.grades = []  # Atribut dengan nilai default
```

---

### Apa itu `self`?

**`self`** = Referensi ke object itu sendiri

**Aturan:**
- Harus ada di semua method
- Selalu parameter pertama
- Untuk akses atribut: `self.nama_atribut`

```python
class Student:
    def __init__(self, name):
        self.name = name  # self = object ini
    
    def greet(self):
        print(f"Hi, I'm {self.name}")  # Akses atribut pakai self
```

---

### Contoh Lengkap: Class Student

```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        self.grades = []
    
    def add_grade(self, subject, score):
        self.grades.append({'subject': subject, 'score': score})
    
    def calculate_gpa(self):
        if len(self.grades) == 0:
            return 0
        total = sum([g['score'] for g in self.grades])
        return total / len(self.grades)
    
    def display_grades(self):
        print(f"Grades for {self.name}:")
        for grade in self.grades:
            print(f"  {grade['subject']}: {grade['score']}")
        print(f"GPA: {self.calculate_gpa():.2f}")
```

**Menggunakan:**
```python
# Bikin student
student1 = Student("Alice", 18)
student2 = Student("Bob", 19)

# Tambah nilai
student1.add_grade("Math", 90)
student1.add_grade("English", 85)
student1.add_grade("History", 92)

student2.add_grade("Math", 88)
student2.add_grade("English", 95)

# Tampilkan
student1.display_grades()
# Output:
# Grades for Alice:
#   Math: 90
#   English: 85
#   History: 92
# GPA: 89.00

print(f"{student2.name}'s GPA: {student2.calculate_gpa()}")
# Output: Bob's GPA: 91.5
```

---

### Method vs Function

| Function | Method |
|----------|--------|
| Di luar class | Di dalam class |
| Berdiri sendiri | Bagian dari object |
| `def function():` | `def method(self):` |
| Panggil: `function()` | Panggil: `object.method()` |

---

### Atribut vs Method

| Atribut | Method |
|---------|--------|
| Data/properti | Aksi/fungsi |
| Tanpa `()` | Pakai `()` |
| `car.color` | `car.drive()` |
| Simpan nilai | Lakukan sesuatu |

---

## 💡 Tips & Best Practices

### Naming Convention

**Variable & Function:**
```python
# snake_case
student_name = "Alice"
def calculate_average():
    pass
```

**Class:**
```python
# PascalCase
class StudentRecord:
    pass
```

**Constant:**
```python
# UPPER_CASE
MAX_STUDENTS = 100
PI = 3.14159
```

---

### String Formatting

**4 Cara Format String di Python:**

**1. Concatenation (Paling Lama)**
```python
name = "Alice"
age = 18
text = "My name is " + name + " and I'm " + str(age)
```

**2. `.format()` Method**
```python
text = "My name is {} and I'm {}".format(name, age)
```

**3. `%` Formatting (Python 2)**
```python
text = "My name is %s and I'm %d" % (name, age)
```

**4. f-string (Paling Modern & Disarankan!)**
```python
text = f"My name is {name} and I'm {age}"
```

**Rekomendasi:** Pakai **f-string** untuk code baru!

---

## 🎓 Rangkuman

### Yang Sudah Dipelajari:

✅ **Control Flow & Looping**
- For loop, while loop
- If-elif-else
- Modulo untuk cek genap/ganjil

✅ **List Comprehension**
- Cara singkat bikin list
- Dengan kondisi & transformasi

✅ **Lambda, Filter, Map**
- Lambda = anonymous function
- Filter = saring data
- Map = transform data

✅ **Module & Package**
- Organisasi code
- Import dari file lain
- Install package dengan pip
- Requirements.txt

✅ **Working with Files**
- Baca & tulis file
- CSV untuk data
- DictReader & DictWriter

✅ **Virtual Environment**
- Isolated environment per project
- Activate & deactivate
- Install package di venv

✅ **OOP Basics**
- Class & Object
- `__init__` constructor
- `self` keyword
- Atribut & Method

---

## 🚀 Next Steps

### Untuk Pemula:
1. **Praktek semua contoh di notebook**
2. **Bikin project kecil:**
   - Program kalkulator dengan OOP
   - CSV data analyzer
   - To-do list app
3. **Coba debug dengan print & debugger**

### Untuk Advanced:
1. **Pelajari OOP lanjutan:**
   - Inheritance
   - Encapsulation
   - Polymorphism
2. **Eksperimen dengan package:**
   - Pandas untuk data
   - NumPy untuk matematika
   - Matplotlib untuk visualisasi

---

## ❓ FAQ

**Q: Kapan pakai list comprehension vs loop biasa?**  
A: List comprehension untuk operasi simpel (1-2 baris). Loop biasa untuk logic kompleks.

**Q: Apa bedanya `import module` vs `from module import function`?**  
A: Yang pertama harus pakai prefix (`module.function()`), yang kedua langsung pakai (`function()`).

**Q: Wajib pakai virtual environment?**  
A: Tidak wajib untuk belajar, tapi sangat disarankan untuk project nyata!

**Q: Kenapa pakai `self` di method?**  
A: Supaya method bisa akses atribut object. Ini aturan Python untuk OOP.

**Q: CSV vs Excel, mana lebih baik?**  
A: CSV lebih ringan & universal. Excel lebih fitur (formatting, formula) tapi lebih berat.

**Q: Kapan pakai Anaconda vs pip?**  
A: Anaconda untuk pemula (all-in-one). Pip untuk production (minimal & custom).

---

## 📚 Resources

**Dokumentasi:**
- Python Docs: https://docs.python.org/3/
- CSV Module: https://docs.python.org/3/library/csv.html

**Practice:**
- Google Colab (gratis!)
- Jupyter Notebook
- VS Code + Python Extension

**Debugging:**
- Print statements
- Python debugger (pdb)
- VS Code debugger

---

## 🎉 Penutup

**Remember:**
- Coding itu seperti buku tulis: coret-coret sampai dapat solusi! 📝
- Debugging is normal - bahkan senior developer sering debug
- Practice makes perfect - semakin banyak coding, semakin paham
- Jangan takut error - error adalah guru terbaik!

**Keep Coding!** 💻🚀