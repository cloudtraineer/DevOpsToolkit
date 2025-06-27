# Grafana + Prometheus on AWS: Step-by-Step Demo Guide

## Objective

Showcase how Prometheus collects metrics and how Grafana visualizes them using an EC2 instance on AWS.

---

## Pre-requisites

- AWS account
- Basic understanding of Linux commands
- SSH key pair

---

## 1. Launch EC2 Instance

- Go to AWS Console > EC2 > Launch Instance
- Choose Amazon Linux 2 (or Ubuntu 20.04)
- Instance type: `t2.micro` (Free Tier eligible)
- Configure Security Group:
  - TCP 22 (SSH)
  - TCP 9090 (Prometheus)
  - TCP 9100 (Node Exporter)
  - TCP 3000 (Grafana)
- Launch and SSH into the instance

---

## 2. Install Prometheus

```bash
sudo yum update -y
wget https://github.com/prometheus/prometheus/releases/download/v2.52.0/prometheus-2.52.0.linux-amd64.tar.gz
tar -xvf prometheus-2.52.0.linux-amd64.tar.gz
cd prometheus-2.52.0.linux-amd64
