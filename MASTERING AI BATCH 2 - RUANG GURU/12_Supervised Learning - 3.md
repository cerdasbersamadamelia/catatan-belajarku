# Supervised Learning - 3

---

## 📌 Ringkasan Materi
Hari ini kita belajar 2 topik penting:
1. **Decision Tree** - Algoritma berbentuk pohon untuk klasifikasi
2. **Logistic Regression** - Algoritma klasifikasi dengan fungsi sigmoid

---

## 🌳 DECISION TREE

### Apa itu Decision Tree?
Decision Tree adalah algoritma yang bentuknya seperti pohon (tapi kebalik!). Di komputer sains, akarnya ada di atas, cabang-cabangnya ke bawah.

**Struktur:**
- **Root** = Akar (di atas)
- **Branch** = Cabang
- **Leaf** = Daun (hasil akhir prediksi)

### Konsep Machine Learning vs AI Biasa

**AI Tradisional (IF-ELSE):**
- Kita yang bikin semua aturan
- Misal: IF gejala A THEN penyakit X
- Komputer cuma ikutin aturan kita
- Komputer gak lebih pintar dari kita

**Machine Learning:**
- Kita kasih data (gejala + penyakit)
- Komputer **belajar sendiri** pola-polanya
- Buat aturannya sendiri tanpa kita ajarin detail

### Cara Kerja Decision Tree

**Training Phase:**
Algoritma bikin pohon keputusan berdasarkan data

**Inference Phase:**
Pakai pohon yang udah jadi untuk prediksi data baru

**Contoh Kasus Titanic:**
```
Root: Gender?
├─ Male → Ticket < $X? 
│  ├─ Yes → Not Survive
│  └─ No → Survive
└─ Female → Age < 6.5?
   ├─ Yes → Survive
   └─ No → Survive
```

### Gini Impurity - Kunci Decision Tree! 🔑

**Apa itu Gini Impurity?**
Ukuran seberapa "pure" (murni) suatu kelompok data.

**Pure = Gini 0**
- Semua datanya satu kelas
- Contoh: Semua Survive atau semua Not Survive

**Impure = Gini > 0**
- Datanya campur-campur
- Contoh: Ada yang Survive, ada yang tidak

**Rumus Gini:**
```
Gini = 1 - Σ(pi²)
```
- pi = probabilitas kelas ke-i

**Contoh Perhitungan:**
```
Data: 5 hijau, 2 merah (total 7)
p_hijau = 5/7 = 0.714
p_merah = 2/7 = 0.286

Gini = 1 - (0.714² + 0.286²)
     = 1 - (0.51 + 0.08)
     = 0.41
```

Kalau semua hijau → Gini = 0 (pure!)

### Cara Membangun Decision Tree

**Goal:** Buat pohon yang Gini-nya sekecil mungkin (ideally 0)

**Proses:**
1. Hitung Gini awal untuk semua data
2. Coba split berdasarkan berbagai fitur:
   - Gender (Male/Female)
   - Age (< 10 tahun, ≥ 10 tahun)
   - Ticket price (< $50, ≥ $50)
3. Hitung **Weighted Gini** setelah split:
   ```
   Weighted Gini = (n_kiri/n_total × Gini_kiri) + (n_kanan/n_total × Gini_kanan)
   ```
4. Pilih split yang **Weighted Gini-nya paling kecil**
5. Ulangi untuk setiap cabang sampai Gini = 0

**Contoh:**
```
Original Gini = 0.482

Split by Gender:
- Male: 50 orang, Gini = 0.3
- Female: 50 orang, Gini = 0.4
Weighted Gini = (50/100 × 0.3) + (50/100 × 0.4) = 0.35
✅ Lebih baik dari 0.482!

Split by Age < 10:
- Yes: 25 orang, Gini = 0
- No: 75 orang, Gini = 0.28
Weighted Gini = (25/100 × 0) + (75/100 × 0.28) = 0.21
✅ Lebih baik lagi!
```

### Kenapa Gini?

Algoritma coba **SEMUA** kombinasi split:
- Gender: Male/Female
- Age: < 5, < 6, < 7, ... < 100
- Ticket: < $10, < $20, < $30, ...

Komputer ngitung cepat banget, jadi bisa coba semua!

### Metrics Lain Selain Gini

1. **Gini Impurity** ✅ (paling umum)
2. **Entropy** / **Information Gain**
3. **Loss Function**

Semua tujuannya sama: ukur seberapa pure kelompoknya.

### Overfitting di Decision Tree ⚠️

**Masalah:**
Kalau pohon terlalu dalam, model jadi **terlalu detail** dan hafal data training.

**Solusi:**
1. **Batasi kedalaman** (max_depth)
2. **Min samples per leaf** (misal min 5 data)
3. **Pruning** (potong cabang yang gak penting)

**Contoh:**
```python
from sklearn.tree import DecisionTreeClassifier

model = DecisionTreeClassifier(
    max_depth=5,           # Maksimal 5 level
    min_samples_leaf=10    # Min 10 data per daun
)
```

### Decision Tree untuk Klasifikasi vs Regresi

| Tipe | Nama di Sklearn | Kegunaan |
|------|----------------|----------|
| **Klasifikasi** | `DecisionTreeClassifier` | Prediksi kategori (Yes/No, 0/1) |
| **Regresi** | `DecisionTreeRegressor` | Prediksi angka (harga, umur) |

---

## 🎲 RANDOM FOREST

### Apa itu Random Forest?

**Random Forest = Banyak Decision Tree digabung!**

```
Random Forest = Hutan Decision Tree 🌲🌲🌲
```

**Kenapa Lebih Bagus?**
- Decision Tree cuma 1 pohon → gampang overfitting
- Random Forest punya banyak pohon → lebih stabil
- Setiap pohon "vote" → hasil prediksi lebih akurat

**Parameter Penting:**
```python
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier(
    n_estimators=100,  # Jumlah pohon = 100
    max_depth=5
)
```

### Perbedaan Setiap Pohon di Random Forest

**1. Fitur yang Dipakai Beda**
- Pohon 1: pakai 6 dari 10 kolom
- Pohon 2: pakai 6 kolom lain

**2. Data yang Dipakai Beda (Bootstrapping)**
- Pohon 1: row 1-100
- Pohon 2: row 50-150

Ini yang bikin setiap pohon punya "perspektif" beda!

---

## 🎯 LOGISTIC REGRESSION

### Perbedaan Linear vs Logistic Regression

| Linear Regression | Logistic Regression |
|------------------|---------------------|
| Prediksi **angka** (harga, suhu) | Prediksi **kategori** (0/1, Yes/No) |
| Output: angka bebas | Output: 0 sampai 1 (probability) |
| `y = wx + b` | `y = sigmoid(wx + b)` |

### Euler's Number (e)

Sebelum masuk sigmoid, kenalan dulu sama **e**!

**e ≈ 2.718**

e adalah konstanta matematika (kayak π = 3.14)

**Di Python:**
```python
import numpy as np
np.e        # 2.718281828...
np.exp(1)   # 2.718... (e^1)
np.exp(2)   # 7.389... (e^2)
```

### Fungsi Sigmoid - Jantungnya Logistic Regression! ❤️

**Fungsi Sigmoid:**
```
σ(z) = 1 / (1 + e^(-z))
```

**Slogan Sigmoid:**
> "Berapapun nilainya, hasilnya pasti 0 sampai 1"

**Grafik Sigmoid:**
```
   1.0 |           ________
       |         /
   0.5 |       /
       |     /
   0.0 |____/
       -10  -5   0   5   10
```

**Contoh:**
```python
def sigmoid(z):
    return 1 / (1 + np.exp(-z))

sigmoid(-5)    # 0.007 (hampir 0)
sigmoid(0)     # 0.5   (tengah-tengah)
sigmoid(5)     # 0.993 (hampir 1)
```

### Cara Kerja Logistic Regression

**Step 1: Linear Regression**
```
z = wx + b
Misal: z = 10.5
```

**Step 2: Masukkan ke Sigmoid**
```
y = sigmoid(z)
y = sigmoid(10.5) = 0.99
```

**Step 3: Threshold (default = 0.5)**
```
If y ≥ 0.5 → Prediksi = 1
If y < 0.5 → Prediksi = 0

0.99 ≥ 0.5 → Prediksi = 1 ✅
```

### Threshold - Mengubah Probability jadi Kelas

**Kenapa Perlu Threshold?**
- Data aktual: 0, 1, 0, 1, 1
- Hasil sigmoid: 0.3, 0.7, 0.2, 0.9, 0.6

Gak bisa bandingkan langsung! Harus diubah jadi 0/1.

**Cara Ubah:**
```
Threshold = 0.5 (default)

0.3 < 0.5 → 0
0.7 ≥ 0.5 → 1
0.2 < 0.5 → 0
0.9 ≥ 0.5 → 1
0.6 ≥ 0.5 → 1
```

**Tuning Threshold (Advanced):**
Bisa diubah sesuai kebutuhan:
- Threshold = 0.3 → Lebih banyak prediksi 1
- Threshold = 0.7 → Lebih sedikit prediksi 1

### Cost Function - Log Loss

**Linear Regression:** Mean Squared Error (MSE)
**Logistic Regression:** Log Loss (Binary Cross-Entropy)

**Rumus Log Loss:**
```
Loss = -[y × log(p) + (1-y) × log(1-p)]
```

**Keterangan:**
- y = label aktual (0 atau 1)
- p = predicted probability (hasil sigmoid)

**Kenapa 2 Part?**
- Part 1: `-y × log(p)` → untuk kelas y=1
- Part 2: `-(1-y) × log(1-p)` → untuk kelas y=0

**Intuisi:**
- Model prediksi benar → loss kecil
- Model prediksi salah → loss besar

**Visualisasi:**
```
Kelas y=1:
- Prediksi mendekati 1 → loss kecil
- Prediksi mendekati 0 → loss besar

Kelas y=0:
- Prediksi mendekati 0 → loss kecil
- Prediksi mendekati 1 → loss besar
```

### Optimisasi - Cari W dan B Optimal

**Manual (Susah):**
```python
w, b = 0.5, 1.0
loss = compute_loss(w, b)  # 7.9

# Coba-coba manual
w, b = 0.46, 1.2
loss = compute_loss(w, b)  # 7.7 (lebih bagus)

# ... terus coba sampe dapet yang terkecil
```

**Pakai Sklearn (Otomatis!):**
```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression()
model.fit(X_train, y_train)

# Langsung dapet W dan B optimal!
model.coef_       # W optimal
model.intercept_  # B optimal
```

**Teknik Optimisasi (Solver):**
- LBFGS
- Newton-CG
- SAG
- SAGA
- Liblinear

Gak usah hafal, default aja udah bagus!

### Logistic Regression untuk Multi-Class

**Default:** Binary classification (2 kelas)

**Bisa Multi-class?** Bisa!

```python
model = LogisticRegression(
    multi_class='multinomial',  # Untuk >2 kelas
    solver='lbfgs'              # Solver yang support multinomial
)
```

**Activation Function:**
- Binary → **Sigmoid**
- Multi-class → **Softmax**

---

## 🎭 SIGMOID vs SOFTMAX

### Sigmoid (Binary)

**Input:** 1 angka
**Output:** 1 probability (0-1)

```python
sigmoid(2.5) → 0.92
```

### Softmax (Multi-class)

**Input:** Vektor angka
**Output:** Vektor probability (total = 1)

```python
softmax([2.0, 1.0, 0.1])
→ [0.66, 0.24, 0.10]  # Total = 1.0
```

**Kenapa Perlu Softmax?**
Kalau pakai sigmoid untuk 3 kelas:
```
sigmoid(2.0) = 0.88
sigmoid(1.0) = 0.73
sigmoid(0.1) = 0.52
Total = 2.13 ❌ (Harusnya 1.0!)
```

Softmax otomatis bikin totalnya = 1.0!

---

## 🔧 IMPLEMENTATION TIPS

### Decision Tree di Sklearn

```python
from sklearn.tree import DecisionTreeClassifier

# Klasifikasi
clf = DecisionTreeClassifier(
    criterion='gini',      # atau 'entropy'
    max_depth=5,
    min_samples_leaf=10
)

# Training
clf.fit(X_train, y_train)

# Prediksi
predictions = clf.predict(X_test)
```

### Random Forest di Sklearn

```python
from sklearn.ensemble import RandomForestClassifier

# Banyak pohon!
rf = RandomForestClassifier(
    n_estimators=100,     # 100 pohon
    max_depth=5,
    max_features='sqrt'   # Fitur random per pohon
)

rf.fit(X_train, y_train)
predictions = rf.predict(X_test)
```

### Logistic Regression di Sklearn

```python
from sklearn.linear_model import LogisticRegression

# Binary classification
lr = LogisticRegression()
lr.fit(X_train, y_train)

# Lihat koefisien
print(lr.coef_)        # W
print(lr.intercept_)   # B

# Prediksi probability
probs = lr.predict_proba(X_test)

# Prediksi kelas
predictions = lr.predict(X_test)
```

---

## 📊 KLASIFIKASI vs REGRESI

### Klasifikasi
**Output:** Kategori
**Contoh:** 
- Spam atau bukan
- Tumor ganas atau jinak
- Hujan atau tidak

**Algoritma:**
- Logistic Regression
- Decision Tree Classifier
- Random Forest Classifier
- KNN
- SVM

### Regresi
**Output:** Angka
**Contoh:**
- Harga rumah
- Suhu besok
- Harga saham

**Algoritma:**
- Linear Regression
- Decision Tree Regressor
- Random Forest Regressor

---

## 🎯 WORKFLOW MACHINE LEARNING

### 1. Pilih Algoritma
Coba beberapa algoritma dengan cara kerja beda:
1. **Logistic Regression** (linear + sigmoid)
2. **Random Forest** (tree-based)
3. **KNN atau SVM** (distance-based)
4. **Light GBM** (advanced boosting)

### 2. Training
```python
model.fit(X_train, y_train)
```

### 3. Evaluasi
```python
score = model.score(X_test, y_test)
predictions = model.predict(X_test)
```

### 4. Tuning
Ubah hyperparameter kalau hasilnya kurang bagus.

---

## 💡 TIPS & TRICKS

### 1. Gak Ada "Model Terbaik"
Tiap dataset beda, model terbaik juga beda. **Coba semua!**

### 2. Overfitting = Musuh Utama
**Tanda-tanda:**
- Training score tinggi (99%)
- Test score rendah (60%)

**Solusi:**
- Kurangi kompleksitas model
- Tambah data
- Regularization

### 3. Decision Tree vs Random Forest
**Decision Tree:**
- ✅ Cepat
- ✅ Mudah interpretasi
- ❌ Gampang overfit

**Random Forest:**
- ✅ Lebih akurat
- ✅ Gak gampang overfit
- ❌ Lebih lambat
- ❌ Susah interpretasi

### 4. Linear vs Logistic Regression
Namanya mirip tapi beda total!
- Linear → angka
- Logistic → kategori

### 5. Activation Function (Neural Network)
Di ML tradisional:
- Logistic → Sigmoid
- Linear → Linear (no activation)

Di Neural Network bisa pakai:
- ReLU
- Sigmoid
- Softmax
- Tanh
- Dan lain-lain

---

## 🎓 EXERCISE

### 1. Decision Tree Exercise
**Dataset:** Holiday di Bali
**Fitur:** Price, Check, dll
**Target:** Apakah jadi holiday?

**Task:**
- Hitung Gini original
- Coba split berdasarkan berbagai fitur
- Cari split dengan Weighted Gini terkecil
- Bisa lebih bagus dari 0.27?

### 2. Hands-on Tips
- Main-main dengan Google Sheets untuk hitung Gini
- Pakai rumus `COUNTIF` untuk hitung kategori
- Coba berbagai threshold value

---

## 📚 KEY TAKEAWAYS

1. **Decision Tree** = Pohon keputusan berdasarkan **Gini Impurity**
2. **Random Forest** = Gabungan banyak Decision Tree
3. **Logistic Regression** = Linear Regression + **Sigmoid Function**
4. **Sigmoid** = Ubah angka apapun jadi probability (0-1)
5. **Threshold** = Ubah probability jadi kelas (default 0.5)
6. **Gini = 0** = Perfect! Semua data pure
7. **e ≈ 2.718** = Konstanta matematika penting
8. **Log Loss** = Cost function untuk Logistic Regression
9. **Optimisasi** = Cari W dan B terbaik (pakai solver)
10. **No Free Lunch** = Gak ada model yang terbaik untuk semua kasus

---

## 🚀 NEXT STEPS

**Minggu Depan:**
- Support Vector Machine (SVM)
- K-Nearest Neighbors (KNN)
- Deep Learning basics

**Practice:**
- Coba Decision Tree exercise
- Eksperimen dengan threshold
- Bandingkan berbagai algoritma

---

**Happy Learning! 🎉**

*"Machine Learning bukan tentang hafal rumus, tapi paham konsepnya!"*
