# 13. RF Planning Parameter - LTE Network Design

## 1. Pengantar RF Planning

**RF Planning** = Radio Frequency Planning

Ini adalah proses **mendesain jaringan radio** supaya coverage bagus dan kapasitas mencukupi! 📶

RF Planning sangat terkait dengan **RNO** (Radio Network Optimization) - keduanya saling mendukung.

---

## 2. Hubungan RF Planning dengan RNO

### Masalah Jaringan Ada 2 Jenis:

#### A. Masalah Software

- Parameter RF planning yang tidak optimal
- Konfigurasi yang salah
- **Solusi:** RF Planning Engineer

#### B. Masalah Hardware

- Antenna rusak/salah posisi
- Kabel bermasalah
- **Solusi:** Rigging Team (Drive Test Engineer)

**Data dari RF Planning → Dipakai oleh RNO → Dipakai oleh Drive Test**

---

## 3. Tools untuk RF Planning

### Software Planning yang Populer:

| Vendor                | Tool                           |
| --------------------- | ------------------------------ |
| **Forsk (Universal)** | Atoll ⭐⭐⭐ (Paling populer!) |
| **Nokia**             | NetAct Planner                 |
| **Huawei**            | U-Net                          |
| **ZTE**               | -                              |
| **Ericsson**          | TEMS Planning / Asset          |
| **Universal**         | Planet (Mahal tapi powerful!)  |

### Supporting Tools:

- **MapInfo** (untuk peta detail)
- **Google Earth** (untuk survey lokasi)
- **Excel** (untuk kalkulasi manual)

**Data hasil RF Planning dipakai oleh:**

- Tim RF Optimization
- Tim Drive Test
- Tim OSS (Operating Support System)

---

## 4. Tugas RF Planning Engineer

### 7 Tugas Utama:

#### 1. Planning New Site 📍

- Tentukan lokasi site baru
- Survey lapangan

#### 2. Determine Physical Parameters 🔧

- Tinggi antenna
- Tipe antenna
- Azimuth (arah antenna)
- Tilt (kemiringan antenna)
- **Data ini untuk Tim Rigging**

#### 3. Menghitung Physical Cell ID (PCI) 🆔

- Menentukan nomor ID untuk setiap cell
- Supaya tidak ada interferensi

#### 4. Network Planning (Coverage & Capacity) 📊

- Pakai Atoll untuk simulasi
- Cari area yang ada interference
- Cari area yang belum ter-cover

#### 5. Capacity Planning (Ekspansi) 📈

- Kalau ada penambahan user
- Perlu tambah site atau tidak?

#### 6. Frequency Planning 🎵

- Tentukan frekuensi kerja (900, 1800, 2100, 2600 MHz)
- Menentukan **propagation model**:
  - < 1500 MHz → **Okumura-Hata**
  - > 1500 MHz → **COST 231-Hata**

#### 7. Link Budget Calculation 💰

- Hitung daya yang dibutuhkan
- Hitung jangkauan maksimal
- **Bagian paling krusial!**

---

## 5. Data Input untuk RF Planning

RF Planning Engineer butuh data ini:

### Traffic Data:

- **Traffic sharing** (kapasitas traffic per cell)
- Berapa banyak user per area?

### Site Data:

- **Tinggi antenna** (tower height)
- **Downtilt** (kemiringan antenna)
- **Carrier** yang dipakai (berapa frekuensi?)

### Expansion Data:

- Ada penambahan BTS atau tidak?
- Ada **cell splitting** atau tidak?
- Apakah perlu **dual band/multi band**?

---

## 6. OSS & BSS (Support System)

### A. OSS (Operating Support System)

**OSS** = Sistem monitoring jaringan

**Tugas OSS Engineer:**

- Monitor **database** jaringan
- Cek **alarm** (ada 3 level):
  1. **Minor** (kecil, bisa langsung ditangani)
  2. **Major** (sedang, eskalasi ke Manager)
  3. **Critical** (parah, eskalasi sampai Direktur!)
- Monitor **capacity & performance**

**Tempat kerja:** NOC (Network Operation Center)

- Ruangan penuh dengan **layar monitor**
- Monitor seluruh Indonesia real-time!
- Kalau ada site down → Merah di peta!

### B. BSS (Business Support System)

**BSS** = Sistem bisnis operator

Fokus ke:

- Billing
- Customer service
- Revenue management

### Tools OSS:

| Vendor       | Tool                           |
| ------------ | ------------------------------ |
| **Huawei**   | U2000                          |
| **ZTE**      | ZXUN iManager                  |
| **Ericsson** | ENM (Ericsson Network Manager) |
| **Nokia**    | NetAct                         |

**Data dari OSS → Dipakai oleh RF Optimization untuk analisa**

---

## 7. Drive Test Engineer

**Drive Test** = Pengukuran jaringan di lapangan dengan kendaraan

### Kapan Drive Test Dilakukan?

1. **Sebelum RF Planning** (survey awal)
2. **Setelah site dibangun** (acceptance test)
3. **Saat ada komplain user**
4. **Untuk optimasi rutin**

### Tools Drive Test:

- **TEMS Investigation** ⭐ (Paling populer)
- **NEMO**
- **SwissQual**

**Data Drive Test → Dipakai oleh RNO untuk optimasi**

---

## 8. Rigging Team (Site Installation)

**Rigging Team** = Tim yang naik tower untuk install/maintenance antenna

### Tugas Rigging:

- **Install antenna** di tower
- **Ubah tilt antenna** (mechanical/electrical)
- **Perbaiki kabel** yang bermasalah
- **Survey kondisi site**

### 2 Jenis Tilting:

#### A. Mechanical Tilt

- **Manual**: Harus naik tower
- Putar antenna secara fisik
- Paling **akurat**

#### B. Electrical Tilt

- **Remote**: Dari kantor (via software)
- Tidak perlu naik tower
- Lebih **praktis** tapi kurang akurat

### Tools Rigging:

- Kompas (untuk azimuth)
- GPS (untuk koordinat)
- Kamera (dokumentasi)
- Tali safety (keselamatan!)

**Rigging harus berani naik tower!** 🧗

---

## 9. LTE Radio Protocol Stack (Review)

Sebelum masuk RF Planning detail, kita review dulu **protocol stack**:

### 3 Layer Utama (dari bawah):

#### 1. Physical Channel (Layer paling bawah)

- Hardware fisik
- Transmisi sinyal radio

#### 2. Transport Channel (Layer tengah)

- Hubungkan Physical dengan MAC layer
- Mengelola data transport

#### 3. Logical Channel (Layer atas)

- Control signaling
- Traffic data

**Konsep OSI Layer → Diterapkan di LTE!**

### Control Plane vs User Plane:

| Plane             | Fungsi                           |
| ----------------- | -------------------------------- |
| **Control Plane** | Signaling, setup connection      |
| **User Plane**    | Data user (browsing, video, dll) |

**Keduanya terpisah di RRC dan NAS layer!**

---

## 10. Parameter RF Planning Utama

Ada **7 parameter penting** untuk RF Planning:

---

### Parameter 1: RSRP (Reference Signal Received Power)

**RSRP** = Daya sinyal yang diterima oleh user

**Range nilai:**

- **-44 dBm** (sangat bagus) ✅
- **-140 dBm** (sangat buruk) ❌

**Semakin tinggi (mendekati -44) → Semakin bagus!**

**Fungsi:**

- Menentukan **coverage** area
- Menentukan apakah perlu tambah site

**Cara hitung:**

```
RSRP = RSSI - 10 × log10(12 × N)
```

Dimana:

- **RSSI** = Total power yang diterima
- **N** = Jumlah Resource Block (RB)

**RSSI = RSRP + Interference + Noise**

---

### Parameter 2: RSRQ (Reference Signal Received Quality)

**RSRQ** = Kualitas sinyal yang diterima

**Range nilai:**

- **-3 dB** (sangat bagus) ✅
- **-19.5 dB** (sangat buruk) ❌

**Semakin tinggi (mendekati -3) → Semakin bagus!**

**Fungsi:**

- Mengukur **kualitas** sinyal (bukan daya)
- Menentukan **user experience**

**Cara hitung:**

```
RSRQ = (N × RSRP) / RSSI
```

Dimana:

- **N** = Jumlah Resource Block

**RSRQ melihat perbandingan sinyal vs interference!**

---

### Parameter 3: SINR (Signal to Interference & Noise Ratio)

**SINR** = Rasio sinyal terhadap interference + noise

**Range nilai:**

- **5 dB** (minimum)
- **20 dB** (sangat bagus) ✅

**Semakin tinggi → Semakin bagus!**

**Fungsi:**

- Menentukan **kualitas koneksi**
- Mengukur **interferensi**

### Penyebab Interferensi:

#### A. Co-Channel Interference

- **2 site** menggunakan **frekuensi yang sama**
- User di tengah dapat sinyal dari keduanya
- Sinyal **saling ganggu**

**Solusi:**

- **Neighbour Cell Planning**
- **PCI Planning** (supaya tidak sama)

#### B. Adjacent Channel Interference

- Frekuensi operator lain **"bocor"** ke frekuensi kita
- Contoh: Frekuensi XL masuk ke Telkomsel
- **Kominfo** akan monitoring ini!
- Kalau ketahuan → **Penalti** sampai ijin dicabut!

### Noise:

**Noise** = Sinyal yang tidak dikehendaki (gangguan)

Bisa dari:

- Perangkat elektronik
- Cuaca (petir)
- Interferensi lain

**Cara hitung SINR:**

```
SINR = Signal Power / (Interference + Noise)
```

---

### Parameter 4: CQI (Channel Quality Indicator)

**CQI** = Indikator kualitas channel pada **dedicated mode**

**Dedicated Mode** = Saat user sedang aktif download/upload

**Fungsi CQI:**

- Memberikan info tentang **modulasi** yang dipakai
- Memberikan info tentang **code rate**
- Menentukan **efisiensi** transmisi

### Modulasi LTE:

LTE punya **3 jenis modulasi**:

| Modulasi    | Kondisi            | Jarak ke Site         |
| ----------- | ------------------ | --------------------- |
| **QPSK**    | Paling jelek       | **Jauh** (worst case) |
| **16-QAM**  | Sedang             | Sedang                |
| **64-QAM**  | Paling bagus       | **Dekat**             |
| **256-QAM** | Terbaik (4x4 MIMO) | Sangat dekat          |

**Aturan:**

- **Jauh dari site** → Dapat QPSK (paling lambat)
- **Dekat dengan site** → Dapat 64-QAM (paling cepat)

**Kenapa pakai QPSK sebagai baseline?**

- Kalau QPSK sudah bagus → Yang lain pasti bagus!
- Jarak terjauh → Kondisi terjelek
- **Best practice untuk planning**

---

### Parameter 5: PCI (Physical Cell ID)

**PCI** = Kode identitas unik untuk setiap cell

**Fungsi:**

- **Identifikasi cell** (supaya tidak bingung)
- **Mencegah interference**

### Aturan PCI:

#### Total PCI Available:

- **504 Physical Cell ID** (0-503)
- Dibagi dalam **168 group** (0-167)
- Setiap group punya **3 identity** (0, 1, 2)

**Formula:**

```
Total PCI = 168 × 3 = 504
```

#### PCI Components:

1. **PSS (Primary Synchronization Signal)**
   - Value: **0, 1, 2**
2. **SSS (Secondary Synchronization Signal)**
   - Value: **0 - 167**

**Formula PCI:**

```
PCI = 3 × N_ID(1) + N_ID(2)
```

Dimana:

- **N_ID(1)** = SSS (0-167)
- **N_ID(2)** = PSS (0-2)

### PCI Planning Rules:

#### Rule #1: PCI Harus Unik

- **2 site bertetangga** tidak boleh punya PCI yang sama!
- Kalau terpaksa sama → Jarak harus jauh

#### Rule #2: Minimum Re-use Distance

- Kalau mau pakai PCI yang sama lagi:

```
Minimum Distance = 6.8 × ISD (Inter-Site Distance)
```

**Contoh:**

- ISD = 500 meter
- Minimum distance = 6.8 × 500 = 3400 meter = 3.4 km

**Kalau jarak < 3.4 km dan PCI sama → Call Handover Failed!**

### PCI Numbering Convention:

**Indoor:**

- **360 - 503** (untuk indoor)

**Outdoor:**

- **0 - 359** (untuk outdoor)

**Total:**

- Indoor: 144 PCI
- Outdoor: 360 PCI

---

### Parameter 6: PRACH (Physical Random Access Channel)

**PRACH** = Channel untuk random access

**PRACH** ada di **Physical Channel Layer** (paling bawah)

### Kapan PRACH Dipakai?

#### Scenario 1: RRC State Transition

- Dari **Idle** ke **Connected**
- Saat user mau mulai komunikasi

#### Scenario 2: Handover

- Saat perpindah antar cell
- **Sangat penting** untuk handover sukses!

#### Scenario 3: Uplink/Downlink Synchronization

- User harus **synchronized** dengan network
- Kalau tidak sync → Connection failed!

#### Scenario 4: Prevent Connection Failure

- Mencegah kegagalan saat membangun koneksi RRC

**PRACH Planning sangat penting supaya handover tidak gagal!**

---

## 11. PCI Planning - Step by Step

### Langkah Planning PCI:

#### Step 1: Tentukan N_ID(1) (SSS)

**SSS Range:** 0 - 167

#### Step 2: Tentukan N_ID(2) (PSS)

**PSS Range:** 0 - 2

#### Step 3: Hitung PCI

```
PCI = 3 × N_ID(1) + N_ID(2)
```

**Contoh 1:**

```
N_ID(1) = 50
N_ID(2) = 1

PCI = 3 × 50 + 1 = 151
```

**Contoh 2:**

```
N_ID(1) = 100
N_ID(2) = 2

PCI = 3 × 100 + 2 = 302
```

#### Step 4: Pastikan PCI Tidak Bentrok

- Cek **neighbour cell**
- Pastikan **jarak cukup** kalau mau re-use PCI

---

## 12. PRACH Planning - Manual Calculation

### Data Dasar PRACH:

#### 1. Total Root Sequence Available:

- **839** root sequences (0-838)
- Defined by 3GPP

#### 2. PRACH Preamble per Cell:

- **64 preamble** per cell (LTE standard)

#### 3. PRACH Uses 6 PRB:

- Butuh **6 Resource Block**

#### 4. Timing Estimation:

- **2 microseconds** (default)

---

### Formula PRACH Planning:

**Step 1: Hitung N_CS**

```
N_CS > 1.04875 × (6.67 × R + T_MD + 2)
```

Dimana:

- **R** = Radius coverage (km)
- **T_MD** = 5 ms (untuk delay maksimum)
- **6.67** = Konstanta timing
- **1.04875** = Konstanta konversi

**Step 2: Cari N_CS Configuration**

Lihat di tabel → Cari nilai yang **lebih besar** dari hasil perhitungan

**Step 3: Tentukan Number of Root Sequences**

```
Number of Root Sequences = 839 / N_CS (dibulatkan ke bawah)
```

**Step 4: Tentukan Z_CS (Cyclic Shift)**

```
Z_CS = 64 / Number of Root Sequences (dibulatkan ke bawah)
```

**Step 5: Hitung Jumlah Site**

```
Jumlah Site = 838 / Z_CS (dibulatkan ke bawah)
```

**Step 6: Tentukan Index**

Index adalah **kelipatan dari Z_CS**

---

## 13. Contoh Perhitungan PRACH: Radius 10 KM

### Given:

- **Radius (R)** = 10 km
- **T_MD** = 5 ms
- **Timing** = 2 μs

### Step 1: Hitung N_CS

```
N_CS > 1.04875 × (6.67 × 10 + 5 + 2)
N_CS > 1.04875 × (66.7 + 5 + 2)
N_CS > 1.04875 × 73.7
N_CS > 77.29
```

**N_CS harus > 77.29**

### Step 2: Cari di Tabel

Lihat tabel PRACH Configuration:

- N_CS = **76** (terlalu kecil ❌)
- N_CS = **93** (oke! ✅)

**Pilih yang lebih besar: N_CS = 93**

### Step 3: N_CS Configuration

Dari tabel, N_CS = 93 → **N_CS Config = 11**

### Step 4: Number of Root Sequences

```
Root Sequences = 839 / 93
Root Sequences = 9.02
Root Sequences = 9 (dibulatkan ke bawah)
```

**Root Sequences = 9**

### Step 5: Z_CS (Cyclic Shift)

```
Z_CS = 64 / 9
Z_CS = 7.11
Z_CS = 7 (dibulatkan ke bawah)

Tapi dari tabel → Z_CS = 8 (lebih aman)
```

**Z_CS = 8**

### Step 6: Jumlah Site

```
Jumlah Site = 838 / 8
Jumlah Site = 104.75
Jumlah Site = 104 (dibulatkan ke bawah)
```

**Hasil:**

- **Jumlah site maksimal**: 104 site
- **Index**: Kelipatan 8 (0, 8, 16, 24, 32, ...)
- **Cell Radius**: 12.2 km (dari tabel)

---

## 14. Contoh Perhitungan PRACH: Radius 8 KM

### Given:

- **Radius (R)** = 8 km
- **T_MD** = 5 ms
- **Timing** = 2 μs

### Step 1: Hitung N_CS

```
N_CS > 1.04875 × (6.67 × 8 + 5 + 2)
N_CS > 1.04875 × (53.36 + 5 + 2)
N_CS > 1.04875 × 60.36
N_CS > 63.30
```

**N_CS harus > 63.30**

### Step 2: Cari di Tabel

Dari tabel:

- N_CS = **76** (oke! ✅)

**Pilih N_CS = 76**

### Step 3: N_CS Configuration

Dari tabel, N_CS = 76 → **N_CS Config = 10**

### Step 4: Number of Root Sequences

```
Root Sequences = 839 / 76
Root Sequences = 11.04
Root Sequences = 11
```

**Root Sequences = 11**

### Step 5: Z_CS

Dari tabel → **Z_CS = 6**

### Step 6: Jumlah Site

```
Jumlah Site = 838 / 6
Jumlah Site = 139.67
Jumlah Site = 139
```

**Hasil:**

- **Jumlah site maksimal**: 139 site
- **Index**: Kelipatan 6 (0, 6, 12, 18, 24, ...)
- **Cell Radius**: 9.77 km (dari tabel)

---

## 15. Tabel PRACH Configuration (Ringkasan)

| Cell Radius (km) | N_CS | N_CS Config | Root Seq | Z_CS | Jumlah Site |
| ---------------- | ---- | ----------- | -------- | ---- | ----------- |
| 0.76             | 839  | 15          | 1        | 64   | 13          |
| 1.94             | 419  | 14          | 2        | 32   | 26          |
| 4.13             | 209  | 13          | 4        | 16   | 52          |
| 7.04             | 139  | 12          | 6        | 11   | 76          |
| 9.77             | 76   | 10          | 11       | 6    | 139         |
| 12.2             | 93   | 11          | 9        | 8    | 104         |
| 18.9             | -    | -           | -        | -    | -           |

**Ini adalah tabel standard dari 3GPP!**

---

## 16. Logical Channel (Review)

**Logical Channel** = Channel di layer atas

### Ada 7 Logical Channel:

#### Bidirectional (Biru):

1. **DTCH** (Dedicated Traffic Channel) - Traffic user
2. **DCCH** (Dedicated Control Channel) - Control per user
3. **CCCH** (Common Control Channel) - Control broadcast

#### Downlink Only (Hijau):

4. **BCCH** (Broadcast Control Channel) - Info broadcast
5. **PCCH** (Paging Control Channel) - Paging
6. **MTCH** (Multicast Traffic Channel) - Multicast
7. **MCCH** (Multicast Control Channel) - Control multicast

**Yang paling penting: DTCH, DCCH, CCCH**

---

## 17. Transport Channel (Review)

**Transport Channel** = Penghubung MAC dan Physical Layer

### Ada 6 Transport Channel:

#### Downlink (Hijau):

1. **DL-SCH** (Downlink Shared Channel)
2. **PCH** (Paging Channel)
3. **BCH** (Broadcast Channel)
4. **MCH** (Multicast Channel)

#### Uplink (Biru):

5. **UL-SCH** (Uplink Shared Channel)
6. **RACH** (Random Access Channel)

**Transport Channel menghubungkan MAC dengan Physical!**

---

## 18. Physical Channel (Review)

**Physical Channel** = Channel di hardware fisik

### Physical Channel Penting:

#### Downlink:

- **PDSCH** (Physical Downlink Shared Channel)
- **PDCCH** (Physical Downlink Control Channel)
- **PBCH** (Physical Broadcast Channel)
- **PMCH** (Physical Multicast Channel)

#### Uplink:

- **PUSCH** (Physical Uplink Shared Channel)
- **PUCCH** (Physical Uplink Control Channel)
- **PRACH** (Physical Random Access Channel) ⭐

**PRACH sangat penting untuk handover & random access!**

---

## 19. Tips Interview RF Planning

### Yang Harus Dikuasai:

1. **Parameter RF:**

   - RSRP, RSRQ, SINR, CQI
   - Tahu bedanya dan fungsinya

2. **PCI Planning:**

   - Formula: PCI = 3 × N_ID(1) + N_ID(2)
   - Total 504 PCI available
   - Aturan re-use distance

3. **PRACH Planning:**

   - Tahu langkah perhitungan manual
   - Paham konsep N_CS, Root Sequence, Z_CS

4. **Tools:**

   - Minimal tahu **Atoll**
   - Tahu tools vendor (U-Net, NetAct, dll)

5. **Konsep Dasar:**
   - Propagation model (Okumura-Hata vs COST 231)
   - Link budget
   - Coverage vs Capacity

### Cara Jawab Interview:

**Pertanyaan:** "Apa tugas RF Planning Engineer?"

**Jawaban bagus:**

> "RF Planning Engineer bertanggung jawab untuk mendesain jaringan radio, mulai dari site selection, frequency planning, PCI planning, PRACH planning, sampai link budget calculation. Tools yang digunakan seperti Atoll untuk coverage simulation. Hasilnya dipakai oleh tim RNO dan Drive Test untuk optimasi."

**Jangan jawab:**

> "RF Planning itu desain jaringan." ❌

**Pertanyaan:** "Apa bedanya RSRP dengan RSRQ?"

**Jawaban bagus:**

> "RSRP mengukur **power** atau daya sinyal yang diterima, dalam satuan dBm. Range -44 sampai -140 dBm. RSRQ mengukur **quality** atau kualitas sinyal, dalam satuan dB. Range -3 sampai -19.5 dB. RSRP untuk coverage, RSRQ untuk user experience. Formula RSRQ = (N × RSRP) / RSSI."

---

## 20. Latihan Soal

### Soal 1: PCI Planning

**Diketahui:**

- N_ID(1) = 75
- N_ID(2) = 2

**Hitunglah PCI!**

**Jawaban:**

```
PCI = 3 × 75 + 2
PCI = 225 + 2
PCI = 227
```

**PCI = 227** ✅

---

### Soal 2: PRACH Planning (Radius 5 km)

**Diketahui:**

- Radius = 5 km
- T_MD = 5 ms
- Timing = 2 μs

**Hitunglah:**

1. N_CS
2. Root Sequences
3. Z_CS
4. Jumlah Site

**Jawaban:**

**1. Hitung N_CS:**

```
N_CS > 1.04875 × (6.67 × 5 + 5 + 2)
N_CS > 1.04875 × (33.35 + 7)
N_CS > 1.04875 × 40.35
N_CS > 42.32
```

**2. Dari tabel:** N_CS = 139 (pilih yang ≥ 42.32)

**3. Root Sequences:**

```
= 839 / 139
= 6.04
= 6
```

**4. Z_CS dari tabel:** Z_CS = 11

**5. Jumlah Site:**

```
= 838 / 11
= 76.18
= 76 site
```

**Hasil:**

- **N_CS = 139**
- **Root Sequences = 6**
- **Z_CS = 11**
- **Jumlah Site = 76**
- **Index = Kelipatan 11** (0, 11, 22, 33, ...)

---

## 21. Checklist RF Planning

Sebelum submit design, cek ini:

### Coverage Planning:

- ☑ RSRP target defined (-85 dBm minimum?)
- ☑ RSRQ target defined (-10 dB minimum?)
- ☑ SINR target defined (15 dB minimum?)
- ☑ Coverage map generated (via Atoll)

### PCI Planning:

- ☑ PCI assigned to all cells
- ☑ No duplicate PCI in neighbour cells
- ☑ Re-use distance calculated
- ☑ Indoor/Outdoor numbering convention followed

### PRACH Planning:

- ☑ Cell radius determined
- ☑ N_CS calculated
- ☑ Root sequences assigned
- ☑ Z_CS determined
- ☑ Index spacing confirmed

### Link Budget:

- ☑ Transmit power defined
- ☑ Antenna gain calculated
- ☑ Path loss calculated
- ☑ Receive sensitivity defined
- ☑ Link margin adequate (> 10 dB)

### Documentation:

- ☑ Site list complete
- ☑ Parameter list ready
- ☑ Coverage map exported
- ☑ BoQ (Bill of Quantity) prepared

---

## 22. Kesimpulan

### RF Planning Itu:

- ✅ **Desain jaringan radio** untuk coverage & capacity
- ✅ **Hitung parameter** (PCI, PRACH, Link Budget)
- ✅ **Gunakan tools** (Atoll, U-Net, dll)
- ✅ **Support RNO** dengan data parameter
- ✅ **Koordinasi dengan** Drive Test & OSS Team

### Formula Kunci:

```
PCI = 3 × N_ID(1) + N_ID(2)

N_CS > 1.04875 × (6.67 × R + T_MD + 2)

RSRQ = (N × RSRP) / RSSI

SINR = Signal / (Interference + Noise)
```

### Yang Harus Dikuasai:

1. **Teori dasar** (RSRP, RSRQ, SINR, CQI)
2. **PCI Planning** (formula & rules)
3. **PRACH Planning** (manual calculation)
4. **Tools** (minimal Atoll)
5. **Link Budget** (akan dibahas lebih detail)

### Next Step:

- **Part 2**: Link Budget Calculation (detail)
- **Part 3**: Praktik dengan Atoll
- **Part 4**: Advanced Optimization Techniques

---

## 23. Motivasi Akhir

**RF Planning Engineer** adalah **arsitek jaringan**! 🏗️

Tanpa RF Planning yang baik:

- ❌ Coverage buruk
- ❌ Interference tinggi
- ❌ User complain
- ❌ Site placement tidak optimal

Dengan RF Planning yang baik:

- ✅ Coverage optimal
- ✅ No interference
- ✅ User puas
- ✅ Efisiensi investasi

**Pro tip:**

- **Jangan cuma bisa pakai tools!** Harus paham **konsep dasar**
- **Tools itu gampang**, yang susah **analisa hasil**
- **RF Planning bukan cuma klik-klik** Atoll!
- Harus bisa jawab: **"Kenapa hasilnya begini?"**

**Perbedaan Engineer S1 vs D3:**

- **D3**: Bisa menjalankan tools (klik-klik)
- **S1**: Bisa **analisa & decision making** 💡

**RF Planning = Foundation of Network Excellence!** 📡

---

**Good luck untuk belajar RF Planning!** 🚀

**Semoga catatan ini bermanfaat untuk kuliah, magang, dan karir!** 💙

---

## 24. Resources Tambahan

### Standards:

- **3GPP TS 36.211** (Physical Channels)
- **3GPP TS 36.321** (MAC Protocol)
- **3GPP TS 36.331** (RRC Protocol)

### Books:

- "LTE for UMTS" - Harri Holma
- "RF Planning and Optimization" - Morten Tolstrup
- "4G LTE/LTE-Advanced" - Arunabha Ghosh

### Online:

- ShareTechnote (excellent RF tutorials!)
- Forsk Atoll Documentation
- Vendor white papers (Huawei, Ericsson, Nokia)

### Sertifikasi:

- **RF Planning & Optimization** (Huawei, Ericsson, Nokia)
- **Drive Test Engineer** (TEMS certified)
- **Network Planning** (Atoll certified)

---

**Catatan penting:**

- Materi ini **berbasis teori + praktek real**
- Data dari **3GPP standard + vendor documentation**
- Perhitungan manual **harus dikuasai** sebelum pakai tools
- **Jangan skip teori** - ini yang membedakan Engineer sejati!

**Keep learning and stay curious!** 🔥📚
