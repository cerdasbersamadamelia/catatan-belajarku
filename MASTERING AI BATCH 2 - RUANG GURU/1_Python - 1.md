===============================================================
CATATAN MATERI WEBINAR PYTHON #1
===============================================================

TENTANG INSTRUKTUR
------------------
- Nama: Pahlevi Fikri (panggilan: Levi)
- Asal: Jogja, sekarang tinggal di Depok, Jawa Barat
- Pendidikan: S1 & S2 di Singapura
- Pekerjaan: Ruang Guru (membantu berbagai sesi termasuk bootcamp AI)
- Pengalaman: Membuat program internship di Ruang Guru
  * Batch 1: 40 intern (20 backend, 20 frontend) dari 1800+ pelamar
  * Program intensif: Golang 1 hari, SQL 1 hari, Kubernetes 1 hari
  * Batch 2: 100+ peserta fokus ke AI

ATURAN KELAS
-------------
1. Kelas dimulai TEPAT WAKTU jam 19:30, tidak ada buffer
2. Pertanyaan langsung ditanyakan di chat (tidak ada sesi tanya jawab terpisah)
3. Akan ada AI assistant untuk membantu menjawab pertanyaan
4. Feedback sangat penting - beri tahu jika materi terlalu cepat/susah dipahami
5. Ada kelas tambahan untuk yang belum bisa coding
6. Peserta diharapkan PROAKTIF - tanya jika kesulitan
7. Tanya di public channel Discord supaya semua bisa dapat manfaatnya
8. Absensi dilakukan di akhir sesi

KONSEP PEMBELAJARAN
--------------------
- Menggunakan konsep UX User Research
- Goal: Bukan hanya menyampaikan materi, tapi memastikan peserta PAHAM
- Jika peserta tidak paham, berarti ada yang salah dengan cara mengajar
- Materi Python hanya 3 hari (asumsi sudah pernah coding sebelumnya)
- Untuk yang belum pernah coding, ikut kelas tambahan

===============================================================
MATERI 1: PENGENALAN BAHASA PEMROGRAMAN
===============================================================

APA ITU BAHASA PEMROGRAMAN?
----------------------------
Bahasa pemrograman adalah interface/jembatan antara MANUSIA dan KOMPUTER supaya bisa berkomunikasi.

KONSEP SINTAKS DAN SEMANTIK
----------------------------
Seperti bahasa manusia, bahasa pemrograman punya:

1. SINTAKS = Cara penulisan/struktur kode
2. SEMANTIK = Arti/makna dari kode

Contoh:
- "Pergi ke mana?" (Indonesia)
- "Where are we going?" (Inggris)
- Sintaksnya BEDA, tapi semantiknya SAMA (menanyakan tujuan)

Dalam pemrograman:
- Python: print("hello")
- Java: System.out.println("hello")
- C: printf("hello")
→ Sintaks beda, semantik sama (mencetak "hello")

SEJARAH SINGKAT
----------------
1. Zaman Dulu: Punch Card
   - Coding dengan membolong kartu kertas
   - Kartu dimasukkan ke mesin besar untuk dieksekusi
   - Istilah "BUG" berasal dari serangga yang nempel di kartu/mesin

2. Perkembangan: Assembly → C → Python, Java, dll
   - Semakin berkembang, semakin mudah dipahami manusia

TINGKATAN BAHASA PEMROGRAMAN
------------------------------
┌─────────────────────────────┐
│     HIGH LEVEL              │ ← Dekat dengan bahasa manusia
│  Python, Java, JavaScript   │   - Lebih mudah dipahami
│          Ruby, Go           │   - Lebih lambat eksekusinya
├─────────────────────────────┤   - Cocok untuk development cepat
│        C, C++               │
├─────────────────────────────┤
│      ASSEMBLY               │
├─────────────────────────────┤
│     LOW LEVEL               │ ← Dekat dengan bahasa mesin
│    Machine Code             │   - Lebih sulit dipahami
└─────────────────────────────┘   - Lebih cepat eksekusinya

KENAPA PYTHON?
---------------
✓ Bahasa paling populer (Top 3-5 di dunia)
✓ Mudah dipahami dan dipelajari
✓ Sangat bagus untuk eksplorasi data
✓ Ekosistem library yang kuat
✓ Cocok untuk Machine Learning & AI
✓ Hampir semua library baru support Python terlebih dahulu

Kekurangan:
✗ Relatif lambat dibanding bahasa compiled (Go, Rust, C++)
✗ Tidak di-compile, jadi error baru ketahuan saat runtime

CONTOH KASUS LIBRARY:
- OpenAI API: Dokumentasi pertama selalu Python & Node.js
- Bahasa populer = lebih banyak library komunitas
- Golang/Rust = kadang harus tunggu komunitas bikin library unofficial

===============================================================
MATERI 2: TOOLS & ENVIRONMENT
===============================================================

EDITOR KODE
-----------
Editor adalah tempat kita menulis kode.

1. VISUAL STUDIO CODE (VS Code) - RECOMMENDED
   - Gratis
   - Editor paling populer
   - Fitur:
     * Syntax Highlighting (warna berbeda untuk keyword)
     * Auto-complete
     * Error detection
     * Debugger
     * Extension/plugin
   
   Extension Penting:
   - Python (Microsoft)
   - Jupyter (Microsoft)
   - GitHub Copilot (berbayar, tapi mahasiswa bisa gratis via GitHub Student Developer Pack)

2. JUPYTER NOTEBOOK
   - Untuk coding Python interaktif
   - Bisa menggabungkan code + dokumentasi + visualisasi
   - Format file: .ipynb
   
   Varian:
   a. Google Colab (RECOMMENDED untuk pemula)
      - Berbasis web, tidak perlu install apapun
      - Gratis
      - Jalan di server Google (RAM 12GB, bisa pakai GPU)
      - Link: https://colab.research.google.com
      
   b. Jupyter Notebook (local)
      - Install di komputer sendiri
      
   c. Jupyter Lab (local)
      - Versi lebih canggih dari Jupyter Notebook
      - Ada file navigation di sidebar
      
   d. VS Code + Jupyter Extension
      - Jupyter notebook tapi di VS Code
      - Paling recommended untuk lokal

CARA INSTALL JUPYTER (LOKAL)
-----------------------------
1. Buka terminal/command prompt
2. Jalankan perintah instalasi (ada di LMS)
3. Jalankan: jupyter lab atau jupyter notebook
4. Akan terbuka di browser

JUPYTER NOTEBOOK - FITUR UTAMA
-------------------------------
1. CELL: Kotak untuk menulis kode atau teks
   
2. Jenis Cell:
   a. CODE Cell
      - Untuk menulis kode Python
      - Bisa dijalankan (Run)
      - Hasil eksekusi muncul di bawah cell
      
   b. MARKDOWN Cell (TEXT)
      - Untuk dokumentasi/catatan
      - Support formatting: **bold**, *italic*, heading
      - Di Google Colab ada toolbar untuk formatting

3. Cara Menjalankan Cell:
   - Klik tombol Run/Play
   - Atau tekan: Ctrl + Enter
   - Cell dieksekusi satu per satu (tidak harus semua)

4. Keuntungan:
   - Bagus untuk eksplorasi & belajar
   - Bisa jalankan sebagian kode saja
   - Bisa ubah-ubah nilai dan test langsung
   - Bisa lihat visualisasi (grafik, plot) langsung

TIPS MENGGUNAKAN GOOGLE COLAB
-------------------------------
1. SELALU COPY FILE ke Google Drive sendiri:
   - File > Save a copy in Drive
   - Supaya bisa edit dan tidak terganggu orang lain

2. Bisa pakai GPU untuk training model (berbayar ~$10/bulan)

3. Semua file tersimpan di Google Drive otomatis

DEBUGGER
---------
Debugger = alat untuk troubleshooting kode step-by-step

Cara pakai di VS Code:
1. Klik di sebelah kiri nomor baris → muncul titik merah (breakpoint)
2. Klik "Debug Cell"
3. Kode akan berhenti di breakpoint
4. Bisa lihat nilai variabel saat itu
5. Bisa jalan step-by-step:
   - F10: Step Over (jalan 1 baris)
   - F11: Step Into (masuk ke function)
   - F5: Continue (lanjut ke breakpoint berikutnya)

Manfaat:
- Bisa lihat alur eksekusi kode
- Bisa lihat nilai variabel berubah
- Membantu memahami logika program
- Troubleshooting error

GITHUB COPILOT
---------------
AI assistant untuk coding (berbayar ~$10/bulan)

Fitur:
1. Auto-complete code (mirip autocorrect tapi untuk kode)
2. Bisa chat dengan AI untuk tanya coding
   - Ctrl + I → Chat
   - Tanya: "kode ini untuk apa?"
   - Minta: "buatkan test untuk kode ini"
   - Minta: "buatkan dokumentasi"
3. Generate code dari comment
4. Sangat membantu produktivitas

Untuk Mahasiswa:
- Bisa GRATIS via GitHub Student Developer Pack
- Daftar di: https://education.github.com/pack

TERMINAL/COMMAND LINE
----------------------
Tempat untuk menjalankan command/perintah teks ke komputer

Contoh penggunaan:
- Install library: pip install numpy
- Jalankan Python: python script.py
- Navigasi folder: cd, ls (Linux/Mac), dir (Windows)
- Buat file: touch nama_file (Linux/Mac), echo > nama_file (Windows)

===============================================================
MATERI 3: PYTHON BASICS
===============================================================

CARA KERJA KOMPUTER
--------------------
Komputer itu BODOH:
- Hanya bisa mengikuti instruksi EKSAK
- Eksekusi dari ATAS ke BAWAH (sequential)
- Tidak bisa "mengira-ngira" seperti manusia

Contoh analogi belanja:
Salah: "Beli telur 3, kalau ada melon beli 5"
→ Komputer akan beli telur 5 (karena ada melon) ✗

Benar: Harus detail step-by-step:
1. Keluar rumah
2. Pakai sepatu
3. Naik motor
4. Ke toko
5. Ambil telur 3
6. Cek apakah ada melon
7. Jika ada melon, ambil melon 5
8. Bayar
9. Pulang

COMMENT (KOMENTAR)
-------------------
Komentar = teks yang TIDAK dieksekusi, hanya untuk dokumentasi

```python
# Ini adalah komentar satu baris

"""
Ini adalah komentar
multi-baris
"""

# Contoh penggunaan:
# Fungsi untuk menghitung luas lingkaran
def hitung_luas(radius):
    return 3.14 * radius * radius
```

Fungsi comment:
- Menjelaskan kode
- Dokumentasi
- Menonaktifkan kode sementara
- Di era AI: memberi instruksi ke Copilot untuk generate code

INDENTATION (INDENTASI)
------------------------
SANGAT PENTING DI PYTHON!

Indentasi = spasi/tab di awal baris untuk menunjukkan blok kode

Aturan:
✓ Harus konsisten (semua pakai Tab atau semua pakai 4 spasi)
✓ Menunjukkan kode yang "di dalam" blok (if, loop, function)
✓ Salah indentasi = ERROR

```python
# BENAR:
if x > 0:
    print("positif")  # 1 tab/4 spasi
    print("lebih dari nol")  # 1 tab/4 spasi

# SALAH:
if x > 0:
    print("positif")  # 1 tab
  print("lebih dari nol")  # 2 spasi → ERROR!
```

VARIABEL
---------
Variabel = tempat menyimpan nilai/data

Seperti di matematika: x = 5, y = 7

```python
x = 10          # x menyimpan angka 10
nama = "Budi"   # nama menyimpan teks "Budi"
umur = 25       # umur menyimpan angka 25
```

Aturan penamaan variabel:
- Huruf, angka, underscore (_)
- Tidak boleh diawali angka
- Case sensitive: nama ≠ Nama ≠ NAMA
- Tidak boleh pakai keyword Python (if, for, while, dll)

TIPE DATA
----------
1. INTEGER (int) = Bilangan bulat
   ```python
   x = 10
   y = -5
   z = 0
   ```

2. FLOAT = Bilangan desimal
   ```python
   pi = 3.14
   tinggi = 175.5
   suhu = -2.5
   ```

3. STRING (str) = Teks/kata/kalimat
   ```python
   nama = "Budi"
   kota = 'Jakarta'  # bisa pakai ' atau "
   alamat = """Jl. Sudirman
   Jakarta Pusat"""  # multi-baris pakai """
   ```

4. BOOLEAN (bool) = True atau False
   ```python
   sudah_makan = True
   sudah_mandi = False
   ```

MULTIPLE ASSIGNMENT
--------------------
Python bisa assign banyak variabel sekaligus:

```python
# Cara biasa:
a = 1
b = 2
c = 3

# Multiple assignment:
a, b, c = 1, 2, 3  # Lebih ringkas!

# Menukar nilai (SWAP):
a, b = 4, 2
a, b = b, a  # Tukar nilai a dan b
print(a, b)  # Output: 2 4
```

Tanpa multiple assignment, perlu variabel temporary:
```python
# Cara manual (panjang):
a = 4
b = 2
temp = a  # Simpan nilai a
a = b     # a jadi 2
b = temp  # b jadi 4
```

OPERASI MATEMATIKA
-------------------
```python
# Operasi dasar:
5 + 3   # Penjumlahan = 8
5 - 3   # Pengurangan = 2
5 * 3   # Perkalian = 15
5 / 3   # Pembagian = 1.666...
5 // 3  # Pembagian bulat (dibulatkan ke bawah) = 1
5 % 3   # Modulo (sisa bagi) = 2
5 ** 3  # Pangkat = 125

# Contoh penggunaan modulo:
7 % 3   # = 1 (7 dibagi 3 = 2 sisa 1)
6 % 3   # = 0 (6 dibagi 3 habis, sisa 0)
```

Penting:
- `5 / 3` → 1.666... (float)
- `5 // 3` → 1 (integer, dibulatkan ke bawah)
- `16 // 3` → 5 (bukan 5.333...)

OPERATOR PERBANDINGAN
----------------------
Membandingkan dua nilai, hasilnya True atau False

```python
==   # Sama dengan
!=   # Tidak sama dengan
>    # Lebih besar
<    # Lebih kecil
>=   # Lebih besar atau sama dengan
<=   # Lebih kecil atau sama dengan

# Contoh:
5 == 5   # True
5 == 3   # False
5 > 3    # True
5 < 3    # False
```

PENTING: Bedakan `=` dan `==`
- `=` → Assignment (memasukkan nilai)
- `==` → Comparison (membandingkan)

```python
a = 5      # Memasukkan 5 ke variabel a
a == 5     # Cek apakah a sama dengan 5 (True)
a == 3     # Cek apakah a sama dengan 3 (False)
```

OPERATOR LOGIKA
----------------
Menggabungkan kondisi boolean

1. AND (dan)
   - Semua kondisi harus True
   - Jika salah satu False → hasilnya False
   
   ```python
   sudah_mandi = True
   sudah_makan = True
   
   # Boleh main game jika SUDAH mandi DAN SUDAH makan
   boleh_main = sudah_mandi and sudah_makan  # True
   
   # Jika salah satu False:
   sudah_mandi = False
   boleh_main = sudah_mandi and sudah_makan  # False
   ```

2. OR (atau)
   - Salah satu True sudah cukup
   - Semua False baru hasilnya False
   
   ```python
   punya_uang = True
   punya_kartu = False
   
   # Bisa beli jika punya uang ATAU punya kartu
   bisa_beli = punya_uang or punya_kartu  # True
   ```

3. NOT (tidak/negasi)
   - Membalik nilai boolean
   
   ```python
   sudah_mandi = True
   belum_mandi = not sudah_mandi  # False
   
   sudah_makan = False
   belum_makan = not sudah_makan  # True
   ```

Truth Table:
```
AND:
True  and True  = True
True  and False = False
False and True  = False
False and False = False

OR:
True  or True  = True
True  or False = True
False or True  = True
False or False = False

NOT:
not True  = False
not False = True
```

===============================================================
MATERI 4: CONTROL STRUCTURE
===============================================================

IF STATEMENT (PERCABANGAN)
---------------------------
Membuat program bisa "berpikir" dan mengambil keputusan.

Tanpa IF: Program hanya jalan lurus atas ke bawah
Dengan IF: Program bisa cabang/pilih jalur berbeda

SYNTAX DASAR:
```python
if kondisi:
    # kode yang dijalankan jika kondisi True
    print("Kondisi terpenuhi")
```

Contoh:
```python
kaos_kaki_ketemu = False

if not kaos_kaki_ketemu:
    print("Pakai sandal")
```

IF-ELSE
--------
```python
if kondisi:
    # Jika kondisi True
    print("Jalan ke sini")
else:
    # Jika kondisi False
    print("Jalan ke sini")
```

Contoh:
```python
kaos_kaki_ketemu = True

if kaos_kaki_ketemu:
    print("Pakai sepatu")
else:
    print("Pakai sandal")
```

IF-ELIF-ELSE (Multiple Conditions)
-----------------------------------
```python
if kondisi1:
    # Jika kondisi1 True
elif kondisi2:
    # Jika kondisi1 False, cek kondisi2
elif kondisi3:
    # Jika kondisi2 False, cek kondisi3
else:
    # Jika semua kondisi False
```

Contoh:
```python
temperatur = 15

if temperatur < 0:
    print("Freezing")
elif temperatur < 10:
    print("Cold")
elif temperatur < 20:
    print("Cool")
elif temperatur < 30:
    print("Warm")
else:
    print("Hot")

# Output: Cool (karena 15 < 20)
```

CARA KERJA IF-ELIF-ELSE:
1. Cek kondisi dari ATAS ke BAWAH
2. Jika ada yang True, jalankan blok itu
3. LANGSUNG KELUAR (tidak cek kondisi di bawahnya)
4. Jika semua False, jalankan blok else

NESTED IF (IF Bersarang)
-------------------------
IF di dalam IF

```python
x = 5
y = -2

if x > 0:
    print("x positif")
    
    if y > 0:
        print("y positif")
    else:
        print("y negatif atau nol")
else:
    print("x negatif atau nol")

# Output:
# x positif
# y negatif atau nol
```

Cara membaca:
1. Cek x > 0? Ya → masuk blok pertama
2. Di dalam blok pertama, cek y > 0? Tidak → masuk else
3. Print "y negatif atau nol"

TERNARY OPERATOR (IF Satu Baris)
---------------------------------
Untuk IF-ELSE sederhana, bisa ditulis 1 baris:

```python
# Cara biasa:
if x > 0:
    hasil = "positif"
else:
    hasil = "negatif"

# Ternary operator:
hasil = "positif" if x > 0 else "negatif"
```

Format: `nilai_jika_true if kondisi else nilai_jika_false`

===============================================================
MATERI 5: LOOPING (PERULANGAN)
===============================================================

KONSEP LOOPING
---------------
Komputer paling jago mengulang hal yang sama berkali-kali (repetitif).

Tanpa loop:
```python
print(1)
print(2)
print(3)
print(4)
print(5)
# Panjang dan tidak efisien!
```

Dengan loop:
```python
for i in range(1, 6):
    print(i)
# Lebih ringkas dan fleksibel!
```

FOR LOOP
---------
Mengulang sejumlah tertentu atau mengiterasi koleksi data

```python
# Loop dengan range:
for i in range(5):
    print(i)
# Output: 0, 1, 2, 3, 4

# Loop dengan list:
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    print(num)
# Output: 1, 2, 3, 4, 5

# Loop dengan string:
for huruf in "PYTHON":
    print(huruf)
# Output: P, Y, T, H, O, N
```

Cara kerja:
1. Ambil elemen pertama
2. Jalankan kode di dalam loop
3. Ambil elemen berikutnya
4. Ulangi sampai semua elemen selesai

===============================================================
LATIHAN & TIPS
===============================================================

EXERCISE
---------
Ada exercise di file Python 00 - Python Basic:
- Hitung area lingkaran dengan radius 5
- Formula: area = π × r²
- π = 3.14 atau 22/7

Tips mengerjakan:
1. Jangan langsung copy-paste jawaban
2. KETIK SENDIRI - penting untuk muscle memory
3. Jalankan kode untuk cek hasilnya
4. Bisa print hasil untuk validasi
5. Coba ubah-ubah nilai untuk eksperimen

CARA BELAJAR CODING
--------------------
1. PUSING itu NORMAL dan PENTING
   - Kalau tidak pusing = belum cukup mencoba
   - Pusing → coba sendiri → tanya → paham → progress!

2. Jangan takut ERROR
   - Error adalah bagian dari belajar
   - Baca pesan error untuk tahu masalahnya

3. PRAKTEK > Teori
   - Coding adalah skill praktis
   - Banyak praktek > banyak baca

4. Untuk PEMULA yang belum pernah coding:
   - Coba Scratch atau Scratch Junior dulu
   - Belajar konsep: loop, if-else, function dengan visual
   - Setelah paham konsep, Python jadi lebih mudah

5. Gunakan Debugger
   - Lihat alur eksekusi step-by-step
   - Lihat nilai variabel berubah
   - Sangat membantu memahami logika

===============================================================
TOOLS MENARIK LAINNYA
===============================================================

1. TLDRAW (https://tldraw.com)
   - Gambar UI/wireframe
   - Klik "Make Real" → jadi kode HTML/CSS/JS
   - Tanpa coding, UI langsung jadi!

2. MATTIX (OCR Tool)
   - Upload foto/gambar teks
   - Otomatis convert jadi teks
   - Bisa baca tabel, tulisan miring, dll
   - Ada API untuk integrasi ke program

3. SCRATCH & SCRATCH JUNIOR
   - Belajar coding dengan visual blocks
   - Cocok untuk pemula & anak-anak
   - Konsep sama dengan text-based programming
   - Gratis di https://scratch.mit.edu

===============================================================
ROADMAP PEMBELAJARAN (MASTERING AI BATCH 5)
===============================================================

Minggu 1-2: Python & Foundation
- Python basics (3 hari)
- Version control (Git)
- Statistics
- Pandas (data manipulation)
- Data visualization

Minggu 3-4: Machine Learning
- Calculus for ML
- Linear algebra
- Supervised learning (KNN, Decision Tree, SVM)
- Unsupervised learning (Clustering, PCA)
- Anomaly detection

Minggu 5-6: Deep Learning
- Neural networks
- PyTorch basics
- PyTorch applications
- MNIST (image classification)

Minggu 7-8: Aplikasi Lanjutan
- Database & SQL
- Computer Vision (CNN)
- NLP (Natural Language Processing)
- Transformer architecture (seperti ChatGPT)

Minggu 9-10: Deployment
- LangChain
- MLOps
- Deploy model ke production

===============================================================
TIPS PENTING
===============================================================

1. SELALU COPY FILE ke Drive sendiri (jika pakai Google Colab)
2. Ketik kode SENDIRI, jangan copy-paste
3. Eksperimen dengan mengubah nilai/kode
4. Gunakan debugger untuk memahami alur
5. Tanya di public channel supaya semua bisa belajar
6. Beri feedback jika materi terlalu cepat/sulit
7. Proaktif - jangan malu bertanya
8. Latihan > Membaca materi
9. Error adalah teman, bukan musuh
10. Konsisten praktek setiap hari

===============================================================
RESOURCES
===============================================================

- LMS: Semua materi ada di Learning Management System
- Discord: Untuk diskusi dan tanya jawab
- Google Colab: https://colab.research.google.com
- GitHub Student Pack: https://education.github.com/pack
- Scratch: https://scratch.mit.edu
- VS Code Download: https://code.visualstudio.com

===============================================================
CATATAN AKHIR
===============================================================

Materi hari pertama fokus pada:
✓ Pengenalan bahasa pemrograman
✓ Tools (VS Code, Jupyter, Google Colab)
✓ Python basics (variabel, tipe data, operator)
✓ Control structure (if-else)
✓ Pengenalan looping

Materi selanjutnya akan lebih mendalam tentang:
- Loop (while, for) lengkap
- Function
- List, Dictionary, Tuple
- File handling
- Dan lain-lain

INGAT: Coding itu PRAKTEK. Jangan cuma baca, HARUS DICOBA!

===============================================================
END OF NOTES
===============================================================