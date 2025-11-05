# NLP-2: Embeddings & Feature Extraction

## 📌 Overview
Materi ini membahas cara mengubah teks menjadi angka (vektor) agar bisa diproses oleh model machine learning. Ada 3 metode utama yang dipelajari:
1. **Bag of Words (BoW)**
2. **TF-IDF**
3. **Word Embeddings**

---

## 🎯 Feature Extraction di NLP

### Kenapa Butuh Feature Extraction?
- Model statistik/matematika cuma bisa ngolah **angka**, bukan teks
- Setelah preprocessing (stemming, lemmatization), teks perlu diubah jadi angka
- Bedanya sama CNN: di CNN feature extraction otomatis, tapi di NLP klasik harus manual

---

## 1️⃣ Bag of Words (BoW)

### Konsep Dasar
Cara paling simpel: kumpulin semua kata unik dari dataset, terus kasih nilai 1 (ada) atau 0 (tidak ada).

### Cara Kerja
1. Ambil semua kata unik dari dataset
2. Jadikan kata-kata itu sebagai **kolom/fitur**
3. Untuk tiap kalimat, kasih nilai:
   - **1** = kata ada di kalimat
   - **0** = kata tidak ada

### Contoh
**Dataset:**
```
1. I am feeling excited
2. I am really excited  
3. I am feeling sad
4. I really set
5. I am excited
6. I am set
```

**Kata Unik:** I, am, feeling, excited, really, set (5 kata = 5 dimensi)

**Matriks BoW:**
| Kalimat | I | am | feeling | excited | really | set |
|---------|---|----|---------|---------+--------|-----|
| I am feeling excited | 1 | 1 | 1 | 1 | 0 | 0 |
| I am really excited | 1 | 1 | 0 | 1 | 1 | 0 |
| I am set | 1 | 1 | 0 | 0 | 0 | 1 |

### Masalah BoW
❌ **Dimensi terlalu besar** jika kata unik banyak
- Contoh: GloVe punya 400,000 kata unik = 400,000 dimensi!
- Komputasi jadi berat
- Belum tentu hasilnya bagus

❌ **Tidak melihat konteks**
- Semua kata diperlakukan sama
- Kata "I am" dan "excited" punya bobot sama

---

## 2️⃣ TF-IDF (Term Frequency - Inverse Document Frequency)

### Konsep Dasar
Kasih **nilai tinggi** ke kata yang **jarang muncul**, kasih **nilai rendah** ke kata yang **sering muncul**.

### Kenapa?
- Kata seperti "I", "am", "the" sering muncul tapi tidak penting untuk klasifikasi
- Kata seperti "excited", "sad", "angry" jarang muncul tapi **penting** untuk menentukan sentimen

### Formula TF-IDF

**Step 1: Hitung Term Frequency (TF)**
- Hitung berapa kali setiap kata muncul di **seluruh dataset**

**Step 2: Hitung Inverse Document Frequency (IDF)**
```
IDF = ln(Total Dataset / Kemunculan Kata)
```

**Step 3: Kalikan TF dengan IDF**

### Contoh Perhitungan

**Dataset (6 kalimat):**
```
1. I am feeling excited
2. I am really excited  
3. I am feeling sad
4. I really set
5. I am excited
6. I am set
```

**Step 1: Hitung kemunculan kata**
| Kata | Kemunculan |
|------|-----------|
| I | 6 |
| am | 6 |
| feeling | 2 |
| excited | 3 |
| really | 2 |
| sad | 1 |
| set | 2 |

**Step 2: Hitung IDF**
| Kata | IDF = ln(6/kemunculan) | Nilai |
|------|------------------------|-------|
| I | ln(6/6) = ln(1) | 0 |
| am | ln(6/6) = ln(1) | 0 |
| feeling | ln(6/2) = ln(3) | 1.09 |
| excited | ln(6/3) = ln(2) | 0.69 |
| really | ln(6/2) = ln(3) | 1.09 |
| sad | ln(6/1) = ln(6) | 1.79 |
| set | ln(6/2) = ln(3) | 1.09 |

**Step 3: Ganti nilai BoW dengan TF-IDF**
| Kalimat | I | am | feeling | excited | really | sad | set |
|---------|---|----|---------|---------+--------|-----|-----|
| I am feeling excited | 0 | 0 | 1.09 | 0.69 | 0 | 0 | 0 |
| I am sad | 0 | 0 | 0 | 0 | 0 | 1.79 | 0 |

### Keuntungan TF-IDF
✅ Kata penting dapat **skor tinggi**
✅ Kata tidak penting (stopwords) dapat **skor 0** atau mendekati 0
✅ Bisa **membatasi dimensi** dengan `max_features`

### Implementasi di Python
```python
from sklearn.feature_extraction.text import TfidfVectorizer

# Buat TF-IDF vectorizer dengan max 1000 fitur
tfidf = TfidfVectorizer(max_features=1000)

# Fit ke corpus
tfidf.fit(corpus)

# Transform input baru
input_text = "Selamat pagi guru"
vector = tfidf.transform([input_text])
# Output: [0.23, 0.22, 0.13, ...]
```

### Cara Mengatasi 500,000 Fitur
1. Gunakan TF-IDF untuk scoring
2. Limit dengan `max_features` (contoh: 100, 300, 1000)
3. Ambil **top N kata** dengan prioritas tertinggi

---

## 3️⃣ Word Embeddings

### Konsep Dasar
Ubah kata jadi **vektor** yang bisa mengerti **konteks** dan **hubungan antar kata**.

### Keuntungan Word Embeddings
✅ Kata yang mirip punya vektor yang **berdekatan**
✅ Bisa menangkap **hubungan semantik** (contoh: king - queen, man - woman)
✅ Dimensi lebih kecil (50, 100, 300) tapi lebih bermakna

### Model Word Embeddings
- **GloVe** (Global Vectors for Word Representation)
- **Word2Vec**
- **FastText**
- **BERT, GPT** (contextualized embeddings)

---

## 📐 Cosine Similarity

### Kenapa Pakai Cosine Similarity?
Untuk mengukur **seberapa dekat** dua kata berdasarkan vektornya.

### Cara Kerja
1. Hitung **sudut** antara 2 vektor
2. Makin kecil sudutnya = makin mirip
3. Nilai cosine:
   - **1** = sangat mirip (sudut 0°)
   - **0** = tidak ada hubungan (sudut 90°)
   - **-1** = berlawanan (sudut 180°)

### Contoh
```
Kata "hitam" vs "gelap" → sudut 20° → cos = 0.8 (mirip!)
Kata "hitam" vs "putih" → sudut 179° → cos = -0.99 (berlawanan!)
```

### Visualisasi Nilai Cosine
| Sudut | Nilai Cosine | Arti |
|-------|-------------|------|
| 0° | 1 | Sama persis |
| 90° | 0 | Tidak ada hubungan |
| 180° | -1 | Berlawanan |

---

## 🔍 GloVe Word Embeddings

### Tentang GloVe
- Dataset: 6 miliar kata
- Vocabulary: 400,000 kata unik
- Dimensi: 50, 100, 200, 300 (bisa pilih)

### Contoh: Kata "King"
Kata yang **dekat** dengan "king":
- queen (0.75)
- prince (0.72)
- sultan (0.68)
- George, Charles, Henry (nama raja Inggris/Prancis)

**Kesimpulan:** GloVe bisa menangkap konteks "kerajaan" dengan baik!

### Negative Word Filtering
Bisa filter kata yang **tidak diinginkan**:
```
Cari similarity: dog + cat
Negative word: man

Hasil: groomer, bite, vet (fokus ke hewan, bukan manusia)
```

### Comparing Words
```
Input: "Hi"
Compare dengan: queen, king

Hasil:
- Hi vs King: 0.6
- Hi vs Queen: 0.5

King lebih related dengan "Hi"
```

---

## 📊 Dimensionality Reduction

### Kenapa Perlu?
- **50 dimensi** susah divisualisasikan
- Butuh **2D** untuk plot grafik

### Metode: t-SNE
- Ubah 50 dimensi → 2 dimensi (x, y)
- Tetap jaga hubungan antar kata
- Kata yang mirip tetap berdekatan di plot

### Trade-off
✅ **Keuntungan:** Komputasi lebih efisien
❌ **Kerugian:** Kehilangan sebagian informasi/konteks

---

## ⚠️ Curse of Dimensionality

### Masalah Dimensi Terlalu Banyak
❌ **Vanishing Gradient**
- Gradient terlalu **kecil**
- Training jadi **lambat**
- Sulit mencapai global optimum

### Masalah Dimensi Terlalu Sedikit
❌ **Exploding Gradient**
- Gradient terlalu **besar**
- Training loss **tidak stabil**
- Tidak bisa mencapai global optimum

### Solusi: Learning Rate Scheduler
```
Awal training → Learning rate BESAR (cepat turun)
Tengah training → Learning rate SEDANG
Akhir training → Learning rate KECIL (fine-tuning)
```

---

## 🎭 Context-Aware Embeddings (Transformer)

### Masalah TF-IDF
Tidak bisa bedakan konteks:
```
❌ "I am not sad" → "sad" tetap dapat skor tinggi (padahal ada "not")
❌ "I am crying out of joy" → "crying" dikira negatif (padahal karena senang)
```

### Solusi: Transformer Models
Model seperti **BERT, GPT** bisa:
✅ Pahami konteks **seluruh kalimat**
✅ Bedakan "bat" (hewan) vs "bat" (pemukul baseball)
✅ Tangkap negasi ("not", "never")

### Contoh Demo
```
Input: "I love this bat because it's grip"
Compare: animal bat vs baseball bat

Hasil: baseball bat (0.85) > animal bat (0.23)

Model BERT paham "grip" → alat olahraga, bukan hewan!
```

---

## 🛠️ Workflow NLP (Lengkap)

```
1. Collect Data
   ↓
2. Data Cleaning (lowercase, remove punctuation)
   ↓
3. Preprocessing (stemming, lemmatization)
   ↓
4. Feature Extraction
   - Bag of Words
   - TF-IDF
   - Word Embeddings
   ↓
5. Model Training (Naive Bayes, SVM, Random Forest)
   ↓
6. Inference/Prediction
```

---

## 💡 Kapan Pakai Metode Apa?

### Bag of Words
👍 **Pakai kalau:**
- Dataset kecil
- Butuh cepat dan simpel
- Kata unik tidak terlalu banyak

### TF-IDF
👍 **Pakai kalau:**
- Ingin filter stopwords otomatis
- Dataset medium (1000-10000 kalimat)
- Butuh fokus ke kata-kata penting

### Word Embeddings
👍 **Pakai kalau:**
- Dataset besar
- Butuh tangkap hubungan semantik
- Masalah kompleks (sentiment analysis, NER)

### Transformer (BERT, GPT)
👍 **Pakai kalau:**
- Butuh pahami konteks kalimat
- Ada budget komputasi tinggi
- Akurasi prioritas utama

---

## 📝 Catatan Penting

### Kualitas Dataset = Kunci!
- TF-IDF butuh dataset **beragam** (bukan seragam)
- Minim 1000-2000 kalimat untuk hasil general
- Dataset imbalanced → model bias

### TF-IDF vs LLM
Tidak semua masalah butuh LLM!
- **Spam filtering** → TF-IDF + keyword matching (cukup!)
- **Sentiment analysis sederhana** → TF-IDF + Naive Bayes
- **Conversational AI** → Butuh LLM (GPT, BERT)

### Trade-off: Cost vs Performance
```
Simple (Murah, Cepat):     BoW → TF-IDF → Word2Vec
Complex (Mahal, Akurat):   GloVe → BERT → GPT
```

Cari **titik optimal** antara biaya komputasi dan performa!

---

## 🔗 Resources

### Model & Dataset
- **GloVe:** Pre-trained word embeddings (6B token, 400k vocab)
- **Scikit-learn:** `TfidfVectorizer` untuk TF-IDF
- **Gensim:** Library untuk Word2Vec
- **Hugging Face:** Model BERT, GPT, dll

### Library Python
```python
# TF-IDF
from sklearn.feature_extraction.text import TfidfVectorizer

# Dimensionality Reduction
from sklearn.manifold import TSNE

# Word Embeddings
import gensim
from transformers import BertModel
```

---

## ❓ FAQ

**Q: Gimana ngurangin 500,000 fitur?**
A: Pakai `max_features` di TfidfVectorizer (set 100-1000)

**Q: Kenapa "I am" dapat skor 0 di TF-IDF?**
A: Karena muncul di semua kalimat → ln(6/6) = 0 → tidak penting

**Q: TF-IDF bisa detect "not sad"?**
A: Tidak! TF-IDF cuma hitung frekuensi, tidak paham konteks. Butuh Transformer.

**Q: Dimensi embedding 50 vs 300, mending mana?**
A: 
- **50:** Lebih cepat, cocok dataset kecil
- **300:** Lebih detail, cocok dataset besar

**Q: GloVe vs Word2Vec?**
A:
- **GloVe:** Pakai statistik global corpus
- **Word2Vec:** Pakai context window lokal
- Performa mirip, pilih sesuai kebutuhan

---

## 🎯 Ringkasan

### Yang Dipelajari
✅ Bag of Words → Cara paling simpel (1/0)
✅ TF-IDF → Kasih prioritas ke kata penting
✅ Word Embeddings → Vektor yang paham konteks
✅ Cosine Similarity → Ukur kedekatan kata
✅ Dimensionality Reduction → Kurangi dimensi tanpa loss info terlalu banyak

### Next Steps
- **Attention Mechanism** (Jumat) → Model yang BENAR-BENAR paham konteks
- **Transformer** → BERT, GPT, dan teman-temannya
- **Fine-tuning LLM** → Sesuaikan model pre-trained ke task spesifik