# My Cybersecurity Journey

## Introduction
Welcome to my cybersecurity portfolio! This repository documents my growth from an enthusiastic beginner to a cybersecurity professional. It showcases hands-on projects, labs, certifications, and personal milestones that highlight my learning path and technical progress in areas such as network security, threat detection, and system hardening.

---

## About Me

- **Name:** Boluwatife Akintunde Alabi  
- **Current Role:** Healthcare Assistant (Domiciliary Care) | Aspiring Cybersecurity Professional  
- **Certifications:** CompTIA Network+, CompTIA Security+, ISC2 Certified in Cybersecurity (CC)  
- **Learning Focus:**  
  - Splunk (paused)  
  - Python (brushing up)  
  - Linux (Ubuntu)  
  - Microsoft Azure (SC-200 in sight)  
  - Cybersecurity lab projects (hands-on implementation)  
- **Contact:** [Boluwatifealabi@gmail.com](mailto:Boluwatifealabi@gmail.com)

---

## Certifications

| Certification | Issued | Status |
|---------------|--------|--------|
| CompTIA Security+ | March 2025 | Active |
| CompTIA Network+ | October 2024 | Active |
| ISC2 Certified in Cybersecurity (CC) | 2024 | Active (CPEs Completed - April 2025) |

---

## Cybersecurity Projects

### Home Lab Setup & Network Security Simulation

A virtualized lab environment built using VirtualBox to simulate and secure a home network. This lab was designed to gradually implement security principles learned from Network+ and Security+ certifications.

**Tools & Technologies Used:**
- **VirtualBox**: Virtualization platform
- **Ubuntu (Attacker Machine)**: Primary system for reconnaissance, scanning, and attack simulation
- **Windows 10 (Target Machine)**: Simulated vulnerable host
- **pfSense Firewall**: Used to route, filter, and block traffic
- **Nmap**: Network scanning and enumeration
- **SMBClient**: SMB enumeration and access
- **NSE Scripts**: Used with Nmap for deeper service probing

---

### Phase 1: Virtual Lab Setup (Single LAN)
- All systems were initially configured on the same internal network.
- **Ubuntu:** `192.168.1.100`  
- **Windows 10:** `192.168.1.101`  
- **pfSense LAN (Gateway):** `192.168.1.1`
- Successfully verified communication across all machines.

---

### Phase 2: Network Scanning & Reconnaissance
- Conducted scanning from Ubuntu to discover hosts and open ports.
- Used the following commands:
  ```bash
  nmap -sn 192.168.1.0/24
  nmap -sS -sV 192.168.1.101
  nmap -O 192.168.1.101
  ```
- Performed OS fingerprinting, service version detection, and host discovery.
- Documented scan logic and output in command journal.

---

### Phase 3: Firewall Rule Testing with pfSense (Segmented Networks)

Network was restructured to separate Ubuntu and Windows 10 into different networks for better traffic control and security testing:

- **Ubuntu Segment:**
  - Ubuntu: `192.168.1.100`
  - pfSense LAN1 (Ubuntu side): `192.168.1.1`
- **Windows Segment:**
  - Windows 10: `192.168.2.100`
  - pfSense LAN2 (Windows side): `192.168.2.1`

#### Phase 3Ai – ICMP Control
- Created pfSense rules to **allow/block ICMP traffic** between Ubuntu and Windows networks.
- Used `ping` and `traceroute` to confirm traffic behavior before and after rule changes.

#### Phase 3Aii – Inter-network Traffic Filtering
- Implemented custom pfSense rules to explicitly permit or deny communication between network segments.
- Verified behavior using tools like `ping` and `netcat`.

#### Phase 3B – Port and Service Filtering
- Configured pfSense to control traffic based on specific TCP/UDP ports.
- Used Nmap to test access from Ubuntu to Windows:
  ```bash
  nmap -p 80,139,445 192.168.2.100
  nmap -sU -p 137 192.168.2.100
  ```

#### Phase 3Bii – Advanced Scanning & SMB Enumeration
- Ran Nmap with NSE scripts for SMB service probing:
  ```bash
  nmap --script=smb-enum-shares.nse,smb-os-discovery.nse -p 445 192.168.2.100
  ```
- Used **SMBClient** to test SMB share accessibility based on firewall status.
- Verified successful blocking of TCP port 445 using pfSense.

---

### Phase 4A (In Progress): Vulnerability Scanning
- Planning to deploy OpenVAS (on Kali or Ubuntu) to scan the Windows 10 target.
- Goals:
  - Identify common vulnerabilities and misconfigurations.
  - Document findings and simulate a basic incident response process.

---

## Key Milestones

| Date | Milestone |
|------|-----------|
| Oct 2024 | Earned CompTIA Network+ |
| Mar 2025 | Earned CompTIA Security+ |
| Apr 2025 | Completed 45 CPEs to maintain ISC2 CC |
| Apr 2025 | Completed Phase 3Bii (Advanced Firewall + Scanning) |
| (Ongoing) | Vulnerability Scanning & Detection with OpenVAS |

---

## Next Steps

- Complete Phase 4A (vulnerability scanning)
- Document and simulate basic SIEM/log correlation (e.g., with Syslog or Splunk)
- Continue sharpening Python and Linux command-line skills
- Begin preparation for Microsoft SC-200 exam

---

## Notes

> This portfolio is a living document. As I grow and learn, new projects, findings, and notes will be added here regularly.

