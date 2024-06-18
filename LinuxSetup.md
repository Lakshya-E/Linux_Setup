# VM Setup Instructions

This document provides a comprehensive list of Linux commands necessary to include required packages after setting up a new virtual machine (VM).

## Table of Contents

1. [System Update](#system-update)
2. [Install Python3](#install-python3)
3. [Install Nvidia-Drivers](#install-nvidia-drivers)
4. [Install Docker](#install-docker)
5. [Relocating Docker /Root Directory](#relocating-docker-root-directory)
6. [Mount Disk to File-System](#mount-disk-to-file-system)
7. [Service Management](#service-management)
8. [Network Configuration](#network-configuration)
9. [Storage Management](#storage-management)
10. [Security Settings](#security-settings)
11. [Backup and Restore](#backup-and-restore)

## Update System

```sh
# Update the package list
sudo apt-get update

# Install required packages
sudo apt-get install -y build-essential
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

- Refer to this site - [Click Here](https://linuxconfig.org/how-to-add-new-disk-to-existing-linux-system)