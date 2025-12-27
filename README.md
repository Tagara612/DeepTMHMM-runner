### DeepTMHMM-runner (100% sucess)
Guide for pulling the DeepTMHMM Docker image and running batch predictions locally.

 Ways to use DeepTMHMM

There are two common ways to use DeepTMHMM:

## 1) Local installation (Academic Version: DeepTMHMM 1.0)
  You can install DeepTMHMM locally using DeepTMHMM 1.0 – Academic Version from BioLib.
  To request an academic license, email: licensing@biolib.com.
  A helpful reference for the local installation workflow is available here:  
  https://www.polarmicrobes.org/local-installation-of-deeptmhmm/

## 2) Docker image (recommended in this project)
  You can also run DeepTMHMM via Docker by pulling images from the following Docker Hub page:https://hub.docker.com/u/hannharris.
  There are multiple image options available there, depending on your needs.
  In this project, we focus on the second approach and provide a practical, step-by-step guide for running DeepTMHMM using Docker (including batch FASTA processing and interpreting the output).

   # Prerequisites (Docker)
   First, make sure Docker is installed on your server. 

   Step 1: Pull the latest image
   
   Then pull the DeepTMHMM image from the official Docker Hub page:
   - Example:
      docker pull hannharris/deeptmhmm:latest
     <img width="1676" height="1308" alt="image" src="https://github.com/user-attachments/assets/7369ccf7-0790-44d5-a6c1-ba4b25d890ce" />

   Step 2: Enable GPU support (if your host has NVIDIA GPUs)
   If your host machine has an NVIDIA GPU and you have installed the NVIDIA driver and NVIDIA Container Toolkit (so --gpus all works).

   Step 3 (Optional): Validate the run (success criteria)
   After pulling DeepTMHMM, you should verify that the prediction finished successfully.
   Mount your working directory into the container and run prediction on a FASTA file:
   
  docker run --rm -it --gpus all \
    -v "$HOME":/work \
    hannharris/deeptmhmm:latest \
    bash -lc "python3 -u predict.py --fasta /work/test.fasta && cp -f predicted_topologies.3line TMRs.gff3 /work/"
    
    Input: host ~/test.fasta → container /work/test.fasta
    <img width="938" height="166" alt="截屏2025-12-27 18 30 42" src="https://github.com/user-attachments/assets/23c5dd72-428b-4410-8821-b5df845bbe77" />
      
   A run is considered **successful only if both output files are generated**:
    - `predicted_topologies.3line`
    - `TMRs.gff3`

   ###Of course, you can also copy the application files from the Docker image to your host machine, set up a local environment, and run DeepTMHMM directly on the host. This is often **much faster** than running everything inside Docker (especially for repeated/batch runs).

   


    

  

  
