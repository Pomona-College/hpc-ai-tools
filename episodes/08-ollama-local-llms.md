---
title: "Ollama and Local LLMs"
teaching: 15
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I set up and run open-source LLMs on Sagehen?
- How do I use Hugging Face models responsibly?
- How do I build a private AI pipeline for restricted data?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Load and run open-source LLMs on Sagehen GPUs
- Use Hugging Face transformers for model inference
- Apply 4-bit quantization to reduce GPU memory usage
- Build a private pipeline for processing sensitive data

::::::::::::::::::::::::::::::::::::::::::::::

## Setting Up Llama on Sagehen

### Step 1: Request Model Access

Llama models require accepting a license:

1. Visit `https://huggingface.co/meta-llama/Llama-2-7b-chat-hf`
2. Click "Request Access" and accept the license
3. Generate a Hugging Face token for authentication

### Step 2: Create Environment and Install

```bash
module load anaconda3
conda create -n llama_env python=3.11
conda activate llama_env
pip install transformers torch accelerate safetensors bitsandbytes
```

### Step 3: Run Model Inference

```python
import torch
from transformers import AutoTokenizer, AutoModelForCausalLM

model_name = "meta-llama/Llama-2-7b-chat-hf"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(
    model_name,
    torch_dtype=torch.float16,
    device_map="auto"
)

prompt = "Explain photosynthesis in simple terms."
inputs = tokenizer(prompt, return_tensors="pt").to("cuda")
outputs = model.generate(**inputs, max_length=200)
print(tokenizer.decode(outputs[0], skip_special_tokens=True))
```

## Using 4-bit Quantization

Quantization reduces memory by 75%, fitting larger models on smaller GPUs:

```python
from transformers import BitsAndBytesConfig, AutoModelForCausalLM

bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_use_double_quant=True,
    bnb_4bit_quant_type="nf4",
    bnb_4bit_compute_dtype=torch.bfloat16
)

model = AutoModelForCausalLM.from_pretrained(
    "meta-llama/Llama-2-70b-chat-hf",
    quantization_config=bnb_config,
    device_map="auto"
)
# 70B model now fits in ~40GB instead of 140GB
```

## Building a Private AI Pipeline

For processing restricted data where nothing leaves Sagehen:

```python
class LocalAIPipeline:
    def __init__(self, model_name="meta-llama/Llama-2-7b-chat-hf"):
        self.tokenizer = AutoTokenizer.from_pretrained(model_name)
        self.model = AutoModelForCausalLM.from_pretrained(
            model_name, torch_dtype=torch.float16, device_map="auto"
        )

    def analyze(self, text, task="summarize"):
        prompts = {
            "summarize": f"Summarize in 2 sentences:\n{text}",
            "classify": f"Classify sentiment (positive/negative/neutral):\n{text}",
            "extract": f"Extract key points:\n{text}"
        }
        prompt = prompts.get(task, text)
        inputs = self.tokenizer(prompt, return_tensors="pt").to(self.model.device)
        with torch.no_grad():
            outputs = self.model.generate(**inputs, max_new_tokens=200,
                                         do_sample=False)
        return self.tokenizer.decode(outputs[0], skip_special_tokens=True)
```

## Auditing AI Usage

Keep records for Pomona compliance:

```python
import json
from datetime import datetime

usage_log = {
    "timestamp": datetime.now().isoformat(),
    "model": "Llama-2-7b-chat",
    "data_type": "Restricted (FERPA)",
    "task": "Sentiment classification",
    "input_tokens": 500,
    "output_tokens": 200
}

with open("ai_usage_log.json", "a") as f:
    json.dump(usage_log, f)
    f.write("\n")
```

::::::::::::::::::::::::::::::::::::: callout

## Model Quality vs. Size Tradeoff

Smaller quantized models run faster and use less memory with slightly lower
quality. For many research tasks (summarization, classification), the
difference is negligible. Always test on your actual data to verify the
quality-efficiency tradeoff is acceptable.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Run a Local LLM Job

Write a Slurm batch script that:

1. Loads modules and activates a conda environment
2. Loads Llama 2 7B with half precision
3. Asks it a question about machine learning
4. Times the inference and prints the result

::::::::::::::::::::::::::::::::::::: solution

## Solution

```bash
#!/bin/bash
#SBATCH --partition=gpu
#SBATCH --gres=gpu:1
#SBATCH --mem=32GB
#SBATCH --time=00:10:00

module purge && module load anaconda3 cuda/12.1 cudnn/8.9.2
conda activate llama_env

python3 << 'EOF'
import time, torch
from transformers import AutoTokenizer, AutoModelForCausalLM

model_name = "meta-llama/Llama-2-7b-chat-hf"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(
    model_name, torch_dtype=torch.float16, device_map="auto")

question = "What is machine learning?"
inputs = tokenizer(question, return_tensors="pt").to(model.device)
start = time.time()
with torch.no_grad():
    outputs = model.generate(**inputs, max_new_tokens=150)
print(f"Time: {time.time() - start:.1f}s")
print(tokenizer.decode(outputs[0], skip_special_tokens=True))
EOF
```

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Open-source LLMs (Llama, Mistral, Phi) run locally on Sagehen GPUs
- 4-bit quantization reduces memory by 75% with minimal quality loss
- Build private pipelines for restricted data that never leaves Sagehen
- Log AI usage for Pomona compliance with timestamps, model, and data type

::::::::::::::::::::::::::::::::::::::::::::::
