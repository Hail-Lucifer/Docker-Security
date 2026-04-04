# Docker Commands Reference – Security Hardened Project

This document lists every command used in this project, why it was used, and what it does.  
It follows the **security-first** approach: from system preparation to Docker daemon hardening, network isolation, resource limits, and auditing.

---

## 📦 Section 1: System Preparation

| Command | Name | What It Does | Why Used in Project |
|---------|------|--------------|----------------------|
| `sudo apt update` | APT Update | Updates package list from repositories | To refresh available packages before installing anything new |
| `sudo apt upgrade -y` | APT Upgrade | Upgrades all installed packages to latest versions | To ensure system has latest security patches before installing Docker |
| `sudo apt install -y ca-certificates curl gnupg lsb-release` | APT Install | Installs prerequisite packages | Required for: HTTPS (ca-certificates), downloads (curl), GPG verification (gnupg), OS detection (lsb-release) |
| `lsb_release -a` | LSB Release | Displays Linux distribution info | To verify running Debian 13 before installing Docker |
| `uname -a` | Unix Name | Displays system kernel information | To verify kernel version and system architecture |

---

## 🔐 Section 2: Docker Repository Setup

| Command | Name | What It Does | Why Used in Project |
|---------|------|--------------|----------------------|
| `sudo mkdir -p /etc/apt/keyrings` | Make Directory | Creates directory for GPG keys | To store Docker’s GPG key in a secure, dedicated location |
| `curl -fsSL https://download.docker.com/linux/debian/gpg \| sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg` | Curl + GPG | Downloads Docker’s GPG key and converts to binary format | To verify Docker packages are authentic and not tampered with |
| `sudo chmod 644 /etc/apt/keyrings/docker.gpg` | Change Mode | Sets file permissions to read-only for everyone | To prevent unauthorized modification of the GPG key |
| `ls -la /etc/apt/keyrings/` | List | Lists files with their permissions | To verify the GPG key was created with correct permissions |
| `echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" \| sudo tee /etc/apt/sources.list.d/docker.list` | Echo + Tee | Adds Docker’s official repository to APT sources | To install Docker from official source, not Debian’s older version |
| `cat /etc/apt/sources.list.d/docker.list` | Cat | Displays contents of the repository file | To verify the repository was added correctly |
| `apt-cache policy docker-ce` | APT Cache | Shows available versions and installation source | To confirm Docker is available from official repository |

---

## 🐳 Section 3: Docker Installation

| Command | Name | What It Does | Why Used in Project |
|---------|------|--------------|----------------------|
| `sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin` | APT Install | Installs Docker Engine and components | To install Docker with all necessary components from official repository |
| `sudo systemctl status docker --no-pager` | System Control | Shows Docker service status | To verify Docker daemon is running properly after installation |
| `sudo docker run hello-world` | Docker Run | Downloads and runs a test container | To verify Docker is working correctly with a simple test |
| `sudo docker version` | Docker Version | Displays Docker client and server version | To verify Docker is installed and showing correct version info |

---

## 👤 Section 4: Post-Installation User Configuration

| Command | Name | What It Does | Why Used in Project |
|---------|------|--------------|----------------------|
| `sudo groupadd docker` | Group Add | Creates the docker group | To create a group that grants Docker access without sudo |
| `sudo usermod -aG docker $USER` | User Modify | Adds current user to docker group | To allow running Docker commands without sudo (convenience, but security note added) |
| `newgrp docker` | New Group | Refreshes group membership | To apply docker group membership without logging out |
| `groups` | Groups | Lists groups the user belongs to | To verify user was successfully added to docker group |
| `docker version` | Docker Version | Displays Docker version | To confirm Docker works without sudo after group membership |

---

## 🛡️ Section 5: Docker Daemon Hardening

| Command | Name | What It Does | Why Used in Project |
|---------|------|--------------|----------------------|
| `sudo mkdir -p /etc/docker` | Make Directory | Creates Docker configuration directory | To create directory where daemon.json will be stored |
| `sudo tee /etc/docker/daemon.json <<EOF ... EOF` | Tee + Here Document | Creates/overwrites daemon.json with security settings | To configure Docker daemon with security hardening options |
| `sudo systemctl daemon-reload` | Systemctl Daemon Reload | Reloads systemd configuration | To make systemd aware of changes to Docker service configuration |
| `sudo systemctl restart docker` | Systemctl Restart | Restarts Docker service | To apply new daemon.json configuration |
| `docker info \| grep -E "(Userns Mode\|Security Options)"` | Docker Info + Grep | Shows Docker security configuration | To verify security options (userns-remap, etc.) are enabled |

### `daemon.json` settings explained:

| Setting | Purpose |
|---------|---------|
| `"icc": false` | Prevents containers from talking to each other without explicit configuration |
| `"log-driver": "json-file"` | Enables structured logging for security auditing |
| `"max-size": "10m"` | Limits log size to prevent disk exhaustion attacks |
| `"no-new-privileges": true` | Prevents privilege escalation inside containers |
| `"userns-remap": "default"` | Maps container root to non-privileged user on host |

---

## 🌐 Section 6: Network Isolation

| Command | Name | What It Does | Why Used in Project |
|---------|------|--------------|----------------------|
| `docker network create --driver bridge --internal --subnet=172.20.0.0/16 secured_network` | Docker Network Create | Creates an isolated internal network | To prevent containers from accessing internet or host network, increasing isolation |
| `docker network ls` | Docker Network List | Lists all Docker networks | To verify new network was created |
| `docker network inspect secured_network \| grep -E "(Name\|Subnet\|Internal)"` | Docker Network Inspect | Shows detailed network configuration | To verify network is internal and has correct subnet |

---

## ⚙️ Section 7: Resource Limits & Security Flags

| Command | Name | What It Does | Why Used in Project |
|---------|------|--------------|----------------------|
| `docker run -d --name hardened-nginx --memory="256m" --memory-swap="256m" --cpus="0.5" --pids-limit=100 --restart=on-failure:3 nginx:alpine` | Docker Run | Runs Nginx with resource limits | To prevent DoS attacks where one container consumes all host resources |
| `docker run -d --name security-test --read-only --cap-drop=ALL --security-opt=no-new-privileges:true alpine:latest sleep 3600` | Docker Run | Runs Alpine with security hardening | To demonstrate a hardened container with: read-only filesystem, no capabilities, no privilege escalation |
| `docker ps` | Docker PS | Lists running containers | To verify containers are running |

---

## 🔍 Section 8: Security Verification

| Command | Name | What It Does | Why Used in Project |
|---------|------|--------------|----------------------|
| `docker inspect security-test \| grep -E "(Privileged\|CapAdd\|ReadonlyRootfs)"` | Docker Inspect + Grep | Shows container security settings | To verify container is running with: no privileged mode, dropped capabilities, read-only filesystem |
| `docker exec security-test whoami` | Docker Exec | Runs a command inside the container | To show container runs as root (but with restricted capabilities) |

---

## 🖼️ Section 9: Image Security

| Command | Name | What It Does | Why Used in Project |
|---------|------|--------------|----------------------|
| `docker pull alpine:3.19` | Docker Pull | Downloads a specific image version | To demonstrate using specific tags instead of 'latest' for reproducibility |
| `docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}\t{{.CreatedAt}}"` | Docker Images | Lists images with formatting | To see which images are present and their sizes |
| `docker history alpine:3.19` | Docker History | Shows image layers | To understand what's inside the image and verify it's minimal |
| `docker inspect alpine:3.19 \| grep -E "(Size\|Created\|Os)"` | Docker Inspect | Shows image metadata | To verify image details including creation date and size |

---

## 🤫 Section 10: Secrets Management

| Command | Name | What It Does | Why Used in Project |
|---------|------|--------------|----------------------|
| `echo "DB_PASSWORD=StrongP@ssw0rd" > .env` | Echo | Creates a file with secret content | To demonstrate storing secrets in a file (never in Dockerfiles) |
| `chmod 600 .env` | Change Mode | Restricts file permissions | To ensure only owner can read/write the secret file |
| `ls -la .env` | List | Shows file permissions | To verify file has restricted permissions (600) |
| `docker run --rm --env-file .env alpine:3.19 env \| grep -E "(DB_PASSWORD\|API_KEY)"` | Docker Run with Env File | Passes secrets to container | To demonstrate using environment files for secrets |
| `rm -f .env` | Remove | Deletes the secret file | To clean up secrets after use |

---

## 📋 Section 11: Security Auditing

| Command | Name | What It Does | Why Used in Project |
|---------|------|--------------|----------------------|
| `docker ps -a` | Docker PS All | Lists all containers (including stopped) | To see complete state of all containers |
| `docker ps --quiet \| xargs -I {} docker inspect {} --format '{{.Name}} \| Privileged: {{.HostConfig.Privileged}}'` | Docker PS + Xargs + Inspect | Shows which containers are privileged | To audit for security risk (privileged containers) |
| `docker ps --quiet \| xargs -I {} docker inspect {} --format '{{.Name}} \| Capabilities: {{.HostConfig.CapAdd}}'` | Docker PS + Xargs + Inspect | Shows container capabilities | To audit for unnecessary Linux capabilities |
| `docker ps --format "table {{.Names}}\t{{.Ports}}"` | Docker PS Format | Shows exposed ports | To audit which ports are exposed to the host |
| `docker stats --no-stream` | Docker Stats | Shows container resource usage | To monitor resource consumption and detect potential DoS attacks |
| `docker logs --tail 10 security-test` | Docker Logs | Shows container logs | To audit container activity and detect suspicious behavior |

---

## ⚡ Quick Reference Card

| Category | Most Important Commands |
|----------|------------------------|
| **Installation** | `sudo apt update`, `sudo apt install docker-ce`, `sudo docker run hello-world` |
| **Hardening** | `sudo tee /etc/docker/daemon.json`, `sudo systemctl restart docker` |
| **Security Flags** | `--read-only`, `--cap-drop=ALL`, `--security-opt=no-new-privileges:true` |
| **Verification** | `docker inspect`, `docker exec`, `docker ps` |
| **Auditing** | `docker inspect \| grep Privileged`, `docker stats`, `docker logs` |

---

## 📌 Notes

- All `sudo` commands are required unless user is added to `docker` group (see Section 4)
- Security hardening is **optional but recommended** for production
- Never store secrets in Dockerfiles or images — use `.env` files or secret managers
- Always prefer specific image tags (`alpine:3.19`) over `latest` for reproducibility

---

