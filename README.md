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

## ⚠️ Problem Encountered
After correctly configuring SSH to use port 2222, the server continued listening on port 22, even after restarting the SSH service.

```sudo ss -tulpn | grep ssh```

Output:

```LISTEN 0 4096 0.0.0.0:22```

Despite:
- Valid configuration
- Successful `sshd -T` validation
- Multiple restarts

---

## 🔍 Root Cause Analysis
Modern Ubuntu versions (22.04+) use systemd socket activation for SSH:
- `ssh.socket` pre-binds port `22`
- `sshd_config` port settings are ignored while socket activation is enabled
- SSH inherits the socket from systemd instead of opening its own port

Verification command:

```sudo sshd -T | grep port```

Output:

```port 2222```

This confirmed the configuration was correct, but not applied at runtime.

---

## 🛠️ Solution

### 1️⃣ Create a Hardening Override File
Instead of modifying default configs, a custom override file was created:

```sudo nano /etc/ssh/sshd_config.d/99-hardening.conf```

```Port 2222```

```PermitRootLogin no```

```AllowUsers richard```

### 2️⃣ Disable systemd SSH Socket Activation

```sudo systemctl stop ssh.socket```

```sudo systemctl disable ssh.socket```

This allows `sshd` to control its own listening port.

### 3️⃣ Restart SSH Daemon

```sudo systemctl restart ssh```

### 4️⃣ Verify Active Listening Port

```sudo ss -tulpn | grep ssh```

Expected output:

```LISTEN 0 4096 0.0.0.0:2222```

### ✅ Verification Tests
Local Test (Ubuntu)

```ssh -p 2222 richard@localhost```

Remote Test (Windows PowerShell)

```ssh -p 2222 richard@192.168.56.101```

Both tests succeeded.

---

## 🔐 Security Improvements Implemented
- SSH moved off default port (22 → 2222)
- Root login disabled
- SSH access restricted to a single user
- systemd socket activation understood and safely disabled
- Configuration validated before restart to avoid lockout

## 🧠 Key Learnings
- SSH configuration files are not shell commands
- sshd -T shows effective configuration, not runtime state
- systemd socket activation can override daemon configs
- Proper validation prevents production lockouts
- Always test locally before disconnecting remote sessions

## 🏷️ Skills Demonstrated
- Linux server administration
- SSH hardening best practices
- systemd troubleshooting
- Secure remote access
- Windows ↔ Linux interoperability
- Incident-style debugging

## 📂 Project Status
### ✅ Completed
### 🛡️ Production-safe hardening applied
