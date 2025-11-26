# Pertemuan 7: RAG (Retrieval Augmented Generation)

## 🎯 Overview
Materi **serius dimulai**! Kita masuk ke framework LangChain dan bikin RAG pertama kita.

---

## 🛠️ Tools yang Dipakai

### 1. LangChain 🦜🔗
**Framework utama untuk LLM!**

**Kenapa LangChain?**
- Lebih praktis & konsisten
- Dokumentasi lengkap
- Komunitas besar

**Alternatif:**
- LlamaIndex (sintaks kurang konsisten)
- Haystack

### 2. Google Gemini 💎
Model LLM yang kita pakai (gratis!)

### 3. HuggingFace 🤗
Buat **embedding** (ubah teks jadi angka)
- Gratis!
- Alternatif dari OpenAI Embedding

### 4. Vector Database 🗄️
Tempat nyimpen dokumen yang udah di-load
- **FAISS** (yang kita pakai - simpel!)
- PineCone
- ChromaDB
- Qdrant
- Weaviate

---

## 📚 Fitur Penting LangChain

### 1️⃣ Document Loaders 📂

**Fungsi:** Baca dokumen yang mau dianalisis

**Format yang Didukung:**
- 📄 PDF
- 📝 Word (DOCX)
- 📊 PowerPoint (PPTX)
- 📈 Excel & CSV
- 🌐 Website (pakai BeautifulSoup)
- ☁️ Cloud (AWS, Azure, Google Drive)
- 📱 Social Media (Twitter, Reddit)
- 💬 Chat (Telegram, WhatsApp)

**Contoh Code:**
```python
from langchain_community.document_loaders import PyPDFLoader

# Load PDF
loader = PyPDFLoader("dokumen.pdf")
docs = loader.load()

print(docs)  # Isi dokumen
```

**Tips:**
- PDF → pakai `PyPDFLoader`
- CSV → pakai `UnstructuredCSVLoader` (lebih stabil)
- Website → butuh install `beautifulsoup4`

### 2️⃣ Memory 🧠

**Fungsi:** Simpan percakapan sebelumnya

**Kenapa Penting?**
Biar LLM gak kehilangan konteks!

**Contoh Tanpa Memory:**
```
User: "Bisa jelaskan jadwal bus Jakarta?"
LLM: "Jadwal bus Jakarta adalah..."

User: "Jurusannya ke mana saja?"
LLM: "Sorry, I cannot answer. Jurusan apa yang dimaksud?"
```

**Contoh Dengan Memory:**
```
User: "Bisa jelaskan jadwal bus Jakarta?"
LLM: "Jadwal bus Jakarta adalah..."

User: "Jurusannya ke mana saja?"
LLM: "Jurusannya akan pergi ke Blok M, Lebak Bulus, ..."
```

### 3️⃣ LLM Agents 🤖

Bakal dibahas pertemuan 8!
- Eksekusi Python code
- Eksekusi SQL query
- Automasi

### 4️⃣ Koneksi ke Tools 🔌
- Google Gemini ✅
- HuggingFace ✅
- Vector Database ✅

---

## 💾 Vector Database Deep Dive

### Apa itu Vector Database?

**Definisi Sederhana:**
Database khusus untuk nyimpen **vektor** (angka-angka hasil embedding)

### Proses Kerja

```
Teks → Embedding → Vektor → Disimpan di Vector DB
```

**Contoh:**
```
"Saya akan pergi" 
   ↓ embedding
[0.034, 0.045, 0.021, 0.067, ...]
   ↓
Disimpan di Vector DB
```

### Pilihan Vector Database

#### **FAISS** (Facebook AI Similarity Search) ⭐
- **Paling gampang** - plug & play!
- Gratis
- Lokal (gak perlu cloud)
- Cocok untuk belajar

**Setup:**
```python
pip install faiss-cpu

from langchain_community.vectorstores import FAISS
```

#### **PineCone** ☁️
- Cloud-based
- Setting lebih ribet
- Butuh API key
- Cocok untuk production

#### **ChromaDB** 📦
- Lebih canggih dari FAISS
- Ada engine SQLite bawaan
- Setup sedang

#### **Qdrant** 🚀
- **Paling susah!**
- Butuh Docker
- Untuk **aplikasi skala BESAR**

#### **Weaviate** 🌊
- Setara Qdrant
- Enterprise-level
- Super kompleks

---

## 🔄 Cara Kerja RAG

### Diagram Alur

```
1. Dokumen PDF/CSV/Word
        ↓
2. LangChain Loader (baca dokumen)
        ↓
3. Embedding (ubah jadi vektor)
        ↓
4. Vektor Database (simpan)
        ↓
5. User tanya → Embedding juga
        ↓
6. Similarity Search (cari yang cocok)
        ↓
7. Kirim hasil + pertanyaan ke LLM
        ↓
8. LLM kasih jawaban!
```

### Detail Proses

#### **Step 1: Chunking** ✂️
Potong dokumen jadi bagian kecil

**Contoh:**
- Dokumen 600 kata
- Chunk size = 200
- Hasil: 3 potongan (chunk 1, 2, 3)

**Code:**
```python
from langchain.text_splitter import RecursiveCharacterTextSplitter

text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=200,
    chunk_overlap=100  # overlap biar lebih smooth
)
chunks = text_splitter.split_documents(docs)
```

#### **Step 2: Embedding** 🔢
Ubah tiap chunk jadi vektor

```python
from langchain_huggingface import HuggingFaceEmbeddings

embeddings = HuggingFaceEmbeddings(
    model_name="sentence-transformers/all-MiniLM-L6-v2"
)
```

#### **Step 3: Simpan ke Vector DB** 💾
```python
vectorstore = FAISS.from_documents(chunks, embeddings)
```

#### **Step 4: User tanya → Similarity Search** 🔍
```python
query = "Bagaimana cara kerja LLM?"
# Query juga di-embedding
# Cari chunk yang paling mirip
docs_found = vectorstore.similarity_search(query, k=3)
```

#### **Step 5: Kirim ke LLM** 🤖
```python
from langchain.prompts import PromptTemplate

template = """
Based on the context: {context}
Answer the question: {question}
"""

# LLM jawab berdasarkan context + question
```

---

## 🎯 Kenapa RAG Itu Bagus?

### 1. Hemat Biaya! 💰
- Input ke LLM **lebih sedikit**
- Cuma kirim chunk yang relevan
- Token lebih murah!

### 2. Jawaban Lebih Tepat! 🎯
- LLM fokus ke context yang relevan
- Gak "ngalor ngidul"
- Lebih detail & jelas

### 3. Gak Perlu Retrain Model! ⚡
- Cukup update dokumen
- Model tetap sama
- Scalable!

---

## 💻 Implementasi Code Lengkap

### Setup Library
```bash
pip install langchain
pip install langchain-google-genai
pip install langchain-community
pip install pypdf
pip install unstructured
pip install faiss-cpu
pip install sentence-transformers
```

### Code RAG Simpel
```python
# 1. Import semua
from langchain_community.document_loaders import PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_huggingface import HuggingFaceEmbeddings
from langchain_community.vectorstores import FAISS
from langchain_google_genai import ChatGoogleGenerativeAI
from langchain.chains import RetrievalQA

# 2. Load dokumen
loader = PyPDFLoader("cv.pdf")
docs = loader.load()

# 3. Chunking
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=200,
    chunk_overlap=100
)
chunks = text_splitter.split_documents(docs)

# 4. Embedding
embeddings = HuggingFaceEmbeddings()

# 5. Vector Database
vectorstore = FAISS.from_documents(chunks, embeddings)

# 6. Setup LLM
llm = ChatGoogleGenerativeAI(
    model="gemini-pro",
    google_api_key="YOUR_API_KEY"
)

# 7. Query!
query = "Can you give me some insight based on my CV?"
results = vectorstore.similarity_search(query)

# 8. Generate jawaban
from langchain.prompts import PromptTemplate

template = """
Based on these documents: {docs}
Answer this question: {question}
"""

prompt = PromptTemplate(template=template, input_variables=["docs", "question"])
response = llm.invoke(prompt.format(docs=results, question=query))
print(response)
```

---

## ⚠️ Warning & Tips

### LangChain itu Inkonsisten!
- **Dokumentasi kadang outdated**
- Code yang work hari ini, besok bisa error
- Sering update tiba-tiba
- **Jangan copas mentah-mentah!**

### Tips Debugging
1. Cek versi library: `pip list | grep langchain`
2. Kalau error → cari di GitHub Issues
3. Gabung komunitas Discord/Slack
4. Tanya ChatGPT/Claude untuk fix

---

## 🆚 Traditional RAG vs No RAG

| Aspek | Tanpa RAG | Dengan RAG |
|-------|-----------|------------|
| **Input** | Kirim SEMUA dokumen | Cuma kirim chunk relevan |
| **Token** | Boros banget | Hemat! |
| **Jawaban** | Bisa ngalor-ngidul | Fokus & tepat |
| **Biaya** | Mahal | Murah |
| **Akurasi** | Tergantung | Lebih tinggi |

---

## 🚀 Use Case RAG

**Cocok untuk:**
- ✅ Chatbot FAQ perusahaan
- ✅ Analisis dokumen legal
- ✅ Research paper summarization
- ✅ Customer support automation
- ✅ Knowledge base internal

**Kurang cocok untuk:**
- ❌ Real-time data (pakai tools/API)
- ❌ Perhitungan kompleks (pakai Python Agent)
- ❌ Data yang sering berubah

---

## 📌 Kesimpulan

✅ LangChain = framework wajib untuk LLM  
✅ RAG = cara bikin LLM lebih pintar & murah  
✅ Vector DB = tempat nyimpen dokumen dalam bentuk vektor  
✅ FAISS = pilihan terbaik untuk belajar  
✅ Traditional RAG masih dipakai di early-stage startup  

**Next:** Advance RAG ada di pertemuan 9!

---

## ❓ FAQ

**Q: BARD vs GPT?**  
A: BARD = Gemini versi lama (2023). Sekarang pakai Gemini aja.

**Q: MCP itu apa?**  
A: Model Context Protocol - untuk koneksi antar aplikasi (Claude → Google Drive → Database). Agak ribet, jarang dipake.

**Q: Kapan pakai Advance RAG?**  
A: Kalau kerja di BUMN/perusahaan mapan. Kalau startup kecil, traditional RAG cukup!

---

**Next:** Pertemuan 8 - LLM Agents (SQL & Python)
