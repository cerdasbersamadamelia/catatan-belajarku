### 1. PENGENALAN AI & MACHINE LEARNING

Struktur Artificial Intelligence:
- AI (Artificial Intelligence) adalah konsep paling luas
- Machine Learning adalah subset dari AI
- Deep Learning adalah subset dari Machine Learning
- Cabang lain AI: NLP (Natural Language Processing), Computer Vision, Generative AI/LLM

Generative AI & LLM:
- Contoh: ChatGPT, Google Gemini, Claude
- Muncul tahun 2022
- Akan dipelajari di pertemuan 6-10
- Paling banyak dibahas karena mayoritas pekerjaan sekarang fokus di sini

---

### 2. LAPANGAN KERJA AI DI INDONESIA

Peluang Kerja:
- Paling luas: Generative AI dan Computer Vision
- Paling terbatas: Data Scientist dan Data Analyst
- Rekomendasi: Fokus ke AI Engineer atau ML Engineer
- Alternatif: Data Engineer (sangat dicari karena susah)

Negara dengan AI Paling Maju:
1. Amerika Serikat (paling banyak job)
2. Singapura
3. Australia
4. Eropa Timur (Romania, Polandia, Cek Republik) - gaji tinggi
5. Timur Tengah (UAE, Arab Saudi)

Kenapa Indonesia masih sedikit:
- Indonesia belum benar-benar mengimplementasikan AI secara massive

---

### 3. SKILL YANG HARUS DIKUASAI

Skill Utama:
- ✅ Fast Learning (paling penting!)
- ✅ Adaptasi cepat - AI berkembang sangat cepat
- ✅ Konsep & cara kerja AI lebih penting dari coding
- ⚠ Matematika tidak perlu terlalu dalam - cukup pahami statistik

Tools yang Wajib Dikuasai:
- Backend: FastAPI (wajib!), Flask (sudah jarang)
- Deep Learning Framework: PyTorch (90% requirement kerja), TensorFlow
- Deployment: Docker, GCP/AWS
- Vibe Code: Claude Sonnet 4.5 (model terbaik untuk coding)

Tools Data Engineer (opsional):
- Apache Spark
- Docker
- Apache Airflow
- DBT (Data Build Tool)
- Prefect

---

### 4. SUMBER BELAJAR & KOMUNITAS

Sumber Berita AI:
- LinkedIn - follow orang yang posting metode AI terbaru
- Hugging Face - model-model terbaru & free
- Google Scholar - research paper (boleh cari: "fine tuning LLM")

Cara Belajar:
- Baca research paper → summarize pakai ChatGPT/Claude
- Minta contoh implementasi kode ke LLM
- Belajar lewat dokumentasi framework

Komunitas:
- LinkedIn Groups (contoh: Python Developer, Artificial Intelligence)
- Kaggle (untuk portfolio & belajar dari notebook orang lain)

---

### 5. JENIS-JENIS MACHINE LEARNING

### A. SUPERVISED LEARNING (Paling Sering Dipakai)

Karakteristik:
- Memiliki target variabel sebagai prediktor
- Seperti belajar dengan kunci jawaban

1. Classification:
- Target variabel: kategori terbatas (biasanya <5)
- Contoh: Yes/No, Fraud/Not Fraud
- Use Case: 
  - Fraud Detection (deteksi penipuan di bank)
  - Churn Prediction (prediksi customer resign)
  - Disease Detection (deteksi penyakit)

2. Regression:
- Target variabel: numerik & bervariasi
- Contoh: 1000, 2000, 3000, dll
- Use Case:
  - Prediksi Harga Rumah
  - Prediksi Salary

---

### B. UNSUPERVISED LEARNING

Karakteristik:
- TIDAK memiliki target variabel
- Model belajar sendiri dari data

1. Clustering:
- Mengelompokkan data berdasarkan kesamaan
- Use Case: Customer Segmentation
- Contoh Implementasi:
  - Kluster 1: Suka belanja banyak & barang branded → kasih promo branded
  - Kluster 2: Tidak suka belanja tapi spend banyak
  - Kluster 3: Suka belanja tapi barang murah → kasih promo buy 1 get 2

2. Dimensionality Reduction:
- Untuk data processing
- Mengurangi jumlah fitur
- Contoh: PCA

3. Hierarchical:
- Visualisasi: Dendrogram
- Jarang dipakai

---

### C. REINFORCEMENT LEARNING

Karakteristik:
- Learn from mistake
- Sistem reward & punishment
- Jawaban benar → dapat poin
- Jawaban salah → kurang poin
- Jarang dipakai

---

### 6. IMPLEMENTASI AI DI INDUSTRI

Computer Vision:
- Object Detection (Tesla, YOLO)
- Image Classification
- Live Object Detection
- Framework: PyTorch, TensorFlow (CNN)

Speech to Text & Text to Speech:
- YouTube auto translate
- Tools: ElevenLabs
- Contoh: meme "tung tung sahur", "bensin perchot"

Virtual Assistant:
- Chatbot menggunakan LLM
- Final project bootcamp

Finance & Banking:
- Fraud Detection
- Loan Approval Prediction

Smart Home & Robotics:
- Alexa
- Menggunakan IoT

Gaming:
- Bot di Mobile Legends
- AI mempelajari karakteristik player

---

### 7. LIFECYCLE PROJECT MACHINE LEARNING

Step-by-Step:

1. Define Business Goal
   - Tentukan apa yang ingin dicapai
   - Contoh: deteksi kanker, fraud detection

2. Machine Learning Problem Framing
   - Tentukan model yang cocok
   - Pilih supervised/unsupervised
   - Pilih algorithm (Decision Tree, Random Forest, dll)

3. Data Processing (PALING SULIT & RIBET!)
   - Check null values
   - Drop kolom yang tidak relevan (contoh: customer ID)
   - Ubah data type (string → integer/float)
   - Label Encoding: ubah teks jadi angka (komputer tidak bisa baca teks)
   - Correlation Heatmap: cek korelasi antar fitur
   - Drop fitur dengan korelasi >0.8 atau <-0.8 (redundansi)

4. Train-Test Split
   - X_train, Y_train: data training (80%)
   - X_test, Y_test: data testing (20%)

5. Model Training
   - Hyperparameter tuning

6. Model Evaluation
   - Akurasi, Precision, Recall
   - Feature Importance: fitur mana yang paling berpengaruh
   - SHAP Value: lebih detail dari feature importance, bisa lihat korelasi positif/negatif

7. Model Deployment
   - Bungkus model dengan FastAPI atau Flask
   - Bikin Dockerfile
   - Deploy ke cloud (GCP/AWS)
   - Metode deploy GCP: Cloud Run (mudah), Compute Engine, Artifact Registry (paling susah)

8. Monitoring
   - MLOps
   - Tools: MLFlow, Comet

---

### 8. TIPS KERJA & PORTFOLIO

Cari Kerja di Luar Negeri:
- Peluang interview sangat kecil - prioritas warga negara sendiri
- Solusi: tingkatkan portfolio sebagus & sekompleks mungkin
- Negara ideologi kanan (contoh: era Trump) lebih susah

Tes Coding:
- Indonesia: HackerRank
- Luar Negeri: Test Gorilla
- Contoh: Astra - 60 soal pilihan ganda + 2 coding (durasi 8 jam!)
- Realita: Coding test sudah tidak relevan di era AI, tapi masih banyak dipakai

Portfolio:
- Platform: Kaggle, LinkedIn
- Format: End-to-end (data collection → processing → EDA → modeling → evaluation)
- Yang diminati: Banyak visualisasi (bar chart, box plot, dll)
- Posting di LinkedIn untuk visibility ke recruiter
- Belajar: Baca notebook orang lain di Kaggle

Framework:
- PyTorch: untuk research (dokumentasi kurang jelas)
- TensorFlow: untuk industri (dokumentasi lebih jelas, dari Google)
- Rekomendasi pemula: mulai dari TensorFlow

---

### 9. JADWAL BOOTCAMP

- Pertemuan 1: Introduction to ML (selesai)
- Pertemuan 2: Model Selection
- Pertemuan 3: ML Models, Hyperparameter Tuning, SHAP Value
- Pertemuan 4: NLP & Text Analysis
- Pertemuan 5: Computer Vision, Deep Learning, YOLO, PyTorch
- Pertemuan 6-10: LLM, RAG, Fine Tuning, Framework LLM
- Final Project: Chatbot

---

### 10. CATATAN PENTING

- ✅ Data cleaning sangat penting - model bagus dimulai dari data bagus
- ✅ Vibe code diperbolehkan - fokus ke konsep, bukan coding
- ✅ SHAP Value: Hanya untuk supervised learning (classification & regression)
- ✅ Time Series: Pakai SARIMA, ARIMA, SARIMAX, LSTM (tidak perlu feature importance)
