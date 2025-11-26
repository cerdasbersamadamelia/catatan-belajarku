# Introduction to LTE

## Apa itu LTE?
LTE (Long Term Evolution) adalah teknologi seluler generasi keempat (4G) yang dikembangkan oleh 3GPP. LTE menawarkan kecepatan data yang jauh lebih tinggi dan latensi yang lebih rendah dibanding teknologi 3G. Sebelum memahami LTE, kita perlu tahu dulu bagaimana teknologi ini berkembang dari generasi sebelumnya.

---

## Badan Telekomunikasi Dunia

### ITU (International Telecommunication Union)
ITU adalah badan khusus dari PBB yang menangani telekomunikasi di seluruh dunia. ITU terbagi menjadi 3 bagian:

1. **ITU-R (Radiocommunication)**
   - Fungsi: Manajemen dan pembagian frekuensi

2. **ITU-T (Telecommunication)**
   - Fungsi: Membuat standarisasi dan aturan dalam bertelekomunikasi

3. **ITU-D (Development)**
   - Fungsi: Menyediakan akses agar teknologi lebih mudah dijangkau pengguna di seluruh dunia

ITU menjadi dasar utama dalam pembuatan aturan telekomunikasi dunia.

---

## Organisasi Teknologi Seluler

Dalam teknologi seluler, ITU menjadi pedoman untuk beberapa organisasi penting:

### 1. 3GPP (3rd Generation Partnership Project)
- Organisasi kolaborasi dari 7 badan standarisasi:
  - ARIB (Jepang)
  - ATIS (Amerika Serikat)
  - CCSA (China)
  - ETSI (Eropa)
  - TSDSI (India)
  - TTA (Korea)
  - TTC (Jepang)
- **Mengembangkan:** GSM, GPRS, EDGE, WCDMA, HSPA, LTE, 5G

### 2. 3GPP2 (3rd Generation Partnership Project 2)
- Organisasi kolaborasi dari 5 badan standarisasi:
  - TIA (Amerika Utara)
  - TTA (Korea)
  - ARIB/TTC (Jepang)
  - CCSA (China)
  - ATSC (Korea)
- **Mengembangkan:** CDMA2000 dan evolusinya

### 3. IEEE (Institute of Electrical and Electronics Engineers)
- Organisasi standarisasi internasional untuk teknologi elektrik dan elektronik
- **Mengembangkan:** WiMAX (IEEE 802.16), WiFi (IEEE 802.11), dan standar lainnya

---

## Perkembangan Teknologi 3GPP

### Project Awal: IMT-2000
**IMT-2000** (International Mobile Telecommunications-2000) adalah standar ITU untuk komunikasi seluler 3G. Proyek ini menjadi dasar pengembangan teknologi 3G di seluruh dunia, termasuk WCDMA yang dikembangkan oleh 3GPP.

### Evolusi Teknologi:

| Generasi | Teknologi | Kecepatan Maksimal |
|----------|-----------|-------------------|
| **2G** | GSM | ~14 Kbps |
| **2.5G** | GPRS | ~56-114 Kbps |
| **2.75G** | EDGE | ~384 Kbps |
| **3G** | WCDMA | ~384 Kbps - 2 Mbps |
| **3.5G** | HSDPA | ~14 Mbps |
| **3.75G** | HSPA+ | ~42 Mbps |
| **4G** | LTE | ~100-300 Mbps |
| **4.5G** | LTE-Advanced | ~1 Gbps |
| **5G** | NR (New Radio) | ~10-20 Gbps |

Teknologi ini terus berkembang mengikuti kemajuan zaman.

---

## Perkembangan Teknologi 3GPP2

### Project Awal: CDMA2000
CDMA2000 adalah standar 3G yang dikembangkan 3GPP2, evolusi dari teknologi cdmaOne (IS-95). Proyek ini setara dengan IMT-2000 dari ITU.

### Evolusi Teknologi:

| Generasi | Teknologi | Kecepatan Maksimal |
|----------|-----------|-------------------|
| **2G** | cdmaOne (IS-95) | ~14.4 Kbps |
| **3G** | CDMA2000 1x (1xRTT) | ~153 Kbps |
| **3G** | CDMA2000 1xEV-DO Rev. 0 | ~2.4 Mbps |
| **3.5G** | CDMA2000 1xEV-DO Rev. A | ~3.1 Mbps |
| **3.75G** | CDMA2000 1xEV-DO Rev. B | ~4.9 Mbps |
| **4G** | UMB (dibatalkan) | - |

**Catatan:** UMB (Ultra Mobile Broadband) adalah rencana 4G dari 3GPP2, tapi **dibatalkan tahun 2008** karena operator lebih memilih LTE.

---

## Perkembangan Teknologi IEEE

### WiMAX (IEEE 802.16)
WiMAX adalah teknologi broadband wireless yang dikembangkan IEEE sebagai alternatif 4G.

| Teknologi | Kecepatan Maksimal | Status |
|-----------|-------------------|--------|
| **WiMAX (IEEE 802.16e)** | ~40 Mbps | Mobile |
| **WiMAX 2 (IEEE 802.16m)** | ~100 Mbps | 4G |

**Catatan:** WiMAX kalah bersaing dengan LTE dan sebagian besar jaringan WiMAX sudah ditutup atau dimigrasi ke LTE.

---

## Teknologi di Indonesia

### Teknologi yang Digunakan
Indonesia menggunakan teknologi dari **3GPP**, yaitu:
- 2G (GSM)
- 2.5G (GPRS)
- 2.75G (EDGE)
- 3G (WCDMA)
- 3.5G (HSDPA)
- 4G (LTE)

### Kenapa Tidak Pakai 3GPP2 dan IEEE?

**CDMA2000 (3GPP2):**
- Indonesia **sempat menggunakan** CDMA2000 melalui operator:
  - Telkom Flexi
  - Bakrie Esia
  - Smartfren
- Operator CDMA akhirnya **beralih atau tutup** karena:
  - Ekosistem device yang terbatas
  - Roaming internasional sulit
  - LTE lebih superior secara teknis
  - Smartfren akhirnya migrasi full ke LTE

**WiMAX (IEEE):**
- Indonesia **sempat mencoba** WiMAX untuk fixed broadband
- Gagal berkembang karena:
  - LTE lebih cepat mendapat dukungan global
  - Device WiMAX sangat terbatas
  - Investasi infrastruktur tidak sebanding
  - Lisensi dan regulasi tidak mendukung

---

## Keunggulan LTE

1. **Kecepatan Tinggi**
   - Download: hingga 300 Mbps
   - Upload: hingga 75 Mbps

2. **Latensi Rendah**
   - <10 ms (jauh lebih cepat dari 3G yang ~100 ms)

3. **All-IP Network (Flat Architecture)**
   - LTE menggunakan **EPC (Evolved Packet Core)** - full IP network
   - Voice over LTE (VoLTE) untuk panggilan suara
   - Lebih efisien dibanding circuit-switched di 2G/3G
   - Menghilangkan RNC (Radio Network Controller), jadi lebih sederhana

4. **Teknologi OFDMA**
   - Orthogonal Frequency Division Multiple Access
   - Lebih efisien dalam penggunaan spektrum frekuensi

5. **MIMO (Multiple Input Multiple Output)**
   - Menggunakan multiple antena untuk meningkatkan throughput

---

## LTE di Indonesia

### Band Frekuensi yang Digunakan
Operator Indonesia menggunakan berbagai band LTE:
- **Band 3 (1800 MHz)** - paling umum
- **Band 5 (850 MHz)** - coverage luas
- **Band 8 (900 MHz)** - coverage area
- **Band 40 (2300 MHz)** - TDD, high capacity

### Operator LTE di Indonesia
Semua operator utama sudah menggunakan LTE:
- **Telkomsel** - coverage terluas
- **Indosat Ooredoo (Indosat)** 
- **XL Axiata**
- **Tri Indonesia (3)**
- **Smartfren** - dulunya CDMA, sekarang full LTE

---

## Kesimpulan

**4G LTE** adalah teknologi seluler generasi keempat yang dikembangkan oleh organisasi **3GPP**. LTE merupakan hasil evolusi panjang dari teknologi 2G hingga 3.5G, dan terus berkembang hingga sekarang ke teknologi 5G.

**Mengapa LTE menang?**
- Dukungan global dari semua vendor besar
- Ekosistem device yang luas
- Backward compatible dengan GSM/WCDMA
- Performa superior (kecepatan, latensi, efisiensi spektrum)
- Investasi infrastruktur lebih ekonomis

Di Indonesia, teknologi 3GPP (GSM → LTE) menjadi standar karena dukungan regulasi, ekosistem yang matang, dan adopsi global yang masif.
