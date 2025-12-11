# Week 7 – Security Audit and System Evaluation  

## Task 1: Lynis Security Audit (Before & After Remediation)

### 1. Introduction

This week focused on performing a full security audit of my Ubuntu server using **Lynis**, a widely used security auditing tool for Linux-based systems. Lynis performs deep inspection of system configurations, authentication policies, firewall settings, update mechanisms, and overall system hardening.  
The objective for this task was to carry out a baseline audit, examine weaknesses, apply targeted remediations, and finally run a second audit to measure improvement.

---

## 2. Installing Lynis

Lynis was installed from the Ubuntu repositories using:

```bash
sudo apt install lynis -y
```

**Evidence**

<img width="1285" height="791" alt="week7-lynis-install" src="https://github.com/user-attachments/assets/7a9cef40-9d01-477a-ade0-2e3402faa1e9" />

Once installation completed, the tool was ready for a full system audit.

---

## 3. Baseline Lynis Scan (Before Remediation)

The initial security assessment was run using:

```bash
sudo lynis audit system
```

**Evidence**

<img width="1270" height="793" alt="week7-lynis-before" src="https://github.com/user-attachments/assets/688799d3-f2e0-4406-94ae-720c51137385" />

---

###  Baseline Hardening Summary

- **Hardening Index:** 60  
- **Total Tests Executed:** 256  
- **Plugins Activated:** 1  

### 🔹Key Baseline Observations

| Category | Status | Notes |
|---------|--------|-------|
| Firewall | ✔ Enabled | UFW already active |
| Malware Scanner | ❌ Not installed | No rkhunter/chkrootkit present |
| Compliance Checks | Incomplete | Additional modules required |
| SSH Configuration | Weak | Hardening improvements required |

### Initial Recommendations

Lynis highlighted important changes needed for better security:
- Install a malware scanning tool  
- Strengthen SSH security settings  
- Enable unattended security updates  
- Improve logging and auditing features  

---

## 4. Security Improvements Applied

### 4.1 Firewall Validation
The firewall was verified and reinforced using:

```bash
sudo ufw enable
sudo ufw allow OpenSSH
```

**Evidence**

<img width="1278" height="797" alt="week-7-ufw" src="https://github.com/user-attachments/assets/3411c1c6-4761-4b39-bc3f-b2407cca40b7" />

---

### 4.2 Installing a Malware Scanner

To address the missing malware protection warning:

```bash
sudo apt install rkhunter -y
sudo rkhunter --update
```

**Evidence**

<img width="1271" height="798" alt="week7-rkhunter-install" src="https://github.com/user-attachments/assets/55de92c7-ecae-4efe-b706-2bd147048f44" />

This ensured the system had a basic integrity- and malware-detection mechanism.

---

### 4.3 Enabling Unattended Security Updates

To ensure security patches are applied automatically, unattended upgrades were configured:

```bash
sudo apt install unattended-upgrades -y
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

Choosing **Yes** at the prompt enabled automatic update functionality.

**Evidence**

<img width="1279" height="795" alt="week-7-auto-updates" src="https://github.com/user-attachments/assets/4d83bf65-64bb-42a3-b8c1-2f3c31bef760" />

---

### 4.4 SSH Hardening Adjustments
Lynis recommended strengthening SSH configurations.  
The SSH configuration file `/etc/ssh/sshd_config` was reviewed and updated to include more secure defaults.

```bash
sudo nano /etc/ssh/sshd_config
```

The following settings were confirmed or applied:

- `PermitRootLogin no`  
- `PasswordAuthentication yes`  
- `X11Forwarding no`

SSH was restarted:

```bash
sudo systemctl restart ssh
```

**Evidence**

<img width="1282" height="804" alt="week-7-ssh-hardening" src="https://github.com/user-attachments/assets/6f5503b7-0730-4895-981a-b942fdecc50b" />

---

## 5. Lynis Second Audit (After Remediation)

A follow-up audit was executed after making all recommended changes:

```bash
sudo lynis audit system
```

**Evidence**

<img width="1272" height="796" alt="week7-lynis-after" src="https://github.com/user-attachments/assets/9b68fd6f-7bec-4b32-a4fc-0a04568f05f5" />

---

### Updated Hardening Summary

- **Hardening Index:** 62  
- **Total Tests Executed:** 261  
- **Plugins Activated:** 1  

### Observed Improvements

| Improvement | Result |
|-------------|--------|
| Malware scanner installed | ✔ Warning resolved |
| SSH configuration strengthened | ✔ Fewer insecure defaults detected |
| Automatic security updates enabled | ✔ Update-related warnings removed |
| Increased test coverage | ✔ More checks performed than before |

While the overall score increased from **60 → 62**, this still represents meaningful security improvements since Lynis becomes stricter as hardening progresses.

---

## 6. Remaining Risks and Future Enhancements

Lynis identified further security opportunities:

- Configure stronger compliance and policy modules  
- Improve system auditing and log retention  
- Consider installing an integrity monitoring tool like **AIDE**  
- Review and disable unnecessary services to reduce the attack surface  

These items can be addressed in subsequent phases of the project.

---

## 7. Conclusion

This task successfully fulfilled the Week 7 auditing requirements.  
The steps completed include:

1. Running an initial Lynis security audit  
2. Applying recommended remediations (firewall, SSH, malware scanner, auto-updates)  
3. Executing a second audit to measure improvements  
4. Comparing before-and-after hardening results  

The system now demonstrates a stronger security posture and aligns more closely with best-practice hardening guidelines. This concludes the Lynis audit and evaluation stage for Week 7.

---
