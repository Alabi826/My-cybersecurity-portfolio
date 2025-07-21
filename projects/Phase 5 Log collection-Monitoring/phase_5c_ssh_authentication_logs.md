# Phase 5C: SSH Authentication Log Analysis  
📅 **Date:** July 21, 2025

## 🔍 Overview  
In this phase, we analyzed both **successful** and **failed** SSH authentication attempts from Kali to Ubuntu. The objective was to understand how these events are logged and how they can be monitored for security purposes using `/var/log/auth.log`.

---

## 🎯 Objectives  
- Establish SSH login from Kali to Ubuntu (successful).
- Attempt login with incorrect credentials (failed).
- Analyze logs for accepted and failed SSH connections.
- Use `grep` to filter SSH authentication events.

---

## 🖥️ Lab Setup  
- **Attacker:** Kali Linux VM  
- **Target:** Ubuntu 22.04 LTS VM  
- **Log File:** `/var/log/auth.log`  
- **SSH Port:** 22  
- **Method:** Username & Password Authentication  

---

## ✅ Successful SSH Login  

### 🔧 Command (from Kali):  
```bash
ssh bolu@192.168.1.101
```

### 🔑 Outcome:  
Correct password used. SSH session was established.

### 📄 Log Output:  
```log
Accepted password for bolu from 192.168.1.104 port 54312 ssh2
```

This indicates that user `bolu` was authenticated successfully from the Kali machine.

---

## ❌ Failed SSH Login Attempts  

### 🔧 Action:  
Entered the wrong password intentionally multiple times.

```bash
ssh bolu@192.168.1.101
# then entered wrong passwords
```

### 📄 Log Output:  
```log
Failed password for bolu from 192.168.1.104 port 41022 ssh2
message repeated 2 times: [ Failed password for bolu from 192.168.1.104 port 41022 ssh2 ]
```

---

## 🧰 Log Filtering with grep  

### 🔧 Command Used:  
```bash
sudo grep "Failed password" /var/log/auth.log
```

### 🔎 Result:
Filtered view showing only failed SSH login attempts, useful for detecting brute-force behavior or unauthorized access.

---

## 📊 Summary Table

| Event Type         | Source IP      | Username | Port   | Result         |
|--------------------|----------------|----------|--------|----------------|
| Successful login   | 192.168.1.104  | bolu     | 54312  | Access granted |
| Failed login (×2)  | 192.168.1.104  | bolu     | 41022  | Access denied  |

---

## ✅ Conclusion  
This log inspection confirms how `/var/log/auth.log` can be used to monitor SSH activities. These logs are essential for intrusion detection, incident response, and overall server hardening.

We demonstrated:
- Detection of failed login attempts
- Confirmation of successful logins
- Use of `grep` for focused log parsing

This completes **Phase 5C** of the cybersecurity project.
