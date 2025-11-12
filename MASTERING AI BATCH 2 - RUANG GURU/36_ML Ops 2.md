# ML Ops 2 - Weights & Biases (W&B) - Track Your ML Like a Pro! 📊

## Apa itu Weights & Biases (W&B)? 🤔

### Definisi:
**W&B = Tools untuk tracking, monitoring, dan kolaborasi Machine Learning projects**

**Singkatan**: W&B atau WandB  
**Website**: https://wandb.ai

### Analogi Sederhana:
W&B itu kayak **Google Drive + GitHub untuk ML experiments**:
- Track semua experiment kamu
- Visualize metrics real-time
- Compare models
- Collaborate dengan tim
- Version control untuk model

---

## Kenapa Butuh W&B? 🎯

### Problem Tanpa Tracking Tools:
```
Experiment 1: Akurasi 85% - Parameter apa? Lupa! 😅
Experiment 2: Akurasi 87% - Dataset apa? Lupa!
Experiment 3: Akurasi 82% - Model apa? Lupa!

Best model kehapus → Start from scratch lagi! 😭
```

### Dengan W&B:
```
✅ Semua experiment ter-record
✅ Semua parameter tersimpan
✅ Semua model bisa di-download
✅ Performa ter-visualize real-time
✅ Compare experiments gampang
```

**Kesimpulan**: W&B bikin hidup ML Engineer lebih mudah! 🎉

---

## Kenapa W&B vs Alternatif? 📊

### Alternatif Open Source:
1. **MLflow** - Tracking & deployment
2. **TensorBoard** - Visualisasi TensorFlow
3. **Optuna** - Hyperparameter optimization

### Keuntungan W&B:
- ✅ **All-in-one**: Tracking + Visualization + Hyperparameter tuning
- ✅ **No setup server**: Langsung cloud-based
- ✅ **Gratis untuk individual**
- ✅ **Beautiful UI** & easy to use
- ✅ **Collaboration built-in**

**Kekurangan**:
- ❌ Enterprise perlu bayar
- ❌ Data disimpan di server W&B (privacy concern untuk beberapa company)

**Kapan Pakai W&B**: 
- Individual projects
- Small teams
- Rapid experimentation
- Learning & research

**Kapan Pakai MLflow**:
- Enterprise projects
- Need full control
- On-premise deployment
- Custom requirements

---

## 4 Alasan Utama Pakai W&B 🌟

### 1. Track & Visualize Models 📈

**Real-time monitoring**:
```python
# Loss turun secara real-time
Epoch 1: Loss = 2.5
Epoch 2: Loss = 1.8
Epoch 3: Loss = 1.2
...
```

**Multiple experiments comparison**:
```
Experiment A: Learning rate 0.001 → Akurasi 85%
Experiment B: Learning rate 0.01  → Akurasi 87%
Experiment C: Learning rate 0.1   → Akurasi 82%
```

Langsung kelihatan mana yang terbaik! 🎯

### 2. Hyperparameter Optimization 🔧

**W&B Sweep** = Auto cari hyperparameter terbaik!

```yaml
parameters:
  learning_rate:
    min: 0.0001
    max: 0.1
  batch_size:
    values: [16, 32, 64, 128]
  epochs:
    value: 50
```

W&B otomatis test kombinasi parameter & kasih tahu yang terbaik!

### 3. Collaboration 👥

**Tanpa W&B**:
```
Dev A: "Model saya akurasi 90%!"
Dev B: "Parameternya apa?"
Dev A: "Lupa... 😅"
```

**Dengan W&B**:
```
Dev A: Share W&B project link
Dev B: Bisa lihat semua detail:
  - Parameters
  - Metrics
  - Logs
  - Model artifacts
```

### 4. Reproducibility 🔄

**Scenario**:
```
Boss: "Model versi kemarin kok bagus? 
       Sekarang kok jelek? Balik ke yang lama!"

Tanpa W&B: ??? Parameters apa? Model mana?
Dengan W&B: ✅ Download model v1, lihat params, reproduce!
```

---

## Setup W&B 🛠️

### 1. Sign Up (GRATIS!)
```
1. Buka: https://wandb.ai
2. Klik "Sign Up"
3. Sign up with Google (instant!)
```

### 2. Install
```bash
pip install wandb
```

### 3. Login
```python
import wandb

# Login (first time)
wandb.login()
# Popup browser → Copy API key → Paste
```

**Sekali login, selamanya teringat!** ✅

---

## Quick Start - Image Classification 🖼️

### Full Example: CIFAR-10 CNN

```python
import torch
import torch.nn as nn
import torchvision
import wandb

# 1. Initialize W&B
wandb.init(
    project="cifar10-classification",  # Project name
    entity="your-username",             # Your username
    name="experiment-1",                # Experiment name
    config={                            # Save hyperparameters
        "learning_rate": 0.001,
        "epochs": 10,
        "batch_size": 64,
        "optimizer": "Adam",
        "model": "CNN"
    }
)

# Access config
config = wandb.config

# 2. Load data
transform = torchvision.transforms.ToTensor()
train_dataset = torchvision.datasets.CIFAR10(
    root='./data', train=True, download=True, transform=transform
)
train_loader = torch.utils.data.DataLoader(
    train_dataset, batch_size=config.batch_size, shuffle=True
)

# 3. Define model
class SimpleCNN(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Conv2d(3, 32, 3)
        self.conv2 = nn.Conv2d(32, 64, 3)
        self.fc1 = nn.Linear(64*6*6, 128)
        self.fc2 = nn.Linear(128, 10)
        self.relu = nn.ReLU()
        self.pool = nn.MaxPool2d(2, 2)
    
    def forward(self, x):
        x = self.pool(self.relu(self.conv1(x)))
        x = self.pool(self.relu(self.conv2(x)))
        x = x.view(-1, 64*6*6)
        x = self.relu(self.fc1(x))
        x = self.fc2(x)
        return x

model = SimpleCNN().cuda()
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=config.learning_rate)

# 4. Training loop
for epoch in range(config.epochs):
    running_loss = 0.0
    correct = 0
    total = 0
    
    for i, (inputs, labels) in enumerate(train_loader):
        inputs, labels = inputs.cuda(), labels.cuda()
        
        optimizer.zero_grad()
        outputs = model(inputs)
        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()
        
        running_loss += loss.item()
        _, predicted = outputs.max(1)
        total += labels.size(0)
        correct += predicted.eq(labels).sum().item()
    
    # 5. Log metrics to W&B
    epoch_loss = running_loss / len(train_loader)
    epoch_acc = 100. * correct / total
    
    wandb.log({
        "epoch": epoch,
        "loss": epoch_loss,
        "accuracy": epoch_acc
    })
    
    print(f"Epoch {epoch}: Loss = {epoch_loss:.4f}, Acc = {epoch_acc:.2f}%")

# 6. Save model
torch.save(model.state_dict(), "model.pt")

# 7. Upload model to W&B
wandb.save("model.pt")

# 8. Finish
wandb.finish()
```

**Dashboard W&B**:
- ✅ Loss turun per epoch (grafik real-time)
- ✅ Accuracy naik per epoch
- ✅ All hyperparameters saved
- ✅ Model tersimpan di Artifacts

---

## W&B Core Concepts 🎓

### 1. Project
**Project = Folder/container untuk experiments**

Contoh:
```
Project: "sentiment-analysis"
  ├── Experiment 1: BERT base
  ├── Experiment 2: BERT large
  └── Experiment 3: DistilBERT
```

### 2. Run (Experiment)
**Run = Single experiment/training session**

Contoh:
```
Run: "bert-base-lr0.001"
  ├── Config: {model: "bert-base", lr: 0.001}
  ├── Metrics: Loss, Accuracy, F1
  └── Artifacts: Model, Dataset
```

### 3. Artifacts
**Artifacts = Files yang disimpan** (model, dataset, config)

Contoh:
```
Artifacts:
  ├── model.pt (v1, v2, v3)
  ├── train_data.csv
  └── config.yaml
```

**Version control otomatis!**

### 4. Sweep
**Sweep = Hyperparameter optimization runs**

Contoh:
```
Sweep: "find-best-lr"
  ├── Run 1: lr=0.001, acc=85%
  ├── Run 2: lr=0.01, acc=87%  ← Best!
  └── Run 3: lr=0.1, acc=82%
```

---

## Logging - Track Everything! 📝

### Basic Logging:
```python
# Log single value
wandb.log({"loss": 0.5})

# Log multiple values
wandb.log({
    "loss": 0.5,
    "accuracy": 85.3,
    "learning_rate": 0.001
})

# Log with step
wandb.log({"loss": 0.5}, step=100)
```

### Log Every Epoch:
```python
for epoch in range(10):
    # Training...
    train_loss = train()
    val_loss, val_acc = validate()
    
    wandb.log({
        "epoch": epoch,
        "train_loss": train_loss,
        "val_loss": val_loss,
        "val_accuracy": val_acc
    })
```

### Log System Metrics (Auto!):
```python
# GPU usage, CPU, Memory - tracked automatically!
wandb.init(project="my-project")

# W&B otomatis track:
# - GPU utilization
# - GPU memory
# - GPU temperature
# - CPU usage
# - RAM usage
# - Disk usage
```

**Ini otomatis! Nggak perlu kode tambahan!** 🎉

---

## W&B Alert - Get Notified! 🔔

### Setup Alert:
```python
import wandb

wandb.init(project="training-monitor")

# Alert kalau accuracy rendah
if accuracy < 0.5:
    wandb.alert(
        title="Low Accuracy Warning",
        text=f"Accuracy is only {accuracy:.2%}",
        level="WARN"
    )

# Alert kalau loss tinggi
if loss > 2.0:
    wandb.alert(
        title="High Loss Alert",
        text=f"Loss is {loss:.2f}, check training!",
        level="ERROR"
    )
```

**Dikirim via**:
- 📧 Email
- 💬 Slack
- 🔔 Browser notification

**Use Case**:
- Training kelamaan → tidur dulu
- Pas error → langsung dapet notif
- Loss meledak → stop training ASAP

---

## W&B Artifacts - Version Control Model! 📦

### Save Model:
```python
# Method 1: Simple save
torch.save(model.state_dict(), "model.pt")
wandb.save("model.pt")

# Method 2: Artifact (recommended!)
artifact = wandb.Artifact(
    name="cnn-model",
    type="model",
    description="CNN model for CIFAR-10"
)
artifact.add_file("model.pt")
wandb.log_artifact(artifact)
```

### Load Model:
```python
# Initialize W&B
wandb.init(project="cifar10-classification")

# Download artifact
artifact = wandb.use_artifact('cnn-model:latest')  # or v1, v2
artifact_dir = artifact.download()

# Load model
model = SimpleCNN()
model.load_state_dict(torch.load(f"{artifact_dir}/model.pt"))
```

### Version Control:
```
Artifacts:
  ├── cnn-model:v0 (latest)
  ├── cnn-model:v1
  └── cnn-model:v2
```

**Rollback gampang!** Tinggal ganti version.

---

## W&B Sweep - Hyperparameter Tuning! 🔍

### Apa itu Sweep?
**Sweep = Auto cari kombinasi hyperparameter terbaik**

### Setup Sweep Config:
```python
sweep_config = {
    'method': 'random',  # random, grid, bayes
    'metric': {
        'name': 'accuracy',
        'goal': 'maximize'  # or 'minimize' for loss
    },
    'parameters': {
        'learning_rate': {
            'min': 0.0001,
            'max': 0.1
        },
        'batch_size': {
            'values': [16, 32, 64, 128]
        },
        'optimizer': {
            'values': ['adam', 'sgd', 'rmsprop']
        },
        'dropout': {
            'min': 0.1,
            'max': 0.5
        }
    }
}
```

### Create Sweep:
```python
sweep_id = wandb.sweep(
    sweep=sweep_config,
    project="cifar10-sweep"
)
```

### Define Training Function:
```python
def train():
    # Initialize W&B with sweep config
    wandb.init()
    config = wandb.config  # Auto-filled by sweep!
    
    # Build model dengan config
    model = build_model(
        lr=config.learning_rate,
        batch_size=config.batch_size,
        optimizer=config.optimizer
    )
    
    # Training loop
    for epoch in range(10):
        loss, acc = train_epoch(model)
        wandb.log({"loss": loss, "accuracy": acc})
    
    wandb.finish()
```

### Run Sweep:
```python
# Run 20 experiments
wandb.agent(sweep_id, function=train, count=20)
```

**W&B otomatis**:
- ✅ Generate 20 kombinasi parameter
- ✅ Train 20 models
- ✅ Track semua metrics
- ✅ Kasih tahu best parameters!

### Lihat Results:
```
Best Run: learning_rate=0.01, batch_size=64, optimizer=adam
Best Accuracy: 87.5%

Worst Run: learning_rate=0.1, batch_size=16, optimizer=sgd
Accuracy: 72.3%
```

**Visualization**: Grafik parallel coordinates untuk compare!

---

## W&B + HuggingFace 🤗

### Super Simple Integration!
```python
from transformers import Trainer, TrainingArguments
import wandb

# 1. Initialize W&B
wandb.init(project="sentiment-analysis")

# 2. Setup trainer
training_args = TrainingArguments(
    output_dir="./results",
    num_train_epochs=3,
    per_device_train_batch_size=16,
    learning_rate=2e-5,
    report_to="wandb",  # ← Magic line!
    run_name="roberta-mini-run"
)

trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=train_dataset,
    eval_dataset=eval_dataset
)

# 3. Train (auto-log to W&B!)
trainer.train()

# 4. Done!
wandb.finish()
```

**That's it!** W&B otomatis track:
- ✅ Loss
- ✅ Learning rate
- ✅ Gradient norms
- ✅ Training time
- ✅ All hyperparameters

---

## W&B Dashboard - UI Keren! 🎨

### Overview Tab:
```
Project: speech-to-text
Runs: 156
Success: 142 ✅
Failed: 14 ❌

Best WER: 12.3% (swift-recommendation-04)
Worst WER: 45.7% (dry-brook-01)
```

### Workspace Tab:
**Interactive charts**:
- Line plot: Loss over time
- Scatter plot: Learning rate vs accuracy
- Histogram: Distribution of metrics
- Bar chart: Compare runs

**Custom panels**: Drag & drop charts!

### Runs Tab:
```
Run Name               | LR    | Batch | Accuracy | Status
----------------------|-------|-------|----------|--------
swift-recommendation  | 0.01  | 64    | 87.5%    | ✅
dry-brook            | 0.001 | 32    | 85.2%    | ✅
colorful-swift       | 0.1   | 128   | 72.1%    | ❌
```

**Filter & sort**: Cari best run gampang!

### Artifacts Tab:
```
Artifact Name      | Type    | Version | Size
-------------------|---------|---------|------
model-after-train  | model   | v3      | 500MB
dataset-clean      | dataset | v1      | 2GB
config             | config  | v2      | 10KB
```

**Download**: One click download!

### System Metrics Tab:
```
GPU Utilization: 95% ✅
GPU Memory: 10GB / 11GB
GPU Temperature: 75°C (safe)
CPU Usage: 45%
RAM Usage: 16GB / 32GB
```

**Real-time monitoring!** 📊

---

## Collaboration - Team Work! 👥

### Share Project:
```
1. Buka project di W&B
2. Klik "Share" 
3. Invite by email atau link
4. Set permission: View / Edit
```

### Team Workflow:
```
Data Scientist A:
  - Experiment dengan berbagai model
  - Track di W&B project

Data Scientist B:
  - Lihat hasil A
  - Lanjutkan experiment terbaik
  - Compare dengan baseline

Manager:
  - Monitor progress
  - Pilih model untuk production
  - Export reports
```

**Benefit**: No more "di laptop saya jalan" problem! 🎉

---

## Best Practices W&B ⭐

### 1. Naming Convention:
```python
# ❌ Bad
wandb.init(name="test")
wandb.init(name="model1")

# ✅ Good
wandb.init(name="bert-base-lr0.001-bs64")
wandb.init(name="resnet50-sgd-aug-v2")
```

**Format**: `{model}-{key_params}-{version}`

### 2. Organize Projects:
```
Projects:
  ├── sentiment-analysis-research (experiments)
  ├── sentiment-analysis-production (final models)
  └── sentiment-analysis-ablation (ablation studies)
```

### 3. Use Config:
```python
# ✅ Save semua hyperparameters
config = {
    "model": "bert-base",
    "learning_rate": 0.001,
    "batch_size": 64,
    "optimizer": "adam",
    "scheduler": "linear",
    "warmup_steps": 500,
    "max_length": 512,
    "dropout": 0.1
}

wandb.init(project="my-project", config=config)
```

### 4. Log Important Metrics Only:
```python
# ❌ Log setiap iteration (too much!)
for i, batch in enumerate(dataloader):
    loss = train_step(batch)
    wandb.log({"loss": loss})  # 10,000 logs!

# ✅ Log aggregate per epoch
epoch_losses = []
for batch in dataloader:
    loss = train_step(batch)
    epoch_losses.append(loss)

wandb.log({"epoch_loss": np.mean(epoch_losses)})
```

### 5. Use Artifacts for Everything:
```python
# Model
model_artifact = wandb.Artifact("model", type="model")
model_artifact.add_file("model.pt")
wandb.log_artifact(model_artifact)

# Dataset
data_artifact = wandb.Artifact("dataset", type="dataset")
data_artifact.add_dir("data/")
wandb.log_artifact(data_artifact)

# Config
config_artifact = wandb.Artifact("config", type="config")
config_artifact.add_file("config.yaml")
wandb.log_artifact(config_artifact)
```

### 6. Tag Runs:
```python
wandb.init(
    project="experiments",
    tags=["baseline", "bert", "production-ready"]
)
```

**Filter by tags** di dashboard!

### 7. Always Finish:
```python
try:
    wandb.init()
    # Training...
    train()
finally:
    wandb.finish()  # Always finish!
```

---

## Common Use Cases 🎯

### 1. Experiment Tracking:
```python
for lr in [0.001, 0.01, 0.1]:
    wandb.init(
        project="lr-experiment",
        name=f"lr-{lr}",
        config={"learning_rate": lr}
    )
    
    model = train_model(lr=lr)
    acc = evaluate(model)
    
    wandb.log({"accuracy": acc})
    wandb.finish()

# Compare di dashboard!
```

### 2. Model Checkpointing:
```python
best_acc = 0

for epoch in range(100):
    acc = train_epoch()
    wandb.log({"accuracy": acc})
    
    if acc > best_acc:
        best_acc = acc
        torch.save(model.state_dict(), f"best_model_epoch{epoch}.pt")
        wandb.save(f"best_model_epoch{epoch}.pt")
```

### 3. Ablation Study:
```python
configs = [
    {"use_attention": True, "use_dropout": True},
    {"use_attention": True, "use_dropout": False},
    {"use_attention": False, "use_dropout": True},
    {"use_attention": False, "use_dropout": False},
]

for config in configs:
    wandb.init(project="ablation", config=config)
    model = build_model(**config)
    acc = train_and_eval(model)
    wandb.log({"accuracy": acc})
    wandb.finish()
```

### 4. Distributed Training:
```python
# Rank 0 (main process)
if rank == 0:
    wandb.init(project="distributed-training")

# All ranks
for epoch in range(epochs):
    loss = train_epoch()
    
    # Only rank 0 logs
    if rank == 0:
        wandb.log({"loss": loss})
```

---

## Troubleshooting 🔧

### 1. "API key not found"
```python
# Solusi: Login manual
import wandb
wandb.login(key="your-api-key")

# Atau set environment variable
import os
os.environ["WANDB_API_KEY"] = "your-key"
```

### 2. "Too many requests"
```python
# Log lebih jarang
# ❌ Every step
for step in range(10000):
    wandb.log({"loss": loss})

# ✅ Every N steps
if step % 100 == 0:
    wandb.log({"loss": loss})
```

### 3. Offline Mode (No Internet):
```python
# Run offline
wandb.init(mode="offline")

# Sync later
# wandb sync wandb/offline-run-xxx
```

### 4. Slow Upload:
```python
# Disable system metrics
wandb.init(
    project="my-project",
    settings=wandb.Settings(_disable_stats=True)
)
```

---

## W&B vs Alternatives 📊

### W&B vs MLflow:
| Feature | W&B | MLflow |
|---------|-----|--------|
| Setup | ✅ Cloud (no setup) | ❌ Need server |
| UI | ✅ Beautiful | ⚠️ Basic |
| Collaboration | ✅ Built-in | ❌ Manual |
| Hyperparameter tuning | ✅ W&B Sweep | ❌ Need Optuna |
| Cost | ⚠️ Free tier limited | ✅ Free (self-host) |

### W&B vs TensorBoard:
| Feature | W&B | TensorBoard |
|---------|-----|-------------|
| Setup | ✅ Cloud | ❌ Local/server |
| Collaboration | ✅ Easy | ❌ Hard |
| Frameworks | ✅ All | ⚠️ Best for TF |
| Artifacts | ✅ Built-in | ❌ Manual |

**Kesimpulan**: 
- **W&B**: Best for rapid experimentation & collaboration
- **MLflow**: Best for enterprise & on-premise
- **TensorBoard**: Best if you only use TensorFlow

---

## Pricing - Free Tier Cukup! 💰

### Free (Individual):
- ✅ Unlimited runs
- ✅ Unlimited projects
- ✅ 100GB storage
- ✅ Public projects
- ✅ Basic collaboration

### Academic (Free):
- ✅ Same as Free
- ✅ Private projects
- ✅ Priority support

### Teams ($50/month):
- ✅ Private projects
- ✅ 1TB storage
- ✅ Advanced collaboration
- ✅ SSO

### Enterprise (Custom):
- ✅ On-premise option
- ✅ Unlimited storage
- ✅ SLA & support
- ✅ Custom integrations

**Untuk belajar & project personal: FREE sudah lebih dari cukup!** 🎉

---

## Summary - Hal Penting! 📝

### W&B Core:
1. **Project**: Container untuk experiments
2. **Run**: Single experiment
3. **Artifacts**: Model/dataset versioning
4. **Sweep**: Hyperparameter optimization
5. **Alerts**: Notifikasi via email/Slack

### Key Benefits:
- ✅ Track experiments automatically
- ✅ Visualize metrics real-time
- ✅ Compare models easily
- ✅ Collaborate with team
- ✅ Reproduce results
- ✅ Version control models

### Basic Workflow:
```python
# 1. Initialize
wandb.init(project="my-project", config={...})

# 2. Training loop
for epoch in range(epochs):
    loss = train()
    wandb.log({"loss": loss})

# 3. Save model
wandb.save("model.pt")

# 4. Finish
wandb.finish()
```

### Pro Tips:
- Use descriptive run names
- Log important metrics only
- Save models as artifacts
- Use sweeps for hyperparameter tuning
- Set up alerts for long training
- Always call `wandb.finish()`

---

## Next Steps 🎯

1. **Practice**: Track your next ML project dengan W&B
2. **Explore**: Coba W&B Sweep untuk hyperparameter tuning
3. **Collaborate**: Share project dengan teman/tim
4. **Compare**: Bandingkan dengan MLflow/TensorBoard
5. **Optimize**: Gunakan insights dari W&B untuk improve model

**Remember**: Good tracking = Better models! 📊🚀

---

## Resources 📚

- W&B Docs: https://docs.wandb.ai
- W&B Examples: https://github.com/wandb/examples
- W&B Course: https://www.wandb.courses
- W&B Community: https://wandb.ai/community

---

## Quick Reference Card 📇

### Essential Code:
```python
# Setup
import wandb
wandb.login()

# Initialize
wandb.init(
    project="project-name",
    entity="username",
    name="run-name",
    config={...}
)

# Log metrics
wandb.log({"metric": value})

# Log with step
wandb.log({"metric": value}, step=i)

# Alert
wandb.alert(title="Alert", text="Message", level="WARN")

# Save model
wandb.save("model.pt")
# or
artifact = wandb.Artifact("model", type="model")
artifact.add_file("model.pt")
wandb.log_artifact(artifact)

# Load model
artifact = wandb.use_artifact("model:latest")
artifact.download()

# Finish
wandb.finish()

# Sweep
sweep_id = wandb.sweep(sweep_config, project="project")
wandb.agent(sweep_id, function=train, count=20)
```

**Keep this for quick reference! 📌**

---

**Happy Tracking! 📊✨**

---

## Bonus: Real World Example 🌟

### Speech-to-Text Training:
```python
import wandb
from transformers import Wav2Vec2ForCTC, Trainer

# Initialize
wandb.init(
    project="xlsr-indonesia",
    config={
        "model": "wav2vec2-xlsr-300m",
        "language": "indonesian",
        "learning_rate": 1e-4,
        "batch_size": 16,
        "gradient_accumulation_steps": 2,
    }
)

# Training arguments
training_args = TrainingArguments(
    output_dir="./results",
    num_train_epochs=30,
    per_device_train_batch_size=16,
    learning_rate=1e-4,
    warmup_steps=500,
    logging_steps=10,
    eval_steps=100,
    save_steps=500,
    report_to="wandb",  # Magic!
)

# Trainer
trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=train_dataset,
    eval_dataset=eval_dataset,
)

# Train
trainer.train()

# Save best model
model.save_pretrained("best_model")
artifact = wandb.Artifact("speech-model", type="model")
artifact.add_dir("best_model")
wandb.log_artifact(artifact)

# Finish
wandb.finish()
```

**Result**: Semua metrics tracked, model saved, reproducible! ✅

---

**That's all! Happy ML Engineering! 🚀**
