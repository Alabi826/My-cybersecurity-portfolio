# Phase 3Bii – Advanced Scanning & Port Rule Validation (SMB)

**Date:** April 15, 2025  
**Phase:** 3Bii  
**Focus:** Advanced Nmap SMB Scanning & Firewall Rule Testing

---

## **Objective**
To perform more advanced scans on the Windows VM using Nmap NSE scripts targeting SMB (port 445), validate the firewall’s ability to block this traffic, and simulate a basic vulnerability assessment setup.

---

## **Tools Used**
- Ubuntu VM (Attacker)
- Windows 10 VM (Target)
- pfSense (Firewall)
- Nmap with NSE scripts
- Telnet
- SSH
- PowerShell / Windows Services
- Firewall Rule Manager in pfSense

---

## **IP Addressing**
| Host       | IP Address     | Notes          |
|------------|----------------|----------------|
| Ubuntu     | 192.168.1.100  | Attacker VM    |
| Windows    | 192.168.2.100  | Target VM      |
| pfSense    | 192.168.1.1 / 192.168.2.1 | Gateway/Firewall |

---

## **Key Commands & Output**

### Check routing and interfaces:
```bash
ip route
ip a
```

---

### Full SMB Enumeration Scan:
```bash
nmap -Pn -p 445 --script smb-enum-shares,smb-enum-users,smb-os-discovery,smb-security-mode,smb2-security-mode 192.168.2.100
```

**Expected Output (when port is open):**
- Shows `445/tcp open microsoft-ds`
- Returns script results for SMB mode, users, shares

**Example:**
```text
Host script results:
| smb2-security-mode:
|   3:1:1:
|     Message signing enabled but not required
```

---

## **Firewall Rule Creation & Testing**

### Block Rule (on pfSense):
| Interface   | Source          | Destination          | Protocol | Port  | Action |
|-------------|------------------|-----------------------|----------|-------|--------|
| LAN         | LAN subnets      | WINDOWSNET subnets    | TCP      | 445   | Block  |

### Re-run scan after applying block:
```bash
nmap -Pn -p 445 192.168.2.100
```

**Expected Output:**
```text
PORT     STATE    SERVICE
445/tcp  filtered microsoft-ds
```

This confirms the firewall rule is blocking SMB traffic.

---

## **Troubleshooting & Fixes**

| Problem | Cause | Fix |
|--------|--------|-----|
| `telnet` not recognized on Windows | Telnet not installed by default | Installed it via "Turn Windows Features On or Off" |
| SMB port 445 not responding | Firewall still allowing or OS firewall blocking | Checked pfSense rule and disabled Windows firewall temporarily |
| `Unable to locate package OpenVAS` | OpenVAS renamed to `gvm` | Used `sudo apt install gvm` instead |
| Nmap shows host down | Ping probes blocked | Used `-Pn` to skip host discovery |

---

## **Conclusion**
We successfully:
- Conducted detailed SMB enumeration using Nmap NSE scripts
- Observed port 445 behavior before and after blocking
- Validated pfSense’s filtering capabilities


