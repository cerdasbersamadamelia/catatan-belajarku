# Unsupervised Learning 🚀

---

## 📋 Agenda
1. Pengumuman
2. Materi Sesi 1 - Clustering
3. Istirahat
4. Materi Sesi 2 - Anomaly Detection & PCA
5. Q&A

---

## 🔔 Pengumuman Penting

### Tugas LMS:
- **Post Test Machine Learning** - Deadline: Jumat tengah malam
- **Pre Test Deep Learning** - Deadline: Jumat 19 Januari tengah malam

### Ketentuan:
- ✅ Wajib dikerjakan melalui LMS
- ⚠️ Tidak ada perpanjangan deadline
- 📊 Nilai dihitung dalam penilaian akhir
- 🔢 Pre-test: 1 kesempatan | Post-test: 3 kesempatan
- 🚫 Hindari buka tab lain saat mengerjakan
- 🔑 Pre-test adalah syarat akses materi di LMS

---

## 🎯 Apa itu Unsupervised Learning?

### Perbedaan dengan Supervised Learning

| Supervised Learning | Unsupervised Learning |
|---------------------|------------------------|
| Ada data X dan target Y | Hanya ada data X |
| Ada label/target | Tidak ada label |
| Prediksi nilai tertentu | Menemukan pola/struktur |

**Contoh:**
- **Supervised**: Data X (fitur) → Y (label) → Prediksi
- **Unsupervised**: Data X (fitur) → Cari pola → Kelompokkan/deteksi

---

## 📊 Tipe-Tipe Unsupervised Learning

### 1. **Clustering** 🎪
Mengelompokkan data berdasarkan kemiripan

**Aplikasi:**
- Customer segmentation (promosi targeted)
- Pengelompokan pelanggan berdasarkan perilaku
- Analisis pasar

### 2. **Anomaly Detection** 🔍
Mendeteksi data yang tidak normal/outlier

**Aplikasi:**
- Deteksi fraud perbankan
- Deteksi transaksi mencurigakan
- Network security (deteksi hacker)

### 3. **Dimensionality Reduction** 📉
Mengurangi jumlah fitur/kolom

**Aplikasi:**
- Mempercepat proses machine learning
- Visualisasi data
- Mengurangi kompleksitas

### 4. **Association** 🔗
Mencari hubungan antar variabel

---

## 🎯 CLUSTERING

### Apa itu Clustering?
Clustering = mengelompokkan data yang mirip jadi satu kelompok (cluster)

**Analogi:**
- Perumahan cluster: rumah dalam 1 cluster hampir sama semua
- Cluster Lavesh vs Cluster Toronto → berbeda tapi dalam clusternya sama

---

## 🔵 K-Means Clustering

### Algoritma Clustering yang Populer

| Algoritma | Kelebihan | Kekurangan |
|-----------|-----------|------------|
| **K-Means** | Cepat, mudah dipahami | Harus tentukan jumlah cluster di awal |
| **DBSCAN** | Auto-detect jumlah cluster, bisa deteksi noise | Proses lambat |
| **Agglomerative** | Visualisasi dendogram | Lebih kompleks |

**K-Means = "Hello World" dari Clustering!**

---

### Perbedaan K-Means vs KNN

| K-Means | KNN |
|---------|-----|
| **Clustering** (unsupervised) | **Klasifikasi** (supervised) |
| Belum tahu clusternya | Sudah tahu clusternya |
| Membentuk cluster dari awal | Assign data baru ke cluster yang ada |
| Ada centroid (pusat cluster) | Tidak ada centroid |
| Hitung jarak → update centroid | Hitung jarak → assign ke cluster terdekat |

---

### Cara Kerja K-Means

**Step 1: Define jumlah cluster (K)**
```
Contoh: K = 4 (bikin 4 cluster)
```

**Step 2: Initialize centroid secara random**
```
Centroid = pusat dari cluster
Awalnya ditaruh random
```

**Step 3: Hitung jarak setiap data ke semua centroid**
```
Contoh: 100 data × 4 centroid = 400 perhitungan jarak
Rumus jarak: Euclidean distance
```

**Step 4: Assign data ke centroid terdekat**
```
Data A → Cluster 1 (0.1)
Data A → Cluster 2 (0.3)  
Data A → Cluster 3 (0.5)
Data A → Cluster 4 (0.6)
Hasil: Data A masuk Cluster 1 (paling dekat)
```

**Step 5: Recalculate centroid baru**
```
Centroid baru = rata-rata dari semua data di cluster tersebut
```

**Step 6: Ulangi Step 3-5**
```
Sampai:
- Centroid tidak berubah lagi, ATAU
- Mencapai max iteration
```

---

### Contoh Kasus: Customer Segmentation

**Data:**
- Annual Income (pendapatan tahunan)
- Annual Spend (pengeluaran tahunan)

**Tujuan:**
Membuat 2 cluster:
1. **Income Sensitive**: pengeluaran tergantung pendapatan
2. **Income Insensitive**: pengeluaran tidak tergantung pendapatan

**Hasil:**
- Cluster 1: Income tinggi → Spend tinggi
- Cluster 2: Income rendah → Spend rendah
- Cocok untuk strategi promosi berbeda!

---

### Visualisasi K-Means

```
Iterasi 1: Centroid di posisi random
Iterasi 2: Centroid bergerak mendekati data
Iterasi 3: Centroid makin optimal
Iterasi 4: Centroid sudah stabil ✓
```

**Fun Fact:**
Walaupun max iteration 10, centroid bisa stabil hanya dalam 4 iterasi!

---

### Inference (Prediksi Data Baru)

**Contoh:**
```python
# Data baru
new_data = [Income: 50000, Spend: 30000]

# Hitung jarak ke semua centroid
jarak_ke_cluster_1 = 0.3
jarak_ke_cluster_2 = 0.7

# Hasil: Masuk Cluster 1 (lebih dekat)
```

---

### Tips Praktis K-Means

✅ **Kapan pakai K-Means:**
- Data terpisah jelas
- Tahu perkiraan jumlah cluster
- Butuh proses cepat

❌ **Kapan TIDAK pakai K-Means:**
- Data tidak terpisah (overlap banyak)
- Tidak tahu jumlah cluster
- Data berbentuk non-linear

---

### K-Means vs K-Medians

| K-Means | K-Medians |
|---------|-----------|
| Pakai **rata-rata** | Pakai **median** |
| Lebih umum digunakan | Lebih robust terhadap outlier |

---

## 🔴 DBSCAN (Density-Based Spatial Clustering)

### Keunggulan DBSCAN:
- ✅ Auto-detect jumlah cluster
- ✅ Bisa deteksi noise/outlier
- ✅ Bisa handle cluster dengan bentuk aneh
- ❌ Proses lebih lambat

**Cara kerja:**
DBSCAN "scanning" area sekitar data → kelompokkan yang berdekatan

**Kapan pakai:**
- Data memiliki bentuk kompleks
- Tidak tahu jumlah cluster
- Butuh deteksi noise

---

## 🔺 Agglomerative Clustering

**Cara kerja:**
Menggunakan **dendogram** (pohon hierarki)

**Kelebihan:**
- Visualisasi bagus
- Bisa lihat hierarki cluster

**Referensi:**
Lihat dokumentasi di scikit-learn untuk perbandingan semua metode clustering!

---

## 🚨 ANOMALY DETECTION

### Apa itu Anomaly?
Data yang **berbeda** dari data lainnya (outlier/pencilan)

**Kenapa penting?**
- 💳 Fraud detection di bank
- 🔒 Security (deteksi hacker)
- 🏥 Deteksi penyakit langka

---

### Studi Kasus: Cambridge Analytica Scandal 😱

**The Great Hack (2018)**

**Cerita singkat:**
1. Alexander Kogan buat game FB: "This is Digital Life"
2. Game minta data pribadi + preferensi politik
3. Kumpulkan 300,000 user → menjaring 50 juta user!
4. Download 80 juta data via Third Party API Facebook
5. **Security Facebook deteksi anomaly** → tapi didiamkan! 😡
6. Data dipakai Cambridge Analytica untuk kampanye Donald Trump
7. Marketing super spesifik per individu (5000 features!)

**Pelajaran:**
Anomaly detection penting! Kalau diabaikan → bahaya!

---

### Metode Anomaly Detection: Gaussian Distribution

**Konsep:**
Gunakan distribusi normal (Gaussian) untuk deteksi outlier

**Parameter:**
- **μ (mu)**: rata-rata
- **σ² (sigma²)**: varians
- **σ (sigma)**: standar deviasi

**Rumus:**

```
Mean (μ) = Σ(xi) / n

Variance (σ²) = Σ(xi - μ)² / n

Standard Deviation (σ) = √σ²
```

---

### Cara Kerja Anomaly Detection

**Step 1: Gather & Preprocess Data**
```
Bersihkan data, siapkan fitur
```

**Step 2: Parameter Estimation**
```
Hitung μ (mean) dan σ² (variance)
```

**Step 3: Hitung Probability Density Function (PDF)**
```
Rumus PDF Gaussian:
P(x) = 1/(√(2πσ²)) × e^(-(x-μ)²/(2σ²))

Kompleks? Pakai library aja! 😄
```

**Step 4: Set Threshold (ε - epsilon)**
```
if P(x) < ε → ANOMALY
if P(x) ≥ ε → NORMAL
```

**Step 5: Classify**
```
Tag data: Anomaly atau Normal
```

---

### Contoh Praktis

**Data:** Price & Time

**Threshold (ε):**
- ε = 0.1 → terlalu banyak anomaly
- ε = 0.01 → optimal! ✓

**Hasil:**
Data dengan probability < 0.01 = **ANOMALY** (outlier)

---

### Metode Simpel: Box Plot & IQR

**IQR (Interquartile Range) Method:**
```
Q1 = Kuartil 1 (25%)
Q3 = Kuartil 3 (75%)
IQR = Q3 - Q1

Outlier:
- Data < Q1 - 1.5×IQR
- Data > Q3 + 1.5×IQR
```

**Visualisasi:**
Box plot langsung tunjukkan outlier dengan titik-titik di luar box!

---

### Advanced: Isolation Forest

**Konsep:**
Gunakan Decision Tree untuk "mengisolasi" anomaly

**Cara kerja:**
Anomaly lebih mudah diisolasi dengan sedikit split

**Kapan pakai:**
- Sudah pakai machine learning
- Data kompleks
- Butuh akurasi tinggi

---

## 📉 DIMENSIONALITY REDUCTION

### Apa itu Dimensionality Reduction?
Mengurangi jumlah fitur (dimensi) dari data

**Contoh:**
- 3D → 2D
- 1000 fitur → 100 fitur
- 784 pixel (MNIST) → 50 komponen

---

### Tujuan Dimensionality Reduction

1. **Mempercepat proses ML** ⚡
   - 1000 fitur → 1 jam
   - 100 fitur → 15 menit

2. **Visualisasi data** 📊
   - 10 fitur susah divisualisasi
   - 2-3 fitur → mudah di-plot

3. **Mengurangi kompleksitas** 🧠
   - Model lebih sederhana
   - Lebih mudah dipahami

---

## 🎨 PCA (Principal Component Analysis)

### Apa itu PCA?

**Definisi:**
Teknik untuk reduksi dimensi dengan mempertahankan informasi sebanyak mungkin

**Prinsip:**
- Mengubah data menjadi **Principal Components**
- PC tidak berkorelasi satu sama lain
- PC diurutkan berdasarkan variance (terbesar → terkecil)

---

### Kapan Pakai PCA?

✅ **Pakai PCA ketika:**
- Fitur terlalu banyak (>10-100)
- Proses training lama
- Butuh visualisasi data dimensi tinggi
- Fitur berkorelasi tinggi

❌ **JANGAN pakai PCA ketika:**
- Fitur sedikit (<10)
- Proses sudah cepat
- Interpretabilitas penting
- Deep learning (biasanya tidak perlu)

---

### Konsep Penting: Correlation vs Covariance

| Correlation | Covariance |
|-------------|------------|
| Nilai: -1 sampai +1 | Nilai: -∞ sampai +∞ |
| Sudah dinormalisasi | Belum dinormalisasi |
| Mudah interpretasi | Susah interpretasi |
| Untuk analisis | Untuk PCA |

**Kenapa PCA pakai Covariance?**
Karena butuh nilai asli (belum dinormalisasi) untuk perhitungan matematis

---

### Cara Kerja PCA (High-Level)

**Step 1: Center the Data**
```
Kurangi setiap data dengan mean
```

**Step 2: Hitung Covariance Matrix**
```
Matrix yang tunjukkan hubungan antar fitur
```

**Step 3: Hitung Eigenvalue & Eigenvector**
```
Matematika rumit! 🤯
(Library yang handle)
```

**Step 4: Proyeksikan Data**
```
Transform data ke principal components
```

**Step 5: Select Components**
```
Pilih n komponen pertama
```

---

### Contoh PCA: 3 Fitur → 2 Komponen

**Data Awal:**
```
- Math Score
- Reading Score
- Writing Score
```

**Korelasi:**
- Math vs Reading: 0.81 (kuat)
- Reading vs Writing: 0.95 (sangat kuat!)
- Math vs Writing: 0.77 (kuat)

**Setelah PCA:**
```
- Principal Component 1 (PC1)
- Principal Component 2 (PC2)
```

**Korelasi PC1 vs PC2:**
```
≈ 0 (tidak berkorelasi!)
```

---

### Explained Variance Ratio

**Konsep:**
Berapa persen informasi yang dipertahankan?

**Contoh:**
```python
n_components = 2
explained_variance_ratio = [0.90, 0.08]

Total = 98%
```

**Artinya:**
- Kita pertahankan 98% informasi
- Hanya kehilangan 2% informasi
- Worth it untuk reduksi 1 fitur!

---

### Trade-off PCA

**Semakin banyak reduksi:**
- ✅ Proses makin cepat
- ❌ Informasi makin hilang
- ❌ Akurasi bisa turun

**Semakin sedikit reduksi:**
- ✅ Informasi tetap banyak
- ✅ Akurasi tetap tinggi
- ❌ Proses makin lambat

---

### Guideline Explained Variance

**Best Practice:**
- **90%**: Standar bagus
- **80%**: Masih OK untuk dataset besar
- **70%**: Hati-hati, info banyak hilang
- **60%**: Last resort (kasus ekstrem)

**Contoh Kasus Nyata:**
```
Dataset: 30,000 fitur (TF-IDF text)
80% variance: ~1 jam proses ⏰
60% variance: ~15 menit proses ⚡
Trade-off: 20% informasi vs 45 menit waktu!
```

---

### Kelebihan & Kekurangan PCA

**Kelebihan ✅:**
- Mengurangi dimensi efektif
- Hilangkan multikolinearitas
- Mempercepat training
- Bagus untuk visualisasi

**Kekurangan ❌:**
- **Tidak ada makna fisis!**
  - PC1 = apa? 🤷
  - PC2 = apa? 🤷
  - Sulit dijelaskan ke bisnis
- Bisa kehilangan informasi
- Tidak cocok untuk interpretasi

---

### Principal Component: Tidak Ada Makna Fisis

**Sebelum PCA:**
```
Math Score = nilai matematika ✓
Reading Score = nilai membaca ✓
Writing Score = nilai menulis ✓
(Jelas maknanya!)
```

**Setelah PCA:**
```
PC1 = ??? (kombinasi kompleks dari Math, Reading, Writing)
PC2 = ??? (kombinasi kompleks lainnya)
(Tidak jelas maknanya! ❌)
```

---

### Inverse Transform PCA

**Bisa kembali ke data asli?**
YA, tapi tidak 100% sama!

```python
# Transform
pca_data = pca.fit_transform(original_data)

# Inverse Transform
reconstructed_data = pca.inverse_transform(pca_data)

# Hasil: Ada perbedaan kecil
# Original: 72.0
# Reconstructed: 71.9
```

**Kenapa berbeda?**
Karena ada informasi yang hilang saat reduksi!

---

## 🛠️ PCA + Machine Learning

### Workflow Umum

```
Data Asli (1000 fitur)
    ↓
PCA (reduksi ke 100 fitur)
    ↓
K-Means Clustering
    ↓
Hasil Cluster
```

**PCA sebagai Preprocessing!**

---

### PCA + Clustering: Contoh MNIST

**Data MNIST:**
- Gambar digit (0-9)
- 784 pixel = 784 fitur!

**Proses:**
1. PCA: 784 fitur → 50 komponen
2. Variance retained: 85%
3. K-Means: Cluster digit yang mirip
4. Visualisasi: 50 komponen → 2D plot

**Hasil:**
Digit yang mirip otomatis ter-cluster! 🎯

---

## 📚 Metode Lain Dimensionality Reduction

### Selain PCA:

1. **Incremental PCA** - untuk data besar
2. **Kernel PCA** - untuk data non-linear
3. **Truncated SVD** - bagus untuk sparse matrix
4. **NMF (Non-Negative Matrix Factorization)** - bagus untuk data non-negatif
5. **LDA (Latent Dirichlet Allocation)** - untuk topic modeling
6. **t-SNE** - visualisasi dimensi tinggi (lambat!)
7. **UMAP** - alternatif t-SNE (lebih cepat)

**Cek dokumentasi:** scikit-learn.org → Dimensionality Reduction

---

## 🎯 FEATURE SELECTION vs DIMENSIONALITY REDUCTION

### Feature Selection
**Pilih subset fitur yang ada**

**Metode:**
- Correlation-based
- Variance Threshold
- Univariate Selection (Chi², ANOVA)
- RFE (Recursive Feature Elimination)
- Feature Importance

**Kelebihan:**
- Fitur tetap punya makna
- Mudah interpretasi

---

### Dimensionality Reduction (PCA)
**Buat fitur baru (kombinasi fitur lama)**

**Metode:**
- PCA
- SVD
- NMF
- Autoencoder

**Kelebihan:**
- Pertahankan informasi maksimal
- Hilangkan korelasi

---

## 💡 Tips Feature Selection Sederhana

### Metode Correlation-Based

```python
# 1. Hitung korelasi
correlation = data.corr()

# 2. Filter korelasi tinggi
# Ambil yang |r| > 0.5
selected_features = correlation[abs(correlation) > 0.5]
```

**Interpretasi:**
- r > 0.5: Korelasi positif kuat
- r < -0.5: Korelasi negatif kuat
- -0.5 < r < 0.5: Korelasi lemah (bisa dibuang)

---

## 🔥 Feature Importance (After Training)

**Kapan pakai:**
Setelah training model (Random Forest, XGBoost)

```python
# Train model
model = RandomForestClassifier()
model.fit(X_train, y_train)

# Get feature importance
importance = model.feature_importance_

# Plot
plt.bar(features, importance)
```

**Kegunaan:**
- Tahu fitur mana yang paling berpengaruh
- Buang fitur yang tidak penting
- Jelaskan ke manajemen!

---

## 📖 Cara Baca Dokumentasi Scikit-Learn

### Struktur Dokumentasi:

1. **Class Name & Description** 
   ```
   Apa fungsi class ini?
   ```

2. **Parameters (Hyperparameters)**
   ```
   Parameter yang di-set SEBELUM training
   ```

3. **Attributes**
   ```
   Nilai yang didapat SETELAH training
   ```

4. **Methods**
   ```
   Fungsi yang bisa dipanggil:
   - fit(): training
   - predict(): prediksi
   - transform(): transformasi
   ```

5. **Examples**
   ```
   Contoh code lengkap
   ```

---

### Contoh: Linear Regression

```python
from sklearn.linear_model import LinearRegression

# 1. Initialize (set hyperparameters)
model = LinearRegression(fit_intercept=True)

# 2. Fit (training)
model.fit(X_train, y_train)

# 3. Access attributes (setelah fit)
coef = model.coef_          # koefisien
intercept = model.intercept_ # intercept

# 4. Use methods
predictions = model.predict(X_test)
```

**Ingat:**
- **Hyperparameters**: sebelum fit
- **Attributes**: setelah fit
- **Methods**: kapan saja (tergantung method)

---

## 🎓 Rangkuman Unsupervised Learning

### 1. Clustering
**Tujuan:** Kelompokkan data mirip

**Algoritma:**
- K-Means (cepat, perlu K)
- DBSCAN (auto K, lambat)
- Agglomerative (hierarki)

**Aplikasi:**
- Customer segmentation
- Analisis pasar
- Pengelompokan dokumen

---

### 2. Anomaly Detection
**Tujuan:** Deteksi data abnormal

**Metode:**
- Gaussian Distribution
- IQR/Box Plot
- Isolation Forest

**Aplikasi:**
- Fraud detection
- Network security
- Quality control

---

### 3. Dimensionality Reduction
**Tujuan:** Kurangi fitur, pertahankan info

**Metode:**
- PCA (paling populer)
- SVD, NMF, t-SNE

**Aplikasi:**
- Mempercepat ML
- Visualisasi data
- Data preprocessing

---

## 💎 Best Practices

### Clustering:
1. Coba beberapa algoritma
2. Evaluasi dengan Silhouette Score
3. Visualisasi hasilnya
4. Analisis setiap cluster

### Anomaly Detection:
1. Pahami distribusi data
2. Set threshold dengan hati-hati
3. Validasi dengan domain expert
4. Cek false positive/negative

### PCA:
1. Standardisasi data dulu
2. Target explained variance ≥90%
3. Plot variance untuk pilih n_components
4. Jangan paksa interpretasi PC

---

## 🚀 Next Steps

1. **Praktek, praktek, praktek!**
   - Coba K-Means
   - Coba anomaly detection
   - Coba PCA

2. **Eksperimen:**
   - Bandingkan algoritma
   - Tune hyperparameters
   - Cek hasil visualisasi

3. **Deep Learning (Next Session):**
   - Jumat bersama Mas Evi
   - Get ready! 🔥

---

## 📚 Resources

**Scikit-Learn Documentation:**
- Clustering: `sklearn.cluster`
- Anomaly Detection: `sklearn.covariance`, `sklearn.ensemble.IsolationForest`
- Dimensionality Reduction: `sklearn.decomposition`

**Dataset Practice:**
- UCI Machine Learning Repository
- Kaggle Datasets
- Scikit-learn built-in datasets

**Film Recommendation:**
- **The Great Hack** (Netflix) - Cambridge Analytica scandal
- Wajib tonton untuk data scientist! 🎬

---

## ❓ FAQ

**Q: PCA untuk Deep Learning?**
A: Jarang. Deep Learning biasanya pakai teknik lain (Dropout, Batch Normalization).

**Q: Berapa fitur banyak untuk PCA?**
A: Case by case. Kalau proses lama atau >100 fitur, pertimbangkan PCA.

**Q: Bisa kontrol fitur mana yang di-PCA?**
A: Bisa! Pilih fitur yang mau di-PCA, sisanya gabung kemudian.

**Q: K-Means vs DBSCAN?**
A: K-Means = cepat, perlu K. DBSCAN = lambat, auto K, deteksi noise.

**Q: Correlation vs Covariance?**
A: Correlation = normalized (-1 to 1). Covariance = unnormalized (bisa apa saja).

---

## 🎉 Penutup

**Ingat:**
- Unsupervised learning = cari pola tanpa label
- Clustering = kelompokkan data mirip
- Anomaly detection = deteksi data aneh
- PCA = reduksi dimensi, jaga informasi

**Semangat belajar!** 🚀

Sampai jumpa di Deep Learning! 👋