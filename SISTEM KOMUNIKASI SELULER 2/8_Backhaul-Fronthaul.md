# 8. BACKHAUL NETWORK

## PENGANTAR

Materi ini membahas **Backhaul Network** di jaringan LTE, yaitu bagian jaringan yang menghubungkan antara **RAN (Radio Access Network)** dengan **Core Network**.

**Topik yang dibahas**:

1. Apa itu Backhaul Network
2. Fronthaul vs Backhaul
3. Jenis-jenis topologi site (Distributed RAN, Centralized RAN, Picocell)
4. Teknologi transport (CPRI, Fiber Optic, Microwave)
5. BTS Macro, BTS Hotel, dan Picocell
6. CAPEX & OPEX

---

## 1. ARSITEKTUR JARINGAN LTE

### Komponen Utama

Jaringan LTE terdiri dari **3 bagian utama**:

```
[RAN]  ←→  [Backhaul Network]  ←→  [Core Network]
```

**Penjelasan**:

1. **RAN (Radio Access Network)**: Bagian radio/akses (eNodeB, antena)
2. **Backhaul Network**: Jaringan transport penghubung
3. **Core Network (EPC)**: Inti jaringan (MME, SGW, PGW)

**Diagram lengkap**:

```
[UE] → [Antena] → [RRH] → [BBU] → [Aggregation] → [EPC]
                    ↑                  ↑            ↑
                   RAN              Backhaul       Core
```

---

## 2. APA ITU BACKHAUL NETWORK?

### Definisi

**Backhaul Network** = Jaringan transport yang menghubungkan **RAN** dengan **Core Network**.

**Fungsi**:

- Membawa data dari banyak site/BTS
- Mengumpulkan traffic sebelum masuk ke core
- Agregasi data

**Kenapa perlu Backhaul?**

> Tidak mungkin setiap user (ribuan/puluhan ribu) langsung terhubung ke core. Perlu **agregasi** dulu!

**Analogi**:

- **User** = banyak kendaraan
- **Backhaul** = jalan tol (agregasi)
- **Core** = pusat kota (tujuan akhir)

---

## 3. FRONTHAUL VS BACKHAUL

### Perbedaan

**Backhaul Network** dibagi menjadi **2 bagian**:

#### 1. Fronthaul

**Fronthaul** = Bagian **depan**, yang terdekat dengan user.

- Dari **RRH (Remote Radio Head)** ke **BBU (Baseband Unit)**
- Jarak: dekat (dalam satu site atau antar site)

#### 2. Backhaul

**Backhaul** = Bagian **belakang**, yang terdekat dengan core.

- Dari **BBU** ke **Aggregation Gateway** menuju **EPC (Core)**
- Jarak: jauh (menuju central office)

### Diagram

```
[RRH] ←─ Fronthaul ─→ [BBU] ←─ Backhaul ─→ [Aggregation] → [EPC]
 (Depan/dekat user)              (Belakang/dekat core)
```

**Ringkasan**:

- **Fronthaul**: RRH ↔ BBU
- **Backhaul**: BBU ↔ Core

---

## 4. KOMPONEN RAN

### RRH (Remote Radio Head)

**RRH** = Perangkat radio yang ditempatkan di dekat antena.

**Fungsi**:

- Modulasi sinyal RF
- Mengirim/terima sinyal ke/dari user

**Posisi**:

- **RRH remote**: Ditempatkan di **atas tower** (dekat antena)
- **RRH local**: Ditempatkan di **bawah tower** (dekat BBU)

**Berat**: Sekitar 12-25 kg per unit.

### BBU (Baseband Unit)

**BBU** = Perangkat yang memproses sinyal baseband.

**Fungsi**:

- Proses baseband (digital processing)
- Kontrol radio
- Konfigurasi teknologi (2G, 3G, 4G)

**Posisi**:

- Indoor (di shelter/ruangan khusus)
- Perlu AC (karena perangkat indoor)

**Catatan**: Di BBU ada modul-modul yang bisa diaktifkan untuk teknologi berbeda (2G, 3G, 4G).

---

## 5. TEKNOLOGI TRANSPORT FRONTHAUL

### CPRI (Common Public Radio Interface)

**CPRI** = Protokol untuk menghubungkan **RRH** dengan **BBU**.

**Fungsi**:

- Interface point-to-point
- Mapping sinyal antena
- Support semua teknologi seluler (2G, 3G, 4G)

**Media**: **Fiber Optic** (Fiber Optik)

**Karakteristik**:

- Point-to-point atau multihop
- Multiplexing: TDM (Time Division Multiplexing)
- Kecepatan: bervariasi (tergantung bandwidth)

**Diagram**:

```
[RRH] ←─ CPRI via Fiber Optic ─→ [BBU]
```

### Alternatif Fronthaul

Selain Fiber Optic, fronthaul bisa menggunakan:

1. **Microwave**: Jika tidak ada fiber
2. **Copper Cable**: Jarak sangat dekat (jarang)
3. **Satellite**: Untuk daerah terpencil

---

## 6. JENIS-JENIS TOPOLOGI SITE

### 6.1 DISTRIBUTED RAN (Macro Site / BTS Macro)

**Distributed RAN** = RRH dan BBU berada dalam **satu lokasi yang sama**.

**Ciri-ciri**:

- Tower tinggi: 20-30 meter
- BBU dan RRH dalam satu tempat (biasanya di shelter)
- Cakupan luas

**Diagram**:

```
       [Antena]
         ↓
       [RRH] ← di atas atau di bawah
         ↓ CPRI
       [BBU] ← di shelter (indoor)
         ↓
       [Core]
```

**Kelebihan**:

- ✅ Mudah troubleshooting (karena dekat)
- ✅ Cakupan luas (tower tinggi)

**Kekurangan**:

- ❌ Perlu ruangan khusus (shelter)
- ❌ Perlu AC (karena BBU indoor)
- ❌ CAPEX tinggi (biaya investasi besar)
- ❌ OPEX tinggi (biaya operasional: listrik, AC, genset)

**Kapan dipakai?**

- Daerah pedesaan
- Daerah dengan user tersebar luas
- Daerah jauh dari central office

---

### 6.2 CENTRALIZED RAN (BTS Hotel)

**Centralized RAN** = RRH dan BBU berada di **tempat berbeda** (terpisah jauh).

**Ciri-ciri**:

- RRH: Di pinggir jalan (tower kecil, 12-15 meter)
- BBU: Di central office / TTC (Telkomsel Telecommunication Center)
- Dihubungkan dengan **Fiber Optic** (CPRI)

**Diagram**:

```
[RRH 1] ─┐
[RRH 2] ─┤
[RRH 3] ─┤─ Fiber Optic ─→ [BBU Pool] → [Core]
[RRH 4] ─┤                  (Central Office)
[RRH 5] ─┘
```

**Kelebihan**:

- ✅ Hemat space (tidak perlu shelter besar)
- ✅ RRH outdoor (tidak perlu AC)
- ✅ Mudah maintenance (BBU terpusat)
- ✅ CAPEX lebih rendah
- ✅ OPEX lebih rendah

**Kekurangan**:

- ❌ Jika fiber putus (**cut-off**), semua site mati
- ❌ Risiko tinggi (troubleshooting lebih sulit)

**Kapan dipakai?**

- Daerah perkotaan padat penduduk
- Banyak site berdekatan
- Akses fiber mudah

**Istilah lain**: **BTS Hotel**

**BTS Hotel** = Kumpulan RRH di pinggir jalan, BBU terpusat di central office.

**Contoh di Indonesia**:

- TTC Pasar Minggu (Jakarta)
- TTC Buaran (Jakarta)
- TTC BSD (Tangerang)
- TTC Simatupang (Jakarta)

---

### 6.3 PICOCELL (Small Cell / In-Building)

**Picocell** = Small cell untuk **indoor** (mall, gedung, underground).

**Ciri-ciri**:

- Kapasitas: sama dengan 1 BTS
- Ukuran: kecil (seperti CCTV atau smoke detector)
- Ditempatkan di: mall, gedung, basement
- Banyak unit di satu gedung

**Diagram**:

```
Lantai 3: [Picocell 1] ┐
Lantai 2: [Picocell 2] ├─ Fiber Optic ─→ [BBU] → [Core]
Lantai 1: [Picocell 3] ┘    (Ruangan khusus di gedung)
```

**Kelebihan**:

- ✅ Praktis untuk indoor
- ✅ Coverage spesifik (per lantai/ruangan)

**Kekurangan**:

- ❌ Perlu ruangan khusus untuk BBU
- ❌ Perlu AC
- ❌ Jumlah unit banyak (untuk gedung besar)

**Kapan dipakai?**

- Mall
- Gedung perkantoran
- Basement/underground
- Stadion
- Bandara

**Istilah lain**: **Lampsite**

---

## 7. PERBANDINGAN TOPOLOGI SITE

| Aspek               | Distributed RAN (Macro) | Centralized RAN (BTS Hotel) | Picocell              |
| ------------------- | ----------------------- | --------------------------- | --------------------- |
| **Tower**           | Tinggi (20-30 m)        | Rendah (12-15 m)            | Tidak ada             |
| **RRH & BBU**       | Satu tempat             | Terpisah jauh               | Terpisah              |
| **Fronthaul**       | CPRI (lokal)            | CPRI via Fiber (jauh)       | Fiber (dalam gedung)  |
| **Shelter**         | Perlu (besar)           | Tidak perlu                 | Perlu (kecil)         |
| **AC**              | Perlu                   | Tidak perlu (RRH outdoor)   | Perlu                 |
| **CAPEX**           | Tinggi                  | Rendah                      | Sedang                |
| **OPEX**            | Tinggi                  | Rendah                      | Sedang                |
| **Coverage**        | Luas                    | Sedang                      | Kecil (indoor)        |
| **Troubleshooting** | Mudah                   | Sulit (jika fiber putus)    | Sedang                |
| **Dipakai di**      | Pedesaan, area luas     | Perkotaan padat             | Indoor (mall, gedung) |

---

## 8. TEKNOLOGI TRANSPORT BACKHAUL

### Teknologi Lama

#### SDH (Synchronous Digital Hierarchy)

- **Sinkron**
- Teknologi lama
- Kecepatan: rendah

**Varian**: **EoSDH (Ethernet over SDH)**

#### ATM (Asynchronous Transfer Mode)

- **Asinkron**
- Teknologi lama
- Kecepatan: sedang

### Teknologi Baru

#### MPLS (Multiprotocol Label Switching)

- **Asinkron**
- Teknologi modern
- Penggabungan ATM + IP
- Kecepatan: tinggi
- **Paling banyak dipakai sekarang**

**Fungsi MPLS**:

- Agregasi traffic dari banyak site
- Routing cepat
- QoS (Quality of Service)

**Diagram Backhaul**:

```
[BBU 1] ┐
[BBU 2] ├──  MPLS Network  ──→ [EPC]
[BBU 3] ┤    (Aggregation)     (Core)
[BBU 4] ┘
```

---

## 9. ALARM MANAGEMENT

### Jenis Alarm

Saat ada masalah di network, muncul **alarm** di NOC (Network Operation Center).

**3 jenis alarm**:

| Alarm        | Tingkat | Eskalasi             | Contoh                           |
| ------------ | ------- | -------------------- | -------------------------------- |
| **Minor**    | Ringan  | Level Supervisor     | Kesalahan kecil (tidak critical) |
| **Major**    | Sedang  | Level Manager        | Masalah sedang (impact sedang)   |
| **Critical** | Berat   | Level GM/VP/Direktur | Tower roboh, fiber putus         |

### Eskalasi Alarm

**Minor**:

- Ditangani oleh supervisor
- Monitoring rutin

**Major**:

- Dilaporkan ke manager
- Perlu penanganan cepat

**Critical**:

- Dilaporkan ke GM, VP, bahkan Direktur
- Emergency (site mati, revenue loss)

**Contoh Critical**:

- Fiber putus di BTS Hotel → semua RRH mati
- Tower roboh → bencana + site mati
- Site platinum mati (revenue loss besar)

---

## 10. SITE PLATINUM

### Apa itu Site Platinum?

**Site Platinum** = Site dengan **traffic sangat tinggi** dan **revenue sangat besar**.

**Contoh lokasi**:

- Bandara Soekarno-Hatta
- Mall besar (Grand Indonesia, Plaza Senayan)
- Stadion
- Pusat bisnis (Sudirman, Thamrin)

**Revenue**:

- **1 site Platinum** bisa menghasilkan **Rp 250-300 juta per hari** (kondisi normal)!

**Kenapa begitu besar?**

- Traffic sangat tinggi
- User sangat banyak
- Semua transaksi lewat site ini

**Konsekuensi jika site mati**:

- Revenue loss sangat besar
- Alarm: **Critical**
- Eskalasi: sampai direktur
- Harus segera diperbaiki (target: beberapa jam)

---

## 11. CAPEX VS OPEX

### CAPEX (Capital Expenditure)

**CAPEX** = Biaya investasi awal.

**Contoh**:

- Pembelian tower
- Pembelian BBU, RRH
- Pembelian fiber optic
- Pembangunan shelter
- Instalasi site baru

**Perbandingan**:

- **Distributed RAN**: CAPEX tinggi (perlu shelter, genset, AC)
- **Centralized RAN**: CAPEX rendah (tidak perlu shelter besar)

### OPEX (Operational Expenditure)

**OPEX** = Biaya operasional.

**Contoh**:

- Listrik
- AC
- Genset (bahan bakar)
- Maintenance
- Sewa lahan
- Gaji teknisi

**Perbandingan**:

- **Distributed RAN**: OPEX tinggi (AC, genset, listrik besar)
- **Centralized RAN**: OPEX rendah (RRH outdoor, tidak perlu AC)

**Strategi operator**:

- Minimalisir CAPEX & OPEX
- Gunakan **Centralized RAN** di perkotaan
- Gunakan **Distributed RAN** di pedesaan

---

## 12. POINTING MICROWAVE

### Apa itu Pointing?

**Pointing** = Mengarahkan antena microwave agar saling terhubung (TX dan RX).

**Kapan dipakai?**

- Jika tidak ada fiber optic
- Fronthaul atau backhaul menggunakan microwave

**Prinsip**:

- Antena TX (transmitter) dan RX (receiver) harus **saling menghadap**
- Idealnya **LOS (Line of Sight)** → tidak ada penghalang

**Tantangan**:

- Jika banyak penghalang (gedung, pohon) → **NLOS (Non-Line of Sight)**
- Harus cari posisi yang tepat

**Contoh**:

- Gedung ke LED kampus: banyak penghalang → susah pointing

---

## 13. SOLAR CELL & GENSET

### Kapan Dipakai?

**Solar Cell & Genset** dipakai untuk site di:

- Daerah terpencil (tidak ada listrik PLN)
- Kepulauan (Kepulauan Seribu, dll)
- Pedalaman

**Fungsi**:

- **Solar Cell**: Energi dari matahari
- **Genset**: Backup jika matahari tidak ada

**Contoh kasus**:

- Site di Kepulauan Seribu: pakai solar cell + genset
- Penjaga site: sekaligus merawat perangkat (ganti battery, isi genset)

---

## 14. RELOKASI SITE

### Kenapa Site Dipindah?

**Alasan**:

- Revenue menurun (traffic turun)
- Kompetitor pasang site berdekatan
- OPEX tidak sebanding dengan revenue

**Contoh**:

- Dulu: Site di Kalimantan menghasilkan revenue tinggi
- Sekarang: Kompetitor banyak → traffic turun → revenue turun
- Keputusan: Site direlokasi ke tempat lain yang lebih menguntungkan

**Proses relokasi**:

- Bongkar site lama
- Pasang di lokasi baru
- Konfigurasi ulang
- **Tidak mudah!** (pekerjaan besar)

---

## 15. SDR (Software Defined Radio)

### Apa itu SDR?

**SDR** = Teknologi radio yang dikonfigurasi via **software**, bukan hardware.

**Fungsi**:

- Aktifkan/nonaktifkan teknologi (2G, 3G, 4G) **tanpa ganti hardware**
- Cukup konfigurasi via software

**Contoh**:

- Site awalnya 3G → mau upgrade ke 4G
- Tidak perlu ganti BBU
- Cukup aktifkan modul 4G di BBU (via remote)

**Keuntungan**:

- ✅ Fleksibel
- ✅ Hemat biaya (tidak perlu ganti hardware)
- ✅ Bisa remote dari pusat

**Tren operator sekarang**:

- **Sunset 3G** (matikan 3G)
- **Aktifkan 4G** di site yang sama

---

## 16. TOWER SHARING (TBG, PROTELINDO)

### Apa itu Tower Sharing?

**Tower Sharing** = Satu tower dipakai oleh **banyak operator**.

**Perusahaan tower**:

- **TBG (Tower Bersama Group)**
- **Protelindo**
- **STP (Solusi Tunas Pratama)**

**Keuntungan operator**:

- ✅ Tidak perlu bangun tower sendiri
- ✅ Hemat CAPEX & OPEX
- ✅ Fokus ke operasional, bukan maintenance tower

**Keuntungan perusahaan tower**:

- ✅ Sewa dari banyak operator
- ✅ Revenue stabil

**Trend**:

- Operator **jual/sewa** site ke tower company
- Fokus operator: jaringan, bukan aset fisik

---

## GLOSSARY

| Istilah             | Kepanjangan                        | Artinya                                  |
| ------------------- | ---------------------------------- | ---------------------------------------- |
| **Backhaul**        | -                                  | Jaringan transport RAN ke Core           |
| **Fronthaul**       | -                                  | Jaringan RRH ke BBU                      |
| **RRH**             | Remote Radio Head                  | Perangkat radio dekat antena             |
| **BBU**             | Baseband Unit                      | Perangkat baseband processing            |
| **CPRI**            | Common Public Radio Interface      | Protokol fronthaul (RRH-BBU)             |
| **Distributed RAN** | -                                  | RRH & BBU satu tempat (Macro Site)       |
| **Centralized RAN** | -                                  | RRH & BBU terpisah (BTS Hotel)           |
| **Picocell**        | -                                  | Small cell untuk indoor                  |
| **SDH**             | Synchronous Digital Hierarchy      | Teknologi transport (sinkron)            |
| **ATM**             | Asynchronous Transfer Mode         | Teknologi transport (asinkron)           |
| **MPLS**            | Multiprotocol Label Switching      | Teknologi transport modern               |
| **EoSDH**           | Ethernet over SDH                  | Ethernet via SDH                         |
| **CAPEX**           | Capital Expenditure                | Biaya investasi awal                     |
| **OPEX**            | Operational Expenditure            | Biaya operasional                        |
| **NOC**             | Network Operation Center           | Pusat monitoring jaringan                |
| **TTC**             | Telkomsel Telecommunication Center | Central office Telkomsel                 |
| **SDR**             | Software Defined Radio             | Radio yang dikonfigurasi via software    |
| **TBG**             | Tower Bersama Group                | Perusahaan tower sharing                 |
| **LOS**             | Line of Sight                      | Jalur pandang langsung (tidak terhalang) |
| **NLOS**            | Non-Line of Sight                  | Jalur terhalang                          |

---

## FAQ

**Q: Apa beda fronthaul dan backhaul?**  
A:

- **Fronthaul**: RRH ↔ BBU (dekat user)
- **Backhaul**: BBU ↔ Core (dekat central office)

**Q: Apa beda Distributed RAN dan Centralized RAN?**  
A:

- **Distributed RAN**: RRH & BBU **satu tempat** (Macro Site)
- **Centralized RAN**: RRH & BBU **terpisah** (BTS Hotel)

**Q: Kapan pakai Macro Site, kapan pakai BTS Hotel?**  
A:

- **Macro Site**: Daerah pedesaan, area luas
- **BTS Hotel**: Perkotaan padat, banyak site berdekatan

**Q: Apa itu CPRI?**  
A: Protokol untuk menghubungkan RRH dengan BBU, menggunakan Fiber Optic.

**Q: Apa itu Site Platinum?**  
A: Site dengan traffic sangat tinggi dan revenue sangat besar (contoh: Bandara, Mall besar). Revenue bisa **Rp 250-300 juta per hari**!

**Q: Kenapa BTS Hotel lebih hemat?**  
A:

- RRH outdoor (tidak perlu AC)
- BBU terpusat (maintenance lebih mudah)
- CAPEX & OPEX lebih rendah

**Q: Apa risiko BTS Hotel?**  
A: Jika fiber putus, semua RRH mati (cut-off).

**Q: Apa itu SDR?**  
A: Software Defined Radio → teknologi radio yang dikonfigurasi via software (tidak perlu ganti hardware untuk upgrade 3G ke 4G).

**Q: Kenapa operator jual/sewa site ke tower company?**  
A: Agar fokus ke operasional jaringan, bukan maintenance tower. Hemat CAPEX & OPEX.

---

## TIPS PENTING

✅ **Fronthaul vs Backhaul**:

- **Fronthaul**: RRH → BBU (dekat user)
- **Backhaul**: BBU → Core (dekat central office)

✅ **Topologi Site**:

- **Macro**: Tower tinggi, RRH & BBU satu tempat (pedesaan)
- **BTS Hotel**: Tower rendah, RRH & BBU terpisah (perkotaan)
- **Picocell**: Indoor (mall, gedung)

✅ **CPRI**:

- Protokol fronthaul (RRH-BBU)
- Media: Fiber Optic
- Bisa juga pakai Microwave

✅ **CAPEX & OPEX**:

- **Macro**: CAPEX & OPEX tinggi (shelter, AC, genset)
- **BTS Hotel**: CAPEX & OPEX rendah (RRH outdoor)

✅ **Alarm**:

- Minor → Supervisor
- Major → Manager
- Critical → GM/VP/Direktur

✅ **Site Platinum**:

- Revenue: Rp 250-300 juta/hari
- Jika mati → alarm Critical

✅ **SDR**:

- Upgrade 3G ke 4G tanpa ganti hardware
- Cukup konfigurasi software

✅ **Tower Sharing**:

- Hemat CAPEX & OPEX
- Fokus operator: jaringan, bukan aset

---

## 17. VENDOR PERANGKAT

### Vendor RRH & BBU

**Vendor utama** yang menyediakan perangkat RRH dan BBU:

1. **ZTE**
2. **Huawei**
3. **Nokia (Nokia Siemens Networks)**
4. **Ericsson**

**Prinsip compatibility**:

- Jika RRH dari **ZTE** → BBU harus dari **ZTE**
- Jika RRH dari **Huawei** → BBU harus dari **Huawei**
- Jika RRH dari **Nokia** → BBU harus dari **Nokia**

> **Tidak bisa mix-and-match!** RRH dan BBU harus dari vendor yang sama.

**Alasan**:

- Protokol proprietary (milik vendor)
- Integrasi sistem
- Garansi dan support

---

## 18. PERAN PERUSAHAAN DI INDUSTRI TELCO

### Jenis Perusahaan

#### 1. Vendor Equipment (ZTE, Huawei, Nokia, Ericsson)

**Peran**:

- Menyediakan perangkat RRH, BBU
- Instalasi perangkat
- Konfigurasi awal

**Produk**:

- RRH
- BBU
- Antena
- Software management

#### 2. Tower Company (TBG, Protelindo, STP)

**Peran**:

- Membangun tower
- Maintenance tower
- Menyewakan tower ke operator

**Tugas**:

- Desain struktur tower
- Instalasi tower
- Perawatan fisik tower
- Cable management

#### 3. Transport Provider (Enis, dll)

**Peran**:

- Menyediakan **jaringan transport** (fronthaul & backhaul)
- Instalasi fiber optic
- Instalasi microwave
- Koneksi satelit

**Teknologi**:

- Fiber Optic
- Microwave
- Satellite
- Copper (jarang)

**Contoh**: **Enis** → penyedia transport untuk menghubungkan RRH ke BBU dan BBU ke Core.

#### 4. Optimization Company (Picocell, dll)

**Peran**:

- **Drive test**: Mengukur kualitas sinyal
- **Optimization**: Meningkatkan performa jaringan
- Troubleshooting coverage

**Aktivitas**:

- Survey coverage
- Signal measurement
- Parameter tuning
- Recommendation untuk improvement

#### 5. System Integrator (iMoVie, dll)

**Peran**:

- Integrasi sistem end-to-end
- Aplikasi monitoring & management
- Desain network optimization
- Support untuk **core network** dan **RAN**

**Fokus**:

- Aplikasi untuk monitoring performa
- Dashboard untuk operator
- Integration dengan berbagai vendor
- Network Function Virtualization (NFV)

**Contoh**: **iMoVie** → menyediakan aplikasi untuk monitoring dan optimasi jaringan.

---

## 19. BTS MOBILE (BTS PORTABLE)

### Apa itu BTS Mobile?

**BTS Mobile** = BTS portable yang bisa dipindah-pindah, biasanya dipasang di kendaraan (mobil/truk).

**Kapan dipakai?**

- Event besar (konser, festival, pameran)
- Bencana alam (untuk emergency network)
- Area dengan traffic tiba-tiba tinggi

**Ciri-ciri**:

- **Distributed RAN**: RRH dan BBU dalam satu unit
- **Perangkat outdoor**: BBU outdoor (tidak perlu AC)
- **Tower portable**: Tinggi 10-15 meter (bisa dinaikkan/diturunkan)
- **Transportasi**: Microwave (bukan fiber)

### Kenapa Distributed RAN?

**Alasan**:

- ✅ Mudah troubleshooting (RRH & BBU satu tempat)
- ✅ Tidak perlu shelter khusus
- ✅ BBU outdoor (tidak perlu AC)
- ✅ Siap pakai (plug-and-play)

**Kenapa pakai Microwave?**

- ✅ Tidak perlu tarik fiber (fleksibel)
- ✅ Cepat setup
- ✅ Point-to-point (LOS)

### Karakteristik

**Kapasitas**:

- Sama dengan BTS regular
- Bisa handle traffic tinggi

**Mobilitas**:

- Bisa dipindah ke lokasi lain
- Setup cepat (beberapa jam)

**Konfigurasi**:

- Remote configuration (dari NOC)
- Bisa aktifkan 2G, 3G, 4G sesuai kebutuhan

---

## 20. CLOUD RAN (C-RAN)

### Apa itu Cloud RAN?

**Cloud RAN (C-RAN)** = Virtualisasi BBU di cloud/server terpusat.

**Konsep**:

- **RRH**: Tetap di lapangan (fisik)
- **BBU**: Divirtualisasi di cloud (tidak ada hardware fisik BBU)

**Diagram**:

```
[RRH 1] ─┐
[RRH 2] ─┤
[RRH 3] ─┤─ Fiber ─→ [Cloud BBU Pool] → [Core]
[RRH 4] ─┤           (Virtual BBU di server)
[RRH 5] ─┘
```

**Prinsip**:

- Semua fungsi BBU dijalankan di **virtual machine (VM)** atau **container**
- BBU tidak ada sebagai hardware fisik
- Satu server bisa menjalankan banyak virtual BBU

### Keuntungan Cloud RAN

✅ **Hemat CAPEX**:

- Tidak perlu beli BBU hardware untuk setiap site
- Satu server bisa handle banyak site

✅ **Hemat OPEX**:

- Maintenance terpusat
- Hemat listrik (tidak perlu BBU di setiap site)
- Tidak perlu AC di setiap site

✅ **Fleksibilitas**:

- Scale up/down sesuai kebutuhan
- Aktifkan/nonaktifkan sesuai traffic
- Resource sharing antar site

✅ **Upgrade mudah**:

- Upgrade software (tidak perlu ganti hardware)
- Rollout fitur baru lebih cepat

### Teknologi Pendukung

#### NFV (Network Function Virtualization)

**NFV** = Virtualisasi fungsi jaringan (MME, SGW, PGW, BBU, dll).

**Prinsip**:

- Fungsi jaringan (yang tadinya hardware) → dijadikan software
- Jalan di virtual machine (VM)

**Contoh virtualisasi**:

- **MME** → Virtual MME (VM)
- **SGW** → Virtual SGW (VM)
- **PGW** → Virtual PGW (VM)
- **PCRF** → Virtual PCRF (VM)
- **BBU** → Virtual BBU (VM)

#### Container

**Container** = Teknologi virtualisasi yang lebih ringan dari VM.

**Perbedaan VM vs Container**:

| Aspek         | Virtual Machine (VM)   | Container               |
| ------------- | ---------------------- | ----------------------- |
| **Resource**  | Besar (perlu OS penuh) | Kecil (share OS host)   |
| **Speed**     | Lambat startup         | Cepat startup           |
| **Efisiensi** | Kurang efisien         | Lebih efisien           |
| **Teknologi** | NFV                    | Container (Docker, K8s) |

**Trend**:

- Dari **NFV** (VM) → **Container**
- Container lebih efisien untuk network function

### Tahap Implementasi Cloud RAN

**Tahap 1**: Virtualisasi **Core Network** (EPC)

- MME, SGW, PGW, PCRF → divirtualisasi
- Sudah banyak diterapkan

**Tahap 2**: Virtualisasi **BBU** (Cloud RAN)

- BBU → divirtualisasi
- Sedang dalam tahap pengembangan

**Tahap 3**: Virtualisasi **RRH**

- RRH → divirtualisasi?
- Masih sulit (karena RF processing butuh hardware khusus)
- **Antena** → kemungkinan tetap fisik

**Prediksi masa depan**:

- Semua BBU akan divirtualisasi
- RRH mungkin sebagian divirtualisasi
- Antena tetap fisik (tidak bisa divirtualisasi)

---

## 21. IP-BASED NETWORK

### Semua Sudah IP

**4G LTE** = **All IP Network**

**Artinya**:

- Semua komunikasi menggunakan **IP (Internet Protocol)**
- Voice, SMS, data → semua via IP
- Tidak ada lagi circuit-switched (seperti 2G/3G)

**Komponen yang pakai IP**:

- **Fronthaul**: RRH ↔ BBU (CPRI over IP atau Ethernet)
- **Backhaul**: BBU ↔ Core (MPLS/IP)
- **Core**: Semua EPC (MME, SGW, PGW) → IP-based

**Media transport**:

- **Fiber Optic** → IP over Fiber
- **Microwave** → IP over Microwave
- **Satellite** → IP over Satellite

**Keuntungan All IP**:

- ✅ Standarisasi (semua pakai IP)
- ✅ Integrasi mudah
- ✅ Fleksibel
- ✅ Mendukung berbagai service (voice, video, data)

---

## 22. CENTRALIZED vs DISTRIBUTED vs CLOUD RAN

### Perbandingan

| Aspek               | Distributed RAN        | Centralized RAN (BTS Hotel) | Cloud RAN              |
| ------------------- | ---------------------- | --------------------------- | ---------------------- |
| **RRH**             | Satu tempat dengan BBU | Terpisah dari BBU           | Terpisah dari BBU      |
| **BBU**             | Hardware fisik         | Hardware fisik (terpusat)   | **Virtual** (di cloud) |
| **Lokasi BBU**      | Di site                | Di central office           | Di cloud/data center   |
| **CAPEX**           | Tinggi                 | Sedang                      | **Rendah**             |
| **OPEX**            | Tinggi                 | Sedang                      | **Rendah**             |
| **Fleksibilitas**   | Rendah                 | Sedang                      | **Tinggi**             |
| **Troubleshooting** | Mudah                  | Sulit (jika fiber putus)    | Mudah (remote)         |
| **Upgrade**         | Sulit (ganti hardware) | Sulit (ganti hardware)      | **Mudah (software)**   |

**Trend masa depan**:

- **Distributed RAN** → masih dipakai untuk pedesaan
- **Centralized RAN** → masih dipakai untuk perkotaan
- **Cloud RAN** → akan dominan di masa depan

---

## GLOSSARY TAMBAHAN

| Istilah       | Kepanjangan                           | Artinya                            |
| ------------- | ------------------------------------- | ---------------------------------- |
| **C-RAN**     | Cloud Radio Access Network            | RAN dengan BBU virtual di cloud    |
| **NFV**       | Network Function Virtualization       | Virtualisasi fungsi jaringan       |
| **VM**        | Virtual Machine                       | Mesin virtual (virtualisasi penuh) |
| **Container** | -                                     | Virtualisasi ringan (share OS)     |
| **ZTE**       | Zhongxing Telecommunication Equipment | Vendor Tiongkok                    |
| **Huawei**    | -                                     | Vendor Tiongkok                    |
| **Nokia**     | -                                     | Vendor Finlandia                   |
| **Ericsson**  | -                                     | Vendor Swedia                      |
| **Enis**      | -                                     | Penyedia transport                 |
| **iMoVie**    | -                                     | System integrator                  |
| **LOS**       | Line of Sight                         | Jalur pandang langsung             |
| **NLOS**      | Non-Line of Sight                     | Jalur terhalang                    |

---

## FAQ TAMBAHAN

**Q: Apakah RRH ZTE bisa pakai BBU Huawei?**  
A: **Tidak bisa!** RRH dan BBU harus dari vendor yang sama (ZTE → ZTE, Huawei → Huawei, Nokia → Nokia).

**Q: Apa itu BTS Mobile?**  
A: BTS portable yang bisa dipindah-pindah, biasanya untuk event atau emergency. Pakai **Distributed RAN** dengan BBU outdoor (tidak perlu AC).

**Q: Kenapa BTS Mobile pakai Distributed RAN, bukan Centralized?**  
A: Karena lebih mudah troubleshooting (RRH & BBU satu tempat), tidak perlu tarik fiber, dan siap pakai (plug-and-play).

**Q: Apa itu Cloud RAN?**  
A: RAN dengan **BBU divirtualisasi** di cloud. BBU tidak ada hardware fisik, hanya software di server.

**Q: Apa keuntungan Cloud RAN?**  
A:

- Hemat CAPEX & OPEX (tidak perlu beli BBU hardware banyak)
- Fleksibel (scale up/down sesuai kebutuhan)
- Upgrade mudah (software, bukan hardware)

**Q: Apa itu NFV?**  
A: Network Function Virtualization → virtualisasi fungsi jaringan (MME, SGW, PGW, BBU, dll) menjadi software di VM.

**Q: Apa beda VM dan Container?**  
A:

- **VM**: Virtualisasi penuh (perlu OS lengkap), berat, lambat startup
- **Container**: Virtualisasi ringan (share OS host), ringan, cepat startup

**Q: Apakah antena bisa divirtualisasi?**  
A: **Sangat sulit**, karena antena butuh RF processing yang memerlukan hardware khusus. Kemungkinan antena tetap fisik di masa depan.

**Q: Apa peran Enis?**  
A: Penyedia **transport** (fiber optic, microwave, satellite) untuk menghubungkan RRH ke BBU dan BBU ke Core.

**Q: Apa peran iMoVie?**  
A: **System integrator** yang menyediakan aplikasi monitoring, dashboard, dan optimasi jaringan.

**Q: Kenapa 4G disebut All IP?**  
A: Karena semua komunikasi (voice, SMS, data) menggunakan **IP (Internet Protocol)**, tidak ada lagi circuit-switched.

---

## TIPS PENTING TAMBAHAN

✅ **Vendor Compatibility**:

- RRH dan BBU harus dari vendor yang sama
- Tidak bisa mix-and-match

✅ **BTS Mobile**:

- Distributed RAN (RRH & BBU satu tempat)
- BBU outdoor (tidak perlu AC)
- Transport: Microwave (bukan fiber)
- Untuk event atau emergency

✅ **Cloud RAN**:

- BBU divirtualisasi di cloud
- Hemat CAPEX & OPEX
- Fleksibel dan mudah upgrade
- Trend masa depan

✅ **NFV**:

- Virtualisasi fungsi jaringan (MME, SGW, PGW, BBU)
- Jalan di VM atau Container
- Tahap 1: Core (sudah banyak diterapkan)
- Tahap 2: BBU (sedang dikembangkan)

✅ **All IP Network**:

- 4G = All IP (semua pakai IP)
- Voice, SMS, data → semua via IP
- Media: Fiber, Microwave, Satellite → semua pakai IP

✅ **Peran Perusahaan**:

- **Vendor**: ZTE, Huawei, Nokia, Ericsson (perangkat)
- **Tower**: TBG, Protelindo (tower & maintenance)
- **Transport**: Enis (fiber, microwave, satellite)
- **Optimization**: Picocell, dll (drive test, optimasi)
- **System Integrator**: iMoVie (aplikasi, monitoring)

---

## KESIMPULAN LENGKAP

### Backhaul Network

- Menghubungkan RAN dengan Core
- Agregasi traffic dari banyak site
- Teknologi: MPLS (paling populer)

### Fronthaul vs Backhaul

- **Fronthaul**: RRH → BBU
- **Backhaul**: BBU → Core

### Backhaul Network

- Menghubungkan RAN dengan Core
- Agregasi traffic dari banyak site
- Teknologi: MPLS (paling populer)

### Fronthaul vs Backhaul

- **Fronthaul**: RRH → BBU
- **Backhaul**: BBU → Core

### Topologi Site

- **Distributed RAN (Macro)**: RRH & BBU satu tempat, tower tinggi, cakupan luas
- **Centralized RAN (BTS Hotel)**: RRH & BBU terpisah, hemat OPEX
- **Cloud RAN (C-RAN)**: BBU divirtualisasi di cloud (masa depan)
- **Picocell**: Indoor, kapasitas 1 BTS
- **BTS Mobile**: Portable, untuk event atau emergency

### CAPEX & OPEX

- **Macro**: Tinggi (shelter, AC, genset)
- **BTS Hotel**: Rendah (RRH outdoor)
- **Cloud RAN**: Paling rendah (BBU virtual, tidak perlu hardware banyak)

### Teknologi

- **CPRI**: Protokol fronthaul (Fiber Optic)
- **MPLS**: Teknologi backhaul modern
- **SDR**: Upgrade via software
- **NFV**: Virtualisasi fungsi jaringan
- **Container**: Virtualisasi ringan (lebih efisien dari VM)

### Vendor & Perusahaan

- **Vendor Equipment**: ZTE, Huawei, Nokia, Ericsson (RRH & BBU harus sama vendor!)
- **Tower Company**: TBG, Protelindo, STP (tower & maintenance)
- **Transport Provider**: Enis (fiber, microwave, satellite)
- **Optimization**: Picocell (drive test, optimasi)
- **System Integrator**: iMoVie (aplikasi, monitoring)

### Strategi Operator

- Minimize CAPEX & OPEX
- Tower sharing (TBG, Protelindo)
- Centralized RAN di perkotaan
- Cloud RAN untuk masa depan
- Relokasi site jika revenue turun
- Virtualisasi core (NFV) → sedang menuju virtualisasi BBU

### All IP Network

- 4G = All IP (semua komunikasi pakai IP)
- Voice, SMS, data → semua via IP
- Transport: Fiber, Microwave, Satellite → semua pakai IP

---

**Catatan**: Materi berikutnya mungkin akan membahas **MPLS** lebih detail di mata kuliah jaringan komputer!

---

**Selamat belajar! Pahami konsep backhaul untuk RF planning & optimization!**
