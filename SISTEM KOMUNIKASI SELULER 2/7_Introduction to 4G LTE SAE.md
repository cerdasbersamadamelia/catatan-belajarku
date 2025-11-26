# Introduction to 4G LTE SAE

## Apa itu 4G LTE?

4G LTE (Long Term Evolution) adalah teknologi jaringan seluler generasi keempat yang dikembangkan oleh **3GPP**. Ada dua standar 4G utama:
- **LTE** (dari 3GPP) - digunakan di Indonesia
- **WiMAX** (dari IEEE) - tidak digunakan di Indonesia

LTE juga disebut **SAE (System Architecture Evolution)** karena merupakan hasil evolusi dari jaringan-jaringan sebelumnya (2G, 3G, dst).

---

## Komponen Utama LTE (EPS)

LTE disebut juga **EPS (Evolved Packet System)** karena fokus pada teknologi paket data. Terdiri dari 3 komponen utama:

### 1. **UE (User Equipment)**
- Perangkat user (smartphone, modem, tablet)
- Ujung dari jaringan yang digunakan pengguna

### 2. **E-UTRAN (Evolved UMTS Terrestrial Radio Access Network)**
- **Access Network** atau bagian radio
- Berisi **eNodeB** (evolved NodeB) - tower BTS yang memancarkan gelombang radio
- UE berkomunikasi dengan eNodeB menggunakan gelombang radio

### 3. **EPC (Evolved Packet Core)**
- **Core Network** - otak dari jaringan
- Komponen utama:
  - **MME (Mobility Management Entity)**
    - Mengatur signaling dan control plane
    - Autentikasi user (cek kartu valid atau tidak)
    - Mobility management (handover antar cell)
    - Session management
  - **SGW (Serving Gateway)**
    - Gateway untuk user data (user plane)
    - Routing paket data dari/ke eNodeB
    - Anchor point saat handover antar eNodeB
  - **PGW (Packet Data Network Gateway)**
    - Gateway keluar ke internet atau jaringan eksternal
    - IP address allocation untuk UE
    - QoS enforcement
    - Charging (billing)
  - **HSS (Home Subscriber Server)**
    - Database user (sim card info, subscription, profile)
    - Mirip HLR di 2G/3G
  - **PCRF (Policy and Charging Rules Function)**
    - Mengatur QoS policy
    - Charging rules (paket data, tarif)

### Alur Data dan Signaling:
```
UE <--radio--> eNodeB <--data--> SGW --> PGW --> Internet
         |                          |
         |--signaling--> MME <--> HSS
                           |       |
                           |---> PCRF
```

**Catatan:**
- **User Plane (Data)**: UE → eNodeB → SGW → PGW → Internet
- **Control Plane (Signaling)**: UE → eNodeB → MME → HSS/PCRF
- MME tidak handle data user, hanya signaling
- SGW/PGW handle data user (streaming, browsing, dll)

---

## Aggregation & Transport Network

Di antara **Access Network** (eNodeB) dan **Core Network** (EPC), ada **Backhaul & Aggregation** yang menghubungkan keduanya.

### Teknologi Transport:

#### 1. **Fiber Optic + IP/MPLS** (paling umum)
- **MPLS (Multiprotocol Label Switching)**
  - Fast forwarding dengan label
  - Traffic engineering (QoS per service)
  - **VPLS (Virtual Private LAN Service)** untuk layer 2 connectivity
- **Metro Ethernet**
  - Simple, native IP
  - Carrier Ethernet (MEF standard)

#### 2. **Microwave (Radio Link)**
- Untuk area yang sulit fiber (pulau, pegunungan, remote area)
- Frequency: 6-42 GHz (licensed)
- Capacity: 100 Mbps - 1 Gbps (depend frequency & weather)
- Latency lebih tinggi dari fiber

#### 3. **Hybrid (Fiber + Microwave)**
- Fiber untuk main backbone
- Microwave untuk last mile ke eNodeB remote

### Interface:
- **S1 Interface:** eNodeB ↔ EPC
  - S1-U: User plane (data) → ke SGW
  - S1-MME: Control plane (signaling) → ke MME
- **X2 Interface:** eNodeB ↔ eNodeB (untuk handover)

**Requirement Transport LTE:**
- **Latency:** < 10 ms (one way)
- **Jitter:** < 5 ms
- **Packet loss:** < 0.001%
- **Bandwidth:** Scalable (10 Gbps+ untuk aggregation node)

Fungsi: Menjembatani eNodeB dengan EPC agar data dan signaling mengalir dengan **low latency** dan **high reliability**.

---

## LTE = All-IP Network

**Perbedaan dengan jaringan sebelumnya:**
- 2G/3G: Circuit-Switched untuk voice, Packet-Switched untuk data
- **4G LTE: 100% Packet-Switched (All-IP)**

### Konsekuensi All-IP:
LTE tidak bisa handle voice call secara native (circuit-switched). Ada 2 solusi:

### 1. **CSFB (Circuit Switched Fallback)**
- Digunakan di **fase awal deployment LTE** atau operator yang belum deploy VoLTE
- Saat ada panggilan voice/SMS:
  - HP "fallback" ke jaringan 3G/2G (perlu waktu ~2-4 detik)
  - Indikator di HP berubah dari 4G → 3G/H+
  - Setelah panggilan selesai, kembali ke 4G
- **Kualitas:** Standar (sama seperti 3G/2G)
- **Kekurangan:** Delay saat switch, tidak bisa bersamaan internet cepat + voice

### 2. **VoLTE (Voice over LTE)**
- Voice call dilakukan **di atas jaringan LTE** menggunakan paket IP
- Memerlukan **IMS (IP Multimedia Subsystem)** di core network
- HP tetap di 4G saat telepon, bisa internet cepat sambil telepon
- **Kualitas:** HD Voice (lebih jernih), video call lebih smooth
- **Kelebihan:** 
  - Call setup lebih cepat (~1 detik)
  - Bisa browsing 4G sambil telepon
  - Battery efficient

**Kesimpulan:** VoLTE > CSFB. Semua operator besar Indonesia (Telkomsel, XL, Indosat, Tri) sudah deploy VoLTE.

---

## Kenapa Perlu 4G LTE?

### 1. **Peningkatan Jumlah User**
- Populasi manusia meningkat → pengguna seluler bertambah
- 3G (HSPA/HSDPA) sudah cepat, tapi **kapasitas tidak cukup** untuk menampung user yang sangat banyak

### 2. **Kelemahan 3G (W-CDMA)**
3G menggunakan **CDMA (Code Division Multiple Access)**:
- Multiplexing berdasarkan kode
- Sudah cukup cepat
- Tapi **kapasitas terbatas** - tidak bisa handle user dalam jumlah besar secara optimal

### Solusi LTE: **OFDMA**

---

## OFDMA (Orthogonal Frequency Division Multiple Access)

**Teknologi multiplexing baru di LTE** yang menggantikan CDMA.

### Cara Kerja:
1. **FDMA (Frequency Division Multiple Access)**
   - Membagi kanal berdasarkan frekuensi
   - Bandwidth LTE dipecah jadi **subcarrier-subcarrier kecil** (15 kHz per subcarrier)

2. **Orthogonal**
   - Subcarrier-subcarrier bisa **dirapatkan** tanpa saling ganggu
   - Dengan teknik orthogonal, frekuensi overlapping tapi tidak interferensi
   - Efisiensi spektrum sangat tinggi

3. **Multiple Access**
   - Setiap user dialokasikan sejumlah subcarrier tertentu
   - Alokasi dinamis sesuai kebutuhan (user ramai = dapat lebih banyak subcarrier)

### Konsep Resource Block (RB):
- **1 RB = 12 subcarrier x 0.5 ms (1 slot)**
- Bandwidth LTE:
  - 5 MHz = 25 RB
  - 10 MHz = 50 RB
  - 20 MHz = 100 RB
- Scheduler eNodeB alokasikan RB ke user sesuai kondisi channel dan kebutuhan

### Keuntungan OFDMA:
- **Kapasitas tinggi** → bisa handle ratusan user simultan
- **Tahan multipath** (sinyal pantul dari gedung, gunung)
- **Flexible bandwidth**: support 1.4, 3, 5, 10, 15, 20 MHz
- Efisiensi spektrum sangat baik

### Cyclic Prefix (CP):
- **Fungsi:** Guard time untuk mengatasi delay spread (sinyal pantul terlambat tiba)
- **Jenis:**
  - Normal CP: untuk area urban/suburban
  - Extended CP: untuk area dengan delay spread tinggi (pulau-pulau kecil)
- Trade-off: CP makan overhead ~7%, tapi crucial untuk performa

### Downlink vs Uplink:
- **Downlink (eNodeB → UE):** **OFDMA**
  - Bisa alokasikan banyak user simultan
- **Uplink (UE → eNodeB):** **SC-FDMA (Single Carrier FDMA)**
  - Hemat battery HP (PAPR rendah)
  - Prinsip mirip OFDMA tapi lebih power-efficient

---

## Teknik IFFT dan FFT

LTE menggunakan teknik **domain transformation**:

1. **Di pengirim (eNodeB → UE):**
   - Data dalam **frequency domain** diubah ke **time domain** menggunakan **IFFT (Inverse Fast Fourier Transform)**

2. **Di penerima (UE):**
   - Data dalam time domain diubah kembali ke **frequency domain** menggunakan **FFT (Fast Fourier Transform)**

**Tujuan:** Memudahkan transmisi dan mengurangi kompleksitas pengiriman data.

---

## Modulasi di LTE

LTE menggunakan **adaptive modulation** yang menyesuaikan dengan kondisi sinyal.

### Jenis Modulasi:

### 1. **QPSK (Quadrature Phase Shift Keying)**
- **2 bits per symbol**
- Digunakan saat **SINR rendah** (sinyal lemah, user jauh, atau interferensi tinggi)
- Kecepatan rendah, tapi **robust** (stabil dan tidak putus)
- Contoh: di pinggir cell, dalam gedung

### 2. **16-QAM (Quadrature Amplitude Modulation)**
- **4 bits per symbol**
- Digunakan saat **SINR sedang**
- Balance antara kecepatan dan stabilitas
- Paling umum dipakai di coverage normal

### 3. **64-QAM**
- **6 bits per symbol**
- Digunakan saat **SINR baik** (sinyal kuat, dekat eNodeB)
- Kecepatan tinggi
- Standard max LTE Release 8-10

### 4. **256-QAM**
- **8 bits per symbol**
- Digunakan saat **SINR sangat baik**
- Kecepatan tertinggi (LTE-Advanced Pro / 4.5G)
- Hanya available di:
  - HP flagship yang support 256-QAM
  - Coverage sangat bagus (dekat eNodeB, line of sight)
  - Tidak semua operator deploy

### Adaptive Modulation and Coding (AMC):
```
SINR Rendah (<5 dB)     →  QPSK + Code Rate rendah → Stabil tapi lambat
SINR Sedang (5-15 dB)   →  16-QAM                  → Balanced
SINR Bagus (15-25 dB)   →  64-QAM                  → Cepat
SINR Sangat Bagus (>25) →  256-QAM (jika support)  → Sangat cepat
```

**Real Practice:**
- eNodeB terus monitor **CQI (Channel Quality Indicator)** yang dilaporkan UE setiap beberapa millisecond
- Scheduler dinamis pilih **MCS (Modulation and Coding Scheme)** terbaik
- User tidak merasakan perpindahan - seamless
- Bisa berubah dalam hitungan milidetik (misal: HP bergerak, masuk terowongan, dll)

### Throughput Maksimal:
- **QPSK:** ~10-20 Mbps (downlink 20 MHz)
- **16-QAM:** ~40-60 Mbps
- **64-QAM:** ~100-150 Mbps (LTE Cat 4-6)
- **256-QAM:** ~200-300 Mbps (LTE-A Pro)

*Note: Throughput actual tergantung bandwidth, MIMO layers, dan kondisi jaringan*

---

## Ringkasan

| Aspek | Penjelasan |
|-------|------------|
| **Nama** | 4G LTE (Long Term Evolution) atau SAE (System Architecture Evolution) |
| **Standar** | 3GPP (Release 8 ke atas) |
| **Arsitektur** | EPS (Evolved Packet System): UE + E-UTRAN + EPC |
| **Access Network** | E-UTRAN (eNodeB) |
| **Core Network** | EPC (MME, SGW, PGW, HSS, PCRF) |
| **Transport** | IP/MPLS, VPLS, Metro Ethernet |
| **Multiplexing** | Downlink: OFDMA, Uplink: SC-FDMA |
| **Modulasi** | QPSK, 16-QAM, 64-QAM, 256-QAM (adaptive) |
| **Bandwidth** | 1.4, 3, 5, 10, 15, 20 MHz (flexible) |
| **Frekuensi di ID** | 900 MHz, 1800 MHz, 2100 MHz, 2300 MHz |
| **Voice Solution** | CSFB (legacy) atau VoLTE (with IMS) |
| **Peak Speed** | DL: 300 Mbps (Cat 6), UL: 50 Mbps (20 MHz, 2x2 MIMO) |
| **Latency** | ~20-30 ms (jauh lebih rendah dari 3G: ~100 ms) |
| **Keunggulan** | Kapasitas tinggi, low latency, all-IP, efisien spektrum, MIMO |

### Kecepatan Real di Indonesia:
- **Telkomsel:** 30-100 Mbps (depend area)
- **XL/Indosat:** 20-80 Mbps
- **Tri:** 10-60 Mbps
- **Smartfren:** 5-40 Mbps

*Actual speed tergantung: coverage, user density, device capability, bandwidth yang di-deploy*

---

## Next Topics
Pada materi selanjutnya akan dibahas lebih detail:
- Access Network (E-UTRAN) dan eNodeB
- Core Network (EPC) dan komponen-komponennya
- Roaming di LTE
- Interkoneksi dengan jaringan lain
- CSFB dan VoLTE secara mendalam
- MPLS dan teknologi transport