# PyTorch - Part 2

## 🎯 Review: Architecture Network

### Cara Baca Network Class

```python
class Network(nn.Module):
    def __init__(self):
        super().__init__()
        self.linear1 = nn.Linear(2, 3)  # 2 input → 3 neuron
        self.linear2 = nn.Linear(3, 1)  # 3 input → 1 output
```

**Penjelasan:**
- `nn.Linear(2, 3)` → bikin 6 weight + 3 bias
  - Kenapa 6? Input 2 × Output 3 = 6 connections
- `nn.Linear(3, 1)` → bikin 3 weight + 1 bias
  - Input 3 × Output 1 = 3 connections

**Jadi `nn.Linear` itu:**
✅ Bikin weight otomatis
✅ Bikin bias otomatis  
✅ Gak perlu manual `torch.tensor()`

---

## 🔄 Forward Function

### Alur Data

```python
def forward(self, x):
    x = self.linear1(x)      # Lewat layer 1
    x = torch.relu(x)         # Activation
    x = self.linear2(x)       # Lewat layer 2
    x = torch.relu(x)         # Activation
    return x
```

**Flow:** Input → Linear1 → ReLU → Linear2 → ReLU → Output

**Aturan naming:**
- Nama layer bebas: `linear1`, `layer1`, `hidden`, `halo` (asal konsisten!)

---

## 🏋️ Training Loop Template

### Template Standar

```python
# 1. Bikin model
model = Network()
optimizer = torch.optim.SGD(model.parameters(), lr=0.1)

# 2. Training loop
for epoch in range(100):
    # Forward pass
    output = model(x)
    
    # Hitung loss
    loss = loss_function(output, y_true)
    
    # Backward pass
    optimizer.zero_grad()  # Reset gradient
    loss.backward()        # Hitung gradient
    optimizer.step()       # Update weight
    
    if epoch % 10 == 0:
        print(f"Loss: {loss.item()}")
```

**Ini template yang akan selalu berulang!**

---

## 💾 Save & Load Model

### Save Model

```python
# Setelah training selesai
torch.save(net, 'model.pth')
```

**Yang disave:**
✅ Architecture (struktur network)
✅ Weight & bias (hasil training)
✅ Siap pakai langsung!

### Load Model

```python
# Di komputer lain atau session baru
model = torch.load('model.pth')
model.eval()  # Set ke evaluation mode

# Langsung bisa pakai!
prediction = model(input_data)
```

**Keuntungan:**
- Bisa share model ke teman
- Gak perlu training ulang
- Tinggal pakai!

---

## 🎲 Random Number Generator

### Konsep Random

**Pertanyaan:** Komputer itu deterministik, tapi gimana bikin random?

**Jawaban:** Pakai **seed**!

```python
import random

# Tanpa seed → random setiap kali
random.random()  # 0.134...
random.random()  # 0.876...

# Dengan seed → deterministik
random.seed(42)
random.random()  # Selalu 0.639...

random.seed(42)
random.random()  # Lagi-lagi 0.639...
```

### Cara Kerja (Simplified)

```python
def my_random(seed):
    return (seed * 3 + 4) % 7

# Rekursif
seed = 7
seed = my_random(seed)  # 6
seed = my_random(seed)  # 2
seed = my_random(seed)  # 3
```

**Real implementation:** Pakai algoritma kompleks (linear congruential generator, Mersenne Twister, dll)

### Kapan Pakai Seed?

**Pakai seed:**
✅ Eksperimen yang perlu reproducible
✅ Debugging (mau hasil konsisten)
✅ Comparing models (fair comparison)

**Jangan pakai seed:**
❌ Production (biar bervariasi)
❌ Avoid local optima (biar gak stuck)

### Seed di PyTorch

```python
torch.manual_seed(42)  # Set seed
x = torch.rand(3, 3)   # Akan sama setiap kali di-run

# Kalau gak set seed → berbeda tiap run
```

---

## 🔧 Manipulasi Dimensi Tensor

### 1. Unsqueeze (Tambah Dimensi)

**Fungsi:** Nambah dimensi dengan size 1

```python
x = torch.tensor([1, 2, 3, 4])  # Shape: (4,)

# Unsqueeze dimensi 0
y = x.unsqueeze(0)  # Shape: (1, 4)
# Output: [[1, 2, 3, 4]]

# Unsqueeze dimensi 1
z = x.unsqueeze(1)  # Shape: (4, 1)
# Output: [[1], [2], [3], [4]]
```

**Analogi:** Kayak botol yang di-expand!

**Parameter:**
- `0` → Tambah di depan (row)
- `1` → Tambah di belakang (column)
- Bisa chain: `x.unsqueeze(0).unsqueeze(0)` → (1, 1, 4)

### 2. Squeeze (Hapus Dimensi)

**Fungsi:** Hapus dimensi yang size-nya 1

```python
x = torch.tensor([[[1, 2, 3, 4]]])  # Shape: (1, 1, 4)

# Squeeze semua
y = x.squeeze()  # Shape: (4,)

# Squeeze dimensi tertentu
z = x.squeeze(0)  # Shape: (1, 4)
```

**Rules:**
- Tanpa parameter → hapus SEMUA dimensi yang size 1
- Dengan parameter → hapus dimensi spesifik (kalau size-nya 1)
- Kalau size bukan 1 → gak berubah

**Contoh:**
```python
x = torch.ones(2, 1, 2, 1, 2)  # (2, 1, 2, 1, 2)
x.squeeze()                     # (2, 2, 2)
x.squeeze(1)                    # (2, 2, 1, 2)
```

### 3. Reshape (Ubah Bentuk)

**Fungsi:** Ubah ukuran dimensi (total elemen harus sama!)

```python
x = torch.tensor([[1, 2, 3, 4],
                  [5, 6, 7, 8]])  # Shape: (2, 4)

# Reshape jadi (4, 2)
y = x.reshape(4, 2)
# [[1, 2],
#  [3, 4],
#  [5, 6],
#  [7, 8]]

# Reshape jadi (1, 8)
z = x.reshape(1, 8)
# [[1, 2, 3, 4, 5, 6, 7, 8]]

# Reshape jadi (8, 1)
w = x.reshape(8, 1)
# [[1], [2], [3], [4], [5], [6], [7], [8]]
```

**Magic number `-1`:**
```python
x = torch.randn(2, 4)  # Total: 8 elemen

x.reshape(-1)      # (8,) → flatten
x.reshape(4, -1)   # (4, 2) → otomatis hitung
x.reshape(-1, 4)   # (2, 4) → otomatis hitung
```

**Aturan:** Total elemen harus sama!
- `(2, 4)` = 8 elemen → bisa jadi `(4, 2)`, `(1, 8)`, `(8, 1)`, `(8,)`
- `(2, 4)` ❌ TIDAK bisa jadi `(2, 2)` (cuma 4 elemen)

### 4. Flatten (Jadi 1D)

**Fungsi:** Ubah semua dimensi jadi 1D

```python
x = torch.tensor([[[[1, 2], [3, 4]],
                   [[5, 6], [7, 8]]]])  # (1, 2, 2, 2)

y = x.flatten()  # (8,)
# [1, 2, 3, 4, 5, 6, 7, 8]
```

**Sama dengan:**
```python
x.reshape(-1)  # Flatten manual
```

**Kegunaan:** Sering dipake sebelum masuk ke fully connected layer!

### 5. Permute (Tukar Urutan Dimensi)

**Fungsi:** Tukar urutan dimensi

```python
x = torch.randn(2, 3, 4)  # (a, b, c)

# Tukar jadi (c, b, a)
y = x.permute(2, 1, 0)  # (4, 3, 2)

# Tukar jadi (b, c, a)
z = x.permute(1, 2, 0)  # (3, 4, 2)
```

**Cara baca:**
- Indeks dimulai dari 0
- `permute(2, 1, 0)` → dimensi ke-2 jadi pertama, ke-1 tetap, ke-0 jadi terakhir

**Contoh praktis:**
```python
# Image format: (batch, channel, height, width)
x = torch.randn(32, 3, 224, 224)  # PyTorch format

# Convert ke TensorFlow format: (batch, height, width, channel)
x = x.permute(0, 2, 3, 1)  # (32, 224, 224, 3)
```

**Penting untuk CNN!** Format channel beda-beda di library.

---

## 📊 Aplikasi: Line Fitting

### Problem

Fit garis ke data X dan Y.

```python
# Data
x = torch.linspace(-10, 10, 100).unsqueeze(1)  # (100, 1)
y = 2 * x + 3 + torch.randn(100, 1) * 0.5     # y = 2x + 3 + noise

# Split data
train_x, train_y = x[:80], y[:80]   # 80% training
test_x, test_y = x[80:], y[80:]     # 20% testing
```

**Kenapa `unsqueeze(1)`?**
- Model expects 2D: (samples, features)
- `x` awalnya (100,) → jadi (100, 1)

### Network

```python
class SimpleNet(nn.Module):
    def __init__(self, input_features, hidden_size):
        super().__init__()
        self.layer1 = nn.Linear(input_features, hidden_size)
        self.layer2 = nn.Linear(hidden_size, 1)
    
    def forward(self, x):
        x = self.layer1(x)
        x = torch.relu(x)
        x = self.layer2(x)
        return x

# Bikin model
model = SimpleNet(input_features=1, hidden_size=10)
```

### Training

```python
optimizer = torch.optim.SGD(model.parameters(), lr=0.01)
loss_fn = nn.MSELoss()

for epoch in range(200):
    # Training
    pred = model(train_x)
    loss = loss_fn(pred, train_y)
    
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
    
    # Testing
    with torch.no_grad():
        test_pred = model(test_x)
        test_loss = loss_fn(test_pred, test_y)
    
    if epoch % 20 == 0:
        print(f"Epoch {epoch}: Train Loss = {loss.item():.4f}, Test Loss = {test_loss.item():.4f}")
```

**Penting:**
- Training loss ⬇️ → model belajar
- Test loss ⬇️ → model generalize
- Test loss ⬆️ → **OVERFITTING!**

---

## 🌈 Aplikasi: Nonlinear Classification

### Problem

Classify data dengan boundary nonlinear (kayak lingkaran).

```python
# Data 2D dengan label binary
X = torch.randn(1000, 2)  # (1000, 2) → 2 features
y = (X[:, 0]**2 + X[:, 1]**2 < 1).long()  # Label: 0 atau 1
```

### Network dengan Nonlinearity

```python
class NonlinearNet(nn.Module):
    def __init__(self):
        super().__init__()
        self.layer1 = nn.Linear(2, 300)
        self.layer2 = nn.Linear(300, 2)
    
    def forward(self, x):
        x = self.layer1(x)
        x = torch.relu(x)  # ⭐ Penting: Nonlinearity!
        x = self.layer2(x)
        return x
```

**Tanpa ReLU → Hanya bisa garis lurus!**
**Dengan ReLU → Bisa bentuk kompleks!**

### Alternative Activations

```python
# Sigmoid
x = torch.sigmoid(x)

# Tanh
x = torch.tanh(x)

# Leaky ReLU
x = torch.nn.functional.leaky_relu(x, negative_slope=0.01)
```

**Populer:** ReLU untuk hidden layers!

---

## 🧠 Memory & Batching

### Problem: Dataset Terlalu Besar

**Bayangkan:**
- 60,000 gambar
- Tiap gambar 28×28 pixels
- Total: 47 juta data points!

**Solusi:** Jangan load semua sekaligus!

### Stochastic Gradient Descent (SGD)

**Cara kerja:**
- Tiap epoch → ambil **1 sample random**
- Hitung loss dari 1 sample itu aja
- Update weight
- Next epoch → ambil sample lain

**Keuntungan:** Memori kecil
**Kekurangan:** Noisy (naik-turun ekstrem)

### Mini-Batch Gradient Descent

**Cara kerja:**
- Tiap epoch → ambil **batch (misal 100 samples)**
- Hitung loss dari batch
- Update weight
- Next epoch → ambil batch lain

**Keuntungan:**
✅ Memori terkontrol
✅ Lebih stabil dari SGD
✅ Bisa pakai GPU parallelization

**Batch size populer:** 32, 64, 128, 256

### Loss Curve

**Full batch:** Smooth, turun terus
**Mini-batch:** Naik-turun tapi trend ke bawah
**SGD:** Sangat naik-turun (zigzag)

```
Loss
 |  Full batch: \_______
 |  Mini-batch: \\/\\/\_
 |  SGD:         \/\/\/\/\_
 |_________________ Epochs
```

---

## 🖼️ MNIST: Digit Recognition

### Dataset

**MNIST = Modified National Institute of Standards and Technology**

**Isi:**
- 60,000 gambar training
- 10,000 gambar testing
- Ukuran: 28×28 pixels
- Grayscale (hitam-putih)
- Label: 0-9

### Download Data

```python
from torchvision import datasets, transforms

# Transform: Convert ke tensor
transform = transforms.ToTensor()

# Download training set
train_dataset = datasets.MNIST(
    root='./data', 
    train=True, 
    download=True, 
    transform=transform
)

# Download test set
test_dataset = datasets.MNIST(
    root='./data', 
    train=False, 
    download=True, 
    transform=transform
)
```

**Otomatis download & simpan di folder `./data`!**

### Explore Data

```python
# Ambil 1 sample
image, label = train_dataset[0]

print(image.shape)  # torch.Size([1, 28, 28])
print(label)        # 5 (contoh)

# Visualize
import matplotlib.pyplot as plt
plt.imshow(image.squeeze(), cmap='gray')
plt.title(f'Label: {label}')
plt.show()
```

**Shape (1, 28, 28):**
- `1` → Channel (grayscale = 1 channel)
- `28` → Height
- `28` → Width

**Kalau RGB:** (3, 28, 28) → 3 channels

### Kenapa 3 Dimensi?

**Konsistensi dengan standar!**
- Grayscale: (1, H, W)
- RGB: (3, H, W)
- RGBA: (4, H, W)

Channel selalu di depan (PyTorch convention).

---

## 🔄 DataLoader

### Kenapa Perlu DataLoader?

**Manual:**
```python
for epoch in range(100):
    for i in range(60000):  # Loop manual!
        x = train_dataset[i]
        # ... training ...
```

**Dengan DataLoader:**
```python
train_loader = torch.utils.data.DataLoader(
    train_dataset, 
    batch_size=100, 
    shuffle=True
)

for epoch in range(100):
    for batch_x, batch_y in train_loader:  # Otomatis batch!
        # batch_x shape: (100, 1, 28, 28)
        # batch_y shape: (100,)
        # ... training ...
```

**Keuntungan:**
✅ Otomatis batching
✅ Otomatis shuffle
✅ Parallel loading
✅ Lebih cepat!

### Parameter DataLoader

```python
DataLoader(
    dataset,
    batch_size=100,    # Ukuran batch
    shuffle=True,      # Acak setiap epoch
    num_workers=4      # Parallel loading
)
```

---

## 🎯 MNIST Network

### Flatten Input

**Problem:** Image shape (1, 28, 28), tapi Linear layer perlu 1D.

**Solusi:** Flatten!

```python
x = x.reshape(-1, 28*28)  # (batch, 1, 28, 28) → (batch, 784)
```

**784 = 28 × 28** (semua pixel jadi 1 row)

### Network Architecture

```python
class MNISTNet(nn.Module):
    def __init__(self):
        super().__init__()
        self.layer1 = nn.Linear(784, 100)  # 784 input → 100 hidden
        self.layer2 = nn.Linear(100, 10)   # 100 hidden → 10 output
    
    def forward(self, x):
        x = x.reshape(-1, 784)  # Flatten
        x = self.layer1(x)
        x = torch.relu(x)
        x = self.layer2(x)
        return x
```

**Output:** 10 angka (probability untuk digit 0-9)

### Berapa Weight?

**Layer 1:** 784 × 100 = **78,400 weights** 😱
**Layer 2:** 100 × 10 = **1,000 weights**
**Total:** ~79,400 weights + bias

---

## 📉 Loss Function untuk Classification

### Cross Entropy Loss

```python
loss_fn = nn.CrossEntropyLoss()
```

**Kenapa bukan MSE?**
- MSE untuk regression
- CrossEntropy untuk classification
- CrossEntropy optimize probability distribution

**Cara kerja:**
```python
# Model output: [0.1, 0.05, 0.8, 0.02, ...]  (10 values)
# True label: 2

loss = CrossEntropyLoss(output, label)
```

Model dimotivasi untuk:
- Maximize probabilitas class yang benar
- Minimize probabilitas class yang salah

---

## 🎯 Prediction & Accuracy

### Get Prediction

```python
# Model output: [0.1, 0.05, 0.8, 0.02, 0.01, ...]
prediction = torch.argmax(output, dim=1)  # Index yang paling tinggi
# Kalau index 2 paling tinggi → predict: 2
```

### Hitung Accuracy

```python
correct = (prediction == labels).sum().item()
accuracy = correct / len(labels)
print(f"Accuracy: {accuracy * 100:.2f}%")
```

**Contoh:**
- 100 samples
- 90 benar, 10 salah
- Accuracy = 90%

---

## 🏋️ Full Training: MNIST

### Complete Code

```python
import torch
import torch.nn as nn
from torchvision import datasets, transforms

# 1. Data
transform = transforms.ToTensor()
train_dataset = datasets.MNIST('./data', train=True, download=True, transform=transform)
test_dataset = datasets.MNIST('./data', train=False, download=True, transform=transform)

train_loader = torch.utils.data.DataLoader(train_dataset, batch_size=100, shuffle=True)
test_loader = torch.utils.data.DataLoader(test_dataset, batch_size=1000, shuffle=False)

# 2. Model
class Net(nn.Module):
    def __init__(self):
        super().__init__()
        self.layer1 = nn.Linear(784, 100)
        self.layer2 = nn.Linear(100, 10)
    
    def forward(self, x):
        x = x.reshape(-1, 784)
        x = torch.relu(self.layer1(x))
        x = self.layer2(x)
        return x

model = Net()
optimizer = torch.optim.SGD(model.parameters(), lr=0.01)
loss_fn = nn.CrossEntropyLoss()

# 3. Training
for epoch in range(10):
    # Training
    model.train()
    for batch_x, batch_y in train_loader:
        optimizer.zero_grad()
        output = model(batch_x)
        loss = loss_fn(output, batch_y)
        loss.backward()
        optimizer.step()
    
    # Testing
    model.eval()
    correct = 0
    total = 0
    with torch.no_grad():
        for batch_x, batch_y in test_loader:
            output = model(batch_x)
            pred = torch.argmax(output, dim=1)
            correct += (pred == batch_y).sum().item()
            total += len(batch_y)
    
    accuracy = correct / total
    print(f"Epoch {epoch+1}: Accuracy = {accuracy*100:.2f}%")
```

**Expected result:** 90-95% accuracy (bisa lebih dengan tuning!)

---

## 🎨 Fashion MNIST

### Apa Bedanya?

**MNIST:** Digit (0-9)
**Fashion MNIST:** Fashion items (sepatu, tas, baju, dll)

**Ukuran sama:** 28×28 grayscale
**Jumlah sama:** 60k training, 10k testing
**Classes:** 10 (tapi fashion, bukan digit)

### Labels

```python
fashion_classes = [
    'T-shirt/top',  # 0
    'Trouser',      # 1
    'Pullover',     # 2
    'Dress',        # 3
    'Coat',         # 4
    'Sandal',       # 5
    'Shirt',        # 6
    'Sneaker',      # 7
    'Bag',          # 8
    'Ankle boot'    # 9
]
```

### Download

```python
train_dataset = datasets.FashionMNIST(
    './data', 
    train=True, 
    download=True, 
    transform=transforms.ToTensor()
)

test_dataset = datasets.FashionMNIST(
    './data', 
    train=False, 
    download=True, 
    transform=transforms.ToTensor()
)
```

**Code training SAMA PERSIS dengan MNIST!**

**Tantangan:** Lebih susah dari MNIST (90% udah bagus!)

---

## 🚀 Pre-trained Models

### Konsep

**Instead of training from scratch:**
1. Download model yang udah di-train orang
2. Load weights-nya
3. Langsung pakai!

### Example: torchvision.models

```python
from torchvision import models

# Download ResNet (pre-trained on ImageNet)
model = models.resnet18(pretrained=True)

# Langsung bisa pakai untuk prediction!
model.eval()
output = model(image)
```

**Pre-trained on ImageNet:**
- 1.2 juta gambar
- 1000 classes
- Training bertahun-tahun
- **Tinggal pakai!**

### Lihat Architecture

```python
print(model)
```

Output:
```
ResNet(
  (conv1): Conv2d(3, 64, kernel_size=(7, 7), stride=(2, 2))
  (bn1): BatchNorm2d(64)
  (relu): ReLU(inplace=True)
  (maxpool): MaxPool2d(kernel_size=3, stride=2)
  (layer1): Sequential(...)
  (layer2): Sequential(...)
  ...
  (fc): Linear(512, 1000)
)
```

**Banyak layer!** Ratusan! Detail di materi CNN.

---

## 🎯 Challenge

### Tugas

**Goal:** Improve MNIST accuracy!

**Baseline:** ~90%
**Target:** 95%+
**Best possible:** 99.8% (tanpa CNN!)

**Yang bisa diubah:**
1. Hidden layer size (coba 100, 300, 500)
2. Jumlah layer (tambah hidden layers)
3. Learning rate (coba 0.001, 0.01, 0.1)
4. Activation function (ReLU, Sigmoid, Tanh)
5. Epochs (train lebih lama)

**Jangan pakai CNN dulu!** (Materi berikutnya)

### Fashion MNIST Challenge

**Target:** 90%+ accuracy

Lebih susah karena fashion items lebih kompleks!

---

## 🔑 Key Takeaways

### 1. Manipulasi Dimensi

- `unsqueeze()` → tambah dimensi
- `squeeze()` → hapus dimensi size 1
- `reshape()` → ubah bentuk (total elemen sama)
- `flatten()` → jadikan 1D
- `permute()` → tukar urutan dimensi

### 2. Random & Reproducibility

- Seed → hasil konsisten
- Tanpa seed → hasil bervariasi
- Pakai seed untuk debugging & experiments

### 3. Batching

- **Full batch:** Semua data sekaligus (berat!)
- **SGD:** 1 sample per update (noisy)
- **Mini-batch:** Sweet spot (32-256 samples)

### 4. MNIST

- 60k training, 10k testing
- 28×28 grayscale
- Flatten ke 784 features
- 10 output classes
- CrossEntropyLoss untuk classification

### 5. Training Best Practices

```python
# Training mode
model.train()
for batch in train_loader:
    # ... forward, backward, step ...

# Evaluation mode
model.eval()
with torch.no_grad():
    for batch in test_loader:
        # ... prediction only ...
```

### 6. Overfitting Detection

✅ **Bagus:** Train loss ⬇️, Test loss ⬇️
❌ **Overfitting:** Train loss ⬇️, Test loss ⬆️

**Solusi:** Regularization, dropout, early stopping (next sessions!)

---

## 📊 Summary Comparison

| Aspect | Linear Regression | Classification |
|--------|------------------|----------------|
| Output | 1 nilai continuous | 10 probabilities |
| Loss | MSELoss | CrossEntropyLoss |
| Activation | ReLU | ReLU + Softmax |
| Metric | Loss value | Accuracy % |
| Goal | Minimize error | Maximize correct predictions |

---

## 🎓 Next Sessions Preview

**Coming up:**
1. **Model Usage & Deployment** → Pakai pre-trained models
2. **CNN (Convolutional Neural Networks)** → Untuk image!
3. **Advanced techniques** → Dropout, batch norm, etc.
4. **Stable Diffusion** → Generate images!
5. **NLP** → Text processing

---

## 💡 Tips Praktis

### 1. Debugging Dimensi

**Selalu print shape!**
```python
print(x.shape)  # Cek dimensi tiap step
```

### 2. Start Simple

Mulai dari:
- Small network (10-100 neurons)
- Few epochs (5-10)
- Simple architecture

Kalau udah jalan → scale up!

### 3. Monitor Training

```python
losses = []
accuracies = []

for epoch in range(100):
    # ... training ...
    losses.append(loss.item())
    accuracies.append(accuracy)

# Plot
plt.plot(losses)
plt.plot(accuracies)
```

### 4. Save Best Model

```python
best_acc = 0
for epoch in range(100):
    # ... training ...
    if accuracy > best_acc:
        best_acc = accuracy
        torch.save(model, 'best_model.pth')
```

---

## 🌟 Fun Facts

1. **MNIST baseline:** Linear model bisa 90%+, CNN bisa 99%+!
2. **Fashion MNIST sengaja dibuat** sebagai MNIST replacement yang lebih challenging
3. **ImageNet:** 14 juta gambar, competition annual sejak 2010
4. **ResNet-18:** "18" = jumlah layers (ada ResNet-50, ResNet-101!)
5. **Transfer learning:** Pre-trained model + fine-tune = powerful!