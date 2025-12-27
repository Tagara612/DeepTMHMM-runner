### DeepTMHMM-runner (100% success)
Guide for pulling the DeepTMHMM Docker image and running batch predictions locally.

## Ways to use DeepTMHMM
There are two common ways to use DeepTMHMM:

### 1) Local installation (Academic Version: DeepTMHMM 1.0)
You can install DeepTMHMM locally using **DeepTMHMM 1.0 – Academic Version** from BioLib.  
To request an academic license, email: **licensing@biolib.com**.  

A helpful reference for the local installation workflow is available here:  
https://www.polarmicrobes.org/local-installation-of-deeptmhmm/

### 2) Docker image (recommended in this project)
You can also run DeepTMHMM via Docker by pulling images from Docker Hub:  
https://hub.docker.com/u/hannharris  

There are multiple image options available there, depending on your needs.  
In this project, we focus on the second approach and provide a practical, step-by-step guide for running DeepTMHMM using Docker.

---

## Prerequisites (Docker)
First, make sure Docker is installed on your server.

---

## Step 1: Pull the latest image
Pull the DeepTMHMM image from the official Docker Hub page:

```bash
docker pull hannharris/deeptmhmm:latest
docker image ls -a
```

<img width="1676" height="1308" alt="image" src="https://github.com/user-attachments/assets/7369ccf7-0790-44d5-a6c1-ba4b25d890ce" />

---

## Step 2: Enable GPU support (if your host has NVIDIA GPUs)
If your host machine has an NVIDIA GPU and you have installed the NVIDIA driver and **NVIDIA Container Toolkit** (so `--gpus all` works), you can verify GPU visibility:

```bash
docker run --rm --gpus all nvidia/cuda:12.4.1-base-ubuntu22.04 nvidia-smi
```

(Optional) Verify CUDA availability inside the DeepTMHMM image:

```bash
docker run --rm --gpus all hannharris/deeptmhmm:latest \
  python3 -c "import torch; print('cuda available:', torch.cuda.is_available()); print('device count:', torch.cuda.device_count())"
```

---

## Step 3: Run prediction and verify success
Mount your host directory into the container and run prediction on a FASTA file:

```bash
docker run --rm -it --gpus all \
  -v "$HOME":/work \
  hannharris/deeptmhmm:latest \
  bash -lc "python3 -u predict.py --fasta /work/test.fasta && cp -f predicted_topologies.3line TMRs.gff3 /work/"
```

**Path mapping**
- Input: host `~/test.fasta` → container `/work/test.fasta`
- Output copied back to host: `~/predicted_topologies.3line` and `~/TMRs.gff3`

<img width="938" height="166" alt="run-example" src="https://github.com/user-attachments/assets/23c5dd72-428b-4410-8821-b5df845bbe77" />

### Success criteria
A run is considered **successful only if both output files are generated**:
- `predicted_topologies.3line`
- `TMRs.gff3`

Check on the host:

```bash
ls -lh ~/predicted_topologies.3line ~/TMRs.gff3
```

---

## Optional: Run locally on the host (often faster)
Of course, you can also copy the application files from the Docker image to your host machine, set up a local environment, and run DeepTMHMM directly on the host. This is often **much faster** than running everything inside Docker (especially for repeated/batch runs).
