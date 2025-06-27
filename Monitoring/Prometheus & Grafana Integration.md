
# Grafana + Prometheus on AWS: Step-by-Step Demo Guide

## Objective

Showcase how Prometheus collects metrics and how Grafana visualizes them using an EC2 instance on AWS.

---

## Pre-requisites

- AWS account
- SSH key pair
- Terminal or SSH client

---

## 1. Launch EC2 Instance

1. Go to AWS Console → EC2 → Launch Instance
2. Choose **Amazon Linux 2** (or **Ubuntu 20.04**)
3. Instance type: `t2.micro` (Free Tier eligible)
4. Configure the Security Group:
   - TCP 22 (SSH)
   - TCP 9090 (Prometheus)
   - TCP 9100 (Node Exporter)
   - TCP 3000 (Grafana)
5. Generate/download a key pair for SSH access
6. Launch the instance and SSH into it:

```bash
ssh -i your-key.pem ec2-user@<EC2-Public-IP>
```

---

## 2. Install Prometheus

```bash
sudo yum update -y
wget https://github.com/prometheus/prometheus/releases/download/v2.52.0/prometheus-2.52.0.linux-amd64.tar.gz
tar -xvf prometheus-2.52.0.linux-amd64.tar.gz
cd prometheus-2.52.0.linux-amd64
```

### Create Prometheus Config File:

```bash
cat <<EOF > prometheus.yml
global:
  scrape_interval: 15s
scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['localhost:9100']
EOF
```

### Start Prometheus:

```bash
./prometheus --config.file=prometheus.yml &
```

Access in browser: `http://<EC2-Public-IP>:9090`

---

## 3. Install Node Exporter

```bash
cd ~
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.1/node_exporter-1.8.1.linux-amd64.tar.gz
tar -xvf node_exporter-1.8.1.linux-amd64.tar.gz
cd node_exporter-1.8.1.linux-amd64
./node_exporter &
```

Access in browser: `http://<EC2-Public-IP>:9100/metrics`

---

## 4. Install Grafana

```bash
sudo tee /etc/yum.repos.d/grafana.repo<<EOF
[grafana]
name=grafana
baseurl=https://packages.grafana.com/oss/rpm
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packages.grafana.com/gpg.key
EOF

sudo yum install grafana -y
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

Access in browser: `http://<EC2-Public-IP>:3000`

- Username: `admin`
- Password: `admin`

---

## 5. Connect Prometheus to Grafana

1. Go to **Grafana > Settings > Data Sources**
2. Click **Add data source > Prometheus**
3. Set URL to `http://localhost:9090`
4. Click **Save & Test**

---

## 6. Import Dashboard

1. Go to **Dashboards > Import**
2. Use Dashboard ID: `1860` (Node Exporter Full)
3. Select your Prometheus data source
4. Click **Import**

---

## 7. Simulate Load (Optional)

Install stress to simulate CPU activity:

```bash
sudo amazon-linux-extras install epel -y
sudo yum install -y stress
stress --cpu 2 --timeout 60
```

---

## 8. Clean Up

- Stop or terminate the EC2 instance to avoid AWS charges

---

## 📌 Notes:

- Prometheus scrapes Node Exporter at port 9100 by default
- Grafana listens on port 3000
- Prometheus is accessed on port 9090
- All processes (Prometheus, Node Exporter) run in the background with `&`

---
![image](https://github.com/user-attachments/assets/c446b70b-2f9d-45ad-89c7-2d7946fc418a)
