# Kubernetes Cluster on Raspberry Pi 3 B+

This guide explains how to set up a Kubernetes cluster using K3s on a Raspberry Pi 3 B+.

## Why K3s?
- Lightweight (Less than 512MB RAM required)
- Perfect for IoT & Edge computing
- Easy to install and manage
- Official CNCF certified Kubernetes distribution
- Created by Rancher Labs (now part of SUSE)

## Prerequisites

- Raspberry Pi 3 B+ with Raspberry Pi OS (64-bit recommended)
- At least 2GB SD card
- Static IP address configured
- SSH enabled

## Installation Guide

### 1. Prepare the Raspberry Pi

Update the system and install dependencies:

```bash
Update the system
sudo apt update && sudo apt upgrade -y
Install dependencies
sudo apt install -y curl openssh-server
```

### 2. Disable Swap

Kubernetes requires swap to be disabled:

```bash
sudo dphys-swapfile swapoff
sudo dphys-swapfile uninstall
sudo systemctl disable dphys-swapfile
```


### 3. Configure cgroup

Edit `/boot/cmdline.txt`:

```bash
sudo sed -i '$ s/$/ cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1/' /boot/cmdline.txt
```

### 4. Install K3s
```bash
curl -sfL https://get.k3s.io | sh -
```

Wait for a minute, then check the status

```bash
sudo systemctl status k3s
```
Get the node status
```bash
sudo kubectl get nodes
```

### 5. Get the Cluster Token

Save this token for adding more nodes later:

```bash
sudo cat /var/lib/rancher/k3s/server/node-token
```

### 6. Install kubectl

Configure kubectl for your user:

```bash
mkdir -p ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $USER:$USER ~/.kube/config
```

## Verification

Verify your installation:

Check running pods

```bash
kubectl get pods --all-namespaces
```

Check cluster info

```bash
kubectl cluster-info
```

## Optional Tools
### 1. Helm - Package Manager

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

Reboot the Raspberry Pi

```bash
sudo reboot
```
