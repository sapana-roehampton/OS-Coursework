# Week 4 – Initial System Configuration & Security Implementation

---

## 1. Introduction

Week 4 focused on strengthening the security of my Ubuntu Server by implementing essential system configuration tasks.  
All actions were completed **remotely via SSH** from my Windows workstation terminal, following the coursework requirement that the server must only be administered through command-line and never via the VirtualBox console.

The main goals for this week were:

- Creating and managing a secure non-root administrative user  
- Enabling and configuring the UFW firewall  
- Hardening SSH access by disabling root login  
- Verifying secure remote access after configuration changes  

These steps form the foundation of server security and are widely used in real-world system administration.

---

## 2. User Privilege Management

A non-root administrative user named **student** was created.  
This reduces security risks because root access is highly privileged and should never be used for normal administration.

### Commands Used:
```bash
sudo adduser student
sudo usermod -aG sudo student
```

After adding the user, its sudo privileges were confirmed.

### 📸 Evidence – Non-root User Creation
![Non-root user creation](non-root-user.png)

---

## 3. Firewall Configuration (UFW)

The Uncomplicated Firewall (UFW) was enabled to protect the server by controlling inbound traffic.  
SSH access was explicitly allowed so that remote administration remained possible.

### Commands Used:
```bash
sudo ufw allow 22/tcp
sudo ufw enable
sudo ufw status
```

### Firewall Status Confirmed:
- Firewall is **active**
- Incoming connections: **Denied by default**
- Outgoing connections: **Allowed**
- SSH (port 22) is **explicitly allowed**

### 📸 Evidence – UFW Enabled
![UFW Enabled](ufw-enable.png)

### 📸 Evidence – UFW Rules Status
![UFW Status](ufw-status.png)

---

## 4. SSH Hardening

To enhance security, I edited the SSH configuration file to **disable direct root login**.  
This prevents attackers from using the root account to attempt a brute-force attack.

### File Edited:
`/etc/ssh/sshd_config`

### Configuration Change Made:
```bash
PermitRootLogin no
```

The SSH service was restarted to apply the security configuration.

```bash
sudo systemctl restart ssh
```

### 📸 Evidence – SSH Root Login Disabled
![SSH Root Login Disabled](sshdconfig-after.png)

---

## 5. Remote Administration Verification

After implementing all security settings, I tested secure SSH access from Windows PowerShell using the **student** user.

### Command Used:
```powershell
ssh -p 2222 student@localhost
```

The login succeeded, confirming that:

- SSH is functioning correctly  
- UFW rules are correct  
- Root login is blocked  
- Non-root administrative access works as intended  

### 📸 Evidence – Successful SSH Login After Hardening
![SSH Login Success](ssh-login-after-hardening.png)

---

## 6. Reflection

Week 4 gave me practical experience applying essential Linux server security techniques.  
I learned how to:

- Create and manage user accounts with correct privilege levels  
- Use UFW to protect the server from unauthorized access  
- Modify SSH configuration files safely  
- Apply service restarts and verify results  
- Test secure remote connectivity using SSH  

These tasks reflect industry-standard system administration practices and prepared me for more advanced security automation in Week 5.

---

