---
title: "Types of AI Models"
teaching: 10
exercises: 5
---

:::::::::::::::::::::::::::::::::::::: questions

- What types of AI models are available for research?
- What are the computational requirements for different model types?
- How do open-source models compare to proprietary ones?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Distinguish between LLMs, vision models, and scientific ML models
- Understand model sizes, quantization, and GPU requirements
- Compare open-source models available on Sagehen with cloud alternatives

::::::::::::::::::::::::::::::::::::::::::::::

## Large Language Models in Detail

### Open-Source Models for Sagehen

**Llama 2/3 (Meta):**
- Sizes: 7B, 13B, 70B parameters
- 7B fits on V100 (16GB); 70B requires A100 (80GB)
- License: Llama Community License (free for research)
- Quality comparable to GPT-3.5 on many tasks

**Mistral 7B:**
- Faster than Llama 2 7B with better instruction-following
- Fits on a single V100 or L40S
- Good balance of speed and quality

**Phi-2 (Microsoft):**
- Only 2.7B parameters -- runs on CPU if needed
- Surprisingly capable for its size
- Best for quick prototyping and low-resource scenarios

### Quantization: Making Models Smaller

Quantization reduces model precision to use less GPU memory:

- **32-bit (full precision)**: Best quality, most memory
- **16-bit (half precision)**: Negligible quality loss, half the memory
- **4-bit quantization**: 75% memory reduction, slight quality trade-off

For many research tasks (summarization, classification), the difference
between 16-bit and 4-bit is negligible. Always test on your actual data.

## Vision and Multimodal Models

| Model | Task | VRAM | Notes |
|-------|------|------|-------|
| YOLO | Object detection | 4-8GB | Fast, real-time capable |
| Stable Diffusion 1.5 | Image generation | 4GB | Text-to-image |
| Stable Diffusion XL | Image generation | 24GB | Higher quality |
| CLIP | Image-text matching | 4GB | Zero-shot classification |
| SAM (Meta) | Image segmentation | 8-16GB | Segment anything |

## Scientific ML Models

**Protein science:** AlphaFold, ESMFold -- predict 3D protein structure from
amino acid sequences. Often require 20-100GB+ memory.

**Climate and earth science:** FourCastNet, climate downscaling models,
satellite imagery analysis. Require massive datasets and sometimes distributed
training across multiple GPUs.

**Genomics:** Language models trained on protein/RNA sequences, disease
prediction models. Often require specialized data preprocessing.

## Choosing the Right Model

::::::::::::::::::::::::::::::::::::: callout

## Start Small, Scale Up

Begin with the smallest model that meets your quality needs:

1. Try Phi 2.7B or Mistral 7B first
2. If quality is insufficient, scale to Llama 13B
3. Use Llama 70B only when smaller models clearly underperform
4. Always use 4-bit quantization unless you need full precision

This conserves GPU resources and reduces queue wait times on Sagehen.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Model Selection for Research Tasks

For each scenario, recommend a model type and size:

1. Summarizing 50 published research papers
2. Classifying sentiment in 10,000 customer reviews (proprietary data)
3. Detecting objects in microscopy images
4. Generating Python code to analyze a public dataset

::::::::::::::::::::::::::::::::::::: solution

## Solution

1. **Summarizing papers (public data):** Cloud AI (ChatGPT/Claude) is fine
   since papers are public. If you prefer local: Mistral 7B on V100.
2. **Sentiment classification (proprietary):** Local AI required. BERT
   (fine-tuned) or Mistral 7B on V100. Batch processing is efficient.
3. **Object detection in microscopy:** YOLO or ResNet on V100/L40S depending
   on image resolution and batch size.
4. **Code generation (public data):** Cloud AI is acceptable. Locally: Llama
   13B or Mistral 7B give good code generation results.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- LLMs range from 2.7B (Phi) to 70B+ (Llama) parameters with varying quality
- Quantization (4-bit) reduces memory by 75% with minimal quality loss
- Vision models (YOLO, Stable Diffusion) and scientific ML have specialized uses
- Start with the smallest model that meets your quality threshold and scale up

::::::::::::::::::::::::::::::::::::::::::::::
