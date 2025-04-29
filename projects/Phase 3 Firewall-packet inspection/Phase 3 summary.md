## Phase 3 Summary: Firewall Rule Testing and Packet Inspection (April 2025)

### Objective:
To implement, test, and validate inter-network traffic control using pfSense firewall rules. This phase was split into multiple sub-phases to reflect troubleshooting, restructuring, and deep inspection of how pfSense manages packet filtering between the Ubuntu and Windows networks.

---

### Phase Breakdown:

#### Phase 3Ai – ICMP Traffic Filtering
- Configured firewall rules on pfSense to **allow/block ping (ICMP)** between Ubuntu and Windows.
- Verified connectivity using the `ping` command.
- Confirmed pfSense firewall enforced ICMP rules.

#### Phase 3Aii – Network Reconfiguration & Firewall Troubleshooting
- Discovered that using "Bridged Adapter" in VirtualBox bypassed firewall rules.
- Reconfigured all VM adapters to **Internal Networks** to enforce proper routing through pfSense.
- Separated Ubuntu and Windows into two internal networks: `lan` and `windowsnet`.
- pfSense connected to both networks using three interfaces: WAN, LAN, and OPT1.

#### Phase 3B – Port & Service Filtering
- Used Nmap to scan Windows from Ubuntu.
- Tested open ports (especially **port 445** for SMB).
- Created pfSense firewall rules to allow/block TCP traffic.
- Verified if blocked services (e.g., SMB) were unreachable.

#### Phase 3Bii – Packet Inspection with Wireshark
- Captured ICMP and TCP traffic on Ubuntu using Wireshark.
- Analyzed TCP handshake behavior (SYN, SYN-ACK, ACK, RST).
- Confirmed that when rules block traffic, TCP handshakes fail or reset prematurely.
- Tested rule placement priority and clearing firewall state tables.

---

### Key Tools Used:
- `ping`, `nmap`, Wireshark, pfSense Web UI
- VirtualBox internal network configuration

---

### Major Challenges:
- Unexpected "VirtualBox backdoor" due to Bridged Adapter
- Firewall rules not taking effect until network structure was fixed
- Ubuntu not receiving DHCP IP after network split
- DNS issues and default gateway ping failures

---

### What Was Learned:
- Importance of adapter types in virtualized environments
- Firewall rule placement and state management
- Deep understanding of packet flow and inspection using Wireshark
- Practical application of ICMP and TCP filtering

---

### Documentation Links:
- [Phase 3Ai - ICMP Filtering](./projects/phase-3-firewall-testing/Phase-3Ai.md)
- [Phase 3Aii - Network Reconfiguration](./projects/phase-3-firewall-testing/Phase-3Aii.md)
- [Phase 3B - Port Filtering & Nmap](./projects/phase-3-firewall-testing/Phase-3B.md)
- [Phase 3Bii - Packet Inspection](./projects/phase-3-firewall-testing/Phase-3Bii.md)
