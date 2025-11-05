# Foundational Statistics - Part 1

---

## 🎯 Kenapa AI Engineer Harus Belajar Statistik?

### AI itu Luas Banget!
AI punya banyak cabang:
- **Machine Learning** → Cara mesin "berpikir"
- **Deep Learning** → ML yang lebih kompleks
- **Natural Language Processing (NLP)** → Mesin memahami bahasa manusia
- **Computer Vision** → Mesin "melihat" seperti mata manusia
- **Speech Recognition** → Mesin mendengar dan bicara
- **Robotics** → Robot pintar

### Intinya?
**AI = Program yang meniru cara kerja manusia dari A-Z**

### Kenapa Harus Statistik?
Karena di balik semua algoritma ML/AI, ada **matematika dan statistik**! 

Contoh nyata:
- Linear Regression → Persamaan garis lurus
- Naive Bayes → Teorema probabilitas
- Model evaluasi → Standar deviasi, variance

**Jangan cuma bisa coding, tapi nggak paham dalamnya!** 🚀

---

## 📊 Statistika vs Statistik

### Bedanya Apa?

| Statistika | Statistik |
|------------|-----------|
| **Ilmunya** - cara mengumpulkan, menganalisis, menafsirkan data | **Datanya** - kumpulan data yang sudah dikumpulkan |
| Contoh: "Belajar cara bikin survei" | Contoh: "Statistik pemain sepak bola di half-time" |

**Ingat:** Di bahasa Inggris sama semua → **Statistics**

---

## 🌍 Statistik di Kehidupan Sehari-hari

1. **Survei** → Tingkat kepopuleran produk
2. **Quick Count Pemilu** → Hitung cepat hasil pemilihan
3. **A/B Testing** → Website/app mana yang lebih bagus
4. **Sensus Penduduk** → Ngitung jumlah penduduk negara
5. **Asuransi** → Hitung risiko berdasarkan umur/kondisi
6. **Peminjaman Bank** → Apakah seseorang bisa bayar cicilan?

**Statistik ada di mana-mana, bahkan tanpa kita sadari!** 💡

---

## 🧮 Dua Jenis Statistik

### 1. **Deskriptif Statistik**
Mendeskripsikan data kita → Seperti apa sih datanya?

**Tujuan:**
- Tahu rata-rata
- Tahu sebaran data
- Tahu bentuk distribusi

### 2. **Inferensial Statistik**
Analisis data untuk **ambil keputusan** atau **prediksi**

**Bedanya dengan Deskriptif:**
- Inferensial pakai **sampel** (sebagian kecil dari populasi)
- Contoh: Survei 1 juta orang dari 270 juta penduduk Indonesia

**Kenapa pakai sampel?**
- ✅ Lebih cepat
- ✅ Lebih murah
- ✅ Lebih efisien
- ✅ Praktis!

---

## 📏 Deskriptif Statistik - Part 1: Central Tendency

**Central Tendency** = Mengukur nilai pusat/sentral data

### 1️⃣ Mean (Rata-rata) ⭐

**Rumus:**
```
Mean = Total semua data ÷ Jumlah data
```

**Contoh:**
Data penjualan roti John selama 7 hari: `[15, 18, 20, 22, 20, 18, 27]`

```
Mean = (15+18+20+22+20+18+27) ÷ 7 = 140 ÷ 7 = 20
```

**Artinya:** John sebaiknya stok 20 roti per hari!

**Notasi:**
- **Populasi:** μ (mu), N
- **Sampel:** x̄ (x-bar), n

---

#### ⚠️ Kelemahan Mean: Tidak Tahan Outliers!

**Outliers** = Data pencilan (nilai yang beda jauh dari yang lain)

**Contoh:**
9 karyawan gajinya Rp 10.000 - 50.000  
CEO gajinya Rp 1.000.000  

**Mean = Rp 113.000**

Padahal sebagian besar cuma Rp 10.000-50.000! Mean jadi **nggak representatif**.

---

### 2️⃣ Median (Nilai Tengah) 🎯

**Cara Hitung:**
1. **Urutkan** data dari kecil ke besar
2. Ambil **nilai tengah**

**Contoh 1 (Data Ganjil - 7 data):**
```
Data: [64, 70, 72, 76, 78, 82, 84]
Median = 76 (nilai ke-4, di tengah!)
```

**Contoh 2 (Data Genap - 8 data):**
```
Data: [64, 70, 72, 76, 77, 78, 82, 84]
Median = (76 + 77) ÷ 2 = 76.5
```

#### ✅ Kelebihan Median: Tahan Outliers!

Walaupun ada CEO bergaji 1 juta, median tetap akurat karena cuma ambil nilai tengah!

**Kekurangan:** Kurang familiar untuk orang awam

---

### 3️⃣ Modus (Nilai Paling Sering Muncul) 🔥

**Cara Hitung:**
Cari nilai yang **paling banyak muncul**

**Contoh:**
```
Data: [70, 72, 72, 76, 76, 76, 82, 84, 84]

70 → muncul 1x
72 → muncul 2x
76 → muncul 3x ← PALING BANYAK!
82 → muncul 1x
84 → muncul 2x

Modus = 76
```

**Kapan Pakai Modus?**
- Untuk tahu nilai yang **paling populer**
- Contoh: Roti apa yang paling laris? Ukuran sepatu yang paling banyak terjual?

---

## 📐 Skewness (Kemencengan/Kemiringan Data)

Dengan **Mean, Median, Modus** → kita bisa tahu **bentuk distribusi data**!

### Tiga Jenis Skewness:

#### 1. **Symmetric (Simetris)**
```
Mean = Median = Modus
```
Distribusi sempurna → **JARANG BANGET!**

#### 2. **Negative Skew (Miring Kiri)**
```
Mean < Median < Modus
```
- Data terpusat di **nilai besar**
- Contoh: Mean = 50, Median = 60, Modus = 70

#### 3. **Positive Skew (Miring Kanan)**
```
Mean > Median > Modus
```
- Data terpusat di **nilai kecil**
- Contoh: Mean = 80, Median = 70, Modus = 50

### 💡 Tips:
- **Positive Skew** → Mean bisa **overestimate**, lebih baik pakai **Median**
- **Negative Skew** → Mean bisa **underestimate**, lebih baik pakai **Median**

---

## 📊 Deskriptif Statistik - Part 2: Variability

**Variability** = Mengukur sebaran/penyebaran data

### 1️⃣ Range (Rentang)

**Rumus:**
```
Range = Nilai Maksimum - Nilai Minimum
```

**Contoh:**
```
Data: [10, 20, 30, 50, 100]
Range = 100 - 10 = 90
```

#### ⚠️ Kelemahan: Tidak Tahan Outliers!

```
Data: [10, 20, 30, 50, 200]
Range = 200 - 10 = 190 ← NAIK DRASTIS!
```

---

### 2️⃣ Standard Deviation (Standar Deviasi) 📏

**Definisi:**
Rata-rata jarak setiap data dari **mean**

**Rumus:**
```
1. Kurangi setiap data dengan mean
2. Kuadratkan hasilnya
3. Jumlahkan semua hasil kuadrat
4. Bagi dengan jumlah data (atau n-1)
5. Akar kuadratkan
```

**Contoh Manual:**
```
Data: [46, 69, 32, 60, 52, 41]

Step 1: Hitung Mean
Mean = (46+69+32+60+52+41) ÷ 6 = 50

Step 2: Kurangi dengan Mean
46-50 = -4
69-50 = 19
32-50 = -18
60-50 = 10
52-50 = 2
41-50 = -9

Step 3: Kuadratkan
(-4)² = 16
19² = 361
(-18)² = 324
10² = 100
2² = 4
(-9)² = 81

Step 4: Jumlahkan
16+361+324+100+4+81 = 886

Step 5: Bagi dengan n (atau n-1)
886 ÷ 6 = 147.67

Step 6: Akar Kuadratkan
√147.67 ≈ 12.15
```

**Artinya:**
```
Mean = 50
Standar Deviasi = 12.15

Sebaran data: 50 ± 12.15 → 37.85 sampai 62.15
```

---

### 📌 Aturan 3-Sigma

| Sigma | Range | Populasi |
|-------|-------|----------|
| ±1σ | Normal | 68% |
| ±2σ | Agak Aneh | 95% |
| ±3σ | **Outliers!** | 99.7% |

**Contoh:**
- Tinggi badan 160 cm → **1 Sigma** (normal)
- Tinggi badan 200 cm → **3 Sigma** (outliers!)

---

### 3️⃣ Variance (Varians)

**Rumus:**
```
Variance = (Standar Deviasi)²
```

**Bedanya dengan Standar Deviasi:**

| | Standar Deviasi | Variance |
|--|----------------|----------|
| **Satuan** | Sama dengan data (cm) | Kuadrat (cm²) |
| **Penggunaan** | ✅ Sering dipakai | ❌ Jarang dipakai |

**Kenapa Variance Jarang Dipakai?**
Karena **satuannya berbeda** dengan data asli → susah dibandingkan!

**Contoh:**
```
Data dalam cm → Variance dalam cm²
Nggak bisa dibandingkan langsung!
```

---

## 📦 Cara Deteksi Outliers: Box Plot

**Komponen Box Plot:**
- **Q1 (Kuartil 1)** → 25% data
- **Q2 (Median)** → 50% data
- **Q3 (Kuartil 3)** → 75% data
- **IQR** = Q3 - Q1
- **Lower Limit** = Q1 - (1.5 × IQR)
- **Upper Limit** = Q3 + (1.5 × IQR)

**Outliers:**
- Data **< Lower Limit** → Outliers!
- Data **> Upper Limit** → Outliers!

**Contoh:**
```
Lower Limit = 2.5
Upper Limit = 9.5

Data = 1 → OUTLIERS! (< 2.5)
Data = 10 → OUTLIERS! (> 9.5)
Data = 5 → AMAN ✅
```

---

## 🎲 Fundamental of Probability

### Apa itu Probability (Peluang)?

**Probability** = Seberapa besar kemungkinan suatu kejadian terjadi

**Range:** 0% - 100% (atau 0 - 1)

---

### Contoh Sederhana:

#### 1. **Coin Toss (Lempar Koin)**
```
Ada 2 sisi: Head (H) dan Tail (T)
P(Head) = 1/2 = 50%
P(Tail) = 1/2 = 50%
```

#### 2. **Dice Roll (Lempar Dadu)**
```
Ada 6 mata: 1, 2, 3, 4, 5, 6
P(muncul angka 1) = 1/6 ≈ 16.67%
P(muncul angka genap) = 3/6 = 50%
```

---

### Probability di Kehidupan Nyata:

1. **Asuransi**
   - Orang tua (60 tahun) → Premi lebih mahal (risiko meninggal lebih tinggi)
   - Orang muda (20 tahun) → Premi lebih murah (risiko lebih rendah)

2. **Pinjaman Bank (KPR)**
   - Bank pakai data untuk hitung **probabilitas gagal bayar**
   - Sekarang pakai **Machine Learning** untuk approve/reject pinjaman!

3. **Quick Count**
   - Hitung probabilitas hasil pemilu tanpa harus nunggu 100% suara masuk

---

### Formula Dasar Probability

**Favorable Outcome**
```
P(Event) = Jumlah Kejadian yang Diinginkan ÷ Total Kemungkinan
```

**Contoh:**
```
Deck kartu = 52 kartu
Kartu Spade = 13 kartu

P(Spade) = 13/52 = 1/4 = 25%
```

---

### Expected Value (Nilai Harapan) 🎯

**Definisi:**
Rata-rata hasil yang diharapkan kalau eksperimen diulang berkali-kali

**Rumus:**
```
E(X) = Probability × Jumlah Percobaan
```

**Contoh:**
```
Ambil kartu 20 kali, berapa kali dapat Spade?

P(Spade) = 1/4 = 0.25
Expected Value = 0.25 × 20 = 5 kali

Artinya: Kita "harapkan" dapat Spade 5 kali dari 20 percobaan
```

**Catatan:** Expected Value ≠ Hasil Nyata!
- Dapat 7 Spade → Lagi **hoki**! 🍀
- Dapat 3 Spade → Lagi **kurang hoki** 😅

---

### Complement (Komplemen)

**Definisi:**
Semua kejadian **selain** yang kita inginkan

**Rumus:**
```
P(A) + P(A') = 1 (atau 100%)
```

**Contoh:**
```
P(Dadu = 1) = 1/6
P(Dadu ≠ 1) = 5/6

Cek: 1/6 + 5/6 = 6/6 = 1 ✅
```

---

## 📈 Data Analysis & Interpretation

### Kenapa Data Analysis Penting?

**Sebelum:**
- Perusahaan punya data tapi dibiarkan
- Keputusan pakai **intuisi**
- Hasilnya: Sering salah! ❌

**Sekarang:**
- Data dianalisis
- Keputusan pakai **data-driven**
- Hasilnya: Lebih akurat! ✅

**Bukti:**
- CPNS 2023 ada lowongan **Data Scientist** dan **Data Analyst** → Pemerintah udah aware!

---

### Contoh Real Data Analysis

#### 1. **Traffic Kemacetan Lebaran (Jabar Digital Service)**
```
Insight: Kemacetan terjadi SETELAH lebaran, bukan sebelum!

Keputusan:
- Tambah petugas lalu lintas SETELAH lebaran
- Hemat budget (nggak buang-buang duit saat Ramadan)
```

#### 2. **Traffic E-commerce Saat Ramadan**
```
Insight: Traffic naik 152% saat jam sahur (1-5 pagi)!

Keputusan:
- Scaling server CPU jadi 2.5x di jam sahur
- Hindari website down saat traffic tinggi
```

---

### Case Study: Soda Pop Company 🥤

**Data:**
- Umur customer
- Customer satisfaction
- Calorie concern

**Analisis:**
```
1. Distribusi Umur
   - Paling banyak: Umur 20-an (expected)
   - Surprise: Umur 50-an juga banyak! 😮

2. Insight
   - Pasar umur 50+ ternyata besar!
   
3. Keputusan
   - Develop produk: Healthy Soda (Sugar-Free)
   - Target: Konsumen 50+ yang peduli kesehatan
```

**Hasil:**
✅ Produk baru → Market baru → Revenue naik!

---

### Case Study: Titanic Dataset 🚢

**Dataset:**
- Passenger info: Umur, gender, kelas tiket
- Survived: Ya/Tidak

**Analisis:**

#### 1. **Gender**
```
Female → Lebih banyak selamat!
Alasan: "Ladies first!" dalam evakuasi
```

#### 2. **Passenger Class**
```
Kelas 1 dan 3 → Lebih banyak selamat
Kelas 2 → Lebih sedikit selamat (?)
```

#### 3. **Umur**
```
Distribusi selamat: Bervariasi di semua umur
Insight: Umur bukan faktor utama
```

**Machine Learning Application:**
Bisa bikin model prediksi: **"Apakah passenger X bakal selamat?"**

---

## 🛠️ Handling Outliers

### Cara Deteksi:
✅ Pakai **Box Plot** (paling simpel!)

### Cara Handle:

1. **Trimming** → Hapus outliers
2. **Capping** → Ganti dengan nilai max/min
3. **Median Imputation** → Ganti dengan median
4. **Mean Imputation** → Ganti dengan mean

**⚠️ INGAT: Nggak semua outliers harus dihapus!**

**Contoh Kapan HARUS Hapus:**
- Data kucing & anjing, tapi ada data kuda → **Hapus!**
- Data bahasa Indonesia, tapi ada bahasa Inggris → **Hapus!**

**Contoh Kapan JANGAN Hapus:**
- Data gaji karyawan vs CEO → Ini **data asli**, bukan error

---

## 🎓 Recap & Key Points

### ✅ Yang Udah Dipelajari:

1. **Statistika vs Statistik**
   - Statistika = Ilmunya
   - Statistik = Datanya

2. **Kenapa AI Engineer Butuh Statistik**
   - Algoritma ML pakai matematika & statistik
   - Harus paham "dalam" model, bukan cuma coding!

3. **Deskriptif Statistik**
   - **Central Tendency:** Mean, Median, Modus
   - **Variability:** Range, Standar Deviasi, Variance

4. **Skewness**
   - Positive, Negative, Symmetric

5. **Probability Basics**
   - Favorable Outcome, Expected Value, Complement

6. **Data Analysis**
   - Interpretasi data untuk keputusan bisnis

---

## 💡 Tips & Tricks

### Kapan Pakai Mean, Median, Modus?

| Kondisi | Pilihan Terbaik |
|---------|----------------|
| Data **normal**, tanpa outliers | **Mean** ✅ |
| Data ada **outliers** | **Median** ✅ |
| Cari nilai **paling populer** | **Modus** ✅ |
| Data **symmetric** | Bebas! Semua sama |

### Formula Cepat:

**Standar Deviasi = Jarak rata-rata data dari mean**

**Variance = (Standar Deviasi)²**

**Coefficient of Variation = Mean ÷ Standar Deviasi**

---

## 📚 Next Steps

1. **Distribusi Data** → Normal, Binomial, Poisson
2. **Bayes Theorem** → Probabilitas lanjutan
3. **Inferensial Statistik** → Uji Hipotesis, ANOVA
4. **Praktik Python** → NumPy, Pandas, Matplotlib

---

## 🔗 Resources

- **Medium Blog:** Artikel tentang categorical data (ordinal vs nominal)
- **Google Colab:** Sudah ada implementasi rumus-rumus
- **StatQuest (YouTube):** Channel belajar statistik yang fun!

---

**Happy Learning! 🚀**

> "Statistik bukan cuma angka, tapi cerita di balik data!" 📊✨
