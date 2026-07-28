---
title: "Running Models on Sagehen"
teaching: 15
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I set up AI tools on Sagehen HPC?
- What modules and environments are needed?
- How do I access GPUs and submit AI jobs?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Load required modules for AI work (CUDA, cuDNN, anaconda)
- Create conda environments for PyTorch and TensorFlow
- Access GPU resources via Slurm for interactive and batch jobs
- Configure model caching to avoid quota issues

::::::::::::::::::::::::::::::::::::::::::::::

## Sagehen Storage for AI Work

| Location | Quota | Use For |
|----------|-------|---------|
| `/rhome/username` | 100GB | Code, conda environments, notebooks |
| `/bigdata/lab_name` | 1TB shared (BeeGFS) | Large datasets, model checkpoints |
| `/scratch` | Temporary (30-day auto-delete) | Job I/O, intermediate results |

Download large models to `/bigdata` or `/scratch` -- not your home directory.

## Loading Modules

```bash
module purge
module load anaconda3          # Python and package management
module load cuda/12.1          # NVIDIA CUDA toolkit
module load cudnn/8.9.2        # cuDNN for deep learning
module list                    # Verify loaded modules
```

## Creating a Conda Environment

```bash
module load anaconda3
conda create -n ai-pytorch python=3.11
conda activate ai-pytorch

# Install PyTorch with GPU support
conda install pytorch::pytorch torchvision torchaudio pytorch-cuda=12.1 \
    -c pytorch -c nvidia

# Install common packages
conda install numpy pandas scikit-learn matplotlib jupyter -c conda-forge
```

For TensorFlow, create a separate environment:

```bash
conda create -n ai-tensorflow python=3.11
conda activate ai-tensorflow
pip install tensorflow[and-cuda]
```

## Configure Model Caching

By default, Hugging Face downloads to `~/.cache/huggingface`, which fills your
home quota quickly. Redirect it:

```bash
mkdir -p /bigdata/your_lab/huggingface_cache
export HF_HOME=/bigdata/your_lab/huggingface_cache
# Add to your .bashrc or Slurm scripts to make permanent
```

## Interactive GPU Sessions

```bash
srun --partition=gpu --gres=gpu:1 --mem=64G \
     --cpus-per-task=8 --time=04:00:00 --pty bash

module purge && module load anaconda3 cuda/12.1 cudnn/8.9.2
conda activate ai-pytorch
python -c "import torch; print(torch.cuda.is_available())"
```

## Batch Jobs for Model Training

```bash
#!/bin/bash
#SBATCH --job-name=llama-inference
#SBATCH --partition=gpu
#SBATCH --gres=gpu:1
#SBATCH --mem=80G
#SBATCH --cpus-per-task=16
#SBATCH --time=08:00:00
#SBATCH --output=logs/%j.log

module purge && module load anaconda3 cuda/12.1 cudnn/8.9.2
source activate ai-pytorch
export HF_HOME=/bigdata/your_lab/huggingface_cache

python your_script.py
```

Submit with `sbatch your_script.sh`.

::::::::::::::::::::::::::::::::::::: callout

## Start Small and Scale Up

When first testing an AI workflow, request a smaller GPU (V100) with less time.
Once your script works reliably, scale to larger GPUs or longer time limits.
This saves compute hours and reduces queue wait time.

::::::::::::::::::::::::::::::::::::::::::::::

## Common Troubleshooting

- **"No module named torch"**: Load anaconda3 and activate your conda environment
- **"CUDA out of memory"**: Use 4-bit quantization, reduce batch size, or
  request a larger GPU
- **Home quota full from models**: Set `HF_HOME` to `/bigdata`
- **GPU not detected**: Verify CUDA/cuDNN modules are loaded

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Write a GPU Setup Script

Write a Slurm script that loads modules, activates a conda environment, sets
HF_HOME, and verifies GPU access by printing `torch.cuda.is_available()` and
the GPU name.

::::::::::::::::::::::::::::::::::::: solution

## Solution

```bash
#!/bin/bash
#SBATCH --job-name=gpu-test
#SBATCH --partition=gpu
#SBATCH --gres=gpu:1
#SBATCH --mem=32G
#SBATCH --cpus-per-task=4
#SBATCH --time=00:10:00

module purge && module load anaconda3 cuda/12.1 cudnn/8.9.2
conda activate ai-pytorch
export HF_HOME=/bigdata/your_lab/huggingface_cache

python3 -c "
import torch
print(f'GPU available: {torch.cuda.is_available()}')
if torch.cuda.is_available():
    print(f'GPU: {torch.cuda.get_device_name(0)}')
    print(f'Memory: {torch.cuda.get_device_properties(0).total_mem / 1e9:.0f} GB')
"
```

Submit with `sbatch gpu_test.sh` and check output in `slurm-*.out`.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Load anaconda3, cuda, and cudnn modules before GPU work
- Use conda environments for reproducible, isolated Python setups
- Configure HF_HOME to /bigdata to avoid filling home quota with models
- Use `srun` for interactive GPU testing and `sbatch` for batch jobs
- Start with smaller GPUs and scale up once your workflow is validated

::::::::::::::::::::::::::::::::::::::::::::::
