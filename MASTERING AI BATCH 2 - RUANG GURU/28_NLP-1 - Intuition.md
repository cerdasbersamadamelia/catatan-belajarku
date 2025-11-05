# NLP-1 - Intuition

---

## 🎯 Pengenalan NLP

### NLP sebagai "Magic Box" 🎩
- Model NLP (seperti ChatGPT) bisa dianggap sebagai **black box**
- Kita kasih pertanyaan → model jawab → seperti magic!
- Tapi ada proses di dalamnya yang perlu dipahami

### Fakta Penting: Komputer Cuma Ngerti Angka! 🔢
- Semua mesin (dari M3 Apple sampai Intel/AMD) **hanya mengerti angka**
- Komputer gak ngerti string, bahasa Inggris, atau bahasa Indonesia
- **Proses NLP:**
  1. Input (teks) → Convert jadi angka
  2. Diproses sama model
  3. Output (angka) → Convert balik jadi teks

---

## 📚 Roadmap Pembelajaran NLP

Kita akan belajar NLP dari yang sederhana ke yang canggih:

1. **NLP tanpa Neural Network** (zaman dulu)
2. **Word Embedding** dan teman-temannya
3. **RNN & LSTM** (sebelum Transformer)
4. **Transformer & Attention Mechanism** (State of the art!)

### Materi Hari Ini:
1. ✅ Preprocessing
2. ✅ Tokenization
3. ✅ Word Embedding (dasar)

---

## 🔄 Flow Kerja NLP Model

### Alur Umum ChatGPT:

```
User Input (String) 
    ↓
Convert ke Number
    ↓
ENCODER → Pahami konteks input
    ↓
DECODER → Generate jawaban
    ↓
Translate ke teks
    ↓
Jawaban (String)
```

**Encoder**: Tools untuk model mengerti input user
**Decoder**: Tools untuk generate output/jawaban

---

## 🎨 Encoder & Embedding

### Tokenizer vs Embedding - Bedanya Apa?

#### Tokenizer
- **Fungsi**: Ubah teks jadi angka (ID token)
- **Output**: Angka ID sederhana
- **Contoh**: "Good morning" → [839, 662]
- **Catatan**: Angka 0 dan 2 (awal & akhir) diabaikan dulu

#### Embedding 
- **Fungsi**: Representasi kata dalam bentuk **vektor angka**
- **Output**: Vektor panjang (misal 1024 dimensi)
- **Contoh**: "Good" → [0.1757, -0.3421, 0.8901, ...]
- **Penting**: Ini yang dipahami model!

### Kenapa Perlu Embedding?

❌ **Tokenizer aja gak cukup:**
- Gak bisa tahu kata "good" dan "better" itu mirip
- Gak bisa cari hubungan antar kata

✅ **Embedding bisa:**
- Representasi kata dalam vektor
- Kata mirip → vektor dekat
- Model bisa pahami konteks!

---

## 📊 Contoh Embedding dalam Action

### Percobaan dengan Model BART:

**Kata "Good" sendiri:**
```
good → [0.1757, ...]
```

**"Good" dalam konteks berbeda:**
```
Good morning  → [0.87, ...]
Good night    → [0.8048, ...]
Very good     → [0.1234, ...]
Good engineer → [0.4348, ...]
```

**Kesimpulan**: Kata yang sama bisa punya vektor berbeda tergantung konteks!

---

## 🧮 Dimensi Embedding

### Apa itu Dimensi?
- **Dimensi** = panjang array/vektor
- Contoh: Shape `[1, 4, 1024]`
  - Batch: 1
  - Token: 4 (jumlah token)
  - Dimensi: 1024 (panjang vektor per token)

### Semakin Tinggi Dimensi:
✅ **Pro**: Lebih akurat, representasi lebih kaya
❌ **Cons**: Butuh komputasi lebih besar (CPU, RAM, GPU)

**Pilihan Dimensi:**
- GloVe: 50, 100, 200, 300
- BART: 1024
- BERT: 768

---

## 🌍 Word Embedding: GloVe

### Apa itu GloVe?
- **Global Vectors for Word Representation** (dari Stanford)
- Word embedding yang sudah jadi (pre-trained)
- Gak perlu training sendiri!

### Dataset GloVe:
- Wikipedia 2014 + Gigaword 5: **6 billion tokens**
- Common Crawl: **840 billion tokens**
- Sudah ditraining dengan algoritma khusus

### Cara Pakai GloVe:

```python
# Load GloVe
glove = load_glove('glove.6B.50d')

# Ambil embedding
embedding = glove['good']
# Output: [0.12099, -0.34521, ...]
```

### ⚠️ Kekurangan GloVe:

1. **Angka Tetap (Statis)**
   - "Good morning" → good = [0.12099, ...]
   - "Good night" → good = [0.12099, ...] (sama!)
   - Gak peduli konteks

2. **Kalau Kata Gak Ada di Dataset → Error!**
   - Kata bahasa daerah: ❌
   - Typo: ❌
   - Kata baru: ❌

3. **Solusi**: Pakai Transformer (dinamis, pahami konteks!)

---

## 🔤 Tokenization

### Tokenization Dasar
- **Fungsi**: Pisah kalimat jadi token (kata/subkata)
- **Contoh sederhana**: Split by space
  ```
  "Good morning" → ["Good", "morning"]
  ```

### BPE (Byte Pair Encoding) 🔥

**Keunggulan BPE:**
- Dipakai ChatGPT, BERT, dan model modern
- **Bisa handle kata yang gak ada di vocab!**

**Contoh Magic BPE:**
```
Input: "turutin ular"
Output: ["tur", "##ut", "##in", "ular"]
```

Tanda `##` = ada kata sebelumnya (lanjutan)

### 💡 Tips Penting Tokenization:

#### 1. Pilih Model Sesuai Bahasa!

**Bahasa Indonesia dengan Model English:**
```
"Badan pemeriksaan keuangan"
→ 17 tokens 😱
```

**Bahasa Indonesia dengan Model Indo:**
```
"Badan pemeriksaan keuangan"
→ 7 tokens ✅
```

#### 2. Pakai Bahasa Inggris Lebih Hemat Token!

**Bahasa Indonesia:**
```
"Pemeriksa Keuangan" → 7 tokens
```

**Bahasa Inggris:**
```
"Financial examiner" → 2 tokens ✨
```

**Hemat token = Hemat biaya API!** 💰

---

## 🧹 Preprocessing Data Teks

### Kenapa Perlu Preprocessing?

Model lama (non-Transformer) butuh data **bersih** karena:
- Kalau kata gak ada di vocab → gak jalan
- Noise (emoji, tagar, URL) ganggu akurasi

### Contoh Input Kotor:

```
"Super excited to share my latest article! 🔥 
@OpenAI #AI #MachineLearning https://example.com"
```

### Langkah Cleaning:

#### 1. **Hapus Noise**
Hapus:
- URL (https://...)
- Mention (@username)
- Hashtag (#AI)
- Emoji (🔥, 😊)
- Simbol khusus

**Hasil:**
```
"Super excited to share my latest article"
```

#### 2. **Hapus Tanda Baca**
```
"Super excited to share my latest article"
(tanda seru sudah hilang)
```

#### 3. **Hapus Double Space**
```
"Super excited to share my latest article"
(rapi!)
```

---

## ✂️ Stemming vs Lemmatization

### Stemming

**Konsep**: Potong akhiran/awalan kata → balik ke bentuk dasar

**Contoh:**
```
exciting  → excite
happiness → happi
worried   → worri
```

**✅ Kelebihan:**
- Cepat
- Bagus untuk regular verb (kata beraturan)

**❌ Kekurangan:**
- **Gak bisa handle irregular verb!**
  ```
  went → went (harusnya → go) ❌
  better → better (harusnya → good) ❌
  best → best (harusnya → good) ❌
  ```

---

### Lemmatization ⭐

**Konsep**: Ubah kata ke bentuk dasar yang **benar** secara linguistik

**Contoh:**
```
went → go ✅
better → good ✅
best → good ✅
hanging → hang ✅
```

**✅ Kelebihan:**
- Lebih akurat
- Handle irregular verb
- Hasil lebih natural

**❌ Kekurangan:**
- Butuh **Part of Speech (POS)** tagging
- Komputasi lebih berat

---

### Part of Speech (POS) Tagging

**Fungsi**: Tentukan jenis kata (kata benda, kerja, sifat, dll)

**Tag Bank:**
- **NN**: Noun (kata benda)
- **VB**: Verb (kata kerja)
- **JJ**: Adjective (kata sifat)
- **RB**: Adverb (kata keterangan)

**Contoh Proses:**

```python
Input: "The striped bats are hanging on their feet"

# Tokenize
tokens = ["The", "striped", "bats", "are", "hanging", ...]

# POS Tagging
pos = [("The", "DT"), ("striped", "JJ"), ("bats", "NNS"), ...]

# Lemmatization (pakai POS)
output = ["the", "striped", "bat", "be", "hang", ...]
```

---

## 🎯 Cara Kerja Word Embedding (Intuisi)

### Visualisasi 2D:

Misalnya ada 2 dimensi: **Place** dan **Vehicle**

```
         Place  Vehicle
house     10      0      → (10, 0)
car        0     10      → (0, 10)
ice        0      0      → (0, 0)
van        5     10      → (5, 10) ← rumah + kendaraan!
train      5     10      → (5, 10)
```

**Plot di grafik:**
```
Vehicle
  10 |    car●  van●  train●
     |
   5 |
     |
   0 |___ice●___________house●
     0    5              10    Place
```

**Insight:**
- Kata dengan makna mirip → posisi dekat
- Bisa dikelompokkan (clustering)
- Model bisa tahu hubungan kata!

---

### Embedding Dimension yang Tinggi

- Real embedding: **50-1024 dimensi** (gak bisa divisualisasi)
- Butuh **Aljabar Linear** untuk hitung jarak
- Metode: Cosine similarity, Euclidean distance, dll
- **Semakin dekat = semakin mirip makna**

---

## 🤖 Text Classification (Sentimen Analysis)

### Konsep Dasar:

**Goal**: Klasifikasi teks → Positif/Negatif/Netral

**Cara Kerja:**
1. Kata-kata di-embed jadi vektor
2. Hitung posisi vektor kata dalam "sentiment space"
3. Pakai **Naive Bayes** (statistical ML) untuk klasifikasi

**Contoh Sentiment Space:**

```
Positif (10, 0) ●
                 \
Netral (5, 5)     ● ← Kata "biasa"
                 /
Negatif (0, 10) ●
```

Kata "semangat" → (9, 1) → Dekat ke Positif! ✅

---

## 📖 Library NLP: NLTK

### Natural Language Toolkit

**Fungsi:**
- Preprocessing teks
- Stemming & Lemmatization
- POS Tagging
- Support **Bahasa Indonesia**! 🇮🇩

**Alternatif untuk Bahasa Indonesia:**
- **Sastrawi** (lemmatization Indonesia, tapi development stopped)
- NLTK masih yang paling populer

---

## 🎓 Ringkasan & Key Takeaways

### NLP itu "Kompleks Kalkulator"
- Semua teks → angka → kalkulasi → angka → teks
- Embedding = representasi vektor dari kata
- Context matters! (Good morning ≠ Good night)

### Tokenization Penting!
- Pilih tokenizer sesuai bahasa dataset
- BPE bisa handle kata yang gak ada di vocab
- Pakai Bahasa Inggris untuk hemat token (API berbayar)

### Preprocessing Workflow:
1. **Cleaning**: Hapus noise (emoji, URL, hashtag)
2. **Tokenization**: Split jadi token
3. **Stemming/Lemmatization**: Balik ke kata dasar
4. **Embedding**: Convert ke vektor

### Model Selection:
- **Resource terbatas**: GloVe (50-300 dimensi)
- **Akurasi tinggi**: Transformer (768-1024 dimensi)
- **Bahasa Indonesia**: Cari model yang pre-trained di dataset Indonesia

---

## 🔍 Cara Cek Dimensi Embedding Model

### Di Hugging Face:

1. Buka model page (misal: `facebook/bart-large`)
2. Klik **Files and versions**
3. Buka `config.json`
4. Cari:
   - `d_model` atau
   - `hidden_size`

**Contoh:**
```json
{
  "d_model": 1024,     // BART
  "hidden_size": 768   // BERT
}
```

---

## 💡 Tips Pro

### 1. Pilih Model Bahasa Indonesia
**Cari di Hugging Face:**
- Filter: **Language → Indonesian**
- Filter: **Task → Text Classification / Feature Extraction**
- Lihat model card → cek dataset training
- Contoh bagus: `IndoBERT`, `IndoLEM`, `IndoRoBERTa`

### 2. Cek Dataset Training
Model bagus biasanya dijelaskan:
- Dataset: Wikipedia Indonesia, Oscar, dll
- Jumlah data
- Bahasa yang dipakai

**Kalau gak dikasih tahu:**
- Baca paper-nya
- Atau langsung test dengan contoh teks!

### 3. Regex untuk Cleaning
Pakai regex untuk hapus noise:
```python
import re

# Hapus URL
text = re.sub(r'http\S+', '', text)

# Hapus mention
text = re.sub(r'@\w+', '', text)

# Hapus hashtag
text = re.sub(r'#\w+', '', text)

# Hapus emoji & simbol
text = re.sub(r'[^\w\s]', '', text)
```

---

## 📝 Special Tokens

### Token Khusus di Model:

- **BOS** (Beginning of Sentence): Awal kalimat
- **EOS** (End of Sentence): Akhir kalimat
- **<s>**: Start (di beberapa model)
- **</s>**: Stop
- **[CLS]**: BERT start token
- **[SEP]**: BERT separator

**Catatan:**
- Otomatis ditambahkan tokenizer
- Gak perlu manual tulis
- Beda model beda special token (gak standar)

---

## 🚀 Pertemuan Selanjutnya

Materi yang akan dipelajari:

1. **Text Classification** (lanjutan)
   - Hitung manual pakai sheet
   - Implementasi Naive Bayes

2. **Word Embedding Methods:**
   - TF-IDF
   - Word2Vec
   - GloVe (detail)
   
3. **Praktik lebih dalam!**

---

## ❓ FAQ & Troubleshooting

### Q: Tokenizer untuk LaTeX gimana?
**A**: 
- Model umum (ChatGPT) sudah support LaTeX
- Tapi gak optimal (banyak token)
- Kalau butuh optimal → cari model khusus LaTeX
- Atau buat tokenizer sendiri

### Q: Kata gak ada di GloVe, gimana?
**A**: 
- GloVe = kamus, gak ada = error
- Solusi: Training ulang atau pakai Transformer

### Q: Stemming vs Lemmatization pilih mana?
**A**:
- **Stemming**: Kalau butuh cepat, resource terbatas
- **Lemmatization**: Kalau prioritas akurasi

### Q: Kenapa special token gak standar?
**A**: 
- Beda research lab, beda convention
- Gak ada standarisasi universal (yet)
- Always check `config.json`!

---

## 🎯 Action Items

Setelah belajar materi ini, coba:

1. ✅ Download GloVe dan test dengan kata-kata sendiri
2. ✅ Bandingkan tokenizer BERT (English) vs IndoBERT
3. ✅ Praktik preprocessing dengan NLTK
4. ✅ Test stemming vs lemmatization
5. ✅ Eksplor Hugging Face untuk cari model Indonesia

---

**Catatan**: Materi ini fokus di **intuisi** NLP. Detail teknis (math, attention mechanism) akan dibahas di sesi selanjutnya!

🔥 **Keep learning and happy coding!** 🚀
