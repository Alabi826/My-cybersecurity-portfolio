## Phase 5 Summary: Log Collection and Monitoring (April 2025)

### Objective:
To simulate security-related events and inspect system logs on both Ubuntu and Windows virtual machines. The goal was to learn how logs are generated, accessed, and managed, and how to correlate events with specific log entries using built-in tools.

---

### Phase Breakdown:

#### Phase 5A – Log Simulation and Inspection (Ubuntu + Windows)
- **Ubuntu:**
  - Simulated failed `su` and `ssh` logins
  - Viewed logs in `/var/log/auth.log` and `/var/log/syslog`
  - Verified logs using:
    - `sudo grep`, `less`, `tail -f`
  - Observed accurate timestamp formats (ISO 8601)
- **Windows:**
  - Simulated failed remote login attempts from Ubuntu using:
    - `net use \\IP /user:username`
  - Used **Event Viewer** to identify Event ID **4625**
  - Confirmed both real and fake usernames triggered the appropriate logs

#### Phase 5B – Log Filtering, Rotation, and Maintenance (Ubuntu)
- Practiced filtering logs with specific keywords (e.g. `grep 'Failed'`)
- Explored how logs are archived and rotated using:
  - `logrotate` utility and `/etc/logrotate.conf`
- Learned about log file aging, size limits, and retention policies
- Observed the naming convention of rotated logs like `auth.log.1`, `syslog.1`, etc.

---

### Key Tools Used:
- **Ubuntu Terminal:** `su`, `ssh`, `less`, `tail`, `grep`, `logrotate`
- **Windows:** `net use`, Event Viewer (Security Logs)
- **GitHub:** For documenting all findings and challenges

---

### Key Challenges:
- Matching timestamps to simulated events
- Interpreting verbose log lines (especially on Windows)
- Understanding logrotate behavior and trigger conditions
- Navigating different log paths and tools per OS

---

### Lessons Learned:
- How to simulate and detect failed login events in logs
- How logrotate manages Ubuntu logs
- The meaning of Event ID 4625 in Windows and how to trace it
- Clear understanding of Linux and Windows logging structure

---

### Documentation Links:
- [Phase 5A - Log Simulation and Inspection](./projects/phase-5-log-monitoring/Phase-5A.md)
- [Phase 5B - Log Filtering and Rotation](./projects/phase-5-log-monitoring/Phase-5B.md)
