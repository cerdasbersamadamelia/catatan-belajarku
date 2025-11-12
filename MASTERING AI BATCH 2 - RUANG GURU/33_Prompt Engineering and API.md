# Prompt Engineering and API

## Novel AI - Tools Seru untuk Bikin Cerita! 🎭

### Apa itu Novel AI?
- Platform AI khusus untuk bikin cerita/novel
- Lebih spesifik dibanding ChatGPT untuk storytelling
- Punya banyak fitur khusus untuk narasi kreatif
- Website: **novelai.net**

### Fitur-Fitur Keren:
1. **Story Mode**: Kamu tinggal klik "send" berulang kali, tiba-tiba sudah jadi novel panjang!
2. **Memory Section**: Bisa tambah karakter, lokasi, lore (catatan cerita)
3. **Text Generation**: Ada free trial ~100 generasi gratis

### Tips Pakai Novel AI:
- Bisa kasih instruksi dalam kurung kurawal `{tell me the story about...}`
- Ada **editor token probability** - bisa pilih kata alternatif berdasarkan probabilitas
- Bisa generate ulang kalau hasil kurang memuaskan
- Pakai **Lorebook** untuk simpan catatan karakter, event, lokasi

**Contoh**: Bikin cerita Avatar - The Legend of Aang
```
Title: Avatar The Legend of Aang
Setting: World of Avatar with 4 elements
Main character: Aang (Airbender)
```

---

## Prompt Engineering - Seni Ngomong ke AI! 🎯

### Apa itu Prompt?
**Prompt = instruksi/clue yang kita kasih ke AI** supaya AI-nya paham mau ngapain.

**Analogi sederhana**: Kayak kamu kasih petunjuk ke teman pintar yang bisa jawab apa aja!

### Kenapa Prompt Penting?
- Prompt yang detail = jawaban lebih akurat
- Prompt terlalu general = AI bingung (halusinasi)
- **Tapi jangan terlalu detail** → boros token = biaya mahal!

### Komponen Prompt yang Baik:

#### 1. **Instruction** (Instruksi)
Kasih tahu AI mau ngapain!
```
"Analyze the sentiment of the following restaurant review"
```

#### 2. **Context** (Konteks)  
Kasih situasi/kondisi yang jelas
```
"Given the following review of a restaurant..."
```

#### 3. **Input Data**
Data yang mau diproses
```
"The food was delicious and the service was excellent!"
```

#### 4. **Output Indicator**
Format output yang diinginkan
```
Sentiment: [positive/negative/neutral]
```

### Contoh Prompt Lengkap:
```
Given the following review of a restaurant, 
classify the sentiment as positive, negative, or neutral:

Review: "The food was amazing but the service was slow"

Sentiment: 
```

---

## Tips Bikin Prompt Mantap! 💡

### 1. **Kasih Konteks yang Jelas**
❌ Buruk: "Tell me about it"
✅ Bagus: "Tell me about the implications of climate change on global agriculture"

### 2. **Spesifik dengan Data**
❌ Buruk: "Analyze the data"  
✅ Bagus: "Analyze the sales data from Q1 2024 and identify trends"

### 3. **Bikin Persona/Karakter**
```
"Kamu adalah seorang guru matematika yang sangat mengerti rumus.
Tolong bantu jawab pertanyaan berikut secara terstruktur step by step:

Pertanyaan: Temukan nilai x dalam persamaan 2x + 5 = 15"
```

Hasilnya jauh lebih baik karena AI punya "role"!

### 4. **Minta Step by Step**
Tambahkan: "langkah demi langkah" atau "step by step"
```
Jawaban:
Langkah 1: 2x + 5 = 15
Langkah 2: 2x = 15 - 5
Langkah 3: 2x = 10
Langkah 4: x = 5
```

### 5. **Adding Outside Knowledge (RAG)**
Kasih informasi tambahan kalau AI nggak tahu!

**Contoh**: ChatGPT nggak tahu CEO Ruang Guru siapa (data training terbatas)

**Solusi**: Kasih konteks dari Wikipedia
```
Konteks: [Copy paste info dari Wikipedia tentang Ruang Guru]

Pertanyaan: Siapa CEO Ruang Guru?
```

---

## Temperature Setting - Atur Kreativitas AI 🌡️

### Low Temperature (0 - 0.3)
- **Less creative**, lebih ngikut aturan
- Cocok untuk: Matematika, Fisika, hal-hal yang butuh akurasi
- Contoh: Soal hitungan, coding, data analysis

### High Temperature (0.7 - 1.0)
- **More creative**, lebih random
- Cocok untuk: Bikin cerita, puisi, brainstorming
- Resiko: Bisa halusinasi (ngasal)

**Pro Tip**: Jangan pakai temperature tinggi untuk fakta/data!

---

## Teknik-Teknik Prompt Engineering 🛠️

### 1. **Zero-Shot Prompting**
Langsung tanya, nggak kasih contoh
```
"Determine the mood of the following statement: 
I love this product!"
```

### 2. **Few-Shot Prompting**  
Kasih beberapa contoh dulu
```
Review: "The food is great!" → Positive
Review: "Service was terrible" → Negative
Review: "It was okay" → Neutral

Review: "Amazing experience!" → ?
```

### 3. **Chain of Thought (CoT)**
Minta AI mikir step by step
```
Soal: Saya punya 20 permen. Saya makan 5 di pagi hari, 
kasih ke teman 12. Berapa sisa?

Jawab dengan langkah-langkah!
```

### 4. **Zero-Shot CoT**
Cukup tambah: **"Let's think step by step"**
```
Pertanyaan: 2x + 5 = 15. Berapa x?

Let's think step by step.
```

### 5. **Generated Knowledge Prompting**
Generate knowledge dulu, baru jawab
```
Step 1: Generate facts about penguins
Step 2: Based on facts, answer: Do penguins fly?
```

---

## RAG - Retrieval Augmented Generation 🔍

### Apa itu RAG?
**RAG = Kasih konteks tambahan ke LLM** supaya jawabannya lebih akurat!

**Beda RAG vs Fine-tuning**:
- **Fine-tuning**: Training ulang model dengan data baru (butuh komputasi gede)
- **RAG**: Inject informasi ke prompt (nggak butuh training)

### Cara Kerja RAG:
1. User tanya sesuatu
2. System cari info relevan dari database
3. Info dimasukkan ke prompt
4. LLM jawab berdasarkan konteks + info database

### Keuntungan RAG:
- ✅ Nggak perlu training ulang
- ✅ Hemat token (cuma ambil info relevan)
- ✅ Bisa update info kapan aja
- ✅ Lebih murah daripada fine-tuning

**Contoh Use Case**:
- Chatbot produk e-commerce (ambil data dari database)
- Q&A dokumen perusahaan
- Customer support dengan knowledge base

---

## API - Application Programming Interface 🔌

### Apa itu API?
**API = Jembatan komunikasi antar aplikasi**

**Analogi sederhana**: Kayak pelayan restoran
- Kamu (Client) pesan makanan
- Pelayan (API) bawa pesanan ke dapur
- Dapur (Server) masak
- Pelayan bawa makanan ke kamu

### Kenapa Butuh API?
1. **Tim beda-beda ngoding**
   - Data Scientist: Bikin model
   - Backend: Bikin API
   - Frontend: Bikin tampilan
   
2. **Integrasi mudah** - Semua tim pake API yang sama

3. **Model bisa diakses banyak orang** tanpa share code

### Contoh Kasus:
```
Data Scientist → Bikin model ML (akurasi 80%)
Backend → Wrap model di API
Frontend → Akses model lewat API
User → Pakai aplikasi tanpa tahu model di belakangnya
```

---

## FastAPI - Framework API Tercepat! ⚡

### Kenapa FastAPI?
1. **Lebih cepat** dari Flask
2. **Built-in UI** (Swagger UI & ReDoc)
3. **Auto documentation**
4. **Type validation** otomatis
5. **Modern & mudah**

### Install FastAPI:
```bash
pip install fastapi uvicorn
```

### Struktur Dasar FastAPI:
```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def welcome():
    return {"message": "Welcome to our API"}
```

### Jalankan Server:
```bash
uvicorn main:app --reload
```

Akses di: `http://127.0.0.1:8000`

---

## HTTP Methods 📡

### 1. **GET** - Ambil Data
```python
@app.get("/products")
def get_products():
    return {"products": ["laptop", "phone", "tablet"]}
```

### 2. **POST** - Tambah Data
```python
@app.post("/cart")
def add_to_cart(item_id: int):
    return {"message": f"Item {item_id} added to cart"}
```

### 3. **DELETE** - Hapus Data
```python
@app.delete("/cart/{item_id}")
def remove_from_cart(item_id: int):
    return {"message": f"Item {item_id} removed"}
```

---

## Automatic Documentation - Swagger UI 📚

FastAPI otomatis bikin dokumentasi!

**Akses dokumentasi**:
- Swagger UI: `http://localhost:8000/docs`
- ReDoc: `http://localhost:8000/redoc`

### Keuntungan:
- ✅ Bisa test API langsung di browser
- ✅ Lihat semua endpoint
- ✅ Debugging jadi gampang
- ✅ Share ke tim untuk testing

---

## Ngrok - Bikin API Online! 🌐

### Apa itu Ngrok?
Tools untuk bikin localhost jadi bisa diakses online (public URL)

### Install & Setup:
```bash
# Install ngrok
pip install pyngrok

# Set auth token
from pyngrok import ngrok
ngrok.set_auth_token("YOUR_TOKEN")

# Start tunnel
public_url = ngrok.connect(8000)
print(public_url)
```

### Benefit:
- Share API ke teman/client tanpa deploy
- Test dari device lain
- Demo aplikasi instant

---

## Router Chain - API Pintar Nentuin Jawaban 🎯

### Konsep:
Kayak polisi lalu lintas yang arahkan traffic ke jalan yang tepat!

```python
# Analogi
if pertanyaan_fisika:
    pakai_prompt_fisika()
elif pertanyaan_matematika:
    pakai_prompt_matematika()
```

### Use Case:
Bikin chatbot yang bisa jawab berbagai topik:
- Pertanyaan Matematika → Expert Matematika
- Pertanyaan Fisika → Expert Fisika  
- Pertanyaan Sejarah → Expert Sejarah
- Lainnya → General Assistant

---

## Text Generation API - Deploy Model! 🚀

### Contoh: Deploy GPT-2
```python
from fastapi import FastAPI
from transformers import pipeline

app = FastAPI()
generator = pipeline("text-generation", model="gpt2")

@app.post("/generate")
def generate_text(prompt: str):
    result = generator(prompt, max_length=50)
    return {"generated_text": result[0]["generated_text"]}
```

### Jalankan:
```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

---

## Pydantic - Data Validation 🛡️

### Kenapa Pydantic?
**Auto validasi tipe data!** Nggak usah manual cek.

### Contoh:
```python
from pydantic import BaseModel

class Item(BaseModel):
    prompt: str
    max_length: int = 50

@app.post("/generate")
def generate_text(item: Item):
    # Pydantic auto-validasi:
    # - prompt harus string
    # - max_length harus integer
    result = generator(item.prompt, max_length=item.max_length)
    return result
```

Kalau user kirim data salah → **Error otomatis!**

---

## Best Practices ✨

### 1. **Prompt Engineering**
- ✅ Spesifik dan jelas
- ✅ Kasih contoh kalau perlu (few-shot)
- ✅ Pakai step-by-step untuk reasoning
- ✅ Sesuaikan temperature dengan task
- ❌ Jangan terlalu panjang (boros token)

### 2. **API Development**
- ✅ Pakai FastAPI untuk speed
- ✅ Validasi input pakai Pydantic
- ✅ Bikin dokumentasi otomatis
- ✅ Handle error dengan baik
- ✅ Test pakai Swagger UI

### 3. **RAG Implementation**
- ✅ Index data ke vector database
- ✅ Cuma ambil info relevan (hemat token)
- ✅ Update knowledge base berkala
- ❌ Jangan masukkan semua data ke prompt

---

## Job Opportunities - Prompt Engineer! 💼

### Apa Itu Prompt Engineer?
Pekerjaan baru yang fokus **utak-atik prompt** untuk hasil optimal!

### Skill yang Dibutuhkan:
- Paham LLM & cara kerjanya
- Bisa eksperimen (A/B testing)
- Tahu tools: Python, LangChain, APIs
- Analisis performa prompt
- Kolaborasi dengan tim ML/Backend

### Jenis Pekerjaan:
1. **Prompt Engineer** - Optimize prompts
2. **LLM Engineer** - Fine-tune & deploy models
3. **AI Engineer** - Full development cycle

**Kabar Baik**: Job-nya banyak & demand tinggi! 🔥

---

## Summary - Yang Wajib Diingat! 📝

### Prompt Engineering:
1. Prompt = instruksi ke AI (makin detail makin bagus, tapi jangan kelamaan)
2. Komponen: Instruction + Context + Input + Output indicator
3. Teknik: Zero-shot, Few-shot, Chain of Thought
4. RAG = inject knowledge tanpa fine-tuning
5. Temperature: Low (akurat) vs High (kreatif)

### API Development:
1. API = jembatan komunikasi antar aplikasi
2. FastAPI = framework tercepat & termudah
3. HTTP Methods: GET, POST, DELETE
4. Auto-documentation di `/docs`
5. Deploy model ML jadi API untuk production

### Tools:
- **Novel AI**: Bikin cerita kreatif
- **FastAPI**: Build API cepat
- **Ngrok**: Share API ke public
- **Pydantic**: Validasi data otomatis

---

## Next Steps 🎯

1. **Practice**: Coba bikin prompt untuk berbagai use case
2. **Build API**: Deploy model ML kamu sendiri pakai FastAPI
3. **Eksperimen**: Test berbagai teknik prompting
4. **Kolaborasi**: Share API ke teman untuk testing
5. **Explore**: Coba tools lain (LangChain, ML Flow)

**Ingat**: Prompt Engineering + API = skill wajib AI Engineer! 🚀

---

## Resources 📚

- FastAPI Docs: https://fastapi.tiangolo.com
- Novel AI: https://novelai.net
- OpenAI Playground: https://platform.openai.com
- Ngrok: https://ngrok.com
- LangChain: https://langchain.com

---

**Happy Learning! 🎉**
