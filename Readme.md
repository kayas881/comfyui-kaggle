# ComfyUI on Kaggle - Universal Workflow Runner

Run **any** ComfyUI workflow on Kaggle's free GPU (Tesla T4 with 15GB VRAM) using this notebook!

## 🚀 Quick Start

1. Upload the notebook to Kaggle
2. Enable GPU: **Settings → Accelerator → GPU T4 x2**
3. Enable Internet: **Settings → Internet → On**
4. Run all cells
5. Access ComfyUI through the Pinggy URL (printed after Cell 3)

## 📦 What's Included

This notebook provides:
- ✅ Full ComfyUI installation with Manager
- ✅ Storage optimization (uses `/tmp` for large files)
- ✅ Public URL via Pinggy tunnel (no ngrok needed)
- ✅ Support for **any workflow** - not limited to specific models!

## 🎯 Using Your Own Workflows

### Default Setup (Wan 2.1 Chrono Edit)
The notebook comes pre-configured with Wan 2.1 models as an example, but you can easily swap these for any models you need.

### Customizing Models (Cell 4)

**Cell 4** is where model downloads happen. Here's how to modify it for your workflow:

#### 1. Identify Your Model Requirements
First, check what models your ComfyUI workflow needs. Common model types:
- `diffusion_models/` - Main models (SD1.5, SDXL, Flux, etc.)
- `text_encoders/` - CLIP, T5, etc.
- `vae/` - VAE models
- `loras/` - LoRA files
- `clip_vision/` - CLIP vision models
- `controlnet/` - ControlNet models
- `upscale_models/` - Upscalers (ESRGAN, etc.)

#### 2. Find Model URLs on Hugging Face
Most models are hosted on Hugging Face. Get direct download links:
```
https://huggingface.co/[org]/[repo]/resolve/main/[file].safetensors
```

#### 3. Modify Cell 4
Replace the `downloads` list with your models:

```python
# Example: SDXL Workflow
downloads = [
    # SDXL Base Model
    ("https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/resolve/main/sd_xl_base_1.0.safetensors",
     f"{DIRS['diffusion_models']}/sd_xl_base_1.0.safetensors"),
    
    # SDXL VAE
    ("https://huggingface.co/stabilityai/sdxl-vae/resolve/main/sdxl_vae.safetensors",
     f"{DIRS['vae']}/sdxl_vae.safetensors"),
    
    # Your favorite LoRA
    ("https://huggingface.co/your-org/your-lora/resolve/main/lora.safetensors",
     f"{DIRS['loras']}/your_lora.safetensors"),
]
```

#### 4. Add More Directories If Needed
If your workflow needs controlnet or upscale models:

```python
DIRS = {
    "text_encoders": f"{BASE}/text_encoders",
    "clip_vision": f"{BASE}/clip_vision",
    "loras": f"{BASE}/loras",
    "diffusion_models": f"{BASE}/diffusion_models",
    "vae": f"{BASE}/vae",
    "controlnet": f"{BASE}/controlnet",      # Add this
    "upscale_models": f"{BASE}/upscale_models",  # Add this
}
```

Then add downloads for those directories:

```python
downloads = [
    # ... your other models ...
    
    # ControlNet
    ("https://huggingface.co/lllyasviel/ControlNet-v1-1/resolve/main/control_v11p_sd15_canny.pth",
     f"{DIRS['controlnet']}/control_v11p_sd15_canny.pth"),
    
    # Upscaler
    ("https://huggingface.co/ai-forever/Real-ESRGAN/resolve/main/RealESRGAN_x4.pth",
     f"{DIRS['upscale_models']}/RealESRGAN_x4.pth"),
]
```

## 💾 Storage Notes

- Kaggle provides ~73GB disk space, but most is in `/tmp`
- The notebook automatically symlinks `models/`, `input/`, and `output/` to `/tmp`
- Large models (30GB+) work fine with this setup
- Downloads resume automatically if interrupted

## 🔧 Troubleshooting

### "Out of VRAM" errors
- T4 has 15GB VRAM - some huge models may not fit
- Try enabling `--lowvram` in Cell 3 if needed

### Models not loading
- Check the Hugging Face URL is correct
- Ensure the file path in Cell 4 matches the model type
- Look at ComfyUI console output for missing files

### Pinggy URL expires
- Pinggy free tier gives you a new URL each session
- URL changes every time you restart the notebook
- For permanent URLs, consider ngrok or paid Pinggy

## 📝 Tips

- **Save your workflow**: Download your workflow JSON before closing Kaggle
- **Upload images**: Use the ComfyUI upload button to add input images
- **Download outputs**: Generated images are in the output folder
- **Session limit**: Kaggle sessions last ~9 hours max

## 🆘 Need Help?

1. Check ComfyUI Manager for missing nodes
2. Review the console output for error messages  
3. Verify your model downloads completed (check file sizes)
4. Make sure your workflow is compatible with the models you downloaded

---

**Note**: This notebook uses Pinggy for tunneling (no account required). The free tier provides temporary URLs. For production use, consider setting up your own tunnel solution.
