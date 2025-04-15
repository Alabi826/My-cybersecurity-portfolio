## Phase 3B – Port & Service Filtering Between Subnets  
**Date:** April 15, 2025  

---

### **Objective:**
To test **pfSense’s ability** to allow or block specific ports and services (e.g. SSH, SMB) between **Ubuntu and Windows VMs** across two internal subnets.

---

### **Network Setup:**

| Device    | IP Address       | Role                |
|-----------|------------------|---------------------|
| pfSense   | 192.168.1.1 / 2.1| Firewall/Router     |
| Ubuntu VM | 192.168.1.100    | Attacker (LAN)      |
| Windows VM| 192.168.2.100    | Target (WINDOWSNET) |

---

### **Initial Configuration and Issues:**

#### **Problem:**  
Ubuntu VM couldn’t resolve domain names or access the internet (e.g. `ping google.com` failed), while Firefox browser worked.

#### **Diagnosis:**
- `ping` showed “temporary failure in name resolution.”
- `resolvectl status` showed correct DNS servers (8.8.8.8, 1.1.1.1).
- `dig google.com` worked (name resolution succeeded).
- `ping 8.8.8.8` and `ping google.com` failed.
- Root cause: pfSense rules were blocking certain outbound traffic.

#### **Fix:**
- Temporarily edited pfSense firewall rules to **allow all protocols**.
- Verified DNS and ping started working.
- Restored control afterward to begin proper firewall testing.

---

### **Service Setup:**

#### **Enable SSH on Ubuntu:**
```bash
sudo apt install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
```

#### **Confirm SMB is Running on Windows:**
```powershell
Get-Service -Name *smb*
```
- Also verified `Server` service is **Running** in `services.msc`.

---

### **Connectivity Test (Before Rules Applied):**

#### **Ubuntu to Windows (Check Port 445):**
```bash
nmap -Pn -p 445 192.168.2.100
```
**Result:** Port 445 was open.

#### **Windows to Ubuntu (Check Port 22):**
```cmd
telnet 192.168.1.100 22
```
**Result:** SSH banner displayed — port 22 open.

---

### **Firewall Rule Configuration in pfSense:**

#### **Block SSH from Windows to Ubuntu:**
- **Interface:** WINDOWSNET
- **Protocol:** TCP
- **Destination Port:** 22
- **Action:** Block
- **Destination:** LAN subnet

#### **Block SMB from Ubuntu to Windows:**
- **Interface:** LAN
- **Protocol:** TCP
- **Destination Port:** 445
- **Action:** Block
- **Destination:** WINDOWSNET subnet

> Note: On pfSense, under "Destination Port Range", leave "To" empty if it's a single port.

---

### **Connectivity Test (After Rules Applied):**

#### **Windows to Ubuntu (Port 22):**
```cmd
telnet 192.168.1.100 22
```
**Result:** `Connect failed` — port 22 is blocked.

#### **Ubuntu to Windows (Port 445):**
```bash
nmap -Pn -p 445 192.168.2.100
```
**Result:** Port 445 showed `filtered` — successfully blocked by firewall.

---

### **Commands Summary:**

#### **Ubuntu VM:**
```bash
# Network debugging
dig google.com
resolvectl status

# Install and start SSH
sudo apt install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh

# Port scan
nmap -Pn -p 445 192.168.2.100
```

#### **Windows VM:**
```cmd
ipconfig
telnet 192.168.1.100 22
```

```powershell
Get-Service -Name *smb*
```

---

### **Problems Faced & Fixes:**

| Issue | Cause | Fix |
|------|-------|-----|
| DNS not resolving | Firewall blocked protocols | Allowed protocols in pfSense |
| `telnet` not found | Not installed on Windows | Enabled Telnet via "Turn Windows features on/off" |
| `nmap` shows "host seems down" | Ping blocked by target | Used `-Pn` to skip ping |
| Port 445 filtered | SMB service not fully allowed or firewall filtering | Verified SMB running, adjusted firewall |

---

### **Conclusion:**
- **pfSense effectively blocked ports** as configured.
- All expected services (SSH, SMB) were initially reachable.
- After firewall rules were added, connections were successfully **blocked**.
- Troubleshooting helped reinforce knowledge of **firewall behavior, services, and network testing tools**.
