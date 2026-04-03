# Secure Containerization with Docker: Deployment, Hardening and DevSecOps Practices

**Prepared by:** Yuri Montano  
**Date:** March 2026  

---

## Introduction

Containerization has become a key technology in modern IT infrastructures, enabling applications to run consistently across different environments. Docker is one of the most widely used containerization platforms, allowing developers and system administrators to package applications and their dependencies into lightweight, portable containers.

While Docker provides efficiency and scalability, it also introduces security challenges. Misconfigured containers, excessive privileges, and untrusted images can expose systems to significant risks. Therefore, securing Docker environments is essential in both development and production contexts.

This project focuses on the deployment and hardening of a Docker environment on Debian 13. It provides a structured, step-by-step approach covering installation, configuration, and the application of security best practices.

---

## Executive Summary

This document provides a comprehensive, security-focused guide to deploying Docker on Debian 13.

### Key Focus Areas

- Security-first configuration from initial setup  
- Least privilege principles throughout the container lifecycle  
- Production-ready hardening techniques  
- Practical DevSecOps implementation  

---

## Project Objectives

The main objective is to deploy and secure a Docker environment on Debian 13.

### Goals

- Install Docker securely using official methods  
- Understand Docker components (images, containers, networking)  
- Deploy and manage containers safely  
- Apply security best practices  
- Demonstrate secure container deployment  

---

## Measurable Outcomes

| Objective | Success Criteria |
|----------|----------------|
| Installation | Docker installed with GPG verification |
| Components | Containers running, networks configured |
| Environment | Resource limits and isolation applied |
| Security | Least privilege and hardening applied |
| Demo | Hardened container validated |

---

## Environment Preparation (Debian 13)

### System Specifications

| Component | Details |
|----------|--------|
| OS | Debian 13 (Trixie) |
| Kernel | Linux 6.x |
| Architecture | x86_64 |
| Privileges | sudo/root |

---

## Pre-Installation Security Considerations

- **Attack Surface:** container breakout, image vulnerabilities  
- **Compliance:** CIS, SOC2, PCI-DSS  
- **Host Isolation:** dedicated or segmented infrastructure  

---

## System Update

```bash
sudo apt update
sudo apt upgrade -y
```

---

## Install Prerequisites

```bash
sudo apt install -y ca-certificates curl gnupg lsb-release
```

---

## System Verification

```bash
lsb_release -a
uname -a
```

---

## Docker Installation

### Add GPG Key

```bash
sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/debian/gpg \
| sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

sudo chmod 644 /etc/apt/keyrings/docker.gpg
```

---

### Add Repository

```bash
echo "deb [arch=$(dpkg --print-architecture) \
signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/debian \
$(lsb_release -cs) stable" \
| sudo tee /etc/apt/sources.list.d/docker.list
```

---

### Install Docker

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io \
docker-buildx-plugin docker-compose-plugin
```

---

### Verify Installation

```bash
sudo systemctl status docker --no-pager
sudo docker run hello-world
docker version
```

---

## Post-Installation User Setup

```bash
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
docker version
```

> ⚠️ Docker group = root-level privileges

---

## Security Hardening

### Docker Daemon Configuration

```bash
sudo mkdir -p /etc/docker

sudo tee /etc/docker/daemon.json <<EOF
{
  "icc": false,
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  },
  "userland-proxy": false,
  "no-new-privileges": true,
  "userns-remap": "default",
  "live-restore": true
}
EOF
```

Restart Docker:

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

---

## Network Isolation

```bash
docker network create \
  --driver bridge \
  --internal \
  --subnet=172.20.0.0/16 \
  secured_network
```

---

## Resource Limiting

```bash
docker run -d \
  --name hardened-nginx \
  --memory="256m" \
  --cpus="0.5" \
  --pids-limit=100 \
  nginx:alpine
```

---

## Runtime Security

```bash
docker run -d \
  --name security-test \
  --read-only \
  --cap-drop=ALL \
  --security-opt=no-new-privileges:true \
  alpine sleep 3600
```

---

## Image Security

```bash
docker pull alpine:3.19
docker images
docker history alpine:3.19
```

### Best Practices

- Use official images  
- Avoid `latest`  
- Keep images minimal  
- Update regularly  

---

## Secrets Management

```bash
echo "DB_PASSWORD=StrongP@ssw0rd" > .env
chmod 600 .env

docker run --env-file .env alpine env
rm -f .env
```

> ❗ Never commit `.env` files

---

## Security Auditing

```bash
docker ps -a

docker inspect $(docker ps -q) \
--format '{{.Name}} | Privileged: {{.HostConfig.Privileged}}'

docker stats --no-stream
```

---

## CIS Benchmark Highlights

| Control | Recommendation |
|--------|---------------|
| 1.1 | Keep system updated |
| 2.1 | Use non-root containers |
| 2.4 | Enable userns-remap |
| 4.1 | Read-only filesystem |
| 5.1 | Trusted images only |

---

## Conclusion

This project demonstrated secure Docker deployment on Debian 13 using best practices.

---

## Achievements

| Objective | Implementation |
|----------|---------------|
| Installation | GPG verified repo |
| Hardening | daemon.json configured |
| Isolation | internal network |
| Security | runtime flags |
| Resources | limits applied |

---

## Skills Demonstrated

- Docker & container lifecycle  
- Linux administration  
- Security hardening  
- DevSecOps practices  
- Technical documentation  

---

## Key Takeaways

1. Security must start from installation  
2. Defense in depth is critical  
3. Least privilege reduces risk  
4. Auditing ensures compliance  

---

## Future Improvements

- CIS automated scanning  
- Docker Content Trust  
- SIEM integration  
- CI/CD security pipelines  
- Kubernetes security  

---

## Final Note

This project demonstrates practical DevSecOps and container security skills validated on Debian 13.
