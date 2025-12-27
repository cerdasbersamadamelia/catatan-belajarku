# 14. Link Budget LTE - Coverage & Capacity Planning

## 1. Pengantar Link Budget

**Link Budget** = Perhitungan untuk menentukan **berapa daya yang dibutuhkan** supaya sinyal bisa sampai ke user dengan baik! 📡

**Tujuan Link Budget:**

- Menghitung **path loss** (rugi-rugi sinyal)
- Menentukan **coverage area** (jangkauan)
- Menentukan **jumlah site** yang dibutuhkan
- Menghitung **power budget** (daya yang diperlukan)

**Link Budget sangat penting sebelum kita pakai tools seperti Atoll!**

---

## 2. Komparasi Jaringan 2G/3G/4G

### Arsitektur Jaringan:

```
Access → Aggregation → Core
```

**3 Bagian Utama:**

- **Access**: BTS/NodeB/eNodeB (site)
- **Aggregation**: Transport network (MPLS, Metro Ethernet)
- **Core**: MSC/SGSN/MME + S-GW

### Perbedaan Utama:

| Aspek          | 2G/3G          | 4G (LTE)                  |
| -------------- | -------------- | ------------------------- |
| **Domain**     | CS + PS        | **PS Only** (All IP)      |
| **Voice**      | Circuit Switch | **VoLTE** (IP based)      |
| **Controller** | BSC/RNC        | **No Controller!** (Flat) |
| **Interface**  | X2 antar RNC   | **X2 antar eNodeB**       |
| **Latency**    | Tinggi         | **Rendah**                |

**4G = Full IP Network!**

---

## 3. RAN Equipment (Base Station)

### Komponen BTS/eNodeB:

**1. BBU (Baseband Unit)**

- Otak dari BTS
- Processing digital signal
- Control & management

**2. RRU (Remote Radio Unit)**

- Radio transceiver
- Transmit & receive RF signal
- Ada di dekat antenna

**3. Antenna**

- Memancarkan sinyal RF
- 3 sector per site (biasanya)

### Slot di dalam BBU:

| Slot     | Fungsi                               |
| -------- | ------------------------------------ |
| **LBBP** | LTE Baseband Processing Unit         |
| **UBBP** | UMTS Baseband Processing Unit        |
| **GBBP** | GSM Baseband Processing Unit         |
| **LMPT** | LTE Main Processing & Transmission   |
| **UPEU** | Universal Power & Environment Unit   |
| **UIUB** | Universal Interface Unit (for X2/S1) |

**SDR (Software Defined Radio):** Tinggal ganti modul untuk upgrade teknologi!

---

## 4. Layering & Multi-Band LTE

### Frequency Layering:

**L900** = LTE di 900 MHz

- Coverage **luas**
- Bandwidth **kecil** (max 7.5 MHz)
- Untuk **rural area**

**L1800** = LTE di 1800 MHz ⭐

- Coverage **sedang**
- Bandwidth **besar** (max 20 MHz)
- Paling **populer** untuk LTE!

**L2100** = LTE di 2100 MHz

- Coverage **sedang**
- Bandwidth **10-15 MHz**

**L2300** = LTE di 2300 MHz

- Coverage **kecil**
- Bandwidth **20 MHz**
- Untuk **capacity**

### Kenapa 1800 MHz Paling Populer?

- ✅ Bandwidth bisa sampai **20 MHz** (paling besar!)
- ✅ Coverage **cukup luas**
- ✅ Cocok untuk **urban & suburban**
- ✅ Bisa dapat **100 Resource Block**

---

## 5. Refarming Spektrum

**Refarming** = Penataan ulang frekuensi

### Proses Refarming:

**Sebelum:**

```
[Telkomsel 10MHz] [Gap] [Smartfren 10MHz] [Gap] [Telkomsel 10MHz]
```

**Sesudah Refarming:**

```
[Telkomsel 20MHz Continuous] [Gap] [Smartfren 20MHz]
```

**Manfaat:**

- **Bandwidth kontinyu** (tidak terputus)
- **Guard band** berkurang
- **Kapasitas** naik
- **Efisiensi** spektrum lebih baik

**Telkomsel dapat tambahan 2300 MHz → Perlu refarming supaya kontinyu!**

---

## 6. Tradisional RAN vs Open RAN

### Tradisional RAN:

**Vendor Lock-in:**

- BBU Huawei → Harus pakai RRU Huawei
- BBU Nokia → Harus pakai RRU Nokia
- BBU Ericsson → Harus pakai RRU Ericsson

**Tidak bisa mix & match!**

### Open RAN:

**Vendor Neutral:**

- BBU Huawei + RRU Nokia → **Bisa!** ✅
- BBU Ericsson + RRU Huawei → **Bisa!** ✅
- Kita bisa **bangun sendiri** dengan komponen berbeda

**Manfaat:**

- **Fleksibel**
- **Tidak tergantung satu vendor**
- **Cost lebih rendah**
- **Inovasi lebih cepat**

**Open RAN = Future of Telecom!** 🚀

**Contoh:** Rakuten (Jepang) sudah full Open RAN!

---

## 7. Site Topology LTE

### 4 Topologi Utama:

#### 1. Star Topology ⭐

```
       eNodeB 1
           |
       Transport
      /    |    \
eNodeB2 eNodeB3 eNodeB4
```

**Kelebihan:** Simple, mudah maintain
**Kekurangan:** Single point of failure

#### 2. Ring Topology 🔄

```
eNodeB1 — eNodeB2
   |           |
eNodeB4 — eNodeB3
```

**Kelebihan:** Redundancy, reliable
**Kekurangan:** Kompleks, mahal

#### 3. Chain Topology ⛓️

```
eNodeB1 → eNodeB2 → eNodeB3 → eNodeB4
```

**Kelebihan:** Murah untuk area memanjang
**Kekurangan:** Latency bertambah

#### 4. Tree Topology 🌳

```
        eNodeB1
       /      \
   eNodeB2  eNodeB3
   /    \      |
 eNB4  eNB5   eNB6
```

**Kelebihan:** Scalable, hierarkis
**Kekurangan:** Kompleksitas tinggi

### Pemilihan Topologi Tergantung:

- **Area type** (Urban, Suburban, Rural)
- **Traffic demand** (tinggi/rendah)
- **Budget** (capex & opex)
- **Backhaul availability** (fiber/microwave)

---

## 8. RF Planning Workflow

### 4 Tahap Utama:

```
1. Network Objective → 2. Network Planning → 3. Project Execution → 4. Network Optimization
```

#### Tahap 1: Network Objective 🎯

**Tentukan:**

- Area coverage yang mau di-cover
- Frekuensi yang akan digunakan
- Target user & capacity
- Use case (residential/industrial/tourist area)

**Contoh:**

> "Kita mau cover area 100 km² di daerah wisata Bali dengan L1800, target 10,000 user aktif di busy hour"

#### Tahap 2: Network Planning 📐

**Proses:**

- **Coverage planning** (berapa luas coverage per site?)
- **Capacity planning** (berapa user per site?)
- **Link budget calculation** (berapa daya yang dibutuhkan?)
- **Site location planning** (dimana lokasi site?)
- **Transport planning** (fiber/microwave?)

#### Tahap 3: Project Execution 🔨

**Implementasi di lapangan:**

- Site acquisition & survey
- Civil work (tower, shelter)
- Equipment installation
- Integration & testing
- Acceptance test

**Bisa ada perubahan dari planning!**

- Ada bangunan baru
- Kondisi lapangan berbeda
- Obstacle tidak terprediksi

#### Tahap 4: Network Optimization 🔧

**Proses optimasi karena:**

- Coverage tidak sesuai planning
- Ada complaint dari user
- Drive test menemukan masalah
- Interference dari site tetangga

**Cycle terus sampai optimal!**

---

## 9. KPI Network & User

### KPI Network (3 Pilar):

#### 1. Accessibility

- **RRC Success Rate** (user bisa akses jaringan?)
- **E-RAB Success Rate** (koneksi data berhasil?)

#### 2. Retainability

- **Call Drop Rate** (berapa % call yang putus?)
- **E-RAB Drop Rate** (koneksi data putus?)

#### 3. Mobility

- **Handover Success Rate**
- **Intra-LTE HO** (L1800 → L1800)
- **Inter-RAT HO** (LTE → 3G)
- **Inter-Freq HO** (L1800 → L2300)

### KPI User:

- **Throughput** (kecepatan download/upload)
- **Latency** (delay)
- **Call setup time** (berapa lama sampai tersambung?)

**Network bagus = KPI tinggi + User puas!** ✅

---

## 10. Tim RF Planning & Operations

### 4 Team Utama:

#### 1. RNP (Radio Network Planning)

**Tugas:**

- Pre-planning (survey awal)
- Planning (link budget, site location)
- Design (coverage & capacity)

#### 2. RNO (Radio Network Operations)

**Tugas:**

- Implementation (pasang site)
- Maintenance (perawatan)
- Optimization (tuning parameter)

#### 3. Drive Test Team

**Tugas:**

- Testing coverage di lapangan
- Ukur RSRP, RSRQ, SINR
- Generate recommendation

**Tools:** TEMS, Nemo, SwissQual

#### 4. Rigging Team

**Tugas:**

- Naik tower untuk install antenna
- Adjust tilt & azimuth
- Perbaikan fisik

**Harus berani naik tower!** 🧗

---

## 11. Coverage Planning vs Capacity Planning

### Coverage Planning 📡

**Fokus:** Berapa luas area yang bisa di-cover?

**Faktor:**

- **Frekuensi** (900/1800/2100/2300 MHz)
- **Power transmit** (berapa watt?)
- **Antenna gain** (berapa dBi?)
- **Path loss** (redaman sinyal)

**Formula:**

```
Site Area = f(Frequency, Power, Antenna, Path Loss)
```

**Contoh:**

- Area total: 100 km²
- 1 site cover: 10 km²
- **Butuh: 10 site**

### Capacity Planning 📊

**Fokus:** Berapa user yang bisa dilayani?

**Faktor:**

- **Bandwidth** (5/10/15/20 MHz)
- **Number of RB** (25/50/75/100)
- **Traffic model** (berapa Mbps per user?)
- **Busy hour** (jam sibuk)

**Formula:**

```
Site Capacity = f(Bandwidth, RB, Traffic Model, Busy Hour)
```

**Contoh:**

- Total 10,000 user
- 1 site handle: 1,000 user
- **Butuh: 10 site**

### Yang Menang = Yang Butuh Lebih Banyak!

**Skenario 1:**

- Coverage: Butuh 10 site
- Capacity: Butuh 15 site
- **Deploy: 15 site** ✅

**Skenario 2:**

- Coverage: Butuh 20 site
- Capacity: Butuh 8 site
- **Deploy: 20 site** ✅

---

## 12. Area Type Classification

### 3 Tipe Area:

#### 1. Urban (Kota) 🏙️

**Karakteristik:**

- Padat penduduk
- Banyak gedung tinggi
- Traffic **tinggi**
- Coverage per site **kecil** (0.5-2 km)

**Frekuensi:** L1800 atau L2300
**Site spacing:** 300-800 meter

#### 2. Suburban (Pinggir Kota) 🏘️

**Karakteristik:**

- Sedang padat
- Campuran gedung & rumah
- Traffic **sedang**
- Coverage per site **sedang** (2-5 km)

**Frekuensi:** L1800
**Site spacing:** 800-2000 meter

**Catatan:** Suburban dengan industri = Traffic tinggi!

#### 3. Rural (Pedesaan) 🌾

**Karakteristik:**

- Tidak padat
- Area terbuka
- Traffic **rendah**
- Coverage per site **luas** (5-15 km)

**Frekuensi:** L900 atau L800
**Site spacing:** 2-10 km

---

## 13. Link Budget - Konsep Dasar

### Apa itu Link Budget?

**Link Budget** = Perhitungan **semua penguatan dan rugi-rugi** dari transmitter sampai receiver!

### Formula Dasar:

```
Received Power = Transmit Power + Gains - Losses
```

**Atau lebih detail:**

```
Pr = Pt + Gt + Gr - Path Loss - Other Losses
```

Dimana:

- **Pr** = Received power (dBm)
- **Pt** = Transmit power (dBm)
- **Gt** = Transmit antenna gain (dBi)
- **Gr** = Receive antenna gain (dBi)
- **Path Loss** = Free space loss + obstacles
- **Other Losses** = Cable loss, connector loss, body loss, dll

### Maximum Allowable Path Loss (MAPL):

```
MAPL = Pt + Gt + Gr - Pr(min) - Fade Margin - Other Losses
```

**MAPL** digunakan untuk menghitung **cell radius** (jangkauan)!

---

## 14. Link Budget Components

### 3 Komponen Utama:

#### 1. System Gain

**Penguatan dari sistem:**

**Downlink:**

- eNodeB transmit power
- eNodeB antenna gain
- UE antenna gain
- UE receiver sensitivity

**Uplink:**

- UE transmit power
- UE antenna gain
- eNodeB antenna gain
- eNodeB receiver sensitivity

#### 2. System Margin

**Rugi-rugi dari sistem:**

- **Fading margin** (slow fading)
- **Interference margin** (dari cell tetangga)
- **Building penetration loss** (masuk gedung)
- **Body loss** (dekat badan manusia)
- **Cable loss** (dari BBU ke antenna)
- **Connector loss** (sambungan kabel)

#### 3. Radio Shell

**Parameter propagasi:**

- **Propagation model** (Okumura-Hata, COST 231, Nui)
- **Frequency** (900/1800/2100/2300 MHz)
- **Antenna height** (tinggi tower)
- **Distance** (jarak site ke user)

---

## 15. Downlink vs Uplink Budget

### Downlink (eNodeB → UE):

```
[eNodeB] ---> Signal ---> [User Equipment]
```

**Power Budget:**

```
Pr(UE) = Pt(eNB) + Gt(eNB) + Gr(UE) - Path Loss - Losses
```

**Parameter:**

- **Pt(eNB):** 43-48 dBm (tergantung kelas)
- **Gt(eNB):** 15-18 dBi (antenna gain)
- **Gr(UE):** 0 dBi (handphone antenna)

### Uplink (UE → eNodeB):

```
[User Equipment] ---> Signal ---> [eNodeB]
```

**Power Budget:**

```
Pr(eNB) = Pt(UE) + Gt(UE) + Gr(eNB) - Path Loss - Losses
```

**Parameter:**

- **Pt(UE):** 23 dBm (max power HP)
- **Gt(UE):** 0 dBi
- **Gr(eNB):** 15-18 dBi

### Mana yang Limiting?

**Biasanya UPLINK!**

Kenapa?

- Power HP cuma **23 dBm**
- Power eNodeB bisa **43-48 dBm**
- **Uplink lebih lemah** → Jadi pembatas jangkauan!

---

## 16. eNodeB Power Class

### 3 Kelas eNodeB:

| Class          | Power     | Antenna          | Use Case               |
| -------------- | --------- | ---------------- | ---------------------- |
| **Wide Area**  | 43-48 dBm | External (besar) | **Outdoor macro cell** |
| **Local Area** | 38-43 dBm | External (kecil) | Small cell, pico cell  |
| **Home**       | 24-30 dBm | Internal         | Femto cell, indoor     |

**Paling umum:** Wide Area (macro cell outdoor)

**Open RAN:** Butuh power lebih besar (sampai 51 dBm!)

---

## 17. Antenna Gain LTE

### Antenna Gain berdasarkan Frekuensi:

| Frekuensi    | Antenna Gain |
| ------------ | ------------ |
| **800 MHz**  | 15-17 dBi    |
| **900 MHz**  | 15-17 dBi    |
| **1800 MHz** | 17-18 dBi    |
| **2100 MHz** | 17-18 dBi    |
| **2300 MHz** | 18-21 dBi    |
| **2600 MHz** | 18-21 dBi    |

**Semakin tinggi frekuensi → Antenna gain semakin besar!**

---

## 18. UE Category (Device Class)

### LTE Category:

| Category  | Max DL Speed | Max UL Speed | Bandwidth |
| --------- | ------------ | ------------ | --------- |
| **Cat 1** | 10 Mbps      | 5 Mbps       | 20 MHz    |
| **Cat 2** | 50 Mbps      | 25 Mbps      | 20 MHz    |
| **Cat 3** | 100 Mbps     | 50 Mbps      | 20 MHz    |
| **Cat 4** | 150 Mbps     | 50 Mbps      | 20 MHz    |
| **Cat 5** | 300 Mbps     | 75 Mbps      | 20 MHz    |

**Cat 3** = Paling umum untuk smartphone entry-level
**Cat 4-5** = Smartphone flagship

### 5G Category:

**eMBB** (enhanced Mobile Broadband)

- Speed **sangat tinggi** (sampai 10 Gbps!)
- Untuk video 4K, AR/VR

**mMTC** (massive Machine Type Communication)

- **Banyak device** (sampai 1 juta per km²!)
- Untuk IoT, sensor

**URLLC** (Ultra-Reliable Low-Latency Communication)

- **Latency rendah** (< 1 ms!)
- Untuk autonomous vehicle, industrial automation

---

## 19. Thermal Noise

### Apa itu Thermal Noise?

**Thermal Noise** = Noise yang berasal dari **alam** (suhu lingkungan)

**Tidak bisa dihilangkan!**

### Formula Thermal Noise:

```
Thermal Noise (dBm) = -174 + 10 × log10(Bandwidth)
```

**Konstanta:** -174 dBm/Hz (suhu 290 K atau 17°C)

### Contoh Perhitungan:

**Bandwidth 5 MHz:**

```
Thermal Noise = -174 + 10 × log10(5,000,000)
             = -174 + 10 × 6.7
             = -174 + 67
             = -107 dBm
```

**Bandwidth 20 MHz:**

```
Thermal Noise = -174 + 10 × log10(20,000,000)
             = -174 + 10 × 7.3
             = -174 + 73
             = -101 dBm
```

**Semakin lebar bandwidth → Thermal noise semakin tinggi!**

---

## 20. Bandwidth vs Resource Block

### Hubungan Bandwidth dengan RB:

| Bandwidth   | Resource Block | Subcarrier |
| ----------- | -------------- | ---------- |
| **1.4 MHz** | 6              | 72         |
| **3 MHz**   | 15             | 180        |
| **5 MHz**   | 25             | 300        |
| **10 MHz**  | 50             | 600        |
| **15 MHz**  | 75             | 900        |
| **20 MHz**  | 100            | 1200       |

### Formula:

```
1 Resource Block = 12 subcarrier = 180 kHz
```

### Kenapa 1800 MHz Paling Populer?

- ✅ Bisa dapat bandwidth **20 MHz**
- ✅ Artinya dapat **100 RB**
- ✅ Kapasitas **paling besar**!

**Kalau L900:**

- Maksimum bandwidth: 7.5 MHz
- Resource Block: Cuma 25-50 RB
- Kapasitas **lebih kecil**

---

## 21. SNR (Signal-to-Noise Ratio)

### Apa itu SNR?

**SNR** = Perbandingan antara **sinyal** dengan **noise**

**Formula:**

```
SNR (dB) = Signal Power - Noise Power
```

### SNR vs Modulasi:

| Modulasi    | SNR Minimum |
| ----------- | ----------- |
| **QPSK**    | -1 dB       |
| **16-QAM**  | 9 dB        |
| **64-QAM**  | 16 dB       |
| **256-QAM** | 22 dB       |

**Semakin tinggi SNR → Bisa pakai modulasi lebih tinggi → Speed lebih cepat!**

---

## 22. Adaptive Modulation & Coding (AMC)

### Kenapa Adaptive?

**Jarak ke site berbeda → Kualitas sinyal berbeda → Modulasi berbeda!**

### 3 Jenis Modulasi LTE:

#### 1. QPSK (Quadrature Phase Shift Keying)

- **2 bit per symbol**
- Untuk **jarak jauh** (edge of cell)
- **Paling lambat** tapi **paling robust**

#### 2. 16-QAM (Quadrature Amplitude Modulation)

- **4 bit per symbol**
- Untuk **jarak sedang**
- **Sedang speed & robustness**

#### 3. 64-QAM

- **6 bit per symbol**
- Untuk **jarak dekat** (near cell)
- **Paling cepat** tapi **butuh SNR tinggi**

#### 4. 256-QAM (4x4 MIMO)

- **8 bit per symbol**
- Untuk **sangat dekat**
- **LTE-Advanced feature**

### Aturan Jarak:

```
Dekat Site = 64-QAM (cepat)
   ↓
Sedang = 16-QAM (sedang)
   ↓
Jauh Site = QPSK (lambat tapi stabil)
```

### Kenapa Link Budget Pakai QPSK?

**QPSK = Worst case!**

Kalau QPSK sudah bagus → 16-QAM & 64-QAM pasti bagus!

**QPSK = Jarak terjauh = Coverage maksimal**

---

## 23. MAPL (Maximum Allowable Path Loss)

### Formula MAPL:

**Downlink:**

```
MAPL(DL) = Pt(eNB) + Gt(eNB) + Gr(UE) - Pr(UE,min) - Fade Margin - Other Losses
```

**Uplink:**

```
MAPL(UL) = Pt(UE) + Gt(UE) + Gr(eNB) - Pr(eNB,min) - Fade Margin - Other Losses
```

### Contoh Perhitungan MAPL:

**Given:**

- Pt(eNB) = 46 dBm
- Gt(eNB) = 18 dBi
- Gr(UE) = 0 dBi
- Pr(UE,min) = -110 dBm
- Fade Margin = 10 dB
- Cable Loss = 2 dB
- Connector Loss = 0.5 dB
- Body Loss = 3 dB

**Hitung:**

```
MAPL = 46 + 18 + 0 - (-110) - 10 - 2 - 0.5 - 3
MAPL = 46 + 18 + 110 - 10 - 2 - 0.5 - 3
MAPL = 158.5 dB
```

**Dari MAPL ini nanti kita hitung cell radius!**

---

## 24. Propagation Model

### 3 Model Propagasi untuk LTE:

#### 1. Okumura-Hata

**Range frekuensi:** 150-1500 MHz

**Cocok untuk:**

- L800 (800 MHz)
- L900 (900 MHz)

**Formula:**

```
Path Loss = 69.55 + 26.16×log10(f) - 13.82×log10(hb) + (44.9 - 6.55×log10(hb))×log10(d) - a(hm)
```

Dimana:

- **f** = Frekuensi (MHz)
- **hb** = Tinggi antenna eNodeB (m)
- **hm** = Tinggi antenna UE (m)
- **d** = Jarak (km)

#### 2. COST 231-Hata

**Range frekuensi:** 1500-2000 MHz ⭐

**Cocok untuk:**

- **L1800** (1800 MHz) → Paling sering dipakai!
- **L2100** (2100 MHz)

**Formula:**

```
Path Loss = 46.3 + 33.9×log10(f) - 13.82×log10(hb) + (44.9 - 6.55×log10(hb))×log10(d) - a(hm) + Cm
```

**Cm:**

- 0 dB (suburban & rural)
- 3 dB (urban)

#### 3. Nui Model

**Range frekuensi:** 2000-6000 MHz

**Cocok untuk:**

- **L2300** (2300 MHz)
- **L2600** (2600 MHz)
- **5G** (3500 MHz)

**Formula lebih kompleks, biasanya ada di tools!**

### Pemilihan Model:

```
Frekuensi < 1500 MHz → Okumura-Hata
1500-2000 MHz → COST 231-Hata ⭐ (paling umum!)
> 2000 MHz → Nui
```

---

## 25. Link Budget Excel Template

### Struktur Excel Link Budget:

**Sheet 1: Input Parameters**

- Frekuensi
- Bandwidth
- Transmit power
- Antenna gain
- Receiver sensitivity
- Losses

**Sheet 2: Downlink Budget**

- MAPL calculation
- SNR calculation
- Cell radius

**Sheet 3: Uplink Budget**

- MAPL calculation
- SNR calculation
- Cell radius

**Sheet 4: Result**

- Coverage radius
- Site area
- Number of sites

**Warna:**

- 🟢 **Hijau** = Fixed value (dari standar)
- 🟡 **Kuning** = Input value (bisa diubah)
- 🔵 **Biru** = Formula (otomatis hitung)

---

## 26. Langkah Perhitungan Link Budget

### Step-by-Step:

#### Step 1: Tentukan Parameter Dasar

- Frekuensi: 1800 MHz
- Bandwidth: 20 MHz
- Area type: Urban

#### Step 2: Tentukan System Gain

- eNodeB Tx power: 46 dBm
- eNodeB antenna gain: 18 dBi
- UE antenna gain: 0 dBi
- UE Rx sensitivity: -110 dBm

#### Step 3: Tentukan System Margin

- Fade margin: 10 dB
- Cable loss: 2 dB
- Connector loss: 0.5 dB
- Body loss: 3 dB
- Building penetration: 15 dB (indoor), 0 dB (outdoor)

#### Step 4: Hitung MAPL

```
MAPL = System Gain - System Margin - Minimum Signal
```

#### Step 5: Pilih Propagation Model

- 1800 MHz → COST 231-Hata

#### Step 6: Hitung Cell Radius

- Input MAPL ke formula propagasi
- Solve untuk distance (d)

#### Step 7: Hitung Site Area

```
Site Area = π × R²  (untuk 1 sector circular)
Site Area = 2.6 × R²  (untuk 3 sector realistic)
```

#### Step 8: Hitung Number of Sites

```
Number of Sites = Total Area / Site Area
```

---

## 27. Contoh Perhitungan Lengkap

### Given:

- **Area:** 100 km² (Urban)
- **Frekuensi:** 1800 MHz
- **Bandwidth:** 20 MHz
- **Target:** Outdoor coverage

### Downlink Budget:

**System Gain:**

- Pt = 46 dBm
- Gt(eNB) = 18 dBi
- Gr(UE) = 0 dBi
- **Total Gain = 46 + 18 + 0 = 64 dBm**

**System Margin:**

- Receiver sensitivity = -110 dBm
- Fade margin = 10 dB
- Cable loss = 2 dB
- Connector loss = 0.5 dB
- Body loss = 3 dB
- **Total Margin = 15.5 dB**

**MAPL:**

```
MAPL = 64 - (-110) - 15.5
MAPL = 174 - 15.5
MAPL = 158.5 dB
```

### Cell Radius (COST 231):

**Asumsi:**

- hb = 30 m
- hm = 1.5 m
- Urban area (Cm = 3 dB)

**Solve untuk d:**

```
158.5 = 46.3 + 33.9×log10(1800) - 13.82×log10(30) + (44.9 - 6.55×log10(30))×log10(d) + 3
```

**Hasil (simplified):**

- **Cell Radius ≈ 0.8 km** (urban)

### Site Area:

```
Site Area = 2.6 × R²
Site Area = 2.6 × 0.8²
Site Area = 1.66 km²
```

### Number of Sites:

```
Number of Sites = 100 / 1.66
Number of Sites ≈ 60 sites
```

**Butuh 60 site untuk cover 100 km² di urban!**

---

## 28. Perbandingan Coverage per Frekuensi

### Contoh Coverage Radius:

**Kondisi sama (46 dBm, urban):**

| Frekuensi | Cell Radius |
| --------- | ----------- |
| **L900**  | ~3 km       |
| **L1800** | ~0.8 km     |
| **L2100** | ~0.6 km     |
| **L2300** | ~0.5 km     |

**Kesimpulan:**

- **Frekuensi rendah** = Coverage luas, tapi bandwidth kecil
- **Frekuensi tinggi** = Coverage sempit, tapi bandwidth besar

### Trade-off Coverage vs Capacity:

**Rural:**

- Butuh **coverage** luas
- Pakai **L900** atau **L800**
- Bandwidth kecil → OK, traffic rendah

**Urban:**

- Butuh **capacity** besar
- Pakai **L1800** atau **L2300**
- Coverage kecil → OK, site banyak

---

## 29. Tips Link Budget Planning

### Do's ✅

1. **Pakai QPSK untuk worst case**

   - Kalau QPSK OK → Yang lain pasti OK!

2. **Uplink biasanya limiting**

   - Cek uplink budget lebih teliti

3. **Fade margin realistis**

   - Urban: 10-12 dB
   - Suburban: 8-10 dB
   - Rural: 6-8 dB

4. **Komparasi dengan tools**

   - Link budget manual vs Atoll
   - Harus match!

5. **Pertimbangkan building penetration**
   - Indoor: +15 dB loss
   - In-car: +8 dB loss

### Don'ts ❌

1. **Jangan abaikan uplink**

   - Banyak yang cuma hitung downlink

2. **Jangan lupa cable loss**

   - Bisa 2-3 dB per 100m

3. **Jangan pakai antenna gain terlalu tinggi**

   - Realistis: 15-18 dBi

4. **Jangan lupa interference margin**

   - Ada site tetangga = ada interference

5. **Jangan skip validation**
   - Harus drive test untuk validasi!

---

## 30. Tools untuk Link Budget

### Software:

1. **Microsoft Excel** ⭐

   - Paling fleksibel
   - Bisa customize formula
   - Template bisa di-share

2. **Atoll (Forsk)**

   - Link budget terintegrasi
   - Auto calculate dari parameter

3. **ICS Telecom**

   - Advanced propagation model
   - 3D terrain analysis

4. **Planet (Mentum)**
   - Comprehensive planning
   - Multi-vendor support

### Yang Wajib Dikuasai:

- ✅ **Excel** → Untuk perhitungan manual
- ✅ **Atoll** → Untuk coverage simulation

**Atoll akan dipraktikkan minggu depan!** 🚀

---

## 31. Link Budget vs Atoll

### Link Budget Excel:

**Kelebihan:**

- **Manual control** penuh
- **Understand** setiap parameter
- **Fleksibel** untuk modifikasi
- **Cepat** untuk estimasi awal

**Kekurangan:**

- Tidak visual (no map)
- Tidak real terrain
- Propagasi simplified

### Atoll:

**Kelebihan:**

- **Visual map** coverage
- **Real terrain** data
- **Propagasi akurat** (3D)
- **Comprehensive** (coverage + capacity)

**Kekurangan:**

- Kurang kontrol manual
- Butuh learning curve
- License mahal

### Best Practice:

```
1. Link Budget Excel (rough estimation)
2. Atoll Simulation (detailed planning)
3. Drive Test (validation)
4. Optimization (tuning)
```

**Link budget manual wajib dikuasai sebelum pakai tools!**

---

## 32. Kesimpulan

### Link Budget Itu:

- ✅ **Perhitungan** gain & loss dari transmitter ke receiver
- ✅ **Menentukan** MAPL (Maximum Allowable Path Loss)
- ✅ **Menghitung** cell radius & site area
- ✅ **Estimasi** jumlah site yang dibutuhkan
- ✅ **Foundation** untuk coverage planning

### Formula Kunci:

```
MAPL = Pt + Gt + Gr - Pr(min) - Fade Margin - Losses

Cell Radius = f(MAPL, Propagation Model)

Site Area = 2.6 × R²

Number of Sites = Total Area / Site Area
```

### Workflow:

```
Network Objective → Network Planning (Link Budget) → Project Execution → Network Optimization
```

### Yang Harus Dikuasai:

1. **Konsep** coverage vs capacity
2. **Formula** link budget (downlink & uplink)
3. **Propagation model** (Okumura-Hata, COST 231, Nui)
4. **Excel calculation** (manual)
5. **Atoll** (tools)

---

## 33. Next Step

### Minggu Depan:

**Praktik Atoll!** 🎯

**Yang akan dipelajari:**

1. Input parameter di Atoll
2. Coverage prediction
3. Best server analysis
4. Interference analysis
5. Export report & map

**Persiapan:**

- Install Atoll (sudah disediakan)
- Review materi link budget ini
- Siapkan pertanyaan

---

## 34. Latihan Soal

### Soal 1: MAPL Calculation

**Diketahui:**

- Pt(eNB) = 46 dBm
- Gt(eNB) = 18 dBi
- Gr(UE) = 0 dBi
- Pr(UE,min) = -105 dBm
- Fade Margin = 8 dB
- Cable Loss = 2 dB
- Body Loss = 3 dB

**Hitung MAPL Downlink!**

**Jawaban:**

```
MAPL = 46 + 18 + 0 - (-105) - 8 - 2 - 3
MAPL = 46 + 18 + 105 - 8 - 2 - 3
MAPL = 156 dB
```

---

### Soal 2: Thermal Noise

**Diketahui:**

- Bandwidth = 10 MHz

**Hitung Thermal Noise!**

**Jawaban:**

```
Thermal Noise = -174 + 10 × log10(10,000,000)
              = -174 + 10 × 7
              = -174 + 70
              = -104 dBm
```

---

### Soal 3: Number of Sites

**Diketahui:**

- Total area = 200 km² (suburban)
- Cell radius = 2 km
- 3 sector per site

**Hitung jumlah site!**

**Jawaban:**

**Site Area:**

```
= 2.6 × R²
= 2.6 × 2²
= 2.6 × 4
= 10.4 km²
```

**Number of Sites:**

```
= 200 / 10.4
= 19.2
≈ 20 sites
```

---

## 35. Motivasi Akhir

**Link Budget = Foundation of RF Planning!** 📡

**Tanpa Link Budget yang baik:**

- ❌ Coverage salah estimasi
- ❌ Site terlalu banyak/sedikit
- ❌ Budget planning meleset
- ❌ Performance buruk

**Dengan Link Budget yang baik:**

- ✅ Coverage terencana baik
- ✅ Jumlah site optimal
- ✅ Budget akurat
- ✅ ROI (Return on Investment) bagus

**Pro Tips:**

1. **Manual calculation is a must!**
   - Tools boleh, tapi konsep harus paham
2. **Always check uplink budget**
   - Uplink usually the bottleneck
3. **Validate with drive test**
   - Theory vs reality beda!
4. **Compare manual vs tools**
   - Harus match!
5. **Document everything**
   - Parameter, assumption, result

**Link Budget = Art + Science!** 🎨🔬

---

**Good luck untuk belajar Link Budget!** 🚀

**Semoga catatan ini bermanfaat untuk kuliah, magang, dan karir!** 💙

---

## 36. Resources Tambahan

### Standards:

- **3GPP TS 36.104** (Base Station Radio Transmission/Reception)
- **3GPP TS 36.101** (UE Radio Transmission/Reception)
- **3GPP TS 36.942** (Radio Frequency System Scenarios)

### Books:

- "LTE Radio Planning and Optimization" - Morten Tolstrup
- "Fundamentals of LTE" - Arunabha Ghosh
- "LTE for UMTS" - Harri Holma

### Online Resources:

- ShareTechnote (link budget calculator)
- 3GPP.org (official standards)
- RF Wireless World (tutorials)

### Tools Download:

- **Atoll** (trial version available)
- **Excel templates** (dari dosen)
- **TEMS Discovery** (untuk drive test)

---

**Catatan penting:**

- Materi ini **berbasis kuliah real + standar 3GPP**
- Perhitungan **harus dikuasai manual** sebelum pakai tools
- **Link budget** adalah skill wajib RF Engineer
- **Jangan skip teori** - ini yang membedakan Engineer sejati!

**Practice makes perfect!** 🔥📚
