# Phase 3Biii - Packet Inspection & Firewall Rule Testing with Wireshark  
**Date:** April 20–22, 2025  
**Objective:**  
To inspect and analyze ICMP, TCP (port 445 & 22), and DNS (UDP port 53) traffic using Wireshark and pfSense firewall rules. Demonstrate how firewall rules impact packet behavior at the network level.

---

## 1. ICMP Packet Inspection (Ping)

**Test Command:**  
```bash
ping 192.168.2.100
```

**Expected Behavior:**  
- ICMP Echo Request and Echo Reply packets should be visible in Wireshark.

**Wireshark Observation:**  
- Packet flow:
  - Echo request: `192.168.1.100 -> 192.168.2.100`
  - Echo reply: `192.168.2.100 -> 192.168.1.100`
- Protocol: ICMP  
- Info: `Echo (ping) request` and `Echo (ping) reply`  
- Inspection confirmed that ICMP traffic was successfully flowing between hosts.

---

## 2. TCP Port 445 Inspection (SMB)

**Test Command (Nmap):**  
```bash
nmap -Pn -p 445 192.168.2.100
```

**Wireshark Observation (Before Firewall Block):**  
- TCP 3-way handshake observed:
  - SYN → SYN/ACK → ACK
- TCP connection successful.
- Service: `microsoft-ds`  
- Port: `445`

**Firewall Rule Applied (on LAN tab):**  
- Block all TCP traffic to port 445  
  - Source: `LAN subnet`
  - Destination: `WINDOWSNET subnet`
  - Port: `445`

**Wireshark Observation (After Block):**  
- TCP handshake completed, but immediately followed by `RST, ACK` from client.  
- Indicates:  
  - Connection was reset by the client after establishing.  
  - **Nmap showed the port as "filtered"**, suggesting interference by firewall or IDS.

---

## 3. TCP Port 22 Inspection (SSH)

**Test Command (from Windows to Ubuntu):**  
```bash
telnet 192.168.1.100 22
```

**Wireshark Observation:**  
- TCP SYN → SYN/ACK → ACK observed (3-way handshake).
- Server replied with SSH banner, confirming open SSH port.
- TCP FIN exchange followed, indicating graceful termination.
- **No firewall rule was applied here** (open by default).

---

## 4. DNS Inspection (UDP Port 53)

**Test Tool on Ubuntu:**  
```bash
dig @8.8.8.8 google.com
```

**Wireshark Observation (Before Block):**  
- DNS Query (UDP) from `192.168.1.100` to `8.8.8.8`
- DNS Response received with IP of `google.com`
- Protocol: DNS  
- Port: 53 UDP

**Firewall Rule Applied (on LAN tab):**  
- Block UDP traffic on port 53  
  - Source: `LAN subnet`
  - Destination: `8.8.8.8`

**Observation After Block:**  
- `dig` output showed:
  ```text
  ;; communications error to 8.8.8.8#53: timed out
  ;; no servers could be reached
  ```
- Wireshark:  
  - Only outbound DNS queries from Ubuntu observed.  
  - **No response packets returned**, confirming successful firewall block.  
  - Unlike TCP, there was **no RST or rejection notice**, since UDP is connectionless.

---

## Conclusion:
This exercise validated how different protocols behave at the packet level and how pfSense firewall rules control traffic flow.  
- ICMP and TCP behaviors are easy to trace with SYN, ACK, RST.
- UDP (like DNS) gives no reply when blocked—making Wireshark crucial for visibility.

**Next Phase:**  
Move on to **Phase 4A - Vulnerability Scanning with OpenVAS**
