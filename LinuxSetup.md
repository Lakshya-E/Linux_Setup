# VM Setup Instructions

This document provides a comprehensive list of Linux commands necessary to include required packages after setting up a new virtual machine (VM).

## Table of Contents

1. [Update System](#update-system)
2. [Install Python3](#install-python3)
3. [Install Nvidia-Drivers](#install-nvidia-drivers)
4. [Install Docker](#install-docker)
5. [Relocating Docker /Root Directory](#relocating-docker-root-directory)
6. [Mount Disk to File-System](#mount-disk-to-file-system)
7. [Hostname Modification](#hostname-modification)
8. [Establishing Self-Signed Certificates](#establishing-self-signed-certificates)
9. [Install Nvidia-Docker](#install-nvidia-docker)
10. [Installing Git-LFS](#installing-git-lfs)

## Update System

```sh
# Update the package list
sudo apt-get update

# Install required packages
sudo apt-get install -y build-essential
sudo apt-get install -y software-properties-common
```


## Install Python3

```sh
# Update the package list
sudo apt-get update

# Install all python packages
sudo apt-get install python3
sudo apt-get install -y python3-pip python3-dev

# Check Python with
python3 --version

```


## Install Nvidia-Drivers

```sh
# Setup distribution for drivers
sudo apt-get install linux-headers-$(uname -r)
distribution=$(. /etc/os-release;echo $ID$VERSION_ID | sed -e 's/\.//g')

wget https://developer.download.nvidia.com/compute/cuda/repos/$distribution/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb

# Update the package list
sudo apt-get update

# Install cuda-drivers
sudo apt-get -y install cuda-drivers

# Set path for your drivers
export PATH=/usr/local/cuda-12.2/bin${PATH:+:${PATH}}

# Check nvidia-drivers with
nvidia-smi

```


## Install Docker

```sh
# Update the package list
sudo apt-get update

# Setup required certificated and keys
sudo apt-get install ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# Install Docker
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Check docker with
sudo docker --version

```


## Relocating Docker /Root Directory

- Refer to this site - [Click Here](https://www.ibm.com/docs/en/z-logdata-analytics/5.1.0?topic=software-relocating-docker-root-directory)


## Mount Disk to File-System

1. Follow steps as instructed - [Click Here](https://linuxconfig.org/how-to-add-new-disk-to-existing-linux-system)

2. Persist mounted disk even after Reboot
```sh
# Open fstab file
nano /etc/fstab

# Add your mounted disk and directory in this file
/dev/sdX /mounted_directory ext4 defaults 0 0

# Save file then,
sudo reboot

```


## Hostname Modification

```sh
sudo hostnamectl set-hostname <newhostname>

```


## Establishing Self-Signed Certificates

```sh
# You'll get two files - cert.pem and cert.key as a result
# These files will be valid for 365 days
openssl req -x509 -newkey rsa:4096 -nodes -out cert.pem -keyout key.pem -days 365

```


## Install Nvidia-Docker
- In case of a dockerized app that requires GPU, follow these steps to install Nvidia-Docker

##### Note: 
 - First make sure you installed Docker and Nvidia-Drivers.
 - If not, follow guides mentiioned: [Install Docker](#install-docker), [Install Nvidia-Drivers](#install-nvidia-drivers)

```sh
# Remove any previously installed nvidia-docker packeges that might hinder the process
sudo docker volume ls -q -f driver=nvidia-docker | xargs -r -I{} -n1 docker ps -q -a -f volume={} | xargs -r docker rm -f
sudo apt-get purge nvidia-docker

# Setup required keys and distribution
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -

distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update

# If `sudo apt-get update` fails in the step above, perform the action mentioned below and again update
# Skip these two steps if `sudo apt-get update` was a success
grep -l "nvidia.github.io" /etc/apt/sources.list.d/* | grep -vE "/nvidia-container-toolkit.list\$"
sudo apt-get update

# Install Nvidia-Docker2
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd
sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker

# To check your docker container connectivity with GPU
sudo docker run --runtime=nvidia --gpus all image_name nvidia-smi
# nvidia-smi at end tests if docker container is linking with nvidia-drivers
# In case of a failure, you'll not see a nvidia-smi pop-up window

# To run docker container with GPU support 
sudo docker run --runtime=nvidia --gpus all image_name

```

## Installing Git-LFS

```sh
# Install required packages
sudo apt-get -y install software-properties-common
sudo curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash*

# Install Git-LFS
sudo apt-get install git-lfs

# Enable Git-LFS
git lfs install

```

