# Comfy Couple

## What is

This is simple custom node for [**ComfyUI**](https://github.com/comfyanonymous/ComfyUI) which helps to generate images of actual _couples_, easier.

If you want to draw two different characters together without blending their features, so you could try to check out this custom node.

| ⭕ with Comfy Couple | ❌ without Comfy Couple  |
| --- | --- |
| ![Ayaka x Lumine](docs/images/ayalumi-comfy-couple.png) | ![Ayaka x OC](docs/images/ayalumi-plain.png) |
| _Lumine with her own hair style_ | _Lumine with hair style of Ayaka_ |

It's fork of [**laksjdjf/attention-couple-ComfyUI**](https://github.com/laksjdjf/attention-couple-ComfyUI), but implementing shortcut for the most of required nodes.

# ComfyUI-ComfyCouple

This repository contains custom nodes and workflows for ComfyUI, a powerful tool for generating and manipulating images using machine learning models. The primary focus of this repository is to provide a workflow for generating couple images using the ComfyCouple node.

## Installation

To install the ComfyCouple custom nodes, follow these steps:

1. Navigate to the `custom_nodes` directory within your ComfyUI installation:
   ```sh
   cd ComfyUI/custom_nodes
   ```

2. Clone the repository:
   ```sh
   git clone https://github.com/Danand/ComfyUI-ComfyCouple.git
   ```

3. Restart your ComfyUI script to load the new custom nodes.

## Workflow Example

### Transpiling the Workflow

To use the provided workflow, you need to transpile the `workflow-comfy-couple.json` file:

1. Download the workflow file from the repository:
   ```sh
   wget https://github.com/Danand/ComfyUI-ComfyCouple/blob/main/workflows/workflow-comfy-couple.json
   ```

2. Transpile the workflow using the ComfyUI script:
   ```sh
   python -m comfy_script.transpile workflow-comfy-couple.json
   ```

### Running the Workflow

Here is an example of how to run the workflow using Python:

```python
with Workflow():
    model, clip, vae = CheckpointLoaderSimple('meinamix_meinaV11.safetensors')
    model, clip = LoraLoader(model, clip, 'genshinfull1-000006.safetensors', 0.84, 0.84)
    conditioning = CLIPTextEncode('holding hands, sitting on ground, picnic, starry sky, fireworks, (kamisatoayakadef:1.1), tender, cute, heartwarming, blushing, forest lake, scenery, 2girl', clip)
    conditioning2 = CLIPTextEncode('holding hands, sitting on ground, picnic, starry sky, fireworks, (lumine:1.1), tender, cute, heartwarming, blushing, forest lake, scenery, 2girl', clip)
    conditioning3 = CLIPTextEncode('(worst quality, low quality:1.64), (jpeg artifacts:1.4), (1boy, abs, muscular, rib:1.0), greyscale, monochrome, dusty sunbeams, trembling, motion lines, motion blur, emphasis lines, text, title, logo, signature, extra body parts, duplicated body parts, extra limbs, extra body parts, duplication, barcode, overexposed, low detailed', clip)
    model, conditioning4, conditioning5 = ComfyCouple(model, conditioning, conditioning2, conditioning3, 'horizontal', 0.5, 512, 512)
    latent = EmptyLatentImage(512, 512, 1)
    latent = KSampler(model, 41, 30, 6.5, 'dpm_2', 'ddim_uniform', conditioning4, conditioning5, latent, 0.98)
    image = VAEDecode(latent, vae)
    SaveImage(image, 'ComfyUI')
```

### Downloading Required Models

To run the workflow, you need to download the following models:

- `genshinfull1-000006.safetensors` from [CivitAI](https://civitai.com/models/108649?modelVersionId=116970)
- `meinamix_meinaV11.safetensors` from [Hugging Face](https://huggingface.co/NickYuan/sd_models/tree/main/models/Stable-diffusion)

You can download `meinamix_meinaV11.safetensors` using the following command:

```sh
wget https://cdn-lfs.hf.co/repos/f8/a8/f8a84486fb0223a6b9070720f177c070c3d20cd1b3e14eb1b006b06e5b2bcf12/54ef3e3610eee7f0f6c9f9b82eee26aa404598512d800494d1db344e3bc560b1?response-content-disposition=attachment%3B+filename*%3DUTF-8%27%27meinamix_meinaV11.safetensors%3B+filename%3D%22meinamix_meinaV11.safetensors%22%3B&Expires=1730983757&Policy=eyJTdGF0ZW1lbnQiOlt7IkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTczMDk4Mzc1N319LCJSZXNvdXJjZSI6Imh0dHBzOi8vY2RuLWxmcy5oZi5jby9yZXBvcy9mOC9hOC9mOGE4NDQ4NmZiMDIyM2E2YjkwNzA3MjBmMTc3YzA3MGMzZDIwY2QxYjNlMTRlYjFiMDA2YjA2ZTViMmJjZjEyLzU0ZWYzZTM2MTBlZWU3ZjBmNmM5ZjliODJlZWUyNmFhNDA0NTk4NTEyZDgwMDQ5NGQxZGIzNDRlM2JjNTYwYjE%7EcmVzcG9uc2UtY29udGVudC1kaXNwb3NpdGlvbj0qIn1dfQ__&Signature=eGyaQIL8vDYjuZpXiM4Uj5RcCeoBNVcxvEzUWlecRGwkZ4y8cp9MAyzxQ2zlAPHkg7otZ6IDgSzg-O7IYWag8oSdF8lNKox4pxfJPXfQQdCa5JnMs7or451P5itoBovYkAPW5GuuBCydqW4J10QBZhjxcC8xMP226BvpefP1WstwFhYLqjfJTgqA09-VJBFEr3ax%7EHQunwg%7EHqop1DCdiLcsR9qrAFsulSfrWtF87mfz-NXK5a7AcSkk9A6PcEPQOQOacj%7E1uF7riZCjULwykZOHwgHIbDhG0VXt485Mjp%7EmDaymV4ZRuO5%7EqALa3O9pEqbdhelFIfv5VEhGOIqosQ__&Key-Pair-Id=K3RPWS32NSSJCE -O meinamix_meinaV11.safetensors
```

Place the downloaded models in the appropriate directories:

```sh
cp meinamix_meinaV11.safetensors ComfyUI/models/checkpoints
cp genshinfull1-000006.safetensors ComfyUI/models/loras
```

## Single Character Image

To generate a single character image, use the following workflow:

```python
with Workflow():
    model, clip, vae = CheckpointLoaderSimple('meinamix_meinaV11.safetensors')
    model, clip = LoraLoader(model, clip, 'genshinfull1-000006.safetensors', 0.84, 0.84)
    conditioning = CLIPTextEncode('sitting on ground, picnic, starry sky, fireworks, (keqingdef:1.1), tender, cute, heartwarming, blushing, forest lake, scenery', clip)
    conditioning2 = CLIPTextEncode('(worst quality, low quality, extra digits, loli, child, male:1.4)), bad_prompt,', clip)
    latent = EmptyLatentImage(512, 512, 1)
    latent = KSampler(model, 1123713238173417, 20, 8, 'euler', 'normal', conditioning, conditioning2, latent, 1)
    image = VAEDecode(latent, vae)
    SaveImage(image, 'ComfyUI')
```

You can open the generated image using PIL:

```python
from PIL import Image
Image.open("ComfyUI/output/ComfyUI_00006_.png")
```

![keqing (1)](https://github.com/user-attachments/assets/d7410ee9-c073-4dfc-a77b-dd9ade140fda)

## Couple Image

To generate a couple image, use the following workflow:

```python
with Workflow():
    model, clip, vae = CheckpointLoaderSimple('meinamix_meinaV11.safetensors')
    model, clip = LoraLoader(model, clip, 'genshinfull1-000006.safetensors', 0.84, 0.84)
    conditioning = CLIPTextEncode('holding hands, sitting on ground, picnic, starry sky, fireworks, (kamisatoayakadef:1.1), tender, cute, heartwarming, blushing, forest lake, scenery, 2girl', clip)
    conditioning2 = CLIPTextEncode('holding hands, sitting on ground, picnic, starry sky, fireworks, (lumine:1.1), tender, cute, heartwarming, blushing, forest lake, scenery, 2girl', clip)
    conditioning3 = CLIPTextEncode('(worst quality, low quality:1.64), (jpeg artifacts:1.4), (1boy, abs, muscular, rib:1.0), greyscale, monochrome, dusty sunbeams, trembling, motion lines, motion blur, emphasis lines, text, title, logo, signature, extra body parts, duplicated body parts, extra limbs, extra body parts, duplication, barcode, overexposed, low detailed', clip)
    model, conditioning4, conditioning5 = ComfyCouple(model, conditioning, conditioning2, conditioning3, 'horizontal', 0.5, 512, 512)
    latent = EmptyLatentImage(512, 512, 1)
    latent = KSampler(model, 41, 30, 6.5, 'dpm_2', 'ddim_uniform', conditioning4, conditioning5, latent, 0.98)
    image = VAEDecode(latent, vae)
    SaveImage(image, 'ComfyUI')
```

You can open the generated image using PIL:

```python
from PIL import Image
Image.open("ComfyUI/output/ComfyUI_00002_.png")
```

![linghua_ying_couple](https://github.com/user-attachments/assets/f903516b-24cb-42a1-8788-997b880d95e4)

## Additional Workflows

### Workflow for Generating a Single Character Image
#### https://civitai.com/images/31409496

To generate a single character image using the `tPonynai3V65.u99W.safetensors` model, follow these steps:

1. Download the model:
   ```sh
   wget https://civitai-delivery-worker-prod.5ac0637cfd0766c97916cefa3764fbdf.r2.cloudflarestorage.com/model/349177/tPonynai3V65.u99W.safetensors?X-Amz-Expires=86400&response-content-disposition=attachment%3B%20filename%3D%22tPonynai3_v65.safetensors%22&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=e01358d793ad6966166af8b3064953ad/20241104/us-east-1/s3/aws4_request&X-Amz-Date=20241104T135443Z&X-Amz-SignedHeaders=host&X-Amz-Signature=30e8059154d5baa6f57a42ccae1ac47e82847da344d60d738bb43e7e660732dc -O tPonynai3V65.u99W.safetensors
   ```

2. Run the following workflow:
   ```python
   with Workflow():
       model, clip, vae = CheckpointLoaderSimple('tPonynai3V65.u99W.safetensors')
       clip = CLIPSetLastLayer(clip, -2)
       model, clip = LoraLoader(model, clip, 'mondstadt_background-ponyxl-v11h.safetensors', 1, 1)
       model, clip = LoraLoader(model, clip, 'Collei_v1-000010.safetensors', 1, 1)
       conditioning = CLIPTextEncode('score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up, source_anime, official art, (1girl, solo, looking at viewer, cowboy shot:1.2),  <lora:mondstadt_background-ponyxl-v11h:0.8>, field, mondstadt, outdoors, scenery, landscape, sky, cloud, nature, tree, grass, rock, bush, water, mountain,   <lora:Collei_v1-000010:0.9>, colleidef, green hair, medium hair, purple eyes, hair ornament, hair between eyes, ahoge, small breasts, earrings, single earring, jewelry, green capelet, black dress, detached sleeves, puffy long sleeves, backless dress, bridal gauntlets, crossed bangs', clip)
       conditioning2 = CLIPTextEncode('score_4, score_5, score_6, source_pony, source_furry, source_cartoon, render, fewer digits, extra digits, jpeg artifacts, watermark, greyscale, censorship, amputee, no humans, 3d, giantess', clip)
       latent = EmptyLatentImage(1024, 1024, 1)
       latent = KSampler(model, 1123713238173417, 50, 5, 'euler', 'normal', conditioning, conditioning2, latent, 1)
       image = VAEDecode(latent, vae)
       SaveImage(image, 'ComfyUI')
   ```
![kelai (1)](https://github.com/user-attachments/assets/e55d140f-f461-4793-b900-e9f9cc3fff8b)


### Workflow for Generating a Background Image
#### https://civitai.com/images/31409753

To generate a background image using the `autismmixASB1.s0JN.safetensors` model, follow these steps:

1. Download the model:
   ```sh
   wget https://civitai-delivery-worker-prod.5ac0637cfd0766c97916cefa3764fbdf.r2.cloudflarestorage.com/model/3048970/autismmixASB1.s0JN.safetensors?X-Amz-Expires=86400&response-content-disposition=attachment%3B%20filename%3D%22autismmixSDXL_autismmixConfetti.safetensors%22&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=e01358d793ad6966166af8b3064953ad/20241104/us-east-1/s3/aws4_request&X-Amz-Date=20241104T142802Z&X-Amz-SignedHeaders=host&X-Amz-Signature=e45d78a9ffff634bfbc90d4e9a155a108b2e455d44c5eda4f11985f596dd7a3c -O autismmixASB1.s0JN.safetensors
   ```

2. Run the following workflow:
   ```python
   with Workflow():
       model, clip, vae = CheckpointLoaderSimple('autismmixASB1.s0JN.safetensors')
       model, clip = LoraLoader(model, clip, 'mondstadt_background-ponyxl-v11h.safetensors', 1, 1)
       conditioning = CLIPTextEncode('source_anime, official art,  <lora:mondstadt_background-ponyxl-v11h:1>, forest, mondstadt, scenery, outdoors, landscape, sky, cloud, nature, tree, grass, rock, bush, plant, flower, leaf, log, river', clip)
       conditioning2 = CLIPTextEncode('source_pony, source_furry, source_cartoon, render, fewer digits, extra digits, jpeg artifacts, watermark, greyscale, censorship, amputee', clip)
       latent = EmptyLatentImage(1024, 1024, 1)
       latent = KSampler(model, 1123713238173417, 50, 8, 'euler', 'normal', conditioning, conditioning2, latent, 1)
       image = VAEDecode(latent, vae)
       SaveImage(image, 'ComfyUI')
   ```

![memgde_background](https://github.com/user-attachments/assets/cc3ef2b3-babd-4ed7-9f84-f4a22b0d7f4d)


This README provides a comprehensive guide to installing and using the ComfyCouple custom nodes for generating couple images in ComfyUI.

## Installation

1. Change directory to custom nodes of **ComfyUI**:

   ```bash
   cd ~/ComfyUI/custom_nodes
   ```

2. Clone this repo here:

   ```bash
   git clone https://github.com/Danand/ComfyUI-ComfyCouple.git
   ```

3. Restart **ComfyUI**.

## Usage

1. Right click in workflow.
2. Choose node: **loaders → Comfy Couple**
3. Connect inputs, connect outputs, notice **two** positive prompts for left side and right side of image respectively.

Example workflow is [here](workflows/workflow-comfy-couple.json).

## Known issues

It **is not** quite actual regional prompting.

## Comparison with [**laksjdjf/attention-couple-ComfyUI**](https://github.com/laksjdjf/attention-couple-ComfyUI)

Mask magic was replaced with comfy shortcut.

| Comfy Couple | attention-couple-ComfyUI |
| --- | --- |
| ![Comfy Couple workflow](docs/images/workflow-comfy-couple.svg) | ![attention-couple-ComfyUI workflow](docs/images/workflow-attention-couple.svg) |

## Credits

- [**@laksjdjf**](https://github.com/laksjdjf) – [original repo](https://github.com/laksjdjf/attention-couple-ComfyUI).
- [**@pythongosssss**](https://github.com/pythongosssss) – [ComfyUI-Custom-Scripts](https://github.com/pythongosssss/ComfyUI-Custom-Scripts) used for capturing SVG for `README.md`
- [**@Meina**](https://civitai.com/user/Meina) – [MeinaMix V11](https://civitai.com/models/7240/meinamix) used in example.
- [**@Numeratic**](https://civitai.com/user/Numeratic) – [Genshin Impact All In One](https://civitai.com/models/108649?modelVersionId=116970) used in example.
