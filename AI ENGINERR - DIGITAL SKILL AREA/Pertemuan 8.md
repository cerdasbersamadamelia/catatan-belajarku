# Pertemuan 8: LangChain Agents (Python & SQL)

## 🎯 Overview
Materi hari ini **gak banyak**, tapi penting! Kita bahas LangChain Agent - tools yang bisa **eksekusi code** sendiri!

---

## 🤖 Apa itu LangChain Agent?

### Definisi Simpel
Agent = **LLM yang bisa jalanin kode sendiri!**

**Beda sama RAG:**
- RAG → analisis dokumen
- Agent → eksekusi code (Python, SQL, dll)

### Bahasa yang Didukung
✅ **Python** - Data analysis, visualization  
✅ **SQL** - Database query  
❌ JavaScript, PHP, HTML, CSS - **BELUM** tersedia

---

## 🔄 Cara Kerja Agent

### Workflow Diagram

```
1. User Input (data + pertanyaan)
        ↓
2. Model LLM (Gemini/Claude/ChatGPT)
        ↓
3. LLM bikin code
        ↓
4. Tools (eksekusi code)
        ↓
5. Feedback (hasil/error)
        ↓
6. LLM evaluasi → Kalau salah, ulangi!
        ↓
7. Output (hasil akhir)
```

### Detail Proses

#### **Step 1-2: Input → LLM**
User kasih:
- Data (CSV, database, dll)
- Pertanyaan/instruksi

#### **Step 3: LLM Generate Code**
LLM bikin code otomatis:
```python
# Contoh: LLM bikin code sendiri
import pandas as pd
df = pd.read_csv('data.csv')
result = df['price'].mean()
```

#### **Step 4: Tools Execute**
Code dijalankan di **environment khusus** (bukan terminal biasa!)

#### **Step 5: Feedback Loop** 🔄
- Kalau **sukses** → kasih hasil ke LLM
- Kalau **error** → LLM perbaiki code
- Loop sampai bener atau give up

#### **Step 6-7: Output**
- ✅ Berhasil → tampilkan hasil
- ❌ Gagal terus → "Sorry, I cannot answer this"

---

## 📊 SQL Agent

### Setup
```bash
pip install langchain
pip install langchain-google-genai
pip install langchain-community
pip install langchain-experimental  # WAJIB!
pip install sqlalchemy  # Database connector
```

### Use Case
- ✅ Query database otomatis
- ✅ Analisis data dari SQL
- ✅ Bikin laporan
- ❌ Summarize dokumen (pakai RAG!)

### Code Implementasi

```python
from langchain_experimental.agents import create_sql_agent
from langchain_community.utilities import SQLDatabase
from langchain_google_genai import ChatGoogleGenerativeAI

# 1. Setup LLM
llm = ChatGoogleGenerativeAI(
    model="gemini-pro",
    google_api_key="YOUR_API_KEY"
)

# 2. Connect ke Database
# Format URI: dialect://user:password@host:port/database
db_uri = "sqlite:///chinook.db"  # Contoh SQLite lokal
db = SQLDatabase.from_uri(db_uri)

# 3. Bikin Agent
agent = create_sql_agent(
    llm=llm,
    db=db,
    agent_type="zero-shot-react-description",  # Jawaban singkat
    verbose=True  # Lihat proses
)

# 4. Tanya!
query = "List all tracks by duration in descending order"
result = agent.invoke(query)
print(result)
```

### Cara Kerja SQL Agent

**Proses Internal:**
```
1. User: "List all tracks by duration..."
   ↓
2. LLM mikir: "Butuh CREATE TABLE dulu"
   → CREATE TABLE tracks (...)
   ↓
3. LLM mikir: "Sekarang query"
   → SELECT * FROM tracks ORDER BY duration DESC
   ↓
4. Tools eksekusi
   ↓
5. Hasil: [Track1, Track2, Track3...]
   ↓
6. LLM format: "Here are the tracks sorted by duration..."
```

### Tips Database

**SQLite (lokal):**
```python
db_uri = "sqlite:///database.db"
```

**PostgreSQL (production):**
```python
# Format: postgresql://user:password@host:port/dbname
db_uri = "postgresql://admin:pass123@localhost:5432/mydb"
```

**MySQL:**
```python
db_uri = "mysql://user:password@localhost:3306/mydb"
```

**Cloud (Google Cloud SQL):**
```python
# Pakai IP address cloud instance
db_uri = "postgresql://user:pass@35.240.xxx.xxx:5432/proddb"
```

---

## 🐍 Python Agent

### Setup
```bash
# Sama seperti SQL Agent +
pip install matplotlib  # Untuk visualisasi
pip install seaborn
```

### Use Case
- ✅ Analisis data kompleks
- ✅ Visualisasi (chart, plot)
- ✅ Statistical analysis
- ❌ Query database (pakai SQL Agent!)

### Code Implementasi

```python
from langchain_experimental.agents import create_python_agent
from langchain_experimental.tools import PythonREPLTool
from langchain_google_genai import ChatGoogleGenerativeAI
import pandas as pd

# 1. Setup LLM
llm = ChatGoogleGenerativeAI(model="gemini-pro")

# 2. Load Data
df = pd.read_csv("iris.csv")

# 3. Bikin Agent
agent = create_python_agent(
    llm=llm,
    tool=PythonREPLTool(),
    verbose=True
)

# 4. Tanya!
query = "Plot the data as a bar chart"
result = agent.invoke({
    "input": query,
    "dataframe": df
})
```

### Contoh Advanced: Neural Network

```python
query = """
Build a simple neural network using PyTorch with:
- 1000 neurons
- 100 epochs
- Train on the given dataset
"""

result = agent.invoke(query)
```

**Proses Internal:**
```
1. LLM mikir strategi
   "I need to create a simple neural network..."
   ↓
2. LLM bikin code PyTorch
   import torch
   import torch.nn as nn
   ...
   ↓
3. Tools eksekusi
   ↓
4. Error? Loss = infinity
   ↓
5. LLM revisi code
   ↓
6. Eksekusi lagi
   ↓
7. Sukses! Final Loss = 0.0001
   ↓
8. Output: "Training completed. Final loss: 0.0001"
```

---

## 🎨 Visualisasi dengan Python Agent

### Contoh Data Analysis

```python
# Load dataset
df = pd.read_csv("sales.csv")

# Tanya kompleks
query = """
1. Calculate correlation between price and quantity
2. Create a correlation heatmap
3. Show top 5 products by revenue
"""

result = agent.invoke(query)
```

**Agent akan:**
1. Bikin correlation matrix
2. Generate heatmap pakai matplotlib/seaborn
3. Sort data & tampilkan top 5

---

## ⚙️ Prompt Template

### Untuk SQL Agent
```python
from langchain.prompts import PromptTemplate

template = """
You are a SQL expert. Given the question: {query}
1. Analyze the database schema
2. Write an optimized SQL query
3. Execute and return results
"""

prompt = PromptTemplate(template=template, input_variables=["query"])
```

### Untuk Python Agent
```python
template = """
You are a Python data analyst. Given:
- DataFrame: {df}
- Question: {query}

Provide:
1. Data analysis code
2. Visualization if needed
3. Clear summary

Note: Query must be in {query} format
"""
```

**⚠️ JANGAN ubah `{query}` dan `{df}` - wajib ada!**

---

## 🆚 Kapan Pakai Apa?

| Task | Pakai Apa? | Alasan |
|------|------------|--------|
| Query database | **SQL Agent** | Langsung akses DB |
| Analisis CSV | **Python Agent** | Lebih fleksibel |
| Visualisasi data | **Python Agent** | Matplotlib/Seaborn |
| Summarize dokumen | **RAG** | Bukan tugas Agent! |
| Statistik kompleks | **Python Agent** | Statistical libraries |
| Join multiple tables | **SQL Agent** | Optimized untuk DB |

---

## 💡 Tips & Best Practices

### 1. Model LLM
- **Gemini** - OK, tapi visualisasi kurang bagus
- **Claude** - Terbaik untuk coding!
- **ChatGPT** - Jawaban terlalu pendek

### 2. Error Handling
Agent akan **retry otomatis** kalau error, tapi ada limit:
- Max iterations: 5-10
- Kalau tetap error → "I cannot answer"

### 3. Virtual Environment
**WAJIB pakai venv!**
```bash
python -m venv env
env\Scripts\activate  # Windows
pip install -r requirements.txt
```

Kenapa? Biar gak konflik library!

### 4. Debugging
```python
agent = create_sql_agent(
    llm=llm,
    db=db,
    verbose=True  # Lihat setiap step!
)
```

---

## 🚫 Limitasi Agent

### Yang TIDAK Bisa:
❌ Generate HTML/CSS/JavaScript  
❌ Akses real-time API  
❌ Multi-language dalam 1 agent  
❌ File system operation (belum)  

### Workaround
- Pakai **custom tools** (advance)
- Kombinasi dengan **FastAPI**
- Manual preprocessing

---

## 🔥 Real-World Example

### Use Case: Fraud Detection Dashboard

```python
# 1. Load transaction data
df = pd.read_csv("transactions.csv")

# 2. Agent setup
agent = create_python_agent(llm, PythonREPLTool())

# 3. Kompleks query
query = """
Analyze the transaction data:
1. Find transactions > $10,000
2. Calculate fraud probability
3. Create visualization
4. Generate summary report
"""

result = agent.invoke({"input": query, "df": df})
```

**Output:**
- Fraud list
- Chart visualisasi
- Statistical summary

---

## 📌 Kesimpulan

✅ Agent = LLM yang bisa eksekusi code  
✅ SQL Agent untuk database  
✅ Python Agent untuk data analysis  
✅ Pakai Claude untuk hasil terbaik  
✅ Virtual environment = WAJIB!  
✅ Gak cocok untuk summarization (pakai RAG)  

---

## ❓ FAQ

**Q: Bisa pakai database perusahaan?**  
A: Bisa! Tapi **jangan pakai data asli** untuk testing (takut masuk training LLM)

**Q: Agent vs RAG - mana lebih bagus?**  
A: Beda use case! Agent untuk code, RAG untuk dokumen.

**Q: Kenapa visualisasi Gemini jelek?**  
A: Emang begitu. Pakai Nvidia Llama atau Claude lebih bagus.

**Q: FastAPI buat apa?**  
A: Backend untuk deploy aplikasi AI ke production (POST/GET requests)

---

**Next:** Pertemuan 9 - Advance RAG (yang susah!)
