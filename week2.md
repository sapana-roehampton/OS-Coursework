# Week 2 - Security Planning & Testing Methodology

## Performance Testing Plan

This will be the testing of how my Ubuntu performs under different workloads for week six. In week 2 I am only planning how I will measure the performance, not running the tests yet. I will 
connect to my server remotely using SSH from my workstation( Windows laptop). This matches how system administrators normally work with servers.

### 1.1 Tools I plan to use

From the workstation (using SSH):

-  top or htop  – to watch CPU and memory usage in real time  
-  free -h  – to check RAM usage before and after tests  
-  df -h  – to confirm available disk space  
-  vmstat  – to monitor CPU, memory and I/O statistics together  
-  iostat  – to measure disk I/O performance during heavy workloads  
-  ping  – to measure basic network latency  
-  curl or wget  – to test response times from services running on the server  

Later, if needed, I may also use more advanced tools such as `iperf3` for network throughput testing or `stress-ng` for generating controlled load.

### 1.2 Metrics I will monitor

For each application that I test in Week 6, I plan to record:

- **CPU usage** – overall CPU % and which processes use the most  
- **Memory usage** – total used, free and cached memory  
- **Disk I/O** – read/write throughput and I/O wait time  
- **Network performance** – latency (ping) and throughput (amount of data sent/received)  
- **System load** – using `uptime` or `top` to see the load average  
- **Service behaviour** – for server applications, I will note response times and if any errors appear

### 1.3 Testing approach

For each workload I will:

1. Record a **baseline** (no extra applications running).  
2. Start the application or service from the workstation using SSH.  
3. Use the monitoring tools above while the workload is running.  
4. Save the results in a small table (CPU, memory, disk, network).  
5. Stop the workload and check if the system returns to normal.

## 2. Security Configuration Checklist

This checklist describes the security settings I plan to implement on my Ubuntu Server in Weeks 4 and 5. I will use this as a checklist later to verify that everything is configured correctly.

### 2.1 SSH Hardening

- Disable direct root login over SSH  
- Disable password-based SSH authentication and use **key-based authentication** only  
- Change SSH configuration file `/etc/ssh/sshd_config` safely and restart the SSH service  
- Limit SSH access to my workstation IP only (combined with firewall rules)  

### 2.2 Firewall Configuration

- Enable `ufw` (Uncomplicated Firewall) on the server  
- Allow **SSH only** from my workstation IP address  
- Deny all incoming connections by default, except necessary services used in testing  
- Keep all outgoing connections allowed (for updates and package installs)  

### 2.3 Mandatory Access Control (MAC)

- Use **AppArmor** (Ubuntu’s default MAC system)  
- Check which AppArmor profiles are loaded and enforced  
- Ensure that key services (e.g. SSH, any future web service) have appropriate AppArmor profiles  
- Learn how to view AppArmor logs to detect blocked actions  

### 2.4 Automatic Updates

- Install and configure `unattended-upgrades`  
- Enable automatic security updates  
- Verify that configuration files are correct and that upgrade logs are being written  

### 2.5 User and Privilege Management

- Create a non-root administrative user (already done in Week 1 install)  
- Ensure this user is in the `sudo` group with least-privilege usage  
- Disable or lock direct `root` login where appropriate  
- Use strong, unique passwords on all accounts  

### 2.6 Network Security

- Keep the server inside the VirtualBox NAT network (no direct exposure to the internet)  
- Only run services that are necessary for coursework (no extra daemons)  
- Regularly check open ports using `ss -tuln` or `netstat`  
- Later, configure `fail2ban` to protect against SSH brute-force attacks

This checklist will be used in Weeks 4 and 5 when I implement and then verify each security control.


## 3. Threat Model

In this coursework my server runs inside a local VirtualBox NAT network, so it is not exposed directly to the internet. However, there are still realistic threats that I need to consider.

### Threat 1 – Brute-force SSH attacks

**Description:**  
If SSH is misconfigured (for example, password login allowed from anywhere), an attacker could repeatedly try many passwords until one works.

**Impact:**  
The attacker could gain remote access to the server, read or modify files, install malware, or disrupt the system.

**Mitigations:**

- Disable password-based SSH login and use **key-based authentication only**  
- Restrict SSH access to just my workstation IP using firewall rules  
- Install and configure **fail2ban** to ban IP addresses that show repeated failed SSH logins  
- Regularly review SSH logs (`/var/log/auth.log`) for suspicious activity  

---

### Threat 2 – Misconfigured or overly open firewall

**Description:**  
If I accidentally allow too many ports or services in the firewall, unwanted network access may be possible from other machines on the host-only or NAT network.

**Impact:**  
Extra open ports can expose services that are not secured or not needed, increasing the attack surface.

**Mitigations:**

- Use `ufw` with a **default deny** incoming policy  
- Explicitly allow only:
  - SSH from my workstation  
  - Any other port that I intentionally use for testing  
- Regularly list firewall rules and open ports and remove anything not needed  
- Keep a record of which services are supposed to be running  

---

### Threat 3 – Privilege escalation by a local user

**Description:**  
If a normal user account on the server is compromised, an attacker might try to gain `sudo` privileges or exploit misconfigurations to become root.

**Impact:**  
Full control over the system, ability to disable security settings, install rootkits, or damage files.

**Mitigations:**

- Use a **single administrative user** with `sudo` access and strong password  
- Avoid giving unnecessary `sudo` permissions to other accounts  
- Use `sudo` for individual commands instead of logging in as root  
- Monitor `/var/log/auth.log` for suspicious `sudo` attempts  

---

### Threat 4 – Out-of-date packages and vulnerabilities

**Description:**  
If the operating system or installed software is not updated, known security vulnerabilities may be left unpatched.

**Impact:**  
Attackers could exploit old vulnerabilities to gain access or crash services.

**Mitigations:**

- Configure `unattended-upgrades` for automatic security updates  
- Run `sudo apt update && sudo apt upgrade` regularly  
- Reboot the server when required after kernel or major package upgrades  
- Periodically review `apt` logs to confirm that updates are being applied

---

This threat model helps me understand which risks are most relevant to my small virtual server and shows how my planned security configuration directly reduces those risks.
   
