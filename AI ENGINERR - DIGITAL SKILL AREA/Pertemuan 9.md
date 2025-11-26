# Pertemuan 9: Advance RAG & LLM OPS

## 🎯 Overview
**MATERI PALING SUSAH!** Kita bahas 4 metode Advance RAG + LLM OPS untuk monitoring.

---

## 🌟 LangGraph - Framework Baru!

### Apa itu LangGraph?

**Workflow builder untuk LLM!**
- Masih satu keluarga dengan LangChain
- Buat bikin alur yang kompleks
- Pakai **node & edge** (kayak flowchart)

### Cara Kerja LangGraph

#### **1. Define Nodes**
```python
# Contoh: 3 LLM calls
def llm_call_1(state):
    # Proses 1
    return result

def llm_call_2(state):
    # Proses 2
    return result

def llm_call_3(state):
    # Proses 3
    return result
```

#### **2. Build Graph**
```python
from langgraph.graph import StateGraph

workflow = StateGraph()

# Tambah nodes
workflow.add_node("llm1", llm_call_1)
workflow.add_node("llm2", llm_call_2)
workflow.add_node("llm3", llm_call_3)

# Sambungin dengan edges
workflow.add_edge("START", "llm1")
workflow.add_edge("llm1", "llm2")
workflow.add_edge("llm2", "llm3")
workflow.add_edge("llm3", "END")

# Compile!
app = workflow.compile()
```

#### **3. Visualisasi Alur**

```
START → LLM Call 1 → LLM Call 2 → LLM Call 3 → END
```

### Karakteristik LangGraph
✅ Wajib dimulai dari **START**  
✅ Setiap node harus di-define dulu  
✅ Pakai `add_edge()` untuk sambungin  
✅ `END` = selesai, no callback  

---

## 🆚 Traditional RAG vs Advance RAG

### Perbedaan Utama

| Aspek | Traditional RAG | Advance RAG |
|-------|-----------------|-------------|
| **Validasi** | Gak ada | Ada pengecekan berkali-kali |
| **Tools** | Gak pakai | Pakai banyak tools (ArXiv, Wikipedia, Tavily) |
| **Kompleksitas** | Simpel | Kompleks banget |
| **Use Case** | Startup kecil | Perusahaan besar/BUMN |
| **Akurasi** | Standar | Tinggi |

### 2 Perbedaan Kunci

#### **1. Validasi** ✅
- **Traditional:** Langsung kasih jawaban
- **Advance:** Cek berkali-kali → valid gak? → kalau gak, ulang!

#### **2. Tools Integration** 🛠️
- **Traditional:** Cuma dokumen user
- **Advance:** Ambil data dari ArXiv, Wikipedia, Tavily (search engine)

---

## 🚀 4 Metode Advance RAG

### 1️⃣ Agentic RAG ⭐ (PALING BAGUS)

#### Workflow

```
START
  ↓
Tool Selection (pilih tools: ArXiv/Wikipedia/Tavily)
  ↓
Multi-Retrieve (ambil data dari berbagai sumber)
  ↓
Grade (nilai: relevan gak?)
  ├─ NO → Adapt (coba strategi lain) → kembali ke Multi-Retrieve
  └─ YES ↓
Generate (kirim ke LLM)
  ↓
Evaluate (cek jawaban: bagus gak?)
  ├─ NO → Adapt (revisi) → kembali ke Generate
  └─ YES ↓
END (deliver ke user)
```

#### **Ciri Khas:**
- 🤖 **Kerja mandiri** (self-planning)
- 🔄 **Feedback loop ganda** (grade + evaluate)
- 🛠️ **Multi-tools** (gak cuma dokumen)
- 🧠 **Paling cerdas** tapi paling kompleks

#### **Tools Selection**

| Tool | Fungsi | Contoh |
|------|--------|--------|
| **ArXiv Query** | Research paper | "Find papers about transformers" |
| **Wikipedia Query** | Info umum | "What is RAG?" |
| **Tavily Research** | Berita & event terkini | "Latest AI news 2024" |

#### **Grade & Evaluate**
- **Grade:** Apakah retrieval relevan?
- **Evaluate:** Apakah jawaban LLM memuaskan?
- Keduanya pakai **self-reflection** (LLM nilai diri sendiri!)

#### **Code Snippet**
```python
from langgraph.graph import StateGraph
from langchain.tools import ArxivQueryRun, WikipediaQueryRun, TavilySearchResults

# Setup tools
tools = [ArxivQueryRun(), WikipediaQueryRun(), TavilySearchResults()]

# Define nodes
def tool_selection(state):
    # LLM pilih tools yang dibutuhkan
    return selected_tools

def multi_retrieve(state):
    # Ambil data dari tools yang dipilih
    return documents

def grade_documents(state):
    # Nilai: relevan gak?
    return "yes" or "no"

def generate(state):
    # LLM kasih jawaban
    return answer

def evaluate(state):
    # Cek kualitas jawaban
    return "yes" or "no"

# Build graph
workflow = StateGraph()
workflow.add_node("tool_selection", tool_selection)
workflow.add_node("multi_retrieve", multi_retrieve)
workflow.add_node("grade", grade_documents)
workflow.add_node("generate", generate)
workflow.add_node("evaluate", evaluate)

# Conditional edges
workflow.add_conditional_edges(
    "grade",
    lambda x: "adapt" if x == "no" else "generate"
)

app = workflow.compile()
```

---

### 2️⃣ Adaptive RAG

#### Workflow

```
START
  ↓
Adaptive Retriever (deteksi karakteristik pertanyaan)
  ↓
Grade Documents
  ├─ NO → Web Search (Tavily)
  └─ YES ↓
Generate (LLM)
  ↓
END
```

#### **Ciri Khas:**
- 🎯 **Adaptif** sesuai jenis pertanyaan
- 🔍 **Karakteristik query:** temporal, faktual, spesifik
- 🌐 **Fallback ke web search** kalau dokumen gak cukup

#### **Query Analyzer**
```python
class QueryAnalyzer:
    def analyze(self, query):
        # Cek karakteristik
        if "latest" in query or "2024" in query:
            return "temporal"  # Butuh data terkini
        elif "what is" in query:
            return "factual"   # Butuh definisi
        else:
            return "specific"  # Butuh dokumen spesifik
```

**Contoh:**
- "Latest AI news" → **temporal** → pakai Tavily
- "What is RAG?" → **factual** → pakai Wikipedia
- "Explain section 3.2 of the paper" → **specific** → pakai dokumen user

#### **Code Snippet**
```python
def adaptive_retriever(state):
    query_type = analyze_query(state["question"])
    
    if query_type == "temporal":
        return tavily_search(state["question"])
    elif query_type == "factual":
        return wikipedia_search(state["question"])
    else:
        return vector_search(state["question"])
```

---

### 3️⃣ Hybrid RAG

#### Workflow

```
START
  ↓
Multi-Retrieval (FAISS + BM25) - DUA METODE!
  ↓
Grade Documents
  ├─ NO → Web Search
  └─ YES ↓
Generate
  ↓
Grade Generation (cek jawaban)
  ├─ NO → Rewrite Question → kembali ke Multi-Retrieval
  └─ YES ↓
END
```

#### **Ciri Khas:**
- 🔄 **Dual retrieval** - FAISS (vector) + BM25 (keyword)
- 📝 **Question rewriting** kalau jawaban jelek
- 🎯 **Hasil lebih luas** karena pakai 2 metode

#### **FAISS vs BM25**

| Metode | Cara Kerja | Kelebihan |
|--------|------------|-----------|
| **FAISS** | Vector similarity | Paham konteks & semantik |
| **BM25** | Keyword matching | Cepat & akurat untuk exact match |

**Kenapa pakai keduanya?**
- FAISS: "cara kerja AI" → "metode artificial intelligence" ✅
- BM25: "transformer architecture" → harus ada kata "transformer" ✅

#### **Code Snippet**
```python
from langchain.retrievers import BM25Retriever, EnsembleRetriever
from langchain_community.vectorstores import FAISS

# Setup dual retrieval
bm25_retriever = BM25Retriever.from_documents(docs)
faiss_retriever = FAISS.from_documents(docs, embeddings).as_retriever()

# Combine!
ensemble_retriever = EnsembleRetriever(
    retrievers=[bm25_retriever, faiss_retriever],
    weights=[0.5, 0.5]  # Equal weight
)
```

#### **Question Rewriting**
Kalau jawaban jelek, LLM tulis ulang pertanyaan:

```
Original: "Gimana caranya?"
Rewritten: "Bagaimana cara kerja Transformer architecture dalam Natural Language Processing?"
```

---

### 4️⃣ Self-Corrective RAG

#### Workflow

```
START
  ↓
Retrieve (pakai 1 metode aja - FAISS)
  ↓
Grade Documents
  ├─ NO → Web Search
  └─ YES ↓
Generate
  ↓
Grade Generation
  ├─ NO → Rewrite Question → kembali ke Retrieve
  └─ YES ↓
END
```

#### **Ciri Khas:**
- Sama kayak **Hybrid** tapi **cuma pakai 1 retrieval method**
- Lebih simpel dari Hybrid
- Cocok untuk dokumen yang udah tersusun rapi

#### **Kapan Pakai Self-Corrective?**
✅ Dokumen sudah spesifik  
✅ Gak butuh multi-source  
✅ Mau lebih cepat dari Hybrid  

---

## 🏆 Perbandingan 4 Metode

| Metode | Kompleksitas | Akurasi | Speed | Use Case |
|--------|--------------|---------|-------|----------|
| **Agentic** ⭐ | ⚫⚫⚫⚫⚫ | ⭐⭐⭐⭐⭐ | 🐌 | BUMN, enterprise besar |
| **Adaptive** | ⚫⚫⚫⚫ | ⭐⭐⭐⭐ | 🏃 | Chatbot dengan berbagai jenis query |
| **Hybrid** | ⚫⚫⚫ | ⭐⭐⭐⭐ | 🚶 | Dokumen besar & kompleks |
| **Self-Corrective** | ⚫⚫ | ⭐⭐⭐ | 🏃‍♂️ | Dokumen spesifik & tersusun |

### Rekomendasi

**Bingung pilih mana?** → Pilih **Agentic RAG**!  
Kenapa?
- Paling fleksibel
- Paling akurat
- Scope paling luas

---

## 🔧 Setup Advance RAG

### Library Wajib
```bash
pip install langchain
pip install langchain-google-genai
pip install langchain-community
pip install langgraph  # PENTING!
pip install arxiv
pip install wikipedia
pip install tavily-python
pip install faiss-cpu
pip install tiktoken
```

### Tavily API

**Cara dapetin:**
1. Buka **tavily.com**
2. Sign up pakai Google
3. Create API key
4. **Limit:** 1000 kredit (1 request = 1 kredit)

### Embedding Warning!
```python
# JANGAN pakai ini (udah deprecated)
from langchain.embeddings import GoogleGenerativeAIEmbeddings

# Pakai ini!
from langchain_huggingface import HuggingFaceEmbeddings
```

---

## 🧪 Implementasi Agentic RAG

### Full Code Structure

```python
# 1. Import semua
from langgraph.graph import StateGraph
from langchain.tools import ArxivQueryRun, WikipediaQueryRun, TavilySearchResults
from langchain_google_genai import ChatGoogleGenerativeAI
from langchain_huggingface import HuggingFaceEmbeddings

# 2. Setup tools
arxiv = ArxivQueryRun()
wikipedia = WikipediaQueryRun()
tavily = TavilySearchResults(api_key="YOUR_TAVILY_KEY")
tools = [arxiv, wikipedia, tavily]

# 3. Setup LLM
llm = ChatGoogleGenerativeAI(model="gemini-pro")

# 4. Load dokumen
from langchain_community.document_loaders import PyPDFLoader
loader = PyPDFLoader("research.pdf")
docs = loader.load()

# 5. Chunking
from langchain.text_splitter import RecursiveCharacterTextSplitter
splitter = RecursiveCharacterTextSplitter(chunk_size=500)
chunks = splitter.split_documents(docs)

# 6. Vector DB
embeddings = HuggingFaceEmbeddings()
from langchain_community.vectorstores import FAISS
vectorstore = FAISS.from_documents(chunks, embeddings)

# 7. Define nodes
def tool_selection_node(state):
    prompt = """Select best tools for: {question}
    Available: arxiv (academic), wikipedia (general), tavily (news)"""
    response = llm.invoke(prompt.format(question=state["question"]))
    return {"selected_tools": response}

def multi_retrieve_node(state):
    # Retrieve dari semua sources
    all_docs = []
    for tool in state["selected_tools"]:
        result = tool.run(state["question"])
        all_docs.append(result)
    return {"documents": all_docs}

def grade_node(state):
    prompt = """Are these documents relevant? {docs}
    Question: {question}
    Answer: yes or no"""
    response = llm.invoke(prompt.format(
        docs=state["documents"],
        question=state["question"]
    ))
    return {"grade": response}

def generate_node(state):
    prompt = """Based on: {docs}
    Answer: {question}"""
    response = llm.invoke(prompt.format(
        docs=state["documents"],
        question=state["question"]
    ))
    return {"answer": response}

def evaluate_node(state):
    prompt = """Is this answer good? {answer}
    For question: {question}
    Answer: yes or no"""
    response = llm.invoke(prompt.format(
        answer=state["answer"],
        question=state["question"]
    ))
    return {"evaluation": response}

# 8. Build workflow
workflow = StateGraph()

workflow.add_node("tool_selection", tool_selection_node)
workflow.add_node("multi_retrieve", multi_retrieve_node)
workflow.add_node("grade", grade_node)
workflow.add_node("generate", generate_node)
workflow.add_node("evaluate", evaluate_node)

# Edges
workflow.add_edge("START", "tool_selection")
workflow.add_edge("tool_selection", "multi_retrieve")
workflow.add_conditional_edges(
    "grade",
    lambda x: "adapt" if x["grade"] == "no" else "generate"
)
workflow.add_conditional_edges(
    "evaluate",
    lambda x: "adapt" if x["evaluation"] == "no" else "END"
)

# 9. Compile
app = workflow.compile()

# 10. Run!
result = app.invoke({
    "question": "Explain transformer architecture"
})
print(result["answer"])
```

---

## 📊 LLM OPS - Monitoring

### Apa itu LLM OPS?

**Tracking performa LLM!**

Yang ditracking:
- ⏱️ **Latency** - Berapa lama LLM jawab?
- 🎫 **Token Usage** - Berapa token terpakai?
- 💰 **Cost** - Berapa biaya per request?
- ✅ **Success Rate** - Berapa % berhasil?

### LangSmith - Tool Monitoring

#### Setup
```python
import os

# Tambah di awal code
os.environ["LANGCHAIN_TRACING_V2"] = "true"
os.environ["LANGCHAIN_PROJECT"] = "my-project"
os.environ["LANGCHAIN_ENDPOINT"] = "https://api.smith.langchain.com"
os.environ["LANGCHAIN_API_KEY"] = "your-key"

# DONE! Otomatis tertrack
```

#### Cara Dapetin API Key

1. Buka **smith.langchain.com**
2. Sign up
3. Klik **New Project**
4. Generate API Key
5. Copy!

#### Dashboard

Bisa lihat:
- 📈 Latency graph (rata-rata response time)
- 🎫 Token consumption
- 🔄 Workflow visualization (START → nodes → END)
- ❌ Error logs

**Super gampang!** Gak perlu coding extra.

---

## 🆚 Alternatif: Comet Opik

**Mirip LangSmith, tapi:**
- Lebih berat
- Setup lebih ribet
- Cocok untuk production skala besar

**Rekomendasi:** Pakai LangSmith aja untuk belajar!

---

## ⚠️ Warning & Tips

### 1. LangChain Inconsistency
**Masalah:** Code yang work hari ini, besok bisa error
**Solusi:**
- Jangan copas mentah-mentah
- Cek versi library
- Kalau error, kontak instruktur

### 2. Embedding Deprecated
```python
# JANGAN pakai Google Generative AI Embeddings
# Pakai HuggingFace aja (gratis & stabil)
```

### 3. Tavily Limit
- 1000 kredit gratis
- 1 search = 1 kredit
- Hemat pakai!

### 4. Computational Cost
**Agentic RAG berat!**
- Butuh GPU kalau bisa
- Google Colab (free tier) cukup untuk testing

---

## 📌 Kesimpulan

✅ Advance RAG = validasi berkali-kali  
✅ 4 metode: Agentic, Adaptive, Hybrid, Self-Corrective  
✅ Agentic paling bagus tapi paling kompleks  
✅ LangGraph = bikin workflow visual  
✅ LangSmith = monitoring wajib untuk production  
✅ Tools integration = kunci advance RAG  

**Pilih Agentic kalau bingung!**

---

## ❓ FAQ

**Q: Traditional vs Advance RAG - kapan pakai mana?**  
A: Early-stage startup → Traditional. Perusahaan besar → Advance.

**Q: Kenapa grading pakai LLM sendiri?**  
A: **Self-reflection** - LLM bisa nilai diri sendiri (yes/no)

**Q: Perlu vector DB gak di Advance RAG?**  
A: Tergantung metode. Agentic & Adaptive kadang skip vector DB kalau pakai web search.

**Q: LangSmith vs Comet Opik?**  
A: LangSmith untuk LLM OPS, Comet untuk ML OPS (training model biasa).

---

**Next:** Pertemuan 10 - Fine-Tuning & Final Project
