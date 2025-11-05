# Machine Learning - Pertemuan 9

---

## 📌 Apa Itu Machine Learning?

### Hierarki AI dan Machine Learning
**AI (Artificial Intelligence)** adalah ilmu yang meniru cara kerja manusia menggunakan program/software. AI punya banyak cabang:

1. **Robotics** → Meniru gerakan manusia
2. **Computer Vision** → Meniru cara manusia melihat gambar/video
3. **Speech Recognition** → Meniru cara manusia berbicara/mendengar
4. **Natural Language Processing (NLP)** → Meniru cara manusia memproses bahasa
5. **Machine Learning** → Meniru cara manusia belajar dan berpikir

**Machine Learning** adalah bagian dari AI yang fokus pada bagaimana mesin belajar dari data.

**Deep Learning** adalah cabang spesifik dari Machine Learning yang menggunakan sistem saraf tiruan (neural network).

---

## 🔄 Classical Programming vs Machine Learning

### Classical Programming
- **Input:** Rules (aturan) + Data
- **Output:** Answers (jawaban)
- **Cara kerja:** Kita bikin aturan manual (if-else)
- **Kelemahan:** Kalau rules makin kompleks, harus tambah manual terus

**Contoh:**
```
IF batuk THEN sakit
ELSE sehat

IF sakit THEN ke dokter
```

### Machine Learning
- **Input:** Data + Answers (jawaban)
- **Output:** Rules (pola/model)
- **Cara kerja:** Mesin belajar sendiri dari data
- **Kelemahan:** Butuh data lengkap dengan jawabannya

**Analogi Sederhana:**
Seperti anak kecil belajar mengenal dinosaurus. Kita kasih gambar T-Rex dan bilang "ini T-Rex", kasih gambar Triceratops dan bilang "ini Triceratops". Nanti dia bisa bedain sendiri kalau lihat gambar baru!

---

## 📊 Tahapan Machine Learning

### 1. Training Stage
- Melatih model dengan dataset
- Model belajar mengenali pola dari data
- Seperti kita ngajarin ponakan gambar-gambar dinosaurus

### 2. Inference/Prediction Stage
- Model yang sudah jadi dipakai untuk prediksi
- Bisa dideploy ke server untuk dipakai user
- Seperti ngetes ponakan: "Gambar apa ini?" → "T-Rex, Om!"

**Contoh Aplikasi:**
- **Roboguru:** Sistem sudah di-deploy, user bisa tanya soal, AI jawab
- **Glassdoor.com:** Prediksi salary berdasarkan data yang kita masukkan

---

## 🎯 Jenis-Jenis Machine Learning

### 1. Supervised Learning ✅
**Ciri utama:** Ada data (X) DAN ada jawaban/label (y)

**Contoh:**
- Data Titanic: Ada data penumpang + label "survive atau tidak"
- Email: Ada konten email + label "spam atau bukan spam"

**Tipe Supervised Learning:**

#### a) Klasifikasi
- **Output:** Kategori (0/1, panas/dingin, hidup/mati)
- **Contoh algoritma:** Decision Tree, Naive Bayes, SVM
- **Contoh kasus:** Deteksi spam, klasifikasi gambar angka

#### b) Regresi
- **Output:** Angka kontinyu (harga, berat, suhu)
- **Contoh algoritma:** Linear Regression
- **Contoh kasus:** Prediksi harga rumah, prediksi berat badan dari tinggi

---

### 2. Unsupervised Learning ⚪
**Ciri utama:** Cuma ada data (X), TIDAK ada label (y)

Model belajar dari hubungan antar fitur saja.

**Tipe Unsupervised Learning:**

#### a) Clustering
- **Fungsi:** Mengelompokkan data berdasarkan kemiripan
- **Contoh algoritma:** K-Means
- **Contoh kasus:** 
  - Mengelompokkan penduduk Indonesia berdasarkan umur (Gen Z, Milenial, Boomer)
  - Segmentasi pelanggan untuk strategi marketing

#### b) Association
- Menemukan hubungan antar item

#### c) Dimensionality Reduction
- Mengurangi jumlah fitur data

---

### 3. Reinforcement Learning 🎮
**Ciri utama:** Ada agen yang belajar dengan sistem **reward & punishment**

**Cara kerja:**
- Agen ditempatkan di environment (misal: labirin)
- Kalau jalan benar → dapat reward (keju)
- Kalau jalan salah → dapat punishment (pengurangan poin)
- Tujuan: Maksimalkan reward

**Contoh:**
- OpenAI Five mengalahkan juara dunia Dota 2
- Game AI yang belajar main game sendiri

---

## 🛠️ Tools dan Library

### Scikit-learn (sklearn)
Library Python untuk Machine Learning, sama seperti Pandas dan NumPy untuk data science.

**Contoh penggunaan:**
```python
from sklearn.tree import DecisionTreeClassifier

# 1. Define model
model = DecisionTreeClassifier()

# 2. Training model
model.fit(X, y)  # X = features, y = target

# 3. Prediksi
hasil = model.predict([[data_baru]])
```

---

## 📝 Contoh Praktis: Decision Tree Classifier

### Case: Prediksi Survival Titanic

**Data:**
- **Features (X):** Pclass, Sex, Age, SibSp, Parch, Fare (7 kolom)
- **Target (y):** Survived (0 = tidak survive, 1 = survive)

**Proses:**
1. Pisahkan data jadi X dan y
2. Training model dengan `model.fit(X, y)`
3. Visualisasi pohon keputusan
4. Deploy model
5. Prediksi data baru: `model.predict([[3, 1, 0, 35, 1, 0, 52]])`
   - Output: 1 (diprediksi survive)

---

## 🔍 Features, Target, dan Notasi

### X (Features/Data/Input)
- Kolom-kolom yang dipakai untuk belajar
- Disebut juga: features, dimensions, columns
- Contoh: tinggi, berat, umur, jenis kelamin

### y (Target/Label/Output)
- Jawaban yang ingin diprediksi
- Disebut juga: target, label, answer
- Contoh: survive/tidak, spam/bukan, harga rumah

**Catatan Penting:**
- Machine Learning itu **spesifik untuk 1 task**
- Model deteksi sentimen Twitter TIDAK bisa translate bahasa
- Model klasifikasi gambar anjing/kucing TIDAK bisa deteksi mobil
- Seperti GPT awal yang tidak bisa bahasa Indonesia karena tidak dilatih dengan data Indonesia

---

## 📈 Training Dataset

### Berapa Kali Training?
**Training dilakukan SEKALI saja** → menghasilkan Model V1

**Kapan perlu Retraining?**
- Setelah beberapa waktu, performa model turun
- Ada **data drift** (data baru yang berbeda dari data training)
- Contoh: Model deteksi penyakit sebelum COVID tidak bisa deteksi COVID

**Contoh:**
- ChatGPT-4 ditraining sampai September 2021
- GPT-4 tidak tahu event setelah September 2021

---

## ⚠️ Problem: Overfitting & Underfitting

### Overfitting 🔴
- Model **terlalu ngepas** dengan data training
- Bagus di training, **jelek di testing**
- Seperti hafalan mati - di ulangan pakai soal berbeda langsung bingung

### Good Fit ✅ (Yang Kita Inginkan)
- Model bisa **generalize** dengan baik
- Performa training dan testing seimbang
- Model belajar polanya, bukan hafalannya

### Underfitting 🔵
- Model **jelek di training**, pasti jelek juga di testing
- Model belum belajar dengan baik

---

## 📊 Cara Cek Overfitting

### Metode 1: Visual
Lihat grafik performa training vs validation

### Metode 2: Split Data (PALING UMUM)
**Bagi data menjadi:**

1. **Training Set (70-80%)**
   - Untuk melatih model
   - Contoh: 800 data dari 1000 total

2. **Testing Set (10-15%)**
   - Untuk cek performa akhir
   - Contoh: 100 data

3. **Validation Set (10-15%)**
   - Untuk tuning hyperparameter
   - Contoh: 100 data

**Indikasi Overfitting:**
- Training accuracy: 90%
- Testing accuracy: 70%
- **Selisih terlalu besar!** ❌

**Model Bagus:**
- Training accuracy: 81%
- Testing accuracy: 76%
- Selisih kecil ✅

**Underfitting:**
- Training accuracy: 55%
- Testing accuracy: 50%
- Keduanya jelek ❌

---

## 🎲 Cross Validation

Metode lebih kompleks untuk evaluasi model.

### K-Fold Cross Validation
Contoh: **5-Fold Cross Validation**

1. Data dibagi jadi 5 bagian (fold)
2. Setiap fold bergantian jadi testing
3. 5 kali training dengan kombinasi berbeda
4. Hasil akhir di-**rata-rata** atau di-voting

**Keuntungan:**
- Lebih robust (kuat)
- Hasil lebih reliable
- Data dipakai lebih efisien

**Contoh:**
- Fold 1 → testing, Fold 2-5 → training (Model 1)
- Fold 2 → testing, Fold 1,3,4,5 → training (Model 2)
- Dan seterusnya...
- Hasil 5 model di-rata-rata

---

## 📊 Evaluasi Model

### Untuk REGRESI

#### 1. MAE (Mean Absolute Error)
- Rata-rata error absolut
- Rumus: |y_aktual - y_prediksi|
- **Kekurangan:** Tidak beri penalti besar untuk error besar

#### 2. MSE (Mean Squared Error)
- Rata-rata error dikuadratkan
- Rumus: (y_aktual - y_prediksi)²
- **Kekurangan:** Satuan jadi pangkat 2 (meter → meter²)

#### 3. RMSE (Root Mean Squared Error) ⭐ PALING SERING DIPAKAI
- Akar dari MSE
- Rumus: √MSE
- **Keuntungan:**
  - Satuan sama dengan data asli
  - Beri penalti besar untuk error besar
  - Mudah dibandingkan dengan data aktual

**Contoh:**
- Rata-rata harga rumah: 500 juta
- **RMSE 500 juta** → JELEK! (Error = rata-rata)
- **RMSE 50 juta** → BAGUS! (Error cuma 10%)

**Rule of Thumb:**
- RMSE **mendekati 0** → Model PERFECT ✅
- RMSE **> rata-rata data** → Model JELEK ❌

#### 4. MAPE (Mean Absolute Percentage Error)
- Error dalam bentuk persentase

#### 5. R-squared
- Mengukur seberapa baik model menjelaskan variasi data

---

### Untuk KLASIFIKASI

#### Confusion Matrix
**Tabel 2x2** yang menunjukkan hasil prediksi vs aktual:

```
                    Predicted
                 Not Spam  |  Spam
        ----------------------
Not Spam |   TN      |   FP
Actual   |           |
Spam     |   FN      |   TP
```

**Istilah:**
- **TP (True Positive):** Prediksi spam, aktual spam ✅
- **TN (True Negative):** Prediksi not spam, aktual not spam ✅
- **FP (False Positive):** Prediksi spam, tapi aktual not spam ❌
- **FN (False Negative):** Prediksi not spam, tapi aktual spam ❌

---

#### 1. Accuracy (Akurasi)
**Rumus:**
```
Accuracy = (TP + TN) / (TP + TN + FP + FN)
```

**Kapan pakai:** Data **balanced** (proporsi label seimbang)

**Kapan TIDAK pakai:** Data **imbalanced**

**Contoh Imbalanced:**
- Label 0: 80%
- Label 1: 20%
- Kalau model asal jawab "0" semua → accuracy 80%, tapi model jelek!

---

#### 2. Precision
**Rumus:**
```
Precision = TP / (TP + FP)
```

**Artinya:** Dari semua yang diprediksi positif, berapa yang benar?

**Kapan penting:**
- Spam detection (jangan sampai email penting masuk spam)
- Medical diagnosis (jangan diagnosa sakit kalau sehat)

---

#### 3. Recall
**Rumus:**
```
Recall = TP / (TP + FN)
```

**Artinya:** Dari semua yang sebenarnya positif, berapa yang berhasil ditangkap?

**Kapan penting:**
- Deteksi kanker (jangan sampai ada yang terlewat)
- Fraud detection (jangan sampai ada penipuan yang lolos)

---

#### 4. F1-Score
**Gabungan Precision dan Recall**

Kalau Precision dan Recall bagus → F1-Score juga bagus

---

## 🔧 Mengatasi Imbalanced Dataset

### Teknik-teknik:

1. **Oversampling**
   - Tambah jumlah data minoritas
   
2. **Undersampling**
   - Kurangi jumlah data mayoritas

3. **SMOTE**
   - Synthetic Minority Over-sampling Technique

4. **Stratified Split**
   - Pastikan setiap kategori ada di training dan testing

**Library:** `imbalanced-learn` (imblearn)

---

## 🎲 Random State

### Apa itu Random State?
Parameter untuk **mengatur keacakan** agar hasil **konsisten**.

**Kenapa perlu?**
- Machine Learning punya proses random (probability, sampling, dll)
- Tanpa random state → hasil beda-beda setiap run
- Dengan random state → hasil **reproducible**

**Contoh:**
```python
# Tanpa random state
model = DecisionTreeClassifier()  # Hasil beda-beda

# Dengan random state
model = DecisionTreeClassifier(random_state=42)  # Hasil sama terus
```

**Manfaat:**
- Eksperimen bisa diulang
- Orang lain bisa dapat hasil yang sama
- Debugging lebih mudah

---

## 📐 Matematika Dasar: Linear Equation

### Persamaan Linear
**Ciri:** Pangkat variabel = 1

**Bentuk umum:** `y = ax + b`
- `a` = slope/koefisien
- `b` = intercept

**Contoh:**
- `y = x` → Garis diagonal 45°
- `y = 2x` → Garis lebih curam
- `y = 0.5x` → Garis lebih landai
- `y = 2x + 1` → Garis naik, mulai dari y=1

### Persamaan Non-Linear
**Ciri:** Ada pangkat > 1

**Contoh:**
- `y = x² + 1` → Parabola (kurva)
- `y = x³` → Kurva kubik

**Aplikasi:**
- Linear Regression pakai persamaan linear
- Polynomial Regression pakai persamaan non-linear

---

## 🔢 Contoh: Linear Regression

### Case: Prediksi Berat dari Tinggi

**Data:**
- X = Height (tinggi)
- y = Weight (berat)

**Model:**
```python
from sklearn.linear_model import LinearRegression

model = LinearRegression()
model.fit(X, y)

# Hasil model
# Koefisien: 64.73
# Intercept: -164

# Persamaan: y = 64.73x - 164
```

**Prediksi:**
```python
# Tinggi 175 cm → Berat berapa?
model.predict([[175]])
# Output: 64 kg
```

**Cara manual:**
```
y = 64.73 × 175 - 164
y = 64 kg
```

---

## 💡 Tips & Best Practices

### 1. Dataset Split
- **70-80%** untuk training
- **10-15%** untuk validation
- **10-15%** untuk testing

### 2. Stratified Split
Untuk **imbalanced data**, pastikan semua kategori ada di setiap set.

### 3. Pilih Metrik yang Tepat
- **Regression:** RMSE (paling umum)
- **Balanced Classification:** Accuracy
- **Imbalanced Classification:** Precision, Recall, F1

### 4. Cek Overfitting
Selalu bandingkan performa training vs testing!

### 5. Set Random State
Untuk reproducibility, selalu set `random_state=42` (atau angka lain).

---

## 📚 Istilah Penting

| Istilah | Arti |
|---------|------|
| **Features** | Data input (X) |
| **Target/Label** | Output yang diprediksi (y) |
| **Training** | Proses melatih model |
| **Inference** | Proses prediksi dengan model jadi |
| **Overfitting** | Model terlalu ngepas dengan training data |
| **Underfitting** | Model belum belajar dengan baik |
| **Hyperparameter** | Pengaturan model yang kita set manual |
| **API** | Application Programming Interface (jembatan komunikasi antar program) |
| **Endpoint** | URL/alamat untuk akses API |

---

## 🔗 Resources

### Library Python
- **Scikit-learn:** Machine Learning algorithms
- **Pandas:** Data manipulation
- **NumPy:** Numerical computing
- **Matplotlib/Seaborn:** Visualization
- **Imbalanced-learn:** Handle imbalanced data
- **Streamlit:** Deploy ML model

### Contoh Aplikasi
- Glassdoor salary prediction
- Roboguru question answering
- Email spam detection
- Image classification
- Sentiment analysis Twitter

---

## 🎯 Rangkuman

1. **Machine Learning** adalah cabang AI yang meniru cara manusia belajar
2. Bedanya dengan **Classical Programming**: ML belajar dari data, bukan rules manual
3. Ada **3 jenis utama**: Supervised, Unsupervised, Reinforcement Learning
4. **Supervised Learning** punya 2 tipe: **Klasifikasi** (output kategori) dan **Regresi** (output angka)
5. **Problem utama**: Overfitting dan Underfitting
6. **Solusi**: Split data training/testing, cross validation
7. **Evaluasi** berbeda untuk regresi (RMSE) dan klasifikasi (Accuracy, Precision, Recall)
8. Untuk **imbalanced data**, jangan pakai accuracy!!
9. **Scikit-learn** adalah library utama untuk ML di Python
10. Setiap model ML itu **spesifik untuk 1 task** saja

---

## ✅ Next Steps

Di pertemuan selanjutnya akan dibahas:
- **Logistic Regression** (Senin depan)
- Detail algoritma-algoritma populer
- Natural Language Processing
- Speech Recognition
- Computer Vision