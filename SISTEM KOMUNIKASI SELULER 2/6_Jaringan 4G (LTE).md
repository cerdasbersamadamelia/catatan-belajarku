# Jaringan 4G (LTE)

## Pengenalan 4G LTE

LTE (Long Term Evolution) adalah teknologi 4G yang dikeluarkan oleh 3GPP. Teknologi ini merupakan evolusi besar karena **didesain khusus untuk menangani data berbasis IP**.

### Karakteristik Utama
- **Fokus utama**: Komunikasi data
- **Berbasis IP**: Semua komunikasi menggunakan protokol IP
- **Switching**: Murni menggunakan **Packet Switching (PS)**
  - Beda dengan 3G yang masih pakai Circuit Switching (CS) untuk voice dan Packet Switching (PS) untuk data
  - 4G LTE sudah 100% Packet Switching

## Penanganan Voice di 4G

Karena 4G dirancang untuk data, bagaimana cara menangani panggilan suara (voice)? Ada 2 solusi:

### 1. VoLTE (Voice over LTE)
- Voice ditransmisikan melalui jaringan 4G
- Menggunakan perangkat **IMS (IP Multimedia Subsystem)**
- Digunakan ketika jaringan 4G bersifat **Stand Alone**

**Stand Alone** artinya:
- Infrastruktur murni 4G dari ujung ke ujung
- Radio Access Network (RAN) → 4G
- Core Network → 4G
- Tidak bergantung pada 3G atau 2G

### 2. CSFB (Circuit Switching Fallback)
- Voice menggunakan teknologi lama (3G atau 2G)
- Digunakan ketika jaringan 4G bersifat **Non-Stand Alone**

**Non-Stand Alone** artinya:
- Radio Access Network (RAN) → 4G
- Core Network → Masih menggunakan 3G atau 2G
- Sistem "pinjam" infrastruktur lama untuk voice

### 3. SRVCC (Single Radio Voice Call Continuity)
- Teknologi handover dari VoLTE ke 2G/3G
- Ketika user bergerak ke area tanpa coverage VoLTE
- Panggilan tetap berlanjut tanpa terputus
- Smooth transition dari 4G ke 3G/2G saat call berlangsung

## Arsitektur Jaringan 4G LTE

### Komponen Utama

**1. UE (User Equipment)**
- Perangkat pengguna (HP)
- Sama seperti istilah di 3G

**2. eNodeB (evolved NodeB)**
- Base station yang memancarkan gelombang radio
- Menggantikan NodeB di 3G

**Keunggulan eNodeB:**
- Bisa komunikasi langsung dengan eNodeB lain menggunakan interface **X2**
- Di 3G, NodeB harus melalui RNC dulu untuk berkomunikasi dengan NodeB lain
- Komunikasi langsung ini membuat jaringan lebih cepat

**3. MME (Mobility Management Entity)**
- Mengatur signaling (pengaturan koneksi)
- Tidak membawa data user
- Menentukan jalur yang akan dilewati data
- Interface ke MME: garis putus-putus (signaling only)

**4. SGW (Serving Gateway)**
- Menangani data user
- Gateway pertama setelah eNodeB

**5. PGW (PDN Gateway)**
- Gateway ke jaringan eksternal
- Menangani koneksi ke internet

### Alur Komunikasi
```
           Data Plane (User Traffic)
UE ← → eNodeB ← → SGW ← → PGW ← → Internet
  ↓         ↓              ↓         ↑
  └───→ MME ←──────────────┘         │
        ↓                            │
       HSS                          PCRF
      
Control Plane (Signaling)
```

### Interface LTE
- **LTE-Uu**: UE ↔ eNodeB (air interface/radio)
- **X2**: eNodeB ↔ eNodeB (handover, load balancing)
- **S1-U**: eNodeB ↔ SGW (user data)
- **S1-MME**: eNodeB ↔ MME (signaling)
- **S11**: MME ↔ SGW (session management)
- **S5/S8**: SGW ↔ PGW (roaming)
- **SGi**: PGW ↔ Internet/External Networks

### Penamaan Arsitektur

**E-UTRAN (Evolved UMTS Terrestrial Radio Access Network)**
- Bagian Radio Access Network (RAN)
- Terdiri dari: eNodeB

**EPC (Evolved Packet Core)**
- Bagian Core Network
- Terdiri dari: MME, SGW, PGW, HSS, PCRF

**EPS (Evolved Packet System)**
- Nama lengkap sistem 4G LTE
- EPS = E-UTRAN + EPC

### Komponen EPC Tambahan

**6. HSS (Home Subscriber Server)**
- Database pelanggan
- Menyimpan informasi profile user, autentikasi, lokasi
- Mirip dengan HLR di 2G/3G

**7. PCRF (Policy and Charging Rules Function)**
- Mengatur QoS (Quality of Service)
- Mengatur charging/billing
- Menentukan prioritas data dan bandwidth per user

## Frekuensi 4G di Indonesia

LTE di Indonesia menggunakan berbagai band frekuensi:

### Band Frekuensi LTE di Indonesia

**1. LTE 900 MHz (Band 8)**
- Refarming dari frekuensi GSM 900
- Coverage luas (penetrasi bagus)
- Kecepatan sedang
- Operator: Telkomsel, Indosat, XL

**2. LTE 1800 MHz (Band 3)**
- Frekuensi utama LTE di Indonesia
- Balance antara coverage dan kecepatan
- Paling banyak digunakan operator
- Operator: Semua operator utama

**3. LTE 2100 MHz (Band 1)**
- Refarming dari frekuensi 3G
- Coverage lebih kecil dari 1800 MHz
- Kecepatan lebih tinggi
- Operator: Telkomsel, Indosat, XL

**4. LTE 2300 MHz (Band 40) - TDD**
- Untuk data kecepatan tinggi
- Coverage terbatas (urban area)
- Menggunakan TDD (Time Division Duplex)
- Operator: Bolt, Smartfren

**Prinsip Frekuensi:**
- Frekuensi rendah (900 MHz) → Coverage luas, kecepatan rendah
- Frekuensi tinggi (2300 MHz) → Coverage sempit, kecepatan tinggi

## Modulasi dan Teknik Akses 4G LTE

### Teknik Akses Radio
LTE menggunakan teknik akses yang berbeda untuk downlink dan uplink:

**Downlink (Base Station → UE):**
- **OFDMA (Orthogonal Frequency Division Multiple Access)**
- Membagi frekuensi menjadi sub-carrier kecil yang orthogonal
- Setiap user dapat menggunakan beberapa sub-carrier secara bersamaan
- Efisien untuk kecepatan tinggi dan mengatasi multipath fading

**Uplink (UE → Base Station):**
- **SC-FDMA (Single Carrier Frequency Division Multiple Access)**
- Varian dari OFDMA yang lebih hemat daya
- PAPR (Peak to Average Power Ratio) lebih rendah → hemat baterai UE

### Modulasi LTE

LTE tidak menggunakan kombinasi modulasi, tapi **adaptif** memilih satu modulasi terbaik sesuai kondisi sinyal:

1. **QPSK (Quadrature Phase Shift Keying)**
   - 2 bits per symbol
   - Digunakan saat sinyal lemah
   - Paling robust tapi kecepatan rendah

2. **16-QAM (16 Quadrature Amplitude Modulation)**
   - 4 bits per symbol
   - Digunakan saat sinyal sedang
   - Balance antara kecepatan dan reliabilitas

3. **64-QAM**
   - 6 bits per symbol
   - Digunakan saat sinyal kuat
   - Kecepatan tinggi tapi butuh sinyal bagus

4. **256-QAM** (LTE-Advanced)
   - 8 bits per symbol
   - Hanya di LTE-Advanced
   - Kecepatan tertinggi, butuh sinyal sangat kuat

Semakin tinggi tingkat QAM, semakin cepat transfer datanya tapi makin sensitif terhadap gangguan.

## Bandwidth dan Kecepatan LTE

### Channel Bandwidth
LTE mendukung berbagai lebar bandwidth:
- 1.4 MHz, 3 MHz, 5 MHz, 10 MHz, 15 MHz, 20 MHz
- Semakin lebar bandwidth, semakin tinggi kecepatan maksimal

### Kecepatan Teoritis
**LTE Category (Cat):**
- **Cat 3**: Download 100 Mbps, Upload 50 Mbps (minimum standar)
- **Cat 4**: Download 150 Mbps, Upload 50 Mbps (umum di Indonesia)
- **Cat 6**: Download 300 Mbps, Upload 50 Mbps (LTE-Advanced)
- **Cat 9**: Download 450 Mbps, Upload 50 Mbps
- **Cat 16**: Download 1 Gbps (LTE-Advanced Pro)

### Kecepatan Real di Indonesia
- Rata-rata: 10-30 Mbps (download)
- Peak: 50-100 Mbps (tergantung lokasi dan bandwidth operator)
- Latency: 30-50 ms (lebih rendah dari 3G yang ~100 ms)

## Keunggulan 4G LTE

1. **Arsitektur flat (lebih sederhana)**
   - Tidak ada RNC seperti di 3G
   - eNodeB punya fungsi baseband dan control
   - Mengurangi latency

2. **Kecepatan lebih tinggi**
   - Teknologi OFDMA lebih efisien
   - Modulasi hingga 256-QAM
   - Mendukung MIMO (Multiple Input Multiple Output)

3. **Latency rendah**
   - 3G: ~100 ms
   - LTE: ~30-50 ms
   - Penting untuk gaming dan video call

4. **All-IP Network**
   - Semua traffic menggunakan paket IP
   - Lebih efisien untuk data internet

5. **Handover lebih cepat**
   - Interface X2 memungkinkan handover langsung antar eNodeB
   - Tanpa melalui core network

## Refarming Frekuensi

### Apa itu Refarming?
Refarming adalah **penataan ulang frekuensi** untuk optimasi jaringan.

### Kondisi Sebelum Refarming
Frekuensi operator tersebar tidak teratur:
- XL → Indosat → Telkomsel → XL → Telkomsel → ...
- Tidak efisien dan mengganggu kualitas sinyal

### Kondisi Setelah Refarming (di Frekuensi 1800 MHz)
Frekuensi operator tertata rapi dengan bandwidth masing-masing:
- **Telkomsel**: 15 MHz
- **Indosat**: 15 MHz
- **XL**: 10 MHz
- **Tri**: 15 MHz (setelah merger dengan Indosat)
- **Smartfren**: 10 MHz (di 850 MHz)

### Tujuan Refarming
- Meningkatkan kinerja jaringan
- Mengurangi interferensi antar operator
- Optimasi transmisi data ke user

## LTE-Advanced dan Carrier Aggregation

### LTE-Advanced (LTE-A)
- Upgrade dari LTE standar
- Release 10 dari 3GPP
- Disebut juga "True 4G"
- Kecepatan hingga 1 Gbps (download)

### Carrier Aggregation (CA)
**Konsep:**
Menggabungkan beberapa frekuensi (carrier) untuk meningkatkan kecepatan.

**Contoh di Indonesia:**
- CA 2 Band: LTE 900 MHz + LTE 1800 MHz
- CA 3 Band: LTE 900 + LTE 1800 + LTE 2100
- Bandwidth dijumlahkan untuk kecepatan lebih tinggi

**Keuntungan CA:**
- Kecepatan download lebih tinggi
- Penggunaan spektrum lebih efisien
- Kualitas koneksi lebih stabil

### MIMO (Multiple Input Multiple Output)
- Menggunakan multiple antena di transmitter dan receiver
- **2x2 MIMO**: 2 antena transmit, 2 antena receive (umum di LTE)
- **4x4 MIMO**: 4 antena transmit, 4 antena receive (LTE-Advanced)
- Meningkatkan throughput dan coverage

## Prosedur Penting di LTE

### 1. Attach Procedure
Prosedur UE untuk terhubung ke jaringan LTE pertama kali:
1. UE scan frekuensi dan cari eNodeB
2. UE kirim **Attach Request** ke MME (via eNodeB)
3. MME cek autentikasi ke HSS
4. HSS validasi subscriber
5. MME setup **Default Bearer** (koneksi data default)
6. MME kirim **Attach Accept** ke UE
7. UE terhubung ke jaringan dan dapat IP address

### 2. Handover
Perpindahan UE dari satu eNodeB ke eNodeB lain:

**X2 Handover (antar eNodeB sama operator):**
1. Source eNodeB deteksi signal quality menurun
2. Source eNodeB kirim **Handover Request** ke Target eNodeB via X2
3. Target eNodeB kirim **Handover Acknowledge**
4. UE pindah ke Target eNodeB
5. Path switching di SGW (update jalur data)

**S1 Handover (antar eNodeB beda operator atau tanpa X2):**
1. Handover melalui MME
2. Lebih lambat dari X2 Handover
3. Digunakan saat X2 tidak tersedia

### 3. Paging
Cara jaringan mencari UE untuk panggilan masuk atau SMS:
1. MME kirim **Paging Message** ke beberapa eNodeB di Tracking Area
2. eNodeB broadcast paging ke semua UE di area coverage
3. UE yang dipanggil kirim **Paging Response**
4. Koneksi dibangun antara UE dan jaringan

### 4. Tracking Area Update (TAU)
- UE berpindah antar Tracking Area
- Update lokasi UE ke MME
- MME selalu tahu posisi UE untuk paging

## Bearer di LTE

### Apa itu Bearer?
Bearer adalah **jalur virtual** untuk membawa data antara UE dan PGW.

### Jenis Bearer:

**1. Default Bearer**
- Dibuat saat UE attach ke jaringan
- Selalu aktif selama UE terkoneksi
- QoS standar (best effort)
- Untuk browsing normal, chat, dll

**2. Dedicated Bearer**
- Dibuat untuk layanan khusus yang butuh QoS tinggi
- Contoh: VoLTE, video streaming, gaming
- Bisa di-setup atau di-release sesuai kebutuhan
- QoS lebih tinggi dengan prioritas tertentu

### QoS Class Identifier (QCI)
Mengatur prioritas dan karakteristik bearer:
- **QCI 1**: Voice (VoLTE) - prioritas tertinggi, delay rendah
- **QCI 5**: IMS signaling
- **QCI 9**: Internet browsing - best effort
- Semakin kecil angka QCI, semakin tinggi prioritas

## TDD vs FDD di LTE

### FDD (Frequency Division Duplex)
- **Downlink dan uplink menggunakan frekuensi berbeda**
- Contoh: Band 1800 MHz → DL: 1805-1880 MHz, UL: 1710-1785 MHz
- Paling umum digunakan di Indonesia
- Performa stabil untuk uplink dan downlink

### TDD (Time Division Duplex)
- **Downlink dan uplink menggunakan frekuensi sama, tapi waktu berbeda**
- Contoh: Band 2300 MHz (TDD)
- Downlink dan uplink bergantian dalam waktu (time slot)
- Lebih fleksibel untuk alokasi bandwidth DL/UL
- Digunakan Bolt dan Smartfren di Indonesia

**Perbedaan:**
| Aspek | FDD | TDD |
|-------|-----|-----|
| Spektrum | Butuh 2x frekuensi (paired) | 1x frekuensi (unpaired) |
| Kompleksitas | Lebih kompleks (duplexer) | Lebih sederhana |
| Flexibility | Fixed ratio DL/UL | Flexible ratio DL/UL |
| Coverage | Lebih baik | Lebih terbatas |

## Evolusi Teknologi Seluler

Ringkasan perkembangan dari 2G hingga 4G:

- **2G (GSM)** 
- → **2.5G (GPRS)**: + SGSN & GGSN untuk data 
- → **2.75G (EDGE)**: + software peningkatan kecepatan 
- → **3G (WCDMA)**: Arsitektur baru (NodeB, RNC, MSC/SGSN/GGSN) 
- → **3.5G (HSDPA)**: + software HSDPA untuk kecepatan lebih tinggi 
- → **4G (LTE)**: Arsitektur simpel, murni packet switching, fokus data
- → **4.5G (LTE-Advanced)**: Carrier Aggregation, MIMO 4x4, kecepatan 1 Gbps

Setiap generasi membawa peningkatan signifikan dalam kecepatan dan efisiensi jaringan!

## Tantangan dan Optimasi LTE

### Tantangan Operator:
1. **Coverage**: Frekuensi tinggi = coverage terbatas
2. **Kapasitas**: Banyak user = koneksi lambat
3. **Interferensi**: Antar operator atau antar cell
4. **Handover failure**: Panggilan terputus saat berpindah cell

### Teknik Optimasi:

**1. Self-Organizing Network (SON)**
- Jaringan mengoptimasi diri secara otomatis
- Auto-tuning parameter eNodeB
- Mengurangi intervensi manual

**2. Load Balancing**
- Distribusi user ke cell yang tidak ramai
- Menggunakan interface X2
- Meningkatkan throughput per user

**3. Inter-Cell Interference Coordination (ICIC)**
- Koordinasi penggunaan resource antar cell
- Mengurangi interferensi di cell edge
- Meningkatkan kualitas sinyal user di pinggir cell

**4. Coverage Optimization**
- Adjusting antenna tilt dan azimuth
- Penggunaan repeater atau small cell
- Carrier Aggregation untuk extend coverage

---

## 🎯 Fun Facts 4G LTE

**1. LTE itu bukan "True 4G"!**
- ITU (organisasi telekomunikasi dunia) definisi 4G: minimal 1 Gbps
- LTE standar cuma 100-150 Mbps
- Makanya disebut "Long Term Evolution" (evolusi menuju 4G)
- Yang bener-bener 4G itu LTE-Advanced (1 Gbps)

**2. Instagram, TikTok, Netflix? Semua karena 4G!**
- Sebelum 4G, streaming video itu mimpi buruk
- 3G cuma 2-3 Mbps, buffer terus!
- 4G bikin era "mobile video" jadi nyata

**3. "4G" di HP kamu belum tentu 4G beneran**
- Kadang cuma 3.5G (HSPA+) yang di-label "4G"
- Marketing operator suka "mempercantik" label
- Cek di *Setting* → simbol LTE = baru beneran 4G

**4. Kenapa 4G lebih boros baterai?**
- OFDMA dan multiple antenna (MIMO) butuh power besar
- Data terus-terusan sync (push notification, auto-update)
- Kecepatan tinggi = prosesor kerja lebih keras

**5. 4G pertama di Indonesia: 2014**
- Telkomsel launch duluan dengan brand "4G LTE"
- Awalnya cuma di kota besar (Jakarta, Surabaya, dll)
- Sekarang? Coverage hampir 95% populasi

**6. Ping/Latency turun drastis: 100ms → 30ms**
- Game online mobile jadi booming
- Mobile Legends, PUBG, dll baru bisa enak dimainkan
- Video call jadi smooth, bukan kayak robot lagi

**7. Carrier Aggregation = "Jalan Tol Express"**
- Gabungin 2-3 frekuensi sekaligus
- Kayak buka 3 jalur tol buat 1 mobil
- Kecepatan bisa 200-300 Mbps!

**8. eNodeB lebih "smart" dari NodeB**
- Bisa ngatur sendiri tanpa kontrol pusat
- Handover antar tower lebih cepat
- Makanya seamless pas pindah area

**9. VoLTE = Voice jadi paket data!**
- Panggilan telepon dikirim kayak WhatsApp call
- Kualitas suara HD (16 kHz vs 8 kHz di 2G/3G)
- Bisa browsing sambil telepon (dulu gak bisa!)

**10. 4G bikin "IoT" jadi mungkin**
- Smartwatch, smart home, mobil connected
- Latency rendah + bandwidth tinggi = real-time control
- Fondasi buat era "everything connected"