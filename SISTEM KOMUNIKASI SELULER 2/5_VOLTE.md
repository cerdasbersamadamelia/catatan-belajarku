# 5. VoLTE (Voice over LTE)

## OVERVIEW

Materi kali ini melanjutkan dari CSFB dan membahas:

- Detail prosedur CSFB (3 fase)
- Skenario CSFB (Blind vs Measurement)
- Release 8 vs Release 9 (Flash CSFB)
- VoLTE sebagai solusi voice di 4G
- IMS (IP Multimedia Subsystem)

---

## 1. PROSEDUR CSFB (3 FASE)

### Apa itu CSFB?

**CSFB** = Circuit Switched Fallback

- Proses voice di 4G dengan cara **fallback ke 2G/3G**
- LTE default untuk data, bukan voice
- Voice harus kembali ke CS domain (2G/3G)

### 3 Fase CSFB

Proses CSFB dibagi menjadi **3 tahap**:

#### Fase 1: Measurement Phase

**Fungsi**: Membuat list kandidat sel 2G/3G

Ada 2 skenario:

1. **Blind Scenario**
2. **Measurement Scenario**

_(Detail dijelaskan di bagian berikutnya)_

#### Fase 2: Decision Phase

**Fungsi**: Menentukan sel mana yang akan dipilih

- Berdasarkan list yang dibuat di Measurement Phase
- Memutuskan sel tujuan handover
- Contoh: Pilih sel 6 karena punya daya paling tinggi

#### Fase 3: Execution Phase

**Fungsi**: Eksekusi handover

- Handover dari 4G ke 3G/2G
- Voice call mulai berjalan
- PS (data) tetap tersambung jika ada
- Di 3G: masuk ke SGSN → MSC

---

## 2. SKENARIO CSFB (RELEASE 8)

### Standarisasi

**3GPP Release 8** → Dokumen standar: **3GPP TS 23.272**

Ada 2 skenario untuk Measurement Phase:

### Skenario 1: Blind Scenario

#### Karakteristik

- **Tidak ada pengukuran daya**
- List sel dibuat **tanpa melihat kualitas sinyal**
- List sudah diset dari awal oleh operator

#### Cara Kerja

Contoh area dengan 1 LTE cell (tengah) + 6 sel 3G/2G (sekitar):

```
         [Sel 3G]
[Sel 2G]  [LTE]  [Sel 3G]
         [Sel 3G]
```

List dibuat: Sel 5 → Sel 2 → Sel 3 → Sel 4 → Sel 6 → Sel 1 → Sel 7

**Urutan list = urutan priority (bukan berdasarkan daya!)**

#### Kapan Dipakai?

- Area dengan **traffic sangat tinggi**
- Tidak ada waktu untuk pengukuran real-time
- Emergency backup scenario

#### Kelebihan vs Kekurangan

✅ **Kelebihan**:

- Lebih cepat (no measurement)
- Cocok untuk traffic tinggi

❌ **Kekurangan**:

- Bisa salah pilih sel (daya lemah)
- Kurang optimal

### Skenario 2: Measurement Scenario

#### Karakteristik

- **Ada pengukuran daya real-time**
- List sel dibuat **berdasarkan kualitas sinyal**
- Menggunakan inter-RAT (Radio Access Technology)

#### Cara Kerja

1. Sistem ukur daya semua sel 3G/2G di sekitar
2. Urutkan berdasarkan **daya terbesar**
3. List: Sel 6 (terkuat) → Sel 1 → Sel 2 → Sel 7 → Sel 3 → Sel 4

**Urutan list = urutan daya (dari terkuat ke terlemah)**

#### Kapan Dipakai?

- **Kondisi normal** (paling umum)
- Area dengan traffic moderat
- Butuh QoS yang baik

#### Kelebihan vs Kekurangan

✅ **Kelebihan**:

- Optimal (pilih sel terbaik)
- QoS lebih baik
- Call success rate tinggi

❌ **Kekurangan**:

- Butuh waktu measurement
- Lebih lambat dari blind

### Perbandingan Blind vs Measurement

| Aspek           | Blind Scenario       | Measurement Scenario    |
| --------------- | -------------------- | ----------------------- |
| Pengukuran daya | ❌ Tidak ada         | ✅ Ada (real-time)      |
| List sel        | Preset oleh operator | Berdasarkan measurement |
| Kecepatan       | Lebih cepat          | Lebih lambat            |
| QoS             | Kurang optimal       | Lebih optimal           |
| Kondisi         | Traffic tinggi       | Normal                  |
| Implementasi    | Backup/emergency     | Default                 |

### Inter-RAT Configuration

**Inter-RAT** = Inter Radio Access Technology

- Konfigurasi dilakukan di **radio access layer**
- eNodeB sudah tahu sel-sel 3G/2G di sekitarnya
- Data disimpan di pusat (NOC/MSC Office)

**Blind Scenario**:

- Operator setting priority berdasarkan:
  - Estimasi coverage
  - Historical data
  - Capacity planning
- Meski "blind", tetap ada logika di baliknya

**Measurement Scenario**:

- Sistem otomatis ukur daya
- Software sudah ada untuk measurement
- Pilih sel dengan daya terbesar

---

## 3. FLASH CSFB (RELEASE 9)

### Apa itu Flash CSFB?

**Flash CSFB** = CSFB yang dipercepat (3GPP Release 9)

### Perbedaan R8 vs R9

| Aspek     | Release 8    | Release 9 (Flash)  |
| --------- | ------------ | ------------------ |
| Nama      | CSFB         | Flash CSFB         |
| Proses    | Melalui core | Bypass core        |
| Kecepatan | Normal       | Lebih cepat        |
| Request   | Lewat MME    | Langsung ke eNodeB |

### Cara Kerja Flash CSFB

**Release 8** (Normal CSFB):

1. UE request ke eNodeB
2. eNodeB → MME
3. MME proses & beri info kandidat sel
4. MME → eNodeB → UE
5. UE handover

**Release 9** (Flash CSFB):

1. UE request ke eNodeB
2. eNodeB **langsung kasih info** kandidat sel (tanpa lewat MME)
3. UE langsung handover

**Keuntungan**:

- Proses lebih cepat
- Latency berkurang
- User experience lebih baik

---

## 4. REVIEW: ROAMING & INTERWORKING

### Roaming (2 Jenis)

#### 1. Home Routed

- Billing & charging: **HPLMN** (operator asal)
- Pulsa dibeli dulu sebelum berangkat
- Bisa prabayar & pascabayar
- **Lebih murah**

#### 2. Local Breakout

- Billing & charging: **VPLMN** (operator tujuan)
- User jadi pelanggan sementara VPLMN
- Hanya pascabayar
- **Lebih mahal**
- Proses lewat vPCRF

### Interworking (2 Kategori)

#### 1. Trusted Non-3GPP Access

- Jaringan dipercaya
- Contoh: **WiMAX**
- Security kuat
- Interface: **S2a**

#### 2. Untrusted Non-3GPP Access

- Jaringan tidak dipercaya
- Contoh: **WiFi, Hotspot**
- Security lemah
- ⚠️ Data bisa dicuri
- Interface: **S2b** (lewat ePDG)

### Interworking dengan Legacy 3GPP

**Legacy 3GPP** = 2G/3G

- LTE bisa koneksi dengan WCDMA/GSM
- Pakai interface standar (S3, S4, S6a)
- Untuk CSFB dan roaming

---

## 5. VoLTE (VOICE OVER LTE)

### Masalah: 4G = Data Only

**4G LTE default TIDAK support voice** karena:

- All-IP network
- Packet Switching only
- Tidak ada Circuit Switching

### 2 Solusi Voice di 4G

| Solusi    | Keterangan                        |
| --------- | --------------------------------- |
| **CSFB**  | Fallback ke 2G/3G (sudah dibahas) |
| **VoLTE** | Voice over LTE (dibahas sekarang) |

---

## 6. KONSEP VoLTE

### Apa itu VoLTE?

**VoLTE** = Voice over LTE

- Teknologi untuk voice & video di jaringan LTE
- Voice dijadikan **data** (VoIP)
- Tetap di 4G, tidak turun ke 2G/3G
- Butuh **IMS** (IP Multimedia Subsystem)

### Kata Kunci: IMS

**IMS** = IP Multimedia Subsystem

- **Wajib ada** untuk VoLTE!
- Server tambahan untuk handle voice
- Mengatur call control
- Interface antara voice dan data network

**Kenapa perlu IMS?**

- LTE default untuk data
- Voice butuh perlakuan khusus
- IMS = jembatan agar voice bisa jalan di LTE

---

## 7. KEUNTUNGAN VoLTE

### 1. Native Voice Quality

✅ **Lebih jernih** dari OTT (WhatsApp Call, dll)

**Kenapa?**

- Pakai jaringan dedicated (bukan numpang)
- Ada 3 fase komunikasi:
  1. **Inisialisasi** → bangun sinyal dulu
  2. **Transfer data** → kirim voice
  3. **Release** → putus koneksi

**OTT (Over-The-Top) service**:

- Numpang di data network
- Tidak pakai jaringan dedicated
- Quality tidak terjamin
- Contoh: WhatsApp Call, Skype, Line Call, Zoom, Google Meet

### 2. Tidak Perlu OTT Service

- Voice sudah native di jaringan
- Tidak perlu install aplikasi pihak ketiga
- Operator bisa kontrol quality
- Revenue tidak hilang

**Catatan**:

- Di Indonesia: OTT tidak dilarang (merugikan operator)
- Di China: OTT dibatasi (lindungi operator)
- Pemerintah yang tentukan regulasi

### 3. Improve Coverage & Connectivity

**Bandwidth lebih besar**:

- 4G: **20 MHz**
- 3G: **15 MHz** (maksimal)
- 2G: **7.5 MHz**

**Latency lebih rendah**:

- 4G: **< 5 ms**
- 3G: **> 10 ms**

**Hasil**:

- Voice quality lebih baik
- Lebih jernih
- Tidak putus-putus

### 4. Hemat Baterai

✅ **Battery saving**

**Kenapa?**

- Tidak perlu bolak-balik 4G ↔ 3G (seperti CSFB)
- Tetap di 4G (lebih efisien)
- Tidak boros seperti OTT apps

**OTT apps**:

- WhatsApp Call = pemakan baterai!
- Butuh charging sering

### 5. Video Calling

✅ Support video call

- HD voice & video
- Mirip Zoom tapi native
- Tidak perlu aplikasi tambahan

---

## 8. PRINSIP VoLTE

### Legacy Network: CS + PS

**3G (UMTS)**:

- Voice → **CS domain** (MSC)
- Data → **PS domain** (SGSN/GGSN)

### VoLTE: PS Only

**4G dengan VoLTE**:

- Voice → **PS domain** (lewat IMS)
- Data → **PS domain**
- **Tidak pakai CS domain sama sekali!**

**Legacy network** tetap ada untuk:

- User yang belum support VoLTE
- Fallback dari VoLTE (SRVCC)
- Backward compatibility

---

## 9. ARSITEKTUR IMS

### Komponen IMS

**IMS menghasilkan server AAA**:

1. **Authentication** → otentikasi user
2. **Authorization** → otorisasi layanan
3. **Accounting** → billing & charging

### Kenapa Perlu AAA?

**Voice = berbasis waktu** (bukan data!)

- Charging berdasarkan **durasi**
- Bukan berdasarkan **paket data**
- Perlu sistem billing khusus

**IMS + PCRF**:

- IMS handle call control
- PCRF handle policy & charging
- Komunikasi untuk tentukan charging

---

## 10. STRATEGI VOICE DI 4G

### Strategi 1: Voice-over CS

- Voice tetap pakai **CS domain** (2G/3G)
- Data pakai **LTE**
- = **CSFB**

**Cocok untuk**:

- Operator punya existing 2G/3G
- Belum siap invest IMS
- Transisi ke full VoLTE

### Strategi 2: Voice-over LTE

- Voice pakai **PS domain** (lewat IMS)
- Data pakai **PS domain**
- = **VoLTE**

**Cocok untuk**:

- Operator baru (tidak punya 2G/3G)
- Mau full IP network
- Investasi IMS sudah ready

### Perbandingan

| Aspek         | CSFB              | VoLTE          |
| ------------- | ----------------- | -------------- |
| Voice         | CS domain (2G/3G) | PS domain (4G) |
| Data          | PS domain (4G)    | PS domain (4G) |
| Perlu IMS     | ❌ Tidak          | ✅ Ya          |
| Perlu 2G/3G   | ✅ Ya             | ❌ Tidak       |
| Battery       | Lebih boros       | Hemat          |
| Voice quality | Bagus             | Lebih bagus    |
| Latency       | > 10 ms           | < 5 ms         |

---

## 11. CALL CONTROL DI VoLTE

### Semua Lewat IMS

**Skenario panggilan**:

1. **LTE ↔ LTE** (sesama VoLTE) → lewat IMS
2. **LTE ↔ 3G** (VoLTE ke non-VoLTE) → lewat IMS
3. **LTE ↔ 2G** → lewat IMS

**IMS = Call Control Center**

- Semua panggilan voice lewat IMS
- IMS tentukan routing
- IMS koordinasi dengan MSC jika perlu

---

## 12. TROUBLESHOOTING

### User Tidak Dapat VoLTE

**Penyebab**:

1. ❌ **Handset tidak support VoLTE**

   - Handset lama (contoh: iPhone 6)
   - Tidak compatible dengan VoLTE

2. ❌ **Area belum ada VoLTE**

   - Operator belum pasang IMS di area tersebut
   - Cek coverage VoLTE operator

3. ❌ **VoLTE tidak diaktifkan di setting HP**
   - Masuk ke Setting
   - Mobile Network → Enable VoLTE

### Cek Handset Support VoLTE

**Cara cek**:

- Lihat di status bar saat call
- Ada icon **VoLTE** atau **HD** = support
- Kalau turun ke **3G/H+** saat call = tidak support

**Operator bisa deteksi**:

- Data handset tersimpan di network
- Tahu HP support VoLTE atau tidak

---

## 13. IMPLEMENTASI DI INDONESIA

### Status VoLTE Indonesia

**Telkomsel**:

- Sudah ada VoLTE di area tertentu
- Bertahap nationwide

**Operator lain**:

- Masih banyak pakai CSFB
- VoLTE mulai diimplementasi

### Tantangan

1. **Coverage**: Belum semua area ada VoLTE
2. **Device**: Banyak HP lama belum support
3. **Investment**: IMS mahal
4. **OTT Competition**: WhatsApp Call gratis

---

## GLOSSARY

| Istilah        | Kepanjangan                               | Artinya                                            |
| -------------- | ----------------------------------------- | -------------------------------------------------- |
| **CSFB**       | Circuit Switched Fallback                 | Voice di 4G dengan turun ke 2G/3G                  |
| **VoLTE**      | Voice over LTE                            | Voice dijadikan data di 4G (lewat IMS)             |
| **IMS**        | IP Multimedia Subsystem                   | Server untuk handle voice di LTE                   |
| **AAA**        | Authentication, Authorization, Accounting | Server untuk security & billing                    |
| **OTT**        | Over-The-Top                              | Layanan dari aplikasi pihak ketiga (WhatsApp, dll) |
| **Inter-RAT**  | Inter Radio Access Technology             | Antar teknologi radio (4G→3G→2G)                   |
| **Flash CSFB** | -                                         | CSFB yang dipercepat (Release 9)                   |
| **SRVCC**      | Single Radio Voice Call Continuity        | Handover dari VoLTE ke 2G/3G saat call             |
| **HD Voice**   | High Definition Voice                     | Voice quality tinggi (VoLTE)                       |

---

## TIPS PENTING

✅ **VoLTE vs WhatsApp Call**:

- VoLTE: native, jernih, reliable
- WhatsApp: numpang, kadang putus-putus, unreliable

📱 **Cek HP Support VoLTE**:

- Telpon seseorang
- Lihat status bar
- Ada icon VoLTE/HD = support
- Turun ke 3G/H+ = tidak support

🔋 **Hemat Baterai**:

- VoLTE hemat baterai (tetap di 4G)
- CSFB boros (bolak-balik 4G↔3G)
- OTT apps sangat boros!

⚡ **Flash CSFB (R9)**:

- Lebih cepat dari R8
- Bypass MME
- User experience lebih baik

---

## FAQ

**Q: Bedanya CSFB sama VoLTE?**  
A:

- CSFB: Voice turun ke 3G/2G (butuh existing network)
- VoLTE: Voice tetap di 4G (butuh IMS, tidak butuh 2G/3G)

**Q: Kenapa VoLTE perlu IMS?**  
A: Karena LTE default untuk data. Voice butuh server khusus (IMS) untuk bisa jalan di LTE.

**Q: Bedanya Blind vs Measurement Scenario?**  
A:

- Blind: List sel preset (tanpa ukur daya), cepat, untuk traffic tinggi
- Measurement: List sel based on daya real-time, optimal, kondisi normal

**Q: Flash CSFB itu apa?**  
A: CSFB yang dipercepat (Release 9). Bypass MME, langsung dari eNodeB ke UE.

**Q: VoLTE lebih bagus dari WhatsApp Call?**  
A: Ya! VoLTE pakai jaringan dedicated, lebih jernih, tidak putus-putus. WhatsApp Call numpang di data network, quality tidak terjamin.

**Q: Kenapa HP saya tidak bisa VoLTE?**  
A:

1. HP tidak support VoLTE (HP lama)
2. Area belum ada coverage VoLTE
3. Setting VoLTE di HP belum aktif

---

## KESIMPULAN

### CSFB

- 3 fase: Measurement → Decision → Execution
- 2 skenario: Blind (cepat) vs Measurement (optimal)
- Release 9 = Flash CSFB (lebih cepat)

### VoLTE

- Voice over LTE (voice jadi data)
- Perlu IMS (IP Multimedia Subsystem)
- Lebih jernih, hemat baterai, support video call
- Strategi: Voice-over CS (CSFB) vs Voice-over LTE (VoLTE)

### Pilihan Operator

- Punya 2G/3G → pakai CSFB dulu
- Tidak punya 2G/3G → pakai VoLTE
- Ideal: VoLTE untuk masa depan (all-IP network)

---

**Catatan**: Materi ini adalah dasar VoLTE. Detail arsitektur IMS dan protokol SIP akan dibahas di materi lanjutan.
