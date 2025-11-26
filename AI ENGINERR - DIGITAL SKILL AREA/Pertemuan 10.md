# Pertemuan 10: LLM Fine-Tuning & Final Project

## 🎯 Overview
**Pertemuan terakhir!** Materi gak banyak - fine-tuning LLM + advance prompting + briefing tugas akhir.

---

## 🔧 LLM Fine-Tuning

### Apa itu Fine-Tuning?

**Melatih ulang model LLM untuk tugas spesifik!**

**Bedanya sama RAG:**
- RAG → kasih dokumen sebagai context
- Fine-tuning → **training ulang** dengan data kita

### Kapan Perlu Fine-Tuning?

✅ **Perlu:**
- Deteksi fraud berdasarkan teks
- Klasifikasi email spam/not spam
- Custom caption generation
- Bahasa/domain spesifik (hukum, medis)

❌ **Gak Perlu:**
- Tanya-jawab dokumen → pakai RAG!
- Analisis data → pakai Agent!
- Use case umum → langsung pakai API

**Intinya:** Fine-tuning untuk **task classification/generation yang sangat spesifik**

---

## 🎨 2 Metode Fine-Tuning

### 1️⃣ LoRA (Low-Rank Adaptation)

**Spek:**
- Model size: **16-bit**
- RAM needed: ~**14 GB**
- Speed: Lambat
- Akurasi: Paling tinggi

**Kapan pakai?**
- PC/laptop spec tinggi
- Ada GPU bagus (RTX 3060+)
- Butuh akurasi maksimal
- Pakai Google Colab (T4 GPU)

### 2️⃣ QLoRA (Quantized LoRA)

**Spek:**
- Model size: **4-bit** (dikurangi!)
- RAM needed: ~**4 GB**
- Speed: Lebih cepat
- Akurasi: Sedikit di bawah LoRA

**Kapan pakai?**
- PC/laptop spec standar
- Gak ada GPU kuat
- Butuh training cepat
- Lokal di laptop

### Perbandingan

| Aspek | LoRA | QLoRA |
|-------|------|-------|
| **Model Size** | 16-bit | 4-bit |
| **RAM** | 14 GB | 4 GB |
| **Akurasi** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Speed** | 🐌 | 🏃 |
| **Use Case** | Production | Development/Testing |

**Hasil:** Bedanya **gak jauh-jauh amat!** QLoRA cukup bagus.

---

## 🛠️ Tools: Unsloth

### Kenapa Unsloth?

✅ Fine-tuning **3x lebih cepat**  
✅ Dokumentasi jelas  
✅ Support LoRA & QLoRA  
✅ Integrasi dengan HuggingFace  

**Alternatif:** HuggingFace Transformers (lebih lambat)

### Model yang Didukung

**Recommended:**
- ⭐ **Llama 3.3 70B** (paling bagus!)
- ⭐ **Llama 3.2 32B**
- Mistral 7B
- Gemma 2 9B (kurang bagus)
- Qwen 2.5 (avoid!)

**Tips:** Pilih Llama series - paling reliable!

---

## 👨‍💻 Implementasi LoRA

### Setup
```bash
pip install unsloth
pip install torch
pip install transformers
pip install datasets
```

### 1. Load Model
```python
from unsloth import FastLanguageModel

model, tokenizer = FastLanguageModel.from_pretrained(
    model_name="unsloth/llama-3-8b-Instruct",
    max_seq_length=2048,
    dtype=None,  # Auto-detect
    load_in_4bit=False  # LoRA = False, QLoRA = True
)
```

### 2. Prepare Dataset

**Format JSON:**
```json
[
  {
    "user": "This user has transferred $15 million",
    "assistant": "fraud"
  },
  {
    "user": "User bought coffee for $5",
    "assistant": "not fraud"
  },
  {
    "user": "Multiple transactions from different countries in 1 hour",
    "assistant": "fraud"
  }
]
```

**Load Dataset:**
```python
from datasets import load_dataset

dataset = load_dataset("json", data_files="fraud_data.json")
```

### 3. Apply LoRA
```python
from peft import LoraConfig, get_peft_model

lora_config = LoraConfig(
    r=16,  # Rank
    lora_alpha=32,
    target_modules=["q_proj", "v_proj"],
    lora_dropout=0.1,
    bias="none"
)

model = get_peft_model(model, lora_config)
```

### 4. Training
```python
from transformers import TrainingArguments, Trainer

training_args = TrainingArguments(
    output_dir="./results",
    num_train_epochs=3,
    per_device_train_batch_size=4,
    learning_rate=2e-4,  # PENTING!
    max_steps=60,  # Kayak epochs di deep learning
    logging_steps=10,
    save_steps=20
)

trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=dataset["train"]
)

# Train!
trainer.train()
```

**Training Loss:**
```
Step 10: Loss = 0.5432
Step 20: Loss = 0.2341
Step 30: Loss = 0.1234
...
Step 60: Loss = 0.0010  ← BAGUS!
```

**Target:** Loss < 0.01 = siap dipakai!

### 5. Inference (Test)
```python
# Test model
prompt = "User transferred $1 million at 3 AM"
inputs = tokenizer(prompt, return_tensors="pt")
outputs = model.generate(**inputs, max_new_tokens=50, temperature=0.7)
result = tokenizer.decode(outputs[0])

print(result)  # Output: "fraud"
```

### 6. Save Model
```python
# Save
model.save_pretrained("fraud_detection_model")
tokenizer.save_pretrained("fraud_detection_model")

# Load nanti
from unsloth import FastLanguageModel
model, tokenizer = FastLanguageModel.from_pretrained("fraud_detection_model")
```

---

## 👨‍💻 Implementasi QLoRA

### Bedanya?

**Cuma beda di 1 parameter:**

```python
# LoRA
model, tokenizer = FastLanguageModel.from_pretrained(
    model_name="unsloth/llama-3-8b-Instruct",
    load_in_4bit=False  # LoRA
)

# QLoRA
model, tokenizer = FastLanguageModel.from_pretrained(
    model_name="unsloth/llama-3-8b-Instruct-bnb-4bit",  # Pakai model 4-bit
    load_in_4bit=True  # QLoRA!
)
```

**Sisanya sama persis!**

### Memory Usage

**LoRA:**
```
GPU Memory Used: 12.4 GB
Training Time: 45 minutes
```

**QLoRA:**
```
GPU Memory Used: 3.2 GB  ← Hemat banget!
Training Time: 30 minutes  ← Lebih cepat!
```

**Final Loss:** Keduanya sama (0.0010)

---

## 📊 Hyperparameters Penting

### Learning Rate
```python
learning_rate=2e-4  # Default
```
- **Terlalu tinggi:** Model gak konvergen
- **Terlalu rendah:** Training kelamaan
- **Sweet spot:** 1e-4 sampai 3e-4

### Max Steps
```python
max_steps=60  # Kayak epochs
```
- Makin banyak = makin bagus (tapi bisa overfit!)
- Monitor training loss!

### Temperature (Inference)
```python
temperature=0.7
```
- **0.1-0.5:** Jawaban konservatif & konsisten
- **0.7-0.9:** Lebih kreatif
- **1.0+:** Random banget

### Max New Tokens
```python
max_new_tokens=50
```
Panjang output yang dihasilkan

---

## 🎯 Advance Prompting

### 1️⃣ Chain-of-Thought (CoT)

**Konsep:** Suruh LLM berpikir **step-by-step**

**Contoh:**
```
Prompt: "Cafetaria had 23 apples. They used 20 for lunch. 
Then bought 6 more. How many apples now?"

Output dengan CoT:
"Step 1: 23 - 20 = 3 apples left
Step 2: 3 + 6 = 9 apples
Answer: 9 apples"
```

**Code:**
```python
from langchain.prompts import PromptTemplate

template = """
Think step-by-step to solve this:
{question}

Let's break it down:
"""

prompt = PromptTemplate(template=template, input_variables=["question"])
```

### 2️⃣ Tree-of-Thought (ToT)

**Konsep:** Bikin **beberapa hipotesis**, pilih yang terbaik

**Contoh:**
```
Question: "Best way to reduce carbon emissions?"

Approach 1: Renewable energy (solar, wind)
Approach 2: Carbon capture technology
Approach 3: Reduce consumption

Evaluation: Approach 1 is most practical and scalable.
Final Answer: Focus on renewable energy adoption.
```

**Code:**
```python
template = """
Generate 3 different approaches for: {question}
Evaluate each approach.
Choose the best one.
"""
```

### 3️⃣ ReAct (Reasoning + Acting)

**Konsep:** LLM **belajar → alasan → action** (iteratif)

```
Step 1: Learn from data
Step 2: Reason (buat hipotesis)
Step 3: Act (test)
Step 4: If wrong, loop back to Step 1
```

**Kurang recommended** - hasilnya agak inkonsisten

### 4️⃣ Self-Consistency

**Konsep:** Generate **beberapa jawaban**, pilih yang **paling sering muncul**

**Contoh:**
```
Question: "How much is 15% of $120?"

Run 1: $18
Run 2: $18
Run 3: $26

Most common: $18 ← Pilih ini!
```

**Code:**
```python
results = []
for i in range(5):
    output = llm.invoke(prompt)
    results.append(output)

# Count most common
from collections import Counter
final_answer = Counter(results).most_common(1)[0][0]
```

---

## 📝 Final Project - Briefing

### Format Pengumpulan
- **PPT atau PDF**
- Screenshot code + hasil
- Boleh post di LinkedIn (tag instruktur!)

### Pilihan Project

#### **1. Advance RAG** 🔥
Bikin chatbot pakai salah satu metode:
- Agentic RAG
- Adaptive RAG
- Hybrid RAG
- Self-Corrective RAG

**Dataset:** Bebas (PDF, Word, RUU, research paper)

#### **2. SQL Agent** 💾
Chatbot untuk query database otomatis

**Dataset:** 
- SQLite lokal
- PostgreSQL
- Dummy data perusahaan

#### **3. Python Agent** 🐍
Analisis data + visualisasi otomatis

**Dataset:**
- CSV (sales, transactions, etc)
- Kaggle datasets

#### **4. Computer Vision** 👁️
Object detection / image classification

**Tools:** YOLO
**Contoh:** Deteksi plat nomor, face recognition

#### **5. Sentiment Analysis** 📊
Klasifikasi sentimen (positif/negatif/netral)

**Dataset:** 
- Indonesian sentiment dataset (HuggingFace)
- Twitter/reviews

#### **6. Research Plan** 📚
Kalau mau lanjut S2/skripsi

**Isi:**
- Research question
- Methodology
- Expected outcome

### Tips Dataset

**Jangan pakai data asli perusahaan!** (takut masuk training LLM)

**Sumber Dataset:**
- 🌐 **HuggingFace Datasets**
- 📊 **Kaggle**
- 🏛️ **Situs pemerintah** (DPR untuk RUU)
- 📰 **News websites**
- 📚 **ArXiv** (research papers)

### Deadline
**1 bulan** (karena pada sibuk kerja/kuliah)

---

## 🚀 Skill Tambahan (Bonus!)

### Docker

**Kenapa penting?**
- Deploy ke cloud (AWS, Google Cloud, Azure)
- Wajib untuk production

**Analogi Burger:**
```
Docker Container = Kotak burger kosong

Isinya:
🍔 main.py
🧅 requirements.txt  
🥬 model.pkl
🍅 config.json

→ Jadi 1 paket lengkap!
```

**Dockerfile Example:**
```dockerfile
FROM python:3.10

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY main.py .

EXPOSE 8000

CMD ["python", "main.py"]
```

### FastAPI

**Backend untuk AI apps!**

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Query(BaseModel):
    question: str

@app.post("/ask")
def ask(query: Query):
    # LLM processing here
    answer = llm.invoke(query.question)
    return {"answer": answer}

# Run: uvicorn main:app --reload
```

**Kenapa FastAPI?**
- ✅ Modern & cepat
- ✅ Auto documentation
- ✅ Type hints
- ✅ Async support

### Streamlit

**Simple UI untuk demo:**
```python
import streamlit as st

st.title("AI Chatbot")

query = st.text_input("Ask something:")
if st.button("Submit"):
    answer = llm.invoke(query)
    st.write(answer)

# Run: streamlit run app.py
```

**Frontend Options:**
1. **Streamlit** - Paling gampang (Python only)
2. **HTML/CSS/JS** - Custom UI
3. **React/Next.js** - Professional (advance)

---

## 💼 Tips Karir AI Engineer

### Skill Priority

**Must Have:**
1. ✅ LangChain + RAG
2. ✅ Python (Pandas, NumPy)
3. ✅ SQL
4. ✅ FastAPI / Backend basics

**Good to Have:**
1. Docker
2. Cloud (AWS/GCP/Azure)
3. Computer Vision / NLP
4. Fine-tuning

**Nice to Have:**
1. React/Next.js
2. LLM OPS (LangSmith)
3. Kubernetes

### Interview Questions (Sering Ditanya!)

1. **"Pernah fine-tuning LLM?"**
   → Jawab: "Yes, pakai LoRA/QLoRA dengan Unsloth"

2. **"Jelaskan perbedaan Encoder vs Decoder"**
   → (Lihat Pertemuan 6!)

3. **"Kenapa pakai RAG, bukan langsung LLM?"**
   → Hemat token, lebih akurat, lebih murah!

4. **"Perbedaan FAISS vs ChromaDB?"**
   → FAISS lokal & simpel, Chroma lebih canggih

---

## 📌 Kesimpulan Bootcamp

### What We Learned

**Pertemuan 1-5:** (sebelum ini)
- Python, Statistics, Pandas, Visualization, Machine Learning

**Pertemuan 6:** Generative AI basics

**Pertemuan 7:** Traditional RAG + LangChain

**Pertemuan 8:** LLM Agents (SQL & Python)

**Pertemuan 9:** Advance RAG (4 metode) + LLM OPS

**Pertemuan 10:** Fine-tuning + Advance Prompting

### Next Steps

1. **Kerjain final project**
2. **Post di LinkedIn** (biar recruiter lihat!)
3. **Belajar Docker & FastAPI**
4. **Explore Google Colab & Kaggle**
5. **Join komunitas AI** (Discord, Slack)

### Resources

**Belajar Lanjutan:**
- 📚 **ArXiv Scholar** - Research papers
- 🎓 **Google Colab** - Free GPU
- 📊 **Kaggle** - Datasets & competitions
- 🤗 **HuggingFace** - Models & datasets
- 📖 **LangChain Docs** - Official documentation

**Komunitas:**
- Discord: LangChain, HuggingFace
- LinkedIn: Follow AI thought leaders
- GitHub: Contribute to open source

---

## ❓ FAQ

**Q: Fine-tuning wajib gak?**  
A: Gak wajib! Tergantung use case. Zero-shot classification sering cukup.

**Q: Model LLM mana yang paling bagus?**  
A: Claude (coding), Gemini (analisis), ChatGPT (general)

**Q: Advance RAG susah banget, harus paham semua?**  
A: Cukup 1 metode (Agentic) + Traditional RAG. Yang lain optional.

**Q: Perlu S2 gak untuk jadi AI Engineer?**  
A: Gak wajib! Portfolio > gelar. Tapi S2 bagus untuk research/academic.

**Q: Docker itu susah ya?**  
A: Install-nya susah (butuh WSL di Windows). Pakainya gampang!

---

## 🎓 Certificate & Next Level

### Setelah Bootcamp

**Apa yang harus dikuasai:**
✅ RAG (Traditional & 1 Advance method)  
✅ LangChain basics  
✅ SQL/Python Agent  
✅ Fine-tuning (minimal tahu konsepnya)  
✅ 1 complete project di portfolio  

**Level selanjutnya:**
- MLOps (Deployment production)
- Multi-agent systems
- Custom tools & integrations
- Real-time LLM apps
- Enterprise-scale RAG

### Thank You! 🙏

**Kontak:**
- LinkedIn: (tag di post final project!)
- WhatsApp: 0897-4200-xxx

**Good luck dengan final project!** 🚀

---

**End of Bootcamp!** 🎉
