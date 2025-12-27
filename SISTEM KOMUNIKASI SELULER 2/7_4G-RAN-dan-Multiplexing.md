# 7. 4G RAN (RADIO ACCESS NETWORK)

## PENGANTAR

Materi ini membahas **Radio Access Network (RAN) 4G**, yang sangat penting karena konsep yang dipelajari di 4G ini juga berlaku untuk 5G. RAN adalah bagian dari jaringan seluler yang menghubungkan perangkat user ke core network.

**Topik yang dibahas**:

1. Radio Interface di 4G
2. Frequency allocation untuk seluler
3. Teknik modulasi: OFDM dan SC-FDMA
4. Cyclic Prefix (CP)
5. TDD vs FDD
6. Resource Element (RE)
7. SC-FDMA untuk uplink

---

## 1. TEKNIK MULTIPLEXING

### Apa itu Multiplexing?

**Multiplexing** = Cara **membagi** kanal agar bisa digunakan oleh banyak user.

> **BUKAN menggabungkan**, tapi **membagi** kanal yang terbatas agar bisa dipakai banyak orang.

**Prinsip dasar**: Kanal (media transmisi) itu terbatas. Dengan multiplexing, kita bagi-bagi agar bisa dipakai bersama-sama.

### Perbedaan Multiplexing vs Modulasi

| Aspek         | Multiplexing                    | Modulasi                                       |
| ------------- | ------------------------------- | ---------------------------------------------- |
| **Tujuan**    | Membagi kanal untuk banyak user | Menumpangkan informasi ke carrier              |
| **Fungsi**    | Berbagi media transmisi         | Mengubah data menjadi sinyal yang bisa dikirim |
| **Parameter** | Frekuensi, waktu, atau kode     | Amplitude, frequency, phase                    |

---

## 2. JENIS-JENIS MULTIPLEXING

### 2.1 FDMA (Frequency Division Multiple Access)

**Konsep**: Membagi kanal berdasarkan **frekuensi**.

```
Frekuensi
  ↑
F5 |  User 5
F4 |  User 4
F3 |  User 3
F2 |  User 2
F1 |  User 1
   └────────→ Waktu (sama untuk semua)
```

**Kelebihan**:

- ✅ Mudah implementasi (tinggal tambah frekuensi baru)
- ✅ Mendukung **frequency reuse** (frekuensi bisa dipakai lagi di cell lain)

**Kelemahan**:

- ❌ Kapasitas terbatas (frekuensi terbatas)
- ❌ Rentan interferensi antar frekuensi

**Dipakai di**: Teknologi **1G (analog)**, seperti AMPS (Analog Mobile Phone System).

**Solusi interferensi**: Tambahkan **guard band** (pembatas) antar frekuensi.

---

### 2.2 TDMA (Time Division Multiple Access)

**Konsep**: Membagi kanal berdasarkan **waktu**.

```
Waktu
  ↑
T5 |  User 5
T4 |  User 4
T3 |  User 3
T2 |  User 2
T1 |  User 1
   └────────→ Frekuensi (sama untuk semua)
```

**Kelebihan**:

- ✅ **Tidak ada interferensi** (karena beda waktu)
- ✅ Efisiensi spektrum lebih baik daripada FDMA

**Kelemahan**:

- ❌ Kapasitas slot terbatas
- ❌ Rentan terhadap error

**Solusi error**: Gunakan **FEC (Forward Error Correction)** untuk deteksi dan koreksi error.

**Dipakai di**: Teknologi **2G (GSM)**.

---

### 2.3 CDMA (Code Division Multiple Access)

**Konsep**: Membagi kanal berdasarkan **kode** (bukan frekuensi atau waktu).

- Semua user menggunakan frekuensi dan waktu yang **sama**
- Setiap user diberi **kode unik** untuk dibedakan

```
        Frekuensi
          ↑
          |  User 1 (Kode C1)
          |  User 2 (Kode C2)
F1, T1    |  User 3 (Kode C3)
          |  User 4 (Kode C4)
          └────────→ Waktu
```

**Kelebihan**:

- ✅ Kapasitas user bisa lebih banyak
- ✅ Tidak ada interferensi (karena kode berbeda)

**Kelemahan**:

- ❌ Algoritma lebih kompleks

**Dipakai di**: Teknologi **3G (WCDMA)**, dengan bandwidth **5 MHz**.

**Teknik pengkodean**: Menggunakan **orthogonal code** untuk membedakan user.

---

### 2.4 OFDMA (Orthogonal Frequency Division Multiple Access)

**Konsep**: Evolusi dari FDMA, dengan frekuensi yang **lebih rapat** dan **orthogonal**.

**Tujuan OFDMA**:

1. Menambah jumlah user (banyak subcarrier)
2. Menghilangkan interferensi dengan membuat frekuensi saling **tegak lurus (orthogonal)**

**Proses**:

- Domain **frekuensi** → diubah ke domain **waktu** (menggunakan **IFFT**)
- Saat transmisi: domain waktu
- Saat penerima: diubah lagi ke domain frekuensi (menggunakan **FFT**)

**Kelebihan OFDMA**:

- ✅ Kapasitas user sangat banyak
- ✅ Tidak ada interferensi (karena orthogonal)
- ✅ Efisiensi spektrum sangat tinggi
- ✅ Mendukung kondisi **NLOS (Non-Line of Sight)**
- ✅ Mengurangi **CAPEX** dan **OPEX** operator

**Dipakai di**: Teknologi **4G LTE** (downlink).

---

## 3. OFDMA: KONSEP ORTHOGONAL

### Kenapa Perlu Orthogonal?

FDMA tradisional:

- Frekuensi rapat → rentan interferensi
- Perlu guard band besar → mengurangi efisiensi

OFDMA:

- Frekuensi rapat → **tanpa interferensi**
- Caranya? Buat frekuensi **orthogonal** (tegak lurus)

### Cara Membuat Orthogonal

**Langkah**:

1. **Domain frekuensi** (banyak subcarrier)
2. **IFFT** → ubah ke domain waktu
3. **Cyclic Prefix (CP)** → tambahkan guard
4. **Transmisi** → kirim via RF
5. **FFT** → ubah kembali ke domain frekuensi
6. **Demodulasi** → data diterima

**Diagram proses**:

```
[Data] →  [Modulasi]  → [IFFT] →  [CP]  → [RF TX]
                                             ↓
[Data] ← [Demodulasi] ← [FFT] ← [RM CP] ← [RF RX]
```

---

## 4. CYCLIC PREFIX (CP)

### Apa itu Cyclic Prefix?

**Cyclic Prefix (CP)** = Guard band untuk mencegah interferensi antar simbol.

**Cara kerja**:

- Ambil **bagian akhir** simbol
- Taruh di **bagian depan** simbol
- Berfungsi sebagai **pembatas** antar user

**Contoh**:

```
Simbol asli:  [A B C D E F G]
Dengan CP:    [F G | A B C D E F G]
               ↑ ↑
            CP (guard)
```

**Penting**:

- ✅ Informasi **tidak berkurang**
- ✅ Hanya sebagian simbol yang di-copy ke depan
- ✅ Mencegah interferensi antar user

### Jenis Cyclic Prefix

| Jenis           | Jumlah Simbol | Penggunaan           |
| --------------- | ------------- | -------------------- |
| **Normal CP**   | 7 simbol      | Umumnya dipakai      |
| **Extended CP** | 6 simbol      | Untuk kondisi khusus |

---

## 5. OFDM vs SC-FDMA

### Kenapa 4G Pakai Dua Modulasi Berbeda?

- **Downlink**: OFDM (multicarrier)
- **Uplink**: SC-FDMA (single carrier)

**Alasan**:

| Aspek           | Downlink              | Uplink                   |
| --------------- | --------------------- | ------------------------ |
| **Jumlah user** | Banyak user           | Satu user per transmisi  |
| **Kapasitas**   | Perlu kapasitas besar | Kapasitas lebih kecil    |
| **Daya pancar** | BTS punya daya besar  | HP punya daya terbatas   |
| **Modulasi**    | Multicarrier (OFDM)   | Single carrier (SC-FDMA) |

### Masalah Multicarrier di Uplink

**Multicarrier (OFDM)** punya masalah:

1. **Frequency offset**: Rentan terhadap efek Doppler
2. **PAPR (Peak-to-Average Power Ratio)**: Daya pancar rendah

**Solusi untuk uplink**: Gunakan **single carrier (SC-FDMA)**.

### SC-FDMA (Single Carrier FDMA)

**Konsep**:

- Satu carrier → satu user
- Jika banyak user → banyak single carrier

**Kelebihan**:

- ✅ Daya pancar lebih tinggi (cocok untuk HP)
- ✅ Tidak rentan frequency offset
- ✅ PAPR lebih rendah

**Dipakai di**: **Uplink 4G LTE**.

---

## 6. MODULASI DI 4G

### Adaptive Modulation and Coding (AMC)

**Konsep**: Modulasi disesuaikan dengan jarak dan kualitas sinyal user.

**Jenis modulasi di 4G**:

| Modulasi    | Bit per Simbol | Kapasitas     | Dipakai Untuk            |
| ----------- | -------------- | ------------- | ------------------------ |
| **QPSK**    | 2 bit          | Rendah        | User jauh (sinyal lemah) |
| **16-QAM**  | 4 bit          | Sedang        | User sedang              |
| **64-QAM**  | 6 bit          | Tinggi        | User dekat (sinyal kuat) |
| **256-QAM** | 8 bit          | Sangat tinggi | User sangat dekat        |

**Prinsip**:

- User **dekat** BTS → sinyal kuat → modulasi tinggi (256-QAM)
- User **jauh** → sinyal lemah → modulasi rendah (QPSK)

**Keuntungan**:

- Semua user dapat layanan (tidak peduli jauh/dekat)
- Sistem otomatis menyesuaikan modulasi (adaptif)

---

## 7. BANDWIDTH DAN FREQUENCY ALLOCATION

### Bandwidth LTE

**Pilihan bandwidth**:

| Bandwidth  | Penggunaan          |
| ---------- | ------------------- |
| 1.4 MHz    | Jarang              |
| 3 MHz      | Jarang              |
| 5 MHz      | Jarang              |
| 10 MHz     | Kadang              |
| 15 MHz     | Kadang              |
| **20 MHz** | **Umumnya dipakai** |

**Catatan**:

- Operator umumnya pakai **20 MHz**
- Dari 20 MHz, hanya **18 MHz** dipakai untuk data
- **2 MHz** sisanya untuk **guard band** (pembatas antar operator)

### Frequency Allocation di Indonesia

**Frekuensi LTE**:

| Frekuensi | Band    | Duplexing |
| --------- | ------- | --------- |
| 800 MHz   | Band 20 | FDD       |
| 900 MHz   | Band 8  | FDD       |
| 1800 MHz  | Band 3  | FDD       |
| 2100 MHz  | Band 1  | FDD       |
| 2300 MHz  | Band 40 | TDD       |
| 2600 MHz  | Band 7  | FDD/TDD   |

**Yang umum dipakai**: Band 3 (1800 MHz), Band 40 (2300 MHz).

### Subcarrier Spacing

**Spacing antar subcarrier**: **15 kHz**

**Tujuan**:

- Mencegah **frequency offset** (akibat efek Doppler)
- Menjaga orthogonalitas antar subcarrier

---

## 8. OFDM SYMBOL

### Struktur Subcarrier di OFDM

Setiap **subcarrier** berisi:

1. **Data subcarrier**: Membawa data user (tergantung modulasi)
2. **Pilot subcarrier**: Untuk estimasi kanal dan referensi
3. **DC subcarrier**: Tidak digunakan, sebagai pembatas
4. **Null subcarrier**: Tidak digunakan, sebagai pembatas

**Diagram**:

```
[Null] [Data] [Pilot] [Data] [DC] [Data] [Pilot] [Data] [Null]
```

---

## 9. BALAI MONITORING (BALMON)

### Apa itu BALMON?

**BALMON** = Balai Monitoring Spektrum Frekuensi (dari Kementerian Kominfo)

**Tugas**:

- Mengawasi penggunaan frekuensi operator
- Memastikan operator tidak "nyaplok" frekuensi operator lain

**Contoh**:

- Telkomsel punya 20 MHz di 1800 MHz
- BALMON cek: apakah Telkomsel melanggar batas frekuensi?
- Jika melanggar → surat peringatan → penalti (bisa cabut izin)

**Metode**:

- Drive test bersama operator
- Monitoring real-time

---

## 10. PERBANDINGAN OFDM vs SC-FDMA

| Aspek                | OFDM                 | SC-FDMA              |
| -------------------- | -------------------- | -------------------- |
| **Jumlah carrier**   | Multicarrier         | Single carrier       |
| **Dipakai di**       | Downlink             | Uplink               |
| **Kapasitas**        | Tinggi (banyak user) | Rendah (per user)    |
| **Daya pancar**      | Rendah (PAPR tinggi) | Tinggi (PAPR rendah) |
| **Kompleksitas**     | Lebih kompleks       | Lebih sederhana      |
| **Frequency offset** | Rentan               | Toleran              |

---

## 11. IFFT vs FFT

### Fungsi

- **FFT (Fast Fourier Transform)**: Mengubah domain **waktu** → domain **frekuensi**
- **IFFT (Inverse FFT)**: Mengubah domain **frekuensi** → domain **waktu**

### Penggunaan di OFDM

**Transmitter (TX)**:

1. Data dalam domain frekuensi (banyak subcarrier)
2. **IFFT** → ubah ke domain waktu
3. Tambahkan **Cyclic Prefix**
4. Kirim via RF

**Receiver (RX)**:

1. Terima dari RF
2. Hilangkan Cyclic Prefix
3. **FFT** → ubah kembali ke domain frekuensi
4. Demodulasi → data asli

---

## 12. RESOURCE ELEMENT (RE)

### Apa itu Resource Element?

**Resource Element (RE)** = Unit terkecil dalam resource grid 4G.

- 1 RE = 1 subcarrier × 1 OFDM symbol
- RE membawa simbol (QPSK, 16-QAM, 64-QAM, 256-QAM)

**Resource Block (RB)**:

- 1 RB = 12 subcarrier × 7 OFDM symbols (Normal CP)
- 1 RB = 12 subcarrier × 6 OFDM symbols (Extended CP)

**Perhitungan kapasitas**: Akan dibahas di pertemuan berikutnya.

---

## 13. TDD vs FDD

### FDD (Frequency Division Duplexing)

**Konsep**: Uplink dan downlink menggunakan **frekuensi berbeda**.

**Contoh**: Band 3 (1800 MHz)

- Uplink: 1710-1785 MHz
- Downlink: 1805-1880 MHz

**Kelebihan**:

- ✅ Bisa transmisi simultan (uplink dan downlink bersamaan)

**Kekurangan**:

- ❌ Perlu 2 frekuensi (boros spektrum)

### TDD (Time Division Duplexing)

**Konsep**: Uplink dan downlink menggunakan **frekuensi sama**, tapi **waktu berbeda**.

**Contoh**: Band 40 (2300 MHz)

- Frekuensi: 2300-2400 MHz
- Uplink dan downlink: bergantian waktu

**Kelebihan**:

- ✅ Hemat spektrum (hanya butuh 1 frekuensi)
- ✅ Fleksibel (rasio uplink/downlink bisa diatur)

**Kekurangan**:

- ❌ Tidak bisa transmisi simultan

---

## 14. TIPS BELAJAR DAN KARIR

### Pentingnya Praktek

- Jangan hanya belajar teori
- Coba sendiri: MATLAB, network simulator (NS-3, srsRAN)
- Buat project lab LTE virtual (OpenAirInterface, srsRAN) → [Seperti ini contohnya](https://www.youtube.com/playlist?list=PLEBDUtoHS8Way-QADI53M11agzfnfNcOZ)

### Peluang Karir

**Bidang kerja**:

1. **Operator** (Telkomsel, Indosat, XL)
2. **Vendor** (Huawei, Ericsson, Nokia)
3. **BALMON** (monitoring spektrum)
4. **Konsultan** (RF planning, optimization)

**Skill yang dibutuhkan**:

- Pahami teori (seperti materi ini)
- Bisa simulasi (MATLAB, NS-3)
- Bisa troubleshooting
- Bisa drive test dan RF planning

---

## GLOSSARY

| Istilah     | Kepanjangan                                   | Artinya                                  |
| ----------- | --------------------------------------------- | ---------------------------------------- |
| **FDMA**    | Frequency Division Multiple Access            | Multiplexing berdasarkan frekuensi       |
| **TDMA**    | Time Division Multiple Access                 | Multiplexing berdasarkan waktu           |
| **CDMA**    | Code Division Multiple Access                 | Multiplexing berdasarkan kode            |
| **OFDMA**   | Orthogonal Frequency Division Multiple Access | Multiplexing dengan frekuensi orthogonal |
| **SC-FDMA** | Single Carrier FDMA                           | FDMA dengan single carrier (uplink)      |
| **CP**      | Cyclic Prefix                                 | Guard band untuk mencegah interferensi   |
| **IFFT**    | Inverse Fast Fourier Transform                | Ubah frekuensi → waktu                   |
| **FFT**     | Fast Fourier Transform                        | Ubah waktu → frekuensi                   |
| **PAPR**    | Peak-to-Average Power Ratio                   | Rasio daya puncak vs rata-rata           |
| **AMC**     | Adaptive Modulation and Coding                | Modulasi otomatis sesuai kondisi         |
| **RE**      | Resource Element                              | Unit terkecil resource grid              |
| **RB**      | Resource Block                                | Kumpulan 12 subcarrier × 7 symbols       |
| **FDD**     | Frequency Division Duplexing                  | Uplink/downlink beda frekuensi           |
| **TDD**     | Time Division Duplexing                       | Uplink/downlink beda waktu               |
| **BALMON**  | Balai Monitoring Spektrum Frekuensi           | Pengawas frekuensi dari Kominfo          |
| **NLOS**    | Non-Line of Sight                             | Tidak ada jalur langsung (terhalang)     |
| **CAPEX**   | Capital Expenditure                           | Biaya investasi awal                     |
| **OPEX**    | Operational Expenditure                       | Biaya operasional                        |

---

## FAQ

**Q: Apa beda multiplexing dan modulasi?**  
A:

- **Multiplexing**: Membagi kanal untuk banyak user
- **Modulasi**: Menumpangkan data ke carrier

**Q: Kenapa 4G pakai OFDM dan SC-FDMA (dua modulasi berbeda)?**  
A:

- **Downlink**: Banyak user → pakai OFDM (multicarrier, kapasitas besar)
- **Uplink**: Per user → pakai SC-FDMA (single carrier, daya pancar lebih efisien untuk HP)

**Q: Apa itu Cyclic Prefix?**  
A: Guard band yang diambil dari bagian akhir simbol, ditaruh di depan, untuk mencegah interferensi antar user. Informasi tidak berkurang.

**Q: Kenapa user jauh dapat QPSK, user dekat dapat 256-QAM?**  
A: Karena **Adaptive Modulation**. User jauh → sinyal lemah → modulasi rendah (QPSK). User dekat → sinyal kuat → modulasi tinggi (256-QAM).

**Q: Apa itu BALMON?**  
A: Balai Monitoring dari Kominfo, tugasnya mengawasi agar operator tidak melanggar alokasi frekuensi.

**Q: Apa beda OFDM dan SC-FDMA?**  
A:

- **OFDM**: Multicarrier, banyak user, dipakai downlink
- **SC-FDMA**: Single carrier, per user, dipakai uplink (karena daya pancar HP terbatas)

**Q: Apa itu orthogonal di OFDM?**  
A: Frekuensi dibuat saling tegak lurus (orthogonal) agar tidak ada interferensi meski jaraknya rapat.

**Q: Berapa spacing subcarrier di LTE?**  
A: **15 kHz** (untuk mencegah frequency offset akibat efek Doppler).

---

## TIPS PENTING

✅ **Multiplexing ≠ Modulasi**:

- Multiplexing = bagi kanal
- Modulasi = tumpangkan data ke carrier

✅ **OFDMA = Multicarrier**:

- Banyak subcarrier → banyak user
- Dipakai untuk downlink

✅ **SC-FDMA = Single Carrier**:

- Satu carrier per user
- Dipakai untuk uplink (karena daya HP terbatas)

✅ **Cyclic Prefix**:

- Guard band antar user
- Diambil dari akhir simbol, ditaruh di depan
- Informasi tidak berkurang

✅ **Adaptive Modulation**:

- Sistem otomatis menyesuaikan modulasi
- User jauh → QPSK (2 bit/simbol)
- User dekat → 256-QAM (8 bit/simbol)

✅ **Bandwidth LTE**:

- Umumnya 20 MHz
- Yang dipakai: 18 MHz
- 2 MHz sisanya: guard band antar operator

✅ **BALMON**:

- Mengawasi penggunaan frekuensi
- Cegah operator melanggar alokasi

---

## TUGAS

**Soal**:

**Diketahui**:

- Bandwidth: 1.4 MHz
- Modulasi: QPSK
- Cyclic Prefix: Normal CP

**Ditanyakan**:

1. Berapa jumlah subcarrier?
2. Berapa jumlah Resource Block (RB)?
3. Berapa kapasitas data (bit/s)?

**Petunjuk**: Coba kerjakan dulu, tidak apa jika salah. Akan dibahas minggu depan.

**Rumus akan dijelaskan minggu depan**:

- Jumlah subcarrier tergantung bandwidth
- 1 RB = 12 subcarrier
- Kapasitas = jumlah RE × bit per simbol × ...

---

## KESIMPULAN

### Multiplexing

- **FDMA**: Bagi frekuensi (teknologi 1G)
- **TDMA**: Bagi waktu (teknologi 2G)
- **CDMA**: Bagi kode (teknologi 3G)
- **OFDMA**: Bagi frekuensi orthogonal (teknologi 4G)

### OFDM di 4G

- Multicarrier (banyak subcarrier)
- Orthogonal (tidak ada interferensi)
- Subcarrier spacing: 15 kHz
- Cyclic Prefix untuk guard band

### Downlink vs Uplink

- **Downlink**: OFDM (multicarrier, kapasitas besar)
- **Uplink**: SC-FDMA (single carrier, daya pancar efisien)

### Adaptive Modulation

- QPSK (2 bit) → user jauh
- 16-QAM (4 bit) → user sedang
- 64-QAM (6 bit) → user dekat
- 256-QAM (8 bit) → user sangat dekat

### Bandwidth LTE

- Pilihan: 1.4, 3, 5, 10, 15, 20 MHz
- Umumnya: **20 MHz** (18 MHz data + 2 MHz guard band)

---

## 15. LTE LOGICAL CHANNEL

### Apa itu Channel?

**Channel** = Jalur atau kanal untuk komunikasi data antara transmitter dan receiver.

Di LTE, channel dibagi menjadi beberapa layer/tingkat:

```
[Application Layer]
       ↓
[Logical Channel]    ← Logika komunikasi
       ↓
[Transport Channel]  ← Pembawa data
       ↓
[Physical Channel]   ← Media fisik (RF)
```

### Jenis Channel di LTE

Ada **3 jenis channel**:

1. **Logical Channel**: Channel secara logika (untuk membangun jalur komunikasi)
2. **Transport Channel**: Channel untuk membawa data secara fisik
3. **Physical Channel**: Channel di physical layer (RF)

---

## 16. LOGICAL CHANNEL

### Kategori Logical Channel

**Logical Channel** dibagi menjadi **2 kategori**:

#### 1. Control Channel (CCH)

**Control Channel** = Channel untuk sinyal kontrol (mengatur transmisi data).

**Jenis Control Channel**:

| Channel  | Nama                      | Fungsi                                     | Arah     |
| -------- | ------------------------- | ------------------------------------------ | -------- |
| **BCCH** | Broadcast Control Channel | Broadcast informasi sistem                 | Downlink |
| **PCCH** | Paging Control Channel    | Paging (panggilan) ke user                 | Downlink |
| **CCCH** | Common Control Channel    | Kontrol umum (sebelum koneksi terbentuk)   | DL & UL  |
| **DCCH** | Dedicated Control Channel | Kontrol khusus (setelah koneksi terbentuk) | DL & UL  |

#### 2. Traffic Channel (TCH)

**Traffic Channel** = Channel untuk membawa data user.

**Jenis Traffic Channel**:

| Channel  | Nama                      | Fungsi                      | Arah     |
| -------- | ------------------------- | --------------------------- | -------- |
| **DTCH** | Dedicated Traffic Channel | Membawa data user (unicast) | DL & UL  |
| **MTCH** | Multicast Traffic Channel | Membawa data multicast      | Downlink |

### Fokus Utama: Traffic Channel (TCH)

**Yang paling penting**: **DTCH (Dedicated Traffic Channel)**

**Alasan**:

- Dipakai user untuk kirim/terima data
- Menentukan kapasitas jaringan
- Fokus perhitungan di RF planning

---

## 17. TRANSPORT CHANNEL

### Apa itu Transport Channel?

**Transport Channel** = Channel yang membawa data antara logical channel dan physical channel.

**Posisi**: Antara **MAC (Medium Access Control)** dan **Physical Layer**.

### Kategori Transport Channel

Transport channel juga dibagi menjadi **2 kategori**:

1. **Dedicated Channel**: Khusus untuk user tertentu
2. **Common Channel**: Bersama untuk banyak user

### Jenis Transport Channel

**Downlink Transport Channel**:

| Channel    | Nama                    | Fungsi                                  |
| ---------- | ----------------------- | --------------------------------------- |
| **BCH**    | Broadcast Channel       | Broadcast informasi sistem              |
| **DL-SCH** | Downlink Shared Channel | Membawa data downlink (paling penting!) |
| **PCH**    | Paging Channel          | Paging user                             |
| **MCH**    | Multicast Channel       | Membawa data multicast                  |

**Uplink Transport Channel**:

| Channel    | Nama                  | Fungsi                                |
| ---------- | --------------------- | ------------------------------------- |
| **UL-SCH** | Uplink Shared Channel | Membawa data uplink (paling penting!) |
| **RACH**   | Random Access Channel | Akses awal user ke network            |

### Fokus Utama: Shared Channel

**Yang paling penting**:

- **DL-SCH (Downlink Shared Channel)**: Untuk downlink
- **UL-SCH (Uplink Shared Channel)**: Untuk uplink

**Kenapa penting?**

- Dipakai untuk desain BCA (Base Station Capacity Analysis)
- Menentukan kemampuan/kapasitas user
- Untuk menghitung jumlah BCA yang bisa dipakai user

---

## 18. PHYSICAL CHANNEL

### Apa itu Physical Channel?

**Physical Channel** = Channel di lapisan fisik (physical layer) yang benar-benar membawa sinyal RF.

### Kategori Physical Channel

Physical channel dibagi menjadi **2 kategori**:

1. **Data Channel**: Membawa data
2. **Control Channel**: Membawa sinyal kontrol

### Jenis Physical Channel

**Downlink Physical Channel**:

| Channel    | Nama                                  | Fungsi                 |
| ---------- | ------------------------------------- | ---------------------- |
| **PDSCH**  | Physical Downlink Shared Channel      | Membawa data downlink  |
| **PBCH**   | Physical Broadcast Channel            | Broadcast info sistem  |
| **PDCCH**  | Physical Downlink Control Channel     | Kontrol downlink       |
| **PCFICH** | Physical Control Format Indicator Ch  | Format control channel |
| **PHICH**  | Physical Hybrid ARQ Indicator Channel | Indikator HARQ         |

**Uplink Physical Channel**:

| Channel   | Nama                            | Fungsi              |
| --------- | ------------------------------- | ------------------- |
| **PUSCH** | Physical Uplink Shared Channel  | Membawa data uplink |
| **PUCCH** | Physical Uplink Control Channel | Kontrol uplink      |
| **PRACH** | Physical Random Access Channel  | Akses awal          |

### Fokus Utama: Shared Channel

**Yang paling penting**:

- **PDSCH (Physical Downlink Shared Channel)**: Data downlink
- **PUSCH (Physical Uplink Shared Channel)**: Data uplink

---

## 19. CHANNEL MAPPING

### Alur Channel Mapping

**Dari Logical → Transport → Physical**:

```
[LOGICAL CHANNEL]
DTCH (Data Traffic Channel)
       ↓
[TRANSPORT CHANNEL]
DL-SCH (Downlink Shared Channel)
       ↓
[PHYSICAL CHANNEL]
PDSCH (Physical Downlink Shared Channel)
```

**Layer positioning**:

1. **Physical Channel** → Paling bawah (dekat RF)
2. **Transport Channel** → Tengah (MAC layer)
3. **Logical Channel** → Paling atas (dekat application)

### Kenapa Perlu 3 Layer Channel?

**Alasan**:

- **Logical**: Membangun jalur komunikasi (logika)
- **Transport**: Membawa data secara efektif
- **Physical**: Transmisi sinyal RF

**Analogi**:

- **Logical**: Alamat tujuan (mau kirim ke mana)
- **Transport**: Kendaraan pengangkut (mobil/truk)
- **Physical**: Jalan raya (jalur fisik)

---

## 20. PENGGUNAAN CHANNEL DALAM DESAIN

### Base Station Capacity Analysis (BCA)

**BCA** = Analisis kapasitas base station.

**Fungsi BCA**:

- Mengetahui kemampuan user
- Berapa jumlah BCA yang bisa dipakai user
- Mencegah interferensi antar user

**Prinsip**:

- Setiap user harus punya BCA berbeda
- Tidak boleh sama antar user
- Tujuan: mencegah interferensi

**Kapan dipakai?**

- Saat mendesain jaringan
- Saat RF planning
- Saat optimasi

---

## 21. PHYSICAL SIGNAL

### Apa itu Physical Signal?

**Physical Signal** = Sinyal kontrol di physical layer (bukan channel).

**Fungsi**:

- Mengatur proses transmisi
- Sinkronisasi
- Referensi

**Jenis Physical Signal**:

| Signal  | Nama                             | Fungsi                         |
| ------- | -------------------------------- | ------------------------------ |
| **PSS** | Primary Synchronization Signal   | Sinkronisasi awal              |
| **SSS** | Secondary Synchronization Signal | Sinkronisasi lanjutan          |
| **RS**  | Reference Signal                 | Referensi untuk estimasi kanal |

**Perbedaan Signal vs Channel**:

- **Channel**: Membawa data atau kontrol
- **Signal**: Hanya untuk sinkronisasi dan referensi

---

## GLOSSARY TAMBAHAN

| Istilah    | Kepanjangan                               | Artinya                           |
| ---------- | ----------------------------------------- | --------------------------------- |
| **CCH**    | Control Channel                           | Channel untuk kontrol             |
| **TCH**    | Traffic Channel                           | Channel untuk data                |
| **BCCH**   | Broadcast Control Channel                 | Broadcast informasi sistem        |
| **PCCH**   | Paging Control Channel                    | Paging ke user                    |
| **CCCH**   | Common Control Channel                    | Kontrol umum                      |
| **DCCH**   | Dedicated Control Channel                 | Kontrol khusus                    |
| **DTCH**   | Dedicated Traffic Channel                 | Data user (unicast)               |
| **MTCH**   | Multicast Traffic Channel                 | Data multicast                    |
| **BCH**    | Broadcast Channel                         | Transport channel untuk broadcast |
| **DL-SCH** | Downlink Shared Channel                   | Transport channel downlink        |
| **UL-SCH** | Uplink Shared Channel                     | Transport channel uplink          |
| **PCH**    | Paging Channel                            | Transport channel paging          |
| **MCH**    | Multicast Channel                         | Transport channel multicast       |
| **RACH**   | Random Access Channel                     | Akses awal user                   |
| **PDSCH**  | Physical Downlink Shared Channel          | Physical channel downlink         |
| **PUSCH**  | Physical Uplink Shared Channel            | Physical channel uplink           |
| **PBCH**   | Physical Broadcast Channel                | Physical channel broadcast        |
| **PDCCH**  | Physical Downlink Control Channel         | Physical control downlink         |
| **PUCCH**  | Physical Uplink Control Channel           | Physical control uplink           |
| **PRACH**  | Physical Random Access Channel            | Physical channel akses awal       |
| **PCFICH** | Physical Control Format Indicator Channel | Indikator format kontrol          |
| **PHICH**  | Physical Hybrid ARQ Indicator Channel     | Indikator HARQ                    |
| **PSS**    | Primary Synchronization Signal            | Sinyal sinkronisasi primer        |
| **SSS**    | Secondary Synchronization Signal          | Sinyal sinkronisasi sekunder      |
| **RS**     | Reference Signal                          | Sinyal referensi                  |
| **MAC**    | Medium Access Control                     | Lapisan akses medium              |
| **BCA**    | Base Station Capacity Analysis            | Analisis kapasitas base station   |

---

## FAQ TAMBAHAN

**Q: Apa beda Logical Channel, Transport Channel, dan Physical Channel?**  
A:

- **Logical Channel**: Channel secara logika (untuk membangun jalur komunikasi)
- **Transport Channel**: Channel untuk membawa data antara MAC dan Physical
- **Physical Channel**: Channel di RF (fisik)

**Q: Apa channel yang paling penting di LTE?**  
A:

- **Logical**: DTCH (Dedicated Traffic Channel)
- **Transport**: DL-SCH dan UL-SCH (Shared Channel)
- **Physical**: PDSCH dan PUSCH (Physical Shared Channel)

**Q: Apa beda Control Channel dan Traffic Channel?**  
A:

- **Control Channel**: Membawa sinyal kontrol (mengatur komunikasi)
- **Traffic Channel**: Membawa data user

**Q: Kenapa perlu 3 layer channel?**  
A: Untuk memisahkan fungsi:

- Logical → logika komunikasi
- Transport → efisiensi pembawa data
- Physical → transmisi RF

**Q: Apa itu BCA?**  
A: Base Station Capacity Analysis → analisis kapasitas base station untuk mengetahui kemampuan user dan mencegah interferensi.

**Q: Apa beda Physical Channel dan Physical Signal?**  
A:

- **Physical Channel**: Membawa data atau kontrol
- **Physical Signal**: Hanya untuk sinkronisasi dan referensi (tidak membawa data)

---

## TIPS PENTING TAMBAHAN

✅ **Channel Mapping**:

- Logical → Transport → Physical
- Fokus: DTCH → DL-SCH/UL-SCH → PDSCH/PUSCH

✅ **Fokus Perhitungan**:

- Traffic Channel (DTCH) paling penting
- Dipakai untuk hitung kapasitas jaringan

✅ **BCA (Base Station Capacity Analysis)**:

- Untuk desain jaringan
- Mencegah interferensi antar user
- Setiap user harus punya BCA berbeda

✅ **Control vs Traffic**:

- Control → mengatur
- Traffic → membawa data user

✅ **Physical Signal**:

- PSS, SSS → sinkronisasi
- RS → referensi estimasi kanal

---

## KESIMPULAN LENGKAP

### Channel di LTE

**3 Layer Channel**:

1. **Logical Channel** → Logika komunikasi (DTCH, DCCH, BCCH, dll)
2. **Transport Channel** → Pembawa data (DL-SCH, UL-SCH, BCH, dll)
3. **Physical Channel** → Transmisi RF (PDSCH, PUSCH, PBCH, dll)

### Channel Penting

**Fokus utama**:

- **Logical**: DTCH (data user)
- **Transport**: DL-SCH & UL-SCH (shared channel)
- **Physical**: PDSCH & PUSCH (physical shared channel)

### Multiplexing

- **FDMA**: Bagi frekuensi (teknologi 1G)
- **TDMA**: Bagi waktu (teknologi 2G)
- **CDMA**: Bagi kode (teknologi 3G)
- **OFDMA**: Bagi frekuensi orthogonal (teknologi 4G)

### OFDM di 4G

- Multicarrier (banyak subcarrier)
- Orthogonal (tidak ada interferensi)
- Subcarrier spacing: 15 kHz
- Cyclic Prefix untuk guard band

### Downlink vs Uplink

- **Downlink**: OFDM (multicarrier, kapasitas besar)
- **Uplink**: SC-FDMA (single carrier, daya pancar efisien)

### Adaptive Modulation

- QPSK (2 bit) → user jauh
- 16-QAM (4 bit) → user sedang
- 64-QAM (6 bit) → user dekat
- 256-QAM (8 bit) → user sangat dekat

### Bandwidth LTE

- Pilihan: 1.4, 3, 5, 10, 15, 20 MHz
- Umumnya: **20 MHz** (18 MHz data + 2 MHz guard band)

---

**Catatan**: Materi berikutnya: **perhitungan Resource Element (RE) dan kapasitas LTE**. Hitung-hitungan untuk RF planning!

---

**Selamat belajar! Coba simulasi sendiri dengan MATLAB atau NS-3!**
