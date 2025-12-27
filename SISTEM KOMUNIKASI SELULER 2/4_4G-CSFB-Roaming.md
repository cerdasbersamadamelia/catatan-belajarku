# 4. 4G CSFB (Circuit Switched Fallback untuk voice) & ROAMING

## OVERVIEW

Materi kali ini membahas tentang:

- Interworking (bagaimana 4G bekerja dengan jaringan lain)
- Roaming (perpindahan jaringan antar negara)
- CSFB (Circuit Switched Fallback untuk voice)

---

## 1. INTERWORKING WITH OTHER NETWORKS

### Apa itu Interworking?

**Interworking** = Bagaimana jaringan 4G bekerja/terkoneksi dengan jaringan lain

Jaringan seluler ada 2 referensi:

- **3GPP** → Contoh: GSM, UMTS, LTE (dipakai di Indonesia)
- **3GPP2** → Contoh: CDMA, WiMAX (dipakai di Eropa & Amerika)

### Jenis Interworking

Ada 2 kategori koneksi 4G dengan jaringan Non-3GPP:

#### 1. Trusted Non-3GPP Access

- Jaringan yang **dipercaya** dan punya kapabilitas lengkap
- Operator punya autentikasi user yang lengkap
- Contoh: **WiMAX**
- Punya aturan security yang kuat
- Interface: **S2a**

#### 2. Untrusted Non-3GPP Access

- Jaringan yang **tidak dipercaya** (security lemah)
- Cakupan jaringan kecil
- Contoh: **WiFi, Hotspot**
- ⚠️ **Hati-hati**: Data bisa diambil orang lain kalau akses WiFi public di luar negeri
- Interface: **S2b** (melalui ePDG)

### Arsitektur Interworking Non-3GPP

```
4G LTE Network
├── eNodeB
├── MME (signaling - garis putus-putus)
├── SGW (data - garis lurus)
├── PGW (gateway ke internet)
└── HSS

Koneksi ke Non-3GPP:
→ Trusted: HSS/AAA → langsung ke S2a
→ Untrusted: WiFi → ePDG → S2b → PGW
```

**Catatan**:

- Garis **putus-putus** = sinyal/control
- Garis **lurus** = data/user plane

### Interworking dengan Legacy 3GPP (2G/3G)

**Release**: 3GPP Release 8

Istilah jaringan:

- 4G = **E-UTRAN** (Evolved UTRAN)
- 3G = **UTRAN**
- 2G = **GERAN**

#### Interface Legacy Network

| Interface | Fungsi                  | Koneksi    |
| --------- | ----------------------- | ---------- |
| **S6a**   | Autentikasi & data user | HSS ↔ MME  |
| **S3**    | Mobility management     | MME ↔ SGSN |
| **S4**    | Data transfer           | SGW ↔ SGSN |

**Catatan Penting**:

- SGW = SGSN (perangkat yang sama, beda fungsi saja)
- SGW dipakai di 4G
- SGSN dipakai di 2G/3G
- PGW = GGSN (juga perangkat yang sama)

---

## 2. ROAMING

### Apa itu Roaming?

**Roaming** = User keluar/meninggalkan home base-nya (keluar negeri)

### Jenis Roaming

#### 1. Roaming Nasional

- Perpindahan dalam 1 negara
- Masih dalam 1 PLMN (operator yang sama)
- **GRATIS** (tidak ada charging)
- Contoh: Jakarta → Malang (masih Telkomsel)

#### 2. Roaming Internasional

- Keluar negeri / keluar dari 1 PLMN
- Ada **charging** (berbayar)
- Contoh: Indonesia (Indosat) → Singapura (Singtel)

### Istilah Penting

| Istilah   | Kepanjangan                | Artinya                  |
| --------- | -------------------------- | ------------------------ |
| **PLMN**  | Public Land Mobile Network | 1 operator = 1 PLMN      |
| **HPLMN** | Home PLMN                  | Operator asal            |
| **VPLMN** | Visited PLMN               | Operator yang dikunjungi |

**Syarat Roaming**: HPLMN dan VPLMN harus punya **kerjasama**!

### Proses Roaming

#### 1. Home Routed

- **Billing** dan **charging** ditangani oleh **HPLMN**
- Pulsa dibeli dulu sebelum berangkat
- Bisa untuk **prabayar** dan **pascabayar**
- Contoh: Beli paket roaming 7 hari

**Cara kerja**:

1. Nyalakan HP di negara tujuan
2. MME VPLMN cek ke HSS HPLMN → apakah benar pelanggan?
3. Jika OK → dapat sinyal operator di negara tujuan
4. Semua charging diambil dari HPLMN (Indonesia)

#### 2. Local Breakout

- **Billing** dan **charging** ditangani oleh **VPLMN**
- Seolah-olah jadi pelanggan sementara VPLMN
- Hanya untuk **pascabayar**
- **Lebih mahal** dari home routed
- Proses charging melalui vPCRF (PCRF di negara tujuan)

**Cara kerja**:

1. HSS hanya untuk **memastikan** user adalah pelanggan HPLMN
2. Setelah itu semua komunikasi ditangani VPLMN
3. Tagihan nanti dibayar di Indonesia setelah pulang

### Perbandingan Home Routed vs Local Breakout

| Aspek    | Home Routed           | Local Breakout        |
| -------- | --------------------- | --------------------- |
| Billing  | HPLMN (Indonesia)     | VPLMN (negara tujuan) |
| Biaya    | Lebih murah           | Lebih mahal           |
| Kartu    | Prabayar & Pascabayar | Hanya pascabayar      |
| Charging | hPCRF                 | vPCRF                 |

### Protokol Roaming: Diameter (Dia)

**Dia** = Diameter Agents

- Mengatur semua protokol koneksi roaming
- Harus disepakati oleh HPLMN dan VPLMN
- Khusus untuk 4G

### Transport Roaming

Jaringan transport bisa melalui:

- **Leased Line**
- **CVN** (Circuit Voice Network)
- **IP-based** (paling umum)
- **IPX** (IP Exchange)

---

## 3. PRIVATE LTE

### Apa itu Private LTE?

Private LTE = Jaringan LTE khusus untuk 1 perusahaan besar

**Contoh**: Freeport di Papua

### Keuntungan Private LTE

1. ✅ Remote monitoring & perbaikan lebih mudah
2. ✅ Tidak perlu koneksi ke kantor pusat dulu
3. ✅ Kapasitas lebih besar
4. ✅ Lebih aman (data tidak bocor)
5. ✅ Bisa koneksi langsung ke luar negeri

### Siapa yang Membangun?

Operator (Telkomsel, Indosat, Smartfren) membangunkan untuk perusahaan

**Biaya**: Mahal, tapi kapasitas besar & aman

### Perangkat Private LTE

Punya perangkat sendiri:

- MME
- SGW
- PGW
- HSS
- PCRF

### Cocok untuk Perusahaan:

- Lokasi jauh dari kota
- Butuh kapasitas komunikasi tinggi
- Banyak data rahasia
- Perusahaan internasional (contoh: tambang, perkebunan sawit)

---

## 4. CSFB (CIRCUIT SWITCHED FALLBACK)

### Apa itu CSFB?

**CSFB** = Metode untuk voice di 4G dengan cara **kembali ke 3G/2G**

**Kenapa perlu CSFB?**

- 4G = untuk **data** (bukan voice)
- Kalau mau nelpon → harus turun ke 3G/2G

### Standarisasi

**3GPP TS 23.272** → Dokumen standar CSFB

### Cara Kerja CSFB

1. User di jaringan 4G (LTE)
2. User melakukan panggilan voice
3. **Fallback** → turun ke 3G atau 2G
4. Voice call dilakukan di 3G/2G
5. Setelah selesai → kembali ke 4G

### Arsitektur CSFB

```
4G LTE:
├── eNodeB
├── MME
├── SGW
├── PGW
└── HSS

3G/2G:
├── NodeB / BTS
├── RNC / BSC
├── SGSN
└── MSC (untuk voice)

Proses:
4G (MME) → 3G (SGSN) → MSC → Voice Call
```

### Yang Bekerja Pertama Kali: MME

**MME** memastikan:

1. Cek ke MSC → MSC mana yang akan handle voice?
2. Bangun koneksi **control/signaling** dulu (garis putus-putus)
3. Kalau signaling OK → baru **data** masuk (garis lurus)
4. Voice baru bisa jalan

### SGW = SGSN?

**YA! SGW dan SGSN itu perangkat yang sama**

- Di 4G → namanya **SGW**
- Di 3G → namanya **SGSN**
- Di 4G → data masuk ke **PGW/GGSN**
- Voice tidak lewat PGW, langsung ke MSC

### Roaming Voice Internasional

Voice roaming internasional:

- **BUKAN pakai Diameter**
- Pakai **STP** (Signaling Transfer Point)
- **SS7** / **Sigtran** (untuk IP-based)
- Gateway: **International Gateway** (biasanya di Singapore)

---

## 5. PERANGKAT RAN

### Apa itu RAN?

**RAN** = Radio Access Network (bagian radio ke user)

### Komponen RAN (Radio Remote Unit/Head)

#### RRU (Radio Remote Unit) / RRH (Radio Remote Head)

Di dalam RRU ada:

- **Slot untuk 2G** (GSM - 900 MHz)
- **Slot untuk 3G** (UMTS - 2100 MHz)
- **Slot untuk 4G** (LTE - 1800 MHz)

**Tinggal diaktifkan yang mana!**

### Jenis BTS Berdasarkan Konfigurasi

| Jenis               | Keterangan                                    |
| ------------------- | --------------------------------------------- |
| **BTS Hotel**       | BTS besar, semua fungsi dalam 1 tempat        |
| **Centralized RAN** | BBU di pusat, RRU di lokasi (terhubung fiber) |
| **Distributed RAN** | BBU & RRU terpisah tapi dekat                 |
| **Cloud RAN**       | BBU di cloud, RRU di banyak lokasi            |

### Satu Site Bisa Multi-Teknologi

Contoh: 1 tower bisa punya:

- **2G** (untuk voice di pelosok)
- **3G** (HSPA/HSDPA)
- **4G** (LTE untuk data)

**Operator bisa deteksi HP kamu!**

- Tahu HP support VoLTE atau tidak
- Tahu user pakai Facebook, WhatsApp, Instagram
- Berapa banyak user pakai data
- Berapa banyak user masih pakai voice

### Refarming Frekuensi

**Refarming** = Pindahkan frekuensi lama untuk teknologi baru

Contoh:

- **2100 MHz** (untuk 3G) → pindah ke **L2100** (LTE 2100 MHz)
- **900 MHz** (untuk 2G) → pindah ke **L900** (LTE 900 MHz)

**Kenapa?** Kebutuhan data sekarang lebih tinggi!

### NMS (Network Management System)

Operator punya **NMS** untuk monitoring:

- Berapa user akses Facebook?
- Berapa user akses WhatsApp?
- Berapa user akses Instagram?
- Berapa user akses Twitter?

**Data ini dijual ke operator** → supaya site dibangun sesuai kebutuhan!

### Design Site Harus Sesuai Kebutuhan

❌ **Salah**: Bangun site mahal tapi tidak ada user

✅ **Benar**: Cek dulu user butuh **data** atau **voice**?

- Kalau user masih banyak pakai voice → tetap sediakan 2G/3G
- Kalau user sudah banyak pakai data → upgrade ke 4G

---

## 6. PROSEDUR PANGGILAN CSFB

### Jenis Panggilan

1. **Originating** = Panggilan keluar (melakukan panggilan)
2. **Terminating** = Panggilan masuk (menerima panggilan)

### Metode CSFB

Ada 2 skenario:

1. **Blind Scenario**
2. **CS Scenario**

---

## GLOSSARY

| Istilah   | Kepanjangan                      | Artinya                            |
| --------- | -------------------------------- | ---------------------------------- |
| **CSFB**  | Circuit Switched Fallback        | Voice di 4G dengan turun ke 2G/3G  |
| **PLMN**  | Public Land Mobile Network       | Jaringan operator seluler          |
| **HPLMN** | Home PLMN                        | Operator asal/home                 |
| **VPLMN** | Visited PLMN                     | Operator yang dikunjungi (roaming) |
| **MME**   | Mobility Management Entity       | Control signaling di 4G            |
| **SGW**   | Serving Gateway                  | Gateway serving untuk data di 4G   |
| **PGW**   | PDN Gateway                      | Gateway ke internet/PDN            |
| **HSS**   | Home Subscriber Server           | Database pelanggan                 |
| **PCRF**  | Policy Charging & Rules Function | Mengatur policy & charging         |
| **SGSN**  | Serving GPRS Support Node        | Gateway serving di 2G/3G           |
| **GGSN**  | Gateway GPRS Support Node        | Gateway ke internet di 2G/3G       |
| **MSC**   | Mobile Switching Center          | Switch untuk voice (2G/3G)         |
| **Dia**   | Diameter Agents                  | Protokol untuk roaming di 4G       |
| **RRU**   | Radio Remote Unit                | Unit radio di tower                |
| **RRH**   | Radio Remote Head                | Sama dengan RRU                    |
| **NMS**   | Network Management System        | Sistem monitoring jaringan         |
| **ePDG**  | Evolved Packet Data Gateway      | Gateway untuk untrusted network    |
| **IPX**   | IP Exchange                      | Jaringan IP untuk roaming          |

---

## TIPS PENTING

✅ **Roaming Internasional**:

- Aktifkan roaming sebelum berangkat!
- Beli paket roaming (lebih murah)
- Home Routed lebih murah dari Local Breakout

⚠️ **Security WiFi Public**:

- Hati-hati pakai WiFi/hotspot di luar negeri
- Security lemah → data bisa dicuri

📊 **Fakta Operator**:

- Operator tahu HP kamu support VoLTE atau tidak
- Operator tahu kamu pakai app apa aja
- Site dibangun berdasarkan data kebutuhan user

🔄 **Refarming**:

- Frekuensi lama dipindah ke teknologi baru
- Contoh: 2G 900 MHz → LTE 900 MHz (L900)

---

## FAQ

**Q: Kenapa nelpon di 4G harus turun ke 3G?**  
A: Karena 4G = untuk data. Kalau mau voice pakai CSFB (turun ke 3G/2G) atau VoLTE (perlu tambahan perangkat IMS)

**Q: Bedanya SGW sama SGSN?**  
A: Sama! Cuma beda nama:

- Di 4G namanya SGW
- Di 3G namanya SGSN

**Q: Bisa roaming tanpa ganti kartu?**  
A: Bisa! Asal:

1. Operator kamu punya kerjasama dengan operator negara tujuan
2. Sudah aktifkan roaming sebelum berangkat

**Q: Private LTE itu apa?**  
A: Jaringan LTE khusus untuk 1 perusahaan. Kapasitas besar, aman, mahal. Cocok untuk perusahaan besar di lokasi terpencil.

**Q: Satu tower bisa ada 2G, 3G, 4G sekaligus?**  
A: Bisa! Di RRU ada slot untuk semua teknologi, tinggal diaktifkan sesuai kebutuhan user di area tersebut.

---

**Catatan**: Materi ini berlanjut ke sesi berikutnya tentang VoLTE dan prosedur CSFB yang lebih detail.
