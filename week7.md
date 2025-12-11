# Week 7 – Security Audit and System Evaluation  

## Task 1: Lynis Security Audit (Before & After Remediation)

## 1. Introduction

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

### Key Baseline Observations

**Firewall:** Enabled and functioning correctly 

**Malware Scanner:** No malware scanner was installed as the system did not have rkhunter.

**Compliance Checks:** Some compliance modules were incomplete or missing.

**SSH Configuration:** Weak. Several SSH settings required hardening.


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


### Observed Improvements After Remediation 

A malware scanner was successfully installed, resolving the previous warning.
SSH security was strengthened, reducing insecure defaults and improving authentication settings.
Automatic security updates were enabled, removing update-related warnings.
The overall system audit improved with more security tests executed than before.

Even though the hardening score increased only slightly from 60 to 62, this still reflects meaningful improvement because Lynis becomes stricter as more configurations are hardened.

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
---

## Task 2: Network Security Assessment

## Installing Nmap (Host Machine)

Before performing the network scan, Nmap was installed on the Windows host machine using the following command in PowerShell:
```
winget install nmap
```

This successfully installed Nmap (version 7.80), enabling full port-scanning and service enumeration for the Ubuntu VM.

**Evidence**

<img width="2349" height="1184" alt="week7-nmap-install" src="https://github.com/user-attachments/assets/e8e9b287-eec6-493c-acf6-464b7379efc2" />

---

## Nmap Scan

A full TCP port and service scan was performed from the Windows host using Nmap to identify exposed services on both the host system and the Ubuntu virtual machine.

Command used:
```
nmap -sV -p- localhost -Pn
```

**Purpose of the scan:**

- Detect open ports
- Identify running services and versions
- Assess potential network exposure
- Confirm VM port forwarding for SSH

## Nmap Scan Results

The scan showed that 65,512 ports were closed, while several key ports were open:
1. The host responded immediately, showing it was online and reachable.
2. Port 2222/tcp displayed OpenSSH 9.6p1 (Ubuntu) — this is the VM’s SSH service.
3. Windows internal services such as RPC (135/tcp) and SMB (445/tcp) were visible.
4. Several high-range dynamic ports (49664–49680, 51779–51782) were open, which is normal for Windows networking.
5. A few encrypted or unknown services were detected but are typical for background host processes.
6. One port showed as filtered, confirming firewall activity.

**Evidence:**

<img width="2879" height="1687" alt="week7-nmap-scan" src="https://github.com/user-attachments/assets/15c84974-5ca5-41fe-b4b7-73021545b6ec" />

---

## Interpretation

- SSH on port 2222 is correctly exposed from the Ubuntu VM.
- Windows system services (RPC, SMB, dynamic ports) are normal and expected.
- Encrypted/unknown ports appear to be standard background Windows services.
- Filtered ports confirm that the firewall is actively blocking some traffic.
- No unexpected or unnecessary Linux services were exposed.

## Security Assessment Conclusion

The Nmap scan confirms that the system’s network exposure is limited and controlled.
Only essential Windows system services and the VM’s SSH port are visible, with the vast majority of ports securely closed. This reflects a low attack surface and aligns with best-practice security hardening for a virtualized lab environment.

---
---

## Task 3: SSH Security Verification

This task focused on reviewing and validating the security configuration of the SSH service, ensuring it aligns with best-practice guidelines for secure remote access. Verification involved examining the SSH configuration file, checking service status, and confirming the installed SSH version.

---

### 1. Reviewing SSH Configuration (`sshd_config`)

The SSH configuration file was inspected to ensure that unsafe options were disabled and essential security controls were active.

```
sudo nano /etc/ssh/sshd_config
```

Key verified settings:

- **PermitRootLogin no** — root login is disabled, preventing high-risk authentication.
- **PasswordAuthentication yes** — allowed for this local testing environment.
- **X11Forwarding no** — disables forwarding to reduce attack surface.
- **UsePAM yes** — ensures proper authentication management.
- **Subsystem sftp /usr/lib/openssh/sftp-server** — confirms SFTP support.
- Several optional insecure features remain **commented out**, keeping them disabled.

These settings collectively help limit exposure to unnecessary attack vectors.

---

### 2. Checking SSH Service Status

The SSH daemon status was inspected using:

```bash
sudo systemctl status ssh
```

**Verification confirmed:**

- Service is active and running
- Correctly loaded from `/usr/lib/systemd/system/ssh.service`
- SSH listens on the expected port (22)
- No warnings or failures were reported

A running and stable SSH service ensures reliable remote connectivity.

---

### 3. Confirming Installed SSH Version

The installed SSH version was retrieved using:

```bash
ssh -V
```

**Result:**

- OpenSSH_9.6p1 Ubuntu-3ubuntu13.14
- OpenSSL 3.0.13 (2024)

This reflects a modern, up-to-date installation, reducing vulnerabilities associated with outdated cryptographic libraries.

---

**Evidence**

The combined screenshot displays:
- The inspected `sshd_config` file  
- SSH service status  
- SSH version output  

<img width="1284" height="805" alt="week7-ssh-verification" src="https://github.com/user-attachments/assets/dd8a9d7f-ed4a-4e22-a876-4282b1af1721" />

---

### 5. Summary

The SSH security verification confirms that:

- Root login is disabled  
- Essential hardening options are correctly configured  
- SSH service is healthy and running  
- A recent, secure version of OpenSSH is installed  

This validates that SSH aligns with recommended security practices and supports a secure remote administration environment.

---
---

## Task 4 — Access Control Verification

This task checked whether user accounts, group memberships, and sudo privileges on the server follow secure access-control practices.

### 1. User Identity

Command:
```bash
id
```

The user sapana (UID 1000) is the main system account and belongs to expected groups such as sudo, adm, cdrom, dip, plugdev, and lxd.

### 2. Sudo Group Verification

Command:
```bash
grep -E "sudo|adm" /etc/group
```
**Result:**
- Both *sapana* and *student* appear in the `sudo` group  
- If student is not meant to be an admin, this is a security risk

### 3. Home Directory Permissions

Command:
```bash
ls -l /home
```

**Result:**
- /home/sapana → secure (owner-only access)  
- /home/student → secure permissions  
-  No unauthorised access possible between users

### 4. Sudo Privileges

Command:
```bash
sudo -l
```
**Result**
The user sapana is permitted to run all commands with elevated privileges, which is appropriate for the system administrator.
Sudo operates with secure defaults such as env_reset, helping reduce risk.

### 5. Summary

- Correct users and groups are present
- Sudo access for sapana is appropriate
- Home directories are securely permissioned
- Key Risk: The student account has sudo access — remove if not required
- No unauthorised or unexpected user privileges detected beyond this

### Evidence

<img width="1287" height="805" alt="week7-access-control" src="https://github.com/user-attachments/assets/15938993-6b9d-43bc-a6c8-b3f59c7ce75f" />


---
---

## Task 5: 
