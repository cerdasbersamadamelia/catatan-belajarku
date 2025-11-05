# Deep Learning - Part 1

## 🎯 Intro: Kenapa Deep Learning Seru?

Akhirnya masuk ke materi inti! Dari sini sampai akhir kita bakal belajar **Neural Network**. Bedanya sama traditional machine learning:
- **Machine Learning tradisional**: Banyak matematika rumit (kayak SVM, PCA)
- **Deep Learning**: Konsepnya lebih simpel - cuma **perkalian** sama **activation function**

### Real World Example: Subtitle Generator

Ada kasus nyata dari Ruang Guru - butuh bikin subtitle untuk video dalam waktu 1 minggu! Kalau manual butuh **40-50 hari** (6 jam/hari).

**Percobaan 1: Google Speech-to-Text (4-5 tahun lalu)**
- Hasilnya? KOCAK! 😂
- "tekuni" → "teguni"
- "kepemimpinan" → "kememinan"  
- "mindset" → "perut" ❌
- "bisnis" → "bisa serius" ❌

**Solusi: OpenAI Whisper**
- Hasilnya jauh lebih bagus!
- Cost: Cuma Rp 800 ribu untuk semua video
- Kesalahan cuma typo kecil (masih bisa ngeles!)
- Quality check bisa lebih cepat pakai ChatGPT

**Tools yang Bisa Dicoba:**
1. **Text-to-Speech & Speech-to-Text**: OpenAI API
2. **UI Version**: Google Cloud Console
3. **Tip QC Cepat**: Copy transcript → Kasih ke LLM → Minta identifikasi kesalahan

💡 **Motto kerja:** "Be lazy - kalau bisa cepat, kenapa harus kerja keras?"

---

## 📚 Recap Machine Learning Sebelumnya

Semua model ML punya pola yang sama:
- Ada **Loss Function** (yang mau di-minimize atau maximize)
- Ada **Algorithm** untuk mencapai nilai optimal

**Contoh:**
- **Decision Tree**: Loss function = Gini Entropy
- **SVM**: Maximize margin (jarak garis ke titik-titik)

---

## 🧠 Konsep Dasar Neural Network

### Persamaan Linear Sederhana

```
y = mx + b
```

Di Machine Learning:
- **x** = input (contoh: tinggi badan)
- **y** = output (contoh: berat badan)
- **m** = slope/kemiringan
- **b** = bias/intercept

**Multiple Input:**
```
y = ax₁ + bx₂ + c
```
- x₁ = tinggi badan
- x₂ = usia
- y = berat badan

Dalam Neural Network:
- **a, b** = **weights** (bobot)
- **c** = **bias**

### Linear Regression: Cara Kerja

1. Punya data points random
2. Bikin garis lurus yang paling pas
3. Hitung **loss** = jarak antara garis prediksi vs titik sebenarnya
4. Gunakan **MSE** (Mean Squared Error): rata-rata jarak dikuadratkan
5. Loop terus sampai dapat garis terbaik!

**Proses:**
- Model nyoba-nyoba nilai **weight (w)** dan **bias (b)**
- Di setiap iterasi (epoch), loss makin turun
- Contoh: Epoch 100 → loss 0.7, Epoch 200 → loss 0.4 (makin bagus!)

### Epoch: Berapa Kali Training?

- **Epoch 10**: Garis masih jelek
- **Epoch 50**: Mulai bagus
- **Epoch 100**: Lebih bagus lagi
- **Epoch 200**: Udah optimal

⚠️ **Tapi hati-hati:** Epoch terlalu tinggi → **overfitting**

---

## 🎨 Visualisasi Neural Network

### Arsitektur Sederhana

```
Input → [Weight] → (+Bias) → Output
  x    ×   w          + b    →   y
```

**Contoh:**
- Input: 2
- Weight: 3
- Bias: 8
- Output: (2 × 3) + 8 = **14**

### Network dengan Multiple Layers

```
Input Layer → Hidden Layer → Output Layer
   x₀            z₀             y
   x₁            z₁
```

**Fully Connected:** Setiap input nyambung ke semua neuron di layer berikutnya!

---

## 🔢 Matematika Neural Network

### Input & Output

**1 Input, 1 Neuron:**
- Input: x₀, x₁
- Weights: w₀, w₁
- Bias: b
- Output: z = (w₀ × x₀) + (w₁ × x₁) + b

### Hitung Jumlah Weights

**Quiz:**
- Input: 2, Neurons: 5 → Weights = **2 × 5 = 10**
- Bias = **5** (satu per neuron)
- Tambah layer kedua (3 neurons) → Total weights = **10 + 15 = 25**, Total bias = **8**

### Representasi Vector

Daripada nulis panjang, pakai **vector**!

```
z = W·X + B
```

Di mana:
- **X** = vector input [x₀, x₁]
- **W** = vector weights [w₀, w₁]
- **B** = vector bias
- **z** = output

**Kenapa pakai vector?**
- Lebih simpel
- Performance lebih bagus
- Scalable (gampang ditambah layer/neuron)

---

## 🚀 Multiple Layers & Matrix

### 2 Input, 2 Neurons

```
Input [x₀, x₁] → [z₀, z₁]
```

**Weights jadi Matrix 2×2:**
```
W = [w₀₀  w₀₁]
    [w₁₀  w₁₁]
```

- Row pertama = weights masuk ke z₀
- Row kedua = weights masuk ke z₁

### Deep Neural Network (2 Layers)

**Layer 1:**
```
z⁽⁰⁾ = W⁽⁰⁾·X + B⁽⁰⁾
```

**Layer 2:**
```
z⁽¹⁾ = W⁽¹⁾·z⁽⁰⁾ + B⁽¹⁾
```

**Gabungan:**
```
z⁽¹⁾ = W⁽¹⁾·(W⁽⁰⁾·X + B⁽⁰⁾) + B⁽¹⁾
```

💡 Tapi kita gak perlu nulis panjang - cukup panggil fungsi matrix multiplication!

---

## ⚡ Activation Function - Kunci Nonlinearity!

### Kenapa Butuh Activation Function?

**Masalah:** Data real gak selalu linear (garis lurus)!

Contoh data yang **GAK BISA** dipisahkan garis lurus:
- Data spiral 🌀
- Data melingkar ⭕
- Data bentuk XOR

**Solusi:** Tambah **Activation Function** setelah setiap layer!

```
Input → [Weight × Input + Bias] → Activation Function → Output
```

### Jenis-Jenis Activation Function

#### 1. **Linear** (Default - Gak Berguna!)
- Output = Input
- Gak bisa handle data nonlinear
- ⚠️ Kalau semua layer pakai linear = sama aja cuma 1 neuron!

#### 2. **ReLU** (Rectified Linear Unit) ⭐ POPULER
```
ReLU(z) = max(0, z)
```
- Input < 0 → Output = 0
- Input ≥ 0 → Output = Input
- **Paling sering dipakai!**

**Contoh:**
- ReLU(5) = 5
- ReLU(-1) = 0
- ReLU(10) = 10
- ReLU(-10) = 0

#### 3. **Leaky ReLU**
```
LeakyReLU(z) = max(0.01z, z)
```
- Mirip ReLU, tapi ada "bocor" dikit di bagian negatif
- Slope negatif = 0.01 (gak benar-benar 0)

**Contoh:**
- Input = -2 → Output = -0.02

#### 4. **Sigmoid**
```
σ(z) = 1 / (1 + e^(-z))
```
- Output range: **0 sampai 1**
- Bentuk huruf S
- Bagus untuk **probabilitas**

#### 5. **Tanh** (Hyperbolic Tangent)
- Output range: **-1 sampai 1**
- Mirip sigmoid tapi centered di 0
- Lebih bagus dari sigmoid di banyak kasus

---

## 🎮 Eksperimen di Playground

**Website:** playground.tensorflow.org

### Challenge 1: Data Melingkar
**Solusi:**
- 2 hidden layers
- Activation: **Tanh** atau **ReLU**
- Neurons: 3-4 per layer

### Challenge 2: Data XOR
**Solusi:**
- 1-2 hidden layers
- 4-8 neurons
- Activation: **ReLU**
- Bisa juga pakai **feature engineering** (x², y²)

### Challenge 3: Data Spiral 🌀 (SUSAH!)
**Solusi:**
- 8+ neurons per layer
- 3+ hidden layers
- Activation: **ReLU**
- Test loss bisa 0.002!

💡 **Pro Tips:**
- Kalau simple network bisa solve → pakai yang simple!
- Feature engineering bisa bantu simplify network
- Lebih banyak neurons/layers ≠ selalu lebih bagus

---

## 🔥 Kenapa Activation Function Penting?

### Tanpa Activation Function

```
Multiple layers tanpa activation = Cuma 1 layer doang!
```

**Bukti:**
```
Layer 1: z₁ = W₁X + B₁
Layer 2: z₂ = W₂z₁ + B₂
       = W₂(W₁X + B₁) + B₂
       = (W₂W₁)X + (W₂B₁ + B₂)
       = W'X + B'  ← Cuma linear biasa!
```

**Dengan Activation Function:**
```
z₁ = ReLU(W₁X + B₁)  ← Nonlinear!
z₂ = ReLU(W₂z₁ + B₂) ← Nonlinear lagi!
```

Sekarang bisa bikin garis melengkung! 🎉

---

## 🖼️ Aplikasi Real: Image Classification

Contoh: Deteksi gambar (Coffee, Red Panda, Orange)

**Arsitektur:**
```
Input Image → Layer 1 → Layer 2 → ... → Layer N → Output
  (RGB)         Hidden    Hidden         Hidden    (Classes)
```

**Output Layer:**
- Setiap class punya nilai
- Class dengan nilai **tertinggi** = prediksi!

**RGB vs Grayscale:**
- **RGB**: 3 channel (Red, Green, Blue) - lebih informatif
- **Grayscale**: 1 channel (hitam-putih) - susah bedain warna

---

## 📈 Loss Function

**MSE** (Mean Squared Error) untuk regression:
```
Loss = 1/n × Σ(y_pred - y_actual)²
```

Tujuan training: **Minimize loss!**

---

## 💡 Fun Facts & Tips

### Sejarah Neural Network
- Neural network **sempat hampir mati** 💀
- Hidup lagi karena:
  1. **Big Data** (internet = unlimited data)
  2. **Cloud Computing** (komputasi murah)

### Secret of Deep Learning Success
**Ternyata simple:** Tambahin data = hasil makin bagus! 😅

Contoh: GPT
- GPT-1: Data dikit
- GPT-2: Data lebih banyak
- GPT-3: **175 billion parameters** + data super banyak!
- GPT-4: ...you know the drill

### GPT-3 Specs
- **96 layers** (dalem banget!)
- **175 billion parameters**
- Konsepnya sama persis: Input → Weights → Activation → Output

---

## 🎯 Challenge untuk Kamu!

**Pertanyaan Menarik:** Kenapa pakai **Sigmoid gak bisa** solve data spiral, tapi **ReLU bisa**?

💭 **Hint:** Bukan karena range output (0-1 vs 0-∞)
🔍 **Cari tahu kenapa!** Akan dibahas sesi depan!

---

## 📝 Summary

✅ **Neural Network = Input → Weight × Input + Bias → Activation → Output**
✅ **Linear aja gak cukup - butuh activation function!**
✅ **ReLU = activation function favorit** (simple tapi powerful)
✅ **Multiple layers + activation = bisa solve masalah kompleks**
✅ **Weights & bias = yang di-training** (berubah tiap epoch)
✅ **Vector/Matrix = cara simpel representasi perhitungan**
✅ **Lebih banyak data + compute power = hasil lebih bagus**

---

## 🔜 Next Session

Kita akan bahas:
- **Backpropagation** (cara neural network belajar)
- **Detail activation function** (kenapa sigmoid gak work?)
- **Overfitting & Regularization**
- **Convolutional Neural Network (CNN)**

Stay tuned! 🚀
