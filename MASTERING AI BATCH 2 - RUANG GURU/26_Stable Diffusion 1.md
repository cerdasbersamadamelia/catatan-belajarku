# Stable Diffusion - Part 1

## Apa itu Stable Diffusion?

Stable Diffusion adalah model AI generative yang bisa membuat gambar dari text prompt (text-to-image). Ini adalah salah satu model generative yang paling populer dan powerful.

**Karakteristik:**
- Open source
- Bisa run di consumer hardware (GPU 4-8GB)
- High quality results
- Fast generation (2-10 detik per image)
- Flexible dan customizable

## Generative AI

### Jenis Generative Models

**1. GANs (Generative Adversarial Networks):**
- Generator vs Discriminator
- Training unstable
- High quality tapi susah train

**2. VAE (Variational Autoencoder):**
- Encode dan decode
- Smooth latent space
- Output agak blurry

**3. Diffusion Models:**
- Add noise step by step
- Reverse process untuk generate
- Stable training
- High quality

## Bagaimana Diffusion Models Bekerja?

### Forward Process (Add Noise)

**Step by step add noise:**
```
Clean Image → Slightly Noisy → More Noisy → ... → Pure Noise
```
- Gaussian noise ditambahkan gradually
- 1000 steps (default)
- Markov chain process

### Reverse Process (Denoise)

**Remove noise step by step:**
```
Pure Noise → Less Noisy → ... → Slightly Noisy → Clean Image
```
- Neural network predict noise
- Subtract predicted noise
- Kondisikan dengan text prompt

**Key Insight:**
Kalau kita bisa predict noise yang ditambahkan, kita bisa reverse process dan generate image dari noise.

## Stable Diffusion Architecture

### Components

**1. Text Encoder (CLIP):**
- Convert text prompt ke embedding
- CLIP model (Contrastive Language-Image Pre-training)
- Output: 77 token embeddings

**2. U-Net:**
- Core model untuk denoise
- Input: noisy latent + text embedding + timestep
- Output: predicted noise
- Attention mechanisms

**3. VAE (Variational Autoencoder):**
- Encoder: image → latent space (compression)
- Decoder: latent → image (decompression)
- 8x compression (512x512 → 64x64 latent)

**4. Scheduler:**
- Control denoising steps
- Different strategies (DDPM, DDIM, etc)
- Trade-off speed vs quality

### Latent Diffusion

**Kenapa latent space?**
- Computational efficiency
- Work di latent space (64x64) bukan pixel space (512x512)
- 64x lebih cepat!
- Memory efficient

**Process:**
```
Text → CLIP Encoder → Text Embeddings
Random Noise (64x64x4)
   ↓
U-Net Denoising (50 steps, guided by text)
   ↓
Latent Representation
   ↓
VAE Decoder
   ↓
Output Image (512x512x3)
```

## Installation dan Setup

### 1. Diffusers Library (Recommended)

**Installation:**
```bash
pip install diffusers transformers accelerate
```

**Basic Usage:**
```python
from diffusers import StableDiffusionPipeline
import torch

# Load model
model_id = "runwayml/stable-diffusion-v1-5"
pipe = StableDiffusionPipeline.from_pretrained(
    model_id,
    torch_dtype=torch.float16  # Use FP16 untuk save memory
)
pipe = pipe.to("cuda")

# Generate image
prompt = "a beautiful sunset over mountains, oil painting style"
image = pipe(prompt).images[0]

# Save
image.save("output.png")
```

### 2. Automatic1111 WebUI

**Installation:**
```bash
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
cd stable-diffusion-webui
# Windows: run webui-user.bat
# Linux: ./webui.sh
```

**Features:**
- User-friendly interface
- Banyak extensions
- Model management
- Inpainting, outpainting
- ControlNet integration

## Prompting

### Prompt Engineering

**Good prompt components:**
1. **Subject:** what you want
2. **Style:** art style, medium
3. **Quality modifiers:** high quality, detailed, 4K
4. **Lighting:** dramatic lighting, soft light
5. **Composition:** close-up, wide angle

**Example:**
```
"portrait of a young woman, long red hair, green eyes, 
oil painting style, dramatic lighting, highly detailed, 
4K, artstation trending, by Greg Rutkowski"
```

### Positive vs Negative Prompts

**Positive Prompt:**
- What you want di image
- Be specific dan descriptive

**Negative Prompt:**
- What you DON'T want
- Common: "blurry, low quality, distorted, deformed"

**Example:**
```python
image = pipe(
    prompt="beautiful landscape with mountains and lake, sunset",
    negative_prompt="ugly, blurry, low quality, distorted, people"
).images[0]
```

### Prompt Weighting

**Syntax:**
- `(text)`: increase weight by 1.1x
- `((text))`: 1.1 * 1.1 = 1.21x
- `[text]`: decrease weight to 0.9x
- `(text:1.5)`: explicit weight

**Example:**
```
"a (beautiful:1.5) woman, [background], ((highly detailed face))"
```

### Prompt Tricks

**1. Artist Styles:**
```
"by Greg Rutkowski" → fantasy digital art
"by Studio Ghibli" → anime style
"by Rembrandt" → classical painting
```

**2. Quality Boosters:**
```
- "highly detailed"
- "8K resolution"
- "trending on artstation"
- "award winning"
- "masterpiece"
```

**3. Composition:**
```
- "close-up portrait"
- "wide angle shot"
- "bird's eye view"
- "symmetrical composition"
```

**4. Lighting:**
```
- "dramatic lighting"
- "soft natural light"
- "golden hour"
- "volumetric lighting"
- "rim lighting"
```

## Generation Parameters

### 1. Steps
- Number of denoising steps
- More steps = better quality (diminishing returns)
- Typical: 20-50 steps
- Trade-off: speed vs quality

```python
image = pipe(prompt, num_inference_steps=30).images[0]
```

### 2. Guidance Scale (CFG)
- Classifier-Free Guidance
- How much follow the prompt
- Low (1-5): creative, less accurate
- Medium (7-10): balanced
- High (10-20): accurate, less creative

```python
image = pipe(prompt, guidance_scale=7.5).images[0]
```

### 3. Seed
- Random seed untuk reproducibility
- Same seed + same prompt = same image
- -1 atau None = random

```python
generator = torch.Generator("cuda").manual_seed(42)
image = pipe(prompt, generator=generator).images[0]
```

### 4. Image Size
- Typical: 512x512 (trained size)
- Larger sizes: 768x768, 1024x1024
- Must be multiple of 64
- Larger = more VRAM

```python
image = pipe(prompt, height=768, width=768).images[0]
```

### 5. Batch Size
- Generate multiple images at once
- Faster than sequential
- More VRAM needed

```python
images = pipe(prompt, num_images_per_prompt=4).images
```

## Advanced Techniques

### 1. Image-to-Image

**Concept:** Start dari existing image, modify berdasarkan prompt

```python
from diffusers import StableDiffusionImg2ImgPipeline
from PIL import Image

# Load pipeline
pipe = StableDiffusionImg2ImgPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    torch_dtype=torch.float16
).to("cuda")

# Load init image
init_image = Image.open("input.jpg").resize((512, 512))

# Generate
prompt = "a fantasy landscape, oil painting"
image = pipe(
    prompt=prompt,
    image=init_image,
    strength=0.75  # 0-1, how much change
).images[0]
```

**Strength parameter:**
- 0.0: no change
- 0.3-0.5: subtle changes
- 0.7-0.8: significant changes
- 1.0: basically text-to-image

### 2. Inpainting

**Concept:** Edit specific part of image

```python
from diffusers import StableDiffusionInpaintPipeline

pipe = StableDiffusionInpaintPipeline.from_pretrained(
    "runwayml/stable-diffusion-inpainting",
    torch_dtype=torch.float16
).to("cuda")

# Load image dan mask
image = Image.open("input.jpg").resize((512, 512))
mask = Image.open("mask.jpg").resize((512, 512))  # White = inpaint area

# Generate
result = pipe(
    prompt="a red car",
    image=image,
    mask_image=mask
).images[0]
```

### 3. Outpainting

**Concept:** Extend image beyond borders

**Process:**
1. Resize image dengan padding
2. Create mask untuk extended area
3. Inpaint dengan appropriate prompt

### 4. Upscaling

**Real-ESRGAN untuk upscaling:**
```bash
pip install realesrgan
```

```python
from realesrgan import RealESRGAN

# Initialize
upsampler = RealESRGAN('cuda', scale=4)

# Upscale
upscaled = upsampler.predict(image)
```

## Model Variants

### 1. Stable Diffusion v1.5
- Original widely-used version
- 512x512 default
- Good balance

### 2. Stable Diffusion v2.1
- Better quality
- 768x768 default
- Different prompt style

### 3. Stable Diffusion XL (SDXL)
- Latest version
- 1024x1024 native
- Higher quality
- More detailed
- Need more VRAM (10GB+)

### 4. Specialized Models

**DreamBooth models:**
- Fine-tuned untuk specific subjects
- "a photo of sks person"

**LoRA models:**
- Lightweight adaptations
- Mix multiple LoRAs
- Change style atau add concepts

**Community models:**
- Realistic Vision (photorealistic)
- Anything V5 (anime)
- OpenJourney (MidJourney style)

## Practical Tips

### 1. Memory Optimization

**Enable attention slicing:**
```python
pipe.enable_attention_slicing()
```

**Enable VAE slicing:**
```python
pipe.enable_vae_slicing()
```

**Use CPU offload:**
```python
pipe.enable_sequential_cpu_offload()
```

**FP16:**
```python
pipe = pipe.to("cuda", torch_dtype=torch.float16)
```

### 2. Speed Optimization

**Use xformers:**
```bash
pip install xformers
```
```python
pipe.enable_xformers_memory_efficient_attention()
```

**Compile model (PyTorch 2.0+):**
```python
pipe.unet = torch.compile(pipe.unet)
```

### 3. Quality Tips

- **Be specific** dalam prompt
- **Experiment** dengan different seeds
- **Use negative prompts** untuk avoid common issues
- **Adjust guidance scale** sesuai needs
- **More steps** untuk complex prompts
- **Upscale** untuk final output

### 4. Common Issues

**Issue: Deformed hands/faces**
- Solution: Use "deformed, bad anatomy" in negative prompt
- Use inpainting untuk fix
- Generate multiple dan pilih yang best

**Issue: Blurry output**
- Increase steps (30-50)
- Add "highly detailed, sharp focus" to prompt
- Upscale dengan Real-ESRGAN

**Issue: Not following prompt**
- Increase guidance scale (10-15)
- Be more specific in prompt
- Use prompt weighting

**Issue: Out of memory**
- Reduce image size
- Enable attention slicing
- Use CPU offload
- Reduce batch size

## Key Takeaways

1. **Stable Diffusion = text-to-image** via diffusion process
2. **Latent diffusion** untuk efficiency
3. **Prompt engineering penting** untuk good results
4. **Parameters:** steps, guidance scale, seed, size
5. **Advanced techniques:** img2img, inpainting, outpainting
6. **Many model variants** untuk different use cases
7. **Optimization crucial** untuk consumer hardware
8. **Experiment dan iterate** untuk best results
