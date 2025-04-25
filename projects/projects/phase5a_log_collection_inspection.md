# Phase 5A – Basic Log Collection and Inspection  
**Date Completed:** April 25, 2025

## Objective
To understand and practice how logs are collected, stored, and analyzed on both Linux and Windows systems. The goal was to simulate activities that generate logs, inspect them using native tools, and identify security-relevant entries.

---

## Ubuntu (Attacker VM)

### Simulated Activities
- Failed `su` attempts using non-existent and wrong usernames.
- Failed SSH attempt using `ssh fakeuser@localhost`.

### Log Files Inspected
- `/var/log/syslog`
- `/var/log/auth.log`

### Tools Used
- `less`
- `tail`
- `grep`

### Commands Used
```bash
# View entire syslog file
sudo less /var/log/syslog

# View the last few lines of syslog
sudo tail /var/log/syslog

# View authentication logs
sudo less /var/log/auth.log

# Search for login failures and rejections
sudo grep -Ei 'invalid|fail|denied|refused' /var/log/auth.log
```

### Sample Log Snippets
```text
FAILED SU (to vboxuser) vboxuser on pts/0
pam_unix(sudo:auth): authentication failure
ssh: Host key verification failed.
```

---

## Windows (Target VM)

### Simulated Activity
- Failed network logon using:
```cmd
net use \\localhost\C$ /user:fakeuser wrongpassword
```

### Log File Inspected
- Event Viewer → Windows Logs → Security

### Key Event ID Identified
- **4625**: An account failed to log on.

### Notable Fields
- Logon Type
- Account Name (e.g., `fakeuser`)
- Workstation Name
- Status/Error Code (e.g., 0xC000006A)

---

## Observations
- Linux logs showed all sudo and su attempts, including failed authentication and use of invalid usernames.
- Windows logs captured the exact time and cause of failed login attempts (invalid credentials), including the Event ID 4625.

---

## Lessons Learned
- `less` is great for scrolling and reading large log files.
- `tail` helps in real-time log updates.
- `grep` allows powerful keyword-based filtering.
- Event Viewer is essential on Windows for tracing authentication events.

---

## Next Phase
Move to **Phase 5B – Advanced Log Filtering and Monitoring**, including tools like `rsyslog`, `logrotate`, or even lightweight log shippers like `Filebeat`—depending on system performance and available RAM.
