# Jaringan 3.5G (HSDPA)

## Apa itu 3.5G?

**3.5G** adalah evolusi dari 3G yang menggunakan teknologi **HSDPA (High-Speed Downlink Packet Access)**.

- **High-Speed**: Kecepatan tinggi
- **Downlink**: Arah download (dari Node B ke UE)
- **Packet Access**: Akses berbasis paket data

Fokus utama: **Mempercepat download**, bukan upload (upload masih pakai 3G standar).

**Nama lain:**
- HSDPA
- 3.5G
- "H" atau "3G+" di indikator HP

---

## Perbedaan 3G vs 3.5G: Software, Bukan Hardware!

### Arsitektur: SAMA!

```
UE → Node B → RNC → Core Network (MSC/SGSN/GGSN)
```

Tidak ada perubahan hardware atau komponen baru. Yang berubah adalah **software**.

### Yang Berubah: Software di Radio Access Network (RAN)

Di 3.5G, ditambahkan **software MAC-hs (Medium Access Control - High Speed)** di:
- **Node B** (bukan di RNC!)
- Ini adalah **game changer** yang bikin HSDPA jauh lebih cepat

**Konsekuensi:**
- Operator tidak perlu ganti tower atau hardware baru
- Cukup **upgrade software** di Node B
- Rollout cepat dan murah (software upgrade via remote)
- HP lama 3G tidak bisa pakai HSDPA (butuh modem HSDPA di chipset)

---

## 3 Pengaruh Besar dari Software MAC-hs

### 1. AMC (Adaptive Modulation and Coding)

**Adaptive** = Bisa menyesuaikan secara otomatis dan cepat.

#### Modulasi di HSDPA:
HSDPA menggunakan **2 modulasi** yang bisa dipilih secara dinamis:

**A. QPSK (Quadrature Phase Shift Keying)**
- 2 bit per simbol
- Dipakai saat **user jauh** dari Node B
- Sinyal lemah, butuh modulasi robust
- Kecepatan lebih rendah, tapi stabil
- Coverage lebih luas

**B. 16-QAM (16 Quadrature Amplitude Modulation)**
- **4 bit per simbol** (2x lebih efisien dari QPSK!)
- Dipakai saat **user dekat** Node B
- Sinyal kuat, bisa pakai modulasi kompleks
- Kecepatan tinggi, throughput bagus
- Tidak mudah interferensi (kalau SINR tinggi)

#### Bagaimana AMC Bekerja?

```
User dekat tower (SINR tinggi) → 16-QAM → 4 bit/simbol → Kecepatan tinggi
User jauh dari tower (SINR rendah) → QPSK → 2 bit/simbol → Kecepatan stabil
```

**Node B continuously monitor:**
- **CQI (Channel Quality Indicator)**: UE lapor kualitas channel setiap 2 ms
- Kalau CQI bagus → switch ke 16-QAM
- Kalau CQI jelek → fallback ke QPSK

**Benefit:**
- Maksimalkan throughput saat kondisi bagus
- Maintain koneksi saat kondisi jelek
- Tidak putus koneksi tiba-tiba (graceful degradation)

#### Coding Rate (Turbo Coding)

Selain modulasi, coding rate juga adaptif:
- **Coding rate tinggi (3/4)**: Sedikit redundancy, kecepatan tinggi (kondisi bagus)
- **Coding rate rendah (1/4)**: Banyak redundancy, error correction kuat (kondisi jelek)

**AMC = Modulation + Coding** adaptif bersamaan untuk optimal performance.

---

### 2. Fast Scheduling (Penjadwalan Cepat)

**Scheduling** = Menentukan **user mana** yang dapat **kirim/terima data** di **slot waktu tertentu**.

#### Kenapa Perlu Scheduling?
- Bandwidth terbatas (5 MHz per carrier)
- Banyak user share resource yang sama
- Perlu adil (fair) tapi juga efisien

#### Proportional Fair (PF) Scheduler

HSDPA menggunakan **PF (Proportional Fair) scheduling**:

**Prinsip:**
- Prioritaskan user dengan **kondisi channel terbaik saat ini**
- Tapi juga kasih kesempatan ke user dengan channel jelek (fairness)
- Balance antara throughput maksimal vs keadilan

**PF mempertimbangkan 3 hal:**

**1. Kualitas Channel (CQI)**
- User dengan CQI tinggi → prioritas lebih tinggi
- Efisien karena bit per Hz maksimal

**2. Kemampuan UE (Device Category)**
- HP flagship (Cat 6-10) → bisa terima kecepatan tinggi
- HP murah (Cat 1-3) → kecepatan terbatas hardware
- Tidak ada gunanya alokasi bandwidth besar ke HP yang tidak mampu

**3. Kelas Pelanggan (QoS Class)**
- Pelanggan VIP/premium → prioritas lebih tinggi
- Pelanggan biasa → prioritas standar
- Background service → prioritas rendah

**Formula PF Scheduler:**
```
Priority = (Instantaneous data rate) / (Average data rate)
```

User dengan instantaneous rate tinggi relatif terhadap average-nya → dapat slot.

#### Teknik Pendukung Scheduling:

**A. Round-Robin**
- Giliran berputar untuk semua user
- Fair tapi tidak efisien (user dengan channel jelek dapat slot sia-sia)
- HSDPA tidak pakai pure round-robin

**B. Max C/I (Carrier to Interference)**
- Selalu prioritaskan user dengan C/I tertinggi
- Throughput maksimal tapi tidak fair (user cell edge kelaparan)
- HSDPA tidak pakai pure max C/I

**C. Proportional Fair (Hybrid)**
- Kombinasi round-robin (fairness) + max C/I (efficiency)
- **Inilah yang dipakai HSDPA!**

---

### 3. Fast Cell Selection (HS-DSCH Serving Cell Change)

**Cell Selection** = Menentukan **Node B mana** yang melayani user saat ini.

#### Perbedaan dengan 3G (Soft Handover)

**3G (Soft Handover):**
- User connect ke **2-3 Node B sekaligus** (active set)
- Data dikirim dari semua Node B → RNC combine → pilih yang terbaik
- **Lambat** (RNC harus koordinasi multi Node B)
- Resource boros (multiple Node B kirim data sama)

**HSDPA (Fast Cell Selection / Hard Handover):**
- User hanya connect ke **1 Node B** (serving cell)
- Kalau pindah cell → **putus langsung connect ke yang baru** (hard handover)
- **Cepat** (tidak perlu koordinasi kompleks)
- Resource efisien (hanya 1 Node B aktif)

#### Bagaimana Memilih Cell?

User (UE) terus measure **CPICH Ec/No** atau **RSCP** dari neighboring cells:
- Node B dengan **daya terbesar** (strongest signal) → dipilih jadi serving cell
- Kalau ada Node B lain lebih kuat → **hard handover** ke sana

**Kriteria:**
- Signal strength (RSCP)
- Signal quality (Ec/No)
- Load balancing (kalau serving cell overloaded)

**Kecepatan:**
- Detection: ~200 ms (measurement report setiap 200-480 ms)
- Switching: ~50-100 ms (execution)
- Total: ~250-350 ms (jauh lebih cepat dari soft handover 3G)

#### Konsekuensi Hard Handover:

✅ **Keuntungan:**
- Proses cepat
- Resource efisien (tidak pakai 2-3 Node B sekaligus)
- Scheduling di Node B (bukan RNC) → low latency

❌ **Kerugian:**
- Bisa ada **brief interruption** saat handover (~50-100 ms)
- Risiko **call drop** kalau handover gagal (tapi jarang)
- Tidak ada **macro diversity** seperti soft handover

**Praktis:**
- User hampir tidak merasakan interupsi (50-100 ms tidak terasa)
- Streaming video/audio: buffer cukup untuk cover interruption
- Browsing/download: tidak terasa sama sekali

---

## Lokasi Scheduling & Cell Selection: Node B!

Ini adalah **revolusi besar** dari 3G ke HSDPA:

### 3G Standard:
- **Scheduling** → di **RNC**
- **Cell selection (soft HO)** → di **RNC**
- Node B hanya "dumb" radio transceiver
- Latency tinggi karena semua keputusan di RNC

### 3.5G HSDPA:
- **Scheduling** → di **Node B** (MAC-hs)
- **Cell selection** → di **Node B** (fast decision)
- Node B punya "intelligence" untuk ambil keputusan cepat
- Latency rendah karena keputusan lokal

**Benefit:**
- **Faster reaction** terhadap kondisi channel (real-time)
- **Lower latency** (tidak perlu round-trip ke RNC)
- **Higher throughput** (scheduling optimal setiap 2 ms)
- **Better user experience** (responsif, tidak lag)

**Jalur Data:**
```
3G:     UE ← Node B ← RNC (scheduling) ← SGSN ← GGSN
HSDPA:  UE ← Node B (scheduling langsung!) ← RNC ← SGSN ← GGSN
```

Node B langsung ambil keputusan tanpa tanya RNC → **super cepat**!

---

## Peningkatan Performa: 3G vs 3.5G

### Data Rate (Kecepatan)

**3G (Release 99):**
- Downlink: **2 Mbps** (theoretical max)
- Uplink: **384 kbps**
- Real speed: 200-500 kbps

**3.5G (HSDPA Release 5):**
- Downlink: **14.4 Mbps** (theoretical max)
- Uplink: **384 kbps** (masih sama, HSDPA hanya downlink)
- Real speed: 2-7 Mbps

**Peningkatan:** 7x lebih cepat! (2 Mbps → 14.4 Mbps)

### TTI (Transmission Time Interval)

**TTI** = Waktu minimum untuk transmit 1 blok data.

**3G:**
- TTI: **10 ms** (10 milisecond)
- Lambat untuk real-time data

**3.5G (HSDPA):**
- TTI: **2 ms** (2 milisecond)
- **5x lebih cepat** dari 3G!

**Benefit TTI pendek:**
- Latency lebih rendah
- Scheduling lebih sering (setiap 2 ms) → adaptasi cepat
- Better untuk real-time apps (streaming, gaming)

**Contoh:**
- Download file 10 MB:
  - 3G: ~40-50 detik
  - HSDPA: ~6-10 detik
- Latency (ping):
  - 3G: 150-200 ms
  - HSDPA: 80-120 ms

---

## HS-DSCH: Channel Khusus HSDPA

**HS-DSCH (High-Speed Downlink Shared Channel)** adalah **transport channel baru** di HSDPA:

### Karakteristik HS-DSCH:
- **Shared channel**: Semua user share, bukan dedicated
- **Time multiplexing**: User dapat slot waktu berbeda (setiap 2 ms)
- **Fast scheduling**: Node B tentukan user mana dapat slot
- **Fast AMC**: Modulasi & coding berubah setiap 2 ms
- **HARQ**: Retransmission cepat kalau ada error

### Physical Channel Terkait:

**1. HS-PDSCH (High-Speed Physical Downlink Shared Channel)**
- Channel fisik untuk kirim data HS-DSCH
- Multi-code: bisa pakai 1-15 code simultan
- Spreading factor SF=16 (fixed)

**2. HS-SCCH (High-Speed Shared Control Channel)**
- Downlink control channel
- Kasih tahu UE: "data untuk kamu ada di code X, pakai modulasi Y"
- Dikirim 2 slot (2 TTI) sebelum data

**3. HS-DPCCH (High-Speed Dedicated Physical Control Channel)**
- Uplink control channel
- UE kirim **CQI** (Channel Quality Indicator) ke Node B
- UE kirim **ACK/NACK** untuk HARQ
- Setiap 2 ms

**Alur:**
```
1. UE kirim CQI ke Node B (via HS-DPCCH)
2. Node B scheduling: pilih user, modulasi, code
3. Node B kirim info control (via HS-SCCH): "User A, code 5-10, 16-QAM"
4. Node B kirim data (via HS-PDSCH)
5. UE decode data, kirim ACK/NACK (via HS-DPCCH)
6. Kalau NACK → Node B retransmit cepat (HARQ)
```

---

## HARQ (Hybrid Automatic Repeat Request)

**HARQ** adalah teknik **retransmission cepat** di HSDPA:

### ARQ vs HARQ

**ARQ (Traditional):**
- Kalau data error → minta retransmit dari awal
- Buang data yang error (tidak disimpan)
- Lambat (round-trip time tinggi)

**HARQ (Hybrid):**
- Kalau data error → **simpan** data yang error (soft combining)
- Minta retransmit
- **Combine** data lama + data baru → decode
- **Lebih cepat** (kemungkinan sukses decode lebih tinggi)

### HARQ di HSDPA:

**1. Stop-and-Wait (N-channel SAW)**
- Ada 6-8 proses HARQ paralel
- Proses 1 transmit → tunggu ACK/NACK
- Sementara tunggu, proses 2 transmit → dan seterusnya
- Efficient (tidak idle waiting)

**2. Soft Combining**
- **Chase Combining**: Retransmit data sama persis, combine dengan yang lama
- **Incremental Redundancy (IR)**: Retransmit dengan redundancy bits berbeda, decode combine

**3. Fast ACK/NACK**
- UE decode data dalam ~2 ms
- Langsung kirim ACK (sukses) atau NACK (error)
- Node B langsung respond (retransmit atau kirim data baru)
- Latency rendah (tidak perlu sampai RNC)

**Benefit:**
- **Throughput tinggi** walaupun kondisi channel jelek
- **Latency rendah** (retransmission cepat)
- **Spektrum efisien** (soft combining tingkatkan decode rate)

---

## UE Category (Device Classes)

HSDPA mendefinisikan **UE Category** untuk bedakan kemampuan device:

| Category | Max Speed (Mbps) | Modulation | Codes | TTI (ms) |
|----------|------------------|------------|-------|----------|
| **Cat 1-2** | 1.2 | QPSK | 5 | 2 |
| **Cat 3-4** | 1.8 | QPSK | 5 | 2 |
| **Cat 5-6** | 3.6 | QPSK, 16-QAM | 5 | 2 |
| **Cat 7-8** | 7.2 | QPSK, 16-QAM | 10 | 2 |
| **Cat 9-10** | 10.2 | QPSK, 16-QAM | 15 | 2 |
| **Cat 11-12** | 14.4 | QPSK, 16-QAM | 15 | 2 |

**Pengaruh:**
- HP murah (Cat 1-6): Max speed 1.2-3.6 Mbps
- HP mid-range (Cat 7-10): Max speed 7.2-10.2 Mbps
- HP flagship (Cat 11-12): Max speed 14.4 Mbps

**Praktis:**
- Operator allocate resource berdasarkan UE category
- Tidak fair alokasi 15 codes ke HP Cat 1 (cuma bisa 1.2 Mbps)
- Better alokasi ke HP Cat 12 (bisa sampai 14.4 Mbps)

---

## Perbandingan: 3G vs 3.5G (HSDPA)

| Aspek | 3G (R99) | 3.5G (HSDPA) |
|-------|----------|--------------|
| **Release** | Release 99 (2000) | Release 5 (2002) |
| **Max Speed DL** | 2 Mbps | 14.4 Mbps |
| **Max Speed UL** | 384 kbps | 384 kbps (sama) |
| **Real Speed DL** | 200-500 kbps | 2-7 Mbps |
| **Modulasi** | QPSK | QPSK, 16-QAM (adaptive) |
| **TTI** | 10 ms | 2 ms (5x lebih cepat) |
| **Scheduling** | RNC (slow) | Node B (fast) |
| **Handover** | Soft (multi Node B) | Hard (single Node B) |
| **Latency** | 150-200 ms | 80-120 ms |
| **Retransmission** | ARQ (RNC) | HARQ (Node B) |
| **Channel** | DCH (dedicated) | HS-DSCH (shared) |
| **AMC** | Tidak ada | Ada (adaptive) |
| **CQI Feedback** | Lambat | Fast (setiap 2 ms) |
| **Indikator HP** | 3G, U, UMTS | H, 3G+, HSDPA |

---

## HSUPA (3.75G): Upload Juga Dipercepat!

Setelah HSDPA (downlink cepat), muncul **HSUPA (High-Speed Uplink Packet Access)**:

### HSUPA (Release 6, 2004):
- **Uplink speed**: Hingga **5.76 Mbps** (dari 384 kbps)
- Teknologi serupa HSDPA tapi untuk uplink
- **E-DCH** (Enhanced Dedicated Channel)
- **Fast scheduling** di Node B
- **HARQ** untuk uplink
- **TTI**: 2 ms atau 10 ms

### HSPA = HSDPA + HSUPA
- **Downlink**: 14.4 Mbps (HSDPA)
- **Uplink**: 5.76 Mbps (HSUPA)
- Indikator: **H** atau **H+** di HP

**Use case HSUPA:**
- Upload foto/video ke social media
- Video call (butuh uplink bagus)
- Live streaming (broadcaster)
- Cloud backup

---

## HSPA+ (3.9G): Evolusi Terakhir 3G

**HSPA+ (Evolved HSPA, Release 7-10, 2008-2010)** adalah evolusi final HSPA:

### Fitur HSPA+:

**1. Kecepatan Lebih Tinggi**
- Downlink: **42 Mbps** (dengan 2x2 MIMO + 64-QAM)
- Uplink: **11.5 Mbps** (dengan 16-QAM)
- Dual-Carrier HSPA+ (DC-HSPA+): **84 Mbps** (2x 42 Mbps)

**2. 64-QAM Modulation**
- **6 bit per simbol** (vs 4 bit di 16-QAM)
- Butuh SINR sangat tinggi (hanya dekat tower)
- Real deployment: jarang dipakai (kondisi jarang ideal)

**3. MIMO (Multiple Input Multiple Output)**
- **2x2 MIMO**: 2 antena transmit, 2 antena receive
- Spatial multiplexing: 2 stream data paralel
- Throughput ~2x lipat

**4. Continuous Packet Connectivity (CPC)**
- Kurangi overhead signaling
- Hemat baterai
- Faster dormancy

**5. Reduced Latency**
- Latency: **50-80 ms** (hampir setara 4G awal)
- Better untuk VoIP, gaming

### DC-HSPA+ (Dual-Carrier)
- Aggregate 2 carrier 5 MHz → 10 MHz total
- Downlink: 2x 21 Mbps = **42 Mbps** per carrier
- Dengan MIMO: 2x 42 Mbps = **84 Mbps** total
- Jarang deploy di Indonesia (lebih fokus 4G)

**Indikator HP:**
- **H+** atau **4G** (beberapa operator label HSPA+ sebagai "4G" untuk marketing)

---

## Real World Performance & User Experience

### Kecepatan Real (Indonesia, 2010-2015):

**HSDPA (3.5G):**
- Urban area: 2-5 Mbps download, 300-500 kbps upload
- Suburban: 1-3 Mbps download, 200-400 kbps upload
- Rural: 500 kbps - 1.5 Mbps download

**HSPA+ (3.75G):**
- Urban area: 5-15 Mbps download, 1-3 Mbps upload
- Suburban: 3-8 Mbps download, 500 kbps - 2 Mbps upload
- Peak: Bisa tembus 20-25 Mbps (dekat tower, kondisi ideal)

### Use Case yang Jadi Smooth:

✅ **Streaming YouTube** - 360p-480p lancar (3.5G), 720p smooth (HSPA+)
✅ **Facebook/Instagram** - load feed cepat, upload foto 1-2 detik
✅ **WhatsApp** - kirim voice note, foto, video instant
✅ **Google Maps** - load map cepat, navigasi real-time OK
✅ **Mobile gaming** - Clash of Clans, Mobile Legends playable (latency ~100ms)
✅ **Video call** - Skype, WhatsApp call mulai bagus (dengan HSUPA)
✅ **Music streaming** - Spotify, Joox lancar
✅ **Download apps** - 10-50 MB download jadi wajar (bukan lama banget)

### Masalah yang Masih Ada:

❌ **Upload masih lambat** (HSDPA pure, sebelum HSUPA)
❌ **Latency masih tinggi** untuk FPS gaming (~100-120 ms)
❌ **Peak hour congestion** - kecepatan drop signifikan
❌ **Indoor coverage** - frekuensi 2100 MHz kurang tembus gedung
❌ **Battery drain** - masih boros, HP panas kalau streaming lama
❌ **Asimetris** - download cepat, upload lambat (annoying untuk upload video)

---

## Timeline Deployment 3.5G di Indonesia

**2006-2007**: HSDPA Launch
- Telkomsel: First HSDPA commercial (2006)
- Indosat, XL: Follow up 2007
- Kecepatan: 3.6-7.2 Mbps
- Paket data masih mahal (Rp 50.000 untuk 1 GB)

**2009-2010**: HSPA (HSDPA + HSUPA)
- Semua operator upgrade ke HSPA
- Upload jadi cepat (5.76 Mbps)
- Paket data mulai terjangkau

**2011-2013**: HSPA+ (21-42 Mbps)
- Deploy HSPA+ di urban area
- Speed real 10-20 Mbps (dekat tower)
- Era kejayaan 3.5G (sebelum 4G mature)

**2014-2016**: Transisi ke 4G
- 4G LTE mulai deploy mass
- 3.5G masih dominant di suburban/rural
- Paket data unlimited mulai muncak

**2017-2025**: 3.5G Decline
- 4G jadi mainstream
- 3.5G untuk fallback/backup
- 3G/3.5G shutdown bertahap (2024-2025)

---

## Kenapa HSDPA Penting?

🎯 **Generasi yang bikin mobile internet USABLE**
- 3G terlalu lambat (2 Mbps), 4G belum ada
- HSDPA adalah "sweet spot" untuk mobile internet 2008-2014

🎯 **Teknologi transisi crucial**
- Bridge antara 3G (circuit+packet) ke 4G (all-IP)
- Konsep fast scheduling, HARQ, AMC diadopsi 4G/5G

🎯 **Software upgrade, bukan hardware**
- Operator hemat cost (tidak ganti tower)
- Rollout cepat (software OTA update)

🎯 **Melahirkan era smartphone**
- iPhone 3G/3GS (2008-2009): Era HSDPA
- Android early days: HSDPA critical
- Mobile apps booming karena internet cukup cepat

🎯 **Fondasi untuk 4G**
- MAC-hs di Node B → eNodeB di 4G (distributed intelligence)
- HARQ → tetap dipakai di 4G/5G
- AMC → jadi lebih advanced di 4G (64-QAM, 256-QAM)
- Shared channel concept → PDSCH di LTE

---

## Poin Penting yang Harus Diingat

- 🎯 **HSDPA = 3.5G** - fokus **downlink** cepat (14.4 Mbps)
- 🎯 **HSUPA = 3.75G** - tambah **uplink** cepat (5.76 Mbps)
- 🎯 **HSPA+ = 3.9G** - evolusi final (42-84 Mbps dengan MIMO)
- 🎯 **Software upgrade**, bukan hardware baru (MAC-hs di Node B)
- 🎯 **3 fitur utama**: AMC, Fast Scheduling, Fast Cell Selection
- 🎯 **AMC**: QPSK (jauh) vs 16-QAM (dekat) - adaptive real-time
- 🎯 **Scheduling di Node B** (bukan RNC) → low latency
- 🎯 **Proportional Fair scheduler** - balance throughput vs fairness
- 🎯 **Hard handover** (bukan soft) → cepat, efisien
- 🎯 **TTI 2 ms** (vs 10 ms di 3G) → 5x lebih responsif
- 🎯 **HARQ**: Retransmission cepat dengan soft combining
- 🎯 **HS-DSCH**: Shared channel, time-multiplexed
- 🎯 **CQI feedback setiap 2 ms** → adaptasi real-time
- 🎯 **UE Category 1-12** - bedakan kemampuan device
- 🎯 **Indikator "H" atau "H+"** di HP

---

## Penutup

HSDPA (3.5G) adalah **revolusi mobile internet** yang bikin browsing, streaming, dan social media jadi smooth di smartphone. Meski sekarang sudah digantikan 4G/5G, konsep-konsep HSDPA (fast scheduling, HARQ, AMC) masih hidup dan berkembang di teknologi terkini.

**Warisan HSDPA:**
- **Distributed intelligence** - processing di Node B/eNodeB (bukan central)
- **Fast adaptation** - AMC jadi adaptive MCS di 4G/5G
- **Shared channel concept** - efisiensi spektrum maksimal
- **HARQ** - tetap dipakai hingga 5G
- **Low TTI** - trend terus berlanjut (1 ms di 4G, 0.125 ms di 5G)

Era HSDPA (2008-2014) adalah **golden age** saat mobile internet mulai jadi kebutuhan, bukan luxury!

**Next**: Jaringan 4G (LTE) - All-IP Network, No Circuit Switching! 🚀
