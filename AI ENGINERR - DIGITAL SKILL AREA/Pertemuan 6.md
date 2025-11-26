# Pertemuan 6: Generative AI - Pengenalan

## 🎯 Overview
Pertemuan pertama tentang **Generative AI**! Kita mulai dari dasar - kenalan sama tools LLM dan praktik langsung pakai Google Gemini.

---

## 📚 Apa itu Generative AI?

### Definisi Simpel
Generative AI itu AI yang bisa **bikin konten sendiri**:
- ✍️ **Teks** - ChatGPT, Google Gemini, Claude
- 🎨 **Gambar** - Midjourney, DALL-E  
- 🎬 **Video** - Sora, Runway
- 🎵 **Audio** - Musik & suara

**Intinya:** AI yang bisa memproduksi output kreatif!

---

## 🏗️ 3 Level Generative AI

### 1️⃣ Model Level
- Training & arsitektur model dasar
- Jarang dibahas di bootcamp ini

### 2️⃣ System Level  
**Yang paling sering dipakai!**
- GitHub Copilot
- Cursor
- Claude Code
- Tools yang udah jadi untuk coding

### 3️⃣ Application Level
- Chatbot custom
- Aplikasi AI spesifik
- Implementasi langsung

---

## 🧠 Arsitektur Transformer

### Sejarah
Semua berawal dari paper **"Attention is All You Need"** → lahirlah Transformer!

### Komponen Utama

#### **ENCODER (Kiri)**
```
Input → Embedding → Positional Encoding → Multi-Head Attention → Feed Forward
```

**Dipakai untuk:** BERT & model understanding
- Bikin **masking** untuk isi kata kosong
- Contoh: Clinical BERT, Medical BERT

#### **DECODER (Kanan)**  
```
Output → Masked Multi-Head Attention → Feed Forward → Softmax
```

**Dipakai untuk:** GPT & model generasi
- ChatGPT
- Google Gemini
- Claude

### Cara Kerja Step-by-Step

**1. Input Embedding**
- Ubah kata jadi angka yang bisa dipahami komputer

**2. Positional Encoding**
- Kasih info posisi tiap kata

**3. Multi-Head Attention**
- Lihat hubungan kata dari **segala sisi**
- Contoh: "Saya mau pergi ke pasar"
  - Hubungan "pasar" dengan "mau"
  - Hubungan "pasar" dengan "pergi"

**4. Feed Forward**
- Pahami **makna** dari tiap kata

**5. Add & Norm**
- Normalisasi data biar stabil

### Bedanya Encoder vs Decoder

#### **Mask Multi-Head Attention (GPT)**
- **Tutup** kata-kata selanjutnya
- Biar model gak "nyontek" jawaban
- Contoh: Udah baca "Saya mau pergi ke..." → kata "pasar" ditutup dulu

#### **Softmax Layer**  
- Prediksi kata apa yang keluar selanjutnya
- Pilih probabilitas tertinggi

---

## 🔧 Tools yang Dipakai

### Google Gemini
**Kenapa pilih Gemini?**
✅ **GRATIS** (ada versi trial)
- Limit: 30 request per menit
- Cocok untuk belajar

**Alternatif:**
- ChatGPT (berbayar)
- Claude (berbayar tapi paling bagus untuk coding)

### Cara Dapetin API Gemini

1. Buka **Google AI Studio** (aistudio.google.com)
2. Klik **Get Started**
3. Klik **Get API Key**
4. Create API → Copy!

---

## 💡 Perbandingan LLM

| Model | Kelebihan | Kekurangan | Harga |
|-------|-----------|------------|-------|
| **ChatGPT** | Cepat, umum | Jawaban pendek | $$ |
| **Claude** | Coding terbaik! | Mahal banget | $$$ |
| **Gemini** | Gratis, analisis bagus | Ada limit | GRATIS |

### Kapan Pakai Apa?

- 🔍 **Analisis data/dokumen** → Gemini
- 💻 **Generate code** → Claude  
- ⚡ **Jawaban cepat** → ChatGPT

---

## 👨‍💻 Implementasi Sederhana

### Setup Awal
```python
import google.generativeai as genai

# Masukin API key
genai.configure(api_key="YOUR_API_KEY")

# Pilih model
model = genai.GenerativeModel('gemini-pro')

# Tanya!
response = model.generate_content("Apa fungsi dari LLM?")
print(response.text)
```

**Super simpel kan?** Ini cuma tanya-jawab biasa, belum kompleks!

---

## 📌 Dokumentasi Gemini

Bisa buat:
- 📄 **Text generation** (yang paling sering)
- 🖼️ **Image understanding** (OCR)
- 🎥 **Video analysis** (mahal!)
- 🔊 **Audio processing**

**Tips:** Dokumentasi Gemini bagus & lengkap!

---

## 🚀 Next Level: RAG

Di pertemuan berikutnya kita bakal bikin LLM yang **lebih kompleks** pakai **RAG (Retrieval Augmented Generation)**.

Contoh advance: **Adaptive RAG** (pertemuan 9-10)

---

## 💭 Kesimpulan

✅ Generative AI = AI yang bisa bikin konten  
✅ Transformer = Dasar semua LLM modern  
✅ Encoder untuk understanding, Decoder untuk generation  
✅ Gemini = pilihan terbaik untuk belajar (gratis!)  
✅ Masih tahap pengenalan - yang kompleks nanti dulu!

---

## ❓ FAQ

**Q: Training data LLM kayak gimana?**  
A: ChatGPT & Gemini ditraining dengan data BESAR banget. Kita di bootcamp fokus pakai API langsung.

**Q: OCR vs Image Understanding?**  
A: Image Understanding pakai CNN, bisa lebih dari sekedar baca teks (bisa analisis konten gambar).

**Q: Perlu fine-tuning gak?**  
A: Belum perlu! Kita pakai model yang udah jadi via API.

---

**Next:** Pertemuan 7 - RAG (Retrieval Augmented Generation)
