# Jaringan 2G (GSM)

## Arsitektur Jaringan 2G

Arsitektur GSM terbagi menjadi 3 subsistem utama:

### A. BSS (Base Station Subsystem) - Radio Access Network

#### 1. MS (Mobile Station)
- MS terdiri dari 2 komponen:
  - **ME (Mobile Equipment)**: perangkat fisik HP
  - **SIM Card**: menyimpan IMSI, Ki (authentication key), dan data subscriber
- MS adalah user equipment yang berkomunikasi dengan jaringan

#### 2. BTS (Base Transceiver Station)
- Tower yang memancarkan dan menerima sinyal radio
- 1 BTS bisa punya beberapa **sektor** (biasanya 3 sektor: 0°, 120°, 240°)
- 1 sektor bisa punya beberapa **TRX (Transceiver)** untuk menambah kapasitas
- **Interface**: Um (antara MS dan BTS) menggunakan air interface
- Fungsi: modulasi, demodulasi, channel coding, power control

#### 3. BSC (Base Station Controller)
- Mengontrol puluhan hingga ratusan BTS
- **Interface**: Abis (antara BTS dan BSC)
- Fungsi utama:
  - **Handover management**: pindah dari satu cell ke cell lain
  - **Radio resource management**: alokasi channel
  - **Power control**: mengatur daya transmisi MS dan BTS
  - **Frequency hopping**: mengurangi interferensi

### 3. BSC (Base Station Controller)
- Perangkat switching yang mengatur banyak BTS
- Karena BTS di Indonesia jumlahnya sangat banyak, maka perlu diatur oleh BSC
- Beberapa BTS akan terhubung ke satu BSC

### B. NSS (Network Switching Subsystem) - Core Network

#### 1. MSC (Mobile Switching Center)
- Otak dari jaringan GSM yang mengatur call routing dan switching
- **Interface**: A-interface (antara BSC dan MSC)
- Fungsi:
  - Call setup, routing, dan release
  - Mobility management
  - SMS routing
  - Interworking dengan PSTN (jaringan telepon tetap)
- Ada 2 tipe:
  - **GMSC (Gateway MSC)**: untuk koneksi ke jaringan lain
  - **VMSC (Visited MSC)**: untuk user yang roaming

#### 2. VLR (Visitor Location Register)
- Database sementara untuk user yang sedang berada di coverage area MSC
- Menyimpan data dinamis:
  - **Location Area (LA)**: posisi user saat ini
  - **TMSI (Temporary IMSI)**: identitas sementara untuk keamanan
  - Status roaming
- VLR ter-attach dengan MSC (biasanya dalam 1 perangkat)
- Update otomatis saat user melakukan **Location Update**

#### 3. HLR (Home Location Register)
- Database permanen semua subscriber di operator
- Menyimpan:
  - **IMSI** (International Mobile Subscriber Identity)
  - **MSISDN** (nomor HP: 628xxx...)
  - Profile layanan (call forwarding, call barring, roaming aktif/tidak)
  - Lokasi terakhir user (VLR mana yang melayani)
- Satu operator biasanya punya beberapa HLR untuk redundancy

#### 4. AuC (Authentication Center)
- Mengurus security dan authentication
- Menyimpan **Ki (Authentication Key)** untuk setiap subscriber
- Generate:
  - **RAND (Random Number)**: challenge untuk authentication
  - **SRES (Signed Response)**: expected response
  - **Kc (Cipher Key)**: untuk enkripsi komunikasi
- Biasanya terintegrasi dengan HLR

#### 5. EIR (Equipment Identity Register)
- Database untuk validasi perangkat MS
- Menyimpan **IMEI** (15 digit unique per perangkat)
- Ada 3 daftar:
  - **White List**: perangkat yang legal dan boleh akses
  - **Grey List**: perangkat bermasalah tapi masih boleh akses
  - **Black List**: perangkat ilegal/hilang/dicuri, DITOLAK aksesnya
- EIR optional, tidak semua operator mengaktifkan

---

### C. OSS (Operation Support Subsystem)

#### OMC (Operation & Maintenance Center)
- Untuk monitoring dan maintenance jaringan
- Fungsi:
  - Network monitoring real-time
  - Performance management
  - Fault management
  - Configuration management
  - Software upgrade untuk BTS/BSC/MSC

---

## Teknologi dalam 2G (GSM)

### Multiple Access: TDMA + FDMA
- **FDMA**: spektrum dibagi menjadi carrier frequency (200 kHz per carrier)
- **TDMA**: setiap carrier dibagi menjadi 8 timeslot
- 1 user = 1 timeslot
- Jadi 1 carrier bisa melayani 8 user sekaligus

### Modulasi
- **GMSK** (Gaussian Minimum Shift Keying)
- Data rate: **270.833 kbps** per carrier (sebelum channel coding)
- User data rate per timeslot: sekitar **13-22.8 kbps** (tergantung codec)

### Channel Structure
#### Physical Channel
- Kombinasi frequency + timeslot
- Contoh: ARFCN 20, Timeslot 3

#### Logical Channel
Ada 2 kategori:

**1. Traffic Channel (TCH)**
- **TCH/FS (Full Rate Speech)**: 13 kbps (codec FR)
- **TCH/HS (Half Rate Speech)**: 5.6 kbps (codec HR)
- **TCH/F9.6**: data 9.6 kbps
- **TCH/F4.8**: data 4.8 kbps

**2. Control Channel**
- **BCCH**: broadcast info system (frequency, LAI, cell ID)
- **PCH**: paging untuk incoming call/SMS
- **RACH**: random access dari MS untuk request channel
- **AGCH**: assignment channel untuk alokasi TCH
- **SDCCH**: signaling untuk location update, authentication, SMS
- **SACCH**: monitoring kualitas link (power control, TA)
- **FACCH**: fast signaling saat handover

### Layanan GSM
**Circuit Switched:**
- Voice call (utama)
- **CSD (Circuit Switched Data)**: data maks 9.6/14.4 kbps
- Fax

**Packet Switched (GPRS - 2.5G):**
- Ini upgrade dari GSM murni
- Internet dengan kecepatan hingga **171 kbps** (teoritis)
- Pakai **SGSN** dan **GGSN** (network element baru)
- Coding scheme: CS-1 sampai CS-4

**Layanan tambahan:**
- SMS (Short Message Service): 160 karakter
- Call forwarding, call waiting, call barring
- USSD (Unstructured Supplementary Service Data): *123#

### Switching Technology
- **Circuit Switching** untuk voice
- End-to-end dedicated connection selama call
- Cocok untuk real-time communication

---

## Frekuensi GSM

### GSM Band di Dunia
- **GSM 850**: 824-849 MHz (UL), 869-894 MHz (DL) - Amerika
- **GSM 900**: 890-915 MHz (UL), 935-960 MHz (DL) - Eropa, Asia, Afrika
- **GSM 1800 (DCS)**: 1710-1785 MHz (UL), 1805-1880 MHz (DL)
- **GSM 1900 (PCS)**: 1850-1910 MHz (UL), 1930-1990 MHz (DL) - Amerika

### Di Indonesia
- **GSM 900**: band utama sejak awal (Telkomsel, Indosat, XL)
- **GSM 1800**: untuk menambah kapasitas di area padat
- Channel spacing: **200 kHz**
- Duplex spacing: **45 MHz** (untuk 900), **95 MHz** (untuk 1800)

### Refarming
- Frekuensi 900 MHz sekarang banyak di-refarming untuk **LTE (L900)**
- Frekuensi 1800 MHz juga di-refarming untuk **LTE (L1800)**
- Alasan:
  - 2G traffic menurun drastis (orang beralih ke 4G)
  - 900 MHz punya propagasi lebih baik (coverage lebih luas)
  - Efisiensi spektrum untuk layanan data

---

## Prosedur Penting dalam GSM

### 1. Location Update
- MS memberitahu network posisinya saat pindah Location Area (LA)
- Trigger:
  - Periodic update (timer habis)
  - Pindah ke LA baru
  - Power on HP
  - IMSI attach
- Flow: MS → BTS → BSC → MSC/VLR → HLR

### 2. Authentication
- Network memverifikasi identitas subscriber
- Menggunakan challenge-response mechanism:
  1. AuC generate RAND
  2. Network kirim RAND ke MS
  3. SIM card di MS compute SRES pakai Ki
  4. MS kirim SRES ke network
  5. Network compare dengan expected SRES
  6. Jika cocok → authenticated ✓

### 3. Ciphering
- Enkripsi komunikasi air interface
- Pakai algoritma **A5** (A5/1, A5/2, A5/3)
- Key: **Kc** yang di-generate saat authentication
- Melindungi privacy user dari penyadapan

### 4. Handover
- Perpindahan MS dari satu cell ke cell lain saat sedang call
- Tipe:
  - **Intra-cell handover**: pindah timeslot/frequency di cell sama
  - **Intra-BSC handover**: antar cell dalam BSC sama
  - **Inter-BSC handover**: antar BSC dalam MSC sama
  - **Inter-MSC handover**: antar MSC (paling kompleks)
- Trigger: signal quality buruk, MS bergerak, load balancing

### 5. Paging
- Network mencari MS untuk incoming call/SMS
- MSC kirim paging ke semua BTS dalam Location Area
- MS yang di-paging akan respond via RACH

### 6. Call Setup
**Mobile Originated Call (MOC):**
1. MS request channel via RACH
2. Network assign SDCCH via AGCH
3. Authentication & ciphering
4. MS kirim call setup (nomor tujuan)
5. Network assign TCH
6. Ringing & connect

**Mobile Terminated Call (MTC):**
1. Network paging MS
2. MS respond
3. Network assign SDCCH
4. Authentication & ciphering
5. Network assign TCH
6. MS ringing & connect

---

## Identitas dalam GSM

### Di Network Side
- **IMSI**: 15 digit (MCC+MNC+MSIN) - permanent identity di SIM
  - Contoh: 510 10 1234567890
  - 510 = Indonesia, 10 = Telkomsel
- **TMSI**: temporary identity (32 bit), lebih aman, sering berubah
- **MSISDN**: nomor HP (contoh: +628123456789)
- **IMEI**: 15 digit identity untuk perangkat ME

### Di Cell Side
- **CGI (Cell Global Identity)**: MCC+MNC+LAC+CI
- **LAI (Location Area Identity)**: MCC+MNC+LAC
- **BSIC (Base Station Identity Code)**: NCC+BCC (identifikasi cell)

---

## Parameter Penting

### Timing Advance (TA)
- Mengkompensasi delay propagasi
- Range: 0-63 (1 TA = 3.69 μs = 550 meter jarak)
- TA = 0: MS sangat dekat ke BTS
- TA = 63: MS sekitar 35 km dari BTS
- Maximum cell radius GSM: **35 km**

### Power Control
- **MS Power Class**: 1-5 (class 4 = 2W paling umum di HP)
- **Power Control Level**: 0-31 (menurunkan dari max power)
- Tujuan: hemat baterai, kurangi interferensi
- Update via SACCH tiap 480 ms

### Frequency Hopping
- MS berpindah frequency setiap burst (4.615 ms)
- **Baseband hopping**: hopping antar frequency dalam 1 TRX
- **Synthesizer hopping**: hopping antar TRX
- Manfaat: diversity gain, kurangi interferensi, anti fading

---

## KPI (Key Performance Indicator) GSM

### Accessibility
- **CSSR (Call Setup Success Rate)**: % panggilan berhasil setup
- Target: > 98%
- Formula: (Successful Call Setup / Total Call Attempts) × 100%

### Retainability
- **DCR (Drop Call Rate)**: % panggilan putus abnormal
- Target: < 2%
- Formula: (Dropped Calls / Total Successful Calls) × 100%

### Integrity
- **Speech Quality (SQI)**: berdasarkan RXQUAL
- **Handover Success Rate**: % HO berhasil
- Target: > 95%

### Availability
- **BTS Availability**: % waktu BTS operasional
- Target: > 99.5%

---

## Interface dalam GSM

| Interface | Antara | Protokol |
|-----------|--------|----------|
| **Um** | MS ↔ BTS | Air interface, Layer 1/2/3 |
| **Abis** | BTS ↔ BSC | LAPD, TDM (E1/T1) |
| **A** | BSC ↔ MSC | SS7 (SCCP, BSSAP) |
| **B** | MSC ↔ VLR | MAP |
| **C** | MSC ↔ HLR | MAP |
| **D** | VLR ↔ HLR | MAP |
| **E** | MSC ↔ MSC | MAP |
| **F** | MSC ↔ EIR | MAP |
| **G** | VLR ↔ VLR | MAP |

*MAP = Mobile Application Part (protokol SS7)*

---

## Ringkasan Arsitektur Lengkap

```
        [MS] ←→ Um ←→ [BTS] ←→ Abis ←→ [BSC] ←→ A ←→ [MSC/VLR]
         ↑                                            ↓
     ME + SIM                                    [HLR/AuC]
                                                      ↓
                                                   [EIR]
                                                      ↓
                                                  [GMSC] → PSTN/PLMN lain
```

---

## Evolusi dari GSM

1. **GSM (2G)**: Voice + SMS, max 14.4 kbps
2. **GPRS (2.5G)**: Packet switching, max 171 kbps
3. **EDGE (2.75G)**: Enhanced data rate, max 384 kbps (pakai 8PSK)
4. **UMTS (3G)**: WCDMA, hingga 384 kbps - 2 Mbps
5. **HSPA (3.5G)**: HSDPA+HSUPA, hingga 14-42 Mbps
6. **LTE (4G)**: OFDMA, hingga 100-300 Mbps
7. **5G NR**: mmWave + sub-6GHz, hingga 1-10 Gbps

---

## Fun Facts GSM Indonesia

- **1993**: Telkomsel launch GSM pertama di Indonesia
- **1995**: Satelindo (sekarang Indosat) mulai operasi
- **2021-2025**: Sunset GSM, operator mulai shutdown 2G untuk realokasi spektrum ke 4G/5G
- SMS pertama di dunia dikirim tahun **1992**: "Merry Christmas"
- Standar GSM dibuat oleh **ETSI** (European Telecommunications Standards Institute)
- GSM digunakan di **219 negara** dan punya **90% market share** di dunia (era puncak)

---

**Kesimpulan:**
- GSM = Global System for Mobile Communications
- Teknologi 2G yang merevolusi komunikasi mobile
- Arsitektur: BSS (MS-BTS-BSC) → NSS (MSC-VLR-HLR-AuC-EIR)
- Multiple access: TDMA/FDMA, Modulasi: GMSK
- Layanan utama: Voice (circuit switched), SMS
- Evolusi: GPRS → EDGE → 3G → 4G → 5G
- Sekarang sedang fase sunset, digantikan oleh VoLTE