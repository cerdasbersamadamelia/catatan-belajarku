# Supervised Learning - Regression 🚀

---

## 📌 Quick Recap: Machine Learning

Machine learning itu cabang dari AI yang belajar **meniru cara manusia berpikir**. Sama kayak manusia:
- Semakin banyak belajar → semakin pintar
- Semakin banyak data → semakin jago solve masalah

**Rumus sederhana:** Machine learning dengan 10 data VS 1000 data = beda jauh kemampuannya!

---

## 🎯 Tipe-Tipe Machine Learning

### 1. **Supervised Learning** ⭐
- Belajar pakai **label** (data Y)
- Ada "guru" yang ngasih tau jawaban
- Contoh: Prediksi harga rumah, klasifikasi email spam

### 2. **Unsupervised Learning**
- Belajar **tanpa label**
- Mesin cari pola sendiri
- Contoh: Clustering pelanggan

### 3. **Reinforcement Learning** 🎮
- Belajar dari **reward & punishment**
- Contoh: AI main game Dota 2, Tesla self-driving car
- Mario belajar melompati rintangan → dapat reward
- Mario jatuh ke lubang → dapat punishment

### 4. **Recommender System**
- Rekomendasi YouTube, Netflix, e-commerce
- Bisa supervised atau unsupervised

---

## 🌳 Pohon Keluarga Supervised Learning

```
AI
└── Machine Learning
    └── Supervised Learning
        ├── Regression (angka kontinyu)
        └── Classification (kategori)
```

### Perbedaan Regression vs Classification

| **Regression** | **Classification** |
|----------------|-------------------|
| Prediksi **angka** | Prediksi **kategori** |
| Harga rumah, suhu, saham | Spam/bukan spam, panas/dingin |
| Output: 100, 250.5, 37°C | Output: Ya/Tidak, A/B/C |

**Contoh Kasus Ramalan Cuaca:**
- Prediksi **hujan/berawan/petir** → Classification ✅
- Prediksi **temperatur 27°C** → Regression ✅
- Prediksi **kecepatan angin 15 km/jam** → Regression ✅

---

## 📊 Linear Regression - The Fundamental

### Konsep Dasar
Linear regression = **menarik garis lurus** yang paling cocok dengan data kita!

**Tujuan utama:** Cari nilai **W (koefisien)** dan **B (intercept)**

### Rumus Magic ✨
```
y = W×X + B
```

- **y** = target/label (yang mau diprediksi)
- **W** = koefisien (gradien garis)
- **X** = feature/independent variable
- **B** = intercept (supaya nilai gak selalu mulai dari 0)

**Analogi intercept:** Bayi baru lahir beratnya gak 0 kg kan? Nah, B ini gunanya!

---

## 🔢 Cara Hitung Manual Linear Regression

Punya data **Area Rumah (X)** dan **Price (Y)**:

### Step 1: Hitung komponen-komponen
- X×Y (X dikali Y)
- X² (X kuadrat)
- Y² (Y kuadrat)
- ΣX, ΣY, ΣXY, ΣX², ΣY² (jumlah semua)

### Step 2: Hitung B (intercept)
```
B = (ΣY × ΣX²) - (ΣX × ΣXY)
    ─────────────────────────
    (n × ΣX²) - (ΣX)²
```

### Step 3: Hitung W (koefisien)
```
W = (n × ΣXY) - (ΣX × ΣY)
    ─────────────────────
    (n × ΣX²) - (ΣX)²
```

**Penting:** 
- ΣX² ≠ (ΣX)²
- ΣX² = jumlahkan X yang udah dikuadratkan
- (ΣX)² = jumlahkan X dulu, baru dikuadratkan

### Step 4: Prediksi!
Misal dapat W=10, B=100
```
y = 10X + 100

Luas rumah 150 m² → y = 10(150) + 100 = 1600 juta
```

---

## 🐍 Linear Regression dengan Python

### Cara Mudah Pakai Scikit-Learn

```python
from sklearn.linear_model import LinearRegression

# Siapkan data
X_train = [[10], [20], [30], [40]]  # harus 2D array!
y_train = [200, 400, 600, 800]

# Bikin & train model
model = LinearRegression()
model.fit(X_train, y_train)  # Training model

# Lihat hasil
W = model.coef_        # Koefisien
B = model.intercept_   # Intercept

# Prediksi
y_pred = model.predict([[50]])  # Prediksi untuk X=50
```

**Tips:** Jangan panggil `model.coef_` sebelum `model.fit()` → bakal error!

---

## 📈 Multiple Linear Regression

Kalau sebelumnya cuma 1 feature, sekarang banyak feature!

### Formula
```
y = W₁X₁ + W₂X₂ + W₃X₃ + ... + WₙXₙ + B
```

**Contoh:** Prediksi harga rumah dari:
- X₁ = Luas
- X₂ = Jumlah kamar
- X₃ = Jarak ke pusat kota
- X₄ = Umur bangunan

### Cara Singkat: Dot Product!
```
y = W⃗ · X⃗ + B
```

**Dot product = perkalian vektor:**
```python
# Contoh
W = [2, 7, 1]
X = [8, 2, 8]

Hasil = (2×8) + (7×2) + (1×8) = 16 + 14 + 8 = 38
```

---

## 🎯 Loss Function - Ngukur Seberapa Bagus Model Kita

Loss function = **ujian buat model** → ngecek seberapa akurat prediksinya!

### Cara Kerja
1. Model prediksi harga rumah: **$1450**
2. Harga aktual: **$1500**
3. Error = |1500 - 1450| = **$50**

### Jenis-Jenis Loss Function

#### 1. **SSE (Sum of Squared Errors)**
```
SSE = Σ(y_aktual - y_prediksi)²
```
- Dijumlahkan langsung
- Belum rata-rata

#### 2. **MSE (Mean Squared Error)** ⭐ Paling populer
```
MSE = (1/n) × Σ(y_aktual - y_prediksi)²
```
- Ada rata-rata → lebih adil
- Satuan: USD²

#### 3. **MAE (Mean Absolute Error)**
```
MAE = (1/n) × Σ|y_aktual - y_prediksi|
```
- Pakai nilai absolut
- Satuan: USD (sama kayak data asli)

#### 4. **RMSE (Root Mean Squared Error)**
```
RMSE = √MSE
```
- MSE di-akar-kan
- Satuan: USD (sama kayak data asli)

### Kenapa Pakai Kuadrat? 🤔

```
Data:
Aktual: [100, 110, 80]
Prediksi: [110, 90, 50]

Selisih: [10, -20, -30]

Tanpa kuadrat → 10 + (-20) + (-30) = -40 ❌ (saling hapus!)
Pakai kuadrat → 100 + 400 + 900 = 1400 ✅
```

**Kesimpulan:** Kuadrat supaya nilai positif & negatif gak saling hapus!

### Kapan Pakai Yang Mana?

| **Metric** | **Kapan Dipakai** |
|-----------|------------------|
| MSE | Pengin kasih **penalti besar** ke error gede |
| MAE | Data ada **outlier** banyak |
| RMSE | Pengin satuan sama kayak data asli |

---

## 🔧 Feature Engineering - Bikin Model Makin Pinter!

### Apa itu Feature Engineering?
**Merekayasa fitur** supaya model belajar lebih banyak!

### Contoh Sederhana
Punya data:
- Panjang rumah: 10m
- Lebar rumah: 8m

**Feature baru:** Luas = Panjang × Lebar = 80m²

### Kenapa Penting?
✅ Model jadi lebih akurat (dapat info lebih banyak)  
✅ Bisa tambah fitur: jarak ke rumah sakit, lokasi, fasilitas

**Tapi...** Butuh **domain expertise** (paham bisnis/konteks data)

---

## 📏 Feature Scaling - Samakan Satuan!

### Kenapa Perlu?
Data kita beda-beda satuan:
- Harga: jutaan rupiah
- Luas: puluhan meter
- Jarak: ratusan km

**Dampak:** Model jadi bingung! Algoritma berbasis jarak (KNN, SVM, Neural Network) jadi gak akurat.

### Normalization vs Standardization

#### **Normalization (Min-Max Scaler)**
```
X_norm = (X - X_min) / (X_max - X_min)
```
- **Range:** -1 sampai 1
- **Tujuan:** Pusatkan di angka 0
- **Kapan:** Data gak terdistribusi normal

#### **Standardization (Standard Scaler)** ⭐
```
X_std = (X - mean) / std_dev
```
- **Hasil:** Mean=0, Std=1
- **Kapan:** Data terdistribusi normal
- **Lebih robust** terhadap outlier

### Untuk Data Gambar 📷
```python
# Super simpel!
X_normalized = X / 255  # Pixel value: 0-255
```

**Tips:** Kalau bingung, **coba keduanya** dan lihat mana yang lebih bagus!

---

## 🌀 Polynomial Regression - Untuk Data Melengkung

### Kapan Dipakai?
Kalau data kamu **gak lurus**, tapi **melengkung** (non-linear)!

### Bedanya dengan Linear?
- **Linear:** y = W×X + B (pangkat 1)
- **Polynomial:** y = W₂X² + W₁X + B (pangkat 2 atau lebih)

### Cara Pakai di Python
```python
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LinearRegression

# Step 1: Transform jadi polynomial
poly = PolynomialFeatures(degree=2)
X_poly = poly.fit_transform(X)

# Step 2: Train seperti biasa
model = LinearRegression()
model.fit(X_poly, y)
```

**Key point:** Tambah 1 step → transform X jadi polynomial dulu!

---

## 🛠️ Tips & Tricks

### Reshape di NumPy
```python
data = [1, 2, 3, 4, 5, 6]

# Jadi 2 baris, 3 kolom
data.reshape(2, 3)  → [[1,2,3], [4,5,6]]

# Jadi 3 baris, 2 kolom
data.reshape(3, 2)  → [[1,2], [3,4], [5,6]]

# -1 = auto calculate (cari aman!)
data.reshape(-1, 1)  → Maximize baris
```

**Reshape -1 = pakai semua resource yang ada!**

### Best Practice: Dokumentasi Function
```python
def my_function(x, y):
    """
    Penjelasan function
    
    Parameters:
    -----------
    x : tipe data
        Penjelasan parameter x
    y : tipe data
        Penjelasan parameter y
    
    Returns:
    --------
    hasil : tipe data
        Penjelasan output
    """
    return x + y
```

---

## 📝 Istilah Penting

| **Istilah** | **Artinya** |
|------------|-----------|
| Feature | Kolom/variabel input (X) |
| Label/Target | Yang mau diprediksi (y) |
| Independent Variable | Feature (X) |
| Dependent Variable | Label (y) |
| Coefficient | Nilai W (gradien) |
| Intercept | Nilai B (konstanta) |
| Training | Model belajar dari data |
| Prediction | Model kasih jawaban |

---

## 🎓 Kesimpulan

1. **Linear Regression** → garis lurus untuk prediksi angka
2. **W & B** → yang kita cari dengan rumus atau scikit-learn
3. **Loss Function** → ukur seberapa akurat model
4. **Feature Engineering** → bikin fitur baru supaya model makin pinter
5. **Feature Scaling** → samakan satuan supaya fair
6. **Polynomial** → kalau data melengkung, bukan lurus

---

## 🚀 Next: Pertemuan Selanjutnya

1. **Logistic Regression** → Regression tapi buat Classification!
2. **Decision Tree** → Pohon keputusan