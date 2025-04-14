## Phase 3Aii – Firewall Rule Testing (Inter-Network ICMP Control)
**Date:** April 14, 2025  
**Objective:** To verify that pfSense firewall rules can allow or block communication between two virtual LANs — Ubuntu and Windows — by controlling ICMP (ping) traffic between them.

---

### Network Setup

#### pfSense (Router)
- **em0 (WAN):** 192.168.0.10/24 (not essential for this test)
- **em1 (LAN):** 192.168.1.1/24 → Ubuntu
- **em2 (OPT1 renamed WINDOWSNET):** 192.168.2.1/24 → Windows

#### Ubuntu VM
- **IP:** 192.168.1.100
- **Gateway:** 192.168.1.1
- **Key Commands:**
  ```bash
  ip a
  ip route
  ping 192.168.1.1
  ping 192.168.2.100
  ```

#### Windows VM
- **IP:** 192.168.2.100
- **Gateway:** 192.168.2.1
- **Key Commands:**
  ```cmd
  ipconfig
  ping 192.168.1.100
  ping 8.8.8.8
  ```

---

### Step-by-Step Summary

#### 1. Baseline Connectivity Test
- Created **allow-all rules** on LAN and WINDOWSNET interfaces:
  - **Source:** [Interface] subnet
  - **Destination:** Opposite subnet
  - **Protocol:** Any
  - **Action:** Pass

**Result:** Ping from Ubuntu to Windows and vice versa worked, confirming successful routing via pfSense.

---

### 2. Troubleshooting & Challenges

#### Issue 1: Ubuntu had IP but couldn’t ping its gateway
- **Fix:** Rebooted Ubuntu and pfSense, confirmed DHCP on LAN, ran:
  ```bash
  sudo dhclient
  ping 192.168.1.1
  ```

#### Issue 2: Ubuntu couldn’t ping Windows
- **Fix:** Enabled this Windows Firewall rule:
  - *File and Printer Sharing (Echo Request - ICMPv4-In)* for all profiles

#### Issue 3: Windows couldn’t ping Ubuntu (but Ubuntu could ping Windows)
- **Cause:** Block rule applied in pfSense (working as intended)

---

### 3. Firewall Rule Testing – Block ICMP from WINDOWSNET to LAN

#### Rule Created:
- **Interface:** WINDOWSNET
- **Protocol:** ICMP
- **Source:** WINDOWSNET subnet
- **Destination:** LAN subnet
- **Action:** Block
- **Description:** Block ICMP from Windows to Ubuntu

**Test Result:**
- Ubuntu → Windows: Success
- Windows → Ubuntu: Failed (blocked)

#### Confirmation:
- Disabled the block rule
- Windows could ping Ubuntu again → rule tested and verified

---

### Logs Observed (Firewall > System Logs):
- Confirmed blocked ICMP traffic before and during tests:
  ```
  Default deny rule IPv4: 192.168.1.100 → 192.168.1.1
  192.168.2.100 → 8.8.8.8 (before DNS allowed)
  ```

---

### Takeaways:
- Rule order is key in pfSense (top-down processing).
- ICMP traffic can be tightly controlled across interfaces.
- Windows Firewall must be manually configured to allow ping.
- Even with a valid IP and gateway, routing won't work without proper rules and adapter matching.
- Logs are extremely helpful for debugging and confirming rule hits.

---

### Outcome:
Firewall rules were successfully tested and enforced. Inter-network ping was allowed and blocked as needed, proving pfSense effectively manages cross-subnet traffic.

---

**Next Phase:** Phase 3B – Advanced Filtering (e.g., port-specific blocking like SMB, RDP, or SSH)
