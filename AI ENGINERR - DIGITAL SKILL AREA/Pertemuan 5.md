# Pertemuan 5 - Deep Learning

## Pengenalan Deep Learning

Deep Learning itu **beda banget** sama Machine Learning biasa! 

**Machine Learning biasa:**
- Fokus untuk prediksi data tabular (baris & kolom)
- Contoh: prediksi kanker, diabetes, fraud detection
- Ada target variabel yang jelas

**Deep Learning:**
- Data yang digunakan: **gambar, suara, dan video**
- Contoh: speech to text, text to speech, deteksi objek
- Semua algoritma deep learning pakai arsitektur **Neural Network**

## Perbedaan Workflow

Deep learning fokus pada **arsitektur neural network** dengan ciri khas:
- Ada neuron-neuron yang saling terhubung
- Bentuknya kayak jaring-jaring (network)
- Strukturnya: Input → Hidden Layer → Output

### Komponen Neural Network

1. **Input**: Data yang kita punya (gambar mobil, gambar apapun)
2. **Hidden Layer**: Bagian yang memproses semua data
3. **Output**: Hasil akhir dari prediksi

## Cara Kerja Neural Network

### 1. Forward Propagation
Jalan lurus dari start sampai akhir!

- Data dimulai dari neuron 1 & 2 (input)
- Dikirim ke neuron 3 & 4 melalui weight (W13, W23, W14, W24)
- Weight berfungsi untuk mengirimkan sinyal antar neuron
- Ada **theta** sebagai gerbang pemrosesan
- Sampai ke neuron 5 (output) dan dapat error rate

### 2. Backward Propagation
Balik lagi dari output ke input!

- Setelah dapat error rate, proses balik dari output → hidden layer → input
- Tujuannya: **memodifikasi semua weight** berdasarkan akurasi/loss score
- Semua neuron weight-nya di-update untuk hasil yang lebih baik

## Activation Function

Ada 3 jenis activation function yang populer:

### 1. Sigmoid
- **Kapan pakai**: Prediksi dengan 2 target variabel (biner: yes/no)
- **Contoh**: Klasifikasi sampah organik vs non-organik
- Kurva mirip logistic regression

### 2. ReLU (Rectified Linear Unit)
- **Kapan pakai**: Algoritma deep learning yang berat & butuh hasil cepat
- **Contoh**: Deep Neural Network, Convolutional Neural Network (CNN)
- Lebih cepat dalam pemrosesan

### 3. Tanh (Tangent Hyperbolic)
- **Kapan pakai**: Data yang sifatnya **sekuensial** (ada hubungan antar data)
- **Contoh 1**: Sentimen analisis - "Saya mau pergi ke pasar"
  - Kata "saya" dan "pergi" punya korelasi
  - "ke pasar" menunjukkan lokasi tujuan
- **Contoh 2**: Prediksi stock price, harga Bitcoin, harga emas
  - Ada pola waktu: kapan naik, kapan turun

---

## Algoritma Deep Learning

## 1. Convolutional Neural Network (CNN)

### Apa itu CNN?
Algoritma deep learning turunan dari neural network yang **bagus untuk klasifikasi gambar dan video** - data yang sifatnya visual!

### Arsitektur CNN

```
Input Image → Convolution → Pooling → Neural Network → Output
```

**Komponen:**
- **Convolution**: Mengkompres/meringkas gambar menjadi ukuran lebih kecil
- **Pooling**: Kompres lagi lebih ketat!
- **Neural Network**: Proses dengan hidden layer
- **Output**: Hasil klasifikasi

### Cara Kerja

1. Input gambar (misal: gambar anjing)
2. **Convolution** mengompres gambar
3. **Pooling** mengompres lagi
4. Masuk ke neural network untuk klasifikasi

### Jenis Pooling

#### Max Pooling
- **Kapan pakai**: Gambar yang jelas dan kontras
- **Contoh**: Foto dari kamera HP bagus (iPhone, Samsung)
- Hasil foto tidak blur, detail tajam

#### Average Pooling
- **Kapan pakai**: Gambar yang samar-samar
- **Contoh**: Hasil X-ray tulang, deteksi tumor
- Gambar tidak kontras seperti foto biasa

---

## 2. Recurrent Neural Network (RNN)

### Apa itu RNN?
Neural network yang **prosesnya mengulang di hidden state** tergantung panjang dokumen/data.

### Perbedaan dengan Neural Network Biasa
- Neural network biasa: Input → Process → Selesai
- RNN: **Mengulang terus** di hidden layer!

### Cara Kerja RNN

**Contoh**: Sentimen analisis untuk kalimat "Saya benci belajar"

1. Hidden layer 1 → proses kata "Saya"
2. Hidden layer 2 → proses kata "benci"
3. Hidden layer 3 → proses kata "belajar"
4. Output → Sentimen: positif/negatif/netral

RNN membuat hidden layer sebanyak jumlah kata yang ada!

---

## 3. LSTM (Long Short-Term Memory)

### Apa itu LSTM?
Sama seperti RNN, tapi **lebih sering dipakai** untuk data sekuensial!

### Kapan Pakai LSTM?
- Sentimen analisis
- Stock price prediction (tapi Sarima/Arima lebih bagus untuk ini)
- Data yang punya hubungan dalam jangka waktu atau antar data

### Arsitektur LSTM

```
Input → Bidirectional LSTM → Attention Layer → Output
```

**Komponen:**
1. **Input**: Teks yang mau diklasifikasi
2. **Bidirectional LSTM**: Baca dari depan ke belakang DAN belakang ke depan
3. **Attention Layer**: Lihat hubungan mana yang paling kuat & faktor paling berpengaruh
4. **Output**: Positif/negatif/netral (pakai Softmax karena 3 kategori)

### Contoh Kasus: "Film ini tidak buruk"

#### Step 1: Input & Tokenize
Kalimat dipisah jadi kata-kata: `film`, `ini`, `tidak`, `buruk`

#### Step 2: Bidirectional LSTM
- **Forward Propagation**: Baca depan → belakang
- **Backward Propagation**: Baca belakang → depan
- Lihat korelasi antar kata:
  - `film` dengan `ini`
  - `tidak` dengan `buruk`

**Kenapa harus bidirectional?**
- Kalau tidak pakai, akurasi jelek!
- Supaya bisa lihat konteks yang lebih luas

#### Step 3: Attention Layer
Dari 4 kata (`film`, `ini`, `tidak`, `buruk`), mana yang paling berpengaruh?

- Kata **"buruk"** paling mempengaruhi sentimen (konotasi negatif)
- TAPI sebelum "buruk" ada kata **"tidak"**
- "tidak buruk" = **positif**! ✅

#### Step 4: Output
Hasil akhir: **Positif** menggunakan Softmax layer (karena ada 3 kategori)

---

## Vector Embedding

### Apa itu Vector Embedding?
Mengubah data (gambar/teks) menjadi **angka-angka** (vektor)!

### Aplikasi Vector Embedding

#### 1. Face Recognition
Mengubah gambar wajah jadi vektor angka

#### 2. LLM (Large Language Model)
Mengubah teks jadi vektor

### Konsep: Cosine Similarity
Melihat **kedekatan antar dokumen** atau kata berdasarkan jarak.
- Semakin dekat = semakin mirip
- Semakin jauh = semakin beda

### Jenis Vector Embedding

**Text Embedding:**
- BERT
- Sentence-BERT
- OpenAI Embedding (berbayar)
- Hugging Face Embedding (gratis)

**Image Embedding:**
- Berbagai model untuk gambar

**Multimodal Embedding:**
- Campuran: teks + gambar + audio

---

## Aplikasi di Dunia Kerja (Computer Vision Engineer)

### Real Talk: Jarang Pakai yang Tadi! 😮

CNN, RNN, LSTM itu **jarang dipakai** di dunia kerja sebagai Computer Vision Engineer.

### Yang Sering Dipakai:

### 1. YOLO (You Only Look Once)
**Real-time object detection** paling populer!
- Dokumentasi lengkap
- Banyak models & integration guides
- Wajib dipelajari!

### 2. NVIDIA Jetson
- Fungsi: Menyeimbangkan FPS (Frame Per Second)
- Supaya FPS tidak drop saat real-time detection
- Penting untuk performa optimal

### 3. OCR (Optical Character Recognition) ⭐⭐⭐
**SANGAT SERING DIPAKAI!**

#### Jenis-jenis OCR:

**1. Tesseract OCR**
- ❌ Instalasi ribet
- ✅ Akurasi lumayan
- Dari Google

**2. EasyOCR**
- ✅ Instalasi mudah
- ⚠️ Bagus untuk data simpel & tidak kompleks
- ⚠️ Fine-tuning lumayan susah

**3. PaddleOCR** 🏆 **PALING BAGUS!**
- ✅ Instalasi gampang
- ✅ Akurasi tinggi
- ✅ Fine-tuning paling mudah (meskipun tetap sulit)
- ⚠️ Fine-tuning Paddle tetap sangat-sangat susah!

#### Workflow OCR:
1. OCR data dari gambar
2. Dapat hasil teks
3. Masukkan data ke database

#### Aplikasi OCR:
- Baca KTP
- Baca Passport
- Baca dokumen pajak
- Know Your Customer (KYC)
- Otomasi baca dokumen

#### Fine-tuning OCR
- Pakai PyTorch (bawaan EasyOCR)
- Harus atur: dataset location, labeling, folder structure
- Sangat-sangat ribet tapi worth it!

### 4. OpenVINO
Untuk CCTV object detection!
- Streaming RTSP
- Classification
- Object detection di CCTV
- Tutorial lengkap tersedia

---

## Use Case Real: Deteksi Plat Nomor Truk

### Problem:
Ada footage CCTV dengan truk lewat. Harus deteksi plat nomor truk!

### Solusi Step-by-Step:

1. **Deteksi Truk** → Cari posisi truk di mana (YOLO)
2. **Deteksi Plat** → Cari lokasi plat di truk (YOLO)
3. **Crop Plat** → Ambil area plat
4. **OCR** → Baca nomor plat

**Workflow**: YOLO → YOLO → Crop → OCR

---

## Skill Tambahan yang Wajib Dikuasai

### Linux Command Line
**Kenapa harus belajar Linux?**
- Di dunia kerja, sistem pakai Linux!
- Tidak coding dari komputer pribadi (untuk production)
- Deploy ke sistem Linux

**Command dasar yang harus dikuasai:**
```bash
mkdir [nama_folder]     # Bikin folder
cd [nama_folder]        # Masuk folder
nano [nama_file.py]     # Edit file Python
```

### Library Tambahan

**Tabula**
- Extract tables from PDF
- Mirip OCR tapi khusus tabel

**Landing AI**
- Document extraction dengan layout asli
- Berbayar tapi ada trial

---

## Tools & Framework Tambahan

### N8N vs LangChain
**N8N:**
- Bikin workflow tanpa ngoding
- Drag and drop
- Bebas buat apapun (tidak cuma LLM)

**LangChain:**
- Framework untuk LLM
- Perlu coding
- Untuk orkestrasi LLM:
  - RAG (Retrieval Augmented Generation)
  - Analisis dokumen PDF
  - LLM Agent
  - Hemat biaya/token

**Lengraph:**
- Mirip N8N (workflow builder)
- Lebih tepat dibanding dengan LangChain

---

## Tips Belajar Computer Vision

### Prioritas Belajar:
1. ⭐⭐⭐ **YOLO** (wajib!)
2. ⭐⭐⭐ **OCR** (sangat sering!)
3. ⭐⭐⭐ **Fine-tuning OCR** (penting!)
4. ⭐⭐ **OpenVINO** (CCTV detection)
5. ⭐⭐ **NVIDIA Jetson** (optimization)
6. ⭐⭐ **Linux** (environment kerja)

### Framework yang Dipakai di Kerja:
- **PyTorch** ✅ (paling sering)
- **TensorFlow** ✅ (sering juga)
- **Keras** ❌ (jarang, cuma buat belajar)

### Fun Fact:
Computer Vision Engineer = **Pekerjaan AI Engineer paling susah!** 🔥
- Lebih susah dari NLP Engineer
- Lebih susah dari LLM Engineer
- Sangat-sangat rumit!

---

## Kesimpulan

**Deep Learning vs Machine Learning:**
- ML: Data tabular
- DL: Gambar, suara, video

**Algoritma Deep Learning:**
- CNN: Untuk gambar/video
- RNN: Untuk data sekuensial
- LSTM: Untuk data sekuensial (lebih bagus dari RNN)

**Real World:**
- Pakai YOLO, OCR, OpenVINO
- CNN/RNN/LSTM jarang dipakai langsung
- Harus belajar Linux
- PyTorch/TensorFlow > Keras

**Next:**
Minggu depan masuk **LLM** (Large Language Model)! 🚀
- Tidak bahas ML/DL lagi
- Full LLM
- Pakai Google Gemini (gratis)
- Boleh pakai OpenAI/Claude kalau punya

---

**Resources yang dibagikan:**
- Tutorial YOLO
- Tutorial CNN
- Tutorial LSTM
- Fine-tuning OCR dengan DeepSea
- KYC automation (baca passport & KTP)

**Catatan Penting:**
Belajar pelan-pelan tapi konsisten. Computer Vision itu challenging tapi sangat rewarding! 💪
