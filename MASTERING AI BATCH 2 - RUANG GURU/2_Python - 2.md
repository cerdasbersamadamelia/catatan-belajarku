# Python - 2: Looping, Function, dan Data Structure

---

## 1. Looping (Perulangan)

### Konsep Dasar Looping
Looping adalah cara untuk mengulang-ulang sesuatu yang membosankan. Komputer sangat jago melakukan hal yang berulang-ulang.

### A. For Loop dengan Range

#### Range dengan 1 Parameter
```python
for i in range(5):
    print(i)
# Output: 0 1 2 3 4
```
- Dimulai dari 0
- Sampai sebelum 5 (5 tidak termasuk)
- Hasilnya: 0, 1, 2, 3, 4

#### Range dengan 2 Parameter
```python
for i in range(1, 5):
    print(i)
# Output: 1 2 3 4
```
- Parameter pertama: starting point (mulai dari 1)
- Parameter kedua: endpoint (sampai sebelum 5)

#### Range dengan 3 Parameter
```python
for i in range(1, 10, 2):
    print(i)
# Output: 1 3 5 7 9
```
- Parameter pertama: starting point
- Parameter kedua: endpoint
- Parameter ketiga: step/lompatan (naik 2-2)

### B. For Loop untuk Array/List

#### Loop Langsung dengan Elemen
```python
numbers = [1, 2, 3, 4, 5]

# Cara 1: Loop langsung
for number in numbers:
    print(number)

# Cara 2: Loop dengan index
for i in range(len(numbers)):
    print(numbers[i])
```

Cara pertama lebih simpel karena langsung dapat nilainya tanpa perlu index.

#### Loop untuk String
```python
# String dianggap sebagai array of character
for letter in "Python":
    print(letter)
# Output: P y t h o n
```

**Penting**: String adalah array of character, jadi bisa di-loop seperti array.

### C. While Loop

#### Konsep While Loop
```python
counter = 0
while counter < 5:
    print(counter)
    counter += 1  # Jangan lupa tambahkan counter!
print("Selesai")
# Output: 0 1 2 3 4 Selesai
```

**Cara Baca**: "Selama counter kurang dari 5, maka eksekusi kode di dalamnya"

#### Perbedaan For vs While
- **For Loop**: Lebih simpel, counter otomatis naik, lebih aman
- **While Loop**: Lebih fleksibel, bisa kontrol counter secara dinamis

#### Bahaya Infinite Loop
```python
# JANGAN DILAKUKAN - Infinite Loop!
counter = 0
while counter < 5:
    print(counter)
    # Lupa tambah counter += 1
    # Akan loop selamanya!
```

#### Kapan Pakai While Loop?
While loop digunakan ketika counter-nya dinamis (bisa naik/turun tergantung kondisi):

```python
counter = 0
while counter < 5:
    decision = input("Naik atau turun? ")
    if decision == "naik":
        counter += 1
    elif decision == "turun":
        counter -= 1
    else:
        print("Input salah")
```

#### While True (Loop Forever)
```python
while True:
    # Kode ini akan jalan terus selamanya
    print("Hello")
```

**Kapan Digunakan?**
- Untuk program yang harus jalan terus (game, UI, server)
- Monitoring sensor (IoT/microprocessor)
- Rendering UI yang terus menerus

**Contoh Penggunaan Real**:
```python
# Game loop sederhana
posisi = 0
lari = ['-', '-', '-', '-', '-', '-', '-', '-', '-', '-']

while True:
    arah = input("Kanan atau kiri? ")
    
    if arah == "kanan":
        lari[posisi] = '-'
        posisi += 1
        lari[posisi] = '🐢'
    elif arah == "kiri":
        lari[posisi] = '-'
        posisi -= 1
        lari[posisi] = '🐢'
    
    print(''.join(lari))
```

**Contoh IoT/Sensor**:
```python
while True:
    light_level = get_light_sensor()
    
    if light_level > 0.6:  # Terang
        set_lamp(0)  # Matikan lampu
    else:  # Gelap
        set_lamp(1)  # Nyalakan lampu
    
    sleep(0.5)  # Tunggu 0.5 detik
```

---

## 2. List Comprehension

List comprehension adalah cara simpel membuat list di Python dalam satu baris.

### Format Dasar
```python
# Cara biasa (3 baris)
squares = []
for x in range(1, 11):
    squares.append(x ** 2)

# List comprehension (1 baris)
squares = [x**2 for x in range(1, 11)]
# Output: [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

### List Comprehension dengan Kondisi
```python
# Hanya bilangan genap
numbers = [x for x in range(1, 11) if x % 2 == 0]
# Output: [2, 4, 6, 8, 10]
```

**Penjelasan**:
- `x`: output yang akan disimpan
- `for x in range(1, 11)`: loop dari 1-10
- `if x % 2 == 0`: hanya yang genap (sisa bagi 2 = 0)

### Keunggulan List Comprehension
- Lebih singkat (one liner)
- Lebih mudah dibaca (setelah terbiasa)
- Lebih Pythonic

**Tidak ada di bahasa lain** - ini fitur khusus Python!

---

## 3. Function (Fungsi)

### Konsep Function
Function adalah cara mengelompokkan kode yang berhubungan menjadi satu kesatuan. 

**Analogi**: 
- "Keluar Kamar" = berdiri + maju + buka pintu
- "Berdiri" = luruskan kaki + angkat badan
- "Luruskan Kaki" = tarik otot + ... dst

Ini disebut **abstraksi/enkapsulasi** - kita wrap detail kompleks menjadi satu perintah sederhana.

### Membuat Function
```python
# Function tanpa parameter
def greet():
    print("Hello World")

# Memanggil function
greet()  # Output: Hello World
```

**Penting**: 
- `greet` (tanpa kurung) = nama function
- `greet()` (dengan kurung) = eksekusi function

### Function dengan Parameter
```python
def greet(name):
    print(f"Hello {name}")

greet("Layla")  # Output: Hello Layla
greet("Agus")   # Output: Hello Agus
```

### Function dengan Return
```python
def calculate_square(x):
    return x ** 2

result = calculate_square(5)
print(result)  # Output: 25
```

### Kegunaan Function
1. **Abstraksi** - Tidak perlu tahu detail implementasi
2. **Reusable** - Bisa dipakai berkali-kali
3. **Maintainable** - Mudah diubah dan diperbaiki
4. **Readable** - Kode lebih mudah dibaca

**Contoh Real: Machine Learning**
```python
# Kita tidak perlu tahu detail cara kerja ML
# Tinggal pakai function yang sudah dibuat
from transformers import pipeline

image_to_text = pipeline("image-to-text", model="salesforce/blip")
result = image_to_text("url_gambar")
# Langsung dapat deskripsi gambar!
```

---

## 4. Lambda Function

### Konsep Lambda
Lambda adalah **anonymous function** (fungsi tanpa nama) yang ditulis dalam satu baris.

### Format Lambda
```python
# Function biasa
def add(x, y):
    return x + y

# Lambda function
add = lambda x, y: x + y

# Cara pakai sama
print(add(3, 5))  # Output: 8
```

### Kapan Pakai Lambda?
Lambda digunakan untuk **function sekali pakai** (one-time use).

#### Contoh dengan Map
```python
numbers = [1, 2, 3, 4, 5]

# Cara 1: Bikin function terpisah
def calculate_square(x):
    return x ** 2

squares = list(map(calculate_square, numbers))

# Cara 2: Pakai lambda (lebih ringkas)
squares = list(map(lambda x: x**2, numbers))
# Output: [1, 4, 9, 16, 25]
```

**Kenapa pakai lambda?**
Karena `calculate_square` cuma dipakai sekali. Daripada bikin function terpisah, lebih simpel pakai lambda langsung.

#### Contoh dengan Filter
```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Filter hanya bilangan genap
evens = list(filter(lambda x: x % 2 == 0, numbers))
# Output: [2, 4, 6, 8, 10]
```

### Built-in Functions untuk Array

#### 1. Map
Map mengubah/transform setiap elemen array.

```python
numbers = [1, 2, 3, 4, 5]
squares = list(map(lambda x: x**2, numbers))
# Input:  [1, 2, 3, 4, 5]
# Output: [1, 4, 9, 16, 25]
```

#### 2. Filter
Filter menyaring elemen berdasarkan kondisi.

```python
numbers = [1, 2, 3, 4, 5, 6]
evens = list(filter(lambda x: x % 2 == 0, numbers))
# Input:  [1, 2, 3, 4, 5, 6]
# Output: [2, 4, 6]
```

**Perbedaan Map vs Filter**:
- **Map**: Jumlah output = jumlah input, mengubah nilai
- **Filter**: Jumlah output ≤ jumlah input, menyaring nilai

#### 3. Sort
Mengurutkan array.

```python
numbers = [5, 2, 8, 1, 9]
numbers.sort()
print(numbers)  # Output: [1, 2, 5, 8, 9]
```

#### 4. Zip
Menggabungkan dua array menjadi pasangan.

```python
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]

combined = list(zip(names, ages))
# Output: [('Alice', 25), ('Bob', 30), ('Charlie', 35)]
```

**Analogi**: Seperti resleting yang menggabungkan sisi kiri dan kanan.

---

## 5. Data Structures

### A. List (Array)

List adalah kumpulan elemen yang berurutan.

```python
friends = ["Agus", "Budi", "Candra"]
```

#### Indexing
```python
# Index dimulai dari 0
friends[0]   # "Agus"
friends[1]   # "Budi"
friends[2]   # "Candra"

# Index negatif (dari belakang)
friends[-1]  # "Candra" (elemen terakhir)
friends[-2]  # "Budi"
friends[-3]  # "Agus"
```

#### Operasi List
```python
# Append (tambah di akhir)
friends.append("David")

# Remove (hapus elemen)
friends.remove("Bob")

# Loop
for friend in friends:
    print(friend)
```

### B. Tuple

Tuple mirip list, tapi **immutable** (tidak bisa diubah).

```python
# List (bisa diubah)
numbers_list = [1, 2, 3]
numbers_list[0] = 4  # ✓ Bisa

# Tuple (tidak bisa diubah)
numbers_tuple = (1, 2, 3)
numbers_tuple[0] = 4  # ✗ Error!
```

#### Kapan Pakai Tuple?
1. **Posisi/Koordinat** (x, y tidak boleh tertukar)
   ```python
   position = (20, 30)  # x=20, y=30
   ```

2. **Data yang urutannya penting**
   ```python
   name = ("John", "Doe")  # First name, Last name
   ```

3. **Lebih memory efficient** (sedikit)

### C. Set

Set adalah koleksi yang **hanya berisi elemen unik** (tidak ada duplikat).

```python
# Array biasa (bisa duplikat)
votes = ["Alice", "Bob", "Alice", "Charlie", "Bob"]
print(len(votes))  # 5

# Set (otomatis hapus duplikat)
unique_votes = {"Alice", "Bob", "Alice", "Charlie", "Bob"}
print(unique_votes)  # {'Alice', 'Bob', 'Charlie'}
print(len(unique_votes))  # 3
```

#### Mengubah List ke Set
```python
# Cara 1: Langsung buat set
votes = {"Alice", "Bob", "Charlie"}

# Cara 2: Convert dari list
votes_list = ["Alice", "Bob", "Alice"]
votes_set = set(votes_list)  # Duplikat otomatis hilang
```

#### Operasi Set

**Union** (Gabung semua):
```python
hobbies_a = {"reading", "gaming", "music"}
hobbies_b = {"gaming", "sports", "music"}

all_hobbies = hobbies_a.union(hobbies_b)
# {'reading', 'gaming', 'music', 'sports'}
```

**Intersection** (Yang sama):
```python
common_hobbies = hobbies_a.intersection(hobbies_b)
# {'gaming', 'music'}
```

**Catatan**: Set tidak bisa diakses dengan index (`votes[0]` tidak bisa).

### D. Dictionary

Dictionary adalah struktur data **key-value** (seperti tabel dengan 2 kolom).

```python
kamus = {
    "makan": "eat",
    "minum": "drink",
    "tidur": "sleep"
}
```

#### Mengakses Dictionary
```python
# Akses dengan key
print(kamus["makan"])  # "eat"
print(kamus["minum"])  # "drink"
```

#### Kenapa Pakai Dictionary?
**Tanpa Dictionary** (pakai array):
```python
kamus = [
    ["makan", "eat"],
    ["minum", "drink"],
    ["tidur", "sleep"]
]

# Untuk cari "makan" = apa, harus loop semua!
for kata in kamus:
    if kata[0] == "makan":
        print(kata[1])  # Lambat!
```

**Dengan Dictionary**:
```python
kamus = {"makan": "eat", "minum": "drink"}
print(kamus["makan"])  # Langsung dapat! Cepat!
```

#### Dictionary Bolak-Balik
Dictionary hanya satu arah (key → value). Tidak bisa sebaliknya.

**Solusi untuk dua arah**:
```python
# Indonesia → Inggris
indo_to_eng = {"makan": "eat", "minum": "drink"}

# Inggris → Indonesia (buat dictionary baru)
eng_to_indo = {"eat": "makan", "drink": "minum"}
```

**Trade-off**:
- **Space (Memory)**: Butuh 2x lipat memory (ada 2 dictionary)
- **Time (Speed)**: Lebih cepat (akses langsung, tidak perlu loop)

#### Masalah: Value yang Sama
```python
# Bahasa Indonesia ke Inggris
kamus = {
    "kamu": "you",
    "anda": "you"  # Value sama!
}

# Kalau dibalik, akan ada masalah
# "you" = "kamu" atau "anda"?
```

#### Solusi: Value Berupa List
```python
# Satu key, multiple values
eng_to_indo = {
    "you": ["kamu", "anda"],
    "can": ["bisa", "kaleng"]
}
```

---

## 6. Perbedaan Tipe Kurung

```python
# List - kurung kotak []
my_list = [1, 2, 3]

# Tuple - kurung lengkung ()
my_tuple = (1, 2, 3)

# Set - kurung kurawal {}
my_set = {1, 2, 3}

# Dictionary - kurung kurawal {} dengan key:value
my_dict = {"key": "value"}
```

---

## 7. Tips dan Catatan Penting

### String Quotes
```python
# Single quote dan double quote sama
text1 = 'Hello'
text2 = "Hello"  # Sama

# Quote di dalam quote
text3 = "She said 'Hi'"  # ✓
text4 = 'He said "Hello"'  # ✓

# Escape character
text5 = "She said \"Hi\""  # ✓
```

### Debugging
- Gunakan **breakpoint** (titik merah di VSCode)
- Gunakan **Debug Cell** untuk melihat step-by-step
- Lihat nilai variabel di setiap step

### Map Object
```python
numbers = [1, 2, 3, 4, 5]
squares = map(lambda x: x**2, numbers)

# squares adalah map object, bukan list!
print(squares)  # <map object ...>

# Harus diconvert ke list
print(list(squares))  # [1, 4, 9, 16, 25]

# Hati-hati! Map object hanya bisa dipakai sekali
print(list(squares))  # [] (kosong!)
```

**Solusi**: Convert ke list dulu jika akan dipakai berkali-kali.

### Truthy dan Falsy di Python
```python
# False values
while 0:      # False (tidak jalan)
while False:  # False (tidak jalan)

# True values  
while 1:      # True (jalan terus)
while 5:      # True (jalan terus)
while -1:     # True (jalan terus)
```

**Aturan**: Hanya `0` dianggap False, angka lain dianggap True.

**Catatan**: Ini tidak standar/normal di Python. Lebih baik pakai:
```python
while condition == True:
while nilai > 0:
```

---

## 8. Space-Time Tradeoff

Konsep penting dalam Computer Science:

**Space-Time Tradeoff**: 
- Jika ingin lebih **cepat** → butuh **memory lebih banyak**
- Jika ingin hemat **memory** → jadi lebih **lambat**

**Contoh**:
```python
# Option 1: Cepat tapi boros memory
indo_to_eng = {...}  # 1 juta kata
eng_to_indo = {...}  # 1 juta kata (duplikasi)
# Total: 2 juta kata di memory, tapi akses cepat

# Option 2: Hemat memory tapi lambat
indo_to_eng = {...}  # 1 juta kata
# Untuk balik arah, harus loop 1 juta kali
# Total: 1 juta kata di memory, tapi akses lambat
```

Tidak ada yang benar/salah, tergantung kebutuhan!

---

## 9. Plagiarism Detection

**Tip untuk Belajar**: 
- Jangan copy-paste kode teman
- Pahami logikanya
- Tulis dengan cara sendiri

**Pendeteksi plagiarisme bisa mendeteksi**:
- Kode yang 100% sama
- Kode yang mirip (bahkan jika ditambah kode sampah seperti `while False: pass`)
- Struktur dan logika yang sama

**Cara Aman**:
- Jika tidak bisa menjelaskan kode sendiri → belajar lagi sampai paham
- Gunakan fasilitas bantuan (private session, kelas tambahan)

---

## 10. Latihan dan Resources

### Latihan
- Ada exercise di materi (akan dibahas di sesi berikutnya)
- Advent of Code: https://adventofcode.com/ (soal coding harian, seru!)

### Bantuan Tambahan
1. **Kelas Tambahan**: Senin & Kamis (untuk yang butuh pace lebih lambat)
2. **Private Session**: Booking melalui kalender Discord (30 menit per sesi)
3. **Discord**: Untuk diskusi dan pengumuman

---

## Rangkuman

### Looping
- **For Loop**: Untuk perulangan dengan jumlah pasti
- **While Loop**: Untuk perulangan dengan kondisi dinamis
- **List Comprehension**: Cara ringkas membuat list dalam 1 baris

### Function
- **Function biasa**: Dengan nama, bisa dipakai berkali-kali
- **Lambda**: Anonymous function, untuk sekali pakai
- **Built-in**: map, filter, sort, zip

### Data Structure
- **List**: Bisa diubah, pakai `[]`
- **Tuple**: Tidak bisa diubah, pakai `()`
- **Set**: Hanya unik, pakai `{}`
- **Dictionary**: Key-value, pakai `{"key": "value"}`

### Prinsip Penting
- **Abstraksi**: Wrap detail kompleks jadi sederhana
- **Space-Time Tradeoff**: Cepat vs Hemat memory
- **Debugging**: Gunakan breakpoint untuk memahami alur kode