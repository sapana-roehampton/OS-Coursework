# Week 5 – Advanced Security & Monitoring

## 🔗 Quick Navigation  
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


The screenshot confirms that:
- Automatic updates are **enabled**
- The service is **running**

---

# 4. Fail2Ban – Intrusion Detection & Mitigation

Fail2Ban was installed to protect the SSH service from brute-force login attempts.

### Commands Used:

- sudo apt install fail2ban -y
- sudo systemctl status fail2ban
- sudo fail2ban-client status sshd


###  **Evidence – Fail2Ban Running & SSH Jail Active**
![Fail2Ban Status](images/week5-fail2ban-status.png)

The screenshot confirms:
- Fail2Ban is **running**
- The **sshd jail** is enabled
- Suspicious login attempts will be blocked automatically

---

# 5. Security Baseline Script (`security-baseline.sh`)

This script automatically checks the most important security controls implemented in Week 4 and Week 5.

###  **What the Script Verifies**
- UFW firewall status  
- SSH root login disabled  
- SSH password authentication setting  
- AppArmor enforcement  
- Fail2Ban protection for SSH  
- Automatic security updates  

---

###  **Final Script Code **

```bash
#!/bin/bash
# security-baseline.sh
# Checks core server security settings.

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

### **Evidence : This output demonstrates that all configured security layers are active and functioning correctly.


---

# 6. Remote Monitoring Script (`monitor-server.sh`)

This script runs on the workstation, connects to the server via SSH, and extracts live performance metrics.

#!/bin/bash
# monitor-server.sh
# Collects performance metrics from the server via SSH.

SERVER="sapana@localhost"
PORT=2222

echo "===== REMOTE SERVER MONITORING ====="

echo "[1] CPU Usage:"
ssh -p $PORT $SERVER "top -bn1 | grep 'Cpu'"

echo "[2] Memory Usage:"
ssh -p $PORT $SERVER "free -h"

echo "[3] Disk Usage:"
ssh -p $PORT $SERVER "df -h"

echo "[4] Uptime:"
ssh -p $PORT $SERVER "uptime -p"

echo "[5] Top 5 Processes:"
ssh -p $PORT $SERVER "ps aux --sort=-%cpu | head -6"

### Evidence: Remote Monitoring Output

The output collected includes:

CPU usage

RAM usage

Disk storage usage

System uptime

Top processes consuming CPU

---

# 7. Reflection

Week 5 significantly improved my understanding of practical server security.
AppArmor strengthened access control, unattended-upgrades automated essential patching, and Fail2Ban protected SSH from repeated login attempts.

Writing the security baseline script taught me how to automate auditing tasks, and the monitoring script gave me real experience retrieving system performance data through SSH.

This week closely simulated real DevOps and system administration workflows, improving both my technical and professional skills.
