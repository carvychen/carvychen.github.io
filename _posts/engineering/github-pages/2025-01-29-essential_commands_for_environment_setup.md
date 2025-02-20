---
title: Essential Commands for Environment Setup
description: A quick reference for essential commands to check system environment information and install environments efficiently.
author: Jameel
date: 2025-02-18 14:50:11 +0800
categories:
  - Engineering
  - Commands-Quick-Guide
tags:
  - commands-quick-guide
pin: true
math: true
mermaid: true
comments: false
---


## Conda

> For detailed instructions on setting up and managing Conda environments, refer to the [official Conda documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html).
{: .prompt-tip}

### Initialize for Shell

```bash
source /opt/conda/etc/profile.d/conda.sh
```

### Activate

```bash
conda activate env_name
```

### Deactivate

```bash
conda deactivate
```

### List all Environments

```bash
conda info --envs
```

### List Installed Packages

```bash
conda list
```

If you want to check for specific packages, such as CUDA, and see which version is installed, use:

```bash
conda list cuda
```

### Create a New Environment

```bash
conda create --name env_name python=3.10.16
```

### Clone the Base Environment

```bash
conda create -n env_name --clone base
```

### Export to YAML File

```bash
conda env export > environment.yml
```

### Create from YAML File

```bash
conda env create -f environment.yml
```

### Remove a Conda Environment

```bash
conda remove -n env_name --all
```

## Pip

> For detailed instructions on pip commands, refer to the [official documentation](https://pip.pypa.io/en/stable/cli/)
{: .prompt-tip}

### List Installed Python Packages

```bash
# list all the installed Python packages and their detailed information (including version, location)
pip list --verbose
```

### Check Packages Information

```bash
# display detailed information about the specified package (e.g., torch) installed in your environment
pip show torch
```

### Install from Custom Index

```bash
# install the specified package from a custom package index, for example:
pip install opencv-python-headless -i https://pypi.org/simple
```

### Domestic Mirrors

| Mirror Source | URL                                      |
| :------------ | :--------------------------------------- |
| Aliyun        | http://mirrors.aliyun.com/pypi/simple    |
| Tsinghua      | https://pypi.tuna.tsinghua.edu.cn/simple |

If you want to set a default mirror globally, you can configure it in pip by running:

```bash
pip config set global.index-url http://mirrors.aliyun.com/pypi/simple
```

## Git

### Git LFS

```bash
git init  # initialize a git repo
git lfs install  # install git LFS
git lfs track *  # track large files
git add .gitattributes
git commit -m "first commit"  # add and commit the .gitattributes file
git remote add origin https://github.com/users/test.git  # link local repo to GitHub
git push origin master  # push initial LFS configuration
git add *
git commit -m "Git LFS commit"  # add and commit large files
git push origin master  # push large files to GitHub
```

### Stash and Pull

```bash
# temporarily save local changes, pull the latest updates from the remote repository, and then reapply your saved changes
git stash           # Save local changes to a stash
git pull            # Pull the latest changes from the remote repository
git stash pop       # Reapply the saved changes from the stash
```

### Reset and Force Push

| `git reset --hard HEAD~3`	 | Removes the last 3 commits and resets to an earlier commit. |
| `git push origin main --force` | Forces the remote repository to match the local history after a reset. |
| `git push origin main --force-with-lease`	| A safer alternative that prevents overwriting remote changes made by others. |

> **Example: git reset**
>
> If the current commit history looks like this: `A - B - C - D - E - F - G (HEAD)`
>
> Running git reset --hard HEAD~3 will result in: `D (HEAD)`, Commits `E` to `G` are completely removed from history.

> **When to Use These Commands?**
>
> Undo recent commits permanently (e.g., bad commits, mistakenly pushed files).
>
> Rewrite commit history to clean up or rebase changes.
>
> Force push after resetting commits, ensuring remote reflects local history.

> This action is irreversible! It replaces the remote history with your local history.
>
> Other collaborators working on the branch may lose their work if they have commits beyond the new history.
>
> If multiple people are working on the repository, use git push --force-with-lease instead, which only forces the push if no new commits were added remotely:
{: .prompt-danger}

## CUDA

### Check Version

```bash
nvcc --version
```

This command will display the version of the CUDA compiler (nvcc) installed on your system. For a more comprehensive version check, you can also refer to the version.txt file in the CUDA installation directory:

```bash
cat /usr/local/cuda/version.txt
```

### Check Installation Location

```bash
ls -l /usr/local | grep cuda
```

### Install CUDA in Conda

```bash
conda install cuda -c nvidia/label/cuda-12.1.0
```

> Check available versions in the [NVIDIA Conda channel](https://anaconda.org/nvidia/cuda).
{: .prompt-tip}

### Switch Versions

> If you’d prefer to use the system’s CUDA version (e.g., CUDA 11.8) instead of the one installed in your Conda environment, adjust the PATH and LD_LIBRARY_PATH environment variables:

```bash
export PATH=/usr/local/cuda-11.8/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-11.8/lib64:$LD_LIBRARY_PATH
```

## PyTorch

### Install with Conda

> For detailed instructions on installing PyTorch, refer to the [official documentation](https://pytorch.org/get-started/locally/).
{: .prompt-tip}

```bash
# install PyTorch 2.4.0 with CUDA 12.1 support in your Conda environment
conda install pytorch==2.4.0 torchvision==0.19.0 torchaudio==2.4.0 pytorch-cuda=12.1 -c pytorch -c nvidia
```

### Install with Pip

```bash
# install PyTorch 1.13.1 along with TorchVision and Torchaudio, and ensure compatibility with CUDA 11.7
pip install torch==1.13.1 torchvision torchaudio --index-url https://download.pytorch.org/whl/cu117
```

## System

### Search

```bash
find . -name "*" | xargs grep "xxxxx"
```

### Nohup

> nohup (no hang up) allows processes to run in the background even after the terminal session is closed.

```bash
nohup python script.py &
```

> Redirect Output to a Log File

```bash
nohup python script.py > script.log 2>&1 &

# `> script.log`: Redirects standard output to script.log.
# `2>&1`: Redirects standard error to the same log file.
```

> Kill a nohup Process

```bash
ps -ef | grep script.py
kill -9 12345  # Replace 12345 with the actual PID
```

> Check Running nohup Processes

```bash
ps aux | grep nohup
```

### Kill Process

```bash
# terminate all running specified processes, e.g., **Python**
ps -ef | grep python | grep -v grep | awk '{print $2}' | xargs kill -9
```

> **Explanation**
> 
> `ps -ef`: Lists all running processes on the system.
>
> `grep python`: Filters the processes to show only those related to Python.
>
> `grep -v grep`: Excludes the grep command itself from the results (since it contains the string “python”).
>
> `awk '{print $2}'`: Extracts the process ID (PID) of each Python process.
>
> `xargs kill -9`: Passes the PIDs to the kill command with signal -9 (force kill).

### Wget

```bash
wget --content-disposition "https://example.com/download/file"
```
> **Why Use --content-disposition?**
>
> Server-Suggested Filenames: Some websites provide a Content-Disposition header that suggests a proper filename. This is helpful when the URL doesn’t provide an obvious file name or is dynamically generated.
>
> Avoid Manual Renaming: It prevents the need to manually rename files after downloading if the server has already suggested a meaningful name.


```bash
# Download Files from Google Drive
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=FILEID' -O FILENAME

# Usage Example: Replace FILEID with the actual Google Drive file ID and FILENAME with the desired output filename.
# Limitation: Won’t work for large files.
```

```bash
# Download Large Files from Google Drive
wget --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate 'https://docs.google.com/uc?export=download&id=FILEID' -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')&id=FILEID" -O FILENAME && rm -rf /tmp/cookies.txt
```

### Gdown

> Download Files from Google Drive Using gdown. gdown is a command-line tool that simplifies downloading files from Google Drive using their file ID.

``` bash
pip install gdown	 # Install gdown
gdown file_id	 # Download a file by ID
gdown -O filename file_id	 # Download and rename file
gdown --fuzzy "URL"  # Download using a full Google Drive link
gdown --folder folder_id  # Download an entire folder
gdown --continue file_id  # Resume an interrupted download
gdown --id file_id --cookies cookies.txt	# Download private files with authentication
```

## Quick Install

### [LLaMA-Factory](https://github.com/hiyouga/LLaMA-Factory.git)

```bash
conda create --name llama-factory python=3.10.16
conda activate llama-factory
conda install cuda -c nvidia/label/cuda-12.1.0
conda install pytorch==2.4.0 torchvision==0.19.0 torchaudio==2.4.0 pytorch-cuda=12.1 -c pytorch -c nvidia
pip install deepspeed
pip install flash-attn --no-build-isolation
pip install git+https://github.com/huggingface/transformers.git
# It's highly recommanded to use `[decord]` feature for faster video loading.
pip install qwen-vl-utils[decord]==0.0.8

git clone --depth 1 https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
pip install -e ".[torch,metrics]"
llamafactory-cli version
```