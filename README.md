<div align="center">

# 🐳 Secure Containerization with Docker

## Deployment · Hardening · DevSecOps Practices

![Project Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Docker](https://img.shields.io/badge/Docker-Engine-blue)
![Debian](https://img.shields.io/badge/Debian-13(Trixie)-red)
![Security](https://img.shields.io/badge/Security-CIS%20Benchmark-purple)

**A comprehensive, security-first guide to deploying and hardening Docker on Debian 13**

</div>

---

## 📌 What is this project?

This project provides a **security-focused walkthrough** for deploying Docker on Debian 13 (Trixie). Unlike basic installation tutorials, this guide emphasizes:

- **Security-first configuration** from initial setup
- **Least privilege principles** throughout the container lifecycle
- **Production-ready hardening** techniques
- **Practical DevSecOps** implementation

The result is a hardened Docker environment suitable for enterprise deployments, demonstrating both technical proficiency and security awareness.

---

## 🎯 Project Objectives

| Objective | Implementation |
|-----------|----------------|
| Official & Secure Installation | Docker installed from official repository with GPG verification |
| Daemon Hardening | `/etc/docker/daemon.json` configured with security parameters |
| User Management | Non-root user added to docker group with least privilege |
| Network Isolation | Internal network created with `--internal` flag |
| Runtime Security | Containers deployed with `--read-only`, `--cap-drop=ALL` |
| Resource Control | Memory and CPU limits applied to prevent DoS attacks |
| Security Auditing | Container inspection and monitoring commands |
| Image Security | Specific version tags used instead of `latest` |
| Secrets Management | Environment files with restricted permissions (600) |

---

## 🛡️ Security Hardening Implemented

### Docker Daemon Configuration

```json
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


```  
---

## 📂 Full Documentation

👉 [Click here for the complete Secure Containerization Report](./Secure-Containerization-With-Docker.md)

---



