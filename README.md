# Kubernetes Node Setup Instructions

Follow these steps to configure your nodes after bringing up Vagrant.

---

## Step 1: Enable `br_netfilter` Module

Run the following commands **on each node**:

```bash
sudo modprobe br_netfilter
echo "br_netfilter" | sudo tee /etc/modules-load.d/br_netfilter.conf
```

Verify the module is loaded:

```bash
lsmod | grep br_netfilter
```

### Expected Output:
The output should show `br_netfilter`. If there is no output, the module is not loaded correctly.

---

## Step 2: Configure Sysctl for Kubernetes Networking

Run the following commands **on each node**:

```bash
cat << EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF
```

---

## Step 3: Apply Sysctl Settings

Run the following command to apply the changes:

```bash
sudo sysctl --system
```

---

## Step 4: Verify Configuration

### Check the following parameters:
1. **Bridge NF Call for IPTables**:
    ```bash
    sysctl net.bridge.bridge-nf-call-iptables
    ```
    **Expected Output**:
    ```
    net.bridge.bridge-nf-call-iptables = 1
    ```

2. **Bridge NF Call for IP6Tables**:
    ```bash
    sysctl net.bridge.bridge-nf-call-ip6tables
    ```
    **Expected Output**:
    ```
    net.bridge.bridge-nf-call-ip6tables = 1
    ```

3. **IPv4 Forwarding**:
    ```bash
    sysctl net.ipv4.ip_forward
    ```
    **Expected Output**:
    ```
    net.ipv4.ip_forward = 1
    ```

---

These steps ensure that the nodes are properly configured for Kubernetes networking.

---
---

## CRI as Docker Installation Instructions

Follow these steps to install Docker on Ubuntu.

---

### Step 1: Install Required Packages

Run the following command to install required certificates and tools:

```bash
sudo apt-get install ca-certificates curl -y
```

---

### Step 2: Set Up Keyrings for Docker Repository

Create the necessary directory for storing keyrings:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

Download and save Docker's GPG key:

```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

Set appropriate permissions for the key:

```bash
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

---

### Step 3: Add Docker's Official APT Repository

Add Docker's APT repository to your sources list:

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

---

### Step 4: Update Package List

Update the APT package list to include Docker's repository:

```bash
sudo apt update
```

Then run the following to ensure all package lists are updated:

```bash
sudo apt-get update
```

---

### Step 5: Install Docker

Install Docker and required components:

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

---

### Step 6: Verify Docker Installation

Verify that Docker is installed and running by listing the running Docker containers:

```bash
sudo docker ps
```

---

Follow these steps to install Docker successfully and verify that it is working on your Ubuntu system.

---
---
## Install kubeadm kubelet and kubectl on each node

Follow the instructions from:
https://v1-31.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl


