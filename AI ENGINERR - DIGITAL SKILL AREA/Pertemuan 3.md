# 📚 RINGKASAN LENGKAP MATERI BOOTCAMP AI ENGINEER - PERTEMUAN 3

## 🎯 TOPIK UTAMA: Machine Learning - Classification, Regression, dan Clustering

---

##  MACHINE LEARNING - CLASSIFICATION

### A. Contoh Use Case Classification:
1. Deteksi Sel Kanker: Berdasarkan ciri-ciri (bentuk, sisi, warna) → menentukan kelas Malignant atau Benign
2. Prediksi Pembelian Customer: Berdasarkan gender, age, estimated salary → prediksi Purchase (Ya/Tidak)

### B. Konsep Dasar Classification:
- Target Variabel: Kolom yang ingin diprediksi (contoh: Purchase)
- Feature/Fitur: Kolom input untuk prediksi (contoh: Gender, Age, Salary)
- Cara Kerja: Model belajar dari data training → mencari pola karakteristik → prediksi data baru

---

### C. Algoritma Classification:

#### 1. Decision Tree Classifier ⭐
- Cara kerja: Pohon keputusan berbentuk if-else
- Model machine learning paling dasar
- Contoh kasus: Cuaca mendung/cerah → Macet/tidak → Keputusan pergi/tidak pergi
- Status: Model untuk pemula, sering dipakai untuk belajar

#### 2. Random Forest Classifier ⭐⭐⭐
- Cara kerja: Gabungan beberapa decision tree
- Melakukan voting berdasarkan hasil mayoritas
- Contoh: 3 decision tree → hasil Yes=2, No=1 → Final result = Yes
- Kelebihan: 
  - Performa TERBAIK
  - Lebih bagus dari XGBoost
  - Tidak perlu khawatir underfitting
- Kelemahan: 
  - SANGAT sensitif terhadap overfitting
  - Perlu hati-hati saat training

#### 3. XGBoost (Extreme Gradient Boosting) ⭐⭐⭐
- Cara kerja: Iterasi bertahap, tree-based model
- Proses:
  - Iterasi 1 → catat kesalahan
  - Iterasi 2 → perbaiki kesalahan dari iterasi 1
  - Iterasi 3 → perbaiki kesalahan dari iterasi 2
  - Dan seterusnya...
- Konsep mirip Epoch (akan dipelajari di Deep Learning)
- Kelebihan:
  - Cepat dalam proses training
  - Performa bagus (akurasi, precision, recall bagus)
  - Algoritma yang "menjamin"
  - Pilihan utama senior Data Scientist
- 🏆 REKOMENDASI MENTOR: Pilih XGBoost atau Random Forest

#### 4. Support Vector Machine (SVM) ❌
- Cara kerja: Menggunakan hyperplane untuk memisahkan 2 kelas
- Membuat garis pemisah antara data kelas 0 dan kelas 1
- Sumbu X & Y (contoh: Usia vs Penghasilan)
- Jika overfitting → garis bisa meliuk-liuk sangat detail
- ⚠ CATATAN MENTOR: 
  - BUKAN algoritma yang bagus
  - Tidak direkomendasikan untuk produksi
  - Hanya dipelajari karena sering dibahas

#### 5. K-Nearest Neighbors (KNN) ❌
- Cara kerja: Melihat karakteristik tetangga terdekat
- Setting jumlah tetangga (3, 10, dll)
- Contoh: 
  - 3 tetangga terdekat = semua biru → prediksi = biru
  - 10 tetangga terdekat = mayoritas biru (9 biru, 1 hitam) → prediksi = biru
- ⚠ CATATAN MENTOR:
  - BUKAN algoritma yang bagus
  - NOT RECOMMENDED untuk produksi
  - Hanya untuk belajar/uji coba

#### 6. Logistic Regression ❌
- Cara kerja: Membentuk kurva S
- Melihat titik-titik data di atas atau di bawah kurva
- Garis mengikuti letak titik-titik mayoritas
- Cocok untuk binary classification
- Perbedaan dengan SVM: 
  - Logistic Regression → bentuk S, lihat arah atas/bawah
  - SVM → membelah antar kelas dengan garis
- ⚠ CATATAN MENTOR: 
  - Algoritma yang "aneh"
  - Tidak terlalu direkomendasikan
  - Tidak tahu kenapa banyak yang pakai

#### 7. Multilayer Perceptron (MLP) 🧠
- Status: Sudah masuk ke Deep Learning
- Ciri khas: Menggunakan Neural Network Architecture
- Komponen:
  - Input Layer: Terima data dari dataset
  - Hidden Layer: Tempat data diproses
  - Output Layer: Hasil akhir (classification)

Detail Arsitektur Neural Network:
- Neuron 1 & 2 (Input) → kirim ke Neuron 3 & 4 (Hidden)
- W13, W23, W14, W24: Bobot untuk kirim data
- Theta (θ): Fungsi sebagai gerbang di setiap hidden layer
- Pembobotan: Dilakukan di setiap neuron

Forward Propagation:
- Arah: Neuron 1 → 2 → 3 → 4 → 5
- Dari X1 sampai Y5
- Maju dari depan ke belakang

Back Propagation:
- Arah: Neuron 5 → 4 → 3 → 2 → 1
- Proses dari belakang ke depan
- Contoh loss: 0.128 (masih tinggi)
- Hasil error dikembalikan untuk revisi bobot
- Neuron 5 → Neuron 3 & 4 (revisi bobot)
- Neuron 3 & 4 → Neuron 1 & 2 (revisi bobot lagi)

#### 8. Algoritma Lainnya yang Disebutkan:
- CatBoost (Gradient Boosting variant)
- LightGBM (Light Gradient Boosting Machine)
- AdaBoost (Adaptive Boosting)

---

##  MACHINE LEARNING - REGRESSION

### Algoritma Regression:
1. Linear Regression
2. Ridge Regression
3. Decision Tree Regressor (cara kerja sama seperti classifier)
4. Random Forest Regressor (voting berdasarkan nilai mayoritas)
5. XGBoost Regressor (cara kerja sama dengan classification)
6. CatBoost Regressor

### Perbedaan dengan Classification:
- Classification: Target variabel adalah kategori (Ya/Tidak, 0/1)
- Regression: Target variabel adalah angka kontinu (harga, suhu, dll)
- Algoritma sama, hanya beda di karakteristik target variabel

---

##  UNSUPERVISED LEARNING - CLUSTERING

### A. K-Means Clustering ⭐
Cara Kerja:
1. User menentukan jumlah cluster (default = 3, bisa 4, 5, dst)
2. Model belajar sendiri tanpa target variabel
3. Model mengelompokkan data secara otomatis
4. Contoh: Cluster 1, 2, 3, 4

Evaluasi K-Means:

1. Elbow Method:
- Lihat grafik penurunan
- Pilih cluster dengan penurunan paling landai
- Contoh: Cluster 4 → turun landai → pilih cluster 4

2. Silhouette Score:
- Seperti akurasi untuk clustering
- Pilih cluster dengan score tertinggi
- Contoh: Cluster 4 = score tertinggi → pilih cluster 4

### B. DBSCAN ⭐⭐
- Mirip K-Means, fungsi untuk clustering
- 🏆 REKOMENDASI MENTOR: Lebih bagus dari K-Means

### C. Hierarchical Clustering
- Menggunakan Dendrogram
- Untuk melihat hierarki pengelompokan
- Library: scikit-learn

### D. A Priori Algorithm 📊
Use Case: Market Basket Analysis

Konsep:
- Analisis kombinasi barang yang sering dibeli bersamaan
- Dataset: Daftar barang (True = dibeli, False = tidak dibeli)

Cara Kerja:
- Support: Seberapa sering produk dibeli oleh user
- Contoh hasil:
  - Cokelat = 0.4204 (paling sering dibeli)
  - Cokelat + Susu (kombinasi sering dibeli)
  - Butter + Ice Cream
  - Oreo + Susu

Manfaat:
- Membuat sistem rekomendasi
- Strategi diskon/penawaran khusus (Buy 1 Get 1)

Library: ML Extend (bukan scikit-learn)

⚠ Catatan: Masuk dalam Unsupervised Learning

### E. PCA (Principal Component Analysis) 📉
- Kategori: Dimensionality Reduction
- Fungsi: Mengecilkan ukuran dataset
- Mengurangi jumlah fitur sambil mempertahankan informasi penting

---

##  KONSEP PENTING MACHINE LEARNING

### A. Overfitting ⚠

Ciri-ciri Overfitting:
1. Terlalu menghafalkan fitur tertentu
2. Akurasi terlalu tinggi (>95% untuk non-computer vision)
3. Feature Importance sangat menonjol di 1 fitur (~70%)

Contoh:
- Model prediksi customer churn
- Feature Importance: Kolom "Kontrak" = 70%
- Artinya: Model TERLALU bergantung pada 1 kolom saja
- Status: OVERFITTING ❌

Cara Deteksi:
- Lihat dari Feature Importance
- Jika ada 1 fitur yang terlalu dominan → overfitting

Pengecualian:
- Computer Vision (object detection, face recognition)
- Akurasi 95-99% adalah NORMAL ✅

Penyebab:
- Ada kolom dengan korelasi sangat tinggi dengan target variabel
- BUKAN karena outlier

Catatan Outlier:
- Outlier malah menyebabkan akurasi dan performa JELEK
- Outlier = nilai di luar minimum/maksimum (di luar kuartil)

### B. Data Preprocessing 🧹

1. Data Cleaning:
- Wajib dilakukan sebelum algoritma apapun
- Bersihkan data dari missing values, duplikasi, dll

2. Normalisasi/Scaling:
- Min-Max Scaler: Untuk CLASSIFICATION
- Standard Scaler: Untuk REGRESSION

3. Hapus Outlier:
- Z-Score: Metode standar
- IQR (Interquartile Range): Metode terbaik 🏆

Khusus A Priori:
- Pastikan data bersih
- Scaling bisa pakai Standard Scaler

---

### C. Hyperparameter Tuning ⚙

#### 1. GridSearchCV 🔍
Cara Kerja:
- Mencoba SEMUA kombinasi parameter
- Contoh parameter:
  - max_depth: [3, 4, 5, 6, 7, 8] → 6 pilihan
  - min_samples_split: [2, 5, 10] → 3 pilihan
  - min_samples_leaf: [1, 2] → 2 pilihan
  - random_state: [0, 42] → 2 pilihan
  
- Total kombinasi: 6 × 3 × 2 × 2 × 2 × 2 = 1440 fits

Output:
- Parameter terbaik
- Akurasi terbaik berdasarkan parameter
- Time taken: Berapa lama proses tuning

#### 2. RandomizedSearchCV ⚡
Cara Kerja:
- TIDAK mencoba semua kombinasi
- Hanya mencoba beberapa kombinasi (contoh: 100 fits)

Kapan Pakai:
- Dataset ukuran besar
- Algoritma kompleks (Random Forest, XGBoost)

Keuntungan:
1. Menghemat waktu training
2. Menghemat biaya (jika pakai komputer perusahaan)

Perbandingan:
- GridSearchCV: 1440 kombinasi
- RandomizedSearchCV: 100 kombinasi
- Selisih: 14x lebih cepat

---

### D. Evaluasi Model

1. Confusion Matrix 📊
- Sudah dibahas sebelumnya
- Menampilkan True Positive, False Positive, dll

2. SHAP Plot 📈
Cara Baca:
- Warna merah ke kanan → Berbanding LURUS
- Warna merah ke kiri → Berbanding TERBALIK

Keterangan lengkap sudah ditulis di notebook

---

##  NATURAL LANGUAGE PROCESSING (NLP) & LARGE LANGUAGE MODELS (LLM)

### Jadwal Pembelajaran:
- Pertemuan 4: NLP, Text Processing
- Pertemuan 6-10: Full bahas LLM (fokus utama bootcamp)
- Pertemuan 9 & 10: Materi PALING BERAT ⚠

### Final Project Requirements:
Mandatory: Bikin Chatbot LLM

Syarat:
1. WAJIB mengandung unsur LLM
2. TIDAK BOLEH chatbot sederhana/biasa
3. HARUS Advance LLM

Contoh Advance LLM:
- LangChain Agent
  - SQL Agent
  - Python Agent
  - Custom Agent

API yang Digunakan:
- Google Gemini API (GRATIS) ✅ 🏆
- Bisa juga: OpenAI, Claude (BERBAYAR)
- Deepseek (cek availability di LangChain)

Framework: LangChain

---

##  TOOLS & TECHNOLOGIES

### A. N8N 🔄
- Fungsi: Workflow automation
- Karakteristik: No-code platform
- Fitur:
  - Connect ke Slack, Telegram, dll
  - Bisa menjalankan LLM
  - Otomasi workflow

### B. Vertex AI ☁
Apa itu:
- Versi berbayar dari Google Gemini
- Platform di Google Cloud Platform (GCP)

Setup Requirements:
1. Punya akun Google Cloud Platform
2. Bikin project
3. Atur permissions
4. Konfigurasi lainnya
- Status: Setup lumayan ribet

Perbedaan dengan Google Gemini:
- Keamanan: Data TIDAK ditraining oleh model ✅
- Privacy: Data lebih aman
- Google Gemini gratis: Data conversation akan ditraining ⚠

Rekomendasi:
- Hati-hati pakai Google Gemini gratis
- Jaga data pribadi
- Vertex AI lebih aman untuk data sensitif

---

##  TIME SERIES

Pertanyaan peserta: Perlu dipelajari? Kepakai?

Jawaban Mentor: KEPAKAI ✅

Algoritma Time Series:
1. SARIMA
2. ARIMA
3. SARIMAX
4. LSTM ⭐ (Yang paling bagus untuk Time Series)

Rekomendasi: Ya, pelajari time series

---

##  REINFORCEMENT LEARNING

Status di Industri:
- Jarang dipakai di industri
- Yang paling sering: Classification, Regression, Clustering

Kapan Dipakai:
- Jika digabung dengan Deep Learning
- Untuk penelitian kompleks tentang Reinforcement Learning
- TIDAK sering dipakai untuk production

---

##  TIPS & REKOMENDASI MENTOR 💡

### A. Algoritma untuk Production:
🏆 TOP 2 PILIHAN:
1. XGBoost (Pilihan utama senior Data Scientist)
2. Random Forest

❌ HINDARI untuk Production:
- Support Vector Machine (SVM)
- K-Nearest Neighbors (KNN)
- Logistic Regression

✅ Untuk Belajar/Uji Coba:
- Semua algoritma boleh dipelajari

### B. Catatan Tambahan:
- PowerPoint lebih lengkap dari catatan Canvas
- Materi sudah dishare di chat
- Mentor akan share link Canvas (akan dihapus besok)
- Ada catatan untuk LLM materi ke-9 (akan dihapus juga)

### C. Jadwal Bootcamp:
Usulan Perubahan:
- Dari: Sabtu & Minggu
- Menjadi: Selasa & Kamis
- Waktu: Jam 19.00-20.00 (malam)
- Alasan: Weekend buat santai, main game, jalan-jalan

Action: Peserta diminta konfirmasi ke admin (Diarya)

---

## 🔟 CODING & PRAKTIK

### Topik Coding yang Sudah Dishare:
1. Overfitting Detection (via Feature Importance)
2. XGBoost, CatBoost, AdaBoost (notebook khusus)
3. GridSearchCV (1440 kombinasi)
4. RandomizedSearchCV (100 kombinasi)
5. SHAP Plot (interpretasi model)
6. K-Means Evaluation (Elbow & Silhouette)

### Notebook yang Dishare:
- Sudah di-share kemarin
- Versi lebih jelas dishare lagi
- Khusus untuk algoritma XGBS, CatBoost, AdaBoost
- Fokus: memahami cara kerja hyperparameter tuning

---

## 📝 KESIMPULAN

Materi Pertemuan 3:
✅ Classification (8+ algoritma)  
✅ Regression (6+ algoritma)  
✅ Clustering (5 algoritma)  
✅ Overfitting & cara deteksi  
✅ Hyperparameter Tuning (Grid vs Randomized)  
✅ Data Preprocessing  
✅ Evaluasi Model  
✅ Preview NLP & LLM  
✅ Tools (N8N, Vertex AI)  
✅ Time Series  

Next Session (Pertemuan 4):
- NLP (Natural Language Processing)
- Text Processing