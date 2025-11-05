# Foundational Statistics-2

---

## 📋 Overview Materi
Sesi ini membahas:
1. **Distribution Statistics** (Binomial & Normal Distribution)
2. **Bayes' Theorem**

**Catatan:** Materi eksponensial distribution tidak dibahas karena terlalu kompleks.

---

## 🎯 1. TERMINOLOGI DASAR

### Population vs Sample
- **Population** = Semua data yang ada
- **Sample** = Subset/bagian dari data

**Kenapa pakai sample?**
- Kalau pakai population = sama aja kayak melakukan sensus/pemilu langsung
- Alasan praktis: **biaya** dan **waktu**
- Contoh: Survei pemilihan presiden - gak mungkin tanya semua orang

---

## ⚠️ 2. SAMPLING BIAS

### Jenis-Jenis Bias yang Umum:

#### a) **Sampling Bias**
**Contoh Kasus:** Survei via telepon rumah di tahun 90-an
- Hasil: Kandidat A diprediksi menang
- **Masalah:** Yang punya telepon rumah = orang kaya
- **Bias:** Sample cuma dari kelompok ekonomi tertentu

#### b) **Sample Too Small**
Data terlalu sedikit untuk representatif

#### c) **Non-Responsive Bias**
- Pertanyaan yang orang enggan jawab (tabu/privasi)
- Contoh: Pertanyaan tentang gaji, hal-hal privat
- Orang cenderung tidak jujur atau skip pertanyaan

#### d) **Random Sampling Bias**
- Ambil sample acak berdasarkan nomor
- Masalah: Orang dengan gaji tinggi cenderung enggan jawab

#### e) **Survival Bias**
**Contoh klasik:**
- Bill Gates & Steve Jobs = dropout tapi sukses
- Orang mikir: "Pendidikan gak penting"
- **Bias:** Yang keliatan cuma yang "survive" (sukses)
- Yang dropout dan **gagal** gak keliatan!

#### f) **Leading Question**
Pertanyaan yang mengarahkan ke jawaban tertentu

**Contoh salah:**
❌ "Produk kami bagus kan?"

**Contoh lebih netral:**
✅ "Bagaimana pendapat Anda tentang produk kami?"

---

## 🎲 3. BINOMIAL DISTRIBUTION

### Konsep Dasar: Coin Flip
Koin yang **fair** (adil):
- P(Head) = 0.5
- P(Tail) = 0.5

### Perhitungan Probability

#### **Flip 2 Kali:**
```
Kemungkinan:
HH → 0.5 × 0.5 = 0.25
HT → 0.5 × 0.5 = 0.25
TH → 0.5 × 0.5 = 0.25
TT → 0.5 × 0.5 = 0.25

P(2 Head) = 0.25
P(2 Tail) = 0.25
P(Berbeda) = 0.25 + 0.25 = 0.5
```

**Kenapa dikali?** Karena kayak "potong kue" - kita ambil bagian dari bagian

#### **Flip 3 Kali:**
```
P(3 Head) = 0.5 × 0.5 × 0.5 = 0.125 (1/8)
P(2 Head, 1 Tail) = ?
```

Kemungkinan **2H, 1T:**
- HHT
- HTH
- THH

**Ada 3 kemungkinan!** Masing-masing = 1/8  
Jadi: **P(2H, 1T) = 3/8 = 0.375**

#### **Flip 4 Kali:**
```
P(4 Head) = 1/16
P(3 Head, 1 Tail) = 4/16
P(2 Head, 2 Tail) = 6/16
P(1 Head, 3 Tail) = 4/16
P(4 Tail) = 1/16
```

---

### 🔺 Segitiga Pascal

Pola koefisien mengikuti **Segitiga Pascal:**

```
                1
              1   1
            1   2   1
          1   3   3   1
        1   4   6   4   1
      1   5  10  10   5   1
```

**Aturan:**
- Paling kiri & kanan selalu **1**
- Angka tengah = **jumlah 2 angka di atasnya**

**Contoh:** 4 kali flip = **1 4 6 4 1**
- 0 Head = 1 cara
- 1 Head = 4 cara
- 2 Head = 6 cara
- 3 Head = 4 cara
- 4 Head = 1 cara

---

### 🎁 Bonus: Hubungan dengan Aljabar!

Segitiga Pascal sama dengan koefisien binomial expansion:

```
(x + y)¹ = 1x + 1y             → 1 1
(x + y)² = 1x² + 2xy + 1y²     → 1 2 1
(x + y)³ = 1x³ + 3x²y + 3xy² + 1y³  → 1 3 3 1
(x + y)⁴ = 1x⁴ + 4x³y + 6x²y² + 4xy³ + 1y⁴  → 1 4 6 4 1
```

**Magic!** Pola angkanya sama persis! 🎩✨

---

### 📐 Rumus Binomial Distribution

Untuk **n** kali flip, dapat **y** head:

```
Koefisien = n! / (y! × (n-y)!)
```

**Contoh:** 4 kali flip, dapat 2 head:
```
= 4! / (2! × 2!)
= (4 × 3 × 2 × 1) / ((2 × 1) × (2 × 1))
= 24 / 4
= 6 ✓
```

### Probability Total:
```
P = Koefisien × P(Head)^y × P(Tail)^(n-y)
```

**Contoh:** 4 kali flip, dapat 2H 2T, koin fair (0.5):
```
P = 6 × (0.5)² × (0.5)²
P = 6 × 0.25 × 0.25
P = 6 × 0.0625
P = 0.375
```

---

### 🎲 Koin Tidak Fair

Kalau koin **tidak fair:**
- P(Head) = 0.6
- P(Tail) = 0.4

**Maka perhitungannya berubah!**

**Contoh:** 4 kali flip, dapat 2H 2T:
```
P = 6 × (0.6)² × (0.4)²
P = 6 × 0.36 × 0.16
P = 0.3456
```

Koefisien tetap **6**, tapi probability berubah!

---

### 🍰 Aplikasi: Baker's Problem

**Analogi:** Kamu masak setiap hari
- P(Sukses) = 0.6 (60%)
- P(Gagal) = 0.4 (40%)

**Pertanyaan:** Dari **10 hari** masak, berapa kemungkinan **9 hari sukses**?

```
Koefisien = 10! / (9! × 1!) = 10

P = 10 × (0.6)⁹ × (0.4)¹
```

**Karakteristik Binomial:**
1. Ada **2 kemungkinan**: Sukses atau Gagal
2. Dilakukan **berkali-kali**
3. Setiap percobaan **independen** (tidak saling mempengaruhi)

---

### 📊 Visualisasi Binomial Distribution

Kalau kita bikin grafik:
- **X-axis:** Jumlah sukses
- **Y-axis:** Probability

**P(sukses) = 0.5 (Fair):**
```
     ●
   ●   ●
 ●       ●
●         ●
```
Grafik **simetris**

**P(sukses) = 0.7 (Bias ke sukses):**
```
         ●
       ●   
     ●     
   ●       
 ●         
```
Grafik **condong ke kanan**

**Manfaat:**
- Bisa lihat berapa P(up to X sukses) dengan menjumlahkan area

---

## 📈 4. NORMAL DISTRIBUTION

### Karakteristik
- Bentuk **lonceng** (bell curve)
- Banyak terjadi **di alam** (nature)
- Ditentukan oleh 2 parameter:
  1. **μ (mu)** = Mean/rata-rata
  2. **σ (sigma)** = Standard deviation/varians

### 🔔 Bentuk Kurva

```
        ╱‾‾╲
      ╱      ╲
    ╱          ╲
  ╱              ╲
━━━━━━━━━━━━━━━━━━━━
      μ
```

**Mean (μ):** Menentukan posisi tengah kurva  
**Standard Deviation (σ):** Menentukan lebar kurva

---

### Pengaruh Parameter

#### **Mean (μ):**
- **μ kecil** → Kurva geser ke kiri
- **μ besar** → Kurva geser ke kanan

#### **Variance/Standard Deviation (σ):**
- **σ kecil** → Kurva tinggi & sempit (data mengelompok)
- **σ besar** → Kurva pendek & lebar (data menyebar)

```
σ kecil:      ╱‾╲          σ besar:    ╱‾‾‾‾╲
            ╱    ╲                   ╱        ╲
          ╱        ╲               ╱            ╲
```

---

### 🌍 Contoh Real-Life

#### 1. **Tinggi Badan**
- Survei 100 orang
- Plot tinggi badan → Dapat kurva normal!
- Mean = rata-rata tinggi
- Variance = seberapa beragam tingginya

#### 2. **Nilai Ujian**
- Nilai siswa di kelas
- Biasanya mengikuti normal distribution

#### 3. **Berat Badan**

#### 4. **Error Measurement**
- Setiap pengukuran ada error
- Error ini mengikuti normal distribution!

#### 5. **Skor Tes IQ**

---

### 🎓 Aplikasi: Sistem Penilaian

**Skenario:** Dosen pakai **normal distribution** untuk nilai

**Contoh 1:**
- Mean = 40
- Nilai kamu = 90
- **Posisi kamu:** Jauh di kanan → **Kemungkinan besar A**

**Contoh 2:**
- Mean = 91 (soal gampang, banyak yang nilai tinggi)
- Nilai kamu = 90
- **Posisi kamu:** Di bawah rata-rata → **Bisa dapat B atau C**

**Kesimpulan:** Nilai absolut gak menentukan - yang penting **posisi relatif**!

---

### 📊 Membaca Normal Distribution

**Pertanyaan yang bisa dijawab:**

1. **Berapa orang yang nilainya > 80?**
   - Cari luas area di kanan titik 80

2. **95% siswa ada di range nilai berapa?**
   - Cari range yang cover 95% area di bawah kurva
   - Biasanya pakai **Z-table**

**Area di bawah kurva = Probability**

---

### 🧪 Aplikasi Lanjutan

- **Hypothesis Testing** (uji hipotesis)
- **Confidence Interval**
- **Quality Control**
- **A/B Testing**

---

## 🎯 5. BAYES' THEOREM

### Konsep Dasar: Venn Diagram

**Setup:**
- 100 orang di survei
- **Wibu** (suka anime Jepang) = 80 orang
- **Data Scientist** = 40 orang
- **Wibu DAN Data Scientist** = 20 orang

```
┌─────────────────────────────┐
│  Wibu (80)                  │
│  ┌───────────┐              │
│  │ Wibu & DS │              │
│  │   (20)    │──────┐       │
│  └───────────┘      │       │
│                     │  DS   │
│                     │ (40)  │
│                     └───────┘
└─────────────────────────────┘
```

**Breakdown:**
- Wibu saja (bukan DS) = 80 - 20 = **60 orang**
- DS saja (bukan Wibu) = 40 - 20 = **20 orang**
- Wibu DAN DS = **20 orang**

---

### 📐 Dalam Probability

```
P(Wibu) = 0.8
P(DS) = 0.4
P(Wibu ∩ DS) = 0.2
```

---

### 🤔 Pertanyaan Bayes

#### **Pertanyaan 1:**
Kita ketemu **random orang**, berapa P(dia DS)?

**Jawab:** P(DS) = 0.4 ✓

---

#### **Pertanyaan 2:**
Kita tahu dia **Wibu**, berapa P(dia DS)?

**Notasi:** P(DS | Wibu) = ?

**Cara berpikir:**
- Kita tahu dia Wibu → Fokus cuma ke 80 orang Wibu
- Dari 80 orang Wibu, berapa yang DS? → 20 orang
- P(DS | Wibu) = 20/80 = **0.25**

**Formula:**
```
P(DS | Wibu) = P(DS ∩ Wibu) / P(Wibu)
             = 0.2 / 0.8
             = 0.25
```

---

#### **Pertanyaan 3:**
Kita tahu dia **DS**, berapa P(dia Wibu)?

**Notasi:** P(Wibu | DS) = ?

**Cara berpikir:**
- Fokus ke 40 orang DS
- Dari 40 orang DS, berapa yang Wibu? → 20 orang
- P(Wibu | DS) = 20/40 = **0.5**

**Formula:**
```
P(Wibu | DS) = P(Wibu ∩ DS) / P(DS)
             = 0.2 / 0.4
             = 0.5
```

---

### 🎓 Rumus Bayes' Theorem

**Dari definisi:**
```
P(A | B) = P(A ∩ B) / P(B)
P(B | A) = P(B ∩ A) / P(A)
```

**Karena P(A ∩ B) = P(B ∩ A), maka:**
```
P(A ∩ B) = P(A | B) × P(B)
P(B ∩ A) = P(B | A) × P(A)
```

**Sehingga:**
```
P(A | B) × P(B) = P(B | A) × P(A)
```

### 🎯 RUMUS AKHIR BAYES:
```
┌─────────────────────────────────┐
│  P(A | B) = P(B | A) × P(A)    │
│             ─────────────────    │
│                  P(B)            │
└─────────────────────────────────┘
```

**Kegunaan:** **Membalik conditional probability!**

---

### 💡 Aplikasi Bayes' Theorem

#### 1. **Coffee & Energy**
```
Tahu: P(Energetic | Minum Kopi)
Cari: P(Minum Kopi | Energetic)
```

#### 2. **Zodiak & Kepribadian**
```
Tahu: P(Pemalu | Libra)
Cari: P(Libra | Pemalu)
```

#### 3. **Quality Control - Mesin**
```
Tahu: P(Defect | Mesin A)
Cari: P(Mesin A | Defect)

Aplikasi: Ada produk defect, dari mesin mana?
```

---

### 🤖 Aplikasi di NLP (Natural Language Processing)

**Konsep:** Prediksi kata berikutnya

**Contoh:**
```
"I am ___"

P(hungry | "I am") = ?
P(tired | "I am") = ?
P(a | "I am") = 0.9  ← Kemungkinan besar!
```

**Cara Kerja ChatGPT (simplified):**
1. Baca semua artikel di internet
2. Hitung P(kata B | kata A) untuk semua kombinasi
3. Saat dapat input, pilih kata dengan P tertinggi
4. Ulangi untuk kata berikutnya

**Contoh:**
```
Input: "Aku adalah dalam bahasa Indonesia"
Next word?

Coba semua kata:
- P("berarti" | "...Indonesia") = 0.4  ← TERTINGGI
- P("yaitu" | "...Indonesia") = 0.2
- P("adalah" | "...Indonesia") = 0.1

Output: "Aku adalah dalam bahasa Indonesia berarti"
```

---

## 🎯 RANGKUMAN

### ✅ Binomial Distribution
- **2 kemungkinan:** Sukses/Gagal
- **Berkali-kali** & **independen**
- Koefisien dari **Segitiga Pascal**
- Rumus: `P = C × p^y × (1-p)^(n-y)`
- Aplikasi: Testing, QA, predictions

### ✅ Normal Distribution
- Bentuk **lonceng**
- Terjadi di **alam**
- Parameter: **μ (mean)** & **σ (std dev)**
- Aplikasi: Penilaian, statistik, quality control

### ✅ Bayes' Theorem
- **Membalik** conditional probability
- Rumus: `P(A|B) = P(B|A) × P(A) / P(B)`
- Aplikasi: NLP, machine learning, diagnostics

---

## 📝 Tips Belajar

1. **Jangan hafal rumus** - Pahami **intuisinya**!
2. **Gunakan visualisasi** - Gambar diagram, grafik
3. **Practice** dengan contoh real-life
4. **Hubungkan konsep** - Lihat pola antar materi

---

## 🔜 Next Session
- **Pandas** - Library untuk data processing
- **Data Visualization** - Bikin grafik keren!
- Materi **praktik** 🎉

---

**"Belajar statistik itu kayak petualangan - kita nemuin rumusnya bareng, bukan langsung dikasih!" - Pahlevi Fikri**