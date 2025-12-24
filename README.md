# 🛡️ SysOps Project #2 — SSH Key Authentication & Hardening

## 📌 Project Overview
This project focuses on securing SSH access to a Linux server by replacing password-based authentication with SSH key-based authentication and applying basic SSH hardening techniques.

The goal is to simulate real-world SysOps security practices used to protect production servers from unauthorized access and brute-force attacks.

---

## 🎯 Objectives
- Enforce SSH key-based authentication
- Disable password-based SSH login
- Disable root login over SSH
- Apply baseline server hardening
- Document rollback and recovery steps

---

## 🏗️ Environment

### Host Machine
- OS: Windows 10 / 11
- Tool: Windows Terminal (PowerShell)

### Server
- OS: Ubuntu Linux
- Hypervisor: VirtualBox
- Network Mode: Host-only Adapter
- SSH Server: OpenSSH
- VM IP: `192.168.56.101`

---

## 🔐 Why This Matters
In real production environments:
- Password SSH is disabled
- Access is authenticated using cryptographic keys
- Root login is blocked
- SSH exposure is minimized

This project mirrors standard security baselines required in cloud and on-prem Linux systems.

---

## 🧪 Implementation Steps

### 1️⃣ Generate SSH Key Pair (Windows)

```powershell
ssh-keygen -t ed25519 -C "sysops-lab"
