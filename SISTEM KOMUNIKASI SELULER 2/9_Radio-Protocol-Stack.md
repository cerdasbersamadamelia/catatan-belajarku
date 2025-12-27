# 9. Radio Protocol Stack LTE

## 1. Pengenalan Protocol Stack LTE

Protocol stack adalah **aturan yang mengatur komunikasi** antara perangkat di jaringan LTE. Seperti tata tertib yang harus diikuti agar perangkat bisa saling berkomunikasi dengan baik.

### Fungsi Utama Protocol Stack:

- Mengatur aliran data dan sinyal
- Memastikan koneksi stabil antara perangkat
- Mengelola pembagian kapasitas untuk user

---

## 2. Arsitektur Dasar LTE

### Komponen Utama:

1. **EPC (Evolved Packet Core)** - Inti jaringan

   - MME (kontrol signaling)
   - SGW & PGW (jalur data)
   - HSS (database pelanggan)
   - PCRF (billing & charging)

2. **E-UTRAN (Radio Access)**
   - eNodeB (Base Station/BTS)

### Perbedaan dengan 3G:

- **4G**: UE bisa langsung komunikasi ke MME dan SGW
- **3G**: Harus melalui RNC dulu

---

## 3. Dua Jalur Komunikasi

Protocol stack LTE membagi komunikasi menjadi 2 jalur terpisah:

### A. Control Plane (Jalur Kontrol/Signaling)

**Fungsi**: Mengatur koneksi dan sinyal

- **Jalur**: UE → eNodeB → MME
- **Protocol**: NAS (Non-Access Stratum)
- **Tugas**:
  - Membangun koneksi awal
  - Autentikasi user
  - Manajemen mobilitas
  - Memastikan device dapat sinyal

### B. User Plane (Jalur Data)

**Fungsi**: Membawa traffic data pengguna

- **Jalur**: UE → eNodeB → SGW → PGW
- **Protocol**: IP (UDP/TCP)
- **Tugas**:
  - Transfer data internet
  - Streaming, download, upload
  - Voice over LTE

**Analogi**: Control Plane seperti petugas yang mengatur traffic, User Plane seperti jalanan yang dilalui kendaraan.

---

## 4. Layer Protocol Stack

Komunikasi di LTE melewati **6 layer** yang dikelompokkan menjadi 3 kategori besar:

### Pengelompokan Layer:

1. **Physical Layer** (Layer 1)
2. **Data Link Layer** (Layer 2-4): MAC, RLC, PDCP
3. **Network/Radio Layer** (Layer 5-6): RRC/IP, NAS

```
Layer 6: NAS (Non-Access Stratum)
Layer 5: RRC (Radio Resource Control) atau IP/Application
Layer 4: PDCP (Packet Data Convergence Protocol)
Layer 3: RLC (Radio Link Control)
Layer 2: MAC (Medium Access Control)
Layer 1: Physical Layer
```

### Layer 1: Physical Layer

**Fungsi**: Transmisi sinyal radio fisik

**Proses yang terjadi**:

- **Link Adaptation**: Menyesuaikan modulasi (QPSK, 16QAM, 64QAM, 256QAM)
- **Power Control**: Mengatur daya transmisi agar efisien
  - **Penting**: Power control adalah pembeda utama antara Open RAN dan RAN tradisional
  - Open RAN lebih efisien dalam power consumption
  - RAN tradisional konsumsi daya lebih tinggi
- **Cell Selection**: Menentukan user masuk ke cell mana
- **OFDM & SC-FDM**: Teknologi multiplexing untuk downlink dan uplink

**Modulasi Default**: QPSK

- Dipakai untuk coverage luas (frekuensi 900 MHz)
- Paling "jelek" tapi jangkauan paling luas
- 1 simbol = 2 bit
- Semakin dekat BTS → modulasi lebih tinggi (64/256 QAM) → kecepatan lebih tinggi

### Layer 2: MAC (Medium Access Control)

**Fungsi**: Transport channel dan logical channel

**Tugas Utama**:

1. **Multiplexing/Demultiplexing**: Menggabung/memisah data dari berbagai sumber
2. **Scheduling**: Mengatur giliran pengiriman data (siapa duluan)
3. **HARQ (Hybrid ARQ)**: Koreksi error otomatis
   - Stop-and-Wait ARQ
   - Go-Back-N ARQ
   - Selective Reject ARQ
4. **Priority Handling**: Mengatur prioritas layanan

### Layer 3: RLC (Radio Link Control)

**Fungsi**: Logical channel control

**3 Mode Operasi**:

1. **Transparent Mode**: Data langsung lewat tanpa header tambahan
2. **Unacknowledged Mode**: Kirim data tanpa konfirmasi penerimaan
3. **Acknowledged Mode**: Kirim data dengan konfirmasi (lebih reliable)

**Tugas**: Memastikan data sampai dengan benar dan menentukan mode transfer sesuai kebutuhan.

### Layer 4: PDCP (Packet Data Convergence Protocol)

**Fungsi**: Membawa data radio dan memetakan ke application

**Tugas Utama**:

- **Header Compression**: Mengecilkan ukuran header data
- **Data Transfer**: Mengirim data ke layer aplikasi
- **Mapping**: Menentukan data masuk ke SRB atau DRB
- **Security**: Enkripsi data

### Layer 5: RRC (Radio Resource Control)

**Fungsi**: Mengatur sumber daya radio antara UE dan eNodeB

**Hanya ada di Control Plane** (bukan di User Plane)

**Tugas**:

- Membangun koneksi radio (RRC Connection)
- Broadcast informasi sistem
- Paging
- Mobility control (handover)
- QoS management

### Layer 6: NAS (Non-Access Stratum)

**Fungsi**: Komunikasi langsung UE ↔ MME (untuk control) atau UE ↔ PGW (untuk data)

**Karakteristik**:

- **Tidak melewati eNodeB** (langsung tembus ke core)
- Mengatur fungsi:
  - Autentikasi
  - Manajemen mobilitas
  - Session management

---

## 5. Bearer (Jalur Virtual)

Bearer adalah **jalur virtual khusus** yang membawa data/sinyal dengan karakteristik tertentu.

### A. Signaling Radio Bearer (SRB)

**Untuk**: Membawa sinyal kontrol (RRC dan NAS signaling)

**3 Jenis**:

1. **SRB0**: Setup awal RRC connection sebelum security diaktifkan
2. **SRB1 (High Priority)**:
   - Wajib diberikan ke SEMUA user
   - Mau pakai data atau tidak, HARUS dapat
   - Memastikan device dapat sinyal
   - Membawa RRC dan NAS signaling
3. **SRB2 (Lower Priority)**:
   - Diberikan saat user perlu koneksi NAS
   - Untuk aktivitas tertentu (logged measurement, NAS messages)
   - Hanya diberi jika dibutuhkan

### B. EPS Bearer (Data Bearer)

**Untuk**: Membawa traffic data user

**2 Kategori**:

#### 1. Default Bearer

- **Otomatis diberikan** ke semua pelanggan
- Mau dipakai atau tidak, tetap diberikan
- Tipe: **Non-GBR (Non-Guarantee Bit Rate)**
- Tidak ada jaminan kecepatan tetap
- Bisa dishare dengan user lain jika tidak dipakai

#### 2. Dedicated Bearer

- **Diberikan sesuai kebutuhan** dan paket langganan
- 2 Tipe:
  - **GBR (Guarantee Bit Rate)**: Ada jaminan kecepatan minimum
    - Contoh: VoLTE, Video Call, Real-time Gaming
  - **Non-GBR**: Tanpa jaminan kecepatan
    - Contoh: Browsing, Email, Streaming video biasa

### QoS Class Identifier (QCI)

Operator mengidentifikasi layanan dengan QCI Priority:

- **Priority 1-4**: Real-time services (VoLTE, gaming)
- **Priority 5-9**: Non-real-time (browsing, streaming, social media)

### Traffic Flow Template (TFT)

**TFT** adalah template yang digunakan operator untuk:

- **Mengidentifikasi jenis traffic** user (browsing, streaming, gaming, VoLTE)
- **Memetakan traffic ke QCI** yang sesuai
- **Menentukan prioritas** berdasarkan profil pelanggan
- **Mendeteksi pola penggunaan** user

**Contoh penggunaan TFT**:

- User beli paket prabayar → TFT mapping ke Non-GBR, priority rendah
- User pascabayar premium → TFT mapping ke GBR, priority tinggi
- User buka aplikasi VoLTE → TFT otomatis mapping ke QCI 1 (highest priority)
- User streaming video → TFT mapping ke QCI 7-9

Operator bisa tahu:

- User A: jarang pakai kuota, mostly WiFi → bisa dikasih priority rendah
- User B: heavy user, download banyak → butuh capacity lebih
- Area X: traffic tinggi jam 7-9 malam → perlu capacity planning

---

## 6. Proses Koneksi Setup

### Tahap 1: RRC Connection (Control Plane)

**Tujuan**: Membangun jalur signaling

**Proses**:

1. **Physical Layer Sync**: Sinkronisasi sinyal
2. **MAC Configuration**: Setup channel transport
3. **RLC Configuration**: Setup logical channel
4. **PDCP Configuration**: Setup transfer data
5. **RRC Connection Established**: Koneksi signaling terbentuk
6. **User dapat SRB1** (high priority signal)

### Tahap 2: Initial Context Setup

**Tujuan**: Setup jalur ke core network

**Proses**:

1. **Cell Selection**: Pilih cell terbaik
2. **Authentication**: Verifikasi identitas user di HSS
3. **IP Address Allocation**: User dapat alamat IP
4. **SGW Selection**: Pilih SGW terdekat
5. **S1 Interface Build**: Bangun jalur eNodeB ↔ SGW
6. **S5/S8 Interface Build**: Bangun jalur SGW ↔ PGW

### Tahap 3: EPS Bearer Setup (User Plane)

**Tujuan**: Membangun jalur data untuk traffic

**Proses**:

1. **RRC Reconfiguration**: Setup ulang RRC untuk data
2. **Radio Bearer Build**: Buat jalur radio untuk data
3. **User dapat Default Bearer** (non-GBR)
4. **Dedicated Bearer** (jika perlu sesuai QoS)

**Catatan Penting**:

- Layer 1-5 dibangun **2 KALI**:
  - **Pertama**: Untuk Control Plane (signaling)
  - **Kedua**: Untuk User Plane (data)
- Jalur berbeda tapi layer sama!

---

## 7. Interface Antar Komponen

### Radio Interface

- **LTE-Uu**: Antara UE dan eNodeB (wireless)

### Core Network Interface

- **S1-MME**: eNodeB ↔ MME (untuk signaling)
- **S1-U**: eNodeB ↔ SGW (untuk data)
- **S5/S8**: SGW ↔ PGW (untuk data)
- **S11**: MME ↔ SGW (untuk kontrol)

### EPS Bearer Architecture (End-to-End)

**EPS Bearer** terdiri dari 3 segmen yang terhubung:

```
UE ←→ eNodeB ←→ SGW ←→ PGW
   [Radio Bearer] [S1 Bearer] [S5/S8 Bearer]
```

1. **Radio Bearer**:

   - Antara UE dan eNodeB
   - Melalui interface LTE-Uu (wireless)
   - Menggunakan layer Physical → MAC → RLC → PDCP

2. **S1 Bearer**:

   - Antara eNodeB dan SGW
   - Melalui interface S1-U
   - Menggunakan GTP-U protocol
   - Membawa user data dari radio ke core

3. **S5/S8 Bearer**:
   - Antara SGW dan PGW
   - Melalui interface S5 (sama operator) atau S8 (beda operator/roaming)
   - Menggunakan GTP-U protocol
   - Membawa data menuju internet/eksternal network

**Catatan**: Ketiga bearer ini membentuk **satu EPS Bearer end-to-end** yang bersifat virtual dan bisa dishare antar user.

---

## 8. SDU vs PDU

Dalam pemrosesan data di setiap layer:

- **SDU (Service Data Unit)**: Data yang **diterima** oleh layer dari layer atasnya
- **PDU (Protocol Data Unit)**: Data yang **keluar** dari layer ke layer bawahnya

**Proses**:

- SDU masuk → ditambah Header → jadi PDU → dikirim ke layer bawah
- Layer bawah terima sebagai SDU → proses → tambah header → jadi PDU lagi
- **Enkapsulasi**: Semakin ke bawah, header makin banyak
- Di penerima: Header dikurangi tiap naik layer (dekapsulasi)

---

## 9. RRC State (Status Koneksi)

User di LTE bisa dalam 2 kondisi:

### 1. RRC_IDLE (Idle State)

**Kondisi**: User tidak melakukan aktivitas

- Tidak kirim/terima data
- TETAP dapat **SRB1** (high priority signal)
- Handset standby, dapat sinyal
- Bisa terima SMS/telepon kapan saja
- Hemat baterai

### 2. RRC_CONNECTED (Connected State)

**Kondisi**: User sedang aktif berkomunikasi

- Sedang browsing, download, telepon, dll
- Dapat **SRB1 + SRB2 + Default Bearer + Dedicated Bearer**
- eNodeB tahu posisi user secara detail
- Konsumsi baterai lebih tinggi

---

## 10. Virtual Connection

**EPS Bearer = Virtual Connection**

Artinya:

- Jalur data bersifat **virtual/logis**, bukan fisik dedicated
- Satu jalur fisik bisa dipakai **banyak user secara bersamaan**
- Selama kapasitas cukup, bisa dishare
- Operator tahu profil user dari traffic pattern

**Contoh**:

- Kamu kirim email ke Gmail → dapat jalur virtual
- Temanmu streaming YouTube → dapat jalur virtual yang sama (berbagi)
- Selama kapasitas site cukup → semua lancar

Operator bisa deteksi:

- User jarang pakai kuota → dikasih prioritas rendah
- User sering pakai banyak kuota → prioritas tinggi
- Area ramai user aktif → tambah site/kapasitas

---

## 11. Planning & Capacity

### Frekuensi LTE di Indonesia

- **900 MHz**: Coverage luas, bandwidth 5-7.5 MHz (untuk daerah 3T)
- **1800 MHz**: Standard, bandwidth 20 MHz
- **2100 MHz**: Bandwidth 15 MHz
- **2300 MHz**: Bandwidth 30 MHz (paling lebar)

### Bandwidth & PRB

- **1.4 MHz** → 6 PRB
- **3 MHz** → 15 PRB
- **5 MHz** → 25 PRB (default untuk frekuensi 900)
- **10 MHz** → 50 PRB
- **15 MHz** → 75 PRB
- **20 MHz** → 100 PRB

### Prinsip Planning

- Frekuensi rendah (900) → Coverage luas, kapasitas kecil
- Frekuensi tinggi (2300) → Coverage sempit, kapasitas besar
- Planning harus hitung: jumlah user, kebutuhan kapasitas, coverage area
- Default planning pakai **QPSK** (coverage paling aman)

### Contoh Kasus: Planning untuk Daerah 3T

**Telkomsel - Implementasi di daerah 3T**:

- Gunakan **frekuensi 900 MHz** (coverage luas)
- Bandwidth yang dialokasikan: **5 MHz** (karena 900 MHz juga dipakai 2G/3G)
- PRB yang tersedia: **25 PRB**
- Modulasi default: **QPSK**

**Pertimbangan**:

- Daerah 3T = populasi jarang, area luas
- Butuh coverage luas, kapasitas tidak terlalu tinggi
- 900 MHz cocok karena jangkauan jauh
- Trade-off: bandwidth kecil tapi coverage maksimal

**Tools Perancangan**:

- Excel (dari vendor seperti Huawei)
- Mobile application untuk perhitungan
- Software untuk kalkulasi capacity & coverage
- Link budget calculation

---

## 12. Perbedaan Layer di Control Plane vs User Plane

| Layer | Control Plane | User Plane     |
| ----- | ------------- | -------------- |
| L6    | NAS (ke MME)  | NAS (ke PGW)   |
| L5    | RRC           | IP/Application |
| L4    | PDCP          | PDCP           |
| L3    | RLC           | RLC            |
| L2    | MAC           | MAC            |
| L1    | Physical      | Physical       |

**Perbedaan**:

- Control Plane ada **RRC** di layer 5
- User Plane ada **IP/Application** di layer 5
- Control Plane pakai **SCTP** (Stream Control Transmission Protocol)
  - Protocol yang reliable seperti TCP
  - Mendukung multi-streaming
  - Dipakai karena signaling harus dijamin sampai
- User Plane bisa pakai **UDP** (cepat) atau **TCP** (reliable)
  - UDP untuk real-time (VoLTE, gaming) - cepat tapi no guarantee
  - TCP untuk browsing, download - reliable tapi lebih lambat

---

## 13. Kesimpulan

### Protocol Stack LTE itu:

- ✅ Mengatur komunikasi dengan **aturan berlapis** (6 layer)
- ✅ Pisahkan **jalur kontrol** (signaling) dan **jalur data** (traffic)
- ✅ Setiap user dapat **SRB** (sinyal) dan **EPS Bearer** (data)
- ✅ Default Bearer wajib diberikan, Dedicated Bearer sesuai kebutuhan
- ✅ Koneksi virtual → bisa dishare antar user
- ✅ Operator bisa kontrol QoS sesuai profil pelanggan

### Flow Komunikasi:

1. **RRC Connection** → dapat sinyal (SRB1)
2. **Initial Setup** → autentikasi, dapat IP
3. **Bearer Setup** → dapat jalur data (Default + Dedicated)
4. **Mulai komunikasi** → browsing, streaming, call

### Tips Mudah Ingat:

- **Layer 1-3**: Fokus ke radio (physical, MAC, RLC)
- **Layer 4-5**: Fokus ke data & kontrol (PDCP, RRC/IP)
- **Layer 6**: Langsung ke core (NAS)
- **Control dulu, baru Data** → signaling dibangun duluan!

---

**Catatan Tambahan**: Materi ini adalah dasar untuk memahami perancangan jaringan LTE. Untuk implementasi real (seperti desain Open RAN, perhitungan capacity, coverage planning) akan dipelajari di sesi berikutnya dengan tools dan kalkulasi detail.
