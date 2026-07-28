---
title: "The AI Landscape for Researchers"
teaching: 10
exercises: 5
---

:::::::::::::::::::::::::::::::::::::: questions

- What AI tools are available for research and academia?
- Why would you run AI models on HPC clusters instead of cloud services?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Survey the landscape of modern AI tools and models
- Understand why HPC matters for AI-assisted research
- Recognize when cloud vs. local AI is appropriate

::::::::::::::::::::::::::::::::::::::::::::::

## Why HPC for AI Tools?

Many researchers ask: "Why use Sagehen HPC for AI when services like ChatGPT
are so easy to use?"

::::::::::::::::::::::::::::::::::::: callout

## Data Privacy is Non-Negotiable for Restricted Data

Cloud AI services cannot be used with restricted data (FERPA, medical records,
export-controlled research). If your data is protected by law, it must stay on
Pomona infrastructure. This decision is about **compliance**, not preference.

::::::::::::::::::::::::::::::::::::::::::::::

### Reasons to Use HPC for AI

- **Data privacy**: Restricted data (FERPA, medical, financial) cannot go to
  external services
- **Computational power**: Sagehen's 11 GPUs — A100 (80 GB), L40S (48 GB),
  V100 (16 GB), and A6000 (48 GB ECC) — handle production workloads
- **Data locality**: Working with large datasets is faster on local storage
  than uploading to cloud services
- **Reproducibility**: Full control over model versions and environments
- **Cost**: Sagehen HPC is included in lab allocations; cloud GPU costs scale
  rapidly with intensive compute

## The Modern AI Landscape

### Large Language Models (LLMs)

**Cloud-based (proprietary):** ChatGPT (OpenAI), Claude (Anthropic),
Gemini (Google). Per-token pricing; best for quick queries with non-sensitive
data.

**Local (open-source, can run on Sagehen):** Llama 2/3 (Meta), Mistral,
Phi (Microsoft), Qwen. Sizes range from 7B to 70B parameters (15GB-350GB
VRAM). Best for sensitive data, fine-tuning, and cost-effective batch work.

### Computer Vision and Scientific ML

**Vision models** for Sagehen: YOLO (object detection), Stable Diffusion
(image generation), CLIP (image-text understanding), SAM (segmentation).

**Scientific ML**: AlphaFold (protein structure), ESMFold, weather prediction
models, climate downscaling, satellite imagery analysis.

### Model Size and Requirements

| Model | Size | VRAM | Speed |
|-------|------|------|-------|
| Phi 2.7B | 3GB | 8GB | Very Fast |
| Llama 7B (4-bit) | 4GB | 8GB | Fast |
| Mistral 7B (4-bit) | 4GB | 8GB | Very Fast |
| Llama 13B (4-bit) | 8GB | 16GB | Medium |
| Llama 70B (4-bit) | 38GB | 48GB+ | Slower |
| Stable Diffusion XL | 7GB | 24GB | Slower |

::::::::::::::::::::::::::::::::::::: callout

## The AI Tool Decision is Data-Driven

It is tempting to choose AI tools based on capability, but the actual decision
rule is simpler: **what data are you using?** Once you know the data
classification, the tool choice is largely determined.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Match Models to GPUs

For each model below, identify which Sagehen GPU (V100 16 GB, L40S 48 GB,
A6000 48 GB, or A100 80 GB) is the minimum required:

1. Phi 2.7B (5GB VRAM)
2. Llama 13B 4-bit quantized (8GB VRAM)
3. Llama 70B 4-bit quantized (40GB VRAM)
4. Stable Diffusion XL (24GB VRAM)

::::::::::::::::::::::::::::::::::::: solution

## Solution

1. **Phi 2.7B**: V100 (16GB) -- plenty of headroom
2. **Llama 13B 4-bit**: V100 (16GB) -- fits with 8GB used
3. **Llama 70B 4-bit**: A100 (80GB) -- needs 40GB, exceeds L40S for comfort
4. **Stable Diffusion XL**: L40S (48GB) -- needs 24GB, V100 too small

Start with the smallest sufficient GPU and scale up only if needed.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- HPC is essential for sensitive data, large models, and reproducible research
- Cloud AI is easy but sends data externally; local AI keeps data private
- Model size, memory requirements, and accuracy vary widely across AI tools
- Data classification -- not model capability -- should drive tool selection

::::::::::::::::::::::::::::::::::::::::::::::
