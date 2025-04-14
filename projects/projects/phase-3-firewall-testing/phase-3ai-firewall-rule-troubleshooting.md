# Phase 3Ai – Firewall Rule Testing: Uncovering Unexpected Direct VM Communication

## Objective
To configure and test firewall rules in pfSense to block ICMP (ping) traffic from the Ubuntu VM to the Windows VM, as part of securing inter-VM communication in a virtual lab environment.

## Network Setup
- **Ubuntu VM**: 192.168.1.100
- **Windows VM**: 192.168.1.101
- **pfSense LAN Interface**: 192.168.1.1
- All VMs were connected to the same **VirtualBox Internal Network (Labnet)**.

## Actions Taken

### 1. Configured Firewall Rule in pfSense
- Accessed pfSense via `https://192.168.1.1`.
- Created a LAN rule under `Firewall > Rules > LAN`:
  - **Action**: Block
  - **Protocol**: ICMP (later updated to Any)
  - **Source**: 192.168.1.100 (Ubuntu)
  - **Destination**: 192.168.1.101 (Windows)
  - Positioned the rule above the default allow LAN rule.
- Saved the rule and applied changes.

### 2. Observed Unexpected Behavior
- Despite correct rule configuration, Ubuntu could still ping Windows.
- Actions attempted to resolve the issue:
  - Reordered rules for priority.
  - Disabled and re-enabled the default allow rule.
  - Reset pfSense state table (UI froze).
  - Rebooted pfSense.
  - Re-tested connectivity multiple times with various protocol block settings.

### 3. Investigated Routing and Flow
- Ran `traceroute 192.168.1.101` on Ubuntu — showed no hops.
- Ran `ip route` — confirmed pfSense (192.168.1.1) as default gateway.
- Shut down pfSense to isolate behavior:
  - **Ping from Ubuntu to Windows still succeeded**, proving packets were not going through pfSense.

## Root Cause Identified
- All three VMs were on the same internal network (`Labnet`), allowing direct Layer 2 communication.
- Even with pfSense set as the gateway, **VMs on the same subnet and switch can bypass the router**.
- This meant that **pfSense had no control over traffic between Ubuntu and Windows**.

## Lessons Learned
- Firewall rules are only effective if traffic flows through the firewall/router.
- VMs on the same internal network can bypass routing logic by communicating directly at Layer 2.
- Proper **network segmentation** is essential to enforce security policies.

## Next Steps
- Redesign the lab topology:
  - Assign **Ubuntu and Windows to separate internal networks**.
  - pfSense will serve as the **router between those networks**, enforcing traffic flow.
  - Re-test firewall rules under new routing structure.

## Summary
Although the goal of blocking ICMP traffic was not immediately successful, this sub-phase exposed a critical aspect of virtual networking: **firewall placement and network path matter as much as the rules themselves**. This discovery will shape a better-architected lab environment in the next sub-phase.
