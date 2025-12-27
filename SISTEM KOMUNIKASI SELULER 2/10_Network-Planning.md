# 10. Design dan Perhitungan LTE - Part 1

## 1. Pengantar LTE Design

Desain jaringan LTE adalah proses **merencanakan dan menghitung** kebutuhan infrastruktur jaringan 4G untuk suatu wilayah tertentu. Bukan cuma pasang BTS sembarangan, tapi ada perhitungan detail!

### Tujuan Pembelajaran:

- Mampu menghitung kebutuhan site/eNodeB untuk area tertentu
- Memahami capacity planning dan coverage planning
- Bisa mendesain jaringan LTE dari nol
- Siap untuk technical interview di perusahaan telco

---

## 2. Metode Perhitungan LTE

Ada **2 teknik** untuk mendesain LTE:

### A. Perhitungan Manual

**Menggunakan**: Rumus matematis dari white paper vendor (Huawei, ZTE, dll)

**Kelebihan**:

- Paham konsep mendalam
- Tahu "kenapa" di balik angka
- Tidak tergantung tools

**Kegunaan**: Untuk memahami dasar teori

### B. Perhitungan dengan Software

**Menggunakan**:

- **Excel/Spreadsheet** - untuk capacity calculation
- **Atoll** - untuk coverage simulation (RF planning)

**Kelebihan**:

- Cepat dan praktis
- Visualisasi bagus (peta coverage)
- Langsung dapat hasil

**Kegunaan**: Untuk implementasi real di industri

**Catatan**: Kita belajar manual dulu, baru pakai software. Kalau langsung software tanpa paham konsep = bahaya!

---

## 3. Dua Aspek Utama dalam LTE Design

### A. RAN Design (Radio Access Network)

**Fokus**: Jaringan akses radio (UE ↔ eNodeB)

**2 Komponen**:

#### 1. Coverage Planning

**Tujuan**: Memastikan sinyal radio mencapai seluruh area target

**Parameter yang dihitung**:

- **RSRP** (Reference Signal Received Power) - kekuatan sinyal
- **RSRQ** (Reference Signal Received Quality) - kualitas sinyal
- **SINR** (Signal to Interference plus Noise Ratio) - rasio sinyal vs noise
- **Radius cell** - jangkauan satu site
- **Jumlah site** - berapa BTS yang dibutuhkan

**Tools**: Atoll (simulasi propagasi radio)

**Output**: Peta coverage dengan warna:

- 🔵 Biru = Excellent (SINR > 20 dB)
- 🟢 Hijau = Good (SINR 10-20 dB)
- 🟡 Kuning = Fair (SINR 0-10 dB)
- 🔴 Merah = Poor (SINR < 0 dB)

#### 2. Capacity Planning

**Tujuan**: Memastikan jaringan bisa menampung traffic user

**Parameter yang dihitung**:

- **Throughput per cell** - kecepatan data per site
- **PRB (Physical Resource Block)** - blok resource radio
- **Jumlah user per cell** - berapa user bisa dilayani
- **Bandwidth** - lebar frekuensi yang dipakai
- **Kebutuhan backhaul** - kapasitas link ke core

**Output**: Jumlah site yang dibutuhkan berdasarkan beban traffic

### B. Core Network Design

**Fokus**: Jaringan inti (MME, SGW, PGW)

**Yang dihitung**:

- Berapa MME yang dibutuhkan untuk handle signaling
- Berapa SGW untuk handle data traffic
- Kapasitas interface (S1-U, S1-MME, X2, S5/S8)
- Throughput total ke internet

### C. Transport Network Design

**Fokus**: Jalur penghubung RAN ↔ Core

**Teknologi**:

- **Metro Ethernet** (modern) - menggunakan MPLS/IP
- **SDH** (lama) - Synchronous Digital Hierarchy

**Yang dihitung**:

- Bandwidth backhaul per site
- Aggregation network capacity
- Redundancy dan backup link

---

## 4. Arsitektur LTE (Review Cepat)

### Komponen EPS (Evolved Packet System)

```
[UE] ←→ [eNodeB] ←→ [SGW] ←→ [PGW] ←→ Internet
           ↓
         [MME]
```

#### E-UTRAN (Radio Access)

- **eNodeB**: Base station 4G

#### EPC (Evolved Packet Core)

- **MME**: Mobility Management Entity (kontrol signaling)
- **SGW**: Serving Gateway (anchor untuk mobilitas)
- **PGW**: PDN Gateway (gerbang ke internet)
- **HSS**: Home Subscriber Server (database pelanggan)
- **PCRF**: Policy Control (billing & QoS)

### Interface Penting:

- **LTE-Uu**: UE ↔ eNodeB (wireless)
- **X2**: eNodeB ↔ eNodeB (handover)
- **S1-U**: eNodeB ↔ SGW (data)
- **S1-MME**: eNodeB ↔ MME (signaling)
- **S5/S8**: SGW ↔ PGW (data)
- **S11**: MME ↔ SGW (kontrol)

**Perbedaan dengan 3G**:

- 3G: NodeB → RNC → Core (butuh RNC)
- 4G: eNodeB → langsung ke Core (RNC dihapus, fungsinya masuk ke eNodeB)

---

## 5. Protocol Stack LTE (Review)

### 6 Layer Protocol:

```
Layer 6: NAS (Non-Access Stratum)
Layer 5: RRC (Radio Resource Control) / IP
Layer 4: PDCP (Packet Data Convergence Protocol)
Layer 3: RLC (Radio Link Control)
Layer 2: MAC (Medium Access Control)
Layer 1: Physical Layer
```

### Pengelompokan:

1. **Physical Layer** (L1)
2. **Data Link Layer** (L2-L4): MAC, RLC, PDCP
3. **Network/Radio Layer** (L5-L6): RRC, NAS

### 2 Plane:

- **Control Plane**: Untuk signaling (setup koneksi, handover)
- **User Plane**: Untuk data (browsing, streaming, download)

---

## 6. RF Planning Fundamentals

### A. Frekuensi LTE di Indonesia

| Frekuensi | Bandwidth | Coverage        | Capacity                | Teknologi |
| --------- | --------- | --------------- | ----------------------- | --------- |
| 900 MHz   | 5-7.5 MHz | ⭐⭐⭐⭐⭐ Luas | ⭐⭐ Kecil              | FDD       |
| 1800 MHz  | 20 MHz    | ⭐⭐⭐⭐ Sedang | ⭐⭐⭐⭐⭐ Besar        | FDD       |
| 2100 MHz  | 15 MHz    | ⭐⭐⭐ Sedang   | ⭐⭐⭐⭐ Besar          | FDD       |
| 2300 MHz  | 30 MHz    | ⭐⭐ Sempit     | ⭐⭐⭐⭐⭐ Sangat Besar | TDD       |

**Prinsip Dasar**:

- **Frekuensi rendah** → coverage luas, capacity kecil
- **Frekuensi tinggi** → coverage sempit, capacity besar

**Contoh Kasus**:

- **Daerah 3T** (Terdepan, Terluar, Tertinggal): Pakai 900 MHz
  - Populasi jarang, area luas
  - Butuh coverage maksimal
  - Traffic tidak terlalu tinggi
- **Kota besar**: Pakai 1800 atau 2300 MHz
  - Populasi padat, area sempit
  - Butuh capacity besar
  - Traffic sangat tinggi

### B. FDD vs TDD

#### FDD (Frequency Division Duplex)

- Uplink dan downlink pakai frekuensi **berbeda**
- Dipakai di: 900, 1800, 2100 MHz
- Lebih cocok untuk voice dan data seimbang

#### TDD (Time Division Duplex)

- Uplink dan downlink pakai frekuensi **sama**, waktu berbeda
- Dipakai di: 2300 MHz keatas
- Lebih cocok untuk data (downlink > uplink)

### C. Bandwidth vs PRB

| Bandwidth | PRB | Subcarrier | Max Throughput (QPSK) | Max Throughput (64QAM) |
| --------- | --- | ---------- | --------------------- | ---------------------- |
| 1.4 MHz   | 6   | 72         | ~1 Mbps               | ~4 Mbps                |
| 3 MHz     | 15  | 180        | ~2.5 Mbps             | ~10 Mbps               |
| 5 MHz     | 25  | 300        | ~4 Mbps               | ~17 Mbps               |
| 10 MHz    | 50  | 600        | ~8 Mbps               | ~34 Mbps               |
| 15 MHz    | 75  | 900        | ~12 Mbps              | ~50 Mbps               |
| 20 MHz    | 100 | 1200       | ~16 Mbps              | ~75 Mbps               |

**Catatan**:

- 1 PRB = 12 subcarrier = 180 kHz
- Throughput real lebih rendah karena overhead signaling

---

## 7. Modulasi dan MIMO

### A. Modulasi LTE

LTE mendukung **4 jenis modulasi**:

1. **QPSK** (Quadrature Phase Shift Keying)

   - 1 simbol = 2 bit
   - Paling "jelek" tapi paling **reliable**
   - Coverage paling luas
   - **Dipakai sebagai default** untuk planning

2. **16-QAM**

   - 1 simbol = 4 bit
   - Kecepatan 2x QPSK
   - Coverage sedang

3. **64-QAM**

   - 1 simbol = 6 bit
   - Kecepatan 3x QPSK
   - Coverage sempit (dekat BTS)

4. **256-QAM**
   - 1 simbol = 8 bit
   - Kecepatan 4x QPSK
   - Coverage sangat sempit (sangat dekat BTS)

**Link Adaptation**:

- Jauh dari BTS → pakai QPSK (sinyal lemah tapi reliable)
- Dekat BTS → pakai 64/256-QAM (sinyal kuat, kecepatan tinggi)

**Kenapa QPSK jadi default planning?**

- Kalau QPSK aja bagus → yang lain pasti lebih bagus!
- Jaminan coverage minimum

### B. MIMO (Multiple Input Multiple Output)

**Konsep**: Banyak antena pemancar dan penerima → kapasitas berlipat

**Konfigurasi umum**:

- **2x2 MIMO**: 2 antena TX, 2 antena RX → throughput x2
- **4x4 MIMO**: 4 antena TX, 4 antena RX → throughput x4
- **8x8 MIMO**: 8 antena TX, 8 antena RX → throughput x8 (5G)

**Keuntungan**:

- **Spatial Multiplexing**: Banyak user bisa dilayani bersamaan
- **Diversity**: Lebih tahan terhadap fading
- **Beamforming**: Sinyal diarahkan ke user tertentu

**Contoh**:

- 20 MHz (100 PRB) dengan QPSK → 16 Mbps
- Pakai 2x2 MIMO → 16 x 2 = **32 Mbps**
- Pakai 4x4 MIMO → 16 x 4 = **64 Mbps**

---

## 8. Scheduling & Resource Allocation

### Teknik Scheduling di LTE: **OFDMA**

**OFDMA (Orthogonal Frequency Division Multiple Access)**:

- Resource radio (PRB) dibagi ke banyak user
- Tiap user dapat time slot dan frequency slot berbeda
- Sangat fleksibel!

**Keuntungan**:

- Banyak user bisa dilayani bersamaan
- Efisiensi spektrum tinggi
- Mendukung berbagai modulasi (adaptive)

**Proses**:

1. **Scheduling**: eNodeB tentukan user mana dapat PRB mana
2. **HARQ**: Error correction otomatis (hybrid ARQ)
3. **Priority Handling**: User VIP dapat prioritas lebih tinggi

---

## 9. Link Budget

**Link Budget** = perhitungan daya dari transmitter sampai receiver

### Parameter Link Budget:

#### Sisi Transmitter (TX):

- **TX Power**: Daya pancar antena
- **TX Antenna Gain**: Penguatan antena pemancar
- **TX Cable Loss**: Rugi-rugi kabel feeder

#### Propagasi:

- **Path Loss**: Rugi-rugi jalur udara (paling besar!)
- **Slow Fading Margin**: Cadangan untuk fading lambat (10-15 dB)
- **Fast Fading Margin**: Cadangan untuk fading cepat (5-10 dB)
- **Interference Margin**: Cadangan untuk interferensi (3-5 dB)

#### Sisi Receiver (RX):

- **RX Antenna Gain**: Penguatan antena penerima
- **RX Cable Loss**: Rugi-rugi kabel
- **Receiver Sensitivity**: Sensitivitas minimum receiver

### Rumus Dasar:

```
EIRP = TX Power + TX Gain - TX Loss
Received Power = EIRP - Path Loss + RX Gain - RX Loss
```

**Path Loss** dihitung dengan model propagasi:

- **Free Space** (teori, tanpa obstacle)
- **Okumura-Hata** (urban, suburban, rural)
- **COST-231 Hata** (extended untuk 1.5-2 GHz)
- **Stanford University Interim (SUI)** (untuk area flat)

### Contoh:

```
TX Power = 46 dBm (40W)
TX Gain = 18 dBi
TX Loss = 2 dB
Path Loss = 130 dB
RX Gain = 0 dBi (handset)
RX Loss = 0 dB

Received Power = 46 + 18 - 2 - 130 + 0 - 0
               = -68 dBm (masih bagus!)
```

---

## 10. Coverage Dimensioning

**Tujuan**: Menghitung **jumlah site** berdasarkan coverage area

### Input:

1. **Area yang akan di-cover** (km²)
2. **Frekuensi** (900, 1800, 2100, 2300 MHz)
3. **Tipe area** (Urban, Suburban, Rural)
4. **Target coverage** (90%, 95%, 98%)

### Proses:

#### Step 1: Hitung Maximum Path Loss (MAPL)

```
MAPL = EIRP - Receiver Sensitivity + Antenna Gain - Margin
```

#### Step 2: Hitung Cell Radius

Gunakan model propagasi (misal Okumura-Hata):

```
Path Loss = 69.55 + 26.16*log(f) - 13.82*log(h_bs)
            - a(h_ms) + (44.9 - 6.55*log(h_bs))*log(d)
```

- f = frekuensi (MHz)
- h_bs = tinggi BTS (m)
- h_ms = tinggi handset (m)
- d = jarak (km)

Dari MAPL, solve untuk d → dapat **cell radius**

#### Step 3: Hitung Luas per Cell

```
Luas hexagonal = 2.6 * R²
```

R = cell radius (km)

#### Step 4: Hitung Jumlah Site

```
Jumlah Site = Total Area / Luas per Cell
```

### Contoh:

**Desain untuk area Mampang (Jakarta Selatan)**

**Input**:

- Area = 5 km²
- Frekuensi = 1800 MHz
- Tipe = Urban
- Target coverage = 95%
- Bandwidth = 20 MHz

**Perhitungan**:

1. MAPL = 135 dB (dari link budget)
2. Cell radius = 0.8 km (dari model propagasi)
3. Luas per cell = 2.6 × 0.8² = 1.66 km²
4. Jumlah site = 5 / 1.66 = **3 site**

**Kesimpulan**: Butuh 3 site untuk cover Mampang dengan coverage 95%

---

## 11. Capacity Dimensioning

**Tujuan**: Menghitung **jumlah site** berdasarkan traffic demand

### Input:

1. **Jumlah subscriber** (orang)
2. **Traffic per user** (Mbps)
3. **Busy hour penetration** (%)
4. **Overbooking ratio** (biasanya 1:10 atau 1:20)

### Proses:

#### Step 1: Hitung Total Traffic

```
Total Traffic = Jumlah User × Traffic per User × BH Penetration
```

**Busy Hour Penetration**: Persentase user yang aktif di jam sibuk

- Residential area: 10-15%
- Business area: 20-30%
- Hotspot: 40-60%

#### Step 2: Hitung Throughput per Cell

```
Throughput per Cell = Bandwidth × Efficiency × MIMO
```

**Efficiency**:

- QPSK: ~0.4
- 16-QAM: ~0.6
- 64-QAM: ~0.7

**Contoh**:

- 20 MHz dengan 64-QAM dan 2x2 MIMO
- Throughput = 20 × 0.7 × 2 = **28 Mbps** (downlink)

#### Step 3: Hitung Jumlah Site

```
Jumlah Site = Total Traffic / (Throughput per Cell × 3)
```

× 3 karena 1 site = 3 sektor

### Contoh:

**Desain untuk area Pondok Indah**

**Input**:

- Subscriber = 10,000 orang
- Traffic per user = 2 Mbps (rata-rata)
- BH Penetration = 20% (residential premium)
- Bandwidth = 20 MHz
- Modulasi = 64-QAM
- MIMO = 2x2

**Perhitungan**:

1. Total traffic = 10,000 × 2 × 0.2 = **4,000 Mbps**
2. Throughput per cell = 20 × 0.7 × 2 = 28 Mbps
3. Throughput per site = 28 × 3 = 84 Mbps
4. Jumlah site = 4,000 / 84 = **48 site**

**Kesimpulan**: Butuh 48 site untuk serve 10,000 subscriber premium

### Perbandingan:

- **Coverage**: 3 site (dari coverage dimensioning)
- **Capacity**: 48 site (dari capacity dimensioning)
- **Final**: Ambil yang **lebih besar** = **48 site**

**Artinya**: Area Pondok Indah ini **capacity-limited**, bukan coverage-limited!

---

## 12. Service Requirements & QoS

### Service Level Agreement (SLA)

Operator punya target kualitas layanan:

**KPI Coverage**:

- **RSRP > -110 dBm**: Coverage area 95%
- **SINR > 0 dB**: Good quality 90%

**KPI Capacity**:

- **Throughput minimum**: 2 Mbps per user
- **Latency**: < 50 ms
- **Packet loss**: < 1%

**Catatan**: Tiap vendor (Huawei, ZTE, Ericsson) punya standar berbeda!

### Traffic Model

Operator harus tahu **profil user**:

1. **Heavy User** (5-10%):

   - Konsumsi 10-50 GB/bulan
   - Aktif 24/7 (streaming, gaming)
   - Butuh: 5 Mbps guaranteed

2. **Regular User** (60-70%):

   - Konsumsi 2-10 GB/bulan
   - Aktif di jam-jam tertentu
   - Butuh: 2 Mbps best-effort

3. **Light User** (20-30%):
   - Konsumsi < 2 GB/bulan
   - Jarang online (WiFi user)
   - Butuh: 1 Mbps best-effort

**Strategi**:

- Heavy user → GBR (Guaranteed Bit Rate)
- Regular/Light → Non-GBR (Best effort)

---

## 13. Forecasting & Growth Planning

Network planning harus **future-proof** (5 tahun ke depan)

### Faktor Pertumbuhan:

1. **Subscriber Growth**:

   - Urban: 10-15% per tahun
   - Suburban: 15-20% per tahun
   - Rural: 20-30% per tahun

2. **Traffic Growth**:

   - 30-50% per tahun (video streaming dominan)
   - 5G adoption → migrasi bertahap

3. **Technology Evolution**:
   - 4G → 4.5G (Carrier Aggregation)
   - 4.5G → 5G NSA
   - 5G NSA → 5G SA

**Contoh Forecasting**:

**Tahun 1 (2024)**:

- Subscriber: 10,000
- Site: 48

**Tahun 3 (2026)**:

- Subscriber: 10,000 × (1.15)² = 13,225
- Traffic growth: 1.4x
- Site: 48 × 1.3225 × 1.4 = **87 site**

**Kesimpulan**: Perlu nambah 39 site dalam 2 tahun!

---

## 14. Open RAN vs Traditional RAN

### Traditional RAN:

- BBU (BaseBand Unit) + RRU (Remote Radio Unit) dari **vendor sama**
- Proprietary interface
- Vendor lock-in (susah ganti vendor)
- **Power consumption tinggi**

### Open RAN:

- BBU → split jadi DU (Distributed Unit) + CU (Central Unit)
- **Open interface** (siapa saja bisa bikin)
- Multi-vendor (DU dari vendor A, CU dari vendor B)
- **Power consumption lebih efisien** (20-30% lebih rendah)

**Perbedaan Utama** (untuk planning):

- **Power control** lebih baik di Open RAN
- Link budget sedikit berbeda
- Tapi secara coverage & capacity → hampir sama!

---

## 15. Flowchart LTE Planning

### Proses Lengkap:

```
START
  ↓
[1. Requirement Gathering]
  - Area target
  - Subscriber forecast
  - Traffic model
  - Budget
  ↓
[2. Site Survey]
  - Cek lokasi kandidat site
  - Cek obstacle
  - Cek akses jalan
  ↓
[3. Coverage Dimensioning]
  - Link budget calculation
  - Cell radius calculation
  - Jumlah site (coverage)
  ↓
[4. Capacity Dimensioning]
  - Traffic calculation
  - Throughput per cell
  - Jumlah site (capacity)
  ↓
[5. Final Site Count]
  Ambil MAX(Coverage, Capacity)
  ↓
[6. Site Placement]
  - Gunakan Atoll untuk simulasi
  - Optimasi posisi site
  - Cek interference
  ↓
[7. Verification]
  - Coverage > 95%?
  - Capacity mencukupi?
  - SINR > target?
  ↓
[8. Detail Planning]
  - Parameter eNodeB
  - Antenna tilt & azimuth
  - Power setting
  ↓
[9. BoQ (Bill of Quantity)]
  - Daftar equipment
  - Jumlah dan harga
  - Total cost project
  ↓
END
```

---

## 16. Tools & Software

### A. Coverage Planning:

- **Atoll** (paling populer)
- **Planet** (Ericsson)
- **ICS Telecom**
- **Google Earth** (site survey)

### B. Capacity Planning:

- **Excel/Spreadsheet** (custom calculator)
- **Matlab** (simulasi Monte Carlo)

### C. Network Optimization:

- **TEMS Investigation** (drive test)
- **Nemo Outdoor** (measurement)
- **Wireshark** (protocol analysis)

---

## 17. Pre-Planning → Detail Planning → Implementation

### A. Pre-Planning (Desk Study)

**Input**:

- Digital map
- Demographic data
- Target area

**Output**:

- Estimasi jumlah site
- Kandidat lokasi site (dari Google Earth)
- Rough cost estimate

**Tools**: Atoll + Google Earth

### B. Site Survey

**Aktivitas**:

- Kunjungi lokasi kandidat
- Foto site
- Cek akses (jalan, listrik)
- Cek obstacle
- Negosiasi sewa lahan

**Output**:

- Site survey report
- Foto dokumentasi
- Rekomendasi site final

### C. Detail Planning

**Input**: Hasil site survey

**Output**:

- Parameter lengkap setiap site:
  - Koordinat GPS
  - Antenna height, tilt, azimuth
  - Frequency, bandwidth, power
  - Neighbor list
- Coverage map final
- Interference analysis

### D. BoQ (Bill of Quantity)

**Isi**:

- **Equipment**: eNodeB, antenna, kabel, connector
- **Civil work**: Tower, shelter, grounding, power
- **Transport**: Fiber optic, microwave
- **Services**: Installation, commissioning, integration
- **Total project cost**

**Contoh BoQ**:

```
Project: LTE Expansion - Area Mampang
Jumlah site: 3
Total cost: Rp 12 Miliar

Detail per site:
- eNodeB: Rp 800 juta
- Antenna 3 sektor: Rp 150 juta
- Tower 42m: Rp 400 juta
- Shelter: Rp 200 juta
- Fiber optic 2 km: Rp 100 juta
- Installation & commissioning: Rp 300 juta
- Misc: Rp 50 juta
Total per site: Rp 2 Miliar
```

### E. Implementation

- Procurement
- Installation
- Commissioning
- Integration
- Testing & acceptance

---

## 18. Interface Dimensioning (Transport & Core)

Setelah tahu jumlah site, hitung kebutuhan transport & core:

### A. Backhaul (S1 Interface)

**Bandwidth per site**:

```
BW = (DL Throughput + UL Throughput) × Overhead
```

**Contoh**:

- DL: 84 Mbps (3 sektor × 28 Mbps)
- UL: 21 Mbps (3 sektor × 7 Mbps)
- Overhead: 1.3
- **BW per site = (84 + 21) × 1.3 = 136 Mbps**

Untuk 48 site:

- **Total backhaul = 48 × 136 = 6.5 Gbps**

### B. Aggregation Network

Site → Aggregation Node → Core

**Aggregation ratio**: Biasanya 1:10 atau 1:20

Artinya: 10-20 site di-aggregate ke 1 node

**Contoh**:

- 48 site / 10 = **5 aggregation node**
- Bandwidth per node = 6.5 / 5 = 1.3 Gbps
- Pakai **10 Gbps link** (cukup dengan headroom)

### C. Core Network (MME, SGW, PGW)

**Rule of thumb**:

- 1 MME: 10,000 - 50,000 user
- 1 SGW: 5,000 - 20,000 user
- 1 PGW: 10,000 - 100,000 user

**Contoh**:

- Subscriber: 10,000
- **MME**: 1 unit (cadangan 1)
- **SGW**: 2 unit (aktif-aktif)
- **PGW**: 1 unit (cadangan 1)

---

## 19. Perbedaan Planning untuk Area Berbeda

### Urban (Kota Besar):

- **Karakteristik**: Padat, bangunan tinggi, traffic tinggi
- **Fokus**: Capacity > Coverage
- **Frekuensi**: 1800, 2300 MHz
- **Site density**: Tinggi (0.5-1 km antar site)
- **Challenge**: Interference, indoor penetration

### Suburban (Pinggiran Kota):

- **Karakteristik**: Sedang padat, rumah 1-2 lantai
- **Fokus**: Balance coverage & capacity
- **Frekuensi**: 1800, 900 MHz
- **Site density**: Sedang (1-3 km antar site)
- **Challenge**: Mix indoor/outdoor user

### Rural (Pedesaan):

- **Karakteristik**: Jarang, lahan terbuka, traffic rendah
- **Fokus**: Coverage > Capacity
- **Frekuensi**: 900 MHz (prioritas)
- **Site density**: Rendah (3-10 km antar site)
- **Challenge**: Long distance, NLOS

### Daerah 3T (Terpencil):

- **Karakteristik**: Sangat jarang, akses sulit
- **Fokus**: Coverage minimum
- **Frekuensi**: 900 MHz
- **Site density**: Sangat rendah (> 10 km)
- **Challenge**: Backhaul (pakai satellite/microwave), power (solar/genset)
- **Implementasi Telkomsel**: 900 MHz, 5 MHz bandwidth, 25 PRB

---

## 20. Contoh Kasus Nyata

### Kasus: Telkomsel LTE Deployment di Daerah 3T

**Target Area**: Desa terpencil di Kalimantan

**Requirements**:

- Area: 100 km²
- Populasi: 5,000 orang
- Coverage target: 85% (tidak perlu 95% karena kendala geografis)
- Traffic: 500 Kbps per user (light usage)

**Design Decision**:

- **Frekuensi**: 900 MHz (coverage maksimal)
- **Bandwidth**: 5 MHz (shared dengan 2G/3G)
- **Modulasi**: QPSK (default)

**Perhitungan Coverage**:

1. Cell radius = 5 km (dari link budget 900 MHz)
2. Luas per cell = 2.6 × 5² = 65 km²
3. Jumlah site = 100 / 65 = **2 site** (round up)

**Perhitungan Capacity**:

1. Total traffic = 5,000 × 0.5 × 0.1 = 250 Mbps
2. Throughput per site = 5 MHz × 0.4 × 2 × 3 = 12 Mbps
3. Jumlah site = 250 / 12 = **21 site**

**Hmm... konflik!**

- Coverage butuh 2 site
- Capacity butuh 21 site

**Solusi**:

- Pasang **2 site** dulu (coverage priority)
- Monitor traffic real
- Jika congestion → tambah site bertahap
- Atau: upgrade ke 10 MHz jika tersedia

**Final Design**:

- **2 site LTE** dengan 900 MHz, 5 MHz
- Backhaul: Microwave link (karena fiber tidak tersedia)
- Power: Hybrid solar + genset

**Hasil**:

- Coverage: 85% area tercapai
- User puas (dapat 4G di daerah terpencil!)
- Telkomsel dapat CSR + subsidi pemerintah (USO)

---

## 21. Optimization vs Planning

### Planning (Awal):

- **Tujuan**: Bangun jaringan baru dari nol
- **Input**: Requirements, demographic data
- **Output**: Jumlah site, BoQ, cost
- **Challenge**: Prediksi (belum tahu real traffic)

### Optimization (Setelah Live):

- **Tujuan**: Improve performance jaringan existing
- **Input**: KPI, drive test, complaint
- **Output**: Parameter tuning, site addition
- **Challenge**: Trade-off (coverage vs capacity vs interference)

**Parameter Optimization**:

- **Antenna tilt**: Ubah arah coverage
- **Power**: Naik/turun sesuai kebutuhan
- **Neighbor list**: Tambah/kurangi untuk handover
- **PCI (Physical Cell ID)**: Hindari collision

**Kapan perlu optimasi?**

- KPI coverage < target (RSRP jelek)
- KPI quality < target (SINR jelek, interference tinggi)
- Throughput rendah (congestion)
- Call drop rate tinggi
- Handover failure

---

## 22. Tips untuk Technical Interview

### Yang sering ditanya:

1. **"Jelaskan perbedaan 3G dan 4G!"**

   - Arsitektur (RNC ada/tidak)
   - Interface (Iu vs S1/X2)
   - Technology (WCDMA vs OFDMA)
   - Speed (max 42 Mbps vs 150+ Mbps)

2. **"Apa itu Link Budget?"**

   - Perhitungan daya TX → RX
   - Untuk tahu cell radius
   - Untuk dimensioning coverage

3. **"Parameter apa yang menentukan coverage?"**

   - RSRP (> -110 dBm)
   - SINR (> 0 dB)
   - RSRQ (> -15 dB)

4. **"Bagaimana cara menghitung jumlah site?"**

   - Coverage dimensioning (dari link budget)
   - Capacity dimensioning (dari traffic)
   - Ambil yang lebih besar

5. **"Sudah pernah pakai Atoll?"**

   - Jawab: "Sudah! Untuk coverage simulation, parameter setting, dan drive test analysis"
   - Ceritakan pengalaman (meski baru belajar)

6. **"Apa bedanya Open RAN dan Traditional RAN?"**
   - Open: Multi-vendor, open interface, power efisien
   - Traditional: Single vendor, proprietary

### Yang HARUS dikuasai:

- ✅ Arsitektur LTE (EPS)
- ✅ Interface (S1, X2, S5/S8)
- ✅ Protocol stack (6 layers)
- ✅ Coverage & capacity concept
- ✅ Link budget basics
- ✅ Pernah praktik Atoll (minimal tahu cara pakai)

### Yang bonus (nilai plus):

- 🌟 Pernah baca white paper vendor
- 🌟 Tahu tren 5G
- 🌟 Paham protocol analysis (Wireshark)
- 🌟 Bisa programming (Python untuk automation)
- 🌟 Experience internship/project

**Attitude penting**:

- **Percaya diri** (tapi tidak sombong)
- **Jujur** (kalau tidak tahu, bilang tidak tahu tapi mau belajar)
- **Antusias** (tunjukkan passion di bidang telco)

---

## 23. Studi Kasus: Lima Wilayah Telkomsel

### Implementasi 5G Telkomsel (2024):

**6 Site Awal** di:

1. **Pondok Indah** (Residential premium)
2. **Kelapa Gading** (Shopping district)
3. **SCBD Sudirman** (Business district)
4. **TB Simatupang** (Office area)
5. **BSD** (Residential + office)
6. **Senayan** (Sport & recreation)

**Kenapa pilih lokasi ini?**

- **Heavy user area** (traffic tinggi)
- **High ARPU** (Average Revenue Per User)
- **Early adopter** (mau pakai 5G)
- **Show case** (promosi 5G)

**Planning approach**:

- Forecasting 5 tahun
- Anticipate traffic growth (50% YoY)
- Use case: eMBB, FWA (Fixed Wireless Access)

**Hasil**:

- Telkomsel jadi **operator 5G pertama di Indonesia** (commercial)
- User experience: 500+ Mbps (vs 4G: 50 Mbps)
- Proof of concept sukses → expansion ke area lain

---

## 24. Tugas & Deliverables

### Tugas Individu (Wajib):

**Buat Desain LTE untuk satu area**

**Requirements**:

1. Pilih **area sendiri** (tidak boleh sama dengan teman)
2. Tentukan **skenario**:
   - Frekuensi (900, 1800, 2100, atau 2300 MHz)
   - Bandwidth (5, 10, 15, atau 20 MHz)
   - Tipe area (Urban, Suburban, atau Rural)
3. Lakukan **perhitungan manual**:
   - Link budget
   - Coverage dimensioning
   - Capacity dimensioning
   - Interface dimensioning (S1-U, X2)
4. **Buat simulasi Atoll** (optional, tapi nilai plus)

**Deliverables**:

- Report (PDF, max 20 halaman)
- Excel calculator
- Presentasi (PPT, 10 slide)

**Contoh judul**:

- "Desain LTE 1800 MHz untuk Area Universitas Indonesia"
- "Planning LTE 900 MHz untuk Daerah 3T Kabupaten Mamuju"
- "Capacity Planning LTE 2300 MHz untuk Jakarta CBD"

**Deadline**: 2 minggu setelah materi selesai

---

## 25. Kesimpulan

### LTE Planning itu:

- ✅ **Coverage Planning** → Berapa site agar sinyal bagus di semua area?
- ✅ **Capacity Planning** → Berapa site agar tidak congestion?
- ✅ **Link Budget** → Perhitungan daya dan jarak cell
- ✅ **Dimensioning** → Hitung jumlah site, throughput, interface
- ✅ **Tools** → Manual calculation, Excel, Atoll
- ✅ **Validation** → Simulasi, drive test, KPI monitoring

### Workflow:

1. **Requirement** → Tahu mau apa
2. **Design** → Hitung coverage & capacity
3. **Simulation** → Pakai Atoll, cek coverage map
4. **Validation** → KPI target tercapai?
5. **BoQ** → Buat daftar equipment dan cost
6. **Implementation** → Build!
7. **Optimization** → Tune parameter untuk improve

### Prinsip Penting:

- **Frekuensi rendah** = Coverage luas, capacity kecil
- **Frekuensi tinggi** = Coverage sempit, capacity besar
- **Ambil MAX** antara coverage site dan capacity site
- **Forecasting 5 tahun** → future proof
- **QPSK default** → jaminan coverage minimum

### Next Steps:

- **Part 2**: Perhitungan detail (link budget, formula)
- **Part 3**: Simulation dengan Atoll
- **Part 4**: Transport & core dimensioning
- **Part 5**: BoQ & project costing

---

**Catatan**: Materi ini adalah **fondasi** untuk memahami LTE planning. Di industri real, ada banyak faktor lain (regulasi, budget, kompetitor, dll). Tapi dengan paham konsep ini, sudah 70% jalan! 🚀

**Motivasi**: Network planning engineer adalah salah satu posisi **most wanted** di telco. Gaji bagus, karir jelas, dan pekerjaan menantang. Kuasai skill ini = investasi masa depan!

**Good luck!** 💪
