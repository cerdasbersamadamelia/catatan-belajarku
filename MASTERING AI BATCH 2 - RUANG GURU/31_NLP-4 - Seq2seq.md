# NLP-4: Sequence-to-Sequence dengan RNN

## 🎯 Apa itu RNN?

**RNN = Recurrent Neural Network**

RNN muncul untuk mengatasi masalah yang ada di Neural Network biasa:

### Masalah Neural Network Biasa:
1. **Input size harus fix (tetap)**
   - Contoh: di YOLO, gambar harus 416×416
   - Kalau beda ukuran → error!
   
2. **Tidak bisa handle data yang ukurannya berubah-ubah**
   - Contoh: kalimat pendek vs kalimat panjang
   - Tidak praktis untuk production

### Solusi: RNN!

RNN bisa mengolah **data sekuensial** yang:
- ✅ Ukurannya berbeda-beda (bisa 10, 20, 300 token)
- ✅ Punya urutan yang penting
- ✅ Kalau urutan dikacaukan → artinya berubah

---

## 📊 Contoh Data Sekuensial

### Data Sekuensial ✅
- 📈 **Harga saham** - prediksi berdasarkan 30 hari sebelumnya
- 📝 **Teks/kalimat** - urutan kata penting
- 🎤 **Audio** - contoh: Whisper (max 30 detik, tapi bisa bervariasi)
- 🌡️ **Suhu & cuaca** - data time series
- 🧬 **DNA** - urutan super penting!
- 🎵 **Musik** - urutan nada

### Bukan Data Sekuensial ❌
- 🖼️ **Gambar/Image** - ukuran fix, bukan sekuensial
- 📊 **Prediksi harga rumah** - urutan tidak penting
- 🔢 **Output dari Word2Vec, TF-IDF, GloVe** - ukuran sama semua

> **Catatan:** Output embedding (Word2Vec, dll) bisa diperdebatkan. Kalau dilihat per kata memang sekuensial, tapi ukuran outputnya tetap.

---

## 🧠 Intuisi RNN

### Analogi: Soal SBMPTN/SNBT
```
1, 1, 2, 3, 5, 8, 13, 21, ???
```

Cara kita ngerjain:
1. Lihat 1 + 1 = 2
2. Lihat 1 + 2 = 3
3. Lihat 2 + 3 = 5
4. Dan seterusnya... **dari awal sampai akhir**

Jawabannya: **34** (karena 13 + 21 = 34)

**RNN juga kerja seperti ini** → memproses dari awal sampai akhir, informasi dibawa terus!

### Contoh di NLP:
**Kalimat:** "I love spicy food"

Model harus ngerti:
- "food" → objeknya
- "spicy" → sifat dari food
- "love" → predikatnya
- "I" → subjeknya

Jadi: **"Saya suka makanan pedas"** ← model harus paham konteks keseluruhan!

---

## 🔧 Cara Kerja RNN

### Komponen Penting:

1. **Time Step** = urutan ke-berapa (pertama, kedua, ketiga, dst)
2. **Input** = data yang masuk di setiap time step
3. **RNN Cell** = tempat pemrosesan
4. **Hidden State** = output dari RNN Cell (bentuknya matrix)
5. **Activation Function** = biasanya **tanh** atau **relu**

### Alur Kerja:

```
Input 1 → RNN Cell → Hidden State 1 → dioper ke...
Input 2 + Hidden State 1 → RNN Cell → Hidden State 2 → dioper ke...
Input 3 + Hidden State 2 → RNN Cell → Hidden State 3 → ...
```

**Key Point:** Output dari time step sebelumnya dipakai di time step selanjutnya!

---

## 📐 Formula RNN

```
h_t = f(W_hh × h_(t-1) + W_xh × x_t + bias)
```

Penjelasan:
- `h_t` = hidden state di time step sekarang
- `h_(t-1)` = hidden state di time step sebelumnya
- `x_t` = input di time step sekarang
- `W_hh` = weight untuk hidden state
- `W_xh` = weight untuk input
- `f` = activation function (tanh/relu)

> **Penting:** Weight (W) sama untuk semua time step!

---

## 🏗️ Tipe-Tipe Arsitektur RNN

### 1. One-to-One
- Input: 1
- Output: 1
- Contoh: Neural Network biasa

### 2. One-to-Many
- Input: 1 (misal: gambar)
- Output: banyak (misal: caption)
- Contoh: **Image Captioning**, Text Generation

### 3. Many-to-One
- Input: banyak (misal: kalimat)
- Output: 1 (misal: sentimen)
- Contoh: **Sentiment Analysis**, Text Classification

### 4. Many-to-Many
- Input: banyak
- Output: banyak
- Contoh: **Translation**, Music Generation

---

## 🔄 Encoder-Decoder Architecture

### Encoder (Many-to-One)
**Fungsi:** Memahami input dengan jelas!

Contoh input: "Nama saya adalah Agung"

Proses:
1. "Nama" → diproses
2. "saya" → diproses + info dari "Nama"
3. "adalah" → diproses + info dari "Nama saya"
4. "Agung" → diproses + info dari keseluruhan

**Output:** Hidden state terakhir yang udah ngerti semua konteks!

### Decoder (One-to-Many)
**Fungsi:** Generate output satu per satu!

Dapat hidden state dari encoder → generate:
1. `<BOS>` (Beginning of Sentence) → "My"
2. "My" → "name"
3. "name" → "is"
4. "is" → "Agung"
5. "Agung" → `<EOS>` (End of Sentence)

> **Makanya Chat GPT keluar satu-satu!** Bukan langsung semuanya. Ini cara kerja decoder.

---

## 🎓 Sequence-to-Sequence (Seq2Seq)

**Definisi:** Input berupa sequence → Output juga berupa sequence

### Contoh Use Case:
- 🌐 **Translation** - Bahasa Indonesia → Bahasa Inggris
- 💬 **Chatbot** - Pertanyaan → Jawaban
- 🎤 **Speech-to-Text** - Audio → Teks
- 📝 **Text Summarization** - Teks panjang → Ringkasan

### Arsitektur:
```
Input Sequence → ENCODER → Context Vector → DECODER → Output Sequence
```

**Context Vector** = Hidden state terakhir dari encoder yang berisi pemahaman lengkap tentang input!

---

## 🔥 Back Propagation Through Time (BPTT)

Cara RNN belajar dari kesalahan:

1. Hitung loss di setiap output
2. Kalau many-to-many → loss di setiap time step dijumlahkan
3. Hitung gradient dari akhir ke awal
4. Update weight berdasarkan gradient

**Catatan:** Weight sama untuk semua time step, cuma di-update bareng!

---

## ⚠️ Masalah RNN & Solusinya

### 1. **Vanishing Gradient** (Hidden state terlalu kecil)
Solusi:
- ✅ Ganti tanh dengan **ReLU**
- ✅ Pakai **Identity Matrix** untuk inisialisasi hidden state
- ✅ Upgrade ke **GRU** (Gated Recurrent Unit)
- ✅ Upgrade ke **LSTM** (Long Short-Term Memory)

### 2. **Exploding Gradient** (Hidden state terlalu besar)
Solusi:
- ✅ **Gradient Clipping** - batasi nilai gradient

---

## 💡 Poin Penting

### Hidden State
- ❓ **Output atau bukan?** → Tergantung!
  - Bisa jadi output terakhir (many-to-one)
  - Bisa jadi operan info antar time step
  - Bentuknya: **Matrix (m × n)**

### Weight di RNN
- Sama untuk semua time step
- Kalau training dari scratch → diinisialisasi **random**
- Kalau pakai pre-trained → ambil dari model yang sudah ada

### Fully Connected Layer
- **Optional!** Bisa dipakai, bisa tidak
- Biasanya untuk classification task

---

## 📝 Contoh Praktis (Spreadsheet)

### Input: "Aku suka cokelat"

**Vocab:**
- aku, suka, tidak, cokelat, jus, `<BOS>`

**Embedding:** One-hot encoding

**Weight preference:**
- `<BOS>`: 3 (sisanya 0)
- suka: 0.1
- tidak: -0.5
- cokelat: 5 (positif)
- jus: negatif

### Proses:
**Time Step 1:** "Aku"
- Input: [1, 0, 0, 0, 0, 0]
- Hidden state sebelumnya: [0, 0, 0] (karena awal)
- Output hidden state: hasil dari tanh(W × input + bias)

**Time Step 2:** "suka"
- Input: [0, 1, 0, 0, 0, 0]
- Hidden state sebelumnya: dari time step 1
- Output hidden state: hasil kalkulasi baru

**Time Step 3:** "cokelat"
- Input: [0, 0, 0, 1, 0, 0]
- Hidden state sebelumnya: dari time step 2
- Output hidden state: **Final hidden state** → ini yang dipakai encoder!

---

## 🚀 Kesimpulan

1. **RNN** dibuat untuk data sekuensial yang ukurannya bervariasi
2. **Urutan penting** → kalau ditukar, artinya berubah
3. **Hidden state** membawa informasi dari awal sampai akhir
4. **Encoder** memahami input, **Decoder** generate output
5. Model LLM (Chat GPT, dll) adalah **decoder-only** model
6. RNN bisa di-upgrade jadi **GRU** atau **LSTM** untuk performa lebih baik

---

## 📚 Next Steps

- Belajar **Attention Mechanism** (materi selanjutnya)
- Skip GRU & LSTM → langsung ke Attention!
- Attention adalah fondasi **Transformer** (GPT, BERT, dll)