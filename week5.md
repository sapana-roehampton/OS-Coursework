# Week 5 – Advanced Security & Monitoring

## 🔗 Quick Navigation  
[Week 1](week1.md) | [Week 2](week2.md) | [Week 3](week3.md) | [Week 4](week4.md) | [Week 5](week5.md) | [Week 6](week6.md) | [Week 7](week7.md)
---

# 1. Introduction

Week 5 focused on implementing advanced Linux security mechanisms and creating automation tools for verifying and monitoring server security. The main tasks were:

- Enforcing mandatory access control (AppArmor)
- Configuring automatic security updates
- Implementing Fail2Ban intrusion detection
- Creating a **security baseline script** to verify system hardening
- Creating a **remote monitoring script** to collect server performance metrics from the workstation

These security practices strengthen system resilience and provide real-world experience in Linux administration and automation.

---

# 2. AppArmor – Access Control Enforcement

AppArmor was verified to ensure mandatory access control (MAC) is enforced on the server.  
This protects key applications by restricting their access beyond standard Linux permissions.

###  **Evidence – AppArmor Status**
![AppArmor Status](images/week5-apparmor-status.png)
<img width="1286" height="807" alt="week5-apparmor-status" src="https://github.com/user-attachments/assets/86f15c82-a730-4aa0-ac84-acc70a9a517d" />

The screenshot confirms:
- AppArmor is **active**
- Profiles are in **enforce** mode  
This means AppArmor is correctly protecting system processes.

---

# 3. Automatic Security Updates

The unattended-upgrades service was configured to automatically install security updates.  
This ensures the server remains protected without requiring manual administrator action.

### Commands Used:
```
 sudo apt install unattended-upgrades -y
 sudo dpkg-reconfigure --priority=low unattended-upgrades
 systemctl status unattended-upgrades

```

###  **Evidence – Automatic Updates Enabled**
![Unattended Upgrades](images/week5-unattended-upgrades.png)

Evidence 1: Installation
<img width="1278" height="799" alt="week5-auto-updated-install" src="https://github.com/user-attachments/assets/5b7ad54d-f392-4f17-b079-63d054c3d6de" />

Evidence 2: Update configuration dialog
<img width="1288" height="818" alt="week5-auto-updates-config" src="https://github.com/user-attachments/assets/3bb1cff5-7c4e-4a38-9628-a03281766d56" />

The screenshot confirms that:
- Automatic updates are **enabled**
- The service is **running**

---

# 4. Fail2Ban – Intrusion Detection & Mitigation

Fail2Ban was installed to protect the SSH service from brute-force login attempts.

### Commands Used:

```
 sudo apt install fail2ban -y
 sudo systemctl status fail2ban
 sudo fail2ban-client status sshd
```

###  **Evidence – Fail2Ban Running & SSH Jail Active**
![Fail2Ban Status](images/week5-fail2ban-status.png)

Evidence 1: Installation
<img width="1294" height="806" alt="week5-fail2ban-install" src="https://github.com/user-attachments/assets/8fde0a8c-a896-4b31-b6a4-6965c8c1e3b9" />

Evidence 2: Sevice started
<img width="1286" height="804" alt="week5-fail2ban-service" src="https://github.com/user-attachments/assets/61304cd0-9bc4-4787-a4b8-753fb104652c" />

Evidence 3: Sevice Status
<img width="1284" height="948" alt="week5-fail2ban-status" src="https://github.com/user-attachments/assets/7d42c58b-dc08-45ac-bce5-2ea7a9a42a4a" />

Evidence 4: SSH Jail Active
<img width="1298" height="961" alt="week5-fail2ban-sshjail" src="https://github.com/user-attachments/assets/4c40b628-71bf-4832-b072-80bc4fcc0b8e" />


The screenshot confirms:
- Fail2Ban is **running**
- The **sshd jail** is enabled
- Suspicious login attempts will be blocked automatically

---

# 5. Security Baseline Script (security-baseline.sh)

This script automatically checks the most important security controls implemented in Week 4 and Week 5.

###  **What the Script Verifies**
- UFW firewall status  
- SSH root login disabled  
- SSH password authentication setting  
- AppArmor enforcement  
- Fail2Ban protection for SSH  
- Automatic security updates  

---

###  **Final Script Code**

#!/bin/bash

#security-baseline.sh

#Checks core server security settings.


echo "===== SECURITY BASELINE CHECK ====="

echo "[1] Firewall Status:"

sudo ufw status

echo "[2] SSH Root Login:"
grep "^PermitRootLogin" /etc/ssh/sshd_config

echo "[3] SSH Password Authentication:"
grep "^PasswordAuthentication" /etc/ssh/sshd_config

echo "[4] AppArmor Status:"
sudo aa-status

echo "[5] Fail2Ban Status (sshd jail):"
sudo fail2ban-client status sshd 2>/dev/null

echo "[6] Automatic Security Updates:"
systemctl is-enabled unattended-upgrades

echo "===== CHECK COMPLETE ====="

### Evidence

<img width="1290" height="948" alt="week5-security-baseline (2)" src="https://github.com/user-attachments/assets/03bf2c92-cffe-4835-8771-3b2181e69f17" />

---

# 6. Remote Monitoring Script (`monitor-server.sh`)

This script runs on the workstation, connects to the server via SSH, and extracts live performance metrics.

#!/bin/bash
#!monitor-server.sh
#!Collects performance metrics from the server via SSH.

#!/bin/bash
# monitor-server.sh
# Collects server performance metrics via SSH.

SERVER="sapana@localhost"
PORT=2222

echo "===== REMOTE SERVER MONITORING ====="

# Show CPU usage
echo "[1] CPU Usage:"
ssh -p $PORT $SERVER "top -bn1 | grep 'Cpu'"

# Show memory usage
echo "[2] Memory Usage:"
ssh -p $PORT $SERVER "free -h"

# Show disk usage
echo "[3] Disk Usage:"
ssh -p $PORT $SERVER "df -h"

# Show system uptime
echo "[4] Uptime:"
ssh -p $PORT $SERVER "uptime -p"

# Show top 5 processes
echo "[5] Top 5 Processes:"
ssh -p $PORT $SERVER "ps aux --sort=-%cpu | head -6"

---

The output collected includes:

CPU usage

RAM usage

Disk storage usage

System uptime

Top processes consuming CPU

### Evidence  
<img width="2286" height="1103" alt="monitor-server-output" src="https://github.com/user-attachments/assets/aacbab0c-ba49-4581-aac6-52025129cf3f" />


---

# 7. Reflection

Week 5 significantly improved my understanding of practical server security.
AppArmor strengthened access control, unattended-upgrades automated essential patching, and Fail2Ban protected SSH from repeated login attempts.

Writing the security baseline script taught me how to automate auditing tasks, and the monitoring script gave me real experience retrieving system performance data through SSH.

This week closely simulated real DevOps and system administration workflows, improving both my technical and professional skills.
