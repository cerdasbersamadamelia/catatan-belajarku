# NLP-3: Word Embedding & Preprocessing

---

## 📌 Overview

Sesi ini membahas tentang **Word Embedding** - cara komputer memahami dan merepresentasikan kata dalam bentuk angka yang punya makna. Kita juga belajar cara bikin model word embedding sendiri!

---

## 🔤 Bagaimana Komputer Memahami Teks?

### ASCII - Encoding Dasar
- **ASCII** = standar encoding untuk komunikasi elektronik
- Setiap karakter punya kode unik:
  - Tanda seru `!` = 33
  - Angka `1` = 49
  - Huruf `A` (kapital) = 65
  - Huruf `a` (kecil) = 97

**Contoh:** Kata "Hello"
- H = 72
- e = 101
- l = 108
- l = 108
- o = 111

### ❌ Masalah ASCII

1. **Tidak mengerti arti kata**
   - Hanya convert string → angka
   - Komputer gak ngerti makna di balik kata

2. **Tidak tangkap hubungan antar kata**
   - Kata "kecil" vs "besar" (antonim) → komputer gak tahu mereka berlawanan
   - Kata "king" vs "raja" (sama artinya) → komputer gak tahu mereka mirip

---

## 🎯 One-Hot Encoding / Bag of Words

### Cara Kerja

**Contoh kalimat:** "I love good morning"

1. Bikin vocabulary (kata unik) → 5 kata unik
2. Setiap kata jadi vektor:
   - `hello` = [1, 0, 0, 0, 0]
   - `morning` = [0, 1, 0, 0, 0]
   - `good` = [0, 0, 1, 0, 0]
   - dll.

3. Kalimat jadi matriks dari gabungan vektor kata-katanya

### ✅ Kelebihan
- Satu-satunya cara dulu untuk olah data teks
- Simpel dan mudah dipahami

### ❌ Kekurangan

1. **Dimensi terlalu tinggi & sparse**
   - Kalau vocab 400,000 kata → vektor [1, 0, 0, 0, ... 400,000 angka nol]
   - Banyak banget nol-nya = tidak efisien
   - Butuh komputasi tinggi

2. **Kata-kata independen**
   - "King" = [1, 0]
   - "Raja" = [0, 1]
   - Padahal artinya sama! Tapi gak keliatan hubungannya

3. **Gak bisa tangkap konteks**

**Sekarang:** Jarang dipakai kecuali untuk data kategorikal

---

## 📐 Nostalgia Vektor (Fisika SMA!)

Sebelum masuk word embedding, kita perlu paham vektor dulu.

### Konsep Vektor

**Vektor (2, 5)** = dari titik (0,0) ke titik (2,5)
- 2 = bergerak 2 langkah ke kanan (sumbu X)
- 5 = bergerak 5 langkah ke atas (sumbu Y)

### Operasi Vektor

**Penjumlahan:**
- v = (2, 5)
- w = (3, -5)
- v + w = (2+3, 5+(-5)) = (5, 0)

**Besaran Vektor (Magnitude/Panjang):**

Vektor (2, 5) → panjangnya = √(2² + 5²) = √29

### Dot Product (Perkalian Titik)

Vektor v = (2, 5) dan w = (3, -5)

v · w = (2×3) + (5×-5) = 6 + (-25) = **-19**

Hasilnya **skalar** (angka biasa), bukan vektor!

### 🎯 Sudut Antar Vektor (Penting!)

**Rumus:**
```
v · w = |v| × |w| × cos(θ)

cos(θ) = (v · w) / (|v| × |w|)
```

**Kenapa penting?**
- Sudut vektor = tingkat kemiripan
- Nanti dipakai di **Cosine Similarity**!

---

## 🔥 Cosine Similarity

### Konsep

**Cosine Similarity** = ukur kemiripan dua vektor dengan sudut di antara mereka

**Nilai:**
- **1** = vektor sama persis (sudut 0°)
- **0** = vektor tegak lurus (gak ada hubungan)
- **-1** = vektor berlawanan arah (sudut 180°)

**Threshold praktis:**
- ≥ 0.9 = dianggap sama
- ≥ 0.8 = lumayan mirip

### Implementasi

```python
# Cara 1: Dari scratch
def cosine_similarity(v, w):
    dot_product = np.dot(v, w)
    magnitude_v = np.sqrt(np.sum(v**2))
    magnitude_w = np.sqrt(np.sum(w**2))
    return dot_product / (magnitude_v * magnitude_w)

# Cara 2: Pakai library
from sklearn.metrics.pairwise import cosine_similarity
```

**Keuntungan bikin dari scratch:**
- Lebih fleksibel
- Bisa di-batch untuk banyak vektor sekaligus
- Bisa dioptimasi pakai NumPy atau Numba

---

## 🌟 Word Embedding

### Apa itu Word Embedding?

**Word Embedding** = cara ubah kata jadi vektor multidimensi yang:
- ✅ Punya makna
- ✅ Tangkap hubungan antar kata
- ✅ Dimensi lebih kecil dari one-hot encoding
- ✅ Kata dengan arti mirip punya vektor yang mirip

### Contoh

```
cat = [0.2, 0.5, 0.8, ..., 0.3]  (50 dimensi)
tiger = [0.25, 0.48, 0.75, ..., 0.35]  (50 dimensi)

cosine_similarity(cat, tiger) = 0.6  → lumayan mirip! ✅
```

### Properties Keren

1. **Semantic similarity**
   - "King" mirip "Ruler"
   - "Cat" mirip "Tiger"

2. **Analogi**
   - King - Man + Woman ≈ Queen
   - Strong - Stronger = Weak - Weaker
   - Japan - Tokyo = Indonesia - Jakarta

3. **Context awareness**
   - Model ngerti konteks kata dalam kalimat

---

## 📚 GloVe (Pre-trained Word Embedding)

### Load GloVe

```python
from gensim.models import KeyedVectors

# Load model GloVe 50 dimensi
wv = KeyedVectors.load_word2vec_format('glove.6B.50d.txt')

# Embed kata
cat_vector = wv['cat']  # Hasil: array 50 dimensi
print(len(cat_vector))  # 50
```

### Cari Kemiripan

```python
# Similarity
wv.similarity('cat', 'dog')  # 0.76 - mirip!
wv.similarity('cat', 'economy')  # 0.1 - gak nyambung

# Kata paling mirip
wv.most_similar('cat', topn=5)
# Hasil: dog, pet, cute, kitten, etc.
```

### Analogi

```python
# King : Man = Queen : ?
wv.most_similar(positive=['king', 'woman'], negative=['man'])
# Hasil: queen, monarch, princess, ...

# Drink : Drank = Eat : ?
wv.most_similar(positive=['drink', 'eaten'], negative=['eat'])
# Hasil: drank, drunk, drinking, ...
```

---

## 🏗️ Word2Vec - Bikin Model Sendiri!

**Word2Vec** (2013) = metode untuk training word embedding yang ngerti konteks

### Kenapa Word2Vec?

**Masalah sebelumnya:**
- TF-IDF, Bag of Words → gak ngerti konteks
- Kata dengan konteks sama gak keliatan hubungannya

**Contoh:**
```
"The king ordered the citizen to leave the city"
"The ruler ordered the citizen to leave the city"
```
→ King ≈ Ruler (konteks sama!)

```
"Fish swim in the water"
"Water is home to many fish"
"The fish is dead because of the poisoned water"
```
→ Fish & Water selalu muncul bareng = ada hubungan!

### 2 Metode Word2Vec

#### 1️⃣ Skip-Gram

**Konsep:** Dari 1 kata input → prediksi kata sekitarnya

**Contoh:**
```
Kalimat: "Ruang Guru have two offices in South Jakarta"
```

**Context Window Size = 2**

Input: `Guru` → Output: `Ruang`, `have`, `two`, `offices`

**Cara kerja:**
- Input kata di-encode (one-hot)
- Masuk Dense Layer
- Masuk Projection Layer
- Prediksi beberapa kata (sesuai window size)
- Update weights

**Skip** = loncat kata, ambil yang jauh
**Gram** = kata

#### 2️⃣ Continuous Bag of Words (CBOW)

**Konsep:** Dari banyak kata sekitar → prediksi 1 kata tengah

**Contoh:**
```
Kalimat: "Ruang Guru have two offices in South Jakarta"
```

**Context Window Size = 2**

Input: `have`, `two`, `in`, `South` → Output: `offices`

**Cara kerja:**
- Ambil 2 kata depan + 2 kata belakang
- Semua di-encode
- Masuk Dense Layer
- Linear + Softmax
- Prediksi kata tengah
- Update weights

**Lebih simpel** dari Skip-Gram! ✨

### Implementasi dengan GenSim

```python
from gensim.models import Word2Vec

# Corpus (list of tokenized sentences)
corpus = [
    ['I', 'love', 'ice', 'cream'],
    ['ice', 'cream', 'is', 'delicious'],
    # ...
]

# Training
model = Word2Vec(
    corpus,
    min_count=1,      # kata minimal muncul 1x
    vector_size=100,  # ukuran embedding
    window=2          # context window size
)

# Prediksi
model.wv.most_similar('ice')  # cream, cold, etc.
model.wv.similarity('ice', 'cream')  # 0.8+
```

### Bikin dari Scratch dengan PyTorch!

#### Model CBOW

```python
import torch
import torch.nn as nn

class CBOWModel(nn.Module):
    def __init__(self, vocab_size, embedding_dim, context_size):
        super().__init__()
        # Embedding layer
        self.embeddings = nn.Embedding(vocab_size, embedding_dim)
        
        # Dense layer: (context_size * 2) kata × embedding_dim
        self.linear1 = nn.Linear(context_size * 2 * embedding_dim, 128)
        
        # Output layer
        self.linear2 = nn.Linear(128, vocab_size)
        
    def forward(self, inputs):
        embeds = self.embeddings(inputs).view((1, -1))
        out = torch.relu(self.linear1(embeds))
        out = self.linear2(out)
        log_probs = torch.log_softmax(out, dim=1)
        return log_probs
```

#### Training

```python
# Inisialisasi
vocab_size = 23
embedding_dim = 100
context_size = 2

model = CBOWModel(vocab_size, embedding_dim, context_size)
optimizer = torch.optim.SGD(model.parameters(), lr=0.001)

# Training loop
for epoch in range(100):
    total_loss = 0
    for context, target in training_data:
        model.zero_grad()
        log_probs = model(context)
        loss = loss_function(log_probs, target)
        loss.backward()
        optimizer.step()
        total_loss += loss.item()
    
    print(f'Epoch {epoch}, Loss: {total_loss}')
```

#### Prediksi

```python
# "I love ___ cream"
context = ['I', 'love', 'cream']
prediction = model(context_to_tensor(context))
# Hasil: "ice"

# "The ice ___ is delicious"
context = ['the', 'ice', 'is', 'delicious']
prediction = model(context_to_tensor(context))
# Hasil: "cream"
```

#### Ambil Embedding

```python
# Ambil embedding dari model
embeddings = model.embeddings.weight.data
# Shape: (vocab_size, embedding_dim)

# Embedding untuk kata tertentu
word_idx = vocab['ice']
ice_embedding = embeddings[word_idx]
```

---

## 🎯 Tips & Best Practices

### Training Word2Vec

1. **Dataset besar = hasil bagus**
   - Min 1 Wikipedia article
   - Atau corpus spesifik (misal: artikel biologi untuk term medis)

2. **Parameter penting:**
   - `min_count`: 2-5 (filter kata jarang)
   - `vector_size`: 100-300 (balance ukuran & akurasi)
   - `window`: 2-5 (konteks sekitar)

3. **Padding untuk edge cases**
   - Kata di awal/akhir kalimat
   - Pakai token khusus: `<PAD>`, `<START>`, `<END>`

### Dimensionality Reduction (Visualisasi)

Embedding 100-300 dimensi gak bisa divisualisasi langsung

**Solusi:**
- **PCA** - Principal Component Analysis
- **t-SNE** - t-Distributed Stochastic Neighbor Embedding

Reduksi ke 2D/3D biar bisa di-plot!

---

## 📊 Perbandingan Metode

| Metode | Dimensi | Konteks | Similarity | Use Case |
|--------|---------|---------|------------|----------|
| **ASCII** | - | ❌ | ❌ | Encoding dasar |
| **One-Hot** | Vocab size | ❌ | ❌ | Data kategorikal |
| **TF-IDF** | Vocab size | ❌ | ⚠️ | Klasifikasi sederhana |
| **Word2Vec** | 100-300 | ✅ | ✅ | NLP modern |
| **GloVe** | 50-300 | ✅ | ✅ | Pre-trained umum |

---

## 🚀 Kapan Pakai Apa?

### One-Hot Encoding
- Data kategorikal (gender, kategori produk)
- Jumlah kategori sedikit (<50)

### Pre-trained (GloVe)
- Proyek cepat
- Bahasa Inggris
- Gak punya banyak data

### Word2Vec Custom
- Domain spesifik (medis, legal, dll)
- Punya corpus besar
- Bahasa Indonesia atau bahasa lain
- Perlu embedding optimal untuk kasus khusus

---

## 💡 Fun Facts

1. **Analogi Word Embedding:**
   - Paris - France + Italy ≈ Rome
   - Windows - Microsoft + Apple ≈ MacOS
   - Coffee - Hot + Cold ≈ Iced Coffee

2. **Cosine Similarity di Real Life:**
   - Recommendation systems
   - Search engines
   - Document similarity
   - Plagiarism detection

3. **Word2Vec → FastText → BERT → GPT**
   - Word2Vec (2013)
   - FastText (2016) - handle typo & rare words
   - BERT (2018) - bidirectional context
   - GPT (2018-now) - generative AI

---

## 📝 Summary

1. **ASCII** = encoding dasar, gak ngerti makna
2. **One-Hot** = simpel tapi sparse & gak ngerti konteks
3. **Vektor & Cosine Similarity** = dasar untuk ukur kemiripan
4. **Word Embedding** = kata jadi vektor bermakna
5. **Word2Vec** ada 2 jenis:
   - **Skip-Gram**: 1 kata → banyak kata sekitar
   - **CBOW**: banyak kata sekitar → 1 kata tengah
6. **GloVe** = pre-trained, tinggal pakai
7. **Custom Word2Vec** = training sendiri dengan PyTorch/Gensim

---

## 🔗 Resources

- **Dataset:** Hugging Face (Wikipedia dumps)
- **Library:** Gensim, PyTorch, Scikit-learn
- **Pre-trained:** GloVe, FastText, Word2Vec Google News

---

**Next:** RNN, LSTM, dan Transformer! 🚀