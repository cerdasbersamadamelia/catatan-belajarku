# Supervised Learning - 4: Support Vector Machine (SVM)

---

## 📌 Recap Machine Learning

Sebelum masuk ke SVM, mari kita ingat kembali:

### Hirarki AI
- **AI** → **Machine Learning** → **Deep Learning**
- Machine Learning bagian dari AI
- Deep Learning bagian dari Machine Learning

### Tipe Machine Learning
1. **Supervised Learning**
   - **Regresi**: Linear Regression
   - **Klasifikasi**: 
     - Logistic Regression
     - Decision Tree
     - KNN (K-Nearest Neighbors)
     - **SVM (Support Vector Machine)** ← Topik kita hari ini!

2. **Unsupervised Learning** (nanti Rabu)
   - Clustering
   - Anomaly Detection
   - Dimensionality Reduction

> **Catatan Penting**: Decision Tree, KNN, dan SVM juga bisa dipakai untuk regresi dengan sedikit modifikasi!

---

## 🎯 Apa Itu Support Vector Machine (SVM)?

### Definisi Simpel
SVM adalah algoritma yang mencoba **membuat garis pemisah (decision boundary)** untuk membedakan kelas A dan kelas B.

### Konsep Dasar
- Mirip dengan Logistic Regression → sama-sama bikin garis lurus
- Persamaan mirip Linear Regression: `wx + b`
- Bedanya: SVM mencari **hyperplane yang paling optimal**

### Visualisasi
```
Data Merah (●)     |     Data Biru (●)
       ●           |           ●
     ●             |             ●
       ●           |           ●
                   ← Hyperplane
```

---

## 🔑 Istilah-Istilah Penting

### 1. Hyperplane
**Apa itu?** Garis lurus yang memisahkan kelas-kelas data

**Dimensi:**
- Data 2D → Hyperplane 1D (garis)
- Data 3D → Hyperplane 2D (bidang)
- Data 4D → Hyperplane 3D
- Dan seterusnya...

**Fungsi:** Decision boundary yang memisahkan kelas satu dengan lainnya

### 2. Support Vector
**Apa itu?** Data point yang **paling dekat** dengan hyperplane

**Ciri-ciri:**
- Ditandai dengan lingkaran pada visualisasi
- Jumlahnya bisa lebih dari 2
- Sangat berpengaruh pada posisi hyperplane

**Contoh:**
```
Support Vector (⊙)    Hyperplane    Support Vector (⊙)
     ⊙ ●                  |                ● ⊙
       ●                  |                ●
     ● ⊙                  |                ⊙ ●
```

### 3. Margin
**Apa itu?** Jarak antara support vector dengan hyperplane

**Rumus:**
- Hyperplane: `wx + b = 0`
- Support Vector kelas positif: `wx + b = 1`
- Support Vector kelas negatif: `wx + b = -1`

**Margin Width:** Jarak total antara kedua support vector

### 4. Maximum Margin
**Konsep:** SVM mencari hyperplane yang memiliki **margin terbesar**

**Kenapa?** Karena margin yang besar berarti:
- Pemisahan lebih jelas
- Model lebih robust
- Lebih optimal dalam membedakan kelas

---

## 🤔 Kenapa Hyperplane Ini yang Optimal?

Bayangkan ada 3 garis (ungu, biru, hijau):

### Garis Ungu ❌
- Margin dengan data merah: KECIL
- Margin dengan data biru: BESAR
- **Tidak optimal** → tidak seimbang

### Garis Hijau ❌
- Margin dengan data merah: BESAR
- Margin dengan data biru: KECIL
- **Tidak optimal** → tidak seimbang

### Garis Biru ✅
- Margin dengan data merah: OPTIMAL
- Margin dengan data biru: OPTIMAL
- **OPTIMAL** → seimbang dan jauh dari kedua kelas!

---

## 📐 Rumus Matematika SVM

### Persamaan Dasar
```
Hyperplane:           wx + b = 0
Support Vector (+):   wx + b = 1
Support Vector (-):   wx + b = -1
```

### Klasifikasi Data Baru
```
wx + b < 0   → Kelas 0 (negatif)
wx + b ≥ 0   → Kelas 1 (positif)
```

### Margin Width
```
Margin = 2 / ||w||
```
Dimana `||w||` adalah Euclidean norm dari w

---

## 💪 Hard Margin vs Soft Margin

### Hard Margin
**Aturan:** Data **TIDAK BOLEH** ada di dalam margin width

**Karakteristik:**
- Sangat ketat (saklek)
- Perfect separation
- Jarang terjadi di dunia nyata
- Tidak toleran terhadap outlier

**Constraint:**
```
y_i(wx_i + b) ≥ 1  untuk semua data
```

**Kapan Dipakai?** Hanya ketika data benar-benar linearly separable dan perfect

### Soft Margin ⭐ (Yang Sering Dipakai)
**Aturan:** Data **BOLEH** ada di dalam margin width (dengan penalti)

**Karakteristik:**
- Lebih fleksibel
- Toleran terhadap outlier
- Realistis untuk data dunia nyata
- Bisa diatur tingkat "kelembutan"nya

**Kenapa Perlu?**
- Data dunia nyata jarang perfect
- Ada outlier/noise
- Beberapa data punya karakteristik yang overlap

**Contoh Kasus:**
```
Data Merah Normal (●)    Hyperplane    Data Biru Normal (●)
     ● ● ●                   |                ● ● ●
       ● ●                   |                ● ●
     ● ● ⊙ ← Outlier        |                ● ●
```

Dengan soft margin, outlier ini tidak akan menggeser hyperplane terlalu jauh.

---

## ⚙️ Parameter C (Slack Variable)

### Apa Itu Parameter C?
**C adalah parameter regularisasi** yang mengatur seberapa "soft" soft margin kita.

### Pengaruh Nilai C

#### C Kecil (mendekati 0)
- Margin sangat lebar
- Banyak toleransi terhadap misclassification
- **Underfitting** (terlalu general)
- Hinge loss kecil

#### C Besar (misal 10, 100, 1000)
- Margin lebih sempit
- Sedikit toleransi terhadap misclassification
- Mendekati **Hard Margin**
- **Overfitting** (terlalu spesifik)

#### C = 1 (Default)
- Balanced
- Starting point yang bagus

### Dalam Loss Function
```
L = (1/n) Σ hinge_loss + C × ||w||²
```

**Interpretasi:**
- Bagian pertama: Hinge loss (error)
- Bagian kedua: Regularization term (margin width)
- C mengatur trade-off antara keduanya

### Cara Tuning Parameter C
```python
from sklearn.svm import SVC

# C kecil
svm1 = SVC(C=0.1)

# C default
svm2 = SVC(C=1)

# C besar
svm3 = SVC(C=10)
```

---

## 📊 Hinge Loss

### Apa Itu Hinge Loss?
**Fungsi loss khusus untuk SVM** yang mengukur seberapa optimal hyperplane kita.

### Rumus
```
Hinge Loss = max(0, 1 - y_i × (wx_i + b))
```

### Cara Kerja

#### Contoh Data A (Correctly Classified & Di Luar Margin)
```
y = 1
wx + b = 0.5
Hinge Loss = max(0, 1 - 1 × 0.5) = max(0, 0.5) = 0.5
```
**Error:** 0 (karena negatif diabaikan) ✅

#### Contoh Data B (Correctly Classified Tapi Di Dalam Margin)
```
y = 1
wx + b = 0.8
Hinge Loss = max(0, 1 - 1 × 0.8) = max(0, 0.2) = 0.2
```
**Error:** 0.2 (tetap kena penalti!) ⚠️

#### Contoh Data C (Misclassified)
```
y = 1
wx + b = -1.5
Hinge Loss = max(0, 1 - 1 × (-1.5)) = max(0, 2.5) = 2.5
```
**Error:** 2.5 (penalti besar!) ❌

### Interpretasi
- **0**: Perfect! Data di posisi yang benar dan di luar margin
- **< 1**: Correct tapi masih di dalam margin
- **≥ 1**: Misclassified atau sangat dekat dengan hyperplane

---

## 🌀 Kernel Trick

### Masalah: Data Non-Linear
Kalau data kita **tidak bisa dipisahkan dengan garis lurus**, gimana?

**Contoh:**
```
     ●     ○ ○ ○     ●
   ●   ●   ○   ○   ●   ●
     ●     ○ ○ ○     ●
```
Data melingkar seperti ini tidak bisa dipisahkan dengan garis lurus!

### Solusi: Kernel Trick!
Kernel mengubah data ke dimensi yang lebih tinggi supaya bisa dipisahkan.

### Jenis-Jenis Kernel

#### 1. Linear Kernel (Default)
```python
SVC(kernel='linear')
```
- Untuk data yang linearly separable
- Paling cepat
- Paling mudah diinterpretasi
- Hasil: Garis lurus

#### 2. Polynomial Kernel
```python
SVC(kernel='poly', degree=3)
```
- Untuk data yang polanya polynomial
- Bisa atur degree (2, 3, 4, dst)
- Hasil: Garis lengkung

#### 3. RBF (Radial Basis Function) ⭐ Paling Populer!
```python
SVC(kernel='rbf')
```
- Untuk data dengan pola radial/melingkar
- **Paling sering dipakai**
- Sangat fleksibel
- Hasil: Boundary yang kompleks

#### 4. Sigmoid Kernel
```python
SVC(kernel='sigmoid')
```
- Mirip fungsi aktivasi neural network
- Jarang dipakai
- Hasil: Boundary sigmoid

### Visualisasi Perbandingan Kernel

```
Linear:          Polynomial:        RBF:
●  ●  |  ○  ○    ●  ●  ⌒  ○  ○     ●  ●  ⊂ ○ ○ ⊃
●  ●  |  ○  ○    ●  ●  ⌣  ○  ○     ●  ●  ⊂ ○ ○ ⊃
```

---

## 💻 Implementasi di Python (Scikit-learn)

### Import Library
```python
from sklearn.svm import SVC, SVR
from sklearn.model_selection import train_test_split
from sklearn.metrics import roc_auc_score, precision_score, recall_score
```

### Persiapan Data
```python
# Split data
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=2024, stratify=y
)
```

### Training Model

#### SVM untuk Classification (SVC)
```python
# Default (RBF kernel, C=1)
svm_clf = SVC()
svm_clf.fit(X_train, y_train)

# Custom parameters
svm_clf = SVC(
    kernel='linear',    # Pilih kernel
    C=5,               # Slack variable
    probability=True   # Untuk mendapatkan probability
)
svm_clf.fit(X_train, y_train)
```

#### SVM untuk Regression (SVR)
```python
from sklearn.svm import SVR

svr = SVR(kernel='rbf', C=1)
svr.fit(X_train, y_train)
```

### Prediksi
```python
# Prediksi kelas
y_pred = svm_clf.predict(X_test)

# Prediksi probabilitas (butuh probability=True)
y_pred_proba = svm_clf.predict_proba(X_test)
```

### Evaluasi Model
```python
# Untuk classification
roc_auc = roc_auc_score(y_test, y_pred)
precision = precision_score(y_test, y_pred)
recall = recall_score(y_test, y_pred)

print(f"ROC-AUC: {roc_auc:.2f}")
print(f"Precision: {precision:.2f}")
print(f"Recall: {recall:.2f}")
```

### Mengakses Informasi Model
```python
# Lihat support vectors
print(svm_clf.support_vectors_)

# Lihat koefisien (w)
print(svm_clf.coef_)

# Lihat intercept (b)
print(svm_clf.intercept_)

# Lihat parameter
print(svm_clf.get_params())
```

---

## 🎯 Tuning Probability Threshold

### Masalah Default
Secara default, threshold = 0.5:
- Prob ≥ 0.5 → Kelas 1
- Prob < 0.5 → Kelas 0

### Cara Mengatur Threshold
```python
import numpy as np

# Prediksi probability
y_pred_proba = svm_clf.predict_proba(X_test)[:, 1]

# Custom threshold
threshold = 0.3
y_pred_custom = np.where(y_pred_proba >= threshold, 1, 0)

# Evaluasi dengan threshold baru
precision = precision_score(y_test, y_pred_custom)
recall = recall_score(y_test, y_pred_custom)
```

### Efek Mengubah Threshold
- **Threshold rendah (0.3)**: Recall naik, Precision turun
- **Threshold tinggi (0.7)**: Precision naik, Recall turun
- **Threshold 0.5**: Balanced

---

## 🔧 Hyperparameter Tuning

### Parameter yang Bisa Dituning

#### 1. C (Regularization)
```python
# Coba berbagai nilai C
for c in [0.1, 1, 5, 10, 100]:
    svm = SVC(C=c)
    svm.fit(X_train, y_train)
    score = svm.score(X_test, y_test)
    print(f"C={c}: Score={score:.4f}")
```

#### 2. Kernel
```python
# Coba berbagai kernel
for kernel in ['linear', 'poly', 'rbf', 'sigmoid']:
    svm = SVC(kernel=kernel)
    svm.fit(X_train, y_train)
    score = svm.score(X_test, y_test)
    print(f"Kernel={kernel}: Score={score:.4f}")
```

#### 3. Degree (untuk Polynomial)
```python
# Coba berbagai degree
for degree in [2, 3, 4, 5]:
    svm = SVC(kernel='poly', degree=degree)
    svm.fit(X_train, y_train)
    score = svm.score(X_test, y_test)
    print(f"Degree={degree}: Score={score:.4f}")
```

---

## ✅ Kelebihan SVM

1. **Efektif di high-dimensional space**
   - Bagus untuk data dengan banyak fitur
   - Tetap bagus walaupun jumlah fitur > jumlah sampel

2. **Memory efficient**
   - Hanya menggunakan support vectors
   - Tidak perlu simpan semua training data

3. **Versatile**
   - Bisa linear dan non-linear
   - Berbagai kernel function

4. **Robust terhadap overfitting**
   - Terutama di high-dimensional space
   - Dengan parameter C yang tepat

---

## ❌ Kekurangan SVM

1. **Lambat untuk dataset besar**
   - Training time: O(n² × m) sampai O(n³ × m)
   - n = jumlah sampel, m = jumlah fitur

2. **Sensitif terhadap noise dan outlier**
   - Terutama dengan Hard Margin
   - Perlu preprocessing yang baik

3. **Tidak memberikan probability estimates secara langsung**
   - Perlu set `probability=True`
   - Computational cost lebih tinggi

4. **Susah diinterpretasi**
   - Terutama dengan kernel non-linear
   - Tidak sejelas Decision Tree

5. **Butuh feature scaling**
   - SVM sensitif terhadap skala fitur
   - Wajib normalisasi/standarisasi

---

## 🎓 Tips & Best Practices

### 1. Preprocessing Wajib!
```python
from sklearn.preprocessing import StandardScaler

# Scaling data
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

### 2. Mulai dari yang Simpel
```python
# Step 1: Coba Linear dulu
svm_linear = SVC(kernel='linear')

# Step 2: Kalau kurang bagus, coba RBF
svm_rbf = SVC(kernel='rbf')
```

### 3. Cek Overfitting
```python
# Evaluasi di training set
train_score = svm.score(X_train, y_train)
# Evaluasi di test set
test_score = svm.score(X_test, y_test)

# Kalau beda > 10%, kemungkinan overfitting
if train_score - test_score > 0.1:
    print("Warning: Possible overfitting!")
```

### 4. Untuk Data Imbalanced
```python
# Gunakan class_weight
svm = SVC(class_weight='balanced')
```

### 5. Hindari SVM untuk Kasus Ini
- Data dengan fitur sangat banyak (> 300-400)
- Dataset sangat besar (> 100k rows)
- Data gambar/teks (lebih baik pakai Deep Learning)

---

## 📊 Kapan Pakai SVM?

### Cocok Untuk:
✅ Dataset medium-sized (1k - 100k rows)  
✅ Fitur tidak terlalu banyak (< 300)  
✅ Data dengan clear margin of separation  
✅ Butuh model yang robust  
✅ Binary classification atau multi-class  

### Tidak Cocok Untuk:
❌ Dataset sangat besar  
❌ Real-time prediction (lambat)  
❌ Interpretability penting  
❌ Data gambar/teks/suara → Pakai Deep Learning!  

---

## 🆚 SVM vs Algoritma Lain

### SVM vs Logistic Regression
| Aspek | SVM | Logistic Regression |
|-------|-----|---------------------|
| Interpretability | Susah | Mudah |
| Speed | Lambat | Cepat |
| Non-linear | Bisa (kernel) | Butuh feature engineering |
| Overfitting | Lebih resistant | Lebih mudah overfit |

### SVM vs Decision Tree
| Aspek | SVM | Decision Tree |
|-------|-----|---------------|
| Interpretability | Susah | Sangat mudah |
| Speed | Lambat | Cepat |
| Feature scaling | Wajib | Tidak perlu |
| Overfitting | Medium | Mudah (perlu pruning) |

### SVM vs KNN
| Aspek | SVM | KNN |
|-------|-----|-----|
| Training time | Lama | Instant |
| Prediction time | Cepat | Lambat |
| Memory | Efisien | Perlu simpan semua data |
| Distance-based | Ya | Ya |

---

## 🏆 Strategi Pemodelan (dari Instruktur)

Urutan algoritma yang disarankan:

### 1. Logistic Regression (WAJIB!)
- Paling simpel
- Mudah diinterpretasi
- Baseline model

### 2. KNN atau SVM
- Distance-based model
- Sedikit lebih complex

### 3. Random Forest
- Tree-based + Ensemble
- Sangat powerful

### 4. Gradient Boosting (LGBM/XGBoost)
- State-of-the-art
- Butuh tuning hyperparameter banyak
- Paling powerful

**Ingat:** Tidak ada satu model terbaik untuk semua data! Harus eksperimen!

---

## 💡 Studi Kasus: Breast Cancer Classification

### Dataset
- 569 sampel
- 30 fitur (numerical)
- Target: Benign (0) vs Malignant (1)

### Code Lengkap
```python
import pandas as pd
from sklearn.svm import SVC
from sklearn.model_selection import train_test_split
from sklearn.metrics import roc_auc_score, precision_score, recall_score
from sklearn.preprocessing import StandardScaler

# Load data
data = pd.read_csv('breast_cancer.csv')

# Preprocessing
data['diagnosis'] = data['diagnosis'].replace({'B': 0, 'M': 1})

# Split features & target
X = data.drop(columns=['diagnosis', 'id'])
y = data['diagnosis']

# Train-test split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=2024, stratify=y
)

# Feature scaling (PENTING!)
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# Training
svm_clf = SVC(kernel='linear', C=5, probability=True)
svm_clf.fit(X_train_scaled, y_train)

# Prediction
y_pred = svm_clf.predict(X_test_scaled)
y_pred_proba = svm_clf.predict_proba(X_test_scaled)[:, 1]

# Evaluation
roc_auc = roc_auc_score(y_test, y_pred)
precision = precision_score(y_test, y_pred)
recall = recall_score(y_test, y_pred)

print(f"ROC-AUC: {roc_auc:.2%}")
print(f"Precision: {precision:.2%}")
print(f"Recall: {recall:.2%}")

# Hasil dengan C=5, kernel='linear'
# ROC-AUC: 89%
# Precision: 94%
# Recall: 80%
```

---

## 🎯 Kesimpulan

### Poin-Poin Penting SVM:
1. **SVM = Mencari hyperplane optimal** dengan margin maksimal
2. **Support Vector** = Data terdekat dengan hyperplane
3. **Margin** = Jarak support vector ke hyperplane
4. **Hard Margin** = Tidak toleran error (jarang dipakai)
5. **Soft Margin** = Toleran error (lebih realistis)
6. **Parameter C** = Mengatur soft margin
7. **Kernel** = Mengatasi non-linearity
8. **Hinge Loss** = Loss function khusus SVM

### Yang Harus Diingat:
- ✅ Wajib feature scaling!
- ✅ Mulai dari linear, baru coba kernel lain
- ✅ Tuning C dan kernel untuk performa optimal
- ✅ Cek overfitting dengan compare train vs test score
- ✅ SVM bagus untuk data medium-sized

### Next Steps:
1. Praktik dengan dataset sendiri
2. Coba berbagai kombinasi hyperparameter
3. Bandingkan dengan algoritma lain
4. Deploy model (akan dipelajari kemudian)

---

## 📚 Referensi & Resources

### Dokumentasi Resmi:
- [Scikit-learn SVM](https://scikit-learn.org/stable/modules/svm.html)
- [SVM Parameters Guide](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html)

### Untuk Belajar Lebih Lanjut:
- Kernel Trick explanation
- Mathematical derivation of SVM
- SVM for multiclass classification
- SVM vs Deep Learning

### Dataset untuk Latihan:
- Kaggle: Breast Cancer Wisconsin
- UCI ML Repository
- Scikit-learn built-in datasets

---

**🎉 Selamat Belajar SVM! Happy Coding! 🚀**

> *"SVM itu kayak mencari garis pemisah terbaik. Margin besar = pemisahan jelas = model bagus!"*