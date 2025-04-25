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
- **Contact:** Boluwatifealabi@gmail.com

---

## Table of Contents

- [Certifications](#certifications)
- [Cybersecurity Projects](#cybersecurity-projects)
  - [Phase 1: Lab-Network Setup](#phase-1-virtual-cybersecurity-lab-setup)
  - [Phase 2: Network Scanning & Reconnaissance](#phase-2-network-scanning--reconnaissance)
  - [Phase 3: Firewall Rule Testing](#phase-3-firewall-rule-testing-with-pfsense-segmented-networks)
  - [Phase 5: Log Collection & Monitoring](#phase-5-log-collection--monitoring)
  - [Phase 4A: Vulnerability Scanning (Planned)](#phase-4a-in-progress-vulnerability-scanning)
- [Key Milestones](#key-milestones)

---

## Certifications

| Certification                    | Issued       | Status                                   |
|----------------------------------|--------------|------------------------------------------|
| CompTIA Network+                | October 2024 | Active                                   |
| CompTIA Security+               | March 2025   | Active                                   |
| ISC2 Certified in Cybersecurity | 2024         | Active (CPEs Completed - April 2025)     |

---

## Cybersecurity Projects

### Phase 1: Virtual Cybersecurity Lab Setup

Set up a basic internal network using VirtualBox with Ubuntu (attacker machine), Windows 10 (target machine), and pfSense (router/firewall) for learning and testing cybersecurity tools.

#### Lab Architecture
- **Ubuntu VM**: Attacker / Analyst — `192.168.1.100`
- **Windows 10 VM**: Victim — `192.168.1.101`
- **pfSense VM (Gateway)** — `192.168.1.1`

---

### Phase 2: Network Scanning & Reconnaissance

Conducted scanning from Ubuntu to discover hosts and open ports.

#### Commands Used:
```bash
nmap -sS -sV 192.168.1.101
nmap -sn 192.168.1.0/24
nmap -O 192.168.1.101
```

- Performed OS fingerprinting, service version detection, and host discovery.
- Documented scan logic and output in command journal.

---

### Phase 3: Firewall Rule Testing with pfSense (Segmented Networks)

Network was restructured to separate Ubuntu and Windows 10 into different networks for better traffic control and security testing.

#### Network Layout:
- **Ubuntu Segment**:
  - Ubuntu: `192.168.1.100`
  - pfSense LAN1: `192.168.1.1`
- **Windows Segment**:
  - Windows 10: `192.168.2.100`
  - pfSense LAN2: `192.168.2.1`

#### Testing Phases:

**Phase 3Ai – ICMP Control**
- Allowed/blocked ICMP between segments.
- Used `ping` and `traceroute` to verify rule enforcement.

**Phase 3Aii – Inter-network Traffic Filtering**
- Configured pfSense to allow/block communication between segments using tools like `ping`, `traceroute`, and `netcat`.

**Phase 3B – Port and Service Filtering**
- Configured pfSense to allow/block traffic on specific ports (e.g., 80, 139, 445).

**Phase 3Bii – Advanced Scanning & SMB Enumeration**
- Ran Nmap NSE scripts to enumerate SMB:
```bash
nmap --script=smb-enum-shares.nse,smb-os-discovery.nse -p 445 192.168.2.100
```

---

### Phase 5: Log Collection & Monitoring

Collected and analyzed logs from Ubuntu and Windows after simulating failed login and access attempts.

#### Ubuntu Logs:
```bash
sudo less /var/log/syslog
sudo tail -n 50 /var/log/auth.log
sudo grep 'Failed' /var/log/auth.log
```

- Simulated:
  - Failed `su` logins
  - Failed `ssh` logins

#### Windows Logs:
- Used **Event Viewer** to review security logs.
- Triggered failed SMB login using:
```bash
net use \\\\192.168.2.100\\IPC$ /user:invaliduser wrongpass
```
- Found **Event ID 4625** (failed logon).

#### Outcome:
- Verified that logs captured simulated attacks.
- Demonstrated basic log inspection and attack detection.

---

### Phase 4A (In Progress): Vulnerability Scanning

Planning to deploy OpenVAS (on Kali or Ubuntu) to scan the Windows 10 target.

#### Goals:
- Identify common vulnerabilities and misconfigurations.
- Document findings and simulate a basic incident response process.

---

## Key Milestones

| Date       | Milestone                                                   |
|------------|-------------------------------------------------------------|
| Oct 2024   | Earned CompTIA Network+                                     |
| Mar 2025   | Earned CompTIA Security+                                    |
| Apr 2025   | Completed 45 CPEs to maintain ISC2 CC                       |
| Apr 2025   | Completed Phase 3Bii (Advanced Firewall + Scanning)         |
| (Ongoing)  | Vulnerability Scanning & Detection with OpenVAS             |

