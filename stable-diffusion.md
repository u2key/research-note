# How to Execute Stable Diffusion with Python3

## 1. Image Generation from Another Image and Text

```img2img.py
import os
commandline_args = os.environ.get('COMMANDLINE_ARGS', "")
commandline_args = os.environ.get('COMMANDLINE_ARGS', "--skip-torch-cuda-test --no-half")
import sys
from datetime import datetime
from diffusers import StableDiffusionImg2ImgPipeline, DPMSolverMultistepScheduler
import torch
import base64
import io
import requests
from PIL import Image
from IPython.display import display

if len(sys.argv) > 1 and (sys.argv[1] == "cpu" or sys.argv[1] == "cuda"):
  device = sys.argv[1]
else:
  device = "cuda"

strength = 0.75
init_imgs_raw = []
init_imgs_base64_path = []
init_imgs_base64_path.append("./sampple.b64")
init_imgs_url = []
init_imgs_url.append("http://localhost/sample.jpg")

for init_img_base64_path in init_imgs_base64_path:
  with open(init_img_base64_path) as f:
    init_imgs_raw.append(base64.b64decode(f.read()))
for init_img_url in init_imgs_url:
  init_imgs_raw.append(requests.get(init_img_url).content)
  
prompts = []
for i in range(len(init_imgs_raw)):
  prompts.append("forest,real picture")

model_id = []
model_id.append("Lykon/NeverEnding-Dream")
model_id.append("CompVis/stable-diffusion-v1-4")
model_id.append("Meina/MeinaMix_V10")
pipeline = StableDiffusionImg2ImgPipeline.from_pretrained(model_id[1], torch_dtype=torch.float32, safety_checker=None)
pipeline.scheduler = DPMSolverMultistepScheduler.from_config(pipeline.scheduler.config)
pipeline = pipeline.to(device)
init_images = []
for init_img_raw in init_imgs_raw: 
  init_images.append(Image.open(io.BytesIO(init_img_raw)))
gen_images = pipeline(prompt=prompts, image=init_images, strength=strength, num_inference_steps=50).images
for i, gen_image in enumerate(gen_images):
  time_stamp = datetime.now().strftime('%Y%m%d%H%M%S')
  gen_image.save(f'{time_stamp}_{i}.png')
```

## 2. Image Generation from Text

```txt2img.py
import os
commandline_args = os.environ.get('COMMANDLINE_ARGS', "")
commandline_args = os.environ.get('COMMANDLINE_ARGS', "--skip-torch-cuda-test --no-half")
import sys
import random
from datetime import datetime
import torch
from diffusers import StableDiffusionPipeline
from IPython.display import display

if len(sys.argv) > 1 and (sys.argv[1] == "cpu" or sys.argv[1] == "cuda"):
  device = sys.argv[1]
else:
  device = "cuda"

model_id = []
model_id.append("Lykon/NeverEnding-Dream")
model_id.append("CompVis/stable-diffusion-v1-4")
model_id.append("Meina/MeinaMix_V10")

n_gen = 3
pos_prompt = "forest,real picture"
neg_prompt = "text"
pos_prompts = [pos_prompt for _ in range(n_gen)]
neg_prompts = [neg_prompt for _ in range(n_gen)]

generator = torch.Generator(device).manual_seed(random.randrange(65535)))

pipe = StableDiffusionPipeline.from_pretrained(model_id[0], torch_dtype=torch.float32, safety_checker=None).to(device)
gen_images = pipe(pos_prompts, negative_prompt=neg_prompts, generator=generator, guidance_scale=7.5, num_inference_steps=50, height=512, width=512).images
for i, gen_image in enumerate(gen_images):
  time_stamp = datetime.now().strftime('%Y%m%d%H%M%S')
  gen_image.save(f'{time_stamp}_{i}.png')
```
