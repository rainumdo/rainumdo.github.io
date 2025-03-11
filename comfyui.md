
# 19 
Upscale Model  
VAE Decode + Upscale Image + Load Upscale Model 

# 18 
workflows  
[openart.ai](https://openart.ai/workflows/home)

# 17 
ComfyUI-Manager
```
cd ComfyUI/custom_nodes
git clone https://github.com/ltdrdata/ComfyUI-Manager.git
```

# 16 
[prompthero](https://prompthero.com/)

# 15 
Lora
CheckpointLoaderSimple + Load Lora 

# 14 
[ComfyUI docs](https://blenderneko.github.io/ComfyUI-docs/)

# 13 
Inpainting  
MASK Image + VAE Encode(for Inpainting)  
Load Image + Pad Image for Outpainting + VAE Encode  

sampler_name: uni_pc_bh2  

# 12 
Img2Img  
Load Image + VAE Encode   

# 11 
2 Pass Txt2Img (Hires fix)  
Upscale Latent + KSampler(Hires fix)
VAE Decode: decode the LATENT where you need.  
Upscale Latent: hires fix with higher resolution.  
The second KSampler has more steps and lower denoise(0.5).  

# 10 
[hugging-face models](https://www.hugging-face.org/models/)
[mirror](https://hf-mirror.com/models)
[civitai](https://civitai.com/)

# 9 
When sampling, the negative prompt is treated almost the same way as the positive prompt.

# 8 
Negative prompts are used to tell Stable Diffusion what you don't want in your image.

# 7 
Safety Filter is wasting precious vram and processing power while throw false positive.

# 6
Prompt Engineering
(word: 1.1) is default
not naked
keep prompts simple

# 5
The VAE is used to translate an image from latent space to pixel space.
Latent space is the format the main Stable Diffusion MODEL understand while pixel space is the format that your image viewer understands.

# 4
In stable diffusion the image is generated using what we refer to as a sampler.
It also takes both the positive and negative prompts encoded by the CLIP model.
The final input it takes is Latent Image.

# 3
The CLIP model connects to the CLIPTextEncode nodes.
The CLIP is used in Stable Diffusion to encode the text to a format that the main MODEL can understand.

# 2
CLIP, the main MODEL, VAE  
CheckpointLoaderSimple

# 1
[ComfyUI Examples](https://comfyanonymous.github.io/ComfyUI_examples/)


