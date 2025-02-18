---
title: Essential Commands for Environment Setup
description: A quick reference for essential commands to view system environment information and install environments efficiently.
author: Jameel
date: 2025-02-18 14:50:11 +0800
categories:
  - Engineering
  - Commands-Quick-Guide
tags:
  - commands-quick-guide
pin: false
math: true
mermaid: true
comments: false
---

> This guide provides key commands for managing system configurations and setting up environments, designed for quick and easy reference.
{: .prompt-tip}

## System Configuration

### Check CUDA Version

To check which version of CUDA is installed on your system, run the following command:

```bash
nvcc --version
```

This command will display the version of the CUDA compiler (nvcc) installed on your system. For a more comprehensive version check, you can also refer to the version.txt file in the CUDA installation directory:

```bash
cat /usr/local/cuda/version.txt
```

### Check CUDA Installation Location

To view the location of CUDA installation, use:

```bash
ls -l /usr/local | grep cuda
```

### View Conda Environments

To list all Conda environments and their respective locations, use:

```bash
conda info --envs
```

### View Installed Packages in Conda Environment

To list all packages installed in the currently active Conda environment, run:

```bash
conda list
```

If you want to check for specific packages, such as CUDA, and see which version is installed, use:

```bash
conda list cuda
```

## Environment Setup

> For detailed instructions on setting up and managing Conda environments, refer to the [official Conda documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html).
{: .prompt-tip}

### Initialize Conda for Shell

To initialize Conda for use in your shell, run:

```bash
source /opt/conda/etc/profile.d/conda.sh
```

### Activate the Conda Environment

To activate a specific Conda environment, run:

```bash
conda activate env_name
```

### Deactivate the Conda Environment

To deactivate the current Conda environment, use:

```bash
conda deactivate
```

### Create a New Conda Environment

To create a new Conda environment with Python version **3.10.16**, run:

```bash
conda create --name env_name python=3.10.16
```

### Clone the Base Environment

To clone the base Conda environment into a new environment, use:

```bash
conda create -n env_name --clone base
```

### Export Environment to YAML File

To export the current Conda environment’s configuration to a YAML file (e.g., `environment.yml`), use:

```bash
conda env export > environment.yml
```

### Create Environment from YAML File

To create a new Conda environment from an exported YAML file, run:

```bash
conda env create -f environment.yml
```

### Remove a Conda Environment

To completely remove a Conda environment and all its packages, use:

```bash
conda remove -n env_name --all
```

### Install CUDA in Conda Environment

To install **CUDA 12.1** in a Conda environment, run:

```bash
conda install cuda -c nvidia/label/cuda-12.1.0
```

> Check available versions in the [NVIDIA Conda channel](https://anaconda.org/nvidia/cuda).
{: .prompt-tip}

### Switch Between Conda and System CUDA Versions

If you’d prefer to use the system’s CUDA version (e.g., CUDA 11.8) instead of the one installed in your Conda environment, adjust the PATH and LD_LIBRARY_PATH environment variables:

```bash
export PATH=/usr/local/cuda-11.8/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-11.8/lib64:$LD_LIBRARY_PATH
```

### Install PyTorch with CUDA Support

To install PyTorch 2.4.0 with CUDA 12.1 support in your Conda environment, run:

```bash
conda install pytorch==2.4.0 torchvision==0.19.0 torchaudio==2.4.0 pytorch-cuda=12.1 -c pytorch -c nvidia
```
> For detailed instructions on installing PyTorch, refer to the [official documentation](https://pytorch.org/get-started/locally/).
{: .prompt-tip}

### Archive

**[LLaMA-Factory](https://github.com/hiyouga/LLaMA-Factory.git)**

```bash
conda install cuda -c nvidia/label/cuda-12.1.0
conda install pytorch==2.4.0 torchvision==0.19.0 torchaudio==2.4.0 pytorch-cuda=12.1 -c pytorch -c nvidia
pip install deepspeed
pip install flash-attn --no-build-isolation
pip install git+https://github.com/huggingface/transformers.git

git clone --depth 1 https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
pip install -e ".[torch,metrics]"
llamafactory-cli version
```