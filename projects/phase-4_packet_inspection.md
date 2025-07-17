## Phase 4: Packet Inspection with Wireshark

This phase focuses on analyzing packet behavior across the network using Wireshark. The goal was to monitor how traffic flows between devices in the internal network and how the pfSense firewall influences or controls these flows.

### 🛠️ Setup

- All virtual machines (Ubuntu, Windows 10, Kali) were connected to the same internal network.
- pfSense acted as the central gateway, routing traffic both internally and externally.
- Wireshark was installed and used on Ubuntu to inspect traffic.
- Proper interfaces were selected in Wireshark (e.g., `enp0s3`) to listen to relevant packets.

---

### 📡 ICMP (Ping) Traffic

**Test**: Ubuntu (192.168.1.101) pinged Windows 10 (192.168.1.100)

**Wireshark Observation**:
- ICMP Echo Request from Ubuntu
- ICMP Echo Reply from Windows

✅ Firewall allowed the ping. Full ICMP exchange visible in Wireshark.

---

### 🔐 SMB (TCP Port 445)

**Test**: Ubuntu scanned Windows using Nmap for port 445

**Initial Observation**:
- Port 445 showed as **filtered**.
- Wireshark displayed two SYN packets from Ubuntu with **no response**.

**Fix**:
- Inbound firewall rule for port 445 added on Windows.

**Final Observation**:
- Wireshark captured a full TCP handshake (SYN → SYN-ACK → ACK).
- RST was sent later from the client, which is normal if no payload follows.

✅ SMB allowed after firewall update. Handshake confirmed.

---

### 🌐 HTTPS (TCP Port 443)

**Test**: Telnet and Nmap scan from Ubuntu to Windows on port 443

**Observation**:
- Wireshark captured repeated SYNs with no SYN-ACK replies.
- Nmap showed port 443 as **filtered** despite inbound rule update.

⚠️ HTTPS traffic remained blocked or unreachable from Ubuntu. Could be due to:
- No web server listening on port 443
- Windows firewall silently discarding

❌ Packet exchange incomplete. Test documented but not conclusive.

---

### 🖥️ RDP (TCP Port 3389)

**Test**: Ubuntu attempted to connect to Windows via Nmap and Telnet on port 3389

**Observation**:
- Initially showed **filtered**
- After adding inbound rule for port 3389, Nmap showed **open**
- Wireshark captured full TCP handshake

✅ Successful RDP connection test. Firewall rule allowed the traffic and packet behavior confirmed.

---

### 🧠 Lessons Learned

- Wireshark is incredibly useful for confirming what traffic gets through and what doesn’t.
- Some protocols like HTTPS may appear filtered even if firewall rules are present, depending on whether a service is actively listening.
- Full TCP handshakes (SYN → SYN-ACK → ACK) are reliable indicators of open ports and successful communication.
- RST flags after a handshake are normal if a session is closed quickly or manually.

---

### 🧰 Tools Used

- **Wireshark** for packet inspection
- **Nmap** for probing open ports
- **Telnet** for manual connection attempts
- **pfSense** for managing internal and external traffic flow

---

### 📁 Status

✅ Phase 4 fully completed  
📌 All tests successfully logged  
📓 Documentation captured for GitHub project continuity
