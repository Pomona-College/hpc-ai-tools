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
- Compare open-source models available on Sagehen HPC with cloud alternatives

::::::::::::::::::::::::::::::::::::::::::::::

## Large Language Models in Detail

### Open-Weight Models for Sagehen HPC

Specific models turn over quickly. Choose along three axes that do not:
**licence**, **size**, and whether the download is **gated**.

*The examples below are current as of August 2026.*

**gpt-oss (OpenAI)** — `gpt-oss-20b`, `gpt-oss-120b`

- Apache 2.0, ungated: `pip install` and go, no access request
- 20b runs in about 16 GB; 120b fits a single A100 (80 GB)
- The path of least resistance for workshop work and for publication

**Qwen3 (Alibaba)**

- Apache 2.0, ungated, unusually wide range of sizes
- Strong multilingual coverage — worth knowing if your corpus is not English

**Mistral Small (Mistral AI)**

- Apache 2.0; efficient general-purpose model that fits a single L40S

**Phi-4-mini (Microsoft)**

- MIT licence, about 3.8B parameters
- The smallest thing here that is still useful; good for prototyping a pipeline
  before committing GPU hours

**Gemma 3 (Google)** — multimodal, handles image input. Note the licence is
Google's own Gemma terms, **not** an OSI-approved open-source licence.

**Llama 4 (Meta)** — mixture-of-experts; Scout is 109B total / 17B active.
Requires accepting the Llama Community Licence before download.

::::::::::::::::::::::::::::::::::::: callout

## Prefer Ungated Models Unless You Need Otherwise

A gated model (Llama, among others) means: accept a licence on the model's
page, generate a token, then authenticate before downloading. That is fine for
sustained project work, but it stalls a workshop and adds a licence term you
must honour at publication.

Unless a gated model gives you something you specifically need, start with an
Apache 2.0 or MIT one.

::::::::::::::::::::::::::::::::::::::::::::::

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

1. Try Phi-4-mini or an 8B-class model such as Qwen3 8B first
2. If quality is insufficient, scale to a 20-30B model such as `gpt-oss-20b`
3. Use a 100B-class model only when smaller ones clearly underperform
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

1. **Summarizing papers (public data):** Cloud AI is fine since papers are
   public. If you prefer local: an 8B-class model on an L40S.
2. **Sentiment classification (proprietary):** Local AI required. A fine-tuned
   encoder such as BERT, or an 8B-class instruct model, on an L40S. Batch
   processing is efficient.
3. **Object detection in microscopy:** YOLO or ResNet on L40S depending
   on image resolution and batch size.
4. **Code generation (public data):** Cloud AI is acceptable. Locally,
   `gpt-oss-20b` on an L40S gives good code generation results.

Note that only scenario 2's answer was forced by the *data*. The others were
choices about cost and convenience — which is the normal case.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Useful local LLMs range from about 4B to over 100B parameters
- Choose on licence, size and whether the download is gated -- not on leaderboards
- Quantization (4-bit) reduces memory by 75% with minimal quality loss
- Vision models (YOLO, Stable Diffusion) and scientific ML have specialized uses
- Start with the smallest model that meets your quality threshold and scale up

::::::::::::::::::::::::::::::::::::::::::::::
