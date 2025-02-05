# DevOps-Commands
## Update the Packages



```sh
# Switch to root user
sudo su
sudo apt update -y
```

## Install AWS CLI
```sh
sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

## Install Jenkins on Ubuntu
(Reference: [Jenkins Installation Guide](https://www.jenkins.io/doc/book/installing/linux/#debianubuntu))

```sh
#!/bin/bash
sudo apt update -y
wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo tee /etc/apt/keyrings/adoptium.asc
echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
sudo apt update -y
sudo apt install temurin-17-jdk -y
/usr/bin/java --version

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y
sudo apt-get install jenkins -y
sudo systemctl start jenkins
sudo systemctl status jenkins
```

### Verify Jenkins Installation
```sh
jenkins --version
```

---

## Install Docker on Ubuntu
(Reference: [Docker Installation Guide](https://docs.docker.com/engine/install/ubuntu/))

### Add Docker's Official GPG Key:
```sh
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

### Add the Repository to Apt Sources:
```sh
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Install Docker:
```sh
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo usermod -aG docker ubuntu
sudo chmod 777 /var/run/docker.sock
newgrp docker
sudo systemctl status docker
```

### Verify Docker Installation:
```sh
docker --version
```

---

## Install Trivy on Ubuntu
(Reference: [Trivy Installation Guide](https://aquasecurity.github.io/trivy/v0.55/getting-started/installation/))

```sh
sudo apt-get install wget apt-transport-https gnupg
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy
```

### Verify Trivy Installation:
```sh
trivy --version
```

---

## Install SonarQube using Docker
```sh
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
docker ps  # Check if the SonarQube container is running
```
## Installing Prometheus

First, create a dedicated Linux user for Prometheus and download Prometheus:

```sh
sudo useradd --system --no-create-home --shell /bin/false prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.47.1/prometheus-2.47.1.linux-amd64.tar.gz
```

Extract Prometheus files, move them, and create directories:

```sh
tar -xvf prometheus-2.47.1.linux-amd64.tar.gz
cd prometheus-2.47.1.linux-amd64/
sudo mkdir -p /data /etc/prometheus
sudo mv prometheus promtool /usr/local/bin/
sudo mv consoles/ console_libraries/ /etc/prometheus/
sudo mv prometheus.yml /etc/prometheus/prometheus.yml
```

Set ownership for directories:

```sh
sudo chown -R prometheus:prometheus /etc/prometheus/ /data/
```

Create a systemd unit configuration file for Prometheus:

```sh
sudo vi /etc/systemd/system/prometheus.service
```

Add the following content to the `prometheus.service` file:

```ini
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
User=prometheus
Group=prometheus
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/data \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries \
  --web.listen-address=0.0.0.0:9090 \
  --web.enable-lifecycle

[Install]
WantedBy=multi-user.target
```

Enable and start Prometheus:

```sh
sudo systemctl enable prometheus
sudo systemctl start prometheus
```

Verify Prometheus's status:

```sh
sudo systemctl status prometheus
```

Access Prometheus in a browser:

```sh
http://<your-server-ip>:9090
```

If it doesn't work, remove 's' from 'https'.

---

##  Installing Node Exporter

Create a system user for Node Exporter and download Node Exporter:

```sh
sudo useradd --system --no-create-home --shell /bin/false node_exporter
wget https://github.com/prometheus/node_exporter/releases/download/v1.6.1/node_exporter-1.6.1.linux-amd64.tar.gz
```

Extract Node Exporter files, move the binary, and clean up:

```sh
tar -xvf node_exporter-1.6.1.linux-amd64.tar.gz
sudo mv node_exporter-1.6.1.linux-amd64/node_exporter /usr/local/bin/
rm -rf node_exporter*
```

Create a systemd unit configuration file for Node Exporter:

```sh
sudo vi /etc/systemd/system/node_exporter.service
```

Add the following content to the `node_exporter.service` file:

```ini
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/node_exporter --collector.logind

[Install]
WantedBy=multi-user.target
```

Enable and start Node Exporter:

```sh
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
```

Verify the Node Exporter's status:

```sh
sudo systemctl status node_exporter
```

---

## Configure Prometheus Plugin Integration

Modify `prometheus.yml` to include Node Exporter and Jenkins jobs:

```sh
cd /etc/prometheus/
sudo vi prometheus.yml
```

Append the following content:

```yaml
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['<MonitoringVMip>:9100']

  - job_name: 'jenkins'
    metrics_path: '/prometheus'
    static_configs:
      - targets: ['<your-jenkins-ip>:<your-jenkins-port>']
```

Validate configuration:

```sh
promtool check config /etc/prometheus/prometheus.yml
```

Reload Prometheus configuration:

```sh
curl -X POST http://localhost:9090/-/reload
```

---

## Install Grafana

Install dependencies:

```sh
sudo apt-get update
sudo apt-get install -y apt-transport-https software-properties-common
```

Add the GPG key for Grafana:

```sh
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```

Add Grafana repository:

```sh
echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

Update and install Grafana:

```sh
sudo apt-get update
sudo apt-get -y install grafana
```

Enable and start Grafana:

```sh
sudo systemctl enable grafana-server
sudo systemctl start grafana-server
```

Verify Grafana status:

```sh
sudo systemctl status grafana-server
```

Access Grafana web interface:

```sh
http://<monitoring-server-ip>:3000
```

Default credentials: `admin/admin`.

---

## Creation of EKS Cluster

Create EKS Cluster using `eksctl`:

```sh
eksctl create cluster --name=kastrocluster \
                      --region=ap-northeast-1 \
                      --zones=ap-northeast-1a,ap-northeast-1c \
                      --without-nodegroup
```

Verify cluster creation:

```sh
eksctl get cluster
```

Create and associate IAM OIDC Provider:

```sh
eksctl utils associate-iam-oidc-provider --region ap-northeast-1 --cluster kastrocluster --approve
```

Create Node Group:

```sh
eksctl create nodegroup --cluster=kastrocluster \
                       --region=ap-northeast-1 \
                       --name=kastrodemo-ng-public1 \
                       --node-type=t3.medium \
                       --nodes=2 \
                       --nodes-min=2 \
                       --nodes-max=4 \
                       --node-volume-size=20 \
                       --ssh-access \
                       --ssh-public-key=Prajwal \
                       --managed \
                       --asg-access \
                       --external-dns-access \
                       --full-ecr-access \
                       --appmesh-access \
                       --alb-ingress-access
```

Verify Cluster & Nodes in AWS EKS Service.

---

Delete Node Group:

```sh
eksctl delete nodegroup --cluster=kastrocluster --name=kastrodemo-ng-public1
```

Delete Cluster:

```sh
eksctl delete cluster kastrocluster
