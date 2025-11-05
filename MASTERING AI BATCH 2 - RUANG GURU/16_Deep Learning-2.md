# Deep Learning 2 - Matematika di Balik Neural Network

---

## 🎯 Overview

Sesi ini fokus pada **matematika di balik Deep Learning**, khususnya:
- Kalkulus (Turunan/Derivative)
- Gradient Descent
- Backpropagation
- Loss Function Optimization

---

## 📌 Recap: Neural Network Basics

### Single Neuron - Single Input
```
Input (x=2) → [Neuron (w=3, b=1)] → Output
```

**Perhitungan:**
- 2 × 3 = 6
- 6 + 1 = 7... tunggu, ada bias yang dikurangi!
- 2 × 3 = 6, lalu 6 - 1 = **5**

**Output: 5**

---

### Multiple Input - Single Neuron
```
Input 1 (x₁=1) → w₁=-2 ┐
                       ├→ [Neuron (b=3)] → Output
Input 2 (x₂=2) → w₂=3  ┘
```

**Perhitungan:**
- (1 × -2) + (2 × 3) + 3
- -2 + 6 + 3 = **7**

**Output: 7**

---

### Multiple Input - Multiple Neurons
```
Input 1 (x₁=1) → w₁=2, w₃=3    ┐
                                ├→ Neuron 1 (b₁=2) → Output 1
Input 2 (x₂=2) → w₂=5, w₄=-2   ├→ Neuron 2 (b₂=2) → Output 2
                                ┘
```

**Neuron 1:**
- (1 × 2) + (2 × 5) + 2
- 2 + 10 + 2 = **14**

**Neuron 2:**
- (1 × 3) + (2 × -2) + 2
- 3 - 4 + 2 = **1**

**Output: (14, 1)**

---

## 🎬 Activation Function

### ReLU (Rectified Linear Unit)
```
f(x) = max(0, x)
```

**Karakteristik:**
- Jika x > 0 → output = x
- Jika x ≤ 0 → output = 0

**Contoh 1:**
```
Input=2, w=3, b=-1 → 2×3-1 = 5 → ReLU(5) = 5
```

**Contoh 2:**
```
Input=2, w=-3, b=-1 → 2×(-3)-1 = -7 → ReLU(-7) = 0
```

---

## 🤖 Fun Fact: LLaMA (Meta)

### Kenapa LLaMA Menarik?
- **Open Source** dari Meta (Facebook/Instagram/WhatsApp)
- Bisa dijalankan di komputer lokal!
- Code cuma **500 baris** (C/C++)
- Yang besar itu **parameter**-nya (weights & biases): **140 GB!**

### Magic-nya di Mana?
```
Code: 500 lines ← Kecil!
Parameters: 140 GB ← BESAR! (ini isinya weights & biases)
```

**ChatGPT kerja gimana?**  
- Predict kata **per kata** (token per token)
- Dibikin seolah ngetik pelan biar keliatan keren
- Tapi sebenarnya emang prediksi satu-satu

---

## 🏆 Chatbot Arena Leaderboard

**Top Models:**
1. GPT-4 (Closed Source)
2. Claude (Closed Source)
3. **LLaMA** (Open Source) ← Naik terus!

**Kenapa Model Besar Lebih Bagus?**
- Makin banyak neurons → Loss makin kecil
- Makin banyak data training → Makin akurat
- Tapi... butuh GPU mahal!

**Kenapa Ada Model Kecil?**
- Bisa jalan di HP! (contoh: Gemini Nano)
- Lebih murah
- Tidak perlu internet
- Cocok untuk task sederhana

---

## 📐 Kalkulus: Derivative (Turunan)

### Aturan Dasar
```
f(x) = 2x³ + 3x² + 2x + 1
```

**Cara Menurunkan:**
- Pangkat × koefisien
- Pangkat turun 1

```
f'(x) = 3×2x² + 2×3x¹ + 1×2x⁰ + 0
f'(x) = 6x² + 6x + 2
```

---

### Tapi... Kenapa Begitu?

**Definisi Turunan:**
> Turunan = **Kemiringan garis singgung** di suatu titik

---

## 📊 Visualisasi: Garis Singgung

```
      ╱
    ╱
  ╱  ← Garis singgung (merah)
 •
╱ ╲
   ╲ ← Kurva asli (biru)
```

**Garis Singgung:**
- Bentuknya: `y = mx + c`
- **m** = kemiringan (slope)
- **c** = konstanta

**Cara Cari Kemiringan:**
1. Ambil 2 titik yang dekat
2. Hitung: `m = Δy / Δx`
3. Makin dekat titiknya, makin akurat!

---

### Contoh Perhitungan Slope

**Titik 1:** (2, 13)  
**Titik 2:** (3, 38)

```
m = (38 - 13) / (3 - 2)
m = 25 / 1
m = 25
```

**Makin dekat, makin akurat:**
- Jarak 1.0 → kurang akurat
- Jarak 0.5 → lebih akurat
- Jarak 0.1 → sangat akurat
- Jarak → 0 → **PERFECT!** ← Ini definisi limit!

---

## 🔢 Limit & Derivative

### Definisi Formal
```
dy/dx = lim(Δx→0) [f(x + Δx) - f(x)] / Δx
```

**Artinya:**
- Δx makin kecil mendekati 0
- Slope makin mendekati nilai sebenarnya

---

### Contoh: Turunan x²

**Given:** `f(x) = x²`

**Langkah 1:** Masukkan ke rumus limit
```
dy/dx = lim(Δx→0) [(x + Δx)² - x²] / Δx
```

**Langkah 2:** Jabarkan
```
= lim(Δx→0) [x² + 2x·Δx + Δx² - x²] / Δx
```

**Langkah 3:** Coret yang sama
```
= lim(Δx→0) [2x·Δx + Δx²] / Δx
```

**Langkah 4:** Bagi semua dengan Δx
```
= lim(Δx→0) [2x + Δx]
```

**Langkah 5:** Δx → 0
```
= 2x
```

**Terbukti!** Turunan x² adalah **2x** ✅

---

## 🧮 Aturan Turunan

### 1. Penjumlahan
```
d/dx[f(x) + g(x)] = f'(x) + g'(x)
```

**Contoh:**
```
f(x) = x² + 2x
f'(x) = 2x + 2
```

---

### 2. Perkalian (Product Rule)
```
d/dx[f(x) · g(x)] = f'(x)·g(x) + f(x)·g'(x)
```

**Contoh:**
```
f(x) = x² · 2x = 2x³
f'(x) = (2x)·(2x) + (x²)·(2) = 4x² + 2x² = 6x²
```

---

### 3. Pangkat
```
d/dx[xⁿ] = n·xⁿ⁻¹
```

**Contoh:**
```
f(x) = x³ → f'(x) = 3x²
f(x) = x⁵ → f'(x) = 5x⁴
```

---

### 4. Chain Rule (Aturan Rantai)
```
dy/dx = (dy/du) · (du/dx)
```

**Penting banget untuk Backpropagation!**

---

## 🎯 Gradient Descent

### Konsep: Turun Gunung

Bayangkan kamu di puncak gunung, mau turun ke lembah (titik minimum):

```
      🏔️
     ╱  ╲
    ╱    ╲
   ╱      ╲
  ╱   🎯   ╲
 ╱          ╲
```

**Pertanyaan:** Gimana caranya turun?
1. Cek kemiringan (slope)
2. Jalan ke arah yang turun
3. Ulangi sampai sampai dasar

---

### Formula Gradient Descent

```
x_new = x_old - α · (dy/dx)
```

**Di mana:**
- **x_old** = posisi sekarang
- **α** (alpha) = learning rate (ukuran langkah)
- **dy/dx** = slope (kemiringan)
- **x_new** = posisi baru

---

### Contoh: Cari Minimum f(x) = x² - 6x + 14

**Langkah 1:** Turunan
```
f'(x) = 2x - 6
```

**Langkah 2:** Mulai dari x = 10, α = 0.1
```
Iterasi 1:
slope = 2(10) - 6 = 14
x_new = 10 - 0.1×14 = 8.6

Iterasi 2:
slope = 2(8.6) - 6 = 11.2
x_new = 8.6 - 0.1×11.2 = 7.48

... (lanjut terus)

Iterasi 20:
x ≈ 3.0 ← MINIMUM! 🎯
```

---

## 📉 Learning Rate (α)

### Learning Rate Terlalu Kecil (α = 0.01)
```
❌ Terlalu lambat!
   Butuh 200 iterasi
   Kayak semut turun gunung
```

### Learning Rate Pas (α = 0.1)
```
✅ Optimal!
   Cukup 20 iterasi
   Cepat & akurat
```

### Learning Rate Terlalu Besar (α = 0.8)
```
⚠️ Masih oke...
   Lebih cepat dari α=0.1
```

### Learning Rate Kebesaran (α = 1.5)
```
❌ BAHAYA!
   Malah bounce sana-sini
   Tidak konvergen
   Bisa diverge (makin jauh dari minimum)
```

---

## 🎨 Visualisasi Learning Rate

```
α terlalu kecil:
🐌 → → → → → → → → → 🎯 (lambat banget)

α optimal:
🏃 → → → 🎯 (pas!)

α terlalu besar:
🚀 ↗️ ↘️ ↗️ ↘️ ↗️ ↘️ (bounce terus!)
```

---

## 🧪 Aplikasi ke Neural Network

### Problem: NOT Gate

**Training Data:**
```
Input | Expected Output
------|----------------
  0   |       1
  1   |       0
```

**Neural Network:**
```
x → [w, b] → z → output
```

**Goal:** Cari w dan b yang tepat!

---

### Manual (Trial & Error)

```
Coba w=2, b=3:
- Input 0: 0×2+3 = 3 → sigmoid → 0.95 ❌ (harusnya 1)
- Input 1: 1×2+3 = 5 → sigmoid → 0.99 ❌ (harusnya 0)
Cost = 0.99 (jelek!)

Coba w=-1, b=1:
- Input 0: 0×(-1)+1 = 1 ✅
- Input 1: 1×(-1)+1 = 0 ✅
Cost = 0.00 (PERFECT!)
```

**Tapi manual itu capek!** 😫

---

### Otomatis: Gradient Descent!

**Cost Function:**
```
C = (y_expected - y_predicted)²
```

**Yang Perlu Dicari:**
1. ∂C/∂w (slope terhadap weight)
2. ∂C/∂b (slope terhadap bias)

---

## 🔗 Backpropagation

### Arsitektur
```
x → [w] → z → a → C
     ↑    ↑   ↑   ↑
     |    |   |   |
    weight z  activation  cost
```

**Problem:** Gimana cari ∂C/∂w kalau jaraknya jauh?

**Solusi:** **Chain Rule!**

---

### Chain Rule untuk Backprop

**Mau cari:** ∂C/∂w

**Tapi C jauh dari w, jadi:**
```
∂C/∂w = (∂C/∂z) · (∂z/∂w)
```

**Atau kalau ada activation:**
```
∂C/∂w = (∂C/∂a) · (∂a/∂z) · (∂z/∂w)
```

**Inilah "chaining" backward dari output ke input!**

---

### Detail Perhitungan

**Given:**
- z = wx + b
- a = sigmoid(z)
- C = (y - a)²

**Step 1: ∂C/∂a**
```
C = (y - a)²
∂C/∂a = -2(y - a) = 2(a - y)
```

**Step 2: ∂a/∂z**
```
a = sigmoid(z)
∂a/∂z = sigmoid'(z) = a(1-a)
```

**Step 3: ∂z/∂w**
```
z = wx + b
∂z/∂w = x
```

**Gabungkan (Chain Rule):**
```
∂C/∂w = ∂C/∂a · ∂a/∂z · ∂z/∂w
∂C/∂w = 2(a-y) · a(1-a) · x
```

**Sama untuk bias:**
```
∂C/∂b = 2(a-y) · a(1-a) · 1
```

---

### Update Weight & Bias

```
w_new = w_old - α · (∂C/∂w)
b_new = b_old - α · (∂C/∂b)
```

**Ulangi sampai Cost → 0!**

---

## 📊 Simulasi Training

### Epoch 1
```
Input: x=0
w=2, b=3
z = 0×2+3 = 3
a = sigmoid(3) = 0.95
Expected: 1
Cost: (1-0.95)² = 0.0025

∂C/∂w = ...hitung... = -0.13
∂C/∂b = ...hitung... = -0.07

Update:
w = 2 - 0.1×(-0.13) = 2.013
b = 3 - 0.1×(-0.07) = 3.007
```

### Epoch 2
```
w=2.013, b=3.007
Cost turun → 0.97
Masih belum bagus, lanjut!
```

### Epoch 20
```
w ≈ -1.0
b ≈ 1.0
Cost ≈ 0.04
Sudah lumayan!
```

### Epoch 100
```
w = -1.0
b = 1.0
Cost ≈ 0.00
PERFECT! 🎉
```

---

## 🌄 Cost Function Landscape

### 2D (1 Parameter)
```
  Cost
    |     ╱╲
    |    ╱  ╲
    |   ╱    ╲
    |  ╱  🎯  ╲
    |_╱________╲___
           w
```

### 3D (2 Parameters: w & b)
```
       Cost
        ↑
        |     ╱╲
        |    ╱  ╲
        |   ╱ 🎯 ╲
        |__╱______╲___→ w
        ╱
       b
```

**Kita cari titik paling bawah (🎯)!**

---

## ⚠️ Local Minima Problem

```
       ╱╲         ╱╲
      ╱  ╲       ╱  ╲
     ╱ 🔴 ╲     ╱ 🎯 ╲
    ╱______╲___╱______╲
   Local Min   Global Min
```

**Problem:** Bisa застрять di local minimum!

**Solusi:**
1. Coba learning rate berbeda
2. Multiple random start
3. Adaptive learning rate
4. Advanced optimizer (Adam, RMSprop)

**Fun Fact:** Di high-dimension, local minima jarang terjadi!

---

## 🎓 Key Takeaways

### 1. Turunan = Kemiringan
- Dipakai untuk tahu arah mana yang turun
- Makin curam, makin cepat turun

### 2. Gradient Descent = Turun Gunung
- Mulai dari titik random
- Cek kemiringan
- Jalan sedikit ke arah turun
- Repeat!

### 3. Learning Rate = Ukuran Langkah
- Terlalu kecil → lambat
- Terlalu besar → bounce
- Harus pas!

### 4. Backpropagation = Chain Rule
- Turunan dari output ke input
- Backward pass untuk update weight
- Ini yang bikin neural network bisa "belajar"

### 5. Cost Function = Target Optimasi
- Ukuran seberapa salah prediksi
- Goal: bikin cost sekecil mungkin
- Pakai gradient descent untuk minimize

---

## 💡 Tips Praktis

### ✅ DO:
- Pahami konsep, bukan cuma rumus
- Visualisasikan (gambar kurva, gunung, dll)
- Eksperimen dengan learning rate
- Mulai dari case sederhana (NOT gate)

### ❌ DON'T:
- Jangan hafalkan rumus tanpa paham
- Jangan skip matematika (penting!)
- Jangan pakai learning rate sembarangan
- Jangan takut mencoba!

---

## 🔧 Praktik: Google Sheets

**Link ada di materi!**

Fitur:
- Simulasi gradient descent
- Manual tuning w & b
- Visualisasi cost function
- Lihat langkah per langkah

**Coba sendiri:**
1. Ubah learning rate
2. Ubah starting point
3. Lihat berapa iterasi sampai konvergen
4. Eksperimen dengan activation function

---

## 📚 Resources Tambahan

### Video Recommended:
- **3Blue1Brown:** Neural Network series (visualisasi keren!)
- Chain rule & backpropagation explained
- Gradient descent visualization

### Tools:
- Google Sheets (ada di materi)
- TensorFlow Playground
- Desmos (untuk plot fungsi)

---

## 🎯 Project Mendatang

### Project 2: Machine Learning
- **Deadline:** Jumat, 26 Januari (tengah malam)
- Sudah bisa diakses di LMS

### Project 3: Deep Learning
- **Deadline:** Minggu depan (tengah malam)
- Akan dibahas di pertemuan berikutnya

**Catatan:** Project akan menggunakan library (TensorFlow/PyTorch), jadi tidak perlu implement gradient descent manual. Tapi **PENTING** untuk paham konsepnya!

---

## 🤔 FAQ

**Q: Apakah harus hapal semua rumus?**  
A: Tidak! Yang penting paham konsep. Library sudah handle semua perhitungan.

**Q: Kenapa harus belajar matematika kalau sudah ada library?**  
A: Supaya tahu "magic" di baliknya! Biar bisa debug, optimize, dan paham kenapa model tidak bagus.

**Q: Apakah selalu bentuk mangkok cost function?**  
A: Tidak selalu. Bisa ada local minima. Tapi di high-dimension, jarang stuck.

**Q: Learning rate optimal berapa?**  
A: Tergantung problem! Biasanya 0.001 - 0.1. Harus trial & error atau pakai adaptive optimizer.

---

## 🚀 Next Steps

1. ✅ Review materi ini
2. ✅ Main-main di Google Sheets
3. ✅ Tonton video 3Blue1Brown
4. ✅ Kerjakan Project 2
5. ✅ Siap untuk pertemuan berikutnya!

---

## 📝 Catatan Penting

> "Matematika di deep learning itu seperti mesin di mobil. Kamu tidak harus jadi mekanik untuk bisa nyetir, tapi kalau paham cara kerja mesin, kamu bisa jadi driver yang lebih baik!"

**Intinya:**
- Library sudah handle semua
- Tapi paham konsep = POWER! 💪
- Gradient Descent = Inti dari learning
- Backpropagation = Chain rule applied
- Turunan = Kemiringan = Arah turun

---

**Happy Learning! 🎉**
