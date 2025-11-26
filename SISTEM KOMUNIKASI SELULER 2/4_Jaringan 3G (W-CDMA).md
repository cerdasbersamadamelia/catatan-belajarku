# Jaringan 3G (W-CDMA)

## Perubahan Arsitektur dari 2G ke 3G

Teknologi 3G mengalami **perubahan arsitektur besar-besaran** dibanding 2G, baik di core network maupun radio access network.

### Perubahan Penamaan Komponen

| 2G (GSM/GPRS) | 3G (W-CDMA) | Fungsi |
|---------------|-------------|--------|
| MS (Mobile Station) | UE (User Equipment) | Perangkat pengguna (HP) |
| BTS | Node B | Tower/stasiun pemancar |
| BSC | RNC (Radio Network Controller) | Pengatur stasiun pemancar |
| MSC | MSC | Core network untuk voice |
| SGSN/GGSN | SGSN/GGSN | Core network untuk data |

**Konsekuensi**: HP 2G tidak bisa dipakai di jaringan 3G karena teknologinya berbeda. Tiap generasi butuh HP yang support teknologi tersebut.

---

## Arsitektur Jaringan 3G

```
UE → Node B → RNC → Core Network (MSC atau SGSN/GGSN)
```

### Alur Komunikasi

**Untuk Data/Internet:**
```
UE → Node B → RNC → SGSN → GGSN → Internet
```

**Untuk Voice/Panggilan:**
```
UE → Node B → RNC → MSC → Jaringan Telepon
```

### Interkoneksi dengan 2G

RNC di 3G bisa **langsung terhubung ke MSC di 2G**, memungkinkan:
- Handover antara 3G dan 2G
- Backward compatibility
- Efisiensi infrastruktur

---

## Teknologi yang Digunakan

### 1. Modulasi: QPSK (Quadrature Phase Shift Keying)
- Berubah dari GMSK (2G) dan 8-PSK (EDGE)
- Lebih efisien untuk transfer data kecepatan tinggi
- 4 simbol per fase (2 bit per simbol)

### 2. Multiplexing: W-CDMA (Wideband CDMA)
- **W-CDMA** = Wideband CDMA (bandwidth 5 MHz, lebih lebar dari CDMA2000)
- Semua user pakai frekuensi yang sama secara bersamaan
- Dibedakan dengan **spreading code** unik untuk tiap user
- **Chip rate**: 3.84 Mcps (Mega chip per second)
- **Spreading Factor**: 4 sampai 256 (makin besar SF = kecepatan rendah tapi coverage luas)
- Lebih efisien dibanding TDMA di 2G
- Kapasitas jauh lebih besar, bisa handle lebih banyak user simultan

### 3. Frekuensi di Indonesia: 2100 MHz
- **Band I (2100 MHz)** - paling umum di Indonesia
- Uplink (UE ke Node B): 1920-1980 MHz (60 MHz)
- Downlink (Node B ke UE): 2110-2170 MHz (60 MHz)
- Duplex spacing: 190 MHz (FDD - Frequency Division Duplex)
- Bandwidth per carrier: **5 MHz** (bisa deploy 12 carrier di 60 MHz)
- Channel number: 10562 - 10838

**Operator Indonesia:**
- Telkomsel: 2100 MHz (15 MHz)
- Indosat: 2100 MHz (10 MHz)
- XL Axiata: 2100 MHz (15 MHz)
- Tri (3): 2100 MHz (15 MHz)

---

## Kecepatan Data 3G

3G punya **3 kategori kecepatan** berdasarkan kondisi penggunaan:

### 1. 144 kbps - Rural (Pedesaan)
- Area dengan mobilitas tinggi
- User bergerak cepat (di kendaraan)
- Coverage luas tapi kecepatan rendah
- Jangkauan cell besar (hingga 35 km)

### 2. 384 kbps - Urban Outdoor (Perkotaan Luar Ruangan)
- Area perkotaan, user di luar gedung
- Mobilitas sedang (jalan kaki/kendaraan pelan)
- Cell size sedang (1-5 km)
- Paling umum di implementasi awal

### 3. 2048 kbps (2 Mbps) - Urban Indoor (Perkotaan Dalam Ruangan)
- User di dalam gedung/ruangan
- Mobilitas rendah atau stasioner
- Cell size kecil (microcell/picocell)
- Kecepatan maksimal 3G standar
- Coverage terbatas tapi kualitas tinggi

**Catatan Real Practice:**
- Kecepatan teoritis vs real sangat berbeda
- Real speed 3G basic: 200-500 kbps (urban), 50-150 kbps (rural)
- Real speed HSPA: 1-5 Mbps download, 500 kbps - 1 Mbps upload
- Real speed HSPA+: 5-15 Mbps download (peak 20-25 Mbps)
- Faktor: interferensi, load jaringan, jarak dari tower, kondisi propagasi, device capability

---

## Teknik Switching di 3G

3G menggunakan **dua teknik switching sekaligus**:

### 1. Circuit Switching (CS) - untuk Voice
- Jalur: UE → Node B → RNC → MSC
- Dedicated channel untuk panggilan
- Bandwidth terpakai terus selama panggilan
- Kualitas konsisten, delay rendah

### 2. Packet Switching (PS) - untuk Data
- Jalur: UE → Node B → RNC → SGSN → GGSN
- Bandwidth dibagi banyak user
- Efisien untuk data burst (browsing, email)
- Throughput dinamis

**Kenapa dua teknik?**
Karena 3G adalah generasi transisi yang harus melayani voice (legacy CS) dan data (modern PS). Baru di 4G semua jadi PS (termasuk voice lewat VoLTE).

### Simultaneous Voice + Data
3G bisa **voice dan data bersamaan**!
- User bisa nelpon sambil browsing (CS + PS jalan bersamaan)
- 2G tidak bisa (GPRS suspend saat voice call)
- Ini fitur killer 3G yang bikin user excited dulu

---

## Enhanced 3G: HSPA

Setelah 3G dasar (Release 99), ada peningkatan:

### HSDPA (High Speed Downlink Packet Access) - 3.5G
- Downlink hingga **14.4 Mbps** (teoretis)
- Adaptive Modulation: QPSK, 16-QAM
- Fast scheduling, HARQ (Hybrid ARQ)
- Support streaming video, download cepat

### HSUPA (High Speed Uplink Packet Access) - 3.75G
- Uplink hingga **5.76 Mbps**
- Upload lebih cepat (video call, upload file)

### HSPA+ (Evolved HSPA) - 3.9G
- Downlink hingga **42 Mbps**
- 64-QAM modulation
- MIMO (Multiple Input Multiple Output)
- Carrier aggregation (DC-HSPA+)
- Hampir setara 4G awal

---

## Evolusi dan UMTS Terrestrial Radio Access Network (UTRAN)

### Komponen UTRAN

**1. Node B (Base Station)**
- RF transceiver (Tx/Rx radio)
- Baseband processing (spreading/despreading)
- **Fast Power Control**: 1500 kali/detik (closed loop power control)
- Soft handover capability (bisa connect ke 2-3 Node B sekaligus)
- Channel coding/decoding
- Modulation/demodulation
- RAKE receiver (mengatasi multipath)
- Tidak punya intelligence (semua kontrol di RNC)

**2. RNC (Radio Network Controller)**
- **Radio Resource Management**: alokasi channel, code, power
- **Admission Control**: terima/tolak koneksi baru berdasarkan load
- **Load Control**: mencegah overload cell (congestion control)
- **Handover Control**: 
  - **Soft Handover**: connect ke 2+ Node B sekaligus (seamless)
  - **Softer Handover**: connect ke 2+ sector dalam 1 Node B
  - **Hard Handover**: putus dulu baru connect (ke 2G atau beda freq)
- **Outer Loop Power Control**: adjust target SIR
- **Ciphering**: enkripsi data
- **Serving RNC (SRNC)**: RNC utama yang handle UE
- **Drift RNC (DRNC)**: RNC lain yang assist saat soft handover

**Interface Penting:**
- **Iub**: Node B ↔ RNC (ATM atau IP)
- **Iur**: RNC ↔ RNC (untuk soft handover antar RNC)
- **Iu-CS**: RNC ↔ MSC (untuk circuit switched)
- **Iu-PS**: RNC ↔ SGSN (untuk packet switched)

---

## Channel Types di 3G

3G punya banyak jenis channel untuk fungsi berbeda:

### Logical Channels
**1. Control Channels:**
- **BCCH** (Broadcast Control Channel): info sistem (MCC, MNC, LAC, Cell ID)
- **PCCH** (Paging Channel): paging user untuk incoming call/SMS
- **DCCH** (Dedicated Control Channel): signaling untuk user tertentu
- **CCCH** (Common Control Channel): signaling umum (random access)

**2. Traffic Channels:**
- **DTCH** (Dedicated Traffic Channel): data/voice user (dedicated)
- **CTCH** (Common Traffic Channel): broadcast data (cell broadcast)

### Transport Channels
- **DCH** (Dedicated Channel): dedicated untuk 1 user
- **RACH** (Random Access Channel): initial access
- **FACH** (Forward Access Channel): small data (SMS, paging response)
- **PCH** (Paging Channel): paging
- **BCH** (Broadcast Channel): system info

### Physical Channels
- **DPCH** (Dedicated Physical Channel): channel fisik dedicated
- **CPICH** (Common Pilot Channel): reference signal untuk sync
- **P-CCPCH** (Primary Common Control Physical Channel): BCH
- **S-CCPCH** (Secondary Common Control Physical Channel): FACH, PCH
- **PRACH** (Physical Random Access Channel): RACH

**Penting:** Node B harus broadcast **CPICH** terus (pilot signal). UE detect CPICH untuk cell search dan measurement.

---

## Power Control di 3G

Power control sangat krusial di 3G karena **near-far problem** di CDMA:

### Near-Far Problem
- User dekat tower → sinyal kuat → bisa "mengalahkan" user jauh
- Tanpa power control, user jauh tidak bisa connect
- Solusi: **semua user harus punya power level sama di Node B** (received power)

### 3 Level Power Control:

**1. Open Loop Power Control**
- UE estimate path loss dari CPICH
- Set initial transmit power saat access
- Akurasi rendah (tidak real-time)

**2. Fast Closed Loop Power Control (Inner Loop)**
- **1500 kali/detik** (setiap 0.667 ms = 1 slot)
- Node B ukur SIR (Signal to Interference Ratio)
- Kirim TPC (Transmit Power Control) command: UP atau DOWN (1 bit)
- UE adjust power ±1 dB per step
- Kompensasi fast fading

**3. Outer Loop Power Control**
- RNC adjust target SIR berdasarkan BLER (Block Error Rate)
- Target BLER: 1-10% (tergantung service)
- Kalau BLER tinggi → naikkan target SIR
- Kalau BLER rendah → turunkan target SIR (hemat power)
- Update lambat (setiap 10-100 ms)

**Benefit:**
- Hemat baterai UE
- Kurangi interferensi ke user lain
- Maksimalkan kapasitas cell
- Atasi near-far problem

---

## QoS Classes (Quality of Service)

3G punya **4 QoS classes** untuk layanan berbeda:

### 1. Conversational Class
- **Use case**: Voice call, video call
- **Karakteristik**: Real-time, delay sensitif, symmetric
- **Max delay**: < 150 ms (two-way)
- **Contoh**: Telpon, video conference

### 2. Streaming Class
- **Use case**: Video streaming, audio streaming
- **Karakteristik**: Real-time, one-way, delay toleran sedikit
- **Max delay**: 250 ms - 1 detik
- **Contoh**: YouTube, Spotify (streaming mode)

### 3. Interactive Class
- **Use case**: Web browsing, gaming online
- **Karakteristik**: Request-response, delay moderate
- **Max delay**: 1-4 detik
- **Contoh**: HTTP, HTTPS, FTP browsing

### 4. Background Class
- **Use case**: Email, download, software update
- **Karakteristik**: Non-real-time, delay insensitive
- **Max delay**: Tidak kritis (bisa > 10 detik)
- **Contoh**: Email sync, background download

**Priority**: Conversational > Streaming > Interactive > Background

Node B/RNC schedule resource berdasarkan QoS class. Voice call dapat prioritas tertinggi, background download prioritas terendah.

---

## Handover di 3G: Soft, Softer, Hard

### 1. Soft Handover (SHO)
- UE connect ke **2 atau 3 Node B berbeda** sekaligus
- **Make-before-break**: koneksi baru dibuat sebelum yang lama diputus
- Terjadi di **overlap area** antar cell
- Node B kirim data ke RNC, RNC pilih frame terbaik (selection combining)
- Uplink: kedua Node B terima, RNC pilih yang terbaik
- Downlink: UE terima dari kedua Node B, combine di RAKE receiver
- **Benefit**: seamless, no call drop, coverage lebih baik
- **Cost**: pakai resource 2 Node B (overhead)

**Active Set**: list Node B yang sedang connect ke UE (max 3)

### 2. Softer Handover
- UE connect ke **2 atau 3 sector dalam Node B yang sama**
- Mirip soft handover tapi dalam 1 Node B
- Combining di Node B (lebih efisien dari soft handover)
- Terjadi di overlap area antar sector

### 3. Hard Handover
- **Break-before-make**: putus dulu koneksi lama, baru connect ke baru
- Terjadi saat:
  - Handover ke **2G** (GSM)
  - Handover ke **frekuensi berbeda** (inter-frequency)
  - Handover ke **operator lain** (roaming)
- Ada kemungkinan **call drop** saat handover
- Delay bisa terasa (silent moment)

**Kapan Soft Handover Trigger?**
- UE continuously measure **Ec/No** atau **RSCP** dari CPICH neighboring cells
- Kalau cell lain kuat (hampir sama atau lebih kuat dari serving cell) → trigger soft handover
- Parameter: **Event 1A** (add cell to active set), **Event 1B** (remove cell), **Event 1C** (replace cell)

---

## Cell Breathing Effect

Karena CDMA, 3G punya fenomena **cell breathing**:

### Apa itu Cell Breathing?
- **Coverage cell berubah-ubah** tergantung load/jumlah user
- Banyak user → interference tinggi → coverage mengecil ("cell bernafas masuk")
- Sedikit user → interference rendah → coverage membesar ("cell bernafas keluar")

### Kenapa Terjadi?
- 3G pakai CDMA: semua user di frekuensi sama
- Tiap user tambah = interference tambah
- Makin banyak interference → **noise floor naik**
- User di edge cell tidak bisa connect (SINR terlalu rendah)
- Akibat: **coverage mengecil saat peak hour**

### Dampak Real:
- Peak hour (siang/malam) → coverage drop 20-40%
- User yang tadinya dapat sinyal bisa tiba-tiba lost signal
- Berbeda dengan 2G/4G yang coverage-nya fixed

### Solusi:
- **Admission control**: tolak user baru kalau cell sudah congested
- **Load balancing**: pindahkan user ke cell lain
- **Carrier addition**: tambah carrier 3G (multi-carrier)

---

## eRIO dan Slot Management

**eRIO (enhanced Radio Input Output)** adalah modul hardware di Node B yang mengandung **3 slot card**:

```
┌─────────────┐
│   Slot 1    │ → Bisa: 2G / 3G / 4G
├─────────────┤
│   Slot 2    │ → Bisa: 2G / 3G / 4G
├─────────────┤
│   Slot 3    │ → Bisa: 2G / 3G / 4G
└─────────────┘
```

### Contoh Konfigurasi:
- **Sebelum decommissioning**: Slot 1 = 2G, Slot 2 = 3G, Slot 3 = 4G
- **Setelah 3G dicabut**: Slot 1 = 2G, Slot 2 = 4G, Slot 3 = 4G

Slot yang dikosongkan bisa diisi teknologi lain sesuai demand area tersebut.

---

## 3G Shutdown (Decommissioning)

### Alasan 3G Ditutup:
1. **Efisiensi Spektrum**: Frekuensi 3G dialihkan ke 4G/5G yang lebih efisien
2. **Biaya Operasional**: Maintenance multi-teknologi mahal
3. **User Migration**: Mayoritas user sudah pakai 4G/5G
4. **Teknologi Transisi**: 3G adalah hybrid (CS + PS), kurang efisien

### Proses Shutdown:
1. **Fase 1**: Matikan 3G di area dengan coverage 4G kuat
2. **Fase 2**: Refarm frekuensi 2100 MHz untuk 4G
3. **Fase 3**: Cabut modul 3G dari Node B (eRIO)
4. **Fase 4**: Realokasi hardware untuk teknologi lain

### Status di Indonesia (2025):
- **Telkomsel**: Full shutdown 1 Juni 2024 (sudah mati total)
- **Indosat (Indosat Ooredoo Hutchison)**: Shutdown 2023-2024 (hampir total)
- **XL Axiata**: Shutdown bertahap 2023-2025 (proses akhir)
- **Tri (3)**: Merger dengan Indosat, shutdown progresif
- **Smartfren**: Tidak pernah deploy W-CDMA (pakai CDMA2000, lalu langsung 4G)

**Realitas di lapangan:**
- Di kota besar: 3G sudah hampir tidak ada
- Di remote area: beberapa site masih 3G untuk backup 2G
- User masih pakai HP 3G-only: harus upgrade atau cuma bisa 2G

**Alternatif Voice:**
- 2G (GSM) masih aktif untuk voice
- VoLTE (Voice over LTE) di 4G
- VoNR (Voice over New Radio) di 5G

---

## Keunggulan 3G

- ✅ **Kecepatan jauh lebih tinggi** dari 2G (2 Mbps standar, HSPA+ 42 Mbps)
- ✅ **Internet mobile jadi mainstream** - browsing, email, social media lancar
- ✅ **Video call** pertama kali di mobile (64-384 kbps)
- ✅ **Simultaneous voice + data** - bisa nelpon sambil browsing
- ✅ **Streaming video** smooth (YouTube 240p-360p, awal era mobile video)
- ✅ **Mobile gaming online** - latency cukup untuk casual gaming
- ✅ **Soft handover** - koneksi seamless, no call drop saat pindah cell
- ✅ **CDMA lebih tahan interferensi** - spreading code mengurangi interference
- ✅ **Kapasitas lebih besar** - bisa handle lebih banyak user per cell
- ✅ **Global roaming** - W-CDMA standar global (kecuali AS yang pakai CDMA2000)
- ✅ **Always-on internet** - tidak perlu dial-up seperti GPRS
- ✅ **Push email & instant messaging** - BlackBerry, WhatsApp era awal

---

## Keterbatasan 3G

- ❌ **Dual switching** (CS + PS) - kompleks dan kurang efisien
- ❌ **Konsumsi baterai sangat tinggi** - terutama HSPA+ (2-3x lebih boros dari 2G)
- ❌ **Upload jauh lebih lambat** dari download (asimetris: 2 Mbps down vs 384 kbps up)
- ❌ **Latency tinggi** - 100-150 ms (kurang bagus untuk gaming/video call)
- ❌ **Cell breathing effect** - coverage mengecil saat peak hour (crowded area)
- ❌ **Near-far problem** - butuh power control kompleks
- ❌ **Interference antar user** - semua di frekuensi sama (self-interference)
- ❌ **Handover delay** - terutama hard handover ke 2G (call drop bisa terjadi)
- ❌ **Coverage indoor lemah** - frekuensi 2100 MHz kurang bagus penetrasi gedung
- ❌ **Network congestion** saat peak hour - kecepatan drop drastis (384 kbps → 50 kbps)
- ❌ **Teknologi transisi** - tidak optimal untuk voice maupun data (jack of all trades)
- ❌ **HP cepat panas** - processing CDMA lebih berat dari TDMA

---

## Perbandingan Cepat: 2G vs 3G

| Aspek | 2G (GSM/EDGE) | 3G (W-CDMA/HSPA) |
|-------|---------------|------------------|
| **Kecepatan Max** | 236 kbps (EDGE) | 42 Mbps (HSPA+) |
| **Kecepatan Real** | 50-150 kbps | 5-15 Mbps |
| **Modulasi** | GMSK, 8-PSK | QPSK, 16-QAM, 64-QAM |
| **Multiplexing** | TDMA | W-CDMA (CDMA) |
| **Frekuensi (ID)** | 900/1800 MHz | 2100 MHz |
| **Switching** | CS (voice), PS (data) | CS + PS (dual) |
| **Bandwidth** | 200 kHz | 5 MHz (25x lebih lebar) |
| **Chip Rate** | - | 3.84 Mcps |
| **Handover** | Hard | Soft/Softer/Hard |
| **Voice + Data** | Tidak (suspend GPRS) | Bisa simultan |
| **Power Control** | Slow (2 Hz) | Fast (1500 Hz) |
| **Latency** | 500-1000 ms | 100-150 ms |
| **Use Case** | Voice, SMS, Email | Video call, streaming, browsing |
| **Tower** | BTS | Node B |
| **Controller** | BSC | RNC |

---

## Poin Penting yang Harus Diingat

- 🎯 **3G = W-CDMA** di Indonesia (standar UMTS Eropa, bukan CDMA2000)
- 🎯 **Perubahan nama semua komponen** (MS→UE, BTS→Node B, BSC→RNC)
- 🎯 **Chip rate 3.84 Mcps** dengan spreading factor 4-256
- 🎯 **Dual switching**: CS untuk voice, PS untuk data (bisa simultan!)
- 🎯 **3 tier kecepatan**: 144 kbps (rural), 384 kbps (urban outdoor), 2 Mbps (urban indoor)
- 🎯 **Frekuensi 2100 MHz** di Indonesia (FDD: UL 1920-1980, DL 2110-2170)
- 🎯 **Bandwidth 5 MHz** per carrier (25x lebih lebar dari GSM)
- 🎯 **Soft handover** = connect ke 2-3 Node B sekaligus (make-before-break)
- 🎯 **Fast power control 1500x/detik** untuk atasi near-far problem
- 🎯 **Cell breathing effect** - coverage berubah-ubah tergantung load
- 🎯 **4 QoS classes**: Conversational, Streaming, Interactive, Background
- 🎯 **RNC bisa connect ke MSC 2G** untuk interoperability & handover
- 🎯 **HSPA/HSPA+** adalah evolution (3.5G/3.75G/3.9G) hingga 42 Mbps
- 🎯 **Shutdown total di Indonesia** - Telkomsel Juni 2024, operator lain menyusul
- 🎯 **eRIO slot management** memungkinkan refarm ke 4G/5G

---

## Real World Experience: Nostalgia Era 3G

### Pengalaman User di Era 3G (2007-2015)
- **BlackBerry Messenger (BBM)** jadi fenomena - chat always-on
- **Facebook & Twitter mobile** mulai booming
- **YouTube mobile** pertama kali bisa ditonton smooth (240p-360p)
- **WhatsApp** lahir dan bertumbuh di era 3G
- **Instagram** upload foto langsung dari HP (bukan dari PC)
- **Mobile gaming online** - Clash of Clans, hay Day era
- **Video call** jadi fitur premium (mahal, jarang dipakai)
- **Paket data mulai umum** - dari "tarif per KB" ke "paket unlimited"

### Masalah yang Sering Dialami
- **Baterai boros** - HP harus di-charge sehari 2x kalau banyak browsing
- **HP panas** saat streaming/gaming lama
- **Sinyal H/H+ loncat ke 3G/E** - kecepatan tidak konsisten
- **Peak hour lemot** - jam makan siang & pulang kantor kecepatan drop
- **Indoor susah sinyal** - harus ke dekat jendela untuk dapat 3G
- **Video call sering putus** atau gambar freeze (latency tinggi)
- **Upload foto lama** - 1 foto 2 MB bisa 1-2 menit

### Kenapa 3G Penting?
3G adalah **game changer** yang:
- Membuat internet bukan lagi "luxury" tapi "daily need"
- Mengubah HP dari "alat telepon" jadi "komputer saku"
- Melahirkan era **mobile-first** apps dan services
- Membuka ekonomi digital mobile (e-commerce, ride-hailing, dll)
- Fondasi untuk 4G & 5G yang kita nikmati sekarang

---

## Penutup

3G adalah **generasi transisi** yang membawa internet mobile mainstream. Meski sekarang sudah shutdown, 3G adalah fondasi penting untuk evolusi ke 4G dan 5G. 

**Warisan 3G:**
- Teknologi **CDMA** dan **spreading code** masih dipakai di 4G/5G (dalam bentuk OFDMA)
- Konsep **packet switching** jadi dominan (4G full PS)
- **QoS classes** masih dipakai hingga 5G
- **Soft handover concept** jadi basis untuk seamless mobility
- **Power control** masih krusial di semua generasi selanjutnya

**Next**: Jaringan 4G (LTE) - All IP Network, No More Circuit Switching! 🚀
