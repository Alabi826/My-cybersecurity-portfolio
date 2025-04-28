# Phase 5B – Log Filtering and Log Rotation Setup
**Date Completed:** April 28, 2025

---

## Objective
To practice more advanced log management by:
- Creating a custom filtered log file for failed login attempts.
- Implementing log rotation to manage log size and storage efficiently.

This phase builds upon the basic log collection and inspection done in Phase 5A.

---

## Ubuntu (Attacker VM)

### Simulated Activities
- Attempted SSH login using a non-existent username (`fakeuser`) to generate failed authentication logs.

---

## Configurations Done

### 1. Log Filtering Setup
Edited `/etc/rsyslog.d/50-default.conf` to add a rule that filters log messages containing `"failed password"` and writes them to a new log file:
```plaintext
:msg, contains, "failed password" /var/log/auth_failures.log
& stop
```

### 2. Restarted rsyslog Service
```bash
sudo systemctl restart rsyslog
```

### 3. Tested Log Filtering
- Generated a failed SSH login attempt.
- Confirmed that the event was recorded inside `/var/log/auth_failures.log`.

---

### 4. Log Rotation Setup
Created a custom logrotate configuration file at `/etc/logrotate.d/auth_failures` with the following content:
```plaintext
/var/log/auth_failures.log {
    su root syslog
    weekly
    rotate 4
    missingok
    notifempty
    compress
    delaycompress
    create 640 syslog adm
}
```

### 5. Tested Log Rotation
Executed a manual log rotation check:
```bash
sudo logrotate -d /etc/logrotate.d/auth_failures
```
- Confirmed no critical errors.
- Ensured correct log handling based on the configuration.

---

## Key Lessons
- **Filtered Logging:** Selectively capturing important events (e.g., failed logins) simplifies monitoring and incident detection.
- **Log Rotation:** Automates log management, preventing excessive disk usage and maintaining organized records.

---
