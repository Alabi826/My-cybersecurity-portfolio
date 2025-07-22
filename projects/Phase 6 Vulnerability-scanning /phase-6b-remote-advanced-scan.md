# Phase 6B: Remote Nessus Advanced Scan of Ubuntu from Kali

## Objective

To demonstrate:
- Remote access to Nessus web interface from Kali Linux
- Scanning of Ubuntu system (192.168.1.101) where Nessus is installed
- Use of a dedicated SSH user (`nessususer`) for secure remote access
- Execution of an **Advanced Scan** using Nessus

---

## Environment Details

- **Kali Linux IP:** 192.168.1.104
- **Ubuntu (Nessus Installed) IP:** 192.168.1.101
- **SSH User for Remote Login:** `nessususer`

---

## Step-by-Step Procedure

### 1. Created a New SSH User on Ubuntu

```bash
sudo adduser nessususer
sudo usermod -aG sudo nessususer
```

This user was used for remote SSH login and secure access.

---

### 2. Verified SSH is Running

```bash
sudo systemctl status ssh
```

If inactive, it was started with:

```bash
sudo systemctl start ssh
```

---

### 3. SSH Login from Kali to Ubuntu

On Kali:

```bash
ssh nessususer@192.168.1.101
```

Successful login confirmed remote connectivity between the two machines.

---

### 4. Remote Access to Nessus Web Interface

On Kali browser:

```
https://192.168.1.101:8834
```

- Proceeded past certificate warning
- Logged into Nessus with admin credentials

---

### 5. Configured and Ran an **Advanced Scan**

- **Scan Type:** Advanced Scan
- **Target IP:** 192.168.1.101 (Ubuntu)
- Clicked "Launch" to begin the scan

---

### 6. Observed and Verified Scan Completion

- Monitored progress through Nessus web UI
- Scan completed successfully
- Reviewed detailed vulnerability results and severity breakdown

---

## Screenshots

📸 *Screenshots will be added and linked here once uploaded to GitHub*

---

## Summary

This phase proved:
- Remote administration of Nessus from another system
- SSH-based secure user access
- Functionality of Nessus Advanced Scan in identifying vulnerabilities on the host
- Versatility of a SOC-like lab setup using VirtualBox VMs
