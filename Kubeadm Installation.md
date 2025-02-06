# Kubeadm Installation Guide

This guide outlines the steps needed to set up a Kubernetes cluster using kubeadm.

## Prerequisites
- Ubuntu OS (Xenial or later)
- `sudo` privileges
- Internet access
- t2.medium instance type or higher

## AWS Setup
Ensure that all instances are in the same Security Group:
- Expose port **6443** in the Security Group to allow worker nodes to join the cluster.
- Expose port **22** in the Security Group to allow SSH access to manage the instance.

To do the above setup, follow the steps below:

### Step 1: Identify or Create a Security Group
#### Log in to the AWS Management Console:
1. Go to the **EC2 Dashboard**.
2. Locate **Security Groups**:
   - In the left menu under **Network & Security**, click on **Security Groups**.
3. **Create a New Security Group**:
   - Click on **Create Security Group**.
   - Provide the following details:
     - **Name**: (e.g., `Kubernetes-Cluster-SG`)
     - **Description**: A brief description for the security group (mandatory)
     - **VPC**: Select the appropriate VPC for your instances (default is acceptable)

#### Add Rules to the Security Group:
- **Allow SSH Traffic (Port 22):**
  ```
  Type: SSH
  Port Range: 22
  Source: 0.0.0.0/0 (Anywhere) or your specific IP
  ```
- **Allow Kubernetes API Traffic (Port 6443):**
  ```
  Type: Custom TCP
  Port Range: 6443
  Source: 0.0.0.0/0 (Anywhere) or specific IP ranges
  ```

#### Save the Rules:
- Click on **Create Security Group** to save the settings.

### Step 2: Select the Security Group While Creating Instances
When launching EC2 instances:
- Under **Configure Security Group**, select the existing security group (`Kubernetes-Cluster-SG`).

> **Note:** Security group settings can be updated later as needed.

## Execute on Both "Master" & "Worker" Nodes

### Disable Swap (Required for Kubernetes to function correctly):
```sh
sudo swapoff -a
```

### Load Necessary Kernel Modules (Required for Kubernetes networking):
```sh
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter
```

### Set Sysctl Parameters (Helps with networking):
```sh
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

sudo sysctl --system
lsmod | grep br_netfilter
lsmod | grep overlay
```

### Install Containerd:
```sh
sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo \"$VERSION_CODENAME\") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install -y containerd.io

containerd config default | sed -e 's/SystemdCgroup = false/SystemdCgroup = true/' -e 's/sandbox_image = "registry.k8s.io\/pause:3.6"/sandbox_image = "registry.k8s.io\/pause:3.9"/' | sudo tee /etc/containerd/config.toml

sudo systemctl restart containerd
sudo systemctl status containerd
```

### Install Kubernetes Components:
```sh
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gpg

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

## Execute ONLY on the "Master" Node

### Initialize the Cluster:
```sh
sudo kubeadm init
```

### Set Up Local kubeconfig:
```sh
mkdir -p "$HOME"/.kube
sudo cp -i /etc/kubernetes/admin.conf "$HOME"/.kube/config
sudo chown "$(id -u)":"$(id -g)" "$HOME"/.kube/config
```

### Install a Network Plugin (Calico):
```sh
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml
```

### Generate Join Command:
```sh
kubeadm token create --print-join-command
```
Copy this generated token for the next command.

## Execute on ALL of your Worker Nodes

### Perform Pre-flight Checks:
```sh
sudo kubeadm reset pre-flight checks
```

### Join the Worker Node to the Cluster:
Paste the join command you got from the master node and append `--v=5` at the end:
```sh
sudo kubeadm join <private-ip-of-control-plane>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash> --cri-socket "unix:///run/containerd/containerd.sock" --v=5
```

> **Note:** When pasting the join command from the master node:
> - Add `sudo` at the beginning of the command
> - Add `--v=5` at the end

### Example format:
```sh
sudo <paste-join-command-here> --v=5
```

## Verify Cluster Connection

### On Master Node:
```sh
kubectl get nodes
```

### Verify Container Status on Worker Node:
```sh
sudo systemctl status containerd
