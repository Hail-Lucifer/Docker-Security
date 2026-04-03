
<div align="center">

# 🐳 Secure Containerization with Docker

### Deployment · Hardening · DevSecOps Practices

![Status](https://img.shields.io/badge/STATUS-COMPLETED-brightgreen)
![Docker](https://img.shields.io/badge/Docker-26%2B-2496ED?logo=docker&logoColor=white)
![Debian](https://img.shields.io/badge/Debian-13(Trixie)-A81D33?logo=debian&logoColor=white)
![Security](https://img.shields.io/badge/Security-CIS%20Benchmark-8A2BE2)
![DevSecOps](https://img.shields.io/badge/DevSecOps-Practices-FF6B35)

![GPG](https://img.shields.io/badge/GPG-Verified-0078D4)
![Hardening](https://img.shields.io/badge/Hardening-Level%203-4CAF50)
![Audit](https://img.shields.io/badge/Audit-Ready-FF9800)

</div>


## 📖 Introduction

Containerization has become a key technology in modern IT infrastructures, enabling applications to run consistently across different environments. Docker is one of the most widely used containerization platforms, allowing developers and system administrators to package applications and their dependencies into lightweight, portable containers.

While Docker provides efficiency and scalability, it also introduces security challenges. Misconfigured containers, excessive privileges, and untrusted images can expose systems to significant risks. Therefore, securing Docker environments is essential in both development and production contexts.

This project focuses on the deployment and hardening of a Docker environment on Debian 13. It provides a structured, step-by-step approach covering installation, configuration, and the application of security best practices. The goal is to demonstrate how to use Docker effectively while minimizing potential security risks.

This document provides a complete walkthrough of the installation and hardening process, with step-by-step commands, security explanations, and verification screenshots to demonstrate real-world implementation.

---

## 📋 Executive Summary

This document provides a comprehensive, security-focused guide to deploying Docker on Debian 13. Unlike basic installation tutorials, this project emphasizes:

- 🔧 Security-first configuration from initial setup
- ⚖️ Least privilege principles throughout the container lifecycle
- 🏭 Production-ready hardening techniques
- 🚀 Practical DevSecOps implementation

The result is a hardened Docker environment suitable for enterprise deployments, demonstrating both technical proficiency and security awareness.

---

## 🎯 Project Objectives

The main objective of this project is to deploy and secure a Docker environment on a Debian 13 system. This includes understanding core Docker concepts, configuring containers, and applying security best practices to reduce potential risks.

More specifically, this project aims to:

- ✅ Install and configure Docker using official and secure methods
- ✅ Understand fundamental Docker components (images, containers, networking)
- ✅ Deploy and manage containers in a controlled environment
- ✅ Apply security best practices such as least privilege, container isolation, and trusted image usage
- ✅ Demonstrate a secure container deployment through practical implementation

This project follows a hands-on approach, combining theoretical understanding with practical implementation.

### 📊 Measurable Outcomes

| Objective | Success Criteria |
|-----------|------------------|
| Official & Secure Installation | Docker installed from official repository with GPG verification |
| Understanding Docker Components | Images pulled, containers running, networks configured |
| Controlled Environment | Containers deployed with resource limits and isolation |
| Security Best Practices | Least privilege applied, capability dropping, read-only filesystems |
| Practical Demonstration | Hardened container running with verified security configuration |

---

## 🖥️ Environment Preparation (Debian 13)

Before installing Docker, it is essential to prepare the system to ensure stability, compatibility, and security. This step includes updating the system, installing basic tools, and verifying the operating system information.

### 📊 System Specifications

| Component | Details |
|-----------|---------|
| OS | Debian 13 (Trixie) |
| Kernel | Linux 6.x |
| Architecture | x86_64 |
| Privileges | Root/sudo access required |

### ⚠️ Pre-Installation Security Considerations

Before installing Docker, consider these security factors:

- **🎯 Surface Attack Analysis:** Docker adds new attack vectors including container breakout, image vulnerabilities, and misconfigured networks
- **📋 Compliance Requirements:** Depending on your environment, you may need to consider CIS Docker Benchmarks, SOC2, or PCI-DSS requirements
- **🔒 Host Isolation:** Docker should run on dedicated or properly segmented infrastructure, especially in production

### 🔄 System Update & Upgrade

The system was updated and upgraded to ensure all existing packages are current, reducing potential vulnerabilities:

    sudo apt update
    sudo apt upgrade -y

<img width="671" height="163" alt="image" src="https://github.com/user-attachments/assets/49ce1972-b81c-444f-bdb7-cc4170b1a0f1" />


### 📦 Installation of Prerequisites

Basic utilities required for Docker installation and repository management were installed:

    sudo apt install -y ca-certificates curl gnupg lsb-release

<img width="541" height="415" alt="image" src="https://github.com/user-attachments/assets/9860372d-1f20-4322-8b5f-ac5d449cba28" />

These packages provide:
- 🔐 `ca-certificates`: SSL/TLS verification for secure connections
- 🌐 `curl`: Downloading files and GPG keys
- 🔑 `gnupg`: Cryptographic verification of packages
- 🏷️ `lsb-release`: Linux Standard Base information for repository detection

### ✅ System Verification

The system was verified to ensure no existing Docker installation and to confirm the operating system version:

    lsb_release -a
    uname -a

<img width="549" height="162" alt="image" src="https://github.com/user-attachments/assets/9e98917d-234e-4b6b-9e15-69ecd94287cc" />
    

---

## 🐳 Docker Installation

To ensure a secure and stable installation, Docker is installed using the official Docker repository rather than the default Debian repositories. This approach guarantees access to the latest features, updates, and security patches.

### 🔑 Add Docker GPG Key

Docker's official GPG key is added to verify package authenticity. This ensures that downloaded packages are trusted and have not been tampered with:

    sudo mkdir -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo chmod 644 /etc/apt/keyrings/docker.gpg
    ls -la /etc/apt/keyrings/

<img width="561" height="166" alt="image" src="https://github.com/user-attachments/assets/3c241220-0014-4684-98c1-7332db4e2151" />

### 📝 Add Docker Repository

The Docker repository is added to the system with automatic architecture and distribution detection:

    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list
    cat /etc/apt/sources.list.d/docker.list

<img width="565" height="94" alt="image" src="https://github.com/user-attachments/assets/133968f8-ccc5-4204-8992-458a06843dc1" />

### 🔍 Verify Repository

The repository configuration is verified by checking Docker package availability:

    sudo apt update
    apt-cache policy docker-ce

<img width="770" height="475" alt="image" src="https://github.com/user-attachments/assets/d1d3c39b-fe4d-4ed5-a0e5-a23b62e339e6" />
<img width="734" height="143" alt="image" src="https://github.com/user-attachments/assets/4ac3e487-e12e-4246-8be5-660fa8f5472f" />



### ⚙️ Install Docker Engine

Docker and its core components are installed:

    sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

<img width="796" height="313" alt="image" src="https://github.com/user-attachments/assets/52a7d111-0c9a-4bff-b382-ead0c6d918ef" />
<img width="737" height="295" alt="image" src="https://github.com/user-attachments/assets/ede58885-ba9d-433d-a209-d17741c5c2fe" />


### 📋 Components Installed

| Component | Purpose |
|-----------|---------|
| docker-ce | Docker Engine (Community Edition) |
| docker-ce-cli | Command-line interface for Docker |
| containerd.io | Container runtime |
| docker-buildx-plugin | Extended build capabilities |
| docker-compose-plugin | Multi-container orchestration |

### ✅ Verify Docker Service

The Docker service status is checked to ensure it is running properly:

    sudo systemctl status docker --no-pager

<img width="774" height="452" alt="image" src="https://github.com/user-attachments/assets/02a04aac-0046-40d7-adc5-5d4819fef351" />

### 🧪 Test Installation

A test container is run to verify Docker is functioning correctly:

    sudo docker run hello-world

<img width="605" height="541" alt="image" src="https://github.com/user-attachments/assets/a9e09a2e-f9a8-49b4-bbf0-b1e7342f88c8" />

    sudo docker version

<img width="554" height="527" alt="image" src="https://github.com/user-attachments/assets/00d4398a-23b8-4050-baa6-696c31474aa1" />

---

## 👤 Post-Installation User Configuration

To avoid using `sudo` with every Docker command, the user is added to the `docker` group:

    sudo groupadd docker
    sudo usermod -aG docker $USER
    newgrp docker
    groups
    docker version

<img width="809" height="631" alt="image" src="https://github.com/user-attachments/assets/c6bcb575-98b3-4616-b86e-4736aadec881" />

> 🔒 **Security Note:** Membership in the `docker` group grants root-equivalent privileges. Only add trusted users who require container management access.

---

## 🛡️ Security Hardening Section

After installing Docker, security hardening is applied to reduce the attack surface and ensure containers run with least privilege. This section covers daemon configuration, user namespace remapping, network isolation, and resource limitations.

### ⚙️ Docker Daemon Hardening

The Docker daemon is configured with security-focused options by creating or modifying the `/etc/docker/daemon.json` file:

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

<img width="533" height="310" alt="image" src="https://github.com/user-attachments/assets/4282de11-84c3-492e-bd01-b3bf9357f7db" />

### 📊 Security Configuration Explained

| Setting | Value | Purpose |
|---------|-------|---------|
| `icc` | `false` | Disables inter-container communication, preventing unauthorized container-to-container access |
| `log-driver` | `json-file` | Enables structured logging for audit and monitoring |
| `max-size` / `max-file` | `10m` / `3` | Prevents disk exhaustion attacks through log rotation |
| `userland-proxy` | `false` | Reduces attack surface by disabling userland proxy |
| `no-new-privileges` | `true` | Prevents privilege escalation within containers |
| `userns-remap` | `default` | Maps container root to non-privileged user on host |
| `live-restore` | `true` | Keeps containers running during daemon restarts |

After creating the configuration, restart the Docker daemon:

    sudo systemctl daemon-reload
    sudo systemctl restart docker
    docker info | grep -E "(Userns Mode|Security Options)"

<img width="667" height="97" alt="image" src="https://github.com/user-attachments/assets/6a12b9f2-a4aa-4442-9baa-21365b3f8b9d" />

---

## 🌐 Network Isolation

To enhance container isolation, a dedicated internal network is created. This network has no external access, forcing containers to communicate only through explicitly defined paths:

    docker network create \
      --driver bridge \
      --internal \
      --subnet=172.20.0.0/16 \
      secured_network
    docker network ls
    docker network inspect secured_network | grep -E "(Name|Subnet|Internal)"

<img width="826" height="332" alt="image" src="https://github.com/user-attachments/assets/60d70a96-d1ab-4b81-854f-c525c1c59e69" />


### 🔒 Network Security Benefits

- 🚫 `--internal`: No external network access, preventing unauthorized outbound connections
- 📍 `--subnet`: Dedicated IP range prevents IP conflicts
- 🔐 **Isolation:** Containers cannot communicate with external networks unless explicitly configured

---

## 📊 Resource Limiting

Resource limits prevent denial-of-service attacks where a single container consumes all host resources:

    docker run -d \
      --name hardened-nginx \
      --memory="256m" \
      --memory-swap="256m" \
      --cpus="0.5" \
      --pids-limit=100 \
      --restart=on-failure:3 \
      nginx:alpine

<img width="713" height="403" alt="image" src="https://github.com/user-attachments/assets/38ea87d9-3b26-4b01-b1ec-afadcdff6a1c" />


### 📈 Resource Limit Explanation

| Parameter | Value | Purpose |
|-----------|-------|---------|
| `--memory` | `256m` | Limits container to 256MB RAM |
| `--memory-swap` | `256m` | Disables swap usage (equal to memory limit) |
| `--cpus` | `0.5` | Limits container to 50% of one CPU core |
| `--pids-limit` | `100` | Restricts number of processes to prevent fork bombs |
| `--restart` | `on-failure:3` | Prevents infinite restart loops |

---

## 🧬 Runtime Security Flags

Additional security flags are applied when running containers to enforce least privilege:

    docker run -d \
      --name security-test \
      --read-only \
      --cap-drop=ALL \
      --security-opt=no-new-privileges:true \
      alpine:latest \
      sleep 3600

<img width="591" height="181" alt="image" src="https://github.com/user-attachments/assets/232ed041-7b32-4f9d-8f82-139c13128df3" />

### 🔐 Security Flag Explanations

| Flag | Purpose |
|------|---------|
| `--read-only` | Makes root filesystem read-only, preventing unauthorized modifications |
| `--cap-drop=ALL` | Removes all Linux capabilities |
| `--security-opt=no-new-privileges:true` | Prevents processes from gaining new privileges |
| `alpine:latest` | Lightweight base image ideal for security testing |
| `sleep 3600` | Keeps container running for verification |

---

## ✅ Verify Security Configuration

Verify that security settings are correctly applied:

    docker ps
    docker inspect security-test | grep -E "(Privileged|CapAdd|ReadonlyRootfs)"
    docker exec security-test whoami

<img width="1047" height="93" alt="image" src="https://github.com/user-attachments/assets/b0e2b553-1ff8-4a87-9577-4b8aa30680b6" />
<img width="846" height="74" alt="image" src="https://github.com/user-attachments/assets/6b4643ca-8db5-4b0a-b21c-f15944a732b6" />
<img width="460" height="36" alt="image" src="https://github.com/user-attachments/assets/694e0c90-5204-493c-a324-8e76064d0bfd" />


---

## 📦 Container Best Practices

This section covers essential security practices for maintaining containers in production environments, including image security, secrets management, and security auditing.

### 🖼️ Image Security

Always use trusted images and scan them for vulnerabilities:

    docker pull alpine:3.19
    docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}\t{{.CreatedAt}}"
    docker history alpine:3.19
    docker inspect alpine:3.19 | grep -E "(Size|Created|Os)" | head -6

<img width="724" height="108" alt="image" src="https://github.com/user-attachments/assets/6370b5ba-ef4b-4bb2-8780-60ebc344622c" />
<img width="913" height="91" alt="image" src="https://github.com/user-attachments/assets/71553502-a3a9-402b-8e6f-c87179f3fd50" />
<img width="1003" height="77" alt="image" src="https://github.com/user-attachments/assets/b5eafd00-034e-441f-9c86-3b26be87a788" />
<img width="760" height="76" alt="image" src="https://github.com/user-attachments/assets/23544006-4a1a-40bb-ac9b-5d1dcc306a4a" />


### ✅ Image Security Best Practices

- ✅ Use official images from Docker Hub verified publishers
- ✅ Pin images to specific versions (e.g., `alpine:3.19` not `alpine:latest`)
- ✅ Review image layers to understand what's included
- ✅ Minimize image size by using Alpine or slim variants
- ✅ Regularly update images to include security patches

---

## 🔐 Secrets Management

Never hardcode secrets in Dockerfiles, images, or environment variables. Use environment files with restricted permissions:

    echo "DB_PASSWORD=StrongP@ssw0rd" > .env
    echo "API_KEY=abc123xyz789" >> .env
    chmod 600 .env
    ls -la .env
    docker run --rm --env-file .env alpine:3.19 env | grep -E "(DB_PASSWORD|API_KEY)"
    rm -f .env

<img width="892" height="167" alt="image" src="https://github.com/user-attachments/assets/2fbf8ec2-3982-4525-b1d4-6d49bf882a6f" />


> ⚠️ **Security Warning:** Never commit `.env` files to version control. Add `.env` to `.gitignore`. Use `chmod 600` to restrict access to owner only. In production, use tools like Docker Secrets, HashiCorp Vault, or cloud secret managers.

---

## 🔍 Security Auditing Commands

Regular auditing helps identify misconfigurations and security gaps:

    docker ps -a
    echo "=== Privileged Containers ==="
    docker ps --quiet | xargs -I {} docker inspect {} --format '{{.Name}} | Privileged: {{.HostConfig.Privileged}}'
    echo "=== Container Capabilities ==="
    docker ps --quiet | xargs -I {} docker inspect {} --format '{{.Name}} | Capabilities: {{.HostConfig.CapAdd}}'
    echo "=== Exposed Ports ==="
    docker ps --format "table {{.Names}}\t{{.Ports}}"
    echo "=== Resource Usage ==="
    docker stats --no-stream
    echo "=== Recent Logs (security-test) ==="
    docker logs --tail 10 security-test 2>/dev/null || echo "Container not running"

<img width="874" height="630" alt="image" src="https://github.com/user-attachments/assets/5758f113-b109-437c-b6f9-5cc824c7e6ec" />


### 📋 Security Audit Checklist

- ✅ No containers running with `--privileged` flag
- ✅ No unnecessary Linux capabilities added
- ✅ Resource limits configured
- ✅ Logs being collected and rotated
- ✅ Only necessary ports exposed

---

## 📜 CIS Docker Benchmark

The Center for Internet Security (CIS) provides a Docker Benchmark with industry-standard security recommendations. Key recommendations include:

| CIS Control | Recommendation | Implementation |
|-------------|----------------|----------------|
| 1.1 | Ensure container host is up to date | `sudo apt update && sudo apt upgrade` |
| 2.1 | Use non-root user within containers | Run containers with `--user` flag |
| 2.4 | Enable user namespace remapping | Set `"userns-remap": "default"` in daemon.json |
| 3.2 | Ensure daemon.json file permissions are set to 644 | `sudo chmod 644 /etc/docker/daemon.json` |
| 4.1 | Ensure containers run with read-only root filesystem | Use `--read-only` flag |
| 5.1 | Ensure containers use trusted images | Use official images with specific tags |

### ✅ Regular Security Checklist

Use this checklist for ongoing container security:

- ✅ All images pulled with specific version tags
- ✅ Image layers reviewed for unexpected content
- ✅ Containers running with `--read-only` where possible
- ✅ No containers running in privileged mode
- ✅ All secrets stored securely (not in images or Dockerfiles)
- ✅ Resource limits configured for all containers
- ✅ Logging enabled with rotation
- ✅ User namespace remapping enabled
- ✅ Regular updates applied to host and images

---

## 🏁 Conclusion

This project successfully demonstrated the deployment and hardening of a Docker environment on Debian 13 with a focus on security best practices.

### ✅ Achievements

The following objectives were accomplished:

| Objective | Implementation |
|-----------|----------------|
| Secure Installation | Docker installed from official repository with GPG key verification |
| Daemon Hardening | `/etc/docker/daemon.json` configured with security parameters |
| User Management | Non-root user added to docker group with least privilege |
| Network Isolation | Internal network created with `--internal` flag |
| Runtime Security | Containers deployed with `--read-only`, `--cap-drop=ALL`, and `--security-opt=no-new-privileges` |
| Resource Control | Memory and CPU limits applied to prevent DoS attacks |
| Security Auditing | Container inspection commands demonstrated |
| Image Security | Specific version tags used instead of `latest` |
| Secrets Management | Environment files with restricted permissions (600) |

### 🧠 Skills Demonstrated

| Category | Skills |
|----------|--------|
| Containerization | Docker Engine, Docker CLI, Container Lifecycle Management |
| Security Hardening | Least Privilege, Capability Dropping, Read-only Filesystems, User Namespace Remapping |
| Linux Administration | Debian 13, Systemd, Package Management, GPG Key Verification |
| DevSecOps | Secure Configuration, Vulnerability Awareness, Secrets Management, Security Auditing |
| Documentation | Technical Writing, Procedure Documentation |

### 💡 Key Takeaways

1. **🔐 Security must be integrated from the start** — Installing Docker from official repositories with GPG verification prevents supply chain attacks

2. **🧱 Defense in depth is essential** — Combining daemon configuration, runtime flags, and network isolation creates multiple security layers

3. **⚖️ Least privilege reduces risk** — Dropping unnecessary capabilities and using read-only filesystems limits attack surfaces

4. **📊 Auditability enables compliance** — Documented configurations and inspection commands support security audits

### 🔮 Future Improvements

Opportunities to extend this project include:

- Implementing Docker Bench Security (CIS Benchmark) automated scanning
- Integrating with Docker Content Trust for image signing
- Deploying a containerized SIEM agent for log collection
- Implementing automated security scanning in CI/CD pipelines
- Exploring Kubernetes security for container orchestration

---

## 📝 Final Note

This project demonstrates practical skills in secure containerization, Linux system administration, and security-focused infrastructure deployment. All configurations and procedures have been validated on Debian 13.

---


*📅 Document completed: March 2026 | 👤 Prepared by: Yuri Montano*


