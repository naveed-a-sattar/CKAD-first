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