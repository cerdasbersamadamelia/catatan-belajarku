# Python - 1 (Webinar #1)

- *Instruktur: Pahlevi Fikri (Levi)*
- Asal: Jogja, sekarang tinggal di Depok, Jawa Barat
- Pendidikan: S1 & S2 di Singapura
- Pekerjaan: Ruang Guru (membantu berbagai sesi termasuk bootcamp AI)
- Pengalaman: Membuat program internship di Ruang Guru
  * Batch 1: 40 intern (20 backend, 20 frontend) dari 1800+ pelamar
  * Program intensif: Golang 1 hari, SQL 1 hari, Kubernetes 1 hari
  * Batch 2: 100+ peserta fokus ke AI

---

## 🎯 Tentang Kelas

### Aturan Kelas
- **On time!** Kelas mulai jam 7:30 WIB tepat (no buffer)
- Tanya langsung di chat kalau ada yang bingung
- Nanti ada AI assistant untuk bantu jawab pertanyaan
- Feedback penting banget - kasih tau kalau ada yang kurang jelas
- Kita semua udah dewasa, jadi harus proaktif sendiri ya!

### Untuk yang Belum Bisa Coding
- Ada kelas tambahan khusus untuk pemula
- Hubungi tim ops untuk ikut kelas tambahan
- Jangan malu untuk minta bantuan!

---

## 💻 Apa Itu Programming Language?

### Konsep Dasar
**Programming language = bahasa untuk ngobrol sama komputer**

Analogi sederhana:
- **Human** ← butuh komunikasi → **Komputer**
- Komputer itu BODOH, cuma bisa ikutin instruksi
- Kita perlu bahasa khusus supaya komputer ngerti

### Sintaks vs Semantik

**Sintaks** = cara nulisnya  
**Semantik** = artinya

Contoh:
- "Pergi ke mana?" (Indonesia)
- "Where are we going?" (Inggris)
- "Arep nyang endi?" (Jawa)

**Beda bahasa, beda sintaks, tapi SEMANTIK SAMA!**

Begitu juga di programming:
```python
# Python
print("hello")

# Java
System.out.println("hello");

# C
printf("hello");
```

Semua punya tujuan sama: nge-print "hello"

---

## 🐍 Kenapa Python?

### Kelebihan Python
1. **Bahasa paling populer** (top 3 dunia!)
2. **Gampang dipelajari** - mirip bahasa manusia
3. **Ekosistem kuat** - library banyak banget
4. **Perfect untuk Machine Learning & AI**

### Kelemahan Python
- Agak lambat dibanding bahasa lain (kayak Golang, Rust)
- Tapi kelebihannya jauh lebih banyak!

### High Level vs Low Level

**Low Level** (dekat ke mesin):
- Assembly, C
- Lebih cepat eksekusinya
- Tapi susah dipahami

**High Level** (dekat ke manusia):
- Python, Java, JavaScript
- Lebih gampang dipahami
- Lebih cepat buat nulis code

Python ada di **High Level** → cocok buat kita!

---

## 📓 Tools: Jupyter Notebook

### Apa Itu Jupyter Notebook?
Tempat buat nulis dan jalanin code Python. Kayak notebook digital!

### Pilihan Editor

#### 1. **Google Colab** (RECOMMENDED untuk pemula!)
- Link: https://colab.research.google.com
- Gratis, gak perlu install apa-apa
- Jalan di browser
- Spek kentang juga OK!
- Code jalan di server Google, bukan di komputer kita

**Cara pakai:**
- Buka link
- Klik "New Notebook"
- Langsung bisa coding!

#### 2. **Jupyter Lab** (lokal)
```bash
# Install via terminal
pip install jupyterlab
jupyter lab
```

#### 3. **VS Code** (untuk yang advanced)
- Download di: https://code.visualstudio.com
- Install extension: 
  - Python (Microsoft)
  - Jupyter (Microsoft)
  - GitHub Copilot (opsional, tapi SUPER HELPFUL!)

### Jupyter Notebook Features

**Cell Types:**
1. **Code Cell** - untuk nulis code Python
2. **Markdown Cell** - untuk nulis catatan/dokumentasi

**Cara jalanin code:**
- Klik tombol "Run" 
- Atau tekan `Ctrl + Enter`

**Kelebihan Notebook:**
- Bisa jalanin code per bagian (gak harus semua)
- Cocok buat eksperimen
- Bisa langsung lihat hasil
- Ada visualisasi (grafik, gambar, dll)

---

## 🔧 Terminal

### Apa Itu Terminal?
Cara lain buat ngobrol sama komputer - pakai teks doang!

Di Mac ada command `say`:
```bash
say "Halo semuanya"
# Komputer akan ngomong!
```

Terminal = UI tanpa grafik, semua pakai text

---

## 📝 Python Basics

### 1. Comments (Komentar)

**Single line:**
```python
# Ini komentar, gak akan dieksekusi
print("Hello")  # Ini juga komentar
```

**Multi line:**
```python
"""
Ini komentar
yang panjang
bisa beberapa baris
"""
```

**Gunanya:**
- Jelasin code kita
- Bantuin orang lain (atau kita sendiri nanti) ngerti code

---

### 2. INDENTATION (SUPER PENTING!)

Python sangat peduli sama **indentasi**!

❌ **SALAH:**
```python
if x > 0:
print("Positif")  # Error! Harus pake tab/spasi
```

✅ **BENAR:**
```python
if x > 0:
    print("Positif")  # Pake tab atau 4 spasi
```

**Aturan:**
- Harus konsisten (tab semua atau spasi semua)
- Blok code yang sama harus lurus
- Pakai Tab atau 4 spasi (pilih salah satu!)

---

### 3. Variables (Variabel)

**Variabel = tempat nyimpen data**

Kayak di matematika: `x = 5`, `y = 7`

```python
x = 10          # Integer (angka bulat)
y = 5.5         # Float (angka desimal)
nama = "Budi"   # String (teks)
sudah_mandi = True  # Boolean (True/False)
```

**Multiple Assignment:**
```python
# Cara biasa
a = 1
b = 2
c = 3

# Cara Python (lebih ringkas!)
a, b, c = 1, 2, 3
```

**Tukar nilai variabel:**
```python
a = 4
b = 2

# Cara biasa (pake variabel temporary)
temp = a
a = b
b = temp
# Hasil: a=2, b=4

# Cara Python (lebih simple!)
a, b = b, a
# Hasil: a=2, b=4
```

---

### 4. Data Types (Tipe Data)

#### Number (Angka)
```python
x = 10          # Integer (bulat)
y = -5          # Integer negatif
z = 3.14        # Float (desimal)
```

#### String (Teks)
```python
nama = "Budi"
alamat = 'Jakarta'  # Bisa pake ' atau "
```

#### Boolean
```python
sudah_login = True
belum_bayar = False
# Cuma ada 2 pilihan: True atau False
```

---

### 5. Operators (Operasi)

#### Matematika
```python
5 + 3      # = 8   (tambah)
5 - 3      # = 2   (kurang)
5 * 3      # = 15  (kali)
5 / 2      # = 2.5 (bagi)
5 // 2     # = 2   (bagi bulat - dibulatkan ke bawah!)
5 ** 2     # = 25  (pangkat)
7 % 3      # = 1   (modulo/sisa bagi)
```

**HATI-HATI dengan pembagian!**
```python
15 / 3     # = 5.0  (hasilnya float)
15 // 3    # = 5    (hasilnya integer)
16 // 3    # = 5    (bukan 5.33, tapi dibulatkan ke bawah!)
```

#### Comparison (Perbandingan)
```python
5 == 3     # False (sama dengan?)
5 != 3     # True  (tidak sama dengan?)
5 > 3      # True  (lebih besar?)
5 < 3      # False (lebih kecil?)
5 >= 3     # True  (lebih besar atau sama dengan?)
5 <= 3     # False (lebih kecil atau sama dengan?)
```

**PENTING!**
- `=` → assignment (masukin nilai)
- `==` → comparison (bandingin nilai)

```python
a = 5      # Masukin angka 5 ke variabel a
a == 5     # Ngecek: apakah a nilainya 5? (True)
```

---

### 6. Logical Operators

#### AND (dan)
**Semua harus TRUE**

```python
sudah_mandi = True
sudah_makan = True

# Boleh main game kalau SUDAH mandi DAN SUDAH makan
if sudah_mandi and sudah_makan:
    print("Boleh main game!")
else:
    print("Belum boleh!")
```

#### OR (atau)
**Salah satu TRUE = TRUE**

```python
sudah_mandi = True
sudah_makan = False

# Boleh main game kalau SUDAH mandi ATAU SUDAH makan
if sudah_mandi or sudah_makan:
    print("Boleh main game!")  # Ini yang dijalanin
```

#### NOT (tidak/bukan)
**Kebalikannya**

```python
sudah_mandi = True

if not sudah_mandi:
    print("Belum mandi")  # Gak dijalanin
else:
    print("Sudah mandi")  # Ini yang dijalanin
```

---

### 7. IF Statement (Percabangan)

**Syntax dasar:**
```python
if kondisi:
    # Code kalau kondisi TRUE
else:
    # Code kalau kondisi FALSE
```

**Contoh:**
```python
temperature = 15

if temperature < 0:
    print("Freezing!")
elif temperature < 10:
    print("Cold")
elif temperature < 20:
    print("Cool")      # Ini yang dijalanin (15 < 20)
elif temperature < 30:
    print("Warm")
else:
    print("Hot")

print("Selesai")
```

**Cara kerja:**
1. Cek dari atas ke bawah
2. Kalau ketemu yang TRUE → jalanin code-nya
3. Skip sisanya, langsung ke bawah

**Nested IF (IF di dalam IF):**
```python
x = 5
y = -2

if x > 0:
    if y > 0:
        print("Keduanya positif")
    else:
        print("x positif, y negatif")  # Ini yang dijalanin
else:
    print("x negatif")
```

**IF one-liner (shortcut):**
```python
x = 5
hasil = "Positif" if x > 0 else "Negatif"
print(hasil)  # Output: "Positif"
```

---

### 8. Loops (Pengulangan)

**Komputer jago banget ngulang hal yang sama berkali-kali!**

#### For Loop
```python
numbers = [1, 2, 3, 4, 5]

for num in numbers:
    print(num)

# Output:
# 1
# 2
# 3
# 4
# 5
```

**Penjelasan:**
- Ambil nilai dari list satu-satu
- Setiap nilai disimpan di variabel `num`
- Jalanin code di dalam loop
- Ulangi sampai habis

---

## 🎮 Fun Tools untuk Belajar

### 1. Scratch Junior
- Web-based programming untuk anak-anak
- Belajar konsep coding tanpa nulis code
- Pakai blok-blok visual
- Cocok untuk pemula banget!

### 2. tldraw + GPT
- Gambar UI di kertas
- AI bikin jadi website yang bisa diklik!
- Magic banget!

### 3. Mathpix
- OCR untuk matematika
- Upload foto rumus → jadi text
- Super akurat!

---

## 🐛 Debugging

**Debugger = alat untuk nyari bug (error) di code**

Cara pakai di VS Code:
1. Klik di sebelah kiri nomor baris (muncul titik merah = breakpoint)
2. Klik tombol debug
3. Code akan berhenti di breakpoint
4. Bisa lihat nilai variabel satu-satu
5. Tekan F10/F11 untuk jalan step-by-step

**Berguna banget buat:**
- Ngerti alur code
- Nyari error
- Belajar code orang lain

---

## 🎯 Tips Belajar

### 1. Jangan Takut Pusing!
> "Coding itu intinya PUSING. Kalau belum pusing, berarti belum benar-benar belajar!"

### 2. Ketik Sendiri!
- Jangan copy-paste
- Ketik sendiri setiap code
- Muscle memory penting!

### 3. Pakai GitHub Copilot
- AI assistant buat coding
- Mahasiswa bisa GRATIS via GitHub Student Developer Pack
- Link: https://education.github.com/pack

### 4. Practice, Practice, Practice!
- Kerjain semua exercise
- Coba-coba sendiri
- Jangan langsung liat jawaban

### 5. Tanya Kalau Bingung!
- Tanya di public channel Discord
- DM instruktur kalau personal
- No stupid questions!

---

## 📚 Jadwal Pembelajaran

**Minggu 1-2: Python Basics**
- Python fundamentals
- OOP (Object-Oriented Programming)
- Version Control (Git)

**Minggu 3-4: Data & Math**
- Pandas (olah data)
- Visualisasi data
- Statistik
- Kalkulus (1 pertemuan)

**Minggu 5+: Machine Learning**
- Machine Learning basics
- Deep Learning
- Computer Vision (CNN)
- NLP (Natural Language Processing)
- Deployment (MLOps)

---

## 🔗 Links Penting

- **LMS**: Ada di email
- **Google Colab**: https://colab.research.google.com
- **VS Code**: https://code.visualstudio.com
- **GitHub Education**: https://education.github.com/pack
- **Discord**: (cek di LMS)

---

## 💡 Quote of the Day

> "Komputer itu BODOH. Dia cuma bisa ikutin instruksi. Tugas kita adalah kasih instruksi yang JELAS dan DETAIL!"

---

**Happy Coding! 🚀**