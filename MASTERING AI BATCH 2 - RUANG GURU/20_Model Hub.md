# Model Hub dan Gradio

## Kenapa Pakai Model Hub? (Bukan Pipeline)

Model Hub memberikan **kebebasan lebih** dibanding pipeline:

### 3 Alasan Utama:

1. **Kontrol Penuh**
   - Bisa atur input/output sesukamu
   - Bisa custom processing
   - Gak terbatas sama default pipeline

2. **Model Baru Belum Support Pipeline**
   - Contoh: Microsoft Phi-2 (LLM baru)
   - Model gede (7B-70B parameter)
   - Harus pakai AutoModel

3. **Bisa Fine-Tuning**
   - Kalau mau training ulang model pakai dataset sendiri
   - Gak bisa pakai pipeline, harus load pakai AutoModel

## Load Model dengan AutoModel

### Cara Lama (Ribet)
```python
# Dulu harus spesifik
from transformers import BertModel

model = BertModel.from_pretrained("bert-base-uncased")
```

### Cara Baru (Gampang)
```python
# Sekarang pakai AutoModel - otomatis detect
from transformers import AutoModel

model = AutoModel.from_pretrained("bert-base-uncased")
```

AutoModel lebih fleksibel - otomatis detect jenis model apa yang kamu load!

### Lihat Arsitektur Model
```python
print(model)
```

Output akan show:
- Layer embedding
- Jumlah encoder (biasanya 12)
- Detail setiap layer

## Apa itu Pre-trained Model?

**Pre-trained** = Model yang udah di-training sama orang lain.

Contoh:
- Orang lain udah habis waktu berminggu-minggu training BERT
- Kamu tinggal download hasilnya
- Bisa langsung pakai atau fine-tune lagi

Hemat waktu banget!

## Tokenizer - Translator Bahasa

### Flow Lengkap Model
```
Kalimat → Tokenizer (encode) → Angka → Model → Angka → Tokenizer (decode) → Kalimat
```

### Cara Kerja Detail:

```python
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM

# Load model translation
model_name = "Helsinki-NLP/opus-mt-en-id"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForSeq2SeqLM.from_pretrained(model_name)

# 1. Kalimat input
sentence = "I am learning"

# 2. Tokenize (jadi angka)
inputs = tokenizer.encode(sentence, return_tensors="pt")
print(inputs)  
# Output: tensor([[72, 1097, 6975]])

# 3. Model proses
outputs = model.generate(inputs, max_length=50)
print(outputs)
# Output: tensor([[54795, 200, ...]])

# 4. Decode (jadi kalimat lagi)
result = tokenizer.decode(outputs[0], skip_special_tokens=True)
print(result)
# Output: "Saya sedang belajar"
```

### Multiple Input
```python
sentences = ["I am learning", "You are learning", "We are learning"]

inputs = tokenizer(sentences, return_tensors="pt", padding=True)
outputs = model.generate(**inputs)

for i, output in enumerate(outputs):
    print(tokenizer.decode(output, skip_special_tokens=True))
```

## Vocabulary Tokenizer

Angka-angka tokenizer itu dari mana? Dari **vocab.json**!

Contoh vocab GPT-2:
```json
{
  "president": 1234,
  "red": 5678,
  "draw": 9012,
  "nature": 3456,
  ...
}
```

Tokenizer harus **di-training dulu** untuk bikin vocabulary ini.

### Case Sensitive atau Tidak?

Tergantung model:
- **BERT uncased** - Semua huruf jadi lowercase (President = president)
- **BERT cased** - Huruf besar/kecil dibedakan (President ≠ president)
- **GPT-2** - Dibedakan

### Kata yang Gak Ada di Vocab?

Misal kata slang: "syapa" (anak alay dulu)

Tokenizer akan **pecah** jadi karakter yang ada di vocab:
- "s" → 123
- "y" → 456
- "a" → 789
- "p" → 012
- "a" → 789

## 3 Tipe Model Transformer

### 1. Autoregressive (Decoder-Only)
**Fungsi:** Generate text - lanjutin kalimat

**Contoh model:** GPT-2, GPT-3, GPT-4, Mistral, Llama

**Cara kerja:**
```python
generator = pipeline("text-generation", model="gpt2")
generator("Hello, I am a language model")
# Output: "Hello, I am a language model and I can help you with..."
```

Outputnya keluar **satu kata demi satu kata** (kayak ChatGPT ngetik).

### 2. Auto-encoding (Encoder-Only)
**Fungsi:** Classification & fill-in-the-blank

**Contoh model:** BERT

**Cara kerja:**
```python
# Fill mask
unmasker = pipeline("fill-mask", model="bert-base-uncased")
unmasker("Hello I am a [MASK] model")
# Output: "role", "language", "new"

# Sentiment analysis
classifier = pipeline("text-classification", model="bert-base-uncased")
classifier("Selamat pagi dunia")
```

Fokus ke **prediksi token** atau **klasifikasi**.

### 3. Sequence-to-Sequence (Encoder + Decoder)
**Fungsi:** Task yang butuh akurasi tinggi

**Contoh model:** T5, BART, Translation models (Helsinki-NLP)

**Kegunaan:**
- Translation (salah dikit fatal!)
- Summarization (konteks harus tetap)
- Question Answering

**Trade-off:**
- ✅ Lebih akurat
- ❌ Parameter lebih banyak (700-800 juta)
- ❌ Butuh resource lebih gede

Auto-encoding cuma ~200 juta parameter.

## Multimodal Models

Model yang bisa proses **multiple jenis input** (teks + gambar).

### Contoh: CLIP dari OpenAI

```python
from transformers import pipeline

# Load CLIP
classifier = pipeline("zero-shot-image-classification", 
                      model="openai/clip-vit-base-patch32")

# Input: gambar kucing
result = classifier(
    "path/to/cat.jpg",
    candidate_labels=["a photo of a cat", "a photo of a dog"]
)

print(result)
# Output:
# [
#   {'label': 'a photo of a cat', 'score': 0.95},
#   {'label': 'a photo of a dog', 'score': 0.05}
# ]
```

**Catatan penting:** 
- Label harus **kalimat lengkap**, bukan cuma "cat" atau "dog"
- Pakai format: "a photo of a ..."

## Summarization

Biasanya pakai **sequence-to-sequence** model supaya konteks gak hilang.

```python
summarizer = pipeline("summarization", model="facebook/bart-large-cnn")
long_text = """
[Teks panjang di sini...]
"""
summary = summarizer(long_text, max_length=130, min_length=30)
```

### Apa itu Konteks?

**Konteks** = Informasi yang dikasih ke model supaya bisa generate output yang sesuai.

Contoh:
- Konteks: Step-by-step cara masak mie instan
- Instruksi: "Buatkan tutorial masak mie instan"
- Model pakai konteks tersebut buat generate jawaban

## Computer Vision dengan Transformer

Transformer gak cuma buat teks! Bisa juga buat gambar.

### Vision Transformer (ViT)

```python
from transformers import AutoImageProcessor, AutoModelForImageClassification
from PIL import Image

# Load model
model_name = "google/vit-base-patch16-224"
processor = AutoImageProcessor.from_pretrained(model_name)
model = AutoModelForImageClassification.from_pretrained(model_name)

# Load gambar
image = Image.open("path/to/leaf.jpg")

# Proses gambar (jadi tensor)
inputs = processor(image, return_tensors="pt")

# Prediksi
outputs = model(**inputs)
prediction = outputs.logits.argmax(-1)
```

### Processor vs Tokenizer

- **Tokenizer** - Buat teks jadi tensor
- **Processor** - Buat gambar jadi tensor

Fungsinya sama, cuma beda jenis data!

### Tips Computer Vision:

1. **Resolusi penting** - Makin besar resolusi, makin bagus
   - Model kecil: 224x224 pixel
   - Model bagus: 400-700+ pixel

2. **Parameter model** - Makin banyak parameter, makin akurat
   - 86 juta parameter: lumayan
   - 300+ juta parameter: bagus

---

# Gradio - Bikin UI Gampang!

## Apa itu Gradio?

**Gradio** adalah library Python buat bikin interface web **tanpa coding front-end**!

Perfect buat:
- Demo model ke atasan/dosen
- Prototype cepat
- Share model ke orang lain

Kompetitor: Streamlit (tapi Gradio lebih fleksibel)

## Instalasi
```bash
pip install gradio
```

## Mulai dari yang Simpel

### Hello World
```python
import gradio as gr

def greet(name):
    return f"Hello {name}!"

# Buat interface
demo = gr.Interface(
    fn=greet,           # Function yang mau dijalanin
    inputs="text",      # Jenis input
    outputs="text"      # Jenis output
)

# Launch!
demo.launch()
```

Boom! Udah jadi web app dengan 2 line of code!

### Input Number, Output Text
```python
def plus_one(number):
    return str(number + 1)

demo = gr.Interface(
    fn=plus_one,
    inputs="number",
    outputs="text"
)

demo.launch()
```

## Jenis Input & Output

Gradio support banyak tipe:

### Input Types:
- `"text"` - Text biasa
- `"textbox"` - Text box (multi-line)
- `"number"` - Angka
- `gr.Slider()` - Slider
- `gr.Dropdown()` - Dropdown menu
- `"image"` - Upload gambar
- `"audio"` - Upload audio
- `"file"` - Upload file

### Output Types:
- `"text"` - Text
- `"label"` - Classification label
- `"image"` - Gambar
- `"json"` - JSON format
- `"audio"` - Audio

## Contoh-Contoh Keren

### 1. Slider Input
```python
def square(x):
    return x * x

demo = gr.Interface(
    fn=square,
    inputs=gr.Slider(minimum=0, maximum=100, step=1),
    outputs="number"
)

demo.launch()
```

### 2. Multiple Inputs
```python
def calculate_area(length, width):
    area = length * width
    if area > 20:
        return area, "Large area"
    else:
        return area, "Small area"

demo = gr.Interface(
    fn=calculate_area,
    inputs=[
        gr.Slider(0, 40, label="Length"),
        gr.Slider(0, 40, label="Width")
    ],
    outputs=[
        "number",  # Area
        "text"     # Description
    ]
)

demo.launch()
```

### 3. Dropdown Menu
```python
def greet_by_title(name, title):
    return f"{title} {name}, nice to meet you!"

demo = gr.Interface(
    fn=greet_by_title,
    inputs=[
        "text",
        gr.Dropdown(["Mr.", "Mrs.", "Ms.", "Dr.", "Prof."])
    ],
    outputs="text"
)

demo.launch()
```

### 4. Image Processing
```python
from PIL import Image

def make_grayscale(image):
    return image.convert("L")

demo = gr.Interface(
    fn=make_grayscale,
    inputs="image",
    outputs="image"
)

demo.launch()
```

## Tambah Title & Description

Biar gak bingung, tambahin judul dan deskripsi:

```python
demo = gr.Interface(
    fn=greet,
    inputs="text",
    outputs="text",
    title="Greeting App",
    description="Enter your name and get a friendly greeting!"
)

demo.launch()
```

## Examples - Biar User Gak Bingung

Kasih contoh input yang bagus:

```python
def text_transform(text):
    return text.upper()

demo = gr.Interface(
    fn=text_transform,
    inputs="text",
    outputs="text",
    examples=[
        ["hello world"],
        ["gradio is awesome"],
        ["machine learning"]
    ]
)

demo.launch()
```

User tinggal klik example, langsung ke-isi!

## Share ke Publik!

Ini fitur paling keren - bisa share ke internet tanpa deploy!

```python
demo.launch(share=True)
```

Akan keluar link publik kayak:
```
Running on public URL: https://1234abcd.gradio.live
```

Link ini bisa dibuka siapa aja di internet! (Aktif 72 jam)

Perfect buat:
- Demo ke atasan dari rumah
- Share model ke teman
- Dapat feedback cepat

**Note:** Gak perlu atur DNS, IP public, atau router. Gradio yang handle semuanya!

## Debug Mode

Kalau ada error tapi gak tau errornya apa:

```python
demo.launch(debug=True)
```

Bakal keluar error message lengkap di terminal.

## Sentiment Analysis dengan Gradio

```python
from transformers import pipeline

# Load model
classifier = pipeline("text-classification", 
                      model="mdhugol/indonesia-bert-sentiment-classification")

def analyze_sentiment(text):
    result = classifier(text)[0]
    return result['label'], result['score']

# Buat interface
demo = gr.Interface(
    fn=analyze_sentiment,
    inputs=gr.Textbox(lines=3, placeholder="Masukkan teks..."),
    outputs=[
        gr.Label(label="Sentiment"),
        gr.Number(label="Confidence Score")
    ],
    title="Sentiment Analyzer - Bahasa Indonesia",
    examples=[
        ["Saya sangat suka produk ini!"],
        ["Pelayanan mengecewakan"],
        ["Biasa aja"]
    ]
)

demo.launch()
```

## Image Classification dengan Label

```python
from transformers import pipeline
from PIL import Image

classifier = pipeline("image-classification", 
                      model="google/vit-base-patch16-224")

def classify_image(image):
    results = classifier(image)
    # Return top 3 predictions as dictionary
    return {result['label']: result['score'] for result in results[:3]}

demo = gr.Interface(
    fn=classify_image,
    inputs="image",
    outputs=gr.Label(num_top_classes=3),
    title="Image Classifier"
)

demo.launch()
```

## Text-to-Speech

```python
from transformers import pipeline

tts = pipeline("text-to-speech", model="facebook/fastspeech2-en-ljspeech")

def text_to_speech(text):
    audio = tts(text)
    return audio

demo = gr.Interface(
    fn=text_to_speech,
    inputs="text",
    outputs="audio"
)

demo.launch()
```

## Translation App

```python
translator = pipeline("translation_en_to_id", 
                      model="Helsinki-NLP/opus-mt-en-id")

def translate(text):
    result = translator(text)[0]
    return result['translation_text']

demo = gr.Interface(
    fn=translate,
    inputs=gr.Textbox(lines=5, label="English"),
    outputs=gr.Textbox(lines=5, label="Indonesian"),
    title="English to Indonesian Translator",
    examples=[
        ["Hello, how are you?"],
        ["I am learning machine learning"],
        ["This is an amazing tool"]
    ]
)

demo.launch()
```

## Custom Layout dengan Blocks

Kalau mau layout lebih custom, pakai **Blocks**:

```python
with gr.Blocks() as demo:
    gr.Markdown("# Welcome to My App")
    
    with gr.Row():
        input1 = gr.Textbox(label="Your Name")
        input2 = gr.Number(label="Your Age")
    
    button = gr.Button("Submit")
    output = gr.Textbox(label="Result")
    
    def process(name, age):
        if age < 18:
            return f"Hello {name}, you are young!"
        elif age > 50:
            return f"Hello {name}, you are wise!"
        else:
            return f"Hello {name}, you are in your prime!"
    
    button.click(fn=process, inputs=[input1, input2], outputs=output)

demo.launch()
```

## Load Model Langsung dari Hugging Face

Gradio punya shortcut buat load model dari Hugging Face:

```python
import gradio as gr

demo = gr.load("huggingface/facebook/fastspeech2-en-ljspeech")
demo.launch()
```

Ini otomatis bikin interface yang sesuai sama modelnya!

**Note:** Ini pakai API, bukan load model ke lokal. Butuh internet!

## Tips & Tricks

### 1. Flagging
```python
demo = gr.Interface(
    fn=your_function,
    inputs="text",
    outputs="text",
    flagging_mode="manual"  # User bisa save hasil ke CSV
)
```

User bisa flag hasil yang bagus, otomatis kesimpan ke CSV!

### 2. Queue untuk Model Berat
```python
demo.launch(share=True).queue()
```

Kalau model berat, pakai queue supaya gak overload.

### 3. Custom CSS
```python
demo = gr.Interface(
    fn=your_function,
    inputs="text",
    outputs="text",
    css=".footer {display:none}"  # Hide footer
)
```

### 4. Multiple Examples dengan Tab
```python
with gr.Blocks() as demo:
    with gr.Tab("Tab 1"):
        gr.Interface(fn=function1, inputs="text", outputs="text")
    
    with gr.Tab("Tab 2"):
        gr.Interface(fn=function2, inputs="image", outputs="text")

demo.launch()
```

## Exercise yang Seru

### 1. Mood Interpreter
Buat function yang:
- Input: kata mood ("happy", "sad", "angry")
- Output: emoji dan deskripsi

### 2. Palindrome Checker
Buat function yang cek kata palindrome:
- "ada" → "Ini palindrome!" (ada dibalik tetap ada)
- "katak" → "Ini palindrome!"
- "hello" → "Bukan palindrome"

### 3. Recipe Suggester
Input:
- Ingredients (text)
- Cuisine type (dropdown: Chinese, Italian, Mexican)
- Vegetarian (checkbox)

Output: Recipe suggestion

## Deployment ke Hugging Face Spaces

Kalau mau deploy permanen (bukan cuma 72 jam):

1. Buat account di Hugging Face
2. Bikin Space baru
3. Upload file:
   - `app.py` (code Gradio kamu)
   - `requirements.txt` (dependencies)
4. Hugging Face otomatis deploy!

Gratis dan running 24/7!

---

## Summary

### Model Hub:
- ✅ Kontrol penuh atas model
- ✅ Bisa fine-tuning
- ✅ Support model-model baru
- ❌ Lebih ribet dari pipeline

### Gradio:
- ✅ Bikin UI tanpa front-end
- ✅ Share ke publik instantly
- ✅ Perfect buat demo & prototype
- ✅ Support semua jenis input/output

### 3 Tipe Transformer:
1. **Autoregressive** (GPT) - Generate text
2. **Auto-encoding** (BERT) - Classification
3. **Seq2Seq** (T5, BART) - Translation, summarization

### Tips Penting:
- Model harus di-**pretrain** dulu (vocab.json)
- Token ≠ kata ≠ karakter
- Cek license sebelum pakai komersial
- Pakai GPU untuk speed up (device=0)
- Gradio + Hugging Face = Combo sempurna!

---

**Link Penting:**
- Hugging Face Model Hub: https://huggingface.co/models
- Hugging Face Datasets: https://huggingface.co/datasets
- Gradio Documentation: https://gradio.app/docs
- LLM Leaderboard: https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard