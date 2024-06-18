# VM Setup Instructions

This document provides a comprehensive list of Linux commands necessary to include required packages after setting up a new virtual machine (VM).

## Table of Contents

1. [System Update](#system-update)
2. [Install Python3](#install-python3)
3. [Install Nvidia-Drivers](#install-nvidia-drivers)
4. [SSH Configuration](#ssh-configuration)
5. [Software Installation](#software-installation)
6. [System Configuration](#system-configuration)
7. [Service Management](#service-management)
8. [Network Configuration](#network-configuration)
9. [Storage Management](#storage-management)
10. [Security Settings](#security-settings)
11. [Backup and Restore](#backup-and-restore)

## System Update

```sh
# Update the package list
<command>

# Upgrade all installed packages
<command>
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