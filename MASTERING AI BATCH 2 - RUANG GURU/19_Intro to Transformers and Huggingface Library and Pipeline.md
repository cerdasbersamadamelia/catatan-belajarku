# Intro to Transformers and Hugging Face Library and Pipeline

## Apa itu Hugging Face?

Hugging Face adalah platform seperti GitHub, tapi khusus untuk model machine learning. Kalau GitHub buat share code, Hugging Face buat share:
- Model machine learning
- Dataset
- Demo aplikasi ML

Awalnya Hugging Face cuma library buat model BERT, sekarang udah jadi multimillion company yang di-backing sama Google, AWS, dan startup-startup besar lainnya.

## Kenapa Pakai Hugging Face?

### 1. Setup Model Gampang Banget
Gak perlu ribet import library yang banyak, gak perlu bikin configuration untuk preprocessing sendiri. Semua udah dimudahkan.

### 2. Manajemen Model yang Baik
Ada version control seperti git - kamu bisa track versi model pertama, kedua, ketiga, dan seterusnya. Semuanya tersimpan rapi dengan hash-nya masing-masing.

### 3. Deployment Mudah
Kalau bayar ke Hugging Face, mereka bisa training dan deploy model kamu langsung tanpa perlu setup cloud provider seperti Google Cloud, AWS, atau Azure.

## Instalasi

```bash
pip install transformers sentence-piece
```

- `transformers` = library utamanya
- `sentence-piece` = library untuk tokenizer

**Catatan:** Ini BUKAN Transformer film Michael Bay ya! Ini library untuk arsitektur model machine learning.

## Pipeline - Cara Tercepat Pakai Model

Pipeline adalah tools dari Hugging Face yang super simpel. Dengan 3 baris code aja, kamu udah bisa pakai model!

### Contoh: Text Classification

```python
from transformers import pipeline

# Buat classifier
classifier = pipeline("text-classification", model="distilbert-base-uncased-finetuned-sst-2-english")

# Langsung pakai!
result = classifier("Hello world")
# Output: [{'label': 'POSITIVE', 'score': 0.9998}]
```

### Multiple Input Sekaligus

```python
results = classifier([
    "Hello world",
    "I don't like you"
])
# Output langsung 3 prediksi sesuai urutan input
```

### Gimana Model-nya Disimpan?

Pas pertama kali running, model akan di-download otomatis dan disimpan di **cache Hugging Face** di lokal komputer kamu. Jadi gak perlu download ulang tiap kali pakai.

## Cara Cari Model di Hugging Face

Kunjungi website Hugging Face dan filter berdasarkan:

### Filter yang Penting:
1. **Task** - Mau buat apa? (text classification, translation, dll)
2. **Language** - Bahasa apa? (Indonesia, Inggris, dll)
3. **License** - Penting kalau mau pakai komersial!
4. **Sort by Most Downloads** - Model populer biasanya lebih bagus

### Contoh: Model Bahasa Indonesia

Kalau mau text classification bahasa Indonesia:
1. Pilih task: Text Classification
2. Pilih language: Indonesian
3. Sort by: Most Downloads
4. Copy nama model yang bagus

```python
# Pakai model bahasa Indonesia
classifier = pipeline("text-classification", 
                      model="mdhugol/indonesia-bert-sentiment-classification")

result = classifier([
    "Saya ingin makan nasi",
    "Saya menyukai gaya makan Anda"
])
# Output: [{'label': 'NEUTRAL', 'score': 0.xx}, {'label': 'POSITIVE', 'score': 0.xx}]
```

## Arsitektur Transformer

Transformer punya 2 komponen utama:

### 1. Encoder
- **Fungsi:** Mengerti input
- Fokusnya memahami maksud dari input yang masuk
- Biasanya terdiri dari 4-12 layer

### 2. Decoder  
- **Fungsi:** Membuat output
- Nerima hasil understanding dari encoder
- Bikin output satu kata demi satu kata (kayak ChatGPT)

**Flow-nya:**
```
Input → Encoder (layer 1 → 2 → 3 → 4) → Decoder → Output
```

## Tokenizer - Translator Antara Manusia dan Mesin

Model gak ngerti bahasa manusia. Makanya perlu **tokenizer** buat translate:

```
Bahasa Manusia → Tokenizer → Angka (dimengerti model) → Model → Angka → Tokenizer → Bahasa Manusia
```

### Contoh Translation:

```python
from transformers import AutoTokenizer, AutoModel

# Load model dan tokenizer
model_name = "Helsinki-NLP/opus-mt-en-id"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModel.from_pretrained(model_name)

# Input bahasa manusia
sentence = "I am a student"

# 1. Translate ke angka (encode)
inputs = tokenizer.encode(sentence, return_tensors="pt")
# Output: tensor([[7303, 15, 4846, 0]])

# 2. Model proses
outputs = model.generate(inputs, max_length=50)
# Output: tensor([[54795, 200, ...]])

# 3. Translate balik ke bahasa manusia (decode)
result = tokenizer.decode(outputs[0])
# Output: "Saya seorang mahasiswa"
```

### Penting Diingat:
- **1 kata ≠ 1 token**
- **1 karakter ≠ 1 token**
- Token adalah entity sendiri yang tergantung jenis tokenizer dan bahasa

Contoh:
- "Selamat pagi" (2 kata) → 2 token
- "Good morning" (2 kata) → 2 token
- Tapi tokennya beda angkanya!

## Attention Mechanism

Attention adalah mekanisme yang membuat model fokus ke bagian input yang paling penting untuk generate output terbaik.

### Contoh Kasus:
Kalimat: "Saya menyukai gaya makan Anda"
Pertanyaan: "Sentimen kalimat ini apa?"

Attention akan membuat model **fokus ke kata "menyukai"** untuk menentukan ini kalimat positif.

### Multihead Attention
Di Transformer, attention-nya gak cuma satu, tapi **banyak** (biasanya 8 atau 12 head). Ini kayak kita minta pendapat dari banyak orang sebelum ambil keputusan - supaya hasilnya lebih optimal!

## Tasks yang Bisa Dikerjain Pipeline

Pipeline bisa ngerjain banyak task:

### 1. Text Classification / Sentiment Analysis
```python
classifier = pipeline("text-classification")
classifier("Saya suka produk ini")
```

### 2. Zero-Shot Classification
Bisa klasifikasi tanpa training! Tinggal kasih label aja:
```python
classifier = pipeline("zero-shot-classification")
classifier(
    "I like to eat spicy food",
    candidate_labels=["eating", "drinking", "playing"]
)
# Output: eating (score tertinggi)
```

### 3. Text Generation
```python
generator = pipeline("text-generation", model="gpt2")
generator("In this course, we will teach you")
# Output: "In this course, we will teach you all about mathematical models..."
```

Parameter penting:
- `max_length`: Panjang maksimal output (dalam token)
- `num_return_sequences`: Mau berapa output?

### 4. Fill-Mask
Isi bagian yang kosong:
```python
unmasker = pipeline("fill-mask")
unmasker("This course will teach you all about <mask> models")
# Output: "mathematical", "building", "predictive"
```

### 5. Named Entity Recognition (NER)
Deteksi nama orang, tempat, organisasi:
```python
ner = pipeline("ner", aggregation_strategy="simple")
ner("My name is Agung and I work at Ruang Guru in Indonesia")
# Output: 
# - Agung → PERSON
# - Ruang Guru → ORGANIZATION
# - Indonesia → LOCATION
```

### 6. Question Answering
Harus ada konteks!
```python
qa = pipeline("question-answering")
qa(
    question="What is my name?",
    context="My name is Imam and I want to show my house"
)
# Output: Imam
```

### 7. Summarization
```python
summarizer = pipeline("summarization")
summarizer("Teks panjang di sini...")
```

### 8. Translation
```python
translator = pipeline("translation_en_to_id", 
                      model="Helsinki-NLP/opus-mt-en-id")
translator("I am a student")
# Output: "Saya seorang mahasiswa"
```

Helsinki-NLP punya banyak bahasa:
- `en-id`: Inggris → Indonesia
- `en-fr`: Inggris → Prancis  
- `id-en`: Indonesia → Inggris
- Dan banyak lagi!

## Audio dan Vision

### Speech-to-Text
```python
transcriber = pipeline("automatic-speech-recognition", 
                       model="openai/whisper-base")
transcriber("path/to/audio.mp3")
```

Whisper dari OpenAI support banyak bahasa, termasuk Indonesia!

### Image Classification
```python
classifier = pipeline("image-classification")
classifier("path/to/image.jpg")
```

### Zero-Shot Image Classification
```python
classifier = pipeline("zero-shot-image-classification", 
                      model="openai/clip-vit-base-patch32")
classifier(
    "path/to/cat.jpg",
    candidate_labels=["a photo of a cat", "a photo of a dog"]
)
# Output: a photo of a cat (score tertinggi)
```

## GPU vs CPU

Pipeline defaultnya pakai **CPU**. Kalau mau lebih cepat, pakai **GPU**:

```python
# Pakai GPU device 0
classifier = pipeline("text-classification", device=0)
```

Contoh perbandingan:
- CPU: 90 ms per prediksi
- GPU: 20 ms per prediksi

Jauh lebih cepat kan?

### Cek GPU di Google Colab
```python
import torch
torch.cuda.is_available()  # True kalau ada GPU

# Atau pakai nvidia-smi
!nvidia-smi
```

## Tips Penting

1. **Model bahasa Inggris gak bagus buat bahasa Indonesia** - Cari model yang spesifik di-training pakai bahasa Indonesia
2. **Cek license model** - Penting kalau mau pakai komersial!
3. **Token ≠ kata ≠ karakter** - Hati-hati pas set max_length
4. **Pipeline gak support semua model** - Model LLM baru biasanya belum support pipeline, harus pakai `AutoModel`
5. **Google Colab gratis udah cukup** - Gak perlu Pro buat latihan

## Dataset yang Bagus untuk Bahasa Indonesia

Kalau mau training model sendiri, dataset rekomendasi:
- **IndoNLI** - Natural Language Inference
- **IndoNLU** - Natural Language Understanding  
- **SMSA** - Sentiment Analysis

Semua bisa ditemukan di Hugging Face Datasets!

## Kenapa Transformer Powerful?

Paper aslinya: **"Attention is All You Need"** (2017)

Transformer lebih bagus dari RNN/LSTM karena pakai **self-attention mechanism**. Gak pakai recurrent, gak pakai GRU, cuma attention aja!

Sebelum GPT-3 populer (2017-2020), ada 3 jenis model:

1. **Autoregressive** (decoder-only) - Buat generate text
   - Contoh: GPT-2, GPT-3, GPT-4
   
2. **Auto-encoding** (encoder-only) - Buat klasifikasi & understanding
   - Contoh: BERT
   
3. **Sequence-to-Sequence** (encoder + decoder) - Buat task kritis
   - Contoh: T5, BART, Translation models

Sekarang yang paling populer: **Autoregressive** (ChatGPT & keluarganya)!