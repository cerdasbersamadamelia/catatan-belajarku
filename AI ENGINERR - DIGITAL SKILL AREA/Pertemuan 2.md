# Pertemuan 2 - Tools & Konsep Machine Learning

## 🛠️ Tools AI/ML Engineer

### 1. Data Processing Tools

#### **Pandas** 
Tool paling penting dan sering dipakai! Wajib dikuasai kalau mau terjun ke dunia data.

**Fungsi:**
- Membersihkan data
- Memproses data
- Mengekstrak data
- Join dan menggabungkan data (misal: CSV + database)

**Fitur Utama:**
- Menampilkan data teratas
- Memilih kolom tertentu
- Cek jumlah data (baris)
- Filter data (mirip SQL)
- Statistical analysis: `mean`, `median`, `describe`, `min`, `max`
- Aggregation untuk otomatisasi analisis

**Belajar:** pandas.pydata.org → Getting Started

---

#### **Spark (PySpark)**
Big data tools! Untuk data ukuran giga sampai tera.

**Keunggulan:**
- ⚡ **7x lebih cepat** dari Pandas
- Mendukung 3 bahasa: Python, Java, Scala (yang populer: Python)
- Cocok untuk big data processing
- Bisa untuk ML modeling juga

**Cara Kerja:**
- **Pandas:** Kerja solo, ngerjain sendiri → lambat
- **Spark:** Membagi tugas ke worker 1, 2, 3, dst → cepat

**Kapan Pakai:**
- Data ukuran besar (giga/tera) → Spark
- Data biasa → Pandas aja

**Install di Colab:**
```python
pip install pyspark==3.4.3  # versi stabil
```

**Kelebihan Spark:**
- ✅ Performa super cepat
- ✅ Mendukung ML modeling
- ❌ Tampilan kurang bagus
- ❌ Syntax agak ribet

---

#### **Polars**
Alternatif modern!

**Keunggulan:**
- **20x lebih cepat** dari Pandas!
- Syntax mirip Pandas

**Kelemahan:**
- Cara penggunaan sedikit kompleks
- Beberapa fitur processing tidak ada (terutama text processing)
- Agak inkonsisten

**Kapan Pakai:**
- Butuh kecepatan tapi tetap praktis → Polars
- Butuh ML modeling terintegrasi → Spark
- Belajar dasar → Pandas

---

### 2. Database

#### **MySQL**
Database untuk pemula! Instalasi mudah.

**Tools:** MySQL Workbench

---

#### **PostgreSQL** 
Database paling sering dipakai di dunia kerja!

**Instalasi:**
1. Download PostgreSQL dari EDB
2. Install PG Admin (sebagai UI)

Lebih kompleks dari MySQL tapi lebih powerful.

---

#### **Oracle & Microsoft SQL**
Biasa dipakai di perusahaan corporate dengan sistem hierarki kuno.

---

### 3. Data Visualization

#### **Matplotlib & Seaborn**
Pakai dua-duanya! Tapi Seaborn lebih utama.

**Kenapa Seaborn?**
- Visualisasi lebih estetik dan modern
- Matplotlib terkesan lebih kuno

**Visualisasi Penting:**

1. **Box Plot** - Untuk deteksi outlier
   - **Outlier:** Nilai pencilan yang terlalu jauh dari median
   - **Median (garis kuning):** Nilai tengah
   - **Q1:** 25% data pertama (nilai kecil)
   - **Q3:** 75% data pertama (nilai besar)

2. **Scatter Plot** - Lihat hubungan antar variabel

3. **Heat Map** - Lihat korelasi antar fitur
   - Apakah berbanding lurus atau terbalik

**Belajar:** seaborn.pydata.org → Gallery

---

#### **Plotly**
Untuk visualisasi interaktif (opsional).

---

### 4. Machine Learning

#### **Traditional ML: Scikit-Learn**
Framework ML tradisional, bagus untuk pemula!

**Fitur:**
- Classification
- Regression  
- Clustering
- Dimensionality Reduction (PCA)

**Contoh penggunaan:**
```python
from sklearn import svm

# Define model
clf = svm.SVC()

# Training
clf.fit(X, y)  # X = fitur, y = target
```

---

#### **Modern ML: TensorFlow & PyTorch**

**TensorFlow:**
- ✅ Dokumentasi bagus dan jelas
- ✅ Cocok untuk pemula
- ✅ Tutorial lengkap
- Gunakan ini kalau baru belajar Deep Learning!

**PyTorch:**
- ✅ 90% lowongan kerja butuh PyTorch!
- ✅ Lebih populer di industri
- ❌ Dokumentasi kurang friendly untuk pemula
- ❌ Instalasi kadang bermasalah
- Gunakan ini kalau mau cepat cari kerja!

**Kesimpulan:**
- Pemula → TensorFlow dulu
- Cari kerja cepat → PyTorch

---

## 📊 Dimensionality Reduction: PCA

**Konsep:**
Mengurangi jumlah fitur untuk memperkecil ukuran dataset.

**Contoh:**
- Punya 7 fitur → setelah PCA jadi 4 fitur
- Data tetap informatif tapi lebih efisien

---

## 🎯 Model Selection

### Resampling vs Probabilistic

**Probabilistic Techniques:**
Untuk model statistical seperti:
- Naive Bayes (berdasarkan probabilitas)
- Linear Regression
- Logistic Regression
- Polynomial Regression

**Resampling Techniques:**
Untuk model machine learning berbasis non-statistical.

**Penting:** Resampling juga cocok untuk model statistical! Tapi probabilistic **tidak cocok** untuk model ML biasa.

---

## 📂 Train-Test Split

### Konsep Dasar

**Dataset dibagi:**
1. **X (Fitur):** Data yang mau kita training
2. **y (Target):** Label/hasil yang mau diprediksi

**Split menjadi:**
- **X_train, y_train:** Untuk model belajar
- **X_test, y_test:** Untuk mengetes akurasi model

---

### Proporsi Pembagian

**Standar:**
- Training: **80%**
- Testing: **20%**

**Dengan Validation:**
- Training: **80%**
- Validation: **10%**
- Testing: **10%**

**Alternatif:**
- 70% - 30%
- 90% - 10% (kalau dataset besar)

**Note:** Validation tidak wajib! Train-test aja sudah cukup.

---

## ✨ Cross Validation

**Konsep:**
Membagi data jadi beberapa bagian (fold), lalu testing secara bergantian.

**Contoh Split 5 Fold:**

**Fold 1:**
- Test: Data 0-20%
- Train: Data 21-100%

**Fold 2:**
- Test: Data 21-40%
- Train: Data 0-20% + 41-100%

**Fold 3:**
- Test: Data 41-60%
- Train: Data 0-40% + 61-100%

Dan seterusnya...

**Hasil Akhir:** Rata-rata akurasi dari semua fold.

---

## 🎨 Feature Selection

Jarang dipakai untuk AI/ML Engineer, lebih ke Data Scientist/Analyst.

### Filter-Based Methods

| Input | Output | Method |
|-------|--------|--------|
| Categorical | Numerical | **ANOVA** (paling sering) / Kendal |
| Categorical | Categorical | **Chi-Square** (paling sering) / Mutual Information |
| Numerical | Numerical | **Pearson** / Spearman (sama-sama populer) |

**Yang paling sering:** Chi-Square, ANOVA, Pearson

---

## 📈 Evaluation Metrics

### Classification

#### **1. Accuracy**
Jarang dipakai! Biasanya untuk deep learning/computer vision.

#### **2. Recall** ⭐
**"Manajemen Risiko - Jaga-jaga anggap jelek"**

**Kapan pakai:**
- **Fraud Detection:** Jaga-jaga nasabah yang mau pinjam 100 miliar
- Situasi high-risk yang harus hati-hati

#### **3. Precision** ⭐
**"Menghindari biaya lebih dalam menangani risiko"**

**Kapan pakai:**
- **Spam Detection:** Email bukan spam tapi masuk spam → user kehilangan email penting
- **Chat Censor (Mobile Legends):** Kata "anjing" atau "babi" (bukan makian) kena sensor → player kesal padahal gak toxic

#### **4. F1-Score** ⭐⭐⭐
**"Penyeimbang antara Precision dan Recall"**

Bingung mau pakai Recall atau Precision? **Pakai F1-Score!**

---

### Confusion Matrix

**Yang Benar:**
- True Positive (TP): Positif, diprediksi Positif ✅
- True Negative (TN): Negatif, diprediksi Negatif ✅

**Yang Salah:**
- False Positive (FP): Negatif tapi diprediksi Positif ❌
- False Negative (FN): Positif tapi diprediksi Negatif ❌

**Note:** Positif/Negatif bisa apa aja (kucing/anjing, spam/bukan spam, dll)

---

### Regression

#### **1. R-squared (R²)** ⭐⭐⭐
**Paling sering dipakai!** Seperti accuracy untuk regression.

**Rentang:** -1 sampai 1
- **Positif (0.7-1.0):** Bagus! ✅
- **Negatif (-1-0):** Jelek banget, model tidak layak ❌

#### **2. Mean Squared Error (MSE) / Root MSE (RMSE)**
Untuk tahu seberapa jauh hasil prediksi dengan hasil sebenarnya.

---

## 🧹 Data Cleaning: 6 Langkah Wajib!

### 1. Cek Unique Value pada String Column
- Unique value **tidak boleh banyak-banyak**
- Kecuali: Province/State (bisa disingkat jadi region)
  - Contoh: 38 provinsi → 3 region (Barat, Tengah, Timur)

### 2. Drop Kolom Identifier
Kolom dengan unique value terlalu banyak = kolom identitas.

**Contoh:**
- Nama
- ID
- Alamat
- Postal Code
- Nama Orang Tua

**Action:** Hapus semua!

### 3. Cek Missing Value

**Kapan pakai Modus:**
- Data **string** → cari nilai terbanyak

**Kapan pakai Median:**
- Data **numerical** + banyak outlier

**Kapan pakai Mean:**
- Data **numerical** + data bersih

**Kapan Drop:**
- Missing value **> 25%** → kolom tidak relevan, hapus!

### 4. Drop Kolom Missing > 25%
Kolom dengan missing value di atas 25% dianggap tidak relevan.

### 5. Labeling
Ubah semua string menjadi angka supaya bisa dibaca komputer.

### 6. Correlation Heatmap
- Korelasi tinggi (0.8 / -0.9) → **redudansi tinggi**
- **Action:** Hapus salah satu/dua kolom yang korelasinya tinggi

---

## 💼 Tips Interview

### Data Scientist / ML Engineer

**Ditanya:**
- Cara cleaning data
- Metode yang dipakai dan alasannya
- Model ML yang dipakai dan keunggulannya
- Cara kerja model (misal: Decision Tree)
- Statistika dasar

**Yang Susah:**
- Live coding langsung!

---

### AI Engineer

**Ditanya:**
- Transformers (Encoder, Decoder)
- Model LLM (OpenAI, Claude, Gemini)
- Perbandingan kelebihan antar model
- FastAPI / Docker
- Deployment ke cloud (GCP/AWS)
- Port management

**Sifat interview:** Mirip Software Engineer!

---

## 🎯 Mini Task

**Exploratory Data Analysis (EDA)**

1. Ambil dataset dari Kaggle (bebas)
2. Lakukan visualisasi dengan Seaborn
3. Fokus: Box Plot, Line Plot, Heat Map
4. Dapatkan insight dari data
5. Boleh dipamerin ke LinkedIn!

---

## 📌 Resource Penting

- **Pandas:** pandas.pydata.org
- **Seaborn:** seaborn.pydata.org
- **Scikit-Learn:** scikit-learn.org
- **TensorFlow:** tensorflow.org
- **PyTorch:** pytorch.org
- **Dataset:** kaggle.com / UCI Machine Learning Repository
- **Big Dataset (2GB+):** Kaggle dengan filter size

---

## 💡 Tips Portofolio

**Pakai dataset dummy boleh!** Asal:
- Paham konsep pengambilan insight
- Bisa jelaskan metode data cleaning
- Tahu cara baca dan ambil kesimpulan

**Nilai Plus:**
- Deploy model (backend dengan FastAPI)
- Dockerize aplikasi
- Deploy ke cloud (GCP/AWS)
- Pakai dataset real (bukan dari Kaggle)
- Scraping data sendiri (misal: scraping Tokopedia kalau mau lamar ke Tokopedia)

---

## 🎓 Next Session

**Pertemuan 3 (Minggu):**
- Machine Learning Modeling
- Classification & Regression
- Cara kerja model-model ML
- Praktik coding!