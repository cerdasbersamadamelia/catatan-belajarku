# Stable Diffusion - Part 2 (Fine-tuning & Advanced)

## Custom Model Training

### Mengapa Fine-tune?

**Use Cases:**
1. **Specific style:** art style tertentu
2. **Specific subject:** orang, karakter, object
3. **Specific domain:** arsitektur, medis, produk
4. **Brand consistency:** untuk design consistency

**Keuntungan:**
- Model understand your specific needs
- Consistent output style
- Better control
- Personalization

## Textual Inversion

### Konsep

Textual Inversion adalah teknik untuk mengajarkan model konsep baru dengan:
- Train embedding untuk token baru
- Freeze model weights
- Hanya train token embedding
- Fast dan lightweight

**Analogi:**
Seperti mengajarkan vocabulary baru ke model tanpa ubah grammar-nya.

### Training Process

**1. Prepare Dataset:**
- 3-5 images minimal
- 10-20 images optimal
- Consistent subject/style
- Different angles/poses
- Good quality

**2. Setup:**
```bash
pip install diffusers accelerate transformers
```

**3. Training Script:**
```python
from diffusers import StableDiffusionPipeline
import torch

# Define unique token
placeholder_token = "sks"  # Unique identifier
initializer_token = "person"  # Similar concept

# Load model
pipe = StableDiffusionPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    torch_dtype=torch.float16
).to("cuda")

# Train (simplified - use official script for full version)
# Training details: learning rate, steps, etc.
```

**4. Usage setelah training:**
```python
prompt = "a photo of sks person in a garden"
image = pipe(prompt).images[0]
```

**Tips:**
- Use unique token (sks, xyz, etc)
- More images = better results
- Diverse poses/angles
- Training: 1000-3000 steps

## DreamBooth

### Konsep

DreamBooth fine-tune entire model untuk specific subject:
- Fine-tune full U-Net
- More powerful dari Textual Inversion
- Need more resources
- Better quality

**Process:**
```
Pre-trained Model + Your Images → Fine-tuned Model
```

### Training Steps

**1. Prepare Dataset:**
- 20-30 images optimal
- Various angles/lighting/backgrounds
- High quality images
- Consistent subject

**2. Class Images (Optional):**
- Generate ~200 images of class (e.g., "person")
- Untuk preserve model knowledge
- Prevent overfitting

**3. Training Config:**
```bash
# Using Hugging Face Diffusers
export MODEL_NAME="runwayml/stable-diffusion-v1-5"
export INSTANCE_DIR="./my_images"
export CLASS_DIR="./class_images"
export OUTPUT_DIR="./dreambooth_model"

accelerate launch train_dreambooth.py \
  --pretrained_model_name_or_path=$MODEL_NAME \
  --instance_data_dir=$INSTANCE_DIR \
  --class_data_dir=$CLASS_DIR \
  --output_dir=$OUTPUT_DIR \
  --instance_prompt="a photo of sks person" \
  --class_prompt="a photo of person" \
  --resolution=512 \
  --train_batch_size=1 \
  --learning_rate=5e-6 \
  --max_train_steps=800 \
  --with_prior_preservation \
  --prior_loss_weight=1.0
```

**4. Usage:**
```python
from diffusers import StableDiffusionPipeline

# Load trained model
pipe = StableDiffusionPipeline.from_pretrained(
    "./dreambooth_model",
    torch_dtype=torch.float16
).to("cuda")

# Generate
prompt = "a photo of sks person as a superhero"
image = pipe(prompt).images[0]
```

### Training Parameters

**Learning Rate:**
- 5e-6 for full model
- 1e-4 for LoRA
- Lower = more stable, slower
- Higher = faster, risk overfitting

**Steps:**
- 400-800 steps typical
- More data = more steps
- Monitor loss curve
- Stop sebelum overfit

**Resolution:**
- 512x512 (SD v1.5)
- 768x768 (SD v2.1)
- 1024x1024 (SDXL)
- Must match model's training resolution

**Batch Size:**
- Depends on GPU VRAM
- 1-2 for consumer GPUs
- Larger = faster tapi need more memory

## LoRA (Low-Rank Adaptation)

### Konsep

LoRA adalah efficient fine-tuning technique:
- Train small adapter layers
- Freeze base model
- Much smaller file size (2-200 MB vs 2-7 GB)
- Fast training
- Mergeable dan stackable

**Keuntungan:**
- Efficient storage
- Fast switching
- Mix multiple LoRAs
- Less overfitting risk

### Training LoRA

**1. Setup:**
```bash
pip install peft
```

**2. Training:**
```python
from diffusers import StableDiffusionPipeline
from peft import LoraConfig, get_peft_model

# Load base model
pipe = StableDiffusionPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    torch_dtype=torch.float16
)

# LoRA config
lora_config = LoraConfig(
    r=8,  # Rank
    lora_alpha=32,
    target_modules=["to_k", "to_q", "to_v", "to_out.0"],
    lora_dropout=0.0
)

# Train (simplified)
# Use official training scripts for complete version
```

**3. Usage:**
```python
from diffusers import StableDiffusionPipeline

# Load base model
pipe = StableDiffusionPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    torch_dtype=torch.float16
).to("cuda")

# Load LoRA weights
pipe.load_lora_weights("./lora_model")

# Generate
image = pipe("a photo of sks person").images[0]

# Unload
pipe.unload_lora_weights()
```

**4. Mix Multiple LoRAs:**
```python
# Load multiple LoRAs dengan different weights
pipe.load_lora_weights("style_lora", adapter_name="style")
pipe.load_lora_weights("character_lora", adapter_name="character")

pipe.set_adapters(["style", "character"], adapter_weights=[0.7, 0.8])
```

### LoRA Parameters

**Rank (r):**
- 4-8 for style
- 16-32 for characters
- Higher = more capacity, larger file
- Start low, increase if needed

**Alpha:**
- Usually 2x of rank
- r=8 → alpha=16
- Controls strength of adaptation

**Target Modules:**
- Attention layers most effective
- Cross-attention untuk text conditioning
- Self-attention untuk image structure

## ControlNet

### Konsep

ControlNet add spatial control ke Stable Diffusion:
- Guide generation dengan additional input
- Edge maps, pose, depth, etc.
- More precise control
- Preserves original model capability

**Types:**
1. **Canny Edge:** edge detection
2. **Pose:** human pose skeleton
3. **Depth:** depth map
4. **Scribble:** rough sketches
5. **Segmentation:** semantic maps
6. **Normal:** surface normals

### Using ControlNet

**1. Installation:**
```bash
pip install controlnet-aux
```

**2. Canny Edge Example:**
```python
from diffusers import StableDiffusionControlNetPipeline, ControlNetModel
import cv2
import numpy as np
from PIL import Image

# Load ControlNet
controlnet = ControlNetModel.from_pretrained(
    "lllyasviel/sd-controlnet-canny",
    torch_dtype=torch.float16
)

# Load pipeline
pipe = StableDiffusionControlNetPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    controlnet=controlnet,
    torch_dtype=torch.float16
).to("cuda")

# Prepare control image (canny edge)
image = Image.open("input.jpg")
image = np.array(image)
edges = cv2.Canny(image, 100, 200)
edges = Image.fromarray(edges)

# Generate
prompt = "a beautiful mansion, highly detailed"
output = pipe(
    prompt=prompt,
    image=edges,
    num_inference_steps=30
).images[0]
```

**3. Pose Control Example:**
```python
from controlnet_aux import OpenposeDetector

# Detect pose
detector = OpenposeDetector.from_pretrained("lllyasviel/ControlNet")
pose_image = detector(input_image)

# Generate dengan pose control
controlnet = ControlNetModel.from_pretrained(
    "lllyasviel/sd-controlnet-openpose",
    torch_dtype=torch.float16
)

pipe = StableDiffusionControlNetPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    controlnet=controlnet,
    torch_dtype=torch.float16
).to("cuda")

output = pipe(
    prompt="a person in a suit, professional photo",
    image=pose_image
).images[0]
```

### Multi-ControlNet

Combine multiple controls:
```python
from diffusers import StableDiffusionControlNetPipeline, ControlNetModel, MultiControlNetModel

# Load multiple ControlNets
controlnet_canny = ControlNetModel.from_pretrained("lllyasviel/sd-controlnet-canny")
controlnet_depth = ControlNetModel.from_pretrained("lllyasviel/sd-controlnet-depth")

controlnet = MultiControlNetModel([controlnet_canny, controlnet_depth])

pipe = StableDiffusionControlNetPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    controlnet=controlnet
)

# Generate dengan multiple controls
output = pipe(
    prompt="a modern house",
    image=[canny_image, depth_image],
    controlnet_conditioning_scale=[0.5, 0.8]
).images[0]
```

## IP-Adapter

### Konsep

IP-Adapter (Image Prompt Adapter) use image sebagai prompt:
- Image-to-image prompting
- Style transfer
- Maintain style consistency
- Complement text prompts

**Usage:**
```python
from diffusers import StableDiffusionPipeline
from ip_adapter import IPAdapter

# Load pipeline
pipe = StableDiffusionPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    torch_dtype=torch.float16
).to("cuda")

# Load IP-Adapter
ip_adapter = IPAdapter(pipe, "ip-adapter_sd15.bin")

# Generate dengan image prompt
style_image = Image.open("style_reference.jpg")
output = ip_adapter.generate(
    prompt="a beautiful landscape",
    image=style_image,
    scale=0.7  # How much follow image style
)
```

## Advanced Techniques

### 1. Latent Couple

Divide canvas untuk different prompts:
```python
# Example: separate prompts untuk different regions
# "left side: a cat | right side: a dog"
```

### 2. Regional Prompter

Apply different prompts ke different regions:
- More precise control
- Complex compositions
- Available in some WebUIs

### 3. AnimateDiff

Animate Stable Diffusion outputs:
- Motion modules
- Text-to-video
- Consistent style across frames

### 4. Depth-Aware Generation

Use depth information untuk 3D-aware generation:
- Better spatial understanding
- Consistent perspectives
- More realistic results

## Model Optimization

### 1. Quantization

Reduce model precision untuk smaller size:
```python
from optimum.onnxruntime import ORTStableDiffusionPipeline

# Load quantized model
pipe = ORTStableDiffusionPipeline.from_pretrained(
    "model_id",
    export=True,
    provider="CPUExecutionProvider"
)
```

### 2. Model Pruning

Remove unnecessary weights:
- Smaller model size
- Faster inference
- Minimal quality loss

### 3. ONNX Export

Convert untuk optimized inference:
```bash
python convert_stable_diffusion_checkpoint_to_onnx.py \
  --model_path="model.safetensors" \
  --output_path="./onnx_model"
```

## Deployment

### 1. API Server

**FastAPI Example:**
```python
from fastapi import FastAPI
from pydantic import BaseModel
import torch
from diffusers import StableDiffusionPipeline
import io
import base64

app = FastAPI()

# Load model at startup
pipe = StableDiffusionPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    torch_dtype=torch.float16
).to("cuda")

class GenerateRequest(BaseModel):
    prompt: str
    negative_prompt: str = ""
    steps: int = 30
    guidance_scale: float = 7.5

@app.post("/generate")
def generate(request: GenerateRequest):
    image = pipe(
        prompt=request.prompt,
        negative_prompt=request.negative_prompt,
        num_inference_steps=request.steps,
        guidance_scale=request.guidance_scale
    ).images[0]
    
    # Convert to base64
    buffer = io.BytesIO()
    image.save(buffer, format="PNG")
    img_str = base64.b64encode(buffer.getvalue()).decode()
    
    return {"image": img_str}
```

### 2. Gradio Interface

```python
import gradio as gr

def generate_image(prompt, negative_prompt, steps, guidance):
    image = pipe(
        prompt=prompt,
        negative_prompt=negative_prompt,
        num_inference_steps=steps,
        guidance_scale=guidance
    ).images[0]
    return image

demo = gr.Interface(
    fn=generate_image,
    inputs=[
        gr.Textbox(label="Prompt"),
        gr.Textbox(label="Negative Prompt"),
        gr.Slider(10, 100, 30, label="Steps"),
        gr.Slider(1, 20, 7.5, label="Guidance Scale")
    ],
    outputs=gr.Image(label="Generated Image")
)

demo.launch()
```

### 3. Batch Processing

```python
def batch_generate(prompts, batch_size=4):
    results = []
    for i in range(0, len(prompts), batch_size):
        batch = prompts[i:i+batch_size]
        images = pipe(batch, num_images_per_prompt=1).images
        results.extend(images)
    return results

# Usage
prompts = ["prompt1", "prompt2", "prompt3", "prompt4"]
images = batch_generate(prompts)
```

## Best Practices

### 1. Training
- **Dataset quality > quantity**
- Start dengan pre-trained weights
- Monitor training loss
- Use validation set
- Regular checkpoints
- Test on diverse prompts

### 2. Prompting
- Be specific dan descriptive
- Use style references
- Iterate dan refine
- Document successful prompts
- Build prompt library

### 3. Resource Management
- Use FP16 untuk memory
- Batch when possible
- Cache models
- Clear CUDA cache regularly
- Monitor GPU usage

### 4. Quality Control
- Generate multiple samples
- Use consistent seed untuk comparison
- A/B testing
- User feedback loop

## Common Issues & Solutions

### Issue: Model Overfit
**Symptoms:** Only generate training images
**Solutions:**
- Less training steps
- More diverse training data
- Lower learning rate
- Use prior preservation (DreamBooth)

### Issue: Style Bleeding
**Symptoms:** Model forget original capabilities
**Solutions:**
- Use LoRA instead of full fine-tune
- Lower learning rate
- More class images
- Regularization

### Issue: CUDA Out of Memory
**Solutions:**
- Reduce batch size
- Enable attention slicing
- Use gradient checkpointing
- Lower resolution
- FP16 precision

### Issue: Inconsistent Results
**Solutions:**
- Use fixed seed
- Consistent prompting
- LoRA untuk style consistency
- Higher guidance scale

## Tools & Resources

### Training Tools
- **Kohya_ss:** GUI for training LoRA/DreamBooth
- **Diffusers:** Official Hugging Face library
- **Automatic1111:** WebUI dengan training extensions

### Model Repositories
- **Hugging Face:** Hub for models
- **Civitai:** Community models
- **LoRA Library:** Curated LoRAs

### Utilities
- **Real-ESRGAN:** Upscaling
- **ControlNet Models:** Various controls
- **CLIP Interrogator:** Reverse prompting

## Key Takeaways

1. **Textual Inversion:** lightweight, train token only
2. **DreamBooth:** powerful, full fine-tuning
3. **LoRA:** efficient, stackable, recommended
4. **ControlNet:** spatial control, various conditions
5. **Training quality depends on dataset**
6. **Optimization crucial untuk deployment**
7. **Experimentation essential** untuk best results
8. **Monitor resources** dan performance
9. **Community resources** sangat valuable
10. **Stay updated** - teknologi berkembang cepat
