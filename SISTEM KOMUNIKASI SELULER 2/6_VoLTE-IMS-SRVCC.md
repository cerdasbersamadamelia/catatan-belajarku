# 6. VoLTE Lanjutan - IMS & SRVCC

## OVERVIEW

Materi kali ini lanjutan VoLTE yang membahas:

- Strategi voice di 4G (Voice-over CS vs Voice-over LTE)
- SRVCC (Single Radio Voice Call Continuity)
- IMS (IP Multimedia Subsystem) detail
- Arsitektur & komponen IMS
- Prosedur handover VoLTE
- Enhanced SRVCC

---

## 1. STRATEGI VOICE DI 4G (REVIEW)

### Dua Strategi Utama

#### Strategi 1: Voice-over CS

- Voice tetap pakai **CS domain** (2G/3G)
- Data pakai **LTE**
- = **CSFB** (Circuit Switched Fallback)

**Cocok untuk**:

- Operator punya existing 2G/3G
- Belum siap invest IMS

#### Strategi 2: Voice-over LTE

- Voice pakai **PS domain** (lewat IMS)
- Data pakai **PS domain**
- = **VoLTE**

**Cocok untuk**:

- Operator baru (tidak punya 2G/3G)
- Mau full IP network

### Kenapa Ada 2 Strategi?

**Masalah**: Coverage VoLTE belum merata!

**Contoh kasus**:

- User di Jakarta → ada VoLTE coverage ✅
- User pindah ke Cirebon → belum ada VoLTE ❌
- Sedang telepon → bagaimana?

**Solusi**: **SRVCC**!

---

## 2. SRVCC (SINGLE RADIO VOICE CALL CONTINUITY)

### Apa itu SRVCC?

**SRVCC** = Single Radio Voice Call Continuity

- Teknologi **handover** dari VoLTE ke CS domain
- Memastikan panggilan **tidak terputus**
- Saat coverage VoLTE hilang → otomatis pindah ke 2G/3G
- Voice call tetap berlanjut

### Kapan SRVCC Bekerja?

**Skenario**:

1. User sedang telepon pakai VoLTE (4G)
2. User pindah ke area tanpa coverage VoLTE
3. Sinyal 4G lemah/hilang
4. **SRVCC aktif** → handover ke 3G/2G
5. Voice call tetap jalan (tidak putus)

### Syarat SRVCC

**Operator HARUS punya**:

- ✅ VoLTE (dengan IMS)
- ✅ Existing 2G/3G network

**Kalau tidak punya 2G/3G?**

- ❌ Tidak bisa SRVCC
- Call akan putus saat coverage VoLTE hilang

**Contoh**: Smartfren

- Hanya punya 4G LTE
- Tidak punya 2G/3G
- Tidak ada SRVCC
- Kalau coverage jelek → call putus

### SRVCC = Software

**Implementasi**:

- Berupa **software**
- Diinstall di: MME, MSC Server, IMS
- Memastikan seamless handover
- User tidak sadar ada perpindahan jaringan

---

## 3. NOMOR MSISDN

### Struktur Nomor HP

**Contoh**: 0812-xxxx-xxxx

**Bagian nomor**:

- **0812** = Kode operator
- **xxxx** = Kode HLR (wilayah)
- **xxxx** = Nomor pelanggan

### Kode HLR (Wilayah)

**Contoh kode HLR**:

- **92** = Jawa Timur
- **33** = Jawa Tengah
- **22** = Jawa Barat (Bandung)
- **21** = Jakarta

**Dulu**:

- Nomor menunjukkan lokasi HLR
- Sekarang: nomor bisa dijual dimana-mana

**Catatan**: Panggilan pakai **nomor MSISDN** (bukan WhatsApp/Line)

---

## 4. IMS (IP MULTIMEDIA SUBSYSTEM)

### Fungsi Utama IMS

**IMS** = Gateway antara LTE dengan network lain

**Koneksi yang dihandle IMS**:

1. **LTE ↔ Non-LTE**

   - VoLTE → 2G/3G
   - VoLTE → PSTN

2. **LTE ↔ LTE (beda operator)**

   - Telkomsel VoLTE → Smartfren VoLTE
   - Lewat IMS (interconnected network)

3. **LTE ↔ LTE (sama operator, beda area)**
   - Telkomsel Jakarta → Telkomsel Bali
   - Dalam 1 PLMN (tidak lewat IMS)

### Prinsip IMS

> **IMS = Interface untuk komunikasi multimedia berbasis IP**

- Voice di LTE = butuh interface khusus
- IMS = jembatan antara IP dan non-IP
- Semua voice lewat IMS

---

## 5. KOMPONEN IMS

### 3 Komponen Utama

#### 1. P-CSCF (Proxy-CSCF)

**P-CSCF** = Proxy Call Session Control Function

- **Fungsi**: Proxy (perantara)
- First point of contact untuk UE
- Routing signaling

#### 2. I-CSCF (Interrogating-CSCF)

**I-CSCF** = Interrogating Call Session Control Function

- **Fungsi**: Query (tanya-jawab)
- Tanya ke HSS: user ada dimana?
- Routing ke S-CSCF yang benar

#### 3. S-CSCF (Serving-CSCF)

**S-CSCF** = Serving Call Session Control Function

- **Fungsi**: Call control utama
- Handle session management
- Routing & policy
- Menghasilkan **SIP protocol**

### Arsitektur IMS

```
UE → P-CSCF → I-CSCF → S-CSCF → HSS
                                 ↓
                           Interconnected
                              Network
```

**NNI** = Network to Network Interface

- Interface antar network berbeda
- IMS sebagai gateway

---

## 6. LAYER PROTOCOL IMS

### Signaling: SS7 vs Diameter

**Legacy (2G/3G)**:

```
ISUP
MTP3
MTP2
MTP1
```

**IP-based (4G)**:

```
SIP/Diameter
SCTP/IP
Ethernet
Physical
```

### PSTN ↔ LTE

**Dari PSTN (non-IP) ke LTE (IP)**:

```
[PSTN]                    [LTE]
ISUP                      SIP
MTP3       → IMS →        Diameter
MTP2                      SCTP (M3UA)
MTP1                      IP/Ethernet
```

**IMS mengubah**:

- SS7 → IP-based
- ISUP → SIP/Diameter
- MTP → SCTP

### SCTP (Stream Control Transmission Protocol)

**Apa itu SCTP?**

- Protokol transport (seperti TCP/UDP)
- Cocok untuk signaling
- Reliable seperti TCP
- Message-oriented seperti UDP

**Varian SCTP**:

- **M2PA**, **M2UA**, **M3UA**
- Yang paling umum: **M3UA**
- M3UA = menggantikan MTP1 & MTP2

---

## 7. ARSITEKTUR VoLTE LENGKAP

### Komponen End-to-End

```
UE → eNodeB → SGW → PGW → DRA → IMS Core
      ↓        ↓
     MME      HSS
```

**Penjelasan**:

**Access Network**:

- UE → eNodeB

**Core Network**:

- MME (signaling)
- SGW/PGW (data)
- HSS (database)

**IMS**:

- DRA (Diameter Routing Agent)
- P-CSCF, I-CSCF, S-CSCF

**DRA** = Diameter Routing Agent

- Router untuk Diameter protocol
- Menghubungkan berbagai komponen
- Seperti router di network IP

### VoLTE Call Flow (Sesama VoLTE)

**Skenario**: Telkomsel (Mampang) → Telkomsel (Pasar Minggu)

1. UE → eNodeB → MME (signaling)
2. UE → eNodeB → SGW → PGW (data)
3. PGW → DRA → IMS Core
4. IMS handle call control
5. Route ke tujuan
6. Voice call berjalan

**Karena 1 PLMN** → tidak lewat interconnected network

### VoLTE Call ke Operator Lain

**Skenario**: Telkomsel VoLTE → Smartfren VoLTE

1. UE → eNodeB → Core → IMS
2. IMS → **Interconnected Network**
3. Interconnected Network → IMS operator lain
4. IMS → Core → eNodeB → UE tujuan

**Beda PLMN** → lewat interconnected network

---

## 8. IP-CAN (IP CONNECTIVITY ACCESS NETWORK)

### Komponen IP-CAN

**IP-CAN** = IP Connectivity Access Network

Terdiri dari:

- **eNodeB**
- **MME**

**Fungsi**:

- Menyediakan koneksi IP ke UE
- Access layer untuk IMS

---

## 9. PROSEDUR SRVCC

### 3 Tahap SRVCC

#### 1. Inisiasi

**Trigger**:

- eNodeB deteksi sinyal 4G lemah
- Perlu handover ke 3G/2G

**Action**:

- eNodeB indikasikan ke MME
- MME inisiasi SRVCC procedure

#### 2. Transfer Session

**Proses**:

- MME kontak MSC Server
- MSC Server kontak IMS
- IMS transfer session ke CS domain

#### 3. Handover Complete

**Hasil**:

- UE reselect ke 3G/2G (UTRAN/GERAN)
- Session continue di CS domain
- Voice call tetap jalan

### Diagram SRVCC

```
[Normal VoLTE]
UE → eNodeB → IMS (VoIP)
       ↓
   [Sinyal lemah]
       ↓
   [SRVCC aktif]
       ↓
UE → NodeB/BTS → MSC (CS voice)
```

**Signaling** (merah):

- UE → eNodeB → MME → MSC → IMS

**Data** (biru):

- Voice path handover dari IMS ke MSC

---

## 10. ENHANCED SRVCC (eSRVCC)

### Apa itu eSRVCC?

**eSRVCC** = Enhanced SRVCC

- Versi upgrade dari SRVCC
- Lebih cepat & seamless
- Support video call handover

### Perbedaan SRVCC vs eSRVCC

| Aspek      | SRVCC         | eSRVCC           |
| ---------- | ------------- | ---------------- |
| Speed      | Normal        | Lebih cepat      |
| Video call | ❌ Voice only | ✅ Voice + video |
| Seamless   | Baik          | Lebih baik       |
| Latency    | Normal        | Lebih rendah     |

### Tugas

**Cari tahu**:

- Apa perbedaan detail SRVCC vs eSRVCC?
- Bagaimana prosedur eSRVCC?
- Kapan dipakai eSRVCC?

---

## 11. IMPLEMENTASI OPERATOR

### Telkomsel

**Status**:

- ✅ Punya VoLTE
- ✅ Punya 2G/3G
- ✅ Ada SRVCC

**Coverage**:

- VoLTE: Jakarta, Surabaya, Bali (kota besar)
- Belum nationwide

**Strategi**:

- Bertahap expand VoLTE
- Maintain 2G/3G untuk backup

### Smartfren

**Status**:

- ✅ Punya VoLTE (dengan IMS)
- ❌ Tidak punya 2G/3G
- ❌ Tidak ada SRVCC

**Risiko**:

- Coverage jelek → call putus
- Tidak ada fallback network

**Solusi**:

- Focus expand coverage 4G
- Pastikan coverage merata

---

## 12. FUTURE: 5G NSA

### 5G NSA dengan VoLTE

**5G NSA** = Non-Standalone

- Radio: 5G
- Core: **4G** (EPC)

**Voice di 5G NSA**:

- Masih pakai **VoLTE**!
- Core 4G → IMS 4G
- Prosedur sama dengan 4G

**Pertanyaan terbuka**:

- Apakah ada SRVCC di 5G NSA?
- Kalau coverage 5G hilang, fallback kemana?
  - 5G → 4G (seamless)
  - 4G → 3G (pakai SRVCC)

---

## 13. LAB PRAKTIKUM (RENCANA)

### Virtual LTE Network

**Rencana**:

- Build virtual 4G LTE network
- Test VoLTE
- Test SRVCC
- Pakai software simulation

**Tools**:

- OpenAirInterface
- srsRAN
- Virtual machines

**Target**:

- Praktik langsung
- Lihat protokol real-time
- Troubleshooting

#### (Alhamdulilah udah gw and team buat [cek disini](https://www.youtube.com/playlist?list=PLEBDUtoHS8Way-QADI53M11agzfnfNcOZ))

---

## GLOSSARY

| Istilah    | Kepanjangan                          | Artinya                                |
| ---------- | ------------------------------------ | -------------------------------------- |
| **SRVCC**  | Single Radio Voice Call Continuity   | Handover seamless VoLTE ke CS          |
| **eSRVCC** | Enhanced SRVCC                       | SRVCC yang lebih cepat & support video |
| **IMS**    | IP Multimedia Subsystem              | Gateway voice di LTE                   |
| **P-CSCF** | Proxy CSCF                           | Proxy call control                     |
| **I-CSCF** | Interrogating CSCF                   | Query HSS                              |
| **S-CSCF** | Serving CSCF                         | Call control utama (SIP)               |
| **DRA**    | Diameter Routing Agent               | Router Diameter protocol               |
| **IP-CAN** | IP Connectivity Access Network       | eNodeB + MME                           |
| **NNI**    | Network to Network Interface         | Interface antar network                |
| **SCTP**   | Stream Control Transmission Protocol | Protokol transport untuk signaling     |
| **M3UA**   | MTP3 User Adaptation                 | SCTP variant untuk SS7 over IP         |
| **MSISDN** | Mobile Station ISDN Number           | Nomor HP                               |
| **HLR**    | Home Location Register               | Database pelanggan (2G/3G)             |

---

## TIPS PENTING

✅ **Operator Ideal**:

- Punya VoLTE (coverage luas)
- Punya 2G/3G (backup)
- Ada SRVCC (continuity)

⚠️ **Risiko Operator Tanpa 2G/3G**:

- Tidak ada fallback
- Coverage jelek = call putus
- Contoh: Smartfren (hanya 4G)

📞 **SRVCC Bekerja Otomatis**:

- User tidak sadar
- Seamless handover
- Voice quality tetap terjaga

🔄 **Handover Path**:

- VoLTE (4G) → SRVCC → CS (3G/2G)
- Seamless, tidak putus

---

## FAQ

**Q: Bedanya SRVCC sama CSFB?**  
A:

- CSFB: Dari awal telepon, langsung turun ke 3G/2G
- SRVCC: Mulai di VoLTE (4G), baru turun ke 3G/2G kalau perlu

**Q: Kapan SRVCC aktif?**  
A: Saat:

1. User sedang call VoLTE
2. Sinyal 4G lemah/hilang
3. Otomatis handover ke 3G/2G

**Q: Apa syarat SRVCC?**  
A:

1. Operator punya VoLTE (IMS)
2. Operator punya 2G/3G
3. Software SRVCC terinstall

**Q: Kenapa Smartfren tidak ada SRVCC?**  
A: Karena Smartfren hanya punya 4G, tidak punya 2G/3G. Tidak ada network untuk fallback.

**Q: IMS itu apa?**  
A: Gateway untuk voice di LTE. Menghubungkan LTE (IP) dengan network lain (non-IP atau IP berbeda).

**Q: Komponen IMS apa saja?**  
A: P-CSCF (proxy), I-CSCF (query), S-CSCF (call control).

**Q: Voice di 5G NSA pakai apa?**  
A: Masih pakai VoLTE (karena core 4G).

---

## KESIMPULAN

### SRVCC

- Handover VoLTE ke CS domain
- Seamless (tidak putus)
- Butuh existing 2G/3G
- Otomatis saat sinyal 4G lemah

### IMS

- Gateway voice di LTE
- Komponen: P-CSCF, I-CSCF, S-CSCF
- Handle call control (SIP)
- Koneksi antar network

### Strategi Operator

- **Ideal**: VoLTE + SRVCC + 2G/3G
- **Risiko**: VoLTE only (no backup)

### Future

- 5G NSA: masih pakai VoLTE
- 5G SA: native voice (belum jelas)
- Materi lanjutan: RF Planning & hitung-hitungan

---

## TUGAS

**Cari tahu**:

1. Perbedaan SRVCC vs eSRVCC (detail!)
2. Prosedur eSRVCC
3. Kapan pakai eSRVCC?
4. Apakah eSRVCC bisa handover video call?

**Review sebelum pertemuan berikutnya!**

---

**Catatan**: Materi berikutnya masuk ke **RF Planning** dan **perhitungan link budget**. Materi ini jadi dasar untuk praktik!
