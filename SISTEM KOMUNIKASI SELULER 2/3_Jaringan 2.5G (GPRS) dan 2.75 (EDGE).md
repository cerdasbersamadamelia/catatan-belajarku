# Jaringan 2.5G (GPRS) dan 2.75G (EDGE)

## Evolusi dari GSM

GSM murni (2G) → GPRS (2.5G) → EDGE (2.75G)

Ini adalah **upgrade bertahap** dari GSM untuk mendukung layanan data packet-switched.

---

## GPRS (2.5G) - General Packet Radio Service

### Apa itu GPRS?
- Upgrade dari GSM yang menambahkan kemampuan **packet-switched data**
- GSM = voice only, GPRS = voice + data
- **Tahun deploy**: 2000-2002 di Indonesia
- Generasi: **2.5G** (pertengahan antara 2G dan 3G)

### Perubahan Arsitektur

**Arsitektur GSM (2G):**
```
MS → BTS → BSC → MSC → HLR/VLR/AuC/EIR
```

**Arsitektur GPRS (2.5G) - MENAMBAHKAN:**
```
MS → BTS → BSC ──┬→ MSC (untuk voice - circuit switched)
                 └→ SGSN → GGSN → Internet (untuk data - packet switched)
```

Jadi ada **2 jalur parallel**:
- **Jalur lama (MSC)**: untuk voice, SMS → circuit switched
- **Jalur baru (SGSN-GGSN)**: untuk data internet → packet switched

---

## Network Element Baru di GPRS

### 1. SGSN (Serving GPRS Support Node)
- Fungsi seperti **MSC tapi untuk data**
- Posisi: di core network, connect ke BSC
- **Interface**: Gb (antara BSC dan SGSN)
- **Tugas utama:**
  - **Mobility management** untuk packet data
  - **Session management** - aktivasi PDP context
  - **Authentication & ciphering** untuk data session
  - **Routing packet data** ke/dari MS
  - **Charging** - catat penggunaan data (per KB/MB)
- SGSN terhubung ke HLR (untuk database subscriber)

### 2. GGSN (Gateway GPRS Support Node)
- Fungsi seperti **gateway ke internet**
- Posisi: di core network, connect ke SGSN
- **Interface**: Gn (antara SGSN dan GGSN)
- **Tugas utama:**
  - **IP address allocation** - kasih IP ke MS
  - **Gateway ke external network** (internet, corporate network)
  - **Firewall dan filtering**
  - **Charging Gateway** - billing untuk data
  - **Routing** - route packet dari/ke internet
- GGSN punya interface **Gi** ke internet

### 3. PCU (Packet Control Unit)
- **Posisi**: di BSC atau bisa standalone
- Fungsi: mengatur packet data scheduling di air interface
- Memisahkan traffic voice (ke MSC) dan data (ke SGSN)

---

## Konsep Penting GPRS

### PDP Context (Packet Data Protocol Context)
- Ini adalah **"sesi data"** antara MS dan GGSN
- Seperti "koneksi internet" yang harus diaktivasi dulu sebelum browsing
- Berisi info:
  - **PDP Type**: IPv4, IPv6, atau PPP
  - **PDP Address**: IP address yang dialokasi
  - **QoS profile**: prioritas dan kecepatan
  - **APN (Access Point Name)**: "internet", "mms", dll

**Proses aktivasi PDP Context:**
1. MS kirim "Activate PDP Context Request" ke SGSN
2. SGSN forward ke GGSN
3. GGSN alokasi IP address
4. GGSN confirm ke SGSN
5. SGSN confirm ke MS
6. **Sekarang MS bisa internetan!** 🌐

### APN (Access Point Name)
- Seperti "pintu masuk" ke layanan data
- Contoh APN:
  - **"internet"**: untuk browsing umum
  - **"mms"**: untuk MMS (Multimedia Message Service)
  - **"wap"**: untuk WAP browsing (jaman dulu)
  - **"enterprise.company.com"**: VPN corporate

### Routing Area (RA)
- Seperti Location Area (LA) di GSM, tapi untuk data
- 1 Routing Area = beberapa cell
- Saat MS pindah RA → **Routing Area Update** ke SGSN
- Lebih granular dari LA (RA lebih kecil dari LA)

---

## Channel di GPRS

### Logical Channel Baru
- **PDCH (Packet Data Channel)**: channel untuk kirim/terima packet data
- **PDTCH (Packet Data Traffic Channel)**: bawa user data
- **PACCH (Packet Associated Control Channel)**: kontrol per user
- **PBCCH (Packet Broadcast Control Channel)**: broadcast info GPRS
- **PPCH (Packet Paging Channel)**: paging untuk packet data
- **PRACH (Packet Random Access Channel)**: access request untuk data
- **PAGCH (Packet Access Grant Channel)**: grant access

### Timeslot Allocation
- GPRS pakai timeslot yang sama seperti GSM (8 TS per carrier)
- **Multislot capability**: MS bisa pakai beberapa TS sekaligus
- Contoh:
  - **Multislot Class 10**: 4 DL + 2 UL = max 6 TS
  - **Multislot Class 12**: 4 DL + 4 UL = max 8 TS
- Semakin banyak TS → semakin cepat

---

## Coding Scheme (CS) di GPRS

GPRS pakai 4 coding scheme dengan trade-off antara **kecepatan vs reliabilitas**:

| CS | Modulasi | Data Rate per TS | Total 8 TS | FEC | Kualitas Signal |
|----|----------|------------------|------------|-----|-----------------|
| **CS-1** | GMSK | 9.05 kbps | 72.4 kbps | Tinggi (1/2) | Buruk (robust) |
| **CS-2** | GMSK | 13.4 kbps | 107.2 kbps | Medium (2/3) | Sedang |
| **CS-3** | GMSK | 15.6 kbps | 124.8 kbps | Rendah (3/4) | Bagus |
| **CS-4** | GMSK | 21.4 kbps | **171.2 kbps** | Tidak ada | Sangat bagus |

**FEC (Forward Error Correction)**: error correction coding
- CS-1: paling banyak FEC → paling lambat tapi paling tahan error
- CS-4: tidak ada FEC → paling cepat tapi butuh signal bagus

**Link Adaptation**: network otomatis switch CS berdasarkan kualitas signal
- Signal bagus → pakai CS-4 (cepat)
- Signal jelek → turun ke CS-1 (lambat tapi stabil)

---

## QoS (Quality of Service) GPRS

Ada 4 QoS class:

| Class | Nama | Contoh Aplikasi | Priority |
|-------|------|-----------------|----------|
| **1** | Conversational | Voice over IP | Tertinggi |
| **2** | Streaming | Video streaming | Tinggi |
| **3** | Interactive | Web browsing, email | Sedang |
| **4** | Background | Download, software update | Terendah |

**Parameter QoS:**
- **Precedence**: prioritas traffic (high, normal, low)
- **Delay**: latency maksimal
- **Reliability**: level error protection
- **Peak throughput**: kecepatan puncak
- **Mean throughput**: kecepatan rata-rata

---

## Layanan GPRS

### Data Service
- **WAP (Wireless Application Protocol)**: browsing mobile (era 2000-an)
- **Mobile Internet**: browsing web
- **Email**: push email, POP3, IMAP
- **Instant Messaging**: Yahoo Messenger, MSN (jaman dulu)

### MMS (Multimedia Messaging Service)
- Kirim gambar, audio, video via pesan
- Pakai APN khusus: "mms"
- Ukuran max: 100-300 KB (tergantung operator)
- Proses: HP upload ke MMS Center → recipient download dari MMS Center

### Always-On Internet
- Berbeda dengan **CSD (Circuit Switched Data)** di GSM
- **CSD**: dial-up seperti modem, bayar per menit, max 14.4 kbps
- **GPRS**: always-on, bayar per MB, lebih cepat

---

## EDGE (2.75G) - Enhanced Data rates for GSM Evolution

### Apa itu EDGE?
- Upgrade dari GPRS dengan **modulasi lebih canggih**
- **Software upgrade**, bukan hardware baru
- Generasi: **2.75G**
- Also known as: **EGPRS (Enhanced GPRS)**
- Tahun deploy: 2003-2005 di Indonesia

### Perbedaan Utama: Modulasi

| Parameter | GPRS | EDGE |
|-----------|------|------|
| **Modulasi** | GMSK only | GMSK + **8-PSK** |
| **Bits per symbol** | 1 bit | 1 bit (GMSK) atau **3 bits (8-PSK)** |
| **Max speed per TS** | 21.4 kbps (CS-4) | **59.2 kbps (MCS-9)** |
| **Max theoretical** | 171.2 kbps | **473.6 kbps** |
| **Real world speed** | 40-80 kbps | 100-200 kbps |

### Modulasi 8-PSK
- **8-PSK = 8 Phase Shift Keying**
- Menggunakan 8 phase yang berbeda
- **3 bits per symbol** (2³ = 8 kombinasi)
- Contoh: 000, 001, 010, 011, 100, 101, 110, 111
- Lebih cepat 3x dari GMSK (1 bit/symbol)
- Tapi butuh **signal lebih bagus** (SNR lebih tinggi)

---

## Modulation and Coding Scheme (MCS) di EDGE

EDGE punya **9 MCS** (lebih banyak dari 4 CS di GPRS):

| MCS | Modulasi | Data Rate per TS | Total 8 TS | FEC | Untuk Signal |
|-----|----------|------------------|------------|-----|--------------|
| **MCS-1** | GMSK | 8.8 kbps | 70.4 kbps | Tinggi | Sangat buruk |
| **MCS-2** | GMSK | 11.2 kbps | 89.6 kbps | Tinggi | Buruk |
| **MCS-3** | GMSK | 14.8 kbps | 118.4 kbps | Medium | Sedang |
| **MCS-4** | GMSK | 17.6 kbps | 140.8 kbps | Medium | Sedang |
| **MCS-5** | 8-PSK | 22.4 kbps | 179.2 kbps | Tinggi | Buruk (8-PSK) |
| **MCS-6** | 8-PSK | 29.6 kbps | 236.8 kbps | Medium | Sedang |
| **MCS-7** | 8-PSK | 44.8 kbps | 358.4 kbps | Rendah | Bagus |
| **MCS-8** | 8-PSK | 54.4 kbps | 435.2 kbps | Rendah | Bagus |
| **MCS-9** | 8-PSK | **59.2 kbps** | **473.6 kbps** | Sangat rendah | Sangat bagus |

**Link Adaptation di EDGE:**
- Network pilih MCS berdasarkan **C/I (Carrier to Interference ratio)**
- Signal bagus → MCS-9 (super cepat)
- Signal menurun → MCS-7, MCS-6, ...
- Signal jelek → MCS-1 atau fallback ke GPRS CS-1

---

## Upgrade dari GPRS ke EDGE

### Yang Perlu Diupgrade:

**1. RAN Side (Radio Access Network):**
- **BTS**: upgrade software + hardware (TRX support 8-PSK)
  - Tambah **ERU (EDGE Radio Unit)** atau
  - Upgrade **BBU (Baseband Unit)**
- **BSC/PCU**: upgrade software untuk MCS dan link adaptation
- Tidak perlu ganti antena atau tower

**2. Core Network:**
- **TIDAK perlu upgrade!** 🎉
- SGSN, GGSN tetap sama
- Karena perbedaan hanya di air interface (Um)

**3. MS (Mobile Station):**
- HP harus support EDGE
- **EDGE Class**: 1-12 (multislot capability)
- Contoh: EDGE Class 10 → 4 DL + 2 UL

---

## Interface GPRS/EDGE

| Interface | Antara | Protokol | Fungsi |
|-----------|--------|----------|--------|
| **Um** | MS ↔ BTS | GPRS/EDGE air interface | Radio |
| **Gb** | BSC/PCU ↔ SGSN | Frame Relay / IP | Packet data dari RAN ke Core |
| **Gn** | SGSN ↔ GGSN | GTP (GPRS Tunneling Protocol) | Tunnel user data |
| **Gi** | GGSN ↔ Internet | IP | Gateway ke external network |
| **Gr** | SGSN ↔ HLR | MAP | Query subscriber data |
| **Gc** | GGSN ↔ HLR | MAP | Query subscriber data |
| **Gd** | SGSN ↔ SMS-C | MAP | SMS over GPRS |
| **Gf** | SGSN ↔ EIR | MAP | Check IMEI |

**GTP (GPRS Tunneling Protocol):**
- Protokol khusus untuk "tunneling" packet data user
- Ada 2 jenis:
  - **GTP-C (Control plane)**: signaling, management
  - **GTP-U (User plane)**: actual user data
- Dipakai di interface Gn dan Gp (roaming)

---

## Identitas Baru di GPRS/EDGE

### P-TMSI (Packet TMSI)
- Temporary identity untuk packet data (seperti TMSI untuk circuit)
- 32-bit, berubah secara periodik
- Lebih aman dari IMSI

### TLLI (Temporary Logical Link Identity)
- Derived dari P-TMSI
- Dipakai di air interface (Um) dan Gb interface
- Identifikasi MS dalam GPRS session

### TI (Transaction Identifier)
- ID untuk setiap PDP context
- Range: 0-15 (max 16 PDP context per MS)

---

## Prosedur Penting GPRS/EDGE

### 1. GPRS Attach
- MS "register" diri ke GPRS network
- Proses:
  1. MS kirim "GPRS Attach Request" ke SGSN
  2. SGSN authentication (via HLR/AuC)
  3. SGSN alokasi P-TMSI
  4. SGSN confirm "GPRS Attach Accept"
  5. MS sekarang **"GPRS attached"** - siap pakai data

### 2. PDP Context Activation
- MS aktivasi sesi data (lihat penjelasan di atas)
- Bisa activate multiple PDP context untuk beda APN

### 3. Routing Area Update
- Saat MS pindah ke RA baru (seperti Location Update di GSM)
- Update posisi ke SGSN
- Ada 3 tipe:
  - **RA update dalam SGSN sama**: cepat
  - **RA update antar SGSN**: perlu koordinasi SGSN lama dan baru
  - **Combined RA/LA update**: update GPRS dan GSM sekaligus

### 4. Cell Reselection
- MS pindah dari cell ke cell saat idle (tidak ada transfer data aktif)
- Berbeda dengan **handover** (saat ada call/data aktif)
- Berdasarkan **PBCCH** atau **BCCH** measurement

---

## GPRS States (Mobility Management)

MS bisa ada di 3 state:

### 1. IDLE State
- MS tidak attached ke GPRS
- Tidak bisa kirim/terima data
- Hemat baterai paling tinggi

### 2. STANDBY State
- MS attached tapi tidak ada data transfer aktif
- SGSN tahu MS ada di RA mana (tapi tidak tahu cell spesifik)
- MS lakukan cell reselection sendiri
- Kalau ada data masuk → SGSN lakukan **paging** ke semua cell dalam RA

### 3. READY State
- MS attached dan ada data transfer aktif (atau baru saja selesai)
- SGSN tahu MS ada di cell mana (granular)
- No paging needed, langsung kirim data
- Setelah idle beberapa detik → balik ke STANDBY

**Timer READY:**
- Default: 44 detik (tergantung operator)
- Selama 44 detik setelah data terakhir, MS masih di READY state
- Habis timer → pindah ke STANDBY

---

## Charging di GPRS/EDGE

### Prinsip Charging
- **Volume-based**: bayar per KB/MB/GB
- Berbeda dengan GSM (time-based: bayar per menit)
- SGSN dan GGSN catat volume data

### CDR (Call Data Record)
- **S-CDR (SGSN-CDR)**: catat di SGSN
- **G-CDR (GGSN-CDR)**: catat di GGSN
- Berisi:
  - IMSI, MSISDN
  - Volume UL dan DL
  - Duration
  - APN
  - QoS used
  - Timestamp

### Charging Gateway Function (CGF)
- Collect CDR dari SGSN dan GGSN
- Kirim ke billing system
- Untuk postpaid atau prepaid deduction

---

## KPI GPRS/EDGE

### Accessibility
- **PDP Context Activation Success Rate**: % PDP context berhasil aktivasi
- Target: > 95%

### Retainability
- **PDP Context Drop Rate**: % PDP context drop abnormal
- Target: < 3%

### Throughput
- **Average DL Throughput**: kecepatan download rata-rata
- **Average UL Throughput**: kecepatan upload rata-rata
- GPRS: 40-80 kbps
- EDGE: 100-200 kbps (real world)

### Latency
- **Round Trip Time (RTT)**: ping time
- GPRS: 500-800 ms
- EDGE: 300-600 ms
- Lebih tinggi dari 3G/4G (karena architecture dan CS limitation)

---

## Perbandingan Lengkap

| Aspek | GSM (2G) | GPRS (2.5G) | EDGE (2.75G) |
|-------|----------|-------------|--------------|
| **Tahun** | 1991 | 2000 | 2003 |
| **Layanan** | Voice, SMS, CSD | Voice, SMS, Packet Data | Voice, SMS, Packet Data |
| **Data Type** | Circuit Switched | Circuit + Packet Switched | Circuit + Packet Switched |
| **Modulasi** | GMSK | GMSK | GMSK + 8-PSK |
| **Max Speed** | 14.4 kbps (CSD) | 171.2 kbps | 473.6 kbps |
| **Real Speed** | 9.6-14.4 kbps | 40-80 kbps | 100-200 kbps |
| **Bits/symbol** | 1 | 1 | 1 (GMSK) atau 3 (8-PSK) |
| **Coding** | - | CS-1 to CS-4 | MCS-1 to MCS-9 |
| **Network Element** | MSC, HLR, VLR | MSC + **SGSN, GGSN, PCU** | Same as GPRS |
| **Charging** | Per menit | Per MB | Per MB |
| **Always-On** | ❌ (dial-up) | ✅ | ✅ |
| **Latency** | Low (voice) | 500-800 ms | 300-600 ms |
| **Upgrade needed** | - | Core + RAN | RAN only |

---

## Keterbatasan GPRS/EDGE

### 1. Kecepatan Terbatas
- Max real speed 200 kbps (EDGE) masih lambat untuk video atau download besar
- Latency tinggi (300-800 ms) → tidak cocok untuk gaming atau video call

### 2. Best Effort Service
- Tidak ada guaranteed bandwidth
- Banyak user share resource → congestion
- QoS terbatas

### 3. Berbagi Resource dengan GSM
- Voice (GSM) punya prioritas lebih tinggi
- Saat voice call masuk → data bisa drop atau slow
- Timeslot terbatas (max 8 per carrier)

### 4. Architecture Limitation
- Masih basis GSM → tidak didesain untuk heavy data
- Core network bottleneck
- Tidak seefisien 3G/4G

---

## Sunset GPRS/EDGE di Indonesia

### Timeline
- **2018-2020**: Operator mulai reduce coverage GPRS/EDGE
- **2021-2023**: Fokus refarming frekuensi untuk 4G LTE
- **2024-2025**: Beberapa operator sudah shutdown EDGE di kota besar
- **2026 (rencana)**: Complete shutdown 2G/2.5G/2.75G

### Alasan Shutdown
1. **Efisiensi spektrum**: 1 carrier GSM → refarming jadi LTE (10x lebih efisien)
2. **User migration**: 95%+ user sudah pakai 4G
3. **Maintenance cost**: maintain legacy network mahal
4. **Energy saving**: equipment lama boros listrik
5. **IoT**: NB-IoT dan LTE-M lebih cocok dari GPRS

### Alternatif
- **VoLTE**: voice over LTE (ganti voice GSM)
- **LTE**: data (ganti GPRS/EDGE)
- **NB-IoT**: untuk IoT device (ganti GPRS IoT)

---

## Fun Facts

- **WAP (Wireless Application Protocol)**: browsing mobile pertama, pakai GPRS. Sites punya versi .wap khusus!
- **Blackberry**: device pertama yang popularkan "always-on" GPRS (push email)
- **MMS**: dulu kirim MMS mahal banget (Rp 1000-3000 per MMS), sekarang gratis via WhatsApp
- **GPRS paket**: awal-awal ada paket 10 MB Rp 50.000 (mahal!), sekarang 10 GB cuma Rp 30.000
- **"Sinyal GPRS"**: icon "G" atau "E" di HP kamu (G=GPRS, E=EDGE)
- **Nokia 3310 (2000)**: tidak support GPRS. Nokia 6600 (2003): support GPRS dan EDGE

---

**Kesimpulan:**
- **GPRS (2.5G)**: upgrade GSM, tambah packet-switched data via SGSN-GGSN, max 171 kbps
- **EDGE (2.75G)**: upgrade GPRS, modulasi 8-PSK, max 473 kbps
- Teknologi bridge antara voice-only (2G) dan mobile internet modern (3G+)
- Always-on internet, charging per MB
- Sekarang sudah obsolete, digantikan 4G LTE
- Legacy network yang pernah bawa internet ke mobile phone! 🌐📱