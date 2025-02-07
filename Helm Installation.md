**Helm Installation Guide** 


# Helm Installation Guide

Helm is a package manager for Kubernetes that allows you to define, install, and manage Kubernetes applications.

## Prerequisites
- A Kubernetes cluster is already set up.
- `kubectl` is installed and configured to interact with your cluster.
- `curl` or `wget` is installed.

## Installing Helm

### Step 1: Download and Install Helm

#### Using Script (Recommended)
Run the following command to install Helm automatically:

```bash
curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

#### Using Package Managers

**For Ubuntu/Debian (APT)**
```bash
sudo apt update && sudo apt install -y apt-transport-https
curl -fsSL https://baltocdn.com/helm/signing.asc | sudo tee /etc/apt/trusted.gpg.d/helm.asc > /dev/null
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt update && sudo apt install -y helm
```

**For macOS (Homebrew)**
```bash
brew install helm
```

**For Windows (Chocolatey)**
```powershell
choco install kubernetes-helm
```

**For Windows (Scoop)**
```powershell
scoop install helm
```

**For RHEL/CentOS (YUM)**
```bash
curl -fsSL https://baltocdn.com/helm/signing.asc | sudo tee /etc/yum.repos.d/helm.repo > /dev/null
sudo yum install helm
```

**For Arch Linux**
```bash
pacman -S helm
```

### Step 2: Verify Helm Installation
After installation, verify that Helm is installed correctly by checking its version:
```bash
helm version
```

You should see output similar to:
```
version.BuildInfo{Version:"v3.x.x", GitCommit:"xxxxx", GitTreeState:"clean", GoVersion:"go1.x.x"}
```

### Step 3: Add Helm Repositories
Helm uses repositories to fetch charts. Add the default Helm stable repository:
```bash
helm repo add stable https://charts.helm.sh/stable
helm repo update
```

### Step 4: Install a Test Chart (Optional)
To test your Helm installation, deploy the **nginx** chart:
```bash
helm install my-nginx stable/nginx-ingress --set controller.publishService.enabled=true
```

To check the deployed Helm release:
```bash
helm list
```

## Uninstalling Helm
To remove Helm from your system, use:
```bash
sudo rm /usr/local/bin/helm  # If installed via script
sudo apt remove helm         # For Ubuntu/Debian
brew uninstall helm          # For macOS
choco uninstall kubernetes-helm # For Windows (Chocolatey)
```

## Conclusion
You have successfully installed Helm and configured it for your Kubernetes cluster. You can now use Helm to deploy and manage applications efficiently.

For more details, visit the official documentation: [Helm Docs](https://helm.sh/docs/)
