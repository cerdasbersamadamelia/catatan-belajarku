# 1. Perkenalan Sistem Komunikasi Seluler

## Pengenalan Mata Kuliah

### Overview Materi

Mata kuliah ini fokus membahas **4G LTE** secara mendalam, dengan materi yang juga diajarkan di **S2**. Yes, materinya cukup berat tapi kita bisa!

**Target utama:** Hafal dan paham arsitektur 2G, 3G, dan 4G LTE.

---

## Implementasi 5G di Indonesia

### NSA vs SA

**Indonesia akan implementasi 5G dengan cara:**

- **NSA (Non-Standalone)** dulu
- Core network: masih 4G
- Radio access: sudah 5G
- Nama: **5G Radio** atau **5G NR (New Radio)**

**SA (Standalone):**

- Core dan radio: semua 5G
- Implementasi belakangan

**Kesimpulan:** 4G masih akan dipakai lama ke depan!

---

## Roadmap Pembelajaran

### 4 Bagian Besar

#### 1. Introduction

- Review 2G (GSM)
- Review 3G (UMTS/WCDMA)
- Introduction to LTE
- CSFB Architecture
- VoLTE (Voice over LTE)

#### 2. Network Architecture & Protocol

- Arsitektur 4G LTE secara keseluruhan
- Protocol stack detail
- Control plane vs User plane
- Diameter protocol

#### 3. RF Planning & Network Planning

**RF Planning:**

- OFDM (Orthogonal Frequency Division Multiplexing)
- SC-FDMA (Single Carrier FDMA)
- Coverage & capacity planning
- Link budget calculation

**Core Network Planning:**

- MME dimensioning
- SGW/PGW dimensioning
- Transport planning
- Contoh kasus nyata

#### 4. Design & Optimization

**Tools yang dipakai:**

- **Manual calculation** (Excel)
- **Atoll software** (RF planning tool)
- Perhitungan pakai rumus **Huawei**

**Project akhir:**

- Design jaringan LTE lengkap
- Hitung jumlah user, site, bandwidth
- Dimensioning S1, MME, SGW, dll

---

## Teknologi Open RAN

### Untuk Daerah 3T

**3T = Terluar, Terbelakang, Tertinggal**

**Open RAN:**

- Bisa untuk 2G, 3G, 4G
- Solusi untuk daerah terpencil
- Pemerintah targetkan semua wilayah tercover
- Teknologi yang akan banyak dipakai: **4G**

**Kenapa 4G, bukan 5G?**

- 5G butuh bandwidth **60 MHz** minimum
- 4G cukup **20 MHz**
- Lebih ekonomis untuk coverage luas

---

## Review: Arsitektur 2G (GSM)

### Komponen Utama

**User Equipment:**

- **MS (Mobile Station)** - handset user

**Radio Access Network:**

- **BTS (Base Transceiver Station)** - pemancar
- **BSC (Base Station Controller)** - controller BTS

**Core Network:**

**Untuk Voice (CS Domain):**

- **MSC (Mobile Switching Center)**
- **HLR (Home Location Register)**
- **VLR (Visitor Location Register)**

**GSM = Circuit Switching only** (voice only)

---

## Review: Arsitektur 2.5G (GPRS)

### Penambahan Kemampuan Data

**Perangkat baru ditambahkan:**

- **SGSN (Serving GPRS Support Node)**
- **GGSN (Gateway GPRS Support Node)**

**Fungsi:**

- SGSN: untuk signaling data
- GGSN: untuk user data

**Karakteristik:**

- Masih add-on dari GSM
- Hardware ditambah (SGSN & GGSN)
- User harus ganti handset!

---

## Review: Arsitektur 2.75G (EDGE)

### Upgrade Software Only

**Yang berubah:**

- **Software upgrade** di BTS dan BSC
- Core network (SGSN & GGSN): TIDAK berubah

**Kenapa lebih cepat?**

- Modulasi berubah
- Processing lebih efisien

**User:** Harus ganti handset lagi!

---

## Review: Arsitektur 3G (UMTS/WCDMA)

### Perubahan Total

**User Equipment:**

- **UE (User Equipment)** - perangkat user

**Radio Access Network:**

- BTS → **NodeB**
- BSC → **RNC (Radio Network Controller)**
- Interface: **Iub** (NodeB ↔ RNC)

**Core Network:**

**2 domain sekaligus:**

1. **CS Domain (Circuit Switched)** - untuk voice
   - MSC
   - HLR/VLR
2. **PS Domain (Packet Switched)** - untuk data
   - SGSN
   - GGSN

**Teknologi:**

- **WCDMA** (Wideband CDMA)
- Bisa handle voice DAN data

---

## Review: Arsitektur 3.5G (HSPA)

### HSPA = High-Speed Packet Access

**Perbedaan dengan 3G:**

| Aspek      | 3G   | HSPA           |
| ---------- | ---- | -------------- |
| Arsitektur | Sama | **Sama**       |
| Hardware   | -    | **No upgrade** |
| Software   | -    | **Upgrade** ✓  |

**Yang berubah:** SOFTWARE ONLY!

**Software baru:**

- **MAC-hs (Medium Access Control - high speed)**
- Ditambahkan di NodeB dan RNC

### 3 Fitur Utama HSPA

#### 1. AMC (Adaptive Modulation and Coding)

- Menyesuaikan modulasi berdasarkan kondisi
- Bisa pilih antara QPSK atau 16QAM

#### 2. Fast Scheduling

- Penjadwalan paket lebih cepat
- Proses di NodeB, tidak di RNC lagi
- Lebih efisien!

#### 3. Fast Retransmission

- Retransmisi lebih cepat
- Scheduling dan retransmission di NodeB

**Impact:**

- Kecepatan meningkat drastis
- Delay berkurang
- Throughput naik

---

## Perbedaan Teknologi Seluler

### Tabel Perbandingan Lengkap

| Generasi | Teknologi | Hardware Upgrade | Software Upgrade | User Ganti HP |
| -------- | --------- | ---------------- | ---------------- | ------------- |
| 2G       | GSM       | -                | -                | -             |
| 2.5G     | GPRS      | ✓ (+ SGSN/GGSN)  | ✓                | ✓             |
| 2.75G    | EDGE      | ✗                | ✓ (BTS/BSC)      | ✓             |
| 3G       | UMTS      | ✓ (Total)        | ✓                | ✓             |
| 3.5G     | HSPA      | ✗                | ✓ (MAC-hs)       | ✓             |
| 4G       | LTE       | ✓ (Total)        | ✓                | ✓             |

### Kunci Perbedaan

**2G → 2.5G (GPRS):**

- Hardware: tambah SGSN & GGSN
- Kemampuan: voice + data
- Handset: harus ganti

**2.5G → 2.75G (EDGE):**

- Hardware core: TIDAK berubah
- Software: upgrade di BTS/BSC
- Modulasi: lebih baik
- Handset: harus ganti

**3G → 3.5G (HSPA):**

- Hardware: TIDAK berubah
- Software: upgrade MAC-hs
- Processing: pindah ke NodeB
- Handset: harus ganti

**Prinsip penting:**

> Setiap ada perubahan teknologi = user harus ganti handset!

---

## Arsitektur 4G (LTE)

### Komponen Utama

**User Equipment:**

- **UE (User Equipment)** - perangkat user

**Access Network (E-UTRAN):**

- **eNodeB** (evolved NodeB)
- Fungsi NodeB + RNC jadi satu!
- Lebih simpel

**Core Network (EPC):**

**Signaling:**

- **MME (Mobility Management Entity)**

**Data:**

- **SGW (Serving Gateway)**
- **PGW (PDN Gateway)**

**Database:**

- **HSS (Home Subscriber Server)**
- **PCRF (Policy and Charging Rules Function)**

### Karakteristik 4G

**All-IP Network:**

- 100% Packet Switching
- Tidak ada Circuit Switching
- Hanya untuk **data**

**Masalah:** Voice gimana?

---

## Voice di 4G

### 2 Solusi

#### 1. CSFB (Circuit Switched Fallback)

**Konsep:**

- Voice "turun" ke 2G/3G
- Data tetap di 4G

**Syarat:**

- Operator harus punya existing 2G/3G

**Cara kerja:**

- User di 4G
- Mau telpon → fallback ke 3G
- Selesai telpon → balik ke 4G

#### 2. VoLTE (Voice over LTE)

**Konsep:**

- Voice dijadikan data (VoIP)
- Tetap di 4G

**Syarat:**

- Butuh **IMS (IP Multimedia Subsystem)**
- Investasi besar

**Status:**

- Indonesia: kebanyakan pakai CSFB
- VoLTE: mulai diimplementasi

---

## NFV (Network Function Virtualization)

### Konsep Virtualisasi

**Kenapa harus hafal arsitektur?**

Karena ke depan semua perangkat akan **divirtualisasi**!

**Yang divirtualisasi:**

- SGSN → virtual SGSN
- GGSN → virtual GGSN
- MME → virtual MME
- SGW → virtual SGW
- PGW → virtual PGW

**Teknologi:**

- **NFV** (Network Function Virtualization) - untuk core network
- **SDN** (Software Defined Network) - untuk management

**Konfigurasi:**

- Perlu tahu interface (Iub, S1, X2, dll)
- Virtual MME ↔ virtual SGW tetap pakai S11
- Standarisasi tetap mengacu 3GPP

**Contoh:**

- Virtual eNodeB connect ke virtual MME pakai S1-MME
- Virtual MME connect ke virtual SGW pakai S11
- Semuanya software-based!

---

## Penilaian Mata Kuliah

### Komponen Nilai

**1. Mid Semester**

- Teori
- Perhitungan

**2. Tugas**

- Tugas mingguan/PR
- Review materi

**3. Tugas Besar**

- Design jaringan LTE
- Pakai Excel (manual calculation)
- Pakai Atoll (RF planning tool)

**4. Ujian Akhir**

- Teori
- Perhitungan
- Studi kasus

### Project Akhir

**Requirements:**

- Design complete LTE network
- Hitung:
  - Jumlah user
  - Jumlah site
  - Bandwidth requirement
  - MME capacity
  - SGW capacity
  - Link budget
- Buat di Excel DAN Atoll

---

## Perbedaan Manual vs Atoll

### Manual Calculation (Excel)

**Kelebihan:**

- Bisa hitung jumlah user
- Bisa hitung dimensioning
- Paham rumus detail
- Fleksibel

**Kekurangan:**

- Tidak ada visualisasi coverage
- Tidak tahu area mana yang bagus/jelek
- Tidak ada radio propagation model

### Atoll Software

**Kelebihan:**

- Ada visualisasi coverage map
- Bisa lihat RSRP, SINR, throughput
- Ada radio propagation model
- Profesional tool

**Kekurangan:**

- Tidak bisa hitung user dimensioning
- Fokus ke RF planning only

**Solusi:** Pakai keduanya!

- Excel: untuk dimensioning & capacity
- Atoll: untuk coverage & optimization

---

## Standarisasi Perhitungan

### Rumus dari Huawei

**Kenapa pakai Huawei?**

- Setiap vendor punya rumus berbeda
- Huawei paling umum di Indonesia
- White paper lengkap
- Standar industri

**Yang dihitung:**

- Link budget
- Coverage area
- Capacity requirement
- Site count
- Bandwidth allocation
- Core dimensioning (MME, SGW, PGW)
- Transport planning

---

## Tips Belajar

### Konsep Kunci

**Yang HARUS dihafal:**

1. **Arsitektur setiap generasi**

   - 2G: MS-BTS-BSC-MSC
   - 2.5G: + SGSN/GGSN
   - 2.75G: sama, upgrade software
   - 3G: UE-NodeB-RNC-SGSN/GGSN/MSC
   - 3.5G: sama, tambah MAC-hs
   - 4G: UE-eNodeB-MME/SGW/PGW

2. **Interface**

   - 2G: Abis (BTS-BSC)
   - 3G: Iub (NodeB-RNC)
   - 4G: S1 (eNodeB-MME/SGW), X2 (eNodeB-eNodeB)

3. **Perbedaan antar generasi**
   - Hardware upgrade atau tidak?
   - Software upgrade atau tidak?
   - Apa yang berubah?

### Untuk Interview

**Pertanyaan klasik:**

> "Apa bedanya 3G dengan HSPA?"

**Jawaban:**

- Arsitektur: sama
- Hardware: tidak ada upgrade
- Software: upgrade (MAC-hs)
- Fitur baru: AMC, fast scheduling, fast retransmission
- Impact: kecepatan naik, delay turun

**Pertanyaan klasik 2:**

> "Apa bedanya 2G dengan 3G?"

**Jawaban:**

- Arsitektur: total overhaul
- Access: BTS/BSC → NodeB/RNC
- Core: CS only → CS + PS
- Teknologi: GSM → WCDMA
- Kemampuan: voice only → voice + data (integrated)

---

## Tugas & PR

**Review soal:**

1. Perbedaan 2G, 2.5G, 2.75G
2. Perbedaan 3G dan HSPA
3. Data flow di 4G LTE (bonus!)

**Tujuan:**

- Review sebelum masuk materi LTE
- Pastikan konsep dasar sudah paham
- Persiapan untuk materi berat

---

## Refarming

### Penataan Ulang Frekuensi

**Konsep:**

- Mengatur ulang alokasi frekuensi
- Agar lebih efisien
- Mengurangi guard band

**Contoh:**

- Frekuensi 2100 MHz (3G) → dialihkan ke 4G
- Frekuensi 900 MHz (2G) → dialihkan ke 4G
- Untuk 5G: akan ada refarming lagi!

**Kenapa penting?**

- Bandwidth terbatas
- Harus dioptimalkan
- Hindari interferensi

---

## Kesimpulan

### Point Penting

**Materi ini berat tapi penting karena:**

1. Materi industri (real-world)
2. Bisa jadi portfolio
3. Nilai plus saat interview
4. Fundamental untuk karir telco

**Yang harus dikuasai:**

1. Arsitektur 2G-3G-4G
2. Perbedaan setiap generasi
3. Interface dan protokol
4. Planning & dimensioning
5. Tools (Excel & Atoll)

**Mindset:**

> Ini bukan cuma hafalan, tapi pemahaman konsep yang bisa dijelaskan kapan saja!

**Target akhir:**

- Bisa design jaringan LTE lengkap
- Paham dari user sampai core
- Bisa hitung capacity & coverage
- Siap kerja di industri telco

---

## FAQ

**Q: Kenapa 4G masih penting padahal ada 5G?**

- 5G butuh bandwidth besar (60 MHz)
- 4G cukup 20 MHz
- Coverage 4G masih expand
- Indonesia pakai NSA (core 4G)
- 4G masih dipakai bertahun-tahun ke depan

**Q: Apa bedanya CSFB dan VoLTE?**

- CSFB: voice turun ke 2G/3G (butuh existing network)
- VoLTE: voice jadi data (butuh IMS, mahal)

**Q: Kenapa harus hafal interface?**

- Untuk konfigurasi jaringan
- Untuk virtualisasi (NFV/SDN)
- Standarisasi 3GPP
- Real-world implementation

**Q: Apa gunanya Atoll kalau ada Excel?**

- Atoll: visualisasi coverage, radio planning
- Excel: dimensioning, capacity planning
- Keduanya diperlukan untuk complete design

---

## Glossary

- **AMC**: Adaptive Modulation and Coding
- **BSC**: Base Station Controller
- **BTS**: Base Transceiver Station
- **CSFB**: Circuit Switched Fallback
- **EDGE**: Enhanced Data rates for GSM Evolution
- **eNodeB**: evolved NodeB
- **EPC**: Evolved Packet Core
- **GGSN**: Gateway GPRS Support Node
- **GPRS**: General Packet Radio Service
- **HSPA**: High-Speed Packet Access
- **HS-DSCH**: High-Speed Downlink Shared Channel
- **HSS**: Home Subscriber Server
- **IMS**: IP Multimedia Subsystem
- **MME**: Mobility Management Entity
- **MS**: Mobile Station
- **MSC**: Mobile Switching Center
- **NFV**: Network Function Virtualization
- **NodeB**: Node B (3G base station)
- **NSA**: Non-Standalone
- **OFDM**: Orthogonal Frequency Division Multiplexing
- **PCRF**: Policy and Charging Rules Function
- **PGW**: PDN Gateway
- **RNC**: Radio Network Controller
- **SA**: Standalone
- **SC-FDMA**: Single Carrier FDMA
- **SDN**: Software Defined Network
- **SGSN**: Serving GPRS Support Node
- **SGW**: Serving Gateway
- **UE**: User Equipment
- **VoLTE**: Voice over LTE
- **WCDMA**: Wideband Code Division Multiple Access
