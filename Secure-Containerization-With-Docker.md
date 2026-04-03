
# 🐳 Secure Containerization with Docker  
### Deployment, Hardening & DevSecOps Practices

![Docker](https://img.shields.io/badge/Docker-Containerization-blue?logo=docker)
![Linux](https://img.shields.io/badge/Linux-Debian%2013-red?logo=debian)
![Security](https://img.shields.io/badge/Security-Hardening-green)
![DevSecOps](https://img.shields.io/badge/DevSecOps-Practices-purple)
![Status](https://img.shields.io/badge/Project-Completed-brightgreen)

---

## 👤 Author
**Yuri Montano**  
📅 March 2026  

---

## 📋 Project Overview

This project demonstrates how to **securely deploy and harden Docker on Debian 13**, following real-world **DevSecOps and security best practices**.

Unlike basic tutorials, this project focuses on:

- 🔧 Security-first Docker setup  
- 🛡 Defense-in-depth architecture  
- ⚙️ Production-ready configurations  
- 🔬 Container hardening techniques  

---

## 🎯 Objectives

- Install Docker securely using official repositories  
- Understand core Docker components  
- Deploy containers in a controlled environment  
- Apply security best practices (least privilege, isolation)  
- Demonstrate a hardened production-ready setup  

---

## 🧰 Tech Stack

| Category | Tools |
|--------|------|
| OS | Debian 13 (Trixie) |
| Containerization | Docker Engine |
| Security | CIS Benchmark Practices |
| Networking | Docker Bridge Networks |
| DevOps | CLI, Systemd |

---

## 🏗 Installation (Secure Method)

### 1. Update System

    sudo apt update && sudo apt upgrade -y

### 2. Install Dependencies

    sudo apt install -y ca-certificates curl gnupg lsb-release

### 3. Add Docker GPG Key

    sudo mkdir -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/debian/gpg \
    | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo chmod 644 /etc/apt/keyrings/docker.gpg

### 4. Add Repository

    echo "deb [arch=$(dpkg --print-architecture) \
    signed-by=/etc/apt/keyrings/docker.gpg] \
    https://download.docker.com/linux/debian \
    $(lsb_release -cs) stable" \
    | sudo tee /etc/apt/sources.list.d/docker.list

### 5. Install Docker

    sudo apt update
    sudo apt install -y docker-ce docker-ce-cli containerd.io \
    docker-buildx-plugin docker-compose-plugin

---

## 🧱 Security Hardening

### Docker Daemon Configuration

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

### Key Protections

- ⛔ Inter-container communication disabled  
- 📄 Log rotation to prevent disk exhaustion  
- 🚫 Privilege escalation blocked  
- 👥 Root mapped to non-privileged user  
- 🔄 Containers survive daemon restart  

---

## 🔌 Network Isolation

    docker network create \
      --driver bridge \
      --internal \
      --subnet=172.20.0.0/16 \
      secured_network

✅ No external internet access  
✅ Isolated container communication  

---

## 📊 Resource Limiting

    docker run -d \
      --memory="256m" \
      --cpus="0.5" \
      --pids-limit=100 \
      nginx:alpine

✅ Prevents DoS attacks  
✅ Controls CPU & memory usage  

---

## 🧬 Runtime Security

    docker run -d \
      --read-only \
      --cap-drop=ALL \
      --security-opt=no-new-privileges:true \
      alpine sleep 3600

✅ Read-only filesystem  
✅ No Linux capabilities  
✅ No privilege escalation  

---

## 🖼️ Image Security

    docker pull alpine:3.19

### Best Practices

- ✅ Use official images  
- ⚠️ Avoid `latest` tag  
- 📉 Keep images minimal  
- 🔄 Update regularly  

---

## 🔐 Secrets Management

    echo "DB_PASSWORD=StrongP@ssw0rd" > .env
    chmod 600 .env
    docker run --env-file .env alpine env

⚠️ Never commit `.env` files to GitHub  

---

## 🔍 Security Auditing

    docker ps -a
    docker stats --no-stream
    docker inspect <container>

✅ Detect misconfigurations  
✅ Monitor resource usage  
✅ Verify security settings  

---

## ✅ Achievements

- ✅ Secure Docker installation (GPG verified)  
- ✅ Hardened daemon configuration  
- ✅ Network isolation implemented  
- ✅ Runtime security enforced  
- ✅ Resource limits applied  
- ✅ Security auditing demonstrated  

---

## 🧠 Skills Demonstrated

- Docker & container lifecycle  
- Linux system administration  
- Security hardening & best practices  
- DevSecOps mindset  
- Technical documentation  

---

## 💡 Key Takeaways

- 🧩 Security must start at installation  
- 🧱 Defense in depth is essential  
- ⚖️ Least privilege reduces attack surface  
- 📊 Auditing ensures compliance  

---

## 🔮 Future Improvements

- CIS Benchmark automated scanning  
- Docker Content Trust (image signing)  
- SIEM integration for logging  
- CI/CD security pipelines  
- Kubernetes security exploration  

---

## 📎 Final Thoughts

This project demonstrates **real-world, production-level Docker security skills**, combining:

- Practical implementation  
- Security awareness  
- DevOps best practices  

---

⭐ *If you found this useful, feel free to star the repo!*



**The fix:** I replaced all inner ```bash and ```json blocks with simple indented code blocks (4 spaces at the start of each line). This keeps everything inside the outer Markdown code block without breaking the formatting.
