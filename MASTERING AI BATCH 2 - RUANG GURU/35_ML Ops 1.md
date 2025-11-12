# ML Ops 1 - Docker (Komputer dalam Komputer!) 🐳

## Apa itu Docker? 🤔

### Definisi Sederhana:
**Docker = Virtual computer dalam computer kamu**

Bayangkan kamu punya komputer Windows, terus kamu mau jalanin program yang butuh Linux. Nah, Docker bisa bikin "komputer Linux virtual" di dalam Windows kamu!

### Analogi Gampang:
Docker itu kayak **container pengiriman barang**:
- Isinya aplikasi + semua yang dibutuhkan
- Bisa dikirim ke mana aja (laptop, server, cloud)
- Isi tetap aman & konsisten

---

## Masalah yang Dipecahkan Docker 🎯

### Problem Klasik:
```
Developer A: "Di laptop saya jalan kok! ✅"
Developer B: "Di laptop saya error! ❌"
```

**Kenapa bisa beda?**
- OS berbeda (Windows, Mac, Linux)
- Python version beda (3.6 vs 3.12)
- Library version beda (TensorFlow 2.1 vs 2.15)

### Solusi Docker:
```
"Di Docker container saya jalan! ✅"
"Di Docker kamu juga jalan! ✅"
```

**Semua jalan karena environment-nya sama!** 🎉

---

## Keuntungan Pakai Docker 🚀

### 1. Portability (Bisa Pindah-Pindah) 📦
- Deploy di mana aja: Laptop, Server AWS, GCP, Azure
- Cross-platform: Windows, Mac, Linux
- Konsisten di semua tempat

### 2. Versioning (Kontrol Versi) 🏷️
```
Version 1: Model v1 + Python 3.8 + TF 2.1
Version 2: Model v2 + Python 3.9 + TF 2.5
Version 3: Model v3 + Python 3.12 + TF 2.15
```

**Error di v3? Tinggal balik ke v2!** Super gampang!

### 3. Isolation (Pemisahan Environment) 🔒
```
Project A: Python 3.6 + TensorFlow 2.1 (old but stable)
Project B: Python 3.12 + TensorFlow 2.15 (new features)
```

**Keduanya bisa jalan bersamaan tanpa konflik!**

### 4. Lightweight (Enteng) 🪶
- Lebih ringan dari Virtual Machine
- Sharing Linux kernel → hemat resource
- Start/stop cepat (seconds, bukan minutes)

### 5. Easy to Scale (Gampang Di-scale) 📈
**Contoh Kasus**:
```
Komputer: 4 CPU, 8GB RAM
Satu app butuh: 0.5 CPU, 500MB RAM

Bisa jalan berapa app? 
CPU: 4 / 0.5 = 8 apps
RAM: 8GB / 500MB = 16 apps

Minimal bisa 8 apps! (daripada beli komputer baru)
```

---

## Terminologi Docker 📚

### 1. Docker Image 📦
**Image = Blueprint/template aplikasi**

**Isinya**:
- OS (Ubuntu, Alpine, dll)
- Code aplikasi kamu
- Dependencies (libraries)
- Environment variables

**Analogi**: Image itu kayak **file .iso Windows** yang belum di-install

### 2. Docker Container 🏃
**Container = Image yang sudah jalan**

**Analogi**: 
- Image = File installer
- Container = Aplikasi yang sudah di-install & jalan

**Hubungan**:
```
Docker Image (dormant)
      ↓ docker run
Docker Container (running)
```

### 3. Docker File 📝
**Dockerfile = Recipe/resep untuk bikin image**

Isinya instruksi:
- OS apa yang dipakai
- File apa yang di-copy
- Library apa yang di-install
- Command apa yang dijalanin

---

## Install Docker 🛠️

### Windows:
1. Install **WSL2** (Windows Subsystem for Linux)
   ```bash
   wsl --install
   ```

2. Install **Ubuntu** dari Microsoft Store

3. Download **Docker Desktop for Windows**
   - Link: https://www.docker.com/products/docker-desktop

4. Install & restart

### Mac:
1. Download **Docker Desktop for Mac**
   - Apple Silicon (M1/M2/M3): ARM version
   - Intel chip: AMD64 version

2. Install & done!

### Linux:
```bash
# Ubuntu/Debian
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Post-install
sudo usermod -aG docker $USER
```

**Cek instalasi**:
```bash
docker --version
```

---

## Command Docker Penting! ⚡

### 1. Lihat Docker Images
```bash
docker images
```

**Output**:
```
REPOSITORY          TAG       IMAGE ID       SIZE
python              3.8       abc123         1.2GB
jupyter/notebook    latest    def456         890MB
```

### 2. Pull Image (Download)
```bash
docker pull python:3.8
docker pull jupyter/base-notebook
```

**Spesifik version (tag)**:
```bash
docker pull python:3.8-slim
docker pull python:3.12-alpine
```

### 3. Run Container
```bash
docker run -p 8888:8888 -d --name my-jupyter jupyter/base-notebook
```

**Breakdown**:
- `docker run` = Jalankan container
- `-p 8888:8888` = Port mapping (komputer:container)
- `-d` = Detached mode (jalan di background)
- `--name my-jupyter` = Nama container
- `jupyter/base-notebook` = Image yang dipakai

### 4. Lihat Container yang Jalan
```bash
docker ps
```

**Output**:
```
CONTAINER ID   IMAGE                    STATUS      PORTS
abc123def      jupyter/base-notebook    Up 5 mins   0.0.0.0:8888->8888/tcp
```

**Lihat semua (termasuk yang stopped)**:
```bash
docker ps -a
```

### 5. Stop Container
```bash
docker stop my-jupyter
# atau pakai container ID
docker stop abc123
```

### 6. Start Container Lagi
```bash
docker start my-jupyter
```

### 7. Hapus Container
```bash
docker rm my-jupyter
# Force delete (kalau masih running)
docker rm -f my-jupyter
```

### 8. Hapus Image
```bash
docker rmi python:3.8
# Force delete
docker rmi -f python:3.8
```

### 9. Lihat Logs
```bash
docker logs my-jupyter
# Follow logs (real-time)
docker logs -f my-jupyter
```

### 10. Bersihkan Cache
```bash
docker system prune
# Hapus semua (images, containers, cache)
docker system prune -a
```

---

## Port Mapping - Hubungan Host & Container 🔌

### Konsep:
Setiap container punya **port sendiri** (kayak komputer terpisah!)

### Syntax:
```bash
-p [HOST_PORT]:[CONTAINER_PORT]
```

### Contoh:
```bash
# Port 7070 di laptop → Port 8888 di container
docker run -p 7070:8888 jupyter/base-notebook
```

**Akses**:
- ❌ `localhost:8888` → Nggak bisa!
- ✅ `localhost:7070` → Bisa!

### Multiple Containers:
```bash
# Container 1
docker run -p 8000:5000 --name app1 my-api

# Container 2  
docker run -p 8001:5000 --name app2 my-api

# Container 3
docker run -p 8002:5000 --name app3 my-api
```

Semua pakai port **5000 di dalam container**, tapi beda port di host!

---

## Bikin Docker Image Sendiri! 🏗️

### Step 1: Siapkan Project
```
sentiment-analysis/
  ├── app.py
  ├── requirements.txt
  └── Dockerfile
```

### Step 2: Tulis requirements.txt
```
gradio
textblob
```

### Step 3: Tulis app.py
```python
import gradio as gr
from textblob import TextBlob

def analyze_sentiment(text):
    blob = TextBlob(text)
    sentiment = blob.sentiment.polarity
    
    if sentiment > 0:
        return "Positive 😊"
    elif sentiment < 0:
        return "Negative 😞"
    else:
        return "Neutral 😐"

interface = gr.Interface(
    fn=analyze_sentiment,
    inputs="text",
    outputs="text",
    title="Sentiment Analysis"
)

interface.launch(server_name="0.0.0.0", server_port=7860)
```

### Step 4: Tulis Dockerfile
```dockerfile
# Base image
FROM python:3.8-slim

# Working directory
WORKDIR /app

# Copy requirements & install
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy aplikasi
COPY app.py .

# Expose port
EXPOSE 7860

# Run aplikasi
CMD ["python", "app.py"]
```

### Step 5: Build Image
```bash
docker build -t sentiment-analysis .
```

**Breakdown**:
- `docker build` = Build image
- `-t sentiment-analysis` = Tag/nama image
- `.` = Build dari folder saat ini

### Step 6: Run Container
```bash
docker run -p 9090:7860 --name sa-gradio sentiment-analysis
```

### Step 7: Akses!
Buka browser: `http://localhost:9090`

**Boom! Aplikasi kamu jalan di Docker!** 🎉

---

## Penjelasan Dockerfile 📝

### FROM - Base Image
```dockerfile
FROM python:3.8-slim
```

**Pilihan**:
- `python:3.8` (default) - 360MB (lengkap)
- `python:3.8-slim` - 48MB (minimal)
- `python:3.8-alpine` - <20MB (super minimal)

**Rekomendasi**: Pakai **slim** (balance antara size & compatibility)

### WORKDIR - Working Directory
```dockerfile
WORKDIR /app
```

Semua file aplikasi akan ada di `/app` dalam container

### COPY - Copy Files
```dockerfile
COPY requirements.txt .
COPY app.py .
```

Copy dari **host** → **container**

**Alternative**: Copy semua
```dockerfile
COPY . .
```

### RUN - Execute Command (saat build)
```dockerfile
RUN pip install -r requirements.txt
RUN apt-get update && apt-get install -y git
```

Jalan saat **build image**, bukan saat run container

### EXPOSE - Expose Port
```dockerfile
EXPOSE 7860
```

Kasih tahu port mana yang bisa diakses dari luar

### CMD - Command to Run (saat run)
```dockerfile
CMD ["python", "app.py"]
```

Command yang dijalankan saat **container start**

**Catatan Penting**: 
- ❌ Nggak bisa multiple CMD
- ✅ Cuma command terakhir yang jalan

**Kalau mau multiple apps → Pakai Docker Compose!**

---

## Docker Compose - Multi Container! 🎭

### Apa itu Docker Compose?
**Docker Compose = Jalankan banyak container sekaligus**

### Use Case:
```
App kita butuh:
  ├── Web server (API)
  ├── Database (PostgreSQL)
  └── Cache (Redis)
```

Daripada jalankan satu-satu → Pakai Docker Compose!

### File: docker-compose.yml
```yaml
version: '3'

services:
  # Service 1: Web API
  api:
    image: my-api:latest
    ports:
      - "5000:5000"
    environment:
      - DATABASE_URL=postgres://db:5432
    depends_on:
      - db

  # Service 2: Database
  db:
    image: postgres:13
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_PASSWORD=secret
    volumes:
      - db-data:/var/lib/postgresql/data

  # Service 3: Redis
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"

volumes:
  db-data:
```

### Jalankan:
```bash
# Start semua services
docker-compose up

# Start di background
docker-compose up -d

# Stop semua
docker-compose down
```

### Scale Service:
```bash
# Jalankan 5 instance API
docker-compose up --scale api=5

# Jalankan 3 instance worker
docker-compose up --scale worker=3
```

**Benefit**: Scale horizontal super gampang! 📈

---

## Docker Compose - Contoh Rasa Chatbot 🤖

```yaml
version: '3'

services:
  # Rasa server
  rasa:
    image: rasa/rasa:latest
    ports:
      - "5005:5005"
    command: run --enable-api --cors "*"

  # Action server
  action-server:
    image: rasa/rasa-sdk:latest
    ports:
      - "5055:5055"

  # Duckling (entity extraction)
  duckling:
    image: rasa/duckling:latest
    ports:
      - "8000:8000"
```

**Jalankan**:
```bash
docker-compose up
```

**Boom! 3 services jalan sekaligus!** 🎉

---

## depends_on - Dependency Management 🔗

### Konsep:
Service A butuh Service B jalan dulu!

### Contoh:
```yaml
services:
  api:
    image: my-api
    depends_on:
      - db
      - redis

  db:
    image: postgres

  redis:
    image: redis
```

**Flow**:
```
1. Start db ✅
2. Start redis ✅
3. Start api ✅ (setelah db & redis ready)
```

Kalau `db` error → `api` nggak akan start!

---

## Best Practices Docker 🌟

### 1. Pakai Base Image yang Tepat
```dockerfile
# ❌ Terlalu gede
FROM python:3.8  # 360MB

# ✅ Optimal
FROM python:3.8-slim  # 48MB

# ⚡ Super kecil (tapi kadang incompatible)
FROM python:3.8-alpine  # 18MB
```

### 2. Multi-Stage Build
```dockerfile
# Stage 1: Build
FROM python:3.8 AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --user -r requirements.txt

# Stage 2: Runtime (lebih kecil!)
FROM python:3.8-slim
WORKDIR /app
COPY --from=builder /root/.local /root/.local
COPY app.py .
CMD ["python", "app.py"]
```

**Benefit**: Image final lebih kecil!

### 3. Layer Caching
```dockerfile
# ✅ Copy requirements dulu (jarang berubah)
COPY requirements.txt .
RUN pip install -r requirements.txt

# ✅ Baru copy code (sering berubah)
COPY . .
```

**Kenapa?** Docker cache layer yang nggak berubah → build lebih cepat!

### 4. .dockerignore
```
# .dockerignore
__pycache__/
*.pyc
.git/
.env
node_modules/
*.log
```

Jangan copy file yang nggak perlu!

### 5. Non-Root User
```dockerfile
# Jangan jalan sebagai root (security risk!)
RUN useradd -m myuser
USER myuser
```

### 6. Health Check
```dockerfile
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost:8000/health || exit 1
```

Docker bisa auto-restart kalau app error!

---

## Docker Hub - Share Your Images! 🌐

### Apa itu Docker Hub?
**Docker Hub = GitHub untuk Docker images**

Website: https://hub.docker.com

### Login:
```bash
docker login
```

### Tag Image:
```bash
docker tag sentiment-analysis yourusername/sentiment-analysis:v1
```

### Push ke Docker Hub:
```bash
docker push yourusername/sentiment-analysis:v1
```

### Pull dari Docker Hub:
```bash
docker pull yourusername/sentiment-analysis:v1
```

**Benefit**:
- Share dengan tim
- Deploy di cloud (AWS, GCP, Azure)
- Version control

---

## Troubleshooting 🔧

### 1. Container Langsung Mati
```bash
# Lihat logs
docker logs container-name

# Common issue: Port sudah dipakai
# Solusi: Ganti port
docker run -p 9090:7860 ...
```

### 2. "Permission Denied"
```bash
# Linux: Tambahin user ke docker group
sudo usermod -aG docker $USER

# Logout & login lagi
```

### 3. Image Terlalu Gede
```bash
# Cek size
docker images

# Solusi:
# 1. Pakai slim/alpine base image
# 2. Multi-stage build
# 3. Hapus cache
docker system prune -a
```

### 4. Port Conflict
```bash
# Error: "port is already allocated"

# Cek port yang dipake
docker ps

# Stop container yang pakai port tsb
docker stop container-name
```

### 5. Network Issues
```bash
# Container gak bisa akses internet

# Restart Docker daemon
# Windows/Mac: Restart Docker Desktop
# Linux:
sudo systemctl restart docker
```

---

## Docker vs Virtual Machine 🥊

### Virtual Machine:
```
Hardware
  ├── Host OS
  └── Hypervisor (VMware, VirtualBox)
       ├── VM 1 (Full OS: 2GB)
       ├── VM 2 (Full OS: 2GB)
       └── VM 3 (Full OS: 2GB)
Total: 6GB+
```

**Karakteristik**:
- ❌ Berat (tiap VM ada full OS)
- ❌ Start lambat (menit)
- ✅ Isolation kuat

### Docker:
```
Hardware
  ├── Host OS
  └── Docker Engine
       ├── Container 1 (50MB)
       ├── Container 2 (50MB)
       └── Container 3 (50MB)
Total: 150MB
```

**Karakteristik**:
- ✅ Ringan (share kernel)
- ✅ Start cepat (detik)
- ✅ Resource efficient

**Kesimpulan**: Docker > VM untuk aplikasi! 🏆

---

## Use Cases Docker di ML/AI 🤖

### 1. Development Environment
```dockerfile
# Jupyter Notebook dengan GPU support
FROM nvidia/cuda:11.8-cudnn8-runtime-ubuntu22.04
RUN pip install jupyter tensorflow torch
```

**Benefit**: Konsisten di semua developer

### 2. Model Serving
```dockerfile
# API untuk serve ML model
FROM python:3.8-slim
COPY model.pkl .
COPY api.py .
CMD ["python", "api.py"]
```

**Benefit**: Deploy mudah, scale gampang

### 3. Training Pipeline
```yaml
# docker-compose.yml
services:
  data-prep:
    image: data-preprocessing:v1
  
  training:
    image: model-training:v1
    depends_on:
      - data-prep
  
  evaluation:
    image: model-evaluation:v1
    depends_on:
      - training
```

**Benefit**: Pipeline otomatis

### 4. Batch Processing
```bash
# Process banyak data
docker run -v /data:/app/data batch-processor:v1
```

**Benefit**: Parallel processing

---

## Cloud Deployment dengan Docker ☁️

### AWS (ECS - Elastic Container Service):
```bash
# 1. Push image ke ECR
aws ecr get-login-password | docker login ...
docker push xxx.dkr.ecr.region.amazonaws.com/my-app

# 2. Deploy ke ECS
aws ecs create-service ...
```

### Google Cloud (Cloud Run):
```bash
# 1. Push ke GCR
docker tag my-app gcr.io/project-id/my-app
docker push gcr.io/project-id/my-app

# 2. Deploy
gcloud run deploy --image gcr.io/project-id/my-app
```

### Azure (Container Instances):
```bash
# 1. Push ke ACR
docker tag my-app myregistry.azurecr.io/my-app
docker push myregistry.azurecr.io/my-app

# 2. Deploy
az container create ...
```

**Benefit**: Auto-scaling, load balancing, high availability!

---

## Kubernetes - Next Level! ☸️

### Apa itu Kubernetes (K8s)?
**Kubernetes = Orchestrator untuk manage Docker containers di scale besar**

**Fitur**:
- Auto-scaling (horizontal & vertical)
- Load balancing
- Self-healing (auto-restart kalau error)
- Rolling updates
- Service discovery

**Kapan Pakai K8s**:
- Production apps
- Microservices architecture
- Need high availability
- Traffic tinggi

**Kapan Cukup Docker Compose**:
- Development
- Small apps
- Single server

---

## Summary - Yang Wajib Diingat! 📝

### Docker Basics:
1. **Image** = Blueprint (belum jalan)
2. **Container** = Image yang sudah jalan
3. **Dockerfile** = Recipe untuk bikin image
4. **Docker Compose** = Manage multiple containers

### Commands Penting:
```bash
docker images          # Lihat images
docker ps              # Lihat containers
docker run             # Jalankan container
docker stop            # Stop container
docker rm              # Hapus container
docker rmi             # Hapus image
docker logs            # Lihat logs
docker build -t name . # Build image
docker-compose up      # Jalankan compose
```

### Benefits:
- ✅ Portability (jalan di mana aja)
- ✅ Consistency (environment sama)
- ✅ Isolation (no dependency conflict)
- ✅ Versioning (easy rollback)
- ✅ Scalability (scale horizontal gampang)

### Best Practices:
- Pakai slim base image
- Layer caching
- .dockerignore
- Multi-stage build
- Health checks

---

## Next Steps 🎯

1. **Practice**: Bikin Docker image untuk project kamu
2. **Explore**: Coba Docker Compose untuk multi-service
3. **Share**: Push image ke Docker Hub
4. **Deploy**: Deploy ke cloud (AWS/GCP/Azure)
5. **Learn K8s**: Next level orchestration

**Remember**: Docker = skill wajib untuk ML Engineer! 🚀

---

## Resources 📚

- Docker Docs: https://docs.docker.com
- Docker Hub: https://hub.docker.com
- Play with Docker: https://labs.play-with-docker.com
- Awesome Docker: https://github.com/veggiemonk/awesome-docker

---

**Happy Dockering! 🐳✨**
