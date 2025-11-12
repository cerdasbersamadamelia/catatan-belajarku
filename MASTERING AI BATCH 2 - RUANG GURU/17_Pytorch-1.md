# PyTorch - Part 1

## 📌 Recap: Matematika Gradient & Backpropagation

### Konsep Dasar Turunan (Derivative)

**Turunan itu sederhana:**
- Rumus: `dy/dx` = perubahan y / perubahan x
- **Artinya:** Seberapa sensitif y terhadap perubahan x
- **Contoh:** Kalau `dy/dx = 2`, artinya x berubah 1 → y berubah 2

**Kemiringan (Slope):**
- `m = Δy / Δx`
- Kemiringan besar (100) → y sangat sensitif terhadap x
- Kemiringan kecil (2) → y kurang sensitif

### Praktik Turunan

**Contoh:**
```
y = x² + 1
dy/dx = 2x
```

Kurva turunannya adalah garis lurus! Kalau diturunin lagi:
```
dy/dx dari 2x = 2 (garis horizontal)
```

---

## 🧠 Gradient Descent di Neural Network

### Konsep Network Sederhana

**Persamaan:**
```
y = wx (tanpa bias)
```

- **Goal:** Minimkan cost function
- **Cara:** Pakai gradient descent
- **Proses:** Ambil titik random → turun dikit-dikit sampai dapat nilai minimum

### Dengan Bias

**Persamaan lebih lengkap:**
```
z = wx + b
a = activation(z)
cost = (a - y)²
```

**Yang perlu kita ubah:** w (weight) dan b (bias)

**Caranya:**
- Cari `∂cost/∂w` (sensitivitas cost terhadap w)
- Cari `∂cost/∂b` (sensitivitas cost terhadap b)

---

## 🔄 Chain Rule & Backpropagation

### Chain Rule

Karena ada perhitungan bertingkat (cost → a → z → w/b), kita pakai **chain rule**:

```
∂cost/∂b = (∂cost/∂a) × (∂a/∂z) × (∂z/∂b)
∂cost/∂w = (∂cost/∂a) × (∂a/∂z) × (∂z/∂w)
```

**Prinsip:** Hitung sensitivitas tiap layer, lalu kalikan semua!

### Backpropagation

- Hitung dari belakang (output) ke depan (input)
- Makanya disebut "back" propagation
- Multiple layer? Sama aja, cuma lebih kompleks

---

## 🎯 Praktik di Google Sheet

### Rumus Turunan

**DC/DW (derivative cost terhadap weight):**
```
dc/da × da/dz × dz/dw
```

**Activation function:**
- Pakai sigmoid → turunannya punya rumus khusus
- Pakai ReLU → turunan = 1 (jika z > 0), 0 (otherwise)

### Update Weight & Bias

**Rumus:**
```
w_baru = w_lama - (learning_rate × gradient)
b_baru = b_lama - (learning_rate × gradient)
```

**Learning Rate:**
- Besar (10) → langkah besar, cepat tapi bisa lewat minimum
- Kecil (0.01) → langkah kecil, lambat tapi lebih akurat
- Kegedean (1000) → malah tidak converge!

---

## 📊 PyTorch: Tensor Basics

### Apa itu Tensor?

Tensor = container untuk data dengan berbagai dimensi

**Dimensi:**
- **0D (scalar):** angka biasa → `tensor(42)`
- **1D (vector):** array → `tensor([1, 2, 3])`
- **2D (matrix):** array dalam array → `tensor([[1, 2], [3, 4]])`
- **3D (cube):** array dalam array dalam array

### Keuntungan Tensor

✅ Bisa pakai GPU untuk kalkulasi cepat!
✅ Operasi matematika otomatis
✅ Gradient descent terotomasi

---

## 🛠️ Membuat Tensor

### Cara-Cara Membuat

```python
import torch

# Dari list
x = torch.tensor([1, 2, 3])

# Random
torch.randn(5, 3)  # 5 baris, 3 kolom, nilai random

# Angka 1 semua
torch.ones(2, 3)  # 2×3 matrix isi 1

# Angka 0 semua
torch.zeros(2, 3)

# Range
torch.arange(1, 10)  # [1, 2, 3, ..., 9]

# Konstan custom
torch.full((2, 3), 42)  # 2×3 matrix isi 42
```

**Kenapa pakai tuple `(2, 3)`?**
- Supaya jelas mana size, mana value
- `(2, 3)` = size, `42` = value

### Cek Dimensi

```python
x.dim()     # Lihat jumlah dimensi
x.shape()   # Lihat ukuran tiap dimensi
```

---

## 💻 Operasi Tensor

### Basic Operations

```python
x + 1       # Tambah semua elemen dengan 1
x * 2       # Kali semua elemen dengan 2
x ** 2      # Pangkat 2 semua elemen
x / 2       # Bagi semua elemen dengan 2
```

### Broadcasting

**Ini magic!** Bisa operasi tensor dengan ukuran berbeda:

```python
x = torch.tensor([[1, 2, 3]])  # 1×3
x + 1  # Broadcasting: 1 disebarkan ke semua elemen
```

**Rules:**
- Dimensi harus sama ATAU salah satunya 1
- Cek dari kanan ke kiri

### Matrix Multiplication

```python
# Perkalian matrix (dot product)
torch.mm(x, y)
x @ y  # Sama aja

# Perkalian elemen-per-elemen (BEDA!)
x * y  # Element-wise, bukan matrix multiplication!
```

### Transpose

```python
x.T        # Tukar baris jadi kolom
x.shape    # Ukuran juga kebalik
```

---

## 🔥 PyTorch: Data Types

### Set Type

```python
# Float 32 (default, recommended)
x = torch.tensor([1, 2], dtype=torch.float32)

# Float 64 (lebih presisi, lebih berat)
x = torch.tensor([1, 2], dtype=torch.float64)

# Integer
x = torch.tensor([1, 2], dtype=torch.int32)
```

**Kenapa penting?**
- Float64 → lebih presisi tapi butuh memori lebih
- Float32 → balance antara akurasi & efisiensi
- Integer → kalau cukup untuk task tertentu

**Fun fact tentang float:**
```python
0.1 + 0.2  # Hasilnya bukan 0.3!
# Output: 0.30000000000000004
```
Ini karena cara komputer simpan angka desimal.

---

## 🎓 Neural Network di PyTorch

### Forward Pass Sederhana

**Manual:**
```python
# Input
x = torch.tensor([[1], [2]])  # 2×1

# Weight & bias
w = torch.tensor([[3, 4]])    # 1×2
b = torch.tensor([5])

# Forward
z = torch.mm(w, x) + b        # Linear
a = torch.relu(z)             # Activation
```

**Output:** `[16]` ✅

### Multiple Layer

**Layer 1:**
```python
w0 = torch.tensor([[3, 5], [-3, 4]])  # 2×2
b0 = torch.tensor([-2, 8])
z0 = torch.mm(w0, x) + b0
a0 = torch.relu(z0)
```

**Layer 2:**
```python
w1 = torch.tensor([[7], [8]])  # 2×1
b1 = torch.tensor([3])
z1 = torch.mm(w1, a0) + b1
a1 = torch.relu(z1)
```

**Keren kan?** Tinggal stack layer demi layer!

---

## 📐 Gradient Descent di PyTorch

### Manual Gradient

**Hitung turunan otomatis:**
```python
x = torch.tensor(5.0, requires_grad=True)  # Aktifkan tracking
y = x**2 + 2*x + 1  # Equation

y.backward()  # Hitung gradient
print(x.grad)  # Output: 12.0 (di titik x=5)
```

**Cara kerjanya:**
1. `requires_grad=True` → aktifkan tracker
2. `backward()` → hitung turunan
3. `x.grad` → simpan hasil gradient

### Gradient Descent Loop

**Turunin gunung manual:**
```python
x = torch.tensor([5.0], requires_grad=True)
alpha = 0.1  # Learning rate

for i in range(10):
    y = x**2 + 2*x + 1  # Cost function
    y.backward()        # Hitung gradient
    
    with torch.no_grad():  # Matikan tracking sementara
        x -= alpha * x.grad  # Update x
    
    x.grad.zero_()  # Reset gradient
```

**Kenapa `no_grad()`?**
- Operasi update `x -= ...` ga perlu di-track
- Kalau di-track, nanti gradient jadi kacau

---

## 🚀 Optimizer: Lebih Gampang!

### Pakai SGD

**Ga perlu manual lagi:**
```python
x = torch.tensor([5.0], requires_grad=True)
optimizer = torch.optim.SGD([x], lr=0.1)

for i in range(10):
    optimizer.zero_grad()  # Reset gradient
    y = x**2 + 2*x + 1
    y.backward()
    optimizer.step()       # Update parameter
```

**Optimizer lain:**
- `SGD` → Stochastic Gradient Descent (simple)
- `Adam` → Lebih advanced, populer banget!
- `RMSprop` → Variant lain

---

## 🧪 Neural Network Class

### Cara Lama: Manual

```python
# Bikin weight & bias manual
w = torch.tensor([[3.0], [1.0]], requires_grad=True)
b = torch.tensor([0.0], requires_grad=True)

# Forward pass manual
def forward(x):
    z = torch.mm(w.T, x) + b
    return torch.sigmoid(z)
```

### Cara Baru: Pakai Class

**Lebih clean & scalable:**
```python
import torch.nn as nn

class NotGate(nn.Module):
    def __init__(self):
        super().__init__()
        self.linear = nn.Linear(1, 1)  # 1 input → 1 output
    
    def forward(self, x):
        x = self.linear(x)
        x = torch.sigmoid(x)
        return x

# Pakai model
model = NotGate()
optimizer = torch.optim.SGD(model.parameters(), lr=0.1)
```

**Keuntungan:**
✅ Weight & bias otomatis dibuat
✅ Ga perlu manual tracking
✅ Lebih mudah di-maintain

---

## 🎯 Multiple Layer dengan Class

### Network 2 Layer

```python
class Network(nn.Module):
    def __init__(self):
        super().__init__()
        self.layer1 = nn.Linear(2, 3)  # 2 input → 3 neuron
        self.layer2 = nn.Linear(3, 1)  # 3 input → 1 output
    
    def forward(self, x):
        x = self.layer1(x)
        x = torch.relu(x)    # Activation layer 1
        x = self.layer2(x)
        x = torch.relu(x)    # Activation layer 2
        return x
```

**Struktur:**
```
Input (2) → Linear → ReLU → Linear → ReLU → Output (1)
```

**Nama layer bebas!**
- Bisa `layer1`, `layer2`
- Bisa `hidden`, `output`
- Bisa `halo`, `world` (tapi jangan 😅)

---

## 🏋️ Training Loop

### Full Training Code

```python
model = Network()
optimizer = torch.optim.SGD(model.parameters(), lr=0.1)
loss_fn = nn.MSELoss()  # Mean Squared Error

# Training loop
for epoch in range(100):
    # Forward pass
    y_pred = model(x)
    
    # Hitung loss
    loss = loss_fn(y_pred, y_true)
    
    # Backward pass
    optimizer.zero_grad()  # Reset gradient
    loss.backward()        # Hitung gradient
    optimizer.step()       # Update weight
    
    if epoch % 10 == 0:
        print(f"Epoch {epoch}, Loss: {loss.item()}")
```

**Alur:**
1. Forward → prediksi
2. Loss → hitung error
3. Backward → hitung gradient
4. Step → update weight
5. Repeat!

---

## 🎨 GPU vs CPU

### Kenapa GPU?

**CPU:**
- Sequential processing
- Kayak 1 orang kerja cepat
- Multi-core bisa parallel (tapi terbatas)

**GPU:**
- Parallel processing
- Kayak 1000+ orang kerja bareng
- Perfect untuk matrix operations!

### Pakai GPU di PyTorch

```python
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')

# Pindah tensor ke GPU
x = x.to(device)
model = model.to(device)
```

**Instant speed boost!** 🚀

---

## 📝 Tips & Tricks

### 1. Learning Rate

- **Terlalu besar** → melompat-lompat, ga converge
- **Terlalu kecil** → lambat banget
- **Sweet spot:** Biasanya 0.001 - 0.1

### 2. Gradient = 0

Artinya udah di titik optimal (minimal/maksimal)!

### 3. Activation Functions

**Pilihan populer:**
- `ReLU` → untuk hidden layers (cepat & efektif)
- `Sigmoid` → untuk output binary (0-1)
- `Tanh` → alternatif sigmoid
- `Softmax` → untuk multi-class classification

**Boleh beda tiap layer!**

### 4. Broadcasting Rules

Dari kanan ke kiri:
- Dimensi sama → ✅
- Salah satu = 1 → ✅
- Salah satu kosong → ✅
- Lainnya → ❌

---

## 🎯 Project Notes

### Google Sheet Exercise

Coba mainkan:
- Learning rate → lihat efeknya ke convergence
- Gradient → lihat sensitivitas
- Weight & bias → lihat update-nya

### PyTorch Exercise

**Task A:** Bikin network sederhana
- Input: [0, 1]
- Output: [1, 0] (NOT gate)

**Task B:** Multiple layer (bonus, lebih susah!)

---

## 🔑 Key Takeaways

1. **Gradient descent** = jalan turun gunung cari titik terendah
2. **Backpropagation** = hitung gradient dari belakang
3. **Tensor** = container data multi-dimensi
4. **PyTorch** = library yang handle semua complexity
5. **Class-based network** = cara modern & scalable
6. **Optimizer** = otomatis update weight
7. **GPU** = boost performa untuk matrix operations

---

## 📚 Next Session Preview

Besok (Jumat) akan belajar:
- PyTorch lebih dalam
- Fitting curves & nonlinearity
- Image classification (digit recognition 0-9)
- Praktik lebih banyak!

**Deadline Project:** 31 Januari
**Jangan lupa:** Google sheet exercises penting untuk intuisi!

---

## 🌟 Fun Facts

1. **Float precision issue:** `0.1 + 0.2 ≠ 0.3` (terjadi di semua bahasa!)
2. **PyTorch vs TensorFlow:** PyTorch lebih populer di research papers sekarang
3. **GPU painting demo:** Nvidia bikin demo keren tentang parallelization
4. **Tuple dengan koma:** `(2,)` bukan `(2)` → python syntax quirk!