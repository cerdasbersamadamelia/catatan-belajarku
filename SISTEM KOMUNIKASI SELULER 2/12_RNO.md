# 12. RNO - Radio Network Optimization untuk 4G LTE

## 1. Pengantar

**RNO** = Radio Network Optimization

Ini adalah proses untuk **mengoptimalkan jaringan radio** supaya performa jaringan tetap bagus dan user puas! 📡

RNO bukan cuma teori - ini pekerjaan **real** yang dilakukan engineer di lapangan setiap hari.

---

## 2. Apa Tugas Tim RNO?

Tim RNO punya **5 langkah utama** kerja:

### Langkah 1: Pengambilan Data 📊

Data diambil dari **dua sumber**:

#### A. Data Hardware

- Dari perangkat fisik di lapangan (antenna, eNodeB, dll)
- Didapat dari **Drive Test** 🚗
- Contoh: Posisi antenna, tilt, azimuth

#### B. Data Software

- Dari sistem RF Planning
- Parameter jaringan (RSRP, SINR, dll)
- Data traffic, throughput

**Drive Test** digunakan:

- Sebelum RF planning (survey awal)
- Setelah site dibangun (acceptance test)
- Saat ada komplain dari pelanggan
- Untuk optimasi rutin

### Langkah 2: Membuat Report 📝

Setelah data terkumpul, engineer RNO harus buat **report** yang berisi:

- Data yang dikumpulkan
- Grafik dan tabel
- Summary kondisi jaringan

### Langkah 3: Analisa Data 🔍

Dari report, dilakukan **analisa**:

- Cari tahu **di mana** masalahnya
- **Hardware** atau **software**?
- Seberapa parah masalahnya?

### Langkah 4: Rekomendasi 💡

Berdasarkan analisa, buat **rekomendasi perbaikan**:

**Jika masalah di Hardware:**

- Dilakukan oleh tim **Network Operation** (NWO)
- Contoh: Ganti antenna, ubah tilt, perbaiki kabel

**Jika masalah di Software:**

- Dilakukan oleh tim **RF Planning**
- Contoh: Ubah parameter, tuning frekuensi

### Langkah 5: Eksekusi & Evaluasi ✅

- Tim yang sesuai **mengeksekusi** rekomendasi
- Setelah eksekusi, tim RNO harus **reporting** lagi
- Lakukan **analisa KPI** → Apakah sudah memenuhi target?

**Belum selesai sampai KPI bagus!** 🎯

---

## 3. Database untuk RNO

Ada **4 jenis database** yang digunakan:

| Database                 | Isi                                        |
| ------------------------ | ------------------------------------------ |
| **Parameter Database**   | Parameter jaringan (power, frequency, dll) |
| **Alarm Database**       | Alert/alarm dari perangkat                 |
| **Capacity Database**    | Data kapasitas & throughput                |
| **Performance Database** | Data performa jaringan (KPI)               |

---

## 4. KPI (Key Performance Indicator)

KPI adalah **metrik** untuk mengukur **seberapa bagus** jaringan kita!

### Ada 2 Jenis KPI:

#### A. RAN KPI (Radio Access Network KPI) / eRAN KPI

Fokus ke **sisi radio** (dari user ke eNodeB/MME)

**6 Parameter RAN KPI:**

1. **Accessibility** ⭐⭐⭐ (Paling penting!)
2. **Retainability** ⭐⭐⭐ (Paling penting!)
3. **Mobility** ⭐⭐⭐ (Paling penting!)
4. **Availability**
5. **Utilization**
6. **Traffic**

**Yang 3 pertama** adalah fokus utama! Kalau 3 ini bagus, yang lain otomatis bagus.

#### B. Service KPI (User Experience)

Fokus ke **pengalaman user**:

1. **Latency** (delay)
2. **Throughput** (kecepatan data)
3. **Packet Loss** (paket yang hilang)

---

## 5. Accessibility (Parameter RAN KPI #1)

**Accessibility** = Kemampuan user untuk **mengakses** jaringan

### Definisi:

> Mengukur **probabilitas** user berhasil mengakses network dan meminta service (voice/data) saat network **sedang beroperasi**.

**Bukan saat idle!** Tapi saat network **aktif digunakan**.

### Ada 3 Parameter Accessibility:

---

#### A. RRC Setup Success Rate (RRC SSR)

**RRC** = Radio Resource Control

**Apa yang diukur:**

- Berapa banyak **request** dari UE ke eNodeB
- Berapa banyak yang **sukses** dapat RRC connection

**Proses:**

```
UE → [Request RRC] → eNodeB
eNodeB → [Setup RRC] → UE
✅ Sukses = RRC Connection Complete
```

**Formula:**

```
RRC SSR = (RRC Connection Complete / RRC Connection Request) × 100%
```

**Counter:**

- **Titik A**: RRC Connection Request (counter mulai)
- **Titik C**: RRC Connection Complete (counter selesai)

**QCI (QoS Class Identifier):**

- Berbeda untuk GBR (Guaranteed Bit Rate) vs Non-GBR
- GBR: VoIP, video call (error rate: 10^-6)
- Non-GBR: Data browsing (error rate lebih besar)

---

#### B. E-RAB Setup Success Rate (E-RAB SSR)

**E-RAB** = E-UTRAN Radio Access Bearer (atau EPS Radio Access Bearer)

**Bedanya dengan RRC:**

- RRC hanya sampai eNodeB
- E-RAB sampai ke **MME** (melibatkan core network!)

**Apa yang diukur:**

- Probabilitas keberhasilan setup dari **UE → eNodeB → MME**
- Untuk **semua service** (voice, data, dll)

**Ada 2 jenis:**

1. **E-RAB SSR untuk VoIP** (khusus voice)
2. **E-RAB SSR untuk Non-VoIP** (data & service lain)

**Proses:**

```
UE → eNodeB → MME
✅ Sukses = Service connection established
```

**Formula:**

```
E-RAB SSR = (E-RAB Setup Success / E-RAB Setup Attempt) × 100%
```

**Kenapa sampai MME?**

- Karena melibatkan **NAS (Non-Access Stratum)** layer
- NAS connect langsung ke MME
- Jadi harus pastikan EPC juga siap

---

#### C. Call Setup Success Rate (CSSR)

**CSSR** = Keberhasilan **panggilan** (call) secara end-to-end

**Apa yang diukur:**

- Keberhasilan **call** dari awal sampai selesai
- Gabungan dari RRC + E-RAB + S1 Connection

**Proses lengkap:**

1. RRC Setup (UE ↔ eNodeB)
2. E-RAB Setup (eNodeB ↔ MME)
3. S1 Connection Establishment (full connection)

**Formula:**

```
CSSR = (Call Setup Complete / Call Setup Attempt) × 100%
```

Atau bisa juga:

```
CSSR = RRC SSR × E-RAB SSR
```

**Ada 2 jenis:**

- **CSSR untuk VoIP** (panggilan suara)
- **CSSR untuk All Services** (semua layanan)

**Counter yang dipakai:**

- RRC Setup
- E-RAB Setup
- S1 Connection Establishment

---

## 6. Retainability (Parameter RAN KPI #2)

**Retainability** = Kemampuan network **mempertahankan** service selama komunikasi berlangsung

### Definisi:

> Mengukur kemampuan network untuk **mempertahankan service** yang diminta user **selama proses komunikasi** (tidak putus di tengah jalan).

**Fokus:** Evaluasi apakah terjadi **Call Drop** atau tidak!

### Parameter Retainability:

---

#### A. Call Drop Rate (CDR)

**Call Drop** = Panggilan yang **putus tiba-tiba** (tidak normal)

**Penyebab Call Drop:**

1. **E-RAB Abnormal Release Indication**

   - Release dari E-RAB yang tidak normal
   - Koneksi antara eNodeB dan MME terputus tidak wajar

2. **UE Context Release Request**
   - UE meminta release connection
   - Tapi dalam kondisi **abnormal** (bukan user yang mematikan)

**Proses normal vs abnormal:**

- **Normal**: User menekan tombol "End Call"
- **Abnormal**: Sinyal hilang, coverage buruk, handover gagal

**Formula:**

```
CDR = (Abnormal Release / Total Call Attempts) × 100%
```

**Target:** CDR harus **serendah mungkin** (idealnya < 2%)

---

#### B. Call Setup Complete Rate (CSCR)

**CSCR** = Tingkat keberhasilan **setup call sampai selesai**

**Apa yang diukur:**

- Dari awal setup sampai **call complete** (tidak drop)
- Gabungan dari **Accessibility** + **Retainability**

**Formula:**

```
CSCR = CSSR × (1 - CDR)
```

Atau:

```
CSCR = (Call Complete Success / Call Setup Attempt) × 100%
```

**Ada 2 jenis:**

- **CSCR untuk VoIP**
- **CSCR untuk All Services**

---

## 7. Mobility (Parameter RAN KPI #3)

**Mobility** = Evaluasi performa radio saat user **bergerak** (mobile)

### Definisi:

> Mengukur **performance dari radio** saat user melakukan **mobility** (berpindah tempat).

**Fokus:** User experience saat bergerak (throughput, latency, packet loss)

### Konsep Penting Sebelum Mobility:

Mari kita bahas **konsep dasar perpindahan** dulu:

---

#### Cell Selection vs Cell Reselection

**Cell Selection:**

- Pemilihan cell **pertama kali** saat HP dinyalakan
- Atau saat awal melakukan komunikasi
- Kondisi: **Idle** (tidak sedang digunakan)

**Cell Reselection:**

- Pemilihan cell **saat berpindah** dalam kondisi **Idle**
- Contoh: Kamu jalan-jalan tapi HP tidak dipakai
- Cell otomatis ganti untuk cari sinyal terbaik

**Contoh:**

- HP kamu mati → nyalakan → **Cell Selection** (cari cell terbaik)
- HP nyala tapi tidak dipakai → jalan-jalan → **Cell Reselection** (ganti cell otomatis)

---

#### Location Update vs Paging

**Location Update:**

- **User** yang memberitahu network: "Saya sekarang di LAC (Location Area Code) ini"
- Dilakukan saat user berpindah antar Location Area
- **Arah:** UE → Network

**Paging:**

- **Network** yang mencari user: "Kamu sekarang di mana?"
- Network broadcast ke semua cell dalam Tracking Area
- **Arah:** Network → UE

**Contoh:**

- Location Update: "Pak operator, saya sekarang di Jakarta Selatan"
- Paging: "Halo UE nomor 0812xxx, kamu ada di mana? Ada panggilan masuk!"

**Kegunaan:**

- Data Location Area tersimpan di **HLR** (Home Location Register)
- Selama **4 jam** terakhir bisa di-track posisi terakhir HP
- Berguna untuk **tracking HP hilang** (kalau belum dimatikan > 4 jam)

---

### 3 Jenis Mobility (Handover):

#### A. Intra-LTE Handover (Intra-Frequency)

**Intra** = Dalam satu frekuensi yang sama

**Definisi:**

- Handover dari **cell A ke cell B**
- Masih dalam **1 eNodeB** yang sama
- Frekuensi yang sama

**Contoh:**

- Dari Cell 1 ke Cell 2 (masih di site yang sama)
- Site melakukan **cell splitting** untuk coverage lebih baik

**Proses:**

```
Cell A (eNodeB 1, Freq 1800 MHz)
        ↓
Cell B (eNodeB 1, Freq 1800 MHz)
```

**Parameter yang diukur:**

- Handover Preparation
- Handover Execution
- Handover Success Rate

---

#### B. Inter-LTE Handover (Inter-Frequency)

**Inter** = Antar eNodeB atau antar frekuensi

**Definisi:**

- Handover dari **eNodeB A ke eNodeB B**
- Bisa beda frekuensi atau beda site

**Proses:**

```
Source eNodeB → Target eNodeB
```

**Yang diukur:**

- **Source eNodeB**: eNodeB awal
- **Target eNodeB**: eNodeB tujuan
- Request handover dari source ke target
- Sampai handover **complete**

**Counter:**

- Handover Request (ke target)
- Handover Success (dari target)

---

#### C. Inter-RAT Handover

**Inter-RAT** = Inter Radio Access Technology

**RAT** = Radio Access Technology (2G, 3G, 4G, 5G)

**Definisi:**

- Handover **antar teknologi**
- Contoh: 4G → 3G, 4G → 2G, 5G → 4G

**Kapan terjadi?**

- Saat **VoLTE** drop ke **CSFB** (Circuit Switched Fallback)
- Coverage 4G buruk → fallback ke 3G/2G
- Untuk **mempertahankan call** supaya tidak putus

**Proses SRVCC** (Single Radio Voice Call Continuity):

```
VoLTE (4G) → [Coverage buruk] → CSFB ke 3G/2G
```

**SRVCC:**

- **SR** = Single Radio (1 radio)
- **VCC** = Voice Call Continuity (panggilan tetap jalan)
- Supaya panggilan **tidak putus** saat pindah teknologi

**Enhanced SRVCC (eSRVCC):**

- SRVCC yang lebih baik
- Handover lebih smooth
- Delay lebih kecil

---

## 8. Perbedaan 2G/3G vs 4G Planning

| Aspek           | 2G/3G (CS Domain)      | 4G (PS Domain)           |
| --------------- | ---------------------- | ------------------------ |
| **Teknologi**   | Circuit Switched       | Packet Switched          |
| **Voice**       | Dedicated channel      | VoIP (VoLTE)             |
| **Perhitungan** | Erlang formula         | Throughput (Mbps)        |
| **Parameter**   | GoS (Grade of Service) | QoS (Quality of Service) |
| **Blocking**    | Blocking probability   | Capacity (user count)    |
| **Channel**     | Dedicated (CS)         | Shared (PS)              |

**Penting untuk interview!** ☝️

---

## 9. Vendor Differences

Setiap **vendor** punya parameter KPI yang **sedikit berbeda**:

### Contoh Vendor:

| Vendor       | Naming Convention | Special Feature                |
| ------------ | ----------------- | ------------------------------ |
| **Huawei**   | Standard naming   | NDC (Network Dynamic Carrier)  |
| **ZTE**      | Standard naming   | DSS (Dynamic Spectrum Sharing) |
| **Ericsson** | Sedikit berbeda   | Advanced Carrier Aggregation   |
| **Nokia**    | Beda istilah      | Fast MI                        |

**Contoh perbedaan:**

- Huawei: WTX (parameter tertentu)
- Nokia: Fast MI (fungsi yang sama)

**Teori sama, implementasi beda!**

---

## 10. Target KPI dan Counter

### Kenapa Butuh Target KPI?

1. **Standar Operator**

   - Setiap operator harus punya standar service
   - Tidak boleh di bawah standar minimum

2. **Kebutuhan Counter**

   - Data pencacah (counter) untuk hitung KPI
   - Counter dimulai saat **Setup**
   - Counter selesai saat **Connected** atau **Success**

3. **Hasil Drive Test**

   - Data dari lapangan untuk analisa
   - Identifikasi kekurangan jaringan

4. **Rekomendasi & Rujukan**

   - Dasar untuk perbaikan
   - Benchmark dengan operator lain

5. **Benchmarking**
   - Membandingkan performa antar operator
   - Contoh: Telkomsel vs Indosat vs XL di area tertentu
   - Mengetahui **di mana kita kalah/menang**

**Tools Benchmarking:**

- Ada alat yang bisa **capture signal** semua operator sekaligus
- Real-time monitoring
- Sangat berguna untuk operator

---

## 11. Proses Signaling (Review Singkat)

### 3 Tahap Komunikasi:

1. **Initialization** (Inisialisasi)
   - Setup awal connection
2. **Transfer** (Transfer Data)

   - Connected state
   - Data dikirim/diterima

3. **Release** (Pelepasan)
   - Connection diputus
   - Normal atau Abnormal

**Counter dihitung di:**

- Setup (attempt)
- Success (complete)

---

## 12. Flow RNO Lengkap

```
┌─────────────────────────────────────────────────────┐
│              1. PENGAMBILAN DATA                    │
│                                                     │
│  Hardware Data     ┌───────────┐    Software Data   │
│  (Drive Test) ────→│  RNO Team │←──── (RF Planning) │
│                    └───────────┘                    │
└─────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────┐
│              2. MEMBUAT REPORT                      │
│                                                     │
│  • Data Summary                                     │
│  • Grafik & Tabel                                   │
│  • KPI Current State                                │
└─────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────┐
│              3. ANALISA DATA                        │
│                                                     │
│  • Identifikasi Masalah                             │
│  • Hardware atau Software?                          │
│  • Tingkat Severity                                 │
└─────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────┐
│              4. REKOMENDASI                         │
│                                                     │
│  Hardware Issue ────→ Network Operation Team        │
│  Software Issue ────→ RF Planning Team              │
└─────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────┐
│              5. EKSEKUSI                            │
│                                                     │
│  • Implementasi perbaikan                           │
│  • Testing                                          │
│  • Reporting hasil                                  │
└─────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────┐
│              6. EVALUASI KPI                        │
│                                                     │
│  • Apakah KPI memenuhi target?                      │
│  • Jika belum → Ulangi dari Step 1                  │
│  • Jika sudah → Success! ✅                         │
└─────────────────────────────────────────────────────┘
```

---

## 13. Summary Formula KPI

### Accessibility:

```
RRC SSR = (RRC Connection Complete / RRC Connection Request) × 100%

E-RAB SSR = (E-RAB Setup Success / E-RAB Setup Attempt) × 100%

CSSR = RRC SSR × E-RAB SSR
```

### Retainability:

```
CDR = (Abnormal Release / Total Call Attempts) × 100%

CSCR = CSSR × (1 - CDR)
```

### Target Umum:

| KPI       | Target |
| --------- | ------ |
| RRC SSR   | > 99%  |
| E-RAB SSR | > 99%  |
| CSSR      | > 98%  |
| CDR       | < 2%   |
| CSCR      | > 96%  |

---

## 14. Checklist Engineer RNO

Sebelum submit hasil optimasi, pastikan sudah:

### Data Collection:

- ☑ Drive Test completed
- ☑ Parameter data collected
- ☑ Counter data verified
- ☑ Alarm check done

### Analysis:

- ☑ KPI calculated correctly
- ☑ Problem identified (HW/SW)
- ☑ Root cause analysis done
- ☑ Comparison with benchmark

### Recommendation:

- ☑ Clear action items
- ☑ Priority defined
- ☑ Timeline estimated
- ☑ Resource requirements

### Execution:

- ☑ Changes implemented
- ☑ Testing completed
- ☑ Documentation updated
- ☑ Stakeholders informed

### Evaluation:

- ☑ Post-optimization KPI measured
- ☑ Improvement verified
- ☑ Final report submitted
- ☑ Lessons learned documented

---

## 15. Tips untuk Interview

### Yang Harus Dikuasai:

1. **Bedakan dengan jelas:**

   - Cell Selection vs Cell Reselection
   - Location Update vs Paging
   - Intra-Handover vs Inter-Handover vs Inter-RAT

2. **Pahami flow:**

   - RRC → E-RAB → CSSR (urutan setup)
   - Normal Release vs Abnormal Release

3. **Hafal formula:**

   - RRC SSR, E-RAB SSR, CSSR, CDR
   - Tahu cara hitungnya

4. **Konsep dasar:**
   - 2G/3G (CS) vs 4G (PS)
   - VoLTE vs CSFB
   - SRVCC vs eSRVCC

### Cara Menjawab Interview:

**Pertanyaan:** "Apa yang kamu tahu tentang RNO?"

**Jawaban yang baik:**

> "RNO adalah Radio Network Optimization, prosesnya meliputi data collection, analysis, recommendation, execution, dan evaluation. Fokus utamanya adalah memastikan KPI jaringan memenuhi target, terutama Accessibility, Retainability, dan Mobility. Saya sudah pernah belajar tools seperti TEMS dan memahami konsep Drive Test."

**Jangan jawab:**

> "RNO itu optimasi jaringan." ❌ (Terlalu singkat!)

**Pertanyaan:** "Apa bedanya Call Drop dengan Call Block?"

**Jawaban yang baik:**

> "Call Drop itu panggilan yang sudah connect tapi putus di tengah jalan (masalah Retainability). Call Block itu panggilan yang gagal connect dari awal (masalah Accessibility). Call Drop penyebabnya biasanya abnormal release dari E-RAB atau UE Context, sedangkan Call Block karena RRC atau E-RAB setup gagal."

---

## 16. Software/Tools RNO

### Tools yang Umum Dipakai:

1. **TEMS Investigation** (Drive Test)

   - Untuk pengambilan data lapangan
   - Measure RSRP, SINR, throughput, dll
   - Export ke Excel untuk analisa

2. **Atoll** (Planning)

   - RF planning & coverage simulation
   - Integration dengan data drive test

3. **Vendor-Specific Tools:**

   - Huawei: U-Net
   - ZTE: ZSmart
   - Ericsson: TEMS Planning
   - Nokia: NetAct

4. **Excel/Python** (Analysis)
   - Untuk processing data
   - Hitung KPI
   - Buat report

---

## 17. Real Case: Optimasi Step-by-Step

### Skenario:

Operator dapat komplain: **"Sinyal 4G bagus tapi call sering putus!"**

### Step 1: Data Collection

- Drive Test di area komplain
- Check parameter eNodeB
- Check alarm di OMC

### Step 2: Analisa

**Data yang ditemukan:**

- RSRP: -85 dBm (bagus)
- SINR: 15 dB (bagus)
- CDR: 5% (buruk! Target < 2%)
- E-RAB Abnormal Release tinggi

**Kesimpulan:** Masalah bukan di coverage, tapi di **Retainability**!

### Step 3: Root Cause

Cek detail E-RAB Abnormal Release:

- Penyebab: **Handover failure** antar cell
- Cell neighbor tidak ter-configure dengan benar

### Step 4: Rekomendasi

- **Update neighbor relation** di eNodeB
- **Optimize handover parameter** (Threshold, Time To Trigger)
- **Verify X2 interface** status

### Step 5: Eksekusi

- RF Planning team update parameter
- Re-test dengan Drive Test

### Step 6: Evaluasi

**Hasil setelah optimasi:**

- CDR: 1.5% ✅ (memenuhi target!)
- User complain berkurang

**Success!** 🎉

---

## 18. Kesimpulan

### RNO itu:

- ✅ **Proses continuous improvement** untuk jaringan
- ✅ **Melibatkan banyak tim**: RNO, NWO, RF Planning
- ✅ **Fokus pada KPI**: Accessibility, Retainability, Mobility
- ✅ **Data-driven**: Semua keputusan berdasarkan data

### Yang Harus Dikuasai:

1. **Konsep KPI** (RRC, E-RAB, CSSR, CDR)
2. **Proses Handover** (Intra, Inter, Inter-RAT)
3. **Drive Test** (minimal tahu cara pakai TEMS)
4. **Analisa Data** (Excel/Python)
5. **Root Cause Analysis** (HW vs SW)

### Career Path:

```
Junior RNO Engineer
        ↓
RNO Engineer
        ↓
Senior RNO Engineer
        ↓
RNO Lead / Team Leader
        ↓
RNO Manager
```

**Skill tambahan yang bagus:**

- Coding (Python untuk automation)
- Machine Learning (untuk predictive maintenance)
- Big Data (untuk analisa massive data)

---

## 19. Resources untuk Belajar Lebih Lanjut

### Sertifikasi:

1. **Kompetensi RNO 4G LTE**

   - 3 hari training + ujian
   - Praktek dengan TEMS

2. **Vendor-Specific:**
   - Huawei HCIP-RNO
   - Ericsson Certified RNO
   - Nokia RNO Certification

### Online:

- 3GPP Standards (technical specs)
- Vendor white papers (Huawei, Ericsson, Nokia)
- YouTube: RF tutorials
- LinkedIn Learning: Telecom courses

---

## 20. Motivasi Akhir

**RNO Engineer** adalah **ujung tombak** operator! 💪

Tanpa RNO:

- ❌ User complain banyak
- ❌ KPI buruk
- ❌ Competitor menang
- ❌ Revenue turun

Dengan RNO yang baik:

- ✅ User puas
- ✅ KPI excellent
- ✅ Competitive advantage
- ✅ Revenue naik

**Pro tip:**

- Percaya diri tapi tidak sombong
- Tunjukkan kamu **paham flow end-to-end**
- Kalau belum pernah praktik: "Saya paham konsepnya dan siap belajar di lapangan"
- Jujur itu penting!

**RNO = Radio Network Optimization = Optimasi berkelanjutan = Karir cemerlang!** 🚀

---

**Good luck untuk magang, PKL, dan karir di telco!** 🎓📡

**Semoga catatan ini bermanfaat!** 💙
