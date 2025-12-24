# 🛡️ SysOps Project #2 — SSH Hardening & Secure Access (Ubuntu + Windows)

## 📌 Project Overview
This project demonstrates how to securely harden SSH access on a Linux server by:

- Moving SSH off the default port
- Disabling root login
- Restricting SSH access to specific users
- Understanding and resolving modern Ubuntu systemd socket activation behavior
- Verifying access from a Windows client

The project simulates a real SysOps hardening task commonly required in production environments.

---

## 🎯 Objectives
- Harden SSH access following best practices
- Change SSH listening port to 2222
- Prevent direct root login
- Allow SSH access only for a specific user
- Ensure access works reliably from Windows

---

## 🏗️ Environment

### Host Machine
- Windows 10
- PowerShell
- OpenSSH Client

### Server
- Ubuntu (VirtualBox VM)
- OpenSSH Server
- systemd

### Networking
- VirtualBox Host-Only Adapter
- Server IP: `192.168.56.101`

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
