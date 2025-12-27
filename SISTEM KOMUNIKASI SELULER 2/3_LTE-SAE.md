# 3. LTE SAE (System Architecture Evolution)

## Pengenalan

**LTE SAE** disebut sebagai **System Architecture Evolution** karena merupakan pengembangan dari jaringan sebelumnya (3G).

### Kenapa disebut "Evolution"?

- LTE mengintegrasikan dan mengembangkan teknologi dari jaringan 3G
- Semua koneksi menggunakan **IP only** (all-IP network)
- Tidak ada lagi Circuit Switching (CS), semuanya Packet Switching (PS)
- Disebut juga **EPS (Evolved Packet System)**

### Komponen Utama 4G LTE

**Dua bagian besar:**

1. **E-UTRAN** (Evolved UTRAN) - bagian akses/access network
2. **EPC** (Evolved Packet Core) - bagian core network

---

## Arsitektur Jaringan 4G

### 1. Struktur Umum

```
User → E-UTRAN (Access) → Aggregation → EPC (Core)
```

**Tiga layer utama:**

- **Access Network**: eNodeB (sisi user)
- **Aggregation**: penghubung antara access dan core (menggunakan MPLS)
- **Core Network**: MME, SGW, PGW, HSS, PCRF

### 2. Perangkat di E-UTRAN (Radio Access Network)

**eNodeB (evolved NodeB)**

- Satu-satunya perangkat di access layer
- Gabungan NodeB + RNC (berbeda dengan 3G)
- Bisa langsung komunikasi antar eNodeB tanpa controller
- Interface: **X2** (antar eNodeB)

**Perbedaan dengan 3G:**
| 3G (UTRAN) | 4G (E-UTRAN) |
|------------|--------------|
| NodeB + RNC terpisah | Jadi satu di eNodeB |
| NodeB butuh RNC untuk kontrol | eNodeB mandiri |
| Interface: Iub | Interface: X2 |

### 3. Perangkat di EPC (Core Network)

**MME (Mobility Management Entity)**

- Khusus untuk **signaling** (control plane)
- Fungsi: mobility management, security, authentication
- **TIDAK membawa data user**
- Interface ke eNodeB: **S1-MME**
- Interface ke HSS: **S6a**

**SGW (Serving Gateway)**

- Membawa **data user** (user plane)
- Mobility anchor point
- Interface ke eNodeB: **S1-U**
- Interface ke PGW: **S5/S8**

**PGW (PDN Gateway)**

- Gateway ke jaringan eksternal (internet, IMS)
- Alokasi IP address untuk user
- Interface ke internet/PDN: **SGi**

**HSS (Home Subscriber Server)**

- Database pelanggan (seperti HLR di 3G)
- Menyimpan data permanen subscriber
- Data profil, authentication, roaming

**PCRF (Policy and Charging Rules Function)**

- **Perangkat baru di 4G**
- Fungsi billing dan charging
- Penting untuk roaming (local breakout vs home routed)

---

## Kelebihan 4G LTE vs 3G

### 1. Teknologi Multiplexing

**4G menggunakan OFDM (Orthogonal Frequency Division Multiplexing)**

**Keunggulan OFDM:**

- Mengurangi interferensi dengan teknik orthogonal
- Kapasitas kanal lebih optimal
- Jumlah subscriber lebih banyak

**Cara kerja:**

- Frekuensi diubah ke fungsi waktu menggunakan **FFT (Fast Fourier Transform)**
- Menghilangkan interferensi antar frekuensi

### 2. Kecepatan Data

| Parameter         | 4G LTE   | 3G HSDPA  |
| ----------------- | -------- | --------- |
| Downlink          | 100 Mbps | ~14 Mbps  |
| Uplink            | 50 Mbps  | ~5.7 Mbps |
| Latency           | < 5 ms   | > 10 ms   |
| Bandwidth minimum | 20 MHz   | 5-15 MHz  |

### 3. Modulasi

**4G punya lebih banyak skema modulasi:**

- QPSK
- 16QAM
- 64QAM
- 256QAM
- Bahkan bisa 512QAM, 1024QAM

**3G:**

- Lebih terbatas (QPSK, 16QAM)

### 4. Spektrum Efisiensi

- Throughput 3-4x lebih besar dari HSDPA
- Packet loss lebih rendah
- QoS lebih baik (delay, jitter berkurang)

### 5. Mobility

- Support kecepatan tinggi karena bandwidth lebih besar
- Better handover mechanism

---

## Frekuensi 4G LTE di Indonesia

### Band 1800 MHz (DCS)

**Kenapa pakai 1800 MHz?**

**Perbandingan opsi:**

- **900 MHz**: Coverage luas tapi bandwidth kecil (tidak cukup untuk LTE)
- **1800 MHz**: Balance antara coverage dan bandwidth ✓
- **2100 MHz**: Bandwidth besar tapi coverage kecil

**Prinsip:**

- Frekuensi tinggi = coverage kecil
- Frekuensi rendah = coverage besar

**Bandwidth allocation:**

- Minimum LTE: **20 MHz**
- Yang bisa dipakai: **90%** = **18 MHz**
- Sisanya 10% untuk guard band (mencegah interferensi)

### Refarming

**Proses penataan ulang frekuensi:**

- Frekuensi 1800 MHz yang tadinya untuk GSM/2G
- Ditata ulang agar tiap operator dapat bandwidth 20 MHz
- Contoh: Operator dapat blok 7.5 MHz → dikumpulkan jadi 20 MHz

---

## Layer di E-UTRAN

### Access Stratum (AS)

**5 layer yang punya 2 fungsi (control plane + user plane):**

1. **PHY (Physical Layer)**
2. **MAC (Medium Access Control)**
3. **RLC (Radio Link Control)**
4. **PDCP (Packet Data Convergence Protocol)**
5. **RRC (Radio Resource Control)**

**Kenapa disebut "Access Stratum"?**

- Karena punya fungsi ganda:
  - Control plane → ke MME (S1-MME)
  - User plane → ke SGW (S1-U)

### Non-Access Stratum (NAS)

**Layer di MME dan SGW:**

- Hanya punya 1 fungsi
- MME: control plane only
- SGW: user plane only

---

## Interface di 4G LTE

### Interface Utama

| Interface  | Koneksi            | Fungsi                          |
| ---------- | ------------------ | ------------------------------- |
| **LTE-Uu** | UE ↔ eNodeB        | Radio interface                 |
| **X2**     | eNodeB ↔ eNodeB    | Handover, load balancing        |
| **S1-MME** | eNodeB ↔ MME       | Control plane (signaling)       |
| **S1-U**   | eNodeB ↔ SGW       | User plane (data)               |
| **S11**    | MME ↔ SGW          | Control messaging               |
| **S5/S8**  | SGW ↔ PGW          | Data transfer                   |
| **S6a**    | MME ↔ HSS          | Authentication, subscriber data |
| **SGi**    | PGW ↔ Internet/PDN | External network                |

### Visualisasi Signaling vs Data

**Garis putus-putus** (---) = Signaling (control plane)

**Garis lurus** (\_\_\_) = Data (user plane)

```
eNodeB ---S1-MME---> MME (signaling only)
       ___S1-U___> SGW → PGW (data only)
```

---

## Teknologi Transport

### MPLS (Multi-Protocol Label Switching)

**Fungsi:**

- Transport layer antara access dan core
- Menggabungkan ATM + IP
- Digunakan di aggregation network

**Kenapa butuh aggregation?**

- Ribuan eNodeB tidak bisa langsung ke core
- Perlu pengelompokan (aggregation) dulu
- MPLS jadi "jalan tol" untuk data

---

## Proses Penting di 4G

### 1. Cell Selection & Reselection

**UE melakukan scanning:**

- Mencari eNodeB dengan sinyal terbaik
- Parameter: **RSRP (Reference Signal Received Power)**
- UE terus-menerus monitoring sinyal

**Tips troubleshooting:**

- Sinyal lemah? Restart HP
- Restart = paksa UE melakukan cell reselection
- UE akan cari sinyal terbaik lagi

### 2. Handover

**Soft Handover vs Hard Handover:**

**Soft Handover (3G):**

- User tetap terhubung ke cell lama sambil connect ke cell baru
- Lebih aman, menghindari blank spot
- Lebih smooth

**Hard Handover (2G, 4G):**

- Putus dari cell lama, baru connect ke cell baru
- Risiko blank spot lebih besar
- Lebih cepat

**Blank Spot:**

- Area tanpa sinyal (tidak tercover 2 cell)
- Atau area dengan 2 sinyal sama kuat

### 3. Packing

**Proses yang dilakukan UE:**

- Mendeteksi eNodeB mana yang meng-cover
- Melaporkan signal strength ke eNodeB
- eNodeB juga melakukan survey untuk handle user

---

## Voice di 4G

### Masalah: LTE = Data Only

**4G LTE default tidak support voice** karena:

- Hanya pakai packet switching
- Circuit switching tidak ada

### Solusi Voice

**2 cara implementasi voice:**

1. **CSFB (Circuit Switched Fallback)**

   - Operator punya existing 2G/3G network
   - Voice turun ke 2G/3G
   - Data tetap di 4G

2. **VoLTE (Voice over LTE)**
   - Operator tidak punya 2G/3G
   - Atau mau full IP
   - Perlu tambahan: **IMS (IP Multimedia Subsystem)**
   - Voice dijadikan data (VoIP)

---

## Perbandingan dengan 5G

### Kebutuhan Bandwidth

| Teknologi | Bandwidth Minimum | Bandwidth Ideal | Latency |
| --------- | ----------------- | --------------- | ------- |
| 4G LTE    | 20 MHz            | 20 MHz          | < 5 ms  |
| 5G NSA    | 60 MHz            | -               | < 1 ms  |
| 5G SA     | 60 MHz            | 100 MHz         | ~0 ms   |

**Catatan 5G:**

- Bisa jalan dengan < 60 MHz, tapi tidak optimal
- Latency tidak akan sebaik spesifikasi

---

## Virtual Core Network

### Konsep Virtualisasi

**Masa depan implementasi:**

- Semua perangkat core menjadi **virtual machines**
- Tidak lagi hardware fisik
- Teknologi: **Container, SDN (Software Defined Network), NFV**

**Yang divirtualisasi:**

- MME, SGW, PGW, HSS, PCRF
- Semua jadi VM (Virtual Machine)

**Konfigurasi:**

- Menggunakan interface standar 3GPP
- Contoh: virtual MME ↔ virtual SGW tetap pakai S11
- Penting hafal interface untuk konfigurasi

---

## Protokol Signaling

### Diameter Protocol

**Menggantikan SS7:**

- 4G pakai **Diameter** untuk signaling
- 3G masih pakai **SS7**
- Lebih cocok untuk all-IP network

**Penggunaan:**

- Interface S6a (MME ↔ HSS)
- Roaming architecture
- Charging (PCRF)

---

## Tips Menghapal

### Perangkat Core

**Urutan dari access ke internet:**

```
UE → eNodeB → MME (signaling)
            → SGW (data) → PGW → Internet
```

**Fungsi gampangnya:**

- **MME** = otak (signaling)
- **SGW** = jalan data (user plane)
- **PGW** = pintu keluar (gateway)

### Interface Penting

**Wajib hafal:**

- S1-MME, S1-U (eNodeB ke core)
- S11 (MME ↔ SGW)
- S6a (MME ↔ HSS)
- X2 (antar eNodeB)

**Prinsip:**

- Putus-putus = signaling → MME
- Lurus = data → SGW

---

## Kesimpulan

**4G LTE = Evolution dari 3G:**

- All-IP network
- Lebih cepat (100 Mbps downlink)
- Latency rendah (< 5 ms)
- OFDM multiplexing
- eNodeB mandiri (tidak perlu RNC)

**Perangkat baru:**

- MME (signaling only)
- PCRF (billing/charging)

**Frekuensi:**

- 1800 MHz (balance coverage & bandwidth)
- Minimum 20 MHz

**Voice:**

- CSFB (fallback ke 2G/3G)
- VoLTE (butuh IMS)

---

## Glossary

- **AS**: Access Stratum
- **CSFB**: Circuit Switched Fallback
- **EPC**: Evolved Packet Core
- **eNodeB**: evolved NodeB
- **EPS**: Evolved Packet System
- **E-UTRAN**: Evolved UTRAN
- **HSS**: Home Subscriber Server
- **IMS**: IP Multimedia Subsystem
- **LTE**: Long Term Evolution
- **MME**: Mobility Management Entity
- **MPLS**: Multi-Protocol Label Switching
- **NAS**: Non-Access Stratum
- **OFDM**: Orthogonal Frequency Division Multiplexing
- **PCRF**: Policy and Charging Rules Function
- **PGW**: PDN Gateway
- **SAE**: System Architecture Evolution
- **SGW**: Serving Gateway
- **VoLTE**: Voice over LTE
