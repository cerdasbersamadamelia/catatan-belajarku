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

### 2. Multiplexing: CDMA (Code Division Multiple Access)
- Semua user pakai frekuensi yang sama secara bersamaan
- Dibedakan dengan kode unik (spreading code)
- Lebih efisien dibanding TDMA di 2G
- Kapasitas jauh lebih besar

### 3. Frekuensi di Indonesia: 2100 MHz
- Band yang umum: Band I (2100 MHz)
- Uplink: 1920-1980 MHz
- Downlink: 2110-2170 MHz
- Bandwidth: 5 MHz per carrier

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

**Catatan**: Kecepatan ini adalah teoritis. Real speed biasanya lebih rendah karena interferensi, load jaringan, dan kondisi propagasi.

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
- RF transceiver
- Baseband processing
- Power control (fast power control 1500x/detik)
- Soft handover capability

**2. RNC (Radio Network Controller)**
- Radio resource management
- Admission control
- Load control
- Handover control (soft, softer, hard)
- Outer loop power control

**Iub Interface**: Node B ↔ RNC
**Iur Interface**: RNC ↔ RNC (untuk inter-RNC handover)

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
- Telkomsel: Shutdown bertahap sejak 2022
- Indosat: Shutdown di area urban
- XL Axiata: Sedang proses shutdown
- Smartfren: Tidak pernah deploy 3G (langsung CDMA2000/4G)

**Alternatif Voice:**
- 2G (GSM) masih aktif untuk voice
- VoLTE (Voice over LTE) di 4G
- VoNR (Voice over New Radio) di 5G

---

## Keunggulan 3G

✅ Kecepatan jauh lebih tinggi dari 2G (hingga 2 Mbps, HSPA+ 42 Mbps)
✅ Bisa browsing internet dengan nyaman
✅ Video call pertama kali (64 kbps - 384 kbps)
✅ Streaming video (YouTube 240p-360p)
✅ Mobile gaming online
✅ Soft handover (koneksi seamless antar cell)
✅ CDMA lebih tahan interferensi

---

## Keterbatasan 3G

❌ Masih pakai Circuit Switching untuk voice (kurang efisien)
❌ Konsumsi baterai tinggi (terutama HSPA+)
❌ Kecepatan upload jauh lebih rendah dari download
❌ Latency masih tinggi (100-150 ms) untuk gaming/real-time app
❌ Kapasitas terbatas di area padat (cell breathing effect)
❌ Teknologi transisi yang akhirnya ditinggalkan

---

## Perbandingan Cepat: 2G vs 3G

| Aspek | 2G (GSM/EDGE) | 3G (W-CDMA/HSPA) |
|-------|---------------|------------------|
| **Kecepatan Max** | 236 kbps | 42 Mbps (HSPA+) |
| **Modulasi** | GMSK, 8-PSK | QPSK, 16-QAM, 64-QAM |
| **Multiplexing** | TDMA | CDMA |
| **Frekuensi (ID)** | 900/1800 MHz | 2100 MHz |
| **Switching** | CS (voice), PS (data) | CS + PS |
| **Bandwidth** | 200 kHz | 5 MHz |
| **Use Case** | Voice, SMS, Email | Video call, streaming, browsing |
| **Tower** | BTS | Node B |
| **Controller** | BSC | RNC |

---

## Poin Penting yang Harus Diingat

🎯 **3G = W-CDMA** di Indonesia (standar UMTS Eropa)
🎯 **Perubahan nama semua komponen** (MS→UE, BTS→Node B, BSC→RNC)
🎯 **Dual switching**: CS untuk voice, PS untuk data
🎯 **3 kecepatan berbeda**: 144 kbps (rural), 384 kbps (urban outdoor), 2 Mbps (urban indoor)
🎯 **Frekuensi 2100 MHz** di Indonesia
🎯 **RNC bisa connect ke MSC 2G** untuk interoperability
🎯 **Shutdown bertahap** karena inefficiency dan migrasi ke 4G/5G
🎯 **eRIO slot management** memungkinkan realokasi hardware
🎯 **HSPA/HSPA+** adalah enhanced version (3.5G/3.75G/3.9G)

---

## Penutup

3G adalah **generasi transisi** yang membawa internet mobile mainstream. Meski sekarang sedang ditutup, 3G adalah fondasi penting untuk evolusi ke 4G dan 5G. Teknologi CDMA dan konsep packet switching di 3G masih dipakai (dalam bentuk lebih advanced) di generasi selanjutnya.

**Next**: Jaringan 4G (LTE) - All IP Network! 🚀
