# Pertemuan 4 - Natural Language Processing (NLP)

## 📚 Materi yang Dibahas
1. Memproses data dalam format teks
2. Melihat similarity antar teks
3. Visualisasi data teks
4. Implementasi sentiment analysis

---

## 🔧 1. Text Processing (Preprocessing)

### Teknik-teknik Preprocessing:

#### a) Hapus Angka dan Simbol
- Menghilangkan karakter yang tidak diperlukan untuk analisis
- Gunakan library **Regex** untuk menghapusnya

#### b) Hapus Stopwords
- **Stopwords** = kata-kata yang tidak dibutuhkan dalam analisis
- Contoh: "saya", "kamu", "dari", "ke", "yang", "dan", dll
- Kata-kata ini seperti "sampah" yang tidak berguna untuk analisis
- Library: 
  - Bahasa Inggris: `nltk.corpus`
  - Bahasa Indonesia: `sastrawi`

#### c) Hapus Emoji
- Penting untuk membersihkan data dari emoji
- Gunakan library Regex

#### d) Normalisasi Teks (Kata Dasar)
- Mengembalikan kata ke bentuk dasarnya
- Contoh: "mengambil" → "ambil"
- Tujuan: Supaya kata tidak muncul ganda dalam visualisasi

#### e) Normalisasi Huruf Kapital
- Ubah semua huruf kapital jadi huruf kecil
- Contoh: "Saya" → "saya"
- Tujuan: Agar kata tidak terhitung dua kali (Saya vs saya)

#### f) Hapus URL
- Hilangkan link seperti https://, www., .com, dll

---

## 🔍 2. Similarity Antar Teks

### Cosine Similarity
- Digunakan untuk mengukur tingkat kemiripan antar dokumen
- Library: `sklearn` (scikit-learn)

### TF-IDF (Term Frequency-Inverse Document Frequency)
- Harus dilakukan SEBELUM cosine similarity
- Fungsi: Melihat bobot antar kata
- Menentukan seberapa penting sebuah kata dalam dokumen

---

## 📊 3. Visualisasi Data Teks

### a) Word Cloud
**Yang Paling Sering Dipakai!**
- Menampilkan kata yang paling sering disebutkan dalam teks
- **Ciri-ciri**: Semakin besar ukuran kata → semakin sering muncul
- Library: `wordcloud`

### b) N-Gram (Bar Chart)

#### 📌 Unigram
- Memisah teks per 1 kata
- Contoh: "saya pergi ke pasar" → ["saya", "pergi", "ke", "pasar"]

#### 📌 Bigram (Paling Recommended!)
- Memisah teks per 2 kata
- Contoh: "saya pergi ke pasar" → ["saya pergi", "pergi ke", "ke pasar"]
- **Keunggulan**: Lebih jelas konteksnya dibanding unigram
- Contoh use case: "sahkan RUU" → kita tahu masyarakat ingin DPR mengesahkan RUU

#### 📌 Trigram
- Memisah teks per 3 kata
- Contoh: "saya pergi ke pasar" → ["saya pergi ke", "pergi ke pasar"]
- Jarang dipakai, tapi berguna untuk konteks yang lebih luas

### c) Named Entity Recognition (NER)
- Fungsi: Mengidentifikasi peran kata dalam kalimat
- Contoh:
  - "Jakarta" → **Location**
  - "Jokowi" → **Person**
  - "Hari Selasa" → **Time/Date**
  - "PBB" → **Organization**
- Library: `spacy`
- **Note**: Agak jarang dipakai, tapi tetap berguna

### d) Topic Modeling
- Mengelompokkan teks menjadi beberapa topik
- Library: `BERTopic`
- Contoh: Dari 1 teks panjang → dibagi jadi topik 1 (Python), topik 2 (NLP), topik 3 (EDA)

---

## 🎯 4. Sentiment Analysis

### Library Wajib untuk NLP:
```python
import nltk
# Wajib install extension NLTK:
nltk.download('punkt')
nltk.download('stopwords')
nltk.download('wordnet')
```

### Metode Sentiment Analysis:

#### A. From Scratch (Paling Sulit)
- Menggunakan **PyTorch** atau **TensorFlow**
- Model: **LSTM (Long Short-Term Memory)**

**Kenapa LSTM?**
- Sentiment analysis adalah data **sekuensial**
- Antar kata saling berhubungan
- Contoh: "saya pergi ke sekolah di hari Senin"
  - "saya" berhubungan dengan "pergi"
  - "pergi" berhubungan dengan "ke sekolah"
  - "di hari Senin" menjelaskan waktu

**Komponen Penting LSTM:**
- **Embedding**: Mengubah kata menjadi vektor
- **Dropout**: Mencegah overfitting (biasanya 0.5 = tutup 50% neuron)
- **Bidirectional**: Model baca dari depan ke belakang DAN belakang ke depan
- **Activation Function**:
  - `sigmoid` → Binary classification (positif/negatif)
  - `softmax` → Multi-class (positif/negatif/netral/dll)
- **Epochs**: Iterasi untuk memperbaiki kesalahan
- **Random State**: Wajib dipasang agar hasil konsisten

#### B. Pretrained Model (Lebih Mudah!)

**Untuk Bahasa Indonesia:**
- Model: `indobert-sentiment-classification`
- Dari Hugging Face

**Untuk Multiple Language:**
- Model: `cardiffnlp/twitter-roberta-base-sentiment`
- Support: Inggris, Mandarin, Spanyol, Arab, dll
- 5 Level: Very Negative, Negative, Neutral, Positive, Very Positive

#### C. TextBlob
- Library: `textblob`
- Dari: `spacy`
- **Keterbatasan**: Hanya untuk bahasa Inggris

---

## 💾 5. Scraping Data Twitter

**Tool**: Twint / Tweet-Harvest
- Authentication token diperlukan (dari cookies Twitter)
- Fitur:
  - Pilih keyword (contoh: "pemilu")
  - Pilih tanggal (dari-sampai)
  - Pilih bahasa (ID = Indonesia)
  - Save hasil ke CSV

---

## ☁️ 6. Deployment ke Cloud

### AWS EC2 (Virtual Machine)

**Langkah-langkah:**

1. **Buat Instance**
   - Pilih OS: Ubuntu (Recommended)
   - Instance type: T2.micro (Free tier)
   - Buat Key Pair (.pem file)

2. **Network Setting**
   - Allow SSH
   - Add Security Group untuk port custom (contoh: 8000)
   - Source type: Anywhere (untuk public access)

3. **Storage**: 8GB (untuk testing)

4. **SSH ke Server**
   ```bash
   ssh -i "fapi.pem" ubuntu@13.213.214.X
   ```

5. **Setup Environment**
   ```bash
   # Update Ubuntu
   sudo apt update
   
   # Install Python
   sudo apt install python3
   
   # Buat folder
   mkdir fasapi
   cd fasapi
   
   # Buat virtual environment
   python3 -m venv venv
   source venv/bin/activate
   
   # Install dependencies
   pip install -r requirements.txt
   ```

6. **Upload Files**
   - Gunakan `nano` untuk copy-paste file
   - File penting: `.env`, `main.py`

7. **Run Application**
   ```bash
   python main.py
   ```

8. **Akses**: `http://IP-ADDRESS:8000`

**Penting!** Set host = `0.0.0.0` (bukan 127.0.0.1) agar bisa diakses public

---

### GCP Cloud Run (Lebih Praktis!)

**File Wajib:**
1. **File Python** (`main.py`)
2. **requirements.txt** - Daftar library
3. **Dockerfile** - Perintah Linux untuk menjalankan app

**Contoh Dockerfile:**
```dockerfile
FROM python:3.9
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

**Langkah Deploy:**
1. Upload ke GitHub Repository
2. Pilih "Setup with Cloud Build"
3. Pilih repository
4. Configure:
   - Service name
   - Port: 8000
   - RAM: 512MB (untuk testing)
   - CPU: sesuai kebutuhan
   - Max concurrent requests: 80 (artinya 80 user bersamaan)
5. Create → Auto build & deploy!

---

## 💡 Tips Penting

### Stopwords Removal:
- **Bahasa Inggris**: Gunakan `nltk.corpus.stopwords`
- **Bahasa Indonesia**: Gunakan `sastrawi` (tapi kurang lengkap, perlu hapus manual)

### N-Gram Priority:
1. **Word Cloud** - Paling penting!
2. **Bigram** - Recommended untuk konteks yang jelas
3. Unigram - Sama dengan word cloud
4. Trigram - Opsional untuk konteks luas

### Emoji:
- **Hindari** memasukkan emoji dalam sentiment analysis
- Lebih baik data bersih meski sedikit meleset

### Port:
- **Fungsi**: Agar aplikasi tidak bentrok/crash
- Port umum: 8000, 9000
- Setiap aplikasi harus punya port berbeda

### Random State:
- **Wajib dipasang** agar hasil model konsisten
- Tanpa random state → akurasi bisa naik-turun setiap run

---

## 📚 Resource

- Cosine Similarity: Dokumentasi scikit-learn
- Sentiment Analysis Models: Hugging Face
- Twitter Scraping: Tutorial YouTube (cari authentication token)
- NLP Processing: Google Colab examples
- BERTopic: Dokumentasi resmi

---

**Next Meeting**: Deep Learning (termasuk LSTM lebih detail)

---

*Catatan dibuat dari Pertemuan 4 - Natural Language Processing*
