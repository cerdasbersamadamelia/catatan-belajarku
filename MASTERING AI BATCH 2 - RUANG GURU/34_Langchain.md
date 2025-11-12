# LangChain - Framework LLM Terkeren! 🦜⛓️

## Intro - Kenalan dengan LangChain

### Apa itu LangChain?
**LangChain = Framework untuk bikin aplikasi pakai LLM** (Large Language Models)

**Website**: https://langchain.com

### Ekosistem LangChain:
1. **LangChain** - Framework utama
2. **LangSmith** - Monitoring & debugging LLM apps
3. **LangServe** - Deploy LangChain apps

**Fun Fact**: Logo-nya burung + rantai! 🦜⛓️ (Burung bisa ngomong, rantai = chain/rangkaian)

---

## Kenapa Pakai LangChain? 🤔

### 1. **High-Level Framework**
- Nggak perlu coding low-level
- Tinggal panggil function yang udah jadi
- Lebih cepat development

### 2. **Support Banyak Provider**
- OpenAI (GPT-3.5, GPT-4)
- Google (Gemini, Vertex AI)
- Anthropic (Claude)
- HuggingFace Models
- Dan masih banyak lagi!

### 3. **Built-in Features**
- ✅ Prompt templates
- ✅ Memory management
- ✅ Agent system
- ✅ Tool integration
- ✅ Vector databases
- ✅ RAG (Retrieval Augmented Generation)

### 4. **Kemudahan Kolaborasi**
Chat LangChain: https://chat.langchain.com
- Test berbagai model
- Bandingkan performa
- Share dengan tim

---

## Dua Konsep Utama: Chain vs Agent ⚔️

### Chain - Basic Mode 🔗
**Chain = Rangkaian tasks yang fixed/tetap**

**Karakteristik**:
- Urutan eksekusi sudah ditentuin
- Step-by-step yang clear
- Seperti SOP (Standard Operating Procedure)

**Senjata Chain**:
- Prompt
- LLM
- Memory (opsional)

**Contoh**: 
```
Input → Translate → Summarize → Output
```

---

### Agent - Power Mode 🤖
**Agent = AI yang bisa mikir & pilih tools sendiri**

**Karakteristik**:
- Bisa pilih tools yang tepat
- Reasoning & action
- Adaptif sesuai pertanyaan
- Lebih powerful & flexible

**Senjata Agent**:
- Prompt
- LLM
- **Tools** (kalkulator, Wikipedia, Google Search, Python, SQL, dll)
- Memory

**Contoh**:
```
User: "Berapa 25% dari 300, lalu cari info tentang Tom Mitchell"

Agent mikir:
1. "Ini matematika" → pakai Calculator tool
2. "Ini info umum" → pakai Wikipedia tool
```

**Perbedaan Utama**:
- **Chain**: Satu jalur, ngikutin urutan
- **Agent**: Bisa cabang-cabang, pakai berbagai tools

---

## Setup LangChain 🛠️

### Install:
```bash
pip install langchain openai
```

### Setup API Key:
```python
import os
os.environ["OPENAI_API_KEY"] = "your-api-key-here"
```

**Cara Dapet API Key**:
1. Daftar di OpenAI: https://platform.openai.com
2. Bikin API key di dashboard
3. Biasanya dapet **$10 free credit** untuk user baru!

---

## Basic LangChain - 3 Komponen Wajib 🎯

### 1. Prompt Template
```python
from langchain import PromptTemplate

template = """
Based on {genre} movies and {actor}, 
I will recommend films you might like.
"""

prompt = PromptTemplate(
    input_variables=["genre", "actor"],
    template=template
)
```

### 2. Chat Model (LLM)
```python
from langchain.chat_models import ChatOpenAI

llm = ChatOpenAI(
    openai_api_key="your-key",
    model_name="gpt-3.5-turbo",
    temperature=0
)
```

### 3. Chain (Gabungan)
```python
from langchain.chains import LLMChain

chain = LLMChain(
    llm=llm,
    prompt=prompt
)

# Jalankan
result = chain.run(genre="comedy", actor="Will Ferrell")
print(result)
```

**Output**: Rekomendasi film comedy Will Ferrell!

---

## Jenis-Jenis Chain 🔗

### 1. LLMChain - Chain Paling Basic
```python
from langchain.chains import LLMChain

chain = LLMChain(llm=llm, prompt=prompt, verbose=True)
result = chain.run(product="Mini bootcamp AI Engineer online")
```

**Verbose=True** → Show logs/proses di console

---

### 2. SimpleSequentialChain - Chain Berurutan
**Konsep**: Output chain 1 → Input chain 2

```python
from langchain.chains import SimpleSequentialChain

# Chain 1: Generate nama produk
chain_1 = LLMChain(llm=llm, prompt=first_prompt)

# Chain 2: Bikin deskripsi dari nama produk
chain_2 = LLMChain(llm=llm, prompt=second_prompt)

# Gabung!
overall_chain = SimpleSequentialChain(
    chains=[chain_1, chain_2],
    verbose=True
)

result = overall_chain.run("Mini bootcamp AI Engineer")
```

**Flow**:
1. Chain 1 → Generate nama produk
2. Chain 2 → Pakai nama produk, bikin deskripsi

---

### 3. SequentialChain - Chain Bercabang
**Konsep**: Bisa multiple input/output, lebih kompleks

```python
from langchain.chains import SequentialChain

# Chain 1: Translate review ke English
chain_1 = LLMChain(
    llm=llm, 
    prompt=first_prompt,
    output_key="english_review"  # Output key!
)

# Chain 2: Summarize review
chain_2 = LLMChain(
    llm=llm,
    prompt=second_prompt,  # Input: english_review
    output_key="summary"
)

# Chain 3: Detect language (pakai input asli!)
chain_3 = LLMChain(
    llm=llm,
    prompt=third_prompt,  # Input: review (original)
    output_key="language"
)

# Chain 4: Follow-up message (pakai output chain 2 & 3)
chain_4 = LLMChain(
    llm=llm,
    prompt=fourth_prompt,  # Input: summary + language
    output_key="followup_message"
)

# Gabung semua!
overall_chain = SequentialChain(
    chains=[chain_1, chain_2, chain_3, chain_4],
    input_variables=["review"],
    output_variables=["english_review", "summary", "followup_message"]
)
```

**Flow Diagram**:
```
Input (review)
    ├─→ Chain 1 → english_review ──→ Chain 2 → summary ──┐
    └─→ Chain 3 → language ─────────────────────────────→ Chain 4 → followup_message
```

---

### 4. RouterChain - Chain Pintar Milih Jalur 🎯
**Konsep**: Kayak polisi lalu lintas yang arahkan ke expert yang tepat!

```python
from langchain.chains.router import MultiPromptChain

# Bikin prompt untuk setiap subject
physics_template = """You are a smart physics professor..."""
math_template = """You are a good mathematician..."""
history_template = """You are a good historian..."""
cs_template = """You are a successful computer scientist..."""

# Bikin destination chains
prompt_infos = [
    {
        "name": "physics",
        "description": "Good for answering questions about physics",
        "prompt_template": physics_template
    },
    {
        "name": "math",
        "description": "Good for answering math questions",
        "prompt_template": math_template
    },
    # ... dan seterusnya
]

# Bikin router chain
router_chain = MultiPromptChain(
    router_chain=router_chain,
    destination_chains=destination_chains,
    default_chain=default_chain,
    verbose=True
)
```

**Use Case**:
```python
# Pertanyaan fisika → Pakai physics expert
router_chain.run("What is black body radiation?")
# Output: "Entering Smart Physics Professor..."

# Pertanyaan math → Pakai math expert  
router_chain.run("What is 2x + 5 = 10?")
# Output: "Entering Good Mathematician..."

# Pertanyaan lain → Pakai default
router_chain.run("What is DNA?")
# Output: "Entering Default Chain..."
```

**Benefit**: Satu chatbot bisa handle berbagai topik dengan expert masing-masing!

---

## Memory - Biar AI Gak Pelupa! 🧠

### Kenapa Butuh Memory?
Tanpa memory, AI kayak ikan (lupa setiap 3 detik) 😅

**Contoh tanpa memory**:
```
User: "Nama saya John"
AI: "Halo John!"

User: "Siapa nama saya?"
AI: "Maaf, saya tidak tahu" ❌
```

**Dengan memory**:
```
User: "Nama saya John"
AI: "Halo John!"

User: "Siapa nama saya?"
AI: "Nama kamu adalah John!" ✅
```

---

### Jenis-Jenis Memory:

#### 1. ConversationBufferMemory - Simpan Semua! 💾
**Konsep**: Save semua conversation history

```python
from langchain.chains import ConversationChain
from langchain.memory import ConversationBufferMemory

memory = ConversationBufferMemory()
conversation = ConversationChain(
    llm=llm,
    memory=memory,
    verbose=True
)

conversation.predict(input="Hello, my name is John")
conversation.predict(input="What is the capital of Indonesia?")
conversation.predict(input="Do you know my name?")
# Output: "Yes, you mentioned that your name is John"
```

**Pro**: Konteks lengkap  
**Con**: Banyak token (mahal kalau conversation panjang)

---

#### 2. ConversationBufferWindowMemory - Simpan Terakhir! 🪟
**Konsep**: Cuma save K conversation terakhir

```python
from langchain.memory import ConversationBufferWindowMemory

memory = ConversationBufferWindowMemory(k=1)  # Cuma 1 pasang terakhir
conversation = ConversationChain(llm=llm, memory=memory)

conversation.predict(input="My name is John")
conversation.predict(input="What is the capital of Malaysia?")
conversation.predict(input="Do you know my name?")
# Output: "I don't have that information" ❌
# Karena cuma ingat conversation terakhir!
```

**Pro**: Hemat token  
**Con**: Bisa kehilangan konteks

**Use Case**: Conversation yang nggak perlu full history

---

#### 3. ConversationTokenBufferMemory - Limit by Token! 🎫
**Konsep**: Save conversation sampai max token

```python
from langchain.memory import ConversationTokenBufferMemory

memory = ConversationTokenBufferMemory(
    llm=llm,
    max_token_limit=50  # Max 50 token
)
```

**Pro**: Kontrol cost lebih presisi  
**Con**: Mungkin potong conversation di tengah

---

#### 4. ConversationSummaryBufferMemory - Ringkas! 📝
**Konsep**: Summarize conversation lama, save detail conversation baru

```python
from langchain.memory import ConversationSummaryBufferMemory

memory = ConversationSummaryBufferMemory(
    llm=llm,
    max_token_limit=100
)
```

**Flow**:
```
Conversation 1-5: [Di-summarize jadi 1 kalimat]
Conversation 6-10: [Disimpan lengkap]
```

**Pro**: Balance antara konteks & cost  
**Con**: Summary bisa hilangkan detail penting

---

## Agent - Mode Power! 🤖⚡

### ReAct Agent (Reason + Act)
**Konsep**: AI mikir dulu (Reason), baru action!

**Flow ReAct**:
```
1. Thought: "I need to multiply 300 by 0.25"
2. Action: Use Calculator tool
3. Observation: Result is 75
4. Thought: "I have the answer"
5. Final Answer: 75
```

### Bikin Agent:
```python
from langchain.agents import load_tools, initialize_agent, AgentType

# Load tools
tools = load_tools(
    ["llm-math", "wikipedia"],  # Calculator & Wikipedia
    llm=llm
)

# Initialize agent
agent = initialize_agent(
    tools=tools,
    llm=llm,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True
)

# Jalankan
agent.run("What is 25% of 300?")
```

**Output**:
```
Thought: This is a math problem
Action: Calculator
Action Input: 300 * 0.25
Observation: 75
Final Answer: 75
```

---

### Jenis-Jenis Tools 🛠️

#### Built-in Tools:
1. **Calculator / LLM-Math** - Kalkulasi matematika
2. **Wikipedia** - Cari info di Wikipedia
3. **Google Search** - Search di Google
4. **Python REPL** - Execute Python code
5. **SQL Database** - Query database
6. **Wolfram Alpha** - Advanced math & science

#### Custom Tools:
```python
from langchain.tools import tool
from datetime import datetime

@tool
def get_current_time():
    """Returns the current date and time"""
    return datetime.now().strftime("%Y-%m-%d %H:%M:%S")

# Tambah ke agent
tools.append(get_current_time)
```

---

### Agent dengan Multiple Tools:
```python
tools = load_tools(
    ["llm-math", "wikipedia", "python_repl"],
    llm=llm
)

agent = initialize_agent(
    tools=tools,
    llm=llm,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True
)

# Contoh 1: Math
agent.run("What is 25% of 300?")
# Uses: Calculator

# Contoh 2: Info
agent.run("Who is Tom Mitchell?")
# Uses: Wikipedia

# Contoh 3: Complex
agent.run("Calculate 10th Fibonacci number")
# Uses: Python REPL
```

**Benefit**: Agent otomatis pilih tool yang tepat! 🎯

---

## RAG - Retrieval Augmented Generation 🔍

### Apa itu RAG?
**RAG = Kasih AI akses ke knowledge base (database)**

**Tanpa RAG**:
```
User: "Siapa CEO Ruang Guru?"
AI: "Saya tidak tahu" ❌
```

**Dengan RAG**:
```
1. User tanya → System cari di database
2. Dapat info: "CEO Ruang Guru adalah Iman Usman"
3. Info dimasukkan ke prompt
4. AI jawab berdasarkan info: "CEO Ruang Guru adalah Iman Usman" ✅
```

---

### Implementasi RAG - Step by Step:

#### 1. Load Document
```python
from langchain.document_loaders import TextLoader

loader = TextLoader("alice_in_wonderland.txt")
documents = loader.load()
```

#### 2. Split Document (Chunking)
**Kenapa perlu split?**
- Document panjang = boros token
- Split jadi potongan kecil = efisien

```python
from langchain.text_splitter import RecursiveCharacterTextSplitter

text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,      # 1000 karakter per chunk
    chunk_overlap=200     # Overlap 200 karakter
)

texts = text_splitter.split_documents(documents)
```

**Kenapa pakai overlap?** 🤔
```
Chunk 1: "AI is amazing. It can help..."
Chunk 2: "...help solve problems. ML is..."
         ^^^^ Overlap

Tanpa overlap: "...help" kehilangan konteks!
Dengan overlap: Konteks tetap terjaga ✅
```

---

#### 3. Create Embeddings & Vector Store
```python
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import FAISS

# Create embeddings
embeddings = OpenAIEmbeddings()

# Create vector store
db = FAISS.from_documents(texts, embeddings)
```

**Analogi Vector Store**: 
Kayak Google untuk document kamu! Bisa cari yang relevan dengan cepat.

---

#### 4. Create Retrieval QA Chain
```python
from langchain.chains import RetrievalQA

qa_chain = RetrievalQA.from_chain_type(
    llm=llm,
    retriever=db.as_retriever(),
    chain_type="stuff"
)

# Tanya!
result = qa_chain.run("Who is the main character?")
print(result)  # "The main character is Alice"
```

**Flow RAG**:
```
Question → Similarity Search di Vector DB → 
Retrieve relevant chunks → 
Add to prompt → 
LLM generate answer
```

---

### RAG dari Website:
```python
from langchain.document_loaders import WebBaseLoader

# Load dari URL
loader = WebBaseLoader("https://example.com/article")
documents = loader.load()

# Sisa proses sama: split → embed → store → query
```

**Use Case**:
- Q&A dari dokumentasi
- Chatbot customer support
- Knowledge base internal perusahaan

---

## LangChain + HuggingFace 🤗

### Pakai Open Source Models:
```python
from langchain import HuggingFaceHub

llm = HuggingFaceHub(
    repo_id="mistralai/Mistral-7B-v0.1",
    huggingfacehub_api_token="your-token"
)

# Sisa pakai kayak biasa
chain = LLMChain(llm=llm, prompt=prompt)
```

**Benefit**:
- ✅ Gratis!
- ✅ Open source
- ✅ Privacy terjaga
- ❌ Performance mungkin kurang dari GPT-4

**Popular Models**:
- Mistral-7B
- LLaMA-2
- Falcon
- BLOOM

---

## LangSmith - Monitoring & Debugging 📊

### Apa itu LangSmith?
**LangSmith = Tools untuk monitoring LangChain apps**

**Features**:
- 🔍 Trace setiap step
- 📊 Track metrics (latency, tokens, cost)
- 🐛 Debug errors
- 👥 Collaborate with team

**Setup**:
```python
import os

os.environ["LANGCHAIN_TRACING_V2"] = "true"
os.environ["LANGCHAIN_API_KEY"] = "your-langsmith-key"
```

Otomatis tracking semua runs! ✨

---

## Docker Compose untuk Multi-Container 🐳

LangChain bisa dijalankan bareng tools lain pakai Docker Compose:

```yaml
version: '3'
services:
  rasa:
    image: rasa/rasa:latest
    ports:
      - "5005:5005"
  
  action-server:
    image: rasa/rasa-sdk:latest
    ports:
      - "5055:5055"
  
  duckling:
    image: rasa/duckling:latest
    ports:
      - "8000:8000"
```

**Jalankan**:
```bash
docker-compose up
```

Semua service jalan bareng! 🚀

---

## Best Practices LangChain ⭐

### 1. **Pilih Chain yang Tepat**
- Simple task → LLMChain
- Sequential → SequentialChain
- Multiple branches → RouterChain
- Need tools → Agent

### 2. **Memory Management**
- Short conversation → BufferMemory
- Long conversation → SummaryBufferMemory
- Cost-sensitive → TokenBufferMemory

### 3. **Agent vs Chain**
- **Chain**: Task sudah jelas & fixed
- **Agent**: Task dinamis & butuh reasoning

### 4. **RAG Implementation**
- Chunk size: 500-1500 karakter (optimal)
- Overlap: 10-20% dari chunk size
- Use metadata untuk filter

### 5. **Error Handling**
```python
from langchain.callbacks import get_openai_callback

with get_openai_callback() as cb:
    result = chain.run(input="test")
    print(f"Total tokens: {cb.total_tokens}")
    print(f"Total cost: ${cb.total_cost}")
```

Track token usage untuk kontrol cost!

---

## Use Cases - Aplikasi Nyata 🌟

### 1. Customer Support Bot
```
Router Chain → Detect topic
  ├─ Product Info → RAG dari product catalog
  ├─ Technical Issue → RAG dari troubleshooting docs
  └─ General → Default chain
```

### 2. Code Assistant
```
Agent dengan tools:
  ├─ Python REPL (execute code)
  ├─ Documentation search (RAG)
  └─ Stack Overflow search
```

### 3. Research Assistant
```
Agent dengan tools:
  ├─ Wikipedia
  ├─ Google Scholar
  ├─ ArXiv search
  └─ Summarizer chain
```

### 4. Data Analysis Bot
```
Agent dengan tools:
  ├─ SQL Database
  ├─ Python REPL (pandas)
  └─ Visualization tools
```

---

## Common Errors & Solutions 🔧

### 1. "Rate limit exceeded"
**Solusi**: 
- Tambah delay antar request
- Pakai model yang lebih murah (GPT-3.5 vs GPT-4)
- Cache responses

### 2. "Context length exceeded"
**Solusi**:
- Perkecil chunk size
- Pakai memory dengan window/summary
- Reduce verbose logs

### 3. "Tool not found"
**Solusi**:
```python
# Cek available tools
from langchain.agents import load_tools
print(load_tools.__doc__)
```

### 4. "API key not found"
**Solusi**:
```python
import os
from dotenv import load_dotenv

load_dotenv()  # Load dari .env file
```

---

## Comparison: LangChain vs Alternatives 📊

### LangChain vs LlamaIndex
- **LangChain**: General-purpose, banyak features
- **LlamaIndex**: Fokus ke RAG & indexing

### LangChain vs Haystack
- **LangChain**: Python-centric, easy setup
- **Haystack**: Production-ready, lebih mature

### LangChain vs Custom Code
- **LangChain**: Fast development, high-level
- **Custom**: Full control, tapi lambat

**Kesimpulan**: LangChain terbaik untuk rapid prototyping! 🚀

---

## Future of LangChain 🔮

### Trend 2024-2025:
1. **Multi-modal**: Text + Image + Audio
2. **Autonomous agents**: AI yang kerja sendiri
3. **Better RAG**: Hybrid search, re-ranking
4. **Cost optimization**: Smart caching, model routing
5. **Enterprise features**: Security, compliance

**Skills yang Perlu Dikuasai**:
- Prompt engineering (masih penting!)
- Vector databases (Pinecone, Weaviate, FAISS)
- Agent orchestration
- RAG optimization

---

## Resources & Next Steps 📚

### Official Resources:
- 🌐 Docs: https://python.langchain.com
- 💬 Chat: https://chat.langchain.com
- 📺 YouTube: LangChain official channel
- 💬 Discord: LangChain community

### Practice Projects:
1. **Beginner**: Simple Q&A bot with memory
2. **Intermediate**: RAG chatbot untuk dokumentasi
3. **Advanced**: Multi-agent system dengan tools

### Learning Path:
```
Week 1: Basic chains + prompt templates
Week 2: Memory + sequential chains
Week 3: Agents + tools
Week 4: RAG implementation
Week 5: Production deployment
```

---

## Summary - Hal Penting! 📝

### Core Concepts:
1. **Chain**: Fixed sequence of tasks
   - LLMChain → Basic
   - SequentialChain → Multi-step
   - RouterChain → Dynamic routing

2. **Agent**: Dynamic, bisa pilih tools
   - ReAct → Reasoning + Acting
   - Tools → Calculator, Wikipedia, Python, dll

3. **Memory**: Biar AI gak pelupa
   - BufferMemory → Semua
   - WindowMemory → N terakhir
   - SummaryMemory → Diringkas

4. **RAG**: Kasih AI knowledge base
   - Load → Split → Embed → Store → Retrieve

### Key Takeaways:
- ✅ LangChain = High-level framework untuk LLM apps
- ✅ Chain untuk tasks yang fixed
- ✅ Agent untuk tasks yang butuh reasoning
- ✅ Memory untuk context management
- ✅ RAG untuk akses ke knowledge base
- ✅ Bisa pakai berbagai LLM (OpenAI, HuggingFace, dll)

### Tools Wajib:
- **LangChain**: Framework
- **LangSmith**: Monitoring
- **Vector DB**: FAISS, Pinecone
- **LLM**: OpenAI, HuggingFace

---

## Final Tips 💡

1. **Start Simple**: Mulai dari LLMChain dulu
2. **Add Memory**: Buat conversation lebih natural
3. **Try RAG**: Kasih AI knowledge domain-specific
4. **Use Agents**: Untuk tasks kompleks
5. **Monitor Cost**: Track token usage!
6. **Test Everything**: Coba berbagai prompt
7. **Read Docs**: LangChain docs sangat lengkap
8. **Join Community**: Discord LangChain aktif banget

**Remember**: LangChain is just a tool. Prompt engineering masih penting! 🎯

---

**Happy Building! 🚀🦜⛓️**

---

## Bonus: Quick Reference Card 📇

### Imports Penting:
```python
# Basic
from langchain.chat_models import ChatOpenAI
from langchain import PromptTemplate
from langchain.chains import LLMChain

# Sequential
from langchain.chains import SimpleSequentialChain, SequentialChain

# Memory
from langchain.memory import ConversationBufferMemory

# Agent
from langchain.agents import load_tools, initialize_agent, AgentType

# RAG
from langchain.document_loaders import TextLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import FAISS
from langchain.chains import RetrievalQA
```

### Template Dasar:
```python
# Setup
llm = ChatOpenAI(openai_api_key="key", model_name="gpt-3.5-turbo")
prompt = PromptTemplate(input_variables=["input"], template="...")

# Chain
chain = LLMChain(llm=llm, prompt=prompt)
result = chain.run(input="test")

# Agent
tools = load_tools(["llm-math", "wikipedia"], llm=llm)
agent = initialize_agent(tools, llm, AgentType.ZERO_SHOT_REACT_DESCRIPTION)
result = agent.run("question")

# RAG
loader = TextLoader("file.txt")
docs = loader.load()
texts = text_splitter.split_documents(docs)
db = FAISS.from_documents(texts, embeddings)
qa = RetrievalQA.from_chain_type(llm=llm, retriever=db.as_retriever())
result = qa.run("question")
```

**Keep this handy untuk development! 📌**
