# Supervised Learning - 2 (Webinar #11)

---

## 📚 Overview Materi
Webinar ini membahas 3 topik utama:
1. **Matriks & Vektor**
2. **Linear Algebra**
3. **KNN (K-Nearest Neighbors)**

---

## 🔢 1. MATRIKS & VEKTOR

### Jenis-Jenis Bilangan

#### 1.1 Skalar
Skalar = angka biasa
```
x = 6
```

#### 1.2 Vektor
Vektor = array satu dimensi
```
v = [1, 2, 3]
```

**Operasi Vektor:**

**✅ Perkalian dengan Skalar (BISA)**
```
2 × [1, 2, 3] = [2, 4, 6]
```
Semua elemen dikali 2!

**❌ Penjumlahan Skalar + Vektor (TIDAK BISA)**
```
2 + [1, 2, 3] = ❌ ERROR!
```

**✅ Penjumlahan Vektor + Vektor (BISA)**
Syarat: dimensi harus sama!
```
[1, 2, 3] + [4, 5, 6] = [5, 7, 9]
```
Dijumlahkan satu per satu!

**✅ Dot Product Vektor**
```
[1, 2, 3] • [4, 5, 6] = ?
```
Cara hitung:
```
(1×4) + (2×5) + (3×6)
= 4 + 10 + 18
= 32
```
**PENTING:** Hasilnya SKALAR (bukan vektor)!

#### 1.3 Matriks
Matriks = array dua dimensi

Contoh matriks 2×3 (2 baris, 3 kolom):
```
[1  2  3]
[4  5  6]
```

**Cara baca ukuran:** **Baris × Kolom** (row × column)

**Hubungan dengan Vektor:**
Vektor = matriks berukuran n×1
```
[1]
[2]  → Matriks 3×1
[3]
```

**Operasi Matriks:**

**✅ Penjumlahan Matriks**
```
[1  2]   [5  6]   [6   8]
[3  4] + [7  8] = [10  12]
```
Tinggal tambahkan satu per satu!

**✅ Dot Product Matriks**
Ini yang agak tricky! Pakai cara **"dua jari"**:
- Jari kiri: gerak ke kanan
- Jari kanan: gerak ke bawah

Contoh:
```
[1  2]   [5]
[3  4] • [6]
```

Cara hitung:
```
Baris 1: (1×5) + (2×6) = 5 + 12 = 17
Baris 2: (3×5) + (4×6) = 15 + 24 = 39

Hasil: [17]
       [39]
```

**SYARAT Dot Product:**
Kalau A berukuran **m×n** dan B berukuran **n×p**:
- Yang di tengah (n) **HARUS SAMA**
- Hasil akhir: **m×p**

Contoh:
- (2×3) • (3×2) = ✅ BISA → hasil 2×2
- (2×3) • (4×2) = ❌ TIDAK BISA (3 ≠ 4)

**PENTING:** Urutan penting!
```
A • B ≠ B • A
```

Tapi asosiatif (urutan operasi bebas):
```
(A • B) • C = A • (B • C)
```

### Operasi Khusus Matriks

#### Transpose
Tukar baris jadi kolom, kolom jadi baris
```
[1  2  3]ᵀ   [1  4]
[4  5  6]  = [2  5]
             [3  6]
```

Yang semula nulis ke kanan → jadi nulis ke bawah!

#### Identity Matrix (I)
Untuk **penjumlahan**: I = matriks nol
```
[0  0]
[0  0]
```

Untuk **dot product**: I = diagonal 1
```
[1  0  0]
[0  1  0]  → ukuran 3×3
[0  0  1]
```

Sifat:
```
A • I = A
```

#### Inverse Matrix (A⁻¹)
Definisi:
```
A • A⁻¹ = I
```

**Kegunaan:** Menyelesaikan persamaan linear!

---

## 📊 Aplikasi Matriks: Menyelesaikan Persamaan Linear

**Contoh Soal:**
```
2 apel + 1 jeruk = $5
3 apel + 4 jeruk = $10

Berapa harga 1 apel dan 1 jeruk?
```

**Solusi Tradisional:**
1. Substitusi
2. Eliminasi

**Solusi dengan Matriks:**

Tulis dalam bentuk matriks:
```
[2  1] [x]   [5]
[3  4] [y] = [10]
```

Atau: **A • X = B**

Untuk dapat X:
```
X = A⁻¹ • B
```

**Langkah-langkah:**
1. Hitung A⁻¹ (pakai NumPy)
```python
import numpy as np
A = np.array([[2, 1], [3, 4]])
A_inv = np.linalg.inv(A)
```

2. Kalikan A⁻¹ dengan B
```python
B = np.array([5, 10])
X = A_inv.dot(B)
```

3. Done! X sekarang berisi nilai x dan y

**Kelebihan cara ini:**
- Bisa untuk sistem persamaan yang BANYAK (x, y, z, a, b, c, ...)
- Cepat kalau pakai komputer!

---

## 🎯 2. LINEAR ALGEBRA

### Visualisasi Vektor

Vektor punya **dua properti:**
1. **Arah** (direction)
2. **Panjang** (magnitude)

Contoh vektor [2, 1]:
```
     y
     ↑
     1 • (2,1)
     |/
     |___→
   O   2   x
```
- Mulai dari origin (0,0)
- Gerak 2 ke kanan, 1 ke atas
- Vektor = panah dari O ke titik (2,1)

**PENTING:** Vektor yang sama bisa digambar di lokasi berbeda!
```
Vektor [2,1] bisa di mana saja
Selama arah & panjang sama = vektor yang sama
```

### Transformasi dengan Matriks

Kita bisa **merotasi**, **memperbesar**, atau **mengubah** vektor pakai matriks!

#### Contoh: Rotasi 90° (berlawanan jarum jam)

**Cara Tradisional (SMA):**
Hafal rumus rotasi... 😵

**Cara Praktis:**

Gunakan 2 vektor test:
1. [1, 0] → kalau dirotasi 90° jadi [0, 1]
2. [0, 1] → kalau dirotasi 90° jadi [-1, 0]

Cari matriks transformasi [a b; c d]:

Dari [1, 0] → [0, 1]:
```
[a  b] [1]   [0]
[c  d] [0] = [1]

→ a = 0, c = 1
```

Dari [0, 1] → [-1, 0]:
```
[a  b] [0]   [-1]
[c  d] [1] = [0]

→ b = -1, d = 0
```

**Matriks rotasi 90°:**
```
[0  -1]
[1   0]
```

Sekarang bisa rotasi vektor apapun!
```
[0  -1] [2]   [-1]
[1   0] [1] = [2]
```
Vektor [2, 1] jadi [-1, 2] ✅

**Aplikasi Real:** CNN (Computer Vision)
- Filter untuk deteksi tepi
- Bottom edge detector
- Top edge detector
- Vertical/horizontal line detector
- Smoothing
- Semua pakai operasi matriks!

### Mengukur Panjang Vektor

#### 1. Euclidean Distance (Jarak Langsung)
Pakai **Pythagoras**!

Vektor [1, 1]:
```
Panjang = √(1² + 1²) = √2 ≈ 1.41
```

Vektor [3, 4]:
```
Panjang = √(3² + 4²) = √(9 + 16) = √25 = 5
```

#### 2. Manhattan Distance (Jarak Kota)
Jalan horizontal + vertikal (kayak mobil di kota)

Vektor [1, 1]:
```
Panjang = |1| + |1| = 2
```

**Kenapa disebut Manhattan?**
Karena jalanan di Manhattan, New York itu kotak-kotak kayak papan catur! Nggak bisa jalan diagonal, harus horizontal-vertikal.

```
Grid Manhattan:
□─□─□─□
│ │ │ │
□─□─□─□
│ │ │ │
□─□─□─□
```

---

## 🎨 Aplikasi Real: Cosine Similarity (Preview NLP)

Kata bisa diubah jadi vektor!
```
"queen" → [0.2, 0.8, 0.3, ...]
"king"  → [0.3, 0.7, 0.4, ...]
"dog"   → [0.9, 0.1, 0.2, ...]
```

**Cosine Similarity** = mengukur sudut antar vektor

**Sudut kecil** = kata mirip
- queen vs king → sudut kecil ✅
- queen vs dog → sudut besar ❌

**Aplikasi:**
- **Semantic Search** di Tokopedia/Shopee
  - Cari "handphone" → muncul juga "HP", "smartphone", "gawai"
  - Tanpa perlu bikin dictionary manual!
  
- **Multilingual Search**
  - Cari pakai Bahasa Indonesia
  - Hasil bisa dari database Bahasa Inggris!

**Teknologi:** Vector Database (Pinecone, etc.)
- Menyimpan embedding (vektor) dari kata/kalimat
- Search berdasarkan semantic, bukan cuma keyword

---

## 🤖 3. K-NEAREST NEIGHBORS (KNN)

### Konsep Dasar

KNN = algoritma yang **super simple** tapi **powerful**!

**Prinsip:** "Teman sejati terlihat dari tetangganya" 🏘️

### Cara Kerja

**Contoh kasus:** Prediksi kanker vs non-kanker

**Data training:**
```
     y (umur)
     ↑
     • (merah = kanker)
     •○
   ○ • ○ (biru = non-kanker)
     ○•
     •○•
     └────→ x (ukuran tumor)
```

**Ada data baru (?):**
```
     y
     ↑
     •
     •○
   ○ • ? ← Data baru, label apa?
     ○•
     •○•
     └────→ x
```

**Cara prediksi:**
1. Tentukan K (misalnya K=3)
2. Cari 3 tetangga terdekat dari (?)
3. Lihat warna mayoritas tetangga
4. (?) dapat label mayoritas!

```
3 tetangga terdekat: • ○ •
Merah: 2, Biru: 1
→ Mayoritas MERAH
→ Prediksi: KANKER ✅
```

### Langkah-Langkah KNN

1. **Split data** → training set & test set
2. **Pilih K** (jumlah tetangga)
3. **Pilih distance metric** (Euclidean/Manhattan)
4. **Untuk setiap data baru:**
   - Hitung jarak ke semua data training
   - Ambil K tetangga terdekat
   - Vote mayoritas → itu prediksi!

### Kode Python (Scikit-learn)

```python
from sklearn.neighbors import KNeighborsClassifier

# Buat model
knn = KNeighborsClassifier(n_neighbors=11, metric='euclidean')

# Training
knn.fit(X_train, y_train)

# Prediksi
y_pred = knn.predict(X_test)

# Evaluasi
from sklearn.metrics import confusion_matrix, f1_score
print(confusion_matrix(y_test, y_pred))
print(f1_score(y_test, y_pred))
```

### Mencari K Optimal

**Cara 1: Heuristic**
```
K = √n
(n = jumlah data training)
```
Contoh: 100 data → coba K=10 dulu

**Cara 2: Brute Force**
Coba K dari 1 sampai 20, pilih yang akurasi terbaik!

```python
# Coba berbagai K
for k in range(1, 20):
    knn = KNeighborsClassifier(n_neighbors=k)
    knn.fit(X_train, y_train)
    score = knn.score(X_test, y_test)
    print(f'K={k}, Accuracy={score}')
```

Plot hasilnya, pilih K dengan akurasi tertinggi + standard deviation rendah!

### Feature Selection (Penting!)

**Memilih feature yang tepat = kunci sukses KNN!**

**Contoh Kanker:**
- ✅ Ukuran tumor
- ✅ Umur pasien
- ❌ Nama pasien (gak relevan!)
- ❌ Warna baju (gak relevan!)

**Contoh Pilpres (analogi):**
Prediksi pilihan presiden seseorang:
- ✅ Umur
- ✅ Pendidikan terakhir
- ✅ Domisili (tetangga geografis biasanya pilihan sama!)
- ❌ Tinggi badan
- ❌ Warna favorit

**Cara memilih feature:**
1. **Tanya expert** (dokter untuk kanker, politisi untuk pilpres)
2. **Feature engineering** (dari materi sebelumnya)
3. **Trial & error** (coba-coba, lihat mana yang performanya bagus)

### Kelebihan KNN

✅ **Super simple** - algoritma paling gampang dipahami!  
✅ **No training needed** - langsung pakai, gak perlu training lama  
✅ **Fleksibel** - bisa untuk klasifikasi & regresi  
✅ **Bisa bikin decision boundary kompleks** (lingkaran, zigzag, dll)

### Kelemahan KNN

❌ **Curse of Dimensionality**
- Kalau feature terlalu banyak (10, 100, 1000 dimensi)
- Data jadi "sparse" (jarang, jauh-jauh)
- Konsep "tetangga dekat" jadi gak bermakna

❌ **Sensitif terhadap feature selection**
- Kalau pilih feature salah → hasil salah!

❌ **Lambat untuk data besar**
- Harus hitung jarak ke SEMUA data training
- Kalau 10,000 data → 10,000 kali perhitungan jarak!

---

## 📌 Notasi Matematika (Bonus)

### Sigma (Σ) - Penjumlahan
```
  5
  Σ  (2i + 1)
 i=2

= (2×2+1) + (2×3+1) + (2×4+1) + (2×5+1)
= 5 + 7 + 9 + 11
= 32
```

### Pi (Π) - Perkalian
```
  4
  Π  i
 i=2

= 2 × 3 × 4
= 24
```

---

## 🎯 Rangkuman

**Matriks & Vektor:**
- Matriks = representasi data
- Operasi matriks = fundamental untuk ML
- GPU = super cepat untuk operasi matriks!

**Linear Algebra:**
- Vektor = punya arah & panjang
- Transformasi matriks = rotasi, scaling, dll
- Aplikasi: CNN, computer vision

**KNN:**
- Algoritma super simple tapi efektif
- Prinsip: tetangga terdekat menentukan label
- Pilih K & feature dengan bijak!
- Evaluasi: confusion matrix, F1-score

---

## 💡 Tips Belajar

1. **Matrix operations akan terus muncul** di materi selanjutnya (Deep Learning, NLP)
2. **Praktek coding** lebih penting daripada hafal rumus
3. **Pakai Copilot/ChatGPT** untuk belajar syntax Python/Pandas
4. **Version control is a must** - commit & push ke GitHub!
5. **Jangan lupa deadline project!** 23:59 malam ini!

---

**Next Week:** Classification & Decision Trees 🌲

*"Bahasa Inggris adalah bahasa pemrograman masa depan"* - dengan GPTs! 🚀
