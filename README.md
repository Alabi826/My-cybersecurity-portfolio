# My Cybersecurity Journey

## Introduction
Welcome to my cybersecurity portfolio! This repository documents my growth from an enthusiastic beginner to a cybersecurity professional. It showcases hands-on projects, labs, certifications, and personal milestones that highlight my learning path and technical progress in areas such as network security, threat detection, and system hardening.

---

## About Me

- **Name:** Boluwatife Akintunde Omo Alabi  
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

### Phase 1: Virtual Lab Setup
- Created two isolated networks within VirtualBox
- Configured internal network adapters for Ubuntu and Windows VMs
- Installed and configured pfSense as a router/firewall between the two networks
- Verified IP assignment, connectivity, and routing between devices

---

### Phase 2: Network Scanning & Reconnaissance
- Conducted scanning from Ubuntu attacker machine to discover hosts and open ports
- Commands used:
  ```bash
  nmap -sn 10.10.10.0/24
  nmap -sS -sV 10.10.10.10
  nmap -O 10.10.10.10
  ```
- Performed OS fingerprinting, service version detection, and host discovery
- Documented scans with flags used and result interpretation

---

### Phase 3: Firewall Rule Testing with pfSense

#### Phase 3Ai – ICMP Control
- Configured pfSense firewall rules to **block and allow ICMP (ping)** between Ubuntu and Windows machines
- Verified rule effectiveness using `ping` and traceroute
- Demonstrated behavior before and after rule application

#### Phase 3Aii – Inter-network Traffic Filtering
- Ensured proper segmentation between Ubuntu and Windows networks
- Created firewall rules in pfSense to explicitly allow or deny specific traffic types

#### Phase 3B – Port and Service Filtering
- Applied firewall rules to control access to specific TCP/UDP ports
- Verified using Nmap scans with the following:
  ```bash
  nmap -p 80,139,445 10.10.10.10
  nmap -sU -p 137 10.10.10.10
  ```

#### Phase 3Bii – Advanced Scanning & SMB Enumeration
- Used Nmap NSE scripts:
  ```bash
  nmap --script=smb-enum-shares.nse,smb-os-discovery.nse -p 445 10.10.10.10
  ```
- Verified whether pfSense could successfully block traffic to TCP port 445
- Used SMBClient to attempt access to shares on the Windows machine

---

### Phase 4A (In Progress): Vulnerability Scanning
- Setting up OpenVAS or alternative scanner
- Target: Windows 10 machine
- Goal: Identify vulnerabilities and misconfigurations, simulate detection and documentation
- Will document findings and create remediation steps

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
- Begin documentation of log analysis and detection (e.g., using Syslog or Splunk in future)
- Continue refining Linux and Python skills
- Prepare for Microsoft SC-200 exam

---

## Notes

> This portfolio is a living document. As I grow and learn, new projects, findings, and notes will be added here regularly.

