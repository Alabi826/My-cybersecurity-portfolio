## Phase 2: Network Scanning & Enumeration

## Date: 11 April 2025

### Target IP: `192.168.1.101`

---

### 1. **Initial Version Scan (Nmap)**

```
nmap -sV 192.168.1.101 -oN version-scan.txt
```

**Result:**
The scan returned `0 hosts up` because the target blocked ICMP (ping) probes. Nmap assumed the host was down.

---

### 2. **Scan with Ping Disabled**

```
nmap -sV -Pn 192.168.1.101 -oN version-scan.txt
```

**Result:**
Nmap detected that the host was up and identified the following open ports:
- `135/tcp` – Microsoft RPC
- `139/tcp` – NetBIOS Session Service
- `445/tcp` – Microsoft-DS (SMB)

---

### 3. **Aggressive Nmap Scan**

```
nmap -A -Pn 192.168.1.101 -oN aggressive-scan.txt
```

**Details Discovered:**
- OS: Windows
- Hostname: `DESKTOP-M0BFE9L`
- MAC: `08:00:27:E6:19:40` (Oracle VirtualBox)
- SMB2 Security Mode: `Message signing enabled but not required`
- Domain/Workgroup Name: `WORKGROUP`

---

### 4. **Installing and Using Enum4linux**

#### Installation Attempt (APT)
```
sudo apt install enum4linux
```
**Result:** Not found in APT repositories.

#### Installed via Snap
```
sudo snap install enum4linux
```
**Result:** Successfully installed version `0.9.1` from snap.

---

### 5. **SMB Enumeration with Enum4linux**

```
enum4linux -a 192.168.1.101
```

**Results:**
- Known Usernames Enumerated:
  - `administrator`, `guest`, `krbtgt`, `domain admins`, `root`, `bin`, `none`
- Workgroup/Domain Name: `WORKGROUP`
- NetBIOS Information:
  - System: `DESKTOP-M0BFE9L`
  - Services: File Server, Workstation, Master Browser, etc.
- MAC Address: `08:00:27:E6:19:40`
- **Note:** The scan stopped early because the server does **not allow null sessions** (no username or password). This means anonymous access to shared resources is restricted.

---

### Key Takeaways:
- The Windows machine is discoverable and has SMB services exposed.
- It allows SMB connections without requiring message signing, which could be exploitable.
- Null session enumeration is disabled — a good security control.
- Enum4linux had to be manually installed via Snap, which was successfully done.
- Next steps could involve authenticated SMB enumeration or trying other protocols if credentials become available.

---

**Files saved:**
- `version-scan.txt`
- `aggressive-scan.txt`
- Enum4linux output saved as screenshot or terminal logs
