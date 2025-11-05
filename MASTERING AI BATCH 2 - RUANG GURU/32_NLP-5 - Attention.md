# NLP-5: Attention Mechanism

## 🎯 Overview
Attention mechanism adalah mekanisme **game-changing** dalam NLP! Sejak munculnya Attention, riset NLP meledak dan mengalahkan Computer Vision.

---

## ❌ Problem dengan RNN

### 1. **Tidak Bisa Paralel (Sequential Processing)**
- RNN memproses input **satu per satu** secara berurutan
- Harus menunggu proses sebelumnya selesai
- Contoh: "I am a student" → harus proses "I" → "am" → "a" → "student"
- Kompleksitas: O(n × d_model)
- **Boros komputasi!**

**Analogi:**
- RNN: Menghitung panjang kata satu-satu → 11 kali operasi
- Yang diinginkan: Hitung semua secara paralel → langsung dikumpulin

### 2. **Vanishing Gradient**
- Gradient terlalu kecil → model tidak bisa belajar dengan baik
- Tidak bisa mencapai global optimum

### 3. **Exploding Gradient**
- Gradient terlalu besar
- Sama-sama menghambat pembelajaran

### 4. **Sequential Dependency**
- Setiap time step bergantung pada time step sebelumnya
- Tidak bisa skip atau proses langsung

---

## ✅ Kenapa CNN Lebih Baik?

CNN bisa memproses **semua input secara paralel**. Tidak satu-satu seperti RNN.

---

## 🎯 Solusi: Attention Mechanism

### Yang Kita Mau:
1. ✅ Model yang bisa memproses input **secara paralel**
2. ✅ Performa **lebih baik** daripada RNN
3. ✅ **Menghilangkan** vanishing & exploding gradient

---

## 💡 Konsep "Attention is All You Need"

Model bisa **fokus ke input tertentu** yang paling penting!

**Contoh:**
```
Kalimat: "Nama saya Muhammad Agung Hambali. Saya kerja di Ruang Guru. 
          Saya suka makan bakso dan bermain game. Saya tidak suka 
          makanan di tempat ini."

Untuk sentiment analysis → Yang penting: "Saya tidak suka makanan di tempat ini"
```

Model harus bisa fokus ke kalimat akhir, **tidak perlu fokus ke info awal** (nama, pekerjaan, dll).

---

## 🔍 Contoh Attention

**Kalimat:** "Father's passport was stolen, so he went to the police station to get a new one."

### Model Harus Tahu:
- **"he"** → mengacu ke **"father"**
- **"stolen"** → relate ke **"passport"**
- **"a new one"** → mengacu ke **"passport"**

### Perbandingan:
- **Word Embedding biasa (GloVe):** Kurang bagus, banyak yang missing
- **Model dengan Attention (BERT):** Lebih baik! Bisa tangkap relasi antar kata

**Visualisasi:**
- Warna **merah/terang** = Model fokus di sini
- Warna **biru/gelap** = Model tidak fokus

---

## 🏗️ Arsitektur Transformer

### Komponen Utama:
1. **Encoder** (kiri)
2. **Decoder** (kanan)

---

## 📥 Input Processing

### 1. **Input → Tokenization**
```
"hello world" → [token_30, token_31]
```

### 2. **Input Embedding Layer**
- Menyimpan **pair antara kata dan vektor representasinya**
- Dimensi matriks: `d_model × jumlah_vocab`
- Contoh BERT Base: `768 × 30,223`
- Paper Attention: `512 × vocab_size`

Output: `[2 × 768]` untuk 2 token

### 3. **Positional Encoding**
**Fungsi:** Memberitahu model **posisi token** (awal/tengah/akhir kalimat)

**Kenapa Penting?**
- Kata "layer" di posisi berbeda = embedding berbeda
- Kata "dan" di kalimat 1 ≠ kata "dan" di kalimat 2

**Dimensi:** Sama dengan embedding → `d_model`

**Cara Kerja:**
```
Embedding + Positional Encoding (per indeks yang sama)
```

**Rumus Positional Encoding:**
- Posisi genap: `PE(pos, 2i) = sin(pos / 10000^(2i/d_model))`
- Posisi ganjil: `PE(pos, 2i+1) = cos(pos / 10000^(2i/d_model))`

**Catatan:** 
- Positional encoding **hanya dibuat sekali** saat inisialisasi model
- Tidak perlu dibuat ulang setiap training/inference

---

## 🧠 Encoder Block

### Komponen:

#### 1. **Multi-Head Attention**

##### A. Attention Head (Single Head)

**Query, Key, Value - Analogi YouTube:**
- **Query (Q):** Pencarian kamu di YouTube ("attention is all you need")
- **Key (K):** Thumbnail video / judul yang muncul
- **Value (V):** Isi video sebenarnya saat kamu klik

**Proses:**
1. Output positional encoding × **W_Q** → **Query**
2. Output positional encoding × **W_K** → **Key**
3. Output positional encoding × **W_V** → **Value**

**Dimensi Weight:**
```
d_k = d_model / H
Contoh: 512 / 8 = 64

W_Q, W_K, W_V = [64 × 512]
```

**Formula Attention:**
```
Attention(Q, K, V) = softmax(Q × K^T / √d_k) × V
```

**Tahapan:**
1. Q × K^T (matrix multiplication)
2. Hasil / √d_k (scaling)
3. Softmax (dapat probability 0-1)
4. × Value (final attention score)

**Output:** Matriks skor relasi antar token

**Contoh Skor:**
```
         X1    X2    X3    X4
X1     0.8   0.1   0.05  0.05
X2     0.2   0.7   0.05  0.05
X3     0.1   0.1   0.6   0.2
X4     0.01  0.01  0.1   0.88
```

##### B. Multi-Head Attention

**Jumlah Head:** Contoh paper = **8 heads**

**Kenapa Perlu Banyak Head?**
- Setiap head fokus pada **aspek berbeda**
- Seperti diskusi dengan banyak orang → dapat banyak perspektif

**Contoh (Image Classification):**
- Head 1 → fokus ke matahari
- Head 2 → fokus ke burung
- Head 3 → fokus ke gunung
- Head 4 → fokus ke pohon
- dst...

**Proses:**
1. Hitung attention untuk **setiap head** (ada 8 attention scores)
2. **Concatenate** semua hasil
3. Masukkan ke **Linear Layer**
4. Dapat **Output Multi-Head Attention**

**Keuntungan:**
- Model mengerti input dari berbagai perspektif
- Komputasi lebih rendah (karena dimensi dipecah)

#### 2. **Add & Normalization (Residual Network)**

```
Output = LayerNorm(Input + Multi-Head-Attention-Output)
```

**Fungsi:**
- Mencegah gradient terlalu tinggi/rendah
- Stabilkan training

#### 3. **Position-wise Feed Forward Network**

- Feed Forward khusus untuk data sequence
- Punya weight tertentu untuk tiap posisi

#### 4. **Add & Normalization (Lagi)**

Sama seperti sebelumnya.

**Output:** Hasil akhir dari Encoder Block

---

## 🔄 Decoder Block

### Perbedaan dengan Encoder:

#### 1. **Masked Multi-Head Attention**

**Aturan:** Token **tidak boleh melihat token di depannya** (future tokens)

**Contoh:**
```
Kalimat: "Soekarno adalah seorang presiden"

❌ Encoder: X1 bisa lihat X2, X3, X4 (Bi-directional)
✅ Decoder: X1 HANYA bisa lihat X1 (Uni-directional)
           X2 bisa lihat X1, X2
           X3 bisa lihat X1, X2, X3
```

**Implementasi:** Set nilai attention score future tokens = -∞

**Istilah:**
- **Bi-directional Encoding:** Lihat ke depan & belakang (Encoder)
- **Uni-directional Encoding:** Hanya ke kiri/belakang (Decoder)

#### 2. **Cross Attention (Multi-Head Attention kedua)**

**Input:**
- **Query (Q):** Dari decoder (Masked Multi-Head Attention)
- **Key (K):** Dari **Encoder output**
- **Value (V):** Dari **Encoder output**

**Fungsi:** Decoder bisa "lihat" informasi dari Encoder

#### 3. **Feed Forward & Normalization**

Sama seperti Encoder

#### 4. **Linear Layer + Softmax**

**Output:** **Probability distribusi** untuk setiap token di vocab

**Contoh Translation (EN → ID):**
```
Input: "hello world"

Output probabilities:
Token 30 (halo):  0.7  ← Pilih ini!
Token 90 (dunia): 0.9  ← Pilih ini!

Result: "halo dunia"
```

---

## 🎯 Keunggulan Transformer

### ✅ Mengatasi Masalah RNN:

1. **Paralel Processing** ✅
   - Semua input diproses bersamaan
   
2. **No Vanishing/Exploding Gradient** ✅
   - Ada residual network & normalization
   
3. **Better Performance** ✅
   - Attention mechanism fokus ke input penting

---

## 🏛️ Tipe Arsitektur Transformer

### 1. **Encoder-Only Architecture**

**Contoh:** BERT, RoBERTa, ALBERT

**Training Task:**
- **MLM (Masked Language Model):** Prediksi kata yang di-mask
  ```
  "Presiden Soekarno tinggal di [MASK]"
  → Prediksi: Jakarta/Bogor/Istana
  ```

- **NSP (Next Sentence Prediction):** Bedakan input vs output

**Use Case:**
- ✅ Sentiment Analysis
- ✅ Text Classification
- ✅ POS Tagging
- ✅ Question Answering (bisa, tapi kurang optimal)
- ✅ Summarization (bisa, tapi kurang optimal)

### 2. **Decoder-Only Architecture**

**Contoh:** GPT, GPT-2, GPT-3, BLOOM

**Model Type:** Causal Language Model

**Cara Kerja:** 
- Prediksi kata berikutnya → kata berikutnya → kata berikutnya...
- Generate output satu per satu
- Pakai Masked Multi-Head Attention
- **TIDAK pakai** Cross Attention

**Use Case:**
- ✅ Text Generation
- ✅ Code Generation
- ✅ Chatbot
- ✅ Story/Puisi Generation
- ✅ Caption Generation

**Catatan:** Riset sekarang kebanyakan fokus ke **Decoder-Only** (karena bisa langsung dipakai & duitnya ada di sini!)

### 3. **Encoder-Decoder Architecture**

**Contoh:** T5, BART, Marian MT

**Use Case:**
- ✅ Translation (EN → ID)
- Text Summarization
- Question Answering

**Cara Kerja:**
- Encoder: Process input
- Decoder: Generate output dengan bantuan Cross Attention

---

## 📊 Perbandingan Arsitektur

| Arsitektur | Model | Attention Type | Use Case |
|------------|-------|----------------|----------|
| **Encoder-Only** | BERT | Bi-directional | Classification, NER |
| **Decoder-Only** | GPT | Uni-directional | Generation |
| **Encoder-Decoder** | T5, BART | Bi + Uni | Translation, Summary |

---

## 🔍 Cara Cek Arsitektur Model

**Satu-satunya cara:** Baca papernya!

Config di Hugging Face biasanya tidak kasih tahu tipe arsitektur.

**Yang dicari:**
- `hidden_size` / `d_model`: Dimensi model
- `num_attention_heads`: Jumlah head
- `vocab_size`: Ukuran vocabulary
- `max_position_embeddings`: Max panjang sequence

---

## 💡 Tips & Catatan

### Kenapa Pakai Query, Key, Value?

**Analogi Google Search:**
1. **Query:** "Samsung S24" (yang kamu cari)
2. **Key:** Judul hasil pencarian ("Harga HP Samsung S24...")
3. **Value:** Isi halaman web saat diklik (detail lengkap)

### Rekomendasi Sekarang:

**Training/Development:**
- ❌ RNN/LSTM → Hampir tidak dipakai lagi ("LSTM is dead")
- ✅ Transformer → The way to go!

**Kapan Pakai RNN?**
- Hanya jika **resource terbatas** (GPU/Memory tidak cukup)
- Kalau ada pilihan → **selalu pakai Transformer**

### Optimisasi Transformer:

Sudah banyak cara optimisasi:
- Quantization
- Distillation
- Pruning
- LoRA, dll

Jadi resource bukan masalah besar lagi!

---

## 📚 Resources Penting

### Paper:
- **"Attention is All You Need"** (Vaswani et al., 2017)
- **"BERT"** (Devlin et al., 2018)

### Follow untuk Update AI:
- Twitter: Yann LeCun, Andrej Karpathy
- Lab: DeepMind, OpenAI, Meta AI, NVIDIA AI, Microsoft Research

### Belajar:
- Baca paper
- Cek job requirement (LinkedIn)
- Lihat kualifikasi ML Engineer/AI Engineer (Junior level)
- Build portfolio (1 NLP + 1 Computer Vision minimal)

---

## 🎓 Summary

### Key Takeaways:
1. **Attention = Game Changer** → Bisa fokus ke input penting
2. **Paralel Processing** → Jauh lebih cepat dari RNN
3. **No Gradient Problem** → Residual Network + Normalization
4. **3 Tipe Arsitektur:** Encoder-Only, Decoder-Only, Encoder-Decoder
5. **Riset Sekarang:** Fokus ke Decoder-Only (GPT-style)

### Flow Lengkap:
```
Input → Tokenization → Embedding → Positional Encoding
  ↓
Encoder Block (Multi-Head Attention + FFN + Normalization)
  ↓
Decoder Block (Masked Attention + Cross Attention + FFN)
  ↓
Linear + Softmax → Output Probabilities
```

**Attention is All You Need! 🚀**