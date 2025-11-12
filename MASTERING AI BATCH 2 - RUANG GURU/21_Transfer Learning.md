# Transfer Learning

## Apa itu Transfer Learning?

Transfer learning adalah teknik machine learning di mana kita menggunakan model yang sudah ditraining sebelumnya (pre-trained model) dan melatihnya lagi dengan data kita sendiri. 

**Analogi sederhana:**
- Seperti anak SMA yang belajar IPA (fisika, biologi, kimia, matematika)
- Saat kuliah, dia ambil Teknik Informatika
- Dia tidak belajar dari nol, tapi fokus ke ilmu yang relevan dengan teknik komputer
- Ini lebih efisien daripada belajar semua dari awal

## Kenapa Pakai Transfer Learning?

### 1. Hemat Biaya
- Training dari scratch (dari awal) memerlukan GPU yang sangat mahal
- Model besar seperti ResNet butuh lebih dari seminggu untuk training
- Biaya bisa ribuan dollar untuk training model besar

### 2. Lebih Cepat
- Model pre-trained sudah belajar dari data yang general
- Kita tinggal fine-tune untuk data spesifik kita
- Proses training jadi lebih singkat

### 3. Data Training Lebih Sedikit
- Training from scratch butuh jutaan data
- Dengan transfer learning, cukup ribuan atau puluhan ribu data

## Proses Transfer Learning

### Training from Scratch vs Transfer Learning

**Training from Scratch:**
1. Inisialisasi weight random
2. Training dengan dataset sangat besar
3. Butuh waktu lama dan biaya tinggi

**Transfer Learning:**
1. Ambil pre-trained model (sudah ditraining dengan dataset general)
2. Fine-tune dengan dataset kita yang lebih spesifik
3. Model fokus ke data kita, tapi tetap punya knowledge dari training sebelumnya

## Contoh Kasus

### Dataset Fashion MNIST
- 60,000 gambar training
- 10,000 gambar testing
- 10 kategori (sepatu, tas, baju, dll)

**Hasil:**
- Model from scratch: akurasi lebih rendah
- Model dengan transfer learning: akurasi lebih tinggi dan training lebih cepat

## Fine-Tuning di Berbagai Domain

### 1. Text Classification (NLP)
**Dataset:** Indo NLU (sentiment analysis)

**Model yang digunakan:** IndoBERT
- Pre-trained model untuk Bahasa Indonesia
- Fine-tune untuk classification task spesifik

**Parameter penting:**
- Batch size: 32 atau 64 (tergantung GPU)
- Learning rate: biasanya 0.0001
- Epochs: 3-5 sudah cukup

**Tools:**
- Hugging Face Transformers
- Tokenizer untuk pre-process text
- Weights & Biases (W&B) untuk tracking experiment

### 2. Audio Classification
**Dataset:** PolyAI MINDS-14

**Model:** Wav2Vec2
- Pre-trained untuk audio processing
- Fine-tune untuk intent classification

**Proses:**
1. Load dataset dan split train/test
2. Pre-process audio (sampling rate: 16,000 Hz)
3. Feature extraction dengan Wav2Vec2
4. Training dengan parameter optimal
5. Evaluasi dengan accuracy metric

### 3. Image Classification
**Dataset:** Food-101

**Model:** Vision Transformer (ViT)
- Pre-trained untuk image recognition
- Fine-tune untuk food classification

**Augmentasi Data:**
- Random flip
- Random rotation
- Color jittering
- Untuk memperbanyak variasi training data

## Tips Training

### 1. Memilih Learning Rate
- Tidak ada formula pasti
- Coba-coba dan monitor loss
- Gunakan tools seperti W&B untuk tracking
- Biasanya: 1e-4 sampai 1e-5

### 2. Menghindari Overfitting
- Gunakan dropout (0.1 - 0.2)
- Data augmentation untuk image
- Early stopping jika validation loss naik
- Regularization

### 3. Monitoring Training
**Gunakan Weights & Biases (W&B):**
- Track learning rate
- Monitor training loss
- Lihat validation metrics
- Compare berbagai experiment

**Metrics penting:**
- Training loss: harus turun
- Validation loss: harus turun dan tidak naik terlalu jauh dari training loss
- Accuracy: semakin tinggi semakin baik

## Model Deployment

### Quantization
Teknik untuk membuat model lebih ringan:
- Mengubah float32 menjadi int8
- Ukuran model jadi lebih kecil
- Inference lebih cepat
- Trade-off: sedikit penurunan akurasi

**Perbandingan:**
- Model vanilla: 0.23s inference time
- Model quantized: 0.11s inference time (50% lebih cepat!)
- Akurasi berkurang sedikit, tapi tetap acceptable

### ONNX (Open Neural Network Exchange)
- Format model universal
- Bisa convert dari PyTorch, TensorFlow, dll
- Optimized untuk inference
- Lebih cepat daripada format asli

## Hugging Face Hub

Platform untuk share dan download model:
- Gratis dan open source
- Ribuan pre-trained model
- Mudah digunakan dengan Transformers library
- Push model dengan `trainer.push_to_hub()`

## Key Takeaways

1. **Transfer learning lebih efisien** daripada training from scratch
2. **Fine-tuning bisa dilakukan** untuk text, audio, dan image
3. **Monitor training dengan tools** seperti W&B
4. **Optimization penting** untuk deployment (quantization, ONNX)
5. **Hugging Face** adalah platform utama untuk NLP dan banyak task lain
6. **Parameter tuning** butuh experiment dan tracking yang baik
7. **Data quality** lebih penting daripada quantity dalam transfer learning