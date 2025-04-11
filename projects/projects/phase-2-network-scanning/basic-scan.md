# Basic Nmap Scan - Phase 2 Reconnaissance

## Date
April 11, 2025

## Objective
Perform a basic Nmap scan on the Windows 10 target machine to identify open TCP ports and gain an overview of exposed services.

---

## Command Used
```bash
nmap -Pn 192.168.1.101 -oN basic-scan.txt

-Pn: Skip host discovery (since Windows blocks ping)
-oN: Output in normal format to basic-scan.txt

Scan Result Summary

Nmap detected the following open TCP ports on the Windows 10 machine:

| Port | Protocol | Service        | Description                                                                 |
|------|----------|----------------|-----------------------------------------------------------------------------|
| 135  | TCP      | msrpc          | Microsoft Remote Procedure Call – used for system-level communication      |
| 139  | TCP      | netbios-ssn    | NetBIOS Session Service – used for legacy Windows file/printer sharing     |
| 445  | TCP      | microsoft-ds   | SMB (Server Message Block) – file sharing, network browsing, Active Directory |

Observations
	•	997 ports were filtered, meaning no response was received (likely due to the Windows firewall).
	•	The system is exposing classic Windows services (SMB/NetBIOS), which are common vectors for exploits like EternalBlue.
	•	These ports are important for future vulnerability scanning and security hardening.
