# Installation-for-NVIDIA-driver-CUDA-toolkit-and-CuDNN-package---Ubuntu-22.04

#!/bin/bash

### steps ####
# verify the system has a cuda-capable gpu
# download and install the nvidia cuda toolkit and cudnn
# setup environmental variables
# verify the installation
###

### to verify your gpu is cuda enable check
lspci | grep -i nvidia

### If you have previous installation remove it first. 
sudo apt-get purge nvidia*
sudo apt remove nvidia-*
sudo rm /etc/apt/sources.list.d/cuda*
sudo apt-get autoremove && sudo apt-get autoclean
sudo rm -rf /usr/local/cuda*

# system update
sudo apt-get update
sudo apt-get upgrade

# install other import packages
sudo apt-get install g++ freeglut3-dev build-essential libx11-dev libxmu-dev libxi-dev libglu1-mesa libglu1-mesa-dev

# first get the PPA repository driver
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update

# install nvidia driver with dependencies
sudo apt install libnvidia-common-525
sudo apt install libnvidia-gl-525
sudo apt install nvidia-driver-525
sudo apt install nvidia-dkms-525

wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin
sudo mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/3bf863cc.pub
sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/ /"
sudo apt-get update

# installing CUDA-11.3
sudo apt install cuda-11-3 

CUDNN_TAR_FILE='cudnn-linux-x86_64-8.7.0.84_cuda11-archive'
tar -xf ${CUDNN_TAR_FILE}.tar.xz

# copy the following files into the cuda toolkit directory.
sudo cp -P ${CUDNN_TAR_FILE}/include/cudnn.h /usr/local/cuda-11.3/include
sudo cp -P ${CUDNN_TAR_FILE}/lib/libcudnn* /usr/local/cuda-11.3/lib64/
sudo chmod a+r /usr/local/cuda-11.3/lib64/libcudnn*

# Finally, to verify the installation, reboot and check
nvidia-smi
nvcc -V
