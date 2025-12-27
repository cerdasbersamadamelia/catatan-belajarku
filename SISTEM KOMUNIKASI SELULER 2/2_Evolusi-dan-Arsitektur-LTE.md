# 2. Introduction to LTE

## Standarisasi Telekomunikasi Seluler

### ITU (International Telecommunication Union)

**ITU punya 3 sektor:**

1. **ITU-R** (Radio Communication) - penata frekuensi dan komunikasi radio
2. **ITU-T** (Telecommunication) - standar dan rekomendasi
3. **ITU-D** (Development) - pengembangan

**ITU menunjuk 2 badan untuk standarisasi seluler:**

- **3GPP** (3rd Generation Partnership Project)
- **3GPP2**

---

## 3GPP vs 3GPP2

### 3GPP

**Kolaborasi global:**

- Vendor dan operator dari seluruh dunia
- ARIB (Jepang), ATIS (Amerika Serikat), CCSA (China), ETSI (Eropa), TSDSI (India), TTA (Korea), TTC (Jepang)

**Project 3GPP:**

- **IMT-2000** (tahun 2000) → 3G
- **IMT-2020** → 5G

**Teknologi yang dikembangkan:**

- GSM → GPRS → EDGE → 3G → HSPA → 4G LTE → 5G

### 3GPP2

**Project berbeda:**

- CDMA2000
- CDMA EV-DO (Evolution Data Optimized)
- WiMAX (untuk 4G)

**Perbedaan 4G:**
| 3GPP | 3GPP2 |
|------|-------|
| 4G LTE | 4G WiMAX |
| Berbeda sejarah | Berbeda teknologi |

---

## Implementasi di Indonesia

### Operator Indonesia

**Operator dengan 2G/3G/4G:**

- Telkomsel
- Indosat (XL Axiata)
- XL
- Tri (3)

**Operator khusus 4G:**

- **Smartfren** - tidak punya 3G, langsung 4G LTE
  - Awalnya mau pakai WiMAX
  - Lisensi frekuensi WiMAX tidak turun
  - Akhirnya beralih ke 4G LTE

### Voice di 4G

**Masalah:**

- 4G = all-IP, tidak support voice secara default
- Circuit Switching (CS) tidak ada

**Solusi voice:**

1. **CSFB (Circuit Switched Fallback)**
   - Untuk operator yang punya existing 2G/3G
   - Voice turun ke 2G/3G
   - Data tetap di 4G
2. **VoLTE (Voice over LTE)**
   - Operator tanpa 2G/3G
   - Butuh **IMS (IP Multimedia Subsystem)**
   - Voice dijadikan data (VoIP)
   - Biaya implementasi tinggi

---

## Evolusi Teknologi: 2G (GSM)

### Arsitektur GSM

**Radio Access Network:**

- **MS (Mobile Station)** - handset user
- **BTS (Base Transceiver Station)** - pemancar
- **BSC (Base Station Controller)** - controller BTS
- Interface: **Abis** (BTS ↔ BSC)

**Core Network:**

- **MSC (Mobile Switching Center)** - switching utama
- **HLR (Home Location Register)** - database permanen
- **VLR (Visitor Location Register)** - database temporary
- **AUC (Authentication Center)** - otentikasi
- **EIR (Equipment Identity Register)** - database IMEI

### Nomor Identitas User

**Setiap pelanggan punya 2 nomor:**

1. **MSISDN** (nomor HP)

   - Contoh: 0812-xxxx-xxxx
   - Nomor yang kita kenal

2. **IMEI** (nomor handset)
   - Disimpan di EIR
   - Harus terdaftar (jika tidak = blackmarket)
   - Tetap meskipun ganti operator

**Tips:**

- Ganti nomor tapi masih terdeteksi? Ganti handset juga!
- Handset lama mudah disadap
- Handset baru (data-enabled) lebih susah disadap

### Teknologi GSM

**Multiplexing:** TDMA (Time Division Multiple Access)

**Channel:**

- **Physical Channel** - carrier untuk informasi
- **Logical Channel** - untuk signaling/kontrol

**Prinsip penting:**

> Sejak GSM, signaling dan data SELALU terpisah!

**Link:**

- **Uplink** - user ke BTS
- **Downlink** - BTS ke user

### Frekuensi GSM

**Default di Indonesia:**

- **900 MHz** - frekuensi utama GSM

**Perkembangan:**

- 2G sudah mulai tidak dipakai di area kebutuhan data tinggi
- Frekuensi 900 MHz dialihkan untuk data
- Disebut **L900** (LTE 900 MHz)

---

## Evolusi: 2.5G (GPRS)

### Perubahan dari GSM

**Penambahan perangkat baru:**

- **SGSN** (Serving GPRS Support Node) - fungsi signaling
- **GGSN** (Gateway GPRS Support Node) - fungsi data

**Fungsi:**

- Menambahkan kemampuan **data** (MMS, kirim gambar, video)
- Bukan lagi voice only

### GPRS = CSD (Circuit Switched Data)

**Bukan PS (Packet Switching)!**

**Kenapa?**

- Masih pakai **SS7** (Signaling System 7) protocol
- Belum ada IP layer di OSI
- Jaringan terpisah dari GSM (add-on)

**Karakteristik:**

- Modulasi: masih **GMSK** (sama seperti GSM)
- Arsitektur berubah (tambah SGSN & GGSN)
- Interface baru: **Gb** (BSC ↔ SGSN), **Gn** (SGSN ↔ GGSN)

### Perubahan Handset

**User harus ganti HP!**

- MS → handset dengan fitur GPRS
- Support MMS dan data

---

## Evolusi: 2.75G (EDGE)

### Perubahan dari GPRS

**Core Network:** TIDAK ada perubahan

- SGSN & GGSN tetap sama
- No upgrade

**Radio Access Network:** UPGRADE software

- Software baru di **BTS** dan **BSC**
- Terutama di **RRU (Radio Remote Unit)** dan **BBU (Baseband Unit)**

### Teknologi EDGE

**Modulasi berubah:**

- GSM: **GMSK** (1 bit per symbol)
- GPRS: **GMSK** (1 bit per symbol)
- EDGE: **8PSK** (3 bit per symbol) ✓

**Pengkodean:**

- 1 symbol = 3 bit
- Throughput lebih besar
- Kecepatan meningkat

**Kecepatan:**

- GPRS: ~57 Kbps
- EDGE: ~114-346 Kbps

### Perubahan Handset

**User harus ganti HP lagi!**

- Handset harus support EDGE
- Support 8PSK modulation

---

## Evolusi: 3G (UMTS/WCDMA)

### Perubahan Besar

**Semua berubah total!**

**Radio Access Network:**

- MS → **UE**
- BTS → **NodeB**
- BSC → **RNC (Radio Network Controller)**
- **UTRAN** (UMTS Terrestrial Radio Access Network)
- Interface: **Iub** (NodeB ↔ RNC)

**Core Network:**

- **2 jaringan dalam 1 sistem:**
  - **CS Domain** (voice) → MSC
  - **PS Domain** (data) → SGSN & GGSN

**Karakteristik:**

- NodeB bisa handle voice DAN data
- Terintegrasi dalam 1 sistem
- Bukan add-on seperti GPRS

### Teknologi 3G

**Multiplexing:** WCDMA (Wideband CDMA)

**Modulasi:** QPSK (Quadrature Phase Shift Keying)

- 2 bit per symbol

**Frekuensi:** 2100 MHz (2.1 GHz)

**Kecepatan bervariasi:**
| Area | Kecepatan | Keterangan |
|------|-----------|------------|
| Rural | 144 Kbps | Area terpencil |
| Urban | 384 Kbps | Perkotaan |
| Indoor | 2 Mbps | Dalam ruangan |

**Q: Kenapa indoor lebih cepat?**

**A:** Jarak lebih dekat, sinyal lebih kuat, kondisi kanal lebih baik

### 3G Shutdown

**Status sekarang:**

- Beberapa wilayah masih ada 3G
- Proses **shutdown** (decommissioning) sedang berjalan
- Frekuensi 2100 MHz dialihkan untuk 4G
- Disebut **L2100** (LTE 2100 MHz)

### Hardware di Site

**RRU (Radio Remote Unit):**

- 1 unit bisa isi 3 teknologi sekaligus
- Rak 1: 4G
- Rak 2: 3G
- Rak 3: 2G

**Proses shutdown:**

- Copot rak 3G
- Ganti dengan rak 4G baru
- Setting frekuensi (misalnya L2100)

**Konfigurasi:**

- Bisa di lokasi langsung
- Bisa remote dari NOC/MSC Office

**Waktu kerja:**

- Upgrade biasanya **malam hari**
- Tidak bisa siang (mengganggu service)
- Engineer kerja shift malam

---

## Evolusi: 3.5G (HSPA)

### HSDPA (High-Speed Downlink Packet Access)

**Arsitektur:** TIDAK berubah

- Sama dengan 3G
- No upgrade hardware

**Yang berubah:** SOFTWARE upgrade

**Software baru:**

- **MAC-hs (Medium Access Control - high speed)**
- Ditambahkan di NodeB dan RNC

### 3 Fitur Utama HSDPA

#### 1. AMC (Adaptive Modulation and Coding)

**Konsep:** Adaptif = menyesuaikan

**2 modulasi di HSPA:**

- **QPSK** (2 bit/symbol)
- **16QAM** (4 bit/symbol)

**HS-DSCH bisa memilih kapan pakai QPSK atau 16QAM**

**Pemilihan berdasarkan:**

| Kondisi         | Modulasi | Alasan                                 |
| --------------- | -------- | -------------------------------------- |
| Dekat NodeB     | 16QAM    | Sinyal kuat, throughput tinggi         |
| Jauh dari NodeB | QPSK     | Sinyal lemah, lebih tahan interferensi |

**Kenapa 16QAM lebih bagus?**

- 4 bit/symbol (lebih banyak)
- Throughput lebih tinggi
- Tapi tidak tahan noise
- Hanya untuk user dengan sinyal bagus

**Kenapa QPSK untuk jarak jauh?**

- 2 bit/symbol (lebih sedikit)
- Lebih tahan terhadap noise dan interferensi
- Cocok untuk sinyal lemah

#### 2. Fast Scheduling

**Penjadwalan transmisi paket data**

**3 dasar scheduling:**

1. **Kualitas channel**
   - Bagaimana kondisi sinyal
2. **Kapabilitas user**

   - Kemampuan handset
   - iPhone vs Android vs Nokia
   - Beda merek = beda kemampuan

3. **QoS (Quality of Service)**
   - Kelas layanan
   - User VIP vs user biasa
   - Paket premium vs paket reguler

**Algoritma: Proportional Fair (PF)**

- Gabungan Round Robin + Max C/I
- Melihat kualitas channel, user capability, dan QoS
- Memilih user mana yang diprioritaskan

**Cell Selection Process:**

**Packing/Camping:**

- User terus-menerus scanning BTS
- Mencari sinyal terbaik
- BTS juga survey user

**Tips sinyal jelek:**

1. ❌ Jalan-jalan cari tempat
2. ✅ **Matikan HP, nyalakan lagi**
   - Paksa handset melakukan packing ulang
   - Cari BTS dengan sinyal terbaik

#### 3. Fast Retransmission

**Proses handover lebih cepat**

**Fast Cell Selection:**

- Handover tanpa delay
- Tanpa menyebabkan call drop
- Menggunakan **hard handover** (seperti 2G)
- Tapi lebih cepat dari hard handover biasa

### Handover: Soft vs Hard

#### Soft Handover (3G)

**Karakteristik:**

- User dipertahankan di cell lama
- Sambil connect ke cell baru
- Baru lepas kalau sudah yakin cell baru OK

**Keuntungan:**

- Aman dari blank spot
- Tidak drop call
- Smooth transition

**Kerugian:**

- Lebih lambat

**Ilustrasi:**

```
Cell A ----[overlap]---- Cell B
        User tetap di A sambil connect B
        Baru lepas A setelah B kuat
```

#### Hard Handover (2G, EDGE)

**Karakteristik:**

- Putus dari cell lama
- Langsung connect cell baru

**Keuntungan:**

- Lebih cepat

**Kerugian:**

- Risiko **blank spot**
- Area tanpa sinyal 2 cell
- Atau 2 sinyal sama kuat

**Ilustrasi:**

```
Cell A ----X---- Cell B
        Putus A → Connect B
        Risiko blank spot di tengah
```

#### Fast Cell Selection (HSPA)

**Konsep:**

- Menggunakan hard handover
- Tapi dipercepat dengan mengurangi delay
- Tanpa de-allocation kanal
- Mencegah blank spot

**Cara kerja:**

- Pilih berdasarkan **SIR (Signal to Interference Ratio)**
- Cell dengan SIR tertinggi dipilih
- Proses sangat cepat

### Perubahan Kecepatan & Parameter

**Kecepatan:**

- 3G: 2 Mbps
- HSPA: 10 Mbps (5x lebih cepat!)

**Modulasi:**

- 3G: QPSK only
- HSPA: QPSK + 16QAM

**TTI (Transmission Time Interval):**

- 3G: 10 ms
- HSPA: 2 ms (5x lebih cepat!)

**Impact:**

- Bit rate meningkat
- Delay berkurang
- Throughput naik

**Fitur tambahan:**

- Simultaneous support UMTS & HSPA
- Adaptive modulation
- Fast retransmission
- Fast scheduling

### Internal & External Handover

**Internal Handover:**

- Dalam 1 RNC
- Lebih cepat

**External Handover:**

- Antar RNC berbeda
- Lebih lambat

---

## Evolusi: 4G (LTE)

### Karakteristik Utama

**All-IP Network:**

- 100% Packet Switching
- Tidak ada Circuit Switching
- Semua berbasis IP

**Tujuan:**

- **Always on, anywhere, anytime**
- Dimana saja, kapan saja, dari mana saja

### Kelebihan 4G

**Teknologi:**

- High throughput
- Low latency
- Flat architecture (lebih simpel)
- Lower operating cost
- Simple interworking dengan 3G
- All-IP network

**User:**

- Kecepatan data tinggi
- Response cepat
- Sharing lebih mudah

### Arsitektur 4G (EPS)

#### E-UTRAN (Radio Access Network)

**Hanya 1 perangkat:**

- **eNodeB** (evolved NodeB)
- NodeB + RNC jadi satu!
- Tidak perlu controller terpisah

**Keunggulan:**

- eNodeB bisa komunikasi langsung antar eNodeB
- Interface: **X2** (antar eNodeB)
- Lebih cepat, lebih efisien

#### EPC (Core Network)

**Perangkat utama:**

**Signaling (Control Plane):**

- **MME (Mobility Management Entity)**
- Hanya untuk signaling
- TIDAK membawa data
- Interface ke eNodeB: **S1-MME**

**Data (User Plane):**

- **SGW (Serving Gateway)**
- **PGW (PDN Gateway)**
- Hanya membawa data
- Interface ke eNodeB: **S1-U**

**Database:**

- **HSS (Home Subscriber Server)** - seperti HLR
- **PCRF (Policy and Charging Rules Function)** - billing/charging

**Prinsip penting:**

> Signaling dan data tetap terpisah!
> MME = signaling only, SGW/PGW = data only

### Perbandingan 3G vs 4G

| Aspek                 | 3G              | 4G             |
| --------------------- | --------------- | -------------- |
| Access                | NodeB + RNC     | eNodeB saja    |
| Komunikasi antar node | Harus lewat RNC | Langsung (X2)  |
| Core                  | SGSN + GGSN     | SGW + PGW      |
| Signaling             | Di core         | MME terpisah   |
| Voice                 | Native support  | Butuh IMS/CSFB |

### Interface Penting 4G

| Interface | Koneksi         | Fungsi         |
| --------- | --------------- | -------------- |
| X2        | eNodeB ↔ eNodeB | Antar eNodeB   |
| S1-MME    | eNodeB ↔ MME    | Signaling      |
| S1-U      | eNodeB ↔ SGW    | Data           |
| S11       | MME ↔ SGW       | Control        |
| S5/S8     | SGW ↔ PGW       | Data           |
| S6a       | MME ↔ HSS       | Authentication |

### EPS (Evolved Packet System)

**Komponen:**

- E-UTRAN (access) + EPC (core) = **EPS**

**Karakteristik:**

- All-IP connectivity
- Tidak ada non-IP
- Tidak ada CS domain
- Hanya PS domain

### Teknologi Modulasi 4G

**Modulasi tersedia:**

- QPSK (2 bit/symbol)
- 16QAM (4 bit/symbol)
- 64QAM (6 bit/symbol)
- 256QAM (8 bit/symbol)

**Semakin tinggi QAM = semakin banyak bit per symbol**

---

## Frekuensi 4G di Indonesia

### Default Frekuensi

| Teknologi | Frekuensi Default |
| --------- | ----------------- |
| 2G (GSM)  | 900 MHz           |
| 3G (UMTS) | 2100 MHz          |
| 4G (LTE)  | 1800 MHz (FDD)    |
| 4G (LTE)  | 2300 MHz (TDD)    |

### Refarming

**Konsep:** Penataan ulang frekuensi

**Kenapa perlu?**

- Operator dapat frekuensi terpisah-pisah
- Ada guard band (pembatas) untuk cegah interferensi
- Tidak efisien

**Sebelum refarming:**

```
[Telkomsel][guard][Indosat][guard][XL][guard][Telkomsel][guard]...
```

Berantakan dan boros!

**Setelah refarming:**

```
[Telkomsel 20 MHz][guard][Indosat 20 MHz][guard][XL 20 MHz]
```

Rapi dan efisien!

**Proses:**

- Kumpulkan frekuensi per operator
- Jadikan 1 blok besar (min 20 MHz)
- Sisakan guard band antar operator
- 90% bisa dipakai (18 MHz dari 20 MHz)
- 10% untuk guard band

### Bandwidth Operator (1800 MHz)

**Telkomsel:**

- 25 MHz di 1800 MHz
- 30 MHz di 2300 MHz
- Total: 55 MHz

**Indosat & XL:**

- ~10-20 MHz masing-masing

**Catatan:**

- Minimum 4G: 20 MHz
- Minimum 5G: 60 MHz (ideal 100 MHz)
- Telkomsel paling siap untuk 5G

### L900 dan L2100

**L900:**

- LTE di frekuensi 900 MHz
- Menggunakan frekuensi GSM yang sudah tidak terpakai
- Coverage lebih luas

**L2100:**

- LTE di frekuensi 2100 MHz
- Menggunakan frekuensi 3G yang di-shutdown
- Kapasitas data lebih besar

---

## Tips Praktis

### Troubleshooting Sinyal

**Sinyal lemah/hilang:**

1. ✅ Matikan HP
2. ✅ Nyalakan kembali
3. ✅ Paksa handset melakukan cell reselection

### Career Tips

**PKL/Magang:**

- Siap kerja malam (upgrade di malam hari)
- Engineer sering berangkat 20:30, pulang 02:00
- Normal untuk industri telekomunikasi

**Portofolio:**

- Pahami arsitektur jaringan
- Hafal interface (X2, S1, S11, dll)
- Bisa jadi nilai plus saat interview
- Bedakan diri dengan fresh graduate lain

---

## Kesimpulan Evolusi

**Timeline:**

```
2G (GSM) → 2.5G (GPRS) → 2.75G (EDGE) → 3G (UMTS) → 3.5G (HSPA) → 4G (LTE) → 5G
```

**Perubahan utama:**

| Generasi     | Perubahan                              |
| ------------ | -------------------------------------- |
| 2G → 2.5G    | Tambah SGSN/GGSN (data)                |
| 2.5G → 2.75G | Modulasi (GMSK → 8PSK)                 |
| 2.75G → 3G   | Total overhaul (NodeB, RNC, PS domain) |
| 3G → 3.5G    | Software upgrade (HSDPA)               |
| 3.5G → 4G    | Flat architecture (eNodeB, all-IP)     |

**Yang harus diingat:**

- Signaling dan data SELALU terpisah (sejak 2G)
- Setiap perubahan teknologi = ganti handset
- Core network lebih stabil dari radio network
- Radio network sering upgrade

---

## Glossary

- **AMC**: Adaptive Modulation and Coding
- **BSC**: Base Station Controller
- **BTS**: Base Transceiver Station
- **CSFB**: Circuit Switched Fallback
- **EDGE**: Enhanced Data rates for GSM Evolution
- **eNodeB**: evolved NodeB
- **EPC**: Evolved Packet Core
- **EPS**: Evolved Packet System
- **E-UTRAN**: Evolved UTRAN
- **GGSN**: Gateway GPRS Support Node
- **GMSK**: Gaussian Minimum Shift Keying
- **GPRS**: General Packet Radio Service
- **HSS**: Home Subscriber Server
- **HSDPA**: High-Speed Downlink Packet Access
- **IMEI**: International Mobile Equipment Identity
- **IMS**: IP Multimedia Subsystem
- **LTE**: Long Term Evolution
- **MME**: Mobility Management Entity
- **MS**: Mobile Station
- **MSISDN**: Mobile Station ISDN Number
- **NodeB**: Node B (3G base station)
- **PGW**: PDN Gateway
- **PCRF**: Policy and Charging Rules Function
- **PSK**: Phase Shift Keying
- **QAM**: Quadrature Amplitude Modulation
- **QPSK**: Quadrature Phase Shift Keying
- **RNC**: Radio Network Controller
- **RRU**: Radio Remote Unit
- **SGSN**: Serving GPRS Support Node
- **SGW**: Serving Gateway
- **SIR**: Signal to Interference Ratio
- **TTI**: Transmission Time Interval
- **UMTS**: Universal Mobile Telecommunications System
- **UTRAN**: UMTS Terrestrial Radio Access Network
- **VoLTE**: Voice over LTE
- **WCDMA**: Wideband Code Division Multiple Access
